---
title: 'Schnellstart: Clientbibliothek zur Gesichtserkennung für .NET | Microsoft-Dokumentation'
description: Hier erhalten Sie Informationen zu den ersten Schritten mit der Clientbibliothek zur Gesichtserkennung für .NET.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 08/20/2019
ms.author: pafarley
ms.openlocfilehash: 4f06e423c6dcc561ef8e51c33f24cd9f88a681b5
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2019
ms.locfileid: "72935886"
---
# <a name="quickstart-face-client-library-for-net"></a>Schnellstart: Clientbibliothek zur Gesichtserkennung für .NET

Hier erhalten Sie Informationen zu den ersten Schritten mit der Clientbibliothek zur Gesichtserkennung für .NET. Führen Sie die nachfolgenden Schritte zum Installieren des Pakets aus, und testen Sie den Beispielcode für grundlegende Aufgaben. Über den Dienst für die Gesichtserkennungs-API haben Sie Zugriff auf erweiterte Algorithmen für die Erkennung von menschlichen Gesichtern in Bildern.

Verwenden Sie die Clientbibliothek zur Gesichtserkennung für .NET für Folgendes:

* [Authentifizieren des Clients](#authenticate-the-client)
* [Gesichtserkennung in einem Bild](#detect-faces-in-an-image)
* [Suchen ähnlicher Gesichter](#find-similar-faces)
* [Erstellen und Trainieren einer Personengruppe](#create-and-train-a-person-group)
* [Identifizieren eines Gesichts](#identify-a-face)
* [Erstellen einer Momentaufnahme für die Datenmigration](#take-a-snapshot-for-data-migration)

[Referenzdokumentation](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/faceapi?view=azure-dotnet) | [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Vision.Face) | [Paket (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.Face/2.5.0-preview.1) | [Beispiele](https://docs.microsoft.com/samples/browse/?products=azure&term=face)

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/)
* Aktuelle Version von [.NET Core](https://dotnet.microsoft.com/download/dotnet-core).

## <a name="setting-up"></a>Einrichten

### <a name="create-a-face-azure-resource"></a>Erstellen einer Azure-Ressource für die Gesichtserkennung

Azure Cognitive Services werden von Azure-Ressourcen dargestellt, die Sie abonnieren. Erstellen Sie mithilfe des [Azure-Portals](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) oder der [Azure CLI](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) auf Ihrem lokalen Computer eine Ressource für die Gesichtserkennung. Weitere Funktionen:

* Rufen Sie einen kostenlosen [Testschlüssel](https://azure.microsoft.com/try/cognitive-services/#decision) ab, der sieben Tage lang gültig ist. Nach der Registrierung steht dieser auf der [Azure-Website](https://azure.microsoft.com/try/cognitive-services/my-apis/) zur Verfügung.  
* Zeigen Sie Ihre Ressource im [Azure-Portal](https://portal.azure.com/) an.

Nachdem Sie einen Schlüssel für Ihr Testabonnement bzw. Ihre Ressource erhalten haben, [erstellen Sie eine Umgebungsvariable](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) für den Schlüssel und eine Endpunkt-URL mit den Namen `FACE_SUBSCRIPTION_KEY` bzw. `FACE_ENDPOINT`.

### <a name="create-a-new-c-application"></a>Erstellen einer neuen C#-Anwendung

Erstellen Sie eine neue .NET Core-Anwendung in Ihrem bevorzugten Editor oder Ihrer bevorzugten IDE. 

Verwenden Sie in einem Konsolenfenster (z. B. cmd, PowerShell oder Bash) den Befehl `dotnet new` zum Erstellen einer neuen Konsolen-App mit dem Namen `face-quickstart`. Dieser Befehl erstellt ein einfaches „Hallo Welt“-C#-Projekt mit einer einzigen Quelldatei: *Program.cs*. 

```console
dotnet new console -n face-quickstart
```

Wechseln Sie zum Ordner der neu erstellten App. Sie können die Anwendung mit folgendem Befehl erstellen:

```console
dotnet build
```

Die Buildausgabe sollte keine Warnungen oder Fehler enthalten. 

```console
...
Build succeeded.
 0 Warning(s)
 0 Error(s)
...
```

Öffnen Sie aus dem Projektverzeichnis die Datei *Program.cs* in Ihrem bevorzugten Editor oder Ihrer bevorzugten IDE. Fügen Sie die folgenden `using`-Anweisungen hinzu:

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_using)]

Erstellen Sie in der Methode `Main` der Anwendung Variablen für den Azure-Endpunkt und -Schlüssel Ihrer Ressource.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_mainvars)]

### <a name="install-the-client-library"></a>Installieren der Clientbibliothek

Installieren Sie im Anwendungsverzeichnis mit dem folgenden Befehl die Clientbibliothek zur Gesichtserkennung für .NET:

```console
dotnet add package Microsoft.Azure.CognitiveServices.Vision.Face --version 2.5.0-preview.1
```

Bei Verwendung der Visual Studio-IDE ist die Clientbibliothek als herunterladbares NuGet-Paket verfügbar.

## <a name="object-model"></a>Objektmodell

Die folgenden Klassen und Schnittstellen verarbeiten einige der Hauptfunktionen des .NET SDK zur Gesichtserkennung:

|NAME|BESCHREIBUNG|
|---|---|
|[FaceClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceclient?view=azure-dotnet) | Diese Klasse stellt Ihre Autorisierung für die Verwendung des Gesichtserkennungsdiensts dar. Sie benötigen sie für alle Gesichtserkennungsfunktionen. Sie instanziieren sie mit Ihren Abonnementinformationen und verwenden sie zum Generieren von Instanzen anderer Klassen. |
|[FaceOperations](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceoperations?view=azure-dotnet)|Diese Klasse behandelt die grundlegenden Erkennungs- und Wiedererkennungsaufgaben, die Sie für menschliche Gesichter ausführen können. |
|[DetectedFace](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.detectedface?view=azure-dotnet)|Diese Klasse stellt alle Daten dar, die in einem einzelnen Gesicht in einem Bild erkannt wurden. Sie können damit ausführliche Informationen zum Gesicht abrufen.|
|[FaceListOperations](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.facelistoperations?view=azure-dotnet)|Diese Klasse verwaltet die in der Cloud gespeicherten **FaceList**-Konstrukte, die verschiedene Gesichter speichern. |
|[PersonGroupPersonExtensions](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.persongrouppersonextensions?view=azure-dotnet)| Diese Klasse verwaltet die in der Cloud gespeicherten **Person**-Konstrukte, die eine Gruppe von Gesichtern speichern, die zu einer einzelnen Person gehören.|
|[PersonGroupOperations](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.persongroupoperations?view=azure-dotnet)| Diese Klasse verwaltet die in der Cloud gespeicherten **PersonGroup**-Konstrukte, die verschiedene **Person**-Objekte speichern. |
|[ShapshotOperations](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.snapshotoperations?view=azure-dotnet)|Mit dieser Klasse wird die Funktion für Momentaufnahmen verwaltet. Sie können mit ihr vorübergehend alle cloudbasierten Gesichtserkennungsdaten speichern und zu einem neuen Azure-Abonnement migrieren. |

## <a name="code-examples"></a>Codebeispiele

Die folgenden Codeausschnitte veranschaulichen, wie die folgenden Aufgaben mit der Clientbibliothek zur Gesichtserkennung für .NET ausgeführt werden:

* [Authentifizieren des Clients](#authenticate-the-client)
* [Gesichtserkennung in einem Bild](#detect-faces-in-an-image)
* [Suchen ähnlicher Gesichter](#find-similar-faces)
* [Erstellen und Trainieren einer Personengruppe](#create-and-train-a-person-group)
* [Identifizieren eines Gesichts](#identify-a-face)
* [Erstellen einer Momentaufnahme für die Datenmigration](#take-a-snapshot-for-data-migration)


## <a name="authenticate-the-client"></a>Authentifizieren des Clients

> [!NOTE]
> In dieser Schnellstartanleitung wird vorausgesetzt, dass Sie für den Schlüssel für die Gesichtserkennung sowie für den Endpunkt (mit den Namen `FACE_SUBSCRIPTION_KEY` bzw. `FACE_ENDPOINT`) [eine Umgebungsvariable erstellt haben](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication).

Instanziieren Sie in einer neuen Methode einen Client mit Ihrem Endpunkt und Schlüssel. Erstellen Sie ein [CognitiveServicesCredentials](https://docs.microsoft.com/python/api/msrest/msrest.authentication.cognitiveservicescredentials?view=azure-python)-Objekt mit Ihrem Schlüssel, und verwenden Sie es mit Ihrem Endpunkt, um ein [FaceClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceclient?view=azure-dotnet)-Objekt zu erstellen.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_auth)]

Rufen Sie diese Methode am besten in der Methode `Main` auf.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_client)]

## <a name="detect-faces-in-an-image"></a>Gesichtserkennung in einem Bild

Legen Sie im Stammverzeichnis Ihrer Klasse die folgende URL-Zeichenfolge fest. Diese URL verweist auf eine Reihe von Beispielbildern.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_image_url)]

Optional können Sie auswählen, welches KI-Modell zum Extrahieren von Daten aus den erkannten Gesichtern verwendet werden soll. Weitere Informationen zu diesen Optionen finden Sie unter [Angeben eines Gesichtserkennungsmodells](../Face-API-How-to-Topics/specify-recognition-model.md).

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_detect_models)]

Der endgültige Erkennungsvorgang nutzt ein [FaceClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceclient?view=azure-dotnet)-Objekt, eine Bild-URL und ein Erkennungsmodell.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_detect_call)]

In diesem Fall erkennt die `DetectFaceExtract`-Methode Gesichter in drei der Bilder unter der angegebenen URL und erstellt eine Liste von [DetectedFace](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.detectedface?view=azure-dotnet)-Objekten im Programmspeicher. Die Liste der [FaceAttributeType](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.faceattributetype?view=azure-dotnet)-Werte gibt an, welche Features extrahiert werden. Im Beispielcode auf [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/documentation-samples/quickstarts/Face/Program.cs) finden Sie Hilfscode, mit dem diese Daten auf intuitive Weise ausgegeben werden können.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_detect)]

## <a name="find-similar-faces"></a>Suchen ähnlicher Gesichter

Der folgende Code verwendet ein einzelnes erkanntes Gesicht (Quelle) und durchsucht eine Reihe anderer Gesichter (Ziel) auf Übereinstimmungen. Bei einer Übereinstimmung wird die ID des entsprechenden Gesichts in der Konsole ausgegeben.

### <a name="detect-faces-for-comparison"></a>Erkennen von Gesichtern zum Vergleich

Definieren Sie zunächst eine zweite Gesichtserkennungsmethode. Sie müssen Gesichter in Bildern erkennen, bevor Sie sie vergleichen können, und diese Erkennungsmethode ist für Vergleichsvorgänge optimiert. Sie extrahiert keine detaillierten Gesichtsattribute wie im obigen Abschnitt und verwendet ein anderes Erkennungsmodell.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_face_detect_recognize)]

### <a name="find-matches"></a>Suchen von Übereinstimmungen

Die folgende Methode erkennt Gesichter in einer Reihe von Zielbildern und in einem einzelnen Quellbild. Anschließend vergleicht sie diese und sucht alle Zielbilder, die dem Quellbild ähneln.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_find_similar)]

