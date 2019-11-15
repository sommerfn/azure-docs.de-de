---
title: Verwenden von Azure Data Factory zum Migrieren von Daten aus einem Data Lake und Data Warehouse zu Azure
description: Verwenden von Azure Data Factory zum Migrieren von Daten aus einem Data Lake und Data Warehouse zu Azure.
services: data-factory
documentationcenter: ''
author: dearandyxu
ms.author: yexu
ms.reviewer: ''
manager: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 7/30/2019
ms.openlocfilehash: 0be9cbc9c5af2e0778654ef70c5350b48f10c35d
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73675759"
---
# <a name="use-azure-data-factory-to-migrate-data-from-your-data-lake-or-data-warehouse-to-azure"></a>Verwenden von Azure Data Factory zum Migrieren von Daten aus einem Data Lake oder Data Warehouse zu Azure

Wenn Sie Ihren Data Lake oder Ihr Enterprise Data Warehouse (EDW) zu Microsoft Azure migrieren möchten, können Sie Azure Data Factory verwenden. Azure Data Factory eignet sich für folgende Szenarien:

- Migration von Big Data-Workloads aus Amazon Simple Storage Service (AWS S3) oder einem lokalen Hadoop Distributed File System (HDFS) zu Azure
- EDW-Migration aus Oracle Exadata, Netezza, Teradata oder Amazon Redshift zu Azure

Mit Azure Data Factory können bei der Data Lake-Migration Daten im Petabyte-Bereich (PB) und bei der Data Warehouse-Migration mehrere Terabytes (TB) an Daten verschoben werden.

## <a name="why-azure-data-factory-can-be-used-for-data-migration"></a>Warum Azure Data Factory für die Datenmigration verwendet werden kann

- Mit Azure Data Factory kann die Verarbeitungsleistung problemlos zentral hochskaliert werden, um Daten serverlos mit hoher Leistung, Resilienz und Skalierbarkeit zu verschieben. Sie bezahlen dabei nur für das, was Sie tatsächlich nutzen. Beachten Sie darüber hinaus Folgendes: 
  - Datenmenge und Dateianzahl sind bei Azure Data Factory unbegrenzt.
  - Azure Data Factory kann Ihre Netzwerk- und Speicherbandbreite vollständig ausnutzen, um in Ihrer Umgebung den höchstmöglichen Datenverschiebungsdurchsatz zu erzielen.
  - Dank des nutzungsbasierten Zahlungsmodells von Azure Data Factory zahlen Sie nur für die Zeit, die tatsächlich für die Datenmigration zu Azure beansprucht wurde.  
- Azure Data Factory kann sowohl einen einmaligen historischen Ladevorgang als auch geplante inkrementelle Ladevorgänge durchführen.
- Azure Data Factory nutzt Azure Integration Runtime (IR), um Daten zwischen öffentlich zugänglichen Data Lake- und Data Warehouse-Endpunkten zu verschieben. Von Azure Data Factory kann auch eine selbstgehostete IR verwendet werden, um Daten für Data Lake- und Data Warehouse-Endpunkte innerhalb von Azure Virtual Network (VNET) oder hinter einer Firewall zu verschieben.
- Azure Data Factory bietet Sicherheit auf Unternehmensniveau: Sie können den Windows Installer (MSI) oder die Dienstidentität für eine sichere Dienst-zu-Dienst-Integration verwenden oder Anmeldeinformationen mithilfe von Azure Key Vault verwalten.
- Azure Data Factory bietet eine codefreie Erstellungsumgebung sowie ein umfangreiches integriertes Überwachungsdashboard.  

## <a name="online-vs-offline-data-migration"></a>Online- im Vergleich zu Offlinedatenmigration

Azure Data Factory ist ein Standardtool für die Onlinedatenmigration und ermöglicht die Übertragung von Daten über ein Netzwerk (Internet, ER oder VPN). Bei der Offlinemigration versenden Benutzer dagegen physische Datenübertragungsgeräte von ihrer Organisation an ein Azure-Rechenzentrum.  

Bei der Entscheidung zwischen Online- und Offlinemigration gibt es drei wichtige Kriterien:  

- Größe der zu migrierenden Daten
- Netzwerkbandbreite
- Migrationszeitfenster

Ein Beispiel: Angenommen, Sie möchten Ihre Datenmigration mithilfe von Azure Data Factory innerhalb von zwei Wochen (Ihr *Migrationszeitfenster*) abwickeln. Beachten Sie die rosafarbene/blaue Markierung in der folgenden Tabelle. Die unterste rosafarbene Zelle einer Spalte gibt jeweils die Kombination aus Datengröße/Netzwerkbandbreite an, deren Migrationszeitfenster den zwei Wochen am nächsten kommt, ohne es zu übersteigen. (Bei den blau eingefärbten Kombinationen aus Größe/Bandbreite wird das zweiwöchige Zeitfenster für die Onlinemigration jeweils überschritten.) 

![Online oder offline](media/data-migration-guidance-overview/online-offline.png) Anhand dieser Tabelle können Sie auf der Grundlage der Größe Ihrer Daten und Ihrer verfügbaren Netzwerkbandbreite ermitteln, ob Sie Ihr gewünschtes Migrationszeitfenster bei einer Onlinemigration (Azure Data Factory) einhalten können. Beträgt das Zeitfenster für die Onlinemigration mehr als zwei Wochen, empfiehlt sich eine Offlinemigration.

> [!NOTE]
> Bei der Onlinemigration können Sie mithilfe eines einzigen Tools im Rahmen eines End-to-End-Vorgangs sowohl historische Daten laden als auch inkrementelle Feeds nutzen.  Dadurch bleiben Ihre Daten während des gesamten Migrationszeitfensters zwischen dem vorhandenen Speicher und dem neuen Speicher synchronisiert. Somit können Sie Ihre ETL-Logik im neuen Speicher mit aktualisierten Daten neu erstellen.


## <a name="next-steps"></a>Nächste Schritte

- [Migrieren von Daten von AWS S3 zu Azure](data-migration-guidance-s3-azure-storage.md)
- [Migrieren der Daten von einem lokalen Hadoop-Cluster zu Azure](data-migration-guidance-hdfs-azure-storage.md)
- [Migrieren der Daten von einem lokalen Netezza-Server zu Azure](data-migration-guidance-netezza-azure-sqldw.md)
