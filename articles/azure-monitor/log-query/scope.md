---
title: Protokollabfragebereich in Azure Monitor Log Analytics | Microsoft-Dokumentation
description: Dieser Artikel beschreibt den Bereich und den Zeitbereich für Protokollabfragen in Azure Monitor Log Analytics.
ms.service: azure-monitor
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 06/25/2019
ms.openlocfilehash: 03e5e1bc79702a979be352095bb4833a7f5fe1c6
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2019
ms.locfileid: "72900232"
---
# <a name="log-query-scope-and-time-range-in-azure-monitor-log-analytics"></a>Protokollabfragebereich und Zeitbereich in Azure Monitor Log Analytics
Wenn Sie eine [Protokollabfrage](log-query-overview.md) in [Log Analytics im Azure-Portal](get-started-portal.md) ausführen, sind die von der Abfrage ausgewerteten Daten von dem von Ihnen ausgewählten Bereich und Zeitbereich abhängig. In diesem Artikel wird der Bereich, der Zeitbereich und das Festlegen dieser Bereiche je nach Ihren Anforderungen beschrieben. Außerdem erfahren Sie mehr über das Verhalten von verschiedenen Bereichstypen.


## <a name="query-scope"></a>Abfragebereich
Der Abfragebereich definiert die Datensätze, die von der Abfrage ausgewertet werden. Dazu gehören in der Regel alle Datensätze in einem einzigen Log Analytics-Arbeitsbereich oder einer Application Insights-Anwendung. Log Analytics ermöglicht auch das Festlegen eines Bereichs für eine bestimmte überwachte Azure-Ressource. So können sich Besitzer einer Ressource auf ihre Daten konzentrieren, auch wenn diese Ressource auf mehreren Arbeitsbereichen schreibt.

Der Bereich wird immer oben links im Log Analytics-Fenster angezeigt. Ein Symbol gibt an, ob es sich bei dem Bereich um einen Log Analytics-Arbeitsbereich oder eine Application Insights-Anwendung handelt. Kein Symbol gibt eine andere Azure-Ressource an.

![`Scope`](media/scope/scope.png)

Der Bereich wird von der Methode bestimmt, die Sie zum Starten von Log Analytics verwenden. In einigen Fällen können Sie den Bereich ändern, indem Sie auf diesen klicken. In der folgenden Tabelle werden die jeweiligen verschiedenen Bereichstypen und Details aufgelistet.

| Abfragebereich | Datensätze in diesem Bereich | Auswählen | Ändern des Bereichs |
|:---|:---|:---|:---|
| Log Analytics-Arbeitsbereich | Alle Datensätze im Log Analytics-Arbeitsbereich. | Klicken Sie im Menü **Azure Monitor** oder im Menü **Log Analytics-Arbeitsbereiche** auf **Protokolle**.  | Bereich kann zu jedem anderen Ressourcentyp geändert werden. |
| Application Insights-Anwendung | Alle Datensätze in der Application Insights-Anwendung. | Klicken Sie auf der Seite **Übersicht** von Application Insights auf **Analytics**. | Bereich kann nur in eine andere Application Insights-Anwendung geändert werden. |
| Resource group | Datensätze, die von allen Ressourcen in der Ressourcengruppe erstellt werden. Diese enthalten möglicherweise Daten aus mehreren Log Analytics-Arbeitsbereichen. | Klicken Sie im Ressourcengruppenmenü auf **Protokolle**. | Bereich kann nicht geändert werden.|
| Subscription | Datensätze, die von allen Ressourcen im Abonnement erstellt werden. Diese enthalten möglicherweise Daten aus mehreren Log Analytics-Arbeitsbereichen. | Klicken Sie im Abonnementmenü auf **Protokolle**.   | Bereich kann nicht geändert werden. |
| Andere Azure-Ressourcen | Datensätze, die von der Ressource erstellt werden. Diese enthalten möglicherweise Daten aus mehreren Log Analytics-Arbeitsbereichen.  | Wählen Sie im Ressourcenmenü auf **Protokolle**.<br>OR<br>Klicken Sie im Menü **Azure Monitor** auf **Protokolle**, und wählen Sie dann einen neuen Bereich aus. | Bereich kann nur in denselben Ressourcentyp geändert werden. |

### <a name="limitations-when-scoped-to-a-resource"></a>Einschränkungen bei Begrenzungen auf eine Ressource

Wenn der Abfragebereich ein Log Analytics-Arbeitsbereich oder eine Application Insights-Anwendung ist, sind alle Optionen im Portal und alle Abfragebefehle verfügbar. Wenn der Abfragebereich jedoch auf eine Ressource beschränkt ist, sind die folgenden Optionen im Portal nicht verfügbar, da sie einem einzelnen Arbeitsbereich oder einer Anwendung zugeordnet sind:

- Speichern
- Abfrage-Explorer
- Neue Warnungsregel

Sie können die folgenden Befehle in einer Abfrage nicht verwenden, wenn diese einer Ressource zugeordnet ist, da der Abfragebereich bereits Arbeitsbereiche mit Daten für diese Ressource(n) enthält:

- [app](app-expression.md)
- [workspace](workspace-expression.md)
 

