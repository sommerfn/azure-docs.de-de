---
title: Installieren von Istio in Azure Kubernetes Service (AKS)
description: Erfahren Sie, wie Sie Istio installieren und zum Erstellen eines Service Mesh in einem AKS-Cluster (Azure Kubernetes Service) verwenden.
author: paulbouwer
ms.service: container-service
ms.topic: article
ms.date: 10/09/2019
ms.author: pabouwer
zone_pivot_groups: client-operating-system
ms.openlocfilehash: 9c9dcd567b8632626bf4b1f0bf2ef6b5e69b8a9d
ms.sourcegitcommit: f29fec8ec945921cc3a89a6e7086127cc1bc1759
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72529846"
---
# <a name="install-and-use-istio-in-azure-kubernetes-service-aks"></a>Installieren und Verwenden von Istio in Azure Kubernetes Service (AKS)

[Istio][istio-github] ist ein Open-Source-Dienstmesh, das eine Reihe wichtiger Funktionen für Microservices in einem Kubernetes-Cluster bereitstellt. Zu diesen Funktionen gehören: Verwaltung des Datenverkehrs, Dienstidentität und -sicherheit, Richtlinienerzwingung und Telemetrie. Weitere Informationen zu Istio finden Sie in der offiziellen Dokumentation [What is Istio?][istio-docs-concepts] (Was ist Istio?).

In diesem Artikel wird gezeigt, wie Sie Istio installieren. Die Istio-Clientbinärdatei `istioctl` wird auf Ihrem Clientcomputer installiert, und anschließend werden die Istio-Komponenten in einem Kubernetes-Cluster in AKS installiert.

> [!NOTE]
> Diese Anweisungen beziehen sich auf die Istio-Version `1.3.2`, und es wird mindestens die Helm-Version `2.14.2` verwendet.
>
> Der Istio-Versionen `1.3.x` wurden vom Istio-Team mit den Kubernetes-Versionen `1.13`, `1.14` und `1.15` getestet. Weitere Istio-Versionen finden Sie unter [GitHub – Istio-Releases][istio-github-releases]. Informationen zu den Releases finden Sie unter [Istio-Neuigkeiten][istio-release-notes], und unterstützte Kubernetes-Versionen finden Sie in den [allgemeinen FAQ für Istio][istio-faq].

In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Herunterladen und Installieren der Istio-Clientbinärdatei „istioctl“
> * Installieren von Istio in AKS
> * Überprüfen der Istio-Installation
> * Zugreifen auf die Add-Ons
> * Deinstallieren von Istio aus AKS

## <a name="before-you-begin"></a>Voraussetzungen

Bei den in diesem Artikel beschriebenen Schritten wird vorausgesetzt, dass Sie einen AKS-Cluster (Kubernetes `1.13` und höher, RBAC aktiviert) erstellt und eine `kubectl`-Verbindung mit dem Cluster hergestellt haben. Wenn Sie Hilfe bei einem dieser Elemente benötigen, beachten Sie das Dokument [AKS-Schnellstart][aks-quickstart].

Sie benötigen für diese Anweisungen [Helm][helm] und müssen Istio installieren. In Ihrem Cluster sollte die neueste stabile Version installiert und ordnungsgemäß konfiguriert sein. Wenn Sie Hilfe bei der Installation von Helm benötigen, beachten Sie den [AKS Helm-Installationsleitfaden][helm-install]. Alle Istio-Pods müssen auch für die Ausführung auf Linux-Knoten geplant werden.

