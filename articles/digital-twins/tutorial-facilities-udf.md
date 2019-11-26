---
title: 'Tutorial: Überwachen eines Gebäudebereichs mit Azure Digital Twins'
description: In diesem Tutorial erfahren Sie, wie Sie mit Azure Digital Twins Ihre räumlichen Ressourcen bereitstellen und die Arbeitsbedingungen überwachen.
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.custom: seodec18
ms.service: digital-twins
ms.topic: tutorial
ms.date: 11/13/2019
ms.openlocfilehash: 6c9403f8355d2f842226c9c7257803edd3215829
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74107484"
---
# <a name="tutorial-provision-your-building-and-monitor-working-conditions-with-azure-digital-twins-preview"></a>Tutorial: Bereitstellen des Gebäudes und Überwachen der Arbeitsbedingungen mit Azure Digital Twins (Vorschauversion)

In diesem Tutorial wird veranschaulicht, wie Sie Ihre Gebäudebereiche mit Azure Digital Twins (Vorschauversion) auf die gewünschten Temperaturbedingungen und weitere Komfortwerte überwachen. Nach dem [Konfigurieren des Beispielgebäudes](tutorial-facilities-setup.md) können Sie wie hier beschrieben das Gebäude bereitstellen und benutzerdefinierte Funktionen für Ihre Sensordaten ausführen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Definieren der zu überwachenden Bedingungen
> * Erstellen einer benutzerdefinierten Funktion (User-Defined Function, UDF)
> * Simulieren von Sensordaten
> * Abrufen der Ergebnisse einer benutzerdefinierten Funktion

## <a name="prerequisites"></a>Voraussetzungen

In diesem Tutorial wird vorausgesetzt, dass Sie das [Azure Digital Twins-Setup abgeschlossen](tutorial-facilities-setup.md) haben. Stellen Sie sicher, dass Sie über Folgendes verfügen, bevor Sie fortfahren:

- Ein [Azure-Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Eine aktive Instanz von Azure Digital Twins. 
- Die [C#-Beispiele für Digital Twins](https://github.com/Azure-Samples/digital-twins-samples-csharp) (auf den Arbeitscomputer heruntergeladen und extrahiert). 
- [.NET Core SDK Version 2.1.403 oder höher](https://www.microsoft.com/net/download) auf dem Entwicklungscomputer zum Erstellen und Ausführen des Beispiels. Führen Sie `dotnet --version` aus, um zu überprüfen, ob die richtige Version installiert ist. 
- [Visual Studio Code](https://code.visualstudio.com/) zum Untersuchen des Beispielcodes. 

> [!TIP]
> Verwenden Sie bei der Bereitstellung einer neuen Instanz einen eindeutigen Namen für die Digital Twins-Instanz.

## <a name="define-conditions-to-monitor"></a>Definieren der zu überwachenden Bedingungen

Sie können eine Reihe spezifischer Bedingungen definieren, die Sie in den Geräte- oder Sensordaten überwachen möchten. Diese Bedingungen werden als *Abgleicher* (Matcher) bezeichnet. Anschließend können Sie Funktionen definieren, die als *benutzerdefinierte Funktionen* bezeichnet werden. Diese Funktionen führen benutzerdefinierte Logik für Daten von Ihren Gebäudebereichen und Geräten aus, wenn die mit den Abgleichern festgelegten Bedingungen auftreten. Weitere Informationen finden Sie unter [Datenverarbeitung und benutzerdefinierte Funktionen](concepts-user-defined-functions.md). 

Öffnen Sie die Datei **src\actions\provisionSample.yaml** aus dem Beispielprojekt **occupancy-quickstart** in Visual Studio Code. Beachten Sie den Abschnitt, der mit dem Typ **matchers** beginnt. Jeder Eintrag unter diesem Typ erstellt einen Abgleicher mit dem angegebenen **Namen**. Der Abgleicher überwacht einen Sensor vom Typ **dataTypeValue**. Beachten Sie die Beziehung zum Gebäudebereich namens *Focus Room A1*, der einen Knoten **devices** mit einigen Sensoren enthält. Stellen Sie zum Bereitstellen eines Abgleichers, der einen dieser Sensoren nachverfolgt, sicher, dass dessen **dataTypeValue**-Wert mit dem **dataType**-Wert des Sensors übereinstimmt. 

Fügen Sie den folgenden Abgleicher unterhalb der vorhandenen Abgleicher hinzu. Stellen Sie sicher, dass die Schlüssel korrekt angepasst sind und Leerzeichen nicht durch Tabstopps ersetzt werden. Diese Zeilen sind auch in der Datei *provisionSample.yaml* als auskommentierte Zeilen vorhanden. Sie können ihre Kommentierung aufheben, indem Sie jeweils das Zeichen `#` am Zeilenanfang entfernen.

```yaml
      - name: Matcher Temperature
        dataTypeValue: Temperature
```

Dieser Abgleicher überwacht den Sensor `SAMPLE_SENSOR_TEMPERATURE`, den Sie im [ersten Tutorial](tutorial-facilities-setup.md) hinzugefügt haben. 

## <a name="create-a-user-defined-function"></a>Erstellen einer benutzerdefinierten Funktion

Mithilfe von benutzerdefinierten Funktionen können Sie die Verarbeitung Ihrer Sensordaten anpassen. Bei diesen Funktionen handelt es sich um benutzerdefinierten JavaScript-Code, der in Ihrer Azure Digital Twins-Instanz ausgeführt werden kann, wenn bestimmte von den Abgleichern definierte Bedingungen erfüllt sind. Sie können Abgleicher und benutzerdefinierte Funktionen für jeden Sensor erstellen, den Sie überwachen möchten. Weitere Informationen finden Sie unter [Datenverarbeitung und benutzerdefinierte Funktionen](concepts-user-defined-functions.md). 

Suchen Sie in der Beispieldatei *provisionSample.yaml* nach einem Abschnitt, der mit dem Typ **userdefinedfunctions** beginnt. Dieser Abschnitt stellt eine benutzerdefinierte Funktion mit einem bestimmten **Namen** bereit. Diese UDF wird für die Liste der Abgleicher unter **matcherNames** ausgeführt. Sie können auch Ihre eigene JavaScript-Datei für die benutzerdefinierte Funktion als **Skript** bereitstellen.

Beachten Sie auch den Abschnitt mit dem Namen **roleassignments**. In diesem Abschnitt wird der benutzerdefinierten Funktion die Rolle des Raumadministrators zugewiesen. Diese Rolle ermöglicht den Zugriff auf die Ereignisse von allen bereitgestellten Gebäudebereichen. 

1. Konfigurieren Sie die benutzerdefinierte Funktion zum Einschließen des Temperaturabgleichers, indem Sie im Knoten `matcherNames` der Datei *provisionSample.yaml* die folgende Zeile hinzufügen oder ihre Auskommentierung aufheben:

    ```yaml
            - Matcher Temperature
    ```

1. Öffnen Sie die Datei **src\actions\userDefinedFunctions\availability.js** im Editor. Auf diese Datei wird im **script**-Element der Datei *provisionSample.yaml* verwiesen. Die benutzerdefinierte Funktion in dieser Datei sucht nach zwei Bedingungen: keine Bewegung im Raum und Kohlendioxidwerte unter 1.000 ppm. 

   Ändern Sie die JavaScript-Datei, um die Temperatur und andere Bedingungen zu überwachen. Fügen Sie die folgenden Codezeilen hinzu, um nach folgenden Bedingungen zu suchen: keine Bewegung im Raum, Kohlendioxidwerte unter 1.000 ppm und Temperaturen unter 78 Grad Fahrenheit.

   > [!NOTE]
   > In diesem Abschnitt wird die Datei *src\actions\userDefinedFunctions\availability.js* geändert, damit Sie sich mit einer Möglichkeit zum Schreiben von benutzerdefinierten Funktionen vertraut machen können. Sie können die Datei [src\actions\userDefinedFunctions\availabilityForTutorial.js](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/userDefinedFunctions/availabilityForTutorial.js) jedoch auch direkt in Ihrem Setup verwenden. Diese Datei enthält alle Änderungen, die für dieses Tutorial erforderlich sind. Wenn Sie stattdessen diese Datei verwenden, müssen Sie darauf achten, dass Sie den richtigen Dateinamen für den **script**-Schlüssel in der Datei [src\actions\provisionSample.yaml](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/provisionSample.yaml) verwenden.

    a. Fügen Sie am Anfang der Datei die folgenden Zeilen für die Temperatur unter dem Kommentar `// Add your sensor type here` hinzu:

    ```JavaScript
        var temperatureType = "Temperature";
        var temperatureThreshold = 78;
    ```

    b. Fügen Sie die folgenden Zeilen nach der Anweisung, die `var motionSensor` definiert, unterhalb des Kommentars `// Add your sensor variable here` hinzu:

     ```JavaScript
        var temperatureSensor = otherSensors.find(function(element) {
            return element.DataType === temperatureType;
        });
    ```

    c. Fügen Sie die folgenden Zeilen nach der Anweisung, die `var carbonDioxideValue` definiert, unterhalb des Kommentars `// Add your sensor latest value here` hinzu:

    ```JavaScript
        var temperatureValue = getFloatValue(temperatureSensor.Value().Value);
    ```

    d. Entfernen Sie die folgenden Codezeilen unter dem Kommentar `// Modify this line to monitor your sensor value`:

     ```JavaScript
        if(carbonDioxideValue === null || motionValue === null) {
            sendNotification(telemetry.SensorId, "Sensor", "Error: Carbon dioxide or motion are null, returning");
            return;
        }
    ```

    Ersetzen Sie sie durch die folgenden Zeilen:

    ```JavaScript
        if(carbonDioxideValue === null || motionValue === null || temperatureValue === null){
            sendNotification(telemetry.SensorId, "Sensor", "Error: Carbon dioxide, motion, or temperature are null, returning");
            return;
        }
    ```

    e. Entfernen Sie die folgenden Codezeilen unter dem Kommentar `// Modify these lines as per your sensor`:

    ```JavaScript
        var availableFresh = "Room is available and air is fresh";
        var noAvailableOrFresh = "Room is not available or air quality is poor";
    ```

    Ersetzen Sie sie durch die folgenden Zeilen:

    ```JavaScript
        var alert = "Room with fresh air and comfortable temperature is available.";
        var noAlert = "Either room is occupied, or working conditions are not right.";
    ```

    f. Entfernen Sie den folgenden *if-else*-Codeblock nach dem Kommentar `// Modify this code block for your sensor`:

    ```JavaScript
        // If carbonDioxide less than threshold and no presence in the room => log, notify and set parent space computed value
        if(carbonDioxideValue < carbonDioxideThreshold && !presence) {
            log(`${availableFresh}. Carbon Dioxide: ${carbonDioxideValue}. Presence: ${presence}.`);
            setSpaceValue(parentSpace.Id, spaceAvailFresh, availableFresh);
        }
        else {
            log(`${noAvailableOrFresh}. Carbon Dioxide: ${carbonDioxideValue}. Presence: ${presence}.`);
            setSpaceValue(parentSpace.Id, spaceAvailFresh, noAvailableOrFresh);

            // Set up custom notification for poor air quality
            parentSpace.Notify(JSON.stringify(noAvailableOrFresh));
        }
    ```

    Ersetzen Sie ihn durch den folgenden *if-else*-Block:

    ```JavaScript
        // If sensor values are within range and room is available
        if(carbonDioxideValue < carbonDioxideThreshold && temperatureValue < temperatureThreshold && !presence) {
            log(`${alert}. Carbon Dioxide: ${carbonDioxideValue}. Temperature: ${temperatureValue}. Presence: ${presence}.`);

            // log, notify and set parent space computed value
            setSpaceValue(parentSpace.Id, spaceAvailFresh, alert);

            // Set up notification for this alert
            parentSpace.Notify(JSON.stringify(alert));
        }
        else {
            log(`${noAlert}. Carbon Dioxide: ${carbonDioxideValue}. Temperature: ${temperatureValue}. Presence: ${presence}.`);

            // log, notify and set parent space computed value
            setSpaceValue(parentSpace.Id, spaceAvailFresh, noAlert);
        }
    ```

    Die geänderte benutzerdefinierte Funktion sucht nach einer Bedingung, unter der ein Raum verfügbar ist und die Kohlendioxid- und Temperaturwerte den akzeptablen Grenzwerten entsprechen. Sie generiert eine Benachrichtigung mit der Anweisung `parentSpace.Notify(JSON.stringify(alert));`, wenn diese Bedingung erfüllt ist. Die Funktion legt den Wert des überwachten Gebäudebereichs mit der entsprechenden Meldung fest, unabhängig davon, ob die Bedingung erfüllt ist.

    g. Speichern Sie die Datei .

1. Öffnen Sie ein Befehlsfenster, und navigieren Sie zum Ordner **occupancy-quickstart\src**. Führen Sie den folgenden Befehl aus, um Ihren Raumintelligenzgraphen und die benutzerdefinierte Funktion bereitzustellen:

    ```cmd/sh
    dotnet run ProvisionSample
    ```

   > [!IMPORTANT]
   > Um nicht autorisierten Zugriff auf die Digital Twins-Verwaltungs-API zu verhindern, erfordert die Anwendung **occupancy-quickstart**, dass Sie sich mit Ihren Azure-Anmeldeinformationen anmelden. Die Anwendung speichert Ihre Anmeldeinformationen für einen kurzen Zeitraum, damit Sie sich nicht jedes Mal anmelden müssen, wenn Sie sie ausführen. Bei der erstmaligen Ausführung des Programms und beim Ablauf der gespeicherten Anmeldeinformationen werden Sie von der Anwendung zu einer Anmeldeseite geleitet, und Sie erhalten einen sitzungsspezifischen Code, den Sie auf dieser Seite eingeben müssen. Folgen Sie den Aufforderungen auf dem Bildschirm, um sich mit Ihrem Azure-Konto anzumelden.

1. Sobald Ihr Konto authentifiziert wurde, beginnt die Anwendung, einen Beispielraumgraphen entsprechend der Konfiguration in der Datei *provisionSample.yaml* zu erstellen. Warten Sie, bis die Bereitstellung abgeschlossen ist. Dieser Vorgang dauert einige Minuten. Sehen Sie sich anschließend die Meldungen im Befehlsfenster an, und beobachten Sie, wie der Raumgraph erstellt wird. Beachten Sie, dass die Anwendung im Stammknoten oder in `Venue` einen IoT-Hub erstellt.

1. Kopieren Sie in der Ausgabe im Befehlsfenster den Wert von `ConnectionString` unter dem Abschnitt `Devices` in die Zwischenablage. Sie benötigen diesen Wert im nächsten Abschnitt, um die Verbindung mit dem Gerät zu simulieren.

    [![Bereitstellen des Beispiels](./media/tutorial-facilities-udf/run-provision-sample.png)](./media/tutorial-facilities-udf/run-provision-sample.png#lightbox)

> [!TIP]
> Falls während der Bereitstellung eine Fehlermeldung mit dem Hinweis angezeigt wird, dass der E/A-Vorgang aufgrund einer Threadbeendigung oder Anwendungsanforderung abgebrochen wurde, führen Sie den Befehl noch einmal aus. Dies kann passieren, wenn wegen eines Netzwerkproblems ein Timeout des HTTP-Clients auftritt.

## <a name="simulate-sensor-data"></a>Simulieren von Sensordaten

In diesem Abschnitt verwenden Sie das Projekt namens *device-connectivity* im Beispiel. Sie simulieren Sensordaten für die Erkennung von Bewegung, Temperatur und Kohlendioxid. Das Projekt generiert Zufallswerte für die Sensoren und sendet diese mithilfe der Verbindungszeichenfolge des Geräts an den IoT-Hub.

1. Navigieren Sie in einem separaten Befehlsfenster zum Azure Digital Twins-Beispiel und anschließend zum Ordner **device-connectivity**.

1. Führen Sie den folgenden Befehl aus, um sicherzustellen, dass die Abhängigkeiten für das Projekt korrekt sind:

    ```cmd/sh
    dotnet restore
    ```

1. Öffnen Sie die Datei [appsettings.json](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/device-connectivity/appsettings.json) im Editor, und bearbeiten Sie die folgenden Werte:

   a. **DeviceConnectionString**: Weisen Sie den `ConnectionString`-Wert zu, der im vorherigen Abschnitt im Ausgabefenster angezeigt wurde. Kopieren Sie die vollständige Zeichenfolge innerhalb der Anführungszeichen, damit der Simulator ordnungsgemäß eine Verbindung mit dem IoT-Hub herstellen kann.

   b. **HardwareId** innerhalb des **Sensors**-Arrays: Da Sie Ereignisse von Sensoren simulieren, die in Ihrer Azure Digital Twins-Instanz bereitgestellt wurden, müssen die Hardware-ID und die Namen der Sensoren in dieser Datei den Angaben im Knoten `sensors` in der Datei *provisionSample.yaml* entsprechen.

      Fügen Sie einen neuen Eintrag für den Temperatursensor hinzu. Der Knoten **Sensors** in *appsettings.json* sollte wie folgt aussehen:

      ```JSON
      "Sensors": [{
        "DataType": "Motion",
        "HardwareId": "SAMPLE_SENSOR_MOTION"
      },{
        "DataType": "CarbonDioxide",
        "HardwareId": "SAMPLE_SENSOR_CARBONDIOXIDE"
      },{
        "DataType": "Temperature",
        "HardwareId": "SAMPLE_SENSOR_TEMPERATURE"
      }]
      ```

1. Führen Sie den folgenden Befehl aus, um die Simulation von Geräteereignissen für Temperatur, Bewegung und Kohlendioxid zu starten:

    ```cmd/sh
    dotnet run
    ```

   > [!NOTE]
   > Da das Simulationsbeispiel nicht direkt mit Ihrer Digital Twins-Instanz kommuniziert, müssen Sie sich nicht authentifizieren.

## <a name="get-results-of-the-user-defined-function"></a>Abrufen der Ergebnisse der benutzerdefinierten Funktion

Die benutzerdefinierte Funktion wird jedes Mal ausgeführt, wenn Ihre Instanz Geräte- und Sensordaten empfängt. In diesem Abschnitt fragen Sie Ihre Azure Digital Twins-Instanz ab, um die Ergebnisse der benutzerdefinierten Funktion abzurufen. Sie sehen nahezu in Echtzeit, wann ein Raum verfügbar, die Luft frisch und die Temperatur geeignet ist. 

1. Öffnen Sie das Befehlsfenster, das Sie zum Bereitstellen des Beispiels verwendet haben, oder ein neues Befehlsfenster, und navigieren Sie erneut zum Ordner **occupancy-quickstart\src** des Beispiels.

1. Führen Sie den folgenden Befehl aus, und melden Sie sich an, wenn Sie dazu aufgefordert werden:

    ```cmd/sh
    dotnet run GetAvailableAndFreshSpaces
    ```

Das Ausgabefenster zeigt die Ausführung der benutzerdefinierten Funktion und fängt Ereignisse der Gerätesimulation ab. 

   [![Ausgabe für die benutzerdefinierte Funktion](./media/tutorial-facilities-udf/udf-running.png)](./media/tutorial-facilities-udf/udf-running.png#lightbox)

Ist die überwachte Bedingung erfüllt, legt die benutzerdefinierte Funktion den Wert des Gebäudebereichs mit der relevanten Meldung fest, wie [zuvor](#create-a-user-defined-function) gezeigt. Die Funktion `GetAvailableAndFreshSpaces` gibt die Meldung in der Konsole aus.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie sich nicht weiter mit Azure Digital Twins befassen möchten, können Sie die in diesem Tutorial erstellten Ressourcen löschen:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) im Menü auf der linken Seite **Alle Ressourcen** und Ihre Digital Twins-Ressourcengruppe aus, und klicken Sie auf **Löschen**.

    > [!TIP]
    > Für den Fall, dass bei Ihnen Probleme beim Löschen der Digital Twins-Instanz aufgetreten sind, wurde ein Dienstupdate mit einer entsprechenden Korrektur bereitgestellt. Versuchen Sie erneut, die Instanz zu löschen.

2. Löschen Sie ggf. die Beispielanwendungen auf Ihrem Arbeitscomputer.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Ihre Gebäudebereiche bereitgestellt und ein Framework zum Auslösen benutzerdefinierter Benachrichtigungen erstellt haben, können Sie mit einem der folgenden Tutorials fortfahren:

> [!div class="nextstepaction"]
> [Tutorial: Empfangen von Benachrichtigungen von den Azure Digital Twins-Gebäudebereichen mit Azure Logic Apps](tutorial-facilities-events.md)

> [!div class="nextstepaction"]
> [Tutorial: Visualisieren und Analysieren von Ereignissen in den Azure Digital Twins-Gebäudebereichen mit Azure Time Series Insights](tutorial-facilities-analyze.md)
