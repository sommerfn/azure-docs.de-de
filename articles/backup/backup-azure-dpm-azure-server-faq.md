---
title: Häufig gestellte Fragen zu Azure Backup Server und Data Protection Manager (DPM)
description: In diesem Artikel finden Sie Antworten auf häufig gestellte Fragen zu Microsoft Azure Backup Server (MABS) und DPM.
ms.reviewer: srinathv
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: dacurwin
ms.openlocfilehash: 940ac4d94bf39583fad828545a45cbb033908f15
ms.sourcegitcommit: a170b69b592e6e7e5cc816dabc0246f97897cb0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2019
ms.locfileid: "74091660"
---
# <a name="azure-backup-server-and-dpm---faq"></a>Azure Backup Server und DPM – häufig gestellte Fragen

## <a name="general-questions"></a>Allgemeine Fragen

In diesem Artikel werden einige häufig gestellte Fragen zu Azure Backup Server und DPM beantwortet.

### <a name="can-i-use-azure-backup-server-to-create-a-bare-metal-recovery-bmr-backup-for-a-physical-server"></a>Kann ich Azure Backup Server verwenden, um eine Bare Metal Recovery-Sicherung (BMR) für einen physischen Server zu erstellen?

Ja.

### <a name="can-i-register-the-server-to-multiple-vaults"></a>Kann ich den Server für mehrere Tresore registrieren?

Nein. Ein DPM- oder Azure Backup-Server kann nur für einen Tresor registriert werden.

### <a name="can-i-use-dpm-to-back-up-apps-in-azure-stack"></a>Kann ich mit DPM Apps in Azure Stack sichern?

Nein. Sie können Azure Stack mit Azure Backup schützen, das Sichern von Apps in Azure Stack mit DPM wird von Azure Backup jedoch nicht unterstützt.

### <a name="if-ive-installed-azure-backup-agent-to-protect-my-files-and-folders-can-i-install-system-center-dpm-to-back-up-on-premises-workloads-to-azure"></a>Kann ich System Center DPM zum Sichern von lokalen Workloads in Azure installieren, wenn ich den Azure Backup-Agent zum Schutz meiner Dateien und Ordner installiert habe?

Ja. Sie sollten jedoch zuerst DPM einrichten und dann den Azure Backup-Agent installieren.  Das Installieren der Komponenten in dieser Reihenfolge stellt sicher, dass der Azure Backup-Agent mit DPM verwendet werden kann. Das Installieren des Agents vor DPM wird nicht empfohlen oder unterstützt.

### <a name="why-cant-i-add-an-external-dpm-server-after-installing-ur7-and-latest-azure-backup-agent"></a>Warum kann ich nach der Installation von UR7 und des neuesten Azure Backup-Agents keinen externen DPM-Server hinzufügen?

Für die vorhandenen DPM-Server mit Datenquellen, die in der Cloud (mithilfe eines Updaterollups vor Update Rollup 7) geschützt sind, müssen Sie mindestens einen Tag nach der Installation von UR7 und des neuesten Azure Backup-Agents warten, bevor Sie **externe DPM-Server hinzufügen**. Diese eintägige Frist ist erforderlich, um die Metadaten der DPM-Schutzgruppen in Azure hochzuladen. Schutzgruppen-Metadaten werden erstmalig über einen nächtlichen Auftrag hochgeladen.

## <a name="vmware-and-hyper-v-backup"></a>VMware- und Hyper-V-Sicherung

### <a name="can-i-back-up-vmware-vcenter-servers-to-azure"></a>Kann ich VMware vCenter-Server in Azure sichern?

Ja. Mit Azure Backup Server können Sie VMware vCenter-Server und ESXi-Hosts in Azure sichern.

- [Erfahren Sie mehr](backup-mabs-protection-matrix.md) über unterstützte Versionen.
- [Gehen Sie folgendermaßen vor](backup-azure-backup-server-vmware.md), um einen VMware-Server zu sichern.

### <a name="do-i-need-a-separate-license-to-recover-a-full-on-premises-vmwarehyper-v-cluster"></a>Benötige ich eine separate Lizenz, um einen vollständigen lokalen VMware-/Hyper-V-Cluster wiederherzustellen?

Sie benötigen für den VMware-/Hyper-V-Schutz keine separate Lizenzierung.

- Wenn Sie ein System Center-Kunde sind, verwenden Sie System Center Data Protection Manager (DPM) zum Schützen von VMware-VMs.
- Falls Sie kein System Center-Kunde sind, können Sie Azure Backup Server (nutzungsbasierte Bezahlung) verwenden, um VMware-VMs zu schützen.

## <a name="sharepoint"></a>SharePoint

### <a name="can-i-recover-a-sharepoint-item-to-the-original-location-if-sharepoint-is-configured-by-using-sql-alwayson-with-protection-on-disk"></a>Kann ich ein SharePoint-Element am ursprünglichen Speicherort wiederherstellen, wenn SharePoint mit SQL AlwaysOn (und datenträgerbasiertem Schutz) konfiguriert wird?

Ja, das Element kann in der ursprünglichen SharePoint-Site wiederhergestellt werden.

### <a name="can-i-recover-a-sharepoint-database-to-the-original-location-if-sharepoint-is-configured-by-using-sql-alwayson"></a>Kann ich eine SharePoint-Datenbank am ursprünglichen Speicherort wiederherstellen, wenn SharePoint mit SQL AlwaysOn konfiguriert wird?

Da SharePoint-Datenbanken in SQL AlwaysOn konfiguriert werden, können sie nur geändert werden, wenn die Verfügbarkeitsgruppe entfernt wird. DPM kann eine Datenbank daher nicht am ursprünglichen Speicherort wiederherstellen. Sie können eine SQL Server-Datenbank in einer anderen SQL Server-Instanz wiederherstellen.

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die anderen häufig gestellten Fragen:

- [Erfahren Sie mehr](backup-support-matrix-mabs-dpm.md) über die Supportmatrix von Azure Backup Server und DPM.
- [Erfahren Sie mehr](backup-azure-mabs-troubleshoot.md) über die Richtlinien zum Troubleshooting für Azure Backup Server und DPM.
