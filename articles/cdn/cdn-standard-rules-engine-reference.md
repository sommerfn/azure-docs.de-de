---
title: Referenz zur Azure CDN-Standardregel-Engine | Microsoft-Dokumentation
description: Referenzdokumentation zu den Übereinstimmungsbedingungen und Aktionen der Azure CDN-Standardregel-Engine.
services: cdn
author: mdgattuso
ms.service: azure-cdn
ms.topic: article
ms.date: 11/01/2019
ms.author: magattus
ms.openlocfilehash: 6fb7e704f3d33cff8c29386b8aba9d8289037cbb
ms.sourcegitcommit: b2fb32ae73b12cf2d180e6e4ffffa13a31aa4c6f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73615833"
---
# <a name="azure-cdn-from-microsoft-rules-engine-reference"></a>Referenz zur Regel-Engine für Azure CDN von Microsoft

Dieser Artikel enthält ausführliche Beschreibungen der verfügbaren Übereinstimmungsbedingungen und Features für die [Standardregel-Engine](cdn-standard-rules-engine.md) des Azure Content Delivery Network (CDN).

Die Regel-Engine fungiert als endgültige Autorität, wie vom CDN bestimmte Typen von Anforderungen verarbeitet werden.

**Allgemeine Zwecke**:

- Außerkraftsetzen oder Definieren einer benutzerdefinierten Cacherichtlinie
- Umleiten von Anforderungen
- Ändern von HTTP-Anforderungs- und -Antwortheadern

## <a name="terminology"></a>Begriff

Eine Regel wird durch die Verwendung von [**Übereinstimmungsbedingungen**](cdn-standard-rules-engine-match-conditions.md) und [**Aktionen**](cdn-standard-rules-engine-actions.md) definiert. Diese Elemente sind in der folgenden Abbildung hervorgehoben:

 ![Struktur von CDN-Regeln](./media/cdn-standard-rules-engine-reference/cdn-rules-structure.png)

Jede Regel kann bis zu 4 Übereinstimmungsbedingungen und 3 Aktionen aufweisen. Pro CDN-Endpunkt sind maximal 5 Regeln zulässig. Außerdem ist standardmäßig eine Regel aktiv, die als **globale Regel** bezeichnet wird. Dabei handelt es sich um eine Regel ohne Übereinstimmungsbedingungen, bei der die definierten Aktionen immer ausgelöst werden. Diese Regel ist im aktuellen Grenzwert von 5 Regeln eingeschlossen.

## <a name="syntax"></a>Syntax

Die Behandlung von Sonderzeichen ist davon abhängig, wie eine Übereinstimmungsbedingung oder Aktion Textwerte behandelt. Eine Übereinstimmungsbedingung (bzw. Feature) kann Text interpretieren als:

1. [**Literalwerte**](#literal-values)
2. [**Platzhalterwerte**](#wildcard-values)


### <a name="literal-values"></a>Literalwerte

Text, der als Literalwert interpretiert wird, behandelt alle Sonderzeichen (mit Ausnahme des Zeichens „%“) als Teil des Werts, für den eine Übereinstimmung gefunden werden muss. Eine Literalübereinstimmungsbedingung, die auf `\'*'\` festgelegt ist, wird demnach nur erfüllt, wenn exakt dieser Wert (d.h. `\'*'\`) gefunden wird.

Ein Prozentzeichen wird zum Angeben der URL-Codierung verwendet (z. B. `%20`).

### <a name="wildcard-values"></a>Platzhalterwerte

Text, der als Platzhalterwert interpretiert wird, weist Sonderzeichen eine zusätzliche Bedeutung zu. In der folgenden Tabelle wird beschrieben, wie die folgende Gruppe von Zeichen interpretiert wird:

Zeichen | BESCHREIBUNG
----------|------------
\ | Ein umgekehrter Schrägstrich wird als Escapezeichen für die in dieser Tabelle angegebenen Zeichen verwendet. Direkt vor dem Sonderzeichen, das mit Escapezeichen versehen werden soll, muss ein umgekehrter Schrägstrich eingegeben werden.<br/>Es folgt als Beispiel die Syntax für das Escapezeichen eines Sternchens: `\*`
% | Ein Prozentzeichen wird zum Angeben der URL-Codierung verwendet (z. B. `%20`).
\* | Ein Sternchen ist ein Platzhalter, der ein oder mehrere Zeichen darstellt.
Leerzeichen | Ein Leerzeichen gibt an, dass eine Übereinstimmungsbedingung entweder mit den angegebenen Werten oder Mustern erfüllt werden kann.
'Wert' | Ein einfaches Anführungszeichen hat keine besondere Bedeutung. Jedoch wird eine Gruppe einfacher Anführungszeichen verwendet, um anzugeben, dass ein Wert als Literalwert behandelt werden soll. Es kann auf folgende Weisen verwendet werden:<br><br/>– Es ermöglicht das Erfüllen einer Übereinstimmungsbedingung, sobald der angegebene Wert mit einem beliebigen Teil des Vergleichswerts übereinstimmt.  Beispielsweise stimmt `'ma'` mit beliebigen der folgenden Zeichenfolgen überein: <br/><br/>/business/**ma**rathon/asset.htm<br/>**ma**p.gif<br/>/business/template.**ma**p<br /><br />– Es ermöglicht die Angabe eines Sonderzeichens als Literalzeichen. Sie können z.B. ein literales Leerzeichen angeben, indem Sie ein Leerzeichen in einfache Anführungszeichen setzen (d.h. `' '` oder `'sample value'`).<br/>– Es ermöglicht die Angabe eines leeren Werts. Geben Sie einen leeren Wert an, indem Sie zwei einfache Anführungszeichen eingeben (d.h. '').<br /><br/>**Wichtig:**<br/>– Wenn der angegebene Wert keinen Platzhalter enthält, wird er automatisch als Literalwert behandelt, d.h., es müssen nicht zwei einfache Anführungszeichen angegeben werden.<br/>– Wenn ein umgekehrter Schrägstrich nicht als Escapezeichen für ein anderes Zeichen in dieser Tabelle fungiert, wird er ignoriert, sofern er zwischen zwei einfachen Anführungszeichen angegeben wird.<br/>– Eine andere Möglichkeit zum Angeben eines Sonderzeichens als Literalzeichen besteht darin, es mithilfe eines umgekehrten Schrägstrichs (d.h. `\`) mit einem Escapezeichen zu versehen.

## <a name="next-steps"></a>Nächste Schritte

- [Übereinstimmungsbedingungen der Standardregel-Engine](cdn-standard-rules-engine-match-conditions.md)
- [Aktionen der Standardregel-Engine](cdn-standard-rules-engine-actions.md)
- [Erzwingen von HTTPS mithilfe der Standardregel-Engine](cdn-standard-rules-engine.md)
- [Übersicht über das Azure Content Delivery Network](cdn-overview.md)
