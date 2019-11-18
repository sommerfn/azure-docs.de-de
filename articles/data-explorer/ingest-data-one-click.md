---
title: Erfassen von Daten in Azure Data Explorer mithilfe der 1-Klick-Erfassung
description: Hier finden Sie Informationen dazu, wie Sie Daten mühelos mit der 1-Klick-Erfassung in Azure Data Explorer erfassen (laden).
author: orspod
ms.author: orspodek
ms.reviewer: tzgitlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 10/31/2019
ms.openlocfilehash: 98598a28e14dfd8175cbb019ff1b001c65503580
ms.sourcegitcommit: 359930a9387dd3d15d39abd97ad2b8cb69b8c18b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73644591"
---
# <a name="use-one-click-ingestion-to-ingest-data-into-azure-data-explorer"></a>Erfassen von Daten in Azure Data Explorer mithilfe der 1-Klick-Erfassung

In diesem Artikel wird gezeigt, wie Sie mit der 1-Klick-Erfassung schnell eine neue Tabelle im JSON- oder CSV-Format aus Speicher in Azure Data Explorer erfassen. Sobald die Daten erfasst wurden, können Sie die Tabelle bearbeiten und Abfragen mithilfe der Webbenutzeroberfläche ausführen.

## <a name="prerequisites"></a>Voraussetzungen

