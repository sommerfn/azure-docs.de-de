---
title: 'Schnellstart: Sprachsynthese, Unity – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: In dieser Anleitung erfahren Sie, wie Sie mit Unity und dem Speech SDK für Unity eine Sprachsyntheseanwendung erstellen. Mit dieser Anleitung können Sie Sprache aus Text zur Wiedergabe über den Lautsprecher Ihres Geräts in Echtzeit synthetisieren.
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 9/19/2019
ms.author: yinhew
ms.openlocfilehash: be5f07b8ea58d0d62c70e0e9dc8ab187ce4a0f63
ms.sourcegitcommit: 4f3f502447ca8ea9b932b8b7402ce557f21ebe5a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2019
ms.locfileid: "71803200"
---
# <a name="quickstart-synthesize-speech-with-the-speech-sdk-for-unity"></a>Schnellstart: Sprachsynthese mit dem Speech SDK für Unity

Schnellstarts sind auch für [Spracherkennung](quickstart-csharp-unity.md) verfügbar.

In dieser Anleitung erfahren Sie, wie Sie mit [Unity](https://unity3d.com/) und dem Speech SDK für Unity eine Sprachsyntheseanwendung erstellen.
Mit dieser Anleitung können Sie Sprache aus Text zur Wiedergabe über den Lautsprecher Ihres Geräts in Echtzeit synthetisieren.
Sollten Sie noch keine Erfahrung mit Unity haben, machen Sie sich zunächst mit dem [Unity-Benutzerhandbuch](https://docs.unity3d.com/Manual/UnityManual.html) vertraut, bevor Sie mit der Anwendungsentwicklung beginnen.

> [!NOTE]
> Unity unterstützt Windows Desktop (x86 und x64) oder die universelle Windows-Plattform (x86, x64, ARM/ARM64) sowie Android (x86, ARM32/64) und iOS (x64-Simulator, ARM32 und ARM64).

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Projekt benötigen Sie Folgendes:

* [Unity 2018.3 oder höher](https://store.unity.com/), wobei [Unity 2019.1 auch UWP ARM64 unterstützt](https://blogs.unity3d.com/2019/04/16/introducing-unity-2019-1/#universal)
* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/). Visual Studio 2017 ab Version 15.9 ist ebenfalls akzeptabel.
* Installieren Sie zur Unterstützung von Windows ARM64 die [optionalen Buildtools für ARM64 und das Windows 10 SDK für ARM64](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development/). 
* Ein Abonnementschlüssel für den Speech-Dienst. [Hier erhalten Sie einen kostenlosen Schlüssel.](get-started.md)

## <a name="create-a-unity-project"></a>Erstellen eines Unity-Projekts

* Starten Sie Unity, und wählen Sie unter der Registerkarte **Projects** (Projekte) die Option **New** (Neu) aus.
* Geben Sie unter **Project name** (Projektname) den Text **csharp-unity** und unter **Template** (Vorlage) den Text **3D** ein, und wählen Sie einen Speicherort aus.
  Wählen Sie **Create project** (Projekt erstellen) aus.
* Kurz darauf wird das Unity-Editorfenster geöffnet.

## <a name="install-the-speech-sdk"></a>Installieren des Speech SDK

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

* Das Speech SDK für Unity (Beta) liegt als Unity-Ressourcenpaket (.unitypackage) vor.
  Laden Sie es [hier](https://aka.ms/csspeech/unitypackage) herunter.
* Wählen Sie **Assets** > **Import Package** > **Custom Package** (Ressourcen > Paket importieren > Benutzerdefiniertes Paket) aus, um das Speech SDK zu importieren.
  Ausführliche Informationen finden Sie in der [Unity-Dokumentation](https://docs.unity3d.com/Manual/AssetPackages.html).
* Wählen Sie in der Dateiauswahl die UNITYPACKAGE-Datei für das Speech SDK aus, die Sie weiter oben heruntergeladen haben.
* Vergewissern Sie sich, dass alle Dateien ausgewählt sind, und klicken Sie auf **Import** (Importieren):

  ![Screenshot des Unity-Editors beim Importieren des Unity-Ressourcenpakets für das Speech SDK](media/sdk/qs-csharp-unity-01-import.png)

## <a name="add-ui"></a>Hinzufügen der Benutzeroberfläche

Wir fügen unserer Szene eine minimalistische Benutzeroberfläche mit einem Eingabefeld zur Eingabe des Textes für die Synthese, einer Schaltfläche zum Starten der Sprachsynthese und einem Textfeld zum Anzeigen des Ergebnisses hinzu.

* Im [Hierarchiefenster](https://docs.unity3d.com/Manual/Hierarchy.html) (standardmäßig auf der linken Seite) ist eine Beispielszene zu sehen, die Unity mit dem neuen Projekt erstellt hat.
* Klicken Sie am oberen Rand des Hierarchiefensters auf die Schaltfläche **Create** (Erstellen), und wählen Sie **UI** > **Input Field** (Benutzeroberfläche > Eingabefeld) aus.
* Daraufhin werden drei Spielobjekte erstellt und im Hierarchiefenster angezeigt: ein Objekt vom Typ **Input Field** (Eingabefeld) in einem Objekt vom Typ **Canvas** sowie ein Objekt vom Typ **EventSystem** (Ereignissystem).
* [Navigieren Sie durch die Szenenansicht](https://docs.unity3d.com/Manual/SceneViewNavigation.html), um sich die Canvas und das Eingabefeld in der [Szenenansicht](https://docs.unity3d.com/Manual/UsingTheSceneView.html) genauer anzusehen.
* Klicken Sie im Hierarchiefenster auf das Objekt **Input Field** (Eingabefeld), um dessen Einstellungen im [Inspektorfenster](https://docs.unity3d.com/Manual/UsingTheInspector.html) (standardmäßig auf der rechten Seite) anzuzeigen.
* Legen Sie die Eigenschaften **Pos X** und **Pos Y** auf **0** fest, um das Eingabefeld in der Mitte der Canvas zu positionieren.
* Klicken Sie am oberen Rand des Hierarchiefensters erneut auf die Schaltfläche **Create** (Erstellen), und wählen Sie **UI** > **Button** (Benutzeroberfläche > Schaltfläche) aus, um ein Textfeld zu erstellen.
* Klicken Sie im Hierarchiefenster auf das Objekt **Button** (Schaltfläche), um dessen Einstellungen im [Inspektorfenster](https://docs.unity3d.com/Manual/UsingTheInspector.html) (standardmäßig auf der rechten Seite) anzuzeigen.
* Legen Sie die Eigenschaften **Pos X** und **Pos Y** auf **0** und **-48** und die Eigenschaften **Width** (Breite) und **Height** (Höhe) auf **160** und **30** fest, um sicherzustellen, dass die Schaltfläche und das Eingabefeld sich nicht überlappen.
* Klicken Sie am oberen Rand des Hierarchiefensters erneut auf die Schaltfläche **Create** (Erstellen), und wählen Sie **UI** > **Text** (Benutzeroberfläche > Text) aus, um ein Textfeld zu erstellen.
* Klicken Sie im Hierarchiefenster auf das Objekt **Text**, um dessen Einstellungen im [Inspektorfenster](https://docs.unity3d.com/Manual/UsingTheInspector.html) (standardmäßig auf der rechten Seite) anzuzeigen.
* Legen Sie die Eigenschaften **Pos X** und **Pos Y** auf **0** und **80** und die Eigenschaften **Width** (Breite) und **Height** (Höhe) auf **320** und **80** fest, um sicherzustellen, dass das Textfeld und das Eingabefeld sich nicht überlappen.
* Klicken Sie am oberen Rand des Hierarchiefensters erneut auf die Schaltfläche **Create** (Erstellen), und wählen Sie **Audio** > **Audio Source** (Audio > Audioquelle) aus, um eine Audioquelle zu erstellen.

Die Benutzeroberfläche sollte dann in etwa wie folgt aussehen:

[![Screenshot der Schnellstart-Benutzeroberfläche im Unity-Editor](media/sdk/qs-tts-csharp-unity-ui-inline.png)](media/sdk/qs-tts-csharp-unity-ui-expanded.png#lightbox)

## <a name="add-the-sample-code"></a>Hinzufügen des Beispielcodes

1. Klicken Sie im [Projektfenster](https://docs.unity3d.com/Manual/ProjectView.html) (standardmäßig links unten) auf die Schaltfläche **Create** (Erstellen), und wählen Sie **C# script** (C#-Skript) aus. Nennen Sie das Skript `HelloWorld`.

1. Doppelklicken Sie auf das Skript, um es zu bearbeiten.

   > [!NOTE]
   > Unter **Edit** > **Preferences** (Bearbeiten > Voreinstellungen) können Sie konfigurieren, welcher Code-Editor gestartet werden soll. Weitere Informationen finden Sie im [Unity-Benutzerhandbuch](https://docs.unity3d.com/Manual/Preferences.html).

1. Ersetzen Sie den gesamten Code durch Folgendes:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/csharp-unity/Assets/Scripts/HelloWorld.cs#code)]

1. Suchen Sie nach der Zeichenfolge `YourSubscriptionKey`, und ersetzen Sie sie durch Ihren Abonnementschlüssel für die Speech-Dienste.

1. Suchen Sie nach der Zeichenfolge `YourServiceRegion`, und ersetzen Sie sie durch die [Region](regions.md), die mit Ihrem Abonnement verknüpft ist. Bei Verwendung der kostenlosen Testversion ist die Region beispielsweise `westus`.

1. Speichern Sie die vorgenommenen Skriptänderungen.

1. Als Nächstes muss das Skript im Unity-Editor einem Ihrer Spielobjekte als Komponente hinzugefügt werden.

   * Klicken Sie im Hierarchiefenster auf das Objekt **Canvas**. Daraufhin werden die Einstellungen im [Inspektorfenster](https://docs.unity3d.com/Manual/UsingTheInspector.html) (standardmäßig auf der rechten Seite) geöffnet.
   * Klicken Sie im Inspektorfenster auf die Schaltfläche **Add Component** (Komponente hinzufügen). Suchen Sie nach dem zuvor erstellten Skript „HelloWorld“, und fügen Sie es hinzu.
   * Die Hello World-Komponente besitzt vier nicht initialisierte Eigenschaften: **Output Text** (Ausgabetext), **Input Field** (Eingabefeld), **Speak Button** (Schaltfläche zum Starten des Sprechens) und **Audio Source** (Audioquelle). Diese entsprechen öffentlichen Eigenschaften der `HelloWorld`-Klasse.
     Klicken Sie zum Verknüpfen dieser Eigenschaften auf die Objektauswahl (kleines Kreissymbol rechts neben der Eigenschaft), und wählen Sie die zuvor erstellten Text- und Schaltflächenobjekte aus.

     > [!NOTE]
     > Das Eingabefeld und die Schaltfläche haben auch ein geschachteltes Textobjekt. Achten Sie darauf, dass Sie nicht versehentlich dieses Objekt für die Textausgabe auswählen (oder benennen Sie im Inspektorfenster die Textobjekte über das Feld „Name“ um, um sie nicht zu verwechseln).

## <a name="run-the-application-in-the-unity-editor"></a>Ausführen der Anwendung im Unity-Editor

* Klicken Sie auf der Symbolleiste des Unity-Editors (unterhalb der Menüleiste) auf die Schaltfläche **Play** (Spielen).

* Wenn die App gestartet wird, geben Sie Text in das Eingabefeld ein, und klicken Sie auf die Schaltfläche. Ihr Text wird an die Spracherkennungsdienste übermittelt und in Sprache synthetisiert, die dann über Ihren Lautsprecher ausgegeben wird.

  [![Screenshot der ausgeführten Schnellstart-App im Unity-Spielefenster](media/sdk/qs-tts-csharp-unity-output-inline.png)](media/sdk/qs-tts-csharp-unity-output-expanded.png#lightbox)

* Überprüfen Sie das [Konsolenfenster](https://docs.unity3d.com/Manual/Console.html) auf Debugmeldungen.

* Klicken Sie nach Abschluss der Sprachsynthese auf der Symbolleiste des Unity-Editors auf die Schaltfläche **Play** (Wiedergeben), um die App zu beenden.

## <a name="additional-options-to-run-this-application"></a>Zusätzliche Optionen für die Anwendungsausführung

Diese Anwendung kann auch für Android, als eigenständige Windows-App oder als UWP-Anwendung bereitgestellt werden.
Die Konfiguration für diese zusätzlichen Ziele wird in unserem [Beispielrepository](https://aka.ms/csspeech/samples) im Ordner „quickstart/csharp-unity“ beschrieben.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [C#-Beispiele auf GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Weitere Informationen

- [Anpassen von Voicefonts](how-to-customize-voice-font.md)
- [Aufnehmen von Sprachbeispielen](record-custom-voice-samples.md)
