---
title: Löschen eines Recovery Services-Tresors in Azure
description: In diesem Artikel wird beschrieben, wie Sie einen Recovery Services-Tresor löschen.
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: dacurwin
ms.openlocfilehash: 34484c309cb186aabec519e54269fefae316165e
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68639911"
---
# <a name="delete-a-recovery-services-vault"></a>Löschen eines Recovery Services-Tresors

In diesem Artikel wird beschrieben, wie Sie einen Recovery Services-Tresor von [Azure Backup](backup-overview.md) löschen. Außerdem enthält er Anweisungen zum Entfernen von Abhängigkeiten und anschließenden Löschen eines Tresors.


## <a name="before-you-start"></a>Vorbereitung

Sie können keinen Recovery Services-Tresor löschen, der über Abhängigkeiten verfügt, z. B. geschützte Server oder Server für die Sicherungsverwaltung, die dem Tresor zugeordnet sind.

- Der Tresor mit Sicherungsdaten kann nicht gelöscht werden. (Dies gilt auch, wenn Sie den Schutz beendet und die Sicherungsdaten beibehalten haben.)

- Wenn Sie einen Tresor löschen, der Abhängigkeiten enthält, wird der folgende Fehler angezeigt:

  ![Fehler beim Löschen des Tresors](./media/backup-azure-delete-vault/error.png)

- Wenn Sie ein lokales geschütztes Element (MARS, MABS oder DPM für Azure) mit Abhängigkeiten über das Portal löschen, wird eine Warnmeldung angezeigt:

  ![Serverfehler beim Löschen eines geschützten Elements](./media/backup-azure-delete-vault/error-message.jpg)

  
Wählen Sie zum korrekten Löschen des Tresors das Szenario, das Ihrem Setup entspricht, und führen Sie die empfohlenen Schritte aus:

