---
title: Azure-VMware-Lösung von CloudSimple – Übersicht über Knoten
description: Erfahren Sie mehr über CloudSimple-Knoten und -Konzepte.
author: sharaths-cs
ms.author: dikamath
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 729b832c068dcd401fa6e9d2f4af9193d3859a83
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/03/2019
ms.locfileid: "71845542"
---
# <a name="cloudsimple-nodes-overview"></a>Übersicht über CloudSimple-Knoten

Knoten sind die Bausteine einer privaten Cloud. Ein Knoten ist:

* Ein dedizierter Bare-Metal-Computehost, auf dem ein VMware ESXi-Hypervisor installiert ist  
* Eine Computingeinheit, die Sie erwerben oder reservieren können, um private Clouds zu erstellen
* Verfügbar, um in einer Region gekauft oder reserviert zu werden, in der der CloudSimple-Service verfügbar ist

Sie erstellen eine private Cloud aus den erworbenen Knoten. Um eine private Cloud zu erstellen, benötigen Sie mindestens drei Knoten derselben SKU. Wenn Sie eine private Cloud erweitern möchten, fügen Sie zusätzliche Knoten hinzu.  Sie können Knoten zu einem vorhandenen Cluster hinzufügen oder einen neuen Cluster erstellen, indem Sie Knoten im Azure-Portal erwerben und diese dem CloudSimple-Dienst zuordnen.  Alle erworbenen Knoten werden im CloudSimple-Dienst angezeigt.  

## <a name="purchased-nodes"></a>Gekaufte Knoten

Gekaufte Knoten bieten Kapazität mit nutzungsbasierter Bezahlung. Durch den Kauf von Knoten können Sie Ihren VMware-Cluster nach Bedarf schnell skalieren. Sie können Knoten nach Bedarf hinzufügen oder einen erworbenen Knoten löschen, um Ihren VMware-Cluster zentral herunterzuskalieren. Erworbene Knoten werden monatlich in dem Abonnement in Rechnung gestellt, mit dem sie erworben wurden.

* Wenn Sie Ihr Azure-Abonnement per Kreditkarte bezahlen, wird die Karte sofort belastet.
* Wenn Sie jeweils eine Rechnung erhalten, werden die Gebühren auf Ihrer nächsten Rechnung aufgeführt.

## <a name="vmware-solution-by-cloudsimple-nodes-sku"></a>VMware-Lösung entsprechend SKU für CloudSimple-Knoten

Die folgenden Knotentypen können erworben oder reserviert werden.

| SKU           | CS28-Knoten                 | CS36-Knoten                 | CS36m-Knoten                |
|---------------|-----------------------------|-----------------------------|-----------------------------|
| Region        | USA, Osten; USA, Westen            | USA, Osten; USA, Westen            | Europa, Westen                 |
| CPU           | 2x2,2 GHz, 28 Kerne (56 HT) | 2x2,3 GHz, 36 Kerne (72 HT) | 2x2,3 GHz, 36 Kerne (72 HT) |
| RAM           | 256 GB                      | 512 GB                      | 576 GB                      |
| Cachedatenträger    | 1,6 TB NVMe                 | 3,2 TB NVMe                 | 3,2 TB NVMe                 |
| Datenträgerkapazität | 5,625 TB unformatiert                | 11,25 TB unformatiert                | 15,36 TB unformatiert                |
| Speichertyp  | Nur Flash                   | Nur Flash                   | Nur Flash                   |

## <a name="limits"></a>Einschränkungen

Für private Clouds gelten die folgenden Knotengrenzwerte.

| Resource | Begrenzung |
|----------|-------|
| Mindestanzahl von Knoten zum Erstellen einer privaten Cloud | 3 |
| Maximale Anzahl von Knoten in einem Cluster in einer privaten Cloud | 16 |
| Maximale Anzahl von Knoten in einer privaten Cloud | 64 |
| Mindestanzahl von Knoten in einem neuem Cluster | 3 |

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie Sie [Knoten erwerben](create-nodes.md).
* Erfahren Sie mehr über [private Clouds](cloudsimple-private-cloud.md).
