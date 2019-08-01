---
title: Ändern des Lizenzierungsmodells für eine SQL Server-VM in Azure
description: Hier erfahren Sie, wie Sie die Lizenzierung für eine SQL-VM in Azure von „nutzungsbasierter Bezahlung“ (Pay-as-you-go, PAYG) in „Bring-Your-Own-License“ (BYOL) mit dem Azure-Hybridvorteil ändern.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: jroth
tags: azure-resource-manager
ms.assetid: aa5bf144-37a3-4781-892d-e0e300913d03
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 02/13/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 78ad784a45d2b0063932791daedc9b1ec1aafd72
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2019
ms.locfileid: "67786763"
---
# <a name="how-to-change-licensing-model-for-a-sql-server-virtual-machine-in-azure"></a>Ändern des Lizenzierungsmodells für eine SQL Server-VM in Azure
In diesem Artikel wird beschrieben, wie Sie das Lizenzierungsmodell für einen virtuellen SQL Server-Computer in Azure ändern, indem Sie den neuen SQL-VM-Ressourcenanbieter **Microsoft.SqlVirtualMachine** verwenden.

Es gibt zwei Lizenzierungsmodelle für eine VM, die SQL Server hostet: nutzungsbasierte Bezahlung und Azure-Hybridvorteil (AHB). Jetzt können Sie das Lizenzierungsmodell Ihrer SQL Server-VM im Azure-Portal, mit der Azure-Befehlszeilenschnittstelle (Azure CLI) oder mit PowerShell ändern. 

