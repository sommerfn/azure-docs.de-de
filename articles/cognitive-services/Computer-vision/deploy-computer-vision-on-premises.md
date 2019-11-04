---
title: Verwenden eines Containers für maschinelles Sehen mit Kubernetes und Helm
titleSuffix: Azure Cognitive Services
description: Stellen Sie den Container für maschinelles Sehen in einer Azure Container Instance bereit, und testen Sie diesen in einem Webbrowser.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: dapine
ms.openlocfilehash: 8d285bf60e356f15caf55271b0791e9adc97ac14
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73481753"
---
# <a name="use-computer-vision-container-with-kubernetes-and-helm"></a>Verwenden eines Containers für maschinelles Sehen mit Kubernetes und Helm

Eine Möglichkeit zur lokalen Verwaltung Ihrer Container für maschinelles Sehen ist die Verwendung von Kubernetes und Helm. Hier erfahren Sie, wie Sie ein Kubernetes-Paket erstellen und dabei Kubernetes und Helm verwenden, um das Containerimage für maschinelles Sehen zu definieren. Dieses Paket wird dann lokal für einen Kubernetes-Cluster bereitgestellt. Außerdem erfahren Sie, wie Sie die bereitgestellten Dienste testen. Weitere Informationen zum Ausführen von Docker-Containern ohne Kubernetes-Orchestrierung finden Sie unter [Installieren und Ausführen von Containern für maschinelles Sehen](computer-vision-how-to-install-containers.md).

## <a name="prerequisites"></a>Voraussetzungen

Die folgenden Voraussetzungen müssen erfüllt sein, damit Container für maschinelles Sehen lokal verwendet werden können:

|Erforderlich|Zweck|
|--|--|
| Azure-Konto | Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto][free-azure-account] erstellen, bevor Sie beginnen. |
| Kubernetes-Befehlszeilenschnittstelle | Mithilfe der [Kubernetes-Befehlszeilenschnittstelle][kubernetes-cli] werden die gemeinsam genutzten Anmeldeinformationen aus der Containerregistrierung verwaltet. Kubernetes wird außerdem vor Helm (Kubernetes-Paket-Manager) benötigt. |
| Helm-Befehlszeilenschnittstelle | Im Rahmen der Installation der [Helm-CLI][helm-install] müssen Sie auch Helm initialisieren, wodurch [Tiller][tiller-install] installiert wird. |
| Ressource für maschinelles Sehen |Um den Container zu verwenden, benötigen Sie Folgendes:<br><br>Eine Azure-Ressource vom Typ **Maschinelles Sehen** sowie den zugehörigen API-Schlüssel und Endpunkt-URI. Beide Werte stehen auf der Übersichts- und auf der Schlüsselseite der Ressource zur Verfügung und werden zum Starten des Containers benötigt.<br><br>**{API_KEY}** : Einer der beiden verfügbaren Ressourcenschlüssel auf der Seite **Schlüssel**<br><br>**{ENDPOINT_URI}** : Der Endpunkt, der auf der **Übersichtsseite** angegeben ist|

[!INCLUDE [Gathering required parameters](../containers/includes/container-gathering-required-parameters.md)]

### <a name="the-host-computer"></a>Der Hostcomputer

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]

### <a name="container-requirements-and-recommendations"></a>Containeranforderungen und -empfehlungen

[!INCLUDE [Container requirements and recommendations](includes/container-requirements-and-recommendations.md)]

## <a name="connect-to-the-kubernetes-cluster"></a>Herstellen einer Verbindung mit dem Kubernetes-Cluster

Für den Hostcomputer muss ein verfügbarer Kubernetes-Cluster vorhanden sein. Das Konzept der Bereitstellung eines Kubernetes-Clusters für einen Hostcomputer wird im Tutorial [Bereitstellen eines Azure Kubernetes Service-Clusters (AKS)](../../aks/tutorial-kubernetes-deploy-cluster.md) erläutert.

### <a name="sharing-docker-credentials-with-the-kubernetes-cluster"></a>Weitergeben von Docker-Anmeldeinformationen an den Kubernetes-Cluster

Damit der Kubernetes-Cluster die konfigurierten Images mittels `docker pull` aus der Containerregistrierung `containerpreview.azurecr.io` pullen kann, müssen die Docker-Anmeldeinformationen in den Cluster übertragen werden. Führen Sie den Befehl [`kubectl create`][kubectl-create] wie weiter unten gezeigt aus, um ein *Docker-Registrierungsgeheimnis* auf der Grundlage der Anmeldeinformationen zu erstellen, die im Rahmen der Vorbereitung für den Zugriff auf die Containerregistrierung angegeben wurden.

