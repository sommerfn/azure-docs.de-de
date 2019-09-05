---
title: Löschen eines Microsoft Azure Backup-Recovery Services-Tresors
description: In diesem Artikel wird beschrieben, wie Sie einen Microsoft Azure Backup-Recovery Services-Tresor löschen.
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: dacurwin
ms.openlocfilehash: 3d6d374b6e516180ec488fe4de1317a3c99a7f7c
ms.sourcegitcommit: bba811bd615077dc0610c7435e4513b184fbed19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2019
ms.locfileid: "70050109"
---
# <a name="delete-an-azure-backup-recovery-services-vault"></a>Löschen eines Azure Backup-Recovery Services-Tresors

In diesem Artikel wird beschrieben, wie Sie einen Microsoft [Azure Backup](backup-overview.md)-Recovery Services-Tresor (MARS) löschen. Außerdem enthält er Anweisungen zum Entfernen von Abhängigkeiten und anschließenden Löschen eines Tresors.


## <a name="before-you-start"></a>Vorbereitung

Sie können keinen Recovery Services-Tresor löschen, der über Abhängigkeiten verfügt, z.B. geschützte Server oder Server für die Sicherungsverwaltung, die ihm zugeordnet sind.

- Tresore mit Sicherungsdaten können nicht gelöscht werden (selbst wenn Sie den Schutz beendet und die Sicherungsdaten beibehalten haben).

- Wenn Sie einen Tresor löschen, der Abhängigkeiten enthält, wird die folgende Meldung angezeigt:

  ![Fehler beim Löschen des Tresors](./media/backup-azure-delete-vault/error.png)

- Wenn Sie ein lokales geschütztes Element aus einem Portal löschen, das Abhängigkeiten enthält, wird eine Warnmeldung angezeigt:

  ![Serverfehler beim Löschen eines geschützten Elements](./media/backup-azure-delete-vault/error-message.jpg)

  
Wählen Sie zum Löschen des Tresors das Szenario, das Ihrem Setup entspricht, und führen Sie die empfohlenen Schritte aus:

