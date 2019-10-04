---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 06/20/2019
ms.author: tamram
ms.openlocfilehash: 5ab03b682dd0ed1dc7b198e89c86e7a74c6275cd
ms.sourcegitcommit: cd70273f0845cd39b435bd5978ca0df4ac4d7b2c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2019
ms.locfileid: "67457351"
---
| Resource | Ziel |
|----------|---------------|
| Maximale Größe einer einzelnen Tabelle | 500 TiB |
| Maximale Größe einer Tabellenentität | 1 MiB |
| Maximale Anzahl von Eigenschaften in einer Tabellenentität | 255, inklusive drei Systemeigenschaften: PartitionKey, RowKey und Timestamp |
| Maximale Gesamtgröße der Eigenschaftswerte in einer Entität | 1 MiB |
| Maximale Gesamtgröße einer individuellen Eigenschaft in einer Entität | Variiert je nach Eigenschaftstyp. Weitere Informationen finden Sie unter **Eigenschaftstypen** in [Grundlegendes zum Tabellendienst-Datenmodell](/rest/api/storageservices/understanding-the-table-service-data-model). |
| Maximale Anzahl gespeicherter Zugriffsrichtlinien pro Tabelle | 5 |
| Maximale Anforderungsrate pro Speicherkonto | 20.000 Transaktionen pro Sekunde, ausgehend von einer Entitätsgröße von 1KiB |
| Zieldurchsatz für eine einzelne Tabellenpartition (Entitäten von 1KiB) | Bis zu 2.000 Entitäten pro Sekunde |