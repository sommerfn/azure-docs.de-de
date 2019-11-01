---
title: Abfragen von Daten aus einer Azure Time Series Insights GA-Umgebung mit C#-Code | Microsoft-Dokumentation
description: Dieser Artikel beschreibt, wie durch Codieren einer in der .NET-Sprache C# (C-Sharp) geschriebenen benutzerdefinierten App Daten aus einer Azure Time Series Insights-Umgebung abgefragt werden.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 10/03/2019
ms.custom: seodec18
ms.openlocfilehash: 23e4ad5667019b5bc38f9ba11d14632d4d55d5b7
ms.sourcegitcommit: 92d42c04e0585a353668067910b1a6afaf07c709
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/28/2019
ms.locfileid: "72989977"
---
# <a name="query-data-from-the-azure-time-series-insights-ga-environment-using-c"></a>Abfragen von Daten aus der Azure Time Series Insights GA-Umgebung mit C#

In diesem C#-Beispiel wird gezeigt, wie Sie Daten aus der Azure Time Series Insights GA-Umgebung abfragen können.

Das Beispiel zeigt einige einfache Beispiele für die Verwendung der Abfrage-API:

1. Rufen Sie zur Vorbereitung das Zugriffstoken mithilfe der Azure Active Directory-API ab. Übergeben Sie dieses Token im `Authorization`-Header jeder Abfrage-API-Anforderung. Informationen zum Einrichten nicht interaktiver Anwendungen finden Sie unter [Authentifizierung und Autorisierung](time-series-insights-authentication-and-authorization.md). Stellen Sie außerdem sicher, dass alle Konstanten, die zu Anfang des Beispiels definiert wurden, ordnungsgemäß festgelegt sind.
1. Die Liste der Umgebungen, auf die der Benutzer Zugriff hat, wird abgerufen. Eine der Umgebungen wird als relevant übernommen, und für diese Umgebung werden weitere Daten abgefragt.
1. Als Beispiel für eine HTTPS-Anforderung werden Verfügbarkeitsdaten für die relevante Umgebung abgefragt.
1. Als Beispiel für eine Websocketanforderung werden aggregierte Ereignisdaten für die relevante Umgebung abgefragt. Daten werden für den gesamten Verfügbarkeitszeitbereich abgefragt.

> [!NOTE]
> Der Beispielcode ist verfügbar unter [https://github.com/Azure-Samples/Azure-Time-Series-Insights](https://github.com/Azure-Samples/Azure-Time-Series-Insights/tree/master/csharp-tsi-ga-sample).

## <a name="project-dependencies"></a>Projektabhängigkeiten

Fügen Sie die NuGet-Pakete `Microsoft.IdentityModel.Clients.ActiveDirectory` und `Newtonsoft.Json` hinzu.

## <a name="c-example"></a>C#-Beispiel

[!code-csharp[csharpquery-example](~/samples-tsi/csharp-tsi-ga-sample/Program.cs)]

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Abfragen finden Sie in der [Abfrage-API-Referenz](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api).

- Lesen Sie die Informationen, wie Sie [eine JavaScript-App mithilfe des Client-SDKs ](https://github.com/microsoft/tsiclient) mit Time Series Insights verbinden.