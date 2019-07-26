---
title: Senden von Azure Service Health-Warnungen mit PagerDuty mit Webhooks
description: Erhalten Sie personalisierte Benachrichtigungen zu Service Health-Ereignissen an Ihre PagerDuty-Instanz.
author: stephbaron
ms.author: stbaron
ms.topic: article
ms.service: service-health
ms.date: 06/10/2019
ms.openlocfilehash: ab3bcffb6453b284c3c8bb0d0373c7155fe8ef23
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67067155"
---
# <a name="send-azure-service-health-alerts-with-pagerduty-using-webhooks"></a>Senden von Azure Service Health-Warnungen mit PagerDuty mit Webhooks

In diesem Artikel erfahren Sie, wie Sie Azure Service Health-Benachrichtigungen über PagerDuty unter Verwendung eines Webhook einrichten. Mithilfe des benutzerdefinierten Microsoft Azure-Integrationstyps von [PagerDuty](https://www.pagerduty.com/) können Sie mühelos Service Health-Warnungen zu neuen oder vorhandenen PagerDuty-Diensten hinzufügen.

## <a name="creating-a-service-health-integration-url-in-pagerduty"></a>Erstellen einer URL für die Service Health-Integration in PagerDuty
1.  Stellen Sie sicher, dass Sie Ihr [PagerDuty](https://www.pagerduty.com/)-Konto registriert haben und angemeldet sind.

1.  Navigieren Sie zum Abschnitt **Services** (Dienste) in PagerDuty.

    ![Der Abschnitt „Services“ in PagerDuty](./media/webhook-alerts/pagerduty-services-section.png)

1.  Wählen Sie **Add New Service** (Neuen Dienst hinzufügen) aus, oder öffnen Sie einen vorhandenen Dienst, den Sie eingerichtet haben.

1.  Wählen Sie unter **Integration Settings** (Integrationseinstellungen) folgendes Einstellungen aus:

    a. **Integrationstyp**: Microsoft Azure

    b. **Name der Integration**: \<Name\>

    ![„Integration Settings“ in PagerDuty](./media/webhook-alerts/pagerduty-integration-settings.png)

1.  Füllen Sie alle weiteren Pflichtfelder aus, und wählen Sie **Add** (Hinzufügen) aus.

1.  Öffnen Sie diese neue Integration und kopieren und speichern Sie die Integrations-URL (**Integration URL**).

    ![„Integration URL“ in PagerDuty](./media/webhook-alerts/pagerduty-integration-url.png)

## <a name="create-an-alert-using-pagerduty-in-the-azure-portal"></a>Erstellen einer Warnung mithilfe von PagerDuty im Azure-Portal
### <a name="for-a-new-action-group"></a>Für eine neue Aktionsgruppe:
1. Befolgen Sie die Schritte 1 bis 8 in [Erstellen einer Warnung zu einer Dienstintegritätsbenachrichtigung für eine neue Aktionsgruppe mit dem Azure-Portal](../azure-monitor/platform/alerts-activity-log-service-notifications.md).

1. Definieren Sie in der Liste der **Aktionen** Folgendes:

    a. **Aktionstyp:** *Webhook*

    b. **Details:** Die zuvor gespeicherte PagerDuty-**Integrations-URL**.

    c. **Name:** Name, Alias oder Bezeichner des Webhook.

1. Wählen Sie **Save** (Speichern) aus, wenn das Erstellen der Warnung abgeschlossen ist.

### <a name="for-an-existing-action-group"></a>Für eine vorhandene Aktionsgruppe:
1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) die Option **Überwachen** aus.

1. Wählen Sie im Abschnitt **Einstellungen** die Option **Aktionsgruppen** aus.

1. Suchen und markieren Sie die Aktionsgruppe, die Sie bearbeiten möchten.

1. Fügen Sie Folgendes zur Liste der **Aktionen** hinzu:

    a. **Aktionstyp:** *Webhook*

    b. **Details:** Die zuvor gespeicherte PagerDuty-**Integrations-URL**.

    c. **Name:** Name, Alias oder Bezeichner des Webhook.

1. Wählen Sie **Save** (Speichern) aus, wenn Sie mit dem Aktualisieren der Aktionsgruppe fertig sind.

## <a name="testing-your-webhook-integration-via-an-http-post-request"></a>Testen der Webhookintegration über eine HTTP POST-Anforderung
1. Erstellen Sie die Service Health-Nutzlast, die Sie senden möchten. Eine Service Health-Beispielwebhook-Nutzlast finden Sie unter [Webhooks für Azure-Aktivitätsprotokollwarnungen](../azure-monitor/platform/activity-log-alerts-webhook.md).

1. Erstellen Sie eine HTTP POST-Anforderung, indem Sie wie folgt vorgehen:

    ```
    POST        https://events.pagerduty.com/integration/<IntegrationKey>/enqueue

    HEADERS     Content-Type: application/json

    BODY        <service health payload>
    ```
1. Sie sollten `202 Accepted` mit einer Meldung erhalten, die die „Ereignis-ID“ enthält.

1. Wechseln Sie zu [PagerDuty](https://www.pagerduty.com/), um zu überprüfen, ob Ihre Integration erfolgreich eingerichtet wurde.

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie, wie Sie [Webhookbenachrichtigungen für vorhandene Problemverwaltungssysteme konfigurieren](service-health-alert-webhook-guide.md).
- Weitere Informationen zum [Webhookschema für Aktivitätsprotokollwarnungen](../azure-monitor/platform/activity-log-alerts-webhook.md). 
- Weitere Informationen zu [Dienstintegritätsbenachrichtigungen](../azure-monitor/platform/service-notifications.md).
- Weitere Informationen zu [Aktionsgruppen](../azure-monitor/platform/action-groups.md).