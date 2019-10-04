---
title: Erstellen von Azure-SSIS Integration Runtime in Azure Data Factory | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie zur Bereitstellung und Ausführung von SSIS-Paketen in Azure eine Azure-SSIS Integration Runtime in Azure Data Factory erstellen.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 09/15/2019
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 36cb4d306cd74dcde09fa55fc582a043bc324f65
ms.sourcegitcommit: a819209a7c293078ff5377dee266fa76fd20902c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/16/2019
ms.locfileid: "71010019"
---
# <a name="create-azure-ssis-integration-runtime-in-azure-data-factory"></a>Erstellen von Azure-SSIS Integration Runtime in Azure Data Factory

In diesem Tutorial werden die Schritte für die Bereitstellung einer Azure-SQL Server Integration Services (SSIS) Integration Runtime (IR) in Azure Data Factory (ADF) beschrieben. Azure-SSIS IR unterstützt die Ausführung von Paketen, die im SSIS-Katalog (SSISDB) bereitgestellt und vom Azure SQL-Datenbank-Server bzw. einer verwalteten Instanz (Projektbereitstellungsmodell) gehostet werden, und von Paketen, die in Dateisystemen/Dateifreigaben/Azure Files (Projektbereitstellungsmodell) bereitgestellt werden. Sobald Azure-SSIS IR bereitgestellt wurde, können Sie vertraute Tools wie SQL Server Data Tools (SSDT)/SQL Server Management Studio (SSMS) und Befehlszeilenprogramme wie `dtinstall`/`dtutil`/`dtexec` verwenden, um Ihre Pakete in Azure bereitzustellen und auszuführen.

Im [Tutorial: Bereitstellen von Azure-SSIS IR](tutorial-create-azure-ssis-runtime-portal.md) erfahren Sie, wie Sie eine Azure-SSIS IR über das Azure-Portal bzw. die ADF-App erstellen und optional den Azure SQL-Datenbank-Server bzw. eine verwaltete Instanz zum Hosten von SSISDB verwenden. In diesem Artikel wird das Tutorial vertieft und erläutert, wie Sie die folgenden Aktionen ausführen:

- Verwenden Sie optional Azure SQL-Datenbank-Server mit VNET-Dienstendpunkten oder einer verwalteten Instanz mit einem privaten Endpunkt zum Hosten von SSISDB. Als Voraussetzung müssen Sie Berechtigungen/Einstellungen für das virtuelle Netzwerk konfigurieren, damit Ihre Azure-SSIS IR einem virtuellen Netzwerk beitreten kann.

- Verwenden Sie optional Azure Active Directory-Authentifizierung (AAD) mit der verwalteten Identität für Ihre ADF, um eine Verbindung mit dem Azure SQL-Datenbank-Server bzw. der verwalteten Instanz herzustellen. Als Voraussetzung müssen Sie die verwaltete Identität für Ihre ADF als Datenbankbenutzer hinzufügen, der SSISDB erstellen kann.

- Optional können Sie Ihre Azure-SSIS IR mit einem virtuellen Netzwerk verknüpfen oder die selbstgehostete Integration Runtime als Proxy für Ihre Azure-SSIS IR konfigurieren, um auf lokale Daten zuzugreifen.

## <a name="overview"></a>Übersicht

Dieser Artikel zeigt verschiedene Methoden für die Bereitstellung einer Azure-SSIS IR auf:

