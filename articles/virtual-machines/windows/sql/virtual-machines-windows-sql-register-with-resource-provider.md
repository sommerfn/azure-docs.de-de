---
title: Registrieren von virtuellen SQL Server-Computern in Azure mit dem SQL-VM-Ressourcenanbieter | Microsoft-Dokumentation
description: Registrieren Sie Ihre SQL Server-VM beim SQL-VM-Ressourcenanbieter, um die Verwaltbarkeit zu verbessern.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/24/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: beef423d11312f83c988ad15655ec852c6fe7b00
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67797998"
---
# <a name="register-sql-server-virtual-machine-in-azure-with-the-sql-vm-resource-provider"></a>Registrieren von virtuellen SQL Server-Computern in Azure mit dem SQL-VM-Ressourcenanbieter

In diesem Artikel wird beschrieben, wie Sie Ihren virtuellen Azure SQL Server-Computer (VM) beim SQL-VM-Ressourcenanbieter registrieren. 

Durch das Bereitstellen eines SQL Server-VM-Marketplace-Image über das Azure-Portal wird die SQL Server-VM automatisch beim Ressourcenanbieter registriert. Wenn Sie jedoch SQL Server selbst auf einem virtuellen Azure-Computer installieren und kein Image im Azure Marketplace auswählen, sollten Sie Ihre SQL Server-VM beim Ressourcenanbieter registrieren. Dies birgt folgende Vorteile:

