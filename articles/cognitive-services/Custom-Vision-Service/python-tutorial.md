---
title: 'Schnellstart: Erstellen eines Bildklassifizierungsprojekts mit dem Custom Vision SDK für Python'
titleSuffix: Azure Cognitive Services
description: Erstellen Sie ein Projekt, fügen Sie Tags hinzu, laden Sie Bilder hoch, trainieren Sie Ihr Projekt, und machen Sie eine Vorhersage unter Verwendung des Python SDK.
services: cognitive-services
author: areddish
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: quickstart
ms.date: 08/08/2019
ms.author: areddish
ms.openlocfilehash: 6a626c2e27bf26ed98dd3244c52671a4dfa4aa76
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "73519373"
---
# <a name="quickstart-create-an-image-classification-project-with-the-custom-vision-python-sdk"></a>Schnellstart: Erstellen eines Bildklassifizierungsprojekts mit dem Custom Vision SDK für Python

Dieser Artikel enthält Informationen und Beispielcode für die ersten Schritte mit dem Custom Vision SDK mit Python und unterstützt Sie beim Erstellen eines Bildklassifizierungsmodells. Nach der Erstellung können Sie Tags hinzufügen, Bilder hochladen, das Projekt trainieren, die veröffentlichte Endpunkt-URL für Vorhersagen des Projekts abrufen und den Endpunkt für die programmgesteuerte Überprüfung eines Bilds verwenden. Verwenden Sie dieses Beispiel als Vorlage für die Erstellung Ihrer eigenen Python-Anwendung. Falls Sie den Prozess zur Erstellung und Verwendung eines Klassifizierungsmodells _ohne_ Code durchlaufen möchten, sehen Sie sich stattdessen die [browserbasierte Anleitung](getting-started-build-a-classifier.md) an.

## <a name="prerequisites"></a>Voraussetzungen

