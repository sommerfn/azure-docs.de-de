---
title: Ändern des Lizenzierungsmodells für eine SQL Server-VM in Azure
description: Hier erfahren Sie, wie Sie die Lizenzierung für eine SQL Server-VM in Azure von „nutzungsbasierter Bezahlung“ (Pay-as-you-go, PAYG) in „Bring-Your-Own-License“ (BYOL) mit dem Azure-Hybridvorteil ändern.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: jroth
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/05/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: aac20034fb4a528e48d5b383f39205a952878539
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2019
ms.locfileid: "72900694"
---
# <a name="change-the-license-model-for-a-sql-server-virtual-machine-in-azure"></a>Ändern des Lizenzierungsmodells für eine SQL Server-VM in Azure
In diesem Artikel wird beschrieben, wie Sie das Lizenzierungsmodell für einen virtuellen SQL Server-Computer (VM) in Azure ändern, indem Sie den neuen SQL-VM-Ressourcenanbieter **Microsoft.SqlVirtualMachine** verwenden.

Es gibt zwei Lizenzierungsmodelle für eine VM, die SQL Server hostet: nutzungsbasierte Bezahlung und Azure-Hybridvorteil (Azure Hybrid Benefit, AHB). Sie können das Lizenzierungsmodell Ihrer SQL Server-VM im Azure-Portal, mit der Azure-Befehlszeilenschnittstelle (Azure CLI) oder mit PowerShell ändern. 

