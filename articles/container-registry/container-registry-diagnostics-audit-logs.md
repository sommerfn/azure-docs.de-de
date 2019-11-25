---
title: Ressourcenprotokolle für Azure Container Registry | Microsoft-Dokumentation
description: Aufzeichnen und Analysieren von Ressourcenprotokollereignissen für Azure Container Registry wie Authentifizierung, Imagepush und Imagepull.
services: container-registry
documentationcenter: ''
author: dlepow
manager: gwallace
editor: ''
ms.assetid: ''
ms.service: container-registry
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: ''
ms.date: 10/30/2019
ms.author: danlep
ms.openlocfilehash: e419f8c5cf06efc93294f9c428e9102c1f81b36a
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73180198"
---
# <a name="azure-container-registry-logs-for-diagnostic-evaluation-and-auditing"></a>Azure Container Registry-Protokolle für die Diagnoseauswertung und -überwachung

In diesem Artikel wird erläutert, wie Sie Protokolldaten für eine Azure-Containerregistrierung mithilfe der Funktionen von [Azure Monitor](../azure-monitor/overview.md) sammeln. Azure Monitor sammelt [Ressourcenprotokolle](../azure-monitor/platform/resource-logs-overview.md) (ehemals *Diagnoseprotokolle*) für benutzergesteuerte Ereignisse in Ihrer Registrierung. Erfassen und nutzen Sie diese Daten, um Anforderungen zu erfüllen wie:

* Überwachen von Registrierungsauthentifizierungsereignissen, um Sicherheit und Compliance sicherzustellen 

* Bereitstellen einer kompletten Aktivitätsspur für Registrierungsartefakte wie Push- und Pull-Ereignisse, damit Sie Betriebsprobleme mit Ihrer Registrierung diagnostizieren können. 

