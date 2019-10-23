---
title: Problembehandlung bei allgemeinen Problemen mit Azure Kubernetes Service
description: Erfahren Sie, wie Sie allgemeine Probleme bei der Verwendung von Azure Kubernetes Service (AKS) beheben und lösen können
services: container-service
author: sauryadas
ms.service: container-service
ms.topic: troubleshooting
ms.date: 08/13/2018
ms.author: saudas
ms.openlocfilehash: 2c25069ce5231a1f89027dea69579231f0fe4bcd
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72517079"
---
# <a name="aks-troubleshooting"></a>AKS-Problembehandlung

Beim Erstellen oder Verwalten von Clustern mit Azure Kubernetes Service (AKS) können gelegentlich Probleme auftreten. In diesem Artikel werden einige allgemeine Probleme und Schritte zur Problembehandlung ausführlich erläutert.

## <a name="in-general-where-do-i-find-information-about-debugging-kubernetes-problems"></a>Wo finde ich grundsätzlich Informationen zum Debuggen von Problemen mit Kubernetes?

Lesen Sie den [offiziellen Leitfaden zur Problembehandlung von Kubernetes-Clustern](https://kubernetes.io/docs/tasks/debug-application-cluster/troubleshooting/).
Es gibt auch einen [Leitfaden zur Problembehandlung](https://github.com/feiskyer/kubernetes-handbook/blob/master/en/troubleshooting/index.md), der von einem Microsoft-Techniker veröffentlicht wurde und sich mit der Problembehandlung von Pods, Knoten, Clustern usw. befasst.

## <a name="im-getting-a-quota-exceeded-error-during-creation-or-upgrade-what-should-i-do"></a>Während der Erstellung oder Aktualisierung erhalte ich die Fehlermeldung, dass das Kontingent überschritten wurde. Wie sollte ich vorgehen? 

Sie müssen [Kerne anfordern](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request).

## <a name="what-is-the-maximum-pods-per-node-setting-for-aks"></a>Was ist die maximale Einstellung für die Anzahl von Pods pro Knoten für AKS?

Die maximale Anzahl von Pods pro Knoten ist standardmäßig auf 30 festgelegt, wenn Sie einen AKS-Cluster im Azure-Portal bereitstellen.
Die maximale Anzahl von Pods pro Knoten ist standardmäßig auf 110 festgelegt, wenn Sie einen AKS-Cluster über die Azure-Befehlszeilenschnittstelle bereitstellen. (Stellen Sie sicher, dass Sie die aktuelle Version der Azure-Befehlszeilenschnittstelle verwenden.) Diese Standardeinstellung kann mithilfe des Flags `–-max-pods` im Befehl `az aks create` geändert werden.

## <a name="im-getting-an-insufficientsubnetsize-error-while-deploying-an-aks-cluster-with-advanced-networking-what-should-i-do"></a>Bei der Bereitstellung eines AKS-Clusters mit erweitertem Netzwerk erhalte ich einen insufficientSubnetSize-Fehler. Wie sollte ich vorgehen?

Wenn Azure CNI (erweiterte Netzwerke) verwendet wird, weist AKS auf Basis des unter „Max. Pods pro Knoten“ konfigurierten Werts vorab IP-Adressen zu. Die Anzahl der Knoten in einem AKS-Cluster können zwischen 1 und 110 liegen. Basierend auf dem unter „Max. Pods pro Knoten“ konfigurierten Wert sollte die Subnetzgröße die Summe aus der Anzahl der Knoten mal der Höchstzahl der Pods pro Knoten überschreiten. Die folgende einfache Gleichung zeigt dies:

Subnetzgröße > Anzahl der Knoten im Cluster (unter Berücksichtigung der zukünftigen Skalierungsanforderungen) * Max. Pods pro Knoten.

Weitere Informationen finden Sie unter [Planen der IP-Adressierung für Ihren Cluster](configure-azure-cni.md#plan-ip-addressing-for-your-cluster).

## <a name="my-pod-is-stuck-in-crashloopbackoff-mode-what-should-i-do"></a>Mein Pod ist im Modus CrashLoopBackOff hängen geblieben. Wie sollte ich vorgehen?

Es kann verschiedene Gründe dafür geben, dass der Pod in diesem Modus hängen bleibt. Untersuchen Sie Folgendes:

* Den Pod selbst mithilfe von `kubectl describe pod <pod-name>`
* Die Protokolle mit `kubectl log <pod-name>`

Weitere Informationen zur Behandlung von Podproblemen finden Sie unter [Debuggen von Anwendungen](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-application/#debugging-pods).

## <a name="im-trying-to-enable-rbac-on-an-existing-cluster-how-can-i-do-that"></a>Ich versuche, RBAC in einem vorhandenen Cluster zu aktivieren. Wie gehe ich dazu vor?

Eine Aktivierung der rollenbasierten Zugriffssteuerung (RBAC) in vorhandenen Clustern wird derzeit nicht unterstützt. Sie müssen explizit neue Cluster erstellen. Wenn Sie die Befehlszeilenschnittstelle verwenden, ist RBAC standardmäßig aktiviert. Wenn Sie das AKS-Portal verwenden, ist eine Umschaltfläche zum Aktivieren von RBAC im Erstellungsworkflow verfügbar.

## <a name="i-created-a-cluster-with-rbac-enabled-by-using-either-the-azure-cli-with-defaults-or-the-azure-portal-and-now-i-see-many-warnings-on-the-kubernetes-dashboard-the-dashboard-used-to-work-without-any-warnings-what-should-i-do"></a>Ich habe über die Azure-Befehlszeilenschnittstelle mit Standardwerten oder über das Azure-Portal einen Cluster mit aktivierter RBAC erstellt, und nun werden im Kubernetes-Dashboard zahlreiche Warnungen angezeigt. Das Dashboard hat vorher ohne Warnungen funktioniert. Wie sollte ich vorgehen?

Der Grund für die Warnungen auf dem Dashboard ist, dass der Cluster nun mit RBAC aktiviert wurde und der Zugriff darauf standardmäßig deaktiviert wurde. Dieser Ansatz gilt allgemein als bewährte Methode, da die standardmäßige Offenlegung des Dashboards für alle Benutzer des Clusters zu Sicherheitsrisiken führen kann. Wenn Sie das Dashboard weiterhin aktivieren möchten, befolgen Sie die Schritte in [diesem Blogbeitrag](https://pascalnaber.wordpress.com/2018/06/17/access-dashboard-on-aks-with-rbac-enabled/).

## <a name="i-cant-connect-to-the-dashboard-what-should-i-do"></a>Ich kann keine Verbindung mit dem Dashboard herstellen. Wie sollte ich vorgehen?

Der einfachste Weg, außerhalb des Clusters auf Ihren Dienst zuzugreifen, besteht im Ausführen von `kubectl proxy`, der Proxyanforderungen für Ihren localhost-Port 8001 an den Kubernetes-API-Server sendet. Von dort kann der API-Server eine Proxyverbindung mit Ihrem Dienst herstellen: `http://localhost:8001/api/v1/namespaces/kube-system/services/kubernetes-dashboard/proxy/#!/node?namespace=default`.

Wenn das Kubernetes-Dashboard nicht angezeigt wird, überprüfen Sie, ob der Pod `kube-proxy` im Namespace `kube-system` ausgeführt wird. Wenn der Pod keinen Ausführungsstatus aufweist, müssen Sie ihn löschen. Er wird anschließend neu gestartet.

## <a name="i-cant-get-logs-by-using-kubectl-logs-or-i-cant-connect-to-the-api-server-im-getting-error-from-server-error-dialing-backend-dial-tcp-what-should-i-do"></a>Ich kann Protokolle nicht mithilfe von kubectl-Protokollen abrufen, oder ich kann keine Verbindung mit dem API-Server herstellen. Ich erhalte „Fehler vom Server: Fehler beim Anwählen des Back-Ends: Wählen Sie tcp…“. Wie sollte ich vorgehen?

Stellen Sie sicher, dass die standardmäßige Netzwerksicherheitsgruppe nicht geändert wurde und dass die Ports 22 und 9000 für die Verbindung mit dem API-Server geöffnet sind. Überprüfen Sie, ob der `tunnelfront`-Pod im *kube-system*-Namespace ausgeführt wird, indem Sie den Befehl `kubectl get pods --namespace kube-system` verwenden. Falls nicht, erzwingen Sie das Löschen des Pods. Er wird anschließend neu gestartet.

## <a name="im-trying-to-upgrade-or-scale-and-am-getting-a-message-changing-property-imagereference-is-not-allowed-error-how-do-i-fix-this-problem"></a>Ich möchte ein Upgrade oder eine Skalierung ausführen und erhalte die folgende Fehlermeldung: „Das Ändern der Eigenschaft ‚imageReference‘ ist nicht zulässig.“ Wie kann ich dieses Problem beheben?

Möglicherweise erhalten Sie diese Fehlermeldung, da Sie die Tags in den Agent-Knoten innerhalb des AKS-Clusters geändert haben. Das Ändern und Löschen von Tags und anderen Eigenschaften von Ressourcen in der Ressourcengruppe MC_* kann zu unerwarteten Ergebnissen führen. Durch das Ändern der Ressourcen unter der Gruppe „MC_*“ im AKS-Cluster wird das Servicelevelziel (SLO) unterbrochen.

## <a name="im-receiving-errors-that-my-cluster-is-in-failed-state-and-upgrading-or-scaling-will-not-work-until-it-is-fixed"></a>Ich erhalte Fehler, dass sich mein Cluster in einem fehlerhaften Zustand befindet und ein Upgrade oder eine Skalierung nicht funktioniert, bis der Fehler behoben wurde.

*Diese Unterstützung bei der Problembehandlung wurde von https://aka.ms/aks-cluster-failed weitergeleitet.*

Dieser Fehler tritt auf, wenn Cluster aus mehreren Gründen in einen fehlerhaften Zustand übergehen. Führen Sie die folgenden Schritte aus, um den Fehlerzustand Ihres Clusters zu beheben, bevor Sie den zuvor fehlgeschlagenen Vorgang erneut versuchen:

1. Bis der Cluster den `failed`-Zustand verlassen hat, schlagen `upgrade`- und `scale`-Vorgänge fehl. Häufige Probleme und Lösungen sind unter anderem:
    * Skalieren mit einem **nicht ausreichenden Computekontingent (CRP)** . Um das Problem zu beheben, skalieren Sie zuerst Ihren Cluster wieder auf einen stabilen Zielzustand innerhalb des Kontingents. Befolgen Sie dann diese [Schritte, um eine Erhöhung des Computekontingents anzufordern](../azure-supportability/resource-manager-core-quotas-request.md), bevor Sie versuchen, den Wert erneut über die anfänglichen Kontingentgrenzen hinaus zentral hochzuskalieren.
    * Skalieren eines Clusters mit erweiterter Netzwerkunterstützung und **nicht ausreichenden Subnetzressourcen (Netzwerk)** . Um das Problem zu beheben, skalieren Sie zuerst Ihren Cluster wieder auf einen stabilen Zielzustand innerhalb des Kontingents. Befolgen Sie dann diese [Schritte, um eine Erhöhung des Ressourcenkontingents anzufordern](../azure-resource-manager/resource-manager-quota-errors.md#solution), bevor Sie versuchen, den Wert erneut über die anfänglichen Kontingentgrenzen hinaus zentral hochzuskalieren.
2. Sobald die zugrunde liegende Ursache für den Upgradefehler behoben wurde, sollte sich Ihr Cluster in einem erfolgreichen Zustand befinden. Nachdem ein erfolgreicher Zustand bestätigt wurde, wiederholen Sie den ursprünglichen Vorgang.

## <a name="im-receiving-errors-when-trying-to-upgrade-or-scale-that-state-my-cluster-is-being-currently-being-upgraded-or-has-failed-upgrade"></a>Ich erhalte Fehler, wenn ich versuche, den Zustand meines Clusters zu aktualisieren oder zu skalieren, der gerade aktualisiert wird oder bei dem ein Upgrade fehlgeschlagen ist.

*Diese Unterstützung bei der Problembehandlung wurde von https://aka.ms/aks-pending-upgrade weitergeleitet.*

Aktualisierungs- und Skalierungsvorgänge in einem Cluster mit einem einzelnen Knotenpool oder einem Cluster mit [mehreren Knotenpools](use-multiple-node-pools.md) schließen sich gegenseitig aus. Es ist nicht möglich, einen Cluster oder Knotenpool gleichzeitig zu aktualisieren und zu skalieren. Stattdessen muss jeder Vorgangstyp für die Zielressource abgeschlossen sein, bevor eine neue Anforderung an dieselbe Ressource gerichtet werden kann. Vorgänge sind daher eingeschränkt, wenn aktive Upgrade- oder Skalierungsvorgänge stattfinden oder gestartet wurden und anschließend fehlgeschlagen sind. 

Um das Problem zu diagnostizieren, führen Sie `az aks show -g myResourceGroup -n myAKSCluster -o table` aus, um den detaillierten Status Ihres Clusters abzurufen. Basierend auf dem Ergebnis:

* Wenn ein aktives Upgrade des Clusters durchgeführt wird, warten Sie, bis der Vorgang beendet wurde. Wenn dies erfolgreich war, versuchen Sie den zuvor fehlgeschlagenen Vorgang erneut.
* Wenn das Upgrade des Clusters fehlgeschlagen ist, führen Sie die im voranstehenden Abschnitt beschriebenen Schritte aus.

## <a name="can-i-move-my-cluster-to-a-different-subscription-or-my-subscription-with-my-cluster-to-a-new-tenant"></a>Kann ich meinen Cluster in ein anderes Abonnement verschieben oder mein Abonnement mit meinem Cluster in einen anderen Mandanten?

Wenn Sie Ihren AKS-Cluster in ein anderes Abonnement verschoben haben oder das Abonnement, das den Cluster besitzt, in einen anderen Mandanten, verliert der Cluster seine Funktionalität aufgrund des Verlusts von Rollenzuweisungen und Dienstprinzipalrechten. Aufgrund dieser Einschränkung **unterstützt AKS das Verschieben von Clustern über Abonnements oder Mandanten nicht**.

## <a name="im-receiving-errors-trying-to-use-features-that-require-virtual-machine-scale-sets"></a>Ich erhalte Fehler, wenn ich versuche, Funktionen zu verwenden, die VM-Skalierungsgruppen erfordern

*Diese Unterstützung bei der Problembehandlung wurde von „aka.ms/aks-vmss-enablement“ weitergeleitet.*

Möglicherweise erhalten Sie Fehlermeldungen, die angeben, dass sich Ihr AKS-Cluster nicht in einer VM-Skalierungsgruppe befindet. Beispiel:

**Der AgentPool „Agent-Pool“ hat die Autoskalierung aktiviert, befindet sich aber nicht in einer VM-Skalierungsgruppe.**

Zur Verwendung von Funktionen wie der automatischen Clusterskalierung oder mehrerer Knotenpools müssen AKS-Cluster erstellt werden, die VM-Skalierungsgruppen verwenden. Es werden Fehler zurückgegeben, wenn Sie versuchen Funktionen zu verwenden, die von VM-Skalierungsgruppen abhängig sind, und Sie einen normalen AKS-Cluster ohne VM-Skalierungsgruppen als Ziel verwenden.

Führen Sie die Schritte unter *Voraussetzungen* im entsprechenden Dokument aus, um einen AKS-Cluster korrekt zu erstellen:

* [Verwenden der automatischen Clusterskalierung](cluster-autoscaler.md)
* [Erstellen und Verwenden mehrerer Knotenpools](use-multiple-node-pools.md)
 
## <a name="what-naming-restrictions-are-enforced-for-aks-resources-and-parameters"></a>Welche Benennungseinschränkungen gelten für AKS-Ressourcen und -Parameter?

*Diese Unterstützung bei der Problembehandlung wurde von „aka.ms/aks-naming-rules“ weitergeleitet.*

Benennungseinschränkungen werden sowohl von der Azure-Plattform als auch AKS implementiert. Verletzt ein Ressourcenname oder Parameter eine dieser Einschränkungen, wird ein Fehler zurückgegeben, der Sie auffordert, eine andere Eingabe zu machen. Es gelten die folgenden allgemeinen Benennungsrichtlinien:

* Der Name der *MC_* -Ressourcengruppe in AKS ist eine Kombination aus Ressourcengruppenname und Ressourcenname. Die automatisch generierte Syntax von `MC_resourceGroupName_resourceName_AzureRegion` darf nicht mehr als 80 Zeichen umfassen. Kürzen Sie bei Bedarf Ihren Ressourcengruppennamen oder AKS-Clusternamen.
* Das *dnsPrefix* muss mit alphanumerischen Werten beginnen und enden. Gültige Zeichen sind alphanumerische Werte und Bindestriche (-). Das *dnsPrefix* darf keine Sonderzeichen wie z.B. einen Punkt (.) enthalten.

## <a name="im-receiving-errors-when-trying-to-create-update-scale-delete-or-upgrade-cluster-that-operation-is-not-allowed-as-another-operation-is-in-progress"></a>Wenn ich versuche, einen Cluster zu erstellen, zu aktualisieren, zu skalieren, zu löschen oder zu aktualisieren, erhalte ich die Fehlermeldung, dass dieser Vorgang nicht erlaubt sei, da ein anderer Vorgang ausgeführt werde.

*Diese Unterstützung bei der Problembehandlung wurde von „aka.ms/aks-pending-operations“ weitergeleitet*

Clustervorgänge sind nur eingeschränkt möglich, wenn ein vorheriger Vorgang noch im Gange ist. Um den detaillierten Status Ihres Clusters abzurufen, verwenden Sie den Befehl `az aks show -g myResourceGroup -n myAKSCluster -o table`. Verwenden Sie bei Bedarf Ihre eigene Ressourcengruppe und Ihren Namen für den AKS-Cluster.

Basierend auf der Ausgabe des Status des Clusters:

* Wenn sich der Cluster einem anderen Bereitstellungsstatus als *Erfolgreich* oder *Fehler* befindet, warten Sie, bis der Vorgang (*Upgrade/Aktualisieren/Erstellen/Skalieren/Löschen/Migrieren*) abgeschlossen ist. Sobald der vorherige Vorgang abgeschlossen ist, wiederholen Sie Ihren letzten Clustervorgang.

* Wenn im Cluster ein fehlerhaftes Upgrade erfolgt, befolgen Sie die Anweisungen unter [Ich erhalte Fehler, dass sich mein Cluster in einem fehlerhaften Zustand befindet und ein Upgrade oder eine Skalierung nicht funktioniert, bis der Fehler behoben wurde](#im-receiving-errors-that-my-cluster-is-in-failed-state-and-upgrading-or-scaling-will-not-work-until-it-is-fixed).

## <a name="im-receiving-errors-that-my-service-principal-was-not-found-when-i-try-to-create-a-new-cluster-without-passing-in-an-existing-one"></a>Ich erhalte Fehler, dass der Dienstprinzipal nicht gefunden wurde, wenn ich versuche, einen neuen Cluster zu erstellen, ohne einen vorhandenen Cluster zu übergeben.

Beim Erstellen eines AKS-Clusters ist ein Dienstprinzipal erforderlich, um Ressourcen in Ihrem Auftrag zu erstellen. AKS bietet die Möglichkeit, zum Zeitpunkt der Clustererstellung einen neuen Cluster zu erstellen. Dazu ist es jedoch erforderlich, dass Azure Active Directory den neuen Dienstprinzipal in einem angemessenen Zeitraum vollständig weitergibt, damit der Cluster erfolgreich erstellt werden kann. Wenn diese Weitergabe zu lange dauert, schlägt die Überprüfung zum Erstellen des Clusters fehl, da kein verfügbarer Dienstprinzipal dafür gefunden werden kann. 

Verwenden Sie in diesem Fall die folgenden Problemumgehungen:
1. Verwenden Sie einen vorhandenen Dienstprinzipal, der bereits über Regionen weitergegeben wurde und zum Zeitpunkt der Clustererstellung für die Übergabe an AKS vorhanden ist.
2. Bei Verwendung von Automatisierungsskripts fügen Sie Zeitverzögerungen zwischen der Erstellung des Dienstprinzipals und der Erstellung des AKS-Clusters ein.
3. Wenn Sie das Azure-Portal verwenden, kehren Sie während der Erstellung zu den Clustereinstellungen zurück, und wiederholen Sie die Überprüfung nach einigen Minuten.

## <a name="im-receiving-errors-after-restricting-my-egress-traffic"></a>Ich erhalte Fehler, nachdem mein ausgehender Datenverkehr eingeschränkt wurde.

Wenn Sie von einem AKS-Cluster ausgehenden Datenverkehr einschränken, gibt es für AKS [erforderliche und optionale empfohlene](limit-egress-traffic.md) Regeln für ausgehende Ports/das Netzwerk sowie für den FQDN/die Anwendung. Wenn Ihre Einstellungen mit einer dieser Regeln in Konflikt stehen, können Sie bestimmte `kubectl`-Befehle möglicherweise nicht ausführen. Beim Erstellen eines AKS-Clusters werden eventuell auch Fehler angezeigt.

Stellen Sie sicher, dass Ihre Einstellungen nicht mit den erforderlichen oder optionalen empfohlenen Regeln für ausgehende Ports/das Netzwerk sowie für den FQDN/die Anwendung in Konflikt stehen.

## <a name="azure-storage-and-aks-troubleshooting"></a>Problembehandlung für Azure Storage und AKS

### <a name="what-are-the-recommended-stable-versions-of-kubernetes-for-azure-disk"></a>Welche stabilen Versionen von Kubernetes werden für Azure Disk empfohlen? 

| Kubernetes-Version | Empfohlene Version |
| -- | :--: |
| 1.12 | 1.12.9 oder höher |
| 1.13 | 1.13.6 oder höher |
| 1,14 | 1.14.2 oder höher |


### <a name="what-versions-of-kubernetes-have-azure-disk-support-on-the-sovereign-cloud"></a>Welche Versionen von Kubernetes haben Azure Disk-Unterstützung in Sovereign Cloud?

| Kubernetes-Version | Empfohlene Version |
| -- | :--: |
| 1.12 | 1.12.0 oder höher |
| 1.13 | 1.13.0 oder höher |
| 1,14 | 1.14.0 oder höher |


### <a name="waitforattach-failed-for-azure-disk-parsing-devdiskazurescsi1lun1-invalid-syntax"></a>Fehler bei WaitForAttach für Azure Disk: Analyse von „/dev/disk/azure/scsi1/lun1“: ungültige Syntax

In Kubernetes, Version 1.10, kann „MountVolume.WaitForAttach“ bei einer erneuten Bereitstellung von Azure Disk fehlschlagen.

Unter Linux wird möglicherweise ein Fehler „Falsches DevicePath-Format“ angezeigt. Beispiel:

```console
MountVolume.WaitForAttach failed for volume "pvc-f1562ecb-3e5f-11e8-ab6b-000d3af9f967" : azureDisk - Wait for attach expect device path as a lun number, instead got: /dev/disk/azure/scsi1/lun1 (strconv.Atoi: parsing "/dev/disk/azure/scsi1/lun1": invalid syntax)
  Warning  FailedMount             1m (x10 over 21m)   kubelet, k8s-agentpool-66825246-0  Unable to mount volumes for pod
```

Unter Windows wird möglicherweise ein Fehler „Falsche DevicePath(LUN)-Nummer“ angezeigt. Beispiel:

```console
Warning  FailedMount             1m    kubelet, 15282k8s9010    MountVolume.WaitForAttach failed for volume "disk01" : azureDisk - WaitForAttach failed within timeout node (15282k8s9010) diskId:(andy-mghyb
1102-dynamic-pvc-6c526c51-4a18-11e8-ab5c-000d3af7b38e) lun:(4)
```

Dieses Problem wurde in den folgenden Versionen von Kubernetes behoben:

| Kubernetes-Version | Korrigierte Version |
| -- | :--: |
| 1.10 | 1.10.2 oder höher |
| 1.11 | 1.11.0 oder höher |
| 1.12 und höher | – |

### <a name="failure-when-setting-uid-and-gid-in-mountoptions-for-azure-disk"></a>Fehler beim Festlegen von UID und GID in „mountOptions“ für Azure Disk.

Azure Disk verwendet standardmäßig das Dateisystem „ext4,xfs“ sowie mountOptions wie „uid=x, gid=x“ können zum Zeitpunkt des Einbindens nicht festgelegt werden. Wenn Sie z. B. versucht haben, die mountOptions „uid=999,gid=999“ festzulegen, würde ein Fehler angezeigt wie:

```console
Warning  FailedMount             63s                  kubelet, aks-nodepool1-29460110-0  MountVolume.MountDevice failed for volume "pvc-d783d0e4-85a1-11e9-8a90-369885447933" : azureDisk - mountDevice:FormatAndMount failed with mount failed: exit status 32
Mounting command: systemd-run
Mounting arguments: --description=Kubernetes transient mount for /var/lib/kubelet/plugins/kubernetes.io/azure-disk/mounts/m436970985 --scope -- mount -t xfs -o dir_mode=0777,file_mode=0777,uid=1000,gid=1000,defaults /dev/disk/azure/scsi1/lun2 /var/lib/kubelet/plugins/kubernetes.io/azure-disk/mounts/m436970985
Output: Running scope as unit run-rb21966413ab449b3a242ae9b0fbc9398.scope.
mount: wrong fs type, bad option, bad superblock on /dev/sde,
       missing codepage or helper program, or other error
```

Führen Sie einen der folgenden Schritte aus, um das Problem zu minimieren:

* [Konfigurieren Sie den Sicherheitskontext für eine Pod](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/), indem Sie die UID in „runAsUser“ und die GID in „fsGroup“ festlegen. Mit der folgenden Einstellung wird z. B. die Pod-Ausführung auf „root“ festgelegt, sodass alle Dateien darauf zugreifen können:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: security-context-demo
spec:
  securityContext:
    runAsUser: 0
    fsGroup: 0
```

  >[!NOTE]
  > Da GID und UID standardmäßig als „root“ oder „0“ eingebunden werden. Wenn GID oder UID auf einen anderen Wert als „root“ festgelegt werden (z. B. 1000), verwendet Kubernetes `chown`, um alle Verzeichnisse und Dateien unterhalb dieses Datenträgers zu ändern. Dieser Vorgang kann zeitaufwändig sein und das Einbinden des Datenträgers stark verlangsamen.

* Verwenden Sie `chown` in „initContainers“, um GID und UID festzulegen. Beispiel:

```yaml
initContainers:
- name: volume-mount
  image: busybox
  command: ["sh", "-c", "chown -R 100:100 /data"]
  volumeMounts:
  - name: <your data volume>
    mountPath: /data
```

### <a name="error-when-deleting-azure-disk-persistentvolumeclaim-in-use-by-a-pod"></a>Fehler beim Löschen des „PersistentVolumeClaim“ von Azure Disk, das von einem Pod verwendet wird.

Wenn Sie versuchen, einen „PersistentVolumeClaim“ von Azure Disk zu löschen, der von einem Pod verwendet wird, wird möglicherweise ein Fehler angezeigt. Beispiel:

```console
$ kubectl describe pv pvc-d8eebc1d-74d3-11e8-902b-e22b71bb1c06
...
Message:         disk.DisksClient#Delete: Failure responding to request: StatusCode=409 -- Original Error: autorest/azure: Service returned an error. Status=409 Code="OperationNotAllowed" Message="Disk kubernetes-dynamic-pvc-d8eebc1d-74d3-11e8-902b-e22b71bb1c06 is attached to VM /subscriptions/{subs-id}/resourceGroups/MC_markito-aks-pvc_markito-aks-pvc_westus/providers/Microsoft.Compute/virtualMachines/aks-agentpool-25259074-0."
```

In Kubernetes, Version 1.10 und höher, gibt es eine standardmäßig aktivierte Schutzfunktion für „PersistentVolumeClaim“, um diesen Fehler zu vermeiden. Wenn Sie eine Version von Kubernetes verwenden, die keine Korrektur für dieses Problem enthält, können Sie dieses Problem minimieren, indem Sie den Pod, der den „PersistentVolumeClaim“ verwendet, löschen, bevor Sie den „PersistentVolumeClaim“ löschen.


### <a name="error-cannot-find-lun-for-disk-when-attaching-a-disk-to-a-node"></a>Fehler „LUN für Datenträger wurde nicht gefunden“ beim Anfügen eines Datenträgers an einen Knoten.

Beim Anfügen eines Datenträgers an einen Knoten wird möglicherweise der folgende Fehler angezeigt:

```console
MountVolume.WaitForAttach failed for volume "pvc-12b458f4-c23f-11e8-8d27-46799c22b7c6" : Cannot find Lun for disk kubernetes-dynamic-pvc-12b458f4-c23f-11e8-8d27-46799c22b7c6
```

Dieses Problem wurde in den folgenden Versionen von Kubernetes behoben:

| Kubernetes-Version | Korrigierte Version |
| -- | :--: |
| 1.10 | 1.10.10 oder höher |
| 1.11 | 1.11.5 oder höher |
| 1.12 | 1.12.3 oder höher |
| 1.13 | 1.13.0 oder höher |
| 1.14 und höher | – |

Wenn Sie eine Version von Kubernetes verwenden, die keine Korrektur für dieses Problem enthält, können Sie dieses Problem minimieren, indem Sie mehrere Minuten warten und es dann noch mal versuchen.

### <a name="azure-disk-attachdetach-failure-mount-issues-or-io-errors-during-multiple-attachdetach-operations"></a>Fehler beim Anfügen/Trennen von Azure Disk, Fehler beim Einbinden oder E/A-Fehler während mehrerer Anfügen/Trennen-Vorgänge.

Beginnend mit der Kubernetes-Version 1.9.2, treten möglicherweise die folgenden Datenträgerprobleme wegen eines fehlerhaften VM-Caches auf, wenn mehrere Anfügen/Trennen-Vorgänge parallel ausgeführt werden:

* Fehler beim Anfügen/Trennen von Datenträgern
* E/A-Datenträgerfehler
* Unerwartete Trennung eines Datenträgers von VM
* Virtueller Computer gelangt während der Ausführung in einen Fehlerzustand wegen des Anfügens eines nicht vorhandenen Datenträgers.

Dieses Problem wurde in den folgenden Versionen von Kubernetes behoben:

| Kubernetes-Version | Korrigierte Version |
| -- | :--: |
| 1.10 | 1.10.12 oder höher |
| 1.11 | 1.11.6 oder höher |
| 1.12 | 1.12.4 oder höher |
| 1.13 | 1.13.0 oder höher |
| 1.14 und höher | – |

Wenn Sie eine Version von Kubernetes verwenden, die keine Korrektur für dieses Problem enthält, können Sie dieses Problem minimieren, indem Sie Folgendes ausprobieren:

* Wenn ein Datenträger vorhanden ist, der schon länger auf seine Trennung wartet, versuchen Sie, den Datenträger manuell zu trennen.

### <a name="azure-disk-waiting-to-detach-indefinitely"></a>Azure Disk wartet unendlich auf Trennung.

Wenn beim ersten Versuch eines Trennvorgangs für eine Azure Disk ein Fehler auftritt, wird dieser Trennvorgang in manchen Fällen nicht wiederholt, und der Datenträger bleibt an die ursprünglichen Knoten-VM angefügt. Dieser Fehler kann auftreten, wenn ein Datenträger von einem Knoten zu einem anderen verschoben wird. Beispiel:

```console
[Warning] AttachVolume.Attach failed for volume “pvc-7b7976d7-3a46-11e9-93d5-dee1946e6ce9” : Attach volume “kubernetes-dynamic-pvc-7b7976d7-3a46-11e9-93d5-dee1946e6ce9" to instance “/subscriptions/XXX/resourceGroups/XXX/providers/Microsoft.Compute/virtualMachines/aks-agentpool-57634498-0” failed with compute.VirtualMachinesClient#CreateOrUpdate: Failure sending request: StatusCode=0 -- Original Error: autorest/azure: Service returned an error. Status= Code=“ConflictingUserInput” Message=“Disk ‘/subscriptions/XXX/resourceGroups/XXX/providers/Microsoft.Compute/disks/kubernetes-dynamic-pvc-7b7976d7-3a46-11e9-93d5-dee1946e6ce9’ cannot be attached as the disk is already owned by VM ‘/subscriptions/XXX/resourceGroups/XXX/providers/Microsoft.Compute/virtualMachines/aks-agentpool-57634498-1’.”
```

Dieses Problem wurde in den folgenden Versionen von Kubernetes behoben:

| Kubernetes-Version | Korrigierte Version |
| -- | :--: |
| 1.11 | 1.11.9 oder höher |
| 1.12 | 1.12.7 oder höher |
| 1.13 | 1.13.4 oder höher |
| 1.14 und höher | – |

Wenn Sie eine Version von Kubernetes verwenden, die keine Korrektur für dieses Problem enthält, können Sie dieses Problem minimieren, indem Sie den Datenträger manuell trennen.

### <a name="azure-disk-detach-failure-leading-to-potential-race-condition-issue-and-invalid-data-disk-list"></a>Azure Disk-Trennungsfehler, der zu einem potenziellen Racebedingungs-Problem und einer ungültigen Datenträgerauflistung führt.

Wenn das Trennen einer Azure Disk fehlschlägt, wird bis zu sechs Mal erneut versucht, den Datenträger mithilfe von exponentiellem Backout zu trennen. Außerdem wird die Datenträgerauflistung auf Knotenebene für ca. 3 Minuten gesperrt. Wenn die Datenträgerauflistung während dieses Zeitraums manuell aktualisiert wird (z. B. ein manueller Anfüge- oder Trennvorgang), führt dies dazu, dass die auf Knotenebene gesperrte Datenträgerauflistung veraltet ist und zu Instabilität auf der Knoten-VM führt.

Dieses Problem wurde in den folgenden Versionen von Kubernetes behoben:

| Kubernetes-Version | Korrigierte Version |
| -- | :--: |
| 1.12 | 1.12.9 oder höher |
| 1.13 | 1.13.6 oder höher |
| 1,14 | 1.14.2 oder höher |
| 1.15 und höher | – |

Wenn Sie eine Version von Kubernetes verwenden, die keine Korrektur für dieses Problem enthält, und Ihre Knoten-VM besitzt eine veraltete Datenträgerauflistung, können Sie das Problem minimieren, indem Sie alle nicht vorhandenen Datenträger von der VM mit einem einzelnen Massenvorgang trennen. **Trennen einzelner, nicht vorhandener Datenträger kann fehlschlagen.**


### <a name="large-number-of-azure-disks-causes-slow-attachdetach"></a>Eine große Anzahl von Azure Disks führt zu langsamem Anfügen/Trennen.

Wenn die Anzahl der an eine Knoten-VM angefügten Azure-Datenträger größer als 10 ist, können Anfüge- und Trennvorgänge langsam sein. Die ist ein bekanntes Problem, und zurzeit gibt es keine Problemumgehungen dafür.

### <a name="azure-disk-detach-failure-leading-to-potential-node-vm-in-failed-state"></a>Fehler beim Trennen von Azure Disks führt zu potenziellem Fehlerzustand von Knoten-VM.

In einigen Randfällen kann es zu einem teilweisen Fehlschlagen des Trennens von Azure Disk kommen, wodurch die Knoten-VM in einem Fehlerzustand verbleibt.

Dieses Problem wurde in den folgenden Versionen von Kubernetes behoben:

| Kubernetes-Version | Korrigierte Version |
| -- | :--: |
| 1.12 | 1.12.10 oder höher |
| 1.13 | 1.13.8 oder höher |
| 1,14 | 1.14.4 oder höher |
| 1.15 und höher | – |

Wenn Sie eine Version von Kubernetes verwenden, die keine Korrektur für dieses Problem enthält, und Ihre Knoten-VM ist in einem Fehlerzustand, können Sie das Problem minimieren, indem Sie den VM-Status mit einer der folgenden Methoden manuell aktualisieren:

* Für einen auf Verfügbarkeitsgruppen basierenden Cluster:
    ```console
    az vm update -n <VM_NAME> -g <RESOURCE_GROUP_NAME>
    ```

* Für einen VMSS-basierenden Cluster:
    ```console
    az vmss update-instances -g <RESOURCE_GROUP_NAME> --name <VMSS_NAME> --instance-id <ID>
    ```

## <a name="azure-files-and-aks-troubleshooting"></a>Problembehandlung für Azure Files und AKS

### <a name="what-are-the-recommended-stable-versions-of-kubernetes-for-azure-files"></a>Welche stabilen Versionen von Kubernetes werden für Azure Files empfohlen?
 
| Kubernetes-Version | Empfohlene Version |
| -- | :--: |
| 1.12 | 1.12.6 oder höher |
| 1.13 | 1.13.4 oder höher |
| 1,14 | 1.14.0 oder höher |

### <a name="what-versions-of-kubernetes-have-azure-files-support-on-the-sovereign-cloud"></a>Welche Versionen von Kubernetes haben Azure Files-Unterstützung in Sovereign Cloud?

| Kubernetes-Version | Empfohlene Version |
| -- | :--: |
| 1.12 | 1.12.0 oder höher |
| 1.13 | 1.13.0 oder höher |
| 1,14 | 1.14.0 oder höher |

### <a name="what-are-the-default-mountoptions-when-using-azure-files"></a>Wie lauten die standardmäßigen „mountOptions“ bei Verwendung von Azure Files?

Empfohlene Einstellungen:

| Kubernetes-Version | fileMode- und dirMode-Wert|
| -- | :--: |
| 1.12.0 – 1.12.1 | 0755 |
| 1.12.2 und höher | 0777 |

Wenn Sie einen Cluster mit Kubernetes-Version 1.8.5 oder höher verwenden und das persistente Volume dynamisch mit einer Speicherklasse erstellen, können Einbindungsoptionen im Speicherklassenobjekt angegeben werden. Im folgenden Beispiel wird *0777* festgelegt:

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: azurefile
provisioner: kubernetes.io/azure-file
mountOptions:
  - dir_mode=0777
  - file_mode=0777
  - uid=1000
  - gid=1000
  - mfsymlinks
  - nobrl
  - cache=none
parameters:
  skuName: Standard_LRS
```

Einige zusätzliche nützliche *mountOptions*-Einstellungen:

* *mfsymlinks* aktiviert Unterstützung bei der Einbindung symbolischer Links (CIFS) in Azure Files.
* *nobrl* verhindert das Senden von Anforderungen für Byte-Bereichssperrungen an den Server. Diese Einstellung ist für bestimmte Anwendungen erforderlich, die obligatorische Byte-Bereichssperren im CIFS-Stil verletzen. Die meisten CIFS-Server unterstützen noch keine Anforderung von Byte Bereichssperrenempfehlungen. Wenn Sie *nobrl* nicht verwenden, verursachen Anwendungen, die obligatorische Byte-Bereichssperren im CIFS-Stil verletzen, möglicherweise ähnliche Fehlermeldungen wie diese:
    ```console
    Error: SQLITE_BUSY: database is locked
    ```

### <a name="error-could-not-change-permissions-when-using-azure-files"></a>Fehler „Berechtigungen konnten nicht geändert werden“ bei Verwendung von Azure Files.

Bei der Ausführung von PostgreSQL auf dem Azure Files-Plug-In wird möglicherweise ein ähnlicher Fehler wie der folgende angezeigt:

```console
initdb: could not change permissions of directory "/var/lib/postgresql/data": Operation not permitted
fixing permissions on existing directory /var/lib/postgresql/data
```

Dieser Fehler wird vom Azure Files-Plug-In verursacht, wenn es das CISF/SMB-Protokoll verursacht. Bei Verwendung des CIFS/SMB-Protokolls konnten die Datei- und Verzeichnisberechtigungen nach der Bereitstellung nicht geändert werden.

Um dieses Problem zu beheben, verwenden Sie *subPath* zusammen mit dem Azure Disk-Plug-In. 

> [!NOTE] 
> Beim Datenträgertyp „ext3/4“ gibt es ein Verzeichnis „lost+found“, nachdem der Datenträger formatiert wurde.

### <a name="azure-files-has-high-latency-compared-to-azure-disk-when-handling-many-small-files"></a>Bei der Verarbeitung vieler kleiner Dateien kommt es in Azure Files zu hohen Wartezeiten im Vergleich mit Azure Disk.

In manchen Fällen, z. B. bei der Verarbeitung vieler kleiner Dateien, kann es bei der Verwendung von Azure Files im Vergleich zu Azure Disk zu hohen Wartezeiten kommen.

### <a name="error-when-enabling-allow-access-allow-access-from-selected-network-setting-on-storage-account"></a>Fehler beim Aktivieren der Einstellung „Zugriff aus ausgewähltem Netzwerk zulassen“ für das Speicherkonto.

Wenn Sie *Zugriff aus ausgewähltem Netzwerk zulassen* für ein Speicherkonto aktivieren, das für die dynamische Bereitstellung in AKS verwendet wird, erhalten Sie eine Fehlermeldung, wenn AKS eine Dateifreigabe erstellt:

```console
persistentvolume-controller (combined from similar events): Failed to provision volume with StorageClass "azurefile": failed to create share kubernetes-dynamic-pvc-xxx in account xxx: failed to create file share, err: storage: service returned error: StatusCode=403, ErrorCode=AuthorizationFailure, ErrorMessage=This request is not authorized to perform this operation.
```

Dieser Fehler ist darauf zurückzuführen, dass sich der *persistentvolume-controller*von Kubernetes nicht im ausgewählten Netzwerk befindet, wenn*Zugriff aus ausgewähltem Netzwerk zulassen* festgelegt wird.

Sie können das Problem minimieren, indem Sie [statische Bereitstellung mit Azure Files ](azure-files-volume.md) verwenden.

### <a name="azure-files-fails-to-remount-in-windows-pod"></a>Azure Files kann in einem Windows-Pod nicht erneut bereitgestellt werden.

Wenn ein Windows-Pod mit einer Azure Files-Einbindung gelöscht und dann für die Neuerstellung auf demselben Knoten geplant wird, wird diese Einbindung fehlschlagen. Dieser Fehler tritt auf, weil der `New-SmbGlobalMapping`-Befehl fehlschlägt, da die Azure Files-Bereitstellung bereits auf dem Knoten eingebunden ist.

Beispielsweise wird eventuell ungefähr folgende Fehlermeldung angezeigt:

```console
E0118 08:15:52.041014    2112 nestedpendingoperations.go:267] Operation for "\"kubernetes.io/azure-file/42c0ea39-1af9-11e9-8941-000d3af95268-pvc-d7e1b5f9-1af3-11e9-8941-000d3af95268\" (\"42c0ea39-1af9-11e9-8941-000d3af95268\")" failed. No retries permitted until 2019-01-18 08:15:53.0410149 +0000 GMT m=+732.446642701 (durationBeforeRetry 1s). Error: "MountVolume.SetUp failed for volume \"pvc-d7e1b5f9-1af3-11e9-8941-000d3af95268\" (UniqueName: \"kubernetes.io/azure-file/42c0ea39-1af9-11e9-8941-000d3af95268-pvc-d7e1b5f9-1af3-11e9-8941-000d3af95268\") pod \"deployment-azurefile-697f98d559-6zrlf\" (UID: \"42c0ea39-1af9-11e9-8941-000d3af95268\") : azureMount: SmbGlobalMapping failed: exit status 1, only SMB mount is supported now, output: \"New-SmbGlobalMapping : Generic failure \\r\\nAt line:1 char:190\\r\\n+ ... ser, $PWord;New-SmbGlobalMapping -RemotePath $Env:smbremotepath -Cred ...\\r\\n+                 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~\\r\\n    + CategoryInfo          : NotSpecified: (MSFT_SmbGlobalMapping:ROOT/Microsoft/...mbGlobalMapping) [New-SmbGlobalMa \\r\\n   pping], CimException\\r\\n    + FullyQualifiedErrorId : HRESULT 0x80041001,New-SmbGlobalMapping\\r\\n \\r\\n\""
```

Dieses Problem wurde in den folgenden Versionen von Kubernetes behoben:

| Kubernetes-Version | Korrigierte Version |
| -- | :--: |
| 1.12 | 1.12.6 oder höher |
| 1.13 | 1.13.4 oder höher |
| 1.14 und höher | – |

### <a name="azure-files-mount-fails-due-to-storage-account-key-changed"></a>Azure Files-Einbindung schlägt fehl, weil sich der Speicherkontoschlüssel geändert hat.

Wenn Ihr Speicherkontoschlüssel geändert wurde, kommt es möglicherweise zu Azure Files-Einbindungsfehlern.

Sie können das Problem minimieren, indem Sie das Feld *azurestorageaccountkey* manuell mit Ihrem Base64-codierten Speicherkontoschlüssel im Azure File-Geheimnis aktualisieren.

Um Ihren Speicherkontoschlüssel in Base64 zu codieren, können Sie `base64` verwenden. Beispiel:

```console
echo X+ALAAUgMhWHL7QmQ87E1kSfIqLKfgC03Guy7/xk9MyIg2w4Jzqeu60CVw2r/dm6v6E0DWHTnJUEJGVQAoPaBc== | base64
```

Verwenden Sie zum Aktualisieren Ihrer Azure-Geheimnisdatei `kubectl edit secret`. Beispiel:

```console
kubectl edit secret azure-storage-account-{storage-account-name}-secret
```

Nach ein paar Minuten versucht der Agent-Knoten die Azure Files-Einbindung erneut mit dem aktualisierten Speicherschlüssel.
