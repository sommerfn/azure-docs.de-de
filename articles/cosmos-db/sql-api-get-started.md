---
title: Erstellen einer .NET-Konsolen-App zum Verwalten von Daten in einem Azure Cosmos DB-SQL-API-Konto
description: Hier erfahren Sie, wie Sie mithilfe einer C#-Konsolenanwendung Azure Cosmos DB-SQL-API-Ressourcen erstellen.
author: kirankumarkolli
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 07/23/2019
ms.author: kirankk
ms.openlocfilehash: 37b6a9947d3cabe1f566f842e321229efe9d03b6
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2019
ms.locfileid: "68598516"
---
# <a name="build-a-net-console-app-to-manage-data-in-azure-cosmos-db-sql-api-account"></a>Erstellen einer .NET-Konsolen-App zum Verwalten von Daten in einem Azure Cosmos DB-SQL-API-Konto

> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [Java](sql-api-java-get-started.md)
> * [Async Java](sql-api-async-java-get-started.md)
> * [Node.js](sql-api-nodejs-get-started.md)
>

Willkommen zum Tutorial zu den ersten Schritten mit der SQL-API von Azure Cosmos DB. Im Rahmen dieses Tutorials erstellen Sie eine Konsolenanwendung, mit der Azure Cosmos DB-Ressourcen erstellt und abgefragt werden können. In diesem Tutorial wird [mindestens Version 3.0](https://www.nuget.org/packages/Microsoft.Azure.Cosmos) des Azure Cosmos DB .NET SDK verwendet, das auf [.NET Framework](https://dotnet.microsoft.com/download) oder [.NET Core](https://dotnet.microsoft.com/download) ausgerichtet werden kann.

In diesem Lernprogramm wird Folgendes behandelt:

> [!div class="checklist"]
> * Erstellen eines Azure Cosmos-Kontos und Herstellen der Verbindung
> * Konfigurieren Ihres Projekts in Visual Studio
> * Erstellen einer Datenbank und eines Containers
> * Hinzufügen von Elementen zum Container
> * Abfragen des Containers
> * CRUD-Vorgänge für das Element
> * Löschen der Datenbank

Sie haben nicht genügend Zeit? Keine Sorge! Die vollständige Lösung ist auf [GitHub](https://github.com/Azure-Samples/cosmos-dotnet-getting-started)verfügbar. Im Abschnitt [Abrufen der vollständigen Projektmappe für das Tutorial](#GetSolution) finden Sie eine Kurzanleitung.

Lassen Sie uns anfangen.

## <a name="prerequisites"></a>Voraussetzungen

* Ein aktives Azure-Konto. Wenn Sie keines besitzen, können Sie sich für ein [kostenloses Konto](https://azure.microsoft.com/free/)registrieren.

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [!INCLUDE [cosmos-db-emulator-vs](../../includes/cosmos-db-emulator-vs.md)]

## <a name="step-1-create-an-azure-cosmos-db-account"></a>Schritt 1: Erstellen eines Azure Cosmos DB-Kontos
Wir erstellen nun ein Azure Cosmos DB-Konto. Wenn Sie bereits über ein Konto verfügen, das Sie verwenden möchten, können Sie diesen Schritt überspringen und mit [Einrichten der Visual Studio-Projektmappe](#SetupVS)fortfahren. Wenn Sie den Azure Cosmos DB-Emulator verwenden, führen Sie die Schritte unter [Azure Cosmos DB-Emulator](local-emulator.md) zum Einrichten des Emulators aus, und fahren Sie dann mit [Einrichten Ihres Visual Studio-Projekts](#SetupVS) fort.

[!INCLUDE [create-dbaccount-preview](../../includes/cosmos-db-create-dbaccount-preview.md)]

## <a id="SetupVS"></a>Schritt 2: Einrichten Ihres Visual Studio-Projekts
1. Öffnen Sie auf Ihrem Computer **Visual Studio 2017**.
1. Wählen Sie im Menü **Datei** die Option **Neu** und anschließend **Projekt** aus.
1. Wählen Sie im Dialogfeld **Neues Projekt** die Option **Visual C#**  / **Konsolen-App (.NET Framework)** aus, geben Sie Ihrem Projekt einen Namen, und klicken Sie dann auf **OK**.
    ![Screenshot des Fensters „Neues Projekt“](./media/sql-api-get-started/dotnet-tutorial-visual-studio-new-project.png)

    > [!NOTE]
    > Wählen Sie bei .NET Core im Dialogfeld **Neues Projekt** die Optionen **Visual C#**  / **Konsolen-App (.NET Core)** aus, geben Sie Ihrem Projekt einen Namen, und klicken Sie dann auf **OK**.

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf die neue Konsolenanwendung, die sich unter Ihrer Visual Studio-Projektmappe befindet, und klicken Sie anschließend auf **NuGet-Pakete verwalten...** .

    ![Screenshot des Kontextmenüs für das Projekt](./media/sql-api-get-started/dotnet-tutorial-visual-studio-manage-nuget.png)
1. Klicken Sie auf der Registerkarte **NuGet** auf **Durchsuchen**, und geben Sie in das Suchfeld **Microsoft.Azure.Cosmos** ein.
1. Suchen Sie in den Ergebnissen nach **Microsoft.Azure.Cosmos**, und klicken Sie auf **Installieren**.
   Die Paket-ID für die SQL-API-Clientbibliothek von Azure Cosmos DB lautet [Microsoft Azure Cosmos DB Client Library](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/).
   ![Screenshot: NuGet-Menü zum Suchen nach dem Azure Cosmos DB-Client-SDK](./media/sql-api-get-started/dotnet-tutorial-visual-studio-manage-nuget-2.png)

    Wenn Sie eine Meldung im Zusammenhang mit der Überprüfung von Änderungen an der Projektmappe erhalten, klicken Sie auf **OK**. Wenn Sie eine Meldung im Zusammenhang mit der Akzeptierung der Lizenz erhalten, klicken Sie auf **Ich stimme zu**.

Prima. Damit ist die Einrichtung abgeschlossen und wir können mit dem Schreiben von Code beginnen. Ein vollständiges Codeprojekt dieses Tutorials finden Sie auf [GitHub](https://github.com/Azure-Samples/cosmos-dotnet-getting-started).

## <a id="Connect"></a>Schritt 3: Herstellen einer Verbindung mit einem Azure Cosmos DB-Konto
1. Ersetzen Sie zunächst in der Datei **Program.cs** die Verweise am Anfang der C#-Anwendung durch die folgenden Verweise:

   ```csharp
   using System;
   using System.Threading.Tasks;
   using System.Configuration;
   using System.Collections.Generic;
   using System.Net;
   using Microsoft.Azure.Cosmos;
   ```

1. Fügen Sie nun in der öffentlichen Klasse ``Program`` die folgenden Konstanten und Variablen hinzu:

    ```csharp
    public class Program
    {
        // ADD THIS PART TO YOUR CODE

        // The Azure Cosmos DB endpoint for running this sample.
        private static readonly string EndpointUri = "<your endpoint here>";
        // The primary key for the Azure Cosmos account.
        private static readonly string PrimaryKey = "<your primary key>";

        // The Cosmos client instance
        private CosmosClient cosmosClient;

        // The database we will create
        private Database database;

        // The container we will create.
        private Container container;

        // The name of the database and container we will create
        private string databaseId = "FamilyDatabase";
        private string containerId = "FamilyContainer";
    }
    ```

    Hinweis: Wenn Sie mit der vorherigen Version des .NET SDK vertraut sind, kennen Sie unter Umständen bereits die Begriffe „Sammlung“ und „Dokument“. Azure Cosmos DB unterstützt mehrere API-Modelle. Daher werden in Version 3.0 und höheren Versionen des .NET SDK die generischen Begriffe „Container“ und „Element“ verwendet. Ein Container kann eine Sammlung, ein Graph oder eine Tabelle sein. Ein Element kann ein Dokument, ein Edge/Vertex oder eine Zeile sein und stellt den Inhalt eines Containers dar. [Weitere Informationen zu Datenbanken, Containern und Elementen](databases-containers-items.md)

1. Rufen Sie im [Azure-Portal](https://portal.azure.com) Ihre Endpunkt-URL und den Primärschlüssel ab.

    Navigieren Sie im Azure-Portal zu Ihrem Azure Cosmos DB-Konto, und klicken Sie auf **Schlüssel**.

    Kopieren Sie den URI im Portal, und fügen Sie ihn in `<your endpoint URL>` in der Datei ```Program.cs``` ein. Kopieren Sie den PRIMÄRSCHLÜSSEL aus dem Portal, und fügen Sie ihn in `<your primary key>` ein.

   ![Screenshot zum Abrufen der Azure Cosmos DB-Schlüssel über das Azure-Portal](./media/sql-api-get-started/dotnet-tutorial-portal-keys.png)

1. Als Nächstes erstellen wir eine neue Instanz von ```CosmosClient``` und richten das Gerüst für das Programm ein.

    Fügen Sie unterhalb der **Main**-Methode eine neue asynchrone Aufgabe mit der Bezeichnung **GetStartedDemoAsync** hinzu. Sie dient zum Instanziieren des neuen ```CosmosClient```-Elements. **GetStartedDemoAsync** wird als Einstiegspunkt verwendet, der Methoden für Azure Cosmos DB-Ressourcen aufruft.

    ```csharp
    public static async Task Main(string[] args)
    {
    }

    // ADD THIS PART TO YOUR CODE
    /*
        Entry point to call methods that operate on Azure Cosmos DB resources in this sample
    */
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
    }
    ```

1. Fügen Sie den folgenden Code hinzu, um die asynchrone Aufgabe **GetStartedDemoAsync** über die **Main**-Methode auszuführen. Mit der **Main** -Methode werden Ausnahmen abgefangen und in die Konsole geschrieben.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=Main)]

1. Drücken Sie **F5** um die Anwendung auszuführen. Durch die Meldung `End of demo, press any key to exit.` in der Ausgabe des Konsolenfensters wird bestätigt, dass die Verbindung mit Azure Cosmos DB hergestellt wurde. Das Konsolenfenster kann nun geschlossen werden.

Glückwunsch! Sie haben erfolgreich eine Verbindung mit einem Azure Cosmos DB-Konto hergestellt. 

## <a name="step-4-create-a-database"></a>Schritt 4: Erstellen einer Datenbank
Eine Datenbank kann mithilfe der Funktion [**CreateDatabaseIfNotExistsAsync**](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmosclient) oder [**CreateDatabaseAsync**](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmosclient) der ``CosmosClient``-Klasse erstellt werden. Eine Datenbank ist ein logischer Container für Elemente, die auf Container aufgeteilt sind.

1. Kopieren Sie die Methode **CreateDatabaseAsync**, und fügen Sie sie unterhalb der Methode **GetStartedDemoAsync** ein. Mit der Funktion **CreateDatabaseAsync** wird eine neue Datenbank mit der ID ``FamilyDatabase`` erstellt (sofern noch nicht vorhanden). Die ID stammt dabei aus dem Feld ``databaseId``. 

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=CreateDatabaseAsync&highlight=7)]

1. Kopieren Sie den Code, und fügen Sie ihn unterhalb der Stelle ein, an der Sie CosmosClient zum Aufrufen der eben hinzugefügten **CreateDatabaseAsync**-Methode instanziiert haben.

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);

        //ADD THIS PART TO YOUR CODE
        await this.CreateDatabaseAsync();
    }
    ```

    Nun sollte der Code mit eingefügtem Endpunkt und Primärschlüssel wie folgt aussehen:

    ```csharp
    using System;
    using System.Threading.Tasks;
    using System.Configuration;
    using System.Collections.Generic;
    using System.Net;
    using Microsoft.Azure.Cosmos;

    namespace CosmosGettingStarted
    {
        class Program
        {
            // The Azure Cosmos DB endpoint for running this sample.
            private static readonly string EndpointUri = "<your endpoint here>";
            // The primary key for the Azure Cosmos account.
            private static readonly string PrimaryKey = "<your primary key>";

            // The Cosmos client instance
            private CosmosClient cosmosClient;

            // The database we will create
            private Database database;

            // The container we will create.
            private Container container;

            // The name of the database and container we will create
            private string databaseId = "FamilyDatabase";
            private string containerId = "FamilyContainer";

            public static async Task Main(string[] args)
            {
                try
                {
                    Console.WriteLine("Beginning operations...");
                    Program p = new Program();
                    await p.GetStartedDemoAsync();
                }
                catch (CosmosException de)
                {
                    Exception baseException = de.GetBaseException();
                    Console.WriteLine("{0} error occurred: {1}\n", de.StatusCode, de);
                }
                catch (Exception e)
                {
                    Console.WriteLine("Error: {0}\n", e);
                }
                finally
                {
                    Console.WriteLine("End of demo, press any key to exit.");
                    Console.ReadKey();
                }
            }

            /// <summary>
            /// Entry point to call methods that operate on Azure Cosmos DB resources in this sample
            /// </summary>
            public async Task GetStartedDemoAsync()
            {
                // Create a new instance of the Cosmos Client
                this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
                await this.CreateDatabaseAsync();
            }

            /// <summary>
            /// Create the database if it does not exist
            /// </summary>
            private async Task CreateDatabaseAsync()
            {
                // Create a new database
                this.database = await this.cosmosClient.CreateDatabaseIfNotExistsAsync(databaseId);
                Console.WriteLine("Created Database: {0}\n", this.database.Id);
            }
        }
    }
    ```

Drücken Sie **F5**, um die Anwendung auszuführen.

Glückwunsch! Sie haben erfolgreich eine Azure Cosmos DB-Datenbank erstellt.  

## <a id="CreateColl"></a>Schritt 5: Erstellen eines Containers
> [!WARNING]
> Durch Aufrufen der Methode **CreateContainerIfNotExistsAsync** wird ein neuer Container erstellt. Dies wirkt sich auf den Preis aus. Weitere Informationen finden Sie auf unserer [Preisseite](https://azure.microsoft.com/pricing/details/cosmos-db/).
>
>

Ein Container kann mit der Funktion [**CreateContainerIfNotExistsAsync**](/dotnet/api/microsoft.azure.cosmos.database.createcontainerifnotexistsasync?view=azure-dotnet#Microsoft_Azure_Cosmos_Database_CreateContainerIfNotExistsAsync_Microsoft_Azure_Cosmos_ContainerProperties_System_Nullable_System_Int32__Microsoft_Azure_Cosmos_RequestOptions_System_Threading_CancellationToken_) oder [**CreateContainerAsync**](/dotnet/api/microsoft.azure.cosmos.database.createcontainerasync?view=azure-dotnet#Microsoft_Azure_Cosmos_Database_CreateContainerAsync_Microsoft_Azure_Cosmos_ContainerProperties_System_Nullable_System_Int32__Microsoft_Azure_Cosmos_RequestOptions_System_Threading_CancellationToken_) in der Klasse **CosmosDatabase** erstellt werden. Ein Container besteht aus Elementen (JSON-Dokumente im Falle der SQL-API) und zugeordneter serverseitiger Anwendungslogik in JavaScript. Dazu gehören beispielsweise gespeicherte Prozeduren, benutzerdefinierte Funktionen und Auslöser.

1. Kopieren Sie die Methode **CreateContainerAsync**, und fügen Sie sie unterhalb der Methode **CreateDatabaseAsync** ein. Mit der Funktion **CreateContainerAsync** wird ein neuer Container mit der ID ``FamilyContainer`` erstellt (sofern noch nicht vorhanden). Die ID stammt dabei aus dem Feld ``containerId`` (partitioniert durch die Eigenschaft ``LastName``).

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=CreateContainerAsync&highlight=9)]

1. Kopieren Sie den Code, und fügen Sie ihn unterhalb der Stelle ein, an der Sie CosmosClient zum Aufrufen der eben hinzugefügten **CreateContainer**-Methode instanziiert haben.

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
        await this.CreateDatabaseAsync();

        //ADD THIS PART TO YOUR CODE
        await this.CreateContainerAsync();
    }
    ```

   Drücken Sie **F5**, um die Anwendung auszuführen.

