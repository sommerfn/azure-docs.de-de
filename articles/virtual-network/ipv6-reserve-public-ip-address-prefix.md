---
title: Reservieren öffentlicher IPv6-Adressen und -Adressbereiche in einem virtuellen Azure-Netzwerk
titlesuffix: Azure Virtual Network
description: Hier erfahren Sie, wie Sie öffentliche IPv6-Adressen und -Adressbereiche in einem virtuellen Azure-Netzwerk reservieren.
services: virtual-network
documentationcenter: na
author: KumudD
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/15/2019
ms.author: kumud
ms.openlocfilehash: 76d1ba2717ac3c8ac8e86687ef1754a8790f3e4d
ms.sourcegitcommit: b4f201a633775fee96c7e13e176946f6e0e5dd85
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2019
ms.locfileid: "72595084"
---
# <a name="reserve-public-ipv6-address-prefix"></a>Reservieren eines Präfix für öffentliche IPv6-Adressen
IPv6 für Azure Virtual Network (VNET) ermöglicht es Ihnen, Anwendungen in Azure mit IPv6- und IPv4-Konnektivität sowohl innerhalb eines virtuellen Netzwerks als auch für das Internet (in ein- und ausgehender Richtung) bereitzustellen. Zusätzlich zum Reservieren einzelner IPv6-Adressen können Sie zusammenhängende Bereiche von Azure-IPv6-Adressen (als IP-Präfix bezeichnet) zur Verwendung reservieren. In diesem Artikel wird beschrieben, wie Sie öffentliche IPv6-Adressen und -Adressbereiche mithilfe von Azure PowerShell und der Azure CLI erstellen.

> [!Important]
> IPv6 für Azure Virtual Network ist derzeit als öffentliche Vorschauversion verfügbar. Diese Vorschau wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="create-a-single-reserved-ipv6-public-ip"></a>Erstellen einer einzelnen reservierten öffentlichen IPv6-Adresse

### <a name="using-azure-powershell"></a>Verwenden von Azure PowerShell

In Azure PowerShell können Sie eine einzelne reservierte (statische) öffentliche IPv6-Adresse mit [New-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress) erstellen. Gehen Sie dazu wie folgt vor:

```azurepowershell  
 $myOwnIPv6Address = New-AzPublicIpAddress `
 -name PIPv6_WestUS `
 -ResourceGroup MyRG `
 -Location "West US" `
 -Sku Standard `
 -allocationMethod static `
 -IpAddressVersion IPv6
 ```

### <a name="using-azure-cli"></a>Verwenden der Azure-Befehlszeilenschnittstelle

 In der Azure CLI können Sie eine einzelne reservierte (statische) öffentliche IPv6-Adresse mit [az network public-ip create](/cli/azure/network/public-ip) erstellen. Gehen Sie dazu wie folgt vor:
  
```azurecli
 az network public-ip create \
 --name dsPublicIP_v6 \
 --resource-group UpgradeInPlace_CLI_RG1 \
 --location WestUS \
 --sku Standard  \
 --allocation-method static  \
 --version IPv6
```

## <a name="create-a-reserved-ipv6-prefix-range"></a>Erstellen eines reservierten IPv6-Präfix (Bereich)

Zum Reservieren eines IPv6-Präfix fügen Sie die IP-Adressfamilie von IPv6 demselben Befehl hinzu, der zum Erstellen von IPv4-Präfixen verwendet wird. Mit den folgenden Befehlen wird ein Präfix der Größe /125 (8 IPv6-Adressen) erstellt.  

### <a name="using-azure-powershell"></a>Verwenden von Azure PowerShell

In der Azure CLI können Sie eine öffentliche IPv6-Adresse mit [az network public-ip create](/powershell/module/az.network/new-azpublicipprefix) erstellen. Gehen Sie dazu wie folgt vor:
```azurepowershell  
 $myOwnIPv6Prefix = New-AzPublicIpPrefix `
 -name IPv6PrefixWestUS `
 -ResourceGroupName MyRG `
 -Location "West US" `
 -Sku Standard `
 -IpAddressVersion IPv6 `
 -PrefixLength 125
```

### <a name="using-azure-cli"></a>Verwenden der Azure-Befehlszeilenschnittstelle

In der Azure CLI können Sie eine öffentliche IPv6-Adresse wie folgt erstellen:

```azurecli  
az network public-ip prefix create \
--name IPv6PrefixWestUS \
--resource-group MyRG \
--location WestUS \
--version IPv6 \
--length 125
```

## <a name="allocate-a-public-ip-address-from-a-reserved-ipv6-prefix"></a>Zuordnen einer öffentlichen IP-Adresse aus einem reservierten IPv6-Präfix

### <a name="using-azure-powershell"></a>Verwenden von Azure PowerShell

 Zum Erstellen einer statischen öffentlichen IPv6-Adresse aus einem reservierten Präfix fügen Sie beim Erstellen der öffentlichen IP-Adresse in Azure PowerShell das Argument `-PublicIpPrefix` hinzu. Im folgenden Beispiel wird davon ausgegangen, dass ein Präfix erstellt und in einer PowerShell-Variablen mit dem Namen *$myOwnIPv 6prefix* gespeichert wurde.

```azurepowershell:  
 $MyIPv6PublicIPFromMyReservedPrefix = New-AzPublicIpAddress \
 -name PIPv6_fromPrefix `
 -ResourceGroup DsStdLb04 `
 -Location "West Central US" `
 -Sku Standard `
 -allocationMethod static `
 -IpAddressVersion IPv6 `
 -PublicIpPrefix $myOwnIPv6Prefix
```

### <a name="using-azure-cli"></a>Verwenden der Azure-Befehlszeilenschnittstelle
 
Im folgenden Beispiel wird davon ausgegangen, dass ein Präfix erstellt und in der folgenden CLI-Variablen gespeichert wurde: *IPv6PrefixWestUS*.

```azurecli 
az network public-ip create \
--name dsPublicIP_v6 \
--resource-group UpgradeInPlace_CLI_RG1 \
--location WestUS \
--sku Standard \
--allocation-method static \
--version IPv6 \
--public-ip-prefix  IPv6PrefixWestUS
```

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über das [Präfix für IPv6-Adressen](ipv6-public-ip-address-prefix.md).
- Erfahren Sie mehr über [IPv6-Adressen](ipv6-overview.md).
