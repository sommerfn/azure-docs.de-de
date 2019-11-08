---
title: Tokenzusammenstellung in Azure Blockchain Tokens
description: Die Tokenzusammenstellung in Azure Blockchain Tokens bietet Flexibilität zum Erstellen von Token für erweiterte Szenarien.
services: azure-blockchain
author: PatAltimore
ms.author: patricka
ms.date: 11/04/2019
ms.topic: conceptual
ms.service: azure-blockchain
ms.reviewer: brendal
ms.openlocfilehash: a82d7ba606eac5dcafc26b1a8527810a5a21840d
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73577115"
---
# <a name="azure-blockchain-tokens-composability"></a>Tokenzusammenstellung in Azure Blockchain Tokens

[!INCLUDE [Preview note](./includes/preview.md)]

Die Tokenzusammenstellung bietet die erforderliche Flexibilität, um Token für erweiterte Szenarien zu erstellen. Möglicherweise arbeiten Sie mit einem komplexen Szenario, das nicht mit den [vier vordefinierten Tokenvorlagen](templates.md#base-token-types) implementiert werden kann. Die Tokenzusammenstellung ermöglicht es Ihnen, benutzerdefinierte Tokenvorlagen zu entwerfen, indem Sie die definierten Verhaltensweisen hinzufügen oder entfernen, um eine eigene Tokenvorlage zu erstellen. Beim Erstellen einer neuen Tokenvorlage überprüft Azure Blockchain Tokens alle Tokengrammatikregeln. Zusammengestellte Vorlagen werden zur Ausgabe in verbundenen Blockchainnetzwerken im Azure Blockchain Tokens-Dienst gespeichert.

Sie können über das [Tokenverhalten](templates.md#token-behaviors) in den folgenden Abschnitten Ihre eigene Tokenvorlage entwerfen.

## <a name="burnable-b"></a>Burnable (b)

Möglichkeit zum Entfernen von Token aus einem Vorrat.

Wenn Sie beispielsweise Onlineguthabenpunkte für eine Geschenkkarte einlösen, werden die Guthabenpunkte „verbrannt“.

## <a name="delegable-g"></a>Delegable (g)

Möglichkeit zum Delegieren der Durchführung von Aktionen für das Token, dessen Besitzer Sie sind.

Der Delegat kann Aktionen als Besitzer des Tokens durchführen. Beispielsweise können Sie ein delegable-Token zum Implementieren einer Abstimmung verwenden. Durch ein delegierbares Token kann der Abstimmungstokenbesitzer eine andere Person in seinem Namen abstimmen lassen.

## <a name="logable-l"></a>Logable (l)

Möglichkeit zur Protokollierung.

Beispielsweise können Sie ein logable-Token für den Filmverleih an jedes Kino ausgeben, das einen bestimmten Film zeigt. Damit der Film vorgeführt werden kann, muss eine Transaktion für jede Vorstellung protokolliert werden, weil pro Filmvorführung Lizenzgebühren abgeführt werden müssen. Ein Schauspielerverband kann anhand der Filmtoken die fälligen Ausschüttungen pro Vorführung und Kino überprüfen.

## <a name="mint-able-m"></a>Mint-able (m)

Möglichkeit zum Erstellen zusätzlicher Token für die Tokenklasse. Die Rolle „minter“ umfasst das Verhalten „mintable“.

Beispielsweise kann ein Einzelhandelsunternehmen mit dieser Art von Token ein Treueprogramm implementieren. Bei wachsendem Kundenstamm können zusätzliche Treuepunkte für die Kunden erstellt werden.  

## <a name="non-subdividable-or-whole-d"></a>Non-subdividable / whole (~d)

Verhindert, dass ein Token in kleinere Bestandteile zerlegt wird.

Beispielsweise kann ein einzelnes Kunstwerk nicht in mehrere kleinere Teile unterteilt werden. 

## <a name="non-transferable-t"></a>Non-transferable (~t)

Verhindert, dass der ursprüngliche Tokenbesitzer geändert wird.

Beispielsweise ist ein Universitätsdiplom ein nicht übertragbares Token. Nachdem einem Absolventen ein Diplom verliehen wurde, kann das Diplom nicht vom Absolventen auf eine andere Person übertragen werden.

## <a name="roles-r"></a>Roles (r)

Möglichkeit zur Definition von Rollen innerhalb der Tokenvorlagenklasse, um ein spezifisches Verhalten zu erzielen.

Sie können bei der Tokenerstellung eine Liste mit Rollennamen angeben, die ein Token unterstützt. Wenn Rollen angegeben wurden, kann der Benutzer diesen Verhaltensweisen Rollen zuweisen. Aktuell wird nur die Rolle „minter“ unterstützt.

## <a name="singleton-s"></a>Singleton (s)

Einschränkung zur Bereitstellung eines Tokens mit nur einem Element.

Beispielsweise ist ein Museumsartefakt ein Singleton-Token. Museumsartefakte sind einzigartig. Ein Token, das ein Artefakt repräsentiert, enthält nur ein einziges Element in der Bereitstellung.

## <a name="subdividable-d"></a>Subdividable (d)

Möglichkeit zur Unterteilung eines Tokens in kleinere Teile.

Beispielsweise kann ein Euro in Cent unterteilt werden.

## <a name="transferable-t"></a>Transferable (t)

Möglichkeit zur Übertragung des Tokenbesitzes.

Beispielsweise ist ein Eigentumsrecht ein übertragbares Token, das beim Verkauf der Immobilie von einer Person auf eine andere übertragen werden kann.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die [Azure Blockchain Tokens-Kontoverwaltung](account-management.md).
