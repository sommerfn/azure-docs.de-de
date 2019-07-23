---
title: Erstellen einer neuen Azure Application Insights-Ressource | Microsoft Docs
description: Richten Sie manuell die Application Insights-Überwachung für eine neue Liveanwendung ein.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 878b007e-161c-4e36-8ab2-3d7047d8a92d
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/10/2019
ms.author: mbullwin
ms.openlocfilehash: 9da52e5a9dfa3b55431d66ed3162172226f71a40
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67073301"
---
# <a name="create-an-application-insights-resource"></a>Erstellen einer Application Insights-Ressource

Azure Application Insights zeigt Daten über Ihre Anwendung in einer Microsoft Azure-*Ressource* an. Die Erstellung einer neuen Ressource gehört daher zur [Einrichtung von Application Insights für das Überwachen einer neuen Anwendung][start]. Nach dem Erstellen der neuen Ressource erhalten Sie den Instrumentierungsschlüssel, den Sie zum Konfigurieren des Application Insights SDK verwenden. Der Instrumentierungsschlüssel verknüpft Ihre Telemetriedaten mit der Ressource.

## <a name="sign-in-to-microsoft-azure"></a>Anmelden bei Microsoft Azure

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="create-an-application-insights-resource"></a>Erstellen einer Application Insights-Ressource

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und erstellen Sie eine Application Insights-Ressource:

![Klicken Sie links oben auf das Pluszeichen (+). Auswählen von „Entwicklertools“ und „Application Insights“](./media/create-new-resource/new-app-insights.png)

   | Einstellungen        |  Wert           | BESCHREIBUNG  |
   | ------------- |:-------------|:-----|
   | **Name**      | Global eindeutiger Wert | Der Name, der die zu überwachende App identifiziert |
   | **Ressourcengruppe**     | myResourceGroup      | Der Name der neuen oder vorhandenen Ressourcengruppe, die Application Insights-Daten hosten soll |
   | **Location** | USA (Ost) | Wählen Sie einen Standort in Ihrer Nähe oder in der Nähe des Standorts aus, an dem Ihre App gehostet wird. |

Geben Sie die entsprechenden Werte in die erforderlichen Felder ein, und wählen Sie dann **Überprüfen + erstellen** aus.

![Geben Sie Werte in die erforderlichen Felder ein, und wählen Sie dann „Überprüfen + erstellen“ aus.](./media/create-new-resource/review-create.png)

Wenn Ihre App erstellt wurde, wird ein neuer Bereich geöffnet. In diesem Bereich werden die Leistungs- und Nutzungsdaten Ihrer überwachten Anwendung angezeigt. 

## <a name="copy-the-instrumentation-key"></a>Kopieren des Instrumentationsschlüssels

Der Instrumentierungsschlüssel identifiziert die Ressource, der Sie Ihre Telemetriedaten zuordnen möchten. Sie müssen den Instrumentierungsschlüssel kopieren, um ihn in Ihrem Anwendungscode hinzuzufügen.

![Auswählen und Kopieren des Instrumentierungsschlüssels](./media/create-new-resource/instrumentation-key.png)

## <a name="install-the-sdk-in-your-app"></a>Installieren des SDK in Ihrer App

Installieren Sie das Application Insights-SDK in Ihrer App. Dieser Schritt hängt stark von der Art der Anwendung ab.

Konfigurieren Sie das [SDK, das Sie in Ihrer Anwendung installieren][start] mithilfe des Instrumentierungsschlüssels.

Das SDK enthält die Standardmodule, die Telemetriedaten senden, ohne dass Sie zusätzlichen Code schreiben müssen. Um Benutzeraktionen nachzuverfolgen oder Probleme im Detail zu diagnostizieren, [verwenden Sie die API][api] zum Senden eigener Telemetriedaten.

## <a name="creating-a-resource-automatically"></a>Automatisches Erstellen einer Ressource
Sie können ein [PowerShell-Skript](../../azure-monitor/app/powershell.md) schreiben, um eine Ressource automatisch zu erstellen.

## <a name="next-steps"></a>Nächste Schritte
* [Diagnosesuche](../../azure-monitor/app/diagnostic-search.md)
* [Untersuchen von Metriken](../../azure-monitor/app/metrics-explorer.md)
* [Schreiben von Analytics-Abfragen](../../azure-monitor/app/analytics.md)

<!--Link references-->

[api]: ../../azure-monitor/app/api-custom-events-metrics.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[metrics]: ../../azure-monitor/app/metrics-explorer.md
[start]: ../../azure-monitor/app/app-insights-overview.md