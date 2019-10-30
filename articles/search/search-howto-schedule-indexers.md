---
title: Festlegen eines Zeitplans für Indexer
titleSuffix: Azure Cognitive Search
description: Legen Sie einen Zeitplan für Indexer der kognitiven Azure-Suche fest, um Inhalte in regelmäßigen Abständen oder zu bestimmten Zeiten zu indizieren.
author: HeidiSteen
manager: nitinme
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: e9d4f49bd0aec1a04b4839b2084a81fb538f7890
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72793696"
---
# <a name="how-to-schedule-indexers-in-azure-cognitive-search"></a>Festlegen eines Zeitplans für Indexer in der kognitiven Azure-Suche

Ein Indexer wird normal einmal ausgeführt, und zwar sofort nach der Erstellung. Sie können ihn über das Portal, die REST-API oder das .NET SDK nach Bedarf erneut ausführen. Sie können den Indexer auch so konfigurieren, dass er regelmäßig nach einem Zeitplan ausgeführt wird.

Situationen, in denen das Festlegen eines Zeitplans für den Indexer nützlich ist:

* Quelldaten ändern sich im Lauf der Zeit, und die Indexer der kognitiven Azure-Suche sollten die geänderten Daten automatisch verarbeiten.
* Der Index wird aus mehreren Datenquellen aufgefüllt, und Sie möchten sicherstellen, dass die Indexer zu unterschiedlichen Zeiten ausgeführt werden, um Konflikte zu vermeiden.
* Die Quelldaten sind sehr groß, und die Indexerausführung soll auf verschiedene Zeiten verteilt werden. Weitere Informationen zur Indizierung großer Datenmengen finden Sie unter [Indizieren großer Datasets in der kognitiven Azure-Suche](search-howto-large-index.md).

Der Scheduler ist eine integrierte Funktion der kognitiven Azure-Suche. Sie können keinen externen Scheduler verwenden, um Azure Search-Indexer zu steuern.

## <a name="define-schedule-properties"></a>Definieren der Zeitplaneigenschaften

Ein Indexerzeitplan besitzt zwei Eigenschaften:
* **Intervall**: Definiert die Zeitspanne zwischen geplanten Indexerausführungen. Das kleinste zulässige Intervall beträgt 5 Minuten, und das größte 24 Stunden.
* **Startzeit (UTC)** : Gibt an, wann der Indexer zum ersten Mal ausgeführt werden soll.

