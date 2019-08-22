---
title: Herstellen einer Verbindung mit einem Wissensspeicher mit Power BI – Azure Search
description: Hier erfahren Sie, wie Sie mithilfe des Datenimport-Assistenten im Azure-Portal einen Wissensspeicher erstellen und anschließend eine Verbindung mit Power BI herstellen, um ihn zu analysieren und zu erkunden.
author: heidisteen
services: search
ms.service: search
ms.subservice: cognitive-search
ms.topic: tutorial
ms.date: 07/30/2019
ms.author: heidist
ms.openlocfilehash: 1c7f297092760f2a92f524347a3c1a5e353d0965
ms.sourcegitcommit: 36e9cbd767b3f12d3524fadc2b50b281458122dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2019
ms.locfileid: "69635553"
---
# <a name="create-an-azure-search-knowledge-store-and-connect-using-power-bi"></a>Erstellen eines Azure Search-Wissensspeichers und Herstellen einer Verbindung unter Verwendung von Power BI

> [!Note]
> Der Wissensspeicher befindet sich in der Vorschauphase und sollte nicht in der Produktion verwendet werden. Dieses Feature wird durch die [Azure Search-REST-API-Version 2019-05-06-Preview](search-api-preview.md) bereitgestellt. Das .NET SDK wird derzeit nicht unterstützt.
>

Der Wissensspeicher ist ein Azure Search-Feature, das Ausgaben aus einer KI-Anreicherungspipeline für die spätere Analyse oder eine anderweitige Downstreamverarbeitung speichert. Eine Pipeline mit KI-Anreicherung akzeptiert Bilddateien oder unstrukturierte Textdateien aus einer unterstützten Datenquelle, sendet sie an die Azure Search-Indizierung, wendet KI-Anreicherungen von Cognitive Services (beispielsweise Bildanalyse und Verarbeitung natürlicher Sprache) an, und speichert die Ergebnisse in einem Wissensspeicher in Azure Storage. Über den Wissensspeicher können Sie Tools wie Power BI oder Storage-Explorer anfügen, um die Ergebnisse zu untersuchen.

In diesem Artikel erstellen Sie einen Wissensspeicher über das Portal und stellen anschließend eine Verbindung mithilfe von Power Query in Power BI Desktop her, um ihn zu erkunden. 

In dieser exemplarischen Vorgehensweise werden ein Dataset mit Kundenrezensionen und Bewertungen, eine Stimmungsbewertung von Cognitive Services und anschließend Power Query zum Abfragen Ihrer Dokumente verwendet. Die Daten stammen aus Hotelrezensionen auf Kaggle.com. 

## <a name="prerequisites"></a>Voraussetzungen

+ [Laden Sie die CSV-Datei mit den Hotelrezensionen (HotelReviews_Free.csv) herunter.](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Free.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D) Dieses Dataset enthält Datensätze mit Kundenfeedback zu Hotels.

