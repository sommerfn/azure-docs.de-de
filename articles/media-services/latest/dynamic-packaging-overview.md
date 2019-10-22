---
title: Übersicht über die dynamische Paketerstellung mit Azure Media Services | Microsoft-Dokumentation
description: Dieser Artikel bietet eine Übersicht über die dynamische Paketerstellung in Azure Media Services.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 10/03/2019
ms.author: juliako
ms.openlocfilehash: af6542757e75d7d6226c2470adf3c2b51d60875a
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72383534"
---
# <a name="dynamic-packaging"></a>Dynamische Paketerstellung

Mit Microsoft Azure Media Services können verschiedenste Medien-Quelldateiformate codiert und über verschiedene Streamingprotokolle mit oder ohne Content Protection bereitgestellt werden, um alle gängigen Geräte (etwa mit iOS und Android) zu erreichen. Diese Clients benötigen verschiedene Protokolle. So müssen Streams für iOS beispielsweise im HLS-Format (HTTP Live Streaming) vorliegen, während Android-Geräte neben HLS auch MPEG-DASH unterstützen. 

In Media Services stellt ein [Streamingendpunkt](streaming-endpoint-concept.md) einen dynamischen (Just-In-Time-)Paketerstellungs- und Ursprungsdienst dar, der Ihre Live- und On-Demand-Inhalte direkt in einer Clientplayeranwendung bereitstellen kann und dabei eines der allgemeinen Streamingmedienprotokolle verwendet, die im folgenden Abschnitt erwähnt werden. Die dynamische Paketerstellung ist ein Feature, das standardmäßig auf allen Streamingendpunkten (Standard oder Premium) vorhanden ist. 

## <a name="a-iddelivery-protocolsto-prepare-your-source-files-for-delivery"></a><a id="delivery-protocols"/>Vorbereiten der Quelldateien für die Lieferung

Um die dynamische Paketerstellung nutzen zu können, müssen Sie Ihre Zwischendatei (Quelldatei) in einen Satz von MP4-Dateien (ISO Base Media 14496-12) mit mehreren Bitraten [codieren](encoding-concept.md). Sie benötigen ein [Medienobjekt](assets-concept.md) mit den codierten MP4-Dateien sowie Streamingkonfigurationsdateien, die für die dynamische Paketerstellung von Media Services erforderlich sind. Aus diesem Satz von MP4-Dateien können Sie mithilfe der dynamischen Paketerstellung über die folgenden Streamingmedienprotokolle Videos bereitstellen:

|Protocol|Beispiel|
|---|---|
|HLS V4 |`https://amsv3account-usw22.streaming.media.azure.net/21b17732-0112-4d76-b526-763dcd843449/ignite.ism/manifest(format=m3u8-aapl)`|
|HLS V3 |`https://amsv3account-usw22.streaming.media.azure.net/21b17732-0112-4d76-b526-763dcd843449/ignite.ism/manifest(format=m3u8-aapl-v3)`|
|HLS CMAF| `https://amsv3account-usw22.streaming.media.azure.net/21b17732-0112-4d76-b526-763dcd843449/ignite.ism/manifest(format=m3u8-cmaf)`|
|MPEG-DASH CSF| `https://amsv3account-usw22.streaming.media.azure.net/21b17732-0112-4d76-b526-763dcd843449/ignite.ism/manifest(format=mpd-time-csf)` |
|MPEG-DASH CMAF|`https://amsv3account-usw22.streaming.media.azure.net/21b17732-0112-4d76-b526-763dcd843449/ignite.ism/manifest(format=mpd-time-cmaf)` |
|Smooth Streaming| `https://amsv3account-usw22.streaming.media.azure.net/21b17732-0112-4d76-b526-763dcd843449/ignite.ism/manifest`|