* Wenn Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
* Melden Sie sich bei der [Anwendung](https://dataexplorer.azure.com/) an.
* Erstellen Sie [einen Azure Data Explorer-Cluster und eine Datenbank](create-cluster-database-portal.md).
* Melden Sie sich bei der [Webbenutzeroberfläche](https://dataexplorer.azure.com/) an, und [fügen Sie eine Verbindung mit Ihrem Cluster hinzu](/azure/data-explorer/web-query-data#add-clusters).
* Datenquelle in Azure Storage.

## <a name="ingest-new-data"></a>Erfassen neuer Daten

1. Klicken Sie im linken Menü der Webbenutzeroberfläche mit der rechten Maustaste auf die Zeile *Datenbank* oder *Tabelle*, und wählen Sie **Neue Daten erfassen (Vorschau)** aus.

    ![Auswahl der 1-Klick Erfassung in der Webbenutzeroberfläche](media/ingest-data-one-click/one-click-ingestion-in-webui.png)   
 
1. Vervollständigen Sie im Fenster **Neue Daten erfassen (Vorschau)** auf der Registerkarte **Quelle** die **Projektdetails**:

    * **Tabelle**: Wählen Sie im Dropdown den Namen einer vorhandenen Tabelle aus, oder klicken Sie auf **Neu erstellen**, um eine neue Tabelle zu erstellen.
    * Wählen Sie **Erfassungstyp** > **Aus Speicher** oder **Aus Datei** aus.
        * Geben Sie bei Auswahl von **Aus Speicher** unter **Link zum Speicher** eine URL zum Speicher ein. Verwenden Sie die [Blob-SAS-URL](/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container) für private Speicherkonten. 
        * Wenn Sie **Aus Datei** ausgewählt haben, klicken Sie auf **Durchsuchen**, und ziehen Sie die Datei in das Feld.
    * Wählen Sie **Schema bearbeiten** aus, um Ihre Tabellenspaltenkonfiguration anzuzeigen und zu bearbeiten.
 
    ![Details zur Quelle bei der 1-Klick Erfassung](media/ingest-data-one-click/one-click-ingestion-source.png) 

    > [!TIP]
    > Wenn Sie **Neue Daten erfassen (Vorschau)** für eine Zeile vom Typ *Tabelle* ausgewählt haben, wird der Name der ausgewählten Tabelle in den **Projektdetails** angezeigt.

1. Wenn Sie eine vorhandene Tabelle ausgewählt haben, wird das Fenster **Spalten zuordnen** geöffnet, um eine Zuordnung zwischen Quelldatenspalten und Zieltabellenspalten durchzuführen. 
    * Verwenden Sie **Spalte auslassen**, um eine Zielspalte aus der Tabelle zu entfernen. 
    * Verwenden Sie **Neue Spalte**, um Ihrer Tabelle eine neue Spalte hinzuzufügen. 

    ![Fenster für die Spaltenzuordnung](media/ingest-data-one-click/one-click-map-columns-window.png)

1. Auf der Registerkarte **Schema**:

    * Wählen Sie im Dropdown **Komprimierungstyp** die Einstellung **Nicht komprimiert** oder **GZip** aus.
    * Wählen Sie im Dropdown **Datenformat** aus den Einstellungen **JSON**, **CSV**, **TSV**, **SCSV**, **SOHSV**, **TSVE** oder **PSV** aus. 
        * Bei Auswahl des **JSON**-Formats müssen Sie eine **JSON-Ebene** festlegen: 1–10. Die Ebene wirkt sich auf die Darstellung der Tabellenspaltendaten aus. 
        * Wenn Sie ein anderes Format als JSON auswählen, aktivieren Sie das Kontrollkästchen **Spaltennamen einschließen**, um die Überschriftenzeile der Datei zu ignorieren.    
    * **Zuordnungsname** wird automatisch festgelegt, kann jedoch bearbeitet werden.
    * Wenn Sie eine vorhandene Tabelle auswählen, können Sie auf die Schaltfläche **Spalten zuordnen** klicken, um das Fenster **Spalten zuordnen** zu öffnen.

    ![1-Klick-Erfassung, CSV-Format, schema.png](media/ingest-data-one-click/one-click-csv-format.png)

1. Klicken Sie rechts im **Editor** auf **V**, um den Editor zu öffnen. Im Editor können Sie die aus Ihren Eingaben generierten automatischen Abfragen anzeigen und kopieren. 

1.  In der Tabelle: 
    * Klicken Sie mit der rechten Maustaste auf die Überschriften einer neuen Zeile, um eine dieser Aufgaben auszuführen: **Datentyp ändern**, **Spalte umbenennen**, **Spalte löschen**, **Aufsteigend sortieren**, **Absteigend sortieren**. Für vorhandene Spalten steht nur eine Datensortierung zur Verfügung. 
    * Doppelklicken Sie zur Bearbeitung auf den Namen der neuen Spalte.

1. Wählen Sie **Erfassung starten** aus, um die Tabelle zu erstellen, die Zuordnung zu erstellen und die Datenerfassung zu starten.

    ![1-Klick-Erfassung, JSON-Formatschema](media/ingest-data-one-click/one-click-json-format.png) 
 
## <a name="query-data"></a>Abfragen von Daten

1. Im Fenster **Datenerfassung abgeschlossen** werden alle drei Schritte mit grünen Häkchen markiert, wenn die Datenerfassung erfolgreich abgeschlossen wurde. 
 
    ![1-Klick-Datenerfassung abgeschlossen](media/ingest-data-one-click/one-click-data-ingestion-complete.png)

1. Wählen Sie **V** aus, um die Abfrage zu öffnen. In Webbenutzeroberfläche kopieren, um die Abfrage zu bearbeiten.

1. Das Menü auf der rechten Seite enthält **Schnellabfragen** und **Tools**. 

    * **Schnellabfragen** enthält Links zur Webbenutzeroberfläche mit Beispielabfragen.
    * **Tools** enthält den Link zur Webbenutzeroberfläche mit **Drop-Befehlen**, die Ihnen die Problembehandlung durch Ausführen des entsprechenden `.drop`-Befehls ermöglichen.

    > [!TIP]
    > Bei Verwendung von `.drop`-Befehlen können Daten verloren gehen. Verwenden Sie sie daher mit Vorsicht.

## <a name="next-steps"></a>Nächste Schritte

* [Abfragen von Daten auf der Azure Data Explorer-Webbenutzeroberfläche](web-query-data.md)
* [Schreiben von Abfragen für Azure Data Explorer mithilfe der Kusto-Abfragesprache](write-queries.md)
