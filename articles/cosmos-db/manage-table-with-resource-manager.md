---
title: Azure Resource Manager-Vorlagen für die Tabellen-API von Azure Cosmos DB
description: Verwenden Sie Azure Resource Manager-Vorlagen zum Erstellen und Konfigurieren der Tabellen-API von Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/05/2019
ms.author: mjbrown
ms.openlocfilehash: aa169ed7001ad858a2a0373f9d5bbbe770a2a727
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73604476"
---
# <a name="manage-azure-cosmos-db-table-api-resources-using-azure-resource-manager-templates"></a>Verwalten von Ressourcen für die Tabellen-API von Azure Cosmos DB mithilfe von Azure Resource Manager-Vorlagen

In diesem Artikel wird beschrieben, wie Sie verschiedene Vorgänge durchführen können, um die Verwaltung Ihrer Azure Cosmos DB-Konten, -Datenbanken und -Container mithilfe von Azure Resource Manager-Vorlagen zu automatisieren. Dieser Artikel enthält nur Beispiele für Tabellen-API-Konten. Weitere Beispiele für andere API-Typen finden Sie in den Artikeln zur Verwendung von Resource Manager-Vorlagen mit der Azure Cosmos DB-API für [Cassandra](manage-cassandra-with-resource-manager.md), [Gremlin](manage-gremlin-with-resource-manager.md), [MongoDB](manage-mongodb-with-resource-manager.md) und  [SQL](manage-sql-with-resource-manager.md).

## Erstellen von Azure Cosmos-Konto und -Tabelle <a id="create-resource"></a>

