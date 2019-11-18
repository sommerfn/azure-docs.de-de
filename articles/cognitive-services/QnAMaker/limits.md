---
title: 'Einschränkungen und Begrenzungen: QnA Maker'
titleSuffix: Azure Cognitive Services
description: QnA Maker weist Metagrenzwerte für Teile der Wissensdatenbank und des Diensts auf. Es ist wichtig, Ihre Wissensdatenbank innerhalb dieser Grenzwerte zu halten, um sie testen und veröffentlichen zu können.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 08/30/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 4effd14029eaaee1e1c22cdb814096820e19e089
ms.sourcegitcommit: 018e3b40e212915ed7a77258ac2a8e3a660aaef8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73794030"
---
# <a name="qna-maker-knowledge-base-limits-and-boundaries"></a>Grenzwerte und Grenzen für QnA Maker-Wissensdatenbanken

Bei den folgenden QnA Maker-Grenzwerten handelt es sich um eine Kombination aus den [Grenzwerten für den Azure Cognitive Search-Dienst](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity) und den [Grenzwerten für den QnA Maker-Tarif](https://azure.microsoft.com/pricing/details/cognitive-services/qna-maker/). Sie müssen mit beiden Grenzwerten vertraut sein, um zu verstehen, wie viele Wissensdatenbanken Sie pro Ressource erstellen können und wie groß die Wissensdatenbanken jeweils werden können.

## <a name="knowledge-bases"></a>Wissensdatenbanken

Die maximale Anzahl von Wissensdatenbanken basiert auf den [Grenzwerten für den Azure Cognitive Search-Dienst](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity).

|**Azure Cognitive Search-Tarif** | **Free** | **Basic** |**S1** | **S2**| **S3** |**S3 HD**|
|---|---|---|---|---|---|----|
|Maximal zulässige Anzahl der veröffentlichten Wissensdatenbanken|2|14|49|199|199|2\.999|

 Wenn Ihr Tarif beispielsweise 15 zulässige Indizes aufweist, können Sie 14 Wissensdatenbanken veröffentlichen (1 Index pro veröffentlichter Wissensdatenbank). Der fünfzehnte Index (`testkb`) wird für alle Wissensdatenbanken zum Erstellen und Testen verwendet. 

## <a name="extraction-limits"></a>Grenzwerte für die Extraktion

### <a name="maximum-number-of-files"></a>Maximale Anzahl von Dateien

Die maximale Anzahl von Dateien, die extrahiert werden können, und die maximale Dateigröße basieren auf den **[Grenzwerten für Ihren QnA Maker-Tarif](https://azure.microsoft.com/pricing/details/cognitive-services/qna-maker/)** .

### <a name="maximum-number-of-deep-links-from-url"></a>Maximale Anzahl von Deep-Links für die URL

Die maximale Anzahl von Deep-Links, die zum Extrahieren von Fragen und Antworten (QnA) aus einer URL-Seite durchforstet werden können, beträgt **20**.

## <a name="metadata-limits"></a>Grenzwerte für Metadaten

### <a name="by-azure-cognitive-search-pricing-tier"></a>Nach Azure Cognitive Search-Tarif

Die maximale Anzahl von Metadatenfeldern pro Wissensdatenbank basiert auf den **[Grenzwerten für den Azure Cognitive Search-Dienst](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity)** .

|**Azure Cognitive Search-Tarif** | **Free** | **Basic** |**S1** | **S2**| **S3** |**S3 HD**|
|---|---|---|---|---|---|----|
|Maximale Anzahl der Metadatenfelder pro QnA Maker-Dienst (für alle Knowledge Bases)|1\.000|100*|1\.000|1\.000|1\.000|1\.000|

### <a name="by-name-and-value"></a>Nach Name und Wert

Die Länge und die zulässigen Zeichen für den Metadatennamen und -wert sind in der folgenden Tabelle aufgeführt:

|Item|Zulässige Zeichen|RegEx-Musterabgleich|Maximale Anzahl von Zeichen|
|--|--|--|--|
|NAME|Zulässig:<br>alphanumerische Zeichen (Buchstaben und Ziffern)<br>`_` (Unterstrich)|`^[a-zA-Z0-9_]+$`|100|
|Wert|Alles zulässig mit Ausnahme von:<br>`:` (Doppelpunkt)<br>`|` (senkrechter Strich)|`^[^:|]+$`|500|
|||||

## <a name="knowledge-base-content-limits"></a>Grenzwerte für die Inhalte einer Knowledge Base
Allgemeine Grenzwerte für die Inhalte in der Knowledge Base:
* Länge des Antworttexts: 25.000
* Länge des Fragentexts: 1.000
* Länge des Texts für den Metadatenschlüssel/-wert: 100
* Unterstützte Zeichen für den Metadatennamen: Buchstaben, Ziffern und `_`  
* Unterstützte Zeichen für den Metadatenwerte: Alle Zeichen außer `:` und `|` 
* Länge des Dateinamens: 200
* Unterstützte Dateiformate: „.tsv“, „.pdf“, „.txt“, „.docx“, „.xlsx“.
* Maximale Anzahl von alternativen Fragen: 300
* Maximale Anzahl von Frage-Antwort-Paaren: Abhängig vom ausgewählten **[Azure Cognitive Search-Tarif](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity#document-limits)** . Ein Frage-Antwort-Paar wird einem Dokument im Azure Cognitive Search-Index zugeordnet. 
* URL/HTML-Seite: Eine Million Zeichen

## <a name="create-knowledge-base-call-limits"></a>Grenzwerte für Aufrufe zum Erstellen einer Knowledge Base
Dabei handelt es sich um die Grenzwerte für die einzelnen Aktionen zum Erstellen einer Knowledge Base, d.h. Klicken auf *Create KB* (Knowledge Base erstellen) oder Aufrufen der CreateKnowledgeBase-API.
* Maximale Anzahl von alternativen Fragen pro Antwort: 300
* Maximale Anzahl von URLs: 10
* Maximale Anzahl von Dateien: 10

## <a name="update-knowledge-base-call-limits"></a>Grenzwerte für Aufrufe zum Aktualisieren einer Knowledge Base
Dabei handelt es sich um die Grenzwerte für die einzelnen Aktualisierungsaktionen, d.h. Klicken auf *Save and train* (Speichern und trainieren) oder Aufrufen der UpdateKnowledgeBase-API.
* Länge jedes Quellnamens: 300
* Maximale Anzahl hinzugefügter oder gelöschter alternativer Fragen: 300
* Maximale Anzahl hinzugefügter oder gelöschter Metadatenfelder: 10
* Maximale Anzahl der URLs, die aktualisiert werden können: 5

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wann und wie [Diensttarife](How-To/set-up-qnamaker-service-azure.md#upgrade-qna-maker) geändert werden:
