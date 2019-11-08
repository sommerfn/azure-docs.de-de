---
title: Verwenden eines benutzerdefinierten Traefik-Eingangscontrollers und Konfigurieren von HTTPS
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
author: zr-msft
ms.author: zarhoads
ms.date: 08/13/2019
ms.topic: conceptual
description: Informationen zum Konfigurieren von Azure Dev Spaces für die Verwendung eines benutzerdefinierten Traefik-Eingangscontrollers und zum Konfigurieren von HTTPS mithilfe dieses Eingangscontrollers
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container, Helm, Service Mesh, Service Mesh-Routing, kubectl, k8s
ms.openlocfilehash: c015fe8e7108f07d66d2464c4f8b6287e8f54446
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73582320"
---
# <a name="use-a-custom-traefik-ingress-controller-and-configure-https"></a>Verwenden eines benutzerdefinierten Traefik-Eingangscontrollers und Konfigurieren von HTTPS

In diesem Artikel erfahren Sie, wie Sie Azure Dev Spaces für die Verwendung eines benutzerdefinierten Traefik-Eingangscontrollers konfigurieren. Außerdem wird in diesem Artikel beschrieben, wie Sie den benutzerdefinierten Eingangscontroller für die Verwendung von HTTPS konfigurieren.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Falls Sie über keins verfügen, können Sie ein [kostenloses Konto][azure-account-create] erstellen.
* [Azure-CLI installiert][az-cli].
* Ein [Azure Kubernetes Service-Cluster (AKS-Cluster) mit aktiviertem Azure Dev Spaces-Dienst][qs-cli].
* [kubectl][kubectl] muss installiert sein.
* [Helm 2.13 oder höher installiert][helm-installed].
* [Eine benutzerdefinierte Domäne][custom-domain] mit einer [DNS-Zone][dns-zone], die sich in derselben Ressourcengruppe befindet wie Ihr AKS-Cluster.

## <a name="configure-a-custom-traefik-ingress-controller"></a>Konfigurieren eines benutzerdefinierten Traefik-Eingangscontrollers

Stellen Sie unter Verwendung des Kubernetes-Befehlszeilenclients ([kubectl][kubectl]) eine Verbindung mit Ihrem Cluster her. Mit dem Befehl [az aks get-credentials][az-aks-get-credentials] können Sie `kubectl` für die Verbindungsherstellung mit Ihrem Kubernetes-Cluster konfigurieren. Mit diesem Befehl werden die Anmeldeinformationen heruntergeladen, und die Kubernetes-Befehlszeilenschnittstelle wird für deren Verwendung konfiguriert.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKS
```

Verwenden Sie zum Überprüfen der Verbindung mit Ihrem Cluster den Befehl [kubectl get][kubectl-get], um eine Liste der Clusterknoten zu erhalten.

```console
$ kubectl get nodes
NAME                                STATUS   ROLES   AGE    VERSION
aks-nodepool1-12345678-vmssfedcba   Ready    agent   13m    v1.14.1
```

Erstellen Sie einen Kubernetes-Namespace für den Traefik-Eingangscontroller, und installieren Sie ihn mithilfe von `helm`.

```console
kubectl create ns traefik
helm init --wait
helm install stable/traefik --name traefik --namespace traefik --set kubernetes.ingressClass=traefik --set kubernetes.ingressEndpoint.useDefaultPublishedService=true
```

Rufen Sie die IP-Adresse des Traefik-Eingangscontrollerdiensts mithilfe von [kubectl get][kubectl-get] ab.

```console
kubectl get svc -n traefik --watch
```

Die Beispielausgabe enthält die IP-Adressen für alle Dienste im *traefik*-Namespace.

```console
NAME      TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)                      AGE
traefik   LoadBalancer   10.0.205.78   <pending>     80:32484/TCP,443:30620/TCP   20s
...
traefik   LoadBalancer   10.0.205.78   MY_EXTERNAL_IP   80:32484/TCP,443:30620/TCP   60s
```

Fügen Sie Ihrer DNS-Zone mithilfe von [az network dns record-set a add-record][az-network-dns-record-set-a-add-record] einen *A*-Eintrag mit der externen IP-Adresse des Traefik-Diensts hinzu.

```console
az network dns record-set a add-record \
    --resource-group myResourceGroup \
    --zone-name MY_CUSTOM_DOMAIN \
    --record-set-name *.traefik \
    --ipv4-address MY_EXTERNAL_IP
```

Im obigen Beispiel wird der DNS-Zone *MY_CUSTOM_DOMAIN* ein *A*-Eintrag hinzugefügt.

In diesem Artikel verwenden Sie die [Azure Dev Spaces-Beispielanwendung „Bike Sharing“](https://github.com/Azure/dev-spaces/tree/master/samples/BikeSharingApp), um die Nutzung von Azure Dev Spaces zu veranschaulichen. Klonen Sie die Anwendung von GitHub, und navigieren Sie zum Verzeichnis der Anwendung:

```cmd
git clone https://github.com/Azure/dev-spaces
cd dev-spaces/samples/BikeSharingApp/charts
```

Öffnen Sie [values.yaml][values-yaml], und ersetzen Sie alle Vorkommen von *<REPLACE_ME_WITH_HOST_SUFFIX>* durch *traefik.MY_CUSTOM_DOMAIN*, wobei Sie für *MY_CUSTOM_DOMAIN* Ihre Domäne verwenden. Ersetzen Sie zudem *kubernetes.io/ingress.class: traefik-azds  # Dev Spaces-specific* durch *kubernetes.io/ingress.class: traefik  # Custom Ingress*. Nachstehend finden Sie ein Beispiel für eine aktualisierte `values.yaml`-Datei:

```yaml
# This is a YAML-formatted file.
# Declare variables to be passed into your templates.

