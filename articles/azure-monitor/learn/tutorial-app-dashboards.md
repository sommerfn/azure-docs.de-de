---
title: Erstellen benutzerdefinierter Dashboards in Azure Application Insights | Microsoft-Dokumentation
description: Tutorial zum Erstellen benutzerdefinierter KPI-Dashboards mithilfe von Azure Application Insights
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: tutorial
author: lgayhardt
ms.author: lagayhar
ms.date: 07/3/2019
ms.custom: mvc
ms.openlocfilehash: 47d8eaff5f154e198c277ec2b63a2d09e66f7180
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2019
ms.locfileid: "72900502"
---
# <a name="create-custom-kpi-dashboards-using-azure-application-insights"></a>Erstellen benutzerdefinierter KPI-Dashboards mithilfe von Azure Application Insights

Sie können mehrere Dashboards im Azure-Portal erstellen, wobei jedes Kacheln enthält, die Daten aus mehreren Azure-Ressourcen in verschiedenen Ressourcengruppen und Abonnements visualisieren.  Außerdem lassen sich verschiedene Diagramme und Ansichten aus Azure Application Insights anheften, um benutzerdefinierte Dashboards zu erstellen, mit denen Sie einen vollständigen Überblick über die Integrität und Leistung der Anwendung erhalten. Dieses Tutorial führt Sie durch die Erstellung eines benutzerdefinierten Dashboards, das mehrere Daten- und Visualisierungstypen aus Azure Application Insights enthält.  Folgendes wird vermittelt:

> [!div class="checklist"]
> * Erstellen eines benutzerdefinierten Dashboards in Azure
> * Hinzufügen einer Kachel aus dem Kachelkatalog
> * Hinzufügen von Standardmetriken in Application Insights zum Dashboard
> * Hinzufügen eines benutzerdefinierten Metrikdiagramms in Application Insights zum Dashboard
> * Hinzufügen der Ergebnisse einer Protokolle (Analytics)-Abfrage zum Dashboard



## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

- Stellen Sie eine .NET-Anwendung in Azure bereit, und [aktivieren Sie das Application Insights SDK](../../azure-monitor/app/asp-net.md).

## <a name="sign-in-to-azure"></a>Anmelden bei Azure
Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="create-a-new-dashboard"></a>Erstellen eines neuen Dashboards
Ein einzelnes Dashboard kann Ressourcen aus mehreren Anwendungen, Ressourcengruppen und Abonnements enthalten.  Starten Sie das Tutorial, indem Sie ein neues Dashboard für Ihre Anwendung erstellen.  

1. Wählen Sie im Bereich „Dashboard“ die Option **Neues Dashboard**

   ![Neues Dashboard](media/tutorial-app-dashboards/1newdashboard.png)

1. Geben Sie einen Namen für das Dashboard ein.
1. Werfen Sie einen Blick auf den **Kachelkatalog**. Dort finden Sie verschiedene Kacheln, die Sie zum Dashboard hinzufügen können.  Sie können aber nicht nur Kacheln aus dem Katalog hinzufügen. Sie haben außerdem die Möglichkeit, Diagramme und andere Ansichten direkt aus Application Insights an das Dashboard anzuheften.
1. Suchen Sie die Kachel **Markdown**, und ziehen Sie sie in Ihr Dashboard.  Diese Kachel ermöglicht es Ihnen, in Markdown formatierten Text hinzuzufügen. Diese Funktion eignet sich ideal, um Ihrem Dashboard beschreibenden Text hinzuzufügen.
1. Fügen Sie den Eigenschaften der Kachel Text hinzu, und passen Sie deren Größe im Dashboardbereich an.
    
    ![Bearbeiten der Markdown-Kachel](media/tutorial-app-dashboards/2dashboard-text.png)

1. Klicken Sie oben auf dem Bildschirm auf **Anpassung abgeschlossen**, um den Bearbeitungsmodus für die Kachel zu beenden.

