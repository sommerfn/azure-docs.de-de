---
title: Bereitstellen des Durchsatzes für Azure Cosmos-Container und -Datenbanken
description: Hier erfahren Sie, wie Sie bereitgestellten Durchsatz für Ihre Azure Cosmos-Container und -Datenbanken festlegen.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/12/2019
ms.openlocfilehash: 4c25e8b93fe9bcce17189bd7b787eaf4c3885716
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2019
ms.locfileid: "72752485"
---
# <a name="provision-throughput-on-containers-and-databases"></a>Bereitstellen des Durchsatzes für Container und Datenbanken

Eine Azure Cosmos-Datenbank ist eine Verwaltungseinheit für eine Gruppe von Containern. Eine Datenbank besteht aus einem Satz von schemaunabhängigen Containern. Ein Azure Cosmos-Container ist die Skalierungseinheit für Durchsatz und Speicher. Ein Container wird horizontal über eine Gruppe von Computern innerhalb einer Azure-Region partitioniert und auf alle Azure-Regionen, die Ihrem Azure Cosmos-Konto zugeordnet sind, verteilt.

Mit Azure Cosmos DB können Sie Durchsatz in zwei Größen bereitstellen:
 
- Azure Cosmos-Container
- Azure Cosmos-Datenbanken

## <a name="set-throughput-on-a-container"></a>Festlegen des Durchsatzes für einen Container  

Der für einen Azure Cosmos-Container bereitgestellte Durchsatz ist ausschließlich für diesen Container reserviert. Der Container erhält den bereitgestellten Durchsatz durchgängig. Der bereitgestellte Durchsatz für einen Container wird finanziell durch SLAs gesichert. Informationen zum Konfigurieren des Durchsatzes für einen Container finden Sie unter [Bereitstellen von Durchsatz für einen Azure Cosmos-Container](how-to-provision-container-throughput.md).

Das Festlegen von bereitgestelltem Durchsatz für einen Container ist die am häufigsten genutzte Option. Sie können den Durchsatz für einen Container elastisch skalieren, indem Sie mithilfe von [Anforderungseinheiten (Request Units, RUs)](request-units.md) eine beliebige Menge an Durchsatz bereitstellen. 

Der für einen Azure Cosmos-Container bereitgestellte Durchsatz wird gleichmäßig auf alle logischen Partitionen des Containers aufgeteilt. Sie können den Durchsatz für logische Partitionen nicht selektiv angeben. Da mindestens eine logische Partition eines Containers auf einer physischen Partition gehostet wird, gehören die physischen Partitionen ausschließlich zu dem Container und unterstützen den Durchsatz für den Container. 

Wenn die Workload in einer logischen Partition mehr als den Durchsatz verbraucht, der dieser logischen Partition zugewiesen wurde, werden Ihre Vorgänge begrenzt. Bei einer Ratenbegrenzung können Sie entweder den bereitgestellten Durchsatz für den gesamten Container erhöhen oder die Vorgänge wiederholen. Weitere Informationen zur Partitionierung finden Sie unter [Logische Partitionen](partition-data.md).

Es wird empfohlen, den Durchsatz auf Containerebene zu konfigurieren, wenn Sie eine garantierte Leistung für den Container benötigen.

Die folgende Abbildung zeigt, wie eine physische Partition eine bzw. mehrere logische Partitionen eines Containers hostet:

![Physische Partition](./media/set-throughput/resource-partition.png)

## <a name="set-throughput-on-a-database"></a>Festlegen des Durchsatzes für eine Datenbank

Wenn Sie Durchsatz für eine Cosmos-Datenbank bereitstellen, wird der Durchsatz von allen Containern in der Datenbank gemeinsam genutzt (auch als gemeinsam genutzte Datenbankcontainer bezeichnet). Ausnahme: Sie haben einen bereitgestellten Durchsatz für bestimmte Container in der Datenbank angegeben. Die gemeinsame Nutzung des auf Datenbankebene bereitgestellten Durchsatzes durch die Container entspricht dem Hosten einer Datenbank in einem Computercluster. Da alle Container in einer Datenbank die auf einem Computer verfügbaren Ressourcen gemeinsam nutzen, erhalten Sie natürlich keine vorhersagbare Leistung für einen bestimmten Container. Informationen zum Konfigurieren des bereitgestellten Durchsatzes für eine Datenbank finden Sie unter [Bereitstellen von Durchsatz für eine Azure Cosmos-Datenbank](how-to-provision-database-throughput.md).

