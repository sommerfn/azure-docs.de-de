---
title: Bereitstellen der Azure-SSIS Integration Runtime | Microsoft-Dokumentation
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
ms.openlocfilehash: 62b11fbf19c803d54c699d3a59454677ac3423f1
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72326075"
---
# <a name="provision-the-azure-ssis-integration-runtime-in-azure-data-factory"></a>Bereitstellen der Azure-SSIS Integration Runtime in Azure Data Factory

In diesem Tutorial werden die Schritte für die Verwendung des Azure-Portals zum Bereitstellen einer Azure-SQL Server Integration Services (SSIS) Integration Runtime (IR) in Azure Data Factory beschrieben. Die Azure-SSIS IR unterstützt Folgendes:

- Ausführen von Paketen, die im SSIS-Katalog (SSISDB) bereitgestellt werden, wobei zum Hosten ein Azure SQL-Datenbank-Server oder eine verwaltete Instanz verwendet wird (Projektbereitstellungsmodell)
- Ausführen von Paketen, die in Dateisystemen, auf Dateifreigaben oder unter Azure Files bereitgestellt werden (Projektbereitstellungsmodell) 

Nach der Bereitstellung einer Azure-SSIS IR können Sie die vertrauten Tools nutzen, um Ihre Pakete in Azure bereitzustellen und auszuführen. Beispiele für diese Tools sind SQL Server Data Tools, SQL Server Management Studio und Befehlszeilentools wie `dtinstall`, `dtutil` und `dtexec`.

