---
title: Bereitstellen von Azure-SSIS Integration Runtime mit PowerShell
description: Hier erfahren Sie, wie Sie zur Bereitstellung und Ausführung von SSIS-Paketen in Azure die Azure-SSIS-Integration Runtime mit PowerShell in Azure Data Factory bereitstellen.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: tutorial
ms.date: 06/26/2019
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 594cdd848a3712d2164616e38f7b75a01a21b7f6
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73683591"
---
# <a name="provision-the-azure-ssis-integration-runtime-in-azure-data-factory-with-powershell"></a>Bereitstellen der Azure-SSIS Integration Runtime in Azure Data Factory mit PowerShell

In diesem Tutorial werden die Schritte für die Bereitstellung einer Azure-SQL Server Integration Services (SSIS) Integration Runtime (IR) in Azure Data Factory (ADF) beschrieben. Azure-SSIS IR unterstützt die Ausführung von Paketen, die im SSIS-Katalog (SSISDB) bereitgestellt und vom Azure SQL-Datenbank-Server bzw. einer verwalteten Instanz (Projektbereitstellungsmodell) gehostet werden, und von Paketen, die in Dateisystemen/Dateifreigaben/Azure Files (Projektbereitstellungsmodell) bereitgestellt werden. Sobald Azure-SSIS IR bereitgestellt wurde, können Sie vertraute Tools wie SQL Server Data Tools (SSDT)/SQL Server Management Studio (SSMS) und Befehlszeilenprogramme wie `dtinstall`/`dtutil`/`dtexec` verwenden, um Ihre Pakete in Azure bereitzustellen und auszuführen. In diesem Tutorial führen Sie die folgenden Schritte aus:

> [!NOTE]
> In diesem Artikel wird Azure PowerShell zum Bereitstellen einer Azure-SSIS IR-Instanz verwendet. Informationen zum Bereitstellen einer Azure-SSIS IR-Instanz über das Azure-Portal und die ADF-App finden Sie unter [Tutorial: Bereitstellen von Azure-SSIS IR](tutorial-create-azure-ssis-runtime-portal.md). 

> [!div class="checklist"]
> * Erstellen einer Data Factory.
> * Erstellen einer Azure SSIS Integration Runtime
> * Starten der Azure SSIS Integration Runtime
> * Überprüfen des kompletten Skripts
> * Bereitstellen von SSIS-Paketen

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

