---
title: Erstellen eines kognitiven Skillsets im Azure-Portal
titleSuffix: Azure Cognitive Search
description: Verwenden Sie den Datenimport-Assistenten, um einer Indizierungspipeline kognitive Qualifikationen hinzuzufügen. Zu kognitiven Qualifikationen zählen die optische Zeichenerkennung (Optical Character Recognition, OCR) und die Verarbeitung natürlicher Sprache.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: quickstart
ms.date: 11/04/2019
ms.openlocfilehash: 14d2751f55f3df26277b1210b9201eed61409c6b
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72792081"
---
# <a name="quickstart-create-an-azure-cognitive-search-cognitive-skillset-in-the-azure-portal"></a>Schnellstart: Erstellen eines kognitiven Skillsets für Azure Cognitive Search über das Azure-Portal

Ein Skillset ist ein KI-Feature, das Informationen und die Struktur aus umfangreichen, undifferenzierten Text- oder Bilddateien extrahiert und sie indizierbar und für Volltext-Suchabfragen in Azure Cognitive Search durchsuchbar macht. 

In dieser Schnellstartanleitung werden Dienste und Daten in der Azure-Cloud miteinander kombiniert, um ein Skillset zu erstellen. Nach Abschluss der Einrichtung wird im Portal der **Datenimport-Assistent** ausgeführt, um alles miteinander zu verknüpfen. Am Ende verfügen Sie über einen durchsuchbaren Index mit Daten, die mittels KI-Verarbeitung erstellt wurden und im Portal mithilfe des [Suchexplorers](search-explorer.md) abgefragt werden können.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="create-services-and-load-data"></a>Erstellen von Diensten und Laden von Daten