## <a name="query-limits"></a>Abfragegrenzwerte
Möglicherweise bestehen Ihre Geschäftsanforderungen an eine Azure-Ressource darin, Daten in mehrere Log Analytics-Arbeitsbereiche zu schreiben. Der Arbeitsbereich muss sich nicht in derselben Region wie die Ressource befinden, und in einem einzelnen Arbeitsbereich könnten Daten aus Ressourcen in einer Vielzahl von Regionen gesammelt werden.  

Das Festlegen des Bereichs auf eine Ressource oder eine Gruppe von Ressourcen ist ein besonderes Leistungsfeature von Log Analytics, da dies Ihnen die automatische Konsolidierung verteilter Daten in einer einzelnen Abfrage ermöglicht. Es kann die Leistung jedoch erheblich beeinträchtigen, wenn Daten aus Arbeitsbereichen in mehreren Azure-Regionen abgerufen werden müssen.

Log Analytics schützt vor übermäßigem Mehraufwand durch Abfragen, die Arbeitsbereiche in mehreren Regionen umfassen, indem eine Warnung oder Fehlermeldung ausgegeben wird, wenn eine bestimmte Anzahl von Regionen verwendet wird. Ihre Abfrage empfängt eine Warnung, wenn der Bereich Arbeitsbereiche in mindestens 5 Regionen enthält. Sie wird zwar weiterhin ausgeführt, aber ihre Ausführung kann übermäßig lange dauern.

![Abfragewarnung](media/scope/query-warning.png)

Die Ausführung Ihrer Abfrage wird blockiert, wenn der Bereich Arbeitsbereiche in mindestens 20 Regionen umfasst. In diesem Fall werden Sie aufgefordert, die Anzahl der Arbeitsbereichsregionen zu verringern und erneut zu versuchen, die Abfrage auszuführen. In der Dropdownliste werden alle Regionen im Bereich der Abfrage angezeigt, und Sie sollten die Anzahl der Regionen verringern, bevor Sie versuchen, die Abfrage erneut auszuführen.

![Fehler bei Abfrage](media/scope/query-failed.png)


## <a name="time-range"></a>Zeitbereich
Der Zeitbereich gibt, basierend auf dem Zeitpunkt der Erstellung des Datensatzes, die Gruppe der für die Abfrage auszuwertenden Datensätzen an. Er wird durch eine in der folgenden Tabelle angegebene Standardeigenschaft in jedem Datensatz im Arbeitsbereich oder in der Anwendung definiert.

| Location | Eigenschaft |
|:---|:---|
| Log Analytics-Arbeitsbereich          | TimeGenerated |
| Application Insights-Anwendung | timestamp     |

Legen Sie den Zeitbereich fest, indem Sie diesen aus der Zeitauswahl im oberen Bereich des Fensters „Log Analytics“ auswählen.  Sie können einen vordefinierten Zeitraum auswählen oder auf **Benutzerdefiniert** klicken, um einen bestimmten Zeitbereich anzugeben.

![Zeitauswahl](media/scope/time-picker.png)

Wenn Sie in der Abfrage einen Filter festlegen, der die in der obigen Tabelle gezeigte standardmäßige Zeiteigenschaft verwendet, wird die Zeitauswahl in **Set in query** (In Abfrage festlegen) geändert und die Zeitauswahl deaktiviert. In diesem Fall ist es am effizientesten, den Filter am Anfang der Abfrage einzufügen, sodass alle nachfolgenden Verarbeitungen nur mit den gefilterten Datensätzen arbeiten muss.

![Gefilterte Abfrage](media/scope/query-filtered.png)

Wenn Sie die Befehle [workspace](workspace-expression.md) oder [app](app-expression.md) zum Abrufen von Daten aus einem anderen Arbeitsbereich oder einer Anwendung verwenden, verhält sich die Zeitauswahl möglicherweise anders. Wenn es sich beim Bereich um einen Log Analytics-Arbeitsbereich handelt und Sie **app** verwenden oder es sich beim Bereich um eine Application Insights-Anwendung handelt und Sie **workspace** verwenden, versteht Log Analytics möglicherweise nicht, dass die im Filter verwendete Eigenschaft den Zeitfilter bestimmen soll.

Im folgenden Beispiel ist der Bereich auf einen Log Analytics-Arbeitsbereich festgelegt.  Die Abfrage verwendet den **workspace**-Befehl zum Abrufen von Daten aus einem anderen Log Analytics-Arbeitsbereich. Die Zeitauswahl wird in **Set in query** (In Abfrage festlegen) geändert, da ein Filter erkannt wird, der die erwartete **TimeGenerated**-Eigenschaft verwendet.

![Abfrage mit dem workspace-Befehl](media/scope/query-workspace.png)

Wenn die Abfrage jedoch den Befehl **app** zum Abrufen von Daten aus einer Application Insights-Anwendung verwendet, erkennt Log Analytics die **timestamp**-Eigenschaft im Filter nicht, und die Zeitauswahl bleibt unverändert. In diesem Fall werden beide Filter angewendet. Im Beispiel sind nur Datensätze in der Abfrage enthalten, die in den letzten 24 Stunden erstellt wurden, obwohl in der **where**-Klausel sieben Tage angegeben sind.

![Abfrage mit dem app-Befehl](media/scope/query-app.png)

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen erhalten Sie im [Tutorial zum Verwenden von Log Analytics im Azure-Portal](get-started-portal.md).
- Weitere Informationen erhalten Sie im [Tutorial zum Schreiben von Abfragen](get-started-queries.md).