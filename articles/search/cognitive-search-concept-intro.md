---
title: Einführung in die KI-Anreicherung
titleSuffix: Azure Cognitive Search
description: Inhaltsextrahierung, Verarbeitung natürlicher Sprache (NLP, Natural Language Processing) und Bildverarbeitung zum Erstellen durchsuchbarer Inhalte in der Azure Cognitive Search-Indizierung mithilfe von kognitiven Fähigkeiten und KI-Algorithmen.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: overview
ms.date: 11/04/2019
ms.openlocfilehash: 5dc33de19ef71a0714052a6457bef9f32fc159c3
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73720162"
---
# <a name="introduction-to-ai-in-azure-cognitive-search"></a>Einführung in die KI in Azure Cognitive Search

Die KI-Anreicherung ist eine Funktion der Azure Cognitive Search-Indizierung zum Extrahieren von Text aus Bildern, Blobs und anderen unstrukturierten Datenquellen, um den Inhalt anzureichern und in einem Index oder Wissensspeicher besser durchsuchbar zu machen. Extrahierung und Erweiterung werden über *kognitive Fähigkeiten* implementiert, die einer Indizierungspipeline angefügt sind. In den Dienst integrierte kognitive Qualifikationen werden wie folgt kategorisiert: 

+ Zu den Fähigkeiten in Bezug auf die **Verarbeitung natürlicher Sprache** gehören [Entitätserkennung](cognitive-search-skill-entity-recognition.md), Spracherkennung, [Schlüsselbegriffserkennung](cognitive-search-skill-language-detection.md), [Textbearbeitung](cognitive-search-skill-keyphrases.md) und [Stimmungserkennung](cognitive-search-skill-sentiment.md). Mithilfe dieser Fähigkeiten kann unstrukturierter Text neue Formen annehmen, die als durchsuchbare und filterbare Felder in einem Index zugeordnet sind.

+ Die **Bildverarbeitungsfähigkeiten** umfassen [Optical Character Recognition (OCR)](cognitive-search-skill-ocr.md) und die Identifizierung von [visuellen Features](cognitive-search-skill-image-analysis.md), z.B. Gesichtserkennung, Bildinterpretation, Bilderkennung (berühmte Personen und Wahrzeichen) oder Attribute wie Farben oder Bildausrichtung. Sie können Textdarstellungen von Bildinhalt erstellen, indem Sie alle Abfragefunktionen von Azure Cognitive Search verwenden.

![Diagramm der KI-Anreicherungspipeline](./media/cognitive-search-intro/cogsearch-architecture.png "Übersicht über die KI-Anreicherungspipeline")

