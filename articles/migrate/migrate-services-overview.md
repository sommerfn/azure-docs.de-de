---
title: Informationen zu Azure Migrate | Microsoft-Dokumentation
description: Enthält eine Übersicht über den Azure Migrate-Dienst.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: overview
ms.date: 11/05/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 8b1e716e482c49ceba0c600aaba30fe276bdb74d
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73748009"
---
# <a name="about-azure-migrate"></a>Informationen zu Azure Migrate

Dieser Artikel enthält eine kurze Übersicht über Azure Migrate.

Mit Azure Migrate können Sie Ihr Unternehmen von der lokalen Umgebung zu Azure migrieren. Azure Migrate bietet einen zentralisierten Hub zum Nachverfolgen der Ermittlung, Bewertung und Migration lokaler Infrastruktur, Anwendungen und Daten zu Azure.  Azure Migrate bietet:

- **Vereinheitlichte Migrationsplattform**: Ein einzelnes Portal zum Starten, Ausführen und Nachverfolgen Ihres Migrationswegs zu Azure.
- **Verfügbare Tools**: Der Hub bietet Azure Migrate-Tools für die Bewertung und Migration und arbeitet mittels Integration mit anderen Azure-Diensten sowie mit Tools und Angeboten unabhängiger Softwarehersteller (Independent Software Vendors, ISVs) zusammen.
- **Workloads**: Azure Migrate ermöglicht die Bewertung und Migration folgender Komponenten:
    - **Server**: Verwenden Sie die Azure Migrate-Serverbewertung, die Azure Migrate-Servermigration und andere Tools, um Server zu bewerten und zu virtuellen Azure-Computern zu migrieren.
    - **Datenbanken**: Nutzen Sie Microsoft- und ISV-Tools für die Bewertung sowie für die Migration lokaler Datenbanken zu einer Azure SQL-Datenbank-Instanz oder einer verwalteten Azure SQL-Instanz.
    - **Webanwendungen**: Verwenden Sie den Assistenten von Azure App Service, um lokale Webanwendungen für die Migration zu Azure App Service zu bewerten.
    - **Virtuelle Desktops**: Verwenden Sie ISV-Tools, um die lokale virtuelle Desktop Infrastruktur (VDI) zu bewerten und zu Windows Virtual Desktop in Azure zu migrieren.
    - **Daten**: Verwenden Sie die Azure Data Box-Produktfamilie, um schnell und kostengünstig große Datenmengen zu Azure zu migrieren.

## <a name="azure-migrate-versions"></a>Azure Migrate-Versionen

Derzeit sind zwei Versionen des Azure Migrate-Diensts verfügbar:

- **Aktuelle Version**: Verwenden Sie diese Version, um Azure Migrate-Projekte zu erstellen, lokale Computer zu ermitteln und Bewertungen und Migrationen zu orchestrieren. [Erfahren Sie mehr](whats-new.md) über die Neuerungen in dieser Version.
- **Vorherige Version**: Wenn Sie die Vorgängerversion von Azure Migrate verwendet haben (es wird nur die Bewertung von lokalen VMware-VMs unterstützt), sollten Sie ab sofort die aktuelle Version verwenden. Azure Migrate-Projekte können nicht mehr mit der vorherigen Version erstellt werden, und es wird empfohlen, keine neuen Ermittlungen auszuführen. Um auf vorhandene Projekte zuzugreifen, wählen Sie im Azure-Portal **Alle Dienste** aus, und suchen Sie dann nach **Azure Migrate**. Im Azure Migrate-Dashboard sind eine Benachrichtigung und ein Link für den Zugriff auf alte Azure Migrate-Projekte vorhanden.



## <a name="isv-integration"></a>ISV-Integration

Zusätzlich zu den nativen Azure-Tools kann Azure Migrate in eine Reihe von ISV-Angeboten integriert werden. 

