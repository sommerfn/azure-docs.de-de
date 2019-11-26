---
title: API-Versionsverwaltung für .NET und REST
titleSuffix: Azure Cognitive Search
description: Versionsrichtlinie für REST-APIs für die kognitive Azure-Suche und die Clientbibliothek im .NET SDK.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 03dbb679c25ea692d2c52f80b9493889e367823d
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74112148"
---
# <a name="api-versions-in-azure-cognitive-search"></a>API-Versionen in der kognitiven Azure-Suche

Für die kognitive Azure-Suche sind regelmäßig Featureupdates verfügbar. Manchmal, aber nicht immer, erfordern diese Updates eine neue Version der API, um die Abwärtskompatibilität zu gewährleisten. Die Veröffentlichung einer neuen Version ermöglicht Ihnen, zu steuern, wann und wie Sie Suchdienstupdates in Ihren Code integrieren.

In der Regel versucht das Team für die kognitive Azure-Suche, neue Versionen nur im Bedarfsfall zu veröffentlichen, da es einen gewissen Aufwand erfordern kann, wenn Sie Ihren Code aktualisieren, um eine neue API-Version zu verwenden. Eine neue Version ist nur dann erforderlich, wenn sich ein Aspekt der API in einer Weise geändert hat, die die Abwärtskompatibilität beeinträchtigt. Ursache für solche Änderungen können Fehlerbehebungen bei vorhandenen Funktionen sein oder neue Funktionen, die vorhandene API-Oberflächenbereiche ändern.

Die gleiche Regel gilt bei SDK-Updates. Das SDK für die kognitive Azure-Suche folgt den Regeln der [semantischen Versionierung](https://semver.org/), d.h., dass seine Version aus drei Teilen besteht: Haupt-, Neben- und Buildnummer (z.B. 1.1.0). Eine neue Hauptversion des SDK wird nur dann veröffentlicht, wenn Änderungen die Abwärtskompatibilität beeinträchtigen. Bei Funktionsupdates ohne Beeinträchtigung der Abwärtskompatibilität wird die Nebenversion, bei Fehlerbehebungen nur die Buildversion heraufgesetzt.

> [!NOTE]
> Ihre Dienstinstanz für die kognitive Azure-Suche unterstützt mehrere REST-API-Versionen, einschließlich der neuesten. Sie können auch ältere Versionen weiterhin verwenden, aber es wird empfohlen, den Code zur neuesten Version zu migrieren. Wenn Sie die REST-API verwenden, müssen Sie die API-Version bei jeder Anforderung über den api-version-Parameter angeben. Wenn Sie das .NET SDK verwenden, bestimmt die Version des von Ihnen verwendeten SDK die Version der REST-API. Wenn Sie ein älteres SDK verwenden, können Sie diesen Code weiterhin ohne Änderungen ausführen, selbst wenn der Dienst aktualisiert wird, um eine neuere Version der API zu unterstützen.

## <a name="snapshot-of-current-versions"></a>Momentaufnahme aktueller Versionen
Unten sehen Sie eine Momentaufnahme der aktuellen Versionen aller Programmierschnittstellen zur kognitiven Azure-Suche.


| Schnittstellen | Letzte Hauptversion | Status |
| --- | --- | --- |
| [.NET SDK](https://aka.ms/search-sdk) |9.0 |Allgemein verfügbar, veröffentlicht Mai 2019 |
| [.NET SDK-Vorschau](https://aka.ms/search-sdk-preview) |8.0-preview |Vorschau, veröffentlicht im April 2019 |
| [Dienst-REST-API](https://docs.microsoft.com/rest/api/searchservice/) |2019-05-06 |Allgemein verfügbar |
| [Dienste-REST-API 2019-05-06-Preview](search-api-preview.md) |2019-05-06-Preview |Vorschau |
| [.NET Management SDK](https://aka.ms/search-mgmt-sdk) |3.0 |Allgemein verfügbar |
| [Verwaltungs-REST-API](https://docs.microsoft.com/rest/api/searchmanagement/) |2015-08-19 |Allgemein verfügbar |

Für die REST-APIs muss bei jedem Aufruf die `api-version` einbezogen werden. Die Verwendung von `api-version` erleichtert das Ansprechen einer bestimmten Version, z.B. einer Vorschau-API. Das folgende Beispiel veranschaulicht, wie der Parameter `api-version` angegeben wird:

    GET https://my-demo-app.search.windows.net/indexes/hotels?api-version=2019-05-06

> [!NOTE]
> Obwohl jede Anforderung über eine `api-version` verfügt, sollten Sie für alle API-Anforderungen die gleiche Version verwenden. Dies gilt insbesondere dann, wenn neue API-Versionen Attribute oder Vorgänge einführen, die von früheren Versionen nicht erkannt werden. Das Mischen von API-Versionen kann unerwartete Konsequenzen haben und sollte vermieden werden.
>
> Die Dienst-REST-API und Verwaltungs-REST-API werden voneinander unabhängig versioniert. Jede Ähnlichkeit von Versionsnummern ist zufällig.

Allgemein verfügbare APIs (oder GA) können in der Produktion verwendet werden und unterliegen den Vereinbarungen zum Servicelevel von Azure. Vorschauversionen weisen experimentelle Funktionen auf, die nicht immer zu GA-Versionen migriert werden. **Wir raten Ihnen dringend davon ab, Vorschau-APIs in Produktionsanwendungen einzusetzen.**

## <a name="about-preview-and-generally-available-versions"></a>Informationen zu Vorschau- und allgemein verfügbaren Versionen
Die kognitive Azure-Suche bietet immer zuerst Vorabversionen experimenteller Funktionen über die REST-API, dann über Vorabversionen des .NET SDK.

Vorschaufunktionen stehen für Tests und Experimente zur Verfügung, mit dem Ziel, Feedback zu Entwurf und Implementierung der Funktionen zu sammeln. Aus diesem Grund können sich Vorschaufunktionen mit der Zeit ändern – mitunter in einer Weise, die die Abwärtskompatibilität beeinträchtigt. Dies steht im Gegensatz zu Funktionen in der allgemein verfügbaren Version (GA), die stabil sind und sich bis auf kleinere abwärtskompatible Fehlerbehebungen und Verbesserungen eher nicht ändern. Außerdem werden Vorschaufunktionen nicht immer in der GA-Version übernommen.

Aus diesen Gründen raten wir davon ab, Produktionscode zu schreiben, der von Vorschauversionen abhängig ist. Wenn Sie eine ältere Vorschauversion verwenden, sollten Sie zur GA-Version migrieren.

Für das .NET SDK: Anleitung zur Codemigration finden Sie unter [Upgrade auf Version 1.1 des Azure Search .NET SDK](search-dotnet-sdk-migration-version-9.md).

Allgemeine Verfügbarkeit bedeutet, dass die kognitive Azure-Suche jetzt der Vereinbarung zum Servicelevel (SLA) unterliegt. Die SLA finden Sie unter [SLA für die kognitive Azure-Suche](https://azure.microsoft.com/support/legal/sla/search/v1_0/).
