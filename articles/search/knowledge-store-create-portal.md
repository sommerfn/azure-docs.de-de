---
title: Erstellen eines Wissensspeichers im Azure-Portal
titleSuffix: Azure Cognitive Search
description: Verwenden Sie den Datenimport-Assistenten, um einen Wissensspeicher zur Speicherung angereicherter Inhalte zu erstellen. Stellen Sie von anderen Apps aus eine Verbindung mit einem Wissensspeicher her, um Analysen durchzuführen, oder senden Sie angereicherte Inhalte an Downstreamprozesse.
author: lisaleib
manager: nitinme
ms.author: v-lilei
ms.service: cognitive-search
ms.topic: quickstart
ms.date: 11/04/2019
ms.openlocfilehash: d714e913d5e03233ed3ffcaaebca6eb989a56bd7
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72790035"
---
# <a name="quickstart-create-an-azure-cognitive-search-knowledge-store-in-the-azure-portal"></a>Schnellstart: Erstellen eines Azure Cognitive Search-Wissensspeichers im Azure-Portal

> [!Note]
> Der Wissensspeicher befindet sich in der Vorschauphase und sollte nicht in der Produktion verwendet werden. Dieses Feature steht sowohl im Azure-Portal als auch in der [Version „2019-05-06-Preview“ der REST-API für den Azure Search-Dienst](search-api-preview.md) zur Verfügung. Das .NET SDK wird derzeit nicht unterstützt.
>

Der Wissensspeicher ist ein Azure Cognitive Search-Feature, das Ausgaben aus einer Pipeline für kognitive Qualifikationen zur späteren Analyse oder für die Downstreamverarbeitung speichert. 

Eine Pipeline akzeptiert Bilder und unstrukturierten Text als Rohinhalt, wendet über Cognitive Services künstliche Intelligenz (etwa Bildverarbeitung und Verarbeitung natürlicher Sprache) an und erstellt angereicherte Inhalte (neue Strukturen und Informationen) als Ausgabe. Eines der physischen Artefakte, die durch eine Pipeline erstellt werden, ist ein [Wissensspeicher](knowledge-store-concept-intro.md), auf den Sie über Tools zugreifen können, um den Inhalt zu analysieren und zu untersuchen.

In dieser Schnellstartanleitung werden Dienste und Daten in der Azure-Cloud miteinander kombiniert, um einen Wissensspeicher zu erstellen. Nach Abschluss der Einrichtung wird im Portal der **Datenimport-Assistent** ausgeführt, um alles miteinander zu verknüpfen. Am Ende verfügen Sie neben den ursprünglichen Inhalten über angereicherte Inhalte, die Sie im Portal mithilfe des [Storage-Explorer](knowledge-store-view-storage-explorer.md) anzeigen können.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="create-services-and-load-data"></a>Erstellen von Diensten und Laden von Daten

