---
title: Referenz zur Azure CDN-Regel-Engine | Microsoft-Dokumentation
description: Referenzdokumentation zu den Übereinstimmungsbedingungen und Features der Azure CDN-Regel-Engine.
services: cdn
author: mdgattuso
ms.service: azure-cdn
ms.topic: article
ms.date: 05/31/2019
ms.author: magattus
ms.openlocfilehash: 5fc611af75a7f733576f9343a4375fb56cacc030
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2019
ms.locfileid: "67593147"
---
# <a name="azure-cdn-from-verizon-premium-rules-engine-reference"></a>Referenz zur Regel-Engine für Azure CDN Premium von Verizon

Dieser Artikel enthält ausführliche Beschreibungen der verfügbaren Übereinstimmungsbedingungen und Features für die [Regel-Engine](cdn-verizon-premium-rules-engine.md) des Azure Content Delivery Network (CDN).

Die Regel-Engine fungiert als endgültige Autorität, wie vom CDN bestimmte Typen von Anforderungen verarbeitet werden.

**Allgemeine Zwecke**:

- Außerkraftsetzen oder Definieren einer benutzerdefinierten Cacherichtlinie
- Schützen oder Ablehnen von Anforderungen vertraulicher Inhalte
- Umleiten von Anforderungen
- Speichern benutzerdefinierter Protokolldaten

## <a name="terminology"></a>Begriff

Eine Regel wird mithilfe von [**bedingten Ausdrücken**](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md), [**Übereinstimmungsbedingungen**](cdn-verizon-premium-rules-engine-reference-match-conditions.md) und [**Features**](cdn-verizon-premium-rules-engine-reference-features.md) definiert. Diese Elemente sind in der folgenden Abbildung hervorgehoben:

 ![CDN-Übereinstimmungsbedingung](./media/cdn-rules-engine-reference/cdn-rules-engine-terminology.png)

## <a name="syntax"></a>Syntax

Die Art und Weise, in der Sonderzeichen behandelt werden, hängt davon ab, wie eine Übereinstimmungsbedingung Textwerte behandelt. Eine Übereinstimmungsbedingung (bzw. Feature) kann Text interpretieren als:

