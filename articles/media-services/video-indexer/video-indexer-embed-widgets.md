---
title: Einbetten von Video Indexer-Widgets in Ihre Anwendungen
titlesuffix: Azure Media Services
description: Erfahren Sie, wie Sie Video Indexer-Widgets in Ihre Anwendung einbetten.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 07/29/2019
ms.author: juliako
ms.openlocfilehash: fc0b447630b5e1ac360b1d84869cea02186672fc
ms.sourcegitcommit: 6eecb9a71f8d69851bc962e2751971fccf29557f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "71036625"
---
# <a name="embed-video-indexer-widgets-in-your-applications"></a>Einbetten von Video Indexer-Widgets in Ihre Anwendungen

In diesem Artikel wird gezeigt, wie Sie Video Indexer-Widgets in Ihre Anwendungen einbetten können. Video Indexer unterstützt das Einbetten von drei Arten von Widgets in Ihre Anwendung: *Kognitive Erkenntnisse*, *Player* und *Editor*. 

Ab Version 2 enthält die Basis-URL des Widgets die Region des angegebenen Kontos. Ein Konto in der Region „USA, Westen“ generiert z.B.: `https://wus2.videoindexer.ai/embed/insights/...`.

## <a name="widget-types"></a>Typen von Widgets

### <a name="cognitive-insights-widget"></a>Widget „Kognitive Erkenntnisse“

Ein Widget vom Typ „Kognitive Erkenntnisse“ enthält alle visuellen Erkenntnisse, die für Sie aus dem Prozess der Videoindizierung extrahiert wurden. Für das Widget vom Typ „Kognitive Erkenntnisse“ werden die folgenden optionalen URL-Parameter unterstützt.

|NAME|Definition|BESCHREIBUNG|
|---|---|---|
|`widgets`|Durch Komma getrennte Zeichenfolgen|Ermöglicht das Steuern der Erkenntnisse, die Sie rendern möchten. <br/> Beispiel: Mit `https://www.videoindexer.ai/embed/insights/<accountId>/<videoId>/?widgets=people,search` werden nur Benutzeroberflächenerkenntnisse für Personen und Marken gerendert.<br/>Verfügbare Optionen: People (Personen), Keywords (Stichwörter), Annotations (Anmerkungen), Brands (Marken), Sentiments (Stimmungen), Transcript (Transkript), Search (Suche)<br/>Beachten Sie, dass der URL-Parameter `widgets` in Version 2 nicht unterstützt wird.<br/>|
|`locale`|Ein kurzer Sprachcode|Steuert die Sprache, in der Erkenntnisse angezeigt werden. Standardwert: `en`. <br/> Beispiel: `locale=de`.|
|`tab`|Die Registerkarte, die standardmäßig ausgewählt ist|Steuert die Registerkarte **Erkenntnisse**, die standardmäßig gerendert wird. <br/> Beispiel: `tab=timeline` rendert Erkenntnisse, während die Registerkarte **Zeitachse** ausgewählt ist.|

### <a name="player-widget"></a>Player-Widget

Sie können das Player-Widget zum Streamen von Videos verwenden, indem Sie eine adaptive Bitrate verwenden. Das Player-Widget unterstützt die folgenden optionalen URL-Parameter.

|NAME|Definition|BESCHREIBUNG|
|---|---|---|
|`t`|Sekunden ab Start|Aktiviert die Wiedergabe durch den Player ab dem angegebenen Zeitpunkt.<br/> Beispiel: `t=60`.|
|`captions`|Ein Sprachcode|Ruft die Beschriftungen beim Laden des Widgets in der angegebenen Sprache ab, damit sie im Menü für die **Beschriftungen** verfügbar sind.<br/> Beispiel: `captions=en-US`.|
|`showCaptions`|Ein boolescher Wert|Dient zum Laden des Players mit bereits geladenen Beschriftungen.<br/> Beispiel: `showCaptions=true`.|
|`type`||Aktiviert ein Design für den Audioplayer (der Videoteil wird entfernt).<br/> Beispiel: `type=audio`.|
|`autoplay`|Ein boolescher Wert|Gibt an, ob der Player beim Laden mit der Wiedergabe des Videos beginnen soll. Standardwert: `true`.<br/> Beispiel: `autoplay=false`.|
|`language`|Ein Sprachcode|Dient zum Steuern der Sprache des Players. Standardwert: `en-US`.<br/>Beispiel: `language=de-DE`.|

### <a name="editor-widget"></a>Editor-Widget

Mit dem Editor-Widget können Sie neue Projekte erstellen und Erkenntnisse aus Videos verwalten. Das Editor-Widget unterstützt die folgenden optionalen URL-Parameter.

