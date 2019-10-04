---
title: Informationen zu Azure Migrate | Microsoft-Dokumentation
description: Enthält eine Übersicht über den Azure Migrate-Dienst.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: overview
ms.date: 07/11/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 03918976935f9071ba2a7951e29195118943a8ef
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/08/2019
ms.locfileid: "68845833"
---
# <a name="about-azure-migrate"></a>Informationen zu Azure Migrate

Dieser Artikel enthält eine kurze Übersicht über Azure Migrate.

Azure Migrate hilft Ihnen bei der Migration zu Azure. Azure Migrate bietet einen zentralisierten Hub zum Nachverfolgen der Ermittlung, Bewertung und Migration lokaler Infrastruktur, Anwendungen und Daten zu Azure. Der Hub stellt Azure-Tools für die Bewertung und Migration sowie unabhängige Drittanbietertools bereit. Die Lösung umfasst Folgendes:

- **Vereinheitlichte Migrationsplattform**: Verwenden Sie ein einzelnes Portal zum Starten, Ausführen und Nachverfolgen Ihres Migrationswegs zu Azure.
- **Verfügbare Tools**: Azure Migrate bietet native Tools und lässt sich in andere Azure-Dienste sowie ISV-Tools integrieren. Wählen Sie basierend auf den Anforderungen Ihrer Organisation die richtigen Bewertungs-und Migrationstools aus. 
- **Azure Migrate-Serverbewertung**: Verwenden Sie das Serverbewertungstool, um lokale VMware-VMs und Hyper-V-VMs für die Migration zu Azure zu bewerten.
- **Azure Migrate-Servermigration**: Verwenden Sie das Servermigrationstool, um lokale VMware-VMs, Hyper-V-VMs, Cloud-VMs und physische Server zu Azure zu migrieren.
- **Azure Migrate-Datenbankbewertung**: Bewerten Sie die lokalen Ressourcen für die Migration zu Azure.
- **Azure Migrate-Datenbankmigration**: Migrieren Sie lokale Datenbanken zu Azure.


## <a name="azure-migrate-versions"></a>Azure Migrate-Versionen

Es sind zwei Versionen des Azure Migrate-Diensts verfügbar:

- **Aktuelle Version**: Verwenden Sie diese Version, um Azure Migrate-Projekte zu erstellen, lokale Computer zu ermitteln und Bewertungen und Migrationen zu orchestrieren. [Erfahren Sie mehr](whats-new.md) über die Neuerungen in dieser Version.
- **Vorherige Version**: Wenn Sie die Vorgängerversion von Azure Migrate verwendet haben (es wird nur die Bewertung von lokalen VMware-VMs unterstützt), sollten Sie ab sofort die aktuelle Version verwenden. Azure Migrate-Projekte können nicht mehr mit der vorherigen Version erstellt werden, und es wird empfohlen, keine neuen Ermittlungen auszuführen. Um auf vorhandene Projekte zuzugreifen, wählen Sie im Azure-Portal **Alle Dienste** aus, und suchen Sie dann nach **Azure Migrate**. Im Azure Migrate-Dashboard sind eine Benachrichtigung und ein Link für den Zugriff auf alte Azure Migrate-Projekte vorhanden.

## <a name="isv-integration"></a>ISV-Integration

Zusätzlich zu den nativen Azure-Tools kann Azure Migrate in eine Reihe von ISV-Angeboten integriert werden. Sie identifizieren das Tool, das Sie benötigen, und fügen es einem Azure Migrate-Projekt hinzu. Sie können Ihren Migrationsweg aus dem Azure Migrate-Projekts zentral über Azure-und ISV-Tools nachverfolgen.

