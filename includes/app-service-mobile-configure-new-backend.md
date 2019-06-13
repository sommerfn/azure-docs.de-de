---
title: Includedatei
description: Includedatei
services: app-service\mobile
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 05/06/2019
ms.author: crdun
ms.custom: include file
ms.openlocfilehash: 3fe571a5e0b10da3c253bca233aad1e16d2ad852
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2019
ms.locfileid: "66235972"
---
1. Laden Sie die Client-SDK-Schnellstarts für die folgenden Plattformen herunter:
    
    [iOS (Objective-C)](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/iOS) [iOS (Swift)](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/iOS-Swift) [Android (Java)](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/android) [Xamarin.iOS](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/xamarin.iOS) [Xamarin.Android](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/xamarin.android) [Xamarin.Forms](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/xamarin.forms) [Cordova](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/cordova) [Windows (C#)](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/windows-uwp-cs)
        
2. Azure Mobile Apps unterstützen das Back-End-SDK für .NET und Node. Laden Sie je nach App-Typ das [.NET](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/backend/dotnet)- oder das [Node](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/backend/node)-Projekt für das Open Source-Repository herunter.

3. Sie müssen eine Datenbankverbindung hinzufügen oder eine Verbindung mit einer vorhandenen Verbindung herstellen. Legen Sie zunächst fest, ob Sie einen Datenspeicher erstellen oder einen bereits vorhandenen verwenden möchten.

4. **Erstellen eines neuen Datenspeichers**: Wenn Sie vorhaben, einen Datenspeicher zu erstellen, verwenden Sie den folgenden Schnellstart:

    [Schnellstart: Erste Schritte mit Einzeldatenbanken in Azure SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/sql-database-single-database-quickstart-guide)

5. **Vorhandene Datenquelle**: Befolgen Sie die nachstehenden Anweisungen, wenn Sie eine vorhandene Verbindung verwenden möchten.
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