|NAME|Definition|BESCHREIBUNG|
|---|---|---|
|`accessToken`<sup>*</sup>|Zeichenfolge|Bietet Zugriff auf Videos, die sich nur in dem Konto befinden, das zum Einbetten des Widgets verwendet wurde.<br> Das Editor-Widget erfordert den Parameter `accessToken`.|
|`language`|Ein Sprachcode|Dient zum Steuern der Sprache des Players. Standardwert: `en-US`.<br/>Beispiel: `language=de-DE`.|
|`locale`|Ein kurzer Sprachcode|Steuert die Sprache, in der Erkenntnisse angezeigt werden. Standardwert: `en`.<br/>Beispiel: `language=de`.|

<sup>*</sup>Der Besitzer sollte beim Bereitstellen von `accessToken` Vorsicht walten lassen.

## <a name="embedding-public-content"></a>Einbetten von öffentlichen Inhalten

1. Melden Sie sich bei der [Video Indexer](https://www.videoindexer.ai/)-Website an.
2. Wählen Sie das Video aus, mit dem Sie arbeiten möchten.
3. Wählen Sie die Schaltfläche **Einbetten** aus, die unter dem Video angezeigt wird.

    ![Widget](./media/video-indexer-embed-widgets/video-indexer-widget01.png)

    Nachdem Sie die Schaltfläche **Einbetten** ausgewählt haben, können Sie das Widget auswählen, das Sie in Ihre Anwendung einbetten möchten. 
4. Wählen Sie den gewünschten Widget-Typ aus (**Kognitive Erkenntnisse**, **Player** oder **Editor**).
 
5. Kopieren Sie den Einbettungscode, und fügen Sie ihn dann zu Ihrer Anwendung hinzu. 

    ![Widget](./media/video-indexer-embed-widgets/video-indexer-widget02.png)

> [!NOTE]
> Wenn beim Teilen Ihrer Video-URLs Probleme auftreten, fügen Sie dem Link den Parameter `location` hinzu. Der Parameter sollte auf die [Azure-Regionen festgelegt werden, in denen Video Indexer vorhanden ist](regions.md). Beispiel: `https://www.videoindexer.ai/accounts/00000000-0000-0000-0000-000000000000/videos/b2b2c74b8e/?location=trial`.

## <a name="embedding-private-content"></a>Einbetten von privaten Inhalten

Sie müssen ein Zugriffstoken im **src**-Attribut des „iframe“ übergeben, um ein privates Video einzubetten:

`https://www.videoindexer.ai/embed/[insights | player]/<accountId>/<videoId>/?accessToken=<accessToken>`
    
Verwenden Sie einen der folgenden Schritte, um die Inhalte des Widgets vom Typ „Kognitive Erkenntnisse“ zu erhalten:<br/>
- Die API des Widgets vom Typ [Erkenntnisse abrufen](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-Video-Insights-Widget?&pattern=widget).<br/>
- Das [Token zum Abrufen des Videozugriffs](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Video-Access-Token?). Fügen Sie es als Abfrageparameter zur URL hinzu. Geben Sie diese URL als **src**-Wert für den iframe an, wie zuvor gezeigt.

Um Funktionen zur Bearbeitung von Erkenntnissen in Ihrem eingebetteten Widget bereitzustellen, müssen Sie ein Zugriffstoken übergeben, das Berechtigungen zur Bearbeitung enthält. Verwenden Sie das Widget vom Typ [Erkenntnisse abrufen](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-Video-Insights-Widget?&pattern=widget) oder das [Token zum Abrufen des Videozugriffs](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Video-Access-Token?) mit `&allowEdit=true`. 

## <a name="widgets-interaction"></a>Interaktion von Widgets

Das Widget „Kognitive Erkenntnisse“ kann mit einem Video in Ihrer Anwendung interagieren. In diesem Abschnitt wird veranschaulicht, wie Sie diese Interaktion erreichen.

![Widget](./media/video-indexer-embed-widgets/video-indexer-widget03.png)

### <a name="cross-origin-communications"></a>Ursprungsübergreifende Kommunikation

Der Video Indexer-Dienst geht wie folgt vor, um für Video Indexer-Widgets die Kommunikation mit anderen Komponenten zu ermöglichen:

- Verwendung der HTML5-Methode **postMessage** für die ursprungsübergreifende Kommunikation. 
- Überprüfung der Nachricht für den VideoIndexer.ai-Ursprung 

Wenn Sie Ihren eigenen Playercode implementieren und die Integration mit Widgets vom Typ „Kognitive Erkenntnisse“ durchführen, sind Sie selbst dafür verantwortlich, den Ursprung der Nachricht von „VideoIndexer.ai“ zu überprüfen.

### <a name="embed-widgets-in-your-application-or-blog-recommended"></a>Einbetten von Widgets in Ihre Anwendung oder Ihren Blog (empfohlen) 

In diesem Abschnitt wird veranschaulicht, wie Sie die Interaktion zwischen zwei Video Indexer-Widgets so konfigurieren, dass der Player zum relevanten Moment springt, wenn ein Benutzer in Ihrer Anwendung das Steuerelement für Erkenntnisse auswählt.

1. Kopieren Sie den Einbettungscode des Player-Widgets.
2. Kopieren Sie den Einbettungscode von „Kognitive Erkenntnisse“.
3. Fügen Sie die [Mediator-Datei](https://breakdown.blob.core.windows.net/public/vb.widgets.mediator.js) für die Kommunikation zwischen den beiden Widgets hinzu:<br/> 
`<script src="https://breakdown.blob.core.windows.net/public/vb.widgets.mediator.js"></script>`

Wenn ein Benutzer in Ihrer Anwendung jetzt das Steuerelement für Erkenntnisse auswählt, springt der Player zum relevanten Moment.

Weitere Informationen finden Sie in der Demo [Video Indexer – Einbetten beider Widgets](https://codepen.io/videoindexer/pen/NzJeOb).

### <a name="embed-the-cognitive-insights-widget-and-use-azure-media-player-to-play-the-content"></a>Einbetten des Widgets „Kognitive Erkenntnisse“ und Verwenden von Azure Media Player zum Wiedergeben der Inhalte

In diesem Abschnitt wird veranschaulicht, wie Sie die Interaktion zwischen einem Widget vom Typ „Kognitive Erkenntnisse“ und einer Azure Media Player-Instanz per [AMP-Plug-In](https://breakdown.blob.core.windows.net/public/amp-vb.plugin.js) einrichten.
 
1. Fügen Sie ein Video Indexer-Plug-In für den AMP-Player hinzu:<br/> `<script src="https://breakdown.blob.core.windows.net/public/amp-vb.plugin.js"></script>`
2. Instanziieren Sie Azure Media Player mit dem Video Indexer-Plug-In.

        // Init the source.
        function initSource() {
            var tracks = [{
            kind: 'captions',
            // To load vtt from VI, replace it with your vtt URL.
            src: this.getSubtitlesUrl("c4c1ad4c9a", "English"),
            srclang: 'en',
            label: 'English'
            }];

            myPlayer.src([
            {
                "src": "//amssamples.streaming.mediaservices.windows.net/91492735-c523-432b-ba01-faba6c2206a2/AzureMediaServicesPromo.ism/manifest",
                "type": "application/vnd.ms-sstr+xml"
            }
            ], tracks);
        }

        // Init your AMP instance.
        var myPlayer = amp('vid1', { /* Options */
            "nativeControlsForTouch": false,
            autoplay: true,
            controls: true,
            width: "640",
            height: "400",
            poster: "",
            plugins: {
            videobreakedown: {}
            }
        }, function () {
            // Activate the plug-in.
            this.videobreakdown({
            videoId: "c4c1ad4c9a",
            syncTranscript: true,
            syncLanguage: true
            });

            // Set the source dynamically.
            initSource.call(this);
        });

3. Kopieren Sie den Einbettungscode von „Kognitive Erkenntnisse“.

Sie sollten jetzt mit Azure Media Player kommunizieren können.

Weitere Informationen finden Sie in der Demo zu [Azure Media Player und Video Indexer-Erkenntnisse](https://codepen.io/videoindexer/pen/rYONrO).

### <a name="embed-the-video-indexer-cognitive-insights-widget-and-use-a-different-video-player"></a>Einbetten des Video Indexer-Widgets vom Typ „Kognitive Erkenntnisse“ und Verwendung eines anderen Videoplayers

Wenn Sie einen anderen Videoplayer als Azure Media Player verwenden, müssen Sie den Videoplayer manuell bearbeiten, um die Kommunikation zu ermöglichen. 

1. Fügen Sie Ihren Videoplayer ein.

    Dies kann z. B. ein HTML5-Standardplayer sein:

        <video id="vid1" width="640" height="360" controls autoplay preload>
           <source src="//breakdown.blob.core.windows.net/public/Microsoft%20HoloLens-%20RoboRaid.mp4" type="video/mp4" /> 
           Your browser does not support the video tag.
        </video>    

2. Betten Sie das Widget „Kognitive Erkenntnisse“ ein.
3. Implementieren Sie die Kommunikation für Ihren Player, indem Sie auf das Ereignis „message“ lauschen. Beispiel:

        <script>
    
            (function(){
            // Reference your player instance.
            var playerInstance = document.getElementById('vid1');
        
            function jumpTo(evt) {
              var origin = evt.origin || evt.originalEvent.origin;
        
              // Validate that the event comes from the videobreakdown domain.
              if ((origin === "https://www.videobreakdown.com") && evt.data.time !== undefined){
                
                // Call your player's "jumpTo" implementation.
                playerInstance.currentTime = evt.data.time;
               
                // Confirm the arrival to us.
                if ('postMessage' in window) {
                  evt.source.postMessage({confirm: true, time: evt.data.time}, origin);
                }
              }
            }
        
            // Listen to the message event.
            window.addEventListener("message", jumpTo, false);
          
            }())    
        
        </script>

Weitere Informationen finden Sie in der Demo zu [Azure Media Player und Video Indexer-Erkenntnisse](https://codepen.io/videoindexer/pen/YEyPLd).

## <a name="adding-subtitles"></a>Hinzufügen von Untertiteln

Wenn Sie Video Indexer-Erkenntnisse in Ihren eigenen [Azure Media Player](https://aka.ms/azuremediaplayer) einbetten, können Sie die **GetVttUrl**-Methode verwenden, um Untertitel zu erhalten. Sie können auch eine JavaScript-Methode aus dem Video Indexer-AMP-Plug-In **getSubtitlesUrl** abrufen (wie bereits gezeigt). 

## <a name="customizing-embeddable-widgets"></a>Anpassen von für die Einbettung geeigneten Widgets

### <a name="cognitive-insights-widget"></a>Widget „Kognitive Erkenntnisse“

Sie können die gewünschte Art von Erkenntnissen auswählen. Geben Sie sie dazu als Wert für den folgenden URL-Parameter an, der dem abgerufenen Einbettungscode hinzugefügt wird (aus der API oder der Webanwendung): `&widgets=<list of wanted widgets>`.

Mögliche Werte: **people** (Personen), **keywords** (Schlüsselwörter), **sentiments** (Stimmungen), **transcript** (Transkript), **search** (Suche).

Wenn Sie z. B. ein Widget einbetten möchten, das nur Erkenntnisse zu Personen und zur Suche enthalten soll, sieht die iframe-URL für die Einbettung wie folgt aus:

`https://www.videoindexer.ai/embed/insights/<accountId>/<videoId>/?widgets=people,search`

Der Titel des iframe-Fensters kann auch angepasst werden, indem für die iframe-URL `&title=<YourTitle>` angegeben wird. (Dient zum Anpassen des HTML-Werts \<title>.)
    
Falls Sie Ihrem iframe-Fenster beispielsweise den Titel „MyInsights“ geben möchten, sieht die URL wie folgt aus:

`https://www.videoindexer.ai/embed/insights/<accountId>/<videoId>/?title=MyInsights`

Beachten Sie, dass diese Option nur in Fällen relevant ist, in denen Sie die Erkenntnisse in einem neuen Fenster öffnen müssen.

### <a name="player-widget"></a>Player-Widget

Wenn Sie den Video Indexer-Player einbetten, können Sie die Größe des Players wählen, indem Sie die iframe-Größe angeben.

Beispiel:

`<iframe width="640" height="360" src="https://www.videoindexer.ai/embed/player/<accountId>/<videoId>/" frameborder="0" allowfullscreen />`

Standardmäßig verfügt der Video Indexer-Player über automatisch generierte Untertitel, die auf dem Transkript des Videos basieren. Das Transkript wird aus dem Video mit der Quellsprache extrahiert, die beim Hochladen des Videos ausgewählt wurde.

Wenn das Einbetten mit einer anderen Sprache erfolgen soll, können Sie `&captions=< Language | ”all” | “false” >` zur Player-URL hinzufügen. Wenn Sie Untertitel in allen verfügbaren Sprachen wünschen, verwenden Sie den Wert `all`. Falls die Untertitel standardmäßig angezeigt werden sollen, können Sie `&showCaptions=true` übergeben.

Die URL für die Einbettung sieht dann wie folgt aus: 

`https://www.videoindexer.ai/embed/player/<accountId>/<videoId>/?captions=italian`

Wenn Sie Untertitel deaktivieren möchten, können Sie den Parameterwert `captions` als `false` übergeben.

#### <a name="autoplay"></a>Automatische Wiedergabe
Standardmäßig beginnt der Player mit der Wiedergabe des Videos. Sie können dies verhindern, indem Sie `&autoplay=false` an die vorhergehende URL für die Einbettung übergeben.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Anzeigen und Bearbeiten von Video Indexer-Erkenntnissen finden Sie in unter [Anzeigen und Bearbeiten von Video Indexer-Erkenntnissen](video-indexer-view-edit.md).

Sehen Sie sich außerdem auch [Video Indexer CodePen](https://codepen.io/videoindexer/pen/eGxebZ) an.
