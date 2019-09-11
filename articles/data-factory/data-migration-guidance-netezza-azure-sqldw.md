---
title: Verwenden von Azure Data Factory zum Migrieren von Daten von einem lokalen Netezza-Server zu Azure | Microsoft-Dokumentation
description: Verwenden Sie Azure Data Factory, um Daten von einem lokalen Netezza-Server zu Azure zu migrieren.
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
ms.date: 9/03/2019
ms.openlocfilehash: 4690fd81247035267861b06c204c6db7a052eba5
ms.sourcegitcommit: 267a9f62af9795698e1958a038feb7ff79e77909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70259458"
---
# <a name="use-azure-data-factory-to-migrate-data-from-on-premises-netezza-server-to-azure"></a>Verwenden von Azure Data Factory zum Migrieren von Daten von einem lokalen Netezza-Server zu Azure 

Azure Data Factory stellt einen leistungsstarken, stabilen und kostengünstigen Mechanismus bereit, um Daten bedarfsabhängig von einem lokalen Netezza-Server zu Azure Storage oder Azure SQL Data Warehouse zu migrieren. Dieser Artikel enthält für Datentechniker und Entwickler Informationen zu den folgenden Bereichen:

> [!div class="checklist"]
> * Leistung 
> * Resilienz beim Kopieren
> * Netzwerksicherheit
> * Allgemeine Lösungsarchitektur 
> * Bewährte Methoden für die Implementierung  

## <a name="performance"></a>Leistung

Azure Data Factory verfügt über eine serverlose Architektur, die Parallelität auf unterschiedlichen Ebenen ermöglicht. Entwickler können dann Pipelines erstellen, um Ihre Netzwerkbandbreite sowie die Bandbreite der Datenbank vollständig zu nutzen und so den Durchsatz für die Datenverschiebung in Ihrer Umgebung zu maximieren.

![Leistung](media/data-migration-guidance-netezza-azure-sqldw/performance.png)

