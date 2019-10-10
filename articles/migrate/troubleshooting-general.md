---
title: Behandeln von Problemen bei Azure Migrate | Microsoft-Dokumentation
description: Bietet eine Übersicht über bekannte Probleme im Azure Migrate-Dienst sowie Problembehandlungstipps für häufige Fehler.
author: musa-57
ms.manager: abhemraj
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 09/17/2019
ms.author: hamusa
ms.openlocfilehash: 468c87e176cc61c48ba4caabd1c5a26f94d5fb5b
ms.sourcegitcommit: c2e7595a2966e84dc10afb9a22b74400c4b500ed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2019
ms.locfileid: "71970649"
---
# <a name="troubleshoot-azure-migrate"></a>Problembehandlung für Azure Migrate

[Azure Migrate](migrate-services-overview.md) stellt einen Hub mit Tools für die Bewertung und Migration sowie unabhängige Drittanbietertools bereit. Dieser Artikel hilft Ihnen bei der Behandlung von Problemen mit Azure Migrate sowie bei der Azure Migrate-Serverbewertung und Azure Migrate-Servermigration.


## <a name="find-a-project"></a>Suchen eines Projekts

Es sind [zwei Versionen](https://docs.microsoft.com/azure/migrate/migrate-services-overview#azure-migrate-versions) von Azure Migrate verfügbar.


Wenn Sie das Azure Migrate-Projekt in der aktuellen Version von Azure Migrate erstellt haben, gehen Sie wie folgt vor:

1. Suchen Sie im [Azure-Portal](https://portal.azure.com) nach **Azure Migrate**.
2. Rufen Sie im Dashboard von Azure Migrate **Server** auf, und wählen Sie oben rechts in der Ecke **Ändern** aus.

    ![Wechseln zu einem vorhandenen Azure Migrate-Projekt](./media/troubleshooting-general/switch-project.png)

3. Wählen Sie das entsprechende Abonnement und das Azure Migrate-Projekt aus.


Wenn Sie das Projekt in der vorherigen Version von Azure Migrate erstellt haben, gehen Sie wie folgt vor:

1. Suchen Sie im [Azure-Portal](https://portal.azure.com) nach **Azure Migrate**.
2. Wenn Sie ein Projekt in der vorherigen Version erstellt haben, wird im Dashboard von Azure Migrate ein Banner mit einem Verweis auf ältere Projekte angezeigt. Wählen Sie das Banner aus.

    ![Zugreifen auf vorhandene Projekte](./media/troubleshooting-general/access-existing-projects.png)

3. Überprüfen Sie die Liste der alten Projekte.


## <a name="create-additional-projects"></a>Erstellen zusätzlicher Projekte

Erstellen Sie wie folgt ein neues Azure Migrate-Projekt:

1. Suchen Sie im [Azure-Portal](https://portal.azure.com) nach **Azure Migrate**.
2. Rufen Sie im Dashboard von Azure Migrate **Server** auf, und wählen Sie oben rechts in der Ecke **Ändern** aus.

   ![Ändern des Azure Migrate-Projekts](./media/troubleshooting-general/switch-project.png)

3. Wählen Sie zum Erstellen eines neuen Projekts **Hier klicken** aus.

   ![Erstellen eines zweiten Azure Migrate-Projekts](./media/troubleshooting-general/create-new-project.png)

## <a name="review-supported-geographies"></a>Überprüfen unterstützter geografischer Regionen

Überprüfen Sie die unterstützten geografischen Regionen für [VMware](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#azure-migrate-projects) und [Hyper-V](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-hyper-v#azure-migrate-projects).

## <a name="delete-projectsworkspaces"></a>Löschen von Projekten/Arbeitsbereichen

Beim Löschen von Azure Migrate-Projekten und Log Analytics-Arbeitsbereichen ist Folgendes zu beachten:

- Wenn Sie ein Azure Migrate-Projekt löschen, werden das Projekt *und* die Metadaten zu den ermittelten Computern gelöscht.
- Wenn Sie dem Serverbewertungstool einen Log Analytics-Arbeitsbereich angefügt hatten, wird dieser nicht automatisch gelöscht.
- Derselbe Log Analytics-Arbeitsbereich kann für mehrere Szenarien verwendet werden.
- Wenn Sie den Log Analytics-Arbeitsbereich löschen möchten, müssen Sie dies manuell tun.


### <a name="delete-a-project"></a>Löschen eines Projekts

So löschen Sie ein Projekt in der aktuellen Version von Azure Migrate

1. Öffnen Sie die Azure-Ressourcengruppe, in der das Projekt erstellt wurde.
2. Wählen Sie auf der Seite für die Ressourcengruppe **Ausgeblendete Typen anzeigen** aus.
3. Wählen Sie das zu löschende Migrationsprojekt aus. Der Ressourcentyp ist „Microsoft.Migrate/migrateprojects“, er löscht das Projekt.

So löschen Sie ein Projekt in der vorherigen Version von Azure Migrate

1. Öffnen Sie die Azure-Ressourcengruppe, in der das Projekt erstellt wurde.
2. Wählen Sie das zu löschende Migrationsprojekt aus. Der Ressourcentyp ist „Migration project“, er löscht das Projekt.


### <a name="delete-a-workspace"></a>Löschen eines Arbeitsbereichs

Navigieren Sie zu dem Log Analytics-Arbeitsbereich, der dem Projekt angefügt ist.
* Wenn Sie das Azure Migrate-Projekt nicht gelöscht haben, finden Sie den Link zum Arbeitsbereich unter **Essentials** > **Serverbewertung**.
       ![LA-Arbeitsbereich](./media/troubleshooting-general/loganalytics-workspace.png)

     * If you've already deleted the Azure Migrate project, select **Resource Groups** in the left pane of the Azure portal. Locate the workspace in the relevant resources group, and [follow the instructions](https://docs.microsoft.com/azure/azure-monitor/platform/delete-workspace) to delete it.


## <a name="error-requests-must-contain-user-identity-headers"></a>Fehler „Anforderungen müssen Benutzeridentitätsheader enthalten“

Beim Erstellen eines Projekts kann dieser Fehler darauf hinweisen, dass Sie keinen Zugriff auf den Azure AD-Mandanten (Azure Active Directory) des Unternehmens haben.

- Wenn Sie erstmalig zu einem Azure AD-Mandanten hinzugefügt werden, erhalten Sie eine E-Mail-Einladung, dem Mandanten beizutreten.
- Nehmen Sie die Einladung an, um erfolgreich dem Mandanten hinzugefügt zu werden.
    - Wenn die E-Mail nicht angezeigt wird, wenden Sie sich an einen Benutzer mit Zugriff auf den Mandanten, und bitten Sie ihn, [die Einladung erneut zu senden](https://docs.microsoft.com/azure/active-directory/b2b/add-users-administrator#resend-invitations-to-guest-users).
    - Nachdem Sie die Einladungs-E-Mail erhalten haben, öffnen Sie die E-Mail, und wählen Sie den Link aus, um die Einladung anzunehmen. Melden Sie sich dann beim Azure-Portal ab und wieder an. (Das Aktualisieren des Browsers funktioniert hierbei nicht.) Sie können dann mit dem Erstellen des Migrationsprojekts beginnen.


## <a name="error-invalid-ovf-manifest-entry"></a>Fehler „Ungültiger OVF-Manifesteintrag“

Wenn Sie den Fehler „Die angegebene Manifestdatei ist ungültig: Ungültiger OVF-Manifesteintrag“ erhalten, gehen Sie wie folgt vor:

1. Überprüfen Sie, ob die OVA-Datei für die Azure Migrate-Appliance ordnungsgemäß heruntergeladen wird, indem Sie deren Hashwert überprüfen. [Weitere Informationen](https://docs.microsoft.com/azure/migrate/tutorial-assessment-vmware) Wenn der Hashwert nicht übereinstimmt, laden Sie die OVA-Datei erneut herunter, und wiederholen Sie die Bereitstellung.
2. Wenn immer noch Fehler bei der Bereitstellung auftreten und Sie den VMware vSphere-Client zum Bereitstellen der OVF-Datei verwenden, versuchen Sie, die Bereitstellung über den vSphere-Webclient vorzunehmen. Wenn die Bereitstellung weiterhin fehlschlägt, versuchen Sie es mit einem anderen Webbrowser.
3. Wenn Sie den vSphere-Webclient verwenden und die Bereitstellung auf vCenter Server 6.5 oder 6.7 ausführen möchten, versuchen Sie, die OVA-Datei direkt auf dem ESXi-Host bereitzustellen:
   - Stellen Sie mit dem Webclient (https://<*Host-IP-Adresse*>/ui) eine direkte Verbindung mit dem ESXi-Host her (anstelle von vCenter Server).
   - Wählen Sie unter **Startseite** > **Bestand** die Optionen **Datei** > **OVF-Vorlage bereitstellen** aus. Navigieren Sie zur OVA-Datei, und schließen Sie die Bereitstellung ab.
4. Wenn weiterhin ein Fehler bei der Bereitstellung auftritt, wenden Sie sich an den Azure Migrate-Support.

## <a name="appliance-cant-connect-to-the-internet"></a>Die Appliance kann keine Verbindung mit dem Internet herstellen

Dies kann der Fall sein, wenn sich der Appliancecomputer hinter einem Proxy befindet.

- Stellen Sie sicher, dass Sie die Anmeldeinformationen für die Autorisierung angeben, wenn der Proxy diese benötigt.
- Wenn Sie einen URL-basierten Firewallproxy zum Steuern der ausgehenden Verbindungen verwenden, fügen Sie die folgenden URLs einer Zulassungsliste hinzu:

    - [URLs für VMware-Bewertung](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#assessment-url-access-requirements)
    - [URLs für Hyper-V-Bewertung](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-hyper-v#assessment-appliance-url-access)
    - [URLs für VMware-Migration ohne Agent](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#agentless-migration-url-access-requirements)
    - [URLs für VMware-Migration mit Agent](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#replication-appliance-url-access)
    - [URLs für Hyper-V-Migration](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-hyper-v#migration-hyper-v-host-url-access)

- Wenn Sie für die Internetverbindung einen abfangenden Proxy verwenden, importieren Sie das Proxyzertifikat [mit diesen Schritten](https://docs.microsoft.com/azure/migrate/concepts-collector) in die Appliance-VM.

## <a name="errordatetime-synchronization"></a>Fehler: Datums-/Zeitsynchronisierung

Ein Fehler bei der Datums- und Zeitsynchronisierung (Fehler 802) weist darauf hin, dass die Serveruhr möglicherweise um mehr als fünf Minuten von der aktuellen Uhrzeit abweicht. Passen Sie die Uhrzeit auf dem virtuellen Collector-Computer an die aktuelle Uhrzeit an:

1. Öffnen Sie auf dem virtuellen Computer eine Administratoreingabeaufforderung.
2. Führen Sie **w32tm /tz** aus, um die Zeitzone zu überprüfen.
3. Führen Sie **w32tm /resync** aus, um die Zeit zu synchronisieren.


## <a name="error-unabletoconnecttoserver"></a>Fehler UnableToConnectToServer

Wenn dieser Verbindungsfehler angezeigt wird, können Sie möglicherweise keine Verbindung mit vCenter Server „*Servername*.com:9443“ herstellen. Die Fehlerdetails weisen darauf hin, dass unter „https://*Servername*.com:9443/sdk“ kein Endpunkt lauscht, der die Nachricht akzeptieren kann.

- Überprüfen Sie, ob Sie die neueste Version der Appliance ausführen. Ist dies nicht der Fall, führen Sie ein Upgrade der Appliance auf die [neueste Version](https://docs.microsoft.com/azure/migrate/concepts-collector) durch.
- Wenn das Problem bei der neuesten Version weiterhin auftritt, ist die Appliance möglicherweise nicht in der Lage, den angegebenen vCenter Server-Namen aufzulösen, oder der angegebene Port ist eventuell falsch. Wenn der Port nicht angegeben ist, versucht Collector standardmäßig, eine Verbindung mit Port 443 herzustellen.

    1. Versuchen Sie, „*Servername*.com“ von der Appliance aus per Ping zu erreichen.
    2. Wenn Schritt 1 nicht erfolgreich ist, können Sie versuchen, eine Verbindung mit vCenter Server über die IP-Adresse herzustellen.
    3. Ermitteln Sie die richtige Portnummer für die Verbindung mit vCenter Server.
    4. Überprüfen Sie, ob vCenter Server ausgeführt wird und betriebsbereit ist.


## <a name="error-appliance-might-not-be-registered"></a>Fehler Die Appliance ist möglicherweise nicht registriert

- Der Fehler 60052 „Möglicherweise wurde die Appliance nicht erfolgreich beim Azure Migrate-Projekt registriert“ tritt auf, wenn das zum Registrieren der Appliance verwendete Azure-Konto nicht über ausreichende Berechtigungen verfügt.
    - Stellen Sie sicher, dass das für die Registrierung der Appliance verwendete Azure-Benutzerkonto mindestens über „Mitwirkender“-Berechtigungen für das Abonnement verfügt.
    - Weitere Informationen zu den erforderlichen Azure-Rollen und -Berechtigungen finden Sie [hier](https://docs.microsoft.com/azure/migrate/migrate-appliance#appliance-deployment-requirements).
- Der Fehler 60039 „Möglicherweise wurde die Appliance nicht erfolgreich beim Azure Migrate-Projekt registriert" kann auftreten, wenn die Registrierung fehlschlägt, da das zum Registrieren der Appliance verwendete Azure Migrate Projekt nicht gefunden werden kann.
    - Überprüfen Sie im Azure-Portal, ob das Projekt in der Ressourcengruppe vorhanden ist.
    - Sollte das Projekt nicht vorhanden sein, erstellen Sie in Ihrer Ressourcengruppe ein neues Azure Migrate-Projekt, und registrieren Sie die Appliance erneut. Informationen zum Erstellen eines neuen Projekts finden Sie [hier](https://docs.microsoft.com/azure/migrate/how-to-add-tool-first-time#create-a-project-and-add-a-tool).

## <a name="error-key-vault-management-operation-failed"></a>Fehler Ein Key Vault-Verwaltungsvorgang war nicht erfolgreich

Wenn der Fehler 60030 oder 60031 „Ein Azure Key Vault-Verwaltungsvorgang war nicht erfolgreich“ auftritt, gehen Sie wie folgt vor:
- Stellen Sie sicher, dass das für die Registrierung der Appliance verwendete Azure-Benutzerkonto mindestens über „Mitwirkender“-Berechtigungen für das Abonnement verfügt.
- Vergewissern Sie sich, dass das Konto Zugriff auf den in der Fehlermeldung angegebenen Key Vault hat, und wiederholen Sie dann den Vorgang.
- Wenn das Problem weiterhin besteht, wenden Sie sich an den Microsoft-Support.
- Weitere Informationen zu den erforderlichen Azure-Rollen und -Berechtigungen finden Sie [hier](https://docs.microsoft.com/azure/migrate/migrate-appliance#appliance-deployment-requirements).

## <a name="fix-discovery-couldnt-be-initiated"></a>Behebung: Die Ermittlung konnte nicht initiiert werden

Fehler 60028: „Die Ermittlung konnte aufgrund eines Fehlers nicht initiiert werden. Der Vorgang war für die angegebene Liste von Hosts oder Clustern nicht erfolgreich“. Dieser Fehler weist darauf hin, dass die Ermittlung aufgrund eines Problems beim Zugreifen auf oder Abrufen von VM-Informationen auf den in der Fehlermeldung aufgelisteten Hosts nicht gestartet werden konnte. Die übrigen Hosts wurden erfolgreich hinzugefügt.

- Fügen Sie die in der Fehlermeldung angegebenen Hosts mithilfe der Option **Host hinzufügen** erneut hinzu.
- Gehen Sie im Falle eines Überprüfungsfehlers gemäß der Korrekturanleitung vor, um den Fehler zu beheben, und verwenden Sie dann erneut die Option **Speichern und Ermittlung starten**.

## <a name="fix-azure-ad-operation-failed-60025"></a>Behebung: Fehler bei einem Azure AD-Vorgang (60025)

Fehler 60025: „Fehler bei einem Azure AD-Vorgang. Der Fehler trat beim Erstellen oder Aktualisieren der Azure AD-Anwendung auf“. Dieser Fehler tritt auf, wenn zum Initiieren der Ermittlung ein anderes Azure-Benutzerkonto als für die Registrierung der Appliance verwendet wird. Führen Sie einen der folgenden Schritte aus:

- Stellen Sie sicher, dass zum Initiieren der Ermittlung dasselbe Benutzerkonto verwendet wird wie zum Registrieren der Appliance.
- Weisen Sie dem Benutzerkonto, bei dem der Ermittlungsvorgang fehlgeschlagen ist, Zugriffsberechtigungen für die Azure Active Directory-Anwendung zu.
- Löschen Sie die Ressourcengruppe, die zuvor für das Azure Migrate-Projekt erstellt wurde. Erstellen Sie eine andere Ressourcengruppe, um neu zu beginnen.
- Weitere Informationen zu den Azure Active Directory-Anwendungsberechtigungen finden Sie [hier](https://docs.microsoft.com/azure/migrate/migrate-appliance#appliance-deployment-requirements).


## <a name="discovered-vms-not-in-portal"></a>Im Portal befinden sich keine ermittelten virtuellen Computer

Wenn Sie die Ermittlung starten und bei **Serverbewertung** und **Servermigration** zwar den Status **Ermittlung wird ausgeführt**, nicht aber die virtuellen Computer im Portal sehen können, beachten Sie Folgendes:

- Nach dem Start der Ermittlung von der Appliance dauert eine VMware-VM-Ermittlung ungefähr 15 Minuten und die Hyper-V-VM-Ermittlung für jeden hinzugefügten Host etwa zwei Minuten.
- Wenn nach dieser Wartezeit immer noch **Ermittlung wird ausgeführt** angezeigt wird, wählen Sie **Aktualisieren** auf der Registerkarte **Server** aus. Dadurch sollte bei **Serverbewertung** und **Servermigration** die Anzahl der ermittelten Server angezeigt werden.


## <a name="deleted-vms-in-the-portal"></a>Im Portal befinden sich gelöschte virtuelle Computer

Wenn Sie eine Appliance bereitgestellt haben, die fortlaufend die lokale Umgebung ermittelt, gelöschte virtuelle Computer jedoch weiterhin im Portal angezeigt werden, beachten Sie Folgendes:  

- Es kann bis zu 30 Minuten dauern, bis die von der Appliance gesammelten Ermittlungsdaten im Portal erscheinen.
- Wenn nach 30 Minuten keine aktuellen Informationen angezeigt werden, aktualisieren Sie die Daten mit den folgenden Schritten:

    1. Wechseln Sie zu **Server** > **Azure Migrate-Serverbewertung**, und wählen Sie **Übersicht** aus.
    2. Wählen Sie unter **Verwalten** die Option **Agent-Integrität** aus.
    3. Wählen Sie **Agent aktualisieren** aus.
    1. Warten Sie, bis der Aktualisierungsvorgang abgeschlossen wurde. Sie sollten nun aktuelle Informationen sehen.

## <a name="vm-information-isnt-in-the-portal"></a>Im Portal befinden sich keine VM-Informationen

- Es kann bis zu 30 Minuten dauern, bis die von der Appliance gesammelten Ermittlungsdaten im Portal erscheinen.
- Wenn nach 30 Minuten keine aktuellen Informationen angezeigt werden, aktualisieren Sie die Daten mit den folgenden Schritten:

    1. Wechseln Sie zu **Server** > **Azure Migrate-Serverbewertung**, und wählen Sie **Übersicht** aus.
    2. Wählen Sie unter **Verwalten** die Option **Agent-Integrität** aus.
    3. Wählen Sie **Agent aktualisieren** aus.
    1. Warten Sie, bis der Aktualisierungsvorgang abgeschlossen wurde. Sie sollten nun aktuelle Informationen sehen.


## <a name="fix-cant-connect-to-host-or-cluster"></a>Behebung: Verbindung mit Host oder Cluster nicht möglich

Fehler 50004: „Es ist keine Verbindung mit einem Host oder Cluster möglich, da der Servername nicht aufgelöst werden kann. WinRM-Fehlercode: 0x803381B9“. Dieser Fehler tritt auf, wenn der Azure DNS-Dienst für die Appliance den von Ihnen angegebenen Cluster- oder Hostnamen nicht auflösen kann.

- Wenn Sie diesen Fehler auf dem Cluster sehen, versuchen Sie, den vollqualifizierten Domänennamen (FQDN) des Clusters anzugeben.
- Sie sehen diesen Fehler eventuell auch für Hosts in einem Cluster. In diesem Fall kann die Appliance zwar eine Verbindung mit dem Cluster herstellen, doch gibt der Cluster Hostnamen zurück, bei denen es sich nicht um FQDNs handelt. Um diesen Fehler zu beheben, aktualisieren Sie die „hosts“-Datei auf der Appliance und fügen Sie ein Mapping von IP-Adresse und Hostnamen hinzu:
    1. Öffnen Sie Notepad als Administrator.
    2. Öffnen Sie die Datei „C:\Windows\System32\Drivers\etc\hosts“.
    3. Fügen Sie die IP-Adresse und den Hostnamen in einer Zeile hinzu. Wiederholen Sie dies für jeden Host oder Cluster, bei dem dieser Fehler angezeigt wird.
    4. Speichern und schließen Sie die Datei „hosts“.
    5. Überprüfen Sie mithilfe der Applianceverwaltungs-App, ob die Appliance eine Verbindung mit den Hosts herstellen kann. Nach 30 Minuten sollten die neuesten Informationen für diese Hosts im Azure-Portal angezeigt werden.



## <a name="fix-assessment-readiness"></a>Behebung: Bewertungsbereitschaft

Beheben Sie Probleme mit der Bewertungsbereitschaft wie folgt:

**Problem** | **Behebung**
--- | ---
Nicht unterstützter Starttyp | Azure unterstützt keine virtuellen Computer mit dem Starttyp „EFI“. Wir empfehlen, dass Sie vor einer Migration den Starttyp in „BIOS“ ändern. <br/><br/>Sie können die Azure Migrate-Servermigration verwenden, um die Migration solcher virtuellen Computer zu verarbeiten. Sie ändert den Starttyp des virtuellen Computers im Rahmen der Migration in „BIOS“.
Bedingt unterstütztes Windows-Betriebssystem | Der Unterstützungszeitraum für das Betriebssystem ist abgelaufen. Für die [Unterstützung in Azure](https://aka.ms/WSosstatement) wird eine benutzerdefinierte Supportvereinbarung (Custom Support Agreement, CSA) benötigt. Ziehen Sie vor der Migration zu Azure ein Upgrade in Erwägung.
Nicht unterstütztes Windows-Betriebssystem | Azure unterstützt nur [ausgewählte Windows-Betriebssystemversionen](https://aka.ms/WSosstatement). Ziehen Sie vor der Migration zu Azure ein Upgrade des Computers in Erwägung.
Bedingt unterstütztes Linux-Betriebssystem | Azure unterstützt nur [ausgewählte Linux-Betriebssystemversionen](../virtual-machines/linux/endorsed-distros.md). Ziehen Sie vor der Migration zu Azure ein Upgrade des Computers in Erwägung.
Nicht unterstütztes Linux-Betriebssystem | Der Computer startet möglicherweise in Azure, aber Azure stellt keine Betriebssystemunterstützung bereit. Ziehen Sie vor der Migration zu Azure ein Upgrade auf eine [unterstützte Linux-Version](../virtual-machines/linux/endorsed-distros.md) in Erwägung.
Unbekanntes Betriebssystem | Das Betriebssystem der VM wurde in vCenter Server als „Sonstige“ angegeben. Dieses Verhalten hindert Azure Migrate daran, die Azure-Bereitschaft der VM zu überprüfen. Stellen Sie sicher, dass das Betriebssystem von Azure [unterstützt](https://aka.ms/azureoslist) wird, bevor Sie den Computer migrieren.
Nicht unterstützte Bitversion | Virtuelle Computer mit einem 32-Bit-Betriebssystem starten zwar unter Umständen in Azure, es empfiehlt sich jedoch, vor der Migration zu Azure ein Upgrade auf 64-Bit durchzuführen.
Erfordert ein Microsoft Visual Studio-Abonnement. | Auf dem Computer wird ein Windows-Clientbetriebssystem ausgeführt, das nur mit einem Visual Studio-Abonnement unterstützt wird.
Kein virtueller Computer für erforderliche Speicherleistung gefunden. | Die erforderliche Speicherleistung (Eingabe/Ausgabe-Vorgänge pro Sekunde (IOPS) und Durchsatz) für den Computer überschreitet die von virtuellen Azure-Computern unterstützte Leistung. Reduzieren Sie vor der Migration die Speicheranforderungen für den Computer.
Kein virtueller Computer für erforderliche Netzwerkleistung gefunden. | Die erforderliche Netzwerkleistung (ein-/ausgehend) für den Computer überschreitet die von virtuellen Azure-Computern unterstützte Leistung. Reduzieren Sie die Netzwerkanforderungen für den Computer.
Kein virtueller Computer am angegebenen Speicherort gefunden. | Geben Sie vor der Migration einen anderen Zielort an.
Mindestens ein Datenträger ist ungeeignet. | Mindestens ein an den virtuellen Computer angeschlossener Datenträger erfüllt die Azure-Anforderungen nicht.<br/><br/> Von der Azure Die Serverbewertung unterstützt derzeit keine SSD Ultra-Datenträger und bewertet die Datenträger anhand der Datenträgergrenzwerte für verwaltete Premium-Datenträger (32 TB).<br/><br/> Stellen Sie für jeden an den virtuellen Computer angeschlossenen Datenträger sicher, dass die Größe des Datenträgers kleiner als 64 TB (von SSD Ultra-Datenträgern unterstützt) ist.<br/><br/> Wenn dies nicht der Fall ist, reduzieren Sie die Datenträgergröße vor der Migration zu Azure, oder verwenden Sie in Azure mehrere Datenträger, und [kombinieren Sie sie in Stripesets](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance#disk-striping), um höhere Speichergrenzwerte zu erzielen. Stellen Sie sicher, dass die von jedem Datenträger benötigte Leistung (IOPS und Durchsatz) von [verwalteten Azure-Datenträgern virtueller Computer](https://docs.microsoft.com/azure/azure-subscription-service-limits#storage-limits) unterstützt wird.
Mindestens ein Netzwerkadapter ist ungeeignet. | Entfernen Sie nicht verwendete Netzwerkadapter vor der Migration vom Computer.
Anzahl der Datenträger überschreitet den Grenzwert | Entfernen Sie nicht verwendete Datenträger vor der Migration vom Computer.
Datenträgergröße überschreitet den Grenzwert | Von der Azure Die Serverbewertung unterstützt derzeit keine SSD Ultra-Datenträger und bewertet die Datenträger anhand der Grenzwerte für Premium-Datenträger (32 TB).<br/><br/> Azure unterstützt jedoch Datenträger mit einer Größe von bis zu 64 TB (von SSD Ultra-Datenträgern unterstützt). Reduzieren Sie die Datenträgergröße vor der Migration auf weniger als 64 TB, oder verwenden Sie mehrere Datenträger in Azure, und [kombinieren Sie sie in Stripesets](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance#disk-striping), um höhere Speichergrenzwerte zu erzielen.
Datenträger am angegebenen Speicherort nicht verfügbar | Stellen Sie vor der Migration sicher, dass sich der Datenträger am Zielspeicherort befindet.
Datenträger für die angegebene Redundanz nicht verfügbar | Der Datenträger muss den in den Bewertungseinstellungen definierten Redundanzspeichertyp verwenden (standardmäßig LRS).
Datenträgereignung konnte aufgrund eines internen Fehlers nicht ermittelt werden. | Versuchen Sie, eine neue Bewertung für die Gruppe zu erstellen.
Kein virtueller Computer gefunden, der die Kern- und Arbeitsspeicheranforderungen erfüllt | Azure konnte keinen geeigneten VM-Typ finden. Reduzieren Sie vor der Migration den Arbeitsspeicher und die Anzahl der Kerne auf dem lokalen Computer.
Eignung des virtuellen Computers konnte aufgrund eines internen Fehlers nicht ermittelt werden. | Versuchen Sie, eine neue Bewertung für die Gruppe zu erstellen.
Eignung konnte für mindestens einen Datenträger aufgrund eines internen Fehlers nicht ermittelt werden. | Versuchen Sie, eine neue Bewertung für die Gruppe zu erstellen.
Eignung konnte für mindestens einen Netzwerkadapter aufgrund eines internen Fehlers nicht ermittelt werden. | Versuchen Sie, eine neue Bewertung für die Gruppe zu erstellen.

## <a name="linux-vms-are-conditionally-ready"></a>Virtuelle Linux-Computer sind „bedingt bereit“

Bei der Serverbewertung werden virtuelle Linux-Computer aufgrund einer bekannten Lücke in der Serverbewertung als „bedingt bereit“ gekennzeichnet.

- Die Lücke verhindert die Erkennung der Nebenversion des Linux-Betriebssystems, das auf den lokalen virtuellen Computern installiert ist.
- Beispielsweise erkennt die Serverbewertung bei RHEL 6.10 derzeit nur RHEL 6 als Betriebssystemversion.
-  Da Azure nur bestimmte Versionen von Linux unterstützt, werden die virtuellen Linux-Computer in der Serverbewertung derzeit als „bedingt bereit“ markiert.
- Sie können ermitteln, ob das Linux-Betriebssystem des lokalen virtuellen Computers für Azure geeignet ist. Informationen hierzu finden Sie unter [Linux-Unterstützung von Azure](https://aka.ms/migrate/selfhost/azureendorseddistros).
-  Nachdem Sie die unterstützte Distribution überprüft haben, können Sie diese Warnung ignorieren.

## <a name="azure-skus-exceed-on-premises-sizing"></a>Azure-SKUs überschreiten lokale Dimensionierung

Je nach Art der Bewertung empfiehlt die Azure Migrate-Serverbewertung möglicherweise Azure-VM-SKUs mit mehr Kernen und Arbeitsspeicher als bei der aktuellen lokalen Zuordnung:


- Die Empfehlung für VM-SKUs richtet sich nach den Bewertungseigenschaften.
- Dies hängt von der Art der Bewertung ab, die Sie in der Serverbewertung ausführen: *Leistungsbasiert* oder *Wie lokal*.
- Bei leistungsbasierten Bewertungen berücksichtigt die Serverbewertung die Nutzungsdaten der lokalen virtuellen Computer (CPU-, Speicher-, Datenträger- und Netzwerkauslastung), um die richtige Ziel-VM-SKU für Ihre lokalen virtuellen Computer zu bestimmen. Außerdem wird bei der Bestimmung der effektiven Auslastung ein Komfortfaktor hinzugefügt.
- Bei der lokalen Dimensionierung werden Leistungsdaten nicht berücksichtigt, und die Ziel-SKU wird auf der Grundlage der lokalen Zuordnung empfohlen.

Um zu veranschaulichen, wie sich dies auf Empfehlungen auswirken kann, sehen wir uns ein Beispiel an:

Wir haben einen lokalen virtuellen Computer mit vier Kernen und 8 GB Arbeitsspeicher, mit einer CPU- und Speicherauslastung von jeweils 50 % und einem angegebenen Komfortfaktor von 1,3.

-  Beim Bewertungstyp **Wie lokal** wird eine Azure-VM-SKU mit 4 Kernen und 8 GB Arbeitsspeicher empfohlen.
- Bei der leistungsbasierten Bewertung würde auf Basis der effektiven CPU- und Speicherauslastung (50 % von 4 Kernen * 1,3 = 2,6 Kerne und 50 % von 8 GB Arbeitsspeicher * 1,3 = 5,3 GB Arbeitsspeicher) die günstigste VM-SKU mit vier Kernen (nächste unterstützte Kernanzahl) und 8 GB Arbeitsspeichergröße (nächste unterstützte Arbeitsspeichergröße) empfohlen.
- Weitere Informationen zur Dimensionierung bei Bewertungen finden Sie [hier](concepts-assessment-calculation.md#sizing).

## <a name="azure-disk-skus-bigger-than-on-premises"></a>Azure-Datenträger-SKUs größer als lokal

Die Azure Migrate-Serverbewertung empfiehlt möglicherweise je nach Art der Bewertung einen größeren Datenträger.
- Die Datenträgerdimensionierung in der Serverbewertung hängt von zwei Bewertungseigenschaften ab: dem Dimensionierungskriterium und dem Speichertyp.
- Wenn das Dimensionierungskriterium **Leistungsbasiert** ist und der Speichertyp **Automatisch** lautet, werden bei der Ermittlung des Zieldatenträgertyps (HDD Standard, SSD Standard oder Premium) die IOPS- und Durchsatzwerte des Datenträgers berücksichtigt. Eine Datenträger-SKU dieses Datenträgertyps wird dann unter Berücksichtigung der Größenanforderungen des lokalen Datenträgers empfohlen.
- Wenn das Dimensionierungskriterium **Leistungsbasiert** ist und der Speichertyp **Premium** lautet, wird eine Premium-Datenträger-SKU in Azure basierend auf den IOPS, dem Durchsatz und den Größenanforderungen des lokalen Datenträgers empfohlen. Die gleiche Logik wird verwendet, um die Datenträgerdimensionierung durchzuführen, wenn das Dimensionierungskriterium **Wie lokal** verwendet wird und der Speichertyp **HDD Standard**, **SSD Standard** oder **Premium** lautet.

Wenn Sie beispielsweise einen lokalen Datenträger mit 32 GB Speicher haben, die aggregierten Lese- und Schreib-IOPS für den Datenträger aber 800 IOPS sind, empfiehlt die Serverbewertung einen Premium-Datenträger (aufgrund der höheren IOPS-Anforderungen) und dann eine Datenträger-SKU, welche die erforderlichen IOPS und die Größe unterstützen kann. Die nächstliegende Übereinstimmung in diesem Beispiel wäre P15 (256 GB, 1100 IOPS). Für den lokalen Datenträger ist also nur eine Größe von 32 GB erforderlich, von der Serverbewertung wurde jedoch aufgrund der hohen IOPS-Anforderungen des lokalen Datenträgers ein größerer Datenträger empfohlen.

## <a name="fix-percentage-of-utilized-core-or-memory-missing"></a>Behebung: Prozentsatz des ausgelasteten Kerns oder Speichers fehlt

Die Serverbewertung gibt die Meldung „PercentageOfCoresUtilizedMissing“ oder „PercentageOfMemoryUtilizedMissing“ aus, wenn die Azure Migrate-Appliance keine Leistungsdaten für den relevanten lokalen virtuellen Computer sammeln kann.

- Dies kann vorkommen, wenn die virtuellen Computer während der Bewertung ausgeschaltet sind. Die Appliance kann keine Leistungsdaten für eine VM erfassen, wenn sie ausgeschaltet ist.
- Falls nur Arbeitsspeicher-Leistungsindikatoren fehlen und Sie versuchen, virtuelle Hyper-V-Computer zu bewerten, überprüfen Sie, ob auf diesen virtuellen Computern dynamischer Arbeitsspeicher aktiviert ist. Aufgrund eines bekannten Problems, das nur bei virtuellen Hyper-V-Computern auftritt, kann die Azure Migrate-Appliance keine Arbeitsspeichernutzungsdaten für virtuelle Computer sammeln, für die kein dynamischer Arbeitsspeicher aktiviert ist.
- Wenn einer der Leistungsindikatoren fehlt, greift die Azure Migrate-Serverbewertung auf die zugewiesenen Kerne und den Arbeitsspeicher zurück und empfiehlt eine entsprechende VM-Größe.

## <a name="is-the-vm-os-license-cost-included-in-cost-assessment"></a>Sind die Lizenzkosten für das VM-Betriebssystem in der Kostenbewertung enthalten?

Die Azure Migrate-Serverbewertung berücksichtigt zurzeit die Kosten einer Betriebssystemlizenz nur für Windows-Computer. Die Lizenzkosten für Linux-Computer werden derzeit nicht berücksichtigt.

## <a name="performance-history-and-percentile-use"></a>Leistungsverlauf und Perzentilverwendung

Diese Eigenschaften gelten bei der Azure Migrate-Serverbewertung nur für die leistungsbasierte Dimensionierung.

Die Serverbewertung sammelt fortlaufend Leistungsdaten von lokalen Computern und nutzt diese Daten, um die VM-SKU und die Datenträger-SKU in Azure zu empfehlen. Diese Leistungsdaten werden wie folgt von der Serverbewertung erfasst:
- Die Azure Migrate-Appliance erstellt kontinuierlich Profile der lokalen Umgebung, um alle 20 Sekunden Echtzeit-Nutzungsdaten für virtuelle VMware-Computer und alle 30 Sekunden für virtuelle Hyper-V-Computer zu erfassen.
- Die Appliance führt für die 20- bzw. 30-Sekunden-Stichproben einen Rollup aus, um alle 10 Minuten einen Datenpunkt zu erstellen. Zum Erstellen jedes Datenpunkts wählt die Appliance den Spitzenwert aus allen 20-Sekunden- und 30-Sekunden-Stichproben aus und sendet diesen dann an Azure.
- Beim Erstellen einer Bewertung in der Serverbewertung, die auf der Leistungsdauer und dem Quantilwert des Leistungsverlaufs basiert, wird der repräsentative Nutzungswert ermittelt. Wenn der Leistungsverlauf beispielsweise eine Woche beträgt und die Quantilauslastung 95. ist, sortiert Azure Migrate alle 10-minütigen Stichprobenpunkte für die letzte Woche in aufsteigender Reihenfolge und wählt dann das 95. Perzentil als repräsentativen Wert aus.
- Mit dem Wert des 95. Quantils wird sichergestellt, dass Ausreißer ignoriert werden. Diese können enthalten sein, wenn Sie das 99. Quantil wählen.
- Falls Sie die Spitzenauslastung für den Zeitraum wählen möchten und keine Ausreißer verpassen möchten, sollten Sie das 99. Quantil als Quantilauslastung wählen.



## <a name="i-cant-find-dependency-visualization-for-azure-government"></a>Ich kann die Abhängigkeitsvisualisierung für Azure Government nicht finden

Azure Migrate ist bei der Funktion zur Visualisierung von Abhängigkeiten von Service Map abhängig. Da Service Map in Azure Government zurzeit nicht verfügbar ist, ist diese Funktionalität auch nicht in Azure Government verfügbar.

## <a name="dependencies-dont-show-after-installing-agents"></a>Abhängigkeiten werden nach der Installation von Agents nicht angezeigt


Nachdem Sie die Agents für die Abhängigkeitsvisualisierung auf lokalen virtuellen Computern installiert haben, benötigt Azure Migrate üblicherweise 15 bis 30 Minuten, um die Abhängigkeiten im Portal anzuzeigen. Stellen Sie nach einer Wartezeit von mehr als 30 Minuten sicher, dass Microsoft Monitoring Agent (MMA) eine Verbindung zum Log Analytics-Arbeitsbereich herstellen kann.

Für virtuelle Windows-Computer:
1. Starten Sie MMA in der Systemsteuerung.
2. Stellen Sie unter **Microsoft Monitoring Agent-Eigenschaften** > **Azure Log Analytics (OMS)** sicher, dass der **Status** für den Arbeitsbereich grün angezeigt wird.
3. Wenn der Status nicht grün angezeigt wird, entfernen Sie den Arbeitsbereich, und fügen Sie ihn erneut zu MMA hinzu.

      ![Dialogfeld „MMA-Eigenschaften“](./media/troubleshooting-general/mma-status.png)

Stellen Sie bei virtuellen Linux-Computern sicher, dass die Installationsbefehle für MMA und den Abhängigkeits-Agent erfolgreich ausgeführt wurden.

## <a name="supported-mma-os"></a>Unterstütztes Betriebssystem für MMA

 Überprüfen Sie die unterstützten [Windows](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems)- und [Linux](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems)-Betriebssysteme.

## <a name="supported-dependency-agent-os"></a>Unterstütztes Betriebssystem für Abhängigkeits-Agent

Überprüfen Sie die unterstützten [Windows- und Linux-](../azure-monitor/insights/vminsights-enable-overview.md#supported-operating-systems)Betriebssysteme.

## <a name="dependencies-for-more-than-an-hour"></a>Abhängigkeiten für mehr als eine Stunde

Mit Azure Migrate können Sie zwar um bis zu einen Monat zu einem bestimmten Datum zurückgehen. Die Abhängigkeiten können Sie jedoch nur für einen Zeitraum von maximal einer Stunde visualisieren.

So können Sie beispielsweise mithilfe der Zeitraumfunktionalität im Abhängigkeitsdiagramm Abhängigkeiten für den gestrigen Tag, jedoch nur für einen Zeitraum von einer Stunde, anzeigen.

Sie können jedoch zum [Abfragen der Abhängigkeitsdaten](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies) über einen längeren Zeitraum Azure Monitor-Protokolle verwenden.

## <a name="i-cant-visualize-dependencies-for-groups-with-more-than-10-vms"></a>Für Gruppen mit mehr als 10 virtuellen Computern kann ich keine Abhängigkeiten visualisieren

Bei der Azure Migrate-Serverbewertung können Sie [Abhängigkeiten für Gruppen](https://docs.microsoft.com/azure/migrate/how-to-create-group-dependencies) mit bis zu 10 virtuellen Computern visualisieren. Bei größeren Gruppen empfiehlt es sich für die visuelle Darstellung von Abhängigkeiten, die virtuellen Computer in kleinere Gruppen aufzuteilen.

## <a name="machines-show-install-agent-not-view-dependencies"></a>Computer zeigen „Agent installieren“ und nicht „Abhängigkeiten anzeigen“ an

Nach dem Migrieren von Computern mit aktivierter Abhängigkeitsvisualisierung zu Azure zeigen Computer möglicherweise aufgrund des folgenden Verhaltens die Aktion „Agent installieren“ anstelle von „Abhängigkeiten anzeigen“ an:


- Nach der Migration zu Azure werden die lokalen Computer ausgeschaltet, und die entsprechenden virtuellen Computer werden in Azure hochgefahren. Diese Computer beziehen eine andere MAC-Adresse.
- Je nachdem, ob Sie die lokale IP-Adresse beibehalten haben oder nicht, haben Computer möglicherweise auch eine andere IP-Adresse.
- Wenn sich sowohl Mac- als auch IP-Adressen von den lokalen Adressen unterscheiden, verknüpft Azure Migrate die lokalen Computer nicht mit Service Map-Abhängigkeitsdaten. In diesem Fall wird die Option zum Installieren des Agents anstatt die Option zum Anzeigen von Abhängigkeiten angezeigt.
- Nach einer Testmigration zu Azure bleiben die lokalen Computer erwartungsgemäß eingeschaltet. Entsprechende Computer, die in Azure hochgefahren wurden, erhalten andere MAC-Adressen und u. U. auch andere IP-Adressen. Sofern Sie ausgehenden Datenverkehr von Azure Monitor-Protokollen von diesen Computern nicht sperren, ordnet Azure Migrate den lokalen Computern keine Service Map-Abhängigkeitsdaten zu und zeigt daher die Option zum Installieren von Agents statt der Option zum Anzeigen von Abhängigkeiten an.


## <a name="collect-network-traffic-logs-in-portal"></a>Sammeln von Protokollen zum Netzwerkdatenverkehr im Portal

Sammeln Sie Protokolle wie folgt:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Drücken Sie F12, um die Entwicklertools zu starten. Deaktivieren Sie ggf. die Einstellung **Einträge beim Navigieren löschen**.
3. Wählen Sie die Registerkarte **Netzwerk** aus, und starten Sie die Erfassung von Netzwerkdatenverkehr:
   - Wählen Sie in Chrome die Option **Protokoll speichern**. Die Aufzeichnung sollte automatisch gestartet werden. Ein roter Kreis gibt an, dass der Datenverkehr erfasst wird. Wenn der rote Kreis nicht angezeigt wird, wählen Sie zum Starten den schwarzen Kreis aus.
   - In Microsoft Edge und Internet Explorer sollte die Aufzeichnung automatisch gestartet werden. Ist dies nicht der Fall, wählen Sie die grüne Wiedergabeschaltfläche aus.
4. Versuchen Sie, den Fehler zu reproduzieren.
5. Nachdem der Fehler während der Aufzeichnung aufgetreten ist, beenden Sie die Aufzeichnung, und speichern Sie eine Kopie der aufgezeichneten Aktivität:
   - Klicken Sie in Chrome mit der rechten Maustaste, und wählen Sie **Als HAR mit Inhalt speichern** aus. Durch diese Aktion werden die Protokolle komprimiert und als HAR-Datei exportiert.
   - Wählen Sie in Microsoft Edge oder Internet Explorer das Symbol **Aufgezeichneten Datenverkehr exportieren** aus. Durch diese Aktion werden die Protokolle komprimiert und exportiert.
6. Wählen Sie die Registerkarte **Konsole** aus, um sie auf Warnungen oder Fehler zu überprüfen. So speichern Sie das Konsolenprotokoll:
   - Klicken Sie in Chrome mit der rechten Maustaste auf eine beliebige Stelle im Konsolenprotokoll. Wählen Sie **Speichern unter** aus, um das Protokoll zu exportieren und zu komprimieren.
   - Klicken Sie in Microsoft Edge oder Internet Explorer mit der rechten Maustaste auf die Fehler, und wählen Sie **Alle kopieren** aus.
7. Schließen Sie die Entwicklertools.