Führen Sie über die Befehlszeilenschnittstelle Ihrer Wahl den folgenden Befehl aus. Ersetzen Sie dabei `<username>`, `<password>` und `<email-address>` durch die Anmeldeinformationen der Containerregistrierung.

```console
kubectl create secret docker-registry containerpreview \
    --docker-server=containerpreview.azurecr.io \
    --docker-username=<username> \
    --docker-password=<password> \
    --docker-email=<email-address>
```

> [!NOTE]
> Falls Sie bereits über Zugriff auf die Containerregistrierung `containerpreview.azurecr.io` verfügen, können Sie stattdessen auch ein Kubernetes-Geheimnis mit dem generischen Flag erstellen. Der folgende Befehl wird für den JSON-Code Ihrer Docker-Konfiguration ausgeführt:
> ```console
>  kubectl create secret generic containerpreview \
>      --from-file=.dockerconfigjson=~/.docker/config.json \
>      --type=kubernetes.io/dockerconfigjson
> ```

Nach erfolgreicher Erstellung des Geheimnisses wird in der Konsole Folgendes ausgegeben:

```console
secret "containerpreview" created
```

Vergewissern Sie sich, dass das Geheimnis erstellt wurde, indem Sie den Befehl [`kubectl get`][kubectl-get] mit dem Flag `secrets` ausführen:

```console
kuberctl get secrets
```

Der Befehl `kubectl get secrets` gibt alle konfigurierten Geheimnisse zurück:

```console
NAME                  TYPE                                  DATA      AGE
containerpreview      kubernetes.io/dockerconfigjson        1         30s
```

## <a name="configure-helm-chart-values-for-deployment"></a>Konfigurieren von Helm-Chart-Werten für die Bereitstellung

Erstellen Sie zunächst einen Ordner namens *read*, kopieren Sie dann den folgenden YAML-Inhalt und fügen Sie ihn in eine neue Datei namens *Chart.yml* ein.

```yaml
apiVersion: v1
name: read
version: 1.0.0
description: A Helm chart to deploy the microsoft/cognitive-services-read to a Kubernetes cluster
```

Kopieren Sie zum Konfigurieren der Standardwerte des Helm-Charts den folgenden YAML-Code, und fügen Sie ihn in eine Datei namens `values.yaml` ein. Ersetzen Sie die Kommentare `# {ENDPOINT_URI}` und `# {API_KEY}` durch Ihre eigenen Werte.

```yaml
# These settings are deployment specific and users can provide customizations

read:
  enabled: true
  image:
    name: cognitive-services-read
    registry: containerpreview.azurecr.io/
    repository: microsoft/cognitive-services-read
    tag: latest
    pullSecret: containerpreview # Or an existing secret
    args:
      eula: accept
      billing: # {ENDPOINT_URI}
      apikey: # {API_KEY}
```

> [!IMPORTANT]
> Ohne Angabe der Werte `billing` und `apikey` laufen die Dienste nach 15 Minuten ab. Dies führt dann dazu, dass die Überprüfung nicht erfolgreich ist, da die Dienste nicht zur Verfügung stehen.

Erstellen Sie den Ordner *templates* unter dem Verzeichnis *read*. Kopieren Sie den folgenden YAML-Code, und fügen Sie ihn in eine Datei mit dem Namen `deployment.yaml` ein. Die `deployment.yaml`-Datei dient als Helm-Vorlage.

> Vorlagen generieren Manifestdateien, die Beschreibungen von Ressourcen im YAML-Format darstellen, die von Kubernetes verstanden werden können. [- Leitfaden zur Vorlage für Helm-Charts][chart-template-guide]

```yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: read
spec:
  template:
    metadata:
      labels:
        app: read-app
    spec:
      containers:
      - name: {{.Values.read.image.name}}
        image: {{.Values.read.image.registry}}{{.Values.read.image.repository}}
        ports:
        - containerPort: 5000
        env:
        - name: EULA
          value: {{.Values.read.image.args.eula}}
        - name: billing
          value: {{.Values.read.image.args.billing}}
        - name: apikey
          value: {{.Values.read.image.args.apikey}}
      imagePullSecrets:
      - name: {{.Values.read.image.pullSecret}}

--- 
apiVersion: v1
kind: Service
metadata:
  name: read
spec:
  type: LoadBalancer
  ports:
  - port: 5000
  selector:
    app: read-app
```

Die Vorlage gibt einen Lastenausgleichsdienst und die Bereitstellung Ihres Containers/Bilds für das Lesen an.

