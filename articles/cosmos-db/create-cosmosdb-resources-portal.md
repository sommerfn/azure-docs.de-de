---
title: Erstellen eines Azure Cosmos-Kontos und eines Containers sowie von Elementen mit dem Azure-Portal.
description: Erstellen eines Azure Cosmos-Kontos und eines Containers sowie von Elementen mit dem Azure-Portal.
author: SnehaGunda
ms.author: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 09/01/2019
ms.openlocfilehash: e0a9f4fa6ca5ff7447d2ffaef3eab2f3c54fdeae
ms.sourcegitcommit: 6794fb51b58d2a7eb6475c9456d55eb1267f8d40
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70241263"
---
# <a name="quickstart-create-an-azure-cosmos-account-container-and-items-with-the-azure-portal"></a>Schnellstart: Erstellen eines Azure Cosmos-Kontos und eines Containers sowie von Elementen mit dem Azure-Portal

> [!div class="op_single_selector"]
> * [Azure-Portal](create-cosmosdb-resources-portal.md)
> * [.NET](create-sql-api-dotnet.md)
> * [Java](create-sql-api-java.md)
> * [Node.js](create-sql-api-nodejs.md)
> * [Python](create-sql-api-python.md)
> * [Xamarin](create-sql-api-xamarin-dotnet.md)
>  

Azure Cosmos DB ist der global verteilte Microsoft-Datenbankdienst mit mehreren Modellen. Sie können Azure Cosmos DB verwenden, um schnell Schlüssel/Wert-, Dokument- und Graph-Datenbanken zu erstellen und abzufragen, und dabei stets die Vorteile der globalen Verteilung und der horizontalen Skalierung nutzen, die Azure Cosmos DB zugrunde liegen. 

In dieser Schnellstartanleitung wird veranschaulicht, wie Sie mithilfe des Azure-Portals ein [SQL-API](sql-api-introduction.md)-Konto, eine Dokumentdatenbank und einen Container für Azure Cosmos DB erstellen und dem Container Daten hinzufügen. 

## <a name="prerequisites"></a>Voraussetzungen

Azure-Abonnement oder kostenloses Azure Cosmos DB-Testkonto
- [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] 

- [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]  

<a id="create-account"></a>
## <a name="create-an-azure-cosmos-db-account"></a>Erstellen eines Azure Cosmos DB-Kontos

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

<a id="create-container-database"></a>
## <a name="add-a-database-and-a-container"></a>Hinzufügen einer Datenbank und eines Containers 

Sie können mit dem Daten-Explorer im Azure-Portal eine Datenbank und einen Container erstellen. 

1.  Wählen Sie auf der Azure Cosmos DB-Kontoseite im linken Navigationsbereich die Option **Daten-Explorer** und dann **Neuer Container** aus. 
    
    Unter Umständen müssen Sie nach rechts scrollen, um das Fenster **Container hinzufügen** anzuzeigen.
    
    ![Daten-Explorer im Azure-Portal, Bereich „Container hinzufügen“](./media/create-sql-api-dotnet/azure-cosmosdb-data-explorer-dotnet.png)
    
1.  Geben Sie im Bereich **Container hinzufügen** die Einstellungen für den neuen Container ein.
    
    |Einstellung|Empfohlener Wert|BESCHREIBUNG
    |---|---|---|
    |**Datenbank-ID**|ToDoList|Geben Sie *ToDoList* als Namen für die neue Datenbank ein. Datenbanknamen müssen zwischen 1 und 255 Zeichen lang sein und dürfen weder `/, \\, #, ?` noch nachgestellte Leerzeichen enthalten. Aktivieren Sie die Option **Provision database throughput** (Datenbankdurchsatz bereitstellen). Diese Option ermöglicht es Ihnen, den für die Datenbank bereitgestellten Durchsatz auf alle Container in der Datenbank zu verteilen. Darüber hinaus hilft sie Ihnen dabei, Kosten zu sparen. |
    |**Durchsatz**|400|Belassen Sie den Durchsatz bei 400 Anforderungseinheiten pro Sekunde (RU/s). Sie können den Durchsatz später zentral hochskalieren, wenn Sie Wartezeiten reduzieren möchten.| 
    |**Container-ID**|Items|Geben Sie *Items* als Namen für den neuen Container ein. Für Container-IDs gelten dieselben Zeichenanforderungen wie für Datenbanknamen.|
    |**Partitionsschlüssel**| /category| Das in diesem Artikel beschriebene Beispiel verwendet */category* als Partitionsschlüssel.|

    
    Fügen Sie für dieses Beispiel keine Schlüssel über die Option **Eindeutige Schlüssel** hinzu. Mit eindeutigen Schlüsseln können Sie die Datenbank mit einer Datenintegritätsebene versehen, indem Sie die Eindeutigkeit von Werten per Partitionsschlüssel sicherstellen. Weitere Informationen finden Sie unter [Eindeutige Schlüssel in Azure Cosmos DB](unique-keys.md).
    
1.  Klicken Sie auf **OK**. Im Daten-Explorer werden die neue Datenbank und der Container angezeigt, die Sie erstellt haben.

## <a name="add-data-to-your-database"></a>Hinzufügen von Daten zu Ihrer Datenbank

Verwenden Sie den Daten-Explorer, um Ihrer neuen Datenbank Daten hinzuzufügen.

1. Erweitern Sie im **Daten-Explorer** die Datenbank **ToDoList** und dann den Container **Elemente**. Wählen Sie als Nächstes **Elemente** und dann **Neues Element** aus. 
   
   ![Neue Dokumente im Daten-Explorer im Azure-Portal erstellen](./media/create-sql-api-dotnet/azure-cosmosdb-new-document.png)
   
1. Fügen Sie dem Dokument rechts im Bereich **Dokumente** die folgende Struktur hinzu:

     ```json
     {
         "id": "1",
         "category": "personal",
         "name": "groceries",
         "description": "Pick up apples and strawberries.",
         "isComplete": false
     }
     ```

1. Wählen Sie **Speichern** aus.
   
   ![Einfügen von JSON-Daten und Wählen von „Speichern“ im Daten-Explorer im Azure-Portal](./media/create-sql-api-dotnet/azure-cosmosdb-save-document.png)
   
1. Wählen Sie erneut **Neues Dokument**, und erstellen und speichern Sie ein weiteres Dokument mit einer eindeutigen `id` und weiteren gewünschten Eigenschaften und Werten. Ihre Dokumente können eine beliebige Struktur aufweisen, da Azure Cosmos DB kein Schema für Ihre Daten vorgibt.

## <a name="query-your-data"></a>Abfragen Ihrer Daten

[!INCLUDE [cosmos-db-create-sql-api-query-data](../../includes/cosmos-db-create-sql-api-query-data.md)] 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie gelernt, wie Sie ein Azure Cosmos DB-Konto, eine Datenbank und einen Container mit dem Daten-Explorer erstellen. Jetzt können Sie zusätzliche Daten in Ihr Azure Cosmos DB-Konto importieren. 

> [!div class="nextstepaction"]
> [Import data into Azure Cosmos DB (Importieren von Daten in Azure Cosmos DB)](import-data.md)