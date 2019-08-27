---
title: Erfassen von Daten in Azure Data Explorer mithilfe der 1-Klick-Erfassung
description: Hier finden Sie Informationen dazu, wie Sie Daten mühelos mit der 1-Klick-Erfassung in Azure Data Explorer erfassen (laden).
author: orspod
ms.author: orspodek
ms.reviewer: tzgitlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 08/14/2019
ms.openlocfilehash: b1ce2d9efe44021b4e3191739bd2f922e34c44cb
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/15/2019
ms.locfileid: "69519829"
---
# <a name="use-one-click-ingestion-to-ingest-data-into-azure-data-explorer"></a>Erfassen von Daten in Azure Data Explorer mithilfe der 1-Klick-Erfassung

In diesem Artikel wird gezeigt, wie Sie mit der 1-Klick-Erfassung schnell eine neue Tabelle im JSON- oder CSV-Format aus Speicher in Azure Data Explorer erfassen. Sobald die Daten erfasst wurden, können Sie die Tabelle bearbeiten und Abfragen mithilfe der Webbenutzeroberfläche ausführen.

## <a name="prerequisites"></a>Voraussetzungen

* Wenn Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
* Melden Sie sich bei der [Anwendung](https://dataexplorer.azure.com/) an.
* Erstellen Sie [einen Azure Data Explorer-Cluster und eine Datenbank](create-cluster-database-portal.md).
* Datenquelle in Azure Storage.

## <a name="ingest-new-data"></a>Erfassen neuer Daten

1. Klicken Sie mit der rechten Maustaste auf den *Datenbanknamen*, und wählen Sie **Neue Daten erfassen (Vorschau)** aus.

    ![Auswahl der 1-Klick Erfassung in der Webbenutzeroberfläche](media/ingest-data-one-click/one-click-ingestion-in-webui.png)   
 
1. Vervollständigen Sie im Fenster **Datenerfassung (Vorschau)** auf der Registerkarte **Quelle** die **Projektdetails**:

    * Geben Sie einen neuen **Tabellennamen** ein. 
    * Wählen Sie **Erfassungstyp** >  **aus Speicher** aus.
    * Geben Sie **Mit Speicher verknüpfen** „URL zum Speicher hinzufügen“ ein. Verwenden Sie die Blob-SAS-URL für private Speicherkonten. 
    * Wählen Sie **Schema bearbeiten** aus.
 
    ![Details zur Quelle bei der 1-Klick Erfassung](media/ingest-data-one-click/one-click-ingestion-source.png) 

1. Wählen Sie auf der Registerkarte **Schema** in der Dropdownliste **Datenformat** > **JSON** oder **CSV** aus. 
   
   Wenn Sie **CSV** auswählen:
    * Aktivieren Sie das Kontrollkästchen **Überschrift ignorieren**, um die Überschriftenzeile der CSV-Datei zu ignorieren.    
    * **Zuordnungsname** wird automatisch festgelegt, kann jedoch bearbeitet werden.

    ![1-Klick-Erfassung, CSV-Format, schema.png](media/ingest-data-one-click/one-click-csv-format.png)

   Wenn Sie **JSON** auswählen:
    * Wählen Sie **JSON-Ebenen**: 1-10 aus der Dropdownliste aus. Die Ebenen in der JSON-Datei werden in der Tabelle unten rechts angezeigt. 
    * **Zuordnungsname** wird automatisch festgelegt, kann jedoch bearbeitet werden.

    ![1-Klick-Erfassung, JSON-Formatschema](media/ingest-data-one-click/one-click-json-format.png)  

1. Wählen Sie im **Editor** auf der rechten Seite **V** aus, um den Editor zu öffnen. Im Editor können Sie die aus Ihren Eingaben generierten automatischen Abfragen anzeigen und kopieren. 

1.  Unten rechts in Tabelle: 
    * Wählen Sie rechts neben der Spalte **V** aus, um die **Spalte umzubenennen**, **zu löschen**, **aufsteigend zu sortieren** oder **absteigend zu sortieren**.
    * Doppelklicken Sie auf den Spaltennamen, um ihn zu bearbeiten.
    * Wählen Sie das Symbol links neben dem Spaltennamen aus, um den Datentyp zu ändern. 

1. Wählen Sie **Erfassung starten** aus, um die Tabelle zu erstellen, die Zuordnung zu erstellen und die Datenerfassung zu starten.
 
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