### <a name="print-matches"></a>Ausgeben von Übereinstimmungen

Mit dem folgenden Code werden Übereinstimmungsdetails in der Konsole ausgegeben:

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_find_similar_print)]

## <a name="create-and-train-a-person-group"></a>Erstellen und Trainieren einer Personengruppe

Mit dem folgenden Code wird ein **PersonGroup**-Objekt mit sechs verschiedenen **Person**-Objekten erstellt. Er ordnet jedem **Person**-Objekt eine Reihe von Beispielbildern zu. Danach folgt ein Trainingsschritt, um die einzelnen Personen anhand ihrer Gesichtsmerkmale zu erkennen. **Person**- und **PersonGroup**-Objekte werden in den Überprüfungs-, Identifizierungs- und Gruppierungsvorgängen verwendet.

Legen Sie im Stammverzeichnis Ihrer Klasse die folgende URL-Zeichenfolge fest, sofern noch nicht geschehen. Sie verweist auf eine Reihe von Beispielbildern.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_image_url)]

Der Code weiter unten in diesem Abschnitt legt ein Erkennungsmodell zum Extrahieren von Daten aus Gesichtern fest, und der folgende Codeausschnitt erstellt Verweise auf die verfügbaren Modelle. Weitere Informationen zu Erkennungsmodellen finden Sie unter [Angeben eines Gesichtserkennungsmodells](../Face-API-How-to-Topics/specify-recognition-model.md).

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_detect_models)]

