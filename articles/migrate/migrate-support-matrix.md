---
title: Unterstützungsmatrix für Azure Migrate
description: Enthält eine Zusammenfassung der Unterstützungseinstellungen und Einschränkungen für den Azure Migrate-Dienst.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 02/25/2019
ms.author: raynew
ms.openlocfilehash: 49bd193303255cdf7d18fd5da9dec8d84c50a829
ms.sourcegitcommit: 3f78a6ffee0b83788d554959db7efc5d00130376
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/26/2019
ms.locfileid: "70019189"
---
# <a name="azure-migrate-support-matrix"></a>Unterstützungsmatrix für Azure Migrate

Mit dem [Azure Migrate-Dienst](migrate-overview.md) können Sie Computer bewerten und in die Microsoft Azure-Cloud migrieren. Dieser Artikel enthält eine Zusammenfassung der allgemeinen Supporteinstellungen und Einschränkungen für Azure Migrate-Szenarien und -Bereitstellungen.


## <a name="azure-migrate-versions"></a>Azure Migrate-Versionen

Es sind zwei Versionen des Azure Migrate-Diensts verfügbar:

- **Aktuelle Version**: Verwenden Sie diese Version, um neue Azure Migrate-Projekte zu erstellen, lokale Bewertungen durchzuführen und Bewertungen und Migrationen zu orchestrieren. [Weitere Informationen](whats-new.md#azure-migrate-new-version)
- **Vorherige Version**: Kunden, die die Vorgängerversion von Azure Migrate verwendet haben (es wird nur die Bewertung von lokalen VMware-VMs unterstützt), sollten ab sofort die aktuelle Version verwenden. In der vorherigen Version können Sie keine neuen Azure Migrate-Projekte erstellen oder neue Ermittlungen durchführen.

## <a name="supported-migration-scenarios"></a>Unterstützte Migrationsszenarien

Die Tabelle fasst die unterstützten Migrationsszenarien zusammen.

**Bereitstellung** | **Details*** 
--- | --- 
**Lokale Bewertung** | Bewerten Sie lokale Workloads und Daten, die auf VMware-VMS und Hyper-V-VMS ausgeführt werden. Bewerten Sie mit Azure Migrate-Serverbewertung und Microsoft Data Migration Assistant (DMA) sowie Tools von Drittanbietern wie Cloudamize, Corent Tech und Turbonomic Server.
**Migration von einem lokalen Standort zu Azure** | Migrieren Sie auf physischen Servern, VMware-VMs, Hyper-V-VMs und AWS/GCP-Instanzen ausgeführte Workloads und Daten zu Azure. Migrieren Sie mit Azure Migrate-Serverbewertung und Azure Database Migration Service (DMS) sowie mit Tools von Drittanbietern wie Carbonite und CorentTech.

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
Subscription | Sie können über ein einzelnes Azure Migrate-Projekt in einem Abonnement verfügen.
Azure-Berechtigungen | Sie benötigen Berechtigungen für Mitwirkende oder Eigentümer im Abonnement, um ein Azure Migrate-Projekt zu erstellen.
Virtuelle VMware-Computer  | Bewerten Sie bis zu 35.000 VMware-VMs in einem einzigen Projekt.
Virtuelle Hyper-V-Computer | Bewerten Sie bis zu 10.000 Hyper-V-VMs in einem einzigen Projekt.

Ein Projekt kann im Rahmen der Bewertungseinschränkungen sowohl VMware-VMs als auch Hyper-V-VMs umfassen.


## <a name="vmware-assessment-and-migration"></a>VMware-Bewertung und -Migration

[Überprüfen](migrate-support-matrix-vmware.md) Sie die Azure Migrate-Serverbewertung und Servermigrations-Unterstützungsmatrix für VMware-VMS.

## <a name="hyper-v-assessment-and-migration"></a>Hyper-V-Bewertung und -Migration

[Überprüfen](migrate-support-matrix-hyper-v.md) Sie die Azure Migrate-Serverbewertung und Servermigrations-Unterstützungsmatrix für Hyper-V-VMS.


## <a name="next-steps"></a>Nächste Schritte

- [Bewerten von virtuellen VMware-Computern](tutorial-assess-vmware.md) für die Migration.
- [Bewerten von virtuellen Hyper-V-Computern](tutorial-assess-hyper-v.md) für die Migration.

