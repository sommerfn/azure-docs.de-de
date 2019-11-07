---
title: Benutzerdefinierte Regel für Web Application Firewall mit Azure Front Door
description: Erfahren Sie, wie Sie mit benutzerdefinierten Regeln für Web Application Firewall (WAF) Ihre Webanwendungen vor böswilligen Angriffen schützen können.
author: vhorne
ms.service: web-application-firewall
ms.topic: article
services: web-application-firewall
ms.date: 09/05/2019
ms.author: victorh
ms.openlocfilehash: 516e327cca1aa6a691a1d932c5f48c9108d818b4
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510187"
---
#  <a name="custom-rules-for-web-application-firewall-with-azure-front-door"></a>Benutzerdefinierte Regeln für Web Application Firewall mit Azure Front Door

Azure Web Application Firewall (WAF) mit Front Door Service ermöglicht Ihnen, den Zugriff auf Ihre Webanwendungen anhand der Bedingungen zu steuern, die Sie definieren. Eine benutzerdefinierte WAF-Regel besteht aus einer Prioritätsnummer, einem Regeltyp, Übereinstimmungsbedingungen und einer Aktion. Es gibt zwei Arten von benutzerdefinierten Regeln: Übereinstimmungsregeln und Ratenlimitregeln. Eine Übereinstimmungsregel steuert den Zugriff basierend auf einer Reihe von Übereinstimmungsbedingungen, während eine Ratenlimitregel den Zugriff basierend auf Übereinstimmungsbedingungen und den Raten der eingehenden Anforderungen steuert. Sie können eine benutzerdefinierte Regel deaktivieren, damit sie nicht ausgewertet wird, aber dennoch die Konfiguration beibehalten. 

## <a name="priority-match-conditions-and-action-types"></a>Priorität, Übereinstimmungsbedingungen und Aktionstypen

Sie können den Zugriff mit einer benutzerdefinierten WAF-Regel steuern, die eine Prioritätsnummer, einen Regeltyp, ein Array von Übereinstimmungsbedingungen und eine Aktion definiert. 

- **Priorität:** Ein eindeutiger Integerwert, der die Reihenfolge der Auswertung von WAF-Regeln beschreibt. Regeln mit niedrigeren Prioritätswerten werden vor Regeln mit höheren Werten ausgewertet. Prioritätsnummern müssen für alle benutzerdefinierten Regeln eindeutig sein.

- **Aktion:** definiert, wie eine Anforderung bei Übereinstimmung mit einer WAF-Regel weitergeleitet wird. Für den Fall, dass eine Anforderung mit einer benutzerdefinierten Regel übereinstimmt, können Sie eine der folgenden Aktionen auswählen.

    - *Zulassen*: WAF leitet die Anforderung an das Back-End weiter, protokolliert einen Eintrag in WAF-Protokollen und wird beendet.
    - *Blockieren*: Die Anforderung wird blockiert. WAF sendet eine Antwort an den Client, ohne die Anforderung an das Back-End weiterzuleiten. WAF protokolliert einen Eintrag in den WAF-Protokollen.
    - *Protokollieren* – WAF protokolliert einen Eintrag in den WAF-Protokollen und fährt mit der Auswertung der nächsten Regel fort.
    - *Umleiten* WAF leitet die Anforderung an einen angegebenen URI um, protokolliert einen Eintrag in den WAF-Protokollen, und wird beendet.

