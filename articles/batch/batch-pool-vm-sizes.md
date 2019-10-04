---
title: Auswählen von VM-Größen für Pools – Azure Batch | Microsoft-Dokumentation
description: Wählen aus den verfügbaren VM-Größen für Computeknoten in Azure Batch-Pools
services: batch
documentationcenter: ''
author: laurenhughes
manager: gwallace
editor: ''
ms.assetid: ''
ms.service: batch
ms.workload: ''
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 09/12/2019
ms.author: lahugh
ms.custom: seodec18
ms.openlocfilehash: e3d96ad7783c43dba6b69f6b11acccf790d0d6c9
ms.sourcegitcommit: 909ca340773b7b6db87d3fb60d1978136d2a96b0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2019
ms.locfileid: "70983750"
---
# <a name="choose-a-vm-size-for-compute-nodes-in-an-azure-batch-pool"></a>Auswählen einer VM-Größe für Computeknoten in einem Azure Batch-Pool

Wenn Sie eine Knotengröße für einen Azure Batch-Pool wählen, können Sie aus fast allen in Azure verfügbaren VM-Größen wählen. Azure bietet eine Reihe von Größen für virtuelle Linux- und Windows-Computer für verschiedene Workloads.

Bei der Auswahl einer VM-Größe gelten einige Ausnahmen und Einschränkungen:

* Einige VM-Serien oder VM-Größen werden in Batch nicht unterstützt.
* Einige VM-Größen sind eingeschränkt und müssen explizit aktiviert werden, damit sie zugeordnet werden können.

## <a name="supported-vm-series-and-sizes"></a>Unterstützte VM-Serien und -Größen

### <a name="pools-in-virtual-machine-configuration"></a>Pools in der Konfiguration des virtuellen Computers

Batch-Pools in der Konfiguration des virtuellen Computers unterstützen nahezu alle VM-Größen ([Linux](../virtual-machines/linux/sizes.md), [Windows](../virtual-machines/windows/sizes.md)). In der folgenden Tabelle finden Sie weitere Informationen zu unterstützten Größen und Einschränkungen.

Für alle nicht aufgeführten Vorschauversionen von VM-Größen oder VM-Größen aus Werbeangeboten kann die Unterstützung nicht gewährleistet werden.

| VM-Serie  | Unterstützte Größen | Poolzuordnungsmodus des Batch-Kontos<sup>1</sup> |
|------------|---------|-----------------|
| Serie „Basic A“ | Alle Größen *außer* Basic_A0 (A0) | Any |
| A-Serie | Alle Größen *außer* Standard_A0 | Any |
| Av2-Serie | Alle Größen | Any |
| B-Serie | Keine | Nicht verfügbar |
| DC-Serie | Keine | Nicht verfügbar |
| Dv2/DSv2-Serie | Alle Größen | Any |
| Dv3/DSv3-Serie | Alle Größen | Any |
| Ev3/ESv3-Serie | Alle Größen | Any |
| Fsv2-Serie | Alle Größen | Any |
| H-Reihe | Alle Größen | Any |
| HB-Serie<sup>2</sup> | Alle Größen | Any |
| HC-Serie<sup>2</sup> | Alle Größen | Any |
| Ls-Serie | Alle Größen | Any |
| Lsv2-Reihe | Keine | Nicht verfügbar |
| M-Serie | Standard_M64ms (nur mit niedriger Priorität), Standard_M128s (nur mit niedriger Priorität) | Any |
| Mv2-Serie | Keine | Nicht verfügbar |
| NC-Serie | Alle Größen | Any |
| NCv2-Serie<sup>2</sup> | Alle Größen | Any |
| NCv3-Serie<sup>2</sup> | Alle Größen | Any |
| ND-Serie<sup>2</sup> | Alle Größen | Any |
| NDv2-Serie | Alle Größen | Modus „Benutzerabonnement“ |
| NV-Serie | Alle Größen | Any |
| NVv3-Serie | Keine | Nicht verfügbar |
| SAP HANA | Keine | Nicht verfügbar |

