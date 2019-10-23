---
title: Erstellen einer Azure-SSIS Integration Runtime in Azure Data Factory | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie zur Bereitstellung und Ausführung von SSIS-Paketen in Azure die Azure-SSIS-Integration Runtime in Azure Data Factory erstellen.
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
ms.openlocfilehash: ddc91a3317d362f6b56e486556f2edf6cdb85131
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72326695"
---
# <a name="create-an-azure-ssis-integration-runtime-in-azure-data-factory"></a>Erstellen einer Azure-SSIS Integration Runtime in Azure Data Factory | Microsoft-Dokumentation

In diesem Artikel werden die Schritte zur Bereitstellung einer Azure-SQL Server Integration Services (SSIS) Integration Runtime (IR) in Azure Data Factory beschrieben. Die Azure-SSIS IR unterstützt Folgendes:

- Ausführen von Paketen, die im SSIS-Katalog (SSISDB) bereitgestellt werden, wobei zum Hosten ein Azure SQL-Datenbank-Server oder eine verwaltete Instanz verwendet wird (Projektbereitstellungsmodell)
- Ausführen von Paketen, die in Dateisystemen, auf Dateifreigaben oder unter Azure Files bereitgestellt werden (Projektbereitstellungsmodell) 

Nach der Bereitstellung einer Azure-SSIS IR können Sie die vertrauten Tools nutzen, um Ihre Pakete in Azure bereitzustellen und auszuführen. Beispiele für diese Tools sind SQL Server Data Tools, SQL Server Management Studio und Befehlszeilentools wie `dtinstall`, `dtutil` und `dtexec`.

Im Tutorial zum [Bereitstellen der Azure-SSIS Integration Runtime](tutorial-create-azure-ssis-runtime-portal.md) wird veranschaulicht, wie Sie eine Azure-SSIS IR mit dem Azure-Portal oder der Data Factory-App erstellen. Außerdem wird im Tutorial veranschaulicht, wie Sie optional einen Azure SQL-Datenbank-Server oder eine verwaltete Instanz zum Hosten der SSISDB verwenden. Dieser Artikel baut auf dem Tutorial auf, und es wird beschrieben, wie Sie die folgenden optionalen Aufgaben durchführen:

- Verwenden Sie einen Azure SQL-Datenbank-Server mit VNET-Dienstendpunkten oder eine verwaltete Instanz mit einem privaten Endpunkt zum Hosten der SSISDB. Als Voraussetzung müssen Sie Berechtigungen und Einstellungen für das virtuelle Netzwerk konfigurieren, damit Ihre Azure-SSIS IR einem virtuellen Netzwerk beitreten kann.

- Verwenden Sie die Azure AD-Authentifizierung (Azure Active Directory) mit der verwalteten Identität für Ihre Data Factory, um eine Verbindung mit dem Azure SQL-Datenbank-Server oder der verwalteten Instanz herzustellen. Als Voraussetzung müssen Sie die verwaltete Identität für Ihre Data Factory als Datenbankbenutzer hinzufügen, der eine SSISDB-Instanz erstellen kann.

- Binden Sie Ihre Azure-SSIS IR in ein virtuelles Netzwerk ein, oder konfigurieren Sie eine selbstgehostete Integration Runtime als Proxy für Ihre Azure-SSIS IR, um auf lokale Daten zuzugreifen.

In diesem Artikel wird veranschaulicht, wie Sie eine Azure-SSIS IR mit dem Azure-Portal, Azure PowerShell und einer Azure Resource Manager-Vorlage bereitstellen.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

