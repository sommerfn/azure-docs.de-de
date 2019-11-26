---
title: 'Tutorial: Bereitstellen von Azure Digital Twins'
description: In diesem Tutorial erfahren Sie, wie Sie Ihre Instanz von Azure Digital Twins bereitstellen und Ihre räumlichen Ressourcen konfigurieren.
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.custom: seodec18
ms.service: digital-twins
ms.topic: tutorial
ms.date: 11/12/2019
ms.openlocfilehash: 4d4e7e47b82c46cca53fab0540a4867031eaab85
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74107745"
---
# <a name="tutorial-deploy-azure-digital-twins-preview-and-configure-a-spatial-graph"></a>Tutorial: Bereitstellen von Azure Digital Twins (Vorschauversion) und Konfigurieren eines Raumgraphen

Mit dem Azure Digital Twins-Dienst (Vorschauversion) können Sie Personen, Orte und Geräte in einem kohärenten räumlichen System zusammenbringen. In dieser Tutorialreihe wird veranschaulicht, wie Sie mit Azure Digital Twins die Raumbelegung mit optimalen Temperatur- und Luftqualitätsbedingungen ermitteln. 

In den Tutorials wird anhand einer .NET-Konsolenanwendung Schritt für Schritt erläutert, wie Sie ein Szenario für ein Bürogebäude erstellen. Das Gebäude hat mehrere Etagen und Räume auf jeder Etage. Die Räume sind mit Geräten mit angeschlossenen Sensoren zur Erkennung von Bewegungen, der Umgebungstemperatur und der Luftqualität ausgestattet. 

Sie erfahren, wie Sie die physischen Bereiche und Entitäten im Gebäude mit dem Azure Digital Twins-Dienst als digitale Objekte replizieren. Mit einer weiteren Konsolenanwendung simulieren Sie Geräteereignisse. Anschließend erfahren Sie, wie Sie die Ereignisse in diesen physischen Bereichen und Entitäten nahezu in Echtzeit überwachen. 

Anhand dieser Informationen kann ein Büroadministrator den im Gebäude tätigen Mitarbeitern dabei helfen, Besprechungsräume mit optimalen Bedingungen zu buchen. Ein Facility-Manager des Bürogebäudes kann mithilfe Ihres Setups Trends bei der Nutzung der Räume erkennen und Arbeitsbedingungen zu Wartungszwecken überwachen.

Im ersten Tutorial dieser Reihe lernen Sie Folgendes:

> [!div class="checklist"]
> * Bereitstellen von Digital Twins
> * Gewähren von Berechtigungen für Ihre App
> * Ändern einer Digital Twins-Beispiel-App
> * Bereitstellen Ihres Gebäudes

In diesen Tutorials werden die Beispiele aus [Quickstart: Find available rooms](quickstart-view-occupancy-dotnet.md) (Schnellstart: Ermitteln von verfügbaren Räumen) verwendet und geändert, um die Konzepte ausführlicher zu erläutern.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Abonnement. Falls Sie noch kein Azure-Konto haben, erstellen Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

- Das .NET Core SDK. Die Azure Digital Twins-Beispiele in diesen Tutorials sind in C# geschrieben. Zum Erstellen und Ausführen des Beispiels müssen Sie [.NET Core SDK Version 2.1.403 oder höher](https://www.microsoft.com/net/download) auf dem Entwicklungscomputer installieren. Überprüfen Sie, ob die richtige Version auf Ihrem Computer installiert ist, indem Sie `dotnet --version` in einem Befehlsfenster ausführen.

