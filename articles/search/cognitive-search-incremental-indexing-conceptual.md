---
title: Einführung in die inkrementelle Indizierung (Vorschau)
titleSuffix: Azure Cognitive Search
description: Konfigurieren Sie Ihre KI-Anreicherungspipeline für letztliche Datenkonsistenz, um für jegliche Aktualisierungen von Qualifikationen, Skillsets Indexern oder Datenquellen gewappnet zu sein. Dieses Feature ist zurzeit als öffentliche Vorschauversion verfügbar.
manager: nitinme
author: Vkurpad
ms.author: vikurpad
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 94ce056185ff6a804521bf583ac4f6ffaa513fb0
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73715420"
---
# <a name="what-is-incremental-indexing-in-azure-cognitive-search"></a>Was ist die inkrementelle Indizierung in Azure Cognitive Search?

> [!IMPORTANT] 
> Die inkrementelle Indizierung ist derzeit als öffentliche Vorschauversion verfügbar. Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Dieses Feature wird durch die [REST-API-Version 2019-05-06-Preview](search-api-preview.md) bereitgestellt. Derzeit werden weder das Portal noch das .NET SDK unterstützt.

Die inkrementelle Indizierung ist ein neues Feature von Azure Cognitive Search, das Zwischenspeicherung und Status zu angereicherten Inhalten in einem kognitiven Skillset hinzufügt, sodass Sie die Verarbeitung und erneute Verarbeitung einzelner Schritte in einer Anreicherungspipeline steuern können. Dies sorgt nicht nur dafür, dass ihre monetären Investitionen in die Verarbeitung gesichert werden, sondern schafft auch ein effizienteres System. Wenn Strukturen und Inhalt zwischengespeichert werden, kann ein Indexer nun ermitteln, welche Qualifikationen sich geändert haben, und nur die geänderten Qualifikationen und alle abhängigen Downstreamqualifikationen ausführen. 

Bei der inkrementellen Indizierung führt die aktuelle Version der Anreicherungspipeline gerade so viele Schritte aus, wie erforderlich sind, um die Konsistenz für alle Dokumente in Ihrem Index sicherzustellen. Für Szenarien, in denen Sie vollständige Kontrolle benötigen, stehen differenzierte Steuerungsmöglichkeiten zur Verfügung, um erwartetes Verhalten außer Kraft zu setzen. Weitere Informationen zur Konfiguration finden Sie unter [Einrichten der inkrementellen Indizierung](search-howto-incremental-index.md).

## <a name="indexer-cache"></a>Indexercache

Die inkrementelle Indizierung ergänzt die Anreicherungspipeline um einen Indexercache. Der Indexer speichert die Ergebnisse der Dokumententschlüsselung sowie die Ausgaben der einzelnen Qualifikationen für jedes Dokument zwischen. Wenn ein Skillset aktualisiert wird, werden lediglich die geänderten Qualifikationen (oder die Downstreamqualifikationen) erneut ausgeführt. Die aktualisierten Ergebnisse werden in den Cache geschrieben, und das Dokument wird im Index und im Wissensspeicher aktualisiert.

Physisch betrachtet ist der Cache ein Speicherkonto. Alle Indizes in einem Suchdienst können für den Indexercache das gleiche Speicherkonto verwenden. Jedem Indexer wird eine eindeutige und unveränderliche Cachekennung zugewiesen.

### <a name="cache-configuration"></a>Cachekonfiguration

Um die inkrementelle Indizierung nutzen zu können, muss die `cache`-Eigenschaft für den Indexer festgelegt werden. Das folgende Beispiel veranschaulicht einen Indexer, bei dem die Zwischenspeicherung aktiviert ist. Bestimmte Teile dieser Konfiguration werden in den folgenden Abschnitten beschrieben.

```json
{
    "name": "myIndexerName",
    "targetIndexName": "myIndex",
    "dataSourceName": "myDatasource",
    "skillsetName": "mySkillset",
    "cache" : {
        "storageConnectionString" : "Your storage account connection string",
        "enableReprocessing": true,
        "id" : "Auto generated Id you do not need to set"
    },
    "fieldMappings" : [],
    "outputFieldMappings": [],
    "parameters":{}
}
```

