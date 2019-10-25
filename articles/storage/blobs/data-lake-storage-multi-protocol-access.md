---
title: Multiprotokollzugriff für Azure Data Lake Storage (Vorschauversion) | Microsoft-Dokumentation
description: Verwenden von BLOB-APIs und Anwendungen, die BLOB-APIs mit Azure Data Lake Storage Gen2 verwenden.
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 10/11/2019
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: 9767282b3dd764a45f25a14d62af70a13c80b0ac
ms.sourcegitcommit: 8b44498b922f7d7d34e4de7189b3ad5a9ba1488b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2019
ms.locfileid: "72300269"
---
# <a name="multi-protocol-access-on-azure-data-lake-storage-preview"></a>Multiprotokollzugriff für Azure Data Lake Storage (Vorschauversion)

BLOB-APIs können jetzt für Konten verwendet werden, die über einen hierarchischen Namespace verfügen. Dadurch werden das gesamte Ökosystem von Tools, Anwendungen und Diensten sowie alle BLOB Storage-Features für Konten mit einem hierarchischen Namespace verfügbar.

Bis vor Kurzem mussten Sie eventuell jeweils eigene Speicherlösungen für den Objektspeicher und den Analysespeicher verwalten. Die Ursache hierfür war die beschränkte Unterstützung des Ökosystems durch Azure Data Lake Storage Gen2. Der Zugriff auf BLOB-Dienstfeatures, z. B. die Diagnoseprotokollierung, war ebenfalls beschränkt. Eine fragmentierte Speicherlösung ist schwierig zu verwalten, da für verschiedene Szenarien Daten zwischen Konten verschoben werden müssen. Dies ist nicht mehr erforderlich.

> [!NOTE]
> Der Multiprotokollzugriff für Data Lake Storage befindet sich in der Public Preview-Phase und steht in allen Regionen zur Verfügung. Sie müssen sich nicht für die Public Preview registrieren, da sie automatisch für alle Konten mit einem hierarchischen Namespace verfügbar ist. Informationen zu den Einschränkungen finden Sie im Artikel [Bekannte Probleme](data-lake-storage-known-issues.md).

## <a name="use-the-entire-ecosystem-of-applications-tools-and-services"></a>Verwenden des gesamten Ökosystems von Anwendungen, Tools und Diensten

Mit Multiprotokollzugriff für Data Lake Storage können Sie das gesamte Ökosystem von Tools, Anwendungen und Diensten für alle Ihre Daten nutzen. Dies umfasst Azure-Dienste, z. B. [Azure Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-introduction), [IoT Hub](https://docs.microsoft.com/azure/iot-hub/), [Power BI](https://docs.microsoft.com/power-bi/desktop-data-sources) und viele andere. Eine vollständige Liste finden Sie unter [Integrieren von Azure Data Lake Storage mit Azure-Diensten](data-lake-store-integrate-with-azure-services.md).

Dies beinhaltet auch Tools und Anwendungen von Drittanbietern. Sie können sie auf Konten mit einem hierarchischen Namespace verweisen, ohne sie ändern zu müssen. Diese Anwendungen *erfordern keine Änderungen*, selbst wenn sie BLOB-APIs aufrufen, da BLOB-APIs jetzt für Daten in Konten mit einem hierarchischen Namespace verwendet werden können.

> [!NOTE]
> Informationen zu den Einschränkungen finden Sie im Artikel [Bekannte Probleme](data-lake-storage-known-issues.md).

## <a name="use-all-blob-storage-features"></a>Verwenden sämtlicher BLOB Storage-Features

BLOB Storage-Features, z. B. [Diagnoseprotokollierung](../common/storage-analytics-logging.md), [Zugriffsebenen](storage-blob-storage-tiers.md) und [Richtlinien für die Azure Blob Storage-Lebenszyklusverwaltung](storage-lifecycle-management-concepts.md) können jetzt für Konten mit einem hierarchischen Namespace verwendet werden. Daher können Sie hierarchische Namespaces in Ihren BLOB Storage-Konten aktivieren, ohne den Zugriff auf diese wichtigen Features zu verlieren. 

> [!NOTE]
> Informationen zu den Einschränkungen finden Sie im Artikel [Bekannte Probleme](data-lake-storage-known-issues.md).

## <a name="how-multi-protocol-access-on-data-lake-storage-works"></a>Funktionsweise des Multiprotokollzugriffs für Azure Data Lake Storage

BLOB-APIs und Data Lake Storage Gen2-APIs können in Speicherkonten mit einem hierarchischen Namespace für dieselben Daten verwendet werden. Data Lake Storage Gen2 leitet BLOB-APIs über den hierarchischen Namespace weiter, sodass Sie die Vorteile von erstklassigen Verzeichnisvorgängen und POSIX-konformen Zugriffssteuerungslisten (Access Control Lists, ACLs) nutzen können. 

![Konzept des Multiprotokollzugriffs für Azure Data Lake Storage](./media/data-lake-storage-interop/interop-concept.png) 

Vorhandene Tools und Anwendungen, die die BLOB-API verwenden, profitieren von diesen Vorteilen automatisch. Entwickler müssen sie nicht ändern. Data Lake Storage Gen2 wendet konsistent ACLs auf Verzeichnis- und Dateiebene an, unabhängig von dem Protokoll, das von den Tools und Anwendungen für den Zugriff auf die Daten verwendet wird. 

## <a name="next-steps"></a>Nächste Schritte

Siehe [Bekannte Probleme](data-lake-storage-known-issues.md)




