---
title: Benutzerdefinierte Regeln für die Azure Web Application Firewall (WAF) v2
description: Dieser Artikel enthält eine Übersicht über benutzerdefinierte WAF v2-Regeln (Web Application Firewall) in Azure Application Gateway.
services: application-gateway
ms.topic: article
author: vhorne
ms.service: application-gateway
ms.date: 6/18/2019
ms.author: victorh
ms.openlocfilehash: 8ae5c9b6b52ea13e3d0981664e8c920cc5b47a01
ms.sourcegitcommit: b4665f444dcafccd74415fb6cc3d3b65746a1a31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2019
ms.locfileid: "72263557"
---
# <a name="overview-custom-rules-for-web-application-firewall-v2"></a>Übersicht: Benutzerdefinierte Regeln für die Web Application Firewall v2

Die Web Application Firewall (WAF) v2 in Azure Application Gateway enthält einen vorkonfigurierten, von der Plattform verwalteten Regelsatz, der Schutz vor verschiedensten Angriffsarten bietet. Hierzu zählen unter anderem Cross-Site Scripting und die Einschleusung von SQL-Befehlen. WAF-Administratoren können die Regeln des Kernregelsatzes bei Bedarf durch selbst geschriebene Regeln erweitern. Die Regeln können angeforderten Datenverkehr auf der Grundlage von Abgleichskriterien blockieren oder zulassen.

Benutzerdefinierte Regeln ermöglichen die Erstellung eigener Regeln. Diese werden für jede Anforderung ausgewertet, die die WAF durchlaufen. Diese Regeln haben eine höhere Priorität als die restlichen Regeln in den verwalteten Regelsätzen. Die benutzerdefinierten Regeln enthalten einen Regelnamen, eine Regelpriorität und ein Array mit Übereinstimmungsbedingungen. Wenn diese Bedingungen erfüllt sind, erfolgt eine Zulassen- oder Blockieren-Aktion.

Sie können beispielsweise eine Regel erstellen, um alle Anforderungen von einer IP-Adresse im Bereich 192.168.5.4/24 zu blockieren. In dieser Regel ist *IPMatch* der Operator und *matchValues* der IP-Adressbereich (192.168.5.4/24), und *action* ist auf die Blockierung des Datenverkehrs festgelegt. Darüber hinaus werden Name und Priorität der Regel festgelegt.

Benutzerdefinierte Regeln unterstützen die Verwendung von Verknüpfungslogik zur Erstellung komplexerer Regeln für Ihre individuellen Sicherheitsanforderungen. „(Condition 1 *and* Condition 2) *or* Condition 3)“ bedeutet beispielsweise, dass bei Erfüllung von Bedingung 1 *und* Bedingung 2 *oder* bei Erfüllung von Bedingung 3 von der WAF die Aktion durchgeführt werden soll, die in der benutzerdefinierten Regel angegeben ist.

Verschiedene Übereinstimmungsbedingungen innerhalb der gleichen Regel sind immer mit *und* verknüpft. Mit einer Regel, für die *und* verwendet wird, wird ggf. das Blockieren von Datenverkehr von einer bestimmten IP-Adresse nur bei Nutzung eines bestimmten Browsers angegeben.

Wenn Sie zwei Bedingungen mit *oder* verknüpfen möchten, müssen sich diese in unterschiedlichen Regeln befinden. Mit der Regel, für die *oder* verwendet wird, kann beispielsweise das Blockieren von Datenverkehr von einer bestimmten IP-Adresse oder bei Nutzung eines bestimmten Browsers angegeben werden.

