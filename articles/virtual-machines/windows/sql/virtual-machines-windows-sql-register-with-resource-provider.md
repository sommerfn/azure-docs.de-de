---
title: Registrieren von virtuellen SQL Server-Computern in Azure mit dem SQL-VM-Ressourcenanbieter | Microsoft-Dokumentation
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
ms.openlocfilehash: 433480b4a587b3a085c3b1c0ba4122ae98eb4508
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2019
ms.locfileid: "72897715"
---
# <a name="register-a-sql-server-virtual-machine-in-azure-with-the-sql-vm-resource-provider"></a>Registrieren von virtuellen SQL Server-Computern in Azure mit dem SQL-VM-Ressourcenanbieter

In diesem Artikel wird beschrieben, wie Sie Ihren virtuellen SQL Server-Computer (VM) in Azure beim SQL-VM-Ressourcenanbieter registrieren. 

Durch das Bereitstellen eines SQL Server-VM-Azure Marketplace-Images über das Azure-Portal wird die SQL Server-VM automatisch beim Ressourcenanbieter registriert. Wenn Sie SQL Server auf einem virtuellen Azure-Computer selbst installieren und kein Image im Azure Marketplace auswählen oder wenn Sie eine Azure-VM aus einer benutzerdefinierten VHD mit SQL Server bereitstellen, sollten Sie Ihre SQL Server-VM beim Ressourcenanbieter registrieren. Dies hat folgende Vorteile:

- **Vereinfachen der Lizenzverwaltung**: Gemäß den Microsoft-Produktbedingungen müssen Kunden Microsoft mitteilen, wenn Sie den [Azure-Hybridvorteil](https://azure.microsoft.com/pricing/hybrid-benefit/) verwenden. Ein Registrieren beim SQL-VM-Ressourcenanbieter vereinfacht die SQL Server-Lizenzverwaltung und ermöglicht es Ihnen, schnell SQL Server-VMs zu erkennen, indem Sie den Azure-Hybridvorteil im [Portal](virtual-machines-windows-sql-manage-portal.md) oder in der Azure CLI verwenden: 

   ```azurecli-interactive
   $vms = az sql vm list | ConvertFrom-Json
   $vms | Where-Object {$_.sqlServerLicenseType -eq "AHUB"}
   ```

- **Featurevorteile:** Wenn Sie Ihre SQL Server-VM beim Ressourcenanbieter registrieren, werden [automatische Patches](virtual-machines-windows-sql-automated-patching.md), [automatisierte Sicherungen](virtual-machines-windows-sql-automated-backup-v2.md) sowie Überwachungs-und Verwaltbarkeitsfunktionen freigegeben. Außerdem erhalten Sie mehr Flexibilität bei der [Lizenzierung](virtual-machines-windows-sql-ahb.md) und bei der Auswahl der [Edition](virtual-machines-windows-sql-change-edition.md). Zuvor waren diese Features nur für SQL Server-VM-Images aus dem Azure Marketplace verfügbar.

- **Kostenlose Verwaltung**:  Das Registrieren beim SQL-VM-Ressourcenanbieter und alle Verwaltbarkeitsmodi sind vollständig kostenlos. Es fallen keine zusätzlichen Kosten für den Ressourcenanbieter oder das Ändern von Verwaltungsmodi an. 

Damit Sie den SQL-VM-Ressourcenanbieter verwenden können, müssen Sie ihn außerdem für Ihr Abonnement registrieren. Verwenden Sie hierzu das Azure-Portal, die Azure-Befehlszeilenschnittstelle oder PowerShell. 

  > [!NOTE]
  > Es gibt keine zusätzlichen Lizenzierungsanforderungen, die mit dem Registrieren beim Ressourcenanbieter verknüpft sind. Das Registrieren beim SQL-VM-Ressourcenanbieter bietet eine vereinfachte Methode, um die Anforderung zu erfüllen, dass Microsoft zu benachrichtigen ist, wenn der Azure-Hybridvorteil aktiviert wurde, statt für jede Ressource Formulare zur Lizenzierungsregistrierung zu verwenden. 

Weitere Informationen zu den Vorzügen der Nutzung des SQL VM-Ressourcenanbieters finden Sie im folgenden [channel9](https://channel9.msdn.com/Shows/Data-Exposed/Benefit-from-SQL-VM-Resource-Provider-when-self-installing-SQL-Server-on-Azure?WT.mc_id=dataexposed-c9-niner)-Video: 

<iframe src="https://channel9.msdn.com/Shows/Data-Exposed/Benefit-from-SQL-VM-Resource-Provider-when-self-installing-SQL-Server-on-Azure/player" width="960" height="540" allowFullScreen frameBorder="0" title="Benefit from SQL VM Resource Provider when self-installing SQL Server on Azure - Microsoft Channel 9 Video (Vorteil durch den SQL VM-Ressourcenanbieter bei der Eigeninstallation von SQL Server in Azure – Microsoft Channel 9-Video)"></iframe>


## <a name="prerequisites"></a>Voraussetzungen

Zum Registrieren der SQL Server-VM beim Ressourcenanbieter benötigen Sie Folgendes: 

- Ein [Azure-Abonnement](https://azure.microsoft.com/free/).
- Eine [SQL Server-VM](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision). 
- Die aktuelle Version von [Azure CLI](/cli/azure/install-azure-cli) oder [PowerShell](/powershell/azure/new-azureps-module-az). 


## <a name="register-with-sql-vm-resource-provider"></a>Registrieren beim SQL VM-Ressourcenanbieter
Wenn die [SQL Server-IaaS-Agent-Erweiterung](virtual-machines-windows-sql-server-agent-extension.md) nicht auf der VM installiert ist, können Sie sich beim SQL-VM-Ressourcenanbieter registrieren, indem Sie den Lightweight-SQL-Verwaltungsmodus angeben. 

Wenn während der Registrierung der Lightweightmodus angegeben wird, installiert der SQL-VM-Ressourcenanbieter die SQL-IaaS-Erweiterung automatisch im [Lightweightmodus](#change-management-modes) und überprüft die Metadaten der SQL Server-Instanz. Dabei wird der SQL Server-Dienst nicht neu gestartet. Bei der Registrierung beim SQL-VM-Ressourcenanbieter müssen Sie den gewünschten Typ von SQL Server-Lizenz mit entweder „PAYG“ oder „AHUB“ angeben.

Die Registrierung beim SQL-VM-Ressourcenanbieter im Lightweightmodus gewährleistet Konformität, eine flexible Lizenzierung sowie flexible direkte SQL Server-Editionsupdates. Failoverclusterinstanzen und Bereitstellungen mit mehreren Instanzen können beim SQL-VM-Ressourcenanbieter nur im Lightweight-Modus registriert werden. Sie können jederzeit ein [Upgrade](#change-management-modes) auf den vollständigen Verwaltungsmodus durchführen. Dadurch wird jedoch der SQL Server-Dienst neu gestartet. 


# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
Verwenden Sie den folgenden Codeausschnitt zum Registrieren beim SQL-VM-Ressourcenanbieter, wenn die SQL Server-IaaS-Erweiterung bereits auf der VM installiert ist. Sie müssen bei der Registrierung beim SQL-VM-Ressourcenanbieter den Typ der gewünschten SQL Server-Lizenz angeben: nutzungsbasierte Bezahlung (Pay-as-you-go, `PAYG`) oder Azure-Hybridvorteil (`AHUB`). 

Registrieren Sie die SQL Server-VM mit dem folgenden PowerShell-Codeausschnitt:

  ```powershell-interactive
     # Get the existing compute VM
     $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
     # Register SQL VM with 'Lightweight' SQL IaaS agent
     New-AzResource -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
        -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines `
        -Properties @{virtualMachineResourceId=$vm.Id;SqlServerLicenseType='PAYG';sqlManagement='LightWeight'}  
  
  ```

# <a name="az-clitabbash"></a>[Azure CLI](#tab/bash)

Für kostenpflichtige Editionen (Enterprise oder Standard):

  ```azurecli-interactive
  # Register Enterprise or Standard self-installed VM in Lightweight mode

  az sql vm create --name <vm_name> --resource-group <resource_group_name> --location <vm_location> --license-type PAYG 

  ```

Für kostenlose Editionen (Developer, Web oder Express):

  ```azurecli-interactive
  # Register Developer, Web, or Express self-installed VM in Lightweight mode

  az sql vm create --name <vm_name> --resource-group <resource_group_name> --location <vm_location> --license-type PAYG 
  ```
---

Wenn die SQL-IaaS-Erweiterung manuell auf der VM installiert wurde, können Sie sich im vollständigen Modus beim SQL-VM-Ressourcenanbieter registrieren, indem Sie einfach eine Metadatenressource vom Typ Microsoft.SqlVirtualMachine/SqlVirtualMachines erstellen. Nachstehend finden Sie den Codeausschnitt zum Registrieren beim SQL-VM-Ressourcenanbieter, wenn die SQL-IaaS-Erweiterung bereits auf der VM installiert ist. Sie müssen den gewünschten SQL Server-Lizenztyp entweder mit „PAYG“ oder „AHUB“ angeben. Verwenden Sie den folgenden PowerShell-Befehl, um sich im vollständigen Verwaltungsmodus zu registrieren:

  ```powershell-interactive
  # Get the existing  Compute VM
   $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
        
   # Register with SQL VM resource provider
   New-AzResource -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
      -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines `
      -Properties @{virtualMachineResourceId=$vm.Id;SqlServerLicenseType='PAYG'}
  ```


## <a name="register-sql-server-2008-or-2008-r2-on-windows-server-2008-vms"></a>Registrieren von SQL Server 2008 oder 2008 R2 auf Windows Server 2008-VMs

Installationen von SQL Server 2008 und 2008 R2 unter Windows Server 2008 können beim SQL-VM-Ressourcenanbieter im Modus [NoAgent](#change-management-modes) registriert werden. Diese Option stellt die Konformität sicher und ermöglicht die Überwachung der SQL Server-VM im Azure-Portal mit eingeschränktem Funktionsumfang.

In der folgenden Tabelle werden die während der Registrierung angegebenen zulässigen Werte für die Parameter aufgeführt:

| Parameter | Zulässige Werte                                 |
| :------------------| :--------------------------------------- |
| **sqlLicenseType** | `AHUB` oder `PAYG`                    |
| **sqlImageOffer**  | `SQL2008-WS2008` oder `SQL2008R2-WS2008`|
| &nbsp;             | &nbsp;                                   |


Verwenden Sie für die Registrierung Ihrer SQL Server 2008- oder 2008 R2-Instanz bei Ihrer Windows Server 2008-Instanz den folgenden Codeausschnitt für PowerShell oder die Azure-Befehlszeilenschnittstelle:  

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
  ```powershell-interactive
     # Get the existing compute VM
     $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
    New-AzResource -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
      -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines `
      -Properties @{virtualMachineResourceId=$vm.Id;SqlServerLicenseType='PAYG'; `
       sqlManagement='NoAgent';sqlImageSku='Standard';sqlImageOffer='SQL2008R2-WS2008'}
  ```

# <a name="az-clitabbash"></a>[Azure CLI](#tab/bash)

  ```azurecli-interactive
   az sql vm create -n sqlvm -g myresourcegroup -l eastus |
   --license-type PAYG --sql-mgmt-type NoAgent 
   --image-sku Enterprise --image-offer SQL2008-WS2008R2
 ```

---

## <a name="verify-registration-status"></a>Überprüfen des Registrierungsstatus
Sie können über das Azure-Portal, die Azure-Befehlszeilenschnittstelle oder PowerShell überprüfen, ob Ihre SQL Server-VM bereits beim SQL-VM-Ressourcenanbieter registriert wurde. 

### <a name="azure-portal"></a>Azure-Portal 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 
1. Navigieren Sie zu Ihren [SQL Server-VMs](virtual-machines-windows-sql-manage-portal.md).
1. Wählen Sie die SQL Server-VM in der Liste aus. Wenn Ihre SQL Server-VM hier nicht aufgeführt ist, wurde sie wahrscheinlich nicht beim SQL-VM-Ressourcenanbieter registriert. 
1. Überprüfen Sie den Wert unter **Status**. Wenn **Status** den Wert **Erfolgreich** aufweist, wurde die SQL Server-VM beim SQL-VM-Ressourcenanbieter registriert. 

![Überprüfen des Status per SQL RP-Registrierung](media/virtual-machines-windows-sql-register-with-rp/verify-registration-status.png)

### <a name="command-line"></a>Befehlszeile

Überprüfen Sie den aktuellen Registrierungsstatus der SQL Server-VM per Azure-Befehlszeilenschnittstelle oder PowerShell. `ProvisioningState` zeigt `Succeeded` an, wenn die Registrierung erfolgreich abgeschlossen wurde. 

# <a name="az-clitabbash"></a>[Azure CLI](#tab/bash)


  ```azurecli-interactive
  az sql vm show -n <vm_name> -g <resource_group>
 ```


# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)

  ```powershell-interactive
  Get-AzResource -ResourceName <vm_name> -ResourceGroupName <resource_group> `
  -ResourceType Microsoft.SqlVirtualMachine/sqlVirtualMachines
  ```

---

Ein Fehler zeigt an, dass die SQL Server-VM nicht beim Ressourcenanbieter registriert wurde. 

## <a name="change-management-modes"></a>Change Management-Modi

Es gibt drei kostenlose Verwaltbarkeitsmodi für die SQL Server-IaaS-Erweiterung: 

- Der Modus **Vollständig** bietet alle Funktionen, erfordert aber einen Neustart des SQL Server-Computers sowie Systemadministratorberechtigungen. Diese Option wird standardmäßig installiert. Verwenden Sie diesen Modus zum Verwalten einer SQL Server-VM mit einer einzelnen Instanz. Für den Modus „Vollständig“ werden zwei Windows-Dienste installiert, die minimale Auswirkungen auf den Arbeitsspeicher und die CPU haben. Diese Dienste können über den Task-Manager überwacht werden. Mit der Verwendung des Verwaltbarkeitsmodus „Vollständig“ sind keine Kosten verbunden. 

- **Lightweight** erfordert keinen Neustart von SQL Server, unterstützt jedoch nur das Ändern des Lizenztyps und der Edition von SQL Server. Verwenden Sie diese Option für SQL Server-VMs mit mehreren Instanzen oder für die Teilnahme an einer Failoverclusterinstanz (FCI). Es gibt keine Auswirkung auf den Arbeitsspeicher oder die CPU, wenn der Modus „Lightweight“ verwendet wird. Mit der Verwendung des Verwaltbarkeitsmodus „Lightweight“ sind keine Kosten verbunden. 

- **NoAgent** ist speziell für Installationen von SQL Server 2008 und SQL Server 2008 R2 unter Windows Server 2008 vorgesehen. Es gibt keine Auswirkung auf den Arbeitsspeicher oder die CPU, wenn der Modus „NoAgent“ verwendet wird. Mit der Verwendung des Verwaltbarkeitsmodus „NoAgent“ sind keine Kosten verbunden. 

Über PowerShell können Sie den aktuellen Modus des SQL Server-IaaS-Agents anzeigen: 

  ```powershell-interactive
     #Get the SqlVirtualMachine
     $sqlvm = Get-AzResource -Name $vm.Name  -ResourceGroupName $vm.ResourceGroupName  -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines
     $sqlvm.Properties.sqlManagement
  ```

Für SQL Server-VMs, auf denen die IaaS-Erweiterung im Modus *Lightweight* installiert ist, können Sie im Azure-Portal ein Upgrade auf den Modus _Vollständig_ durchführen. Für SQL Server-VMs im Modus _NoAgent_ kann ein Upgrade auf _Vollständig_ durchgeführt werden, nachdem das Betriebssystem auf Windows 2008 R2 oder höher aktualisiert wurde. Es ist nicht möglich, ein Downgrade durchzuführen. Stattdessen müssen Sie die Registrierung der SQL Server-VM des SQL-VM-Ressourcenanbieters [aufheben](#unregister-vm-from-resource-provider), indem Sie die SQL-VM-Ressource löschen und erneut beim SQL-VM-Ressourcenanbieter registrieren. 

So führen Sie Upgrade des Agent-Modus auf „Vollständig“ durch: 


### <a name="azure-portal"></a>Azure-Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wechseln Sie zu Ihrer Ressource [Virtuelle SQL-Computer](virtual-machines-windows-sql-manage-portal.md#access-the-sql-virtual-machines-resource). 
1. Wählen Sie Ihren virtuellen SQL Server-Computer aus, und klicken Sie auf **Übersicht**. 
1. Wählen Sie für SQL Server-VMs mit dem IaaS-Modus „NoAgent“ oder „Lightweight“ die Meldung **Mit der SQL-IaaS-Erweiterung sind nur Lizenztyp- und Editionsaktualisierungen verfügbar** aus.

   ![Auswahlmöglichkeiten zum Ändern des Modus im Portal](media/virtual-machines-windows-sql-server-agent-extension/change-sql-iaas-mode-portal.png)

1. Aktivieren Sie das Kontrollkästchen **Ich stimme dem Neustart des SQL Server-Diensts auf dem virtuellen Computer** zu, und klicken Sie dann auf **Bestätigen**, um ein Upgrade des IaaS-Modus auf „Vollständig“ durchzuführen. 

    ![Kontrollkästchen für die Zustimmung zum Neustart des SQL Server-Diensts auf dem virtuellen Computer](media/virtual-machines-windows-sql-server-agent-extension/enable-full-mode-iaas.png)

### <a name="command-line"></a>Befehlszeile

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

## <a name="register-subscription-with-rp"></a>Registrieren eines Abonnements mit RP

Um Ihre SQL Server-VM beim SQL-VM-Ressourcenanbieter zu registrieren, müssen Sie zunächst Ihr Abonnement beim Ressourcenanbieter registrieren.  Verwenden Sie hierzu das Azure-Portal, die Azure-Befehlszeilenschnittstelle oder PowerShell.

### <a name="azure-portal"></a>Azure-Portal

1. Öffnen Sie das Azure-Portal, und wechseln Sie zu **Alle Dienste**. 
1. Wechseln Sie zu **Abonnements**, und wählen Sie das gewünschte Abonnement aus.  
1. Wechseln Sie auf der Seite **Abonnements** zu **Ressourcenanbieter**. 
1. Geben Sie im Filter **sql** ein, um die SQL-bezogenen Ressourcenanbieter aufzurufen. 
1. Wählen Sie je nach gewünschter Aktion für den Anbieter **Microsoft.SqlVirtualMachine** eine der Optionen **Registrieren**, **Erneut registrieren** oder **Registrierung aufheben** aus. 

![Ändern des Anbieters](media/virtual-machines-windows-sql-ahb/select-resource-provider-sql.png)


### <a name="command-line"></a>Befehlszeile

Registrieren Sie Ihren SQL-VM-Ressourcenanbieter mithilfe von Azure-Befehlszeilenschnittstelle oder PowerShell bei Ihrem Azure-Abonnement. 

# <a name="az-clitabbash"></a>[Azure CLI](#tab/bash)
Der folgende Codeausschnitt registriert den SQL-VM-Ressourcenanbieter bei Ihrem Azure-Abonnement. 

```azurecli-interactive
# Register the new SQL VM resource provider to your subscription 
az provider register --namespace Microsoft.SqlVirtualMachine 
```

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)

```powershell-interactive
# Register the new SQL VM resource provider to your subscription
Register-AzResourceProvider -ProviderNamespace Microsoft.SqlVirtualMachine
```
---

## <a name="unregister-vm-from-resource-provider"></a>Aufheben der Registrierung der VM beim Ressourcenanbieter 

Um die Registrierung Ihrer SQL Server-VM beim SQL-VM-Ressourcenanbieter aufzuheben, löschen Sie die SQL-VM-*Ressource* über das Azure-Portal oder die Azure-Befehlszeilenschnittstelle. Das Löschen der SQL-VM-*Ressource* löscht nicht die SQL Server-VM. Seien Sie jedoch vorsichtig, und führen Sie die Schritte sorgfältig aus, da es möglich ist, den virtuellen Computer versehentlich zu löschen, wenn Sie versuchen, die *Ressource* zu entfernen. 

Das Aufheben der Registrierung der SQL-VM beim SQL-VM-Ressourcenanbieter ist notwendig, um den Verwaltungsmodus von „Vollständig“ herabzustufen. 

### <a name="azure-portal"></a>Azure-Portal

Gehen Sie wie folgt vor, um Ihre SQL Server-VM beim Ressourcenanbieter über das Azure-Portal abzumelden:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Navigieren Sie zur SQL Server-VM-Ressource. 
  
   ![Ressource „Virtuelle SQL-Computer“](media/virtual-machines-windows-sql-manage-portal/sql-vm-manage.png)

1. Klicken Sie auf **Löschen**. 

   ![Löschen des SQL-VM-Ressourcenanbieters](media/virtual-machines-windows-sql-register-with-rp/delete-sql-vm-resource-provider.png)

1. Geben Sie den Namen der SQL-VM ein, und **deaktivieren Sie das Kontrollkästchen neben dem virtuellen Computer**.

   ![Löschen des SQL-VM-Ressourcenanbieters](media/virtual-machines-windows-sql-register-with-rp/confirm-delete-of-resource-uncheck-box.png)

   >[!WARNING]
   > Wenn Sie das Kontrollkästchen neben dem Namen des virtuellen Computers nicht deaktivieren, wird der virtuelle Computer vollständig *gelöscht*. Deaktivieren Sie das Kontrollkästchen, um die Registrierung der SQL Server-VM beim Ressourcenanbieter aufzuheben, aber *den aktuellen virtuellen Computer nicht zu löschen*. 

1. Wählen Sie **Löschen** aus, um das Löschen der SQL-VM-*Ressource* und nicht der SQL Server-VM zu bestätigen. 


### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle 

Um die Registrierung Ihrer SQL Server-VM beim Ressourcenanbieter mithilfe der Azure-Befehlszeilenschnittstelle aufzuheben, verwenden Sie den Befehl [az sql vm delete](/cli/azure/sql/vm?view=azure-cli-latest#az-sql-vm-delete). Dadurch wird die SQL Server-VM-*Ressource* entfernt, aber der virtuelle Computer nicht gelöscht. 


```azurecli-interactive
   az sql vm delete 
     --name <SQL VM resource name> |
     --resource-group <Resource group name> |
     --yes 
```



## <a name="remarks"></a>Anmerkungen

- Der SQL-VM-Ressourcenanbieter unterstützt ausschließlich SQL Server-VMs, die über Azure Resource Manager bereitgestellt wurden. SQL Server-VMs, die mit dem klassischen Modell bereitgestellt wurden, werden nicht unterstützt. 
- Der SQL-VM-Ressourcenanbieter unterstützt ausschließlich SQL Server-VMs, die in der öffentlichen Cloud bereitgestellt wurden. Bereitstellungen in der Private Cloud oder der Government Cloud werden nicht unterstützt. 
 

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen 

**Sollte ich meine SQL Server-VM registrieren, die über ein SQL Server-Image im Azure Marketplace bereitgestellt wird?**

Nein. Microsoft registriert VMs, die über die SQL Server-Images im Azure Marketplace bereitgestellt werden, automatisch. Eine Registrierung beim SQL-VM-Ressourcenanbieter ist nur erforderlich, wenn die VM *nicht* über die SQL Server-Images im Azure Marketplace bereitgestellt und SQL Server selbst installiert wurde.

**Ist der SQL-VM-Ressourcenanbieter für alle Kunden verfügbar?** 

Ja. Kunden sollten Ihre SQL Server-VM beim SQL-VM-Ressourcenanbieter registrieren, wenn sie kein SQL Server-Image aus dem Azure Marketplace verwenden, sondern stattdessen SQL Server selbst installiert oder ihre benutzerdefinierte VHD verwendet haben. Beim SQL-VM-Ressourcenanbieter können VMs im Besitz aller Abonnementtypen (direkt, Enterprise Agreement und Cloud Solution Provider) registriert werden.

**Sollte ich mich beim SQL-VM-Ressourcenanbieter registrieren, wenn bei meiner SQL Server-VM die SQL Server-IaaS-Erweiterung bereits installiert wurde?**

Wenn Ihre SQL Server-VM selbst installiert und nicht über die SQL-Images im Azure Marketplace bereitgestellt wurde, sollten Sie sich beim SQL-VM-Ressourcenanbieter auch dann registrieren, wenn Sie die SQL Server-IaaS-Erweiterung installiert haben. Bei der Registrierung beim SQL-VM-Ressourcenanbieter wird eine neue Ressource vom Typ „Microsoft.SqlVirtualMachines“ erstellt. Bei der Installation der SQL Server-IaaS-Erweiterung wird diese Ressource nicht erstellt.

**Was ist der Standardverwaltungsmodus bei der Registrierung beim SQL-VM-Ressourcenanbieter?**

Der Standardverwaltungsmodus bei der Registrierung beim SQL-VM-Ressourcenanbieter ist *Vollständig*. Wenn die Eigenschaft zur Verwaltung von SQL Server bei der Registrierung beim SQL-VM-Ressourcenanbieter nicht festgelegt wurde, wird der Verwaltungsmodus „Vollständig“ festgelegt. Die Installation der SQL Server-IaaS-Erweiterung auf der VM ist die Voraussetzung für die Registrierung beim SQL-VM-Ressourcenanbieter im Verwaltungsmodus „Vollständig“.

**Was sind die Voraussetzungen für die Registrierung beim SQL-VM-Ressourcenanbieter?**

Es gibt keine Voraussetzungen für die Registrierung beim SQL-VM-Ressourcenanbieter im Lightweight- oder NoAgent-Modus. Als Voraussetzung für die Registrierung beim SQL-VM-Ressourcenanbieter im Modus „Vollständig“ muss die SQL Server-IaaS-Erweiterung auf der VM installiert sein.

**Kann ich mich beim SQL-VM-Ressourcenanbieter registrieren, wenn die SQL Server-IaaS-Erweiterung nicht auf der VM installiert ist?**

Ja, Sie können sich beim SQL-VM-Ressourcenanbieter im Lightweight-Verwaltungsmodus registrieren, wenn die SQL Server-IaaS-Erweiterung auf der VM nicht installiert wurde. Im Lightweight-Modus verwendet der SQL-VM-Ressourcenanbieter eine Konsolen-App, um die Version und Edition der SQL Server-Instanz zu überprüfen. 

Der SQL-Standardverwaltungsmodus bei der Registrierung beim SQL-VM-Ressourcenanbieter ist _Vollständig_. Wenn die Eigenschaft „SQL-Verwaltung“ bei der Registrierung beim SQL-VM-Ressourcenanbieter nicht festgelegt wird, wird für den Modus „Vollständige Verwaltbarkeit“ festgelegt. Die Installation der SQL-IaaS-Erweiterung auf der VM ist die Voraussetzung für die Registrierung beim SQL-VM-Ressourcenanbieter im Verwaltbarkeitsmodus „Vollständig“.

**Wird bei der Registrierung beim SQL-VM-Ressourcenanbieter ein Agent auf meiner VM installiert?**

Nein. Bei der Registrierung beim SQL-VM-Ressourcenanbieter wird lediglich eine neue Metadatenressource erstellt. Es wird kein Agent auf der VM installiert.

Die SQL Server-IaaS-Erweiterung ist nur erforderlich, um vollständige Verwaltbarkeit zu aktivieren. Bei einem Upgrade des Verwaltungsmodus von „Lightweight“ auf „Vollständig“ wird die SQL Server-IaaS-Erweiterung installiert, und SQL Server wird neu gestartet.

**Wird SQL Server durch die Registrierung beim SQL-VM-Ressourcenanbieter auf meiner VM neu gestartet?**

Nein. Bei der Registrierung beim SQL-VM-Ressourcenanbieter wird lediglich eine neue Metadatenressource erstellt. SQL Server wird auf der VM nicht neu gestartet. 

Ein Neustart von SQL Server ist nur erforderlich, um die SQL Server-IaaS-Erweiterung zu installieren. Die SQL Server-IaaS-Erweiterung ist erforderlich, um vollständige Verwaltbarkeit zu aktivieren. Bei einem Upgrade des Verwaltungsmodus von „Lightweight“ auf „Vollständig“ wird die SQL Server-IaaS-Erweiterung installiert, und SQL Server wird neu gestartet.

**Worin besteht der Unterschied zwischen den Verwaltungsmodi „Lightweight“ und „NoAgent“ bei der Registrierung beim SQL-VM-Ressourcenanbieter?** 

Der Verwaltungsmodus „NoAgent“ ist nur für SQL Server 2008 und SQL Server 2008 R2 unter Windows Server 2008 verfügbar. Es handelt sich um den einzigen verfügbaren Verwaltungsmodus für diese Versionen. Für alle anderen Versionen von SQL Server sind die beiden verfügbaren Verwaltungsmodi „Lightweight“ und „Vollständig“. 

Für den Modus „NoAgent“ müssen die Eigenschaften für die SQL Server-Version und die Edition vom Kunden festgelegt werden. Der Modus „Lightweight“ fragt die VM ab, um die Version und Edition der SQL Server-Instanz zu ermitteln.

**Kann ich mich beim SQL-VM-Ressourcenanbieter im Lightweight- oder NoAgent-Modus über die Azure-Befehlszeilenschnittstelle registrieren?**

Nein. Die Eigenschaft für den Verwaltungsmodus steht nur zur Verfügung, wenn die Registrierung beim SQL-VM-Ressourcenanbieter über Azure PowerShell vorgenommen wird. Die Azure-Befehlszeilenschnittstelle unterstützt das Festlegen der SQL Server-Verwaltungseigenschaft nicht. Sie wird immer beim SQL-VM-Ressourcenanbieter im Standardmodus registriert: vollständige Verwaltbarkeit.

**Kann ich mich beim SQL-VM-Ressourcenanbieter registrieren, ohne den SQL Server-Lizenztyp anzugeben?**

Nein. Der SQL Server-Lizenztyp ist bei der Registrierung beim SQL-VM-Ressourcenanbieter keine optionale Eigenschaft. Sie müssen in allen Verwaltbarkeitsmodi („NoAgent“, „Lightweight“ und „Vollständig“) einen der SQL Server-Lizenztypen „nutzungsbasierte Bezahlung“ oder „Azure-Hybridvorteil“ festlegen, wenn Sie sich beim SQL-VM-Ressourcenanbieter über die Azure-Befehlszeilenschnittstelle oder PowerShell registrieren.

**Kann ich die SQL Server-IaaS-Erweiterung vom Modus „NoAgent“ auf „Vollständig“ upgraden?**

Nein. Das Upgrade des SQL-Verwaltungsmodus auf „Vollständig“ oder „Lightweight“ steht für den Modus „NoAgent“ nicht zur Verfügung. Dies ist eine technische Einschränkung von Windows Server 2008.

**Kann ich die SQL Server-IaaS-Erweiterung vom Modus „Lightweight“ auf „Vollständig“ upgraden?**

Ja. Das Upgrade des SQL-Verwaltbarkeitsmodus von „Lightweight“ auf „Vollständig“ wird über PowerShell und das Azure-Portal unterstützt. SQL Server muss dabei neu gestartet werden.

**Kann ich eine Herabstufung der SQL Server-IaaS-Erweiterung vom Modus „Vollständig“ zu den Verwaltungsmodi „NoAgent“ oder „Lightweight“ durchführen?**

Nein. Eine Herabstufung des Verwaltungsmodus für die SQL Server-IaaS-Erweiterung wird nicht unterstützt. Der SQL Server-Verwaltungsmodus kann vom Modus „Vollständig“ nicht auf „Lightweight“ oder „NoAgent“ herabgestuft werden und auch nicht vom Modus „Lightweight“ auf den Modus „NoAgent“. 

Um den Verwaltbarkeitsmodus von „Vollständige Verwaltbarkeit“ zu ändern, heben Sie die [Registrierung](#unregister-vm-from-resource-provider) der SQL Server-VM beim SQL Server-Ressourcenanbieter auf, indem Sie die SQL Server-*Ressource* verwerfen und die SQL Server-VM beim SQL-VM-Ressourcenanbieter in einem anderen Verwaltungsmodus erneut registrieren.

**Kann ich mich über das Azure-Portal beim SQL-VM-Ressourcenanbieter registrieren?**

Nein. Die Registrierung beim SQL-VM-Ressourcenanbieter steht im Azure-Portal nicht zur Verfügung. Die Registrierung beim SQL-VM-Ressourcenanbieter im Verwaltungsmodus „Vollständig“ wird mit der Azure-Befehlszeilenschnittstelle oder PowerShell unterstützt. Die Registrierung beim SQL-VM-Ressourcenanbieter in den Verwaltungsmodi „Lightweight“ und „NoAgent“ wird nur über Azure PowerShell-APIs unterstützt.

**Kann ich eine VM beim SQL-VM-Ressourcenanbieter registrieren, bevor SQL Server installiert wird?**

Nein. Die VM muss mindestens eine SQL Server-Instanz aufweisen, damit sie beim SQL-VM-Ressourcenanbieter registriert werden kann. Wenn auf der VM keine SQL Server-Instanz vorhanden ist, weist die neue Ressource Microsoft.SqlVirtualMachine einen Fehlerzustand auf.

**Kann ich eine VM bei einem SQL-VM-Ressourcenanbieter registrieren, wenn mehrere SQL Server-Instanzen vorhanden sind?**

Ja. Der SQL-VM-Ressourcenanbieter registriert nur eine SQL Server-Instanz. Wenn mehrere Instanzen vorhanden sind, registriert der SQL-VM-Ressourcenanbieter die SQL Server-Standardinstanz. Gibt es keine Standardinstanz, wird nur die Registrierung im Lightweight-Modus unterstützt. Für ein Upgrade vom Verwaltbarkeitsmodus „Lightweight“ auf „Vollständig“ sollte entweder die SQL Server-Standardinstanz vorhanden sein, oder die VM sollte nur eine benannte SQL Server-Instanz aufweisen.

**Kann ich eine SQL Server-Failoverclusterinstanz beim SQL-VM-Ressourcenanbieter registrieren?**

Ja. SQL Server-Failoverclusterinstanzen auf einer Azure-VM können beim SQL-VM-Ressourcenanbieter im Lightweight-Modus registriert werden. Ein Upgrade von SQL Server-Failoverclusterinstanzen auf den Verwaltungsmodus „Vollständig“ ist jedoch nicht möglich.

**Kann ich meine VM beim SQL-VM-Ressourcenanbieter registrieren, wenn eine Always On-Verfügbarkeitsgruppe konfiguriert wurde?**

Ja. Es gibt keine Einschränkungen beim Registrieren einer SQL Server-Instanz auf einer Azure-VM beim SQL-VM-Ressourcenanbieter, wenn Sie einer Always On-Verfügbarkeitsgruppenkonfiguration angehören.

**Welche Kosten fallen für das Registrieren beim SQL-VM-Ressourcenanbieter oder beim Upgraden auf den Verwaltbarkeitsmodus „Vollständig“ an?**
None (Keine): Für das Registrieren beim SQL-VM-Ressourcenanbieter oder das Verwenden eines der drei Verwaltbarkeitsmodi fallen keine Gebühren an. Das Verwalten Ihrer SQL Server-VM mit dem Ressourcenanbieter ist vollständig kostenlos. 

**Welche Auswirkungen hat ein Verwenden der verschiedenen Verwaltbarkeitsmodi auf die Leistung?**
Es gibt keine Auswirkungen, wenn der Verwaltbarkeitsmodus *NoAgent* oder *Lightweight* verwendet wird. Es gibt minimale Auswirkungen, wenn der Verwaltbarkeitsmodus *Vollständig* von zwei Diensten verwendet wird, die in das Betriebssystem installiert wurden. Diese Dienste können über den Task-Manager überwacht werden. 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Artikeln: 

* [Übersicht über SQL Server auf einem virtuellen Windows-Computer](virtual-machines-windows-sql-server-iaas-overview.md)
* [Häufig gestellte Fragen zu SQL Server auf virtuellen Windows-Computern](virtual-machines-windows-sql-server-iaas-faq.md)
* [Preisinformationen für SQL Server auf virtuellen Windows-Computern](virtual-machines-windows-sql-server-pricing-guidance.md)
* [SQL Server auf Windows-VMs – Versionshinweise](virtual-machines-windows-sql-server-iaas-release-notes.md)
