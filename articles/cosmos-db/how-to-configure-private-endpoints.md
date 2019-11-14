---
title: Konfigurieren von Azure Private Link für ein Azure Cosmos-Konto
description: Erfahren Sie, wie Sie Azure Private Link für den Zugriff auf ein Azure Cosmos-Konto über eine private IP-Adresse in einem virtuellen Netzwerk einrichten.
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: thweiss
ms.openlocfilehash: 34b54459629560ba80e6a38d10edbab32ea44778
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73820156"
---
# <a name="configure-azure-private-link-for-an-azure-cosmos-account-preview"></a>Konfigurieren von Azure Private Link für ein Azure Cosmos-Konto (Vorschau)

Durch Verwendung von Azure Private Link können Sie eine Verbindung mit einem Azure Cosmos-Konto über einen privaten Endpunkt herstellen. Bei einem privaten Endpunkt handelt es sich um eine Gruppe privater IP-Adressen in einem Subnetz innerhalb Ihres virtuellen Netzwerks. Mithilfe von Private Link können Sie den Zugriff auf ein bestimmtes Azure Cosmos-Konto über private IP-Adressen einschränken. Durch die Kombination von Private Link und eingeschränkten NSG-Richtlinien kann das Risiko der Datenexfiltration verringert werden. Weitere Informationen zu privaten Endpunkten finden Sie im Artikel zu [Azure Private Link](../private-link/private-link-overview.md).

Außerdem kann bei Verwendung von Private Link innerhalb des virtuellen Netzwerks oder über alle mittels Peering verbundenen Netzwerke auf ein Azure Cosmos-Konto zugegriffen werden. Auf Ressourcen, die Private Link zugeordnet sind, kann auch lokal über privates Peering über VPN oder ExpressRoute zugegriffen werden. 

