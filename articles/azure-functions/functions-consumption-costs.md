---
title: Abschätzen der Kosten des Verbrauchstarifs in Azure Functions
description: Erfahren Sie, wie Sie die Kosten besser abschätzen, die Sie eventuell verursachen, wenn Sie Ihre Funkions-App in einem Verbrauchstarif in Azure ausführen.
author: ggailey777
ms.author: glenga
ms.date: 9/20/2019
ms.topic: conceptual
ms.service: azure-functions
manager: gwallace
ms.openlocfilehash: 0ff41eb511ad4513fc9bf5a2ded7ef47b08d12ab
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2019
ms.locfileid: "72243305"
---
# <a name="estimating-consumption-plan-costs"></a>Abschätzen der Kosten des Verbrauchstarifs

Zurzeit gibt es drei Typen von Hostingplänen für eine App, die in Azure Functions ausgeführt wird, wobei jeder Plan über ein eigenes Preismodell verfügt: 

| Plan | BESCHREIBUNG |
| ---- | ----------- |
| [**Verbrauch**](functions-scale.md#consumption-plan) | Ihnen wird nur die Zeit in Rechnung gestellt, die Ihre Funktions-App auch ausgeführt wird. Dieser Tarif umfasst pro Abonnement eine [free grant][seite mit der preisübersicht] (kostenlose Zuweisung).|
| [**Premium**](functions-scale.md#premium-plan) | Bietet Ihnen dieselben Features und Skalierungsverfahren wie der Verbrauchstarif, aber mit mehr Leistung und VNET-Zugriff. Die Kosten basieren auf dem von Ihnen gewählten Tarif. Weitere Informationen finden Sie unter [Premium-Plan (Premium-Tarif) für Azure Functions](functions-premium-plan.md). |
| [**Dediziert (App Service)** ](functions-scale.md#app-service-plan) <br/>(Tarif „Basic“ oder höher) | Wenn Sie in dedizierten VMs oder in Isolierung ausführen müssen, benutzerdefinierte Images verwenden oder die Überkapazität Ihres App Service-Plans nutzen möchten. Verwendet [normale App Service-Planabrechnung](https://azure.microsoft.com/pricing/details/app-service/). Die Kosten basieren auf dem von Ihnen gewählten Tarif.|

Sie haben den Plan ausgewählt, der die Leistung Ihrer Funktion sowie Ihre Kostenanforderungen am besten unterstützt. Weitere Informationen finden Sie unter [Skalierung und Hosting von Azure Functions](functions-scale.md).

Dieser Artikel behandelt nur den Verbrauchstarif, da dieser Tarif zu variablen Kosten führt. 

Durable Functions können auch in einem Verbrauchstarif ausgeführt werden. Weitere Informationen zu den Kostenüberlegungen bei der Verwendung von Durable Functions finden Sie unter [Abrechnung von Durable Functions](./durable/durable-functions-billing.md).

## <a name="consumption-plan-costs"></a>Kosten des Verbrauchstarifs

Die Ausführungs*kosten* einer einzelnen Funktionsausführung werden in *GB-Sekunden* gemessen. Die Ausführungskosten werden berechnet, indem die Speichernutzung mit der Ausführungsdauer kombiniert wird. Eine Funktion, die länger ausgeführt wird, kostet mehr, ebenso wie eine Funktion, die mehr Arbeitsspeicher verbraucht. 

Stellen Sie sich einen Fall vor, bei dem die von der Funktion verwendete Arbeitsspeichermenge konstant bleibt. In diesem Fall erfolgt die Berechnung der Kosten durch einfache Multiplikation. Nehmen wir beispielsweise an, dass ihre Funktion 0,5 GB für 3 Sekunden verbraucht hat. Dann betragen die Ausführungskosten `0.5GB * 3s = 1.5 GB-seconds`. 

Da sich die Speichernutzung im Laufe der Zeit ändert, ist die Berechnung im Wesentlichen das Integral der Arbeitsspeichernutzung im Lauf der Zeit.  Das System führt diese Berechnung durch, indem die Speichernutzung des Prozesses (zusammen mit untergeordneten Prozessen) in regelmäßigen Abständen stichprobenhaft erfasst wird. Wie auf der [Seite mit der Preisübersicht] erwähnt, wird die Speichernutzung auf den nächstgelegenen 128-MB-Bucket aufgerundet. Wenn Ihr Prozess 160 MB verwendet, werden Ihnen 256 MB in Rechnung gestellt. Die Berechnung berücksichtigt dabei die Parallelität, bei der es sich um die gleichzeitige Ausführung mehrerer Funktionen im selben Prozess handelt.

> [!NOTE]
> Obwohl die CPU-Nutzung in den Ausführungskosten nicht direkt berücksichtigt wird, kann sie sich auf die Kosten auswirken, wenn sie die Ausführungszeit der Funktion beeinflusst.

## <a name="other-related-costs"></a>Andere verwandte Kosten

Wenn Sie die Gesamtkosten der Ausführung ihrer Funktionen in einem beliebigen Plan abschätzen, denken Sie daran, dass die Functions-Laufzeit mehrere andere Azure-Dienste verwendet, die jeweils gesondert abgerechnet werden. Wenn Sie Preise für Funktions-Apps berechnen, müssen Sie für alle Trigger und Bindungen, die in andere Azure-Dienste integriert werden, diese zusätzlichen Dienste erstellen und dafür bezahlen. 

Für Funktionen, die in einem Verbrauchstarif ausgeführt werden, bestehen die Gesamtkosten aus den Ausführungskosten ihrer Funktionen plus die Kosten für Bandbreite und zusätzliche Dienste. 

Wenn Sie die Gesamtkosten für ihre Funktions-App und der zugehörige Dienste abschätzen, verwenden Sie den [Azure-Preisrechner](https://azure.microsoft.com/pricing/calculator/?service=functions). 

| Verwandte Kosten | BESCHREIBUNG |
| ------------ | ----------- |
| **Speicherkonto** | Für jede Funktions-App ist es erforderlich, dass Sie über ein zugeordnetes universelles [Azure Storage-Konto](../storage/common/storage-introduction.md#types-of-storage-accounts) verfügen, das [gesondert abgerechnet](https://azure.microsoft.com/pricing/details/storage/) wird. Dieses Konto wird von der Functions-Laufzeit intern verwendet, aber Sie können es auch für Storage-Trigger und -Bindungen verwenden. Wenn Sie kein Speicherkonto besitzen, wird mit der Erstellung der Funktions-App eins für Sie erstellt. Weitere Informationen finden Sie unter [Speicherkontoanforderungen](functions-scale.md#storage-account-requirements).|
| **Application Insights** | Functions basiert auf [Application Insights](../azure-monitor/app/app-insights-overview.md), um eine Hochleistungsüberwachungs-Erfahrung für Ihre Funktions-Apps bereitzustellen. Wenn dies auch nicht erforderlich ist, sollten Sie doch die [Application Insights-Integration aktivieren](functions-monitoring.md#enable-application-insights-integration). Monatlich ist eine kostenlose Zuweisung von Telemetriedaten enthalten. Weitere Informationen finden Sie auf der [Seite mit der Preisübersicht für Azure Monitor](https://azure.microsoft.com/pricing/details/monitor/). |
| **Netzwerkbandbreite** | Sie bezahlen nicht für Datenübertragungen zwischen Azure-Diensten innerhalb derselben Region. Allerdings können Sie Kosten für ausgehende Datenübertragungen in eine andere Region oder außerhalb von Azure verursachen. Weitere Informationen finden Sie in den [Bandbreitenpreisdetails](https://azure.microsoft.com/pricing/details/bandwidth/). |

## <a name="behaviors-affecting-execution-time"></a>Verhalten, die die Ausführungszeit beeinflussen

Die folgenden Verhalten Ihrer Funktionen können sich auf die Ausführungszeit auswirken:

+ **Trigger und Bindungen**: Die Zeit, die das Lesen von Eingaben aus Ihren und das Schreiben der Ausgaben in Ihre [Funktionsbindungen](functions-triggers-bindings.md) benötigt, wird als Ausführungszeit gezählt. Wenn Ihre Funktion beispielsweise eine Ausgabenbindung verwendet, um eine Nachricht in eine Azure Storage-Warteschlange zu schreiben, beinhaltet Ihre Ausführungszeit die Zeit, die zum Schreiben der Nachricht in die Warteschlange benötigt wird, was in die Berechnung der Funktionskosten einfließt. 

+ **Asynchrone Ausführung**: Die Zeit, die Ihre Funktion auf die Ergebnisse einer asynchronen Anforderung wartet (`await` in C#), wird als Ausführungszeit gezählt. Die GB-Sekunden-Berechnung basiert auf der Start- und Endzeit der Funktion und der Arbeitsspeichernutzung in diesem Zeitraum. Was im Verlauf dieses Zeitraums hinsichtlich der CPU-Aktivität geschieht, wird nicht in die Berechnung einbezogen. Sie können die Kosten möglicherweise während asynchroner Vorgänge verringern, indem Sie [Durable Functions](durable/durable-functions-overview.md) verwenden. In awaits verbrachte Zeit wird Ihnen für Orchestratorfunktionen nicht berechnet.

## <a name="view-execution-data"></a>Ausführungsdaten anzeigen

In [Ihrer Rechnung](/azure/billing/billing-download-azure-invoice) können Sie die kostenbezogenen Daten **Total Executions – Functions** (Ausführungen gesamt) und **Execution Time – Functions** (Ausführungszeit) zusammen mit den tatsächlich abgerechneten Kosten sehen. Diese Rechnungsdaten stellen jedoch eine monatliche Aggregation für einen vergangenen Abrechnungszeitraum dar. 

Um die Kostenauswirkungen Ihrer Funktionen besser zu verstehen, können Sie Azure Monitor verwenden, um kostenbezogene Metriken anzuzeigen, die von Ihren Funktions-Apps generiert werden. Sie können entweder den [Azure Monitor-Metrik-Explorer](../azure-monitor/platform/metrics-getting-started.md) im [Azure-Portal] oder REST-APIs verwenden, um diese Daten zu erhalten.

### <a name="monitor-metrics-explorer"></a>Monitor-Metrik-Explorer

Verwenden Sie den [Azure Monitor-Metrik-Explorer](../azure-monitor/platform/metrics-getting-started.md), um kostenbezogene Daten für Ihre Funktions-Apps im Verbrauchstarif in einem grafischen Format anzuzeigen. 

1. Suchen Sie im oberen Bereich des [Azure-Portal] in **Dienste, Ressourcen und Dokumente durchsuchen** nach `monitor`, und wählen Sie unter **Monitor** die Option **Dienste** aus.

1. Wählen Sie links **Metriken** > **Ressource auswählen** aus, und verwenden Sie dann die Einstellungen unterhalb der Abbildung, um ihre Funktions-App auszuwählen.

    ![Auswählen Ihrer Funktions-App](media/functions-consumption-costing/select-a-resource.png)

      
    |Einstellung  |Empfohlener Wert  |BESCHREIBUNG  |
    |---------|---------|---------|
    | Subscription    |  Ihr Abonnement  | Das Abonnement Ihrer Funktions-App.  |
    | Resource group     | Ihre Ressourcengruppe  | Die Ressourcengruppe, die Ihre Funktions-App enthält.   |
    | Ressourcentyp     |  App-Dienste | Funktions-Apps werden als App Services-Instanzen im Monitor angezeigt. |
    | Resource     |  Ihre Funktions-App  | Die zu überwachende Funktions-App.        |

1. Wählen Sie **Anwenden** aus, um Ihre Funktions-App als zu überwachende Ressource auszuwählen.

1. Wählen Sie in **Metrik** die Einträge **Ausführungsanzahl für Funktion** und **Summe** für **Aggregation** aus. Dadurch wird die Summe der Ausführungen während des gewählten Zeitraums dem Diagramm hinzugefügt.

    ![Definieren einer Funktions-App-Metrik, um sie dem Diagramm hinzuzufügen](media/functions-consumption-costing/monitor-metrics-add-metric.png)

1. Wählen Sie **Metrik hinzufügen** aus, und wiederholen Sie die Schritte 2–4, um dem Diagramm **Ausführungseinheiten für Funktion** hinzuzufügen. 

Das resultierende Diagramm enthält die Summen für beide Ausführungsmetriken im ausgewählten Zeitraum, der in diesem Fall zwei Stunden beträgt.

![Diagramm der Funktionsausführungen und Ausführungseinheiten](media/functions-consumption-costing/monitor-metrics-execution-sum.png)

Da die Anzahl der Ausführungseinheiten so viel höher ist, als die Ausführungen, zeigt das Diagramm nur die Ausführungseinheiten an.

Dieses Diagramm zeigt eine Gesamtzahl von 1,11 Mrd. `Function Execution Units`, die in einem Zeitraum von zwei Stunden verbraucht wurden, gemessen in MB-Millisekunden. Um den Wert in GB-Sekunden umzuwandeln, teilen Sie durch 1.024.000. In diesem Beispiel hat die Funktions-App `1110000000 / 1024000 = 1083.98` GB-Sekunden verbraucht. Sie können diesen Wert mit dem aktuellen Preis der Ausführungszeit auf der Seite mit den [Functions-Preisen][Seite mit der Preisübersicht] multiplizieren, wodurch Sie die Kosten für diese zwei Stunden erhalten, vorausgesetzt, Sie haben bereits alle kostenlosen Zuweisungen von Ausführungszeit verbraucht. 

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Die [Azure CLI](/cli/azure/) enthält Befehle zum Abrufen von Metriken. Sie können die CLI aus einer lokalen Befehlsumgebung heraus oder direkt über das Portal mithilfe von [Azure Cloud Shell](../cloud-shell/overview.md) verwenden. Beispielsweise gibt der folgende [az monitor metrics list](/cli/azure/monitor/metrics#az-monitor-metrics-list)-Befehl stündlich Daten aus demselben zuvor verwendeten Zeitraum zurück.

Stellen Sie sicher, dass Sie `<AZURE_SUBSCRIPTON_ID>` durch Ihre Azure-Abonnement-ID ersetzen, wenn Sie den Befehl ausführen.

```azurecli-interactive
az monitor metrics list --resource /subscriptions/<AZURE_SUBSCRIPTION_ID>/resourceGroups/metrics-testing-consumption/providers/Microsoft.Web/sites/metrics-testing-consumption --metric FunctionExecutionUnits,FunctionExecutionCount --aggregation Total --interval PT1H --start-time 2019-09-11T21:46:00Z --end-time 2019-09-11T23:18:00Z
```

Dieser Befehl gibt eine JSON-Nutzlast zurück, die in etwa wie folgt aussieht:

```json
{
  "cost": 0.0,
  "interval": "1:00:00",
  "namespace": "Microsoft.Web/sites",
  "resourceregion": "centralus",
  "timespan": "2019-09-11T21:46:00Z/2019-09-11T23:18:00Z",
  "value": [
    {
      "id": "/subscriptions/XXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXX/resourceGroups/metrics-testing-consumption/providers/Microsoft.Web/sites/metrics-testing-consumption/providers/Microsoft.Insights/metrics/FunctionExecutionUnits",
      "name": {
        "localizedValue": "Function Execution Units",
        "value": "FunctionExecutionUnits"
      },
      "resourceGroup": "metrics-testing-consumption",
      "timeseries": [
        {
          "data": [
            {
              "average": null,
              "count": null,
              "maximum": null,
              "minimum": null,
              "timeStamp": "2019-09-11T21:46:00+00:00",
              "total": 793294592.0
            },
            {
              "average": null,
              "count": null,
              "maximum": null,
              "minimum": null,
              "timeStamp": "2019-09-11T22:46:00+00:00",
              "total": 316576256.0
            }
          ],
          "metadatavalues": []
        }
      ],
      "type": "Microsoft.Insights/metrics",
      "unit": "Count"
    },
    {
      "id": "/subscriptions/XXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXX/resourceGroups/metrics-testing-consumption/providers/Microsoft.Web/sites/metrics-testing-consumption/providers/Microsoft.Insights/metrics/FunctionExecutionCount",
      "name": {
        "localizedValue": "Function Execution Count",
        "value": "FunctionExecutionCount"
      },
      "resourceGroup": "metrics-testing-consumption",
      "timeseries": [
        {
          "data": [
            {
              "average": null,
              "count": null,
              "maximum": null,
              "minimum": null,
              "timeStamp": "2019-09-11T21:46:00+00:00",
              "total": 33538.0
            },
            {
              "average": null,
              "count": null,
              "maximum": null,
              "minimum": null,
              "timeStamp": "2019-09-11T22:46:00+00:00",
              "total": 13040.0
            }
          ],
          "metadatavalues": []
        }
      ],
      "type": "Microsoft.Insights/metrics",
      "unit": "Count"
    }
  ]
}
```
Diese spezifische Antwort zeigt, dass die App von `2019-09-11T21:46` bis `2019-09-11T23:18` 1.110.000.000 MB-Millisekunden (1.083,98 GB-Sekunden) verbraucht hat.

## <a name="determine-memory-usage"></a>Arbeitsspeichernutzung ermitteln

Funktionsausführungseinheiten sind eine Kombination aus Ausführungszeit und Ihrer Speichernutzung, was diese zu einer schwierigen Metrik macht, um die Speichernutzung zu verstehen. Arbeitsspeicherdaten sind keine aktuell in Azure Monitor verfügbare Metrik. Wenn Sie jedoch die Speichernutzung Ihrer App optimieren möchten, können Sie die von Application Insights erfassten Daten der Leistungsindikatoren verwenden.  

Falls noch nicht geschehen, [aktivieren Sie Application Insights in Ihrer Funktions-App](functions-monitoring.md#enable-application-insights-integration). Mit dieser aktivierten Integration können Sie [diese Telemetriedaten im Portal abfragen](functions-monitoring.md#query-telemetry-data).  

Wählen Sie unter **Überwachung** den Eintrag **Protokolle (Analytics)** aus, kopieren Sie dann die folgende Telemetrieabfrage, fügen Sie sie in das Abfragefenster ein, und wählen Sie **Ausführen** aus. Diese Abfrage gibt die Gesamtarbeitsspeichernutzung zu jedem Stichprobenzeitpunkt zurück.

```
performanceCounters
| where name == "Private Bytes"
| project timestamp, name, value
```

Die Ergebnisse sehen ungefähr wie folgt aus:

| Zeitstempel \[UTC\]          | name          | value       |
|----------------------------|---------------|-------------|
| 12.9.2019, 1:05:14\.947 AM | Private Bytes | 209.932.288 |
| 12.9.2019, 1:06:14\.994 AM | Private Bytes | 212.189.184 |
| 12.9.2019, 1:06:30\.010 AM | Private Bytes | 231.714.816 |
| 12.9.2019, 1:07:15\.040 AM | Private Bytes | 210.591.744 |
| 12.9.2019, 1:12:16\.285 AM | Private Bytes | 216.285.184 |
| 12.9.2019, 1:12:31\.376 AM | Private Bytes | 235.806.720 |

## <a name="function-level-metrics"></a>Metriken auf Funktionsebene

Azure Monitor erfasst Metriken auf Ressourcenebene, wobei es sich für Functions um die Funktions-App handelt. Application Insights-Integration gibt Metriken pro Funktion aus. Im Folgenden finden Sie ein Beispiel für eine Analytics-Abfrage, mit der die durchschnittliche Dauer einer Funktion abgefragt wird:

```
customMetrics
| where name contains "Duration"
| extend averageDuration = valueSum / valueCount
| summarize averageDurationMilliseconds=avg(averageDuration) by name
```

| name                       | averageDurationMilliseconds |
|----------------------------|-----------------------------|
| QueueTrigger AvgDurationMs | 16\.087                     |
| QueueTrigger MaxDurationMs | 90\.249                     |
| QueueTrigger MinDurationMs | 8\.522                      |

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Weitere Informationen zur Überwachung von Funktions-Apps](functions-monitoring.md)

[Seite mit der Preisübersicht]: https://azure.microsoft.com/pricing/details/functions/
[Azure-Portal]: https://portal.azure.com
