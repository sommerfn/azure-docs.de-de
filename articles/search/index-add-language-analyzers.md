---
title: Hinzufügen von Sprachanalysetools zu Zeichenfolgenfeldern in einem Index
titleSuffix: Azure Cognitive Search
description: Mehrsprachige lexikalische Textanalyse für nicht englischsprachige Abfragen und Indizes in der kognitiven Azure-Suche.
manager: nitinme
author: Yahnoosh
ms.author: jlembicz
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: f5833da5b15c893499b0d786972eff61c7391137
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72790136"
---
# <a name="add-language-analyzers-to-an-azure-cognitive-search-index"></a>Hinzufügen von Sprachanalysetools zu einem Index für die kognitive Azure-Suche

Ein *Sprachanalysetool* ist eine bestimmte Art einer [Textanalyse](search-analyzers.md), die eine lexikalische Analyse mithilfe der linguistischen Regeln der Zielsprache durchführt. Jedes durchsuchbare Feld verfügt über die Eigenschaft **analyzer**. Wenn Ihr Index übersetzte Zeichenfolgen enthält, wie z.B. separate Felder für englischen und chinesischen Text, können Sie für jedes Feld Sprachanalysetools angeben, um auf die umfangreichen linguistischen Funktionen dieser Sprachanalysetools zuzugreifen.  

Die kognitive Azure-Suche unterstützt 35 Analysetools auf Basis von Lucene und 50 Analysetools, die von Microsoft-Technologien zur Verarbeitung natürlicher Sprache unterstützt werden, die in Office und Bing zum Einsatz kommen.

## <a name="comparing-analyzers"></a>Vergleichen von Analysetools

Einige Entwickler bevorzugen möglicherweise die vertrautere, einfachere Open Source-Lösung von Lucene. Lucene-Sprachanalysetools sind schneller, während Microsoft-Sprachanalysetools erweiterte Funktionen bieten, wie z.B. Lemmatisierung, Wortzerlegung (in Sprachen wie Deutsch, Dänisch, Niederländisch, Schwedisch, Norwegisch, Estnisch, Finnisch, Ungarisch und Slowakisch) und Entitätserkennung (URLs, E-Mails, Datumsangaben und Zahlen). Vergleichen Sie nach Möglichkeit die Analyseprogramme von Microsoft und Lucene, um die für Ihre Anforderungen passendere Lösung zu ermitteln. 

Die Indizierung mit Analyseprogrammen von Microsoft dauert je nach Sprache durchschnittlich zwei bis drei Mal länger als mit entsprechenden Analyseprogrammen von Lucene. Die Suchleistung sollte bei durchschnittlich großen Abfragen nicht wesentlich eingeschränkt sein. 

### <a name="english-analyzers"></a>Analysetools für Englisch

Standardmäßig wird das Lucene-Standardanalysetool verwendet, das für Englisch gut funktioniert, vielleicht aber nicht ganz so gut wie das Analysetool für Englisch von Lucene oder das Analysetool für Englisch von Microsoft. 
 
+ Das Analysetool für Englisch von Lucene ist beispielsweise eine Erweiterung des Standardanalysetools. Es entfernt Possessivformen (nachgestelltes -s) bei Wörtern, wendet gemäß dem Wortstammerkennungsalgorithmus von Porter die Wortstammerkennung an und entfernt englische Stoppwörter.  

+ Das Analysetool für Englisch von Microsoft führt die Lemmatisierung anstelle der Wortstammerkennung durch. Dadurch können gebeugte und unregelmäßige Wortformen viel besser verarbeitet werden, was zu relevanteren Suchergebnissen führt. 

## <a name="configuring-analyzers"></a>Konfigurieren von Analysetools

Sprachanalysen werden in der vorliegenden Form verwendet. Sie können für die Eigenschaft **analyzer** aller Felder in der Indexdefinition den Namen eines Analyseprogramms festlegen, das die Sprache und den Linguistikstapel (Microsoft oder Lucene) angibt. Bei der Indizierung dieses Felds und der Suche danach wird das gleiche Analyseprogramm verwendet. Beispiel: Sie können im selben Index separate Felder für englische, französische und spanische Hotelbeschreibungen verwenden. Alternativ dazu können Sie anstelle von **analyzer** die Eigenschaften **indexAnalyzer** und **searchAnalyzer** verwenden, um unterschiedliche Analyseregeln zur Indizierungszeit und zur Abfragezeit anzuwenden. 

