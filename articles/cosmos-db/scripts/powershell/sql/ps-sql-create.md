---
title: 'Azure PowerShell-Skript: Azure Cosmos DB – Erstellen einer Datenbank und eines Containers für die SQL-API (Core-API)'
description: 'Azure PowerShell-Skript: Azure Cosmos DB – Erstellen einer Datenbank und eines Containers für die SQL-API (Core-API)'
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 09/20/2019
ms.author: mjbrown
ms.openlocfilehash: eee1e31808412dc5e4308dee92f3685507e771f3
ms.sourcegitcommit: 83df2aed7cafb493b36d93b1699d24f36c1daa45
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2019
ms.locfileid: "71178797"
---
# <a name="create-a-database-and-container-for-azure-cosmos-db---sql-core-api"></a>Erstellen einer Datenbank und eines Containers für Azure Cosmos DB – SQL-API (Core-API)

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Beispielskript

Dieses Skript erstellt ein Cosmos-Konto für die SQL-API (Core-API) in zwei Regionen mit Konsistenz auf Sitzungsebene, eine Datenbank mit gemeinsam genutztem Durchsatz und einen Container mit einem Partitionsschlüssel, benutzerdefinierter Indizierungsrichtlinie, einer Richtlinie für eindeutige Schlüssel, TTL, dediziertem Durchsatz und einer Konfliktlösungsrichtlinie vom Typ „Letzter Schreiber gewinnt“ mit einem benutzerdefinierten Konfliktauflösungspfads, der im Fall von `multipleWriteLocations=true` verwendet wird.

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/sql/ps-sql-create.ps1 "Create an account, database, and container for SQL (Core) API")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe und alle damit verbundenen Ressourcen entfernt werden.

```powershell
Remove-AzResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
|**Azure-Ressourcen**| |
| [New-AzResource](https://docs.microsoft.com/powershell/module/az.resources/new-azresource) | Dient zum Erstellen einer Ressource. |
|**Azure-Ressourcengruppen**| |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |
|||

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](https://docs.microsoft.com/powershell/).

Zusätzliche PowerShell-Skriptbeispiele für Azure Cosmos DB finden Sie unter [PowerShell-Skripts für Azure Cosmos DB](../../../powershell-samples.md).