- [Azure-Portal](#azure-portal)
- [Azure PowerShell](#azure-powershell)
- [Azure Resource Manager-Vorlage](#azure-resource-manager-template)

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

- **Azure-Abonnement**. Falls Sie noch nicht über ein Abonnement verfügen, können Sie ein [kostenloses Testkonto](https://azure.microsoft.com/pricing/free-trial/) erstellen.
- **Azure SQL-Datenbank-Server/Verwaltete Instanz (optional)** . Wenn Sie noch nicht über einen Datenbankserver verfügen, erstellen Sie einen im Azure-Portal, bevor Sie beginnen. ADF erstellt SSISDB für diesen Datenbankserver. Es empfiehlt sich, den Datenbankserver in derselben Azure-Region zu erstellen, in der sich auch die Integration Runtime befindet. Diese Konfiguration ermöglicht es der Integration Runtime, Ausführungsprotokolle in SSISDB zu schreiben, ohne Azure-Regionen zu überschreiten. 
    - Basierend auf dem ausgewählten Datenbankserver kann SSISDB in Ihrem Auftrag als einzelne Datenbank, als Teil eines Pools für elastische Datenbanken oder in einer verwalteten Instanz erstellt werden und in einem öffentlichen Netzwerk oder durch das Hinzufügen zu einem virtuellen Netzwerk zugänglich sein. Einen Leitfaden zum Auswählen des Datenbankservertyps zum Hosten von SSISDB finden Sie unter [Vergleich zwischen SQL-Datenbank mit Einzeldatenbank/Pool für elastische Datenbanken und verwalteter SQL-Datenbank-Instanz](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance). Wenn Sie den Azure SQL-Datenbank-Server mit VNET-Dienstendpunkten oder mit einer verwalteten Instanz mit einem privaten Endpunkt zum Hosten von SSISDB verwenden oder Zugriff auf lokale Daten ohne Konfiguration einer selbstgehosteten IR benötigen, müssen Sie Azure-SSIS IR einem virtuellen Netzwerk hinzufügen. Weitere Informationen finden Sie unter [Verknüpfen einer Azure-SSIS IR mit einem virtuellen Netzwerk](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network).
    - Vergewissern Sie sich, dass die Einstellung **Zugriff auf Azure-Dienste zulassen** für den Datenbankserver aktiviert ist. Dies gilt nicht, wenn Sie einen Azure SQL-Datenbank-Server mit VNET-Dienstendpunkten oder einer verwalteten Instanz mit einem privaten Endpunkt zum Hosten von SSISDB verwenden. Weitere Informationen finden Sie unter [Schützen der Azure SQL-Datenbank](../sql-database/sql-database-security-tutorial.md#create-firewall-rules). Informationen zum Aktivieren dieser Einstellung mithilfe von PowerShell finden Sie unter [New-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule).
    - Fügen Sie der Client-IP-Adressenliste in den Firewalleinstellungen für den Datenbankserver die IP-Adresse des Clientcomputers oder einen IP-Adressbereich hinzu, der die IP-Adresse des Clientcomputers enthält. Weitere Informationen finden Sie unter [Firewallregeln auf Serverebene und Datenbankebene für Azure SQL-Datenbank](../sql-database/sql-database-firewall-configure.md).
    - Sie können eine Verbindung mit dem Datenbankserver herstellen, indem Sie die SQL-Authentifizierung mit Ihren Serveradministrator-Anmeldeinformationen oder die Azure Active Directory-Authentifizierung (AAD) mit der verwalteten Identität für Ihre ADF-Instanz verwenden.  Im letztgenannten Fall müssen Sie die verwaltete Identität für die ADF einer AAD-Gruppe mit Zugriffsberechtigungen für den Datenbankserver hinzufügen. Weitere Informationen finden Sie unter [Aktivieren der AAD-Authentifizierung für die Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/enable-aad-authentication-azure-ssis-ir).
    - Vergewissern Sie sich, dass Ihr Datenbankserver noch nicht über eine SSISDB verfügt. Für die Bereitstellung einer Azure-SSIS IR wird die Verwendung einer vorhandenen SSISDB nicht unterstützt.
- **Virtuelles Azure Resource Manager-Netzwerk (optional)** . Sie benötigen ein virtuelles Azure Resource Manager-Netzwerk, wenn mindestens eine der folgenden Bedingungen zutrifft:
    - Sie hosten SSISDB auf einem Azure SQL-Datenbank-Server mit VNET-Dienstendpunkten oder einer verwalteten Instanz mit einem privaten Endpunkt.
    - Sie möchten über SSIS-Pakete, die in der Azure-SSIS IR ausgeführt werden, eine Verbindung mit lokalen Datenspeichern herstellen, ohne eine selbstgehostete IR zu konfigurieren.
- **Azure PowerShell (optional)** . Folgen Sie den Anweisungen unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/install-az-ps), wenn Sie ein PowerShell-Skript zur Bereitstellung von Azure-SSIS IR ausführen möchten.

### <a name="region-support"></a>Unterstützung für Regionen

Eine Liste mit den Azure-Regionen, in denen ADF und Azure-SSIS IR zurzeit verfügbar sind, finden Sie unter [ADF- und SSIS IR-Verfügbarkeit nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all).

### <a name="compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance"></a>Vergleich zwischen SQL-Datenbank mit Einzeldatenbank/Pool für elastische Datenbanken und verwalteter SQL-Datenbank-Instanz

In der folgenden Tabelle werden bestimmte Features des Azure SQL-Datenbank-Servers und der verwalteten Instanz in Bezug auf Azure-SSIR IR verglichen:

| Feature | Einzeldatenbank/Pool für elastische Datenbanken| Verwaltete Instanz |
|---------|--------------|------------------|
| **Zeitplanung** | SQL Server-Agent ist nicht verfügbar.<br/><br/>Informationen finden Sie unter [Planen der Ausführung von Paketen in ADF-Pipeline](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-schedule-packages?view=sql-server-2017#activity).| Agent für die verwaltete Instanz ist verfügbar. |
| **Authentifizierung** | Sie können SSISDB mit einem Benutzer für eigenständige Datenbank erstellen, der eine beliebige AAD-Gruppe mit der verwalteten Identität Ihrer ADF als ein Mitglied in der Rolle **Db_owner** darstellt.<br/><br/>Informationen finden Sie unter [Aktivieren der Azure AD-Authentifizierung zum Erstellen von SSISDB auf einem Azure SQL-Datenbank-Server](enable-aad-authentication-azure-ssis-ir.md#enable-azure-ad-on-azure-sql-database). | Sie können SSISDB mit einem Benutzer für eigenständige Datenbank erstellen, der die verwaltete Identität Ihrer ADF darstellt. <br/><br/>Informationen finden Sie unter [Aktivieren der Azure AD-Authentifizierung zum Erstellen von SSISDB in einer verwalteten Azure SQL-Datenbank-Instanz](enable-aad-authentication-azure-ssis-ir.md#enable-azure-ad-on-azure-sql-database-managed-instance). |
| **Dienstebene** | Wenn Sie Azure-SSIS IR mit Ihrem Azure SQL-Datenbank-Server erstellen, können Sie die Dienstebene für SSISDB auswählen. Es gibt mehrere Dienstebenen. | Wenn Sie Azure-SSIS IR mit Ihrer verwalteten Instanz erstellen, können Sie die Dienstebene für SSISDB nicht auswählen. Alle Datenbanken in Ihrer verwalteten Instanz nutzen dieselbe Ressource, die dieser Instanz zugeordnet ist, gemeinsam. |
| **Virtuelles Netzwerk** | Unterstützt nur virtuelle Azure Resource Manager-Netzwerke, denen Ihre Azure-SSIS IR beitreten kann, wenn Sie den Azure SQL-Datenbank-Server mit VNET-Dienstendpunkten verwenden oder Zugriff auf lokale Datenspeicher benötigen, ohne eine selbstgehostete IR zu konfigurieren. | Unterstützt nur virtuelle Azure Resource Manager-Netzwerke, denen Ihre Azure-SSIS IR beitreten kann. Das virtuelle Netzwerk ist erforderlich, wenn Sie keinen öffentlichen Endpunkt für Ihre verwaltete Instanz aktivieren.<br/><br/>Wenn Sie Ihre Azure-SSIS IR mit demselben virtuellen Netzwerk verknüpfen wie Ihre verwaltete Instanz, stellen Sie sicher, dass sich Ihre Azure-SSIS IR in einem anderen Subnetz als Ihre verwaltete Instanz befindet. Wenn Sie Ihre Azure-SSIS IR mit einem anderen virtuellen Netzwerk verknüpfen als Ihre verwaltete Instanz, empfiehlt sich ein Peering virtueller Netzwerke oder eine Verbindung zwischen den virtuellen Netzwerken. Weitere Informationen finden Sie unter [Herstellen einer Verbindung zwischen einer Anwendung und einer verwalteten Azure SQL-Datenbank-Instanz](../sql-database/sql-database-managed-instance-connect-app.md). |
| **Verteilte Transaktionen** | Unterstützt über elastische Transaktionen. MS DTC-Transaktionen (Microsoft Distributed Transaction Coordinator) werden nicht unterstützt. Wenn Ihre SSIS-Pakete MSDTC verwenden, um verteilte Transaktionen zu koordinieren, sollten Sie eine Migration zu elastischen Transaktionen für Azure SQL-Datenbank erwägen. Weitere Informationen finden Sie unter [Verteilte Transaktionen über Clouddatenbanken](../sql-database/sql-database-elastic-transactions-overview.md). | Nicht unterstützt. |
| | | |

## <a name="azure-portal"></a>Azure-Portal

In diesem Abschnitt erstellen Sie mithilfe des Azure-Portals (insbesondere der ADF-Benutzeroberfläche/App) eine Azure-SSIS IR.

### <a name="create-a-data-factory"></a>Erstellen einer Data Factory

Zum Erstellen Ihrer ADF über das Azure-Portal befolgen Sie die Schritt-für-Schritt-Anleitung im Artikel [Erstellen der ADF über die Benutzeroberfläche](https://docs.microsoft.com/azure/data-factory/quickstart-create-data-factory-portal#create-a-data-factory), und wählen Sie dabei **An Dashboard anheften** aus, um nach Erstellung den schnellen Zugriff zuzulassen. 

Sobald Ihre ADF erstellt ist, öffnen Sie die zugehörige Übersichtsseite im Azure-Portal, und klicken Sie auf die Kachel **Erstellen und überwachen**, um die Seite **Let's get started** (Erste Schritte) auf einer separaten Registerkarte zu öffnen, wo Sie mit der Erstellung Ihrer Azure-SSIS IR fortfahren können.   

### <a name="provision-an-azure-ssis-integration-runtime"></a>Bereitstellen einer Azure SSIS Integration Runtime

1. Klicken Sie auf der Seite **Let's get started** (Erste Schritte) die Kachel **Configure SSIS Integration Runtime** (SSIS Integration Runtime konfigurieren).

   ![Kachel „Configure SSIS Integration Runtime“ (SSIS Integration Runtime konfigurieren)](./media/tutorial-create-azure-ssis-runtime-portal/configure-ssis-integration-runtime-tile.png)

2. Führen Sie auf der Seite **Allgemeine Einstellungen** von **Integration Runtime Setup** (Integration Runtime-Setup) die folgenden Schritte aus:

   ![Allgemeine Einstellungen](./media/tutorial-create-azure-ssis-runtime-portal/general-settings.png)

    a. Geben Sie unter **Name** den Namen Ihrer Integration Runtime ein.

    b. Geben Sie unter **Beschreibung** die Beschreibung Ihrer Integration Runtime ein.

    c. Wählen Sie unter **Standort** den Standort für Ihre Integration Runtime aus. Es werden nur unterstützte Standorte angezeigt. Sie sollten den gleichen Standort wie für den Datenbankserver zum Hosten von SSISDB auswählen.

    d. Wählen Sie unter **Knotengröße** die Größe des Knotens in Ihrem Integration Runtime-Cluster aus. Nur unterstützte Knotengrößen werden angezeigt. Wählen Sie eine große Knotengröße (zentrales Hochskalieren) aus, wenn Sie zahlreiche compute-/arbeitsspeicherintensive Pakete ausführen möchten.

    e. Wählen Sie unter **Knotenzahl** die Anzahl von Knoten in Ihrer Integration Runtime aus. Nur unterstützte Knotenzahlen werden angezeigt. Wählen Sie einen großen Cluster mit zahlreichen Knoten (horizontales Hochskalieren) aus, wenn Sie viele Pakete parallel ausführen möchten.

    f. Wählen Sie unter **Edition/Lizenz** die SQL Server-Edition/-Lizenz für Ihre Integration Runtime aus: Standard oder Enterprise. Wählen Sie Enterprise aus, wenn Sie erweiterte Funktionen oder Premium-Funktionen für Ihre Integration Runtime verwenden möchten.

    g. Wählen Sie unter **Sparen Sie Geld** die Option „Azure-Hybridvorteil“ (Azure Hybrid Benefit, AHB) für Ihre Integration Runtime aus: „Ja“ oder „Nein“. Wählen Sie „Ja“, wenn Sie eine eigene SQL Server-Lizenz mit Software Assurance verwenden möchten, um bei der Hybridnutzung von Kostenersparnissen zu profitieren.

    h. Klicken Sie auf **Weiter**.

3. Führen Sie auf der Seite **SQL-Einstellungen** die folgenden Schritte aus:

   ![SQL-Einstellungen](./media/tutorial-create-azure-ssis-runtime-portal/sql-settings.png)

    a. Wählen Sie mit dem Kontrollkästchen **SSIS-Katalog erstellen** das Bereitstellungsmodell für Pakete aus, die unter Ihrer Azure-SSIS IR ausgeführt werden: Projektbereitstellungsmodell, bei dem Pakete in SSISDB bereitgestellt werden, die von Ihrem Datenbankserver gehostet werden, oder Paketbereitstellungsmodell, bei dem Pakete in Ihren Dateisystemen/Dateifreigaben/Azure Files bereitgestellt werden. Wenn Sie das Kontrollkästchen aktivieren, müssen Sie Ihren eigenen Datenbankserver zum Hosten von SSISDB bereitstellen, die wir in Ihrem Namen erstellen und verwalten.
   
    b. Geben Sie unter **Abonnement** das Azure-Abonnement mit dem Azure-Datenbankserver zum Hosten von SSISDB an. 

    c. Wählen Sie als **Standort** den Standort des Datenbankservers zum Hosten von SSISDB aus. Sie sollten denselben Standort wie den für die Integration Runtime auswählen. 

    d. Wählen Sie unter **Katalogdatenbank-Serverendpunkt** den Endpunkt Ihres Datenbankservers zum Hosten von SSISDB aus. Basierend auf dem ausgewählten Datenbankserver kann SSISDB in Ihrem Auftrag als einzelne Datenbank, als Teil eines Pools für elastische Datenbanken oder in einer verwalteten Instanz erstellt werden und in einem öffentlichen Netzwerk oder durch das Hinzufügen zu einem virtuellen Netzwerk zugänglich sein. Einen Leitfaden zum Auswählen des Datenbankservertyps zum Hosten von SSISDB finden Sie unter [Vergleich zwischen SQL-Datenbank mit Einzeldatenbank/Pool für elastische Datenbanken und verwalteter SQL-Datenbank-Instanz](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance). Wenn Sie den Azure SQL-Datenbank-Server mit VNET-Dienstendpunkten oder mit einer verwalteten Instanz mit einem privaten Endpunkt zum Hosten von SSISDB auswählen oder Zugriff auf lokale Daten ohne Konfiguration einer selbstgehosteten IR benötigen, müssen Sie Azure-SSIS IR einem virtuellen Netzwerk hinzufügen. Weitere Informationen finden Sie unter [Verknüpfen von Azure-SSIS IR mit einem virtuellen Netzwerk](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network). 

    e. Wählen Sie mit dem Kontrollkästchen **AAD-Authentifizierung verwenden...** die Authentifizierungsmethode für Ihren Datenbankserver zum Hosten von SSISDB aus: SQL-Authentifizierung oder AAD-Authentifizierung mit der verwalteten Identität für Ihre ADF. Wenn Sie dieses Kontrollkästchen aktivieren, müssen Sie die verwaltete Identität für die ADF einer AAD-Gruppe mit Zugriffsberechtigungen für Ihren Datenbankserver hinzufügen. Weitere Informationen finden Sie unter [Aktivieren der AAD-Authentifizierung für die Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/enable-aad-authentication-azure-ssis-ir). 

    f. Geben Sie unter **Administratorbenutzername** den Benutzernamen für die SQL-Authentifizierung für den Datenbankserver zum Hosten von SSISDB an. 

    g. Geben Sie unter **Administratorbenutzerkennwort** das Kennwort für die SQL-Authentifizierung für den Datenbankserver zum Hosten von SSISDB an. 

    h. Wählen Sie unter **Katalogdatenbank-Dienstebene** die Dienstebene für Ihren Datenbankserver zum Hosten von SSISDB aus: Basic/Standard/Premium oder Name des Pools für elastische Datenbanken. 

    i. Klicken Sie auf **Verbindung testen** und bei erfolgreichem Test auf **Weiter**. 

4. Führen Sie auf der Seite **Erweiterte Einstellungen** die folgenden Schritte aus:

    ![Erweiterte Einstellungen](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)

    a. Legen Sie unter **Maximale Anzahl von parallelen Ausführungen pro Knoten** die maximale Anzahl von Paketen fest, die in Ihrem Integration Runtime-Cluster pro Knoten gleichzeitig ausgeführt werden sollen. Nur unterstützte Paketzahlen werden angezeigt. Wählen Sie eine niedrigere Anzahl aus, wenn Sie mehrere Kerne verwenden möchten, um ein einzelnes großes/umfangreiches Paket auszuführen, das compute-/arbeitsspeicherintensiv ist. Wählen Sie eine hohe Anzahl aus, wenn Sie mindestens ein kleines Paket in einem einzelnen Kern ausführen möchten.

    b. Geben Sie unter **Container-SAS-URI des benutzerdefinierten Setups** optional den SAS-URI (Shared Access Signature, Uniform Resource Identifier) Ihres Azure Storage Blob-Containers an, in dem Ihr Einrichtungsskript und die zugehörigen Dateien gespeichert sind. Weitere Informationen finden Sie unter [Benutzerdefiniertes Setup von Azure-SSIS-Integration Runtime](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup).

5. Geben Sie mithilfe des Kontrollkästchens **Virtuelles Netzwerk auswählen...** an, ob Ihre Integration Runtime einem virtuellen Netzwerk hinzugefügt werden soll. Aktivieren Sie es, wenn Sie den Azure SQL-Datenbank-Server mit VNET-Dienstendpunkten/mit verwalteter Instanz mit einem privaten Endpunkt zum Hosten von SSISDB verwenden oder Zugriff auf lokale Daten benötigen, d. h. in Ihren SSIS-Paketen über lokale Datenquellen/-ziele verfügen, ohne eine selbstgehostete IR zu konfigurieren. Weitere Informationen finden Sie unter [Verknüpfen von Azure-SSIS IR mit einem virtuellen Netzwerk](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network). Falls Sie es nicht aktivieren, führen Sie die folgenden Schritte aus:

   ![Erweiterte Einstellungen mit virtuellem Netzwerk](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-vnet.png)

    a. Wählen Sie unter **Abonnement** das Azure-Abonnement aus, das Ihr virtuelles Netzwerk enthält.

    b. Für **Standort** ist derselbe Standort wie für Ihre Integration Runtime ausgewählt.

    c. Wählen Sie für **Typ** den Typ des virtuellen Netzwerks aus: „Klassisch“ oder „Azure Resource Manager“. Sie sollten das virtuelle Azure Resource Manager-Netzwerk auswählen, da das klassische virtuelle Netzwerk bald veraltet sein wird.

    d. Wählen Sie unter **VNET-Name** den Namen des virtuellen Netzwerks aus. Dieses virtuelle Netzwerk muss dasselbe virtuelle Netzwerk sein, das für den Azure SQL-Datenbank-Server mit VNET-Dienstendpunkten/mit verwalteter Instanz in einem virtuellen Netzwerk zum Hosten von SSISDB verwendet wird, oder das mit dem lokalen Netzwerk verbundene sein.

    e. Wählen Sie unter **Subnetzname** den Namen des Subnetzes für Ihr virtuelles Netzwerk aus. Dieses virtuelle Netzwerk sollte ein anderes Subnetz sein als das für die verwaltete Instanz in einem virtuellen Netzwerk zum Hosten von SSISDB verwendete.

6. Wählen Sie mit dem Kontrollkästchen **Set up Self-Hosted...** (Selbstgehostete Integration Runtime als Proxy für die Azure-SSIS Integration Runtime einrichten) aus, ob Sie eine selbstgehostete IR als Proxy für die Azure-SSIS IR konfigurieren möchten. Informationen dazu finden Sie unter [Einrichten einer selbstgehosteten IR als Proxy](https://docs.microsoft.com/azure/data-factory/self-hosted-integration-runtime-proxy-ssis). Falls Sie es nicht aktivieren, führen Sie die folgenden Schritte aus:

   ![Erweiterte Einstellungen bei einer selbstgehosteten IR](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-shir.png)

    a. Wählen Sie für die **selbstgehostete Integration Runtime** Ihre vorhandene selbstgehostete IR als Proxy für Azure-SSIS IR aus.

    b. Wählen Sie für den **verknüpften Dienst des Stagingspeichers** Ihren vorhandenen, mit Azure Blob Storage verknüpften Dienst aus, oder erstellen Sie einen neuen Dienst für den Stagingprozess.

    c. Geben Sie für den **Stagingpfad** einen Blobcontainer in Ihrem ausgewählten Azure Blob Storage aus, oder lassen Sie das Feld leer, um einen Standardpfad für den Stagingprozess zu verwenden.

7. Klicken Sie auf die **VNet-Überprüfung** und/oder **Weiter**. 

8. Überprüfen Sie auf der Seite **Zusammenfassung** alle Bereitstellungseinstellungen, lesen Sie die empfohlenen Dokumentationslinks, und klicken Sie auf **Fertig stellen**, um die Erstellung ihrer Integration Runtime zu starten.

    > [!NOTE]
    > Abgesehen von einer benutzerdefinierten Einrichtungszeit sollte dieser Prozess innerhalb von 5 Minuten abgeschlossen sein. Es kann aber etwa 20 bis 30 Minuten dauern, bis Azure-SSIS IR einem virtuellen Netzwerk beitritt.
    >
    > Bei Verwendung von SSISDB stellt der ADF-Dienst eine Verbindung mit Ihrem Datenbankserver her, um SSISDB vorzubereiten. Außerdem konfiguriert er Berechtigungen und Einstellungen für das virtuelle Netzwerk (sofern angegeben) und verknüpft die Azure-SSIS IR mit dem virtuellen Netzwerk.
    > 
    > Wenn Sie Azure-SSIS IR bereitstellen, werden auch das Access Redistributable-Komponente und Azure Feature Pack für SSIS installiert. Diese Komponenten ermöglichen nicht nur die Konnektivität mit Excel- und Access-Dateien und verschiedenen Azure-Datenquellen, sondern auch mit den Datenquellen, die bereits von integrierten Komponenten unterstützt werden. Sie können auch zusätzliche Komponenten installieren. Weitere Informationen dazu finden Sie unter [Benutzerdefiniertes Setup für Azure-SSIS IR](how-to-configure-azure-ssis-ir-custom-setup.md).

7. Wechseln Sie im Fenster **Verbindungen** ggf. zu **Integration Runtimes**. Klicken Sie zum Aktualisieren des Status auf **Aktualisieren**.

   ![Status der Erstellung](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-creation-status.png)

8. Verwenden Sie die Links in der Spalte **Aktionen**, um die Integration Runtime zu überwachen, zu beenden und zu starten, zu bearbeiten oder zu löschen. Über den letzten Link können Sie den JSON-Code für die Integration Runtime anzeigen. Die Schaltflächen zum Bearbeiten und Löschen sind nur verfügbar, wenn die IR beendet wird.

   ![Azure SSIS IR – Aktionen](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-actions.png)

### <a name="azure-ssis-integration-runtimes-in-the-portal"></a>Azure SSIS Integration Runtimes im Portal

1. Wechseln Sie auf der Azure Data Factory-Benutzeroberfläche zur Registerkarte **Bearbeiten**, klicken Sie auf **Verbindungen**, und wechseln Sie dann zur Registerkarte **Integration Runtimes**, um vorhandene Integration Runtimes in Ihrer Data Factory anzuzeigen.

   ![Anzeigen vorhandener IRs](./media/tutorial-create-azure-ssis-runtime-portal/view-azure-ssis-integration-runtimes.png)

2. Klicken Sie auf **Neu**, um eine neue Azure SSIS IR zu erstellen.

   ![Integration Runtime über das Menü](./media/tutorial-create-azure-ssis-runtime-portal/edit-connections-new-integration-runtime-button.png)

3. Klicken Sie zum Erstellen einer Azure SSIS Integration Runtime auf **Neu** wie in der Abbildung gezeigt.

4. Klicken Sie im Fenster für die Integration Runtime-Einrichtung auf **Lift-and-shift existing SSIS packages to execute in Azure** (Migrieren vorhandener SSIS-Pakete per Lift & Shift für die Ausführung in Azure), und klicken Sie auf **Weiter**.

   ![Angeben des Integration Runtime-Typs](./media/tutorial-create-azure-ssis-runtime-portal/integration-runtime-setup-options.png)

5. Die übrigen Schritte zum Einrichten einer Azure SSIS IR finden Sie im Abschnitt [Bereitstellen einer Azure SSIS Integration Runtime](#provision-an-azure-ssis-integration-runtime).

## <a name="azure-powershell"></a>Azure PowerShell

In diesem Abschnitt verwenden Sie Azure PowerShell zum Erstellen einer Azure-SSIS IR.

### <a name="create-variables"></a>Erstellen von Variablen

Kopieren Sie das folgende Skript, und fügen Sie es ein. Geben Sie Werte für die Variablen an. 

```powershell
### Azure Data Factory information
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$"
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
# Data factory name - Must be globally unique
$DataFactoryName = "[your data factory name]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$DataFactoryLocation = "EastUS"

### Azure-SSIS integration runtime information - This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "[your Azure-SSIS IR name]"
$AzureSSISDescription = "[your Azure-SSIS IR description]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$AzureSSISLocation = "EastUS"
# For supported node sizes, see https://azure.microsoft.com/pricing/details/data-factory/ssis/
$AzureSSISNodeSize = "Standard_D8_v3"
# 1-10 nodes are currently supported
$AzureSSISNodeNumber = 2
# Azure-SSIS IR edition/license info: Standard or Enterprise
$AzureSSISEdition = "Standard" # Standard by default, while Enterprise lets you use advanced/premium features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "LicenseIncluded" # LicenseIncluded by default, while BasePrice lets you bring your own on-premises SQL Server license with Software Assurance to earn cost savings from Azure Hybrid Benefit (AHB) option
# For a Standard_D1_v2 node, up to 4 parallel executions per node are supported, but for other nodes, up to (2 x number of cores) are currently supported
$AzureSSISMaxParallelExecutionsPerNode = 8
# Custom setup info
$SetupScriptContainerSasUri = "" # OPTIONAL to provide SAS URI of blob container where your custom setup script and its associated files are stored
# Virtual network info: Classic or Azure Resource Manager
$VnetId = "[your virtual network resource ID or leave it empty]" # REQUIRED if you use Azure SQL Database server with virtual network service endpoints/Managed Instance with a private endpoint/on-premises data without configuring Self-Hosted IR, Azure Resource Manager virtual network is recommended, Classic virtual network will be deprecated soon
$SubnetName = "[your subnet name or leave it empty]" # WARNING: Please use the same subnet as the one used for your Azure SQL Database server with virtual network service endpoints or a different subnet than the one used for your Managed Instance with a private endpoint

### SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name.database.windows.net or Managed Instance name.DNS prefix.database.windows.net or Managed Instance name.public.DNS prefix.database.windows.net,3342 or leave it empty if you do not use SSISDB]" # WARNING: If you use SSISDB, please ensure that there is no existing SSISDB on your database server, so we can prepare and manage one on your behalf
# Authentication info: SQL or Azure Active Directory (AAD)
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication or leave it empty for AAD authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication or leave it empty for AAD authentication]"
# For the basic pricing tier, specify "Basic", not "B" - For standard/premium/elastic pool tiers, specify "S0", "S1", "S2", "S3", etc., see https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-database-server
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>) for Azure SQL Database server or leave it empty for Managed Instance]"
```

### <a name="sign-in-and-select-subscription"></a>Anmelden und Auswählen des Abonnements

Fügen Sie dem Skript zum Anmelden bei Ihrem Azure-Abonnement und Auswählen des Abonnements den folgenden Code hinzu:

```powershell
Connect-AzAccount
Select-AzSubscription -SubscriptionName $SubscriptionName
```

### <a name="validate-the-connection-to-database-server"></a>Überprüfen der Verbindung mit dem Datenbankserver

Fügen Sie das folgende Skript hinzu, um den Azure SQL-Datenbank-Server bzw. die verwaltete Instanz zu überprüfen.

```powershell
# Validate only if you use SSISDB and do not use VNet or AAD authentication
if(![string]::IsNullOrEmpty($SSISDBServerEndpoint))
{
    if([string]::IsNullOrEmpty($VnetId) -and [string]::IsNullOrEmpty($SubnetName))
    {
        if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) -and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword))
        {
            $SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID=" + $SSISDBServerAdminUserName + ";Password=" + $SSISDBServerAdminPassword
            $sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
            Try
            {
                $sqlConnection.Open();
            }
            Catch [System.Data.SqlClient.SqlException]
            {
                Write-Warning "Cannot connect to your Azure SQL Database server, exception: $_";
                Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
                $yn = Read-Host
                if(!($yn -ieq "Y"))
                {
                    Return;
                }
            }
        }
    }
}
```

### <a name="configure-virtual-network"></a>Konfigurieren eines virtuellen Netzwerks

Fügen Sie das folgende Skript hinzu, um Berechtigungen/Einstellungen für das virtuelle Netzwerk für die Azure SSIS Integration Runtime automatisch zu konfigurieren, die Sie einbinden möchten.

```powershell
# Make sure to run this script against the subscription to which the virtual network belongs
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    # Register to the Azure Batch resource provider
    $BatchApplicationId = "ddbf3205-c6bd-46ae-8127-60eb93363864"
    $BatchObjectId = (Get-AzADServicePrincipal -ServicePrincipalName $BatchApplicationId).Id
    Register-AzResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
    Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign the VM contributor role to Microsoft.Batch
        New-AzRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}
```

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit dem Befehl [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) eine [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md). Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und als Gruppe verwaltet werden.

Wenn die Ressourcengruppe bereits vorhanden ist, müssen Sie diesen Code nicht in das Skript kopieren. 

```powershell
New-AzResourceGroup -Location $DataFactoryLocation -Name $ResourceGroupName
```

### <a name="create-a-data-factory"></a>Erstellen einer Data Factory

Führen Sie den folgenden Befehl aus, um eine Data Factory zu erstellen:

```powershell
Set-AzDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName
```

### <a name="create-an-integration-runtime"></a>Erstellen einer Integration Runtime

Führen Sie die folgenden Befehle aus, um eine Azure-SSIS-Integration Runtime zu erstellen, die SSIS-Pakete in Azure ausführt.

Wenn Sie SSISDB nicht verwenden, können Sie die Parameter CatalogServerEndpoint, CatalogPricingTier und CatalogAdminCredential auslassen.

Wenn Sie weder den Azure SQL-Datenbank-Server mit VNET-Dienstendpunkten/mit verwalteter Instanz mit einem privaten Endpunkt zum Hosten von SSISDB verwenden noch Zugriff auf lokale Daten benötigen, können Sie die Parameter „VNetId“ und „Subnet“ weglassen oder dafür leere Werte übergeben. Sie können sie auch auslassen, wenn Sie die selbstgehostete IR als Proxy für Ihren Azure-SSIS IR für den Zugriff auf lokale Daten konfigurieren. Andernfalls können Sie sie nicht weglassen und müssen gültige Werte aus der Konfiguration des virtuellen Netzwerks übergeben, wie unter [Verknüpfen einer Azure-SSIS Integration Runtime mit einem virtuellen Netzwerk](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network) beschrieben.

Wenn Sie eine verwaltete Instanz zum Hosten von SSISDB verwenden, können Sie den Parameter CatalogPricingTier weglassen oder dafür einen leeren Wert übergeben. Andernfalls können Sie ihn nicht weggelassen und müssen einen gültigen Wert aus der Liste der für Azure SQL-Datenbank unterstützten Tarife übergeben, wie unter [Ressourceneinschränkungen für Azure SQL-Datenbank](../sql-database/sql-database-resource-limits.md) beschrieben.

Wenn Sie die Azure Active Directory-Authentifizierung (AAD) mit der verwalteten Identität für Ihre ADF verwenden, um eine Verbindung mit dem Datenbankserver herzustellen, können Sie den Parameter „CatalogAdminCredential“ auslassen, aber Sie müssen die verwaltete Identität für Ihre ADF einer AAD-Gruppe mit Zugriffsberechtigungen für den Datenbankserver hinzufügen, wie unter [Aktivieren der AAD-Authentifizierung für die Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/enable-aad-authentication-azure-ssis-ir) beschrieben. Andernfalls können Sie ihn nicht auslassen, sondern müssen ein gültiges Objekt, gebildet aus Ihrem Administratorbenutzername und dem zugehörigen Kennwort für den Server, für die SQL-Authentifizierung übergeben.

```powershell
Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Name $AzureSSISName `
                                           -Description $AzureSSISDescription `
                                           -Type Managed `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                           -VnetId $VnetId `
                                           -Subnet $SubnetName
       
# Add CatalogServerEndpoint, CatalogPricingTier, and CatalogAdminCredential parameters if you use SSISDB
if(![string]::IsNullOrEmpty($SSISDBServerEndpoint))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -CatalogServerEndpoint $SSISDBServerEndpoint `
                                               -CatalogPricingTier $SSISDBPricingTier

    if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) –and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword)) # Add CatalogAdminCredential parameter if you do not use AAD authentication
    {
        $secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
        $serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)

        Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                                   -DataFactoryName $DataFactoryName `
                                                   -Name $AzureSSISName `
                                                   -CatalogAdminCredential $serverCreds
    }
}

# Add SetupScriptContainerSasUri parameter if you use custom setup
if(![string]::IsNullOrEmpty($SetupScriptContainerSasUri))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -SetupScriptContainerSasUri $SetupScriptContainerSasUri
}
```

### <a name="start-integration-runtime"></a>Starten der Integration Runtime

Führen Sie die folgenden Befehle aus, um die Azure SSIS Integration Runtime zu starten:

```powershell
write-host("##### Starting #####")
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")
```

> [!NOTE]
> Abgesehen von einer benutzerdefinierten Einrichtungszeit sollte dieser Prozess innerhalb von 5 Minuten abgeschlossen sein. Es kann aber etwa 20 bis 30 Minuten dauern, bis Azure-SSIS IR einem virtuellen Netzwerk beitritt.
>
> Bei Verwendung von SSISDB stellt der ADF-Dienst eine Verbindung mit Ihrem Datenbankserver her, um SSISDB vorzubereiten. Außerdem konfiguriert er Berechtigungen und Einstellungen für das virtuelle Netzwerk (sofern angegeben) und verknüpft die Azure-SSIS IR mit dem virtuellen Netzwerk.
> 
> Wenn Sie Azure-SSIS IR bereitstellen, werden auch das Access Redistributable-Komponente und Azure Feature Pack für SSIS installiert. Diese Komponenten ermöglichen nicht nur die Konnektivität mit Excel- und Access-Dateien und verschiedenen Azure-Datenquellen, sondern auch mit den Datenquellen, die bereits von integrierten Komponenten unterstützt werden. Sie können auch weitere Komponenten installieren. Weitere Informationen dazu finden Sie unter [Benutzerdefiniertes Setup für Azure-SSIS IR](how-to-configure-azure-ssis-ir-custom-setup.md).

### <a name="full-script"></a>Vollständiges Skript

Hier ist das vollständige Skript, das eine Azure-SSIS-Integration Runtime erstellt.

```powershell
### Azure Data Factory information
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$"
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
# Data factory name - Must be globally unique
$DataFactoryName = "[your data factory name]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$DataFactoryLocation = "EastUS"

### Azure-SSIS integration runtime information - This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "[your Azure-SSIS IR name]"
$AzureSSISDescription = "[your Azure-SSIS IR description]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$AzureSSISLocation = "EastUS"
# For supported node sizes, see https://azure.microsoft.com/pricing/details/data-factory/ssis/
$AzureSSISNodeSize = "Standard_D8_v3"
# 1-10 nodes are currently supported
$AzureSSISNodeNumber = 2
# Azure-SSIS IR edition/license info: Standard or Enterprise
$AzureSSISEdition = "Standard" # Standard by default, while Enterprise lets you use advanced/premium features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "LicenseIncluded" # LicenseIncluded by default, while BasePrice lets you bring your own on-premises SQL Server license with Software Assurance to earn cost savings from Azure Hybrid Benefit (AHB) option
# For a Standard_D1_v2 node, up to 4 parallel executions per node are supported, but for other nodes, up to (2 x number of cores) are currently supported
$AzureSSISMaxParallelExecutionsPerNode = 8
# Custom setup info
$SetupScriptContainerSasUri = "" # OPTIONAL to provide SAS URI of blob container where your custom setup script and its associated files are stored
# Virtual network info: Classic or Azure Resource Manager
$VnetId = "[your virtual network resource ID or leave it empty]" # REQUIRED if you use Azure SQL Database server with virtual network service endpoints/Managed Instance with a private endpoint/on-premises data without configuring Self-Hosted IR, Azure Resource Manager virtual network is recommended, Classic virtual network will be deprecated soon
$SubnetName = "[your subnet name or leave it empty]" # WARNING: Please use the same subnet as the one used with your Azure SQL Database server with virtual network service endpoints or a different subnet than the one used for your Managed Instance with a private endpoint

### SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name.database.windows.net or Managed Instance name.DNS prefix.database.windows.net or Managed Instance name.public.DNS prefix.database.windows.net,3342 or leave it empty if you do not use SSISDB]" # WARNING: If you use SSISDB, please ensure that there is no existing SSISDB on your database server, so we can prepare and manage one on your behalf
# Authentication info: SQL or Azure Active Directory (AAD)
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication or leave it empty for AAD authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication or leave it empty for AAD authentication]"
# For the basic pricing tier, specify "Basic", not "B" - For standard/premium/elastic pool tiers, specify "S0", "S1", "S2", "S3", etc., see https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-database-server
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>) for Azure SQL Database server or leave it empty for Managed Instance]"