Glückwunsch! Sie haben erfolgreich einen Azure Cosmos DB-Container erstellt.  

## <a id="CreateDoc"></a>Schritt 6: Hinzufügen von Elementen zum Container
Ein Element kann mithilfe der Funktion [**CreateItemAsync**](/dotnet/api/microsoft.azure.cosmos.container.createitemasync?view=azure-dotnet#Microsoft_Azure_Cosmos_Container_CreateItemAsync__1___0_System_Nullable_Microsoft_Azure_Cosmos_PartitionKey__Microsoft_Azure_Cosmos_ItemRequestOptions_System_Threading_CancellationToken_) der Klasse **CosmosContainer** erstellt werden. Bei Verwendung der SQL-API werden Elemente als Dokumente projiziert. Dabei handelt es sich um benutzerdefinierten (beliebigen) JSON-Inhalt. Sie können jetzt ein Element in Ihren Azure Cosmos DB-Container einfügen.

Zunächst erstellen wir eine **Family**-Klasse, mit der in diesem Beispiel in Azure Cosmos DB gespeicherte Objekte dargestellt werden. Außerdem erstellen wir die Unterklassen **Parent**, **Child**, **Pet** und **Address**, die in **Family** verwendet werden. Hinweis: Das Element muss über eine **Id**-Eigenschaft verfügen, die in JSON als **id** serialisiert wird.

1. Wählen Sie **STRG+UMSCHALT+A**, um das Dialogfeld **Neues Element hinzufügen** zu öffnen. Fügen Sie Ihrem Projekt die neue Klasse **Family.cs** hinzu.

    ![Screenshot: Hinzufügen der neuen Klasse „Family.cs“ zum Projekt](./media/sql-api-get-started/dotnet-tutorial-visual-studio-add-family-class.png)

1. Kopieren Sie jeweils die Klasse **Family**, **Parent**, **Child**, **Pet** und **Address**, und fügen Sie sie in **Family.cs** ein.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Family.cs)]

