---
title: Datenstreaming mit Azure Event Hubs mithilfe des Kafka-Protokolls | Microsoft-Dokumentation
description: Dieser Artikel enthält Informationen zum Streamen in Azure Event Hubs mit dem Kafka-Protokoll und APIs.
services: event-hubs
author: basilhariri
ms.author: bahariri
ms.service: event-hubs
ms.topic: quickstart
ms.custom: seodec18
ms.date: 05/06/2019
ms.openlocfilehash: a4e050fdef20cdc62ee92e6383c455ffcb9abc90
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65203909"
---
# <a name="data-streaming-with-event-hubs-using-the-kafka-protocol"></a>Datenstreaming mit Event Hubs mithilfe des Kafka-Protokolls
In dieser Schnellstartanleitung erfahren Sie, wie Daten an Kafka-fähige Event Hubs gestreamt werden können, ohne Protokollclients zu ändern oder eigene Cluster zu betreiben. Sie lernen, wie Sie über Ihre Producer und Consumer mit Kafka-fähigen Event Hubs kommunizieren – mit nur einer Konfigurationsänderung in Ihren Anwendungen. Azure Event Hubs unterstützt [Apache Kafka Version 1.0](https://kafka.apache.org/10/documentation.html).

> [!NOTE]
> Dieses Beispiel ist auf [GitHub](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/quickstart/java) verfügbar.

## <a name="prerequisites"></a>Voraussetzungen

Für diese Schnellstartanleitung benötigen Sie Folgendes:

* Lesen Sie den Artikel [Event Hubs für Apache Kafka](event-hubs-for-kafka-ecosystem-overview.md).
* Ein Azure-Abonnement. Wenn Sie keins besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) erstellen, bevor Sie beginnen.
* [Java Development Kit (JDK) 1.7 oder höher](https://aka.ms/azure-jdks)
* Ein binäres Maven-Archiv ([Download](https://maven.apache.org/download.cgi)/[Installationsanleitung](https://maven.apache.org/install.html))
* [Git-Client](https://www.git-scm.com/)
* [Ein Kafka-fähiger Event Hubs-Namespace](event-hubs-create.md)

## <a name="create-a-kafka-enabled-event-hubs-namespace"></a>Erstellen eines Kafka-fähigen Event Hubs-Namespace

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und klicken Sie links oben auf **Ressource erstellen**.

2. Suchen Sie nach Event Hubs, und wählen Sie die hier gezeigten Optionen aus:
    
    ![Suchen nach Event Hubs im Portal](./media/event-hubs-create-kafka-enabled/event-hubs-create-event-hubs.png)
 
3. Stellen Sie einen eindeutigen Namen bereit, und aktivieren Sie Kafka für den Namespace. Klicken Sie auf **Create**. Hinweis: Event Hubs für Kafka wird nur von Event Hubs in den Tarifen „Standard“ und „Dedicated“ unterstützt. Event Hubs im Tarif „Basic“ geben einen Themaauthentifizierungsfehler für alle Kafka-Vorgänge zurück.
    
    ![Erstellen eines Namespace](./media/event-hubs-create-kafka-enabled/create-kafka-namespace.jpg)
 
4. Nachdem der Namespace erstellt wurde, klicken Sie auf der Registerkarte **Einstellungen** auf **SAS-Richtlinien**, um die Verbindungszeichenfolge abzurufen.

    ![Klicken auf „SAS-Richtlinien“](./media/event-hubs-create/create-event-hub7.png)

5. Sie können die Standardrichtlinie **RootManageSharedAccessKey** auswählen oder eine neue Richtlinie hinzufügen. Klicken Sie auf den Richtliniennamen, und kopieren Sie die Verbindungszeichenfolge. 
    
    ![Auswählen einer Richtlinie](./media/event-hubs-create/create-event-hub8.png)
 
6. Fügen Sie diese Verbindungszeichenfolge zur Konfiguration Ihrer Kafka-Anwendung hinzu.

Nun können Sie Ereignisse von Ihren Anwendungen streamen, die das Kafka-Protokoll in Event Hubs verwenden.

## <a name="send-and-receive-messages-with-kafka-in-event-hubs"></a>Senden und Empfangen von Nachrichten mit Kafka in Event Hubs

1. Klonen Sie das [Azure Event Hubs-Repository für Kafka](https://github.com/Azure/azure-event-hubs-for-kafka).

2. Navigieren Sie zu `azure-event-hubs-for-kafka/quickstart/java/producer`.

3. Aktualisieren Sie die Konfigurationsdetails für die Producer in `src/main/resources/producer.config` wie folgt:

    ```xml
    bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
    security.protocol=SASL_SSL
    sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
    ```
    
4. Führen Sie den Producer-Code aus, und streamen Sie Daten an Kafka-fähige Event Hubs:
   
    ```shell
    mvn clean package
    mvn exec:java -Dexec.mainClass="TestProducer"                                    
    ```
    
5. Navigieren Sie zu `azure-event-hubs-for-kafka/quickstart/java/consumer`.

6. Aktualisieren Sie die Konfigurationsdetails für die Consumer in `src/main/resources/consumer.config` wie folgt:
   
    ```xml
    bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
    security.protocol=SASL_SSL
    sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
    ```

7. Führen Sie den Consumer-Code und den Prozess über Kafka-fähige Event Hubs unter Verwendung Ihrer Kafka-Clients aus:

    ```java
    mvn clean package
    mvn exec:java -Dexec.mainClass="TestConsumer"                                    
    ```

Wenn Ihr Kafka-Cluster in Event Hubs über Ereignisse verfügt, werden diese nun allmählich vom Consumer empfangen.

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie erfahren, wie Daten an Kafka-fähige Event Hubs gestreamt werden können, ohne dass Sie Protokollclients ändern oder eigene Cluster betreiben müssen. Fahren Sie mit dem folgenden Tutorial fort, um weitere Informationen zu erhalten:

* [Informationen zu Event Hubs](event-hubs-what-is-event-hubs.md)
* [Informationen zu Event Hubs für Kafka](event-hubs-for-kafka-ecosystem-overview.md)
* [Weitere Beispiele zu Event Hubs für Kafka auf GitHub](https://github.com/Azure/azure-event-hubs-for-kafka)
* Verwenden Sie [MirrorMaker](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) zum [Streamen von Ereignissen aus der lokalen Kafka-Instanz an Kafka-fähige Event Hubs in der Cloud](event-hubs-kafka-mirror-maker-tutorial.md).
* Erfahren Sie, wie Sie [Apache Flink](event-hubs-kafka-flink-tutorial.md) oder [Akka Streams](event-hubs-kafka-akka-streams-tutorial.md) zum Streamen an Kafka-fähige Event Hubs verwenden.
