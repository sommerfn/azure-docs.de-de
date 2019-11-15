---
title: Sichern einer Azure-VM über die VM-Einstellungen mit Azure Backup
description: In diesem Artikel erfahren Sie, wie Sie entweder einen einzelnen virtuellen Azure-Computer oder mehrere virtuelle Azure-Computer mit dem Azure Backup-Dienst sichern können.
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 06/13/2019
ms.author: dacurwin
ms.openlocfilehash: 232c027ef60a031df53f2439586a0ae459ab67d5
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73747242"
---
# <a name="back-up-an-azure-vm-from-the-vm-settings"></a>Sichern einer Azure-VM über die VM-Einstellungen

Dieser Artikel beschreibt, wie Sie Azure-VMs mit dem [Azure Backup](backup-overview.md)-Dienst sichern. Sie können Azure-VMs mit verschiedenen Methoden sichern:

- Einzelne Azure-VM: Die Anweisungen in diesem Artikel beschreiben, wie Sie eine Azure-VM direkt aus den VM-Einstellungen sichern können.
- Mehrere Azure-VMs: Sie können einen Recovery Services-Tresor einrichten und die Sicherung für mehrere Azure-VMs konfigurieren. Folgen Sie für dieses Szenario den Anweisungen in [diesem Artikel](backup-azure-arm-vms-prepare.md).

## <a name="before-you-start"></a>Vorbereitung