Szenario | Schritte zum Entfernen von Abhängigkeiten beim Löschen eines Tresors |
-- | --
Ich verfüge über lokale Dateien und Ordner, die per Azure Backup-Agent (MARS) geschützt sind und in Azure gesichert werden. | Führen Sie die Schritte unter „Löschen von Sicherungsdaten und Sicherungselementen: [MARS-Agent](#delete-backup-items-from-mars-management-console)“ aus.
Ich verfüge über lokale Computer, die per MABS (Microsoft Azure Backup Server) oder DPM in Azure (System Center Data Protection Manager) geschützt sind. | Führen Sie die Schritte unter „Löschen von Sicherungsdaten und Sicherungselementen: [MABS-Agent](#delete-backup-items-from-mabs-management-console)“ aus.
Ich verfüge über geschützte Elemente in der Cloud (z. B. laaS-VM, Azure-Dateifreigabe usw.).  | Führen Sie die Schritte unter „Löschen von Sicherungsdaten und Sicherungselementen: [Geschützte Elemente in der Cloud](#delete-protected-items-in-cloud)“ aus.
Ich verfüge sowohl lokal als auch in der Cloud über geschützte Elemente. | Führen Sie die Schritte unter „Löschen von Sicherungsdaten und Sicherungselementen“ in der unten angegebenen Sequenz aus: <br> - [Geschützte Elemente in der Cloud](#delete-protected-items-in-cloud)<br> - [Für MARS-Agent](#delete-backup-items-from-mars-management-console) <br> - [MARS-Agent](#delete-backup-items-from-mabs-management-console)
Ich verfüge weder lokal noch in der Cloud über geschützte Elemente, erhalte aber immer noch den „Fehler beim Löschen von Tresor“. | Führen Sie die Schritte unter [Löschen des Recovery Services-Tresors mit dem Azure Resource Manager-Client](#delete-the-recovery-services-vault-using-azure-resource-manager-client) aus.


## <a name="delete-protected-items-in-cloud"></a>Löschen von geschützten Elementen in der Cloud

Lesen Sie vor dem Fortfahren **[diesen Abschnitt](#before-you-start)** , um sich mit den Abhängigkeiten und dem Prozess zum Löschen des Tresors vertraut zu machen.

Führen Sie Folgendes aus, um den Schutz zu beenden und die Sicherungsdaten zu löschen:

1. Wählen Sie im Portal unter **Recovery Services-Tresor** > **Sicherungselemente** die geschützten Elemente in der Cloud aus: Azure Virtual Machine-Beispiel, Azure Storage (Azure Files), SQL, Azure-VM usw.

    ![Sicherungstyp auswählen](./media/backup-azure-delete-vault/azure-storage-selected.png)

2. Klicken Sie mit der rechten Maustaste auf das Sicherungselement. Je nachdem, ob das Sicherungselement geschützt ist oder nicht, zeigt das Menü **Sicherung beenden** oder **Sicherungsdaten löschen** an.

    - Wenn **Sicherung beenden** angezeigt wird, wählen Sie im Dropdownmenü die Option **Sicherungsdaten löschen** aus. Geben Sie den **Namen** des Sicherungselements ein (beachten Sie dabei die Groß- und Kleinschreibung), wählen Sie einen **Grund** aus, geben Sie **Kommentare** ein, und klicken Sie auf **Sicherung beenden**.

        ![Sicherungstyp auswählen](./media/backup-azure-delete-vault/stop-backup-item.png)

    - Wenn **Sicherungsdaten löschen** angezeigt wird, geben Sie den Namen des Sicherungselements ein (beachten Sie dabei die Groß- und Kleinschreibung), wählen Sie einen **Grund** aus, geben Sie **Kommentare** ein, und klicken Sie auf **löschen**. 

         ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

5. Überprüfen Sie die **Benachrichtigung** ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/messages.png). Nach Abschluss des Vorgangs zeigt der Dienst die folgende Meldung an: **Sicherung beenden und Sicherungsdaten für „*Sicherungselement*“ löschen**. **Der Vorgang wurde erfolgreich abgeschlossen.**
6. Klicken Sie im Menü **Sicherungselemente** auf **Aktualisieren**, um zu überprüfen, ob das Sicherungselement entfernt wurde.

      ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/empty-items-list.png)

## <a name="delete-protected-items-on-premises"></a>Löschen von geschützten lokalen Elementen

Lesen Sie vor dem Fortfahren **[diesen Abschnitt](#before-you-start)** , um sich mit den Abhängigkeiten und dem Prozess zum Löschen des Tresors vertraut zu machen.

1. Klicken Sie im Menü des Tresordashboards auf **Sicherungsinfrastruktur**.
2. Wählen Sie je nach lokalem Szenario die entsprechende Option aus:

      - Wählen Sie für **Azure Backup-Agent** die Option **Geschützte Server** > **Azure Backup-Agent** aus, und wählen Sie dann den Server aus, den Sie löschen möchten. 

        ![Tresor auswählen, um das zugehörige Dashboard zu öffnen](./media/backup-azure-delete-vault/identify-protected-servers.png)

      - Wählen Sie für **Azure Backup Server**/**DPM** die Option **Sicherungsverwaltungsserver** aus. Wählen Sie den Server aus, den Sie löschen möchten. 


          ![Auswählen des Tresors, um das zugehörige Dashboard zu öffnen](./media/backup-azure-delete-vault/delete-backup-management-servers.png)

3. Das Blatt **Löschen** wird geöffnet und zeigt eine Warnmeldung an.

     ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/delete-protected-server.png)

     Lesen Sie die Warnmeldung und die Anweisungen im Kontrollkästchen für die Zustimmung.
    
    > [!NOTE]
    >- Wenn der geschützte Server mit dem Azure-Dienst synchron ist und Sicherungselemente vorhanden sind, werden im Kontrollkästchen für die Zustimmung die Anzahl von abhängigen Sicherungselementen und der Link angezeigt, unter dem die Sicherungselemente angezeigt werden können.
    >- Wenn der geschützte Server nicht mit dem Azure-Dienst synchron ist und Sicherungselemente vorhanden sind, wird im Kontrollkästchen für die Zustimmung die Anzahl von Sicherungselementen angezeigt.
    >- Wenn keine Sicherungselemente vorhanden sind, zeigt das Kontrollkästchen für die Zustimmung eine Aufforderung zum Löschen an.

4. Aktivieren Sie das Kontrollkästchen für die Zustimmung, und klicken Sie auf **Löschen**.




5. Überprüfen Sie die **Benachrichtigung** ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/messages.png). Nach Abschluss des Vorgangs zeigt der Dienst die folgende Meldung an: **Sicherung beenden und Sicherungsdaten für „*Sicherungselement*“ löschen**. **Der Vorgang wurde erfolgreich abgeschlossen.**
6. Klicken Sie im Menü **Sicherungselemente** auf **Aktualisieren**, um zu überprüfen, ob das Sicherungselement entfernt wurde.

Sie können die Sicherungselemente jetzt über die Verwaltungskonsole löschen:
    
   - [Über MARS geschützte Elemente](#delete-backup-items-from-mars-management-console)
    - [Über MABS geschützte Elemente](#delete-backup-items-from-mabs-management-console)


### <a name="delete-backup-items-from-mars-management-console"></a>Löschen von Sicherungselementen über die MARS-Verwaltungskonsole

So löschen Sie Sicherungselemente über die MARS-Verwaltungskonsole

- Starten Sie die MARS-Verwaltungskonsole, navigieren Sie zum Bereich **Aktionen**, und wählen Sie **Sicherung planen** aus.
- Wählen Sie im Assistenten **Geplante Sicherung ändern oder beenden** die Option **Sicherungszeitplan nicht länger verwenden und alle gespeicherten Sicherungen löschen** aus, und klicken Sie anschließend auf **Weiter**.

    ![Ändern oder Beenden einer geplanten Sicherung](./media/backup-azure-delete-vault/modify-schedule-backup.png)

- Klicken Sie im Assistenten **Geplante Sicherung beenden** auf **Fertig stellen**.

    ![Beenden einer geplanten Sicherung](./media/backup-azure-delete-vault/stop-schedule-backup.png)
- Sie werden zur Eingabe einer Sicherheits-PIN aufgefordert. Gehen Sie zum Generieren der PIN wie folgt vor:
  - Melden Sie sich beim Azure-Portal an.
  - Navigieren Sie zu **Recovery Services-Tresor** > **Einstellungen** > **Eigenschaften**.
  - Klicken Sie unter **Sicherheits-PIN** auf **Generieren**. Kopieren Sie diese PIN. (Sie ist nur fünf Minuten lang gültig.)
- Fügen Sie die PIN in der Verwaltungskonsole (Client-App) ein, und klicken Sie anschließend auf **OK**.

  ![Sicherheits-PIN](./media/backup-azure-delete-vault/security-pin.png)

- Im Assistenten **Sicherungsstatus ändern** wird folgender Hinweis angezeigt: *Gelöschte Sicherungsdaten werden 14 Tage lang aufbewahrt. Danach werden Sicherungsdaten dauerhaft gelöscht.*  

    ![Löschen der Sicherungsinfrastruktur](./media/backup-azure-delete-vault/deleted-backup-data.png)

Nachdem Sie die Sicherungselemente aus Ihrer lokalen Umgebung gelöscht haben, können Sie die nächsten Schritte über das Portal ausführen.

### <a name="delete-backup-items-from-mabs-management-console"></a>Löschen von Sicherungselementen über die MABS-Verwaltungskonsole

So löschen Sie Sicherungselemente über die MABS-Verwaltungskonsole

**Methode 1** Führen Sie diese Schritte aus, um den Schutz zu beenden und die Sicherungsdaten zu löschen:

1.  Klicken Sie in der DPM-Verwaltungskonsole auf der Navigationsleiste auf **Schutz**.
2.  Wählen Sie im Anzeigebereich das Schutzgruppenmitglied aus, das Sie entfernen möchten. Klicken Sie mit der rechten Maustaste, und wählen Sie die Option **Stop Protection of Group Members** (Schutz von Gruppenmitgliedern beenden) aus.
3.  Aktivieren Sie im Dialogfeld **Schutz beenden** das Kontrollkästchen **Geschützte Daten löschen** > **Speicher online löschen**, und klicken Sie anschließend auf **Schutz beenden**.

    ![Löschen des Onlinespeichers](./media/backup-azure-delete-vault/delete-storage-online.png)

Der Status des geschützten Mitglieds ändert sich daraufhin in **Inaktives Replikat verfügbar**.

4. Klicken Sie mit der rechten Maustaste auf die inaktive Schutzgruppe, und wählen Sie **Inaktiven Schutz entfernen** aus.

    ![Entfernen des inaktiven Schutzes](./media/backup-azure-delete-vault/remove-inactive-protection.png)

5. Aktivieren Sie im Fenster **Inaktiven Schutz entfernen** das Kontrollkästchen **Speicher online löschen**, und klicken Sie auf **OK**.

    ![Entfernen von Replikaten auf dem Datenträger und online](./media/backup-azure-delete-vault/remove-replica-on-disk-and-online.png)

**Methode 2** Starten Sie die **MABS-Verwaltungskonsole**. Deaktivieren Sie im Abschnitt **Datenschutzmethode auswählen** das Kontrollkästchen **Ich möchte Onlineschutz**.

  ![Auswählen der Datenschutzmethode](./media/backup-azure-delete-vault/data-protection-method.png)

Nachdem Sie die Sicherungselemente aus Ihrer lokalen Umgebung gelöscht haben, können Sie die nächsten Schritte über das Portal ausführen.


## <a name="delete-the-recovery-services-vault"></a>Löschen des Recovery Services-Tresors

1. Nachdem alle Abhängigkeiten entfernt wurden, scrollen Sie im Tresormenü zum Bereich **Zusammenfassung**.
2. Vergewissern Sie sich, dass keine **Sicherungselemente**, **Sicherungsverwaltungsserver** oder **Replizierte Elemente** aufgelistet werden. Wenn das Element weiterhin im Tresor angezeigt wird, lesen Sie die Informationen im Abschnitt [Vorbereitung](#before-you-start).

3. Wenn sich keine Elemente mehr in Tresor befinden, klicken Sie im Tresor-Dashboard auf **Löschen**.

    ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

4. Klicken Sie auf **Ja**, um das Löschen des Tresors zu bestätigen. Der Tresor wird gelöscht, und Sie gelangen im Portal zurück zum Dienstmenü **Neu** .

## <a name="delete-the-recovery-services-vault-using-azure-resource-manager-client"></a>Löschen des Recovery Services-Tresors mit dem Azure Resource Manager-Client

Diese Option zum Löschen des Recovery Services-Tresors wird nur empfohlen, wenn alle Abhängigkeiten entfernt wurden und Sie den *Fehler beim Löschen von Tresor* trotzdem erhalten.

- Vergewissern Sie sich im Bereich **Zusammenfassung** im Tresormenü, dass keine **Sicherungselemente**, **Sicherungsverwaltungsserver** oder **Replizierten Elemente** aufgelistet sind. Wenn Sicherungselemente vorhanden sind, lesen Sie die Informationen im Abschnitt [Vorbereitung](#before-you-start).
- Versuchen Sie erneut, [den Tresor über das Portal zu löschen](#delete-the-recovery-services-vault).
- Wenn alle Abhängigkeiten entfernt wurden und der *Fehler beim Löschen von Tresor* weiterhin auftritt, sollten Sie das ARMClient-Tool zum Ausführen der unten angegebenen Schritte verwenden:

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

1. Installieren Sie chocolatey von [hier](https://chocolatey.org/) aus, und installieren Sie ARMClient mit folgendem Befehl:

   `choco install armclient --source=https://chocolatey.org/api/v2/`
2. Melden Sie sich bei Ihrem Azure-Konto an, und führen Sie den folgenden Befehl aus:

    `ARMClient.exe login [environment name]`

3. Suchen Sie im Azure-Portal nach der Abonnement-ID und dem Ressourcengruppennamen für den Tresor, den Sie löschen möchten.

Weitere Informationen zum Befehl ARMClient finden Sie in diesem [Dokument](https://github.com/projectkudu/ARMClient/blob/master/README.md).

### <a name="use-azure-resource-manager-client-to-delete-recovery-services-vault"></a>Löschen des Recovery Services-Tresors mit dem Azure Resource Manager

1. Führen Sie mit Ihrer Abonnement-ID, dem Namen der Ressourcengruppe und dem Namen des Tresors den folgenden Befehl aus. Wenn Sie den Befehl ausführen, wird der Tresor gelöscht, sofern keine Abhängigkeiten vorliegen.

   ```
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>?api-version=2015-03-15
   ```
2. Wenn der Tresor nicht leer ist, wird die folgende Fehlermeldung angezeigt: „Der Tresor kann nicht gelöscht werden, weil darin Ressourcen enthalten sind.“ Gehen Sie folgendermaßen vor, um in einem Tresor enthaltene geschützte Elemente / Container zu entfernen:

   ```
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>/registeredIdentities/<container name>?api-version=2016-06-01
   ```

3. Vergewissern Sie sich im Azure-Portal, dass der Tresor gelöscht wird.


## <a name="next-steps"></a>Nächste Schritte

[Übersicht über Recovery Services-Tresore](backup-azure-recovery-services-vault-overview.md)<br/>
[Überwachen und Verwalten von Recovery Services-Tresoren](backup-azure-manage-windows-server.md)
