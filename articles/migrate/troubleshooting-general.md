---
title: Behandeln von Problemen bei Azure Migrate | Microsoft-Dokumentation
description: Bietet eine Übersicht über bekannte Probleme im Azure Migrate-Dienst sowie Problembehandlungstipps für häufige Fehler.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 08/06/2019
ms.author: raynew
ms.openlocfilehash: fa1e7fcf89ccc06e429831191ba5dfce3cf33797
ms.sourcegitcommit: 3073581d81253558f89ef560ffdf71db7e0b592b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2019
ms.locfileid: "68828316"
---
# <a name="troubleshoot-azure-migrate"></a>Problembehandlung für Azure Migrate

[Azure Migrate](migrate-services-overview.md) stellt einen Hub mit Tools für die Bewertung und Migration sowie unabhängige Drittanbietertools bereit. Dieser Artikel hilft Ihnen bei der Behandlung von Fehlern in Azure Migrate, Azure Migrate-Serverbewertung und Azure Migrate-Servermigration.

## <a name="azure-migrate-project-issues"></a>Probleme mit Azure Migrate-Projekten

### <a name="i-cant-find-my-existing-azure-migrate-project"></a>Ich kann mein vorhandenes Azure Migrate-Projekt nicht finden.

Es sind [zwei Versionen](https://docs.microsoft.com/azure/migrate/migrate-services-overview#azure-migrate-versions) von Azure Migrate verfügbar. Je nachdem, in welcher Version Sie das Projekt angelegt haben, können Sie eine der folgenden Methoden verwenden:

* Wenn Sie nach einem Projekt suchen, das mit der vorherigen Version (Vorgängerversion) von Azure Migrate erstellt wurde, führen Sie diese Schritte aus, um das Projekt zu finden:

    1. Wechseln Sie zum [Azure-Portal](https://portal.azure.com) und suchen Sie dort nach **Azure Migrate**.
    2. Im Azure Migrate-Dashboard sehen Sie ein Banner, das den Zugriff auf ältere Projekte erwähnt. Dieses Banner erscheint nur, wenn Sie ein Projekt mit der alten Version erstellt haben. Wählen Sie das Banner aus.

       ![Zugriff auf vorhandene Projekte](./media/troubleshooting-general/access-existing-projects.png)

    3. Sie sehen nun die Liste der vorhandenen Projekte, die mit der Vorgängerversion von Azure Migrate erstellt wurden.

* Wenn Sie nach einem Projekt suchen, das mit der aktuellen Version (neue Version) von Azure Migrate erstellt wurde, führen Sie diese Schritte aus, um das Projekt zu finden:

    1. Wechseln Sie zum [Azure-Portal](https://portal.azure.com) und suchen Sie dort nach **Azure Migrate**.
    2. Rufen Sie im Dashboard von Azure Migrate die Seite **Server** links auf und wählen Sie oben rechts in der Ecke **Ändern** in der rechten oberen Ecke.

       ![Wechseln zu einem vorhandenen Azure Migrate-Projekt](./media/troubleshooting-general/switch-project.png)

    3. Wählen Sie das entsprechende Abonnement und das Azure Migrate-Projekt aus.

### <a name="how-do-i-create-a-second-azure-migrate-project"></a>Wie erstelle ich ein zweites Azure Migrate-Projekt?

Führen Sie diese Schritte aus, um ein neues Azure Migrate-Projekt zu erstelle:

1. Wechseln Sie zum [Azure-Portal](https://portal.azure.com) und suchen Sie dort nach **Azure Migrate**.
2. Rufen Sie im Dashboard von Azure Migrate die Seite **Server** links auf und wählen Sie oben rechts in der Ecke **Ändern** in der rechten oberen Ecke.

   ![Ändern eines Azure Migrate-Projekts](./media/troubleshooting-general/switch-project.png)

3. Wählen Sie zum Erstellen eines neuen Projekts **Hier klicken** aus.

   ![Erstellen eines zweiten Azure Migrate-Projekts](./media/troubleshooting-general/create-new-project.png)

### <a name="which-azure-geographies-does-azure-migrate-support"></a>Welche Azure-Geografien werden von Azure Migrate unterstützt?

Weitere Informationen finden Sie in den Listen für [VMware](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#azure-migrate-projects) und für [Hyper-V](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-hyper-v#azure-migrate-projects).

### <a name="how-do-i-delete-azure-migrate-projects-and-associated-log-analytics-workspaces"></a>Wie lösche ich Azure Migrate-Projekte und zugehörige Log Analytics-Arbeitsbereiche?

Wenn Sie ein Azure Migrate-Projekt löschen, wird das Migrationsprojekt *zusammen mit* den Metadaten zu den ermittelten Computern gelöscht. Wenn Sie dem Serverbewertungstool jedoch einen Log Analytics-Arbeitsbereich angefügt hatten, wird dieser nicht automatisch gelöscht. (Derselbe Log Analytics-Arbeitsbereich kann für mehrere Anwendungsfälle verwendet werden.) Wenn Sie auch den Log Analytics-Arbeitsbereich löschen möchten, müssen Sie dies manuell tun:

1. Navigieren Sie zu dem Log Analytics-Arbeitsbereich, der dem Projekt angefügt ist.
     * Wenn Sie das Migrationsprojekt noch nicht gelöscht haben, finden Sie den Link zum Arbeitsbereich auf der Übersichtsseite der Serverbewertung im Abschnitt „Essentials“.

       ![LA-Arbeitsbereich](./media/troubleshooting-general/loganalytics-workspace.png)

     * Wenn Sie das Migrationsprojekt bereits gelöscht haben, wählen Sie im linken Bereich des Azure-Portals **Ressourcengruppen**. Rufen Sie die Ressourcengruppe auf, in der der Arbeitsbereich erstellt wurde, und navigieren Sie dann dorthin.
2. Befolgen Sie die Anleitungen in [Löschen eines Log Analytics-Arbeitsbereichs mit dem Azure-Portal](https://docs.microsoft.com/azure/azure-monitor/platform/delete-workspace).

### <a name="migration-project-creation-failed-with-a-requests-must-contain-user-identity-headers-error"></a>Erstellen des Migrationsprojekts mit dem Fehler „Anforderungen müssen Benutzeridentitätsheader enthalten.“

Dieses Problem kann für Benutzer auftreten, die nicht über Zugriff auf den Azure Active Directory (Azure AD)-Mandanten der Organisation verfügen. Wenn ein Benutzer zum ersten Mal zu einem Azure AD-Mandanten hinzugefügt wird, erhält er eine E-Mail-Einladung, dem Mandanten beizutreten. Benutzer müssen die Einladung annehmen, um erfolgreich zum Mandanten hinzugefügt zu werden. Wenn Sie die E-Mail nicht sehen können, wenden Sie sich an einen Benutzer, der bereits auf den Mandanten zugreifen kann, und bitten Sie ihn, die Einladung anhand der unter [Erneutes Senden von Einladungen an Gastbenutzer](https://docs.microsoft.com/azure/active-directory/b2b/add-users-administrator#resend-invitations-to-guest-users) angegebenen Schritte erneut zu senden.

Nachdem Sie die Einladung per E-Mail erhalten haben, müssen Sie die E-Mail öffnen und den Link auswählen, um die Einladung anzunehmen. Dann müssen Sie sich beim Azure-Portal abmelden und wieder anmelden. (Das Aktualisieren des Browsers funktioniert hierbei nicht.) Sie können dann mit dem Erstellen des Migrationsprojekts beginnen.

## <a name="appliance-issues"></a>Appliance-Probleme

### <a name="deployment-of-an-azure-migrate-appliance-for-vmware-failed-with-a-the-provided-manifest-file-is-invalid-invalid-ovf-manifest-entry-error"></a>Die Bereitstellung einer Azure Migrate-Appliance für VMware schlägt mit folgendem Fehler fehl: „Die angegebene Manifestdatei ist ungültig: Ungültiger OVF-Manifesteintrag.“.

1. Überprüfen Sie, ob die OVA-Datei für die Azure Migrate-Appliance ordnungsgemäß heruntergeladen wird, indem Sie deren Hashwert überprüfen. Eine Anleitung finden Sie unter [Vorbereiten von VMware-VMs für die Bewertung und die Migration zu Azure](https://docs.microsoft.com/azure/migrate/tutorial-assessment-vmware). Wenn der Hashwert nicht übereinstimmt, laden Sie die OVA-Datei erneut herunter, und wiederholen Sie die Bereitstellung.
2. Wenn die Bereitstellung weiterhin fehlschlägt und Sie zum Bereitstellen der OVF-Datei VMware vSphere-Client verwenden, führen Sie die Bereitstellung über den vSphere-Webclient aus. Wenn die Bereitstellung weiterhin fehlschlägt, versuchen Sie es mit einem anderen Webbrowser.
3. Wenn Sie den vSphere-Webclient und die Bereitstellung auf vCenter Server 6.5 oder 6.7 ausführen möchten, stellen Sie die OVA-Datei mit diesen Schritten direkt auf dem ESXi-Host bereit:
   - Stellen Sie mithilfe des Webclients (https://<*Host-IP-Adresse*>/ui) eine direkte Verbindung mit dem ESXi-Host her (anstelle von vCenter Server).
   - Wechseln Sie zu **Home** > **Inventar**.
   - Wählen Sie **Datei** > **OVF-Vorlage bereitstellen** aus. Navigieren Sie zur OVA und schließen Sie die Bereitstellung ab.
4. Wenn die Bereitstellung weiterhin fehlschlägt, wenden Sie sich an den Azure Migrate-Support.

### <a name="the-appliance-cant-connect-to-the-internet"></a>Die Appliance kann keine Verbindung mit dem Internet herstellen.

Dieses Verhalten kann auftreten, wenn sich der Computer, den Sie verwenden, hinter einem Proxy befindet. Stellen Sie sicher, dass Sie die Anmeldeinformationen für die Autorisierung angeben, wenn der Proxy diese benötigt.
Wenn Sie URL-basierte Firewallproxys zur Steuerung ausgehender Verbindungen verwenden, stellen Sie sicher, dass die folgenden erforderlichen Dienst-URLs zur Liste der zugelassenen Dienste hinzugefügt werden:

- [Serverbewertung für VMware](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#assessment-url-access-requirements)
- [Serverbewertung für Hyper-V](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-hyper-v#assessment-appliance-url-access)
- [Servermigration für VMware (ohne Agent)](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#agentless-migration-url-access-requirements)
- [Servermigration für VMware (mit Agent)](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#replication-appliance-url-access)
- [Servermigration für Hyper-V](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-hyper-v#migration-hyper-v-host-url-access)

Wenn Sie einen abfangenden Proxy verwenden, um eine Internetverbindung herzustellen, dann müssen Sie das Proxyzertifikat in die Appliance-VM importieren. Sie können das Proxyzertifikat importieren, indem Sie die in [Azure Migrate-Appliance](https://docs.microsoft.com/azure/migrate/concepts-collector) beschriebenen Schritte befolgen.

### <a name="error-802-date-and-time-synchronization-error"></a>Fehler 802: Fehler bei der Datums- und Zeitsynchronisierung.

Möglicherweise weicht die Serveruhr um mehr als fünf Minuten von der aktuellen Uhrzeit ab. Passen Sie die Uhrzeit auf dem virtuellen Collector-Computer folgendermaßen an die aktuelle Uhrzeit an:

1. Öffnen Sie auf dem virtuellen Computer eine Administratoreingabeaufforderung.
2. Führen Sie **w32tm /tz** aus, um die Zeitzone zu überprüfen.
3. Führen Sie **w32tm /resync** aus, um die Zeit zu synchronisieren.


###  <a name="connection-failed-error-unabletoconnecttoserver"></a>Fehler bei der Verbindung. Fehler UnableToConnectToServer.

Möglicherweise können Sie keine Verbindung mit vCenter Server „*Servername*.com:9443“ herstellen. Die Fehlerdetails weisen darauf hin, dass unter „https://*Servername*.com:9443/sdk“ kein Endpunkt lauscht, der die Nachricht akzeptieren könnte.

Überprüfen Sie in diesem Fall, ob Sie die neueste Version der Collector-Appliance ausführen. Ist dies nicht der Fall, führen Sie ein Upgrade der Appliance auf die [neueste Version](https://docs.microsoft.com/azure/migrate/concepts-collector) durch.

Wenn das Problem auch bei der neuesten Version weiterhin auftritt, ist der Collectorcomputer möglicherweise nicht in der Lage, den angegebenen vCenter Server-Namen auszulösen, oder der angegebene Port ist eventuell falsch. Wenn der Port nicht angegeben ist, versucht Collector standardmäßig, eine Verbindung mit Port 443 herzustellen.

1. Versuchen Sie, „*Servername*.com“ vom Collectorcomputer aus per Ping zu erreichen.
2. Wenn Schritt 1 nicht erfolgreich ist, können Sie versuchen, eine Verbindung mit vCenter Server über die IP-Adresse herzustellen.
3. Ermitteln Sie die richtige Portnummer für die Verbindungsherstellung mit vCenter.
4. Überprüfen Sie, ob vCenter Server ausgeführt wird und betriebsbereit ist.


### <a name="the-appliance-might-not-be-registered-successfully-to-the-azure-migrate-project-error-id-60052"></a>Möglicherweise wurde die Appliance nicht erfolgreich beim Azure Migrate-Projekt registriert. (Fehler-ID: 60052).

Dieser Fehler tritt auf, wenn das Azure-Konto, mit dem die Appliance registriert wurde, über unzureichende Berechtigungen verfügt. Stellen Sie sicher, dass das für die Registrierung der Appliance verwendete Azure-Benutzerkonto mindestens über „Mitwirkender“-Berechtigungen für das Abonnement verfügt. Weitere Informationen zu den erforderlichen Azure-Rollen und -Berechtigungen finden Sie [hier](https://docs.microsoft.com/azure/migrate/migrate-appliance#appliance-deployment-requirements).

### <a name="the-appliance-might-not-be-registered-successfully-to-the-azure-migrate-project-error-id-60039"></a>Möglicherweise wurde die Appliance nicht erfolgreich beim Azure Migrate-Projekt registriert. (Fehler-ID: 60039).

Möglicherweise wird das Azure Migrate-Projekt, das für die Registrierung der Appliance ausgewählt wurde, nicht gefunden. In diesem Fall schlägt die Registrierung fehl. Überprüfen Sie im Azure-Portal, ob das Projekt in der Ressourcengruppe vorhanden ist. Sollte das Projekt nicht vorhanden sein, erstellen Sie in Ihrer Ressourcengruppe ein neues Azure Migrate-Projekt, und registrieren Sie die Appliance erneut. Weitere Informationen zum Erstellen eines neuen Azure Migrate-Projekts finden Sie [hier](https://docs.microsoft.com/azure/migrate/how-to-add-tool-first-time#create-a-project-and-add-a-tool).

### <a name="an-azure-key-vault-management-operation-failed-error-id-60030-60031"></a>Ein Azure Key Vault-Verwaltungsvorgang war nicht erfolgreich. (Fehler-ID: 60030, 60031).

Stellen Sie sicher, dass das für die Registrierung der Appliance verwendete Azure-Benutzerkonto mindestens über „Mitwirkender“-Berechtigungen für das Abonnement verfügt. Stellen Sie außerdem sicher, dass das Konto Zugriff auf die in der Fehlermeldung angegebene Key Vault-Instanz hat, und wiederholen Sie dann den Vorgang. Wenn das Problem weiterhin besteht, wenden Sie sich an den Microsoft-Support. Weitere Informationen zu den erforderlichen Azure-Rollen und -Berechtigungen finden Sie [hier](https://docs.microsoft.com/azure/migrate/migrate-appliance#appliance-deployment-requirements).

### <a name="discovery-couldnt-be-initiated-because-of-an-error-the-operation-failed-for-the-specified-list-of-hosts-or-clusters-error-id-60028"></a>Die Ermittlung konnte aufgrund eines Fehlers nicht initiiert werden. Der Vorgang war für die angegebene Liste von Hosts oder Clustern nicht erfolgreich. (Fehler-ID: 60028).

Die Ermittlung konnte aufgrund eines Problems beim Zugreifen auf oder Abrufen von VM-Informationen auf den im Fehler aufgeführten Hosts nicht gestartet werden. Die restlichen Hosts wurden erfolgreich hinzugefügt. Fügen Sie die im Fehler angegebenen Hosts mithilfe der Option **Host hinzufügen** erneut hinzu. Gehen Sie im Falle eines Überprüfungsfehlers gemäß der Korrekturanleitung vor, um den Fehler zu beheben, und verwenden Sie dann erneut die Option **Speichern und Ermittlung starten**.

### <a name="an-azure-ad-operation-failed-the-error-occurred-while-creating-or-updating-the-azure-ad-application-error-id-60025"></a>Fehler bei einem Azure AD-Vorgang. Der Fehler trat beim Erstellen oder Aktualisieren der Azure AD-Anwendung auf. (Fehler-ID: 60025).

Dieser Fehler tritt auf, wenn zum Initiieren der Ermittlung ein anderes Azure-Benutzerkonto verwendet wird als für die Registrierung der Appliance. Führen Sie einen der folgenden Schritte aus:
1. Stellen Sie sicher, dass zum Initiieren der Ermittlung dasselbe Benutzerkonto verwendet wird wie zum Registrieren der Appliance.
1. Weisen Sie dem Benutzerkonto, bei dem der Ermittlungsfehler auftritt, Zugriffsberechtigungen für die AAD-Anwendung zu.
1. Löschen Sie die Ressourcengruppe, die zuvor für das Azure Migrate-Projekt erstellt wurde, und erstellen Sie eine andere Ressourcengruppe, um den Vorgang erneut zu starten.

Weitere Informationen zu AAD-Anwendungsberechtigungen finden Sie [hier](https://docs.microsoft.com/azure/migrate/migrate-appliance#appliance-deployment-requirements).

## <a name="discovery-issues"></a>Probleme bei der Ermittlung

### <a name="i-started-discovery-but-dont-see-the-discovered-vms-on-the-azure-portal-the-server-assessment-and-server-migration-tiles-show-a-status-of-discovery-in-progress"></a>Ich habe mit der Ermittlung begonnen, aber ich kann die ermittelten VMs nicht im Azure-Portal finden. Auf den Kacheln für **Serverbewertung** und **Servermigration** wird der Status „Erkennung wird ausgeführt“ angezeigt.
Nachdem Sie mit der Ermittlung durch das Gerät begonnen haben, warten Sie einen Moment, bis die gefundenen Geräte auf dem Azure-Portal angezeigt werden. Eine VMware-Ermittlung dauert etwa 15 Minuten und eine Hyper-V-Ermittlung benötigt 2 Minuten pro hinzugefügtem Host. Wenn „Erkennung wird ausgeführt“ selbst nach Ablauf dieser Wartezeiten immer noch angezeigt wird, wählen Sie **Aktualisieren** auf der Registerkarte **Server** aus. Dies sollte die Anzahl der ermittelten Server in den Kacheln „Serverbewertung“ und „Servermigration“ anzeigen.


### <a name="im-using-an-appliance-that-continuously-discovers-my-on-premises-environment-but-the-vms-that-are-deleted-in-my-on-premises-environment-are-still-shown-in-the-portal"></a>Ich verwende die Appliance, die kontinuierlich meine lokale Umgebung ermittelt, aber die VMs, die in meiner lokalen Umgebung gelöscht wurden, werden immer noch im Portal angezeigt.

Es kann bis zu 30 Minuten dauern, bis die von der Appliance gesammelten Ermittlungsdaten im Portal erscheinen. Wenn Sie auch nach 30 Minuten immer noch keine aktuellen Informationen sehen, aktualisieren Sie die Daten mit den folgenden Schritten:

1. Wechseln Sie zu **Server** > **Azure Migrate-Serverbewertung**, und wählen Sie **Übersicht** aus.
2. Wählen Sie unter **Verwalten** die Option **Agent-Integrität** aus.
3. Wählen Sie **Agent aktualisieren** aus. Sie werden diese Option unter der Liste der Agenten sehen.
4. Warten Sie, bis der Aktualisierungsvorgang abgeschlossen wurde. Sie sollten nun aktuelle Informationen zu Ihren VMs sehen.

### <a name="i-dont-see-the-latest-information-on-the-on-premise-vms-on-the-portal"></a>Ich sehe keine aktuellen Informationen über die lokalen VMs im Portal.

Es kann bis zu 30 Minuten dauern, bis die von der Appliance gesammelten Ermittlungsdaten im Portal erscheinen. Wenn Sie auch nach 30 Minuten immer noch keine aktuellen Informationen sehen, aktualisieren Sie die Daten mit den folgenden Schritten:

1. Wechseln Sie zu **Server** > **Azure Migrate-Serverbewertung**, und wählen Sie **Übersicht** aus.
2. Wählen Sie unter **Verwalten** die Option **Agent-Integrität** aus.
3. Wählen Sie **Agent aktualisieren** aus. Sie werden diese Option unter der Liste der Agenten sehen.
4. Warten Sie, bis der Aktualisierungsvorgang abgeschlossen wurde. Sie sollten nun aktuelle Informationen zu Ihren VMs sehen.

### <a name="cant-connect-to-a-host-or-cluster-because-the-server-name-cant-be-resolved-winrm-error-code-0x803381b9-error-id-50004"></a>Es ist keine Verbindung mit einem Host oder Cluster möglich, da der Servername nicht aufgelöst werden kann. WinRM-Fehlercode: 0x803381b9 (Fehler-ID: 50004).
Dieser Fehler tritt auf, wenn der Azure DNS-Dienst für die Appliance den von Ihnen angegebenen Cluster- oder Hostnamen nicht auflösen kann. Wenn Sie diesen Fehler auf dem Cluster sehen, versuchen Sie, den vollqualifizierten Domänennamen (FQDN) des Clusters anzugeben.

Sie sehen diesen Fehler eventuell auch für Hosts in einem Cluster. In diesem Fall kann die Appliance eine Verbindung mit dem Cluster herstellen, aber der Cluster gibt Hostnamen zurück, bei denen es sich nicht um FQDNs handelt.

Um diesen Fehler zu beheben, aktualisieren Sie die „hosts“-Datei auf der Appliance und fügen Sie ein Mapping von IP-Adresse und Hostnamen hinzu:
1. Öffnen Sie Notepad als Administrator. Öffnen Sie dann die Datei „c:\Windows\System32\Drivers\etc\hosts“.
2. Fügen Sie die IP-Adresse und den Hostnamen in einer Zeile hinzu. Wiederholen Sie dies für jeden Host oder Cluster, bei dem Sie diesen Fehler sehen.
3. Speichern und schließen Sie die Datei „hosts“.
4. Sie können mit der Applianceverwaltungs-App überprüfen, ob die Appliance eine Verbindung mit den Hosts herstellen kann. Nach 30 Minuten sollten Sie die neuesten Informationen für diese Hosts im Azure-Portal sehen können.


## <a name="assessment-issues"></a>Bewertungsprobleme

### <a name="azure-readiness-issues"></a>Azure-Bereitschaftsprobleme

Problem | Wiederherstellung
--- | ---
Nicht unterstützter Starttyp | Azure unterstützt keine virtuellen Computer mit dem Starttyp „EFI“. Wir empfehlen, dass Sie vor einer Migration den Starttyp in „BIOS“ ändern. <br/><br/>Sie können die Azure Migrate-Servermigration verwenden, um die Migration solcher virtuellen Computer zu verarbeiten. Sie ändert den Starttyp des virtuellen Computers im Rahmen der Migration in „BIOS“.
Bedingt unterstütztes Windows-Betriebssystem | Der Unterstützungszeitraum für das Betriebssystem ist abgelaufen. Für die [Unterstützung in Azure](https://aka.ms/WSosstatement) wird eine benutzerdefinierte Supportvereinbarung (Custom Support Agreement, CSA) benötigt. Ziehen Sie vor der Migration zu Azure ein Upgrade des Betriebssystems in Erwägung.
Nicht unterstütztes Windows-Betriebssystem | Azure unterstützt nur [ausgewählte Windows-Betriebssystemversionen](https://aka.ms/WSosstatement). Ziehen Sie vor der Migration zu Azure ein Upgrade des Betriebssystems des Computers in Erwägung.
Bedingt unterstütztes Linux-Betriebssystem | Azure unterstützt nur [ausgewählte Linux-Betriebssystemversionen](../virtual-machines/linux/endorsed-distros.md). Ziehen Sie vor der Migration zu Azure ein Upgrade des Betriebssystems des Computers in Erwägung.
Nicht unterstütztes Linux-Betriebssystem | Der Computer startet möglicherweise in Azure, aber Azure stellt keine Betriebssystemunterstützung bereit. Ziehen Sie vor der Migration zu Azure ein Upgrade des Betriebssystems auf eine [unterstützte Linux-Version](../virtual-machines/linux/endorsed-distros.md) in Erwägung.
Unbekanntes Betriebssystem | Das Betriebssystem der VM wurde in vCenter Server als „Sonstige“ angegeben. Dieses Verhalten hindert Azure Migrate daran, die Azure-Bereitschaft der VM zu überprüfen. Stellen Sie sicher, dass das Betriebssystem des Computers von Azure [unterstützt](https://aka.ms/azureoslist) wird, bevor Sie den Computer migrieren.
Nicht unterstützte Bitanzahl für das Betriebssystem | Virtuelle Computer mit einem 32-Bit-Betriebssystem starten zwar unter Umständen in Azure, es empfiehlt sich jedoch, vor der Migration zu Azure ein Betriebssystemupgrade der VM auf 64-Bit durchzuführen.
Erfordert ein Microsoft Visual Studio-Abonnement. | Auf dem Computer wird ein Windows-Clientbetriebssystem ausgeführt, das nur mit einem Visual Studio-Abonnement unterstützt wird.
Kein virtueller Computer für erforderliche Speicherleistung gefunden. | Die erforderliche Speicherleistung (Eingabe/Ausgabe-Vorgänge pro Sekunde (IOPS) und Durchsatz) für den Computer überschreitet die von virtuellen Azure-Computern unterstützte Leistung. Reduzieren Sie vor der Migration die Speicheranforderungen für den Computer.
Kein virtueller Computer für erforderliche Netzwerkleistung gefunden. | Die erforderliche Netzwerkleistung (ein-/ausgehend) für den Computer überschreitet die von virtuellen Azure-Computern unterstützte Leistung. Reduzieren Sie die Netzwerkanforderungen für den Computer.
Kein virtueller Computer am angegebenen Speicherort gefunden. | Geben Sie vor der Migration einen anderen Zielort an.
Mindestens ein Datenträger ist ungeeignet. | Mindestens ein mit der VM verbundener Datenträger erfüllt nicht die Azure-Anforderungen. Azure Migrate: Die Serverbewertung unterstützt derzeit keine SSD Ultra-Datenträger und bewertet die Datenträger basierend auf den Datenträgerlimits für verwaltete Premium-Datenträger (32 TB).  Stellen Sie für jeden an den virtuellen Computer angefügten Datenträger sicher, dass die Größe des Datenträgers kleiner als 64 TB ist (von SSD Ultra-Datenträgern unterstützt). Wenn dies nicht der Fall ist, verringern Sie die Größe des Datenträgers, bevor Sie zu Azure migrieren oder mehrere Datenträger in Azure verwenden, und [kombinieren Sie sie in Stripesets](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance#disk-striping), um höhere Speicherlimits zu erzielen. Stellen Sie sicher, dass die von jedem Datenträger benötigte Leistung (IOPS und Durchsatz) von [verwalteten Azure-Datenträgern virtueller Computer](https://docs.microsoft.com/azure/azure-subscription-service-limits#storage-limits) unterstützt wird.
Mindestens ein Netzwerkadapter ist ungeeignet. | Entfernen Sie nicht verwendete Netzwerkadapter vor der Migration vom Computer.
Anzahl der Datenträger überschreitet den Grenzwert | Entfernen Sie nicht verwendete Datenträger vor der Migration vom Computer.
Datenträgergröße überschreitet den Grenzwert | Azure Migrate: Die Serverbewertung unterstützt derzeit keine SSD Ultra-Datenträger und bewertet die Datenträger basierend auf den Limits für Premium-Datenträger (32 TB). Azure unterstützt jedoch Datenträger mit einer Größe von bis zu 64 TB (von SSD Ultra-Datenträgern unterstützt). Verkleinern Sie Datenträger vor der Migration auf weniger als 64 TB, oder verwenden Sie mehrere Datenträger in Azure, und [kombinieren Sie sie in Stripesets](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance#disk-striping), um höhere Speicherlimits zu erzielen.
Datenträger am angegebenen Speicherort nicht verfügbar | Stellen Sie vor der Migration sicher, dass sich der Datenträger am Zielspeicherort befindet.
Datenträger für die angegebene Redundanz nicht verfügbar | Der Datenträger muss den in den Bewertungseinstellungen definierten Redundanzspeichertyp verwenden (standardmäßig LRS).
Datenträgereignung konnte aufgrund eines internen Fehlers nicht ermittelt werden. | Versuchen Sie, eine neue Bewertung für die Gruppe zu erstellen.
Kein virtueller Computer gefunden, der die Kern- und Arbeitsspeicheranforderungen erfüllt | Azure konnte keinen geeigneten VM-Typ finden. Reduzieren Sie vor der Migration den Arbeitsspeicher und die Anzahl der Kerne auf dem lokalen Computer.
Eignung des virtuellen Computers konnte aufgrund eines internen Fehlers nicht ermittelt werden. | Versuchen Sie, eine neue Bewertung für die Gruppe zu erstellen.
Eignung konnte für mindestens einen Datenträger aufgrund eines internen Fehlers nicht ermittelt werden. | Versuchen Sie, eine neue Bewertung für die Gruppe zu erstellen.
Eignung konnte für mindestens einen Netzwerkadapter aufgrund eines internen Fehlers nicht ermittelt werden. | Versuchen Sie, eine neue Bewertung für die Gruppe zu erstellen.

### <a name="i-cant-specify-enterprise-agreement-ea-as-an-azure-offer-in-the-assessment-properties"></a>Ich kann kein Enterprise Agreement (EA) als Azure-Angebot in den Bewertungseigenschaften angeben.
Die Azure Migrate-Serverbewertung unterstützt derzeit keine Preise, die auf Enterprise Agreement (EA) basieren. Die Umgehung dieser Einschränkung besteht darin, **Nutzungsbasierte Zahlung** als Azure-Angebot und die Eigenschaft **Rabatt** zu verwenden, um benutzerdefinierte Rabatte anzugeben, die Sie erhalten. [Erfahren Sie mehr darüber, wie Sie eine Bewertung anpassen können](https://aka.ms/migrate/selfhelp/eapricing).

### <a name="why-does-server-assessment-mark-my-linux-vms-conditionally-ready"></a>Warum markiert die Serverbewertung meine virtuellen Linux-Computer als „bedingt bereit“?
Es gibt eine bekannte Lücke in der Serverbewertung, die sie daran hindert, die auf den lokalen virtuellen Computern installierte Nebenversion des Linux-Betriebssystems zu erkennen (für RHEL 6.10 erkennt die Serverbewertung derzeit nur RHEL 6 als Betriebssystemversion). Da Azure nur bestimmte Versionen von Linux unterstützt, werden die virtuellen Linux-Computer in der Serverbewertung derzeit als „bedingt bereit“ markiert. Sie können ermitteln, ob das Linux-Betriebssystem des lokalen virtuellen Computers für Azure geeignet ist. Sehen Sie sich dazu die [Dokumentation zur Linux-Unterstützung von Azure](https://aka.ms/migrate/selfhost/azureendorseddistros) an. Nachdem Sie die unterstützte Distribution überprüft haben, können Sie diese Warnung ignorieren.

### <a name="why-does-the-vm-sku-recommended-by-server-assessment-have-more-cores-and-more-memory-than-whats-allocated-on-premises"></a>Warum hat die von der Serverbewertung empfohlene VM-SKU mehr Kerne und einen größeren Arbeitsspeicher, als in der lokalen Bereitstellung zugewiesen sind?
Die in der Serverbewertung empfohlene VM-SKU hängt von den Bewertungseigenschaften ab. Sie können zwei Bewertungsarten in der Serverbewertung erstellen:  *Leistungsbasiert* und *Wie lokal*. Bei leistungsbasierten Bewertungen berücksichtigt die Serverbewertung die Nutzungsdaten der lokalen virtuellen Computer (CPU-, Speicher-, Datenträger- und Netzwerkauslastung), um die richtige Ziel-VM-SKU für Ihre lokalen virtuellen Computer zu bestimmen. Zusätzlich wird bei der leistungsbasierten Dimensionierung der Komfortfaktor berücksichtigt, um die effektive Auslastung zu ermitteln. Bei der lokalen Dimensionierung werden Leistungsdaten nicht berücksichtigt, und eine Ziel-SKU wird auf der Grundlage der lokalen Zuordnung empfohlen.

Angenommen, es gibt einen lokalen virtuellen Computer mit 4 Kernen und 8 GB Speicher mit 50 % CPU-Auslastung und 50% Speicherauslastung, und in der Bewertung ist ein Komfortfaktor von 1,3 angegeben. Wenn das Dimensionierungskriterium der Bewertung **Wie lokal** ist, wird eine Azure-VM-SKU mit 4 Kernen und 8 Gigabyte (GB) Arbeitsspeicher empfohlen.

Nehmen Sie jedoch an, dass das Dimensionierungskriterium leistungsbasiert ist. Basierend auf der effektiven CPU- und Speicherauslastung (50 % von 4 Kernen * 1,3 = 2,6 Kerne und 50 % von 8 GB Arbeitsspeicher * 1,3 = 5,3 GB Arbeitsspeicher) würde die günstigste VM-SKU mit vier Kernen (nächste unterstützte Kernanzahl) und 8 GB Arbeitsspeichergröße (nächste unterstützte Arbeitsspeichergröße) empfohlen. [Erfahren Sie über die Größenanpassung durch die Serverbewertung.](https://docs.microsoft.com/azure/migrate/concepts-assessment-calculation#sizing)

### <a name="why-is-the-disk-sku-recommended-by-server-assessment-bigger-than-whats-allocated-on-premises"></a>Warum ist die von der Serverbewertung empfohlene Datenträger-SKU größer als die lokale Zuordnung?
Die Datenträgerdimensionierung in der Serverbewertung hängt von zwei Bewertungseigenschaften ab: dem Dimensionierungskriterium und dem Speichertyp. Wenn das Dimensionierungskriterium **Leistungsbasiert** ist und der Speichertyp **Automatisch** lautet, werden die IOPS- und Durchsatzwerte des Datenträgers berücksichtigt, um den Typ des Zieldatenträgers zu ermitteln (HDD Standard, SSD Standard oder Premium). Eine Datenträger-SKU dieses Datenträgertyps wird dann unter Berücksichtigung der Größenanforderungen des lokalen Datenträgers empfohlen. Wenn das Dimensionierungskriterium **Leistungsbasiert** ist und der Speichertyp **Premium** lautet, wird eine Premium-Datenträger-SKU in Azure basierend auf den IOPS, dem Durchsatz und den Größenanforderungen des lokalen Datenträgers empfohlen. Die gleiche Logik wird verwendet, um die Datenträgerdimensionierung durchzuführen, wenn das Dimensionierungskriterium **Wie lokal** verwendet wird und der Speichertyp **HDD Standard**, **SSD Standard** oder **Premium** lautet.

Wenn Sie beispielsweise einen lokalen Datenträger mit 32 GB Speicher haben, die aggregierten Lese- und Schreib-IOPS für den Datenträger aber 800 IOPS sind, empfiehlt die Serverbewertung einen Premium-Datenträgertyp (aufgrund der höheren IOPS-Anforderungen) und empfiehlt dann eine Datenträger-SKU, welche die erforderlichen IOPS und Größen unterstützen kann. Die nächstliegende Übereinstimmung in diesem Beispiel wäre P15 (256 GB, 1100 IOPS). Für den lokalen Datenträger ist also nur eine Größe von 32 GB erforderlich, von der Serverbewertung wurde jedoch aufgrund der hohen IOPS-Anforderungen des lokalen Datenträgers ein größerer Datenträger empfohlen.

### <a name="why-does-my-assessment-report-indicate-percentageofcoresutilizedmissing-or-percentageofmemoryutilizedmissing-for-some-vms"></a>Warum zeigt mein Bewertungsbericht „PercentageOfCoresUtilizedMissing“ oder „PercentageOfMemoryUtilizedMissing“ für einige virtuelle Computer an?
Diese Probleme werden gemeldet, wenn die Azure Migrate-Appliance keine Leistungsdaten für die lokalen virtuellen Computer erfassen kann. Dieses Verhalten kann eintreten, wenn die virtuellen Computer für die Dauer der Erstellung der Bewertung (letzten Tag/1 Woche/1 Monat) ausgeschaltet sind. Die Appliance kann keine Leistungsdaten für eine VM erfassen, wenn sie ausgeschaltet ist. Falls nur Arbeitsspeicher-Leistungsindikatoren fehlen und Sie versuchen, virtuelle Hyper-V-Computer zu bewerten, überprüfen Sie, ob auf diesen virtuellen Computern dynamischer Arbeitsspeicher aktiviert ist. Die Azure Migrate-Appliance kann aufgrund eines bekannten Problems keine Arbeitsspeichernutzungsdaten für virtuelle Computer erfassen, für die kein dynamischer Arbeitsspeicher aktiviert ist. Dieses Problem betrifft nur Hyper-V-VMs, keine VMware-VMs. Wenn einer der Leistungsindikatoren fehlt, greift die Azure Migrate-Serverbewertung auf die zugewiesenen Kerne und den Arbeitsspeicher zurück und empfiehlt eine entsprechende VM-Größe.

### <a name="is-the-os-license-cost-of-the-vm-included-in-the-compute-cost-estimated-by-server-assessment"></a>Sind die Kosten der Betriebssystemlizenz des virtuellen Computers in den von der Serverbewertung geschätzten Computekosten enthalten?
Die Serverbewertung berücksichtigt zurzeit die Kosten einer Betriebssystemlizenz nur für Windows-Computer. Die Kosten einer Betriebssystemlizenz für Linux-Computer werden derzeit nicht berücksichtigt.

### <a name="what-impact-do-performance-history-and-percentile-utilization-have-on-the-size-recommendations"></a>Welche Auswirkungen haben der Leistungsverlauf und der Quantilwert der Nutzung auf die Größenempfehlungen?
Diese Eigenschaften gelten nur für die leistungsbasierte Dimensionierung. Die Serverbewertung erfasst Leistungsdaten von lokalen Computern und nutzt diese Daten, um die VM-SKU und die Datenträger-SKU in Azure zu empfehlen. Diese Leistungsdaten werden wie folgt von der Serverbewertung erfasst:
- Die Azure Migrate-Appliance erstellt kontinuierlich Profile der lokalen Umgebung, um Echtzeit-Nutzungsdaten alle 20 Sekunden für virtuelle VMware-Computer und alle 30 Sekunden für virtuelle Hyper-V-Computer zu erfassen.
- Die Appliance führt für die 20-Sekunden- und 30-Sekunden-Stichproben einen Rollup aus, um für jeweils 10 Minuten einen Datenpunkt zu erstellen. Zum Erstellen jedes Datenpunkts wählt die Appliance den Spitzenwert aus allen 20-Sekunden- und 30-Sekunden-Stichproben aus und sendet diesen dann an Azure.
- Beim Erstellen einer Bewertung in der Serverbewertung, die auf der Leistungsdauer und dem Quantilwert des Leistungsverlaufs basiert, wird der repräsentative Nutzungswert ermittelt. Wenn der Leistungsverlauf beispielsweise eine Woche beträgt und die Quantilauslastung 95. ist, sortiert Azure Migrate alle 10-minütigen Stichprobenpunkte für die letzte Woche in aufsteigender Reihenfolge und wählt dann das 95. Perzentil als repräsentativen Wert aus.
Mit dem Wert des 95. Quantils wird sichergestellt, dass Ausreißer ignoriert werden. Diese können enthalten sein, wenn Sie das 99. Quantil wählen. Falls Sie die Spitzenauslastung für den Zeitraum wählen möchten und keine Ausreißer verpassen möchten, sollten Sie das 99. Quantil als Quantilauslastung wählen.

## <a name="dependency-visualization-issues"></a>Probleme bei der Visualisierung von Abhängigkeiten

### <a name="i-cant-find-the-dependency-visualization-functionality-for-azure-government-projects"></a>Ich kann die Funktion zur Visualisierung von Abhängigkeiten für Azure Government-Projekte nicht finden.

Azure Migrate ist bei der Funktion zur Visualisierung von Abhängigkeiten von Service Map abhängig. Da Service Map in Azure Government zurzeit nicht verfügbar ist, ist diese Funktionalität auch nicht in Azure Government verfügbar.

### <a name="i-installed-the-microsoft-monitoring-agent-mma-and-the-dependency-agent-on-my-on-premises-vms-but-the-dependencies-are-now-showing-up-in-the-azure-migrate-portal"></a>Ich habe Microsoft Monitoring Agent (MMA) und den Dependency-Agent auf meinen lokalen virtuellen Computern installiert. Jetzt werden die Abhängigkeiten jedoch im Azure Migrate-Portal angezeigt.

Nachdem Sie die Agents installiert haben, benötigt Azure Migrate üblicherweise 15 bis 30 Minuten, um die Abhängigkeiten im Portal anzuzeigen. Wenn Sie bereits länger als 30 Minuten gewartet haben, stellen Sie sicher, dass MMA mit dem OMS-Arbeitsbereich kommunizieren kann. Gehen Sie hierzu wie folgt vor.

Bei einer virtuellen Windows-VM:
1. Wechseln Sie zur Systemsteuerung, und starten Sie den Microsoft Monitoring Agent.
2. Vergewissern Sie sich auf der Registerkarte **Azure Log Analytics (OMS)** im Dialogfeld **Microsoft Monitoring Agent-Eigenschaften**, dass der **Status** für den Arbeitsbereich grün angezeigt wird.
3. Wenn der Status nicht grün angezeigt wird, entfernen Sie den Arbeitsbereich, und fügen Sie ihn zu MMA neu hinzu.

      ![Dialogfeld „MMA-Eigenschaften“](./media/troubleshooting-general/mma-status.png)

Stellen Sie bei einer Linux-VM sicher, dass die Installationsbefehle für MMA und den Dependency-Agent erfolgreich ausgeführt wurden.

### <a name="what-operating-systems-does-mma-support"></a>Welche Betriebssysteme unterstützt MMA?

 [Hier finden Sie eine Liste der von MMA unterstützten Windows-Betriebssysteme](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems).
Ferner [finden Sie hier eine Liste der von MMA unterstützten Linux-Betriebssysteme](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems).

### <a name="what-operating-systems-does-the-dependency-agent-support"></a>Welche Betriebssysteme werden vom Dependency-Agent unterstützt?

[Hier finden Sie eine Liste der vom Dependency-Agent unterstützten Windows-Betriebssysteme](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-windows-operating-systems). Und [hier finden Sie eine Liste der vom Dependency-Agent unterstützten Linux-Betriebssysteme](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-linux-operating-systems).

### <a name="i-cant-visualize-dependencies-in-azure-migrate-for-more-than-a-one-hour-duration"></a>Ich kann Abhängigkeiten in Azure Migrate nicht für länger als eine Stunde visualisieren.
In Azure Migrate können Sie Abhängigkeiten für eine Dauer von bis zu einer Stunde visualisieren. Mit Azure Migrate können Sie zwar um bis zu einen Monat zu einem bestimmten Datum zurückgehen. Die Abhängigkeiten können Sie jedoch nur für einen Zeitraum von maximal einer Stunde visualisieren.

So können Sie beispielsweise mithilfe der Zeitraumfunktionalität im Abhängigkeitsdiagramm Abhängigkeiten für gestern, jedoch nur für einen Zeitraum von einer Stunde anzeigen. Sie können jedoch zum [Abfragen der Abhängigkeitsdaten](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies) über einen längeren Zeitraum Azure Monitor-Protokolle verwenden.

### <a name="i-cant-visualize-dependencies-for-groups-that-have-more-than-10-vms"></a>Ich kann Abhängigkeiten für Gruppen nicht visualisieren, die mehr als zehn VMs umfassen.
Sie können [Abhängigkeiten für Gruppen visualisieren](https://docs.microsoft.com/azure/migrate/how-to-create-group-dependencies), die aus bis zu zehn VMs bestehen. Falls eine Gruppe aus mehr als zehn VMs besteht, sollten Sie die Gruppe in kleinere Gruppen aufteilen und die Abhängigkeiten anschließend visualisieren.

### <a name="i-installed-agents-and-used-the-dependency-visualization-to-create-groups-now-post-failover-the-machines-show-install-agent-action-instead-of-view-dependencies"></a>Ich habe Agents installiert und die Visualisierung von Abhängigkeiten verwendet, um Gruppen zu erstellen. Jetzt zeigen die Computer nach dem Failover die Aktion „Agent installieren“ anstelle von „Abhängigkeiten anzeigen“ an.
* Nach einem geplanten oder ungeplanten Failover werden lokale Computer abgeschaltet, und entsprechende Computer in Azure werden hochgefahren. Diese Computer beziehen eine andere MAC-Adresse. Je nachdem, ob der Benutzer die lokale IP-Adresse beibehalten möchte oder nicht, beziehen sie möglicherweise auch eine andere IP-Adresse.

  Wenn sich sowohl MAC- als auch IP-Adressen unterscheiden, ordnet Azure Migrate die lokalen Computern keinen Service Map-Abhängigkeitsdaten zu. Stattdessen fordert es den Benutzer auf, Agents zu installieren, anstatt Abhängigkeiten anzuzeigen.
* Nach dem Testfailover bleiben die lokalen Computer erwartungsgemäß eingeschaltet. Entsprechende Computer, die in Azure hochgefahren wurden, erhalten andere MAC-Adressen und u. U. auch andere IP-Adressen. Sofern der Benutzer ausgehenden Datenverkehr von Azure Monitor-Protokolle von diesen Computern nicht sperrt, ordnet Azure Migrate die lokalen Computer keinen Service Map-Abhängigkeitsdaten zu und fordert den Benutzer auf, Agents zu installieren, anstatt Abhängigkeiten anzuzeigen.

## <a name="collect-azure-portal-logs"></a>Sammeln von Azure-Portalprotokollen

**Wie erfasse ich im Azure-Portal Protokolle des Netzwerkdatenverkehrs?**

1. Öffnen Sie den Browser, wechseln Sie [zum Portal](https://portal.azure.com), und melden Sie sich an.
2. Drücken Sie F12, um die Entwicklertools zu starten. Deaktivieren Sie ggf. die Einstellung **Einträge beim Navigieren löschen**.
3. Wählen Sie die Registerkarte **Netzwerk** aus, und starten Sie die Erfassung von Netzwerkdatenverkehr:
   - Wählen Sie in Chrome die Option **Protokoll speichern**. Die Aufzeichnung sollte automatisch gestartet werden. Ein roter Kreis gibt an, dass der Datenverkehr erfasst wird. Wenn der rote Kreis nicht angezeigt wird, wählen Sie zum Starten den schwarzen Kreis aus.
   - In Microsoft Edge und Internet Explorer sollte die Aufzeichnung automatisch gestartet werden. Ist dies nicht der Fall, wählen Sie die grüne Wiedergabeschaltfläche aus.
4. Versuchen Sie, den Fehler zu reproduzieren.
5. Nachdem der Fehler während der Aufzeichnung aufgetreten ist, beenden Sie die Aufzeichnung, und speichern Sie eine Kopie der aufgezeichneten Aktivität:
   - Klicken Sie in Chrome mit der rechten Maustaste, und wählen Sie **Als HAR mit Inhalt speichern** aus. Durch diese Aktion werden die Protokolle komprimiert und als HAR-Datei exportiert.
   - Wählen Sie in Microsoft Edge oder Internet Explorer das Symbol **Aufgezeichneten Datenverkehr exportieren** aus. Durch diese Aktion werden die Protokolle komprimiert und exportiert.
6. Wählen Sie die Registerkarte **Konsole** aus, um sie auf Warnungen oder Fehler zu überprüfen. So speichern Sie das Konsolenprotokoll:
   - Klicken Sie in Chrome mit der rechten Maustaste auf eine beliebige Stelle im Konsolenprotokoll. Wählen Sie **Speichern als**, um das Protokoll zu exportieren und zu komprimieren.
   - Klicken Sie in Microsoft Edge oder Internet Explorer mit der rechten Maustaste auf die Fehler, und wählen Sie **Alle kopieren** aus.
7. Schließen Sie die Entwicklertools.