1. [Erfahren Sie](backup-architecture.md#how-does-azure-backup-work), wie die Sicherung funktioniert, und [überprüfen](backup-support-matrix.md#azure-vm-backup-support) Sie die Supportanforderungen.
2. [Verschaffen Sie sich einen Überblick](backup-azure-vms-introduction.md) über die Sicherung von Azure-VMs.

### <a name="azure-vm-agent-installation"></a>Azure-VM-Agent-Installation

Um Azure-VMs zu sichern, installiert Azure Backup eine Erweiterung auf dem VM-Agent, der auf dem Computer ausgeführt wird. Wenn Ihre VM aus einem Azure Marktplatz-Image erstellt wurde, wird der Agent ausgeführt. In einigen Fällen, z.B. wenn Sie eine benutzerdefinierte VM erstellen oder einen Computer von lokalen Standorten aus migrieren, müssen den Agent ggf. manuell installieren.

- Wenn Sie den VM-Agent manuell installieren müssen, folgen Sie den Anweisungen für [Windows](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows)- oder [Linux](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux)-VMs.
- Nach der Installation des Agent installiert Azure Backup die Sicherungserweiterung auf den Agent, wenn Sie die Sicherung aktivieren. Die Erweiterung wird ohne Eingreifen des Benutzers aktualisiert und gepatcht.

## <a name="back-up-from-azure-vm-settings"></a>Sichern über Azure-VM-Einstellungen

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Klicken Sie auf **Alle Dienste** und geben Sie im Filter **Virtuelle Computer** ein, und klicken Sie dann auf **Virtuelle Computer**.
3. Wählen Sie aus der Liste der VMs die VM aus, die Sie sichern möchten.
4. Klicken Sie auf der VM-Menü auf **Sichern**.
5. Gehen Sie im **Recovery Services-Tresor** wie folgt vor:
   - Wenn Sie bereits über einen Tresor verfügen, klicken Sie auf **Vorhanden auswählen**, und wählen Sie einen Tresor aus.
   - Wenn Sie keinen Tresor haben, klicken Sie auf **Neu erstellen**. Geben Sie einen Namen für den Tresor an. Es wird in der gleichen Region und Ressourcengruppe erstellt wie die VM. Sie können diese Einstellungen nicht ändern, wenn Sie die Sicherung direkt aus den VM-Einstellungen heraus aktivieren.

   ![Aktivieren des Sicherungs-Assistenten](./media/backup-azure-vms-first-look-arm/vm-menu-enable-backup-small.png)

6. Gehen Sie in **Sicherungsrichtlinie auswählen** wie folgt vor:

   - Übernehmen Sie die Standardrichtlinie. Dadurch wird die VM einmal täglich zur angegebenen Zeit gesichert und die Sicherungen werden 30 Tage lang im Tresor aufbewahrt.
   - Wählen Sie eine bestehende Sicherungsrichtlinie aus, sofern vorhanden.
   - Erstellen Sie eine neue Richtlinie, und definieren Sie die Richtlinieneinstellungen.  

   ![Sicherungsrichtlinie auswählen](./media/backup-azure-vms-first-look-arm/set-backup-policy.png)

7. Klicken Sie auf **Sicherung aktivieren**. Dadurch wird die Sicherungsrichtlinie mit der VM verknüpft.

    ![Schaltfläche „Sicherung aktivieren“](./media/backup-azure-vms-first-look-arm/vm-management-menu-enable-backup-button.png)

8. Den Konfigurationsprozess können Sie über die Benachrichtigungen im Portal nachverfolgen.
9. Klicken Sie nach Abschluss des Auftrags im VM-Menü auf **Sicherung**. Die Seite zeigt den Sicherungsstatus für die VM, Informationen über Wiederherstellungspunkte, laufende Aufträge und ausgegebene Warnmeldungen an.

   ![Sicherungsstatus](./media/backup-azure-vms-first-look-arm/backup-item-view-update.png)

10. Nachdem Sie die Sicherung aktiviert haben, wird eine erste Sicherung ausgeführt. Sie können die erste Sicherung sofort starten oder warten, bis sie gemäß dem Sicherungszeitplan beginnt.
    - Bis zum Abschluss des ersten Sicherungsvorgangs wird **Status der letzten Sicherung** als **Warnung (erste Sicherung steht aus)** angezeigt.
    - Um zu sehen, wann die nächste geplante Sicherung ausgeführt wird, klicken Sie auf den Namen der Sicherungsrichtlinie.

> [!NOTE]
> Der Azure Backup-Dienst erstellt zum Speichern der Momentaufnahme eine separate Ressourcengruppe (nicht die VM-Ressourcengruppe) mit dem Namensformat **AzureBackupRG_geography_number** (Beispiel: AzureBackupRG_northeurope_1). Die Daten in dieser Ressourcengruppe werden für die Dauer (in Tagen) aufbewahrt, die in der Sicherungsrichtlinie für virtuelle Azure-Computer unter „Momentaufnahme(n) zur sofortigen Wiederherstellung beibehalten für“ angegeben ist. Das Anwenden einer Sperre auf diese Ressourcengruppe kann zu Sicherungsfehlern führen.<br>
Diese Ressourcengruppe sollte von allen Namens-/Tag-Einschränkungen ausgeschlossen werden, da eine Einschränkungsrichtlinie die Erstellung von Wiederherstellungspunktsammlungen in dieser Gruppe blockieren und erneut zu Fehlern führen würde.

## <a name="run-a-backup-immediately"></a>Sofortige Ausführung einer Sicherung

1. Um eine Sicherung sofort auszuführen, klicken Sie im VM-Menü auf **Sicherung** > **Jetzt sichern**.

    ![Ausführen der Sicherung](./media/backup-azure-vms-first-look-arm/backup-now-update.png)

2. Verwenden Sie unter **Jetzt sichern** im Kalender aus, bis wann der Wiederherstellungspunkt beibehalten wird > und **OK**.

    ![Tage für Aufbewahrung von Sicherungen](./media/backup-azure-vms-first-look-arm/backup-now-blade-calendar.png)

3. Sie werden über Portalbenachrichtigungen darüber informiert, dass der Sicherungsauftrag ausgelöst wurde. Um den Fortschritt der Sicherung zu überwachen, klicken Sie auf **Alle Aufträge anzeigen**.

## <a name="back-up-from-the-recovery-services-vault"></a>Sichern über den Recovery Services-Tresor

Befolgen Sie die Anweisungen in diesem Artikel, um die Sicherung für Azure-VMs zu aktivieren, indem Sie einen Azure Backup Recovery Services-Tresor einrichten und die Sicherung im Tresor aktivieren.

## <a name="next-steps"></a>Nächste Schritte

- Wenn Sie Schwierigkeiten mit einem der Verfahren in diesem Artikel haben, lesen Sie den [Leitfaden zur Problembehandlung](backup-azure-vms-troubleshoot.md).
- [Erfahren Sie mehr](backup-azure-manage-vms.md) über das Verwalten Ihrer Sicherungen.
