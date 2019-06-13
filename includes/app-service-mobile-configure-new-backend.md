---
title: include file
description: include file
services: app-service\mobile
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 05/06/2019
ms.author: crdun
ms.custom: include file
ms.openlocfilehash: a7c994f85d90e94d514bb4e4f91a6644ed45432c
ms.sourcegitcommit: 087ee51483b7180f9e897431e83f37b08ec890ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/31/2019
ms.locfileid: "66455130"
---
1. Laden Sie die Client-SDK-Schnellstarts für die folgenden Plattformen herunter:
    
    [iOS (Objective-C)](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/iOS)  
    [iOS (Swift)](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/iOS-Swift)  
    [Android (Java)](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/android)  
    [Xamarin.iOS](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/xamarin.iOS)  
    [Xamarin.Android](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/xamarin.android)  
    [Xamarin.Forms](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/xamarin.forms)  
    [Cordova](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/cordova)  
    [Windows (C#)](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/windows-uwp-cs)  

    > [!NOTE]
    > Wenn Sie das iOS-Projekt verwenden, müssen Sie zum Herunterladen "Azuresdk-iOS -\*ZIP" von [neueste Version von GitHub](https://github.com/Azure/azure-mobile-apps-ios-client/releases/latest). Entpacken Sie aus, und fügen die `MicrosoftAzureMobile.framework` Datei im Stamm des Projekts.
    >

2. Sie müssen eine Datenbankverbindung hinzufügen oder eine Verbindung mit einer vorhandenen Verbindung herstellen. Legen Sie zunächst fest, ob Sie einen Datenspeicher erstellen oder einen bereits vorhandenen verwenden möchten.

    - **Erstellen Sie einen neuen Datenspeicher**: Wenn Sie vorhaben, einen Datenspeicher zu erstellen, verwenden Sie den folgenden Schnellstart:

        [Schnellstart: Erste Schritte mit Einzeldatenbanken in Azure SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/sql-database-single-database-quickstart-guide)

    - **Vorhandene Datenquelle**: Befolgen Sie die nachstehenden Anweisungen, wenn Sie eine vorhandene Verbindung verwenden möchten.

        1. Format der SQL-Datenbank-Verbindungszeichenfolge: `Data Source=tcp:{your_SQLServer},{port};Initial Catalog={your_catalogue};User ID={your_username};Password={your_password}`

           **{your_SQLServer}** : Name des Servers. Diesen finden Sie auf der Übersichtsseite für Ihre Datenbank, in der Regel im Format „server_name.database.windows.net“.
            **{port}** : Normalerweise 1433.
            **{your_catalogue}** : Name der Datenbank.
            **{your_username}** : Benutzername für den Zugriff auf Ihre Datenbank.
            **{your_password}** : Kennwort für den Zugriff auf Ihre Datenbank.

            [Weitere Informationen zum Format der SQL-Verbindungszeichenfolge](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-string-syntax#sqlclient-connection-strings)

        2. Fügen Sie die Verbindungszeichenfolge Ihrer **mobilen App** hinzu. In App Service können Sie Verbindungszeichenfolgen für Ihre Anwendung über die Option **Konfiguration** im Menü verwalten.

            So fügen Sie eine Verbindungszeichenfolge hinzu:

            1. Klicken Sie auf die Registerkarte **Anwendungseinstellungen**.

            2. Klicken Sie auf **[+] Neue Verbindungszeichenfolge**.

            3. Sie müssen den **Namen**, **Wert** und **Typ** Ihrer Verbindungszeichenfolge angeben.

            4. Geben Sie als **Name** `MS_TableConnectionString` ein.

            5. Der Wert muss der Verbindungszeichenfolge entsprechen, die Sie im vorherigen Schritt zusammengestellt haben.

            6. Wenn Sie eine Verbindungszeichenfolge einer SQL Azure-Datenbank hinzufügen, wählen Sie unter **Typ** die Option **SQLAzure**.

3. Azure Mobile Apps hat SDKs für .NET und Node.js-Back-Ends.

   - **Node.js-Back-End**
    
     Wenn Sie Node.js-Schnellstart-app verwenden möchten, führen Sie die nachstehenden Anweisungen.

     1. Navigieren Sie im Azure-Portal zu **Easy Tables**, wird dieser Bildschirm wird angezeigt.
      
        ![Einfache Knotentabellen](./media/app-service-mobile-configure-new-backend/node-easy-tables.png)

     2. Stellen Sie sicher, dass die SQL-Verbindungszeichenfolge wurde bereits der **Konfiguration** Registerkarte. Aktivieren Sie das Kontrollkästchen der **bestätigen Sie, dass dadurch alle Websiteinhalte überschrieben werden** , und klicken Sie auf die **TodoItem-Tabelle erstellen** Schaltfläche.
     
        ![Knotenkonfiguration für einfache Tabellen](./media/app-service-mobile-configure-new-backend/node-easy-tables-configuration.png)

     3. In **Easy Tables**, klicken Sie auf die **+ Add** Schaltfläche.
    
        ![Schaltfläche "hinzufügen" einfache Tabellen von Knoten](./media/app-service-mobile-configure-new-backend/node-easy-tables-add.png)

     4. Erstellen Sie eine `TodoItem` Tabelle mit anonymem Zugriff.
      
        ![Einfache Knotentabellen hinzufügen Tabelle](./media/app-service-mobile-configure-new-backend/node-easy-tables-table-add.png)

   - **.NET-Back-End**
    
        Wenn Sie .NET Schnellstart-app verwenden möchten, führen Sie die nachstehenden Anweisungen.

        1. Laden Sie die Azure Mobile Apps .NET Server-Projekt aus der [Repository für Azure-Mobile-apps-Schnellstarts](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/backend/dotnet/Quickstart).

        2. Erstellen Sie das .NET Server-Projekt lokal in Visual Studio.

        3. Öffnen Sie in Visual Studio Projektmappen-Explorer, mit der rechten Maustaste auf `ZUMOAPPNAMEService` Projekt, klicken Sie auf **veröffentlichen**, sehen Sie eine `Publish to App Service` Fenster. Wenn Sie auf einem Mac arbeiten, sehen Sie sich andere Möglichkeiten zum Bereitstellen der app [hier](https://docs.microsoft.com/azure/app-service/deploy-local-git).
        
           ![Visual Studio-Veröffentlichung](./media/app-service-mobile-configure-new-backend/visual-studio-publish.png)

        4. Wählen Sie **App Service** Ziel für die Veröffentlichung, klicken Sie dann auf **vorhandene auswählen**, klicken Sie dann auf die **veröffentlichen** am unteren Rand des Fensters.

        5. Sie müssen Visual Studio mit Ihrem Azure-Abonnement zunächst anmelden. Wählen Sie die `Subscription`, `Resource Group`, und wählen Sie dann den Namen Ihrer App. Wenn Sie bereit sind, klicken Sie auf **OK**, dadurch wird das .NET Server-Projekt, die Sie im App Service-Back-End lokal bereitstellen. Nach Abschluss der Bereitstellung werden Sie umgeleitet `http://{zumoappname}.azurewebsites.net/` im Browser.
        
           ![Back-End-ist aktiv.](./media/app-service-mobile-configure-new-backend/backend-is-up.png)
