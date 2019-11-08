---
title: 'Schnellstart: Erkennen von Spracheingaben per Mikrofon, C# (Unity) – Spracherkennungsdienst'
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
ms.openlocfilehash: babc6b1c0b6207d404091db3117f95f6d71c53ac
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73505555"
---
> [!NOTE]
> Das Speech SDK für Unity unterstützt Windows Desktop (x86 und x64) oder die universelle Windows-Plattform (x86, x64, ARM/ARM64) sowie Android (x86, ARM32/64) und iOS (x64-Simulator, ARM32 und ARM64).

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die folgenden Schritte aus, bevor Sie beginnen:

> [!div class="checklist"]
> * [Erstellen einer Azure Speech-Ressource](../../../../get-started.md)
> * [Einrichten Ihrer Entwicklungsumgebung](../../../../quickstarts/setup-platform.md?tabs=unity)
> * [Erstellen eines leeren Beispielprojekts](../../../../quickstarts/create-project.md?tabs=unity) Wenn Sie dies bereits getan haben, umso besser. Wir fahren fort.

## <a name="create-a-unity-project"></a>Erstellen eines Unity-Projekts

1. Öffnen Sie Unity. Wenn Sie Unity zum ersten Mal verwenden, wird das Fenster **Unity Hub** *<version number>* angezeigt. (Sie können Unity Hub auch direkt öffnen, um zu diesem Fenster zu gelangen.)

   [![Unity Hub-Fenster](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-hub.png)](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-hub.png#lightbox)
1. Wählen Sie **Neu**aus. Das Fenster **Create a new project with Unity** *<version number>* (Neues Projekt mit Unity erstellen) wird angezeigt.

   [![Erstellen eines neuen Projekts in Unity](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-create-a-new-project.png)](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-create-a-new-project.png#lightbox)
1. Geben Sie unter **Project Name** (Projektname) die Zeichenfolge **csharp-unity** ein.
1. Wählen Sie unter **Templates** (Vorlagen) die Option **3D** aus (sofern sie nicht bereits ausgewählt ist).
1. Wählen Sie unter **Location** (Speicherort) einen Ordner zum Speichern des Projekts aus, oder erstellen Sie einen Ordner für diesen Zweck.
1. Klicken Sie auf **Erstellen**.

Kurz darauf wird das Unity-Editorfenster angezeigt.



## <a name="add-ui"></a>Hinzufügen der Benutzeroberfläche

Als Nächstes fügen wir unserer Szene eine minimalistische Benutzeroberfläche hinzu. Diese Benutzeroberfläche besteht aus einer Schaltfläche zum Starten der Spracherkennung und einem Textfeld zum Anzeigen des Ergebnisses. Im [**Hierarchiefenster**](https://docs.unity3d.com/Manual/Hierarchy.html) ist eine Beispielszene zu sehen, die Unity mit dem neuen Projekt erstellt hat.

1. Wählen Sie im oberen Bereich des **Hierarchiefensters** **Create** > **UI** > **Button** („Erstellen“ > „Benutzeroberfläche“> „Schaltfläche“) aus.

   Daraufhin werden drei Spielobjekte erstellt und im **Hierarchiefenster** angezeigt: ein Objekt vom Typ **Button** (Schaltfläche), ein Objekt vom Typ **Canvas**, das die Schaltfläche enthält, und ein Objekt vom Typ **EventSystem** (Ereignissystem).

   [![Unity-Editorumgebung](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-editor-window.png)](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-editor-window.png#lightbox)

1. [Navigieren Sie durch die **Szenenansicht**](https://docs.unity3d.com/Manual/SceneViewNavigation.html), um sich die Canvas und die Schaltfläche in der [**Szenenansicht**](https://docs.unity3d.com/Manual/UsingTheSceneView.html) genauer anzusehen.

1. Legen Sie im [**Inspektorfenster**](https://docs.unity3d.com/Manual/UsingTheInspector.html) (standardmäßig auf der rechten Seite) die Eigenschaften **Pos X** und **Pos Y** auf **0** fest, um die Schaltfläche in der Mitte der Canvas zu positionieren.

1. Wählen Sie im **Hierarchiefenster** **Create** > **UI** > **Text** („Erstellen“ > „Benutzeroberfläche“> „Text“) aus, um ein **Textobjekt** zu erstellen.

1. Legen Sie im **Inspektorfenster** die Eigenschaften **Pos X** und **Pos Y** auf **0** und **120** und die Eigenschaften **Width** (Breite) und **Height** (Höhe) auf **240** und **120** fest. Dadurch wird sichergestellt, dass sich Textfeld und Schaltfläche nicht überschneiden.

Die **Szenenansicht** sollte dann in etwa wie folgt aussehen:

[![Szenenansicht im Unity-Editor](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-02-ui-inline.png)](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-02-ui-inline.png#lightbox)

## <a name="add-the-sample-code"></a>Hinzufügen des Beispielcodes

Gehen Sie wie folgt vor, um den Code des Beispielskripts für das Unity-Projekt hinzuzufügen:

1. Wählen Sie im Projektfenster **Create** > **C# script** („Erstellen“ > „C#-Skript“) aus, um ein neues [C#-Skript](https://docs.unity3d.com/Manual/ProjectView.html) hinzuzufügen.

   [![Projektfenster im Unity-Editor](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-project-window.png)](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-project-window.png#lightbox)
1. Nennen Sie das Skript `HelloWorld`.

1. Doppelklicken Sie auf `HelloWorld`, um das neu erstellte Skript zu bearbeiten.

   > [!NOTE]
   > Um den Code-Editor zu konfigurieren, der von Unity für die Bearbeitung verwendet werden soll, wählen Sie **Edit** > **Preferences** („Bearbeiten“ > „Einstellungen“) aus, und navigieren Sie zu den Einstellungen für **externe Tools**. Weitere Informationen finden Sie im [Unity-Benutzerhandbuch](https://docs.unity3d.com/Manual/Preferences.html).

1. Ersetzen Sie das vorhandene Skript durch den folgenden Code:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp/unity/from-microphone/Assets/Scripts/HelloWorld.cs#code)]

1. Suchen Sie nach der Zeichenfolge `YourSubscriptionKey`, und ersetzen Sie sie durch Ihren Abonnementschlüssel für die Speech-Dienste.

1. Suchen Sie nach der Zeichenfolge `YourServiceRegion`, und ersetzen Sie sie durch die [Region](~/articles/cognitive-services/Speech-Service/regions.md), die mit Ihrem Abonnement verknüpft ist. Bei Verwendung der kostenlosen Testversion ist die Region beispielsweise `westus`.

1. Speichern Sie die vorgenommenen Skriptänderungen.

Kehren Sie zum Unity-Editor zurück, und fügen Sie das Skript als Komponente einem Ihrer Spielobjekte hinzu:

1. Wählen Sie im **Hierarchiefenster** das Objekt **Canvas** aus.

1. Wählen Sie im **Inspektorfenster** die Schaltfläche **Add Component** (Komponente hinzufügen) aus.

   [![Inspektorfenster im Unity-Editor](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-inspector-window.png)](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-inspector-window.png#lightbox)

1. Suchen Sie in der Dropdownliste nach dem weiter oben erstellten Skript `HelloWorld`, und fügen Sie es hinzu. Im **Inspektorfenster** wird der Abschnitt **Hello World (Script)** (Hallo Welt (Skript)) mit zwei nicht initialisierten Eigenschaften angezeigt: **Output Text** (Ausgabetext) und **Start Reco Button** (Schaltfläche zum Starten der Aufnahme). Diese Unity-Komponenteneigenschaften entsprechen den öffentlichen Eigenschaften der Klasse `HelloWorld`.

1. Wählen Sie für die Eigenschaft **Start Reco Button** (Schaltfläche zum Starten der Aufnahme) die Objektauswahl (kleines Kreissymbol rechts neben der Eigenschaft) und anschließend das zuvor erstellte Objekt **Button** (Schaltfläche) aus.

1. Wählen Sie für die Eigenschaft **Output Text** (Ausgabetext) die Objektauswahl und anschließend das zuvor erstellte Objekt **Text** aus.

   > [!NOTE]
   > Die Schaltfläche verfügt auch über ein geschachteltes Textobjekt. Achten Sie darauf, dass Sie nicht versehentlich dieses Objekt für die Textausgabe auswählen (oder benennen Sie im **Inspektorfenster** eines der Textobjekte mithilfe des Felds **Name** um, um sie nicht zu verwechseln).

## <a name="run-the-application-in-the-unity-editor"></a>Ausführen der Anwendung im Unity-Editor

Nun können Sie die Anwendung im Unity-Editor ausführen.

1. Wählen Sie auf der Symbolleiste des Unity-Editors (unterhalb der Menüleiste) die Schaltfläche **Play** (Spielen; nach rechts zeigendes Dreieck) aus.

1. Navigieren Sie zur [**Spielansicht**](https://docs.unity3d.com/Manual/GameView.html), und warten Sie, bis für das Textobjekt der **Text** **Click button to recognize speech** (Auf Schaltfläche klicken, um Spracherkennung zu starten) angezeigt wird. (Falls die Anwendung noch nicht gestartet wurde oder noch nicht bereit ist, wird **New Text** (Neuer Text) angezeigt.)

1. Wählen Sie die Schaltfläche aus, und sprechen Sie einen englischen Ausdruck oder Satz in das Mikrofon Ihres Computers. Ihre Spracheingabe wird an die Speech-Dienste übermittelt, transkribiert und in der **Spielansicht** angezeigt.

   [![Spielansicht im Unity-Editor](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-03-output-inline.png)](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-unity-03-output-inline.png#lightbox)

1. Überprüfen Sie das [**Konsolenfenster**](https://docs.unity3d.com/Manual/Console.html) auf Debugmeldungen. Sollte das **Konsolenfenster** nicht angezeigt werden, wählen Sie auf der Menüleiste **Window** > **General** > **Console** („Fenster“ > „Allgemein“ > „Konsole“) aus, um es anzuzeigen.

1. Wählen Sie nach Abschluss der Spracherkennung auf der Symbolleiste des Unity-Editors die Schaltfläche **Play** (Spielen) aus, um die Anwendung zu beenden.

## <a name="additional-options-to-run-this-application"></a>Zusätzliche Optionen für die Anwendungsausführung

Diese Anwendung kann auch als Android-App, als eigenständige Windows-App oder als UWP-Anwendung bereitgestellt werden.
Weitere Informationen finden Sie in unserem [Beispielrepository](https://aka.ms/csspeech/samples). Die Konfiguration für diese zusätzlichen Ziele wird im Ordner `quickstart/csharp-unity` beschrieben.

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [footer](./footer.md)]
