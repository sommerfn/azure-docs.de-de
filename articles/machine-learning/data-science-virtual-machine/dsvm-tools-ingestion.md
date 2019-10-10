---
title: Datenerfassungstools
titleSuffix: Azure Data Science Virtual Machine
description: Erfahren Sie mehr über die Datenerfassungstools und Hilfsprogramme, die in der Data Science Virtual Machine vorinstalliert sind.
keywords: Data Science-Tools, virtuelle Computer für Data Science, Tools für Data Science, Linux Data Science
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: gvashishtha
ms.author: gopalv
ms.topic: conceptual
ms.date: 10/3/2019
ms.openlocfilehash: 47a701cce348e86359947376b3d6a9915dfb2264
ms.sourcegitcommit: 4f7dce56b6e3e3c901ce91115e0c8b7aab26fb72
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2019
ms.locfileid: "71950159"
---
# <a name="data-science-virtual-machine-data-ingestion-tools"></a>Datenerfassungstools für die Data Science-VM

Als einen der ersten technischen Schritte in einem Data Science- oder KI-Projekt müssen Sie die zu verwendenden Datasets bestimmen und diese in Ihre Analyseumgebung importieren. Die Data Science Virtual Machine (DSVM) stellt Tools und Bibliotheken bereit, um Daten aus verschiedenen Quellen lokal in einen analytischen Datenspeicher auf der DSVM oder Daten in eine cloudbasierte oder lokale Datenplattform zu importieren.

Nachstehend sind einige der Datenverschiebungstools aufgeführt, die in der DSVM verfügbar sind.

## <a name="adlcopy"></a>AdlCopy

|    |           |
| ------------- | ------------- |
| Was ist das?   | Ein Tool zum Kopieren von Daten aus Azure-Blobspeicher in Azure Data Lake Store. Kann auch zum Kopieren von Daten zwischen zwei Azure Data Lake Store-Konten verwendet werden.      |
| Unterstützte DSVM-Versionen      | Windows      |
| Typische Verwendung      | Importieren mehrerer Blobs aus Azure-Blobspeicher in Azure Data Lake Store.      |
|  Verwendung/Ausführung    |   Öffnen Sie eine Eingabeaufforderung, und geben Sie `adlcopy` ein, um die Hilfe anzuzeigen.    |
| Links zu Beispielen      | [Verwenden von AdlCopy](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-copy-data-azure-storage-blob)      |
| Verwandte Tools auf der DSVM      | AzCopy, Azure CLI     |

## <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

|    |           |
| ------------- | ------------- |
| Was ist das?   | Ein Verwaltungstool für Azure. Enthält auch Befehlsverben zum Verschieben von Daten aus Azure-Datenplattformen wie Azure-Blobspeicher und Azure Data Lake Store.     |
| Unterstützte DSVM-Versionen      | Windows, Linux     |
| Typische Verwendung      | Importieren und Exportieren von Daten in bzw. aus Azure Storage und Azure Data Lake Store.      |
|  Verwendung/Ausführung    |   Öffnen Sie eine Eingabeaufforderung, und geben Sie `az` ein, um die Hilfe anzuzeigen.    |
| Links zu Beispielen      | [Verwenden der Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure)     |
| Verwandte Tools auf der DSVM      | AzCopy, AdlCopy      |


## <a name="azcopy"></a>AzCopy

|    |           |
| ------------- | ------------- |
| Was ist das?   | Ein Tool zum Kopieren von Daten in lokale und aus lokalen Dateien sowie in und aus Azure-Blobspeicher, -Dateien und -Tabellen.      |
| Unterstützte DSVM-Versionen      | Windows      |
| Typische Verwendung      | Kopieren von Dateien in Azure-Blobspeicher und Kopieren von Blobs zwischen Konten.      |
|  Verwendung/Ausführung    |   Öffnen Sie eine Eingabeaufforderung, und geben Sie `azcopy` ein, um die Hilfe anzuzeigen.    |
| Links zu Beispielen      | [AzCopy unter Windows](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy)      |
| Verwandte Tools auf der DSVM      | AdlCopy     |


## <a name="azure-cosmos-db-data-migration-tool"></a>Azure Cosmos DB-Datenmigrationstool

|    |           |
| ------------- | ------------- |
| Was ist das?   | Tool, mit dem Daten aus verschiedenen Quellen in Azure Cosmos DB importiert werden können. Zu diesen Quellen gehören JSON-Dateien, CSV-Dateien, SQL, MongoDB, Azure-Tabellenspeicher, Amazon DynamoDB und Azure Cosmos DB-SQL-API-Sammlungen.      |
| Unterstützte DSVM-Versionen      | Windows      |
| Typische Verwendung      | Importieren von Dateien von einem virtuellen Computer in CosmosDB, Importieren von Daten aus Azure-Tabellenspeicher in CosmosDB und Importieren von Daten aus einer Microsoft SQL Server-Datenbank in CosmosDB     |
|  Verwendung/Ausführung    |   Öffnen Sie zum Verwenden der Befehlszeilenversion eine Eingabeaufforderung, und geben Sie `dt` ein. Öffnen Sie zum Verwenden des GUI-Tools eine Eingabeaufforderung, und geben Sie `dtui` ein.    |
| Links zu Beispielen      | [Importieren von Daten in Azure Cosmos DB mit der DocumentDB-API](https://docs.microsoft.com/azure/cosmos-db/import-data)      |
| Verwandte Tools auf der DSVM      | AzCopy, AdlCopy      |


## <a name="bcp"></a>bcp

|    |           |
| ------------- | ------------- |
| Was ist das?   | SQL Server-Tool zum Kopieren von Daten zwischen SQL Server und einer Datendatei.      |
| Unterstützte DSVM-Versionen      | Windows      |
| Typische Verwendung      | Importieren einer CSV-Datei in eine SQL Server-Tabelle und Exportieren einer SQL Server-Tabelle in eine Datei.      |
|  Verwendung/Ausführung    |   Öffnen Sie eine Eingabeaufforderung, und geben Sie `bcp` ein, um die Hilfe anzuzeigen.    |
| Links zu Beispielen      | [bcp-Hilfsprogramm](https://docs.microsoft.com/sql/tools/bcp-utility)      |
| Verwandte Tools auf der DSVM      | SQL Server, sqlcmd      |

## <a name="blobfuse"></a>blobfuse

|    |           |
| ------------- | ------------- |
| Was ist das?   | Ein Tool zum Einbinden eines Azure-Blobspeichercontainers in das Linux-Dateisystem.      |
| Unterstützte DSVM-Versionen      | Linux      |
| Typische Verwendung      | Lesen aus und Schreiben in Blobs in einem Container.      |
|  Verwenden und Ausführen    |   Führen Sie _blobfuse_ auf einem Terminal aus.    |
| Links zu Beispielen      | [blobfuse auf GitHub](https://github.com/Azure/azure-storage-fuse)      |
| Verwandte Tools auf der DSVM      | Azure-Befehlszeilenschnittstelle      |
