---
title: 'Projekt Akustik: Unity-Integration und Bereitstellung'
titlesuffix: Azure Cognitive Services
description: In diesem Artikel wird die Integration des Unity-Plug-Ins von Projekt Akustik in Ihr Unity-Projekt beschrieben.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: a8ddb0e4ca2ee4396a25a70c8b60b653aebb72d8
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2019
ms.locfileid: "72243002"
---
# <a name="project-acoustics-unity-integration"></a>Projekt Akustik: Unity-Integration
In diesem Artikel wird die Integration des Unity-Plug-Ins von Projekt Akustik in Ihr Unity-Projekt beschrieben.

Softwareanforderungen:
* [Unity 2018.2 oder höher](https://unity3d.com) für Windows
* [Projekt Akustik: Unity-Paket](https://www.microsoft.com/download/details.aspx?id=57346)

## <a name="import-the-plug-in"></a>Importieren des Plug-Ins
1. Importieren Sie das Akustik-Unity-Paket in Ihr Projekt. 
 Wechseln Sie in Unity zu **Assets (Ressourcen)**  > **Import Package (Paket importieren)**  > **Custom Package (Benutzerdefiniertes Paket)** .

    ![Das Unity-Menü zum Importieren von Paketen](media/import-package.png)  

1. Wählen Sie **ProjectAcoustics.unitypackage** aus.

1. Wählen Sie die Schaltfläche **Import** (Importieren) aus, um das Unity-Paket in Ihr Projekt zu integrieren.

    ![Das Unity-Dialogfeld zum Importieren von Paketen](media/import-dialog.png)  

Wenn Sie das Plug-In in ein vorhandenes Projekt importieren, ist in Ihrem Projekt möglicherweise bereits eine Datei *mcs.rsp* im Projektstammverzeichnis vorhanden. Diese Datei gibt Optionen für den C#-Compiler an. Führen Sie diese Datei mit der Datei „mcs.rsp“ zusammen, die im Plug-In Projekt Akustik enthalten ist.

## <a name="enable-the-plug-in"></a>Aktivieren des Plug-Ins
Für den Bakingvorgang des Akustiktoolkits ist die Skriptlaufzeitversion .NET 4.*x* erforderlich. Durch den Paketimport werden die Einstellungen des Unity-Players aktualisiert. Starten Sie Unity neu, damit die Einstellungen übernommen werden.

![Screenshot des Unity-Bereichs für Playereinstellungen](media/player-settings.png)

![Der Unity-Bereich für Playereinstellungen mit der ausgewählten Option „.NET 4.5“](media/net45.png)

## <a name="set-up-audio-dsp"></a>Audio-DSP einrichten
Projekt Akustik enthält die DSP-Audioruntime, die in das Spatializer-Framework der Unity-Audioengine integriert ist. Sie bietet sowohl HRTF-basierte als auch Schwenkverräumlichung. Wählen Sie zum Aktivieren von Projekt Akustik-DSP **Edit (Bearbeiten)**  > **Project Settings (Projekteinstellungen)**  > **Audio** aus, um die Unity-Audioeinstellungen zu öffnen. Wählen Sie **Projekt Akustik** als **Spatializer-Plugin** für Ihr Projekt aus. Stellen Sie sicher, dass **DSP Buffer Size** (DSP-Puffergröße) auf *Best Performance* (Optimale Leistung) festgelegt ist.

![Einstellungsmenü des Unity-Projekts](media/project-settings.png)  

![Der Bereich für Unity Spatializer-Einstellungen mit ausgewähltem Projekt Akustik-Spatializer](media/choose-spatializer.png)

Öffnen Sie anschließend den Audiomixer (**Window** > **Audio Mixer** (Fenster > Audiomixer)). Stellen Sie sicher, dass mindestens ein Mixer mit einer Gruppe festgelegt ist. Wenn dies noch nicht der Fall ist, klicken Sie auf die Schaltfläche **+** rechts neben **Mixers** (Mixer). Klicken Sie mit der rechten Maustaste unten in der Kanalleiste auf den Effektbereich, und fügen Sie den Effekt **Microsoft Acoustics Mixer** hinzu. In Projekt Akustik wird jeweils nur ein Mixer unterstützt.

![Der Unity-Audiomixer, der den Projekt Akustik-Mixer hostet](media/audio-mixer.png)

## <a name="enable-acoustics-on-sound-sources"></a>Aktivieren der Akustik für Soundquellen
Erstellen Sie eine Audioquelle: Aktivieren Sie das Kontrollkästchen **Spatialize** (Verräumlichen) im unteren Inspektorbereich der Audioquelle. Stellen Sie sicher, dass **Spatial Blend** (Räumliche Überlagerung) auf vollständiges *3D* festgelegt ist.  

![Audioquellen-Bereich von Unity](media/audio-source.png)

## <a name="enable-acoustic-design"></a>Aktivieren des Akustikdesigns
Fügen Sie das Skript *AcousticsAdjust* an eine Soundquelle in Ihrer Szene an, um zusätzliche Quellentwurfsparameter zu aktivieren: Wählen Sie **Add Component** (Komponente hinzufügen) aus, und wählen Sie **Scripts** > **AcousticsAdjust** (Skripts > AcousticsAdjust) aus.

![Das Unity-Skript „AcousticsAdjust“](media/acoustics-adjust.png)

## <a name="next-steps"></a>Nächste Schritte
* [Ausführen des Bakings für Ihre Szene mit Projekt Akustik für Unity](unity-baking.md)
* [Erstellen eines Azure Batch-Kontos](create-azure-account.md) für das Baking Ihrer Szene in der Cloud
* Erkunden des [Unity-Entwurfsvorgangs in Projekt Akustik](unity-workflow.md)