- **Azure-Abonnement**. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen. Konzeptionelle Informationen zur Azure-SSIS-IR finden Sie unter [Azure-SSIS Integration Runtime](concepts-integration-runtime.md#azure-ssis-integration-runtime).
- **Azure SQL-Datenbank-Server (optional)** . Wenn Sie noch nicht über einen Datenbankserver verfügen, erstellen Sie einen im Azure-Portal, bevor Sie beginnen. ADF erstellt SSISDB für diesen Datenbankserver. Es empfiehlt sich, den Datenbankserver in derselben Azure-Region zu erstellen, in der sich auch die Integration Runtime befindet. Diese Konfiguration ermöglicht es der Integration Runtime, Ausführungsprotokolle in SSISDB zu schreiben, ohne Azure-Regionen zu überschreiten. 
    - Basierend auf dem ausgewählten Datenbankserver kann SSISDB in Ihrem Auftrag als einzelne Datenbank, als Teil eines Pools für elastische Datenbanken oder in einer verwalteten Instanz erstellt werden und in einem öffentlichen Netzwerk oder durch das Hinzufügen zu einem virtuellen Netzwerk zugänglich sein. Einen Leitfaden zum Auswählen des Datenbankservertyps zum Hosten der SSISDB finden Sie unter [Vergleich zwischen SQL-Datenbank mit Einzeldatenbank/Pool für elastische Datenbanken und verwalteter SQL-Datenbank-Instanz](../data-factory/create-azure-ssis-integration-runtime.md#comparison-of-a-sql-database-single-database-elastic-pool-and-managed-instance). Wenn Sie den Azure SQL-Datenbank-Server mit VNET-Dienstendpunkten oder mit einer verwalteten Instanz in einem virtuellen Netzwerk zum Hosten von SSISDB verwenden oder Zugriff auf lokale Daten benötigen, müssen Sie Azure-SSIS IR einem virtuellen Netzwerk hinzufügen. Weitere Informationen finden Sie unter [Erstellen von Azure-SSIS IR in einem virtuellen Netzwerk](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).
    - Vergewissern Sie sich, dass die Einstellung **Zugriff auf Azure-Dienste zulassen** für den Datenbankserver aktiviert ist. Dies gilt nicht, wenn Sie einen Azure SQL-Datenbank-Server mit VNET-Dienstendpunkten oder einer verwalteten Instanz in einem virtuellen Netzwerk zum Hosten von SSISDB verwenden. Weitere Informationen finden Sie unter [Schützen der Azure SQL-Datenbank](../sql-database/sql-database-security-tutorial.md#create-firewall-rules). Informationen zum Aktivieren dieser Einstellung mithilfe von PowerShell finden Sie unter [New-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule).
    - Fügen Sie der Client-IP-Adressenliste in den Firewalleinstellungen für den Datenbankserver die IP-Adresse des Clientcomputers oder einen IP-Adressbereich hinzu, der die IP-Adresse des Clientcomputers enthält. Weitere Informationen finden Sie unter [Firewallregeln auf Serverebene und Datenbankebene für Azure SQL-Datenbank](../sql-database/sql-database-firewall-configure.md).
    - Sie können eine Verbindung mit dem Datenbankserver herstellen, indem Sie die SQL-Authentifizierung mit Ihren Serveradministrator-Anmeldeinformationen oder die Azure Active Directory-Authentifizierung (AAD) mit der verwalteten Identität für Ihre ADF-Instanz verwenden.  Für Letztere müssen Sie die verwaltete Identität für Ihre Azure Data Factory-Instanz einer AAD-Gruppe mit Zugriffsberechtigungen für den Datenbankserver hinzufügen. Weitere Informationen finden Sie unter [Erstellen einer Azure-SSIS Integration Runtime in Azure Data Factory](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).
    - Vergewissern Sie sich, dass Ihr Datenbankserver noch nicht über eine SSISDB verfügt. Für die Bereitstellung einer Azure-SSIS IR wird die Verwendung einer vorhandenen SSISDB nicht unterstützt.
- **Azure PowerShell**. Befolgen Sie die Anweisungen unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/install-Az-ps), wenn Sie ein PowerShell-Skript zur Bereitstellung von Azure-SSIS IR ausführen möchten.

> [!NOTE]
> - Eine Liste mit den Azure-Regionen, in denen ADF und Azure-SSIS IR derzeit verfügbar sind, finden Sie unter [ADF- und SSIS IR-Verfügbarkeit nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all). 

## <a name="launch-windows-powershell-ise"></a>Starten von Windows PowerShell ISE

Starten Sie **Windows PowerShell ISE** mit Administratorrechten. 

## <a name="create-variables"></a>Erstellen von Variablen

Kopieren Sie das folgende Skript, und fügen Sie es ein. Geben Sie Werte für die Variablen an. 

```powershell
### Azure Data Factory information 
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$"
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
# Data factory name - Must be globally unique
$DataFactoryName = "[your data factory name]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$DataFactoryLocation = "EastUS"

### Azure-SSIS integration runtime information - This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "[your Azure-SSIS IR name]"
$AzureSSISDescription = "[your Azure-SSIS IR description]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$AzureSSISLocation = "EastUS"
# For supported node sizes, see https://azure.microsoft.com/pricing/details/data-factory/ssis/
$AzureSSISNodeSize = "Standard_D8_v3"
# 1-10 nodes are currently supported
$AzureSSISNodeNumber = 2 
# Azure-SSIS IR edition/license info: Standard or Enterprise 
$AzureSSISEdition = "Standard" # Standard by default, while Enterprise lets you use advanced/premium features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "LicenseIncluded" # LicenseIncluded by default, while BasePrice lets you bring your own on-premises SQL Server license with Software Assurance to earn cost savings from Azure Hybrid Benefit (AHB) option
# For a Standard_D1_v2 node, up to 4 parallel executions per node are supported, but for other nodes, up to (2 x number of cores) are currently supported
$AzureSSISMaxParallelExecutionsPerNode = 8
# Custom setup info
$SetupScriptContainerSasUri = "" # OPTIONAL to provide SAS URI of blob container where your custom setup script and its associated files are stored

### SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name.database.windows.net or Managed Instance name.public.DNS prefix.database.windows.net,3342 or leave it empty if you do not use SSISDB]" # WARNING: If you use SSISDB, please ensure that there is no existing SSISDB on your database server, so we can prepare and manage one on your behalf    
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication]"
# For the basic pricing tier, specify "Basic", not "B" - For standard/premium/elastic pool tiers, specify "S0", "S1", "S2", "S3", etc., see https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-database-server
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>) for Azure SQL Database server or leave it empty for Managed Instance]"
```

## <a name="sign-in-and-select-subscription"></a>Anmelden und Auswählen des Abonnements

Fügen Sie dem Skript den folgenden Code hinzu, um sich anzumelden und Ihr Azure-Abonnement auszuwählen.

```powershell
Connect-AzAccount
Select-AzSubscription -SubscriptionName $SubscriptionName
```

## <a name="validate-the-connection-to-database-server"></a>Überprüfen der Verbindung mit dem Datenbankserver

Fügen Sie das folgende Skript hinzu, um den Azure SQL-Datenbank-Server zu überprüfen. 

```powershell
# Validate only if you use SSISDB
if(![string]::IsNullOrEmpty($SSISDBServerEndpoint))
{
    $SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID=" + $SSISDBServerAdminUserName + ";Password=" + $SSISDBServerAdminPassword    
    $sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
    Try
    {
        $sqlConnection.Open();
    }
    Catch [System.Data.SqlClient.SqlException]
    {
        Write-Warning "Cannot connect to your Azure SQL Database server, exception: $_";
        Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
        $yn = Read-Host
        if(!($yn -ieq "Y"))
        {
            Return;
        } 
    }
}
```

Das folgende Beispiel veranschaulicht die Erstellung einer Azure SQL-Datenbank-Instanz im Rahmen des Skripts: 

Legen Sie Werte für die noch nicht definierten Variablen fest. Beispiel:  SSISDBServerName, FirewallIPAddress. 

```powershell
New-AzSqlServer -ResourceGroupName $ResourceGroupName `
    -ServerName $SSISDBServerName `
    -Location $DataFactoryLocation `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $SSISDBServerAdminUserName, $(ConvertTo-SecureString -String $SSISDBServerAdminPassword -AsPlainText -Force))    

New-AzSqlServerFirewallRule -ResourceGroupName $ResourceGroupName `
    -ServerName $SSISDBServerName `
    -FirewallRuleName "ClientIPAddress_$today" -StartIpAddress $FirewallIPAddress -EndIpAddress $FirewallIPAddress

New-AzSqlServerFirewallRule -ResourceGroupName $ResourceGroupName -ServerName $SSISDBServerName -AllowAllAzureIPs
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit dem Befehl [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) eine [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md). Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und als Gruppe verwaltet werden.

Wenn die Ressourcengruppe bereits vorhanden ist, müssen Sie diesen Code nicht in das Skript kopieren. 

```powershell
New-AzResourceGroup -Location $DataFactoryLocation -Name $ResourceGroupName
```

## <a name="create-a-data-factory"></a>Erstellen einer Data Factory

Führen Sie den folgenden Befehl aus, um eine Data Factory zu erstellen:

```powershell
Set-AzDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName
```

## <a name="create-an-integration-runtime"></a>Erstellen einer Integration Runtime

Führen Sie die folgenden Befehle aus, um eine Azure-SSIS-Integration Runtime zu erstellen, die SSIS-Pakete in Azure ausführt.

Wenn Sie SSISDB nicht verwenden, können Sie die Parameter CatalogServerEndpoint, CatalogPricingTier und CatalogAdminCredential auslassen.

```powershell
Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Name $AzureSSISName `
                                           -Description $AzureSSISDescription `
                                           -Type Managed `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode

# Add CatalogServerEndpoint, CatalogPricingTier, and CatalogAdminCredential parameters if you use SSISDB
if(![string]::IsNullOrEmpty($SSISDBServerEndpoint))
{
    $secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
    $serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)

    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -CatalogServerEndpoint $SSISDBServerEndpoint `
                                               -CatalogPricingTier $SSISDBPricingTier `
                                               -CatalogAdminCredential $serverCreds
}

