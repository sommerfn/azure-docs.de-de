---
title: 'Schnellstart: Übersetzen von Sprache, C# (.NET Framework, Windows) – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung erstellen Sie eine .NET Framework-Anwendung zum Erfassen der Benutzersprache, Übersetzen in eine andere Sprache und Ausgeben des Texts in der Befehlszeile. Dieser Leitfaden ist für Windows-Benutzer bestimmt.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 08/28/2019
ms.author: erhopf
ms.openlocfilehash: bb710a3e3adb13aa3999c13043c8bb93f6b885f1
ms.sourcegitcommit: e9936171586b8d04b67457789ae7d530ec8deebe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71327342"
---
# <a name="quickstart-translate-speech-with-the-speech-sdk-for-net-framework-windows"></a>Schnellstart: Übersetzen von Sprache mit dem Speech SDK für .NET Framework (Windows)

Schnellstarts sind auch für [Spracherkennung](quickstart-csharp-dotnet-windows.md) und [Sprachsynthese](quickstart-text-to-speech-dotnet-windows.md) verfügbar.

In dieser Schnellstartanleitung erstellen Sie eine .NET Framework-Anwendung, mit der die Spracheingabe des Benutzers über das Mikrofon Ihres Computers erfasst, die Spracheingabe übersetzt und der übersetzte Text in Echtzeit in der Befehlszeile transkribiert wird. Diese Anwendung kann unter Windows (32 Bit oder 64 Bit) ausgeführt werden und basiert auf dem [NuGet-Paket für das Speech SDK](https://aka.ms/csspeech/nuget) und Microsoft Visual Studio 2019.

Eine vollständige Liste mit den verfügbaren Sprachen für die Sprachübersetzung finden Sie auf der Seite zur [Sprachunterstützung](language-support.md).

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung ist Folgendes erforderlich:

* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/).
* Ein Azure-Abonnementschlüssel für den Speech-Dienst. [Hier erhalten Sie einen kostenlosen Schlüssel.](get-started.md)

## <a name="create-a-visual-studio-project"></a>Erstellen eines Visual Studio-Projekts

[!INCLUDE [Create project](../../../includes/cognitive-services-speech-service-create-speech-project-vs-csharp.md)]

## <a name="add-sample-code"></a>Hinzufügen von Beispielcode

1. Öffnen Sie die Datei **Program.cs**, und ersetzen Sie den gesamten vorhandenen Code durch folgenden Code:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/speech-translation/csharp-dotnet-windows/helloworld/Program.cs#code)]

1. Suchen Sie nach der Zeichenfolge `YourSubscriptionKey`, und ersetzen Sie sie durch Ihren Abonnementschlüssel.

1. Suchen Sie nach der Zeichenfolge `YourServiceRegion`, und ersetzen Sie sie durch die [Region](regions.md), die mit Ihrem Abonnement verknüpft ist. Bei Verwendung des kostenlosen Testabonnements ist die Region beispielsweise `westus`.

1. Wählen Sie auf der Menüleiste **Datei** > **Alle speichern** aus.

## <a name="build-and-run-the-application"></a>Erstellen und Ausführen der Anwendung

1. Wählen Sie auf der Menüleiste **Erstellen** > **Projektmappe erstellen** aus, um die Anwendung zu erstellen. Der Code sollte nun ohne Fehler kompiliert werden.

1. Wählen Sie **Debuggen** > **Debuggen starten** aus (oder drücken Sie**F5**), um die Anwendung **helloworld** zu starten.

1. Sprechen Sie einen englischen Ausdruck oder Satz in das Mikrofon Ihres Geräts. Die Anwendung überträgt ihn an die Speech-Dienste, die ihn in Text in einer anderen Sprache (in diesem Fall: Deutsch) übersetzen. Der Speech-Dienst sendet den übersetzten Text zurück an die Anwendung, die die Übersetzung im Fenster anzeigt.

   ![Benutzeroberfläche der Sprachübersetzung](media/sdk/qs-translate-csharp-dotnetcore-windows-output.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Beispiele, z.B. zum Auslesen von Sprache aus einer Audiodatei und Ausgeben von übersetztem Text als synthetisierte Sprache, sind auf GitHub verfügbar.

> [!div class="nextstepaction"]
> [C#-Beispiele auf GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Weitere Informationen

- [Trainieren eines Modells für Custom Speech](how-to-custom-speech-train-model.md)