Mit dem Festlegen des Durchsatzes für eine Azure Cosmos-Datenbank wird sichergestellt, dass Sie zu jeder Zeit den bereitgestellten Durchsatz für diese Datenbank erhalten. Da alle Container in der Datenbank den bereitgestellten Durchsatz gemeinsam nutzen, garantiert Azure Cosmos DB keinen vorhersagbaren Durchsatz für einen bestimmten Container in der Datenbank. Der Anteil des Durchsatzes, den ein bestimmter Container erhält, hängt von den folgenden Faktoren ab:

* Anzahl von Containern
* Auswahl der Partitionsschlüssel für verschiedene Container
* Verteilung der Workload auf die verschiedenen logischen Partitionen der Container 

Es wird empfohlen, den Durchsatz für eine Datenbank zu konfigurieren, wenn der Durchsatz von mehreren Containern gemeinsam genutzt werden soll, Sie aber keinen spezifischen Durchsatz für bestimmte Container bereitstellen möchten. 

In den folgenden Beispielen wird erläutert, wo das Bereitstellen von Durchsatz auf Datenbankebene empfehlenswert ist:

* Die gemeinsame Nutzung des für eine Datenbank bereitgestellten Durchsatzes durch eine Gruppe von Containern ist bei mehrinstanzenfähigen Anwendungen nützlich. Jeder Benutzer kann durch einen eigenen Azure Cosmos-Container dargestellt werden.

* Die gemeinsame Nutzung des bereitgestellten Durchsatzes einer Datenbank durch eine Gruppe von Containern ist nützlich, wenn Sie eine NoSQL-Datenbank (z. B. MongoDB oder Cassandra), die in einem Cluster von virtuellen Computern oder auf lokalen physischen Servern gehostet wird, zu Azure Cosmos DB migrieren. Stellen Sie sich den bereitgestellten Durchsatz, der für Ihre Azure Cosmos-Datenbank konfiguriert wurde, als logische Entsprechung (jedoch kostengünstiger und flexibler) zur Computekapazität des MongoDB- oder Cassandra-Clusters vor.  

Alle in einer Datenbank erstellten Container mit bereitgestelltem Durchsatz müssen mit einem [Partitionsschlüssel](partition-data.md) erstellt werden. Der einem Container innerhalb einer Datenbank zugeteilte Durchsatz wird jederzeit auf alle logischen Partitionen des Containers verteilt. Wenn Ihre Container den für eine Datenbank konfigurierten bereitgestellten Durchsatz gemeinsam nutzen, können Sie den Durchsatz nicht selektiv einem bestimmten Container oder einer logischen Partition zuordnen. 

Wenn die Workload in einer logischen Partition mehr als den Durchsatz verbraucht, der der jeweiligen logischen Partition zugewiesen wurde, werden Ihre Vorgänge begrenzt. Bei einer Ratenbegrenzung können Sie entweder den Durchsatz für die gesamte Datenbank erhöhen oder die Vorgänge wiederholen. Weitere Informationen zur Partitionierung finden Sie unter [Logische Partitionen](partition-data.md).

Der für eine Datenbank bereitgestellte Durchsatz kann von den Containern innerhalb der Datenbank gemeinsam genutzt werden. Jeder neue Container in auf Datenbankebene gemeinsam genutztem Durchsatz benötigt 100 RU/s. Beim Bereitstellen von Containern mit gemeinsam genutzter Datenbank gilt Folgendes:

* Es werden jeweils 25 Container zu einer Partitionsgruppe zusammengefasst, und der Datenbankdurchsatz (D) wird von den Containern in der Partitionsgruppe gemeinsam genutzt. Wenn in der Datenbank bis zu 25 Container vorhanden sind und Sie nur einen einzelnen Container verwenden, steht diesem Container der maximale Durchsatz (D) zur Verfügung.