<sup>1</sup> Einige neuere VM-Serien werden anfänglich nur teilweise unterstützt. Diese VM-Serien können von Batch-Konten zugeordnet werden, bei denen der **Poolzuordnungsmodus** auf **Benutzerabonnement** festgelegt ist. Weitere Informationen zur Konfiguration von Batch-Konten finden Sie unter [Verwalten von Batch-Konten](batch-account-create-portal.md#additional-configuration-for-user-subscription-mode). Unter [Kontingente und Grenzwerte](batch-quota-limit.md) erfahren Sie, wie Sie Kontingente für diese teilweise unterstützten VM-Serien für Batch-Konten mit Poolzuordnungsmodus **Benutzerabonnement** anfordern.  

<sup>2</sup> Diese VM-Größen können bei der Konfiguration des virtuellen Computers in Batch-Pools zugewiesen werden. Sie müssen jedoch eine bestimmte [Kontingenterhöhung](batch-quota-limit.md#increase-a-quota) anfordern.

### <a name="pools-in-cloud-service-configuration"></a>Pools in der Clouddienstkonfiguration

Batch-Pools in der Clouddienstkonfiguration unterstützen alle [VM-Größen für Cloud Services](../cloud-services/cloud-services-sizes-specs.md) **außer** den folgenden:

| VM-Serie  | Nicht unterstützte Größen |
|------------|-------------------|
| A-Serie   | Sehr klein       |
| Av2-Serie | Standard_A1_v2, Standard_A2_v2, Standard_A2m_v2 |

## <a name="size-considerations"></a>Überlegungen zu Größen

* **Anwendungsanforderungen**: Berücksichtigen Sie die Merkmale und Anforderungen der Anwendungen, die auf den Knoten ausgeführt werden sollen. Die Beantwortung der Fragen, ob es sich beispielsweise um eine Multithreadanwendung handelt und wie viel Arbeitsspeicher sie beansprucht, kann Ihnen dabei behilflich sein, die am besten geeignete und kostengünstigste Knotengröße zu bestimmen. Ziehen Sie für [MPI-Workloads](batch-mpi.md) mit mehreren Instanzen oder CUDA-Anwendungen spezielle [HPC](../virtual-machines/linux/sizes-hpc.md)- bzw. [GPU-fähige ](../virtual-machines/linux/sizes-gpu.md) VM-Größen in Betracht. (Informationen dazu finden Sie unter [Verwenden RDMA-fähiger oder GPU-fähiger Instanzen in Batch-Pools](batch-pool-compute-intensive-sizes.md).)

* **Aufgaben pro Knoten**: Die Knotengröße wird normalerweise unter der Annahme ausgewählt, dass jeweils nur eine Aufgabe auf einem Knoten ausgeführt wird. Es kann jedoch von Vorteil sein, mehrere Aufgaben (und somit mehrere Anwendungsinstanzen) während der Auftragsausführung auf Computeknoten [parallel zu nutzen](batch-parallel-node-tasks.md). In diesem Fall wird häufig eine Knotengröße mit mehreren Kernen gewählt, um den höheren Bedarf an parallelen Aufgabenausführungen decken zu können.

* **Auslastungsgrade für verschiedene Aufgaben**: Alle Knoten in einem Pool haben die gleiche Größe. Wenn Sie Anwendungen mit unterschiedlichen Systemanforderungen bzw. Auslastungsgraden ausführen möchten, empfehlen wir Ihnen die Verwendung von separaten Pools.

* **Regionale Verfügbarkeit**: Eine VM-Serie oder -Größe ist in den Regionen, in denen Sie die Batch-Konten erstellen, möglicherweise nicht verfügbar. Informationen dazu, welche Größen verfügbar sind, finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/regions/services/).

* **Kontingente**: Die [Kernkontingente](batch-quota-limit.md#resource-quotas) in Ihrem Batch-Konto können die Anzahl der Knoten einer bestimmten Größe beschränken, die Sie einem Batch-Pool hinzufügen können. Informationen zum Anfordern einer Kontingenterhöhung finden Sie in [diesem Artikel](batch-quota-limit.md#increase-a-quota). 

* **Poolkonfiguration**: Im Allgemeinen stehen Ihnen mehr VM-Größenoptionen zur Verfügung, wenn Sie einen Pool in der Konfiguration des virtuellen Computers anstatt in der Clouddienstkonfiguration erstellen.

## <a name="next-steps"></a>Nächste Schritte

* Eine detaillierte Übersicht über Batch finden Sie unter [Entwickeln von parallelen Computelösungen in größerem Umfang mit Batch](batch-api-basics.md).
* Informationen zur Verwendung von rechenintensiven VM-Größen finden Sie unter [Verwenden RDMA-fähiger oder GPU-fähiger Instanzen in Batch-Pools](batch-pool-compute-intensive-sizes.md).
