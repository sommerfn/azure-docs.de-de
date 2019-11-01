---
title: Verwalten und Überwachen von Azure-VM-Sicherungen mit dem Azure Backup-Dienst
description: Erfahren Sie mehr über das Verwalten und Überwachen von Azure-VM-Sicherungen mit dem Azure Backup-Dienst.
ms.reviewer: sogup
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 09/18/2019
ms.author: dacurwin
ms.openlocfilehash: 3781ac1be547f6733417c4f0cae9f3e8681ea9e8
ms.sourcegitcommit: b1c94635078a53eb558d0eb276a5faca1020f835
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2019
ms.locfileid: "72969231"
---
# <a name="manage-azure-vm-backups-with-azure-backup-service"></a>Verwalten von Azure-VM-Sicherungen mit dem Azure Backup-Dienst

In diesem Artikel erfahren Sie mehr über die Verwaltung von Azure-VMs, die mit dem [Azure Backup-Dienst](backup-overview.md) gesichert wurden. Außerdem werden in diesem Artikel die Sicherungsinformationen auf dem Tresordashboard zusammengefasst.

Im Azure-Portal ermöglicht das Dashboard für den Recovery Services-Tresor den Zugriff auf Informationen zum Tresor, z. B.:

* Die letzte Sicherung, die gleichzeitig der letzte Wiederherstellungspunkt ist.
* Die Sicherungsrichtlinie.
* Die Gesamtgröße aller Sicherungsmomentaufnahmen.
* Die Anzahl von virtuellen Computern, die für die Sicherung aktiviert sind.

Sie können Backups über das Dashboard und durch ein Drilldown auf einzelne virtuelle Computer verwalten. Öffnen Sie den Tresor über das Dashboard, um mit dem Sichern von virtuellen Computern zu beginnen.

![Vollständige Dashboardansicht mit Schieberegler](./media/backup-azure-manage-vms/bottom-slider.png)

## <a name="view-vms-on-the-dashboard"></a>Anzeigen von virtuellen Computern auf dem Dashboard

So zeigen Sie virtuelle Computer auf dem Tresordashboard an:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Wählen Sie im Menü „Hub“ die Option **Durchsuchen** aus. Geben Sie in der Liste mit den Ressourcen **Recovery Services**ein. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Wählen Sie **Recovery Services-Tresore**.

    ![Erstellen eines Recovery Services-Tresors](./media/backup-azure-manage-vms/browse-to-rs-vaults.png)

3. Klicken Sie mit der rechten Maustaste auf den Tresor, und wählen Sie **An Dashboard anheften** aus.
4. Öffnen Sie das Dashboard des Tresors.

    ![Öffnen des Tresor-Dashboards und des Blatts „Einstellungen“](./media/backup-azure-manage-vms/full-view-rs-vault.png)

5. Wählen Sie auf der Kachel **Sicherungselemente** die Option **Virtuelle Azure-Computer** aus.

    ![Öffnen der Kachel „Sicherungselemente“](./media/backup-azure-manage-vms/contoso-vault-1606.png)

6. Auf dem Blatt **Sicherungselemente** können Sie die Liste der geschützten virtuellen Computer anzeigen. In diesem Beispiel schützt der Tresor einen virtuellen Computer: demobackup.  

    ![Anzeigen des Blatts „Sicherungselemente“](./media/backup-azure-manage-vms/backup-items-blade-select-item.png)

7. Im Dashboard für Tresorobjekte können Sie Sicherungsrichtlinien ändern, eine On-Demand-Sicherung durchführen, den Schutz von virtuellen Computern beenden und wieder aufnehmen, Sicherungsdaten löschen, Wiederherstellungspunkte anzeigen und eine Wiederherstellung durchführen.

    ![Dashboard „Sicherungselemente“ mit Blatt „Einstellungen“](./media/backup-azure-manage-vms/item-dashboard-settings.png)

## <a name="manage-backup-policy-for-a-vm"></a>Verwalten der Sicherungsrichtlinie für einen virtuellen Computer

