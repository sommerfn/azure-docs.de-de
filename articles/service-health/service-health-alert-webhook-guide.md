---
title: Konfigurieren von Azure-Dienstintegritätsbenachrichtigungen für vorhandene Problemverwaltungssysteme mit einem Webhook
description: Erhalten Sie personalisierte Benachrichtigungen zu Service Health-Ereignissen an Ihr vorhandenes Problemverwaltungssystem.
author: stephbaron
ms.author: stbaron
ms.topic: conceptual
ms.service: service-health
ms.workload: Supportability
ms.date: 3/27/2018
ms.openlocfilehash: 3e9a564310d750e63c9cf81d19f698e75712d09a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67067189"
---
# <a name="configure-health-notifications-for-existing-problem-management-systems-using-a-webhook"></a>Konfigurieren von Integritätsbenachrichtigungen für vorhandene Problemverwaltungssysteme mit einem Webhook

In diesem Artikel wird beschrieben, wie Sie Ihre Service Health-Warnungen zum Senden von Daten über Webhooks an Ihr vorhandenes Benachrichtigungssystem konfigurieren.

Heute können Sie Service Health-Warnungen so konfigurieren, dass Sie per SMS oder E-Mail-benachrichtigt werden, wenn Sie von einem Azure Service-Vorfall betroffen sind.
Allerdings ist möglicherweise bereits ein externes Benachrichtigungssystem vorhanden, das Sie verwenden möchten.
In diesem Dokument werden die wichtigsten Bestandteile der Webhooknutzlast beschrieben, und Sie erfahren, wie Sie benutzerdefinierte Warnungen erstellen, um benachrichtigt zu werden, wenn der Dienstprobleme auftreten, die Sie betreffen.

Wenn Sie eine vorkonfigurierte Integration verwenden möchten, ziehen Sie folgende Artikel zurate:
* [Konfigurieren von Warnungen mit ServiceNow](service-health-alert-webhook-servicenow.md)
* [Konfigurieren von Warnungen mit PagerDuty](service-health-alert-webhook-pagerduty.md)
* [Konfigurieren von Warnungen mit OpsGenie](service-health-alert-webhook-opsgenie.md)

### <a name="watch-an-introductory-video"></a>Sehen Sie sich das Einführungsvideo an

>[!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2OtUV]

## <a name="configuring-a-custom-notification-using-the-service-health-webhook-payload"></a>Konfigurieren einer benutzerdefinierten Benachrichtigung mit der Service Health-Webhooknutzlast
Wenn Sie eine eigene benutzerdefinierte Webhookintegration einrichten möchten, müssen Sie die während der Service Health-Benachrichtigungen gesendete JSON-Nutzlast analysieren.

[Hier finden Sie ein Beispiel](../azure-monitor/platform/activity-log-alerts-webhook.md), wie die `ServiceHealth`-Webhooknutzlast aussieht.

Mit einem Blick auf `context.eventSource == "ServiceHealth"` können Sie erkennen, dass dies eine Service Health-Warnung ist. Die folgenden Eigenschaften sind dabei die wichtigsten:
 * `data.context.activityLog.status`
 * `data.context.activityLog.level`
 * `data.context.activityLog.subscriptionId`
 * `data.context.activityLog.properties.title`
 * `data.context.activityLog.properties.impactStartTime`
 * `data.context.activityLog.properties.communication`
 * `data.context.activityLog.properties.impactedServices`
 * `data.context.activityLog.properties.trackingId`

## <a name="creating-a-direct-link-to-the-service-health-dashboard-for-an-incident"></a>Erstellen eines direkten Links zum Service Health-Dashboard für einen Vorfall
Sie können einen direkten Link zu Ihrem Service Health-Dashboard auf dem Desktop oder mobilen Gerät erstellen, indem Sie eine spezialisierte URL generieren. Erstellen Sie den Link unter Verwendung der `trackingId` sowie der ersten und letzten drei Ziffern Ihrer `subscriptionId` wie folgt:
```
https://app.azure.com/h/<trackingId>/<first and last three digits of subscriptionId>
```

Wenn Ihre `subscriptionId` z.B. `bba14129-e895-429b-8809-278e836ecdb3` lautet und Ihre `trackingId` `0DET-URB`, dann sieht Ihre Service Health-URL folgendermaßen aus:

```
https://app.azure.com/h/0DET-URB/bbadb3
```

## <a name="using-the-level-to-detect-the-severity-of-the-issue"></a>Erkennen des Schweregrads des Problems anhand der Eigenschaft „Level“
Vom niedrigsten Schweregrad zum höchsten Schweregrad kann die Eigenschaft `level` in der Nutzlast folgende Werte annehmen: `Informational`, `Warning`, `Error` und `Critical`.

## <a name="parsing-the-impacted-services-to-understand-the-full-scope-of-the-incident"></a>Analysieren der betroffenen Dienste, um das komplette Ausmaß des Vorfalls zu verstehen
Service Health-Warnungen informieren Sie über Probleme über mehrere Regionen und Dienste hinweg. Um die vollständigen Details zu erhalten, müssen Sie den Wert von `impactedServices` analysieren.
Der enthaltene Inhalt ist eine [JSON-Zeichenfolge mit Escapezeichen](https://json.org/); wenn ohne Escapezeichen, dann enthält es ein anderes JSON-Objekt, das normal analysiert werden kann.

```json
{"data.context.activityLog.properties.impactedServices": "[{\"ImpactedRegions\":[{\"RegionName\":\"Australia East\"},{\"RegionName\":\"Australia Southeast\"}],\"ServiceName\":\"Alerts & Metrics\"},{\"ImpactedRegions\":[{\"RegionName\":\"Australia Southeast\"}],\"ServiceName\":\"App Service\"}]"}
```

Wird zu:

```json
[
   {
      "ImpactedRegions":[
         {
            "RegionName":"Australia East"
         },
         {
            "RegionName":"Australia Southeast"
         }
      ],
      "ServiceName":"Alerts & Metrics"
   },
   {
      "ImpactedRegions":[
         {
            "RegionName":"Australia Southeast"
         }
      ],
      "ServiceName":"App Service"
   }
]
```

Dies zeigt, dass es Probleme mit „Warnungen & Metriken“ in Australien, Osten und Südosten, sowie Probleme mit „App Service“ in Australien, Südosten, gibt.


## <a name="testing-your-webhook-integration-via-an-http-post-request"></a>Testen der Webhookintegration über eine HTTP POST-Anforderung
1. Erstellen Sie die Service Health-Nutzlast, die Sie senden möchten. Eine Service Health-Beispielwebhook-Nutzlast finden Sie unter [Webhooks für Azure-Aktivitätsprotokollwarnungen](../azure-monitor/platform/activity-log-alerts-webhook.md).

2. Erstellen Sie eine HTTP POST-Anforderung, indem Sie wie folgt vorgehen:

    ```
    POST        https://your.webhook.endpoint

    HEADERS     Content-Type: application/json

    BODY        <service health payload>
    ```
3. Sie sollten eine `2XX - Successful`-Antwort erhalten.

4. Wechseln Sie zu [PagerDuty](https://www.pagerduty.com/), um zu überprüfen, ob Ihre Integration erfolgreich eingerichtet wurde.

## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zum [Webhookschema für Aktivitätsprotokollwarnungen](../azure-monitor/platform/activity-log-alerts-webhook.md). 
- Weitere Informationen zu [Dienstintegritätsbenachrichtigungen](../azure-monitor/platform/service-notifications.md).
- Weitere Informationen zu [Aktionsgruppen](../azure-monitor/platform/action-groups.md).