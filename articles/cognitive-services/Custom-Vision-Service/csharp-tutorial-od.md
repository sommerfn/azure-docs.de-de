---
title: 'Schnellstart: Erstellen eines Objekterkennungsprojekts mit dem Custom Vision SDK für C#'
titleSuffix: Azure Cognitive Services
description: Erstellen Sie ein Projekt, fügen Sie Tags hinzu, laden Sie Bilder hoch, trainieren Sie Ihr Projekt, und erkennen Sie Objekte unter Verwendung des .NET SDK mit C#.
services: cognitive-services
author: areddish
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: quickstart
ms.date: 08/08/2019
ms.author: areddish
ms.openlocfilehash: de8858b1b47e36a4d8b86f317f7471118fa4a772
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "73519487"
---
# <a name="quickstart-create-an-object-detection-project-with-the-custom-vision-net-sdk"></a>Schnellstart: Erstellen eines Objekterkennungsprojekts mit dem Custom Vision SDK für .NET

Dieser Artikel enthält Informationen und Beispielcode für die ersten Schritte mit dem Custom Vision SDK und C# und unterstützt Sie beim Erstellen eines Objekterkennungsmodells. Nachdem es erstellt wurde, können Sie markierte Bereiche hinzufügen, Bilder hochladen, das Projekt trainieren, die Standardendpunkt-URL für Vorhersagen des Projekts abrufen und den Endpunkt für die programmgesteuerte Überprüfung eines Bilds verwenden. Verwenden Sie dieses Beispiel als Vorlage für die Erstellung Ihrer eigenen .NET-Anwendung. 

## <a name="prerequisites"></a>Voraussetzungen

- Eine beliebige Edition von [Visual Studio 2015 oder 2017](https://www.visualstudio.com/downloads/)
- [!INCLUDE [create-resources](includes/create-resources.md)]

## <a name="get-the-custom-vision-sdk-and-sample-code"></a>Abrufen von Custom Vision SDK und Beispielcode

Wenn Sie eine .NET-App schreiben möchten, die Custom Vision verwendet, benötigen Sie die NuGet-Pakete für Custom Vision. Diese Pakete sind in dem Beispielprojekt enthalten, das Sie herunterladen. Sie können hier aber auch einzeln auf sie zugreifen.

- [Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training/)
- [Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction/)

Klonen Sie das Projekt [Cognitive Services .NET Samples](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples) (Cognitive Services-Beispiele für .NET), oder laden Sie es herunter. Navigieren Sie zum Ordner **CustomVision/ObjectDetection**, und öffnen Sie _ObjectDetection.csproj_ in Visual Studio.

Dieses Visual Studio-Projekt erstellt ein neues Custom Vision-Projekt namens __My New Project__, auf das über die [Custom Vision-Website](https://customvision.ai/) zugegriffen werden kann. Anschließend werden Bilder zum Trainieren und Testen eines Objekterkennungsmodells hochgeladen. In diesem Projekt wird das Modell für die Erkennung von Gabeln und Scheren auf Bildern trainiert.

[!INCLUDE [get-keys](includes/get-keys.md)]

## <a name="understand-the-code"></a>Grundlegendes zum Code

Öffnen Sie die Datei _Program.cs_, und sehen Sie sich den Code an. [Erstellen Sie Umgebungsvariablen](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) für Ihre Trainings- und Vorhersageschlüssel mit dem Namen `CUSTOM_VISION_TRAINING_KEY` bzw. `CUSTOM_VISION_PREDICTION_KEY`. Das Skript sucht nach ihnen.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_keys)]

Ermitteln Sie außerdem Ihre Endpunkt-URL über die Seite „Einstellungen“ der Custom Vision-Website. Speichern Sie sie in einer Umgebungsvariablen mit dem Namen `CUSTOM_VISION_ENDPOINT`. Das Skript speichert einen Verweis darauf im Stamm der Klasse.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_endpoint)]

### <a name="create-a-new-custom-vision-service-project"></a>Erstellen eines neuen Custom Vision Service-Projekts

Der nächste Teil des Codes erstellt ein Objekterkennungsprojekt. Das erstellte Projekt wird auf der [Custom Vision-Website](https://customvision.ai/) angezeigt, die Sie zuvor besucht haben. Informationen zur Angabe weiterer Optionen bei der Erstellung Ihres Projekts finden Sie im Artikel zur Methode [CreateProject](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.customvision.training.customvisiontrainingclientextensions.createproject?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Vision_CustomVision_Training_CustomVisionTrainingClientExtensions_CreateProject_Microsoft_Azure_CognitiveServices_Vision_CustomVision_Training_ICustomVisionTrainingClient_System_String_System_String_System_Nullable_System_Guid__System_String_System_Collections_Generic_IList_System_String__). Informationen zur Projekterstellung finden Sie unter [Schnellstart: Informationen zum Erstellen einer Objekterkennung mit Custom Vision](get-started-build-detector.md).  

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_create)]


### <a name="add-tags-to-the-project"></a>Hinzufügen von Tags zum Projekt

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_tags)]

### <a name="upload-and-tag-images"></a>Hochladen und Kennzeichnen von Bildern

Wenn Sie Bilder in Objekterkennungsprojekten mit Tags versehen, müssen Sie mithilfe normalisierter Koordinaten die Region des jeweiligen markierten Objekts angeben. Im folgenden Code wird jedem der Beispielbilder die entsprechende markierte Region zugeordnet.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_upload_regions)]

Auf der Grundlage dieser Zuordnungen werden dann die einzelnen Bilder zusammen mit den jeweiligen Regionskoordinaten hochgeladen. Sie können bis zu 64 Bilder in einem Batch hochladen.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_upload)]

Nun sind alle Bilder hochgeladen, und jedes Bild verfügt über ein Tag für Gabel (**fork**) oder Schere (**scissors**) sowie über ein zugeordnetes Pixelrechteck für das Tag.

### <a name="train-the-project"></a>Trainieren des Projekts

Dieser Code erstellt die erste Trainingsiteration im Projekt.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_train)]

### <a name="publish-the-current-iteration"></a>Veröffentlichen der aktuellen Iteration

Der Name der veröffentlichten Iteration kann zum Senden von Vorhersageanforderungen verwendet werden. Eine Iteration ist erst im Vorhersageendpunkt verfügbar, wenn sie veröffentlicht wurde.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_publish)]

### <a name="create-a-prediction-endpoint"></a>Erstellen eines Vorhersageendpunkts

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_prediction_endpoint)]

### <a name="use-the-prediction-endpoint"></a>Verwenden des Vorhersageendpunkts

Dieser Teil des Skripts lädt das Testbild, fragt den Modellendpunkt ab und gibt Vorhersagedaten an die Konsole aus.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_prediction)]

## <a name="run-the-application"></a>Ausführen der Anwendung

Während der Anwendungsausführung sollte ein Konsolenfenster mit folgender Ausgabe geöffnet werden:

```console
Creating new project:
        Training
Done!

Making a prediction:
        fork: 98.2% [ 0.111609578, 0.184719115, 0.6607002, 0.6637112 ]
        scissors: 1.2% [ 0.112389535, 0.119195729, 0.658031344, 0.7023591 ]
```

Daraufhin können Sie sich vergewissern, dass das Testbild (unter **Images/Test/** ) ordnungsgemäß gekennzeichnet und die Erkennungsregion korrekt ist. Drücken Sie eine beliebige Taste, um die Anwendung zu beenden.

[!INCLUDE [clean-od-project](includes/clean-od-project.md)]

## <a name="next-steps"></a>Nächste Schritte

Sie wissen nun, wie die einzelnen Schritte des Objekterkennungsprozesses im Code ausgeführt werden. In diesem Beispiel wird eine einzelne Trainingsiteration ausgeführt. Zur Verbesserung der Genauigkeit muss ein Modell jedoch häufig mehrmals trainiert und getestet werden. In der folgenden Anleitung wird die Bildklassifizierung behandelt. Die Prinzipien sind jedoch mit denen der Objekterkennung vergleichbar.

> [!div class="nextstepaction"]
> [Testen und erneutes Trainieren eines Modells mit Custom Vision Service](test-your-model.md)
