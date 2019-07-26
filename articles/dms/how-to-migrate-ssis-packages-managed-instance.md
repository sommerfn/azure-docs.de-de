---
title: Migrieren von SQL Server Integration Services-Paketen zu einer verwalteten Azure SQL-Datenbank-Instanz | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie SQL Server Integration Services-Pakete zu einer verwalteten Azure SQL-Datenbank-Instanz migrieren.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 06/08/2019
ms.openlocfilehash: 82a047616c199e37bfa22f53e02f3f7b224b47c1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67082183"
---
# <a name="migrate-sql-server-integration-services-packages-to-an-azure-sql-database-managed-instance"></a>Migrieren von SQL Server Integration Services-Paketen zu einer verwalteten Azure SQL-Datenbank-Instanz
Wenn Sie SQL Server Integration Services (SSIS) verwenden und Ihre SSIS-Projekte/-Pakete aus der vom SQL Server gehosteten SSIS-Quelldatenbank in die von einer verwalteten Azure SQL-Datenbank-Instanz gehostete SSIS-Zieldatenbank migrieren möchten, können Sie den Azure Database Migration Service verwenden.

Wenn Sie eine ältere Version als SSIS 2012 oder SSIS-Datenbank-fremde Paketspeichertypen verwenden, müssen Sie Ihre SSIS-Projekte/-Pakete mithilfe des Bereitstellungs-Assistenten für Integration Services konvertieren, bevor Sie sie migrieren können. Der Bereitstellungs-Assistent kann ebenfalls direkt in SSMS gestartet werden. Weitere Informationen finden Sie im Artikel [Bereitstellen von SQL Server Integration Services-Projekten und Paketen (SSIS)](https://docs.microsoft.com/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages?view=sql-server-2017#convert).

> [!NOTE]
> Azure SQL-Datenbank wird derzeit von Azure Database Migration Service (DMS) nicht als Migrationsziel unterstützt. Informationen zum erneuten Bereitstellen von SSIS-Projekten/-Paketen für Azure SQL-Datenbank finden Sie im Artikel [Erneutes Bereitstellen von SQL Server Integration Services-Paketen für Azure SQL-Datenbank](https://docs.microsoft.com/azure/dms/how-to-migrate-ssis-packages).

In diesem Artikel werden folgende Vorgehensweisen behandelt:
> [!div class="checklist"]
>
> * Bewerten von SSIS-Quellprojekten/-paketen.
> * Migrieren von SSIS-Projekten/-Paketen zu Azure

## <a name="prerequisites"></a>Voraussetzungen

Zum Ausführen dieser Schritte benötigen Sie Folgendes:

* Erstellen Sie ein virtuelles Azure-Netzwerk (VNET) für Azure Database Migration Service, indem Sie das Azure Resource Manager-Bereitstellungsmodell verwenden, das entweder über [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) oder über [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) Site-to-Site-Konnektivität für Ihre lokalen Quellserver bereitstellt. Weitere Informationen finden Sie im Artikel [Netzwerktopologien für Migrationen von verwalteten Azure SQL-Datenbank-Instanzen mithilfe von Azure Database Migration Service]( https://aka.ms/dmsnetworkformi). Weitere Informationen zum Erstellen eines VNET finden Sie in der [Dokumentation zu Virtual Network](https://docs.microsoft.com/azure/virtual-network/) und insbesondere in den Schnellstartartikeln mit Schritt-für-Schritt-Anleitungen.
* Stellen Sie sicher, dass die Netzwerksicherheitsgruppen-Regeln des VNET nicht die folgenden Ports für eingehende Kommunikation in Azure Database Migration Service blockieren: 443, 53, 9354, 445, 12000. Ausführlichere Informationen zur NSG-Datenverkehrsfilterung in einem Azure-VNET finden Sie im Artikel [Filtern des Netzwerkdatenverkehrs mit Netzwerksicherheitsgruppen](https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm).
* Konfigurieren Sie Ihre [Windows-Firewall für Quelldatenbank-Engine-Zugriff](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access?view=sql-server-2017).
* Öffnen Sie Ihre Windows-Firewall, damit Azure Database Migration Service auf die SQL Server-Quellinstanz zugreifen kann (standardmäßig TCP-Port 1433).
* Bei der Ausführung mehrerer benannter SQL Server-Instanzen mit dynamischen Ports empfiehlt es sich, den SQL-Browser-Dienst zu aktivieren und den Zugriff auf den UDP-Port 1434 durch Ihre Firewalls zuzulassen, sodass Azure Database Migration Service eine Verbindung mit einer benannten Instanz auf Ihrem Quellserver herstellen kann.
* Wenn Sie eine Firewallappliance vor Ihren Quelldatenbanken verwenden, müssen Sie möglicherweise Firewallregeln hinzufügen, damit Azure Database Migration Service auf die Quelldatenbanken für die Migration sowie auf Dateien über SMB-Port 445 zugreifen kann.
* Eine verwaltete Azure SQL-Datenbank-Instanz zum Hosten der SSIS-Datenbank. Wenn Sie eine solche erstellen müssen, befolgen Sie die Anweisungen im Artikel [Erstellen einer verwalteten Azure SQL-Datenbank-Instanz](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-get-started).
* Stellen Sie sicher, dass die zum Herstellen einer Verbindung zwischen der SQL Server-Quellinstanz und der verwalteten Zielinstanz verwendeten Anmeldungen Mitglieder der sysadmin-Serverrolle sind.
* Stellen Sie sicher, dass SSIS in Azure Data Factory (ADF) bereitgestellt werden. Dieser Dienst beinhaltet Azure-SSIS Integration Runtime (IR), wobei die SSIS-Zieldatenbank von der verwalteten Azure SQL-Datenbank-Instanz gehostet wird (wie im Artikel [Erstellen der Azure-SSIS Integration Runtime in Azure Data Factory](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime) beschrieben).

## <a name="assess-source-ssis-projectspackages"></a>Bewerten von SSIS-Quellprojekten/-paketen

Während das Bewerten von SSIS-Quelldatenbanken momentan nicht im Datenmigrations-Assistenten (DMA) integriert ist, werden Ihre SSIS-Projekte/-Pakete bewertet/überprüft, wenn Sie in der von einer verwalteten Azure SQL-Datenbank-Instanz gehosteten SSIS-Zieldatenbank erneut bereitgestellt werden.

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Registrieren des Ressourcenanbieters „Microsoft.DataMigration“

1. Melden Sie sich beim Azure-Portal an, und klicken Sie auf **Alle Dienste** und anschließend auf **Abonnements**.

    ![Abonnements im Portal anzeigen](media/how-to-migrate-ssis-packages-mi/portal-select-subscriptions.png)

2. Wählen Sie das Abonnement aus, in dem Sie die Azure Database Migration Service-Instanz erstellen möchten, und klicken Sie dann auf **Ressourcenanbieter**.

    ![Ressourcenanbieter anzeigen](media/how-to-migrate-ssis-packages-mi/portal-select-resource-provider.png)

3. Suchen Sie nach „Migration“, und wählen Sie rechts neben **Microsoft.DataMigration** die Option **Registrieren** aus.

    ![Registrieren des Ressourcenanbieters](media/how-to-migrate-ssis-packages-mi/portal-register-resource-provider.png)

## <a name="create-an-azure-database-migration-service-instance"></a>Erstellen einer Instanz von Azure Database Migration Service

1. Wählen Sie im Azure-Portal die Option **+ Ressource erstellen**, suchen Sie nach **Azure Database Migration Service**, und wählen Sie anschließend **Azure Database Migration Service** aus der Dropdownliste aus.

     ![Azure Marketplace](media/how-to-migrate-ssis-packages-mi/portal-marketplace.png)

2. Wählen Sie auf dem Bildschirm **Azure Database Migration Service** die Schaltfläche **Erstellen** aus.

    ![Erstellen einer Instanz von Azure Database Migration Service](media/how-to-migrate-ssis-packages-mi/dms-create1.png)

3. Geben Sie auf dem Bildschirm **Migrationsdienst erstellen** einen Namen für den Dienst, das Abonnement und eine neue oder vorhandene Ressourcengruppe an.

4. Wählen Sie den Speicherort aus, an dem Sie die DMS-Instanz erstellen möchten.

5. Wählen Sie ein vorhandenes VNET aus, oder erstellen Sie ein VNET.

    Das VNET erteilt Azure Database Migration Service Zugriff auf die SQL Server-Quellinstanz und die verwaltete Azure SQL-Datenbank-Zielinstanz.

    Weitere Informationen zum Erstellen eines VNET im Azure-Portal finden Sie im Artikel [Erstellen eines virtuellen Netzwerks im Azure Portal](https://aka.ms/DMSVnet).

    Weitere Einzelheiten finden Sie im Artikel [Netzwerktopologien für Migrationen von verwalteten Azure SQL-Datenbank-Instanzen mithilfe von Azure Database Migration Service](https://aka.ms/dmsnetworkformi).

6. Wählen Sie einen Tarif.

    Weitere Informationen zu Kosten und Tarifen finden Sie in der [Preisübersicht](https://aka.ms/dms-pricing).

    ![Erstellen eines DMS-Diensts](media/how-to-migrate-ssis-packages-mi/dms-create-service2.png)

7. Wählen Sie **Erstellen**, um den Dienst zu erstellen.

## <a name="create-a-migration-project"></a>Erstellen eines Migrationsprojekts

Nachdem eine Instanz des Diensts erstellt wurde, suchen Sie diesen im Azure-Portal, öffnen Sie ihn, und erstellen Sie anschließend ein neues Migrationsprojekt.

1. Wählen Sie im Azure-Portal **Alle Dienste**, suchen Sie nach Azure Database Migration Service, und wählen Sie dann **Azure Database Migration Service** aus.

    ![Suchen aller Instanzen von Azure Database Migration Service](media/how-to-migrate-ssis-packages-mi/dms-search.png)

2. Suchen Sie im Bildschirm **Azure Database Migration Service** nach dem Namen der von Ihnen erstellten Instanz, und wählen Sie anschließend die Instanz aus.

3. Wählen Sie **+ Neues Migrationsprojekt** aus.

4. Geben Sie auf dem Bildschirm **Neues Migrationsprojekt** einen Projektnamen ein, und wählen Sie im Textfeld **Quellservertyp** die Option **SQL Server** und im Textfeld **Zielservertyp** die Option **Verwaltete Azure SQL-Datenbank-Instanz** aus. Wählen Sie außerdem für **Aktivitätstyp auswählen** die Option **SSIS-Paketmigration** aus.

   ![Erstellen eines DMS-Projekts](media/how-to-migrate-ssis-packages-mi/dms-create-project2.png)

5. Wählen Sie **Create** (Erstellen), um das Projekt zu erstellen.

## <a name="specify-source-details"></a>Angeben von Quelldetails

1. Geben Sie im Bildschirm **Migrationsquelldetail** die Verbindungsdetails für die SQL Server-Quellinstanz an.

2. Falls Sie auf Ihrem Server kein vertrauenswürdiges Zertifikat installiert haben, aktivieren Sie das Kontrollkästchen **Serverzertifikat vertrauen**.

    Wenn kein vertrauenswürdiges Zertifikat installiert ist, generiert SQL Server beim Starten der Instanz ein selbstsigniertes Zertifikat. Dieses Zertifikat wird zum Verschlüsseln der Anmeldeinformationen für Clientverbindungen verwendet.

    > [!CAUTION]
    > SSL-Verbindungen, die mit einem selbstsignierten Zertifikat verschlüsselt sind, bieten keine hohe Sicherheit. Sie sind anfällig für Man-in-the-Middle-Angriffe. Verlassen Sie sich in einer Produktionsumgebung oder auf Servern, die mit dem Internet verbunden sind, nicht auf SSL-Verbindungen, die selbstsignierte Zertifikate verwenden.

   ![Quellendetails](media/how-to-migrate-ssis-packages-mi/dms-source-details1.png)

3. Wählen Sie **Speichern** aus.

## <a name="specify-target-details"></a>Angeben von Zieldetails

1. Geben Sie im Bildschirm **Migrationszieldetail** die Verbindungsdetails für das Ziel an.

     ![Zieldetails](media/how-to-migrate-ssis-packages-mi/dms-target-details2.png)

2. Wählen Sie **Speichern** aus.

## <a name="review-the-migration-summary"></a>Überprüfen der Migrationszusammenfassung

1. Geben Sie auf dem Bildschirm **Migrationszusammenfassung** im Textfeld **Aktivitätsname** einen Namen für die Migrationsaktivität an.

2. Geben Sie für die Option **SSIS-Projekt(e) und -Umgebung(en) überschreiben**an, ob vorhandene SSIS-Projekte und -Umgebungen überschrieben oder ignoriert werden sollen.

    ![Zusammenfassung des Migrationsprojekts](media/how-to-migrate-ssis-packages-mi/dms-project-summary2.png)

3. Überprüfen und verifizieren Sie die dem Migrationsprojekt zugeordneten Details.

## <a name="run-the-migration"></a>Ausführen der Migration

* Wählen Sie **Migration ausführen** aus.

## <a name="next-steps"></a>Nächste Schritte

* Überprüfen der Migrationsanleitung im [Leitfaden zur Datenbankmigration von Microsoft](https://datamigration.microsoft.com/)