bikesharingweb:
  ingress:
    annotations:
      kubernetes.io/ingress.class: traefik  # Custom Ingress
    hosts:
      - dev.bikesharingweb.traefik.MY_CUSTOM_DOMAIN  # Assumes deployment to the 'dev' space

gateway:
  ingress:
    annotations:
      kubernetes.io/ingress.class: traefik  # Custom Ingress
    hosts:
      - dev.gateway.traefik.MY_CUSTOM_DOMAIN  # Assumes deployment to the 'dev' space
```

Speichern Sie Ihre Änderungen, und schließen Sie die Datei.

Stellen Sie die Beispielanwendung mithilfe von `helm install` bereit.

```console
helm install -n bikesharing . --dep-up --namespace dev --atomic
```

Im obigen Beispiel wird die Beispielanwendung für den *dev*-Namespace bereitgestellt.

Wählen Sie den Bereich *dev* mit Ihrer Beispielanwendung mithilfe von `azds space select` aus, und zeigen Sie die URLs für den Zugriff auf die Beispielanwendung mithilfe von `azds list-uris` an.

```console
azds space select -n dev
azds list-uris
```

Die folgende Ausgabe zeigt die Beispiel-URLs aus `azds list-uris`.

```console
Uri                                                  Status
---------------------------------------------------  ---------
http://dev.bikesharingweb.traefik.MY_CUSTOM_DOMAIN/  Available
http://dev.gateway.traefik.MY_CUSTOM_DOMAIN/         Available
```

Navigieren Sie zum Dienst *bikesharingweb*, indem Sie die öffentliche URL über den Befehl `azds list-uris` öffnen. Im obigen Beispiel lautet die öffentliche URL für den Dienst *bikesharingweb* `http://dev.bikesharingweb.traefik.MY_CUSTOM_DOMAIN/`.

Verwenden Sie den Befehl `azds space select`, um unter *dev* einen untergeordneten Bereich zu erstellen, und listen Sie die URLs für den Zugriff auf den untergeordneten Entwicklungsbereich auf.

```console
azds space select -n dev/azureuser1 -y
azds list-uris
```

Die folgende Ausgabe zeigt die Beispiel-URLs aus `azds list-uris` für den Zugriff auf die Beispielanwendung im untergeordneten Entwicklungsbereich *azureuser1*.

```console
Uri                                                  Status
---------------------------------------------------  ---------
http://azureuser1.s.dev.bikesharingweb.traefik.MY_CUSTOM_DOMAIN/  Available
http://azureuser1.s.dev.gateway.traefik.MY_CUSTOM_DOMAIN/         Available
```

Navigieren Sie zum Dienst *bikesharingweb* im untergeordneten Entwicklungsbereich *azureuser1*, indem Sie die öffentliche URL über den Befehl `azds list-uris` öffnen. Im obigen Beispiel wird für den Dienst *bikesharingweb* im untergeordneten Entwicklungsbereich *azureuser1* die öffentliche URL `http://azureuser1.s.dev.bikesharingweb.traefik.MY_CUSTOM_DOMAIN/` verwendet.

## <a name="configure-the-traefik-ingress-controller-to-use-https"></a>Konfigurieren des Traefik-Eingangscontrollers für die Verwendung von HTTPS

Erstellen Sie eine Datei `dev-spaces/samples/BikeSharingApp/traefik-values.yaml`, die dem folgenden Beispiel ähnelt. Ändern Sie den Wert *email* in Ihre eigene E-Mail-Adresse, die zum Generieren des Zertifikats von Let's Encrypt verwendet wird.

```yaml
fullnameOverride: traefik
replicas: 1
cpuLimit: 400m
memoryRequest: 200Mi
memoryLimit: 500Mi
externalTrafficPolicy: Local
kubernetes:
  ingressClass: traefik
  ingressEndpoint:
    useDefaultPublishedService: true
dashboard:
  enabled: false
debug:
  enabled: false
accessLogs:
  enabled: true
  fields:
    defaultMode: keep
    headers:
      defaultMode: keep
      names:
        Authorization: redact
acme:
  enabled: true
  email: "someone@example.com"
  staging: false
  challengeType: tls-alpn-01
ssl:
  enabled: true
  enforced: true
  permanentRedirect: true
  tlsMinVersion: VersionTLS12
  cipherSuites:
    - TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
    - TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
    - TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
    - TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
    - TLS_RSA_WITH_AES_128_GCM_SHA256
    - TLS_RSA_WITH_AES_256_GCM_SHA384
    - TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
```

