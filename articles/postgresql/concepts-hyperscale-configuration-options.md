---
title: Azure Database for PostgreSQL – Leistungsoptionen für Hyperscale (Citus)
description: Optionen für eine Hyperscale (Citus)-Servergruppe, einschließlich der Compute- und Speicheroptionen für Knoten und Regionen.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 10/17/2019
ms.openlocfilehash: 2b848827d3d2017e5e787989553c0bf1e26d48e7
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73482637"
---
# <a name="azure-database-for-postgresql--hyperscale-citus-performance-options"></a>Azure Database for PostgreSQL – Leistungsoptionen für Hyperscale (Citus)

## <a name="compute-and-storage"></a>Compute und Speicher
 
Sie können die Compute- und Speichereinstellungen für Workerknoten und den Koordinatorknoten in einer Hyperscale (Citus)-Servergruppe unabhängig voneinander auswählen.  Computeressourcen werden in Form von virtuellen Kernen bereitgestellt und repräsentieren die logische CPU der zugrunde liegenden Hardware. Die Speichergröße für die Bereitstellung bezieht sich auf die für die Koordinator- und Workerknoten in Ihrer Hyperscale (Citus)-Servergruppe verfügbare Kapazität. Der Speicher umfasst Datenbankdateien, temporäre Dateien, Transaktionsprotokolle und die Postgres-Serverprotokolle. Außerdem wird durch die Gesamtmenge an bereitgestelltem Speicher die für jeden Worker- und Koordinatorknoten verfügbare E/A-Kapazität definiert.
 
|                       | Workerknoten           | Koordinatorknoten      |
|-----------------------|-----------------------|-----------------------|
| Compute, virtuelle Kerne       | 4, 8, 16, 32, 64      | 4, 8, 16, 32, 64      |
| Arbeitsspeicher pro V-Kern, GiB | 8                     | 4                     |
| Speichergröße, TiB     | 0,5, 1, 2             | 0,5, 1, 2             |
| Speichertyp          | Allgemein (SSD) | Allgemein (SSD) |
| IOPS                  | Bis zu 3 IOPS/GiB      | Bis zu 3 IOPS/GiB      |


## <a name="regions"></a>Regions
Hyperscale (Citus)-Servergruppen sind in den folgenden Azure-Regionen verfügbar:

* Amerika:
    * Kanada, Mitte*
    * East US
    * USA (Ost) 2
    * USA, Norden-Mitte*
    * USA, Westen 2
* Asien-Pazifik:
    * Australien, Osten*
    * Asien, Südosten
* Europa:
    * Nordeuropa
    * UK, Süden
    * Europa, Westen

Regionen mit einem Sternchen (\*) unterstützen noch keine [Hochverfügbarkeit](concepts-hyperscale-high-availability.md).

## <a name="pricing"></a>Preise
Aktuelle Preisinformationen finden Sie auf der Seite [Azure-Datenbank für MySQL – Preise](https://azure.microsoft.com/pricing/details/postgresql/).
Informationen zu den Kosten der gewünschten Konfiguration können Sie im [Azure-Portal](https://portal.azure.com/#create/Microsoft.PostgreSQLServer) auf der Registerkarte **Konfigurieren** anzeigen. Dort werden die monatlichen Kosten basierend auf den von Ihnen ausgewählten Optionen angegeben. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie den Azure-Preisrechner verwenden, um einen geschätzten Preis zu erhalten. Wählen Sie auf der Website des [Azure-Preisrechners](https://azure.microsoft.com/pricing/calculator/) die Option **Elemente hinzufügen** aus, erweitern Sie die Kategorie **Datenbanken**, und wählen Sie **Azure Database for PostgreSQL – Hyperscale (Citus)** aus, um die Optionen anzupassen.
 
## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie, wie Sie [eine Hyperscale (Citus)-Servergruppe im Portal erstellen](quickstart-create-hyperscale-portal.md).