-  **Konformität**: Gemäß den Microsoft-Produktbestimmungen müssen Kunden, die den [Azure-Hybridvorteil](https://azure.microsoft.com/pricing/hybrid-benefit/) verwenden, Microsoft über die Verwendung des Azure-Hybridvorteils informieren, und dafür müssen sie sich beim SQL-VM-Ressourcenanbieter registrieren. 

-  **Vorteile in Bezug auf Funktionen**: Durch das Registrieren Ihrer SQL Server-VM beim Ressourcenanbieter werden [automatisches Patchen](virtual-machines-windows-sql-automated-patching.md), [automatisches Sichern](virtual-machines-windows-sql-automated-backup-v2.md), Überwachungs- und Verwaltbarkeitsfunktionen sowie Flexibilität in Bezug auf [Lizenzen](virtual-machines-windows-sql-ahb.md) und [Editions](virtual-machines-windows-sql-change-edition.md) freigeschaltet. Zuvor waren diese Vorteile nur für SQL Server-VM-Images aus dem Azure Marketplace verfügbar.

Das selbstständige Installieren von SQL Server auf einer Azure-VM oder das Bereitstellen einer Azure-VM von einer benutzerdefinierten VHD mit SQL Server ist konform mit dem Azure-Hybridvorteil für SQL Server; Kunden müssen Microsoft jedoch über die Verwendung informieren, indem sie sich beim SQL-VM-Ressourcenanbieter registrieren. 

Damit Sie den SQL-VM-Ressourcenanbieter verwenden können, müssen Sie ihn außerdem für Ihr Abonnement registrieren. Dies kann über das Azure-Portal, die Azure CLI und PowerShell erfolgen. 

## <a name="prerequisites"></a>Voraussetzungen

Zum Registrieren der SQL Server-VM beim Ressourcenanbieter benötigen Sie Folgendes: 

- Ein [Azure-Abonnement](https://azure.microsoft.com/free/).
- Eine [SQL Server-VM](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision). 
- [Azure CLI](/cli/azure/install-azure-cli) und [PowerShell](/powershell/azure/new-azureps-module-az). 

## <a name="register-with-sql-vm-resource-provider"></a>Registrieren beim SQL VM-Ressourcenanbieter
Wenn die [SQL-IaaS-Erweiterung](virtual-machines-windows-sql-server-agent-extension.md) auf der VM installiert ist, muss zum Registrieren beim SQL-VM-Ressourcenanbieter lediglich eine Metadatenressource vom Typ Microsoft.SqlVirtualMachine/SqlVirtualMachines erstellt werden. Nachstehend finden Sie den Codeausschnitt zum Registrieren beim SQL-VM-Ressourcenanbieter, wenn die SQL-IaaS-Erweiterung bereits auf der VM installiert ist. Bei der Registrierung beim SQL-VM-Ressourcenanbieter müssen Sie den gewünschten Typ von SQL Server-Lizenz mit entweder „PAYG“ oder „AHUB“ angeben. 

Registrieren Sie die SQL Server-VM über PowerShell mit dem folgenden Codeausschnitt:

  ```powershell-interactive
     // Get the existing  Compute VM
     $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
     // Register with SQL VM resource provider
     New-AzResource -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
        -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines `
        -Properties @{virtualMachineResourceId=$vm.Id;SqlServerLicenceType='AHUB'}  
  
  ```

Wenn die SQL-IaaS-Erweiterung nicht auf der VM installiert ist, können Sie sich beim SQL-VM-Ressourcenanbieter registrieren, indem Sie den Lightweight-SQL-Verwaltungsmodus angeben. Im Lightweight-SQL-Verwaltungsmodus installiert der SQL-VM-Ressourcenanbieter automatisch die SQL-IaaS-Erweiterung im [Lightweight-Modus](virtual-machines-windows-sql-server-agent-extension.md#install-in-lightweight-mode) und verifiziert die Metadaten der SQL Server-Instanz; hierbei wird der SQL Server-Dienst nicht neu gestartet. Bei der Registrierung beim SQL-VM-Ressourcenanbieter müssen Sie den gewünschten Typ von SQL Server-Lizenz mit entweder „PAYG“ oder „AHUB“ angeben. 

Registrieren Sie die SQL Server-VM im Lightweight-SQL-Verwaltungsmodus mithilfe von PowerShell mit dem folgenden Codeausschnitt:

  ```powershell-interactive
     // Get the existing  Compute VM
     $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
     // Register SQL VM with 'Lightweight' SQL IaaS agent
     New-AzResource -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
        -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines `
        -Properties @{virtualMachineResourceId=$vm.Id;SqlServerLicenceType='AHUB';sqlManagement='LightWeight'}  
  
  ```

Die Registrierung beim SQL-VM-Ressourcenanbieter im [Lightweight-Modus](virtual-machines-windows-sql-server-agent-extension.md#install-in-lightweight-mode) stellt Konformität sicher und bietet Flexibilität beim Lizenzieren sowie beim Aktualisieren vorhandener SQL Server-Editions. Failoverclusterinstanzen und Bereitstellungen mit mehreren Instanzen können beim SQL-VM-Ressourcenanbieter nur im Lightweight-Modus registriert werden. Sie können jederzeit die Anweisungen im Azure-Portal für die Aktualisierung auf den [Vollmodus](virtual-machines-windows-sql-server-agent-extension.md#full-mode-installation) befolgen und umfassende Verwaltbarkeitsfunktionen mit einem Neustart von SQL Server aktivieren. 

## <a name="register-sql-server-2008r2-on-windows-server-2008-vms"></a>Registrieren von SQL Server 2008/R2 auf Windows Server 2008-VMs

Installationen von SQL Server 2008 und 2008 R2 unter Windows Server 2008 können beim SQL-VM-Ressourcenanbieter im [NoAgent](virtual-machines-windows-sql-server-agent-extension.md)-Modus registriert werden. Diese Option stellt die Konformität sicher und ermöglicht die Überwachung der SQL Server-VM im Azure-Portal mit eingeschränktem Funktionsumfang.

In der folgenden Tabelle werden die während der Registrierung angegebenen zulässigen Werte für die Parameter aufgeführt:

| Parameter | Zulässige Werte                                 |
| :------------------| :--------------------------------------- |
| **sqlLicenseType** | `'AHUB'` oder `'PAYG'`                    |
| **sqlImageOffer**  | `'SQL2008-WS2008'` oder `'SQL2008R2-WS2008`|
| &nbsp;             | &nbsp;                                   |


Verwenden Sie für die Registrierung Ihrer SQL Server 2008- oder 2008 R2-Instanz unter Windows Server 2008 den folgenden Powershell-Codeausschnitt:  

  ```powershell-interactive
     // Get the existing  Compute VM
     $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
    New-AzResource -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
      -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines `
      -Properties @{virtualMachineResourceId=$vm.Id;SqlServerLicenceType='AHUB'; `
       sqlManagement='NoAgent';sqlImageSku='Standard';sqlImageOffer='SQL2008R2-WS2008'}
  ```

## <a name="verify-registration-status"></a>Überprüfen des Registrierungsstatus
Sie können über das Azure-Portal, die Azure CLI oder PowerShell prüfen, ob Ihr SQL Server bereits beim SQL-VM-Ressourcenanbieter registriert wurde. 

### <a name="azure-portal"></a>Azure-Portal 
Führen Sie die folgenden Schritte aus, um den Status der Registrierung über das Azure-Portal zu überprüfen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 
1. Navigieren Sie zu Ihren [virtuellen SQL-Computern](virtual-machines-windows-sql-manage-portal.md).
1. Wählen Sie die SQL Server-VM in der Liste aus. Wenn Ihre SQL Server-VM hier nicht aufgelistet ist, wurde die SQL Server-VM wahrscheinlich nicht beim SQL-VM-Ressourcenanbieter registriert. 
1. Überprüfen Sie den Wert unter `Status`. Wenn der Wert `Status = Succeeded` ist, wurde die SQL Server-VM erfolgreich beim SQL-VM-Ressourcenanbieter registriert. 

    ![Überprüfen des Status per SQL RP-Registrierung](media/virtual-machines-windows-sql-register-with-rp/verify-registration-status.png)

### <a name="az-cli"></a>Azure CLI

Überprüfen Sie den aktuellen Registrierungsstatus für die SQL Server-VM mit dem folgenden Azure CLI-Befehl. `ProvisioningState` zeigt `Succeeded` an, wenn die Registrierung erfolgreich abgeschlossen wurde. 

  ```azurecli-interactive
  az sql vm show -n <vm_name> -g <resource_group>
  ```


### <a name="powershell"></a>PowerShell

Überprüfen Sie den aktuellen Registrierungsstatus für die SQL Server-VM mit dem folgenden PowerShell-Cmdlet. `ProvisioningState` zeigt `Succeeded` an, wenn die Registrierung erfolgreich abgeschlossen wurde. 

  ```powershell-interactive
  Get-AzResource -ResourceName <vm_name> -ResourceGroupName <resource_group> -ResourceType Microsoft.SqlVirtualMachine/sqlVirtualMachines
  ```
Ein Fehler zeigt an, dass die SQL Server-VM nicht beim Ressourcenanbieter registriert wurde. 

---

## <a name="register-sql-vm-resource-provider-with-subscription"></a>Registrieren des SQL-VM-Ressourcenanbieters beim Abonnement 

Um Ihre SQL Server-VM beim SQL-VM-Ressourcenanbieter zu registrieren, müssen Sie den Ressourcenanbieter bei Ihrem Abonnement registrieren. Sie können dazu das Azure-Portal oder die Azure-Befehlszeilenschnittstelle verwenden.

### <a name="azure-portal"></a>Azure-Portal

Durch Ausführen der folgenden Schritte wird der SQL-VM-Ressourcenanbieter bei Ihrem Azure-Abonnement mithilfe des Azure-Portals registriert. 

1. Öffnen Sie das Azure-Portal, und navigieren Sie zu **Alle Dienste**. 
1. Navigieren Sie zu **Abonnements**, und wählen Sie das gewünschte Abonnement aus.  
1. Navigieren Sie auf der Seite **Abonnements** zu **Ressourcenanbieter**. 
1. Geben Sie im Filter `sql` ein, um die SQL-bezogenen Ressourcenanbieter aufzurufen. 
1. Wählen Sie je nach gewünschter Aktion für den Anbieter **Microsoft.SqlVirtualMachine** entweder *Registrieren*, *Erneut registrieren* oder *Registrierung aufheben*. 

   ![Ändern des Anbieters](media/virtual-machines-windows-sql-ahb/select-resource-provider-sql.png)

### <a name="az-cli"></a>Azure CLI
Der folgende Codeausschnitt registriert den SQL-VM-Ressourcenanbieter bei Ihrem Azure-Abonnement. 

```azurecli-interactive
# Register the new SQL VM resource provider to your subscription 
az provider register --namespace Microsoft.SqlVirtualMachine 
```

### <a name="powershell"></a>PowerShell

Der folgende PowerShell-Codeausschnitt registriert den SQL-VM-Ressourcenanbieter bei Ihrem Azure-Abonnement.

```powershell-interactive
# Register the new SQL VM resource provider to your subscription
Register-AzResourceProvider -ProviderNamespace Microsoft.SqlVirtualMachine
```
---

## <a name="remarks"></a>Anmerkungen

 - Der SQL-VM-Ressourcenanbieter unterstützt ausschließlich SQL Server-VMs, die über den Ressourcen-Manager bereitgestellt wurden. SQL Server-VMs, die mit dem klassischen Modell bereitgestellt wurden, werden nicht unterstützt. 
 - Der SQL-VM-Ressourcenanbieter unterstützt ausschließlich SQL Server-VMs, die in der öffentlichen Cloud bereitgestellt wurden. Bereitstellungen in der Private Cloud oder der Government Cloud werden nicht unterstützt. 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Artikeln: 

* [Übersicht über SQL Server auf einem virtuellen Windows-Computer](virtual-machines-windows-sql-server-iaas-overview.md)
* [Häufig gestellte Fragen zu SQL Server auf virtuellen Windows-Computern in Azure](virtual-machines-windows-sql-server-iaas-faq.md)
* [Preisinformationen für Azure-VMs mit SQL Server](virtual-machines-windows-sql-server-pricing-guidance.md)
* [SQL Server on Azure Virtual Machine release notes](virtual-machines-windows-sql-server-iaas-release-notes.md) (SQL Server auf virtuellem Azure-Computer – Versionshinweise)