Stellen Sie sicher, dass Sie die Dokumentation zur [Istio-Leistung und -Skalierbarkeit](https://istio.io/docs/concepts/performance-and-scalability/) gelesen und die zusätzlichen Ressourcenanforderungen für die Ausführung von Istio in Ihrem AKS-Cluster verstanden haben. Die Anforderungen an Core und Arbeitsspeicher variieren je nach Workload. Wählen Sie eine geeignete Anzahl von Knoten und eine geeignete VM-Größe für Ihr Setup aus.

Im vorliegenden Artikel wird die Istio-Installationsanleitung in mehrere Einzelschritte untergliedert. Das Endergebnis hat dieselbe Struktur wie beim Befolgen der offiziellen [Anleitung][istio-install-helm] für die Installation von Istio.

::: zone pivot="client-operating-system-linux"

[!INCLUDE [Linux - download and install client binary](includes/servicemesh/istio/install-client-binary-linux.md)]

::: zone-end

::: zone pivot="client-operating-system-macos"

[!INCLUDE [MacOS - download and install client binary](includes/servicemesh/istio/install-client-binary-macos.md)]

::: zone-end

::: zone pivot="client-operating-system-windows"

[!INCLUDE [Windows - download and install client binary](includes/servicemesh/istio/install-client-binary-windows.md)]

::: zone-end

## <a name="add-the-istio-helm-chart-repository"></a>Hinzufügen des Helm-Chartrepositorys für Istio

Fügen Sie das Helm-Chartrepository für das Istio-Release hinzu. Führen Sie `helm repo update` aus, um Ihre lokalen Informationen für das Chartrepository zu aktualisieren.

```azurecli
helm repo add istio.io https://storage.googleapis.com/istio-release/releases/$ISTIO_VERSION/charts/
helm repo update
```

## <a name="install-the-istio-crds-on-aks"></a>Installieren der Istio-CRDs in AKS

Istio verwendet [benutzerdefinierte Ressourcendefinitionen (Custom Resource Definitions, CRDs)][kubernetes-crd] für das Verwalten der Laufzeitkonfiguration. Wir müssen die Istio-CRDs zuerst installieren, da die Istio-Komponenten Abhängigkeiten zu diesen aufweisen. Verwenden Sie Helm und das Diagramm `istio-init` für das Installieren der Istio-CRDs im `istio-system`-Namespace in Ihrem AKS-Cluster:

```azurecli
helm install istio.io/istio-init --name istio-init --namespace istio-system
```

[Aufträge][kubernetes-jobs] werden als Teil des Helm-Diagramms `istio-init` bereitgestellt, um die CRDs zu installieren. Die Ausführung dieser Aufträge sollte je nach Ihrer Clusterumgebung weniger als 20 Sekunden dauern. Folgendermaßen können Sie überprüfen, ob die Aufträge erfolgreich abgeschlossen wurden:

```azurecli
kubectl get jobs -n istio-system
```

Die folgende Beispielausgabe zeigt, dass die Aufträge erfolgreich abgeschlossen wurden.

```console
NAME                      COMPLETIONS   DURATION   AGE
istio-init-crd-10-1.3.2   1/1           14s        14s
istio-init-crd-11-1.3.2   1/1           12s        14s
istio-init-crd-12-1.3.2   1/1           14s        14s
```

Nachdem Sie den erfolgreichen Abschluss der Aufträge bestätigt haben, überprüfen Sie, ob die richtige Anzahl von Istio-CRDs installiert wurde. Sie können überprüfen, ob alle 23 Istio-CRDs installiert wurden, indem Sie den folgenden Befehl ausführen. Der Befehl sollte als Anzahl `23` zurückgeben.

::: zone pivot="client-operating-system-linux"

[!INCLUDE [Bash - check CRD count](includes/servicemesh/istio/install-check-crd-count-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-macos"

[!INCLUDE [Bash - check CRD count](includes/servicemesh/istio/install-check-crd-count-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-windows"

[!INCLUDE [PowerShell - check CRD count](includes/servicemesh/istio/install-check-crd-count-powershell.md)]

::: zone-end

Wenn Sie diesen Punkt erreichen, haben Sie die Istio-CRDs erfolgreich installiert. Fahren Sie nun mit dem nächsten Abschnitt [Installieren der Istio-Komponenten in AKS](#install-the-istio-components-on-aks) fort.

## <a name="install-the-istio-components-on-aks"></a>Installieren der Istio-Komponenten in AKS

Wir installieren [Grafana][grafana] und [Kiali][kiali] als Teil unserer Istio-Installation. Grafana stellt Analyse- und Überwachungsdashboards bereit, und Kiali bietet ein Übersichtsdashboard für das Dienstmesh. Bei diesem Setup erfordert jede dieser Komponenten Anmeldeinformationen, die als ein [Geheimnis][kubernetes-secrets] bereitgestellt werden müssen.

Bevor Sie die Istio-Komponenten installieren können, müssen Sie die Geheimnisse für Grafana und Kiali erstellen. 

::: zone pivot="client-operating-system-linux"

[!INCLUDE [Bash - create secrets for Grafana and Kiali](includes/servicemesh/istio/install-create-secrets-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-macos"

[!INCLUDE [Bash check for CRDs](includes/servicemesh/istio/install-create-secrets-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-windows"

[!INCLUDE [PowerShell check for CRDs](includes/servicemesh/istio/install-create-secrets-powershell.md)]

::: zone-end

### <a name="install-istio-components"></a>Installieren der Istio-Komponenten

Nachdem Sie in Ihrem AKS-Cluster erfolgreich die Geheimnisse für Grafana und Kiali erstellt haben, können Sie die Istio-Komponenten installieren. Verwenden Sie Helm und das Diagramm `istio` für das Installieren der Istio-Komponenten im `istio-system`-Namespace in Ihrem AKS-Cluster. 

> [!NOTE]
> **Installationsoptionen**
> 
> Im Rahmen dieser Installation verwenden Sie die folgenden Optionen:
> - `global.controlPlaneSecurityEnabled=true`: Aktivieren von gegenseitigem TLS für die Steuerungsebene
> - `global.mtls.enabled=true`: Festlegen, dass für jegliche Kommunikation zwischen Diensten mTLS verwendet werden muss
> - `grafana.enabled=true`: Aktivieren der Grafana-Bereitstellung für Analyse- und Überwachungsdashboards
> - `grafana.security.enabled=true`: Aktivieren der Authentifizierung für Grafana
> - `tracing.enabled=true`: Aktivieren der Jaeger-Bereitstellung für die Ablaufverfolgung
> - `kiali.enabled=true`: Aktivieren der Kiali-Bereitstellung für ein Übersichtsdashboard für das Dienstmesh
>
> **Knotenselektoren**
>
> Istio muss aktuell für die Ausführung auf Linux-Knoten geplant werden. Wenn sich in Ihrem Cluster Windows Server-Knoten befinden, müssen Sie sicherstellen, dass Istio-Pods nur für die Ausführung auf Linux-Knoten geplant sind. Wir verwenden [Knotenselektoren][kubernetes-node-selectors], um sicherzustellen, dass Pods für die richtigen Knoten geplant werden.

> [!CAUTION]
> Die Istio-Features [SDS (Secret Discovery Service)][istio-feature-sds] und [Istio CNI][istio-feature-cni] befinden sich derzeit in der [Alpha-Phase][istio-feature-stages] und müssen daher mit Bedacht verwendet werden. Darüber hinaus ist das Kubernetes-Feature [Service Account Token Volume Projection][kubernetes-feature-sa-projected-volume] (Dienstkontotoken-Volumenprojektion; eine Voraussetzung für SDS) in aktuellen AKS-Versionen nicht aktiviert.

::: zone pivot="client-operating-system-linux"

[!INCLUDE [Bash - install Istio components](includes/servicemesh/istio/install-components-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-macos"

[!INCLUDE [Bash - install Istio components](includes/servicemesh/istio/install-components-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-windows"

[!INCLUDE [PowerShell - install Istio components](includes/servicemesh/istio/install-components-powershell.md)]

::: zone-end

Das Helm-Diagramm `istio` stellt eine große Anzahl von Objekten bereit. Eine Liste der Ausgabe Ihres Befehls `helm install` finden Sie weiter oben. Die Bereitstellung der Istio-Komponenten sollte je nach Clusterumgebung weniger als zwei Minuten dauern.

An diesem Punkt haben Sie Istio in Ihrem AKS-Cluster bereitgestellt. Um eine erfolgreiche Bereitstellung von Istio sicherzustellen, fahren Sie mit dem nächsten Abschnitt [Überprüfen der Istio-Installation](#validate-the-istio-installation) fort.

## <a name="validate-the-istio-installation"></a>Überprüfen der Istio-Installation

Vergewissern Sie sich zunächst, dass die erwarteten Dienste erstellt wurden. Verwenden Sie den Befehl [Kubectl get svc][kubectl-get], um die ausgeführten Dienste anzuzeigen. Fragen Sie den `istio-system`-Namespace ab, in dem die Istio- und Add-On-Komponenten vom Helm-Diagramm `istio` installiert wurden:

```console
kubectl get svc --namespace istio-system --output wide
```

Die folgende Beispielausgabe zeigt die Dienste, die jetzt ausgeführt werden sollten:

- `istio-*`-Dienste
- Add-On-Ablaufverfolgungsdienste `jaeger-*`, `tracing` und `zipkin`
- Add-On-Metrikdienst `prometheus`
- Add-On-Dienst `grafana` für Analyse- und -Überwachungsdashboards
- Add-On-Dashboarddienst `kiali` für das Dienstmesh

Wenn das `istio-ingressgateway` für die externe IP-Adresse `<pending>` anzeigt, warten Sie einige Minuten, bis vom Azure-Netzwerk eine IP-Adresse zugewiesen wurde.

```console
NAME                     TYPE           CLUSTER-IP     EXTERNAL-IP      PORT(S)                                                                                                                                      AGE   SELECTOR
grafana                  ClusterIP      10.0.164.244   <none>           3000/TCP                                                                                                                                     53s   app=grafana
istio-citadel            ClusterIP      10.0.49.16     <none>           8060/TCP,15014/TCP                                                                                                                           53s   istio=citadel
istio-galley             ClusterIP      10.0.175.173   <none>           443/TCP,15014/TCP,9901/TCP                                                                                                                   53s   istio=galley
istio-ingressgateway     LoadBalancer   10.0.226.151   20.188.221.111   15020:31128/TCP,80:31380/TCP,443:31390/TCP,31400:31400/TCP,15029:30817/TCP,15030:30436/TCP,15031:32485/TCP,15032:30980/TCP,15443:30124/TCP   53s   app=istio-ingressgateway,istio=ingressgateway,release=istio
istio-pilot              ClusterIP      10.0.102.158   <none>           15010/TCP,15011/TCP,8080/TCP,15014/TCP                                                                                                       53s   istio=pilot
istio-policy             ClusterIP      10.0.234.53    <none>           9091/TCP,15004/TCP,15014/TCP                                                                                                                 53s   istio-mixer-type=policy,istio=mixer
istio-sidecar-injector   ClusterIP      10.0.216.8     <none>           443/TCP,15014/TCP                                                                                                                            53s   istio=sidecar-injector
istio-telemetry          ClusterIP      10.0.154.215   <none>           9091/TCP,15004/TCP,15014/TCP,42422/TCP                                                                                                       53s   istio-mixer-type=telemetry,istio=mixer
jaeger-agent             ClusterIP      None           <none>           5775/UDP,6831/UDP,6832/UDP                                                                                                                   52s   app=jaeger
jaeger-collector         ClusterIP      10.0.26.109    <none>           14267/TCP,14268/TCP                                                                                                                          52s   app=jaeger
jaeger-query             ClusterIP      10.0.70.55     <none>           16686/TCP                                                                                                                                    52s   app=jaeger
kiali                    ClusterIP      10.0.36.206    <none>           20001/TCP                                                                                                                                    53s   app=kiali
prometheus               ClusterIP      10.0.236.99    <none>           9090/TCP                                                                                                                                     53s   app=prometheus
tracing                  ClusterIP      10.0.83.152    <none>           80/TCP                                                                                                                                       52s   app=jaeger
zipkin                   ClusterIP      10.0.25.86     <none>           9411/TCP                                                                                                                                     52s   app=jaeger
```

Vergewissern Sie sich als Nächstes, dass die erforderlichen Pods erstellt wurden. Verwenden Sie den Befehl [kubectl get pods][kubectl-get], und fragen Sie den `istio-system`-Namespace erneut ab:

```console
kubectl get pods --namespace istio-system
```

Die folgende Beispielausgabe zeigt die Pods, die ausgeführt werden:

- `istio-*`-Pods
- Add-On-Metrikpod `prometheus-*`
- Add-On-Pod `grafana-*` für Analyse- und -Überwachungsdashboards
- Add-On-Pod `kiali` für Dienstmeshdashboard

```console
NAME                                     READY   STATUS      RESTARTS   AGE
grafana-7c48555456-msl7b                 1/1     Running     0          88s
istio-citadel-566fc66db7-m8wgl           1/1     Running     0          87s
istio-galley-5746db8d56-pl5gg            1/1     Running     0          88s
istio-ingressgateway-6c94f7c9bf-f5lt5    1/1     Running     0          88s
istio-init-crd-10-1.3.2-xw9g2            0/1     Completed   0          92m
istio-init-crd-11-1.3.2-54rz8            0/1     Completed   0          92m
istio-init-crd-12-1.3.2-789qj            0/1     Completed   0          92m
istio-pilot-6748968b6d-rvdfx             2/2     Running     0          87s
istio-policy-7576bbbcf7-2stft            2/2     Running     0          87s
istio-sidecar-injector-76d79d494-7jk9n   1/1     Running     0          87s
istio-telemetry-74b7bf676d-tfrcl         2/2     Running     0          88s
istio-tracing-655d9588bc-d2htg           1/1     Running     0          86s
kiali-65d55bcfb8-tqrfk                   1/1     Running     0          88s
prometheus-846f9849bd-br8kp              1/1     Running     0          87s
```

Es müssen drei Pods vom Typ `istio-init-crd-*` mit dem Status `Completed` vorhanden sein. Diese Pods waren verantwortlich für die Ausführung der Aufträge zur Erstellung der CRDs in einem vorherigen Schritt. Alle anderen Pods sollten den Status `Running` aufweisen. Wenn Ihre Pods nicht diesen Status haben, warten Sie ein oder zwei Minuten, bis dies der Fall ist. Wenn einer der Pods ein Problem meldet, verwenden Sie den Befehl [kubectl describe pod][kubectl-describe], um die Ausgabe und den Status des betreffenden Pods zu überprüfen.

## <a name="accessing-the-add-ons"></a>Zugreifen auf die Add-Ons

Einige der Add-Ons, die bei der obigen Einrichtung durch Istio installiert wurden, bieten zusätzliche Funktionen. Die Webanwendungen für die Add-Ons werden **nicht** öffentlich über eine externe IP-Adresse verfügbar gemacht. 

Verwenden Sie den Befehl `istioctl dashboard`, um auf diese Add-On-Benutzeroberflächen zuzugreifen. Dieser Befehl nutzt [kubectl port-forward][kubectl-port-forward] und einen willkürlichen Port, um eine sichere Verbindung zwischen Ihrem Clientcomputer und dem relevanten Pod in Ihrem AKS-Cluster herzustellen. Anschließend wird automatisch die Add-On-Webanwendung in Ihrem Standardbrowser geöffnet.

Sie haben zuvor in diesem Artikel für Grafana und Kiali eine zusätzliche Sicherheitsebene hinzugefügt, indem Sie Anmeldeinformationen angegeben haben.

### <a name="grafana"></a>Grafana

Die Analyse- und Überwachungsdashboards für Istio werden von [Grafana][grafana] bereitgestellt. Sie müssen die Anmeldeinformationen verwenden, die Sie zuvor nach Aufforderung mithilfe des Grafana-Geheimnisses erstellten haben. Gehen Sie wie folgt vor, um das Grafana-Dashboard auf sichere Weise zu öffnen:

```console
istioctl dashboard grafana
```

### <a name="prometheus"></a>Prometheus

Die Metriken für Istio werden von [Prometheus][prometheus] bereitgestellt. Gehen Sie wie folgt vor, um das Prometheus-Dashboard auf sichere Weise zu öffnen:

```console
istioctl dashboard prometheus
```

### <a name="jaeger"></a>Jaeger

Die Ablaufverfolgung in Istio wird über [Jaeger][jaeger] bereitgestellt. Gehen Sie wie folgt vor, um das Jaeger-Dashboard auf sichere Weise zu öffnen:

```console
istioctl dashboard jaeger
```

### <a name="kiali"></a>Kiali

Ein Service Mesh-Telemetriedashboard wird von [Kiali][kiali] bereitgestellt. Sie müssen die Anmeldeinformationen verwenden, die Sie zuvor nach Aufforderung mithilfe des Kiali-Geheimnisses erstellten haben. Gehen Sie wie folgt vor, um das Kiali-Dashboard auf sichere Weise zu öffnen:

```console
istioctl dashboard kiali
```

### <a name="envoy"></a>Envoy

Für die [Envoy][envoy]-Proxys steht eine einfache Oberfläche zur Verfügung. Sie enthält Konfigurationsinformationen und Metriken für einen Envoy-Proxy, der in einem bestimmten Pod ausgeführt wird. Gehen Sie wie folgt vor, um die Envoy-Oberfläche auf sichere Weise zu öffnen:

```console
istioctl dashboard envoy <pod-name>.<namespace>
```

## <a name="uninstall-istio-from-aks"></a>Deinstallieren von Istio aus AKS

> [!WARNING]
> Das Löschen von Istio in einem laufenden System kann zu Problemen beim Datenverkehr zwischen Ihren Diensten führen. Stellen Sie sicher, dass Sie das System so weit vorbereitet haben, dass es auch ohne Istio ordnungsgemäß fortgesetzt werden kann.

### <a name="remove-istio-components-and-namespace"></a>Entfernen von Istio-Komponenten und -Namespace

Verwenden Sie die folgenden Befehle, um Istio aus Ihrem AKS-Cluster zu entfernen. Die Befehle `helm delete` entfernen die Diagramme `istio` und `istio-init`, und der Befehl `kubectl delete namespace` entfernt den `istio-system`-Namespace.

```azurecli
helm delete --purge istio
helm delete --purge istio-init
kubectl delete namespace istio-system
```

### <a name="remove-istio-crds-and-secrets"></a>Entfernen von Istio-CRDs und Geheimnissen

Durch die obigen Befehle werden alle Istio-Komponenten und der Namespace gelöscht, die Istio-CRDs und Geheimnisse bleiben jedoch erhalten. 

::: zone pivot="client-operating-system-linux"

[!INCLUDE [Bash - remove Istio CRDs and secrets](includes/servicemesh/istio/uninstall-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-macos"

[!INCLUDE [Bash - remove Istio CRDs and secrets](includes/servicemesh/istio/uninstall-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-windows"

[!INCLUDE [PowerShell - remove Istio CRDs and secrets](includes/servicemesh/istio/uninstall-powershell.md)]

::: zone-end

## <a name="next-steps"></a>Nächste Schritte

In der folgenden Dokumentation wird beschrieben, wie Sie Istio verwenden können, um ein intelligentes Routing für den Rollout eines Canaryrelease bereitzustellen:

> [!div class="nextstepaction"]
> [AKS Istio intelligent routing scenario (Szenario für intelligentes Routing mit AKS Istio)][istio-scenario-routing]

Um mehr über Installations- und Konfigurationsoptionen für Istio zu erfahren, beachten Sie die folgenden offiziellen Istio-Artikel:

- [Istio – Helm installation guide (Istio – Installationshandbuch für Helm)][istio-install-helm]
- [Istio – Helm installation options (Istio – Installationsoptionen für Helm)][istio-install-helm-options]

Weitere Szenarien finden Sie hier:

- [Istio-Anwendungsbeispiel: BookInfo][istio-bookinfo-example]

Um zu erfahren, wie Sie Ihre AKS-Anwendung mit Application Insights und Istio überwachen können, lesen Sie die folgende Dokumentation zu Azure Monitor:

- [Anwendungsüberwachung ohne Instrumentierung für gehostete Kubernetes-Anwendungen][app-insights]

<!-- LINKS - external -->
[istio]: https://istio.io
[helm]: https://helm.sh

[istio-faq]: https://istio.io/faq/general/
[istio-docs-concepts]: https://istio.io/docs/concepts/what-is-istio/
[istio-github]: https://github.com/istio/istio
[istio-github-releases]: https://github.com/istio/istio/releases
[istio-release-notes]: https://istio.io/news/
[istio-install-download]: https://istio.io/docs/setup/kubernetes/download-release/
[istio-install-helm]: https://istio.io/docs/setup/install/helm/
[istio-install-helm-options]: https://istio.io/docs/reference/config/installation-options/
[istio-bookinfo-example]: https://istio.io/docs/examples/bookinfo/

[istio-feature-stages]: https://istio.io/about/feature-stages/
[istio-feature-sds]: https://istio.io/docs/tasks/security/auth-sds/
[istio-feature-cni]: https://istio.io/docs/setup/additional-setup/cni/

[install-wsl]: https://docs.microsoft.com/windows/wsl/install-win10

[kubernetes-feature-sa-projected-volume]: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/#service-account-token-volume-projection
[kubernetes-crd]: https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/#customresourcedefinitions
[kubernetes-jobs]: https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/
[kubernetes-secrets]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-node-selectors]: https://docs.microsoft.com/en-us/azure/aks/concepts-clusters-workloads#node-selectors
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubectl-port-forward]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#port-forward

[grafana]: https://grafana.com/
[prometheus]: https://prometheus.io/
[jaeger]: https://www.jaegertracing.io/
[kiali]: https://www.kiali.io/
[envoy]: https://www.envoyproxy.io/

[app-insights]: https://docs.microsoft.com/azure/azure-monitor/app/kubernetes

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
[istio-scenario-routing]: ./servicemesh-istio-scenario-routing.md
[helm-install]: ./kubernetes-helm.md
