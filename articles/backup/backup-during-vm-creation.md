---
title: Aktivieren der Sicherung beim Erstellen eines virtuellen Azure-Computers mit Azure Backup
description: Hier wird beschrieben, wie die Sicherung beim Erstellen eines virtuellen Azure-Computers mit Azure Backup aktiviert wird.
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 06/13/2019
ms.author: dacurwin
ms.openlocfilehash: e873980ce7aab1c5454a28e88df24bdb189c4860
ms.sourcegitcommit: a107430549622028fcd7730db84f61b0064bf52f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2019
ms.locfileid: "74074847"
---
# <a name="enable-backup-when-you-create-an-azure-vm"></a>Aktivieren der Sicherung beim Erstellen eines virtuellen Azure-Computers

Verwenden Sie den Azure Backup-Dienst zum Sichern von Azure-VMs (virtuelle Computer). VMs werden gemäß eines Zeitplans gesichert, der in einer Sicherungsrichtlinie festgelegt wird. Die Wiederherstellungspunkte werden aus Sicherungen erstellt. Die Wiederherstellungspunkte werden in Recovery Services-Tresoren gespeichert.

In diesem Artikel erfahren Sie, wie Sie die Sicherung beim Erstellen eines virtuellen Computers (Virtual Machine, VM) über das Azure-Portal aktivieren.  

## <a name="before-you-start"></a>Vorbereitung

- [Überprüfen Sie](backup-support-matrix-iaas.md#supported-backup-actions), welche Betriebssysteme unterstützt werden, wenn Sie beim Erstellen einer VM die Sicherung aktivieren.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Falls Sie noch nicht bei Ihrem Konto angemeldet sind, melden Sie beim [Azure-Portal](https://portal.azure.com) an.

## <a name="create-a-vm-with-backup-configured"></a>Erstellen einer VM mit konfigurierter Sicherung

1. Klicken Sie im Azure-Portal auf **Ressource erstellen**.

2. Klicken Sie im Azure Marketplace auf **Compute**, und wählen Sie dann ein VM-Image aus.

3. Richten Sie die VM gemäß den Anweisungen für [Windows](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal) oder [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-portal) ein.

4. Klicken Sie auf der Registerkarte **Verwaltung** unter **Backup aktivieren** auf **Ein**.
5. Azure Backup führt Sicherungen in einen Recovery Services-Tresor durch. Klicken Sie auf **Neu erstellen**, wenn Sie über keinen vorhandenen Tresor verfügen.
6. Akzeptieren Sie den vorgeschlagenen Tresornamen, oder geben Sie einen eigenen an.
7. Geben Sie eine Ressourcengruppe an oder erstellen Sie sie, in der sich der Tresor befinden wird. Der Tresor der Ressourcengruppe kann sich von der VM-Ressourcengruppe unterscheiden.

    ![Aktivieren der Sicherung für einen virtuellen Computer](./media/backup-during-vm-creation/enable-backup.png)

8. Akzeptieren Sie die Standardsicherungsrichtlinie, oder ändern Sie die Einstellungen.
    - Eine Sicherungsrichtlinie gibt an, wie häufig Sicherungsmomentaufnahmen des virtuellen Computers erstellt und wie lang die Sicherungskopien aufbewahrt werden sollen.
    - Die Standardrichtlinie sichert den virtuellen Computer einmal täglich.
    - Sie können Ihre eigene Sicherungsrichtlinie für eine Azure-VM anpassen, um täglich oder wöchentlich Sicherungen zu erstellen.
    - [Weitere Informationen](backup-azure-vms-introduction.md#backup-and-restore-considerations) zu Überlegungen zur Sicherung für Azure-VMs.
    - [Weitere Informationen](backup-instant-restore-capability.md) zur Funktionalität der sofortigen Wiederherstellung.

      ![Standardsicherungsrichtlinie](./media/backup-during-vm-creation/daily-policy.png)

> [!NOTE]
> Der Azure Backup-Dienst erstellt zum Speichern der Momentaufnahme eine separate Ressourcengruppe (nicht die VM-Ressourcengruppe) mit dem Namensformat **AzureBackupRG_geography_number** (Beispiel: AzureBackupRG_northeurope_1). Die Daten in dieser Ressourcengruppe werden so lange aufbewahrt, wie dies in der Sicherungsrichtlinie für virtuelle Azure-Computer unter *Momentaufnahme(n) zur sofortigen Wiederherstellung beibehalten für* angegeben ist (in Tagen).  Das Anwenden einer Sperre auf diese Ressourcengruppe kann zu Sicherungsfehlern führen. <br> Diese Ressourcengruppe sollte von allen Namens-/Tag-Einschränkungen ausgeschlossen werden, da eine Einschränkungsrichtlinie die Erstellung von Wiederherstellungspunktsammlungen in dieser Gruppe blockieren und erneut zu Fehlern führen würde.

## <a name="start-a-backup-after-creating-the-vm"></a>Starten einer Sicherung nach dem Erstellen der VM

Ihre VM-Sicherung wird in Übereinstimmung mit Ihrer Sicherungsrichtlinie ausgeführt. Es wird jedoch empfohlen, eine erste Sicherung auszuführen.

Nachdem die VM erstellt wurde, gehen Sie wie folgt vor:

1. Klicken Sie unter den VM-Eigenschaften auf **Sicherung**. Der VM-Status lautet „Erste Sicherung ausstehend“, bis die erste Sicherung ausgeführt wird.
2. Klicken Sie auf **Jetzt sichern**, um eine On-Demand-Sicherung auszuführen.

    ![Ausführen einer On-Demand-Sicherung](./media/backup-during-vm-creation/run-backup.png)

## <a name="use-a-resource-manager-template-to-deploy-a-protected-vm"></a>Bereitstellen einer geschützten VM mithilfe einer Resource Manager-Vorlage

In den vorherigen Schritten wurde erläutert, wie Sie eine VM über das Azure-Portal erstellen und in einem Recovery Services-Tresor schützen. Informationen zur schnellen Bereitstellung und zum Schützen von VMs in einem Recovery Services-Tresor finden Sie unter [Deploy a Windows VM and enable backup (Bereitstellen einer Windows-VM und Aktivieren der Sicherung)](https://azure.microsoft.com/resources/templates/101-recovery-services-create-vm-and-configure-backup/).

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Ihre VM geschützt haben, erfahren Sie, wie Sie diese verwalten und wiederherstellen können.

- [Verwalten und Überwachen virtueller Computer](backup-azure-manage-vms.md)
- [Wiederherstellen eines virtuellen Computers](backup-azure-arm-restore-vms.md)

Wenn Sie auf Probleme stoßen, [lesen](backup-azure-vms-troubleshoot.md) Sie den Leitfaden zur Problembehandlung.