* Für jeden neuen Container, der erstellt wird, wenn bereits 25 Container vorhanden sind, wird eine neue Partitionsgruppe erstellt, und der Datenbankdurchsatz wird auf die neu erstellten Partitionsgruppen aufgeteilt: D/2 bei zwei Partitionsgruppen, D/3 bei drei Partitionsgruppen usw. Falls Sie lediglich einen einzelnen Container aus der Datenbank verwenden, steht für diesen der jeweilige maximale Durchsatz (D/2, D/3, D/4 usw.) zur Verfügung. Aufgrund des geringeren Durchsatzes empfiehlt es sich, in einer Datenbank maximal 25 Container zu erstellen.

**Beispiel**

* Angenommen, Sie erstellen eine Datenbank namens „MyDB“ mit einem bereitgestellten Durchsatz von 10.000 RU/s.

* Wenn Sie unter „MyDB“ 25 Container bereitstellen, werden alle Container in einer Partitionsgruppe zusammengefasst. Falls Sie lediglich einen einzelnen Container aus der Datenbank verwenden, steht für diesen der maximale Durchsatz von 10.000 RU/s (D) zur Verfügung.

* Wenn Sie einen 26. Container bereitstellen, wird eine neue Partitionsgruppe erstellt, und der Durchsatz wird gleichmäßig auf die beiden Partitionsgruppen verteilt. Das bedeutet: Wenn Sie nun lediglich einen einzelnen Container aus der Datenbank verwenden, stehen für diesen maximal 5.000 RU/s (D/2) zur Verfügung. Da zwei Partitionsgruppen vorhanden sind, beträgt der Faktor für die gemeinsame Durchsatznutzung D/2.

   In der folgenden Abbildung wird das obige Beispiel grafisch dargestellt:

   ![Faktor für die gemeinsame Durchsatznutzung auf Datenbankebene](./media/set-throughput/database-level-throughput-shareability-factor.png)


Wenn Ihre Workloads das Löschen und Wiederherstellen aller Sammlungen in einer Datenbank beinhalten, wird empfohlen, die leere Datenbank zu löschen und vor der Erstellung der Sammlung eine neue Datenbank anzulegen. Die folgende Abbildung zeigt, wie eine physische Partition eine bzw. mehrere logische Partitionen hosten kann, die zu unterschiedlichen Containern innerhalb einer Datenbank gehören:

![Physische Partition](./media/set-throughput/resource-partition2.png)

## <a name="set-throughput-on-a-database-and-a-container"></a>Festlegen des Durchsatzes für eine Datenbank und einen Container

Sie können die beiden Modelle kombinieren. Es ist erlaubt, Durchsatz sowohl auf Datenbank- als auch auf Containerebene bereitzustellen. Das folgende Beispiel zeigt die Vorgehensweise bei der Bereitstellung von Durchsatz für eine Azure Cosmos-Datenbank und einen Container:

* Sie können eine Azure Cosmos-Datenbank mit dem Namen *Z* erstellen und für diese als Durchsatz *K* RUs bereitstellen. 
* Als Nächstes erstellen Sie die fünf Container *A*, *B*, *C*, *D* und *E* innerhalb der Datenbank. Achten Sie beim Erstellen von Container B darauf, die **Option zum Bereitstellen von dediziertem Durchsatz für diesen Container**zu aktivieren, und konfigurieren Sie explizit *P* RUs des bereitgestellten Durchsatzes für diesen Container. Beachten Sie, dass Sie freigegebenen und dedizierten Durchsatz nur beim Erstellen der Datenbank und des Containers konfigurieren können. 

   ![Festlegen des Durchsatzes auf Containerebene](./media/set-throughput/coll-level-throughput.png)

* Der Durchsatz von *K* RUs ist für die vier Container *A*, *C*, *D*, und *E* freigegeben. Die genaue für *A*, *C*, *D* oder *E* verfügbare Durchsatzmenge variiert. Es gibt keine SLAs für die einzelnen Containerdurchsätze.
* Für den Container *B* wird jederzeit ein Durchsatz von *P* RUs sichergestellt. Er wird durch SLAs abgedeckt.

> [!NOTE]
> Ein Container mit bereitgestelltem Durchsatz kann nicht in einen gemeinsam genutzten Datendankcontainer konvertiert werden. Umgekehrt kann ein gemeinsam genutzter Datenbankcontainer nicht in einen dedizierten Durchsatz umgewandelt werden.

