---
title: Vorteile der Multimasterunterstützung von Azure Cosmos DB
description: Informationen zum Verständnis der Vorteile der Multimasterunterstützung in Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/08/2019
ms.author: mjbrown
ms.openlocfilehash: c78e5e4f8d396d777738bddfd6baf086c0b2ecf4
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2019
ms.locfileid: "67788581"
---
# <a name="understand-multi-master-benefits-in-azure-cosmos-db"></a>Vorteile der Multimasterunterstützung in Azure Cosmos DB

Die Betreiber von Cosmos DB-Konten sollten eine geeignete globale Verteilungskonfiguration auswählen, um die Latenz, Verfügbarkeit und RTO-Anforderungen für ihre Anwendungen sicherzustellen. Azure Cosmos-Konten, die mit mehreren Schreibstandorten konfiguriert sind, bieten erhebliche Vorteile gegenüber Konten mit einem einzigen Schreibstandort, z.B. eine Schreibverfügbarkeits-SLA von 99,999 %, eine Schreiblatenz-SLA mit weniger als 10 ms im 99. Quantil und RTO = 0 bei einer regionalen Katastrophe.

## <a name="comparison-of-features"></a>Vergleich der Features

|Anwendungsanforderung|Mehrere Schreibstandorte|Einzelner Schreibstandort|Hinweis|
|---|---|---|---|
|Schreiblatenz-SLA < 10ms in Q99|**Ja**|Nein|Konten mit einem einzelnen Schreibstandort verursachen zusätzliche regionsübergreifende Netzwerklatenz für jeden Schreibvorgang.|
|Leselatenz-SLA < 10ms in Q99|**Ja**|Ja| |
|Schreib-SLA mit 99,999 %|**Ja**|Nein|Konten mit einem einzelnen Schreibstandort garantieren eine SLA von 99,99 %|
|RTO = 0|**Ja**|Nein|Keine Ausfallzeiten für Schreibvorgänge bei regionalen Katastrophen. Konten mit einem einzelnen Schreibstandort weisen einen RTO-Wert von 15 Minuten auf.|

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie EnableMultipleWriteLocations in Ihrem Azure Cosmos-Konto trotzdem deaktivieren möchten, [öffnen Sie ein Supportticket](https://azure.microsoft.com/support/create-ticket/).