Durch Verwendung der Genehmigungsmethoden „Automatisch“ oder „Manuell“ können Sie eine Verbindung mit einem Azure Cosmos-Konto herstellen, das über Private Link konfiguriert wurde. Weitere Informationen finden Sie im Abschnitt zum [Genehmigungsworkflow](../private-link/private-endpoint-overview.md#access-to-a-private-link-resource-using-approval-workflow) in der Dokumentation zu Private Link. In diesem Artikel werden die Schritte zum Erstellen einer privaten Verknüpfung unter Verwendung der Genehmigungsmethode „Automatisch“ beschrieben.

## <a name="create-a-private-link-using-the-azure-portal"></a>Erstellen einer privaten Verknüpfung über das Azure-Portal

Führen Sie die folgenden Schritte aus, um im Azure-Portal eine private Verknüpfung für ein vorhandenes Azure Cosmos-Konto zu erstellen:

1. Suchen Sie im Bereich **Alle Ressourcen** das Azure Cosmos DB-Konto, das Sie schützen möchten.

1. Wählen Sie im Menü „Einstellungen“ die Option **Private Endpunktverbindung** und dann die Option **Privater Endpunkt** aus:

   ![Erstellen eines privaten Endpunkts über das Azure-Portal](./media/how-to-configure-private-endpoints/create-private-endpoint-portal.png)

1. Geben Sie im Bereich **Privaten Endpunkt erstellen (Vorschau) – Grundlagen** folgende Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | **Projektdetails** | |
    | Subscription | Wählen Sie Ihr Abonnement aus. |
    | Resource group | Wählen Sie eine Ressourcengruppe aus.|
    | **Instanzendetails** |  |
    | NAME | Geben Sie einen Namen für den privaten Endpunkt ein. Wenn der Name bereits vergeben ist, erstellen Sie einen eindeutigen Namen. |
    |Region| Wählen Sie die Region aus, in der Sie die private Verknüpfung bereitstellen möchten. Der private Endpunkt muss am gleichen Standort erstellt werden, an dem sich das virtuelle Netzwerk befindet.|
    |||
1. Klicken Sie auf **Weiter: Ressource** aus.
1. Geben Sie unter **Privaten Endpunkt erstellen – Ressource** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    |Verbindungsmethode  | Wählen Sie das Herstellen einer Verbindung mit einer Azure-Ressource im eigenen Verzeichnis aus. <br/><br/> Über diese Option können Sie eine Ihrer Ressourcen zum Einrichten einer privaten Verknüpfung auswählen oder mit einer freigegebenen Ressourcen-ID oder einem freigegebenen Alias eine Verbindung mit der Ressource eines anderen Benutzers herstellen.|
    | Subscription| Wählen Sie Ihr Abonnement aus. |
    | Ressourcentyp | Wählen Sie **Microsoft.AzureCosmosDB/databaseAccounts** aus. |
    | Resource |Wählen Sie Ihr Azure Cosmos-Konto aus. |
    |Zielunterressource |Wählen Sie den Typ der zuzuordnenden Cosmos DB-API aus. Standardmäßig ist für die SQL-, MongoDB- und Cassandra-API jeweils nur eine Auswahl möglich. Für die Gremlin- und die Tabellen-API können Sie auch *SQL* auswählen, da diese APIs mit der SQL-API interoperabel sind. |
    |||

1. Klicken Sie auf **Weiter: Konfiguration** aus.
1. Geben Sie unter **Privaten Endpunkt erstellen (Vorschau) – Konfiguration** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    |**Netzwerk**| |
    | Virtuelles Netzwerk| Wählen Sie Ihr virtuelles Netzwerk aus. |
    | Subnet | Wählen Sie das Subnetz aus. |
    |**Private DNS-Integration**||
    |Integration in eine private DNS-Zone |Wählen Sie **Ja** aus. <br><br/> Für die Herstellung einer privaten Verbindung mit Ihrem privaten Endpunkt benötigen Sie einen DNS-Eintrag. Es empfiehlt sich, den privaten Endpunkt in eine private DNS-Zone zu integrieren. Sie können auch Ihre eigenen DNS-Server verwenden oder DNS-Einträge mithilfe der Hostdateien auf Ihren virtuellen Computern erstellen. |
    |Private DNS-Zone |Wählen Sie *privatelink.documents.azure.com* aus. <br><br/> Die private DNS-Zone wird automatisch bestimmt und kann aktuell nicht über das Azure-Portal geändert werden.|
    |||

1. Klicken Sie auf **Überprüfen + erstellen**. Sie werden zur Seite **Überprüfen und erstellen** weitergeleitet, auf der Azure Ihre Konfiguration überprüft.
1. Wenn die Meldung **Überprüfung erfolgreich** angezeigt wird, wählen Sie **Erstellen** aus.

Wenn Sie private Verknüpfungen für ein Azure Cosmos-Konto genehmigt haben, ist die Option **Alle Netzwerke** im Bereich **Firewall und virtuelle Netzwerke** im Azure-Portal ausgegraut.

Die folgende Tabelle veranschaulicht die Zuordnung zwischen verschiedenen Arten von Azure Cosmos-Konto-APIs, unterstützten Unterressourcen und entsprechenden Namen für die private Zone. Da auf das Gremlin- und das Tabellen-API-Konto auch über die SQL-API zugegriffen werden kann, sind für diese APIs zwei Einträge vorhanden:

|Art der Azure Cosmos-Konto-API  |Unterstützte Unterressourcen (oder Gruppen-IDs) |Name der privaten Zone  |
|---------|---------|---------|
|Sql    |   Sql      | privatelink.documents.azure.com   |
|Cassandra    | Cassandra        |  privatelink.cassandra.cosmos.azure.com    |
|Mongo   |  MongoDB       |  privatelink.mongo.cosmos.azure.com    |
|Gremlin     | Gremlin        |  privatelink.gremlin.cosmos.azure.com   |
|Gremlin     |  Sql       |  privatelink.documents.azure.com    |
|Table    |    Table     |   privatelink.table.cosmos.azure.com    |
|Table     |   Sql      |  privatelink.documents.azure.com    |

### <a name="fetch-the-private-ip-addresses"></a>Abrufen der privaten IP-Adressen

Nach der Bereitstellung des privaten Endpunkts können Sie die IP-Adressen abfragen. Zum Anzeigen der IP-Adressen im Azure-Portal wählen Sie **Alle Ressourcen** aus, suchen Sie den zuvor erstellten privaten Endpunkt (in diesem Fall „dbPrivateEndpoint3“), und wählen Sie die Registerkarte „Übersicht“ aus, um die DNS-Einstellungen und IP-Adressen anzuzeigen:

![Private IP-Adressen im Azure-Portal](./media/how-to-configure-private-endpoints/private-ip-addresses-portal.png)

Für jeden privaten Endpunkt werden mehrere IP-Adressen erstellt:

* Eine IP-Adresse für den globalen (regionsunabhängig) Endpunkt des Azure Cosmos-Kontos
* Eine IP-Adresse für jede Region, in der das Azure Cosmos-Konto bereitgestellt wird

## <a name="create-a-private-link-using-azure-powershell"></a>Erstellen einer privaten Verknüpfung mithilfe von Azure PowerShell

Führen Sie das folgende PowerShell-Skript aus, um die private Verknüpfung „MyPrivateEndpoint“ für ein vorhandenes Azure Cosmos-Konto zu erstellen. Ersetzen Sie die Variablenwerte durch die für Ihre Umgebung spezifischen Angaben.

```azurepowershell-interactive
Fill in these details, make sure to replace the variable values with the details specific to your environment.
$SubscriptionId = "<your Azure subscription ID>"
# Resource group where the Cosmos DB and VNet resources live
$ResourceGroupName = "myResourceGroup"
# Name of the Cosmos DB account
$CosmosDbAccountName = "mycosmosaccount"

# API type of the Cosmos DB account: Sql or MongoDB or Cassandra or Gremlin or Table
$CosmosDbApiType = "Sql"
# Name of the existing VNet
$VNetName = "myVnet"
# Name of the target subnet in the VNet
$SubnetName = "mySubnet"
# Name of the private endpoint to create
$PrivateEndpointName = "MyPrivateEndpoint"
# Location where the private endpoint can be created. The private endpoint should be created in the same location where your subnet or the virtual network exists
$Location = "westcentralus"

$cosmosDbResourceId = "/subscriptions/$($SubscriptionId)/resourceGroups/$($ResourceGroupName)/providers/Microsoft.DocumentDB/databaseAccounts/$($CosmosDbAccountName)"

$privateEndpointConnection = New-AzPrivateLinkServiceConnection -Name "myConnectionPS" -PrivateLinkServiceId $cosmosDbResourceId -GroupId $CosmosDbApiType
 
$virtualNetwork = Get-AzVirtualNetwork -ResourceGroupName  $ResourceGroupName -Name $VNetName  
 
$subnet = $virtualNetwork | Select -ExpandProperty subnets | Where-Object  {$_.Name -eq $SubnetName}  
 
$privateEndpoint = New-AzPrivateEndpoint -ResourceGroupName $ResourceGroupName -Name $PrivateEndpointName -Location "westcentralus" -Subnet  $subnet -PrivateLinkServiceConnection $privateEndpointConnection
```

### <a name="integrate-the-private-endpoint-with-private-dns-zone"></a>Integrieren des privaten Endpunkts in eine private DNS-Zone

Nachdem Sie den privaten Endpunkt erstellt haben, können Sie ihn mithilfe des folgenden PowerShell-Skripts in eine private DNS-Zone integrieren:

```azurepowershell-interactive
Import-Module Az.PrivateDns
$zoneName = "privatelink.documents.azure.com"
$zone = New-AzPrivateDnsZone -ResourceGroupName $ResourceGroupName `
  -Name $zoneName

$link  = New-AzPrivateDnsVirtualNetworkLink -ResourceGroupName $ResourceGroupName `
  -ZoneName $zoneName `
  -Name "myzonelink" `
  -VirtualNetworkId $virtualNetwork.Id  
 
$pe = Get-AzPrivateEndpoint -Name $PrivateEndpointName `
  -ResourceGroupName $ResourceGroupName

$networkInterface = Get-AzResource -ResourceId $pe.NetworkInterfaces[0].Id `
  -ApiVersion "2019-04-01"
 
foreach ($ipconfig in $networkInterface.properties.ipConfigurations) { 
foreach ($fqdn in $ipconfig.properties.privateLinkConnectionProperties.fqdns) { 
Write-Host "$($ipconfig.properties.privateIPAddress) $($fqdn)"  
$recordName = $fqdn.split('.',2)[0] 
$dnsZone = $fqdn.split('.',2)[1] 
New-AzPrivateDnsRecordSet -Name $recordName `
  -RecordType A -ZoneName $zoneName  `
  -ResourceGroupName $ResourceGroupName -Ttl 600 `
  -PrivateDnsRecords (New-AzPrivateDnsRecordConfig `
  -IPv4Address $ipconfig.properties.privateIPAddress)  
}
}
```

### <a name="fetch-the-private-ip-addresses"></a>Abrufen der privaten IP-Adressen

Nach der Bereitstellung des privaten Endpunkts können Sie mit dem folgenden PowerShell-Skript die IP-Adressen und die FQDN-Zuordnung abfragen:

```azurepowershell-interactive

$pe = Get-AzPrivateEndpoint -Name MyPrivateEndpoint -ResourceGroupName myResourceGroup
$networkInterface = Get-AzNetworkInterface -ResourceId $pe.NetworkInterfaces[0].Id
foreach ($IPConfiguration in $networkInterface.IpConfigurations)
{
    Write-Host $IPConfiguration.PrivateIpAddress ":" $IPConfiguration.PrivateLinkConnectionProperties.Fqdns
}
```

## <a name="create-a-private-link-using-a-resource-manager-template"></a>Erstellen einer privaten Verknüpfung mithilfe einer Resource Manager-Vorlage

Sie können eine private Verknüpfung einrichten, indem Sie einen privaten Endpunkt in einem Subnetz des virtuellen Netzwerks erstellen. Dies erfolgt mithilfe einer Azure Resource Manager-Vorlage. Erstellen Sie mit dem folgenden Code die Resource Manager-Vorlage „PrivateEndpoint_template.json“. Mit dieser Vorlage wird ein privater Endpunkt für ein vorhandenes Azure Cosmos-SQL-API-Konto in einem vorhandenen virtuellen Netzwerk erstellt:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
          "type": "string",
          "defaultValue": "[resourceGroup().location]",
          "metadata": {
            "description": "Location for all resources."
          }
        },
        "privateEndpointName": {
            "type": "string"
        },
        "resourceId": {
            "type": "string"
        },
        "groupId": {
            "type": "string"
        },
        "subnetId": {
            "type": "string"
        }
    },
    "resources": [
        {
            "name": "[parameters('privateEndpointName')]",
            "type": "Microsoft.Network/privateEndpoints",
            "apiVersion": "2019-04-01",
            "location": "[parameters('location')]",
            "properties": {
                "subnet": {
                    "id": "[parameters('subnetId')]"
                },
                "privateLinkServiceConnections": [
                    {
                        "name": "MyConnection",
                        "properties": {
                            "privateLinkServiceId": "[parameters('resourceId')]",
                            "groupIds": [ "[parameters('groupId')]" ],
                            "requestMessage": ""
                        }
                    }
                ]
            }
        }
    ],
    "outputs": {
        "privateEndpointNetworkInterface": {
          "type": "string",
          "value": "[reference(concat('Microsoft.Network/privateEndpoints/', parameters('privateEndpointName'))).networkInterfaces[0].id]"
        }
    }
}
```

### <a name="define-the-template-parameters-file"></a>Definieren der Vorlagenparameterdatei

Erstellen Sie eine Parameterdatei für die Vorlage, und nennen Sie sie „PrivateEndpoint_parameters.json“. Fügen Sie in der Parameterdatei den folgenden Code ein:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "privateEndpointName": {
            "value": ""
        },
        "resourceId": {
            "value": ""
        },
        "groupId": {
            "value": ""
        },
        "subnetId": {
            "value": ""
        }
    }
}
```

