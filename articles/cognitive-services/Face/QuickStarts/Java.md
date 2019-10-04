---
title: 'Schnellstart: Erkennen von Gesichtern in einem Bild mit der Azure-REST-API und Java'
titleSuffix: Azure Cognitive Services
description: In diesem Schnellstart verwenden Sie die Azure-Gesichtserkennungs-REST-API mit Java, um Gesichter in einem Bild zu erkennen.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 09/06/2019
ms.author: pafarley
ms.openlocfilehash: 30e4852668fc12c38cd7d1794c461041acd654db
ms.sourcegitcommit: 65131f6188a02efe1704d92f0fd473b21c760d08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2019
ms.locfileid: "70859198"
---
# <a name="quickstart-detect-faces-in-an-image-using-the-rest-api-and-java"></a>Schnellstart: Erkennen von Gesichtern in einem Bild mit der REST-API und Java

In diesem Schnellstart verwenden Sie die Azure-Gesichtserkennungs-REST-API mit Java, um Gesichter von Personen in einem Bild zu erkennen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen. 

## <a name="prerequisites"></a>Voraussetzungen

- Ein Abonnementschlüssel für die Gesichtserkennungs-API. Über die Seite [Cognitive Services ausprobieren](https://azure.microsoft.com/try/cognitive-services/?api=face-api) können Sie einen Abonnementschlüssel für eine kostenlose Testversion abrufen. Gehen Sie andernfalls wie unter [Schnellstart: Erstellen eines Cognitive Services-Kontos im Azure-Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) beschrieben vor, um den Gesichtserkennungs-API-Dienst zu abonnieren und Ihren Schlüssel zu erhalten.
- Eine Java-IDE Ihrer Wahl.

## <a name="create-the-java-project"></a>Erstellen des Java-Projekts

1. Erstellen Sie eine neue Java-Befehlszeilen-App in Ihrer IDE, und fügen eine **Main**-Klasse mit einer **main**-Methode hinzu.
1. Importieren Sie die folgenden Bibliotheken in Ihr Java-Projekt. Wenn Sie Maven verwenden, werden die Maven-Koordinaten für jede Bibliothek bereitgestellt.
   - [Apache-HTTP-Client](https://hc.apache.org/downloads.cgi) (org.apache.httpcomponents:httpclient:4.5.6)
   - [Apache-HTTP-Core](https://hc.apache.org/downloads.cgi) (org.apache.httpcomponents:httpcore:4.4.10)
   - [JSON-Bibliothek](https://github.com/stleary/JSON-java) (org.json:json:20180130)
   - [Apache Commons-Protokollierung](https://commons.apache.org/proper/commons-logging/download_logging.cgi) (commons-logging:commons-logging:1.1.2)

## <a name="add-face-detection-code"></a>Hinzufügen von Code für die Gesichtserkennung

Öffnen Sie die Main-Klasse Ihres Projekts. Hier fügen Sie den Code hinzu, der zum Laden von Bildern und Erkennen von Gesichtern erforderlich ist.

### <a name="import-packages"></a>Importieren von Paketen

Fügen Sie am Anfang der Datei die folgenden `import`-Anweisungen hinzu.

```java
// This sample uses Apache HttpComponents:
// http://hc.apache.org/httpcomponents-core-ga/httpcore/apidocs/
// https://hc.apache.org/httpcomponents-client-ga/httpclient/apidocs/

import java.net.URI;
import org.apache.http.HttpEntity;
import org.apache.http.HttpResponse;
import org.apache.http.client.HttpClient;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.StringEntity;
import org.apache.http.client.utils.URIBuilder;
import org.apache.http.impl.client.HttpClientBuilder;
import org.apache.http.util.EntityUtils;
import org.json.JSONArray;
import org.json.JSONObject;
```

### <a name="add-essential-fields"></a>Hinzufügen wichtiger Felder

Ersetzen Sie die **Main**-Klasse durch den folgenden Code: Mit diesen Daten wird angegeben, wie eine Verbindung mit dem Gesichtserkennungsdienst hergestellt wird und wo die Eingabedaten abgerufen werden. Sie müssen das Feld `subscriptionKey` mit dem Wert Ihres Abonnementschlüssels aktualisieren. Ändern Sie außerdem die Zeichenfolge `uriBase`, sodass sie die korrekte Endpunktzeichenfolge enthält. Sie können den Wert von `imageWithFaces` ggf. auch auf einen Pfad festlegen, der auf eine andere Bilddatei verweist.

[!INCLUDE [subdomains-note](../../../../includes/cognitive-services-custom-subdomains-note.md)]

Das Feld `faceAttributes` ist einfach eine Liste bestimmter Attributtypen. In diesem Feld wird angegeben, welche Informationen zu den erkannten Gesichtern abgerufen werden sollen.

```Java
public class Main {
    // Replace <Subscription Key> with your valid subscription key.
    private static final String subscriptionKey = "<Subscription Key>";

    private static final String uriBase =
        "https://<My Endpoint String>.com/face/v1.0/detect";

    private static final String imageWithFaces =
        "{\"url\":\"https://upload.wikimedia.org/wikipedia/commons/c/c3/RH_Louise_Lillian_Gish.jpg\"}";

    private static final String faceAttributes =
        "age,gender,headPose,smile,facialHair,glasses,emotion,hair,makeup,occlusion,accessories,blur,exposure,noise";
```

### <a name="call-the-face-detection-rest-api"></a>Aufrufen der Gesichtserkennungs-REST-API

Fügen Sie die **main**-Methode mit dem folgenden Code hinzu. Diese Methode erstellt einen REST-Aufruf der Gesichtserkennungs-API, um Gesichtsinformationen im Remotebild zu erkennen. (Die Zeichenfolge `faceAttributes` gibt an, welche Gesichtsattribute abgerufen werden sollen.) Anschließend werden die Ausgabedaten in eine JSON-Zeichenfolge geschrieben.

```Java
    public static void main(String[] args) {
        HttpClient httpclient = HttpClientBuilder.create().build();

        try
        {
            URIBuilder builder = new URIBuilder(uriBase);

            // Request parameters. All of them are optional.
            builder.setParameter("returnFaceId", "true");
            builder.setParameter("returnFaceLandmarks", "false");
            builder.setParameter("returnFaceAttributes", faceAttributes);

            // Prepare the URI for the REST API call.
            URI uri = builder.build();
            HttpPost request = new HttpPost(uri);

            // Request headers.
            request.setHeader("Content-Type", "application/json");
            request.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);

            // Request body.
            StringEntity reqEntity = new StringEntity(imageWithFaces);
            request.setEntity(reqEntity);

            // Execute the REST API call and get the response entity.
            HttpResponse response = httpclient.execute(request);
            HttpEntity entity = response.getEntity();
```

### <a name="parse-the-json-response"></a>Analysieren der JSON-Antwort

Fügen Sie direkt unter dem obigen Code den folgenden Block hinzu, der die zurückgegebenen JSON-Daten vor der Ausgabe an die Konsole in ein leichter lesbares Format konvertiert. Schließen Sie zum Schluss den try-catch-Block, die **main**-Methode und die **Main**-Klasse.

```Java
            if (entity != null)
            {
                // Format and display the JSON response.
                System.out.println("REST Response:\n");

                String jsonString = EntityUtils.toString(entity).trim();
                if (jsonString.charAt(0) == '[') {
                    JSONArray jsonArray = new JSONArray(jsonString);
                    System.out.println(jsonArray.toString(2));
                }
                else if (jsonString.charAt(0) == '{') {
                    JSONObject jsonObject = new JSONObject(jsonString);
                    System.out.println(jsonObject.toString(2));
                } else {
                    System.out.println(jsonString);
                }
            }
        }
        catch (Exception e)
        {
            // Display error message.
            System.out.println(e.getMessage());
        }
    }
}
```

## <a name="run-the-app"></a>Ausführen der App

Kompilieren Sie den Code, und führen Sie ihn aus. Bei einer erfolgreichen Antwort werden Gesichtserkennungsdaten in einem leicht lesbaren JSON-Format im Konsolenfenster angezeigt. Beispiel:

```json
[{
  "faceRectangle": {
    "top": 131,
    "left": 177,
    "width": 162,
    "height": 162
  },
  "faceAttributes": {
    "makeup": {
      "eyeMakeup": true,
      "lipMakeup": true
    },
    "facialHair": {
      "sideburns": 0,
      "beard": 0,
      "moustache": 0
    },
    "gender": "female",
    "accessories": [],
    "blur": {
      "blurLevel": "low",
      "value": 0.06
    },
    "headPose": {
      "roll": 0.1,
      "pitch": 0,
      "yaw": -32.9
    },
    "smile": 0,
    "glasses": "NoGlasses",
    "hair": {
      "bald": 0,
      "invisible": false,
      "hairColor": [
        {
          "color": "brown",
          "confidence": 1
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
    },
    "emotion": {
      "contempt": 0,
      "surprise": 0.005,
      "happiness": 0,
      "neutral": 0.986,
      "sadness": 0.009,
      "disgust": 0,
      "anger": 0,
      "fear": 0
    },
    "exposure": {
      "value": 0.67,
      "exposureLevel": "goodExposure"
    },
    "occlusion": {
      "eyeOccluded": false,
      "mouthOccluded": false,
      "foreheadOccluded": false
    },
    "noise": {
      "noiseLevel": "low",
      "value": 0
    },
    "age": 22.9
  },
  "faceId": "49d55c17-e018-4a42-ba7b-8cbbdfae7c6f"
}]
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie eine einfache Java-Konsolenanwendung erstellt, die REST-Aufrufe mit der Azure-Gesichtserkennungs-API verwendet, um Gesichter in einem Bild zu erkennen und ihre Attribute zurückzugeben. Als Nächstes erfahren Sie, welche weiteren Möglichkeiten diese Funktion in einer Android-Anwendung bietet.

> [!div class="nextstepaction"]
> [Tutorial: Erstellen einer Android-App zum Erkennen und Umranden von Gesichtern in einem Bild](../Tutorials/FaceAPIinJavaForAndroidTutorial.md)
