---
title: Bewerten einer großen Anzahl von VMware-VMs für die Migration zu Azure mit Azure Migrate | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie eine große Anzahl von VMware-VMs mithilfe des Azure Migrate-Diensts für die Migration zu Azure bewerten.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/12/2019
ms.author: hamusa
ms.openlocfilehash: c9c57a07100f2ea6db86408826bf74d05c8df5aa
ms.sourcegitcommit: 10251d2a134c37c00f0ec10e0da4a3dffa436fb3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2019
ms.locfileid: "67868679"
---
# <a name="assess-large-numbers-of-vmware-vms-for-migration-to-azure"></a>Bewerten einer großen Anzahl von virtuellen VMware-Computern für die Migration zu Azure


In diesem Artikel wird beschrieben, wie eine große Anzahl von lokalen VMware-VMs (1000-35.000) mithilfe des Azure Migrate-Serverbewertungstools für die Migration zu Azure bewertet wird

[Azure Migrate](migrate-services-overview.md) stellt einen Hub mit Tools bereit, die Ihnen dabei helfen, Apps, Infrastrukturen und Workloads zu ermitteln, zu bewerten und zu Microsoft Azure zu migrieren. Der Hub umfasst Azure Migrate-Tools sowie Angebote von unabhängigen Drittanbietern (Independent Software Vendors, ISVs). 

In diesem Artikel werden folgende Vorgehensweisen behandelt:
> [!div class="checklist"]
> * Planen Sie die Bewertung in gewünschtem Umfang.
> * Konfigurieren Sie Azure-Berechtigungen, und bereiten Sie VMware für die Bewertung vor.
> * Erstellen Sie ein Azure Migrate-Projekt, und erstellen Sie eine Bewertung.
> * Überprüfen Sie die Bewertung beim Planen der Migration.


> [!NOTE]
> Wenn Sie einen Proof of Concept ausprobieren möchten, um einige virtuelle Computer zu bewerten, bevor Sie eine Bewertung im gewünschtem Umfang durchführen, lesen Sie unsere [Tutorialreihe](tutorial-prepare-vmware.md)

## <a name="plan-for-assessment"></a>Planen für die Bewertung

Bei der Planung für die Bewertung einer großen Anzahl von VMware-VMs sind einige Dinge zu berücksichtigen:

- **Planen von Azure Migrate-Projekten**: Finden Sie heraus, wie Sie Azure Migrate-Projekte bereitstellen möchten. Wenn sich Ihre Rechenzentren beispielsweise in verschiedenen geografischen Regionen befinden oder Sie ermittlungs-, bewertungs- oder migrationsspezifische Metadaten in einer anderen geografischen Region speichern müssen, benötigen Sie möglicherweise mehrere Projekte. 
- **Planen von Appliances**: Azure Migrate verwendet eine lokale Azure Migrate-Appliance, die als VMware-VM bereitgestellt wird, um VMs kontinuierlich zu identifizieren. Die Appliance überwacht Umgebungsänderungen wie z.B. das Hinzufügen von VMs, Datenträgern oder Netzwerkadaptern. Sie sendet außerdem deren Meta- und Leistungsdaten an Azure. Sie müssen herausfinden, wie viele Appliances Sie bereitstellen wollen.
- **Planen von Konten für die Ermittlung**: Die Azure Migrate-Appliance verwendet ein Konto mit Zugriff auf vCenter Server, um VMs für die Bewertung und Migration zu ermitteln. Wenn Sie mehr als 10.000 VMs ermitteln, richten Sie mehrere Konten ein.


## <a name="planning-limits"></a>Planen von Einschränkungen
 
Verwenden Sie die in dieser Tabelle zusammengefassten Einschränkungen für die Planung.

**Planung** | **Einschränkungen**
--- | --- 
**Azure Migrate-Projekte** | Bewerten Sie bis zu 35.000 VMs in einem Projekt.
**Azure Migrate-Appliance** | Ein Gerät kann sich nur mit einem einzelnen vCenter Server verbinden.<br/><br/> Eine einzelne Appliance kann nur einer einzelnen Azure Migrate-Ressource zugeordnet werden.<br/> Ein Gerät kann bis zu 10.000 VMs auf einem vCenter Server ermitteln.
**Azure Migrate-Bewertung** | Sie können bis zu 35.000 virtuelle Computer in einer einzelnen Bewertung bewerten.

In Anbetracht dieser Einschränkungen sind hier einige Beispiele für Implementierungen aufgeführt:


**vCenter-Server** | **VMs auf Server** | **Empfehlung** | **Aktion**
---|---|---
Eine | < 10.000 | Ein Azure Migrate-Projekt.<br/> Eine Appliance.<br/> Ein vCenter-Konto für die Ermittlung. | Einrichten der Appliance, Herstellen einer Verbindung mit vCenter Server mit einem Konto.
Eine | > 10.000 | Ein Azure Migrate-Projekt.<br/> Mehrere Appliances.<br/> Mehrere vCenter-Konten. | Richten Sie Appliances für alle 10.000 VMs ein.<br/><br/> Richten Sie vCenter-Konten ein und teilen Sie das Inventar, um den Zugriff auf ein Konto auf weniger als 10.000 VMs zu beschränken.<br/> Verbinden Sie jede Appliance mit dem vCenter-Server über ein Konto.<br/> Sie können Abhängigkeiten zwischen Computern analysieren, die mit verschiedenen Geräten ermittelt werden.
Mehrere | < 10.000 |  Ein Azure Migrate-Projekt.<br/> Mehrere Appliances.<br/> Ein vCenter-Konto für die Ermittlung. | Einrichten der Appliance, Herstellen einer Verbindung mit vCenter Server mit einem Konto.<br/> Sie können Abhängigkeiten zwischen Computern analysieren, die mit verschiedenen Geräten ermittelt werden.
Mehrere | > 10.000 | Ein Azure Migrate-Projekt.<br/> Mehrere Appliances.<br/> Mehrere vCenter-Konten. | Wenn die vCenter Server-Ermittlung < 10.000 VMs beträgt, richten Sie für jeden vCenter Server eine Appliance ein.<br/><br/> Wenn vCenter Server-Ermittlung > 10.000 VMs beträgt, richten Sie eine Appliance für jeweils 10.000 VMs ein.<br/> Richten Sie vCenter-Konten ein und teilen Sie das Inventar, um den Zugriff auf ein Konto auf weniger als 10.000 VMs zu beschränken.<br/> Verbinden Sie jede Appliance mit dem vCenter-Server über ein Konto.<br/> Sie können Abhängigkeiten zwischen Computern analysieren, die mit verschiedenen Geräten ermittelt werden.


## <a name="plan-discovery-in-a-multi-tenant-environment"></a>Planen Sie die Ermittlung in einer mandantenfähigen Umgebung

Wenn Sie eine mandantenfähige Umgebung planen, können Sie die Ermittlung auf dem vCenter Server überprüfen.

- Sie können das Spektrum der Appliance-Ermittlung auf vCenter Server-Datencenter, Cluster, Clusterordner, Hosts, Hostordner oder einzelne VMs festlegen.
- Wenn Ihre Umgebung für alle Mandanten gemeinsam genutzt wird und Sie jeden Mandanten separat ermitteln möchten, können Sie den Zugriff auf das vCenter-Konto einschränken, das die Appliance für die Ermittlung verwendet. 
    - Sie können nach VM-Ordnern suchen, wenn die Mandanten Hosts gemeinsam nutzen. Azure Migrate kann keine VMs erkennen, wenn das vCenter-Konto über einen auf der Ordnerebene von vCenter VM gewährten Zugriff verfügt. Wenn Ihre Ermittlung auf VM-Ordnern basieren soll, muss das vCenter-Konto reinen Lesezugriff auf der VM-Ebene haben. Erfahren Sie [hier](tutorial-assess-vmware.md#scoping-discovery) mehr über das Beschränken der Ermittlung.

## <a name="prepare-for-assessment"></a>Vorbereiten für die Bewertung

Bereiten Sie Azure und VMware für die Serverbewertung vor. 

1. Überprüfen Sie die [VMware-Unterstützungsanforderungen und -Einschränkungen](migrate-support-matrix-vmware.md).
2. Richten sie Berechtigungen für Ihr Azure-Konto zur Interaktion mit Azure Migrate ein.
3. Vorbereiten von VMware für die Bewertung.

Befolgen Sie die Anweisungen in [diesem Tutorial](tutorial-prepare-vmware.md), um diese Einstellungen zu konfigurieren.


## <a name="create-a-project"></a>Erstellen eines Projekts

Führen Sie in Übereinstimmung mit ihren Planungsanforderungen folgende Schritte aus:

1. Erstellen Sie Azure Migrate-Projekte.
2. Fügen Sie den Projekten das Azure Migrate-Serverbewertungs-Tool hinzu.

[Weitere Informationen](how-to-add-tool-first-time.md)

## <a name="create-and-review-an-assessment"></a>Erstellen und Überprüfen einer Bewertung

1. Erstellen Sie Bewertungen für VMware-VMS.
1. Überprüfen Sie die Bewertungen in Vorbereitung auf die Migrationsplanung.


Befolgen Sie die Anweisungen in [diesem Tutorial](tutorial-assess-vmware.md), um diese Einstellungen zu konfigurieren.
    

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel führen Sie folgende Schritte aus:
 
> [!div class="checklist"] 
> * Planen von Azure Migrate-Bewertungen für VMware-VMs in gewünschtem Umfang
> * Vorbereiten von Azure und VMware auf die Bewertung
> * Erstellen eines Azure Migrate-Projekts und Ausführen von Bewertungen
> * Überprüfen der Bewertungen in Vorbereitung auf die Migrationsplanung.

[Erfahren Sie nun](concepts-assessment-calculation.md), wie Bewertungen berechnet und [geändert](how-to-modify-assessment.md) werden.
