---
title: Installieren von Hashicorp Consul in Azure Kubernetes Service (AKS)
description: Hier erfahren Sie, wie Sie Consul installieren und zum Erstellen eines Dienstmeshs in einem AKS-Cluster (Azure Kubernetes Service) verwenden.
services: container-service
author: dstrebel
ms.service: container-service
ms.topic: article
ms.date: 06/19/2019
ms.author: dastrebe
ms.openlocfilehash: 7acd2533079499091427c7e63741b9c587ee29e5
ms.sourcegitcommit: 7a6d8e841a12052f1ddfe483d1c9b313f21ae9e6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2019
ms.locfileid: "70189079"
---
# <a name="install-and-use-consul-connect-in-azure-kubernetes-service-aks"></a>Installieren und Verwenden von Consul Connect in Azure Kubernetes Service (AKS)

[Consul][consul-github] ist ein Open-Source-Dienstmesh, das eine Reihe wichtiger Funktionen für Microservices in einem Kubernetes-Cluster bereitstellt. Zu diesen Features zählen Dienstermittlung, Integritätsüberprüfung, Dienstsegmentierung und Einblicke. Weitere Informationen zu Consul finden Sie in der offiziellen Dokumentation [What is Consul?][consul-docs-concepts] (Was ist Consul?).

In diesem Artikel wird gezeigt, wie Sie Consul installieren. Die Konsul-Komponenten werden in einem Kubernetes-Cluster unter AKS installiert.

> [!NOTE]
> Diese Anweisungen beziehen sich auf die Consul-Version `1.5`.
>
> Die Consul-Releases mit der Version `1.5.x` wurden vom Hashicorp-Team mit den Kubernetes-Versionen `1.12`, `1.14` und `1.14` getestet. Weitere Consul-Versionen finden Sie auf [GitHub][consul-github-releases]. Informationen zu den Releases finden Sie in den [Consul-Versionshinweisen][consul-release-notes].

In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Installieren der Consul-Komponenten in AKS
> * Überprüfen der Consul-Installation
> * Deinstallieren von Consul in AKS

## <a name="before-you-begin"></a>Voraussetzungen

Bei den in diesem Artikel beschriebenen Schritten wird vorausgesetzt, dass Sie einen AKS-Cluster (Kubernetes `1.11` und höher, RBAC aktiviert) erstellt und eine `kubectl`-Verbindung mit dem Cluster hergestellt haben. Wenn Sie Hilfe bei einem dieser Elemente benötigen, beachten Sie das Dokument [AKS-Schnellstart][aks-quickstart].

Sie benötigen [Helm][helm], um diese Schritte ausführen und Consul installieren zu können. Es wird empfohlen, im Cluster Version `2.12.2` oder höher ordnungsgemäß zu installieren und zu konfigurieren. Wenn Sie Hilfe bei der Installation von Helm benötigen, beachten Sie den [AKS Helm-Installationsleitfaden][helm-install]. Alle Consul-Pods müssen auch für die Ausführung auf Linux-Knoten geplant werden.

Im vorliegenden Artikel wird die Consul-Installationsanleitung in mehrere Einzelschritte untergliedert. Das Endergebnis hat dieselbe Struktur wie die offizielle [Anleitung][consul-install-k8] für die Consul-Installation.

### <a name="install-the-consul-components-on-aks"></a>Installieren der Consul-Komponenten in AKS

Als Erstes klonen wir das offizielle GitHub-Repository für Hashicorp Consul in Kubernetes.

```bash
git clone https://github.com/hashicorp/consul-helm.git
cd consul-helm
```

Danach müssen wir eine benutzerdefinierte Helm-Wertedatei für die Consul-Installation erstellen. Erstellen Sie vor der Installation von Consul die folgende benutzerdefinierte Wertedatei:

```bash
vim consul-values.yaml
```

Kopieren Sie anschließend die folgenden Werte, und fügen Sie sie in die Datei „consul-values.yaml“ ein.