### Sign in and select subscription
Connect-AzAccount
Select-AzSubscription -SubscriptionName $SubscriptionName

### Validate the connection to database server
# Validate only if you use SSISDB and do not use VNet or AAD authentication
if(![string]::IsNullOrEmpty($SSISDBServerEndpoint))
{
    if([string]::IsNullOrEmpty($VnetId) -and [string]::IsNullOrEmpty($SubnetName))
    {
        if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) -and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword))
        {
            $SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID=" + $SSISDBServerAdminUserName + ";Password=" + $SSISDBServerAdminPassword
            $sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
            Try
            {
                $sqlConnection.Open();
            }
            Catch [System.Data.SqlClient.SqlException]
            {
                Write-Warning "Cannot connect to your Azure SQL Database server, exception: $_";
                Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
                $yn = Read-Host
                if(!($yn -ieq "Y"))
                {
                    Return;
                }
            }
        }
    }
}

### Configure virtual network
# Make sure to run this script against the subscription to which the virtual network belongs
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    # Register to the Azure Batch resource provider
    $BatchApplicationId = "ddbf3205-c6bd-46ae-8127-60eb93363864"
    $BatchObjectId = (Get-AzADServicePrincipal -ServicePrincipalName $BatchApplicationId).Id
    Register-AzResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
    Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign the VM contributor role to Microsoft.Batch
        New-AzRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}

