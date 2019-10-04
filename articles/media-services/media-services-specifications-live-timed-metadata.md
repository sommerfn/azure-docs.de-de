---
title: Azure Media Services – Signalisierung von zeitgesteuerten Metadaten beim Livestreaming | Microsoft-Dokumentation
description: In dieser Spezifikation werden Methoden zur Signalisierung von zeitgesteuerten Metadaten beim Erfassen und Streamen von Daten in Azure Media Services dargelegt. Dies schließt Unterstützung für Signale für generische zeitgesteuerte Metadaten (ID3) sowie SCTE-35-Signalisierung für die Einfügung von Werbeinhalten und Signalisierung für Einblendungsbedingung ein.
services: media-services
documentationcenter: ''
author: johndeu
manager: femila
editor: johndeu
ms.assetid: 265b94b1-0fb8-493a-90ec-a4244f51ce85
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2019
ms.author: johndeu
ms.openlocfilehash: df2a86dd1292f58511765e842ee97daddcff4e3e
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70102918"
---
# <a name="signaling-timed-metadata-in-live-streaming"></a>Signalisieren von zeitgesteuerten Metadaten beim Livestreaming 

Letzte Aktualisierung: 2019-08-22

### <a name="conformance-notation"></a>Konformität der Schlüsselbegriffe

Die Schlüsselwörter "MUSS", "DARF NICHT", "ERFORDERLICH", "SOLL", "SOLL NICHT", "SOLLTE", "SOLLTE NICHT", "EMPFOHLEN", "KANN" und "OPTIONAL" in diesem Dokument sind wie in RFC 2119 zu verstehen.

## <a name="1-introduction"></a>1. Einführung 

Um die Einfügung von Werbeinhalten oder benutzerdefinierten Metadatenereignissen auf Clientplayern zu signalisieren, nutzen Broadcaster oftmals im Video eingebettete Metadaten mit Zeitinformationen. Um diese Szenarien zu ermöglichen, bieten Media Services Unterstützung für den Transport von zeitgesteuerten Metadaten vom Einspeisepunkt des Livestreamingkanals bis zur Clientanwendung.
In dieser Spezifikation werden mehrere Modi allgemein beschrieben, die von Media Services für die Signalisierung von zeitgesteuerten Metadaten beim Livestreaming verwendet werden.

1. [SCTE-35] Signalisierung, die die in [SCTE-35], [SCTE-214-1], [SCTE-214-3] und [RFC8216] beschriebenen Standards erfüllt.

2. [SCTE-35] Signalisierung, die die Legacy [Adobe-Primetime]-Spezifikation für die RTMP-Werbungssignalisierung erfüllt.
   
3. Ein generischer, zeitgesteuerter Metadatensignalisierungsmodus für Nachrichten, die **NICHT** [SCTE-35] sind und [ID3v2] oder andere benutzerdefinierte Schemas enthalten könnten, die vom Anwendungsentwickler definiert werden.

## <a name="11-terms-used"></a>1.1 Terminologie

| Begriff              | Definition |
|-------------------|------------|
| Werbeunterbrechung          | Ein Ort oder ein Zeitpunkt, an dem ein oder mehrere Werbeeinblendungen zur Präsentation geplant sein können. Identisch mit der Verfügbarkeits- und Platzierungsmöglichkeit. |
| Werbungsentscheidungsdienst| Externer Dienst, der entscheidet, welche Werbung(en) und Dauer dem Benutzer angezeigt werden. Die Dienste werden in der Regel von einem Partner bereitgestellt und sprengen den Rahmen dieses Dokuments.|
| Position (Cue)               | Angabe von Zeit und Parametern der bevorstehenden Werbeunterbrechung. Beachten Sie, dass Cues einen ausstehenden Wechsel zu einer Werbeunterbrechung, einen ausstehenden Wechsel zur nächsten Werbung innerhalb einer Werbeunterbrechung sowie einen ausstehenden Wechsel von einer Werbeunterbrechung zurück zum Hauptinhalt anzeigen können. |
| Packager          | Der „Streamingendpunkt“ der Azure Media Services stellt dynamische Paketerstellungsfunktionen für DASH und HLS bereit und wird in der Medienbranche als „Packager“ bezeichnet. 
| Präsentationszeit | Die Zeit, zu der ein Ereignis einem Benutzer präsentiert wird. Diese Zeit stellt den Moment auf der Medienzeitachse dar, in dem ein Betrachter das Ereignis zu sehen bekommt. Beispielsweise ist die Präsentationszeit für einen SCTE-35-Befehl „splice_info()“ die „splice_time()“. |
| Ankunftszeit      | Der Zeitpunkt, zu dem eine Ereignismeldung eintrifft. Diese Zeit unterscheidet sich normalerweise von der Präsentationszeit des Ereignisses, da Ereignismeldungen vor der Präsentationszeit des Ereignisses gesendet werden.                                     |
| Platzsparende Spur      | Medienspur, die nicht fortlaufend ist und mit einer übergeordneten Spur oder Steuerungsspur zeitsynchronisiert ist.                                                                                                                                    |
| Origin            | Der Azure Media-Streamingdienst                                                                                                                                                                                                |
| Kanalsenke      | Der Azure Media-Livestreamingdienst                                                                                                                                                                                           |
| HLS               | Apple HTTP Live Streaming-Protokoll                                                                                                                                                                                               |
| DASH              | Dynamisches adaptives Streaming über HTTP                                                                                                                                                                                             |
| Smooth            | Smooth Streaming-Protokoll                                                                                                                                                                                                        |
| MPEG2-TS          | MPEG-2-Transportdatenströme                                                                                                                                                                                                         |
| RTMP              | Real-Time Multimedia Protocol (Echtzeit-Multimediaprotokoll)                                                                                                                                                                                                    |
| uimsbf            | Unsigned integer, most significant bit first (Ganzzahl ohne Vorzeichen, höchstwertiges Bit zuerst).                                                                                                                                                                                    |

---

## <a name="12-normative-references"></a>1.2 Normative Verweise

Die folgenden Dokumente enthalten Bestimmungen, die durch Verweise innerhalb dieses Texts Bestimmungen dieses Dokuments bilden. Alle Dokumente unterliegen der Überarbeitung durch die Standardisierungsorganisationen, und Leser werden aufgefordert, eventuell verfügbare neueste Ausgaben der unten aufgeführten Dokumente zu recherchieren, um diese dann auch anzuwenden. Leser werden außerdem daran erinnert, dass neuere Ausgaben der Dokumente, auf die verwiesen wird, möglicherweise nicht mit dieser Version der Spezifikation für zeitgesteuerte Metadaten für Azure Media Services kompatibel sind.


