---
title: 'Schnellstart: Erstellen eines Bildklassifizierungsprojekts mit dem Custom Vision SDK für Go'
titleSuffix: Azure Cognitive Services
description: Erstellen Sie ein Projekt, fügen Sie Tags hinzu, laden Sie Bilder hoch, trainieren Sie Ihr Projekt, und machen Sie eine Vorhersage unter Verwendung des Go SDK.
services: cognitive-services
author: areddish
manager: daauld
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: quickstart
ms.date: 08/08/2019
ms.author: areddish
ms.openlocfilehash: 7f78230028b66fb12e52f7d5e96cbecbfe8b99a3
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "73519484"
---
# <a name="quickstart-create-an-image-classification-project-with-the-custom-vision-go-sdk"></a>Schnellstart: Erstellen eines Bildklassifizierungsprojekts mit dem Custom Vision SDK für Go

Dieser Artikel enthält Informationen und Beispielcode für die ersten Schritte mit dem Custom Vision SDK für Go und unterstützt Sie beim Erstellen eines Bildklassifizierungsmodells. Nach der Erstellung können Sie Tags hinzufügen, Bilder hochladen, das Projekt trainieren, die veröffentlichte Endpunkt-URL für Vorhersagen des Projekts abrufen und den Endpunkt für die programmgesteuerte Überprüfung eines Bilds verwenden. Verwenden Sie dieses Beispiel als Vorlage für die Erstellung Ihrer eigenen Go-Anwendung. Falls Sie den Prozess zur Erstellung und Verwendung eines Klassifizierungsmodells _ohne_ Code durchlaufen möchten, sehen Sie sich stattdessen die [browserbasierte Anleitung](getting-started-build-a-classifier.md) an.

## <a name="prerequisites"></a>Voraussetzungen