### <a name="create-persongroup"></a>Erstellen einer Personengruppe

Deklarieren Sie eine Zeichenfolgenvariable im Stammverzeichnis Ihrer Klasse, die die ID des erstellten **PersonGroup**-Objekts darstellt.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_persongroup_declare)]

Fügen Sie in einer neuen Methode den folgenden Code hinzu. Mit diesem Code werden die Namen von Personen mit ihren Beispielbildern verknüpft.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_persongroup_files)]

Fügen Sie als Nächstes den folgenden Code hinzu, um ein **Person**-Objekt für jede Person im Wörterbuch zu erstellen und die Gesichtsdaten aus den entsprechenden Bildern hinzuzufügen. Jedes **Person**-Objekt wird über seine eindeutige ID-Zeichenfolge dem gleichen **PersonGroup**-Objekt zugeordnet. Denken Sie daran, die Variablen `client`, `url` und `RECOGNITION_MODEL1` an diese Methode zu übergeben.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_persongroup_create)]

### <a name="train-persongroup"></a>Trainieren von PersonGroup

Nachdem Sie aus Ihren Bildern Gesichtsdaten extrahiert und in verschiedene **Person**-Objekte eingeordnet haben, müssen Sie das **PersonGroup**-Objekt trainieren, um die visuellen Features zu identifizieren, die den einzelnen **Person**-Objekten zugeordnet sind. Der folgende Code ruft die asynchrone **train**-Methode auf, ruft die Ergebnisse ab und gibt den Status in der Konsole aus.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_persongroup_train)]

