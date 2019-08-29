---
title: Automatisieren von Verwaltungsaufgaben auf virtuellen Azure-Computern mit der SQL Server-IaaS-Agent-Erweiterung | Microsoft-Dokumentation
description: In diesem Artikel wird die Verwaltung der SQL Server-IaaS-Agent-Erweiterung beschrieben, die bestimmte SQL Server-Verwaltungsaufgaben automatisiert. Dazu gehören die automatische Sicherung, das automatische Patchen und die Azure Key Vault-Integration.
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
manager: jroth
editor: ''
tags: azure-resource-manager
ms.assetid: effe4e2f-35b5-490a-b5ef-b06746083da4
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/24/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: f4dd529481a6216e43d35c76ecee734543d487f3
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70100479"
---
# <a name="automate-management-tasks-on-azure-virtual-machines-by-using-the-sql-server-iaas-agent-extension"></a>Automatisieren von Verwaltungsaufgaben auf virtuellen Azure-Computern mit der SQL Server-IaaS-Agent-Erweiterung
> [!div class="op_single_selector"]
> * [Ressourcen-Manager](virtual-machines-windows-sql-server-agent-extension.md)
> * [Klassisch](../sqlclassic/virtual-machines-windows-classic-sql-server-agent-extension.md)

Die Erweiterung für SQL Server-IaaS-Agent (SqlIaasExtension) wird auf virtuellen Azure-Computern zum Automatisieren von Verwaltungsaufgaben ausgeführt. Dieser Artikel bietet eine Übersicht über die Dienste, die von dieser Erweiterung unterstützt werden. Er enthält auch Anweisungen zu Installation, Statusanzeige und Entfernung der Erweiterung.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Den Artikel zum klassischen Bereitstellungsmodell finden Sie unter [SQL Server-IaaS-Agent-Erweiterung für virtuelle SQL Server-Computer (klassisch)](../sqlclassic/virtual-machines-windows-classic-sql-server-agent-extension.md).

Es gibt drei Verwaltungsmodi für die SQL Server-IaaS-Erweiterung: 

- Der Modus **Vollständig** bietet alle Funktionen, erfordert aber einen Neustart des SQL Server-Computers sowie Systemadministratorberechtigungen. Diese Option wird standardmäßig installiert. Verwenden Sie diesen Modus zum Verwalten einer SQL Server-VM mit einer einzelnen Instanz. 

- **Lightweight** erfordert keinen Neustart von SQL Server, unterstützt jedoch nur das Ändern des Lizenztyps und der Edition von SQL Server. Verwenden Sie diese Option für SQL Server-VMs mit mehreren Instanzen oder für die Teilnahme an einer Failoverclusterinstanz (FCI). 

