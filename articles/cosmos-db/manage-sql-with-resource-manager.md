---
title: Erstellen und Verwalten von Azure Cosmos DB mithilfe von Azure Resource Manager-Vorlagen
description: Verwenden von Azure Resource Manager-Vorlagen zum Erstellen und Konfigurieren der SQL (Core)-API von Azure Cosmos DB
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/31/2019
ms.author: mjbrown
ms.openlocfilehash: 5babcadee02da0ba3e112f75e8b4d1aed5f3339f
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73721070"
---
# <a name="manage-azure-cosmos-db-sql-core-api-resources-using-azure-resource-manager-templates"></a>Verwalten von Ressourcen für die SQL (Core)-API von Azure Cosmos DB mithilfe von Azure Resource Manager-Vorlagen

In diesem Artikel wird beschrieben, wie Sie verschiedene Vorgänge durchführen können, um die Verwaltung Ihrer Azure Cosmos DB-Konten, -Datenbanken und -Container mithilfe von Azure Resource Manager-Vorlagen zu automatisieren. Dieser Artikel enthält nur Beispiele für SQL-API-Konten. Weitere Beispiele für andere API-Typen finden Sie in den Artikeln zur Verwendung von Resource Manager-Vorlagen mit der Azure Cosmos DB-API für [Cassandra](manage-cassandra-with-resource-manager.md), [Gremlin](manage-gremlin-with-resource-manager.md), [MongoDB](manage-mongodb-with-resource-manager.md) und [Tabellen](manage-table-with-resource-manager.md).

Erstellen und Verwalten von Cosmos DB-Konten, Datenbanken und Containern für MongoDB, Gremlin, Cassandra und Tabellen-API.

## Erstellen eines Kontos, einer Datenbank und eines Containers in Azure Cosmos <a id="create-resource"></a>