Sie können einen Zeitplan festlegen, wenn Sie den Indexer erstmalig erstellen, oder die Eigenschaften des Indexers später aktualisieren. Zeitpläne für Indexer können mit dem [Portal](#portal), der [REST-API](#restApi) oder dem [.NET SDK](#dotNetSdk) festgelegt werden.

Nur eine Ausführung eines Indexers kann zu einem gegebenen Zeitpunkt ausgeführt werden. Wird ein Indexer zu dem Zeitpunkt, für den seine nächste Ausführung geplant ist, bereits ausgeführt, wird diese Ausführung auf den nächsten geplanten Zeitpunkt verschoben.

Betrachten wir ein Beispiel, um dies zu konkreter veranschaulichen. Angenommen, Sie haben einen Indexerzeitplan mit dem **Intervall** „Stündlich“ und der **Startzeit** „1. Juni 2019 um 8:00:00 Uhr (UTC)“ konfiguriert. Folgendes könnte passieren, wenn eine Indexerausführung länger als eine Stunde dauert:

* Die erste Indexerausführung beginnt am 1. Juni 2019 um oder gegen 8:00 Uhr (UTC). Angenommen, diese Ausführung dauert 20 Minuten (oder hat eine andere Dauer unter 1 Stunde).
* Die zweite Indexerausführung beginnt am 1. Juni 2019 um oder gegen 9:00 Uhr (UTC). Angenommen, diese Ausführung dauert 70 Minuten – also mehr als einer Stunde – und wird erst gegen 10:10 Uhr (UTC) abgeschlossen.
* Der Start der dritten Ausführung ist für 10:00 Uhr (UTC) geplant, aber zu diesem Zeitpunkt ist die vorherige Ausführung noch nicht abgeschlossen. Diese geplante Ausführung wird dann übersprungen. Die nächste Ausführung des Indexers startet somit erst um 11:00 Uhr (UTC).

> [!NOTE]
> Wenn ein Indexer auf einen bestimmten Zeitplan festgelegt ist, im gleichen Dokument bei erneuter Ausführung aber immer wieder ein Fehler auftritt, wird der Indexer in selteneren Intervallen ausgeführt (bis hin zum Maximum von mindestens einmal in 24 Stunden), bis die Ausführung fehlerfrei verläuft.  Wenn Sie der Meinung sind, dass Sie das Problem behoben haben, das dafür gesorgt hat, dass der Indexer an einem bestimmten Punkt hängengeblieben ist, können Sie eine bedarfsgesteuerte Ausführung des Indexers veranlassen. Wenn diese Ausführung erfolgreich verläuft, wird der Indexer wieder im festgelegten Zeitplanintervall ausgeführt.

<a name="portal"></a>

## <a name="schedule-in-the-portal"></a>Festlegen eines Zeitplans im Portal

Mit dem Datenimport-Assistenten im Portal können Sie den Zeitplan für einen Indexer zum Zeitpunkt der Erstellung definieren. Die Standardeinstellung für den Zeitplan ist **Stündlich**. Das bedeutet, dass der Indexer einmal nach der Erstellung ausgeführt und die Ausführung dann stündlich einmal wiederholt wird.

Sie können die Zeitplaneinstellung auf **Einmal** ändern, wenn der Indexer nicht automatisch erneut ausgeführt werden soll, oder auf **Täglich**, um ihn einmal pro Tag auszuführen. Legen Sie ihn auf **Benutzerdefiniert** fest, wenn Sie ein anderes Intervall oder eine bestimmte Startzeit in der Zukunft angeben möchten.

Wenn Sie den Zeitplan auf **Benutzerdefiniert** festlegen, werden weitere Felder angezeigt, in denen Sie das **Intervall** und die **Startzeit (UTC)** angeben können. Das kürzeste zulässige Zeitintervall ist 5 Minuten und das längste 1440 Minuten (24 Stunden).

   ![Festlegen des Indexerzeitplans im Datenimport-Assistenten](media/search-howto-schedule-indexers/schedule-import-data.png "Festlegen des Indexerzeitplans im Datenimport-Assistenten")

Nach der Erstellung eines Indexers können Sie die Zeitplaneinstellungen mithilfe des Bearbeitungsbereichs im Indexer ändern. Die Zeitplanfelder im Datenimport-Assistent sind identisch.

   ![Festlegen des Zeitplans im Bearbeitungsbereich des Indexers](media/search-howto-schedule-indexers/schedule-edit.png "Festlegen des Zeitplans im Bearbeitungsbereich des Indexers")

<a name="restApi"></a>

## <a name="schedule-using-rest-apis"></a>Festlegen eines Zeitplans mithilfe von REST-APIs

Sie können den Zeitplan für einen Indexer mithilfe der REST-API definieren. Dazu fügen Sie die **schedule**-Eigenschaft beim Erstellen oder Aktualisieren des Indexers hinzu. Das folgende Beispiel zeigt eine PUT-Anforderung zum Aktualisieren eines vorhandenen Indexers:

    PUT https://myservice.search.windows.net/indexers/myindexer?api-version=2019-05-06
    Content-Type: application/json
    api-key: admin-key

    {
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name",
        "schedule" : { "interval" : "PT10M", "startTime" : "2015-01-01T00:00:00Z" }
    }

Der Parameter **interval** ist erforderlich. Das Intervall bezieht sich auf den Zeitraum zwischen dem Start von zwei aufeinander folgenden Indexerausführungen. Das kleinste zulässige Intervall beträgt 5 Minuten. Das längste ist ein Tag. Es muss als XSD-Wert „dayTimeDuration“ formatiert sein (eine eingeschränkte Teilmenge eines [ISO 8601-Zeitwerts](https://www.w3.org/TR/xmlschema11-2/#dayTimeDuration)). Das Muster hierfür lautet wie folgt: `P(nD)(T(nH)(nM))`. Beispiele: `PT15M` = alle 15 Minuten, `PT2H` = alle 2 Stunden.

Der optionale Parameter **startTime** gibt an, wann die geplanten Ausführungen beginnen sollen. Wird er weggelassen, wird die aktuelle UTC-Zeit verwendet. Diese Zeitangabe kann in der Vergangenheit liegen. In diesem Fall wird die erste Ausführung so geplant, als wäre der Indexer seit der ursprünglich mit **startTime** angegebenen Startzeit kontinuierlich ausgeführt worden.

Sie können einen Indexer bei Bedarf auch jederzeit mit dem Aufruf „Indexer ausführen“ ausführen. Weitere Informationen zum Ausführen von Indexern und zum Festlegen von Indexerzeitplänen finden Sie unter [Run Indexer](https://docs.microsoft.com/rest/api/searchservice/run-indexer) (Ausführen eines Indexers), [Get Indexer](https://docs.microsoft.com/rest/api/searchservice/get-indexer) (Abrufen eines Indexers) und [Update Indexer](https://docs.microsoft.com/rest/api/searchservice/update-indexer) (Aktualisieren eines Indexers) in der REST-API-Referenz.

<a name="dotNetSdk"></a>

## <a name="schedule-using-the-net-sdk"></a>Festlegen eines Zeitplans mit dem .NET SDK

Sie können den Zeitplan für einen Indexer mithilfe des .NET SDK der kognitiven Azure-Suche definieren. Dazu fügen Sie die **schedule**-Eigenschaft beim Erstellen oder Aktualisieren eines Indexers hinzu.

Das folgende C#-Beispiel erstellt einen Indexer unter Verwendung einer vordefinierten Datenquelle und eines Index, und legt den Zeitplan so fest, dass der Indexer in 30 Minuten ab jetzt zum ersten Mal und dann einmal pro Tag ausgeführt wird:

```
    Indexer indexer = new Indexer(
        name: "azure-sql-indexer",
        dataSourceName: dataSource.Name,
        targetIndexName: index.Name,
        schedule: new IndexingSchedule(
                        TimeSpan.FromDays(1), 
                        new DateTimeOffset(DateTime.UtcNow.AddMinutes(30))
                    )
        );
    await searchService.Indexers.CreateOrUpdateAsync(indexer);
```
Wenn der **schedule**-Parameter ausgelassen wird, wird der Indexer nur einmal ausgeführt, unmittelbar nachdem er erstellt wurde.

Der **startTime**-Parameter kann auf einen Zeitpunkt in der Vergangenheit festgelegt werden. In diesem Fall wird die erste Ausführung so geplant, als wäre der Indexer seit der mit **startTime** angegebenen Startzeit kontinuierlich ausgeführt worden.

Der Zeitplan wird mit der [IndexingSchedule](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexingschedule?view=azure-dotnet)-Klasse definiert. Der **IndexingSchedule**-Konstruktor erfordert einen **interval**-Parameter, der mit einem **TimeSpan**-Objekt angegeben wird. Das kleinste zulässige Intervall beträgt 5 Minuten, und das größte 24 Stunden. Der zweite **startTime**-Parameter, der als ein **DateTimeOffset**-Objekt angegeben wird, ist optional.

Mit dem .NET SDK können Sie die Indexervorgänge mithilfe der [SearchServiceClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchserviceclient)-Klasse und der zugehörigen [Indexers](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchserviceclient.indexers)-Eigenschaft steuern, die die Methoden aus der **IIndexersOperations**-Schnittstelle implementiert. 

Sie können einen Indexer bei Bedarf jederzeit mit einem der [Run](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.indexersoperationsextensions.run), [RunAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.indexersoperationsextensions.runasync) oder [RunWithHttpMessagesAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.iindexersoperations.runwithhttpmessagesasync)-Methoden ausführen.

Weitere Informationen zum Erstellen, Aktualisieren und Ausführen von Indexern finden Sie unter [IIindexersOperations](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.iindexersoperations?view=azure-dotnet).
