---
title: Herstellen einer Verbindung mit einem Wissensspeicher (Vorschauversion) mit Power BI
titleSuffix: Azure Cognitive Search
description: Stellen Sie mit Power BI eine Verbindung mit einem Azure Cognitive Search-Wissensspeicher (Vorschauversion) her, um Analysen und Untersuchungen durchzuführen.
author: lisaleib
manager: nitinme
ms.author: v-lilei
ms.service: cognitive-search
ms.topic: tutorial
ms.date: 11/04/2019
ms.openlocfilehash: 7b12f0f14003389d36e2df5bcffe7828c135cf2b
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73715493"
---
# <a name="connect-a-knowledge-store-with-power-bi"></a>Herstellen einer Verbindung mit einem Wissensspeicher mit Power BI

> [!IMPORTANT] 
> „Wissensspeicher“ ist zurzeit als öffentliche Vorschauversion verfügbar. Die Vorschaufunktion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Die [REST-API-Version 2019-05-06-Preview](search-api-preview.md) verfügt über Previewfunktionen. Die Portalunterstützung ist momentan eingeschränkt, und das .NET SDK wird nicht unterstützt.

In diesem Artikel erfahren Sie, wie Sie mithilfe von Power Query in der Power BI Desktop-App eine Verbindung mit einem Wissensspeicher herstellen und ihn anschließend erkunden. Sie können mit Vorlagen schneller beginnen oder ein benutzerdefiniertes Dashboard von Grund auf neu erstellen.

+ Führen Sie die Schritte aus, die unter [Erstellen eines Wissensspeichers im Azure-Portal](knowledge-store-create-portal.md) bzw. [Erstellen eines Azure Cognitive Search-Wissensspeichers mithilfe von REST](knowledge-store-create-rest.md) beschrieben sind, um den Beispielwissensspeicher zu erstellen, der in dieser exemplarischen Vorgehensweise verwendet wird. Sie benötigen außerdem den Namen des Azure-Speicherkontos, das Sie zum Erstellen des Wissensspeichers verwendet haben, sowie den Zugriffsschlüssel aus dem Azure-Portal.

