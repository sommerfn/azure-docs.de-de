---
title: Erstellen eines Wissensspeichers im Azure-Portal – Azure Search
description: Hier erfahren Sie, wie Sie unter Verwendung des Datenimport-Assistenten im Azure-Portal einen Azure Search-Wissensspeicher zum Speichern von Anreicherungen aus der Pipeline für die kognitive Suche erstellen.
author: lisaleib
services: search
ms.service: search
ms.topic: tutorial
ms.date: 09/03/2019
ms.author: v-lilei
ms.openlocfilehash: fb979a7ff4144694aecad0985c5bce9be2de05bd
ms.sourcegitcommit: 3f22ae300425fb30be47992c7e46f0abc2e68478
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "71265195"
---
# <a name="create-an-azure-search-knowledge-store-in-the-azure-portal"></a>Erstellen eines Azure Search-Wissensspeichers im Azure-Portal

> [!Note]
> Der Wissensspeicher befindet sich in der Vorschauphase und sollte nicht in der Produktion verwendet werden. Dieses Feature wird durch die [Azure Search-REST-API-Version 2019-05-06-Preview](search-api-preview.md) bereitgestellt. Das .NET SDK wird derzeit nicht unterstützt.
>

Der Wissensspeicher ist ein Azure Search-Feature, das Ausgaben aus einer KI-Anreicherungspipeline zur späteren Analyse oder anderweitigen Downstreamverarbeitung speichert. Eine Pipeline mit KI-Anreicherung akzeptiert Bilddateien oder unstrukturierte Textdateien, indiziert sie mithilfe von Azure Search, wendet KI-Anreicherungen von Cognitive Services (beispielsweise Bildanalyse und Verarbeitung natürlicher Sprache) an, und speichert die Ergebnisse in einem Wissensspeicher in Azure Storage. Der Wissensspeicher kann dann mithilfe von Tools wie Power BI oder Storage-Explorer erkundet werden.

In diesem Artikel verwenden Sie den Datenimport-Assistenten im Azure-Portal, um eine Gruppe von Hotelrezensionen zu erfassen, zu indizieren und KI-Anreicherungen auf sie anzuwenden. Die Hotelrezensionen werden in Azure Blob Storage importiert und die Ergebnisse als Wissensspeicher in Azure Table Storage gespeichert.

Nachdem Sie den Wissensspeicher erstellt haben, können Sie sich darüber informieren, wie Sie per Storage-Explorer oder Power BI auf diesen Wissensspeicher zugreifen.

## <a name="prerequisites"></a>Voraussetzungen

+ [Erstellen Sie einen Azure Search-Dienst](search-create-service-portal.md), oder suchen Sie in Ihrem aktuellen Abonnement [nach einem vorhandenen Dienst](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices). In diesem Tutorial können Sie einen kostenlosen Dienst verwenden.

+ [Erstellen Sie ein Azure-Speicherkonto](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) zum Speichern der Beispieldaten und des Wissensspeichers. Ihr Speicherkonto muss den gleichen Standort haben wie Ihr Azure Search-Dienst (beispielsweise „USA, Westen“), und die *Kontoart* muss *StorageV2 (allgemein, Version 2)* (Standardeinstellung) oder *Storage (allgemein, Version 1)* lauten.

## <a name="load-the-data"></a>Laden der Daten

Laden Sie die CSV-Datei mit den Hotelrezensionen in Azure Blob Storage, damit sie für einen Azure Search-Indexer zur Verfügung steht und die KI-Anreicherungspipeline durchlaufen kann.

### <a name="create-an-azure-blob-container-with-the-data"></a>Erstellen eines Azure-Blobcontainers mit den Daten

