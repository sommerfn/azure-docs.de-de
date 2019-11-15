---
title: Konfigurieren einer Failovergruppe
description: Erfahren Sie, wie Sie eine Autofailover-Gruppe für eine einzelne Datenbank, einen Pool für elastische Datenbanken und eine verwaltete Instanz von Azure SQL-Datenbank mithilfe des Azure-Portals, des AZ-CLI und PowerShell konfigurieren.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.reviewer: sstein, carlrab
ms.date: 08/14/2019
ms.openlocfilehash: fb9ee2378679c420a7675856ec95e60f6ae1d14f
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73827153"
---
# <a name="configure-a-failover-group-for-azure-sql-database"></a>Konfigurieren einer Failovergruppe für Azure SQL-Datenbank

In diesem Thema erfahren Sie, wie Sie eine [Autofailover-Gruppe](sql-database-auto-failover-group.md) für eine einzelne Datenbank, einen Pool für elastische Datenbanken und eine verwaltete Instanz von Azure SQL-Datenbank mithilfe des Azure-Portals oder PowerShell konfigurieren. 

## <a name="single-database"></a>Einzeldatenbank
Erstellen Sie die Failovergruppe, und fügen Sie ihr eine einzelne Datenbank mithilfe des Azure-Portals oder PowerShell hinzu.

### <a name="prerequisites"></a>Voraussetzungen

Beachten Sie die folgenden erforderlichen Voraussetzungen:

- Die Einstellungen für Serveranmeldung und Firewall für den sekundären Server müssen mit denen Ihres primären Servers übereinstimmen. 

### <a name="create-failover-group"></a>Erstellen einer Failovergruppe

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)
Erstellen Sie Ihre Failovergruppe und fügen Sie Ihre Einzeldaten mithilfe des Azure-Portals zu dieser Gruppe hinzu.

1. Wählen Sie im linken Menü im [Azure-Portal](https://portal.azure.com) die Option **Azure SQL** aus. Wenn **Azure SQL** nicht in der Liste aufgeführt ist, wählen Sie **Alle Dienste** aus, und geben Sie dann „Azure SQL“ in das Suchfeld ein. (Optional:) Wählen Sie den Stern neben **Azure SQL** aus, um die Option als Favorit zu markieren und als Element im linken Navigationsbereich hinzuzufügen. 
1. Wählen Sie die einzelne Datenbank aus, die Sie der Failovergruppe hinzufügen möchten. 
1. Wählen Sie unter **Servername** den Namen des Servers aus, um die Einstellungen für diesen Server zu öffnen.

   ![Öffnen des Servers für einen Singleton](media/sql-database-single-database-failover-group-tutorial/open-sql-db-server.png)

1. Wählen Sie **Failovergruppen** im Bereich **Einstellungen** aus, und klicken Sie dann auf **Gruppe hinzufügen**, um eine neue Failovergruppe zu erstellen. 

    ![Hinzufügen einer neuen Failovergruppe](media/sql-database-single-database-failover-group-tutorial/sqldb-add-new-failover-group.png)

1. Wählen Sie auf der Seite **Failovergruppe** die erforderlichen Werte aus bzw. geben Sie sie ein, und wählen Sie dann **Erstellen** aus.

   - **Datenbank innerhalb der Gruppe**: Wählen Sie die Datenbank aus, die Sie Ihrer Failovergruppe hinzufügen möchten. Durch das Hinzufügen der Datenbank zur Failovergruppe wird automatisch der Georeplikationsprozess gestartet. 
        
    ![Hinzufügen von SQL DB zur Failovergruppe](media/sql-database-single-database-failover-group-tutorial/add-sqldb-to-failover-group.png)

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)
Erstellen Sie Ihre Failovergruppe und fügen Sie Ihre Einzeldaten mithilfe von PowerShell zu dieser Gruppe hinzu. 

   ```powershell-interactive
   $subscriptionId = "<SubscriptionID>"
   $resourceGroupName = "<Resource-Group-Name>"
   $location = "<Region>"
   $adminLogin = "<Admin-Login>"
   $password = "<Complex-Password>"
   $serverName = "<Primary-Server-Name>"
   $databaseName = "<Database-Name>"
   $drLocation = "<DR-Region>"
   $drServerName = "<Secondary-Server-Name>"
   $failoverGroupName = "<Failover-Group-Name>"

   # Create a secondary server in the failover region
   Write-host "Creating a secondary logical server in the failover region..."
   $drServer = New-AzSqlServer -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName `
      -Location $drLocation `
      -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential `
         -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
   $drServer
   
   # Create a failover group between the servers
   $failovergroup = Write-host "Creating a failover group between the primary and secondary server..."
   New-AzSqlDatabaseFailoverGroup `
      –ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -PartnerServerName $drServerName  `
      –FailoverGroupName $failoverGroupName `
      –FailoverPolicy Automatic `
      -GracePeriodWithDataLossHours 2
   $failovergroup
   
   # Add the database to the failover group
   Write-host "Adding the database to the failover group..." 
   Get-AzSqlDatabase `
      -ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -DatabaseName $databaseName | `
   Add-AzSqlDatabaseToFailoverGroup `
      -ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -FailoverGroupName $failoverGroupName
   Write-host "Successfully added the database to the failover group..." 
   ```

