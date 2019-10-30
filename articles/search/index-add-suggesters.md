---
title: Hinzufügen von Eingabevorschlägen für Abfragen zu einem Index
titleSuffix: Azure Cognitive Search
description: Aktivieren Sie Eingabevorschläge für Abfrageaktionen in der kognitiven Azure-Suche, indem Sie Vorschlagsfunktionen erstellen und Anforderungen formulieren, die automatisch Vervollständigungen oder Vorschläge für Abfrageausdrücke abrufen.
manager: nitinme
author: Brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: a312068d5c8c574e7b069263cf37e3b855810e4b
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72790098"
---
# <a name="add-suggesters-to-an-index-for-typeahead-in-azure-cognitive-search"></a>Hinzufügen von Vorschlagsfunktionen zu einem Index für Eingabevorschläge in der kognitiven Azure-Suche

In der kognitiven Azure-Suche basieren die Funktionen „Vorschläge während der Eingabe“ oder Eingabevorschläge auf einem **Vorschlagsfunktionen**-Konstrukt, das Sie einem [Suchindex](search-what-is-an-index.md) hinzufügen. Es handelt sich um eine Liste mit einem oder mehreren Feldern, für die Sie Eingabevorschläge aktivieren möchten.

Eine Vorschlagsfunktion unterstützt zwei Varianten für Eingabevorschläge: *AutoVervollständigen*, wodurch der Begriff oder Ausdruck, den Sie eingeben, vervollständigt wird, und *Vorschläge*, wodurch eine kurze Liste passender Dokumente zurückgegeben wird.  