- **Azure-Abonnement**. Falls Sie noch nicht über ein Abonnement verfügen, können Sie ein [kostenloses Testkonto](https://azure.microsoft.com/pricing/free-trial/) erstellen.
- **Azure SQL-Datenbank-Server oder verwaltete Instanz (optional)** . Wenn Sie noch nicht über einen Datenbankserver verfügen, erstellen Sie einen im Azure-Portal, bevor Sie beginnen. Mit Data Factory wird wiederum eine SSISDB-Instanz auf diesem Datenbankserver erstellt. 

  Es empfiehlt sich, den Datenbankserver in derselben Azure-Region zu erstellen, in der sich auch die Integration Runtime befindet. Diese Konfiguration ermöglicht es der Integration Runtime, Ausführungsprotokolle in SSISDB zu schreiben, ohne Azure-Regionen zu überschreiten.

  Beachten Sie Folgendes:

  - Basierend auf dem ausgewählten Datenbankserver kann die SSISDB-Instanz in Ihrem Namen als Einzeldatenbank, als Teil eines Pools für elastische Datenbanken oder auf einer verwalteten Instanz erstellt werden. Der Zugriff kann über ein öffentliches Netzwerk oder durch den Beitritt zu einem virtuellen Netzwerk erfolgen. Einen Leitfaden zum Auswählen des Datenbankservertyps zum Hosten der SSISDB finden Sie in diesem Artikel im Abschnitt mit dem [Vergleich zwischen einer Azure SQL-Datenbank mit Einzeldatenbank, einem Pool für elastische Datenbanken und einer verwalteten Instanz](#comparison-of-a-sql-database-single-database-elastic-pool-and-managed-instance). 
  
    Wenn Sie einen Azure SQL-Datenbank-Server mit VNET-Dienstendpunkten oder mit einer verwalteten Instanz mit einem privaten Endpunkt zum Hosten der SSISDB verwenden oder Zugriff auf lokale Daten ohne Konfiguration einer selbstgehosteten IR benötigen, müssen Sie die Azure-SSIS IR einem virtuellen Netzwerk hinzufügen. Weitere Informationen finden Sie im Artikel zum [Beitritt einer Azure-SSIS Integration Runtime zu einem virtuellen Netzwerk](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network).
  - Vergewissern Sie sich, dass die Einstellung **Zugriff auf Azure-Dienste zulassen** für den Datenbankserver aktiviert ist. Diese Einstellung gilt nicht, wenn Sie einen Azure SQL-Datenbank-Server mit VNET-Dienstendpunkten oder eine verwaltete Instanz mit einem privaten Endpunkt zum Hosten der SSISDB verwenden. Weitere Informationen finden Sie unter [Schützen der Azure SQL-Datenbank](../sql-database/sql-database-security-tutorial.md#create-firewall-rules). Informationen zum Aktivieren dieser Einstellung mithilfe von PowerShell finden Sie unter [New-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule).
  - Fügen Sie der Client-IP-Adressenliste in den Firewalleinstellungen für den Datenbankserver die IP-Adresse des Clientcomputers oder einen IP-Adressbereich hinzu, der die IP-Adresse des Clientcomputers enthält. Weitere Informationen finden Sie unter [Firewallregeln auf Serverebene und Datenbankebene für Azure SQL-Datenbank](../sql-database/sql-database-firewall-configure.md).
  - Sie können eine Verbindung mit dem Datenbankserver herstellen, indem Sie die SQL-Authentifizierung mit Ihren Serveradministrator-Anmeldeinformationen oder die Azure AD-Authentifizierung mit der verwalteten Identität für Ihre Data Factory-Instanz verwenden. Im letzteren Fall müssen Sie die verwaltete Identität für Ihre Data Factory-Instanz einer Azure AD-Gruppe mit Zugriffsberechtigungen für den Datenbankserver hinzufügen. Weitere Informationen finden Sie unter [Aktivieren der Azure Active Directory-Authentifizierung für Azure-SSIS Integration Runtime](https://docs.microsoft.com/azure/data-factory/enable-aad-authentication-azure-ssis-ir).
  - Vergewissern Sie sich, dass Ihr Datenbankserver noch nicht über eine SSISDB-Instanz verfügt. Für die Bereitstellung einer Azure-SSIS IR wird die Verwendung einer vorhandenen SSISDB-Instanz nicht unterstützt.
- **Virtuelles Azure Resource Manager-Netzwerk (optional)** . Sie benötigen ein virtuelles Azure Resource Manager-Netzwerk, wenn mindestens eine der folgenden Bedingungen zutrifft:
    - Sie hosten die SSISDB auf einem Azure SQL-Datenbank-Server mit VNET-Dienstendpunkten oder einer verwalteten Instanz mit einem privaten Endpunkt.
    - Sie möchten über SSIS-Pakete, die in der Azure-SSIS IR ausgeführt werden, eine Verbindung mit lokalen Datenspeichern herstellen, ohne eine selbstgehostete IR zu konfigurieren.
- **Azure PowerShell (optional)** . Befolgen Sie die Anweisungen unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/install-az-ps), wenn Sie ein PowerShell-Skript zur Bereitstellung von Azure-SSIS IR ausführen möchten.

### <a name="regional-support"></a>Regionsunterstützung

Eine Liste mit den Azure-Regionen, in denen Data Factory und eine Azure-SSIS Integration Runtime verfügbar sind, finden Sie unter [Data Factory und SSIS IR: Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all).

### <a name="comparison-of-a-sql-database-single-database-elastic-pool-and-managed-instance"></a>Vergleich zwischen SQL-Datenbank mit Einzeldatenbank, Pool für elastische Datenbanken und verwalteter Instanz

In der folgenden Tabelle werden bestimmte Features eines Azure SQL-Datenbank-Servers und der verwalteten Instanz in Bezug auf die Azure-SSIS IR verglichen:

| Feature | Einzeldatenbank/Pool für elastische Datenbanken| Verwaltete Instanz |
|---------|--------------|------------------|
| **Zeitplanung** | Der SQL Server-Agent ist nicht verfügbar.<br/><br/>Weitere Informationen finden Sie unter [Planen eines Pakets als Teil einer Azure Data Factory-Pipeline](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-schedule-packages?view=sql-server-2017#activity).| Der Agent für die verwaltete Instanz ist verfügbar. |
| **Authentifizierung** | Sie können eine SSISDB-Instanz mit einem Benutzer für eine eigenständige Datenbank erstellen, der eine beliebige Azure AD-Gruppe mit der verwalteten Identität Ihrer Data Factory als Member in der Rolle **db_owner** darstellt.<br/><br/>Informationen hierzu finden Sie unter [Aktivieren von Azure AD in Azure SQL-Datenbank](enable-aad-authentication-azure-ssis-ir.md#enable-azure-ad-on-azure-sql-database). | Sie können eine SSISDB-Instanz mit einem Benutzer für eine eigenständige Datenbank erstellen, der die verwaltete Identität Ihrer Data Factory darstellt. <br/><br/>Informationen hierzu finden Sie unter [Aktivieren von Azure AD in der verwalteten Azure SQL-Datenbank-Instanz](enable-aad-authentication-azure-ssis-ir.md#enable-azure-ad-on-azure-sql-database-managed-instance). |
| **Dienstebene** | Wenn Sie eine Azure-SSIS IR mit Ihrem Azure SQL-Datenbank-Server erstellen, können Sie die Dienstebene für die SSISDB auswählen. Es gibt mehrere Dienstebenen. | Wenn Sie eine Azure-SSIS IR mit Ihrer verwalteten Instanz erstellen, können Sie die Dienstebene für die SSISDB nicht auswählen. Alle Datenbanken in Ihrer verwalteten Instanz nutzen dieselbe Ressource, die dieser Instanz zugeordnet ist, gemeinsam. |
| **Virtuelles Netzwerk** | Ihre Azure-SSIS IR kann virtuellen Azure Resource Manager-Netzwerken nur beitreten, wenn Sie einen Azure SQL-Datenbank-Server mit VNET-Dienstendpunkten verwenden oder Zugriff auf lokale Datenspeicher benötigen, ohne eine selbstgehostete IR zu konfigurieren. | Ihre Azure-SSIS IR kann nur virtuellen Azure Resource Manager-Netzwerken beitreten. Das virtuelle Netzwerk ist erforderlich, wenn Sie keinen öffentlichen Endpunkt für Ihre verwaltete Instanz aktivieren.<br/><br/>Wenn Sie Ihre Azure-SSIS IR in dasselbe virtuelle Netzwerk wie Ihre verwaltete Instanz einbinden, sollten Sie sicherstellen, dass sich Ihre Azure-SSIS IR in einem anderen Subnetz als Ihre verwaltete Instanz befindet. Wenn Sie Ihre Azure-SSIS IR in ein anderes virtuelles Netzwerk als Ihre verwaltete Instanz einbinden, empfehlen wir Ihnen, entweder ein Peering virtueller Netzwerke oder eine Verbindung zwischen den virtuellen Netzwerken zu verwenden. Informationen hierzu finden Sie unter [Herstellen einer Verbindung zwischen einer Anwendung und einer verwalteten Azure SQL-Datenbank-Instanz](../sql-database/sql-database-managed-instance-connect-app.md). |
| **Verteilte Transaktionen** | Dieses Feature wird über elastische Transaktionen unterstützt. MS DTC-Transaktionen (Microsoft Distributed Transaction Coordinator) werden nicht unterstützt. Wenn Ihre SSIS-Pakete MSDTC verwenden, um verteilte Transaktionen zu koordinieren, sollten Sie eine Migration zu elastischen Transaktionen für Azure SQL-Datenbank erwägen. Weitere Informationen finden Sie unter [Verteilte Transaktionen über Clouddatenbanken](../sql-database/sql-database-elastic-transactions-overview.md). | Nicht unterstützt. |
| | | |

## <a name="use-the-azure-portal-to-create-an-integration-runtime"></a>Verwenden des Azure-Portals zum Erstellen einer Integration Runtime

In diesem Abschnitt erstellen Sie mit dem Azure-Portal über die Data Factory-Benutzeroberfläche (UI) oder die App eine Azure-SSIS IR.

### <a name="create-a-data-factory"></a>Erstellen einer Data Factory

Befolgen Sie die Schritt-für-Schritt-Anleitung unter [Erstellen einer Data Factory](https://docs.microsoft.com/azure/data-factory/quickstart-create-data-factory-portal#create-a-data-factory), um Ihre Data Factory-Instanz über das Azure-Portal zu erstellen. Wählen Sie hierbei die Option **An Dashboard anheften** aus, um nach der Erstellung den schnellen Zugriff zu ermöglichen. 

Öffnen Sie nach der Erstellung Ihrer Data Factory-Instanz im Azure-Portal die zugehörige Übersichtsseite. Wählen Sie die Kachel **Erstellen und überwachen** aus, um die zugehörige Seite **Erste Schritte** auf einer separaten Registerkarte zu öffnen. Sie können darauf dann mit der Erstellung Ihrer Azure-SSIS IR fortfahren.   

### <a name="provision-an-azure-ssis-integration-runtime"></a>Bereitstellen einer Azure-SSIS Integration Runtime

1. Wählen Sie auf der Seite **Let's get started** (Erste Schritte) die Kachel **Configure SSIS Integration Runtime** (SSIS Integration Runtime konfigurieren).

   ![Kachel „Configure SSIS Integration Runtime“ (SSIS Integration Runtime konfigurieren)](./media/tutorial-create-azure-ssis-runtime-portal/configure-ssis-integration-runtime-tile.png)

2. Führen Sie auf der Seite **Allgemeine Einstellungen** von **Integration Runtime Setup** (Integration Runtime-Setup) die unten angegebenen Schritte aus.

   ![Allgemeine Einstellungen](./media/tutorial-create-azure-ssis-runtime-portal/general-settings.png)

    a. Geben Sie unter **Name** den Namen Ihrer Integration Runtime ein.

    b. Geben Sie unter **Beschreibung** die Beschreibung Ihrer Integration Runtime ein.

    c. Wählen Sie unter **Standort** den Standort für Ihre Integration Runtime aus. Es werden nur unterstützte Standorte angezeigt. Sie sollten den gleichen Standort wie für den Datenbankserver zum Hosten von SSISDB auswählen.

    d. Wählen Sie unter **Knotengröße** die Größe des Knotens in Ihrem Integration Runtime-Cluster aus. Nur unterstützte Knotengrößen werden angezeigt. Wählen Sie eine große Knotengröße (zentrales Hochskalieren) aus, wenn Sie zahlreiche compute- oder arbeitsspeicherintensive Pakete ausführen möchten.

    e. Wählen Sie unter **Knotenzahl** die Anzahl von Knoten in Ihrer Integration Runtime aus. Nur unterstützte Knotenzahlen werden angezeigt. Wählen Sie einen großen Cluster mit zahlreichen Knoten (horizontales Hochskalieren) aus, wenn Sie viele Pakete parallel ausführen möchten.

    f. Wählen Sie unter **Edition/Lizenz** die SQL Server-Edition für Ihre Integration Runtime aus: Standard oder Enterprise. Wählen Sie Enterprise aus, wenn Sie erweiterte Funktionen für Ihre Integration Runtime verwenden möchten.

    g. Wählen Sie unter **Sparen Sie Geld** die Option „Azure-Hybridvorteil“ für Ihre Integration Runtime aus: **Ja** oder **Nein**. Wählen Sie **Ja**, wenn Sie eine eigene SQL Server-Lizenz mit Software Assurance verwenden möchten, um bei der Hybridnutzung von Kostenersparnissen zu profitieren.

    h. Klicken Sie auf **Weiter**.

3. Führen Sie auf der Seite **SQL-Einstellungen** die unten angegebenen Schritte aus.

   ![SQL-Einstellungen](./media/tutorial-create-azure-ssis-runtime-portal/sql-settings.png)

   a. Aktivieren Sie das Kontrollkästchen **SSIS-Katalog erstellen**, um das Bereitstellungsmodell für Pakete auszuwählen, die unter Ihrer Azure-SSIS IR ausgeführt werden. Sie wählen entweder das Projektbereitstellungsmodell aus, bei dem Pakete in der SSISDB bereitgestellt werden, die von Ihrem Datenbankserver gehostet wird, oder das Paketbereitstellungsmodell, bei dem Pakete in Dateisystemen, Dateifreigaben oder unter Azure Files bereitgestellt werden. 
    
   Wenn Sie das Kontrollkästchen aktivieren, müssen Sie Ihren eigenen Datenbankserver zum Hosten der SSISDB-Instanz bereitstellen, die wir in Ihrem Namen erstellen und verwalten.
   
   b. Geben Sie unter **Abonnement** das Azure-Abonnement mit dem Azure-Datenbankserver zum Hosten von SSISDB an. 

   c. Wählen Sie als **Standort** den Standort des Datenbankservers zum Hosten von SSISDB aus. Sie sollten denselben Standort wie den für die Integration Runtime auswählen. 

   d. Wählen Sie unter **Katalogdatenbank-Serverendpunkt** den Endpunkt Ihres Datenbankservers zum Hosten von SSISDB aus. 
    
   Basierend auf dem ausgewählten Datenbankserver kann die SSISDB-Instanz in Ihrem Namen als Einzeldatenbank, als Teil eines Pools für elastische Datenbanken oder auf einer verwalteten Instanz erstellt werden. Der Zugriff kann über ein öffentliches Netzwerk oder durch den Beitritt zu einem virtuellen Netzwerk erfolgen. Einen Leitfaden zum Auswählen des Datenbankservertyps zum Hosten der SSISDB finden Sie in diesem Artikel im Abschnitt mit dem [Vergleich zwischen einer Azure SQL-Datenbank mit Einzeldatenbank, einem Pool für elastische Datenbanken und einer verwalteten Instanz](#comparison-of-a-sql-database-single-database-elastic-pool-and-managed-instance). 
    
   Wenn Sie einen Azure SQL-Datenbank-Server mit VNET-Dienstendpunkten oder mit einer verwalteten Instanz mit einem privaten Endpunkt zum Hosten der SSISDB auswählen oder Zugriff auf lokale Daten ohne Konfiguration einer selbstgehosteten IR benötigen, müssen Sie die Azure-SSIS IR einem virtuellen Netzwerk hinzufügen. Weitere Informationen finden Sie im Artikel zum [Beitritt einer Azure-SSIS Integration Runtime zu einem virtuellen Netzwerk](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network). 

   e. Aktivieren Sie das Kontrollkästchen **Use AAD authentication with the managed identity for your ADF** (AAD-Authentifizierung mit der verwalteten Identität für Ihre ADF verwenden), um die Authentifizierungsmethode für Ihren Datenbankserver zum Hosten der SSISDB auszuwählen. Sie wählen entweder die SQL-Authentifizierung oder die Azure AD-Authentifizierung mit der verwalteten Identität für Ihre Data Factory-Instanz aus. 
    
   Wenn Sie das Kontrollkästchen aktivieren, müssen Sie die verwaltete Identität für Ihre Data Factory-Instanz einer Azure AD-Gruppe mit Zugriffsberechtigungen für Ihren Datenbankserver hinzufügen. Weitere Informationen finden Sie unter [Aktivieren der Azure Active Directory-Authentifizierung für Azure-SSIS Integration Runtime](https://docs.microsoft.com/azure/data-factory/enable-aad-authentication-azure-ssis-ir). 

   f. Geben Sie unter **Administratorbenutzername** den Benutzernamen für die SQL-Authentifizierung für den Datenbankserver zum Hosten der SSISDB an. 

   g. Geben Sie unter **Administratorbenutzerkennwort** das Kennwort für die SQL-Authentifizierung für den Datenbankserver zum Hosten der SSISDB an. 

   h. Wählen Sie unter **Katalogdatenbank-Dienstebene** die Dienstebene für Ihren Datenbankserver zum Hosten der SSISDB aus. Wählen Sie den Tarif „Basic“, „Standard“ oder „Premium“ oder den Namen eines Pools für elastische Datenbanken aus. 

   i. Wählen Sie **Verbindung testen** aus. Wählen Sie **Weiter** aus, wenn der Test erfolgreich ist. 

4. Führen Sie auf der Seite **Erweiterte Einstellungen** die unten angegebenen Schritte aus.

   ![Erweiterte Einstellungen](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)

   a. Legen Sie unter **Maximale Anzahl von parallelen Ausführungen pro Knoten** die maximale Anzahl von Paketen fest, die in Ihrem Integration Runtime-Cluster pro Knoten gleichzeitig ausgeführt werden sollen. Nur unterstützte Paketzahlen werden angezeigt. Wählen Sie eine niedrigere Anzahl aus, wenn Sie mehrere Kerne verwenden möchten, um ein einzelnes großes Paket auszuführen, das compute- oder arbeitsspeicherintensiv ist. Wählen Sie eine hohe Anzahl aus, wenn Sie mindestens ein kleines Paket in einem einzelnen Kern ausführen möchten.

   b. Geben Sie unter **Container-SAS-URI des benutzerdefinierten Setups** optional den SAS-URI (Shared Access Signature-Uniform Resource Identifier) des Azure-Blobspeichercontainers an, in dem Ihr Einrichtungsskript und die zugehörigen Dateien gespeichert sind. Weitere Informationen finden Sie unter [Anpassen des Setups für Azure-SSIS Integration Runtime](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup).

5. Aktivieren Sie das Kontrollkästchen **Select a VNET for your Azure-SSIS Integration Runtime to join and allow ADF to create certain network resources** (VNET für Ihre Azure-SSIS Integration Runtime für den Beitritt auswählen und Erstellung bestimmter Netzwerkressourcen für ADF ermöglichen), um auszuwählen, ob Sie Ihre Integration Runtime-Instanz in ein virtuelles Netzwerk einbinden möchten. 

   Aktivieren Sie dieses Kontrollkästchen, wenn Sie einen Azure SQL-Datenbank-Server mit VNET-Dienstendpunkten oder mit einer verwalteten Instanz mit einem privaten Endpunkt zum Hosten der SSISDB verwenden oder Zugriff auf lokale Daten benötigen. (Dies bedeutet, dass Ihre SSIS-Pakete lokale Datenquellen oder Ziele enthalten, ohne dass eine selbstgehostete IR konfiguriert wird.) Weitere Informationen finden Sie im Artikel zum [Beitritt einer Azure-SSIS Integration Runtime zu einem virtuellen Netzwerk](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network). 

   Führen Sie die unten angegebenen Schritte aus, falls Sie das Kontrollkästchen aktivieren.

   ![Erweiterte Einstellungen mit einem virtuellen Netzwerk](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-vnet.png)

   a. Wählen Sie unter **Abonnement** das Azure-Abonnement aus, das Ihr virtuelles Netzwerk enthält.

   b. Für **Standort** ist derselbe Standort wie für Ihre Integration Runtime ausgewählt.

   c. Wählen Sie für **Typ** den Typ des virtuellen Netzwerks aus: „Klassisch“ oder „Azure Resource Manager“. Sie sollten ein virtuelles Azure Resource Manager-Netzwerk auswählen, da das klassische virtuelle Netzwerk bald veraltet sein wird.

   d. Wählen Sie unter **VNET-Name** den Namen des virtuellen Netzwerks aus. Dieses virtuelle Netzwerk muss dasselbe virtuelle Netzwerk sein, das für den Azure SQL-Datenbank-Server mit VNET-Dienstendpunkten oder eine verwaltete Instanz in einem virtuellen Netzwerk zum Hosten von SSISDB verwendet wird. Dieses virtuelle Netzwerk kann auch das Netzwerk sein, das mit Ihrem lokalen Netzwerk verbunden ist.

   e. Wählen Sie unter **Subnetzname** den Namen des Subnetzes für Ihr virtuelles Netzwerk aus. Dieses virtuelle Netzwerk sollte nicht das Subnetz sein, das für die verwaltete Instanz in einem virtuellen Netzwerk zum Hosten der SSISDB verwendet wird, sondern ein anderes.

6. Aktivieren Sie das Kontrollkästchen **Set up Self-Hosted Integration Runtime as a proxy for your Azure-SSIS Integration Runtime** (Selbstgehostete Integration Runtime als Proxy für Ihre Azure-SSIS Integration Runtime einrichten), um auszuwählen, ob Sie eine selbstgehostete IR als Proxy für Ihre Azure-SSIS IR konfigurieren möchten. Weitere Informationen finden Sie unter [Konfigurieren einer selbstgehosteten IR als Proxy für Azure-SSIS IR in ADF](https://docs.microsoft.com/azure/data-factory/self-hosted-integration-runtime-proxy-ssis). 

   Führen Sie die unten angegebenen Schritte aus, falls Sie das Kontrollkästchen aktivieren.

   ![Erweiterte Einstellungen bei einer selbstgehosteten IR](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-shir.png)

   a. Wählen Sie für die **selbstgehostete Integration Runtime** Ihre vorhandene selbstgehostete IR als Proxy für die Azure-SSIS IR aus.

   b. Wählen Sie für den **verknüpften Dienst des Stagingspeichers** Ihren vorhandenen Dienst aus, der mit Azure-Blobspeicher verknüpft ist. Sie können für das Staging auch einen neuen Dienst erstellen.

   c. Geben Sie unter **Stagingpfad** in Ihrem ausgewählten Azure-Blobspeicherkonto einen Blobcontainer an. Oder lassen Sie die Option leer, um für das Staging einen Standardpfad zu verwenden.

7. Wählen Sie **VNET-Überprüfung** > **Weiter** aus. 

8. Überprüfen Sie auf der Seite **Zusammenfassung** alle Bereitstellungseinstellungen, lesen Sie die empfohlenen Dokumentationslinks, und wählen Sie **Fertig stellen** aus, um die Erstellung Ihrer Integration Runtime zu starten.

    > [!NOTE]
    > Abgesehen von einer benutzerdefinierten Einrichtungszeit sollte dieser Prozess innerhalb von fünf Minuten abgeschlossen sein. Es kann aber 20 bis 30 Minuten dauern, bis der Betritt einer Azure-SSIS IR zu einem virtuellen Netzwerk abgeschlossen ist.
    >
    > Bei Verwendung der SSISDB stellt der Data Factory-Dienst eine Verbindung mit Ihrem Datenbankserver her, um die SSISDB vorzubereiten. Außerdem konfiguriert er Berechtigungen und Einstellungen für das virtuelle Netzwerk (sofern angegeben) und verknüpft die Azure-SSIS IR mit dem virtuellen Netzwerk.
    > 
    > Wenn Sie Azure-SSIS IR bereitstellen, werden auch die Access Redistributable-Komponente und das Azure Feature Pack für SSIS installiert. Diese Komponenten ermöglichen nicht nur die Konnektivität mit Excel-Dateien, Access-Dateien und verschiedenen Azure-Datenquellen, sondern auch mit den Datenquellen, die von integrierten Komponenten bereits unterstützt werden. Informationen zu anderen Komponenten, die Sie installieren können, finden Sie unter [Anpassen des Setups für Azure-SSIS Integration Runtime](how-to-configure-azure-ssis-ir-custom-setup.md).

7. Wechseln Sie auf der Registerkarte **Verbindungen** ggf. zu **Integration Runtimes**. Wählen Sie zum Aktualisieren des Status die Option **Aktualisieren**.

   ![Status der Erstellung](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-creation-status.png)

8. Über die Links in der Spalte **Aktionen** können Sie die Integration Runtime beenden/starten, bearbeiten oder löschen. Über den letzten Link können Sie den JSON-Code für die Integration Runtime anzeigen. Die Schaltflächen zum Bearbeiten und Löschen sind nur verfügbar, wenn die IR beendet wird.

   ![Azure-SSIS IR-Aktionen](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-actions.png)

### <a name="azure-ssis-integration-runtimes-in-the-portal"></a>Azure SSIS Integration Runtimes im Portal

1. Wechseln Sie auf der Azure Data Factory-Benutzeroberfläche zur Registerkarte **Bearbeiten**, und wählen Sie **Verbindungen** aus. Wechseln Sie anschließend zur Registerkarte **Integration Runtimes**, um die vorhandenen Integration Runtimes Ihrer Data Factory-Instanz anzuzeigen.

   ![Anzeigen vorhandener IRs](./media/tutorial-create-azure-ssis-runtime-portal/view-azure-ssis-integration-runtimes.png)

2. Wählen Sie **Neu** aus, um eine neue Azure-SSIS IR zu erstellen.

   ![Integration Runtime über das Menü](./media/tutorial-create-azure-ssis-runtime-portal/edit-connections-new-integration-runtime-button.png)

4. Wählen Sie im Fenster für die **Integration Runtime-Einrichtung** die Option **Lift-and-shift existing SSIS packages to execute in Azure** (Migrieren vorhandener SSIS-Pakete per Lift & Shift für die Ausführung in Azure) und dann **Weiter**.

   ![Angeben des Integration Runtime-Typs](./media/tutorial-create-azure-ssis-runtime-portal/integration-runtime-setup-options.png)

5. Die übrigen Schritte zum Einrichten einer Azure-SSIS IR finden Sie im Abschnitt [Bereitstellen einer Azure-SSIS Integration Runtime](#provision-an-azure-ssis-integration-runtime).

## <a name="use-azure-powershell-to-create-an-integration-runtime"></a>Verwenden von Azure PowerShell zum Erstellen einer Integration Runtime

In diesem Abschnitt verwenden Sie Azure PowerShell zum Erstellen einer Azure-SSIS IR.

### <a name="create-variables"></a>Erstellen von Variablen

Kopieren und fügen Sie das folgende Skript ein. Geben Sie Werte für die Variablen an. 

```powershell
### Azure Data Factory information
# If your input contains a PSH special character like "$", precede it with the escape character "`" - for example, "`$"
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
# Data factory name - must be globally unique
$DataFactoryName = "[your data factory name]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$DataFactoryLocation = "EastUS"

### Azure-SSIS integration runtime information. This is a Data Factory compute resource for running SSIS packages.
$AzureSSISName = "[your Azure-SSIS IR name]"
$AzureSSISDescription = "[your Azure-SSIS IR description]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$AzureSSISLocation = "EastUS"
# For supported node sizes, see https://azure.microsoft.com/pricing/details/data-factory/ssis/
$AzureSSISNodeSize = "Standard_D8_v3"
# 1-10 nodes are currently supported
$AzureSSISNodeNumber = 2
# Azure-SSIS IR edition/license info: Standard or Enterprise
$AzureSSISEdition = "Standard" # Standard by default, whereas Enterprise lets you use advanced features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "LicenseIncluded" # LicenseIncluded by default, whereas BasePrice lets you bring your own on-premises SQL Server license with Software Assurance to earn cost savings from Azure Hybrid Benefit option
# For a Standard_D1_v2 node, up to 4 parallel executions per node are supported. For other nodes, up to (2 x number of cores) are currently supported.
$AzureSSISMaxParallelExecutionsPerNode = 8
# Custom setup info
$SetupScriptContainerSasUri = "" # OPTIONAL to provide the SAS URI of the blob container where your custom setup script and its associated files are stored
# Virtual network info: classic or Azure Resource Manager
$VnetId = "[your virtual network resource ID or leave it empty]" # REQUIRED if you use an Azure SQL Database server with virtual network service endpoints or a managed instance with a private endpoint, or if you require on-premises data without configuring a self-hosted IR. We recommend an Azure Resource Manager virtual network, because classic virtual networks will be deprecated soon.
$SubnetName = "[your subnet name or leave it empty]" # WARNING: Use the same subnet as the one used for your Azure SQL Database server with virtual network service endpoints, or a different subnet from the one used for your managed instance with a private endpoint

### SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name.database.windows.net or managed instance name.DNS prefix.database.windows.net or managed instance name.public.DNS prefix.database.windows.net,3342 or leave it empty if you do not use SSISDB]" # WARNING: If you use SSISDB, ensure that there's no existing SSISDB on your database server, so we can prepare and manage one on your behalf
# Authentication info: SQL or Azure AD
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication or leave it empty for Azure AD authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication or leave it empty for Azure AD authentication]"
# For the basic pricing tier, specify "Basic," not "B." For standard, premium, and elastic pool tiers, specify "S0," "S1," "S2," "S3," etc. See https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-database-server.
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>) for Azure SQL Database server or leave it empty for managed instance]"
```

### <a name="sign-in-and-select-a-subscription"></a>Anmelden und Auswählen eines Abonnements

Fügen Sie das folgende Skript hinzu, um sich anzumelden und Ihr Azure-Abonnement auszuwählen.

```powershell
Connect-AzAccount
Select-AzSubscription -SubscriptionName $SubscriptionName
```

### <a name="validate-the-connection-to-database-server"></a>Überprüfen der Verbindung mit dem Datenbankserver

Fügen Sie das folgende Skript hinzu, um den Azure SQL-Datenbank-Server oder die verwaltete Instanz zu überprüfen.

```powershell
# Validate only if you use SSISDB and you don't use virtual network or Azure AD authentication
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

### <a name="configure-the-virtual-network"></a>Konfigurieren des virtuellen Netzwerks

Fügen Sie das folgende Skript hinzu, um Berechtigungen und Einstellungen für das virtuelle Netzwerk für die Azure-SSIS Integration Runtime, die Sie einbinden möchten, automatisch zu konfigurieren.

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

Wenn Sie die SSISDB nicht nutzen, können Sie die Parameter `CatalogServerEndpoint`, `CatalogPricingTier` und `CatalogAdminCredential` weglassen.

Falls Sie keinen Azure SQL-Datenbank-Server mit VNET-Dienstendpunkten oder eine verwaltete Instanz mit einem privaten Endpunkt zum Hosten der SSISDB verwenden oder Zugriff auf lokale Daten benötigen, können Sie die Parameter `VNetId` und `Subnet` weglassen oder dafür leere Werte übergeben. Sie können sie auch weglassen, wenn Sie die selbstgehostete IR als Proxy für Ihre Azure-SSIS IR für den Zugriff auf lokale Daten konfigurieren. Andernfalls können Sie sie nicht weglassen und müssen gültige Werte aus Ihrer Konfiguration des virtuellen Netzwerks übergeben. Weitere Informationen finden Sie im Artikel zum [Beitritt einer Azure-SSIS Integration Runtime zu einem virtuellen Netzwerk](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network).

Wenn Sie eine verwaltete Instanz zum Hosten der SSISDB verwenden, können Sie den Parameter `CatalogPricingTier` weglassen oder dafür einen leeren Wert übergeben. Andernfalls können Sie ihn nicht weggelassen und müssen einen gültigen Wert aus der Liste mit den für Azure SQL-Datenbank unterstützten Tarifen übergeben. Weitere Informationen finden Sie unter [Ressourceneinschränkungen für SQL-Datenbank](../sql-database/sql-database-resource-limits.md).

Wenn Sie die Azure AD-Authentifizierung mit der verwalteten Identität für Ihre Data Factory verwenden, um eine Verbindung mit dem Datenbankserver herzustellen, können Sie den Parameter `CatalogAdminCredential` weglassen. Sie müssen aber die verwaltete Identität für Ihre Data Factory-Instanz einer Azure AD-Gruppe mit Zugriffsberechtigungen für den Datenbankserver hinzufügen. Weitere Informationen finden Sie unter [Aktivieren der Azure Active Directory-Authentifizierung für Azure-SSIS Integration Runtime](https://docs.microsoft.com/azure/data-factory/enable-aad-authentication-azure-ssis-ir). Andernfalls können Sie ihn nicht weglassen, sondern müssen ein gültiges Objekt – gebildet aus Ihrem Administratorbenutzernamen und dem zugehörigen Kennwort für den Server –für die SQL-Authentifizierung übergeben.

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
       
# Add the CatalogServerEndpoint, CatalogPricingTier, and CatalogAdminCredential parameters if you use SSISDB
if(![string]::IsNullOrEmpty($SSISDBServerEndpoint))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -CatalogServerEndpoint $SSISDBServerEndpoint `
                                               -CatalogPricingTier $SSISDBPricingTier

    if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) –and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword)) # Add the CatalogAdminCredential parameter if you don't use Azure AD authentication
    {
        $secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
        $serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)

        Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                                   -DataFactoryName $DataFactoryName `
                                                   -Name $AzureSSISName `
                                                   -CatalogAdminCredential $serverCreds
    }
}

# Add the SetupScriptContainerSasUri parameter if you use custom setup
if(![string]::IsNullOrEmpty($SetupScriptContainerSasUri))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -SetupScriptContainerSasUri $SetupScriptContainerSasUri
}
```

### <a name="start-the-integration-runtime"></a>Starten der Integration Runtime

Führen Sie die folgenden Befehle aus, um Azure-SSIS Integration Runtime zu starten.

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
> Abgesehen von einer benutzerdefinierten Einrichtungszeit sollte dieser Prozess innerhalb von fünf Minuten abgeschlossen sein. Es kann aber 20 bis 30 Minuten dauern, bis der Betritt einer Azure-SSIS IR zu einem virtuellen Netzwerk abgeschlossen ist.
>
> Bei Verwendung der SSISDB stellt der Data Factory-Dienst eine Verbindung mit Ihrem Datenbankserver her, um die SSISDB vorzubereiten. Außerdem konfiguriert er Berechtigungen und Einstellungen für das virtuelle Netzwerk (sofern angegeben) und verknüpft die Azure-SSIS IR mit dem virtuellen Netzwerk.
> 
> Wenn Sie Azure-SSIS IR bereitstellen, werden auch die Access Redistributable-Komponente und das Azure Feature Pack für SSIS installiert. Diese Komponenten ermöglichen nicht nur die Konnektivität mit Excel-Dateien, Access-Dateien und verschiedenen Azure-Datenquellen, sondern auch mit den Datenquellen, die von integrierten Komponenten bereits unterstützt werden. Informationen zu anderen Komponenten, die Sie installieren können, finden Sie unter [Anpassen des Setups für Azure-SSIS Integration Runtime](how-to-configure-azure-ssis-ir-custom-setup.md).

### <a name="full-script"></a>Vollständiges Skript

Hier ist das vollständige Skript angegeben, mit dem eine Azure-SSIS Integration Runtime erstellt wird.

```powershell
### Azure Data Factory information
# If your input contains a PSH special character like "$", precede it with the escape character "`" - for example, "`$"
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
# Data factory name - must be globally unique
$DataFactoryName = "[your data factory name]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$DataFactoryLocation = "EastUS"

### Azure-SSIS integration runtime information. This is a Data Factory compute resource for running SSIS packages.
$AzureSSISName = "[your Azure-SSIS IR name]"
$AzureSSISDescription = "[your Azure-SSIS IR description]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$AzureSSISLocation = "EastUS"
# For supported node sizes, see https://azure.microsoft.com/pricing/details/data-factory/ssis/
$AzureSSISNodeSize = "Standard_D8_v3"
# 1-10 nodes are currently supported
$AzureSSISNodeNumber = 2
# Azure-SSIS IR edition/license info: Standard or Enterprise
$AzureSSISEdition = "Standard" # Standard by default, whereas Enterprise lets you use advanced features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "LicenseIncluded" # LicenseIncluded by default, whereas BasePrice lets you bring your own on-premises SQL Server license with Software Assurance to earn cost savings from the Azure Hybrid Benefit option
# For a Standard_D1_v2 node, up to four parallel executions per node are supported. For other nodes, up to (2 x number of cores) are currently supported.
$AzureSSISMaxParallelExecutionsPerNode = 8
# Custom setup info
$SetupScriptContainerSasUri = "" # OPTIONAL to provide the SAS URI of the blob container where your custom setup script and its associated files are stored
# Virtual network info: classic or Azure Resource Manager
$VnetId = "[your virtual network resource ID or leave it empty]" # REQUIRED if you use an Azure SQL Database server with virtual network service endpoints or a managed instance with a private endpoint, or if you require on-premises data without configuring a self-hosted IR. We recommend an Azure Resource Manager virtual network, because classic virtual networks will be deprecated soon.
$SubnetName = "[your subnet name or leave it empty]" # WARNING: Use the same subnet as the one used for your Azure SQL Database server with virtual network service endpoints, or a different subnet from the one used for your managed instance with a private endpoint

### SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name.database.windows.net or managed instance name.DNS prefix.database.windows.net or managed instance name.public.DNS prefix.database.windows.net,3342 or leave it empty if you do not use SSISDB]" # WARNING: If you use SSISDB, ensure that there's no existing SSISDB on your database server, so we can prepare and manage one on your behalf
# Authentication info: SQL or Azure AD
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication or leave it empty for Azure AD authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication or leave it empty for Azure AD authentication]"
# For the basic pricing tier, specify "Basic," not "B." For standard, premium, and elastic pool tiers, specify "S0," "S1," "S2," "S3," etc. See https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-database-server.
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>) for Azure SQL Database server or leave it empty for managed instance]"

### Sign in and select a subscription
Connect-AzAccount
Select-AzSubscription -SubscriptionName $SubscriptionName

### Validate the connection to the database server
# Validate only if you use SSISDB and don't use a virtual network or Azure AD authentication
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

### Configure a virtual network
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

    if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) –and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword)) # Add the CatalogAdminCredential parameter if you don't use Azure AD authentication
    {
        $secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
        $serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)

        Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                                   -DataFactoryName $DataFactoryName `
                                                   -Name $AzureSSISName `
                                                   -CatalogAdminCredential $serverCreds
    }
}

