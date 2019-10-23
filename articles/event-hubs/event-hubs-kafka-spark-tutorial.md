---
title: 'Herstellen einer Verbindung mit Ihrer Apache Spark-App: Azure Event Hubs | Microsoft-Dokumentation'
description: Dieser Artikel enthält Informationen zur Verwendung von Apache Spark mit Azure Event Hubs für Kafka.
services: event-hubs
documentationcenter: .net
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.topic: tutorial
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 5bd25604844ce0521b6bac2d637a2d48a6ef089b
ms.sourcegitcommit: ae461c90cada1231f496bf442ee0c4dcdb6396bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72555737"
---
# <a name="connect-your-apache-spark-application-with-kafka-enabled-azure-event-hubs"></a>Herstellen einer Verbindung zwischen Ihrer Apache Spark-Anwendung und einer Kafka-fähigen Azure Event Hubs-Instanz
In diesem Tutorial wird ausführlich beschrieben, wie Sie zum Zweck des Echtzeitstreamings eine Verbindung zwischen Ihrer Spark-Anwendung und einer Kafka-fähigen Event Hubs-Instanz herstellen. Diese Integration ermöglicht das Streaming, ohne dass die Protokollclients geändert oder eigene Kafka- oder Zookeeper-Cluster ausgeführt werden müssen. Für dieses Tutorial sind mindestens Apache Spark v2.4 und Apache Kafka v2.0 erforderlich.

> [!NOTE]
> Dieses Beispiel ist auf [GitHub](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/spark/) verfügbar.

