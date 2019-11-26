---
title: C#-Tutorial zur Suchergebnispaginierung
titleSuffix: Azure Cognitive Search
description: In diesem Tutorial wird die Paginierung von Suchergebnissen veranschaulicht. Es basiert auf einem vorhandenen „hotels“-Projekt, bei dem Schaltflächen für „Erste“, „Nächste“, „Vorherige“ und „Letzte“ sowie mit Seitenzahlen verwendet werden. Bei einem zweiten Paginierungssystem wird das unendliche Scrollen verwendet. Dies wird ausgelöst, indem eine vertikale Scrollleiste ganz nach unten verschoben wird.
manager: nitinme
author: PeterTurcan
ms.author: v-pettur
ms.service: cognitive-search
ms.topic: tutorial
ms.date: 11/04/2019
ms.openlocfilehash: 04f8229a86fbd8fbd5404997926412e760e74973
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74113768"
---
# <a name="c-tutorial-search-results-pagination---azure-cognitive-search"></a>C#-Tutorial: Suchergebnispaginierung – Azure Cognitive Search

Es wird beschrieben, wie Sie zwei unterschiedliche Paginierungssysteme implementieren, wobei das erste auf Seitenzahlen und das zweite auf dem unendlichen Scrollen basiert. Beide Paginierungssysteme werden häufig genutzt. Die jeweilige richtige Wahl hängt von der Benutzeroberfläche ab, die Sie für Ihre Ergebnisse verwenden möchten. In diesem Tutorial werden die Paginierungssysteme in das Projekt integriert, die im Tutorial [C#-Tutorial: Erstellen Ihrer ersten App – Azure Cognitive Search](tutorial-csharp-create-first-app.md) erstellt wurden.

In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Erweitern Ihrer App um die Paginierung mit Seitenzahlen
> * Erweitern Ihrer App um unendliches Scrollen

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

Das Projekt [C#-Tutorial: Erstellen Ihrer ersten App – Azure Cognitive Search](tutorial-csharp-create-first-app.md) muss einsatzbereit sein. Dieses Projekt kann entweder Ihre eigene Version sein, oder Sie installieren es über GitHub: [Erstellen der ersten App](https://github.com/Azure-Samples/azure-search-dotnet-samples).

## <a name="extend-your-app-with-numbered-paging"></a>Erweitern Ihrer App um die Paginierung mit Seitenzahlen

Die Paginierung mit Seitenzahlen ist das Paginierungssystem, das von den meisten großen Internetsuchmaschinen und vielen anderen Suchwebsites eingesetzt wird. Bei der Paginierung mit Seitenzahlen werden normalerweise zusätzlich zu den aktuellen Seitenzahlen die Optionen „Nächste“ und „Vorherige“ verwendet. Unter Umständen sind auch Optionen für „Erste Seite“ und „Letzte Seite“ vorhanden. Mit diesen Optionen erhält der Benutzer eine gute Steuerungsmöglichkeit für die Navigation durch Ergebnisse, die auf mehreren Seiten angeordnet sind.

Wir fügen ein System hinzu, das über Optionen für „Erste“, „Vorherige“, „Nächste“ und „Letzte“ sowie über Seitenzahlen verfügt, die nicht bei 1 beginnen, sondern für die Seiten vor und nach der aktuellen Seite des Benutzers gelten (wenn sich der Benutzer beispielsweise auf Seite 10 befindet, werden also ggf. Schaltflächen für die Seiten 8, 9, 10, 11 und 12 angezeigt).

Das System ist flexibel genug, um die Anzahl von sichtbaren Seitennummern in einer globalen Variablen festlegen zu können.

Das System behandelt die Schaltflächen für die Seitenzahlen ganz links und rechts gesondert. Dies bedeutet, dass hierüber die Änderung des Bereichs mit den Seitenzahlen ausgelöst wird. Wenn beispielsweise die Seitenzahlen 8, 9, 10, 11 und 12 angezeigt werden und der Benutzer auf „8“ klickt, ändert sich der angezeigte Bereich in 6, 7, 8, 9 und 10. Entsprechend erfolgt eine Verschiebung nach rechts, wenn „12“ ausgewählt wird.

### <a name="add-paging-fields-to-the-model"></a>Hinzufügen von Paginierungsfeldern zum Modell

Öffnen Sie die einfache Lösung für die Seitensuche.

1. Öffnen Sie die Modelldatei „SearchData.cs“.

2. Fügen Sie zuerst einige globale Variablen hinzu. In MVC werden globale Variablen in einer eigenen statischen Klasse deklariert. Mit **ResultsPerPage** wird die Anzahl von Ergebnissen pro Seite festgelegt. Mit **MaxPageRange** wird bestimmt, wie viele Seitenzahlen in der Ansicht angezeigt werden. Mit **PageRangeDelta** wird bestimmt, um wie viele Seiten der Seitenbereich nach links oder rechts verschoben werden soll, wenn die ganz links bzw. rechts angezeigte Seitenzahl ausgewählt wird. Normalerweise entspricht letztere Zahl ungefähr der Hälfte von **MaxPageRange**. Fügen Sie den folgenden Code in den Namespace ein.

    ```cs
    public static class GlobalVariables
    {
        public static int ResultsPerPage
        {
            get
            {
                return 3;
            }
        }
        public static int MaxPageRange
        {
            get
            {
                return 5;
            }
        }

        public static int PageRangeDelta
        {
            get
            {
                return 2;
            }
        }
    }
    ```

    >[!Tip]
    >Wenn Sie dieses Projekt auf einem Gerät mit einem kleineren Bildschirm ausführen, z. B. einem Laptop, sollten Sie erwägen, den Wert für **ResultsPerPage** in „2“ zu ändern.

3. Fügen Sie der **SearchData**-Klasse Eigenschaften für die Paginierung hinzu, z. B. nach der **searchText**-Eigenschaft.

    ```cs
        // The current page being displayed.
        public int currentPage { get; set; }

        // The total number of pages of results.
        public int pageCount { get; set; }

        // The left-most page number to display.
        public int leftMostPage { get; set; }

        // The number of page numbers to display - which can be less than MaxPageRange towards the end of the results.
        public int pageRange { get; set; }

        // Used when page numbers, or next or prev buttons, have been selected.
        public string paging { get; set; }
    ```

### <a name="add-a-table-of-paging-options-to-the-view"></a>Hinzufügen einer Tabelle mit Paginierungsoptionen zur Ansicht

1. Öffnen Sie die Datei „index.cshtml“, und fügen Sie den folgenden Code direkt vor dem schließenden &lt;/body&gt;-Tag hinzu. Dieser neue Code steht für eine Tabelle mit Paginierungsoptionen der folgenden Art: Erste, Vorherige, 1, 2, 3, 4, 5, Nächste, Letzte.

    ```cs
    @if (Model != null && Model.pageCount > 1)
    {
    // If there is more than one page of results, show the paging buttons.
    <table>
        <tr>
            <td>
                @if (Model.currentPage > 0)
                {
                    <p class="pageButton">
                        @Html.ActionLink("|<", "Page", "Home", new { paging = "0" }, null)
                    </p>
                }
                else
                {
                    <p class="pageButtonDisabled">|&lt;</p>
                }
            </td>

            <td>
                @if (Model.currentPage > 0)
                {
                    <p class="pageButton">
                        @Html.ActionLink("<", "Page", "Home", new { paging = "prev" }, null)
                    </p>
                }
                else
                {
                    <p class="pageButtonDisabled">&lt;</p>
                }
            </td>

            @for (var pn = Model.leftMostPage; pn < Model.leftMostPage + Model.pageRange; pn++)
            {
                <td>
                    @if (Model.currentPage == pn)
                    {
                        // Convert displayed page numbers to 1-based and not 0-based.
                        <p class="pageSelected">@(pn + 1)</p>
                    }
                    else
                    {
                        <p class="pageButton">
                            @Html.ActionLink((pn + 1).ToString(), "Page", "Home", new { paging = @pn }, null)
                        </p>
                    }
                </td>
            }

            <td>
                @if (Model.currentPage < Model.pageCount - 1)
                {
                    <p class="pageButton">
                        @Html.ActionLink(">", "Page", "Home", new { paging = "next" }, null)
                    </p>
                }
                else
                {
                    <p class="pageButtonDisabled">&gt;</p>
                }
            </td>

            <td>
                @if (Model.currentPage < Model.pageCount - 1)
                {
                    <p class="pageButton">
                        @Html.ActionLink(">|", "Page", "Home", new { paging = Model.pageCount - 1 }, null)
                    </p>
                }
                else
                {
                    <p class="pageButtonDisabled">&gt;|</p>
                }
            </td>
        </tr>
    </table>
    }
    ```

    Wir verwenden eine HTML-Tabelle, um alles richtig auszurichten. Die gesamten Aktionen beruhen aber auf den @Html.ActionLink-Anweisungen. Diese rufen jeweils den Controller mit einem **neuen** Modell auf, das mit anderen Einträgen als für die zuvor hinzugefügte **paging**-Eigenschaft erstellt wurde.

    Die Optionen für die erste und letzte Seite senden keine Zeichenfolgen wie „first“ und „last“, sondern stattdessen die richtigen Seitennummern.

2. Fügen Sie der Liste mit den HTML-Formaten in der Datei „hotels.css“ einige Paginierungsklassen hinzu. Die **pageSelected**-Klasse dient zum Identifizieren der Seite, die vom Benutzer gerade angezeigt wird, in der Liste mit den Seitenzahlen (indem die Zahl fett hervorgehoben wird).

    ```html
        .pageButton {
            border: none;
            color: darkblue;
            font-weight: normal;
            width: 50px;
        }

        .pageSelected {
            border: none;
            color: black;
            font-weight: bold;
            width: 50px;
        }

        .pageButtonDisabled {
            border: none;
            color: lightgray;
            font-weight: bold;
            width: 50px;
        }
    ```

### <a name="add-a-page-action-to-the-controller"></a>Hinzufügen einer Page-Aktion zum Controller

1. Öffnen Sie die Datei „HomeController.cs“, und fügen Sie die **Page**-Aktion hinzu. Diese Aktion reagiert auf alle ausgewählten Seitenoptionen.

    ```cs
        public async Task<ActionResult> Page(SearchData model)
        {
            try
            {
                int page;

                switch (model.paging)
                {
                    case "prev":
                        page = (int)TempData["page"] - 1;
                        break;

                    case "next":
                        page = (int)TempData["page"] + 1;
                        break;

                    default:
                        page = int.Parse(model.paging);
                        break;
                }

                // Recover the leftMostPage.
                int leftMostPage = (int)TempData["leftMostPage"];

                // Recover the search text and search for the data for the new page.
                model.searchText = TempData["searchfor"].ToString();

                await RunQueryAsync(model, page, leftMostPage);

                // Ensure Temp data is stored for next call, as TempData only stored for one call.
                TempData["page"] = (object)page;
                TempData["searchfor"] = model.searchText;
                TempData["leftMostPage"] = model.leftMostPage;
            }

            catch
            {
                return View("Error", new ErrorViewModel { RequestId = "2" });
            }
            return View("Index", model);
        }
    ```

    Die **RunQueryAsync**-Methode zeigt jetzt aufgrund des dritten Parameters einen Syntaxfehler an. Hierauf gehen wir etwas weiter unten ein.

    > [!Note]
    > Die **TempData**-Aufrufe speichern einen Wert (ein **Objekt**) im temporären Speicher, aber dieser Speicher ist _nur einen Aufruf lang_ beständig. Wenn wir ein Datenelement in den temporären Daten speichern, ist es für den nächsten Aufruf einer Controlleraktion verfügbar, aber dies gilt nicht mehr für den nächsten Aufruf. Aufgrund dieser kurzen Lebensdauer speichern wir den Suchtext und die Paginierungseigenschaften für jeden Aufruf von **Page** erneut im temporären Speicher.

2. Die **Index(model)** -Aktion muss aktualisiert werden, um die temporären Variablen zu speichern und den Parameter für die Seitenzahl ganz links dem Aufruf **RunQueryAsync** hinzuzufügen.

    ```cs
        public async Task<ActionResult> Index(SearchData model)
        {
            try
            {
                // Ensure the search string is valid.
                if (model.searchText == null)
                {
                    model.searchText = "";
                }

                // Make the search call for the first page.
                await RunQueryAsync(model, 0, 0);

                // Ensure temporary data is stored for the next call.
                TempData["page"] = 0;
                TempData["leftMostPage"] = 0;
                TempData["searchfor"] = model.searchText;
            }

            catch
            {
                return View("Error", new ErrorViewModel { RequestId = "1" });
            }
            return View(model);
        }
    ```

3. Die **RunQueryAsync**-Methode muss in erheblichem Maße aktualisiert werden. Wir verwenden die Felder **Skip**, **Top** und **IncludeTotalResultCount** der **SearchParameters**-Klasse, um nur eine Seite mit Ergebnissen anzufordern. Hierbei beginnen wir mit der Einstellung **Skip**. Wir müssen auch die Paginierungsvariablen für unsere Ansicht berechnen. Ersetzen Sie die gesamte Methode durch den folgenden Code.

    ```cs
        private async Task<ActionResult> RunQueryAsync(SearchData model, int page, int leftMostPage)
        {
            InitSearch();

            var parameters = new SearchParameters
            {
                   // Enter Hotel property names into this list so only these values will be returned.
                   // If Select is empty, all values will be returned, which can be inefficient.
                   Select = new[] { "HotelName", "Description" },
                   SearchMode = SearchMode.All,

                   // Skip past results that have already been returned.
                   Skip = page * GlobalVariables.ResultsPerPage,

                   // Take only the next page worth of results.
                   Top = GlobalVariables.ResultsPerPage,

                   // Include the total number of results.
                   IncludeTotalResultCount = true,
               };

            // For efficiency, the search call should be asynchronous, so use SearchAsync rather than Search.
            model.resultList = await _indexClient.Documents.SearchAsync<Hotel>(model.searchText, parameters);

            // This variable communicates the total number of pages to the view.
            model.pageCount = ( (int)model.resultList.Count + GlobalVariables.ResultsPerPage - 1) / GlobalVariables.ResultsPerPage;

            // This variable communicates the page number being displayed to the view.
            model.currentPage = page;

            // Calculate the range of page numbers to display.
            if (page == 0)
            {
                leftMostPage = 0;
            }
            else
               if (page <= leftMostPage)
            {
                // Trigger a switch to a lower page range.
                leftMostPage = Math.Max(page - GlobalVariables.PageRangeDelta, 0);
            }
            else
            if (page >= leftMostPage + GlobalVariables.MaxPageRange - 1)
            {
                // Trigger a switch to a higher page range.
                leftMostPage = Math.Min(page - GlobalVariables.PageRangeDelta, model.pageCount - GlobalVariables.MaxPageRange);
            }
            model.leftMostPage = leftMostPage;

            // Calculate the number of page numbers to display.
            model.pageRange = Math.Min(model.pageCount - leftMostPage, GlobalVariables.MaxPageRange);

            return View("Index", model);
        }
    ```

4. Abschließend müssen wir noch eine kleine Änderung an der Ansicht vornehmen. Die Variable **resultsList.Results.Count** enthält jetzt nicht die Gesamtzahl, sondern die Anzahl von Ergebnissen, die auf einer Seite zurückgegeben werden (in unserem Beispiel sind dies drei). Da wir **IncludeTotalResultCount** auf „true“ festgelegt haben, enthält die Variable **resultsList.Count** jetzt die Gesamtzahl der Ergebnisse. Ermitteln Sie also, wo die Anzahl von Ergebnissen in der Ansicht angezeigt wird, und ändern Sie dies in den folgenden Code.

    ```cs
            // Show the result count.
            <p class="sampleText">
                @Html.DisplayFor(m => m.resultList.Count) Results
            </p>
    ```

    > [!Note]
    > Wird **IncludeTotalResultCount** auf „true“ festgelegt, wirkt sich dies (in der Regel geringfügig) auf die Leistung aus. Der Grund dafür ist, dass dieser Gesamtwert von Azure Cognitive Search berechnet werden muss. Bei komplexen Datasets wird eine Warnung mit dem Hinweis angezeigt, dass der zurückgegebene Wert ein _Näherungswert_ ist. Für unsere Hoteldaten wird aber ein genauer Wert angegeben.

### <a name="compile-and-run-the-app"></a>Kompilieren und Ausführen der App

Wählen Sie jetzt **Ohne Debuggen starten** (oder drücken Sie die F5-TASTE).

1. Suchen Sie nach Text, für den viele Ergebnisse zurückgegeben werden (z. B. „wifi“). Können Sie ohne Probleme durch die Ergebnisse blättern?

    ![Blättern durch Ergebnisse für „pool“ mit Seitenzahlen](./media/tutorial-csharp-create-first-app/azure-search-numbered-paging.png)

2. Versuchen Sie, auf die Seitenzahl ganz links und dann auf die Seitenzahl ganz rechts zu klicken. Werden die Seitenzahlen richtig angepasst, sodass die aktuelle Seite mittig angeordnet ist?

3. Sind die Optionen „Erste“ und „Letzte“ hilfreich? Bei einigen gängigen Websuchen werden diese Optionen genutzt, während dies bei anderen nicht der Fall ist.

4. Wechseln Sie zur letzten Seite mit den Ergebnissen. Die letzte Seite ist die einzige Seite, die ggf. weniger Ergebnisse als unter **ResultsPerPage** angegeben enthält.

    ![Untersuchen der letzten Seite für „wifi“](./media/tutorial-csharp-create-first-app/azure-search-pool-last-page.png)

5. Geben Sie „town“ ein, und klicken Sie auf „Search“ (Suchen). Es werden keine Paginierungsoptionen angezeigt, wenn die Ergebnisse weniger als eine Seite füllen.

    ![Suchen nach „town“](./media/tutorial-csharp-create-first-app/azure-search-town.png)

Speichern Sie das Projekt jetzt, und probieren Sie eine Alternative zu dieser Form der Paginierung aus.

## <a name="extend-your-app-with-infinite-scrolling"></a>Erweitern Ihrer App um unendliches Scrollen

Das unendliche Scrollen wird ausgelöst, wenn ein Benutzer eine vertikale Scrollleiste bis zu den letzten angezeigten Ergebnissen verschiebt. In diesem Fall wird die nächste Seite mit Ergebnissen vom Server abgerufen. Falls keine weiteren Ergebnisse vorhanden sind, wird nichts zurückgegeben, und die vertikale Scrollleiste ändert sich nicht. Wenn weitere Ergebnisse vorhanden sind, werden sie an die aktuelle Seite angefügt. Die Scrollleiste ändert sich, um anzugeben, dass weitere Ergebnisse verfügbar sind.

Der wichtigste Punkt hierbei ist, dass die angezeigte Seite nicht ersetzt wird, sondern dass die neuen Ergebnisse angefügt werden. Ein Benutzer kann immer zurück zu den ersten Ergebnissen der Suche scrollen.

Zum Implementieren des unendlichen Scrollens beginnen wir mit dem Stand des Projekts vor dem Hinzufügen von Scrollelementen für Seitenzahlen. Erstellen Sie also bei Bedarf über GitHub eine weitere Kopie der einfachen Suchseite: [Erstellen der ersten App](https://github.com/Azure-Samples/azure-search-dotnet-samples).

### <a name="add-paging-fields-to-the-model"></a>Hinzufügen von Paginierungsfeldern zum Modell

1. Fügen Sie zuerst der **SearchData**-Klasse eine **paging**-Eigenschaft hinzu (in der Modelldatei „SearchData.cs“).

    ```cs
        // Record if the next page is requested.
        public string paging { get; set; }
    ```

    Diese Variable ist eine Zeichenfolge, die „next“ enthält, wenn die nächste Seite mit Ergebnissen gesendet werden soll, oder „null“ für die erste Seite der Suche.

2. Fügen Sie in derselben Datei und innerhalb des Namespace eine globale Variablenklasse mit einer Eigenschaft hinzu. In MVC werden globale Variablen in einer eigenen statischen Klasse deklariert. Mit **ResultsPerPage** wird die Anzahl von Ergebnissen pro Seite festgelegt. 

    ```cs
    public static class GlobalVariables
    {
        public static int ResultsPerPage
        {
            get
            {
                return 3;
            }
        }
    }
    ```

### <a name="add-a-vertical-scroll-bar-to-the-view"></a>Hinzufügen einer vertikalen Scrollleiste zur Ansicht

1. Suchen Sie in der Datei „index.cshtml“ nach dem Abschnitt, in dem die Ergebnisse angezeigt werden (beginnt mit **@if (Model != null)** ).

2. Ersetzen Sie den Abschnitt durch den unten angegebenen Code. Der neue Abschnitt **&lt;div&gt;** befindet sich in dem Bereich, für den das Scrollen möglich sein soll. Es wird wie folgt sowohl ein **overflow-y**-Attribut als auch ein Aufruf einer **onscroll**-Funktion mit dem Namen „scrolled()“ hinzugefügt.

    ```cs
        @if (Model != null)
        {
            // Show the result count.
            <p class="sampleText">
                @Html.DisplayFor(m => m.resultList.Count) Results
            </p>

            <div id="myDiv" style="width: 800px; height: 450px; overflow-y: scroll;" onscroll="scrolled()">

                <!-- Show the hotel data. -->
                @for (var i = 0; i < Model.resultList.Results.Count; i++)
                {
                    // Display the hotel name and description.
                    @Html.TextAreaFor(m => Model.resultList.Results[i].Document.HotelName, new { @class = "box1" })
                    @Html.TextArea($"desc{i}", Model.resultList.Results[i].Document.Description, new { @class = "box2" })
                }
            </div>
        }
    ```

3. Fügen Sie direkt unterhalb der Schleife nach dem Tag &lt;/div&gt; die Funktion **scrolled** hinzu.

    ```javascript
        <script>
                function scrolled() {
                    if (myDiv.offsetHeight + myDiv.scrollTop >= myDiv.scrollHeight) {
                        $.getJSON("/Home/Next", function (data) {
                            var div = document.getElementById('myDiv');

                            // Append the returned data to the current list of hotels.
                            for (var i = 0; i < data.length; i += 2) {
                                div.innerHTML += '\n<textarea class="box1">' + data[i] + '</textarea>';
                                div.innerHTML += '\n<textarea class="box2">' + data[i + 1] + '</textarea>';
                            }
                        });
                    }
                }
        </script>
    ```

    Mit der **if**-Anweisung im obigen Skript wird getestet, ob der Benutzer bis zum unteren Ende der vertikalen Scrollleiste gescrollt hat. Wenn ja, wird über den **Home**-Controller eine Aktion mit dem Namen **Next** aufgerufen. Der Controller benötigt keine weiteren Informationen. Er gibt die nächste Seite mit Daten zurück. Diese Daten werden dann wie die Originalseite mit identischen HTML-Formaten formatiert. Wenn keine Ergebnisse zurückgegeben werden, wird nichts angefügt und keine Änderung vorgenommen.

### <a name="handle-the-next-action"></a>Verarbeiten der „Next“-Aktion

Es sind nur drei Aktionen vorhanden, die an den Controller gesendet werden müssen: die erste Ausführung der App, bei der **Index()** aufgerufen wird, die erste Suche des Benutzers, bei der **Index(model)** aufgerufen wird, und dann die nachfolgenden Aufrufe weiterer Ergebnisse per **Next(model)** .

1. Öffnen Sie die Home-Controller-Datei, und löschen Sie die **RunQueryAsync**-Methode aus dem ursprünglichen Tutorial.

2. Ersetzen Sie die **Index(model)** -Aktion durch den folgenden Code. Nun wird das Feld **paging** verarbeitet, wenn „null“ angegeben ist. Bei „next“ wird der Aufruf von Azure Cognitive Search durchgeführt.

    ```cs
        public async Task<ActionResult> Index(SearchData model)
        {
            try
            {
                InitSearch();

                int page;

                if (model.paging != null && model.paging == "next")
                {
                    // Increment the page.
                    page = (int)TempData["page"] + 1;

                    // Recover the search text.
                    model.searchText = TempData["searchfor"].ToString();
                }
                else
                {
                    // First call. Check for valid text input.
                    if (model.searchText == null)
                    {
                        model.searchText = "";
                    }
                    page = 0;
                }

                // Setup the search parameters.
                var parameters = new SearchParameters
                {
                    // Enter Hotel property names into this list so only these values will be returned.
                    // If Select is empty, all values will be returned, which can be inefficient.
                    Select = new[] { "HotelName", "Description" },
                    SearchMode = SearchMode.All,

                    // Skip past results that have already been returned.
                    Skip = page * GlobalVariables.ResultsPerPage,

                    // Take only the next page worth of results.
                    Top = GlobalVariables.ResultsPerPage,

                    // Include the total number of results.
                    IncludeTotalResultCount = true,
                };

                // For efficiency, the search call should be asynchronous, so use SearchAsync rather than Search.
                model.resultList = await _indexClient.Documents.SearchAsync<Hotel>(model.searchText, parameters);

                // Ensure TempData is stored for the next call.
                TempData["page"] = page;
                TempData["searchfor"] = model.searchText;
            }
            catch
            {
                return View("Error", new ErrorViewModel { RequestId = "1" });
            }
            return View("Index", model);
        }
    ```

    Ähnlich wie bei der Methode für die Paginierung mit Seitenzahlen verwenden wir die Sucheinstellungen**Skip** und **Top**, um nur die Rückgabe der benötigten Daten anzufordern.

3. Fügen Sie die **Next**-Aktion dem Home-Controller hinzu. Beachten Sie, wie eine Liste zurückgegeben wird und jedes Hotel der Liste zwei Elemente hinzufügt: einen Hotelnamen und eine Hotelbeschreibung. Dieses Format ist an die Verwendung der zurückgegebenen Daten in der Ansicht durch die Funktion **scrolled** angepasst.

    ```cs
        public async Task<ActionResult> Next(SearchData model)
        {
            // Set the next page setting, and call the Index(model) action.
            model.paging = "next";
            await Index(model);

            // Create an empty list.
            var nextHotels = new List<string>();

            // Add a hotel name, then description, to the list.
            for (int n = 0; n < model.resultList.Results.Count; n++)
            {
                nextHotels.Add(model.resultList.Results[n].Document.HotelName);
                nextHotels.Add(model.resultList.Results[n].Document.Description);
            }

            // Rather than return a view, return the list of data.
            return new JsonResult(nextHotels);
        }
    ```

4. Wenn Sie für **List&lt;string&gt;** einen Fehler erhalten, sollten Sie oben in der Controllerdatei die folgende **using**-Anweisung hinzufügen.

    ```cs
    using System.Collections.Generic;
    ```

### <a name="compile-and-run-your-project"></a>Kompilieren und Ausführen Ihres Projekts

Wählen Sie jetzt **Ohne Debuggen starten** (oder drücken Sie die F5-TASTE).

1. Geben Sie einen Suchbegriff ein, für den viele Ergebnisse angezeigt werden (z. B. „pool“), und testen Sie anschließend die vertikale Scrollleiste. Wird eine neue Seite mit Ergebnissen ausgelöst?

    ![Unendliches Scrollen durch Ergebnisse für „pool“](./media/tutorial-csharp-create-first-app/azure-search-infinite-scroll.png)

    > [!Tip]
    > Um sicherzustellen, dass auf der ersten Seite eine Scrollleiste angezeigt wird, muss die erste Seite mit den Ergebnissen etwas höher als der Anzeigebereich sein. In unserem Beispiel hat **.box1** eine Höhe von 30 Pixel und **.box2** eine Höhe von 100 Pixel _und_ einen unteren Rand von 24 Pixel. Für jeden Eintrag werden also 154 Pixel genutzt. Für drei Einträge werden 3 · 154 = 462 Pixel benötigt. Um sicherzustellen, dass eine vertikale Scrollleiste angezeigt wird, muss für den Anzeigebereich eine Höhe festgelegt werden, die kleiner als 462 Pixel ist. Sogar 461 funktioniert schon. Dieses Problem tritt nur auf der ersten Seite auf. Auf den weiteren Seiten wird auf jeden Fall eine Scrollleiste angezeigt. Die folgende Zeile muss aktualisiert werden: **&lt;div id="myDiv" style="width: 800px; height: 450px; overflow-y: scroll;" onscroll="scrolled()"&gt;** .

2. Scrollen Sie nach unten an das Ende der Ergebnisse. Beachten Sie, dass sich alle Informationen jetzt auf nur einer Seite der Ansicht befinden. Sie können bis ganz nach oben scrollen, ohne Serveraufrufe auszulösen.

Bei anspruchsvolleren Systemen für das unendliche Scrollen wird ggf. das Mausrad oder ein ähnlicher Mechanismus verwendet, um das Laden einer neuen Seite mit Ergebnissen auszulösen. Wir gehen in diesen Tutorials nicht weiter auf das unendliche Scrollen ein, aber es ist interessant, weil weitere Mausklicks vermieden werden. Wenn Sie möchten, können Sie sich weitere Optionen hierzu ansehen.

## <a name="takeaways"></a>Wesentliche Punkte

Beachten Sie die folgenden Erkenntnisse aus diesem Projekt:

* Die Paginierung mit Seitenzahlen eignet sich gut für Suchen, bei denen die Reihenfolge der Ergebnisse eher willkürlich ist. Dies bedeutet, dass die Seiten weiter hinten unter Umständen noch Informationen enthalten, die für Ihre Benutzer interessant sind.
* Das unendliche Scrollen ist gut, wenn die Reihenfolge der Ergebnisse besonders wichtig ist. Ein Beispiel hierfür ist der Fall, in dem die Ergebnisse nach dem Zentrum einer bestimmten Stadt als Ziel sortiert sind.
* Die Paginierung mit Seitenzahlen ermöglicht dann eine bessere Navigation. Beispielsweise erinnert sich ein Benutzer ggf. daran, dass Seite 6 ein interessantes Ergebnis enthalten hat. Ein solch genauer Verweis ist beim unendlichen Scrollen nicht möglich.
* Der Vorteil des unendlichen Scrollens ist klar ersichtlich. Benutzer können nach oben und unten scrollen, ohne umständlich auf Seitenzahlen klicken zu müssen.
* Ein wichtiges Feature beim unendlichen Scrollen ist, dass Ergebnisse an eine vorhandene Seite angefügt werden können und diese nicht ersetzen. Dies ist ein effizientes Verfahren.
* Die Daten im temporären Speicher werden nur für die Dauer eines Aufrufs beibehalten. Der Speicher muss für weitere Aufrufe zurückgesetzt werden.


## <a name="next-steps"></a>Nächste Schritte

Die Paginierung ist für Internetsuchen von entscheidender Bedeutung. Nachdem wir die Paginierung ausführlich behandelt haben, verbessern wir im nächsten Schritt die Benutzeroberfläche weiter, indem wir die Textvervollständigung für die Suche hinzufügen.

> [!div class="nextstepaction"]
> [C#-Tutorial: Hinzufügen von AutoVervollständigen und Vorschlägen – Azure Cognitive Search](tutorial-csharp-type-ahead-and-suggestions.md)