# Add SetupScriptContainerSasUri parameter if you use custom setup
if(![string]::IsNullOrEmpty($SetupScriptContainerSasUri))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -SetupScriptContainerSasUri $SetupScriptContainerSasUri
}
```

## <a name="start-integration-runtime"></a>Starten der Integration Runtime

Führen Sie die folgenden Befehle aus, um Azure-SSIS Integration Runtime zu starten.

```powershell
write-host("##### Starting #####")
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")                                  
```

> [!NOTE]
> Abgesehen von einer benutzerdefinierten Einrichtungszeit sollte dieser Prozess innerhalb von 5 Minuten abgeschlossen sein.
>
> Bei Verwendung von SSISDB stellt der ADF-Dienst eine Verbindung mit Ihrem Datenbankserver her, um SSISDB vorzubereiten. 
> 
> Wenn Sie Azure-SSIS IR bereitstellen, werden auch die Access Redistributable-Komponente und das Azure Feature Pack für SSIS installiert. Diese Komponenten ermöglichen nicht nur die Konnektivität mit Excel- und Access-Dateien und verschiedenen Azure-Datenquellen, sondern auch mit den Datenquellen, die bereits von integrierten Komponenten unterstützt werden. Sie können auch weitere Komponenten installieren. Weitere Informationen dazu finden Sie unter [Benutzerdefiniertes Setup für Azure-SSIS IR](how-to-configure-azure-ssis-ir-custom-setup.md).

## <a name="full-script"></a>Vollständiges Skript

Mit dem PowerShell-Skript in diesem Abschnitt wird eine Instanz von Azure-SSIS IR konfiguriert, die SSIS-Pakete ausführt. Nachdem Sie dieses Skript erfolgreich ausgeführt haben, können Sie SSIS-Pakete in Azure bereitstellen und ausführen.

1. Starten Sie die Windows PowerShell Integrated Scripting Environment (ISE).
2. Führen Sie in der ISE den folgenden Befehl an der Eingabeaufforderung aus.  

    ```powershell
    Set-ExecutionPolicy Unrestricted -Scope CurrentUser
    ```

3. Kopieren Sie das PowerShell-Skript in diesem Abschnitt, und fügen Sie es in die ISE ein.
4. Geben Sie am Anfang des Skripts geeignete Werte für alle Parameter an.
5. Führen Sie das Skript aus. 

```powershell
### Azure Data Factory information 
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$"
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
# Data factory name - Must be globally unique
$DataFactoryName = "[your data factory name]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$DataFactoryLocation = "EastUS"