Diese **Person**-Gruppe und die ihr zugeordneten **Person**-Objekte sind jetzt für die Verwendung in den Überprüfungs-, Identifizierungs- und Gruppierungsvorgängen bereit.

## <a name="identify-a-face"></a>Identifizieren eines Gesichts

Der Identifizierungsvorgang verwendet ein Bild einer Person (oder mehrerer Personen) und sucht im Bild nach der Identität der einzelnen Gesichter. Er vergleicht jedes erkannte Gesicht mit einem **PersonGroup**-Objekt, einer Datenbank mit verschiedenen **Person**-Objekten,deren Gesichtsmerkmale bekannt sind.

> [!IMPORTANT]
> Damit Sie dieses Beispiel ausführen können, müssen Sie zunächst den Code unter [Erstellen und Trainieren einer Personengruppe](#create-and-train-a-person-group) ausführen. Die in diesem Abschnitt verwendeten Variablen `client`, `url` und `RECOGNITION_MODEL1` müssen hier ebenfalls verfügbar sein.

### <a name="get-a-test-image"></a>Abrufen eines Testbilds

Beachten Sie, dass der Code zum [Erstellen und Trainieren einer Personengruppe](#create-and-train-a-person-group) eine `sourceImageFileName`-Variable definiert. Diese Variable entspricht dem Quellbild, das die zu identifizierenden Personen enthält.

### <a name="identify-faces"></a>Identifizieren von Gesichtern

Der folgende Code nutzt das Quellbild und erstellt eine Liste aller Gesichter, die im Bild erkannt wurden. Dies sind die Gesichter, die anhand des **PersonGroup**-Objekts identifiziert werden.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_identify_sources)]

Der nächste Codeausschnitt ruft den Identifizierungsvorgang auf und gibt die Ergebnisse in der Konsole aus. Hier versucht der Dienst, jedes Gesicht im Quellbild eines **Person**-Objekts im angegebenen **PersonGroup**-Objekt zuzuordnen.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_identify)]

## <a name="take-a-snapshot-for-data-migration"></a>Erstellen einer Momentaufnahme für die Datenmigration

Mit dem Momentaufnahmefeature können Sie Ihre gespeicherten Gesichtserkennungsdaten (wie etwa ein trainiertes **PersonGroup**-Objekt) in ein anderes Abonnement für die Gesichtserkennung von Azure Cognitive Services verschieben. Sie können diese Funktion beispielsweise verwenden, wenn Sie ein **PersonGroup**-Objekt mit einem kostenlosen Testabonnement erstellt haben und dieses nun zu einem kostenpflichtigen Abonnement migrieren möchten. Eine Übersicht über das Momentaufnahmefeature finden Sie unter [Migrieren Ihrer Gesichtserkennungsdaten in ein anderes Abonnement für die Gesichtserkennung](../Face-API-How-to-Topics/how-to-migrate-face-data.md).

