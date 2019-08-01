---
title: Behandeln von Problemen bei Azure Migrate | Microsoft-Dokumentation
description: Bietet eine Übersicht über bekannte Probleme im Azure Migrate-Dienst und Problembehandlungstipps für häufige Fehler.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/17/2019
ms.author: raynew
ms.openlocfilehash: 0e2a8f269a98babc17f36ceff209ee2f057e6911
ms.sourcegitcommit: af58483a9c574a10edc546f2737939a93af87b73
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2019
ms.locfileid: "68302323"
---
# <a name="troubleshoot-azure-migrate"></a>Problembehandlung für Azure Migrate

[Azure Migrate](migrate-services-overview.md) stellt einen Hub mit Microsoft-Tools für die Bewertung und Migration sowie unabhängige Drittanbietertools bereit. Dieses Dokument bietet Hilfe bei der Problembehandlung von Fehlern bei Azure Migrate, Azure Migrate an: Serverbewertung und Azure Migrate: Servermigration.

## <a name="azure-migrate-project-issues"></a>Probleme mit Azure Migrate-Projekten

### <a name="i-am-unable-to-find-my-existing-azure-migrate-project"></a>Ich kann mein vorhandenes Azure Migrate-Projekt nicht finden.

Es sind [zwei Versionen](https://docs.microsoft.com/azure/migrate/migrate-services-overview#azure-migrate-versions) von Azure Migrate verfügbar. Je nachdem, in welcher Version Sie das Projekt angelegt haben, können Sie die folgenden Schritte ausführen, um nach dem Projekt zu suchen:

1. Wenn Sie nach einem Projekt suchen, das mit der vorherigen Version (Vorgängerversion) von Azure Migrate erstellt wurde, führen Sie die folgenden Schritte aus, um das Projekt zu finden.

    1. Wechseln Sie zum [Azure-Portal](https://portal.azure.com) und suchen Sie dort nach **Azure Migrate**.
    2. Im Azure Migrate-Dashboard sehen Sie ein Banner, das über den Zugriff auf ältere Projekte informiert. Dieses Banner erscheint nur, wenn Sie ein Projekt mit der alten Version erstellt haben. Klicken Sie auf das Banner.

    ![Zugriff auf vorhandene Projekte](./media/troubleshooting-general/access-existing-projects.png)

    3. Sie sehen nun die Liste der vorhandenen Projekte, die mit der Vorgängerversion von Azure Migrate erstellt wurden.

2. Wenn Sie nach einem Projekt suchen, das mit der aktuellen Version (neue Version) von Azure Migrate erstellt wurde, führen Sie die folgenden Schritte aus, um das Projekt zu finden.

    1. Wechseln Sie zum [Azure-Portal](https://portal.azure.com) und suchen Sie dort nach **Azure Migrate**.
    2. Rufen Sie im Dashboard von Azure Migrate die Seite **„Server“** links auf und wählen Sie oben rechts in der Ecke **„Ändern“** in der rechten oberen Ecke:

    ![Wechseln zu einem vorhandenen Azure Migrate-Projekt](./media/troubleshooting-general/switch-project.png)

    3. Wählen Sie das entsprechende **Abonnement** und das **Migrationsprojekt** aus.

### <a name="i-am-unable-to-create-a-second-azure-migrate-project"></a>Ich kann kein zweites Azure Migrate-Projekt erstellen.

Führen Sie die folgenden Schritte aus, um ein neues Azure Migrate-Projekt zu erstellen.

1. Wechseln Sie zum [Azure-Portal](https://portal.azure.com) und suchen Sie dort nach **Azure Migrate**.
2. Rufen Sie im Dashboard von Azure Migrate die Seite **„Server“** links auf und wählen Sie oben rechts in der Ecke **„Ändern“** in der rechten oberen Ecke:

   ![Ändern eines Azure Migrate-Projekts](./media/troubleshooting-general/switch-project.png)

3. Um ein neues Projekt zu erstellen, **klicken Sie hier**, wie im Screenshot unten gezeigt:

   ![Erstellen eines zweiten Azure Migrate-Projekts](./media/troubleshooting-general/create-new-project.png)

### <a name="deletion-of-azure-migrate-projects-and-associated-log-analytics-workspace"></a>Löschen von Azure Migrate-Projekten und zugehörigen Log Analytics-Arbeitsbereichen

Wenn Sie ein Azure Migrate-Projekt löschen, wird das Migrationsprojekt zusammen mit den Metadaten zu den ermittelten Computern gelöscht. Wenn Sie dem Serverbewertungstool jedoch einen Log Analytics-Arbeitsbereich angefügt hatten, wird dieser nicht automatisch gelöscht. Dies liegt daran, dass derselbe Log Analytics-Arbeitsbereich für mehrere Anwendungsfälle verwendet werden kann. Wenn Sie den Log Analytics-Arbeitsbereich ebenfalls löschen möchten, müssen Sie dies manuell tun.

1. Navigieren Sie zu dem Log Analytics-Arbeitsbereich, der dem Projekt angefügt ist.
     1. Wenn Sie das Migrationsprojekt noch nicht gelöscht haben, finden Sie den Link zum Arbeitsbereich auf der Übersichtsseite der Serverbewertung im Abschnitt „Essentials“.

     ![LA-Arbeitsbereich](./media/troubleshooting-general/loganalytics-workspace.png)

     2. Wenn Sie das Migrationsprojekt bereits gelöscht haben, wählen Sie im linken Bereich des Azure-Portals **„Ressourcengruppen“** . Rufen Sie die Ressourcengruppe auf, in der der Arbeitsbereich erstellt wurde, und navigieren Sie dann dorthin.
2. Befolgen Sie die Anweisungen [in diesem Artikel](https://docs.microsoft.com/azure/azure-monitor/platform/delete-workspace), um den Arbeitsbereich zu löschen.

### <a name="migration-project-creation-failed-with-error-requests-must-contain-user-identity-headers"></a>Erstellen des Migrationsprojekts mit dem Fehler *Anforderungen müssen Benutzeridentitätsheader enthalten*

Dieses Problem kann für Benutzer auftreten, die nicht über Zugriff auf den Azure Active Directory (Azure AD)-Mandanten der Organisation verfügen. Wenn ein Benutzer zum ersten Mal zu einem Azure AD-Mandanten hinzugefügt wird, erhält er eine E-Mail-Einladung, dem Mandanten beizutreten. Benutzer müssen die E-Mail aufrufen und die Einladung annehmen, um erfolgreich zum Mandanten hinzugefügt zu werden. Wenn Sie die E-Mail nicht sehen können, wenden Sie sich an einen Benutzer, der bereits auf den Mandanten zugreifen kann, und bitten Sie ihn, die Einladung anhand der [hier](https://docs.microsoft.com/azure/active-directory/b2b/add-users-administrator#resend-invitations-to-guest-users) angegebenen Schritte erneut zu senden.

Wenn Sie die Einladung per E-Mail erhalten haben, öffnen Sie die diese und klicken Sie auf den darin angegebenen Link, um die Einladung anzunehmen. Sobald dies geschehen ist, müssen Sie sich beim Azure-Portal ab- und erneut anmelden. Das Aktualisieren des Browsers allein funktioniert nicht. Sie können dann versuchen, das Migrationsprojekt zu erstellen.

## <a name="appliance-issues"></a>Appliance-Probleme

### <a name="deployment-of-azure-migrate-appliance-for-vmware-failed-with-the-error-the-provided-manifest-file-is-invalid-invalid-ovf-manifest-entry"></a>Die Bereitstellung der Azure Migrate-Appliance für VMware schlägt mit folgendem Fehler fehl: Die angegebene Manifestdatei ist ungültig: Ungültiger OVF-Manifesteintrag.

1. Überprüfen Sie, ob die OVA-Datei für die Azure Migrate-Appliance ordnungsgemäß heruntergeladen wird, indem Sie deren Hashwert überprüfen. Informationen zum Überprüfen des Hashwerts finden Sie in [diesem Artikel](https://docs.microsoft.com/azure/migrate/tutorial-assessment-vmware#verify-the-collector-appliance). Wenn der Hashwert nicht übereinstimmt, laden Sie die OVA-Datei erneut herunter, und wiederholen Sie die Bereitstellung.
2. Wenn der Fehler weiterhin auftritt und Sie zum Bereitstellen der OVF-Datei VMware vSphere-Client verwenden, führen Sie die Bereitstellung über den vSphere-Webclient aus. Wenn der Fehler weiterhin auftritt, versuchen Sie es mit einem anderen Webbrowser.
3. Wenn Sie den vSphere-Webclient und die Bereitstellung auf vCenter Server 6.5 oder 6.7 ausführen möchten, stellen Sie die OVA-Datei mit den folgenden Schritten direkt auf dem ESXi-Host bereit:
   - Stellen Sie mithilfe des Webclients (https://<*Host-IP-Adresse*>/ui) eine direkte Verbindung mit dem ESXi-Host her (anstelle von vCenter Server).
   - Wechseln Sie zu **Home** > **Inventar**.
   - Klicken Sie auf **Datei** > **OVF-Vorlage bereitstellen**. Navigieren Sie zur OVA und schließen Sie die Bereitstellung ab.
4. Wenn weiterhin ein Fehler bei der Bereitstellung auftritt, wenden Sie sich an den Azure Migrate-Support.

### <a name="appliance-is-not-able-to-connect-to-the-internet"></a>Die Appliance kann keine Verbindung mit dem Internet herstellen

Dies kann passieren, wenn sich der Computer, den Sie verwenden, hinter einem Proxy befindet. Stellen Sie sicher, dass Sie die Anmeldeinformationen für die Autorisierung angeben, wenn der Proxy diese benötigt.
Wenn Sie URL-basierte Firewallproxys zur Steuerung ausgehender Verbindungen verwenden, stellen Sie sicher, dass die folgenden erforderlichen Dienst-URLs zur Liste der zugelassenen Dienste hinzugefügt werden:

Szenario | URL-Liste
--- | ---
Serverbewertung für VMware | [Hier](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#assessment-url-access-requirements)
Serverbewertung für Hyper-V | [Hier](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-hyper-v#assessment-appliance-url-access)
Servermigration für VMware (ohne Agent) | [Hier](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#agentless-migration-url-access-requirements)
Servermigration für VMware (mit Agent) | [Hier](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#replication-appliance-url-access)
Servermigration für Hyper-V | [Hier](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-hyper-v#migration-hyper-v-host-url-access)

Wenn Sie einen abfangenden Proxy verwenden, um eine Internetverbindung herzustellen, dann müssen Sie das Proxyzertifikat in die Appliance-VM importieren. Eine Anleitung zum Importieren des Proxyzertifikats finden Sie [hier](https://docs.microsoft.com/azure/migrate/concepts-collector).

### <a name="error-802-date-and-time-synchronization-error"></a>Fehler 802: Fehler bei der Datums- und Zeitsynchronisierung

Möglicherweise weicht die Serveruhr um mehr als fünf Minuten von der aktuellen Uhrzeit ab. Passen Sie die Uhrzeit auf dem virtuellen Collector-Computer folgendermaßen an die aktuelle Uhrzeit an:

1. Öffnen Sie auf dem virtuellen Computer eine Administratoreingabeaufforderung.
2. Führen Sie „w32tm /tz“ aus, um die Zeitzone zu überprüfen.
3. Führen Sie „w32tm /resync“ aus, um die Zeit zu synchronisieren.


### <a name="error-unabletoconnecttoserver"></a>Fehler: UnableToConnectToServer

Wegen folgendem Fehler kann keine Verbindung mit vCenter Server „Servername.com:9443“ hergestellt werden: Unter https://Servername.com:9443/sdk hat kein Endpunkt gelauscht, der die Nachricht akzeptieren konnte.

Überprüfen Sie, ob die neueste Version der Collectorappliance ausgeführt wird. Wenn das nicht der Fall ist, aktualisieren Sie auf die [neueste Version](https://docs.microsoft.com/azure/migrate/concepts-collector).

Wenn das Problem auch bei der neuesten Version auftritt, kann der Collectorcomputer den angegebenen vCenter Server-Namen möglicherweise nicht auflösen oder der angegebene Port ist falsch. Wenn der Port nicht angegeben ist, versucht Collector standardmäßig, eine Verbindung mit Port 443 herzustellen.

1. Versuchen Sie, „Servername.com“ vom Collectorcomputer aus per Ping zu erreichen.
2. Wenn Schritt 1 nicht erfolgreich ist, können Sie versuchen, über die IP-Adresse eine Verbindung mit vCenter Server herzustellen.
3. Ermitteln Sie die richtige Portnummer für die Verbindungsherstellung mit vCenter.
4. Überprüfen Sie abschließend, ob vCenter Server ausgeführt wird und betriebsbereit ist.

## <a name="discovery-issues"></a>Probleme bei der Ermittlung

### <a name="i-started-discovery-but-i-dont-see-the-discovered-vms-on-azure-portal-server-assessment-and-server-migrate-tiles-show-a-status-of-discovery-in-progress"></a>Ich habe mit der Ermittlung begonnen, aber ich kann die ermittelten VMs nicht im Azure-Portal finden. Die Kacheln für Serverbewertung und Servermigration zeigen den Status „Erkennung wird ausgeführt“
Nachdem Sie mit der Ermittlung durch das Gerät begonnen haben, warten Sie einen Moment, bis die gefundenen Geräte auf dem Azure-Portal angezeigt werden. Eine VMware-Ermittlung dauert etwa 15 Minuten und eine Hyper-V-Ermittlung benötigt 2 Minuten pro hinzugefügtem Host. Wenn „Erkennung wird ausgeführt“ nach Ablauf dieser Zeit immer noch angezeigt wird, dann klicken Sie **„Aktualisieren“** in der Registerkarte **„Server“** . Dies sollte die Anzahl der ermittelten Server in den Kacheln „Serverbewertung“ und „Servermigration“ anzeigen.


### <a name="i-am-using-the-appliance-that-continuously-discovers-my-on-premises-environment-but-the-vms-that-are-deleted-in-my-on-premises-environment-are-still-being-shown-in-the-portal"></a>Ich verwende die Appliance, die kontinuierlich meine lokale Umgebung ermittelt, aber die VMs, die in meiner lokalen Umgebung gelöscht wurden, werden immer noch im Portal angezeigt.

Es kann bis zu 30 Minuten dauern, bis die von der Appliance gesammelten Ermittlungsdaten im Portal erscheinen. Wenn Sie 30 Minuten später immer noch keine aktuellen Informationen sehen, führen Sie eine Aktualisierung der Daten mit den folgenden Schritten durch:

1. Klicken Sie unter Server > Azure Migrate: Serverbewertung auf **„Übersicht“** .
2. Klicken Sie unter **„Verwalten“** auf **„Agent-Integritätsdiagnose“** .
3. Klicken Sie auf die Option zum **„Agent aktualisieren“** . Sie werden diese Option unten auf der Liste der Agenten sehen.
4. Warten Sie, bis der Aktualisierungsvorgang abgeschlossen wurde. Vergewissern Sie sich, dass Sie aktuelle Informationen über Ihre VMs sehen.

### <a name="i-dont-the-latest-information-on-the-on-premise-vms-on-the-portal"></a>Ich sehe keine aktuellen Informationen über die lokalen VMs auf dem Portal.

Es kann bis zu 30 Minuten dauern, bis die von der Appliance gesammelten Ermittlungsdaten im Portal erscheinen. Wenn Sie 30 Minuten später immer noch keine aktuellen Informationen sehen, führen Sie eine Aktualisierung der Daten mit den folgenden Schritten durch:

1. Klicken Sie unter Server > Azure Migrate: Serverbewertung auf **„Übersicht“** .
2. Klicken Sie unter **„Verwalten“** auf **„Agent-Integritätsdiagnose“** .
3. Klicken Sie auf die Option zum **„Agent aktualisieren“** . Sie werden diese Option auf der Liste der Agenten sehen.
4. Warten Sie, bis der Aktualisierungsvorgang abgeschlossen wurde. Sie sollten nun aktuelle Informationen zu Ihren VMs sehen.

### <a name="unable-to-connect-to-hosts-or-cluster-as-the-server-name-cannot-be-resolved-winrm-error-code-0x803381b9-error-id-50004"></a>Es ist keine Verbindung zu Host(s) oder dem Cluster möglich, da der Servername nicht aufgelöst werden kann. WinRM-Fehlercode: 0x803381b9 (Fehler-ID: 50004)
Dieser Fehler tritt auf, wenn das DNS, das für die Appliance zuständig ist, den von Ihnen angegebenen Cluster- oder Hostnamen nicht auflösen kann. Wenn Sie diesen Fehler auf dem Cluster sehen, versuchen Sie, den vollqualifizierten Domänennamen des Clusters anzugeben. 

Sie sehen diesen Fehler eventuell auch für Hosts in einem Cluster. In diesem Fall kann sich die Appliance mit dem Cluster verbinden. Der Cluster hat jedoch die Hostnamen zurückgegeben, die keine vollqualifizierten Domänennamen sind. 

Um diesen Fehler zu beheben, aktualisieren Sie die „hosts“-Datei auf der Appliance und fügen Sie ein Mapping von IP-Adresse und Hostnamen hinzu.
1. Öffnen Sie Notepad als Administrator. Öffnen Sie die Datei c:\windows\system32\drivers\etc\hosts.
2. Fügen Sie die IP-Adresse und den Hostnamen in einer Zeile hinzu. Wiederholen Sie dies für jeden Host oder Cluster, bei dem Sie diesen Fehler sehen.
3. Speichern und schließen Sie die Datei „hosts“.
4. Sie können mit der Applianceverwaltungsanwendung überprüfen, ob die Appliance eine Verbindung zu den Hosts herstellen kann. Nach 30 Minuten sollten Sie die neuesten Informationen über diese Hosts auf dem Azure-Portal sehen können. 


## <a name="assessment-issues"></a>Bewertungsprobleme

### <a name="azure-readiness-issues"></a>Azure-Bereitschaftsprobleme

Problem | Wiederherstellung
--- | ---
Nicht unterstützter Starttyp | Azure unterstützt keine virtuellen Computer mit dem Starttyp „EFI“. Wir empfehlen, dass Sie vor einer Migration den Starttyp in „BIOS“ konvertieren. <br/><br/>Die entsprechenden virtuellen Computer können mit Azure Migrate-Servermigration migriert werden, da es den Starttyp des virtuellen Computers im Rahmen der Migration in „BIOS“ konvertiert.
Bedingt unterstütztes Windows-Betriebssystem | Der Unterstützungszeitraum für das Betriebssystem ist abgelaufen. Für die [Unterstützung in Azure](https://aka.ms/WSosstatement) wird eine benutzerdefinierte Supportvereinbarung (Custom Support Agreement, CSA) benötigt. Führen Sie vor der Migration ggf. ein Upgrade des Betriebssystems durch.
Nicht unterstütztes Windows-Betriebssystem | Azure unterstützt nur [bestimmte Windows-Betriebssystemversionen](https://aka.ms/WSosstatement). Führen Sie vor der Migration zu Azure ggf. ein Upgrade des Betriebssystems durch.
Bedingt unterstütztes Linux-Betriebssystem | Azure unterstützt nur [bestimmte Linux-Betriebssystemversionen](../virtual-machines/linux/endorsed-distros.md). Führen Sie vor der Migration zu Azure ggf. ein Upgrade des Betriebssystems durch.
Nicht unterstütztes Linux-Betriebssystem | Der Computer startet zwar unter Umständen in Azure, das Betriebssystem wird von Azure jedoch nicht unterstützt. Führen Sie vor der Migration zu Azure ggf. ein Upgrade auf eine [unterstützte Linux-Version](../virtual-machines/linux/endorsed-distros.md) durch.
Unbekanntes Betriebssystem | Das Betriebssystem des virtuellen Computers wurde in vCenter Server als „Anderes“ angegeben, und Azure Migrate kann deshalb nicht beurteilen, ob der virtuelle Computer für Azure bereit ist. Vergewissern Sie sich, dass das Betriebssystem auf dem Computer von Azure [unterstützt](https://aka.ms/azureoslist) wird, bevor Sie den Computer migrieren.
Nicht unterstützte Bitanzahl für das Betriebssystem | Virtuelle Computer mit 32-Bit-Betriebssystem starten zwar unter Umständen in Azure, es empfiehlt sich jedoch, vor der Migration zu Azure ein Betriebssystemupgrade auf die 64-Bit-Version durchzuführen.
Erfordert Visual Studio-Abonnement. | Auf dem Computer wird ein Windows-Clientbetriebssystem ausgeführt, das nur in einem Visual Studio-Abonnement unterstützt wird.
Kein virtueller Computer für erforderliche Speicherleistung gefunden. | Die erforderliche Speicherleistung (IOPS/Durchsatz) für den Computer überschreitet die von virtuellen Azure-Computern unterstützte Leistung. Reduzieren Sie vor der Migration die Speicheranforderungen für den Computer.
Kein virtueller Computer für erforderliche Netzwerkleistung gefunden. | Die erforderliche Netzwerkleistung (ein-/ausgehend) für den Computer überschreitet die von virtuellen Azure-Computern unterstützte Leistung. Reduzieren Sie die Netzwerkanforderungen für den Computer.
Kein virtueller Computer am angegebenen Speicherort gefunden. | Geben Sie vor der Migration einen anderen Zielort an.
Mindestens ein Datenträger ist ungeeignet. | Mindestens ein mit der VM verbundener Datenträger erfüllt nicht die Azure-Anforderungen. Stellen Sie für jeden mit der VM verbundenen Datenträger sicher, dass die Größe des Datenträgers kleiner als 4 TB ist. Anderenfalls verkleinern Sie die Datenträgergröße, bevor Sie zu Azure migrieren. Stellen Sie sicher, dass die von jedem Datenträger benötigte Leistung (IOPS/Durchsatz) von [Azure-Datenträgern verwalteter virtueller Computer](https://docs.microsoft.com/azure/azure-subscription-service-limits#storage-limits) unterstützt wird.   
Mindestens ein Netzwerkadapter ist ungeeignet. | Entfernen Sie nicht verwendete Netzwerkadapter vor der Migration vom Computer.
Anzahl der Datenträger überschreitet den Grenzwert | Entfernen Sie nicht verwendete Datenträger vor der Migration vom Computer.
Datenträgergröße überschreitet den Grenzwert | Azure unterstützt Datenträger mit einer Größe von bis zu 4 TB. Verkleinern Sie Datenträger vor der Migration auf weniger als 4 TB.
Datenträger am angegebenen Speicherort nicht verfügbar | Stellen Sie vor der Migration sicher, dass sich der Datenträger am Zielspeicherort befindet.
Datenträger für die angegebene Redundanz nicht verfügbar | Der Datenträger muss den in den Bewertungseinstellungen definierten Redundanzspeichertyp verwenden (standardmäßig LRS).
Datenträgereignung konnte aufgrund eines internen Fehlers nicht ermittelt werden | Versuchen Sie, eine neue Bewertung für die Gruppe zu erstellen.
Kein virtueller Computer gefunden, der die Kern- und Arbeitsspeicheranforderungen erfüllt | Azure konnte keinen geeigneten VM-Typ finden. Reduzieren Sie vor der Migration den Arbeitsspeicher und die Anzahl der Kerne auf dem lokalen Computer.
Eignung des virtuellen Computers konnte aufgrund eines internen Fehlers nicht ermittelt werden. | Versuchen Sie, eine neue Bewertung für die Gruppe zu erstellen.
Eignung konnte für mindestens einen Datenträger aufgrund eines internen Fehlers nicht ermittelt werden. | Versuchen Sie, eine neue Bewertung für die Gruppe zu erstellen.
Eignung konnte für mindestens einen Netzwerkadapter aufgrund eines internen Fehlers nicht ermittelt werden. | Versuchen Sie, eine neue Bewertung für die Gruppe zu erstellen.

### <a name="i-am-unable-to-specify-enterprise-agreement-ea-as-an-azure-offer-in-the-assessment-properties"></a>Ich kann kein Enterprise Agreement (EA) als Azure-Angebot in den Bewertungseigenschaften angeben.
Azure Migrate: Die Serverbewertung unterstützt derzeit keine Preise, die auf Enterprise Agreement (EA) basieren. Die Problemumgehung besteht darin, „Nutzungsbasierte Zahlung“ als Azure-Angebot und die Eigenschaft „Rabatt“ zu verwenden, um benutzerdefinierte Rabatte anzugeben, die Sie erhalten. [Erfahren Sie mehr darüber, wie Sie eine Bewertung anpassen können](https://aka.ms/migrate/selfhelp/eapricing).

### <a name="why-does-server-assessment-mark-my-linux-vms-conditionally-ready-is-there-anything-i-need-to-do-to-fix-this"></a>Warum markiert die Serverbewertung meine virtuellen Linux-Computer als „bedingt bereit“? Kann ich dieses Problem beheben?
Es gibt die bekannte Lücke in der Serverbewertung, dass sie nicht in der Lage ist, die auf den lokalen virtuellen Computern installierte Nebenversion des Linux-Betriebssystems zu erkennen (für RHEL 6.10 erkennt die Serverbewertung derzeit nur RHEL 6 als Betriebssystemversion). Da Azure nur bestimmte Versionen von Linux unterstützt, werden die virtuellen Linux-Computer in der Serverbewertung derzeit als „bedingt bereit“ markiert. Sie können manuell prüfen, ob das Linux-Betriebssystem des lokalen virtuellen Computers für Azure geeignet ist. Sehen Sie sich dazu die [Dokumentation zur Linux-Unterstützung von Azure](https://aka.ms/migrate/selfhost/azureendorseddistros) an. Sobald Sie die unterstützte Distribution überprüft haben, können Sie diese Warnung ignorieren.

### <a name="the-vm-sku-recommended-by-server-assessment-has-more-number-of-cores-and-a-larger-memory-size-than-what-is-allocated-on-premises-why-is-that-so"></a>Die von der Serverbewertung empfohlene VM-SKU hat mehr Kerne und einen größeren Arbeitsspeicher, als in der lokalen Bereitstellung zugewiesen sind. Woran liegt das?
Die in der Serverbewertung empfohlene VM-SKU hängt von den Bewertungseigenschaften ab. Sie können zwei Bewertungsarten in der Serverbewertung erstellen: „Leistungsbasiert“ und „Wie lokal“. Bei leistungsbasierten Bewertungen berücksichtigt die Serverbewertung die Nutzungsdaten der lokalen virtuellen Computer (CPU-, Speicher-, Datenträger- und Netzwerkauslastung), um die richtige Ziel-VM-SKU für Ihre lokalen virtuellen Computer zu bestimmen. Zusätzlich wird bei der leistungsbasierten Dimensionierung der Komfortfaktor berücksichtigt, um die effektive Auslastung zu ermitteln. Bei der lokalen Dimensionierung werden Leistungsdaten nicht berücksichtigt, und eine Ziel-SKU wird auf der Grundlage der lokalen Zuordnung empfohlen.

Angenommen, es gibt einen lokalen virtuellen Computer mit 4 Kernen und 8 GB Speicher mit 50 % CPU-Auslastung und 50% Speicherauslastung, und in der Bewertung ist ein Komfortfaktor von 1,3 angegeben. Wenn das Größenkriterium der Bewertung „Wie lokal“ lautet, wird eine Azure-VM-SKU mit 4 Kernen und 8 GB Speicher empfohlen. Wenn das Größenkriterium aber leistungsbasiert ist – basierend auf der effektiven CPU- und Speicherauslastung (50 % von 4 Kernen * 1,3 = 2,6 Kerne und 50 % von 8 GB Speicher * 1,3 = 5,3 GB Speicher) – wäre die günstigste VM-SKU mit 8 Kernen (nächste unterstützte Kernanzahl) und 8 GB Speichergröße (nächste unterstützte Speichergröße) zu empfehlen. [Erfahren Sie über die Größenanpassung durch die Serverbewertung.](https://docs.microsoft.com/azure/migrate/concepts-assessment-calculation#sizing)

### <a name="the-disk-sku-recommended-by-server-assessment-has-a-bigger-size-than-what-is-allocated-on-premises-why-is-that-so"></a>Die von der Serverbewertung empfohlene Datenträger-SKU ist größer als die lokale Zuordnung. Woran liegt das?
Die Größenanpassung in der Serverbewertung hängt von zwei Bewertungseigenschaften ab: dem Größenkriterium und dem Speichertyp. Wenn das Größenkriterium „Leistungsbasiert“ ist und der Speichertyp „Automatisch“ lautet, werden die IOPS- und Durchsatzwerte des Datenträgers berücksichtigt, um den Typ des Zieldatenträgers zu ermitteln (HDD Standard, SSD Standard oder Premium). Eine Datenträger-SKU innerhalb des Datenträgertyps wird dann unter Berücksichtigung der Größenanforderungen des lokalen Datenträgers empfohlen. Wenn das Größenkriterium „Leistungsbasiert“ ist und der Speichertyp „Premium“ lautet, wird eine Premium-Datenträger-SKU in Azure basierend auf den IOPS, dem Durchsatz und den Größenanforderungen des lokalen Datenträgers empfohlen. Die gleiche Logik wird verwendet, um die Größenanpassung für Datenträger durchzuführen, wenn das Größenkriterium „Wie lokal“ verwendet wird und der Speichertyp „HDD Standard“, „SSD Standard“ oder „Premium“ lautet.

Wenn Sie beispielsweise einen lokalen Datenträger mit 32 GB Speicher haben, die aggregierten Lese- und Schreib-IOPS für den Datenträger aber 800 IOPS sind, empfiehlt die Serverbewertung einen Premium-Datenträgertyp (aufgrund der höheren IOPS-Anforderungen) und empfiehlt dann eine Datenträger-SKU, welche die erforderlichen IOPS und Größen unterstützen kann. Die nächstliegende Übereinstimmung in diesem Beispiel wäre P15 (256 GB, 1100 IOPS). Für den lokalen Datenträger ist also nur eine Größe von 32 GB erforderlich, von der Serverbewertung wurde jedoch aufgrund der hohen IOPS-Anforderungen des lokalen Datenträgers ein größerer Datenträger empfohlen.

### <a name="is-the-os-license-cost-of-the-vm-included-in-the-compute-cost-estimated-by-server-assessment"></a>Sind die Kosten der Betriebssystemlizenz des virtuellen Computers in den von der Serverbewertung geschätzten Computekosten enthalten?
Bei der Serverbewertung werden derzeit nur die Lizenzkosten für Betriebssysteme von Windows-Computern berücksichtigt (Betriebssystem-Lizenzkosten für Linux-Computer derzeit nicht). 

### <a name="what-impact-does-performance-history-and-percentile-utilization-have-on-the-size-recommendations"></a>Welche Auswirkungen haben der Leistungsverlauf und der Quantilwert der Nutzung auf die Größenempfehlungen?
Diese Eigenschaften gelten nur für die leistungsbasierte Größenanpassung. Die Serverbewertung erfasst Leistungsdaten von lokalen Computern und nutzt diese Daten, um die VM-SKU und die Datenträger-SKU in Azure zu empfehlen. Im Folgenden wird erläutert, wie Leistungsdaten von der Serverbewertung erfasst werden:
- Die Azure Migrate-Appliance erstellt kontinuierlich Profile der lokalen Umgebung, um Echtzeit-Nutzungsdaten alle 20 Sekunden für virtuelle VMware-Computer und alle 30 Sekunden für virtuelle Hyper-V-Computer zu erfassen.
- Die Appliance führt für die 20/30-Sekunden-Stichproben einen Rollup aus, um für jeweils 10 Minuten einen Datenpunkt zu erstellen. Zum Erstellen jedes Datenpunkts wählt die Appliance den Spitzenwert aus allen 20/30-Sekunden-Stichproben aus und sendet diesen an Azure.
- Beim Erstellen einer Bewertung in der Serverbewertung, die auf der Leistungsdauer und dem Quantilwert des Leistungsverlaufs basiert, wird der repräsentative Nutzungswert ermittelt. Wenn der Leistungsverlauf beispielsweise eine Woche beträgt und die Quantilauslastung 95. ist, sortiert Azure Migrate alle 10-minütigen Stichprobenpunkte für die letzte Woche in aufsteigender Reihenfolge und wählt dann das 95. Perzentil als repräsentativen Wert aus.
Mit dem Wert des 95. Quantils wird sichergestellt, dass Ausreißer ignoriert werden. Diese können enthalten sein, wenn Sie das 99. Quantil wählen. Falls Sie die Spitzenauslastung für den Zeitraum wählen möchten und keine Ausreißer verpassen möchten, sollten Sie das 99. Quantil als Quantilauslastung wählen.

## <a name="dependency-visualization-issues"></a>Probleme bei der Visualisierung von Abhängigkeiten

### <a name="i-am-unable-to-find-the-dependency-visualization-functionality-for-azure-government-projects"></a>Ich kann die Funktion zur Visualisierung von Abhängigkeiten für Azure Government-Projekte nicht finden.

Azure Migrate hängt hinsichtlich der Visualisierungsfunktion von der Dienstzuordnung ab, und da die Dienstzuordnung in Azure Government zurzeit nicht verfügbar ist, ist diese Funktionalität auch nicht in Azure Government verfügbar.

### <a name="i-installed-the-microsoft-monitoring-agent-mma-and-the-dependency-agent-on-my-on-premises-vms-but-the-dependencies-are-now-showing-up-in-the-azure-migrate-portal"></a>Ich habe Microsoft Monitoring Agent (MMA) und den Dependency-Agent auf meinen lokalen virtuellen Computern installiert. Jetzt werden die Abhängigkeiten jedoch im Azure Migrate-Portal angezeigt.

Nachdem Sie die Agents installiert haben, benötigt Azure Migrate üblicherweise 15 bis 30 Minuten, um die Abhängigkeiten im Portal anzuzeigen. Wenn Sie bereits länger als 30 Minuten warten, stellen Sie sicher, dass MMA mit dem OMS-Arbeitsbereich kommunizieren kann. Gehen Sie hierzu wie folgt vor:

Bei einer virtuellen Windows-VM:
1. Wechseln Sie zur **Systemsteuerung**, und starten Sie **Microsoft Monitoring Agent**.
2. Rufen Sie im Popup zu den MMA-Eigenschaften die Registerkarte **Azure Log Analytics (OMS)** auf.
3. Stellen Sie sicher, dass der **Status** für den Arbeitsbereich grün angezeigt wird.
4. Wenn der Status nicht grün angezeigt wird, entfernen Sie den Arbeitsbereich, und fügen Sie ihn zu MMA neu hinzu.
        ![MMA-Status](./media/troubleshooting-general/mma-status.png)

Überprüfen Sie bei einer Linux-VM, ob die Installationsbefehle für MMA und den Dependency-Agent erfolgreich ausgeführt wurden.

### <a name="what-are-the-operating-systems-supported-by-mma"></a>Welche Betriebssysteme werden von MMA unterstützt?

Die Liste der von MMA unterstützten Windows-Betriebssysteme finden Sie [hier](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems).
Die Liste der von MMA unterstützten Linux-Betriebssysteme finden Sie [hier](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems).

### <a name="what-are-the-operating-systems-supported-by-dependency-agent"></a>Welche Betriebssysteme werden vom Dependency-Agent unterstützt?

Die Liste der vom Dependency-Agent unterstützten Windows-Betriebssysteme finden Sie [hier](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-windows-operating-systems).
Die Liste der vom Dependency-Agent unterstützten Linux-Betriebssysteme finden Sie [hier](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-linux-operating-systems).

### <a name="i-am-unable-to-visualize-dependencies-in-azure-migrate-for-more-than-one-hour-duration"></a>Ich kann Abhängigkeiten in Azure Migrate nicht länger als eine Stunde visualisieren.
Azure Migrate ermöglicht Ihnen das Visualisieren von Abhängigkeiten für einen Zeitraum von bis zu einer Stunde. Mit Azure Migrate können Sie zwar um bis zu einen Monat zu einem bestimmten Datum im Verlauf zurückgehen. Die Abhängigkeiten können Sie jedoch nur für einen Zeitraum von maximal einer Stunde visualisieren. So können Sie beispielsweise mithilfe der Zeitraumfunktionalität im Abhängigkeitsdiagramm Abhängigkeiten für gestern, jedoch nur für ein Zeitfenster von einer Stunde anzeigen. Sie können jedoch zum [Abfragen der Abhängigkeitsdaten](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies) über einen längeren Zeitraum Azure Monitor-Protokolle verwenden.

### <a name="i-am-unable-to-visualize-dependencies-for-groups-with-more-than-10-vms"></a>Für Gruppen mit mehr als 10 virtuellen Computern kann ich keine Abhängigkeiten visualisieren.
Sie können [Abhängigkeiten für Gruppen visualisieren](https://docs.microsoft.com/azure/migrate/how-to-create-group-dependencies), die aus bis zu zehn VMs bestehen. Falls eine Gruppe aus mehr als zehn VMs besteht, sollten Sie die Gruppe in kleinere Gruppen aufteilen und die Abhängigkeiten anschließend visualisieren.

### <a name="i-installed-agents-and-used-the-dependency-visualization-to-create-groups-now-post-failover-the-machines-show-install-agent-action-instead-of-view-dependencies"></a>Ich habe Agents installiert und die Visualisierung von Abhängigkeiten verwendet, um Gruppen zu erstellen. Jetzt zeigen die Computer nach dem Failover die Aktion „Agent installieren“ anstelle von „Abhängigkeiten anzeigen“ an.
* Nach einem geplanten oder ungeplanten Failover werden lokale Computer abgeschaltet, und entsprechende Computer in Azure werden hochgefahren. Diese Computer beziehen eine andere MAC-Adresse. Je nachdem, ob der Benutzer die lokale IP-Adresse beibehalten möchte oder nicht, beziehen sie möglicherweise auch eine andere IP-Adresse. Wenn sich sowohl MAC- als auch IP-Adressen unterscheiden, ordnet Azure Migrate die lokalen Computern keinen Dienstzuordnungs-Abhängigkeitsdaten zu und fordert den Benutzer auf, Agents zu installieren, anstatt Abhängigkeiten anzuzeigen.
* Nach dem Testfailover bleiben die lokalen Computer erwartungsgemäß eingeschaltet. Entsprechende Computer, die in Azure hochgefahren wurden, erhalten andere MAC-Adressen und u.U. auch andere IP-Adressen. Sofern der Benutzer ausgehenden Datenverkehr von Azure Monitor-Protokolle von diesen Computern nicht sperrt, ordnet Azure Migrate die lokalen Computer keinen Dienstzuordnungs-Abhängigkeitsdaten zu und fordert den Benutzer auf, Agents zu installieren, anstatt Abhängigkeiten anzuzeigen.

## <a name="collect-azure-portal-logs"></a>Sammeln von Azure-Portalprotokollen

**Wie erfasse ich im Azure-Portal Protokolle des Netzwerkdatenverkehrs?**

1. Öffnen Sie den Browser, navigieren Sie zum [Portal](https://portal.azure.com), und melden Sie sich an.
2. Drücken Sie F12, um die Entwicklertools zu starten. Deaktivieren Sie ggf. die Einstellung **Einträge beim Navigieren löschen**.
3. Klicken Sie auf die Registerkarte **Netzwerk**, und starten Sie die Erfassung von Netzwerkdatenverkehr:
   - Wählen Sie in Chrome die Option **Protokoll speichern**. Die Aufzeichnung sollte automatisch gestartet werden. Ein roter Kreis gibt an, dass der Datenverkehr erfasst wird. Wenn er nicht angezeigt wird, klicken Sie zum Starten auf den schwarzen Kreis.
   - In Microsoft Edge/IE sollte die Aufzeichnung automatisch gestartet werden. Ist dies nicht der Fall, klicken Sie auf die grüne Wiedergabeschaltfläche.
4. Versuchen Sie, den Fehler zu reproduzieren.
5. Nachdem der Fehler während der Aufzeichnung aufgetreten ist, beenden Sie die Aufzeichnung, und speichern Sie eine Kopie der aufgezeichneten Aktivität:
   - Klicken Sie in Chrome mit der rechten Maustaste, und klicken Sie dann auf **Als HAR mit Inhalt speichern**. Daraufhin werden die Protokolle komprimiert und als HAR-Datei exportiert.
   - Klicken Sie in Microsoft Edge/IE auf das Symbol **Aufgezeichneten Datenverkehr exportieren**. Daraufhin werden die Protokolle komprimiert und exportiert.
6. Navigieren Sie zur Registerkarte **Konsole**, um sie auf Warnungen oder Fehler zu prüfen. So speichern Sie das Konsolenprotokoll:
   - Klicken Sie in Chrome mit der rechten Maustaste auf eine beliebige Stelle im Konsolenprotokoll. Wählen Sie **Speichern als**, um das Protokoll zu exportieren und zu komprimieren.
   - Klicken Sie in Microsoft Edge/IE mit der rechten Maustaste auf die Fehler, und wählen Sie **Alle kopieren** aus.
7. Schließen Sie die Entwicklertools.
