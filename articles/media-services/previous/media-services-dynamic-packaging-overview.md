---
title: Übersicht über die dynamische Paketerstellung mit Azure Media Services | Microsoft-Dokumentation
description: In diesem Thema finden Sie eine Übersicht über die dynamische Paketerstellung.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2019
ms.author: juliako
ms.openlocfilehash: 4b4f2ec779c37f78b371c27df80c354eccb41e7a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "64869433"
---
# <a name="dynamic-packaging"></a>Dynamische Paketerstellung

> [!div class="op_single_selector" title1="Wählen Sie die von Ihnen verwendete Media Services-Version aus:"]
> * [Version 3](../latest/dynamic-packaging-overview.md)
> * [Version 2](media-services-dynamic-packaging-overview.md)

> [!NOTE]
> Media Services v2 werden derzeit keine neuen Features oder Funktionen hinzugefügt. <br/>Sehen Sie sich die neuste Version – [Media Services v3](https://docs.microsoft.com/azure/media-services/latest/) – an. Lesen Sie außerdem die [Hinweise zur Migration von v2 zu v3](../latest/migrate-from-v2-to-v3.md).

Microsoft Azure Media Services kann verwendet werden, um zahlreiche Medien-Quelldateiformate, Medienstreamingformate und Inhaltsschutzformate in verschiedenen Clienttechnologien wie iOS, XBOX, Silverlight oder Windows 8 bereitzustellen. Von den Clients werden verschiedene Protokolle verarbeitet. So ist für iOS ein HLS-V4-Format (HTTP Live Streaming) erforderlich, für Silverlight und Xbox dagegen Smooth Streaming. Wenn Sie über einen Satz MP4-Dateien (ISO Base Media 14496-12) mit adaptiver Bitrate (Multi-Bitrate) oder einen Satz von Smooth Streaming-Dateien mit adaptiver Bitrate verfügen, die Sie Clients bereitstellen möchten, von denen MPEG DASH, HLS oder Smooth Streaming verarbeitet wird, sollten Sie die Vorteile der dynamischen Paketerstellung von Media Services nutzen.

Bei der dynamischen Paketerstellung müssen Sie lediglich ein Medienobjekt erstellen, das einen Satz von MP4- oder Smooth Streaming-Dateien mit adaptiver Bitrate enthält. Dann wird durch den bedarfsgesteuerten Streamingserver auf Basis des in der Manifest- oder Fragmentanforderung angegebenen Formats sichergestellt, dass Sie den Datenstrom im ausgewählten Protokoll erhalten. So müssen Sie die Dateien nur in einem Speicherformat speichern und bezahlen. Die entsprechende Antwort wird von Media Services basierend auf Clientanforderungen erstellt und verfügbar gemacht.

Im folgenden Diagramm wird der herkömmliche Workflow zur Codierung und statischen Paketerstellung dargestellt.

![Statische Codierung](./media/media-services-dynamic-packaging-overview/media-services-static-packaging.png)

Im folgenden Diagramm wird der Workflow zur dynamischen Paketerstellung dargestellt.

![Dynamische Codierung](./media/media-services-dynamic-packaging-overview/media-services-dynamic-packaging.png)

## <a name="common-scenario"></a>Allgemeines Szenario

1. Laden Sie eine Eingabedatei (auch Mezzaninedatei genannt) hoch. Beispielformate: H.264, MP4 oder WMV (eine Liste unterstützter Formate finden Sie unter [Von Media Encoder Standard unterstützte Formate](media-services-media-encoder-standard-formats.md)).
2. Codieren Sie Ihre Mezzaninedatei zu H.264-MP4-Sätzen mit adaptiver Bitrate.
3. Veröffentlichen Sie das Medienobjekt, das den MP4-Satz mit adaptiver Bitrate enthält, indem Sie den On-Demand-Locator erstellen.
4. Erstellen Sie die Streaming-URLs zum Zugreifen und Streamen Ihrer Inhalte.

## <a name="preparing-assets-for-dynamic-streaming"></a>Vorbereiten von Medienobjekten auf dynamisches Streaming

Das Medienobjekt kann wie folgt für dynamisches Streaming vorbereitet werden:

- [Laden Sie eine Masterdatei hoch](media-services-dotnet-upload-files.md).
- [Verwenden Sie den Media Encoder Standard-Encoder zum Erzeugen von H.264-MP4-Sätzen mit adaptiver Bitrate](media-services-dotnet-encode-with-media-encoder-standard.md).
- [Streamen Sie Ihre Inhalte](media-services-deliver-content-overview.md).

## <a name="audio-codecs-supported-by-dynamic-packaging"></a>Von der dynamischen Paketerstellung unterstützte Audiocodecs

Die dynamische Paketerstellung unterstützt MP4-Dateien mit Audio, das mit [AAC](https://en.wikipedia.org/wiki/Advanced_Audio_Coding) (AAC-LC, HE-AAC v1, HE-AAC v2), [Dolby Digital Plus](https://en.wikipedia.org/wiki/Dolby_Digital_Plus) (Enhanced AC-3 oder E-AC3), Dolby Atmos oder [DTS](https://en.wikipedia.org/wiki/DTS_%28sound_system%29) (DTS Express, DTS LBR, DTS HD, DTS HD Lossless) codiert wurde. Das Streaming von Dolby Atmos-Inhalten wird für Standards wie das MPEG-DASH-Protokoll mit fragmentiertem CSF- (Common Streaming Format) oder CMAF-MP4 (Common Media Application Format) und über HTTP Live Streaming (HLS) mit CMAF unterstützt.

> [!NOTE]
> Dateien mit [Dolby Digital](https://en.wikipedia.org/wiki/Dolby_Digital)-Audio (AC3) werden von der dynamischen Paketerstellung nicht unterstützt, da es sich dabei um einen Legacy-Codec handelt.

## <a name="media-services-learning-paths"></a>Media Services-Lernpfade

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben

[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

