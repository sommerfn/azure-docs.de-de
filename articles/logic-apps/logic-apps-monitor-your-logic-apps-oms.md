---
title: Überwachen von Logik-Apps mit Azure Monitor – Azure Logic Apps
description: Sammeln Sie mit Azure Monitor-Protokollen Erkenntnisse und Debuggingdaten für die Problembehandlung und Diagnose von Logik-App-Ausführungen.
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: divswa, LADocs
ms.topic: article
ms.date: 08/29/2019
ms.openlocfilehash: a038a05f03ce7a209ae82203441750749bc6c4c4
ms.sourcegitcommit: d200cd7f4de113291fbd57e573ada042a393e545
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2019
ms.locfileid: "70138806"
---
# <a name="get-insights-and-debugging-data-for-logic-apps-by-using-azure-monitor-logs"></a>Sammeln Sie mit Azure Monitor-Protokollen Erkenntnisse und Debuggingdaten für Logik-Apps.

Um Ihre Logik-Apps zu überwachen und umfassendere Details über sie zu erhalten, aktivieren Sie [Azure Monitor-Protokolle](../log-analytics/log-analytics-overview.md), wenn Sie die Logik-App erstellen. Azure Monitor-Protokolle bieten Diagnoseprotokollierung und -überwachung für Ihre Logik-Apps, wenn Sie die Lösung Logic Apps-Verwaltung im Azure-Portal installieren. Diese Lösung bietet auch aggregierte Informationen für Ihre Logik-App-Ausführungen mit spezifischen Details wie Status, Ausführungszeit, Status der erneuten Übermittlung und Korrelations-IDs. In diesem Artikel wird veranschaulicht, wie Sie Azure Monitor-Protokolle aktivieren, um Laufzeitereignisse und Daten für Ihre Logik-App-Ausführung anzuzeigen.

In diesem Thema wird gezeigt, wie Sie Azure Monitor-Protokolle einrichten, wenn Sie Ihre Logik-App erstellen. Führen Sie diese Schritte zum [Aktivieren der Diagnoseprotokollierung und Senden von Logik-App-Laufzeitdaten an Azure Monitor-Protokolle](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics) aus, um Azure Monitor-Protokolle für vorhandene Logik-Apps zu aktivieren.

> [!NOTE]
> Auf dieser Seite wurden zuvor Schritte zum Ausführen dieser Aufgaben mit der Microsoft Operations Management Suite (OMS) beschrieben, die [im Januar 2019 eingestellt wurde](../azure-monitor/platform/oms-portal-transition.md). Stattdessen finden Sie hier nun diese Schritte mit [Azure Monitor-Protokollen](../azure-monitor/platform/data-platform-logs.md), die den Begriff Log Analytics ersetzt haben. Protokolldaten werden immer noch in einem Log Analytics-Arbeitsbereich gespeichert und weiterhin mit dem gleichen Log Analytics-Dienst erfasst und analysiert. Weitere Informationen finden Sie unter [Azure Monitor-Terminologieänderungen](../azure-monitor/terminology.md).

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen, benötigen Sie einen Log Analytics-Arbeitsbereich. Erfahren Sie mehr über das [Erstellen eines Log Analytics-Arbeitsbereichs](../azure-monitor/learn/quick-create-workspace.md).

## <a name="turn-on-logging-for-new-logic-apps"></a>Aktivieren der Protokollierung für neue Logik-Apps

