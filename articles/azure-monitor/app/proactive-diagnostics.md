---
title: Intelligente Erkennung in Azure Application Insights | Microsoft Docs
description: Application Insights führt eine automatische umfassende Analyse Ihrer App-Telemetrie durch und warnt Sie vor potenziellen Problemen.
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: mrbullwinkle
ms.author: mbullwin
ms.date: 02/07/2019
ms.openlocfilehash: ae9af5721e53277f6c939840721ca50ea67ca51e
ms.sourcegitcommit: 8e271271cd8c1434b4254862ef96f52a5a9567fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72818814"
---
# <a name="smart-detection-in-application-insights"></a>Intelligente Erkennung in Application Insights
 Die intelligente Erkennung warnt Sie automatisch vor potenziellen Leistungsproblemen und Fehleranomalien in Ihrer Webanwendung. Sie führt eine proaktive Analyse der Telemetriedaten durch, die Ihre App an [Application Insights](../../azure-monitor/app/app-insights-overview.md) sendet. Bei einem plötzlichen Anstieg der Fehlerraten oder bei ungewöhnlichen Mustern in der Client- oder Serverleistung erhalten Sie eine Warnung. Diese Funktion muss nicht konfiguriert werden. Sie wird ausgeführt, wenn Ihre Anwendung genügend Telemetriedaten sendet.

Sie können über die erhaltenen E-Mails und das Blatt für die intelligenten Erkennung auf Ergebnisse der intelligenten Erkennung zugreifen.

## <a name="review-your-smart-detections"></a>Überprüfen der Ergebnisse der intelligenten Erkennung
Sie haben zwei Möglichkeiten, Erkennungen anzuzeigen:

* **Sie erhalten eine E-Mail** von Application Insights. Hier sehen Sie ein typisches Beispiel:
  
    ![E-Mail-Warnung](./media/proactive-diagnostics/03.png)
  
    Klicken Sie auf die große Schaltfläche, um ausführlichere Informationen im Portal zu öffnen.
* **Das Blatt „Intelligente Erkennung“** in Application Insights. Wählen Sie **Intelligente Erkennung** im Menü **Untersuchen** aus, um eine Liste der aktuellen Erkennungen anzuzeigen.

![Aktuelle Erkennungen anzeigen](./media/proactive-diagnostics/04.png)

Wählen Sie eine Erkennung aus, um ihre Details anzuzeigen.

## <a name="what-problems-are-detected"></a>Welche Probleme werden erkannt?
Die intelligente Erkennung entdeckt eine Reihe von Problemen und gibt Benachrichtigungen dazu aus, z.B.:

* [Intelligente Erkennung – Fehlerabweichungen](../../azure-monitor/app/proactive-failure-diagnostics.md). Wir nutzen Machine Learning, um die voraussichtliche Rate fehlerhafter Anforderungen für Ihre App (korreliert mit Lastangaben und anderen Faktoren) festzulegen. Falls die Fehlerrate den erwarteten Rahmen überschreitet, wird eine Warnung gesendet.
* [Intelligente Erkennung – Leistungsabweichungen](../../azure-monitor/app/proactive-performance-diagnostics.md). Sie erhalten Benachrichtigungen, wenn sich die Reaktionszeit eines Vorgangs oder die Abhängigkeitsdauer im Vergleich zu den typischen Verlaufsdaten verschlechtert oder wenn für die Reaktionszeit oder die Seitenladezeit ein anomales Muster erkannt wird.   
* Allgemeine Beeinträchtigungen und Probleme wie [Verschlechterung des Schweregrads der Ablaufverfolgung](https://docs.microsoft.com/azure/azure-monitor/app/proactive-trace-severity), [Speicherverlust](https://docs.microsoft.com/azure/azure-monitor/app/proactive-potential-memory-leak), [anormaler Anstieg im Ausnahmevolume](https://docs.microsoft.com/azure/azure-monitor/app/proactive-exception-volume) und [Verdachtsmomente für ein Sicherheitsproblem](https://docs.microsoft.com/azure/azure-monitor/app/proactive-application-security-detection-pack).

(Über die Hilfelinks in den jeweiligen Benachrichtigungen gelangen Sie zu den relevanten Artikeln.)

## <a name="smart-detection-email-notifications"></a>E-Mail-Benachrichtigungen bei intelligenter Erkennung

Alle Regeln für die intelligente Erkennung, mit Ausnahme der als _Vorschauversion_ gekennzeichneten Regeln, sind standardmäßig so konfiguriert, dass bei Erkennungen E-Mail-Benachrichtigungen gesendet werden.

Zum Konfigurieren von E-Mail-Benachrichtigungen für eine bestimmte Regel für die intelligente Erkennung können Sie das Blatt **Einstellungen** für die intelligente Erkennung öffnen, und die entsprechende Regel auswählen. Dadurch wird das Blatt **Regel bearbeiten** geöffnet.

Alternativ können Sie die Konfiguration mithilfe von Azure Resource Manager-Vorlagen ändern. Weitere Informationen finden Sie unter [Verwalten von intelligenten Erkennungsregeln von Azure Application Insights mit Azure Resource Manager-Vorlagen](https://docs.microsoft.com/azure/azure-monitor/app/proactive-arm-config).

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Nächste Schritte
Mit den folgenden Diagnosetools können Sie die Telemetrie Ihrer App untersuchen:

* [Metrik-Explorer](../../azure-monitor/app/metrics-explorer.md)
* [Suchexplorer](../../azure-monitor/app/diagnostic-search.md)
* [Analytics: Leistungsfähige Abfragesprache](../../azure-monitor/log-query/get-started-portal.md)

Die intelligente Erkennungen erfolgt vollkommen automatisch. Vielleicht möchten Sie aber weitere Warnungen einrichten?

* [Einrichten von Warnungen in Application Insights](../../azure-monitor/app/alerts.md)
* [Verfügbarkeitswebtests](../../azure-monitor/app/monitor-web-app-availability.md) 