Wenn Sie diese Eigenschaft zum ersten Mal für einen vorhandenen Indexer festlegen, müssen Sie sie auch zurücksetzen, was dazu führt, dass alle Dokumente in der Datenquelle erneut verarbeitet werden. Das Ziel der inkrementellen Indizierung besteht darin, die Dokumente in Ihrem Index mit Ihrer Datenquelle und der aktuellen Version Ihres Skillsets konsistent zu machen. Das Zurücksetzen des Index ist der erste Schritt in Richtung dieser Konsistenz, da dadurch alle Dokumente entfernt werden, die durch vorherige Versionen des Skillsets angereichert wurden. Der Indexer muss zurückgesetzt werden, um über eine konsistente Baseline zu verfügen.

### <a name="cache-lifecycle"></a>Cachelebenszyklus

Der Lebenszyklus des Caches wird vom Indexer verwaltet. Wird die `cache`-Eigenschaft im Indexer auf „Null“ festgelegt oder die Verbindungszeichenfolge geändert, wird der vorhandene Cache gelöscht. Der Cachelebenszyklus ist auch an den Lebenszyklus des Indexers gebunden. Wird ein Indexer gelöscht, wird auch der zugehörige Cache gelöscht.

### <a name="indexer-cache-mode"></a>Indexercachemodus

Der Indexercache kann in Modi betrieben werden, in denen Daten entweder nur in den Cache geschrieben oder Daten in den Cache geschrieben und zur erneuten Anreicherung von Dokumenten verwendet werden.  Sie können die inkrementelle Bereicherung vorübergehend aussetzen, indem Sie die Eigenschaft `enableReprocessing` im Cache auf `false` festlegen, und sie später durch Festlegen der Eigenschaft auf `true`fortsetzen, um letztliche Konsistenz zu erreichen. Diese Steuerungsmöglichkeit ist insbesondere hilfreich, wenn die Indizierung neuer Dokumente Vorrang vor der Konsistenz in Ihrem Dokumentkorpus haben soll.

## <a name="change-detection-override"></a>Außerkraftsetzung der Änderungserkennung

Mit der inkrementellen Indizierung lassen sich sämtliche Aspekte der Anreicherungspipeline präzise steuern. Diese Steuerungsmöglichkeiten ermöglichen auch den Umgang mit Situationen, in denen eine Änderung möglicherweise unbeabsichtigte Folgen hat. Wenn Sie beispielsweise ein Skillset bearbeiten und die URL für eine benutzerdefinierte Qualifikation aktualisieren, werden die zwischengespeicherten Ergebnisse für die betroffene Qualifikation ungültig. Wenn Sie allerdings lediglich den Endpunkt auf einen anderen virtuellen Computer verschieben oder Ihre Qualifikation mit einem neuen Zugriffsschlüssel erneut bereitstellen, sollen natürlich nicht alle vorhandenen Dokumente erneut verarbeitet werden.

Um sicherzustellen, dass der Indexer nur Anreicherungen vornimmt, die Sie explizit benötigen, kann bei Aktualisierungen des Skillsets optional der Abfragezeichenfolgenparameter `disableCacheReprocessingChangeDetection` auf `true` festgelegt werden. Dadurch wird sichergestellt, dass nur Aktualisierungen für die Qualifikation committet werden. Die Auswirkungen der Änderung auf den vorhandenen Korpus werden dagegen nicht ausgewertet.

Im folgenden Beispiel wird die Verwendung der Abfragezeichenfolge (querystring) veranschaulicht. Er ist Teil der Anforderung mit durch & getrennten Schlüssel/Wert-Paaren. 

```http
PUT https://customerdemos.search.windows.net/skillsets/callcenter-text-skillset?api-version=2019-05-06-Preview&disableCacheReprocessingChangeDetection=true
```

## <a name="cache-invalidation"></a>Cacheinvalidierung