### Create a data factory
Set-AzDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName

### Create an integration runtime
Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Name $AzureSSISName `
                                           -Description $AzureSSISDescription `
                                           -Type Managed `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                           -VnetId $VnetId `
                                           -Subnet $SubnetName
       
# Add CatalogServerEndpoint, CatalogPricingTier, and CatalogAdminCredential parameters if you use SSISDB
if(![string]::IsNullOrEmpty($SSISDBServerEndpoint))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -CatalogServerEndpoint $SSISDBServerEndpoint `
                                               -CatalogPricingTier $SSISDBPricingTier

    if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) –and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword)) # Add CatalogAdminCredential parameter if you do not use AAD authentication
    {
        $secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
        $serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)

        Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                                   -DataFactoryName $DataFactoryName `
                                                   -Name $AzureSSISName `
                                                   -CatalogAdminCredential $serverCreds
    }
}

# Add SetupScriptContainerSasUri parameter when you use custom setup
if(![string]::IsNullOrEmpty($SetupScriptContainerSasUri))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -SetupScriptContainerSasUri $SetupScriptContainerSasUri
}

### Start integration runtime
write-host("##### Starting #####")
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")
```

## <a name="azure-resource-manager-template"></a>Azure Resource Manager-Vorlage

In diesem Abschnitt verwenden Sie eine Azure Resource Manager-Vorlage, um eine Azure-SSIS-Integration Runtime zu erstellen. Die folgende exemplarische Vorgehensweise kann als Beispiel dienen:

1. Erstellen Sie eine JSON-Datei mit der folgenden Azure Resource Manager-Vorlage. Ersetzen Sie die Werte in den eckigen Klammern (Platzhalter) durch Ihre eigenen Werte.

    ```json
    {
      "contentVersion": "1.0.0.0",
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {},
        "variables": {},
        "resources": [{
            "name": "<Specify a name for your data factory>",
            "apiVersion": "2018-06-01",
            "type": "Microsoft.DataFactory/factories",
            "location": "East US",
            "properties": {},
            "resources": [{
                "type": "integrationruntimes",
                "name": "<Specify a name for your Azure-SSIS IR>",
                "dependsOn": [ "<The name of the data factory you specified at the beginning>" ],
                "apiVersion": "2018-06-01",
                "properties": {
                    "type": "Managed",
                    "typeProperties": {
                        "computeProperties": {
                            "location": "East US",
                            "nodeSize": "Standard_D8_v3",
                            "numberOfNodes": 1,
                            "maxParallelExecutionsPerNode": 8
                        },
                        "ssisProperties": {
                            "catalogInfo": {
                                "catalogServerEndpoint": "<Azure SQL Database server name>.database.windows.net",
                                "catalogAdminUserName": "<Azure SQL Database server admin username>",
                                "catalogAdminPassword": {
                                    "type": "SecureString",
                                    "value": "<Azure SQL Database server admin password>"
                                },
                                "catalogPricingTier": "Basic"
                            }
                        }
                    }
                }
            }]
        }]
    }
    ```