So verwalten Sie eine Sicherungsrichtlinie:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an. Öffnen Sie das Dashboard des Tresors.
2. Wählen Sie auf der Kachel **Sicherungselemente** die Option **Virtuelle Azure-Computer** aus.

    ![Öffnen der Kachel „Sicherungselemente“](./media/backup-azure-manage-vms/contoso-vault-1606.png)

3. Auf dem Blatt **Sicherungselemente** können Sie die Liste der geschützten virtuellen Computer und den Status der letzten Sicherung mit dem Zeitpunkt der letzten Wiederherstellung anzeigen.

    ![Anzeigen des Blatts „Sicherungselemente“](./media/backup-azure-manage-vms/backup-items-blade-select-item.png)

4. Auf dem Dashboard für Tresorelemente können Sie eine Sicherungsrichtlinie auswählen.

   * Wählen Sie zum Ändern von Richtlinien eine andere Richtlinie und dann **Speichern** aus. Die neue Richtlinie wird sofort auf den Tresor angewendet.

     ![Auswählen einer Sicherungsrichtlinie](./media/backup-azure-manage-vms/backup-policy-create-new.png)

## <a name="run-an-on-demand-backup"></a>Ausführen einer On-Demand-Sicherung

Sie können eine On-Demand-Sicherung eines virtuellen Computers ausführen, nachdem Sie dessen Schutz konfiguriert haben. Beachten Sie dabei Folgendes:

* Wenn die erste Sicherung ansteht, erstellt die On-Demand-Sicherung eine vollständige Kopie des virtuellen Computers im Recovery Services-Tresor.
* Nachdem die erste Sicherung durchgeführt wurde, werden bei einer On-Demand-Sicherung nur Änderungen der vorherigen Momentaufnahme an den Recovery Services-Tresor gesendet. Spätere Sicherungen erfolgen also immer inkrementell.
* Die Aufbewahrungsdauer für eine On-Demand Sicherung ist der Aufbewahrungswert, den Sie beim Auslösen des Sicherungsvorgangs festlegen.

So lösen Sie eine On-Demand-Sicherung aus:

1. Wählen Sie auf dem[Dashboard für die Tresorelemente](#view-vms-on-the-dashboard) unter **Geschütztes Element** die Option **Sicherungselement** aus.

    ![Die Option „Jetzt sichern“](./media/backup-azure-manage-vms/backup-now-button.png)

2. Wählen Sie unter **Sicherungsverwaltungstyp** die Option **Virtueller Azure-Computer** aus. Das Blatt **Sicherungselemente (Virtueller Azure-Computer)** wird angezeigt.
3. Wählen Sie einen virtuellen Computer und dann **Jetzt sichern** aus, um eine On-Demand-Sicherung zu erstellen. Das Blatt **Jetzt sichern** wird angezeigt.
4. Geben Sie im Feld **Sicherung beibehalten bis** ein Datum ein, bis zu dem die Sicherung beibehalten werden soll.

    ![Der Kalender „Jetzt sichern“](./media/backup-azure-manage-vms/backup-now-check.png)

5. Wählen Sie **OK** aus, um die Sicherung auszuführen.

Wählen Sie auf dem Tresordashboard die Kachel **Sicherungsaufträge** aus, um den Auftragsstatus nachzuverfolgen.

## <a name="stop-protecting-a-vm"></a>Beenden des Schutzes für eine VM

Sie haben zwei Möglichkeiten, den Schutz einer virtuellen Maschine zu beenden:

* **Schutz beenden und Sicherungsdaten beibehalten**. Mit dieser Option werden alle zukünftigen Sicherungsaufträge zum Schutz Ihres virtuellen Computers beendet, jedoch behält der Azure Backup-Dienst die gesicherten Wiederherstellungspunkte bei.  Die Aufbewahrung der Wiederherstellungspunkte im Tresor wird Ihnen in Rechnung gestellt (Einzelheiten finden Sie unter [Azure Backup – Preise](https://azure.microsoft.com/pricing/details/backup/)). Sie können den virtuellen Computer bei Bedarf wiederherstellen. Mit der Option *Sicherung fortsetzen* können Sie bei Bedarf den VM-Schutz fortsetzen.
* **Schutz beenden und Sicherungsdaten löschen**. Mit dieser Option werden alle zukünftigen Sicherungsaufträge zum Schutz Ihres virtuellen Computers beendet und alle Wiederherstellungspunkte gelöscht. Sie können den virtuellen Computer nicht wiederherstellen und auch nicht die Option *Sicherung fortsetzen* verwenden.

>[!NOTE]
>Wenn Sie eine Datenquelle löschen, ohne die Sicherungen zu beenden, treten bei neuen Sicherungen Fehler auf. Alte Wiederherstellungspunkte laufen gemäß der Richtlinie ab. Ein letzter Wiederherstellungspunkt wird jedoch immer beibehalten, bis Sie die Sicherungen beenden und die Daten löschen.
>

### <a name="stop-protection-and-retain-backup-data"></a>Schutz beenden und Sicherungsdaten beibehalten

So beenden Sie den Schutz eines virtuellen Computers und behalten die Sicherungsdaten bei

1. Wählen Sie auf dem [Dashboard für die Tresorelemente](#view-vms-on-the-dashboard) die Option **Sicherung beenden** aus.
2. Wählen Sie **Sicherungsdaten beibehalten** aus, und bestätigen Sie Ihre Auswahl bei Bedarf. Geben Sie bei Bedarf einen Kommentar ein. Falls Sie den Namen des Elements nicht kennen, zeigen Sie mit dem Cursor auf das Ausrufezeichen, um den Namen anzuzeigen.

    ![Beibehalten von Sicherungsdaten](./media/backup-azure-manage-vms/retain-backup-data.png)

Sie erhalten einer Benachrichtigung, wenn die Sicherungsaufträge beendet wurden.

### <a name="stop-protection-and-delete-backup-data"></a>Schutz beenden und Sicherungsdaten löschen

So beenden Sie den Schutz eines virtuellen Computers und löschen die Daten

1. Wählen Sie auf dem [Dashboard für die Tresorelemente](#view-vms-on-the-dashboard) die Option **Sicherung beenden** aus.
2. Wählen Sie **Sicherungsdaten löschen** aus, und bestätigen Sie Ihre Auswahl bei Bedarf. Geben Sie den Namen des Sicherungselements ein, und fügen Sie gegebenenfalls einen Kommentar hinzu.

    ![Löschen von Sicherungsdaten](./media/backup-azure-manage-vms/delete-backup-data1.png)

## <a name="resume-protection-of-a-vm"></a>Fortsetzen des Schutzes einer VM

Wenn Sie beim Beenden des Schutzes des virtuellen Computers die Option [Schutz beenden und Sicherungsdaten beibehalten](#stop-protection-and-retain-backup-data) ausgewählt hatten, können Sie anschließend die Option **Sicherung fortsetzen** verwenden. Diese Option ist nicht verfügbar, wenn Sie [Schutz beenden und Sicherungsdaten löschen](#stop-protection-and-delete-backup-data) oder [Sicherungsdaten löschen](#delete-backup-data) ausgewählt haben.

So setzen Sie den Schutz für einen virtuellen Computer fort:

1. Wählen Sie auf dem [Dashboard für die Tresorelemente](#view-vms-on-the-dashboard) die Option **Sicherung fortsetzen** aus.

2. Führen Sie die Schritte unter [Verwalten von Sicherungsrichtlinien](#manage-backup-policy-for-a-vm) aus, um die Richtlinie für den virtuellen Computer zuzuweisen. Sie müssen nicht die ursprüngliche Schutzrichtlinie des virtuellen Computers auswählen.
3. Wenn Sie die Sicherungsrichtlinie auf dem virtuellen Computer angewendet haben, wird die folgende Meldung angezeigt:

    ![Meldung über erfolgreichen Schutz eines virtuellen Computers](./media/backup-azure-manage-vms/success-message.png)

## <a name="delete-backup-data"></a>Löschen von Sicherungsdaten

Es gibt zwei Möglichkeiten, die Sicherungsdaten eines virtuellen Computers zu löschen:

* Wählen Sie auf dem Dashboard für die Tresorelemente „Sicherung beenden“ aus, und befolgen Sie die für die Option [Schutz beenden und Sicherungsdaten löschen](#stop-protection-and-delete-backup-data) beschriebenen Anweisungen.

  ![Auswählen von „Sicherung beenden“](./media/backup-azure-manage-vms/stop-backup-buttom.png)

* Wählen Sie auf dem Dashboard für die Tresorelemente „Sicherungsdaten löschen“ aus. Diese Option ist aktiviert, wenn Sie beim Beenden des Schutzes des virtuellen Computers die Option [Schutz beenden und Sicherungsdaten beibehalten](#stop-protection-and-retain-backup-data) ausgewählt hatten.

  ![Auswählen von „Sicherung löschen“](./media/backup-azure-manage-vms/delete-backup-buttom.png)

  * Wählen Sie auf dem [Dashboard für die Tresorelemente](#view-vms-on-the-dashboard) die Option **Sicherungsdaten löschen** aus.
  * Geben Sie den Namen des Sicherungselements ein, um zu bestätigen, dass Sie die Wiederherstellungspunkte löschen möchten.

    ![Löschen von Sicherungsdaten](./media/backup-azure-manage-vms/delete-backup-data1.png)

  * Wählen Sie **Löschen** aus, um die Sicherungsdaten des Elements zu löschen. Sie erhalten einer Benachrichtigung, dass die Sicherungsdaten gelöscht wurden.

  > [!NOTE]
  > Wenn Sie die Sicherungsdaten löschen, werden alle zugehörigen Wiederherstellungspunkte gelöscht. Sie können keine bestimmten Wiederherstellungspunkte zum Löschen auswählen.

### <a name="backup-item-where-primary-data-source-no-longer-exists"></a>Sichern eines Elements, bei dem die primäre Datenquelle nicht mehr vorhanden ist

* Wenn für Azure Backup konfigurierte Azure-VMs entweder gelöscht oder verschoben werden, ohne den Schutz zu beenden, schlagen geplante Sicherungsaufträge und bedarfsgesteuerte (Ad-hoc-) Sicherungsaufträge mit dem Fehler „UserErrorVmNotFoundV2“ fehl. Die Vorüberprüfung der Sicherung wird nur bei fehlgeschlagenen Ad-hoc-Sicherungsaufträgen als kritisch angezeigt (fehlerhafte geplante Aufträge werden nicht angezeigt).
* Diese Sicherungselemente bleiben in dem System aktiv, das die vom Benutzer festgelegte Sicherungs-und Aufbewahrungsrichtlinie einhält. Die gesicherten Daten für diese Azure-VMs werden gemäß der Aufbewahrungsrichtlinie beibehalten. Die abgelaufenen Wiederherstellungspunkte (mit Ausnahme des letzten Wiederherstellungspunkts) werden gemäß der Aufbewahrungsdauer bereinigt, die in der Sicherungsrichtlinie festgelegt wurde.
* Benutzern wird empfohlen, die Sicherungselemente zu löschen, bei denen die primäre Datenquelle nicht mehr vorhanden ist. Auf diese Weise werden zusätzliche Kosten vermieden, wenn das Sicherungselement/die Daten für die Löschressourcen nicht mehr benötigt wird bzw. werden, weil der letzte Wiederherstellungspunkt dauerhaft beibehalten und dem Benutzer gemäß den geltenden Preisen für die Sicherung eine Gebühr berechnet wird.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über das [Sichern einer Azure-VM über die VM-Einstellungen](backup-azure-vms-first-look-arm.md).
* Erfahren Sie mehr über das [Wiederherstellen virtueller Azure-Computer](backup-azure-arm-restore-vms.md).
* Erfahren Sie mehr über das [Überwachen von Azure-VM-Sicherungen](backup-azure-monitor-vms.md).