## <a name="update-throughput-on-a-database-or-a-container"></a>Aktualisieren des Durchsatzes für eine Datenbank oder einen Container

Nachdem Sie einen Azure Cosmos-Container oder eine Datenbank erstellt haben, können Sie den bereitgestellten Durchsatz aktualisieren. Es gibt keine Beschränkung für den bereitgestellten Maximaldurchsatz, den Sie in der Datenbank oder dem Container konfigurieren können. Der bereitgestellte Mindestdurchsatz hängt von den folgenden Faktoren ab: 

* Maximale Datengröße, die jemals im Container gespeichert wird
* Maximaler Durchsatz, der jemals im Container bereitgestellt wird
* Maximale Anzahl von Azure Cosmos-Containern, die jemals in einer Datenbank mit gemeinsam genutztem Durchsatz erstellt werden 

Sie können den Mindestdurchsatz eines Containers oder einer Datenbank programmgesteuert mithilfe der SDKs abrufen oder den Wert im Azure-Portal anzeigen. Bei Verwendung des .NET SDK ermöglicht Ihnen die [DocumentClient.ReplaceOfferAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.replaceofferasync?view=azure-dotnet)-Methode das Skalieren des Werts für den bereitgestellten Durchsatz. Bei Verwendung des Java SDK ermöglicht Ihnen die [RequestOptions.setOfferThroughput](sql-api-java-samples.md#offer-examples)-Methode das Skalieren des Werts für den bereitgestellten Durchsatz. 

Bei Verwendung des .NET SDK ermöglicht Ihnen die [DocumentClient.ReadOfferAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.readofferasync?view=azure-dotnet)-Methode das Abrufen des Mindestdurchsatzes eines Containers oder einer Datenbank. 

Sie können den bereitgestellten Durchsatz eines Containers oder einer Datenbank jederzeit skalieren. Wenn ein Skalierungsvorgang ausgeführt wird, um den Durchsatz zu erhöhen, kann sich der Zeitaufwand aufgrund der Systemtasks für die Bereitstellung der erforderlichen Ressourcen erhöhen. Sie können den Status des Skalierungsvorgangs im Azure-Portal oder programmgesteuert mit den SDKs überprüfen. Bei Verwendung des .NET SDK können Sie den Status des Skalierungsvorgangs mithilfe der Methode `DocumentClient.ReadOfferAsync` abrufen.

## <a name="comparison-of-models"></a>Vergleich der Modelle

|**Parameter**  |**Bereitgestellter Durchsatz für eine Datenbank**  |**Bereitgestellter Durchsatz für einen Container**|
|---------|---------|---------|
|Minimale RUs |400 (Nach den ersten vier Containern benötigt jeder weitere Container mindestens 100 RUs pro Sekunde.) |400|
|Minimale RUs pro Container|100|400|
|Maximale RUs|Unbegrenzt, in der Datenbank|Unbegrenzt, im Container|
|RUs zugewiesen oder verfügbar für einen bestimmten Container|Keine Garantien. Die einem bestimmten Container zugewiesenen RUs hängen von den Eigenschaften ab. Bei diesen Eigenschaften kann es sich um die Partitionsschlüssel der Container, die den Durchsatz gemeinsam nutzen, um die Verteilung der Workload oder um die Anzahl von Containern handeln. |Alle RUs, die für den Container konfiguriert wurden, sind ausschließlich für diesen Container reserviert.|
|Maximale Speicherkapazität für einen Container|Unbegrenzt.|Unbegrenzt.|
|Maximaler Durchsatz pro logischer Partition eines Containers|10.000 RUs|10.000 RUs|
|Maximale Speicherkapazität (Daten und Index) pro logischer Partition eines Containers|10 GB|10 GB|

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über [logische Partitionen](partition-data.md).
* Erfahren Sie mehr über das [Bereitstellen von Durchsatz für einen Azure Cosmos-Container](how-to-provision-container-throughput.md).
* Erfahren Sie mehr über das [Bereitstellen des Durchsatzes für eine Datenbank in Azure Cosmos](how-to-provision-database-throughput.md).

