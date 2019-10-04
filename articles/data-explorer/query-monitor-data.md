---
title: Abfragen von Daten in Azure Monitor mit Azure Data Explorer (Vorschau)
description: In diesem Thema werden Daten in Azure Monitor abgefragt, indem ein Azure Data Explorer-Proxy für produktübergreifende Abfragen mit Application Insights und Log Analytics erstellt wird.
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 07/10/2019
ms.openlocfilehash: 43d91bff6b8b67e79a9549c1524f918166c9adc4
ms.sourcegitcommit: f3f4ec75b74124c2b4e827c29b49ae6b94adbbb7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2019
ms.locfileid: "70933998"
---
# <a name="query-data-in-azure-monitor-using-azure-data-explorer-preview"></a>Abfragen von Daten in Azure Monitor mit Azure Data Explorer (Vorschau)

Der Azure Data Explorer-Proxycluster (ADX Proxy) ermöglicht Ihnen das Ausführen von produktübergreifenden Abfragen zwischen Azure Data Explorer, [Application Insights (AI)](/azure/azure-monitor/app/app-insights-overview) und [Log Analytics (LA)](/azure/azure-monitor/platform/data-platform-logs) im Dienst [Azure Monitor](/azure/azure-monitor/). Sie können Azure Monitor den Log Analytics-Arbeitsbereichen oder Application Insights-Apps als Proxycluster zuordnen. Anschließend können Sie den Proxycluster mit Azure Data Explorer-Tools abfragen und in einer clusterübergreifenden Abfrage auf ihn verweisen. In dem Artikel wird gezeigt, wie eine Verbindung mit einem Proxycluster hergestellt wird, der Azure Data Explorer-Webbenutzeroberfläche ein Proxycluster hinzugefügt wird und über Azure Data Explorer Abfragen der AI-Apps oder LA-Arbeitsbereiche durchgeführt werden.

Abfolge der Schritte für den Azure Data Explorer-Proxy: 

![Abfolge der Schritte für den ADX-Proxy](media/adx-proxy/adx-proxy-flow.png)

## <a name="prerequisites"></a>Voraussetzungen

> [!NOTE]
> Der ADX-Proxy ist im Vorschaumodus. Um dieses Feature zu aktivieren, wenden Sie sich an das [ADXProxy](mailto:adxproxy@microsoft.com)-Team.

## <a name="connect-to-the-proxy"></a>Herstellen einer Verbindung mit dem Proxy

1. Überprüfen Sie vor dem Herstellen der Verbindung mit dem Log Analytics- oder Application Insights-Cluster, ob im linken Menü der native Azure Data Explorer-Cluster (z. B. der *Hilfecluster*) angezeigt wird.

    ![Nativer ADX-Cluster](media/adx-proxy/web-ui-help-cluster.png)

