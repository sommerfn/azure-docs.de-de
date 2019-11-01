---
title: Erstellen und Verwalten von Azure Cosmos DB mit PowerShell
description: Verwenden Sie Azure PowerShell, um Ihre Azure Cosmos DB-Konten, -Datenbanken, -Container und den zugehörigen Durchsatz zu verwalten.
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 10/23/2019
ms.author: mjbrown
ms.custom: seodec18
ms.openlocfilehash: 978f37d08275de704dd01c0251dde42665fca552
ms.sourcegitcommit: 7efb2a638153c22c93a5053c3c6db8b15d072949
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2019
ms.locfileid: "72882105"
---
# <a name="manage-azure-cosmos-db-sql-api-resources-using-powershell"></a>Verwalten von SQL-API-Ressourcen für Azure Cosmos DB mit PowerShell

In der folgenden Anleitung erfahren Sie, wie Sie PowerShell-Skripts verwenden und die Verwaltung von Azure Cosmos DB-Ressourcen (einschließlich Konten, Datenbanken, Container und Durchsatz) automatisieren. Die Verwaltung von Azure Cosmos DB erfolgt direkt über das Cmdlet „AzResource“ (für den Azure Cosmos DB-Ressourcenanbieter). Sie können sich alle Eigenschaften anzeigen lassen, die mit PowerShell für den Azure Cosmos DB-Ressourcenanbieter verwaltet werden können. Informationen dazu finden Sie unter [Azure Cosmos DB resource provider schema (Schema des Azure Cosmos DB-Ressourcenanbieters)](/azure/templates/microsoft.documentdb/allversions).

