---
title: 'Tutorial: Artikelpersonalisierung – Custom Decision Service'
titlesuffix: Azure Cognitive Services
description: Ein Tutorial zur Artikelpersonalisierung für die kontextbezogene Entscheidungsfindung.
services: cognitive-services
author: slivkins
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-decision-service
ms.topic: tutorial
ms.date: 05/08/2018
ms.author: slivkins
ROBOTS: NOINDEX
ms.openlocfilehash: f7eafed9db25fba904d98ddea652671dc45aa01d
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2019
ms.locfileid: "68707215"
---
# <a name="tutorial-article-personalization-for-contextual-decision-making"></a>Tutorial: Artikelpersonalisierung für die kontextbezogene Entscheidungsfindung

Dieses Tutorial beschäftigt sich mit der Personalisierung der Artikelauswahl auf der Startseite einer Website. Custom Decision Service wirkt sich beispielsweise auf *mehrere* Listen von Artikeln auf der Startseite aus. Möglicherweise ist die Seite eine Newswebsite, die sich nur mit Politik und Sport beschäftigt. Sie würde drei Rangfolgelisten mit Artikeln anzeigen: Politik, Sport und Zuletzt gelesen.

## <a name="applications-and-action-sets"></a>Anwendungen und Aktionssätze

Erfahren Sie, wie Ihr Szenario in das Framework integriert wird. Stellen wir uns drei Anwendungen vor (eine für jede Liste, die optimiert wird): app-politics, app-sports und app-recent. Um die Kandidatenartikel für jede Anwendung anzugeben, sind zwei Aktionssätze vorhanden: einer für Politik und einer für Sport. Der Aktionssatz für app-recent wird automatisch als Vereinigungsmenge der anderen beiden Sätze generiert.

> [!TIP]
> Aktionssätze können von Anwendungen in Custom Decision Service gemeinsam verwendet werden.

## <a name="prepare-action-set-feeds"></a>Vorbereiten von Aktionssatzfeeds

Custom Decision Service nutzt Aktionssätze über RSS- oder Atom-Feeds, die vom Kunden bereitgestellt werden. Sie stellen zwei Feeds bereit: einen für Politik und einen für Sport. Angenommen, diese werden von `http://www.domain.com/feeds/<feed-name>` bereitgestellt.

Jeder Feed stellt eine Liste von Artikeln bereit. In RSS wird jede Liste durch ein `<item>`-Element wie folgt angegeben:

```xml
<rss version="2.0"><channel>
   <item>
      <title><![CDATA[article title]]></title>
      <link>"article url"</link>
      <pubDate>publication date</pubDate>
    </item>
</channel></rss>
```

Die Reihenfolge der Artikel ist von Bedeutung. Es wird die Standardrangfolge angegeben, also Ihre Vermutung, welche Reihenfolge die Artikel aufweisen sollten. Die Standardrangfolge wird dann für einen Leistungsvergleich auf dem Dashboard verwendet.

Weitere Informationen zum Feedformat finden Sie in der [API-Referenz](custom-decision-service-api-reference.md#action-set-api-customer-provided).

## <a name="register-a-new-app"></a>Registrieren einer neuen App

1. Melden Sie sich mit Ihrem [Microsoft-Konto](https://portal.ds.microsoft.com/) an. Klicken Sie im Menüband auf **Mein Portal**.

2. Klicken Sie zum Registrieren einer neuen Anwendung auf die Schaltfläche **Neue App**.

    ![Custom Decision Service-Portal](./media/custom-decision-service-tutorial/portal.png)

3. Geben Sie einen eindeutigen Namen für Ihre Anwendung im Textfeld **App-ID** ein. Wenn dieser Name bereits von einem anderen Kunden verwendet wird, fordert das System Sie auf, eine andere App-ID auszuwählen. Aktivieren Sie das Kontrollkästchen **Erweitert**, und geben Sie die [Verbindungszeichenfolge](../../storage/common/storage-configure-connection-string.md) für Ihr Azure-Speicherkonto ein. Normalerweise verwenden Sie dasselbe Speicherkonto für alle Ihre Anwendungen.

    ![Dialogfeld „Neue App“](./media/custom-decision-service-tutorial/new-app-dialog.png)

    Nachdem Sie alle drei Anwendungen im Szenario oben registriert haben, werden sie aufgelistet:

    ![Liste der Apps](./media/custom-decision-service-tutorial/apps.png)

    Sie können zu dieser Liste zurückkehren, indem Sie auf die Schaltfläche **Apps** klicken.

4. Geben Sie im Dialogfeld **Neue App** einen Aktionsfeed an. Aktionsfeeds können auch angegeben werden, indem Sie auf die Schaltfläche **Feeds** und dann auf die Schaltfläche **Neuer Feed** klicken. Geben Sie einen **Namen** für den neuen Feed ein, geben Sie die **URL** ein, über die er bereitgestellt wird, und geben Sie die **Aktualisierungszeit** ein. Die Aktualisierungszeit gibt an, wie häufig Custom Decision Service den Feed aktualisieren soll.

    ![Dialogfeld „Neuer Feed“](./media/custom-decision-service-tutorial/new-feed-dialog.png)

    Aktionsfeeds können von beliebigen Apps verwendet werden, und zwar unabhängig davon, wo diese angegeben werden. Nachdem Sie beide Aktionsfeeds in einem Szenario angegeben haben, werden sie aufgelistet:

    ![Liste der Feeds](./media/custom-decision-service-tutorial/feeds.png)

    Sie können zu dieser Liste zurückkehren, indem Sie auf die Schaltfläche **Feeds** klicken.

## <a name="use-the-apis"></a>Verwenden der APIs

Custom Decision Service versieht Artikel mithilfe der Rangfolge-API mit einer Rangfolge. Um diese API zu verwenden, fügen Sie den folgenden Code in den HTML-Header der Startseite ein:

```html
<!-- Define the "callback function" to render UI -->
<script> function callback(data) { … } </script>

<!--  call Ranking API after callback() is defined, separately for each app -->
<script src="https://ds.microsoft.com/api/v2/app-politics/rank/feed-politics" async></script>
<script src="https://ds.microsoft.com/api/v2/app-sports/rank/feed-sports" async></script>
<script src="https://ds.microsoft.com/api/v2/app-recent/rank/feed-politics/feed-sports" async></script>
<!-- NB: action feeds for 'app-recent' are listed one after another. -->
```

Die HTTP-Antwort der Rangfolge-API entspricht einer JSONP-formatierten Zeichenfolge. Für app-politics sieht die Zeichenfolge z.B. folgendermaßen aus:

```json
callback({
   "ranking":[{"id":"url1"}, {"id":"url2"}, {"id":"url3"}],
   "eventId":"<opaque event string>",
   "appId":"app-politics",
   "actionSets":[{"id":"feed-politics","lastRefresh":"date"}] });
```

Der Browser führt diese Zeichenfolge dann als Aufruf der `callback()`-Funktion aus. Das `data`-Argument in der `callback()`-Funktion enthält die App-ID und die Rangfolge der zu rendernden URLs. Insbesondere sollte `callback()` zur Unterscheidung zwischen den drei Anwendungen `data.appId` verwenden. `eventId` wird intern von Custom Decision Service verwendet, um die angegebene Rangfolge mit dem entsprechenden Klick (wenn vorhanden) abzugleichen.

> [!TIP]
> `callback()` kann jeden Aktionsfeed ggf. mit dem `lastRefresh`-Feld auf Aktualität überprüfen. Ist ein bestimmter Feed nicht ausreichend aktuell, ignoriert `callback()` möglicherweise die angegebene Rangfolge, ruft diesen Feed direkt auf und verwendet die Standardrangfolge, die durch den Feed bereitgestellt wird.

Weitere Informationen zu den Spezifikationen und weiteren Optionen, die von der Rangfolge-API bereitgestellt werden, finden Sie in der [API-Referenz](custom-decision-service-api-reference.md).

Die Auswahl der beliebtesten Artikel des Benutzers werden durch Aufrufen der Ergebnis-API zurückgegeben. Wenn eine Auswahl der beliebtesten Artikel des Benutzers empfangen wird, sollte der folgende Code auf der Startseite aufgerufen werden:

```javascript
$.ajax({
    type: "POST",
    url: '//ds.microsoft.com/api/v2/<appId>/reward/<eventId>',
    contentType: "application/json" })
```

Die Verwendung von `appId` und `eventId` im Code zur Klickverarbeitung muss mit Sorgfalt erfolgen. Sie können die `callback()`-Funktion z.B. wie folgt implementieren:

```javascript
function callback(data) {
    $.map(data.ranking, function (element) {
        //custom rendering function given content info
        render({appId:data.appId,
                article:element,
                onClick: element.id != data.rewardAction ? null :
                   function() {
                       $.ajax({
                       type: "POST",
                       url: '//ds.microsoft.com/api/v2/' + data.appId + '/reward/' + data.eventId,
                       contentType: "application/json" })}
                });
}}
```

Implementieren Sie in diesem Beispiel die `render()`-Funktion, um einen bestimmten Artikel für eine bestimmte Anwendung zu rendern. Diese Funktion verwendet als Eingabe die App-ID und den Artikel (im Format aus der Rangfolge-API). Der `onClick`-Parameter ist die Funktion, die aus `render()` aufgerufen werden sollte, um einen Klick zu verarbeiten. Sie überprüft, ob der Klick für den beliebtesten Artikel erfolgt. Dann wird die Ergebnis-API mit der entsprechenden App-ID und Ereignis-ID aufgerufen.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu den bereitgestellten Funktionen finden Sie in der [API-Referenz](custom-decision-service-api-reference.md).
