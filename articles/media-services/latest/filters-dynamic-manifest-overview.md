---
title: Filter und dynamische Azure Media Services-Manifeste | Microsoft-Dokumentation
description: In diesem Thema wird erläutert, wie Sie Filter erstellen, mit denen Ihre Kunden bestimmte Abschnitte eines Streams streamen können. Media Services erstellt dynamische Manifeste, um dieses selektive Streaming zu erreichen.
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
ms.date: 07/11/2019
ms.author: juliako
ms.openlocfilehash: dc9f59894da071e956283591cf7206bc371650b7
ms.sourcegitcommit: 007ee4ac1c64810632754d9db2277663a138f9c4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/23/2019
ms.locfileid: "69991430"
---
# <a name="pre-filtering-manifests-by-using-dynamic-packager"></a>Vorfiltern von Manifesten mithilfe des dynamischen Packagers

Bei der Bereitstellung von Inhalten mit Adaptive Bitrate Streaming auf Geräten müssen Sie häufig mehrere Versionen eines Manifests veröffentlichen, um bestimmte Gerätefunktionen oder eine verfügbare Netzwerkbandbreite als Ziel verwenden zu können. Der [dynamische Packager](dynamic-packaging-overview.md) gestattet es Ihnen, Filter anzugeben, die bestimmte Codecs, Auflösungen, Bitraten und Audiospurkombinationen unmittelbar ausfiltern können und so die Notwendigkeit beseitigen, mehrere Kopien erstellen zu müssen. Sie müssen lediglich eine neue URL mit einem bestimmten Satz von Filtern veröffentlichen, die für Ihre Zielgeräte (iOS, Android, SmartTV oder Browser) und die Netzwerkgegebenheiten (Szenarien mit hoher Bandbreite, mobiler Datenübertragung oder niedriger Bandbreite) konfiguriert sind. In diesem Fall können Clients das Streamen Ihrer Inhalte durch die Abfragezeichenfolge manipulieren (durch Angeben verfügbarer [Medienobjekt- oder Kontofilter](filters-concept.md)) und Filter verwenden, um bestimmte Abschnitte eines Datenstroms zu filtern.

Einige Lieferszenarien erfordern, dass Sie sicherstellen, dass ein auf bestimmte Spuren nicht zugreifen kann. Beispielsweise, wenn Sie ein Manifest, das HD-Titel enthält, nicht in einem bestimmten Abonnententarif veröffentlichen möchten. Oder wenn Sie bestimmte Titel mit Adaptive Bitrate (ABR) entfernen möchten, um die Bereitstellungskosten auf einem bestimmten Gerät zu reduzieren, das von den zusätzlichen Spuren nicht profitieren würde. In diesem Fall könnten Sie beim Erstellen einem [Streaminglocator](streaming-locators-concept.md) eine Liste zuvor erstellter Filter zuordnen. In diesem Fall können Clients nicht manipulieren, wie der Inhalt gestreamt wird, weil dies im **Streaminglocator** definiert ist.

