---
title: 'ASP.NET Core MVC-Tutorial für Azure Cosmos DB: Entwicklung von Webanwendungen'
description: 'ASP.NET Core MVC-Tutorial zum Erstellen einer MVC-Webanwendung mit Azure Cosmos DB Speichern von JSON- und Zugriffsdaten über eine To-Do-App, die in Azure App Service gehostet wird: Schrittanleitung im ASP NET Core MVC-Tutorial'
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 06/24/2019
ms.author: sngun
ms.openlocfilehash: 9824e1468604763834e37abe94290d68d81077ab
ms.sourcegitcommit: 80dff35a6ded18fa15bba633bf5b768aa2284fa8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/26/2019
ms.locfileid: "70020124"
---
# <a name="tutorial-develop-an-aspnet-core-mvc-web-application-with-azure-cosmos-db-by-using-net-sdk"></a>Tutorial: Entwickeln einer ASP.NET Core MVC-Webanwendung mit Azure Cosmos DB unter Verwendung des .NET SDK 

> [!div class="op_single_selector"]
> * [.NET](sql-api-dotnet-application.md)
> * [Java](sql-api-java-application.md)
> * [Node.js](sql-api-nodejs-application.md)
> * [Python](sql-api-python-application.md)
> * [Xamarin](mobile-apps-with-xamarin.md)


In diesem Tutorial erfahren Sie, wie Sie mithilfe von Azure Cosmos DB Daten aus einer in Azure gehosteten ASP.NET MVC-Webanwendung speichern und abrufen. In diesem Tutorial verwenden Sie .NET SDK V3. Die folgende Abbildung zeigt die Webseite, die Sie anhand des Beispiels in diesem Artikel erstellen:
 
![Screenshot der in diesem Tutorial erstellten MVC-Webanwendung für Aufgabenlisten: Schrittanleitung im ASP.NET Core MVC-Tutorial](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-image01.png)

Sollten Sie keine Zeit für das Tutorial haben, können Sie das gesamte Beispielprojekt von [GitHub][GitHub] herunterladen.

In diesem Lernprogramm wird Folgendes behandelt:

> [!div class="checklist"]
> * Erstellen eines Azure Cosmos-Kontos
> * Erstellen einer ASP.NET Core MVC-App
> * Herstellen einer Verbindung zwischen App und Azure Cosmos DB 
> * Ausführen von CRUD-Vorgängen für die Daten

