---
title: Neuerungen in Azure Migrate | Microsoft-Dokumentation
description: Enthält eine Übersicht über den Azure Migrate-Dienst.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: overview
ms.date: 11/06/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: af9305aafc3a77df9d7c4cffa65f6c61c53ad8a4
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73715394"
---
# <a name="whats-new-in-azure-migrate"></a>Neuerungen in Azure Migrate

[Azure Migrate](migrate-services-overview.md) unterstützt Sie beim Ermitteln, Bewerten und Migrieren von lokalen Servern, Apps und Daten in die Microsoft Azure Cloud. In diesem Artikel werden die neuen Features in Azure Migrate zusammengefasst.



## <a name="update-november-2019"></a>Update (November 2019)



Azure Migrate wurden einige neue Features hinzugefügt:

> [!NOTE]
> Falls Sie einige dieser Features noch nicht im Azure Migrate-Portal sehen, bitten wir Sie um etwas Geduld. Sie werden voraussichtlich im Laufe der nächsten Woche verfügbar.

- **Bewertung physischer Server**: Zusätzlich zur bereits vorhandenen Migration physischer Server wird jetzt auch die Bewertung von lokalen physischen Servern unterstützt.
- **Importbasierte Bewertung**. Die Bewertung von Computern, für die Metadaten und Leistungsdaten aus einer CSV-Datei verwendet werden, wird jetzt unterstützt.
- **Anwendungsermittlung**: Azure Migrate unterstützt über die Azure Migrate-Appliance jetzt die Ermittlung von Apps, Rollen und Features auf Anwendungsebene. Dies wird derzeit nur für VMware-VMs unterstützt und ist auf die Ermittlung beschränkt (die Bewertung wird derzeit nicht unterstützt). [Weitere Informationen](how-to-discover-applications.md)
- **Visualisierung von Abhängigkeiten ohne Agent**: Es ist nicht mehr erforderlich, Agents explizit für die Abhängigkeitsvisualisierung zu installieren. Sowohl die Vorgehensweise ohne als auch mit Agent wird jetzt unterstützt.
- **Virtueller Desktop**: Verwenden Sie ISV-Tools, um die lokale Virtual Desktop Infrastructure (VDI) zu bewerten und zu Windows Virtual Desktop in Azure zu migrieren.
- **Web-App**: Der Azure App Service-Migrations-Assistent, der für die Bewertung und Migration von Web-Apps verwendet wird, ist jetzt in Azure Migrate integriert.

Azure Migrate wurden neue Tools zur Bewertung und Migration hinzugefügt:

- **Rackware:** für die Cloudmigration
- **Movere:** für die Bewertung

[Hier](migrate-services-overview.md) finden Sie weitere Informationen zur Verwendung von Tools und ISV-Angeboten für die Bewertung und Migration in Azure Migrate.

## <a name="release-version-july-2019"></a>Releaseversion (Juli 2019)

Die aktuelle Version von Azure Migrate wurde im Juli 2019 veröffentlicht. 

- **Aktuelle Version**: Verwenden Sie diese Version, um Azure Migrate-Projekte zu erstellen, lokale Computer zu ermitteln und Bewertungen und Migrationen zu orchestrieren. 
- **Vorherige Version**: Kunden, die die Vorgängerversion von Azure Migrate verwendet haben (es wird nur die Bewertung von lokalen VMware-VMs unterstützt), sollten ab sofort die aktuelle Version verwenden. In der vorherigen Version können Sie keine neuen Azure Migrate Projekte mehr erstellen oder neue Ermittlungen durchführen. Sie können weiterhin auf vorhandene Projekte zugreifen. Wählen Sie zu diesem Zweck im Azure-Portal die Option **Alle Dienste** aus, und suchen Sie dann nach **Azure Migrate**. In den Azure Migrate-Benachrichtigungen finden Sie einen Link für den Zugriff auf alte Azure Migrate-Projekte.


### <a name="azure-migrate-features"></a>Azure Migrate-Funktionen

Die aktuelle Version von Azure Migrate enthält eine Reihe neuer Features:


- **Vereinheitlichte Migrationsplattform**: Azure Migrate bietet jetzt ein einziges Portal zum Zentralisieren, Verwalten und Nachverfolgen Ihres Migrationswegs zu Azure mit einem verbesserten Bereitstellungsfluss und einer optimierten Portalbenutzeroberfläche.
- **Bewertungs- und Migrationstools**: Azure Migrate bietet native Tools und lässt sich in andere Azure-Dienste sowie ISV-Tools (Independent Software Vendor) integrieren. [Erfahren Sie mehr](migrate-services-overview.md#isv-integration) über die ISV-Integration.
- **Azure Migrate-Bewertung**: Mit dem Azure Migrate-Serverbewertungstool können Sie lokale VMware-VMs und Hyper-V-VMs für die Migration zu Azure ermitteln und bewerten. Sie können die Migration auch mithilfe anderer Azure-Dienste und ISV-Tools bewerten.
- **Azure Migrate-Migration**: Mit dem Azure Migrate-Servermigrationstool können Sie lokale VMware-VMs, Hyper-V-VMs, physische Server, andere virtualisierte Server und VMs der privaten/öffentlichen Cloud zu Azure migrieren. Außerdem können Sie die Migration zu Azure mithilfe von ISV-Tools ausführen.
- **Azure Migrate-Appliance**: Azure Migrate stellt eine schlanke Appliance für die Ermittlung und Bewertung von lokalen VMware-VMs und Hyper-V-VMs bereit.
    - Diese Appliance wird von der Azure Migrate-Serverbewertung und Azure Migrate-Servermigration für die Migration ohne Agent verwendet.
    - Die Appliance ermittelt kontinuierlich Servermetadaten und Leistungsdaten für die Zwecke der Bewertung und Migration.  
- **VMware-VM-Migration**:  Azure Migrate-Servermigration bietet eine Reihe von Methoden zum Migrieren von lokalen VMware-VMs zu Azure.  Eine Migration ohne Agent mithilfe der Azure Migrate-Appliance und eine agentbasierte Migration, bei der eine Replikationsappliance verwendet und auf jedem virtuellen Computer ein Agent bereitgestellt wird, den Sie migrieren möchten. [Weitere Informationen](server-migrate-overview.md)
 - **Datenbankbewertung und -migration**: Aus Azure Migrate können Sie lokale Datenbanken für die Migration zu Azure mithilfe des Migrations-Assistenten der Azure-Datenbank bewerten. Sie können Datenbanken mit dem Azure Database Migration Service migrieren.
- **Web-App-Migration**: Sie können Web-Apps mit einer öffentlichen Endpunkt-URL mit Azure App Service bewerten. Für die Migration interner .NET-Apps können Sie den Migrations-Assistenten von App Service herunterladen und ausführen. 
- **Data Box**: Importieren Sie große Mengen von Offlinedaten in Azure mithilfe von Azure Data Box in Azure Migrate.


## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu den Preisen von Azure Migrate finden Sie [hier](https://azure.microsoft.com/pricing/details/azure-migrate/).
- Sehen Sie sich die [häufig gestellten Fragen](resources-faq.md) zu Azure Migrate an.
- Testen Sie unsere Tutorials, um [VMware-VMs](tutorial-assess-vmware.md) und [Hyper-V-VMs](tutorial-assess-hyper-v.md) zu bewerten.