### Azure-SSIS integration runtime information - This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "[your Azure-SSIS IR name]"
$AzureSSISDescription = "[your Azure-SSIS IR description]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$AzureSSISLocation = "EastUS"
# For supported node sizes, see https://azure.microsoft.com/pricing/details/data-factory/ssis/
$AzureSSISNodeSize = "Standard_D8_v3"
# 1-10 nodes are currently supported
$AzureSSISNodeNumber = 2 
# Azure-SSIS IR edition/license info: Standard or Enterprise 
$AzureSSISEdition = "Standard" # Standard by default, while Enterprise lets you use advanced/premium features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "LicenseIncluded" # LicenseIncluded by default, while BasePrice lets you bring your own on-premises SQL Server license with Software Assurance to earn cost savings from Azure Hybrid Benefit (AHB) option
# For a Standard_D1_v2 node, up to 4 parallel executions per node are supported, but for other nodes, up to (2 x number of cores) are currently supported
$AzureSSISMaxParallelExecutionsPerNode = 8
# Custom setup info
$SetupScriptContainerSasUri = "" # OPTIONAL to provide SAS URI of blob container where your custom setup script and its associated files are stored

### SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name.database.windows.net or Managed Instance name.public.DNS prefix.database.windows.net,3342 or leave it empty if you do not use SSISDB]" # WARNING: If you want to use SSISDB, please ensure that there is no existing SSISDB on your database server, so we can prepare and manage one on your behalf    
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication]"
# For the basic pricing tier, specify "Basic", not "B" - For standard/premium/elastic pool tiers, specify "S0", "S1", "S2", "S3", etc., see https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-database-server
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>) for Azure SQL Database server or leave it empty for Managed Instance]"

### Sign in and select subscription
Connect-AzAccount
Select-AzSubscription -SubscriptionName $SubscriptionName