### <a name="deploy-the-template-by-using-a-powershell-script"></a>Bereitstellen der Vorlage mithilfe eines PowerShell-Skripts

Erstellen Sie als Nächstes mit dem folgenden Code ein PowerShell-Skript. Ersetzen Sie vor dem Ausführen des Skripts zunächst die Abonnement-ID, den Ressourcengruppennamen und andere Variablenwerte durch die für Ihre Umgebung spezifischen Angaben:

```azurepowershell-interactive
### This script creates a private endpoint for an existing Cosmos DB account in an existing VNet

## Step 1: Fill in these details, make sure to replace the variable values with the details specific to your environment.
$SubscriptionId = "<your Azure subscription ID>"
# Resource group where the Cosmos DB and VNet resources live
$ResourceGroupName = "myResourceGroup"
# Name of the Cosmos DB account
$CosmosDbAccountName = "mycosmosaccount"
# API type of the Cosmos DB account. It can be one of the following: "Sql", "MongoDB", "Cassandra", "Gremlin", "Table"
$CosmosDbApiType = "Sql"
# Name of the existing VNet
$VNetName = "myVnet"
# Name of the target subnet in the VNet
$SubnetName = "mySubnet"
# Name of the private endpoint to create
$PrivateEndpointName = "myPrivateEndpoint"

$cosmosDbResourceId = "/subscriptions/$($SubscriptionId)/resourceGroups/$($ResourceGroupName)/providers/Microsoft.DocumentDB/databaseAccounts/$($CosmosDbAccountName)"
$VNetResourceId = "/subscriptions/$($SubscriptionId)/resourceGroups/$($ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($VNetName)"
$SubnetResourceId = "$($VNetResourceId)/subnets/$($SubnetName)"
$PrivateEndpointTemplateFilePath = "PrivateEndpoint_template.json"
$PrivateEndpointParametersFilePath = "PrivateEndpoint_parameters.json"

## Step 2: Sign in to your Azure account and select the target subscription.
Login-AzAccount
Select-AzSubscription -SubscriptionId $subscriptionId

## Step 3: Make sure private endpoint network policies are disabled in the subnet.
$VirtualNetwork= Get-AzVirtualNetwork -Name "$VNetName" -ResourceGroupName "$ResourceGroupName"
($virtualNetwork | Select -ExpandProperty subnets | Where-Object  {$_.Name -eq "$SubnetName"} ).PrivateEndpointNetworkPolicies = "Disabled"
$virtualNetwork | Set-AzVirtualNetwork

## Step 4: Create the private endpoint.
Write-Output "Deploying private endpoint on $($resourceGroupName)"
$deploymentOutput = New-AzResourceGroupDeployment -Name "PrivateCosmosDbEndpointDeployment" `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile $PrivateEndpointTemplateFilePath `
    -TemplateParameterFile $PrivateEndpointParametersFilePath `
    -SubnetId $SubnetResourceId `
    -ResourceId $CosmosDbResourceId `
    -GroupId $CosmosDbApiType `
    -PrivateEndpointName $PrivateEndpointName

$deploymentOutput
```

