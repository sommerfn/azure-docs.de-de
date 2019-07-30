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
ms.openlocfilehash: 95a167cbc8fde4488e3f46ffd850f0619cf1b651
ms.sourcegitcommit: 770b060438122f090ab90d81e3ff2f023455213b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2019
ms.locfileid: "68305867"
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
     # Get the existing  Compute VM
     $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
     # Register with SQL VM resource provider
     New-AzResource -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
        -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines `
        -Properties @{virtualMachineResourceId=$vm.Id;SqlServerLicenseType='AHUB'}  
  
  ```

Wenn die SQL-IaaS-Erweiterung nicht auf der VM installiert ist, können Sie sich beim SQL-VM-Ressourcenanbieter registrieren, indem Sie den Lightweight-SQL-Verwaltungsmodus angeben. Im Lightweight-SQL-Verwaltungsmodus installiert der SQL-VM-Ressourcenanbieter automatisch die SQL-IaaS-Erweiterung im [Lightweight-Modus](virtual-machines-windows-sql-server-agent-extension.md#install-in-lightweight-mode) und verifiziert die Metadaten der SQL Server-Instanz; hierbei wird der SQL Server-Dienst nicht neu gestartet. Bei der Registrierung beim SQL-VM-Ressourcenanbieter müssen Sie den gewünschten Typ von SQL Server-Lizenz mit entweder „PAYG“ oder „AHUB“ angeben. 

Registrieren Sie die SQL Server-VM im Lightweight-SQL-Verwaltungsmodus mithilfe von PowerShell mit dem folgenden Codeausschnitt:

  ```powershell-interactive
     # Get the existing  Compute VM
     $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
     # Register SQL VM with 'Lightweight' SQL IaaS agent
     New-AzResource -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
        -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines `
        -Properties @{virtualMachineResourceId=$vm.Id;SqlServerLicenseType='AHUB';sqlManagement='LightWeight'}  
  
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
     # Get the existing  Compute VM
     $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
    New-AzResource -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
      -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines `
      -Properties @{virtualMachineResourceId=$vm.Id;SqlServerLicenseType='AHUB'; `
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
 
## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen 

**Sollte ich meine SQL Server-VM registrieren, die über ein SQL-Image im Azure Marketplace bereitgestellt wird?**

Nein. Microsoft registriert automatisch VMs, die über die SQL-Images im Azure Marketplace bereitgestellt werden. Eine Registrierung beim SQL-VM-Ressourcenanbieter ist nur erforderlich, wenn die VM NICHT über die SQL-Images im Azure Marketplace bereitgestellt und wenn SQL Server selbst installiert wurde.

**Ist der SQL-VM-Ressourcenanbieter für alle Kunden verfügbar?** 

Ja. Kunden sollten Ihre SQL Server-VM beim SQL-VM-Ressourcenanbieter registrieren, wenn sie kein SQL Server-Image aus dem Azure Marketplace, sondern stattdessen selbst installiertes SQL Server oder ihre benutzerdefinierte VHD verwendet haben. VMs, die sich im Besitz aller Abonnementtypen („Direct“, „EA“ und „CSP“) befinden, können beim SQL-VM-Ressourcenanbieter registriert werden.

**Sollte ich mich beim SQL-VM-Ressourcenanbieter registrieren, wenn bei meiner SQL Server-VM die SQL-IaaS-Erweiterung bereits installiert wurde?**

Wenn Ihre SQL Server-VM selbst installiert und nicht über die SQL-Images im Azure Marketplace bereitgestellt wurde, sollten Sie sich beim SQL-VM-Ressourcenanbieter auch dann registrieren, wenn Sie die SQL-IaaS-Erweiterung installiert haben. Bei der Registrierung beim SQL-VM-Ressourcenanbieter wird eine neue Ressource vom Typ „Microsoft.SqlVirtualMachines“ erstellt. Durch die Installation der SQL-IaaS-Erweiterung wird diese Ressource nicht erstellt.

**Was ist der standardmäßige SQL-Verwaltungsmodus bei der Registrierung beim SQL-VM-Ressourcenanbieter?**

Der standardmäßige SQL-Verwaltungsmodus bei der Registrierung beim SQL-VM-Ressourcenanbieter ist der _Vollmodus_. Wenn die Eigenschaft „SQL-Verwaltung“ bei der Registrierung beim SQL-VM-Ressourcenanbieter nicht festgelegt wird, wird für den Modus „Vollständige Verwaltbarkeit“ festgelegt. Die Installation der SQL-IaaS-Erweiterung auf der VM ist die Voraussetzung für die Registrierung beim SQL-VM-Ressourcenanbieter im Verwaltbarkeitsmodus „Vollständig“.

**Was sind die Voraussetzungen für die Registrierung beim SQL-VM-Ressourcenanbieter?**

Es gibt keine Voraussetzungen für die Registrierung beim SQL-VM-Ressourcenanbieter im Lightweight- oder NoAgent-Modus. Als Voraussetzung für die Registrierung beim SQL-VM-Ressourcenanbieter im Vollmodus muss die SQL-IaaS-Erweiterung auf der VM installiert sein.

**Kann ich mich beim SQL-VM-Ressourcenanbieter registrieren, wenn die SQL IaaS-Erweiterung auf der VM nicht installiert wurde?**

Ja, Sie können sich beim SQL-VM-Ressourcenanbieter im Lightweight-Verwaltungsmodus registrieren, wenn die SQL-IaaS-Erweiterung auf der VM nicht installiert wurde. Im Lightweight-Modus verwendet der SQL-VM-Ressourcenanbieter eine Konsolen-App, um die Version und Edition der SQL-Instanz zu überprüfen. Die Konsolen-App wird automatisch heruntergefahren, nachdem überprüft wurde, ob mindestens eine SQL-Instanz auf der VM ausgeführt wird. Bei der Registrierung beim SQL-VM-Ressourcenanbieter im Lightweight-Modus wird SQL Server nicht neu gestartet, und auf der VM wird kein Agent erstellt.

**Wird bei der Registrierung beim SQL-VM-Ressourcenanbieter ein Agent auf meiner VM installiert?**

Nein. Bei der Registrierung beim SQL-VM-Ressourcenanbieter wird nur eine neue Metadatenressource erstellt, und es wird kein Agent auf der VM installiert. Die SQL-IaaS-Erweiterung ist nur erforderlich, um vollständige Verwaltbarkeit zu ermöglichen. Durch ein Upgrade des Verwaltbarkeitsmodus von „Lightweight“ auf „Vollständig“ wird die SQL-IaaS-Erweiterung installiert und SQL Server neu gestartet.

**Wird SQL Server durch die Registrierung beim SQL-VM-Ressourcenanbieter auf meiner VM neu gestartet?**

Nein. Durch die Registrierung beim SQL-VM-Ressourcenanbieter wird nur eine neue Metadatenressource erstellt, und SQL Server wird auf der VM nicht neu gestartet. Ein Neustart von SQL Server ist nur erforderlich, um die SQL-IaaS-Erweiterung zu installieren, und die SQL-IaaS-Erweiterung ist erforderlich, um vollständige Verwaltbarkeit zu ermöglichen. Bei einem Upgrade des Verwaltbarkeitsmodus von „Lightweight“ auf „Vollständig“ wird die SQL-IaaS-Erweiterung installiert und SQL Server neu gestartet.

**Worin besteht der Unterschied zwischen den Verwaltungsmodi „Lightweight“ und „NoAgent“ bei der Registrierung beim SQL-VM-Ressourcenanbieter?** 

Der Verwaltungsmodus „NoAgent“ steht nur für SQL Server 2008/R2 unter Windows Server 2008 zur Verfügung und ist der einzige verfügbare Verwaltungsmodus für diese Versionen. Für alle anderen Versionen von SQL Server sind die beiden verfügbaren Verwaltbarkeitsmodi "Lightweight" und "Vollständig". Der Modus „NoAgent“ erfordert es, dass die SQL Server-Version und die Editionseigenschaften vom Kunden festgelegt werden müssen; im Lightweight-Modus wird die VM abgefragt, um die Version und Edition der SQL-Instanz zu ermitteln.

**Kann ich mich beim SQL-VM-Ressourcenanbieter im Lightweight- oder NoAgent-Modus über die Azure CLI registrieren?**

Nein. Die Eigenschaft „SQL-Verwaltungsmodus“ steht nur zur Verfügung, wenn die Registrierung beim SQL-VM-Ressourcenanbieter über Azure PowerShell vorgenommen wird. Die Azure CLI unterstützt nicht das Festlegen der Eigenschaft „SQL-Verwaltbarkeit“ und registriert sich beim SQL-VM-Ressourcenanbieter immer im Standardmodus – „Vollständige Verwaltbarkeit“.

**Kann ich mich beim SQL-VM-Ressourcenanbieter registrieren, ohne den SQL-Lizenztyp anzugeben?**

Nein. Der SQL-Lizenztyp ist keine optionale Eigenschaft bei der Registrierung beim SQL-VM-Ressourcenanbieter. Sie müssen den SQL-Lizenztyp „PAYG“ oder „AHUB“ festlegen, wenn Sie sich beim SQL-VM-Ressourcenanbieter in allen Verwaltbarkeitsmodi („NoAgent“, „Lightweight“ und „Vollständig“) über Azure CLI und PowerShell registrieren.

**Kann ich die SQL-IaaS-Erweiterung vom Modus „NoAgent“ auf „Voll“ aktualisieren?**

Nein. Das Upgrade des SQL-Verwaltbarkeitsmodus auf „Vollständig“ oder „Lightweight“ steht für den Modus „NoAgent“ nicht zur Verfügung. Dies ist eine technische Einschränkung von Windows Server 2008.

**Kann ich die SQL-IaaS-Erweiterung vom Modus „Lightweight“ auf den Modus „Voll“ aktualisieren?**

Ja. Das Upgrade des SQL-Verwaltbarkeitsmodus von „Lightweight“ auf „Vollständig“ wird über PowerShell oder das Azure-Portal unterstützt. Außerdem muss SQL Server neu gestartet werden.

**Kann ich eine Herabstufung der SQL-IaaS-Erweiterung vom Vollmodus zu den Verwaltungsmodi „NoAgent“-oder „Lightweight“ durchführen?**

Nein. Eine Herabstufung des Verwaltbarkeitsmodus für die SQL-IaaS-Erweiterung wird nicht unterstützt. Der SQL-Verwaltbarkeitsmodus kann vom Modus „Vollständig“ nicht auf „Lightweight“ oder „NoAgent“ herabgestuft werden, und dies ist auch nicht möglich vom Modus „Lightweight“ auf den Modus „NoAgent“. Wenn Sie den Verwaltbarkeitsmodus von „Vollständige Verwaltbarkeit“ ändern möchten, entfernen Sie die SQL-IaaS-Erweiterung, löschen Sie die Ressource „Microsoft.SqlVirtualMachine“, und registrieren Sie die SQL Server-VM erneut beim SQL-VM-Ressourcenanbieter.

**Kann ich mich beim SQL-VM-Ressourcenanbieter über das Azure-Portal registrieren?**

Nein. Die Registrierung beim SQL-VM-Ressourcenanbieter steht im Azure-Portal nicht zur Verfügung. Die Registrierung beim SQL-VM-Ressourcenanbieter im Verwaltbarkeitsmodus „Vollständig“ wird über die Azure CLI oder PowerShell unterstützt. Die Registrierung beim SQL-VM-Ressourcenanbieter in den Verwaltbarkeitsmodi „Lightweight“ oder „NoAgent“ wird nur von Azure PowerShell-APIs unterstützt.

**Kann ich eine VM beim SQL-VM-Ressourcenanbieter registrieren, bevor SQL Server installiert wird?**

Nein. Die VM muss mindestens eine SQL-Instanz haben, damit sie beim SQL-VM-Ressourcenanbieter erfolgreich registriert werden kann. Wenn es auf der VM keine SQL-Instanz gibt, wird die neue Ressource „Micosoft.SqlVirtualMachine“ in einem fehlerhaften Zustand angezeigt.

**Kann ich eine VM bei einer SQL-VM-Ressource registrieren, wenn mehrere SQL-Instanzen vorhanden sind?**

Ja. Der SQL-VM-Ressourcenanbieter registriert nur eine SQL-Instanz. Wenn es mehrere Instanzen gibt, registriert der SQL-VM-Ressourcenanbieter die standardmäßige SQL-Instanz verwenden. Gibt es keine Standardinstanz, wird nur die Registrierung im Lightweight-Modus unterstützt. Für ein Upgrade vom Verwaltbarkeitsmodus „Lightweight“ auf „Vollständig“ sollte entweder die standardmäßige SQL-Instanz vorhanden sein, oder die VM sollte nur eine benannte SQL-Instanz haben.

**Kann ich eine SQL Server-Failoverclusterinstanz beim SQL-VM-Ressourcenanbieter registrieren?**

Ja. SQL-FCI-Instanzen auf einer Azure-VM können beim SQL-VM-Ressourcenanbieter im Lightweight-Modus registriert werden. SQL-FCI-Instanzen können jedoch nicht auf den vollständigen Verwaltbarkeitsmodus aktualisiert werden.

**Kann ich meine VM beim SQL-VM-Ressourcenanbieter registrieren, wenn die Always On-Verfügbarkeitsgruppe konfiguriert wurde?**

Ja. Es gibt keine Einschränkungen beim Registrieren einer SQL Server-Instanz auf einer Azure-VM beim SQL-VM-Ressourcenanbieter, wenn sie Teil einer Always On-Verfügbarkeitsgruppenkonfiguration ist.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Artikeln: 

* [Übersicht über SQL Server auf einem virtuellen Windows-Computer](virtual-machines-windows-sql-server-iaas-overview.md)
* [Häufig gestellte Fragen zu SQL Server auf virtuellen Windows-Computern in Azure](virtual-machines-windows-sql-server-iaas-faq.md)
* [Preisinformationen für Azure-VMs mit SQL Server](virtual-machines-windows-sql-server-pricing-guidance.md)
* [SQL Server on Azure Virtual Machine release notes](virtual-machines-windows-sql-server-iaas-release-notes.md) (SQL Server auf virtuellem Azure-Computer – Versionshinweise)
