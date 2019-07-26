---
title: include file
description: include file
services: logic-apps
author: ecfan
ms.service: logic-apps
ms.topic: include
ms.date: 05/15/2018
ms.author: estfan
ms.custom: include file
ms.openlocfilehash: da03c5247b8ebe0a3305b08a05d661264497663f
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2019
ms.locfileid: "67178344"
---
* Wenn Sie Azure SQL-Datenbank verwenden, führen Sie die Schritte unter [Herstellen einer Verbindung mit Azure SQL-Datenbank](#connect-azure-sql-db) aus. 

* Wenn Sie SQL Server verwenden, führen Sie die Schritte unter [Herstellen einer Verbindung mit SQL Server](#connect-sql-server) aus.

<a name="connect-azure-sql-db"></a>

### <a name="connect-to-azure-sql-database"></a>Herstellen einer Verbindung mit Azure SQL-Datenbank

1. Wenn Sie durch den SQL-Trigger oder die Aktion aufgefordert werden, Verbindungsinformationen anzugeben, gehen Sie folgendermaßen vor:

   1. Geben Sie einen Namen für die Verbindung an.

   2. Wählen Sie Ihren SQL-Server und dann die Datenbank aus. 

      Die Datenbankliste wird erst nach der Auswahl des SQL-Servers angezeigt.
 
   3. Geben Sie den Benutzernamen und das Kennwort für Ihren Server an.

      Sie finden diese Informationen im Azure-Portal in den Eigenschaften Ihrer SQL-Datenbank oder in der Verbindungszeichenfolge: 
      
      „User ID=<*IhrBenutzername*>“
      <br>
      „Password=<*IhrKennwort*>“

   Dieses Beispiel zeigt die Verbindungsinformationen für einen Trigger, die Schritte gelten aber auch für Aktionen.

   ![Erstellen einer Azure SQL-Datenbank-Verbindung](./media/connectors-create-api-sqlazure/azure-sql-database-create-connection.png)
   <br>
   Sternchen (*) kennzeichnen erforderliche Werte.

   | Eigenschaft | Wert | Details | 
   |----------|-------|---------| 
   | Verbindungsname | <*my-sql-connection*> | Name der Verbindung | 
   | SQL Server Name | <*my-sql-server*> | Name des SQL-Servers |
   | SQL Database Name | <*my-sql-database*>  | Name der SQL-Datenbank | 
   | Username | <*my-sql-username*> | Benutzername für den Zugriff auf die Datenbank |
   | Kennwort | <*my-sql-password*> | Kennwort für den Zugriff auf die Datenbank | 
   |||| 

2. Wenn Sie fertig sind, wählen Sie **Erstellen** aus.

3. Nachdem Sie die Verbindung erstellt haben, fahren Sie mit [Hinzufügen eines SQL-Triggers](#add-sql-trigger) oder [Hinzufügen einer SQL-Aktion](#add-sql-action) fort.

<a name="connect-sql-server"></a>

### <a name="connect-to-sql-server"></a>Herstellen einer Verbindung mit SQL Server

Bevor Sie Ihr Gateway auswählen können, sollten Sie sicherstellen, dass Sie bereits das [Datengateway eingerichtet](https://docs.microsoft.com/azure/logic-apps/logic-apps-gateway-connection) haben. In diesem Fall wird das Gateway bei der Erstellung der Verbindung in der Gatewayliste angezeigt.

1. Wenn Sie durch den SQL-Trigger oder die Aktion aufgefordert werden, Verbindungsinformationen anzugeben, gehen Sie folgendermaßen vor:

   1. Wählen Sie in den Trigger oder die Aktion und dann **Über lokales Datengateway verbinden** aus, damit die SQL Server-Optionen angezeigt werden.

   2. Geben Sie einen Namen für die Verbindung an.

   3. Geben Sie die Adresse des SQL-Servers und den Namen der Datenbank an.
   
      Sie finden diese Informationen in der Verbindungszeichenfolge: 
      
      * „Server=<*AdresseIhresServers*>“
      * „Database=<*NameIhrerDatenbank*>“

   4. Geben Sie den Benutzernamen und das Kennwort für Ihren Server an.

      Sie finden diese Informationen in der Verbindungszeichenfolge: 
      
      * „User ID=<*IhrBenutzername*>“
      * „Password=<*IhrKennwort*>“

   5. Wenn auf dem SQL-Server die Windows- oder die Standardauthentifizierung verwendet wird, wählen Sie den Authentifizierungstyp aus.

   6. Wählen Sie den Namen des lokalen Datengateways aus, das Sie zuvor erstellt haben.
   
      Wenn Ihr Gateway nicht in der Liste angezeigt wird, überprüfen Sie, ob Sie das [Gateway richtig eingerichtet](https://docs.microsoft.com/azure/logic-apps/logic-apps-gateway-connection) haben.

   Dieses Beispiel zeigt die Verbindungsinformationen für einen Trigger, die Schritte gelten aber auch für Aktionen.

   ![Erstellen einer SQL Server-Verbindung](./media/connectors-create-api-sqlazure/sql-server-create-connection.png)
   <br>
   Sternchen (*) kennzeichnen erforderliche Werte.

   | Eigenschaft | Wert | Details | 
   |----------|-------|---------| 
   | Über lokales Datengateway verbinden | Wählen Sie diese Option zuerst für SQL Server-Einstellungen aus. | | 
   | Verbindungsname | <*my-sql-connection*> | Name der Verbindung | 
   | SQL Server Name | <*my-sql-server*> | Name des SQL-Servers |
   | SQL Database Name | <*my-sql-database*>  | Name der SQL-Datenbank |
   | Username | <*my-sql-username*> | Benutzername für den Zugriff auf die Datenbank |
   | Kennwort | <*my-sql-password*> | Kennwort für den Zugriff auf die Datenbank | 
   | Authentication Type | Windows oder Standard | Optional: Der von Ihrer SQL-Server-Instanz verwendete Authentifizierungstyp | 
   | Gateways | <*my-data-gateway*> | Name Ihres lokalen Datengateways | 
   |||| 

2. Wenn Sie fertig sind, wählen Sie **Erstellen** aus. 

3. Nachdem Sie die Verbindung erstellt haben, fahren Sie mit [Hinzufügen eines SQL-Triggers](#add-sql-trigger) oder [Hinzufügen einer SQL-Aktion](#add-sql-action) fort.