### <a name="the-kubernetes-package-helm-chart"></a>Das Kubernetes-Paket (Helm-Chart)

Das *Helm-Chart* enthält die Konfiguration, die angibt, welche Docker-Images aus der Containerregistrierung `containerpreview.azurecr.io` gepullt werden sollen.

> Bei einem [Helm-Chart][helm-charts] handelt es sich um eine Sammlung von Dateien, die eine zusammengehörige Gruppe von Kubernetes-Ressourcen beschreiben. Ein einzelnes Chart kann sowohl für eine einfache Bereitstellung (beispielsweise eines Memcached-Pods) als auch für komplexere Bereitstellungen (etwa eines vollständigen Web-App-Stapels mit HTTP-Servern, Datenbanken, Caches und Ähnlichem) verwendet werden.

Die bereitgestellten *Helm-Charts* pullen die Docker-Images des Diensts für maschinelles Sehen und den entsprechenden Dienst aus der Containerregistrierung `containerpreview.azurecr.io`.

## <a name="install-the-helm-chart-on-the-kubernetes-cluster"></a>Installieren des Helm-Charts für den Kubernetes-Cluster

Zum Installieren des *Helm-Charts* muss der Befehl [`helm install`][helm-install-cmd] ausgeführt werden. Stellen Sie sicher, dass Sie den Installationsbefehl im Verzeichnis über dem Ordner `read` ausführen.

```console
helm install read --name read
```

Die Ausgabe nach einer erfolgreichen Installation kann beispielsweise wie folgt aussehen:

```console
NAME: read
LAST DEPLOYED: Thu Sep 04 13:24:06 2019
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1/Pod(related)
NAME                    READY  STATUS             RESTARTS  AGE
read-57cb76bcf7-45sdh   0/1    ContainerCreating  0         0s

==> v1/Service
NAME     TYPE          CLUSTER-IP    EXTERNAL-IP  PORT(S)         AGE
read     LoadBalancer  10.110.44.86  localhost    5000:31301/TCP  0s

==> v1beta1/Deployment
NAME    READY  UP-TO-DATE  AVAILABLE  AGE
read    0/1    1           0          0s
```

Der Kubernetes-Bereitstellungsvorgang kann mehrere Minuten dauern. Vergewissern Sie sich durch Ausführen des folgenden Befehls, dass die Pods und Dienste ordnungsgemäß bereitgestellt wurden und verfügbar sind:

```console
kubectl get all
```

Die Ausgabe sollte in etwa wie folgt aussehen:

```console
kubectl get all
NAME                        READY   STATUS    RESTARTS   AGE
pod/read-57cb76bcf7-45sdh   1/1     Running   0          17s

NAME                   TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
service/kubernetes     ClusterIP      10.96.0.1      <none>        443/TCP          45h
service/read           LoadBalancer   10.110.44.86   localhost     5000:31301/TCP   17s

NAME                   READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/read   1/1     1            1           17s

NAME                              DESIRED   CURRENT   READY   AGE
replicaset.apps/read-57cb76bcf7   1         1         1       17s
```
<!--  ## Validate container is running -->

[!INCLUDE [Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="next-steps"></a>Nächste Schritte

Ausführlichere Informationen zum Installieren von Anwendungen mit Helm in Azure Kubernetes Service (AKS) finden Sie [hier][installing-helm-apps-in-aks].

> [!div class="nextstepaction"]
> [Containerunterstützung in Azure Cognitive Services][cog-svcs-containers]

<!-- LINKS - external -->
[free-azure-account]: https://azure.microsoft.com/free
[git-download]: https://git-scm.com/downloads
[azure-cli]: https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest
[docker-engine]: https://www.docker.com/products/docker-engine
[kubernetes-cli]: https://kubernetes.io/docs/tasks/tools/install-kubectl
[helm-install]: https://helm.sh/docs/using_helm/#installing-helm
[helm-install-cmd]: https://helm.sh/docs/helm/#helm-install
[tiller-install]: https://helm.sh/docs/install/#installing-tiller
[helm-charts]: https://helm.sh/docs/developing_charts
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[helm-test]: https://helm.sh/docs/helm/#helm-test
[chart-template-guide]: https://helm.sh/docs/chart_template_guide

<!-- LINKS - internal -->
[vision-container-host-computer]: computer-vision-how-to-install-containers.md#the-host-computer
[installing-helm-apps-in-aks]: ../../aks/kubernetes-helm.md
[cog-svcs-containers]: ../cognitive-services-container-support.md