Die kognitiven Fähigkeiten in Azure Cognitive Search basieren auf vortrainierten Machine Learning-Modellen in Cognitive Services-APIs: [Maschinelles Sehen](https://docs.microsoft.com/azure/cognitive-services/computer-vision/) und [Textanalyse](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview). 

Die Verarbeitung von natürlicher Sprache und Bildern wird während der Phase der Datenerfassung angewendet, wobei die Ergebnisse zu einem Teil einer Dokumentkomposition in einem durchsuchbaren Index in Azure Cognitive Search werden. Daten stammen aus einem Azure-Dataset und werden dann über eine Indizierungspipeline übertragen, indem jeweils die erforderlichen [integrierten Fähigkeiten](cognitive-search-predefined-skills.md) verwendet werden. Die Architektur ist erweiterbar. Falls die integrierten Fähigkeiten also nicht ausreichen, können Sie [benutzerdefinierte Fähigkeiten](cognitive-search-create-custom-skill-example.md) erstellen und anfügen, um die benutzerdefinierte Verarbeitung zu integrieren. Beispiele hierfür sind ein benutzerdefiniertes Entitätsmodul oder ein benutzerdefinierter Dokumentenklassifizierer, das bzw. der auf eine bestimmte Domäne ausgerichtet ist, z.B. Finanzen, wissenschaftliche Veröffentlichungen oder Medizin.

> [!NOTE]
> Wenn Sie den Umfang erweitern, indem Sie die Verarbeitungsfrequenz erhöhen oder weitere Dokumente oder KI-Algorithmen hinzufügen, müssen Sie [eine kostenpflichtige Cognitive Services-Ressource anfügen](cognitive-search-attach-cognitive-services.md). Gebühren fallen beim Aufrufen von APIs in Cognitive Services sowie für die Bildextraktion im Rahmen der Dokumententschlüsselungsphase in Azure Cognitive Search an. Für die Textextraktion aus Dokumenten fallen keine Gebühren an.
>
> Die Ausführung integrierter Qualifikationen wird nach dem bestehenden [nutzungsbasierten Preis für Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/) berechnet. Die Preise für die Bildextraktion sind in der [Preisübersicht für Azure Cognitive Search](https://go.microsoft.com/fwlink/?linkid=2042400) angegeben.

## <a name="when-to-use-cognitive-skills"></a>Einsatzgebiete der kognitiven Fähigkeiten

Integrierte kognitive Qualifikationen können verwendet werden, wenn es sich bei Ihren Rohinhalten um unstrukturierten Text, um Bildinhalte oder um Inhalte handelt, für die Spracherkennung und -übersetzung benötigt werden. Die Anwendung von KI in Form der integrierten kognitiven Qualifikationen kann den Nutzen dieser Inhalte in Ihren Such- und Data Science-Apps erhöhen. 

Darüber hinaus können Sie auch eine benutzerdefinierte Qualifikation hinzufügen, wenn Sie über Open-Source-, Drittanbieter- oder Erstanbietercode verfügen, den Sie in die Pipeline integrieren möchten. Zu dieser Kategorie zählen Klassifizierungsmodelle, mit denen wichtige Merkmale verschiedener Dokumenttypen identifiziert werden. Es können aber auch andere Pakete verwendet werden, die den Nutzen Ihrer Inhalte erhöhen.

### <a name="more-about-built-in-skills"></a>Weitere Informationen zu integrierten Qualifikationen

Ein Skillset mit vorkonfigurierten Fähigkeiten eignet sich sehr gut für die folgenden Anwendungsszenarien:

+ Gescannte Dokumente (JPEG), die für die Volltextsuche verfügbar gemacht werden sollen. Sie können eine Fähigkeit zur optischen Zeichenerkennung (Optical Character Recognition, OCR) anfügen, um Text aus JPEG-Dateien zu identifizieren, zu extrahieren und zu erfassen.

+ PDF-Dateien mit Kombinationen aus Bild und Text. Texte in PDF-Dateien können während der Indizierung extrahiert werden, ohne dass die Schritte zur Anreicherung ausgeführt werden. Beim Hinzufügen von Bildverarbeitung und natürlicher Sprachverarbeitung erzielen Sie jedoch häufig ein besseres Ergebnis als bei einer Standardindizierung.

+ Mehrsprachiger Inhalt, für den Sie die Spracherkennung und möglicherweise die Textübersetzung anwenden möchten.

+ Unstrukturierte oder teilweise strukturierte Dokumente mit Inhalten, die eine inhärente Bedeutung oder einen Kontext haben, der im größeren Dokument ausgeblendet ist. 

  Insbesondere Blobs enthalten häufig einen großen Textteil in einem einzelnen „Feld“. Durch das Anfügen von Fähigkeiten zur Bildverarbeitung und natürlicher Sprachverarbeitung an einen Indexer können Sie neue Informationen erstellen, die in den Rohdaten noch vorhanden sind, aber sonst nicht als unterschiedliche Felder aufgeführt werden. Einige einsatzbereite, integrierte kognitive Fähigkeiten, die hilfreich sein können: Schlüsselbegriffserkennung, Stimmungsanalyse und Entitätserkennung (Personen, Organisationen und Standorte).

  Darüber hinaus können Sie mit integrierten Fähigkeiten Inhalte durch Textaufteilung, Textzusammenführung und Shape-Vorgänge neu strukturieren.

### <a name="more-about-custom-skills"></a>Weitere Informationen zu benutzerdefinierten Qualifikationen

Benutzerdefinierte Fähigkeiten können komplexere Szenarien unterstützen, z. B. das Erkennen von Formularen oder die benutzerdefinierte Entitätserkennung mithilfe eines Modells, das Sie bereitstellen und in der [benutzerdefinierten Skills-Webschnittstelle](cognitive-search-custom-skill-interface.md) umschließen. Beispiele für benutzerdefinierte Fähigkeiten sind die [Formularerkennung](/azure/cognitive-services/form-recognizer/overview), die Integration der [Bing-Entitätssuche-API](https://docs.microsoft.com/azure/search/cognitive-search-create-custom-skill-example) und [die Erkennung von benutzerdefinierten Entitäten](https://github.com/Microsoft/SkillsExtractorCognitiveSearch).


## <a name="components-of-an-enrichment-pipeline"></a>Komponenten einer Anreicherungspipeline

Eine Anreicherungspipeline basiert auf [*Indexern*](search-indexer-overview.md), die Datenquellen durchforsten und End-to-End-Indexverarbeitung bieten. Die Fähigkeitengruppen werden jetzt an Indexer angefügt. Dabei werden Dokumente gemäß der von Ihnen definierten Fähigkeiten abgefangen und angereichert. Nach der Indizierung können Sie über Suchanforderungen über sämtliche [von Azure Cognitive Search unterstützte Abfragetypen](search-query-overview.md) auf Inhalte zugreifen.  Wenn Sie mit Indexern noch nicht vertraut sind, werden Ihnen in diesem Abschnitt die erforderlichen Schritte erläutert.

### <a name="step-1-connection-and-document-cracking-phase"></a>Schritt 1: Phase der Entschlüsselung von Verbindung und Dokument

Am Anfang der Pipeline befindet sich unstrukturierter Text oder Nicht-Text-Inhalt (z.B. Bilder und gescannte Dokumente als JPEG-Dateien). Die Daten müssen in einem Azure-Datenspeicherdienst enthalten sein, auf den ein Indexer Zugriff hat. Indexer können Quelldokumente entschlüsseln, um Text aus Quelldaten zu extrahieren.

![Phase der Dokumententschlüsselung](./media/cognitive-search-intro/document-cracking-phase-blowup.png "Dokumententschlüsselung")

 Zu den unterstützten Quellen zählen Azure Blob Storage, Azure Table Storage, Azure SQL-Datenbank und Azure Cosmos DB. Textbasierte Inhalte können aus den folgenden Dateitypen extrahiert werden: PDF-, Word-, PowerPoint- und CSV-Dateien. Eine vollständige Liste finden Sie unter [Unterstützte Formate](search-howto-indexing-azure-blob-storage.md#supported-document-formats).

### <a name="step-2-cognitive-skills-and-enrichment-phase"></a>Schritt 2: Phase der kognitiven Fähigkeiten und Anreicherung

Die Anreicherung erfolgt durch *kognitive Fähigkeiten*, die atomische Vorgänge durchführen. Sobald Sie über Textinhalte aus einer PDF-Datei verfügen, können Sie beispielsweise die Entitätserkennung, die Sprachenerkennung oder die Schlüsselbegriffserkennung anwenden, um neue Felder in Ihrem Index zu erstellen, die nativ in der Quelle nicht verfügbar sind. Die Auflistung der in Ihrer Pipeline verwendeten Fähigkeiten wird als *Fähigkeitengruppe* bezeichnet.  

![Phase der Anreicherung](./media/cognitive-search-intro/enrichment-phase-blowup.png "Phase der Anreicherung")

Ein Skillset basiert auf [integrierten kognitiven Fähigkeiten](cognitive-search-predefined-skills.md) oder [benutzerdefinierten Fähigkeiten](cognitive-search-create-custom-skill-example.md), die von Ihnen bereitgestellt und mit dem Skillset verbunden werden. Eine Fähigkeitengruppe kann minimal oder höchst komplex sein. Sie bestimmt nicht nur den Verarbeitungstyp, sondern auch die Reihenfolge der Vorgänge. Eine Fähigkeitengruppe bietet zusammen mit den als Bestandteil eines Indexers definierten Feldzuordnungen eine umfassende Beschreibung der Anreicherungspipeline. Weitere Informationen zum Zusammensetzen all dieser Teile finden Sie unter [Definieren einer Fähigkeitengruppe](cognitive-search-defining-skillset.md).

Die Pipeline generiert intern eine Sammlung angereicherter Dokumente. Sie können entscheiden, welche Teile der angereicherten Dokumente indizierbaren Feldern in Ihrem Suchindex zugeordnet werden sollen. Wenn Sie beispielsweise die Fähigkeiten der Schlüsselbegriffserkennung und der Entitätserkennung angewendet haben, werden diese neuen Felder Bestandteil des angereicherten Dokuments und können Feldern in Ihrem Index zugeordnet werden. Weitere Informationen zu Eingabe-/Ausgabeformationen finden Sie unter [Anmerkungen](cognitive-search-concept-annotations-syntax.md).

#### <a name="add-a-knowledgestore-element-to-save-enrichments"></a>Hinzufügen eines knowledgeStore-Elements zum Speichern von Anreicherungen

Die [REST-API-Version 2019-05-06-Preview des Search-Diensts](search-api-preview.md) erweitert Qualifikationsgruppen um eine `knowledgeStore`-Definition, die eine Azure Storage-Verbindung und Projektionen bereitstellt, die beschreiben, wie die Anreicherungen gespeichert werden. 

Das Hinzufügen eines Wissensspeichers zu einer Qualifikationsgruppe bietet Ihnen die Möglichkeit, eine Darstellung Ihrer Anreicherungen für andere Szenarien als die Volltextsuche zu projizieren. Weitere Informationen finden Sie unter [Wissensspeicher (Vorschau)](knowledge-store-concept-intro.md).

### <a name="step-3-search-index-and-query-based-access"></a>Schritt 3: Suchindex und abfragebasierter Zugriff

Nach Abschluss der Verarbeitung verfügen Sie über einen Suchindex mit angereicherten Dokumenten, die mit Azure Cognitive Search im Volltext durchsucht werden können. Entwickler und Benutzer greifen über das [Abfragen des Indexes](search-query-overview.md) auf den von der Pipeline generierten angereicherten Inhalt zu. 

![Index mit Suchsymbol](./media/cognitive-search-intro/search-phase-blowup.png "Index mit Suchsymbol")

Der Index ist vergleichbar mit beliebigen anderen Indizes, die für Azure Cognitive Search erstellt werden können: Sie können ihn durch benutzerdefinierte Analysen ergänzen, Fuzzysuchabfragen aufrufen, gefilterte Suchvorgänge hinzufügen oder zur Umgestaltung der Suchergebnisse mit Bewertungsprofilen experimentieren.

Indizes werden über ein Indexschema generiert, das die Felder, Attribute und weitere Konstrukte definiert, die an einen bestimmten Index angefügt wurden, wie z.B. Bewertungsprofile und Synonymzuordnungen. Nachdem ein Index definiert und aufgefüllt wurde, können Sie die Indizierung inkrementell durchführen, um neue und aktualisierte Quelldokumente zu übernehmen. Für bestimmte Änderungen ist eine vollständige Neuerstellung erforderlich. Sie sollten ein kleines Dataset verwenden, bis der Schemaentwurf stabil ist. Weitere Informationen finden Sie unter [Neuerstellen eines Indexes](search-howto-reindex.md).

<a name="feature-concepts"></a>

## <a name="key-features-and-concepts"></a>Wichtige Features und Konzepte

| Konzept | BESCHREIBUNG| Links |
|---------|------------|-------|
| Fähigkeitengruppe | Eine benannte allgemeine Ressource mit einer Sammlung von Fähigkeiten. Eine Fähigkeitengruppe in der Anreicherungspipeline. Sie wird während der Indizierung durch einen Indexer aufgerufen. | Siehe [Definieren eines Skillsets](cognitive-search-defining-skillset.md) |
| Kognitive Fähigkeit | Eine atomische Transformation in einer Anreicherungspipeline. Häufig handelt es sich dabei um eine Komponente, die sich mit der Extrahierung oder der Erschließung einer Struktur befasst und folglich Ihr Verständnis der Eingabedaten ergänzt. Die Ausgabe ist fast immer textbasiert. Bei der Verarbeitung wird auf die Verarbeitung natürlicher Sprache oder die Bildbearbeitung zurückgegriffen, bei der Text aus Bildeingaben extrahiert oder generiert wird. Die Ausgabe aus einer Fähigkeit kann einem Feld in einem Index zugeordnet werden oder als Eingabe für eine Downstream-Anreicherung verwendet werden. Eine Fähigkeit ist entweder vordefiniert und wird von Microsoft bereitgestellt, oder sie ist benutzerdefiniert und wird von Ihnen erstellt und bereitgestellt. | [Integrierte kognitive Qualifikationen](cognitive-search-predefined-skills.md) |
| Extrahieren von Daten | Deckt ein breites Verarbeitungsspektrum ab. In Bezug auf die KI-Anreicherung wird die Fähigkeit zur Erkennung von Entitäten jedoch zumeist zum Extrahieren von Daten (einer Entität) aus einer Quelle verwendet, die diese Informationen nicht nativ bereitstellt. | Siehe [Die kognitive Qualifikation „Entitätserkennung“](cognitive-search-skill-entity-recognition.md) und [Kognitive Qualifikation „Dokumentextrahierung“](cognitive-search-skill-document-extraction.md)| 
| Bildverarbeitung | Erschließt sich Text aus einem Bild, z.B. die Fähigkeit der Erkennung eines Wahrzeichens oder des Extrahierens von Text aus einem Bild. Typische Beispiele dafür sind OCR für das Löschen von Zeichen aus einer Datei mit einem gescannten Dokument (JPEG) oder das Erkennen eines Straßennamens auf einem Foto mit einem Straßenschild. | Siehe [Fähigkeit: Bildanalyse](cognitive-search-skill-image-analysis.md) oder [Fähigkeit: OCR](cognitive-search-skill-ocr.md)
| Verarbeitung natürlicher Sprache | Textverarbeitung für Einblicke und Informationen zu Texteingaben. Sprachenerkennung, Standpunktanalyse und Schlüsselbegriffserkennung sind Fähigkeiten, die in den Bereich der Verarbeitung natürlicher Sprache fallen.  | Siehe [Qualifikation „Schlüsselbegriffserkennung“](cognitive-search-skill-keyphrases.md), [Qualifikation „Sprachenerkennung“](cognitive-search-skill-language-detection.md), [Qualifikation „Textübersetzung“ der kognitiven Suche](cognitive-search-skill-text-translation.md), [Qualifikation „Standpunktanalyse“](cognitive-search-skill-sentiment.md) |
| Dokumententschlüsselung | Der Prozess des Extrahierens oder Erstellens von Textinhalt aus Nicht-Text-Quellen während der Indizierung. OCR (Optical Character Recognition) ist ein Beispiel hierfür. In der Regel bezieht sich dies jedoch auf die Kernfunktionalität des Indexers, da dieser Inhalte aus Anwendungsdateien extrahiert. Sowohl die Datenquelle, die den Speicherort der Quelldatei bereitstellt, als auch die Indexerdefinition, die Feldzuordnungen zur Verfügung stellt, stellen Schlüsselfaktoren bei der Dokumententschlüsselung dar. | Siehe [Indexer in Azure Search](search-indexer-overview.md) |
| Strukturierung | Textfragmente werden zu einer größeren Struktur konsolidiert, oder größere Textblöcke werden zur weiteren Downstream-Verarbeitung in eine überschaubare Größe aufgeteilt. | Siehe [Fähigkeit: Strukturierung](cognitive-search-skill-shaper.md), [Fähigkeit: Textzusammenführung](cognitive-search-skill-textmerger.md), [Fähigkeit: Textunterteilung](cognitive-search-skill-textsplit.md) |
| Angereicherte Dokumente | Eine vorübergehende interne Struktur, die bei der Verarbeitung generiert wird. Die endgültige Ausgabe spiegelt sich in einem Suchindex wider. Eine Skillset bestimmt, welche Anreicherungen vorgenommen werden. Feldzuordnungen bestimmen, welche Datenelemente zum Index hinzugefügt werden. Optional können Sie einen Wissensspeicher erstellen, um angereicherte Dokumente mit Tools wie dem Storage-Explorer, Power BI oder einem anderen Tool, das mit Azure Blob Storage verbunden ist, zu speichern und zu untersuchen. | Siehe [Wissensspeicher (Vorschau)](knowledge-store-concept-intro.md) |
| Indexer |  Ein Crawler, mit dem durchsuchbare Daten und Metadaten aus einer externen Datenquelle extrahiert werden und mit dem ein Index basierend auf Feld-zu-Feld-Zuordnungen zwischen dem Index und Ihrer Datenquelle zur Dokumententschlüsselung aufgefüllt wird. Bei der KI-Anreicherung ruft der Indexer ein Skillset auf und enthält die Feldzuordnungen, durch welche die Ausgabe der Anreicherung Zielfeldern im Index zugeordnet wird. Die Indexerdefinition enthält sämtliche Anweisungen und Verweise für Pipelinevorgänge. Die Pipeline wird bei Ausführung des Indexers aufgerufen. Darüber hinaus können Sie bereits vorhandene Verarbeitungsschritte wiederverwenden und nur die geänderten Schritte und Qualifikationen ausführen. Hierzu sind allerdings weitere Konfigurationsschritte erforderlich. | Siehe [Indexer in Azure Search](search-indexer-overview.md) und [Was ist die inkrementelle Indizierung in Azure Cognitive Search?](cognitive-search-incremental-indexing-conceptual.md) |
| Data source  | Ein von einem Indexer verwendetes Objekt zum Verbinden mit einer externen Datenquelle von unterstützten Typen in Azure. | Siehe [Indexer in Azure Search](search-indexer-overview.md) |
| Index | Ein persistenter Suchindex in Azure Cognitive Search, der aus einem Indexschema erstellt wurde, das die Feldstruktur und -verwendung definiert. | Siehe [Erstellen eines grundlegenden Index](search-what-is-an-index.md) | 

<a name="where-do-i-start"></a>

## <a name="where-do-i-start"></a>Wo beginne ich?

**Schritt 1: [Erstellen einer Ressource für Azure Cognitive Search](search-create-service-portal.md)** 

**Schritt 2: Ausprobieren einiger Schnellstarts und Beispiele, um praktische Erfahrungen zu sammeln**

+ [Schnellstart (Portal)](cognitive-search-quickstart-blob.md)
+ [Tutorial (HTTP-Anforderungen)](cognitive-search-tutorial-blob.md)
+ [Beispiel: Erstellen einer benutzerdefinierten Fähigkeit für die KI-Anreicherung (C#)](cognitive-search-create-custom-skill-example.md)

Wir empfehlen, den kostenlosen Dienst zu Lernzwecken zu verwenden. Jedoch ist die Anzahl kostenloser Transaktionen auf 20 Dokumente pro Tag beschränkt. Wenn Sie sowohl die Schnellstartanleitung als auch das Tutorial am gleichen Tag ausführen möchten, verwenden Sie einen kleineren Dateisatz (zehn Dokumente), damit Sie beide Übungen ausführen können, oder löschen Sie den im Schnellstart oder Tutorial verwendeten Indexer, um den Zähler auf Null zu setzen.

**Schritt 3: Überprüfen der API**

Sie können in REST `api-version=2019-05-06` in Anforderungen oder dem .NET SDK verwenden. Wenn Sie den Wissensspeicher ausprobieren, verwenden Sie stattdessen die Vorschau-REST-API (`api-version=2019-05-06-Preview`).

Bei diesem Schritt werden die REST-APIs zum Erstellen einer Lösung für die KI-Anreicherung verwendet. Es werden nur zwei APIs für die KI-Anreicherung hinzugefügt oder erweitert. Andere APIs haben die gleiche Syntax wie die allgemein verfügbaren Versionen.

| REST-API | BESCHREIBUNG |
|-----|-------------|
| [Erstellen der Datenquelle](https://docs.microsoft.com/rest/api/searchservice/create-data-source)  | Eine Ressource, die eine externe Datenquelle identifiziert, welche Quelldaten zum Erstellen angereicherter Dokumente bereitstellt.  |
| [Erstellen einer Qualifikationsgruppe (API-Version 2019-05-06)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)  | Diese API ist spezifisch für die KI-Anreicherung. Es handelt sich hierbei um eine Ressource, die die Verwendung von [integrierten Qualifikationen](cognitive-search-predefined-skills.md) und [benutzerdefinierten kognitiven Qualifikationen](cognitive-search-custom-skill-interface.md) in einer Anreicherungspipeline während der Indizierung koordiniert. |
| [Index erstellen](https://docs.microsoft.com/rest/api/searchservice/create-index)  | Ein Schema zur Beschreibung eines Azure Cognitive Search-Index. Felder im Index werden Feldern in den Quelldaten zugeordnet oder Feldern, die während der Anreicherungsphase erstellt wurden (z.B. ein von der Entitätserkennung erstelltes Feld für Organisationsnamen). |
| [Erstellen eines Indexers (API-Version 2019-05-06)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)  | Eine Ressource, die während der Indizierung verwendete Komponenten definiert: Dazu zählen eine Datenquelle, eine Fähigkeitengruppe, Feldzuordnungen der Quelle, intermediäre Datenstrukturen für den Zielindex und der Index selbst. Die Ausführung des Indexers ist Auslöser für die Datenerfassung und -anreicherung. Die Ausgabe ist ein Suchindex basierend auf dem Indexschema, der mit Quelldaten aufgefüllt wird, die durch Qualifikationsgruppen angereichert werden. Diese vorhandene API wird in kognitiven Suchszenarien mit Einbindung einer Skillset-Eigenschaft erweitert. |

**Checkliste: Typischer Workflow**

1. Fügen Sie Teilmengen Ihrer Azure-Quelldaten in eine repräsentative Stichprobe ein. Die Indizierung braucht Zeit. Daher sollten Sie mit einem kleinen, repräsentativen Dataset beginnen, das Sie anschließend schrittweise aufbauen, während sich Ihre Lösung entwickelt.

1. Erstellen Sie ein [Datenquellenobjekt](https://docs.microsoft.com/rest/api/searchservice/create-data-source) in Azure Cognitive Search, um eine Verbindungszeichenfolge für den Datenabruf bereitzustellen.

1. Erstellen Sie mithilfe von Anreicherungsschritten [Fähigkeitengruppen](https://docs.microsoft.com/rest/api/searchservice/create-skillset).

1. Definieren Sie das [Indexschema](https://docs.microsoft.com/rest/api/searchservice/create-index). Die *Fields*-Auflistung enthält Felder aus Quelldaten. Sie sollten zudem zusätzliche Felder durch Stubs ersetzen, um generierte Werte für Inhalte zu speichern, die während der Anreicherung erstellt werden.

1. Definieren Sie den [Indexer](https://docs.microsoft.com/rest/api/searchservice/create-skillset), der auf die Datenquelle, die Fähigkeitengruppen und den Index verweist.

1. Fügen Sie *outputFieldMappings* innerhalb des Indexers hinzu. In diesem Abschnitt wird die Ausgabe aus der Fähigkeitengruppe (in Schritt 3) den Eingabefeldern im Indexschema (in Schritt 4) zugeordnet.

1. Senden Sie die gerade erstellte Anforderung *Indexer erstellen* (eine POST-Anforderung mit einer Indexerdefinition im Anforderungstext), um den Indexer in Azure Cognitive Search darzustellen. Dieser Schritt befasst sich mit der Ausführung des Indexers durch Aufrufen der Pipeline.

1. Führen Sie zum Auswerten von Ergebnissen und zum Ändern von Code Abfragen aus, um Fähigkeitengruppen, Schemas oder die Indexerkonfiguration zu aktualisieren.

1. [Setzen Sie den Indexer zurück](search-howto-reindex.md), bevor Sie die Pipeline neu erstellen.

Weitere Informationen zu bestimmten Fragen oder Problemen finden Sie unter [Problembehandlungstipps](cognitive-search-concept-troubleshooting.md).

## <a name="next-steps"></a>Nächste Schritte

+ [Dokumentation zur KI-Anreicherung](cognitive-search-resources-documentation.md)
+ [Schnellstart: Testen der KI-Anreicherung in einer exemplarischen Vorgehensweise im Portal](cognitive-search-quickstart-blob.md)
+ [Tutorial: Weitere Informationen zu den APIs für die KI-Anreicherung](cognitive-search-tutorial-blob.md)
+ [Wissensspeicher (Vorschau)](knowledge-store-concept-intro.md)
+ [Wissensspeicher: exemplarische Vorgehensweise](knowledge-store-howto.md)
