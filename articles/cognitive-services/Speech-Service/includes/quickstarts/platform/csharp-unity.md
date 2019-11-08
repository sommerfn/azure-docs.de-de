---
title: 'Schnellstart: Speech SDK für C# Unity: Plattformeinrichtung – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: Verwenden Sie diesen Leitfaden, um Ihre Plattform mit dem Speech Services SDK für C# Unity einzurichten.
services: cognitive-services
author: markamos
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 10/10/2019
ms.author: erhopf
ms.openlocfilehash: 6fa1024b68bae6a8353cc71f2749b221ab7e3bc4
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73504403"
---
In diesem Leitfaden erfahren Sie, wie Sie das [Speech SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) für [Unity](https://unity3d.com/) installieren.

> [!NOTE]
> Das Speech SDK für Unity unterstützt Windows Desktop (x86 und x64) oder die universelle Windows-Plattform (x86, x64, ARM/ARM64) sowie Android (x86, ARM32/64) und iOS (x64-Simulator, ARM32 und ARM64).

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung ist Folgendes erforderlich:

- [Mindestens Unity 2018.3](https://store.unity.com/), wobei [Unity 2019.1 auch UWP ARM64 unterstützt](https://blogs.unity3d.com/2019/04/16/introducing-unity-2019-1/#universal).
- [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/). Visual Studio 2017 ab Version 15.9 ist ebenfalls akzeptabel.
- Installieren Sie zur Unterstützung von Windows ARM64 die [optionalen Buildtools für ARM64 und das Windows 10 SDK für ARM64](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development/).

## <a name="install-the-speech-sdk"></a>Installieren des Speech SDK

Führen Sie die folgenden Schritte aus, um das Speech SDK für Unity zu installieren:

1. Laden Sie das als Unity-Ressourcenpaket (UNITYPACKAGE-Datei) vorliegende [Speech SDK für Unity](https://aka.ms/csspeech/unitypackage) herunter, und öffnen Sie es. Das Paket sollte bereits mit Unity verknüpft sein. Nach dem Öffnen des Ressourcenpakets wird das Dialogfeld **Import Unity Package** (Unity-Paket importieren) angezeigt. Unter Umständen muss ein leeres Projekt erstellt und geöffnet werden, damit dieser Schritt funktioniert.

   [![Dialogfeld „Import Unity Package“ (Unity-Paket importieren) im Unity-Editor](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-unity-01-import.png)](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-unity-01-import.png#lightbox)

1. Vergewissern Sie sich, dass alle Dateien ausgewählt sind, und wählen Sie **Import** (Importieren) aus. Daraufhin wird das Unity-Ressourcenpaket in Ihr Projekt importiert.

Weitere Informationen zum Importieren von Ressourcenpaketen in Unity finden Sie in der [Unity-Dokumentation](https://docs.unity3d.com/Manual/AssetPackages.html).

Nun können Sie mit den [nächsten Schritten](#next-steps) fortfahren.

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [windows](../quickstart-list.md)]