In dieser Schnellstartanleitung werden Azure Cognitive Search, Azure Blob Storage und [Azure Cognitive Services](https://azure.microsoft.com/services/cognitive-services/) (für die KI) verwendet. 

Aufgrund der geringen Workloadgröße wird Cognitive Services im Hintergrund genutzt und bietet eine kostenlose Verarbeitung von bis zu 20 Transaktionen pro Tag für Aufrufe, die von Azure Cognitive Search stammen. Solange Sie die von uns bereitgestellten Beispieldaten verwenden, können Sie das Erstellen oder Anfügen einer Cognitive Services-Ressource überspringen.

1. [Laden Sie die Beispieldaten herunter](https://1drv.ms/f/s!As7Oy81M_gVPa-LCb5lC_3hbS-4), die aus einem kleinen Satz Dateien verschiedener Typen bestehen. Entzippen Sie die Dateien.

1. [Erstellen Sie ein Azure-Speicherkonto](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal), oder [suchen Sie nach einem vorhandenen Konto](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Storage%2storageAccounts/) in Ihrem aktuellen Abonnement. 

   Wählen Sie die gleiche Region aus wie für Azure Cognitive Search. Wählen Sie den Kontotyp „StorageV2 (allgemein, Version 2)“ aus, wenn Sie später auch das Wissensspeicher-Feature (in einer anderen exemplarischen Vorgehensweise) ausprobieren möchten. Andernfalls können Sie einen beliebigen Typ auswählen.

1. Öffnen Sie die Seiten für Blobdienste, und erstellen Sie einen Container. Sie können die standardmäßige öffentliche Zugriffsebene verwenden. 

1. Klicken Sie im Container auf **Hochladen**, um die im ersten Schritt heruntergeladenen Beispieldateien hochzuladen. Ihnen steht ein breites Spektrum an Inhaltstypen zur Verfügung – einschließlich Bild- und Anwendungsdateien, die in ihrem nativen Format nicht für die Volltextsuche geeignet sind.

   ![Quelldateien in Azure Blob Storage](./media/cognitive-search-quickstart-blob/sample-data.png)

1. [Erstellen Sie einen Azure Cognitive Search-Dienst](search-create-service-portal.md), oder [suchen Sie nach einem vorhandenen Dienst](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) im gleichen Abonnement. Für diesen Schnellstart können Sie einen kostenlosen Dienst verwenden.

<!-- 1. You are almost done with this resource, but before you leave these pages, use a link on the left navigation pane to open the **Access Keys** page. In many tutorials, especially those that use the REST API, you will need a connection string to retrieve data from Blob storage. A connection string looks similar to the following example: `DefaultEndpointsProtocol=https;AccountName=<YOUR-ACCOUNT-NAME>;AccountKey=<YOUR-ACCOUNT-KEY>;EndpointSuffix=core.windows.net` -->

Nun können Sie zum Datenimport-Assistenten wechseln.

## <a name="run-the-import-data-wizard"></a>Ausführen des Datenimport-Assistenten

Klicken Sie auf der Übersichtsseite des Suchdiensts auf der Befehlsleiste auf **Daten importieren**, um in vier Schritten die kognitive Anreicherung einzurichten.

  ![Befehl zum Importieren von Daten](media/cognitive-search-quickstart-blob/import-data-cmd2.png)

### <a name="step-1-create-a-data-source"></a>Schritt 1: Erstellen einer Datenquelle

1. Wählen Sie unter **Verbindung mit Ihren Daten herstellen** die Option **Azure Blob Storage** sowie das erstellte Storage-Konto und den erstellten Container aus. Geben Sie der Datenquelle einen Namen, und verwenden Sie für alles andere die Standardwerte. 

   ![Azure-Blobkonfiguration](./media/cognitive-search-quickstart-blob/blob-datasource.png)

1. Wechseln Sie zur nächsten Seite.

### <a name="step-2-add-cognitive-skills"></a>Schritt 2: Hinzufügen von kognitiven Qualifikationen

Fügen Sie als Nächstes kognitive Qualifikationen hinzu, um die Verarbeitung natürlicher Sprache aufzurufen. Die Beispieldaten umfassen 12 Dateien. Das kostenlose Kontingent von 20 Transaktionen für Cognitive Services ist somit für diese Schnellstartanleitung ausreichend. Da wir die OCR nicht verwenden, werden in diesem Prozess nur Dateien gezählt, entschlüsselt und verwendet, bei denen es sich nicht um Bilddateien handelt.

1. In dieser Schnellstartanleitung verwenden wir die Cognitive Services-Ressource vom Typ **Free**.

   ![Cognitive Services-Instanz anfügen](media/cognitive-search-quickstart-blob/cog-search-attach.png)

1. Erweitern Sie **Qualifikationen hinzufügen**, und wählen Sie Qualifikationen mit Verarbeitung natürlicher Sprache aus. Wählen Sie für diesen Schnellstart Entitätserkennung für Personen, Organisationen und Orte aus.

   ![Cognitive Services-Instanz anfügen](media/cognitive-search-quickstart-blob/skillset.png)

1. Übernehmen Sie das Standardquellfeld: `content`. Das sieht vielleicht nach einem kleinen Ziel aus, im Falle von Azure-Blobs enthält das Feld `content` jedoch den Großteil des Blob-Dokuments (beispielsweise ein Word-Dokument oder einen PowerPoint-Foliensatz), was es zu einem guten Kandidaten macht.

1. Wechseln Sie zur nächsten Seite.

> [!NOTE]
> Qualifikationen für die Verarbeitung natürlicher Sprache arbeiten auf Textinhalten im Beispieldatenset. Da die OCR-Option nicht ausgewählt wurde, werden die JPEG- und PNG-Dateien im Beispieldatenset im Rahmen dieses Schnellstarts nicht verarbeitet. 

### <a name="step-3-configure-the-index"></a>Schritt 3: Konfigurieren des Index

In Azure Cognitive Search enthält ein Index Ihre durchsuchbaren Inhalte, und der **Datenimport-Assistent** kann in der Regel die Datenquelle untersuchen und das Schema für Sie erstellen. Überprüfen Sie in diesem Schritt das generierte Schema, und überarbeiten Sie ggf. die Einstellungen. Im Anschluss sehen Sie das für das Blobdataset der Demo erstellte Standardschema.

Für diesen Schnellstart legt der Assistent sinnvolle Standardwerte fest: 

+ Basierend auf dem Datenquellentyp lautet der Standardname *azureblob-index*. 

+ Standardfelder basieren auf dem ursprünglichen Quelldatenfeld (`content`) sowie auf den von den kognitiven Qualifikationen erstellten Ausgabefeldern (`people`, `organizations` und `locations`). Standarddatentypen werden aus Metadaten und Datenstichproben abgeleitet.

+ Der Standarddokumentschlüssel ist *metadata_storage_path* (da dieses Feld eindeutige Werte enthält).

+ Standardattribute sind für diese Felder **abrufbar** und **durchsuchbar**. **Durchsuchbar** gibt an, dass ein Feld durchsucht werden kann. **Abrufbar** bedeutet, dass es in Ergebnissen zurückgegeben werden kann. Der Assistent geht davon aus, dass diese Felder abrufbar und durchsuchbar sein sollen, da Sie sie über eine Qualifikationsgruppe erstellt haben.

  ![Indexfelder](media/cognitive-search-quickstart-blob/index-fields.png)

Beachten Sie, dass das Attribut **Abrufbar** für das Feld `content` durchgestrichen und mit einem Fragezeichen versehen ist. Bei textlastigen Blobdokumenten enthält das Feld `content` den Großteil der Datei, der mehrere tausende Zeilen umfassen kann. Wenn Sie Dateiinhalte an Clientcode übergeben müssen, stellen Sie sicher, dass **Abrufbar** ausgewählt bleibt. Entfernen Sie andernfalls ggf. dieses Attribut für `content`, wenn die extrahierten Elemente (`people`, `organizations` und `locations`) für Ihre Zwecke ausreichend sind.

Die Markierung eines Felds als **abrufbar** bedeutet nicht, dass das Feld in den Suchergebnissen vorhanden sein *muss*. Sie können die Zusammenstellung der Suchergebnisse präzise steuern und mit dem Abfrageparameter **$select** angeben, welche Felder enthalten sein sollen. Bei textlastigen Feldern wie `content` können Sie mit dem Parameter **$select** verwaltbare Suchergebnisse für die menschlichen Benutzer Ihrer Anwendung bereitstellen und gleichzeitig sicherstellen, dass Clientcode über das Attribut **Abrufbar** Zugriff auf alle erforderlichen Informationen hat.
  
Wechseln Sie zur nächsten Seite.

### <a name="step-4-configure-the-indexer"></a>Schritt 4: Konfigurieren des Indexers

Der Indexer ist eine allgemeine Ressource, die den Indizierungsvorgang antreibt. Er gibt den Datenquellennamen, einen Zielindex und die Häufigkeit der Ausführung an. Der **Datenimport-Assistent** erstellt mehrere Objekte, und eines davon ist immer ein Indexer, den Sie wiederholt ausführen können.

1. Auf der Seite **Indexer** können Sie den Standardnamen übernehmen und auf die Zeitplanoption **Einmal** klicken, um ihn sofort auszuführen. 

   ![Indexerdefinition](media/cognitive-search-quickstart-blob/indexer-def.png)

1. Klicken Sie auf **Senden**, um den Indexer zu erstellen und gleichzeitig auszuführen.

## <a name="monitor-status"></a>Überwachen des Status

Die Indizierung kognitiver Qualifikationen dauert länger als die übliche textbasierte Indizierung. Navigieren Sie zum Überwachen des Fortschritts zur Übersichtsseite, und klicken Sie in der Mitte der Seite auf **Indexer**.

Die Warnung wird angezeigt, da die Datenquelle JPG- und PNG-Bilddateien enthält und wir bei dieser Pipeline die OCR-Qualifikation weggelassen haben. Sie finden auch Benachrichtigungen über Kürzungen. Die Extraktion ist im Free-Tarif auf 32.000 Zeichen beschränkt.

  ![Azure Cognitive Search-Benachrichtigung](./media/cognitive-search-quickstart-blob/indexer-notification.png)

Indizierung und Anreicherung können zeitaufwändig sein, was der Grund dafür ist, dass wir fürs erste Kennenlernen kleinere Datensets empfehlen. 

Im Azure-Portal können Sie auch das Benachrichtigungsaktivitätsprotokoll auf einen klickbaren Statuslink für eine **Azure Cognitive Search-Benachrichtigung** überprüfen. Die Ausführung kann mehrere Minuten dauern.

## <a name="query-in-search-explorer"></a>Abfragen im Suchexplorer

Nach der Erstellung eines Index können Sie Abfragen senden, um Dokumente aus dem Index zurückzugeben. Verwenden Sie im Portal den **Suchexplorer**, um Abfragen auszuführen und Ergebnisse anzuzeigen. 

1. Klicken Sie auf der Dashboardseite des Suchdiensts auf der Befehlsleiste auf **Suchexplorer**.

1. Wählen Sie oben **Index ändern** aus, um den von Ihnen erstellten Index auszuwählen.

1. Geben Sie eine Suchzeichenfolge ein, um den Index abzufragen, z.B. `search=Microsoft&searchFields=Organizations`.

Die Ergebnisse werden in JSON zurückgegeben, was sehr ausführlich und schwierig zu lesen sein kann, insbesondere bei langen Dokumenten, die aus Azure-Blobs stammen. Wenn Sie die Ergebnisse nicht komfortabel durchsuchen können, verwenden Sie STRG-F, um in Dokumenten zu suchen. Bei dieser Abfrage können Sie im JSON-Code nach bestimmten Begriffen suchen. 

STRG-F kann Ihnen darüber hinaus helfen, zu ermitteln, wie viele Dokumente sich in einem bestimmten Resultset befinden. Für Azure-Blobs wählt das Portal „metadata_storage_path“ als Schlüssel aus, das jeder Wert für das Dokument eindeutig ist. Suchen Sie mithilfe von STRG-F nach „metadata_storage_path“, um die Anzahl der Dokumente zu erhalten. 

  ![Suchexplorer-Beispiel](./media/cognitive-search-quickstart-blob/search-explorer.png)

## <a name="takeaways"></a>Wesentliche Punkte

Sie haben nun Ihr erstes Skillset erstellt und sich mit wichtigen Konzepten vertraut gemacht, die für die Erstellung von Prototypen für eine angereicherte Suchlösung mit Ihren eigenen Daten hilfreich sind.

Einige wichtige Konzepte, von denen wir hoffen, dass Sie sie verinnerlicht haben, schließen die Abhängigkeit von Azure-Datenquellen ein. Ein Skillset ist an einen Indexer gebunden, und Indexer sind Azure- und quellenspezifisch. Dieser Schnellstart verwendet Azure Blob Storage, jedoch sind auch andere Azure-Datenquellen möglich. Weitere Informationen finden Sie unter [Indexer in Azure Cognitive Search](search-indexer-overview.md).

Ein anderes wichtiges Konzept ist, dass Qualifikationen auf Eingabefeldern arbeiten. Im Portal müssen Sie ein einzelnes Quellfeld für alle Qualifikationen auswählen. Im Code können andere Felder oder die Ausgabe einer Upstreamqualifikation als Eingaben dienen.

Die Ausgabe wird an einen Suchindex weitergeleitet, und es gibt eine Zuordnung zwischen Name-Wert-Paaren, die im Zuge der Indizierung erstellt wurden, und einzelnen Feldern in Ihrem Index. Intern richtet das Portal [Anmerkungen](cognitive-search-concept-annotations-syntax.md) ein und definiert eine [Qualifikationsgruppe](cognitive-search-defining-skillset.md), um die Reihenfolge der Vorgänge und den allgemeinen Ablauf festzulegen. Diese Schritte sind im Portal ausgeblendet, werden aber wichtig, wenn Sie selbst mit der Erstellung von Code beginnen.

Außerdem haben Sie gelernt, dass Sie Inhalte durch Abfragen des Index überprüfen können. Azure Cognitive Search stellt letztendlich einen durchsuchbaren Index bereit, den Sie entweder mit der [einfachen](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) oder mit der [vollständig erweiterten Abfragesyntax](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) abfragen können. Ein Index, der angereicherte Felder enthält, ist wie jeder andere. Wenn Sie standardmäßige oder [benutzerdefinierte Analysetools](search-analyzers.md), [Bewertungsprofile](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index), [Synonyme](search-synonyms.md), [facettierte Filter](search-filters-facets.md), die geografische Suche oder andere Azure Cognitive Search-Features einbeziehen möchten, stehen Ihnen alle Wege offen.

## <a name="clean-up"></a>Bereinigen

Wenn Sie in Ihrem eigenen Abonnement arbeiten, sollten Sie sich am Ende eines Projekts überlegen, ob Sie die erstellten Ressourcen noch benötigen. Ressourcen, die weiterhin ausgeführt werden, können Sie Geld kosten. Sie können entweder einzelne Ressourcen oder aber die Ressourcengruppe löschen, um den gesamten Ressourcensatz zu entfernen.

Ressourcen können im Portal über den Link **Alle Ressourcen** oder **Ressourcengruppen** im linken Navigationsbereich gesucht und verwaltet werden.

Denken Sie bei Verwendung eines kostenlosen Diensts an die Beschränkung auf maximal drei Indizes, Indexer und Datenquellen. Sie können einzelne Elemente über das Portal löschen, um unter dem Limit zu bleiben. 

## <a name="next-steps"></a>Nächste Schritte

Skillsets können über das Portal, per .NET SDK oder per REST-API erstellt werden. Probieren Sie bei Interesse die REST-API mit Postman und weiteren Beispieldaten aus.

> [!div class="nextstepaction"]
> [Tutorial: Strukturieren unstrukturierter Inhalte mit KI-Anreicherung](cognitive-search-tutorial-blob.md)

> [!Tip]
> Wenn Sie diese Übung wiederholen oder eine andere exemplarische Vorgehensweise für die KI-Anreicherung ausprobieren möchten, löschen Sie den Indexer im Portal. Durch Löschen des Indexers wird der Zähler für kostenlose Transaktionen pro Tag für die Cognitive Services-Verarbeitung auf Null zurückgesetzt.