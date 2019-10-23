---
title: Azure Application Insights-Telemetriedatenmodell – Abhängigkeitstelemetrie | Microsoft-Dokumentation
description: Application Insights-Datenmodell für Abhängigkeitstelemetrie
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: mrbullwinkle
ms.author: mbullwin
ms.date: 04/17/2017
ms.reviewer: sergkanz
ms.openlocfilehash: e0923c20b11aa02f380af1faa6766d2346ad1fb2
ms.sourcegitcommit: 1bd2207c69a0c45076848a094292735faa012d22
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72677428"
---
# <a name="dependency-telemetry-application-insights-data-model"></a>Telemetriedaten zu Abhängigkeiten: Application Insights-Datenmodell

Abhängigkeitstelemetrie (in [Application Insights](../../azure-monitor/app/app-insights-overview.md)) stellt eine Interaktion der überwachten Komponente mit einer Remotekomponente wie SQL oder einem HTTP-Endpunkt dar.

## <a name="name"></a>NAME

Name des Befehls, der mit diesem Abhängigkeitsaufruf ausgelöst wird. Niedriger Kardinalitätswert. Beispiele sind der Name einer gespeicherten Prozedur und eine URL-Pfadvorlage.

## <a name="id"></a>id

Bezeichner einer Instanz eines Aufrufs einer Abhängigkeit. Dient zur Korrelation mit dem Anforderungstelemetrieelement, das diesem Aufruf einer Abhängigkeit entspricht. Weitere Informationen hierzu finden Sie auf der Seite [Korrelation](../../azure-monitor/app/correlation.md).

## <a name="data"></a>Daten

Der durch diesen Aufruf einer Abhängigkeit ausgelöste Befehl. Beispiele sind eine SQL-Anweisung und HTTP-URL mit allen Abfrageparametern.

## <a name="type"></a>type

Name des Abhängigkeitstyps. Niedriger Kardinalitätswert für die logische Gruppierung von Abhängigkeiten und Interpretation der anderen Felder wie „commandName“ und „resultCode“. Beispiele sind die SQL, Azure-Tabelle und HTTP.

## <a name="target"></a>Ziel

Zielstandort eines Aufrufs einer Abhängigkeit. Beispiele sind Servername und Hostadresse. Weitere Informationen hierzu finden Sie auf der Seite [Korrelation](../../azure-monitor/app/correlation.md).

## <a name="duration"></a>Duration

Dauer der Anforderung im Format `DD.HH:MM:SS.MMMMMM`. Muss kleiner als `1000` Tage sein.

## <a name="result-code"></a>Ergebniscode

Ergebniscode eines Aufrufs einer Abhängigkeit. Beispiele sind SQL-Fehlercode und HTTP-Statuscode.

## <a name="success"></a>Erfolgreich

Angabe eines erfolgreichen oder fehlgeschlagenen Aufrufs.

## <a name="custom-properties"></a>Benutzerdefinierte Eigenschaften

[!INCLUDE [application-insights-data-model-properties](../../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Benutzerdefinierte Messungen

[!INCLUDE [application-insights-data-model-measurements](../../../includes/application-insights-data-model-measurements.md)]


## <a name="next-steps"></a>Nächste Schritte

- Einrichten der Abhängigkeitsnachverfolgung für [.NET](../../azure-monitor/app/asp-net-dependencies.md).
- Einrichten der Abhängigkeitsnachverfolgung für [Java](../../azure-monitor/app/java-agent.md).
- [Schreiben benutzerdefinierter Telemetriedaten zu Abhängigkeiten](../../azure-monitor/app/api-custom-events-metrics.md#trackdependency)
- Lesen Sie die Informationen zu den Application Insights-Typen und zum Datenmodell unter [Datenmodell](data-model.md).
- Lesen Sie die Informationen zu den von Application Insights unterstützten [Plattformen](../../azure-monitor/app/platforms.md).
