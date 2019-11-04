---
title: Verwenden eines Containers für den Speech-Dienst mit Kubernetes und Helm
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie ein Kubernetes-Paket erstellen und dabei Kubernetes und Helm verwenden, um die Containerimages für Spracherkennung und Sprachsynthese zu definieren. Dieses Paket wird dann lokal für einen Kubernetes-Cluster bereitgestellt.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: dapine
ms.openlocfilehash: b413bc6d29f1b08949b50570cb5baa2eb758d779
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73491024"
---
# <a name="use-speech-service-container-with-kubernetes-and-helm"></a>Verwenden eines Containers für den Speech-Dienst mit Kubernetes und Helm

Eine Möglichkeit zur lokalen Verwaltung Ihrer Speech-Container ist die Verwendung von Kubernetes und Helm. Hier erfahren Sie, wie Sie ein Kubernetes-Paket erstellen und dabei Kubernetes und Helm verwenden, um die Containerimages für Spracherkennung und Sprachsynthese zu definieren. Dieses Paket wird dann lokal für einen Kubernetes-Cluster bereitgestellt. Außerdem erfahren Sie, wie Sie die bereitgestellten Dienste testen, und es werden verschiedene Konfigurationsoptionen vorgestellt. Weitere Informationen zum Ausführen von Docker-Containern ohne Kubernetes-Orchestrierung finden Sie unter [Installieren und Ausführen von Containern für den Speech-Dienst](speech-container-howto.md).

## <a name="prerequisites"></a>Voraussetzungen

Die folgenden Voraussetzungen müssen erfüllt sein, damit Speech-Container lokal verwendet werden können:

|Erforderlich|Zweck|
|--|--|
| Azure-Konto | Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto][free-azure-account] erstellen, bevor Sie beginnen. |
| Zugriff auf die Containerregistrierung | Kubernetes benötigt Zugriff auf die Containerregistrierung, um die Docker-Images in den Cluster pullen zu können. |
| Kubernetes-Befehlszeilenschnittstelle | Mithilfe der [Kubernetes-Befehlszeilenschnittstelle][kubernetes-cli] werden die gemeinsam genutzten Anmeldeinformationen aus der Containerregistrierung verwaltet. Kubernetes wird außerdem vor Helm (Kubernetes-Paket-Manager) benötigt. |
| Helm-Befehlszeilenschnittstelle | Im Rahmen der Installation der [Helm-CLI][helm-install] müssen Sie auch Helm initialisieren, wodurch [Tiller][tiller-install] installiert wird. |
|Speech-Ressource |Um diese Container verwenden zu können, benötigen Sie Folgendes:<br><br>Eine Azure-Ressource vom Typ _Speech_, um den entsprechenden Abrechnungsschlüssel und den URI des Abrechnungsendpunkts zu erhalten. Beide Werte stehen im Azure-Portal auf der Übersichts- und auf der Schlüsselseite für **Speech** zur Verfügung und werden zum Starten des Containers benötigt.<br><br>**{API_KEY}** : Der Ressourcenschlüssel.<br><br>**{ENDPOINT_URI}** : Der Endpunkt-URI. Beispiel: `https://westus.api.cognitive.microsoft.com/sts/v1.0`|

## <a name="the-recommended-host-computer-configuration"></a>Empfohlene Hostcomputerkonfiguration

Orientieren Sie sich an den Details unter [Der Hostcomputer][speech-container-host-computer] des Artikels „Installieren und Ausführen von Containern für den Speech-Dienst“. Dieses *Helm-Chart* berechnet automatisch die CPU- und Arbeitsspeicheranforderungen auf der Grundlage der vom Benutzer angegebenen Anzahl von Decodierungen (gleichzeitige Anforderungen). Darüber hinaus wird es entsprechend angepasst, wenn Optimierungen für die Audio-/Texteingabe als `enabled` konfiguriert sind. Standardmäßig verwendet das Helm-Chart zwei gleichzeitige Anforderungen und keine Optimierung.

| Dienst | CPU/Container | Arbeitsspeicher/Container |
|--|--|--|
| **Spracherkennung** | Für einen einzelnen Decoder sind mindestens 1.150 Millicores erforderlich. Wenn `optimizedForAudioFile` aktiviert ist, werden 1.950 Millicores benötigt. (Standard: zwei Decoder) | Erforderlich: 2 GB<br>Eingeschränkt:  4 GB |
| **Text-zu-Sprache** | Pro gleichzeitiger Anforderung sind mindestens 500 Millicores erforderlich. Wenn `optimizeForTurboMode` aktiviert ist, werden 1.000 Millicores benötigt. (Standard: zwei gleichzeitige Anforderungen) | Erforderlich: 1 GB<br> Eingeschränkt: 2 GB |

