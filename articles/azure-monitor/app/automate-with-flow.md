---
title: Automatisieren von Azure Application Insights-Prozessen mit Microsoft Flow
description: In diesem Artikel erfahren Sie, wie Sie Microsoft Flow zur schnellen Automatisierung von wiederholbaren Prozessen mit dem Application Insights-Connector verwenden.
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: mrbullwinkle
ms.author: mbullwin
ms.date: 08/29/2019
ms.openlocfilehash: ff0896498c0270b8eb43b762228916985f924def
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72678321"
---
# <a name="automate-azure-application-insights-processes-with-the-connector-for-microsoft-flow"></a>Automatisieren von Azure Application Insights-Prozessen mit dem Connector für Microsoft Flow

Ertappen Sie sich dabei, wie Sie wiederholt dieselben Abfragen von Ihren Telemetriedaten ausführen, um zu überprüfen, ob Ihr Dienst ordnungsgemäß ausgeführt wird? Möchten Sie diese Abfragen für die Suche nach Trends und Anomalien automatisieren und eigene Workflows dafür erstellen? Dann benötigen Sie den Azure Application Insights-Connector für Microsoft Flow.

Mit dieser Integration können Sie nun zahlreiche Prozesse automatisieren, ohne eine einzige Codezeile zu schreiben. Nachdem Sie einen Flow mit einer Application Insights-Aktion erstellt haben, wird Ihre Application Insights Analytics-Abfrage automatisch ausgeführt.

