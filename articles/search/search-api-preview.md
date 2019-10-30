---
title: REST-API-Version 2019-05-06-Preview
titleSuffix: Azure Cognitive Search
description: Die REST-API für den Dienst für die kognitive Azure-Suche, Version 2019-05-06-Preview, beinhaltet experimentelle Features wie Wissensspeicher und von Kunden verwaltete Verschlüsselungsschlüssel.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: a4daaac6abababcedb5d6dd7eb2122ef29846ef4
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72792530"
---
# <a name="azure-cognitive-search-service-rest-api-version-2019-05-06-preview"></a>REST-API für den Dienst für die kognitive Azure-Suche: Version 2019-05-06-Preview

Dieser Artikel beschreibt die Version `api-version=2019-05-06-Preview` der Search-Dienst-REST-API, welche die folgenden experimentellen Features bietet, die noch nicht allgemein verfügbar sind.

> [!NOTE]
> Vorschaufeatures stehen für Tests und Versuche zur Verfügung, um Feedback zu sammeln, und können sich ändern. Wir raten dringend davon ab, Vorschau-APIs in Produktionsanwendungen einzusetzen.


## <a name="new-in-2019-05-06-preview"></a>Neuerungen in 2019-05-06-Preview

Der [**Wissensspeicher** ](knowledge-store-concept-intro.md) ist ein neues Ziel einer Pipeline für KI-basierte Anreicherung. Zusätzlich zu einem Index können Sie nun aufgefüllte Datenstrukturen speichern, die während der Indizierung in Azure Storage erstellt wurden. Sie steuern die physischen Strukturen Ihrer Daten durch Elemente in einer Qualifikationsgruppe, z. B. wie Daten dargestellt werden, ob Daten in Table Storage oder Blob Storage gespeichert werden und ob mehrere Ansichten verfügbar sind.

Auch die [**von Kunden verwalteten Verschlüsselungsschlüssel**](search-security-manage-encryption-keys.md) für die dienstseitige Verschlüsselung ruhender Daten stellen eine neue Previewfunktion dar. Neben der integrierten von Microsoft verwalteten Verschlüsselung ruhender Daten können Sie eine zusätzliche Verschlüsselungsebene anwenden, bei der Sie alleiniger Besitzer der Schlüssel sind.

## <a name="other-preview-features"></a>Weitere Vorschaufeatures

In früheren Vorschauversionen angekündigte Features befinden sich immer noch in der öffentlichen Vorschau. Wenn Sie eine API mit einer früheren API-Vorschauversion aufrufen, können Sie weiterhin diese Version verwenden oder zu `2019-05-06-Preview` wechseln, ohne Änderungen am erwarteten Verhalten feststellen zu müssen.

+ [moreLikeThis-Abfrageparameter](search-more-like-this.md) sucht Dokumente, die für ein bestimmtes Dokument relevant sind. Dieses Feature war in früheren Vorschauversionen enthalten. 
* [CSV-Blobindizierung](search-howto-index-csv-blobs.md) erstellt ein Dokument pro Zeile, statt einem Dokument pro Textblob.
* Die [MongoDB-API-Unterstützung für Cosmos DB-Indexer](search-howto-index-cosmosdb.md) befindet sich in der Vorschau.


## <a name="how-to-call-a-preview-api"></a>Aufrufen einer Vorschau-API

Ältere Vorschauversionen sind zwar noch betriebsbereit, veralten jedoch mit der Zeit. Wenn Ihr Code `api-version=2016-09-01-Preview` oder `api-version=2017-11-11-Preview` aufruft, sind diese Aufrufe noch gültig. Allerdings wird nur die neueste Vorschauversion mit Verbesserungen aktualisiert. 

Die folgende Beispielsyntax veranschaulicht einen Aufruf der API-Vorschauversion.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?search=*&api-version=2019-05-06-Preview

Der Dienst für die kognitive Azure-Suche ist in mehreren Versionen verfügbar. Weitere Informationen finden Sie unter [API-Versionen](search-api-versions.md).

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die Referenzdokumentation für die Search-REST-API an. Wenn Probleme auftreten, können Sie sich über [Stack Overflow](https://stackoverflow.com/) an uns wenden oder [den Support kontaktieren](https://azure.microsoft.com/support/community/?product=search).

> [!div class="nextstepaction"]
> [Referenz zur REST-API für den Azure Search-Dienst](https://docs.microsoft.com/rest/api/searchservice/)