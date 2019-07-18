---
title: Einrichten von Warnungen zur Verfügbarkeit mit Azure Application Insights | Microsoft-Dokumentation
description: Richten Sie Webtests in Application Insights ein. Erhalten Sie Benachrichtigungen, wenn eine Website nicht mehr zur Verfügung steht oder langsam reagiert.
services: application-insights
documentationcenter: ''
author: lgayhardt
manager: carmonm
ms.assetid: 46dc13b4-eb2e-4142-a21c-94a156f760ee
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/19/2019
ms.reviewer: sdash
ms.author: lagayhar
ms.openlocfilehash: cc022f91d4b4fec42929769df8c979320548a1f9
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2019
ms.locfileid: "67304757"
---
# <a name="availability-alerts"></a>Verfügbarkeitswarnungen

[Azure Application Insights](../../azure-monitor/app/app-insights-overview.md) sendet regelmäßig Webanforderungen von verschiedenen Punkten auf der ganzen Welt an Ihre Anwendung. Außerdem kann dieser Dienst Sie warnen, wenn Ihre Anwendung nicht oder zu langsam reagieren sollte.

## <a name="enable-alerts"></a>Aktivieren von Warnungen

Warnungen werden jetzt zwar standardmäßig automatisch aktiviert, aber Sie müssen zunächst einen Verfügbarkeitstest erstellen, damit diese vollständig konfiguriert werden können.

![Erstellen einer Erfahrung](./media/availability-alerts/create-test.png)

> [!NOTE]
>  Mit den [neuen einheitlichen Warnungen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts), **müssen** der Schweregrad der Warnungsregel sowie die Benachrichtigungseinstellungen mit [Aktionsgruppen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups) in der Warnungserfahrung konfiguriert werden. Ohne die folgenden Schritten erhalten Sie nur portalinterne Benachrichtigungen.

1. Nach dem Speichern des Verfügbarkeitstests, klicken Sie auf der Registerkarte „Details“ auf die Auslassungspunkte neben dem Test, den Sie gerade erstellt haben. Klicken Sie auf „Warnung bearbeiten“.

   ![Nach dem Speichern bearbeiten](./media/availability-alerts/edit-alert.png)

2. Legen Sie den gewünschten Schweregrad, die Regelbeschreibung und vor allem die Aktionsgruppe fest, die über die Benachrichtigungseinstellungen verfügt, die Sie für diese Warnungsregel verwenden möchten.

   ![Nach dem Speichern bearbeiten](./media/availability-alerts/set-action-group.png)

### <a name="alert-on-x-out-of-y-locations-reporting-failures"></a>Warnung bei X von Y Standorten, die Fehler melden

Die Warnungsregel „X von Y Standorten“ ist in der [neuen einheitlichen Warnungserfahrung](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts) standardmäßig aktiviert, wenn Sie einen neuen Verfügbarkeitstest erstellen. Sie können sich dagegen entscheiden, indem Sie die „klassische“ Option auswählen oder die Warnungsregel deaktivieren.

> [!NOTE]
> Konfigurieren Sie die Aktionsgruppen so, dass sie Benachrichtigungen empfangen, wenn die Warnung ausgelöst wird, indem Sie die oben beschriebenen Schritte ausführen. Ohne diese Schritte erhalten Sie nur portalinterne Benachrichtigungen, wenn die Regel ausgelöst wird.
>

### <a name="alert-on-availability-metrics"></a>Warnung bei Verfügbarkeitsmetriken

Mithilfe der [neuen einheitlichen Warnungen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts) können Sie sowohl bei segmentierter aggregierter Verfügbarkeit als auch bei Testdauermetriken warnen:

1. Wählen Sie eine Application Insights-Ressource in der Metrikenerfahrung aus, und wählen Sie eine Verfügbarkeitsmetrik aus:

    ![Auswahl einer Verfügbarkeitsmetrik](./media/availability-alerts/select-metric.png)

2. Wenn Sie die Option „Warnungen“ über das Menü konfigurieren, gelangen Sie zur neuen Erfahrung, wo Sie bestimmte Tests oder Standorte auswählen können, für die Warnungsregeln eingerichtet werden sollen. Sie können hier außerdem die Aktionsgruppen für diese Warnungsregel konfigurieren.

### <a name="alert-on-custom-analytics-queries"></a>Warnung bei benutzerdefinierten Analyseabfragen

Mithilfe der [neuen einheitlichen Warnungen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts) können Sie bei [benutzerdefinierten Protokollabfragen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log) warnen. Mit benutzerdefinierten Abfragen können Sie bei jeder beliebigen Bedingung warnen, die Ihnen hilft, das zuverlässigste Signal für Verfügbarkeitsprobleme abzurufen. Dies ist insbesondere anwendbar, wenn Sie benutzerdefinierte Verfügbarkeitsergebnisse mithilfe des TrackAvailability SDKs senden. 

> [!Tip]
> Die Metriken zu Verfügbarkeitsdaten umfassen alle benutzerdefinierten Verfügbarkeitsergebnisse, die Sie möglicherweise durch Aufrufen unseres TrackAvailability SDKs übermitteln. Sie können die Unterstützung für Warnungen bei Metriken verwenden, um bei benutzerdefinierten Verfügbarkeitsergebnissen zu warnen.
>

## <a name="troubleshooting"></a>Problembehandlung

[Artikel zur Problembehandlung](troubleshoot-availability.md) (Dedicated)

## <a name="next-steps"></a>Nächste Schritte

* [Multi-step web tests (Mehrstufige Webtests)](availability-multistep.md)
* [Url ping web tests (URL-Pingtests)](monitor-web-app-availability.md)