## <a name="connect-to-the-kubernetes-cluster"></a>Herstellen einer Verbindung mit dem Kubernetes-Cluster

Für den Hostcomputer muss ein verfügbarer Kubernetes-Cluster vorhanden sein. Das Konzept der Bereitstellung eines Kubernetes-Clusters für einen Hostcomputer wird im Tutorial [Bereitstellen eines Azure Kubernetes Service-Clusters (AKS)](../../aks/tutorial-kubernetes-deploy-cluster.md) erläutert.

### <a name="sharing-docker-credentials-with-the-kubernetes-cluster"></a>Weitergeben von Docker-Anmeldeinformationen an den Kubernetes-Cluster

Damit der Kubernetes-Cluster die konfigurierten Images mittels `docker pull` aus der Containerregistrierung `mcr.microsoft.com` pullen kann, müssen die Docker-Anmeldeinformationen in den Cluster übertragen werden. Führen Sie den Befehl [`kubectl create`][kubectl-create] wie weiter unten gezeigt aus, um ein *Docker-Registrierungsgeheimnis* auf der Grundlage der Anmeldeinformationen zu erstellen, die im Rahmen der Vorbereitung für den Zugriff auf die Containerregistrierung angegeben wurden.

Führen Sie über die Befehlszeilenschnittstelle Ihrer Wahl den folgenden Befehl aus. Ersetzen Sie dabei `<username>`, `<password>` und `<email-address>` durch die Anmeldeinformationen der Containerregistrierung.

```console
kubectl create secret docker-registry mcr \
    --docker-server=mcr.microsoft.com \
    --docker-username=<username> \
    --docker-password=<password> \
    --docker-email=<email-address>
```

> [!NOTE]
> Falls Sie bereits über Zugriff auf die Containerregistrierung `mcr.microsoft.com` verfügen, können Sie stattdessen auch ein Kubernetes-Geheimnis mit dem generischen Flag erstellen. Der folgende Befehl wird für den JSON-Code Ihrer Docker-Konfiguration ausgeführt:
> ```console
>  kubectl create secret generic mcr \
>      --from-file=.dockerconfigjson=~/.docker/config.json \
>      --type=kubernetes.io/dockerconfigjson
> ```

Nach erfolgreicher Erstellung des Geheimnisses wird in der Konsole Folgendes ausgegeben:

```console
secret "mcr" created
```

Vergewissern Sie sich, dass das Geheimnis erstellt wurde, indem Sie den Befehl [`kubectl get`][kubectl-get] mit dem Flag `secrets` ausführen:

```console
kuberctl get secrets
```

Der Befehl `kubectl get secrets` gibt alle konfigurierten Geheimnisse zurück:

```console
NAME    TYPE                              DATA    AGE
mcr     kubernetes.io/dockerconfigjson    1       30s
```

## <a name="configure-helm-chart-values-for-deployment"></a>Konfigurieren von Helm-Chart-Werten für die Bereitstellung

Im [Helm-Hub von Microsoft][ms-helm-hub] finden Sie alle öffentlich verfügbaren Helm-Charts von Microsoft. Dort steht auch das Chart **cognitive-services-speech-onpremise** zur Verfügung. Wir installieren hier das Chart **cognitive-services-speech-onpremise**. Zuvor müssen wir jedoch die Datei `config-values.yaml` mit expliziten Konfigurationen erstellen. Als Erstes fügen wir das Microsoft-Repository unserer Helm-Instanz hinzu:

```console
helm repo add microsoft https://microsoft.github.io/charts/repo
```

