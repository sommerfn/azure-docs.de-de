---
title: Entwickeln unter Azure Kubernetes Service (AKS) mit Draft
description: Verwenden von Draft mit AKS und Azure Container Registry
services: container-service
author: zr-msft
ms.service: container-service
ms.topic: article
ms.date: 06/20/2019
ms.author: zarhoads
ms.openlocfilehash: bd099b9d76e17eda36be1650ef5081e5aaa7e53a
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2019
ms.locfileid: "67303544"
---
# <a name="quickstart-develop-on-azure-kubernetes-service-aks-with-draft"></a>Schnellstart: Entwickeln unter Azure Kubernetes Service (AKS) mit Draft

Draft ist ein praktisches Open-Source-Tool zum Packen und Ausführen von Anwendungscontainern in einem Kubernetes-Cluster. Mit Draft können Sie für Kubernetes schnell eine Anwendung erneut bereitstellen, wenn sich Änderungen am Code ergeben, ohne Ihre Änderungen an die Versionskontrolle zu committen. Weitere Informationen zu Draft finden Sie in der [Draft-Dokumentation auf GitHub][draft-documentation].

In diesem Artikel erfahren Sie, wie Sie das Draft-Paket verwenden und eine Anwendung unter AKS ausführen können.


## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Falls Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.
* [Azure-CLI installiert](/cli/azure/install-azure-cli?view=azure-cli-latest).
* Docker muss installiert und konfiguriert sein. Für Docker sind Pakete erhältlich, mit denen Docker auf einem [Mac-][docker-for-mac], [Windows][docker-for-windows], oder [Linux]-System ([docker-for-linux]) konfiguriert werden kann.
* [Helm muss installiert sein.](https://github.com/helm/helm/blob/master/docs/install.md)
* [Draft muss installiert sein][draft-documentation]

## <a name="create-an-azure-kubernetes-service-cluster"></a>Erstellen eines Azure Kubernetes Service-Clusters

Erstellen Sie einen AKS-Cluster. Über die unten angegebenen Befehle werden eine Ressourcengruppe mit dem Namen „MyResourceGroup“ sowie der AKS-Cluster „MyAKS“ erstellt.

```azurecli
az group create --name MyResourceGroup --location eastus
az aks create -g MyResourceGroup -n MyAKS --location eastus --node-vm-size Standard_DS2_v2 --node-count 1 --generate-ssh-keys
```

## <a name="create-an-azure-container-registry"></a>Erstellen einer Azure-Containerregistrierung
Damit Sie Draft zum Ausführen Ihrer Anwendung in Ihrem AKS-Cluster verwenden können, benötigen Sie eine Azure Container Registry-Instanz (ACR), um darin Ihre Containerimages zu speichern. Im nachfolgenden Beispiel wird [az acr create][az-acr-create] verwendet, um mithilfe der SKU *Basic* eine ACR-Instanz mit dem Namen *MyDraftACR* in der Ressourcengruppe *MyResourceGroup* zu erstellen. Geben Sie Ihren eigenen eindeutigen Registrierungsnamen an. Der Registrierungsname muss innerhalb von Azure eindeutig sein und zwischen 5 und 50 alphanumerische Zeichen enthalten. Die *Basic*-SKU ist ein kostenoptimierter Einstiegspunkt für Entwicklungszwecke, der ein ausgewogenes Verhältnis von Speicher und Durchsatz bietet.

```azurecli
az acr create --resource-group MyResourceGroup --name MyDraftACR --sku Basic
```

Die Ausgabe sieht in etwa wie das folgende Beispiel aus: Notieren Sie sich den *loginServer*-Wert für Ihre ACR-Instanz, da Sie diesen in einem späteren Schritt benötigen werden. Im nachfolgenden Beispiel ist *mydraftacr.azurecr.io* der *loginServer*-Wert für *MyDraftACR*.

```console
{
  "adminUserEnabled": false,
  "creationDate": "2019-06-11T13:35:17.998425+00:00",
  "id": "/subscriptions/<ID>/resourceGroups/MyResourceGroup/providers/Microsoft.ContainerRegistry/registries/MyDraftACR",
  "location": "eastus",
  "loginServer": "mydraftacr.azurecr.io",
  "name": "MyDraftACR",
  "networkRuleSet": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "MyResourceGroup",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "status": null,
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```


Zur Verwendung der ACR-Instanz mit Draft müssen Sie sich zunächst anmelden. Verwenden Sie hierzu den Befehl [az acr login][az-acr-login]. Im folgenden Beispiel erfolgt eine Anmeldung bei einer ACR-Instanz mit dem Namen *MyDraftACR*.

```azurecli
az acr login --name MyDraftACR
```

Nach Abschluss des Vorgangs wird eine *Erfolgsmeldung* zurückgegeben.

## <a name="create-trust-between-aks-cluster-and-acr"></a>Erstellen der Vertrauensstellung zwischen dem AKS-Cluster und ACR

Außerdem benötigt Ihr AKS-Cluster Zugriff auf Ihre ACR-Instanz, um dieser Ihre Containerimages über einen Pullvorgang entnehmen und diese ausführen zu können. Sie können über AKS den Zugriff auf die ACR-Instanz einrichten, indem Sie eine Vertrauensstellung einrichten. Wenn Sie eine Vertrauensstellung zwischen einem AKS-Cluster und einer ACR-Registrierung erstellen möchten, müssen Sie dem Azure Active Directory-Dienstprinzipal, der vom AKS-Cluster für den Zugriff auf die ACR-Registrierung verwendet wird, Berechtigungen zuweisen. Über die folgenden Befehle erhalten Sie Berechtigungen auf den Dienstprinzipal des *MyAKS*-Clusters in der Ressourcengruppe *MyResourceGroup* für die ACR-Instanz *MyDraftACR* in der Ressourcengruppe *MyResourceGroup*.

```azurecli
# Get the service principal ID of your AKS cluster
AKS_SP_ID=$(az aks show --resource-group MyResourceGroup --name MyAKS --query "servicePrincipalProfile.clientId" -o tsv)

# Get the resource ID of your ACR instance
ACR_RESOURCE_ID=$(az acr show --resource-group MyResourceGroup --name MyDraftACR --query "id" -o tsv)

# Create a role assignment for your AKS cluster to access the ACR instance
az role assignment create --assignee $AKS_SP_ID --scope $ACR_RESOURCE_ID --role contributor
```

## <a name="connect-to-your-aks-cluster"></a>Herstellen einer Verbindung mit dem AKS-Cluster

Zum Herstellen der Verbindung mit dem Kubernetes-Cluster auf Ihrem lokalen Computer können Sie den Kubernetes-Befehlszeilenclient [kubectl][kubectl] verwenden.

Wenn Sie Azure Cloud Shell verwenden, ist `kubectl` bereits installiert. Mit dem Befehl [az aks install-cli][] können Sie ihn auch lokal installieren:

```azurecli
az aks install-cli
```

Mit dem Befehl [az aks get-credentials][] können Sie `kubectl` für die Verbindungsherstellung mit Ihrem Kubernetes-Cluster konfigurieren. Im folgenden Beispiel werden Anmeldeinformationen für den AKS-Cluster namens *MyAKS* in der Ressourcengruppe *myResourceGroup* abgerufen:

```azurecli
az aks get-credentials --resource-group MyResourceGroup --name MyAKS
```

## <a name="create-a-service-account-for-helm"></a>Erstellen eines Dienstkontos für Helm

Vor der Bereitstellung von Helm in einem RBAC-fähigen AKS-Cluster benötigen Sie ein Dienstkonto und eine Rollenbindung für den Tiller-Dienst. Weitere Informationen zum Sichern von Helm/Tiller in einem RBAC-fähigen Cluster finden Sie unter [Tiller, Namespaces, and RBAC (Tiller, Namespaces und RBAC)][tiller-rbac]. Überspringen Sie diesen Schritt, wenn Ihr AKS-Cluster nicht RBAC-fähig ist.

Erstellen Sie eine Datei namens `helm-rbac.yaml`, und fügen Sie den folgenden YAML-Code ein:

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: tiller
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: tiller
    namespace: kube-system
```

Erstellen Sie das Dienstkonto und die Rollenbindung mithilfe des Befehls `kubectl apply`:

```console
kubectl apply -f helm-rbac.yaml
```

## <a name="configure-helm"></a>Konfigurieren von Helm
Verwenden Sie den Befehl [helm init][helm-init], um Tiller in einem AKS-Cluster bereitzustellen. Wenn Ihr Cluster nicht RBAC-fähig ist, entfernen Sie das Argument `--service-account` und den Wert.

```console
helm init --service-account tiller --node-selectors "beta.kubernetes.io/os"="linux"
```

## <a name="configure-draft"></a>Konfigurieren von Draft

Wenn Sie Draft noch nicht auf Ihrem lokalen Computer konfiguriert haben, führen Sie `draft init` aus:

```console
$ draft init
Installing default plugins...
Installation of default plugins complete
Installing default pack repositories...
...
Happy Sailing!
```

Außerdem müssen Sie Draft konfigurieren, um den *loginServer*-Wert Ihrer ACR-Instanz verwenden zu können. Der folgende Befehl verwendet `draft config set`, um `mydraftacr.azurecr.io` als Registrierung zu verwenden.

```console
draft config set registry mydraftacr.azurecr.io
```

Damit sollten Sie Draft für die Verwendung Ihrer ACR-Instanz konfiguriert haben, und Draft sollte Containerimages über einen Pushvorgang auf diese Instanz übertragen können. Da Draft Ihre Anwendung in Ihrem AKS-Cluster ausführt, sind keine Kennwörter oder Geheimnisse für Pull- oder Pushvorgänge im Zusammenhang mit der ACR-Registrierung erforderlich. Da Sie eine Vertrauensstellung zwischen dem AKS-Cluster und Ihrer ACR-Instanz eingerichtet haben, erfolgt die Authentifizierung unter Verwendung von Azure Active Directory auf der Ebene des Azure Resource Managers.

## <a name="download-the-sample-application"></a>Herunterladen der Beispielanwendung

In diesem Schnellstart wird [eine Java-Beispielanwendung aus dem GitHub-Repository für Draft][example-java] verwendet. Klonen Sie die Anwendung von GitHub, und navigieren Sie zum Verzeichnis `draft/examples/example-java/`.

```console
git clone https://github.com/Azure/draft
cd draft/examples/example-java/
```

## <a name="run-the-sample-application-with-draft"></a>Ausführen der Beispielanwendung mit Draft

Führen Sie den Befehl `draft create` aus, um die Anwendung vorzubereiten.

```console
draft create
```

Dieser Befehl erstellt die Artefakte, die verwendet werden, um die Anwendung in einem Kubernetes-Cluster auszuführen. Zu diesen Elementen zählen eine Dockerfile-Datei, ein Helm-Diagramm und eine *draft.toml*-Datei, bei der es sich um die Draft-Konfigurationsdatei handelt.

```console
$ draft create

--> Draft detected Java (92.205567%)
--> Ready to sail
```

Führen Sie die Beispielanwendung in Ihrem AKS-Cluster mit dem Befehl `draft up` aus.

```console
draft up
```

Dieser Befehl erstellt das Dockerfile, um ein Containerimage zu erstellen. Das Image wird per Push an Ihre ACR-Instanz übertragen, und das Helm-Diagramm wird installiert, um die Anwendung in AKS zu starten. Push- und Pullvorgänge für das Containerimage können beim ersten Ausführen dieses Befehls einige Zeit in Anspruch nehmen. Nach der Zwischenspeicherung der Basisebenen verkürzt sich die Zeit zum Bereitstellen der Anwendung erheblich.

```
$ draft up

Draft Up Started: 'example-java': 01CMZAR1F4T1TJZ8SWJQ70HCNH
example-java: Building Docker Image: SUCCESS ⚓  (73.0720s)
example-java: Pushing Docker Image: SUCCESS ⚓  (19.5727s)
example-java: Releasing Application: SUCCESS ⚓  (4.6979s)
Inspect the logs with `draft logs 01CMZAR1F4T1TJZ8SWJQ70HCNH`
```

## <a name="connect-to-the-running-sample-application-from-your-local-machine"></a>Herstellen einer Verbindung mit der Beispielanwendung über Ihren lokalen Computer

Testen Sie die Anwendung mithilfe des Befehls `draft connect`.

```console
draft connect
```

Mit diesem Befehl wird eine sichere Proxyverbindung mit dem Kubernetes-Pod hergestellt. Nach Abschluss des Vorgangs kann auf die Anwendung über die angegebene URL zugegriffen werden.

```console
$ draft connect

Connect to java:4567 on localhost:49804
[java]: SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
[java]: SLF4J: Defaulting to no-operation (NOP) logger implementation
[java]: SLF4J: See https://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
[java]: == Spark has ignited ...
[java]: >> Listening on 0.0.0.0:4567
```

Navigieren Sie mithilfe der `localhost`-URL zu der Anwendung in Ihrem Browser, um die Beispielanwendung abzurufen. Im obigen Beispiel lautet die URL `http://localhost:49804`. Trennen Sie mithilfe der Tastenkombination `Ctrl+c` die Verbindung.

## <a name="access-the-application-on-the-internet"></a>Zugreifen auf die Anwendung über das Internet

Im letzten Schritt wurde eine Proxyverbindung mit dem Anwendungs-Pod in Ihrem AKS-Cluster hergestellt. Beim Entwickeln und Testen Ihrer Anwendung können Sie die Anwendung im Internet verfügbar machen. Erstellen Sie hierzu einen Kubernetes-Dienst des Typs [LoadBalancer][kubernetes-service-loadbalancer].

Aktualisieren Sie `charts/example-java/values.yaml`, um einen *LoadBalancer*-Dienst zu erstellen. Ändern Sie den *service.type*-Wert von *ClusterIP* in *LoadBalancer*.

```yaml
...
service:
  name: java
  type: LoadBalancer
  externalPort: 80
  internalPort: 4567
...
```

Speichern Sie Ihre Änderungen, schließen Sie die Datei, und führen Sie `draft up` aus, um die Anwendung nochmals auszuführen.

```console
draft up
```

Es kann einige Minuten dauern, bis der Dienst eine öffentliche IP-Adresse zurückgibt. Verwenden Sie zum Überwachen des Status den Befehl `kubectl get service` mit dem *watch*-Parameter:

```console
$ kubectl get service --watch

NAME                TYPE          CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
example-java-java   LoadBalancer  10.0.141.72   <pending>     80:32150/TCP   2m
...
example-java-java   LoadBalancer   10.0.141.72   52.175.224.118  80:32150/TCP   7m
```

Navigieren Sie in Ihrem Browser mithilfe der *EXTERNAL-IP* zum Lastenausgleich Ihrer Anwendung, um die Beispielanwendung abzurufen. Im obigen Beispiel lautet die IP `52.175.224.118`.

## <a name="iterate-on-the-application"></a>Durchlaufen der Anwendung

Sie können Ihre Anwendung durchlaufen, indem Sie Änderungen lokal vornehmen und `draft up` nohc mal ausführen.

Aktualisieren Sie die Nachricht, die [in Zeile 7 von src/main/java/helloworld/Hello.java][example-java-hello-l7] zurückgegeben wird.

```java
    public static void main(String[] args) {
        get("/", (req, res) -> "Hello World, I'm Java in AKS!");
    }
```

Führen Sie den Befehl `draft up` aus, um die Anwendung erneut bereitzustellen:

```console
$ draft up

Draft Up Started: 'example-java': 01CMZC9RF0TZT7XPWGFCJE15X4
example-java: Building Docker Image: SUCCESS ⚓  (25.0202s)
example-java: Pushing Docker Image: SUCCESS ⚓  (7.1457s)
example-java: Releasing Application: SUCCESS ⚓  (3.5773s)
Inspect the logs with `draft logs 01CMZC9RF0TZT7XPWGFCJE15X4`
```

Wenn Sie die aktualisierte Anwendung aufrufen möchten, navigieren Sie erneut zu der IP-Adresse Ihres Lastenausgleichs, und überprüfen Sie, ob Ihre Änderungen angezeigt werden.

## <a name="delete-the-cluster"></a>Löschen des Clusters

Wenn der Cluster nicht mehr benötigt wird, entfernen Sie über den Befehl [az group delete][az-group-delete] die Ressourcengruppe, den AKS-Cluster, die Containerregistrierung, die darin gespeicherten Containerimages sowie alle weiteren mit dem Cluster in Verbindung stehenden Ressourcen.

```azurecli-interactive
az group delete --name MyResourceGroup --yes --no-wait
```

> [!NOTE]
> Wenn Sie den Cluster löschen, wird der vom AKS-Cluster verwendete Azure Active Directory-Dienstprinzipal nicht entfernt. Die Schritte zum Entfernen des Dienstprinzipals werden im Artikel [Dienstprinzipale mit Azure Kubernetes Service (AKS)][sp-delete] beschrieben.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung von Draft finden Sie in der Draft-Dokumentation auf GitHub.

> [!div class="nextstepaction"]
> [Draft-Dokumentation][draft-documentation]


[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-create]: /cli/azure/acr#az-acr-login
[az-group-delete]: /cli/azure/group#az-group-delete
[az aks get-credentials]: /cli/azure/aks#az-aks-get-credentials
[az aks install-cli]: /cli/azure/aks#az-aks-install-cli
[kubernetes-ingress]: ./ingress-basic.md

[docker-for-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-for-mac]: https://docs.docker.com/docker-for-mac/
[docker-for-windows]: https://docs.docker.com/docker-for-windows/
[draft-documentation]: https://github.com/Azure/draft/tree/master/docs
[example-java]: https://github.com/Azure/draft/tree/master/examples/example-java
[example-java-hello-l7]: https://github.com/Azure/draft/blob/master/examples/example-java/src/main/java/helloworld/Hello.java#L7
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubernetes-service-loadbalancer]: https://kubernetes.io/docs/concepts/services-networking/service/#type-loadbalancer
[helm-init]: https://docs.helm.sh/helm/#helm-init
[sp-delete]: kubernetes-service-principal.md#additional-considerations
[tiller-rbac]: https://docs.helm.sh/using_helm/#tiller-namespaces-and-rbac