1. Navigieren Sie zurück zur Datei **Program.cs**, und fügen Sie die Methode **AddItemsToContainerAsync** unterhalb der Methode **CreateContainerAsync** ein.
Mit dem Code wird vor der Erstellung sichergestellt, dass noch kein Element mit der gleichen ID vorhanden ist. Fügen Sie zwei Elemente ein: eines für die Familie Andersen und eines für die Familie Wakefield.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=AddItemsToContainerAsync)]

1. Fügen Sie in der Methode ``GetStartedDemoAsync`` einen Aufruf für ``AddItemsToContainerAsync`` hinzu.

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
        await this.CreateDatabaseAsync();
        await this.CreateContainerAsync();

        //ADD THIS PART TO YOUR CODE
        await this.AddItemsToContainerAsync();
    }
    ```

Drücken Sie **F5**, um die Anwendung auszuführen.

Glückwunsch! Sie haben erfolgreich zwei Azure Cosmos DB-Elemente erstellt.  

## <a id="Query"></a>Schritt 7: Abfragen von Azure Cosmos DB-Ressourcen
Azure Cosmos DB unterstützt umfassende [Abfragen](sql-api-sql-query.md) der in jeder Sammlung gespeicherten JSON-Dokumente. Der folgende Beispielcode zeigt, wie Sie eine Abfrage für die im vorherigen Schritt eingefügten Elemente ausführen.

1. Kopieren Sie die Methode **QueryItemsAsync**, und fügen Sie sie unterhalb der Methode **AddItemsToContainerAsync** ein.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=QueryItemsAsync&highlight=10-11,17-18)]

1. Fügen Sie in der ``GetStartedDemoAsync`` Methode einen Aufruf für ``QueryItemsAsync`` hinzu.

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
        await this.CreateDatabaseAsync();
        await this.CreateContainerAsync();
        await this.AddItemsToContainerAsync();

        //ADD THIS PART TO YOUR CODE
        await this.QueryItemsAsync();
    }
    ```