Im PowerShell-Skript kann die GroupId-Variable nur einen Wert enthalten, d. h. den API-Typ des Kontos. Zulässige Werte sind: SQL, MongoDB, Cassandra, Gremlin und Table. Einige Azure Cosmos-Kontotypen sind über mehrere APIs zugänglich. Beispiel:

* Auf ein Gremlin-API-Konto kann über Gremlin-API- sowie über SQL-API-Konten zugegriffen werden.
* Auf ein Tabellen-API-Konto kann über Tabellen-API- sowie über SQL-API-Konten zugegriffen werden.

Für diese Konten müssen Sie einen privaten Endpunkt für jeden API-Typ erstellen und den entsprechenden API-Typ im GroupId-Array angeben.

Nach der erfolgreichen Bereitstellung der Vorlage wird die Ausgabe ähnlich der Ausgabe in der folgenden Abbildung angezeigt. Der Wert für „provisioningState“ lautet „Succeeded“, wenn die privaten Endpunkte ordnungsgemäß eingerichtet sind.

![Resource Manager-Vorlage: Bereitstellungsausgabe](./media/how-to-configure-private-endpoints/resource-manager-template-deployment-output.png)

Nach der Bereitstellung der Vorlage werden die privaten IP-Adressen innerhalb des Subnetzes reserviert. Die Firewallregel für das Azure Cosmos-Konto ist so konfiguriert, dass nur Verbindungen über den privaten Endpunkt angenommen werden.

