---
title: Bereitstellen von Azure Firewall mit Verfügbarkeitszonen mithilfe von Azure PowerShell
description: In diesem Artikel erfahren Sie, wie Sie eine Azure Firewall mit Verfügbarkeitszonen mithilfe von Azure PowerShell bereitstellen.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 06/20/2019
ms.author: victorh
ms.openlocfilehash: 2fa6a62a28a1536da83994cb07b7c5fa5d7bda9f
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2019
ms.locfileid: "67276567"
---
# <a name="deploy-an-azure-firewall-with-availability-zones-using-azure-powershell"></a>Bereitstellen einer Azure Firewall mit Verfügbarkeitszonen mithilfe von Azure PowerShell

> [!IMPORTANT]
> Azure Firewall mit Verfügbarkeitszonen befindet sich derzeit in der öffentlichen Vorschauversion.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Zur Erhöhung der Verfügbarkeit kann Azure Firewall während der Bereitstellung so konfiguriert werden, dass mehrere Verfügbarkeitszonen abgedeckt werden.

Diese Funktion ermöglicht die folgenden Szenarios:

- Sie können die Verfügbarkeit der Betriebszeit auf 99,99 % steigern. Weitere Informationen finden Sie in der [Vereinbarung zum Servicelevel (SLA) für Azure Firewall](https://azure.microsoft.com/support/legal/sla/azure-firewall/v1_0/). Die SLA für 99,99 % Betriebszeit wird angeboten, wenn zwei oder mehr Verfügbarkeitszonen ausgewählt werden.
- Unter Verwendung der Standard-SLA des Diensts von 99,95 % können Sie Azure Firewall außerdem aus Gründen der Nähe einer bestimmten Zone zuordnen.

Weitere Informationen zu Azure Firewall-Verfügbarkeitszonen finden Sie unter [What is Azure Firewall? (Was ist Azure Firewall?)](overview.md).

Das folgende Azure PowerShell-Beispiel veranschaulicht das Bereitstellen einer Azure Firewall mit Verfügbarkeitszonen.

## <a name="create-a-firewall-with-availability-zones"></a>Erstellen einer Firewall mit Verfügbarkeitszonen

In diesem Beispiel wird eine Firewall in den Zonen 1, 2 und 3 erstellt.

Beim Erstellen der standardmäßigen öffentlichen IP-Adresse wird keine bestimmte Zone angegeben. Dadurch wird standardmäßig eine zonenredundante IP-Adresse erstellt. Standardmäßige öffentliche IP-Adressen können entweder in allen Zonen oder in einer einzelnen Zone konfiguriert werden.

Dies ist wichtig, da es nicht möglich ist, eine Firewall in Zone 1 und eine IP-Adresse in Zone 2 zu haben. Sie können allerdings eine Firewall in Zone 1 und eine IP-Adresse in allen Zonen bzw. aus Gründen der Nähe eine Firewall und eine IP-Adresse in der gleichen Zone haben.

```azurepowershell
$rgName = "resourceGroupName"

$vnet = Get-AzVirtualNetwork `
  -Name "vnet" `
  -ResourceGroupName $rgName

$pip1 = New-AzPublicIpAddress `
  -Name "AzFwPublicIp1" `
  -ResourceGroupName "rg" `
  -Sku "Standard" `
  -Location "centralus" `
  -AllocationMethod Static

New-AzFirewall `
  -Name "azFw" `
  -ResourceGroupName $rgName `
  -Location centralus `
  -VirtualNetwork $vnet `
  -PublicIpAddress @($pip1)
  -Zone 1,2,3
```

## <a name="next-steps"></a>Nächste Schritte

- [Tutorial: Überwachen von Azure Firewall-Protokollen](./tutorial-diagnostics.md)
