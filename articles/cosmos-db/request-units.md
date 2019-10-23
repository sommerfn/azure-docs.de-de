---
title: Anforderungseinheiten und Durchsatz in Azure Cosmos DB
description: Hier erfahren Sie, wie Sie die erforderlichen Anforderungseinheiten in Azure Cosmos DB angeben und einschätzen.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/23/2019
ms.openlocfilehash: 6e5d95a47261445e3031f55368f4e2cd8e2830a7
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2019
ms.locfileid: "72754865"
---
# <a name="request-units-in-azure-cosmos-db"></a>Anforderungseinheiten in Azure Cosmos DB

Bei Azure Cosmos DB zahlen Sie für den bereitgestellten Durchsatz und den verwendeten Speicher auf Stundenbasis. Durchsatz muss bereitgestellt werden, um zu gewährleisten, dass jederzeit genügend Systemressourcen für Ihre Azure Cosmos-Datenbank zur Verfügung stehen. Sie benötigen ausreichend Ressourcen, um die [Azure Cosmos DB-SLAs](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/) zu erfüllen oder zu übertreffen.

Azure Cosmos DB unterstützt viele APIs wie z.B. SQL, MongoDB, Cassandra, Gremlin und Tabelle. Jede API verfügt über einen eigenen Satz von Datenbankvorgängen. Diese Vorgänge reichen von einfachen Lese- und Schreibvorgängen für Datenpunkte bis hin zu komplexen Abfragen. Jeder Datenbankvorgang beansprucht je nach Komplexität eine bestimmte Menge an Systemressourcen. 

Die Kosten sämtlicher Datenbankvorgänge werden von Azure Cosmos DB normalisiert und als *Anforderungseinheiten* (Request Units, kurz RUs) ausgedrückt. RUs pro Sekunde ist also gewissermaßen die Währung für Durchsatz. RUs pro Sekunde ist eine ratenbasierte Währung. Sie abstrahiert die Systemressourcen wie CPU, IOPS und Arbeitsspeicher, die zum Ausführen der von Azure Cosmos DB unterstützten Datenbankvorgänge erforderlich sind. 

Eine Anforderungseinheit (oder eine RU) entspricht den Kosten für das Lesen eines Elements mit einer Größe von 1 KB. Allen anderen Datenbankvorgängen werden analog dazu ebenfalls Kosten in RUs zugewiesen. Unabhängig davon, welche API Sie für die Interaktion mit Ihrem Azure Cosmos-Container verwenden, werden die Kosten immer in RUs gemessen. Unabhängig davon, ob es sich bei dem Datenbankvorgang um eine Schreib-, Lese- oder Abfragevorgang handelt, werden die Kosten immer in RUs gemessen.

Die folgende Abbildung zeigt eine Übersicht über RUs:

![Von Datenbankvorgängen genutzte Anforderungseinheiten](./media/request-units/request-units.png)