## <a name="configure-custom-dns"></a>Konfigurieren von benutzerdefiniertem DNS

Verwenden Sie möglichst ein privates DNS innerhalb des Subnetzes, in dem der private Endpunkt erstellt wurde. Konfigurieren Sie die Endpunkte außerdem so, dass jede private IP-Adresse einem DNS-Eintrag zugeordnet ist (siehe fqdns-Eigenschaft in der oben gezeigten Antwort).

Wenn Sie den privaten Endpunkt erstellen, können Sie ihn in eine private DNS-Zone in Azure integrieren. Wenn Sie Ihren privaten Endpunkt nicht in eine private DNS-Zone in Azure integrieren und stattdessen ein benutzerdefiniertes DNS verwenden, müssen Sie das DNS so konfigurieren, dass DNS-Einträge für alle privaten IP-Adressen hinzugefügt werden, die für den privaten Endpunkt reserviert sind.

## <a name="firewall-configuration-with-private-link"></a>Firewallkonfiguration mit Private Link

Es folgen unterschiedliche Fälle und Ergebnisse bei Verwendung von Private Link in Verbindung mit Firewallregeln:

* Wenn keine Firewallregeln konfiguriert sind, ist ein Azure Cosmos-Konto standardmäßig für den gesamten Datenverkehr zugänglich.