---

### <a name="test-failover"></a>Testfailover 

Testen Sie das Failover Ihrer Failovergruppe mithilfe des Azure-Portals oder PowerShell. 

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

Testen Sie das Failover Ihrer Failovergruppe mithilfe des Azure-Portals. 

1. Wählen Sie im linken Menü im [Azure-Portal](https://portal.azure.com) die Option **Azure SQL** aus. Wenn **Azure SQL** nicht in der Liste aufgeführt ist, wählen Sie **Alle Dienste** aus, und geben Sie dann „Azure SQL“ in das Suchfeld ein. (Optional:) Wählen Sie den Stern neben **Azure SQL** aus, um die Option als Favorit zu markieren und als Element im linken Navigationsbereich hinzuzufügen. 
1. Wählen Sie die einzelne Datenbank aus, die Sie der Failovergruppe hinzufügen möchten. 

   ![Öffnen des Servers für einen Singleton](media/sql-database-single-database-failover-group-tutorial/open-sql-db-server.png)

1. Wählen Sie **Failovergruppen** im Bereich **Einstellungen** aus, und wählen Sie dann die soeben erstellte Failovergruppe aus. 
  
   ![Auswählen der Failovergruppe aus dem Portal](media/sql-database-single-database-failover-group-tutorial/select-failover-group.png)

1. Überprüfen Sie, welcher Server der primäre und welcher der sekundäre ist. 
1. Wählen Sie im Aufgabenbereich **Failover** aus, um ein Failover für die Failovergruppe mit Ihrer einzelnen Datenbank durchzuführen. 
1. Wählen Sie in der Meldung, dass die TDS-Sitzungen getrennt werden, **Ja** aus. 

   ![Failover für die Failovergruppe mit Ihrer SQL-Datenbank](media/sql-database-single-database-failover-group-tutorial/failover-sql-db.png)

1. Überprüfen Sie, welcher Server jetzt der primäre und welcher der sekundäre ist. Wenn das Failover erfolgreich ausgeführt wurde, sollten die beiden Server die Rollen getauscht haben. 
1. Wählen Sie erneut **Failover** aus, um die Server auf ihre ursprünglichen Rollen zurückzusetzen. 

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Testen Sie das Failover Ihrer Failovergruppe mithilfe von PowerShell.  

Überprüfen Sie die Rolle des sekundären Replikats: 

   ```powershell-interactive
   # Set variables
   $resourceGroupName = "<Resource-Group-Name>"
   $serverName = "<Primary-Server-Name>"
   $failoverGroupName = "<Failover-Group-Name>"
   
   # Check role of secondary replica
   Write-host "Confirming the secondary replica is secondary...." 
   (Get-AzSqlDatabaseFailoverGroup `
      -FailoverGroupName $failoverGroupName `
      -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName).ReplicationRole
   ```
Failover auf den sekundären Server: 

   ```powershell-interactive
   # Set variables
   $resourceGroupName = "<Resource-Group-Name>"
   $serverName = "<Primary-Server-Name>"
   $failoverGroupName = "<Failover-Group-Name>"
   
   # Failover to secondary server
   Write-host "Failing over failover group to the secondary..." 
   Switch-AzSqlDatabaseFailoverGroup `
      -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName `
      -FailoverGroupName $failoverGroupName
   Write-host "Failed failover group to successfully to" $drServerName 
   ```

Stellen Sie die Failovergruppe auf dem primären Server wieder her:

   ```powershell-interactive
   # Set variables
   $resourceGroupName = "<Resource-Group-Name>"
   $serverName = "<Primary-Server-Name>"
   $failoverGroupName = "<Failover-Group-Name>"
   
   # Revert failover to primary server
   Write-host "Failing over failover group to the primary...." 
   Switch-AzSqlDatabaseFailoverGroup `
      -ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -FailoverGroupName $failoverGroupName
   Write-host "Failed failover group successfully to back to" $serverName
   ```

---

## <a name="elastic-pool"></a>Pool für elastische Datenbanken
Erstellen Sie die Failovergruppe, und fügen Sie ihr einen Pool für elastische Datenbanken mithilfe des Azure-Portals oder PowerShell hinzu.  

### <a name="prerequisites"></a>Voraussetzungen

Beachten Sie die folgenden erforderlichen Voraussetzungen:

- Die Einstellungen für Serveranmeldung und Firewall für den sekundären Server müssen mit denen Ihres primären Servers übereinstimmen. 

### <a name="create-the-failover-group"></a>Erstellen der Failovergruppe 

Erstellen Sie die Failovergruppe für Ihren Pool für elastische Datenbanken mithilfe des Azure-Portals oder PowerShell. 

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)
Erstellen Sie Ihre Failovergruppe, und fügen Sie ihr Ihren Pool für elastische Datenbanken mithilfe des Azure-Portals hinzu.

1. Wählen Sie im linken Menü im [Azure-Portal](https://portal.azure.com) die Option **Azure SQL** aus. Wenn **Azure SQL** nicht in der Liste aufgeführt ist, wählen Sie **Alle Dienste** aus, und geben Sie dann „Azure SQL“ in das Suchfeld ein. (Optional:) Wählen Sie den Stern neben **Azure SQL** aus, um die Option als Favorit zu markieren und als Element im linken Navigationsbereich hinzuzufügen. 
1. Wählen Sie den Pool für elastische Datenbanken aus, den Sie der Failovergruppe hinzufügen möchten. 
1. Wählen Sie im Bereich **Übersicht** unter **Servername** den Namen des Servers aus, um die Einstellungen für den Server zu öffnen.
  
    ![Öffnen des Servers für den Pool für elastische Datenbanken](media/sql-database-elastic-pool-failover-group-tutorial/server-for-elastic-pool.png)

1. Wählen Sie **Failovergruppen** im Bereich **Einstellungen** aus, und klicken Sie dann auf **Gruppe hinzufügen**, um eine neue Failovergruppe zu erstellen. 

    ![Hinzufügen einer neuen Failovergruppe](media/sql-database-single-database-failover-group-tutorial/sqldb-add-new-failover-group.png)

1. Wählen Sie auf der Seite **Failovergruppe** die erforderlichen Werte aus bzw. geben Sie sie ein, und wählen Sie dann **Erstellen** aus. Erstellen Sie entweder einen neuen sekundären Server, oder wählen Sie einen vorhandenen sekundären Server aus. 

1. Wählen Sie **Datenbanken in der Gruppe** und dann den Pool für elastische Datenbanken aus, den Sie der Failovergruppe hinzufügen möchten. Wenn auf dem sekundären Server noch kein Pool für elastische Datenbanken vorhanden ist, wird eine Warnung angezeigt, die Sie auffordert, einen Pool für elastische Datenbanken auf dem sekundären Server zu erstellen. Wählen Sie die Warnung aus, und klicken Sie dann auf **OK**, um den Pool für elastische Datenbanken auf dem sekundären Server zu erstellen. 
        
    ![Hinzufügen eines Pools für elastische Datenbanken zu einer Failovergruppe](media/sql-database-elastic-pool-failover-group-tutorial/add-elastic-pool-to-failover-group.png)
        
1. Wählen Sie **Auswählen** aus, um die Einstellungen für den Pool für elastische Datenbanken auf die Failovergruppe anzuwenden, und klicken Sie dann auf **Erstellen**, um Ihre Failovergruppe zu erstellen. Durch das Hinzufügen des Pool für elastische Datenbanken zur Failovergruppe wird automatisch der Georeplikationsprozess gestartet. 

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Erstellen Sie Ihre Failovergruppe, und fügen Sie ihr Ihren Pool für elastische Datenbanken mithilfe von PowerShell hinzu. 

   ```powershell-interactive
   $subscriptionId = "<SubscriptionID>"
   $resourceGroupName = "<Resource-Group-Name>"
   $location = "<Region>"
   $adminLogin = "<Admin-Login>"
   $password = "<Complex-Password>"
   $serverName = "<Primary-Server-Name>"
   $databaseName = "<Database-Name>"
   $poolName = "myElasticPool"
   $drLocation = "<DR-Region>"
   $drServerName = "<Secondary-Server-Name>"
   $failoverGroupName = "<Failover-Group-Name>"

   # Create a failover group between the servers
   Write-host "Creating failover group..." 
   New-AzSqlDatabaseFailoverGroup `
       –ResourceGroupName $resourceGroupName `
       -ServerName $serverName `
       -PartnerServerName $drServerName  `
       –FailoverGroupName $failoverGroupName `
       –FailoverPolicy Automatic `
       -GracePeriodWithDataLossHours 2
   Write-host "Failover group created successfully." 

   # Add elastic pool to the failover group
   Write-host "Enumerating databases in elastic pool...." 
   $FailoverGroup = Get-AzSqlDatabaseFailoverGroup `
                    -ResourceGroupName $resourceGroupName `
                    -ServerName $serverName `
                    -FailoverGroupName $failoverGroupName
   $databases = Get-AzSqlElasticPoolDatabase `
               -ResourceGroupName $resourceGroupName `
               -ServerName $serverName `
               -ElasticPoolName $poolName
   Write-host "Adding databases to failover group..." 
   $failoverGroup = $failoverGroup | Add-AzSqlDatabaseToFailoverGroup `
                                     -Database $databases 
   Write-host "Databases added to failover group successfully." 
  ```

---

### <a name="test-failover"></a>Testfailover

Testen Sie das Failover Ihres Pool für elastische Datenbanken mithilfe des Azure-Portals oder PowerShell. 

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

Führen Sie ein Failover für Ihre Failovergruppe auf den sekundären Server und anschließend ein Failback mit dem Azure-Portal aus. 

1. Wählen Sie im linken Menü im [Azure-Portal](https://portal.azure.com) die Option **Azure SQL** aus. Wenn **Azure SQL** nicht in der Liste aufgeführt ist, wählen Sie **Alle Dienste** aus, und geben Sie dann „Azure SQL“ in das Suchfeld ein. (Optional:) Wählen Sie den Stern neben **Azure SQL** aus, um die Option als Favorit zu markieren und als Element im linken Navigationsbereich hinzuzufügen. 
1. Wählen Sie den Pool für elastische Datenbanken aus, den Sie der Failovergruppe hinzufügen möchten. 
1. Wählen Sie im Bereich **Übersicht** unter **Servername** den Namen des Servers aus, um die Einstellungen für den Server zu öffnen.
  
    ![Öffnen des Servers für den Pool für elastische Datenbanken](media/sql-database-elastic-pool-failover-group-tutorial/server-for-elastic-pool.png)
1. Wählen Sie **Failovergruppen** im Bereich **Einstellungen** aus, und wählen Sie dann die in Abschnitt 2 erstellte Failovergruppe aus. 
  
   ![Auswählen der Failovergruppe aus dem Portal](media/sql-database-single-database-failover-group-tutorial/select-failover-group.png)

1. Überprüfen Sie, welcher Server der primäre und welcher der sekundäre ist. 
1. Wählen Sie im Aufgabenbereich **Failover** aus, um ein Failover für die Failovergruppe mit dem Pool für elastische Datenbanken durchzuführen. 
1. Wählen Sie in der Meldung, dass die TDS-Sitzungen getrennt werden, **Ja** aus. 

   ![Failover für die Failovergruppe mit Ihrer SQL-Datenbank](media/sql-database-single-database-failover-group-tutorial/failover-sql-db.png)

1. Überprüfen Sie, welcher Server der primäre und welcher der sekundäre ist. Wenn das Failover erfolgreich ausgeführt wurde, sollten die beiden Server die Rollen getauscht haben. 
1. Wählen Sie erneut **Failover** aus, um die Failovergruppe wieder auf die ursprünglichen Einstellungen zurückzusetzen. 

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Testen Sie das Failover Ihrer Failovergruppe mithilfe von PowerShell.

Überprüfen Sie die Rolle des sekundären Replikats: 

   ```powershell-interactive
   # Set variables
   $resourceGroupName = "<Resource-Group-Name>"
   $serverName = "<Primary-Server-Name>"
   $failoverGroupName = "<Failover-Group-Name>"
   
   # Check role of secondary replica
   Write-host "Confirming the secondary replica is secondary...." 
   (Get-AzSqlDatabaseFailoverGroup `
      -FailoverGroupName $failoverGroupName `
      -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName).ReplicationRole
   ```

Failover auf den sekundären Server: 

   ```powershell-interactive
   # Set variables
   $resourceGroupName = "<Resource-Group-Name>"
   $serverName = "<Primary-Server-Name>"
   $failoverGroupName = "<Failover-Group-Name>"
   
   # Failover to secondary server
   Write-host "Failing over failover group to the secondary..." 
   Switch-AzSqlDatabaseFailoverGroup `
      -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName `
      -FailoverGroupName $failoverGroupName
   Write-host "Failed failover group to successfully to" $drServerName 
   ```

---

## <a name="managed-instance"></a>Verwaltete Instanz

Erstellen Sie eine Failovergruppe zwischen zwei verwalteten Instanzen mithilfe des Azure-Portals oder PowerShell. 

Sie müssen ein Gateway für das virtuelle Netzwerk jeder verwalteten Instanz erstellen, eine Verbindung der beiden Gateways herstellen und dann die Failovergruppe erstellen.

### <a name="prerequisites"></a>Voraussetzungen
Beachten Sie die folgenden erforderlichen Voraussetzungen:

- Die sekundäre verwaltete Instanz muss leer sein.
- Der Subnetzbereich des sekundären virtuellen Netzwerks darf sich nicht mit dem Subnetzbereich des primären virtuellen Netzwerks überschneiden. 
- Sortierung und Zeitzone der sekundären Instanz müssen mit denen der primären Instanz übereinstimmen. 
- Beim Herstellen der Verbindung der zwei Gateways sollte der **gemeinsam verwendete Schlüssel** für beide Verbindungen der gleiche sein. 

### <a name="create-primary-virtual-network-gateway"></a>Erstellen eines Gateways für das primäre virtuelle Netzwerk 

Erstellen Sie das Gateway für das primäre virtuelle Netzwerk mithilfe des Azure-Portals oder PowerShell. 

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

Erstellen Sie das Gateway für das primäre virtuelle Netzwerk mithilfe des Azure-Portals. 

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrer Ressourcengruppe, und wählen Sie die Ressource **Virtuelles Netzwerk** für Ihre primäre verwaltete Instanz aus. 
1. Wählen Sie unter **Einstellungen**die Option **Subnetze** aus, und wählen Sie dann ein neues**Gatewaysubnetz** aus. Lassen Sie die Standardwerte unverändert. 

   ![Hinzufügen eines Gateways für die primäre verwaltete Instanz](media/sql-database-managed-instance-failover-group-tutorial/add-subnet-gateway-primary-vnet.png)

1. Nachdem das Subnetzgateway erstellt wurde, wählen Sie im linken Navigationsbereich **Ressource erstellen** aus, und geben Sie dann `Virtual network gateway` in das Suchfeld ein. Wählen Sie die Ressource **Virtuelles Netzwerkgateway** aus, die von **Microsoft** veröffentlicht wurde. 

   ![Erstellen eines neuen Gateways für das virtuelle Netzwerk](media/sql-database-managed-instance-failover-group-tutorial/create-virtual-network-gateway.png)

1. Füllen Sie die Pflichtfelder aus, um das Gateway ihrer primären verwalteten Instanz zu konfigurieren. 

   Die folgende Tabelle enthält die erforderlichen Werte das Gateway für die primäre verwaltete Instanz:
 
    | **Feld** | Wert |
    | --- | --- |
    | **Abonnement** |  Das Abonnement, in dem sich Ihre primäre verwaltete Instanz befindet. |
    | **Name** | Der Name für das Gateway des virtuellen Netzwerks. | 
    | **Region** | Die Region, in der sich die sekundäre verwaltete Instanz befindet. |
    | **Gatewaytyp** | Wählen Sie **VPN** aus. |
    | **VPN-Typ** | Wählen Sie **Routenbasiert** aus. |
    | **SKU**| Lassen Sie den Standardwert `VpnGw1` unverändert. |
    | **Location**| Der Standort, an dem sich Ihre sekundäre verwaltete Instanz und das sekundäre virtuelle Netzwerk befinden.   |
    | **Virtuelles Netzwerk**| Wählen Sie das virtuelle Netzwerk für Ihre sekundäre verwaltete Instanz aus. |
    | **Öffentliche IP-Adresse**| Wählen Sie **Neu erstellen**. |
    | **Name der öffentlichen IP-Adresse**| Geben Sie einen Namen für die IP-Adresse ein. |
    | &nbsp; | &nbsp; |

1. Belassen Sie die anderen Werte als Standardwerte, und wählen Sie dann **Überprüfen und erstellen** aus, um die Einstellungen für Ihr virtuelles Netzwerkgateway zu überprüfen.

   ![Primäre Gatewayeinstellungen](media/sql-database-managed-instance-failover-group-tutorial/settings-for-primary-gateway.png)

1. Wählen Sie **Erstellen** aus, um das neue Gateway für Ihr virtuelles Netzwerk zu erstellen. 

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Erstellen Sie das Gateway für das primäre virtuelle Netzwerk mithilfe von PowerShell. 

   ```powershell-interactive
   $primaryResourceGroupName = "<Primary-Resource-Group>"
   $primaryVnetName = "<Primary-Virtual-Network-Name>"
   $primaryGWName = "<Primary-Gateway-Name>"
   $primaryGWPublicIPAddress = $primaryGWName + "-ip"
   $primaryGWIPConfig = $primaryGWName + "-ipc"
   $primaryGWAsn = 61000
   
   # Get the primary virtual network
   $vnet1 = Get-AzVirtualNetwork -Name $primaryVnetName -ResourceGroupName $primaryResourceGroupName
   $primaryLocation = $vnet1.Location
   
   # Create primary gateway
   Write-host "Creating primary gateway..."
   $subnet1 = Get-AzVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet1
   $gwpip1= New-AzPublicIpAddress -Name $primaryGWPublicIPAddress -ResourceGroupName $primaryResourceGroupName `
            -Location $primaryLocation -AllocationMethod Dynamic
   $gwipconfig1 = New-AzVirtualNetworkGatewayIpConfig -Name $primaryGWIPConfig `
            -SubnetId $subnet1.Id -PublicIpAddressId $gwpip1.Id
    
   $gw1 = New-AzVirtualNetworkGateway -Name $primaryGWName -ResourceGroupName $primaryResourceGroupName `
       -Location $primaryLocation -IpConfigurations $gwipconfig1 -GatewayType Vpn `
       -VpnType RouteBased -GatewaySku VpnGw1 -EnableBgp $true -Asn $primaryGWAsn
   $gw1
   ```

---

### <a name="create-secondary-virtual-network-gateway"></a>Erstellen des Gateways für das sekundäre virtuelle Netzwerk

Erstellen Sie das Gateway für das sekundäre virtuelle Netzwerk mithilfe des Azure-Portals oder PowerShell. 

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)
Wiederholen Sie die Schritte im vorherigen Abschnitt, um das Subnetz des virtuellen Netzwerks und das Gateway für die sekundäre verwaltete Instanz zu erstellen. Füllen Sie die Pflichtfelder aus, um das Gateway für die sekundäre verwaltete Instanz zu konfigurieren. 

   Die folgende Tabelle enthält die erforderlichen Werte für das Gateway für die sekundäre verwaltete Instanz:

   | **Feld** | Wert |
   | --- | --- |
   | **Abonnement** |  Das Abonnement, in dem sich Ihre sekundäre verwaltete Instanz befindet. |
   | **Name** | Der Name für das Gateway des virtuellen Netzwerks, z.B. `secondary-mi-gateway`. | 
   | **Region** | Die Region, in der sich die sekundäre verwaltete Instanz befindet. |
   | **Gatewaytyp** | Wählen Sie **VPN** aus. |
   | **VPN-Typ** | Wählen Sie **Routenbasiert** aus. |
   | **SKU**| Lassen Sie den Standardwert `VpnGw1` unverändert. |
   | **Location**| Der Standort, an dem sich Ihre sekundäre verwaltete Instanz und das sekundäre virtuelle Netzwerk befinden.   |
   | **Virtuelles Netzwerk**| Wählen Sie das virtuelle Netzwerk aus, das in Abschnitt 2 erstellt wurde, z.B. `vnet-sql-mi-secondary`. |
   | **Öffentliche IP-Adresse**| Wählen Sie **Neu erstellen**. |
   | **Name der öffentlichen IP-Adresse**| Geben Sie einen Namen für die IP-Adresse ein, z.B. `secondary-gateway-IP`. |
   | &nbsp; | &nbsp; |

   ![Sekundäre Gatewayeinstellungen](media/sql-database-managed-instance-failover-group-tutorial/settings-for-secondary-gateway.png)

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Erstellen Sie das Gateway für das sekundäre virtuelle Netzwerk mithilfe von PowerShell. 

   ```powershell-interactive
   $secondaryResourceGroupName = "<Secondary-Resource-Group>"
   $secondaryVnetName = "<Secondary-Virtual-Network-Name>"
   $secondaryGWName = "<Secondary-Gateway-Name>"
   $secondaryGWPublicIPAddress = $secondaryGWName + "-IP"
   $secondaryGWIPConfig = $secondaryGWName + "-ipc"
   $secondaryGWAsn = 62000
   
   # Get the secondary virtual network
   $vnet2 = Get-AzVirtualNetwork -Name $secondaryVnetName -ResourceGroupName $secondaryResourceGroupName
   $secondaryLocation = $vnet2.Location
    
   # Create the secondary gateway
   Write-host "Creating secondary gateway..."
   $subnet2 = Get-AzVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet2
   $gwpip2= New-AzPublicIpAddress -Name $secondaryGWPublicIPAddress -ResourceGroupName $secondaryResourceGroupName `
            -Location $secondaryLocation -AllocationMethod Dynamic
   $gwipconfig2 = New-AzVirtualNetworkGatewayIpConfig -Name $secondaryGWIPConfig `
            -SubnetId $subnet2.Id -PublicIpAddressId $gwpip2.Id
    
   $gw2 = New-AzVirtualNetworkGateway -Name $secondaryGWName -ResourceGroupName $secondaryResourceGroupName `
       -Location $secondaryLocation -IpConfigurations $gwipconfig2 -GatewayType Vpn `
       -VpnType RouteBased -GatewaySku VpnGw1 -EnableBgp $true -Asn $secondaryGWAsn
   
   $gw2
   ```

---


### <a name="connect-the-gateways"></a>Verbinden der Gateways 
Erstellen Sie Verbindungen zwischen den beiden Gateways mithilfe des Azure-Portals oder PowerShell. 

Es müssen zwei Verbindungen hergestellt werden – die Verbindung vom primären Gateway zum sekundären Gateway und dann die Verbindung vom sekundären Gateway zum primären Gateway. 

Der gemeinsam verwendete Schlüssel für beide Verbindungen sollte übereinstimmen. 

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)
Erstellen Sie Verbindungen zwischen den beiden Gateways mithilfe des Azure-Portals. 

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Option **Ressource erstellen** aus.
1. Geben Sie `connection` in das Suchfeld ein, und drücken Sie die EINGABETASTE, um die Suche zu starten. Sie gelangen zur Ressource **Verbindung**, die von Microsoft veröffentlicht wurde.
1. Wählen Sie **Erstellen** aus, um die Verbindung zu erstellen. 
1. Wählen Sie auf der Registerkarte **Grundlagen** die folgenden Werte und dann **OK** aus. 
    1. Wählen Sie für **Verbindungstyp** den Eintrag `VNet-to-VNet` aus. 
    1. Wählen Sie in der Dropdownliste Ihr Abonnement aus. 
    1. Wählen Sie die Ressourcengruppe für Ihre verwaltete Instanz aus der Dropdownliste aus. 
    1. Wählen Sie den Speicherort der primären verwalteten Instanz aus der Dropdownliste aus. 