Drücken Sie **F5**, um die Anwendung auszuführen.

Glückwunsch! Sie haben erfolgreich eine Abfrage für einen Azure Cosmos DB-Container ausgeführt.

## <a id="ReplaceItem"></a>Schritt 8: Ersetzen eines JSON-Elements
Sie aktualisieren nun ein Element in Azure Cosmos DB.

1. Kopieren Sie die Methode **ReplaceFamilyItemAsync**, und fügen Sie sie unterhalb der Methode **QueryItemsAsync** ein. Beachten Sie, dass die Eigenschaft ``IsRegistered`` der Familie und das Element ``Grade`` für eins der Kinder geändert wird.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=ReplaceFamilyItemAsync&highlight=15)]

1. Fügen Sie in der ``GetStartedDemoAsync`` Methode einen Aufruf für ``ReplaceFamilyItemAsync`` hinzu.

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
        await this.CreateDatabaseAsync();
        await this.CreateContainerAsync();
        await this.AddItemsToContainerAsync();
        await this.QueryItemsAsync();

        //ADD THIS PART TO YOUR CODE
        await this.ReplaceFamilyItemAsync();
    }
    ```

   Drücken Sie **F5**, um die Anwendung auszuführen.

Glückwunsch! Sie haben erfolgreich ein Azure Cosmos DB-Element ersetzt.

## <a id="DeleteDocument"></a>Schritt 9: Delete item (Element löschen)
Als Nächstes löschen wir ein Element in Azure Cosmos DB.

1. Kopieren Sie die Methode **DeleteFamilyItemAsync**, und fügen Sie sie unterhalb der Methode **ReplaceFamilyItemAsync** ein.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=DeleteFamilyItemAsync&highlight=10)]

1. Fügen Sie in der ``GetStartedDemoAsync`` Methode einen Aufruf für ``DeleteFamilyItemAsync`` hinzu.

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
        await this.CreateDatabaseAsync();
        await this.CreateContainerAsync();
        await this.AddItemsToContainerAsync();
        await this.QueryItemsAsync();
        await this.ReplaceFamilyItemAsync();

        //ADD THIS PART TO YOUR CODE
        await this.DeleteFamilyItemAsync();
    }
    ```

