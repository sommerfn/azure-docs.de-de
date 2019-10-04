---
title: Konfigurieren von Azure Service Health-Benachrichtigungen für vorhandene Problemverwaltungssysteme mit einem Webhook
description: Senden Sie personalisierte Benachrichtigungen zu Service Health-Ereignissen an Ihr vorhandenes Problemverwaltungssystem.
author: stephbaron
ms.author: stbaron
ms.topic: conceptual
ms.service: service-health
ms.workload: Supportability
ms.date: 3/27/2018
ms.openlocfilehash: 8f84b43519c197797b39397cfd15c4f90444177c
ms.sourcegitcommit: 470041c681719df2d4ee9b81c9be6104befffcea
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2019
ms.locfileid: "67854379"
---
# <a name="use-a-webhook-to-configure-health-notifications-for-problem-management-systems"></a>Konfigurieren von Integritätsbenachrichtigungen für vorhandene Problemverwaltungssysteme mit einem Webhook

In diesem Artikel wird beschrieben, wie Sie Azure Service Health-Warnungen zum Senden von Daten über Webhooks an Ihr vorhandenes Benachrichtigungssystem konfigurieren.

Sie können Service Health-Warnungen so konfigurieren, dass Sie per SMS oder E-Mail benachrichtigt werden, wenn Sie von einem Azure-Dienstvorfall betroffen sind.

Möglicherweise verfügen Sie aber bereits über ein externes Benachrichtigungssystem, das Sie lieber verwenden möchten. In diesem Artikel werden die wichtigsten Teile der Webhooknutzlast identifiziert. Außerdem wird beschrieben, wie Sie benutzerdefinierte Warnungen erstellen, damit Sie benachrichtigt werden, wenn relevante Dienstprobleme auftreten.

Wenn Sie eine vorkonfigurierte Integration verwenden möchten, lesen Sie die folgenden Artikel:
* [Konfigurieren von Warnungen mit ServiceNow](service-health-alert-webhook-servicenow.md)
* [Konfigurieren von Warnungen mit PagerDuty](service-health-alert-webhook-pagerduty.md)
* [Konfigurieren von Warnungen mit OpsGenie](service-health-alert-webhook-opsgenie.md)

**Sehen Sie sich ein Einführungsvideo an:**

>[!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2OtUV]

## <a name="configure-a-custom-notification-by-using-the-service-health-webhook-payload"></a>Konfigurieren einer benutzerdefinierten Benachrichtigung mit der Service Health-Webhooknutzlast
Wenn Sie Ihre eigene benutzerdefinierte Webhookintegration einrichten möchten, müssen Sie die über die Service Health-Benachrichtigung gesendete JSON-Nutzlast analysieren.

[Hier finden Sie ein Beispiel](../azure-monitor/platform/activity-log-alerts-webhook.md) für die `ServiceHealth`-Webhooknutzlast.

Durch einen Blick auf `context.eventSource == "ServiceHealth"` können Sie bestätigen, dass dies eine Service Health-Warnung ist. Die folgenden Eigenschaften sind die relevantesten:
- **data.context.activityLog.status**
- **data.context.activityLog.level**
- **data.context.activityLog.subscriptionId**
- **data.context.activityLog.properties.title**
- **data.context.activityLog.properties.impactStartTime**
- **data.context.activityLog.properties.communication**
- **data.context.activityLog.properties.impactedServices**
- **data.context.activityLog.properties.trackingId**

## <a name="create-a-link-to-the-service-health-dashboard-for-an-incident"></a>Erstellen eines Links zum Service Health-Dashboard für einen Vorfall
Sie können einen direkten Link zu Ihrem Service Health-Dashboard auf einem Desktop oder mobilen Gerät erstellen, indem Sie eine spezialisierte URL generieren. Verwenden Sie die *Nachverfolgungs-ID* (trackingId) und die ersten drei und die letzten drei Ziffern Ihrer *Abonnement-ID* (subscriptionId) im folgenden Format:

https<i></i>://app.azure.com/h/ *&lt;Nachverfolgungs-ID&gt;* / *&lt;die ersten drei und die letzten drei Ziffern der Abonnement-ID&gt;*

Wenn Ihre *Abonnement-ID* (subscriptionId) z. B. „bba14129-e895-429b-8809-278e836ecdb3“ und Ihre *Nachverfolgungs-ID* (trackingId) „0DET-URB“ lautet, sieht Ihre Service Health-URL folgendermaßen aus:

https<i></i>://app.azure.com/h/0DET-URB/bbadb3

## <a name="use-the-level-to-detect-the-severity-of-the-issue"></a>Erkennen des Schweregrads des Problems anhand der Level-Eigenschaft
Vom niedrigsten zum höchsten Schweregrad kann die **level**-Eigenschaft in der Nutzlast den Wert *Informativ*, *Warnung*, *Fehler* oder *Kritisch* aufweisen.

## <a name="parse-the-impacted-services-to-determine-the-incident-scope"></a>Analysieren der betroffenen Dienste zum Ermitteln des Ausmaßes des Vorfalls
Service Health-Warnungen informieren Sie über Probleme über mehrere Regionen und Dienste hinweg. Um die vollständigen Details zu erhalten, müssen Sie den Wert von `impactedServices` analysieren.

Der enthaltene Inhalt ist eine [JSON](https://json.org/)-Zeichenfolge mit Escapezeichen, die ohne Escapezeichen ein anderes JSON-Objekt enthält, das normal analysiert werden kann. Beispiel:

```json
{"data.context.activityLog.properties.impactedServices": "[{\"ImpactedRegions\":[{\"RegionName\":\"Australia East\"},{\"RegionName\":\"Australia Southeast\"}],\"ServiceName\":\"Alerts & Metrics\"},{\"ImpactedRegions\":[{\"RegionName\":\"Australia Southeast\"}],\"ServiceName\":\"App Service\"}]"}
```

wird zu:

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

In diesem Beispiel wird gezeigt, dass Probleme bestehen bei:
- „Warnungen & Metriken“ in „Australien, Osten“ und „Australien, Südosten“.
- „App Service“ in „Australien, Südosten“.

## <a name="test-your-webhook-integration-via-an-http-post-request"></a>Testen der Webhookintegration über eine HTTP POST-Anforderung

Folgen Sie diesen Schritten:

1. Erstellen Sie die Service Health-Nutzlast, die Sie senden möchten. Ein Beispiel für eine Service Health-Webhooknutzlast finden Sie unter [Webhooks für Azure-Aktivitätsprotokollwarnungen](../azure-monitor/platform/activity-log-alerts-webhook.md).

1. Erstellen Sie eine HTTP POST-Anforderung, indem Sie wie folgt vorgehen:

    ```
    POST        https://your.webhook.endpoint

    HEADERS     Content-Type: application/json

    BODY        <service health payload>
    ```
   Sie sollten eine Antwort vom Typ „2xx“ (erfolgreich) erhalten.

1. Wechseln Sie zu [PagerDuty](https://www.pagerduty.com/), um zu überprüfen, ob Ihre Integration erfolgreich eingerichtet wurde.

## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zum [Webhookschema für Aktivitätsprotokollwarnungen](../azure-monitor/platform/activity-log-alerts-webhook.md). 
- Weitere Informationen zu [Dienstintegritätsbenachrichtigungen](../azure-monitor/platform/service-notifications.md).
- Weitere Informationen zu [Aktionsgruppen](../azure-monitor/platform/action-groups.md).