---
title: 'Schnellstart: Formularerkennungs-Clientbibliothek für .NET | Microsoft-Dokumentation'
description: Führen Sie erste Schritte mit der Formularerkennungs-Clientbibliothek für .NET aus, um eine Ausgabe von strukturierten Daten zu trainieren, zu extrahieren, zu analysieren und abzurufen.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: quickstart
ms.date: 07/12/2019
ms.author: pafarley
ms.openlocfilehash: 6d360939a0196d7e5dc651ecf9a01ef5cbd2f689
ms.sourcegitcommit: bc193bc4df4b85d3f05538b5e7274df2138a4574
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2019
ms.locfileid: "73904604"
---
# <a name="quickstart-form-recognizer-client-library-for-net"></a>Schnellstart: Formularerkennungs-Clientbibliothek für .NET

Erste Schritte mit der Formularerkennungs-Clientbibliothek für .NET. Die Formularerkennung ist ein kognitiver Dienst, der mithilfe von Machine Learning-Technologie Schlüssel/Wert-Paare und Tabellendaten aus Formulardokumenten identifiziert und extrahiert. Anschließend werden strukturierte Daten ausgegeben, die die Beziehungen in der Originaldatei enthalten. Führen Sie die nachfolgenden Schritte zum Installieren des SDK-Pakets aus, und testen Sie den Beispielcode für grundlegende Aufgaben.

Führen Sie mit der Formularerkennungs-Clientbibliothek für .NET die folgenden Aktionen aus:

* [Trainieren eines benutzerdefinierten Formularerkennungsmodells](#train-a-custom-model)
* [Abrufen einer Liste der extrahierten Schlüssel](#get-a-list-of-extracted-keys)
* [Analysieren von Formularen mit einem benutzerdefinierten Modell](#analyze-forms-with-a-custom-model)
* [Abrufen einer Liste mit benutzerdefinierten Modellen](#get-a-list-of-custom-models)
* [Löschen eines benutzerdefinierten Modells](#delete-a-custom-model)

[Referenzdokumentation](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/formrecognizer?view=azure-dotnet-preview) | [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Vision.FormRecognizer) | [Paket (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.FormRecognizer/)

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/)
* Zugriff auf die Vorschauversion der Formularerkennung mit eingeschränktem Zugriff. Um Zugriff auf die Vorschauversion zu erhalten, füllen Sie das [Formular zum Anfordern des Zugriffs auf die Formularerkennung](https://aka.ms/FormRecognizerRequestAccess) aus, und übermitteln Sie es.
* Trainingsdaten in einem Azure Storage-Blob. Unter [Erstellen eines Schulungsdatensatzes für ein benutzerdefiniertes Modell](../build-training-data-set.md) finden Sie Tipps und Optionen für das Zusammenstellen von Trainingsdaten. In dieser Schnellstartanleitung können Sie die Dateien im Ordner **Trainieren** des [Beispieldatasets](https://go.microsoft.com/fwlink/?linkid=2090451) verwenden.
* Aktuelle Version von [.NET Core](https://dotnet.microsoft.com/download/dotnet-core).

## <a name="setting-up"></a>Einrichten

### <a name="create-a-form-recognizer-azure-resource"></a>Erstellen einer Azure-Formularerkennungsressource

[!INCLUDE [create resource](../includes/create-resource.md)]

Nachdem Sie einen Schlüssel für Ihr Testabonnement bzw. Ihre Ressource erhalten haben, [erstellen Sie Umgebungsvariablen](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) für den Schlüssel und den Endpunkt mit dem Namen `FORM_RECOGNIZER_KEY` bzw. `FORM_RECOGNIZER_ENDPOINT`.

### <a name="create-a-new-c-application"></a>Erstellen einer neuen C#-Anwendung

Verwenden Sie in einem Konsolenfenster (z. B. cmd, PowerShell oder Bash) den Befehl `dotnet new` zum Erstellen einer neuen Konsolen-App mit dem Namen `formrecognizer-quickstart`. Dieser Befehl erstellt ein einfaches „Hallo Welt“-C#-Projekt mit einer einzigen Quelldatei: _Program.cs_. 

```console
dotnet new console -n formrecognizer-quickstart
```

Wechseln Sie zum Ordner der neu erstellten App. Erstellen Sie dann die Anwendung mit folgendem Befehl:

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

Öffnen Sie aus dem Projektverzeichnis die Datei _Program.cs_ in Ihrem bevorzugten Editor oder Ihrer bevorzugten IDE. Fügen Sie die folgenden `using` -Anweisungen ein:

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/Program.cs?name=snippet_using)]

Fügen Sie anschließend den folgenden Code in der **Main**-Methode der Anwendung hinzu. Diese asynchrone Aufgabe definieren Sie später.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/Program.cs?name=snippet_main)]

### <a name="install-the-client-library"></a>Installieren der Clientbibliothek

Installieren Sie im Anwendungsverzeichnis mit dem folgenden Befehl die Formularerkennungs-Clientbibliothek für .NET:

```console
dotnet add package Microsoft.Azure.CognitiveServices.FormRecognizer --version 0.8.0-preview
```

Bei Verwendung der Visual Studio-IDE ist die Clientbibliothek als herunterladbares NuGet-Paket verfügbar.

## <a name="object-model"></a>Objektmodell

Die folgenden Klassen verarbeiten die Hauptfunktionen des SDK für die Formularerkennung.

|NAME|BESCHREIBUNG|
|---|---|
|[FormRecognizerClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.formrecognizer.formrecognizerclient?view=azure-dotnet-preview)|Diese Klasse wird für alle Formularerkennungsfunktionen benötigt. Sie instanziieren sie mit Ihren Abonnementinformationen und verwenden sie zum Generieren von Instanzen anderer Klassen.|
|[TrainRequest](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.formrecognizer.models.trainrequest?view=azure-dotnet-preview)| Sie verwenden diese Klasse, um mithilfe Ihrer eigenen Trainingseingabedaten ein benutzerdefiniertes Formularerkennungsmodell zu trainieren. |
|[TrainResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.formrecognizer.models.trainresult?view=azure-dotnet-preview)| Mit dieser werden die Ergebnisse eines Trainingsvorgangs eines benutzerdefinierten Modells angegeben, einschließlich der Modell-ID, mit der Sie anschließend Formulare analysieren können. |
|[AnalyzeResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.formrecognizer.models.analyzeresult?view=azure-dotnet-preview)| Mit dieser Klasse werden die Ergebnisse eines Analysevorgangs eines benutzerdefinierten Modells angegeben. Sie enthält eine Liste mit **ExtractedPage**-Instanzen. |
|[ExtractedPage](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.formrecognizer.models.extractedpage?view=azure-dotnet-preview)| Diese Klasse stellt alle Daten dar, die aus einem einzelnen Formulardokument extrahiert werden.|

## <a name="code-examples"></a>Codebeispiele

<!--
    Include code snippets and short descriptions for each task you list in the the bulleted list. Briefly explain each operation, but include enough clarity to explain complex or otherwise tricky operations.

    Include links to the service's reference content when introducing a class for the first time
-->

Diese Codeausschnitte veranschaulichen, wie die folgenden Aufgaben mit der Formularerkennungs-Clientbibliothek für .NET ausgeführt werden:

* [Authentifizieren des Clients](#authenticate-the-client)
* [Trainieren eines benutzerdefinierten Formularerkennungsmodells](#train-a-custom-model)
* [Abrufen einer Liste der extrahierten Schlüssel](#get-a-list-of-extracted-keys)
* [Analysieren von Formularen mit einem benutzerdefinierten Modell](#analyze-forms-with-a-custom-model)
* [Abrufen einer Liste mit benutzerdefinierten Modellen](#get-a-list-of-custom-models)
* [Löschen eines benutzerdefinierten Modells](#delete-a-custom-model)

## <a name="define-variables"></a>Definieren von Variablen

Fügen Sie vor dem Definieren von Methoden die folgenden Variablendefinitionen am Anfang der **Program**-Klasse hinzu. Sie müssen einige der Variablen selbst ausfüllen. 

* Öffnen Sie zum Abrufen der SAS-URL für Ihre Trainingsdaten den Microsoft Azure Storage-Explorer, klicken Sie mit der rechten Maustaste auf Ihren Container, und wählen Sie **Shared Access Signature abrufen** aus. Stellen Sie sicher, dass die Berechtigungen **Lesen** und **Auflisten**  aktiviert sind, und klicken Sie auf **Erstellen**. Kopieren Sie den Wert im **URL**-Abschnitt. Er muss das Format `https://<storage account>.blob.core.windows.net/<container name>?<SAS value>` aufweisen.
* Wenn Sie ein Beispielformular für die Analyse benötigen, können Sie eine der Dateien im Ordner **Test** des [Beispieldatasets](https://go.microsoft.com/fwlink/?linkid=2090451) verwenden. In diesem Leitfaden werden nur PDF-Formulare verwendet.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/Program.cs?name=snippet_variables)]

## <a name="authenticate-the-client"></a>Authentifizieren des Clients

Definieren Sie unterhalb der Methode `Main` die Aufgabe, auf die in `Main` verwiesen wird. Hier authentifizieren Sie das Clientobjekt mithilfe der oben definierten Abonnementvariablen. Die anderen Methoden definieren Sie später.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/Program.cs?name=snippet_maintask)]

## <a name="train-a-custom-model"></a>Trainieren eines benutzerdefinierten Modells

Bei der folgenden Methode wird das Formularerkennungs-Clientobjekt verwendet, um ein neues Erkennungsmodell für die im Azure-Blobcontainer gespeicherten Dokumente zu trainieren. Mithilfe einer Hilfsmethode werden Informationen zum neu trainierten Modell (dargestellt durch ein Objekt vom Typ [ModelResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.formrecognizer.models.modelresult?view=azure-dotnet-preview)) angezeigt, und die Modell-ID wird zurückgegeben.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/Program.cs?name=snippet_train)]

Die folgende Hilfsmethode zeigt Informationen zu einem Formularerkennungsmodell an.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/Program.cs?name=snippet_displaymodel)]

## <a name="get-a-list-of-extracted-keys"></a>Abrufen einer Liste der extrahierten Schlüssel

Nach Abschluss des Trainings behält das benutzerdefinierte Modell eine Liste der Schlüssel bei, die es aus den Trainingsdokumenten extrahiert hat. Es wird erwartet, dass diese Schlüssel in künftigen Formulardokumenten enthalten sind, und die entsprechenden Werte werden im Analysevorgang extrahiert. Verwenden Sie die folgende Methode, um die Liste der extrahierten Schlüssel abzurufen und sie in der Konsole auszugeben. Dies ist eine gute Möglichkeit, um zu überprüfen, ob der Trainingsprozess wirksam war.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/Program.cs?name=snippet_getkeys)]

## <a name="analyze-forms-with-a-custom-model"></a>Analysieren von Formularen mit einem benutzerdefinierten Modell

Diese Methode verwendet den Formularerkennungsclient und eine Modell-ID, um ein PDF-Formulardokument zu analysieren und Schlüssel-Wert-Daten zu extrahieren. Zum Anzeigen der Ergebnisse (dargestellt durch ein Objekt vom Typ [AnalyzeResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.formrecognizer.models.analyzeresult?view=azure-dotnet-preview)) wird eine Hilfsmethode verwendet.

> [!NOTE]
> Mit der folgenden Methode wird ein PDF-Formular analysiert. Informationen zu ähnlichen Methoden, die JPEG- und PNG-Formulare analysieren, finden Sie im vollständigen Beispielcode auf [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/dotnet/FormRecognizer).

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/Program.cs?name=snippet_analyzepdf)]

Die folgende Hilfsmethode zeigt Informationen zu einem Analysevorgang an:

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/Program.cs?name=snippet_displayanalyze)]