Der umgekehrte Fall wäre ein Szenario, in dem Sie eine neue Version einer benutzerdefinierten Qualifikation bereitstellen, die Anreicherungspipeline unverändert bleibt, eine bestimmte Qualifikation aber ungültig gemacht werden muss und alle betroffenen Dokumente neu verarbeitet werden müssen, um die Vorteile eines aktualisierten Modells zu nutzen. Für solche Fällen steht ein entsprechender Vorgang zur Verfügung, den Sie für das Skillset aufrufen können, um Qualifikationen ungültig zu machen. Die API zum Zurücksetzen von Qualifikationen akzeptiert eine POST-Anforderung mit der Liste der Qualifikationsausgaben im Cache, die ungültig gemacht werden sollen. Weitere Informationen zur API zum Zurücksetzen von Qualifikationen finden Sie unter [Zurücksetzen des Indexers (Search REST-API)](https://docs.microsoft.com/rest/api/searchservice/reset-indexer).

## <a name="bi-directional-change-detection"></a>Bidirektionale Änderungserkennung

Indexer arbeiten nicht nur vorwärts und verarbeiten neue Dokumente, sondern können nun auch rückwärts arbeiten und für Konsistenz bei bereits verarbeiteten Dokumenten sorgen. Aufgrund dieser neuen Funktion ist es wichtig zu verstehen, wie sich Änderungen an den Komponenten Ihrer Anreicherungspipeline auf die Arbeit von Indexern auswirken. Der Indexer fügt anstehende Aufgaben einer Warteschlange hinzu, wenn eine Änderung erkannt wird, die entweder eine Invalidierung oder eine Inkonsistenz relativ zum zwischengespeicherten Inhalt zur Folge hat.

### <a name="invalidating-changes"></a>Änderungen mit Invalidierung

Änderungen mit Invalidierung sind zwar selten, haben aber erhebliche Auswirkungen auf den Zustand Ihrer Anreicherungspipeline. Durch eine solche Änderung wird der gesamte Cache ungültig. Ein Beispiel für eine solche Änderung wäre die Aktualisierung Ihrer Datenquelle. Wenn Sie wissen, dass die Änderung keine Cacheinvalidierung zur Folge hat (etwa bei der Schlüsselrotation für das Speicherkonto), muss der Abfragezeichenfolgenparameter `ignoreResetRequirement` für den Aktualisierungsvorgang der spezifischen Ressource auf `true` festgelegt werden, um sicherzustellen, dass der Vorgang nicht abgelehnt wird.

Im Anschluss folgt eine vollständige Liste der Änderungen, die dazu führen, dass Ihr Cache ungültig wird:

* Änderung des Datenquellentyps
* Änderung des Datenquellencontainers
* Anmeldeinformationen für die Datenquelle
* Richtlinie zur Erkennung von Änderungen für die Datenquelle
* Richtlinie zur Erkennung von Löschungen für die Datenquelle
* Indexerfeldzuordnungen
* Indexerparameter
    * Analysemodus
    * Ausgeschlossene Dateierweiterungen
    * Indizierter Dateierweiterungen
    * Indexspeicher-Metadaten (nur für übergroße Dokumente)
    * Textheader mit Trennzeichen
    * Trennzeichen für Text mit Trennzeichen
    * Dokumentstamm
    * Bildaktion (Änderung an der Art der Bildextraktion)

### <a name="inconsistent-changes"></a>Inkonsistente Änderungen

Ein Beispiel für eine inkonsistente Änderung wäre eine Aktualisierung Ihres Skillsets, die die Änderung einer Qualifikation zur Folge hat. Die Änderung kann dazu führen, dass ein Teil des Caches inkonsistent wird. Der Indexer ermittelt, welche Schritte erforderlich sind, um die Konsistenz wiederherzustellen.  

Im Anschluss folgt eine vollständige Liste der Änderungen, die einen inkonsistenten Cache zur Folge haben:

* Anderer Qualifikationstyp im Skillset Aktualisierung des OData-Typs der Qualifikation
* Aktualisierung qualifikationsspezifischer Parameter wie URL, Standardwerte oder andere Parameter
* Änderung der Qualifikationsausgaben (Rückgabe zusätzlicher oder anderer Ausgaben durch die Qualifikation)
* Qualifikationsaktualisierungen, durch die sich eine andere Herkunft ergibt (Änderung der Qualifikationsverkettung, also Qualifikationseingaben)
* Upstream-Qualifikationsinvalidierung (im Falle der Aktualisierung einer Qualifikation, die eine Eingabe für diese Qualifikation bereitstellt)
* Aktualisierung des Projektionsspeicherorts für den Wissensspeicher, wodurch Dokumente neu projiziert werden müssen
* Änderung der Wissensspeicherprojektionen, wodurch Dokumente neu projiziert werden müssen
* Änderung der Ausgabefeldzuordnungen für einen Indexer, wodurch Dokumente erneut im Index projiziert werden müssen

## <a name="rest-api-reference-for-incremental-indexing"></a>REST-API-Referenz für inkrementelle Indizierung

REST `api-version=2019-05-06-Preview` stellt die APIs für die inkrementelle Indizierung mit Ergänzungen für Indexer, Skillsets und Datenquellen bereit. In der Referenzdokumentation sind diese Ergänzungen zurzeit nicht enthalten. Im folgenden Abschnitt werden API-Änderungen beschrieben.

### <a name="indexers"></a>Indexer

[Indexer erstellen](https://docs.microsoft.com/rest/api/searchservice/create-indexer) und [Indexer aktualisieren](https://docs.microsoft.com/rest/api/searchservice/update-indexer) machen nun neue Eigenschaften im Zusammenhang mit dem Cache verfügbar:

* `StorageAccountConnectionString`: Die Verbindungszeichenfolge für das Speicherkonto, das zum Zwischenspeichern der Zwischenergebnisse verwendet wird.

* `CacheId`: Die Cache-ID (`cacheId`) ist der Bezeichner des Containers innerhalb des `annotationCache`-Speicherkontos, das als Cache für diesen Indexer verwendet wird. Hierbei handelt es sich um einen eindeutigen Cache für den Indexer. Wird der Indexer gelöscht und dann mit dem gleichen Namen erneut erstellt, wird auch `cacheId` erneut generiert. `cacheId` kann nicht festgelegt werden, sondern wird durch den Dienst generiert.

* `EnableReprocessing`: Ist standardmäßig auf `true` festgelegt. Wird die Eigenschaft auf `false` festgelegt, werden zwar weiterhin Dokumente in den Cache geschrieben, vorhandene Dokumente werden jedoch nicht auf der Grundlage der zwischengespeicherten Daten erneut verarbeitet.

Einige Indexer (über [Datenquellen](https://docs.microsoft.com/rest/api/searchservice/create-data-source)) rufen Daten über Abfragen ab. Für Abfragen, die Daten abrufen, unterstützen Indexer außerdem einen neuen Abfragezeichenfolgenparameter: `ignoreResetRequirement` muss auf `true` festgelegt werden, wenn Ihre Aktualisierungsaktion den Cache nicht ungültig machen soll.

### <a name="skillsets"></a>Qualifikationsgruppen

Skillsets unterstützen zwar keine neuen Vorgänge, aber einen neuen Abfragezeichenfolgenparameter: `disableCacheReprocessingChangeDetection` muss auf `true` festgelegt werden, wenn bereits vorhandene Dokumente durch die aktuelle Aktion nicht aktualisiert werden sollen.

### <a name="datasources"></a>Datenquellen

Datenquellen unterstützen zwar keine neuen Vorgänge, aber einen neuen Abfragezeichenfolgenparameter: `ignoreResetRequirement` muss auf `true` festgelegt werden, wenn Ihre Aktualisierungsaktion den Cache nicht ungültig machen soll.

## <a name="best-practices"></a>Bewährte Methoden

Für die Verwendung der inkrementellen Indizierung wird folgende Vorgehensweise empfohlen: Konfigurieren Sie die inkrementelle Indizierung, indem Sie die Cacheeigenschaft für einen neuen Indexer festlegen, oder setzen Sie einen bereits vorhandenen Indexer zurück, und legen Sie die Cacheeigenschaft fest.

Setzen Sie `ignoreResetRequirement` mit Bedacht ein, da diese Option zu unerwünschten Inkonsistenzen in Ihren Daten führen kann, die nicht ohne Weiteres erkennbar sind.

## <a name="takeaways"></a>Wesentliche Punkte

Die inkrementelle Indizierung ist ein leistungsstarkes Feature, das die Änderungsnachverfolgung von der Datenquelle auf alle Aspekte der Anreicherungspipeline erweitert, z.B. die Datenquelle, die aktuelle Version Ihres Skillsets und den Indexer. Sollten sich Ihre Qualifikationen, Skillsets oder Erweiterungen verändern, sorgt die Anreicherungspipeline für letztliche Konsistenz bei Ihren Dokumenten und hält dabei den Aufwand so gering wie möglich.

## <a name="next-steps"></a>Nächste Schritte

Beginnen Sie mit der Verwendung der inkrementellen Indizierung, indem Sie einen vorhandenen Indexer mit einem Cache ausstatten oder den Cache beim Definieren eines neuen Indexers hinzufügen.

> [!div class="nextstepaction"]
> [Einrichten der inkrementellen Indizierung](search-howto-incremental-index.md)
