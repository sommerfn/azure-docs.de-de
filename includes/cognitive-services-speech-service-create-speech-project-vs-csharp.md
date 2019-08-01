---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 07/05/2019
ms.author: wolfma
ms.openlocfilehash: 308ee2ef121648cb45152948926c5fd7fb934744
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/19/2019
ms.locfileid: "68362512"
---
1. Öffnen Sie Visual Studio 2019.

1. Wählen Sie im Fenster „Start“ die Option **Neues Projekt erstellen** aus. 

1. Wählen Sie **Konsolen-App (.NET Framework)** und dann **Weiter** aus.

1. Geben Sie unter **Projektname** `helloworld` ein, und wählen Sie dann **Erstellen** aus.

1. Wählen Sie auf der Menüleiste in Visual Studio **Tools** > **Tools und Features abrufen** aus, und prüfen Sie, ob die Workload **.NET-Desktopentwicklung** verfügbar ist. Wurde die Workload noch nicht installiert, aktivieren Sie das Kontrollkästchen, und wählen Sie dann **Ändern** aus, um die Installation zu starten. Der Download und die Installation können einige Minuten dauern.

   Ist das Kontrollkästchen neben **.NET-Desktopentwicklung** aktiviert, können Sie das Dialogfeld jetzt schließen.

   ![Aktivieren der .NET-Desktopentwicklung](../articles/cognitive-services/speech-service/media/sdk/vs-enable-net-desktop-workload.png)

Der nächste Schritt ist die Installation des [Speech-SDK-NuGet-Pakets](https://aka.ms/csspeech/nuget), damit Sie im Code darauf verweisen können.

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf `helloworld`, und wählen Sie dann **NuGet-Pakete verwalten**, um den NuGet-Paket-Manager anzuzeigen.

   ![NuGet-Paket-Manager](../articles/cognitive-services/speech-service/media/sdk/vs-nuget-package-manager.png)

1. Navigieren Sie oben rechts zum Dropdownfeld **Paketquelle**, und stellen Sie sicher, dass **nuget.org** ausgewählt ist.

1. Wählen Sie in der oberen linken Ecke **Durchsuchen** aus.

1. Geben Sie im Suchfeld `Microsoft.CognitiveServices.Speech`-Paket ein, und drücken Sie die EINGABETASTE.

1. Wählen Sie `Microsoft.CognitiveServices.Speech` und dann **Installieren** aus, um die neueste stabile Version zu installieren.

   ![Installieren des NuGet-Pakets Microsoft.CognitiveServices.Speech](../articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-03-nuget-install-1.0.0.png)

1. Akzeptieren Sie alle Vereinbarungen und Lizenzen, um die Installation zu starten.

   Nachdem das Paket installiert wurde, wird eine Bestätigung im Fenster **Paket-Manager-Konsole** angezeigt.

Erstellen Sie jetzt eine Plattformkonfiguration, die der Architektur Ihres Computers entspricht, um die Konsolenanwendung zu erstellen und auszuführen.

1. Wählen Sie auf der Menüleiste **Build** > **Konfigurations-Manager** aus. Das Dialogfeld **Konfigurations-Manager** wird angezeigt.

   ![Dialogfeld „Konfigurations-Manager“](../articles/cognitive-services/speech-service/media/sdk/vs-configuration-manager-dialog-box.png)

1. Wählen Sie im Dropdownfeld **Aktive Projektmappenplattform** den Eintrag **Neu** aus. Das Dialogfeld **Neue Projektmappenplattform** wird angezeigt.

1. Gehen Sie im Feld **Neue Plattform eingeben oder auswählen** wie folgt vor:
   - Wenn Sie 64-Bit-Windows ausführen, wählen Sie **x64** aus.
   - Wenn Sie 32-Bit-Windows ausführen, wählen Sie **x86** aus.

1. Wählen Sie **OK** und dann **Schließen** aus.