# Add the SetupScriptContainerSasUri parameter when you use custom setup
if(![string]::IsNullOrEmpty($SetupScriptContainerSasUri))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -SetupScriptContainerSasUri $SetupScriptContainerSasUri
}

### Start the integration runtime
write-host("##### Starting #####")
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")
```

## <a name="use-an-azure-resource-manager-template-to-create-an-integration-runtime"></a>Verwenden einer Azure Resource Manager-Vorlage zum Erstellen einer Integration Runtime

In diesem Abschnitt verwenden Sie eine Azure Resource Manager-Vorlage, um die Azure-SSIS Integration Runtime zu erstellen. Hier ist eine exemplarische Vorgehensweise angegeben:

1. Erstellen Sie eine JSON-Datei mit der folgenden Azure Resource Manager-Vorlage. Ersetzen Sie die Werte in spitzen Klammern (Platzhalter) durch Ihre eigenen Werte.

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

2. Führen Sie den Befehl `New-AzResourceGroupDeployment` wie im folgenden Beispiel aus, um die Azure Resource Manager-Vorlage bereitzustellen. Im Beispiel ist `ADFTutorialResourceGroup` der Name Ihrer Ressourcengruppe. `ADFTutorialARM.json` ist die Datei, die die JSON-Definition für Ihre Data Factory und die Azure-SSIS IR enthält.

    ```powershell
    New-AzResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json
    ```

    Mit diesem Befehl wird die Data Factory und darin eine Azure-SSIS IR erstellt, aber er dient nicht zum Starten der IR.

3. Führen Sie den Befehl `Start-AzDataFactoryV2IntegrationRuntime` aus, um Ihre Azure-SSIS IR zu starten:

    ```powershell
    Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName "<Resource Group Name>" `
                                                 -DataFactoryName "<Data Factory Name>" `
                                                 -Name "<Azure SSIS IR Name>" `
                                                 -Force
    ```