Sie können die Filterung über bestimmte [Filter im Streaminglocator](filters-concept.md#associating-filters-with-streaming-locator) mit zusätzlichen, gerätespezifischen Filtern kombinieren, die Ihr Client in der URL angibt. Dies kann hilfreich sein, um zusätzliche Spuren einzuschränken – beispielsweise Metadaten- oder Ereignisdatenströme, Audiosprachen oder beschreibende Audiospuren. 

Diese Fähigkeit, verschiedene Filter in Ihrem Datenstrom angeben zu können, bietet eine leistungsstarke Lösung zur Manipulation **dynamischer Manifeste** für gezielte mehrfache Anwendungsfallszenarien auf Ihren Zielgeräten. In diesem Thema werden Konzepte im Zusammenhang mit **dynamischen Manifesten** erläutert. Außerdem sind Beispiele für Szenarien enthalten, in denen Sie dieses Feature möglicherweise verwenden möchten.

> [!NOTE]
> Dynamische Manifeste nehmen keine Änderungen am Medienobjekt und dem Standardmanifest für dieses Medienobjekt vor. 
> 

##  <a name="overview-of-manifests"></a>Übersicht über Manifeste

Azure Media Services unterstützt die Protokolle HLS, MPEG DASH und Smooth Streaming. Als Teil der [dynamischen Paketerstellung](dynamic-packaging-overview.md) werden die Streamingclientmanifeste (HLS Master Playlist, DASH Media Presentation Description (MPD) und Smooth Streaming) basierend auf dem Formatselektor in der URL dynamisch generiert. Weitere Informationen finden Sie unter [Übermittlungsprotokolle](dynamic-packaging-overview.md#delivery-protocols). 

### <a name="get-and-examine-manifest-files"></a>Abrufen und Untersuchen von Manifestdateien

Sie geben eine Liste von Eigenschaftsbedingungen für die Filterung nach Spuren an, auf deren Grundlage Spuren Ihres Streams (Live oder Video on Demand (VOD)) in ein dynamisch erstelltes Manifest aufgenommen werden sollen. Um die Eigenschaften der Spuren abzurufen und zu untersuchen, müssen Sie zunächst das Smooth Streaming-Manifest laden.

Im Tutorial [Hochladen, Codieren und Streamen von Dateien mit .NET](stream-files-tutorial-with-api.md#get-streaming-urls) erfahren Sie, wie Sie die Streaming-URLs mit .NET erstellen. Wenn Sie die App ausführen, zeigt einer der URLs auf das Smooth Streaming-Manifest: `https://amsaccount-usw22.streaming.media.azure.net/00000000-0000-0000-0000-0000000000000/ignite.ism/manifest`.<br/> Kopieren Sie die URL, und fügen Sie sie in die Adressleiste eines Browsers ein. Die Datei wird heruntergeladen. Sie können die Datei in einem Text-Editor Ihrer Wahl öffnen.

Ein REST-Beispiel finden Sie unter [Hochladen, Codieren und Streamen von Dateien mit REST](stream-files-tutorial-with-rest.md#list-paths-and-build-streaming-urls).

### <a name="monitor-the-bitrate-of-a-video-stream"></a>Überwachen der Bitrate eines Videostreams

Sie können mithilfe der [Azure Media Player-Demoseite](https://aka.ms/azuremediaplayer) die Bitrate eines Videostreams überwachen. Die Demoseite zeigt Diagnoseinformationen auf der Registerkarte **Diagnose** an:

![Azure Media Player-Diagnose][amp_diagnostics]
 
### <a name="examples-urls-with-filters-in-query-string"></a>Beispiele: URLs mit Filtern in der Abfragezeichenfolge

Sie können Filter auf ABR-Streamingprotokolle anwenden: HLS, MPEG-DASH und Smooth Streaming. Die folgende Tabelle zeigt einige Beispiele für URLs mit Filtern:

|Protocol|Beispiel|
|---|---|
|HLS|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(format=m3u8-aapl,filter=myAccountFilter)`|
|MPEG DASH|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(format=mpd-time-csf,filter=myAssetFilter)`|
|Smooth Streaming|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(filter=myAssetFilter)`|

## <a name="rendition-filtering"></a>Filtern der Wiedergabe

Sie können ein Medienobjekt in mehreren Codierungsprofilen (H.264 Baseline, H.264 High, AACL, AACH, Dolby Digital Plus) und mit Bitraten für unterschiedliche Qualität codieren. Die Profile und Bitraten des Medienobjekts werden jedoch nicht auf allen Clientgeräten unterstützt. Auf älteren Android-Geräten wird beispielsweise nur H.264 Baseline+AACL unterstützt. Wenn höhere Bitraten an ein Gerät gesendet werden, das daraus kein Nutzen ziehen kann, werden Bandbreite und Rechenressourcen des Geräts vergeudet. Ein solches Gerät muss alle bereitgestellten Informationen decodieren, um sie dann für die Anzeige herunterzuskalieren.

Mit dynamischen Manifesten können Sie beispielsweise Geräteprofile für Mobiltelefone, Konsolen oder HD/SD erstellen und die für das jeweilige Profil gewünschten Spuren und Qualitätsversionen einschließen. Dies wird als „Wiedergabefilterung“ bezeichnet. Das folgende Diagramm zeigt ein entsprechendes Beispiel:

![Beispiel für die Wiedergabefilterung][renditions2]

Im folgenden Beispiel wurde ein Encoder zum Codieren eines Zwischenassets in sieben ISO-MP4s-Videowiedergaben (von 180p bis 1080p) verwendet. Das codierte Medienobjekt kann [dynamisch](dynamic-packaging-overview.md) in eines der folgenden Streamingprotokolle gepackt werden: HLS, MPEG-DASH und Smooth. 

Der obere Bereich des folgenden Diagramms zeigt das HLS-Manifest für die Medienobjekte ohne Filter. (Es enthält alle sieben Wiedergabeversionen.)  Links unten zeigt das Diagramm ein HLS-Manifest, auf das ein Filter namens „ott“ angewendet wurde. Der Filter „ott“ gibt an, dass alle Bitraten unter 1 MBit/s entfernt werden sollen. Daher wurden in der Antwort die beiden untersten Qualitätsstufen entfernt. Rechts unten zeigt das Diagramm das HLS-Manifest, auf das ein Filter namens „mobile“ angewendet wurde. Der Filter „mobile“ gibt an, dass Wiedergaben mit einer Auflösung von mehr als 720p entfernt werden sollen. Daher wurden die beiden Wiedergaben mit 1080p entfernt.

![Filtern der Wiedergabe][renditions1]

## <a name="removing-language-tracks"></a>Entfernen von Sprachspuren
Ihre Medienobjekte können mehrere Audiosprachen enthalten, z. B. Englisch, Spanisch, Französisch usw. In der Regel verwaltet das Player-SDK die Auswahl der Standardaudiospur und verfügbare Audiospuren entsprechend der Benutzerauswahl.

Die Entwicklung solcher Player-SDKs ist nicht ganz einfach, da hierbei verschiedene Implementierungen in verschiedenen gerätespezifischen Playerframeworks benötigt werden. Außerdem sind die Player-APIs auf manchen Plattformen eingeschränkt und umfassen keine Funktion zur Audioauswahl, sodass Benutzer die Standardaudiospur nicht auswählen oder ändern können. Mit Filtern für Medienobjekte können Sie das Verhalten steuern, indem Sie Filter erstellen, die nur die gewünschten Audiosprachen enthalten.

![Filtern von Sprachspuren][language_filter]

## <a name="trimming-the-start-of-an-asset"></a>Kürzen des Starts eines Medienobjekts

Bei den meisten Live-Streaming-Ereignissen führen Operatoren vor dem eigentlichen Ereignis einige Tests durch. Sie fügen z.B. die folgende Meldung vor dem Start des Ereignisses ein: „Programm beginnt in Kürze.“ 

Wenn das Programm archiviert wird, werden auch die Test- und Meldungsdaten archiviert und in die Präsentation einbezogen. Diese Informationen sollten jedoch nicht auf den Clients angezeigt werden. Mit dynamischen Manifesten können Sie einen Filter für die Startzeit erstellen und die unerwünschten Daten aus dem Manifest entfernen.

![Kürzen des Starts][trim_filter]

## <a name="creating-subclips-views-from-a-live-archive"></a>Erstellen von Subclips (Ansichten) aus einem Livearchiv

Viele Liveereignisse werden lange ausgeführt. Daher können Livearchive mehrere Ereignisse enthalten. Nach dem Ende des Liveereignisses möchten Sendeanstalten das Livearchiv möglicherweise in logische Sequenzen mit Programmstart und Programmende unterteilen. 

Diese virtuellen Programme können separat ohne Nachbearbeitung des Livearchivs und ohne Erstellung separater Medienobjekte veröffentlicht werden (wobei kein Nutzen aus den vorhandenen, in den CDNs zwischengespeicherten Fragmenten gezogen wird). Beispiele für solche virtuellen Programme sind Halbzeiten eines Fußball- oder Basketballspiels, die Innings beim Baseball oder einzelne Wettbewerbe beliebiger Sportprogramme.

Mit dynamischen Manifesten können Sie Filter mit Start- und Endzeiten sowie virtuelle Ansichten für Ihr Livearchiv erstellen. 

![Filter für Subclip][subclip_filter]

Hier sehen Sie das gefilterte Medienobjekt:

![Skilaufen][skiing]

## <a name="adjusting-the-presentation-window-dvr"></a>Anpassen des Präsentationsfensters (DVR)

Zum gegenwärtigen Zeitpunkt umfasst Azure Media Services Umlaufarchive, bei denen eine Dauer zwischen 1 Minute und 25 Stunden konfiguriert werden kann. Durch Filter über Manifeste kann ein DVR-Gleitfenster über dem Archiv erstellt werden, ohne dass Medien gelöscht werden. Es gibt viele Szenarios, bei denen Sendeanstalten ein in der Größe begrenztes DVR-Fenster bereitstellen möchten, das sich am Rand des Livevideos bewegt, und gleichzeitig ein größeres Archivierungsfenster beibehalten möchten. Ein bestimmter Sender möchte eventuell die Daten verwenden, die sich außerhalb des DVR-Fensters befinden, um Clips hervorzuheben oder unterschiedliche DVR-Fenster für verschiedene Geräte bereitstellen. Auf den meisten Mobilgeräten werden beispielsweise keine großen DVR-Fenster verarbeitet. (Sie können ein 2-Minuten-Fenster für Mobilgeräte und ein 1-Stunden-Fenster für Desktopclients einrichten.)

![DVR-Fenster][dvr_filter]

## <a name="adjusting-livebackoff-live-position"></a>Anpassen von LiveBackoff (Liveposition)

Mit der Filterung über Manifeste können mehrere Sekunden vom Rand eines Liveprogramms entfernt werden. Die Filterung ermöglicht es Sendeanstalten, die Präsentation am Vorschauveröffentlichungspunkt anzusehen und Einfügepunkte für Werbespots zu erstellen, bevor die Zuschauer den Stream empfangen (um 30 Sekunden zurückgehalten). Sendeanstalten können diese Werbespots dann an ihre Clientframeworks senden, sodass diese sie rechtzeitig empfangen und die Informationen vor der Ausstrahlung verarbeiten können.

Neben der Unterstützung für Werbespots kann mithilfe der LiveBackoff-Einstellung die Betrachterposition angepasst werden, sodass Clients weiterhin Fragmente vom Server empfangen, wenn sie den Rand des Livevideos erreichen. Dadurch tritt für Clients kein HTTP-Fehler vom Typ 404 oder 412 auf.

![Filtern nach LiveBackoff][livebackoff_filter]

## <a name="combining-multiple-rules-in-a-single-filter"></a>Kombinieren mehrerer Regeln in einem Filter

Sie können mehrere Filterregeln in ein und demselben Filter kombinieren. Als Beispiel können Sie eine „Bereichsregel“ zum Entfernen von Meldungen aus einem Livearchiv definieren und zudem verfügbare Bitraten filtern. Wenn Sie mehrere Filterregeln anwenden, ist das Endergebnis die Schnittmenge aller Regeln.

![Mehrere Filterregeln][multiple-rules]

## <a name="combining-multiple-filters-filter-composition"></a>Kombinieren mehrerer Filter (Filterkomposition)

Sie können auch mehrere Filter in einer einzelnen URL kombinieren. Das folgende Szenario zeigt, warum es sinnvoll sein kann, Filter zu kombinieren:

1. Sie müssen Ihre Videoqualitäten für mobile Geräte wie Android oder iPad filtern (um die Videoqualität einzuschränken). Zum Entfernen der unerwünschten Qualitätsstufen muss ein für die Geräteprofile geeigneter Kontofilter erstellt werden. Kontofilter können ohne weitere Zuordnung für alle Medienobjekte des gleichen Media Services-Kontos verwendet werden.
1. Sie möchten außerdem die Start- und Endzeit Zeit eines Medienobjekts kürzen. Zu diesem Zweck erstellen Sie einen Medienobjektfilter und legen die Start-/Endzeit fest. 
1. Sie möchten beide Filter kombinieren. Ohne Kombinierung müssen Sie den Qualitätsfilter dem Kürzungsfilter hinzufügen, was die Verwendung des Filters erschwert.


Um Filter zu kombinieren, müssen Sie die Filternamen auf die Manifest-/Wiedergabelisten-URL festlegen (in einem durch Semikolons getrennten Format). Angenommen, Sie verfügen über einen Filter mit dem Namen *MyMobileDevice* zum Filtern der Qualitäten und über einen weiteren Filter mit dem Namen *MyStartTime* zum Festlegen einer bestimmten Startzeit. Sie können bis zu drei Filter kombinieren. 

Weitere Informationen finden Sie in [diesem Blogbeitrag](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/).

## <a name="considerations-and-limitations"></a>Überlegungen und Einschränkungen

- Die Werte für **forceEndTimestamp**, **presentationWindowDuration** und **liveBackoffDuration** dürfen für einen VoD-Filter nicht festgelegt werden. Sie werden nur in Szenarien mit Livefiltern verwendet. 
-  Für dynamische Manifeste gelten GOP-Grenzen (Keyframes), sodass Kürzungen mit GOP-Genauigkeit erfolgen.
-  Sie können die gleichen Filternamen für Konto- und Medienobjektfilter verwenden. Medienobjektfilter haben Vorrang und überschreiben Kontofilter.
- Wenn Sie einen Filter aktualisieren, kann es bis zu zwei Minuten dauern, bis die Regeln am Streamingendpunkt aktualisiert wurden. Falls Sie Filter verwendet haben, um den Inhalt bereitzustellen, und der Inhalt in Proxys und CDN-Caches zwischengespeichert wurde, können infolge der Aktualisierung dieser Filter Playerfehler auftreten. Es empfiehlt sich, den Cache nach der Filteraktualisierung zu leeren. Sollte das nicht möglich sein, empfiehlt sich die Verwendung eines anderen Filters.
- Kunden müssen das Manifest manuell herunterladen und den exakten Wert von „startTimestamp“ und Zeitskala analysieren.
    
    - Um die Eigenschaften der Spuren in einem Medienobjekt zu bestimmen, müssen Sie die [Manifestdatei abrufen und untersuchen](#get-and-examine-manifest-files).
    - Die Formel zum Festlegen der Zeitstempeleigenschaften für einen Medienobjektfilter sieht wie folgt aus: <br/>startTimestamp = &lt;Startzeit im Manifest&gt; +  &lt;erwartete Filterstartzeit in Sekunden&gt; × Zeitskala

## <a name="next-steps"></a>Nächste Schritte

Die folgenden Artikel enthalten Informationen zum programmgesteuerten Erstellen von Filtern:  

- [Erstellen von Filtern mit REST-APIs](filters-dynamic-manifest-rest-howto.md)
- [Erstellen von Filtern mit .NET](filters-dynamic-manifest-dotnet-howto.md)
- [Erstellen von Filtern mit der CLI](filters-dynamic-manifest-cli-howto.md)

[renditions1]: ./media/filters-dynamic-manifest-overview/media-services-rendition-filter.png
[renditions2]: ./media/filters-dynamic-manifest-overview/media-services-rendition-filter2.png

[rendered_subclip]: ./media/filters-dynamic-manifests/media-services-rendered-subclip.png
[timeline_trim_event]: ./media/filters-dynamic-manifests/media-services-timeline-trim-event.png
[timeline_trim_subclip]: ./media/filters-dynamic-manifests/media-services-timeline-trim-subclip.png

[multiple-rules]:./media/filters-dynamic-manifest-overview/media-services-multiple-rules-filters.png

[subclip_filter]: ./media/filters-dynamic-manifest-overview/media-services-subclips-filter.png
[trim_event]: ./media/filters-dynamic-manifests/media-services-timeline-trim-event.png
[trim_subclip]: ./media/filters-dynamic-manifests/media-services-timeline-trim-subclip.png
[trim_filter]: ./media/filters-dynamic-manifest-overview/media-services-trim-filter.png
[redered_subclip]: ./media/filters-dynamic-manifests/media-services-rendered-subclip.png
[livebackoff_filter]: ./media/filters-dynamic-manifest-overview/media-services-livebackoff-filter.png
[language_filter]: ./media/filters-dynamic-manifest-overview/media-services-language-filter.png
[dvr_filter]: ./media/filters-dynamic-manifest-overview/media-services-dvr-filter.png
[skiing]: ./media/filters-dynamic-manifest-overview/media-services-skiing.png
[amp_diagnostics]: ./media/filters-dynamic-manifest-overview/amp_diagnostics.png