Nutzungsbasierte Bezahlung bedeutet, dass die sekundenbasierten Kosten für die Ausführung des virtuellen Azure-Computers die Kosten für die SQL Server-Lizenz enthalten.
Der [Azure-Hybridvorteil](https://azure.microsoft.com/pricing/hybrid-benefit/) ermöglicht Ihnen die Verwendung Ihrer eigenen SQL Server-Lizenz mit einer VM, auf der SQL Server ausgeführt wird. 

Beim Azure-Hybridvorteil können Sie SQL Server-Lizenzen mit Software Assurance („qualifizierte Lizenz“) auf Azure-VMs verwenden. Beim Azure-Hybridvorteil wird Kunden nicht die Verwendung einer SQL Server-Lizenz auf einem virtuellen Computer in Rechnung gestellt. Sie müssen jedoch weiterhin die Kosten des zugrunde liegenden Cloudcomputings (d. h. den Basistarif), des Speichers und der Sicherungen bezahlen. Außerdem müssen sie für die E/A-Vorgänge bezahlen, die mit der Nutzung der Dienste in Verbindung stehen (sofern zutreffend).

Gemäß den Microsoft-Produktbestimmungen: „...müssen Kunden beim Konfigurieren von Workloads in Azure angeben, dass sie Azure SQL-Datenbank (verwaltete Instanz, Pool für elastische Datenbanken und Einzeldatenbank), Azure Data Factory, SQL Server Integration Services oder SQL Server-VMs im Rahmen des Azure-Hybridvorteils für SQL Server verwenden“.

Um die Verwendung des Azure-Hybridvorteils für SQL Server auf einer Azure-VM anzugeben und die Bestimmungen einzuhalten, stehen drei Optionen zu Ihrer Auswahl:

- Stellen Sie eine VM mit einem „Bring-Your-Own-License“-SQL Server-Image aus dem Azure Marketplace bereit. Diese Option ist nur für Kunden verfügbar, die über ein Enterprise Agreement verfügen.
- Stellen Sie eine VM mit einem SQL Server-Image mit nutzungsbasierter Bezahlung aus dem Azure Marketplace bereit, und aktivieren Sie den Azure-Hybridvorteil.
- Installieren Sie SQL Server selbst auf einer Azure-VM, [registrieren Sie Ihre SQL Server-VM](virtual-machines-windows-sql-register-with-resource-provider.md) manuell, und aktivieren Sie den Azure-Hybridvorteil.

Der SQL Server-Lizenztyp wird beim Bereitstellen der VM festgelegt. Sie können ihn später jederzeit ändern. Der Wechsel zwischen den Lizenzierungsmodellen erfolgt unterbrechungsfrei (keine Downtime). Die VM wird nicht neu gestartet, es entstehen keine zusätzlichen Kosten, und das neue Modell ist sofort wirksam. Durch die Aktivierung des Azure-Hybridvorteils werden die Kosten *reduziert*.

## <a name="prerequisites"></a>Voraussetzungen

Für die Verwendung des SQL-VM-Ressourcenanbieters ist die SQL Server-IaaS-Erweiterung erforderlich. Hierfür benötigen Sie Folgendes:
- Ein [Azure-Abonnement](https://azure.microsoft.com/free/).
- [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default). 
- Eine [SQL Server-VM](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision), die beim [SQL-VM-Ressourcenanbieter](virtual-machines-windows-sql-register-with-resource-provider.md) registriert ist.


## <a name="change-the-license-for-vms-already-registered-with-the-resource-provider"></a>Ändern der Lizenzen für VMs, die bereits beim Ressourcenanbieter registriert sind 

# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

[!INCLUDE [windows-virtual-machines-sql-use-new-management-blade](../../../../includes/windows-virtual-machines-sql-new-resource.md)]

Sie können das Lizenzierungsmodell direkt über das Portal ändern: 

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com), und starten Sie die Ressource [Virtueller SQL-Computer](virtual-machines-windows-sql-manage-portal.md#access-the-sql-virtual-machines-resource) für Ihre SQL Server-VM. 
1. Wählen Sie **Konfigurieren** unter **Einstellungen** aus. 
1. Wählen Sie **Azure-Hybridvorteil** aus, und aktivieren Sie das Kontrollkästchen, um zu bestätigen, dass Sie über eine SQL Server-Lizenz mit Software Assurance verfügen. 
1. Wählen Sie **Übernehmen** am unteren Rand der Seite **Konfigurieren** aus. 

![Azure-Hybridvorteil im Portal](media/virtual-machines-windows-sql-ahb/ahb-in-portal.png)


# <a name="azure-clitabazure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Sie können die Azure CLI (Azure-Befehlszeilenschnittstelle) nutzen, um das Lizenzierungsmodell zu wechseln.  

Mit dem folgenden Codeausschnitt wird Ihr Lizenzierungsmodell von der nutzungsbasierten Bezahlung in Bring-Your-Own-License (bzw. die Verwendung des Azure-Hybridvorteils) geändert:

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

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)
Sie können Ihr Lizenzierungsmodell auch mit PowerShell ändern.

Mit dem folgenden Codeausschnitt wird Ihr Lizenzierungsmodell von der nutzungsbasierten Bezahlung in Bring-Your-Own-License (bzw. die Verwendung des Azure-Hybridvorteils) geändert:

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

Mit dem folgenden Codeausschnitt wird Ihr Bring-Your-Own-License-Modell in die nutzungsbasierte Bezahlung geändert:

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

## <a name="change-the-license-for-vms-not-registered-with-the-resource-provider"></a>Ändern der Lizenzen für VMs, die nicht beim Ressourcenanbieter registriert sind

Wenn Sie eine SQL Server-VM mit Images mit nutzungsbasierter Bezahlung aus dem Azure Marketplace bereitgestellt haben, lautet der SQL Server-Lizenztyp „nutzungsbasierte Bezahlung“. Wenn Sie eine SQL Server-VM mit einem Bring-Your-Own-License-Image aus dem Azure Marketplace bereitgestellt haben, lautet der Lizenztyp AHUB. Alle über standardmäßige (nutzungsbasierte Bezahlung) oder Bring-Your-Own-License-Azure Marketplace-Images bereitgestellten SQL Server-VMs werden automatisch beim SQL-VM-Ressourcenanbieter registriert, sodass der [Lizenztyp](#change-the-license-for-vms-already-registered-with-the-resource-provider) geändert werden kann.

Sie sind nur dazu berechtigt, SQL Server auf einer Azure-VM über den Azure-Hybridvorteil selbst zu installieren. Sie sollten [diese VMs beim SQL-VM-Ressourcenanbieter registrieren](virtual-machines-windows-sql-register-with-resource-provider.md), indem Sie die SQL Server-Lizenz auf den Azure-Hybridvorteil festlegen, um die Nutzung des Azure-Hybridvorteils gemäß der Microsoft-Produktbestimmungen anzugeben.

Sie können den Lizenztyp „Nutzungsbasierte Bezahlung“ oder Azure-Hybridvorteil einer SQL Server-VM nur ändern, wenn die SQL Server-VM beim SQL-VM-Ressourcenanbieter registriert ist.

## <a name="remarks"></a>Anmerkungen

- Azure CSP-Kunden (Azure Cloud Solution Provider) können den Azure-Hybridvorteil nutzen, indem sie zuerst eine VM mit nutzungsbasierter Bezahlung bereitstellen und sie dann in Bring-Your-Own-License umwandeln, wenn sie über aktive Software Assurance verfügen.
- Wenn Sie Ihre SQL Server-VM-Ressource verwerfen, kehren Sie wieder zur hartcodierten Lizenzeinstellung des Images zurück. 
- Die Möglichkeit, das Lizenzierungsmodell zu ändern, ist ein Feature des SQL-VM-Ressourcenanbieters. Bei der Bereitstellung eines Azure Marketplace-Images über das Azure-Portal wird eine SQL Server-VM automatisch beim Ressourcenanbieter registriert. Kunden, die SQL Server selbst installieren, müssen [ihre SQL Server-VM jedoch manuell registrieren](virtual-machines-windows-sql-register-with-resource-provider.md). 
- Das Hinzufügen einer SQL Server-VM zu einer Verfügbarkeitsgruppe erfordert die Neuerstellung der VM. Daher kehren alle VMs, die einer Verfügbarkeitsgruppe hinzugefügt werden, zum Standardlizenztyp „Nutzungsbasierte Bezahlung“ zurück. Der Azure-Hybridvorteil muss erneut aktiviert werden. 


## <a name="limitations"></a>Einschränkungen

- Das Ändern des Lizenzierungsmodells ist nur für Kunden mit Software Assurance verfügbar.
- Das Ändern des Lizenzierungsmodells wird nur für die Standard und Enterprise Edition von SQL Server unterstützt. Lizenzänderungen für Express, Web und Developer werden nicht unterstützt. 
- Das Ändern des Lizenzierungsmodells wird nur für virtuelle Computer unterstützt, die mit dem Azure Resource Manager-Modell bereitgestellt wurden. VMs, die mit dem klassischen Modell bereitgestellt wurden, werden nicht unterstützt. Sie können Ihre VM vom klassischen zum Resource Manager-Modell migrieren und sie beim SQL-VM-Ressourcenanbieter registrieren. Nach der Registrierung der VM beim SQL-VM-Ressourcenanbieter kann das Lizenzierungsmodell für den virtuellen Computer geändert werden.
- Das Ändern des Lizenzierungsmodells ist nur für Installationen in der öffentlichen Cloud aktiviert.
- Das Ändern des Lizenzierungsmodells wird nur auf virtuellen Computern unterstützt, die einen einzigen Netzwerkadapter haben. Auf virtuellen Computern, die mehrere Netzwerkkarten haben, müssen Sie zunächst eine der Karten (über das Azure-Portal) entfernen, bevor Sie die Vorgehensweise versuchen. Andernfalls erhalten Sie etwa folgende Fehlermeldung: 
   
  `The virtual machine '\<vmname\>' has more than one NIC associated.` 
   
  Obwohl Sie den Netzwerkadapter der VM möglicherweise wieder hinzufügen können, nachdem Sie den Lizenzierungsmodus geändert haben, werden über die SQL Server-Konfigurationsseite im Azure-Portal ausgeführte Vorgänge wie automatisches Patchen und automatische Sicherung nicht mehr unterstützt.

## <a name="known-errors"></a>Bekannte Fehler

### <a name="the-resource-microsoftsqlvirtualmachinesqlvirtualmachinesresource-group-under-resource-group-resource-group-was-not-found"></a>Die Ressource „Microsoft.SqlVirtualMachine/SqlVirtualMachines/\<Ressourcengruppe>“ unter der Ressourcengruppe „\<Ressourcengruppe>“ wurde nicht gefunden.
Dieser Fehler tritt auf, wenn Sie versuchen, das Lizenzierungsmodell auf einer SQL Server-VM zu ändern, die nicht beim SQL-VM-Ressourcenanbieter registriert wurde:

`The Resource 'Microsoft.SqlVirtualMachine/SqlVirtualMachines/\<resource-group>' under resource group '\<resource-group>' was not found. The property 'sqlServerLicenseType' cannot be found on this object. Verify that the property exists and can be set.`

Sie müssen Ihr Abonnement beim Ressourcenanbieter registrieren und anschließend [Ihre SQL Server-VM beim Ressourcenanbieter registrieren](virtual-machines-windows-sql-register-with-resource-provider.md). 

### <a name="cannot-validate-argument-on-parameter-sku"></a>Argument für „Sku“-Parameter kann nicht überprüft werden.
Dieser Fehler könnte auftreten, wenn Sie versuchen, das Lizenzierungsmodell für Ihre SQL Server-VM mit einer Azure PowerShell-Version über 4.0 zu ändern:

`Set-AzResource: Cannot validate argument on parameter 'Sku'. The argument is null or empty. Provide an argument that is not null or empty, and then try the command again.`

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
* [Häufig gestellte Fragen zu SQL Server auf virtuellen Windows-Computern](virtual-machines-windows-sql-server-iaas-faq.md)
* [Preisinformationen für SQL Server auf virtuellen Windows-Computern](virtual-machines-windows-sql-server-pricing-guidance.md)
* [SQL Server auf Windows-VMs – Versionshinweise](virtual-machines-windows-sql-server-iaas-release-notes.md)