```yaml
# Sets datacenter name and version Consul to use
global:
  datacenter: dc1
  image: "consul:1.5.2"

# Enables proxies to be injected into pods
connectInject:
  enabled: true

# Enables UI on ClusterIP
ui:
  service:
    type: "ClusterIP"

# Enables GRCP that is required for connectInject
client:
  enabled: true
  grpc: true

# Sets replicase to 3 for HA
server:
  replicas: 3
  bootstrapExpect: 1
  disruptionBudget:
    enabled: true
    maxUnavailable: 1

# Syncs Kubernetes service discovery with Consul
syncCatalog:
  enabled: true
```

Nachdem wir nun über die benutzerdefinierte Wertedatei verfügen, können wir Consul in unserem AKS-Cluster installieren.

Bash

```bash
helm install -f consul-values.yaml --name consul --namespace consul .
```
Das Helm-Chart `Consul` stellt eine Reihe von Objekten bereit. Eine Liste der Ausgabe Ihres Befehls `helm install` finden Sie weiter oben. Die Bereitstellung der Consul-Komponenten dauert je nach Clusterumgebung zwischen vier und fünf Minuten.

> [!NOTE]
> Alle Consul-Pods müssen für die Ausführung auf Linux-Knoten geplant werden. Wenn Sie in Ihrem Cluster neben Linux-Knotenpools auch über Windows Server-Knotenpools verfügen, vergewissern Sie sich, dass alle Consul-Pods für die Ausführung auf Linux-Knoten geplant wurden.

Consul ist nun in Ihrem AKS-Cluster bereitgestellt. Im nächsten Abschnitt überprüfen wir die Consul-Installation, um uns zu vergewissern, dass die Bereitstellung von Consul erfolgreich war.

## <a name="validate-the-consul-installation"></a>Überprüfen der Consul-Installation

Vergewissern Sie sich zunächst, dass die erwarteten Dienste erstellt wurden. Verwenden Sie den Befehl [Kubectl get svc][kubectl-get], um die ausgeführten Dienste anzuzeigen. Fragen Sie den Namespace `consul` ab, in dem die Consul-Komponenten durch das Helm-Chart `consul` installiert wurden:

```console
kubectl get svc --namespace consul
```

Die folgende Beispielausgabe zeigt die Dienste, die jetzt ausgeführt werden sollten:

```console
NAME                                 TYPE           CLUSTER-IP     EXTERNAL-IP             PORT(S)                                                                   AGE
consul                               ExternalName   <none>         consul.service.consul   <none>                                                                    38m
consul-consul-connect-injector-svc   ClusterIP      10.0.89.113    <none>                  443/TCP                                                                   40m
consul-consul-dns                    ClusterIP      10.0.166.82    <none>                  53/TCP,53/UDP                                                             40m
consul-consul-server                 ClusterIP      None           <none>                  8500/TCP,8301/TCP,8301/UDP,8302/TCP,8302/UDP,8300/TCP,8600/TCP,8600/UDP   40m
consul-consul-ui                     ClusterIP      10.0.117.164   <none>                  80/TCP                                                                    40m
```

Vergewissern Sie sich als Nächstes, dass die erforderlichen Pods erstellt wurden. Verwenden Sie den Befehl [kubectl get pods][kubectl-get], und fragen Sie den `consul`-Namespace erneut ab:

```console
kubectl get pods --namespace consul
```

Die folgende Beispielausgabe zeigt die Pods, die ausgeführt werden:

```console
NAME                                                              READY   STATUS    RESTARTS   AGE
consul-consul-7cc9v                                               1/1     Running   0          37m
consul-consul-7klg7                                               1/1     Running   0          37m
consul-consul-connect-injector-webhook-deployment-57f88df8hgmfs   1/1     Running   0          37m
consul-consul-lq8qb                                               1/1     Running   0          37m
consul-consul-server-0                                            1/1     Running   0          37m
consul-consul-server-1                                            1/1     Running   0          36m
consul-consul-server-2                                            1/1     Running   0          36m
consul-consul-sync-catalog-7cf7d5bfff-jjbjv                       1/1     Running   2          37m
```

 Alle Pods müssen den Status `Running` haben. Wenn Ihre Pods nicht diesen Status haben, warten Sie ein oder zwei Minuten, bis dies der Fall ist. Wenn einer der Pods ein Problem meldet, verwenden Sie den Befehl [kubectl describe pod][kubectl-describe], um die Ausgabe und den Status des betreffenden Pods zu überprüfen.