1. Wählen Sie auf der Seite **Einstellungen** die folgenden Werte aus, oder geben Sie die Werte ein, und klicken Sie dann auf **OK**:
    1. Wählen Sie das primäre Netzwerkgateway für **Erstes Gateway für virtuelle Netzwerke** aus, z.B. `Primary-Gateway`.  
    1. Wählen Sie das sekundäre Netzwerkgateway für **Zweites Gateway für virtuelle Netzwerke** aus, z.B. `Secondary-Gateway`. 
    1. Aktivieren Sie das Kontrollkästchen neben **Bidirektionale Konnektivität einrichten**. 
    1. Übernehmen Sie entweder den Standardnamen für die primäre Verbindung, oder ändern Sie ihn in einen Namen Ihrer Wahl. 
    1. Geben Sie einen **Gemeinsam verwendeter Schlüssel (PSK)** für die Verbindung an, z.B. `mi1m2psk`. 

   ![Erstellen einer Gatewayverbindung](media/sql-database-managed-instance-failover-group-tutorial/create-gateway-connection.png)

1. Überprüfen Sie auf der Registerkarte **Zusammenfassung** die Einstellungen für die bidirektionale Verbindung, und wählen Sie dann **OK** aus, um die Verbindung zu erstellen. 

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Erstellen Sie Verbindungen zwischen den beiden Gateways mithilfe von PowerShell. 

   ```powershell-interactive
   $vpnSharedKey = "mi1mi2psk"
    
   $primaryResourceGroupName = "<Primary-Resource-Group>"
   $primaryGWConnection = "<Primary-connection-name>"
   $primaryLocation = "<Primary-Region>"
    
   $secondaryResourceGroupName = "<Secondary-Resource-Group>"
   $secondaryGWConnection = "<Secondary-connection-name>"
   $secondaryLocation = "<Secondary-Region>"
  
   # Connect the primary to secondary gateway
   Write-host "Connecting the primary gateway"
   New-AzVirtualNetworkGatewayConnection -Name $primaryGWConnection -ResourceGroupName $primaryResourceGroupName `
       -VirtualNetworkGateway1 $gw1 -VirtualNetworkGateway2 $gw2 -Location $primaryLocation `
       -ConnectionType Vnet2Vnet -SharedKey $vpnSharedKey -EnableBgp $true
   $primaryGWConnection
   
   # Connect the secondary to primary gateway
   Write-host "Connecting the secondary gateway"
   
   New-AzVirtualNetworkGatewayConnection -Name $secondaryGWConnection -ResourceGroupName $secondaryResourceGroupName `
       -VirtualNetworkGateway1 $gw2 -VirtualNetworkGateway2 $gw1 -Location $secondaryLocation `
       -ConnectionType Vnet2Vnet -SharedKey $vpnSharedKey -EnableBgp $true
   $secondaryGWConnection
   ```