1. Erstellen Sie im [Azure-Portal](https://portal.azure.com) Ihre Logik-App. Klicken Sie im Hauptmenü von Azure auf **Ressource erstellen** > **Integration** > **Logik-App**.

   ![Erstellen einer neuen Logik-App](media/logic-apps-monitor-your-logic-apps-oms/create-new-logic-app.png)

1. Führen Sie unter **Logik-App** diese Schritte aus:

   1. Geben Sie einen Namen für Ihre Logik-App an, und wählen Sie Ihr Azure-Abonnement aus.

   1. Erstellen Sie eine Azure-Ressourcengruppe, oder wählen Sie sie aus. Wählen Sie den Speicherort für Ihre Logik-App aus.

   1. Wählen Sie unter **Log Analytics** die Option **Ein** aus.

   1. Wählen Sie in der Liste der **Log Analytics-Arbeitsbereiche** den Arbeitsbereich aus, an den Sie Daten für Ihre Logik-App-Ausführungen senden möchten.

      ![Angeben von Informationen zur Logik-App](./media/logic-apps-monitor-your-logic-apps-oms/create-logic-app-details.png)

      Nach Abschluss dieses Schritts erstellt Azure Ihre Logik-App, die jetzt Ihrem Log Analytics-Arbeitsbereich zugeordnet ist. Mit diesem Schritt wird die Lösung für die Logic Apps-Verwaltung automatisch in Ihrem Arbeitsbereich installiert.

   1. Wählen Sie **Erstellen**, wenn Sie fertig sind.

1. Fahren Sie mit [diesen Schritten](#view-logic-app-runs-oms) fort, um Ihre Logik-App-Ausführungen anzuzeigen.

## <a name="install-logic-apps-management-solution"></a>Installieren der Lösung Logic Apps-Verwaltung

Überspringen Sie diesen Schritt, falls Sie Azure Monitor-Protokolle bereits beim Erstellen Ihrer Logik-App eingerichtet haben. Sie haben die Lösung für die Logik-App-Verwaltung bereits installiert.

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Option **Alle Dienste**. Geben Sie in das Suchfeld „log analytics Arbeitsbereiche“ ein, und wählen Sie **Log Analytics-Arbeitsbereiche** aus.

   ![Auswählen von „Log Analytics-Arbeitsbereiche“](./media/logic-apps-monitor-your-logic-apps-oms/find-log-analytics.png)

1. Wählen Sie unter **Log Analytics-Arbeitsbereiche** Ihren Arbeitsbereich aus.

   ![Auswählen Ihres Log Analytics-Arbeitsbereichs](./media/logic-apps-monitor-your-logic-apps-oms/select-log-analytics-workspace.png)

1. Wählen Sie auf der Übersichtsseite unter **Erste Schritte mit Log Analytics** > **Überwachungslösungen konfigurieren** die Option **Lösungen anzeigen** aus.

   ![Auswählen von „Lösungen anzeigen“](media/logic-apps-monitor-your-logic-apps-oms/log-analytics-workspace.png)

1. Wählen Sie unter **Übersicht** die Option **Hinzufügen** aus.

   ![Wählen Sie "Hinzufügen" aus.](./media/logic-apps-monitor-your-logic-apps-oms/add-logic-apps-management-solution.png)

1. Geben Sie, nachdem der **Marketplace** geöffnet wurde, in das Suchfeld „Logic Apps Verwaltung“ ein, und wählen Sie **Logic Apps-Verwaltung** aus.

   ![Auswählen von „Logic Apps-Verwaltung“](./media/logic-apps-monitor-your-logic-apps-oms/select-logic-apps-management.png)

1. Wählen Sie im Bereich mit der Lösungsbeschreibung **Erstellen** aus.

   ![Auswählen von „Erstellen“ für „Logic Apps-Verwaltung“](./media/logic-apps-monitor-your-logic-apps-oms/create-logic-apps-management-solution.png)

1. Überprüfen und bestätigen Sie den Log Analytics-Arbeitsbereich, in dem Sie die Lösung installieren möchten, und wählen Sie erneut **Erstellen** aus.

   ![Auswählen von „Erstellen“ für „Logic Apps-Verwaltung“](./media/logic-apps-monitor-your-logic-apps-oms/confirm-log-analytics-workspace.png)

   Nachdem Azure die Lösung in der Azure-Ressourcengruppe bereitgestellt hat, die den Log Analytics Arbeitsbereich enthält, wird die Lösung im Zusammenfassungsbereich Ihres Arbeitsbereichs angezeigt.

   ![Zusammenfassungsbereich des Arbeitsbereichs](./media/logic-apps-monitor-your-logic-apps-oms/workspace-summary-pane-logic-apps-management.png)

<a name="view-logic-app-runs-oms"></a>

## <a name="view-logic-app-run-information"></a>Anzeigen der Informationen zur Ausführung der Logik-App

Nachdem Ihre Logik-App ausgeführt wird, können Sie den Status und die Anzahl der Ausführungen auf der Kachel **Logic Apps-Verwaltung** anzeigen.

1. Wechseln Sie zu Ihrem Log Analytics-Arbeitsbereich und wählen Sie **Arbeitsbereichszusammenfassung** > **Logic Apps-Verwaltung** aus.

   ![Ausführungsstatus und -anzahl für die Logik-App](media/logic-apps-monitor-your-logic-apps-oms/logic-app-runs-summary.png)

   Hier sind Ihre Logik-App-Ausführungen nach Name oder Ausführungsstatus gruppiert. Auf dieser Seite finden Sie auch Details zu Fehlern in Aktionen oder Triggern für die Logik-App-Ausführungen.

   ![Statuszusammenfassung für Ihre Logik-App-Ausführungen](media/logic-apps-monitor-your-logic-apps-oms/logic-app-runs-summary-details.png)

1. Wählen Sie die Zeile für eine Logik-App oder einen Status aus, um alle Ausführungen für eine bestimmte Logik-App bzw. einen Status anzuzeigen.

   Hier ist ein Beispiel angegeben, bei dem alle Ausführungen für eine bestimmte Logik-App aufgeführt werden:

   ![Anzeigen von Logik-App-Ausführungen und des Status](media/logic-apps-monitor-your-logic-apps-oms/logic-app-run-details.png)

   Diese Seite enthält erweiterte Optionen: 

   * Spalte **Nachverfolgte Eigenschaften**: Für eine Logik-App, für die Sie nachverfolgte Eigenschaften eingerichtet haben, die nach Aktionen gruppiert sind, können Sie diese Eigenschaften in dieser Spalte anzeigen. Wählen Sie zum Anzeigen dieser nachverfolgten Eigenschaften die Option **Anzeigen** aus. Sie können die nachverfolgten Eigenschaften mit dem Spaltenfilter durchsuchen.

      ![Anzeigen der überwachten Eigenschaften für eine Logik-App](media/logic-apps-monitor-your-logic-apps-oms/logic-app-tracked-properties.png)

      Alle neu hinzugefügten überwachten Eigenschaften werden möglicherweise erst nach 10 bis 15 Minuten erstmalig angezeigt. Erfahren Sie mehr über das [Hinzufügen überwachter Eigenschaften zu Ihrer Logik-App](logic-apps-monitor-your-logic-apps.md#azure-diagnostics-event-settings-and-details).

   * **Erneut übermitteln**: Sie können eine oder mehrere Logik-App-Ausführungen, die fehlgeschlagen sind, erfolgreich waren oder noch ausgeführt werden, erneut übermitteln. Aktivieren Sie die Kontrollkästchen für die Ausführungen, die Sie erneut übermitteln möchten, und wählen Sie anschließend **Erneut übermitteln** aus.

     ![Erneutes Übermitteln von Logik-App-Ausführungen](media/logic-apps-monitor-your-logic-apps-oms/logic-app-resubmit.png)

1. Zum Filtern Ihrer Ergebnisse können Sie sowohl die clientseitige als auch die serverseitige Filterung durchführen.

   * **Clientseitiger Filter**: Wählen Sie für jede Spalte die gewünschten Filter aus. Beispiel:

     ![Beispiel für Spaltenfilter](media/logic-apps-monitor-your-logic-apps-oms/filters.png)

   * **Serverseitiger Filter**: Verwenden Sie das Steuerelement für die Bereichsauswahl oben auf der Seite, um ein bestimmtes Zeitfenster auszuwählen oder die Anzahl von angezeigten Ausführungen zu begrenzen. Standardmäßig werden nur jeweils 1.000 Datensätze angezeigt.

     ![Ändern des Zeitfensters](media/logic-apps-monitor-your-logic-apps-oms/change-interval.png)

1. Um alle Aktionen und deren Details für eine bestimmte Ausführung anzuzeigen, wählen Sie die Zeile für eine Logik-App-Ausführung aus.

   Hier ist ein Beispiel mit allen Aktionen und Triggern für eine bestimmte Logik-App-Ausführung dargestellt:

   ![Anzeigen von Aktionen für eine Logik-App-Ausführung](media/logic-apps-monitor-your-logic-apps-oms/logic-app-action-details.png)

1. Um auf einer Ergebnisseite die Abfrage hinter den Ergebnissen oder alle Ergebnisse anzuzeigen, wählen Sie **Alle anzeigen** aus. Hiermit wird die Seite **Protokolle** geöffnet.

   ![Anzeigen aller Ergebnisse](media/logic-apps-monitor-your-logic-apps-oms/logic-app-see-all.png)

   Auf der Seite **Protokolle** können Sie diese Optionen auswählen:

   * Um die Abfrageergebnisse in einer Tabelle anzuzeigen, wählen Sie **Tabelle** aus.

   * Für Abfragen wird die [Kusto-Abfragesprache](https://aka.ms/LogAnalyticsLanguageReference) verwendet. Sie können diese Sprache bearbeiten, wenn Sie unterschiedliche Ergebnisse anzeigen möchten. Aktualisieren Sie zum Ändern der Abfrage die Abfragezeichenfolge, und wählen Sie **Ausführen** aus, um die Ergebnisse in der Tabelle anzuzeigen. 

     ![Log Analytics – Abfrageansicht](media/logic-apps-monitor-your-logic-apps-oms/query.png)

## <a name="next-steps"></a>Nächste Schritte

* [Überwachen von B2B-Nachrichten](../logic-apps/logic-apps-monitor-b2b-message.md)