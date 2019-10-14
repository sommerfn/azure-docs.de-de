---
title: Häufige Fehler und Warnungen – Azure Search
description: Dieser Artikel enthält Informationen und Lösungen für häufige Fehler und Warnungen, die bei der KI-Anreicherung in Azure Search auftreten können.
services: search
manager: heidist
author: amotley
ms.service: search
ms.workload: search
ms.topic: conceptual
ms.date: 09/18/2019
ms.author: abmotley
ms.openlocfilehash: b5a161e570489e6382f2226ab5dc9a1c34dc67df
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2019
ms.locfileid: "72028322"
---
# <a name="common-errors-and-warnings-of-the-ai-enrichment-pipeline-in-azure-search"></a>Häufige Fehler und Warnungen der KI-Anreicherungspipeline in Azure Search

Dieser Artikel enthält Informationen und Lösungen für häufige Fehler und Warnungen, die bei der KI-Anreicherung in Azure Search auftreten können.

## <a name="errors"></a>Errors
Die Indizierung wird angehalten, wenn die Fehleranzahl ['maxFailedItems'](cognitive-search-concept-troubleshooting.md#tip-3-see-what-works-even-if-there-are-some-failures) überschreitet. 

Wenn Sie möchten, dass Indexer diese Fehler ignorieren (und „fehlerhafte Dokumente“ überspringen), sollten Sie `maxFailedItems` und `maxFailedItemsPerBatch` wie [hier](https://docs.microsoft.com/rest/api/searchservice/create-indexer#general-parameters-for-all-indexers) beschrieben aktualisieren.

> [!NOTE]
> Jedes fehlerhafte Dokumente mit dem dazugehörigen Dokumentschlüssel (falls verfügbar) wird im Status der Indexer-Ausführung als Fehler angezeigt. Sie können die [Index-API](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) verwenden, um die Dokumente zu einem späteren Zeitpunkt manuell hochzuladen, wenn Sie festgelegt haben, dass der Indexer Fehler toleriert.

Die folgenden Abschnitte helfen Ihnen, Fehler zu beheben, damit die Indizierung fortgesetzt werden kann.

### <a name="could-not-read-document"></a>Dokument konnte nicht gelesen werden
Der Indexer konnte das Dokument nicht aus der Datenquelle lesen. Möglich sind folgende Ursachen:

| `Reason` | Beispiel | Aktion |
| --- | --- | --- |
| Inkonsistente Feldtypen in verschiedenen Dokumenten | Der Typ des Werts stimmt nicht mit dem Spaltentyp überein. `'{47.6,-122.1}'` konnte nicht in der Autorenspalte gespeichert werden.  Erwarteter Typ: JArray. | Stellen Sie sicher, dass der Typ aller Felds in verschiedenen Dokumenten identisch ist. Wenn das `'startTime'`-Feld des ersten Dokuments z.B. den Datentyp DateTime aufweist und im zweiten Dokument eine Zeichenfolge ist, wird dieser Fehler angezeigt. |
| Fehler aus dem zugrunde liegenden Dienst der Datenquelle | (aus Cosmos DB) `{"Errors":["Request rate is large"]}` | Überprüfen Sie die Speicherinstanz, um sicherzustellen, dass Sie fehlerfrei ist. Möglicherweise müssen Sie die Skalierung/Partitionierung anpassen. |
| Vorübergehende Probleme | Beim Empfangen von Ergebnissen vom Server ist ein Fehler auf Übertragungsebene aufgetreten. (Anbieter: TCP-Anbieter, Fehler: 0 – Eine vorhandene Verbindung wurde erzwungenermaßen vom Remotehost geschlossen | Gelegentlich treten unerwartete Konnektivitätsprobleme auf. Versuchen Sie später erneut, das Dokument über den Indexer laufen zu lassen. |

### <a name="could-not-extract-document-content"></a>Dokumentinhalt konnte nicht extrahiert werden
Der Indexer mit einer Blob-Datenquelle konnte den Inhalt aus dem Dokument (z.B. einer PDF-Datei) nicht extrahieren. Möglich sind folgende Ursachen:

| `Reason` | Beispiel | Aktion |
| --- | --- | --- |
| Blob überschreitet das Größenlimit | Das Dokument ist `'150441598'` Bytes groß und überschreitet daher das Größenlimit von `'134217728'` Bytes für die Dokumentenextrahierung für Ihre aktuelle Dienstebene. | [Blob-Indizierungsfehler](search-howto-indexing-azure-blob-storage.md#dealing-with-errors) |
| Blob weist nicht unterstützten Inhaltstyp auf | Das Dokument weist den nicht unterstützten Inhaltstyp `'image/png'` auf | [Blob-Indizierungsfehler](search-howto-indexing-azure-blob-storage.md#dealing-with-errors) |
| Blob ist verschlüsselt | Das Dokument konnte nicht verarbeitet werden. Es ist möglicherweise verschlüsselt oder kennwortgeschützt. | [Blob-Einstellungen](search-howto-indexing-azure-blob-storage.md#controlling-which-parts-of-the-blob-are-indexed) |
| Vorübergehende Probleme | Fehler beim Verarbeiten des Blobs: Anforderung abgebrochen: Die Anforderung wurde angebrochen. | Gelegentlich treten unerwartete Konnektivitätsprobleme auf. Versuchen Sie später erneut, das Dokument über den Indexer laufen zu lassen. |

### <a name="could-not-parse-document"></a>Dokument konnte nicht analysiert werden
Der Indexer hat das Dokument aus der Datenquelle gelesen, aber es gab ein Problem beim Konvertieren des Dokumentinhalts in das angegebene Feldzuordnungsschema. Möglich sind folgende Ursachen:

| `Reason` | Beispiel | Aktion |
| --- | --- | --- |
| Dokumentschlüssel fehlt | Der Dokumentschlüssel darf nicht fehlen oder leer sein | Stellen Sie sicher, dass alle Dokumente gültige Dokumentschlüssel aufweisen |
| Dokumentschlüssel ist ungültig | Der Dokumentschlüssel darf nicht länger als 1024 Zeichen sein | Ändern Sie den Dokumentschlüssel so, dass er die Überprüfungsanforderungen erfüllt. |
| Feldzuordnung konnte nicht auf ein Feld angewendet werden | Die Zuordnungsfunktion `'functionName'` konnte nicht auf das Feld `'fieldName'` angewendet werden. Das Array darf nicht NULL sein. Parametername: Bytes | Überprüfen Sie die [Feldzuordnungen](search-indexer-field-mappings.md), die im Indexer definiert sind, und vergleichen Sie sie mit den Daten des angegebenen Felds des fehlerhaften Dokuments. Es kann erforderlich sein, die Feldzuordnungen oder die Dokumentdaten zu ändern. |
| Feldwert konnte nicht gelesen werden | Der Wert der Spalte `'fieldName'` bei Index `'fieldIndex'` konnte nicht gelesen werden. Beim Empfangen von Ergebnissen vom Server ist ein Fehler auf Übertragungsebene aufgetreten. (Anbieter: TCP-Anbieter, Fehler: 0 – Eine vorhandene Verbindung wurde erzwungenermaßen vom Remotehost geschlossen.) | Diese Fehler sind in der Regel auf unerwartete Konnektivitätsprobleme beim zugrunde liegenden Dienst der Datenquelle zurückzuführen. Versuchen Sie später erneut, das Dokument über den Indexer laufen zu lassen. |

### <a name="could-not-index-document"></a>Dokument konnte nicht indiziert werden.
Das Dokument wurde gelesen und verarbeitet, aber der Indexer konnte es nicht zum Suchindex hinzufügen. Möglich sind folgende Ursachen:

| `Reason` | Beispiel | Aktion |
| --- | --- | --- |
| Ein Feld enthält einen Begriff, der zu groß ist. | Ein Begriff in Ihrem Dokument ist größer als der [Grenzwert von 32 KB](search-limits-quotas-capacity.md#api-request-limits). | Sie können diese Einschränkung vermeiden, indem Sie sicherstellen, dass das Feld nicht als filterbar, facettierbar oder sortierbar konfiguriert ist.
| Das Dokument ist zu groß für die Indizierung. | Ein Dokument ist größer als die [maximale API-Anforderungsgröße](search-limits-quotas-capacity.md#api-request-limits). | [Indizieren großer Datasets](search-howto-large-index.md)

### <a name="skill-input-languagecode-has-the-following-language-codes-xyz-at-least-one-of-which-is-invalid"></a>Qualifikationseingabe „languageCode“ enthält die Sprachcodes „X, Y, Z“, wovon mindestens einer ungültig ist.
Mindestens einer der an die optionale `languageCode`-Eingabe einer Downstreamqualifikation übergebenen Werte wird nicht unterstützt. Dies kann vorkommen, wenn Sie die Ausgabe von [LanguageDetectionSkill](cognitive-search-skill-language-detection.md) an nachfolgende Qualifikationen übergeben und die Ausgabe aus mehr Sprachen besteht, als in diesen Downstreamqualifikationen unterstützt werden.

Wenn Sie wissen, dass Ihr Dataset nur eine einzige Sprache enthält, sollten Sie die [LanguageDetectionSkill](cognitive-search-skill-language-detection.md) und die `languageCode`-Qualifikationseingabe entfernen und stattdessen den Qualifikationsparameter `defaultLanguageCode` für diese Qualifikation verwenden, vorausgesetzt, die Sprache wird für diese Qualifikation unterstützt.

Wenn Sie wissen, dass Ihr Dataset mehrere Sprachen enthält und Sie daher die Eingaben [LanguageDetectionSkill](cognitive-search-skill-language-detection.md) und `languageCode` benötigen, sollten Sie ggf. eine [ConditionalSkill](cognitive-search-skill-conditional.md)-Qualifikation hinzufügen, um Text in nicht unterstützten Sprachen herauszufiltern, bevor der Text an die Downstreamqualifikation übergeben wird.  Hier ein Beispiel, wie dies für die Qualifikation „EntityRecognitionSkill“ aussehen könnte:

```json
{
    "@odata.type": "#Microsoft.Skills.Util.ConditionalSkill",
    "context": "/document",
    "inputs": [
        { "name": "condition", "source": "= $(/document/language) == 'de' || $(/document/language) == 'en' || $(/document/language) == 'es' || $(/document/language) == 'fr' || $(/document/language) == 'it'" },
        { "name": "whenTrue", "source": "/document/content" },
        { "name": "whenFalse", "source": "= null" }
    ],
    "outputs": [ { "name": "output", "targetName": "supportedByEntityRecognitionSkill" } ]
}
```

Im folgenden finden Sie einige Referenzen für die derzeit unterstützten Sprachen für jede der Qualifikationen, die diese Fehlermeldung verursachen können:
* [Unterstützte Sprachen für die Textanalyse](https://docs.microsoft.com/azure/cognitive-services/text-analytics/text-analytics-supported-languages) (für die Qualifikationen [KeyPhraseExtractionSkill](cognitive-search-skill-keyphrases.md), [EntityRecognitionSkill](cognitive-search-skill-entity-recognition.md) und [SentimentSkill](cognitive-search-skill-sentiment.md))
* [Unterstützte Sprachen für den Übersetzer](https://docs.microsoft.com/azure/cognitive-services/translator/language-support) (für die Qualifikation [Text TranslationSkill](cognitive-search-skill-text-translation.md))
* [Text SplitSkill](cognitive-search-skill-textsplit.md) Unterstützte Sprachen: `da, de, en, es, fi, fr, it, ko, pt`

### <a name="skill-did-not-execute-within-the-time-limit"></a>Die Qualifikation wurde nicht innerhalb des Zeitlimits ausgeführt
Es gibt zwei Fälle, in denen möglicherweise diese Fehlermeldung angezeigt wird, die jeweils unterschiedlich behandelt werden sollten. Befolgen Sie die nachfolgenden Anweisungen, je nachdem, welche Qualifikation diesen Fehler zurückgegeben hat.

#### <a name="built-in-cognitive-service-skills"></a>Integrierte Cognitive Services-Qualifikationen
Viele der integrierten kognitiven Qualifikationen, wie z.B. Spracherkennung, Entitätserkennung oder OCR, werden von einem Cognitive Service-API-Endpunkt unterstützt. Manchmal treten vorübergehende Probleme mit diesen Endpunkten auf, und eine Anforderung führt zu einem Timeout. Bei vorübergehenden Problemen gibt es keine Abhilfe, außer zu warten und es erneut versuchen. Zur Abhilfe können Sie überlegen, Ihren Indexer auf [Gemäß einem Zeitplan ausführen](search-howto-schedule-indexers.md) festzulegen. Die geplante Indizierung setzt an der Stelle an, an der sie unterbrochen wurde. Wenn die vorübergehenden Probleme gelöst sind, sollten die Indizierung und die Verarbeitung der kognitiven Qualifikationen bei der nächsten geplanten Ausführung fortgesetzt werden können.

#### <a name="custom-skills"></a>Benutzerdefinierte Qualifikationen
Wenn ein Timeout bei einer selbst erstellten benutzerdefinierten Qualifikation auftritt, können Sie verschiedene Dinge ausprobieren. Überprüfen Sie zunächst Ihre benutzerdefinierte Qualifikation, und stellen Sie sicher, dass sie nicht in einer unendlichen Schleife hängen geblieben ist und dass Sie ein konsistentes Ergebnis zurückgibt. Nachdem Sie dies überprüft haben, ermitteln Sie die Ausführungszeit ihrer Qualifikation. Wenn Sie in der Definition Ihrer benutzerdefinierten Qualifikation nicht explizit einen `timeout`-Wert festgelegt haben, beträgt der Standardwert für `timeout` 30 Sekunden. Wenn 30 Sekunden nicht lang genug für die Ausführung Ihrer Qualifikation sind, können Sie einen höheren `timeout`-Wert für die Definition Ihrer benutzerdefinierten Qualifikation angeben. Im folgenden finden Sie ein Beispiel für eine Definition einer benutzerdefinierten Qualifikation, bei der das Timeout auf 90 Sekunden festgelegt ist:

```json
  {
        "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
        "uri": "<your custom skill uri>",
        "batchSize": 1,
        "timeout": "PT90S",
        "context": "/document",
        "inputs": [
          {
            "name": "input",
            "source": "/document/content"
          }
        ],
        "outputs": [
          {
            "name": "output",
            "targetName": "output"
          }
        ]
      }
```

Der maximale Wert, den Sie für den `timeout`-Parameter festlegen können, beträgt 230 Sekunden.  Wenn Ihre benutzerdefinierte Qualifikation nicht innerhalb von 230 Sekunden konsistent ausgeführt werden kann, sollten Sie ggf. die `batchSize` Ihrer benutzerdefinierten Qualifikation verringern, sodass weniger Dokumente innerhalb einer einzelnen Ausführung verarbeitet werden.  Wenn Sie die `batchSize` bereits auf 1 festgelegt haben, müssen Sie die Qualifikation neu schreiben, damit Sie in unter 230 Sekunden ausgeführt werden kann, oder Sie müssen sie anderweitig in mehrere benutzerdefinierte Qualifikationen aufteilen, damit die Ausführungszeit für jede einzelne benutzerdefinierte Qualifikation maximal 230 Sekunden beträgt. Weitere Informationen finden Sie in der [Dokumentation zu benutzerdefinierten Qualifikationen](cognitive-search-custom-skill-web-api.md).

### <a name="could-not-mergeorupload--delete-document-to-the-search-index"></a>„`MergeOrUpload`“ | „`Delete`“ des Dokuments in den Suchindex nicht möglich

Das Dokument wurde gelesen und verarbeitet, aber der Indexer konnte es nicht zum Suchindex hinzufügen. Möglich sind folgende Ursachen:

| `Reason` | Beispiel | Aktion |
| --- | --- | --- |
| Ein Begriff in Ihrem Dokument ist größer als der [Grenzwert von 32 KB](search-limits-quotas-capacity.md#api-request-limits). | Ein Feld enthält einen Begriff, der zu groß ist. | Sie können diese Einschränkung vermeiden, indem Sie sicherstellen, dass das Feld nicht als filterbar, facettierbar oder sortierbar konfiguriert ist.
| Ein Dokument ist größer als die [maximale API-Anforderungsgröße](search-limits-quotas-capacity.md#api-request-limits). | Das Dokument ist zu groß für die Indizierung. | [Indizieren großer Datasets](search-howto-large-index.md)
| Probleme beim Herstellen einer Verbindung mit dem Zielindex (die nach Wiederholungsversuchen weiterhin bestehen), da der Dienst durch eine andere Aufgabe ausgelastet ist, z.B. eine Abfrage oder Indizierung. | Die Verbindung zum Hochladen des Indexes konnte nicht hergestellt werden. Der Suchdienst ist stark ausgelastet. | [Zentrales Hochskalieren des Suchdiensts](search-capacity-planning.md)
| Der Suchdienst wird für das Dienstupdate gepatcht oder befindet sich mitten in einer Neukonfiguration der Topologie. | Die Verbindung zum Hochladen des Indexes konnte nicht hergestellt werden. Der Suchdienst ist derzeit nicht verfügbar / Der Suchdienst befindet sich im Übergang. | Konfigurieren Sie den Dienst mit mindestens 3 Replikaten für eine Verfügbarkeit von 99,9 % gemäß [SLA-Dokumentation](https://azure.microsoft.com/support/legal/sla/search/v1_0/).
| Fehler in der zugrunde liegenden Compute-/Netzwerkressource (selten) | Die Verbindung zum Hochladen des Indexes konnte nicht hergestellt werden. Ein unbekannter Fehler ist aufgetreten. | Konfigurieren Sie Indexer für die [Ausführung nach Zeitplan](search-howto-schedule-indexers.md), um den Betrieb nach einem Fehler wieder aufzunehmen.

##  <a name="warnings"></a>Warnungen
Die Indizierung wird von Warnungen nicht beendet, aber Warnungen weisen auf Bedingungen hin, die zu unerwarteten Ergebnissen führen können. Ob Sie Maßnahmen ergreifen oder nicht, hängt von den Daten und Ihrem Szenario ab.

### <a name="skill-input-was-truncated"></a>Die Qualifikationseingabe wurde abgeschnitten
Für kognitive Qualifikationen gelten Beschränkungen hinsichtlich der Länge von Text, der gleichzeitig analysiert werden kann. Wenn die Texteingabe dieser Qualifikationen diesen Grenzwert überschreitet, wird der Text abgeschnitten, um den Grenzwert einzuhalten, und die Anreicherung wird anschließend für diesen abgeschnittene Text ausgeführt. Dies bedeutet, dass die Qualifikation ausgeführt wird, aber nicht für all Ihre Daten.

Im unten gezeigten LanguageDetectionSkill-Beispiel kann das `'text'`-Eingabefeld diese Warnung auslösen, wenn das Zeichenlimit überschritten wird. Die Grenzwerte für Qualifikationseingaben finden Sie in der [Dokumentation zu Qualifikationen](cognitive-search-predefined-skills.md).

```json
 {
    "@odata.type": "#Microsoft.Skills.Text.LanguageDetectionSkill",
    "inputs": [
      {
        "name": "text",
        "source": "/document/text"
      }
    ],
    "outputs": [...]
  }
```

Wenn Sie sicherstellen möchten, dass der gesamte Text analysiert wird, sollten Sie die [Qualifikation „Aufteilung“](cognitive-search-skill-textsplit.md) verwenden.