> [!NOTE]
> Abgesehen von einer benutzerdefinierten Einrichtungszeit sollte dieser Prozess innerhalb von fünf Minuten abgeschlossen sein. Es kann aber 20 bis 30 Minuten dauern, bis der Betritt einer Azure-SSIS IR zu einem virtuellen Netzwerk abgeschlossen ist.
>
> Bei Verwendung der SSISDB stellt der Data Factory-Dienst eine Verbindung mit Ihrem Datenbankserver her, um die SSISDB vorzubereiten. Außerdem konfiguriert er Berechtigungen und Einstellungen für das virtuelle Netzwerk (sofern angegeben) und verknüpft die Azure-SSIS IR mit dem virtuellen Netzwerk.
> 
> Wenn Sie Azure-SSIS IR bereitstellen, werden auch die Access Redistributable-Komponente und das Azure Feature Pack für SSIS installiert. Diese Komponenten ermöglichen nicht nur die Konnektivität mit Excel-Dateien, Access-Dateien und verschiedenen Azure-Datenquellen, sondern auch mit den Datenquellen, die von integrierten Komponenten bereits unterstützt werden. Informationen zu anderen Komponenten, die Sie installieren können, finden Sie unter [Anpassen des Setups für Azure-SSIS Integration Runtime](how-to-configure-azure-ssis-ir-custom-setup.md).

