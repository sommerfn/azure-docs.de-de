---
title: Wählen Sie die richtige VM-SKU für Ihren Azure Data Explorer-Cluster aus.
description: Dieser Artikel beschreibt, wie Sie die optimale SKU-Größe für den Azure Data Explorer-Cluster auswählen.
author: avnera
ms.author: avnera
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 07/14/2019
ms.openlocfilehash: b0cf6eab86b0b932e44b6824305c23df01f35808
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2019
ms.locfileid: "68383816"
---
# <a name="select-the-correct-vm-sku-for-your-azure-data-explorer-cluster"></a>Wählen Sie die richtige VM-SKU für Ihren Azure Data Explorer-Cluster aus. 

Azure Data Explorer verfügt über mehrere VM-SKUs, aus denen Sie wählen können, wenn Sie einen neuen Cluster erstellen oder den Cluster für eine sich ändernde Workload optimieren. Die VMs wurden sorgfältig ausgewählt, um die optimalsten Kosten für jede Workload zu erzielen. 

Die Größe und VM-SKU des Datenverwaltungsclusters wird vollständig vom Azure Data Explorer-Dienst verwaltet. Sie wird durch Faktoren wie die VM-Größe der Engine und die Erfasstungsworkload bestimmt. 

Die VM-SKU für den Engine-Cluster kann durch [Skalierung des Clusters](manage-cluster-vertical-scaling.md) jederzeit geändert werden. Daher ist es am besten, mit der minimalen SKU-Größe zu beginnen, die zum Ausgangsszenario passt. Beachten Sie, dass die Skalierung des Clusters zu Ausfallzeiten von bis zu 30 Minuten führt, während der Cluster mit der neuen VM-SKU neu erstellt wird.