Erstellen Sie Azure Cosmos DB-Ressourcen mithilfe einer Azure Resource Manager-Vorlage. Mit dieser Vorlage wird ein Azure Cosmos-Konto mit zwei Containern, die sich auf Datenbankebene einen Durchsatz von 400 RU/s teilen, und einem einzelnen Container mit einem dedizierten Durchsatz von 400 RU/s erstellt. Kopieren Sie die Vorlage, und stellen Sie sie wie unten gezeigt bereit, oder besuchen Sie den [Azure-Schnellstartkatalog](https://azure.microsoft.com/resources/templates/101-cosmosdb-sql/), und führen Sie die Bereitstellung über das Azure-Portal durch. Sie können die Vorlage auch auf Ihren lokalen Computer herunterladen oder eine neue Vorlage erstellen und den lokalen Pfad mit dem Parameter `--template-file` angeben.

> [!NOTE]
>
> - Das gleichzeitige Hinzufügen und Entfernen von Speicherorten zu einem Azure Cosmos-Konto bzw. das Ändern anderer Eigenschaften ist nicht möglich. Dies muss in separaten Vorgängen ausgeführt werden.
> - Kontonamen müssen aus Kleinbuchstaben bestehen und weniger als 44 Zeichen enthalten.
> - Zum Aktualisieren der RU/s müssen Sie die Vorlage mit den aktualisierten Eigenschaftswerten für den Durchsatz erneut übermitteln.

[!code-json[create-cosmosdb-sql](~/quickstart-templates/101-cosmosdb-sql/azuredeploy.json)]

> [!NOTE]
> Um einen Container mit einem großen Partitionsschlüssel zu erstellen, fügen Sie die `"version":2`-Eigenschaft innerhalb des `partitionKey`-Objekts in der vorherigen Vorlage ein.

### <a name="deploy-via-powershell"></a>Bereitstellen über PowerShell

Um die Resource Manager-Vorlage mit PowerShell bereitzustellen, **kopieren** Sie das Skript, und klicken Sie auf **Ausprobieren**, um Azure Cloud-Shell zu öffnen. Klicken Sie zum Einfügen des Skripts mit der rechten Maustaste auf die Shell, und wählen Sie **Einfügen** aus:

```azurepowershell-interactive

$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$accountName = Read-Host -Prompt "Enter the account name"
$location = Read-Host -Prompt "Enter the location (i.e. westus2)"
$primaryRegion = Read-Host -Prompt "Enter the primary region (i.e. westus2)"
$secondaryRegion = Read-Host -Prompt "Enter the secondary region (i.e. eastus2)"
$databaseName = Read-Host -Prompt "Enter the database name"
$sharedThroughput = Read-Host -Prompt "Enter the shared database throughput (i.e. 400)"
$sharedContainer1Name = Read-Host -Prompt "Enter the first shared container name"
$sharedContainer2Name = Read-Host -Prompt "Enter the second shared container name"
$dedicatedContainer1Name = Read-Host -Prompt "Enter the dedicated container name"
$dedicatedThroughput = Read-Host -Prompt "Enter the dedicated container throughput (i.e. 400)"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cosmosdb-sql/azuredeploy.json" `
    -accountName $accountName `
    -location $location `
    -primaryRegion $primaryRegion `
    -secondaryRegion $secondaryRegion `
    -databaseName $databaseName `
    -sharedThroughput $ $sharedThroughput `
    -sharedContainer1Name $sharedContainer1Name `
    -sharedContainer2Name $sharedContainer2Name `
    -dedicatedContainer1Name $dedicatedContainer1Name `
    -dedicatedThroughput $dedicatedThroughput

 (Get-AzResource --ResourceType "Microsoft.DocumentDb/databaseAccounts" --ApiVersion "2019-08-01" --ResourceGroupName $resourceGroupName).name
```

Wenn Sie eine lokal installierte Version von PowerShell anstelle von Azure Cloud Shell verwenden möchten, müssen Sie das Azure PowerShell-Modul [installieren](/powershell/azure/install-az-ps). Führen Sie `Get-Module -ListAvailable Az` aus, um die Version zu finden.

### <a name="deploy-via-azure-cli"></a>Bereitstellen über Azure CLI

Um die Resource Manager-Vorlage mit der Azure CLI bereitzustellen, kopieren Sie das Skript, und klicken Sie auf **Ausprobieren**, um Azure Cloud-Shell zu öffnen. Klicken Sie zum Einfügen des Skripts mit der rechten Maustaste auf die Shell, und wählen Sie **Einfügen** aus:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the location (i.e. westus2): ' location
read -p 'Enter the account name: ' accountName
read -p 'Enter the primary region (i.e. westus2): ' primaryRegion
read -p 'Enter the secondary region (i.e. eastus2): ' secondaryRegion
read -p 'Enter the database name: ' databaseName
read -p 'Enter the shared database throughput: sharedThroughput
read -p 'Enter the first shared container name: ' sharedContainer1Name
read -p 'Enter the second shared container name: ' sharedContainer2Name
read -p 'Enter the dedicated container name: ' dedicatedContainer1Name
read -p 'Enter the dedicated container throughput: dedicatedThroughput

az group create --name $resourceGroupName --location $location
az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-sql/azuredeploy.json \
   --parameters accountName=$accountName \
   primaryRegion=$primaryRegion \
   secondaryRegion=$secondaryRegion \
   databaseName=$databaseName \
   sharedThroughput=$sharedThroughput \
   sharedContainer1Name=$sharedContainer1Name \
   sharedContainer2Name=$sharedContainer2Name \
   dedicatedContainer1Name=$dedicatedContainer1Name \
   dedicatedThroughput=$dedicatedThroughput

az cosmosdb show --resource-group $resourceGroupName --name accountName --output tsv
```

Der Befehl `az cosmosdb show` zeigt das neu erstellte Azure Cosmos-Konto, nachdem es bereitgestellt wurde. Wenn Sie eine lokal installierte Version von Azure CLI anstelle von CloudShell verwenden möchten, lesen Sie den Artikel [Azure-Befehlszeilenschnittstelle (CLI)](/cli/azure/).

## Erstellen eines Azure Cosmos DB-Containers mit serverseitiger Funktionalität<a id="create-sproc"></a>

Erstellen Sie mithilfe einer Azure Resource Manager-Vorlage einen Azure Cosmos DB-Container mit einer gespeicherten Prozedur, einem Trigger und einer benutzerdefinierten Funktion. Kopieren Sie die Vorlage, und stellen Sie sie wie unten gezeigt bereit, oder besuchen Sie den [Azure-Schnellstartkatalog](https://azure.microsoft.com/resources/templates/101-cosmosdb-sql-container-sprocs/), und führen Sie die Bereitstellung über das Azure-Portal durch. Sie können die Vorlage auch auf Ihren lokalen Computer herunterladen oder eine neue Vorlage erstellen und den lokalen Pfad mit dem Parameter `--template-file` angeben.

[!code-json[create-cosmosdb-sql-sprocs](~/quickstart-templates/101-cosmosdb-sql-container-sprocs/azuredeploy.json)]

### <a name="deploy-stored-procedure-template-via-powershell"></a>Bereitstellen einer Vorlage für gespeicherte Prozeduren über PowerShell

Um die Resource Manager-Vorlage mit PowerShell bereitzustellen, **kopieren** Sie das Skript, und klicken Sie auf **Ausprobieren**, um Azure Cloud-Shell zu öffnen. Klicken Sie zum Einfügen des Skripts mit der rechten Maustaste auf die Shell, und wählen Sie **Einfügen** aus:

```azurepowershell-interactive

$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$accountName = Read-Host -Prompt "Enter the account name"
$location = Read-Host -Prompt "Enter the location (i.e. westus2)"
$primaryRegion = Read-Host -Prompt "Enter the primary region (i.e. westus2)"
$secondaryRegion = Read-Host -Prompt "Enter the secondary region (i.e. eastus2)"
$databaseName = Read-Host -Prompt "Enter the database name"
$containerName = Read-Host -Prompt "Enter the container name"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cosmosdb-sql-container-sprocs/azuredeploy.json" `
    -accountName $accountName `
    -location $location `
    -primaryRegion $primaryRegion `
    -secondaryRegion $secondaryRegion `
    -databaseName $databaseName `
    -containerName $containerName

 (Get-AzResource --ResourceType "Microsoft.DocumentDb/databaseAccounts" --ApiVersion "2019-08-01" --ResourceGroupName $resourceGroupName).name
```

Wenn Sie eine lokal installierte Version von PowerShell anstelle von Azure Cloud Shell verwenden möchten, müssen Sie das Azure PowerShell-Modul [installieren](/powershell/azure/install-az-ps). Führen Sie `Get-Module -ListAvailable Az` aus, um die Version zu finden.

### <a name="deploy-stored-procedure-template-via-azure-cli"></a>Bereitstellen einer Vorlage für gespeicherte Prozeduren über die Azure-Befehlszeilenschnittstelle

Um die Resource Manager-Vorlage mit der Azure CLI bereitzustellen, kopieren Sie das Skript, und klicken Sie auf **Ausprobieren**, um Azure Cloud-Shell zu öffnen. Klicken Sie zum Einfügen des Skripts mit der rechten Maustaste auf die Shell, und wählen Sie **Einfügen** aus:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the location (i.e. westus2): ' location
read -p 'Enter the account name: ' accountName
read -p 'Enter the primary region (i.e. westus2): ' primaryRegion
read -p 'Enter the secondary region (i.e. eastus2): ' secondaryRegion
read -p 'Enter the database name: ' databaseName
read -p 'Enter the container name: ' containerName

az group create --name $resourceGroupName --location $location
az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-sql-container-sprocs/azuredeploy.json \
   --parameters accountName=$accountName primaryRegion=$primaryRegion secondaryRegion=$secondaryRegion databaseName=$databaseName \
   containerName=$containerName

az cosmosdb show --resource-group $resourceGroupName --name accountName --output tsv
```

## <a name="next-steps"></a>Nächste Schritte

Hier sind einige zusätzlichen Ressourcen:

- [Dokumentation zu Azure Resource Manager](/azure/azure-resource-manager/)
- [Schema von Azure Cosmos DB-Ressourcenanbietern](/azure/templates/microsoft.documentdb/allversions)
- [Schnellstartvorlagen für Azure Cosmos DB](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.DocumentDB&pageNumber=1&sort=Popular)
- [Beheben gängiger Azure-Bereitstellungsfehler mit Azure Resource Manager](../azure-resource-manager/resource-manager-common-deployment-errors.md)