- **NoAgent** ist speziell für Installationen von SQL Server 2008 und SQL Server 2008 R2 unter Windows Server 2008 vorgesehen. Informationen zur Verwendung dieses Modus für Ihr Windows Server 2008-Image finden Sie unter [Registrierung bei Windows Server 2008](virtual-machines-windows-sql-register-with-resource-provider.md#register-sql-server-2008-or-2008-r2-on-windows-server-2008-vms). 

## <a name="supported-services"></a>Unterstützte Dienste
Die Erweiterung für SQL Server-IaaS-Agent unterstützt die folgenden Verwaltungsaufgaben:

| Verwaltungsfeature | BESCHREIBUNG |
| --- | --- |
| **Automatisierte Sicherung für SQL Server** |Automatisiert die Planung von Sicherungen für alle Datenbanken entweder der Standardinstanz oder einer [ordnungsgemäß installierten](virtual-machines-windows-sql-server-iaas-faq.md#administration) benannten Instanz von SQL Server in der VM. Weitere Informationen finden Sie unter [Automatisierte Sicherung für SQL Server auf virtuellen Azure-Computern (Resource Manager)](virtual-machines-windows-sql-automated-backup.md). |
| **Automatisierte Patches für SQL Server** |Mit diesem Dienst können Sie ein Wartungsfenster konfigurieren, in dem wichtige Windows-Updates für den virtuellen Computer ausgeführt werden. Auf diese Weise werden Updates für Ihre Workload während der Spitzenzeiten vermieden. Weitere Informationen finden Sie unter [Automatisiertes Patchen für SQL Server auf virtuellen Azure-Computern (Resource Manager)](virtual-machines-windows-sql-automated-patching.md). |
| **Azure Key Vault-Integration** |Mit diesem Dienst können Sie Azure Key Vault auf Ihrem virtuellen SQL Server-Computer automatisch installieren und konfigurieren. Weitere Informationen finden Sie unter [Konfigurieren der Azure Key Vault-Integration für SQL Server auf virtuellen Azure-Computern (Resource Manager)](virtual-machines-windows-ps-sql-keyvault.md). |

Wenn die SQL Server-IaaS-Agent-Erweiterung installiert wurde und ausgeführt wird, stellt sie Verwaltungsfeatures folgendermaßen zur Verfügung:

* Im Bereich „SQL Server“ des virtuellen Computers im Azure-Portal sowie über Azure PowerShell für SQL Server-Images im Azure Marketplace.
* Über Azure PowerShell bei manuellen Installationen der Erweiterung. 

## <a name="prerequisites"></a>Voraussetzungen
Anforderungen für die Verwendung der SQL Server-IaaS-Agent-Erweiterung auf Ihrem virtuellen Computer:

**Betriebssystem**:

* Windows Server 2008 R2
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016
* Windows Server 2019 

**SQL Server-Version:**

* SQL Server 2008 
* SQL Server 2008 R2
* SQL Server 2012
* SQL Server 2014
* SQL Server 2016
* SQL Server 2017

**Azure PowerShell:**

* [Herunterladen und Konfigurieren der aktuellen Azure PowerShell-Befehle](/powershell/azure/overview)

[!INCLUDE [updated-for-az.md](../../../../includes/updated-for-az.md)]


## <a name="change-management-modes"></a>Change Management-Modi

Über PowerShell können Sie den aktuellen Modus des SQL Server-IaaS-Agents anzeigen: 

  ```powershell-interactive
     #Get the SqlVirtualMachine
     $sqlvm = Get-AzResource -Name $vm.Name  -ResourceGroupName $vm.ResourceGroupName  -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines
     $sqlvm.Properties.sqlManagement
  ```

Für SQL Server-VMs, auf denen die IaaS-Erweiterung im Modus *Lightweight* installiert ist, können Sie im Azure-Portal ein Upgrade auf den Modus _Vollständig_ durchführen. Für SQL Server-VMs im Modus _NoAgent_ kann ein Upgrade auf _Vollständig_ durchgeführt werden, nachdem das Betriebssystem auf Windows 2008 R2 oder höher aktualisiert wurde. Es ist nicht möglich, ein Downgrade durchzuführen. Stattdessen müssen Sie die SQL-IaaS-Erweiterung vollständig deinstallieren und erneut installieren. 

So führen Sie Upgrade des Agent-Modus auf „Vollständig“ durch: 


# <a name="azure-portaltabazure-portal"></a>[Azure-Portal](#tab/azure-portal)

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wechseln Sie zu Ihrer Ressource [Virtuelle SQL-Computer](virtual-machines-windows-sql-manage-portal.md#access-the-sql-virtual-machines-resource). 
1. Wählen Sie Ihren virtuellen SQL Server-Computer aus, und klicken Sie auf **Übersicht**. 
1. Wählen Sie für SQL Server-VMs mit dem IaaS-Modus „NoAgent“ oder „Lightweight“ die Meldung **Mit der SQL-IaaS-Erweiterung sind nur Lizenztyp- und Editionsaktualisierungen verfügbar** aus.

   ![Auswahlmöglichkeiten zum Ändern des Modus im Portal](media/virtual-machines-windows-sql-server-agent-extension/change-sql-iaas-mode-portal.png)

1. Aktivieren Sie das Kontrollkästchen **Ich stimme dem Neustart des SQL Server-Diensts auf dem virtuellen Computer** zu, und klicken Sie dann auf **Bestätigen**, um ein Upgrade des IaaS-Modus auf „Vollständig“ durchzuführen. 

    ![Kontrollkästchen für die Zustimmung zum Neustart des SQL Server-Diensts auf dem virtuellen Computer](media/virtual-machines-windows-sql-server-agent-extension/enable-full-mode-iaas.png)

# <a name="az-clitabbash"></a>[Azure CLI](#tab/bash)

Führen Sie den folgenden Codeausschnitt in der Azure CLI aus:

  ```azurecli-interactive
  # Update to full mode

  az sql vm update --name <vm_name> --resource-group <resource_group_name> --sql-mgmt-type full  
  ```

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)

Führen Sie den folgenden Codeausschnitt in PowerShell aus:

  ```powershell-interactive
  # Update to full mode

  $SqlVm = Get-AzResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
  $SqlVm.Properties.sqlManagement="Full"
  $SqlVm | Set-AzResource -Force
  ```

---


##  <a name="installation"></a>Installation
Die SQL Server-IaaS-Erweiterung wird installiert, wenn Sie Ihre SQL Server-VM beim [SQL-VM-Ressourcenanbieter](virtual-machines-windows-sql-register-with-resource-provider.md) registrieren. Falls erforderlich, können Sie den SQL Server-IaaS-Agent im vollständigen oder Lightweight-Modus manuell installieren. 

Die SQL Server-IaaS-Agent-Erweiterung im vollständigen Modus wird automatisch installiert, wenn Sie eins der Azure Marketplace-Images für virtuelle SQL Server-Computer über das Azure-Portal bereitstellen. 

### <a name="install-in-full-mode"></a>Installation im vollständigen Modus
Der vollständige Modus der SQL Server-IaaS-Erweiterung bietet alle Verwaltungsfunktionen für eine einzelne Instanz auf der SQL Server-VM. Wenn eine Standardinstanz vorhanden ist, funktioniert die Erweiterung nur für diese Standardinstanz und unterstützt die Verwaltung anderer Instanzen nicht. Wenn es keine Standardinstanz, sondern nur eine benannte Instanz gibt, wird die benannte Instanz verwaltet. Wenn es keine Standardinstanz, aber mehrere benannte Instanzen gibt, kann die Erweiterung nicht installiert werden. 

Installieren des SQL Server-IaaS-Agents im vollständigen Modus mit PowerShell:

  ```powershell-interactive
     # Get the existing compute VM
     $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
     # Install 'Full' SQL Server IaaS agent extension
     New-AzResource -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
        -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines `
        -Properties @{virtualMachineResourceId=$vm.Id;sqlServerLicenseType='AHUB';sqlManagement='Full'}  
  
  ```

| Parameter | Zulässige Werte                        |
| :------------------| :-------------------------------|
| **sqlServerLicenseType** | `AHUB` oder `PAYG`     |
| &nbsp;             | &nbsp;                          |


> [!NOTE]
> Wenn die Erweiterung noch nicht installiert ist, wird der SQL Server-Dienst durch die Installation der Erweiterung im vollständigen Modus neu gestartet. Um einen Neustart des SQL Server-Diensts zu vermeiden, installieren Sie stattdessen den Modus „Lightweight“ mit eingeschränkten Verwaltungsfunktionen.
> 
> Eine Aktualisierung der SQL Server-IaaS-Erweiterung bewirkt keinen Neustart des SQL Server-Diensts. 

### <a name="install-on-a-vm-with-a-single-named-sql-server-instance"></a>Installieren eines virtuellen Computers mit einer einzelnen benannten SQL Server-Instanz
Die SQL Server-IaaS-Erweiterung funktioniert mit einer benannten SQL Server-Instanz, wenn die Standardinstanz deinstalliert und die IaaS-Erweiterung neu installiert wird.

So verwenden Sie eine benannte SQL Server-Instanz:
   1. Stellen Sie eine SQL Server-VM vom Azure Marketplace bereit. 
   1. Deinstallieren Sie die IaaS-Erweiterung über das [Azure-Portal](https://portal.azure.com).
   1. Deinstallieren Sie SQL Server in der SQL Server-VM vollständig.
   1. Installieren Sie SQL Server mit einer benannten Instanz in der SQL Server-VM. 
   1. Installieren Sie die IaaS-Erweiterung über das Azure-Portal.  


### <a name="install-in-lightweight-mode"></a>Installieren im Lightweight-Modus
Im Modus „Einfach“ erfolgt kein Neustart des SQL Server-Diensts, der Modus bietet jedoch eingeschränkte Funktionalität. 

Installieren des SQL Server-IaaS-Agents im Lightweight-Modus mit PowerShell:


  ```powershell-interactive
     /#Get the existing  Compute VM
     $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
     #Register the SQL Server VM with the 'Lightweight' SQL IaaS agent
     New-AzResource -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
        -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines `
        -Properties @{virtualMachineResourceId=$vm.Id;sqlServerLicenseType='AHUB';sqlManagement='LightWeight'}  
  
  ```

| Parameter | Zulässige Werte                        |
| :------------------| :-------------------------------|
| **sqlServerLicenseType** | `AHUB` oder `PAYG`     |
| &nbsp;             | &nbsp;                          |


## <a name="get-the-status-of-the-sql-server-iaas-extension"></a>Abrufen des Status der SQL Server-IaaS-Erweiterung
Eine Möglichkeit, zu überprüfen, ob die Erweiterung installiert ist, ist das Anzeigen des Agent-Status im Azure-Portal. Wählen Sie im Fenster des virtuellen Computers die Option **Alle Einstellungen** aus, und klicken Sie dann auf **Erweiterungen**. Die Erweiterung **SqlIaasExtension** sollte aufgeführt sein.

![Status der SQL Server-IaaS-Agent-Erweiterung im Azure-Portal](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-portal.png)

Sie können auch das Azure PowerShell-Cmdlet **Get-AzVMSqlServerExtension** verwenden:

   ```powershell-interactive
   Get-AzVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
   ```

Der Befehl oben überprüft, ob der Agent installiert ist, und stellt allgemeine Statusinformationen bereit. Mit den folgenden Befehlen können Sie spezifische Statusinformationen zu automatischen Sicherungen und Patches abrufen:

   ```powershell-interactive
    $sqlext = Get-AzVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
    $sqlext.AutoPatchingSettings
    $sqlext.AutoBackupSettings
   ```

## <a name="removal"></a>Entfernen
Im Azure-Portal können Sie die Erweiterung deinstallieren, indem Sie im Fenster **Erweiterungen** in den Eigenschaften des virtuellen Computers auf die Auslassungspunkte klicken. Wählen Sie anschließend die Option **Löschen**.

![Deinstallieren der SQL Server-IaaS-Agent-Erweiterung im Azure-Portal](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-uninstall.png)

Sie können auch das PowerShell-Cmdlet **Remove-AzVMSqlServerExtension** verwenden:

   ```powershell-interactive
    Remove-AzVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SqlIaasExtension"
   ```

## <a name="next-steps"></a>Nächste Schritte
Verwenden Sie einen der Dienste, die von der Erweiterung unterstützt werden. Weitere Informationen finden Sie in den Artikeln, die im Abschnitt [Unterstützte Dienste](#supported-services) dieses Artikels genannt werden.

Ausführlichere Informationen zur Verwendung von SQL Server auf virtuellen Azure-Computern finden Sie unter [Was ist SQL Server auf virtuellen Azure-Computern?](virtual-machines-windows-sql-server-iaas-overview.md).

