---
title: Erstellen von zeitplanbasierten automatisierten Workflows – Azure Logic Apps | Microsoft-Dokumentation
description: 'Tutorial: Erstellen eines zeitplanbasierten, periodischen automatisierten Workflows mit Azure Logic Apps'
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.manager: carmonm
ms.reviewer: klam, LADocs
ms.topic: tutorial
ms.custom: mvc
ms.date: 01/12/2018
ms.openlocfilehash: ec29eef7e733155b205d4feda844883bbc4496c9
ms.sourcegitcommit: bba811bd615077dc0610c7435e4513b184fbed19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2019
ms.locfileid: "70051748"
---
# <a name="check-traffic-on-a-schedule-with-azure-logic-apps"></a>Überprüfen der Verkehrslage gemäß eines Zeitplans mit Azure Logic Apps

Azure Logic Apps unterstützt Sie beim Automatisieren von Workflows, die gemäß einem Zeitplan ausgeführt werden. In diesem Tutorial wird gezeigt, wie Sie eine [Logik-App](../logic-apps/logic-apps-overview.md) mit einem Planer-Trigger erstellen, die morgens an Wochentagen ausgeführt wird und die Reisezeit (einschließlich Verkehrslage) zwischen zwei Orten überprüft. Wenn die Reisezeit einen bestimmten Grenzwert überschreitet, sendet die Logik-App eine E-Mail mit der Reisezeit und der zusätzlich bis zum Ziel benötigten Zeit.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer leeren Logik-App 
> * Hinzufügen eines Triggers, der als Planer für Ihre Logik-App fungiert
> * Hinzufügen einer Aktion, die die Reisezeit für eine Route abruft
> * Hinzufügen einer Aktion, die eine Variable erstellt, die die Reisezeit von Sekunden in Minuten umwandelt und dieses Ergebnis in der Variablen speichert.
> * Hinzufügen einer Bedingung, die die Reisezeit mit einem bestimmten Grenzwert vergleicht
> * Hinzufügen einer Aktion, die eine E-Mail sendet, wenn die Reisezeit den Grenzwert überschreitet

Am Ende entspricht Ihre Logik-App grob dem folgenden Workflow:

![Grundlegende Logik-App](./media/tutorial-build-scheduled-recurring-logic-app-workflow/check-travel-time-overview.png)

Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/), bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

* Ein E-Mail-Konto eines von Logic Apps unterstützten E-Mail-Anbieters wie etwa Office 365 Outlook, Outlook.com oder Gmail. Informationen zu Connectors für andere Anbieter finden Sie in [dieser Liste](https://docs.microsoft.com/connectors/). In dieser Schnellstartanleitung wird ein Outlook.com-Konto verwendet. Bei Verwendung eines anderen E-Mail-Kontos bleiben die allgemeinen Schritte zwar gleich, die Benutzeroberfläche sieht aber unter Umständen etwas anders aus.

* Um die Reisezeit für eine Route zu ermitteln, benötigen Sie einen Zugriffsschlüssel für die Bing Maps-API. Führen Sie zum Abrufen dieses Schlüssels die Schritte zum [Abrufen eines Bing Maps-Schlüssels](https://msdn.microsoft.com/library/ff428642.aspx) aus. 

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich mit den Anmeldeinformationen Ihres Azure-Kontos beim [Azure-Portal](https://portal.azure.com) an.

## <a name="create-your-logic-app"></a>Erstellen Ihrer Logik-App

1. Klicken Sie im Hauptmenü von Azure auf **Ressource erstellen** > **Unternehmensintegration** > **Logik-App**.

   ![Erstellen einer Logik-App](./media/tutorial-build-scheduled-recurring-logic-app-workflow/create-logic-app.png)

2. Geben Sie unter **Logik-App erstellen** die Informationen wie beschrieben ein. Wenn Sie fertig sind, wählen Sie **An Dashboard anheften** > **Erstellen** aus.

   ![Angeben von Informationen zur Logik-App](./media/tutorial-build-scheduled-recurring-logic-app-workflow/create-logic-app-settings.png)

   | Einstellung | Wert | BESCHREIBUNG | 
   | ------- | ----- | ----------- | 
   | **Name** | LA-TravelTime | Der Name Ihrer Logik-App | 
   | **Abonnement** | <*Name Ihres Azure Abonnements*> | Der Name Ihres Azure-Abonnements | 
   | **Ressourcengruppe** | LA-TravelTime-RG | Der Name der [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md), die zum Organisieren verwandter Ressourcen verwendet wird. | 
   | **Location** | USA (Ost) 2 | Die Region, in der die Informationen zu Ihrer Logik-App gespeichert werden sollen. | 
   | **Log Analytics** | Aus | Behalten Sie die Einstellung **Aus** für die Diagnoseprotokollierung bei. | 
   |||| 

3. Nachdem Ihre App von Azure bereitgestellt wurde, wird der Designer für Logik-Apps geöffnet und eine Seite mit einem Einführungsvideo und Vorlagen für allgemeine Logik-App-Mustern angezeigt. Wählen Sie unter **Vorlagen** die Option **Leere Logik-App**.

   ![Auswählen der Vorlage „Leere Logik-App“](./media/tutorial-build-scheduled-recurring-logic-app-workflow/choose-logic-app-template.png)

Fügen Sie als Nächstes den Wiederholungs-[Trigger](../logic-apps/logic-apps-overview.md#logic-app-concepts) hinzu, der basierend auf einem angegebenen Zeitplan ausgelöst wird. Jede Logik-App muss mit einem Trigger beginnen, der ausgelöst wird, wenn ein bestimmtes Ereignis eintritt oder wenn neue Daten eine bestimmte Bedingung erfüllen. Weitere Informationen finden Sie unter [Erstellen Ihrer ersten Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="add-scheduler-trigger"></a>Hinzufügen eines Planer-Triggers

1. Geben Sie im Designer „Wiederholung“ in das Suchfeld ein. Wählen Sie den folgenden Trigger aus: **Zeitplan: Wiederholung**

   ![Suchen und Hinzufügen des Triggers „Zeitplan: Wiederholung“](./media/tutorial-build-scheduled-recurring-logic-app-workflow/add-schedule-recurrence-trigger.png)

2. Klicken Sie im Bereich **Wiederholung** auf die Schaltfläche **...** (**Ellipse**) und anschließend auf **Umbenennen**. Nennen Sie den Trigger wie folgt: ```Check travel time every weekday morning```

   ![Umbenennen des Triggers](./media/tutorial-build-scheduled-recurring-logic-app-workflow/rename-recurrence-schedule-trigger.png)

3. Wählen Sie im Trigger **Erweiterte Optionen anzeigen** aus.

4. Geben Sie die Zeitplan- und Wiederholungsdetails für den Trigger wie gezeigt und beschrieben ein:

   ![Eingeben der Zeitplan- und Wiederholungsdetails](./media/tutorial-build-scheduled-recurring-logic-app-workflow/schedule-recurrence-trigger-settings.png)

   | Einstellung | Wert | BESCHREIBUNG | 
   | ------- | ----- | ----------- | 
   | **Intervall** | 1 | Die Anzahl von Warteintervallen zwischen Überprüfungen | 
   | **Frequency** | Woche | Die Zeiteinheit für die Wiederholung | 
   | **Zeitzone** | Keine | Gilt nur, wenn Sie eine Startzeit angegeben haben. Diese Option ist nützlich zum Angeben einer nicht lokalen Zeitzone. | 
   | **Startzeit** | Keine | Verzögern Sie die Wiederholung bis zu einem bestimmten Zeitpunkt (Datum und Uhrzeit). Weitere Informationen finden Sie unter [Planen von regelmäßig ausgeführten Aufgaben und Workflows mit Logik-Apps](../connectors/connectors-native-recurrence.md). | 
   | **An diesen Tagen** | Montag,Dienstag,Mittwoch,Donnerstag,Freitag | Nur verfügbar, wenn für **Häufigkeit** die Option „Woche“ festgelegt ist. | 
   | **Zu diesen Stunden** | 7,8,9 | Nur verfügbar, wenn für **Häufigkeit** die Option „Woche“ oder „Tag“ festgelegt ist. Wählen Sie für die Ausführung der Wiederholung die Stunden des Tages aus. Dieses Beispiel wird um 7, 8 und 9 Uhr ausgeführt. | 
   | **Zu diesen Minuten** | 0,15,30,45 | Nur verfügbar, wenn für **Häufigkeit** die Option „Woche“ oder „Tag“ festgelegt ist. Wählen Sie für die Ausführung der Wiederholung die Minuten des Tages aus. Dieses Beispiel wird ab 0 Uhr alle 15 Minuten ausgeführt. | 
   ||||

   Dieser Trigger wird an jedem Wochentag alle 15 Minuten ausgelöst. Die Auslösung beginnt um 7:00 Uhr und endet um 9:45 Uhr. 
   Im Feld **Vorschau** wird der Wiederholungszeitplan angezeigt. 
   Weitere Informationen finden Sie unter [Planen von regelmäßig ausgeführten Aufgaben und Workflows mit Logik-Apps](../connectors/connectors-native-recurrence.md) und [Trigger und Aktionen für Logik-App-Workflows](../logic-apps/logic-apps-workflow-actions-triggers.md#recurrence-trigger).

5. Klicken Sie auf die Titelleiste des Bereichs, um die Triggerdetails vorerst auszublenden.

   ![Ausblenden der Details durch Reduzieren des Bereichs](./media/tutorial-build-scheduled-recurring-logic-app-workflow/collapse-trigger-shape.png)

6. Speichern Sie Ihre Logik-App. Wählen Sie auf der Symbolleiste des Designers **Speichern**. 

Ihre Logik-App befindet sich jetzt im Livemodus, wird aber vorerst nur periodisch ausgeführt. Fügen Sie daher eine Aktion hinzu, die reagiert, wenn der Trigger ausgelöst wird.

## <a name="get-the-travel-time-for-a-route"></a>Abrufen der Reisezeit für eine Route

Sie verfügen über einen Trigger und können nun eine [Aktion](../logic-apps/logic-apps-overview.md#logic-app-concepts) hinzufügen, die die Reisezeit zwischen zwei Orten ermittelt. Logik-Apps stellt einen Connector für die Bing Maps-API bereit, sodass Sie diese Informationen problemlos abrufen können. Stellen Sie vor dem Starten dieser Aufgabe sicher, dass Sie einen Bing Maps-API-Schlüssel besitzen, wie in den Voraussetzungen dieses Tutorials beschrieben.

1. Klicken Sie im Designer für Logik-Apps unter dem Trigger auf **+ Neuer Schritt** > **Aktion hinzufügen**.

2. Suchen Sie nach „maps“, und wählen Sie diese Aktion aus: **Bing Maps - Get route** (Bing Maps – Route ermitteln)

3. Falls keine Verbindung mit Bing Maps besteht, werden Sie aufgefordert, eine Verbindung herzustellen. Geben Sie die entsprechenden Verbindungsdetails an, und klicken Sie auf **Erstellen**.

   ![Auswählen der Aktion „Bing Maps - Get route“ (Bing Maps – Route ermitteln)](./media/tutorial-build-scheduled-recurring-logic-app-workflow/create-maps-connection.png)

   | Einstellung | Wert | BESCHREIBUNG |
   | ------- | ----- | ----------- |
   | **Verbindungsname** | BingMapsConnection | Geben Sie einen Namen für die Verbindung an. | 
   | **API-Schlüssel** | <*Ihr Bing Maps-Schlüssel*> | Geben Sie den Bing Maps-Schlüssel ein, den Sie zuvor abgerufen haben. Falls Sie keinen Bing Maps-Schlüssel besitzen, lesen Sie die Informationen zum [Abrufen eines Schlüssels](https://msdn.microsoft.com/library/ff428642.aspx). | 
   | | | |  

4. Nennen Sie die Aktion wie folgt: ```Get route and travel time with traffic```

5. Geben Sie wie hier gezeigt und beschrieben Details für die Aktion **Get route** (Route ermitteln) an. Beispiel:

   ![Angeben von Informationen für die Aktion „Bing Maps - Get route“ (Bing Maps – Route ermitteln)](./media/tutorial-build-scheduled-recurring-logic-app-workflow/get-route-action-settings.png) 

   | Einstellung | Wert | BESCHREIBUNG |
   | ------- | ----- | ----------- |
   | **Wegpunkt 1** | <*Start*> | Ausgangspunkt für die Route | 
   | **Wegpunkt 2** | <*Ziel*> | Ziel der Route | 
   | **Vermeiden** | Keine | Elemente, die auf der Route vermieden werden sollen, etwa Autobahnen, Mautgebühren usw. | 
   | **Optimieren** | timeWithTraffic | Ein Parameter zur Optimierung der Route, z.B. Entfernung, Reisezeit basierend auf der aktuellen Verkehrslage, usw. Wählen Sie den Parameter „timeWithTraffic“. | 
   | **Distance unit** (Einheit für Entfernung) | <*Ihre Präferenz*> | Die Einheit der Entfernung für die Route. In diesem Artikel wird die folgende Einheit verwendet: „Meile“  | 
   | **Travel mode** (Reisemodus) | Driving (Auto) | Der Reisemodus für die Route. Wählen Sie den folgenden Modus aus: Driving (Auto) | 
   | **Transit Date-Time** (Datum und Uhrzeit für Transit) | Keine | Gilt für nur für den Transitmodus. | 
   | **Date-Time Type** (Datums-/Uhrzeittyp) | Keine | Gilt für nur für den Transitmodus. | 
   |||| 

   Weitere Informationen zu diesen Parametern finden Sie unter [Calculate a route](https://msdn.microsoft.com/library/ff701717.aspx) (Berechnen einer Route).

6. Speichern Sie Ihre Logik-App.

Erstellen Sie als Nächstes eine Variable, sodass Sie die aktuelle Reisezeit von Sekunden in Minuten umwandeln und speichern können. So müssen Sie die Umwandlung nicht wiederholen und können den Wert in späteren Schritten einfacher verwenden. 

## <a name="create-variable-to-store-travel-time"></a>Erstellen einer Variablen zum Speichern der Reisezeit

In manchen Fällen empfiehlt es sich, Vorgänge für Daten in Ihrem Workflow durchzuführen und die Ergebnisse in nachfolgenden Aktionen zu verwenden. Wenn Sie Ergebnisse speichern möchten, um sie einfach wiederverwenden oder einfach darauf verweisen zu können, erstellen Sie Variablen, um diese Ergebnisse nach ihrer Verarbeitung zu speichern. Sie können Variablen nur auf der obersten Ebene in der Logik-App erstellen.

Die vorherige Aktion **Get route** (Route abrufen) gibt im Feld **Travel Duration Traffic** (Reisedauer (Verkehr)) standardmäßig die aktuelle Reisezeit in Sekunden zurück. Wenn Sie diesen Wert stattdessen in Minuten umwandeln und speichern, kann er später ohne erneute Umwandlung einfacher verwendet werden.

1. Klicken Sie unter der Aktion **Get route** (Route ermitteln) auf **+ Neuer Schritt** > **Aktion hinzufügen**.

2. Suchen Sie nach „Variablen“, und wählen Sie diese Aktion aus: **Variablen – Variable initialisieren**

   ![Auswählen der Aktion „Variablen – Variable initialisieren“](./media/tutorial-build-scheduled-recurring-logic-app-workflow/select-initialize-variable-action.png)

3. Nennen Sie diese Aktion wie folgt: ```Create variable to store travel time```

4. Geben Sie die Details für die Variable an wie hier beschrieben:

   | Einstellung | Wert | BESCHREIBUNG | 
   | ------- | ----- | ----------- | 
   | **Name** | travelTime | Der Name für die Variable | 
   | **Typ** | Integer | Der Datentyp für die Variable | 
   | **Wert** | Ein Ausdruck, der die aktuelle Reisezeit von Sekunden in Minuten umwandelt. (Sehen Sie sich die Schritte unterhalb dieser Tabelle an.) | Der Anfangswert für die Variable | 
   |||| 

   1. Klicken Sie zum Erstellen des Ausdrucks für das Feld **Wert** in das Feld, sodass die Liste mit den dynamischen Inhalten angezeigt wird. 
   Verbreitern Sie ggf. das Browserfenster, bis die Liste angezeigt wird. 
   Wenn die Liste mit den dynamischen Inhalten erscheint, wählen Sie **Ausdruck** aus. 

      ![Angeben von Informationen für die Aktion „Variablen – Variable Initialisieren“](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings.png)

      Wenn Sie in einige Bearbeitungsfelder klicken, wird entweder eine Liste mit den dynamischen Inhalten oder eine Inlineparameterliste angezeigt. Diese Liste zeigt alle Parameter von vorherigen Aktionen, die als Eingabe in Ihrem Workflow verwendet werden können. 
      Die Liste mit den dynamischen Inhalten enthält einen Ausdrucks-Editor, in dem Sie Funktionen zum Ausführen von Vorgängen auswählen können. 
      Dieser Ausdrucks-Editor wird nur in der Liste mit den dynamischen Inhalten angezeigt.

      Die Breite des Browserfensters bestimmt, welche Liste angezeigt wird. 
      Ist das Browserfenster breit, wird die Liste mit den dynamischen Inhalten angezeigt. 
      Ist das Browserfenster schmal, wird eine Parameterliste inline unterhalb des Bearbeitungsfelds angezeigt, das gerade fokussiert ist.

   2. Geben Sie im Ausdrucks-Editor den folgenden Ausdruck ein: ```div(,60)```.

      ![Eingeben des Ausdrucks „div(,60)“](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings-2.png)

   3. Positionieren Sie den Cursor innerhalb des Ausdrucks zwischen der linken Klammer ( **(** ) und dem Komma ( **,** ). 
   Klicken Sie auf **Dynamischer Inhalt**.

      ![Positionieren des Cursors, Auswählen von „Dynamischer Inhalt“](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings-3.png)

   4. Klicken Sie in der Liste mit den dynamischen Inhalten auf **Travel Duration Traffic** (Reisedauer (Verkehr)).

      ![Klicken auf das Feld „Travel Duration Traffic“ (Reisedauer (Verkehr))](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings-4.png)

   5. Nachdem das Feld im Ausdruck aufgelöst wurde, klicken Sie auf **OK**.

      ![Klicken auf „OK“](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings-5.png)

      Das Feld **Wert** erscheint nun wie hier gezeigt:

      ![Feld „Wert“ mit aufgelöstem Ausdruck](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings-6.png)

5. Speichern Sie Ihre Logik-App.

Fügen Sie als Nächstes eine Bedingung hinzu, die überprüft, ob die aktuelle Reisezeit größer als ein bestimmter Grenzwert ist.

## <a name="compare-travel-time-with-limit"></a>Vergleichen der Reisezeit mit einem Grenzwert

1. Klicken Sie unter der vorherigen Aktion auf **+ Neuer Schritt** > **Bedingung hinzufügen**. 

2. Nennen Sie die Bedingung wie folgt: ```If travel time exceeds limit```

3. Erstellen Sie wie hier beschrieben und gezeigt eine Bedingung, die überprüft, ob **travelTime** den von Ihnen angegebenen Grenzwert überschreitet:

   1. Klicken Sie in der Bedingung auf das Feld **Wert auswählen**. Dieses Feld befindet sich entweder links (breite Browseransicht) oder oben (schmale Browseransicht).

   2. Klicken Sie in der Liste mit den dynamischen Inhalten oder in der Parameterliste unter **Variablen** auf das Feld **travelTime**.

   3. Wählen Sie im Vergleichsfeld den folgenden Operator: **ist größer als**.

   4. Geben Sie rechts (breite Ansicht) oder unten (schmale Ansicht) im Feld **Wert auswählen** den folgenden Grenzwert ein: ```15```.

   Wenn Sie in der schmalen Ansicht arbeiten, erstellen Sie diese Bedingung beispielsweise wie folgt:

   ![Erstellen einer Bedingung in der schmalen Ansicht](./media/tutorial-build-scheduled-recurring-logic-app-workflow/build-condition-check-travel-time-narrow.png)

4. Speichern Sie Ihre Logik-App.

Fügen Sie anschließend die Aktion hinzu, die ausgeführt werden soll, wenn die Reisezeit den Grenzwert überschreitet.

## <a name="send-email-when-limit-exceeded"></a>Senden einer E-Mail bei Überschreiten des Grenzwerts

Fügen Sie nun eine Aktion hinzu, die Ihnen eine E-Mail sendet, wenn die Reisezeit Ihren Grenzwert überschreitet. Diese E-Mail enthält die aktuelle Reisezeit sowie die zusätzliche Zeit, die für die angegebene Route erforderlich ist. 

1. Klicken Sie in der Bedingungsverzweigung **Bei TRUE** auf **Aktion hinzufügen**.

2. Suchen Sie nach „E-Mail senden“, und wählen Sie dann den E-Mail-Connector und die zu verwendende Aktion zum Senden einer E-Mail aus.

   ![Suchen und Auswählen der Aktion „E-Mail senden“](./media/tutorial-build-scheduled-recurring-logic-app-workflow/add-action-send-email.png)

   * Wählen Sie für persönliche Microsoft-Konten die Option **Outlook.com**. 
   * Wählen Sie für Geschäfts-, Schul- oder Unikonten für Azure die Option **Office 365 Outlook**.

3. Besteht noch keine Verbindung, werden Sie zur Anmeldung bei Ihrem E-Mail-Konto aufgefordert.

   Logic Apps erstellt eine Verbindung mit Ihrem E-Mail-Konto.

4. Nennen Sie die Aktion wie folgt: ```Send email with travel time```

5. Geben Sie im Feld **An** die E-Mail-Adresse des Empfängers ein. Geben Sie zu Testzwecken Ihre E-Mail-Adresse an.

6. Geben Sie im Feld **Betreff** den Betreff der E-Mail sowie die Variable **travelTime** ein.

   1. Geben Sie den Text ```Current travel time (minutes):``` mit einem nachstehenden Leerzeichen ein. 
   
   2. Klicken Sie in der Parameterliste oder in der Liste mit den dynamischen Inhalten unter **Variablen** auf **travelTime**. 
   
      Beispiel für die schmale Browseransicht:

      ![Eingeben des Betrefftexts und des Ausdrucks, der die Reisezeit zurückgibt](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-subject-settings.png)

7. Geben Sie im Feld **Text** den Inhalt für den E-Mail-Text ein. 

   1. Geben Sie den Text ```Add extra travel time (minutes):``` mit einem nachstehenden Leerzeichen ein. 
   
   2. Verbreitern Sie ggf. das Browserfenster, bis die Liste mit den dynamischen Inhalten angezeigt wird. 
   Wenn die Liste mit den dynamischen Inhalten erscheint, wählen Sie **Ausdruck** aus.

      ![Erstellen eines Ausdrucks für den E-Mail-Text](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings.png)

   3. Geben Sie im Ausdrucks-Editor diesen Ausdruck ein, sodass Sie die Anzahl von Minuten berechnen können, um die der Grenzwert überschritten wird: ```sub(,15)```

      ![Eingeben eines Ausdrucks zum Berechnen der zusätzlichen Reisezeit](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings-2.png)

   4. Positionieren Sie den Cursor innerhalb des Ausdrucks zwischen der linken Klammer ( **(** ) und dem Komma ( **,** ). Klicken Sie auf **Dynamischer Inhalt**.

      ![Fortsetzen der Erstellung eines Ausdrucks zum Berechnen der zusätzlichen Reisezeit](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings-3.png)

   5. Klicken Sie unter **Variablen** auf **travelTime**.

      ![Klicken auf das Feld „travelTime“, um es im Ausdruck zu verwenden](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings-4.png)

   6. Nachdem das Feld im Ausdruck aufgelöst wurde, klicken Sie auf **OK**.

      ![Feld „Text“ mit aufgelöstem Ausdruck](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings-5.png)

      Das Feld **Text** erscheint nun wie hier gezeigt:

      ![Feld „Text“ mit aufgelöstem Ausdruck](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings-6.png)

8. Speichern Sie Ihre Logik-App.

Testen Sie als Nächstes Ihre Logik-App, die nun in etwa wie folgt aussieht:

![Fertiggestellte Logik-App](./media/tutorial-build-scheduled-recurring-logic-app-workflow/check-travel-time-finished.png)

## <a name="run-your-logic-app"></a>Ausführen Ihrer Logik-App

Wählen Sie in der Symbolleiste des Designers die Option **Ausführen**, um Ihre Logik-App manuell zu starten. Wenn die aktuelle Reisezeit den Grenzwert nicht überschreitet, führ Ihre Logik-App keine weitere Aktion aus und wartet vor der erneuten Überprüfung auf das nächste Intervall.
Wenn die aktuelle Reisezeit jedoch den Grenzwert überschreitet, erhalten Sie eine E-Mail mit der aktuellen Reisezeit und der Anzahl von Minuten, um die der Grenzwert überschritten wird. Hier sehen Sie ein Beispiel für eine von Logik-App gesendete E-Mail:

![Gesendete E-Mail mit Reisezeit](./media/tutorial-build-scheduled-recurring-logic-app-workflow/email-notification.png)

Überprüfen Sie Ihren Ordner mit den Junk-E-Mails, falls Sie keine E-Mails erhalten. E-Mails dieser Art werden unter Umständen durch Ihren Junk-E-Mail-Filter umgeleitet. Wenn Sie unsicher sind, ob Ihre Logik-App richtig ausgeführt wurde, helfen Ihnen die Informationen unter [Diagnostizieren von Fehlern bei Logik-Apps](../logic-apps/logic-apps-diagnosing-failures.md) weiter.

Glückwunsch! Sie haben eine zeitplanbasierte Logik-App für die wiederholte Ausführung erstellt und ausgeführt. 

Wenn Sie weitere Logik-Apps erstellen möchten, die den Trigger **Zeitplan: Wiederholung** verwenden, sehen Sie sich die folgenden Vorlagen an. Diese sind nach der Erstellung einer Logik-App verfügbar:

* Tägliche Erinnerungen erhalten
* Ältere Azure-Blobs löschen
* Nachricht zu einer Azure Storage-Warteschlange hinzufügen

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie sie nicht mehr benötigen, löschen Sie die Ressourcengruppe mit Ihrer Logik-App und den dazugehörigen Ressourcen. Wechseln Sie im Azure-Hauptmenü zu **Ressourcengruppen**, und wählen Sie die Ressourcengruppe für Ihre Logik-App aus. Klicken Sie auf **Ressourcengruppe löschen**. Geben Sie zur Bestätigung den Ressourcengruppennamen ein, und klicken Sie auf **Löschen**.

![„Übersicht“ > „Ressourcengruppe löschen“](./media/tutorial-build-scheduled-recurring-logic-app-workflow/delete-resource-group.png)

## <a name="get-support"></a>Support

* Sollten Sie Fragen haben, besuchen Sie das [Azure Logic Apps-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Wenn Sie Features vorschlagen oder für Vorschläge abstimmen möchten, besuchen Sie die [Website für Logic Apps-Benutzerfeedback](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine Logik-App erstellt, die die Verkehrslage gemäß einem festgelegten Zeitplan (morgens an Wochentagen) überprüft und eine Aktion ausführt (E-Mail senden), wenn die Reisezeit einen angegebenen Grenzwert überschreitet. Erfahren Sie nun, wie Sie eine Logik-App erstellen, die Adressenlistenanforderungen zur Genehmigung sendet, und dabei Azure-Dienste, Microsoft-Dienste und andere SaaS-Apps integrieren.

> [!div class="nextstepaction"]
> [Verwalten von Adressenlistenanforderungen](../logic-apps/tutorial-process-mailing-list-subscriptions-workflow.md)