In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Erstellen eines Event Hubs-Namespace
> * Klonen des Beispielprojekts
> * Ausführen von Spark
> * Lesen aus Event Hubs für Kafka
> * Schreiben in Event Hubs für Kafka

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie vor dem Ausführen dieses Tutorials sicher, dass Sie über Folgendes verfügen:
-   Azure-Abonnement. Falls Sie nicht über ein Abonnement verfügen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/).
-   [Apache Spark v2.4](https://spark.apache.org/downloads.html)
-   [Apache Kafka v2.0]( https://kafka.apache.org/20/documentation.html)
-   [Git-Client](https://www.git-scm.com/downloads)

> [!NOTE]
> Der Spark-Kafka-Adapter wurde aktualisiert, um Kafka v2.0 zu unterstützen (ab Spark v2.4). In früheren Spark-Versionen unterstützte der Adapter Kafka v0.10 und höhere Versionen, war jedoch speziell von Kafka v0.10-APIs abhängig. Da Kafka v0.10 von Event Hubs für Kafka nicht unterstützt wird, werden die Spark-Kafka-Adapter aus Spark-Versionen vor v2.4 von Event Hubs für Kafka-Ökosysteme nicht unterstützt.


## <a name="create-an-event-hubs-namespace"></a>Erstellen eines Event Hubs-Namespace
Ein Event Hubs-Namespace ist erforderlich, um Nachrichten an einen Event Hubs-Dienst zu senden und von diesem zu empfangen. Eine Anleitung zum Abrufen eines Event Hubs-Kafka-Endpunkts finden Sie unter [Creating a Kafka enabled Event Hub](event-hubs-create.md) (Erstellen eines Kafka-fähigen Event Hub). Rufen Sie die Event Hubs-Verbindungszeichenfolge und den vollqualifizierten Domänennamen (Fully Qualified Domain Name, FQDN) zur späteren Verwendung ab. Anweisungen hierzu finden Sie unter [Get an Event Hubs connection string](event-hubs-get-connection-string.md) (Abrufen einer Event Hubs-Verbindungszeichenfolge). 

## <a name="clone-the-example-project"></a>Klonen des Beispielprojekts
Klonen Sie das Azure Event Hubs-Repository, und navigieren Sie zum Unterordner `tutorials/spark`:

```bash
git clone https://github.com/Azure/azure-event-hubs-for-kafka.git
cd azure-event-hubs-for-kafka/tutorials/spark
```

## <a name="read-from-event-hubs-for-kafka"></a>Lesen aus Event Hubs für Kafka
Mit einigen wenigen Konfigurationsänderungen können Sie mit dem Lesen aus Event Hubs für Kafka beginnen. Aktualisieren Sie **BOOTSTRAP_SERVERS** und **EH_SASL** mit Details aus Ihrem Namespace, und Sie können wie mit Kafka mit Event Hubs streamen. Den vollständigen Beispielcode finden Sie auf GitHub in der Datei „sparkConsumer.scala“. 

```scala
//Read from your Event Hub!
val df = spark.readStream
    .format("kafka")
    .option("subscribe", TOPIC)
    .option("kafka.bootstrap.servers", BOOTSTRAP_SERVERS)
    .option("kafka.sasl.mechanism", "PLAIN")
    .option("kafka.security.protocol", "SASL_SSL")
    .option("kafka.sasl.jaas.config", EH_SASL)
    .option("kafka.request.timeout.ms", "60000")
    .option("kafka.session.timeout.ms", "30000")
    .option("kafka.group.id", GROUP_ID)
    .option("failOnDataLoss", "false")
    .load()

//Use dataframe like normal (in this example, write to console)
val df_write = df.writeStream
    .outputMode("append")
    .format("console")
    .start()
```

## <a name="write-to-event-hubs-for-kafka"></a>Schreiben in Event Hubs für Kafka
Sie können auch in Event Hubs schreiben. Die Vorgehensweise ist dieselbe wie bei Kafka. Vergessen Sie nicht, Ihre Konfiguration anzupassen und **BOOTSTRAP_SERVERS** und **EH_SASL** mit den Informationen aus Ihrem Event Hubs-Namespace zu aktualisieren.  Den vollständigen Beispielcode finden Sie auf GitHub in der Datei „sparkProducer.scala“. 

```scala
df = /**Dataframe**/

//Write to your Event Hub!
df.writeStream
    .format("kafka")
    .option("topic", TOPIC)
    .option("kafka.bootstrap.servers", BOOTSTRAP_SERVERS)
    .option("kafka.sasl.mechanism", "PLAIN")
    .option("kafka.security.protocol", "SASL_SSL")
    .option("kafka.sasl.jaas.config", EH_SASL)
    .option("checkpointLocation", "./checkpoint")
    .start()
```



## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie mit dem Spark-Kafka-Connector und Event Hubs für Kafka streamen. Sie haben die folgenden Schritte ausgeführt: 

> [!div class="checklist"]
> * Erstellen eines Event Hubs-Namespace
> * Klonen des Beispielprojekts
> * Ausführen von Spark
> * Lesen aus Event Hubs für Kafka
> * Schreiben in Event Hubs für Kafka

Weitere Informationen zu Event Hubs und Event Hubs für Kafka finden Sie unter folgendem Thema:  

- [Informationen zu Event Hubs](event-hubs-what-is-event-hubs.md)
- [Event Hubs für Apache Kafka](event-hubs-for-kafka-ecosystem-overview.md)
- [Erstellen von Kafka-fähigen Event Hubs](event-hubs-create-kafka-enabled.md)
- [Streamen von Ereignissen in Event Hubs aus Kafka-Anwendungen](event-hubs-quickstart-kafka-enabled-event-hubs.md)
- [Spiegeln eines Kafka-Brokers in einem Kafka-fähigen Event Hub](event-hubs-kafka-mirror-maker-tutorial.md)
- [Verwenden von Apache Flink mit Azure Event Hubs für Apache Kafka](event-hubs-kafka-flink-tutorial.md)
- [Integrieren von Kafka Connect in einen Kafka-fähigen Event Hub](event-hubs-kafka-connect-tutorial.md)
- [Verbinden von Akka Streams mit einem Kafka-fähigen Event Hub](event-hubs-kafka-akka-streams-tutorial.md)
- [Erkunden von Beispielen auf GitHub](https://github.com/Azure/azure-event-hubs-for-kafka)