- [Python 2.7+ oder 3.5+](https://www.python.org/downloads/)
- [Pip](https://pip.pypa.io/en/stable/installing/)-Tool
- [!INCLUDE [create-resources](includes/create-resources.md)]

## <a name="install-the-custom-vision-sdk"></a>Installieren des Custom Vision SDK

Führen Sie den folgenden Befehl in PowerShell aus, um das Custom Vision Service SDK für Python zu installieren:

```powershell
pip install azure-cognitiveservices-vision-customvision
```

[!INCLUDE [get-keys](includes/get-keys.md)]

[!INCLUDE [python-get-images](includes/python-get-images.md)]

## <a name="add-the-code"></a>Hinzufügen des Codes

Erstellen Sie in Ihrem bevorzugten Projektverzeichnis eine neue Datei namens *sample.py*.

### <a name="create-the-custom-vision-service-project"></a>Erstellen des Custom Vision Service-Projekts

Fügen Sie Ihrem Skript den folgenden Code hinzu, um ein neues Custom Vision Service-Projekt zu erstellen. Fügen Sie Ihre Abonnementschlüssel in die entsprechenden Definitionen ein. Ermitteln Sie außerdem Ihre Endpunkt-URL über die Seite „Einstellungen“ der Custom Vision-Website.

Informationen zur Angabe weiterer Optionen bei der Erstellung Ihres Projekts finden Sie im Artikel zur Methode [create_project](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-customvision/azure.cognitiveservices.vision.customvision.training.custom_vision_training_client.customvisiontrainingclient?view=azure-python#create-project-name--description-none--domain-id-none--classification-type-none--target-export-platforms-none--custom-headers-none--raw-false----operation-config- ). Informationen zur Projekterstellung finden Sie unter [Schnellstart: Erstellen einer Klassifizierung mit Custom Vision](getting-started-build-a-classifier.md).  

```Python
from azure.cognitiveservices.vision.customvision.training import CustomVisionTrainingClient
from azure.cognitiveservices.vision.customvision.training.models import ImageFileCreateEntry

ENDPOINT = "<your API endpoint>"

# Replace with a valid key
training_key = "<your training key>"
prediction_key = "<your prediction key>"
prediction_resource_id = "<your prediction resource id>"

publish_iteration_name = "classifyModel"

trainer = CustomVisionTrainingClient(training_key, endpoint=ENDPOINT)

# Create a new project
print ("Creating project...")
project = trainer.create_project("My New Project")
```

### <a name="create-tags-in-the-project"></a>Erstellen von Tags im Projekt

Um Ihrem Projekt Klassifizierungstags hinzuzufügen, fügen Sie den folgenden Code am Ende der Datei *sample.py* hinzu:

```Python
# Make two tags in the new project
hemlock_tag = trainer.create_tag(project.id, "Hemlock")
cherry_tag = trainer.create_tag(project.id, "Japanese Cherry")
```

### <a name="upload-and-tag-images"></a>Hochladen und Kennzeichnen von Bildern

Um dem Projekt die Beispielbilder hinzuzufügen, fügen Sie nach der Erstellung der Kategorien den folgenden Code ein. Dieser Code lädt das Bild mit dem entsprechenden Tag hoch. Sie können bis zu 64 Bilder in einem Batch hochladen.

> [!NOTE]
> Sie müssen den Pfad zu den Bildern basierend darauf ändern, wo Sie zuvor das Cognitive Services-Beispielprojekt für das Python SDK heruntergeladen haben.

```Python
base_image_url = "<path to project>"

print("Adding images...")

image_list = []

for image_num in range(1, 11):
    file_name = "hemlock_{}.jpg".format(image_num)
    with open(base_image_url + "images/Hemlock/" + file_name, "rb") as image_contents:
        image_list.append(ImageFileCreateEntry(name=file_name, contents=image_contents.read(), tag_ids=[hemlock_tag.id]))

for image_num in range(1, 11):
    file_name = "japanese_cherry_{}.jpg".format(image_num)
    with open(base_image_url + "images/Japanese Cherry/" + file_name, "rb") as image_contents:
        image_list.append(ImageFileCreateEntry(name=file_name, contents=image_contents.read(), tag_ids=[cherry_tag.id]))

upload_result = trainer.create_images_from_files(project.id, images=image_list)
if not upload_result.is_batch_successful:
    print("Image batch upload failed.")
    for image in upload_result.images:
        print("Image status: ", image.status)
    exit(-1)
```

### <a name="train-the-classifier-and-publish"></a>Trainieren der Klassifizierung und Veröffentlichen

Dieser Code erstellt die erste Iteration im Projekt und veröffentlicht anschließend diese Iteration im Vorhersageendpunkt. Der Name der veröffentlichten Iteration kann zum Senden von Vorhersageanforderungen verwendet werden. Eine Iteration ist erst im Vorhersageendpunkt verfügbar, wenn sie veröffentlicht wurde.

```Python
import time

print ("Training...")
iteration = trainer.train_project(project.id)
while (iteration.status != "Completed"):
    iteration = trainer.get_iteration(project.id, iteration.id)
    print ("Training status: " + iteration.status)
    time.sleep(1)

# The iteration is now trained. Publish it to the project endpoint
trainer.publish_iteration(project.id, iteration.id, publish_iteration_name, prediction_resource_id)
print ("Done!")
```

### <a name="get-and-use-the-published-iteration-on-the-prediction-endpoint"></a>Abrufen und Verwenden der veröffentlichten Iteration für den Vorhersageendpunkt

Um ein Bild an den Vorhersageendpunkt zu senden und die Vorhersage abzurufen, fügen Sie am Ende der Datei den folgenden Code hinzu:

```python
from azure.cognitiveservices.vision.customvision.prediction import CustomVisionPredictionClient

# Now there is a trained endpoint that can be used to make a prediction
predictor = CustomVisionPredictionClient(prediction_key, endpoint=ENDPOINT)

with open(base_image_url + "images/Test/test_image.jpg", "rb") as image_contents:
    results = predictor.classify_image(
        project.id, publish_iteration_name, image_contents.read())

    # Display the results.
    for prediction in results.predictions:
        print("\t" + prediction.tag_name +
              ": {0:.2f}%".format(prediction.probability * 100))
```

## <a name="run-the-application"></a>Ausführen der Anwendung

Führen Sie *sample.py* aus.

```powershell
python sample.py
```

Die Ausgabe der Anwendung sollte dem folgenden Text ähneln:

```console
Creating project...
Adding images...
Training...
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