|Standard  |Definition  |
|---------|---------|
|[Adobe-Primetime] | [Primetime Digital Program Insertion Signaling Specification 1.2 (Spezifikation für digitale Programmeinfügungssignalisierung zur Primetime)](https://www.adobe.com/content/dam/acom/en/devnet/primetime/PrimetimeDigitalProgramInsertionSignalingSpecification.pdf) |
|[Adobe-Flash-AS] | [FLASH ActionScript Language Reference (FLASH ActionScript-Sprachreferenz)](https://help.adobe.com/archive/en_US/as2/flashlite_2.x_3.x_aslr.pdf) |
| [AMF0]            | [„Action Message Format AMF0“ (Aktionsnachrichtenformat AMF0)](https://download.macromedia.com/pub/labs/amf/amf0_spec_121207.pdf) |
| [DASH-IF-IOP]     | DASH Industry Forum Interop Guidance v 4.2 (DASH-Industrieforums-Interop-Anleitung v 4.2) [https://dashif-documents.azurewebsites.net/DASH-IF-IOP/master/DASH-IF-IOP.html](https://dashif-documents.azurewebsites.net/DASH-IF-IOP/master/DASH-IF-IOP.html) |
| [HLS-TMD]         | Timed Metadata for HTTP Live Streaming (Zeitgesteuerte Metadaten für HTTP-Livestreaming) – [https://developer.apple.com/streaming](https://developer.apple.com/streaming) |
| [CMAF-ID3]         | [Timed Metadata in the Common Media Application Format (CMAF) (Zeitgesteuerte Metadaten im allgemeinen Medienanwendungsformat)](https://aomediacodec.github.io/av1-id3/)
| [ID3v2]           | ID3 Tag Version 2.4.0 [http://id3.org/id3v2.4.0-structure](http://id3.org/id3v2.4.0-structure) |
| [ISO-14496-12]    | ISO/IEC 14496-12: Part 12 ISO base media file format, Fourth Edition 2012-07-15 (Teil 12 ISO-Mediendatei-Basisformat, 4. Auflage, 15. Juli 2012)  |
| [MPEGDASH]        | Information Technology – Dynamic adaptive streaming over HTTP (Informationstechnologie – Dynamisches adaptives Streaming über HTTP) (DASH) – Part 1 (Teil 1): Media Presentation description und segment formats (Medienpräsentationsbeschreibung und -segmentformate). Mai 2014. Veröffentlicht. URL: https://www.iso.org/standard/65274.html |
| [MPEGCMAF]        | Information Technology – Multimedia application format (MPEG-A) (Informationstechnologie – Multimediaanwendungsformat) – Part 19 (Teil 19): Common media application format (CMAF) for segmented media (Allgemeines Medienanwendungsformat (CMAF) für segmentierte Medien). Januar 2018. Veröffentlicht. URL: https://www.iso.org/standard/71975.html |
| [MPEGCENC]        | Information Technology – MPEG systems technologies (Informationstechnologie – MPEG-Systemtechnologien) – Part 7 (Teil 7): Common encryption in ISO base media file format files (Allgemeine Verschlüsselung in ISO-Basismediendateiformat-Dateien) Februar 2016. Veröffentlicht. URL: https://www.iso.org/standard/68042.html |
| [MS-SSTR]         | [„Microsoft Smooth Streaming Protocol“, 15. Mai 2014](https://docs.microsoft.com/openspecs/windows_protocols/ms-sstr/8383f27f-7efe-4c60-832a-387274457251) |
| [MS-SSTR-Ingest]  | [Spezifikation der Fragmented MP4-Echtzeiterfassung für Azure Media Services](https://docs.microsoft.com/azure/media-services/media-services-fmp4-live-ingest-overview) |
| [RFC8216]         | R. Pantos, Ed.; W. May. HTTP Live Streaming. August 2017. Zur Information. [https://tools.ietf.org/html/rfc8216](https://tools.ietf.org/html/rfc8216) |
| [RFC4648]         |The Base16, Base32, and Base64 Data Encodings (Die Datencodierungen Base16, Base32 und Base64) – [https://tools.ietf.org/html/rfc4648](https://tools.ietf.org/html/rfc4648) |
| [RTMP]            |[„Adobe’s Real-Time Messaging Protocol“, 21. Dezember 2012 (Echtzeit-Nachrichtenprotokoll von Adobe)](https://www.adobe.com/devnet/rtmp.html)  |
| [SCTE-35-2019]    | SCTE 35: 2019 – Digital Program Insertion Cueing Message for Cable (Positionsnachricht für die Einfügung von digitalen Programmen im Kabel) – https://www.scte.org/SCTEDocs/Standards/ANSI_SCTE%2035%202019r1.pdf  |
| [SCTE-214-1]      | SCTE 214-1 2016 – MPEG DASH for IP-Based Cable Services (MPEG DASH für IP-basierte Kabeldienste) Part 1 (Teil 1): MPD Constraints and Extensions (MPD-Einschränkungen und -Erweiterungen) |
| [SCTE-214-3]      | SCTE 214-3 2015 – MPEG DASH for IP-Based Cable Services (MPEG DASH für IP-basierte Kabeldienste) Part 3 (Teil 3): DASH/FF Profile |
| [SCTE-224]        | SCTE 224 2018r1 – Event Scheduling and Notification Interface (Schnittstelle für Ereignisplanung und -benachrichtigung) |
| [SCTE-250]        | Event and Signaling Management API (ESAM) (Ereignis- und Signalisierungsverwaltungs-API) |

---


## <a name="2-timed-metadata-ingest"></a>2. Erfassung von zeitgesteuerten Metadaten

Azure Media Services unterstützt In-Band-Metadaten in Echtzeit für die [RTMP]- und [MS-SSTR-Ingest]-Protokolle (Smooth Streaming). Echtzeit-Metadaten können verwendet werden, um benutzerdefinierte Ereignisse mit eigenen benutzerdefinierten Schemas (JSON, Binary, XML) sowie branchendefinierten Formaten wie ID3 oder SCTE-35 für die Werbesignalisierung in einem Broadcaststream zu definieren. 

Dieser Artikel enthält die Details zum Senden von benutzerdefinierten Metadatensignalen mithilfe der unterstützten Erfassungsprotokolle von Azure Media Services. In diesem Artikel wird auch erläutert, wie die Manifeste für HLS, DASH und Smooth Streaming mit den zeitgesteuerten Metadatensignalen ausgestattet sind. Zudem ist eine Erläuterung enthalten, wie sie bandintern übertragen werden, wenn der Inhalt über CMAF- (MP4-Fragmente) oder TS-Segmente (Transportdatenstrom) für HLS bereitgestellt wird. 

Allgemeine Anwendungsszenarien für zeitgesteuerte Metadaten sind:

 - SCTE-35-Werbesignale zum Auslösen von Werbeunterbrechungen in einem Liveereignis oder bei einer linearen Übertragung
 - Benutzerdefinierte ID3-Metadaten, mit denen Ereignisse bei einer Clientanwendung (Browser, iOS oder Android) ausgelöst werden können
 - Benutzerdefinierte JSON-, Binär- oder XML-Metadaten zum Auslösung von Ereignissen in einer Clientanwendung
 - Telemetrie von einem Liveencoder, einer IP-Kamera oder Drohne
 - Ereignisse von einer IP-Kamera wie Bewegungsabläufe, Gesichtserkennung usw.
 - Geografische Positionsinformationen von einer Actionkamera, einer Drohne oder einem sich bewegenden Gerät
 - Liedtexte
 - Programmgrenzen bei einem linearen Livefeed
 - Bilder oder ergänzende Metadaten, die in einem Livefeed angezeigt werden sollen
 - Sportergebnisse oder Zeitinformationen zu Spielen
 - Interaktive Werbepakete, die neben dem Video im Browser angezeigt werden sollen
 - Quiz oder Umfragen
  
Azure Media Services Live Events und Packager sind in der Lage, diese zeitgesteuerten Metadatensignale zu empfangen und in einen Metadatenstrom umzuwandeln, der Clientanwendungen über standardbasierte Protokolle wie HLS und DASH erreichen kann.


## <a name="21-rtmp-timed-metadata"></a>2.1 RTMP – Zeitgesteuerte Metadaten

Das [RTMP]-Protokoll ermöglicht das Senden von zeitgesteuerten Metadatensignalen für verschiedene Szenarien, einschließlich benutzerdefinierter Metadaten und SCTE-35-Werbesignalen. 

Werbesignale (Positionsnachrichten) werden als [AMF0]-Positionsnachrichten gesendet, die in den [RTMP]-Stream eingebettet sind. Das Senden der Positionsnachrichten kann einige Zeit vor dem erforderlichen Eintreten des eigentlichen Ereignisses oder [SCTE35] dem Werbeeinblendungssignal erfolgen. Zur Unterstützung dieses Szenarios wird der tatsächliche Präsentationszeitstempel des Ereignisses innerhalb der Positionsnachricht gesendet. Weitere Informationen finden Sie unter [AMF0].

Die folgenden [AMF0]-Befehle werden von Azure Media Services für die RTMP-Erfassung unterstützt:

- **onUserDataEvent** – Wird für benutzerdefinierte Metadaten oder zeitgesteuerte [ID3v2]-Metadaten verwendet.
- **onAdCue** – Wird hauptsächlich zur Signalisierung einer Werbeplatzierungsmöglichkeit im Livestream verwendet. Es werden zwei Formen der Position unterstützt, ein einfacher und ein SCTE-35-Modus. 
- **onCuePoint** – Wird von bestimmten lokalen Hardwareencodern, wie dem Elemental Liveencoder, unterstützt, um [SCTE35]-Nachrichten zu signalisieren. 
  

In der folgenden Tabelle ist das Format der AMF-Nachrichtennutzlast beschrieben, die von Media Services sowohl für den „einfachen“ als auch für den [SCTE35]-Nachrichtenmodus erfasst wird.

Der Name der [AMF0]-Nachricht kann verwendet werden, um mehrere Ereignisdatenströme vom gleichen Typ zu unterscheiden.  Sowohl für [SCTE-35]-Nachrichten als auch für Nachrichten im „einfachen“ Modus MUSS der Name der AMF-Nachricht „onAdCue“ sein, wie in der [Adobe-Primetime]-Spezifikation gefordert.  Alle unten nicht aufgeführten Felder MÜSSEN von Azure Media Services bei der Erfassung ignoriert werden.

## <a name="211-rtmp-with-custom-metadata-using-onuserdataevent"></a>2.1.1 RTMP mit benutzerdefinierten Metadaten mithilfe von „onUserDataEvent“

Wenn Sie benutzerdefinierte Metadatenfeeds von Ihrem vorgeschalteten Encoder, Ihrer IP-Kamera, Ihrer Drohne oder Ihrem Gerät über das RTMP-Protokoll bereitstellen möchten, verwenden Sie den [AMF0]-Befehlstyp „onUserDataEvent“ für Datennachrichten.

Der Datennachrichtenbefehl **„onUserDataEvent“** MUSS eine Nachrichtennutzlast mit der folgenden Definition enthalten, die von Media Services erfasst und in das In-Band-Dateiformat sowie die Manifeste für HLS, DASH und Smooth Streaming verpackt werden muss.
Es wird empfohlen, zeitgesteuerte Metadatennachrichten nicht häufiger als einmal alle 0,5 Sekunden (500 ms) zu senden, da sonst Stabilitätsprobleme beim Livestream auftreten können. Jede Nachricht könnte Metadaten aus mehreren Frames aggregieren, wenn Sie Metadaten auf Frameebene bereitstellen müssen. Wenn Sie Datenströme mit Mehrfachbitrate senden, wird empfohlen, die Metadaten auch nur für eine Einzelbitrate bereitzustellen, um die Bandbreite zu reduzieren und Störungen bei der Video-/Audioverarbeitung zu vermeiden. 

Die Nutzlast für **„onUserDataEvent“** sollte eine Nachricht im [MPEGDASH]-EventStream-XML-Format sein. Dadurch ist es einfach, benutzerdefinierte Schemas per In-Band-Übermittlung in „emsg“-Nutzlasten für CMAF [MPEGCMAF]-Inhalte zu übergeben, die über HLS- oder DASH-Protokolle bereitgestellt werden. Jede DASH Event Stream-Nachricht enthält ein „schemeIdUri“, das als URN-Nachrichtenschemabezeichner fungiert und die Nutzlast der Nachricht definiert. Einige Schemas wie „https://aomedia.org/emsg/ID3“ für [ID3v2] oder **urn:scte:scte35:2013:scte35:2013:bin** für [SCTE-35] werden von Industriekonsortien zwecks Interoperabilität standardisiert. Jeder Anwendungsanbieter kann sein eigenes benutzerdefiniertes Schema unter Verwendung einer von ihm kontrollierten URL (eigene Domäne) definieren und kann bei Bedarf eine Spezifikation unter dieser URL bereitstellen. Wenn ein Player über einen Handler für das definierte Schema verfügt, dann ist dies die einzige Komponente, die die Nutzlast und das Protokoll verstehen muss.

Das Schema für die [MPEG-DASH] EventStream-XML-Nutzlast ist definiert als (Auszug aus DASH ISO-IEC-23009-1-3rd Edition). Beachten Sie, dass derzeit nur ein „EventType“ pro „EventStream“ unterstützt wird. Nur das erste **Event**-Element wird verarbeitet, wenn mehrere Ereignisse im **EventStream** bereitgestellt werden.

```xml
  <!-- Event Stream -->
  <xs:complexType name="EventStreamType">
    <xs:sequence>
      <xs:element name="Event" type="EventType" minOccurs="0" maxOccurs="unbounded"/>
      <xs:any namespace="##other" processContents="lax" minOccurs="0" maxOccurs="unbounded"/>
    </xs:sequence>
    <xs:attribute ref="xlink:href"/>
    <xs:attribute ref="xlink:actuate" default="onRequest"/>
    <xs:attribute name="schemeIdUri" type="xs:anyURI" use="required"/>
    <xs:attribute name="value" type="xs:string"/>
    <xs:attribute name="timescale" type="xs:unsignedInt"/>
  </xs:complexType>
  <!-- Event  -->
  <xs:complexType name="EventType">
    <xs:sequence>
      <xs:any namespace="##other" processContents="lax" minOccurs="0" maxOccurs="unbounded"/>
    </xs:sequence>
    <xs:attribute name="presentationTime" type="xs:unsignedLong" default="0"/>
    <xs:attribute name="duration" type="xs:unsignedLong"/>
    <xs:attribute name="id" type="xs:unsignedInt"/>
    <xs:attribute name="contentEncoding" type="ContentEncodingType"/>
    <xs:attribute name="messageData" type="xs:string"/>
    <xs:anyAttribute namespace="##other" processContents="lax"/>
  </xs:complexType>
```


### <a name="example-xml-event-stream-with-id3-schema-id-and-base64-encoded-data-payload"></a>XML Event Stream-Beispiel mit ID3-Schema-ID und base64-codierter Datennutzlast.  
```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <EventStream schemeIdUri="https://aomedia.org/emsg/ID3">
         <Event contentEncoding="Base64">
          -- base64 encoded ID3v2 full payload here per [CMAF-TMD] --
         </Event>
   <EventStream>
```

### <a name="example-event-stream-with-custom-schema-id-and-base64-encoded-binary-data"></a>Event Stream-Beispiel mit benutzerdefinierter Schema-ID und base64-codierten Binärdaten.  
```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <EventStream schemeIdUri="urn:example.org:custom:binary">
         <Event contentEncoding="Base64">
          -- base64 encoded custom binary data message --
         </Event>
   <EventStream>
```

### <a name="example-event-stream-with-custom-schema-id-and-custom-json"></a>Event Stream-Beispiel mit benutzerdefinierter Schema-ID und benutzerdefiniertem JSON.  
```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <EventStream schemeIdUri="urn:example.org:custom:JSON">
         <Event>
          [
            {"key1" : "value1"},
            {"key2" : "value2"}
          ]
         </Event>
   <EventStream>
```

### <a name="built-in-supported-scheme-id-uris"></a>Integrierte unterstützte Schema-ID-URIs
| Schema-ID-URI                 |  BESCHREIBUNG                                             |
|-------------------------------|----------------------------------------------------------|
| https:\//aomedia.org/emsg/ID3   | Beschreibt, wie [ID3v2]-Metadaten als zeitgesteuerte Metadaten in CMAF-kompatiblem [MPEGCMAF]-fragmentierten MP4 übertragen werden können. Weitere Informationen finden Sie unter [Timed Metadata in the Common Media Application Format (CMAF) (Zeitgesteuerte Metadaten im allgemeinen Medienanwendungsformat)](https://aomediacodec.github.io/av1-id3/) |

### <a name="event-processing-and-manifest-signaling"></a>Ereignisverarbeitung und Manifestsignalisierung

Nach Erhalt eines gültigen **„onUserDataEvent“** -Ereignisses sucht Azure Media Services nach einer gültigen XML-Nutzlast, die mit dem „EventStreamType“ (definiert in [MPEGDASH]) übereinstimmt, analysiert die XML-Nutzlast und konvertiert sie in ein [MPEGCMAF]-MP4-Fragmentfeld „emsg“ Version 1 zur Speicherung im Livearchiv und Übertragung an den Media Services Packager.   Der Packager erkennt das „emsg“-Feld im Livestream und führ folgende Aktionen aus:

- (a) „Dynamisches Verpacken“ in TS-Segmenten für die Bereitstellung für HLS-Clients gemäß der HLS-Spezifikationen für zeitgesteuerte Metadaten [HLS-TMD] oder
- (b) Weiterleiten zur Bereitstellung in CMAF-Fragmenten über HLS oder DASH oder 
- (c) Konvertieren in ein platzsparenden Spursignal für die Übermittlung über Smooth Streaming [MS-SSTR].

Zusätzlich zu den In-Band-CMAF- oder TS-PES-Paketen im emsg-Format für HLS enthalten die Manifeste für DASH (MPD) und Smooth Streaming einen Verweis auf die In-Band-Ereignisdatenströme (im Smooth Streaming auch als „platzsparende Datenstromspur“ bezeichnet). 

Einzelne Ereignisse oder deren Datennutzlasten werden NICHT direkt in den HLS-, DASH- oder Smooth-Manifesten ausgegeben. 

### <a name="additional-informational-constraints-and-defaults-for-onuserdataevent-events"></a>Zusätzliche informative Einschränkungen und Voreinstellungen für onUserDataEvent-Ereignisse

- Wenn die Zeitskala im EventStream-Element nicht festgelegt ist, wird standardmäßig die Zeitskala „RTMP 1 kHz“ verwendet.
- Die Übermittlung einer onUserDataEvent-Nachricht ist auf maximal einmal alle 500 ms begrenzt. Wenn Sie Ereignisse häufiger senden, kann dies Auswirkungen auf die Bandbreite und die Stabilität des Livefeeds haben.

## <a name="212-rtmp-ad-cue-signaling-with-oncuepoint"></a>2.1.2 RTMP – Werbepositionssignalisierung mit „onCuePoint“

Azure Media Services können auf mehrere [AMF0]-Nachrichtentypen lauschen und auf diese reagieren, die zum Signalisieren verschiedener in Echtzeit synchronisierter Metadaten im Livestream verwendet werden können.  Die [Adobe-Primetime]-Spezifikation definiert zwei Positionstypen (Cues), bezeichnet als „einfacher“ (simple) und „SCTE-35“-Modus. Im „einfachen“ Modus unterstützt Media Services eine einzelne AMF-Positionsnachricht namens „onAdCue“, indem eine Nutzlast verwendet wird, die der nachfolgenden Tabelle entspricht, die für das „Simple Mode“-Signal definiert ist.  

Im folgenden Abschnitt wird die RTMP-Nutzlast des „einfachen“ Modus gezeigt, die verwendet werden kann, um ein einfaches „spliceOut“-Werbungssignal zu signalisieren, das in das Clientmanifest für HLS, Dash und Microsoft Smooth Streaming übertragen wird. Dies ist sehr nützlich für Szenarien, in denen der Kunde über keine komplexe SCTE-35-basierte Signalisierung für Werbungsbereitstellung oder Werbungseinfügung verfügt und einen einfachen lokalen Encoder verwendet, um die Positionsnachricht über eine API hineinzusenden. In der Regel unterstützt der lokale Encoder eine REST-basierte API, um dieses Signal auszulösen, wodurch auch der Videostream „splice-konditioniert“ wird, indem ein IDR-Frame in das Video einfügt und eine neue GOP gestartet wird.

## <a name="213--rtmp-ad-cue-signaling-with-oncuepoint---simple-mode"></a>2.1.3 RTMP – Werbepositionssignalisierung mit „onCuePoint“ – Einfacher Modus

| Feldname | Feldtyp | Erforderlich? | Beschreibungen                                                                                                             |
|------------|------------|----------|--------------------------------------------------------------------------------------------------------------------------|
| cue        | Zeichenfolge     | Erforderlich | Die Ereignisnachricht.  Ist „SpliceOut“, um eine einfache Einfügung anzugeben.                                              |
| id         | Zeichenfolge     | Erforderlich | Ein eindeutiger Bezeichner, der die Einfügung oder das Segment beschreibt. Identifiziert diese Instanz der Nachricht                            |
| duration   | Number     | Erforderlich | Die Dauer der Einfügung. Die Einheit sind Sekundenbruchteile.                                                                |
| elapsed    | Number     | Optional | Wenn das Signal zur Unterstützung der Abstimmung wiederholt wird, gibt dieses Feld die Menge der Präsentationszeit an, die seit dem Anfang der Einfügung verstrichen ist. Die Einheit sind Sekundenbruchteile. Bei der Verwendung des einfachen Modus sollte dieser Wert die ursprüngliche Dauer der Einfügung nicht überschreiten.                                                  |
| time       | Number     | Erforderlich | Die Zeit der Einfügung in der Präsentationszeit. Die Einheit sind Sekundenbruchteile.                                     |

---
 
## <a name="214-rtmp-ad-cue-signaling-with-oncuepoint---scte-35-mode"></a>2.1.4 RTMP – Werbepositionssignalisierung mit „onCuePoint“ – SCTE-35-Modus

Wenn Sie mit einem komplexeren Broadcastproduktions-Workflow arbeiten, für den die vollständige SCTE-35-Nutzlastnachricht in das HLS- oder DASH-Manifest übertragen werden muss, empfiehlt es sich, den „SCTE-35-Modus“ der [Adobe-Primetime]-Spezifikation zu verwenden.  Dieser Modus unterstützt In-Band-SCTE-35-Signale, die direkt in einen lokalen Liveencoder gesendet werden, der dann die Signale als Ausgabe in den RTMP-Stream codiert, indem er den „SCTE-35“-Modus verwendet, der in der [Adobe-Primetime]-Spezifikation angegeben ist. 

Normalerweise können SCTE-35-Nachrichten nur in MPEG-2-Transportdatenstromeingaben (TS) auf einem lokalen Encoder vorkommen. Informieren Sie sich beim Hersteller Ihres Encoders hinsichtlich der Details, wie Sie eine Transportdatenstromerfassung konfigurieren müssen, die SCTE-35 enthält, und wie Sie diese für das Pass-Through an RTMP im Adobe SCTE-35-Modus aktivieren.

In diesem Szenario MUSS die folgende Nutzlast vom lokalen Encoder mithilfe des [AMF0]-Nachrichtentyps **„onAdCue“** gesendet werden.

| Feldname | Feldtyp | Erforderlich? | Beschreibungen                                                                                                             |
|------------|------------|----------|--------------------------------------------------------------------------------------------------------------------------|
| cue        | Zeichenfolge     | Erforderlich | Die Ereignisnachricht.  Für [SCTE-35]-Nachrichten MUSS dies die base64-codierte [RFC4648] binäre „splice_info_section()“ sein, damit Nachrichten an HLS-, Smooth- und Dash-Clients gesendet werden können.                                              |
| type       | Zeichenfolge     | Erforderlich | Ein URN oder eine URL, der bzw. die das Nachrichtenschema angibt. Für [SCTE-35]-Nachrichten **SOLLTE** dies **„scte35“** sein, damit Nachrichten [Adobe-Primetime]-konform an HLS-, Smooth- und Dash-Clients gesendet werden können. Optional kann auch der URN „urn:scte:scte35:2013:bin“ verwendet werden, um eine [SCTE-35]-Nachricht zu signalisieren. |
| id         | Zeichenfolge     | Erforderlich | Ein eindeutiger Bezeichner, der die Einfügung oder das Segment beschreibt. Gibt die Instanz der Nachricht an.  Nachrichten mit gleichbedeutender Semantik weisen den gleichen Wert auf.|
| duration   | Number     | Erforderlich | Die Dauer des Ereignisses oder der Werbeeinblendung, sofern bekannt. Wenn sie unbekannt ist, **SOLLTE** der Wert 0 sein.                                                                 |
| elapsed    | Number     | Optional | Wenn das [SCTE-35]-Werbesignal zur Abstimmung wiederholt wird, gibt dieses Feld die Menge der Präsentationszeit an, die seit dem Anfang der Einfügung verstrichen ist. Die Einheit sind Sekundenbruchteile. Im [SCTE-35]-Modus darf dieser Wert die ursprünglich angegebene Dauer der Einfügung oder des Segments überschreiten.                                                  |
| time       | Number     | Erforderlich | Die Präsentationszeit des Ereignisses oder der Werbeeinblendung.  Die Präsentationszeit und -dauer **SOLLTEN** an den Datenstrom-Zugriffspunkten (Stream Access Points, SAPs) vom Typ 1 oder 2 ausgerichtet sein, wie in [ISO-14496-12], Anhang I, beschrieben. Für HLS-Ausgang **SOLLTEN** Zeit und Dauer an den Segmentgrenzen ausgerichtet sein. Die Präsentationszeit und die Dauer verschiedener Ereignisnachrichten innerhalb des gleichen Ereignisdatenstroms DÜRFEN sich NICHT überschneiden. Die Einheit sind Sekundenbruchteile.

---
## <a name="215-rtmp-ad-signaling-with-oncuepoint-for-elemental-live"></a>2.1.5 RTMP – Werbesignalisierung mit „onCuePoint“ für Elemental Live

Der lokale Encoder von Elemental Live unterstützt Werbungsmarkierungen im RTMP-Signal. Azure Media Services unterstützt zurzeit nur den Werbungsmarkierungstyp „onCuePoint“ für RTMP.  Dies kann in den Adobe RTMP-Gruppeneinstellungen in den Einstellungen oder der API des Elemental Media-Liveencoders aktiviert werden, indem die „**ad_markers**“ auf „onCuePoint“ festgelegt werden.  Weitere Informationen finden Sie in der Dokumentation zu Elemental Live. Durch die Aktivierung dieser Funktion in der RTMP-Gruppe werden SCTE-35-Signale an die Adobe RTMP-Ausgaben übergeben, um sie von Azure Media Services verarbeiten zu lassen.

Der Nachrichtentyp „onCuePoint“ ist in [Adobe-Flash-AS] definiert und hat folgende Nutzlaststruktur, wenn er von der Elemental Live-RTMP-Ausgabe gesendet wird.


| Eigenschaft  |BESCHREIBUNG  |
|---------|---------|
|  name     | Der Name SOLLTE von Elemental Live **scte35** lauten. |
|time     |  Die Zeit in Sekunden, zu der der Positionspunkt während der Zeitachse in der Videodatei aufgetreten ist. |
| type     | Der Typ des Positionspunkts SOLLTE auf „**event**“ festgelegt werden. |
| parameters | Ein assoziatives Array aus Name/Wert-Paarzeichenfolgen, das die Informationen aus der SCTE-35-Nachricht enthält, einschließlich ID und Dauer. Diese Werte werden von Azure Media Services analysiert und in das Manifestdekorations-Tag eingeschlossen.  |


Wenn dieser Modus für Werbungsmarkierungen verwendet wird, ähnelt die HLS-Manifestausgabe dem „einfachen“ Adobe-Modus. 

### <a name="216-cancellation-and-updates"></a>2.1.6 Stornierung und Aktualisierungen

Nachrichten können durch das Senden mehrerer Nachrichten mit der gleichen Präsentationszeit und ID storniert oder aktualisiert werden. Ein Ereignis wird durch die Präsentationszeit und die ID eindeutig identifiziert, und die letzte für eine bestimmte Präsentationszeit empfangene Nachricht, die den Vorlaufbedingungen entspricht, ist die Nachricht, für die die Aktion ausgeführt wird. Das aktualisierte Ereignis ersetzt alle zuvor empfangenen Nachrichten. Die Vorlaufbedingung beträgt vier Sekunden. Aktionen werden für Nachrichten ausgeführt, die mindestens vier Sekunden vor der Präsentationszeit empfangen wurden.

## <a name="22-fragmented-mp4-ingest-smooth-streaming"></a>2.2 Fragmentierte MP4-Erfassung (Smooth Streaming)

Informationen zu den Anforderungen für die Livestreamerfassung finden Sie unter [MS-SSTR-Ingest]. Die folgenden Abschnitte enthalten Detailinformationen zur Erfassung von zeitgesteuerten Präsentationsmetadaten.  Zeitgesteuerte Präsentationsmetadaten werden als Sparsespur erfasst, die sowohl in der Live Server Manifest Box (siehe dazu MS-SSTR) als auch in der Movie Box (‚moov‘) definiert ist.  

Jedes Sparsefragment besteht aus einer Movie Fragment Box (‚moof‘) und einer Media Data Box (‚mdat‘), wobei es sich bei der mdat-Box um die binäre Nachricht handelt.

Um eine Frame-genaue Einfügung von Werbungen zu erzielen, MUSS der Encoder das Fragment bei der Präsentationszeit aufteilen, wo der Cue eingefügt werden muss.  Es MUSS ein neues Fragment erstellt werden, das mit einem neu erstellten IDR-Frame beginnt, oder Streamzugriffspunkte (SAP) vom Typ 1 oder 2, wie in [ISO-14496-12] Anhang I definiert. Dies ermöglicht es dem Azure Media Packager, ein HLS-Manifest und DASH-Manifest mit mehreren Zeiträumen zu generieren, wobei der neue Zeitraum bei der Frame-genauen, splice-konditionierten Präsentationszeit beginnt.

### <a name="221-live-server-manifest-box"></a>2.2.1 Live Server Manifest Box

Die Sparsespur **MUSS** in der Live Server Manifest Box mit dem Eintrag **\<textstream\>** deklariert werden, und dafür **MÜSSEN** die folgenden Attribute festgelegt sein:

| **Attributname** | **Feldtyp** | **Erforderlich?** | **Beschreibung**                                                                                                                                                                                                                                                 |
|--------------------|----------------|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| systemBitrate      | Number         | Erforderlich      | **MUSS** „0“ sein, was eine Spur mit einer unbekannten, variablen Bitrate angibt.                                                                                                                                                                                                 |
| parentTrackName    | Zeichenfolge         | Erforderlich      | **MUSS** der Name der übergeordneten Spur sein, an deren Zeitachse die Zeitcodes der Sparsespur ausgerichtet sind. Die übergeordnete Spur darf ihrerseits keine Sparsespur sein.                                                                                                                    |
| manifestOutput     | Boolean        | Erforderlich      | **MUSS** „true“ sein, um anzugeben, dass die Sparsespur im Smooth-Clientmanifest eingebettet ist.                                                                                                                                                               |
| Subtype            | Zeichenfolge         | Erforderlich      | **MUSS** der aus vier Buchstaben bestehende Code „DATA“ sein.                                                                                                                                                                                                                        |
| Schema             | Zeichenfolge         | Erforderlich      | **MUSS** ein URN oder eine URL sein, der bzw. die das Nachrichtenschema angibt. Für [SCTE-35]-Nachrichten **MUSS** dies „urn:scte:scte35:2013:bin“ sein, damit Nachrichten [SCTE-35]-konform an HLS-, Smooth- und Dash-Clients gesendet werden können. |
| trackName          | Zeichenfolge         | Erforderlich      | **MUSS** der Name der Sparsespur sein. Der Spurname kann verwendet werden, um mehrere Ereignisstreams mit dem gleichen Schema zu unterscheiden. Jeder eindeutige Ereignisdatenstrom **MUSS** einen eindeutigen Spurnamen aufweisen.                                                                           |
| timescale          | Number         | Optional      | **MUSS** die Zeitskala der übergeordneten Spur sein.                                                                                                                                                                                                                      |

---

### <a name="222-movie-box"></a>2.2.2 Movie Box

Die Movie Box (‚moov‘) folgt auf die Live Server Manifest Box als Teil des Datenstromheaders für eine Sparsespur.

Die „moov“-Box **SOLLTE** eine **TrackHeaderBox (‚tkhd‘)** gemäß der Definition in [ISO-14496-12] mit den folgenden Einschränkungen enthalten:

| **Feldname** | **Feldtyp**          | **Erforderlich?** | **Beschreibung**                                                                                                |
|----------------|-------------------------|---------------|----------------------------------------------------------------------------------------------------------------|
| duration       | 64-Bit-Ganzzahl ohne Vorzeichen | Erforderlich      | **SOLLTE** 0 sein, da die Spurbox 0 (null) Stichproben enthält und die Dauer der Stichproben in der Spurbox 0 ist. |

---

Die „moov“-Box **SOLLTE** eine **HandlerBox (‚hdlr‘)** gemäß der Definition in [ISO-14496-12] mit den folgenden Einschränkungen enthalten:

| **Feldname** | **Feldtyp**          | **Erforderlich?** | **Beschreibung**   |
|----------------|-------------------------|---------------|-------------------|
| handler_type   | 32-Bit-Ganzzahl ohne Vorzeichen | Erforderlich      | **SOLLTE** ‚meta‘ sein. |

---

Die „stsd“-Box **SOLLTE** eine MetaDataSampleEntry-Box mit einem gemäß [ISO-14496-12] definierten Codierungsnamen enthalten.  Beispielsweise **SOLLTE** der Codierungsname für SCTE-35-Nachrichten ‚scte‘ sein.

### <a name="223-movie-fragment-box-and-media-data-box"></a>2.2.3 Movie Fragment Box und Media Data Box

Fragmente von Sparsespuren bestehen aus einer Movie Fragment Box (‚moof‘) und einer Media Data Box (‚mdat‘).

> [!NOTE]
> Um eine Frame-genaue Einfügung von Werbungen zu erzielen, MUSS der Encoder das Fragment bei der Präsentationszeit aufteilen, wo der Cue eingefügt werden muss.  Es MUSS ein neues Fragment erstellt werden, das mit einem neu erstellten IDR-Frame beginnt, oder Streamzugriffspunkte (SAP) vom Typ 1 oder 2, wie in [ISO-14496-12] Anhang I definiert.
> 

Die MovieFragmentBox („moof“) **MUSS** eine **TrackFragmentExtendedHeaderBox („uuid“)** gemäß Definition in [MS-SSTR] mit den folgenden Feldern enthalten:

| **Feldname**         | **Feldtyp**          | **Erforderlich?** | **Beschreibung**                                                                               |
|------------------------|-------------------------|---------------|-----------------------------------------------------------------------------------------------|
| fragment_absolute_time | 64-Bit-Ganzzahl ohne Vorzeichen | Erforderlich      | **MUSS** die Ankunftszeit des Ereignisses sein. Dieser Wert richtet die Nachricht an der übergeordneten Spur aus.   |
| fragment_duration      | 64-Bit-Ganzzahl ohne Vorzeichen | Erforderlich      | **MUSS** die Dauer des Ereignisses sein. Die Dauer kann 0 (null) sein, um anzugeben, dass die Dauer unbekannt ist. |

---


Die MediaDataBox (‚mdat‘) **MUSS** das folgende Format aufweisen:

| **Feldname**          | **Feldtyp**                   | **Erforderlich?** | **Beschreibung**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|-------------------------|----------------------------------|---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| version                 | 32-Bit-Ganzzahl ohne Vorzeichen (uimsbf) | Erforderlich      | Bestimmt das Format des Inhalts der mdat-Box. Nicht erkannte Versionen werden ignoriert. Aktuell wird nur der Wert 1 unterstützt.                                                                                                                                                                                                                                                                                                                                                      |
| id                      | 32-Bit-Ganzzahl ohne Vorzeichen (uimsbf) | Erforderlich      | Gibt die Instanz der Nachricht an. Nachrichten mit gleichbedeutender Semantik weisen den gleichen Wert auf; d.h., die Verarbeitung einer beliebigen von mehreren Ereignisnachrichtenboxen mit gleicher ID ist ausreichend.                                                                                                                                                                                                                                                                                                            |
| presentation_time_delta | 32-Bit-Ganzzahl ohne Vorzeichen (uimsbf) | Erforderlich      | Die Summe von „fragment_absolute_time“, die in „TrackFragmentExtendedHeaderBox“ angegeben ist, und das „presentation_time_delta“ **MUSS** die Präsentationszeit des Ereignisses bilden. Die Präsentationszeit und -dauer **SOLLTEN** an den Datenstrom-Zugriffspunkten (Stream Access Points, SAPs) vom Typ 1 oder 2 ausgerichtet sein, wie in [ISO-14496-12], Anhang I, beschrieben. Für HLS-Ausgang **SOLLTEN** Zeit und Dauer an den Segmentgrenzen ausgerichtet sein. Die Präsentationszeit und die Dauer verschiedener Ereignisnachrichten innerhalb des gleichen Ereignisdatenstroms **DÜRFEN sich NICHT** überschneiden. |
| message                 | Bytearray                       | Erforderlich      | Die Ereignisnachricht. Für [SCTE-35]-Nachrichten ist die Nachricht die binäre „splice_info_section()“. Für [SCTE-35]-Nachrichten **MUSS** dies „splice_info_section()“ sein, damit Nachrichten [SCTE-35]-konform an HLS-, Smooth- und Dash-Clients gesendet werden können. Für [SCTE-35]-Nachrichten ist die binäre „splice_info_section()“ die Nutzlast der „mdat“-Box, die **NICHT** base64-codiert ist.                                                            |

---


### <a name="224-cancellation-and-updates"></a>2.2.4 Stornierung und Updates

Nachrichten können durch das Senden mehrerer Nachrichten mit der gleichen Präsentationszeit und ID storniert oder aktualisiert werden.  Das Ereignis ist durch die Präsentationszeit und die ID eindeutig identifiziert. Die letzte für eine bestimmte Präsentationszeit empfangene Nachricht, die den Vorlaufbedingungen entspricht, ist die Nachricht, für die die Aktion ausgeführt wird. Die aktualisierte Nachricht ersetzt alle zuvor empfangenen Nachrichten.  Die Vorlaufbedingung beträgt vier Sekunden. Aktionen werden für Nachrichten ausgeführt, die mindestens vier Sekunden vor der Präsentationszeit empfangen wurden. 


## <a name="3-timed-metadata-delivery"></a>3 Zustellung von zeitgesteuerten Metadaten

Ereignisdatenstrom-Daten sind für Media Services nicht transparent. Media Services übergibt einfach drei Informationsstücke zwischen dem Erfassungsendpunkt und dem Clientendpunkt. Die folgenden Eigenschaften werden gemäß [SCTE-35] und/oder dem Streamingprotokoll des Clients an den Client zugestellt:

1.  Scheme: ein URN oder eine URL, der bzw. die das Schema der Nachricht angibt.
2.  Presentation Time: die Präsentationszeit des Ereignisses auf der Medienzeitachse.
3.  Duration: die Dauer des Ereignisses.
4.  ID: ein optionaler eindeutiger Bezeichner für das Ereignis.
5.  Message: die Ereignisdaten.

## <a name="31-microsoft-smooth-streaming-manifest"></a>3.1 Microsoft Smooth Streaming-Manifest  

Ausführliche Informationen zum Formatieren einer Sparsenachrichtenspur finden Sie unter „Behandlung von Sparsespuren [MS-SSTR]“. Für [SCTE35]-Nachrichten gibt Smooth Streaming die base64-codierten splice_info_section() in ein Sparsefragment aus.
Der StreamIndex **MUSS** den Untertyp „DATA“ aufweisen, und die CustomAttributes **MÜSSEN** ein Attribut mit Name="Schema" und Value="urn:scte:scte35:2013:bin" enthalten.

#### <a name="smooth-client-manifest-example-showing-base64-encoded-scte35-splice_info_section"></a>Beispiel für ein Smooth-Clientmanifest, das base64-codiertes [SCTE35] splice_info_section() zeigt
~~~ xml
<?xml version=”1.0” encoding=”utf-8”?>
<SmoothStreamingMedia MajorVersion=”2” MinorVersion=”0” TimeScale=”10000000” IsLive=”true” Duration=”0”
  LookAheadFragmentCount=”2” DVRWindowLength=”6000000000”>

  <StreamIndex Type=”video” Name=”video” Subtype=”” Chunks=”0” TimeScale=”10000000”
    Url=”QualityLevels({bitrate})/Fragments(video={start time})”>
    <QualityLevel Index=”0” Bitrate=”230000”
      CodecPrivateData=”250000010FC3460B50878A0B5821FF878780490800704704DC0000010E5A67F840” FourCC=”WVC1”
      MaxWidth=”364” MaxHeight=”272”/>
    <QualityLevel Index=”1” Bitrate=”305000”
      CodecPrivateData=”250000010FC3480B50878A0B5821FF87878049080894E4A7640000010E5A67F840” FourCC=”WVC1”
      MaxWidth=”364” MaxHeight=”272”/>
    <c t=”0” d=”20000000” r=”300” />
  </StreamIndex>
  <StreamIndex Type=”audio” Name=”audio” Subtype=”” Chunks=”0” TimeScale=”10000000”
    Url=”QualityLevels({bitrate})/Fragments(audio={start time})”>
    <QualityLevel Index=”0” Bitrate=”96000” CodecPrivateData=”1000030000000000000000000000E00042C0”
      FourCC=”WMAP” AudioTag=”354” Channels=”2” SamplingRate=”44100” BitsPerSample=”16” PacketSize=”4459”/>
    <c t=”0” d=”20000000” r=”300” />
  </StreamIndex>
  <StreamIndex Type=”text” Name=”scte35-sparse-stream” Subtype=”DATA” Chunks=”0” TimeScale=”10000000”
    ParentStreamIndex=”video” ManifestOutput=”true” 
    Url=”QualityLevels({bitrate})/Fragments(captions={start time})”>
    <QualityLevel Index=”0” Bitrate=”0” CodecPrivateData=”” FourCC=””>
      <CustomAttributes>
        <Attribute Name=”Scheme” Value=”urn:scte:scte35:2013:bin”/>
      </CustomAttributes>
    </QualityLevel>
    <!-- The following <c> and <f> fragments contains the base64-encoded [SCTE35] splice_info_section() message -->
    <c t=”600000000” d=”300000000”>    <f>PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz48QWNxdWlyZWRTaWduYWwgeG1sbnM9InVybjpjYWJsZWxhYnM6bWQ6eHNkOnNpZ25hbGluZzozLjAiIGFjcXVpc2l0aW9uUG9pbnRJZGVudGl0eT0iRVNQTl9FYXN0X0FjcXVpc2l0aW9uX1BvaW50XzEiIGFjcXVpc2l0aW9uU2lnbmFsSUQ9IjRBNkE5NEVFLTYyRkExMUUxQjFDQTg4MkY0ODI0MDE5QiIgYWNxdWlzaXRpb25UaW1lPSIyMDEyLTA5LTE4VDEwOjE0OjI2WiI+PFVUQ1BvaW50IHV0Y1BvaW50PSIyMDEyLTA5LTE4VDEwOjE0OjM0WiIvPjxTQ1RFMzVQb2ludERlc2NyaXB0b3Igc3BsaWNlQ29tbWFuZFR5cGU9IjUiPjxTcGxpY2VJbnNlcnQgc3BsaWNlRXZlbnRJRD0iMzQ0NTY4NjkxIiBvdXRPZk5ldHdvcmtJbmRpY2F0b3I9InRydWUiIHVuaXF1ZVByb2dyYW1JRD0iNTUzNTUiIGR1cmF0aW9uPSJQVDFNMFMiIGF2YWlsTnVtPSIxIiBhdmFpbHNFeHBlY3RlZD0iMTAiLz48L1NDVEUzNVBvaW50RGVzY3JpcHRvcj48U3RyZWFtVGltZXM+PFN0cmVhbVRpbWUgdGltZVR5cGU9IkhTUyIgdGltZVZhbHVlPSI1MTUwMDAwMDAwMDAiLz48L1N0cmVhbVRpbWVzPjwvQWNxdWlyZWRTaWduYWw+</f>
    </c>
    <c t=”1200000000” d=”400000000”>      <f>PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz48QWNxdWlyZWRTaWduYWwgeG1sbnM9InVybjpjYWJsZWxhYnM6bWQ6eHNkOnNpZ25hbGluZzozLjAiIGFjcXVpc2l0aW9uUG9pbnRJZGVudGl0eT0iRVNQTl9FYXN0X0FjcXVpc2l0aW9uX1BvaW50XzEiIGFjcXVpc2l0aW9uU2lnbmFsSUQ9IjRBNkE5NEVFLTYyRkExMUUxQjFDQTg4MkY0ODI0MDE5QiIgYWNxdWlzaXRpb25UaW1lPSIyMDEyLTA5LTE4VDEwOjE0OjI2WiI+PFVUQ1BvaW50IHV0Y1BvaW50PSIyMDEyLTA5LTE4VDEwOjE0OjM0WiIvPjxTQ1RFMzVQb2ludERlc2NyaXB0b3Igc3BsaWNlQ29tbWFuZFR5cGU9IjUiPjxTcGxpY2VJbnNlcnQgc3BsaWNlRXZlbnRJRD0iMzQ0NTY4NjkxIiBvdXRPZk5ldHdvcmtJbmRpY2F0b3I9InRydWUiIHVuaXF1ZVByb2dyYW1JRD0iNTUzNTUiIGR1cmF0aW9uPSJQVDFNMFMiIGF2YWlsTnVtPSIxIiBhdmFpbHNFeHBlY3RlZD0iMTAiLz48L1NDVEUzNVBvaW50RGVzY3JpcHRvcj48U3RyZWFtVGltZXM+PFN0cmVhbVRpbWUgdGltZVR5cGU9IkhTUyIgdGltZVZhbHVlPSI1MTYyMDAwMDAwMDAiLz48L1N0cmVhbVRpbWVzPjwvQWNxdWlyZWRTaWduYWw+</f>
    </c>
  </StreamIndex>
</SmoothStreamingMedia>
~~~

## <a name="32-apple-hls-manifest-decoration"></a>3.2 Apple HLS-Manifestdekoration

Azure Media Services unterstützt die folgenden HLS-Manifest-Tags, um Informationen zur Werbungsverfügbarkeit während eines Live- oder On-Demand-Ereignisses zu signalisieren. 

- EXT-X-DATERANGE, wie in Apple HLS [RFC8216] definiert
- EXT-X-CUE, wie in [Adobe-Primetime] definiert. Dieser Modus wird als „Legacy“ betrachtet. Kunden sollten nach Möglichkeit das EXT-X-DATERANGE-Tag übernehmen.

Die Datenausgabe an jedes einzelne Tag variiert je nach verwendetem Erfassungssignalmodus. Beispielsweise enthält die RTMP-Erfassung mit dem Adobe Simple-Modus nicht die vollständige, base64-codierte SCTE-35-Nutzlast.

## <a name="321-apple-hls-with-adobe-primetime-ext-x-daterange-recommended"></a>3.2.1 Apple HLS mit Adobe Primetime EXT-X-DATERANGE (empfohlen)

Die Apple HTTP Live Streaming-Spezifikation [RFC8216] ermöglicht das Signalisieren von [SCTE-35]-Nachrichten. Die Nachrichten werden in die Segmentwiedergabeliste in einem EXT-X-DATERANGE-Tag eingefügt, gemäß dem [RFC8216]-Abschnitt mit dem Titel „Mapping SCTE-35 into EXT-X-DATERANGE“ (Zuordnen von SCTE-35 zu EXT-X-DATERANGE).  Die Clientanwendungsebene kann die M3U-Wiedergabeliste analysieren und M3U-Tags verarbeiten oder die Ereignisse durch das Apple Player-Framework empfangen.  

Der **EMPFOHLENE** Ansatz in Azure Media Services (API der Version 3) besteht im Befolgen von [RFC8216] und der Verwendung des EXT-X-DATERANGE-Tags für [SCTE35]-Werbungsverfügbarkeitsdekoration im Manifest.

## <a name="322-apple-hls-with-adobe-primetime-ext-x-cue-legacy"></a>3.2.2 Apple HLS mit Adobe Primetime EXT-X-CUE (Legacy)

Es gibt auch eine „Legacy“-Implementierung, die in Azure Media Services (API der Version 2 und 3) bereitgestellt wird, die das EXT-X-CUE-Tag verwendet, wie in [Adobe-Primetime] „SCTE-35-Modus“ definiert. In diesem Modus bettet Azure Media Services base64-codierte [SCTE-35] splice_info_section() in das EXT-X-CUE-Tag ein.  

Das „Legacy“-Tag EXT-X-CUE ist wie folgt definiert und kann auch normativ in der [Adobe-Primetime]-Spezifikation referenziert werden. Dies sollte nur für die Legacy-SCTE35-Signalisierung bei Bedarf verwendet werden. Andernfalls ist das empfohlene Tag in [RFC8216] als EXT-X-DATERANGE definiert. 

| **Attributname** | **Typ**                      | **Erforderlich?**                             | **Beschreibung**                                                                                                                                                                                                                                                                      |
|--------------------|-------------------------------|-------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| CUE                | Zeichenfolge in Anführungszeichen                 | Erforderlich                                  | Die als base64-Zeichenfolge codierte Nachricht, wie in [RFC4648] beschrieben. Für [SCTE-35]-Nachrichten ist dies die base64-codierte „splice_info_section()“.                                                                                                |
| TYPE               | Zeichenfolge in Anführungszeichen                 | Erforderlich                                  | Ein URN oder eine URL, der bzw. die das Nachrichtenschema angibt. Für [SCTE-35]-Nachrichten nimmt der Typ den Sonderwert „scte35“ an.                                                                                                                                |
| id                 | Zeichenfolge in Anführungszeichen                 | Erforderlich                                  | Eindeutiger Bezeichner für das Ereignis. Wenn die ID bei der Erfassung der Nachricht nicht angegeben wird, erstellt Azure Media Services eine eindeutige ID.                                                                                                                                          |
| DURATION           | Dezimale Gleitkommazahl | Erforderlich                                  | Die Dauer des Ereignisses. Wenn sie unbekannt ist, **SOLLTE** der Wert 0 sein. Die Einheit sind Sekundenbruchteile.                                                                                                                                                                                           |
| ELAPSED            | Dezimale Gleitkommazahl | Optional, für gleitendes Fenster jedoch erforderlich | Wenn das Signal wiederholt wird, um ein gleitendes Präsentationsfenster zu unterstützen, **MUSS** dieses Feld gleich dem Betrag der seit dem Beginn des Ereignisses verstrichenen Präsentationszeit sein. Die Einheit sind Sekundenbruchteile. Dieser Wert darf die ursprünglich angegebene Dauer der Einfügung oder des Segments überschreiten. |
| TIME               | Dezimale Gleitkommazahl | Erforderlich                                  | Die Präsentationszeit des Ereignisses. Die Einheit sind Sekundenbruchteile.                                                                                                                                                                                                                    |


Die Anwendungsschicht des HLS-Players verwendet TYPE, um das Format der Nachricht zu bestimmen, die Nachricht zu decodieren, die erforderlichen Zeitkonvertierungen anzuwenden und das Ereignis zu verarbeiten.  Die Ereignisse sind in der Segmentwiedergabeliste der übergeordneten Spur entsprechend dem Ereigniszeitstempel zeitsynchronisiert.  Sie werden vor dem nächstgelegenen Segment (#EXTINF-Tag) eingefügt.

### <a name="323-hls-segment-playlist-example-using-legacy-adobe-primetime-ext-x-cue"></a>3.2.3 Beispiel für HLS-Segmentwiedergabelisten unter Verwendung von „Legacy“ Adobe Primetime EXT-X-CUE

Das folgende Beispiel zeigt die HLS-Manifestdekoration mithilfe des Adobe Primetime-Tags EXT-X-CUE.  Der „CUE“-Parameter enthält die vollständige, base64-codierte SCTE-35 splice_info-Nutzlast, die angibt, dass dieses Signal mithilfe von RTMP im Adobe SCTE-35-Signalmodus oder im Smooth Streaming SCTE-35-Signalmodus eingegangen ist. 

~~~
#EXTM3U
#EXT-X-VERSION:4
#EXT-X-ALLOW-CACHE:NO
#EXT-X-MEDIA-SEQUENCE:346
#EXT-X-TARGETDURATION:6
#EXT-X-I-FRAMES-ONLY
#EXT-X-PROGRAM-DATE-TIME:2018-12-13T15:54:19.462Z
#EXTINF:4.000000,no-desc
KeyFrames(video_track=15447164594627600,format=m3u8-aapl)
#EXTINF:6.000000,no-desc
KeyFrames(video_track=15447164634627600,format=m3u8-aapl)
#EXT-X-CUE:ID="1026",TYPE="scte35",DURATION=30.000000,TIME=1544716520.022760,CUE="/DAlAAAAAAAAAP/wFAUAAAQCf+//KRjAfP4AKTLgAAAAAAAAVYsh2w=="
#EXTINF:6.000000,no-desc
KeyFrames(video_track=15447165474627600,format=m3u8-aapl)
~~~

### <a name="324-hls-message-handling-for-legacy-adobe-primetime-ext-x-cue"></a>3.2.4 HLS-Nachrichtenbehandlung für „Legacy“ Adobe Primetime EXT-X-CUE

Ereignisse werden in der Segmentwiedergabeliste jeder Video- und Audiospur signalisiert. Die Position des EXT-X-CUE-Tags **MUSS** immer entweder unmittelbar vor dem ersten HLS-Segment (für splice_out oder Segmentstart) oder unmittelbar nach dem letzten HLS-Segment (für splice_in oder Segmentende) sein, auf das sich die TIME- und DURATION-Attribute beziehen, wie für [Adobe-Primetime] erforderlich.

Wenn ein gleitendes Präsentationsfenster aktiviert ist, **MUSS** das EXT-X-CUE-Tag ausreichend oft wiederholt werden, dass die Einfügung oder das Segment jederzeit vollständig in der Segmentwiedergabeliste beschrieben ist, und das ELAPSED-Attribut **MUSS** verwendet werden, um die Aktivitätsdauer der Einfügung oder des Segments anzugeben, wie für [Adobe-Primetime] erforderlich.

Wenn ein gleitendes Präsentationsfenster aktiviert ist, werden die EXT-X-CUE-Tags aus der Segmentwiedergabeliste entfernt, wenn sich die Medienzeit, auf die sie sich beziehen, aus dem gleitenden Präsentationsfenster herausbewegt hat.

## <a name="33-dash-manifest-decoration-mpd"></a>3.3 DASH-Manifestdekoration (MPD)

[MPEGDASH] stellt drei Möglichkeiten zur Signalisierung von Ereignissen bereit:

1.  Im MPD EventStream signalisierte Ereignisse
2.  In-Band-signalisierte Ereignisse unter Verwendung der Ereignisnachrichten-Box (‚emsg‘)
3.  Eine Kombination aus 1 und 2

Im MPD EventStream signalisierte Ereignisse sind für das VOD-Streaming nützlich, da Clients beim Download der MPD sofort Zugriff auf alle Ereignisse haben. Die Methode ist auch nützlich für die SSAI-Signalisierung, bei der der SSAI-Downstreamanbieter die Signale aus einem MPD-Manifest mit mehreren Zeiträumen analysieren und Werbungsinhalt dynamisch einfügen muss.  Die In-Band-Lösung („emsg“) eignet sich für Livestreaming, bei dem Clients das MPD nicht erneut herunterladen müssen, oder wo zwischen dem Client und dem Ursprung keine Manipulation von SSAI-Manifesten stattfindet. 

Das Standardverhalten von Azure Media Services für DASH besteht darin, sowohl im MPD-EventStream als auch In-Band mithilfe der Ereignisnachrichten-Box („emsg“) zu signalisieren.

Über [RTMP] oder [MS-SSTR-Ingest] erfasste Positionsnachrichten werden DASH-Ereignissen zugeordnet, wobei die In-Band-Boxen „emsg“ und/oder In-MPD-EventStreams verwendet werden. 

Die SCTE-35-In-Band-Signalisierung für DASH folgt der Definition und den Anforderungen, die in [SCTE-214-3] sowie im [DASH-IF-IOP]-Abschnitt 13.12.2 („SCTE35 Events“) definiert sind. 

Für die [SCTE-35]-In-Band-Übertragung verwendet die Ereignisnachrichten-Box („emsg“) die schemeId = „urn:scte:scte35:2013:bin“. Bei der MPD-Manifestdekoration verwendet die EventStream schemeId „urn:scte:scte35:2014:xml+bin“.  Dieses Format ist eine XML-Darstellung des Ereignisses, die eine binäre base64-codierte Ausgabe der vollständigen SCTE-35-Nachricht enthält, die bei der Erfassung eingetroffen ist. 

Normative Verweisdefinitionen der Übertragung von [SCTE-35]-Positionsnachrichten in DASH finden Sie in [SCTE-214-1], Abschnitt 6.7.4 (MPD), und [SCTE-214-3], Abschnitt 7.3.2 (Übertragung von SCTE 35-Positionsnachrichten).

### <a name="331-mpeg-dash-mpd-eventstream-signaling"></a>3.3.1 MPEG Dash (MPD) EventStream-Signalisierung

Manifestdekoration (MPD) von Ereignissen wird im MPD mithilfe des EventStream-Elements signalisiert, das innerhalb des Period-Elements vorkommt. Die verwendete schemeid ist „urn:scte:scte35:2014:xml+bin“.

> [!NOTE]
> Aus Gründen der Kürze ermöglicht [SCTE-35] die Verwendung des base64-codierten Abschnitts im „Signal.Binary“-Element (anstatt im „Signal.Signal.SpliceInfoSection“-Elements) als Alternative zur Übertragung einer vollständig analysierten Positionsnachricht.
> Azure Media Services verwendet diesen „xml+bin“-Ansatz zur Signalisierung im MPD-Manifest.
> Dies ist auch die empfohlene Methode, die im [DASH-IF-IOP] verwendet wird. Siehe im Abschnitt [„Werbeeinfügungs-Ereignisdatenströme“ der DASH IF IOP-Richtlinie](https://dashif-documents.azurewebsites.net/DASH-IF-IOP/master/DASH-IF-IOP.html#ads-insertion-event-streams).
> 

Das EventStream-Element weist die folgenden Attribute auf:

| **Attributname** | **Typ**                | **Erforderlich?** | **Beschreibung**                                                                                                                                                                                                                                                                                   |
|--------------------|-------------------------|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| scheme_id_uri      | Zeichenfolge                  | Erforderlich      | Gibt das Schema der Nachricht an. Das Schema ist auf den Wert des Scheme-Attributs in der Live Server Manifest-Box festgelegt. Der Wert **SOLLTE** ein URN oder eine URL sein, der/die das Nachrichtenschema identifiziert. Die unterstützte Ausgabe schemeId sollte „urn:scte:scte35:2014:xml+bin“ gemäß [SCTE-214-1], Abschnitt 6.7.4 (MPD), lauten, da der Dienst zurzeit aus Gründen der Kürze nur „xml+bin“ in MPD unterstützt.  |
| value              | Zeichenfolge                  | Optional      | Ein zusätzlicher Zeichenfolgenwert, der von den Besitzern des Schemas zum Anpassen der Semantik der Nachricht verwendet wird. Um mehrere Ereignisdatenströme mit demselben Schema unterscheiden zu können, **MUSS** der Wert auf den Namen des Ereignisdatenstroms festgelegt werden (‚trackName‘ für [MS-SSTR-Ingest]-Erfassung oder AMF-Nachrichtenname für RTMP-Erfassung). |
| Timescale          | 32-Bit-Ganzzahl ohne Vorzeichen | Erforderlich      | Die Zeitspanne in Sekundenbruchteilen.                                                                                                                                                                                                       |


### <a name="332-example-mpeg-dash-manifest-mpd-signaling-of-scte-35-using-eventstream"></a>3.3.2 Beispiel für MPEG DASH-Manifestsignalisierung (MPD) von SCTE-35 mithilfe von EventStream

~~~ xml
<!-- Example MPD using xml+bin style signaling per [SCTE-214-1] -->
  <EventStream schemeIdUri="urn:scte:scte35:2014:xml+bin" value="scte35_track_001_000" timescale="10000000">
        <Event presentationTime="15447165200227600" duration="300000000" id="1026">
            <scte35:Signal>
                <scte35:Binary>
                    /DAlAAAAAAAAAP/wFAUAAAQDf+//KaeGwP4AKTLgAAAAAAAAn75a3g==
                </scte35:Binary>
            </scte35:Signal>
        </Event>
        <Event presentationTime="15447166250227600" duration="300000000" id="1027">
           <scte35:Signal>
                <scte35:Binary>
                    /DAlAAAAAAAAAP/wFAUAAAQDf+//KaeGwP4AKTLgAAAAAAAAn75a3g==
                </scte35:Binary>
            </scte35:Signal>
        </Event>
    </EventStream>
~~~

> [!IMPORTANT]
> Beachten Sie, dass „presentationTime“ die Präsentationszeit des [SCTE-35]-Ereignisses ist, relativ zur Startzeit des Zeitraums übersetzt, nicht die Ankunftszeit der Nachricht.
> [MPEGDASH] definiert die Event@presentationTime als „Gibt die Präsentationszeit des Ereignisses relativ zum Start des Zeitraums an“.
> Der Wert der Präsentationszeit in Sekunden entspricht der Division des Werts dieses Attributs und dem Wert des Attributs EventStream@timescale.
> Wenn dieser Wert nicht vorhanden ist, ist der Wert der Präsentationszeit gleich 0.


### <a name="333-mpeg-dash-in-band-event-message-box-signaling"></a>3.3.3 MPEG DASH In-Band-Event Message Box-Signalisierung

Für einen In-Band-Ereignisdatenstrom muss das MPD ein InbandEventStream-Element auf der Adaptation Set-Ebene aufweisen.  Dieses Element weist ein obligatorisches schemeIdUri-Attribut und ein optionales timescale-Attribut auf, die ebenfalls in der Event Message Box (‚emsg‘) vorkommen.  Es **SOLLTEN** keine Ereignisnachrichten-Boxen mit Schemabezeichnern vorhanden sein, die nicht im MPD definiert sind.

Für die [SCTE-35]-In-Band-Übertragung **MÜSSEN** Signale die schemeId = „urn:scte:scte35:2013:bin“ verwenden.
Normative Definitionen der Übertragung von [SCTE-35]-In-Band-Nachrichten finden Sie in [SCTE-214-3], Abschnitt 7.3.2 (Übertragung von SCTE 35-Positionsnachrichten).

Die folgenden Details beschreiben die spezifischen Werte, die der Client in der „emsg“ in Übereinstimmung mit [SCTE-214-3] erwarten sollte:

| **Feldname**          | **Feldtyp**          | **Erforderlich?** | **Beschreibung**                                                                                                                                                                                                                                                                                                                                                    |
|-------------------------|-------------------------|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| scheme_id_uri           | Zeichenfolge                  | Erforderlich      | Gibt das Schema der Nachricht an. Das Schema ist auf den Wert des Scheme-Attributs in der Live Server Manifest-Box festgelegt. Der Wert **MUSS** ein URN sein, der das Nachrichtenschema angibt. Für [SCTE-35]-Nachrichten **MUSS** dies „urn:scte:scte35:2013:bin“ in Übereinstimmung mit [SCTE-214-3] sein. |
| Wert                   | Zeichenfolge                  | Erforderlich      | Ein zusätzlicher Zeichenfolgenwert, der von den Besitzern des Schemas zum Anpassen der Semantik der Nachricht verwendet wird. Um mehrere Ereignisdatenströme mit gleichem Schema zu unterscheiden, wird der Wert auf den Namen des Ereignisdatenstroms festgelegt (‚trackName‘ für Smooth-Erfassung oder AMF-Nachrichtenname für RTMP-Erfassung).                                                                  |
| Timescale               | 32-Bit-Ganzzahl ohne Vorzeichen | Erforderlich      | Die Zeitskala in Sekundenbruchteilen der Zeit- und Dauerfelder innerhalb der emsg-Box.                                                                                                                                                                                                                                                                        |
| Presentation_time_delta | 32-Bit-Ganzzahl ohne Vorzeichen | Erforderlich      | Das Delta der Medienpräsentationszeit aus der Präsentationszeit des Ereignisses und der frühesten Präsentationszeit in diesem Segment. Die Präsentationszeit und die Dauer **SOLLTEN** an den Datenstrom-Zugriffspunkten, (Stream Access Points, SAPs) von Typ 1 oder Typ 2 ausgerichtet sein, wie in [ISO-14496-12] Anhang I definiert.                                                                                            |
| event_duration          | 32-Bit-Ganzzahl ohne Vorzeichen | Erforderlich      | Die Dauer des Ereignisses, oder 0xFFFFFFFF, um eine unbekannte Dauer anzugeben.                                                                                                                                                                                                                                                                                          |
| Id                      | 32-Bit-Ganzzahl ohne Vorzeichen | Erforderlich      | Gibt die Instanz der Nachricht an. Nachrichten mit gleichbedeutender Semantik weisen den gleichen Wert auf. Wenn die ID bei der Erfassung der Nachricht nicht angegeben wird, erstellt Azure Media Services eine eindeutige ID.                                                                                                                                                    |
| Message_data            | Bytearray              | Erforderlich      | Die Ereignisnachricht. Für [SCTE-35]-Nachrichten sind die Nachrichtendaten die binäre „splice_info_section()“ in Übereinstimmung mit [SCTE-214-3]. |

### <a name="334-dash-message-handling"></a>3.3.4 DASH-Nachrichtenbehandlung

Ereignisse werden sowohl für Video- als auch für Audiospuren innerhalb der emsg-Box In-Band signalisiert.  Die Signalisierung erfolgt für alle Segmentanforderungen, deren „presentation_time_delta“ 15 Sekunden oder weniger beträgt. 

Wenn ein gleitendes Präsentationsfenster aktiviert ist, werden Ereignisnachrichten aus dem MPD entfernt, wenn die Summe von Zeit und Dauer der Ereignisnachricht kleiner als die Zeit der Mediendaten im Manifest ist.  Anders ausgedrückt: Die Ereignisnachrichten werden aus dem Manifest entfernt, wenn die Medienzeit, auf die sie sich beziehen, sich aus dem gleitenden Präsentationsfenster heraus bewegt hat.

## <a name="4-scte-35-ingest-implementation-guidance-for-encoder-vendors"></a>4. Implementierungsleitfaden für die SCTE-35-Erfassung für Encoder-Anbieter

Die folgenden Richtlinien zeigen häufige Probleme, die die Implementierung dieser Spezifikation durch den Encoder-Anbieter beeinträchtigen können.  Die nachstehenden Richtlinien wurden basierend auf dem echten Feedback von Partnern gesammelt, um anderen die Implementierung dieser Spezifikation zu erleichtern. 

[SCTE-35]-Nachrichten werden im binären Format gemäß dem Schema **„urn:scte:scte35:2013:bin“** bei [MS-SSTR-Ingest]-Erfassung und gemäß dem Typ **„scte35“** bei RTMP-Erfassung erfasst. Um die Konvertierung der [SCTE-35]-Zeitgebung, die auf MPEG-2-Transportdatenstrom-Präsentationszeitstempeln (PTS) basiert, zu erleichtern, wird in Form der Ereignispräsentationszeit (dem Feld „fragment_absolute_time“ bei Smooth-Erfassung und dem Feld „time“ bei RTMP-Erfassung) eine Zuordnung zwischen PTS (pts_time + pts_adjustment der splice_time()) und der Medienzeitachse bereitgestellt. Die Zuordnung ist erforderlich, da ein Rollover des 33-Bit PTS-Werts ungefähr alle 26,5 Stunden erfolgt.

Die Smooth Streaming-Erfassung [MS-SSTR-Ingest] erfordert, dass die Media Data Box (‚mdat‘) die in [SCTE-35] definierte **splice_info_section()** enthalten **MUSS**. 

Bei der RTMP-Erfassung wird das cue-Attribut der AMF-Nachricht auf die base64-codierte **splice_info_section()** festgelegt, die in [SCTE-35] definiert ist.  

Wenn die Nachrichten das oben beschriebene Format aufweisen, werden sie, wie weiter oben definiert, an HLS-, Smooth- und DASH-Clients gesendet.  

Wenn Sie Ihre Implementierung mit der Azure Media Services-Plattform testen, beginnen Sie das Testen zunächst mit einem „Pass-Through“-Liveereignis, bevor Sie zu Tests mit einem Codierungs-Liveereignis wechseln.

---

## <a name="change-history"></a>Änderungsverlauf

| Date     | Änderungen                                                                            |
|----------|------------------------------------------------------------------------------------|
| 07/2/19  | Überarbeitete RTMP-Erfassung für SCTE35-Unterstützung, RTMP „onCuePoint“ für Elemental Live wurde hinzugefügt | 
| 08/22/19 | Aktualisiert, um OnUserDataEvent zu RTMP für benutzerdefinierte Metadaten hinzuzufügen                         |

## <a name="next-steps"></a>Nächste Schritte
Sehen Sie sich die Media Services-Lernpfade an.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
