---
title: 'Erstellen eines Azure Search-Indexes im Azure-Portal: Azure Search'
description: Erfahren Sie, wie Sie einen Index für Azure Search erstellen, indem Sie einen im Portal integrierten Index-Designer verwenden.
manager: nitinme
author: heidisteen
services: search
ms.service: search
ms.topic: conceptual
ms.date: 10/02/2019
ms.author: heidist
ms.openlocfilehash: 4abef5a3030643d4c7b91d2911f350190972f1eb
ms.sourcegitcommit: f2d9d5133ec616857fb5adfb223df01ff0c96d0a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/03/2019
ms.locfileid: "71937264"
---
# <a name="create-an-azure-search-index-in-the-portal"></a>Erstellen eines Azure Search-Index im Portal

Azure Search enthält einen im Portal integrierten Index-Designer, der für Prototypen oder die Erstellung eines [Suchindexes](search-what-is-an-index.md), der auf Ihrem Azure Search-Dienst gehostet wird, nützlich ist. Das Tool wird für die Schemakonstruktion verwendet. Beim Speichern der Definition wird ein leerer Index vollständig in Azure Search ausgedrückt. Wie Sie darin durchsuchbare Inhalte eingeben, bleibt Ihnen überlassen.

Der Index-Designer ist nur einer von mehreren Ansätzen bei der Erstellung eines Indexes. Alternativ können Sie einen Index erstellen und laden, indem Sie den [Datenimport-Assistenten](search-get-started-portal.md) verwenden. Der Assistent funktioniert nur mit Indizes, die er selbst erstellt. Programmgesteuert können Sie einen Index mit den [.NET](search-create-index-dotnet.md)- oder [REST](search-create-index-rest-api.md)-APIs erstellen.

## <a name="start-index-designer"></a>Starten des Index-Designers

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und öffnen Sie das Servicedashboard. Sie können auf der Navigationsleiste auf **Alle Dienste** klicken, um nach vorhandenen Suchdiensten des aktuellen Abonnements zu suchen. 

2. Klicken Sie in der Befehlsleiste oben auf der Seite auf den Link **Index hinzufügen**.

   ![Link „Index hinzufügen“ in der Befehlsleiste](media/search-create-index-portal/add-index.png "Link „Index hinzufügen“ in der Befehlsleiste")

3. Benennen Sie den Azure Search-Index. Auf Indexnamen wird in Indizes und Abfragevorgängen verwiesen. Der Indexname wird Teil der Endpunkt-URL, die für Verbindungen mit dem Index und zum Senden von HTTP-Anforderungen in der Azure Search-REST-API verwendet wird.

   * Beginnen Sie mit einem Buchstaben.
   * Verwenden Sie nur Kleinbuchstaben, Ziffern oder Bindestriche („-“).
   * Beschränken Sie den Namen und 60 Zeichen.

## <a name="add-fields"></a>Hinzufügen von Feldern

Zur Indexerstellung gehört eine *Feldersammlung*, die durchsuchbare Daten im Index definiert. Insgesamt gibt die Feldersammlung die Struktur von Dokumenten an, die Sie separat hochladen. Eine Feldersammlung umfasst erforderliche und optionale Felder (mit Name und Typ) mit Indexattributen, die bestimmen, wie das Feld verwendet werden kann.

1. Fügen Sie Felder hinzu, um die hochzuladenden Dokumente vollständig anzugeben, und legen Sie jeweils einen [Datentyp](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) fest. Wenn Dokumente beispielsweise aus *Hotel-ID*, *Hotelname*, *Adresse*, *Stadt* und *Region* bestehen, erstellen Sie jeweils ein entsprechendes Feld im Index. Im [Entwurfsleitfaden im Abschnitt weiter unten](#design) finden Sie hilfreiche Informationen zum Festlegen von Attributen.

1. Wenn eingehende Daten hierarchisch sind, sollte das Schema [komplexe Typen](search-howto-complex-data-types.md) enthalten, um die geschachtelten Strukturen darzustellen. Das integrierte Beispieldataset „Hotels“ veranschaulicht komplexe Typen anhand einer Adresse (enthält mehrere untergeordnete Felder) mit einer 1:1-Beziehung zu den einzelnen Hotels verwenden und einer komplexen Zimmersammlung, wobei jedem Hotel mehrere Zimmer zugeordnet sind. 

1. Geben Sie ein Feld *key* vom Typ „Edm.String“ an. Für jeden Azure Search-Index ist ein Schlüsselfeld erforderlich, und es muss eine Zeichenfolge sein. Werte für dieses Feld müssen jedes Dokument eindeutig identifizieren. Standardmäßig heißt das Feld *id*. Allerdings können Sie es umbenennen, solange die Zeichenfolge den [Benennungsregeln](https://docs.microsoft.com/rest/api/searchservice/Naming-rules) entspricht. Wenn Ihre Sammlung „fields“ beispielsweise das Element *hotel-id* enthält, können Sie es für Ihren Schlüssel auswählen. 

1. Legen Sie Attribute für jedes Feld fest. Der Index-Designer schließt alle Attribute aus, die für den Datentyp ungültig sind, aber es wird nicht vorgeschlagen, was einbezogen werden soll. Lesen Sie die Anleitung im nächsten Abschnitt, um sich damit vertraut zu machen, wofür die Attribute bestimmt sind.

    Die Azure Search-API-Dokumentation enthält Codebeispiele mit einem einfachen *hotels*-Index. Im nachfolgenden Screenshot sehen Sie die Indexdefinition, einschließlich der Analysen für die französische Sprache, die während der Indexdefinition angegeben wurde. Sie können sie zu Übungszwecken im Portal neu erstellen.

    ![Hotel-Demoindex](media/search-create-index-portal/field-definitions.png "Hotel-Demoindex")

1. Klicken Sie auf **Erstellen**, wenn Sie fertig sind, um den Index zu speichern und zu erstellen.

<a name="design"></a>

## <a name="set-attributes"></a>Festlegen von Attributen

Obwohl Sie jederzeit neue Felder hinzufügen können, sind vorhandene Felddefinitionen für die Lebensdauer des Index gesperrt. Aus diesem Grund verwenden Entwickler in der Regel das Portal zum Erstellen einfacher Indizes und zum Testen von Ideen. Oder sie verwenden Seiten des Portals, um eine Einstellung zu suchen. Häufige Iterationen über einen Indexentwurf sind effizienter, wenn Sie einem codebasierten Ansatz folgen, sodass Sie den Index einfach neu erstellen können.

Analysen und Vorschläge werden Feldern zugeordnet, bevor der Index gespeichert wird. Achten Sie darauf, dass Sie Ihrer Indexdefinition bei der Erstellung Sprachanalysen oder Vorschlagsfunktionen hinzufügen.

Zeichenfolgenfelder werden häufig als **Durchsuchbar** und **Abrufbar** gekennzeichnet. Zu den Feldern zum Eingrenzen der Suchergebnisse gehören **Sortierbar**, **Filterbar** und **Facettenreich**.

Feldattribute bestimmen, wie ein Feld verwendet wird, z.B. ob es in der Volltextsuche, in der Facettennavigation, für Sortiervorgänge usw. verwendet wird. In der folgenden Tabelle werden die einzelnen Attribute beschrieben.

|Attribut|BESCHREIBUNG|  
|---------------|-----------------|  
|**Durchsuchbar**|Volltextsuche ist möglich, unterliegt einer lexikalischen Analyse, z.B. Worttrennung, während der Indizierung. Wenn Sie ein durchsuchbares Feld auf einen Wert wie „sunny day“ festlegen, wird es intern in die einzelnen Token „sunny“ und „day“ unterteilt. Weitere Informationen finden Sie unter [Funktionsweise der Volltextsuche](search-lucene-query-architecture.md).|  
|**Filterbar**|Wird in **$filter**-Abfragen angegeben. Filterbare Felder vom Typ `Edm.String` oder `Collection(Edm.String)` werden keiner Worttrennung unterzogen, sodass nur nach exakten Übereinstimmungen gesucht wird. Beispiel: Wenn Sie ein solches Feld „f“ auf „sunny day“ festlegen, werden mit `$filter=f eq 'sunny'` keine Übereinstimmungen gefunden, während Sie mit `$filter=f eq 'sunny day'` Suchergebnisse erhalten. |  
|**Sortierbar**|Standardmäßig sortiert das System Ergebnisse nach Bewertung, aber Sie können die Sortierung auf Grundlage von Feldern in den Dokumenten konfigurieren. Felder vom Typ `Collection(Edm.String)` können nicht **sortierbar** sein. |  
|**Facettenreich**|Wird in der Regel in einer Darstellung der Suchergebnisse verwendet, die eine Trefferanzahl nach Kategorie umfasst (z.B. Hotels in einer bestimmten Stadt). Diese Option kann nicht für Felder vom Typ `Edm.GeographyPoint` verwendet werden. Felder des Typs `Edm.String`, die **filterbar**, **sortierbar** oder **facettenreich** sind, können höchstens 32 KB lang sein. Weitere Details finden Sie unter [Erstellen des Index (REST API)](https://docs.microsoft.com/rest/api/searchservice/create-index).|  
|**key**|Eindeutiger Bezeichner für Dokumente im Index. Es muss genau ein Feld als Schlüsselfeld ausgewählt werden, und es muss vom Typ `Edm.String` sein.|  
|**Abrufbar**|Legt fest, ob das Feld in einem Suchergebnis zurückgegeben werden kann. Dies ist hilfreich, wenn Sie ein Feld (z.B. *Gewinnspanne*) zum Filtern, Sortieren oder Bewerten verwenden möchten, das Feld jedoch für den Endbenutzer nicht sichtbar sein soll. Dieses Attribut muss für `key`-Felder auf `true` gesetzt sein.|  

## <a name="next-steps"></a>Nächste Schritte

Nach dem Erstellen eines Azure Search-Index können Sie mit dem nächsten Schritt fortfahren: [Hochladen durchsuchbarer Daten in den Index](search-what-is-data-import.md).

Alternativ können Sie sich auch [genauer mit Indizes beschäftigen](search-what-is-an-index.md). Zusätzlich zur Feldersammlung gibt ein Index auch Analysen, Vorschläge, Bewertungsprofile und CORS-Einstellungen an. Das Portal bietet Registerkartenseiten zum Definieren der gängigsten Elemente: Felder, Analysetools und Vorschlagsfunktionen. Zum Erstellen oder Ändern anderer Elemente können Sie die REST-API oder das .NET-SDK verwenden.

## <a name="see-also"></a>Weitere Informationen

 [Funktionsweise der Volltextsuche](search-lucene-query-architecture.md)  
 [Suchdienst-REST API](https://docs.microsoft.com/rest/api/searchservice/)[.NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/search?view=azure-dotnet)

