---
title: 'Schnellstart: Erkennen von Sprache, C++ (Windows): Speech-Dienste'
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie mit dem Speech SDK Sprache in C++ unter Windows Desktop erkennen.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: wolfma
ms.openlocfilehash: d63d1c96077642e660e2272cbd8c2ee1250b1471
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2019
ms.locfileid: "67606489"
---
# <a name="quickstart-recognize-speech-in-c-on-windows-by-using-the-speech-sdk"></a>Schnellstart: Erkennen von Sprache in C++ unter Windows mit dem Speech SDK

Schnellstarts sind auch für [Sprachsynthese](quickstart-text-to-speech-cpp-windows.md) und [Sprachübersetzung](quickstart-translate-speech-cpp-windows.md) verfügbar.

Wählen Sie bei Bedarf eine andere Programmiersprache und/oder Umgebung:<br/>
[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

In diesem Artikel erstellen Sie eine C++-Konsolenanwendung für Windows. Sie verwenden das Cognitive Services [Speech SDK](speech-sdk.md), um Sprache über das Mikrofon Ihres Computers in Echtzeit in Text zu transkribieren. Die Anwendung basiert auf dem [NuGet-Paket für das Speech SDK](https://aka.ms/csspeech/nuget) und auf Microsoft Visual Studio 2017 (beliebige Edition).

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung benötigen Sie einen Abonnementschlüssel für die Speech-Dienste. Sie können einen solchen Schlüssel kostenlos abrufen. Ausführliche Informationen finden Sie unter [Kostenloses Testen der Spracherkennungsdienste](get-started.md).

## <a name="create-a-visual-studio-project"></a>Erstellen eines Visual Studio-Projekts

[!INCLUDE [Quickstart C++ project](../../../includes/cognitive-services-speech-service-quickstart-cpp-create-proj.md)]

## <a name="add-sample-code"></a>Hinzufügen von Beispielcode

1. Öffnen Sie die Quelldatei *helloworld.cpp*. Ersetzen Sie den gesamten Code unter der ersten include-Anweisung (`#include "stdafx.h"` oder `#include "pch.h"`) durch Folgendes:

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/cpp-windows/helloworld/helloworld.cpp#code)]

1. Ersetzen Sie in der gleichen Datei die Zeichenfolge `YourSubscriptionKey` durch Ihren Abonnementschlüssel.

1. Ersetzen Sie die Zeichenfolge `YourServiceRegion` durch die [Region](regions.md), die mit Ihrem Abonnement verknüpft ist (z. B. `westus` für das kostenlose Testabonnement).

1. Speichern Sie die Änderungen am Projekt.

## <a name="build-and-run-the-app"></a>Erstellen und Ausführen der App

1. Erstellen Sie die Anwendung. Wählen Sie in der Menüleiste **Erstellen** > **Projektmappe erstellen** aus. Der Code sollte ohne Fehler kompiliert werden.

   ![Screenshot der Visual Studio-Anwendung mit hervorgehobener Option „Projektmappe erstellen“](media/sdk/qs-cpp-windows-06-build.png)

1. Starten Sie die Anwendung. Wählen Sie in der Menüleiste **Debuggen** > **Debuggen starten** aus, oder drücken Sie **F5**.

   ![Screenshot der Visual Studio-Anwendung mit hervorgehobener Option „Debuggen starten“](media/sdk/qs-cpp-windows-07-start-debugging.png)

1. Ein Konsolenfenster wird angezeigt, in dem Sie aufgefordert werden, etwas zu sagen. Sprechen Sie einen englischen Ausdruck oder Satz. Ihre Spracheingabe wird an den Spracherkennungsdienst übermittelt und in Text transkribiert, der im selben Fenster angezeigt wird.

   ![Screenshot der Konsolenausgabe nach erfolgreicher Erkennung](media/sdk/qs-cpp-windows-08-console-output-release.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Beispiele, etwa das Lesen von Sprache aus einer Audiodatei, sind auf GitHub verfügbar.

> [!div class="nextstepaction"]
> [C++-Beispiele auf GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Weitere Informationen

- [Tutorial: Erstellen eines benutzerdefinierten Akustikmodells](how-to-customize-acoustic-models.md)
- [Tutorial: Erstellen eines benutzerdefinierten Sprachmodells](how-to-customize-language-model.md)
