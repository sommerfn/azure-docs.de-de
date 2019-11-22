---
title: 'Schnellstart: Content Moderator-Clientbibliothek für .NET | Microsoft-Dokumentation'
titleSuffix: Azure Cognitive Services
description: Hier erhalten Sie Informationen zu den ersten Schritten mit der Content Moderator-Clientbibliothek für .NET.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: quickstart
ms.date: 10/25/2019
ms.author: pafarley
ms.openlocfilehash: 5b287253e471ced244db3169be9f7d8f14e5862e
ms.sourcegitcommit: 38251963cf3b8c9373929e071b50fd9049942b37
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73045221"
---
# <a name="quickstart-content-moderator-client-library-for-net"></a>Schnellstart: Content Moderator-Clientbibliothek für .NET

Hier erhalten Sie Informationen zu den ersten Schritten mit der Content Moderator-Clientbibliothek für .NET. Führen Sie die nachfolgenden Schritte zum Installieren des Pakets aus, und testen Sie den Beispielcode für grundlegende Aufgaben. Content Moderator gehört zu Cognitive Services und überprüft Text-, Bild- und Videoinhalte auf potenziell anstößiges, riskantes oder anderweitig unerwünschtes Material. Gefundenes Material dieser Art wird mithilfe von Flags entsprechend gekennzeichnet. Mit Flags versehene Inhalte können dann von Ihrer App angemessen behandelt werden, um Vorgaben zu erfüllen oder für Benutzer die vorgesehene Umgebung bereitzustellen.

Führen Sie mit der Content Moderator-Clientbibliothek für .NET die folgenden Aktionen aus:

* [Moderieren von Text](#moderate-text)
* [Moderieren von Bildern](#moderate-images)
* [Erstellen einer Überprüfung](#create-a-review)

[Referenzdokumentation](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/contentmoderator?view=azure-dotnet) | [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Vision.ContentModerator) | [Paket (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) | [Beispiele](https://docs.microsoft.com/azure/cognitive-services/content-moderator/samples-dotnet)

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/)
* Aktuelle Version von [.NET Core](https://dotnet.microsoft.com/download/dotnet-core).

## <a name="setting-up"></a>Einrichten

### <a name="create-a-content-moderator-azure-resource"></a>Erstellen einer Content Moderator-Ressource in Azure

Azure Cognitive Services werden von Azure-Ressourcen dargestellt, die Sie abonnieren. Erstellen Sie mithilfe des [Azure-Portals](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) oder der [Azure CLI](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) auf Ihrem lokalen Computer eine Ressource für Content Moderator. Weitere Funktionen:

* Rufen Sie einen kostenlosen [Testschlüssel](https://azure.microsoft.com/try/cognitive-services/#decision) ab, der sieben Tage lang gültig ist. Nach der Registrierung steht dieser auf der [Azure-Website](https://azure.microsoft.com/try/cognitive-services/my-apis/) zur Verfügung.  
* Zeigen Sie Ihre Ressource im [Azure-Portal](https://portal.azure.com/) an.

Nachdem Sie einen Schlüssel für Ihr Testabonnement bzw. Ihre Ressource erhalten haben, [erstellen Sie Umgebungsvariablen](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) für den Schlüssel und eine Endpunkt-URL mit den Namen `CONTENT_MODERATOR_SUBSCRIPTION_KEY` bzw. `CONTENT_MODERATOR_ENDPOINT`.

### <a name="create-a-new-c-application"></a>Erstellen einer neuen C#-Anwendung

Erstellen Sie eine neue .NET Core-Anwendung in Ihrem bevorzugten Text-Editor oder Ihrer bevorzugten IDE. 

Verwenden Sie in einem Konsolenfenster (z. B. cmd, PowerShell oder Bash) den Befehl `dotnet new` zum Erstellen einer neuen Konsolen-App mit dem Namen `content-moderator-quickstart`. Dieser Befehl erstellt ein einfaches „Hallo Welt“-C#-Projekt mit einer einzigen Quelldatei: *Program.cs*.

```console
dotnet new console -n content-moderator-quickstart
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

Öffnen Sie aus dem Projektverzeichnis die Datei *Program.cs* in Ihrem bevorzugten Editor oder Ihrer bevorzugten IDE. Fügen Sie die folgenden `using` -Anweisungen ein:

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ContentModerator/Program.cs?name=snippet_using)]

Erstellen Sie in der **Program**-Klasse Variablen für den Endpunktstandort der Ressource und den Schlüssel als Umgebungsvariable.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ContentModerator/Program.cs?name=snippet_creds)]

> [!NOTE]
> Wenn Sie die Umgebungsvariablen nach dem Start der Anwendung erstellt haben, müssen der Editor, die IDE oder die Shell, in dem bzw. der sie ausgeführt wird, geschlossen und erneut geöffnet werden, damit der Zugriff auf die Variable möglich ist.

### <a name="install-the-client-library"></a>Installieren der Clientbibliothek

Installieren Sie im Anwendungsverzeichnis mit dem folgenden Befehl die Content Moderator-Clientbibliothek für .NET:

```console
dotnet add package Microsoft.Azure.CognitiveServices.ContentModerator --version 2.0.0
```

Bei Verwendung der Visual Studio-IDE ist die Clientbibliothek als herunterladbares NuGet-Paket verfügbar.

## <a name="object-model"></a>Objektmodell

Mit den folgenden Klassen werden einige Hauptfunktionen des Content Moderator .NET SDK verarbeitet.

|NAME|BESCHREIBUNG|
|---|---|
|[ContentModeratorClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.contentmoderator.contentmoderatorclient?view=azure-dotnet)|Diese Klasse wird für alle Content Moderator-Funktionen benötigt. Sie instanziieren sie mit Ihren Abonnementinformationen und verwenden sie zum Generieren von Instanzen anderer Klassen.|
|[ImageModeration](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.contentmoderator.imagemoderation?view=azure-dotnet)|Diese Klasse stellt die Funktionalität zum Analysieren von Bildern auf nicht jugendfreie Inhalte, persönliche Informationen oder menschliche Gesichter bereit.|
|[TextModeration](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.contentmoderator.textmoderation?view=azure-dotnet)|Diese Klasse stellt die Funktionalität zum Analysieren von Text auf Sprache, Obszönitäten, Fehler und persönliche Informationen bereit.|
|[Überprüfungen](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.contentmoderator.reviews?view=azure-dotnet)|Diese Klasse stellt die Funktionalität der Überprüfungs-APIs bereit, einschließlich der Methoden zum Erstellen von Aufträgen, benutzerdefinierten Workflows und Überprüfungen durch Personen.|

## <a name="code-examples"></a>Codebeispiele


Diese Codeausschnitte veranschaulichen, wie die folgenden Aufgaben mit der Content Moderator-Clientbibliothek für .NET durchgeführt werden:

* [Authentifizieren des Clients](#authenticate-the-client)
* [Moderieren von Text](#moderate-text)
* [Moderieren von Bildern](#moderate-images)
* [Erstellen einer Überprüfung](#create-a-review)

## <a name="authenticate-the-client"></a>Authentifizieren des Clients

Instanziieren Sie in einer neuen Methode Clientobjekte mit Ihrem Endpunkt und Schlüssel. Sie benötigen nicht für jedes Szenario einen anderen Client, aber dies kann für die Organisation Ihres Codes hilfreich sein.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ContentModerator/Program.cs?name=snippet_client)]

## <a name="moderate-text"></a>Moderieren von Text

Der folgende Code verwendet einen Content Moderator-Client zum Analysieren von Text und Ausgeben der Ergebnisse in der Konsole. Definieren Sie im Stammverzeichnis Ihrer **Program**-Klasse die Eingabe- und Ausgabedateien:

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ContentModerator/Program.cs?name=snippet_text_vars)]

Fügen Sie dann im Stammverzeichnis Ihres Projekts die Datei *TextFile.txt* hinzu. Fügen Sie dieser Datei Ihren eigenen Text hinzu, oder verwenden Sie den folgenden Beispieltext:

```
Is this a grabage email abcdef@abcd.com, phone: 6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, WA 98052.
Crap is the profanity here. Is this information PII? phone 3144444444
```

Fügen Sie Ihrer `Main`-Methode die folgende Methode hinzu:

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ContentModerator/Program.cs?name=snippet_textmod_call)]

Definieren Sie anschließend in Ihrer **Program**-Klasse die Methode für die Textmoderation:

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ContentModerator/Program.cs?name=snippet_textmod)]

## <a name="moderate-images"></a>Moderieren von Bildern

Im folgenden Code werden ein Content Moderator-Client und ein [ImageModeration](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.contentmoderator.imagemoderation?view=azure-dotnet)-Objekt verwendet, um Remotebilder auf nicht jugendfreie und freizügige Inhalte zu untersuchen.

> [!NOTE]
> Sie können auch den Inhalt eines lokalen Bilds analysieren. Die [Referenzdokumentation](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.contentmoderator.imagemoderation.evaluatefileinputwithhttpmessagesasync?view=azure-dotnet#Microsoft_Azure_CognitiveServices_ContentModerator_ImageModeration_EvaluateFileInputWithHttpMessagesAsync_System_IO_Stream_System_Nullable_System_Boolean__System_Collections_Generic_Dictionary_System_String_System_Collections_Generic_List_System_String___System_Threading_CancellationToken_) enthält eine Beschreibung der Methoden und Vorgänge, die für lokale Bilder geeignet sind.

### <a name="get-sample-images"></a>Abrufen von Beispielbildern

Definieren Sie Ihre Eingabe- und Ausgabedateien:

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ContentModerator/Program.cs?name=snippet_image_vars)]

Erstellen Sie anschließend im Stammverzeichnis Ihres Projekts die Datei *ImageFiles.txt*. In diese Datei fügen Sie die URLs der zu analysierenden Bilder ein (jeweils eine URL pro Zeile). Sie können die folgenden Beispielbilder verwenden:

```
https://moderatorsampleimages.blob.core.windows.net/samples/sample2.jpg
https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png
```

Übergeben Sie Ihre Eingabe- und Ausgabedateien im folgenden Methodenaufruf an die `Main`-Methode. Sie definieren diese Methode in einem späteren Schritt.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ContentModerator/Program.cs?name=snippet_textmod_call)]

### <a name="define-helper-class"></a>Definieren der Hilfsklasse

Fügen Sie die folgende Klassendefinition in der **Program**-Klasse hinzu. Mit dieser inneren Klasse werden Ergebnisse der Bildmoderation verarbeitet.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ContentModerator/Program.cs?name=snippet_dataclass)]

### <a name="define-the-image-moderation-method"></a>Definieren der Methode für die Bildmoderation

Mit der folgenden Methode werden die Bild-URLs in einer Textdatei durchlaufen, es wird eine **EvaluationData**-Instanz erstellt, und das Bild wird auf nicht jugendfreie bzw. freizügige Inhalte, Texte und Gesichter untersucht. Anschließend wird die endgültige **EvaluationData**-Instanz einer Liste hinzugefügt, und die vollständige Liste mit zurückgegebenen Daten wird in die Konsole geschrieben.

#### <a name="iterate-through-image-urls"></a>Durchlaufen von Bild-URLs

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ContentModerator/Program.cs?name=snippet_imagemod_iterate)]

#### <a name="analyze-content"></a>Analysieren von Inhalten

Weitere Informationen zu den Bildattributen, die von Content Moderator untersucht werden, finden Sie im Leitfaden zu [Bildmoderationskonzepten](./image-moderation-api.md).

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ContentModerator/Program.cs?name=snippet_imagemod_analyze)]

#### <a name="write-moderation-results-to-file"></a>Schreiben der Moderationsergebnisse in eine Datei

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ContentModerator/Program.cs?name=snippet_imagemod_save)]

## <a name="create-a-review"></a>Erstellen einer Überprüfung

Mithilfe des Content Moderator .NET SDK können Sie Inhalte in das [Überprüfungstool](https://contentmoderator.cognitive.microsoft.com) eingeben, damit sie von menschlichen Moderatoren überprüft werden können. Weitere Informationen zum Überprüfungstool finden Sie im [konzeptionellen Leitfaden zum Überprüfungstool](./review-tool-user-guide/human-in-the-loop.md).

In der Methode in diesem Abschnitt wird nach dem Eingang menschlicher Eingaben über das Webportal des Überprüfungstools die [Reviews](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.contentmoderator.reviews?view=azure-dotnet)-Klasse verwendet, um eine Überprüfung zu erstellen, die ID abzurufen und die zugehörigen Details zu überprüfen. Alle diese Informationen werden in einer Ausgabetextdatei protokolliert. Rufen Sie die Methode über Ihre `Main`-Methode auf:

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ContentModerator/Program.cs?name=snippet_review_call)]

### <a name="get-sample-images"></a>Abrufen von Beispielbildern

Deklarieren Sie das folgende Array im Stammverzeichnis Ihrer **Program**-Klasse. Diese Variable verweist auf ein Beispielbild für die Erstellung der Überprüfung.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ContentModerator/Program.cs?name=snippet_review_urls)]

### <a name="get-review-credentials"></a>Abrufen von Anmeldeinformationen für die Überprüfung

Melden Sie sich beim [Überprüfungstool](https://contentmoderator.cognitive.microsoft.com) an, und rufen Sie den Teamnamen ab. Weisen Sie ihn dann der entsprechenden Variablen in der **Program**-Klasse zu. Optional können Sie einen Rückrufendpunkt einrichten, um Aktualisierungen für die Aktivität der Überprüfung zu empfangen.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ContentModerator/Program.cs?name=snippet_review_vars)]

### <a name="define-helper-class"></a>Definieren der Hilfsklasse

Fügen Sie die folgende Klassendefinition in Ihrer **Program**-Klasse hinzu. Diese Klasse wird verwendet, um eine einzelne Überprüfungsinstanz darzustellen, die an das Überprüfungstool übermittelt wird.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ContentModerator/Program.cs?name=snippet_review_item)]

### <a name="define-helper-method"></a>Definieren der Hilfsmethode

Fügen Sie der **Program**-Klasse die folgende Methode hinzu. Mit dieser Methode werden die Ergebnisse von Überprüfungsabfragen in die Ausgabetextdatei geschrieben.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ContentModerator/Program.cs?name=snippet_writeline)]

### <a name="define-the-review-creation-method"></a>Definieren der Methode für die Überprüfungserstellung

Nun können Sie die Methode definieren, mit der die Überprüfung erstellt wird und die Abfragen durchgeführt werden. Fügen Sie die neue **CreateReviews**-Methode hinzu, und definieren Sie die folgenden lokalen Variablen.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ContentModerator/Program.cs?name=snippet_createreview_fields)]

#### <a name="post-reviews-to-the-review-tool"></a>Bereitstellen von Überprüfungen für das Überprüfungstool

Fügen Sie anschließend den folgenden Code hinzu, mit dem die jeweiligen Beispielbilder durchlaufen werden. Fügen Sie Metadaten hinzu, und senden Sie sie als einzelnen Batch an das Überprüfungstool. 

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ContentModerator/Program.cs?name=snippet_createreview_create)]

Das vom API-Aufruf zurückgegebene Objekt enthält eindeutige ID-Werte für jedes hochgeladene Bild. Mit dem folgenden Code werden diese IDs analysiert und anschließend verwendet, um über Content Moderator den Status der einzelnen Bilder im Batch abzufragen.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ContentModerator/Program.cs?name=snippet_createreview_ids)]

### <a name="get-review-details"></a>Abrufen von Bewertungsdetails

Der folgende Code bewirkt, dass das Programm auf die Benutzereingabe wartet. Wenn Sie zur Laufzeit zu diesem Schritt gelangen, können Sie selbst zum [Überprüfungstool](https://contentmoderator.cognitive.microsoft.com) wechseln, den erfolgreichen Upload des Beispielbilds überprüfen und die Interaktion damit durchführen. Informationen zur Interaktion mit einer Überprüfung finden Sie unter [Erstellen von Überprüfungen durch Personen](https://docs.microsoft.com/azure/cognitive-services/content-moderator/review-tool-user-guide/review-moderated-images). Nach Abschluss des Vorgangs können Sie eine beliebige Taste drücken, um das Programm fortzusetzen und die Ergebnisse des Überprüfungsprozesses abzurufen.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/documentation-samples/quickstarts/ContentModerator/Program.cs?name=snippet_createreview_results)]

Wenn Sie in diesem Szenario einen Rückrufendpunkt verwendet haben, sollte ein Ereignis im folgenden Format zurückgegeben werden:

```console
{'callback_endpoint': 'https://requestb.in/qmsakwqm',
 'content': '',
 'content_id': '3ebe16cb-31ed-4292-8b71-1dfe9b0e821f',
 'created_by': 'cspythonsdk',
 'metadata': [{'key': 'sc', 'value': 'True'}],
 'review_id': '201901i14682e2afe624fee95ebb248643139e7',
 'reviewer_result_tags': [{'key': 'a', 'value': 'True'},
                          {'key': 'r', 'value': 'True'}],
 'status': 'Complete',
 'sub_team': 'public',
 'type': 'Image'}
```

## <a name="run-the-application"></a>Ausführen der Anwendung

Führen Sie die Anwendung mit dem Befehl `dotnet run` aus dem Anwendungsverzeichnis aus.

```dotnet
dotnet run 
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie ein Cognitive Services-Abonnement bereinigen und entfernen möchten, können Sie die Ressource oder die Ressourcengruppe löschen. Wenn Sie die Ressourcengruppe löschen, werden auch alle anderen Ressourcen gelöscht, die ihr zugeordnet sind.

* [Portal](../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure-Befehlszeilenschnittstelle](../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Nächste Schritte


In dieser Schnellstartanleitung haben Sie gelernt, wie Sie die Content Moderator-.NET-Bibliothek zum Durchführen von Moderationsaufgaben verwenden. Im Anschluss können Sie sich im konzeptionellen Leitfaden ausführlicher über die Moderation von Bildern oder anderen Medien informieren.

> [!div class="nextstepaction"]
> [Lernen von Bildmoderationskonzepten](https://docs.microsoft.com/azure/cognitive-services/content-moderator/image-moderation-api)

* [Was ist Azure Content Moderator?](./overview.md)
* Den Quellcode für dieses Beispiel finden Sie auf [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/documentation-samples/quickstarts/ContentModerator/Program.cs).