### Validate the connection to database server
# Validate only if you use SSISDB
if(![string]::IsNullOrEmpty($SSISDBServerEndpoint))
{
    $SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID=" + $SSISDBServerAdminUserName + ";Password=" + $SSISDBServerAdminPassword    
    $sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
    Try
    {
        $sqlConnection.Open();
    }
    Catch [System.Data.SqlClient.SqlException]
    {
        Write-Warning "Cannot connect to your Azure SQL Database server, exception: $_";
        Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
        $yn = Read-Host
        if(!($yn -ieq "Y"))
        {
            Return;
        } 
    }
}

### Create a data factory
Set-AzDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName

### Create an integration runtime
Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Name $AzureSSISName `
                                           -Description $AzureSSISDescription `
                                           -Type Managed `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode

# Add CatalogServerEndpoint, CatalogPricingTier, and CatalogAdminCredential parameters if you use SSISDB
if(![string]::IsNullOrEmpty($SSISDBServerEndpoint))
{
    $secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
    $serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)

    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -CatalogServerEndpoint $SSISDBServerEndpoint `
                                               -CatalogPricingTier $SSISDBPricingTier `
                                               -CatalogAdminCredential $serverCreds
}

# Add SetupScriptContainerSasUri parameter if you use custom setup
if(![string]::IsNullOrEmpty($SetupScriptContainerSasUri))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -SetupScriptContainerSasUri $SetupScriptContainerSasUri
}

### Start integration runtime
write-host("##### Starting #####")
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")   
```

## <a name="monitor-and-manage-azure-ssis-ir"></a>Überwachen und Verwalten einer Azure-SSIS-IR

In den folgenden Artikeln finden Sie ausführliche Informationen zum Überwachen und Verwalten einer Azure-SSIS-IR. 

- [Überwachen von Azure-SSIS IR](monitor-integration-runtime.md#azure-ssis-integration-runtime)
- [Verwalten von Azure-SSIS IR](manage-azure-ssis-integration-runtime.md)

## <a name="deploy-ssis-packages"></a>Bereitstellen von SSIS-Paketen

Wenn Sie SSISDB verwenden, können Sie Ihre Pakete darin bereitstellen und sie in Azure-SSIS IR mithilfe von SSDT/SSMS-Tools ausführen, die über den Serverendpunkt eine Verbindung mit Ihrem Datenbankserver herstellen. Für den Azure SQL-Datenbank-Server oder eine verwaltete Instanz mit einem öffentlichen Endpunkt wird jeweils das Serverendpunktformat `<server name>.database.windows.net`/`<server name>.public.<dns prefix>.database.windows.net,3342` verwendet. Wenn Sie SSISDB nicht verwenden, können Sie Ihre Pakete in Dateisystemen, Dateifreigaben und Azure Files bereitstellen und sie mithilfe von Befehlszeilenhilfsprogrammen (`dtinstall`/`dtutil`/`dtexec`) unter Azure-SSIS IR ausführen. Weitere Informationen finden Sie unter [Bereitstellen von SSIS-Paketen](/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages#deploy-packages-to-integration-services-server). In beiden Fällen können Sie Ihre bereitgestellten Pakete auch unter Azure-SSIS IR ausführen, indem Sie die Aktivität „SSIS-Paket ausführen“ in ADF-Pipelines verwenden. Weitere Informationen finden Sie unter [Aufrufen der SSIS-Paketausführung als erstklassige ADF-Aktivität](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).

Informationen finden Sie außerdem in den folgenden Artikeln der SSIS-Dokumentation: 

- [Bereitstellen und Ausführen eines SSIS-Pakets in Azure](/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial)   
- [Herstellen einer Verbindung mit dem SSIS-Katalog (SSISDB) in Azure](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database)
- [Planen der Ausführung von in Azure bereitgestellten SSIS-Paketen](/sql/integration-services/lift-shift/ssis-azure-schedule-packages)
- [Connect to on-premises data sources with Windows Authentication](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth) (Herstellen einer Verbindung mit lokalen Datenquellen mit Windows-Authentifizierung) 

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt: 

> [!div class="checklist"]
> * Erstellen einer Data Factory
> * Erstellen einer Azure SSIS Integration Runtime
> * Starten der Azure SSIS Integration Runtime
> * Überprüfen des kompletten Skripts
> * Bereitstellen von SSIS-Paketen

Informationen zum Anpassen Ihrer Azure-SSIS Integration Runtime finden Sie im folgenden Artikel:

> [!div class="nextstepaction"]
>[Anpassen des Setups für Azure-SSIS Integration Runtime](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup)