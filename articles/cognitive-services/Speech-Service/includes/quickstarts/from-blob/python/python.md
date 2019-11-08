---
title: 'Schnellstart: Erkennen von im Blobspeicher gespeicherter Sprache, C# – Spracherkennungsdienst'
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 10/28/2019
ms.author: erhopf
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: 40b226796b4dfb9aced3c6b00eba1a12bad66894
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73506155"
---
## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die folgenden Schritte aus, bevor Sie beginnen:

> [!div class="checklist"]
> * [Erstellen einer Azure Speech-Ressource](../../../../get-started.md)
> * [Schnellstart: Hochladen, Herunterladen und Auflisten von Blobs mit dem Azure-Portal](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal)
> * [Einrichten Ihrer Entwicklungsumgebung](../../../../quickstarts/setup-platform.md)
> * [Erstellen eines leeren Beispielprojekts](../../../../quickstarts/create-project.md)

## <a name="download-and-install-the-api-client-library"></a>Herunterladen und Installieren der API-Clientbibliothek

Um das Beispiel auszuführen, müssen Sie die Python-Bibliothek für die REST-API generieren, wozu Sie [Swagger](https://swagger.io) verwenden.

Führen Sie die folgenden Schritte für die Installation aus:

1. Wechseln Sie zur Adresse https://editor.swagger.io.
1. Klicken Sie auf **Datei** und dann auf **URL importieren**.
1. Geben Sie die Swagger-URL ein, einschließlich der Region für Ihr Spracherkennungsdienste-Abonnement: `https://<your-region>.cris.ai/docs/v2.0/swagger`.
1. Klicken Sie auf **Generate Client** (Client generieren), und wählen Sie **Python** aus.
1. Speichern Sie die Clientbibliothek.
1. Extrahieren Sie die heruntergeladene Datei „python-client-generated.zip“ an einer beliebigen Stelle im Dateisystem.
1. Installieren Sie das extrahierte Python-Clientmodul mithilfe von PIP in ihrer Python-Umgebung: `pip install path/to/package/python-client`.
1. Das installierte Paket hat den Namen `swagger_client`. Sie können mithilfe des Befehls `python -c "import swagger_client"` überprüfen, ob die Installation funktioniert hat.

> **Hinweis:** Aufgrund eines [bekannten Fehlers in der automatischen Swagger-Generierung](https://github.com/swagger-api/swagger-codegen/issues/7541) könnten beim Importieren des `swagger_client`-Pakets Fehler auftreten.
> Diese können Sie beheben, indem Sie die Zeile mit dem Inhalt
> ```py
> from swagger_client.models.model import Model  # noqa: F401,E501
> ```
> aus der Datei `swagger_client/models/model.py` und die Zeile mit dem Inhalt
> ```py
> from swagger_client.models.inner_error import InnerError  # noqa: F401,E501
> ```
> aus der Datei `swagger_client/models/inner_error.py` innerhalb des installierten Pakets löschen. Die Fehlermeldung informiert Sie darüber, wo sich diese Dateien für Ihre Installation befinden.

## <a name="install-other-dependencies"></a>Installieren anderer Abhängigkeiten

Im Beispiel wird die `requests`-Bibliothek verwendet. Sie können sie installieren mithilfe des Befehls

```bash
pip install requests
```

## <a name="start-with-some-boilerplate-code"></a>Beginnen mit Codebausteinen

Fügen Sie Code hinzu, der als Gerüst für das Projekt fungiert.

[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/from-blob/python-client/main.py?range=1-2,7-34,115-119)]
(Sie müssen die Werte von `YourSubscriptionKey`, `YourServiceRegion` und `YourFileUrl` durch Ihre eigenen Werte ersetzen.)

## <a name="create-and-configure-an-http-client"></a>Erstellen und Konfigurieren eines HTTP-Clients
Als Erstes benötigen wir einen HTTP-Client, der über eine korrekte Basis-URL und einen Authentifizierungssatz verfügt.
Fügen Sie diesen Code in `transcribe` ein. [!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/from-blob/python-client/main.py?range=37-45)]

## <a name="generate-a-transcription-request"></a>Generieren einer Transkriptionsanforderung
Als Nächstes generieren wir die Transkriptionsanforderung. Fügen Sie diesen Code `transcribe` hinzu. [!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/from-blob/python-client/main.py?range=52-54)]

## <a name="send-the-request-and-check-its-status"></a>Senden der Anforderung und Überprüfen ihres Status
Nun senden wir die Anforderung an den Spracherkennungsdienst und überprüfen den ersten Antwortcode. Mit diesem Antwortcode wird lediglich angegeben, ob der Dienst die Anforderung erhalten hat. Der Dienst gibt in den Antwortheadern eine URL zurück, die für den Speicherort steht, an dem der Transkriptionsstatus gespeichert wird.
[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/from-blob/python-client/main.py?range=65-73)]

## <a name="wait-for-the-transcription-to-complete"></a>Warten auf den Abschluss der Transkription
Da die Transkription vom Dienst asynchron verarbeitet wird, müssen wir regelmäßig den Status abrufen. Wir führen alle fünf Sekunden eine Überprüfung durch.

Wir werden alle Transkriptionen auflisten, die von dieser Spracherkennungsdienst-Ressource verarbeitet werden, und nach der von uns erstellten suchen.

Hier ist der Abrufcode mit der Statusanzeige für alle Vorgänge mit Ausnahme des erfolgreichen Abschlusses angegeben. Dies führen wir als Nächstes durch.
[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/from-blob/python-client/main.py?range=75-94,99-112)]

## <a name="display-the-transcription-results"></a>Anzeigen der Transkriptionsergebnisse
Nachdem der Dienst die Transkription erfolgreich abgeschlossen hat, werden die Ergebnisse unter einer anderen URL gespeichert, die wir in der Statusantwort erhalten.

Hier erhalten wir den JSON-Ergebniscode und zeigen ihn an.
[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/from-blob/python-client/main.py?range=95-98)]

## <a name="check-your-code"></a>Überprüfen des Codes
Ihr Code sollte nun wie folgt aussehen: (Wir haben dieser Version einige Kommentare hinzugefügt.) [!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/from-blob/python-client/main.py?range=1-118)]

## <a name="build-and-run-your-app"></a>Erstellen und Ausführen der App

Sie können nun Ihre App erstellen und die Spracherkennung mit dem Spracherkennungsdienst testen.

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [footer](./footer.md)]