Drücken Sie **F5**, um die Anwendung auszuführen.

Glückwunsch! Sie haben erfolgreich ein Azure Cosmos DB-Element gelöscht.

## <a id="DeleteDatabase"></a>Schritt 10: Löschen der Datenbank
Im nächsten Schritt wird die Datenbank gelöscht. Beim Löschen der erstellten Datenbank werden die Datenbank und alle untergeordneten Ressourcen (Container, Elemente sowie alle gespeicherten Prozeduren, benutzerdefinierten Funktionen und Auslöser) entfernt. Die **CosmosClient**-Instanz wird ebenfalls entfernt.

1. Kopieren Sie die Methode **DeleteDatabaseAndCleanupAsync**, und fügen Sie sie unterhalb der Methode **DeleteFamilyItemAsync** ein.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=DeleteDatabaseAndCleanupAsync)]

1. Fügen Sie in der ``GetStartedDemoAsync`` Methode einen Aufruf für ``DeleteDatabaseAndCleanupAsync`` hinzu.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=GetStartedDemoAsync&highlight=14)]

Drücken Sie **F5**, um die Anwendung auszuführen.

Glückwunsch! Sie haben erfolgreich eine Azure Cosmos DB-Datenbank gelöscht.

## <a id="Run"></a>Schritt 11: Ausführen der gesamten C#-Konsolenanwendung!
Drücken Sie in Visual Studio F5, um die Anwendung im Debugmodus zu erstellen und auszuführen.

