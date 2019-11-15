---
title: 'Tutorial: Hinzufügen einer Einzeldatenbank zu einer Failovergruppe'
description: Fügen Sie eine Azure SQL-Einzeldatenbank mit dem Azure-Portal, PowerShell oder der Azure CLI zu einer Failovergruppe hinzu.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.reviewer: sstein, carlrab
ms.date: 06/19/2019
ms.openlocfilehash: 6e3b4be836699cc200d30168c14462f81136646b
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73821092"
---
# <a name="tutorial-add-an-azure-sql-database-single-database-to-a-failover-group"></a>Tutorial: Hinzufügen einer Azure SQL-Einzeldatenbank zu einer Failovergruppe

Konfigurieren Sie eine Failovergruppe für eine Azure SQL-Einzeldatenbank und testen Sie das Failover entweder mithilfe des Azure-Portals, PowerShell oder der Azure CLI.  In diesem Lernprogramm lernen Sie Folgendes:

> [!div class="checklist"]
> - Erstellen einer Azure SQL-Einzeldatenbank.
> - Erstellen einer [Failovergruppe](sql-database-auto-failover-group.md) für eine Einzeldatenbank zwischen zwei logischen SQL Server-Instanzen.
> - Testen des Failovers.

## <a name="prerequisites"></a>Voraussetzungen

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)
Damit Sie dieses Tutorial ausführen können, benötigen Sie folgende Komponenten: 

- Ein Azure-Abonnement. [Erstellen Sie ein kostenloses Konto](https://azure.microsoft.com/free/), wenn Sie noch keines besitzen.


# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)
Für dieses Tutorial wird Folgendes vorausgesetzt:

- Ein Azure-Abonnement. [Erstellen Sie ein kostenloses Konto](https://azure.microsoft.com/free/), wenn Sie noch keines besitzen.
- [Azure PowerShell](/powershell/azureps-cmdlets-docs)


# <a name="azure-clitabazure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)
Für dieses Tutorial wird Folgendes vorausgesetzt:

- Ein Azure-Abonnement. [Erstellen Sie ein kostenloses Konto](https://azure.microsoft.com/free/), wenn Sie noch keines besitzen.
- Die neueste Version von [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest). 

---

## <a name="1---create-a-single-database"></a>1 – Erstellen einer Einzeldatenbank 

[!INCLUDE [sql-database-create-single-database](includes/sql-database-create-single-database.md)]

## <a name="2---create-the-failover-group"></a>2 – Erstellen der Failovergruppe 
In diesem Schritt erstellen Sie eine [Failovergruppen](sql-database-auto-failover-group.md) zwischen einem vorhandenen Azure SQL Server und einem neuen Azure SQL Server in einer anderen Region. Fügen Sie dann der Failovergruppe die Beispieldatenbank hinzu. 

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)
Erstellen Sie Ihre Failovergruppe und fügen Sie Ihre Einzeldaten mithilfe des Azure-Portals zu dieser Gruppe hinzu. 

1. Wählen Sie im linken Menü im [Azure-Portal](https://portal.azure.com) die Option **Azure SQL** aus. Wenn **Azure SQL** nicht in der Liste aufgeführt ist, wählen Sie **Alle Dienste** aus, und geben Sie dann „Azure SQL“ in das Suchfeld ein. (Optional:) Wählen Sie den Stern neben **Azure SQL** aus, um die Option als Favorit zu markieren und als Element im linken Navigationsbereich hinzuzufügen. 
1. Wählen Sie die Einzeldatenbank aus, die in Abschnitt 1 erstellt wurde, z.B. `mySampleDatabase`. 
1. Wählen Sie unter **Servername** den Namen des Servers aus, um die Einstellungen für diesen Server zu öffnen.

   ![Öffnen des Servers für einen Singleton](media/sql-database-single-database-failover-group-tutorial/open-sql-db-server.png)

1. Wählen Sie **Failovergruppen** im Bereich **Einstellungen** aus, und klicken Sie dann auf **Gruppe hinzufügen**, um eine neue Failovergruppe zu erstellen. 

    ![Hinzufügen einer neuen Failovergruppe](media/sql-database-single-database-failover-group-tutorial/sqldb-add-new-failover-group.png)

1. Wählen Sie auf der Seite **Failovergruppe** die folgenden Werte aus bzw. geben Sie sie ein, und wählen Sie dann **Erstellen**:
    - **Name der Failovergruppe**: Geben Sie einen eindeutigen Namen für die Failovergruppe ein, z.B. `failovergrouptutorial`. 
    - **Sekundärer Server**: Wählen Sie die Option  *zum Konfigurieren der erforderlichen Einstellungen* aus, und wählen Sie dann **Neuen Server erstellen** aus. Alternativ können Sie auch einen bereits vorhandenen Server als sekundären Server auswählen. Nachdem Sie die folgenden Werte eingegeben haben, wählen Sie **Auswählen** aus. 
        - **Servername**: Geben Sie einen eindeutigen Namen für den sekundären Server ein, z.B. `mysqlsecondary`. 
        - **Serveradministratoranmeldung**: Geben Sie Folgendes ein: `azureuser`
        - **Kennwort**: Geben Sie ein komplexes Kennwort ein, das die Anforderungen für Kennwörter erfüllt.
        - **Standort**: Wählen Sie in der Dropdownliste einen Standort aus, z. B. `East US`. Dieser Standort darf nicht mit dem Standort des primären Servers übereinstimmen.

    > [!NOTE]
    > Die Einstellungen für Serveranmeldung und Firewall müssen jedoch mit denen Ihres primären Servers übereinstimmen. 
    
      ![Erstellen eines sekundären Servers für die Failovergruppe](media/sql-database-single-database-failover-group-tutorial/create-secondary-failover-server.png)

   - **Datenbank innerhalb der Gruppe**: Sobald ein sekundärer Server ausgewählt wurde, wird diese Option entsperrt. Wählen Sie die Option aus, um **Hinzuzufügende Datenbank auswählen** auszuwählen. Wählen Sie anschließend die in Abschnitt 1 erstellte Datenbank aus. Durch das Hinzufügen der Datenbank zur Failovergruppe wird automatisch der Georeplikationsprozess gestartet. 
        
    ![Hinzufügen von SQL DB zur Failovergruppe](media/sql-database-single-database-failover-group-tutorial/add-sqldb-to-failover-group.png)
        

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)
Erstellen Sie Ihre Failovergruppe und fügen Sie Ihre Einzeldaten mithilfe von PowerShell zu dieser Gruppe hinzu. 

   > [!NOTE]
   > Die Einstellungen für Serveranmeldung und Firewall müssen jedoch mit denen Ihres primären Servers übereinstimmen. 

   ```powershell-interactive
   # $subscriptionId = '<SubscriptionID>'
   # $resourceGroupName = "myResourceGroup-$(Get-Random)"
   # $location = "West US"
   # $adminLogin = "azureuser"
   # $password = "PWD27!"+(New-Guid).Guid
   # $serverName = "mysqlserver-$(Get-Random)"
   # $databaseName = "mySampleDatabase"
   $drLocation = "East US"
   $drServerName = "mysqlsecondary-$(Get-Random)"
   $failoverGroupName = "failovergrouptutorial-$(Get-Random)"

   # The ip address range that you want to allow to access your server 
   # (leaving at 0.0.0.0 will prevent outside-of-azure connections to your DB)
   $startIp = "0.0.0.0"
   $endIp = "0.0.0.0"

   # Show randomized variables
   Write-host "DR Server name is" $drServerName 
   Write-host "Failover group name is" $failoverGroupName
   
   # Create a secondary server in the failover region
   Write-host "Creating a secondary logical server in the failover region..."
   $drServer = New-AzSqlServer -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName `
      -Location $drLocation `
      -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential `
         -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
   $drServer

   # Create a server firewall rule that allows access from the specified IP range
   Write-host "Configuring firewall for secondary logical server..."
   $serverFirewallRule = New-AzSqlServerFirewallRule -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName `
      -FirewallRuleName "AllowedIPs" -StartIpAddress $startIp -EndIpAddress $endIp
   $serverFirewallRule   
   
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

In diesem Teil des Tutorials werden die folgenden PowerShell-Cmdlets verwendet:

| Get-Help | Notizen |
|---|---|
| [New-AzSqlServer](/powershell/module/az.sql/new-azsqlserver) | Erstellt einen SQL-Datenbank-Server, der Einzeldatenbanken und Pools für elastische Datenbanken hostet. |
| [New-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule) | Erstellt eine Firewallregel für einen logischen Server. | 
| [New-AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase) | Erstellt eine neue Azure SQL-Einzeldatenbank. | 
| [New-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/new-azsqldatabasefailovergroup) | Erstellt eine neue Failovergruppe. |
| [Get-AzSqlDatabase](/powershell/module/az.sql/get-azsqldatabase) | Ruft mindestens eine SQL-Datenbank ab. |
| [Add-AzSqlDatabaseToFailoverGroup](/powershell/module/az.sql/add-azsqldatabasetofailovergroup) | Fügt einer Failovergruppe mindestens eine Azure SQL-Datenbank hinzu. |

# <a name="azure-clitabazure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)
Erstellen Sie Ihre Failovergruppe und fügen Sie Ihre Einzeldaten mithilfe der Azure CLI zu dieser Gruppe hinzu. 

   > [!NOTE]
   > Die Einstellungen für Serveranmeldung und Firewall müssen jedoch mit denen Ihres primären Servers übereinstimmen. 

   ```azurecli-interactive
   #!/bin/bash
   # Set variables
   # subscriptionID=<SubscriptionID>
   # resourceGroupName=myResourceGroup-$RANDOM
   # location=SouthCentralUS
   # adminLogin=azureuser
   # password="PWD27!"+`openssl rand -base64 18`
   # serverName=mysqlserver-$RANDOM
   # databaseName=mySampleDatabase
   drLocation=NorthEurope
   drServerName=mysqlsecondary-$RANDOM
   failoverGroupName=failovergrouptutorial-$RANDOM

   # Create a secondary server in the failover region
   echo "Creating a secondary logical server in the DR region..."
   az sql server create \
      --name $drServerName \
      --resource-group $resourceGroupName \
      --location $drLocation  \
      --admin-user $adminLogin\
      --admin-password $password

   # Configure a firewall rule for the server
   echo "Configuring firewall..."
   az sql server firewall-rule create \
      --resource-group $resourceGroupName \
      --server $drServerName \
      -n AllowYourIp \
      --start-ip-address $startip \
      --end-ip-address $endip
   
   # Create a failover group between the servers and add the database
   echo "Creating a failover group between the two servers..."
   az sql failover-group create \
      --name $failoverGroupName  \
      --partner-server $drServerName \
      --resource-group $resourceGroupName \
      --server $serverName \
      --add-db $databaseName
      --failover-policy Automatic
   ```

In diesem Teil des Tutorials werden die folgenden Azure CLI-Cmdlets verwendet:

| Get-Help | Notizen |
|---|---|
| [az sql server create](/cli/azure/sql/server#az-sql-server-create) | Erstellt einen SQL-Datenbank-Server, der Einzeldatenbanken und Pools für elastische Datenbanken hostet. |
| [az sql server firewall-rule create](/cli/azure/sql/server/firewall-rule) | Erstellt die Firewallregeln eines Servers. | 
| [az sql failover-group create](/cli/azure/sql/failover-group?view=azure-cli-latest#az-sql-failover-group-create) | Erstellt eine Failovergruppe. | 

---

## <a name="3---test-failover"></a>3 – Testen des Failovers 
In diesem Schritt führen Sie ein Failover für Ihre Failovergruppe auf dem sekundären Server und anschließend ein Failback mit dem Azure-Portal aus. 

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)
Testen Sie ein Failover über das Azure-Portal. 

1. Wählen Sie im linken Menü im [Azure-Portal](https://portal.azure.com) die Option **Azure SQL** aus. Wenn **Azure SQL** nicht in der Liste aufgeführt ist, wählen Sie **Alle Dienste** aus, und geben Sie dann „Azure SQL“ in das Suchfeld ein. (Optional:) Wählen Sie den Stern neben **Azure SQL** aus, um die Option als Favorit zu markieren und als Element im linken Navigationsbereich hinzuzufügen. 
1. Wählen Sie den Singleton aus, der im Abschnitt 2 erstellt wurde, z. B. `mySampleDatbase`. 
1. Wählen Sie unter **Servername** den Namen des Servers aus, um die Einstellungen für diesen Server zu öffnen.

   ![Öffnen des Servers für einen Singleton](media/sql-database-single-database-failover-group-tutorial/open-sql-db-server.png)

1. Wählen Sie **Failovergruppen** im Bereich **Einstellungen** aus, und wählen Sie dann die in Abschnitt 2 erstellte Failovergruppe aus. 
  
   ![Auswählen der Failovergruppe aus dem Portal](media/sql-database-single-database-failover-group-tutorial/select-failover-group.png)

1. Überprüfen Sie, welcher Server der primäre und welcher der sekundäre ist. 
1. Wählen Sie im Aufgabenbereich **Failover** aus, um ein Failover für die Failovergruppe mit der Beispieleinzeldatei durchzuführen. 
1. Wählen Sie in der Meldung, dass die TDS-Sitzungen getrennt werden, **Ja** aus. 

   ![Failover für die Failovergruppe mit Ihrer SQL-Datenbank](media/sql-database-single-database-failover-group-tutorial/failover-sql-db.png)

1. Überprüfen Sie, welcher Server jetzt der primäre und welcher der sekundäre ist. Wenn das Failover erfolgreich ausgeführt wurde, sollten die beiden Server die Rollen getauscht haben. 
1. Wählen Sie erneut **Failover** aus, um die Server auf ihre ursprünglichen Rollen zurückzusetzen. 

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)
Testen Sie das Failovers mithilfe von PowerShell. 


Überprüfen Sie die Rolle des sekundären Replikats: 

   ```powershell-interactive
   # Set variables
   # $resourceGroupName = "myResourceGroup-$(Get-Random)"
   # $serverName = "mysqlserver-$(Get-Random)"
   # $failoverGroupName = "failovergrouptutorial-$(Get-Random)"
   
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
   # $resourceGroupName = "myResourceGroup-$(Get-Random)"
   # $serverName = "mysqlserver-$(Get-Random)"
   # $failoverGroupName = "failovergrouptutorial-$(Get-Random)"
   
   # Failover to secondary server
   Write-host "Failing over failover group to the secondary..." 
   Switch-AzSqlDatabaseFailoverGroup `
      -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName `
      -FailoverGroupName $failoverGroupName
   Write-host "Failed failover group successfully to" $drServerName 
   ```

Stellen Sie die Failovergruppe auf dem primären Server wieder her:

   ```powershell-interactive
   # Set variables
   # $resourceGroupName = "myResourceGroup-$(Get-Random)"
   # $serverName = "mysqlserver-$(Get-Random)"
   # $failoverGroupName = "failovergrouptutorial-$(Get-Random)"
   
   # Revert failover to primary server
   Write-host "Failing over failover group to the primary...." 
   Switch-AzSqlDatabaseFailoverGroup `
      -ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -FailoverGroupName $failoverGroupName
   Write-host "Failed failover group successfully back to" $serverName
   ```

In diesem Teil des Tutorials werden die folgenden PowerShell-Cmdlets verwendet:

| Get-Help | Notizen |
|---|---|
| [Get-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/get-azsqldatabasefailovergroup) | Ruft Azure SQL-Datenbank-Failovergruppen ab oder listet diese auf. |
| [Switch-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/switch-azsqldatabasefailovergroup)| Führt ein Failover für eine Azure SQL-Datenbank-Failovergruppe aus. |



# <a name="azure-clitabazure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)
Testen Sie das Failovers über die Azure CLI. 

Überprüfen Sie, welcher Server der sekundäre Server ist:

   
   ```azurecli-interactive
   # Set variables
   # resourceGroupName=myResourceGroup-$RANDOM
   # serverName=mysqlserver-$RANDOM
   
   # Verify which server is secondary
   echo "Verifying which server is in the secondary role..."
   az sql failover-group list \
      --server $serverName \
      --resource-group $resourceGroupName
   ```

Failover auf den sekundären Server: 

   ```azurecli-interactive
   # Set variables
   # resourceGroupName=myResourceGroup-$RANDOM
   # drServerName=mysqlsecondary-$RANDOM
   # failoverGroupName=failovergrouptutorial-$RANDOM

   
   echo "Failing over group to the secondary server..."
   az sql failover-group set-primary \
      --name $failoverGroupName \
      --resource-group $resourceGroupName \
      --server $drServerName
   echo "Successfully failed failover group over to" $drServerName
   ```

Stellen Sie die Failovergruppe auf dem primären Server wieder her:

   ```azurecli-interactive
   # Set variables
   # resourceGroupName=myResourceGroup-$RANDOM
   # serverName=mysqlserver-$RANDOM
   # failoverGroupName=failovergrouptutorial-$RANDOM
   
   echo "Failing over group back to the primary server..."
   az sql failover-group set-primary \
      --name $failoverGroupName \
      --resource-group $resourceGroupName \
      --server $serverName
   echo "Successfully failed failover group back to" $serverName
   ```

In diesem Teil des Tutorials werden die folgenden Azure CLI-Cmdlets verwendet:

| Get-Help | Notizen |
|---|---|
| [az sql failover-group list](/cli/azure/sql/failover-group?view=azure-cli-latest#az-sql-failover-group-list) | Listet die Failovergruppen eines Servers auf. |
| [az sql failover-group set-primary](/cli/azure/sql/failover-group?view=azure-cli-latest#az-sql-failover-group-set-primary) | Legt die primäre Failovergruppe fest, indem für alle Datenbanken auf dem aktuellen primären Server ein Failover ausgeführt wird. | 

---

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen 
Bereinigen Sie die Ressourcen, indem Sie die Ressourcengruppe löschen. 

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)
Löschen Sie die Ressourcengruppe über das Azure-Portal. 

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrer Ressourcengruppe.
1. Wählen Sie **Ressourcengruppe löschen** aus, um alle Ressourcen in der Gruppe sowie die Ressourcengruppe selbst zu löschen. 
1. Geben Sie den Namen der Ressourcengruppe `myResourceGroup` in das Textfeld ein, und wählen Sie dann **Löschen**, um die Ressourcengruppe zu löschen.  

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Löschen Sie die Ressourcengruppe mit PowerShell. 


   ```powershell-interactive
   # Set variables
   # $resourceGroupName = "myResourceGroup-$(Get-Random)"

   # Remove the resource group
   Write-host "Removing resource group..."
   Remove-AzResourceGroup -ResourceGroupName $resourceGroupName
   Write-host "Resource group removed =" $resourceGroupName
   ```

In diesem Teil des Tutorials werden die folgenden PowerShell-Cmdlets verwendet:

| Get-Help | Notizen |
|---|---|
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Entfernt eine Ressourcengruppe. | 

# <a name="azure-clitabazure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Löschen Sie die Ressourcengruppe mithilfe der Azure CLI. 


   ```azurecli-interactive
   # Set variables
   # resourceGroupName=myResourceGroup-$RANDOM
   
   # Clean up resources by removing the resource group
   echo "Cleaning up resources by removing the resource group..."
   az group delete \
     --name $resourceGroupName
   echo "Successfully removed resource group" $resourceGroupName
   ```

In diesem Teil des Tutorials werden die folgenden Azure CLI-Cmdlets verwendet:

| Get-Help | Notizen |
|---|---|
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#az-vm-extension-set) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

---


## <a name="full-scripts"></a>Vollständige Skripts

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

[!code-powershell-interactive[main](../../powershell_scripts/sql-database/failover-groups/add-single-db-to-failover-group-az-ps.ps1 "Add single database to a failover group")]

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [New-AzSqlServer](/powershell/module/az.sql/new-azsqlserver) | Erstellt einen SQL-Datenbank-Server, der Einzeldatenbanken und Pools für elastische Datenbanken hostet. |
| [New-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule) | Erstellt eine Firewallregel für einen logischen Server. | 
| [New-AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase) | Erstellt eine neue Azure SQL-Einzeldatenbank. | 
| [New-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/new-azsqldatabasefailovergroup) | Erstellt eine neue Failovergruppe. |
| [Get-AzSqlDatabase](/powershell/module/az.sql/get-azsqldatabase) | Ruft mindestens eine SQL-Datenbank ab. |
| [Add-AzSqlDatabaseToFailoverGroup](/powershell/module/az.sql/add-azsqldatabasetofailovergroup) | Fügt einer Failovergruppe mindestens eine Azure SQL-Datenbank hinzu. |
| [Get-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/get-azsqldatabasefailovergroup) | Ruft Azure SQL-Datenbank-Failovergruppen ab oder listet diese auf. |
| [Switch-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/switch-azsqldatabasefailovergroup)| Führt ein Failover für eine Azure SQL-Datenbank-Failovergruppe aus. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Entfernt eine Ressourcengruppe. | 

# <a name="azure-clitabazure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

[!code-azurecli-interactive[main](../../cli_scripts/sql-database/failover-groups/add-single-db-to-failover-group-az-cli.sh "Add single database to a failover group")]

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az account set](/cli/azure/account?view=azure-cli-latest#az-account-set) | Legt ein Abonnement als aktuelles aktives Abonnement fest. | 
| [az group create](/cli/azure/group#az-group-create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az sql server create](/cli/azure/sql/server#az-sql-server-create) | Erstellt einen SQL-Datenbank-Server, der Einzeldatenbanken und Pools für elastische Datenbanken hostet. |
| [az sql server firewall-rule create](/cli/azure/sql/server/firewall-rule) | Erstellt die Firewallregeln eines Servers. | 
| [az sql db create](/cli/azure/sql/db?view=azure-cli-latest) | Erstellt eine Datenbank. | 
| [az sql failover-group create](/cli/azure/sql/failover-group?view=azure-cli-latest#az-sql-failover-group-create) | Erstellt eine Failovergruppe. | 
| [az sql failover-group list](/cli/azure/sql/failover-group?view=azure-cli-latest#az-sql-failover-group-list) | Listet die Failovergruppen eines Servers auf. |
| [az sql failover-group set-primary](/cli/azure/sql/failover-group?view=azure-cli-latest#az-sql-failover-group-set-primary) | Legt die primäre Failovergruppe fest, indem für alle Datenbanken auf dem aktuellen primären Server ein Failover ausgeführt wird. | 
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#az-vm-extension-set) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)
Es sind keine Skripts für das Azure-Portal verfügbar. 
 
---

Weitere Azure SQL-Datenbank-Skripts finden Sie hier: [Azure PowerShell](sql-database-powershell-samples.md) und [Azure CLI](sql-database-cli-samples.md). 

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine Azure SQL-Einzeldatenbank zu einer Failovergruppe hinzugefügt und das Failover getestet. Es wurde Folgendes vermittelt: 

> [!div class="checklist"]
> - Erstellen einer Azure SQL-Einzeldatenbank. 
> - Erstellen einer [Failovergruppe](sql-database-auto-failover-group.md) für eine Einzeldatenbank zwischen zwei logischen SQL Server-Instanzen.
> - Testen des Failovers.

Fahren Sie mit dem nächsten Tutorial fort, in dem Sie erfahren, wie Sie einen Pool für elastische Datenbanken einer Failovergruppe hinzufügen. 

> [!div class="nextstepaction"]
> [Tutorial: Hinzufügen eines Pools für elastische Azure SQL-Datenbank-Instanzen zu einer Failovergruppe](sql-database-elastic-pool-failover-group-tutorial.md)
