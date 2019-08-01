---
title: 'Schnellstart: Erstellen eines Objekterkennungsprojekts mit dem Custom Vision SDK für Java'
titleSuffix: Azure Cognitive Services
description: Erstellen Sie ein Projekt, fügen Sie Tags hinzu, laden Sie Bilder hoch, trainieren Sie Ihr Projekt, und erkennen Sie Objekte unter Verwendung des Java SDK.
services: cognitive-services
author: areddish
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: quickstart
ms.date: 07/03/2019
ms.author: areddish
ms.openlocfilehash: c7d44efee4f6bf6ec954b015b5f34271cbca5110
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68565507"
---
# <a name="quickstart-create-an-object-detection-project-with-the-custom-vision-sdk-for-java"></a>Schnellstart: Erstellen eines Objekterkennungsprojekts mit dem Custom Vision SDK für Java

Dieser Artikel enthält Informationen und Beispielcode für die ersten Schritte mit dem Custom Vision SDK und Java und unterstützt Sie beim Erstellen eines Objekterkennungsmodells. Nachdem es erstellt wurde, können Sie markierte Bereiche hinzufügen, Bilder hochladen, das Projekt trainieren, die Standardendpunkt-URL für Vorhersagen des Projekts abrufen und den Endpunkt für die programmgesteuerte Überprüfung eines Bilds verwenden. Verwenden Sie dieses Beispiel als Vorlage für die Erstellung Ihrer eigenen Java-Anwendung.

## <a name="prerequisites"></a>Voraussetzungen

