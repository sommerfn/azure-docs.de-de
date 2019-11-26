---
title: Zusammenstellen virtueller Windows-Computer in Azure
description: Erfahren Sie, wie das Zusammenstellen von Azure-VM-Ressourcen die Latenz verbessern kann.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 10/30/2019
ms.author: zivr
ms.openlocfilehash: b61a84f2b284177b7443be00efeea65e7eb8aaef
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73171084"
---
# <a name="co-locate-resource-for-improved-latency"></a>Zusammenstellen von Ressourcen für verbesserte Latenzen

Wenn Sie Ihre Anwendung in Azure bereitstellen und die Instanzen auf Regionen oder Verfügbarkeitszonen verteilen, führt dies zu einer Netzwerklatenz, die sich möglicherweise auf die Gesamtleistung Ihrer Anwendung auswirkt. 


## <a name="proximity-placement-groups"></a>Näherungsplatzierungsgruppen 

[!INCLUDE [virtual-machines-common-ppg-overview](../../../includes/virtual-machines-common-ppg-overview.md)]

## <a name="next-steps"></a>Nächste Schritte

Stellen Sie mithilfe von Azure PowerShell eine VM in einer [Näherungsplatzierungsgruppe](proximity-placement-groups.md) bereit.

Erfahren Sie, wie Sie [die Netzwerklatenz testen](https://aka.ms/TestNetworkLatency?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Erfahren Sie, wie Sie [den Netzwerkdurchsatz optimieren](https://docs.microsoft.com/azure/virtual-network/virtual-network-optimize-network-bandwidth?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).  

Erfahren Sie, wie Sie [Näherungsplatzierungsgruppen mit SAP-Anwendungen verwenden](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-proximity-placement-scenarios?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).