Konzeptionelle Informationen zu Azure-SSIS IRs finden Sie unter [Azure-SSIS-Integrationslaufzeit](concepts-integration-runtime.md#azure-ssis-integration-runtime).

In diesem Tutorial führen Sie folgende Schritte aus:

> [!div class="checklist"]
> * Erstellen einer Data Factory.
> * Bereitstellen einer Azure-SSIS Integration Runtime

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

- **Azure-Abonnement**. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
- **Azure SQL-Datenbank-Server (optional)** . Wenn Sie noch nicht über einen Datenbankserver verfügen, erstellen Sie einen im Azure-Portal, bevor Sie beginnen. Mit Data Factory wird wiederum eine SSISDB-Instanz auf diesem Datenbankserver erstellt. 

  Es empfiehlt sich, den Datenbankserver in derselben Azure-Region zu erstellen, in der sich auch die Integration Runtime befindet. Diese Konfiguration ermöglicht es der Integration Runtime, Ausführungsprotokolle in SSISDB zu schreiben, ohne Azure-Regionen zu überschreiten.

  Beachten Sie Folgendes:

  - Basierend auf dem ausgewählten Datenbankserver kann die SSISDB-Instanz in Ihrem Namen als Einzeldatenbank, als Teil eines Pools für elastische Datenbanken oder auf einer verwalteten Instanz erstellt werden. Der Zugriff kann über ein öffentliches Netzwerk oder durch den Beitritt zu einem virtuellen Netzwerk erfolgen. Einen Leitfaden zum Auswählen des Datenbankservertyps zum Hosten der SSISDB finden Sie unter [Vergleich zwischen SQL-Datenbank mit Einzeldatenbank/Pool für elastische Datenbanken und verwalteter SQL-Datenbank-Instanz](../data-factory/create-azure-ssis-integration-runtime.md#comparison-of-a-sql-database-single-database-elastic-pool-and-managed-instance). 
  
    Wenn Sie einen Azure SQL-Datenbank-Server mit VNET-Dienstendpunkten oder mit einer verwalteten Instanz mit einem privaten Endpunkt zum Hosten der SSISDB verwenden oder Zugriff auf lokale Daten ohne Konfiguration einer selbstgehosteten IR benötigen, müssen Sie die Azure-SSIS IR einem virtuellen Netzwerk hinzufügen. Weitere Informationen finden Sie unter [Erstellen von Azure-SSIS Integration Runtime in Azure Data Factory](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).
  - Vergewissern Sie sich, dass die Einstellung **Zugriff auf Azure-Dienste zulassen** für den Datenbankserver aktiviert ist. Diese Einstellung gilt nicht, wenn Sie einen Azure SQL-Datenbank-Server mit VNET-Dienstendpunkten oder eine verwaltete Instanz mit einem privaten Endpunkt zum Hosten von SSISDB verwenden. Weitere Informationen finden Sie unter [Schützen der Azure SQL-Datenbank](../sql-database/sql-database-security-tutorial.md#create-firewall-rules). Informationen zum Aktivieren dieser Einstellung mithilfe von PowerShell finden Sie unter [New-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule).
  - Fügen Sie der Client-IP-Adressenliste in den Firewalleinstellungen für den Datenbankserver die IP-Adresse des Clientcomputers oder einen IP-Adressbereich hinzu, der die IP-Adresse des Clientcomputers enthält. Weitere Informationen finden Sie unter [Firewallregeln auf Serverebene und Datenbankebene für Azure SQL-Datenbank](../sql-database/sql-database-firewall-configure.md).
  - Sie können eine Verbindung mit dem Datenbankserver herstellen, indem Sie die SQL-Authentifizierung mit Ihren Serveradministrator-Anmeldeinformationen oder die Azure AD-Authentifizierung mit der verwalteten Identität für Ihre Data Factory-Instanz verwenden. Im letzteren Fall müssen Sie die verwaltete Identität für Ihre Data Factory-Instanz einer Azure AD-Gruppe mit Zugriffsberechtigungen für den Datenbankserver hinzufügen. Weitere Informationen finden Sie unter [Erstellen von Azure-SSIS Integration Runtime in Azure Data Factory](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).
  - Vergewissern Sie sich, dass Ihr Datenbankserver noch nicht über eine SSISDB-Instanz verfügt. Für die Bereitstellung einer Azure-SSIS IR wird die Verwendung einer vorhandenen SSISDB-Instanz nicht unterstützt.


> [!NOTE]
> Eine Liste mit den Azure-Regionen, in denen Data Factory und eine Azure-SSIS Integration Runtime derzeit verfügbar sind, finden Sie unter [Data Factory und SSIS IR: Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all). 

## <a name="create-a-data-factory"></a>Erstellen einer Data Factory

Befolgen Sie die Schritt-für-Schritt-Anleitung unter [Erstellen einer Data Factory](https://docs.microsoft.com/azure/data-factory/quickstart-create-data-factory-portal#create-a-data-factory), um Ihre Data Factory-Instanz über das Azure-Portal zu erstellen. Wählen Sie hierbei die Option **An Dashboard anheften** aus, um nach der Erstellung den schnellen Zugriff zu ermöglichen. 

Öffnen Sie nach der Erstellung Ihrer Data Factory-Instanz im Azure-Portal die zugehörige Übersichtsseite. Wählen Sie die Kachel **Erstellen und überwachen** aus, um die Seite **Erste Schritte** auf einer separaten Registerkarte zu öffnen. Sie können darauf dann mit der Erstellung Ihrer Azure-SSIS IR fortfahren.

## <a name="create-an-azure-ssis-integration-runtime"></a>Erstellen einer Azure SSIS Integration Runtime

### <a name="from-the-data-factory-overview"></a>Über die Seite mit der Übersicht über Data Factory

1. Wählen Sie auf der Seite **Let's get started** (Erste Schritte) die Kachel **Configure SSIS Integration Runtime** (SSIS Integration Runtime konfigurieren). 

   ![Kachel „Configure SSIS Integration Runtime“ (SSIS Integration Runtime konfigurieren)](./media/tutorial-create-azure-ssis-runtime-portal/configure-ssis-integration-runtime-tile.png)

1. Die übrigen Schritte zum Einrichten einer Azure-SSIS IR finden Sie im Abschnitt [Bereitstellen einer Azure-SSIS Integration Runtime](#provision-an-azure-ssis-integration-runtime). 

### <a name="from-the-authoring-ui"></a>Über die Benutzeroberfläche für die Erstellung

1. Wechseln Sie auf der Azure Data Factory-Benutzeroberfläche zur Registerkarte **Bearbeiten**, und wählen Sie **Verbindungen** aus. Wechseln Sie anschließend zur Registerkarte **Integration Runtimes**, um die vorhandenen Integration Runtimes Ihrer Data Factory-Instanz anzuzeigen. 

   ![Auswahl zum Anzeigen von vorhandenen IRs](./media/tutorial-create-azure-ssis-runtime-portal/view-azure-ssis-integration-runtimes.png)

1. Wählen Sie **Neu**, um eine Azure-SSIS IR zu erstellen. 

   ![Integration Runtime über das Menü](./media/tutorial-create-azure-ssis-runtime-portal/edit-connections-new-integration-runtime-button.png)

1. Wählen Sie im Fenster für die **Integration Runtime-Einrichtung** die Option **Lift-and-shift existing SSIS packages to execute in Azure** (Migrieren vorhandener SSIS-Pakete per Lift & Shift für die Ausführung in Azure) und dann **Weiter**. 

   ![Angeben des Integration Runtime-Typs](./media/tutorial-create-azure-ssis-runtime-portal/integration-runtime-setup-options.png)

1. Die übrigen Schritte zum Einrichten einer Azure-SSIS IR finden Sie im Abschnitt [Bereitstellen einer Azure-SSIS Integration Runtime](#provision-an-azure-ssis-integration-runtime). 

## <a name="provision-an-azure-ssis-integration-runtime"></a>Bereitstellen einer Azure-SSIS Integration Runtime

1. Führen Sie auf der Seite **Allgemeine Einstellungen** von **Integration Runtime Setup** (Integration Runtime-Setup) die unten angegebenen Schritte aus. 

   ![Allgemeine Einstellungen](./media/tutorial-create-azure-ssis-runtime-portal/general-settings.png)

   a. Geben Sie unter **Name** den Namen Ihrer Integration Runtime ein. 

   b. Geben Sie unter **Beschreibung** die Beschreibung Ihrer Integration Runtime ein. 

   c. Wählen Sie unter **Standort** den Standort für Ihre Integration Runtime aus. Es werden nur unterstützte Standorte angezeigt. Sie sollten den gleichen Standort wie für den Datenbankserver zum Hosten von SSISDB auswählen. 

   d. Wählen Sie unter **Knotengröße** die Größe des Knotens in Ihrem Integration Runtime-Cluster aus. Nur unterstützte Knotengrößen werden angezeigt. Wählen Sie eine große Knotengröße (zentrales Hochskalieren) aus, wenn Sie zahlreiche compute- oder arbeitsspeicherintensive Pakete ausführen möchten. 

   e. Wählen Sie unter **Knotenzahl** die Anzahl von Knoten in Ihrer Integration Runtime aus. Nur unterstützte Knotenzahlen werden angezeigt. Wählen Sie einen großen Cluster mit zahlreichen Knoten (horizontales Hochskalieren) aus, wenn Sie viele Pakete parallel ausführen möchten. 

   f. Wählen Sie unter **Edition/Lizenz** die SQL Server-Edition für Ihre Integration Runtime aus: Standard oder Enterprise. Wählen Sie Enterprise aus, wenn Sie erweiterte Funktionen für Ihre Integration Runtime verwenden möchten. 

   g. Wählen Sie unter **Sparen Sie Geld** die Option „Azure-Hybridvorteil“ für Ihre Integration Runtime aus: **Ja** oder **Nein**. Wählen Sie **Ja**, wenn Sie eine eigene SQL Server-Lizenz mit Software Assurance verwenden möchten, um bei der Hybridnutzung von Kostenersparnissen zu profitieren. 

   h. Klicken Sie auf **Weiter**. 

1. Führen Sie auf der Seite **SQL-Einstellungen** die unten angegebenen Schritte aus. 

   ![SQL-Einstellungen](./media/tutorial-create-azure-ssis-runtime-portal/sql-settings.png)

   a. Aktivieren Sie das Kontrollkästchen **SSIS-Katalog erstellen**, um das Bereitstellungsmodell für Pakete auszuwählen, die unter Ihrer Azure-SSIS IR ausgeführt werden. Sie wählen entweder das Projektbereitstellungsmodell aus, bei dem Pakete in der SSISDB bereitgestellt werden, die von Ihrem Datenbankserver gehostet werden, oder das Paketbereitstellungsmodell, bei dem Pakete in Dateisystemen, Dateifreigaben oder unter Azure Files bereitgestellt werden.
   
   Wenn Sie das Kontrollkästchen aktivieren, müssen Sie Ihren eigenen Datenbankserver zum Hosten der SSISDB bereitstellen, die wir in Ihrem Namen erstellen und verwalten.
   
   b. Geben Sie unter **Abonnement** das Azure-Abonnement mit dem Azure-Datenbankserver zum Hosten von SSISDB an. 

   c. Wählen Sie als **Standort** den Standort des Datenbankservers zum Hosten von SSISDB aus. Sie sollten denselben Standort wie den für die Integration Runtime auswählen.

   d. Wählen Sie unter **Katalogdatenbank-Serverendpunkt** den Endpunkt Ihres Datenbankservers zum Hosten von SSISDB aus. 
   
   Basierend auf dem ausgewählten Datenbankserver kann die SSISDB-Instanz in Ihrem Namen als Einzeldatenbank, als Teil eines Pools für elastische Datenbanken oder auf einer verwalteten Instanz erstellt werden. Der Zugriff kann über ein öffentliches Netzwerk oder durch den Beitritt zu einem virtuellen Netzwerk erfolgen. Einen Leitfaden zum Auswählen des Datenbankservertyps zum Hosten der SSISDB finden Sie unter [Vergleich zwischen SQL-Datenbank mit Einzeldatenbank/Pool für elastische Datenbanken und verwalteter SQL-Datenbank-Instanz](../data-factory/create-azure-ssis-integration-runtime.md#comparison-of-a-sql-database-single-database-elastic-pool-and-managed-instance).   

   Wenn Sie einen Azure SQL-Datenbank-Server mit VNET-Dienstendpunkten oder mit einer verwalteten Instanz mit einem privaten Endpunkt zum Hosten der SSISDB auswählen oder Zugriff auf lokale Daten ohne Konfiguration einer selbstgehosteten IR benötigen, müssen Sie die Azure-SSIS IR einem virtuellen Netzwerk hinzufügen. Weitere Informationen finden Sie unter [Erstellen von Azure-SSIS Integration Runtime in Azure Data Factory](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).

   e. Aktivieren Sie das Kontrollkästchen **Use AAD authentication with the managed identity for your ADF** (AAD-Authentifizierung mit der verwalteten Identität für Ihre ADF verwenden), um die Authentifizierungsmethode für Ihren Datenbankserver zum Hosten der SSISDB auszuwählen. Sie wählen entweder die SQL-Authentifizierung oder die Azure AD-Authentifizierung mit der verwalteten Identität für Ihre Data Factory-Instanz aus.

   Wenn Sie das Kontrollkästchen aktivieren, müssen Sie die verwaltete Identität für Ihre Data Factory-Instanz einer Azure AD-Gruppe mit Zugriffsberechtigungen für Ihren Datenbankserver hinzufügen. Weitere Informationen finden Sie unter [Erstellen von Azure-SSIS Integration Runtime in Azure Data Factory](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).
   
   f. Geben Sie unter **Administratorbenutzername** den Benutzernamen für die SQL-Authentifizierung für den Datenbankserver zum Hosten der SSISDB an. 

   g. Geben Sie unter **Administratorbenutzerkennwort** das Kennwort für die SQL-Authentifizierung für den Datenbankserver zum Hosten der SSISDB an. 

   h. Wählen Sie unter **Katalogdatenbank-Dienstebene** die Dienstebene für Ihren Datenbankserver zum Hosten der SSISDB aus. Wählen Sie den Tarif „Basic“, „Standard“ oder „Premium“ oder den Namen eines Pools für elastische Datenbanken aus.

   i. Wählen Sie **Verbindung testen** aus. Wählen Sie **Weiter** aus, wenn der Test erfolgreich ist. 

1. Führen Sie auf der Seite **Erweiterte Einstellungen** die unten angegebenen Schritte aus. 

   ![Erweiterte Einstellungen](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)

   a. Legen Sie unter **Maximale Anzahl von parallelen Ausführungen pro Knoten** die maximale Anzahl von Paketen fest, die in Ihrem Integration Runtime-Cluster pro Knoten gleichzeitig ausgeführt werden sollen. Nur unterstützte Paketzahlen werden angezeigt. Wählen Sie eine niedrigere Anzahl aus, wenn Sie mehrere Kerne verwenden möchten, um ein einzelnes großes Paket auszuführen, das compute- oder arbeitsspeicherintensiv ist. Wählen Sie eine hohe Anzahl aus, wenn Sie mindestens ein kleines Paket in einem einzelnen Kern ausführen möchten. 

   b. Geben Sie unter **Container-SAS-URI des benutzerdefinierten Setups** optional den SAS-URI (Shared Access Signature-Uniform Resource Identifier) des Azure-Blobspeichercontainers an, in dem Ihr Einrichtungsskript und die zugehörigen Dateien gespeichert sind. Weitere Informationen finden Sie unter [Anpassen des Setups für Azure-SSIS Integration Runtime](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup).
   
   c. Aktivieren Sie das Kontrollkästchen **Select a VNET for your Azure-SSIS Integration Runtime to join and allow ADF to create certain network resources** (VNET für Ihre Azure-SSIS Integration Runtime für den Beitritt auswählen und Erstellung bestimmter Netzwerkressourcen für ADF ermöglichen), um auszuwählen, ob Sie Ihre Integration Runtime-Instanz in ein virtuelles Netzwerk einbinden möchten.

   Aktivieren Sie dieses Kontrollkästchen, wenn Sie einen Azure SQL-Datenbank-Server mit VNET-Dienstendpunkten oder mit einer verwalteten Instanz mit einem privaten Endpunkt zum Hosten der SSISDB verwenden oder Zugriff auf lokale Daten ohne Konfiguration einer selbstgehosteten IR benötigen. Weitere Informationen finden Sie unter [Erstellen von Azure-SSIS Integration Runtime in Azure Data Factory](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). 
   
   d. Aktivieren Sie das Kontrollkästchen **Set up Self-Hosted Integration Runtime as a proxy for your Azure-SSIS Integration Runtime** (Selbstgehostete Integration Runtime als Proxy für Ihre Azure-SSIS Integration Runtime einrichten), um auszuwählen, ob Sie eine selbstgehostete IR als Proxy für Ihre Azure-SSIS IR konfigurieren möchten. Weitere Informationen finden Sie unter [Konfigurieren einer selbstgehosteten IR als Proxy für Azure-SSIS IR in ADF](https://docs.microsoft.com/azure/data-factory/self-hosted-integration-runtime-proxy-ssis).   

1. Klicken Sie auf **Weiter**. 

1. Überprüfen Sie auf der Seite **Zusammenfassung** alle Bereitstellungseinstellungen, lesen Sie die empfohlenen Dokumentationslinks, und wählen Sie **Fertig stellen** aus, um die Erstellung Ihrer Integration Runtime zu starten. 

   > [!NOTE]
   > Abgesehen von einer benutzerdefinierten Einrichtungszeit sollte dieser Prozess innerhalb von fünf Minuten abgeschlossen sein.
   >
   > Bei Verwendung der SSISDB stellt der Data Factory-Dienst eine Verbindung mit Ihrem Datenbankserver her, um die SSISDB vorzubereiten. 
   > 
   > Wenn Sie Azure-SSIS IR bereitstellen, werden auch die Access Redistributable-Komponente und das Azure Feature Pack für SSIS installiert. Diese Komponenten ermöglichen nicht nur die Konnektivität mit Excel-Dateien, Access-Dateien und verschiedenen Azure-Datenquellen, sondern auch mit den Datenquellen, die von integrierten Komponenten bereits unterstützt werden. Informationen zu anderen Komponenten, die Sie installieren können, finden Sie unter [Anpassen des Setups für Azure-SSIS Integration Runtime](how-to-configure-azure-ssis-ir-custom-setup.md).

1. Wechseln Sie auf der Registerkarte **Verbindungen** ggf. zu **Integration Runtimes**. Wählen Sie zum Aktualisieren des Status die Option **Aktualisieren**. 

   ![Erstellungsstatus, Schaltfläche „Aktualisieren“](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-creation-status.png)

1. Über die Links in der Spalte **Aktionen** können Sie die Integration Runtime beenden/starten, bearbeiten oder löschen. Über den letzten Link können Sie den JSON-Code für die Integration Runtime anzeigen. Die Schaltflächen zum Bearbeiten und Löschen sind nur verfügbar, wenn die IR beendet wird. 

   ![Links in der Spalte „Aktionen“](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-actions.png) 

## <a name="deploy-ssis-packages"></a>Bereitstellen von SSIS-Paketen

Bei Verwendung der SSISDB können Sie Ihre Pakete darin bereitstellen und in der Azure-SSIS IR ausführen, indem Sie SQL Server Data Tools oder SQL Server Management Studio-Tools verwenden. Mit diesen Tools wird über den zugehörigen Serverendpunkt eine Verbindung mit Ihrem Datenbankserver hergestellt: 

- Für einen Azure SQL-Datenbank-Server mit einem privaten Endpunkt lautet das Format des Serverendpunkts `<server name>.database.windows.net`.
- Für eine verwaltete Instanz mit einem privaten Endpunkt lautet das Format des Serverendpunkts `<server name>.<dns prefix>.database.windows.net`.
- Für eine verwaltete Instanz mit einem öffentlichen Endpunkt lautet das Format des Serverendpunkts `<server name>.public.<dns prefix>.database.windows.net,3342`. 

Falls Sie die SSISDB nicht verwenden, können Sie Ihre Pakete in Dateisystemen, auf Dateifreigaben oder unter Azure Files bereitstellen. Sie können sie dann über die Azure-SSIS IR ausführen, indem Sie die Befehlszeilentools `dtinstall`, `dtutil` und `dtexec` verwenden. Weitere Informationen finden Sie unter [Bereitstellen von SSIS-Paketen](/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages#deploy-packages-to-integration-services-server). 

In beiden Fällen können Sie Ihre bereitgestellten Pakete auch auf der Azure-SSIS IR ausführen, indem Sie die Aktivität „SSIS-Paket ausführen“ in Data Factory-Pipelines verwenden. Weitere Informationen finden Sie unter [Ausführen eines SSIS-Pakets mit der Aktivität „SSIS-Paket ausführen“ in Azure Data Factory](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).

Sehen Sie sich auch die folgende SSIS-Dokumentation an: 

- [Bereitstellen und Ausführen eines SSIS-Pakets in Azure](/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial) 
- [Herstellen einer Verbindung mit dem SSIS-Katalog (SSISDB) in Azure](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database) 
- [Planen der Ausführung von in Azure bereitgestellten SSIS-Paketen](/sql/integration-services/lift-shift/ssis-azure-schedule-packages) 
- [Connect to on-premises data sources with Windows Authentication](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth) (Herstellen einer Verbindung mit lokalen Datenquellen mit Windows-Authentifizierung) 

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Anpassen Ihrer Azure-SSIS Integration Runtime finden Sie im folgenden Artikel: 

> [!div class="nextstepaction"]
> [Anpassen des Setups für Azure-SSIS Integration Runtime](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup)