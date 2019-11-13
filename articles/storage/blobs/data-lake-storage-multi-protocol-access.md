---
title: Multiprotokollzugriff für Azure Data Lake Storage | Microsoft-Dokumentation
description: Verwenden von BLOB-APIs und Anwendungen, die BLOB-APIs mit Azure Data Lake Storage Gen2 verwenden.
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 11/01/2019
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: f782c5abe611086baae5d2ad71bc38516039a231
ms.sourcegitcommit: 018e3b40e212915ed7a77258ac2a8e3a660aaef8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73796051"
---
# <a name="multi-protocol-access-on-azure-data-lake-storage"></a>Multiprotokollzugriff für Azure Data Lake Storage

BLOB-APIs können jetzt für Konten verwendet werden, die über einen hierarchischen Namespace verfügen. Dadurch werden das Ökosystem von Tools, Anwendungen und Diensten sowie mehrere Blobspeicherfunktionen für Konten mit einem hierarchischen Namespace verfügbar.

Bis vor Kurzem mussten Sie eventuell jeweils eigene Speicherlösungen für den Objektspeicher und den Analysespeicher verwalten. Die Ursache hierfür war die beschränkte Unterstützung des Ökosystems durch Azure Data Lake Storage Gen2. Der Zugriff auf BLOB-Dienstfeatures, z. B. die Diagnoseprotokollierung, war ebenfalls beschränkt. Eine fragmentierte Speicherlösung ist schwierig zu verwalten, da für verschiedene Szenarien Daten zwischen Konten verschoben werden müssen. Dies ist nicht mehr erforderlich.

Mit Multiprotokollzugriff für Data Lake Storage können Sie das Ökosystem von Tools, Anwendungen und Diensten für Ihre Daten nutzen. Dies beinhaltet auch Tools und Anwendungen von Drittanbietern. Sie können sie auf Konten mit einem hierarchischen Namespace verweisen, ohne sie ändern zu müssen. Diese Anwendungen *erfordern keine Änderungen*, selbst wenn sie BLOB-APIs aufrufen, da BLOB-APIs jetzt für Daten in Konten mit einem hierarchischen Namespace verwendet werden können.

BLOB Storage-Features, z. B. [Diagnoseprotokollierung](../common/storage-analytics-logging.md), [Zugriffsebenen](storage-blob-storage-tiers.md) und [Richtlinien für die Azure Blob Storage-Lebenszyklusverwaltung](storage-lifecycle-management-concepts.md) können jetzt für Konten mit einem hierarchischen Namespace verwendet werden. Daher können Sie hierarchische Namespaces in Ihren Blobspeicherkonten aktivieren, ohne den Zugriff auf diese wichtigen Features zu verlieren. 

> [!NOTE]
> Der Multiprotokollzugriff für Data Lake Storage steht allgemein und in allen Regionen zur Verfügung. Einige Azure-Dienste oder Blobspeicherfunktionen, die durch Multiprotokollzugriff aktiviert werden, befinden sich weiterhin in der Vorschauphase. Weitere Informationen finden in den Tabellen in den einzelnen Abschnitten dieses Artikels. 

## <a name="how-multi-protocol-access-on-data-lake-storage-works"></a>Funktionsweise des Multiprotokollzugriffs für Azure Data Lake Storage

BLOB-APIs und Data Lake Storage Gen2-APIs können in Speicherkonten mit einem hierarchischen Namespace für dieselben Daten verwendet werden. Data Lake Storage Gen2 leitet BLOB-APIs über den hierarchischen Namespace weiter, sodass Sie die Vorteile von erstklassigen Verzeichnisvorgängen und POSIX-konformen Zugriffssteuerungslisten (Access Control Lists, ACLs) nutzen können. 

![Konzept des Multiprotokollzugriffs für Azure Data Lake Storage](./media/data-lake-storage-interop/interop-concept.png) 

Vorhandene Tools und Anwendungen, die die BLOB-API verwenden, profitieren von diesen Vorteilen automatisch. Entwickler müssen sie nicht ändern. Data Lake Storage Gen2 wendet konsistent ACLs auf Verzeichnis- und Dateiebene an, unabhängig von dem Protokoll, das von den Tools und Anwendungen für den Zugriff auf die Daten verwendet wird. 

## <a name="blob-storage-feature-support"></a>Unterstützung für Blobspeicherfunktionen

