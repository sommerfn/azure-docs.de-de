---
title: Verwenden von Azure Data Factory zum Migrieren von Daten aus einem Data Lake und Data Warehouse zu Azure | Microsoft-Dokumentation
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
ms.openlocfilehash: 937a076b3e0e3c5170779d3449776f0aa1cf5b49
ms.sourcegitcommit: 267a9f62af9795698e1958a038feb7ff79e77909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70258999"
---
# <a name="use-azure-data-factory-to-migrate-data-from-your-data-lake-or-data-warehouse-to-azure"></a>Verwenden von Azure Data Factory zum Migrieren von Daten aus einem Data Lake oder Data Warehouse zu Azure 

Azure Data Factory kann das Tool für die Datenmigration sein, wenn Sie Ihren Data Lake oder Ihr Enterprise Data Warehouse (EDW) zu Azure migrieren möchten. Die Data Lake- und Data Warehouse-Migration bezieht sich auf die folgenden Szenarien: 

- Migration von Big Data-Workloads aus AWS S3, einem lokalen Hadoop-Dateisystem zu Azure. 
- EDW-Migration aus Oracle Exadata, Netezza, Teradata, AWS Redshift zu Azure. 

Azure Data Factory kann PB an Daten für die Data Lake-Migration und Dutzende TB an Daten für die Data Warehouse-Migration verschieben. 

## <a name="why-azure-data-factory-can-be-used-for-data-migration"></a>Warum Azure Data Factory für die Datenmigration verwendet werden kann 

- Azure Data Factory kann leicht die Leistung erhöhen, um Daten serverlos mit Hochleistung, Ausfallsicherheit und Skalierbarkeit zu verschieben. Sie bezahlen dabei nur für das, was Sie verwenden.  
  - Azure Data Factory weist keine Einschränkung hinsichtlich der Datenmenge und der Anzahl von Dateien auf.
  - Azure Data Factory kann Ihr Netzwerk und Ihre Speicherbandbreite zu 100 % nutzen, um den höchsten Datenverschiebungsdurchsatz in Ihrer Umgebung zu erreichen.   
  - Azure Data Factory folgt der Strategie der nutzungsbasierten Bezahlung, damit Sie nur für die Zeit bezahlen müssen, in der Sie Azure Data Factory verwenden, um die Datenmigration zu Azure durchzuführen.  
- Azure Data Factory ist in der Lage, einen einmaligen historischen Ladevorgang sowie einen geplanten inkrementellen Ladevorgang auszuführen. 
- Azure Data Factory verwendet Azure IR zum Verschieben von Daten zwischen öffentlich zugänglichen Data Lake-/Warehouse-Endpunkten, oder verwenden Sie alternativ die selbstgehostete IR zum Verschieben von Daten für Data Lake/Warehouse-Endpunkte innerhalb des VNETs oder hinter der Firewall. 
- Azure Data Factory bietet Sicherheit auf Unternehmensniveau: Verwenden Sie MSI oder die Dienstidentität für eine sichere Dienst-zu-Dienst-Integration, oder nutzen Sie alternativ Azure Key Vault für die Verwaltung von Anmeldeinformationen. 
- Azure Data Factory bietet codefreie Dokumenterstellung und ein umfangreiches integriertes Überwachungsdashboard.  

## <a name="online-vs-offline-data-migration"></a>Online- im Vergleich zu Offlinedatenmigration

Azure Data Factory ist ein typisches Online-Datenmigrationstool für die Übertragung von Daten über ein Netzwerk (Internet, ER oder VPN), wohingegen im Rahmen der Offlinedatenmigration Personen physisch Datenübertragungsgeräte aus Ihrer Organisation an das Azure-Rechenzentrum versenden.  

Es gibt drei wichtige Überlegungen bei der Auswahl eines Online-oder Offlinemigrationsansatzes:  

- Die Größe der zu migrierenden Daten. 
- Die Netzwerkbandbreite. 
- Das Migrationsfenster.   

Wenn Sie die Datenmigration innerhalb von zwei Wochen (Migrationsfenster) abschließen möchten, sehen Sie in der folgenden Abbildung eine Schnittlinie, die zeigt, wann es sinnvoll ist, ein Onlinemigrationstool (Azure Data Factory) mit unterschiedlichen Datengrößen und Netzwerkbandbreiten einzusetzen.   

![Online im Vergleich zu offline](media/data-migration-guidance-overview/online-offline.png)

> [!NOTE]
> Der Vorteil des Onlinemigrationsansatzes besteht darin, dass Sie sowohl das historische Laden von Daten als auch inkrementelle Feeds durch ein einziges Tool in einem End-to-End-Vorgang erreichen können.  Auf diese Weise können die Daten während des gesamten Migrationsfensters zwischen vorhandenem und neuem Speicher synchron gehalten werden, sodass Sie Ihre ETL-Logik auf dem neuen Speicher mit aktualisierten Daten erneut erstellen können. 


## <a name="next-steps"></a>Nächste Schritte

- [Migrieren von Daten von AWS S3 zu Azure](data-migration-guidance-s3-azure-storage.md)
- [Migrieren der Daten von einem lokalen Hadoop-Cluster zu Azure](data-migration-guidance-hdfs-azure-storage.md)
- [Migrieren der Daten von einem lokalen Netezza-Server zu Azure](data-migration-guidance-netezza-azure-sqldw.md)
