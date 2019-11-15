---
title: Bereitstellen von Azure FarmBeats
description: Beschreibt, wie FarmBeats bereitgestellt wird.
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: c609285b727414b4849c9ef6654406a035005bb1
ms.sourcegitcommit: 018e3b40e212915ed7a77258ac2a8e3a660aaef8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73797243"
---
# <a name="deploy-farmbeats"></a>Bereitstellen von FarmBeats

In diesem Artikel wird beschrieben, wie Sie Azure FarmBeats einrichten.

Azure FarmBeats ist eine branchenspezifische, erweiterbare Lösung für die datengesteuerte Landwirtschaft. Sie ermöglicht die nahtlose Bereitstellung und Verbindung von Sensorgeräten mit der Azure-Cloud und unterstützt die Sammlung und Aggregation von Telemetriedaten. Azure FarmBeats verfügt über verschiedene Sensoren, wie Kameras, Drohnen und Bodensensoren, und unterstützt die Verwaltung von Geräten über die Cloud – von der Azure-Infrastruktur und Azure-Diensten für IoT-fähige Geräte (Internet der Dinge) bis hin zu einer erweiterbaren Web-App und mobilen App, die Visualisierungen, Benachrichtigungen und Erkenntnisse liefern.

> [!NOTE]
> Azure FarmBeats wird nur in öffentlichen Cloudumgebungen unterstützt. Weitere Informationen zur Cloudumgebung finden Sie unter [Azure](https://azure.microsoft.com/overview/what-is-a-public-cloud/).

Azure FarmBeats verfügt über die folgenden beiden Komponenten:

- **Datenhub**: Der Datenhub stellt die Plattformebene von Azure FarmBeats dar. Hier können Sie Daten erstellen, speichern und verarbeiten und Erkenntnisse aus vorhandenen oder neuen Datenpipelines ziehen. Diese Plattformebene eignet sich zum Ausführen und Erstellen von Pipelines und Modellen für landwirtschaftliche Daten.

- **Accelerator**: Der Accelerator stellt die Lösungsebene von Azure FarmBeats dar. Er verfügt über eine integrierte Anwendung, um die Fähigkeiten von Azure FarmBeats anhand der vorgefertigten Landwirtschaftsmodelle zu veranschaulichen. Mithilfe dieser Lösungsebene können Sie innerhalb des landwirtschaftlichen Betriebs Grenzen festlegen und aus den landwirtschaftlichen Daten im Kontext dieser Grenzen Erkenntnisse gewinnen.

Eine schnelle Bereitstellung von Azure FarmBeats sollte weniger als eine Stunde dauern. Die Kosten für Datenhub und Accelerator variieren je nach Nutzung.

## <a name="deployed-resources"></a>Bereitgestellte Ressourcen

Bei der Bereitstellung von Azure FarmBeats werden die unten aufgeführten Ressourcen in Ihrem Abonnement erstellt:

|Nr.  |Ressourcenname  |Azure FarmBeats-Komponente  |
|---------|---------|---------|
|1  |       Azure Cosmos DB   |  Datenhub       |
|2  |    Application Insights      |     Datenhub/Accelerator     |
|3  |Azure Cache for Redis   |Datenhub   |
|4  |       Azure Key Vault    |  Datenhub/Accelerator        |
|5  |    Time Series Insights       |     Datenhub      |
|6 |      EventHub-Namespace    |  Datenhub       |
|7  |    Azure Data Factory V2       |     Datenhub/Accelerator      |
|8  |Batch-Konto    |Datenhub   |
|9  |       Speicherkonto     |  Datenhub/Accelerator        |
|10  |    Logik-App        |     Datenhub      |
|11  |    API-Verbindung        |     Datenhub      |
|12|      App Service      |  Datenhub/Accelerator       |
|13 |    App Services-Plan        |     Datenhub/Accelerator      |
|14 |Azure Maps-Konto     |Accelerator    |
|15 |       Time Series Insights      |  Datenhub     |

Azure FarmBeats kann vom Azure Marketplace heruntergeladen werden. Sie können direkt im Azure-Portal darauf zugreifen.  

## <a name="prepare"></a>Vorbereiten

Zum Bereitstellen von Azure FarmBeats benötigen Sie die folgenden Berechtigungen:

- Mandant: Lesezugriff
- Abonnement: Mitwirkender/Besitzer
- Ressourcengruppe: Besitzer

## <a name="before-you-begin"></a>Voraussetzungen

Stellen Sie vor dem Start der Bereitstellung sicher, dass Sie über Folgendes verfügen:

- Sentinel-Konto
- Azure Active Directory (App-Registrierung)
- Azure FarmBeats

## <a name="create-a-sentinel-account"></a>Erstellen eines Sentinel-Kontos    

Ein Sentinel-Konto hilft Ihnen beim Herunterladen der Sentinel-Satellitenbilder von der offiziellen Website auf Ihr Gerät. Führen Sie folgende Schritte durch, um ein kostenloses Konto zu erstellen:

1. Wechseln Sie zur Adresse https://scihub.copernicus.eu/dhus/#/self-registration. Geben Sie auf der Registrierungsseite Vornamen, Nachnamen, Benutzernamen, Kennwort und E-Mail-Adresse ein.
2. Wählen Sie im Dropdownmenü **Select Domain** (Domäne auswählen) die Option **Land** aus.
3. Wählen Sie im Dropdownmenü **Select Usage** (Verwendungszweck auswählen) die Option **Education** (Bildung) aus.
4. Wählen Sie im Dropdownmenü **Select Country** (Land auswählen) Ihr Land aus.
5. Wählen Sie **Register** (Registrieren) aus, um die Registrierung abzuschließen.

Zur Bestätigung wird eine Überprüfungs-E-Mail an die registrierte E-Mail-Adresse gesendet. Wählen Sie den Link aus, und bestätigen Sie die Adresse. Der Registrierungsvorgang ist abgeschlossen.

## <a name="create-azure-ad-app-registration"></a>Erstellen einer Azure AD-App-Registrierung

Zur Authentifizierung und Autorisierung in Azure FarmBeats benötigen Sie eine Azure Active Directory-Anwendungsregistrierung. Dabei gilt Folgendes:

- Fall 1: Diese kann vom Installationsprogramm automatisch erstellt werden (vorausgesetzt, Sie verfügen über die erforderlichen Zugriffsberechtigungen für den Mandanten, das Abonnement und die Ressourcengruppe).
- Fall 2: Sie können diese vor der Bereitstellung von Azure FarmBeats erstellen und konfigurieren (erfordert manuelle Schritte).

**Fall 1**: Das Installationsprogramm geht davon aus, dass Sie berechtigt sind, eine Azure Active Directory-Anwendungsregistrierung innerhalb des gewünschten Abonnements zu erstellen. Um sich zu registrieren, melden Sie sich beim Portal an und wählen **Azure Active Directory** > **App-Registrierung** > **Neue Registrierung** aus.

Wenn Sie bereits über ein Abonnement verfügen, können Sie direkt zum nächsten Verfahren übergehen.

**Fall 2**: Diese Methode wird bevorzugt, wenn Sie keine ausreichenden Berechtigungen zum Erstellen und Konfigurieren einer Azure AD-App-Registrierung innerhalb Ihres Abonnements besitzen. Bitten Sie Ihren Administrator, das [benutzerdefinierte Skript](https://aka.ms/FarmBeatsAADScript) zu verwenden. Dadurch kann der IT-Administrator die Azure AD-App-Registrierung im Azure-Portal automatisch generieren und konfigurieren. Nach der Ausführung dieses benutzerdefinierten Skripts in der PowerShell-Umgebung muss Ihnen der IT-Administrator die resultierende Azure Active Directory-Anwendungsclient-ID und das zugehörige Kennwortgeheimnis mitteilen. Notieren Sie sich diese Werte.

Führen Sie die folgenden Schritte aus, um das Skript für die Azure AD-Anwendungsregistrierung auszuführen:

1. Rufen Sie das [Registrierungsskript](https://aka.ms/FarmBeatsAADScript) ab.
2. Melden Sie sich beim Azure-Portal an, und wählen Sie Ihr Abonnement und den AD-Mandanten aus.
3. Starten Sie Cloud Shell über den oberen Navigationsbereich im Azure-Portal.

    ![FarmBeats-Projekt](./media/prepare-for-deployment/navigation-bar-1.png)


4. Benutzer, die sich erstmalig anmelden, werden aufgefordert, ein Abonnement auszuwählen, um ein Speicherkonto und eine Microsoft Azure Files-Freigabe zu erstellen. Klicken Sie auf **Speicher erstellen**.
5. Erstmalige Benutzer werden aufgefordert, ihre bevorzugte Shell-Oberfläche auszuwählen – Bash oder PowerShell. Wählen Sie „PowerShell“ aus.
6. Geben Sie in der Cloud Shell die folgenden Befehle ein, um das Skript auszuführen.

    ```powershell
    ./create_aad_script.ps1
    ```
7. Notieren Sie sich die Azure AD-Anwendungs-ID und den geheimen Clientschlüssel. Sie müssen diese der Person mitteilen, die Azure FarmBeats bereitstellt.

## <a name="create-azure-farmbeats-offer-on-marketplace"></a>Erstellen eines Azure FarmBeats-Angebots im Marketplace

Gehen Sie folgendermaßen vor, um ein Azure FarmBeats-Angebot im Marketplace zu erstellen:

1. Melden Sie sich beim **Azure-Portal** an, und wählen Sie oben rechts Ihr Konto aus. Wechseln Sie zum Azure AD-Mandanten, unter dem Microsoft Azure FarmBeats bereitgestellt werden soll.
2. Wählen Sie **Marketplace durchsuchen** oder **Ressourcen erstellen** aus. Geben Sie **FarmBeats** ein, um die Angebotsdetails anzuzeigen. Laden Sie auf dieser Seite die über die aka.ms-Links verfügbaren Anleitungen herunter, bevor Sie mit den nächsten Schritten fortfahren.
3. Wählen Sie **Erstellen** aus, und geben Sie die folgenden Informationen ein:

   - Geben Sie den Abonnementnamen ein.
   - Geben Sie einen vorhandenen Ressourcengruppennamen (nur eine leere Ressourcengruppe) ein, oder erstellen Sie eine neue Ressourcengruppe, um Azure FarmBeats bereitzustellen. Notieren Sie sich diese Ressourcengruppe, um Sie später in die Datei „input.json“ einzugeben.
   - Geben Sie die Region ein, in der Azure FarmBeats installiert werden soll. FarmBeats kann derzeit in folgenden Regionen installiert werden: USA, Mitte; Europa, Westen; USA, Osten 2; Europa, Norden; USA, Westen; Asien, Südosten; USA, Osten; Australien, Osten; USA, Westen 2
4. Wählen Sie **OK** aus, wodurch Sie zur Seite mit den Nutzungsbedingungen umgeleitet werden. Lesen Sie die Standardbedingungen für den Marketplace, oder wählen Sie den Link aus, um die Nutzungsbedingungen zu lesen.
5. Wählen Sie **Schließen** aus, aktivieren Sie dann das Kontrollkästchen „Ich stimme zu“, und wählen Sie dann **Erstellen** aus.
6. Sie haben den Endbenutzer-Lizenzvertrag (EULA) für Azure FarmBeats im Marketplace nun erfolgreich unterzeichnet. Um mit der Bereitstellung fortzufahren, führen Sie die nächsten Schritte in diesem Leitfaden aus.

### <a name="prepare-inputjson-file"></a>Vorbereiten der Datei „Input.Json“

Diese Datei ist Ihre Eingabedatei für Azure Cloud Shell. Parameter, deren Werte vor dem Hochladen in dieser Datei festgelegt werden, werden während der Bereitstellung nicht angefordert, sodass Sie Zeit sparen.  

> [!NOTE]
> Durch diese Datei werden Werte in Azure Cloud Shell eingefügt.  Während der Bereitstellung werden keine Parameter angefordert, die Sie in diese Datei eingegeben haben. So sparen Sie Zeit. Sie werden aufgefordert, fehlende Parameter anzugeben.

Überprüfen Sie die Parameter, bevor Sie die Datei vorbereiten.

|Get-Help | BESCHREIBUNG|
|--- | ---|
|„sku“  | Bietet die Möglichkeit, eine oder beide Komponenten von Azure FarmBeats herunterzuladen. Gibt an, welche Komponenten heruntergeladen werden sollen. Verwenden Sie „onlydatabhub“, um nur den Datenhub zu installieren. Verwenden Sie „both“, um den Datenhub und den Accelerator zu installieren.|
|„subscriptionId“  | Gibt das Abonnement für die Installation von FarmBeats an.|
|„datahubResourceGroup“  | Der Name der Ressourcengruppe für Datenhubressourcen|
|„datahubLocation“ | Der Speicherort, an dem die Datenhubressourcen gespeichert werden sollen|
|„acceleratorWebsiteName“  |Ein eindeutiges URL-Präfix zum Benennen des Datenhubs
Swagger-Website Der Standardwert entspricht dem Namen der Datenhub-Ressourcengruppe. Drücken Sie EINGABE, um mit dem Standardwert fortzufahren.|
|„acceleratorResourceGroup“  | Der Name der Ressourcengruppe für Datenhubressourcen |
|„acceleratorLocation“  | Der Speicherort zum Speichern der Datenhubressourcen
„acceleratorWebsiteName“  | Ein eindeutiges URL-Präfix zum Benennen Ihrer Accelerator-Website. Der Standardwert ist „accelerator“. Drücken Sie EINGABE, um mit dem Standardwert fortzufahren.|
|„sentinelUsername“ | Der Benutzername für die Anmeldung: https://scihub.copernicus.eu/dhus/#/self-registration|
|„sentinelPassword“  | Das Kennwort für die Anmeldung: https://scihub.copernicus.eu/dhus/#/self-registration|
|„farmbeatsAppId“  | Werte, die vom Azure FarmBeats-Team gemeinsam genutzt werden  |
|„farmbeatsPassword“  | Werte, die vom Azure FarmBeats-Team gemeinsam genutzt werden|
|„notificationEmailAddress“  | Die E-Mail-Adresse, über die Benachrichtigungen zu Warnungen empfangen werden, die Sie im Datenhub konfigurieren|
|„upda"aadAppClientId" “  |[**Optional**] Ein Parameter, der nur in „Input.Json“ eingefügt werden muss, wenn die Azure AD-App bereits vorhanden ist.  – True/False Bei der Erstinstallation „False“ und bei einem Upgrade „True“|
|„aadAppClientId“  | [**Optional**] Ein Parameter, der nur in „Input.Json“ eingefügt werden muss, wenn die Azure AD-App bereits vorhanden ist.  |
|„aadAppClientSecret“   | [**Optional**] Ein Parameter, der nur in „Input.Json“ eingefügt werden muss, wenn die Azure AD-App bereits vorhanden ist.|


Beispieleingabe für die JSON-Datei:

```json
{  
   "sku":"both", 
   "subscriptionId": "da9xxxec-dxxf-4xxc-xxx21-xxx3ee7xxxxx", 
   "datahubResourceGroup": "dummy-test-dh1", 
   "datahubLocation": "westus2", 
   "datahubWebsiteName": "dummy-test-dh1", 
   "acceleratorResourceGroup": "dummy-test-acc1", 
   "acceleratorLocation": "westus2", 
   "acceleratorWebsiteName": "dummy-test-acc1", 
   "sentinelUsername": "dummy-dev", 
   "farmbeatsAppId": "c3cb3xxx-27xx-4xxb-8xx6-3xxx2xxdxxx5c", 
   "notificationEmailAddress": "dummy@microsoft.com", 
   "updateIfExists": true
}
```
## <a name="deploy-within-cloud-shell-browser-based-command-line"></a>Bereitstellen in der browserbasierten Befehlszeile von Cloud Shell

Im Rahmen des Marketplace-Workflows haben Sie eine Ressourcengruppe erstellt und den Endbenutzer-Lizenzvertrag unterzeichnet, der im Rahmen der eigentlichen Bereitstellung erneut gelesen werden kann. Die Bereitstellung erfolgt über Azure Cloud Shell (browserbasierte Befehlszeile) unter Verwendung der Bash-Umgebung.  

> [!NOTE]
> Inaktive Cloud Shell-Sitzungen laufen nach 20 Minuten ab. Versuchen Sie, die Bereitstellung innerhalb dieses Zeitraums abzuschließen.

1. Melden Sie sich beim **Azure**-Portal an, und wählen Sie das gewünschte Abonnement und den AD-Mandanten aus.
2. Starten Sie **Cloud Shell** über den oberen Navigationsbereich im **Azure**-Portal.

   ![FarmBeats-Projekt](./media/prepare-for-deployment/navigation-bar-1.png)

3. Wählen Sie ein Abonnement aus, in dem ein Speicherkonto und eine Microsoft Azure Files-Freigabe erstellt werden sollen.
4. Klicken Sie auf **Speicher erstellen**.
5. Wählen Sie auf der linken Seite des Shellfensters (Bash) das Dropdownmenü für Umgebungen aus.

   ![FarmBeats-Projekt](./media/prepare-for-deployment/bash-1-1.png)

### <a name="deployment-scenario-1"></a>Bereitstellungsszenario 1

Die Azure AD-App-Registrierung wird vom Installationsprogramm erstellt (Sie verfügen über Leseberechtigungen für den AD-Mandanten).  

1. Laden Sie die Vorlage [input.json](https://aka.ms/PPInputJsonTemplate) herunter. Fügen Sie die Azure-Anwendungsclient-ID und das Kennwort in die Datei „input.json“ ein, und speichern Sie diese.
2. Öffnen Sie die heruntergeladene Datei in einem Editor, und vervollständigen Sie die Datei, indem Sie die Werte eingeben.

    ```json
    {
    "sku":"both",
    "subscriptionId":"daxx9xxx-d18f-4xxc-9c21-5xx3exxxxx45",
    "datahubResourceGroup":"dummy-test-dh1",  
    "location":"westus2",  
    "datahubWebsiteName":"dummy-test-dh1",  
    "acceleratorResourceGroup":"dummy-test-acc1",  
    "acceleratorWebsiteName":"dummy-test-acc1",  
    "sentinelUsername":"dummy-dev",
    "notificationEmailAddress":"dummyuser@org1.com",
    "updateIfExists":true
    }

    ```

    > [!NOTE]
    > Sie können diesen Schritt überspringen. In diesem Fall werden die Eingaben zur Laufzeit angefordert.

3. Speichern Sie die Datei, und notieren Sie sich den Pfad (auf Ihrem lokalen Computer).  
4. Wechseln Sie zu Azure Cloud Shell, und wählen Sie nach erfolgreicher Authentifizierung den Upload aus (siehe das hervorgehobene Symbol in der Abbildung unten). Laden Sie dann die Datei „input.json“ in den Cloud Shell-Speicher hoch. Sie müssen den Azure AD-Parameter nicht in der JSON-Datei angeben, da die Azure AD-App-Registrierung vom Installationsprogramm für Sie erstellt und konfiguriert wird.

   ![FarmBeats-Projekt](./media/prepare-for-deployment/bash-2-1.png)

5. Geben Sie den „Bereitstellungsbefehl“ in die Cloud Shell ein, oder fügen Sie diesen ein. Achten Sie darauf, den Pfad zur Datei „input.json“ zu ändern, und drücken Sie EINGABE.  

    ```
    wget -N -O farmbeats-installer.sh https://aka.ms/FB_1.2.0 && bash farmbeats-installer.sh> /home/dummyuser/input.json

    ```
   Vom Skript werden automatisch alle Abhängigkeiten heruntergeladen und die Bereitstellung erstellt. Anschließend werden Sie aufgefordert, dem Endbenutzer-Lizenzvertrag (EULA) von Azure FarmBeats zuzustimmen.

   - Geben Sie „Y“ ein, wenn Sie zustimmen und mit dem nächsten Schritt fortfahren möchten.
   - Geben Sie „N“ ein, wenn Sie den Bedingungen nicht zustimmen. Dann wird die Bereitstellung beendet.

   Anschließend werden Sie aufgefordert, ein Zugriffstoken für die Bereitstellung einzugeben. Kopieren Sie den generierten Code, und melden Sie sich mit Ihren Azure-Anmeldeinformationen bei https://microsoft.com/devicelogin an.

   > [!NOTE]
   > Der Code läuft nach 60 Minuten ab. Wenn er abläuft, können Sie neu beginnen, indem Sie den Bereitstellungsbefehl erneut eingeben.

6. Geben Sie bei der nächsten Aufforderung das Kennwort für das Sentinel-Konto ein.
7. Nach der Überprüfung wird die Bereitstellung vom Installationsprogramm gestartet. Das kann etwa 20 Minuten dauern.
8. Nachdem die Bereitstellung erfolgreich abgeschlossen wurde, erhalten Sie die folgenden Ausgabelinks:
    - **Datenhub-URL**: Swagger-Link zum Testen von Azure FarmBeats-APIs
    - **Accelerator-URL**: Benutzeroberfläche zum Erkunden des intelligenten Azure FarmBeats-Accelerators für landwirtschaftliche Daten.
    - **Bereitstellungs-Protokolldatei**: Protokolldatei, die während der Bereitstellung erstellt wird und für die Problembehandlung verwendet werden kann.

    - Geben Sie „Y“ ein, wenn Sie zustimmen und mit dem nächsten Schritt fortfahren möchten.
    - Geben Sie „N“ ein, wenn Sie den Bedingungen nicht zustimmen. Dann wird die Bereitstellung beendet.

   Anschließend werden Sie aufgefordert, ein Zugriffstoken für die Bereitstellung einzugeben. Kopieren Sie den generierten Code, und melden Sie sich mit Ihren Azure-Anmeldeinformationen bei https://microsoft.com/devicelogin an.

   > [!NOTE]
    > Notieren Sie diese Angaben, und halten Sie den Pfad zur Bereitstellungs-Protokolldatei griffbereit, falls Sie ihn zukünftig benötigen.


### <a name="deployment-scenario-2"></a>Bereitstellungsszenario 2

Zur Bereitstellung wird die vorhandene Azure Active Directory-App-Registrierung verwendet.

1. Laden Sie [input.json](https://aka.ms/PPInputJsonTemplate) herunter, fügen Sie die Azure-Anwendungsclient-ID und das Kennwort in die Datei „input.json“ ein, und speichern Sie diese.

    ```json
       {
       "sku":"both",
       "subscriptionId":"daxx9xxx-d18f-4xxc-9c21-5xx3exxxxx45",
       "datahubResourceGroup":"dummy-test-dh1",  
       "location":"westus2",  
       "datahubWebsiteName":"dummy-test-dh1",  
       "acceleratorResourceGroup":"dummy-test-acc1",  
       "acceleratorWebsiteName":"dummy-test-acc1",  
       "sentinelUsername":"dummy-dev",
       "notificationEmailAddress":"dummyuser@org1.com",
       "updateIfExists": true,
       "aadAppClientId": "lmtlemlemylmylkmerkywmkm823",
       "aadAppClientSecret": "Kxxxqxxxxu*kxcxxx3Yxxu5xx/db[xxx"
       }
     ```

     Führen Sie die restlichen Schritte aus:

2. Notieren Sie sich den Pfad zur Datei „input.json“ (auf Ihrem lokalen Computer).
3. Wechseln Sie noch einmal zu Azure Cloud Shell, und wählen Sie nach erfolgreicher Authentifizierung die Schaltfläche zum Hochladen aus (siehe das hervorgehobene Symbol in der Abbildung unten). Laden Sie dann die Datei „input.json“ in den Cloud Shell-Speicher hoch.

    ![FarmBeats-Projekt](./media/prepare-for-deployment/bash-2-1.png)

4. Geben Sie den *Bereitstellungsbefehl* in die Cloud Shell ein, oder fügen Sie diesen ein. Achten Sie darauf, den Pfad zur Datei „input.json“ zu ändern, und drücken Sie EINGABE.

    ```
    wget -N -O farmbeats-installer.sh https://aka.ms/FB_1.2.0 && bash farmbeats-installer.sh> /home/dummyuser/input.json

    ```
   Führen Sie die restlichen Schritte aus:

5. Vom Skript werden automatisch alle Abhängigkeiten heruntergeladen und die Bereitstellung erstellt.
6. Sie werden aufgefordert, den Endbenutzer-Lizenzvertrag (EULA) von Azure FarmBeats zu lesen und ihm zuzustimmen.

   - Geben Sie „Y“ ein, wenn Sie zustimmen und mit dem nächsten Schritt fortfahren möchten.
   - Geben Sie „N“ ein, wenn Sie den Bedingungen nicht zustimmen. Dann wird die Bereitstellung beendet.

7. Sie werden aufgefordert, ein Zugriffstoken für die Bereitstellung einzugeben. Kopieren Sie den generierten Code, und melden Sie sich mit Ihren Azure-Anmeldeinformationen bei https://microsoft.com/devicelogin an.
8. Nach der Überprüfung wird die Ressourcenerstellung vom Installationsprogramm nun gestartet. Das kann etwa 20 Minuten dauern. Die Sitzung in der Cloud Shell muss während dieser Zeit aktiv bleiben.
9. Nachdem die Bereitstellung erfolgreich durchgeführt wurde, erhalten Sie die folgenden Ausgabelinks:
   - **Datenhub-URL**: Swagger-Link zum Testen von FarmBeats-APIs
   - **Accelerator-URL**: Benutzeroberfläche zum Erkunden des intelligenten FarmBeats-Accelerators für landwirtschaftliche Daten.
   - **Bereitstellungs-Protokolldatei**: In dieser Datei werden Protokolle während der Bereitstellung gespeichert. Außerdem kann sie für die Problembehandlung verwendet werden.

Falls Probleme auftreten, informieren Sie sich unter [Problembehandlung](troubleshoot-project-farmbeats.md).


## <a name="validate-deployment"></a>Überprüfen der Bereitstellung

### <a name="data-hub"></a>Datenhub

Nachdem die Installation des Datenhubs abgeschlossen ist, erhalten Sie die URL für den Zugriff auf die Azure FarmBeats-APIs über die Swagger-Oberfläche im folgenden Format: https://\<yourdatahub-website-name>.azurewebsites.net

1. Um sich über Swagger anzumelden, kopieren Sie die URL und fügen sie in den Browser ein.
2. Melden Sie sich mit den Anmeldeinformationen für das Azure-Portal an.
3. Integritätstest (optional)

     - Sie können sich mit dem Datenhublink, den Sie nach einer erfolgreichen Bereitstellung als Ausgabe erhalten haben, erfolgreich beim Swagger-Portal anmelden.
     - Get-API für erweiterte Typen – Wählen Sie „Try it out/Execute“ aus.
     - Sie sollten Code 200 als Serverantwort und keine Ausnahme wie „403 – Nicht autorisierter Benutzer“ erhalten.

### <a name="accelerator"></a>Accelerator

Nachdem die Installation des Accelerators abgeschlossen ist, erhalten Sie die URL für den Zugriff auf die FarmBeats-Benutzeroberfläche im folgenden Format: https://\<accelerator-website-name>.azurewebsites.net

1. Um sich vom Accelerator aus anzumelden, kopieren Sie die URL und fügen sie in den Browser ein.
2. Melden Sie sich mit den Anmeldeinformationen für das Azure-Portal an.
3. Führen Sie einen optionalen Integritätstest aus.

    - Überprüfen Sie, ob Sie sich mit dem Accelerator-Link, den Sie nach einer erfolgreichen Bereitstellung als Ausgabe erhalten haben, erfolgreich beim Accelerator-Portal anmelden können.
    - Wählen Sie **Create farm** (Landwirtschaftsbetrieb erstellen) aus.
    - Verwenden Sie die Schaltfläche **Get Started** (Erste Schritte), um die FarmBeats-Leitfäden unter dem Symbol „?“ zu öffnen.

## <a name="upgrade"></a>Upgrade

Die Upgradeschritte sind mit den Schritten für die Erstinstallation vergleichbar. Folgen Sie diesen Schritten:

1. Melden Sie sich beim Azure-Portal an, und wählen Sie das gewünschte Abonnement und den AD-Mandanten aus.
2. Starten Sie Cloud Shell über den oberen Navigationsbereich im Azure-Portal.

   ![FarmBeats-Projekt](./media/prepare-for-deployment/navigation-bar-1.png)

3. Wählen Sie ein Abonnement aus, um ein Speicherkonto und eine Azure Files-Dateifreigabe zu erstellen.
4. Klicken Sie auf **Speicher erstellen**.
5. Wählen Sie aus dem Dropdownmenü auf der linken Seite der Shell (Bash) die Umgebung aus.
6. Nehmen Sie nur unbedingt erforderliche Änderungen an der Datei „input.json“ vor, und laden Sie diese in Azure Cloud Shell hoch. Beispielsweise können Sie Ihre E-Mail-Adresse für die Benachrichtigung aktualisieren, die Sie empfangen möchten.
7. Notieren Sie sich den Pfad zur Datei „input.json“ (auf Ihrem lokalen Computer).
8. Wechseln Sie zu Azure Cloud Shell. Wählen Sie nach der erfolgreichen Authentifizierung die Schaltfläche zum Hochladen aus (siehe das hervorgehobene Symbol in der Abbildung unten). Laden Sie die dann Datei „input.json“ in den Cloud Shell-Speicher hoch.

   ![FarmBeats-Projekt](./media/prepare-for-deployment/bash-2-1.png)

9. Geben Sie den **Bereitstellungsbefehl** in die Cloud Shell ein, oder fügen Sie diesen ein. Achten Sie darauf, den Pfad zur Datei „input.json“ zu ändern, und drücken Sie EINGABE.

    ```
    wget -N -O farmbeats-installer.sh https://aka.ms/FB_1.2.0 && bash farmbeats-installer.sh> /home/dummyuser/input.json

    ```
    Führen Sie die restlichen Schritte aus:

10. Die erforderlichen Eingaben werden zur Laufzeit automatisch vom Installationsprogramm angefordert:

    - Geben Sie ein Zugriffstoken für die Bereitstellung ein. Kopieren Sie den generierten Code, und melden Sie sich mit Ihren Azure-Anmeldeinformationen bei https://microsoft.com/devicelogin an.

     > [!NOTE]
     > Der Code läuft nach 60 Minuten ab. Wenn er abläuft, können Sie neu beginnen, indem Sie den Bereitstellungsbefehl erneut eingeben.

     - Sentinel-Kennwort

11. Nach der Überprüfung wird nun die Ressourcenerstellung vom Installationsprogramm gestartet. Das kann etwa 20 Minuten dauern.
12. Nachdem die Bereitstellung erfolgreich abgeschlossen wurde, erhalten Sie die folgenden Ausgabelinks:

    - **Datenhub-URL**: Swagger-Link zum Testen von FarmBeats-APIs
    - **Accelerator-URL**: Benutzeroberfläche zum Erkunden des intelligenten FarmBeats-Accelerators für landwirtschaftliche Daten.
    - **Bereitstellungs-Protokolldatei**: In dieser Datei werden Protokolle während der Bereitstellung gespeichert. Sie kann für die Problembehandlung verwendet werden.

    > [!NOTE]
    > Notieren Sie die oben aufgeführten Links, und halten Sie den Pfad zur Bereitstellungs-Protokolldatei griffbereit, falls Sie ihn zukünftig benötigen.

## <a name="uninstall"></a>Deinstallieren

Derzeit wird keine automatische Deinstallation von FarmBeats durch das Installationsprogramm unterstützt. Um den Datenhub oder den Accelerator zu entfernen, löschen Sie im Azure-Portal die Ressourcengruppe, in der diese Komponenten installiert sind, oder löschen Sie die Ressourcen manuell.

Wenn Sie den Datenhub und Accelerator beispielsweise in zwei verschiedenen Ressourcengruppen bereitgestellt haben, löschen Sie diese Ressourcengruppen wie folgt:

1. Melden Sie sich beim Azure-Portal an.
2. Wählen Sie oben rechts Ihr Konto aus, und wechseln Sie zum gewünschten Azure AD-Mandanten, in dem Sie Microsoft FarmBeats bereitstellen möchten.

   > [!NOTE]
   > Der Datenhub ist erforderlich, damit der Accelerator ordnungsgemäß funktioniert. Es wird davon abgeraten, den Datenhub ohne den Accelerator zu deinstallieren.

3. Wählen Sie „Ressourcengruppen“ aus, und geben Sie den Namen der Datenhub- oder Accelerator-Ressourcengruppe ein, die Sie löschen möchten.
4. Klicken Sie auf den Namen der Ressourcengruppe. Geben Sie den Namen zur Sicherheit erneut ein, und klicken Sie auf „Löschen“, um die Ressourcengruppe und alle zugrunde liegenden Ressourcen zu entfernen.
5. Alternativ können Sie jede Ressource manuell löschen. Dies wird jedoch nicht empfohlen.
7. Wechseln Sie zum Löschen/Deinstallieren des Datenhubs direkt zur Ressourcengruppe in Azure, und löschen Sie die Ressourcengruppe dort.

## <a name="next-steps"></a>Nächste Schritte

Sie haben Azure FarmBeats bereitgestellt. Erfahren Sie jetzt, wie Sie [landwirtschaftliche Betriebe erstellen](manage-farms.md#create-farms).