* Wenn öffentlicher Datenverkehr oder ein Dienstendpunkt konfiguriert ist und private Endpunkte erstellt werden, werden verschiedene Arten von eingehendem Datenverkehr durch den entsprechenden Typ der Firewallregel autorisiert.

* Wenn kein öffentlicher Datenverkehr oder Dienstendpunkt konfiguriert ist und private Endpunkte erstellt werden, ist das Azure Cosmos-Konto nur über die privaten Endpunkte zugänglich. Wenn kein öffentlicher Datenverkehr oder Dienstendpunkt konfiguriert ist, ist das Konto nach dem Ablehnen oder Löschen aller genehmigten privaten Endpunkte für das gesamte Netzwerk offen.

## <a name="update-private-endpoint-when-you-add-or-remove-a-region"></a>Aktualisieren des privaten Endpunkts beim Hinzufügen oder Entfernen einer Region

Beim Hinzufügen oder Entfernen von Regionen in einem Azure Cosmos-Konto müssen Sie die DNS-Einträge für dieses Konto hinzufügen oder entfernen. Diese Änderungen müssen im privaten Endpunkt entsprechend aktualisiert werden. Derzeit sollten Sie diese Änderungen manuell vornehmen, indem Sie folgende Schritte ausführen:

1. Der Azure Cosmos DB-Administrator entfernt oder fügt Regionen hinzu. Anschließend werden die Netzwerkadministratoren über die ausstehenden Änderungen benachrichtigt. Die ActionsRequired-Eigenschaften des privaten Endpunkts, der einem Azure Cosmos-Konto zugeordnet ist, werden von „None“ in „Recreate“ geändert. Der Netzwerkadministrator aktualisiert dann den privaten Endpunkt, indem er eine PUT-Anforderung mit der gleichen Resource Manager-Nutzlast ausgibt, die für die Erstellung verwendet wurde.

