---
title: Einrichten der inkrementellen Indizierung (Vorschau) von angereichertem Inhalt basierend auf der Änderungsnachverfolgung
titleSuffix: Azure Cognitive Search
description: Aktivieren Sie die Änderungsnachverfolgung, und bewahren Sie den Status von angereicherten Inhalten für die kontrollierte Verarbeitung in einem kognitiven Skillset. Dieses Feature ist zurzeit als öffentliche Preview verfügbar.
author: vkurpad
manager: eladz
ms.author: vikurpad
ms.service: cognitive-search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 74631ee3167c65e59fbd05f53fe5327d1b532dba
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73719940"
---
# <a name="how-to-set-up-incremental-indexing-of-enriched-documents-in-azure-cognitive-search"></a>Einrichten der inkrementellen Indizierung von angereicherten Dokumenten in Azure Cognitive Search

> [!IMPORTANT] 
> Die inkrementelle Indizierung ist derzeit als öffentliche Vorschauversion verfügbar. Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Dieses Feature wird durch die [REST-API-Version 2019-05-06-Preview](search-api-preview.md) bereitgestellt. Derzeit werden weder das Portal noch das .NET SDK unterstützt.

In diesem Artikel erfahren Sie, wie Sie Status und Zwischenspeicherung zu mit angereicherten Dokumenten hinzufügen, indem Sie eine Azure Cognitive Search-Anreicherungspipeline durchlaufen, sodass Sie Dokumente aus allen unterstützten Datenquellen inkrementell indizieren können. Standardmäßig ist ein Skillset zustandslos, und eine Änderung eines beliebigen Teils seiner Komposition erfordert eine vollständige erneute Ausführung des Indexers. Bei der inkrementellen Indizierung kann der Indexer ermitteln, welche Teile der Pipeline geändert wurden, indem vorhandene Anreicherungen für unveränderte Teile wiederverwendet und Anreicherungen für die Schritte, die sich ändern, überarbeitet werden. Zwischengespeicherter Inhalt wird in Azure Storage platziert.

Wenn Sie mit dem Einrichten von Indexern nicht vertraut sind, beginnen Sie mit [Übersicht über Indexer](search-indexer-overview.md), und fahren Sie dann mit [Skillsets](cognitive-search-working-with-skillsets.md) fort, um sich über Anreicherungspipelines zu informieren. Weitere Hintergrundinformationen zu Schlüsselkonzepten finden Sie unter [Inkrementelle Indizierung](cognitive-search-incremental-indexing-conceptual.md).

## <a name="modify-an-existing-indexer"></a>Ändern eines vorhandenen Indexers

Wenn Sie über einen vorhandenen Indexer verfügen, führen Sie die folgenden Schritte aus, um die inkrementelle Indizierung zu aktivieren.

### <a name="step-1-get-the-indexer"></a>Schritt 1: Abrufen des Indexers

