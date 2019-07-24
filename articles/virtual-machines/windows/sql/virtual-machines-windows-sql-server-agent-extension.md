---
title: Automatisieren von Verwaltungsaufgaben auf virtuellen Azure-Computern mit der Erweiterung für SQL Server-IaaS-Agent | Microsoft-Dokumentation
description: In diesem Artikel wird die Verwaltung der SQL Server-Agent-Erweiterung beschrieben, mit der bestimmte SQL Server-Verwaltungsaufgaben automatisiert werden. Dazu gehören die automatische Sicherung, das automatische Patchen und die Azure-Schlüsseltresor-Integration.
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
manager: jroth
editor: ''
tags: azure-resource-manager
ms.assetid: effe4e2f-35b5-490a-b5ef-b06746083da4
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/24/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 41023103dc30d16f599e847f9d324bc7bb4be11c
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67798051"
---
# <a name="automate-management-tasks-on-azure-virtual-machines-with-the-sql-server-iaas-agent-extension"></a>Automatisieren von Verwaltungsaufgaben auf virtuellen Azure-Computern mit der Erweiterung für SQL Server-IaaS-Agent
> [!div class="op_single_selector"]
> * [Ressourcen-Manager](virtual-machines-windows-sql-server-agent-extension.md)
> * [Klassisch](../sqlclassic/virtual-machines-windows-classic-sql-server-agent-extension.md)

Die Erweiterung für SQL Server-IaaS-Agent (SqlIaasExtension) wird auf virtuellen Azure-Computern zum Automatisieren von Verwaltungsaufgaben ausgeführt. Dieser Artikel enthält eine Übersicht und Informationen zu den Diensten, die von der Erweiterung unterstützt werden, sowie Anweisungen zu Installation, Status und Deinstallation.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Den Artikel zum klassischen Bereitstellungsmodell finden Sie unter [SQL Server-Agent-Erweiterung für virtuelle SQL Server-Computer (klassisch)](../sqlclassic/virtual-machines-windows-classic-sql-server-agent-extension.md).

Es gibt drei SQL-Verwaltbarkeitsmodi für die SQL-IaaS-Erweiterung: **Vollständig**, **Einfach** und **NoAgent**. 

- Im Modus **Vollständig** werden alle Funktionen geboten, jedoch sind ein Neustart von SQL Server und Systemadministratorberechtigungen erforderlich. Dies ist die Standardoption. Sie sollte für die Verwaltung eines SQL Server-VM mit einer einzelnen Instanz verwendet werden. 

- **Einfach** erfordert keinen Neustart von SQL Server, unterstützt jedoch nur das Ändern des Lizenztyps und der Edition von SQL Server. Diese Option sollte für SQL Server-VMs mit mehreren Instanzen oder für die Teilnahme an einer Failoverclusterinstanz (FCI) verwendet werden. 

