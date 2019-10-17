---
title: Visuelles Überwachen von Azure Data Factory | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Azure Data Factorys visuell überwachen.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/19/2018
ms.openlocfilehash: f7c27bde3806684045bc43f8ff99eefb14c8d04a
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2019
ms.locfileid: "72029142"
---
# <a name="visually-monitor-azure-data-factories"></a>Visuelles Überwachen von Azure Data Factory
Azure Data Factory ist ein cloudbasierter Datenintegrationsdienst. Mit diesem Dienst können Sie in der Cloud datengesteuerte Workflows zur Orchestrierung und Automatisierung der Datenverschiebung und Datentransformation erstellen. Durch Verwendung von Azure Data Factory haben Sie folgende Möglichkeiten:

- Erstellen und Planen von datengesteuerten Workflows (so genannte Pipelines), die Daten aus unterschiedlichen Datenspeichern erfassen können
- Verarbeiten oder Transformieren der Daten mithilfe von Compute Services wie Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics und Azure Machine Learning
- Veröffentlichen von Ausgabedaten für Datenspeicher wie Azure SQL Data Warehouse für die Nutzung durch BI-Anwendungen (Business Intelligence)

In dieser Schnellstartanleitung erfahren Sie, wie Sie Data Factory-Pipelines visuell überwachen, ohne eine einzige zu Codezeile schreiben.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="monitor-data-factory-pipelines"></a>Überwachen von Data Factory-Pipelines

Überwachen Sie Pipeline- und Aktivitätsausführungen in einer einfachen Benutzeroberfläche mit Listenansicht. Alle Ausführungen werden in der lokalen Zeitzone des Browsers angezeigt. Wenn Sie die Zeitzone ändern, werden alle Datums- und Zeitfelder auf die ausgewählte Zeitzone ausgerichtet.  

1. Starten Sie Microsoft Edge oder Google Chrome. Derzeit wird die Data Factory-Benutzeroberfläche nur in diesen beiden Webbrowsern unterstützt.
2. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
3. Wechseln Sie zum Blatt für die erstellte Data Factory im Azure-Portal. Wählen Sie die Kachel **Überwachung und Verwaltung** aus, um die visuelle Überwachung von Data Factory zu starten.

## <a name="monitor-pipeline-runs"></a>Überwachen der Pipelineausführungen
In der Listenansicht wird jede Pipelineausführung für Ihre Data Factory-Pipelines angezeigt. Sie enthält folgende Spalten:

| **Spaltenname** | **Beschreibung** |
| --- | --- |
| Pipeline Name | Name der Pipeline |
| Aktionen | Verfügbare einzelne Aktionen zum Anzeigen von Aktivitätsausführungen |
| Run Start | Startdatum und -uhrzeit der Pipelineausführung (MM/TT/JJJJ, HH:MM:SS AM/PM) |
| Duration | Dauer der Ausführung (hh:mm:ss) |
| Triggered By | Manueller Trigger oder geplanter Trigger |
| Status | **Fehlerhaft**, **Erfolgreich** oder **In Bearbeitung** |
| Parameter | Parameter für die Pipelineausführung (Name-Wert-Paare) |
| Error | Fehler bei der Pipelineausführung (falls vorhanden) |
| Run ID | ID der Pipelineausführung |

![Listenansicht zum Überwachen von Pipelineausführungen](media/monitor-visually/pipeline-runs.png)

## <a name="monitor-activity-runs"></a>Überwachung der Aktivitätsausführungen
In der Listenansicht werden die Aktivitätsausführungen angezeigt, die den einzelnen Pipelineausführungen entsprechen. Wählen Sie das Symbol **Aktivitätsausführungen** unter der Spalte **Aktionen** aus, um die Aktivitätsausführungen für jede Pipelineausführung anzuzeigen. Die Listenansicht enthält folgende Spalten:

| **Spaltenname** | **Beschreibung** |
| --- | --- |
| Activity Name | Name der Aktivität innerhalb der Pipeline |
| Activity Type | Art der Aktivität, z. B. **Kopieren**, **HDInsightSpark** oder **HDInsightHive** |
| Run Start | Startdatum und -uhrzeit der Aktivitätsausführung (MM/TT/JJJJ, HH:MM:SS AM/PM) |
| Duration | Dauer der Ausführung (hh:mm:ss) |
| Status | **Fehlerhaft**, **Erfolgreich** oder **In Bearbeitung** |
| Eingabe | JSON-Array, das die Aktivitätseingaben beschreibt |
| Output | JSON-Array, das die Aktivitätsausgaben beschreibt |
| Error | Fehler bei der Aktivitätsausführung (falls vorhanden) |

