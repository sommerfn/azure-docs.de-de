---
title: Azure Front Door Service – URL-Umleitung| Microsoft-Dokumentation
description: In diesem Artikel erläutert, wie Azure Front Door Service die URL-Umleitung für Routes unterstützt, wenn diese konfiguriert wurde.
services: front-door
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/08/2019
ms.author: sharadag
ms.openlocfilehash: 3d77a16d24a1a843b39d97904a675518c43a525a
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/22/2019
ms.locfileid: "67332410"
---
# <a name="url-redirect"></a>URL Redirect
Mithilfe von Azure Front Door Service können Sie Datenverkehr umleiten. Der Datenverkehr lässt sich auf mehreren Ebenen (Protokoll, Hostname, Pfad, Abfragezeichenfolge) umleiten, und sämtliche Funktionen können für einzelne Microservices konfiguriert werden, da die Umleitung pfadbasiert ist. Dies vereinfacht die Anwendungskonfiguration, optimiert die Ressourcennutzung und ermöglicht neue Umleitungsszenarien (einschließlich globale und pfadbasierte Umleitung).
</br>

![URL-Umleitung in Azure Front Door][1]

## <a name="redirection-types"></a>Umleitungstypen
Ein Umleitungstyp legt den Antwortstatuscode für die Clients fest und informiert so über den Zweck der Umleitung. Die folgenden Umleitungstypen werden unterstützt:

- **301 Moved permanently (301 Permanent verschoben)** : Gibt an, dass der Zielressource ein neuer permanenter URI zugewiesen wurde und dass alle zukünftigen Verweise auf diese Ressource einen der beigefügten URIs verwenden sollten. Verwenden Sie den Statuscode 301 für die HTTP-HTTPS-Umleitung. 
- **302 Found (302 Gefunden)** : Gibt an, dass sich die Zielressource vorübergehend unter einem anderen URI befindet. Da die Umleitung gelegentlich geändert werden kann, sollte der Client weiterhin den effektiven Anforderungs-URI für künftige Anforderungen verwenden.
- **307 Temporary redirect (307 Temporäre Umleitung)** : Gibt an, dass sich die Zielressource vorübergehend unter einem anderen URI befindet. Der Benutzer-Agent DARF die Anforderungsmethode NICHT ändern, wenn sie eine automatische Umleitung an diesen URI ausführt. Da die Umleitung im Laufe der Zeit geändert werden kann, sollte der Client weiterhin den ursprünglichen, effektiven Anforderungs-URI für künftige Anforderungen verwenden.
- **308 Permanent redirect (308 Dauerhafte Umleitung)** : Gibt an, dass der Zielressource ein neuer permanenter URI zugewiesen wurde und dass alle zukünftigen Verweise auf diese Ressource einen der beigefügten URIs verwenden sollten. Clients mit Funktionen zum Bearbeiten von Links sollten Verweise zum effektiven Anforderungs-URI möglichst mit einem oder mehreren neuen Verweisen automatisch neu verknüpfen, die vom Server gesendet wurden.

## <a name="redirection-protocol"></a>Umleitungsprotokoll
Sie können das Protokoll festlegen, das für die Umleitung verwendet werden soll. Dadurch wird einer der häufigsten Anwendungsfälle der Umleitungsfunktion ermöglicht, d.h. das Festlegen einer HTTP-HTTPS-Umleitung.

- **Nur HTTPS**: Legen Sie das Protokoll auf „Nur HTTPS“ fest, wenn der Datenverkehr von HTTP zu HTTPS umgeleitet werden soll. Es wird empfohlen, die Umleitung in Azure Front Door Service immer auf „Nur HTTPS“ zu setzen.
- **Nur HTTP**: Über diese Option wird die eingehende Anforderung zu HTTP umgeleitet. Verwenden Sie diese Einstellung nur, wenn der Datenverkehr im HTTP-Format, d.h. nicht verschlüsselt, bleiben soll.
- **Übereinstimmungsanforderung** : Über diese Option wird das von der eingehenden Anforderung verwendete Protokoll beibehalten. Das bedeutet, dass eine HTTP-Anforderung nach der Umleitung im HTTP-Format und eine HTTPS-Anforderung im HTTPS-Format bleibt.

## <a name="destination-host"></a>Zielhost
Bei der Konfiguration einer Umleitung können Sie auch den Hostnamen und die Domäne für die Umleitungsanforderung ändern. Über dieses Feld können Sie festlegen, dass der Hostname in der URL für die Umleitung geändert wird, oder aber den Hostnamen aus der eingehenden Anforderung beibehalten. Mit anderen Worten: Über dieses Feld lassen sich alle an https://www.contoso.com/ * gesendeten Anforderungen zu https://www.fabrikam.com/ * umleiten.

## <a name="destination-path"></a>Zielpfad
Für den Fall, dass Sie das Pfadsegment einer URL im Zuge der Umleitung ersetzen möchten, können Sie in diesem Feld den neuen Pfadwert festlegen. Alternativ können Sie den Pfadwert als Teil der Umleitung auch beibehalten. Mit anderen Worten: Über dieses Feld lassen sich alle an https://www.contoso.com/ * gesendeten Anforderungen zu https://www.contoso.com/redirected-site * umleiten.

## <a name="query-string-parameters"></a>Abfragezeichenfolge-Parameter
Sie haben die Möglichkeit, die Abfragezeichenfolge-Parameter in der umgeleiteten URL zu ersetzen. Damit die vorhandene Abfragezeichenfolge in der eingehenden Anforderungs-URL ersetzt wird, setzen Sie dieses Feld auf „Ersetzen“ und legen dann den entsprechenden Wert fest. Alternativ können Sie die ursprünglichen Abfragezeichenfolgen auch übernehmen, indem Sie das Feld auf „Beibehalten“ setzen. Beispiel: Über dieses Feld lässt sich der gesamte an https://www.contoso.com/foo/bar gesendete Datenverkehr zu https://www.contoso.com/foo/bar?&utm_referrer=https%3A%2F%2Fwww.bing.com%2F umleiten. 

## <a name="destination-fragment"></a>Zielfragment
Als Zielfragment wird der Teil der URL nach „#“ bezeichnet, der normalerweise von Browsern verwendet wird, um einen bestimmten Abschnitt einer Seite anzuzeigen. Über dieses Feld können Sie festlegen, dass ein Fragment zur Umleitungs-URL hinzugefügt wird.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über das [Erstellen einer Azure Front Door Service-Konfiguration](quickstart-create-front-door.md).
- Informieren Sie sich über die [Funktionsweise von Azure Front Door Service](front-door-routing-architecture.md).

<!--Image references-->
[1]: ./media/front-door-url-redirect/front-door-url-redirect.png