Erstellen Sie Azure Cosmos DB-Ressourcen mithilfe einer Azure Resource Manager-Vorlage. Diese Vorlage erstellt ein Azure Cosmos-Konto für die Tabellen-API mit einer Tabelle mit einem Durchsatz von 400 RU/s (Ressourceneinheiten pro Sekunde). Kopieren Sie die Vorlage, und stellen Sie sie wie unten gezeigt bereit, oder besuchen Sie den [Azure-Schnellstartkatalog](https://azure.microsoft.com/resources/templates/101-cosmosdb-table/), und führen Sie die Bereitstellung über das Azure-Portal durch. Sie können die Vorlage auch auf Ihren lokalen Computer herunterladen oder eine neue Vorlage erstellen und den lokalen Pfad mit dem Parameter `--template-file` angeben.

> [!NOTE]
> Kontonamen müssen aus Kleinbuchstaben bestehen und weniger als 31 Zeichen enthalten.

[!code-json[create-cosmos-table](~/quickstart-templates/101-cosmosdb-table/azuredeploy.json)]

### <a name="deploy-via-powershell"></a>Bereitstellen über PowerShell

Um die Resource Manager-Vorlage mit PowerShell bereitzustellen, **kopieren** Sie das Skript, und klicken Sie auf **Ausprobieren**, um Azure Cloud-Shell zu öffnen. Klicken Sie zum Einfügen des Skripts mit der rechten Maustaste auf die Shell, und wählen Sie **Einfügen** aus:

```azurepowershell-interactive

$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$accountName = Read-Host -Prompt "Enter the account name"
$location = Read-Host -Prompt "Enter the location (i.e. westus2)"
$primaryRegion = Read-Host -Prompt "Enter the primary region (i.e. westus2)"
$secondaryRegion = Read-Host -Prompt "Enter the secondary region (i.e. eastus2)"
$tableName = Read-Host -Prompt "Enter the table name"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cosmosdb-table/azuredeploy.json" `
    -primaryRegion $primaryRegion `
    -secondaryRegion $secondaryRegion `
    -tableName $tableName

 (Get-AzResource --ResourceType "Microsoft.DocumentDb/databaseAccounts" --ApiVersion "2015-04-08" --ResourceGroupName $resourceGroupName).name
```

Wenn Sie eine lokal installierte Version von PowerShell anstelle von Azure Cloud Shell verwenden möchten, müssen Sie das Azure PowerShell-Modul [installieren](/powershell/azure/install-az-ps). Führen Sie `Get-Module -ListAvailable Az` aus, um die Version zu finden.

### <a name="deploy-via-azure-cli"></a>Bereitstellen über Azure CLI

Um die Resource Manager-Vorlage mit der Azure CLI bereitzustellen, **kopieren** Sie das Skript, und klicken Sie auf **Ausprobieren**, um Azure Cloud-Shell zu öffnen. Klicken Sie zum Einfügen des Skripts mit der rechten Maustaste auf die Shell, und wählen Sie **Einfügen** aus:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the location (i.e. westus2): ' location
read -p 'Enter the account name: ' accountName
read -p 'Enter the primary region (i.e. westus2): ' primaryRegion
read -p 'Enter the secondary region (i.e. eastus2): ' secondaryRegion
read -p 'Enter the table name: ' tableName

az group create --name $resourceGroupName --location $location
az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-table/azuredeploy.json \
   --parameters accountName=$accountName primaryRegion=$primaryRegion secondaryRegion=$secondaryRegion tableName=$tableName

az cosmosdb show --resource-group $resourceGroupName --name accountName --output tsv
```

Der Befehl `az cosmosdb show` zeigt das neu erstellte Azure Cosmos-Konto, nachdem es bereitgestellt wurde. Wenn Sie eine lokal installierte Version von Azure CLI anstelle von CloudShell verwenden möchten, lesen Sie den Artikel [Azure-Befehlszeilenschnittstelle (CLI)](/cli/azure/).

## Aktualisieren des Durchsatzes (RU/s) für eine Tabelle <a id="table-ru-update"></a>

Mit der folgenden Vorlage wird der Durchsatz einer Tabelle aktualisiert. Kopieren Sie die Vorlage, und stellen Sie sie wie unten gezeigt bereit, oder besuchen Sie den [Azure-Schnellstartkatalog](https://azure.microsoft.com/resources/templates/101-cosmosdb-table-ru-update/), und führen Sie die Bereitstellung über das Azure-Portal durch. Sie können die Vorlage auch auf Ihren lokalen Computer herunterladen oder eine neue Vorlage erstellen und den lokalen Pfad mit dem Parameter `--template-file` angeben.

[!code-json[cosmosdb-table-ru-update](~/quickstart-templates/101-cosmosdb-table-ru-update/azuredeploy.json)]

### <a name="deploy-table-throughput-via-powershell"></a>Bereitstellen von Tabellendurchsatz über PowerShell

Um die Resource Manager-Vorlage mit PowerShell bereitzustellen, **kopieren** Sie das Skript, und klicken Sie auf **Ausprobieren**, um Azure Cloud-Shell zu öffnen. Klicken Sie zum Einfügen des Skripts mit der rechten Maustaste auf die Shell, und wählen Sie **Einfügen** aus:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$accountName = Read-Host -Prompt "Enter the account name"
$tableName = Read-Host -Prompt "Enter the table name"
$throughput = Read-Host -Prompt "Enter new throughput for table"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cosmosdb-table-ru-update/azuredeploy.json" `
    -accountName $accountName `
    -tableName $tableName `
    -throughput $throughput
```

### <a name="deploy-table-template-via-azure-cli"></a>Bereitstellen einer Tabellenvorlage über die Azure CLI

Um die Resource Manager-Vorlage mit der Azure CLI bereitzustellen, kopieren Sie das Skript, und klicken Sie auf **Ausprobieren**, um Azure Cloud-Shell zu öffnen. Klicken Sie zum Einfügen des Skripts mit der rechten Maustaste auf die Shell, und wählen Sie **Einfügen** aus:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the account name: ' accountName
read -p 'Enter the table name: ' tableName
read -p 'Enter the new throughput: ' throughput

az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-table-ru-update/azuredeploy.json \
   --parameters accountName=$accountName tableName=$tableName throughput=$throughput
```

## <a name="next-steps"></a>Nächste Schritte

Hier sind einige zusätzlichen Ressourcen:

- [Dokumentation zu Azure Resource Manager](/azure/azure-resource-manager/)
- [Schema von Azure Cosmos DB-Ressourcenanbietern](/azure/templates/microsoft.documentdb/allversions)
- [Schnellstartvorlagen für Azure Cosmos DB](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.DocumentDB&pageNumber=1&sort=Popular)
- [Beheben gängiger Azure-Bereitstellungsfehler mit Azure Resource Manager](../azure-resource-manager/resource-manager-common-deployment-errors.md)