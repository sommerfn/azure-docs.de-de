---
title: Azure Blockchain Tokens-Vorlagen
description: Azure Blockchain Tokens-Vorlagen sind standardisierte und wiederverwendbare Vorlagen, die das Erstellen und Bereitstellen Ledger-basierter Token vereinfachen.
services: azure-blockchain
author: PatAltimore
ms.author: patricka
ms.date: 11/04/2019
ms.topic: conceptual
ms.service: azure-blockchain
ms.reviewer: brendal
ms.openlocfilehash: 5b8fc2bd4d2cedadb4c50f7ec11def8bea7b37c4
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73577289"
---
# <a name="azure-blockchain-tokens-templates"></a>Azure Blockchain Tokens-Vorlagen

[!INCLUDE [Preview note](./includes/preview.md)]

Eine Azure Blockchain Tokens-Vorlage ist eine standardisierte und wiederverwendbare Vorlage, die das Erstellen und Bereitstellen Ledger-basierter Token vereinfacht. Eine Vorlage besteht aus einer Formel, die auf der [TTF (Token Taxonomy Framework)](overview.md#token-taxonomy-framework)-Grammatik basiert. Die Grammatik umfasst den Basistokentyp und die Verhalten für das Token.  

Die Tokenvorlage **τϜ{d,m,b,r}** beschreibt beispielsweise ein „Fungible“-Basistoken, das „Sub-dividable“, „Mintable“ und „Burnable“ ist und über „Role Support“ verfügt.
  
## <a name="base-token-types"></a>Basistokentypen

Wenn Sie das Ledger-basierte Token für ihr jeweiliges Asset definieren und erstellen, sollten Sie darauf achten, welches Basistoken verwendet werden soll.

### <a name="fungible"></a>Fungible

Fungible-Token (𝜏F) verfügen über einen austauschbaren Wert, solange diese in derselben Klasse oder Reihe enthalten sind. Ein Token hat denselben Wert wie ein anderes Token, oder eine bestimme Anzahl von Token hat denselben Wert wie eine andere entsprechende Anzahl von Token. Beispielsweise ist ein Dollar ein Fungible-Token. Wenn zwei Personen jeweils einen Dollarschein besitzen, könnten Sie diese ohne weitere Folgen austauschen. Die Dollarscheine haben den gleichen Wert. 

### <a name="non-fungible"></a>Non-Fungible

Non-Fungible-Token (𝜏N) sind nicht mit anderen Token desselben Typs austauschbar, da sie normalerweise über unterschiedliche Werte verfügen. Beispielsweise ist ein Eigentumstitel ein Non-Fungible-Token. Eigentumstitel für zwei verschiedene Wohnungen in einem Apartmentkomplex haben aufgrund der Lage oder Etage nicht notwendigerweise denselben Kaufpreis. Der wahrgenommene Wert der beiden Eigentumstitel-Token ist nicht identisch.

### <a name="hybrid"></a>Hybrid

Hybrid-Token verfügen sowohl über Fungible- als auch Non-Fungible-Tokenkomponenten. Ein Hybrid-Token ist ein Basistokentyp, der eine Klasse des anderen Tokentyps besitzt.

#### <a name="hybrid-non-fungible-base-with-fungible-segments"></a>Hybride Non-Fungible-Basis mit Fungible-Segmenten

Eine hybride Non-Fungible-Basis mit Fungible-Segmenttoken verfügt über eine Non-Fungible-Basis mit Fungible-Tokensegmenten.
Beispielsweise entspricht eine Konzertkarte einem Hybrid-Token, während das Datum und die Uhrzeit des Konzerts dem Non-Fungible-Basistoken entspricht. Die Karten in den verschiedenen Sitzordnungen für das jeweilige Konzert sind die Segmente mit Fungible-Token. Die Karten können in den individuellen Sitzordnungen aber nicht übergreifend ausgetauscht werden.

#### <a name="hybrid-fungible-base-with-non-fungible-segments"></a>Hybride Fungible-Basis mit Non-Fungible-Segmenten

Eine hybride Fungible-Basis mit einem Non-Fungible-Segmenttoken verfügt über eine Fungible-Basis mit Non-Fungible-Tokensegmenten. Beispielsweise ist ein hypothekarisch besichertes Wertpapier ein Hybrid-Token, bei dem mehrere Besitzer die Fungible-Basis sind, die auf viele Besitzer verteilt ist. Das Wertpapier ist austauschbar. Die einzelnen Hypotheken sind die Non-Fungible-Segmente, die das spezifische hypothekarisch besicherte Wertpapier darstellen.

## <a name="token-behaviors"></a>Tokenverhalten

Durch das Tokenverhalten werden Fähigkeiten oder Einschränkungen des Tokens definiert. Das Verhalten umfasst unterstützende Eigenschaften, die Teil der Tokendefinition sind. Verhalten können auf alle oder nur auf einen Tokentyp angewendet werden. Verhalten können intern oder extern sein, je nachdem, welche Auswirkungen das Verhalten hat. Mit einem internen Verhalten werden Eigenschaften für das Token selbst aktiviert oder eingeschränkt. Durch ein externes Verhalten wird der Aufruf des Verhaltens von einem externen Akteur aktiviert oder eingeschränkt.

Weitere Informationen zu TTF (Token Taxonomy Framework)-Tokenverhalten, die von Azure Blockchain Tokens unterstützt werden, finden Sie unter [Tokenzusammenstellung](composability.md).

## <a name="pre-built-token-templates"></a>Vordefinierte Tokenvorlagen

Azure Blockchain Tokens bietet vier vordefinierte Tokenvorlagen, die unverändert verwendet werden können. Sie können diese vordefinierten Vorlagen für die meisten Anwendungsfälle verwenden, um schnell mit der Erstellung, Bereitstellung und Verwaltung Ihrer Token zu beginnen.

### <a name="commodity-tokens"></a>Warentoken

Warentoken verfügen über einen gleichbleibenden Wert und sind übertragbar, beispielsweise ein Barrel Öl oder eine Energieeinheit.

**𝜏F{~d,t,m,b,r}** – Fungible, Whole, Transferable, Mintable, Burnable mit Role Support

Viele Blockchainszenarien erfordern Transparenz und Sichtbarkeit in der gesamten Lieferkette oder über mehreren Organisationen hinweg. Warentoken basieren auf diesen gängigen Anwendungsfällen. Die Token sind austauschbar und konsistent. Die Vorlage für Warentoken ist flexibel und lässt sich mit Metadaten anpassen.

### <a name="qualified-tokens"></a>Qualifizierte Token

Qualifizierte Token stellen etwas dar, das erworben wurde. Sie sind in der Regel mit einer Entität verknüpft und nicht übertragbar, beispielsweise eine Urkunde oder ein Strafzettel wegen Falschparkens.

**𝜏N{s,~t}** – Non-Fungible, Singleton und Non-Transferable

Verschiedene Szenarien, die Überprüfungen oder Nachweise beinhalten, erfordern, dass der Besitzer des Tokens nicht geändert werden kann. In einer Reihe von Anwendungsfällen muss ein qualifiziertes Token bereitgestellt werden, unabhängig davon, ob die Zuordnung gut oder schlecht ist.

### <a name="asset-tokens"></a>Assettoken

Assettoken verfügen über einen eindeutigen Wert, der vom Objekt abhängig ist, und stellen keine Waren dar, beispielsweise ein Kunstgegenstand oder ein Eigentumstitel.

**𝜏N{s,t}** – Non-Fungible, Singleton und Transferable

Assettoken können mit Warentoken verwechselt werden. Der Hauptunterschied zwischen den beiden Token besteht darin, dass Assettoken von Natur aus einzigartig sind und dass der Wert unabhängig vom Typ des Tokens ist. Beispielsweise handelt es sich bei einem Kunstgegenstand, wie einem Ölgemälde eines etablierten Künstlers, um ein Assettoken. Ein Kunstdruck der Mona Lisa wird jedoch als Warentoken angesehen. Entsprechend ist ein Eigentumstitel ein Assettoken, da der Wert auf den subjektiven Eigenschaften des Eigentums beruht.

### <a name="ticket-tokens"></a>Tickettoken

Tickettoken haben einen gleichbleibenden Wert, laufen in der Regel jedoch ab, beispielsweise ein Flugticket.

**𝜏N{m,b,r}** – Non-Fungible, Mintable, Burnable mit Role Support

Tickettoken verfügen in der Regel über ein Ablaufdatum, das sie von regulären Warentoken unterscheidet. Beispielsweise verfügen Flugtickets bzw. Karten für Konzerte oder Sportveranstaltungen alle über eine Sitzplatzzuweisung zu einem bestimmten Datum. Es ist nicht einfach möglich, Tickets für ein Veranstaltungsdatum und einen Sitzbereich auszutauschen.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie für Ihr Szenario mehr Flexibilität benötigen, informieren Sie sich über das Erstellen eigener Tokenvorlagen mithilfe der [Tokenzusammenstellung](composability.md).
