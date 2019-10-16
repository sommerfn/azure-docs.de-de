---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 09/13/2019
ms.author: erhopf
ms.openlocfilehash: 0e4e67710c98b80dce2b0d55a86869625f3942d2
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/03/2019
ms.locfileid: "71837421"
---
Zum Erstellen eines Visual Studio-Projekts für die plattformübergreifende Mobile App-.NET-Entwicklung mit Xamarin müssen Sie Visual Studio-Entwicklungsoptionen einrichten, das Projekt erstellen, die Zielarchitektur auswählen und das Speech SDK installieren.

### <a name="set-up-visual-studio-development-options"></a>Einrichten von Visual Studio-Entwicklungsoptionen

Stellen Sie zunächst sicher, dass Visual Studio ordnungsgemäß für die plattformübergreifende Mobile-Entwicklung mit .NET eingerichtet ist:

1. Öffnen Sie Visual Studio 2019.

1. Wählen Sie auf der Visual Studio-Menüleiste **Extras** > **Tools und Features abrufen** aus, um den Visual Studio-Installer zu öffnen und das Dialogfeld **Änderung** anzuzeigen.

   ![Registerkarte „Workloads“, Dialogfeld „Änderung“, Visual Studio-Installer](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-xamarin-workload.png)

1. Suchen Sie auf der Registerkarte **Workloads** unter **Windows** nach der Workload **Mobile-Entwicklung mit .NET**. Wenn das Kontrollkästchen neben dieser Workload bereits aktiviert ist, schließen Sie das Dialogfeld **Ändern**, und fahren Sie mit Schritt 5 fort.

1. Aktivieren Sie das Kontrollkästchen **Mobile-Entwicklung mit .NET**, und wählen Sie **Ändern** und dann im Dialogfeld **Bevor wir loslegen** die Option **Weiter** aus, um die Workload für Mobile-Entwicklung mit .NET zu installieren. Die Installation des neuen Features kann etwas Zeit in Anspruch nehmen.

1. Schließen Sie den Visual Studio-Installer.

### <a name="create-the-project"></a>Erstellen des Projekts

1. Wählen Sie in der Visual Studio-Menüleiste **Datei** > **Neu** > **Projekt** aus, um das Fenster **Neues Projekt erstellen** anzuzeigen.

   ![Erstellen eines neuen Projekts: Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-xamarin-create-new-project.png)

1. Suchen Sie nach **Mobile App (Xamarin Forms)** .

1. Wählen Sie **Weiter** aus, um den Bildschirm **Neues Projekt konfigurieren** anzuzeigen. 

   ![Konfigurieren Ihres neuen Projekts: Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-xamarin-configure-your-new-project.png)

1. Geben Sie unter **Projektname** die Zeichenfolge `helloworld` ein.

1. Navigieren Sie unter **Speicherort** zum Ordner, in dem das Projekt gespeichert werden soll, oder erstellen Sie einen.

1. Wählen Sie **Erstellen** aus, um zum Fenster **New Mobile App Xamarin Forms Project** (Neues Mobile App-Xamarin Forms-Projekt) zu navigieren.

   ![Dialogfeld „Neues UWP-Projekt (Universelle Windows-Plattform)“: Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-xamarin-new-xamarin-project.png)

1. Wählen Sie die Vorlage **Leer** aus.

1. Aktivieren Sie unter **Plattform** die Kontrollkästchen für **Android**, **iOS** und **Windows (UWP)** .

1. Klicken Sie auf **OK**. Sie werden in die Visual Studio-IDE zurückgeleitet. Das neue Projekt ist erstellt und wird im Bereich **Projektmappen-Explorer** angezeigt.

   ![helloworld-Projekt: Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-xamarin-helloworld.png)

Wählen Sie nun die Architektur Ihrer Zielplattform und das Startprojekt aus. Suchen Sie auf der Visual Studio-Symbolleiste das Dropdownfeld **Projektmappenplattformen**. (Wenn Sie es nicht sehen, wählen Sie **Ansicht** > **Symbolleisten** > **Standard** aus, um die Symbolleiste mit **Projektmappenplattformen** anzuzeigen.) Wenn Sie 64-Bit-Windows ausführen, wählen Sie im Dropdownfeld **x64** aus. Mit 64-Bit-Windows können auch 32-Bit-Anwendungen ausgeführt werden, sodass Sie **x86** wählen können, wenn Sie möchten. Legen Sie im Dropdownfeld **Start-up Projects** (Startprojekte) die Option „helloworld.UWP (Universal Windows)“ fest.

### <a name="install-the-speech-sdk"></a>Installieren des Speech SDK

Installieren Sie das [Speech SDK-NuGet-Paket](https://aka.ms/csspeech/nuget), und verweisen Sie in Ihrem Projekt auf das Speech SDK:

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf die Projektmappe, und wählen Sie dann **NuGet-Pakete für Projektmappe verwalten**, um zum Fenster **NuGet – Projektmappe** zu wechseln.

1. Wählen Sie **Durchsuchen**.

   ![Screenshot des Dialogfelds „Pakete für Projektmappe verwalten“](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-uwp-nuget-solution-browse.png)

1. Wählen Sie unter **Paketquelle** die Option **nuget.org** aus.

1. Geben Sie `Microsoft.CognitiveServices.Speech` in das **Suchfeld** ein, und wählen Sie dann das Paket aus, nachdem es in den Suchergebnissen angezeigt wird.

   ![Screenshot des Dialogfelds „Pakete für Projektmappe verwalten“](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-xamarin-nuget-install.png)
   > Hinweis: Bitcode ist für die iOS-Bibliothek im NuGet-Paket „Microsoft.CognitiveServices.Speech“ nicht aktiviert. Falls Sie für Ihre Anwendung eine Bibliothek mit Bitcodeaktivierung benötigen, verwenden Sie das NuGet-Paket „Microsoft.CognitiveServices.Speech.Xamarin.iOS“ speziell für das iOS-Projekt.

1. Wählen Sie im Statusbereich des Pakets neben den Suchergebnissen alle Projekte aus. **helloworld**, **helloworld.Android**, **helloworld.iOS** und **helloworld.UWP**.

1. Wählen Sie **Installieren** aus.

1. Wählen Sie im Dialogfeld **Vorschau der Änderungen anzeigen** die Option **OK**.

1. Zeigen Sie im Dialogfeld **Zustimmung zur Lizenz** die Lizenz an, wählen Sie **Ich stimme zu** aus, und installieren Sie den Speech SDK-Paketverweis für alle Projekte. Nach erfolgreichem Abschluss der Installation wird für „helloworld.iOS“ unter Umständen die folgende Warnung angezeigt. Dies ist ein bekanntes Problem, das sich nicht auf die App-Funktionalität auswirken sollte.

> Der Verweis „C:\Users\Default\.nuget\packages\microsoft.cognitiveservices.speech\1.7.0\build\Xamarin.iOS\libMicrosoft.CognitiveServices.Speech.core.a“ konnte nicht aufgelöst werden. Ist dieser Verweis für Ihren Code erforderlich, treten unter Umständen Kompilierungsfehler auf.