**ISV** | **Feature**
--- | ---
[Cloudamize](https://www.cloudamize.com/platform) | Bewerten
[Device42](https://docs.device42.com/) | Bewerten
[Turbonomic](https://learn.turbonomic.com/azure-migrate-portal-free-trial) | Bewerten
[UnifyCloud](https://www.cloudatlasinc.com/cloudrecon/) | Bewerten
[Corent Technology](https://www.corenttech.com/AzureMigrate/) | Bewerten und Migrieren
[Carbonite](https://www.carbonite.com/globalassets/files/datasheets/carb-migrate4azure-microsoft-ds.pdf) | Migrieren

### <a name="selecting-an-isv-tool"></a>Auswählen eines ISV-Tools

Nachdem Sie einem Azure Migrate-Projekt ein ISV-Tool hinzugefügt haben, können Sie mit dem Tool beginnen, indem Sie eine Lizenz abrufen oder sich für eine kostenlose Testversion in Übereinstimmung mit der ISV-Richtlinie registrieren. In jedem Tool ist eine Option zum Herstellen einer Verbindung mit Azure Migrate vorhanden. Befolgen Sie die Anweisungen des Tools und seine Dokumentation, um das Tool mit Azure Migrate zu verbinden. Die Lizenzierung für ISV-Tools entspricht dem ISV-Lizenzierungsmodell.

## <a name="azure-migrate-server-assessment"></a>Azure Migrate-Serverbewertung

Die Azure Migrate-Serverbewertung ermittelt und bewertet lokale VMware-VMs und Hyper-V-VMs für die Migration zu Azure. Sie hilft Ihnen, Folgendes zu identifizieren:

- **Azure-Bereitschaft:** Bewertet, ob lokale Computer bereit für die Migration zu Azure sind.
- **Azure-Dimensionierung:** Schätzt die Größe von Azure-VMs nach der Migration.
- **Azure-Kostenschätzung:** Schätzt die Kosten für die Ausführung lokaler Server in Azure.
- **Abhängigkeitsvisualisierung:** Identifiziert serverübergreifende Abhängigkeiten und die beste Methode, um abhängige Server in Azure zu verschieben. 

Die Serverbewertung verwendet eine schlanke Appliance, die Sie lokal bereitstellen und bei der Serverbewertung registrieren.

- Die Appliance ermittelt lokale Computer, stellt eine Verbindung mit der Serverbewertung her und sendet kontinuierlich Metadaten und leistungsbezogene Daten an Azure Migrate.
- Die Ermittlung erfolgt ohne Agent. Auf ermittelten virtuellen Computern muss nichts installiert werden.
- Nachdem Computer ermittelt wurden, werden sie in Gruppen zusammengefasst, die in der Regel aus VMs bestehen, die Sie zusammen migrieren möchten.
- Sie erstellen eine Bewertung für eine Gruppe. Anschließend können Sie die Bewertung analysieren, um die Migrationsstrategie zu ermitteln.

## <a name="azure-migrate-server-migration"></a>Azure Migrate-Servermigration

Mit der Azure Migrate-Servermigration können Sie lokale VMware-VMs, Hyper-V-VMs, physische Server, andere virtualisierte Computer und VMs der öffentlichen Cloud zu Azure migrieren. Computer können nach der Bewertung oder ohne eine Bewertung migriert werden. 

## <a name="azure-migrate-database-assessment"></a>Azure Migrate-Datenbankbewertung

Azure Migrate ist in den Datenmigrations-Assistenten (DMA) integriert, um lokale SQL Server-Datenbanken für die Migration zu Azure SQL-Datenbank, in die verwaltete Azure SQL-Instanz oder zu Azure-VMs zu bewerten, auf denen SQL Server ausgeführt wird. Der DMA stellt Informationen zu möglichen Blockierungsproblemen bei der Migration zur Verfügung. Er identifiziert nicht unterstützte Features sowie neue Features, von denen Sie nach der Migration profitieren können, und hilft Ihnen bei der Identifizierung des richtigen Pfads für die Datenbankmigration. [Weitere Informationen](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017)


## <a name="azure-migrate-database-migration"></a>Azure Migrate-Datenbankmigration

Azure Migrate ist in Azure Database Migration Service (DMS) integriert, um lokale Datenbanken zu Azure zu migrieren. Verwenden Sie DMS zum Migrieren von lokalen Datenbanken zu Azure-VMs, auf denen SQL, Azure SQL DB und verwaltete Azure SQL-Instanzen ausgeführt werden. [Weitere Informationen](https://docs.microsoft.com/azure/dms/dms-overview)

## <a name="web-app-assessment-and-migration"></a>Web-App-Bewertung und -Migration

Über den Azure Migrate-Hub können Sie lokale Web-Apps bewerten und zu Azure migrieren.

- **Onlinebewertung von Web-Apps**: Verwenden Sie den Migrations-Assistenten von Azure App Service, um lokale Websites für die Migration zu Azure App Service zu bewerten.
- **Migrieren von Web-Apps**: Migrieren Sie .NET- und PHP-Web-Apps mithilfe des Migrations-Assistenten von Azure App Service zu Azure.

[Weitere Informationen.](https://appmigration.microsoft.com/)

## <a name="offline-data-migration"></a>Offlinedatenmigration

Mit der Data Box-Offlineproduktfamilie können Sie große Datenmengen nach Azure verschieben. [Weitere Informationen](https://docs.microsoft.com/azure/databox/)

## <a name="next-steps"></a>Nächste Schritte

- Testen Sie unsere Tutorials, um [VMware-VMs](tutorial-assess-vmware.md) und [Hyper-V-VMs](tutorial-assess-hyper-v.md) zu bewerten.
- Weitere Informationen zu den Preisen von Azure Migrate finden Sie [hier](https://azure.microsoft.com/pricing/details/azure-migrate/).
- Sehen Sie sich die [häufig gestellten Fragen](resources-faq.md) zu Azure Migrate an.
