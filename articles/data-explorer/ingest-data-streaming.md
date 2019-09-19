---
title: Erfassen von Daten in Azure Data Explorer mithilfe der Streamingerfassung
description: Hier erfahren Sie, wie Sie Daten mithilfe der Streamingerfassung in Azure Data Explorer erfassen (laden).
author: orspod
ms.author: orspodek
ms.reviewer: tzgitlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 08/30/2019
ms.openlocfilehash: 5aa2d694c2c74b493a7fd1a2a89d39866928d1d4
ms.sourcegitcommit: adc1072b3858b84b2d6e4b639ee803b1dda5336a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2019
ms.locfileid: "70843862"
---
# <a name="streaming-ingestion-preview"></a>Streamingerfassung (Vorschauversion)

Die Streamingerfassung ist für Szenarien vorgesehen, in denen eine geringe Wartezeit mit einer Erfassungsdauer von weniger als zehn Sekunden für unterschiedliche Volumedaten benötigt wird. Sie dient zur Optimierung der operativen Verarbeitung zahlreicher Tabellen in einer oder mehreren Datenbanken, bei denen der Datenstrom für die einzelnen Tabellen jeweils relativ klein (wenige Datensätze pro Sekunde), das Gesamtvolumen der Datenerfassung jedoch hoch ist (mehrere tausend Datensätze pro Sekunde).

Wenn die Datenmenge mehr als 1 MB pro Sekunde und Tabelle beträgt, verwenden Sie anstelle der Streamingerfassung die klassische (Massen-)Erfassung. Weitere Informationen zu den verschiedenen Erfassungsmethoden finden Sie in der [Übersicht über die Datenerfassung](/azure/data-explorer/ingest-data-overview).

> [!NOTE]
> Folgende Features werden von der Streamingerfassung nicht unterstützt:
> * [Datenbankcursor](/azure/kusto/management/databasecursor)
> * [Datenzuordnung](/azure/kusto/management/mappings). Es wird nur die [vorab erstellte](/azure/kusto/management/tables#create-ingestion-mapping) Datenzuordnung unterstützt. 

## <a name="prerequisites"></a>Voraussetzungen

* Wenn Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
* Melden Sie sich bei der [Webbenutzeroberfläche](https://dataexplorer.azure.com/) an.
* Erstellen Sie [einen Azure Data Explorer-Cluster und eine Datenbank](create-cluster-database-portal.md).

## <a name="enable-streaming-ingestion-on-your-cluster"></a>Aktivieren der Streamingerfassung in Ihrem Cluster

1. Navigieren Sie im Azure-Portal zum Azure Data Explorer-Cluster. Wählen Sie unter **Einstellungen** die Option **Konfigurationen** aus. 
1. Wählen Sie im Bereich **Konfigurationen** die Option **Ein** aus, um die **Streamingerfassung** zu aktivieren.
1. Wählen Sie **Speichern** aus.
 
    ![Streamingerfassung aktiviert](media/ingest-data-streaming/streaming-ingestion-on.png)
 
1. Definieren Sie auf der [Webbenutzeroberfläche](https://dataexplorer.azure.com/) die [Streamingerfassungsrichtlinie](/azure/kusto/concepts/streamingingestionpolicy) für Tabellen oder Datenbanken, die Streamingdaten empfangen. 

    > [!TIP]
    > Wird die Richtlinie auf der Datenbankebene definiert, kann die Streamingerfassung für alle Tabellen in der Datenbank verwendet werden.

## <a name="use-streaming-ingestion-to-ingest-data-to-your-cluster"></a>Erfassen von Daten in Ihrem Cluster mithilfe der Streamingerfassung

Es werden zwei Streamingerfassungstypen unterstützt:

* [Event Hub](/azure/data-explorer/ingest-data-event-hub) als Datenquelle
* Für die benutzerdefinierte Erfassung muss eine Anwendung geschrieben werden, die eine der Azure Data Explorer-Clientbibliotheken verwendet. Eine Beispielanwendung finden Sie [hier](https://github.com/Azure/azure-kusto-samples-dotnet/tree/master/client/StreamingIngestionSample).

### <a name="choose-the-appropriate-streaming-ingestion-type"></a>Auswählen des geeigneten Streamingerfassungstyps

|   |Event Hub  |Benutzerdefinierte Erfassung  |
|---------|---------|---------|
|Datenverzögerung zwischen der Initiierung der Erfassung und der Verfügbarkeit der Daten für Abfragen   |    Längere Verzögerung     |   Kürzere Verzögerung      |
|Zusätzlicher Entwicklungsaufwand    |   Schnelle und einfache Einrichtung, kein zusätzlicher Entwicklungsaufwand    |   Hoher zusätzlicher Entwicklungsaufwand für die Anwendung, um Fehler zu behandeln und die Datenkonsistenz sicherzustellen     |

## <a name="disable-streaming-ingestion-on-your-cluster"></a>Deaktivieren der Streamingerfassung in Ihrem Cluster

> [!WARNING]
> Die Deaktivierung der Streamingerfassung kann mehrere Stunden dauern.

1. Entfernen Sie die [Streamingerfassungsrichtlinie](/azure/kusto/concepts/streamingingestionpolicy) aus allen relevanten Tabellen und Datenbanken. Wenn Sie die Streamingerfassungsrichtlinie entfernen, werden die Streamingerfassungsdaten aus dem anfänglichen Speicher in den permanenten Speicher im Spaltenspeicher (Erweiterungen oder Shards) verschoben. Die Datenverschiebung kann zwischen einigen Sekunden und mehreren Stunden dauern. Dies ist abhängig von der Datenmenge im anfänglichen Speicher sowie von der CPU- und Arbeitsspeicherauslastung des Clusters.
1. Navigieren Sie im Azure-Portal zum Azure Data Explorer-Cluster. Wählen Sie unter **Einstellungen** die Option **Konfigurationen** aus. 
1. Wählen Sie im Bereich **Konfigurationen** die Option **Off** aus, um die **Streamingerfassung** zu aktivieren.
1. Wählen Sie **Speichern** aus.

    ![Streamingerfassung deaktiviert](media/ingest-data-streaming/streaming-ingestion-off.png)

## <a name="limitations"></a>Einschränkungen

* Leistung und Kapazität der Streamingerfassung werden für größere virtuelle Computer und Cluster skaliert. Für einen einzelnen D14-Knoten wird eine Last von bis zu 150 Anforderungen pro Sekunde empfohlen.
* Aktuell werden nur SKUs mit acht und 16 Kernen (D13, D14, L8 und L16) unterstützt.
* Die Datengröße pro Erfassungsanforderung ist auf 4 MB beschränkt.
* Schemaaktualisierungen wie etwa die Erstellung und Änderung von Tabellen und Erfassungszuordnungen können für den Streamingerfassungsdienst bis zu fünf Minuten dauern.
* Wenn die Streamingerfassung in einem Cluster aktiviert wird, wird ein Teil des lokalen SSD-Datenträgers der Clustercomputer für Streamingerfassungsdaten genutzt, wodurch sich der verfügbare Speicherplatz für den aktiven Cache verringert. Dies gilt auch, wenn gar keine Daten per Streaming erfasst werden.

## <a name="next-steps"></a>Nächste Schritte

* [Abfragen von Daten in Azure Data Explorer](web-query-data.md)
