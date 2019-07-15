---
title: Wie man einen Schulungsdatensatz für ein benutzerdefiniertes Modell erstellt – Form Recognizer
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie sicherstellen können, dass Ihr Schulungsdatensatz für das Training eines Form Recognizer-Modells optimiert ist.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: form-recognizer
ms.topic: conceptual
ms.date: 06/19/2019
ms.author: pafarley
ms.openlocfilehash: ad9bba53390e3c4262f999ebcc57ab354f1e3d69
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2019
ms.locfileid: "67537618"
---
# <a name="build-a-training-data-set-for-a-custom-model"></a>Erstellen eines Schulungsdatensatzes für ein benutzerdefiniertes Modell

Wenn Sie das benutzerdefinierte Modell der Formularerkennung verwenden, stellen Sie Ihre eigenen Schulungsdaten zur Verfügung, damit das Modell auf Ihre branchenspezifischen Formulare angewendet werden kann. Sie können ein Modell mit fünf ausgefüllten Formularen oder einem leeren Formular (Sie müssen das Wort "leer" in den Dateinamen aufnehmen) sowie zwei ausgefüllten Formularen trainieren. Auch wenn Sie genügend ausgefüllte Formulare zum Trainieren haben, kann das Hinzufügen eines leeren Formulars zu Ihrem Schulungsdatensatz die Genauigkeit des Modells verbessern.

## <a name="training-data-tips"></a>Tipps zu Trainingsdaten

Es ist wichtig, einen Datensatz zu verwenden, der für das Training optimiert ist. Verwenden Sie die folgenden Tipps, um sicherzustellen, dass Sie die besten Ergebnisse aus dem Betrieb des [Train Model](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api/operations/TrainCustomModel) erzielen:

* Verwenden Sie wenn möglich, textbasierte PDF-Dokumente anstelle von Image-basierten Dokumenten. Gescannte PDF-Dateien werden als Bilder behandelt.
* Verwenden Sie ein leeres Formular und zwei ausgefüllte Formulare, wenn Sie diese zur Verfügung haben.
* Verwenden Sie für ausgefüllte Formulare Beispiele, bei denen alle ihre Felder ausgefüllt sind.
* Verwenden Sie Formen mit unterschiedlichen Werten in jedem Feld.
* Wenn Ihre Formularbilder geringerer Qualität sind, verwenden Sie einen größeren Datensatz (z. B. 10-15 Bilder).

## <a name="general-input-requirements"></a>Eingabeanforderungen

Stellen Sie sicher, dass Ihr Schulungsdatensatz auch den Eingabeanforderungen für alle Form Recognizer-Inhalte entspricht. 

[!INCLUDE [input requirements](./includes/input-requirements.md)]

## <a name="upload-your-training-data"></a>Hochladen Ihrer Schulungsdaten

Wenn Sie den Satz von Formulardokumenten, die Sie für die Schulung verwenden werden, zusammengestellt haben, müssen Sie ihn in einen Azure Blob-Speicherbehälter hochladen. Wenn Sie nicht wissen, wie Sie ein Azure-Speicherkonto mit einem Container erstellen können, folgen Sie dem [Azure Storage-Schnellstart für das Azure-Portal](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal).

### <a name="organize-your-data-in-subfolders-optional"></a>Organisieren von Daten in Unterordnern (optional)

Standardmäßig verwendet die [Train Model](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api/operations/TrainCustomModel) API nur Formulardokumente, die sich im Stammverzeichnis Ihres Lagercontainers befinden. Sie können jedoch mit Daten in Unterordnern trainieren, wenn Sie diese im API-Aufruf angeben. Normalerweise hat der Körper [Train Model](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api/operations/TrainCustomModel) beim Aufruf die folgende Form, wobei `<SAS URL>` die URL der Signatur für den gemeinsamen Zugriff Ihres Containers ist:

```json
{
  "source":"<SAS URL>"
}
```

Wenn Sie den folgenden Inhalt zum Anforderungstext hinzufügen, trainiert die API mit Dokumenten, die sich in Unterordnern befinden. Das `"prefix"` Feld ist optional und beschränkt den Datensatz des Trainings auf Dateien, deren Pfade mit der angegebenen Zeichenkette beginnen. Ein Wert von `"Test"`, zum Beispiel, bewirkt also, dass die API nur die Dateien oder Ordner betrachtet, die mit dem Wort "Test" beginnen.

```json
{
  "source": "<SAS URL>",
  "sourceFilter": {
    "prefix": "<prefix string>",
    "includeSubFolders": true
  }
}
```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun gelernt haben, wie man einen Schulungsdatensatz erstellt, folgen Sie einem Schnellstart, um ein benutzerdefiniertes Formularerkennungsmodell zu trainieren und es in Ihren Formularen zu verwenden.

* [Schnellstart: Trainieren eines Modells und Extrahieren von Formulardaten mit cURL](./quickstarts/curl-train-extract.md)
* [Schnellstart: Trainieren Sie ein Modell und extrahieren Sie Formulardaten über die REST-API mit Python](./quickstarts/python-train-extract.md)