1. [Laden Sie die CSV-Datei „HotelReviews_Free.csv“ mit den gespeicherten Hotelrezensionsdaten herunter.](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Free.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D) Diese Daten stammen von Kaggle.com und enthalten Kundenfeedback zu Hotels.
1. [Melden Sie sich beim Azure-Portal an](https://portal.azure.com), und navigieren Sie zu Ihrem Azure-Speicherkonto.
1. [Erstellen Sie einen Blobcontainer.](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) Klicken Sie hierzu auf der linken Navigationsleiste für Ihr Speicherkonto auf **Blobs** und anschließend auf der Befehlsleiste auf **+ Container**.
1. Geben Sie unter **Name** den Namen `hotel-reviews` für den neuen Container ein.
1. Wählen Sie unter **Öffentliche Zugriffsebene** eine beliebige Option aus. Wir haben den Standardwert verwendet.
1. Klicken Sie auf **OK**, um den Azure-Blobcontainer zu erstellen.
1. Öffnen Sie den neuen Container (`hotels-review`), klicken Sie auf **Hochladen**, und wählen Sie die Datei **HotelReviews-Free.csv** aus, die Sie im ersten Schritt heruntergeladen haben.

    ![Hochladen der Daten](media/knowledge-store-create-portal/upload-command-bar.png "Hochladen der Hotelrezensionen")

1. Klicken Sie auf **Hochladen**, um die CSV-Datei in Azure Blob Storage zu importieren. Daraufhin wird der neue Container angezeigt.

    ![Erstellen des Azure-Blobcontainers](media/knowledge-store-create-portal/hotel-reviews-blob-container.png "Erstellen des Azure-Blobcontainers")

### <a name="get-the-azure-storage-account-connection-string"></a>Abrufen der Verbindungszeichenfolge des Azure Storage-Kontos

1. Navigieren Sie im Portal zu Ihrem Azure Storage-Konto.
1. Klicken Sie im linken Navigationsbereich für den Dienst auf **Zugriffsschlüssel**.
1. Kopieren Sie unter **Schlüssel 1** die *Verbindungszeichenfolge*, und speichern Sie sie. Die Zeichenfolge beginnt mit `DefaultEndpointsProtocol=https`. Name und Schlüssel Ihres Speicherkontos sind in die Zeichenfolge eingebettet. Halten Sie diese Zeichenfolge bereit. Sie wird später benötigt.

## <a name="create-and-run-ai-enrichments"></a>Erstellen und Ausführen von KI-Anreicherungen

Verwenden Sie den Datenimport-Assistenten, um den Wissensspeicher zu erstellen. Sie erstellen eine Datenquelle, wählen Anreicherungen aus, konfigurieren einen Wissensspeicher und einen Index und führen den Prozess anschließend aus.

### <a name="start-the-import-data-wizard"></a>Starten des Datenimport-Assistenten

1. [Suchen Sie im Azure-Portal nach Ihrem Suchdienst.](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices)

1. Klicken Sie auf der Befehlsleiste auf **Daten importieren**, um den Import-Assistenten zu starten.

### <a name="connect-to-your-data-import-data-wizard"></a>Daten verbinden (Datenimport-Assistent)

In diesem Schritt des Assistenten erstellen Sie eine Datenquelle auf der Grundlage des Azure-Blobs mit Ihren Hoteldaten.

1. Wählen Sie in der Liste **Datenquelle** die Option **Azure Blob Storage** aus.
1. Geben Sie unter **Name** den Namen `hotel-reviews-ds` ein.
1. Wählen Sie unter **Analysemodus** die Option **Durch Trennzeichen getrennter Text** aus, und aktivieren Sie anschließend das Kontrollkästchen **Erste Zeile enthält Überschrift**. Vergewissern Sie sich, dass unter **Trennzeichen** ein Komma (,) angegeben ist.
1. Geben Sie unter **Verbindungszeichenfolge** die Verbindungszeichenfolge Ihres Speicherdiensts ein, die Sie in einem früheren Schritt gespeichert haben.
1. Geben Sie unter **Containername** den Namen `hotel-reviews` ein.
1. Klicken Sie auf **Weiter: Kognitive Suche hinzufügen (Optional)** .

      ![Erstellen eines Datenquellenobjekts](media/knowledge-store-create-portal/hotel-reviews-ds.png "Erstellen eines Datenquellenobjekts")

## <a name="add-cognitive-search-import-data-wizard"></a>Kognitive Suche hinzufügen (Datenimport-Assistent)

In diesem Schritt des Assistenten erstellen Sie ein Skillset mit Anreicherungen für kognitive Qualifikationen. Die in diesem Beispiel verwendeten Qualifikationen dienen zum Extrahieren von Schlüsselbegriffen sowie zum Erkennen der Sprache und der Stimmung. Diese Anreicherungen werden als Azure-Tabellen in einen Wissensspeicher „projiziert“.

1. Erweitern Sie **Cognitive Services-Instanz anfügen**. Standardmäßig ist **Free (begrenzte Anreicherung)** ausgewählt. Da „HotelReviews-Free.csv“ 19 Datensätze enthält und mit der kostenlosen Ressource 20 Transaktionen pro Tag möglich sind, können Sie diese Ressource verwenden.
1. Erweitern Sie **Anreicherungen hinzufügen**.
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

1. Klicken Sie auf **Weiter: Zielindex anpassen**.

### <a name="import-data-import-data-wizard"></a>Daten importieren (Datenimport-Assistent)

In diesem Schritt des Assistenten konfigurieren Sie einen Index für optionale Volltextsuchabfragen. Der Assistent analysiert Ihre Datenquelle anhand einer Stichprobe, um Felder und Datentypen abzuleiten. Sie müssen lediglich die Attribute für das gewünschte Verhalten auswählen. Mit dem Attribut **Abrufbar** kann der Suchdienst beispielsweise einen Feldwert zurückgeben. Das Attribut **Suchbar** ermöglicht dagegen eine Volltextsuche für das Feld.

1. Geben Sie unter **Indexname** den Namen `hotel-reviews-idx` ein.
1. Konfigurieren Sie die Attribute wie folgt:
    + Wählen Sie **Abrufbar** für alle Felder aus.
    + Wählen Sie **Filterbar** und **Facettenreich** für folgende Felder aus: *Sentiment*, *Language*, *Keyphrases*
    + Wählen Sie **Suchbar** für folgende Felder aus: *city*, *name*, *reviews_text*, *language*, *Keyphrases*

    Ihr Index sollte in etwa wie in der folgenden Abbildung aussehen: Aufgrund der Länger der Liste sind in der Abbildung nicht alle Felder zu sehen.

    ![Konfigurieren eines Index](media/knowledge-store-create-portal/hotel-reviews-idx.png "Konfigurieren eines Index")

1. Klicken Sie auf **Weiter: Indexer erstellen**.

### <a name="create-an-indexer"></a>Erstellen eines Indexers

In diesem Schritt des Assistenten konfigurieren Sie einen Indexer, der die Datenquelle, das Skillset und den Index aus den vorherigen Schritten zusammenführt.

1. Geben Sie unter **Name** `hotel-reviews-idxr` ein.
1. Behalten Sie unter **Zeitplan** den Standardwert **Einmalig** bei.
1. Klicken Sie auf **Senden**, um den Indexer auszuführen. In diesem Schritt werden die Datenextraktion und die Indizierung durchgeführt und die kognitiven Qualifikationen angewendet.

### <a name="monitor-the-notifications-queue-for-status"></a>Überwachen des Status anhand der Benachrichtigungswarteschlange

1. Überprüfen Sie das Benachrichtigungsaktivitätsprotokoll im Azure-Portal auf einen klickbaren Statuslink vom Typ **Azure Search-Benachrichtigung**. Die Ausführung kann mehrere Minuten dauern.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Ihre Daten mithilfe von Cognitive Services angereichert und die Ergebnisse in einen Wissensspeicher projiziert haben, können Sie Storage-Explorer oder Power BI verwenden, um Ihr angereichertes Dataset zu erkunden.

In der folgenden exemplarischen Vorgehensweise erfahren Sie, wie Sie diesen Wissensspeicher mithilfe von Storage-Explorer erkunden:

> [!div class="nextstepaction"]
> [Anzeigen mit Storage-Explorer](knowledge-store-view-storage-explorer.md)

In der folgenden exemplarischen Vorgehensweise erfahren Sie, wie Sie diesen Wissensspeicher mit Power BI verbinden:

> [!div class="nextstepaction"]
> [Herstellen einer Verbindung mit Power BI](knowledge-store-connect-power-bi.md)

Wenn Sie diese Übung wiederholen oder eine andere exemplarische Vorgehensweise für die KI-Anreicherung ausprobieren möchten, löschen Sie den Indexer *hotel-reviews-idxr*. Durch Löschen des Indexers wird der Zähler für kostenlose Transaktionen pro Tag auf Null zurückgesetzt.
