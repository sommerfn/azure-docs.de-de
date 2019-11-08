---
title: 'ASP.NET Core MVC-Tutorial für Azure Cosmos DB: Entwicklung von Webanwendungen'
description: 'ASP.NET Core MVC-Tutorial zum Erstellen einer MVC-Webanwendung mit Azure Cosmos DB Speichern von JSON- und Zugriffsdaten über eine To-Do-App, die in Azure App Service gehostet wird: Schrittanleitung im ASP NET Core MVC-Tutorial'
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 11/05/2019
ms.author: sngun
ms.openlocfilehash: 1cfb7718c55920d817e1f87407fa1af590e9f006
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73720912"
---
# <a name="tutorial-develop-an-aspnet-core-mvc-web-application-with-azure-cosmos-db-by-using-net-sdk"></a>Tutorial: Entwickeln einer ASP.NET Core MVC-Webanwendung mit Azure Cosmos DB unter Verwendung des .NET SDK

> [!div class="op_single_selector"]
> * [.NET](sql-api-dotnet-application.md)
> * [Java](sql-api-java-application.md)
> * [Node.js](sql-api-nodejs-application.md)
> * [Python](sql-api-python-application.md)
> * [Xamarin](mobile-apps-with-xamarin.md)

In diesem Tutorial erfahren Sie, wie Sie mithilfe von Azure Cosmos DB Daten aus einer in Azure gehosteten ASP.NET MVC-Webanwendung speichern und abrufen. In diesem Tutorial verwenden Sie .NET SDK V3. In der folgenden Abbildung ist die Webseite dargestellt, die Sie anhand des Beispiels in diesem Artikel erstellen:

![Screenshot der in diesem Tutorial erstellten MVC-Webanwendung für Aufgabenlisten: Schrittanleitung im ASP.NET Core MVC-Tutorial](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-image01.png)

Sollten Sie keine Zeit für das Tutorial haben, können Sie das gesamte Beispielprojekt von [GitHub][GitHub] herunterladen.

In diesem Lernprogramm wird Folgendes behandelt:

> [!div class="checklist"]
>
> * Erstellen eines Azure Cosmos-Kontos
> * Erstellen einer ASP.NET Core MVC-App
> * Herstellen einer Verbindung zwischen App und Azure Cosmos DB
> * Durchführen von Erstellungs-, Lese-, Aktualisierungs- und Löschvorgängen (Create, Read, Update, Delete (CRUD)) für die Daten

