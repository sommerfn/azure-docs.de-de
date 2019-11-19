---
title: Problembehandlung
description: Erfahren Sie, wie Sie Probleme bei Azure FarmBeats beheben können.
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: ab0d3aac6944e331223d91eb98b50b3308be12c3
ms.sourcegitcommit: 018e3b40e212915ed7a77258ac2a8e3a660aaef8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73797215"
---
# <a name="troubleshooting"></a>Problembehandlung

In den folgenden Abschnitten werden häufige Probleme bei Azure FarmBeats und deren Behebung beschrieben.

Wenn Sie weitere Unterstützung benötigen, senden Sie eine E-Mail an farmbeatssupport@microsoft.com, und fügen Sie die Datei „deployer.log“ an.

 Führen Sie die folgenden Schritte aus, um die Datei „deployer.log“ herunterzuladen:

1. Klicken Sie auf das markierte Symbol, und wählen Sie aus der Dropdownliste die Option **Download** aus.

    ![FarmBeats-Projekt](./media/troubleshooting-farmbeats/download-deployer-log-1.png)

2. Geben Sie im nächsten Bildschirm den Pfad zu Ihrer deployer.log-Datei ein. Beispiel: farmbeats-deployer.log.

## <a name="sensor-telemetry"></a>Sensortelemetrie

### <a name="telemetry-not-seen"></a>Es werden keine Telemetriedaten gesendet

**Symptom**: Geräte/Sensoren sind bereitgestellt, und Sie haben FarmBeats mit Ihrem Gerätepartner verknüpft, können aber keine Telemetriedaten aus FarmBeats abrufen oder anzeigen.

**Korrekturmaßnahme**: Besuchen Sie das Azure-Portal, und führen Sie die folgenden Schritte aus:

1. Wechseln Sie zur Ressourcengruppe für Ihren FarmBeats-Datenhub.   
2. Wählen Sie den **Event Hub** (DatafeedEventHubNamespace...) aus, und überprüfen Sie die Anzahl von eingehenden Nachrichten.   
3. Wenn **KEINE eingehenden Nachrichten** vorhanden sind, wenden Sie sich an Ihren Gerätepartner.  
4. Wenn **Eingehende Nachrichten** vorhanden sind, wenden Sie sich an farmbeatssupport@microsoft.com, und stellen Sie Informationen zum Datenhub und der erfassten Telemetrie sowie Accelerator-Protokolle bereit.