## <a name="add-health-overview"></a>Hinzufügen der Integritätsübersicht
Ein Dashboard, das statischen Text enthält, ist nicht besonders interessant. Fügen Sie daher eine Kachel aus Application Insights hinzu, um Informationen zu Ihrer Anwendung darzustellen.  Application Insights-Kacheln lassen sich aus dem Kachelkatalog hinzufügen oder direkt von Application Insights-Bildschirmen anheften.  Dadurch können Sie Diagramme und Ansichten konfigurieren, mit denen Sie bereits vertraut sind, bevor Sie diese an Ihr Dashboard anheften.  Fügen Sie zuerst die Standardübersicht über die Integrität Ihrer Anwendung hinzu.  Dies erfordert keine Konfiguration und ermöglicht minimale Anpassungen im Dashboard.


1. Wählen Sie auf dem Startbildschirm Ihre **Application Insights**-Ressource aus.
2. Klicken Sie im Bereich **Übersicht** auf das ![Stecknadelsymbol](media/tutorial-app-dashboards/pushpin.png), um die Kachel zum zuletzt angezeigten Dashboard hinzuzufügen.  
 
3. Rechts oben wird eine Benachrichtigung mit dem Hinweis angezeigt, dass die Kachel an Ihr Dashboard angeheftet wurde. Klicken Sie in der Benachrichtigung auf **An Dashboard angeheftet**, um zum Dashboard zurückzukehren oder den Bereich „Dashboard“ zu verwenden.
4. Diese Kachel wurde Ihrem Dashboard hinzugefügt. Wählen Sie **Bearbeiten**, um die Positionierung der Kachel zu ändern. Klicken Sie darauf, ziehen Sie sie an die gewünschte Position, und klicken Sie dann auf **Anpassung abgeschlossen**. Ihr Dashboard verfügt jetzt über eine Kachel mit hilfreichen Informationen.

    ![Dashboard mit einer Übersichtszeitachse](media/tutorial-app-dashboards/4dashboard-edit.png)

## <a name="add-custom-metric-chart"></a>Hinzufügen eines benutzerdefinierten Metrikdiagramms
Im Bereich **Metriken** können Sie eine Metrik grafisch darstellen, die von Application Insights über einen Zeitraum hinweg mit optionalen Filtern und Gruppierungen erfasst wurde.  Wie alle anderen Elemente in Application Insights auch, können Sie dieses Diagramm dem Dashboard hinzufügen.  Dies erfordert aber kleine Anpassungen.

1. Wählen Sie auf dem Startbildschirm die **Application Insights**-Ressource aus.
1. Klicken Sie auf **Metriken**.  
2. Ein leeres Diagramm wurde bereits erstellt, und Sie werden aufgefordert, eine Metrik hinzuzufügen.  Fügen Sie dem Diagramm eine Metrik hinzu sowie optional einen Filter und eine Gruppierung.  Im folgenden Beispiel wird die Anzahl der Serveranforderungen gruppiert nach Erfolg dargestellt.  Dies bietet einen aktuellen Überblick über erfolgreiche und fehlgeschlagene Anforderungen.

    ![Hinzufügen von Metriken](media/tutorial-app-dashboards/metrics.png)

4. Wählen Sie rechts die Option **An Dashboard anheften** aus. Dadurch wird die Ansicht dem letzten Dashboard hinzugefügt, mit dem Sie gearbeitet haben.

3.  Rechts oben wird eine Benachrichtigung mit dem Hinweis angezeigt, dass die Kachel an Ihr Dashboard angeheftet wurde. Klicken Sie in der Benachrichtigung auf **An Dashboard angeheftet**, um zum Dashboard zurückzukehren oder das Blatt „Dashboard“ zu verwenden.

4. Diese Kachel wurde Ihrem Dashboard hinzugefügt. Wählen Sie **Bearbeiten**, um die Positionierung der Kachel zu ändern. Klicken Sie darauf, ziehen Sie sie an die gewünschte Position, und klicken Sie dann auf **Anpassung abgeschlossen**.

## <a name="add-logs-analytics-query"></a>Exportieren einer Protokolle (Analytics)-Abfrage
Azure Application Insights-Protokolle (Analytics) bietet eine leistungsfähige Abfragesprache, mit der Sie alle von Application Insights erfassten Daten analysieren können. Sie können Ihrem Dashboard die Ausgabe einer Protokollabfrage genauso wie Diagramme und andere Ansichten hinzufügen.