+ [Installieren Sie Power BI Desktop](https://powerbi.microsoft.com/downloads/).

## <a name="sample-power-bi-template---azure-portal-only"></a>Power BI-Beispielvorlage (nur Azure-Portal)

Wenn Sie Ihren [Wissensspeicher über das Azure-Portal erstellt haben](knowledge-store-create-portal.md), können Sie das [Beispiel für eine Power BI-Vorlage für Azure Cognitive Search](https://github.com/Azure-Samples/cognitive-search-templates) nutzen, um Power BI-Visualisierungen anzuzeigen und damit zu experimentieren. Sie können diese Vorlage auch herunterladen, während Sie den Assistenten **Daten importieren** durcharbeiten.

Mit der Beispielvorlage werden automatisch die Setupschritte ausgeführt, die im restlichen Teil dieses Artikels beschrieben sind. Falls Sie zum Erstellen Ihres Wissensspeichers aber die REST-API verwendet haben, können Sie die Vorlage überspringen und die Informationen in den verbleibenden Abschnitten dieses Artikels nutzen, um Ihren Wissensspeicher mit Power BI zu verbinden. Beginnen Sie mit [Herstellen einer Verbindung mit Power BI](#connect-with-power-bi).

Die Beispielvorlage enthält mehrere Visualisierungen, z. B. WordCloud und Network Navigator. Einige Visualisierungen in der Vorlage, z. B. die Standortkarte und der Entitätsgraph-Viewer, zeigen keine Daten für den Beispielwissensspeicher an, der unter [Erstellen eines Wissensspeichers im Azure-Portal](knowledge-store-create-portal.md) erstellt wurde. Der Grund ist, dass nur eine Teilmenge der KI-Anreicherungen, die im Assistenten **Daten importieren** verfügbar sind, verwendet wurde.

![Beispiel: Power BI-Vorlage für Azure Cognitive Search](media/knowledge-store-connect-power-bi/powerbi-sample-template-portal-only.png "Beispiel für Power BI-Vorlage")

## <a name="connect-with-power-bi"></a>Herstellen einer Verbindung mit Power BI

1. Starten Sie Power BI Desktop, und klicken Sie auf **Daten abrufen**.

1. Wählen Sie im Fenster **Daten abrufen** die Option **Azure** und anschließend **Azure-Tabellenspeicher** aus.

1. Klicken Sie auf **Verbinden**.

1. Geben Sie unter **Kontoname oder URL** den Namen Ihres Azure Storage-Kontos ein (die vollständige URL wird für Sie erstellt).

1. Wenn Sie dazu aufgefordert werden, geben Sie den Schlüssel des Speicherkontos ein.

1. Wählen Sie die Tabellen *hotelReviewsSsDocument*, *hotelReviewsSsKeyPhrases* und *hotelReviewsSsPages* aus. Diese Tabellen sind Azure-Tabellenprojektionen der Beispieldaten für Hotelrezensionen und enthalten die KI-Anreicherungen, die beim Erstellen des Wissensspeichers ausgewählt wurden.

1. Klicken Sie auf **Laden**.

1. Klicken Sie im oberen Menüband auf **Abfragen bearbeiten**, um den **Power Query-Editor** zu öffnen.

   ![Power Query öffnen](media/knowledge-store-connect-power-bi/powerbi-edit-queries.png "Power Query öffnen")

1. Wählen Sie *hotelReviewsSsDocument* aus, und entfernen Sie anschließend die Spalten *PartitionKey*, *RowKey* und *Timestamp*. 

   ![Tabellen bearbeiten](media/knowledge-store-connect-power-bi/powerbi-edit-table.png "Tabellen bearbeiten")

1. Klicken Sie rechts oben in der Tabelle auf das Symbol mit den entgegengesetzten Pfeilen, um den *Inhalt* zu erweitern. Wenn die Liste mit Spalten angezeigt wird, wählen Sie alle Spalten aus, und heben Sie anschließend die Markierung der Spalten auf, die mit „metadata“ beginnen. Klicken Sie auf **OK**, um die ausgewählten Spalten anzuzeigen.

   ![Tabellen bearbeiten](media/knowledge-store-connect-power-bi/powerbi-expand-content-table.png "Inhalt erweitern")

1. Ändern Sie den Datentyp für die folgenden Spalten, indem Sie oben links in der Spalte auf das Symbol „ABC-123“ klicken.

   + Wählen Sie für *content.latitude* und *Content.longitude* die Option **Dezimalzahl** aus.
   + Wählen Sie für *Content.reviews_date* und *Content.reviews_dateAdded* die Option **Datum/Uhrzeit** aus.

   ![Datentypen ändern](media/knowledge-store-connect-power-bi/powerbi-change-type.png "Datentypen ändern")

1. Wählen Sie *hotelReviewsSsPages* aus, und wiederholen Sie dann die Schritte 9 und 10, um die Spalten zu löschen und den *Inhalt* zu erweitern.
1. Ändern Sie den Datentyp für *Content.SentimentScore* in **Dezimalzahl**.
1. Wählen Sie *hotelReviewsSsKeyPhrases*, und wiederholen Sie die Schritte 9 und 10, um die Spalten zu löschen und den *Inhalt* zu erweitern. Für diese Tabelle sind keine Datentypänderungen erforderlich.

1. Klicken Sie in der Befehlsleiste auf **Close and Apply** (Schließen und Anwenden).

1. Klicken Sie im linken Navigationsbereich auf die Modellkachel, und überprüfen Sie, ob Power BI Beziehungen zwischen allen drei Tabellen anzeigt.

   ![Beziehungen überprüfen](media/knowledge-store-connect-power-bi/powerbi-relationships.png "Beziehungen überprüfen")

1. Doppelklicken Sie auf die einzelnen Beziehungen, und stellen Sie sicher, dass die **Kreuzfilterrichtung** auf **Beide** festgelegt ist.  Dadurch können visuelle Elemente aktualisiert werden, wenn ein Filter angewendet wird.

<!-- ## Try with larger data sets

We purposely kept the data set small to avoid charges for a demo walkthrough. For a more realistic experience, you can create and then attach a billable Cognitive Services resource to enable a larger number of transactions against the sentiment analyzer, keyphrase extraction, and language detector skills.

Create new containers in Azure Blob storage and upload each CSV file to its own container. Specify one of these containers in the data source creation step in Import data wizard.

| Description | Link |
|-------------|------|
| Free tier   | [HotelReviews_Free.csv](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Free.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D) |
| Small (500 Records) | [HotelReviews_Small.csv](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Small.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D) |
| Medium (6000 Records)| [HotelReviews_Medium.csv](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Medium.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D)
| Large (Full dataset 35000 Records) | [HotelReviews_Large.csv](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Large.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D). Be aware that very large data sets are expensive to process. This one costs roughly $1000 U.S dollars.|

In the enrichment step of the wizard, attach a billable [Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) resource, created at the *S0* tier, in the same region as Azure Cognitive Search to use larger data sets. 

  ![Create a Cognitive Services resource](media/knowledge-store-connect-power-bi/create-cognitive-service.png "Create a Cognitive Services resource") -->

## <a name="clean-up"></a>Bereinigen

Wenn Sie in Ihrem eigenen Abonnement arbeiten, sollten Sie sich am Ende eines Projekts überlegen, ob Sie die erstellten Ressourcen noch benötigen. Ressourcen, die weiterhin ausgeführt werden, können Sie Geld kosten. Sie können entweder einzelne Ressourcen oder aber die Ressourcengruppe löschen, um den gesamten Ressourcensatz zu entfernen.

Ressourcen können im Portal über den Link **Alle Ressourcen** oder **Ressourcengruppen** im linken Navigationsbereich gesucht und verwaltet werden.

Denken Sie bei Verwendung eines kostenlosen Diensts an die Beschränkung auf maximal drei Indizes, Indexer und Datenquellen. Sie können einzelne Elemente über das Portal löschen, um unter dem Limit zu bleiben.

## <a name="next-steps"></a>Nächste Schritte

Im folgenden Artikel finden Sie Informationen dazu, wie Sie diesen Wissensspeicher mithilfe von Storage-Explorer untersuchen können.

> [!div class="nextstepaction"]
> [Anzeigen mit Storage-Explorer](knowledge-store-view-storage-explorer.md)

Informationen zum Erstellen eines Wissensspeichers mithilfe der REST-APIs und Postman finden Sie im folgenden Artikel.  

> [!div class="nextstepaction"]
> [Erstellen von Wissensspeichern in REST](knowledge-store-howto.md)