Zur plattformübergreifenden Verwaltung von Azure Cosmos DB können Sie die [Azure-Befehlszeilenschnittstelle](manage-with-cli.md), die [REST-API][rp-rest-api] oder das [Azure-Portal](create-sql-api-dotnet.md#create-account) verwenden.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="getting-started"></a>Erste Schritte

Führen Sie die Schritte unter [Installieren und Konfigurieren von Azure PowerShell][powershell-install-configure] aus, um PowerShell zu installieren und sich bei Ihrem Azure-Konto anzumelden.

* Wenn die folgenden Befehle ohne Benutzerbestätigung ausgeführt werden sollen, fügen Sie das Flag `-Force` an den Befehl an.
* Alle folgenden Befehle sind synchron.

## <a name="azure-cosmos-accounts"></a>Azure Cosmos DB-Konten

In den folgenden Abschnitten erfahren Sie, wie Sie Azure Cosmos DB-Konten verwalten. Dabei lernen Sie u. a. Folgendes:

* [Erstellen eines Azure Cosmos DB-Kontos](#create-account)
* [Aktualisieren eines Azure Cosmos DB-Kontos](#update-account)
* [Auflisten aller Azure Cosmos DB-Konten in einem Abonnement](#list-accounts)
* [Abrufen der Eigenschaften eines Azure Cosmos DB-Kontos](#get-account)
* [Löschen eines Azure Cosmos DB-Kontos](#delete-account)
* [Aktualisieren der Tags eines Azure Cosmos DB-Kontos](#update-tags)
* [Anzeigen von Schlüsseln für ein Azure Cosmos DB-Konto](#list-keys)
* [Erneutes Generieren von Schlüsseln für ein Azure Cosmos DB-Konto](#regenerate-keys)
* [Anzeigen von Verbindungszeichenfolgen für ein Azure Cosmos DB-Konto](#list-connection-strings)
* [Anpassen der Failoverpriorität für ein Azure Cosmos DB-Konto](#modify-failover-priority)
* [Auslösen eines manuellen Failovers für ein Azure Cosmos-Konto](#trigger-manual-failover)

### <a id="create-account"></a> Erstellen eines Azure Cosmos DB-Kontos

Der folgende Befehl erstellt ein Azure Cosmos-Datenbankkonto mit einer [Konsistenzrichtlinie](consistency-levels.md) mit [mehreren Regionen][distribute-data-globally] und begrenzter Veraltung:

```azurepowershell-interactive
# Create an Azure Cosmos Account for Core (SQL) API
$resourceGroupName = "myResourceGroup"
$location = "West US 2"
$accountName = "mycosmosaccount" # must be lowercase and < 31 characters .

$locations = @(
    @{ "locationName"="West US 2"; "failoverPriority"=0 },
    @{ "locationName"="East US 2"; "failoverPriority"=1 }
)

$consistencyPolicy = @{
    "defaultConsistencyLevel"="BoundedStaleness";
    "maxIntervalInSeconds"=300;
    "maxStalenessPrefix"=100000
}

$CosmosDBProperties = @{
    "databaseAccountOfferType"="Standard";
    "locations"=$locations;
    "consistencyPolicy"=$consistencyPolicy;
    "enableMultipleWriteLocations"="false"
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName -Location $location `
    -Name $accountName -PropertyObject $CosmosDBProperties
```

* `$accountName`: der Name des Azure Cosmos DB-Kontos. Dieser Wert darf nur alphanumerische Kleinbuchstaben und das Zeichen „-“ enthalten und muss zwischen drei und 31 Zeichen lang sein.
* `$location`: der Standort für die Azure Cosmos DB-Kontoressource.
* `$locations`: die Replikatregionen für das Datenbankkonto. Es muss für jedes Datenbankkonto eine Schreibregion mit dem Failoverprioritätswert 0 geben.
* `$consistencyPolicy`: die Standardkonsistenzebene des Azure Cosmos DB-Kontos. Weitere Informationen finden Sie unter [Konsistenzebenen in Azure Cosmos DB](consistency-levels.md).
* `$CosmosDBProperties`: die Eigenschaftswerte, die dem Azure Resource Manager-Anbieter von Cosmos DB zum Bereitstellen des Kontos übergeben werden.

Azure Cosmos DB-Konten können sowohl einer IP-Firewall als auch mit Dienstendpunkten eines virtuellen Netzwerks konfiguriert werden. Informationen zum Konfigurieren der IP-Firewall für Azure Cosmos DB finden Sie unter [Konfigurieren der IP-Firewall](how-to-configure-firewall.md).  Weitere Informationen zum Aktivieren von Dienstendpunkten für Azure Cosmos DB finden Sie unter [Konfigurieren des Zugriffs über virtuelle Netzwerke](how-to-configure-vnet-service-endpoint.md).

### <a id="list-accounts"></a> Auflisten aller Azure Cosmos DB-Konten in einem Abonnement

Mit dem folgenden Befehl können Sie alle Azure Cosmos-Konten in einem Abonnement auflisten:

```azurepowershell-interactive
# List Azure Cosmos Accounts

Get-AzResource -ResourceType Microsoft.DocumentDb/databaseAccounts | ft
```

### <a id="get-account"></a> Abrufen der Eigenschaften eines Azure Cosmos DB-Kontos

Mit diesem Befehl können Sie die Eigenschaften eines Azure Cosmos DB-Kontos abrufen.

```azurepowershell-interactive
# Get the properties of an Azure Cosmos Account

$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName | Select-Object Properties
```

### <a id="update-account"></a> Aktualisieren eines Azure Cosmos DB-Kontos

Mit diesem Befehl können Sie die Eigenschaften Ihres Azure Cosmos-Datenbankkontos aktualisieren. Die Eigenschaften für folgende Aufgaben können aktualisiert werden:

* Hinzufügen oder Entfernen von Regionen
* Anpassen der Standardkonsistenzrichtlinie
* Anpassen des IP-Bereichsfilters
* Anpassen von Konfigurationen für das virtuelle Netzwerk
* Aktivieren der Multimasterarchitektur

> [!NOTE]
> Sie können nicht gleichzeitig Regionselemente vom Typ `locations` hinzufügen oder entfernen und andere Eigenschaften für ein Azure Cosmos-Konto ändern. Die Anpassung von Regionen muss getrennt von anderen Änderungen an der Kontoressource vorgenommen werden.
> [!NOTE]
> Mit diesem Befehl können Sie Regionen hinzufügen und entfernen, aber weder Failoverprioritäten anpassen noch ein manuelles Failover auslösen. Weitere Informationen finden Sie unter [Anpassen der Failoverpriorität für ein Azure Cosmos DB-Konto](#modify-failover-priority) und [Auslösen eines manuellen Failovers](#trigger-manual-failover).

```azurepowershell-interactive
# Create an account with 2 regions
$resourceGroupName = "myResourceGroup"
$resourceType = "Microsoft.DocumentDb/databaseAccounts"
$accountName = "mycosmosaccount" # must be lower case and < 31 characters

$locations = @(
    @{ "locationName"="West US 2"; "failoverPriority"=0, "isZoneRedundant"=false },
    @{ "locationName"="East US 2"; "failoverPriority"=1, "isZoneRedundant"=false }
)
$consistencyPolicy = @{ "defaultConsistencyLevel"="Session" }
$CosmosDBProperties = @{
    "databaseAccountOfferType"="Standard";
    "locations"=$locations;
    "consistencyPolicy"=$consistencyPolicy
}
New-AzResource -ResourceType $resourceType `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName -PropertyObject $CosmosDBProperties

# Add a region
$account = Get-AzResource -ResourceType $resourceType `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName

$locations = @(
    @{ "locationName"="West US 2"; "failoverPriority"=0, "isZoneRedundant"=false },
    @{ "locationName"="East US 2"; "failoverPriority"=1, "isZoneRedundant"=false },
    @{ "locationName"="South Central US"; "failoverPriority"=2, "isZoneRedundant"=false }
)

$account.Properties.locations = $locations
$CosmosDBProperties = $account.Properties

Set-AzResource -ResourceType $resourceType `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName -PropertyObject $CosmosDBProperties

# Azure Resource Manager does not wait on the resource update
Write-Host "Confirm region added before continuing..."

# Remove a region
$account = Get-AzResource -ResourceType $resourceType `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName

$locations = @(
    @{ "locationName"="West US 2"; "failoverPriority"=0, "isZoneRedundant"=false },
    @{ "locationName"="East US 2"; "failoverPriority"=1, "isZoneRedundant"=false }
)

$account.Properties.locations = $locations
$CosmosDBProperties = $account.Properties

Set-AzResource -ResourceType $resourceType `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName -PropertyObject $CosmosDBProperties
```
### <a id="multi-master"></a> Aktivieren mehrerer Schreibregionen für ein Azure Cosmos-Konto

```azurepowershell-interactive
# Update an Azure Cosmos account from single to multi-master
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$resourceType = "Microsoft.DocumentDb/databaseAccounts"

$account = Get-AzResource -ResourceType $resourceType `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName

$account.Properties.enableMultipleWriteLocations = "true"
$CosmosDBProperties = $account.Properties

Set-AzResource -ResourceType $resourceType `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName -PropertyObject $CosmosDBProperties
```

### <a id="delete-account"></a> Löschen eines Azure Cosmos DB-Kontos

Mit diesem Befehl können Sie ein Azure Cosmos DB-Konto löschen.

```azurepowershell-interactive
# Delete an Azure Cosmos Account
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

Remove-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName
```

### <a id="update-tags"></a> Aktualisieren der Tags eines Azure Cosmos DB-Kontos

Im folgenden Beispiel wird beschrieben, wie Sie [Azure-Ressourcentags][azure-resource-tags] für ein Azure Cosmos DB-Konto festlegen.

> [!NOTE]
> Sie können diesen Befehl mit den Befehlen zum Erstellen oder Aktualisieren kombinieren, indem Sie das `-Tags`-Flag mit dem entsprechenden Parameter anfügen.

```azurepowershell-interactive
# Update tags for an Azure Cosmos Account

$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

$tags = @{
    "dept" = "Finance";
    "environment" = "Production"
}

Set-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName -Tags $tags
```

### <a id="list-keys"></a> Auflisten von Kontoschlüsseln

Wenn Sie ein Azure Cosmos DB-Konto erstellen, generiert der Dienst zwei Hauptzugriffsschlüssel, die für die Authentifizierung verwendet werden können, wenn der Zugriff auf das Azure Cosmos DB-Konto erfolgt. Durch Bereitstellen von zwei Zugriffsschlüsseln ermöglicht Azure Cosmos DB Ihnen das erneute Generieren der Schlüssel ohne Unterbrechung des Zugriffs auf das Azure Cosmos DB-Konto. Schreibgeschützte Schlüssel für die Authentifizierung von schreibgeschützten Vorgängen sind ebenfalls verfügbar. Es gibt zwei Lese-Schreib-Schlüssel (primär und sekundär) und zwei schreibgeschützte Schlüssel (primär und sekundär).

```azurepowershell-interactive
# List keys for an Azure Cosmos Account

$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

$keys = Invoke-AzResourceAction -Action listKeys `
    -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" `
    -ResourceGroupName $resourceGroupName -Name $accountName

Write-Host "PrimaryKey =" $keys.primaryMasterKey
Write-Host "SecondaryKey =" $keys.secondaryMasterKey
```

### <a id="list-connection-strings"></a>: Verbindungszeichenfolgen auflisten

Für MongoDB-Konten kann die Verbindungszeichenfolge zum Herstellen einer Verbindung Ihrer MongoDB-App mit dem Datenbankkonto mit dem folgenden Befehl abgerufen werden.

```azurepowershell-interactive
# List connection strings for an Azure Cosmos Account

$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

$keys = Invoke-AzResourceAction -Action listConnectionStrings `
    -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" `
    -ResourceGroupName $resourceGroupName -Name $accountName

Select-Object $keys
```

### <a id="regenerate-keys"></a> Erneutes Generieren von Kontoschlüsseln

Zugriffsschlüssel für ein Azure Cosmos DB-Konto sollten in regelmäßigen Abständen erneut generiert werden, um die Sicherheit von Verbindungen langfristig zu erhöhen. Dem Konto werden ein primärer und ein sekundärer Zugriffsschlüssel zugewiesen. Dadurch wird ein anhaltender Clientzugriff sichergestellt, wenn einer der beiden Schlüssel erneut generiert wird. Für Azure Cosmos DB-Konten werden vier Schlüsselarten verwendet: Primär- und Sekundärschlüssel sowie PrimaryReadonly- und SecondaryReadonly-Schlüssel.

```azurepowershell-interactive
# Regenerate the primary key for an Azure Cosmos Account

$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

$keyKind = @{ "keyKind"="Primary" }

$keys = Invoke-AzResourceAction -Action regenerateKey `
    -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" `
    -ResourceGroupName $resourceGroupName -Name $accountName -Parameters $keyKind

Select-Object $keys
```

### <a id="enable-automatic-failover"></a> Aktivieren des automatischen Failovers

Ermöglicht einem Cosmos-Konto das Failover auf die sekundäre Region, wenn die primäre Region nicht mehr verfügbar ist.

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$resourceType = "Microsoft.DocumentDb/databaseAccounts"

$account = Get-AzResource -ResourceType $resourceType `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName

$account.Properties.enableAutomaticFailover="true";
$CosmosDBProperties = $account.Properties;

Set-AzResource -ResourceType $resourceType `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName -PropertyObject $CosmosDBProperties
```

### <a id="modify-failover-priority"></a> Anpassen der Failoverpriorität für ein Azure Cosmos DB-Konto

Bei Konten, für die automatisches Failover konfiguriert ist, können Sie die Reihenfolge ändern, in der Cosmos sekundäre Replikate auf primäre Replikate höher stuft, sollten die primären Replikate nicht mehr verfügbar sein.

Bei dem folgenden Beispiel wird von der aktuellen Failoverpriorität ausgegangen: `West US 2 = 0`, `East US 2 = 1`, `South Central US = 2`.

> [!CAUTION]
> Wenn `locationName` in `failoverPriority=0` geändert wird, wird ein manuelles Failover für ein Azure Cosmos DB-Konto ausgelöst. Bei anderen Prioritätsänderungen wird kein Failover ausgelöst.

```azurepowershell-interactive
# Change the failover priority for an Azure Cosmos Account
# Assume existing priority is "West US 2" = 0, "East US 2" = 1, "South Central US" = 2

$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

$failoverRegions = @(
    @{ "locationName"="West US 2"; "failoverPriority"=0 },
    @{ "locationName"="South Central US"; "failoverPriority"=1 },
    @{ "locationName"="East US 2"; "failoverPriority"=2 }
)

$failoverPolicies = @{
    "failoverPolicies"= $failoverRegions
}

Invoke-AzResourceAction -Action failoverPriorityChange `
    -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" `
    -ResourceGroupName $resourceGroupName -Name $accountName -Parameters $failoverPolicies
```

### <a id="trigger-manual-failover"></a> Auslösen eines manuellen Failovers

Bei Konten, für die manuelles Failover konfiguriert ist, können Sie ein Failover ausführen und sekundäre Replikate auf primäre Replikate höher stufen, indem Sie `failoverPriority=0` festlegen. Mit diesem Vorgang kann ein Notfallwiederherstellungsverfahren initiiert werden, um die Notfallwiederherstellungsplanung zu testen.

Gehen Sie im folgenden Beispiel davon aus, dass für das Konto aktuell die Failoverprioritäten `West US 2 = 0` und `East US 2 = 1` festgelegt sind und die Regionen vertauscht werden.

> [!CAUTION]
> Wenn `locationName` in `failoverPriority=0` geändert wird, wird ein manuelles Failover für ein Azure Cosmos DB-Konto ausgelöst. Bei anderen Prioritätsänderungen wird kein Failover ausgelöst.

```azurepowershell-interactive
# Change the failover priority for an Azure Cosmos Account
# Assume existing priority is "West US 2" = 0, "East US 2" = 1, "South Central US" = 2

$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

$failoverRegions = @(
    @{ "locationName"="South Central US"; "failoverPriority"=0 },
    @{ "locationName"="East US 2"; "failoverPriority"=1 },
    @{ "locationName"="West US 2"; "failoverPriority"=2 }
)

$failoverPolicies = @{
    "failoverPolicies"= $failoverRegions
}

Invoke-AzResourceAction -Action failoverPriorityChange `
    -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" `
    -ResourceGroupName $resourceGroupName -Name $accountName -Parameters $failoverPolicies
```

## <a name="azure-cosmos-database"></a>Azure Cosmos DB-Datenbank

In den folgenden Abschnitten erfahren Sie, wie Sie Azure Cosmos DB-Datenbanken verwalten. Dabei lernen Sie u. a. Folgendes:

* [Erstellen einer Azure Cosmos DB-Datenbank](#create-db)
* [Erstellen einer Azure Cosmos DB-Datenbank mit gemeinsam genutzten Durchsatz](#create-db-ru)
* [Abrufen des Durchsatzes einer Azure Cosmos DB-Datenbank](#get-db-ru)
* [Abrufen aller Azure Cosmos DB-Datenbanken eines Kontos](#list-db)
* [Abrufen einer einzelnen Azure Cosmos DB-Datenbank](#get-db)
* [Löschen einer Azure Cosmos DB-Datenbank](#delete-db)

### <a id="create-db"></a>Erstellen einer Azure Cosmos DB-Datenbank

```azurepowershell-interactive
# Create an Azure Cosmos database
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$resourceName = $accountName + "/sql/" + $databaseName

$DataBaseProperties = @{
    "resource"=@{"id"=$databaseName}
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $DataBaseProperties
```

### <a id="create-db-ru"></a>Erstellen einer Azure Cosmos DB-Datenbank mit gemeinsam genutzten Durchsatz

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database2"
$resourceName = $accountName + "/sql/" + $databaseName

$DataBaseProperties = @{
    "resource"=@{ "id"=$databaseName };
    "options"=@{ "Throughput"="400" }
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $DataBaseProperties
```

### <a id="get-db-ru"></a>Abrufen des Durchsatzes einer Azure Cosmos DB-Datenbank

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container1"
$databaseThroughputResourceType = "Microsoft.DocumentDb/databaseAccounts/apis/databases/settings"
$databaseThroughputResourceName = $accountName + "/sql/" + $databaseName + "/throughput"

Get-AzResource -ResourceType $databaseThroughputResourceType `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $databaseThroughputResourceName  | Select-Object Properties
```

### <a id="list-db"></a>Abrufen aller Azure Cosmos DB-Datenbanken eines Kontos

```azurepowershell-interactive
# Get all databases in an Azure Cosmos account
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$resourceName = $accountName + "/sql/"

Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName  | Select-Object Properties
```

### <a id="get-db"></a>Abrufen einer einzelnen Azure Cosmos DB-Datenbank

```azurepowershell-interactive
# Get a single database in an Azure Cosmos account
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$resourceName = $accountName + "/sql/" + $databaseName

Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName | Select-Object Properties
```

### <a id="delete-db"></a>Löschen einer Azure Cosmos DB-Datenbank

```azurepowershell-interactive
# Delete a database in an Azure Cosmos account
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$resourceName = $accountName + "/sql/" + $databaseName
Remove-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName -Name $resourceName
```

## <a name="azure-cosmos-container"></a>Azure Cosmos DB-Container

In den folgenden Abschnitten erfahren Sie, wie Sie Azure Cosmos DB-Container verwalten. Dabei lernen Sie u. a. Folgendes:

* [Erstellen eines Containers in Azure Cosmos DB](#create-container)
* [Erstellen eines Azure Cosmos DB-Containers mit einem großen Partitionsschlüssel](#create-container-big-pk)
* [Abrufen des Durchsatzes eines Azure Cosmos DB-Containers](#get-container-ru)
* [Erstellen eines Azure Cosmos DB-Containers mit gemeinsam genutzten Durchsatz](#create-container-ru)
* [Erstellen eines Azure Cosmos DB-Containers mit benutzerdefinierter Indizierungsrichtlinie](#create-container-custom-index)
* [Erstellen eines Azure Cosmos DB-Containers mit deaktivierter Indizierung](#create-container-no-index)
* [Erstellen eines Azure Cosmos DB-Containers mit einer Richtlinie für einen eindeutigen Schlüssel und mit einer Gültigkeitsdauer](#create-container-unique-key-ttl)
* [Erstellen eines Azure Cosmos DB-Containers mit Konfliktauflösung](#create-container-lww)
* [Abrufen aller Azure Cosmos DB-Container einer Datenbank](#list-containers)
* [Abrufen eines einzelnen Azure Cosmos DB-Containers einer Datenbank](#get-container)
* [Löschen eines Azure Cosmos DB-Containers](#delete-container)

### <a id="create-container"></a>Erstellen eines Azure Cosmos DB-Containers

```azurepowershell-interactive
# Create an Azure Cosmos container with default indexes and throughput at 400 RU
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container1"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

$ContainerProperties = @{
    "resource"=@{
        "id"=$containerName;
        "partitionKey"=@{
            "paths"=@("/myPartitionKey");
            "kind"="Hash"
        }
    };
    "options"=@{ "Throughput"="400" }
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $ContainerProperties
```

### <a id="create-container-big-pk"></a>Erstellen eines Azure Cosmos DB-Containers mit einem großen Partitionsschlüssel

```azurepowershell-interactive
# Create an Azure Cosmos container with a large partition key value (version = 2)
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container1"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

$ContainerProperties = @{
    "resource"=@{
        "id"=$containerName;
        "partitionKey"=@{
            "paths"=@("/myPartitionKey");
            "kind"="Hash";
            "version" = 2
        }
    };
    "options"=@{ "Throughput"="400" }
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $ContainerProperties
```

### <a id="get-container-ru"></a>Abrufen des Durchsatzes eines Azure Cosmos DB-Containers

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container1"
$containerThroughputResourceType = "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers/settings"
$containerThroughputResourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName + "/throughput"

Get-AzResource -ResourceType $containerThroughputResourceType `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $containerThroughputResourceName  | Select-Object Properties
```

### <a id="create-container-ru"></a>Erstellen eines Azure Cosmos DB-Containers mit gemeinsam genutzten Durchsatz

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container2"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

$ContainerProperties = @{
    "resource"=@{
        "id"=$containerName; 
        "partitionKey"=@{
            "paths"=@("/myPartitionKey");
            "kind"="Hash"
        }
    };
    "options"=@{}
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $ContainerProperties 
```

### <a id="create-container-custom-index"></a>Erstellen eines Azure Cosmos DB-Containers mit benutzerdefinierter Indizierungsrichtlinie

```azurepowershell-interactive
# Create a container with a custom indexing policy
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container3"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

$ContainerProperties = @{
    "resource"=@{
        "id"=$containerName; 
        "partitionKey"=@{
            "paths"=@("/myPartitionKey"); 
            "kind"="Hash"
        }; 
        "indexingPolicy"=@{
            "indexingMode"="Consistent";
            "includedPaths"= @(@{
                "path"="/*";
                "indexes"= @()
            });
            "excludedPaths"= @(@{
                "path"="/myPathToNotIndex/*"
            })
        }
    };
    "options"=@{ "Throughput"="400" }
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $ContainerProperties
```

### <a id="create-container-no-index"></a>Erstellen eines Azure Cosmos DB-Containers mit deaktivierter Indizierung

```azurepowershell-interactive
# Create an Azure Cosmos container with no indexing
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container4"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

$ContainerProperties = @{
    "resource"=@{
        "id"=$containerName; 
        "partitionKey"=@{
            "paths"=@("/myPartitionKey");
            "kind"="Hash"
        };
        "indexingPolicy"=@{
            "indexingMode"="none"
        }
    };
    "options"=@{ "Throughput"="400" }
} 

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $ContainerProperties
```

### <a id="create-container-unique-key-ttl"></a>Erstellen eines Azure Cosmos DB-Containers mit einer Richtlinie für einen eindeutigen Schlüssel und mit einer Gültigkeitsdauer

```azurepowershell-interactive
# Create a container with a unique key policy and TTL of one day
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container5"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

$ContainerProperties = @{
    "resource"=@{
        "id"=$containerName;
        "partitionKey"=@{
            "paths"=@("/myPartitionKey");
            "kind"="Hash"
        }; 
        "indexingPolicy"=@{
            "indexingMode"="Consistent";
            "includedPaths"= @(@{
                "path"="/*";
                "indexes"= @()
            });
            "excludedPaths"= @()
        };
        "uniqueKeyPolicy"= @{
            "uniqueKeys"= @(@{
                "paths"= @(
                    "/myUniqueKey1";
                    "/myUniqueKey2"
                )
            })
        };
        "defaultTtl"= 86400;
    };
    "options"=@{ "Throughput"="400" }
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $ContainerProperties
```

### <a id="create-container-lww"></a>Erstellen eines Azure Cosmos DB-Containers mit Konfliktauflösung

Wenn Sie eine gespeicherte Prozedur verwenden möchten, können Sie eine Konfliktauflösungsrichtlinie erstellen. Legen Sie dazu `"mode"="custom"` und den Auflösungspfad als Name der gespeicherten Prozedur (`"conflictResolutionPath"="myResolverStoredProcedure"`) fest. Legen Sie `"mode"="custom"` und `"conflictResolutionPath"=""` fest, um alle Konflikte in „ConflictsFeed“ zu schreiben und diese getrennt zu behandeln.

```azurepowershell-interactive
# Create container with last-writer-wins conflict resolution policy
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container6"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

$ContainerProperties = @{
    "resource"=@{
        "id"=$containerName;
        "partitionKey"=@{
            "paths"=@("/myPartitionKey");
            "kind"="Hash"
        };
        "conflictResolutionPolicy"=@{
            "mode"="lastWriterWins";
            "conflictResolutionPath"="/myResolutionPath"
        }
    };
    "options"=@{ "Throughput"="400" }
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $ContainerProperties
```

### <a id="list-containers"></a>Abrufen aller Azure Cosmos DB-Container einer Datenbank

```azurepowershell-interactive
# List all Azure Cosmos containers in a database
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$resourceName = $accountName + "/sql/" + $databaseName

Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName | Select-Object Properties
```

### <a id="get-container"></a>Abrufen eines einzelnen Azure Cosmos DB-Containers einer Datenbank

```azurepowershell-interactive
# Get a single Azure Cosmos container in a database
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container5"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName | Select-Object Properties
```

### <a id="delete-container"></a>Löschen eines Azure Cosmos DB-Containers

```azurepowershell-interactive
# Delete an Azure Cosmos container
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container1"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

Remove-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName -Name $resourceName
```

## <a name="next-steps"></a>Nächste Schritte

* [PowerShell-Beispiele](powershell-samples.md)
* [Verwalten eines Azure Cosmos DB-Kontos](how-to-manage-database-account.md)
* [Erstellen eines Containers in Azure Cosmos DB](how-to-create-container.md)
* [Configure time-to-live in Azure Cosmos DB (Konfigurieren der Gültigkeitsdauer in Azure Cosmos DB)](how-to-time-to-live.md)

<!--Reference style links - using these makes the source content way more readable than using inline links-->

[powershell-install-configure]: https://docs.microsoft.com/azure/powershell-install-configure
[scaling-globally]: distribute-data-globally.md#EnableGlobalDistribution
[distribute-data-globally]: distribute-data-globally.md
[azure-resource-groups]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups
[azure-resource-tags]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags
[rp-rest-api]: https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/
