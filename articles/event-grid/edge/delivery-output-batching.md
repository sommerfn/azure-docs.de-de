---
title: Ausgabebatchverarbeitung in Azure Event Grid IoT Edge | Microsoft-Dokumentation
description: Ausgabebatchverarbeitung in Event Grid in IoT Edge.
author: HiteshMadan
manager: rajarv
ms.author: himad
ms.reviewer: spelluru
ms.date: 10/06/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 7d6b83354baf3db5ddb65f94fee1c3dce2dcca94
ms.sourcegitcommit: 92d42c04e0585a353668067910b1a6afaf07c709
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/28/2019
ms.locfileid: "72991823"
---
# <a name="output-batching"></a>Ausgabebatches

Event Grid kann mehrere Ereignisse in einer einzelnen Übermittlungsanforderung zustellen. Durch dieses Feature kann der Gesamtdurchsatz der Übermittlungen erhöht werden, ohne dass ein HTTP-Mehraufwand pro Anforderung auftritt. Die Batchverarbeitung ist standardmäßig deaktiviert und kann für einzelne Abonnements aktiviert werden.

> [!WARNING]
> Die maximal zulässige Verarbeitungsdauer jeder Übermittlungsanforderung ändert sich nicht, selbst wenn für den Abonnentencode potenziell mehr Aufwand pro Anforderung im Batch entsteht. Das Zeitlimit für Übermittlungen beträgt standardmäßig 60 Sekunden.

## <a name="batching-policy"></a>Richtlinie für die Batchverarbeitung

Das Verhalten der Batchverarbeitung von Event Grid kann für einzelne Abonnenten angepasst werden; hierfür sind die folgenden zwei Einstellungen zu optimieren:

* Maximale Anzahl von Ereignissen pro Batch

  Durch diese Einstellung wird eine Obergrenze für die Anzahl der Ereignisse festgelegt, die einer Batch-Übermittlungsanforderung hinzugefügt werden können.

* Bevorzugte Batchgröße in KB

  Dieser Knopf dient zur weiteren Anpassung der maximalen Anzahl von Kilobytes, die per Übermittlungsanforderung gesendet werden können.

## <a name="batching-behavior"></a>Verhalten der Batchverarbeitung

* Alle oder keine

  Event Grid baut auf einer Alle-oder-keine-Semantik auf. Ein teilweiser Erfolg einer Übermittlung im Batch wird nicht unterstützt. Abonnenten sollten darauf achten, nur so viele Ereignisse pro Batch anzufordern, wie Sie nach sinnvollem Ermessen in 60 Sekunden tatsächlich verarbeiten können.

* Optimistische Batchverarbeitung

  Die Richtlinieneinstellungen für die Batchverarbeitung sind keine strengen Vorgaben für das Batchverarbeitungsverhalten, sie werden jeweils im Hinblick auf bestmögliche Leistung eingehalten. Bei niedrigen Ereignisraten stellen Sie häufig fest, dass die Batchgröße unter der angeforderten maximalen Anzahl von Ereignissen pro Batch liegt.

* Standardwert ist AUS

  Standardmäßig fügt Event Grid jeder Übermittlungsanforderung nur ein Ereignis hinzu. Zum Aktivieren der Batchverarbeitung müssen Sie eine der zuvor in diesem Artikel beschriebenen Einstellungen im JSON-Code des Ereignisabonnements festlegen.

* Standardwerte

  Beim Erstellen eines Ereignisabonnements müssen nicht beide Einstellungen („Maximale Anzahl von Ereignissen pro Batch“ und „Bevorzugte Batchgröße in KB“) angegeben werden. Wenn nur eine Einstellung festgelegt ist, verwendet Event Grid (konfigurierbare) Standardwerte. In den folgenden Abschnitten finden Sie die Standardwerte und es wird beschrieben, wie dieser außer Kraft gesetzt werden können.

## <a name="turn-on-output-batching"></a>Aktivieren der Ausgabebatchverarbeitung

```json
{
    "properties":
    {
        "destination":
        {
            "endpointType": "WebHook",
            "properties":
             {
                "endpointUrl": "<your_webhook_url>",
                "maxEventsPerBatch": 10,
                "preferredBatchSizeInKilobytes": 64
             }
        },
    }
}
```

## <a name="configuring-maximum-allowed-values"></a>Konfigurieren der maximal zulässigen Werte

Die folgenden zur Bereitstellung festgelegten Einstellungen steuern den zulässigen Höchstwert beim Erstellen eines Ereignisabonnements.

| Eigenschaftenname | BESCHREIBUNG |
| ------------- | ----------- | 
| `api:deliveryPolicyLimits:maxpreferredBatchSizeInKilobytes` | Der zulässige Höchstwert für den Knopf `PreferredBatchSizeInKilobytes`. Standardwert: `1033`.
| `api:deliveryPolicyLimits:maxEventsPerBatch` | Der zulässige Höchstwert für den Knopf `MaxEventsPerBatch`. Standardwert: `50`.

## <a name="configuring-runtime-default-values"></a>Konfigurieren von Runtime-Standardwerten

Die folgenden zur Bereitstellung festgelegten Einstellungen steuern den Runtime-Standardwert jedes Knopfs, wenn dieser nicht im Ereignisabonnement angegeben ist. Für einen erneuten Durchlauf muss mindestens ein Knopf im Ereignisabonnement so festgelegt werden, dass das Batchverarbeitungsverhalten aktiviert ist.

| Eigenschaftenname | BESCHREIBUNG |
| ------------- | ----------- |
| `broker:defaultMaxBatchSizeInBytes` | Maximale Größe der Übermittlungsanforderung, wenn nur `MaxEventsPerBatch` angegeben ist. Standardwert: `1_058_576`.
| `broker:defaultMaxEventsPerBatch` | Maximale Anzahl der einem Batch hinzugefügten Ereignisse, wenn nur `MaxBatchSizeInBytes` angegeben ist. Standardwert: `10`.