Wenn Sie Ihre Inhalte mit der dynamischen Verschlüsselung von Media Services schützen möchten, finden Sie unter [Streamingprotokolle und Verschlüsselungstypen](content-protection-overview.md#streaming-protocols-and-encryption-types) weitere Informationen.

> [!TIP]
> Zum Abrufen der MP4-Dateien und Streamingkonfigurationsdateien können Sie beispielsweise [Ihre Zwischendatei (Quelldatei) mit Media Services codieren](#encode-to-adaptive-bitrate-mp4s). 

Um Videos in dem codierten Medienobjekt für die Clientwiedergabe verfügbar zu machen, müssen Sie einen [Streaminglocator](streaming-locators-concept.md) sowie Streaming-URLs erstellen. Daraufhin erhalten Sie den Stream abhängig vom im Streamingclientmanifest angegebenen Format (HLS, MPEG-DASH oder Smooth Streaming) in dem Protokoll, das Sie gewählt haben.

So müssen Sie die Dateien nur in einem Speicherformat speichern und bezahlen. Die entsprechende Antwort wird von Media Services basierend auf Clientanforderungen erstellt und verfügbar gemacht. 

## <a name="on-demand-streaming-workflow"></a>Workflow für das On-Demand-Streaming

In diesem Abschnitt wird ein allgemeiner Media Services-Streamingworkflow mit dynamischer Paketerstellung und dem Standard-Encoder in Azure Media Services beschrieben.

1. Laden Sie eine Eingabedatei (beispielsweise eine QuickTime/MOV- oder MXF-Datei) hoch. Eine Liste unterstützter Formate finden Sie unter [Von Media Encoder Standard unterstützte Formate](media-encoder-standard-formats.md). Diese Datei wird auch als Mezzanine- oder Quelldatei bezeichnet.
1. [Codieren](#encode-to-adaptive-bitrate-mp4s) Sie Ihre Mezzaninedatei als H.264/AAC-MP4-Satz mit adaptiver Bitrate. 
1. Veröffentlichen Sie das Ausgabemedienobjekt, das den MP4-Satz mit adaptiver Bitrate enthält. Zur Veröffentlichung wird ein Streaminglocator erstellt.
1. Erstellen Sie URLs für unterschiedliche Formate (HLS, MPEG-DASH und Smooth Streaming). Der **Streamingendpunkt** sorgt dafür, dass das korrekte Manifest bereitgestellt wird und die Anforderungen für alle diese Formate korrekt verarbeitet werden.

Das folgende Diagramm zeigt den Workflow für das On-Demand-Streaming mit dynamischer Paketerstellung.

![Diagramm eines Workflows für das On-Demand-Streaming mit dynamischer Paketerstellung](./media/dynamic-packaging-overview/media-services-dynamic-packaging.svg)

### <a name="encode-to-adaptive-bitrate-mp4s"></a>Codieren als MP4-Dateien mit adaptiver Bitrate

Beispiele für die [Codierung mit Media Services](encoding-concept.md) finden Sie in den folgenden Artikeln:

* [Codieren aus einer HTTPS-URL mithilfe von integrierten Voreinstellungen](job-input-from-http-how-to.md)
* [Codieren einer lokalen Datei mithilfe von integrierten Voreinstellungen](job-input-from-local-file-how-to.md)
* [Entwickeln einer benutzerdefinierten Voreinstellung für Ihr spezielles Szenario oder Ihre Geräteanforderungen](customize-encoder-presets-how-to.md)

Sehen Sie sich die Liste der [Media Encoder Standard-Formate und -Codecs](media-encoder-standard-formats.md) an.

## <a name="live-streaming-workflow"></a>Workflow für das Livestreaming

Für ein Liveereignis ist einer von zwei Typen möglich: Pass-Through oder Livecodierung. 

Nachfolgend wird ein allgemeiner Workflow für das Livestreaming mit dynamischer Paketerstellung beschrieben:

1. Erstellen Sie ein [Liveereignis](live-events-outputs-concept.md).
1. Rufen Sie die Erfassungs-URL ab, und konfigurieren Sie Ihren lokalen Encoder so, dass die URL zum Senden des Beitragsfeeds verwendet wird.
1. Rufen Sie die Vorschau-URL ab, und verwenden Sie sie, um zu überprüfen, ob die Eingabe des Encoders empfangen wird.
1. Erstellen Sie ein neues Medienobjekt.
1. Erstellen Sie eine Liveausgabe, und verwenden Sie den Namen des erstellten Medienobjekts.<br />Die Liveausgabe archiviert den Datenstrom im Medienobjekt.
1. Erstellen Sie einen Streaminglocator mit den integrierten Typen von Streamingrichtlinien.<br />Wenn Sie beabsichtigen, den Inhalt zu verschlüsseln, lesen Sie [Übersicht über den Inhaltsschutz](content-protection-overview.md).
1. Listen Sie die Pfade für den Streaminglocator so auf, dass die zu verwendenden URLs abgerufen werden.
1. Rufen Sie den Hostnamen für den Streamingendpunkt ab, von dem aus Sie streamen möchten.
1. Erstellen Sie URLs für unterschiedliche Formate (HLS, MPEG-DASH und Smooth Streaming). Der Streamingendpunkt sorgt dafür, dass das korrekte Manifest bereitgestellt wird und die Anforderungen für die verschiedenen Formate korrekt verarbeitet werden.

Im folgenden Diagramm ist der Workflow für das Livestreaming mit dynamischer Paketerstellung dargestellt:

![Diagramm eines Workflows für die Pass-Through-Codierung mit dynamischer Paketerstellung](./media/live-streaming/pass-through.svg)

Informationen zum Livestreaming in Media Services v3 finden Sie unter [Übersicht über das Livestreaming](live-streaming-overview.md).

## <a name="video-codecs-supported-by-dynamic-packaging"></a>Von der dynamischen Paketerstellung unterstützte Videocodecs

Die dynamische Paketerstellung unterstützt MP4-Dateien mit Video, das mit [H.264](https://en.m.wikipedia.org/wiki/H.264/MPEG-4_AVC) (MPEG-4 AVC oder AVC1) oder [H.265](https://en.m.wikipedia.org/wiki/High_Efficiency_Video_Coding) (HEVC, hev1 oder hvc1) codiert wurde.

> [!NOTE]
> Auflösungen von bis zu 4K und Frameraten von bis zu 60 Frames/Sekunde wurden mit der dynamischen Paketerstellung getestet. Der [Premium-Encoder](https://docs.microsoft.com/azure/media-services/previous/media-services-encode-asset#media-encoder-premium-workflow) unterstützt die H.265-Codierung über die v2-Legacy-APIs. Sollten Sie Fragen zu diesem Thema haben, wenden Sie sich an amshelp@microsoft.com. 

## <a name="a-idaudio-codecsaudio-codecs-supported-by-dynamic-packaging"></a><a id="audio-codecs"/>Von der dynamischen Paketerstellung unterstützte Audiocodecs

Die dynamische Paketerstellung unterstützt Audiodaten, die mit den folgenden Protokollen codiert sind:

* [AAC](https://en.wikipedia.org/wiki/Advanced_Audio_Coding) (AAC-LC, HE-AAC v1 oder HE-AAC v2)
* [Dolby Digital Plus](https://en.wikipedia.org/wiki/Dolby_Digital_Plus) (Enhanced AC-3 oder E-AC3)
* Dolby Atmos<br />
   Das Streaming von Dolby Atmos-Inhalten wird für Standards wie das MPEG-DASH-Protokoll mit fragmentiertem CSF- (Common Streaming Format) oder CMAF-MP4 (Common Media Application Format) und über HTTP Live Streaming (HLS) mit CMAF unterstützt.

* [DTS](https://en.wikipedia.org/wiki/DTS_%28sound_system%29)<br />
   Folgende DTS-Codecs werden von den Paketerstellungsformaten DASH-CSF, DASH-CMAF, HLS-M2TS und HLS-CMAF unterstützt:  

    * DTS Digital Surround (dtsc)
    * DTS-HD High Resolution und DTS-HD Master Audio (dtsh)
    * DTS Express (dtse)
    * DTS-HD Lossless ohne Core (dtsl)

Die dynamische Paketerstellung unterstützt mehrere Audiospuren mit DASH oder HLS (ab Version 4) für Streaming-Medienobjekte, die mehrere Audiospuren mit mehreren Codecs und Sprachen besitzen.

### <a name="additional-notes"></a>Zusätzliche Hinweise

Dateien mit [Dolby Digital](https://en.wikipedia.org/wiki/Dolby_Digital)-Audio (AC3) werden von der dynamischen Paketerstellung nicht unterstützt, da es sich dabei um einen Legacy-Codec handelt.

> [!NOTE]
> Der [Premium-Encoder](https://docs.microsoft.com/azure/media-services/previous/media-services-encode-asset#media-encoder-premium-workflow) unterstützt die Dolby Digital Plus-Codierung über die v2-Legacy-APIs. Sollten Sie Fragen zu diesem Thema haben, wenden Sie sich an amshelp@microsoft.com. 

## <a name="manifests"></a>Manifeste 
 
Bei der dynamischen Paketerstellung von Media Services werden die Streamingclientmanifeste für HLS, MPEG-DASH und Smooth Streaming dynamisch basierend auf dem Formatselektor in der URL generiert.  

Eine Manifestdatei enthält Streamingmetadaten wie etwa Typ (Audio, Video oder Text), Titelname, Start- und Endzeit, Bitrate (Qualität), Sprachen, Präsentationsfenster (gleitendes Fenster mit fester Dauer) und Videocodec (FourCC). Sie weist den Player zudem zum Abrufen des nächsten Fragments an, indem Informationen zu den nächsten für die Wiedergabe verfügbaren Videofragmenten und den zugehörigen Speicherorten angegeben werden. Fragmente (oder Segmente) sind die eigentlichen „Blöcke“ von Videoinhalten.

### <a name="examples"></a>Beispiele

#### <a name="hls"></a>HLS

Beispiel für eine HLS-Manifestdatei, auch als HLS Master Playlist bezeichnet: 

```
#EXTM3U
#EXT-X-VERSION:4
#EXT-X-MEDIA:TYPE=AUDIO,GROUP-ID="audio",NAME="aac_eng_2_128041_2_1",LANGUAGE="eng",DEFAULT=YES,AUTOSELECT=YES,URI="QualityLevels(128041)/Manifest(aac_eng_2_128041_2_1,format=m3u8-aapl)"
#EXT-X-STREAM-INF:BANDWIDTH=536608,RESOLUTION=320x180,CODECS="avc1.64000d,mp4a.40.2",AUDIO="audio"
QualityLevels(381048)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=536608,RESOLUTION=320x180,CODECS="avc1.64000d",URI="QualityLevels(381048)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=884544,RESOLUTION=480x270,CODECS="avc1.640015,mp4a.40.2",AUDIO="audio"
QualityLevels(721495)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=884544,RESOLUTION=480x270,CODECS="avc1.640015",URI="QualityLevels(721495)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=1327398,RESOLUTION=640x360,CODECS="avc1.64001e,mp4a.40.2",AUDIO="audio"
QualityLevels(1154816)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=1327398,RESOLUTION=640x360,CODECS="avc1.64001e",URI="QualityLevels(1154816)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=2413312,RESOLUTION=960x540,CODECS="avc1.64001f,mp4a.40.2",AUDIO="audio"
QualityLevels(2217354)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=2413312,RESOLUTION=960x540,CODECS="avc1.64001f",URI="QualityLevels(2217354)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=3805760,RESOLUTION=1280x720,CODECS="avc1.640020,mp4a.40.2",AUDIO="audio"
QualityLevels(3579827)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=3805760,RESOLUTION=1280x720,CODECS="avc1.640020",URI="QualityLevels(3579827)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=139017,CODECS="mp4a.40.2",AUDIO="audio"
QualityLevels(128041)/Manifest(aac_eng_2_128041_2_1,format=m3u8-aapl)
```

#### <a name="mpeg-dash"></a>MPEG-DASH

Beispiel für eine MPEG-DASH-Manifestdatei, auch als MPEG-DASH Media Presentation Description (MPD) bezeichnet:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<MPD xmlns="urn:mpeg:dash:schema:mpd:2011" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" profiles="urn:mpeg:dash:profile:isoff-live:2011" type="static" mediaPresentationDuration="PT1M10.315S" minBufferTime="PT7S">
   <Period>
      <AdaptationSet id="1" group="5" profiles="ccff" bitstreamSwitching="false" segmentAlignment="true" contentType="audio" mimeType="audio/mp4" codecs="mp4a.40.2" lang="en">
         <SegmentTemplate timescale="10000000" media="QualityLevels($Bandwidth$)/Fragments(aac_eng_2_128041_2_1=$Time$,format=mpd-time-csf)" initialization="QualityLevels($Bandwidth$)/Fragments(aac_eng_2_128041_2_1=i,format=mpd-time-csf)">
            <SegmentTimeline>
               <S d="60160000" r="10" />
               <S d="41386666" />
            </SegmentTimeline>
         </SegmentTemplate>
         <Representation id="5_A_aac_eng_2_128041_2_1_1" bandwidth="128041" audioSamplingRate="48000" />
      </AdaptationSet>
      <AdaptationSet id="2" group="1" profiles="ccff" bitstreamSwitching="false" segmentAlignment="true" contentType="video" mimeType="video/mp4" codecs="avc1.640020" maxWidth="1280" maxHeight="720" startWithSAP="1">
         <SegmentTemplate timescale="10000000" media="QualityLevels($Bandwidth$)/Fragments(video=$Time$,format=mpd-time-csf)" initialization="QualityLevels($Bandwidth$)/Fragments(video=i,format=mpd-time-csf)">
            <SegmentTimeline>
               <S d="60060000" r="10" />
               <S d="42375666" />
            </SegmentTimeline>
         </SegmentTemplate>
         <Representation id="1_V_video_1" bandwidth="3579827" width="1280" height="720" />
         <Representation id="1_V_video_2" bandwidth="2217354" codecs="avc1.64001F" width="960" height="540" />
         <Representation id="1_V_video_3" bandwidth="1154816" codecs="avc1.64001E" width="640" height="360" />
         <Representation id="1_V_video_4" bandwidth="721495" codecs="avc1.640015" width="480" height="270" />
         <Representation id="1_V_video_5" bandwidth="381048" codecs="avc1.64000D" width="320" height="180" />
      </AdaptationSet>
   </Period>
</MPD>
```
#### <a name="smooth-streaming"></a>Smooth Streaming

Beispiel für eine Smooth Streaming-Manifestdatei:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SmoothStreamingMedia MajorVersion="2" MinorVersion="2" Duration="703146666" TimeScale="10000000">
   <StreamIndex Chunks="12" Type="audio" Url="QualityLevels({bitrate})/Fragments(aac_eng_2_128041_2_1={start time})" QualityLevels="1" Language="eng" Name="aac_eng_2_128041_2_1">
      <QualityLevel AudioTag="255" Index="0" BitsPerSample="16" Bitrate="128041" FourCC="AACL" CodecPrivateData="1190" Channels="2" PacketSize="4" SamplingRate="48000" />
      <c t="0" d="60160000" r="11" />
      <c d="41386666" />
   </StreamIndex>
   <StreamIndex Chunks="12" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="5">
      <QualityLevel Index="0" Bitrate="3579827" FourCC="H264" MaxWidth="1280" MaxHeight="720" CodecPrivateData="0000000167640020ACD9405005BB011000003E90000EA600F18319600000000168EBECB22C" />
      <QualityLevel Index="1" Bitrate="2217354" FourCC="H264" MaxWidth="960" MaxHeight="540" CodecPrivateData="000000016764001FACD940F0117EF01100000303E90000EA600F1831960000000168EBECB22C" />
      <QualityLevel Index="2" Bitrate="1154816" FourCC="H264" MaxWidth="640" MaxHeight="360" CodecPrivateData="000000016764001EACD940A02FF9701100000303E90000EA600F162D960000000168EBECB22C" />
      <QualityLevel Index="3" Bitrate="721495" FourCC="H264" MaxWidth="480" MaxHeight="270" CodecPrivateData="0000000167640015ACD941E08FEB011000003E90000EA600F162D9600000000168EBECB22C" />
      <QualityLevel Index="4" Bitrate="381048" FourCC="H264" MaxWidth="320" MaxHeight="180" CodecPrivateData="000000016764000DACD941419F9F011000003E90000EA600F14299600000000168EBECB22C" />
      <c t="0" d="60060000" r="11" />
      <c d="42375666" />
   </StreamIndex>
</SmoothStreamingMedia>
```

### <a name="naming-of-tracks-in-the-manifest"></a>Benennen von Spuren im Manifest

Wird in der ISM-Datei ein Audiospurname angegeben, wird von Media Services ein `Label`-Element innerhalb eines `AdaptationSet`-Elements hinzugefügt, um die Strukturinformationen für die bestimmte Audiospur anzugeben. Beispiel für das DASH-Ausgabemanifest:

```xml
<AdaptationSet codecs="mp4a.40.2" contentType="audio" lang="en" mimeType="audio/mp4" subsegmentAlignment="true" subsegmentStartsWithSAP="1">
  <Label>audio_track_name</Label>
  <Role schemeIdUri="urn:mpeg:dash:role:2011" value="main"/>
  <Representation audioSamplingRate="48000" bandwidth="131152" id="German_Forest_Short_Poem_english-en-68s-2-lc-128000bps_seg">
    <BaseURL>German_Forest_Short_Poem_english-en-68s-2-lc-128000bps_seg.mp4</BaseURL>
  </Representation>
</AdaptationSet>
```

Der Player kann das Element `Label` für die Anzeige auf der Benutzeroberfläche verwenden.

### <a name="signaling-audio-description-tracks"></a>Kennzeichnen von Spuren für die Audiobeschreibung

Sie können dem Video eine Audiokommentarspur hinzufügen, damit Benutzer mit Sehbehinderung dem Video folgen können, indem sie sich den Audiokommentar anhören. Sie müssen eine Audiospur als Audiobeschreibung im Manifest kommentieren. Fügen Sie dafür in der ISM-Datei die Parameter „accessibility“ und „role“ hinzu. Sie sind dafür verantwortlich, diese Parameter korrekt festzulegen, um eine Audiospur als Audiobeschreibung zu signalisieren. Fügen Sie beispielsweise `<param name="accessibility" value="description" />` und `<param name="role" value="alternate"` zur ISM-Datei für eine bestimmte Audiospur hinzu. 

Weitere Informationen finden Sie im Beispiel [Signalisieren einer beschreibenden Audiospur](signal-descriptive-audio-howto.md).

#### <a name="smooth-streaming-manifest"></a>Smooth Streaming-Manifest

Wenn Sie einen Smooth Streaming-Stream wiedergeben, enthält das Manifest Werte in den `Accessibility`- und `Role`-Attributen für diese Audiospur. Beispielsweise wird `Role="alternate" Accessibility="description"` zum `StreamIndex`-Element hinzugefügt, um anzugeben, dass es sich um eine Audiobeschreibung handelt.

#### <a name="dash-manifest"></a>DASH-Manifest

Für das DASH-Manifest werden die folgenden beiden Elemente hinzugefügt, um die Audiobeschreibung zu signalisieren:

```xml
<Accessibility schemeIdUri="urn:mpeg:dash:role:2011" value="description"/>
<Role schemeIdUri="urn:mpeg:dash:role:2011" value="alternate"/>
```

#### <a name="hls-playlist"></a>HLS-Wiedergabeliste

Bei HLS Version 7 und höher `(format=m3u8-cmaf)` enthält die Wiedergabeliste `AUTOSELECT=YES,CHARACTERISTICS="public.accessibility.describes-video"`, wenn die Audiobeschreibungsspur signalisiert wird.

#### <a name="example"></a>Beispiel

Weitere Informationen finden Sie unter [Signalisieren einer beschreibenden Audiospur](signal-descriptive-audio-howto.md).

## <a name="dynamic-manifest"></a>Dynamisches Manifest

Zur Steuerung der Anzahl von Spuren sowie der Formate, Bitraten und Präsentationszeitfenster können Sie die dynamische Filterung mit dem dynamischen Packager von Media Services verwenden. Weitere Informationen finden Sie unter [Vorfiltern von Manifesten mit dem dynamischen Packager](filters-dynamic-manifest-overview.md).

## <a name="dynamic-encryption"></a>Dynamische Verschlüsselung

Mit der *dynamischen Verschlüsselung* können Sie Ihre Live- oder On-Demand-Inhalte mit AES-128 oder einem der drei hauptsächlichen DRM-Systeme (Digital Rights Management) dynamisch verschlüsseln: Microsoft PlayReady, Google Widevine und Apple FairPlay. Media Services bietet außerdem einen Dienst für die Übermittlung von AES-Schlüsseln und DRM-Lizenzen an autorisierte Clients. Weitere Informationen finden Sie unter [Dynamische Verschlüsselung](content-protection-overview.md).

## <a name="more-information"></a>Weitere Informationen

Unter [Azure Media Services-Community](media-services-community.md) finden Sie verschiedene Möglichkeiten, Fragen zu stellen, Feedback zu geben und Updates zu Media Services abzurufen.

## <a name="next-steps"></a>Nächste Schritte

> [!NOTE]
> Derzeit können Sie das Azure-Portal nicht für die Verwaltung von v3-Ressourcen verwenden. Verwenden Sie die [REST-API](https://aka.ms/ams-v3-rest-ref), die [Befehlszeilenschnittstelle](https://aka.ms/ams-v3-cli-ref) oder eines der unterstützten [SDKs](media-services-apis-overview.md#sdks).

Informationen zum [Hochladen, Codieren und Streamen von Videos](stream-files-tutorial-with-api.md).

