---
title: 'Schnellstart: Synthetisieren von Sprache, C++ (Windows): Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie mit dem Speech SDK Sprache in C++ unter Windows Desktop synthetisieren.
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: yinhew
ms.openlocfilehash: fdc5e540dd7e84d47aa139768fbfc2614f38b376
ms.sourcegitcommit: 6cff17b02b65388ac90ef3757bf04c6d8ed3db03
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2019
ms.locfileid: "68607716"
---
# <a name="quickstart-synthesize-speech-in-c-on-windows-by-using-the-speech-sdk"></a>Schnellstart: Synthetisieren von Sprache in C++ unter Windows mit dem Speech SDK

Schnellstarts sind auch für [Spracherkennung](quickstart-cpp-windows.md) und [Sprachübersetzung](quickstart-translate-speech-cpp-windows.md) verfügbar.

In diesem Artikel erstellen Sie eine C++-Konsolenanwendung für Windows. Mit dem [Speech SDK](speech-sdk.md) von Cognitive Services können Sie Text in Echtzeit in Sprache synthetisieren und das Ergebnis über den Lautsprecher Ihres PCs ausgeben. Die Anwendung basiert auf dem [NuGet-Paket für das Speech SDK](https://aka.ms/csspeech/nuget) und auf Microsoft Visual Studio 2017 oder höher (beliebige Edition).

Das in diesem Artikel beschriebene Feature steht ab dem [Speech SDK 1.5.0](https://www.nuget.org/packages/Microsoft.CognitiveServices.Speech/1.5.0) zur Verfügung.

Eine vollständige Liste mit den verfügbaren Sprachen/Stimmen für die Sprachsynthese finden Sie auf der Seite zur [Sprachunterstützung](language-support.md#text-to-speech).

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung benötigen Sie einen Abonnementschlüssel für die Speech-Dienste. Sie können einen solchen Schlüssel kostenlos abrufen. Ausführliche Informationen finden Sie unter [Kostenloses Testen der Spracherkennungsdienste](get-started.md).

## <a name="create-a-visual-studio-project"></a>Erstellen eines Visual Studio-Projekts

[!INCLUDE [Quickstart C++ project](../../../includes/cognitive-services-speech-service-quickstart-cpp-create-proj.md)]

## <a name="add-sample-code"></a>Hinzufügen von Beispielcode

1. Öffnen Sie die Quelldatei *helloworld.cpp*. Ersetzen Sie den gesamten Code unter der ersten include-Anweisung (`#include "stdafx.h"` oder `#include "pch.h"`) durch Folgendes:

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/cpp-windows/helloworld/helloworld.cpp#code)]

1. Ersetzen Sie in der gleichen Datei die Zeichenfolge `YourSubscriptionKey` durch Ihren Abonnementschlüssel.

1. Ersetzen Sie die Zeichenfolge `YourServiceRegion` durch die [Region](regions.md), die mit Ihrem Abonnement verknüpft ist (z. B. `westus` für das kostenlose Testabonnement).

1. Speichern Sie die Änderungen am Projekt.

## <a name="build-and-run-the-app"></a>Erstellen und Ausführen der App

1. Erstellen Sie die Anwendung. Wählen Sie in der Menüleiste **Erstellen** > **Projektmappe erstellen** aus. Der Code sollte ohne Fehler kompiliert werden.

   ![Screenshot der Visual Studio-Anwendung mit hervorgehobener Option „Projektmappe erstellen“](media/sdk/qs-cpp-windows-06-build.png)

1. Starten Sie die Anwendung. Wählen Sie in der Menüleiste **Debuggen** > **Debuggen starten** aus, oder drücken Sie **F5**.

   ![Screenshot der Visual Studio-Anwendung mit hervorgehobener Option „Debuggen starten“](media/sdk/qs-cpp-windows-07-start-debugging.png)

1. Ein Konsolenfenster mit einer Eingabeaufforderung wird angezeigt. Geben Sie einige Wörter oder einen Satz ein. Der eingegebene Text wird an die Speech-Dienste übermittelt und in Sprache synthetisiert, die dann über Ihren Lautsprecher ausgegeben wird.

   ![Screenshot: Konsolenausgabe nach erfolgreicher Synthese](media/sdk/qs-tts-cpp-windows-console-output.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Beispiele (etwa zum Speichern von Sprache aus einer Audiodatei), sind auf GitHub verfügbar.

> [!div class="nextstepaction"]
> [C++-Beispiele auf GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Weitere Informationen

- [Anpassen von Voicefonts](how-to-customize-voice-font.md)
- [Aufnehmen von Sprachbeispielen](record-custom-voice-samples.md)
