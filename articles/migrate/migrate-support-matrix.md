---
title: Unterstützungsmatrix für Azure Migrate
description: Enthält eine Zusammenfassung der Unterstützungseinstellungen und Einschränkungen für den Azure Migrate-Dienst.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 10/16/2019
ms.author: raynew
ms.openlocfilehash: 18032250bc5c321d638ad46204738f49f1a0c744
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73480143"
---
# <a name="azure-migrate-support-matrix"></a>Unterstützungsmatrix für Azure Migrate

Mit dem [Azure Migrate-Dienst](migrate-overview.md) können Sie Computer bewerten und in die Microsoft Azure-Cloud migrieren. Dieser Artikel enthält eine Zusammenfassung der allgemeinen Supporteinstellungen und Einschränkungen für Azure Migrate-Szenarien und -Bereitstellungen.


## <a name="azure-migrate-versions"></a>Azure Migrate-Versionen

Es sind zwei Versionen des Azure Migrate-Diensts verfügbar:

- **Aktuelle Version**: Verwenden Sie diese Version, um neue Azure Migrate-Projekte zu erstellen, lokale Bewertungen durchzuführen und Bewertungen und Migrationen zu orchestrieren. [Weitere Informationen](whats-new.md#release-version-july-2019)
- **Vorherige Version**: Kunden, die die Vorgängerversion von Azure Migrate verwendet haben (es wird nur die Bewertung von lokalen VMware-VMs unterstützt), sollten ab sofort die aktuelle Version verwenden. In der vorherigen Version können Sie keine neuen Azure Migrate-Projekte erstellen oder neue Ermittlungen durchführen.

## <a name="supported-assessmentmigration-scenarios"></a>Unterstützte Bewertungs-/Migrationsszenarien

Die Tabelle enthält eine Übersicht über die unterstützten Ermittlungs-, Bewertungs- und Migrationsszenarien.

**Bereitstellung** | **Details** 
--- | --- 
**App-spezifische Ermittlung** | Sie können die Apps, Rollen und Features ermitteln, die auf VMware-VMs ausgeführt werden. Diese Funktion ist zurzeit nur auf die Ermittlung beschränkt. Die Bewertung erfolgt zurzeit auf der Computerebene. Es ist noch keine app-, rollen- oder featurespezifische Bewertung verfügbar. 
**Lokale Bewertung** | Bewerten Sie lokale Workloads und Daten, die auf VMware-VMS und Hyper-V-VMS ausgeführt werden. Bewerten Sie mit Azure Migrate-Serverbewertung und Microsoft Data Migration Assistant (DMA) sowie Tools von Drittanbietern wie Cloudamize, Corent Tech und Turbonomic Server.
**Migration von einem lokalen Standort zu Azure** | Migrieren Sie auf physischen Servern, VMware-VMs, Hyper-V-VMs und cloudbasierten VMs ausgeführte Workloads und Daten zu Azure. Migrieren Sie mit Azure Migrate-Serverbewertung und Azure Database Migration Service (DMS) sowie mit Tools von Drittanbietern wie Carbonite und CorentTech.

Spezifische Toolunterstützung ist im Folgenden zusammengefasst.

**Tool** | **Bewertung/Migration** | **Details**
--- | --- | ---
Azure Migrate-Serverbewertung | Bewertung | Probieren Sie die Serverbewertung für [Hyper-V](tutorial-prepare-hyper-v.md) and [VMware](tutorial-prepare-vmware.md) aus.
Cloudamize | Bewertung | [Weitere Informationen](https://www.cloudamize.com/platform#tab-0)
CorentTech | Bewertung | [Weitere Informationen](https://www.corenttech.com/)
Turbonomic | Bewertung | [Weitere Informationen](https://turbonomic.com/solutions/technologies/azure-cloud/)
Azure Migrate-Servermigration | Migration | Probieren Sie die Servermigration für [Hyper-V](tutorial-migrate-hyper-v.md) and [VMware](tutorial-migrate-vmware.md) aus.
Carbonite | Migration | [Weitere Informationen](https://www.carbonite.com/data-protection-resources/resource/Datasheet/carbonite-migrate-for-microsoft-azure)
CorentTech | Migration | [Weitere Informationen](https://www.corenttech.com/)


## <a name="azure-migrate-projects"></a>Azure Migrate-Projekte

**Unterstützung** | **Details**
--- | ---
Subscription | Ein Abonnement kann mehrere Azure Migrate-Projekte aufweisen.
Azure-Berechtigungen | Sie benötigen Berechtigungen für Mitwirkende oder Eigentümer im Abonnement, um ein Azure Migrate-Projekt zu erstellen.
Virtuelle VMware-Computer  | Bewerten Sie bis zu 35.000 VMware-VMs in einem einzigen Projekt.
Virtuelle Hyper-V-Computer | Bewerten Sie bis zu 35.000 Hyper-V-VMs in einem einzigen Projekt.

Ein Projekt kann im Rahmen der Bewertungseinschränkungen sowohl VMware-VMs als auch Hyper-V-VMs umfassen.

## <a name="supported-geographies"></a>Unterstützte geografischer Regionen

Sie können Azure Migrate-Projekte in verschiedenen geografischen Regionen erstellen. Obwohl Sie Projekte nur in diesen geografischen Regionen erstellen können, haben Sie die Möglichkeit, Computer für andere Zielstandorte zu bewerten und dorthin zu migrieren. Die Projektgeografie wird nur zum Speichern der ermittelten Metadaten verwendet.

**Geografie** | **Speicherort der Metadaten**
--- | ---
Azure Government | US Government, Virginia
Asien-Pazifik | „Asien, Osten“ und „Asien, Südosten“
Australien | „Australien, Osten“ oder „Australien, Südosten“
Brasilien | Brasilien Süd
Kanada | „Kanada, Mitte“ oder „Kanada, Osten“
Europa | „Europa, Norden“ oder „Europa, Westen“
Frankreich | Frankreich, Mitte
Indien | „Indien, Mitte“ oder „Indien, Süden“
Japan |  „Japan, Osten“ oder „Japan, Westen“
Korea | „Südkorea, Mitte“ oder „Südkorea, Süden“
Vereinigtes Königreich | „Vereinigtes Königreich, Süden“ oder „Vereinigtes Königreich, Westen“
USA | „USA, Mitte“ oder „USA, Westen 2“


 > [!NOTE]
 > Der Support für Azure Government ist derzeit nur für die [ältere Version](https://docs.microsoft.com/azure/migrate/migrate-services-overview#azure-migrate-versions) von Azure Migrate verfügbar.



## <a name="vmware-assessment-and-migration"></a>VMware-Bewertung und -Migration

[Überprüfen](migrate-support-matrix-vmware.md) Sie die Azure Migrate-Serverbewertung und Servermigrations-Unterstützungsmatrix für VMware-VMS.

## <a name="hyper-v-assessment-and-migration"></a>Hyper-V-Bewertung und -Migration

[Überprüfen](migrate-support-matrix-hyper-v.md) Sie die Azure Migrate-Serverbewertung und Servermigrations-Unterstützungsmatrix für Hyper-V-VMS.


## <a name="next-steps"></a>Nächste Schritte

- [Bewerten von virtuellen VMware-Computern](tutorial-assess-vmware.md) für die Migration.
- [Bewerten von virtuellen Hyper-V-Computern](tutorial-assess-hyper-v.md) für die Migration.