> [!TIP]
> Die Berechnung von [RI (reservierte Instanzen)](https://docs.microsoft.com/azure/virtual-machines/windows/prepay-reserved-vm-instances) ist für den Azure Data Explorer-Cluster anwendbar.  

Dieser Artikel beschreibt die verschiedenen VM-SKU-Optionen und liefert die technischen Details, die Ihnen helfen, die optimale Wahl zu treffen.

## <a name="select-the-cluster-type"></a>Auswählen des Clustertyps

Azure Data Explorer stellt zwei Arten von Clustern bereit:

* **Produktion**: Produktionscluster enthalten zwei Knoten für Engine und Datenverwaltungscluster und werden unter dem Azure Data Explorer-[SLA](https://azure.microsoft.com/support/legal/sla/data-explorer/v1_0/) betrieben.

* **Dev/Test (kein SLA)** : Dev/Test-Cluster haben einen einzigen D11_v2-Knoten für den Engine-Cluster und einen einzigen D1-Knoten für den Datenverwaltungscluster. Dieser Clustertyp ist die kostengünstigste Konfiguration aufgrund der geringen Anzahl von Instanzen und der fehlenden Aufschlaggebühren für die Engine. Für diese Clusterkonfiguration gibt es kein SLA, da ihr die Redundanz fehlt.

## <a name="sku-types"></a>SKU-Typen

Wählen Sie beim Erstellen eines Azure Data Explorer-Clusters die *optimale* VM-SKU für die geplante Workload. Azure Data Explorer hat zwei SKU-Familien zur Auswahl:

* **D_V2**: Die D-SKU ist für Compute optimiert und in zwei Varianten erhältlich.
    * Die VM selbst
    * Die VM mit Premium-Speicher für Datenträger im Paket

* **LS**: Die L-SKU ist datenspeicheroptimiert. Sie hat eine viel größere SSD-Größe als die preislich vergleichbare **D**-SKU.

Die folgende Tabelle zeigt die wesentlichen Unterschiede zwischen den verfügbaren SKU-Typen:
 
|**Attribut** | **D-SKU** | **L-SKU**
|---|---|---
|**Kleine SKUs**|Mindestgröße ist D11 mit zwei Kernen|Mindestgröße ist L4 mit vier Kernen
|**Verfügbarkeit**|In allen Regionen verfügbar (die DS+PS-Version ist nur eingeschränkt verfügbar)|In einigen Region verfügbar
|**Kosten pro GB-Cache pro Kern**|Hoch mit der D-SKU, gering mit der DS+PS-Version|Am günstigsten mit der *nutzungsbasierten* Zahlung
|**Preise für RI (reservierte Instanzen)**|Hoher Rabatt (über 55 % für eine dreijährige Verpflichtung)|Kleiner Rabatt (20 % für eine dreijährige Verpflichtung)  

## <a name="select-your-cluster-vm"></a>Auswählen Ihrer Cluster-VM 

Um Ihre Cluster-VM auszuwählen, [konfigurieren Sie die vertikale Skalierung](manage-cluster-vertical-scaling.md#configure-vertical-scaling). 

Mit den verschiedenen VM-SKU-Optionen können Sie die Kosten für die notwendige Leistung und die Anforderungen des heißen Caches für das gewünschte Szenario optimieren. Wenn das Szenario die optimale Leistung bei hohem Abfragevolumen erfordert, sollte die ideale SKU für Compute optimiert werden. Andererseits, wenn das Szenario die Abfrage großer Datenmengen mit relativ geringer Abfragelast erfordert, reduziert die speicheroptimierte SKU die Kosten bei gleichzeitig exzellenter Leistung.

Die Anzahl der Instanzen pro Cluster für die kleinen SKUs ist begrenzt, daher ist es besser, größere VMs mit mehr RAM zu verwenden. Die RAM-Größe wird für einige Abfragetypen benötigt, die die RAM-Ressource stärker beanspruchen, z. B. für Abfragen, die `joins` verwenden. Daher wird empfohlen, bei der Betrachtung von Skalierungsoptionen auf eine größere SKU zu skalieren, anstatt mehr Instanzen horizontal hinzuzufügen.

## <a name="vm-options"></a>VM-Optionen

Die folgende Tabelle enthält die technischen Spezifikationen für die Cluster-VMs des Azure Data Explorer:

|**Name**| **Kategorie** | **SSD-Größe** | **Kerne** | **RAM** | **Storage Premium-Datenträger (1 TB)**| **Minimale Anzahl der Instanzen pro Cluster** | **Maximale Anzahl der Instanzen pro Cluster**
|---|---|---|---|---|---|---|---
|D11_v2| Computeoptimiert | 75 GB    | 2 | 14 GB | 0 | 1 | 8 (außer für Dev/Test-SKU, denn dort liegt sie bei 1)
|D12_v2| Computeoptimiert | 150 GB   | 4 | 28 GB | 0 | 2 | 16
|D13_v2| Computeoptimiert | 307 GB   | 8 | 56 GB | 0 | 2 | 1\.000
|D14_v2| Computeoptimiert | 614 GB   | 16| 112 GB | 0 | 2 | 1\.000
|DS13_v2+1TB PS| Speicheroptimiert | 1 TB | 8 | 56 GB | 1 | 2 | 1\.000
|DS13_v2+2TB PS| Speicheroptimiert | 2 TB | 8 | 56 GB | 2 | 2 | 1\.000
|DS14_v2+3TB PS| Speicheroptimiert | 3 TB | 16 | 112 GB | 2 | 2 | 1\.000
|DS14_v2+4TB PS| Speicheroptimiert | 4 TB | 16 | 112 GB | 4 | 2 | 1\.000
|L4s_v1| Speicheroptimiert | 650 GB | 4 | 32 GB | 0 | 2 | 16
|L8s_v1| Speicheroptimiert | 1,3 TB | 8 | 64 GB | 0 | 2 | 1\.000
|L16s_1| Speicheroptimiert | 2,6 TB | 16| 128 GB | 0 | 2 | 1\.000

* Zeigen Sie die aktualisierte VM-SKU-Liste pro Region mithilfe der Azure Data Explorer [ListSkus API](/dotnet/api/microsoft.azure.management.kusto.clustersoperationsextensions.listskus?view=azure-dotnet) an. 
* Weitere Informationen zu den [verschiedenen Compute-SKUs](/azure/virtual-machines/windows/sizes-compute). 

## <a name="next-steps"></a>Nächste Schritte

* Der Engine-Cluster kann jederzeit nach [oben oder unten skaliert](manage-cluster-vertical-scaling.md) werden, indem die VM-SKU für unterschiedliche Anforderungen geändert wird. 

* Die Größe des Engine-Clusters kann [horizontal skaliert werden](manage-cluster-horizontal-scaling.md), um die Kapazität bei wechselnden Anforderungen zu verändern.

