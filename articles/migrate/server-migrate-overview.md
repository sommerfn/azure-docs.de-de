---
title: Auswählen einer VMware-Migrationsoption mit der Azure Migrate-Servermigration | Microsoft-Dokumentation
description: Bietet eine Übersicht über die Optionen zum Migrieren von VMware-VMs zu Azure mit der Azure Migrate-Servermigration
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: raynew
ms.openlocfilehash: f27982b4e310d9865e497a3e1e10be9948beb928
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68640745"
---
# <a name="select-a-vmware-migration-option"></a>Auswählen einer VMware-Migrationsoption

Sie können VMware-VMs zu Azure migrieren, indem Sie das Tool für die Azure Migrate-Servermigration verwenden. Dieses Tool verfügt über Optionen für die Migration von VMware-VMs:

- Durchführen einer Migration per Replikation ohne Agent Migrieren von VMs, ohne dass darauf Software installiert werden muss
- Durchführen einer Migration mit einem Agent für die Replikation Installieren eines Agents für die Replikation auf der VM




## <a name="compare-migration-methods"></a>Vergleichen von Migrationsmethoden

Die folgende Tabelle hilft Ihnen bei der Wahl der geeigneten Methode. Sie können sich auch die Anforderungen der vollständigen Unterstützung für die Migration [ohne Agent](migrate-support-matrix-vmware.md#agentless-migration-vmware-server-requirements) und die [Agent-basierte](migrate-support-matrix-vmware.md#agent-based-migration-vmware-server-requirements) Migration ansehen.

**Einstellung** | **Ohne Agent** | **Agent-basiert**
--- | --- | ---
**Azure-Berechtigungen** | Sie benötigen Berechtigungen zum Erstellen eines Azure Migrate-Projekts sowie zum Registrieren von Azure AD-Apps, die beim Bereitstellen der Azure Migrate-Appliance erstellt werden. | Sie benötigen Berechtigungen vom Typ „Mitwirkender“ für das Azure-Abonnement. 
**Gleichzeitige Replikation** | Es können maximal 100 VMs gleichzeitig von einem vCenter-Server repliziert werden.<br/> Wenn mehr als 50 VMs migriert werden sollen, erstellen Sie mehrere Batches von VMs.<br/> Wenn eine größere Anzahl als diese gleichzeitig migriert wird, kommt es zu einer Beeinträchtigung der Leistung. | Nicht verfügbar
**Bereitstellung einer Appliance** | Die [Azure Migrate-Appliance](migrate-appliance.md) wird lokal bereitgestellt. | Die [Azure Migrate-Replikationsappliance](migrate-replication-appliance.md) wird lokal bereitgestellt.
**Site Recovery-Kompatibilität** | Kompatibel | Wenn Sie die Replikation für einen Computer mit Site Recovery eingerichtet haben, können Sie keine Replikation mit der Azure Migrate-Servermigration durchführen.
**Zieldatenträger** | Verwaltete Datenträger | Verwaltete Datenträger
**Einschränkungen für Datenträger** | Betriebssystemdatenträger: 2 TB<br/><br/> Datenträger für Daten: 4 TB<br/><br/> Maximale Datenträger: 60 | Betriebssystemdatenträger: 2 TB<br/><br/> Datenträger für Daten: 4 TB<br/><br/> Maximale Datenträger: 63
**Pass-Through-Datenträger** | Nicht unterstützt | Unterstützt
**UEFI-Start** | Nicht unterstützt | Der migrierte virtuelle Computer in Azure wird automatisch in einen virtuellen Computer mit BIOS-Start konvertiert.<br/><br/> Der Betriebssystemdatenträger sollte bis zu vier Partitionen aufweisen, und Volumes sollten mit NTFS formatiert sein.


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

[Migrieren von virtuellen VMware-Computern](tutorial-migrate-vmware.md) ohne Agents



