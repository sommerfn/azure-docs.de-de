---
title: Anzeigen von Azure Stream Analytics-Aufträgen in Visual Studio
description: Erfahren Sie, wie Sie mit Visual Studio Verbindungen anzeigen, starten, stoppen und testen, Ergebnisse überprüfen und Ihre Azure Stream Analytics-Aufträge exportieren.
services: stream-analytics
author: su-jie
ms.author: sujie
manager: kfile
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 07/10/2018
ms.openlocfilehash: d73955436e651dfdb9db9d602481213fcc2cf502
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2019
ms.locfileid: "72893556"
---
# <a name="use-visual-studio-to-view-azure-stream-analytics-jobs"></a>Anzeigen von Azure Stream Analytics-Aufträgen mit Visual Studio

Azure Stream Analytics-Tools für Visual Studio vereinfacht Entwicklern das Verwalten von Stream Analytics-Aufträgen direkt über die IDE. Azure Stream Analytics-Tools ermöglicht Folgendes:
- [Erstellen neuer Aufträge](stream-analytics-quick-create-vs.md)
- Starten, Beenden und [Überwachen von Aufträgen](stream-analytics-monitor-jobs-use-vs.md)
- Überprüfen von Auftragsergebnissen
- Exportieren vorhandener Aufträge in ein Projekt
- Testen von Eingabe- und Ausgabeverbindungen
- [Lokales Ausführen von Abfragen](stream-analytics-vs-tools-local-run.md)

Erfahren Sie mehr über das [Installieren der Azure Stream Analytics-Tools für Visual Studio](stream-analytics-tools-for-visual-studio-install.md).

## <a name="explore-the-job-view"></a>Untersuchen der Auftragsansicht

In der Auftragsansicht können Sie mit Azure Stream Analytics-Aufträgen aus Visual Studio arbeiten.

### <a name="open-the-job-view"></a>Öffnen der Auftragsansicht

1. Wählen Sie im **Server-Explorer** die Option **Stream Analytics-Aufträge** und dann **Aktualisieren** aus. Ihr Auftrag wird unter **Stream Analytics-Aufträge** angezeigt.

    ![Liste „Stream Analytics-Aufträge“ im Server-Explorer](./media/stream-analytics-vs-tools/stream-analytics-tools-for-vs-list-jobs-01.png)

2. Erweitern Sie den Auftragsknoten, und doppelklicken Sie auf den Knoten **Auftragsansicht**, um eine Auftragsansicht zu öffnen.
    
   ![Erweiterter Auftragsknoten](./media/stream-analytics-vs-tools/stream-analytics-tools-for-vs-job-view-01.png)

### <a name="start-and-stop-jobs"></a>Starten und Beenden von Aufträgen

Azure Stream Analytics-Aufträge können vollständig über die Auftragsansicht in Visual Studio verwaltet werden. Verwenden Sie die Steuerelemente zum Starten, Beenden oder Löschen eines Auftrags.
    
   ![Steuerelemente für Stream Analytics-Aufträge](./media/stream-analytics-vs-tools/azure-stream-analytics-job-view-controls.png)

## <a name="check-job-results"></a>Überprüfen von Auftragsergebnissen

Stream Analytics-Tools für Visual Studio unterstützt derzeit die Ausgabevorschauversion für Azure Data Lake Storage und Blob Storage. Um das Ergebnis anzuzeigen, doppelklicken Sie einfach auf den Ausgabeknoten des Auftragsdiagramms in der **Auftragsansicht**, und geben Sie die entsprechenden Anmeldeinformationen ein.

   ![Ausgabe des Stream Analytics-Auftragsblobs](./media/stream-analytics-vs-tools/stream-analytics-blob-preview.png)

## <a name="export-jobs-to-a-project"></a>Exportieren von Aufträgen in ein Projekt

Es gibt zwei Möglichkeiten zum Exportieren eines vorhandenen Auftrags in ein Projekt.

1. Klicken Sie im **Server-Explorer** unter dem Knoten „Stream Analytics-Aufträge“ mit der rechten Maustaste auf den Auftragsknoten. Wählen Sie **In neues Stream Analytics-Projekt exportieren** aus.
    
   ![Exportieren eines Auftrags in ein Projekt](./media/stream-analytics-vs-tools/stream-analytics-tools-for-vs-export-job-01.png)
    
    Das erstellte Projekt wird im **Projektmappen-Explorer** angezeigt.
    
   ![Projektmappen-Explorer](./media/stream-analytics-vs-tools/stream-analytics-tools-for-vs-export-job-02.png)

2. Wählen Sie in der Auftragsansicht **Projekt generieren** aus.
    
   ![Generieren eines Projekts über die Auftragsansicht](./media/stream-analytics-vs-tools/stream-analytics-tools-for-vs-export-job-03.png)

## <a name="test-connections"></a>Testen von Verbindungen

Eingabe- und Ausgabeverbindungen können über die **Auftragsansicht** getestet werden, indem die jeweilige Option aus der Dropdownliste **Verbindung testen** ausgewählt wird.

   ![Dropdownliste „Verbindung testen“](./media/stream-analytics-vs-tools/stream-analytics-test-connection-dropdown.png)

Die Ergebnisse für den Vorgang **Verbindung testen** werden im Fenster **Ausgabe** angezeigt.

   ![Ergebnisse für den Vorgang „Verbindung testen“](./media/stream-analytics-vs-tools/stream-analytics-test-connection-results.png)

## <a name="next-steps"></a>Nächste Schritte

* [Überwachen und Verwalten von Azure Stream Analytics-Aufträgen mithilfe von Visual Studio](stream-analytics-monitor-jobs-use-vs.md)
* [Schnellstart: Erstellen eines Stream Analytics-Auftrags mithilfe von Visual Studio](stream-analytics-quick-create-vs.md)
* [Tutorial: Bereitstellen eines Azure Stream Analytics-Auftrags mit CI/CD mithilfe von Azure Pipelines](stream-analytics-tools-visual-studio-cicd-vsts.md)
* [Kontinuierliche Integration und Entwicklung mit Stream Analytics-Tools](stream-analytics-tools-for-visual-studio-cicd.md)