Sie können auch weitere Aktionen hinzufügen. Mit Microsoft Flow stehen Ihnen Hunderte von Aktionen zur Verfügung. So können Sie mit Microsoft Flow beispielsweise automatisch eine E-Mail-Benachrichtigung senden oder einen Fehler in Azure DevOps protokollieren. Außerdem können Sie eine der zahlreichen [Vorlagen](https://ms.flow.microsoft.com/connectors/shared_applicationinsights/?slug=azure-application-insights) verwenden, die für den Connector für Microsoft Flow zur Verfügung stehen. Diese Vorlagen beschleunigen den Vorgang zum Erstellen eines Flow.

<!--The Application Insights connector also works with [Azure Power Apps](https://powerapps.microsoft.com/) and [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/?v=17.23h). -->

## <a name="create-a-flow-for-application-insights"></a>Erstellen eines Flows für Application Insights

In diesem Tutorial erfahren Sie, wie Sie einen Flow erstellen, der zum Gruppieren von Attributen in den Daten für eine Webanwendung den automatischen Clusteralgorithmus von Analytics verwendet. Die Ergebnisse werden automatisch per E-Mail gesendet. Dies ist nur eins von vielen Beispielen für die gemeinsame Verwendung von Microsoft Flow und Application Insights Analytics.

### <a name="step-1-create-a-flow"></a>Schritt 1: Erstellen eines Datenflusses

1. Melden Sie sich bei [Microsoft Flow](https://flow.microsoft.com) an, und wählen Sie **Meine Flows** aus.
2. Klicken Sie auf **Neu** und dann auf **Geplant – ohne Vorlage**.

    ![Erstellen eines neuen geplanten Flows ohne Vorlage](./media/automate-with-flow/1-create.png)

### <a name="step-2-create-a-trigger-for-your-flow"></a>Schritt 2: Erstellen eines Triggers für Ihren Flow

1. Geben Sie im Popup **Geplanten Flow erstellen** den Namen Ihres Flows und die Häufigkeit der Ausführung ein.

    ![Wiederholung des Zeitplans durch Eingabe der Häufigkeit und des Intervalls einrichten](./media/automate-with-flow/2-schedule.png)

1. Klicken Sie auf **Create**.

### <a name="step-3-add-an-application-insights-action"></a>Schritt 3: Hinzufügen einer Application Insights-Aktion

1. Suchen Sie nach **Application Insights**.
2. Klicken Sie auf **Azure Application Insights – Analyseabfrage visualisieren**.

    ![Aktion auswählen: Azure Application Insights – Analyseabfrage visualisieren](./media/automate-with-flow/3-visualize.png)

3. Wählen Sie **Neuer Schritt** aus.

### <a name="step-4-connect-to-an-application-insights-resource"></a>Schritt 4: Herstellen einer Verbindung mit einer Application Insights-Ressource

Für diesen Schritt benötigen Sie eine Anwendungs-ID und einen API-Schlüssel für Ihre Ressource. Diese können Sie über das Azure-Portal abrufen, wie in der folgenden Abbildung zu sehen:

![Anwendungs-ID im Azure-Portal](./media/automate-with-flow/5apiaccess.png)

![API-Schlüssel im Azure-Portal](./media/automate-with-flow/6apikey.png)

Geben Sie einen Namen für Ihre Verbindung sowie die Anwendungs-ID und den API-Schlüssel ein.

   ![Fenster für die Microsoft Flow-Verbindung](./media/automate-with-flow/4-connection.png)

Wenn das Feld für die Verbindung nicht sofort angezeigt wird und stattdessen direkt mit der Eingabe der Abfrage begonnen wird, sollten Sie oben rechts im Feld auf das Auslassungszeichen klicken. Wählen Sie dann „Meine Verbindungen“ aus, oder verwenden Sie eine vorhandene Verbindung.

Klicken Sie auf **Create**.

### <a name="step-5-specify-the-analytics-query-and-chart-type"></a>Schritt 5: Angeben der Analytics-Abfrage und des Diagrammtyps
Diese Beispielabfrage wählt die nicht erfolgreichen Anforderungen des letzten Tages aus und gleicht sie mit Ausnahmen ab, die im Zusammenhang mit dem Vorgang aufgetreten sind. Analytics führt den Abgleich auf der Grundlage des Bezeichners „operation_Id“ durch. Die Ergebnisse werden anschließend mithilfe des automatischen Clusteralgorithmus segmentiert.

Vergewissern Sie sich beim Erstellen eigener Abfragen, dass sie ordnungsgemäß in Analytics funktionieren, bevor Sie sie zu Ihrem Flow hinzufügen.

- Fügen Sie die folgenden Analytics-Abfrage hinzu, und wählen Sie als Diagrammtyp „HTML-Tabelle“. Wählen Sie dann **Neuer Schritt** aus.

    ```
    requests
    | where timestamp > ago(1d)
    | where success == "False"
    | project name, operation_Id
    | join ( exceptions
        | project problemId, outerMessage, operation_Id
    ) on operation_Id
    | evaluate autocluster()
    ```
    
    ![Konfigurationsfenster für Analytics-Abfragen](./media/automate-with-flow/5-query.png)

### <a name="step-6-configure-the-flow-to-send-email"></a>Schritt 6: Konfigurieren des Flows zum Senden von E-Mails

1. Suchen Sie nach **Office 365 Outlook**.
2. Klicken Sie auf **Office 365 Outlook – E-Mail senden**.

    ![Fenster zum Auswählen von Office 365 Outlook](./media/automate-with-flow/6-outlook.png)

1. Gehen Sie im Fenster **E-Mail senden** wie folgt vor:

   a. Geben Sie die E-Mail-Adresse des Empfängers ein.

   b. Geben Sie einen Betreff für die E-Mail ein.

   c. Klicken Sie auf das Feld **Text**, und wählen Sie im dynamischen Inhaltsmenü, das auf der rechten Seite geöffnet wird, das Feld **Text** aus.

   e. Wählen Sie **Erweiterte Optionen anzeigen** aus.

1. Im Menü „Dynamischer Inhalt“:

    a. Wählen Sie **Anlagenname** aus.

    b. Wählen Sie **Anlageninhalt** aus.
    
    c. Wählen Sie im Feld **Ist HTML** die Option **Ja** aus.

    ![Konfiguration von Office 365 Outlook](./media/automate-with-flow/7-email.png)

### <a name="step-7-save-and-test-your-flow"></a>Schritt 7: Speichern und Testen Ihres Flows

Klicken Sie auf **Speichern**.

Sie können warten, bis der Trigger diese Aktion ausführt, oder Sie können oben auf ![Becherglassymbol](./media/automate-with-flow/testicon.png) **Testen** klicken.

Nach Auswahl von **Testen**:

1. Wählen Sie **Ich führe die Triggeraktion durch** aus.
2. Wählen Sie **Flow ausführen** aus.

Wenn der Flow ausgeführt wird, erhalten die Empfänger, die Sie in der E-Mail-Liste angegeben haben, eine E-Mail wie unten angegeben.

![Beispiel-E-Mail](./media/automate-with-flow/flow9.png)

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die Erstellung von [Analytics-Abfragen](../../azure-monitor/log-query/get-started-queries.md).
- Erfahren Sie mehr über [Microsoft Flow](https://ms.flow.microsoft.com).

<!--Link references-->