- [Go 1.8+](https://golang.org/doc/install)
- [!INCLUDE [create-resources](includes/create-resources.md)]

## <a name="install-the-custom-vision-sdk"></a>Installieren des Custom Vision SDK

Führen Sie in PowerShell den folgenden Befehl aus, um das Custom Vision Service SDK für Go zu installieren:

```shell
go get -u github.com/Azure/azure-sdk-for-go/...
```

Bei Verwendung von `dep` können Sie alternativ Folgendes in Ihrem Repository ausführen:
```shell
dep ensure -add github.com/Azure/azure-sdk-for-go
```

[!INCLUDE [get-keys](includes/get-keys.md)]

[!INCLUDE [python-get-images](includes/python-get-images.md)]

## <a name="add-the-code"></a>Hinzufügen des Codes

Erstellen Sie in Ihrem bevorzugten Projektverzeichnis eine neue Datei namens *sample.go*.

### <a name="create-the-custom-vision-service-project"></a>Erstellen des Custom Vision Service-Projekts

Fügen Sie Ihrem Skript den folgenden Code hinzu, um ein neues Custom Vision Service-Projekt zu erstellen. Fügen Sie Ihre Abonnementschlüssel in die entsprechenden Definitionen ein. Sie können auch Ihre Endpunkt-URL über die Seite „Einstellungen“ auf der Custom Vision-Website aufrufen.

Informationen zur Angabe weiterer Optionen bei der Erstellung Ihres Projekts finden Sie im Artikel zur Methode [CreateProject](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.customvision.training.trainings.createproject?view=azure-java-stable#com_microsoft_azure_cognitiveservices_vision_customvision_training_Trainings_createProject_String_CreateProjectOptionalParameter_). Informationen zur Projekterstellung finden Sie unter [Schnellstart: Erstellen einer Klassifizierung mit Custom Vision](getting-started-build-a-classifier.md).

```go
import(
    "context"
    "bytes"
    "fmt"
    "io/ioutil"
    "path"
    "log"
    "time"
    "github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v3.0/customvision/training"
    "github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v3.0/customvision/prediction"
)

var (
    training_key string = "<your training key>"
    prediction_key string = "<your prediction key>"
    prediction_resource_id = "<your prediction resource id>"
    endpoint string = "<your endpoint URL>"
    project_name string = "Go Sample Project"
    iteration_publish_name = "classifyModel"
    sampleDataDirectory = "<path to sample images>"
)

func main() {
    fmt.Println("Creating project...")

    ctx = context.Background()

    trainer := training.New(training_key, endpoint)

    project, err := trainer.CreateProject(ctx, project_name, "sample project", nil, string(training.Multilabel))
    if (err != nil) {
        log.Fatal(err)
    }
```

### <a name="create-tags-in-the-project"></a>Erstellen von Tags im Projekt

Fügen Sie am Ende der Datei *sample.go* den folgenden Code hinzu, um Ihrem Projekt Klassifizierungstags hinzuzufügen:

```go
// Make two tags in the new project
hemlockTag, _ := trainer.CreateTag(ctx, *project.ID, "Hemlock", "Hemlock tree tag", string(training.Regular))
cherryTag, _ := trainer.CreateTag(ctx, *project.ID, "Japanese Cherry", "Japanese cherry tree tag", string(training.Regular))
```

### <a name="upload-and-tag-images"></a>Hochladen und Kennzeichnen von Bildern

Um dem Projekt die Beispielbilder hinzuzufügen, fügen Sie nach der Erstellung der Kategorien den folgenden Code ein. Dieser Code lädt das Bild mit dem entsprechenden Tag hoch. Sie können bis zu 64 Bilder in einem Batch hochladen.

> [!NOTE]
> Sie müssen den Pfad zu den Bildern basierend darauf ändern, wo Sie zuvor das Cognitive Services SDK-Beispielprojekt für Go heruntergeladen haben.

```go
fmt.Println("Adding images...")
japaneseCherryImages, err := ioutil.ReadDir(path.Join(sampleDataDirectory, "Japanese Cherry"))
if err != nil {
    fmt.Println("Error finding Sample images")
}

hemLockImages, err := ioutil.ReadDir(path.Join(sampleDataDirectory, "Hemlock"))
if err != nil {
    fmt.Println("Error finding Sample images")
}

for _, file := range hemLockImages {
    imageFile, _ := ioutil.ReadFile(path.Join(sampleDataDirectory, "Hemlock", file.Name()))
    imageData := ioutil.NopCloser(bytes.NewReader(imageFile))

    trainer.CreateImagesFromData(ctx, *project.ID, imageData, []string{ hemlockTag.ID.String() })
}

for _, file := range japaneseCherryImages {
    imageFile, _ := ioutil.ReadFile(path.Join(sampleDataDirectory, "Japanese Cherry", file.Name()))
    imageData := ioutil.NopCloser(bytes.NewReader(imageFile))
    trainer.CreateImagesFromData(ctx, *project.ID, imageData, []string{ cherryTag.ID.String() })
}
```

### <a name="train-the-classifier-and-publish"></a>Trainieren der Klassifizierung und Veröffentlichen

Dieser Code erstellt die erste Iteration im Projekt und veröffentlicht anschließend diese Iteration im Vorhersageendpunkt. Der Name der veröffentlichten Iteration kann zum Senden von Vorhersageanforderungen verwendet werden. Eine Iteration ist erst im Vorhersageendpunkt verfügbar, wenn sie veröffentlicht wurde.

```go
fmt.Println("Training...")
iteration, _ := trainer.TrainProject(ctx, *project.ID)
for {
    if *iteration.Status != "Training" {
        break
    }
    fmt.Println("Training status: " + *iteration.Status)
    time.Sleep(1 * time.Second)
    iteration, _ = trainer.GetIteration(ctx, *project.ID, *iteration.ID)
}
fmt.Println("Training status: " + *iteration.Status)

trainer.PublishIteration(ctx, *project.ID, *iteration.ID, iteration_publish_name, prediction_resource_id))
```

### <a name="get-and-use-the-published-iteration-on-the-prediction-endpoint"></a>Abrufen und Verwenden der veröffentlichten Iteration für den Vorhersageendpunkt

Um ein Bild an den Vorhersageendpunkt zu senden und die Vorhersage abzurufen, fügen Sie am Ende der Datei den folgenden Code hinzu:

```go
    fmt.Println("Predicting...")
    predictor := prediction.New(prediction_key, endpoint)

    testImageData, _ := ioutil.ReadFile(path.Join(sampleDataDirectory, "Test", "test_image.jpg"))
    results, _ := predictor.ClassifyImage(ctx, *project.ID, iteration_publish_name, ioutil.NopCloser(bytes.NewReader(testImageData)), "")

    for _, prediction := range *results.Predictions {
        fmt.Printf("\t%s: %.2f%%", *prediction.TagName, *prediction.Probability * 100)
        fmt.Println("")
    }
}
```

## <a name="run-the-application"></a>Ausführen der Anwendung

Führen Sie *sample.go* aus.

```shell
go run sample.go
```

Die Ausgabe der Anwendung sollte dem folgenden Text ähneln:

```console
Creating project...
Adding images...
Training...
Training status: Training
Training status: Training
Training status: Training
Training status: Completed
Done!
        Hemlock: 93.53%
        Japanese Cherry: 0.01%
```

Daraufhin können Sie sich vergewissern, dass das Testbild (unter **<Basisimage-URL>/Images/Test/** ) ordnungsgemäß gekennzeichnet ist. Sie können auch zur [Custom Vision-Website](https://customvision.ai) zurückgehen und den aktuellen Status Ihres neu erstellten Projekts ansehen.

[!INCLUDE [clean-ic-project](includes/clean-ic-project.md)]

## <a name="next-steps"></a>Nächste Schritte

Sie wissen nun, wie die einzelnen Schritte des Bildklassifizierungsprozesses im Code ausgeführt werden. In diesem Beispiel wird eine einzelne Trainingsiteration ausgeführt. Zur Verbesserung der Genauigkeit muss ein Modell jedoch häufig mehrmals trainiert und getestet werden.

> [!div class="nextstepaction"]
> [Testen und erneutes Trainieren eines Modells mit Custom Vision Service](test-your-model.md)