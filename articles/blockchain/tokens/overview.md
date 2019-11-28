---
title: Was ist Azure Blockchain Tokens?
description: Azure Blockchain Tokens ist eine PaaS (Platform-as-a-Service)-Lösung für die Tokenausstellung und -verwaltung.
ms.date: 11/04/2019
ms.topic: overview
ms.reviewer: brendal
ms.openlocfilehash: c1c18298ba82775c9b10bb79c420704bd462e2b4
ms.sourcegitcommit: b77e97709663c0c9f84d95c1f0578fcfcb3b2a6c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/22/2019
ms.locfileid: "74326084"
---
# <a name="what-is-azure-blockchain-tokens"></a>Was ist Azure Blockchain Tokens?

[!INCLUDE [Preview note](./includes/preview.md)]

Azure Blockchain Tokens ist eine PaaS (Platform-as-a-Service)-Lösung für die übergreifende Ausstellung und Verwaltung standardisierter Token für Blockchain-Ledger in Azure.

Mithilfe von Azure Blockchain Tokens können Sie unter Verwendung einer vordefinierten Tokenvorlage standardisierte Token für Ihre Blockchainlösung erstellen. Mit dem Dienst können Sie auch eine eigene Tokenvorlage erstellen. Verwenden Sie nach der Erstellung Azure Blockchain Tokens, um eine Verbindung mit einer Blockchain herzustellen und die Token auszustellen. Nach dem Ausstellen können Sie die Token übergreifend über mehrere Blockchainnetzwerke verwalten.

## <a name="templates"></a>Vorlagen

Verwenden Sie Azure Blockchain Tokens, um eine vordefinierte Tokenvorlage auszuwählen oder eine eigene Tokenvorlage zu erstellen. Azure Blockchain Tokens unterstützt die Zusammenstellung von Tokenvorlagen, die Ihnen das Erstellen einer eigenen Tokenvorlage auf der Grundlage unterstützter Verhalten ermöglicht. Tokenvorlagen können für die meisten Blockchainlösungen verwendet werden, da sie den am häufigsten verwendeten Token zugeordnet sind. Sie können mit einer Vorlage beginnen, diese personalisieren und die Token für die Lösung bereitstellen.

Weitere Informationen zu Azure Blockchain Tokens-Vorlagen finden Sie unter [Azure Blockchain Tokens-Vorlagen](templates.md).

## <a name="management"></a>Verwaltung

Azure Blockchain Tokens unterstützt die Verwaltung über das Azure-Portal oder APIs, um eine Verbindung mit einem vorhandenen Blockchainnetzwerk herzustellen. Derzeit können Sie eine Verbindung mit dem [Azure Blockchain Service](../service/overview.md) oder einer anderen Blockchain aus der Ethereum-Familie herstellen.

Nachdem eine Verbindung mit einem oder mehreren Blockchainnetzwerken hergestellt wurde, können Sie Azure Blockchain Tokens-APIs verwenden, um Token für die Verwendung in der Blockchainlösung auszugeben und zu verwalten. Mithilfe von APIs können Sie die Tokenverwaltung in Ihre Geschäftsanwendungen und Logik integrieren. Beispielsweise können Sie die REST-API zum Verwalten von Token verwenden, anstatt Token direkt in der Blockchain zu verwalten.

## <a name="blockchains-and-accounts"></a>Blockchains und Konten

Azure Blockchain Tokens unterstützt die Verwaltung über das Azure-Portal und APIs. So können Sie neue Gruppen und neue Blockchainkonten in verbundenen Blockchainnetzwerken erstellen. Sie können neue Konten direkt in Ihren verbundenen Netzwerken erstellen. Anschließend werden Ihre privaten Kontoschlüssel in Ihrem Namen von Azure Blockchain Tokens verwaltet. Mithilfe von Gruppen können Sie verschiedene Blockchainkonten aus mehreren Netzwerken gruppieren und die Zugriffssteuerung über die Gruppen verwalten.

Weitere Informationen zur Kontoverwaltung mit Azure Blockchain Tokens finden Sie im Thema zur [Kontoverwaltung mit Azure Blockchain Tokens](account-management.md).

## <a name="token-taxonomy-framework"></a>Token Taxonomy Framework

Azure Blockchain Tokens verfügt über ein Fundament, das auf Standards basiert und als „Token Taxonomie Framework (TTF)“ bezeichnet wird. TTF umfasst eine Reihe von Komponenten, die von der Token-Arbeitsgruppe der [Token Taxonomy Initiative](https://entethalliance.org/participate/token-taxonomy-initiative/) (TTI) erstellt wurden. Die TTI-Arbeitsgruppe definiert eine Geschäftstaxonomie für Token und deren Verhaltensweisen, die auf alle größeren Ledger angewendet werden können, einschließlich Ethereum, Quorum, Corda und Hyperledger Fabric. Das Ziel der Arbeitsgruppe besteht in der Definition eines Frameworks, durch das die Verwendung von Token aus geschäftlicher Sicht standardisiert wird, um die tokenbasierte Entwicklung weiter zu vereinfachen und zu demokratisieren. Wenn die Branche diese Token und ihr Verhalten auf Geschäftsebene definieren kann, wird die detaillierte Implementierung der Token von der Geschäftslogik getrennt abstrahiert, durch die die Token beeinflusst werden.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die verfügbaren [Azure Blockchain Tokens-Vorlagen](templates.md).