Der folgende Screenshot aus dem Beispiel [Erstellen Ihrer ersten App in C#](tutorial-csharp-type-ahead-and-suggestions.md) veranschaulicht Eingabevorschläge. AutoVervollständigen erkennt im Voraus, was der Benutzer in das Suchfeld eingeben könnte. Angenommen, der Benutzer gibt „Zwi“ ein, das von „AutoVervollständigen“ mit „lling“ vervollständigt wird, wodurch sich als potenzieller Suchbegriff „Zwilling“ ergibt. Vorschläge werden in der Dropdownliste angezeigt. Für Vorschläge können Sie jeden Bereich eines Dokuments anzeigen, der das Ergebnis am besten beschreibt. In diesem Beispiel werden Hotelnamen als Vorschläge angezeigt. 

![Visueller Vergleich von AutoVervollständigen und vorgeschlagenen Abfragen](./media/index-add-suggesters/hotel-app-suggestions-autocomplete.png "Visueller Vergleich von AutoVervollständigen und vorgeschlagenen Abfragen")

Um dieses Verhalten in der kognitiven Azure-Suche zu implementieren, gibt es eine Index- und eine Abfragekomponente. 

+ Fügen Sie im Index eine Vorschlagsfunktion zu einem Index hinzu. Sie können das Portal, eine [REST-API](https://docs.microsoft.com/rest/api/searchservice/create-index) oder das [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.suggester?view=azure-dotnet) verwenden. Im weiteren Verlauf dieses Artikels geht es um die Erstellung einer Vorschlagsfunktion. 

+ Rufen Sie in der Abfrageanforderung eine der [folgenden APIs](#how-to-use-a-suggester) auf.

Die „Search-as-you-Type“-Unterstützung (Vorschläge bei der Eingabe) wird auf Feldebene aktiviert. Sie können beide Verhaltensweisen für Eingabevorschläge in derselben Suchlösung implementieren, wenn Sie ein ähnliches Erlebnis wie im Screenshot wünschen. Beide Anforderungen zielen auf die *Dokumentensammlung* eines bestimmten Index ab, und die Antworten werden zurückgegeben, nachdem ein Benutzer eine Zeichenfolge aus mindestens 3 Zeichen eingegeben hat.

## <a name="create-a-suggester"></a>Erstellen einer Vorschlagsfunktion

Eine Vorschlagsfunktion verfügt zwar über mehrere Eigenschaften, in erster Linie handelt es sich aber um eine Sammlung von Feldern, für die Sie die Umgebung für Eingabevorschläge aktivieren. In einer Reise-App können Eingabevorschläge beispielsweise für die Suche nach Zielen, Städten und Attraktionen aktiviert sein. Somit würden alle drei Felder in die Feldsammlung aufgenommen.

Um eine Vorschlagsfunktion zu erstellen, fügen Sie sie zu einem Indexschema hinzu. Sie können eine Vorschlagsfunktion in einem Index verwenden (nämlich eine Vorschlagsfunktion in der Sammlung der Vorschlagsfunktionen). 

### <a name="when-to-create-a-suggester"></a>Wann wird eine Vorschlagsfunktion erstellt?

Der beste Zeitpunkt zum Erstellen einer Vorschlagsfunktion ist während der Erstellung der eigentlichen Felddefinition.

Wenn Sie versuchen, eine Vorschlagsfunktion mithilfe bereits vorhandener Felder zu erstellen, wird dies von der API nicht zugelassen. Der Text für die Eingabevorschläge wird während der Indizierung erstellt, wenn partielle Begriffe in Kombinationen aus zwei oder mehr Zeichen zusammen mit ganzen Begriffen tokenisiert werden. Wenn vorhandene Felder bereits tokenisiert sind, müssen Sie den Index neu erstellen, wenn Sie ihn zu einer Vorschlagsfunktion hinzufügen möchten. Weitere Informationen zum Neuerstellen eines Index finden Sie unter [Neuerstellen eines Index für die kognitive Azure-Suche](search-howto-reindex.md).

### <a name="create-using-the-rest-api"></a>Erstellen mithilfe der REST-API

Fügen Sie Vorschlagsfunktionen in der REST-API über [Index erstellen](https://docs.microsoft.com/rest/api/searchservice/create-index) oder [Index aktualisieren](https://docs.microsoft.com/rest/api/searchservice/update-index) hinzu. 

  ```json
  {
    "name": "hotels",
    "fields": [
      . . .
    ],
    "suggesters": [
      {
        "name": "sg",
        "searchMode": "analyzingInfixMatching",
        "sourceFields": ["hotelName", "category"]
      }
    ],
    "scoringProfiles": [
      . . .
    ]
  }
  ```


### <a name="create-using-the-net-sdk"></a>Erstellen mithilfe des .NET SDK

Definieren Sie in C# ein [Suggester-Objekt](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.suggester?view=azure-dotnet). `Suggesters` ist eine Sammlung, aber sie kann nur ein Element enthalten. 

```csharp
private static void CreateHotelsIndex(SearchServiceClient serviceClient)
{
    var definition = new Index()
    {
        Name = "hotels",
        Fields = FieldBuilder.BuildForType<Hotel>(),
        Suggesters = new List<Suggester>() {new Suggester()
            {
                Name = "sg",
                SourceFields = new string[] { "HotelId", "Category" }
            }}
    };

    serviceClient.Indexes.Create(definition);

}
```

### <a name="property-reference"></a>Eigenschaftsverweis

|Eigenschaft      |BESCHREIBUNG      |
|--------------|-----------------|
|`name`        |Der Name der Vorschlagsfunktion.|
|`searchMode`  |Die Strategie, mit der nach möglichen Ausdrücken gesucht wird. Derzeit wird nur der Modus `analyzingInfixMatching` unterstützt. Darin werden Ausdrücke am Anfang oder in der Mitte von Sätzen flexibel verglichen.|
|`sourceFields`|Eine Liste mit einem oder mehreren Feldern, die als Quelle für den Inhalt von Vorschlägen dienen. Felder müssen vom Typ `Edm.String` und `Collection(Edm.String)` sein. Wenn ein Analysetool für das Feld angegeben wird, muss es sich um ein benanntes Analysetool aus [dieser Liste](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.analyzername?view=azure-dotnet) (kein benutzerdefiniertes Analysetool) handeln.<p/>Folgende Vorgehensweise wird empfohlen: Geben Sie nur die Felder an, die sich für eine erwartete und angemessene Antwort eignen, sei es eine vollständige Zeichenfolge in einer Suchleiste oder eine Dropdownliste.<p/>Ein Hotelname ist ein guter Kandidat, weil er präzise ist. Ausführliche Felder wie Beschreibungen und Kommentare sind zu informationsreich. Sich wiederholende Felder wie Kategorien und Tags sind ebenso weniger effektiv. In den Beispielen schließen wir ohnehin „category“ ein, um zu zeigen, dass Sie mehrere Felder einbeziehen können. |

### <a name="analyzer-restrictions-for-sourcefields-in-a-suggester"></a>Einschränkungen des Analysetools für sourceFields in einer Vorschlagsfunktion

Die kognitive Azure-Suche analysiert den Feldinhalt, um das Abfragen einzelner Begriffe zu ermöglichen. Für Vorschlagsfunktionen müssen Präfixe zusätzlich zu vollständigen Begriffen indiziert werden. Hierfür ist eine zusätzliche Analyse der Quellfelder erforderlich. In benutzerdefinierten Analysetoolkonfigurationen können die verschiedenen Tokenizer und Filter kombiniert werden. Häufig kommt es hierbei zu Kombinationen, die das Erstellen der für die Vorschläge benötigten Präfixe unmöglich machen. Aus diesem Grund verhindert die kognitive Azure-Suche, dass Felder mit benutzerdefinierten Analysetools in eine Vorschlagsfunktion eingebunden werden.

> [!NOTE] 
>  Wenn Sie die obige Einschränkung umgehen müssen, verwenden Sie zwei separate Felder für denselben Inhalt. Eines der Felder kann so über Vorschlagsfunktionen verfügen, während das andere mit einer benutzerdefinierten Analysetoolkonfiguration eingerichtet wird.

<a name="how-to-use-a-suggester"></a>

## <a name="use-a-suggester-in-a-query"></a>Verwenden einer Vorschlagsfunktion in einer Abfrage

Nachdem eine Vorschlagsfunktion erstellt wurde, verwenden Sie zum Aufrufen des Features die entsprechende API in Ihrer Abfragelogik. 

+ [Vorschläge-REST-API](https://docs.microsoft.com/rest/api/searchservice/suggestions) 
+ [AutoVervollständigen-REST-API](https://docs.microsoft.com/rest/api/searchservice/autocomplete) 
+ [SuggestWithHttpMessagesAsync-Methode](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations.suggestwithhttpmessagesasync?view=azure-dotnet)
+ [AutocompleteWithHttpMessagesAsync-Methode](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations.autocompletewithhttpmessagesasync?view=azure-dotnet&viewFallbackFrom=azure-dotnet)

Die Verwendung der API wird im folgenden Aufruf der AutoVervollständigen-REST-API veranschaulicht. In diesem Beispiel ergeben sich zwei wichtige Erkenntnisse. Erstens: Wie bei allen Abfragen wird der Vorgang für die Dokumentensammlung eines Indexes ausgeführt. Zweitens: Sie können Abfrageparameter hinzufügen. Viele Abfrageparameter gelten zwar für beide APIs, aber die Liste ist für jede der beiden APIs unterschiedlich.

```http
GET https://[service name].search.windows.net/indexes/[index name]/docs/autocomplete?[query parameters]  
api-key: [admin or query key]
```

Wenn eine Vorschlagsfunktion nicht im Index definiert ist, tritt bei einem Aufruf von „AutoVervollständigen“ oder „Vorschläge“ ein Fehler auf.

## <a name="sample-code"></a>Beispielcode

+ Das Beispiel [Erstellen Ihrer ersten App in C#](tutorial-csharp-type-ahead-and-suggestions.md) veranschaulicht ein Vorschlagsfunktion-Konstrukt, vorgeschlagene Abfragen, AutoVervollständigen und Facettennavigation. Dieses Codebeispiel wird in einem Sandboxdienst für die kognitive Azure-Suche ausgeführt und verwendet einen vorinstallierten Hotelindex, sodass Sie zum Ausführen der Anwendung lediglich F5 drücken müssen. Es ist weder ein Abonnement noch eine Anmeldung erforderlich.

+ [DotNetHowToAutocomplete](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToAutocomplete) ist ein älteres Beispiel, das sowohl C#- als auch Java-Code enthält. Es veranschaulicht ebenfalls ein Vorschlagsfunktion-Konstrukt, vorgeschlagene Abfragen, AutoVervollständigen und Facettennavigation. In diesem Codebeispiel werden die Beispieldaten des gehosteten [NYCJobs](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs) verwendet. 


## <a name="next-steps"></a>Nächste Schritte

Wir empfehlen, das folgende Beispiel anzuschauen, um zu verstehen, wie die Anforderungen formuliert werden.

> [!div class="nextstepaction"]
> [Beispiele für Vorschläge und AutoVervollständigen](search-autocomplete-tutorial.md) 
