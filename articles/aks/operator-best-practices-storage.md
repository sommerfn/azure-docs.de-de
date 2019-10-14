---
title: Best Practices für Operatoren – Speicherung in Azure Kubernetes Service (AKS)
description: Lernen Sie die bewährten Methoden für Speicherung, Datenverschlüsselung und Sicherungen in Azure Kubernetes Service (AKS) für Clusteroperatoren kennen.
services: container-service
author: mlearned
ms.service: container-service
ms.topic: conceptual
ms.date: 5/6/2019
ms.author: mlearned
ms.openlocfilehash: 8e5f394987de06feaeb9a635face643eecc97cb9
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72174222"
---
# <a name="best-practices-for-storage-and-backups-in-azure-kubernetes-service-aks"></a>Best Practices für Speicherung und Sicherungen in Azure Kubernetes Service (AKS)

Beim Erstellen und Verwalten von Clustern in Azure Kubernetes Service (AKS) benötigen Ihre Anwendungen oft Speicher. Es ist wichtig, die Leistungsanforderungen und Zugriffsmethoden für Pods zu verstehen, damit Sie Anwendungen den entsprechenden Speicherplatz zur Verfügung stellen können. Die Größe des AKS-Knotens kann sich auf diese Speicheroptionen auswirken. Sie sollten auch Möglichkeiten zur Sicherung und zum Testen des Wiederherstellungsprozesses für angefügte Speicher planen.

Dieser Best Practices-Artikel konzentriert sich auf Überlegungen zur Speicherung für Clusteroperatoren. In diesem Artikel wird Folgendes behandelt:

> [!div class="checklist"]
> * Welche Arten von Speicher verfügbar sind
> * Wie man AKS-Knoten für die Speicherleistung richtig dimensioniert
> * Unterschiede zwischen dynamischer und statischer Bereitstellung von Volumes
> * Möglichkeiten zum Sichern und Schützen Ihrer Datenvolumes

## <a name="choose-the-appropriate-storage-type"></a>Auswählen des geeigneten Speichertyps

**Best Practices-Anleitung**: Grundlegendes zu den Anforderungen Ihrer Anwendung für die Auswahl des richtigen Speichers. Verwenden Sie SSD-basierten Hochleistungsspeicher für Produktionsworkloads. Planen Sie netzwerkbasierten Speicher ein, wenn mehrere gleichzeitige Verbindungen erforderlich sind.

Anwendungen erfordern häufig verschiedene Arten und von Speicher mit unterschiedlichen Geschwindigkeiten. Benötigen Ihre Anwendungen Speicherplatz, der mit einzelnen Pods verbunden ist oder über mehrere Pods freigegeben wird? Ist der Speicher für den schreibgeschützten Zugriff auf Daten oder für das Schreiben großer Mengen strukturierter Daten vorgesehen? Dieser Speicherbedarf bestimmt, welcher Speichertyp am besten geeignet ist.

In der folgenden Tabelle sind die verfügbarer Speichertypen und ihre Fähigkeiten aufgeführt:

| Anwendungsfall | Volume-Plug-In | Lese-/Schreibzugriff, einmal | Schreibgeschützt, mehrfach | Lese-/Schreibzugriff, mehrfach | Unterstützung für Windows Server-Container |
|----------|---------------|-----------------|----------------|-----------------|--------------------|
| Freigegebene Konfiguration       | Azure Files   | Ja | Ja | Ja | Ja |
| Strukturierte App-Daten        | Azure Disks   | Ja | Nein  | Nein  | Ja |
| Unstrukturierte Daten, Dateisystemvorgänge | [BlobFuse (Vorschau)][blobfuse] | Ja | Ja | Ja | Nein |

Die beiden primären Speichertypen, die für Volumes in AKS zur Verfügung stehen, werden durch Azure-Datenträger oder Azure Files gesichert. Um die Sicherheit zu verbessern, verwenden beide Speichertypen standardmäßig Azure-Speicherdienstverschlüsselung (Storage Service Encryption, SSE) zur Verschlüsselung von ruhenden Daten. Festplatten können derzeit nicht mit der Azure Disk Encryption auf AKS-Knotenebene verschlüsselt werden.

Azure Files ist derzeit in der Leistungsstufe „Standard“ verfügbar. Azure-Datenträger sind derzeit in der Leistungsstufe „Standard“ und „Premium“ verfügbar:

- *Premium*-Datenträger werden von Hochleistungs-SSDs gesichert. Für alle Produktionsworkloads werden Premium-Datenträger empfohlen.
- *Standard*-Datenträger werden durch normale rotierende Festplatten (HDDs) gesichert und eignen sich gut für die Archivierung oder für Daten, auf die selten zugegriffen wird.

Um die geeignete Speicherebene auszuwählen, sollten Sie die Anforderungen an Anwendungsleistung und die Zugriffsmuster kennen. Weitere Informationen zur Größe und den Leistungsstufen von Managed Disks finden Sie in der [Übersicht über Managed Disks][managed-disks].

