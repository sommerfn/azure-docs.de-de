---
title: Schützen einer Einzel- oder Pooldatenbank
description: In diesem Tutorial erfahren Sie mehr über die Methoden und Funktionen zum Schützen einer Einzeldatenbank oder einer in einem Pool zusammengefassten Datenbank in Azure SQL-Datenbank.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.topic: tutorial
author: VanMSFT
ms.author: vanto
ms.reviewer: carlrab
ms.date: 09/03/2019
ms.custom: seoapril2019
ms.openlocfilehash: 05ea21f7b3221425e5b052797d79e629fdfe3ba5
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73818039"
---
# <a name="tutorial-secure-a-single-or-pooled-database"></a>Tutorial: Schützen einer Einzel- oder Pooldatenbank

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> - Erstellen von Firewallregeln auf Server- und Datenbankebene
> - Konfigurieren eines Azure AD-Administrators (Azure Active Directory)
> - Verwalten des Benutzerzugriffs mit SQL-Authentifizierung, Azure AD-Authentifizierung und sicheren Verbindungszeichenfolgen
> - Aktivieren von Sicherheitsfeatures (beispielsweise Advanced Data Security, Überwachung, Datenmaskierung und Verschlüsselung)

Azure SQL-Datenbank bietet folgende Möglichkeiten, um Daten in einer Einzel- oder Pooldatenbank zu schützen:

- Beschränken des Zugriffs mithilfe von Firewallregeln
- Verwenden von Authentifizierungsmechanismen mit Identitätsnachweis
- Verwenden einer Autorisierung mit rollenbasierten Mitgliedschaften und Berechtigungen
- Aktivieren der Sicherheitsfeatures

> [!NOTE]
> Eine Azure SQL-Datenbank in einer verwalteten Instanz wird mithilfe von Netzwerksicherheitsregeln und privaten Endpunkten geschützt, wie unter [Verwaltete Azure SQL-Datenbank-Instanz](sql-database-managed-instance-index.yml) und unter [Verbindungsarchitektur der verwalteten Azure SQL-Datenbank-Instanz](sql-database-managed-instance-connectivity-architecture.md) beschrieben.

Weitere Informationen finden Sie in den Artikeln [Azure SQL-Datenbank – Erweiterte Sicherheit](/azure/sql-database/sql-database-security-index) und [Eine Übersicht über die Sicherheitsfunktionen von Azure SQL-Datenbank](sql-database-security-overview.md).