- **NoAgent** ist speziell für Installationen von SQL Server 2008 und SQL Server 2008 R2 unter Windows Server 2008 vorgesehen. Informationen zur Verwendung des Modus `NoAgent` für ein Windows Server 2008-Image finden Sie unter [Registrierung von Windows Server 2008](virtual-machines-windows-sql-register-with-resource-provider.md#register-sql-server-2008r2-on-windows-server-2008-vms). 

## <a name="supported-services"></a>Unterstützte Dienste
Die Erweiterung für SQL Server-IaaS-Agent unterstützt die folgenden Verwaltungsaufgaben:

| Verwaltungsfeature | BESCHREIBUNG |
| --- | --- |
| **SQL – Automatisierte Sicherung** |Automatisiert die Planung von Sicherungen für alle Datenbanken entweder der Standardinstanz oder einer [ordnungsgemäß installierten](virtual-machines-windows-sql-server-iaas-faq.md#administration) benannten Instanz von SQL Server in der VM. Weitere Informationen finden Sie unter [Automatisierte Sicherung für SQL Server auf virtuellen Azure-Computern (Resource Manager)](virtual-machines-windows-sql-automated-backup.md). |
| **SQL – Automatisiertes Patchen** |Mit diesem Dienst können Sie ein Wartungsfenster konfigurieren, in dem wichtige Windows-Updates für den virtuellen Computer ausgeführt werden. Auf diese Weise werden Updates für Ihre Workload während der Spitzenzeiten vermieden. Weitere Informationen finden Sie unter [Automatisiertes Patchen für SQL Server auf virtuellen Azure-Computern (Resource Manager)](virtual-machines-windows-sql-automated-patching.md). |
| **Azure-Schlüsseltresor-Integration** |Mit diesem Dienst können Sie Azure Key Vault auf Ihrem virtuellen SQL Server-Computer automatisch installieren und konfigurieren. Weitere Informationen finden Sie unter [Konfigurieren der Azure Key Vault-Integration für SQL Server auf virtuellen Azure-Computern (Resource Manager)](virtual-machines-windows-ps-sql-keyvault.md). |

Nach der Installation und Ausführung stellt die SQL Server-IaaS-Agent-Erweiterung diese Verwaltungsfeatures im Bereich „SQL Server“ des virtuellen Computers im Azure-Portal bereit. Außerdem stehen sie für SQL Server-Marketplace-Images über Azure PowerShell und bei der manuellen Installation der Erweiterung über Azure PowerShell zur Verfügung. 

## <a name="prerequisites"></a>Voraussetzungen
Anforderungen für die Verwendung der Erweiterung für SQL Server-IaaS-Agent auf Ihrem virtuellen Computer:

**Betriebssystem:**

* Windows Server 2008 R2
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016
* Windows Server 2019 

**SQL Server-Versionen:**

* SQL Server 2008 
* SQL Server 2008 R2
* SQL Server 2012
* SQL Server 2014
* SQL Server 2016
* SQL Server 2017

**Azure PowerShell:**

* [Herunterladen und Konfigurieren der aktuellen Azure PowerShell-Befehle](/powershell/azure/overview)

[!INCLUDE [updated-for-az.md](../../../../includes/updated-for-az.md)]


## <a name="change-management-modes"></a>Change-Management-Modi

Sie können mit PowerShell den aktuellen Modus des SQL-IaaS-Agents anzeigen: 

  ```powershell-interactive
     //Get the SqlVirtualMachine
     $sqlvm = Get-AzResource -Name $vm.Name  -ResourceGroupName $vm.ResourceGroupName  -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines
     $sqlvm.Properties.sqlManagement
  ```

Für SQL Server-VMs, auf denen die IaaS-Erweiterung mit dem Modus *NoAgent* oder *Einfach* installiert ist, können Sie im Azure-Portal ein Upgrade des Modus auf *Vollständig* durchführen. Es ist nicht möglich, ein Downgrade durchzuführen. Stattdessen müssen Sie die SQL-IaaS-Erweiterung vollständig deinstallieren und erneut installieren. 

Gehen Sie wie folgt vor, um ein Upgrade des Agentmodus auf *Vollständig* durchzuführen: 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Navigieren Sie zur Ressource [Virtuelle SQL-Computer](virtual-machines-windows-sql-manage-portal.md#access-sql-virtual-machine-resource). 
1. Wählen Sie den virtuellen SQL Server-Computer aus, und wählen Sie **Übersicht**. 
1. Wählen Sie für SQL-VMs mit dem IaaS-Modus *NoAgent* oder *Einfach* die Nachricht für **Only license type and edition updates are available with the SQL IaaS extension** (Mit der SQL-IaaS-Erweiterung sind nur Lizenztyp- und Editionsaktualisierungen verfügbar) aus.

    ![Starten der Modusänderung über das Portal](media/virtual-machines-windows-sql-server-agent-extension/change-sql-iaas-mode-portal.png)

1. Stimmen Sie dem **Neustart des SQL Server-Diensts** zu, indem Sie das Kontrollkästchen aktivieren, und wählen Sie dann **Bestätigen** aus, um ein Upgrade des IaaS-Modus auf „Vollständig“ durchzuführen. 

    ![Aktivieren der vollständigen Verwaltung für die IaaS-Erweiterung](media/virtual-machines-windows-sql-server-agent-extension/enable-full-mode-iaas.png)

##  <a name="installation"></a>Installation
Die SQL-IaaS-Erweiterung wird installiert, wenn Sie den SQL Server-VM bei dem [SQL-VM-Ressourcenanbieter](virtual-machines-windows-sql-register-with-resource-provider.md#register-with-sql-vm-resource-provider) registrieren. Gegebenenfalls kann auch der SQL Server-IaaS-Agent manuell im Modus *Vollständig* oder *Einfach* installiert werden. 

Die Erweiterung für SQL Server-IaaS-Agent im Modus *Vollständig* wird automatisch installiert, wenn Sie mit dem Azure-Portal eines der Katalogimages für virtuelle SQL Server-Computer bereitstellen. 

### <a name="full-mode-installation"></a>Installation im Modus „Vollständig“
Die SQL-IaaS-Erweiterung im Modus *Vollständig* ermöglicht die vollständige Verwaltbarkeit für eine einzelne Instanz in der SQL Server-VM. Wenn es eine Standardinstanz gibt, arbeitet die Erweiterung mit der Standardinstanz und unterstützt nicht die Verwaltung anderer Instanzen. Wenn es keine Standardinstanz, sondern nur eine benannte Instanz gibt, wird die benannte Instanz verwaltet. Wenn es keine Standardinstanz und mehrere benannte Instanzen gibt, kann die Erweiterung nicht installiert werden. 

Beim Installieren des Modus *Vollständig* der SQL-IaaS-Erweiterung wird der SQL Server-Dienst neu gestartet. Wenn Sie den Neustart des SQL Server-Diensts vermeiden möchten, installieren Sie stattdessen den Modus *Einfach* mit beschränkter Verwaltbarkeit. 

Installieren des SQL-IaaS-Agents im Modus *Vollständig* mit PowerShell:

  ```powershell-interactive
     // Get the existing  Compute VM
     $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
     // Register SQL VM with 'Full' SQL IaaS agent
     New-AzResource -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
        -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines `
        -Properties @{virtualMachineResourceId=$vm.Id;sqlServerLicenseType='AHUB';sqlManagement='Full'}  
  
  ```

| Parameter | Zulässige Werte                        |
| :------------------| :-------------------------------|
| **sqlServerLicenseType** | `'AHUB'` oder `'PAYG'`     |
| &nbsp;             | &nbsp;                          |


> [!WARNING]
> - Wenn die Erweiterung noch nicht installiert ist, wird der SQL Server-Dienst durch die Installation der Erweiterung **Vollständig** neu gestartet. Verwenden Sie den Modus **Einfach**, um den Neustart des SQL Server-Diensts zu vermeiden. 
> - Das Aktualisieren der SQL-IaaS-Erweiterung bewirkt nicht, dass der SQL Server-Dienst neu gestartet wird. 

#### <a name="install-on-a-vm-with-a-single-named-sql-server-instance"></a>Installieren eines virtuellen Computers mit einer einzelnen benannten SQL Server-Instanz
Die SQL-IaaS-Erweiterung funktioniert mit einer benannten Instanz auf einem SQL Server, wenn die Standardinstanz deinstalliert und die IaaS-Erweiterung neu installiert wird.

Um eine benannte Instanz von SQL Server zu verwenden, gehen Sie folgendermaßen vor:
   1. Stellen Sie eine SQL Server-VM aus Marketplace bereit. 
   1. Deinstallieren Sie die IaaS-Erweiterung im [Azure-Portal](https://portal.azure.com).
   1. Deinstallieren Sie SQL Server in der SQL Server-VM vollständig.
   1. Installieren Sie SQL Server mit einer benannten Instanz in der SQL Server-VM. 
   1. Installieren Sie die IaaS-Erweiterung im Azure-Portal.  


### <a name="install-in-lightweight-mode"></a>Installieren im Modus „Einfach“
Im Modus „Einfach“ erfolgt kein Neustart des SQL Server-Diensts, der Modus bietet jedoch eingeschränkte Funktionalität. 

Installieren des SQL-IaaS-Agents im Modus *Einfach* mit PowerShell:


  ```powershell-interactive
     // Get the existing  Compute VM
     $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
     // Register SQL VM with 'Lightweight' SQL IaaS agent
     New-AzResource -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
        -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines `
        -Properties @{virtualMachineResourceId=$vm.Id;sqlServerLicenseType='AHUB';sqlManagement='LightWeight'}  
  
  ```

| Parameter | Zulässige Werte                        |
| :------------------| :-------------------------------|
| **sqlServerLicenseType** | `'AHUB'` oder `'PAYG'`     |
| &nbsp;             | &nbsp;                          |


## <a name="get-status-of-sql-iaas-extension"></a>Abrufen des Status der SQL-IaaS-Erweiterung
Eine Möglichkeit, zu überprüfen, ob die Erweiterung installiert ist, ist das Anzeigen des Agent-Status im Azure-Portal. Wählen Sie im Fenster des virtuellen Computers die Option **Alle Einstellungen** aus, und klicken Sie dann auf **Erweiterungen**. Die Erweiterung **SqlIaasExtension** sollte aufgeführt sein.

![Erweiterung für SQL Server-IaaS-Agent im Azure-Portal](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-portal.png)

Sie können auch das Azure PowerShell-Cmdlet **Get-AzVMSqlServerExtension** verwenden.

   ```powershell-interactive
   Get-AzVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
   ```

Mit dem Befehl oben wird überprüft, ob der Agent installiert ist. Zudem werden allgemeine Statusinformationen angegeben. Mit den folgenden Befehlen können Sie zudem bestimmte Statusinformationen zur automatischen Sicherung und zum automatischen Patchen abrufen.

   ```powershell-interactive
    $sqlext = Get-AzVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
    $sqlext.AutoPatchingSettings
    $sqlext.AutoBackupSettings
   ```

## <a name="removal"></a>Entfernen
Im Azure-Portal können Sie die Erweiterung deinstallieren, indem Sie im Fenster **Erweiterungen** in den Eigenschaften des virtuellen Computers auf die Auslassungspunkte klicken. Klicken Sie dann auf **Löschen**.

![Deinstallieren der Erweiterung für SQL Server-IaaS-Agent im Azure-Portal](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-uninstall.png)

Sie können auch das PowerShell-Cmdlet **Remove-AzVMSqlServerExtension** verwenden.

   ```powershell-interactive
    Remove-AzVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SqlIaasExtension"
   ```

## <a name="next-steps"></a>Nächste Schritte
Verwenden Sie einen der von der Erweiterung unterstützten Dienste. Weitere Informationen finden Sie in den Artikeln, die im Abschnitt [Unterstützte Dienste](#supported-services) dieses Artikels genannt werden.

Ausführlichere Informationen zur Verwendung von SQL Server auf virtuellen Azure-Computern finden Sie unter [Übersicht zu SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-server-iaas-overview.md).

