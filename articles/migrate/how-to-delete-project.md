---
title: Löschen eines Azure Migrate-Projekts
description: Hier wird beschrieben, wie Sie ein Azure Migrate-Projekt erstellt und ein Bewertungs-/Migrationstool hinzufügen.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 10/22/2019
ms.author: raynew
ms.openlocfilehash: 55842d36cddb2a7851ff5bd7002c20e9873158f5
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510251"
---
# <a name="delete-an-azure-migrate-project"></a>Löschen eines Azure Migrate-Projekts

In diesem Artikel wird beschrieben, wie Sie ein [Azure Migrate](migrate-overview.md)-Projekt löschen.


## <a name="before-you-start"></a>Vorbereitung

Vor dem Löschen eines Projekts:

- Wenn Sie ein Projekt löschen, werden das Projekt und die ermittelten Computermetadaten gelöscht.
- Wenn Sie dem Serverbewertungstool für die Abhängigkeitsanalyse einen Log Analytics-Arbeitsbereich angefügt haben, müssen Sie entscheiden, ob Sie den Arbeitsbereich löschen möchten. 
    - Der Arbeitsbereich wird nicht automatisch gelöscht. Er muss manuell gelöscht werden.
    - Überprüfen Sie, wofür ein Arbeitsbereich verwendet wird, bevor Sie ihn löschen. Derselbe Log Analytics-Arbeitsbereich kann für mehrere Szenarien verwendet werden.
    - Bevor Sie das Projekt löschen, finden Sie einen Link zum Arbeitsbereich in **Azure Migrate – Server** > **Azure Migrate – Serverbewertung** unter **OMS-Arbeitsbereich**.
    - Zum Löschen eines Arbeitsbereichs, nachdem ein Projekt gelöscht wurde, suchen Sie den Arbeitsbereich in der relevanten Ressourcengruppe und befolgen Sie [diese Anweisungen](../azure-monitor/platform/delete-workspace.md).


## <a name="delete-a-project"></a>Löschen eines Projekts


1. Öffnen Sie im Azure-Portal die Ressourcengruppe, in der das Projekt erstellt wurde.
2. Wählen Sie auf der Seite für die Ressourcengruppe **Ausgeblendete Typen anzeigen** aus.
3. Wählen Sie das Projekt und die zugehörigen Ressourcen aus, die Sie löschen möchten.
    - Der Ressourcentyp für Azure Migrate-Projekte ist **Microsoft.Migrate/migrateprojects**.
    - Überprüfen Sie im nächsten Abschnitt die Ressourcen, die für die Ermittlung, Bewertung und Migration in einem Azure Migrate-Projekt erstellt wurden.
    - Wenn die Ressourcengruppe nur das Azure Migrate-Projekt enthält, können Sie die gesamte Ressourcengruppe löschen.
    - Wenn Sie ein Projekt aus der früheren Version von Azure Migrate löschen möchten, sind die Schritte identisch. Der Ressourcentyp für diese Projekte ist **Migrationsprojekt**.


## <a name="created-resources"></a>Erstellte Ressourcen

Diese Tabelle enthält eine Übersicht über die Ressourcen, die für die Ermittlung, Bewertung und Migration in einem Azure Migrate-Projekt erstellt werden.

> [!NOTE]
> Löschen Sie den Schlüsseltresor mit Vorsicht, da er Sicherheitsschlüssel enthalten kann.

### <a name="vmwarephysical-server"></a>VMware/physischer Server

**Ressource** | **Typ**
--- | ---
"Appliancename"kv | Schlüsseltresor
"Appliancename"site | Microsoft.OffAzure/VMwareSites
"ProjectName" | Microsoft.Migrate/migrateprojects
"ProjectName"project | Microsoft.Migrate/assessmentProjects
"ProjectName"rsvault | Recovery Services-Tresor
"ProjectName"-MigrateVault-* | Recovery Services-Tresor
migrateappligwsa* | Speicherkonto
migrateapplilsa* | Speicherkonto
migrateapplicsa* | Speicherkonto
migrateapplikv* | Schlüsseltresor
migrateapplisbns16041 | Service Bus- Namespace

### <a name="hyper-v-vm"></a>Virtueller Hyper-V-Computer 

**Ressource** | **Typ**
--- | ---
"ProjectName" | Microsoft.Migrate/migrateprojects
"ProjectName"project | Microsoft.Migrate/assessmentProjects
HyperV*kv | Schlüsseltresor
HyperV*Site | Microsoft.OffAzure/HyperVSites
"ProjectName"-MigrateVault-* | Recovery Services-Tresor


## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie zusätzliche Tools für [Bewertung](how-to-assess.md) und [Migration](how-to-migrate.md) hinzufügen. 