Da es sich bei Azure Applications Insights-Protokolle (Analytics) um einen separaten Dienst handelt, müssen Sie Ihr Dashboard dafür freigeben, damit eine Protokollabfrage eingeschlossen werden kann. Wenn Sie ein Azure-Dashboard freigeben, können Sie es als Azure-Ressource veröffentlichen, wodurch es für andere Benutzer und Ressourcen verfügbar wird.  

1. Klicken Sie dazu oben im Dashboard auf **Freigeben**.

    ![Veröffentlichen des Dashboards](media/tutorial-app-dashboards/8dashboard-share.png)

2. Behalten Sie für **Dashboardname** den ursprünglichen bei, und legen Sie für **Abonnementname** denselben wie für das Dashboard fest.  Klicken Sie auf **Veröffentlichen**.  Das Dashboard ist jetzt für andere Abonnements und Dienste verfügbar.  Sie können optional bestimmte Benutzer definieren, die Zugriff auf das Dashboard haben sollen.
1. Wählen Sie auf dem Startbildschirm die **Application Insights**-Ressource aus.
2. Klicken Sie auf der linken Seite unter „Überwachung“ auf **Protokolle (Analytics)** , um das Portal „Protokolle (Analytics)“ zu öffnen.
3. Geben Sie die folgende Abfrage ein, die die zehn am häufigsten angeforderten Seiten und die Anzahl der Anforderungen zurückgibt:

    ``` Kusto
    requests
    | summarize count() by name
    | sort by count_ desc
    | take 10
    ```

4. Klicken Sie auf **Ausführen**, um die Ergebnisse der Abfrage zu überprüfen.
5. Klicken Sie auf das Stecknadelsymbol, ![Stecknadelsymbol](media/tutorial-app-dashboards/pushpin.png) und wählen Sie den Namen des Dashboards aus. Im Gegensatz zu den vorherigen Schritten, in denen das zuletzt verwendete Dashboard genutzt wurde, müssen Sie für diese Option ein Dashboard auswählen, weil die Protokolle (Analytics)-Konsole ein separater Dienst ist und aus allen verfügbaren, freigegebenen Dashboards eine Auswahl treffen muss.

5. Fügen Sie eine weitere Abfrage hinzu, bevor Sie zum Dashboard zurückkehren. Rendern Sie sie dieses Mal jedoch als Diagramm, um die verschiedenen Möglichkeiten der Visualisierung von Protokollabfragen in einem Dashboard kennenzulernen. Beginnen Sie mit der folgenden Abfrage, die die zehn häufigsten Vorgänge mit den meisten Ausnahmen zusammenfasst.

    ``` Kusto
    exceptions
    | summarize count() by operation_Name
    | sort by count_ desc
    | take 10
    ```

6. Klicken Sie auf **Diagramm**, und ändern Sie es in ein **Ringdiagramm**, um die Ausgabe anzuzeigen.

    ![Diagramm von Protokolle (Analytics)](media/tutorial-app-dashboards/11querychart.png)

6. Klicken Sie auf das Stecknadelsymbol, ![Stecknadelsymbol](media/tutorial-app-dashboards/pushpin.png) um das Diagramm an Ihr Dashboard anzuheften, und klicken Sie dieses Mal auf den Link, um zum Dashboard zurückzukehren.
4. Die Ergebnisse der Abfragen werden Ihrem Dashboard jetzt in dem Format hinzugefügt, das Sie ausgewählt haben.  Klicken Sie auf die einzelnen Ergebnisse, ziehen Sie sie an die gewünschte Position, und klicken Sie dann auf **Anpassung abgeschlossen**.
5. Klicken Sie auf das Stiftsymbol ![Stiftsymbol](media/tutorial-app-dashboards/pencil.png) jedes Titels, um einen aussagekräftigen Titel einzugeben.

5. Wählen Sie **Freigeben**, um die Änderungen an Ihrem Dashboard erneut zu veröffentlichen, das jetzt eine Vielzahl von Diagrammen und Visualisierungen aus Application Insights enthält.


## <a name="next-steps"></a>Nächste Schritte
Da Sie jetzt wissen, wie Sie benutzerdefinierte Dashboards erstellen, können Sie einen Blick auf die übrige Application Insights-Dokumentation einschließlich einer Fallstudie werfen.

> [!div class="nextstepaction"]
> [Eingehende Diagnose](../../azure-monitor/app/devops.md)