- Für eine Kopieraktivität können skalierbare Computeressourcen genutzt werden: Bei Verwendung der Azure Integration Runtime können Sie [bis zu 256 Datenintegrationseinheiten (DIUs)](https://docs.microsoft.com/azure/data-factory/copy-activity-performance#data-integration-units) serverlos für jede Kopieraktivität angeben. Wenn Sie die selbstgehostete Integration Runtime nutzen, können Sie den Computer manuell zentral hochskalieren oder das horizontale Hochskalieren auf mehrere Computer durchführen ([bis zu vier Knoten](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime#high-availability-and-scalability)). Bei einer einzelnen Kopieraktivität wird die Partition dann auf alle Knoten verteilt. 
- Für eine Kopieraktivität werden mehrere Threads genutzt, um für den Datenspeicher Lese- und Schreibvorgänge durchzuführen. 
- Mit der Azure Data Factory-Ablaufsteuerung können mehrere Kopieraktivitäten parallel gestartet werden, z.B. per [ForEach-Schleife](https://docs.microsoft.com/azure/data-factory/control-flow-for-each-activity). 

Weitere Informationen finden Sie im [Handbuch zur Leistung der Kopieraktivität](https://docs.microsoft.com/azure/data-factory/copy-activity-performance).

## <a name="resilience"></a>Resilienz

Bei Ausführung einer einzelnen Kopieraktivität verfügt Azure Data Factory über einen integrierten Wiederholungsmechanismus, damit vorübergehende Fehler in den Datenspeichern oder im zugrunde liegenden Netzwerk bis zu einem gewissen Grad gehandhabt werden können.

Die Kopieraktivität von Azure Data Factory bietet beim Kopieren von Daten zwischen Quell- und Senkendatenspeichern außerdem zwei Optionen für den Umgang mit inkompatiblen Zeilen. Sie können die Kopieraktivität entweder mit einem Fehler abbrechen, wenn inkompatible Daten auftreten, oder den Kopiervorgang für die übrigen Daten fortsetzen, indem inkompatiblen Datenzeilen übersprungen werden. Darüber hinaus können Sie die inkompatiblen Zeilen in Azure Blob Storage oder Azure Data Lake Store protokollieren, um die Ursache des Fehlers zu ermitteln, die Daten in der Datenquelle korrigieren und die Kopieraktivität wiederholen.

## <a name="network-security"></a>Netzwerksicherheit 

Standardmäßig werden Daten mit Azure Data Factory über eine verschlüsselte Verbindung per HTTPS-Protokoll von einem lokalen Netezza-Server an Azure Storage oder Azure SQL Data Warehouse übertragen. Dies ermöglicht die Datenverschlüsselung während der Übertragung und verhindert Abhör- und Man-in-the-Middle-Angriffe.

Falls Sie nicht möchten, dass Daten über das öffentliche Internet übertragen werden, können Sie auch eine bessere Sicherheit erzielen, indem Sie Daten über einen privaten Peeringlink per Azure ExpressRoute übertragen. Unten im Abschnitt zur Lösungsarchitektur ist beschrieben, wie Sie dies erreichen.

## <a name="solution-architecture"></a>Lösungsarchitektur

Migrieren von Daten über das öffentliche Internet:

![solution-architecture-public-network](media/data-migration-guidance-netezza-azure-sqldw/solution-architecture-public-network.png)

- Bei dieser Architektur werden Daten auf sichere Weise per HTTPS über das öffentliche Internet übertragen.
- Sie müssen die selbstgehostete Integration Runtime von Azure Data Factory auf einem Windows-Computer hinter der Unternehmensfirewall installieren, um diese Architektur zu erhalten. Stellen Sie sicher, dass die selbstgehostete Integration Runtime von Azure Data Factory auf einem Windows-Computer direkten Zugriff auf Ihren Netezza-Server hat. Sie können Ihren Computer manuell zentral hochskalieren oder das horizontale Hochskalieren auf mehrere Computer durchführen, um die Bandbreite Ihres Netzwerks und der Datenspeicher zum Kopieren von Daten vollständig zu nutzen.
- Mit dieser Architektur ist sowohl die Migration der Daten einer Anfangsmomentaufnahme als auch der Deltadaten möglich.

Migrieren von Daten über einen privaten Link: 

![solution-architecture-private-network](media/data-migration-guidance-netezza-azure-sqldw/solution-architecture-private-network.png)

- Bei dieser Architektur wird die Datenmigration über einen privaten Peeringlink per Azure ExpressRoute durchgeführt, damit Daten niemals über das öffentliche Internet übertragen werden. 
- Sie müssen die selbstgehostete Integration Runtime von Azure Data Factory auf einer Windows-VM in Ihrem virtuellen Azure-Netzwerk installieren, um diese Architektur zu erhalten. Sie können Ihre VMs manuell zentral hochskalieren oder das horizontale Hochskalieren auf mehrere VMs durchführen, um die Bandbreite Ihres Netzwerks und der Datenspeicher zum Kopieren von Daten vollständig zu nutzen.
- Mit dieser Architektur ist sowohl die Migration der Daten einer Anfangsmomentaufnahme als auch der Deltadaten möglich.

## <a name="implementation-best-practices"></a>Bewährte Methoden für die Implementierung 

### <a name="authentication-and-credential-management"></a>Authentifizierung und Verwaltung von Anmeldeinformationen 

- Zum Authentifizieren bei Netezza können Sie die [ODBC-Authentifizierung über eine Verbindungszeichenfolge](https://docs.microsoft.com/azure/data-factory/connector-netezza#linked-service-properties) verwenden. 
- Für die Verbindungsherstellung mit Azure Blob Storage werden mehrere Authentifizierungstypen unterstützt.  Die Verwendung von [verwalteten Identitäten für Azure-Ressourcen](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage#managed-identity) wird dringend empfohlen: Sie basieren auf einer automatisch verwalteten Azure Data Factory-Identität in Azure AD und ermöglichen Ihnen die Konfiguration von Pipelines, ohne dass in der Definition des verknüpften Diensts Anmeldeinformationen angegeben werden müssen.  Alternativ können Sie die Authentifizierung für Azure Blob Storage auch per [Dienstprinzipal](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage#service-principal-authentication), [Shared Access Signature](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage#shared-access-signature-authentication) oder [Speicherkontoschlüssel](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage#account-key-authentication) durchführen. 
- Auch für die Verbindungsherstellung mit Azure Data Lake Storage Gen2 werden mehrere Authentifizierungstypen unterstützt.  Die Verwendung von [verwalteten Identitäten für Azure-Ressourcen](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage#managed-identity) wird dringend empfohlen. Es können aber auch ein [Dienstprinzipal](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage#service-principal-authentication) oder ein [Speicherkontoschlüssel](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage#account-key-authentication) verwendet werden. 
- Auch für die Verbindungsherstellung mit Azure SQL Data Warehouse werden mehrere Authentifizierungstypen unterstützt. Die Verwendung von [verwalteten Identitäten für Azure-Ressourcen](https://docs.microsoft.com/azure/data-factory/connector-azure-sql-data-warehouse#managed-identity) wird dringend empfohlen. Es können aber auch ein [Dienstprinzipal](https://docs.microsoft.com/azure/data-factory/connector-azure-sql-data-warehouse#service-principal-authentication) oder eine [SQL-Authentifizierung](https://docs.microsoft.com/azure/data-factory/connector-azure-sql-data-warehouse#sql-authentication) verwendet werden.
- Wenn Sie keine verwalteten Identitäten für Azure-Ressourcen nutzen, ist das [Speichern der Anmeldeinformationen in Azure Key Vault](https://docs.microsoft.com/azure/data-factory/store-credentials-in-key-vault) dringend zu empfehlen, um das zentrale Verwalten und Rotieren von Schlüsseln ohne Änderung der verknüpften Azure Data Factory-Dienste zu vereinfachen.  Dies ist auch eine der [bewährten Methoden für CI/CD](https://docs.microsoft.com/azure/data-factory/continuous-integration-deployment#best-practices-for-cicd). 

### <a name="initial-snapshot-data-migration"></a>Migration der Daten der Anfangsmomentaufnahme 

Bei kleinen Tabellen, deren Volumegröße weniger als 100 GB beträgt oder die innerhalb von zwei Stunden zu Azure migriert werden können, können mit jedem Kopieraufträge die Daten pro Tabelle geladen werden. Sie können mehrere Azure Data Factory-Kopieraufträge ausführen, um unterschiedliche Tabellen gleichzeitig zu laden und so einen besseren Durchsatz zu erzielen. 

Innerhalb der einzelnen Kopieraufträge können Sie ebenfalls einen gewissen Grad an Parallelität erreichen, indem Sie die [parallelCopies-Einstellung](https://docs.microsoft.com/azure/data-factory/copy-activity-performance#parallel-copy) mit der Datenpartitionsoption verwenden, um parallele Abfragen auszuführen und Daten nach Partitionen zu kopieren. Es stehen zwei Datenpartitionsoptionen zur Auswahl, die nachfolgend genauer beschrieben sind.
- Es wird empfohlen, mit dem Datenslice zu beginnen, da dies effizienter ist.  Stellen Sie sicher, dass die Anzahl für Parallelität in der parallelCopies-Einstellung unter der Gesamtzahl der Datenslicepartitionen in Ihrer Tabelle auf dem Netezza-Server liegt.  
- Wenn die Volumegröße für die einzelnen Datenslicepartitionen immer noch hoch ist (z.B. mehr als 10 GB), wird empfohlen, zu einer dynamischen Bereichspartition zu wechseln. Diese bietet größere Flexibilität beim Definieren der Anzahl von Partitionen und der Größe des Volumes für jede Partition nach Partitionsspalte, Obergrenze und Untergrenze.

Bei großen Tabellen, deren Volumegröße mehr als 100 GB beträgt oder die nicht innerhalb von zwei Stunden zu Azure migriert werden können, empfiehlt es sich, die Daten nach benutzerdefinierter Abfrage zu partitionieren und dann mit jedem Kopierauftrag jeweils eine Partition zu kopieren. Sie können mehrere Azure Data Factory-Kopieraufträge gleichzeitig ausführen, um einen besseren Durchsatz zu erzielen. Für jedes Kopierauftragsziel zum Laden einer Partition nach benutzerdefinierter Abfrage kann zudem Parallelität per Datenslice oder dynamischem Bereich aktiviert werden, um den Durchsatz zu erhöhen. 

Falls Kopieraufträge aufgrund eines vorübergehenden Problems mit dem Netzwerk oder dem Datenspeicher nicht erfolgreich sind, können Sie den fehlerhaften Kopierauftrag erneut ausführen, um diese spezifische Partition erneut aus der Tabelle zu laden. Alle anderen Kopieraufträge, bei denen andere Partitionen geladen werden, werden nicht beeinträchtigt.

Beim Laden von Daten in Azure SQL Data Warehouse wird die Aktivierung von PolyBase innerhalb des Kopierauftrags mit Azure Blob Storage als Staging empfohlen.

### <a name="delta-data-migration"></a>Migration von Deltadaten 

Um die neuen oder aktualisierten Zeilen aus der Tabelle zu identifizieren, wird eine Zeitstempelspalte oder ein Inkrementierungsschlüssel innerhalb des Schemas verwendet. Anschließend wird der neueste Wert als hoher Grenzwert in einer externen Tabelle gespeichert, die zum Filtern der Deltadaten beim nächsten Laden von Daten verwendet werden kann. 

Verschiedene Tabellen können unterschiedliche Grenzwertspalten verwenden, um die neuen oder aktualisierten Zeilen zu identifizieren. Es wird empfohlen, eine externe Steuerungstabelle zu erstellen, in der jede Zeile eine Tabelle auf dem Netezza-Server mit dem jeweiligen Namen der Grenzwertspalte und dem hohen Grenzwert darstellt. 

### <a name="self-hosted-integration-runtime-configuration-on-azure-vm-or-machine"></a>Konfiguration der selbstgehosteten Integration Runtime auf Azure-VM oder -Computer

Wenn Sie Daten von einem Netezza-Server zu Azure migrieren, müssen Sie unabhängig davon, ob der Netezza-Server lokal hinter der Unternehmensfirewall oder innerhalb einer VNET-Umgebung vorhanden ist, die selbstgehostete Integration Laufzeit auf einem Windows-Computer oder einer Windows-VM als Engine zum Verschieben von Daten installieren.

- Die empfohlene Konfiguration, mit der für jeden Computer oder jede VM begonnen werden sollte, ist eine Konfiguration mit 32 vCPUs und 128 GB Arbeitsspeicher. Sie können die Auslastung der CPU und des Arbeitsspeichers für den IR-Computer während der Datenmigration weiter überwachen. So können Sie ermitteln, ob Sie den Computer weiter zentral hochskalieren müssen, um die Leistung zu verbessern, oder zentral herunterskalieren müssen, um Kosten zu sparen.
- Sie können auch horizontal hochskalieren, indem Sie bis zu vier Knoten einer selbstgehosteten Integration Runtime zuordnen. Bei einem einzelnen Kopierauftrag, der für eine selbstgehostete IR ausgeführt wird, werden automatisch alle VM-Knoten genutzt, um die Daten parallel zu kopieren. Zur Sicherstellung von Hochverfügbarkeit ist es ratsam, mit zwei VM-Knoten zu beginnen, um bei der Datenmigration einen Single Point of Failure zu vermeiden.

### <a name="rate-limiting"></a>Ratenbegrenzung

Die bewährte Methode besteht darin, mit einem repräsentativen Beispieldataset einen Proof of Concept-Vorgang in Bezug auf die Leistung durchzuführen, damit Sie eine geeignete Partitionsgröße für jede Kopieraktivität ermitteln können. Es wird empfohlen, jede Partition so zu dimensionieren, dass sie innerhalb von zwei Stunden in Azure geladen wird.  

Beginnen Sie mit einer einzelnen Kopieraktivität mit einem einzelnen Computer mit der selbstgehosteten IR, um eine Tabelle zu kopieren. Erhöhen Sie schrittweise die parallelCopies-Einstellung basierend auf der Anzahl von Datenslicepartitionen in der Tabelle, und prüfen Sie, ob die gesamte Tabelle gemäß dem erkannten Durchsatz des Kopierauftrags innerhalb von zwei Stunden geladen werden kann. 

Wenn dies nicht möglich ist und gleichzeitig die Kapazität des Knotens der selbstgehosteten IR und des Datenspeichers nicht voll ausgeschöpft wird, erhöhen Sie schrittweise die Anzahl der gleichzeitigen Kopieraktivitäten, bis Sie die Grenzen Ihres Netzwerks oder das Bandbreitenlimit der Datenspeicher erreichen. 

Überwachen Sie weiterhin die CPU-/Arbeitsspeicherauslastung auf dem Computer mit der selbstgehosteten IR, und halten Sie sich bereit, den Computer zentral hochzuskalieren oder das horizontale Hochskalieren auf mehrere Computer durchzuführen, wenn die CPU bzw. der Arbeitsspeicher voll ausgelastet ist. 

Wenn für die Azure Data Factory-Kopieraktivität Drosselungsfehler gemeldet werden, sollten Sie entweder die Parallelitäts- oder die parallelCopies-Einstellung in Azure Data Factory reduzieren oder erwägen, die IOPS-/Bandbreitenlimits für das Netzwerk und die Datenspeicher zu erhöhen. 


### <a name="estimating-price"></a>Schätzen des Preises 

Sehen Sie sich die folgende Pipeline zum Migrieren von Daten vom lokalen Netezza-Server zu Azure SQL Data Warehouse an:

![pricing-pipeline](media/data-migration-guidance-netezza-azure-sqldw/pricing-pipeline.png)

Wir nehmen Folgendes an: 

- Das gesamte Datenvolumen beträgt 50 TB. 
- Das Migrieren der Daten erfolgt mithilfe der ersten Lösungsarchitektur (Netezza-Server befindet sich lokal hinter der Firewall).
- Die 50 TB sind in 500 Partitionen unterteilt, und bei jeder Kopieraktivität wird eine Partition verschoben.
- Jede Kopieraktivität ist mit einer selbstgehosteten IR für vier Computer konfiguriert und erzielt einen Durchsatz von 20 MBit/s. (Innerhalb der Kopieraktivität ist parallelCopies auf 4 festgelegt, und jeder Thread zum Laden von Daten aus der Tabelle erzielt einen Durchsatz von 5 MBit/s.)
- Die ForEach-Parallelität ist auf 3 festgelegt, und der aggregierte Durchsatz beträgt 60 MBit/s.
- Insgesamt dauert es 243 Stunden, bis die Migration abgeschlossen ist.

Hier ist der geschätzte Preis basierend auf den obigen Annahmen angegeben: 

![pricing-table](media/data-migration-guidance-netezza-azure-sqldw/pricing-table.png)

> [!NOTE]
> Dies ist ein hypothetisches Preisbeispiel. Der tatsächliche Preis richtet sich nach dem tatsächlichen Durchsatz in Ihrer Umgebung. Der Preis für den Windows-Computer (mit installierter selbstgehosteter Integration Runtime) ist nicht enthalten. 

### <a name="additional-references"></a>Zusätzliche Verweise 
- [Datenmigration von einem relationalen lokalen Data Warehouse zu Azure mithilfe von Azure Data Factory](https://azure.microsoft.com/mediahandler/files/resourcefiles/data-migration-from-on-premise-relational-data-warehouse-to-azure-data-lake-using-azure-data-factory/Data_migration_from_on-prem_RDW_to_ADLS_using_ADF.pdf)
- [Netezza-Connector](https://docs.microsoft.com/azure/data-factory/connector-netezza)
- [ODBC-Connector](https://docs.microsoft.com/azure/data-factory/connector-odbc)
- [Azure Blob Storage-Connector](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage)
- [Azure Data Lake Storage Gen2-Connector](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage)
- [Azure SQL Data Warehouse-Connector](https://docs.microsoft.com/azure/data-factory/connector-azure-sql-data-warehouse)
- [Handbuch zur Leistung und Optimierung der Kopieraktivität](https://docs.microsoft.com/azure/data-factory/copy-activity-performance)
- [Erstellen und Konfigurieren einer selbstgehosteten Integration Runtime](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime)
- [Selbstgehostete Integration Runtime: Hochverfügbarkeit und Skalierbarkeit](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime#high-availability-and-scalability)
- [Sicherheitsüberlegungen für Datenverschiebung in Azure Data Factory](https://docs.microsoft.com/azure/data-factory/data-movement-security-considerations)
- [Speichern von Anmeldeinformationen in Azure Key Vault](https://docs.microsoft.com/azure/data-factory/store-credentials-in-key-vault)
- [Inkrementelles Kopieren von Daten aus einer einzelnen Tabelle](https://docs.microsoft.com/azure/data-factory/tutorial-incremental-copy-portal)
- [Inkrementelles Kopieren von Daten aus mehreren Tabellen](https://docs.microsoft.com/azure/data-factory/tutorial-incremental-copy-multiple-tables-portal)
- [Azure Data Factory – Preise](https://azure.microsoft.com/pricing/details/data-factory/data-pipeline/)

## <a name="next-steps"></a>Nächste Schritte

- [Kopieren von Dateien aus mehreren Containern mit Azure Data Factory](solution-template-copy-files-multiple-containers.md)