> [!TIP]
> Dieses Tutorial setzt Erfahrung mit der Verwendung von ASP.NET Core MVC und Azure App Service voraus. Sollten Sie noch nicht mit ASP.NET Core oder den [erforderlichen Tools](#prerequisites) vertraut sein, empfiehlt es sich, das vollständige Beispielprojekt von [GitHub][GitHub] herunterzuladen, die erforderlichen NuGet-Pakete hinzuzufügen und das Projekt auszuführen. Nachdem Sie das Projekt erstellt haben, können Sie den Artikel lesen, um sich im Kontext des Projekts mit dem Code vertraut zu machen.

## <a name="prerequisites"></a>Voraussetzungen

Vergewissern Sie sich zunächst, dass Sie über die folgenden Ressourcen verfügen:

* Ein aktives Azure-Konto. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* Visual Studio 2019: [!INCLUDE [cosmos-db-emulator-vs](../../includes/cosmos-db-emulator-vs.md)]  

Alle Screenshots in diesem Artikel stammen aus Microsoft Visual Studio Community 2019. Bei Verwendung einer anderen Version stimmen Ihre Bildschirme und Optionen ggf. nicht vollständig überein. Die Projektmappe sollte funktionieren, sofern Sie die Voraussetzungen erfüllt haben.

## <a name="create-an-azure-cosmos-account"></a>Schritt 1: Erstellen eines Azure Cosmos-Kontos

Erstellen Sie zunächst ein Azure Cosmos-Konto. Falls Sie bereits über ein Azure Cosmos DB-SQL-API-Konto verfügen oder den Azure Cosmos DB-Emulator verwenden, können Sie mit [Schritt 2: Erstellen einer neuen ASP.NET MVC-Anwendung](#create-a-new-mvc-application) fortfahren.

[!INCLUDE [create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [keys](../../includes/cosmos-db-keys.md)]

Im nächsten Abschnitt erstellen Sie eine neue ASP.NET Core MVC-Anwendung.

## <a name="create-a-new-mvc-application"></a>Schritt 2: Erstellen einer neuen ASP.NET Core MVC-Anwendung

1. Öffnen Sie Visual Studio, und wählen Sie **Neues Projekt erstellen** aus.

1. Suchen Sie unter **Neues Projekt erstellen** nach der Option **ASP.NET Core-Webanwendung**  für C#, und wählen Sie sie aus. Klicken Sie auf **Weiter**, um fortzufahren.

   ![Erstellen eines neuen ASP.NET Core-Webanwendungsprojekts](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-new-project-dialog.png)

1. Geben Sie dem Projekt unter **Neues Projekt konfigurieren** den Namen *todo*, und wählen Sie **Erstellen** aus.

1. Wählen Sie unter **Neue ASP.NET Core-Webanwendung erstellen** die Option **Webanwendung (Model-View-Controller)** aus. Klicken Sie auf **Erstellen** , um fortzufahren.

   Visual Studio erstellt eine leere MVC-Anwendung.

1. Wählen Sie **Debuggen** > **Debugging starten** aus (oder F5), um Ihre ASP.NET-Anwendung lokal auszuführen.

## <a name="add-nuget-packages"></a>Schritt 3: Hinzufügen des Azure Cosmos DB-NuGet-Pakets zum Projekt

Nachdem Sie nun über den Großteil des ASP.NET Core MVC-Frameworkcodes verfügen, der für diese Lösung erforderlich ist, fügen Sie als Nächstes die NuGet-Pakete hinzu, die für die Verbindungsherstellung mit Azure Cosmos DB benötigt werden.

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **NuGet-Pakete verwalten** aus.

1. Suchen Sie im **NuGet-Paket-Manager** nach der Option **Microsoft.Azure.Cosmos**, und wählen Sie sie aus. Wählen Sie **Installieren** aus.

   ![Installieren des NuGet-Pakets](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-nuget.png)

   Visual Studio führt den Download und die Installation des Azure Cosmos DB-Pakets und der zugehörigen Abhängigkeiten durch.

   Sie können auch die Option **Paket-Manager-Konsole** verwenden, um das NuGet-Paket zu installieren. Klicken Sie hierzu auf **Extras** > **NuGet-Paket-Manager** > **Paket-Manager-Konsole**. Geben Sie an der Eingabeaufforderung den folgenden Befehl ein:

   ```ps
   Install-Package Microsoft.Azure.Cosmos
   ```
  
## <a name="set-up-the-mvc-application"></a>Schritt 4: Einrichten der ASP.NET Core MVC-Anwendung

Fügen Sie der MVC-Anwendung als Nächstes die Modelle, Ansichten und Controller hinzu.

### <a name="add-a-model"></a> Hinzufügen eines Modells

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner **Modelle**, und wählen Sie **Hinzufügen** > **Klasse** aus.

1. Geben Sie Ihrer neuen Klasse im Feld **Neues Element hinzufügen** den Namen *Item.cs*, und wählen Sie die Option **Hinzufügen**.

1. Ersetzen Sie den Inhalt der *Item.cs*-Klasse durch den folgenden Code:

   [!code-csharp[Main](~/samples-cosmosdb-dotnet-core-web-app/src/Models/Item.cs)]

In Azure Cosmos DB wird JSON-Code zum Verschieben und Speichern von Daten verwendet. Sie können das `JsonProperty`-Attribut verwenden, um zu steuern, wie Objekte mit JSON-Code serialisiert und deserialisiert werden. Anhand der `Item`-Klasse wird das `JsonProperty`-Attribut demonstriert. Mit diesem Code wird das Format des Eigenschaftsnamens gesteuert, der in den JSON-Code eingeht. Außerdem wird die .NET-Eigenschaft in `Completed` umbenannt.

### <a name="add-views"></a>Hinzufügen von Ansichten

Erstellen Sie als Nächstes die drei folgenden Ansichten.

* Hinzufügen einer Ansicht zum Auflisten von Elementen
* Hinzufügen einer Ansicht für neue Elemente
* Hinzufügen einer Ansicht zum Bearbeiten von Elementen

#### <a name="AddItemIndexView"></a>Hinzufügen einer Ansicht zum Auflisten von Elementen

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner **Ansichten**, und wählen Sie **Hinzufügen** > **Neuer Ordner** aus. Geben Sie dem Ordner den Namen *Element*.

1. Klicken Sie mit der rechten Maustaste auf den leeren Ordner **Element**, und wählen Sie anschließend **Hinzufügen** > **Ansicht** aus.

1. Geben Sie unter **Add MVC View** (MVC-Ansicht hinzufügen) die folgenden Werte an:

   * Geben Sie unter **Sichtname** den Namen *Index* ein.
   * Wählen Sie unter **Vorlage** die Option **Liste** aus.
   * Wählen Sie unter **Modellklasse** die Option **Item (todo.Models)** aus.
   * Wählen Sie **Use a layout page** (Layoutseite verwenden) aus, und geben Sie *~/Views/Shared/_Layout.cshtml* ein.

   ![Screenshot: Dialogfeld „MVC-Ansicht hinzufügen“](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-add-mvc-view.png)

1. Wählen Sie nach dem Hinzufügen dieser Werte die Option **Hinzufügen** aus. Daraufhin erstellt Visual Studio eine neue Vorlagenansicht.

Nach Abschluss des Vorgangs öffnet Visual Studio die erstellte *CSHTML*-Datei. Sie können diese Datei in Visual Studio schließen. Wir werden später darauf zurückkommen.

#### <a name="AddNewIndexView"></a>Hinzufügen einer Ansicht für neue Elemente

Gehen Sie wie folgt vor, um eine neue Ansicht für die Elementerstellung zu erstellen:

1. Klicken Sie im **Projektmappen-Explorer** erneut mit der rechten Maustaste auf den Ordner **Element**, und wählen Sie **Hinzufügen** > **Ansicht**.

1. Nehmen Sie unter **Add MVC View** (MVC-Ansicht hinzufügen) die folgenden Änderungen vor:

   * Geben Sie unter **Sichtname** den Namen *Erstellen* ein.
   * Wählen Sie unter **Vorlage** die Option **Erstellen** aus.
   * Wählen Sie unter **Modellklasse** die Option **Item (todo.Models)** aus.
   * Wählen Sie **Use a layout page** (Layoutseite verwenden) aus, und geben Sie *~/Views/Shared/_Layout.cshtml* ein.
   * Wählen Sie **Hinzufügen**.

#### <a name="AddEditIndexView"></a>Hinzufügen einer Ansicht zum Bearbeiten von Elementen

Fügen Sie abschließend eine Ansicht für die Elementbearbeitung hinzu:

1. Klicken Sie im **Projektmappen-Explorer** erneut mit der rechten Maustaste auf den Ordner **Element**, und wählen Sie **Hinzufügen** > **Ansicht** aus.

1. Nehmen Sie unter **Add MVC View** (MVC-Ansicht hinzufügen) die folgenden Änderungen vor:

   * Geben Sie im Feld **Ansichtsname** den Namen *Edit* ein.
   * Wählen Sie im Feld **Vorlage** die Option **Bearbeiten** aus.
   * Wählen Sie im Feld **Modellklasse** die Option **Item (todo.Models)** aus.
   * Wählen Sie **Use a layout page** (Layoutseite verwenden) aus, und geben Sie *~/Views/Shared/_Layout.cshtml* ein.
   * Wählen Sie **Hinzufügen**.

Schließen Sie nach dem Ausführen dieser Schritte alle *CSHTML*-Dokumente in Visual Studio. Wir kommen später noch auf diese Ansichten zurück.

### <a name="add-a-controller"></a>Hinzufügen eines Controllers

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner **Controller**, und wählen Sie **Hinzufügen** > **Controller** aus.

1. Wählen Sie unter **Gerüst hinzufügen** die Option **MVC-Controller – leer** und dann **Hinzufügen** aus.

   ![Auswahl von „MVC-Controller – leer“ unter „Gerüst hinzufügen“](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-controller-add-scaffold.png)

1. Geben Sie dem neuen Controller den Namen *ItemController*.

1. Ersetzen Sie den Inhalt von *ItemController.cs* durch den folgenden Code:

   [!code-csharp[Main](~/samples-cosmosdb-dotnet-core-web-app/src/Controllers/ItemController.cs)]

Das Attribut **ValidateAntiForgeryToken** wird hier verwendet, um den Schutz dieser Anwendung vor websiteübergreifenden Anforderungsfälschungen zu unterstützen. Ihre Ansichten sollten auch mit diesem Fälschungssicherheitstoken funktionieren. Weitere Informationen und Beispiele finden Sie unter [Preventing Cross-Site Request Forgery (CSRF) Attacks in ASP.NET MVC Application][Preventing Cross-Site Request Forgery] (Verhindern von CSRF-Angriffen (Cross-Site Request Forgery) in ASP.NET MVC-Anwendungen). Der auf [GitHub][GitHub] bereitgestellte Quellcode enthält die vollständige Implementierung.

Zum Schutz vor Overposting-Angriffen wird außerdem das Attribut **Bind** im Methodenparameter verwendet. Weitere Informationen finden Sie unter [Tutorial: Implement CRUD Functionality with the Entity Framework in ASP.NET MVC][Basic CRUD Operations in ASP.NET MVC] (Tutorial: Implementieren der CRUD-Funktionalität mit dem Entity Framework in ASP.NET MVC).

## <a name="connect-to-cosmosdb"></a>Schritt 5: Herstellen der Verbindung mit Azure Cosmos DB

Nachdem Sie sich nun um alle grundlegenden MVC-Elemente gekümmert haben, können Sie den Code für die Verbindungsherstellung mit Azure Cosmos DB sowie für die Durchführung von CRUD-Vorgängen hinzufügen.

### <a name="perform-crud-operations"></a>Durchführen von CRUD-Vorgängen für die Daten

Zunächst fügen wir eine Klasse hinzu, die die Logik zur Verbindungsherstellung mit bzw. zur Verwendung von Azure Cosmos DB enthält. In diesem Tutorial wird die entsprechende Logik in einer Klasse namens `CosmosDBService` gekapselt und eine Schnittstelle namens `ICosmosDBService` aufgerufen. Dieser Dienst führt die CRUD-Vorgänge durch. Darüber hinaus führt er Lesefeedvorgänge aus, z. B. das Auflisten unvollständiger Elemente sowie das Erstellen, Bearbeiten und Löschen der Elemente.

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf Ihr Projekt, und wählen Sie **Hinzufügen** > **Neuer Ordner** aus. Geben Sie dem Ordner den Namen *Dienste*.

1. Klicken Sie mit der rechten Maustaste auf den Ordner **Dienste**, und wählen Sie **Hinzufügen** > **Klasse** aus. Nennen Sie die neue Klasse *CosmosDBService*, und wählen Sie **Hinzufügen** aus.

1. Ersetzen Sie den Inhalt von *CosmosDBService.cs* durch den folgenden Code:

   [!code-csharp[Main](~/samples-cosmosdb-dotnet-core-web-app/src/Services/CosmosDbService.cs)]

1. Wiederholen Sie die beiden vorherigen Schritte, aber verwenden Sie dieses Mal den Namen *ICosmosDBService* und den folgenden Code:

   [!code-csharp[Main](~/samples-cosmosdb-dotnet-core-web-app/src/Services/ICosmosDbService.cs)]

1. Fügen Sie im Handler **ConfigureServices** die folgende Zeile hinzu:

    ```csharp
    services.AddSingleton<ICosmosDbService>(InitializeCosmosClientInstanceAsync(Configuration.GetSection("CosmosDb")).GetAwaiter().GetResult());
    ```

    Der Code im vorherigen Schritt empfängt als Teil des Konstruktors ein `CosmosClient`-Element. Basierend auf der ASP.NET Core-Pipeline müssen wir auf die Datei *Startup.cs* des Projekts zugreifen. Mit dem Code in diesem Schritt wird der Client basierend auf der Konfiguration als Singletoninstanz initialisiert, um per [Dependency Injection in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) injiziert zu werden.

1. Fügen Sie in derselben Datei die folgende **InitializeCosmosClientInstanceAsync**-Methode hinzu, mit der die Konfiguration gelesen und der Client initialisiert wird.

    [!code-csharp[](~/samples-cosmosdb-dotnet-core-web-app/src/Startup.cs?name=InitializeCosmosClientInstanceAsync)]

1. Definieren Sie die Konfiguration in der Datei *appsettings.json* des Projekts. Öffnen Sie die Datei, und fügen Sie einen Abschnitt mit dem Namen **CosmosDb** hinzu:

   ```csharp
     "CosmosDb": {
        "Account": "<enter the URI from the Keys blade of the Azure Portal>",
        "Key": "<enter the PRIMARY KEY, or the SECONDARY KEY, from the Keys blade of the Azure  Portal>",
        "DatabaseName": "Tasks",
        "ContainerName": "Items"
      }
   ```

Wenn Sie die Anwendung ausführen, instanziiert die Pipeline von ASP.NET Core **CosmosDbService** und stellt eine Singletoninstanz bereit. Wenn mit **ItemController** clientseitige Anforderungen verarbeitet werden, wird diese einzelne Instanz empfangen und kann für CRUD-Vorgänge verwendet werden.

Wenn Sie dieses Projekt jetzt erstellen und ausführen, sollte ein Ergebnis ähnlich dem folgenden angezeigt werden:

![Screenshot der in diesem Datenbanktutorial erstellten Aufgabenlisten-Webanwendung](./media/sql-api-dotnet-application/build-and-run-the-project-now.png)

## <a name="run-the-application"></a>Schritt 6: Lokales Ausführen der Anwendung

Gehen Sie wie folgt vor, um die Anwendung auf Ihrem lokalen Computer zu testen:

1. Drücken Sie in Visual Studio F5, um die Anwendung im Debugmodus zu erstellen. Die Anwendung sollte erstellt, und ein Browser mit der zuvor angezeigten leeren Rasterseite sollte geöffnet werden:

   ![Screenshot der in diesem Tutorial erstellten Aufgabenlisten-Webanwendung](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-create-an-item-a.png)

1. Wählen Sie den Link **Neu erstellen** aus, und fügen Sie Werte in die Felder **Name** und **Beschreibung** ein. Lassen Sie das Kontrollkästchen **Abgeschlossen** deaktiviert. Wenn Sie es aktivieren, fügt die App das neue Element im abgeschlossenen Zustand hinzu. Das Element wird dann nicht mehr in der ursprünglichen Liste angezeigt.

1. Klicken Sie auf **Erstellen**. Sie erhalten von der App die Ansicht **Index**, und Ihr Element wird in der Liste angezeigt. Sie können Ihrer **Aufgabenliste** noch einige weitere Elemente hinzufügen.

    ![Screenshot der Ansicht „Index“](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-create-an-item.png)
  
1. Wählen Sie neben einem **Element** in der Liste die Option **Bearbeiten** aus. Die App öffnet die Ansicht **Bearbeiten**, in der Sie alle Eigenschaften Ihres Objekts aktualisieren können, einschließlich des Flags **Abgeschlossen**. Wenn Sie **Abgeschlossen** und dann **Speichern** auswählen, zeigt die App das **Element** in der Liste als „Abgeschlossen“ an.

   ![Screenshot der Ansicht „Index“ mit aktiviertem Feld „Abgeschlossen“](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-completed-item.png)

1. Überprüfen Sie den Zustand der Daten im Azure Cosmos DB-Dienst mit dem [Cosmos-Explorer](https://cosmos.azure.com) oder dem Daten-Explorer des Azure Cosmos DB-Emulators.

1. Nachdem Sie die App getestet haben, wählen Sie STRG+F5 aus, um das Debuggen der App zu beenden. Jetzt können Sie Ihre App bereitstellen.

## <a name="deploy-the-application-to-azure"></a>Schritt 7: Bereitstellen der Anwendung

Nachdem die vollständige Anwendung korrekt mit Azure Cosmos DB zusammenarbeitet, stellen wir diese Web-App in Azure App Service bereit.  

1. Klicken Sie zum Veröffentlichen der Anwendung im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **Veröffentlichen** aus.

1. Wählen Sie unter **Veröffentlichungsziel auswählen** die Option **App Service** aus.

1. Wählen Sie zum Verwenden eines vorhandenen App Service-Profils die Option **Vorhandene auswählen** und dann **Veröffentlichen** aus.

1. Wählen Sie unter **App Service** ein **Abonnement** aus. Sortieren Sie mithilfe des Filters **Ansicht** nach der Ressourcengruppe oder dem Ressourcentyp.

1. Suchen Sie nach Ihrem Profil, und wählen Sie anschließend **OK** aus. Suchen Sie dann nach der erforderlichen Azure App Service-Instanz, und wählen Sie **OK** aus.

   ![Dialogfeld „App Service“ in Visual Studio](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-app-service-2019.png)

Eine weitere Möglichkeit ist das Erstellen eines neuen Profils:

1. Klicken Sie wie in der vorherigen Prozedur im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **Veröffentlichen** aus.
  
1. Wählen Sie unter **Veröffentlichungsziel auswählen** die Option **App Service** aus.

1. Wählen Sie unter **Veröffentlichungsziel auswählen** die Option **Neu erstellen** und dann **Veröffentlichen** aus.

1. Geben Sie unter **App Service** den Namen Ihrer Web-App und das entsprechende Abonnement, die Ressourcengruppe und den Hostingplan ein, und wählen Sie anschließend **Erstellen** aus.

   ![Dialogfeld „App Service erstellen“ in Visual Studio](./media/sql-api-dotnet-application/asp-net-mvc-tutorial-create-app-service-2019.png)

Nach wenigen Sekunden veröffentlicht Visual Studio Ihre Webanwendung und startet einen Browser mit Ihrem in Azure ausgeführten Projekt.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde beschrieben, wie Sie eine ASP.NET Core MVC-Webanwendung erstellen. Ihre Anwendung kann auf Daten zugreifen, die in Azure Cosmos DB gespeichert sind. Sie können nun mit diesen Ressourcen fortfahren:

* [Partitioning in Azure Cosmos DB](./partitioning-overview.md) (Partitionierung in Azure Cosmos DB)
* [Erste Schritte mit SQL-Abfragen](./how-to-sql-query.md)
* [Modellieren und Partitionieren von Daten in Azure Cosmos DB anhand eines praktischen Beispiels](./how-to-model-partition-example.md)

[Visual Studio Express]: https://www.visualstudio.com/products/visual-studio-express-vs.aspx
[Microsoft Web Platform Installer]: https://www.microsoft.com/web/downloads/platform.aspx
[Preventing Cross-Site Request Forgery]: https://docs.microsoft.com/aspnet/web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
[Basic CRUD Operations in ASP.NET MVC]: https://go.microsoft.com/fwlink/?LinkId=317598
[GitHub]: https://github.com/Azure-Samples/cosmos-dotnet-core-todo-app
