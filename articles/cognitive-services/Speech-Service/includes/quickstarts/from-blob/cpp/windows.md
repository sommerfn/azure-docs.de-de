---
title: 'Schnellstart: Erkennen von im Blobspeicher gespeicherter Sprache (C++): Speech-Dienst'
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
ms.openlocfilehash: 2173dbabc83ff0a03c0cfd18e02a6f3183ef90e2
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73506091"
---
## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die folgenden Schritte aus, bevor Sie beginnen:

> [!div class="checklist"]
> * [Erstellen einer Azure Speech-Ressource](../../../../get-started.md)
> * [Schnellstart: Hochladen, Herunterladen und Auflisten von Blobs mit dem Azure-Portal](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal)
> * [Einrichten Ihrer Entwicklungsumgebung](../../../../quickstarts/setup-platform.md?tabs=dotnet)
> * [Erstellen eines leeren Beispielprojekts](../../../../quickstarts/create-project.md?tabs=dotnet)

## <a name="open-your-project-in-visual-studio"></a>Öffnen Sie Ihr Projekt in Visual Studio.

Im ersten Schritt müssen Sie sicherstellen, dass das Projekt in Visual Studio geöffnet ist.

1. Starten Sie Visual Studio 2019.
2. Laden Sie Ihr Projekt, und öffnen Sie `helloworld.cpp`.

## <a name="add-a-references"></a>Hinzufügen eines Verweises

Um bei der Codeentwicklung Zeit zu sparen, verwenden wir zwei externe Komponenten:
* [CPP REST SDK](https://github.com/microsoft/cpprestsdk): Eine Clientbibliothek zum Senden von REST-Aufrufen an einen REST-Dienst.
* [nlohmann/json](https://github.com/nlohmann/json): Benutzerfreundliche Bibliothek für JSON-Analyse, -Serialisierung und -Deserialisierung.

Beide Komponenten können mit [vcpkg](https://github.com/Microsoft/vcpkg/) installiert werden.

```
vcpkg install cpprestsdk cpprestsdk:x64-windows
vcpkg install nlohmann-json
```

## <a name="start-with-some-boilerplate-code"></a>Beginnen mit Codebausteinen

Fügen Sie Code hinzu, der als Gerüst für das Projekt fungiert.

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/from-blob/helloworld.cpp?range=7-32,187-190,300-309)]
(Sie müssen die Werte von `YourSubscriptionKey`, `YourServiceRegion` und `YourFileUrl` durch Ihre eigenen Werte ersetzen.)

## <a name="json-wrappers"></a>JSON-Wrapper

Da für die REST-APIs Anforderungen im JSON-Format verwendet und Ergebnisse ebenfalls im JSON-Format zurückgegeben werden, können wir auch ausschließlich über Zeichenfolgen interagieren. Diese Vorgehensweise wird aber nicht empfohlen.
Um die Verwaltung der Anforderungen und Antworten zu vereinfachen, deklarieren wir einige Klassen für die Serialisierung/Deserialisierung des JSON-Codes und einige Methoden als Unterstützung für „nlohmann/json“.

Fügen Sie die entsprechenden Deklarationen vor `recognizeSpeech` ein.
[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/from-blob/helloworld.cpp?range=33-185)]

## <a name="create-and-configure-an-http-client"></a>Erstellen und Konfigurieren eines HTTP-Clients
Als Erstes benötigen wir einen HTTP-Client, der über eine korrekte Basis-URL und einen Authentifizierungssatz verfügt.
Fügen Sie diesen Code in `recognizeSpeech` ein. [!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/from-blob/helloworld.cpp?range=191-197)]

## <a name="generate-a-transcription-request"></a>Generieren einer Transkriptionsanforderung
Als Nächstes generieren wir die Transkriptionsanforderung. Fügen Sie diesen Code `recognizeSpeech` hinzu. [!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/from-blob/helloworld.cpp?range=199-203)]

## <a name="send-the-request-and-check-its-status"></a>Senden der Anforderung und Überprüfen ihres Status
Nun senden wir die Anforderung an den Speech-Dienst und überprüfen den ersten Antwortcode. Mit diesem Antwortcode wird lediglich angegeben, ob der Dienst die Anforderung erhalten hat. Der Dienst gibt in den Antwortheadern eine URL zurück, die für den Speicherort steht, an dem der Transkriptionsstatus gespeichert wird.
[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/from-blob/helloworld.cpp?range=204-216)]

## <a name="wait-for-the-transcription-to-complete"></a>Warten auf den Abschluss der Transkription
Da die Transkription vom Dienst asynchron verarbeitet wird, müssen wir regelmäßig den Status abrufen. Wir führen alle fünf Sekunden eine Überprüfung durch.

Wir können den Status überprüfen, indem wir den Inhalt unter der URL abrufen, die wir beim Senden der Anforderung erhalten haben. Wenn wir den Inhalt zurückerhalten, führen wir dafür eine Deserialisierung in eine unserer Hilfsklassen durch, um die Interaktion zu vereinfachen.

Hier ist der Abrufcode mit der Statusanzeige für alle Vorgänge mit Ausnahme des erfolgreichen Abschlusses angegeben. Dies führen wir als Nächstes durch.
[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/from-blob/helloworld.cpp?range=222-245,285-299)]

## <a name="display-the-transcription-results"></a>Anzeigen der Transkriptionsergebnisse
Nachdem der Dienst die Transkription erfolgreich abgeschlossen hat, werden die Ergebnisse unter einer anderen URL gespeichert, die wir in der Statusantwort erhalten.

Wir laden den Inhalt dieser URL herunter, deserialisieren den JSON-Code und führen eine Schleife durch die Ergebnisse durch. Während dieses Vorgangs wird der Anzeigetext ausgegeben.
[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/from-blob/helloworld.cpp?range=246-284)]

## <a name="check-your-code"></a>Überprüfen des Codes
Ihr Code sollte nun wie folgt aussehen: (Wir haben dieser Version einige Kommentare hinzugefügt.) [!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/from-blob/helloworld.cpp?range=7-308)]

## <a name="build-and-run-your-app"></a>Erstellen und Ausführen der App

Sie können nun Ihre App erstellen und die Spracherkennung mit dem Speech-Dienst testen.

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [footer](./footer.md)]