1. Nach diesem Vorgang muss auch das private DNS des Subnetzes aktualisiert werden, um den hinzugefügten oder entfernten DNS-Einträgen und den zugehörigen privaten IP-Adressen zu entsprechen.

Sie stellen z. B. ein Azure Cosmos-Konto in 3 Regionen bereit: „USA, Westen“, „USA, Mitte“ und „Europa, Westen“. Wenn Sie einen privaten Endpunkt für das Konto erstellen, werden 4 private IP-Adressen im Subnetz reserviert: eine für jede Region, d. h. insgesamt 3 IP-Adressen, und eine für den globalen und regionsunabhängigen Endpunkt.

Später fügen Sie dem Azure Cosmos-Konto eine neue Region hinzu, z. B. „USA, Osten“. Standardmäßig ist die neue Region nicht über den vorhandenen privaten Endpunkt zugänglich. Der Azure Cosmos-Kontoadministrator muss die Verbindung mit dem privaten Endpunkt aktualisieren, bevor der Zugriff auf die neue Region erfolgt. 

Wenn Sie den Befehl ` Get-AzPrivateEndpoint -Name <your private endpoint name> -ResourceGroupName <your resource group name>` ausführen, enthält die Ausgabe des Befehls den `actionsRequired`-Parameter, der auf „Recreate“ festgelegt ist. Dieser Wert gibt an, dass der private Endpunkt aktualisiert werden muss. Als Nächstes führt der Azure Cosmos-Kontoadministrator den Befehl `Set-AzPrivateEndpoint` aus, um die Aktualisierung des privaten Endpunkts auszulösen.

```powershell
$pe = Get-AzPrivateEndpoint -Name <your private endpoint name> -ResourceGroupName <your resource group name>

Set-AzPrivateEndpoint -PrivateEndpoint $pe
```

Eine neue private IP-Adresse wird automatisch im Subnetz unter diesem privaten Endpunkt reserviert, und der Wert `actionsRequired` wird in `None` geändert. Wenn Sie keine Integration mit privaten DNZ-Zonen verwenden (d. h., wenn Sie ein benutzerdefiniertes privates DNS verwenden), müssen Sie das private DNS so konfigurieren, dass ein neuer DNS-Eintrag für die private IP-Adresse hinzugefügt wird, die der neuen Region entspricht.

Beim Entfernen einer Region können die gleichen Schritte verwendet werden. Die private IP-Adresse der entfernten Region wird automatisch freigegeben, und das Flag `actionsRequired` wird in `None` geändert. Wenn Sie keine Integration in die private DNS-Zone verwenden, müssen Sie das private DNS so konfigurieren, dass der DNS-Eintrag für die entfernte Region entfernt wird.

DNS-Einträge in der privaten DNS-Zone werden nicht automatisch entfernt, wenn ein privater Endpunkt gelöscht oder eine Region aus dem Azure Cosmos-Konto entfernt wird. Die DNS-Einträge müssen manuell entfernt werden.

## <a name="current-limitations"></a>Aktuelle Einschränkungen

Bei Verwendung von Private Link mit einem Azure Cosmos-Konto gelten die folgenden Einschränkungen:

