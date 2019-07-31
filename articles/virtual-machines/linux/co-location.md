---
title: Zusammenstellen von Linux-Azure-VMs | Microsoft-Dokumentation
description: Erfahren Sie, wie das Zusammenstellen von Azure-VM-Ressourcen die Latenz senken kann.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 06/28/2019
ms.author: cynthn
ms.openlocfilehash: d6d8986117bd2899ea0de0aa6490954aef6c3a56
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2019
ms.locfileid: "67850671"
---
# <a name="co-locate-resources-for-improved-latency"></a>Zusammenstellen von Ressourcen für geringere Latenz

Wenn Sie Ihre Anwendung in Azure bereitstellen und die Instanzen auf Regionen oder Verfügbarkeitszonen verteilen, führt dies zu einer Netzwerklatenz, die sich möglicherweise auf die Gesamtleistung Ihrer Anwendung auswirkt. 

## <a name="preview-proximity-placement-groups"></a>Vorschau: Näherungsplatzierungsgruppen

[!INCLUDE [virtual-machines-common-ppg-overview](../../../includes/virtual-machines-common-ppg-overview.md)]

## <a name="next-steps"></a>Nächste Schritte

Stellen Sie mithilfe der Azure CLI eine VM in einer [Näherungsplatzierungsgruppe](proximity-placement-groups.md) bereit.