Zur Verwaltung und Planung der Kapazität stellt Azure Cosmos DB sicher, dass die Anzahl von RUs für einen bestimmten Datenbankvorgang und ein bestimmtes Dataset deterministisch ist. Sie können den Antwortheader untersuchen, um die von einem Datenbankvorgang genutzte Anzahl von RUs zu ermitteln. Wenn Sie mit den [Faktoren, die sich auf die Gebühren für RUs auswirken](request-units.md#request-unit-considerations), sowie mit den Durchsatzanforderungen Ihrer Anwendung vertraut sind, können Sie Ihre Anwendung kostengünstig ausführen.

Die Anzahl von Anforderungseinheiten für Ihre Anwendung wird auf Sekundenbasis (in Schritten von jeweils 100 RUs pro Sekunde) bereitgestellt. Um den bereitgestellten Durchsatz für Ihre Anwendung zu skalieren, können Sie die Anzahl der RUs jederzeit erhöhen oder verringern. Die Skalierung kann in Inkrementen oder Dekrementen von 100 RUs erfolgen. Sie können Ihre Änderungen entweder programmgesteuert oder über das Azure-Portal vornehmen. Die Abrechnung erfolgt auf Stundenbasis.

Durchsatz kann in zwei Granularitäten bereitgestellt werden: 

* **Container**: Weitere Informationen finden Sie unter [Bereitstellen von Durchsatz für einen Azure Cosmos-Container](how-to-provision-container-throughput.md).
* **Datenbanken**: Weitere Informationen finden Sie unter [Bereitstellen von Durchsatz für eine Azure Cosmos-Datenbank](how-to-provision-database-throughput.md).

## <a name="request-unit-considerations"></a>Aspekte zu Anforderungseinheiten

Während Sie die Anzahl der RUs pro Sekunde für die Bereitstellung schätzen, berücksichtigen Sie die folgenden Faktoren:

* **Elementgröße**: Je größer ein Element, desto mehr RUs werden beim Lesen oder Schreiben des Elements genutzt.

* **Elementindizierung**: Standardmäßig wird jedes Element automatisch indiziert. Wenn Sie einige Elemente in einem Container nicht indizieren, werden weniger RUs genutzt.

* **Anzahl der Elementeigenschaften**: Bei Verwendung der standardmäßigen Indizierung für alle Eigenschaften erhöht sich die Anzahl von RUs, die beim Schreiben eines Elements genutzt werden, wenn sich die Anzahl von Elementeigenschaften erhöht.

* **Indizierte Eigenschaften**: Eine Indexrichtlinie für jeden Container gibt an, welche Eigenschaften standardmäßig indiziert werden. Zum Verringern der für Schreibvorgänge genutzten RUs begrenzen Sie die Anzahl indizierter Eigenschaften.

* **Datenkonsistenz**: Im Vergleich zu anderen, weniger strengen Konsistenzebenen nutzen die Konsistenzebenen „Stark“ und „Begrenzte Veraltung“ bei Lesevorgängen ungefähr zweimal mehr RUs.

* **Abfragemuster**: Die Komplexität einer Abfrage wirkt sich darauf aus, wie viele RUs für einen Vorgang verbraucht werden. Faktoren, die die Kosten von Abfragevorgängen beeinflussen: 
    
    - Die Anzahl der Abfrageergebnisse
    - Die Anzahl der Prädikate
    - Die Art der Prädikate
    - Die Anzahl der benutzerdefinierten Funktionen
    - Die Größe der Quelldaten
    - Die Größe des Resultsets
    - Projektionen

  Azure Cosmos DB garantiert, dass die gleiche Abfrage mit den gleichen Daten bei wiederholter Ausführung immer die gleiche Anzahl von RUs beansprucht.

* **Skriptnutzung**: Bei gespeicherten Prozeduren und Triggern hängt die Nutzung von RUs genau wie bei Abfragen von der Komplexität der ausgeführten Vorgänge ab. Sehen Sie sich beim Entwickeln Ihrer Anwendung den [Request-Charge-Header](optimize-cost-queries.md#evaluate-request-unit-charge-for-a-query) an, um zu ermitteln, wie viele RUs der jeweilige Vorgang erfordert.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über das [Bereitstellen von Durchsatz für Cosmos-Container und -Datenbanken](set-throughput.md).
* Erfahren Sie mehr über [logische Partitionen](partition-data.md).
* Erfahren Sie mehr über [globales Skalieren von bereitgestelltem Durchsatz](scaling-throughput.md).
* Erfahren Sie mehr über das [Bereitstellen von Durchsatz für einen Azure Cosmos-Container](how-to-provision-container-throughput.md).
* Erfahren Sie mehr über das [Bereitstellen des Durchsatzes für eine Datenbank in Azure Cosmos](how-to-provision-database-throughput.md).
* Erfahren Sie mehr über das [Finden der Gebühr für Anforderungseinheiten für einen Vorgang](find-request-unit-charge.md).
* Erfahren Sie mehr über das [Optimieren der Kosten für bereitgestellten Durchsatz in Azure Cosmos DB](optimize-cost-throughput.md).
* Erfahren Sie mehr über das [Optimieren der Lese- und Schreibkosten in Azure Cosmos DB](optimize-cost-reads-writes.md).
* Erfahren Sie mehr über das [Optimieren der Abfragekosten in Azure Cosmos DB](optimize-cost-queries.md).
* Erfahren Sie mehr über das [Verwenden von Metriken zum Überwachen des Durchsatzes](use-metrics.md).