---

### <a name="create-the-failover-group"></a>Erstellen der Failovergruppe 
Erstellen Sie die Failovergruppe für Ihre verwalteten Instanzen mithilfe des Azure-Portals oder PowerShell. 

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

Erstellen Sie die Failovergruppe für Ihre verwalteten Instanzen mithilfe des Azure-Portals. 

1. Wählen Sie im linken Menü im [Azure-Portal](https://portal.azure.com) die Option **Azure SQL** aus. Wenn **Azure SQL** nicht in der Liste aufgeführt ist, wählen Sie **Alle Dienste** aus, und geben Sie dann „Azure SQL“ in das Suchfeld ein. (Optional:) Wählen Sie den Stern neben **Azure SQL** aus, um die Option als Favorit zu markieren und als Element im linken Navigationsbereich hinzuzufügen. 
1. Wählen Sie die primäre verwaltete Instanz aus, die Sie der Failovergruppe hinzufügen möchten.  
1. Navigieren Sie unter **Einstellungen** zu **Instanzfailovergruppen**, und wählen Sie dann **Gruppe hinzufügen** aus, um die Seite **Instanzfailovergruppe** zu öffnen. 

   ![Hinzufügen einer Failovergruppe](media/sql-database-managed-instance-failover-group-tutorial/add-failover-group.png)

1. Geben Sie auf der Seite **Instanzfailovergruppe** den Namen Ihrer Failovergruppe ein, und wählen Sie dann im Dropdownmenü die sekundäre verwaltete Instanz aus. Wählen Sie **Erstellen** aus, um die Failovergruppe zu erstellen. 

   ![Erstellen einer Failovergruppe](media/sql-database-managed-instance-failover-group-tutorial/create-failover-group.png)

1. Nachdem die Bereitstellung der Failovergruppe abgeschlossen wurde, gelangen Sie erneut auf die Seite **Failovergruppe**. 

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Erstellen Sie die Failovergruppe für Ihre verwalteten Instanzen mithilfe von PowerShell. 

   ```powershell-interactive
   $primaryResourceGroupName = "<Primary-Resource-Group>"
   $failoverGroupName = "<Failover-Group-Name>"
   $primaryLocation = "<Primary-Region>"
   $secondaryLocation = "<Secondary-Region>"
   $primaryManagedInstance = "<Primary-Managed-Instance-Name>"
   $secondaryManagedInstance = "<Secondary-Managed-Instance-Name>"
   
   # Create failover group
   Write-host "Creating the failover group..."
   $failoverGroup = New-AzSqlDatabaseInstanceFailoverGroup -Name $failoverGroupName `
        -Location $primaryLocation -ResourceGroupName $primaryResourceGroupName -PrimaryManagedInstanceName $primaryManagedInstance `
        -PartnerRegion $secondaryLocation -PartnerManagedInstanceName $secondaryManagedInstance `
        -FailoverPolicy Automatic -GracePeriodWithDataLossHours 1
   $failoverGroup
   ```
---

### <a name="test-failover"></a>Testfailover

Testen Sie das Failover Ihrer Failovergruppe mithilfe des Azure-Portals oder PowerShell. 

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

Testen Sie das Failover Ihrer Failovergruppe mithilfe des Azure-Portals. 

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrer verwalteten Instanz, und wählen Sie unter „Einstellungen“ die Option **Instanzfailovergruppen** aus. 
1. Überprüfen Sie, welche verwaltete Instanz die primäre Instanz ist und welche verwaltete Instanz die sekundäre Instanz ist. 
1. Wählen Sie **Failover** aus, und klicken Sie dann in der Warnung zu TDS-Sitzungen, die getrennt werden, auf **Ja**. 

   ![Ausführen des Failovers der Failovergruppe](media/sql-database-managed-instance-failover-group-tutorial/failover-mi-failover-group.png)

1. Überprüfen Sie, welche verwaltete Instanz die primäre Instanz ist und welche Instanz die sekundäre Instanz ist. Wenn das Failover erfolgreich ausgeführt wurde, sollten die beiden Instanzen die Rollen getauscht haben. 

   ![Verwaltete Instanzen haben nach einem Failover die Rollen getauscht](media/sql-database-managed-instance-failover-group-tutorial/mi-switched-after-failover.png)

1. Wählen Sie erneut **Failover** aus, um ein Failback der primären Instanz in die primäre Rolle durchzuführen. 

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Testen Sie das Failover Ihrer Failovergruppe mithilfe von PowerShell. 

   ```powershell-interactive
   $primaryResourceGroupName = "<Primary-Resource-Group>"
   $secondaryResourceGroupName = "<Secondary-Resource-Group>"
   $failoverGroupName = "<Failover-Group-Name>"
   $primaryLocation = "<Primary-Region>"
   $secondaryLocation = "<Secondary-Region>"
   $primaryManagedInstance = "<Primary-Managed-Instance-Name>"
   $secondaryManagedInstance = "<Secondary-Managed-Instance-Name>"
   
   # Verify the current primary role
   Get-AzSqlDatabaseInstanceFailoverGroup -ResourceGroupName $primaryResourceGroupName `
       -Location $secondaryLocation -Name $failoverGroupName
   
   # Failover the primary managed instance to the secondary role
   Write-host "Failing primary over to the secondary location"
   Get-AzSqlDatabaseInstanceFailoverGroup -ResourceGroupName $secondaryResourceGroupName `
       -Location $secondaryLocation -Name $failoverGroupName | Switch-AzSqlDatabaseInstanceFailoverGroup
   Write-host "Successfully failed failover group to secondary location"
   
   # Verify the current primary role
   Get-AzSqlDatabaseInstanceFailoverGroup -ResourceGroupName $primaryResourceGroupName `
       -Location $secondaryLocation -Name $failoverGroupName
   
   # Fail primary managed instance back to primary role
   Write-host "Failing primary back to primary role"
   Get-AzSqlDatabaseInstanceFailoverGroup -ResourceGroupName $primaryResourceGroupName `
       -Location $primaryLocation -Name $failoverGroupName | Switch-AzSqlDatabaseInstanceFailoverGroup
   Write-host "Successfully failed failover group to primary location"
   
   # Verify the current primary role
   Get-AzSqlDatabaseInstanceFailoverGroup -ResourceGroupName $primaryResourceGroupName `
       -Location $secondaryLocation -Name $failoverGroupName
   ```

---

## <a name="locate-listener-endpoint"></a>Listenerendpunkt suchen

Nachdem die Failovergruppe konfiguriert wurde, aktualisieren Sie die Verbindungszeichenfolge für die Anwendung auf den Listenerendpunkt. Dadurch bleibt Ihre Anwendung mit dem Listener der Failovergruppe verbunden, statt mit der primären Datenbank, dem Pool für elastische Datenbanken oder der verwalteten Instanz. Auf diesen Weise brauchen Sie die Verbindungszeichenfolge nicht bei jedem Failover Ihrer Azure SQL-Datenbankentität manuell zu aktualisieren, und der Datenverkehr wird immer zu der Entität geroutet, die aktuell die primäre ist. 

Der Listenerendpunkt hat die Form `fog-name.database.windows.net` und ist beim Anzeigen der Failovergruppe im Azure-Portal sichtbar:

![Failovergruppen-Verbindungszeichenfolge](media/sql-database-configure-failover-group/find-failover-group-connection-string.png)

## <a name="remarks"></a>Anmerkungen

- Wenn Sie eine Failovergruppe für eine einzelne Datenbank oder eine Datenbank im Pool entfernen, wird die Replikation nicht beendet, und die replizierte Datenbank wird nicht gelöscht. Sie müssen die Georeplikation manuell beenden und die Datenbank von dem sekundären Server löschen, wenn Sie eine einzelne Datenbank oder eine Datenbank im Pool nach dem Löschen wieder zu einer Failovergruppe hinzufügen möchten. Wenn Sie keine der beiden Aktionen ausführen, kann ein Fehler in der Art von `The operation cannot be performed due to multiple errors` angezeigt werden, wenn Sie versuchen, der Failovergruppe die Datenbank hinzuzufügen. 


## <a name="next-steps"></a>Nächste Schritte

Ausführliche Schritte zum Konfigurieren einer Failovergruppe finden Sie in den folgenden Tutorials:
- [Hinzufügen einer einzelnen Datenbank zu einer Failovergruppe](sql-database-single-database-failover-group-tutorial.md)
- [Hinzufügen eines Pools für elastische Datenbanken zu einer Failovergruppe](sql-database-elastic-pool-failover-group-tutorial.md)
- [Hinzufügen verwalteter Instanzen zu einer Failovergruppe](sql-database-managed-instance-failover-group-tutorial.md)
 
Eine Übersicht der Optionen für Hochverfügbarkeit für Azure SQL-Datenbank finden Sie unter [Georeplikation](sql-database-active-geo-replication.md) und [Autofailover-Gruppen](sql-database-auto-failover-group.md). 