### <a name="create-and-use-storage-classes-to-define-application-needs"></a>Erstellen und Verwenden von Speicherklassen zum Definieren von Anwendungsanforderungen

Der verwendete Speichertyp wird unter Verwendung von Kubernetes-*Speicherklassen* definiert. Die Speicherklasse wird dann in der Pod- oder Bereitstellungsspezifikation referenziert. Durch Kombination dieser Definitionen wird der geeignete Speicher erstellt und mit den Pods verbunden. Weitere Informationen finden Sie unter [Speicherklassen in AKS][aks-concepts-storage-classes].

## <a name="size-the-nodes-for-storage-needs"></a>Größe der Knoten für den Speicherbedarf

**Best Practices-Anleitung**: Jede Knotengröße unterstützt eine maximale Anzahl von Datenträgern. Unterschiedliche Knotengrößen stellen auch verschiedene Mengen an lokalem Speicher und Netzwerkbandbreite bereit. Planen Sie Ihre Anwendungsanforderungen, um die richtige Größe der Knoten bereitzustellen.

AKS-Knoten werden als virtuelle Azure-Computer ausgeführt. Verschiedene Arten und Größen von virtuellen Computern sind verfügbar. Jede VM-Größe stellt eine unterschiedliche Anzahl von Kernressourcen wie CPU und Arbeitsspeicher bereit. Für jede VM-Größe gilt eine maximale Anzahl von Datenträgern, die angefügt werden können. Die Speicherleistung der verschiedenen VM-Größen variiert auch bezüglich der maximalen IOPS (Eingabe-/Ausgabevorgänge pro Sekunde) auf lokalen und angeschlossenen Datenträgern.

Wenn Ihre Anwendungen Azure-Datenträger als Speicherlösung benötigen, planen und wählen Sie eine geeignete Größe der Knoten-VM. Die Menge an CPU und Arbeitsspeicher ist nicht der einzige bei der Auswahl der VM-Größe zu berücksichtigende Faktor. Auch die Speicherkapazitäten sind wichtig. So beinhalten die VM-Größen *Standard_B2ms* und *Standard_DS2_v2* beispielsweise eine ähnliche Menge an CPU- und Arbeitsspeicherressourcen. Ihre potenzielle Speicherleistung ist unterschiedlich, wie die folgende Tabelle zeigt:

| Knotentyp und -größe | vCPU | Arbeitsspeicher (GiB) | Max. Anzahl Datenträger | Maximale Anzahl nicht zwischengespeicherter Datenträger-IOPS | Maximaler nicht zwischengespeicherter Durchsatz (Mbit/s) |
|--------------------|------|--------------|----------------|------------------------|--------------------------------|
| Standard_B2ms      | 2    | 8            | 4              | 1\.920                  | 22,5                           |
| Standard_DS2_v2    | 2    | 7            | 8              | 6\.400                  | 96                             |

Hier ermöglicht die VM-Größe *Standard_DS2_v2* die doppelte Anzahl an angeschlossenen Datenträgern und bietet das Drei- bis Vierfache an IOPS und Datenträgerdurchsatz. Wenn Sie sich nur die wichtigsten Computeressourcen ansehen und die Kosten vergleichen, wählen Sie möglicherweise die VM-Größe *Standard_B2ms* aus und stellen dann eine schlechte Speicherleistung sowie Einschränkungen fest. Arbeiten Sie mit Ihrem Anwendungsentwicklungsteam zusammen, um ihren Speicherkapazitätsbedarf und ihre Leistungsanforderungen zu verstehen. Wählen Sie die geeignete VM-Größe für die AKS-Knoten aus, um ihre Leistungsanforderungen zu erfüllen oder zu überschreiten. Ermitteln Sie regelmäßig die Baseline von Anwendungen, um die VM-Größe bei Bedarf anzupassen.

Weitere Informationen zu verfügbaren VM-Größen finden Sie unter [Größen für virtuelle Linux-Computer in Azure][vm-sizes].

## <a name="dynamically-provision-volumes"></a>Dynamisches Bereitstellen von Volumes

**Best Practices-Anleitung** – Um den Verwaltungsaufwand zu reduzieren und ein Skalieren zu ermöglichen, sollten Sie keine persistenten Volumes statisch erstellen und zuweisen. Verwenden Sie die dynamische Bereitstellung. Definieren Sie in Ihren Speicherklassen die entsprechende Freigaberichtlinie, um die Kosten für nicht benötigten Speicher nach dem Löschen von Pods zu minimieren.

Wenn Sie Speicher an Pods anfügen müssen, verwenden Sie persistente Volumes. Diese persistenten Volumes können manuell oder dynamisch erstellt werden. Die manuelle Erstellung von persistenten Volumes erhöht den Verwaltungsaufwand und beschränkt Ihre Möglichkeiten der Skalierung. Verwenden Sie die dynamische Bereitstellung persistenter Volumes, um die Speicherverwaltung zu vereinfachen und Ihre Anwendungen bei Bedarf erweitern und skalieren zu können.

