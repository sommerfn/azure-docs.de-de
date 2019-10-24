---
title: Warten und Reagieren auf Ereignisse – Azure Logic Apps
description: Automatisieren Sie mithilfe von Azure Logic Apps Workflows, die basierend auf Ereignissen an einem Dienstendpunkt ausgelöst, angehalten und fortgesetzt werden.
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: conceptual
ms.date: 10/10/2019
tags: connectors
ms.openlocfilehash: 36b0ea7233b449584bd83450b45276da5baa135b
ms.sourcegitcommit: b4665f444dcafccd74415fb6cc3d3b65746a1a31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2019
ms.locfileid: "72264343"
---
# <a name="create-and-run-automated-event-based-workflows-by-using-http-webhooks-in-azure-logic-apps"></a>Erstellen und Ausführen automatisierter ereignisbasierter Workflows mithilfe von HTTP-Webhooks in Azure Logic Apps

Mit [Azure Logic Apps](../logic-apps/logic-apps-overview.md) und dem integrierten HTTP-Webhookconnector können Sie Workflows automatisieren, die auf der Grundlage bestimmter Ereignisse warten und ausgeführt werden, die an einem HTTP- oder HTTPS-Endpunkt auftreten, indem Sie Logik-Apps erstellen. So können Sie beispielsweise eine Logik-App erstellen, die einen Dienstendpunkt überwacht, indem sie auf ein bestimmtes Ereignis wartet, bevor sie den Workflow auslöst und die angegebenen Aktionen ausführt, anstatt diesen Endpunkt regelmäßig zu überprüfen oder *abzufragen*.

Hier sind einige Beispiele für ereignisbasierte Workflows:

* Warten, dass ein Element von einem [Azure Event Hub](https://github.com/logicappsio/EventHubAPI) eintrifft, bevor die Ausführung einer Logik-App ausgelöst wird.
* Warten auf eine Genehmigung, bevor ein Workflow fortgesetzt wird.

## <a name="how-do-webhooks-work"></a>Wie funktionieren Webhooks?

Ein HTTP-Webhooktrigger ist ereignisbasiert und erfordert nicht, dass regelmäßig auf neue Elemente geprüft wird oder diese abgefragt werden. Wenn Sie eine Logik-App speichern, die mit einem Webhooktrigger gestartet wird, oder Ihre Logik-App von „deaktiviert“ in „aktiviert“ ändern, *abonniert* der Webhooktrigger einen angegebenen Dienst oder Endpunkt, indem er eine *Rückruf-URL* bei diesem Dienst oder Endpunkt registriert. Der Trigger wartet dann darauf, dass der Dienst oder Endpunkt die URL aufruft, wodurch die Ausführung der Logik-App beginnt. Die Logik-App wird ähnlich wie der [Anforderungstrigger](connectors-native-reqres.md) sofort ausgelöst, wenn das angegebene Ereignis eintritt. Der Trigger *kündigt das Abonnement* des Diensts oder Endpunkts, wenn Sie ihn entfernen und Ihre Logik-App speichern oder wenn Sie Ihre Logik-App von „aktiviert“ in „deaktiviert“ ändern.

Eine Webhookaktion ist ebenfalls ereignisbasiert und *abonniert* den angegebenen Dienst oder Endpunkt durch die Registrierung einer *Rückruf-URL* bei diesem Dienst oder Endpunkt. Die Webhookaktion hält den Workflow der Logik-App an und wartet, bis der Dienst oder der Endpunkt die URL aufruft, bevor die Logik-App die Ausführung fortsetzt. Die Aktionslogik-App *kündigt das Abonnement* für den Dienst oder Endpunkt in den folgenden Fällen:

* Wenn die Webhookaktion erfolgreich abgeschlossen wurde
* Wenn die Logik-App während des Wartens auf eine Antwort abgebrochen wird
* Bevor Ein Timeout für die Logik-App auftritt

Die Aktion zum [**Senden einer Genehmigungs-E-Mail**](connectors-create-api-office365-outlook.md) von Office 365 Outlook ist ein Beispiel für eine Webhookaktion, die diesem Muster folgt. Sie können dieses Muster mithilfe der Webhookaktion auf jeden Dienst erweitern.

> [!NOTE]
> Azure Logic Apps erzwingt Transport Layer Security (TLS) 1.2, wenn der Rückruf auf den HTTP-Webhook-Trigger oder eine Aktion empfangen wird. Wenn SSL-Handshakefehler auftreten, stellen Sie sicher, dass Sie TLS 1.2 verwenden.

Weitere Informationen finden Sie in den folgenden Themen:

* [Triggerparameter des HTTP-Webhooks](../logic-apps/logic-apps-workflow-actions-triggers.md#http-webhook-trigger)
* [Webhooks und Abonnements](../logic-apps/logic-apps-workflow-actions-triggers.md#webhooks-and-subscriptions)
* [Erstellen von benutzerdefinierten APIs, die einen Webhook unterstützen](../logic-apps/logic-apps-create-api-app.md)

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/).

* Die URL für einen bereits bereitgestellten Endpunkt oder eine API, die das Webhookmuster für das Abonnieren und Kündigen von Abonnements für [Webhooktrigger in Logik-Apps](../logic-apps/logic-apps-create-api-app.md#webhook-triggers) oder [Webhookaktionen in Logik-Apps](../logic-apps/logic-apps-create-api-app.md#webhook-actions) je nach Bedarf unterstützt.

* Grundlegende Kenntnisse über das [Erstellen von Logik-Apps](../logic-apps/quickstart-create-first-logic-app-workflow.md). Falls Sie noch nicht mit Logik-Apps vertraut sind, finden Sie weitere Informationen unter [Was ist Azure Logic Apps?](../logic-apps/logic-apps-overview.md).

* Die Logik-App, in der auf bestimmte Ereignisse am Zielendpunkt gewartet werden soll. Um mit dem HTTP-Webhooktrigger zu beginnen, [erstellen Sie eine leere Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md). Um die HTTP-Webhookaktion zu verwenden, starten Sie Ihre Logik-App mit einem beliebigen Trigger. Dieses Beispiel verwendet den HTTP-Trigger als ersten Schritt.

## <a name="add-an-http-webhook-trigger"></a>Hinzufügen eines HTTP-Webhooktriggers

Dieser integrierte Trigger registriert eine Rückruf-URL beim angegebenen Dienst und wartet darauf, dass dieser Dienst eine HTTP POST-Anforderung an diese URL sendet. Wenn dieses Ereignis eintritt, wird der Trigger ausgelöst und führt sofort die Logik-App aus.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. Öffnen Sie Ihre leere Logik-App im Logik-App-Designer.

1. Geben Sie im Suchfeld des Designers „http webhook“ als Filter ein. Wählen Sie aus der Liste **Trigger** den Trigger **HTTP-Webhook** aus.

   ![Auswählen des HTTP-Webhooktriggers](./media/connectors-native-webhook/select-http-webhook-trigger.png)

   Dieses Beispiel benennt den Trigger in „HTTP-Webhooktrigger“ um, damit der Schritt über einen aussagekräftigeren Namen verfügt. Darüber hinaus fügt das Beispiel später eine HTTP-Webhookaktion hinzu, und beide Namen müssen eindeutig sein.

1. Geben Sie die Werte für die [Parameter des HTTP-Webhooktriggers](../logic-apps/logic-apps-workflow-actions-triggers.md#http-webhook-trigger) an, die Sie für die Aufrufe zum Abonnieren und Kündigen eines Abonnements verwenden möchten:

   ![Eingeben von Triggerparametern des HTTP-Webhooks](./media/connectors-native-webhook/http-webhook-trigger-parameters.png)

1. Öffnen Sie zum Hinzufügen weiterer verfügbarer Parameter die Liste **Neuen Parameter hinzufügen**, und wählen Sie die gewünschten Parameter aus.

   Weitere Informationen zu verfügbaren Authentifizierungstypen für HTTP-Webhooks finden Sie unter [Authentifizieren von HTTP-Triggern und -Aktionen](../logic-apps/logic-apps-workflow-actions-triggers.md#connector-authentication).

1. Fahren Sie mit dem Erstellen des Workflows Ihrer Logik-App fort, und fügen Sie weitere Aktionen hinzu, die bei Auslösung des Triggers ausgeführt werden.

1. Speichern Sie die Logik-App unbedingt, wenn Sie fertig sind. Wählen Sie auf der Symbolleiste des Designers **Speichern** aus.

   Das Speichern Ihrer Logik-App ruft den Abonnementendpunkt auf und registriert die Rückruf-URL für das Auslösen dieser Logik-App.

1. Wenn der Zieldienst nun eine `HTTP POST`-Anforderung an die Rückruf-URL sendet, wird die Logik-App ausgelöst und enthält anschließend alle Daten, die durch die Anforderung übergeben werden.

## <a name="add-an-http-webhook-action"></a>Hinzufügen einer HTTP-Webhookaktion

Diese integrierte Aktion registriert eine Rückruf-URL beim angegebenen Dienst, hält den Workflow der Logik-App an und wartet darauf, dass dieser Dienst eine HTTP POST-Anforderung an diese URL sendet. Wenn dieses Ereignis eintritt, wird die Aktion fortgesetzt und die Logik-App ausgeführt.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. Öffnen Sie Ihre Logik-App im Logik-App-Designer.

   Dieses Beispiel verwendet den HTTP-Webhooktrigger als ersten Schritt.

1. Wählen Sie im Schritt zum Hinzufügen der HTTP-Webhookaktion die Option **Neuer Schritt** aus.

   Wenn Sie zwischen Schritten eine Aktion einfügen möchten, bewegen Sie den Mauszeiger über den Pfeil zwischen den Schritten. Wählen Sie das angezeigte Pluszeichen ( **+** ) aus, und wählen Sie dann **Aktion hinzufügen** aus.

1. Geben Sie im Suchfeld des Designers „http webhook“ als Filter ein. Wählen Sie in der Liste **Aktionen** die Aktion **HTTP-Webhook** aus.

   ![Auswählen einer HTTP-Webhookaktion](./media/connectors-native-webhook/select-http-webhook-action.png)

   Dieses Beispiel benennt die Aktion in „HTTP-Webhookaktion“ um, damit der Schritt über einen aussagekräftigeren Namen verfügt.

1. Geben Sie die Werte für die Parameter der HTTP-Webhookaktion an (die ähnlich wie die [Parameter des Webhooktriggers](../logic-apps/logic-apps-workflow-actions-triggers.md##http-webhook-trigger) sind), die Sie für die Aufrufe zum Abonnieren und Kündigen eines Abonnements verwenden möchten:

   ![Eingeben der Parameter für die HTTP-Webhookaktion](./media/connectors-native-webhook/http-webhook-action-parameters.png)

   Während der Laufzeit ruft die Logik-App beim Ausführen dieser Aktion den Abonnementendpunkt auf. Ihre Logik-App hält dann den Workflow an und wartet darauf, dass der Zieldienst eine `HTTP POST`-Anforderung an die Rückruf-URL sendet. Wenn die Aktion erfolgreich abgeschlossen wird, kündigt die Aktion das Abonnement des Endpunkts, und Ihre Logik-App setzt die Ausführung des Workflows fort.

1. Öffnen Sie zum Hinzufügen weiterer verfügbarer Parameter die Liste **Neuen Parameter hinzufügen**, und wählen Sie die gewünschten Parameter aus.

   Weitere Informationen zu verfügbaren Authentifizierungstypen für HTTP-Webhooks finden Sie unter [Authentifizieren von HTTP-Triggern und -Aktionen](../logic-apps/logic-apps-workflow-actions-triggers.md#connector-authentication).

1. Speichern Sie die Logik-App unbedingt, wenn Sie fertig sind. Wählen Sie auf der Symbolleiste des Designers **Speichern** aus.

## <a name="connector-reference"></a>Connector-Referenz

Weitere Informationen zu Triggern und Aktionsparametern, die einander ähnlich sind, finden Sie unter [HTTP-Webhookparameter](../logic-apps/logic-apps-workflow-actions-triggers.md##http-webhook-trigger).

### <a name="output-details"></a>Ausgabedetails

Hier finden Sie weitere Informationen zu den Ausgaben aus einem HTTP-Webhooktrigger oder einer -aktion, die diese Informationen zurückgeben:

| Eigenschaftenname | type | BESCHREIBUNG |
|---------------|------|-------------|
| headers | object | Die Header aus der Anforderung |
| body | object | JSON-Objekt | Das Objekt mit dem Inhalt des Texts aus der Anforderung |
| status code | int | Der Statuscode aus der Anforderung |
|||

| Statuscode | BESCHREIBUNG |
|-------------|-------------|
| 200 | OK |
| 202 | Zulässig |
| 400 | Ungültige Anforderung |
| 401 | Nicht autorisiert |
| 403 | Verboten |
| 404 | Nicht gefunden |
| 500 | Interner Serverfehler. Unbekannter Fehler. |
|||

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu anderen [Logic Apps-Connectors](../connectors/apis-list.md)
