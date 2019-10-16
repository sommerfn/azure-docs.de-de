---
title: 'Schnellstart: Analysieren von Textinhalten in C# – Content Moderator'
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie Text unter Verwendung des Content Moderator SDK für .NET auf verschiedene anstößige Inhalte analysieren.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: quickstart
ms.date: 07/03/2019
ms.author: sajagtap
ms.openlocfilehash: 69c190b9ce9e5a99d39a46c147f7159ace47e4f7
ms.sourcegitcommit: be344deef6b37661e2c496f75a6cf14f805d7381
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/07/2019
ms.locfileid: "72001429"
---
# <a name="quickstart-analyze-text-content-for-objectionable-material-in-c"></a>Schnellstart: Analysieren von Text auf anstößige Inhalte in C#

Dieser Artikel enthält Informationen und Codebeispiele, die Ihnen den Einstieg in die Verwendung des [Content Moderator SDK für .NET](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) erleichtern. Sie erfahren, wie Sie eine begriffsbasierte Filterung und Klassifizierung von Textinhalten ausführen, um potenziell anstößige Inhalte zu moderieren.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen. 

## <a name="prerequisites"></a>Voraussetzungen
- Ein Content Moderator-Abonnementschlüssel. Gehen Sie wie unter [Schnellstart: Erstellen eines Cognitive Services-Kontos im Azure-Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) beschrieben vor, um Content Moderator zu abonnieren und Ihren Schlüssel zu erhalten.
- Eine beliebige Edition von [Visual Studio 2015 oder 2017](https://www.visualstudio.com/downloads/).

> [!NOTE]
> In dieser Anleitung wird ein kostenloses Content Moderator-Abonnement verwendet. Weitere Informationen zum Umfang der einzelnen Abonnementtarife finden Sie auf der Seite [Preise und Einschränkungen](https://azure.microsoft.com/pricing/details/cognitive-services/content-moderator/).

## <a name="create-the-visual-studio-project"></a>Erstellen des Visual Studio-Projekts

1. Erstellen Sie in Visual Studio ein neues Projekt vom Typ **Konsolen-App (.NET Framework)** , und nennen Sie es **TextModeration**. 
1. Sollte Ihre Projektmappe noch andere Projekte enthalten, wählen Sie dieses Projekt als das alleinige Startprojekt aus.
1. Rufen Sie das erforderliche NuGet-Paket ab. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf Ihr Projekt, und wählen Sie **NuGet-Pakete verwalten** aus. Suchen und installieren Sie das **Paket `Microsoft.Azure.CognitiveServices.ContentModerator`** . Alternativ können Sie den folgenden Befehl im Projektmappenverzeichnis ausführen:

```console
dotnet add package Microsoft.Azure.CognitiveServices.ContentModerator
```

## <a name="add-text-moderation-code"></a>Hinzufügen von Code für die Textmoderation

Kopieren Sie als nächstes den Code aus dieser Anleitung, und fügen Sie ihn in Ihr Projekt ein, um ein einfaches Inhaltsmoderationsszenario zu implementieren.

### <a name="include-namespaces"></a>Einschließen von Namespaces

Fügen Sie am Anfang der Datei *Program.cs* die folgenden `using`-Anweisungen hinzu:

[!code-csharp[](~/cognitive-services-content-moderator-samples/documentation-samples/csharp/text-moderation-quickstart-dotnet.cs?range=1-8)]

### <a name="create-the-content-moderator-client"></a>Erstellen des Content Moderator-Clients

Fügen Sie der Datei *Program.cs* den folgenden Code hinzu, um einen Content Moderator-Clientanbieter für Ihr Abonnement zu erstellen. Fügen Sie den Code parallel zur Klasse **Program** im gleichen Namespace hinzu. Aktualisieren Sie die Felder **AzureRegion** und **CMSubscriptionKey** mit den Werten Ihres Regionsbezeichners und des Abonnementschlüssels.

[!code-csharp[](~/cognitive-services-content-moderator-samples/documentation-samples/csharp/text-moderation-quickstart-dotnet.cs?range=54-77)]

### <a name="set-up-input-and-output-targets"></a>Einrichten von Ein- und Ausgabezielen

Fügen Sie der Klasse **Program** in _Program.cs_ die folgenden statischen Felder hinzu. Diese Felder dienen zum Angeben der Dateien für die Eingabe (Textinhalte) und die Ausgabe (JSON-Inhalte).

[!code-csharp[](~/cognitive-services-content-moderator-samples/documentation-samples/csharp/text-moderation-quickstart-dotnet.cs?range=15-19)]

Sie müssen die Eingabedatei *TextFile.txt* erstellen und ihren Pfad aktualisieren. (Pfade sind relativ zum Ausführungsverzeichnis.) Öffnen Sie _TextFile.txt_, und fügen Sie den zu moderierenden Text hinzu. In dieser Schnellstartanleitung wird folgender Beispieltext verwendet:

```
Is this a grabage or crap email abcdef@abcd.com, phone: 6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, WA 98052.
These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 
0800 820 3300. Also, 999-99-9999 looks like a social security number (SSN).
```

### <a name="load-the-input-text"></a>Laden des Eingabetexts

Fügen Sie der Methode **Main** den folgenden Code hinzu. Der entscheidende Vorgang ist die Methode **ScreenText**. Die dazugehörigen Parameter geben die auszuführenden Inhaltsmoderationsvorgänge an. In diesem Beispiel wird die Methode für Folgendes konfiguriert:
- Erkennen potenziell anstößiger Ausdrücke im Text
- Normalisieren des Texts und automatisches Korrigieren von Tippfehlern
- Erkennen personenbezogener Informationen wie etwa US- und UK-Telefonnummern, E-Mail-Adressen und US-Postadressen.
- Klassifizieren von Text in drei Kategorien mithilfe Machine Learning-basierter Modelle

Weitere Informationen zur Funktionsweise dieser Vorgänge finden Sie unter dem Link im Abschnitt [Nächste Schritte](#next-steps).

[!code-csharp[](~/cognitive-services-content-moderator-samples/documentation-samples/csharp/text-moderation-quickstart-dotnet.cs?range=23-48)]

## <a name="run-the-program"></a>Ausführen des Programms

Das Programm schreibt JSON-Zeichenfolgendaten in die Datei _TextModerationOutput.txt_. Der in dieser Schnellstartanleitung verwendete Beispieltext führt zu folgender Ausgabe:

```json
Autocorrect typos, check for matching terms, PII, and classify.
{
"OriginalText": "\"Is this a grabage or crap email abcdef@abcd.com, phone: 6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, WA 98052. These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300. Also, 999-99-9999 looks like a social security number (SSN).\"",
"NormalizedText": "\" Is this a garbage or crap email abide@ abed. com, phone: 6657789887, IP: 255. 255. 255. 255, 1 Microsoft Way, Redmond, WA 98052. These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300. Also, 999-99-9999 looks like a social security number ( SSN) . \"",
"AutoCorrectedText": "\" Is this a garbage or crap email abide@ abed. com, phone: 6657789887, IP: 255. 255. 255. 255, 1 Microsoft Way, Redmond, WA 98052. These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300. Also, 999-99-9999 looks like a social security number ( SSN) . \"",
"Misrepresentation": null,
    
"Classification": {
        "Category1": {
        "Score": 1.5113095059859916E-06
        },
        "Category2": {
        "Score": 0.12747249007225037
        },
        "Category3": {
        "Score": 0.98799997568130493
        },
        "ReviewRecommended": true
  },
  "Status": {
    "Code": 3000,
    "Description": "OK",
    "Exception": null
  },
  "PII": {
        "Email": [
            {
            "Detected": "abcdef@abcd.com",
            "SubType": "Regular",
            "Text": "abcdef@abcd.com",
            "Index": 33
            }
            ],
        "IPA": [
            {
            "SubType": "IPV4",
            "Text": "255.255.255.255",
            "Index": 73
            }
            ],
        "Phone": [
            {
            "CountryCode": "US",
            "Text": "6657789887",
            "Index": 57
            },
            {
            "CountryCode": "US",
            "Text": "870 608 4000",
            "Index": 211
            },
            {
            "CountryCode": "UK",
            "Text": "+44 870 608 4000",
            "Index": 207
            },
            {
            "CountryCode": "UK",
            "Text": "0344 800 2400",
            "Index": 227
            },
            {
        "CountryCode": "UK",
            "Text": "0800 820 3300",
            "Index": 244
            }
            ],
         "Address": [{
             "Text": "1 Microsoft Way, Redmond, WA 98052",
             "Index": 89
                }]
    },
  "Language": "eng",
  "Terms": [
    {
        "Index": 22,
        "OriginalIndex": 22,
        "ListId": 0,
        "Term": "crap"
    }
  ],
  "TrackingId": "9392c53c-d11a-441d-a874-eb2b93d978d3"
}
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie eine einfache .NET-Anwendung entwickelt, die unter Verwendung des Content Moderator-Diensts relevante Informationen zu einem bestimmten Textbeispiel zurückgibt. Informieren Sie sich als Nächstes über die Bedeutung der verschiedenen Flags und Klassifizierungen, um entscheiden zu können, welche Daten Sie benötigen und wie Ihre App mit ihnen umgehen soll.

> [!div class="nextstepaction"]
> [Textmoderation](text-moderation-api.md)
