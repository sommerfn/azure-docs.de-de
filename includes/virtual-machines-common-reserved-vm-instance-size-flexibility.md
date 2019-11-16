---
author: yashar
ms.author: banders
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 11-20-2018
ms.openlocfilehash: 9a7de2b41c8016bdb7849cdef428d6f54a8ccf64
ms.sourcegitcommit: ae8b23ab3488a2bbbf4c7ad49e285352f2d67a68
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74005450"
---
Bei einer für Instanzgrößenflexibilität optimierten reservierten VM-Instanz kann die von Ihnen gekaufte Reservierung für die VM-Größen in derselben Gruppe der Instanzgrößenflexibilität gelten. Wenn Sie beispielsweise eine Reservierung für eine VM-Größe kaufen, die in der DSv2-Serie aufgeführt ist, z. B. Standard_DS5_v2, kann der Reservierungsrabatt für die anderen vier Größen gelten, die in derselben Gruppe der Instanzgrößenflexibilität aufgeführt sind:

- Standard_DS1_v2
- Standard_DS2_v2
- Standard_DS3_v2
- Standard_DS4_v2

Dieser Reservierungsrabatt gilt jedoch nicht für VM-Größen, die in anderen Gruppen der Instanzgrößenflexibilität aufgeführt sind, beispielsweise SKUs in DSv2-Serie, oberer Speicher: Standard_DS11_v2, Standard_DS12_v2 usw.

Innerhalb der Gruppe der Instanzgrößenflexibilität hängt die Anzahl von VMs, für die der Reservierungsrabatt gilt, von der VM-Größe ab, die Sie beim Kauf einer Reservierung auswählen. Sie ist auch von den Größen der VMs abhängig, die Sie ausführen. Die Spalte „Verhältnis“ stellt einen Vergleich des relativen Speicherbedarfs der einzelnen VM-Größen in dieser Gruppe der Instanzgrößenflexibilität dar. Anhand des Verhältniswerts können Sie berechnen, inwieweit der Reservierungsrabatt für die von Ihnen ausgeführten VMs gilt.

## <a name="examples"></a>Beispiele

In den folgenden Beispielen werden die Größen und Verhältnisse in der Tabelle der DSv2-Serie verwendet.

Sie kaufen eine reservierte VM-Instanz der Größe Standard_DS4_v2, für die das Verhältnis bzw. der relative Speicherbedarf im Vergleich zu den anderen Größen in der Serie 8 ist.

- Szenario 1: Sie führen acht VMs der Größe Standard_DS1_v2 mit einem Verhältnis von 1 aus. Ihr Reservierungsrabatt gilt für alle acht VMs.
- Szenario 2: Sie führen zwei VMs der Größe Standard_DS2_v2 mit einem Verhältnis von jeweils 2 aus. Außerdem führen Sie eine VM der Größe Standard_DS3_v2 mit einem Verhältnis von 4 aus. Der gesamte Speicherbedarf ist 2 + 2 + 4 = 8. Ihr Reservierungsrabatt gilt somit für alle drei VMs.
- Szenario 3: Sie führen eine VM der Größe Standard_DS5_v2 mit einem Verhältnis von 16 aus. Ihr Reservierungsrabatt gilt für die Hälfte der Computekosten dieser VM.

Den folgenden Abschnitten können Sie entnehmen, welche Größen in derselben Gruppe einer Größenserie enthalten sind, wenn Sie eine für Flexibilität bei der Instanzgröße optimierte reservierte VM-Instanz kaufen.

## <a name="instance-size-flexibility-ratio-for-vms"></a>Verhältnis der Instanzgrößenflexibilität für VMs 

Das CSV unten enthält die Gruppen der Instanzgrößenflexibilität, den ArmSkuName und die Verhältnisse.  

[Verhältnisse der Instanzgrößenflexibilität](https://isfratio.blob.core.windows.net/isfratio/ISFRatio.csv)

Datei-URL und Schema bleiben fest, damit Sie diese Datei programmgesteuert verwenden können. Die Daten werden in Kürze auch über API verfügbar sein.