**ISV** | **Feature**
--- | ---
[Carbonite](https://www.carbonite.com/globalassets/files/datasheets/carb-migrate4azure-microsoft-ds.pdf) | Migrieren von Servern
[Cloudamize](https://www.cloudamize.com/platform) | Bewerten von Servern
[Corent Technology](https://www.corenttech.com/AzureMigrate/) | Bewerten und Migrieren von Servern
[Device42](https://docs.device42.com/) | Bewerten von Servern
[Lakeside](https://go.microsoft.com/fwlink/?linkid=2104908) | Bewerten der VDI
[RackWare](https://go.microsoft.com/fwlink/?linkid=2102735) | Migrieren von Servern
[Turbonomic](https://learn.turbonomic.com/azure-migrate-portal-free-trial) | Bewerten von Servern
[UnifyCloud](https://www.cloudatlasinc.com/cloudrecon/) | Bewerten von Servern und Datenbanken

## <a name="azure-tool-integration"></a>Azure-Toolintegration

Die folgende Tabelle enthält eine Übersicht über weitere Tools, die in Azure Migrate integriert sind:

**Tool** | **Details**
--- | ---
Von der Azure Migrate-Serverbewertung | Bewerten von Servern
Von der Azure Servermigration | Migrieren von Servern
Datenbankmigrations-Assistent (DMA) | Bewerten von Datenbanken
Database Migration Service (DMS) | Migrieren von Datenbanken
Movere | Bewerten von Servern
Migrations-Assistent für Web-Apps | Bewerten und Migrieren von Web-Apps



### <a name="selecting-a-tool"></a>Auswählen eines Tools

Ermitteln Sie das benötigte Tool, und fügen Sie es einem Azure Migrate-Projekt hinzu.

- Falls Sie ein ISV-Tool oder Movere hinzufügen, gehen Sie wie folgt vor:
    - Beziehen Sie zunächst eine Lizenz, oder registrieren Sie sich für eine kostenlose Testversion (gemäß der Toolrichtlinie). Die Lizenzierung für ISV-Tools erfolgt gemäß dem Lizenzierungsmodell des ISV oder Tools.
    - In jedem Tool ist eine Option zum Herstellen einer Verbindung mit Azure Migrate vorhanden. Befolgen Sie die Anweisungen des Tools und seine Dokumentation, um das Tool mit Azure Migrate zu verbinden.
- Sie verfolgen Ihre Migration innerhalb des Azure Migrate-Projekts zentral über Azure und andere Tools hinweg nach.



## <a name="azure-migrate-server-assessment-tool"></a>Azure Migrate-Serverbewertungstool

Von der Azure Das Serverbewertungstool ermittelt und bewertet lokale VMware-VMs, Hyper-V-VMs und physische Server für die Migration zu Azure. Sie hilft Ihnen, Folgendes zu identifizieren:

- **Azure-Bereitschaft:** Bewertet, ob lokale Computer bereit für die Migration zu Azure sind.
- **Azure-Dimensionierung:** Die geschätzte Größe von Azure-VMs nach der Migration.
- **Azure-Kostenschätzung:** Geschätzte Kosten für die Ausführung lokaler Server in Azure.
- **Abhängigkeitsvisualisierung:** Serverübergreifende Abhängigkeiten (bei aktivierter Abhängigkeitsvisualisierung) und optimale Möglichkeiten, abhängige Server zu Azure zu verschieben.

Die Serverbewertung verwendet eine schlanke Appliance, die Sie lokal bereitstellen und bei der Serverbewertung registrieren.

- Die Appliance ermittelt lokale Computer.
- Sie stellt eine Verbindung mit der Serverbewertung her und sendet kontinuierlich Computermetadaten und Leistungsdaten an Azure Migrate.
- Die Applianceermittlung erfolgt ohne Agent. Auf ermittelten Computern muss nichts installiert werden.
- Nach der Ermittlung erfassen Sie ermittelte Computer in Gruppen. Normalerweise fassen Sie Computer, die Sie zusammen migrieren möchten, in Gruppen zusammen.
- Sie erstellen eine Bewertung für eine Gruppe. Anschließend analysieren Sie die Bewertung, um die Migrationsstrategie zu ermitteln.

## <a name="azure-migrate-server-migration-tool"></a>Tool für die Azure Migrate-Servermigration

Von der Azure Mit dem Servermigrationstool können Sie lokale VMware-VMs, Hyper-V-VMs, physische Server, andere virtualisierte Computer und VMs der öffentlichen Cloud zu Azure migrieren. Computer können nach der Bewertung oder ohne eine Bewertung migriert werden.


## <a name="database-migration-assistant"></a>Datenbankmigrations-Assistent

Azure Migrate ist in den Datenmigrations-Assistenten (DMA) von Microsoft integriert, um lokale SQL Server-Datenbanken für die Migration zu Azure SQL-Datenbank, verwalteten Azure SQL-Instanzen oder Azure-VMs, auf denen SQL Server ausgeführt wird, zu bewerten. Der DMA stellt Informationen zu möglichen Blockierungsproblemen bei der Migration zur Verfügung. Er identifiziert nicht unterstützte Features sowie neue Features, von denen Sie nach der Migration profitieren können, und hilft Ihnen bei der Identifizierung des richtigen Pfads für die Datenbankmigration. [Weitere Informationen](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017)

## <a name="database-migration-service"></a>Database Migration Service

Azure Migrate ist in Azure Database Migration Service (DMS) integriert, um lokale Datenbanken zu Azure zu migrieren. Verwenden Sie DMS zum Migrieren von lokalen Datenbanken zu Azure-VMs, auf denen SQL, Azure SQL DB und verwaltete Azure SQL-Instanzen ausgeführt werden. [Weitere Informationen](https://docs.microsoft.com/azure/dms/dms-overview)

## <a name="movere"></a>Movere

 
Bei Movere handelt es sich um eine SaaS-Plattform, die durch die präzise Darstellung vollständiger IT-Umgebungen innerhalb eines Tages zur Verbesserung der Business Intelligence beiträgt. Organisationen wachsen, verändern sich und werden digital optimiert. Mit dieser Lösung können Unternehmen darauf vertrauen, dass sie über die nötige Transparenz und Kontrolle für Ihre Umgebungen verfügen – unabhängig von Plattform, Anwendung oder Geografie. Movere wurde von Microsoft [übernommen](https://azure.microsoft.com/blog/microsoft-acquires-movere-to-help-customers-unlock-cloud-innovation-with-seamless-migration-tools/) und wird nicht mehr als eigenständiges Angebot verkauft.  Movere steht im Rahmen der Microsoft-Programme für Lösungsbewertungen und Wirtschaftlichkeit in der Cloud zur Verfügung. Weitere Informationen zu Movere finden Sie [hier](https://www.movere.io). Sollten Sie Fragen haben, senden Sie diese entweder an movereq@microsoft.com, oder wenden Sie sich an Ihren Microsoft-Vertreter.

Informieren Sie sich auch über Azure Migrate, unseren integrierten Migrationsdienst. Azure Migrate bietet einen zentralen Hub, um die Migration zur Cloud zu vereinfachen. Der Hub zeichnet sich durch umfassende Unterstützung verschiedenster Workloads aus. Hierzu zählen unter anderem physische und virtuelle Server sowie Datenbanken und Anwendungen. Die End-to-End-Transparenz ermöglicht eine mühelose Nachverfolgung des Status während der Ermittlung, Bewertung und Migration. Dank der integrierten Azure- und Partner-ISV-Tools bietet Azure Migrate auch ein breites Spektrum an Features wie etwa die Ermittlung virtueller und physischer Server, eine leistungsbasierte Größenanpassung, eine Kostenplanung, importbasierte Bewertungen sowie eine Anwendungsabhängigkeitsanalyse ohne Agent. Sollten Sie professionelle Hilfe bei Ihren ersten Schritten benötigen: Microsoft verfügt über kompetente [Azure Expert Managed Service Provider](https://azure.microsoft.com/partners), die Sie auf Ihrem Weg begleiten. Besuchen Sie die [Azure Migrate-Website](https://azure.microsoft.com/services/azure-migrate/). 
 

## <a name="web-app-migration-assistant"></a>Migrations-Assistent für Web-Apps

Azure Migrate ist in den Migrations-Assistenten von Azure App Service integriert. Über den Azure Migrate-Hub können Sie lokale Web-Apps mit dem Assistenten wie folgt bewerten und zu Azure migrieren:

- **Onlinebewertung von Web-Apps**: Verwenden Sie den Migrations-Assistenten von Azure App Service, um lokale Websites für die Migration zu Azure App Service zu bewerten.
- **Migrieren von Web-Apps**: Migrieren Sie .NET- und PHP-Web-Apps mithilfe des Migrations-Assistenten von Azure App Service zu Azure.

[Weitere Informationen](https://appmigration.microsoft.com/) zum Assistenten.



## <a name="offline-data-migration"></a>Offlinedatenmigration

Mit Azure Data Box können Sie große Datenmengen offline nach Azure verschieben. [Weitere Informationen](https://docs.microsoft.com/azure/databox/)

## <a name="next-steps"></a>Nächste Schritte

- Testen Sie unsere Tutorials, um [VMware-VMs](tutorial-assess-vmware.md) und [Hyper-V-VMs](tutorial-assess-hyper-v.md) zu bewerten.
- Weitere Informationen zu den Preisen von Azure Migrate finden Sie [hier](https://azure.microsoft.com/pricing/details/azure-migrate/).
- Sehen Sie sich die [häufig gestellten Fragen](resources-faq.md) zu Azure Migrate an.