+ [Power BI Desktop](https://powerbi.microsoft.com/downloads/)

+ [Azure Search](search-create-service-portal.md). In dieser exemplarischen Vorgehensweise können Sie einen kostenlosen Dienst verwenden. 

+ [Azure Storage](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal). Bei dem Konto muss es sich um ein universelles Konto handeln – also entweder um die Standardoption *StorageV2 (allgemein, Version 2)* oder um *Storage (allgemein, Version 1)* . Wählen Sie die gleiche Region aus wie für Azure Search.
 
## <a name="prepare-data"></a>Vorbereiten von Daten 

Laden Sie die CSV-Datei in Azure Blob Storage hoch, damit von einem Azure Search-Indexer darauf zugegriffen werden kann.

1. [Melden Sie sich beim Azure-Portal an](https://portal.azure.com), navigieren Sie zu Ihrem Azure-Speicherkonto, klicken Sie auf **BLOBs** und dann auf **+ Container**.

1. [Erstellen Sie einen Blobcontainer](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) für Beispieldaten: 

   1. Geben Sie dem Container den Namen `hotel-reviews`. 
   
   1. Legen Sie die öffentliche Zugriffsebene auf beliebige gültige Werte fest. Wir haben den Standardwert verwendet.

1. Nachdem der Container erstellt wurde, öffnen Sie ihn, und wählen Sie auf der Befehlsleiste die Option **Hochladen** aus.

1. Navigieren Sie zum Ordner mit der Datei **HotelReviews-Free.csv**, wählen Sie die Datei aus, und klicken Sie anschließend auf **Hochladen**.

   ![Hochladen der CSV-Datei](media/knowledge-store-howto-powerbi/hotel-reviews-blob-container.png "Hochladen der CSV-Datei")

1. Während Sie sich im Azure-Speicher befinden, rufen Sie die Verbindungszeichenfolge und den Containernamen ab.  Sie benötigen beide Zeichenfolgen, wenn Sie ein Datenquellenobjekt erstellen:

   1. Klicken Sie auf der Übersichtsseite auf **Zugriffsschlüssel**, und kopieren Sie eine *Verbindungszeichenfolge*. Sie beginnt mit `DefaultEndpointsProtocol=https;` und endet mit `EndpointSuffix=core.windows.net`. Ihr Kontoname und der Schlüssel befinden sich dazwischen. 

   1. Der Containername muss `hotel-reviews` oder ein beliebiger Name sein, den Sie zugewiesen haben. 

## <a name="create-and-run-ai-enrichments"></a>Erstellen und Ausführen von KI-Anreicherungen

Verwenden Sie den Datenimport-Assistenten, um den Wissensspeicher zu erstellen. In diesem Abschnitt importieren Sie das Dataset, wählen Anreicherungen aus, konfigurieren einen Wissensspeicher und einen Index und führen diesen aus.

1. [Suchen Sie im Azure-Portal nach Ihrem Suchdienst.](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices)

1. Klicken Sie auf der Befehlsleiste auf **Daten importieren**.

1. Erstellen Sie auf der ersten Seite des Assistenten das Datenquellenobjekt.

   - Wählen Sie **Azure Blob Storage** aus.

   - Geben Sie der Datenquelle einen Namen (beispielsweise *hotel-reviews-ds*).

   - Die Daten liegen im CSV-Format vor. Wählen Sie daher für den Analysemodus die Option **Durch Trennzeichen getrennter Text** aus, aktivieren Sie das Kontrollkästchen **Erste Zeile enthält Überschrift**, und achten Sie darauf, dass als Trennzeichen ein Komma angegeben ist.

   - Die Verbindungszeichenfolge für Ihr Azure Storage-Konto finden Sie im Portal unter **Zugriffsschlüssel**.

   - Den Containername finden Sie ebenfalls im Portal (in der Liste mit Ihren Azure Storage-Blobs).

      ![Erstellen eines Datenquellenobjekts](media/knowledge-store-howto-powerbi/hotel-reviews-ds.png "Erstellen eines Datenquellenobjekts")

1. Fügen Sie auf der zweiten Seite des Assistenten Anreicherungen hinzu, und konfigurieren Sie einen Wissensspeicher.

   - Unter **Cognitive Services-Instanz anfügen** können Sie die kostenlose Ressource verwenden, mit der bis zu 20 Transaktionen pro Tag möglich sind. „HotelReviews-Free.csv“ enthält weniger als 20 Datensätze.

   - Geben Sie unter **Anreicherungen hinzufügen** für das Skillset den Namen *hotel-reviews-ss* ein, wählen Sie das Feld *reviews_text* aus, wählen Sie als Granularitätsebene *Seiten (5.000 Zeichenblöcke)* aus, und wählen Sie anschließend die drei folgenden kognitiven Qualifikationen aus: *Schlüsselbegriffe extrahieren*, *Sprache erkennen*, *Stimmung erkennen*.

      ![Erstellen eines Skillsets](media/knowledge-store-howto-powerbi/hotel-reviews-ss.png "Erstellen eines Skillsets")

   - Geben Sie unter **Anreicherungen in einem Wissensspeicher speichern (Vorschau)** die Verbindungszeichenfolge für Ihr universelles Azure Storage-Konto an. Ein Wissensspeicher wird in Azure-Tabellenspeicher erstellt, wenn Sie in diesem Abschnitt die Optionen für ** Azure-Tabellenprojekte auswählen.

      ![Konfigurieren des Wissensspeichers](media/knowledge-store-howto-powerbi/hotel-reviews-ks.png "Konfigurieren des Wissensspeichers")
   
   Klicken Sie auf **Weiter: Zielindex anpassen**, um mit dem nächsten Schritt fortzufahren.

1. Konfigurieren Sie einen Index für optionale Volltextsuchabfragen in Azure Search. Der Schwerpunkt dieser exemplarischen Vorgehensweise liegt zwar auf der Verbindung zwischen Power BI und Azure-Tabellenspeicher, der Datenimport-Assistent **** kann jedoch auch einen Index für die Volltextsuche in Azure Search erstellen. 

   Der Assistent leitet Felder und Datentypen aus Ihrer Datenquelle ab, sodass Sie lediglich die erforderlichen Attribute auswählen müssen, um das gewünschte Verhalten zu erreichen. *Abrufbar* bedeutet beispielsweise, dass der Feldinhalt aus dem Dienst abgerufen werden kann, während *Durchsuchbar* die Volltextsuche für ausgewählte Felder ermöglicht.

   - Geben Sie dem Index einen Namen (beispielsweise *hotel-reviews-idx*).
   - Legen Sie alle Felder auf **Abrufbar** fest.
   - Legen Sie *city*, *name*, *reviews_text*, *language* und *keyphrases* auf **Durchsuchbar** fest.
   - Legen Sie *sentiment*, *language* und *keyphrases* auf **Filterbar** und **Facettenreich** fest. 
   
    Ihr Index sollte in etwa wie in der folgenden Abbildung aussehen:

     ![Konfigurieren eines Index](media/knowledge-store-howto-powerbi/hotel-reviews-idx.png "Konfigurieren eines Index")

   Klicken Sie auf **Weiter: Indexer erstellen**, um mit dem nächsten Schritt fortzufahren.

1. Konfigurieren Sie einen Indexer mit einem Namen und einem Ausführungszeitplan. Verwenden Sie für diese exemplarische Vorgehensweise den Indexernamen *hotel-reviews-idxr* sowie den Standardzeitplan **Einmalig**, um den Indexer sofort auszuführen.

   Durch die Indexerausführung werden alle vorherigen Konfigurationen angewendet. In diesem Schritt findet die gesamte Extraktion, Verarbeitung und Erfassung statt.

1. Überwachen Sie die Indizierung anhand der Benachrichtigungswarteschlange im Portal. Die Ausführung dauert mehrere Minuten.

## <a name="connect-with-power-bi"></a>Herstellen einer Verbindung mit Power BI

1. Starten Sie Power BI Desktop, und wählen Sie **Daten abrufen** aus.

1. Wählen Sie unter „Daten abrufen“ die Option **Azure** und anschließend **Azure-Tabellenspeicher** aus. Klicken Sie auf **Verbinden**.

1. Fügen Sie unter „Kontoname oder URL“ den Namen Ihres Azure Storage Kontos ein. (Die vollständige URL wird automatisch aufgelöst.)

1. Geben Sie den Kontoschlüssel ein.

1. Wählen Sie „Document“, „KeyPhrases“ und „Pages“ aus. Hierbei handelt es sich um die Tabellen, die durch den Datenimport-Assistenten erstellt werden, wenn Sie die entsprechenden Elemente in der Wissensspeicherkonfiguration auswählen. Klicken Sie auf **Laden**.

1. Klicken Sie zum Öffnen von Power Query auf den Befehl **Abfragen bearbeiten**.

   ![Öffnen von Power Query](media/knowledge-store-howto-powerbi/powerbi-edit-queries.png "Öffnen von Power Query")

1. Gehen Sie für Dokumente wie folgt vor:

   - Entfernen Sie die Spalten „PartitionKey“, „RowKey“ und „Timestamp“, die durch den Azure-Tabellenspeicher erstellt wurden. Die in dieser Analyse verwendeten Beziehungen werden vom Wissensspeicher bereitgestellt.

   - Erweitern Sie die Inhaltsspalte.

     ![Bearbeiten von Tabellen](media/knowledge-store-howto-powerbi/powerbi-edit-table.png "Bearbeiten von Tabellen")

1. Wählen Sie alle Felder aus, und heben Sie anschließend die Auswahl der Felder auf, die mit „metadata“ beginnen.

1. Korrigieren Sie den Datentyp für die folgenden Spalten mithilfe des Symbols „ABC-123“ der jeweiligen Spalte:

   - Datumsspalten müssen den Datentyp **DateTime** besitzen.
   - *Latitude* (Breitengrad) muss den Datentyp **Dezimalzahl** besitzen.
   - *Longitude* (Längengrad) muss den Datentyp **Dezimalzahl** besitzen.

1. Wiederholen Sie die vorherigen Schritte für „KeyPhrases“. Entfernen Sie also „PartitionKey“ und andere Spalten, erweitern Sie die Inhaltsspalten, und legen Sie *SentimentScore* auf **Dezimalzahl** fest.

1. Wiederholen Sie den Vorgang erneut für „Pages“. Entfernen Sie also „PartitionKey“ und andere Spalten, erweitern Sie die Inhaltsspalten. Für diese Tabelle sind keine Datentypänderungen erforderlich.

1. Klicken Sie auf der Power Query-Befehlsleiste ganz links auf **Schließen und übernehmen**.

1. Vergewissern Sie sich, dass Power BI die Beziehungen erkennt, die der Wissensspeicher in Ihren Daten erstellt hat. Klicken Sie im linken Navigationsbereich auf die Beziehungskachel. Alle drei Tabellen sollten miteinander verknüpft werden. Bearbeiten Sie die Beziehungen, und vergewissern Sie sich, dass für „Kreuzfilterrichtung“ beide Richtungen festgelegt sind. Dadurch wird sichergestellt, dass alle visuellen Elemente bei der Anwendung eines Filters aktualisiert werden.

   ![Überprüfen der Beziehungen](media/knowledge-store-howto-powerbi/powerbi-relationships.png "Überprüfen der Beziehungen")

## <a name="try-with-larger-data-sets"></a>Ausprobieren mit großen Datasets

In dieser exemplarischen Vorgehensweise wurde bewusst ein kleines Dataset verwendet, um Kosten zu vermeiden. Für ein realistischeres Szenario können Sie eine kostenpflichtige Cognitive Services-Ressource erstellen und anfügen, um eine größere Anzahl von Transaktionen für die Stimmungsanalyse, die Schlüsselbegriffserkennung und die Spracherkennung zu verwenden.

Erstellen Sie neue Container in Azure Blob Storage, und laden Sie die CSV-Dateien jeweils in ihren eigenen Container hoch. Geben Sie im Datenimport-Assistenten bei der Erstellung der Datenquelle einen dieser Container an.

| BESCHREIBUNG | Link |
|-------------|------|
| Free-Tarif   | [HotelReviews_Free.csv](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Free.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D) |
| Klein (500 Datensätze) | [HotelReviews_Small.csv](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Small.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D) |
| Mittel (6.000 Datensätze)| [HotelReviews_Medium.csv](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Medium.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D)
| Groß (vollständiges Dataset mit 35.000 Datensätzen) | [HotelReviews_Large.csv](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Large.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D). Beachten Sie, dass die Verarbeitung sehr großer Datasets mit hohen Kosten verbunden ist. Bei diesem belaufen sich die Kosten auf etwa 1.000 US-Dollar.|

Fügen Sie im Anreicherungsschritt des Assistenten eine kostenpflichtige Ressource vom Typ [Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) an, die im Tarif *S0* erstellt wurde und sich in der gleichen Region befindet wie Azure Search, um größere Datasets zu verwenden. 

  ![Erstellen einer Cognitive Services-Ressource](media/knowledge-store-howto-powerbi/create-cognitive-service.png "Erstellen einer Cognitive Services-Ressource")

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie diese Übung wiederholen oder eine weitere exemplarische Vorgehensweise für die KI-Anreicherung absolvieren möchten, löschen Sie den hier erstellten Indexer *hotel-reviews-idx*. Durch Löschen des Indexers wird der Zähler für kostenlose Transaktionen pro Tag auf Null zurückgesetzt. 

Weitere ausführliche exemplarische Vorgehensweisen zum Wissensspeicher finden Sie im nächsten Artikel. Dort wird die Erstellung eines Wissensspeichers mit REST-APIs und Postman gezeigt.

> [!div class="nextstepaction"]
> [Erste Schritte mit Knowledge Mining in Azure Search](knowledge-store-howto.md)
