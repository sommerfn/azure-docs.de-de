---
title: 'Suchen nach verfügbaren Räumen: Azure Digital Twins | Microsoft-Dokumentation'
description: In dieser Schnellstartanleitung führen Sie zwei .NET Core-Beispielanwendungen aus, um simulierte Bewegungs- und CO2-Telemetriedaten an einen Bereich in Azure Digital Twins zu senden. Das Ziel besteht darin, über Verwaltungs-APIs verfügbare Räume mit frischer Luft zu finden, nachdem die Verarbeitung in der Cloud durchgeführt wurde.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.devlang: csharp
ms.topic: quickstart
ms.custom: mvc seodec18
ms.date: 10/03/2019
ms.openlocfilehash: 3c9a806b936b9f167d1700c95b1e769926abb17b
ms.sourcegitcommit: 7868d1c40f6feb1abcafbffcddca952438a3472d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2019
ms.locfileid: "71958912"
---
# <a name="quickstart-find-available-rooms-by-using-azure-digital-twins"></a>Schnellstart: Suchen nach verfügbaren Räumen mithilfe von Azure Digital Twins

Mit dem Azure Digital Twins-Dienst können Sie Ihre physische Umgebung digital abbilden. Anschließend können Sie durch Ereignisse in Ihrer Umgebung benachrichtigt werden und Ihre Reaktionen anpassen.

