---
title: Anwenden einer Windows-Lizenz auf virtuelle Sitzungshostcomputer – Azure
description: Hier wird beschrieben, wie die Windows-Lizenz für virtuelle Windows Virtual Desktop-Computer angewendet wird.
services: virtual-desktop
author: ChristianMontoya
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 08/14/2019
ms.author: chrimo
ms.openlocfilehash: af8542ccc8fad8d833d8329999ded2f5a52b3d03
ms.sourcegitcommit: 39d95a11d5937364ca0b01d8ba099752c4128827
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69563849"
---
# <a name="apply-windows-license-to-session-host-virtual-machines"></a>Anwenden einer Windows-Lizenz auf virtuelle Sitzungshostcomputer

Kunden, die ordnungsgemäß für das Ausführen von Windows Virtual Desktop-Workloads lizenziert sind, können eine Windows-Lizenz auf Ihre virtuellen Sitzungshostcomputer anwenden und diese ausführen, ohne für eine weitere Lizenz zu bezahlen. Weitere Informationen finden Sie unter [Windows Virtual Desktop – Preise](https://azure.microsoft.com/pricing/details/virtual-desktop/).

## <a name="ways-to-use-your-windows-virtual-desktop-license"></a>Möglichkeiten zum Verwenden Ihrer Windows Virtual Desktop-Lizenz
Die Windows Virtual Desktop-Lizenzierung gestattet es Ihnen, eine Lizenz auf jeden virtuellen Windows- oder Windows Server-Computer anzuwenden, der als Sitzungshost in einem Hostpool registriert ist und Benutzerverbindungen empfängt. Diese Lizenz gilt nicht für virtuelle Computer, die als Dateifreigabeserver, Domänencontroller usw. ausgeführt werden.

Es gibt einige Möglichkeiten, die Windows Virtual Desktop-Lizenz zu verwenden:
- Sie können das [Azure Marketplace-Angebot](./create-host-pools-azure-marketplace.md) verwenden, um einen Hostpool und dessen virtuelle Sitzungshostcomputer zu erstellen. Virtuellen Computern, die auf diese Weise erstellt werden, wird die Lizenz automatisch zugewiesen.
- Sie können die [Azure Resource Manager-GitHub-Vorlage](./create-host-pools-arm-template.md) verwenden, um einen Hostpool und dessen virtuelle Sitzungshostcomputer zu erstellen. Virtuellen Computern, die auf diese Weise erstellt werden, wird die Lizenz automatisch zugewiesen.
- Sie können eine Lizenz auf einen vorhandenen virtuellen Sitzungshostcomputer anwenden. Führen Sie dazu zunächst die Schritte aus, die in [Erstellen eines Hostpools mit PowerShell](./create-host-pools-powershell.md) aufgeführt sind, um eine Hostpool und zugehörige virtuelle Computer zu erstellen, und kehren Sie dann zu diesem Artikel zurück, um zu erfahren, wie Sie die Lizenz anwenden.

## <a name="apply-a-windows-license-to-a-session-host-vm"></a>Anwenden einer Windows-Lizenz auf einen virtuellen Sitzungshostcomputer
Stellen Sie sicher, dass Sie die [neueste Azure PowerShell-Version installiert und konfiguriert](/powershell/azure/overview)haben. Führen Sie das folgende PowerShell-Cmdlet aus, um die Windows-Lizenz anzuwenden:

```powershell
$vm = Get-AzVM -ResourceGroup <resourceGroupName> -Name <vmName>
$vm.LicenseType = "Windows_Client"
Update-AzVM -ResourceGroupName <resourceGroupName> -VM $vm
```

## <a name="verify-your-session-host-vm-is-utilizing-the-licensing-benefit"></a>Feststellen, ob für den virtuellen Sitzungshostcomputer der Lizenzierungsvorteil genutzt wird
Nachdem Sie Ihren virtuellen Computer bereitgestellt haben, führen Sie das folgende Cmdlet aus, um den Lizenztyp zu überprüfen:
```powershell
Get-AzVM -ResourceGroup <resourceGroupName> -Name <vmName>
```

Für einen virtuellen Sitzungshostcomputer mit der angewendeten Windows-Lizenz werden Informationen angezeigt, die in etwa wie folgt aussehen:

```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : Windows_Client
```

Für virtuelle Computer ohne die angewendete Windows-Lizenz werden Informationen angezeigt, die in etwa wie folgt aussehen:

```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              :
```

Führen Sie das folgende Cmdlet aus, um eine Liste aller virtuellen Sitzungshostcomputer anzuzeigen, auf die in Ihrem Azure-Abonnement die Windows-Lizenz angewendet wurde:

```powershell
$vms = Get-AzVM
$vms | ?{$_.LicenseType -like "Windows_Client"} | select ResourceGroupName, Name, LicenseType
```