- Eine Java-IDE Ihrer Wahl.
- [JDK 7 oder 8](https://aka.ms/azure-jdks) muss installiert sein.
- Maven muss installiert sein.

## <a name="get-the-custom-vision-sdk-and-sample-code"></a>Abrufen von Custom Vision SDK und Beispielcode

Wenn Sie eine Java-App schreiben möchten, die Custom Vision verwendet, benötigen Sie die Maven-Pakete für Custom Vision. Diese Pakete sind in dem Beispielprojekt enthalten, das Sie herunterladen. Sie können hier aber auch einzeln auf sie zugreifen.

Sie können das Custom Vision SDK aus dem zentralen Maven-Repository installieren:
- [Trainings-SDK](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-training)
- [Vorhersage-SDK](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-prediction)

Klonen Sie das Projekt [Cognitive Services Java SDK Samples](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master) (Cognitive Services SDK-Beispiele für Java), oder laden Sie es herunter. Navigieren Sie zum Ordner **Vision/CustomVision/** .

Dieses Java-Projekt erstellt ein neues Custom Vision-Objekterkennungsprojekt namens __Sample Java OD Project__, auf das über die [Custom Vision-Website](https://customvision.ai/) zugegriffen werden kann. Anschließend werden Bilder zum Trainieren und Testen einer Klassifizierung hochgeladen. In diesem Projekt soll die Klassifizierung bestimmen, ob es sich bei einem Baum um eine __Hemlocktanne__ oder um eine __japanische Zierkirsche__ handelt.

[!INCLUDE [get-keys](includes/get-keys.md)]

Das Programm ist so konfiguriert, dass Ihre zentralen Daten als Umgebungsvariablen gespeichert werden. Legen Sie diese Variablen fest, indem Sie in PowerShell zum Ordner **Vision/CustomVision** navigieren. Geben Sie dann die folgenden Befehle ein:

```powershell
$env:AZURE_CUSTOMVISION_TRAINING_API_KEY ="<your training api key>"
$env:AZURE_CUSTOMVISION_PREDICTION_API_KEY ="<your prediction api key>"
```

## <a name="understand-the-code"></a>Grundlegendes zum Code

Laden Sie das Projekt `Vision/CustomVision` in Ihre Java-IDE, und öffnen Sie die Datei _CustomVisionSamples.java_. Suchen Sie nach der Methode **runSample**, und kommentieren Sie den Methodenaufruf **ImageClassification_Sample** aus. Das durch diese Methode ausgeführte Bildklassifizierungsszenario wird in dieser Anleitung nicht behandelt. Die Methode **ObjectDetection_Sample** implementiert die Hauptfunktionen dieser Schnellstartanleitung. Navigieren Sie zur entsprechenden Definition, und sehen Sie sich den Code an. 

### <a name="create-a-new-custom-vision-service-project"></a>Erstellen eines neuen Custom Vision Service-Projekts

Navigieren Sie zu dem Codeblock, der einen Trainingsclient und ein Objekterkennungsprojekt erstellt. Das erstellte Projekt wird auf der [Custom Vision-Website](https://customvision.ai/) angezeigt, die Sie zuvor besucht haben. 

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?range=181-206)]

### <a name="add-tags-to-your-project"></a>Hinzufügen von Kategorien zu Ihrem Projekt

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?range=208-218)]

### <a name="upload-and-tag-images"></a>Hochladen und Kennzeichnen von Bildern

Wenn Sie Bilder in Objekterkennungsprojekten mit Tags versehen, müssen Sie mithilfe normalisierter Koordinaten die Region des jeweiligen markierten Objekts angeben. Navigieren Sie zur Definition der Zuordnung `regionMap`. In diesem Code wird jedem der Beispielbilder die entsprechende markierte Region zugeordnet.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?range=130-179)]

Navigieren Sie als Nächstes zu dem Codeblock, der die Bilder dem Projekt hinzufügt. Die Bilder werden aus dem Ordner **Src/Main/Resources** des Projekts gelesen und zusammen mit den jeweiligen Tags und Regionskoordinaten in den Dienst hochgeladen.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?range=220-231)]

Der vorherige Codeausschnitt verwendet zwei Hilfsfunktionen, die die Bilder als Ressourcenstreams abrufen und in den Dienst hochladen.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?range=277-314)]

### <a name="train-the-project-and-publish"></a>Trainieren des Projekts und Veröffentlichen

Dieser Code erstellt die erste Iteration im Projekt und veröffentlicht anschließend diese Iteration im Vorhersageendpunkt. Der Name der veröffentlichten Iteration kann zum Senden von Vorhersageanforderungen verwendet werden. Eine Iteration ist erst im Vorhersageendpunkt verfügbar, wenn sie veröffentlicht wurde.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?range=233-242)]

### <a name="use-the-prediction-endpoint"></a>Verwenden des Vorhersageendpunkts

Der Vorhersageendpunkt (hier dargestellt durch das Objekt `predictor`) ist die Referenz, die Sie verwenden, um ein Bild an das aktuelle Modell zu übermitteln und eine Klassifizierungsvorhersage zu erhalten. In diesem Beispiel wird `predictor` an anderer Stelle unter Verwendung der Vorhersageschlüssel-Umgebungsvariablen definiert.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?range=244-270)]

## <a name="run-the-application"></a>Ausführen der Anwendung

Führen Sie im Projektverzeichnis in PowerShell den folgenden Befehl aus, um die Lösung zu kompilieren und unter Verwendung von Maven auszuführen:

```powershell
mvn compile exec:java
```

Sehen Sie sich die Protokollierung und die Vorhersageergebnisse in der Konsolenausgabe an. Vergewissern Sie sich, dass das Testbild ordnungsgemäß gekennzeichnet und die Erkennungsregion korrekt ist.

[!INCLUDE [clean-od-project](includes/clean-od-project.md)]

## <a name="next-steps"></a>Nächste Schritte

Sie wissen nun, wie die einzelnen Schritte des Objekterkennungsprozesses im Code ausgeführt werden. In diesem Beispiel wird eine einzelne Trainingsiteration ausgeführt. Zur Verbesserung der Genauigkeit muss ein Modell jedoch häufig mehrmals trainiert und getestet werden. In der folgenden Anleitung wird die Bildklassifizierung behandelt. Die Prinzipien sind jedoch mit denen der Objekterkennung vergleichbar.

> [!div class="nextstepaction"]
> [Testen und erneutes Trainieren eines Modells mit Custom Vision Service](test-your-model.md)
