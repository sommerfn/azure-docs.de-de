---
title: 'Schnellstart: Analysieren eines Bilds – SDK, C#'
titleSuffix: Azure Cognitive Services
description: In diesem Schnellstart analysieren Sie ein Bild mithilfe der Windows-C#-Clientbibliothek für Maschinelles Sehen.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 07/03/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 59641973a953d1db501f7b660b04df900bc55cb0
ms.sourcegitcommit: d200cd7f4de113291fbd57e573ada042a393e545
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2019
ms.locfileid: "70141034"
---
# <a name="quickstart-analyze-an-image-using-the-computer-vision-sdk-and-c"></a>Schnellstart: Analysieren eines Bilds mit dem SDK für maschinelles Sehen und C#

In dieser Schnellstartanleitung analysieren Sie mithilfe der Maschinelles Sehen-Clientbibliothek für C# ein lokales Bild und ein Remotebild, um visuelle Merkmale zu extrahieren. Auf Wunsch können Sie den Code in dieser Anleitung als vollständige Beispiel-App aus dem GitHub-Repository [Cognitive Services Csharp Vision](https://github.com/Azure-Samples/cognitive-services-vision-csharp-sdk-quickstarts/tree/master/ComputerVision) herunterladen.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Schlüssel für ein Maschinelles Sehen-Abonnement. Über die Seite [Cognitive Services ausprobieren](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision) können Sie einen Abonnementschlüssel für eine kostenlose Testversion abrufen. Alternativ gehen Sie wie unter [Schnellstart: Erstellen eines Cognitive Services-Kontos im Azure-Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) beschrieben vor, um „Maschinelles Sehen“ zu abonnieren und Ihren Schlüssel zu erhalten. [Erstellen Sie dann Umgebungsvariablen](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) für die Schlüssel- und Dienstendpunkt-Zeichenfolge, und nennen Sie sie `COMPUTER_VISION_SUBSCRIPTION_KEY` bzw. `COMPUTER_VISION_ENDPOINT`.
* Eine beliebige Edition von [Visual Studio 2015 oder 2017](https://www.visualstudio.com/downloads/).
* Das NuGet-Paket [Microsoft.Azure.CognitiveServices.Vision.ComputerVision](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.ComputerVision) mit der Clientbibliothek. Es ist nicht erforderlich, das Paket herunterzuladen. Unten ist die Installationsanleitung angegeben.

## <a name="create-and-run-the-sample-application"></a>Erstellen und Ausführen der Beispielanwendung

Führen Sie zum Ausführen des Beispiels die folgenden Schritte aus:

1. Erstellen Sie in Visual Studio eine neue Visual C#-Konsolen-App.
1. Installieren Sie das NuGet-Paket mit der Clientbibliothek der Maschinelles Sehen-API.
    1. Klicken Sie im Menü auf **Werkzeuge**, wählen Sie **NuGet-Paket-Manager** und dann **NuGet-Pakete für Projektmappe verwalten** aus.
    1. Klicken Sie auf die Registerkarte **Durchsuchen**, und geben Sie „Microsoft.Azure.CognitiveServices.Vision.ComputerVision“ in das Feld **Suchen** ein.
    1. Wählen Sie **Microsoft.Azure.CognitiveServices.Vision.ComputerVision** aus, wenn das Paket angezeigt wird, aktivieren Sie das Kontrollkästchen neben dem Projektnamen, und klicken Sie dann auf **Installieren**.
1. Ersetzen Sie den Inhalt von *Program.cs* durch den folgenden Code. Die Methoden `AnalyzeImageAsync` und `AnalyzeImageInStreamAsync` umschließen die [Bildanalyse-REST-API](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) für lokale Bilder bzw. Remotebilder.

    ```csharp
    using Microsoft.Azure.CognitiveServices.Vision.ComputerVision;
    using Microsoft.Azure.CognitiveServices.Vision.ComputerVision.Models;

    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Threading.Tasks;

    namespace ImageAnalyze
    {
        class Program
        {
            // subscriptionKey = "0123456789abcdef0123456789ABCDEF"
            private const string subscriptionKey = "<SubscriptionKey>";

            // localImagePath = @"C:\Documents\LocalImage.jpg"
            private const string localImagePath = @"<LocalImage>";

            private const string remoteImageUrl =
                "https://upload.wikimedia.org/wikipedia/commons/3/3c/Shaki_waterfall.jpg";

            // Specify the features to return
            private static readonly List<VisualFeatureTypes> features =
                new List<VisualFeatureTypes>()
            {
                VisualFeatureTypes.Categories, VisualFeatureTypes.Description,
                VisualFeatureTypes.Faces, VisualFeatureTypes.ImageType,
                VisualFeatureTypes.Tags
            };

            static void Main(string[] args)
            {
                ComputerVisionClient computerVision = new ComputerVisionClient(
                    new ApiKeyServiceClientCredentials(subscriptionKey),
                    new System.Net.Http.DelegatingHandler[] { });

                // You must use the same region as you used to get your subscription
                // keys. For example, if you got your subscription keys from westus,
                // replace "westcentralus" with "westus".
                //
                // Free trial subscription keys are generated in the "westus"
                // region. If you use a free trial subscription key, you shouldn't
                // need to change the region.

                // Specify the Azure region
                computerVision.Endpoint = "https://westcentralus.api.cognitive.microsoft.com";

                Console.WriteLine("Images being analyzed ...");
                var t1 = AnalyzeRemoteAsync(computerVision, remoteImageUrl);
                var t2 = AnalyzeLocalAsync(computerVision, localImagePath);

                Task.WhenAll(t1, t2).Wait(5000);
                Console.WriteLine("Press ENTER to exit");
                Console.ReadLine();
            }

            // Analyze a remote image
            private static async Task AnalyzeRemoteAsync(
                ComputerVisionClient computerVision, string imageUrl)
            {
                if (!Uri.IsWellFormedUriString(imageUrl, UriKind.Absolute))
                {
                    Console.WriteLine(
                        "\nInvalid remoteImageUrl:\n{0} \n", imageUrl);
                    return;
                }

                ImageAnalysis analysis =
                    await computerVision.AnalyzeImageAsync(imageUrl, features);
                DisplayResults(analysis, imageUrl);
            }

            // Analyze a local image
            private static async Task AnalyzeLocalAsync(
                ComputerVisionClient computerVision, string imagePath)
            {
                if (!File.Exists(imagePath))
                {
                    Console.WriteLine(
                        "\nUnable to open or read localImagePath:\n{0} \n", imagePath);
                    return;
                }

                using (Stream imageStream = File.OpenRead(imagePath))
                {
                    ImageAnalysis analysis = await computerVision.AnalyzeImageInStreamAsync(
                        imageStream, features);
                    DisplayResults(analysis, imagePath);
                }
            }

            // Display the most relevant caption for the image
            private static void DisplayResults(ImageAnalysis analysis, string imageUri)
            {
                Console.WriteLine(imageUri);
                if (analysis.Description.Captions.Count != 0)
                {
                    Console.WriteLine(analysis.Description.Captions[0].Text + "\n");
                }
                else
                {
                    Console.WriteLine("No description generated.");
                }
            }
        }
    }
    ```

1. Ersetzen Sie `<Subscription Key>` durch Ihren gültigen Abonnementschlüssel.
1. Ändern Sie `computerVision.Endpoint` bei Bedarf in die Azure-Region, die Ihren Abonnementschlüsseln zugeordnet ist, wenn erforderlich.
1. Ersetzen Sie `<LocalImage>` durch den Pfad und Dateinamen eines lokalen Bilds.
1. Legen Sie optional `remoteImageUrl` auf eine andere Bild-URL fest.
1. Führen Sie das Programm aus.

## <a name="examine-the-response"></a>Untersuchen der Antwort

In einer erfolgreichen Antwort werden die relevantesten Titel für jedes Bild angezeigt. Die Methode `DisplayResults` kann geändert werden, um andere Bilddaten auszugeben. Weitere Informationen finden Sie unter der Methode [AnalyzeLocalAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.computervision.computervisionclientextensions.analyzeimageinstreamasync?view=azure-dotnet).

Unter [API-Schnellstartanleitungen: Analysieren von lokalen Bildern mit C#](../QuickStarts/CSharp-analyze.md#examine-the-response) finden Sie ein Beispiel für eine unformatierte JSON-Ausgabe.

```console
https://upload.wikimedia.org/wikipedia/commons/3/3c/Shaki_waterfall.jpg
a large waterfall over a rocky cliff
```

## <a name="next-steps"></a>Nächste Schritte

Erkunden Sie die Maschinelles Sehen-APIs, die zum Analysieren von Bildern, Erkennen von Prominenten und Sehenswürdigkeiten, Erstellen von Miniaturansichten und Extrahieren von gedrucktem sowie handschriftlichem Text verwendet werden.

> [!div class="nextstepaction"]
> [Maschinelles Sehen-APIs erkunden](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