In diesem Beispiel migrieren Sie das **PersonGroup**-Objekt, das Sie unter [Erstellen und Trainieren einer Personengruppe](#create-and-train-a-person-group) erstellt haben. Sie können entweder zuerst diesen Abschnitt abschließen oder Ihre eigenen zu migrierenden Konstrukte mit Gesichtserkennungsdaten erstellen.

### <a name="set-up-target-subscription"></a>Einrichten des Zielabonnements

Sie benötigen zunächst ein zweites Azure-Abonnement mit einer Gesichtserkennungsressource. Führen Sie dazu die Schritte im Abschnitt [Einrichten](#setting-up) aus. 

Definieren Sie anschließend die folgenden Variablen in der `Main`-Methode Ihres Programms. Darüber hinaus müssen Sie neue Umgebungsvariablen für die Abonnement-ID Ihres Azure-Kontos sowie den Schlüssel, den Endpunkt und die Abonnement-ID Ihres neuen Kontos (Zielkonto) erstellen. 

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_snapshot_vars)]

Deklarieren Sie in diesem Beispiel eine Variable für die ID des **PersonGroup**-Zielobjekts, das zu dem neuen Abonnement gehört, in das Sie die Daten kopieren.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_snapshot_vars)]

### <a name="authenticate-target-client"></a>Authentifizieren des Zielclients

Fügen Sie als Nächstes den Code hinzu, um Ihr sekundäres Abonnement für die Gesichtserkennung zu authentifizieren.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_snapshot_client)]

### <a name="use-a-snapshot"></a>Verwenden einer Momentaufnahme

Die übrigen Momentaufnahmevorgänge werden innerhalb einer asynchronen Methode ausgeführt. 

1. Der erste Schritt besteht darin, die Momentaufnahme zu **erstellen**. Dadurch werden die Gesichtserkennungsdaten Ihres ursprünglichen Abonnements an einem temporären Cloudspeicherort gespeichert. Diese Methode gibt eine ID zurück, mit der Sie den Status des Vorgangs abfragen.

    [!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_snapshot_take)]

1. Fragen Sie als Nächstes die ID ab, bis der Vorgang abgeschlossen ist.

    [!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_snapshot_take_wait)]

1. Schreiben Sie anschließend mithilfe des Vorgangs **apply** die Gesichtserkennungsdaten in Ihr Zielabonnement. Mit dieser Methode wird ebenfalls ein ID-Wert zurückgegeben.

    [!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_snapshot_apply)]

1. Fragen Sie als Nächstes die neue ID ab, bis der Vorgang abgeschlossen ist.

    [!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_snapshot_apply)]

1. Vervollständigen Sie schließlich den try/catch-Block, und schließen Sie die Methode ab.

    [!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_snapshot_trycatch)]

Nun verfügt Ihr neues **PersonGroup**-Objekt über die gleichen Daten wie das ursprüngliche Objekt, und Sie können von Ihrem neuen Azure-Abonnement für die Gesichtserkennung (Ziel) darauf zugreifen.

## <a name="run-the-application"></a>Ausführen der Anwendung

Führen Sie die Anwendung mit dem Befehl `dotnet run` aus dem Anwendungsverzeichnis aus.

```dotnet
dotnet run
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie ein Cognitive Services-Abonnement bereinigen und entfernen möchten, können Sie die Ressource oder die Ressourcengruppe löschen. Wenn Sie die Ressourcengruppe löschen, werden auch alle anderen Ressourcen gelöscht, die ihr zugeordnet sind.

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure-Befehlszeilenschnittstelle](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

Falls Sie in dieser Schnellstartanleitung ein **PersonGroup**-Objekt erstellt haben und dieses löschen möchten, führen Sie in Ihrem Programm den folgenden Code aus:

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_persongroup_delete)]

Legen Sie die Löschmethode durch den folgenden Code fest:

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_deletepersongroup)]

Wenn Sie außerdem in dieser Schnellstartanleitung Daten mithilfe des Momentaufnahmefeatures migriert haben, müssen Sie auch das im Zielabonnement gespeicherte **PersonGroup**-Objekt löschen.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/Face/Program.cs?name=snippet_target_persongroup_delete)]

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie gelernt, wie Sie die Gesichtserkennungsbibliothek für .NET für allgemeine Aufgaben verwenden. In der Referenzdokumentation finden Sie weitere Informationen zur Bibliothek.

> [!div class="nextstepaction"]
> [Referenz zur Gesichtserkennungs-API (.NET)](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/faceapi?view=azure-dotnet)

* [Was ist die Gesichtserkennungs-API?](../overview.md)
* Den Quellcode für dieses Beispiel finden Sie auf [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/documentation-samples/quickstarts/Face/Program.cs).