> [!TIP]
> In den folgenden Microsoft Learn-Modulen können Sie sich kostenlos über das [Schützen von Azure SQL-Datenbank](https://docs.microsoft.com/learn/modules/secure-your-azure-sql-database/) informieren.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial wird Folgendes vorausgesetzt:

- [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms)
- Azure SQL-Server und -Datenbank
  - Erstellen Sie sie über das [Azure-Portal](sql-database-single-database-get-started.md), mithilfe der [Befehlszeilenschnittstelle](sql-database-cli-samples.md) oder mithilfe von [PowerShell](sql-database-powershell-samples.md).

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Für alle Schritte in diesem Tutorial ist eine Anmeldung beim [Azure-Portal](https://portal.azure.com/) erforderlich.

## <a name="create-firewall-rules"></a>Erstellen von Firewallregeln

SQL-Datenbanken werden in Azure durch Firewalls geschützt. Standardmäßig werden alle Verbindungen mit dem Server und der Datenbank abgelehnt. Weitere Informationen finden Sie unter [Firewallregeln auf Serverebene und Datenbankebene für Azure SQL-Datenbank](sql-database-firewall-configure.md).

Die sicherste Konfiguration erhalten Sie, indem Sie **Zugriff auf Azure-Dienste erlauben** auf **AUS** festlegen. Erstellen Sie anschließend für die Ressource, die eine Verbindung herstellen muss (beispielsweise ein virtueller Computer oder ein Clouddienst), eine [reservierte IP-Adresse (klassische Bereitstellung)](../virtual-network/virtual-networks-reserved-public-ip.md), und lassen Sie den Zugriff durch die Firewall nur für diese IP-Adresse zu. Bei Verwendung des [Resource Manager-Bereitstellungsmodells](/azure/virtual-network/virtual-network-ip-addresses-overview-arm) wird für jede Ressource eine dedizierte öffentliche IP-Adresse benötigt.

> [!NOTE]
> SQL-Datenbank kommuniziert über Port 1433. Wenn Sie versuchen, eine Verbindung aus einem Unternehmensnetzwerk heraus herzustellen, wird der ausgehende Datenverkehr über Port 1433 von der Firewall Ihres Netzwerks unter Umständen nicht zugelassen. In diesem Fall können Sie nur dann eine Verbindung mit Ihrem Azure SQL-Datenbank-Server herstellen, wenn Ihr Administrator den Port 1433 öffnet.

### <a name="set-up-sql-database-server-firewall-rules"></a>Einrichten von Firewallregeln für SQL-Datenbank-Server

IP-Firewallregeln auf Serverebene gelten für alle Datenbanken auf dem gleichen SQL-Datenbank-Server.

So richten Sie eine Firewallregel auf Serverebene ein:

1. Wählen Sie im Azure-Portal im Menü auf der linken Seite die Option **SQL-Datenbanken** und anschließend auf der Seite **SQL-Datenbanken** Ihre Datenbank aus.

    ![Serverfirewallregel](./media/sql-database-security-tutorial/server-name.png)

    > [!NOTE]
    > Kopieren Sie Ihren vollqualifizierten Servernamen (Beispiel: *<Ihr Server>.database.windows.net*). Er wird im weiteren Verlauf dieses Tutorials benötigt.

1. Wählen Sie auf der Seite **Übersicht** die Option **Serverfirewall festlegen** aus. Die Seite **Firewalleinstellungen** für den Datenbankserver wird geöffnet.

   1. Wählen Sie in der Symbolleiste die Option **Client-IP-Adresse hinzufügen** aus, um Ihre aktuelle IP-Adresse einer neuen Firewallregel hinzuzufügen. Die Regel kann den Port 1433 für eine einzelne IP-Adresse oder einen Bereich von IP-Adressen öffnen. Wählen Sie **Speichern** aus.

      ![Festlegen der Serverfirewallregel](./media/sql-database-security-tutorial/server-firewall-rule2.png)

   1. Wählen Sie **OK** aus, und schließen Sie die Seite **Firewalleinstellungen**.

Sie können nun eine Verbindung zu einer beliebigen Datenbank auf dem Server mit der angegebenen IP-Adresse oder dem angegebenen IP-Adressbereich herstellen.

### <a name="setup-database-firewall-rules"></a>Einrichten von Firewallregeln für eine Datenbank

Firewallregeln auf Datenbankebene gelten nur für einzelne Datenbanken. Die Datenbank behält diese Regeln bei einem Serverfailover bei. Firewallregeln auf Datenbankebene können nur mithilfe von T-SQL-Anweisungen (Transact-SQL) konfiguriert werden, und es muss bereits eine Firewallregel auf Serverebene konfiguriert worden sein.

So richten Sie eine Firewallregel auf Datenbankebene ein:

1. Stellen Sie eine Verbindung mit Ihrer Datenbank her (beispielsweise mithilfe von [SQL Server Management Studio](./sql-database-connect-query-ssms.md)).

1. Klicken Sie im **Objekt-Explorer** mit der rechten Maustaste auf die Datenbank, und wählen Sie **Neue Abfrage** aus.

1. Fügen Sie im Abfragefenster die folgende Anweisung hinzu, und ändern Sie die IP-Adresse in Ihre öffentliche IP-Adresse:

    ```sql
    EXECUTE sp_set_database_firewall_rule N'Example DB Rule','0.0.0.4','0.0.0.4';
    ```

1. Wählen Sie auf der Symbolleiste die Option **Ausführen** aus, um die Firewallregel zu erstellen.

> [!NOTE]
> Sie können in SSMS mithilfe des Befehls [sp_set_firewall_rule](/sql/relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database?view=azuresqldb-current) auch eine Firewallregel auf Serverebene erstellen. In diesem Fall muss allerdings eine Verbindung mit der *Masterdatenbank* bestehen.

## <a name="create-an-azure-ad-admin"></a>Erstellen eines Azure AD-Administrators

Vergewissern Sie sich, dass Sie die richtige verwaltete Azure AD-Domäne (Azure Active Directory) verwenden. Die AD-Domäne kann rechts oben im Azure-Portal ausgewählt werden. Mit diesem Prozess wird sichergestellt, dass sowohl für Azure AD als auch für die SQL Server-Instanz, die Ihre Azure SQL-Datenbank oder Ihr Data Warehouse hostet, das gleiche Abonnement verwendet wird.

   ![Administrator auswählen](./media/sql-database-security-tutorial/8choose-ad.png)

So legen Sie den Azure AD-Administrator fest:

1. Wählen Sie im Azure-Portal auf der Seite **SQL Server** die Option **Active Directory-Administrator** aus. Wählen Sie als Nächstes **Administrator festlegen** aus.

    ![Auswählen von Active Directory](./media/sql-database-security-tutorial/admin-settings.png)  

    > [!IMPORTANT]
    > Für diese Aufgabe müssen Sie entweder Unternehmensadministrator oder globaler Administrator sein.

1. Suchen Sie auf der Seite **Administrator hinzufügen** nach dem AD-Benutzer oder der Gruppe, wählen Sie den Benutzer oder die Gruppe aus, und wählen Sie anschließend **Auswählen** aus. Alle Mitglieder und Gruppen Ihrer Active Directory-Instanz werden aufgeführt. Abgeblendete Einträge werden nicht als Azure AD-Administratoren unterstützt. Weitere Informationen finden Sie unter [Funktionen und Einschränkungen von Azure AD](sql-database-aad-authentication.md#azure-ad-features-and-limitations).

    ![Auswählen des Administrators](./media/sql-database-security-tutorial/admin-select.png)

    > [!IMPORTANT]
    > Die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) gilt nur für das Portal und wird nicht an SQL Server weitergegeben.

1. Wählen Sie oben auf der Seite **Active Directory-Administrator** die Option **Speichern** aus.

    Das Ändern des Administrators kann mehrere Minuten dauern. Der neue Administrator wird im Feld **Active Directory-Administrator** angezeigt.

> [!NOTE]
> Wenn Sie einen Azure AD-Administrator festlegen, darf der Name des neuen Administrators (Benutzer oder Gruppe) nicht als SQL Server-Authentifizierungsbenutzer in der *Masterdatenbank* vorhanden sein. Andernfalls ist die Einrichtung nicht erfolgreich, und die Änderungen werden mit dem Hinweis, dass ein solcher Administratorname bereits vorhanden ist, rückgängig gemacht. Da der SQL Server-Authentifizierungsbenutzer nicht Teil von Azure AD ist, kann für den Benutzer keine Verbindung unter Verwendung der Azure AD-Authentifizierung hergestellt werden.

Informationen zum Konfigurieren von Azure AD finden Sie hier:

- [Was ist eine Hybrididentität?](../active-directory/hybrid/whatis-hybrid-identity.md)
- [Hinzufügen Ihres benutzerdefinierten Domänennamens über das Azure Active Directory-Portal](../active-directory/active-directory-domains-add-azure-portal.md)
- [Windows Azure now supports federation with Windows Server Active Directory](https://azure.microsoft.com/blog/20../../windows-azure-now-supports-federation-with-windows-server-active-directory/) (Microsoft Azure unterstützt jetzt die Verbunderstellung mit Windows Server AD)
- [Verwalten Ihres Azure AD-Verzeichnisses](../active-directory/fundamentals/active-directory-administer.md)
- [Übersicht über Azure PowerShell](/powershell/azure/overview?view=azureadps-2.0)
- [Erforderliche Ports und Protokolle für die Hybrid-Identität](../active-directory/hybrid/reference-connect-ports.md)

## <a name="manage-database-access"></a>Verwalten des Datenbankzugriffs

Verwalten Sie den Zugriff auf die Datenbank, indem Sie der Datenbank Benutzer hinzufügen oder Benutzern den Zugriff über sichere Verbindungszeichenfolgen ermöglichen. Verbindungszeichenfolgen sind hilfreich für externe Anwendungen. Weitere Informationen finden Sie unter [Zugriffssteuerung für Azure SQL-Datenbank und SQL Data Warehouse](sql-database-control-access.md) sowie unter [Verwenden der Azure Active Directory-Authentifizierung für die Authentifizierung mit SQL](sql-database-aad-authentication.md).

Wählen Sie zum Hinzufügen von Benutzern den Datenbankauthentifizierungstyp aus:

- **SQL-Authentifizierung:** Verwenden Sie für Anmeldungen eine Kombination aus Benutzername und Kennwort, die nur im Kontext einer bestimmten Datenbank auf dem Server gültig ist.

- **Azure AD-Authentifizierung:** Verwenden Sie von Azure Active Directory verwaltete Identitäten.

### <a name="sql-authentication"></a>SQL-Authentifizierung

So fügen Sie einen Benutzer mit SQL-Authentifizierung hinzu:

1. Stellen Sie eine Verbindung mit Ihrer Datenbank her (beispielsweise mithilfe von [SQL Server Management Studio](./sql-database-connect-query-ssms.md)).

1. Klicken Sie im **Objekt-Explorer** mit der rechten Maustaste auf die Datenbank, und wählen Sie **Neue Abfrage** aus.

1. Geben Sie im Abfragefenster den folgenden Befehl ein:

    ```sql
    CREATE USER ApplicationUser WITH PASSWORD = 'YourStrongPassword1';
    ```

1. Wählen Sie auf der Symbolleiste die Option **Ausführen** aus, um den Benutzer zu erstellen.

1. Standardmäßig kann der Benutzer eine Verbindung zur Datenbank herstellen, besitzt jedoch keine Berechtigungen zum Lesen oder Schreiben von Daten. Führen Sie in einem neuen Abfragefenster die folgenden Befehle aus, um diese Berechtigungen zu gewähren:

    ```sql
    ALTER ROLE db_datareader ADD MEMBER ApplicationUser;
    ALTER ROLE db_datawriter ADD MEMBER ApplicationUser;
    ```

> [!NOTE]
> Erstellen Sie auf der Datenbankebene Konten ohne Administratorrechte, es sei denn, es müssen Administratoraufgaben wie etwa die Erstellung neuer Benutzer ausgeführt werden.

### <a name="azure-ad-authentication"></a>Azure AD-Authentifizierung

Für die Azure Active Directory-Authentifizierung müssen Datenbankbenutzer als Benutzer für eigenständige Datenbanken erstellt werden. Benutzer für eigenständige Datenbanken sind einer Identität im Azure AD-Verzeichnis zugeordnet, die wiederum mit der Datenbank verknüpft ist, und verfügen über keine Anmeldung in der *Masterdatenbank*. Die Azure AD-Identität kann entweder für einen einzelnen Benutzer oder für eine Gruppe verwendet werden. Weitere Informationen finden Sie unter [Eigenständige Datenbankbenutzer - machen Sie Ihre Datenbank portabel](https://msdn.microsoft.com/library/ff929188.aspx). Im Tutorial [Konfigurieren und Verwalten der Azure Active Directory-Authentifizierung mit SQL](./sql-database-aad-authentication-configure.md) finden Sie außerdem Informationen zur Authentifizierung mithilfe von Azure AD.

> [!NOTE]
> Datenbankbenutzer (mit Ausnahme von Administratoren) können nicht über das Azure-Portal erstellt werden. Azure RBAC-Rollen werden nicht an SQL-basierte Server, Datenbanken oder Data Warehouses weitergegeben. Sie dienen lediglich zur Verwaltung von Azure-Ressourcen und gelten nicht für Datenbankberechtigungen.
>
> So gewährt etwa die Rolle *SQL Server-Mitwirkender* keinen Zugriff für die Verbindungsherstellung mit einer Datenbank oder einem Data Warehouse. Die Berechtigung muss mithilfe von T-SQL-Anweisungen direkt in der Datenbank erteilt werden.

> [!IMPORTANT]
> In den T-SQL-Anweisungen `CREATE LOGIN` und `CREATE USER` werden in Benutzernamen keine Sonderzeichen wie Doppelpunkt (`:`) oder kaufmännisches Und-Zeichen (`&`) unterstützt.

So fügen Sie einen Benutzer mit Azure AD-Authentifizierung hinzu:

1. Stellen Sie eine Verbindung mit Ihrer Azure SQL Server-Instanz her, und verwenden Sie dabei ein Azure AD-Konto, das mindestens über die Berechtigung *ALTER ANY USER* verfügt.

1. Klicken Sie im **Objekt-Explorer** mit der rechten Maustaste auf die Datenbank, und wählen Sie **Neue Abfrage** aus.

1. Geben Sie im Abfragefenster den folgenden Befehl ein, und ändern Sie `<Azure_AD_principal_name>` in den Prinzipalnamen des Azure AD-Benutzers oder in den Anzeigenamen der Azure AD-Gruppe:

   ```sql
   CREATE USER <Azure_AD_principal_name> FROM EXTERNAL PROVIDER;
   ```

> [!NOTE]
> In den Datenbankmetadaten sind Azure AD-Benutzer mit `E (EXTERNAL_USER)` und Gruppen mit `X (EXTERNAL_GROUPS)` gekennzeichnet. Weitere Informationen finden Sie unter [sys.database_principals](/sql/relational-databases/system-catalog-views/sys-database-principals-transact-sql).

### <a name="secure-connection-strings"></a>Sichere Verbindungszeichenfolgen

Um eine sichere, verschlüsselte Verbindung zwischen der Clientanwendung und der SQL-Datenbank zu gewährleisten, muss die konfigurierte Verbindungszeichenfolge folgende Kriterien erfüllen:

- Sie muss eine verschlüsselte Verbindung anfordern.
- Sie darf dem Serverzertifikat nicht vertrauen.

Die Verbindung wird unter Verwendung von Transport Layer Security (TLS) hergestellt, und die Gefahr von Man-in-the-Middle-Angriffen wird verringert. Verbindungszeichenfolgen sind pro Datenbank verfügbar und für die Unterstützung von Clienttreibern wie ADO.NET, JDBC, ODBC und PHP vorkonfiguriert. Informationen zu TLS und zur Konnektivität finden Sie unter [Überlegungen zu TLS](sql-database-connect-query.md#tls-considerations-for-sql-database-connectivity).

So kopieren Sie eine sichere Verbindungszeichenfolge:

1. Wählen Sie im Azure-Portal im Menü auf der linken Seite die Option **SQL-Datenbanken** und anschließend auf der Seite **SQL-Datenbanken** Ihre Datenbank aus.

1. Wählen Sie auf der Seite **Übersicht** die Option **Datenbank-Verbindungszeichenfolgen anzeigen** aus.

1. Wählen Sie eine Treiberregisterkarte aus, und kopieren Sie die vollständige Verbindungszeichenfolge.

    ![ADO.NET-Verbindungszeichenfolge](./media/sql-database-security-tutorial/connection.png)

## <a name="enable-security-features"></a>Aktivieren der Sicherheitsfeatures

Azure SQL-Datenbank bietet Sicherheitsfeatures, die über das Azure-Portal zur Verfügung stehen. Diese Features sind sowohl für die Datenbank als auch für den Server verfügbar (mit Ausnahme der Datenmaskierung, die nur für die Datenbank zur Verfügung steht). Weitere Informationen finden Sie unter [Advanced Threat Protection für Azure SQL-Datenbank](sql-database-advanced-data-security.md), [Erste Schritte bei der Überwachung von SQL-Datenbank](sql-database-auditing.md), [Dynamische Datenmaskierung für SQL-Datenbank](sql-database-dynamic-data-masking-get-started.md) und [Transparente Datenverschlüsselung für SQL-Datenbank und Data Warehouse](transparent-data-encryption-azure-sql.md).

### <a name="advanced-data-security"></a>Advanced Data Security

Das Advanced Data Security-Feature erkennt potenzielle Bedrohungen in Echtzeit und stellt Sicherheitswarnungen zu anomalen Aktivitäten bereit. Benutzer können diese verdächtigen Ereignisse mithilfe des Überwachungsfeatures untersuchen und so ermitteln, ob es sich bei dem Ereignis um einen Zugriffsversuch, um eine Verletzung der Datensicherheit oder um einen Missbrauch von Daten in der Datenbank handelt. Benutzern stehen zudem eine Sicherheitsübersicht mit Sicherheitsrisikobewertung sowie das Tool zur Datenermittlung und -klassifizierung zur Verfügung.

> [!NOTE]
> Ein Bespiel für eine Bedrohung ist die Einschleusung von SQL-Befehlen. Dabei schleusen Angreifer schädliche SQL-Befehle in Eingaben von Anwendungen ein. Dies kann dazu führen, dass die Anwendung unwissentlich die schädlichen SQL-Befehle ausführt und Angreifern Zugriff auf die Datenbank gewährt, die dann die darin enthaltenen Daten entwenden oder manipulieren können.

So aktivieren Sie Advanced Data Security:

1. Wählen Sie im Azure-Portal im Menü auf der linken Seite die Option **SQL-Datenbanken** und anschließend auf der Seite **SQL-Datenbanken** Ihre Datenbank aus.

1. Wählen Sie auf der Seite **Übersicht** den Link **Servername** aus. Die Datenbankserverseite wird geöffnet.

1. Navigieren Sie auf der Seite **SQL Server** zum Abschnitt **Sicherheit**, und wählen Sie **Advanced Data Security** aus.

   1. Wählen Sie unter **Advanced Data Security** die Option **EIN** aus, um das Feature zu aktivieren. Wählen Sie ein Speicherkonto zum Speichern der Ergebnisse der Sicherheitsrisikobewertung aus. Klicken Sie dann auf **Speichern**.

      ![Navigationsbereich](./media/sql-database-security-tutorial/threat-settings.png)

      Sie können auch E-Mail-Adressen für den Empfang von Sicherheitswarnungen sowie Speicherdetails und Bedrohungserkennungstypen konfigurieren.

1. Kehren Sie zur Seite **SQL-Datenbanken** Ihrer Datenbank zurück, und wählen Sie im Abschnitt **Sicherheit** die Option **Advanced Data Security** aus. Hier finden Sie verschiedene Sicherheitsindikatoren für die Datenbank.

    ![Bedrohungsstatus](./media/sql-database-security-tutorial/threat-status.png)

Wenn anomale Aktivitäten erkannt werden, erhalten Sie eine E-Mail mit Ereignisinformationen. Hierzu zählen die Art der Aktivität, die Datenbank, der Server, der Zeitpunkt des Ereignisses, mögliche Ursachen sowie empfohlene Schritte zur Untersuchung und Abwehr der potenziellen Bedrohung. Wählen Sie in einer solchen E-Mail den Link für das **Azure SQL-Überwachungsprotokoll** aus, um das Azure-Portal zu starten und relevante Überwachungsdatensätze für den Zeitpunkt des Ereignisses anzuzeigen.

   ![Bedrohungserkennungs-E-Mail](./media/sql-database-security-tutorial/threat-email.png)

### <a name="auditing"></a>Überwachung

Das Überwachungsfeature verfolgt Datenbankereignisse nach und schreibt Ereignisse in ein Überwachungsprotokoll (entweder in Azure-Speicher, in Azure Monitor-Protokollen oder in einem Event Hub). Die Überwachung hilft Ihnen dabei, gesetzliche Bestimmungen einzuhalten, die Datenbankaktivität nachzuvollziehen und Einblicke in Abweichungen und Anomalien zu erhalten, die Hinweise auf potenzielle Sicherheitsverstöße darstellen können.

So aktivieren Sie die Überwachung:

1. Wählen Sie im Azure-Portal im Menü auf der linken Seite die Option **SQL-Datenbanken** und anschließend auf der Seite **SQL-Datenbanken** Ihre Datenbank aus.

1. Wählen Sie im Abschnitt **Sicherheit** die Option **Überwachung** aus.

1. Legen Sie unter **Überwachung** die folgenden Einstellungswerte fest:

   1. Legen Sie **Überwachung** auf **EIN** fest.

   1. Wählen Sie unter **Ziel für Überwachungsprotokoll** eine der folgenden Optionen aus:

       - **Speicher:** Ein Azure-Speicherkonto, in dem Ereignisprotokolle gespeichert werden, die dann als *XEL-Dateien* heruntergeladen werden können.

          > [!TIP]
          > Verwenden Sie zur optimalen Nutzung von Überwachungsberichtvorlagen das gleiche Speicherkonto für alle überwachten Datenbanken.

       - **Log Analytics:** Speichert automatisch Ereignisse, die dann abgefragt und näher analysiert werden können.

           > [!NOTE]
           > Für erweiterte Features wie Analysen, benutzerdefinierte Warnungsregeln und Excel- oder Power BI-Berichte ist ein **Log Analytics-Arbeitsbereich** erforderlich. Ohne Arbeitsbereich ist nur der Abfrage-Editor verfügbar.

       - **Event Hub:** Ermöglicht das Weiterleiten von Ereignissen für die Verwendung in anderen Anwendungen.

   1. Wählen Sie **Speichern** aus.

      ![Überwachungseinstellungen](./media/sql-database-security-tutorial/audit-settings.png)

1. Nun können Sie **Überwachungsprotokolle anzeigen** auswählen, um Datenbankereignisdaten anzuzeigen.

    ![Überwachungsdatensätze](./media/sql-database-security-tutorial/audit-records.png)

> [!IMPORTANT]
> Informationen zur weiteren Anpassung von Überwachungsereignissen mittels PowerShell oder REST-API finden Sie unter [Erste Schritte bei der Überwachung von SQL-Datenbank](sql-database-auditing.md).

### <a name="dynamic-data-masking"></a>Dynamische Datenmaskierung

Das Datenmaskierungsfeature blendet sensible Daten in Ihrer Datenbank automatisch aus.

So aktivieren Sie die Datenmaskierung:

1. Wählen Sie im Azure-Portal im Menü auf der linken Seite die Option **SQL-Datenbanken** und anschließend auf der Seite **SQL-Datenbanken** Ihre Datenbank aus.

1. Wählen Sie im Abschnitt **Sicherheit** die Option **Dynamische Datenmaskierung** aus.

1. Wählen Sie unter **Dynamische Datenmaskierung** die Option **Maske hinzufügen** aus, um eine Maskierungsregel hinzuzufügen. Die zur Auswahl stehenden Datenbankschemas, Tabellen und Spalten werden von Azure automatisch aufgefüllt.

    ![Maskierungseinstellungen](./media/sql-database-security-tutorial/mask-settings.png)

1. Wählen Sie **Speichern** aus. Die ausgewählten Informationen werden nun maskiert, um den Datenschutz zu wahren.

    ![Maskierungsbeispiel](./media/sql-database-security-tutorial/mask-query.png)

### <a name="transparent-data-encryption"></a>Transparent Data Encryption

Das Verschlüsselungsfeature verschlüsselt automatisch Ihre ruhenden Daten und erfordert keine Änderungen an Anwendungen, die auf die verschlüsselte Datenbank zugreifen. Bei neuen Datenbanken ist die Verschlüsselung standardmäßig aktiviert. Daten können auch mit SSMS und dem Feature [Always Encrypted](sql-database-always-encrypted.md) verschlüsselt werden.

So aktivieren oder überprüfen Sie die Verschlüsselung:

1. Wählen Sie im Azure-Portal im Menü auf der linken Seite die Option **SQL-Datenbanken** und anschließend auf der Seite **SQL-Datenbanken** Ihre Datenbank aus.

1. Wählen Sie im Abschnitt **Sicherheit** die Option **Transparent Data Encryption** aus.

1. Legen Sie bei Bedarf die Option **Datenverschlüsselung** auf **EIN** fest. Wählen Sie **Speichern** aus.

    ![Transparent Data Encryption](./media/sql-database-security-tutorial/encryption-settings.png)

> [!NOTE]
> Stellen Sie zum Anzeigen des Verschlüsselungsstatus über [SSMS](./sql-database-connect-query-ssms.md) eine Verbindung mit der Datenbank her, und fragen Sie die Spalte `encryption_state` der Sicht [sys.dm_database_encryption_keys](/sql/relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql) ab. Der Zustand `3` gibt an, dass die Datenbank verschlüsselt ist.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie mit wenigen einfachen Schritten die Sicherheit Ihrer Datenbank verbessern. Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> - Erstellen von Firewallregeln auf Server- und Datenbankebene
> - Konfigurieren eines Azure AD-Administrators (Azure Active Directory)
> - Verwalten des Benutzerzugriffs mit SQL-Authentifizierung, Azure AD-Authentifizierung und sicheren Verbindungszeichenfolgen
> - Aktivieren von Sicherheitsfeatures (beispielsweise Advanced Data Security, Überwachung, Datenmaskierung und Verschlüsselung)

Im nächsten Tutorial erfahren Sie, wie Sie eine geografische Verteilung implementieren.

> [!div class="nextstepaction"]
>[Implementieren einer geografisch verteilten Datenbank](sql-database-implement-geo-distributed-database.md)