Beginnen Sie mit einem gültigen, vorhandenen Indexer, bei dem die erforderliche Datenquelle und der Index bereits definiert sind. Der Indexer sollte ausführbar sein. Erstellen Sie mithilfe eines API-Clients eine [GET-Anforderung](https://docs.microsoft.com/rest/api/searchservice/get-indexer), um die aktuelle Konfiguration des Indexers abzurufen, dem Sie die inkrementelle Indizierung hinzufügen möchten.

```http
GET https://[service name].search.windows.net/indexers/[your indexer name]?api-version=2019-05-06-Preview
Content-Type: application/json
api-key: [admin key]
```

### <a name="step-2-add-the-cache-property"></a>Schritt 2: Hinzufügen der cache-Eigenschaft

Bearbeiten Sie die Antwort auf die GET-Anforderung, um dem Indexer die Eigenschaft `cache` hinzuzufügen. Das cache-Objekt erfordert nur eine einzelne Eigenschaft, nämlich die Verbindungszeichenfolge für ein Azure Storage-Konto.

```json
    "cache": {
        "storageConnectionString": "[your storage connection string]"
    }
```

### <a name="step-3-reset-the-indexer"></a>Schritt 3: Zurücksetzen des Indexers

> [!NOTE]
> Das Zurücksetzen des Indexers führt dazu, dass alle Dokumente in Ihrer Datenquelle noch einmal verarbeitet werden, damit Inhalt zwischengespeichert werden kann. Alle kognitiven Anreicherungen werden für alle Dokumente erneut ausgeführt.
>

Ein Zurücksetzen des Indexers ist beim Einrichten der inkrementellen Indizierung für vorhandene Indexer erforderlich, um sicherzustellen, dass sich alle Dokumente in einem konsistenten Zustand befinden. Setzen Sie den Indexer mithilfe der [REST-API](https://docs.microsoft.com/rest/api/searchservice/reset-indexer) zurück.

```http
POST https://[service name].search.windows.net/indexers/[your indexer name]/reset?api-version=2019-05-06-Preview
Content-Type: application/json
api-key: [admin key]
```

### <a name="step-4-save-the-updated-definition"></a>Schritt 4: Speichern der aktualisierten Definition

Aktualisieren Sie die Indexerdefinition mit einer PUT-Anforderung. Der Anforderungstext sollte die aktualisierte Indexerdefinition enthalten.

```http
PUT https://[service name].search.windows.net/indexers/[your indexer name]/reset?api-version=2019-05-06-Preview
Content-Type: application/json
api-key: [admin key]
{
    "name" : "your indexer name",
    ...
    "cache": {
        "storageConnectionString": "[your storage connection string]",
        "enableReprocessing": true
    }
}
```

Wenn Sie jetzt eine weitere GET-Anforderung für den Indexer ausgeben, enthält die Antwort vom Dienst eine Eigenschaft `cacheId` im Cacheobjekt. `cacheId` ist der Name des Containers, der alle zwischengespeicherten Ergebnisse und den Zwischenzustand der einzelnen Dokumente enthält, die von diesem Indexer verarbeitet werden.

## <a name="enable-incremental-indexing-on-new-indexers"></a>Aktivieren der inkrementellen Indizierung für neue Indexer

Schließen Sie zum Einrichten der inkrementellen Indizierung für einen neuen Indexer die `cache`-Eigenschaft in die Indexerdefinition ein. Vergewissern Sie sich, dass Sie die Version `2019-05-06-Preview` der API verwenden.

## <a name="overriding-incremental-indexing"></a>Außerkraftsetzen der inkrementellen Indizierung

Nach dem Konfigurieren verfolgt die inkrementelle Indizierung Änderungen über die gesamte Indizierungspipeline und führt letztendlich zu Konsistenz der Dokumente im gesamten Index und allen Projektionen. In einigen Fällen müssen Sie dieses Verhalten jedoch außer Kraft setzen, um sicherzustellen, dass der Indexer keine zusätzlichen Arbeitsschritte aufgrund einer Aktualisierung der Indizierungspipeline durchführt. Beispielsweise ist beim Aktualisieren der Verbindungszeichenfolge für die Datenquelle ein Zurücksetzen des Indexers und das erneute Indizieren aller Dokumente erforderlich, da sich die Datenquelle geändert hat. Wenn Sie aber nur die Verbindungszeichenfolge mit einem neuen Schlüssel aktualisiert haben, soll die Änderung keine Aktualisierungen vorhandener Dokumente zur Folge haben. Umgekehrt kann es sein, dass der Indexer den Cache für ungültig erklären und Dokumente anreichern soll, auch wenn keine Änderungen an der Indizierungspipeline vorgenommen werden. Beispielsweise können Sie den Indexer für ungültig erklären, wenn Sie eine benutzerdefinierte Qualifikation mit einem neuen Modell bereitstellen möchten und die Qualifikation für alle Ihre Dokumente erneut ausgeführt werden soll.

### <a name="override-reset-requirement"></a>Außerkraftsetzen der Zurücksetzungsanforderung

Bei Änderungen an der Indizierungspipeline, die zum Aufheben der Gültigkeit des Caches führen, ist ein Zurücksetzen des Indexers erforderlich. Wenn Sie eine Änderung an der Indizierungspipeline vornehmen und nicht möchten, dass die Änderungsnachverfolgung den Cache für ungültig erklärt, müssen Sie den Abfragezeichenfolgen-Parameter `ignoreResetRequirement` für Vorgänge im Indexer oder in der Datenquelle auf `true` festlegen.

### <a name="override-change-detection"></a>Außerkraftsetzen der Änderungserkennung

Bei Aktualisierungen der Qualifikationsgruppe, die dazu führen würden, dass Dokumente als inkonsistent gekennzeichnet werden (z.B. Aktualisieren einer URL für benutzerdefinierte Qualifikation beim erneuten Bereitstellen der Qualifikation), legen Sie den Abfragezeichenfolgen-Parameter `disableCacheReprocessingChangeDetection` für Aktualisierungen der Qualifikationsgruppe auf `true` fest.

### <a name="force-change-detection"></a>Erzwingen der Änderungserkennung

Wenn Sie möchten, dass die Indizierungspipeline eine Änderung an einer externen Entität erkennt, z.B. die Bereitstellung einer neuen Version einer benutzerdefinierten Qualifikation, müssen Sie die Qualifikationsgruppe aktualisieren und die jeweilige Qualifikation „abändern“, indem Sie die Qualifikationsdefinition bearbeiten, insbesondere die URL, um die Änderungserkennung zu erzwingen und den Cache für diese Qualifikation für ungültig zu erklären.

## <a name="next-steps"></a>Nächste Schritte

Dieser Artikel behandelt die inkrementelle Indizierung für Indexer, die Skillsets enthalten. Um Ihr Wissen weiter zu vertiefen, lesen Sie Artikel zur erneuten Indizierung im Allgemeinen, die für alle Indizierungsszenarien in Azure Cognitive Search gelten.

+ [Neuerstellen eines Azure Cognitive Search-Index](search-howto-reindex.md). 
+ [Indizieren großer Datasets in der Azure Cognitive Search](search-howto-large-index.md). 
