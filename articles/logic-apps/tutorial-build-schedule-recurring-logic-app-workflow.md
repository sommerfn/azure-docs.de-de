---
title: Erstellen zeitplanbasierter automatisierter Workflows – Azure Logic Apps
description: 'Tutorial: Erstellen eines zeitplanbasierten, periodischen automatisierten Workflows mithilfe von Azure Logic Apps'
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.manager: carmonm
ms.reviewer: klam, LADocs
ms.topic: tutorial
ms.custom: mvc
ms.date: 09/12/2019
ms.openlocfilehash: eae2319e8d1c162969a04f8dafa18eec671ee1d0
ms.sourcegitcommit: f9e81b39693206b824e40d7657d0466246aadd6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2019
ms.locfileid: "72034659"
---
# <a name="tutorial-create-automated-schedule-based-recurring-workflows-by-using-azure-logic-apps"></a>Tutorial: Erstellen automatisierter, zeitplanbasierter periodischer Workflows mithilfe von Azure Logic Apps

In diesem Tutorial erfahren Sie, wie Sie eine [Logik-App](../logic-apps/logic-apps-overview.md) erstellen und einen periodischen, zeitplanbasierten Workflow automatisieren. Diese exemplarische Logik-App wird am Morgen jedes Werktags ausgeführt und überprüft die Reisezeit zwischen zwei Orten unter Berücksichtigung des Verkehrsaufkommens. Wenn die Reisezeit einen bestimmten Grenzwert überschreitet, sendet die Logik-App eine E-Mail mit der Reisezeit und der zusätzlich bis zum Ziel benötigten Zeit.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer leeren Logik-App
> * Hinzufügen eines Serientriggers, der den Zeitplan für Ihre Logik-App angibt
> * Hinzufügen einer Bing Maps-Aktion, die die Reisezeit für eine Route ermittelt
> * Hinzufügen einer Aktion, die eine Variable erstellt, die die Reisezeit von Sekunden in Minuten umrechnet und das Ergebnis in der Variablen speichert
> * Hinzufügen einer Bedingung, die die Reisezeit mit einem bestimmten Grenzwert vergleicht
> * Hinzufügen einer Aktion, die Ihnen eine E-Mail sendet, wenn die Reisezeit den Grenzwert überschreitet

Am Ende entspricht Ihre Logik-App grob dem folgenden Workflow:

![Allgemeine Übersicht über den Logik-App-Workflow](./media/tutorial-build-scheduled-recurring-logic-app-workflow/check-travel-time-overview.png)

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Sollten Sie über kein Abonnement verfügen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/), bevor Sie beginnen.

* Ein E-Mail-Konto eines von Logic Apps unterstützten E-Mail-Anbieters (beispielsweise Office 365 Outlook, Outlook.com oder Gmail). Informationen zu Connectors für andere Anbieter finden Sie in [dieser Liste](https://docs.microsoft.com/connectors/). In dieser Schnellstartanleitung wird ein Office 365 Outlook-Konto verwendet. Bei Verwendung eines anderen E-Mail-Kontos bleiben die allgemeinen Schritte zwar gleich, die Benutzeroberfläche unterscheidet sich aber ggf. etwas.

* Um die Reisezeit für eine Route zu ermitteln, benötigen Sie einen Zugriffsschlüssel für die Bing Maps-API. Führen Sie zum Abrufen dieses Schlüssels die Schritte zum [Abrufen eines Bing Maps-Schlüssels](https://docs.microsoft.com/bingmaps/getting-started/bing-maps-dev-center-help/getting-a-bing-maps-key) aus.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich mit den Anmeldeinformationen Ihres Azure-Kontos beim [Azure-Portal](https://portal.azure.com) an.

## <a name="create-your-logic-app"></a>Erstellen Ihrer Logik-App

1. Klicken Sie im Hauptmenü von Azure auf **Ressource erstellen** > **Integration** > **Logik-App**.

   ![Erstellen einer Logik-App-Ressource](./media/tutorial-build-scheduled-recurring-logic-app-workflow/create-new-logic-app-resource.png)

1. Geben Sie unter **Logik-App erstellen** die Informationen wie beschrieben ein. Wählen Sie **Erstellen**, wenn Sie fertig sind.

   ![Angeben von Informationen zu Ihrer Logik-App](./media/tutorial-build-scheduled-recurring-logic-app-workflow/create-logic-app-settings.png)

   | Eigenschaft | Wert | BESCHREIBUNG |
   |----------|-------|-------------|
   | **Name** | LA-TravelTime | Der Name Ihrer Logik-App. Er darf nur Buchstaben, Ziffern, Bindestriche (`-`), Unterstriche (`_`), Klammern (`(`, `)`) und Punkte (`.`) enthalten. In diesem Beispiel wird „LA-TravelTime“ verwendet. |
   | **Abonnement** | <*Name Ihres Azure Abonnements*> | Der Name Ihres Azure-Abonnements |
   | **Ressourcengruppe** | LA-TravelTime-RG | Der Name der [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md), die zum Organisieren verwandter Ressourcen verwendet wird. In diesem Beispiel wird „LA-TravelTime-RG“ verwendet. |
   | **Location** | USA (Westen) | Die Region, in der die Informationen zu Ihrer Logik-App gespeichert werden sollen. In diesem Beispiel wird „USA, Westen“ verwendet. |
   | **Log Analytics** | Aus | Behalten Sie die Einstellung **Aus** für die Diagnoseprotokollierung bei. |
   ||||

1. Wählen Sie nach der Bereitstellung Ihrer App durch Azure für Ihre bereitgestellte Logik-App auf der Azure-Symbolleiste **Benachrichtigungen** > **Zu Ressource wechseln** aus.

   ![Navigieren zu Ihrer neuen Logik-App-Ressource](./media/tutorial-build-scheduled-recurring-logic-app-workflow/go-to-new-logic-app-resource.png)

   Sie können zum Suchen und Auswählen Ihrer Logik-App auch den Namen in das Suchfeld eingeben.

   Der Designer für Logik-Apps wird geöffnet. Es wird eine Seite mit einem Einführungsvideo sowie mit häufig verwendeten Triggern und Logik-App-Mustern angezeigt. Wählen Sie unter **Vorlagen** die Option **Leere Logik-App**.

   ![Auswählen der Vorlage „Leere Logik-App“](./media/tutorial-build-scheduled-recurring-logic-app-workflow/select-logic-app-template.png)

Fügen Sie als Nächstes den [Serientrigger](../logic-apps/logic-apps-overview.md#logic-app-concepts) hinzu, der auf der Grundlage des angegebenen Zeitplans ausgelöst wird. Jede Logik-App beginnt mit einem Trigger, der ausgelöst wird, wenn ein bestimmtes Ereignis eintritt oder neue Daten eine bestimmte Bedingung erfüllen. Weitere Informationen finden Sie unter [Erstellen Ihrer ersten Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="add-the-recurrence-trigger"></a>Hinzufügen des Serientriggers

1. Geben Sie im Designer für Logik-Apps die Zeichenfolge „Serie“ als Filter in das Suchfeld ein. Wählen Sie in der Liste **Trigger** den Trigger **Serie** aus.

   ![Hinzufügen eines Recurrence-Triggers](./media/tutorial-build-scheduled-recurring-logic-app-workflow/add-schedule-recurrence-trigger.png)

1. Wählen Sie Bereich **Serie** die Schaltfläche mit den **Auslassungspunkten** ( **...** ) und anschließend **Umbenennen** aus. Nennen Sie den Trigger wie folgt: `Check travel time every weekday morning`

   ![Umbenennen der Beschreibung des Wiederholungstriggers](./media/tutorial-build-scheduled-recurring-logic-app-workflow/rename-recurrence-schedule-trigger.png)

1. Ändern Sie innerhalb des Triggers die folgenden Eigenschaften:

   ![Ändern des Intervalls und der Häufigkeit des Wiederholungstriggers](./media/tutorial-build-scheduled-recurring-logic-app-workflow/change-interval-frequency.png)

   | Eigenschaft | Erforderlich | Value | BESCHREIBUNG |
   |----------|----------|-------|-------------|
   | **Intervall** | Ja | 1 | Die Anzahl von Warteintervallen zwischen Überprüfungen |
   | **Frequency** | Ja | Woche | Die Zeiteinheit für die Wiederholung |
   |||||

1. Öffnen Sie unter **Intervall** und **Frequenz** die Liste **Neuen Parameter hinzufügen**, und wählen Sie die folgenden Eigenschaften aus, um sie dem Trigger hinzuzufügen:

   * **An diesen Tagen**
   * **Zu diesen Stunden**
   * **Zu diesen Minuten**

   ![Hinzufügen von Eigenschaften für den Wiederholungstrigger](./media/tutorial-build-scheduled-recurring-logic-app-workflow/add-trigger-properties.png)

1. Legen Sie als Nächstes die Werte für die zusätzlichen Eigenschaften wie hier gezeigt und beschrieben fest.

   ![Eingeben der Zeitplan- und Wiederholungsdetails](./media/tutorial-build-scheduled-recurring-logic-app-workflow/recurrence-trigger-property-values.png)

   | Eigenschaft | Wert | BESCHREIBUNG |
   |----------|-------|-------------|
   | **An diesen Tagen** | Montag,Dienstag,Mittwoch,Donnerstag,Freitag | Nur verfügbar, wenn für **Häufigkeit** die Option „Woche“ festgelegt ist. |
   | **Zu diesen Stunden** | 7,8,9 | Nur verfügbar, wenn für **Häufigkeit** die Option „Woche“ oder „Tag“ festgelegt ist. Wählen Sie für die Ausführung der Wiederholung die Stunden des Tages aus. Dieses Beispiel wird um 7, 8 und 9 Uhr ausgeführt. |
   | **Zu diesen Minuten** | 0,15,30,45 | Nur verfügbar, wenn für **Häufigkeit** die Option „Woche“ oder „Tag“ festgelegt ist. Wählen Sie für die Ausführung der Wiederholung die Minuten des Tages aus. Dieses Beispiel wird ab 0 Uhr alle 15 Minuten ausgeführt. |
   ||||

   Dieser Trigger wird an jedem Wochentag alle 15 Minuten ausgelöst. Die Auslösung beginnt um 7:00 Uhr und endet um 9:45 Uhr. Im Feld **Vorschau** wird der Wiederholungszeitplan angezeigt. Weitere Informationen finden Sie unter [Planen von regelmäßig ausgeführten Aufgaben und Workflows mit Logik-Apps](../connectors/connectors-native-recurrence.md) und [Trigger und Aktionen für Logik-App-Workflows](../logic-apps/logic-apps-workflow-actions-triggers.md#recurrence-trigger).

1. Klicken Sie auf die Titelleiste des Bereichs, um die Triggerdetails vorerst auszublenden.

   ![Ausblenden der Details durch Reduzieren des Bereichs](./media/tutorial-build-scheduled-recurring-logic-app-workflow/collapse-trigger-shape.png)

1. Speichern Sie Ihre Logik-App. Wählen Sie auf der Symbolleiste des Designers **Speichern** aus.

Ihre Logik-App befindet sich jetzt im Livemodus, wird aber vorerst nur periodisch ausgeführt. Fügen Sie daher eine Aktion hinzu, die reagiert, wenn der Trigger ausgelöst wird.

## <a name="get-the-travel-time-for-a-route"></a>Abrufen der Reisezeit für eine Route

Sie verfügen über einen Trigger und können nun eine [Aktion](../logic-apps/logic-apps-overview.md#logic-app-concepts) hinzufügen, die die Reisezeit zwischen zwei Orten ermittelt. Logik-Apps stellt einen Connector für die Bing Maps-API bereit, sodass Sie diese Informationen problemlos abrufen können. Stellen Sie vor dem Starten dieser Aufgabe sicher, dass Sie einen Bing Maps-API-Schlüssel besitzen, wie in den Voraussetzungen dieses Tutorials beschrieben.

1. Wählen Sie im Designer für Logik-Apps unter Ihrem Trigger die Option **Neuer Schritt** aus.

1. Wählen Sie unter **Aktion auswählen** die Option **Standard** aus. Geben Sie „bing maps“ als Filter in das Suchfeld ein, und wählen Sie die Aktion **Get route** (Route ermitteln) aus.

   ![Auswählen der Aktion „Get route“ (Route ermitteln)](./media/tutorial-build-scheduled-recurring-logic-app-workflow/select-get-route-action.png)

1. Falls keine Verbindung mit Bing Maps besteht, werden Sie aufgefordert, eine Verbindung herzustellen. Geben Sie die folgenden Verbindungsdetails an, und wählen Sie **Erstellen** aus:

   ![Erstellen einer Verbindung mit der Bing Karten-API](./media/tutorial-build-scheduled-recurring-logic-app-workflow/create-maps-connection.png)

   | Eigenschaft | Erforderlich | Value | BESCHREIBUNG |
   |----------|----------|-------|-------------|
   | **Verbindungsname** | Ja | BingMapsConnection | Geben Sie einen Namen für die Verbindung an. In diesem Beispiel wird „BingMapsConnection“ verwendet. |
   | **API-Schlüssel** | Ja | <*Ihr Bing Maps-Schlüssel*> | Geben Sie den Bing Maps-Schlüssel ein, den Sie zuvor abgerufen haben. Falls Sie keinen Bing Maps-Schlüssel besitzen, lesen Sie die Informationen zum [Abrufen eines Schlüssels](https://msdn.microsoft.com/library/ff428642.aspx). |
   |||||

1. Nennen Sie die Aktion wie folgt: `Get route and travel time with traffic`

1. Öffnen Sie innerhalb der Aktion die Liste **Neuen Parameter hinzufügen**, und wählen Sie die folgenden Eigenschaften aus, um sie der Aktion hinzuzufügen:

   * **Optimieren**
   * **Distance unit** (Einheit für Entfernung)
   * **Travel mode** (Reisemodus)

   ![Hinzufügen von Eigenschaften zur Aktion „Get route“ (Route ermitteln)](./media/tutorial-build-scheduled-recurring-logic-app-workflow/add-bing-maps-action-properties.png) 

1. Legen Sie als Nächstes die Werte für die Aktionseigenschaften wie hier gezeigt und beschrieben fest.

   ![Angeben von Details für die Aktion „Get route“ (Route ermitteln)](./media/tutorial-build-scheduled-recurring-logic-app-workflow/get-route-action-settings.png) 

   | Eigenschaft | Erforderlich | Value | BESCHREIBUNG |
   |----------|----------|-------|-------------|
   | **Wegpunkt 1** | Ja | <*Start*> | Ausgangspunkt für die Route |
   | **Wegpunkt 2** | Ja | <*Ziel*> | Ziel der Route |
   | **Optimieren** | Nein | timeWithTraffic | Ein Parameter zur Optimierung der Route, z.B. Entfernung, Reisezeit basierend auf der aktuellen Verkehrslage, usw. Wählen Sie den Parameter „timeWithTraffic“ aus. |
   | **Distance unit** (Einheit für Entfernung) | Nein | <*Ihre Präferenz*> | Die Einheit der Entfernung für die Route. In diesem Beispiel wird „Meile“ als Einheit verwendet. |
   | **Travel mode** (Reisemodus) | Nein | Driving (Auto) | Der Reisemodus für die Route. Wählen Sie den Modus „Driving“ (Auto) aus. |
   ||||

   Weitere Informationen zu diesen Parametern finden Sie unter [Calculate a route](https://docs.microsoft.com/bingmaps/rest-services/routes/calculate-a-route) (Berechnen einer Route).

1. Speichern Sie Ihre Logik-App.

Erstellen Sie als Nächstes eine Variable, sodass Sie die aktuelle Reisezeit von Sekunden in Minuten umwandeln und speichern können. So müssen Sie die Umwandlung nicht wiederholen und können den Wert in späteren Schritten einfacher verwenden. 

## <a name="create-a-variable-to-store-travel-time"></a>Erstellen einer Variablen zum Speichern der Reisezeit

Manchmal sollen gegebenenfalls Vorgänge für Daten in Ihrem Workflow ausgeführt und die Ergebnisse in späteren Aktionen verwendet werden. Wenn Sie Ergebnisse speichern möchten, um sie einfach wiederverwenden oder einfach darauf verweisen zu können, erstellen Sie Variablen, um diese Ergebnisse nach ihrer Verarbeitung zu speichern. Sie können Variablen nur auf der obersten Ebene in der Logik-App erstellen.

Die vorherige Aktion **Get route** (Route ermitteln) gibt standardmäßig die aktuelle Reisezeit aus der Eigenschaft **Travel Duration Traffic** (Reisedauer (Verkehr)) in Sekunden zurück. Wenn Sie diesen Wert stattdessen in Minuten umwandeln und speichern, kann er später ohne erneute Umwandlung einfacher verwendet werden.

1. Wählen Sie unter der Aktion **Get route** (Route ermitteln) die Option **Neuer Schritt** aus.

1. Wählen Sie unter **Aktion auswählen** die Option **Integriert** aus. Geben Sie „Variablen“ in das Suchfeld ein, und wählen Sie die Aktion **Variable initialisieren** aus.

   ![Auswählen der Aktion „Variable initialisieren“](./media/tutorial-build-scheduled-recurring-logic-app-workflow/select-initialize-variable-action.png)

1. Nennen Sie diese Aktion wie folgt: `Create variable to store travel time`

1. Geben Sie die Details für die Variable an wie hier beschrieben:

   | Eigenschaft | Erforderlich | Value | BESCHREIBUNG |
   |----------|----------|-------|-------------|
   | **Name** | Ja | travelTime | Der Name für Ihre Variable. In diesem Beispiel wird „travelTime“ verwendet. |
   | **Typ** | Ja | Integer | Der Datentyp für die Variable |
   | **Wert** | Nein| Ein Ausdruck, der die aktuelle Reisezeit von Sekunden in Minuten umwandelt. (Sehen Sie sich die Schritte unterhalb dieser Tabelle an.) | Der Anfangswert für die Variable |
   ||||

   1. Klicken Sie zum Erstellen des Ausdrucks für die Eigenschaft **Wert** innerhalb des Felds, sodass die Liste mit den dynamischen Inhalten angezeigt wird. Verbreitern Sie ggf. das Browserfenster, bis die Liste angezeigt wird. Wählen Sie in der Liste mit den dynamischen Inhalten die Option **Ausdruck** aus.

      ![Angeben von Informationen für die Aktion „Variable initialisieren“](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings.png)

      Wenn Sie auf einige der Bearbeitungsfelder klicken, wird die Liste mit den dynamischen Inhalten angezeigt. Diese Liste enthält alle Eigenschaften aus vorherigen Aktionen, die als Eingabe in Ihrem Workflow verwendet werden können. Die Liste mit den dynamischen Inhalten verfügt über einen Ausdrucks-Editor, in dem Sie Funktionen zum Ausführen von Vorgängen auswählen können. Dieser Ausdrucks-Editor wird nur in der Liste mit den dynamischen Inhalten angezeigt.

   1. Geben Sie im Ausdrucks-Editor den folgenden Ausdruck ein: `div(,60)`.

      ![Eingeben des Ausdrucks „div(,60)“](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings-2.png)

   1. Positionieren Sie den Cursor innerhalb des Ausdrucks zwischen der linken Klammer ( **(** ) und dem Komma ( **,** ). 
   Wählen Sie **Dynamischer Inhalt** aus.

      ![Positionieren des Cursors und Auswählen von „Dynamischer Inhalt“](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings-3.png)

   1. Klicken Sie in der Liste mit den dynamischen Inhalten auf **Travel Duration Traffic** (Reisedauer (Verkehr)).

      ![Auswählen der Eigenschaft „Travel Duration Traffic“ (Reisedauer (Verkehr))](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings-4.png)

   1. Warten Sie, bis der Eigenschaftswert innerhalb des Ausdrucks aufgelöst wurde, und wählen Sie **OK** aus.

      ![Auswählen von „OK“, um die Erstellung des Ausdrucks abzuschließen](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings-5.png)

      Die Eigenschaft **Wert** wird nun wie folgt angezeigt:

      ![Eigenschaft „Wert“ mit aufgelöstem Ausdruck](./media/tutorial-build-scheduled-recurring-logic-app-workflow/initialize-variable-action-settings-6.png)

1. Speichern Sie Ihre Logik-App.

Fügen Sie als Nächstes eine Bedingung hinzu, die überprüft, ob die aktuelle Reisezeit größer als ein bestimmter Grenzwert ist.

## <a name="compare-the-travel-time-with-limit"></a>Vergleichen der Reisezeit mit einem Grenzwert

1. Wählen Sie unter der vorherigen Aktion die Option **Neuer Schritt** aus.

1. Wählen Sie unter **Aktion auswählen** die Option **Integriert** aus. Geben Sie im Suchfeld den Begriff „Bedingung“ als Filter ein. Wählen Sie in der Liste „Aktionen“ die Aktion **Bedingung** aus.

   ![Auswählen der Aktion „Bedingung“](./media/tutorial-build-scheduled-recurring-logic-app-workflow/select-condition-action.png)

1. Nennen Sie die Bedingung wie folgt: `If travel time exceeds limit`

1. Erstellen Sie wie hier beschrieben und gezeigt eine Bedingung, die überprüft, ob der Eigenschaftswert **travelTime** den von Ihnen angegebenen Grenzwert überschreitet:

   1. Klicken Sie in der Bedingung auf das Feld **Wert auswählen** (links von der Bedingung).

   1. Wählen Sie in der angezeigten Liste mit den dynamischen Inhalten unter **Variablen** die Eigenschaft **travelTime** aus.

      ![Erstellen der linken Seite der Bedingung](./media/tutorial-build-scheduled-recurring-logic-app-workflow/build-condition-left-side.png)

   1. Wählen Sie im mittleren Vergleichsfeld den Operator **ist größer als** aus.

   1. Geben Sie auf der rechten Seite der Bedingung im Feld **Wert auswählen** den folgenden Grenzwert ein: `15`

      Danach sieht die Bedingung wie im folgenden Beispiel aus:

      ![Fertige Bedingung zum Überprüfen der Reisezeit](./media/tutorial-build-scheduled-recurring-logic-app-workflow/build-condition-check-travel-time.png)

1. Speichern Sie Ihre Logik-App.

Fügen Sie als Nächstes die Aktion hinzu, die ausgeführt werden soll, wenn die Reisezeit Ihren Grenzwert überschreitet.

## <a name="send-email-when-limit-exceeded"></a>Senden einer E-Mail bei Überschreiten des Grenzwerts

Fügen Sie nun eine Aktion hinzu, die Ihnen eine E-Mail sendet, wenn die Reisezeit Ihren Grenzwert überschreitet. Diese E-Mail enthält die aktuelle Reisezeit sowie die zusätzliche Zeit, die für die angegebene Route erforderlich ist.

1. Wählen Sie in der Bedingungsverzweigung **Bei TRUE** die Option **Aktion hinzufügen** aus.

1. Wählen Sie unter **Aktion auswählen** die Option **Standard** aus. Geben Sie „E-Mail senden“ in das Suchfeld ein. Daraufhin wird eine Liste mit zahlreichen Ergebnissen zurückgegeben. Wählen Sie daher zunächst den gewünschten E-Mail-Connector aus. Beispiel:

   ![Auswählen des gewünschten E-Mail-Connectors](./media/tutorial-build-scheduled-recurring-logic-app-workflow/add-action-send-email.png)

   * Wählen Sie für Geschäfts-, Schul- oder Unikonten für Azure die Option **Office 365 Outlook**.
   * Wählen Sie für persönliche Microsoft-Konten die Option **Outlook.com**.

1. Wenn die Aktionen des Connectors angezeigt werden, wählen Sie die gewünschte Aktion vom Typ „E-Mail senden“ aus. Beispiel:

   ![Auswählen der Aktion „E-Mail senden“](./media/tutorial-build-scheduled-recurring-logic-app-workflow/select-send-email-action.png)

1. Besteht noch keine Verbindung, werden Sie zur Anmeldung bei Ihrem E-Mail-Konto aufgefordert.

   Logic Apps erstellt eine Verbindung mit Ihrem E-Mail-Konto.

1. Nennen Sie die Aktion wie folgt: `Send email with travel time`

1. Geben Sie im Feld **An** die E-Mail-Adresse des Empfängers ein. Geben Sie zu Testzwecken Ihre E-Mail-Adresse an.

1. Geben Sie im Feld **Betreff** den Betreff der E-Mail sowie die Variable **travelTime** ein.

   1. Geben Sie den Text `Current travel time (minutes):` mit einem nachstehenden Leerzeichen ein. 

   1. Wählen Sie in der Liste mit den dynamischen Inhalten unter **Variablen** die Option **Mehr anzeigen** aus.

      ![Suchen nach der Variablen „travelTime“](./media/tutorial-build-scheduled-recurring-logic-app-workflow/find-travelTime-variable.png)

   1. Wenn **travelTime** unter **Variablen** angezeigt wird, wählen Sie **travelTime** aus.

      ![Eingeben des Betrefftexts und des Ausdrucks, der die Reisezeit zurückgibt](./media/tutorial-build-scheduled-recurring-logic-app-workflow/select-travelTime-variable.png)

1. Geben Sie im Feld **Text** den Inhalt für den E-Mail-Text ein.

   1. Geben Sie den Text `Add extra travel time (minutes):` mit einem nachstehenden Leerzeichen ein.

   1. Wählen Sie in der Liste mit den dynamischen Inhalten die Option **Ausdruck** aus.

      ![Erstellen eines Ausdrucks für den E-Mail-Text](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings.png)

   1. Geben Sie im Ausdrucks-Editor diesen Ausdruck ein, sodass Sie die Anzahl von Minuten berechnen können, um die der Grenzwert überschritten wird: ```sub(,15)```

      ![Eingeben eines Ausdrucks zum Berechnen der zusätzlichen Reisezeit](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings-2.png)

   1. Positionieren Sie den Cursor innerhalb des Ausdrucks zwischen der linken Klammer ( **(** ) und dem Komma ( **,** ). Wählen Sie **Dynamischer Inhalt** aus.

      ![Fortsetzen der Erstellung eines Ausdrucks zum Berechnen der zusätzlichen Reisezeit](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings-3.png)

   1. Klicken Sie unter **Variablen** auf **travelTime**.

      ![Auswählen der Eigenschaft „travelTime“, um sie im Ausdruck zu verwenden](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings-4.png)

   1. Warten Sie, bis die Eigenschaft innerhalb des Ausdrucks aufgelöst wurde, und wählen Sie **OK** aus.

      ![Auswählen von „OK“ nach Auflösen der Eigenschaft „Text“](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings-5.png)

      Die Eigenschaft **Text** wird nun wie folgt angezeigt:

      ![Aufgelöste Eigenschaft „Text“ im Ausdruck](./media/tutorial-build-scheduled-recurring-logic-app-workflow/send-email-body-settings-6.png)

1. Speichern Sie Ihre Logik-App.

Testen Sie als Nächstes Ihre Logik-App, die nun in etwa wie folgt aussieht:

![Fertiges Beispiel für Logik-App-Workflow](./media/tutorial-build-scheduled-recurring-logic-app-workflow/check-travel-time-finished.png)

## <a name="run-your-logic-app"></a>Ausführen Ihrer Logik-App

Wählen Sie auf der Symbolleiste des Designers die Option **Ausführen** aus, um Ihre Logik-App manuell zu starten.

* Wenn die aktuelle Reisezeit den Grenzwert nicht überschreitet, führt Ihre Logik-App keine weitere Aktion aus und wartet mit der nächsten Überprüfung bis zum nächsten Intervall. 

* Sollte die aktuelle Reisezeit den Grenzwert überschreiten, erhalten Sie eine E-Mail mit der aktuellen Reisezeit und der Anzahl von Minuten, um die der Grenzwert überschritten wird. Hier sehen Sie ein Beispiel für eine von Logik-App gesendete E-Mail:

![Gesendete Beispiel-E-Mail mit der Reisezeit](./media/tutorial-build-scheduled-recurring-logic-app-workflow/received-example-email-notification.png)

Sollten Sie keine E-Mails erhalten, überprüfen Sie Ihren Ordner für Junk-E-Mails. E-Mails dieser Art werden unter Umständen durch Ihren Junk-E-Mail-Filter umgeleitet. Wenn Sie unsicher sind, ob Ihre Logik-App richtig ausgeführt wurde, helfen Ihnen die Informationen unter [Diagnostizieren von Fehlern bei Logik-Apps](../logic-apps/logic-apps-diagnosing-failures.md) weiter.

Glückwunsch! Sie haben eine zeitplanbasierte Logik-App für die wiederholte Ausführung erstellt und ausgeführt. 

Wenn Sie weitere Logik-Apps mit dem Trigger **Serie** erstellen möchten, sehen Sie sich die folgenden Vorlagen an. Diese sind nach der Erstellung einer Logik-App verfügbar:

* Tägliche Erinnerungen erhalten
* Ältere Azure-Blobs löschen
* Nachricht zu einer Azure Storage-Warteschlange hinzufügen

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie diese exemplarische Logik-App nicht mehr benötigen, löschen Sie die Ressourcengruppe, die Ihre Logik-App und die zugehörigen Ressourcen enthält. 

1. Wechseln Sie im Azure-Hauptmenü zu **Ressourcengruppen**, und wählen Sie die Ressourcengruppe für Ihre Logik-App aus.

1. Wählen Sie im Ressourcengruppenmenü **Übersicht** > **Ressourcengruppe löschen** aus. 

   ![„Übersicht“ > „Ressourcengruppe löschen“](./media/tutorial-build-scheduled-recurring-logic-app-workflow/delete-resource-group.png)

1. Geben Sie zur Bestätigung den Ressourcengruppennamen ein, und wählen Sie **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine Logik-App erstellt, die die Verkehrslage gemäß einem festgelegten Zeitplan (morgens an Wochentagen) überprüft und eine Aktion ausführt (E-Mail senden), wenn die Reisezeit einen angegebenen Grenzwert überschreitet. Erfahren Sie nun, wie Sie eine Logik-App erstellen, die Adressenlistenanforderungen zur Genehmigung sendet, und dabei Azure-Dienste, Microsoft-Dienste und andere SaaS-Apps integrieren.

> [!div class="nextstepaction"]
> [Verwalten von Adressenlistenanforderungen](../logic-apps/tutorial-process-mailing-list-subscriptions-workflow.md)