**Nutzungsbasierte Bezahlung** bedeutet, dass die sekundenbasierten Kosten für die Ausführung des virtuellen Azure-Computers die Kosten für die SQL Server-Lizenz enthalten.
Das Modell [Azure-Hybridvorteil (AHB)](https://azure.microsoft.com/pricing/hybrid-benefit/) ermöglicht Ihnen die Verwendung Ihrer eigenen SQL Server-Lizenz mit einer VM, auf der SQL Server ausgeführt wird. 

Beim Microsoft Azure-Hybridvorteil für SQL Server können Sie SQL Server-Lizenzen mit Software Assurance („qualifizierte Lizenz“) in Azure Virtual Machines verwenden. Kunden wird beim Azure-Hybridvorteil für SQL Server die Nutzung der SQL Server-Lizenz auf der VM nicht in Rechnung gestellt, sie müssen jedoch weiterhin für das zugrunde liegende Cloud Computing (d. h. den Basistarif), den Speicher und Sicherungen sowie E/A-Vorgänge in Verbindung mit der Nutzung der Dienste (sofern zutreffend) bezahlen.

Gemäß den Microsoft-Produktbestimmungen „müssen Kunden beim Konfigurieren von Workloads in Azure angeben, dass sie Azure SQL-Datenbank (verwaltete Instanz, Pool für elastische Datenbanken und Einzeldatenbank), Azure Data Factory, SQL Server Integration Services oder SQL Server-VMs im Rahmen des Azure-Hybridvorteils für SQL Server verwenden“.

Um die Verwendung des Azure-Hybridvorteils für SQL Server auf einer Azure-VM anzugeben und die Bestimmungen einzuhalten, stehen drei Optionen zur Auswahl:

1. Stellen Sie eine VM mit einem BYOL-SQL Server-Image aus dem Azure Marketplace bereit (nur für Kunden mit Enterprise Agreement verfügbar).
1. Stellen Sie eine VM mit einem PAYG-SQL Server Image aus dem Azure Marketplace bereit, und aktivieren Sie AHB.
1. Installieren Sie SQL Server selbst auf einer Azure-VM, [registrieren Sie Ihre SQL Server-VM](virtual-machines-windows-sql-register-with-resource-provider.md) manuell, und aktivieren Sie AHB.

Der SQL Server-Lizenztyp wird beim Bereitstellen der VM festgelegt und kann danach jederzeit geändert werden. Der Wechsel zwischen den Lizenzierungsmodellen erfolgt unterbrechungsfrei (**keine Downtime**). Die VM wird nicht neu gestartet, es entstehen **keine zusätzlichen Kosten** (durch das Aktivieren von AHB werden die Kosten sogar *reduziert*), und das neue Modell ist **sofort wirksam**. 

## <a name="prerequisites"></a>Voraussetzungen

Für die Verwendung des SQL-VM-Ressourcenanbieters ist die SQL-IaaS-Erweiterung erforderlich. Um den SQL-VM-Ressourcenanbieter verwenden zu können, benötigen Sie daher Folgendes:
- Ein [Azure-Abonnement](https://azure.microsoft.com/free/).
- [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default). 
- Eine [SQL Server-VM](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision), die beim installierten [SQL-VM-Ressourcenanbieter](virtual-machines-windows-sql-register-with-resource-provider.md) registriert ist. 


## <a name="change-license-for-vms-already-registered-with-resource-provider"></a>Ändern der Lizenzen für VMs, die bereits beim Ressourcenanbieter registriert sind 

# <a name="azure-portaltabazure-portal"></a>[Azure-Portal](#tab/azure-portal)

[!INCLUDE [windows-virtual-machines-sql-use-new-management-blade](../../../../includes/windows-virtual-machines-sql-new-resource.md)]

Sie können das Lizenzierungsmodell direkt über das Portal ändern. 

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com), und starten Sie die Ressource [Virtueller SQL-Computer](virtual-machines-windows-sql-manage-portal.md#access-sql-virtual-machine-resource) für Ihre SQL Server-VM. 
1. Wählen Sie **Konfigurieren** unter **Einstellungen** aus. 
1. Wählen Sie **Azure-Hybridvorteil** aus, und aktivieren Sie das Kontrollkästchen, um zu bestätigen, dass Sie über eine SQL Server-Lizenz mit Software Assurance verfügen. 
1. Wählen Sie **Übernehmen** am unteren Rand der Seite **Konfigurieren** aus. 

![AHB im Portal](media/virtual-machines-windows-sql-ahb/ahb-in-portal.png)


# <a name="az-clitabbash"></a>[Azure CLI](#tab/bash)

Sie können die Azure CLI (Azure-Befehlszeilenschnittstelle) nutzen, um das Lizenzierungsmodell zu wechseln.  

Mit dem folgenden Codeausschnitt wird Ihr Lizenzierungsmodell von der nutzungsbasierten Bezahlung in BYOL (bzw. die Verwendung des Azure-Hybridvorteils) geändert:

```azurecli-interactive
# Switch your SQL Server VM license from pay-as-you-go to bring-your-own
# example: az sql vm update -n AHBTest -g AHBTest --license-type AHUB

az sql vm update -n <VMName> -g <ResourceGroupName> --license-type AHUB
```

Mit dem folgenden Codeausschnitt wird Ihr Bring-Your-Own-License-Modell in die nutzungsbasierte Bezahlung geändert: 

```azurecli-interactive
# Switch your SQL Server VM license from bring-your-own to pay-as-you-go
# example: az sql vm update -n AHBTest -g AHBTest --license-type PAYG

az sql vm update -n <VMName> -g <ResourceGroupName> --license-type PAYG
```

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
Sie können Ihr Lizenzierungsmodell auch mit PowerShell ändern.

Mit dem folgenden Codeausschnitt wird Ihr Lizenzierungsmodell von der nutzungsbasierten Bezahlung in BYOL (bzw. die Verwendung des Azure-Hybridvorteils) geändert:

```powershell-interactive
# Switch your SQL Server VM license from pay-as-you-go to bring-your-own
#example: $SqlVm = Get-AzResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName AHBTest -ResourceName AHBTest
$SqlVm = Get-AzResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
$SqlVm.Properties.sqlServerLicenseType="AHUB"
<# the following code snippet is only necessary if using Azure Powershell version > 4
$SqlVm.Kind= "LicenseChange"
$SqlVm.Plan= [Microsoft.Azure.Management.ResourceManager.Models.Plan]::new()
$SqlVm.Sku= [Microsoft.Azure.Management.ResourceManager.Models.Sku]::new() #>
$SqlVm | Set-AzResource -Force 
```

Mit dem folgenden Codeausschnitt wird Ihr BYOL-Modell in die nutzungsbasierte Bezahlung geändert:

```powershell-interactive
# Switch your SQL Server VM license from bring-your-own to pay-as-you-go
#example: $SqlVm = Get-AzResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName AHBTest -ResourceName AHBTest
$SqlVm = Get-AzResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
$SqlVm.Properties.sqlServerLicenseType="PAYG"
<# the following code snippet is only necessary if using Azure Powershell version > 4
$SqlVm.Kind= "LicenseChange"
$SqlVm.Plan= [Microsoft.Azure.Management.ResourceManager.Models.Plan]::new()
$SqlVm.Sku= [Microsoft.Azure.Management.ResourceManager.Models.Sku]::new() #>
$SqlVm | Set-AzResource -Force 
```
---

## <a name="change-license-for-vms-not-registered-with-resource-provider"></a>Ändern der Lizenzen für VMs, die nicht beim Ressourcenanbieter registriert sind

Wenn Sie eine SQL Server-VM mit einem PAYG-Image aus dem Azure Marketplace bereitgestellt haben, lautet der SQL-Lizenztyp PAYG. Wenn Sie eine SQL Server-VM mit einem BYOL-Image aus dem Azure Marketplace bereitgestellt haben, lautet der Lizenztyp AHUB. Alle über Standard- (PAYG) oder BYOL-Azure Marketplace-Images bereitgestellten SQL Server-VMs werden automatisch beim SQL-VM-Ressourcenanbieter registriert, sodass der [Lizenztyp](#change-license-for-vms-already-registered-with-resource-provider) geändert werden kann.

Gemäß den Microsoft-Produktbestimmungen sind Sie nur berechtigt, SQL Server über den Azure-Hybridvorteil auf einer Azure-VM selbst zu installieren. Zudem sollten Sie [diese VMs beim SQL-VM-Ressourcenanbieter registrieren](virtual-machines-windows-sql-register-with-resource-provider.md), indem Sie die SQL Server-Lizenz auf AHB festlegen, um die Nutzung von AHB anzugeben.

Sie können den Lizenztyp einer SQL Server-VM nur in PAYG oder AHB ändern, wenn die SQL-VM beim SQL-VM-Ressourcenanbieter registriert ist. Aus Gründen der Lizenzkonformität sollten alle SQL-VMs beim SQL-VM-Ressourcenanbieter registriert werden.

## <a name="remarks"></a>Anmerkungen

 - Azure CSP-Kunden (Azure Cloud Solution Partner) können den Azure-Hybridvorteil nutzen, indem sie zuerst eine VM mit nutzungsbasierter Bezahlung bereitstellen und sie dann in Bring-Your-Own-License umwandeln, wenn sie über aktive Software Assurance verfügen.
 - Wenn Sie Ihre SQL Server-VM-Ressource verwerfen, kehren Sie wieder zur hartcodierten Lizenzeinstellung des Images zurück. 
  - Die Möglichkeit, das Lizenzierungsmodell zu ändern, ist ein Feature des SQL-VM-Ressourcenanbieters. Wird ein Marketplace-Image über das Azure-Portal bereitgestellt, wird automatisch eine SQL Server-VM mit dem Ressourcenanbieter registriert. Kunden, die SQL Server selbst installieren, müssen [ihre SQL Server-VM jedoch manuell registrieren](virtual-machines-windows-sql-register-with-resource-provider.md). 
- Beim Hinzufügen einer SQL Server-VM zu einer Verfügbarkeitsgruppe muss die VM erneut erstellt werden. Daher kehren alle VMs, die einer Verfügbarkeitsgruppe hinzugefügt werden, zum Standardlizenztyp „Nutzungsbasierte Bezahlung“ zurück, und AHB muss erneut aktiviert werden. 


## <a name="limitations"></a>Einschränkungen

 - Das Ändern des Lizenzierungsmodells ist nur für Kunden mit Software Assurance verfügbar.
 - Das Ändern des Lizenzierungsmodells wird nur für die Standard und Enterprise Edition von SQL Server unterstützt. Lizenzänderungen für Express, Web und Developer werden nicht unterstützt. 
 - Ändern des Lizenzierungsmodells wird nur für virtuelle Computer unterstützt, die mit dem Resource Manager-Modell bereitgestellt wurden. Virtuelle Computer, die mit dem klassischen Modell bereitgestellt wurden, werden nicht unterstützt. 
 - Ändern des Lizenzierungsmodells ist nur für Installationen in der öffentlichen Cloud aktiviert.
 - Ändern des Lizenzierungsmodells wird nur auf virtuellen Computern unterstützt, die eine einzige NIC (Netzwerkkarte) haben. Auf virtuellen Computern, die mehrere Netzwerkkarten haben, müssen Sie zunächst eine der Karten (über das Azure-Portal) entfernen, bevor Sie die Vorgehensweise versuchen. Andernfalls wird ein Fehler auftreten, der in etwa wie folgt aussieht: `The virtual machine '\<vmname\>' has more than one NIC associated.` Obwohl Sie die Netzwerkkarte der VM möglicherweise wieder hinzufügen können, nachdem Sie den Lizenzierungsmodus geändert haben, werden über die SQL-Konfigurationsseite im Azure-Portal ausgeführte Vorgänge (z. B. automatisches Patchen und automatische Sicherung) nicht mehr unterstützt.


## <a name="known-errors"></a>Bekannte Fehler

### <a name="the-resource-microsoftsqlvirtualmachinesqlvirtualmachinesresource-group-under-resource-group-resource-group-was-not-found-the-property-sqlserverlicensetype-cannot-be-found-on-this-object-verify-that-the-property-exists-and-can-be-set"></a>Die Ressource „Microsoft.SqlVirtualMachine/SqlVirtualMachines/\<Ressourcengruppe>“ unter der Ressourcengruppe „\<Ressourcengruppe>“ wurde nicht gefunden. Die sqlServerLicenseType-Eigenschaft kann in diesem Objekt nicht gefunden werden. Vergewissern Sie sich, dass die Eigenschaft vorhanden ist und festgelegt werden kann.
Dieser Fehler tritt auf, wenn Sie versuchen, das Lizenzierungsmodell auf einer SQL Server-VM zu ändern, die nicht beim SQL-VM-Ressourcenanbieter registriert wurde. Sie müssen den Ressourcenanbieter bei Ihrem [Abonnement](virtual-machines-windows-sql-register-with-resource-provider.md#register-sql-vm-resource-provider-with-subscription) und dann Ihre SQL Server-VM beim SQL-[Ressourcenanbieter](virtual-machines-windows-sql-register-with-resource-provider.md) registrieren. 

### <a name="cannot-validate-argument-on-parameter-sku"></a>Argument für „Sku“-Parameter kann nicht überprüft werden.
Dieser Fehler kann auftreten, wenn Sie versuchen, das Lizenzierungsmodell für Ihre SQL Server-VM mit einer Azure PowerShell-Version über 4.0 zu ändern: `Set-AzResource: Cannot validate argument on parameter 'Sku'. The argument is null or empty. Provide an argument that is not null or empty, and then try the command again.`

Um diesen Fehler zu beheben, heben Sie die Auskommentierung dieser Zeilen im zuvor genannten PowerShell-Codeausschnitt auf, wenn Sie das Lizenzierungsmodell wechseln:

  ```powershell-interactive
  # the following code snippet is necessary if using Azure Powershell version > 4
  $SqlVm.Kind= "LicenseChange"
  $SqlVm.Plan= [Microsoft.Azure.Management.ResourceManager.Models.Plan]::new()
  $SqlVm.Sku= [Microsoft.Azure.Management.ResourceManager.Models.Sku]::new()
  ```
  
Verwenden Sie den folgenden Code, um Ihre Azure PowerShell-Version zu überprüfen:
  
  ```powershell-interactive
  Get-Module -ListAvailable -Name Azure -Refresh
  ```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Artikeln: 

* [Übersicht über SQL Server auf einem virtuellen Windows-Computer](virtual-machines-windows-sql-server-iaas-overview.md)
* [Häufig gestellte Fragen zu SQL Server auf virtuellen Windows-Computern in Azure](virtual-machines-windows-sql-server-iaas-faq.md)
* [Preisinformationen für Azure-VMs mit SQL Server](virtual-machines-windows-sql-server-pricing-guidance.md)
* [SQL Server on Azure Virtual Machine release notes](virtual-machines-windows-sql-server-iaas-release-notes.md) (SQL Server auf virtuellem Azure-Computer – Versionshinweise)


