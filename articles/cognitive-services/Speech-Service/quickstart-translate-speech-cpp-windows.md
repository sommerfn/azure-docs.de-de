---
title: 'Schnellstart: Übersetzen von Sprache, C++ (Windows): Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung erstellen Sie eine einfache C++-Anwendung zum Erfassen der Benutzersprache, Übersetzen in eine andere Sprache und Ausgeben des Texts in der Befehlszeile. Dieser Leitfaden ist für Windows-Benutzer bestimmt.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: 379087ca94eee6ce3d45bfd97b4771c5f08d6333
ms.sourcegitcommit: 6cff17b02b65388ac90ef3757bf04c6d8ed3db03
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2019
ms.locfileid: "68607699"
---
# <a name="quickstart-translate-speech-with-the-speech-sdk-for-c"></a>Schnellstart: Übersetzen von Sprache mit dem Speech-SDK für C++

Schnellstarts sind auch für [Spracherkennung](quickstart-cpp-windows.md) und [Sprachsynthese](quickstart-text-to-speech-cpp-windows.md) verfügbar.

In dieser Schnellstartanleitung erstellen Sie eine einfache C++-Anwendung, mit die Spracheingabe des Benutzers über das Mikrofon Ihres Computers erfasst, die Sprache übersetzt und der übersetzte Text in Echtzeit in der Befehlszeile transkribiert wird. Diese Anwendung ist für die Ausführung unter Windows (64 Bit) konzipiert und basiert auf dem [NuGet-Paket des Speech SDK](https://aka.ms/csspeech/nuget) und Microsoft Visual Studio 2017 oder höher.

Eine vollständige Liste mit den verfügbaren Sprachen für die Sprachübersetzung finden Sie auf der Seite zur [Sprachunterstützung](language-support.md).

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung ist Folgendes erforderlich:

* Mindestens [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Ein Azure-Abonnementschlüssel für den Speech-Dienst. [Hier erhalten Sie einen kostenlosen Schlüssel.](get-started.md)

## <a name="create-a-visual-studio-project"></a>Erstellen eines Visual Studio-Projekts

[!INCLUDE [Quickstart C++ project](../../../includes/cognitive-services-speech-service-quickstart-cpp-create-proj.md)]

## <a name="add-sample-code"></a>Hinzufügen von Beispielcode

1. Öffnen Sie die Quelldatei *helloworld.cpp*. Ersetzen Sie den gesamten Code unter der ersten include-Anweisung (`#include "stdafx.h"` oder `#include "pch.h"`) durch Folgendes:

    [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/speech-translation/cpp-windows/helloworld/helloworld.cpp#code)]

1. Ersetzen Sie in der gleichen Datei die Zeichenfolge `YourSubscriptionKey` durch Ihren Abonnementschlüssel.

1. Ersetzen Sie die Zeichenfolge `YourServiceRegion` durch die [Region](regions.md), die mit Ihrem Abonnement verknüpft ist (z. B. `westus` für das kostenlose Testabonnement).

1. Speichern Sie die Änderungen am Projekt.

## <a name="build-and-run-the-app"></a>Erstellen und Ausführen der App

1. Erstellen Sie die Anwendung. Wählen Sie in der Menüleiste **Erstellen** > **Projektmappe erstellen** aus. Der Code sollte ohne Fehler kompiliert werden.

   ![Screenshot der Visual Studio-Anwendung mit hervorgehobener Option „Projektmappe erstellen“](media/sdk/qs-cpp-windows-06-build.png)

1. Starten Sie die Anwendung. Wählen Sie in der Menüleiste **Debuggen** > **Debuggen starten** aus, oder drücken Sie **F5**.

   ![Screenshot der Visual Studio-Anwendung mit hervorgehobener Option „Debuggen starten“](media/sdk/qs-cpp-windows-07-start-debugging.png)

1. Ein Konsolenfenster wird angezeigt, in dem Sie aufgefordert werden, etwas zu sagen. Sprechen Sie einen englischen Ausdruck oder Satz. Ihre Spracheingabe wird an den Spracherkennungsdienst übermittelt, übersetzt und in Text transkribiert, der in demselben Fenster angezeigt wird.

   ![Screenshot: Konsolenausgabe nach erfolgreicher Übersetzung](media/sdk/qs-translate-cpp-windows-output.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Beispiele, z.B. zum Auslesen von Sprache aus einer Audiodatei und Ausgeben von übersetztem Text als synthetisierte Sprache, sind auf GitHub verfügbar.

> [!div class="nextstepaction"]
> [C++-Beispiele auf GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Weitere Informationen

- [Tutorial: Erstellen eines benutzerdefinierten Akustikmodells](how-to-customize-acoustic-models.md)
- [Tutorial: Erstellen eines benutzerdefinierten Sprachmodells](how-to-customize-language-model.md)