- [Visual Studio Code](https://code.visualstudio.com/) zum Untersuchen des Beispielcodes. 

## <a name="deploy-digital-twins"></a>Bereitstellen von Azure Digital Twins

Erstellen Sie anhand der Schritte in diesem Abschnitt eine neue Instanz des Azure Digital Twins-Diensts. Pro Abonnement kann nur eine Instanz erstellt werden. Fahren Sie mit dem nächsten Abschnitt fort, wenn Sie bereits über eine Instanz verfügen. 

[!INCLUDE [create-digital-twins-portal](../../includes/digital-twins-create-portal.md)]

## <a name="grant-permissions-to-your-app"></a>Gewähren von Berechtigungen für Ihre App

Digital Twins verwendet [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) (Azure AD), um den [Lese-/Schreibzugriff](../active-directory/develop/v1-permissions-and-consent.md) auf den Dienst zu steuern. Jede Anwendung, die eine Verbindung mit Ihrer Digital Twins-Instanz herstellen muss, muss in Azure AD registriert werden. In diesem Abschnitt wird erläutert, wie Sie Ihre Beispiel-App registrieren.

Falls Sie bereits über eine App-Registrierung verfügen, können Sie sie für das Beispiel wiederverwenden. Sie sollten diesen Abschnitt aber trotzdem kurz durchlesen, um sicherzustellen, dass Ihre App-Registrierung korrekt konfiguriert ist.

[!INCLUDE [digital-twins-permissions](../../includes/digital-twins-permissions.md)]

## <a name="configure-the-digital-twins-sample"></a>Konfigurieren des Digital Twins-Beispiels

In diesem Abschnitt wird die Konfiguration einer Azure Digital Twins-Anwendung erläutert, die mit den [Digital Twins-REST-APIs](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index) kommuniziert. 

### <a name="download-the-sample"></a>Herunterladen des Beispiels

Wenn Sie die Beispiele für [Quickstart: Find available rooms](quickstart-view-occupancy-dotnet.md) (Schnellstart: Ermitteln verfügbarer Räume) bereits heruntergeladen haben, können Sie diese Schritte überspringen.

1. Laden Sie die [.NET-Beispiele für Digital Twins](https://github.com/Azure-Samples/digital-twins-samples-csharp/archive/master.zip) herunter.
2. Extrahieren Sie den Inhalt des ZIP-Ordners auf Ihrem Computer.

### <a name="explore-the-sample"></a>Untersuchen des Beispiels

Öffnen Sie im extrahierten Ordner die Datei **digital-twins-samples-csharp\digital-twins-samples.code-workspace** in Visual Studio Code. Das Beispiel umfasst zwei Projekte:

* Mit dem Bereitstellungsbeispiel **occupancy-quickstart** können Sie einen [Raumintelligenzgraphen](concepts-objectmodel-spatialgraph.md#digital-twins-object-models) konfigurieren und bereitstellen. Bei diesem Graphen handelt es sich um das digitalisierte Abbild Ihrer physischen Gebäudebereiche und der darin enthaltenen Ressourcen. Im Beispiel wird ein [Objektmodell](concepts-objectmodel-spatialgraph.md#digital-twins-object-models) verwendet, das Objekte für ein intelligentes Gebäude definiert. Eine vollständige Liste der Digital Twins-Objekte und -REST-APIs finden Sie in der [REST-API-Dokumentation](https://docs.westcentralus.azuresmartspaces.net/management/swagger) oder unter der Verwaltungs-API-URL, die für [Ihre Instanz](#deploy-digital-twins) erstellt wurde.

   Um die Beispielanwendung zu untersuchen und herauszufinden, wie sie mit Ihrer Digital Twins-Instanz kommuniziert, können Sie mit dem Ordner **src\actions** beginnen. Die Dateien in diesem Ordner implementieren die Befehle, die Sie in diesen Tutorials verwenden werden:
    - Die Datei **provisionSample.cs** zeigt, wie Sie Ihren Raumgraphen bereitstellen.
    - Die Datei **getSpaces.cs** ruft Informationen zu den bereitgestellten Gebäudebereichen ab.
    - Die Datei **getAvailableAndFreshSpaces.cs** ruft die Ergebnisse einer benutzerdefinierten Funktion ab.
    - Die Datei **createEndpoints.cs** erstellt Endpunkte für die Interaktion mit anderen Diensten.

* Das Simulationsbeispiel **device-connectivity** simuliert Sensordaten und sendet diese an den für Ihre Digital Twins-Instanz bereitgestellten IoT-Hub. Sie verwenden dieses Beispiel im [nächsten Tutorial, nachdem Sie den Raumgraphen bereitgestellt haben](tutorial-facilities-udf.md#simulate-sensor-data). Die zum Konfigurieren des Beispiels verwendeten Sensor- und Gerätebezeichner sollten mit den Bezeichnern identisch sein, die Sie zum Bereitstellen Ihres Graphen verwenden.

### <a name="configure-the-provisioning-sample"></a>Konfigurieren des Bereitstellungsbeispiels

1. Öffnen Sie ein Befehlsfenster, und navigieren Sie zum heruntergeladenen Beispiel. Führen Sie den folgenden Befehl aus:

    ```cmd/sh
    cd occupancy-quickstart/src
    ```

1. Stellen Sie die Abhängigkeiten im Beispielprojekt wieder her, indem Sie den folgenden Befehl ausführen:

    ```cmd/sh
    dotnet restore
    ```

1. Öffnen Sie in Visual Studio Code die Datei [appSettings.json](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/appSettings.json) des Projekts **occupancy-quickstart**. Aktualisieren Sie die folgenden Werte:
   * **ClientId**: Geben Sie die Anwendungs-ID Ihrer Azure AD-App-Registrierung ein. Diese ID haben Sie sich im Abschnitt zum [Festlegen der App-Berechtigungen](#grant-permissions-to-your-app) notiert.
   * **Tenant**: Geben Sie die Verzeichnis-ID Ihres [Azure AD-Mandanten](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) ein. Diese ID haben Sie sich ebenfalls im Abschnitt zum [Festlegen der App-Berechtigungen](#grant-permissions-to-your-app) notiert.
   * **BaseUrl**: Geben Sie die URL Ihrer Digital Twins-Instanz ein. Diese URL erhalten Sie, indem Sie die Platzhalter in der folgenden URL durch die Werte für Ihre Instanz ersetzen: `https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/api/v1.0/`. Alternativ können Sie die Verwaltungs-API-URL aus dem [Abschnitt zur Bereitstellung](#deploy-digital-twins) ändern, um die URL zu erhalten. Ersetzen Sie **swagger/** durch **api/v1.0/** .

1. Zeigen Sie eine Liste der Digital Twins-Funktionen an, die Sie mithilfe des Beispiels erkunden können. Führen Sie den folgenden Befehl aus:

    ```cmd/sh
    dotnet run
    ```

## <a name="understand-the-provisioning-process"></a>Grundlegendes zum Bereitstellungsprozess

In diesem Abschnitt wird erläutert, wie mit dem Beispiel ein Raumgraph eines Gebäudes bereitgestellt wird.

Navigieren Sie in Visual Studio Code zum Ordner **occupancy-quickstart\src\actions**, und öffnen Sie Datei **provisionSample.cs**. Beachten Sie die folgende Funktion:

```csharp
public static async Task<IEnumerable<ProvisionResults.Space>> ProvisionSample(HttpClient httpClient, ILogger logger)
{
    IEnumerable<SpaceDescription> spaceCreateDescriptions;
    using (var r = new StreamReader("actions/provisionSample.yaml"))
    {
        spaceCreateDescriptions = await GetProvisionSampleTopology(r);
    }

    var results = await CreateSpaces(httpClient, logger, spaceCreateDescriptions, Guid.Empty);

    Console.WriteLine($"Completed Provisioning: {JsonConvert.SerializeObject(results, Formatting.Indented)}");

    return results;
}

```

Diese Funktion verwendet die Datei [provisionSample.yaml](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/provisionSample.yaml) im selben Ordner. Öffnen Sie diese Datei. Dort sehen Sie die Hierarchie eines Bürogebäudes: *Venue* (Ort), *Floor* (Etage), *Area* (Bereich) und *Rooms* (Räume). Jeder dieser physischen Gebäudebereiche kann *devices*(Geräte) und *sensors* (Sensoren) enthalten. Jeder Eintrag besitzt einen vordefinierten `type`, beispielsweise „Floor“ oder „Room“.

Die **YAML**-Beispieldatei zeigt einen Raumgraphen mit dem Digital Twins-Objektmodell `Default`. Dieses Modell stellt allgemeine Namen für die meisten Typen bereit. Allgemeine Namen sind für ein Gebäude ausreichend. Beispiele sind „Temperature“ für „SensorDataType“ und „Map“ für „SpaceBlobType“. Ein Beispielbereichstyp ist „Room“ mit den Untertypen „FocusRoom“, „ConferenceRoom“ usw. 

Zum Erstellen eines Raumgraphen für eine andere Art von Ort (z. B. eine Fabrik) benötigen Sie möglicherweise ein anderes Objektmodell. Sie können die verfügbaren Modelle anzeigen, indem Sie den Befehl `dotnet run GetOntologies` in der Befehlszeile für das Bereitstellungsbeispiel ausführen. 

Weitere Informationen zu Raumgraphen und den Objektmodellen finden Sie unter [Grundlegendes zum Digital Twins-Objektmodell und zum Raumintelligenzgraphen](concepts-objectmodel-spatialgraph.md).

### <a name="modify-the-sample-spatial-graph"></a>Ändern des Beispielraumgraphen

Die Datei **provisionSample.yaml** enthält die folgenden Knoten:

- **resources**: Der Knoten `resources` erstellt eine Azure IoT Hub-Ressource für die Kommunikation mit den Geräten in Ihrem Setup. Ein IoT-Hub im Stammknoten des Graphen kann mit allen Geräten und Sensoren in Ihrem Graphen kommunizieren.  

- **spaces**: Im Digital Twins-Objektmodell stellen `spaces` (Gebäudebereiche) die physischen Orte dar. Jeder Gebäudebereich verfügt über einen `Type`, beispielsweise „Region“, „Venue“ oder „Customer“ und einen angezeigten `Name`. Gebäudebereiche können zu anderen Gebäudebereichen gehören, sodass eine hierarchische Struktur entsteht. Die Datei „provisionSample.yaml“ enthält einen Raumgraphen für ein imaginäres Gebäude. Beachten Sie die logische Schachtelung von Gebäudebereichen des Typs `Floor` innerhalb von `Venue`, von `Area` innerhalb einer Etage und von `Room`-Knoten in einem Bereich. 

- **devices**: Gebäudebereiche können `devices` (Geräte) enthalten. Dies sind physische oder virtuelle Entitäten, die eine Reihe von Sensoren verwalten. Ein Gerät kann beispielsweise das Telefon eines Benutzers sein, ein Raspberry Pi-Sensorhalter oder ein Gateway. Im imaginären Gebäude im Beispiel enthält der Raum namens **Focus Room** ein Gerät **Raspberry Pi 3 A1**. Jeder Geräteknoten wird durch eine eindeutige `hardwareId` identifiziert, die in diesem Beispiel hartcodiert ist. Ersetzen Sie diese Werte durch die Werte aus Ihrem Setup, um das Beispiel für eine reale Produktionsumgebung zu konfigurieren.  

- **sensors**: Ein Gerät kann mehrere `sensors` (Sensoren) enthalten. Sie können physische Änderungen wie Temperatur, Bewegung und Akkustand erkennen und aufzeichnen. Jeder Sensorknoten wird durch eine `hardwareId` eindeutig identifiziert, die hier hartcodiert ist. Ersetzen Sie diese IDs für eine reale Anwendung durch die eindeutigen Bezeichner der Sensoren in Ihrem Setup. Die Datei „provisionSample.yaml“ enthält zwei Sensoren zum Aufzeichnen von *Motion* (Bewegung) und *CarbonDioxide* (Kohlendioxid). Fügen Sie einen weiteren Sensor zum Aufzeichnen der *Temperature* (Temperatur) hinzu, indem Sie die folgenden Zeilen unterhalb der Zeilen für den CarbonDioxide-Sensor hinzufügen. Beachten Sie, dass diese in der Datei „provisionSample.yaml“ als auskommentierte Zeilen angegeben werden. Sie können ihre Kommentierung aufheben, indem Sie jeweils das Zeichen `#` am Zeilenanfang entfernen. 

    ```yaml
            - dataType: Temperature
              hardwareId: SAMPLE_SENSOR_TEMPERATURE
    ```
    > [!NOTE]
    > Stellen Sie sicher, dass die Schlüssel `dataType` und `hardwareId` den Anweisungen über diesem Codeausschnitt entsprechen. Vergewissern Sie sich außerdem, dass Ihr Editor Leerzeichen nicht durch Tabstopps ersetzt. 

Speichern und schließen Sie die Datei „provisionSample.yaml“. Im nächsten Tutorial fügen Sie dieser Datei weitere Informationen hinzu, und anschließend stellen Sie Ihr Azure Digital Twins-Beispielgebäude bereit.

> [!TIP]
> Sie können Ihren Raumgraphen mithilfe des [Azure Digital Twins Graph Viewer](https://github.com/Azure/azure-digital-twins-graph-viewer) anzeigen und ändern.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie sich nicht weiter mit Azure Digital Twins befassen möchten, können Sie die in diesem Tutorial erstellten Ressourcen löschen:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) im Menü auf der linken Seite **Alle Ressourcen** und Ihre Digital Twins-Ressourcengruppe aus, und klicken Sie auf **Löschen**.

    > [!TIP]
    > Für den Fall, dass bei Ihnen Probleme beim Löschen der Digital Twins-Instanz aufgetreten sind, wurde ein Dienstupdate mit einer entsprechenden Korrektur bereitgestellt. Versuchen Sie erneut, die Instanz zu löschen.

1. Löschen Sie ggf. die Beispielanwendung auf Ihrem Arbeitscomputer.

## <a name="next-steps"></a>Nächste Schritte

Im nächsten Tutorial dieser Reihe erfahren Sie, wie Sie eine benutzerdefinierte Logik zum Überwachen von Bedingungen in Ihrem Beispielgebäude implementieren. 
> [!div class="nextstepaction"]
> [Tutorial: Bereitstellen des Gebäudes und Überwachen der Arbeitsbedingungen](tutorial-facilities-udf.md)