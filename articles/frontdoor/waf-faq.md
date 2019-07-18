---
title: 'Azure Web Application Firewall: häufig gestellte Fragen'
description: Diese Seite enthält Antworten auf häufig gestellte Fragen zu Azure Front Door Service.
services: frontdoor
documentationcenter: ''
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/10/2019
ms.author: kumud
ms.reviewer: tyao
ms.openlocfilehash: c993e465bc439ff52cba3241dbff64b7655d1f12
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2019
ms.locfileid: "67849178"
---
# <a name="frequently-asked-questions-for-azure-web-application-firewall"></a>Häufig gestellte Fragen zu Azure Web Application Firewall

In diesem Artikel werden allgemeine Fragen zu den Features und Funktionen von Azure Web Application Firewall (WAF) beantwortet. 

## <a name="what-is-azure-waf"></a>Was ist Azure WAF?

Azure WAF ist eine Web Application Firewall, die dazu beiträgt, Ihre Webanwendungen vor allgemeinen Bedrohungen wie Einschleusung von SQL-Befehlen, Cross-Site Scripting und anderen Web-Exploits zu schützen. Sie können eine WAF-Richtlinie mit einer Kombination aus benutzerdefinierten und verwalteten Regeln definieren, um den Zugriff auf Ihre Webanwendungen zu steuern.

Eine Azure WAF-Richtlinie kann auf Webanwendungen angewendet werden, die in Application Gateway- oder Azure Front Door-Diensten gehostet werden.

## <a name="what-is-waf-for-azure-front-door-service"></a>Was ist WAF für Azure Front Door Service? 

Azure Front Door Service ist ein hochgradig skalierbares, global verteiltes APN (Application Delivery Network) und CDN (Content Delivery Network). Nach der Integration in Front Door unterbindet Azure WAF Denial-of-Service-Angriffe und gezielte Anwendungsangriffe im Edgebereich nahe der Angriffsquelle, bevor Angreifer in Ihr virtuelles Netzwerk eindringen, und bietet Schutz ohne Leistungseinbußen.

## <a name="does-azure-waf-support-https"></a>Wird HTTPS von Azure WAF unterstützt?

Front Door Service bietet SSL-Abladung. WAF lässt sich nativ in Front Door integrieren und kann Anforderungen nach der Entschlüsselung untersuchen.

## <a name="does-azure-waf-support-ipv6"></a>Wird IPv6 von Azure WAF unterstützt?

Ja. Sie können IP-Einschränkungen für IPv4 und IPv6 konfigurieren.

## <a name="how-up-to-date-are-the-managed-rule-sets"></a>Wie aktuell sind die verwalteten Regelsätze?

Wir tun unser Bestes, um mit der dynamischen Bedrohungslage Schritt zu halten. Nachdem eine neue Regel aktualisiert wurde, wird sie dem Standardregelsatz mit einer neuen Versionsnummer hinzugefügt.

## <a name="what-is-the-propagation-time-if-i-make-a-change-to-my-waf-policy"></a>Wie lange dauert es, bis eine Änderung, die ich an meiner WAF-Richtlinie vorgenommen habe, verteilt wurde?

Das globale Bereitstellen einer WAF-Richtlinie dauert in der Regel etwa fünf Minuten, geht häufig aber schneller.

## <a name="can-waf-policies-be-different-for-different-regions"></a>Können für verschiedene Regionen unterschiedliche WAF-Richtlinien verwendet werden?

Wenn WAF in Front Door Service integriert wird, ist WAF eine globale Ressource. Die gleiche Konfiguration gilt also für alle Front Door-Standorte.
 
## <a name="how-do-i-limit-access-to-my-back-end-to-be-from-front-door-only"></a>Wie kann ich den Zugriff auf mein Back-End allein auf Zugriffe über Front Door beschränken?

Sie können die IP-Zugriffssteuerungsliste Ihres Back-Ends so konfigurieren, dass Zugriffe nur über ausgehende IP-Adressbereiche von Front Door zugelassen werden und jeglicher Direktzugriff über das Internet abgelehnt wird. Diensttags werden unterstützt und können für Ihr virtuelles Netzwerk verwendet werden. Darüber hinaus können Sie überprüfen, ob das HTTP-Headerfeld „X-Forwarded-Host“ für Ihre Webanwendung gültig ist.




## <a name="which-azure-waf-options-should-i-choose"></a>Welche Azure WAF-Optionen soll ich auswählen?

Beim Anwenden von WAF-Richtlinien in Azure gibt es zwei Optionen. WAF mit Azure Front Door ist eine global verteilte Edgesicherheitslösung. WAF mit Application Gateway ist eine regionale, dedizierte Lösung. Es empfiehlt sich, eine Lösung zu wählen, die zu Ihren individuellen Leistungs- und Sicherheitsanforderungen passt. Weitere Informationen finden Sie unter [Lastenausgleich mit der Azure-Suite für die Anwendungsbereitstellung](https://docs.microsoft.com/azure/frontdoor/front-door-lb-with-azure-app-delivery-suite).


## <a name="do-you-support-same-waf-features-in-all-integrated-platforms"></a>Werden auf allen integrierten Plattformen die gleichen WAF-Funktionen unterstützt?

ModSec CRS 2.2.9- und CRS 3.0-Regeln werden derzeit nur bei Verwendung von WAF mit Application Gateway unterstützt. Ratenbegrenzung, Geofilterung und von Azure verwaltete Standardregelsatz-Regeln werden nur bei Verwendung von WAF mit Azure Front Door unterstützt.

## <a name="is-ddos-protection-integrated-with-front-door"></a>Ist DDoS-Schutz in Front Door integriert? 

Azure Front Door wird global im Edgebereich des Azure-Netzwerks verteilt. Dadurch kann der Dienst groß angelegte Angriffe absorbieren und geografisch isolieren. Sie können eine benutzerdefinierte WAF-Richtlinie erstellen, um HTTP(S)-Angriffe mit bekannten Signaturen automatisch zu blockieren sowie eine Ratenbegrenzung anzuwenden. Darüber hinaus können Sie DDoS Protection Standard für das VNET aktivieren, in dem Ihre Back-Ends bereitgestellt werden. Azure DDoS Protection Standard-Kunden profitieren von weiteren Vorteilen wie Kostenschutz, SLA-Garantie und Zugang zu Experten des DDoS Rapid Response-Teams für Soforthilfe im Angriffsfall. 

## <a name="next-steps"></a>Nächste Schritte

- Informationen zu Azure Web Application Firewall finden Sie [hier](waf-overview.md).
- Erfahren Sie mehr zu [Azure Front Door](front-door-overview.md).