1. [**Literalwerte**](#literal-values)
2. [**Platzhalterwerte**](#wildcard-values)
3. [**Reguläre Ausdrücke**](#regular-expressions)

### <a name="literal-values"></a>Literalwerte

Text, der als Literalwert interpretiert wird, behandelt alle Sonderzeichen (mit Ausnahme des Zeichens „%“) als Teil des Werts, für den eine Übereinstimmung gefunden werden muss. Eine Literalübereinstimmungsbedingung, die auf `\'*'\` festgelegt ist, wird demnach nur erfüllt, wenn exakt dieser Wert (d.h. `\'*'\`) gefunden wird.

Ein Prozentzeichen wird zum Angeben der URL-Codierung verwendet (z.B. `%20`).

### <a name="wildcard-values"></a>Platzhalterwerte

Text, der als Platzhalterwert interpretiert wird, weist Sonderzeichen eine zusätzliche Bedeutung zu. In der folgenden Tabelle wird beschrieben, wie die folgende Gruppe von Zeichen interpretiert wird:

Zeichen | BESCHREIBUNG
----------|------------
\ | Ein umgekehrter Schrägstrich wird als Escapezeichen für die in dieser Tabelle angegebenen Zeichen verwendet. Direkt vor dem Sonderzeichen, das mit Escapezeichen versehen werden soll, muss ein umgekehrter Schrägstrich eingegeben werden.<br/>Es folgt als Beispiel die Syntax für das Escapezeichen eines Sternchens: `\*`
% | Ein Prozentzeichen wird zum Angeben der URL-Codierung verwendet (z.B. `%20`).
\* | Ein Sternchen ist ein Platzhalter, der ein oder mehrere Zeichen darstellt.
Leerzeichen | Ein Leerzeichen gibt an, dass eine Übereinstimmungsbedingung entweder mit den angegebenen Werten oder Mustern erfüllt werden kann.
'Wert' | Ein einfaches Anführungszeichen hat keine besondere Bedeutung. Jedoch wird eine Gruppe einfacher Anführungszeichen verwendet, um anzugeben, dass ein Wert als Literalwert behandelt werden soll. Es kann auf folgende Weisen verwendet werden:<br><br/>– Es ermöglicht das Erfüllen einer Übereinstimmungsbedingung, sobald der angegebene Wert mit einem beliebigen Teil des Vergleichswerts übereinstimmt.  Beispielsweise stimmt `'ma'` mit beliebigen der folgenden Zeichenfolgen überein: <br/><br/>/business/**ma**rathon/asset.htm<br/>**ma**p.gif<br/>/business/template.**ma**p<br /><br />– Es ermöglicht die Angabe eines Sonderzeichens als Literalzeichen. Sie können z.B. ein literales Leerzeichen angeben, indem Sie ein Leerzeichen in einfache Anführungszeichen setzen (d.h. `' '` oder `'sample value'`).<br/>– Es ermöglicht die Angabe eines leeren Werts. Geben Sie einen leeren Wert an, indem Sie zwei einfache Anführungszeichen eingeben (d.h. '').<br /><br/>**Wichtig:**<br/>– Wenn der angegebene Wert keinen Platzhalter enthält, wird er automatisch als Literalwert behandelt, d.h., es müssen nicht zwei einfache Anführungszeichen angegeben werden.<br/>– Wenn ein umgekehrter Schrägstrich nicht als Escapezeichen für ein anderes Zeichen in dieser Tabelle fungiert, wird er ignoriert, sofern er zwischen zwei einfachen Anführungszeichen angegeben wird.<br/>– Eine andere Möglichkeit zum Angeben eines Sonderzeichens als Literalzeichen besteht darin, es mithilfe eines umgekehrten Schrägstrichs (d.h. `\`) mit einem Escapezeichen zu versehen.

### <a name="regular-expressions"></a>Reguläre Ausdrücke

Reguläre Ausdrücke definieren ein Muster, das in einem Textwert gesucht wird. Die Notation regulärer Ausdrücke definiert spezifische Bedeutungen für eine Vielzahl von Symbolen. Die folgende Tabelle zeigt, wie Sonderzeichen von Übereinstimmungsbedingungen behandelt werden, die reguläre Ausdrücke unterstützen.

Sonderzeichen | BESCHREIBUNG
------------------|------------
\ | Ein umgekehrter Schrägstrich als Escapezeichen für das nach diesem folgende Zeichen bewirkt, dass das Zeichen als Literalwert behandelt wird, anstatt seine Bedeutung als regulärer Ausdruck zu haben. Es folgt als Beispiel die Syntax für das Escapezeichen eines Sternchens: `\*`
% | The meaning of a percentage symbol depends on its usage.<br/><br/> `%{HTTPVariable}`: This syntax identifies an HTTP variable.<br/>`%{HTTPVariable%Pattern}`: This syntax uses a percentage symbol to identify an HTTP variable and as a delimiter.<br />`\%`: Escaping a percentage symbol allows it to be used as a literal value or to indicate URL encoding (for example, `\%20`).
\* | An asterisk allows the preceding character to be matched zero or more times.
Space | A space character is typically treated as a literal character.
'value' | Single quotes are treated as literal characters. A set of single quotes does not have special meaning.

## Next steps

- [Rules engine match conditions](cdn-verizon-premium-rules-engine-reference-match-conditions.md)
- [Rules engine conditional expressions](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md)
- [Rules engine features](cdn-verizon-premium-rules-engine-reference-features.md)
- [Override HTTP behavior using the rules engine](cdn-verizon-premium-rules-engine.md)
- [Azure CDN overview](cdn-overview.mdDie Bedeutung eines Prozentzeichens hängt von seiner Verwendung ab. Docs
descr`%{HTTPVariable}`: Diese Syntax gibt eine HTTP-Variable an.match`%{HTTPVariable%Pattern}`: Diese Syntax verwendet ein Prozentzeichen, um eine HTTP-Variable und ein Trennzeichen anzugeben.1/2019`\``: Escaping a percentage symbol allows it to be used as a literal value or to indicate URL encoding (for example, `\`20').f the available match conditions and features for the Azure Content Delivery Network (CDN) [rules engine](cdn-verizon-premium-rules-engine.md).

The rules engine is designed to be the final authority on how specific types of requests are processed by the CDN.

**Common uses**:

- Override or define a custom cache policy.
- Secure or deny requests for sensitive content.
- Redirect requests.
- Store custom log data.

## Terminology

A rule is defined through the use of [**conditional expressions**](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md), [**match conditions**](cdn-verizon-premium-rules-engine-reference-match-conditions.md), and [**features**](cdn-verizon-premium-rules-engine-reference-features.md). These elements are highlighted in the following illustration:

 ![CDN match condition](./media/cdn-rules-engine-reference/cdn-rules-engine-terminology.png)

## Syntax

The manner in which special characters are treated varies according to how a match condition or feature handles text values. A match condition or feature may interpret text in one of the following ways:

1. [**Literal values**](#literal-values)
2. [**Wildcard values**](#wildcard-values)
3. [**Regular expressions**](#regular-expressions)

### Literal values

Text that is interpreted as a literal value treats all special characters, with the exception of the % symbol, as a part of the value that must be matched. In other words, a literal match condition set to `\'*'\` is only satisfied when that exact value (that is, `\'*'\`) is found.

A percentage symbol is used to indicate URL encoding (for example, `%20`).

### Wildcard values

Text that is interpreted as a wildcard value assigns additional meaning to special characters. The following table describes how the following set of characters is interpreted:

Character | Description
----------|------------
\ | A backslash is used to escape any of the characters specified in this table. A backslash must be specified directly before the special character that should be escaped.<br/>For example, the following syntax escapes an asterisk: `\*`
% | A percentage symbol is used to indicate URL encoding (for example, `%20`).
\* | An asterisk is a wildcard that represents one or more characters.
Space | A space character indicates that a match condition may be satisfied by either of the specified values or patterns.
'value' | A single quote does not have special meaning. However, a set of single quotes is used to indicate that a value should be treated as a literal value. It can be used in the following ways:<br><br/>- It allows a match condition to be satisfied whenever the specified value matches any portion of the comparison value.  For example, `'ma'` would match any of the following strings: <br/><br/>/business/**ma**rathon/asset.htm<br/>**ma**p.gif<br/>/business/template.**ma**p<br /><br />- It allows a special character to be specified as a literal character. For example, you may specify a literal space character by enclosing a space character within a set of single quotes (that is, `' '` or `'sample value'`).<br/>- It allows a blank value to be specified. Specify a blank value by specifying a set of single quotes (that is, '').<br /><br/>**Important:**<br/>- If the specified value does not contain a wildcard, then it is automatically considered a literal value, which means that it is not necessary to specify a set of single quotes.<br/>- If a backslash does not escape another character in this table, it is ignored when it is specified within a set of single quotes.<br/>- Another way to specify a special character as a literal character is to escape it using a backslash (that is, `\`).

### Regular expressions

Regular expressions define a pattern that is searched for within a text value. Regular expression notation defines specific meanings to a variety of symbols. The following table indicates how special characters are treated by match conditions and features that support regular expressions.

Special Character | Description
------------------|------------
\ | A backslash escapes the character the follows it, which causes that character to be treated as a literal value instead of taking on its regular expression meaning. For example, the following syntax escapes an asterisk: `\*`
% | The meaning of a percentage symbol depends on its usage.<br/><br/> `%{HTTPVariable}`: This syntax identifies an HTTP variable.<br/>`%{HTTPVariable%Pattern}`: This syntax uses a percentage symbol to identify an HTTP variable and as a delimiter.<br />`\%`: Escaping a percentage symbol allows it to be used as a literal value or to indicate URL encoding (for example, `\%20`).
\* | Ein Sternchen ermöglicht, dass für das vorherige Zeichen keine oder mehrere Übereinstimmungen gefunden werden.
Leerzeichen | Ein Leerzeichen wird in der Regel als Literalzeichen behandelt.
'Wert' | Einfache Anführungszeichen werden als Literalzeichen behandelt. Eine Gruppe einfacher Anführungszeichen hat keine besondere Bedeutung.

## <a name="next-steps"></a>Nächste Schritte

- [Übereinstimmungsbedingungen der Regel-Engine](cdn-verizon-premium-rules-engine-reference-match-conditions.md)
- [Regel-Engine – bedingte Ausdrücke](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md)
- [Regel-Engine – Features](cdn-verizon-premium-rules-engine-reference-features.md)
- [Überschreiben des HTTP-Verhaltens mithilfe der Regel-Engine](cdn-verizon-premium-rules-engine.md)
- [Übersicht über das Azure Content Delivery Network](cdn-overview.md)