Das Sammeln von Ressourcenprotokolldaten mithilfe von Azure Monitor kann zusätzliche Kosten verursachen. Weitere Informationen finden Sie unter [Azure Monitor – Preise](https://azure.microsoft.com/pricing/details/monitor/). 


> [!IMPORTANT]
> Diese Funktion befindet sich derzeit in der Vorschauphase. Es gelten einige [Einschränkungen](#preview-limitations). Vorschauversionen werden Ihnen zur Verfügung gestellt, wenn Sie die [zusätzlichen Nutzungsbedingungen][terms-of-use] akzeptieren. Einige Aspekte dieses Features werden bis zur allgemeinen Verfügbarkeit unter Umständen noch geändert.

## <a name="preview-limitations"></a>Einschränkungen der Vorschau

Das Protokollieren von Ereignissen auf Repositoryebene schließt derzeit keine Lösch- und „Markierung aufheben“-Ereignisse ein. Nur die folgenden Repositoryereignisse werden protokolliert:
* **Push-Ereignisse** für Images und andere Artefakte
* **Pull-Ereignisse** für Images und andere Artefakte

## <a name="registry-resource-logs"></a>Registrierungsressourcenprotokolle

Ressourcenprotokolle enthalten Informationen, die von Azure-Ressourcen ausgegeben werden und deren internen Betrieb beschreiben. Für eine Azure-Containerregistrierung enthalten die Protokolle Authentifizierungsereignisse und Ereignisse auf Repositoryebene, die in den folgenden Tabellen gespeichert werden. 

* **ContainerRegistryLoginEvents**: Registrierungauthentifizierungsereignisse und -status, einschließlich der eingehenden Identität und IP-Adresse.
* **ContainerRegistryRepositoryEvents**: Vorgänge wie Push und Pull von Images und anderen Artefakten in Registrierungsrepositorys.
* **AzureMetrics** - [Containerregistrierungsmetriken](../azure-monitor/platform/metrics-supported.md#microsoftcontainerregistryregistries): wie aggregierte Push- und Pullanzahlen.

Für Vorgänge umfassen Protokolldaten:
  * Erfolg- oder Fehlerstatus
  * Zeitstempel für Start und Beendigung

Zusätzlich zu den Ressourcenprotokollen bietet Azure ein [Aktivitätsprotokoll](../azure-monitor/platform/activity-logs-overview.md), eine einzelne Aufzeichnung von Azure-Verwaltungsereignissen auf Abonnementebene wie das Erstellen oder Löschen einer Containerregistrierung.

## <a name="enable-collection-of-resource-logs"></a>Aktivieren der Sammlung von Ressourcenprotokollen

Die Sammlung von Ressourcenprotokollen für eine Containerregistrierung ist nicht standardmäßig aktiviert. Aktivieren Sie Diagnoseeinstellungen für jede Registrierung explizit, die Sie überwachen möchten. Informationen zu Optionen zum Aktivieren von Diagnoseeinstellung finden Sie unter [Erstellen einer Diagnoseeinstellung zum Erfassen von Plattformprotokollen und Metriken in Azure](../azure-monitor/platform/diagnostic-settings.md).

Um beispielsweise Protokolle und Metriken für eine Containerregistrierung in Azure Monitor nahezu in Echtzeit anzuzeigen, erfassen Sie die Ressourcenprotokolle in einem Log Analytics-Arbeitsbereich. So aktivieren Sie diese Diagnoseeinstellung über das Azure-Portal

1. Wenn Sie nicht bereits einen Arbeitsbereich besitzen, erstellen Sie einen über das [Azure-Portal](../azure-monitor/learn/quick-create-workspace.md). Um die Wartezeit bei der Datensammlung zu minimieren, stellen Sie sicher, dass sich der Arbeitsbereich in **derselben Region befindet** wie Ihre Containerregistrierung befindet.
1. Wählen Sie im Portal die Registrierung und dann **Überwachung > Diagnoseeinstellungen > Diagnoseeinstellung hinzufügen** aus.
1. Geben Sie einen Namen für die Einstellung ein, und wählen Sie **An Log Analytics senden** aus.
1. Wählen Sie den Arbeitsbereich für die Registrierungsdiagnoseprotokolle aus.
1. Wählen Sie die Protokolldaten aus, die Sie sammeln möchten, und klicken Sie auf **Speichern**.

Die folgende Abbildung zeigt die Erstellung einer Diagnoseeinstellung für eine Registrierung mithilfe des Portals.

![Aktivieren von Diagnoseeinstellungen](media/container-registry-diagnostics-audit-logs/diagnostic-settings.png)

> [!TIP]
> Sammeln Sie nur die Daten, die Sie benötigen, um die Balance zwischen Kosten und Ihren Überwachungsbedürfnissen zu halten. Wenn Sie z. B. nur Authentifizierungsereignisse überwachen müssen, wählen Sie nur das Protokoll **ContainerRegistryLoginEvents** aus. 

## <a name="view-data-in-azure-monitor"></a>Anzeigen von Daten in Azure Monitor

Nachdem Sie die Sammlung von Diagnoseprotokollen in Log Analytics aktiviert haben, kann es ein paar Minuten dauern, bis die Daten in Azure Monitor angezeigt werden. Um die Daten im Portal anzuzeigen, wählen Sie die Registrierung und dann **Überwachung > Protokolle** aus. Wählen Sie eine der Tabellen aus, die Daten für die Registrierung enthält. 

Führen Sie Abfragen aus, um die Daten anzuzeigen. Es stehen mehrere Beispielabfragen zur Verfügung, oder Sie können Ihre eigenen ausführen. Beispielsweise ruft die folgende Abfrage die jüngsten 24 Stunden von Daten aus der Tabelle **ContainerRegistryRepositoryEvents** ab:

```Kusto
ContainerRegistryRepositoryEvents
| where TimeGenerated > ago(1d) 
```

In der folgenden Abbildung ist eine Beispielausgabe dargestellt:

![Abfragen von Protokolldaten](media/container-registry-diagnostics-audit-logs/azure-monitor-query.png)

Ein Tutorial zur Verwendung von Log Analytics im Azure-Portal finden Sie unter [Erste Schritte mit Azure Monitor Log Analytics](../azure-monitor/log-query/get-started-portal.md), oder probieren Sie die [Demoumgebung](https://portal.loganalytics.io/demo) von Log Analytics aus. 

Weitere Informationen zu Protokollabfragen finden Sie unter [Übersicht über Protokollabfragen in Azure Monitor](../azure-monitor/log-query/log-query-overview.md).

### <a name="additional-query-examples"></a>Zusätzliche Abfragebeispiele

#### <a name="100-most-recent-registry-events"></a>Die 100 jüngsten Registrierungsereignisse

```Kusto
ContainerRegistryRepositoryEvents
| union ContainerRegistryLoginEvents
| top 100 by TimeGenerated
| project TimeGenerated, LoginServer , OperationName , Identity , Repository , DurationMs , Region , ResultType
```

## <a name="additional-log-destinations"></a>Zusätzliche Protokollziele

Ein gängiges Szenario, zusätzlich oder alternativ zum Senden der Protokolle an Log Analytics, ist die Auswahl eines Azure-Speicherkontos als Protokollziel. Um Protokolle in Azure Storage zu archivieren, erstellen Sie ein Speicherkonto, bevor Sie durch die Diagnoseeinstellungen archivieren.

Sie können-Diagnoseprotokollereignisse auch an einen [Azure Event Hub](../event-hubs/event-hubs-what-is-event-hubs.md) streamen. Event Hubs können mit einem beliebigen Echtzeitanalyse-Anbieter Millionen Ereignisse pro Sekunde erfassen und anschließend transformieren und speichern. 

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über das Verwenden von [Log Analytics](../azure-monitor/log-query/get-started-portal.md) und das Erstellen von [Protokollabfragen](../azure-monitor/log-query/get-started-queries.md).
* Informationen zu Plattformprotokollen, die auf unterschiedlichen Ebenen von Azure verfügbar sind, finden Sie unter [Übersicht über Protokolle der Azure-Plattform](../azure-monitor/platform/platform-logs-overview.md).

<!-- LINKS - External -->
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/
