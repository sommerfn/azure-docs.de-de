---
title: Unterstützte Datenspeicher in Azure Data Share
description: Erfahren Sie mehr über die Datenspeicher, die zur Verwendung in Azure Data Share unterstützt werden.
ms.service: data-share
author: joannapea
ms.author: joanpo
ms.topic: conceptual
ms.date: 10/30/2019
ms.openlocfilehash: 762cea6dce3e0c6be3f5e977c5f9de806ca0880e
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73511379"
---
# <a name="supported-data-stores-in-azure-data-share"></a>Unterstützte Datenspeicher in Azure Data Share

Azure Data Share bietet eine offene und flexible Datenfreigabe, einschließlich der Möglichkeit zur Freigabe aus und in unterschiedliche Datenspeicher. Datenanbieter können Daten aus einem Datenspeichertyp freigeben, und ihre Datenconsumer können den Datenspeicher auswählen, in dem Daten empfangen werden sollen. 

In diesem Artikel erfahren Sie mehr über die umfangreiche Gruppe von Azure-Datenspeichern, die in Azure Data Share unterstützt werden. Außerdem erhalten Sie Informationen zu den Kombinationen von Datenspeichern, die von Datenanbietern und Datenconsumern genutzt werden können. 

## <a name="what-data-stores-are-supported-in-azure-data-share"></a>Welche Datenspeicher werden in Azure Data Share unterstützt? 

In der folgenden Tabelle sind die unterstützten Datenquellen für Azure Data Share aufgeführt. 

| Datenspeicher | Momentaufnahmebasierte Freigabe | Direkte Freigabe 
|:--- |:--- |:--- |:--- |:--- |:--- |
| Azure Blob Storage |✓ | |
| Azure Data Lake Storage Gen1 |✓ | |
| Azure Data Lake Storage Gen2 |✓ ||
| Azure SQL-Datenbank |Öffentliche Vorschau | |
| Azure SQL Data Warehouse |Öffentliche Vorschau | |
| Azure-Daten-Explorer | |[Eingeschränkte Vorschau](https://aka.ms/azuredatasharepreviewsignup) |

## <a name="data-store-support-matrix"></a>Datenspeicher-Unterstützungsmatrix

Azure Data Share bietet Datenconsumern Flexibilität bei der Entscheidung für einen Datenspeicher, in dem Daten empfangen werden. Beispielsweise können Daten, die von Azure SQL-Datenbank freigegeben werden, in Azure Data Lake Store Gen2, Azure SQL-Datenbank oder Azure SQL Data Warehouse empfangen werden. Kunden können beim Konfigurieren einer empfangenen Datenfreigabe das Format auswählen, in dem Daten empfangen werden sollen. 

In der folgenden Tabelle sind die verschiedenen Kombinationen und Auswahlmöglichkeiten aufgeführt, die Datenconsumern beim Akzeptieren und Konfigurieren ihrer Datenfreigabe zur Verfügung stehen. Weitere Informationen zum Konfigurieren von Datasetzuordnungen finden Sie unter [Konfigurieren von Datasetzuordnungen](how-to-configure-mapping.md).

|  | Azure Blob Storage | Azure SQL Data Lake Gen1 | Azure SQL Data Lake Gen2 | Azure SQL-Datenbank | Azure SQL Data Warehouse 
|:--- |:--- |:--- |:--- |:--- |:--- |
| Azure Blob Storage |✓ ||✓|
| Azure Data Lake Storage Gen1 |✓ | |✓|
| Azure Data Lake Storage Gen2 |✓ | |✓|
| Azure SQL-Datenbank |✓ | |✓|✓|✓|
| Azure SQL Data Warehouse |✓ | |✓|✓|✓|

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Freigeben von Daten finden Sie im Tutorial zum [Freigeben Ihrer Daten](share-your-data.md).
