---
title: LiveEvent-Typen in Azure Media Services | Microsoft-Dokumentation
description: Dieser Artikel enthält eine detaillierte Tabelle, in der Liveereignistypen (LiveEvent-Typen) verglichen werden.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 06/13/2019
ms.author: juliako
ms.openlocfilehash: 884cf8d913cec038df3b38c8af2ed0a67bd8060d
ms.sourcegitcommit: b7b0d9f25418b78e1ae562c525e7d7412fcc7ba0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2019
ms.locfileid: "70802258"
---
# <a name="live-event-types-comparison"></a>Vergleich von Liveereignistypen

In Azure Media Services gibt es zwei Arten von [Liveereignissen](https://docs.microsoft.com/rest/api/media/liveevents): Livecodierung und Pass-Through. 

## <a name="types-comparison"></a>Typenvergleich 

In der folgenden Tabelle werden die Features der Liveereignistypen verglichen. Die Typen werden während der Erstellung mit [LiveEventEncodingType](https://docs.microsoft.com/rest/api/media/liveevents/create#liveeventencodingtype) festgelegt:

* **LiveEventEncodingType.None**: Ein lokaler Liveencoder sendet einen Datenstrom mit mehreren Bitraten. Der erfasste Datenstrom durchläuft das Liveereignis ohne weitere Verarbeitung. 
* **LiveEventEncodingType.Standard**: Ein lokaler Liveencoder sendet einen Datenstrom mit einer einzigen Bitrate an das Liveereignis, und Media Services erstellt einen Datenstrom mit mehreren Bitraten. Wenn der Beitragsfeed eine Auflösung von 720p oder höher hat, bewirkt die Voreinstellung **Default720p**, dass eine Reihe von 6 Auflösung/Bitrate-Paaren codiert wird (weitere Informationen folgen später in diesem Artikel).
* **LiveEventEncodingType.Premium1080p**: Ein lokaler Liveencoder sendet einen Datenstrom mit einer einzigen Bitrate an das Liveereignis, und Media Services erstellt einen Datenstrom mit mehreren Bitraten. Die Voreinstellung „Default1080p“ gibt den Ausgabesatz von Auflösung/Bitrate-Paaren an (weitere Informationen folgen später in diesem Artikel). 

| Feature | Liveereignis vom Typ „Pass-Through“ | Liveereignis vom Typ „Standard“ oder „Premium1080p“ |
| --- | --- | --- |
| Die Single-Bitrate-Eingabe wird in mehreren Bitraten in der Cloud codiert. |Nein |Ja |
| Maximale Videoauflösung für Beitragsfeeds |4K (4096 × 2160 bei 60 Frames/Sekunde) |1080p (1920 x 1088 bei 30 Frames/Sekunde)|
| Empfohlene maximale Anzahl von Ebenen für Beitragsfeeds|Bis zu 12|Eine Audioebene|
| Maximale Anzahl von Ebenen in der Ausgabe| Identisch mit der Eingabe|Bis zu 6 (siehe unten „Systemvoreinstellungen“)|
| Maximale aggregierte Bandbreite von Beitragsfeeds|60 MBit/s|–|
| Maximale Bitrate für eine einzelne Ebene im Beitrag |20 MBit/s|20 MBit/s|
| Unterstützung von Audiotiteln mit mehreren Sprachen|Ja|Nein|
| Unterstützte Codecs für Videoeingang |H.264/AVC und H.265/HEVC|H.264/AVC|
| Unterstützte Codecs für Videoausgang|Identisch mit der Eingabe|H.264/AVC|
| Unterstützte Videobittiefe, -eingang und -ausgang|Bis zu 10 Bit, einschließlich HDR 10/HLG|8 Bit|
| Unterstützte Codecs für Audioeingang|AAC-LC, HE-AAC v1, HE-AAC v2|AAC-LC, HE-AAC v1, HE-AAC v2|
| Unterstützte Codecs für Audioausgang|Identisch mit der Eingabe|AAC-LC|
| Maximale Videoauflösung des Ausgabevideos|Identisch mit der Eingabe|Standard (720p), Premium1080p (1080p)|
| Maximale Bildfrequenz eines Eingangsvideos|60 Frames/Sekunde|Standard oder Premium1080p: 30 Frames/Sekunde|
| Eingabeprotokolle|RTMP, fragmentiertes MP4 (Smooth Streaming)|RTMP, fragmentiertes MP4 (Smooth Streaming)|
| Preis|Informieren Sie sich auf der [Preisseite](https://azure.microsoft.com/pricing/details/media-services/) , und klicken Sie auf die Registerkarte „Live-Video“.|Informieren Sie sich auf der [Preisseite](https://azure.microsoft.com/pricing/details/media-services/) , und klicken Sie auf die Registerkarte „Live-Video“.|
| Maximale Laufzeit| 24 Stunden × 365 Tage, live linear | 24 Stunden × 365 Tage, live linear (Vorschauversion)|
| Möglichkeit zum Übergeben von eingebetteten CEA-608/708-Untertiteldaten|Ja|Ja|
| Unterstützung für das Einfügen von Slates|Nein|Nein|
| Unterstützung für Werbeeinblendungen über API| Nein|Nein|
| Unterstützung für Werbeeinblendungen über SCTE-35 Inband|Ja|Ja|
| Möglichkeit zum Wiederherstellen nach kurzen Unterbrechungen im Beitrag|Ja|Teilweise|
| Unterstützung für nicht einheitliche Eingabe-GOPs|Ja|Nein, Eingabe erfordert feste GOP-Dauer|
| Unterstützung für Eingaben mit variable Bildwiederholrate|Ja|Nein, Eingabe muss eine feste Bildfrequenz aufweisen. Kleinere Abweichungen, beispielsweise bei Szenen mit viel Bewegung, werden toleriert. Der Beitragsfeed kann die Bildfrequenz jedoch nicht verringern (z.B. auf 15 Frames/Sekunde).|
| Automatische Abschaltung des Liveereignisses, wenn der Eingabefeed verloren geht|Nein|Nach 12 Stunden, wenn kein LiveOutput erfolgt|

## <a name="system-presets"></a>Systemvoreinstellungen

Die Auflösungen und Bitraten, die in der Ausgabe vom Liveencoder enthalten sind, sind durch [presetName](https://docs.microsoft.com/rest/api/media/liveevents/create#liveeventencoding) bestimmt. Wird ein **Standard**-Liveencoder (LiveEventEncodingType.Standard) verwendet, gibt die Voreinstellung *Default720p* einen Satz von 6 Auflösung/Bitrate-Paaren an, der weiter unten beschrieben ist. Wird ein **Premium1080p**-Liveencoder (LiveEventEncodingType.Premium1080p) verwendet, gibt die Voreinstellung *Default1080p* den Ausgabesatz der Auflösung/Bitrate-Paare an.

> [!NOTE]
> Sie können die Voreinstellung „Default1080p“ nicht auf ein Liveereignis anwenden, das für eine Livecodierung des Typs „Standard“ konfiguriert ist – Sie erhalten einen Fehler. Sie erhalten auch einen Fehler, wenn Sie versuchen, die Voreinstellung „Default720p“ auf einen „Premium1080p“-Liveencoder anzuwenden.

### <a name="output-video-streams-for-default720p"></a>Videoausgabedatenströme für „Default720p“

Hat der Beitragsfeed eine Auflösung von 720p oder höher, bewirkt die Voreinstellung **Default720p**, dass der Feed in den folgenden 6 Ebenen codiert wird. In der folgenden Tabelle ist die „Bitrate“ in KBit/s angegeben, entspricht „Max. Bilder/s“ der maximal zulässigen Bildfrequenz (in Frames/Sekunde), und entspricht „Profil“ dem verwendeten H.264-Profil.

| Bitrate | Breite | Höhe | Max. Bilder/s | Profil |
| --- | --- | --- | --- | --- |
| 3500 |1280 |720 |30 |Hoch |
| 2200 |960 |540 |30 |Hoch |
| 1350 |704 |396 |30 |Hoch |
| 850 |512 |288 |30 |Hoch |
| 550 |384 |216 |30 |Hoch |
| 200 |340 |192 |30 |Hoch |

> [!NOTE]
> Wenn Sie die Voreinstellung für Livecodierung anpassen müssen, öffnen Sie ein Supportticket über das Azure-Portal. Geben Sie hierbei die gewünschte Tabelle mit den Angaben zur Auflösung und zu den Bitraten an. Vergewissern Sie sich, dass nur eine Ebene mit 720p und maximal sechs Ebenen vorhanden sind. Geben Sie außerdem an, dass Sie eine Voreinstellung für einen „Standard“-Liveencoder anfordern.
> Die speziellen Werte der Bitraten und Auflösungen können im Laufe der Zeit angepasst werden.

### <a name="output-video-streams-for-default1080p"></a>Videoausgabedatenströme für „Default1080p“

Hat der Beitragsfeed eine Auflösung von 1080p oder höher, bewirkt die Voreinstellung **Default1080p**, dass der Feed in den folgenden 6 Ebenen codiert wird.

| Bitrate | Breite | Höhe | Max. Bilder/s | Profil |
| --- | --- | --- | --- | --- |
| 5500 |1920 |1080 |30 |Hoch |
| 3000 |1280 |720 |30 |Hoch |
| 1600 |960 |540 |30 |Hoch |
| 800 |640 |360 |30 |Hoch |
| 400 |480 |270 |30 |Hoch |
| 200 |320 |180 |30 |Hoch |

> [!NOTE]
> Wenn Sie die Voreinstellung für Livecodierung anpassen müssen, öffnen Sie ein Supportticket über das Azure-Portal. Geben Sie hierbei die gewünschte Tabelle mit den Angaben zur Auflösung und zu den Bitraten an. Vergewissern Sie sich, dass nur eine Ebene mit 1080p und maximal sechs Ebenen vorhanden sind. Geben Sie außerdem an, dass Sie eine Voreinstellung für einen „Premium1080p“-Liveencoder anfordern.
> Die speziellen Werte der Bitraten und Auflösungen können im Laufe der Zeit angepasst werden.

### <a name="output-audio-stream-for-default720p-and-default1080p"></a>Audioausgabedatenstrom für „Default720p“ und „Default1080p“

Sowohl für die Voreinstellung *Default720p* als auch für die Voreinstellung *Default1080p* wird Audio in Stereo-AAC-LC mit 128 KBit/s codiert. Die Samplingrate folgt derjenigen der Audiospur im Beitragsfeed.

## <a name="implicit-properties-of-the-live-encoder"></a>Implizite Eigenschaften des Liveencoders

Im vorherigen Abschnitt sind die Eigenschaften des Liveencoders beschrieben, die explizit über die Voreinstellung gesteuert werden können – z.B. die Anzahl der Ebenen, Auflösungen und Bitraten. In diesem Abschnitt sind die impliziten Eigenschaften beschrieben.

### <a name="group-of-pictures-gop-duration"></a>Gruppe von Bildern-Dauer (GOP-Dauer)

Der Liveencoder folgt der [GOP](https://en.wikipedia.org/wiki/Group_of_pictures)-Struktur des Beitragsfeeds, d. h., die Ausgabeebenen haben dieselbe GOP-Dauer. Daher sollten Sie den lokalen Encoder so konfigurieren, dass er einen Beitragsfeed erzeugt, der eine feste GOP-Dauer hat (üblicherweise 2 Sekunden). Dadurch ist sichergestellt, dass die aus dem Dienst ausgehenden HLS- und MPEG DASH-Datenströme ebenfalls feste GOP-Dauern haben. Kleine Abweichungen der GOP-Dauern werden von den meisten Geräten wahrscheinlich toleriert.

### <a name="frame-rate"></a>Bildfrequenz

Der Liveencoder folgt auch den Dauern der einzelnen Videoframes im Beitragsfeed. Dies bedeutet, dass die Ausgabeebenen Frames mit denselben Dauern haben. Daher sollten Sie den lokalen Encoder so konfigurieren, dass er einen Beitragsfeed erzeugt, der eine feste Bildfrequenz hat (meistens 30 Frames/Sekunde). Dadurch ist sichergestellt, dass die aus dem Dienst ausgehenden HLS- und MPEG DASH-Datenströme ebenfalls feste Dauern für die Bildfrequenz haben. Kleine Abweichungen bei den Bildfrequenzen werden von den meisten Geräten möglicherweise toleriert, es gibt aber keine Garantie, dass der Liveencoder eine Ausgabe erzeugt, die korrekt wiedergegeben wird. Ihr lokaler Liveencoder sollte weder in irgendeiner Weise Frames verwerfen (z.B. bei niedriger Akkuladung) noch in irgendeiner Weise die Bildfrequenz variieren.

### <a name="resolution-of-contribution-feed-and-output-layers"></a>Auflösung von Beitragsfeed und Ausgabeebenen

Der Liveencoder ist so konfiguriert, dass übermäßiges Konvertieren des Beitragsfeeds vermieden wird. Daraus folgt, dass die maximale Auflösung der Ausgabeebenen nicht größer wird als die Auflösung des Beitragsfeeds.

Wenn Sie beispielsweise einen Beitragsfeed mit 720p an ein Liveereignis senden, das für eine „Default1080p“-Livecodierung konfiguriert ist, hat die Ausgabe nur 5 Ebenen, und zwar beginnend mit 720p bei 3 MBit/s bis hinunter zu 1080p bei 200 KBit/s. Oder wenn Sie einen Beitragsfeed mit 360p an ein Liveereignis senden, das für „Standard“-Livecodierung konfiguriert ist, enthält die Ausgabe 3 Ebenen (mit Auflösungen von 288p, 216p und 192p). Wenn Sie im degenerierten Fall einen Beitragsfeed von beispielsweise 160x90 Pixeln an einen „Standard“-Liveencoder senden, enthält die Ausgabe eine Ebene mit einer 160x90-Auflösung mit derselben Bitrate wie derjenigen des Beitragsfeeds.

### <a name="bitrate-of-contribution-feed-and-output-layers"></a>Bitrate von Beitragsfeed und Ausgabeebenen

Der Liveencoder ist so konfiguriert, dass er sich an die Bitrateninstellungen in der Voreinstellung hält, unabhängig von der Bitrate des Beitragsfeeds. Daraus folgt, dass die Bitrate der Ausgabeebenen größer sein kann als die Bitrate des Beitragsfeeds. Wenn Sie beispielsweise einen Beitragsfeed mit einer Auflösung von 720p bei 1 MBit/s senden, bleiben die Ausgabeebenen dieselben wie in der obigen [Tabelle](live-event-types-comparison.md#output-video-streams-for-default720p).

## <a name="next-steps"></a>Nächste Schritte

[Übersicht über das Livestreaming](live-streaming-overview.md)
