---
title: Zustände von Azure Stream Analytics-Aufträgen
description: 'In diesem Artikel werden die vier verschiedenen Zustände eines Stream Analytics-Auftrags beschrieben: „Wird ausgeführt“, „Beendet“, „Beeinträchtigt“ und „Fehler“.'
services: stream-analytics
author: sidram
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.openlocfilehash: c533463ff544dc315142f7fb95c34c67933f9614
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2019
ms.locfileid: "68444249"
---
# <a name="azure-stream-analytics-job-states"></a>Zustände von Azure Stream Analytics-Aufträgen

Ein Stream Analytics-Auftrag kann sich in einem von vier Zuständen befinden: „Wird ausgeführt“, „Beendet“, „Beeinträchtigt“ oder „Fehler“. Den Zustand Ihres Stream Analytics-Auftrags können Sie im Azure-Portal auf der zugehörigen Seite „Übersicht“ bestimmen. 

| Zustand | Beschreibung | Empfohlene Aktionen |
| --- | --- | --- |
| **Wird ausgeführt** | Ihr in Azure ausgeführter Auftrag liest Ereignisse aus den definierten Eingabequellen, verarbeitet sie und schreibt die Ergebnisse in die konfigurierten Ausgabesenken. | Es ist eine bewährte Methode, die Leistung Ihres Auftrags durch Überwachung [wichtiger Metriken](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-set-up-alerts#scenarios-to-monitor) zu verfolgen. |
| **Beendet** | Ihr Auftrag wurde beendet und verarbeitet keine Ereignisse mehr. | Nicht verfügbar | 
| **Beeinträchtigt** | Möglicherweise gibt es kurzzeitige Probleme mit Ihren Ein- und Ausgabeverbindungen. Diese Fehler werden als vorübergehende Fehler bezeichnet, die dazu führen können, dass Ihr Auftrag den Status „Degraded“ (Beeinträchtigt) erhält. Stream Analytics versucht unverzüglich, solche Fehler zu beheben, und (binnen weniger Minuten) in den Zustand „Wird ausgeführt“ zurückzukehren. Diese Fehler können durch Netzwerkprobleme, die Nichtverfügbarkeit anderer Azure-Ressourcen, Deserialisierungsfehler usw. verursacht werden. Die Leistung Ihres Auftrags kann sich verschlechtern, wenn er sich in einem beeinträchtigten Zustand befindet.| Sie können die [Diagnose- oder Aktivitätsprotokolle](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-job-diagnostic-logs#debugging-using-activity-logs) einsehen, um mehr über die Ursache dieser vorübergehenden Fehler zu erfahren. Bei Deserialisierungsfehlern wird empfohlen, Korrekturmaßnahmen zu ergreifen, um sicherzustellen, dass Ereignisse kein falsches Format aufweisen. Wenn der Auftrag weiterhin die Ressourcenauslastungsgrenze erreicht, versuchen Sie, die Anzahl der Speichereinheiten zu erhöhen oder [Ihren Auftrag zu parallelisieren](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-parallelization). In anderen Fällen, in denen Sie keine Maßnahmen ergreifen können, versucht Stream Analytics, sich wieder in den Zustand *Wird ausgeführt* zu versetzen. <br> Sie können der Metrik [Wasserzeichenverzögerung](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-set-up-alerts#scenarios-to-monitor) entnehmen, ob die vorübergehenden Fehler die Leistung Ihres Auftrags beeinträchtigen.|
| **Fehler** | Ihr Auftrag ist auf einen kritischen Fehler gestoßen, der zu einem fehlerhaften Zustand geführt hat. Ereignisse werden weder gelesen noch verarbeitet. Laufzeitfehler sind eine häufige Ursache für Aufträge, die in einem fehlerhaften Zustand enden. | Sie können [Warnungen so konfigurieren](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-set-up-alerts#set-up-alerts-in-the-azure-portal), dass Sie benachrichtigt werden, wenn der Auftrag in den Zustand „Fehler“ wechselt. <br> <br>Sie können mithilfe von [Aktivitäts- und Diagnoseprotokollen](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-job-diagnostic-logs#debugging-using-activity-logs) debuggen, um die Ursache zu ermitteln und das Problem zu beheben.|

## <a name="next-steps"></a>Nächste Schritte
* [Einrichten von Warnungen für Azure Stream Analytics-Aufträge](stream-analytics-set-up-alerts.md)
* [In Stream Analytics verfügbare Metriken](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-monitoring#metrics-available-for-stream-analytics)
* [Problembehandlung mithilfe von Aktivitäts- und Diagnoseprotokollen](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-job-diagnostic-logs)
