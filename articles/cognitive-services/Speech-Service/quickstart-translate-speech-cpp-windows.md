---
title: 'Schnellstart: Übersetzen von Sprache, C++ (Windows): Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung erstellen Sie eine C++-Anwendung zum Erfassen der Benutzersprache, Übersetzen in eine andere Sprache und Ausgeben des Texts in der Befehlszeile. Dieser Leitfaden ist für Windows-Benutzer bestimmt.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 08/24/2019
ms.author: erhopf
ms.openlocfilehash: 6ba4e44efc7ff24aa48f9f16840b2248423f7241
ms.sourcegitcommit: 49c4b9c797c09c92632d7cedfec0ac1cf783631b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/05/2019
ms.locfileid: "70382678"
---
# <a name="quickstart-translate-speech-in-c-on-windows-by-using-the-speech-sdk"></a>Schnellstart: Übersetzen von Sprache in C++ unter Windows mit dem Speech SDK

Schnellstarts sind auch für [Spracherkennung](quickstart-cpp-windows.md) und [Sprachsynthese](quickstart-text-to-speech-cpp-windows.md) verfügbar.

In dieser Schnellstartanleitung erstellen Sie eine C++-Anwendung, mit der die Spracheingabe des Benutzers über das Mikrofon Ihres Computers erfasst, die Sprache übersetzt und der übersetzte Text in Echtzeit in der Befehlszeile transkribiert wird. Diese Anwendung basiert auf dem [NuGet-Paket für das Speech SDK](https://aka.ms/csspeech/nuget) und auf Microsoft Visual Studio 2019 (beliebige Edition).

Eine vollständige Liste mit den verfügbaren Sprachen für die Sprachübersetzung finden Sie auf der Seite zur [Sprachunterstützung](language-support.md).

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung benötigen Sie einen Abonnementschlüssel für die Speech-Dienste. Sie können einen solchen Schlüssel kostenlos abrufen. Ausführliche Informationen finden Sie unter [Kostenloses Testen der Spracherkennungsdienste](get-started.md).

## <a name="create-a-visual-studio-project"></a>Erstellen eines Visual Studio-Projekts

[!INCLUDE [Quickstart C++ project](../../../includes/cognitive-services-speech-service-quickstart-cpp-create-proj.md)]

## <a name="add-sample-code"></a>Hinzufügen von Beispielcode

1. Öffnen Sie die Quelldatei **helloworld.cpp**.

1. Ersetzen Sie den gesamten Code durch den folgenden Codeausschnitt:

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/speech-translation/cpp-windows/helloworld/helloworld.cpp?range=2-#code)]

1. Ersetzen Sie in der gleichen Datei die Zeichenfolge `YourSubscriptionKey` durch Ihren Abonnementschlüssel.

1. Ersetzen Sie die Zeichenfolge `YourServiceRegion` durch die [Region](regions.md), die mit Ihrem Abonnement verknüpft ist (z. B. `westus` für das kostenlose Testabonnement).

1. Wählen Sie auf der Menüleiste **Datei** > **Alle speichern** aus.

## <a name="build-and-run-the-application"></a>Erstellen und Ausführen der Anwendung

1. Wählen Sie auf der Menüleiste **Erstellen** > **Projektmappe erstellen** aus, um die Anwendung zu erstellen. Der Code sollte nun ohne Fehler kompiliert werden.

1. Wählen Sie **Debuggen** > **Debuggen starten** aus (oder drücken Sie**F5**), um die Anwendung **helloworld** zu starten.

1. Sprechen Sie einen englischen Ausdruck oder Satz. Die Anwendung überträgt ihn an die Speech-Dienste, die den Text übersetzen und transkribieren (in diesem Fall ins Französische und Deutsche). Die Speech-Dienste senden den Text dann zum Anzeigen zurück an die Anwendung.

   ![Konsolenausgabe nach erfolgreicher Sprachübersetzung](media/sdk/qs-translate-cpp-windows-output.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Beispiele, etwa zum Auslesen von Sprache aus einer Audiodatei und Konvertieren von übersetztem Text in synthetisierte Sprache, sind auf GitHub verfügbar.

> [!div class="nextstepaction"]
> [C++-Beispiele auf GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Weitere Informationen

- [Trainieren eines Modells für Custom Speech](how-to-custom-speech-train-model.md)