* Die Unterstützung von Private Link für Azure Cosmos-Konten und VNETs ist nur in bestimmten Regionen verfügbar. Eine Liste der unterstützten Regionen finden Sie im Abschnitt zu den [verfügbaren Regionen](../private-link/private-link-overview.md#availability) im Artikel zu Private Link. **VNET und Azure Cosmos-Konto müssen sich in den unterstützten Regionen befinden, damit ein privater Endpunkt erstellt werden kann.**

* Wenn Sie Private Link mit einem Azure Cosmos-Konto im direkten Verbindungsmodus verwenden, kann lediglich das TCP-Protokoll verwendet werden. Das HTTP-Protokoll wird noch nicht unterstützt.

* Bei Verwendung der Azure Cosmos DB-API für MongoDB-Konten wird der private Endpunkt nur für Konten unter Serverversion 3.6 unterstützt (d. h. Konten mit Endpunkt im Format `*.mongo.cosmos.azure.com`). Private Link wird nicht für Konten unter Serverversion 3.2 unterstützt (d. h. Konten mit Endpunkt im Format `*.documents.azure.com`). Zur Verwendung von Private Link sollten Sie alte Konten zur neuen Version migrieren.

* Bei Verwendung der Azure Cosmos DB-API für MongoDB-Konten, für die private Verknüpfungen festgelegt sind, können Sie Tools wie z. B. Robo 3T, Studio 3T oder Mongoose nicht verwenden. Für den Endpunkt ist die Unterstützung für Private Link nur möglich, wenn als Parameter „appName=<account name>“ angegeben wird. Beispiel: replicaSet=globaledb&appName=meindbkontoname. Da diese Tools den Anwendungsnamen in der Verbindungszeichenfolge nicht an den Dienst übergeben, kann Private Link nicht verwendet werden. Sie können jedoch dennoch in der Version 3.6 mit SDK-Treibern auf diese Konten zugreifen.

* Ein virtuelles Netzwerk kann nicht verschoben oder gelöscht werden, wenn es eine private Verknüpfung enthält.

* Ein Azure Cosmos-Konto kann nicht gelöscht werden, wenn es an einen privaten Endpunkt angefügt ist.

* Für ein Azure Cosmos-Konto kann kein Failover auf eine Region durchgeführt werden, wenn diese nicht allen privaten Endpunkten zugeordnet ist, die an das Konto angefügt sind. Weitere Informationen finden Sie im vorherigen Abschnitt in den Informationen zum Hinzufügen oder Entfernen von Regionen.

* Einem Netzwerkadministrator muss durch einen Administrator mindestens die Berechtigung „*/PrivateEndpointConnectionsApproval“ im Azure Cosmos-Kontobereich erteilt werden, um automatisch genehmigte private Endpunkte erstellen zu können.

### <a name="private-dns-zone-integration-limitations"></a>Einschränkungen bei der Integration für eine private DNS-Zone

DNS-Einträge in der privaten DNS-Zone werden nicht automatisch entfernt, wenn ein privater Endpunkt gelöscht oder eine Region aus dem Azure Cosmos-Konto entfernt wird. Entfernen Sie die DNS-Einträge manuell, bevor Sie einen der folgenden Schritte ausführen:

* Hinzufügen eines neuen privaten Endpunkts, der mit dieser privaten DNS-Zone verknüpft ist
* Hinzufügen einer neuen Region zu einem Datenbankkonto mit privaten Endpunkten, die mit dieser privaten DNS-Zone verknüpft sind

Ohne Bereinigung der DNS-Einträge können unerwartete Probleme mit der Datenebene auftreten. Hierzu zählen beispielsweise Datenausfälle für Regionen, die nach dem Entfernen eines privaten Endpunkts oder einer Region hinzugefügt wurden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den anderen Sicherheitsfunktionen in Azure Cosmos DB finden Sie in folgenden Artikeln:

* Informationen zum Konfigurieren einer Firewall für Azure Cosmos DB finden Sie unter [Firewallunterstützung](firewall-support.md).

* [Konfigurieren von VNET-Dienstendpunkten für Ihr Azure Cosmos-Konto](how-to-configure-vnet-service-endpoint.md)

* Weitere Informationen zu Private Link finden Sie in der Dokumentation zu [Azure Private Link](../private-link/private-link-overview.md).
