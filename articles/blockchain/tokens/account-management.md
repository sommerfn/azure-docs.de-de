---
title: Azure Blockchain Tokens-Kontoverwaltung
description: Über die Azure Blockchain Tokens-Kontoverwaltung können Sie Gruppen erstellen und Blockchainkonten verknüpfen, um den Zugriff auf Blockchainaktionen zu steuern.
services: azure-blockchain
author: PatAltimore
ms.author: patricka
ms.date: 11/04/2019
ms.topic: conceptual
ms.service: azure-blockchain
ms.reviewer: brendal
ms.openlocfilehash: 91dcadd3c7704b7b8c6cab45005bc83e1bba18bb
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73579777"
---
# <a name="azure-blockchain-tokens-account-management"></a>Azure Blockchain Tokens-Kontoverwaltung

[!INCLUDE [Preview note](./includes/preview.md)]

In einer Blockchainlösung müssen Benutzer Token, die mit dem Azure Blockchain Tokens Service erstellt wurden, ggf. über verschiedene Zugriffsebenen abrufen. In den meisten Blockchainszenarien müssen Sie verschiedene Blockchainkonten planen und bereitstellen, die im Ledger enthalten sind. Außerdem kann es erforderlich sein, den Zugriff aller Teilnehmer zu verwalten. Über die Azure Blockchain Tokens-Kontoverwaltung können Sie Gruppen erstellen und Blockchainkonten verknüpfen, um den Zugriff auf Blockchainaktionen zu steuern.

## <a name="blockchain-networks"></a>Blockchainnetzwerke

Azure Blockchain Tokens ermöglicht die Bereitstellung und Verwaltung von Token in mehreren Blockchainnetzwerken. Sie können einen einzelnen oder mehrere Blockchain-Ledger mit dem Dienst verbinden.

## <a name="accounts"></a>Konten

Für Blockchainnetzwerke, die mit Azure Blockchain Tokens verbunden sind, erstellt und verwaltet der Dienst die aus privatem und öffentlichem Schlüssel bestehenden Schlüsselpaare des Kontos und führt die Transaktionssignierung und -übermittlung aus. Azure Blockchain Tokens bietet außerdem die Identitätszuordnung, um Konten mit der Identität für öffentliche Schlüssel im Ledger abzugleichen.

## <a name="groups"></a>Gruppen

Über Gruppen können Sie eine große Anzahl von Blockchainkonten in verbundenen Netzwerken verwalten. Sie können nachverfolgen und überwachen, welche Anwendungen und Benutzer aus dem Verzeichnis in der Lage sind, Konten über Azure Blockchain Tokens-APIs zu verwenden. Beispielsweise können Sie einige Konten gruppieren, die unterschiedliche Geschäftsbereiche oder Rollen darstellen und auf Blockchaintoken zugreifen.

Sie können eine Gruppe auch einem Azure Active Directory-Benutzer oder -Dienstprinzipal zuordnen. Der Prinzipal verfügt dann über Berechtigungen für die Gruppe und zugehörigen Konten.  

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die verfügbaren [Azure Blockchain Tokens-Vorlagen](templates.md).