Informationen zum Herunterladen von Protokollen finden Sie im [Abschnitt zu Protokollen](#collect-logs-manually) des vorliegenden Dokuments.    

### <a name="device-appears-offline"></a>Das Gerät wird als offline angezeigt

**Symptome:** Geräte sind installiert, und Sie haben FarmBeats mit Ihrem Gerätepartner verknüpft. Die Geräte sind online und senden Telemetriedaten, werden aber als offline angezeigt.

**Korrekturmaßnahme**: Für das Gerät ist kein Berichtsintervall konfiguriert. Wenden Sie sich an den Hersteller Ihres Geräts, um das Berichtsintervall festzulegen. 

### <a name="error-deleting-a-resource"></a>Fehler beim Löschen einer Ressource

Folgende Fehlerszenarien treten beim Löschen eines Geräts häufig auf:  

**Meldung**: Auf das Gerät wird in Sensoren verwiesen: Dem Gerät ist mindestens ein Sensor zugeordnet. Löschen Sie den Sensor bzw. die Sensoren und dann das Gerät.  

**Bedeutung**: Dem Gerät sind mehrere im landwirtschaftlichen Betrieb bereitgestellte Sensoren zugeordnet.   

**Korrekturmaßnahme**:  

1. Löschen Sie die Sensoren, die dem Gerät über den Accelerator zugeordnet sind.  
2. Falls Sie die Sensoren einem anderen Gerät zuordnen möchten, bitten Sie Ihren Gerätepartner, dasselbe zu tun.  
3. Löschen Sie das Gerät mithilfe eines DELETE-API-Aufrufs, in dem der force-Parameter auf TRUE festgelegt ist.  

**Meldung**: Auf das Gerät wird in Geräten als ParentDeviceId verwiesen: Diesem Gerät sind ein oder mehrere Geräte als untergeordnete Geräte zugeordnet. Löschen Sie diese Geräte, und löschen Sie dann das Gerät.  

**Bedeutung**: Ihrem Gerät sind andere Geräte zugeordnet.  

**Korrekturmaßnahme**:

1. Löschen Sie die Geräte, die diesem Gerät zugeordnet sind.  
2. Löschen Sie das Gerät.  

    > [!NOTE]
    > Sie können ein Gerät nicht löschen, wenn diesem Sensoren zugeordnet sind. Im Artikel „Abrufen von Sensordaten“ im Abschnitt [Löschen von Sensoren](get-sensor-data-from-sensor-partner.md) finden Sie weitere Informationen zum Löschen von zugeordneten Sensoren.


## <a name="issues-with-jobs"></a>Probleme mit Aufträgen

### <a name="farmbeats-internal-error"></a>Interner FarmBeats-Fehler

**Meldung**: Interner FarmBeats-Fehler. Weitere Informationen finden Sie im Leitfaden zur Problembehandlung.

**Korrekturmaßnahme**: Dies könnte durch einen temporären Ausfall der Datenpipeline verursacht werden. Erstellen Sie den Auftrag erneut. Wenn der Fehler weiterhin besteht, stellen Sie eine entsprechende Frage im FarmBeats-Forum, oder wenden Sie sich an FarmBeatsSupport@microsoft.com.

## <a name="accelerator-troubleshooting"></a>Problembehandlung bei Accelerators

### <a name="access-control"></a>Zugriffssteuerung

**Fehler beim Hinzufügen einer Rollenzuweisung**

**Meldung**: Es wurden keine übereinstimmenden Benutzer gefunden.

**Korrekturmaßnahme**: Überprüfen Sie die E-Mail-ID, für Sie eine Rollenzuweisung durchführen möchten. Die E-Mail-ID muss genau mit der ID übereinstimmen, die für diesen Benutzer in Active Directory registriert ist. Wenn der Fehler weiterhin besteht, stellen Sie eine entsprechende Frage im FarmBeats-Forum, oder wenden Sie sich an FarmBeatsSupport@microsoft.com.

### <a name="unable-to-log-in-to-accelerator"></a>Anmeldung beim Accelerator nicht möglich

**Meldung**: Fehler Sie sind nicht zum Aufrufen des Diensts berechtigt. Wenden Sie sich an den Administrator, um die entsprechende Autorisierung zu erhalten.

**Korrekturmaßnahme**: Bitten Sie den Administrator, Sie für den Zugriff auf die FarmBeats-Bereitstellung zu autorisieren. Dies kann mit einer POST-Anweisung der RoleAssignment-APIs oder über die Zugriffssteuerung im Einstellungsbereich im Accelerator erfolgen.  

Wenn Sie bereits autorisiert sind und dieser Fehler auftritt, versuchen Sie es erneut, indem Sie die Seite aktualisieren.  Wenn der Fehler weiterhin besteht, stellen Sie eine entsprechende Frage im FarmBeats-Forum, oder wenden Sie sich an FarmBeatsSupport@microsoft.com.

![FarmBeats-Projekt](./media/troubleshooting-farmbeats/accelerator-troubleshooting-1.png)

### <a name="accelerator---an-unknown-error-occurred"></a>Accelerator: Unbekannter Fehler  

**Meldung**: Fehler Unbekannter Fehler.

**Korrekturmaßnahme**: Dieser Fehler tritt auf, wenn die Seite zu lange im Leerlauf belassen wurde. Aktualisieren Sie die Seite.  

Wenn der Fehler weiterhin besteht, stellen Sie eine entsprechende Frage im FarmBeats-Forum, oder wenden Sie sich an FarmBeatsSupport@microsoft.com.

**Der FarmBeats-Accelerator zeigt auch nach einem Upgrade von FarmBeatsDeployment nicht die neueste Version an**

**Korrekturmaßnahme**: Dieser Fehler tritt auf, weil ein Dienstworker im Browser persistent gespeichert wurde.
Schließen Sie alle Registerkarten, in denen der Accelerator geöffnet ist, und schließen Sie das Browserfenster. Starten Sie eine neue Instanz des Browsers, und laden Sie die Accelerator-URI erneut. Damit wird die neue Accelerator-Version geladen.

## <a name="sentinel-imagery-related-issues"></a>Probleme im Zusammenhang mit Sentinel-Bildern

### <a name="sentinel-wrong-username-or-password"></a>Falscher Benutzername oder falsches Kennwort für Sentinel

**Auftragsfehlermeldung**: Für den Zugriff auf diese Ressource ist eine vollständige Authentifizierung erforderlich.

**Korrekturmaßnahme**: Führen Sie das Installationsprogramm erneut aus, um ein Upgrade des Datenhub mit dem richtigen Benutzernamen und Kennwort durchzuführen.

**Korrekturmaßnahme**: Führen Sie den fehlerhaften Auftrag erneut aus, oder führen Sie einen Satellitenauftrag zur Indizierung für einen Datumsbereich von 5–7 Tagen aus, und überprüfen Sie, ob der Auftrag erfolgreich ausgeführt wird.

### <a name="sentinel-hub-wrongurlor-not-accessible"></a>Sentinel-Hub-URL falsch oder kein Zugriff möglich 

**Auftragsfehlermeldung**: Leider ist ein Problem aufgetreten. Die Seite, auf die Sie versucht haben zuzugreifen, ist (vorübergehend) nicht verfügbar. 

**Korrekturmaßnahme**:
1.  Öffnen Sie die Sentinel-URL (https://scihub.copernicus.eu/dhus/) in einem Browser, und überprüfen Sie, ob auf die Website zugegriffen werden kann. 
2.  Wenn kein Zugriff auf die Website möglich ist, überprüfen Sie, ob eine Einstellung der Firewall, des Unternehmensnetzwerks usw. die Website blockiert, und ergreifen Sie entsprechende Maßnahmen, um die oben genannte URL zuzulassen. 
3.  Führen Sie den fehlerhaften Auftrag erneut aus, oder führen Sie einen Satellitenauftrag zur Indizierung für einen Datumsbereich von 5–7 Tagen aus, und überprüfen Sie, ob der Auftrag erfolgreich ausgeführt wird.  

### <a name="sentinel-server-down-for-maintenance"></a>Sentinel-Server zu Wartungszwecken heruntergefahren

**Auftragsfehlermeldung**: Der Copernicus Open Access-Hub ist bald wieder verfügbar! Wir führen derzeit Wartungsarbeiten durch. Der Server ist in Kürze wieder online. 

**Korrekturmaßnahme**:

1.  Dieses Problem kann auftreten, wenn auf dem Sentinel-Server Wartungsaktivitäten ausgeführt werden. 
2.  Wenn bei einem Auftrag oder einer Pipeline aus dem oben genannten Grund ein Fehler auftritt, übermitteln Sie den Auftrag nach einiger Zeit erneut. 
3.  Benutzer können unter https://scihub.copernicus.eu/news/ Informationen zu geplanten oder ungeplanten Wartungsaktivitäten von Sentinel erhalten.  
4.  Führen Sie den fehlerhaften Auftrag erneut aus, oder führen Sie einen Satellitenauftrag zur Indizierung für einen Datumsbereich von 5–7 Tagen aus, und überprüfen Sie, ob der Auftrag erfolgreich ausgeführt wird.

### <a name="sentinel-maximum-number-of-connections-reached"></a>Maximale Anzahl von Sentinel-Verbindungen erreicht

**Auftragsfehlermeldung**: Die maximale Anzahl von zwei gleichzeitigen Flows wurde durch den Benutzer <username> erreicht. 

**Korrekturmaßnahme**:
1.  Wenn bei einem Auftrag aus dem oben genannten Grund ein Fehler auftritt, wird dasselbe Sentinel-Konto bereits in einer anderen Bereitstellung oder Software verwendet. 
2.  Der Benutzer kann ein neues Sentinel-Konto erstellen und das Installationsprogramm erneut ausführen, um den Datenhub mit einem neuen Benutzernamen und Kennwort für Sentinel zu aktualisieren.  
3.  Führen Sie den fehlerhaften Auftrag erneut aus, oder führen Sie einen Satellitenauftrag zur Indizierung für einen Datumsbereich von 5–7 Tagen aus, und überprüfen Sie, ob der Auftrag erfolgreich ausgeführt wird.

### <a name="sentinel-server-refused-connection"></a>Der Sentinel-Server hat die Verbindung verweigert 

**Auftragsfehlermeldung**:

Der Server hat die Verbindung unter http://172.30.175.69:8983/solr/dhus verweigert. 

**Korrekturmaßnahme**: Dieses Problem kann auftreten, wenn auf dem Sentinel-Server Wartungsaktivitäten ausgeführt werden. 
1.  Wenn bei einem Auftrag oder einer Pipeline aus dem oben genannten Grund ein Fehler auftritt, übermitteln Sie den Auftrag nach einiger Zeit erneut. 
2.  Benutzer können unter https://scihub.copernicus.eu/news/ Informationen zu geplanten oder ungeplanten Wartungsaktivitäten von Sentinel erhalten.  
3.  Führen Sie den fehlerhaften Auftrag erneut aus, oder führen Sie einen Satellitenauftrag zur Indizierung für einen Datumsbereich von 5–7 Tagen aus, und überprüfen Sie, ob der Auftrag erfolgreich ausgeführt wird.


## <a name="collect-logs-manually"></a>Manuelles Erfassen von Protokollen

[Installieren und implementieren]( https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows) Sie Azure Storage-Explorer.

### <a name="how-to-collect-data-hub-adf-job-logs"></a>Erfassen von Azure Data Factory-Auftragsprotokollen für Datenhubs
1. Melden Sie sich bei https://portal.azure.com an.
2. Suchen Sie im Textfeld **Suche** nach der Ressourcengruppe für den FarmBeats-Datenhub.

    > [!NOTE]
    > Wählen Sie die Datenhub-Ressourcengruppe aus, die bei der Bereitstellung von FarmBeats angegeben wurde.

Suchen Sie auf dem Dashboard der Ressourcengruppe nach dem Speicherkonto, das mit „datahublogs…“ beginnt. Beispiel: datahublogsmvxmq.  

1.  Wählen Sie das Speicherkonto in der Spalte **Name** aus, um das Dashboard **Speicherkonto** anzuzeigen.
2.  Wählen Sie auf der Seite für „datahublogs…“ die Option **In Explorer öffnen** aus, um die Anwendung **Azure Storage-Explorer öffnen** anzuzeigen.
3.  Erweitern Sie im linken Panel („datahublogs...“) den Eintrag **Blobcontainer**, und wählen Sie **job-logs** aus.
4.  Wählen Sie auf der Registerkarte **job-logs** die Option **Download** aus.
5.  Wählen Sie als Speicherort einen lokalen Ordner auf Ihrem Computer aus, in den die Protokolle heruntergeladen werden sollen.
6.  Senden Sie die heruntergeladene ZIP-Datei per E-Mail an farmbeatssupport@microsoft.com.

    ![FarmBeats-Projekt](./media/troubleshooting-farmbeats/collecting-logs-manually-1.png)

### <a name="how-to-collect-accelerator-adf-job-logs"></a>Erfassen von Azure Data Factory-Auftragsprotokollen für Accelerators

1.  Melden Sie sich bei https://portal.azure.com an.
2.  Suchen Sie im Textfeld **Suche** nach der Ressourcengruppe für den FarmBeats-Accelerator.

    > [!NOTE]
    > Wählen Sie die Accelerator-Ressourcengruppe aus, die bei der Bereitstellung von FarmBeats angegeben wurde.

3.  Suchen Sie auf dem Dashboard für die Ressourcengruppe nach einem Speicherkonto, das mit „storage...“ beginnt. Beispiel: storagedop4k.
4.  Wählen Sie das Speicherkonto in der Spalte **Name** aus, um das Dashboard für das Speicherkonto anzuzeigen.
5.  Wählen Sie auf der Seite für „storage…“ die Option **In Explorer öffnen** aus, um die Anwendung „Azure Storage-Explorer öffnen“ anzuzeigen.
6.  Erweitern Sie im linken Panel („storage...“) den Eintrag **Blobcontainer**, und wählen Sie **job-logs** aus.
7.  Wählen Sie auf der Registerkarte **job-logs** die Option **Download** aus.
8.  Wählen Sie als Speicherort einen lokalen Ordner auf Ihrem Computer aus, in den die Protokolle heruntergeladen werden sollen.
9.  Senden Sie die heruntergeladene ZIP-Datei per E-Mail an farmbeatssupport@microsoft.com.


### <a name="how-to-collect-data-hub-app-service-logs"></a>Erfassen von App-Dienstprotokollen für Datenhubs

1.  Melden Sie sich bei https://portal.azure.com an.
2.  Suchen Sie im Textfeld **Suche** nach der Ressourcengruppe für den FarmBeats-Datenhub.

    > [!NOTE]
    > Wählen Sie die Datenhub-Ressourcengruppe aus, die bei der Bereitstellung von FarmBeats angegeben wurde.

3.  Suchen Sie in der Ressourcengruppe nach dem Speicherkonto, das mit „datahublogs…“ beginnt. Beispiel: datahublogsmvxmq.
4.  Wählen Sie das Speicherkonto in der Spalte **Name** aus, um das Dashboard **Speicherkonto** anzuzeigen.
5.  Wählen Sie auf der Seite für „datahublogs…“ die Option **In Explorer öffnen** aus, um die Anwendung **Azure Storage-Explorer öffnen** anzuzeigen.
6.  Erweitern Sie im linken Panel („datahublogs...“) den Eintrag **Blobcontainer**, und wählen Sie „appinsights-logs“ aus.
7.  Wählen Sie auf der Registerkarte „appinsights-logs“ die Option **Download** aus.
8.  Wählen Sie als Pfad einen lokalen Ordner auf Ihrem Computer aus, in den die Protokolle heruntergeladen werden sollen.
9.  Senden Sie den heruntergeladenen Ordner per E-Mail an farmbeatssupport@microsoft.com.

### <a name="how-to-collect-accelerator-app-service-logs"></a>Erfassen von App-Dienstprotokollen für Accelerators

1.  Melden Sie sich bei https://portal.azure.com an.
2.  Suchen Sie im Textfeld **Suche** nach der Ressourcengruppe für den FarmBeats-Accelerator.

    > [!NOTE]
    > Wählen Sie die FarmBeats-Accelerator-Ressourcengruppe aus, die bei der Bereitstellung von FarmBeats angegeben wurde.

3.  Suchen Sie in der Ressourcengruppe nach einem Speicherkonto, das mit „storage…“ beginnt. Beispiel: storagedop4k.
4.  Wählen Sie das Speicherkonto in der Spalte **Name** aus, um das Dashboard **Speicherkonto** anzuzeigen.
5.  Wählen Sie auf der Seite für „storage…“ die Option **In Explorer öffnen** aus, um die Anwendung **Azure Storage-Explorer öffnen** anzuzeigen.
6.  Erweitern Sie im linken Panel („storage...“) den Eintrag **Blobcontainer**, und wählen Sie „appinsights-logs“ aus.
7.  Wählen Sie auf der Registerkarte „appinsights-logs“ die Option **Download** aus.
8.  Wählen Sie als Speicherort einen lokalen Ordner auf Ihrem Computer aus, in den die Protokolle heruntergeladen werden sollen.
9.  Senden Sie den heruntergeladenen Ordner per E-Mail an farmbeatssupport@microsoft.com.

## <a name="known-issues"></a>Bekannte Probleme

## <a name="batch-related-issues"></a>Probleme im Zusammenhang mit Batch

**Fehler**: Das regionale Kontokontingent für Batch-Konten wurde für das angegebene Abonnement erreicht.

**Korrekturmaßnahme**: Erhöhen Sie das Kontingent, oder löschen Sie nicht verwendete Batch-Konten, und führen Sie die Bereitstellung erneut aus.

### <a name="azure-active-directory-related-issues"></a>Probleme im Zusammenhang mit Azure Active Directory

**Fehler**: Die erforderlichen Einstellungen für die Azure AD-App „d41axx40-xx21-4fbd-8xxf-97xxx9e2xxc0“ konnten nicht aktualisiert werden: Nicht genügend Berechtigungen zum Abschließen des Vorgangs. Stellen Sie sicher, dass die oben genannten Einstellungen ordnungsgemäß für die Azure AD-App konfiguriert wurden.

**Bedeutung**: Die Azure AD-App-Registrierung wurde nicht ordnungsgemäß konfiguriert.  

**Korrekturmaßnahme**: Bitten Sie den IT-Administrator (mit Lesezugriff auf den Mandanten), unser [Skript](https://aka.ms/PPCreateAADappregistration) zu verwenden, um die Azure AD-App-Registrierung zu generieren. Dieses Skript führt die Konfigurationsschritte automatisch aus. as  

**Fehler**: Die neue Active Directory-Anwendung „dummyname“ konnte in diesem Mandanten nicht erstellt werden: Es ist bereits ein anderes Objekt mit dem gleichen Wert für die Eigenschaftenbezeichner-URIs vorhanden.

**Bedeutung**: Es ist bereits eine Azure AD-App-Registrierung mit demselben Namen vorhanden.

**Korrekturmaßnahme**: Löschen Sie die vorhandene Azure AD-App-Registrierung, oder verwenden Sie sie erneut für die Installation. Wenn Sie die vorhandene Azure AD-App-Registrierung erneut verwenden, übergeben Sie die Anwendungs-ID und den geheimen Clientschlüssel an das Installationsprogramm, und führen Sie die Bereitstellung erneut aus.

## <a name="inputjson-related-issues"></a>Probleme im Zusammenhang mit der JSON-Eingabedatei

**Fehler** beim Lesen der Eingabe aus der JSON-Eingabedatei.

**Korrekturmaßnahme**: Dieses Problem tritt meistens deswegen auf, weil für das Installationsprogramm nicht der richtige Pfad oder Name der JSON-Eingabedatei angegeben wurde. Nehmen Sie die entsprechenden Änderungen vor, und versuchen Sie die Bereitstellung erneut.

**Fehler** beim Analysieren der JSON-Eingabedatei.

**Korrekturmaßnahme**: Dieser Fehler tritt meistens aufgrund falscher Werte in der JSON-Eingabedatei auf. Nehmen Sie die entsprechenden Änderungen vor, und versuchen Sie die Bereitstellung erneut.

## <a name="high-cpu-usage"></a>Hohe CPU-Auslastung

**Fehler**: Sie erhalten eine E-Mail-Benachrichtigung mit einer Warnung zu einer hohen CPU-Auslastung. 

**Korrekturmaßnahme**: 
1.  Wechseln Sie zur Ressourcengruppe für Ihren FarmBeats-Datenhub.
2.  Wählen Sie den App-Dienst aus.  
3.  Wechseln Sie zu „Zentral hochskalieren (App Service-Plan)“, und wählen Sie einen geeigneten [Tarif](https://azure.microsoft.com/pricing/details/app-service/windows/) aus.

## <a name="next-steps"></a>Nächste Schritte

Wenn weiterhin Probleme auftreten, kontaktieren Sie uns über unser [Supportforum](https://social.msdn.microsoft.com/Forums/home?forum=ProjectFarmBeats).
