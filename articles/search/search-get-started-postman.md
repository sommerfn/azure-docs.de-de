---
title: 'Schnellstart: Erstellen, Laden und Abfragen von Indizes mithilfe von Postman und REST-APIs – Azure Search'
description: Erfahren Sie, wie Sie mithilfe von Postman und Beispieldaten und -definitionen die Azure Search-REST-APIs aufrufen.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: quickstart
ms.date: 07/09/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: ee3c4da96629910801413cbbad69963defb87dfe
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67798783"
---
# <a name="quickstart-create-an-azure-search-index-in-postman-using-rest-apis"></a>Schnellstart: Erstellen eines Azure Search-Indexes in Postman mit REST-APIs
> [!div class="op_single_selector"]
> * [Postman](search-get-started-postman.md)
> * [C#](search-create-index-dotnet.md)
> * [Python](search-get-started-python.md)
> * [Portal](search-get-started-portal.md)
> * [PowerShell](search-howto-dotnet-sdk.md)
>*

Eine der einfachsten Möglichkeiten zum Erkunden der [Azure Search-REST-APIs](https://docs.microsoft.com/rest/api/searchservice) besteht darin, mithilfe von Postman oder eines anderen Webtesttools HTTP-Anforderungen zu formulieren und die Antworten zu untersuchen. Mit den richtigen Tools und dieser Anleitung können Sie vor dem Schreiben von Code Anforderungen senden und Antworten anzeigen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie zuerst ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen und dann die [Registrierung für Azure Search](search-create-service-portal.md) durchführen.

## <a name="prerequisites"></a>Voraussetzungen

In diesem Schnellstart werden die folgenden Dienste und Tools verwendet. 

+ [Erstellen Sie einen Azure Search-Dienst](search-create-service-portal.md), oder suchen Sie in Ihrem aktuellen Abonnement [nach einem vorhandenen Dienst](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices). Für diesen Schnellstart können Sie einen kostenlosen Dienst verwenden. 

+ Die [Postman-Desktop-App](https://www.getpostman.com/) wird zum Senden von Anforderungen an Azure Search verwendet.

## <a name="get-a-key-and-url"></a>Abrufen eines Schlüssels und einer URL

Für REST-Aufrufe sind die Dienst-URL und ein Zugriffsschlüssel für jede Anforderung erforderlich. Hierfür wird jeweils ein Suchdienst erstellt. Wenn Sie Azure Search also Ihrem Abonnement hinzugefügt haben, können Sie diese Schritte ausführen, um die erforderlichen Informationen zu erhalten:

1. [Melden Sie sich beim Azure-Portal an](https://portal.azure.com/), und rufen Sie auf der Seite **Übersicht** Ihres Suchdiensts die URL ab. Ein Beispiel für einen Endpunkt ist `https://mydemo.search.windows.net`.

1. Rufen Sie unter **Einstellungen** > **Schlüssel** einen Administratorschlüssel ab, um Vollzugriff auf den Dienst zu erhalten. Es gibt zwei austauschbare Administratorschlüssel – diese wurden zum Zweck der Geschäftskontinuität bereitgestellt, falls Sie einen Rollover für einen Schlüssel durchführen müssen. Für Anforderungen zum Hinzufügen, Ändern und Löschen von Objekten können Sie den primären oder den sekundären Schlüssel verwenden.

![Abrufen eines HTTP-Endpunkts und Zugriffsschlüssels](media/search-get-started-postman/get-url-key.png "Abrufen eines HTTP-Endpunkts und Zugriffsschlüssels")

Für alle an Ihren Dienst gesendeten Anforderungen ist ein API-Schlüssel erforderlich. Ein gültiger Schlüssel stellt anforderungsbasiert eine Vertrauensstellung her zwischen der Anwendung, die die Anforderung versendet, und dem Dienst, der sie verarbeitet.

## <a name="connect-to-azure-search"></a>Herstellen einer Verbindung mit Azure Search

Verwenden Sie in diesem Abschnitt das Webtool Ihrer Wahl, um Verbindungen mit Azure Search einzurichten. Jedes Tool behält Anforderungsheaderinformationen für die Sitzung bei, sodass Sie den API-Schlüssel und den Inhaltstyp nur einmal eingeben müssen.

Bei beiden Tools müssen Sie einen Befehl auswählen (GET, POST, PUT usw.) und einen URL-Endpunkt angeben. Für bestimmte Aufgaben muss im Text der Anforderung auch JSON-Code angegeben werden. Setzen Sie für den Namen des Suchdiensts (YOUR-SEARCH-SERVICE-NAME) einen gültigen Wert ein. 

    https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes?api-version=2019-05-06

Beachten Sie, das HTTPS-Präfix, den Namen des Diensts, den Namen eines Objekts (in diesem Fall: die Indexsammlung) und die [API-Version](search-api-versions.md). Die API-Version ist eine erforderliche Zeichenfolge in Kleinbuchstaben und wird für die aktuelle Version als `?api-version=2019-05-06` angegeben. API-Versionen werden regelmäßig aktualisiert. Wenn Sie die API-Version in jede Anforderung einfügen, haben Sie die vollständige Kontrolle darüber, welche jeweils verwendet wird.  

Der Anforderungsheader setzt sich aus dem Inhaltstyp (content-type) sowie dem API-Schlüssel (api-key) für die Authentifizierung bei Azure Search zusammen. Setzen Sie für den Admin-API-Schlüssel (YOUR-ADMIN-API-KEY) einen gültigen Wert ein. 

    api-key: <YOUR-ADMIN-API-KEY>
    Content-Type: application/json

Formulieren Sie in Postman eine Anforderung, die wie auf dem folgenden Screenshot aussieht. Wählen Sie **GET** als Verb aus, geben Sie die URL an, und klicken Sie auf **Senden**. Dieser Befehl stellt eine Verbindung mit Azure Search her, liest die Indexsammlung und gibt nach erfolgreicher Verbindungsherstellung den HTTP-Statuscode 200 zurück. Falls Ihr Dienst bereits über Indizes verfügt, enthält die Antwort auch Indexdefinitionen.

![Postman-Anforderungsheader][6]

## <a name="1---create-an-index"></a>1\. Erstellen eines Index

In Azure Search wird der Index für gewöhnlich vor dem Laden von Daten erstellt. Für diese Aufgabe wird die [REST-API für die Indexerstellung](https://docs.microsoft.com/rest/api/searchservice/create-index) verwendet. 

Die URL wird um den Indexnamen `hotels` erweitert.

Gehen Sie dazu in Postman wie folgt vor:

1. Ändern Sie das Verb in **PUT**.

2. Fügen Sie diese URL ein: `https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes/hotels?api-version=2019-05-06`.

3. Geben Sie die (weiter unten dargestellte) Indexdefinition im Text der Anforderung an.

4. Klicken Sie auf **Send**.

![Postman-Anforderungstext][8]

### <a name="index-definition"></a>Indexdefinition

Die Feldsammlung definiert die Dokumentstruktur. Jedes Dokument muss über diese Felder verfügen, und jedes Feld muss einen Datentyp besitzen. Da Zeichenfolgenfelder in der Volltextsuche verwendet werden, kann es ratsam sein, numerische Daten in Zeichenfolgen umzuwandeln, wenn dieser Inhalt durchsuchbar sein soll.

Die Attribute im Feld bestimmen die zulässige Aktion. Die REST-APIs ermöglichen standardmäßig viele Aktionen. Beispielsweise sind alle Zeichenfolgen durchsuchbar, abrufbar, filterbar und facettierbar. Häufig müssen Sie Attribute nur festlegen, wenn Sie ein Verhalten deaktivieren möchten.

          {
         "name": "hotels",  
         "fields": [
           {"name": "hotelId", "type": "Edm.String", "key":true, "searchable": false},
           {"name": "baseRate", "type": "Edm.Double"},
           {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
           {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
           {"name": "hotelName", "type": "Edm.String"},
           {"name": "category", "type": "Edm.String"},
           {"name": "tags", "type": "Collection(Edm.String)"},
           {"name": "parkingIncluded", "type": "Edm.Boolean"},
           {"name": "smokingAllowed", "type": "Edm.Boolean"},
           {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
           {"name": "rating", "type": "Edm.Int32"},
           {"name": "location", "type": "Edm.GeographyPoint"}
          ]
         }

Wenn Sie diese Anforderung senden, sollten Sie die Antwort „HTTP 201“ erhalten, mit der angezeigt wird, dass die Erstellung des Index erfolgreich war. Sie können diese Aktion im Portal überprüfen, aber Sie sollten berücksichtigen, dass für die Portalseite bestimmte Aktualisierungsintervalle gelten. Es kann also ein oder zwei Minuten dauern, bis alles angezeigt wird.

> [!TIP]
> Falls Sie eine "HTTP 504"-Antwort erhalten, prüfen Sie, ob die URL mit HTTPS beginnt. Wenn Sie eine "HTTP 400"- oder "HTTP 404"-Antwort erhalten, prüfen Sie den Hauptteil der Anforderung auf Fehler, die durch Kopieren und Einfügen entstanden sind. "HTTP 403" deutet in der Regel auf ein Problem mit dem api-key hin (entweder ein ungültiger Schlüssel oder ein Syntaxproblem bei der Angabe des API-Schlüssels).

## <a name="2---load-documents"></a>2\. Laden von Dokumenten

Das Erstellen und das Auffüllen des Index sind separate Schritte. In Azure Search enthält der Index alle durchsuchbaren Daten, die Sie als JSON-Dokumente bereitstellen können. Für diese Aufgabe wird die [REST-API zum Hinzufügen, Aktualisieren oder Löschen von Dokumenten](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) verwendet. 

Die URL wird um die Sammlungen vom Typ `docs` sowie um den Vorgang `index` erweitert.

Gehen Sie dazu in Postman wie folgt vor:

1. Ändern Sie das Verb in **POST**.

2. Fügen Sie diese URL ein: `https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes/hotels/docs/index?api-version=2019-05-06`.

3. Geben Sie die (weiter unten dargestellten) JSON-Dokumente im Text der Anforderung an.

4. Klicken Sie auf **Send**.

![Postman-Anforderungsnutzlast][10]

### <a name="json-documents-to-load-into-the-index"></a>JSON-Dokumente, die in den Index geladen werden sollen

Der Anforderungstext enthält vier Dokumente, die zum Index "hotels" hinzugefügt werden sollen.

         {
         "value": [
         {
             "@search.action": "upload",
             "hotelId": "1",
             "baseRate": 199.0,
             "description": "Best hotel in town",
             "description_fr": "Meilleur hôtel en ville",
             "hotelName": "Fancy Stay",
             "category": "Luxury",
             "tags": ["pool", "view", "wifi", "concierge"],
             "parkingIncluded": false,
             "smokingAllowed": false,
             "lastRenovationDate": "2010-06-27T00:00:00Z",
             "rating": 5,
             "location": { "type": "Point", "coordinates": [-122.131577, 47.678581] }
           },
           {
             "@search.action": "upload",
             "hotelId": "2",
             "baseRate": 79.99,
             "description": "Cheapest hotel in town",
             "description_fr": "Hôtel le moins cher en ville",
             "hotelName": "Roach Motel",
             "category": "Budget",
             "tags": ["motel", "budget"],
             "parkingIncluded": true,
             "smokingAllowed": true,
             "lastRenovationDate": "1982-04-28T00:00:00Z",
             "rating": 1,
             "location": { "type": "Point", "coordinates": [-122.131577, 49.678581] }
           },
           {
             "@search.action": "upload",
             "hotelId": "3",
             "baseRate": 279.99,
             "description": "Surprisingly expensive",
             "hotelName": "Dew Drop Inn",
             "category": "Bed and Breakfast",
             "tags": ["charming", "quaint"],
             "parkingIncluded": true,
             "smokingAllowed": false,
             "lastRenovationDate": null,
             "rating": 4,
             "location": { "type": "Point", "coordinates": [-122.33207, 47.60621] }
           },
           {
             "@search.action": "upload",
             "hotelId": "4",
             "baseRate": 220.00,
             "description": "This could be the one",
             "hotelName": "A Hotel for Everyone",
             "category": "Basic hotel",
             "tags": ["pool", "wifi"],
             "parkingIncluded": true,
             "smokingAllowed": false,
             "lastRenovationDate": null,
             "rating": 4,
             "location": { "type": "Point", "coordinates": [-122.12151, 47.67399] }
           }
          ]
         }

Innerhalb weniger Sekunden sollten Sie eine "HTTP 200"-Antwort in der Sitzungsliste sehen. Dies bedeutet, dass die Dokumente erfolgreich erstellt wurden. 

Falls Sie eine 207-Antwort erhalten, ist der Upload von mindestens einem Dokument fehlgeschlagen. Wenn Sie eine 404-Antwort erhalten, ist entweder im Header oder im Text der Anforderung ein Syntaxfehler enthalten: Vergewissern Sie sich, dass Sie den Endpunkt so geändert haben, dass er `/docs/index` enthält.

> [!Tip]
> Für ausgewählte Datenquellen können Sie als Alternative auch den *Indexer*-Ansatz verwenden, bei dem die für die Indizierung erforderliche Menge an Code vereinfacht und reduziert wird. Weitere Informationen finden Sie unter [Indexer operations](https://docs.microsoft.com/rest/api/searchservice/indexer-operations) (Indexer-Vorgänge).


## <a name="3---search-an-index"></a>3\. Durchsuchen eines Index

Nachdem Sie einen Index erstellt und Dokumente geladen haben, können Sie nun über die [Search Documents-REST-API](https://docs.microsoft.com/rest/api/searchservice/search-documents) Abfragen ausführen.

Die URL wird um eine Abfragezeichenfolge erweitert, die unter Verwendung des Suchoperators angegeben wird.

Gehen Sie dazu in Postman wie folgt vor:

1. Ändern Sie das Verb in **GET**.

2. Fügen Sie diese URL ein: `https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes/hotels/docs?search=motel&$count=true&api-version=2019-05-06`.

3. Klicken Sie auf **Send**.

Mit dieser Abfrage wird nach dem Wort „motel“ gesucht und die Anzahl von Dokumenten in den Suchergebnissen zurückgegeben. Die Anforderung und die Antwort sollten für Postman in etwa wie im folgenden Screenshot aussehen, nachdem Sie auf **Send** (Senden) geklickt haben. Der Statuscode sollte 200 lauten.

 ![Postman-Abfrageantwort][11]

## <a name="get-index-properties"></a>Abfragen von Indexeigenschaften
Sie können Systeminformationen auch abfragen, um die Anzahl von Dokumenten oder die Speicherauslastung zu erhalten: `https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes/hotels/stats?api-version=2019-05-06`.

In Postman sollte Ihre Anforderung in etwa wie folgt aussehen, und die Antwort enthält dann eine Anzahl von Dokumenten und den belegten Speicher in Byte.

 ![Postman-Systemabfrage][12]

Beachten Sie, dass die Syntax für die API-Version hier anders ist. Verwenden Sie für diese Anforderung `?`, um die API-Version anzufügen. `?` trennt den URL-Pfad von der Abfragezeichenfolge; „&“ trennt die einzelnen Name-Wert-Paare in der Abfragezeichenfolge. Für diese Abfrage ist die API-Version das erste und einzige Element in der Abfragezeichenfolge.

Weitere Informationen zu dieser API finden Sie unter [Get Index Statistics (Azure Search Service REST API)](https://docs.microsoft.com/rest/api/searchservice/get-index-statistics) (Get Index Statistics (Azure Search-REST-API)).

## <a name="clean-up"></a>Bereinigen

Wenn Sie in Ihrem eigenen Abonnement arbeiten, sollten Sie sich am Ende eines Projekts überlegen, ob Sie die erstellten Ressourcen noch benötigen. Ressourcen, die weiterhin ausgeführt werden, können Sie Geld kosten. Sie können entweder einzelne Ressourcen oder aber die Ressourcengruppe löschen, um den gesamten Ressourcensatz zu entfernen.

Ressourcen können im Portal über den Link **Alle Ressourcen** oder **Ressourcengruppen** im linken Navigationsbereich gesucht und verwaltet werden.

Denken Sie bei Verwendung eines kostenlosen Diensts an die Beschränkung auf maximal drei Indizes, Indexer und Datenquellen. Sie können einzelne Elemente über das Portal löschen, um unter dem Limit zu bleiben. 

## <a name="next-steps"></a>Nächste Schritte

REST-Clients sind sehr nützlich für spontane Erkundungen, aber da Sie jetzt mit der Funktionsweise der REST-APIs vertraut sind, können Sie mit dem Code fortfahren. Die nächsten Schritte finden Sie unter den folgenden Links:

+ [Schnellstart: Erstellen eines Index mit dem .NET SDK](search-create-index-dotnet.md)
+ [Schnellstart: Erstellen eines Index (REST) mithilfe von PowerShell](search-create-index-rest-api.md)

<!--Image References-->
[1]: ./media/search-get-started-postman/fiddler-url.png
[2]: ./media/search-get-started-postman/AzureSearch_Fiddler2_PostDocs.png
[3]: ./media/search-get-started-postman/AzureSearch_Fiddler3_Query.png
[4]: ./media/search-get-started-postman/AzureSearch_Fiddler4_QueryResults.png
[5]: ./media/search-get-started-postman/AzureSearch_Fiddler5_QueryStats.png
[6]: ./media/search-get-started-postman/postman-url.png
[7]: ./media/search-get-started-postman/fiddler-request.png
[8]: ./media/search-get-started-postman/postman-request.png
[9]: ./media/search-get-started-postman/fiddler-docs.png
[10]: ./media/search-get-started-postman/postman-docs.png
[11]: ./media/search-get-started-postman/postman-query.png
[12]: ./media/search-get-started-postman/postman-system-query.png