Danach konfigurieren wir unsere Helm-Chart-Werte. Kopieren Sie den folgenden YAML-Code, und fügen Sie ihn in eine Datei mit dem Namen `config-values.yaml` ein. Weitere Informationen zum Anpassen des Charts **cognitive-services-speech-onpremise** finden Sie unter [Anpassen von Helm-Charts](#customize-helm-charts). Ersetzen Sie die Kommentare `# {ENDPOINT_URI}` und `# {API_KEY}` durch Ihre eigenen Werte.

```yaml
# These settings are deployment specific and users can provide customizations

# speech-to-text configurations
speechToText:
  enabled: true
  numberOfConcurrentRequest: 3
  optimizeForAudioFile: true
  image:
    registry: mcr.microsoft.com
    repository: azure-cognitive-services/speech-to-text
    tag: latest
    pullSecrets:
      - mcr # Or an existing secret
    args:
      eula: accept
      billing: # {ENDPOINT_URI}
      apikey: # {API_KEY}

# text-to-speech configurations
textToSpeech:
  enabled: true
  numberOfConcurrentRequest: 3
  optimizeForTurboMode: true
  image:
    registry: mcr.microsoft.com
    repository: azure-cognitive-services/text-to-speech
    tag: latest
    pullSecrets:
      - mcr # Or an existing secret
    args:
      eula: accept
      billing: # {ENDPOINT_URI}
      apikey: # {API_KEY}
```

> [!IMPORTANT]
> Ohne Angabe der Werte `billing` und `apikey` laufen die Dienste nach 15 Minuten ab. Dies führt dann dazu, dass die Überprüfung nicht erfolgreich ist, da die Dienste nicht zur Verfügung stehen.

### <a name="the-kubernetes-package-helm-chart"></a>Das Kubernetes-Paket (Helm-Chart)

Das *Helm-Chart* enthält die Konfiguration, die angibt, welche Docker-Images aus der Containerregistrierung `mcr.microsoft.com` gepullt werden sollen.

> Bei einem [Helm-Chart][helm-charts] handelt es sich um eine Sammlung von Dateien, die eine zusammengehörige Gruppe von Kubernetes-Ressourcen beschreiben. Ein einzelnes Chart kann sowohl für eine einfache Bereitstellung (beispielsweise eines Memcached-Pods) als auch für komplexere Bereitstellungen (etwa eines vollständigen Web-App-Stapels mit HTTP-Servern, Datenbanken, Caches und Ähnlichem) verwendet werden.

Die bereitgestellten *Helm-Charts* pullen die Docker-Images des Speech-Diensts (Sprachsynthese- und Spracherkennungsdienst) aus der Containerregistrierung `mcr.microsoft.com`.

## <a name="install-the-helm-chart-on-the-kubernetes-cluster"></a>Installieren des Helm-Charts für den Kubernetes-Cluster

Führen Sie zum Installieren des *Helm-Charts* den Befehl [`helm install`][helm-install-cmd] aus, und ersetzen Sie dabei den Platzhalter `<config-values.yaml>` durch das entsprechende Pfad- und Dateinamenargument. Das unten referenzierte Helm-Chart `microsoft/cognitive-services-speech-onpremise` finden Sie [hier][ms-helm-hub-speech-chart].

```console
helm install microsoft/cognitive-services-speech-onpremise \
    --version 0.1.1 \
    --values <config-values.yaml> \
    --name onprem-speech
```

Die Ausgabe nach einer erfolgreichen Installation kann beispielsweise wie folgt aussehen:

```console
NAME:   onprem-speech
LAST DEPLOYED: Tue Jul  2 12:51:42 2019
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1/Pod(related)
NAME                             READY  STATUS             RESTARTS  AGE
speech-to-text-7664f5f465-87w2d  0/1    Pending            0         0s
speech-to-text-7664f5f465-klbr8  0/1    ContainerCreating  0         0s
text-to-speech-56f8fb685b-4jtzh  0/1    ContainerCreating  0         0s
text-to-speech-56f8fb685b-frwxf  0/1    Pending            0         0s

==> v1/Service
NAME            TYPE          CLUSTER-IP    EXTERNAL-IP  PORT(S)       AGE
speech-to-text  LoadBalancer  10.0.252.106  <pending>    80:31811/TCP  1s
text-to-speech  LoadBalancer  10.0.125.187  <pending>    80:31247/TCP  0s

==> v1beta1/PodDisruptionBudget
NAME                                MIN AVAILABLE  MAX UNAVAILABLE  ALLOWED DISRUPTIONS  AGE
speech-to-text-poddisruptionbudget  N/A            20%              0                    1s
text-to-speech-poddisruptionbudget  N/A            20%              0                    1s

==> v1beta2/Deployment
NAME            READY  UP-TO-DATE  AVAILABLE  AGE
speech-to-text  0/2    2           0          0s
text-to-speech  0/2    2           0          0s

==> v2beta2/HorizontalPodAutoscaler
NAME                       REFERENCE                  TARGETS        MINPODS  MAXPODS  REPLICAS  AGE
speech-to-text-autoscaler  Deployment/speech-to-text  <unknown>/50%  2        10       0         0s
text-to-speech-autoscaler  Deployment/text-to-speech  <unknown>/50%  2        10       0         0s


NOTES:
cognitive-services-speech-onpremise has been installed!
Release is named onprem-speech
```

Der Kubernetes-Bereitstellungsvorgang kann mehrere Minuten dauern. Vergewissern Sie sich durch Ausführen des folgenden Befehls, dass die Pods und Dienste ordnungsgemäß bereitgestellt wurden und verfügbar sind:

```console
kubectl get all
```

Die Ausgabe sollte in etwa wie folgt aussehen:

```console
NAME                                  READY     STATUS    RESTARTS   AGE
pod/speech-to-text-7664f5f465-87w2d   1/1       Running   0          34m
pod/speech-to-text-7664f5f465-klbr8   1/1       Running   0          34m
pod/text-to-speech-56f8fb685b-4jtzh   1/1       Running   0          34m
pod/text-to-speech-56f8fb685b-frwxf   1/1       Running   0          34m

NAME                     TYPE           CLUSTER-IP     EXTERNAL-IP      PORT(S)        AGE
service/kubernetes       ClusterIP      10.0.0.1       <none>           443/TCP        3h
service/speech-to-text   LoadBalancer   10.0.252.106   52.162.123.151   80:31811/TCP   34m
service/text-to-speech   LoadBalancer   10.0.125.187   65.52.233.162    80:31247/TCP   34m

NAME                             DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/speech-to-text   2         2         2            2           34m
deployment.apps/text-to-speech   2         2         2            2           34m

NAME                                        DESIRED   CURRENT   READY     AGE
replicaset.apps/speech-to-text-7664f5f465   2         2         2         34m
replicaset.apps/text-to-speech-56f8fb685b   2         2         2         34m

NAME                                                            REFERENCE                   TARGETS   MINPODS   MAXPODS   REPLICAS   AGE
horizontalpodautoscaler.autoscaling/speech-to-text-autoscaler   Deployment/speech-to-text   1%/50%    2         10        2          34m
horizontalpodautoscaler.autoscaling/text-to-speech-autoscaler   Deployment/text-to-speech   0%/50%    2         10        2          34m
```

### <a name="verify-helm-deployment-with-helm-tests"></a>Überprüfen der Helm-Bereitstellung mit Helm-Tests

Die installierten Helm-Charts definieren *Helm-Tests*, um die Überprüfung zu vereinfachen. Diese Tests dienen zur Überprüfung der Dienstbereitschaft. Wir führen den Befehl [Helm test][helm-test] aus, um sowohl die **Spracherkennung** als auch die **Sprachsynthese** zu überprüfen.

```console
helm test onprem-speech
```

> [!IMPORTANT]
> Diese Tests sind nicht erfolgreich, wenn der Podstatus nicht `Running` lautet oder die Bereitstellung nicht in der Spalte `AVAILABLE` aufgeführt ist. Dieser Vorgang kann mehr als zehn Minuten dauern.

Von diesen Tests werden verschiedene Statusergebnisse ausgegeben:

```console
RUNNING: speech-to-text-readiness-test
PASSED: speech-to-text-readiness-test
RUNNING: text-to-speech-readiness-test
PASSED: text-to-speech-readiness-test
```

Als Alternative zur Ausführung der *Helm-Tests* können Sie auch die *externen IP-Adressen* und die entsprechenden Ports des Befehls `kubectl get all` sammeln. Öffnen Sie unter Verwendung der IP-Adresse und des Ports einen Webbrowser, und navigieren Sie zu `http://<external-ip>:<port>:/swagger/index.html`, um die API-Swagger-Seite(n) anzuzeigen.

## <a name="customize-helm-charts"></a>Anpassen von Helm-Charts

Helm-Charts sind hierarchisch. Die hierarchische Struktur ermöglicht Chart-Vererbungen und kommt auch dem Konzept der Spezifität zugute, das dafür sorgt, dass geerbte Regeln durch spezifischere Einstellungen überschrieben werden.

[!INCLUDE [Speech umbrella-helm-chart-config](includes/speech-umbrella-helm-chart-config.md)]

[!INCLUDE [Speech-to-Text Helm Chart Config](includes/speech-to-text-chart-config.md)]

[!INCLUDE [Text-to-Speech Helm Chart Config](includes/text-to-speech-chart-config.md)]

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
[ms-helm-hub]: https://hub.helm.sh/charts/microsoft
[ms-helm-hub-speech-chart]: https://hub.helm.sh/charts/microsoft/cognitive-services-speech-onpremise

<!-- LINKS - internal -->
[speech-container-host-computer]: speech-container-howto.md#the-host-computer
[installing-helm-apps-in-aks]: ../../aks/kubernetes-helm.md
[cog-svcs-containers]: ../cognitive-services-container-support.md
