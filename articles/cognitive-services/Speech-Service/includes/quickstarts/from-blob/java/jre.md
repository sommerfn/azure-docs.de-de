---
title: 'Schnellstart: Erkennen von Sprache aus einem Blobspeicher (Java) – Speech-Dienst'
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
ms.openlocfilehash: c5fa12621d689287db0d91885fc0a35edb772294
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73506059"
---
## <a name="prerequisites"></a>Voraussetzungen

Führen Sie unbedingt die folgenden Schritte aus, bevor Sie beginnen:

> [!div class="checklist"]
> * [Erstellen einer Azure Speech-Ressource](../../../../get-started.md)
> * [Hochladen einer Quelldatei in ein Azure-Blob](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal)
> * [Einrichten Ihrer Entwicklungsumgebung](../../../../quickstarts/setup-platform.md?tabs=dotnet)
> * [Erstellen eines leeren Beispielprojekts](../../../../quickstarts/create-project.md?tabs=dotnet)

## <a name="open-your-project-in-eclipse"></a>Öffnen Ihres Projekts in Eclipse

Stellen Sie zunächst sicher, dass Ihr Projekt in Eclipse geöffnet ist.

1. Starten von Eclipse
2. Laden Sie Ihr Projekt, und öffnen Sie `Main.java`.

## <a name="add-a-reference-to-gson"></a>Hinzufügen eines Verweises auf Gson
In dieser Schnellstartanleitung verwenden wir einen externen JSON-Serialisierer/Deserialisierer. Für Java haben wir [Gson](https://github.com/google/gson) gewählt.

Öffnen Sie die Datei „pom.xml“, und fügen Sie den folgenden Verweis hinzu: [!code-xml[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/from-blob/pom.xml?range=19-25)]

## <a name="start-with-some-boilerplate-code"></a>Beginnen mit Codebausteinen

Fügen Sie Code hinzu, der als Gerüst für das Projekt fungiert.

[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/from-blob/src/quickstart/Main.java?range=1-13,95-105,206-207)]
(Ersetzen Sie die Werte von `YourSubscriptionKey`, `YourServiceRegion` und `YourFileUrl` durch Ihre eigenen Werte.)

## <a name="json-wrappers"></a>JSON-Wrapper

Da die REST-APIs Anforderungen im JSON-Format akzeptieren und Ergebnisse im JSON-Format zurückgeben, könnten wir auch nur über Zeichenfolgen mit ihnen interagieren. Diese Vorgehensweise wird jedoch nicht empfohlen.
Um die Verwaltung der Anforderungen und Antworten zu vereinfachen, deklarieren wir einige Klassen für die Serialisierung/Deserialisierung des JSON-Codes.

Fügen Sie die entsprechenden Deklarationen vor `Main` ein.
[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/from-blob/src/quickstart/Main.java?range=15-93)]

## <a name="create-and-configure-an-http-client"></a>Erstellen und Konfigurieren eines HTTP-Clients
Als Erstes benötigen wir einen HTTP-Client, der über eine korrekte Basis-URL und einen korrekten Authentifizierungssatz verfügt.
Fügen Sie den folgenden Code in `Main` ein: [!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/from-blob/src/quickstart/Main.java?range=106-113)]

## <a name="generate-a-transcription-request"></a>Generieren einer Transkriptionsanforderung
Als Nächstes generieren wir die Transkriptionsanforderung. Fügen Sie den folgenden Code in `Main` ein: [!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/from-blob/src/quickstart/Main.java?range=115-116)]

## <a name="send-the-request-and-check-its-status"></a>Senden der Anforderung und Überprüfen ihres Status
Nun senden wir die Anforderung an den Speech-Dienst und überprüfen den ersten Antwortcode. Mit diesem Antwortcode wird lediglich angegeben, ob der Dienst die Anforderung erhalten hat. Der Dienst gibt in den Antwortheadern eine URL zurück, die den Speicherort des Transkriptionsstatus angibt.
[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/from-blob/src/quickstart/Main.java?range=118-129)]

## <a name="wait-for-the-transcription-to-complete"></a>Warten auf den Abschluss der Transkription
Da die Transkription vom Dienst asynchron verarbeitet wird, müssen wir regelmäßig den Status abrufen. Wir führen alle fünf Sekunden eine Überprüfung durch.

Wir können den Status überprüfen, indem wir den Inhalt unter der URL abrufen, die wir beim Senden der Anforderung erhalten haben. Wenn wir den Inhalt zurückerhalten, führen wir dafür eine Deserialisierung in eine unserer Hilfsklassen durch, um die Interaktion zu vereinfachen.

Hier ist der Abrufcode mit der Statusanzeige für alle Vorgänge angegeben (mit Ausnahme eines erfolgreichen Abschlusses). Das kommt als Nächstes.
[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/from-blob/src/quickstart/Main.java?range=131-159,192-204)]

## <a name="display-the-transcription-results"></a>Anzeigen der Transkriptionsergebnisse
Nachdem der Dienst die Transkription erfolgreich abgeschlossen hat, werden die Ergebnisse unter einer anderen URL gespeichert, die wir in der Statusantwort erhalten.

Wir laden den Inhalt dieser URL herunter, deserialisieren den JSON-Code und durchlaufen die Ergebnisse in einer Schleife, wobei wir den Anzeigetext ausgeben.
[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/from-blob/src/quickstart/Main.java?range=6-160-190)]

## <a name="check-your-code"></a>Überprüfen Ihres Codes
Ihr Code sollte nun wie folgt aussehen: (Wir haben dieser Version einige Kommentare hinzugefügt.) [!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/from-blob/src/quickstart/Main.java]

## <a name="build-and-run-your-app"></a>Erstellen und Ausführen Ihrer App

Nun können Sie Ihre App erstellen und die Spracherkennung mit dem Speech-Dienst testen.

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [footer](./footer.md)]