## <a name="accessing-the-consul-ui"></a>Zugreifen auf die Consul-Benutzeroberfläche

Die Consul-Benutzeroberfläche wurde oben im Rahmen des Setups installiert, das eine UI-basierte Konfiguration für Consul ermöglicht. Die Benutzeroberfläche für Consul wird nicht öffentlich über eine externe IP-Adresse verfügbar gemacht. Um auf diese Add-On-Benutzeroberflächen zuzugreifen, verwenden Sie den Befehl [kubectl port-forward][kubectl-port-forward]. Dieser Befehl erstellt eine sichere Verbindung zwischen Ihrem Clientcomputer und dem entsprechenden Pod in Ihrem AKS-Cluster.

```bash
kubectl port-forward -n consul svc/consul-consul-ui 8080:80
```
Sie können nun einen Browser öffnen und zu `http://localhost:8080/ui` navigieren, um die Consul-Benutzeroberfläche zu öffnen. Wenn Sie die Benutzeroberfläche öffnen, sollte Folgendes angezeigt werden:

![Consul-Benutzeroberfläche](./media/consul/consul-ui.png)

## <a name="uninstall-consul-from-aks"></a>Deinstallieren von Consul in AKS

> [!WARNING]
> Das Löschen von Consul in einem laufenden System kann zu Problemen beim Datenverkehr zwischen Ihren Diensten führen. Stellen Sie sicher, dass Sie das System so weit vorbereitet haben, dass es auch ohne Consul ordnungsgemäß funktioniert.

### <a name="remove-consul-components-and-namespace"></a>Entfernen von Consul-Komponenten und -Namespace

Verwenden Sie die folgenden Befehle, um Consul aus Ihrem AKS-Cluster zu entfernen. Der Befehl `helm delete` entfernt das Chart `consul` und der Befehl `kubectl delete ns` den Namespace `consul`.

```bash
helm delete --purge consul
kubectl delete ns consul
```

## <a name="next-steps"></a>Nächste Schritte

Informationen zu weiteren Installations- und Konfigurationsoptionen für Consul finden Sie in den folgenden offiziellen Consul-Artikeln:

- [Consul - Helm installation guide][consul-install-k8] (Consul – Installationshandbuch für Helm)
- [Consul - Helm installation options][consul-install-helm-options] (Consul – Installationsoptionen für Helm)

Anhand der [Consul-Beispielanwendung][consul-app-example] können Sie sich mit weiteren Szenarien beschäftigen.

<!-- LINKS - external -->
[Hashicorp]: https://hashicorp.com
[cosul-github]: https://github.com/hashicorp/consul
[helm]: https://helm.sh

[consul-docs-concepts]: https://www.consul.io/docs/platform/k8s/index.html
[consul-github]: https://github.com/hashicorp/consul
[consul-github-releases]: https://github.com/hashicorp/consul/releases
[consul-release-notes]: https://github.com/hashicorp/consul/blob/master/CHANGELOG.md
[consul-install-download]: https://www.consul.io/downloads.html
[consul-install-k8]: https://www.consul.io/docs/platform/k8s/run.html
[consul-install-helm-options]: https://www.consul.io/docs/platform/k8s/helm.html#configuration-values-
[consul-app-example]: https://github.com/hashicorp/demo-consul-101/tree/master/k8s
[install-wsl]: https://docs.microsoft.com/windows/wsl/install-win10

[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubectl-port-forward]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#port-forward

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
[consul-scenario-mtls]: ./consul-mtls.md
[helm-install]: ./kubernetes-helm.md