![Listenansicht zum Überwachen von Aktivitätsausführungen](media/monitor-visually/activity-runs.png)

> [!IMPORTANT]
> Sie müssen die Schaltfläche **Aktualisieren** im oberen Bereich auswählen, um die Liste der Pipeline- und Aktivitätsausführungen zu aktualisieren. Ein automatisches Update wird derzeit nicht unterstützt.

![Schaltfläche „Aktualisieren“](media/monitor-visually/refresh.png)

## <a name="select-a-data-factory-to-monitor"></a>Auswählen einer zu überwachenden Data Factory
Zeigen Sie auf das Symbol **Data Factory** im linken oberen Bereich. Wählen Sie das Pfeilsymbol aus, um die Liste der Azure-Abonnements und Data Factorys anzuzeigen, die Sie überwachen können.

![Auswählen der Data Factory](media/monitor-visually/select-datafactory.png)

## <a name="configure-the-list-view"></a>Konfigurieren der Listenansicht

### <a name="apply-rich-ordering-and-filtering"></a>Umfangreiches Sortieren und Filtern

Sie können die Pipelineausführungen nach der Startzeit in auf- oder absteigender Reihenfolge sortieren. Sie können die Pipelineausführungen nach den folgenden Spalten filtern:

| **Spaltenname** | **Beschreibung** |
| --- | --- |
| Pipeline Name | Name der Pipeline. Die Optionen umfassen Schnellfilter für **Letzte 24 Stunden**, **Letzte Woche** und **Letzte 30 Tage**. Sie können auch ein benutzerdefiniertes Datum und eine Uhrzeit auswählen. |
| Run Start | Startdatum und -uhrzeit der Pipelineausführung |
| Run Status | Filtern der Ausführungen nach Status: **Erfolgreich**, **Fehlerhaft** oder **In Bearbeitung** |

![Optionen zum Filtern](media/monitor-visually/filter.png)

### <a name="add-or-remove-columns"></a>Hinzufügen oder Entfernen von Spalten
Klicken Sie mit der rechten Maustaste auf den Header der Listenansicht, und wählen Sie die Spalten aus, die in der Listenansicht angezeigt werden sollen.

![Optionen für Spalten](media/monitor-visually/columns.png)

### <a name="adjust-column-widths"></a>Anpassen der Spaltenbreite
Erhöhen oder verringern Sie die Spaltenbreiten in der Listenansicht, indem Sie mit der Maus auf die Spaltenüberschrift zeigen.

## <a name="promote-user-properties-to-monitor"></a>Heraufstufen von Benutzereigenschaften für die Überwachung

Sie können jede Pipelineaktivitätseigenschaft als Benutzereigenschaft heraufstufen, sodass sie zu einer Entität wird, die Sie überwachen können. Sie können z. B. die Eigenschaften **Quelle** und **Ziel** der Kopieraktivität in der Pipeline als Benutzereigenschaften höher stufen. Sie können auch **Automatisch generieren** auswählen, um die Benutzereigenschaften **Quelle** und **Ziel** für eine Kopieraktivität zu generieren.

![Erstellen von Benutzereigenschaften](media/monitor-visually/monitor-user-properties-image1.png)

> [!NOTE]
> Sie können nur bis zu fünf Pipelineaktivitätseigenschaften als Benutzereigenschaften höher stufen.

Nachdem Sie die Benutzereigenschaften erstellt haben, können Sie sie in den Überwachungslistenansichten überwachen. Wenn ein Tabellenname als Quelle für die Kopieraktivität dient, können Sie den Namen der Quelltabelle als Spalte in der Listenansicht für Aktivitätsausführungen überwachen.

![Aktivitätsausführungenliste ohne Benutzereigenschaften](media/monitor-visually/monitor-user-properties-image2.png)

![Hinzufügen von Spalten für Benutzereigenschaften zur Liste der Aktivitätsausführungen](media/monitor-visually/monitor-user-properties-image3.png)

![Aktivitätsausführungenliste mit Spalten für Benutzereigenschaften](media/monitor-visually/monitor-user-properties-image4.png)

## <a name="rerun-activities-inside-a-pipeline"></a>Erneutes Ausführen von Aktivitäten innerhalb einer Pipeline

Aktivitäten innerhalb einer Pipeline können nun erneut ausgeführt werden. Wählen Sie **View Activity Runs** (Aktivitätsausführungen anzeigen) und dann die Aktivität in Ihrer Pipeline aus, ab der die Pipeline erneut ausgeführt werden soll.

![Anzeigen der Aktivitätsausführungen](media/monitor-visually/rerun-activities-image1.png)

![Auswählen einer Aktivitätsausführung](media/monitor-visually/rerun-activities-image2.png)

