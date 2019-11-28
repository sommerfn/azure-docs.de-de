---
title: 'Azure Resource Manager: Erstellen einer Einzeldatenbank'
description: Erstellen Sie mithilfe der Azure Resource Manager-Vorlage eine Einzeldatenbank in Azure SQL-Datenbank.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: mumian
ms.author: jgao
ms.reviewer: carlrab
ms.date: 06/28/2019
ms.openlocfilehash: 5d090add7bdb2c3ee08f4c186bd57d63f14ab113
ms.sourcegitcommit: 4c831e768bb43e232de9738b363063590faa0472
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/23/2019
ms.locfileid: "74422557"
---
# <a name="quickstart-create-a-single-database-in-azure-sql-database-using-the-azure-resource-manager-template"></a>Schnellstart: Erstellen einer Einzeldatenbank in Azure SQL-Datenbank mithilfe der Azure Resource Manager-Vorlage

Die Erstellung einer [Einzeldatenbank](sql-database-single-database.md) ist die schnellste und einfachste Bereitstellungsoption zum Erstellen einer Datenbank in Azure SQL-Datenbank. In dieser Schnellstartanleitung wird gezeigt, wie Sie mit der Azure Resource Manager-Vorlage eine Einzeldatenbank erstellen. Weitere Informationen finden Sie in der [Dokumentation zu Azure Resource Manager](/azure/azure-resource-manager/).

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/).

## <a name="create-a-single-database"></a>Erstellen einer Einzeldatenbank

Eine Einzeldatenbank enthält einen definierten Satz von Compute-, Arbeitsspeicher-, EA- und Speicherressourcen und verwendet eins von zwei möglichen [Kaufmodellen](sql-database-purchase-models.md). Wenn Sie eine Einzeldatenbank erstellen, legen Sie auch einen [SQL-Datenbank-Server](sql-database-servers.md) für ihre Verwaltung fest und platzieren ihn in einer [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) in einer bestimmten Region.

Die folgende JSON-Datei ist die Vorlage, die in diesem Artikel verwendet wird. Die Vorlage ist in [GitHub](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/SQLServerAndDatabase/azuredeploy.json) gespeichert. Weitere Vorlagenbeispiele für Azure SQL-Datenbank finden Sie in [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Sql&pageNumber=1&sort=Popular).

[!code-json[create-azure-sql-database-server-and-database](~/resourcemanager-templates/SQLServerAndDatabase/azuredeploy.json)]

Wählen Sie **Ausprobieren** aus dem folgenden PowerShell-Codeblock, um die Azure Cloud Shell zu öffnen.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter a project name that is used for generating resource names"
$location = Read-Host -Prompt "Enter an Azure location (i.e. centralus)"
$adminUser = Read-Host -Prompt "Enter the SQL server administrator username"
$adminPassword = Read-Host -Prompt "Enter the SQl server administrator password" -AsSecureString

$resourceGroupName = "${projectName}rg"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile "D:\GitHub\azure-docs-json-samples\SQLServerAndDatabase\azuredeploy.json" -projectName $projectName -adminUser $adminUser -adminPassword $adminPassword

Read-Host -Prompt "Press [ENTER] to continue ..."
```

# <a name="azure-clitabazure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
$projectName = Read-Host -Prompt "Enter a project name that is used for generating resource names"
$location = Read-Host -Prompt "Enter an Azure location (i.e. centralus)"
$adminUser = Read-Host -Prompt "Enter the SQL server administrator username"
$adminPassword = Read-Host -Prompt "Enter the SQl server administrator password" -AsSecureString

$resourceGroupName = "${projectName}rg"

az group create --location $location --name $resourceGroupName

az group deployment create -g $resourceGroupName --template-uri "D:\GitHub\azure-docs-json-samples\SQLServerAndDatabase\azuredeploy.json" `
    --parameters 'projectName=' + $projectName \
                 'adminUser=' + $adminUser \
                 'adminPassword=' + $adminPassword

Read-Host -Prompt "Press [ENTER] to continue ..."
```

* * *

## <a name="query-the-database"></a>Abfragen der Datenbank

Informationen zum Abfragen der Datenbank finden Sie unter [Abfragen der Datenbank](./sql-database-single-database-get-started.md#query-the-database).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Behalten Sie die Ressourcengruppe, den Datenbankserver und die Einzeldatenbank, wenn Sie mit [Nächste Schritte](#next-steps) fortfahren möchten. Die nächsten Schritte zeigen, wie Sie mithilfe verschiedener Methoden eine Verbindung mit Ihrer Datenbank herstellen und die Datenbank abfragen.

So löschen Sie die Ressourcengruppe:

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName
```

# <a name="azure-clitabazure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName
```

* * *

## <a name="next-steps"></a>Nächste Schritte

- Erstellen Sie eine Firewallregel auf Serverebene, um über lokale Tools oder Remotetools eine Verbindung mit der Einzeldatenbank herstellen zu können. Weitere Informationen finden Sie unter [Erstellen einer Firewallregel auf Serverebene](sql-database-server-level-firewall-rule.md).
- Nachdem Sie eine Firewallregel auf Serverebene erstellt haben, können Sie mit verschiedenen Tools und Programmiersprachen eine [Verbindung mit Ihrer Datenbank herstellen und Abfragen ausführen](sql-database-connect-query.md).
  - [Verbinden und Abfragen mit SQL Server Management Studio (SSMS)](sql-database-connect-query-ssms.md)
  - [Verbinden und Abfragen mit Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/quickstart-sql-database?toc=/azure/sql-database/toc.json)
- Informationen zum Erstellen einer Einzeldatenbank mit der Azure CLI finden Sie unter [Azure CLI-Beispiele für Azure SQL-Datenbank](sql-database-cli-samples.md).
- Informationen zum Erstellen einer Einzeldatenbank mit Azure PowerShell finden Sie unter [Azure PowerShell-Beispiele für Azure SQL-Datenbank](sql-database-powershell-samples.md).
