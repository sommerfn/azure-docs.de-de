---
title: Hinzufügen von Bewertungstools in Azure Migrate | Microsoft-Dokumentation
description: Hier wird beschrieben, wie Sie im Azure Migrate-Hub Bewertungstools hinzufügen.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 07/04/2019
ms.author: raynew
ms.openlocfilehash: d176e6276d69cd3465aa4943efa86ea1e6b0736d
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67810173"
---
# <a name="add-assessment-tools"></a>Hinzufügen von Bewertungstools

In diesem Artikel wird beschrieben, wie Sie in [Azure Migrate](migrate-overview.md) Bewertungstools hinzufügen.

Azure Migrate stellt einen Hub mit Tools für die Bewertung und Migration zu Azure bereit. Hier finden Sie native Tools, von anderen Azure-Diensten bereitgestellte Tools sowie ISV-Angebote (Independent Software Vendor) von Drittanbietern.

Wenn Sie ein Bewertungstool hinzufügen möchten und noch nicht über ein Azure Migrate-Projekt verfügen, hilft Ihnen [dieser Artikel](how-to-add-tool-first-time.md) weiter.

## <a name="selecting-an-isv-tool"></a>Auswählen eines ISV-Tools

Wenn Sie ein [ISV-Tool](migrate-services-overview.md#isv-integration) für die Bewertung auswählen, können Sie zunächst eine Lizenz erwerben oder sich in Übereinstimmung mit der ISV-Richtlinie für eine kostenlose Testversion anmelden. In jedem Tool ist eine Option zum Herstellen einer Verbindung mit Azure Migrate vorhanden. Befolgen Sie die Anleitung und Dokumentation zum Tool, um den Toolarbeitsbereich mit Azure Migrate zu verbinden. 


## <a name="select-an-assessment-scenario"></a>Auswählen eines Bewertungsszenarios

1. Klicken Sie im Azure Migrate-Projekt auf **Übersicht**.
2. Wählen Sie das Bewertungsszenario aus, das Sie verwenden möchten:

    - Wählen Sie **Server bewerten und migrieren**, um Computer und Workloads zu ermitteln und für die Migration zu Azure zu bewerten.
    - Wählen Sie die Option **Datenbanken bewerten und migrieren**, um lokale SQL-Computer zu bewerten.
    - Wählen Sie die Option **Web-Apps bewerten und migrieren**, um lokale Web-Apps zu bewerten.

    ![Bewertungsszenario](./media/how-to-assess/assess-scenario.png)

## <a name="select-a-server-assessment-tool"></a>Auswählen eines Tools für die Serverbewertung 

1. Klicken Sie auf **Server bewerten und migrieren**.
2. Wenn Sie in **Azure Migrate: Server** noch kein Bewertungstool hinzugefügt haben, wählen Sie unter **Bewertungstools** die Option **Klicken Sie hier, um ein Bewertungstool hinzuzufügen**. Falls Sie bereits Bewertungstools hinzugefügt haben, wählen Sie unter **Weitere Bewertungstools hinzufügen** die Option **Ändern**.

    > [!NOTE]
    > Wenn Sie zu einem anderen Projekt navigieren müssen, klicken Sie in **Azure Migrate: Server** neben **Details zu einem anderen Migrationsprojekt anzeigen** auf **Klicken Sie hier**.

3. Wählen Sie in **Azure Migrate** das gewünschte Bewertungstool aus.

    
    ![Bewertungstools](./media/how-to-assess/assess-tool.png)

    - Bei Verwendung der Azure Migrate-Serverbewertung können Sie Bewertungen direkt im Azure Migrate-Projekt einrichten, ausführen und anzeigen.
    - Navigieren Sie bei Verwendung eines Bewertungstools von einem Drittanbieter über den bereitgestellten Link zur entsprechenden Site, und führen Sie die Bewertung gemäß der angegebenen Anleitung durch.


## <a name="select-a-database-assessment-tool"></a>Auswählen eines Tools für die Datenbankbewertung

1. Klicken Sie auf **Datenbanken bewerten und migrieren**.
2. Klicken Sie unter **Datenbanken** auf **Tools hinzufügen**.
3. Wählen Sie unter „Tool hinzufügen“ > **Bewertungstool auswählen** das Tool aus, das Sie zum Bewerten der Datenbank verwenden möchten.

## <a name="select-a-web-app-assessment-tool"></a>Auswählen eines Tools für die Web-App-Bewertung

1. Klicken Sie auf **Web-Apps bewerten und migrieren**.
2. Folgen Sie dem Link zum Migrationstool für den Azure App Service. Verwenden Sie das Migrationstool, um folgende Aufgaben auszuführen:

    - **Onlinebewertung von Apps**: Sie können Apps mit einer öffentlichen URL online bewerten, indem Sie den Azure App Service-Migrations-Assistenten verwenden.
    - **.NET/PHP**: Für interne .NET- und PHP-Apps können Sie den Migrations-Assistenten herunterladen und ausführen.



## <a name="next-steps"></a>Nächste Schritte

Probieren Sie eine Bewertung aus, indem Sie die Azure Migrate-Serverbewertung für [Hyper-V](tutorial-prepare-hyper-v.md)- oder [VMware](tutorial-prepare-vmware.md)-VMs verwenden.