> [!TIP]
> Dieses Tutorial setzt Erfahrung mit der Verwendung von ASP.NET Core MVC und Azure App Service voraus. Sollten Sie noch nicht mit ASP.NET Core oder den [erforderlichen Tools](#prerequisites) vertraut sein, empfiehlt es sich, das vollständige Beispielprojekt von [GitHub][GitHub] herunterzuladen, die erforderlichen NuGet-Pakete hinzuzufügen und das Projekt auszuführen. Nachdem Sie das Projekt erstellt haben, können Sie den Artikel lesen, um sich im Kontext des Projekts mit dem Code vertraut zu machen.

## <a name="prerequisites"></a>Voraussetzungen 

Vergewissern Sie sich zunächst, dass Sie über die folgenden Ressourcen verfügen:

* **Ein aktives Azure-Konto**: Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen. 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [!INCLUDE [cosmos-db-emulator-vs](../../includes/cosmos-db-emulator-vs.md)]  

Alle Screenshots in diesem Artikel wurden mithilfe von Microsoft Visual Studio Community 2019 erstellt. Sollte Ihr System mit einer anderen Version konfiguriert sein, unterscheiden sich die Bildschirme und Optionen möglicherweise geringfügig. Wenn Sie die oben aufgeführten Voraussetzungen erfüllen, sollte diese Lösung jedoch funktionieren.

## <a name="create-an-azure-cosmos-account"></a>Schritt 1: Erstellen eines Azure Cosmos-Kontos

Erstellen Sie zunächst ein Azure Cosmos-Konto. Falls Sie bereits über ein SQL-API-Konto für Azure Cosmos DB verfügen oder den Azure Cosmos DB-Emulator für dieses Tutorial verwenden, können Sie direkt mit dem Abschnitt [Erstellen einer neuen ASP.NET MVC-Anwendung](#create-a-new-mvc-application) fortfahren.

[!INCLUDE [create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [keys](../../includes/cosmos-db-keys.md)]

Im nächsten Abschnitt erstellen Sie eine neue ASP.NET Core MVC-Anwendung. 

## <a name="create-a-new-mvc-application"></a>Schritt 2: Erstellen einer neuen ASP.NET Core MVC-Anwendung

1. Wählen Sie in Visual Studio im Menü **Datei** die Option **Neu** und anschließend **Projekt** aus. Das Dialogfeld **Neues Projekt** wird angezeigt.

2. Suchen Sie im Fenster **Neues Projekt** über das Eingabefeld **Nach Vorlagen suchen** nach „Web“, und wählen Sie dann **ASP.NET Core-Webanwendung** aus. 

   ![Erstellen eines neuen ASP.NET Core-Webanwendungsprojekts](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-new-project-dialog.png)

3. Geben Sie im Feld **Name** den Namen des Projekts ein. In diesem Lernprogramm wird der Name "Todo" (Aufgabe) verwendet. Wenn Sie einen anderen Namen verwenden, müssen Sie immer dann, wenn im Tutorial der todo-Namespace erwähnt wird, die bereitgestellten Codebeispiele mit dem von Ihnen verwendeten Anwendungsnamen anpassen. 

4. Klicken Sie auf **Durchsuchen**, um zu dem Ordner zu navigieren, in dem Sie das Projekt erstellen möchten. Klicken Sie auf **Erstellen**. 

5. Das Dialogfeld **Neue ASP.NET Core-Webanwendung erstellen** wird angezeigt. Wählen Sie in der Vorlagenliste **Webanwendung (Model View Controller)** aus.

6. Wählen Sie **OK** aus, und lassen Sie Visual Studio das Gerüst um die leere ASP.NET Core MVC-Vorlage erstellen. 

7. Wenn Visual Studio die Bausteine der MVC-Anwendung erstellt hat, steht Ihnen eine leere ASP.NET-Anwendung zur Verfügung, die lokal ausgeführt werden kann.

## <a name="add-nuget-packages"></a>Schritt 3: Hinzufügen des Azure Cosmos DB-NuGet-Pakets zum Projekt

Nachdem Sie nun über den Großteil des ASP.NET Core MVC-Frameworkcodes verfügen, der für diese Lösung erforderlich ist, fügen Sie als Nächstes die NuGet-Pakete hinzu, die für die Verbindungsherstellung mit Azure Cosmos DB benötigt werden.

1. Das Azure Cosmos DB .NET SDK wird als NuGet-Paket verteilt. Verwenden Sie zum Abrufen des NuGet-Pakets den NuGet-Paket-Manager in Visual Studio, indem Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt klicken und anschließend **NuGet-Pakete verwalten** auswählen.
   
   ![Screenshot der Kontextmenüoptionen für das Webanwendungsprojekt im Projektmappen-Explorer, in dem „NuGet-Pakete verwalten“ hervorgehoben ist.](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-manage-nuget.png)
   
2. Das Dialogfeld **NuGet-Pakete verwalten** wird geöffnet. Geben Sie im NuGet-Feld **Durchsuchen** die Zeichenfolge **Microsoft.Azure.Cosmos** ein. Installieren Sie das in den Ergebnissen enthaltene Paket **Microsoft.Azure.Cosmos**. Dadurch werden das Azure Cosmos DB-Paket sowie alle Abhängigkeiten heruntergeladen und installiert. Wählen Sie im Fenster **Zustimmung zur Lizenz** die Option **Ich stimme zu** aus, um die Installation abzuschließen.
   
   Alternativ können Sie die Paketverwaltungskonsole verwenden, um das NuGet-Paket zu installieren. Wählen Sie dafür im Menü **Extras** die Option **NuGet-Paket-Manager** und anschließend **Paket-Manager-Konsole** aus. Geben Sie an der Eingabeaufforderung den folgenden Befehl ein:
   
   ```bash
   Install-Package Microsoft.Azure.Cosmos
   ```        

3. Nach der Installation des Pakets sollte das Visual Studio-Projekt den Bibliotheksverweis auf Microsoft.Azure.Cosmos enthalten.
  
## <a name="set-up-the-mvc-application"></a>Schritt 4: Einrichten der ASP.NET Core MVC-Anwendung

Fügen Sie der MVC-Anwendung als Nächstes die Modelle, Ansichten und Controller hinzu:

* [Hinzufügen eines Modells](#add-a-model).
* [Hinzufügen eines Controllers](#add-a-controller).
* [Hinzufügen von Ansichten](#add-views).

### <a name="add-a-model"></a> Hinzufügen eines Modells

1. Klicken Sie im**Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner **Modelle**, und wählen Sie **Hinzufügen** > **Klasse** aus. Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.

1. Nennen Sie die neue Klasse **Item.cs**, und wählen Sie **Hinzufügen** aus. 

1. Ersetzen Sie als Nächstes den Code in der Klasse „Item.cs“ durch folgenden Code:

   [!code-csharp[Main](~/samples-cosmosdb-dotnet-core-web-app/src/Models/Item.cs)]
   
   Die in Azure Cosmos DB gespeicherten Daten werden übertragen und im JSON-Format gespeichert. Die Serialisierung/Deserialisierung Ihrer Objekte durch JSON.NET kann mithilfe des Attributs **JsonProperty** gesteuert werden, wie in der erstellten Klasse **Item** veranschaulicht. Sie können nicht nur das Format des an JSON weitergegebenen Eigenschaftennamens steuern, sondern auch Ihre .NET-Eigenschaften umbenennen, wie hier am Beispiel der Eigenschaft **Completed** zu sehen. 

### <a name="add-views"></a>Hinzufügen von Ansichten

Erstellen Sie als Nächstes die drei folgenden Ansichten: 

* [Fügen Sie eine Ansicht zum Auflisten von Elementen hinzu.](#AddItemIndexView)
* [Fügen Sie eine Ansicht für neue Elemente hinzu.](#AddNewIndexView)
* [Fügen Sie eine Ansicht zum Bearbeiten von Elementen hinzu.](#AddEditIndexView)

#### <a name="AddItemIndexView"></a>Hinzufügen einer Ansicht zum Auflisten von Elementen

1. Erweitern Sie im**Projektmappen-Explorer** den Ordner **Ansichten**, klicken Sie mit der rechten Maustaste auf den leeren Ordner **Item**, der zuvor beim Hinzufügen des Elements **ItemController** von Visual Studio erstellt wurde, und klicken Sie auf **Hinzufügen** und dann auf **Ansicht**.
   
   ![Screenshot des Projektmappen-Explorers, in dem der von Visual Studio erstellte Ordner „Element“ gezeigt wird und die Befehle „Hinzufügen“ > „Ansicht“ hervorgehoben sind](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-add-view.png)

2. Aktualisieren Sie im Dialogfeld **Ansicht hinzufügen** folgende Werte:
   
   * Geben Sie im Feld **Ansichtsname** den Namen ***Index*** ein.
   * Wählen Sie im Feld **Vorlage** die Option ***Liste*** aus.
   * Wählen Sie im Feld **Modellklasse** die Option ***Item (todo.Models)*** aus.
   * Geben Sie im Feld für die Layoutseite ***~/Views/Shared/_Layout.cshtml*** ein.
     
   ![Screenshot des Dialogfelds „Ansicht hinzufügen“](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-add-view-dialog.png)

3. Wählen Sie nach dem Hinzufügen dieser Werte die Option **Hinzufügen** aus. Daraufhin erstellt Visual Studio eine neue Vorlagenansicht. Anschließend wird die erstellte CSHTML-Datei geöffnet. Diese Datei wird erst später benötigt und kann in Visual Studio geschlossen werden.

#### <a name="AddNewIndexView"></a>Hinzufügen einer Ansicht für neue Elemente

Gehen Sie wie folgt vor, um eine neue Ansicht für die Elementerstellung zu erstellen:

1. Klicken Sie im**Projektmappen-Explorer** mit der rechten Maustaste wieder auf den Ordner **Element**, und wählen Sie **Hinzufügen** > **Ansicht** aus.

1. Aktualisieren Sie im Dialogfeld **Ansicht hinzufügen** folgende Werte:
   
   * Geben Sie im Feld **Ansichtsname** den Namen ***Create*** ein.
   * Wählen Sie im Feld **Vorlage** die Option ***Erstellen*** aus.
   * Wählen Sie im Feld **Modellklasse** die Option ***Item (todo.Models)*** aus.
   * Geben Sie im Feld für die Layoutseite ***~/Views/Shared/_Layout.cshtml*** ein.
   * Wählen Sie **Hinzufügen**.
   
#### <a name="AddEditIndexView"></a>Hinzufügen einer Ansicht zum Bearbeiten von Elementen

Fügen Sie abschließend eine Ansicht für die Elementbearbeitung hinzu:

1. Klicken Sie im**Projektmappen-Explorer** mit der rechten Maustaste wieder auf den Ordner **Element**, und wählen Sie **Hinzufügen** > **Ansicht** aus.

1. Gehen Sie im Dialogfeld **Ansicht hinzufügen** folgendermaßen vor:
   
   * Geben Sie im Feld **Ansichtsname** den Namen ***Edit*** ein.
   * Wählen Sie im Feld **Vorlage** die Option ***Bearbeiten*** aus.
   * Wählen Sie im Feld **Modellklasse** die Option ***Item (todo.Models)*** aus.
   * Geben Sie im Feld für die Layoutseite ***~/Views/Shared/_Layout.cshtml*** ein.
   * Wählen Sie **Hinzufügen**.

Schließen Sie nach Abschluss dieses Vorgangs alle CSHTML-Dokumente in Visual Studio. Sie werden erst später benötigt.

### <a name="add-a-controller"></a>Hinzufügen eines Controllers

1. Klicken Sie im**Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner **Controller**, und wählen Sie **Hinzufügen** > **Controller** aus. Das Dialogfeld **Gerüst hinzufügen** wird angezeigt.

1. Wählen Sie **MVC-Controller – leer** und anschließend **Hinzufügen** aus.

   ![Screenshot des Dialogfelds „Gerüst hinzufügen“, in dem die Option „MVC-Controller – leer“ hervorgehoben ist](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-controller-add-scaffold.png)

1. Nennen Sie Ihren neuen Controller **ItemController**, und ersetzen Sie den Code in der Datei durch folgenden Code:

   [!code-csharp[Main](~/samples-cosmosdb-dotnet-core-web-app/src/Controllers/ItemController.cs)]

   Das Attribut **ValidateAntiForgeryToken** wird hier verwendet, um den Schutz dieser Anwendung vor websiteübergreifenden Anforderungsfälschungen zu unterstützen. Das Hinzufügen dieses Attributs reicht jedoch nicht aus; Ihre Ansichten müssen ebenfalls mit diesem Antifälschungstoken arbeiten. Weitere Informationen zu diesem Thema sowie Beispiele für eine ordnungsgemäße Implementierung finden Sie unter [Verhindern der websiteübergreifenden Anforderungsfälschung][Preventing Cross-Site Request Forgery]. Der auf [GitHub][GitHub] bereitgestellte Quellcode enthält die vollständige Implementierung.

   Zum Schutz vor Overposting-Angriffen wird außerdem das Attribut **Bind** im Methodenparameter verwendet. Weitere Informationen finden Sie unter [Grundlegende CRUD-Vorgänge in ASP.NET MVC][Basic CRUD Operations in ASP.NET MVC].

## <a name="connect-to-cosmosdb"></a>Schritt 5: Herstellen der Verbindung mit Azure Cosmos DB 

Nachdem Sie sich nun um alle grundlegenden MVC-Elemente gekümmert haben, können Sie den Code für die Verbindungsherstellung mit Azure Cosmos DB sowie für die Ausführung von CRUD-Vorgängen hinzufügen. 

### <a name="perform-crud-operations"></a> Ausführen von CRUD-Vorgängen für die Daten

Hier muss zunächst eine Klasse hinzugefügt werden, die die Logik zur Verbindungsherstellung mit bzw. zur Verwendung von Azure Cosmos DB enthält. In diesem Tutorial wird die entsprechende Logik in einer Klasse namens `CosmosDBService` gekapselt und eine Schnittstelle namens `ICosmosDBService` aufgerufen. Dieser Dienst führt die CRUD- und Lesefeedvorgänge wie etwa das Auflisten unvollständiger Elemente sowie das Erstellen, Bearbeiten und Löschen der Elemente aus. 

1. Erstellen Sie über den**Projektmappen-Explorer** unter Ihrem Projekt einen neuen Ordner namens **Services**.

1. Klicken Sie mit der rechten Maustaste auf den Ordner **Services**, und wählen Sie **Hinzufügen** > **Klasse** aus. Nennen Sie die neue Klasse **CosmosDBService**, und wählen Sie **Hinzufügen** aus.

1. Fügen Sie der Klasse **CosmosDBService** den folgenden Code hinzu, und ersetzen Sie den Code in dieser Datei durch folgenden Code:

   [!code-csharp[Main](~/samples-cosmosdb-dotnet-core-web-app/src/Services/CosmosDbService.cs)]

1. Wiederholen Sie die Schritte 2 und 3 dieses Mal für eine Klasse mit dem Namen **ICosmosDBService**, und fügen Sie den folgenden Code hinzu:

   [!code-csharp[Main](~/samples-cosmosdb-dotnet-core-web-app/src/Services/ICosmosDbService.cs)]
 
1. Der vorherige Code empfängt `CosmosClient` als Teil des Konstruktors. Navigieren Sie gemäß der ASP.NET Core-Pipeline zur Datei **Startup.cs** des Projekts, und initialisieren Sie den Client basierend auf der Konfiguration als Singleton-Instanz, die per [Abhängigkeitsinjektion](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) injiziert wird. Im Handler **ConfigureServices** wird Folgendes definiert:

    ```csharp
    services.AddSingleton<ICosmosDbService>(InitializeCosmosClientInstanceAsync(Configuration.GetSection("CosmosDb")).GetAwaiter().GetResult());
    ```

1. In derselben Datei definieren wir die Hilfsmethode **InitializeCosmosClientInstanceAsync**, mit der die Konfiguration gelesen und der Client initialisiert wird.

    [!code-csharp[](~/samples-cosmosdb-dotnet-core-web-app/src/Startup.cs?name=InitializeCosmosClientInstanceAsync)] 

1. Die Konfiguration wird in der Datei **appsettings.json** des Projekts definiert. Öffnen Sie die Datei, und fügen Sie einen Abschnitt namens **CosmosDb** hinzu:

   ```csharp
     "CosmosDb": {
        "Account": "<enter the URI from the Keys blade of the Azure Portal>",
        "Key": "<enter the PRIMARY KEY, or the SECONDARY KEY, from the Keys blade of the Azure  Portal>",
        "DatabaseName": "Tasks",
        "ContainerName": "Items"
      }
   ```
 
Wenn Sie die Anwendung ausführen, instanziiert die ASP.NET Core-Pipeline nun **CosmosDbService** und behält eine einzelne Instanz als Singleton bei. Wird **ItemController** zum Verarbeiten clientseitiger Anforderungen verwendet, wird diese einzelne Instanz empfangen und kann zum Ausführen von CRUD-Vorgängen verwendet werden.

Wenn Sie dieses Projekt jetzt erstellen und ausführen, sollte ein Ergebnis ähnlich dem folgenden angezeigt werden:

![Screenshot der in diesem Datenbanktutorial erstellten Aufgabenlisten-Webanwendung](./media/sql-api-dotnet-application/build-and-run-the-project-now.png)


## <a name="run-the-application"></a>Schritt 6: Lokales Ausführen der Anwendung

Gehen Sie wie folgt vor, um die Anwendung lokal zu testen:

1. Drücken Sie in Visual Studio F5, um die Anwendung im Debugmodus zu erstellen. Die Anwendung sollte erstellt, und ein Browser mit der zuvor angezeigten leeren Rasterseite sollte geöffnet werden:
   
   ![Screenshot der in diesem Tutorial erstellten Aufgabenlisten-Webanwendung](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-create-an-item-a.png)
       
2. Klicken Sie auf den Link **Neu erstellen**, und fügen Sie Werte in die Felder **Name** und **Beschreibung** ein. Lassen Sie das Kontrollkästchen **Abgeschlossen** deaktiviert, da das neue Element ansonsten im Zustand „Abgeschlossen“ hinzugefügt und nicht in der Anfangsliste angezeigt wird.
   
3. Klicken Sie auf **Erstellen**. Sie werden zur Ansicht **Index** zurückgeleitet, und Ihr Element wird in der Liste angezeigt. Sie können Ihrer Aufgabenliste noch ein paar weitere Elemente hinzufügen.

    ![Screenshot der Ansicht „Index“](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-create-an-item.png)
  
4. Wenn Sie auf **Bearbeiten** neben einem **Element** in der Liste klicken, werden Sie zur Ansicht **Edit** weitergeleitet, in der Sie beliebige Eigenschaften Ihres Objekts aktualisieren können, einschließlich der Markierung als **Abgeschlossen**. Wenn Sie das Flag **Abgeschlossen** markieren und auf **Speichern** klicken, wird das **Element** in der Liste als abgeschlossen angezeigt.
   
   ![Screenshot der Ansicht „Index“ mit aktiviertem Feld „Abgeschlossen“](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-completed-item.png)

5. Sie können den Zustand der Daten im Azure Cosmos DB-Dienst jederzeit mit dem [Cosmos-Explorer](https://cosmos.azure.com) oder dem Daten-Explorer des Azure Cosmos DB-Emulators überprüfen.

6. Nachdem Sie die App getestet haben, drücken Sie STRG+F5, um das Debuggen der App zu beenden. Jetzt können Sie Ihre App bereitstellen.

## <a name="deploy-the-application-to-azure"></a>Schritt 7: Bereitstellen der Anwendung 
Nachdem die vollständige Anwendung korrekt mit Azure Cosmos DB zusammenarbeitet, stellen wir diese Web-App in Azure App Service bereit.  

1. Klicken Sie zum Veröffentlichen der Anwendung im**Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **Veröffentlichen** aus.
   
2. Wählen Sie im Dialogfeld **Veröffentlichen** die Option **App Service** und anschließend **Neu erstellen** aus, um ein App Service-Profil zu erstellen, oder wählen Sie **Vorhandenes auswählen** aus, um ein vorhandenes Profil zu verwenden.

3. Falls Sie bereits über ein Azure App Service-Profil verfügen, wählen Sie in der Dropdownliste ein **Abonnement** aus. Sortieren Sie mithilfe des Filters **Ansicht** nach der Ressourcengruppe oder dem Ressourcentyp. Suchen Sie dann nach der erforderlichen Azure App Service-Instanz, und wählen Sie **OK** aus.
   
   ![Dialogfeld „App Service“ in Visual Studio](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-app-service.png)

4. Um ein neues Azure App Service-Profil zu erstellen, klicken Sie im Dialogfeld **Veröffentlichen** auf **Neu erstellen**. Geben Sie im Dialogfeld **App Service erstellen** den Namen Ihrer Web-App und das entsprechende Abonnement, die Ressourcengruppe und den App Service-Plan ein, und wählen Sie **Erstellen** aus.

   ![Dialogfeld „App Service erstellen“ in Visual Studio](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-create-app-service.png)

Nach wenigen Sekunden veröffentlicht Visual Studio Ihre Webanwendung und startet einen Browser mit Ihrem in Azure ausgeführten Projekt.

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie gelernt, wie Sie eine ASP.NET Core MVC-Webanwendung erstellen, die auf in Azure Cosmos DB gespeicherte Daten zugreifen kann. Sie können jetzt mit dem nächsten Artikel fortfahren:

* [Partitionierung in Azure Cosmos DB](./partitioning-overview.md)
* [Erste Schritte mit SQL-Abfragen](./how-to-sql-query.md)
* [Modellieren und Partitionieren von Daten in Azure Cosmos DB anhand eines praktischen Beispiels](./how-to-model-partition-example.md)


[Visual Studio Express]: https://www.visualstudio.com/products/visual-studio-express-vs.aspx
[Microsoft Web Platform Installer]: https://www.microsoft.com/web/downloads/platform.aspx
[Preventing Cross-Site Request Forgery]: https://go.microsoft.com/fwlink/?LinkID=517254
[Basic CRUD Operations in ASP.NET MVC]: https://go.microsoft.com/fwlink/?LinkId=317598
[GitHub]: https://github.com/Azure-Samples/cosmos-dotnet-core-todo-app