Szenario | Schritte zum Entfernen von Abhängigkeiten beim Löschen eines Tresors |
-- | --
Ich verfüge über lokale Dateien und Ordner, die per Azure Backup-Agent geschützt sind und in Azure gesichert werden. | Führen Sie die Schritte unter [Löschen von Sicherungselementen über die MARS-Verwaltungskonsole](#delete-backup-items-from-the-mars-management-console) aus.
Ich verfüge über lokale Computer, die per MABS (Microsoft Azure Backup Server) oder DPM (System Center Data Protection Manager) in Azure geschützt sind. | Führen Sie die Schritte unter [Löschen von Sicherungselementen über die MABS-Verwaltungskonsole](#delete-backup-items-from-the-mabs-management-console) aus.
Ich verfüge über geschützte Elemente in der Cloud (z.B. einen virtuellen IaaS-Computer oder eine Azure Files-Freigabe).  | Führen Sie die Schritte unter [Löschen von geschützten Elementen in der Cloud](#delete-protected-items-in-the-cloud) aus.
Ich verfüge sowohl lokal als auch in der Cloud über geschützte Elemente. | Führen Sie die Schritte in den folgenden Abschnitten in der angegebenen Reihenfolge aus: <br> 1. [Löschen von geschützten Elementen in der Cloud](#delete-protected-items-in-the-cloud)<br> 2. [Löschen von Sicherungselementen über die MARS-Verwaltungskonsole](#delete-backup-items-from-the-mars-management-console) <br> 3. [Löschen von Sicherungselementen über die MABS-Verwaltungskonsole](#delete-backup-items-from-the-mabs-management-console)
Ich verfüge weder lokal noch in der Cloud über geschützte Elemente, erhalte aber immer noch den „Fehler beim Löschen von Tresor“. | Führen Sie die Schritte unter [Löschen des Recovery Services-Tresors mit Azure Resource Manager](#delete-the-recovery-services-vault-by-using-azure-resource-manager) aus.


## <a name="delete-protected-items-in-the-cloud"></a>Löschen von geschützten Elementen in der Cloud

Lesen Sie zuerst den Abschnitt **[Vorbereitung](#before-you-start)** , um sich mit den Abhängigkeiten und dem Prozess zum Löschen des Tresors vertraut zu machen.

Führen Sie die folgenden Schritte aus, um den Schutz zu beenden und die Sicherungsdaten zu löschen:

1. Navigieren Sie im Portal zu **Recovery Services-Tresor** und dann zu **Sicherungselemente**. Wählen Sie anschließend die geschützten Elemente in der Cloud aus (z.B. Azure Virtual Machines, Azure Storage [den Azure Files-Dienst] oder SQL Server auf Azure Virtual Machines).

    ![Auswählen des Sicherungstyps](./media/backup-azure-delete-vault/azure-storage-selected.png)

2. Klicken Sie mit der rechten Maustaste, um das Sicherungselement auszuwählen. Je nachdem, ob das Sicherungselement geschützt ist oder nicht, wird im Menü entweder der Bereich **Sicherung beenden** oder der Bereich **Sicherungsdaten löschen** angezeigt.

    - Wenn der Bereich **Sicherung beenden** angezeigt wird, wählen Sie im Dropdownmenü die Option **Sicherungsdaten löschen** aus. Geben Sie den Namen des Sicherungselements ein (bei diesem Feld muss die Groß-/Kleinschreibung beachtet werden), und wählen Sie dann im Dropdownmenü einen Grund aus. Geben Sie ggf. Ihre Kommentare ein. Wählen Sie dann **Sicherung beenden** aus.

        ![Bereich „Sicherung beenden“](./media/backup-azure-delete-vault/stop-backup-item.png)

    - Wenn der Bereich **Sicherungsdaten löschen** angezeigt wird, geben Sie den Namen des Sicherungselements ein (bei diesem Feld muss die Groß-/Kleinschreibung beachtet werden), und wählen Sie dann im Dropdownmenü einen Grund aus. Geben Sie ggf. Ihre Kommentare ein. Wählen Sie dann **Löschen** aus. 

         ![Bereich „Sicherungsdaten löschen“](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

5. Überprüfen Sie das Symbol **Benachrichtigung**: ![Benachrichtigungssymbol](./media/backup-azure-delete-vault/messages.png) Nachdem der Vorgang abgeschlossen ist, zeigt der Dienst die folgende Meldung an: *Sicherung beenden und Sicherungsdaten für „* Sicherungselement *“ löschen*. *Der Vorgang wurde erfolgreich abgeschlossen.*
6. Wählen Sie im Menü **Sicherungselemente** die Option **Aktualisieren** aus, um sich zu vergewissern, dass das Sicherungselement gelöscht wurde.

      ![Seite „Sicherungselemente löschen“](./media/backup-azure-delete-vault/empty-items-list.png)

## <a name="delete-protected-items-on-premises"></a>Löschen von geschützten lokalen Elementen

Lesen Sie zuerst den Abschnitt **[Vorbereitung](#before-you-start)** , um sich mit den Abhängigkeiten und dem Prozess zum Löschen des Tresors vertraut zu machen.

1. Wählen Sie im Menü des Tresordashboards die Option **Sicherungsinfrastruktur** aus.
2. Wählen Sie je nach lokalem Szenario eine der folgenden Optionen aus:

      - Wählen Sie für MARS die Option **Geschützte Server** und dann **Azure Backup-Agent** aus. Wählen Sie anschließend den Server aus, den Sie löschen möchten. 

        ![MARS: Auswählen des Tresors zum Öffnen des zugehörigen Dashboards](./media/backup-azure-delete-vault/identify-protected-servers.png)

      - Wählen Sie für MABS oder DPM die Option  **Sicherungsverwaltungsserver** aus. Wählen Sie anschließend den Server aus, den Sie löschen möchten. 


          ![MABS: Auswählen des Tresors zum Öffnen des zugehörigen Dashboards](./media/backup-azure-delete-vault/delete-backup-management-servers.png)

3. Der Bereich **Löschen** wird geöffnet und zeigt eine Warnmeldung an.

     ![Bereich „Löschen“](./media/backup-azure-delete-vault/delete-protected-server.png)

     Lesen Sie die Warnmeldung und die Anweisungen im Kontrollkästchen für die Zustimmung.
    > [!NOTE]
    >- Wenn der geschützte Server mit Azure-Diensten synchronisiert wird und Sicherungselemente vorhanden sind, werden im Kontrollkästchen für die Zustimmung die Anzahl von abhängigen Sicherungselementen und der Link angezeigt, unter dem die Sicherungselemente angezeigt werden können.
    >- Wenn der geschützte Server nicht mit Azure-Diensten synchronisiert wird und Sicherungselemente vorhanden sind, wird im Kontrollkästchen für die Zustimmung nur die Anzahl von Sicherungselementen angezeigt.
    >- Wenn keine Sicherungselemente vorhanden sind, zeigt das Kontrollkästchen für die Zustimmung eine Aufforderung zum Löschen an.

4. Aktivieren Sie das Kontrollkästchen für die Zustimmung, und klicken Sie dann auf **Löschen**.




5. Überprüfen Sie das Symbol **Benachrichtigung** ![Sicherungsdaten löschen](./media/backup-azure-delete-vault/messages.png). Nach Abschluss des Vorgangs zeigt der Dienst die folgende Meldung an: *Sicherung beenden und Sicherungsdaten für „Sicherungselement“ löschen.* *Der Vorgang wurde erfolgreich abgeschlossen.*
6. Wählen Sie im Menü **Sicherungselemente** die Option **Aktualisieren** aus, um sich zu vergewissern, dass das Sicherungselement gelöscht wurde.

Nach Abschluss dieses Vorgangs können Sie die Sicherungselemente über die Verwaltungskonsole löschen:
    
   - [Löschen von Sicherungselementen über die MARS-Verwaltungskonsole](#delete-backup-items-from-the-mars-management-console)
    - [Löschen von Sicherungselementen über die MABS-Verwaltungskonsole](#delete-backup-items-from-the-mabs-management-console)


### <a name="delete-backup-items-from-the-mars-management-console"></a>Löschen von Sicherungselementen über die MARS-Verwaltungskonsole

1. Öffnen Sie die MARS-Verwaltungskonsole, navigieren Sie zum Bereich **Aktionen**, und wählen Sie **Sicherung planen** aus.
2. Wählen Sie auf der Seite **Geplante Sicherung ändern oder beenden** die Option **Sicherungszeitplan nicht länger verwenden und alle gespeicherten Sicherungen löschen** aus. Klicken Sie anschließend auf **Weiter**.

    ![Ändern oder Beenden einer geplanten Sicherung](./media/backup-azure-delete-vault/modify-schedule-backup.png)

3. Wählen Sie auf der Seite **Geplante Sicherung beenden** die Option **Fertig stellen** aus.

    ![Beenden einer geplanten Sicherung](./media/backup-azure-delete-vault/stop-schedule-backup.png)
4. Sie werden aufgefordert, eine Sicherheits-PIN (persönliche Identifikationsnummer) einzugeben, die Sie manuell generieren müssen. Dazu melden Sie sich zunächst beim Azure-Portal an.
5. Navigieren Sie zu **Recovery Services-Tresor** > **Einstellungen** > **Eigenschaften**.
6. Wählen Sie unter **Sicherheits-PIN** die Option **Generieren** aus. Kopieren Sie diese PIN. Die PIN ist nur fünf Minuten lang gültig.
7. Fügen Sie die PIN in der Verwaltungskonsole ein, und klicken Sie anschließend auf **OK**.

    ![Generieren einer Sicherheits-PIN](./media/backup-azure-delete-vault/security-pin.png)

8. Auf der Seite **Sicherungsstatus ändern** wird die folgende Meldung angezeigt: *Gelöschte Sicherungsdaten werden 14 Tage lang aufbewahrt. Danach werden Sicherungsdaten dauerhaft gelöscht.*  

    ![Löschen der Sicherungsinfrastruktur](./media/backup-azure-delete-vault/deleted-backup-data.png)

Nachdem Sie die lokalen Sicherungselemente gelöscht haben, führen Sie die nächsten Schritte im Portal aus.

### <a name="delete-backup-items-from-the-mabs-management-console"></a>Löschen von Sicherungselementen über die MABS-Verwaltungskonsole

Es gibt zwei Methoden zum Löschen von Sicherungselementen über die MABS-Verwaltungskonsole.

#### <a name="method-1"></a>Methode 1
Führen Sie die folgenden Schritte aus, um den Schutz zu beenden und die Sicherungsdaten zu löschen:

1.  Öffnen Sie die DPM-Verwaltungskonsole, und wählen Sie dann auf der Navigationsleiste die Option **Schutz** aus.
2.  Wählen Sie im Anzeigebereich das Schutzgruppenmitglied aus, das Sie entfernen möchten. Klicken Sie mit der rechten Maustaste, und wählen Sie **Schutz von Gruppenmitgliedern beenden** aus.
3.  Aktivieren Sie im Dialogfeld **Schutz beenden** das Kontrollkästchen **Geschützte Daten löschen** und anschließend das Kontrollkästchen **Speicher online löschen**. Wählen Sie dann **Schutz beenden** aus.

    ![Auswählen von „Geschützte Daten löschen“ im Bereich „Schutz beenden“](./media/backup-azure-delete-vault/delete-storage-online.png)

    Der Status des geschützten Mitglieds ändert sich in *Inaktives Replikat verfügbar*.

4. Klicken Sie mit der rechten Maustaste auf die inaktive Schutzgruppe, und wählen Sie **Inaktiven Schutz entfernen** aus.

    ![Entfernen des inaktiven Schutzes](./media/backup-azure-delete-vault/remove-inactive-protection.png)

5. Aktivieren Sie im Fenster **Inaktiven Schutz entfernen** das Kontrollkästchen **Speicher online löschen**, und klicken Sie dann auf **OK**.

    ![Löschen des Onlinespeichers](./media/backup-azure-delete-vault/remove-replica-on-disk-and-online.png)

#### <a name="method-2"></a>Methode 2
Öffnen Sie die **MABS-Verwaltungskonsole**. Deaktivieren Sie unter **Datenschutzmethode auswählen** das Kontrollkästchen **Ich möchte Onlineschutz**.

  ![Wählen Sie die Methoden zum Schützen der Daten aus.](./media/backup-azure-delete-vault/data-protection-method.png)

Nachdem Sie die lokalen Sicherungselemente gelöscht haben, führen Sie die nächsten Schritte im Portal aus.


## <a name="delete-the-recovery-services-vault"></a>Löschen des Recovery Services-Tresors

1. Nachdem alle Abhängigkeiten entfernt wurden, scrollen Sie im Tresormenü zum Bereich **Zusammenfassung**.
2. Vergewissern Sie sich, dass keine Sicherungselemente, Sicherungsverwaltungsserver oder replizierte Elemente aufgelistet sind. Wenn weiterhin Elemente im Tresor angezeigt werden, lesen Sie die Informationen im Abschnitt [Vorbereitung](#before-you-start).

3. Wenn sich keine Elemente mehr im Tresor befinden, klicken Sie im Tresordashboard auf **Löschen**.

    ![Auswählen von „Löschen“ auf dem Tresordashboard](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

4. Wählen Sie **Ja** aus, um das Löschen des Tresors zu bestätigen. Der Tresor wird gelöscht. Sie werden im Portal zum Dienstmenü **Neu** zurückgeführt.

## <a name="delete-the-recovery-services-vault-by-using-azure-resource-manager"></a>Löschen des Recovery Services-Tresors mit Azure Resource Manager

Diese Option zum Löschen des Recovery Services-Tresors wird nur empfohlen, wenn alle Abhängigkeiten entfernt wurden und Sie den *Fehler beim Löschen von Tresor* trotzdem erhalten. Probieren Sie einen oder alle der folgenden Tipps aus:

- Vergewissern Sie sich im Bereich **Zusammenfassung** im Tresormenü, dass keine Sicherungselemente, Sicherungsverwaltungsserver oder replizierten Elemente aufgelistet sind. Wenn Sicherungselemente vorhanden sind, lesen Sie die Informationen im Abschnitt [Vorbereitung](#before-you-start).
- Versuchen Sie erneut, [den Tresor über das Portal zu löschen](#delete-the-recovery-services-vault).
- Wenn alle Abhängigkeiten entfernt wurden und der *Fehler beim Löschen von Tresor* weiterhin auftritt, verwenden Sie das ARMClient-Tool zum Ausführen der folgenden Schritte (nach dem Hinweis).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

1. Wechseln Sie zu [chocolatey.org](https://chocolatey.org/), um Chocolatey herunterzuladen und zu installieren. Dann installieren Sie ARMClient, indem Sie den folgenden Befehl ausführen:

   `choco install armclient --source=https://chocolatey.org/api/v2/`
2. Melden Sie sich bei Ihrem Azure-Konto an, und führen Sie anschließend den folgenden Befehl aus:

    `ARMClient.exe login [environment name]`

3. Suchen Sie im Azure-Portal nach der Abonnement-ID und dem Ressourcengruppennamen für den Tresor, den Sie löschen möchten.

Weitere Informationen zum ARMClient-Befehl finden Sie unter [ARMClient README](https://github.com/projectkudu/ARMClient/blob/master/README.md).

### <a name="use-the-azure-resource-manager-client-to-delete-a-recovery-services-vault"></a>Löschen eines Recovery Services-Tresors mit dem Azure Resource Manager-Client

1. Führen Sie den folgenden Befehl mit Ihrer Abonnement-ID, dem Namen der Ressourcengruppe und dem Namen des Tresors aus. Wenn keine Abhängigkeiten vorliegen, wird der Tresor beim Ausführen des folgenden Befehls gelöscht:

   ```azurepowershell
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>?api-version=2015-03-15
   ```
2. Ist der Tresor nicht leer, erhalten Sie die folgende Fehlermeldung: *Der Tresor kann nicht gelöscht werden, weil sich im Tresor vorhandene Ressourcen befinden.* Führen Sie den folgenden Befehl aus, um in einem Tresor enthaltene geschützte Elemente oder Container zu entfernen:

   ```azurepowershell
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>/registeredIdentities/<container name>?api-version=2016-06-01
   ```

3. Vergewissern Sie sich im Azure-Portal, dass der Tresor gelöscht wurde.


## <a name="next-steps"></a>Nächste Schritte

[Übersicht über Recovery Services-Tresore](backup-azure-recovery-services-vault-overview.md)<br/>
[Überwachen und Verwalten von Recovery Services-Tresoren](backup-azure-manage-windows-server.md)
