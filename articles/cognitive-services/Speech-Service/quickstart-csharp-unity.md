---
title: 'Schnellstart: Erkennen von Sprache, Unity – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: In dieser Anleitung erfahren Sie, wie Sie mit Unity und dem Speech SDK für Unity (Beta) eine Spracherkennungsanwendung erstellen. Danach können Sie das Mikrofon Ihres Computers verwenden, um Sprache in Echtzeit zu transkribieren.
services: cognitive-services
author: jhakulin
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/24/2019
ms.author: jhakulin
ms.openlocfilehash: 1b6e60edd86cff2d657b562f05351e20571c0909
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2019
ms.locfileid: "68815403"
---
# <a name="quickstart-recognize-speech-with-the-speech-sdk-for-unity-beta"></a>Schnellstart: Erkennen von Sprache mit dem Speech SDK für Unity (Beta)

Schnellstarts sind auch für [Sprachsynthese](quickstart-text-to-speech-csharp-unity.md) verfügbar.

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

In dieser Anleitung erfahren Sie, wie Sie mit [Unity](https://unity3d.com/) und dem Speech SDK für Unity (Beta) eine Spracherkennungsanwendung erstellen.
Danach können Sie in Ihr Gerät sprechen, um Sprache in Echtzeit zu transkribieren.
Sollten Sie noch nicht mit Unity vertraut sein, sehen Sie sich das [Unity-Benutzerhandbuch](https://docs.unity3d.com/Manual/UnityManual.html) an, bevor Sie Ihre Anwendung entwickeln.

> [!NOTE]
> Das Speech SDK für Unity befindet sich derzeit in der Betaphase.
> Es unterstützt Windows Desktop (x86 und x64) oder universelle Windows-Plattform (x86, x64, ARM/ARM64) sowie Android (x86, ARM32/64).

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Projekt benötigen Sie Folgendes:

- [Mindestens Unity 2018.3](https://store.unity.com/), wobei [Unity 2019.1 auch UWP ARM64 unterstützt](https://blogs.unity3d.com/2019/04/16/introducing-unity-2019-1/#universal).
- [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/). Visual Studio 2017 ab Version 15.9 ist ebenfalls akzeptabel.
  - Installieren Sie zur Unterstützung von ARM64 die [optionalen Buildtools für ARM64 und das Windows 10 SDK für ARM64](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development/).
- Ein Abonnementschlüssel für den Speech-Dienst. [Hier erhalten Sie einen kostenlosen Schlüssel.](get-started.md)
- Zugriff auf das Mikrofon Ihres Computers

## <a name="create-a-unity-project"></a>Erstellen eines Unity-Projekts

1. Öffnen Sie Unity. Wenn Sie Unity zum ersten Mal verwenden, wird das Fenster **Unity Hub** *<version number>* angezeigt. (Sie können Unity Hub auch direkt öffnen, um zu diesem Fenster zu gelangen.)

   [![Unity Hub-Fenster](media/sdk/qs-csharp-unity-hub.png)](media/sdk/qs-csharp-unity-hub.png#lightbox)
1. Wählen Sie **Neu**aus. Das Fenster **Create a new project with Unity** *<version number>* (Neues Projekt mit Unity erstellen) wird angezeigt.

   [![Erstellen eines neuen Projekts in Unity](media/sdk/qs-csharp-unity-create-a-new-project.png)](media/sdk/qs-csharp-unity-create-a-new-project.png#lightbox)
1. Geben Sie unter **Project Name** (Projektname) die Zeichenfolge **csharp-unity** ein.
1. Wählen Sie unter **Templates** (Vorlagen) die Option **3D** aus (sofern sie nicht bereits ausgewählt ist).
1. Wählen Sie unter **Location** (Speicherort) einen Ordner zum Speichern des Projekts aus, oder erstellen Sie einen Ordner für diesen Zweck.
1. Klicken Sie auf **Erstellen**.

Kurz darauf wird das Unity-Editorfenster angezeigt.

## <a name="install-the-speech-sdk"></a>Installieren des Speech SDK

Führen Sie die folgenden Schritte aus, um das Speech SDK für Unity zu installieren:

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

1. Laden Sie das als Unity-Ressourcenpaket (UNITYPACKAGE-Datei) vorliegende [Speech SDK für Unity (Beta)](https://aka.ms/csspeech/unitypackage) herunter, und öffnen Sie es. Nach dem Öffnen des Ressourcenpakets wird das Dialogfeld **Import Unity Package** (Unity-Paket importieren) angezeigt.

   [![Dialogfeld „Import Unity Package“ (Unity-Paket importieren) im Unity-Editor](media/sdk/qs-csharp-unity-01-import.png)](media/sdk/qs-csharp-unity-01-import.png#lightbox)
1. Vergewissern Sie sich, dass alle Dateien ausgewählt sind, und wählen Sie **Import** (Importieren) aus. Daraufhin wird das Unity-Ressourcenpaket in Ihr Projekt importiert.

Weitere Informationen zum Importieren von Ressourcenpaketen in Unity finden Sie in der [Unity-Dokumentation](https://docs.unity3d.com/Manual/AssetPackages.html).

## <a name="add-ui"></a>Hinzufügen der Benutzeroberfläche

Als Nächstes fügen wir unserer Szene eine minimalistische Benutzeroberfläche hinzu. Diese Benutzeroberfläche besteht aus einer Schaltfläche zum Starten der Spracherkennung und einem Textfeld zum Anzeigen des Ergebnisses. Im [Hierarchiefenster](https://docs.unity3d.com/Manual/Hierarchy.html) ist eine Beispielszene zu sehen, die Unity mit dem neuen Projekt erstellt hat. ****

1. Wählen Sie im oberen Bereich des Hierarchiefensters **Create** > **UI** > **Button** („Erstellen“ > „Benutzeroberfläche“> „Schaltfläche“) aus. ****

   Daraufhin werden drei Spielobjekte erstellt und im Hierarchiefenster angezeigt: ein Objekt vom Typ **Button** (Schaltfläche), ein Objekt vom Typ **Canvas**, das die Schaltfläche enthält, und ein Objekt vom Typ **EventSystem** (Ereignissystem). ****

   [![Unity-Editorumgebung](media/sdk/qs-csharp-unity-editor-window.png)](media/sdk/qs-csharp-unity-editor-window.png#lightbox)

1. [Navigieren Sie durch die Szenenansicht](https://docs.unity3d.com/Manual/SceneViewNavigation.html), um sich die Canvas und die Schaltfläche in der [Szenenansicht](https://docs.unity3d.com/Manual/UsingTheSceneView.html) genauer anzusehen. **** ****

1. Legen Sie im [Inspektorfenster](https://docs.unity3d.com/Manual/UsingTheInspector.html) (standardmäßig auf der rechten Seite) die Eigenschaften **Pos X** und **Pos Y** auf **0** fest, um die Schaltfläche in der Mitte der Canvas zu positionieren. ****

1. Wählen Sie im Hierarchiefenster **Create** > **UI** > **Text** („Erstellen“ > „Benutzeroberfläche“> „Text“) aus, um ein Textobjekt zu erstellen. **** ****

1. Legen Sie im Inspektorfenster die Eigenschaften **Pos X** und **Pos Y** auf **0** und **120** und die Eigenschaften **Width** (Breite) und **Height** (Höhe) auf **240** und **120** fest. **** Dadurch wird sichergestellt, dass sich Textfeld und Schaltfläche nicht überschneiden.

Die Szenenansicht sollte dann in etwa wie folgt aussehen: ****

[![Szenenansicht im Unity-Editor](media/sdk/qs-csharp-unity-02-ui-inline.png)](media/sdk/qs-csharp-unity-02-ui-inline.png#lightbox)

## <a name="add-the-sample-code"></a>Hinzufügen des Beispielcodes

Gehen Sie wie folgt vor, um den Code des Beispielskripts für das Unity-Projekt hinzuzufügen:

1. Wählen Sie im Projektfenster **Create** > **C# script** („Erstellen“ > „C#-Skript“) aus, um ein neues C#-Skript hinzuzufügen.[](https://docs.unity3d.com/Manual/ProjectView.html)

   [![Projektfenster im Unity-Editor](media/sdk/qs-csharp-unity-project-window.png)](media/sdk/qs-csharp-unity-project-window.png#lightbox)
1. Nennen Sie das Skript `HelloWorld`.

1. Doppelklicken Sie auf `HelloWorld`, um das neu erstellte Skript zu bearbeiten.

   > [!NOTE]
   > Um den Code-Editor zu konfigurieren, der von Unity für die Bearbeitung verwendet werden soll, wählen Sie **Edit** > **Preferences** („Bearbeiten“ > „Einstellungen“) aus, und navigieren Sie zu den Einstellungen für externe Tools. **** Weitere Informationen finden Sie im [Unity-Benutzerhandbuch](https://docs.unity3d.com/Manual/Preferences.html).

1. Ersetzen Sie das vorhandene Skript durch den folgenden Code:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-unity/Assets/Scripts/HelloWorld.cs#code)]

1. Suchen Sie nach der Zeichenfolge `YourSubscriptionKey`, und ersetzen Sie sie durch Ihren Abonnementschlüssel für die Speech-Dienste.

1. Suchen Sie nach der Zeichenfolge `YourServiceRegion`, und ersetzen Sie sie durch die [Region](regions.md), die mit Ihrem Abonnement verknüpft ist. Bei Verwendung der kostenlosen Testversion ist die Region beispielsweise `westus`.

1. Speichern Sie die vorgenommenen Skriptänderungen.

Kehren Sie zum Unity-Editor zurück, und fügen Sie das Skript als Komponente einem Ihrer Spielobjekte hinzu:

1. Wählen Sie im Hierarchiefenster das Objekt **Canvas** aus. ****

1. Wählen Sie im Inspektorfenster die Schaltfläche **Add Component** (Komponente hinzufügen) aus. ****

   [![Inspektorfenster im Unity-Editor](media/sdk/qs-csharp-unity-inspector-window.png)](media/sdk/qs-csharp-unity-inspector-window.png#lightbox)

1. Suchen Sie in der Dropdownliste nach dem weiter oben erstellten Skript `HelloWorld`, und fügen Sie es hinzu. Im Inspektorfenster wird der Abschnitt **Hello World (Script)** (Hallo Welt (Skript)) mit zwei nicht initialisierten Eigenschaften angezeigt: **Output Text** (Ausgabetext) und **Start Reco Button** (Schaltfläche zum Starten der Aufnahme). **** Diese Unity-Komponenteneigenschaften entsprechen den öffentlichen Eigenschaften der Klasse `HelloWorld`.

1. Wählen Sie für die Eigenschaft **Start Reco Button** (Schaltfläche zum Starten der Aufnahme) die Objektauswahl (kleines Kreissymbol rechts neben der Eigenschaft) und anschließend das zuvor erstellte Objekt **Button** (Schaltfläche) aus.

1. Wählen Sie für die Eigenschaft **Output Text** (Ausgabetext) die Objektauswahl und anschließend das zuvor erstellte Objekt **Text** aus.

   > [!NOTE]
   > Die Schaltfläche verfügt auch über ein geschachteltes Textobjekt. Achten Sie darauf, dass Sie nicht versehentlich dieses Objekt für die Textausgabe auswählen (oder benennen Sie im Inspektorfenster eines der Textobjekte mithilfe des Felds **Name** um, um sie nicht zu verwechseln). ****

## <a name="run-the-application-in-the-unity-editor"></a>Ausführen der Anwendung im Unity-Editor

Nun können Sie die Anwendung im Unity-Editor ausführen.

1. Wählen Sie auf der Symbolleiste des Unity-Editors (unterhalb der Menüleiste) die Schaltfläche **Play** (Spielen; nach rechts zeigendes Dreieck) aus.

1. Navigieren Sie zur [Spielansicht](https://docs.unity3d.com/Manual/GameView.html), und warten Sie, bis für das Textobjekt der Text **Click button to recognize speech** (Auf Schaltfläche klicken, um Spracherkennung zu starten) angezeigt wird. **** **** (Falls die Anwendung noch nicht gestartet wurde oder noch nicht bereit ist, wird **New Text** (Neuer Text) angezeigt.)

1. Wählen Sie die Schaltfläche aus, und sprechen Sie einen englischen Ausdruck oder Satz in das Mikrofon Ihres Computers. Ihre Spracheingabe wird an die Speech-Dienste übermittelt, transkribiert und in der Spielansicht angezeigt. ****

   [![Spielansicht im Unity-Editor](media/sdk/qs-csharp-unity-03-output-inline.png)](media/sdk/qs-csharp-unity-03-output-inline.png#lightbox)

1. Überprüfen Sie das [Konsolenfenster](https://docs.unity3d.com/Manual/Console.html) auf Debugmeldungen. **** Sollte das Konsolenfenster nicht angezeigt werden, wählen Sie auf der Menüleiste **Window** > **General** > **Console** („Fenster“ > „Allgemein“ > „Konsole“) aus, um es anzuzeigen. ****

1. Wählen Sie nach Abschluss der Spracherkennung auf der Symbolleiste des Unity-Editors die Schaltfläche **Play** (Spielen) aus, um die Anwendung zu beenden.

## <a name="additional-options-to-run-this-application"></a>Zusätzliche Optionen für die Anwendungsausführung

Diese Anwendung kann auch als Android-App, als eigenständige Windows-App oder als UWP-Anwendung bereitgestellt werden.
Weitere Informationen finden Sie in unserem [Beispielrepository](https://aka.ms/csspeech/samples). Die Konfiguration für diese zusätzlichen Ziele wird im Ordner `quickstart/csharp-unity` beschrieben.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [C#-Beispiele auf GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Weitere Informationen

- [Trainieren eines Modells für Custom Speech](how-to-custom-speech-train-model.md)