Verwenden Sie den Abfrageparameter **searchFields**, um anzugeben, für welches sprachspezifische Feld in den Abfragen gesucht werden soll. Beispiele für Abfragen mit der Eigenschaft „analyzer“ finden Sie unter [Search Documents (Azure Search Service REST API)](https://docs.microsoft.com/rest/api/searchservice/search-documents) (Durchsuchen von Dokumenten (REST-API des Azure Search-Diensts)). 

Weitere Informationen zu Indexeigenschaften finden Sie unter [Erstellen eines Index &#40;REST-API für die kognitive Azure-Suche&#41;](https://docs.microsoft.com/rest/api/searchservice/create-index). Weitere Informationen zur Analyse in der kognitiven Azure-Suche finden Sie unter [Analysetools in der kognitiven Azure-Suche](https://docs.microsoft.com/azure/search/search-analyzers).

<a name="language-analyzer-list"></a>

## <a name="language-analyzer-list"></a>Liste der Sprachanalysen 
 Es folgt die Liste der unterstützten Sprachen sowie die Namen der Lucene- und Microsoft-Analyseprogramme.  

|Sprache|Name des Microsoft-Analysetools|Name des Lucene-Analysetools|  
|--------------|-----------------------------|--------------------------|  
|Arabisch|ar.microsoft|ar.lucene|  
|Armenisch||hy.lucene|  
|Bengalisch|bn.microsoft||  
|Baskisch||eu.lucene|  
|Bulgarisch|bg.microsoft|bg.lucene|  
|Katalanisch|ca.microsoft|ca.lucene|  
|Chinesisch (vereinfacht)|zh-Hans.microsoft|zh-Hans.lucene|  
|Chinesisch (traditionell)|zh-Hant.microsoft|zh-Hant.lucene|  
|Kroatisch|hr.microsoft||  
|Tschechisch|cs.microsoft|cs.lucene|  
|Dänisch|da.microsoft|da.lucene|  
|Niederländisch|nl.microsoft|nl.lucene|  
|Englisch|en.microsoft|en.lucene|  
|Estnisch|et.microsoft||  
|Finnisch|fi.microsoft|fi.lucene|  
|Französisch|fr.microsoft|fr.lucene|  
|Galizisch||gl.lucene|  
|Deutsch|de.microsoft|de.lucene|  
|Griechisch|el.microsoft|el.lucene|  
|Gujarati|gu.microsoft||  
|Hebräisch|he.microsoft||  
|Hindi|hi.microsoft|hi.lucene|  
|Ungarisch|hu.microsoft|hu.lucene|  
|Isländisch|is.microsoft||  
|Indonesisch (Bahasa)|id.microsoft|id.lucene|  
|Irisch||ga.lucene|  
|Italienisch|it.microsoft|it.lucene|  
|Japanisch|ja.microsoft|ja.lucene|  
|Kannada|kn.microsoft||  
|Koreanisch|ko.microsoft|ko.lucene|  
|Lettisch|lv.microsoft|lv.lucene|  
|Litauisch|lt.microsoft||  
|Malayalam|ml.microsoft||  
|Malaiisch (Lateinisch)|ms.microsoft||  
|Marathi|mr.microsoft||  
|Norwegisch|nb.microsoft|no.lucene|  
|Persisch||fa.lucene|  
|Polnisch|pl.microsoft|pl.lucene|  
|Portugiesisch (Brasilien)|pt-Br.microsoft|pt-Br.lucene|  
|Portugiesisch (Portugal)|pt-Pt.microsoft|pt-Pt.lucene|  
|Pandschabi|pa.microsoft||  
|Rumänisch|ro.microsoft|ro.lucene|  
|Russisch|ru.microsoft|ru.lucene|  
|Serbisch (Kyrillisch)|sr-cyrillic.microsoft||  
|Serbisch (Lateinisch)|sr-latin.microsoft||  
|Slowakisch|sk.microsoft||  
|Slowenisch|sl.microsoft||  
|Spanisch|es.microsoft|es.lucene|  
|Schwedisch|sv.microsoft|sv.lucene|  
|Tamilisch|ta.microsoft||  
|Telugu|te.microsoft||  
|Thailändisch|th.microsoft|th.lucene|  
|Türkisch|tr.microsoft|tr.lucene|  
|Ukrainisch|uk.microsoft||  
|Urdu|ur.microsoft||  
|Vietnamesisch|vi.microsoft||  

 Alle Analysetools mit **Lucene** im Namen werden von den [Sprachanalysetools von Apache Lucene](https://lucene.apache.org/core/6_6_1/core/overview-summary.html ) unterstützt.

## <a name="see-also"></a>Weitere Informationen  

+ [Erstellen eines Index &#40;REST-API für die kognitive Azure-Suche&#41;](https://docs.microsoft.com/rest/api/searchservice/create-index)  

+ [AnalyzerName-Klasse](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.analyzername)  

