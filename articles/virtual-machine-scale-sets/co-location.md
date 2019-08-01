---
title: Zusammenstellen von Azure-VM-Skalierungsgruppen | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Leistung von Ressourcen in Azure-VM-Skalierungsgruppen durch das Zusammenstellen verbessern können.
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
ms.service: virtual-machine-scale-sets
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/14/2019
ms.author: cynthn
ms.openlocfilehash: 555c4da18169ae9fd1e917fd0b8e3c4e98e87178
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2019
ms.locfileid: "67850731"
---
# <a name="co-location"></a>Zusammenstellung

Einer der wichtigsten Gründe für die Latenz zwischen VMs ist einfach die Entfernung.

## <a name="preview-proximity-placement-groups"></a>Vorschau: Näherungsplatzierungsgruppen 

[!INCLUDE [virtual-machines-common-ppg-overview](../../includes/virtual-machines-common-ppg-overview.md)]

## <a name="next-steps"></a>Nächste Schritte

Erstellen Sie eine [Näherungsplatzierungsgruppe](proximity-placement-groups.md) für Ihre Skalierungsgruppe.

