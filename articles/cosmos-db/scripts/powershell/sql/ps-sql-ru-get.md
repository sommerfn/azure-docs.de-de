---
title: 'Azure PowerShell-Skript: Azure Cosmos DB – Abrufen des bereitgestellten Durchsatzes (RU/s) für die SQL (Core)-API'
description: 'Azure PowerShell-Skript: Azure Cosmos DB – Abrufen des bereitgestellten Durchsatzes (RU/s) für die SQL (Core)-API'
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 07/03/2019
ms.author: mjbrown
ms.openlocfilehash: c7d7f295589d4e3b152016c2b55cfe07355d1cb0
ms.sourcegitcommit: 83df2aed7cafb493b36d93b1699d24f36c1daa45
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2019
ms.locfileid: "71178762"
---
# <a name="get-the-provisioned-throughput-rus-for-a-database-or-container-for-azure-cosmos-db---sql-core-api"></a>Abrufen des bereitgestellten Durchsatzes (RU/s) für eine Datenbank oder einen Container für Azure Cosmos DB – SQL (Core)-API

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/sql/ps-sql-ru-get.ps1 "Get throughput for a database or container for SQL (Core) API")]

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