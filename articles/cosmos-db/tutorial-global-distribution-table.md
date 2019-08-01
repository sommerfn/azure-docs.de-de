---
title: Tutorial zur globalen Verteilung von Azure Cosmos DB mit der Tabellen-API
description: Hier erfahren Sie, wie Sie die globale Verteilung von Azure Cosmos DB mithilfe der Tabellen-API einrichten.
author: wmengmsft
ms.author: wmeng
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.topic: tutorial
ms.date: 12/13/2018
ms.reviewer: sngun
ms.openlocfilehash: 8562d37d81ce02e150e6ad1cc2a440cf7bb1e5e3
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2019
ms.locfileid: "68693357"
---
# <a name="set-up-azure-cosmos-db-global-distribution-using-the-table-api"></a>Einrichten der globalen Verteilung von Azure Cosmos DB mithilfe der Tabellen-API

In diesem Artikel werden die folgenden Aufgaben behandelt: 

> [!div class="checklist"]
> * Konfigurieren der globalen Verteilung mit dem Azure-Portal
> * Konfigurieren der globalen Verteilung mit der [Tabellen-API](table-introduction.md)

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-to-a-preferred-region-using-the-table-api"></a>Herstellen einer Verbindung mit einer bevorzugten Region mithilfe der Tabellen-API

Um von der [globalen Verteilung](distribute-data-globally.md)zu profitieren, können Clientanwendungen in einer Liste die Reihenfolge angeben, in der Regionen bei Dokumentvorgängen bevorzugt verwendet werden sollen. Legen Sie dazu die Eigenschaft [TableConnectionPolicy.PreferredLocations](/dotnet/api/microsoft.azure.documents.client.connectionpolicy.preferredlocations?view=azure-dotnet) fest. Das Azure Cosmos DB Table API SDK wählt den optimalen Endpunkt für die Kommunikation auf der Grundlage der Kontokonfiguration, der aktuellen regionalen Verfügbarkeit und der angegebenen Präferenzliste aus.

„PreferredLocations“ sollte eine durch Trennzeichen getrennte Liste der bevorzugten Standorte (Multihosting) für Lesevorgänge enthalten. Jede Clientinstanz kann eine Teilmenge dieser Regionen in der bevorzugten Reihenfolge für Lesevorgänge mit geringer Latenz angeben. Die Regionen müssen mit ihren [Anzeigenamen](https://msdn.microsoft.com/library/azure/gg441293.aspx) benannt sein, z.B. `West US`.

Alle Lesevorgänge werden an die erste verfügbare Region in der Liste „PreferredLocations“ gesendet. Wenn bei der Anforderung ein Fehler auftritt, führt der Client ein Failover zur nächsten Region auf der Liste durch, usw.

Die SDKs versuchen, in den in „PreferredLocations“ angegebenen Regionen Lesevorgänge auszuführen. Wenn ein Datenbankkonto in drei Regionen verfügbar ist, der Client aber nur zwei Leseregionen für PreferredLocations angibt, werden auch bei einem Failover keine Lesevorgänge außerhalb der Schreibregion verarbeitet.

Das SDK sendet automatisch alle Schreibvorgänge an die aktuelle Schreibregion.

Wenn die PreferredLocations-Eigenschaft nicht festgelegt ist, werden alle Anforderungen von der aktuellen Schreibregion verarbeitet.

Das ist alles, und dieses Tutorial ist abgeschlossen. Informationen dazu, wie Sie die Konsistenz Ihres global replizierten Kontos verwalten, finden Sie unter [Konsistenzebenen in Azure Cosmos DB](consistency-levels.md). Weitere Informationen zur Funktionsweise der globalen Datenbankreplikation in Azure Cosmos DB finden Sie unter [Globale Verteilung von Daten mit Azure Cosmos DB](distribute-data-globally.md).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie die folgenden Aufgaben ausgeführt:

> [!div class="checklist"]
> * Konfigurieren der globalen Verteilung mit dem Azure-Portal
> * Konfigurieren der globalen Verteilung mithilfe der Table-APIs von Azure Cosmos DB

