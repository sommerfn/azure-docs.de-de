---
title: include file
description: include file
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 04/23/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 40ae634897361219c39e60d2161d3576cc44a400
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67077514"
---
Sie können zum schnellen Erstellen eines VNET in diesem Artikel auf die Option zum Ausprobieren klicken, um in Azure Cloud Shell eine PowerShell-Konsole zu öffnen. Passen Sie die Werte an, und kopieren Sie die Befehle dann in das Konsolenfenster. 

Stellen Sie sicher, dass sich der Adressraum für das von Ihnen erstellte VNET nicht mit den Adressbereichen anderer VNETs, mit denen Sie eine Verbindung herstellen möchten, oder mit Ihren Adressräumen des lokalen Netzwerks überlappt.

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Wenn Sie nicht bereits über eine Ressourcengruppe verfügen, erstellen Sie eine neue. Passen Sie die PowerShell-Befehle gemäß dem gewünschten Ressourcengruppennamen an, und führen Sie dann das folgende Cmdlet aus:

```azurepowershell-interactive
New-AzResourceGroup -ResourceGroupName WANTestRG -Location WestUS
```

### <a name="create-a-vnet"></a>Erstellen eines VNET

Passen Sie die PowerShell-Befehle an, um ein VNET zu erstellen, das für Ihre Umgebung kompatibel ist.

```azurepowershell-interactive
$fesub1 = New-AzVirtualNetworkSubnetConfig -Name FrontEnd -AddressPrefix "10.1.0.0/24"
$vnet   = New-AzVirtualNetwork `
            -Name WANVNet1 `
            -ResourceGroupName WANTestRG `
            -Location WestUS `
            -AddressPrefix "10.1.0.0/16" `
            -Subnet $fesub1
```
