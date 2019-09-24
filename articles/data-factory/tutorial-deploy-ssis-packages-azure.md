---
title: Bereitstellen der Azure-SSIS-Integration Runtime | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie zur Bereitstellung und Ausführung von SSIS-Paketen in Azure die Azure-SSIS-Integration Runtime in Azure Data Factory bereitstellen.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: tutorial
ms.date: 09/15/2019
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: b3b180fe4b465f3e0c7de888043326fd1a43cf23
ms.sourcegitcommit: a819209a7c293078ff5377dee266fa76fd20902c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/16/2019
ms.locfileid: "71009652"
---
# <a name="provision-the-azure-ssis-integration-runtime-in-azure-data-factory"></a>Bereitstellen der Azure-SSIS Integration Runtime in Azure Data Factory

In diesem Tutorial werden die Schritte für die Verwendung des Azure-Portals zum Bereitstellen einer Azure-SQL Server Integration Services (SSIS) Integration Runtime (IR) in Azure Data Factory (ADF) beschrieben. Azure-SSIS IR unterstützt die Ausführung von Paketen, die im SSIS-Katalog (SSISDB) bereitgestellt und vom Azure SQL-Datenbank-Server bzw. einer verwalteten Instanz (Projektbereitstellungsmodell) gehostet werden, und von Paketen, die in Dateisystemen/Dateifreigaben/Azure Files (Projektbereitstellungsmodell) bereitgestellt werden. Sobald Azure-SSIS IR bereitgestellt wurde, können Sie vertraute Tools wie SQL Server Data Tools (SSDT)/SQL Server Management Studio (SSMS) und Befehlszeilenprogramme wie `dtinstall`/`dtutil`/`dtexec` verwenden, um Ihre Pakete in Azure bereitzustellen und auszuführen. Konzeptionelle Informationen zu Azure-SSIS IRs finden Sie unter [Azure-SSIS-Integrationslaufzeit](concepts-integration-runtime.md#azure-ssis-integration-runtime).

In diesem Tutorial führen Sie folgende Schritte aus:

> [!div class="checklist"]
> * Erstellen einer Data Factory.
> * Bereitstellen einer Azure-SSIS Integration Runtime

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

- **Azure-Abonnement**. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
- **Azure SQL-Datenbank-Server (optional)** . Wenn Sie noch nicht über einen Datenbankserver verfügen, erstellen Sie einen im Azure-Portal, bevor Sie beginnen. ADF erstellt SSISDB für diesen Datenbankserver. Es empfiehlt sich, den Datenbankserver in derselben Azure-Region zu erstellen, in der sich auch die Integration Runtime befindet. Diese Konfiguration ermöglicht es der Integration Runtime, Ausführungsprotokolle in SSISDB zu schreiben, ohne Azure-Regionen zu überschreiten. 
    - Basierend auf dem ausgewählten Datenbankserver kann SSISDB in Ihrem Auftrag als einzelne Datenbank, als Teil eines Pools für elastische Datenbanken oder in einer verwalteten Instanz erstellt werden und in einem öffentlichen Netzwerk oder durch das Hinzufügen zu einem virtuellen Netzwerk zugänglich sein. Einen Leitfaden zum Auswählen des Datenbankservertyps zum Hosten von SSISDB finden Sie unter [Vergleich zwischen SQL-Datenbank mit Einzeldatenbank/Pool für elastische Datenbanken und verwalteter SQL-Datenbank-Instanz](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance). Wenn Sie den Azure SQL-Datenbank-Server mit VNET-Dienstendpunkten oder mit einer verwalteten Instanz mit einem privaten Endpunkt zum Hosten von SSISDB verwenden oder Zugriff auf lokale Daten ohne Konfiguration einer selbstgehosteten IR benötigen, müssen Sie Azure-SSIS IR einem virtuellen Netzwerk hinzufügen. Weitere Informationen finden Sie unter [Erstellen von Azure-SSIS IR in einem virtuellen Netzwerk](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).
    - Vergewissern Sie sich, dass die Einstellung **Zugriff auf Azure-Dienste zulassen** für den Datenbankserver aktiviert ist. Dies gilt nicht, wenn Sie einen Azure SQL-Datenbank-Server mit VNET-Dienstendpunkten oder einer verwalteten Instanz mit einem privaten Endpunkt zum Hosten von SSISDB verwenden. Weitere Informationen finden Sie unter [Schützen der Azure SQL-Datenbank](../sql-database/sql-database-security-tutorial.md#create-firewall-rules). Informationen zum Aktivieren dieser Einstellung mithilfe von PowerShell finden Sie unter [New-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule).
    - Fügen Sie der Client-IP-Adressenliste in den Firewalleinstellungen für den Datenbankserver die IP-Adresse des Clientcomputers oder einen IP-Adressbereich hinzu, der die IP-Adresse des Clientcomputers enthält. Weitere Informationen finden Sie unter [Firewallregeln auf Serverebene und Datenbankebene für Azure SQL-Datenbank](../sql-database/sql-database-firewall-configure.md).
    - Sie können eine Verbindung mit dem Datenbankserver herstellen, indem Sie die SQL-Authentifizierung mit Ihren Serveradministrator-Anmeldeinformationen oder die Azure Active Directory-Authentifizierung (AAD) mit der verwalteten Identität für Ihre ADF-Instanz verwenden. Für Letztere müssen Sie die verwaltete Identität für Ihre Azure Data Factory-Instanz einer AAD-Gruppe mit Zugriffsberechtigungen für den Datenbankserver hinzufügen. Weitere Informationen finden Sie unter [Erstellen einer Azure-SSIS Integration Runtime in Azure Data Factory](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).
    - Vergewissern Sie sich, dass Ihr Datenbankserver noch nicht über eine SSISDB verfügt. Für die Bereitstellung einer Azure-SSIS IR wird die Verwendung einer vorhandenen SSISDB nicht unterstützt.

> [!NOTE]
> - Eine Liste mit den Azure-Regionen, in denen ADF und Azure-SSIS IR derzeit verfügbar sind, finden Sie unter [ADF- und SSIS IR-Verfügbarkeit nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all). 

## <a name="create-a-data-factory"></a>Erstellen einer Data Factory

Zum Erstellen Ihrer ADF über das Azure-Portal befolgen Sie die Schritt-für-Schritt-Anleitung im Artikel [Erstellen der ADF über die Benutzeroberfläche](https://docs.microsoft.com/azure/data-factory/quickstart-create-data-factory-portal#create-a-data-factory), und wählen Sie dabei **An Dashboard anheften** aus, um nach Erstellung den schnellen Zugriff zuzulassen. 

Sobald Ihre ADF erstellt ist, öffnen Sie die zugehörige Übersichtsseite im Azure-Portal, und klicken Sie auf die Kachel **Erstellen und überwachen**, um die Seite **Let's get started** (Erste Schritte) auf einer separaten Registerkarte zu öffnen, wo Sie mit der Erstellung Ihrer Azure-SSIS IR fortfahren können.   

## <a name="create-an-azure-ssis-integration-runtime"></a>Erstellen einer Azure SSIS Integration Runtime

### <a name="from-the-data-factory-overview"></a>Über die Seite mit der Übersicht über Data Factory

1. Klicken Sie auf der Seite **Let's get started** (Erste Schritte) die Kachel **Configure SSIS Integration Runtime** (SSIS Integration Runtime konfigurieren). 

   ![Kachel „Configure SSIS Integration Runtime“ (SSIS Integration Runtime konfigurieren)](./media/tutorial-create-azure-ssis-runtime-portal/configure-ssis-integration-runtime-tile.png)

1. Die übrigen Schritte zum Einrichten einer Azure-SSIS IR finden Sie im Abschnitt [Bereitstellen einer Azure-SSIS Integration Runtime](#provision-an-azure-ssis-integration-runtime). 

### <a name="from-the-authoring-ui"></a>Über die Benutzeroberfläche für die Erstellung

1. Wechseln Sie auf der Azure Data Factory-Benutzeroberfläche zur Registerkarte **Bearbeiten**, wählen Sie die Option **Verbindungen**, und wechseln Sie dann zur Registerkarte **Integration Runtimes**, um vorhandene Integration Runtimes in Ihrer Data Factory anzuzeigen. 

   ![Auswahl zum Anzeigen von vorhandenen IRs](./media/tutorial-create-azure-ssis-runtime-portal/view-azure-ssis-integration-runtimes.png)

1. Wählen Sie **Neu**, um eine Azure-SSIS IR zu erstellen. 

   ![Integration Runtime über das Menü](./media/tutorial-create-azure-ssis-runtime-portal/edit-connections-new-integration-runtime-button.png)

1. Wählen Sie im Fenster für die **Integration Runtime-Einrichtung** die Option **Lift-and-shift existing SSIS packages to execute in Azure** (Migrieren vorhandener SSIS-Pakete per Lift & Shift für die Ausführung in Azure) und dann **Weiter**. 

   ![Angeben des Integration Runtime-Typs](./media/tutorial-create-azure-ssis-runtime-portal/integration-runtime-setup-options.png)

1. Die übrigen Schritte zum Einrichten einer Azure-SSIS IR finden Sie im Abschnitt [Bereitstellen einer Azure-SSIS Integration Runtime](#provision-an-azure-ssis-integration-runtime). 

## <a name="provision-an-azure-ssis-integration-runtime"></a>Bereitstellen einer Azure-SSIS Integration Runtime

1. Führen Sie auf der Seite **Allgemeine Einstellungen** von **Integration Runtime Setup** (Integration Runtime-Setup) die folgenden Schritte aus: 

   ![Allgemeine Einstellungen](./media/tutorial-create-azure-ssis-runtime-portal/general-settings.png)

   a. Geben Sie unter **Name** den Namen Ihrer Integration Runtime ein. 

   b. Geben Sie unter **Beschreibung** die Beschreibung Ihrer Integration Runtime ein. 

   c. Wählen Sie unter **Standort** den Standort für Ihre Integration Runtime aus. Es werden nur unterstützte Standorte angezeigt. Sie sollten den gleichen Standort wie für den Datenbankserver zum Hosten von SSISDB auswählen. 

   d. Wählen Sie unter **Knotengröße** die Größe des Knotens in Ihrem Integration Runtime-Cluster aus. Nur unterstützte Knotengrößen werden angezeigt. Wählen Sie eine große Knotengröße (zentrales Hochskalieren) aus, wenn Sie zahlreiche compute-/arbeitsspeicherintensive Pakete ausführen möchten. 

   e. Wählen Sie unter **Knotenzahl** die Anzahl von Knoten in Ihrer Integration Runtime aus. Nur unterstützte Knotenzahlen werden angezeigt. Wählen Sie einen großen Cluster mit zahlreichen Knoten (horizontales Hochskalieren) aus, wenn Sie viele Pakete parallel ausführen möchten. 

   f. Wählen Sie unter **Edition/Lizenz** die SQL Server-Edition/-Lizenz für Ihre Integration Runtime aus: Standard oder Enterprise. Wählen Sie Enterprise aus, wenn Sie erweiterte Funktionen oder Premium-Funktionen für Ihre Integration Runtime verwenden möchten. 

   g. Wählen Sie unter **Sparen Sie Geld** die Option „Azure-Hybridvorteil“ (Azure Hybrid Benefit, AHB) für Ihre Integration Runtime aus: „Ja“ oder „Nein“. Wählen Sie „Ja“, wenn Sie eine eigene SQL Server-Lizenz mit Software Assurance verwenden möchten, um bei der Hybridnutzung von Kostenersparnissen zu profitieren. 

   h. Klicken Sie auf **Weiter**. 

1. Führen Sie auf der Seite **SQL-Einstellungen** die folgenden Schritte aus: 

   ![SQL-Einstellungen](./media/tutorial-create-azure-ssis-runtime-portal/sql-settings.png)

   a. Wählen Sie mit dem Kontrollkästchen **SSIS-Katalog erstellen** das Bereitstellungsmodell für Pakete aus, die unter Ihrer Azure-SSIS IR ausgeführt werden: Projektbereitstellungsmodell, bei dem Pakete in SSISDB bereitgestellt werden, die von Ihrem Datenbankserver gehostet werden, oder Paketbereitstellungsmodell, bei dem Pakete in Ihren Dateisystemen/Dateifreigaben/Azure Files bereitgestellt werden. Wenn Sie das Kontrollkästchen aktivieren, müssen Sie Ihren eigenen Datenbankserver zum Hosten von SSISDB bereitstellen, die wir in Ihrem Namen erstellen und verwalten.
   
   b. Geben Sie unter **Abonnement** das Azure-Abonnement mit dem Azure-Datenbankserver zum Hosten von SSISDB an. 

   c. Wählen Sie als **Standort** den Standort des Datenbankservers zum Hosten von SSISDB aus. Sie sollten denselben Standort wie den für die Integration Runtime auswählen.

   d. Wählen Sie unter **Katalogdatenbank-Serverendpunkt** den Endpunkt Ihres Datenbankservers zum Hosten von SSISDB aus. Basierend auf dem ausgewählten Datenbankserver kann SSISDB in Ihrem Auftrag als einzelne Datenbank, als Teil eines Pools für elastische Datenbanken oder in einer verwalteten Instanz erstellt werden und in einem öffentlichen Netzwerk oder durch das Hinzufügen zu einem virtuellen Netzwerk zugänglich sein. Einen Leitfaden zum Auswählen des Datenbankservertyps zum Hosten von SSISDB finden Sie unter [Vergleich zwischen SQL-Datenbank mit Einzeldatenbank/Pool für elastische Datenbanken und verwalteter SQL-Datenbank-Instanz](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance). Wenn Sie den Azure SQL-Datenbank-Server mit VNET-Dienstendpunkten oder mit einer verwalteten Instanz mit einem privaten Endpunkt zum Hosten von SSISDB auswählen oder Zugriff auf lokale Daten ohne Konfiguration einer selbstgehosteten IR benötigen, müssen Sie Azure-SSIS IR einem virtuellen Netzwerk hinzufügen. Weitere Informationen finden Sie unter [Erstellen von Azure-SSIS IR in einem virtuellen Netzwerk](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). 

   e. Wählen Sie mit dem Kontrollkästchen **AAD-Authentifizierung verwenden...** die Authentifizierungsmethode für Ihren Datenbankserver zum Hosten von SSISDB aus: SQL-Authentifizierung oder AAD-Authentifizierung mit der verwalteten Identität für Ihre ADF. Wenn Sie das Kontrollkästchen aktivieren, müssen Sie die verwaltete Identität für Ihre Azure Data Factory-Instanz in einer AAD-Gruppe mit Zugriffsberechtigungen für Ihren Datenbankserver hinzufügen. Weitere Informationen finden Sie unter [Erstellen von Azure-SSIS Integration Runtime in Azure Data Factory](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). 

   f. Geben Sie unter **Administratorbenutzername** den Benutzernamen für die SQL-Authentifizierung für den Datenbankserver zum Hosten von SSISDB an. 

   g. Geben Sie unter **Administratorbenutzerkennwort** das Kennwort für die SQL-Authentifizierung für den Datenbankserver zum Hosten von SSISDB an. 

   h. Wählen Sie unter **Katalogdatenbank-Dienstebene** die Dienstebene für Ihren Datenbankserver zum Hosten von SSISDB aus: Basic/Standard/Premium oder Name des Pools für elastische Datenbanken.

   i. Klicken Sie auf **Verbindung testen** und bei erfolgreichem Test auf **Weiter**. 

1. Führen Sie auf der Seite **Erweiterte Einstellungen** die folgenden Schritte aus: 

   ![Erweiterte Einstellungen](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)

   a. Legen Sie unter **Maximale Anzahl von parallelen Ausführungen pro Knoten** die maximale Anzahl von Paketen fest, die in Ihrem Integration Runtime-Cluster pro Knoten gleichzeitig ausgeführt werden sollen. Nur unterstützte Paketzahlen werden angezeigt. Wählen Sie eine niedrigere Anzahl aus, wenn Sie mehrere Kerne verwenden möchten, um ein einzelnes großes/umfangreiches Paket auszuführen, das compute-/arbeitsspeicherintensiv ist. Wählen Sie eine hohe Anzahl aus, wenn Sie mindestens ein kleines Paket in einem einzelnen Kern ausführen möchten. 

   b. Geben Sie unter **Container-SAS-URI des benutzerdefinierten Setups** optional den SAS-URI (Shared Access Signature, Uniform Resource Identifier) Ihres Azure Storage Blob-Containers an, in dem Ihr Einrichtungsskript und die zugehörigen Dateien gespeichert sind. Weitere Informationen finden Sie unter [Benutzerdefiniertes Setup von Azure-SSIS-Integration Runtime](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup). 

   c. Geben Sie mithilfe des Kontrollkästchens **Select a VNet...** (VNET für den Beitritt der Azure-SSIS-Integration Runtime auswählen und Azure-Diensten die Konfiguration von VNET-Berechtigungen/-Einstellungen erlauben) an, ob Ihre Integration Runtime zu einem virtuellen Netzwerk hinzugefügt werden soll. Sie sollten es aktivieren, wenn Sie den Azure SQL-Datenbank-Server mit VNET-Dienstendpunkten oder mit einer verwalteten Instanz mit einem privaten Endpunkt zum Hosten von SSISDB verwenden oder Zugriff auf lokale Daten ohne Konfiguration einer selbstgehosteten IR benötigen. Weitere Informationen finden Sie unter [Erstellen von Azure-SSIS IR in einem virtuellen Netzwerk](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). 

   d. Wählen Sie mit dem Kontrollkästchen **Set up Self-Hosted...** (Selbstgehostete Integration Runtime als Proxy für die Azure-SSIS Integration Runtime einrichten) aus, ob Sie eine selbstgehostete IR als Proxy für die Azure-SSIS IR konfigurieren möchten. Informationen dazu finden Sie unter [Einrichten einer selbst gehosteten IR als Proxy](https://docs.microsoft.com/azure/data-factory/self-hosted-integration-runtime-proxy-ssis). 

1. Klicken Sie auf **Weiter**. 

1. Überprüfen Sie auf der Seite **Zusammenfassung** alle Bereitstellungseinstellungen, lesen Sie die empfohlenen Dokumentationslinks, und klicken Sie auf **Fertig stellen**, um die Erstellung ihrer Integration Runtime zu starten. 

   > [!NOTE]
   > Abgesehen von einer benutzerdefinierten Einrichtungszeit sollte dieser Prozess innerhalb von 5 Minuten abgeschlossen sein.
   >
   > Bei Verwendung von SSISDB stellt der ADF-Dienst eine Verbindung mit Ihrem Datenbankserver her, um SSISDB vorzubereiten. 
   > 
   > Wenn Sie Azure-SSIS IR bereitstellen, werden auch die Access Redistributable-Komponente und das Azure Feature Pack für SSIS installiert. Diese Komponenten ermöglichen nicht nur die Konnektivität mit Excel- und Access-Dateien und verschiedenen Azure-Datenquellen, sondern auch mit den Datenquellen, die bereits von integrierten Komponenten unterstützt werden. Sie können auch weitere Komponenten installieren. Weitere Informationen dazu finden Sie unter [Benutzerdefiniertes Setup für Azure-SSIS IR](how-to-configure-azure-ssis-ir-custom-setup.md).

1. Wechseln Sie auf der Registerkarte **Verbindungen** ggf. zu **Integration Runtimes**. Wählen Sie zum Aktualisieren des Status die Option **Aktualisieren**. 

   ![Erstellungsstatus, Schaltfläche „Aktualisieren“](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-creation-status.png)

1. Über die Links in der Spalte **Aktionen** können Sie die Integration Runtime beenden/starten, bearbeiten oder löschen. Über den letzten Link können Sie den JSON-Code für die Integration Runtime anzeigen. Die Schaltflächen zum Bearbeiten und Löschen sind nur verfügbar, wenn die IR beendet wird. 

   ![Links in der Spalte „Aktionen“](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-actions.png) 

## <a name="deploy-ssis-packages"></a>Bereitstellen von SSIS-Paketen

Wenn Sie SSISDB verwenden, können Sie Ihre Pakete darin bereitstellen und sie in Azure-SSIS IR mithilfe von SSDT/SSMS-Tools ausführen, die über den Serverendpunkt eine Verbindung mit Ihrem Datenbankserver herstellen. Für den Azure SQL-Datenbank-Server oder eine verwaltete Instanz mit einem öffentlichen Endpunkt wird jeweils das Serverendpunktformat `<server name>.database.windows.net`/`<server name>.public.<dns prefix>.database.windows.net,3342` verwendet. Wenn Sie SSISDB nicht verwenden, können Sie Ihre Pakete in Dateisystemen, Dateifreigaben und Azure Files bereitstellen und sie mithilfe von Befehlszeilenhilfsprogrammen (`dtinstall`/`dtutil`/`dtexec`) unter Azure-SSIS IR ausführen. Weitere Informationen finden Sie unter [Bereitstellen von SSIS-Paketen](/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages#deploy-packages-to-integration-services-server). In beiden Fällen können Sie Ihre bereitgestellten Pakete auch unter Azure-SSIS IR ausführen, indem Sie die Aktivität „SSIS-Paket ausführen“ in ADF-Pipelines verwenden. Weitere Informationen finden Sie unter [Aufrufen der SSIS-Paketausführung als erstklassige ADF-Aktivität](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity). 

Informationen finden Sie außerdem in den folgenden Artikeln der SSIS-Dokumentation: 

- [Bereitstellen und Ausführen eines SSIS-Pakets in Azure](/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial) 
- [Herstellen einer Verbindung mit dem SSIS-Katalog (SSISDB) in Azure](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database) 
- [Planen der Ausführung von in Azure bereitgestellten SSIS-Paketen](/sql/integration-services/lift-shift/ssis-azure-schedule-packages) 
- [Connect to on-premises data sources with Windows Authentication](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth) (Herstellen einer Verbindung mit lokalen Datenquellen mit Windows-Authentifizierung) 

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt: 

> [!div class="checklist"]
> * Erstellen einer Data Factory.
> * Bereitstellen einer Azure-SSIS Integration Runtime
> * Bereitstellen von SSIS-Paketen

Informationen zum Anpassen Ihrer Azure-SSIS Integration Runtime finden Sie im folgenden Artikel: 

> [!div class="nextstepaction"]
> [Anpassen des Setups für Azure-SSIS Integration Runtime](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup)