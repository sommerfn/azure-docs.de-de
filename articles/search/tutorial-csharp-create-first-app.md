---
title: C#-Tutorial zur Erstellung Ihrer ersten App
titleSuffix: Azure Cognitive Search
description: Hier erfahren Sie Schritt für Schritt, wie Sie Ihre erste Such-App erstellen. Das Tutorial enthält sowohl einen Link zu einer funktionierenden App auf GitHub als auch den vollständigen Prozess für die Neuerstellung der App. Informieren Sie sich über die grundlegenden Komponenten von Azure Cognitive Search.
manager: nitinme
author: PeterTurcan
ms.author: v-pettur
ms.service: cognitive-search
ms.topic: tutorial
ms.date: 11/04/2019
ms.openlocfilehash: 3f234a11aeaf7af4e47fb0cf6310ecd68d35e4da
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72794130"
---
# <a name="c-tutorial-create-your-first-app---azure-cognitive-search"></a>C#-Tutorial: Erstellen Ihrer ersten App – Azure Cognitive Search

Hier wird beschrieben, wie Sie eine Webschnittstelle zum Abfragen und Darstellen von Suchergebnissen aus einem Index mit Azure Cognitive Search erstellen. Dieses Tutorial beginnt mit einem vorhandenen gehosteten Index, damit Sie sich auf die Erstellung einer Suchseite konzentrieren können. Der Index enthält fiktive Hoteldaten. Wenn Sie dann über eine einfache Seite verfügen, können Sie diese in den nachfolgenden Lektionen so erweitern, dass sie über eine Paginierung, Facetten und eine Textvervollständigung verfügt.

In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Einrichten einer Entwicklungsumgebung
> * Modellieren von Datenstrukturen
> * Erstellen einer Webseite
> * Methoden definieren
> * Testen der App

Außerdem wird beschrieben, wie einfach die Durchführung eines Suchaufrufs ist. Die zu entwickelnden wichtigen Anweisungen für den Code sind in den folgenden wenigen Zeilen enthalten.

```cs
var parameters = new SearchParameters
{
    // Enter Hotel property names into this list, so only these values will be returned.
    Select = new[] { "HotelName", "Description" }
};

DocumentSearchResult<Hotel> results  = await _indexClient.Documents.SearchAsync<Hotel>("search text", parameters);
```

Mit diesem Aufruf wird eine Suche nach Azure-Daten initiiert, und die Ergebnisse werden zurückgegeben.

![Suchen nach „pool“](./media/tutorial-csharp-create-first-app/azure-search-pool.png)


## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

[Installieren Sie Visual Studio](https://visualstudio.microsoft.com/) für die Verwendung als IDE.

### <a name="install-and-run-the-project-from-github"></a>Installieren und Ausführen des Projekts von GitHub

1. Beispiel auf GitHub: [Erstellen der ersten App](https://github.com/Azure-Samples/azure-search-dotnet-samples).
1. Wählen Sie die Option **Clone or download** (Klonen oder herunterladen), und erstellen Sie eine private lokale Kopie des Projekts.
1. Navigieren Sie mit Visual Studio zur Projektmappe mit der einfachen Suchseite, und öffnen Sie sie. Wählen Sie anschließend die Option **Ohne Debuggen starten** (oder drücken Sie F5).
1. Geben Sie einige Wörter ein (z. B. „wifi“, „view“, „bar“, „parking“), und sehen Sie sich die Ergebnisse an.

    ![Suchen nach „wifi“](./media/tutorial-csharp-create-first-app/azure-search-wifi.png)

Dieses Projekt und Ihre Azure-App werden hoffentlich reibungslos ausgeführt. Viele der grundlegenden Komponenten für komplexere Suchen sind in dieser einen App enthalten. Es ist also hilfreich, die App durchzugehen und Schritt für Schritt neu zu erstellen.

Führen Sie die folgenden Schritte aus, um dieses Projekt von Grund auf neu zu erstellen und so die Komponenten von Azure Cognitive Search noch besser kennenzulernen.

## <a name="set-up-a-development-environment"></a>Einrichten einer Entwicklungsumgebung

1. Wählen Sie in Visual Studio 2017 oder höher die Option **Neu/Projekt** und dann **ASP.NET Core-Webanwendung**. Geben Sie dem Projekt einen Namen wie „FirstAzureSearchApp“.

    ![Erstellen eines Cloudprojekts](./media/tutorial-csharp-create-first-app/azure-search-project1.png)

2. Nachdem Sie für diesen Projekttyp auf **OK** geklickt haben, erhalten Sie eine zweite Gruppe mit Optionen, die für dieses Projekt gelten. Wählen Sie **Webanwendung (Model-View-Controller)** .

    ![Erstellen eines MVC-Projekts](./media/tutorial-csharp-create-first-app/azure-search-project2.png)

3. Wählen Sie als Nächstes im Menü **Extras** die Option **NuGet-Paket-Manager** und dann **NuGet-Pakete für Projektmappe verwalten**. Es ist ein Paket vorhanden, das wir installieren müssen. Wählen Sie die Registerkarte **Durchsuchen** aus, und geben Sie „Azure Cognitive Search“ in das Suchfeld ein. Installieren Sie **Microsoft.Azure.Search**, wenn der Eintrag in der Liste angezeigt wird (Version 9.0.1 oder höher). Klicken Sie sich durch einige zusätzliche Dialogfelder, um die Installation abzuschließen.

    ![Hinzufügen von Azure-Bibliotheken per NuGet](./media/tutorial-csharp-create-first-app/azure-search-nuget-azure.png)

### <a name="initialize-azure-cognitive-search"></a>Initialisieren von Azure Cognitive Search

Für dieses Beispiel verwenden wir öffentlich verfügbare Hoteldaten. Diese Daten umfassen eine willkürliche Sammlung mit 50 fiktiven Hotelnamen und -beschreibungen, die nur den Zweck der Bereitstellung von Demodaten erfüllen. Um auf diese Daten zugreifen zu können, müssen Sie einen Namen und einen Schlüssel dafür angeben.

1. Öffnen Sie die Datei „appsettings.json“ in Ihrem neuen Projekt, und ersetzen Sie die Standardzeilen durch den folgenden Namen und Schlüssel. Der hier angezeigte API-Schlüssel ist kein Beispiel für einen Schlüssel, sondern _genau_ der Schlüssel, den Sie zum Zugreifen auf die Hoteldaten benötigen. Die Datei „appsettings.json“ sollte jetzt wie folgt aussehen.

    ```cs
    {
        "SearchServiceName": "azs-playground",
        "SearchServiceQueryApiKey": "EA4510A6219E14888741FCFC19BFBB82"
    }
    ```

2. Wir sind mit dieser Datei noch nicht fertig. Wählen Sie die Eigenschaften für diese Datei aus, und ändern Sie die Einstellung **In Ausgabeverzeichnis kopieren** in **Kopieren, wenn neuer**.

    ![Kopieren der App-Einstellungen in die Ausgabe](./media/tutorial-csharp-create-first-app/azure-search-copy-if-newer.png)

## <a name="model-data-structures"></a>Modellieren von Datenstrukturen

Modelle (C#-Klassen) werden verwendet, um für Daten die Kommunikation zwischen dem Client (View), dem Server (Controller) und auch der Azure-Cloud über die MVC-Architektur (Model, View, Controller) zu ermöglichen. Normalerweise spiegeln diese Modelle die Struktur der Daten wider, auf die zugegriffen wird. Darüber hinaus benötigen wir ein Modell zum Verarbeiten der View/Controller-Kommunikation.

1. Öffnen Sie im Projektmappen-Explorer den Ordner **Models** Ihres Projekts. Darin ist ein Standardmodell enthalten: **ErrorViewModel.cs**.

2. Klicken Sie mit der rechten Maustaste auf den Ordner **Models**, und wählen Sie **Hinzufügen** und dann **Neues Element**. Wählen Sie anschließend im angezeigten Dialogfeld die Option **ASP.NET Core** und dann die erste Option **Klasse**. Benennen Sie die CS-Datei in „Hotel.cs“ um, und klicken Sie auf **Hinzufügen**. Ersetzen Sie den gesamten Inhalt von „Hotel.cs“ durch den folgenden Code. Beachten Sie die Member **Address** und **Room** der Klasse. Da es sich bei diesen Feldern selbst um Klassen handelt, benötigen wir auch hierfür Modelle.

    ```cs
    using System;
    using Microsoft.Azure.Search;
    using Microsoft.Azure.Search.Models;
    using Microsoft.Spatial;
    using Newtonsoft.Json;

    namespace FirstAzureSearchApp.Models
    {
        public partial class Hotel
        {
            [System.ComponentModel.DataAnnotations.Key]
            [IsFilterable]
            public string HotelId { get; set; }

            [IsSearchable, IsSortable]
            public string HotelName { get; set; }

            [IsSearchable]
            [Analyzer(AnalyzerName.AsString.EnLucene)]
            public string Description { get; set; }

            [IsSearchable]
            [Analyzer(AnalyzerName.AsString.FrLucene)]
            [JsonProperty("Description_fr")]
            public string DescriptionFr { get; set; }

            [IsSearchable, IsFilterable, IsSortable, IsFacetable]
            public string Category { get; set; }

            [IsSearchable, IsFilterable, IsFacetable]
            public string[] Tags { get; set; }

            [IsFilterable, IsSortable, IsFacetable]
            public bool? ParkingIncluded { get; set; }

            [IsFilterable, IsSortable, IsFacetable]
            public DateTimeOffset? LastRenovationDate { get; set; }

            [IsFilterable, IsSortable, IsFacetable]
            public double? Rating { get; set; }

            public Address Address { get; set; }

            [IsFilterable, IsSortable]
            public GeographyPoint Location { get; set; }

            public Room[] Rooms { get; set; }
        }
    }
    ```

3. Führen Sie die gleichen Schritte zum Erstellen eines Modells für die **Address**-Klasse aus, aber geben Sie der Datei den Namen „Address.cs“. Ersetzen Sie den Inhalt durch den folgenden Code.

    ```cs
    using Microsoft.Azure.Search;

    namespace FirstAzureSearchApp.Models
    {
        public partial class Address
        {
            [IsSearchable]
            public string StreetAddress { get; set; }

            [IsSearchable, IsFilterable, IsSortable, IsFacetable]
            public string City { get; set; }

            [IsSearchable, IsFilterable, IsSortable, IsFacetable]
            public string StateProvince { get; set; }

            [IsSearchable, IsFilterable, IsSortable, IsFacetable]
            public string PostalCode { get; set; }

            [IsSearchable, IsFilterable, IsSortable, IsFacetable]
            public string Country { get; set; }
        }
    }
    ```

4. Führen Sie den Prozess noch einmal durch, um die **Room**-Klasse zu erstellen, und geben Sie der Datei den Namen „Room.cs“. Ersetzen Sie den Inhalt wieder durch den folgenden Code.

    ```cs
    using Microsoft.Azure.Search;
    using Microsoft.Azure.Search.Models;
    using Newtonsoft.Json;

    namespace FirstAzureSearchApp.Models
    {
        public partial class Room
        {
            [IsSearchable]
            [Analyzer(AnalyzerName.AsString.EnMicrosoft)]

            public string Description { get; set; }

            [IsSearchable]
            [Analyzer(AnalyzerName.AsString.FrMicrosoft)]
            [JsonProperty("Description_fr")]
            public string DescriptionFr { get; set; }

            [IsSearchable, IsFilterable, IsFacetable]
            public string Type { get; set; }

            [IsFilterable, IsFacetable]
            public double? BaseRate { get; set; }

            [IsSearchable, IsFilterable, IsFacetable]
            public string BedOptions { get; set; }

            [IsFilterable, IsFacetable]

            public int SleepsCount { get; set; }

            [IsFilterable, IsFacetable]
            public bool? SmokingAllowed { get; set; }

            [IsSearchable, IsFilterable, IsFacetable]
            public string[] Tags { get; set; }
        }
    }
    ```

5. Die Klassen **Hotel**, **Address** und **Room** werden in Azure als [_komplexe Typen_](search-howto-complex-data-types.md) bezeichnet. Dies ist ein wichtiges Feature von Azure Cognitive Search. Komplexe Typen können über viele Ebenen mit Klassen und Unterklassen verfügen und ermöglichen die Darstellung von deutlich komplexeren Datenstrukturen, als wenn _einfache Typen_ (Klasse, die nur primitive Member enthält) verwendet werden. Wir benötigen ein weiteres Modell. Führen Sie den Prozess zum Erstellen einer neuen Modellklasse also noch einmal durch, aber rufen Sie dieses Mal die Klasse „SearchData.cs“ auf, und ersetzen Sie den Standardcode durch Folgendes.

    ```cs
    using Microsoft.Azure.Search.Models;

    namespace FirstAzureSearchApp.Models
    {
        public class SearchData
        {
            // The text to search for.
            public string searchText { get; set; }

            // The list of results.
            public DocumentSearchResult<Hotel> resultList;
        }
    }
    ```

    Diese Klasse enthält die Eingabe des Benutzers (**searchText**) und die Ausgabe der Suche (**resultList**). Der Typ der Ausgabe ist kritisch (**DocumentSearchResult&lt;Hotel&gt;** ). Dieser Typ stimmt nämlich genau mit den Ergebnissen der Suche überein, und wir müssen diesen Verweis an die Ansicht (View) übergeben.



## <a name="create-a-web-page"></a>Erstellen einer Webseite

Für das von Ihnen erstellte Projekt wird standardmäßig eine bestimmte Anzahl von Clientansichten erstellt. Die Ansichten richten sich jeweils nach der Version von Core .NET, die Sie verwenden (in diesem Beispiel 2.1). Sie sind alle im Ordner **Views** des Projekts enthalten. Sie müssen nur die Datei „Index.cshtml“ ändern (im Ordner **Views/Home**).

Löschen Sie den gesamten Inhalt von „Index.cshtml“, und erstellen Sie die Datei in den folgenden Schritten neu.

1. Wir verwenden in dieser Ansicht zwei kleine Bilder. Sie können eigene Bilder nutzen oder die Bilder aus dem GitHub-Projekt kopieren: „azure-logo.png“ und „search.png“. Diese beiden Bilder sollten in den Ordner **wwwroot/images** eingefügt werden.

2. In der ersten Zeile der Datei „Index.cshtml“ sollte auf das Modell verwiesen werden, das wir für die Kommunikation der Daten zwischen dem Client (View) und dem Server (Controller) verwenden. Dies ist das von uns erstellte Modell **SearchData**. Fügen Sie diese Zeile der Datei „Index.cshtml“ hinzu.

    ```cs
    @model FirstAzureSearchApp.Models.SearchData
    ```

3. Die Standardmethode besteht darin, einen Titel für die Ansicht einzugeben. Die nächsten Zeilen sollten also wie folgt lauten:

    ```cs
    @{
        ViewData["Title"] = "Home Page";
    }
    ```

4. Geben Sie nach dem Titel einen Verweis auf ein HTML-Stylesheet ein, das wir in Kürze erstellen.

    ```cs
    <head>
        <link rel="stylesheet" href="~/css/hotels.css" />
    </head>
    ```

5. Nun kümmern wir uns um den Hauptteil der Ansicht. Ein wichtiger zu beachtender Punkt ist, dass die Ansicht zwei Situationen verarbeiten können muss. Erstens muss die Anzeige verarbeitet werden, wenn die App zum ersten Mal gestartet wird und der Benutzer noch keinen Suchtext eingegeben hat. Zweitens muss zusätzlich zum Textfeld für die Suche die Anzeige der Ergebnisse verarbeitet werden, damit diese vom Benutzer mehrfach genutzt werden kann. Zur Bewältigung dieser beiden Situationen müssen wir überprüfen, ob das für die Ansicht bereitgestellte Modell NULL ist. Ein NULL-Modell ist ein Hinweis darauf, dass wir uns in der ersten der beiden Situationen befinden (erste Ausführung der App). Fügen Sie der Datei „Index.cshtml“ Folgendes hinzu, und lesen Sie sich die Kommentare durch.

    ```cs
    <body>
    <h1 class="sampleTitle">
        <img src="~/images/azure-logo.png" width="80" />
        Hotels Search
    </h1>

    @using (Html.BeginForm("Index", "Home", FormMethod.Post))
    {
        // Display the search text box, with the search icon to the right of it.
        <div class="searchBoxForm">
            @Html.TextBoxFor(m => m.searchText, new { @class = "searchBox" }) <input class="searchBoxSubmit" type="submit" value="">
        </div>

        @if (Model != null)
        {
            // Show the result count.
            <p class="sampleText">
                @Html.DisplayFor(m => m.resultList.Results.Count) Results
            </p>

            @for (var i = 0; i < Model.resultList.Results.Count; i++)
            {
                // Display the hotel name and description.
                @Html.TextAreaFor(m => Model.resultList.Results[i].Document.HotelName, new { @class = "box1" })
                @Html.TextArea($"desc{i}", Model.resultList.Results[i].Document.Description, new { @class = "box2" })
            }
        }
    }
    </body>
    ```

6. Abschließend fügen wir das Stylesheet hinzu. Wählen Sie in Visual Studio im Menü **Datei** die Option **Neu/Datei** und dann **Stylesheet** (mit Hervorhebung von **Allgemein**). Ersetzen Sie den Standardcode durch Folgendes. Wir gehen auf diese Datei nicht näher ein. Bei den Formaten handelt es sich um HTML-Standardformate.

    ```html
    textarea.box1 {
        width: 648px;
        height: 30px;
        border: none;
        background-color: azure;
        font-size: 14pt;
        color: blue;
        padding-left: 5px;
    }

    textarea.box2 {
        width: 648px;
        height: 100px;
        border: none;
        background-color: azure;
        font-size: 12pt;
        padding-left: 5px;
        margin-bottom: 24px;
    }

    .sampleTitle {
        font: 32px/normal 'Segoe UI Light',Arial,Helvetica,Sans-Serif;
        margin: 20px 0;
        font-size: 32px;
        text-align: left;
    }

    .sampleText {
        font: 16px/bold 'Segoe UI Light',Arial,Helvetica,Sans-Serif;
        margin: 20px 0;
        font-size: 14px;
        text-align: left;
        height: 30px;
    }

    .searchBoxForm {
        width: 648px;
        box-shadow: 0 0 0 1px rgba(0,0,0,.1), 0 2px 4px 0 rgba(0,0,0,.16);
        background-color: #fff;
        display: inline-block;
        border-collapse: collapse;
        border-spacing: 0;
        list-style: none;
        color: #666;
    }

    .searchBox {
        width: 568px;
        font-size: 16px;
        margin: 5px 0 1px 20px;
        padding: 0 10px 0 0;
        border: 0;
        max-height: 30px;
        outline: none;
        box-sizing: content-box;
        height: 35px;
        vertical-align: top;
    }

    .searchBoxSubmit {
        background-color: #fff;
        border-color: #fff;
        background-image: url(/images/search.png);
        background-repeat: no-repeat;
        height: 20px;
        width: 20px;
        text-indent: -99em;
        border-width: 0;
        border-style: solid;
        margin: 10px;
        outline: 0;
    }
    ```

7. Speichern Sie die Stylesheetdatei als „hotels.css“ im Ordner „wwwroot/css“ zusammen mit der Standarddatei „site.css“.

Unsere Ansicht ist nun fertig. Wir kommen gut voran. Die Modelle und Ansichten sind fertig. Jetzt muss nur noch der Controller erstellt werden, um alles zu verbinden.

## <a name="define-methods"></a>Methoden definieren

Wir müssen den Inhalt des einen Controllers (**Home-Controller**) ändern, der standardmäßig erstellt wird.

1. Öffnen Sie die Datei „HomeController.cs“, und ersetzen Sie die **using**-Anweisungen durch Folgendes.

    ```cs
    using System;
    using System.Diagnostics;
    using System.Threading.Tasks;
    using Microsoft.AspNetCore.Mvc;
    using FirstAzureSearchApp.Models;
    using Microsoft.Extensions.Configuration;
    using Microsoft.Azure.Search;
    using Microsoft.Azure.Search.Models;
    ```

### <a name="add-index-methods"></a>Hinzufügen von Index-Methoden

Wir benötigen zwei **Index**-Methoden: eine ohne Parameter (für das erste Öffnen der App), und eine mit einem Modell als Parameter (für den Fall, in dem der Benutzer Suchtext eingegeben hat). Die erste dieser Methoden wird standardmäßig erstellt. 

1. Fügen Sie die folgende Methode nach der **Index()** -Standardmethode hinzu.

    ```cs
        [HttpPost]
        public async Task<ActionResult> Index(SearchData model)
        {
            try
            {
                // Ensure the search string is valid.
                if (model.searchText == null)
                {
                    model.searchText = "";
                }

                // Make the Azure Cognitive Search call.
                await RunQueryAsync(model);
            }

            catch
            {
                return View("Error", new ErrorViewModel { RequestId = "1" });
            }
            return View(model);
        }
    ```

    Achten Sie auf die **async**-Deklaration der Methode und den **await**-Aufruf von **RunQueryAsync**. Diese Schlüsselwörter sorgen dafür, dass unsere Aufrufe asynchron sind, sodass die Blockade von Threads auf dem Server vermieden wird.

    Für den **catch**-Block wird das Fehlermodell verwendet, das für uns standardmäßig erstellt wurde.

### <a name="note-the-error-handling-and-other-default-views-and-methods"></a>Fehlerbehandlung und andere Standardansichten und -methoden

Je nach verwendeter .NET Core-Version wird standardmäßig jeweils eine andere Gruppe von Standardansichten erstellt. Für .NET Core 2.1 lauten die Standardansichten „Index“, „About“, „Contact“, „Privacy“ und „Error“. Für .NET Core 2.2 lauten die Standardansichten beispielsweise „Index“, „Privacy“ und „Error“. In beiden Fällen können Sie diese Standardseiten beim Ausführen der App anzeigen und dann untersuchen, wie sie im Controller verarbeitet werden.

Wir testen die Ansicht „Error“ zu einem späteren Zeitpunkt dieses Tutorials.

Im GitHub-Beispiel haben wir die nicht verwendeten Ansichten und die zugehörigen Aktionen gelöscht.

### <a name="add-the-runqueryasync-method"></a>Hinzufügen der RunQueryAsync-Methode

Der Azure Cognitive Search-Aufruf ist in unserer **RunQueryAsync**-Methode gekapselt.

1. Fügen Sie zuerst einige statische Variablen zum Einrichten des Azure-Diensts und einen Aufruf für deren Initialisierung hinzu.

    ```cs
        private static SearchServiceClient _serviceClient;
        private static ISearchIndexClient _indexClient;
        private static IConfigurationBuilder _builder;
        private static IConfigurationRoot _configuration;

        private void InitSearch()
        {
            // Create a configuration using the appsettings file.
            _builder = new ConfigurationBuilder().AddJsonFile("appsettings.json");
            _configuration = _builder.Build();

            // Pull the values from the appsettings.json file.
            string searchServiceName = _configuration["SearchServiceName"];
            string queryApiKey = _configuration["SearchServiceQueryApiKey"];

            // Create a service and index client.
            _serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(queryApiKey));
            _indexClient = _serviceClient.Indexes.GetClient("hotels");
        }
    ```

2. Fügen Sie nun die eigentliche **RunQueryAsync**-Methode hinzu.

    ```cs
        private async Task<ActionResult> RunQueryAsync(SearchData model)
        {
            InitSearch();

            var parameters = new SearchParameters
            {
                // Enter Hotel property names into this list so only these values will be returned.
                // If Select is empty, all values will be returned, which can be inefficient.
                Select = new[] { "HotelName", "Description" }
            };

            // For efficiency, the search call should be asynchronous, so use SearchAsync rather than Search.
            model.resultList = await _indexClient.Documents.SearchAsync<Hotel>(model.searchText, parameters);

            // Display the results.
            return View("Index", model);
        }
    ```

    Bei dieser Methode stellen wir zuerst sicher, dass unsere Azure-Konfiguration initiiert wurde, und legen dann einige Suchparameter fest. Die Namen der Felder im Parameter **Select** stimmen genau mit den Eigenschaftennamen in der **hotel**-Klasse überein. Es ist auch möglich, den Parameter **Select** wegzulassen. In diesem Fall werden alle Eigenschaften zurückgegeben. Das Weglassen von **Select**-Parametern ist aber ineffizient, wenn wir nur an einer Teilmenge der Daten interessiert sind. Indem wir die für uns interessanten Eigenschaften angeben, werden auch nur diese Eigenschaften zurückgegeben.

    In diesem Tutorial und in der App geht es um den asynchronen Aufruf für die Suche (**model.resultList = await _indexClient.Documents.SearchAsync&lt;Hotel&gt;(model.searchText, parameters);** ). Die **DocumentSearchResult**-Klasse ist interessant. Es ist ratsam, (bei ausgeführter App) hier einen Haltepunkt festzulegen und einen Debugger zu nutzen, um den Inhalt von **model.resultList** zu untersuchen. Dies ist ein intuitiver Vorgang, bei dem Sie außer den gewünschten Daten nicht noch viele weitere Informationen zusätzlich erhalten.

Jetzt kommt der Moment der Wahrheit.

### <a name="test-the-app"></a>Testen der App

Wir überprüfen nun, ob die App richtig ausgeführt wird.

1. Wählen Sie **Debuggen > Ohne Debuggen starten**, oder drücken Sie die F5-TASTE. Wenn Sie die Codierung richtig durchgeführt haben, wird die erste Index-Ansicht angezeigt.

     ![Öffnen der App](./media/tutorial-csharp-create-first-app/azure-search-index.png)

2. Geben Sie Suchtext ein, z. B. „beach“ (oder anderen Text, der Ihnen gerade einfällt), und klicken Sie auf das Suchsymbol. Es sollten einige Ergebnisse angezeigt werden.

     ![Suchen nach „beach“](./media/tutorial-csharp-create-first-app/azure-search-beach.png)

3. Versuchen Sie, „five star“ einzugeben. Sie sehen, dass Sie keine Ergebnisse erhalten. Bei einer anspruchsvolleren Suche würde „five star“ als Synonym für „luxury“ erkannt werden, und die entsprechenden Ergebnisse würden angezeigt werden. Die Verwendung von Synonymen ist in Azure Cognitive Search verfügbar, aber dies wird in den ersten Tutorials nicht behandelt.
 
4. Versuchen Sie, „hot“ als Suchtext einzugeben. Es werden _keine_ Einträge zurückgegeben, die das Wort „hotel“ enthalten. Bei unserer Suche wird nur nach ganzen Wörtern gesucht, aber es werden trotzdem einige Ergebnisse zurückgegeben.

5. Versuchen Sie es mit anderen Wörtern: „pool“, „sunshine“, „view“ usw. Dies ist eine sehr einfache Funktion von Azure Cognitive Search, die aber trotzdem in überzeugender Manier ausgeführt wird.

## <a name="test-edge-conditions-and-errors"></a>Testen von Edgebedingungen und -fehlern

Es ist wichtig zu überprüfen, ob unsere Features für die Fehlerbehandlung wie gewünscht funktionieren, auch wenn gerade keine Fehler zu beobachten sind. 

1. Geben Sie in der **Index**-Methode nach dem **try {** -Aufruf die Zeile **Throw new Exception()** ein. Diese Ausnahme erzwingt einen Fehler, wenn wir nach Text suchen.

2. Führen Sie die App aus, geben Sie „bar“ als Suchtext ein, und klicken Sie auf das Suchsymbol. Die Ausnahme sollte in der Fehleransicht angezeigt werden.

     ![Erzwingen eines Fehlers](./media/tutorial-csharp-create-first-app/azure-search-error.png)

    > [!Important]
    > Es wird als Sicherheitsrisiko angesehen, interne Fehlernummern auf Fehlerseiten zurückzugeben. Falls Ihre App für die allgemeine Nutzung bestimmt ist, sollten Sie sich über die sicheren und bewährten Methoden informieren, die beim Auftreten eines Fehlers befolgt werden sollten.

3. Entfernen Sie **Throw new Exception()** , wenn Sie der Meinung sind, dass die Fehlerbehandlung wie gewünscht durchgeführt wird.

## <a name="takeaways"></a>Wesentliche Punkte

Beachten Sie die folgenden Erkenntnisse aus diesem Projekt:

* Ein Azure Cognitive Search-Aufruf ist kompakt, und es ist einfach, die Ergebnisse zu interpretieren.
* Mit asynchronen Aufrufen wird dem Controller ein geringer Grad an Komplexität hinzugefügt, aber dies ist die bewährte Methode, wenn Sie qualitativ gute Apps entwickeln möchten.
* Mit dieser App wurde eine einfache Textsuche durchgeführt, die gemäß der Einrichtung von **searchParameters** definiert wurde. Diese eine Klasse kann aber mit vielen Membern aufgefüllt werden, die der Erweiterung der Suche dienen. Es ist kein großer Zusatzaufwand erforderlich, um diese App erheblich leistungsstärker zu gestalten.

## <a name="next-steps"></a>Nächste Schritte

Damit wir die höchste Benutzerfreundlichkeit für Azure Cognitive Search erzielen können, müssen wir weitere Features hinzufügen, insbesondere Paginierung (entweder Seitenzahlen oder unendliches Scrollen) und AutoVervollständigen/Vorschläge. Außerdem sollten wir die Verwendung von anspruchsvolleren Suchparametern erwägen (z. B. geografische Suchen nach Hotels innerhalb eines bestimmten Radius um einen bestimmten Punkt und Sortierung von Suchergebnissen).

Diese nächsten Schritte werden im Rahmen einer Tutorialreihe behandelt. Wir beginnen mit der Paginierung.

> [!div class="nextstepaction"]
> [C#-Tutorial: Suchergebnispaginierung – Azure Cognitive Search](tutorial-csharp-paging.md)