![Ansprüche auf persistente Volumes in einem Azure Kubernetes Service-Cluster (AKS)](media/concepts-storage/persistent-volume-claims.png)

Mit einem Anspruch auf persistente Volumes (PVC) können Sie bei Bedarf dynamisch Speicher erstellen. Die zugrunde liegenden Azure-Datenträger werden erstellt, wenn sie von Pods angefordert werden. In der Poddefinition fordern Sie an, dass ein Volume erstellt und an einen entworfenen Einbindungspfad angefügt wird.

Informationen zu den Konzepten zum dynamischen Erstellen und Verwenden von Volumes finden Sie unter [Ansprüche auf persistente Volumes][aks-concepts-storage-pvcs].

Um diese Volumes in Aktion zu sehen, schauen Sie sich an, wie Sie ein persistentes Volume mit [Azure-Datenträgern][dynamic-disks] oder [Azure Files][dynamic-files] dynamisch erstellen und verwenden.

Legen Sie als Teil Ihrer Speicherklassendefinitionen die entsprechende *reclaimPolicy* fest. Diese reclaimPolicy steuert das Verhalten der zugrunde liegenden Azure-Speicherressource, wenn die Pods gelöscht werden und das persistente Volume vielleicht nicht mehr benötigt wird. Die zugrunde liegende Speicherressource kann gelöscht oder für die Verwendung mit einem zukünftigen Pod beibehalten werden. „reclaimPolicy“ kann auf *Beibehalten* oder *Löschen* festgelegt werden. Sie sollten die Anforderungen Ihrer Anwendung kennen und regelmäßige Prüfungen für den beibehaltenen Speicher implementieren, um die Menge an nicht verwendetem Speicher zu minimieren, der genutzt und abgerechnet wird.

Weitere Informationen zu Speicherklassenoptionen finden Sie unter [Richtlinien zur Freigabe von Speicher][reclaim-policy].

## <a name="secure-and-back-up-your-data"></a>Schützen und Sichern Ihrer Daten

**Best Practices-Anleitung**: Sichern Sie Ihre Daten mithilfe eines entsprechenden Tools für Ihren Speichertyp, z.B. Velero oder Azure Site Recovery. Überprüfen Sie die Integrität und die Sicherheit dieser Sicherungen.

Wenn Ihre Anwendungen Daten speichern und verbrauchen, die auf Festplatten oder in Dateien gespeichert sind, müssen Sie regelmäßig Sicherungen oder Momentaufnahmen dieser Daten erstellen. Azure-Datenträger können integrierte Momentaufnahmetechnologien verwenden. Möglicherweise müssen Sie Ihre Anwendungen suchen, damit Schreibzugriffe auf die Festplatte geleert werden, bevor Sie den Momentaufnahmevorgang ausführen. [Velero][velero] kann persistente Volumes zusammen mit zusätzlichen Clusterressourcen und -konfigurationen sichern. Wenn Sie [den Zustand nicht aus Ihren Anwendungen entfernen können][remove-state], sichern Sie die Daten von persistenten Volumes, und testen Sie die Wiederherstellungsvorgänge regelmäßig, um die Datenintegrität und die erforderlichen Prozesse zu überprüfen.

Sie sollten die Grenzen der verschiedenen Ansätze für Datensicherungen kennen und wissen, ob Sie Ihre Daten vor dem erstellen der Momentaufnahme stilllegen müssen. Datensicherungen ermöglichen es Ihnen nicht unbedingt, Ihre Anwendungsumgebung der Clusterbereitstellung wiederherzustellen. Weitere Informationen zu diesen Szenarien finden Sie unter [Best Practices für Geschäftskontinuität und Notfallwiederherstellung in Azure Kubernetes Service (AKS)][best-practices-multi-region].

## <a name="next-steps"></a>Nächste Schritte

Dieser Artikel konzentriert sich auf bewährte Speichermethoden in AKS. Weitere Informationen zu den Grundlagen der Speicherung in Kubernetes finden Sie unter [Speicherkonzepte für Anwendungen in AKS][aks-concepts-storage].

<!-- LINKS - External -->
[velero]: https://github.com/heptio/velero
[blobfuse]: https://github.com/Azure/azure-storage-fuse

<!-- LINKS - Internal -->
[aks-concepts-storage]: concepts-storage.md
[vm-sizes]: ../virtual-machines/linux/sizes.md
[dynamic-disks]: azure-disks-dynamic-pv.md
[dynamic-files]: azure-files-dynamic-pv.md
[reclaim-policy]: concepts-storage.md#storage-classes
[aks-concepts-storage-pvcs]: concepts-storage.md#persistent-volume-claims
[aks-concepts-storage-classes]: concepts-storage.md#storage-classes
[managed-disks]: ../virtual-machines/linux/managed-disks-overview.md
[best-practices-multi-region]: operator-best-practices-multi-region.md
[remove-state]: operator-best-practices-multi-region.md#remove-service-state-from-inside-containers
