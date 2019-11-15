---
title: Verwenden von Java zum Erstellen einer Dokumentdatenbank – Azure Cosmos DB
description: Hier finden Sie ein Java-Codebeispiel, mit dem Sie eine Verbindung mit der SQL-API von Azure Cosmos DB herstellen und diese API abfragen können.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: quickstart
ms.date: 10/31/2019
ms.author: sngun
ms.custom: seo-java-august2019, seo-java-september2019
ms.openlocfilehash: d5a32780f8598c0843958b99f02cd18aa33bea2e
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73582848"
---
# <a name="quickstart-build-a-java-app-to-manage-azure-cosmos-db-sql-api-data"></a>Schnellstart: Erstellen einer Java-App zum Verwalten von Azure Cosmos DB-SQL-API-Daten


> [!div class="op_single_selector"]
> * [.NET V3](create-sql-api-dotnet.md)
> * [.NET V4](create-sql-api-dotnet-V4.md)
> * [Java](create-sql-api-java.md)
> * [Node.js](create-sql-api-nodejs.md)
> * [Python](create-sql-api-python.md)
> * [Xamarin](create-sql-api-xamarin-dotnet.md)

In dieser Schnellstartanleitung wird gezeigt, wie Sie mithilfe einer Java-Anwendung eine Dokumentdatenbank über Ihr Azure Cosmos DB-Konto für die SQL-API erstellen und verwalten. Sie erstellen zunächst über das Azure-Portal ein Azure Cosmos DB-Konto für die SQL-API und anschließend mithilfe des SQL Java SDK eine Java-App. Dann fügen Sie mithilfe der Java-Anwendung dem Cosmos DB-Konto Ressourcen hinzu. Die Anweisungen in dieser Schnellstartanleitung gelten für alle Betriebssysteme, unter denen Java ausgeführt werden kann. Nach Abschluss dieser Schnellstartanleitung können Sie Cosmos DB-Datenbanken und -Container sowohl über die Benutzeroberfläche als auch programmgesteuert erstellen und ändern.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] 
[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

Außerdem haben Sie folgende Möglichkeiten: 

* [Java Development Kit (JDK), Version 8](https://aka.ms/azure-jdks)
    * Achten Sie darauf, dass die Umgebungsvariable „JAVA_HOME“ auf den Ordner verweist, in dem das JDK installiert ist.
* Ein binäres [Maven](https://maven.apache.org/)-Archiv ([Download](https://maven.apache.org/download.cgi)/[Installationsanleitung](https://maven.apache.org/install.html))
    * Unter Ubuntu können Sie `apt-get install maven` ausführen, um Maven zu installieren.
* [Git-Client](https://www.git-scm.com/)
    * Unter Ubuntu können Sie `sudo apt-get install git` ausführen, um Git zu installieren.

## <a name="create-a-database-account"></a>Erstellen eines Datenbankkontos

Vor dem Erstellen einer Dokumentdatenbank müssen Sie ein SQL-API-Konto mit Azure Cosmos DB erstellen.

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-container"></a>Hinzufügen eines Containers

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

<a id="add-sample-data"></a>
## <a name="add-sample-data"></a>Hinzufügen von Beispieldaten

[!INCLUDE [cosmos-db-create-sql-api-add-sample-data](../../includes/cosmos-db-create-sql-api-add-sample-data.md)]

## <a name="query-your-data"></a>Abfragen Ihrer Daten

[!INCLUDE [cosmos-db-create-sql-api-query-data](../../includes/cosmos-db-create-sql-api-query-data.md)]

## <a name="clone-the-sample-application"></a>Klonen der Beispielanwendung

Beginnen wir nun mit der Verwendung von Code. Klonen Sie zunächst eine SQL-API-App aus GitHub, legen Sie die Verbindungszeichenfolge fest, und führen Sie sie aus. Sie werden feststellen, wie einfach Sie programmgesteuert mit Daten arbeiten können. 

1. Führen Sie den folgenden Befehl aus, um das Beispielrepository zu klonen. Dieser Befehl erstellt eine Kopie der Beispiel-App auf Ihrem Computer.

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-java-getting-started.git
    ```

## <a name="review-the-code"></a>Überprüfen des Codes

Dieser Schritt ist optional. Wenn Sie erfahren möchten, wie die Datenbankressourcen im Code erstellt werden, können Sie sich die folgenden Codeausschnitte ansehen. Andernfalls können Sie mit [Ausführen der App](#run-the-app) fortfahren. 

* `CosmosClient`-Initialisierung `CosmosClient` bietet die clientseitige logische Darstellung für den Azure Cosmos-Datenbankdienst. Mit diesem Client werden Anforderungen für den Dienst konfiguriert und ausgeführt.
    
    [!code-java[](~/azure-cosmosdb-java-v4-getting-started/src/main/java/com/azure/cosmos/sample/sync/SyncMain.java?name=CreateSyncClient)]

* Erstellung einer Cosmos-Datenbank:

    [!code-java[](~/azure-cosmosdb-java-v4-getting-started/src/main/java/com/azure/cosmos/sample/sync/SyncMain.java?name=CreateDatabaseIfNotExists)]

* Erstellung eines Cosmos-Containers:

    [!code-java[](~/azure-cosmosdb-java-v4-getting-started/src/main/java/com/azure/cosmos/sample/sync/SyncMain.java?name=CreateContainerIfNotExists)]

* Erstellung eines Elements mithilfe der Methode `createItem`:

    [!code-java[](~/azure-cosmosdb-java-v4-getting-started/src/main/java/com/azure/cosmos/sample/sync/SyncMain.java?name=CreateItem)]
   
* Punktlesevorgänge werden mithilfe der Methoden `getItem` und `read` ausgeführt:

    [!code-java[](~/azure-cosmosdb-java-v4-getting-started/src/main/java/com/azure/cosmos/sample/sync/SyncMain.java?name=ReadItem)]

* SQL-Abfragen über JSON erfolgen mithilfe der `queryItems`-Methode:

    [!code-java[](~/azure-cosmosdb-java-v4-getting-started/src/main/java/com/azure/cosmos/sample/sync/SyncMain.java?name=QueryItems)]

## <a name="run-the-app"></a>Ausführen der App

Wechseln Sie nun zurück zum Azure-Portal, um die Informationen zur Verbindungszeichenfolge abzurufen und die App mit den Endpunktinformationen zu starten. Dadurch kann Ihre App mit Ihrer gehosteten Datenbank kommunizieren.


1. Wechseln Sie im Git-Terminalfenster mithilfe von `cd` in den Ordner mit dem Beispielcode.

    ```bash
    cd azure-cosmos-java-getting-started
    ```

2. Verwenden Sie im Git-Terminalfenster den folgenden Befehl, um die erforderlichen Java-Pakete zu installieren.

    ```bash
    mvn package
    ```

3. Starten Sie im Git-Terminalfenster die Java-Anwendung mithilfe des folgenden Befehls. (Ersetzen Sie YOUR_COSMOS_DB_HOSTNAME durch den in Anführungszeichen gesetzten URI-Wert aus dem Portal und YOUR_COSMOS_DB_MASTER_KEY durch den in Anführungszeichen gesetzten Primärschlüssel aus dem Portal.)

    ```bash
    mvn exec:java -DACCOUNT_HOST=YOUR_COSMOS_DB_HOSTNAME -DACCOUNT_KEY=YOUR_COSMOS_DB_MASTER_KEY

    ```

    Im Terminalfenster wird eine Benachrichtigung angezeigt, dass die FamilyDB-Datenbank erstellt wurde. 
    
4. Die App erstellt eine Datenbank mit dem Namen `AzureSampleFamilyDB`.
5. Die App erstellt einen Container mit dem Namen `FamilyContainer`.
6. Die App führt Punktlesevorgänge mithilfe der Objekt-IDs und des Partitionsschlüsselwerts aus (in unserem Beispiel „lastName“). 
7. Die App fragt Elemente ab, um alle Familien mit dem entsprechenden Nachnamen („Andersen“, „Wakefield“, „Johnson“) abzurufen.

7. Die App löscht die erstellten Ressourcen nicht. Gehen Sie zurück zum Portal, um in Ihrem Konto [die Ressourcen zu löschen](#clean-up-resources),  damit keine Gebühren anfallen.

## <a name="review-slas-in-the-azure-portal"></a>Überprüfen von SLAs im Azure-Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie gelernt, wie Sie mit dem Daten-Explorer ein Azure Cosmos-Konto, eine Dokumentdatenbank und einen Container erstellen und wie Sie das gleiche Ergebnis programmgesteuert mithilfe einer App erreichen. Sie können jetzt zusätzliche Daten in Ihren Azure Cosmos-Container importieren. 

> [!div class="nextstepaction"]
> [Import data into Azure Cosmos DB (Importieren von Daten in Azure Cosmos DB)](import-data.md)
