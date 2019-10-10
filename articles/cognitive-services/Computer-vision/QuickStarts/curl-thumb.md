---
title: 'Schnellstart: Generieren einer Miniaturansicht – REST, cURL'
titleSuffix: Azure Cognitive Services
description: In diesem Schnellstart generieren Sie eine Miniaturansicht von einem Bild, indem Sie die Maschinelles Sehen-API mit cURL verwenden.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 07/03/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 350bc95e08aa994e4cb70db6bf1f08d53bbec5a3
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72177302"
---
# <a name="quickstart-generate-a-thumbnail-using-the-computer-vision-rest-api-and-curl"></a>Schnellstart: Generieren einer Miniaturansicht mit der Maschinelles Sehen-REST-API und cURL

In dieser Schnellstartanleitung generieren Sie eine Miniaturansicht eines Bilds mithilfe der REST-API für maschinelles Sehen. Sie geben die gewünschte Höhe und Breite an. Diese können sich vom Seitenverhältnis des Eingabebilds unterscheiden. Für maschinelles Sehen wird die intelligente Zuschneidefunktion verwendet, um den gewünschten Bereich zu identifizieren und Zuschneidekoordinaten um diese Region zu generieren.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/ai/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=cognitive-services) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

- Sie benötigen [cURL](https://curl.haxx.se/windows).
- Sie benötigen einen Abonnementschlüssel für maschinelles Sehen. Über die Seite [Cognitive Services ausprobieren](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision) können Sie einen Schlüssel für eine kostenlose Testversion abrufen. Oder gehen Sie wie unter [Schnellstart: Erstellen eines Cognitive Services-Kontos im Azure-Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) beschrieben vor, um „Maschinelles Sehen“ zu abonnieren und Ihren Schlüssel zu erhalten.

## <a name="get-thumbnail-request"></a>Get Thumbnail-Anforderung

Mit der [Get Thumbnail-Methode](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb) können Sie aus einem Bild eine Miniaturansicht generieren.

Führen Sie zum Ausführen des Beispiels die folgenden Schritte aus:

1. Kopieren Sie den folgenden Code in einen Editor.
1. Ersetzen Sie `<Subscription Key>` durch Ihren gültigen Abonnementschlüssel.
1. Ersetzen Sie `<File>` durch den Pfad und Dateinamen für die Speicherung der Miniaturansicht.
1. Ändern Sie die Anforderungs-URL (`https://westcentralus.api.cognitive.microsoft.com/vision/v2.1`) ggf. in die Region, in der Sie Ihre Abonnementschlüssel erhalten haben.
1. Ändern Sie optional das Bild (`{\"url\":\"...`), das Sie analysieren möchten.
1. Öffnen Sie auf einem Computer, auf dem cURL installiert ist, ein Befehlsfenster.
1. Fügen Sie den Code in das Fenster ein, und führen Sie den Befehl aus.

>[!NOTE]
>Sie müssen in Ihrem REST-Aufruf den gleichen Standort verwenden, den Sie zum Erwerb Ihrer Abonnementschlüssel verwendet haben. Wenn Sie Ihre Abonnementschlüssel beispielsweise von „westus“ erhalten haben, ersetzen Sie „westcentralus“ in der URL unten durch „westus“.

## <a name="create-and-run-the-sample-command"></a>Erstellen und Ausführen des Beispielbefehls

Führen Sie zum Erstellen und Ausführen des Beispiels die folgenden Schritte aus:

1. Kopieren Sie den folgenden Befehl, und fügen Sie ihn in einen Text-Editor ein.
1. Nehmen Sie die folgenden Änderungen im Befehl vor, falls dies erforderlich ist:
    1. Ersetzen Sie den `<subscriptionKey>`-Wert durch Ihren Abonnementschlüssel.
    1. Ersetzen Sie den Wert von `<thumbnailFile>` durch den Pfad und den Namen der Datei, in der die Miniaturansicht gespeichert werden soll.
    1. Ersetzen Sie den ersten Teil der Anforderungs-URL (`westcentralus`) durch den Text in Ihrer eigenen Endpunkt-URL.
        [!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]
    1. Ändern Sie optional die Bild-URL im Anforderungstext (`https://upload.wikimedia.org/wikipedia/commons/thumb/5/56/Shorkie_Poo_Puppy.jpg/1280px-Shorkie_Poo_Puppy.jpg\`) in die URL eines anderen Bilds, von dem eine Miniaturansicht generiert werden soll.
1. Öffnen Sie ein Eingabeaufforderungsfenster.
1. Fügen Sie den Befehl aus dem Text-Editor in das Eingabeaufforderungsfenster ein, und führen Sie den Befehl aus.

    ```bash
    curl -H "Ocp-Apim-Subscription-Key: <subscriptionKey>" -o <thumbnailFile> -H "Content-Type: application/json" "https://westcentralus.api.cognitive.microsoft.com/vision/v2.1/generateThumbnail?width=100&height=100&smartCropping=true" -d "{\"url\":\"https://upload.wikimedia.org/wikipedia/commons/thumb/5/56/Shorkie_Poo_Puppy.jpg/1280px-Shorkie_Poo_Puppy.jpg\"}"
    ```

## <a name="examine-the-response"></a>Untersuchen der Antwort

Bei einer erfolgreichen Antwort wird das Miniaturbild in die unter `<thumbnailFile>` angegebene Datei geschrieben. Wenn die Anforderung nicht erfolgreich ist, enthält die Antwort einen Fehlercode und eine Meldung, damit Sie ermitteln können, wo der Fehler liegt. Wenn die Anforderung scheinbar erfolgreich war, die erstellte Miniaturansicht jedoch keine gültige Bilddatei ist, ist Ihr Abonnementschlüssel unter Umständen ungültig.

## <a name="next-steps"></a>Nächste Schritte

Erkunden der Maschinelles Sehen-API zum Analysieren von Bildern, Erkennen von Prominenten und Sehenswürdigkeiten, Erstellen von Miniaturansichten und Extrahieren von gedrucktem und handschriftlichem Text Um schnell mit der Maschinelles Sehen-API zu experimentieren, probieren Sie die [Open API-Testkonsole](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console) aus.

> [!div class="nextstepaction"]
> [Erkunden der Maschinelles Sehen-API](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