In dieser Schnellstartanleitung werden Azure Cognitive Search, Azure Blob Storage und [Azure Cognitive Services](https://azure.microsoft.com/services/cognitive-services/) (für die KI) verwendet. 

Aufgrund der geringen Workloadgröße wird Cognitive Services im Hintergrund genutzt und bietet eine kostenlose Verarbeitung von bis zu 20 Transaktionen pro Tag für Aufrufe über Azure Cognitive Search. Solange Sie die von uns bereitgestellten Beispieldaten verwenden, können Sie das Erstellen oder Anfügen einer Cognitive Services-Ressource überspringen.

1. [Laden Sie „HotelReviews_Free.csv“ herunter.](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Free.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D) Bei diesen Daten handelt es sich um gespeicherte Hotelrezensionen (von Kaggle.com) in einer CSV-Datei. Die Daten umfassen 19 Kundenfeedbacks für ein einzelnes Hotel. 

1. [Erstellen Sie ein Azure-Speicherkonto](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal), oder [suchen Sie nach einem vorhandenen Konto](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Storage%2storageAccounts/) in Ihrem aktuellen Abonnement. Azure Storage wird sowohl für die zu importierenden Rohinhalte als auch für den resultierenden Wissensspeicher verwendet.

   Das Konto muss zwei Anforderungen erfüllen:

   + Es muss sich in der gleichen Region befinden wie Azure Cognitive Search. 
   
   + Es muss den Kontotyp „StorageV2 (allgemein, Version 2)“ besitzen. 

1. Öffnen Sie die Seiten für Blobdienste, und erstellen Sie einen Container.  

1. Klicken Sie auf **Hochladen**.

    ![Hochladen der Daten](media/knowledge-store-create-portal/upload-command-bar.png "Hochladen der Hotelrezensionen")

1. Wählen Sie die Datei **HotelReviews-Free.csv** aus, die Sie im ersten Schritt heruntergeladen haben.

    ![Erstellen des Azure-Blobcontainers](media/knowledge-store-create-portal/hotel-reviews-blob-container.png "Erstellen des Azure-Blobcontainers")

1. Die Ressource ist fast fertig. Öffnen Sie vor dem Verlassen dieser Seiten jedoch über einen Link im linken Navigationsbereich die Seite **Zugriffsschlüssel**. Rufen Sie eine Verbindungszeichenfolge für den Datenabruf aus Blob Storage ab. Eine Verbindungszeichenfolge sieht in etwa wie im folgenden Beispiel aus: `DefaultEndpointsProtocol=https;AccountName=<YOUR-ACCOUNT-NAME>;AccountKey=<YOUR-ACCOUNT-KEY>;EndpointSuffix=core.windows.net`

1. [Erstellen Sie einen Azure Cognitive Search-Dienst](search-create-service-portal.md), oder [suchen Sie nach einem vorhandenen Dienst](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) im gleichen Abonnement. Für diesen Schnellstart können Sie einen kostenlosen Dienst verwenden.

Nun können Sie zum Datenimport-Assistenten wechseln.

## <a name="run-the-import-data-wizard"></a>Ausführen des Datenimport-Assistenten

Klicken Sie auf der Übersichtsseite des Suchdiensts auf der Befehlsleiste auf **Daten importieren**, um in vier Schritten einen Wissensspeicher zu erstellen.

  ![Befehl zum Importieren von Daten](media/cognitive-search-quickstart-blob/import-data-cmd2.png)

### <a name="step-1-create-a-data-source"></a>Schritt 1: Erstellen einer Datenquelle

1. Wählen Sie unter **Verbindung mit Ihren Daten herstellen** die Option **Azure Blob Storage** sowie das erstellte Konto und den erstellten Container aus. 
1. Geben Sie unter **Name** den Namen `hotel-reviews-ds` ein.
1. Wählen Sie unter **Analysemodus** die Option **Durch Trennzeichen getrennter Text** aus, und aktivieren Sie anschließend das Kontrollkästchen **Erste Zeile enthält Überschrift**. Vergewissern Sie sich, dass unter **Trennzeichen** ein Komma (,) angegeben ist.
1. Geben Sie unter **Verbindungszeichenfolge** die Verbindungszeichenfolge Ihres Speicherdiensts ein, die Sie in einem früheren Schritt gespeichert haben.
1. Geben Sie unter **Containername** den Namen `hotel-reviews` ein.
1. Klicken Sie auf **Weiter: KI-Anreicherung hinzufügen (Optional)** .

      ![Erstellen eines Datenquellenobjekts](media/knowledge-store-create-portal/hotel-reviews-ds.png "Erstellen eines Datenquellenobjekts")

1. Wechseln Sie zur nächsten Seite.

### <a name="step-2-add-cognitive-skills"></a>Schritt 2: Hinzufügen von kognitiven Qualifikationen

In diesem Schritt des Assistenten erstellen Sie ein Skillset mit Anreicherungen für kognitive Qualifikationen. Die in diesem Beispiel verwendeten Qualifikationen dienen zum Extrahieren von Schlüsselbegriffen sowie zum Erkennen der Sprache und der Stimmung. Diese Anreicherungen werden in einem späteren Schritt als Azure-Tabellen in einen Wissensspeicher „projiziert“.

1. Erweitern Sie **Cognitive Services-Instanz anfügen**. Standardmäßig ist **Free (begrenzte Anreicherung)** ausgewählt. Da „HotelReviews-Free.csv“ 19 Datensätze enthält und mit der kostenlosen Ressource 20 Transaktionen pro Tag möglich sind, können Sie diese Ressource verwenden.
1. Erweitern Sie **Kognitive Qualifikationen hinzufügen (Optional)** .
1. Geben Sie unter **Name des Skillsets** den Namen `hotel-reviews-ss` ein.
1. Wählen Sie unter **Quelldatenfeld** die Option *reviews_text* aus.
1. Wählen Sie unter **Granularitätsebene für Anreicherung** die Option **Seiten (5.000 Zeichenblöcke)** aus.
1. Wählen Sie die folgenden kognitiven Qualifikationen aus:
    + **How to extract key phrases in Text Analytics** (Extrahieren von Schlüsselbegriffen mithilfe der Textanalyse)
    + **Sprache erkennen**
    + **Stimmung erkennen**

      ![Erstellen eines Skillsets](media/knowledge-store-create-portal/hotel-reviews-ss.png "Erstellen eines Skillsets")

1. Erweitern Sie **Anreicherungen in einem Wissensspeicher speichern (Vorschau)** .
1. Geben Sie unter **Verbindungszeichenfolge für Speicherkonto** die Verbindungszeichenfolge ein, die Sie in einem früheren Schritt gespeichert haben.
1. Wählen Sie unter **Azure-Tabellenprojektionen** die folgenden Projektionen aus:
    + **Dokumente**
    + **Seiten**
    + **Schlüsselbegriffe**

    ![Konfigurieren des Wissensspeichers](media/knowledge-store-create-portal/hotel-reviews-ks.png "Konfigurieren des Wissensspeichers")

1. Wechseln Sie zur nächsten Seite.

### <a name="step-3-configure-the-index"></a>Schritt 3: Konfigurieren des Index

In diesem Schritt des Assistenten konfigurieren Sie einen Index für optionale Volltextsuchabfragen. Der Assistent analysiert Ihre Datenquelle anhand einer Stichprobe, um Felder und Datentypen abzuleiten. Sie müssen lediglich die Attribute für das gewünschte Verhalten auswählen. Mit dem Attribut **Abrufbar** kann der Suchdienst beispielsweise einen Feldwert zurückgeben. Das Attribut **Suchbar** ermöglicht dagegen eine Volltextsuche für das Feld.

1. Geben Sie unter **Indexname** den Namen `hotel-reviews-idx` ein.
1. Konfigurieren Sie die Attribute wie folgt:
    + Wählen Sie **Abrufbar** für alle Felder aus.
    + Wählen Sie **Filterbar** und **Facettenreich** für folgende Felder aus: *Sentiment*, *Language*, *Keyphrases*
    + Wählen Sie **Suchbar** für folgende Felder aus: *city*, *name*, *reviews_text*, *language*, *Keyphrases*

    Ihr Index sollte in etwa wie in der folgenden Abbildung aussehen: Aufgrund der Länger der Liste sind in der Abbildung nicht alle Felder zu sehen.

    ![Konfigurieren eines Index](media/knowledge-store-create-portal/hotel-reviews-idx.png "Konfigurieren eines Index")

1. Wechseln Sie zur nächsten Seite.

### <a name="step-4-configure-the-indexer"></a>Schritt 4: Konfigurieren des Indexers

In diesem Schritt des Assistenten konfigurieren Sie einen Indexer, der die Datenquelle, das Skillset und den Index aus den vorherigen Schritten zusammenführt.

1. Geben Sie unter **Name** `hotel-reviews-idxr` ein.
1. Behalten Sie unter **Zeitplan** den Standardwert **Einmalig** bei.
1. Klicken Sie auf **Senden**, um den Indexer auszuführen. In diesem Schritt werden die Datenextraktion und die Indizierung durchgeführt und die kognitiven Qualifikationen angewendet.

## <a name="monitor-status"></a>Überwachen des Status

Die Indizierung kognitiver Qualifikationen dauert länger als die übliche textbasierte Indizierung. Der Assistent sollte die Liste „Indexer“ auf der Übersichtsseite öffnen, damit Sie den Status nachverfolgen können. Zur eigenen Navigation wechseln Sie zur Seite „Übersicht“, und klicken Sie auf **Indexer**.

Im Azure-Portal können Sie auch das Benachrichtigungsaktivitätsprotokoll auf einen klickbaren Statuslink für eine **Azure Cognitive Search-Benachrichtigung** überprüfen. Die Ausführung kann mehrere Minuten dauern.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Ihre Daten mithilfe von Cognitive Services angereichert und die Ergebnisse in einen Wissensspeicher projiziert haben, können Sie den Storage-Explorer oder Power BI verwenden, um Ihr angereichertes Dataset zu untersuchen.

Sie können Inhalte im Storage-Explorer anzeigen oder mit Power BI noch einen Schritt weiter gehen und Erkenntnisse mittels Visualisierung gewinnen.

> [!div class="nextstepaction"]
> [Anzeigen mit dem Storage-Explorer](knowledge-store-view-storage-explorer.md)
> [Verbinden mit Power BI](knowledge-store-connect-power-bi.md)

> [!Tip]
> Wenn Sie diese Übung wiederholen oder eine andere exemplarische Vorgehensweise für die KI-Anreicherung ausprobieren möchten, löschen Sie den Indexer *hotel-reviews-idxr*. Durch Löschen des Indexers wird der Zähler für kostenlose Transaktionen pro Tag für die Cognitive Services-Verarbeitung auf Null zurückgesetzt.