## <a name="get-a-list-of-custom-models"></a>Abrufen einer Liste mit benutzerdefinierten Modellen

Sie können eine Liste aller trainierten Modelle zurückgeben, die zu Ihrem Konto gehören, und Informationen zum Erstellungszeitpunkt abrufen. Die Liste der Modelle wird durch ein Objekt vom Typ [ModelsResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.formrecognizer.models.modelsresult?view=azure-dotnet-preview) dargestellt.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/Program.cs?name=snippet_getmodellist)]

## <a name="delete-a-custom-model"></a>Löschen eines benutzerdefinierten Modells

Wenn Sie das benutzerdefinierte Modell aus Ihrem Konto löschen möchten, verwenden Sie die folgende Methode:

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/FormRecognizer/Program.cs?name=snippet_deletemodel)]

## <a name="run-the-application"></a>Ausführen der Anwendung

Führen Sie die Anwendung aus, indem Sie den Befehl `dotnet run` über das Anwendungsverzeichnis aufrufen.

```console
dotnet run
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie ein Cognitive Services-Abonnement bereinigen und entfernen möchten, können Sie die Ressource oder die Ressourcengruppe löschen. Wenn Sie die Ressourcengruppe löschen, werden auch alle anderen Ressourcen gelöscht, die ihr zugeordnet sind.

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure-Befehlszeilenschnittstelle](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

Wenn Sie ein benutzerdefiniertes Modell trainiert haben, das Sie aus Ihrem Konto löschen möchten, führen Sie außerdem die Methode in [Löschen eines benutzerdefinierten Modells](#delete-a-custom-model) aus.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie die Formularerkennungs-Clientbibliothek für .NET verwendet, um ein benutzerdefiniertes Modell zu trainieren und Formulare zu analysieren. Lesen Sie im Anschluss die Tipps zum Erstellen eines besseren Trainingsdatasets und zum Generieren genauerer Modelle.

> [!div class="nextstepaction"]
>[Erstellen eines Trainingsdatasets](../build-training-data-set.md)

* [Was ist die Formularerkennung?](../overview.md)
* Den Quellcode für dieses Beispiel finden Sie auf [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/dotnet/FormRecognizer).
