---
title: Auswählen einer VMware-Migrationsoption mit der Azure Migrate-Servermigration | Microsoft-Dokumentation
description: Bietet eine Übersicht über die Optionen zum Migrieren von VMware-VMs zu Azure mit der Azure Migrate-Servermigration
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: raynew
ms.openlocfilehash: f8bfbe26dc4c6ddbcf622f91938ba060de00b2ec
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67810149"
---
# <a name="select-a-vmware-migration-option"></a>Auswählen einer VMware-Migrationsoption

Sie können VMware-VMs zu Azure migrieren, indem Sie das Tool für die Azure Migrate-Servermigration verwenden. Dieses Tool verfügt über Optionen für die Migration von VMware-VMs:

- Durchführen einer Migration per Replikation ohne Agent Migrieren von VMs, ohne dass darauf Software installiert werden muss
- Durchführen einer Migration mit einem Agent für die Replikation Installieren eines Agents für die Replikation auf der VM


Obwohl die Replikation ohne Agents im Hinblick auf die Bereitstellung einfacher ist, besteht derzeit eine Reihe von Einschränkungen.

## <a name="agentless-migration-limitations"></a>Einschränkungen der Migration ohne Agent

Es bestehen die folgenden Einschränkungen:

- **Gleichzeitige Replikation**: Es können maximal 50 VMs gleichzeitig von einem vCenter-Server repliziert werden.<br/> Wenn mehr als 50 VMs migriert werden sollen, erstellen Sie mehrere Batches von VMs.<br/> Wenn eine größere Anzahl als diese gleichzeitig migriert wird, kommt es zu einer Beeinträchtigung der Leistung.
- **VM-Datenträger**: Eine VM, die migriert werden soll, darf nicht mehr als 60 Datenträger aufweisen.
- **VM-Betriebssysteme**: Im Allgemeinen kann Azure Migrate jedes Windows Server- oder Linux-Betriebssystem migrieren. Möglicherweise sind jedoch Änderungen an VMs erforderlich, damit sie in Azure ausgeführt werden können. Bei diesen Betriebssystemen führt Azure Migrate diese Änderungen automatisch durch:
    - Red Hat Enterprise Linux 6.5+, 7.0+
    - CentOS 6.5+, 7.0+
    - SUSE Linux Enterprise Server 12 SP1+
    - Ubuntu 14.04 LTS, 16.04 LTS, 18.04 LTS
    - Debian 7, 8
    - Bei anderen Betriebssystemen müssen Sie vor der Migration manuell Anpassungen vornehmen. Im [Migrationstutorial](tutorial-migrate-vmware.md) wird erläutert, wie dies geschieht.
- **Linux-Start**: Wenn sich „/boot“ in einer dedizierten Partition befindet, sollte diese auf dem Betriebssystemdatenträger und nicht auf mehrere Datenträger verteilt vorhanden sein.<br/> Wenn „/boot“ Teil der Stammpartition (/) ist, sollte sich diese auf dem Betriebssystemdatenträger befinden und sich nicht auf andere Datenträger erstrecken.
- **UEFI-Start**: Die Migration von VMs mit UEFI-Start wird nicht unterstützt.
- **Verschlüsselte Datenträger/Volumes (BitLocker, Cryptfs)** : Die Migration von VMs mit verschlüsselten Datenträgern/Volumes wird nicht unterstützt.
- **RDM-Datenträger/Pass-Through-Datenträger**: Wenn VMs über RDM-Datenträger oder Pass-Through-Datenträger verfügen, werden diese nicht in Azure repliziert.
- **NFS**: NFS-Volumes, die als Volumes auf den VMs bereitgestellt sind, werden nicht repliziert.
- **Zielspeicher**: Sie können VMware-VMs nur mit verwalteten Datenträgern (HDD Standard, SSD Premium) zu Azure-VMs migrieren.



## <a name="deployment-steps-comparison"></a>Vergleich der Bereitstellungsschritte

Nachdem Sie sich über die Einschränkungen informiert haben, hilft Ihnen die Kenntnis der Schritte zum Bereitstellen der einzelnen Lösungen möglicherweise bei der Auswahl der Option.

**Aufgabe** | **Details** |**Ohne Agent** | **Agent-basiert**
--- | --- | --- | ---
**Vorbereiten der VMware-Server und -VMs für die Migration** | Konfigurieren Sie eine Reihe von Einstellungen für VMware-Server und -VMs. | Erforderlich | Erforderlich
**Hinzufügen des Tools für die Servermigration** | Fügen Sie das Tool für die Azure Migrate-Servermigration im Azure Migrate-Projekt hinzu. | Erforderlich | Erforderlich
**Bereitstellen der Azure Migrate-Appliance** | Richten Sie auf einer VMware-VM eine einfache Appliance für die VM-Ermittlung und -Bewertung ein. | Erforderlich | Nicht erforderlich.
**Installieren des Mobilitätsdiensts auf VMs** | Installieren Sie den Mobilitätsdienst auf jeder VM, die Sie replizieren möchten. | Nicht erforderlich | Erforderlich
**Bereitstellen der Replikationsappliance für die Azure Migrate-Server-Servermigration** | Richten Sie eine Appliance auf einer VMware-VM ein, um VMs zu ermitteln und eine Brücke zwischen dem auf VMs ausgeführten Mobilitätsdienst und der Azure Migrate-Servermigration zu schlagen. | Nicht erforderlich | Erforderlich
**Replizieren von VMs** Aktivieren Sie die VM-Replikation. | Konfigurieren Sie Replikationseinstellungen, und wählen Sie VMs für die Replikation aus. | Erforderlich | Erforderlich
**Ausführen einer Testmigration** | Führen Sie eine Testmigration aus, um sicherzustellen, dass alles wie erwartet funktioniert. | Erforderlich | Erforderlich
**Durchführen einer vollständigen Migration** | Migrieren Sie die VMs. | Erforderlich | Erforderlich




## <a name="next-steps"></a>Nächste Schritte

[Migrieren von VMware-VMs](tutorial-migrate-vmware.md) ohne Agents



