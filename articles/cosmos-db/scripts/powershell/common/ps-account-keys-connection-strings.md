---
title: 'Azure PowerShell-Skript: Kontoschlüssel- und Verbindungszeichenfolgenvorgänge für ein Azure Cosmos-Konto'
description: 'Beispiel für ein Azure PowerShell-Skript: Kontoschlüssel- und Verbindungszeichenfolgenvorgänge für ein Azure Cosmos-Konto'
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 05/20/2019
ms.author: mjbrown
ms.openlocfilehash: 55f28ef6c6ff82f0b82b10ac1dd7931ac46aa2fc
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2019
ms.locfileid: "67602512"
---
# <a name="connection-string-and-account-key-operations-for-an-azure-cosmos-account-using-powershell"></a>Verbindungszeichenfolgen- und Kontoschlüsselvorgänge für ein Azure Cosmos-Konto mithilfe von PowerShell

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Beispielskript

In diesem Beispiel müssen Ressourcengruppe und Konto vorhanden sein. Verwenden Sie ein vorhandenes PowerShell-„Erstellen“-Beispiel, um zuerst ein Konto zur Verfügung zu stellen.

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/common/ps-account-keys-connection-strings.ps1 "Connection strings and account keys for Azure Cosmos account")]

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
| [Invoke-AzResourceAction](https://docs.microsoft.com/powershell/module/az.resources/invoke-azresourceaction) | Dient zum Aufrufen einer Aktion für eine Ressource. |
|**Azure-Ressourcengruppen**| |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |
|||

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](https://docs.microsoft.com/powershell/).

Zusätzliche PowerShell-Skriptbeispiele für Azure Cosmos DB finden Sie unter [PowerShell-Skripts für Azure Cosmos DB](../../../powershell-samples.md).