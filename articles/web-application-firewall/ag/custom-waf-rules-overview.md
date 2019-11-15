---
title: Benutzerdefinierte Regeln für Azure Web Application Firewall (WAF) v2 auf Application Gateway
description: Dieser Artikel enthält eine Übersicht über benutzerdefinierte WAF v2-Regeln (Web Application Firewall) in Azure Application Gateway.
services: web-application-firewall
ms.topic: article
author: vhorne
ms.service: web-application-firewall
ms.date: 10/04/2019
ms.author: victorh
ms.openlocfilehash: 0ac37378797c59d79af5d026200b68154836c5ac
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73585391"
---
# <a name="custom-rules-for-web-application-firewall-v2-on-azure-application-gateway"></a>Benutzerdefinierte Regeln für Web Application Firewall v2 in Azure Application Gateway

Die Web Application Firewall (WAF) v2 in Azure Application Gateway enthält einen vorkonfigurierten, von der Plattform verwalteten Regelsatz, der Schutz vor verschiedensten Angriffsarten bietet. Hierzu zählen unter anderem websiteübergreifende Skripts und die Einschleusung von SQL-Befehlen. WAF-Administratoren können die Regeln des Kernregelsatzes (Core Rule Set, CRS) bei Bedarf durch selbst geschriebene Regeln erweitern. Die Regeln können angeforderten Datenverkehr auf der Grundlage von Abgleichskriterien blockieren oder zulassen.

Benutzerdefinierte Regeln ermöglichen die Erstellung eigener Regeln, die für jede Anforderung ausgewertet werden, die die WAF durchläuft. Diese Regeln haben eine höhere Priorität als die restlichen Regeln in den verwalteten Regelsätzen. Die benutzerdefinierten Regeln enthalten einen Regelnamen, eine Regelpriorität und ein Array von Abgleichsbedingungen. Sind diese Bedingungen erfüllt, erfolgt eine Aktion (Zulassen oder Blockieren).

So können beispielsweise alle Anforderungen von einer IP-Adresse im Bereich 192.168.5.4/24 blockiert werden. In dieser Regel ist der Operator *IPMatch*, „matchValues“ ist der IP-Adressbereich (192.168.5.4/24), und die Aktion ist die Blockierung des Datenverkehrs. Darüber hinaus werden Name und Priorität der Regel festgelegt.

Benutzerdefinierte Regeln unterstützen die Verwendung von Verknüpfungslogik zur Erstellung komplexerer Regeln für Ihre individuellen Sicherheitsanforderungen. Beispiel: (Bedingung 1 **und** Bedingung 2) **oder** Bedingung 3).  In diesem Beispiel soll die in der benutzerdefinierten Regel angegebene Aktion ausgeführt werden, wenn die Bedingung 1 **und** die Bedingung 2 erfüllt sind **oder** wenn die Bedingung 3 erfüllt ist.

Verschiedene Abgleichsbedingungen innerhalb der gleichen Regel sind immer durch **und** verknüpft. Beispiel: Blockieren des Datenverkehrs einer bestimmten IP-Adresse – und zwar nur bei Verwendung eines bestimmten Browsers.

Wenn Sie zwei Bedingungen mit **oder** verknüpfen möchten, müssen sich diese in unterschiedlichen Regeln befinden. Beispiel: Blockieren des Datenverkehrs einer bestimmten IP-Adresse oder Blockieren von Datenverkehr bei Verwendung eines bestimmten Browsers.

