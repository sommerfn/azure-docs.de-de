---
title: Datenspeicherung und -eingang in Azure Time Series Insights Preview | Microsoft-Dokumentation
description: Grundlagen zur Datenspeicherung und zum Dateneingang in Azure Time Series Insights Preview.
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 11/04/2019
ms.custom: seodec18
ms.openlocfilehash: d0cdd78aaa2b58743e16a2e7cfe213a9daed85ff
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73605884"
---
# <a name="data-storage-and-ingress-in-azure-time-series-insights-preview"></a>Datenspeicherung und -eingang in Azure Time Series Insights Preview

In diesem Artikel werden Updates für Datenspeicherung und Dateneingang für Azure Time Series Insights Preview beschrieben. Es werden die zugrunde liegende Speicherstruktur, das Dateiformat und die „Time Series-ID“-Eigenschaft besprochen. Der Artikel erläutert auch den zugrunde liegenden eingehenden Prozess, Best Practices und aktuelle Einschränkungen der Vorschau.

## <a name="data-ingress"></a>Dateneingang

Ihre Azure Time Series Insights-Umgebung (TSI) enthält eine Erfassungs-Engine, mit der Zeitreihendaten gesammelt, verarbeitet und gespeichert werden. Beim Planen Ihrer Umgebung sind einige Aspekte zu berücksichtigen, um die Verarbeitung aller eingehenden Daten sicherzustellen, ein hohes Eingangsvolumen zu erzielen und die Latenz bei der Erfassung zu minimieren (dies ist die Zeit, die TSI benötigt, um Daten aus der Ereignisquelle zu lesen und zu verarbeiten). In Time Series Insights Preview bestimmen Richtlinien für den Dateneingang, aus welchen Quellen Daten bezogen werden können und welches Format diese Daten haben müssen.

### <a name="ingress-policies"></a>Eingangsrichtlinien

Time Series Insights Preview unterstützt die folgenden Ereignisquellen:

- [Azure IoT Hub](../iot-hub/about-iot-hub.md)
- [Azure Event Hubs](../event-hubs/event-hubs-about.md)

Time Series Insights Preview unterstützt maximal zwei Ereignisquellen pro Instanz.
  
Azure Time Series Insights unterstützt JSON-Daten, die über Azure IoT Hub oder Azure Event Hubs übermittelt werden.

> [!WARNING] 
> Wenn Sie eine neue Ereignisquelle an Ihre Time Series Insights Preview-Umgebung anfügen, werden Sie möglicherweise eine hohe Latenz bei der ersten Erfassung feststellen, je nachdem, wie viele Ereignisse sich derzeit in Ihrer IoT Hub- oder Event Hub-Instanz befinden. Im Lauf der Datenerfassung sollte dieser hohe Latenzwert sinken. Wenn dies nicht der Fall ist, kontaktieren Sie uns, indem Sie im Azure-Portal ein Supportticket übermitteln.

## <a name="ingress-best-practices"></a>Best Practices für den Dateneingang

Folgende Best Practices werden empfohlen:

* Konfigurieren Sie Time Series Insights und eine IoT Hub- oder Event Hub-Instanz in derselben Region. So reduzieren Sie die durch das Netzwerk verursachte Erfassungslatenz.
* Planen Sie Ihre Anforderungen an das Eingangsvolumen, indem Sie die erwartete Erfassungsrate berechnen und überprüfen, ob diese sich innerhalb der unterstützten Rate befindet, wie weiter unten erläutert.
* Informieren Sie sich unter [Gestalten von JSON für Abfragen](./time-series-insights-update-how-to-shape-events.md) über die Optimierung und Gestaltung Ihrer JSON-Daten sowie über die aktuellen Einschränkungen der Vorschau.

### <a name="ingress-scale-and-limitations-in-preview"></a>Dateneingangsvolumen und Einschränkungen der Vorschauversion

Standardmäßig unterstützt Time Series Insights Preview beim anfänglichen Dateneingang ein Volumen von 1 Megabyte pro Sekunde (MB/s) pro Umgebung. Bei Bedarf steht ein Durchsatz von bis zu 16 MB/s zur Verfügung. Kontaktieren Sie uns in diesem Fall, indem Sie im Azure-Portal ein Supportticket übermitteln. Darüber hinaus besteht ein Limit von 0,5 MB/s pro Partition. Angesichts der Affinität zwischen einem IoT Hub-Gerät und einer Partition kann dies umfassende Auswirkungen auf Kunden haben, die in großem Umfang IoT Hub verwenden. In Szenarien, in denen ein Gatewaygerät Nachrichten unter Verwendung der eigenen Geräte-ID und einer eigenen Verbindungszeichenfolge an den Hub weiterleitet, besteht die Gefahr, dass der Grenzwert von 0,5 MB/s erreicht wird. Dies liegt daran, dass die Nachrichten bei einer einzelnen Partition eintreffen, auch wenn die Ereignisnutzlast verschiedene Time Series-IDs angibt. Im Allgemeinen wird die Eingangsrate als Faktor der Anzahl von Geräten in Ihrer Organisation, der Häufigkeit der Ereignisausgabe und der Größe eines Ereignisses betrachtet. Beim Berechnen der Erfassungsrate sollten IoT Hub-Benutzer die Anzahl der verwendeten Hubverbindungen einbeziehen, nicht die Gesamtanzahl von Geräten in der Organisation. Verbesserte Skalierungsunterstützung erfolgt fortwährend. Die vorliegende Dokumentation wird mit diesen Verbesserungen aktualisiert. 

> [!WARNING]
> In Umgebungen, die IoT Hub als Ereignisquelle verwenden, berechnen Sie die Erfassungsrate, nicht die Anzahl von verwendeten Hubgeräten.

Weitere Informationen zu Durchsatzeinheiten und Partitionen finden Sie unter den folgenden Links:

* [Skalieren von Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-scaling)
* [Skalierbarkeit – Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/event-hubs-scalability#throughput-units)
* [Event Hub-Partitionen](https://docs.microsoft.com/azure/event-hubs/event-hubs-features#partitions)

### <a name="data-storage"></a>Datenspeicher

Wenn Sie eine Time Series Insights Preview-Umgebung mit einer SKU mit nutzungsbasierter Bezahlung erstellen, erstellen Sie zwei Azure-Ressourcen:

* Eine Time Series Insights Preview-Umgebung, die optional warme Speicherfunktionen enthalten kann.
* Ein Azure Storage-Blobkonto vom Typ „Universell V1“ für die Speicherung von kalten Daten.

Daten im warmen Speicher sind nur über [Zeitreihenabfragen](./time-series-insights-update-tsq.md) und den [Azure Time Series Insights Preview-Explorer](./time-series-insights-update-explorer.md) verfügbar. 

Time Series Insights Preview speichert kalte Speicherdaten im [Parquet-Dateiformat](#parquet-file-format-and-folder-structure) im Azure-Blobspeicher. Time Series Insights Preview verwaltet diese kalten Speicherdaten exklusiv. Sie können diese Daten jedoch direkt als standardmäßige Parquet-Dateien lesen.

> [!WARNING]
> Als Besitzer des Azure-Blobspeicherkontos, in dem sich die kalten Daten befinden, haben Sie Vollzugriff auf alle Daten im Konto. Dieser Zugriff umfasst Berechtigungen zum Schreiben und Löschen. Bearbeiten oder löschen Sie keine von Time Series Insights Preview geschriebenen Daten, da dies zu Datenverlusten führen kann.

### <a name="data-availability"></a>Datenverfügbarkeit

Time Series Insights Preview partitioniert und indiziert Daten, um eine optimale Abfrageleistung zu erzielen. Die Daten stehen nach der Indizierung für Abfragen zur Verfügung. Die Menge an erfassten Daten kann sich auf diese Verfügbarkeit auswirken.

> [!IMPORTANT]
> Das anstehende allgemein verfügbare Time Series Insights-Release wird Daten innerhalb von 60 Sekunden nach dem Lesen aus der Ereignisquelle zur Verfügung stellen. Während der Vorschau kann es länger dauern, bis die Daten verfügbar sind. Wenn Sie eine erhebliche Latenz von weit über 60 Sekunden feststellen, übermitteln Sie ein Supportticket über das Azure-Portal.

## <a name="azure-storage"></a>Azure Storage

Dieser Abschnitt enthält für Azure Time Series Insights Preview relevante Informationen zu Azure Storage.

Eine ausführliche Beschreibung des Azure Blobspeichers finden Sie in der [Einführung in Azure Blob Storage](../storage/blobs/storage-blobs-introduction.md).

### <a name="your-storage-account"></a>Ihr Speicherkonto

Wenn Sie eine Time Series Insights Preview-Umgebung mit nutzungsbasierter Bezahlung erstellen, wird ein Azure Storage-Blobkonto vom Typ „Universell V1“ als langfristiger Speicher für kalte Daten erstellt.  

Time Series Insights Preview veröffentlicht bis zu zwei Kopien jedes Ereignisses in Ihrem Azure-Speicherkonto. In der ersten Kopie sind die Ereignisse nach Erfassungszeitpunkt sortiert. Diese Kopie wird dauerhaft beibehalten, sodass Sie andere Dienste für den Zugriff darauf verwenden können. Sie können Spark, Hadoop oder andere vertraute Tools verwenden, um die unformatierten Parquet-Dateien zu verarbeiten. 

Time Series Insights Preview partitioniert die Parquet-Dateien neu, um sie für die Time Series Insights-Abfragen zu optimieren. Diese neu partitionierte Kopie der Daten wird ebenfalls gespeichert.

Während der öffentlichen Preview werden Daten mit unbegrenzter Dauer in Ihrem Azure-Speicherkonto gespeichert.

### <a name="writing-and-editing-time-series-insights-blobs"></a>Schreiben und Bearbeiten von Time Series Insights-Blobs

Um Abfrageleistung und Datenverfügbarkeit sicherzustellen, bearbeiten oder löschen Sie keine Blobs, die von Time Series Insights Preview erstellt werden.

### <a name="accessing-and-exporting-data-from-time-series-insights-preview"></a>Zugreifen auf und Exportieren von Daten aus Time Series Insights Preview

Sie können auf im Time Series Insights Preview-Explorer gespeicherte Daten zugreifen, um sie in Verbindung mit anderen Diensten zu verwenden. Sie können die Daten z. B. verwenden, um einen Bericht in Power BI zu erstellen oder in Azure Machine Learning Studio ein Machine Learning-Modell zu trainieren. Sie können die Daten auch zum Transformieren, Visualisieren und Modellieren in Ihren Jupyter-Notebooks verwenden.

Sie können auf drei allgemeine Arten auf Ihre Daten zugreifen:

* Aus dem Time Series Insights Preview-Explorer. Sie können Daten als CSV-Datei aus dem Explorer exportieren. Weitere Informationen finden Sie unter [Time Series Insights Preview-Explorer](./time-series-insights-update-explorer.md).
* Über die Time Series Insights Preview-API. Sie können unter `/getRecorded` auf den API-Endpunkt zugreifen. Weitere Informationen zu dieser API finden Sie unter [Zeitreihenabfrage](./time-series-insights-update-tsq.md).
* Direkt aus einem Azure-Speicherkonto. Sie benötigen Lesezugriff auf das jeweilige Konto, das Sie für den Zugriff auf Ihre Time Series Insights Preview-Daten verwenden. Weitere Informationen finden Sie unter [Verwalten des Zugriffs auf Ihre Speicherkontoressourcen](../storage/blobs/storage-manage-access-to-resources.md).

### <a name="data-deletion"></a>Löschen von Daten

Löschen Sie Ihre Time Series Insights Preview-Daten nicht. Sie sollten zugehörige Daten nur innerhalb von Time Series Insights Preview verwalten.

## <a name="parquet-file-format-and-folder-structure"></a>Parquet-Dateiformat und Ordnerstruktur

Parquet ist ein spaltenbasiertes Open-Source-Dateiformat, das für effiziente Speicherung und Leistung konzipiert wurde. Aus diesen Gründen wird Parquet von Time Series Insights Preview verwendet. Die Daten werden anhand der Zeitreihen-ID partitioniert, um bei großen Datenmengen eine hohe Abfrageleistung zu erzielen.  

Weitere Informationen zum Parquet-Dateityp finden Sie in der [Parquet-Dokumentation](https://parquet.apache.org/documentation/latest/).

Time Series Insights Preview speichert wie folgt Kopien Ihrer Daten:

* Die erste, anfängliche Kopie wird anhand des Erfassungszeitpunkts partitioniert und speichert Daten in etwa in der Reihenfolge ihres Eingangs. Die Daten befinden sich im Ordner `PT=Time`:

  `V=1/PT=Time/Y=<YYYY>/M=<MM>/<YYYYMMDDHHMMSSfff>_<TSI_INTERNAL_SUFFIX>.parquet`

* Die zweite, neu partitionierte Kopie wird durch Gruppierung von Zeitreihen-IDs partitioniert und befindet sich im Ordner `PT=TsId`:

  `V=1/PT=TsId/Y=<YYYY>/M=<MM>/<YYYYMMDDHHMMSSfff>_<TSI_INTERNAL_SUFFIX>.parquet`

In beiden Fällen entsprechen die Werte für die Uhrzeit der Uhrzeit der Bloberstellung. Daten im Ordner `PT=Time` werden beibehalten. Daten im Ordner `PT=TsId` werden im Lauf der Zeit für Abfragen optimiert und bleiben nicht statisch.

> [!NOTE]
> * `<YYYY>` entspricht einer vierstelligen Jahresdarstellung.
> * `<MM>` entspricht einer zweistelligen Monatsdarstellung.
> * `<YYYYMMDDHHMMSSfff>` entspricht einer Zeitstempeldarstellung mit folgendem Format: vierstellige Jahresangabe (`YYYY`), zweistellige Monatsangabe (`MM`), zweistellige Tagesangabe (`DD`), zweistellige Stundenangabe (`HH`), zweistellige Minutenangabe (`MM`), zweistellige Sekundenangabe (`SS`) und dreistellige Millisekundenangabe (`fff`).

Time Series Insights Preview-Ereignisse werden dem Inhalt von Parquet-Dateien wie folgt zugeordnet:

* Jedes Ereignis wird einer einzelnen Zeile zugeordnet.
* Jede Zeile enthält die Spalte **timestamp** mit einem Zeitstempel für das Ereignis. Die timestamp-Eigenschaft ist nie NULL. Ihr Standardwert ist der **Zeitpunkt der Einreihung des Ereignisses in die Warteschlange**, wenn die timestamp-Eigenschaft in der Ereignisquelle nicht angegeben ist. Der Zeitstempel wird immer in UTC angegeben.
* Jede Zeile enthält eine Spalte mit der Zeitreihen-ID, wie beim Erstellen der Time Series Insights-Umgebung definiert. Der Eigenschaftsname enthält das Suffix `_string`.
* Alle anderen als Telemetriedaten gesendeten Eigenschaften werden Spaltennamen zugeordnet, die je nach Eigenschaftstyp auf `_string` (Zeichenfolge), `_bool` (boolesch), `_datetime` (Datum/Uhrzeit) und `_double` (Double) enden.
* Dieses Zuordnungsschema gilt für die erste Version des Dateiformats, auf das als **V=1** verwiesen wird. Mit der Weiterentwicklung dieses Features wird der Name hochgezählt.

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie den Artikel [Datenspeicherung und -eingang in Azure Time Series Insights Preview](./time-series-insights-update-storage-ingress.md).

- Informieren Sie sich über die neue [Datenmodellierung](./time-series-insights-update-tsm.md).