Der Multiprotokollzugriff auf Data Lake Storage ermöglicht die Verwendung von mehr Blobspeicherfunktionen mit Ihrem Data Lake Storage. In der folgenden Tabelle sind die Features aufgelistet, die durch den Multiprotokollzugriff auf Data Lake Storage aktiviert werden. 

Die in dieser Tabelle angezeigten Elemente ändern sich im Laufe der Zeit, da die Unterstützung für Blobspeicherfunktionen laufend erweitert wird. 

> [!NOTE]
> Obwohl der Multiprotokollzugriff auf Data Lake Storage im Allgemeinen verfügbar ist, bleibt die Unterstützung für einige dieser Features weiterhin in der Vorschauphase. 

|Blobspeicherfunktionen | Supportebene |
|---|---|
|[Kalte Zugriffsebene](storage-blob-storage-tiers.md)|Allgemein verfügbar|
|BLOB-SDKs |Allgemein verfügbar|
|[PowerShell](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-powershell) |Allgemein verfügbar|
|[BEFEHLSZEILENSCHNITTSTELLE (CLI)](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-cli) |Allgemein verfügbar|
|[Diagnoseprotokolle](../common/storage-analytics-logging.md)| Vorschau|
|[Richtlinien für die Lebenszyklusverwaltung](storage-lifecycle-management-concepts.md)| Vorschau|
|[Benachrichtigungen über Azure Event Grid](data-lake-storage-events.md)|Vorschau|
|[Zugriffsebene „Archiv“](storage-blob-storage-tiers.md)| Vorschau|
|[blobfuse](storage-how-to-mount-container-linux.md)|Noch nicht unterstützt|
|[Unveränderlicher Speicher](storage-blob-immutable-storage.md)|Noch nicht unterstützt|
|[Momentaufnahmen](storage-blob-snapshots.md)|Noch nicht unterstützt|
|[Vorläufiges Löschen](storage-blob-soft-delete.md)|Noch nicht unterstützt|
|[Statische Websites](storage-blob-static-website.md)|Noch nicht unterstützt|

Weitere Informationen zu allgemein bekannten Problemen und Einschränkungen bei Azure Data Lake Storage Gen2 finden Sie unter [Bekannte Probleme](data-lake-storage-known-issues.md).

## <a name="azure-ecosystem-support"></a>Unterstützung für das Azure-Ökosystem

Der Multiprotokollzugriff auf Data Lake Storage ermöglicht auch Verbindungen von mehr Azure-Diensten mit Ihrem Data Lake Storage. In der folgenden Tabelle sind die Dienste aufgelistet, die durch den Multiprotokollzugriff auf Data Lake Storage aktiviert werden. 

Ebenso wie bei der Tabelle der unterstützten Blobspeicherfunktionen ändern sich die in dieser Tabelle angezeigten Elemente im Laufe der Zeit, da die Unterstützung für Azure-Dienste laufend erweitert wird. 

> [!NOTE]
> Obwohl der Multiprotokollzugriff auf Data Lake Storage im Allgemeinen verfügbar ist, bleibt die Unterstützung für einige dieser Dienste weiterhin in der Vorschauphase. 

|Azure-Dienst | Supportebene |
|---|---|
|[Azure Data Box](data-lake-storage-migrate-on-premises-hdfs-cluster.md)|Allgemein verfügbar|
|[Azure Event Hubs-Erfassung](https://docs.microsoft.com/azure/event-hubs/event-hubs-capture-overview)|Allgemein verfügbar|
|[Azure Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-quick-create-portal)|Allgemein verfügbar|
|[IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-d2c)|Allgemein verfügbar|
|[Logik-Apps](https://azure.microsoft.com/services/logic-apps/)|Allgemein verfügbar|
|[Azure Cognitive Search](https://docs.microsoft.com/azure/search/search-howto-index-azure-data-lake-storage)|Vorschau|

Eine umfassende Liste der Azure-Ökosystemunterstützung für Azure Data Lake Storage Gen2 verwenden können, finden Sie unter [Integrieren von Azure Data Lake Storage mit Azure-Diensten](data-lake-storage-integrate-with-azure-services.md).

Weitere Informationen zu allgemein bekannten Problemen und Einschränkungen bei Azure Data Lake Storage Gen2 finden Sie unter [Bekannte Probleme](data-lake-storage-known-issues.md).

## <a name="next-steps"></a>Nächste Schritte

Siehe [Bekannte Probleme](data-lake-storage-known-issues.md)




