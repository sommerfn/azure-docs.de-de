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
ms.date: 07/23/2019
ms.author: jhakulin
ms.openlocfilehash: 06831fa933c04827c966e8f6e12aa817f5008b88
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68554138"
---
# <a name="quickstart-recognize-speech-with-the-speech-sdk-for-unity-beta"></a>Schnellstart: Erkennen von Sprache mit dem Speech SDK für Unity (Beta)

Schnellstarts sind auch für [Sprachsynthese](quickstart-text-to-speech-csharp-unity.md) verfügbar.

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

In dieser Anleitung erfahren Sie, wie Sie mit [Unity](https://unity3d.com/) und dem Speech SDK für Unity (Beta) eine Spracherkennungsanwendung erstellen.
Danach können Sie das Mikrofon Ihres Computers verwenden, um Sprache in Echtzeit zu transkribieren.
Sollten Sie noch keine Erfahrung mit Unity haben, machen Sie sich zunächst mit dem [Unity-Benutzerhandbuch](https://docs.unity3d.com/Manual/UnityManual.html) vertraut, bevor Sie mit der Anwendungsentwicklung beginnen.

> [!NOTE]
> Das Speech SDK für Unity befindet sich derzeit in der Betaphase.
> Es unterstützt Windows Desktop (x86 und x64) oder universelle Windows-Plattform (x86, x64, ARM/ARM64) sowie Android (x86, ARM32/64).

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Projekt benötigen Sie Folgendes:

* [Unity 2018.3 oder höher](https://store.unity.com/), wobei [Unity 2019.1 auch UWP ARM64 unterstützt](https://blogs.unity3d.com/2019/04/16/introducing-unity-2019-1/#universal)
* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
     * Installieren Sie zur Unterstützung von ARM64 die [optionalen Buildtools für ARM64 und das Windows 10 SDK für ARM64](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development/).
* Ein Abonnementschlüssel für den Speech-Dienst. [Hier erhalten Sie einen kostenlosen Schlüssel.](get-started.md)
* Zugriff auf das Mikrofon Ihres Computers

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

Wir fügen unserer Szene eine minimalistische Benutzeroberfläche mit einer Schaltfläche zum Starten der Spracherkennung und einem Textfeld zum Anzeigen des Ergebnisses hinzu.

* Im [Hierarchiefenster](https://docs.unity3d.com/Manual/Hierarchy.html) (standardmäßig auf der linken Seite) ist eine Beispielszene zu sehen, die Unity mit dem neuen Projekt erstellt hat.
* Klicken Sie am oberen Rand des Hierarchiefensters auf die Schaltfläche **Create** (Erstellen), und wählen Sie **UI** > **Button** (Benutzeroberfläche > Schaltfläche) aus.
* Daraufhin werden drei Spielobjekte erstellt und im Hierarchiefenster angezeigt: ein Objekt vom Typ **Button** (Schaltfläche) in einem Objekt vom Typ **Canvas** sowie ein Objekt vom Typ **EventSystem** (Ereignissystem).
* [Navigieren Sie durch die Szenenansicht](https://docs.unity3d.com/Manual/SceneViewNavigation.html), um sich die Canvas und die Schaltfläche in der [Szenenansicht](https://docs.unity3d.com/Manual/UsingTheSceneView.html) genauer anzusehen.
* Klicken Sie im Hierarchiefenster auf das Objekt **Button** (Schaltfläche), um dessen Einstellungen im [Inspektorfenster](https://docs.unity3d.com/Manual/UsingTheInspector.html) (standardmäßig auf der rechten Seite) anzuzeigen.
* Legen Sie die Eigenschaften **Pos X** und **Pos Y** auf **0** fest, um die Schaltfläche in der Mitte der Canvas zu positionieren.
* Klicken Sie am oberen Rand des Hierarchiefensters erneut auf die Schaltfläche **Create** (Erstellen), und wählen Sie **UI** > **Text** (Benutzeroberfläche > Text) aus, um ein Textfeld zu erstellen.
* Klicken Sie im Hierarchiefenster auf das Objekt **Text**, um dessen Einstellungen im [Inspektorfenster](https://docs.unity3d.com/Manual/UsingTheInspector.html) (standardmäßig auf der rechten Seite) anzuzeigen.
* Legen Sie die Eigenschaften **Pos X** und **Pos Y** auf **0** und **120** und die Eigenschaften **Width** (Breite) und **Height** (Höhe) auf **240** und **120** fest, um sicherzustellen, dass das Textfeld und die Schaltfläche nicht überlappen.

Die Benutzeroberfläche sollte dann in etwa wie folgt aussehen:

[![Screenshot der Schnellstart-Benutzeroberfläche im Unity-Editor](media/sdk/qs-csharp-unity-02-ui-inline.png)](media/sdk/qs-csharp-unity-02-ui-expanded.png#lightbox)

## <a name="add-the-sample-code"></a>Hinzufügen des Beispielcodes

1. Klicken Sie im [Projektfenster](https://docs.unity3d.com/Manual/ProjectView.html) (standardmäßig links unten) auf die Schaltfläche **Create** (Erstellen), und wählen Sie **C# script** (C#-Skript) aus. Nennen Sie das Skript `HelloWorld`.

1. Doppelklicken Sie auf das Skript, um es zu bearbeiten.

   > [!NOTE]
   > Unter **Edit** > **Preferences** (Bearbeiten > Voreinstellungen) können Sie konfigurieren, welcher Code-Editor gestartet werden soll. Weitere Informationen finden Sie im [Unity-Benutzerhandbuch](https://docs.unity3d.com/Manual/Preferences.html).

1. Ersetzen Sie den gesamten Code durch Folgendes:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-unity/Assets/Scripts/HelloWorld.cs#code)]

1. Suchen Sie nach der Zeichenfolge `YourSubscriptionKey`, und ersetzen Sie sie durch Ihren Abonnementschlüssel für die Speech-Dienste.

1. Suchen Sie nach der Zeichenfolge `YourServiceRegion`, und ersetzen Sie sie durch die [Region](regions.md), die mit Ihrem Abonnement verknüpft ist. Bei Verwendung der kostenlosen Testversion ist die Region beispielsweise `westus`.

1. Speichern Sie die vorgenommenen Skriptänderungen.

1. Als Nächstes muss das Skript im Unity-Editor einem Ihrer Spielobjekte als Komponente hinzugefügt werden.

   * Klicken Sie im Hierarchiefenster auf das Objekt **Canvas**. Daraufhin werden die Einstellungen im [Inspektorfenster](https://docs.unity3d.com/Manual/UsingTheInspector.html) (standardmäßig auf der rechten Seite) geöffnet.
   * Klicken Sie im Inspektorfenster auf die Schaltfläche **Add Component** (Komponente hinzufügen). Suchen Sie nach dem zuvor erstellten Skript „HelloWorld“, und fügen Sie es hinzu.
   * Die Hello World-Komponente besitzt zwei nicht initialisierte Eigenschaften: **Output Text** (Ausgabetext) und **Start Reco Button** (Schaltfläche zum Starten der Aufnahme). Diese entsprechen öffentlichen Eigenschaften der Klasse `HelloWorld`.
     Klicken Sie zum Verknüpfen dieser Eigenschaften auf die Objektauswahl (kleines Kreissymbol rechts neben der Eigenschaft), und wählen Sie die zuvor erstellten Text- und Schaltflächenobjekte aus.

     > [!NOTE]
     > Die Schaltfläche verfügt auch über ein geschachteltes Textobjekt. Achten Sie darauf, dass Sie nicht versehentlich dieses Objekt für die Textausgabe auswählen (oder benennen Sie im Inspektorfenster eines der Textobjekte über das Feld „Name“ um, um sie nicht zu verwechseln).

## <a name="run-the-application-in-the-unity-editor"></a>Ausführen der Anwendung im Unity-Editor

* Klicken Sie auf der Symbolleiste des Unity-Editors (unterhalb der Menüleiste) auf die Schaltfläche **Play** (Spielen).

* Klicken Sie nach dem Start der App auf die Schaltfläche, und sprechen Sie einen englischen Ausdruck oder Satz in das Mikrofon Ihres Computers. Ihre Spracheingabe wird an die Speech-Dienste übermittelt, in Text transkribiert und im Fenster angezeigt.

  [![Screenshot der ausgeführten Schnellstart-App im Unity-Spielefenster](media/sdk/qs-csharp-unity-03-output-inline.png)](media/sdk/qs-csharp-unity-03-output-expanded.png#lightbox)

* Überprüfen Sie das [Konsolenfenster](https://docs.unity3d.com/Manual/Console.html) auf Debugmeldungen.

* Klicken Sie nach Abschluss der Spracherkennung auf der Symbolleiste des Unity-Editors auf die Schaltfläche **Play** (Spielen), um die App zu beenden.

## <a name="additional-options-to-run-this-application"></a>Zusätzliche Optionen für die Anwendungsausführung

Diese Anwendung kann auch für Android, als eigenständige Windows-App oder als UWP-Anwendung bereitgestellt werden.
Die Konfiguration für diese zusätzlichen Ziele wird in unserem [Beispielrepository](https://aka.ms/csspeech/samples) im Ordner „quickstart/csharp-unity“ beschrieben.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [C#-Beispiele auf GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Weitere Informationen

- [Tutorial: Erstellen eines benutzerdefinierten Akustikmodells](how-to-customize-acoustic-models.md)
- [Tutorial: Erstellen eines benutzerdefinierten Sprachmodells](how-to-customize-language-model.md)
