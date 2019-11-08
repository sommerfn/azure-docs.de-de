---
title: 'Schnellstart: Speech SDK für .NET Framework (Windows): Plattformeinrichtung – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: Verwenden Sie diesen Leitfaden, um Ihre Plattform mit dem Speech Services SDK für C# unter .NET Framework für Windows einzurichten.
services: cognitive-services
author: markamos
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 10/10/2019
ms.author: erhopf
ms.openlocfilehash: 2caa5124144146e2286b102bf6ded9c5d120f904
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73504427"
---
In diesem Leitfaden erfahren Sie, wie Sie das [Speech SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) für .NET Framework (Windows) installieren.

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung ist Folgendes erforderlich:

* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/)

## <a name="create-a-visual-studio-project-and-install-the-speech-sdk"></a>Erstellen eines Visual Studio-Projekts und Installieren des Speech SDK

Sie müssen das [NuGet-Paket für das Speech SDK](https://aka.ms/csspeech/nuget) installieren, um in Ihrem Code darauf verweisen zu können. Hierzu muss ggf. zuerst ein Projekt vom Typ **helloworld** erstellt werden. Falls Sie bereits über ein Projekt mit der Workload **.NET-Desktopentwicklung** verfügen, können Sie dieses Projekt verwenden und direkt mit [Installieren des Speech SDK mithilfe des NuGet-Paket-Managers](#use-nuget-package-manager-to-install-the-speech-sdk) fortfahren.

### <a name="create-helloworld-project"></a>Erstellen eines helloworld-Projekts

1. Öffnen Sie Visual Studio 2019.

1. Wählen Sie im Fenster „Start“ die Option **Neues Projekt erstellen** aus. 

1. Wählen Sie im Fenster **Neues Projekt erstellen** die Option **Konsolen-App (.NET Framework)** und dann **Weiter** aus.

1. Geben Sie im Fenster **Neues Projekt konfigurieren** unter **Projektname** den Namen *helloworld* ein, und wählen Sie unter **Speicherort** den Verzeichnispfad aus, oder erstellen Sie ihn. Wählen Sie anschließend **Erstellen** aus.

1. Wählen Sie auf der Visual Studio-Menüleiste **Extras** > **Tools und Features abrufen** aus, um den Visual Studio-Installer zu öffnen und das Dialogfeld **Änderung** anzuzeigen.

1. Überprüfen Sie, ob die Workload **.NET-Desktopentwicklung** verfügbar ist. Wurde die Workload noch nicht installiert, aktivieren Sie das Kontrollkästchen daneben, und wählen Sie dann **Ändern** aus, um die Installation zu starten. Der Download und die Installation können einige Minuten dauern.

   Ist das Kontrollkästchen neben **.NET-Desktopentwicklung** bereits aktiviert, wählen Sie **Schließen** aus, um das Dialogfeld zu schließen.

   ![Aktivieren der .NET-Desktopentwicklung](~/articles/cognitive-services/speech-service/media/sdk/vs-enable-net-desktop-workload.png)

1. Schließen Sie den Visual Studio-Installer.

### <a name="use-nuget-package-manager-to-install-the-speech-sdk"></a>Installieren des Speech SDK mithilfe des NuGet-Paket-Managers

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **helloworld**, und wählen Sie dann **NuGet-Pakete verwalten** aus, um den NuGet-Paket-Manager anzuzeigen.

   ![NuGet-Paket-Manager](~/articles/cognitive-services/speech-service/media/sdk/vs-nuget-package-manager.png)

1. Vergewissern Sie sich rechts oben im Dropdownfeld **Paketquelle**, dass **`nuget.org`** ausgewählt ist.

1. Wählen Sie in der oberen linken Ecke **Durchsuchen** aus.

1. Geben Sie ins Suchfeld *Microsoft.CognitiveServices.Speech* ein, und drücken Sie die **EINGABETASTE**.

1. Wählen Sie in den Suchergebnissen das Paket **Microsoft.CognitiveServices.Speech** und anschließend **Installieren** aus, um die aktuelle stabile Version zu installieren.

   ![Installieren des NuGet-Pakets Microsoft.CognitiveServices.Speech](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-03-nuget-install-1.0.0.png)

1. Akzeptieren Sie alle Vereinbarungen und Lizenzen, um die Installation zu starten.

   Nachdem das Paket installiert wurde, wird eine Bestätigung im Fenster **Paket-Manager-Konsole** angezeigt.

Nun können Sie mit den [nächsten Schritten](#next-steps) fortfahren.

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [windows](../quickstart-list.md)]
