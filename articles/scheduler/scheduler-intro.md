---
title: Was ist Azure Scheduler? | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie automatisierte Aufträge, die Dienste innerhalb und außerhalb von Azure aufrufen, erstellen, planen und ausführen.
services: scheduler
ms.service: scheduler
ms.suite: infrastructure-services
author: derek1ee
ms.author: deli
ms.reviewer: klam
ms.assetid: 52aa6ae1-4c3d-43fb-81b0-6792c84bcfae
ms.topic: conceptual
ms.date: 09/17/2018
ms.openlocfilehash: 2f418a78f80d65cbb784685804a4cc6790c28b99
ms.sourcegitcommit: 29880cf2e4ba9e441f7334c67c7e6a994df21cfe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2019
ms.locfileid: "71300901"
---
# <a name="what-is-azure-scheduler"></a>Was ist Azure Scheduler?

> [!IMPORTANT]
> Azure Scheduler wird [eingestellt](../scheduler/migrate-from-scheduler-to-logic-apps.md#retire-date) und durch [Azure Logic Apps](../logic-apps/logic-apps-overview.md) ersetzt. Wenn Sie weiterhin mit den Aufträgen arbeiten möchten, die Sie in Scheduler eingerichtet haben, sollten Sie so bald wie möglich [zu Azure Logic Apps migrieren](../scheduler/migrate-from-scheduler-to-logic-apps.md).

[Azure Scheduler](https://azure.microsoft.com/services/scheduler/) unterstützt Sie durch deklarative Beschreibung von Aktionen beim Erstellen von [Aufträgen](../scheduler/scheduler-concepts-terms.md), die in der Cloud ausgeführt werden. Anschließend werden diese Aktionen vom Dienst automatisch geplant und ausgeführt. Sie können beispielsweise Dienste inner- und außerhalb von Azure aufrufen (z. B. HTTP- oder HTTPS-Endpunkte) und zudem Nachrichten in Azure Storage-Warteschlangen sowie Azure Service Bus-Warteschlangen oder -Themen veröffentlichen. Sie können Aufträge sofort oder zu einem späteren Zeitpunkt ausführen. [Komplexe Zeitpläne und erweiterte Serien](../scheduler/scheduler-advanced-complexity.md) werden von Scheduler problemlos unterstützt. Scheduler legt fest, wann Aufträge ausgeführt werden, speichert einen Verlauf der Auftragsergebnisse, den Sie überprüfen können, und plant die auszuführenden Workloads dann auf vorhersehbare und zuverlässige Weise.

Der Scheduler-Dienst kann zwar zum Erstellen, Verwalten und Ausführen von geplanten Workloads verwendet werden, er hostet aber weder Workloads noch führt er Code aus. Der Dienst *ruft* lediglich an anderer Stelle (beispielsweise in Azure, lokal oder bei einem anderen Anbieter) gehostete(n) Dienste oder Code auf. Scheduler kann den Aufruf über HTTP, HTTPS, eine Storage-Warteschlange, eine Service Bus-Warteschlange oder ein Service Bus-Thema ausführen. Zum Erstellen, Verwalten und Planen von Aufträgen können Sie das [Azure-Portal](../scheduler/scheduler-get-started-portal.md), Code, die [Scheduler-REST-API](https://docs.microsoft.com/rest/api/scheduler/) oder [Azure Scheduler-PowerShell-Cmdlets (Referenz)](scheduler-powershell-reference.md) verwenden. Beispielsweise können Sie Aufträge und [Auftragssammlungen](../scheduler/scheduler-concepts-terms.md) mithilfe von Skripts und im Azure-Portal programmgesteuert erstellen, anzeigen, aktualisieren, verwalten oder löschen.

Andere Azure-Planungsfunktionen nutzen Scheduler ebenfalls im Hintergrund, beispielsweise der Dienst [Azure WebJobs](../app-service/webjobs-create.md), bei dem es sich um eine [Web-Apps](https://azure.microsoft.com/services/app-service/web/)-Funktion von Azure App Service handelt. Sie können die Kommunikation für diese Aktionen mit der [Scheduler-REST-API](https://docs.microsoft.com/rest/api/scheduler/) verwalten.

Im Folgenden sind einige Szenarien aufgeführt, in denen Scheduler hilfreich sein kann:

* **Ausführen wiederkehrender App-Aktionen**: Ein Beispiel hierfür ist die regelmäßige Erfassung von Daten aus Twitter in einem Feed.

* **Ausführen täglicher Wartungsaufgaben**: Beispiele hierfür sind die tägliche Bereinigung von Protokollen, die Erstellung von Sicherungen und die Ausführung anderer Wartungsaufgaben. 

  Ein Administrator kann beispielsweise planen, dass seine Datenbank in den kommenden neun Monaten täglich um 1:00 Uhr gesichert wird.

## <a name="next-steps"></a>Nächste Schritte

* [Erste Schritte mit Scheduler im Azure-Portal](scheduler-get-started-portal.md)
* Erfahren Sie mehr über die [Pläne und Abrechnung für Azure Scheduler](scheduler-plans-billing.md).
* Erfahren Sie, wie Sie [komplexe Zeitpläne und erweiterte Serien mit Azure Scheduler erstellen](scheduler-advanced-complexity.md).