## <a name="deploy-ssis-packages"></a>Bereitstellen von SSIS-Paketen

Bei Verwendung der SSISDB können Sie Ihre Pakete darin bereitstellen und in der Azure-SSIS IR ausführen, indem Sie SQL Server Data Tools oder SQL Server Management Studio-Tools verwenden. Mit diesen Tools wird über den zugehörigen Serverendpunkt eine Verbindung mit Ihrem Datenbankserver hergestellt: 

- Für einen Azure SQL-Datenbank-Server mit einem privaten Endpunkt lautet das Format des Serverendpunkts `<server name>.database.windows.net`.
- Für eine verwaltete Instanz mit einem privaten Endpunkt lautet das Format des Serverendpunkts `<server name>.<dns prefix>.database.windows.net`.
- Für eine verwaltete Instanz mit einem öffentlichen Endpunkt lautet das Format des Serverendpunkts `<server name>.public.<dns prefix>.database.windows.net,3342`. 

Falls Sie die SSISDB nicht verwenden, können Sie Ihre Pakete in Dateisystemen, auf Dateifreigaben oder unter Azure Files bereitstellen. Sie können sie dann über die Azure-SSIS IR ausführen, indem Sie die Befehlszeilentools `dtinstall`, `dtutil` und `dtexec` verwenden. Weitere Informationen finden Sie unter [Bereitstellen von SSIS-Paketen](/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages#deploy-packages-to-integration-services-server). 

In beiden Fällen können Sie Ihre bereitgestellten Pakete auch auf der Azure-SSIS IR ausführen, indem Sie die Aktivität „SSIS-Paket ausführen“ in Data Factory-Pipelines verwenden. Weitere Informationen finden Sie unter [Ausführen eines SSIS-Pakets mit der Aktivität „SSIS-Paket ausführen“ in Azure Data Factory](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich auch andere Themen zur Azure-SSIS IR in dieser Dokumentation an:

- [Azure SSIS-Integration Runtime:](concepts-integration-runtime.md#azure-ssis-integration-runtime) Dieser Artikel enthält allgemeine Informationen zu Integration Runtimes, einschließlich der Azure-SSIS IR.
- [Überwachen einer Azure-SSIS-Integrationslaufzeit](monitor-integration-runtime.md#azure-ssis-integration-runtime): In diesem Artikel erfahren Sie, wie Sie Informationen zur Azure-SSIS IR abrufen und verstehen.
- [Verwalten einer Azure-SSIS-Integrationslaufzeit](manage-azure-ssis-integration-runtime.md): In diesem Artikel wird beschrieben, wie Sie die Azure-SSIS IR beenden, starten oder löschen. Es wird zudem gezeigt, wie Sie Ihre Azure-SSIS Integration Runtime horizontal hochskalieren, indem Sie weitere Knoten hinzufügen.
- [Verknüpfen einer Azure-SSIS-Integration Runtime mit einem virtuellen Netzwerk:](join-azure-ssis-integration-runtime-virtual-network.md). Dieser Artikel enthält Informationen zum Hinzufügen Ihrer Azure-SSIS IR zu einem virtuellen Netzwerk.