1. Wählen Sie auf der Azure Data Explorer-Benutzeroberfläche (https://dataexplorer.azure.com/clusters) **Cluster hinzufügen** aus.

1. Im Fenster **Cluster hinzufügen**:

    * Fügen Sie dem LA- oder AI-Cluster die URL hinzu. Beispiel: `https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>`

    * Wählen Sie **Hinzufügen**.

    ![Hinzufügen eines Clusters](media/adx-proxy/add-cluster.png)

    Wenn Sie eine Verbindung mit mehreren Proxyclustern hinzufügen, geben Sie jedem Cluster einen anderen Namen. Andernfalls haben alle Cluster im linken Bereich den gleichen Namen.

1. Nachdem die Verbindung hergestellt wurde, wird der LA- oder AI-Cluster im linken Bereich mit dem nativen ADX-Cluster angezeigt. 

    ![Log Analytics- und Azure Data Explorer-Cluster](media/adx-proxy/la-adx-clusters.png)

## <a name="run-queries"></a>Ausführen von Abfragen

Sie können zum Abfragen der Proxycluster den Kusto-Explorer, den ADX-Web-Explorer, Jupyter-Kqlmagic oder die REST-API verwenden. 

> [!TIP]
> * Datenbanknamen müssen den gleichen Namen wie die im Proxycluster angegebene Ressource haben. Bei Namen wird die Groß-/Kleinschreibung beachtet.
> * Achten Sie bei clusterübergreifenden Abfragen auf eine korrekte Benennung der Application Insights-Apps und Log Analytics-Arbeitsbereiche.
>     * Wenn Namen Sonderzeichen enthalten, werden diese im Proxyclusternamen durch URL-Codierung ersetzt. 
>     * Wenn Namen Zeichen enthalten, die nicht den [KQL-Regeln für Bezeichnernamen](/azure/kusto/query/schema-entities/entity-names) entsprechen, werden sie durch einen Bindestrich ( **-** ) ersetzt.

### <a name="query-against-the-native-azure-data-explorer-cluster"></a>Abfrage des nativen Azure Data Explorer-Clusters 

Führen Sie Abfragen des Azure Data Explorer-Clusters (z. B. der Tabelle *StormEvents* im *Hilfecluster*) aus. Überprüfen Sie beim Ausführen der Abfrage, ob der native Azure Data Explorer-Cluster im linken Bereich ausgewählt ist.

```kusto
StormEvents | take 10 // Demonstrate query through the native ADX cluster
```

![Abfragen der Tabelle „StormEvents“](media/adx-proxy/query-adx.png)

### <a name="query-against-your-la-or-ai-cluster"></a>Abfragen des LA- oder AI-Clusters

Wenn Sie Abfragen des LA- oder AI-Clusters ausführen, stellen Sie sicher, dass der LA- bzw. AI-Cluster im linken Bereich ausgewählt ist. 

```kusto
Perf | take 10 // Demonstrate query through the proxy on the LA workspace
```

![Abfragen des LA-Arbeitsbereichs](media/adx-proxy/query-la.png)

### <a name="query-your-la-or-ai-cluster-from-the-adx-proxy"></a>Abfragen des LA- oder AI-Clusters über den ADX-Proxy  

Wenn Sie über den Proxy Abfragen des LA- oder AI-Clusters ausführen, stellen Sie sicher, dass der LA- bzw. AI-Cluster im linken Bereich ausgewählt ist. Das folgende Beispiel zeigt eine Abfrage im LA-Arbeitsbereich mit dem nativen ADX-Cluster.

```kusto
cluster('https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>').database('<workspace-name').Perf
| take 10 
```

![Abfragen über den Azure Data Explorer-Proxy](media/adx-proxy/query-adx-proxy.png)

### <a name="cross-query-of-la-or-ai-cluster-and-the-adx-cluster-from-the-adx-proxy"></a>Clusterübergreifende Abfrage des LA- oder AI-Clusters und ADX-Clusters über den ADX-Proxy 

Wenn Sie über den Proxy clusterübergreifende Abfragen ausführen, überprüfen Sie, ob der native ADX-Cluster im linken Bereich ausgewählt ist. Die folgenden Beispiele veranschaulichen das Kombinieren von ADX-Clustertabellen (mithilfe von `union`) mit dem LA-Arbeitsbereich.

```kusto
union StormEvents, cluster('https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>').database('<workspace-name>').Perf
| take 10 
```

```kusto
let CL1 = 'https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>';
union <ADX table>, cluster(CL1).database(<workspace-name>).<table name>
```

![Clusterübergreifende Abfrage über den Azure Data Explorer-Proxy](media/adx-proxy/cross-query-adx-proxy.png)

Wenn Sie anstelle von „union“ den [`join`-Operator](/azure/kusto/query/joinoperator) verwenden, ist möglicherweise ein [`hint`](/azure/kusto/query/joinoperator#join-hints) erforderlich, um die Abfrage für den nativen Azure Data Explorer-Cluster (und nicht für den Proxy) auszuführen. 

## <a name="additional-syntax-examples"></a>Zusätzliche Syntaxbeispiele

Beim Aufrufen der Application Insights-Cluster (AI) oder Log Analytics-Cluster (LA) sind die folgenden Syntaxoptionen verfügbar:

|Syntaxbeschreibung  |Application Insights  |Log Analytics  |
|----------------|---------|---------|
| Datenbank in einem Cluster, der nur die definierte Ressource in diesem Abonnement enthält (**empfohlen für clusterübergreifende Abfragen**) |   cluster(`https://ade.applicationinsights.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.insights/components/<ai-app-name>').database('<ai-app-name>`) | cluster(`https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>').database('<workspace-name>`)     |
| Cluster, der alle Apps/Arbeitsbereiche in diesem Abonnement enthält    |     cluster(`https://ade.applicationinsights.io/subscriptions/<subscription-id>`)    |    cluster(`https://ade.loganalytics.io/subscriptions/<subscription-id>`)     |
|Cluster, der alle Apps/Arbeitsbereiche im Abonnement enthält, und diese sind Mitglieder dieser Ressourcengruppe    |   cluster(`https://ade.applicationinsights.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>`)      |    cluster(`https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>`)      |
|Cluster, der nur die definierte Ressource in diesem Abonnement enthält      |    cluster(`https://ade.applicationinsights.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.insights/components/<ai-app-name>`)    |  cluster(`https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>`)     |

## <a name="next-steps"></a>Nächste Schritte

[Write queries](write-queries.md) (Schreiben von Abfragen)
