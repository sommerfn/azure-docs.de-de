---
title: 'Azure Cosmos DB: SQL .NET Standard-API, -SDK und -Ressourcen'
description: Erhalten Sie alle Informationen zu SQL-API und .NET SDK, einschließlich Veröffentlichungsterminen, Deaktivierungsterminen und Änderungen an den einzelnen Versionen des Azure Cosmos DB .NET SDK.
author: deborahc
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
ms.date: 07/12/2019
ms.author: dech
ms.openlocfilehash: 0368e99135dd6e377dd2820b3e673c55182319b9
ms.sourcegitcommit: e3b0fb00b27e6d2696acf0b73c6ba05b74efcd85
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68663812"
---
# <a name="azure-cosmos-db-net-standard-sdk-for-sql-api-download-and-release-notes"></a>Azure Cosmos DB .NET Standard-SDK für SQL-API: Download und Versionshinweise
> [!div class="op_single_selector"]
> * [.NET Standard](sql-api-sdk-dotnet-standard.md)
> * [.NET](sql-api-sdk-dotnet.md)
> * [.NET-Änderungsfeed](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Async Java](sql-api-sdk-async-java.md)
> * [Java](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [REST-Ressourcenanbieter](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](sql-api-query-reference.md)
> * [Bulk Executor – .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [Bulk Executor – Java](sql-api-sdk-bulk-executor-java.md)

| |  |
|---|---|
|**SDK-Download**|[NuGet](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/)|
|**API-Dokumentation**|[.NET API-Referenzdokumentation](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet)|
|**Beispiele**|[.NET-Codebeispiele](https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/CodeSamples)|
|**Erste Schritte**|[Erste Schritte mit dem Azure Cosmos DB .NET SDK](sql-api-get-started.md)|
|**Web-App-Tutorial**|[Entwicklung von Webanwendungen mit Azure Cosmos DB](sql-api-dotnet-application.md)|
|**Aktuelles unterstütztes Framework**|[Microsoft .NET Standard 2.0](/dotnet/standard/net-standard)|

## <a name="release-notes"></a>Versionshinweise
### <a name="a-name310310"></a><a name="3.1.0"/>3.1.0
#### <a name="added"></a>Hinzugefügt
- [#541](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/541): Konsistenzebene zu Client- und Abfrageoptionen hinzugefügt
- [#544](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/544): Unterstützung für Fortsetzungstoken für LINQ hinzugefügt
- [#557](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/557): Triggeroptionen zu Elementanforderungsoptionen hinzugefügt
- [#571](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/571): Standardmäßiges JSON.NET-Serialisierungsmodul mit optionalen Einstellungen hinzugefügt
- [#572](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/572): Partitionsschlüsselüberprüfung in CreateContainerIfNotExistsAsync hinzugefügt
- [#581](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/581): Hinzufügen von LINQ zur QueryDefinition-API
- [#592](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/592) CreateIfNotExistsAsync zu Container-Generator hinzugefügt
- [#597](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/597): Fortsetzungstokeneigenschaft zu ResponseMessage hinzugefügt
- [#604](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/604): LINQ-ToStreamIterator-Erweiterungsmethode hinzugefügt

#### <a name="fixed"></a>Korrigiert
- [#548](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/548): Falsche Meldung in CosmosException.ToString(); korrigiert
- [#558](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/558): Korrektur für LocationCache ConcurrentDict-Sperrkonflikt
- [#561](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/561): GetItemLinqQueryable funktioniert jetzt mit NULL-Abfrage
- [#567](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/567): Abfrage verarbeitet verschiedene Sprachkulturen korrekt
- [#574](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/574): Leere Fehlermeldung bei unerwarteter Ausnahme für Abfrageanalyse korrigiert
- [#576](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/576): Abfrage serialisiert die Eingabe ordnungsgemäß in eine Stream

### <a name="a-name300300"></a><a name="3.0.0"/>3.0.0 
* Allgemeine Verfügbarkeit der [Version 3.0.0](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/) des .NET SDK
* Bestimmt für .NET Standard 2.0, was .NET Framework 4.6.1 und höher sowie .NET Core 2.0 und höher unterstützt
* Neues Objektmodell, bei dem CosmosClient und Methoden der obersten Ebene auf die entsprechenden Datenbank- und Containerklassen aufgeteilt werden.
* Neue hoch leistungsfähige Stream-APIs
* Integrierte Unterstützung für Change Feed Processor-APIs
* Fließende Generator-APIs für CosmosClient-, Container- und Change Feed Processor
* Idiomatische Durchsatzverwaltungs-APIs
* Granulare RequestOptions und ResponseTypes für Datenbank-, Container-, Element-, Abfrage- und Durchsatzanforderungen
* Möglichkeit zum Skalieren nicht partitionierter Container 
* Erweiterbares und anpassbares Serialisierungsmodul
* Erweiterbare Anforderungspipeline mit Unterstützung für benutzerdefinierte Handler


## <a name="release--retirement-dates"></a>Veröffentlichungs- und Deaktivierungstermine
Wenn Microsoft ein SDK deaktiviert, werden Sie mindestens **12 Monate** vorher benachrichtigt, um einen reibungslosen Übergang zu einer neueren/unterstützten Version zu gewährleisten.

Neue Features, Funktionen und Optimierungen werden nur dem aktuellen SDK hinzugefügt. Daher wird empfohlen, immer so früh wie möglich auf die neueste SDK-Version zu aktualisieren. 

Anforderungen an Azure Cosmos DB mithilfe eines deaktivierten SDK werden vom Dienst abgelehnt.

<br/>

| Version | Herausgabedatum | Deaktivierungstermine |
| --- | --- | --- |
| [3.1.0](#3.1.0) |29. Juli 2019 |--- |
| [3.0.0](#3.0.0) |15. Juli 2019 |--- |


## <a name="faq"></a>Häufig gestellte Fragen
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Weitere Informationen
Weitere Informationen zu Cosmos DB finden Sie auf der Seite zum Dienst [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/). 