> [!NOTE]
> Es können maximal 100 benutzerdefinierte WAF-Regeln verwendet werden. Weitere Informationen zu Application Gateway-Grenzwerten finden Sie unter [Grenzwerte für Azure-Abonnements, -Dienste und -Kontingente sowie allgemeine Beschränkungen](../azure-subscription-service-limits.md#application-gateway-limits).

Reguläre Ausdrücke werden auch in benutzerdefinierten Regeln unterstützt, so wie dies bei Kernregelsätzen der Fall ist. Beispiele für diese Regeln finden Sie in Beispiel 3 und 5 unter [Erstellen und Verwenden von benutzerdefinierten Regeln für die Web Application Firewall v2](create-custom-waf-rules.md).

## <a name="allowing-or-blocking-traffic"></a>Zulassen oder Blockieren von Datenverkehr

Mit benutzerdefinierten Regeln lässt sich Datenverkehr ganz einfach zulassen oder blockieren. Sie können beispielsweise den gesamten Datenverkehr blockieren, der aus einem IP-Adressbereich stammt. Mit einer weiteren Regel können Sie Datenverkehr zulassen, wenn die Anforderung von einem bestimmten Browser stammt.

Wenn Sie etwas zulassen möchten, muss der Parameter `-Action` auf **Allow** festgelegt sein. Stellen Sie zum Blockieren sicher, dass der Parameter `-Action` auf **Block** festgelegt ist. Dies ist im folgenden Code dargestellt:

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

Diese benutzerdefinierte Regel enthält einen Namen, eine Priorität, eine Aktion und ein Array mit Übereinstimmungsbedingungen, die erfüllt sein müssen, damit die Aktion ausgeführt wird. Beschreibungen der Felder für benutzerdefinierte Regeln finden Sie in den folgenden Abschnitten. Beispiele für benutzerdefinierte Regeln finden Sie unter [Erstellen und Verwenden von benutzerdefinierten Regeln für die Web Application Firewall v2](create-custom-waf-rules.md).

## <a name="custom-rule-fields"></a>Felder für benutzerdefinierte Regeln

### <a name="name-optional"></a>Name (optional)

Der Name der Regel. Der Name wird in den Protokollen angezeigt.

### <a name="priority-required"></a>Priorität (erforderlich)

- Die Priorität bestimmt die Reihenfolge, in der die Regeln ausgewertet werden. Je niedriger der Wert, desto früher wird die Regel ausgewertet. Der zulässige Bereich liegt zwischen 1 und 100.
- Die Priorität muss für alle benutzerdefinierten Regeln eindeutig sein. Eine Regel mit der Priorität 40 wird vor einer Regel mit der Priorität 80 ausgewertet.

### <a name="rule-type-required"></a>Regeltyp (erforderlich)

Derzeit muss der Regeltyp **MatchRule** lauten.

### <a name="match-variable-required"></a>Übereinstimmungsvariable (erforderlich)

Es muss eine der folgenden Übereinstimmungsvariablen verwendet werden:

- RemoteAddr: Die IP-Adresse bzw. der Hostname der Remotecomputerverbindung.
- RequestMethod: Die HTTP-Anforderungsmethode (GET, POST, PUT, DELETE usw.)
- QueryString: Die Variable im URI.
- PostArgs: Die Argumente, die im POST-Text gesendet werden. Benutzerdefinierte Regeln mit dieser Übereinstimmungsvariablen werden nur angewendet, wenn der Header „Content-Type“ auf „application/x-www-form-urlencoded“ und „multipart/form-data“ festgelegt ist.
- RequestUri: Der URI der Anforderung.
- RequestHeaders: Die Header der Anforderung.
- RequestBody: Die Variable, die den gesamten Anforderungstext enthält. Benutzerdefinierte Regeln mit dieser Übereinstimmungsvariablen werden nur angewendet, wenn der Header „Content-Type“ auf „application/x-www-form-urlencoded“ festgelegt ist. 
- RequestCookies: Die Cookies der Anforderung.

### <a name="selector-optional"></a>Selektor (optional)

Mit dem Selektor wird das Feld der matchVariable-Auflistung beschrieben. Lautet die Übereinstimmungsvariable (matchVariable) „RequestHeaders“, kann der Selektor beispielsweise auf den Header „User-Agent“ festgelegt werden.

### <a name="operator-required"></a>Operator (erforderlich)

Es muss einer der folgenden Operatoren verwendet werden:

- IPMatch: Dieser Operator wird nur verwendet, wenn die Übereinstimmungsvariable *RemoteAddr* lautet.
- Equals: Die Eingabe entspricht dem Übereinstimmungswert.
- Contains
- LessThan
- GreaterThan
- LessThanOrEqual
- GreaterThanOrEqual
- BeginsWith
- EndsWith
- RegEx

### <a name="negate-condition-optional"></a>Negationsbedingung (optional)

Negiert die aktuelle Bedingung.

### <a name="transform-optional"></a>Transformation (optional)

Eine Zeichenfolgenliste mit den Namen von Transformationen, die vor der Ermittlung von Übereinstimmungen ausgeführt werden sollen. Transformationen:

- Kleinbuchstaben
- Trim
- UrlDecode
- UrlEncode 
- RemoveNulls
- HtmlEntityDecode

### <a name="match-values-required"></a>Übereinstimmungswerte (erforderlich)

Das Feld „matchValues“ enthält eine Liste mit Werten für den Abgleich auf Übereinstimmungen (verknüpft durch *oder*). Bei diesen Werten kann es sich beispielsweise um IP-Adressen oder andere Zeichenfolgen handeln. Das Wertformat hängt vom vorherigen Operator ab.

### <a name="action-required"></a>Aktion (erforderlich)

Das Feld „Aktion“ enthält die folgenden Optionen: 

- Zulassen: Autorisiert die Transaktion und überspringt alle nachfolgenden Regeln. Dies bedeutet, dass die angegebene Anforderung der Zulassungsliste hinzugefügt wird. Nach dem erfolgreichem Abgleich findet keine weitere Auswertung mehr statt, und die Anforderung wird an den Back-End-Pool gesendet. Regeln in der Zulassungsliste werden für keine weiteren benutzerdefinierten oder verwalteten Regeln ausgewertet.

- Blockieren: Blockiert die Transaktion auf Grundlage von *SecDefaultAction* (Erkennungs-/Schutzmodus). Wie bei der Aktion „Zulassen“ auch, findet nach dem Auswerten der Anforderung und dem Hinzufügen der Blockierungsliste keine weitere Auswertung mehr statt, und die Anforderung wird blockiert. Nachfolgende Anforderungen, die die gleichen Bedingungen erfüllen, werden nicht ausgewertet. Sie werden lediglich blockiert. 

- Protokoll: Ermöglicht der Regel das Schreiben in das Protokoll und die Auswertung der restlichen Regeln. Nachfolgende benutzerdefinierte Regeln werden gemäß ihrer Priorität ausgewertet – gefolgt von den verwalteten Regeln.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun mit benutzerdefinierten Regeln vertraut sind, können Sie [Ihre eigenen benutzerdefinierten Regeln erstellen](create-custom-waf-rules.md).
