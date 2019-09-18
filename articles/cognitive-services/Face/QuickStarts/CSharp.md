---
title: 'Schnellstart: Erkennen von Gesichtern in einem Bild mit der Azure-REST-API und C#'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung verwenden Sie die Azure-Gesichtserkennungs-REST-API mit C#, um Gesichter in einem Bild zu erkennen.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 09/06/2019
ms.author: pafarley
ms.openlocfilehash: f7e1507289d3c21d51a0ec8529598e5eeb089d37
ms.sourcegitcommit: 65131f6188a02efe1704d92f0fd473b21c760d08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2019
ms.locfileid: "70859011"
---
# <a name="quickstart-detect-faces-in-an-image-using-the-face-rest-api-and-c"></a>Schnellstart: Erkennen von Gesichtern in einem Bild mit der Gesichtserkennungs-REST-API und C#

In dieser Schnellstartanleitung verwenden Sie die Azure-Gesichtserkennungs-REST-API mit C#, um menschliche Gesichter in einem Bild zu erkennen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen. 

## <a name="prerequisites"></a>Voraussetzungen

- Ein Abonnementschlüssel für die Gesichtserkennungs-API. Über die Seite [Cognitive Services ausprobieren](https://azure.microsoft.com/try/cognitive-services/?api=face-api) können Sie einen Abonnementschlüssel für eine kostenlose Testversion abrufen. Gehen Sie andernfalls wie unter [Schnellstart: Erstellen eines Cognitive Services-Kontos im Azure-Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) beschrieben vor, um den Gesichtserkennungs-API-Dienst zu abonnieren und Ihren Schlüssel zu erhalten.
- Eine beliebige Edition von [Visual Studio 2015 oder 2017](https://www.visualstudio.com/downloads/).

## <a name="create-the-visual-studio-project"></a>Erstellen des Visual Studio-Projekts

1. Erstellen Sie in Visual Studio ein neues Projekt vom Typ **Konsolen-App (.NET Framework)** , und nennen Sie es **FaceDetection**. 
1. Sollte Ihre Projektmappe noch andere Projekte enthalten, wählen Sie dieses Projekt als das alleinige Startprojekt aus.

## <a name="add-face-detection-code"></a>Hinzufügen von Code für die Gesichtserkennung

Öffnen Sie die Datei *Program.cs* des neuen Projekts. Hier fügen Sie den Code hinzu, der zum Laden von Bildern und Erkennen von Gesichtern erforderlich ist.

### <a name="include-namespaces"></a>Einschließen von Namespaces

Fügen Sie am Anfang der Datei *Program.cs* die folgenden `using`-Anweisungen hinzu:

```csharp
using System;
using System.IO;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;
```

### <a name="add-essential-fields"></a>Hinzufügen wichtiger Felder

Fügen Sie die **Program**-Klasse mit den folgenden Felder hinzu. Mit diesen Daten wird angegeben, wie eine Verbindung mit dem Gesichtserkennungsdienst hergestellt wird und wo die Eingabedaten abgerufen werden. Sie müssen das Feld `subscriptionKey` mit dem Wert Ihres Abonnementschlüssels aktualisieren. Unter Umständen müssen Sie die Zeichenfolge `uriBase` ändern, sodass sie die Zeichenfolge des Ressourcenendpunkts enthält.

[!INCLUDE [subdomains-note](../../../../includes/cognitive-services-custom-subdomains-note.md)]

```csharp
namespace DetectFace
{
    class Program
    {

        // Replace <Subscription Key> with your valid subscription key.
        const string subscriptionKey = "<Subscription Key>";
        
        // replace <myresourcename> with the string found in your endpoint URL
        const string uriBase =
            "https://<myresourcename>.cognitive.microsoft.com/face/v1.0/detect";
```

### <a name="receive-image-input"></a>Empfangen der Bildeingabe

Fügen Sie der **Main**-Methode der **Program**-Klasse den folgenden Code hinzu. Durch diesen Code wird der Benutzer über die Konsole aufgefordert, eine Bild-URL einzugeben. Anschließend wird eine weitere Methode (**MakeAnalysisRequest**) aufgerufen, um das Bild an diesem Speicherort zu verarbeiten.

```csharp
        static void Main(string[] args)
        {

            // Get the path and filename to process from the user.
            Console.WriteLine("Detect faces:");
            Console.Write(
                "Enter the path to an image with faces that you wish to analyze: ");
            string imageFilePath = Console.ReadLine();

            if (File.Exists(imageFilePath))
            {
                try
                {
                    MakeAnalysisRequest(imageFilePath);
                    Console.WriteLine("\nWait a moment for the results to appear.\n");
                }
                catch (Exception e)
                {
                    Console.WriteLine("\n" + e.Message + "\nPress Enter to exit...\n");
                }
            }
            else
            {
                Console.WriteLine("\nInvalid file path.\nPress Enter to exit...\n");
            }
            Console.ReadLine();
        }
```

### <a name="call-the-face-detection-rest-api"></a>Aufrufen der Gesichtserkennungs-REST-API

Fügen Sie der **Program**-Klasse die folgende Methode hinzu. Diese Methode erstellt einen REST-Aufruf der Gesichtserkennungs-API, um Gesichtsinformationen im Remotebild zu erkennen. (Die Zeichenfolge `requestParameters` gibt an, welche Gesichtsattribute abgerufen werden sollen.) Anschließend werden die Ausgabedaten in eine JSON-Zeichenfolge geschrieben.

Sie definieren die Hilfsmethoden in den folgenden Schritten.

```csharp
        // Gets the analysis of the specified image by using the Face REST API.
        static async void MakeAnalysisRequest(string imageFilePath)
        {
            HttpClient client = new HttpClient();

            // Request headers.
            client.DefaultRequestHeaders.Add(
                "Ocp-Apim-Subscription-Key", subscriptionKey);

            // Request parameters. A third optional parameter is "details".
            string requestParameters = "returnFaceId=true&returnFaceLandmarks=false" +
                "&returnFaceAttributes=age,gender,headPose,smile,facialHair,glasses," +
                "emotion,hair,makeup,occlusion,accessories,blur,exposure,noise";

            // Assemble the URI for the REST API Call.
            string uri = uriBase + "?" + requestParameters;

            HttpResponseMessage response;

            // Request body. Posts a locally stored JPEG image.
            byte[] byteData = GetImageAsByteArray(imageFilePath);

            using (ByteArrayContent content = new ByteArrayContent(byteData))
            {
                // This example uses content type "application/octet-stream".
                // The other content types you can use are "application/json"
                // and "multipart/form-data".
                content.Headers.ContentType =
                    new MediaTypeHeaderValue("application/octet-stream");

                // Execute the REST API call.
                response = await client.PostAsync(uri, content);

                // Get the JSON response.
                string contentString = await response.Content.ReadAsStringAsync();

                // Display the JSON response.
                Console.WriteLine("\nResponse:\n");
                Console.WriteLine(JsonPrettyPrint(contentString));
                Console.WriteLine("\nPress Enter to exit...");
            }
        }
```

### <a name="process-the-input-image-data"></a>Verarbeiten der Eingabebilddaten

Fügen Sie der **Program**-Klasse die folgende Methode hinzu. Durch diese Methode wird das Bild unter der angegebenen URL in ein Bytearray konvertiert.

```csharp
        // Returns the contents of the specified file as a byte array.
        static byte[] GetImageAsByteArray(string imageFilePath)
        {
            using (FileStream fileStream =
                new FileStream(imageFilePath, FileMode.Open, FileAccess.Read))
            {
                BinaryReader binaryReader = new BinaryReader(fileStream);
                return binaryReader.ReadBytes((int)fileStream.Length);
            }
        }
```

### <a name="parse-the-json-response"></a>Analysieren der JSON-Antwort

Fügen Sie der **Program**-Klasse die folgende Methode hinzu. Durch diese Methode wird die JSON-Antwort formatiert, damit sie besser lesbar ist. Ihre App schreibt diese Zeichenfolgendaten in die Konsole. Anschließend können Sie die Klasse und den Namespace schließen.

```csharp
        // Formats the given JSON string by adding line breaks and indents.
        static string JsonPrettyPrint(string json)
        {
            if (string.IsNullOrEmpty(json))
                return string.Empty;

            json = json.Replace(Environment.NewLine, "").Replace("\t", "");

            StringBuilder sb = new StringBuilder();
            bool quote = false;
            bool ignore = false;
            int offset = 0;
            int indentLength = 3;

            foreach (char ch in json)
            {
                switch (ch)
                {
                    case '"':
                        if (!ignore) quote = !quote;
                        break;
                    case '\'':
                        if (quote) ignore = !ignore;
                        break;
                }

                if (quote)
                    sb.Append(ch);
                else
                {
                    switch (ch)
                    {
                        case '{':
                        case '[':
                            sb.Append(ch);
                            sb.Append(Environment.NewLine);
                            sb.Append(new string(' ', ++offset * indentLength));
                            break;
                        case '}':
                        case ']':
                            sb.Append(Environment.NewLine);
                            sb.Append(new string(' ', --offset * indentLength));
                            sb.Append(ch);
                            break;
                        case ',':
                            sb.Append(ch);
                            sb.Append(Environment.NewLine);
                            sb.Append(new string(' ', offset * indentLength));
                            break;
                        case ':':
                            sb.Append(ch);
                            sb.Append(' ');
                            break;
                        default:
                            if (ch != ' ') sb.Append(ch);
                            break;
                    }
                }
            }

            return sb.ToString().Trim();
        }
    }
}
```

## <a name="run-the-app"></a>Ausführen der App

Eine erfolgreiche Antwort zeigt Gesichtserkennungsdaten in leicht lesbarem JSON-Format an. Beispiel:

```json
[
   {
      "faceId": "f7eda569-4603-44b4-8add-cd73c6dec644",
      "faceRectangle": {
         "top": 131,
         "left": 177,
         "width": 162,
         "height": 162
      },
      "faceAttributes": {
         "smile": 0.0,
         "headPose": {
            "pitch": 0.0,
            "roll": 0.1,
            "yaw": -32.9
         },
         "gender": "female",
         "age": 22.9,
         "facialHair": {
            "moustache": 0.0,
            "beard": 0.0,
            "sideburns": 0.0
         },
         "glasses": "NoGlasses",
         "emotion": {
            "anger": 0.0,
            "contempt": 0.0,
            "disgust": 0.0,
            "fear": 0.0,
            "happiness": 0.0,
            "neutral": 0.986,
            "sadness": 0.009,
            "surprise": 0.005
         },
         "blur": {
            "blurLevel": "low",
            "value": 0.06
         },
         "exposure": {
            "exposureLevel": "goodExposure",
            "value": 0.67
         },
         "noise": {
            "noiseLevel": "low",
            "value": 0.0
         },
         "makeup": {
            "eyeMakeup": true,
            "lipMakeup": true
         },
         "accessories": [

         ],
         "occlusion": {
            "foreheadOccluded": false,
            "eyeOccluded": false,
            "mouthOccluded": false
         },
         "hair": {
            "bald": 0.0,
            "invisible": false,
            "hairColor": [
               {
                  "color": "brown",
                  "confidence": 1.0
               },
               {
                  "color": "black",
                  "confidence": 0.87
               },
               {
                  "color": "other",
                  "confidence": 0.51
               },
               {
                  "color": "blond",
                  "confidence": 0.08
               },
               {
                  "color": "red",
                  "confidence": 0.08
               },
               {
                  "color": "gray",
                  "confidence": 0.02
               }
            ]
         }
      }
   }
]
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie eine einfache .NET-Konsolenanwendung erstellt, die REST-Aufrufe mit der Azure-Gesichtserkennungs-API verwendet, um Gesichter in einem Bild zu erkennen und ihre Attribute zurückzugeben. Sehen Sie sich als Nächstes die Referenzdokumentation zur Gesichtserkennungs-API an, um mehr über die unterstützten Szenarien zu erfahren.

> [!div class="nextstepaction"]
> [Gesichtserkennungs-API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