Die Ausgabe der gesamten App sollte in einem Konsolenfenster angezeigt werden. Die Ausgabe zeigt die Ergebnisse der hinzugefügten Abfragen an. Sie sollte mit unten stehendem Beispieltext übereinstimmen.

```
Beginning operations...

Created Database: FamilyDatabase

Created Container: FamilyContainer

Created item in database with id: Andersen.1 Operation consumed 11.43 RUs.

Created item in database with id: Wakefield.7 Operation consumed 14.29 RUs.

Running query: SELECT * FROM c WHERE c.LastName = 'Andersen'

        Read {"id":"Andersen.1","LastName":"Andersen","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":false}

Updated Family [Wakefield,Wakefield.7].
        Body is now: {"id":"Wakefield.7","LastName":"Wakefield","Parents":[{"FamilyName":"Wakefield","FirstName":"Robin"},{"FamilyName":"Miller","FirstName":"Ben"}],"Children":[{"FamilyName":"Merriam","FirstName":"Jesse","Gender":"female","Grade":6,"Pets":[{"GivenName":"Goofy"},{"GivenName":"Shadow"}]},{"FamilyName":"Miller","FirstName":"Lisa","Gender":"female","Grade":1,"Pets":null}],"Address":{"State":"NY","County":"Manhattan","City":"NY"},"IsRegistered":true}

Deleted Family [Wakefield,Wakefield.7]

Deleted Database: FamilyDatabase

End of demo, press any key to exit.
```

Glückwunsch! Sie haben dieses Tutorial abgeschlossen und verfügen über eine funktionierende C#-Konsolenanwendung.

## <a id="GetSolution"></a> Herunterladen der vollständigen Projektmappe für das Tutorial
Falls Sie die Schritte in diesem Tutorial aus Zeitmangel nicht durchführen konnten oder nur die Codebeispiele herunterladen möchten, können Sie sie über [GitHub](https://github.com/Azure-Samples/cosmos-dotnet-getting-started) beziehen. 

Zum Erstellen der GetStarted-Projektmappe benötigen Sie Folgendes:

* Ein aktives Azure-Konto. Wenn Sie keines besitzen, können Sie sich für ein [kostenloses Konto](https://azure.microsoft.com/free/)registrieren.
* Ein [Azure Cosmos DB-Konto][cosmos-db-create-account]
* [GetStarted-Projektmappe](https://github.com/Azure-Samples/cosmos-dotnet-getting-started) (erhältlich auf GitHub)

Um die Verweise auf das Azure Cosmos DB .NET SDK in Visual Studio wiederherzustellen, klicken Sie mit der rechten Maustaste im Projektmappen-Explorer auf die Projektmappe **GetStarted**, und klicken Sie dann auf die Option **NuGet-Pakete wiederherstellen**. Aktualisieren Sie dann in der Datei „App.config“ die Werte „EndPointUri“ und „PrimaryKey“, wie unter [Herstellen einer Verbindung mit einem Azure Cosmos DB-Konto](#Connect) beschrieben.

Das ist alles, nun müssen Sie nur noch die Erstellung durchführen.

## <a name="next-steps"></a>Nächste Schritte
* Möchten Sie ein komplexeres ASP.NET MVC-Tutorial durcharbeiten? Siehe [ASP.NET MVC-Tutorial: Entwicklung von Webanwendungen mit Azure Cosmos DB](sql-api-dotnet-application-preview.md).
* Möchten Sie Azure Cosmos DB nutzen, um Skalierungs- und Leistungstests durchzuführen? Weitere Informationen finden Sie unter [Leistungs- und Skalierungstests mit Azure Cosmos DB](performance-testing.md).
* Informationen zum [Überwachen von Anforderungen, Nutzung und Speicher in Azure Cosmos DB](monitor-accounts.md).
* Fragen Sie unser Beispieldataset im [Query Playground](https://www.documentdb.com/sql/demo)ab.
* Weitere Informationen zu Azure Cosmos DB finden Sie unter [Willkommen bei Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction).

[cosmos-db-create-account]: create-sql-api-java.md#create-a-database-account