In diesem Schnellstart wird [ein .NET-Beispielpaar](https://github.com/Azure-Samples/digital-twins-samples-csharp) verwendet, um ein imaginäres Bürogebäude zu digitalisieren. Er veranschaulicht die Suche nach verfügbaren Räumen in diesem Gebäude. Mit Digital Twins können Sie mehrere Sensoren mit Ihrer Umgebung verknüpfen. Dank eines simulierten CO2-Sensors können Sie auch ermitteln, ob die Luftqualität des verfügbaren Raums gut genug ist. Eine der Beispielanwendungen generiert willkürliche Sensordaten, damit Sie sich das Szenario besser vorstellen können.

Im folgenden Video wird die Schnellstarteinrichtung zusammengefasst:

>[!VIDEO https://www.youtube.com/embed/1izK266tbMI]

## <a name="prerequisites"></a>Voraussetzungen

1. Sollten Sie kein Azure-Konto haben, erstellen Sie zunächst ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

1. Die beiden in dieser Schnellstartanleitung ausgeführten Konsolenanwendungen sind in C# geschrieben. Installieren Sie [.NET Core SDK Version 2.1.403 oder höher](https://www.microsoft.com/net/download) auf dem Entwicklungscomputer. Wenn das .NET Core SDK bereits installiert ist, überprüfen Sie, ob auf Ihrem Entwicklungscomputer die aktuelle Version von C# ausgeführt wird. Führen Sie `dotnet --version` an einer Eingabeaufforderung aus.

1. Laden Sie das [C#-Beispielprojekt](https://github.com/Azure-Samples/digital-twins-samples-csharp/archive/master.zip) herunter. Extrahieren Sie das ZIP-Archiv „digital-twins-samples-csharp-master.zip“.

## <a name="create-a-digital-twins-instance"></a>Erstellen einer Digital Twins-Instanz

Erstellen Sie im [Portal](https://portal.azure.com) mithilfe der Schritte in diesem Abschnitt eine neue Instanz von Digital Twins.

[!INCLUDE [create-digital-twins-portal](../../includes/digital-twins-create-portal.md)]

## <a name="set-permissions-for-your-app"></a>Festlegen von Berechtigungen für Ihre App

In diesem Abschnitt wird Ihre Beispielanwendung bei Azure Active Directory (Azure AD) registriert, damit sie auf Ihre Digital Twins-Instanz zugreifen kann. Falls Sie bereits über eine Azure AD-App-Registrierung verfügen, verwenden Sie diese für das Beispiel wieder. Stellen Sie sicher, dass sie gemäß der Beschreibung in diesem Abschnitt konfiguriert ist.

[!INCLUDE [digital-twins-permissions](../../includes/digital-twins-permissions.md)]

## <a name="build-application"></a>Erstellen der Anwendung

Führen Sie diese Schritte aus, um die Belegungsanwendung zu erstellen.

1. Öffnen Sie eine Eingabeaufforderung. Wechseln Sie zu dem Ordner, in dem die Dateien aus `digital-twins-samples-csharp-master.zip` extrahiert wurden.
1. Führen Sie `cd occupancy-quickstart/src`aus.
1. Führen Sie `dotnet restore`aus.
1. Bearbeiten Sie [appSettings.json](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/appSettings.json), um folgende Variablen zu aktualisieren:
    - **ClientId:** Geben Sie die Anwendungs-ID Ihrer Azure AD-App-Registrierung ein, die Sie sich im vorherigen Abschnitt notiert haben.
    - **Tenant:** Geben Sie die Verzeichnis-ID Ihres Azure AD-Mandanten ein, die Sie sich ebenfalls im vorherigen Abschnitt notiert haben.
    - **BaseUrl:** Die URL der Verwaltungs-API Ihrer Digital Twins-Instanz im Format `https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/api/v1.0/`. Ersetzen Sie die Platzhalter in dieser URL durch die Werte für Ihre Instanz aus dem vorherigen Abschnitt.

## <a name="provision-graph"></a>Bereitstellen des Graphen

Mit diesem Schritt wird Ihr Digital Twins-Raumgraph bereitgestellt, der Folgendes enthält:

- Mehrere Bereiche
- Ein Gerät
- Zwei Sensoren
- Eine benutzerdefinierte Funktion
- Eine Rollenzuweisung

Der Raumgraph wird mithilfe der Datei [provisionSample.yaml](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/provisionSample.yaml) bereitgestellt.

1. Führen Sie `dotnet run ProvisionSample`aus.

    >[!NOTE]
    >Das Azure CLI-Tool für die Geräteanmeldung wird verwendet, um den Benutzer bei Azure AD zu authentifizieren. Der Benutzer muss einen bestimmten Code eingeben, um sich über die [Microsoft-Anmeldeseite](https://microsoft.com/devicelogin) zu authentifizieren. Führen Sie nach der Codeeingabe die Schritte für die Authentifizierung aus. Wenn das Tool ausgeführt wird, muss sich der Benutzer authentifizieren.

    >[!TIP]
    > Stellen Sie beim Ausführen dieses Schritts sicher, dass Ihre Variablen ordnungsgemäß kopiert wurden, wenn die folgende Fehlermeldung angezeigt wird: `EXIT: Unexpected error: The input is not a valid Base-64 string ...`

1. Der Bereitstellungsschritt kann einige Minuten dauern. Dabei wird auch ein IoT Hub in Ihrer Digital Twins-Instanz bereitgestellt. Er durchläuft eine Schleife, bis der IoT Hub „Status=`Running`“ anzeigt.

    [![Bereitstellen des Beispiels](media/quickstart-view-occupancy-dotnet/digital-twins-provision-sample1.png)](media/quickstart-view-occupancy-dotnet/digital-twins-provision-sample1.png#lightbox)

1. Kopieren Sie am Ende der Ausführung die Verbindungszeichenfolge (`ConnectionString`) des Geräts, um sie im Gerätesimulatorbeispiel verwenden zu können. Kopieren Sie nur die in dieser Abbildung markierte Zeichenfolge.

    [![Kopieren der Verbindungszeichenfolge](media/quickstart-view-occupancy-dotnet/digital-twins-provision-sample.png)](media/quickstart-view-occupancy-dotnet/digital-twins-provision-sample.png#lightbox)

    >[!TIP]
    > Sie können Ihren Raumgraphen mithilfe des [Azure Digital Twins Graph Viewer](https://github.com/Azure/azure-digital-twins-graph-viewer) anzeigen und ändern.

## <a name="send-sensor-data"></a>Senden von Sensordaten

Führen Sie diese Schritte aus, um die Sensorsimulatoranwendung zu erstellen und auszuführen.

1. Öffnen Sie eine neue Eingabeaufforderung. Wechseln Sie zum Projekt, das Sie in den Ordner „digital-twins-samples-csharp-master“ heruntergeladen haben.
1. Führen Sie `cd device-connectivity`aus.
1. Führen Sie `dotnet restore`aus.
1. Bearbeiten Sie [appsettings.json](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/device-connectivity/appsettings.json), um **DeviceConnectionString** mit der vorherigen Verbindungszeichenfolge (`ConnectionString`) zu aktualisieren.
1. Führen Sie `dotnet run` aus, um das Senden von Sensordaten zu starten. Sie sehen, wie die Daten wie in der folgenden Abbildung gezeigt an Digital Twins gesendet werden.

     [![Gerätekonnektivität](media/quickstart-view-occupancy-dotnet/digital-twins-device-connectivity.png)](media/quickstart-view-occupancy-dotnet/digital-twins-device-connectivity.png#lightbox)

1. Führen Sie diesen Simulator weiter aus, um Ergebnisse parallel zur Aktion des nächsten Schritts anzeigen zu können. Dieses Fenster zeigt die simulierten Sensordaten, die an Digital Twins gesendet werden. Im nächsten Schritt werden anhand von Abfragen in Echtzeit verfügbare Räume frischer Luft ermittelt.

    >[!TIP]
    > Stellen Sie beim Ausführen dieses Schritts sicher, dass `DeviceConnectionString` ordnungsgemäß kopiert wurde, wenn die folgende Fehlermeldung angezeigt wird: `EXIT: Unexpected error: The input is not a valid Base-64 string ...`

## <a name="find-available-spaces-with-fresh-air"></a>Suchen nach verfügbaren Bereichen mit frischer Luft

Das Sensorbeispiel simuliert zufällige Datenwerte für zwei Sensoren. Es handelt sich um Bewegungs- und CO2-Daten. Verfügbare Bereiche mit frischer Luft sind im Beispiel als Räume definiert, in denen sich niemand aufhält, und deren CO2-Wert unter 1.000 ppm liegt. Ist die Bedingung nicht erfüllt, ist der Bereich nicht verfügbar oder die Luftqualität zu schlecht.

1. Öffnen Sie die Eingabeaufforderung, die Sie zum Ausführen des vorherigen Bereitstellungsschritts verwendet haben.
1. Führen Sie `dotnet run GetAvailableAndFreshSpaces`aus.
1. Sehen Sie sich diese Eingabeaufforderung und die Eingabeaufforderung für die Sensordaten nebeneinander an.

    Eine Eingabeaufforderung sendet alle fünf Sekunden simulierte Bewegungs- und CO2-Daten an Digital Twins. Der andere Befehl liest den Graphen in Echtzeit, um auf der Grundlage willkürlich simulierter Daten verfügbare Räume mit frischer Luft zu ermitteln. Abhängig von den zuletzt übermittelten Sensordaten wird nahezu in Echtzeit eine dieser Bedingungen angezeigt:
   - Verfügbare Räume mit frischer Luft
   - Belegte Räume oder schlechte Luftqualität

     [![Abrufen verfügbarer Bereiche mit frischer Luft](media/quickstart-view-occupancy-dotnet/digital-twins-get-available.png)](media/quickstart-view-occupancy-dotnet/digital-twins-get-available.png#lightbox)

Öffnen Sie [Visual Studio Code](https://code.visualstudio.com/Download) mit dem Codearbeitsbereich-Projekt in „digital-twins-samples-csharp“, um die Vorgänge in diesem Schnellstart und die aufgerufenen APIs nachzuvollziehen. Verwenden Sie den folgenden Befehl:

```plaintext
<path>\occupancy-quickstart\src>code ..\..\digital-twins-samples.code-workspace
```

Die Tutorials enthalten detaillierte Informationen zum Code. Sie erfahren, wie Konfigurationsdaten geändert und wie APIs aufgerufen werden. Weitere Informationen zu Verwaltungs-APIs auf Ihrer Digital Twins Swagger-Seite:

```plaintext
https://YOUR_INSTANCE_NAME.YOUR_LOCATION.azuresmartspaces.net/management/swagger
```

| NAME | Ersetzen durch |
| --- | --- |
| YOUR_INSTANCE_NAME | Den Namen Ihrer Digital Twins-Instanz |
| YOUR_LOCATION | Die Serverregion, in der Ihre Instanz gehostet wird |

Oder navigieren Sie der Einfachheit halber zu [Digital Twins Swagger](https://docs.westcentralus.azuresmartspaces.net/management/swagger).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

In den Tutorials wird Folgendes ausführlich behandelt:

- Erstellen einer Anwendung für Facility-Manager zum Steigern der Produktivität von Personen
- Effizientere Nutzung des Gebäudes

Wenn Sie mit den Tutorials fortfahren möchten, bereinigen Sie die in diesem Schnellstart erstellten Ressourcen nicht. Wenn Sie nicht fortfahren möchten, löschen Sie alle von diesem Schnellstart erstellten Ressourcen.

1. Löschen Sie den Ordner, der beim Herunterladen des Beispielrepositorys erstellt wurde.
1. Wählen Sie im [Azure-Portal](https://portal.azure.com) im Menü auf der linken Seite die Option **Alle Ressourcen** aus. Wählen Sie dann Ihre Digital Twins-Ressource aus. Wählen Sie oben im Bereich **Alle Ressourcen** die Option **Löschen** aus.

    > [!TIP]
    > Für den Fall, dass bei Ihnen Probleme beim Löschen der Digital Twins-Instanz aufgetreten sind, wurde ein Dienstupdate mit einer entsprechenden Korrektur bereitgestellt. Versuchen Sie erneut, die Instanz zu löschen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart wurde anhand eines einfachen Szenarios die Suche nach Räumen mit guten Arbeitsbedingungen veranschaulicht. Eine ausführliche Analyse dieses Szenarios finden Sie in diesem Tutorial:

>[!div class="nextstepaction"]
>[Tutorial: Bereitstellen von Azure Digital Twins und Konfigurieren eines Raumgraphen](tutorial-facilities-setup.md)