### <a name="view-rerun-history"></a>Anzeigen des Verlaufs für erneute Ausführungen

Der Verlauf für erneute Ausführungen kann für alle Pipelineausführungen in der Listenansicht angezeigt werden.

![Anzeigen des Verlaufs](media/monitor-visually/rerun-history-image1.png)

Sie können aber auch den Verlauf für erneute Ausführungen für eine bestimmte Pipelineausführung anzeigen.

![Anzeigen des Verlaufs für eine Pipelineausführung](media/monitor-visually/rerun-history-image2.png)

## <a name="gantt-views"></a>Gantt-Ansichten

Mit Gantt-Ansichten können Sie schnell Ihre Pipelines und Aktivitätsausführungen anzeigen. Die Gantt-Ansicht kann pro Pipeline oder nach Gruppen über von Ihnen erstellte Anmerkungen und Tags für Ihre Pipelines angezeigt werden.

![Beispiel für ein Gantt-Diagramm](media/monitor-visually/gantt1.png)

![Gantt-Diagrammanmerkungen](media/monitor-visually/gantt2.png)

Die Länge des Balkens informiert über die Dauer der Pipeline. Sie können auch den Balken auswählen, um weitere Details anzuzeigen.

![Gantt-Diagramm zur Dauer](media/monitor-visually/gantt3.png)

## <a name="guided-tours"></a>Führungen
Wählen Sie links unten das **Informationssymbol** aus. Wählen Sie dann **Guided Tours** (Führungen) aus, um eine ausführliche Anleitung zum Überwachen der Pipeline- und Aktivitätsausführungen zu erhalten.

![Führungen](media/monitor-visually/guided-tours.png)

## <a name="feedback"></a>Feedback
Wählen Sie das Symbol **Feedback** aus, um uns Ihr Feedback zu den verschiedenen Funktionen oder zu möglichen Problemen zu übermitteln.

![Feedback](media/monitor-visually/feedback.png)

## <a name="alerts"></a>Alerts

Sie können Warnungen auf der Grundlage unterstützter Metriken in Data Factory auslösen. Wählen Sie dazu auf der Überwachungsseite von Data Factory die Option **Überwachen** > **Warnungen und Metriken** aus.

![Data Factory-Überwachungsseite](media/monitor-visually/alerts01.png)

Das folgende Video enthält eine siebenminütige Einführung und Demonstration dieses Features:

> [!VIDEO https://channel9.msdn.com/shows/azure-friday/Monitor-your-Azure-Data-Factory-pipelines-proactively-with-alerts/player]

### <a name="create-alerts"></a>Erstellen von Warnungen

1.  Wählen Sie **Neue Warnungsregel** aus, um eine neue Warnung zu erstellen.

    ![Schaltfläche „Neue Warnungsregel“](media/monitor-visually/alerts02.png)

1.  Geben Sie den Namen der Warnungsregel an, und wählen Sie den Schweregrad der Warnung aus.

    ![Felder für Regelname und Schweregrad](media/monitor-visually/alerts03.png)

1.  Wählen Sie die Warnungskriterien aus.

    ![Feld für Zielkriterien](media/monitor-visually/alerts04.png)

    ![Liste der Kriterien](media/monitor-visually/alerts05.png)

1.  Konfigurieren Sie die Warnungslogik. Sie können eine Warnung für die ausgewählte Metrik für alle Pipelines und entsprechenden Aktivitäten erstellen. Sie können außerdem einen bestimmten Aktivitätstyp, Aktivitätsnamen, Pipelinenamen oder Fehlertyp auswählen.

    ![Optionen zum Konfigurieren der Warnungslogik](media/monitor-visually/alerts06.png)

1.  Konfigurieren Sie E-Mail-, SMS-, Push- und Sprachbenachrichtigungen für die Warnung. Erstellen Sie eine Aktionsgruppe für die Warnungsbenachrichtigungen, oder wählen Sie eine vorhandene Aktionsgruppe aus.

    ![Optionen zum Konfigurieren von Benachrichtigungen](media/monitor-visually/alerts07.png)

    ![Optionen zum Hinzufügen einer Benachrichtigung](media/monitor-visually/alerts08.png)

1.  Erstellen Sie die Warnungsregel.

    ![Optionen zum Erstellen einer Warnungsregel](media/monitor-visually/alerts09.png)

## <a name="next-steps"></a>Nächste Schritte

Im Artikel [Programmgesteuertes Überwachen und Verwalten von Pipelines](https://docs.microsoft.com/azure/data-factory/monitor-programmatically) erfahren Sie mehr über das Überwachen und Verwalten von Pipelines.