- **Übereinstimmungsbedingung**: definiert eine Übereinstimmungsvariable, einen Operator und einen Übereinstimmungswert. Jede Regel kann mehrere Übereinstimmungsbedingungen enthalten. Eine Übereinstimmungsbedingung kann auf einer geografischen Position, Client-IP-Adressen (CIDR), der Größe oder einer übereinstimmenden Zeichenfolge basieren. Eine übereinstimmende Zeichenfolge kann mit einer Liste von Übereinstimmungsvariablen abgeglichen werden.
  - **Übereinstimmungsvariable:**
    - RequestMethod
    - QueryString
    - PostArgs
    - RequestUri
    - RequestHeader
    - RequestBody
    - Cookies
  - **Operator:**
    - Beliebig: wird häufig verwendet, um die Standardaktion zu definieren, wenn mit keiner Regel Übereinstimmung vorliegt. Der Beliebig-Operator stimmt mit allem überein.
    - Gleich
    - Contains
    - LessThan: Größenbeschränkung
    - GreaterThan: Größenbeschränkung
    - LessThanOrEqual: Größenbeschränkung
    - GreaterThanOrEqual: Größenbeschränkung
    - BeginsWith
    - EndsWith
    - RegEx
  
  - Die folgenden Vorgänge werden von **Regex** nicht unterstützt: 
    - Rückverweise und Erfassung von Teilausdrücken
    - Willkürliche Assertionen mit einer Breite von 0
    - Unterroutinenverweise und rekursive Muster
    - Bedingte Muster
    - Rückverfolgung von Steuerelementverben
    - Die Einzelbyte-Anweisung – „\C“
    - Die Anweisung für Zeilenvorschubübereinstimmung – „\R“
    - Die Startanweisung zum Zurücksetzen der Übereinstimmung – „\K“
    - Callouts und eingebetteter Code
    - Atomische Gruppierung und besitzanzeigende Quantifizierer

  - **Negation [optional]:** Sie können die *Negationsbedingung* auf TRUE festlegen, wenn das Ergebnis einer Bedingung negiert werden soll.
      
  - **Transformation [optional]:** Eine Zeichenfolgenliste mit Namen von Transformationen, die vor dem Abgleich ausgeführt werden sollen. Mögliche Transformationen:
     - Großbuchstaben 
     - Kleinbuchstaben
     - Trim
     - RemoveNulls
     - UrlDecode
     - UrlEncode
     
   - **Übereinstimmungswert:** Zu den unterstützten HTTP-Anforderungsmethodenwerten zählen:
     - GET
     - POST
     - PUT
     - HEAD
     - DELETE
     - LOCK
     - UNLOCK
     - PROFILE
     - OPTIONS
     - PROPFIND
     - PROPPATCH
     - MKCOL
     - COPY
     - MOVE

## <a name="examples"></a>Beispiele

### <a name="waf-custom-rules-example-based-on-http-parameters"></a>Benutzerdefinierte WAF-Regeln basierend auf HTTP-Parametern

Hier ist ein Beispiel, das die Konfiguration einer benutzerdefinierten Regel mit zwei Übereinstimmungsbedingungen zeigt. Die Anforderungen stammen von einer angegebenen Website, die vom Verweiser definierten wird. Die Abfragezeichenfolge enthält nicht „password“.

```
# http rules example
{
  "name": "AllowFromTrustedSites",
  "priority": 1,
  "ruleType": "MatchRule",
  "matchConditions": [
    {
      "matchVariable": "RequestHeader",
      "selector": "Referer",
      "operator": "Equal",
      "negateCondition": false,
      "matchValue": [
        "www.mytrustedsites.com/referpage.html"
      ]
    },
    {
      "matchVariable": "QueryString",
      "operator": "Contains",
      "matchValue": [
        "password"
      ],
      "negateCondition": true
    }
  ],
  "action": "Allow",
  "transforms": []
}

```
Eine Beispielkonfiguration für das Blockieren der „PUT“-Methode wird im Folgenden gezeigt:

``` 
# http Request Method custom rules
{
  "name": "BlockPUT",
  "priority": 2,
  "ruleType": "MatchRule",
  "matchConditions": [
    {
      "matchVariable": "RequestMethod",
      "selector": null,
      "operator": "Equal",
      "negateCondition": false,
      "matchValue": [
        "PUT"
      ]
    }
  ],
  "action": "Block",
  "transforms": []
}
```

### <a name="size-constraint"></a>Größenbeschränkung

Sie können eine benutzerdefinierte Regel erstellen, die die Größenbeschränkung für eine eingehende Anforderung angibt. Die folgende Regel blockiert z.B. eine URL mit mehr als 100 Zeichen.

```
# http parameters size constraint
{
  "name": "URLOver100",
  "priority": 5,
  "ruleType": "MatchRule",
  "matchConditions": [
    {
      "matchVariable": "RequestUri",
      "selector": null,
      "operator": "GreaterThanOrEqual",
      "negateCondition": false,
      "matchValue": [
        "100"
      ]
    }
  ],
  "action": "Block",
  "transforms": []
}
```

## <a name="next-steps"></a>Nächste Schritte
- [Konfigurieren einer Web Application Firewall-Richtlinie mit Azure PowerShell](waf-front-door-custom-rules-powershell.md) 
- Erfahren Sie mehr über [Web Application Firewall mit Front Door](afds-overview.md)
- Erfahren Sie mehr über das [Erstellen einer Front Door-Instanz](../../frontdoor/quickstart-create-front-door.md).

