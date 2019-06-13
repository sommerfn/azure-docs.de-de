---
title: Bewährte Methoden für die JSON-Strukturierung in Azure Time Series Insights-Abfragen | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Effizienz Ihrer Azure Time Series Insights-Abfragen steigern.
services: time-series-insights
author: ashannon7
manager: cshankar
ms.service: time-series-insights
ms.topic: article
ms.date: 05/09/2019
ms.author: anshan
ms.custom: seodec18
ms.openlocfilehash: 535fe9a7920db89e434b4cc1b66aa38bf72a7829
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2019
ms.locfileid: "65790078"
---
# <a name="how-to-shape-json-to-maximize-query-performance"></a>Strukturieren von JSON zum Maximieren der Abfrageleistung 

Dieser Artikel bietet Hilfestellung bei der JSON-Strukturierung, um die Effizienz Ihrer Azure Time Series Insights-Abfragen zu maximieren.

## <a name="video"></a>Video

### <a name="learn-best-practices-for-shaping-json-to-meet-your-storage-needsbr"></a>Bewährte Methoden für die Anpassung von JSON an Ihre Speicheranforderungen</br>

> [!VIDEO https://www.youtube.com/embed/b2BD5hwbg5I]

## <a name="best-practices"></a>Bewährte Methoden

Es ist wichtig, beim Senden von Ereignissen an Time Series Insights verschiedene Aspekte zu berücksichtigen. Insbesondere sollte Folgendes sichergestellt werden:

1. Senden Sie Daten so effizient wie möglich über das Netzwerk.
1. Stellen Sie sicher, dass Ihre Daten in einer Weise gespeichert werden, die das Ausführen geeigneter Aggregationen für Ihr Szenario ermöglichen.
1. Stellen Sie sicher, dass die Grenzwerte für Time Series Insights-Eigenschaften nicht überschritten werden:
   - 600 Eigenschaften (Spalten) für S1-Umgebungen
   - 800 Eigenschaften (Spalten) für S2-Umgebungen

Die folgenden Richtlinien unterstützen Sie dabei, die bestmögliche Abfrageleistung zu erzielen:

1. Verwenden Sie keine dynamischen Eigenschaften wie z.B. eine Tag-ID als Eigenschaftenname, weil hierdurch die Limits für die maximal zulässige Anzahl von Eigenschaften schneller erreicht werden.
1. Senden Sie keine unnötigen Eigenschaften. Wenn eine Abfrageeigenschaft nicht benötigt wird, sollte sie nicht gesendet werden, um Speicherengpässe zu vermeiden.
1. Verwenden Sie [Verweisdaten](time-series-insights-add-reference-data-set.md), um das Senden von statischen Daten über das Netzwerk zu vermeiden.
1. Verwenden Sie Dimensionseigenschaften für mehrere Ereignisse gemeinsam, um Daten effizienter über das Netzwerk zu senden.
1. Verwenden Sie keine tiefe Arrayschachtelung. Time Series Insights unterstützt bis zu zwei Ebenen für geschachtelte Arrays, die Objekte enthalten. Time Series Insights sorgt für flache Arrays in Nachrichten, indem eine Aufteilung in mehrere Ereignisse mit Eigenschaft-Wert-Paaren durchgeführt wird.
1. Wenn nur wenige Messwerte für viele oder alle Ereignisse vorhanden sind, ist es besser, diese Messwerte als separate Eigenschaften innerhalb desselben Objekts zu senden. Durch das separate Senden wird die Anzahl von Ereignissen verringert und die Abfrageleistung möglicherweise gesteigert, weil weniger Ereignisse verarbeitet werden müssen. Wenn mehrere Messwerte vorliegen, wird durch das Senden dieser als Werte in einer einzelnen Eigenschaft die Möglichkeit verringert, das Limit für die maximal zulässige Anzahl von Eigenschaften zu erreichen.

## <a name="example-overview"></a>Übersicht über das Beispiel

Die folgenden zwei Beispiele veranschaulichen das Senden von Ereignissen, um die zuvor genannten Empfehlungen zu verdeutlichen. Im Anschluss an jedes Beispiel können Sie sehen, wie die Empfehlungen umgesetzt wurden.

Die Beispiele basieren auf einem Szenario, in dem mehrere Geräte Messungen oder Signale senden. Bei diesen Messungen oder Signalen kann es sich um Werte wie Flussrate, Öldruck, Temperatur und Feuchtigkeit handeln. Im ersten Beispiel liegen einige wenige Messungen vor, die auf alle Geräte verteilt sind. Im zweiten Beispiel sind viele Geräte vorhanden, von denen jedes zahlreiche eindeutige Messungen sendet.

## <a name="scenario-one-only-a-few-measurements-exist"></a>1. Szenario: Nur wenige Messungen sind vorhanden.

> [!TIP]
> **Empfehlung:** Senden Sie jede Messung und jedes Signal als separate Eigenschaft bzw. Spalte.

Im folgenden Beispiel wird eine einzelne IoT Hub-Nachricht verwendet, bei der das äußere Array einen gemeinsam verwendeten Abschnitt für allgemeine Dimensionswerte enthält. Das äußere Array verwendet Verweisdaten, um die Effizienz der Nachricht zu erhöhen. Verweisdaten enthalten Gerätemetadaten, die sich nicht bei jedem Ereignis ändern, aber nützliche Eigenschaften für die Datenanalyse bereitstellen. Sowohl durch die Batchverarbeitung allgemeiner Dimensionswerte als auch durch die Verwendung von Verweisdaten werden beim Senden der Nachricht über das Netzwerk Bytes eingespart und somit die Effizienz gesteigert.

Beispiel-JSON-Nutzlast:

```json
[
    {
        "deviceId": "FXXX",
        "timestamp": "2018-01-17T01:17:00Z",
        "series": [
            {
                "Flow Rate ft3/s": 1.0172575712203979,
                "Engine Oil Pressure psi ": 34.7
            },
            {
                "Flow Rate ft3/s": 2.445906400680542,
                "Engine Oil Pressure psi ": 49.2
            }
        ]
    },
    {
        "deviceId": "FYYY",
        "timestamp": "2018-01-17T01:18:00Z",
        "series": [
            {
                "Flow Rate ft3/s": 0.58015072345733643,
                "Engine Oil Pressure psi ": 22.2
            }
        ]
    }
]
```

* Verweisdatentabelle (Schlüsseleigenschaft ist **deviceID**):

   | deviceId | messageId | deviceLocation |
   | --- | --- | --- |
   | FXXX | LINE\_DATA | EU |
   | FYYY | LINE\_DATA | US |

* Time Series Insights-Ereignistabelle (flache Ansicht):

   | deviceId | messageId | deviceLocation | timestamp | series.Flow Rate ft3/s | series.Engine Oil Pressure psi |
   | --- | --- | --- | --- | --- | --- |
   | FXXX | LINE\_DATA | EU | 2018-01-17T01:17:00Z | 1.0172575712203979 | 34.7 |
   | FXXX | LINE\_DATA | EU | 2018-01-17T01:17:00Z | 2.445906400680542 | 49.2 |
   | FYYY | LINE\_DATA | US | 2018-01-17T01:18:00Z | 0.58015072345733643 | 22.2 |

Zu den obigen Tabellen:

- Die Spalte **deviceId** dient als Spaltenüberschrift für die verschiedenen Geräte in einem Bestand. Eine Verwendung von „deviceID“ als eigener Eigenschaftenname hätte die Gesamtzahl der Geräte mit den fünf weiteren Spalten auf 595 (S1-Umgebungen) oder 795 (S2-Umgebungen) beschränkt.

- Unnötige Eigenschaften werden vermieden, z.B. Fabrikat- und Modellinformationen usw. Da diese künftig nicht abgefragt werden, wird durch ihr Entfernen die Netzwerk- und Speichereffizienz verbessert.

- Verweisdaten werden verwendet, um die Anzahl von Bytes zu verringern, die über das Netzwerk übertragen werden. Zwei Attribute (**messageId** und **deviceLocation**) werden mithilfe der Schlüsseleigenschaft **deviceId** verknüpft. Diese Daten werden zum Zeitpunkt ihres Eingangs mit den Telemetriedaten verknüpft und anschließend zur Abfrage in Time Series Insights gespeichert.

- Es werden zwei Schachtelungsebenen verwendet. Diese entsprechen der maximal zulässigen Schachtelung, die von Time Series Insights unterstützt wird. Tief verschachtelte Arrays müssen vermieden werden.

- Messwerte werden als separate Eigenschaften innerhalb desselben Objekts gesendet, weil nur wenige Messwerte vorliegen. In diesem Fall sind **series.Flow Rate psi** und **series.Engine Oil Pressure ft3/s** eindeutige Spalten.

## <a name="scenario-two-several-measures-exist"></a>2. Szenario: Mehrere Messwerte sind vorhanden.

> [!TIP]
> **Empfehlung**: Senden Sie Messungen als "type"-, "unit"-, "value"-Tupel.

Beispiel-JSON-Nutzlast:

```json
[
    {
        "deviceId": "FXXX",
        "timestamp": "2018-01-17T01:17:00Z",
        "series": [
            {
                "tagId": "pumpRate",
                "value": 1.0172575712203979
            },
            {
                "tagId": "oilPressure",
                "value": 49.2
            },
            {
                "tagId": "pumpRate",
                "value": 2.445906400680542
            },
            {
                "tagId": "oilPressure",
                "value": 34.7
            }
        ]
    },
    {
        "deviceId": "FYYY",
        "timestamp": "2018-01-17T01:18:00Z",
        "series": [
            {
                "tagId": "pumpRate",
                "value": 0.58015072345733643
            },
            {
                "tagId": "oilPressure",
                "value": 22.2
            }
        ]
    }
]
```

* Verweisdaten (Schlüsseleigenschaften sind **deviceId** und **series.tagId**):

   | deviceId | series.tagId | messageId | deviceLocation | type | unit |
   | --- | --- | --- | --- | --- | --- |
   | FXXX | pumpRate | LINE\_DATA | EU | Flow Rate | ft3/s |
   | FXXX | oilPressure | LINE\_DATA | EU | Engine Oil Pressure | psi |
   | FYYY | pumpRate | LINE\_DATA | US | Flow Rate | ft3/s |
   | FYYY | oilPressure | LINE\_DATA | US | Engine Oil Pressure | psi |

* Time Series Insights-Ereignistabelle (flache Ansicht):

   | deviceId | series.tagId | messageId | deviceLocation | type | unit | timestamp | series.value |
   | --- | --- | --- | --- | --- | --- | --- | --- |
   | FXXX | pumpRate | LINE\_DATA | EU | Flow Rate | ft3/s | 2018-01-17T01:17:00Z | 1.0172575712203979 | 
   | FXXX | oilPressure | LINE\_DATA | EU | Engine Oil Pressure | psi | 2018-01-17T01:17:00Z | 34.7 |
   | FXXX | pumpRate | LINE\_DATA | EU | Flow Rate | ft3/s | 2018-01-17T01:17:00Z | 2.445906400680542 | 
   | FXXX | oilPressure | LINE\_DATA | EU | Engine Oil Pressure | Psi | 2018-01-17T01:17:00Z | 49.2 |
   | FYYY | pumpRate | LINE\_DATA | US | Flow Rate | ft3/s | 2018-01-17T01:18:00Z | 0.58015072345733643 |
   | FYYY | oilPressure | LINE\_DATA | US | Engine Oil Pressure | psi | 2018-01-17T01:18:00Z | 22.2 |

Zu den obigen Tabellen:

- Die Spalten **deviceId** und **series.tagId** dienen als Spaltenüberschriften für die verschiedenen Geräte und Tags in einem Bestand. Eine Verwendung als eigenes Attribut hätte die Abfrage mit den weiteren sechs Spalten auf insgesamt 594 (S1-Umgebungen) oder 794 (S2-Umgebungen) beschränkt.

- Unnötige Eigenschaften werden (aus dem gleichen Grund wie im ersten Beispiel) vermieden.

- Verweisdaten werden verwendet, um die Anzahl von Bytes zu verringern, die über das Netzwerk übertragen werden. Dies wird durch die Einführung von **deviceId** für ein eindeutiges Paar aus **messageId** und **deviceLocation** erreicht. Für das eindeutige Paar aus **type** und **unit.** wird ein zusammengesetzter Schlüssel verwendet: **series.tagId**. Der zusammengesetzte Schlüssel ermöglicht die Verwendung des Paars aus **deviceId** und **series.tagId**, um auf vier Werte zu verweisen: **messageId, deviceLocation, type** und **unit**. Diese Daten werden zum Zeitpunkt ihres Eingangs mit den Telemetriedaten verknüpft und anschließend zur Abfrage in Time Series Insights gespeichert.

- Es werden (aus dem gleichen Grund wie im ersten Beispiel) zwei Schachtelungsebenen verwendet.

### <a name="for-both-scenarios"></a>Für beide Szenarien

Wenn Sie über eine Eigenschaft mit einer großen Anzahl möglicher Werte verfügen, werden diese am besten als eindeutige Werte innerhalb einer einzelnen Spalte gesendet, statt eine neue Spalte für jeden Wert zu erstellen. Aus den zwei vorangehenden Beispielen:

  - Im ersten Beispiel liegen einige wenige Eigenschaften mit verschiedenen Werten vor, deshalb ist es angemessen, jeweils eine separate Eigenschaft zu verwenden.
  - Im zweiten Beispiel hingegen werden die Messwerte nicht als individuelle Eigenschaften, sondern als ein Array aus Werten/Messwerten unterhalb einer gemeinsamen series-Eigenschaft angegeben. Es wird ein neuer Schlüssel – **tagId** – gesendet, mit dem eine neue Spalte **series.tagId** in der flachen Tabelle erstellt wird. Anhand von Verweisdaten werden die neuen Eigenschaften **type** und **unit** erstellt, um das Erreichen des Eigenschaftenlimits zu verhindern.

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie den Artikel [Azure Time Series Insights-Abfragesyntax](/rest/api/time-series-insights/ga-query-syntax), um mehr über die Abfragesyntax der REST-API für den TSI-Datenzugriff zu erfahren

- Lesen Sie mehr über das [Strukturieren von Ereignissen](./time-series-insights-send-events.md).