2. Führen Sie zum Bereitstellen der Azure Resource Manager-Vorlage den Befehl New-AzResourceGroupDeployment wie im folgenden Beispiel gezeigt aus, wobei ADFTutorialResourceGroup der Name der Ressourcengruppe und „ADFTutorialARM.json“ die Datei ist, die die JSON-Definition für Ihre Data Factory und Azure-SSIS-IR enthält.

    ```powershell
    New-AzResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json
    ```

    Dieser Befehl erstellt die Data Factory und darin eine Azure-SSIS-IR. Er startet die IR aber nicht.

3. Führen Sie zum Starten der Azure-SSIS-IR den Befehl Start-AzDataFactoryV2IntegrationRuntime aus:

    ```powershell
    Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName "<Resource Group Name>" `
                                                 -DataFactoryName "<Data Factory Name>" `
                                                 -Name "<Azure SSIS IR Name>" `
                                                 -Force
    ```

> [!NOTE]
> Abgesehen von einer benutzerdefinierten Einrichtungszeit sollte dieser Prozess innerhalb von 5 Minuten abgeschlossen sein. Es kann aber etwa 20 bis 30 Minuten dauern, bis Azure-SSIS IR einem virtuellen Netzwerk beitritt.
>
> Bei Verwendung von SSISDB stellt der ADF-Dienst eine Verbindung mit Ihrem Datenbankserver her, um SSISDB vorzubereiten. Außerdem konfiguriert er Berechtigungen und Einstellungen für das virtuelle Netzwerk (sofern angegeben) und verknüpft die Azure-SSIS IR mit dem virtuellen Netzwerk.
> 
> Wenn Sie Azure-SSIS IR bereitstellen, werden auch das Access Redistributable-Komponente und Azure Feature Pack für SSIS installiert. Diese Komponenten ermöglichen nicht nur die Konnektivität mit Excel- und Access-Dateien und verschiedenen Azure-Datenquellen, sondern auch mit den Datenquellen, die bereits von integrierten Komponenten unterstützt werden. Sie können auch zusätzliche Komponenten installieren. Weitere Informationen dazu finden Sie unter [Benutzerdefiniertes Setup für Azure-SSIS IR](how-to-configure-azure-ssis-ir-custom-setup.md).

## <a name="deploy-ssis-packages"></a>Bereitstellen von SSIS-Paketen

Wenn Sie SSISDB verwenden, können Sie Ihre Pakete darin bereitstellen und sie in Azure-SSIS IR mithilfe von SSDT/SSMS-Tools ausführen, die über den Serverendpunkt eine Verbindung mit Ihrem Datenbankserver herstellen.  Für den Azure SQL-Datenbank-Server/eine verwaltete Instanz mit einem privaten Endpunkt/einer verwalteten Instanz mit einem öffentlichen Endpunkt lautet das Serverendpunktformat `<server name>.database.windows.net`/`<server name>.<dns prefix>.database.windows.net`/`<server name>.public.<dns prefix>.database.windows.net,3342`. Wenn Sie SSISDB nicht verwenden, können Sie Ihre Pakete in Dateisystemen, Dateifreigaben und Azure Files bereitstellen und sie mithilfe von Befehlszeilenhilfsprogrammen (`dtinstall`/`dtutil`/`dtexec`) unter Azure-SSIS IR ausführen. Weitere Informationen finden Sie unter [Bereitstellen von SSIS-Paketen](/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages#deploy-packages-to-integration-services-server). In beiden Fällen können Sie Ihre bereitgestellten Pakete auch unter Azure-SSIS IR ausführen, indem Sie die Aktivität „SSIS-Paket ausführen“ in ADF-Pipelines verwenden. Weitere Informationen finden Sie unter [Aufrufen der SSIS-Paketausführung als erstklassige ADF-Aktivität](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich auch die anderen Themen zur Azure-SSIS IR in dieser Dokumentation an:

- [Azure-SSIS-Integrationslaufzeit](concepts-integration-runtime.md#azure-ssis-integration-runtime): Dieser Artikel enthält allgemeine Informationen zu Integration Runtimes, einschließlich der Azure-SSIS IR.
- [Überwachen der Azure-SSIS IR](monitor-integration-runtime.md#azure-ssis-integration-runtime). In diesem Artikel erfahren Sie, wie Sie Informationen zur Azure-SSIS IR abrufen und verstehen.
- [Verwalten der Azure-SSIS IR](manage-azure-ssis-integration-runtime.md). In diesem Artikel erfahren Sie, wie Sie die Azure-SSIS IR beenden, starten oder löschen. Außerdem wird gezeigt, wie Sie Ihre Azure-SSIS IR durch Hinzufügen weiterer Knoten horizontal hochskalieren können.
- [Hinzufügen der Azure-SSIS IR zu einem virtuellen Netzwerk](join-azure-ssis-integration-runtime-virtual-network.md). Dieser Artikel enthält Informationen zum Hinzufügen Ihrer Azure-SSIS IR zu einem virtuellen Netzwerk.