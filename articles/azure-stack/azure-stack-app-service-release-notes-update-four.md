---
title: App Service in Azure Stack – Versionshinweise zu Update 4 | Microsoft-Dokumentation
description: Erfahren Sie, was in Update 4 für App Service in Azure Stack enthalten ist, welche bekannten Probleme es gibt und wo das Update heruntergeladen werden kann.
services: azure-stack
documentationcenter: ''
author: apwestgarth
manager: stefsch
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2018
ms.author: anwestg
ms.reviewer: anwestg
ms.lastreviewed: 11/13/2018
ms.openlocfilehash: 5108d4f65208f12875ad592e2e9222f8e1fdb130
ms.sourcegitcommit: f7f4b83996640d6fa35aea889dbf9073ba4422f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2019
ms.locfileid: "56991080"
---
# <a name="app-service-on-azure-stack-update-4-release-notes"></a>App Service in Azure Stack: Versionshinweise zu Update 4

*Anwendungsbereich: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*

In diesen Versionshinweisen werden die Verbesserungen und Fixes in Update 4 für Azure App Service in Azure Stack sowie bekannte Probleme beschrieben. Die bekannten Probleme sind in Probleme unterteilt, die sich direkt auf die Bereitstellung und den Updateprozess beziehen, und in Probleme mit dem Build (nach der Installation).

> [!IMPORTANT]
> Wenden Sie Update 1809 auf Ihr integriertes Azure Stack-System an, oder stellen Sie das aktuelle Azure Stack Development Kit vor der Bereitstellung von Azure App Service 1.4 bereit.
>
>

## <a name="build-reference"></a>Buildreferenz

Die Buildnummer von Update 4 für App Service in Azure Stack ist **78.0.13698.5**.

### <a name="prerequisites"></a>Voraussetzungen

Lesen Sie die Dokumentation [Vor den ersten Schritten mit App Service in Azure Stack](azure-stack-app-service-before-you-get-started.md), bevor Sie mit der Bereitstellung beginnen.

Bevor Sie mit dem Upgrade von Azure App Service in Azure Stack auf 1.4 beginnen:

- Stellen Sie sicher, dass alle Rollen in der Azure App Service-Verwaltung im Azure Stack-Verwaltungsportal bereit sind.

- Sichern Sie App Service- und Masterdatenbanken:
  - AppService_Hosting
  - AppService_Metering
  - Master

- Sichern Sie die Inhaltsdateifreigabe der Mandanten-App.

- Syndizieren Sie die benutzerdefinierte Skripterweiterung, Version 1.9, über den Marketplace.

### <a name="new-features-and-fixes"></a>Neue Features und Fehlerbehebungen

Update 4 für Azure App Service in Azure Stack enthält die folgenden Verbesserungen und Fixes:

- Lösung für das XSS-Sicherheitsrisiko [CVE-2018-8600](https://aka.ms/CVE20188600).

- Hinzugefügte Unterstützung für App Service, API-Version 2018-02-01

- Updates für **App Service-Mandanten, Admin, Functions-Portale und Kudu-Tools**. Mit Azure Stack-Portal-SDK-Version konsistent.

- Updates für den Kerndienst zur Verbesserung der Zuverlässigkeit und der Fehlermeldungen, die eine einfachere Diagnose von häufigen Problemen ermöglichen.

- **Updates für folgende Anwendungsframeworks und Tools**:
  - NodeJS 10.6.0 hinzugefügt
  - NPM 6.1.0 hinzugefügt
  - Zulu OpenJDK 8.31.0.2 hinzugefügt
  - Tomcat 8.5.34 und 9.0.12 hinzugefügt
  - Hinzugefügte PHP-Versionen:
    - 5.6.37
    - 7.0.31
    - 7.1.20
    - 7.2.8
  - Update von Python-Versionen:
    - 2.7.15
    - 3.6.6
  - Git für Windows auf v2.17.1.2 aktualisiert
  - Kudu auf 78.11022.3613 aktualisiert
  
- **Updates des zugrunde liegenden Betriebssystems aller Rollen**:
  - [Kumulatives Update für Windows Server 2016 für x64-basierte Systeme aus Oktober 2018 (KB4462928)](https://support.microsoft.com/help/4462928/windows-10-update-kb4462928)

- Das Vorlagenüberprüfungsproblem bei der Bereitstellung von Wordpress-, DNN- und Orchard CMS-Katalogelementen wurde behoben

- Das Konfigurationsproblem, wenn Azure Stack das Azure Resource Manager-Clientzertifikat wechselt, wurde behoben

- Funktionen in den CORS-Einstellungen (Cross-Origin Resource Sharing, Ressourcenfreigabe zwischen verschiedenen Ursprüngen) im App Service-Mandantenportal wurden wiederhergestellt

- Eine Fehlermeldung wird im App Service-Verwaltungsportal angezeigt, wenn die Steuerungsebene des Ressourcenanbieters keine Verbindung mit der konfigurierten SQL Server-Instanz herstellen kann

- Es wird sichergestellt, dass der Endpunkt in der benutzerdefinierten Speicherverbindungszeichenfolge angegeben wird, wenn er in der neuen Funktionsanwendung angegeben wird

### <a name="post-deployment-steps"></a>Schritte nach der Bereitstellung

> [!IMPORTANT]  
> Wenn Sie den App Service-Ressourcenanbieter mit einer SQL Always On-Instanz bereitgestellt haben, MÜSSEN Sie [die Datenbanken „appservice_hosting“ und „appservice_metering“ einer Verfügbarkeitsgruppe hinzufügen](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/availability-group-add-a-database) und die Datenbanken synchronisieren, damit es im Falle eines Datenbankfailovers nicht zu Dienstausfällen kommt.

### <a name="post-update-steps-optional"></a>Schritte nach dem Update (optional)

Führen Sie diese Schritte aus, nachdem das Update für Azure App Service in Azure Stack 1.4 abgeschlossen wurde, falls Sie die Migration zu eigenständigen Datenbanken für vorhandene Bereitstellungen von Azure App Service in Azure Stack durchführen möchten:

> [!IMPORTANT]
> Das Migrationsverfahren dauert etwa 5 bis 10 Minuten.  Das Verfahren umfasst das Beenden der vorhandenen Datenbankanmeldesitzungen.  Planen Sie Ausfallzeiten für die Migration und Überprüfung von Azure App Service in Azure Stack nach der Migration ein.  Wenn Sie diese Schritte nach dem Upgrade auf Azure App Service in Azure Stack 1.3 abgeschlossen haben, sind diese Schritte nicht erforderlich.
>
>

1. Hinzufügen von [App Service-Datenbanken (appservice_hosting und appservice_metering) zu einer Verfügbarkeitsgruppe](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/availability-group-add-a-database)

1. Aktivieren der eigenständigen Datenbank
    ```sql

        sp_configure 'contained database authentication', 1;
        GO
        RECONFIGURE;
            GO
    ```

1. Beim Konvertieren einer Datenbank in eine teilweise eigenständige Datenbank entstehen durch die Konvertierung Ausfallzeiten, da alle aktiven Sitzungen beendet werden müssen.

    ```sql
        /******** [appservice_metering] Migration Start********/
            USE [master];

            -- kill all active sessions
            DECLARE @kill varchar(8000) = '';  
            SELECT @kill = @kill + 'kill ' + CONVERT(varchar(5), session_id) + ';'  
            FROM sys.dm_exec_sessions
            WHERE database_id  = db_id('appservice_metering')

            EXEC(@kill);

            USE [master]  
            GO  
            ALTER DATABASE [appservice_metering] SET CONTAINMENT = PARTIAL  
            GO  

        /********[appservice_metering] Migration End********/

        /********[appservice_hosting] Migration Start********/

            -- kill all active sessions
            USE [master];

            DECLARE @kill varchar(8000) = '';  
            SELECT @kill = @kill + 'kill ' + CONVERT(varchar(5), session_id) + ';'  
            FROM sys.dm_exec_sessions
            WHERE database_id  = db_id('appservice_hosting')

            EXEC(@kill);

            -- Convert database to contained
            USE [master]  
            GO  
            ALTER DATABASE [appservice_hosting] SET CONTAINMENT = PARTIAL  
            GO  

            /********[appservice_hosting] Migration End********/
    '''

1. Migrate Logins to Contained Database Users

    ```sql
        IF EXISTS(SELECT * FROM sys.databases WHERE Name=DB_NAME() AND containment = 1)
        BEGIN
        DECLARE @username sysname ;  
        DECLARE user_cursor CURSOR  
        FOR
            SELECT dp.name
            FROM sys.database_principals AS dp  
            JOIN sys.server_principals AS sp
                ON dp.sid = sp.sid  
                WHERE dp.authentication_type = 1 AND dp.name NOT IN ('dbo','sys','guest','INFORMATION_SCHEMA');
            OPEN user_cursor  
            FETCH NEXT FROM user_cursor INTO @username  
                WHILE @@FETCH_STATUS = 0  
                BEGIN  
                    EXECUTE sp_migrate_user_to_contained
                    @username = @username,  
                    @rename = N'copy_login_name',  
                    @disablelogin = N'do_not_disable_login';  
                FETCH NEXT FROM user_cursor INTO @username  
            END  
            CLOSE user_cursor ;  
            DEALLOCATE user_cursor ;
            END
        GO
    ```

Überprüfen

1. Überprüfen, ob für SQL Server die Eigenständigkeit aktiviert ist

    ```sql
        sp_configure  @configname='contained database authentication'
    ```

1. Überprüfen des vorhandenen eigenständigen Verhaltens
    ```sql
        SELECT containment FROM sys.databases WHERE NAME LIKE (SELECT DB_NAME())
    ```

### <a name="known-issues-post-installation"></a>Bekannte Probleme (nach der Installation)

- Worker können den Dateiserver nicht erreichen, wenn App Service in einem bestehenden virtuellen Netzwerk bereitgestellt wird und der Dateiserver nur im privaten Netzwerk verfügbar ist. Dies ist in der Bereitstellungsdokumentation zu Azure App Service in Azure Stack dargestellt.

Wenn Sie sich für die Bereitstellung in einem bestehenden virtuellen Netzwerk und eine interne IP-Adresse für die Verbindung mit Ihrem Dateiserver entschieden haben, müssen Sie eine Sicherheitsregel für ausgehenden Datenverkehr hinzufügen, die den SMB-Verkehr zwischen dem Workersubnetz und dem Dateiserver ermöglicht. Wechseln Sie im Verwaltungsportal zu WorkersNsg, und fügen Sie eine Sicherheitsregel für ausgehenden Datenverkehr mit den folgenden Eigenschaften hinzu:
 * Quelle: Beliebig
 * Quellportbereich: *
 * Ziel: IP-Adressen
 * IP-Zieladressbereich: Bereich der IPs für Ihren Dateiserver
 * Zielportbereich: 445
 * Protokoll: TCP
 * Aktion: ZULASSEN
 * Priorität: 700
 * Name: Outbound_Allow_SMB445

### <a name="known-issues-for-cloud-admins-operating-azure-app-service-on-azure-stack"></a>Bekannte Probleme von Cloudadministratoren, die Azure App Service in Azure Stack betreiben

Lesen Sie die Dokumentation in den [Versionshinweisen zum Azure Stack-Update 1809](azure-stack-update-1809.md).

## <a name="next-steps"></a>Nächste Schritte

- Einen Überblick über Azure App Service finden Sie unter [Übersicht über App Service in Azure Stack](azure-stack-app-service-overview.md).
- Weitere Informationen zum Vorbereiten der Bereitstellung von App Service in Azure Stack finden Sie unter [Vor den ersten Schritten mit App Service in Azure Stack](azure-stack-app-service-before-you-get-started.md).