> [!NOTE]
> Es können maximal 100 benutzerdefinierte WAF-Regeln verwendet werden. Weitere Informationen zu Application Gateway-Grenzwerten finden Sie unter [Grenzwerte für Azure-Abonnements, -Dienste und -Kontingente sowie allgemeine Beschränkungen](../../azure-subscription-service-limits.md#application-gateway-limits).

In benutzerdefinierten Regeln werden genau wie in den Kernregelsätzen auch reguläre Ausdrücke unterstützt. Entsprechende Beispiele finden Sie unter [Create and use Web Application Firewall custom rules](create-custom-waf-rules.md) (Erstellen und Verwenden benutzerdefinierter Web Application Firewall-Regeln) in den Beispielen 3 und 5.

## <a name="allowing-vs-blocking"></a>Zulassen/Blockieren

Mit benutzerdefinierten Regeln lässt sich Datenverkehr ganz einfach zulassen oder blockieren. So können Sie beispielsweise sämtlichen Datenverkehr aus einem IP-Adressbereich blockieren. Mithilfe einer weiteren Regel können Sie Datenverkehr zulassen, wenn die Anforderung von einem bestimmten Browser stammt.

Wenn Sie etwas zulassen möchten, muss der Parameter `-Action` auf **Allow** festgelegt sein. Wenn Sie etwas blockieren möchten, muss der Parameter `-Action` auf **Block** festgelegt sein.

```azurepowershell
$AllowRule = New-AzApplicationGatewayFirewallCustomRule `
   -Name example1 `
   -Priority 2 `
   -RuleType MatchRule `
   -MatchCondition $condition `
   -Action Allow

$BlockRule = New-AzApplicationGatewayFirewallCustomRule `
   -Name example2 `
   -Priority 2 `
   -RuleType MatchRule `
   -MatchCondition $condition `
   -Action Block
```

`$BlockRule` im obigen Code ist der folgenden benutzerdefinierten Regel in Azure Resource Manager zugeordnet:

```json
"customRules": [
      {
        "name": "blockEvilBot",
        "priority": 2,
        "ruleType": "MatchRule",
        "action": "Block",
        "matchConditions": [
          {
            "matchVariables": [
              {
                "variableName": "RequestHeaders",
                "selector": "User-Agent"
              }
            ],
            "operator": "Contains",
            "negationConditon": false,
            "matchValues": [
              "evilbot"
            ],
            "transforms": [
              "Lowercase"
            ]
          }
        ]
      }
    ], 
```

Diese benutzerdefinierte Regel enthält einen Namen, eine Priorität, eine Aktion und das Array von Abgleichsbedingungen, die erfüllt sein müssen, damit die Aktion ausgeführt wird. Die einzelnen Felder werden in den folgenden Feldbeschreibungen ausführlicher erläutert. Beispiele für benutzerdefinierte Regeln finden Sie unter [Create and use Web Application Firewall custom rules](create-custom-waf-rules.md) (Erstellen und Verwenden benutzerdefinierter Web Application Firewall-Regeln).

## <a name="fields-for-custom-rules"></a>Felder für benutzerdefinierte Regeln

### <a name="name-optional"></a>Name (optional)

Der Name der Regel. Dieser Name wird in den Protokollen angezeigt.

### <a name="priority-required"></a>Priorität (erforderlich)

- Bestimmt die Reihenfolge der Regelbewertung. Je niedriger der Wert, desto früher wird die Regel ausgewertet. Der zulässige Bereich liegt zwischen 1 und 100. 
- Muss für alle benutzerdefinierten Regeln eindeutig sein. Eine Regel mit der Priorität 40 wird vor einer Regel mit der Priorität 80 ausgewertet.

### <a name="rule-type-required"></a>Regeltyp (erforderlich)

Muss derzeit **MatchRule** lauten.

### <a name="match-variable-required"></a>Abgleichsvariable (erforderlich)

Muss eine der folgenden Variablen sein:

- RemoteAddr: IP-Adresse/Hostname der Remotecomputerverbindung
- RequestMethod: HTTP-Anforderungsmethode (GET, POST, PUT, DELETE usw.)
- QueryString: Variable im URI
- PostArgs: Im POST-Text gesendete Argumente. Benutzerdefinierte Regeln mit dieser Abgleichsvariablen werden nur angewendet, wenn der Header „Content-Type“ auf „application/x-www-form-urlencoded“ und „multipart/form-data“ festgelegt ist.
- RequestUri: URI der Anforderung
- RequestHeaders: Header der Anforderung
- RequestBody: Enthält den gesamten Anforderungstext. Benutzerdefinierte Regeln mit dieser Abgleichsvariablen werden nur angewendet, wenn der Header „Content-Type“ auf „application/x-www-form-urlencoded“ festgelegt ist. 
- RequestCookies: Cookies der Anforderung

### <a name="selector-optional"></a>Selektor (optional)

Beschreibt das Feld der matchVariable-Auflistung. Lautet die Abgleichsvariable (matchVariable) beispielsweise „RequestHeaders“, kann der Selektor etwa auf den Header *User-Agent* festgelegt werden.

### <a name="operator-required"></a>Operator (erforderlich)

Muss einer der folgenden Operatoren sein:

- IPMatch: Wird nur verwendet, wenn die Abgleichsvariable *RemoteAddr* lautet.
- Equals: Die Eingabe entspricht dem Abgleichswert (MatchValue).
- Contains
- LessThan
- GreaterThan
- LessThanOrEqual
- GreaterThanOrEqual
- BeginsWith
- EndsWith
- RegEx
- Geomatch (Vorschau)

### <a name="negate-condition-optional"></a>Negationsbedingung (optional)

Negiert die aktuelle Bedingung.

### <a name="transform-optional"></a>Transformation (optional)

Eine Zeichenfolgenliste mit Namen von Transformationen, die vor dem Abgleich ausgeführt werden sollen. Mögliche Transformationen:

- Kleinbuchstaben
- Trim
- UrlDecode
- UrlEncode 
- RemoveNulls
- HtmlEntityDecode

### <a name="match-values-required"></a>Abgleichswerte (erforderlich)

Liste mit Werten für den Abgleich (verknüpft durch *oder*). Dabei kann es sich beispielsweise um IP-Adressen oder um andere Zeichenfolgen handeln. Das Wertformat hängt vom vorherigen Operator ab.

### <a name="action-required"></a>Aktion (erforderlich)

- Allow: Autorisiert die Transaktion. Alle nachfolgenden Regeln werden übersprungen. Die angegebene Anforderung wird also der Zulassungsliste hinzugefügt. Nach erfolgreichem Abgleich findet keine weitere Auswertung mehr statt, und die Anforderung wird an den Back-End-Pool gesendet. Regeln in der Zulassungsliste werden für keine weiteren benutzerdefinierten oder verwalteten Regeln ausgewertet.
- Block: Blockiert die Transaktion auf der Grundlage von *SecDefaultAction* (Erkennungs-/Schutzmodus). Nachdem die Anforderung ausgewertet und der Blockierungsliste hinzugefügt wurde, findet keine weitere Auswertung mehr statt, und die Anforderung wird blockiert. Nachfolgende Anforderungen, die die gleichen Bedingungen erfüllen, werden nicht ausgewertet und einfach blockiert. 
- Log: Ermöglicht der Regel das Schreiben in das Protokoll sowie die Auswertung der restlichen Regeln. Nachfolgende benutzerdefinierte Regeln werden gemäß ihrer Priorität ausgewertet – gefolgt von den verwalteten Regeln.

## <a name="geomatch-custom-rules-preview"></a>Benutzerdefinierte Geomatch-Regeln (Vorschau)

Benutzerdefinierte Regeln ermöglichen die Erstellung maßgeschneiderter Regeln, die genau die Anforderungen Ihrer Anwendungen und Sicherheitsrichtlinien erfüllen. Sie können den Zugriff auf Ihre Webanwendungen nun nach Land oder Region beschränken (als öffentliche Vorschau verfügbar). Wie bei allen benutzerdefinierten Regeln kann diese Logik auch mit anderen Regeln kombiniert werden, um den Anforderungen Ihrer Anwendung zu entsprechen. 

   > [!NOTE]
   > Benutzerdefinierte Geomatch-Regeln sind in den Regionen „USA, Süden-Mitte“ und „Europa, Norden“ verfügbar. Wenn Sie im Portal darauf zugreifen möchten, verwenden Sie bis zur Liveschaltung für alle Benutzer [diesen Link](https://aka.ms/AppGWWAFGeoMatch). 

Wenn Sie den Geomatch-Operator verwenden, kann es sich bei den Selektoren um einen der folgenden zweistelligen Ländercodes handeln. 

|Landesvorwahl | Name des Lands |
| ----- | ----- |
| AD | Andorra |
| AE | Vereinigte Arabische Emirate|
| AF | Afghanistan|
| AG | Antigua und Barbuda|
| AL | Albanien|
| AM | Armenien|
| AO | Angola|
| AR | Argentinien|
| AS | Amerikanisch-Samoa|
| AT | Österreich|
| AU | Australien|
| AZ | Aserbaidschan|
| BA | Bosnien und Herzegowina|
| BB | Barbados|
| BD | Bangladesch|
| BE | Belgien|
| BF | Burkina Faso|
| BG | Bulgarien|
| BH | Bahrain|
| BI | Burundi|
| BJ | Benin|
| BL | St. Barthélemy|
| BN | Brunei Darussalam|
| BO | Bolivien|
| BR | Brasilien|
| BS | Bahamas|
| BT | Bhutan|
| BW | Botsuana|
| BY | Belarus|
| BZ | Belize|
| CA | Kanada|
| CD | Demokratische Republik Kongo|
| CF | Zentralafrikanische Republik|
| CH | Schweiz|
| CI | Côte d'Ivoire|
| CL | Chile|
| CM | Kamerun|
| CN | China|
| CO | Kolumbien|
| CR | Costa Rica|
| CU | Kuba|
| CV | Cabo Verde|
| CY | Zypern|
| CZ | Tschechische Republik|
| DE | Deutschland|
| DK | Dänemark|
| DO | Dominikanische Republik|
| DZ | Algerien|
| EC | Ecuador|
| EE | Estland|
| EG | Ägypten|
| ES | Spanien|
| ET | Äthiopien|
| FI | Finnland|
| FJ | Fidschi|
| FM | Föderierte Staaten von Mikronesien|
| BV | Frankreich|
| GB | Vereinigtes Königreich|
| GE | Georgien|
| GF | Französisch-Guayana|
| GH | Ghana|
| GN | Guinea|
| GP | Guadeloupe|
| GR | Griechenland|
| GT | Guatemala|
| GY | Guayana|
| HK | Hongkong (SAR)|
| HN | Honduras|
| HR | Kroatien|
| HT | Haiti|
| HU | Ungarn|
| id | Indonesien|
| IE | Irland|
| IL | Israel|
| IN | Indien|
| IQ | Irak|
| IR | Islamische Republik Iran|
| IS | Island|
| IT | Italien|
| JM | Jamaika|
| JO | Jordan|
| JP | Japan|
| KE | Kenia|
| KG | Kirgisistan|
| KH | Kambodscha|
| KI | Kiribati|
| KN | St. Kitts und Nevis|
| KP | Demokratische Volksrepublik Korea|
| KR | Republik Korea|
| KW | Kuwait|
| KY | Kaimaninseln|
| KZ | Kasachstan|
| LA | Demokratische Volksrepublik Laos|
| LB | Libanon|
| LI | Liechtenstein|
| LK | Sri Lanka|
| LR | Liberia|
| LS | Lesotho|
| LT | Litauen|
| LU | Luxemburg|
| LV | Lettland|
| LY | Libyen |
| MA | Marokko|
| MD | Republik Moldau|
| MG | Madagaskar|
| MK | Nordmazedonien|
| ML | Mali|
| MM | Myanmar|
| MN | Mongolei|
| MO | Macau (SAR)|
| MQ | Martinique|
| MR | Mauretanien|
| MT | Malta|
| MV | Malediven|
| MW | Malawi|
| MX | Mexiko|
| MY | Malaysia|
| MZ | Mosambik|
| Nicht verfügbar | Namibia|
| NE | Niger|
| NG | Nigeria|
| NI | Nicaragua|
| NL | Niederlande|
| NO | Norwegen|
| NP | Nepal|
| NR | Nauru|
| NZ | Neuseeland|
| OM | Oman|
| PA | Panama|
| PE | Peru|
| PH | Philippinen|
| PK | Pakistan|
| PL | Polen|
| PR | Puerto Rico|
| PT | Portugal|
| PW | Palau|
| PY | Paraguay|
| QA | Katar|
| RE | Réunion|
| RO | Rumänien|
| RS | Serbien|
| RU | Russische Föderation|
| RW | Ruanda|
| SA | Saudi-Arabien|
| SD | Sudan|
| SE | Schweden|
| SG | Singapur|
| SI | Slowenien|
| SK | Slowakei|
| SN | Senegal|
| SO | Somalia|
| SR | Surinam|
| SS | Südsudan|
| SV | El Salvador|
| SY | Arabische Republik Syrien|
| SZ | Swasiland|
| TC | Turks- und Caicosinseln|
| TG | Togo|
| TH | Thailand|
| TN | Tunesien|
| TR | Türkei|
| TT | Trinidad und Tobago|
| TW | Taiwan|
| TZ | Vereinigte Republik Tansania|
| UA | Ukraine|
| UG | Uganda|
| US | USA|
| UY | Uruguay|
| UZ | Usbekistan|
| VC | St. Vincent und die Grenadinen|
| VE | Venezuela|
| VG | Britische Jungferninseln|
| VI | Amerikanische Jungferninseln|
| VN | Vietnam|
| ZA | Südafrika|
| ZM | Sambia|
| ZW | Simbabwe|

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich mit benutzerdefinierten Regeln vertraut gemacht haben, können Sie [Ihre eigenen benutzerdefinierten Regeln erstellen](create-custom-waf-rules.md).