Aktualisieren Sie Ihren *Traefik*-Dienst mithilfe von `helm repo update` und einschließlich der von Ihnen erstellten Datei *traefik-values.yaml*.

```console
cd ..
helm upgrade traefik stable/traefik --namespace traefik --values traefik-values.yaml
```

Der obige Befehl führt eine neue Version des Traefik-Diensts mit den Werten aus der Datei *traefik-values.yaml* aus und entfernt den vorherigen Dienst. Der Traefik-Dienst erstellt auch ein TLS-Zertifikat mit Let's Encrypt und beginnt mit der Umleitung des Webdatenverkehrs über HTTPS.

> [!NOTE]
> Es kann einige Minuten dauern, bis die neue Version des Traefik-Diensts gestartet wird. Sie können den Fortschritt mithilfe von `kubectl get pods --namespace traefik --watch` überprüfen.

Navigieren Sie zur Beispielanwendung im untergeordneten Bereich *dev/azureuser1*, und beachten Sie, dass Sie zur Verwendung von HTTPS umgeleitet werden. Beachten Sie auch, dass die Seite geladen wird, im Browser jedoch einige Fehler angezeigt werden. Wenn Sie die Browserkonsole öffnen, wird angezeigt, dass sich der Fehler auf eine HTTPS-Seite bezieht, auf der HTTP-Ressourcen geladen werden. Beispiel:

```console
Mixed Content: The page at 'https://azureuser1.s.dev.bikesharingweb.traefik.MY_CUSTOM_DOMAIN/devsignin' was loaded over HTTPS, but requested an insecure resource 'http://azureuser1.s.dev.gateway.traefik.MY_CUSTOM_DOMAIN/api/user/allUsers'. This request has been blocked; the content must be served over HTTPS.
```

Aktualisieren Sie zum Beheben dieses Fehlers die Datei [BikeSharingWeb/azds.yaml][azds-yaml] so, dass *traefik* für *kubernetes.io/ingress.class* und Ihre benutzerdefinierte Domäne für *$(hostSuffix)* verwendet wird. Beispiel:

```yaml
...
    ingress:
      annotations:
        kubernetes.io/ingress.class: traefik
      hosts:
      # This expands to [space.s.][rootSpace.]bikesharingweb.<random suffix>.<region>.azds.io
      - $(spacePrefix)$(rootSpacePrefix)bikesharingweb.traefik.MY_CUSTOM_DOMAIN
...
```

Aktualisieren Sie [BikeSharingWeb/package.json][package-json] mit einer Abhängigkeit für das *URL*-Paket.

```json
{
...
    "react-responsive": "^6.0.1",
    "universal-cookie": "^3.0.7",
    "url": "0.11.0"
  },
...
```

Aktualisieren Sie die *getApiHostAsync*-Methode in [BikeSharingWeb/pages/helpers.js][helpers-js] zur Verwendung von HTTPS:

```javascript
...
    getApiHostAsync: async function() {
        const apiRequest = await fetch('/api/host');
        const data = await apiRequest.json();
        
        var urlapi = require('url');
        var url = urlapi.parse(data.apiHost);

        console.log('apiHost: ' + "https://"+url.host);
        return "https://"+url.host;
    },
...
```

Navigieren Sie zum Verzeichnis `BikeSharingWeb`, und verwenden Sie `azds up`, um den aktualisierten Dienst BikeSharingWeb auszuführen.

```console
cd BikeSharingWeb/
azds up
```

Navigieren Sie zur Beispielanwendung im untergeordneten Bereich *dev/azureuser1*, und beachten Sie, dass Sie ohne Fehler zur Verwendung von HTTPS umgeleitet werden.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich darüber, wie Azure Dev Spaces Sie bei der Entwicklung komplexerer containerübergreifender Anwendungen unterstützt und wie Sie die gemeinsame Entwicklung vereinfachen können, indem Sie in verschiedenen Bereichen mit verschiedenen Versionen oder Branches Ihres Codes arbeiten.

> [!div class="nextstepaction"]
> [Schnellstart: Entwicklung im Team mit Java unter Kubernetes mithilfe von Azure Dev Spaces][team-development-qs]


[az-cli]: /cli/azure/install-azure-cli?view=azure-cli-latest
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-network-dns-record-set-a-add-record]: /cli/azure/network/dns/record-set/a?view=azure-cli-latest#az-network-dns-record-set-a-add-record
[custom-domain]: ../../app-service/manage-custom-dns-buy-domain.md#buy-the-domain
[dns-zone]: ../../dns/dns-getstarted-cli.md
[qs-cli]: ../quickstart-cli.md
[team-development-qs]: ../quickstart-team-development.md

[azds-yaml]: https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/BikeSharingWeb/azds.yaml
[azure-account-create]: https://azure.microsoft.com/free
[helm-installed]: https://helm.sh/docs/using_helm/#installing-helm
[helpers-js]: https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/BikeSharingWeb/pages/helpers.js#L7
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[package-json]: https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/BikeSharingWeb/package.json
[values-yaml]: https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/charts/values.yaml