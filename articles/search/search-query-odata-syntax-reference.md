---
title: Referenz zur OData-Ausdruckssyntax – Azure Search
description: Formale Grammatik- und Syntaxspezifikation für OData-Ausdrücke in Azure Search-Abfragen
ms.date: 06/13/2019
services: search
ms.service: search
ms.topic: conceptual
author: brjohnstmsft
ms.author: brjohnst
ms.manager: cgronlun
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
ms.openlocfilehash: ebe41ba61ac5136900328db9c35acb8551dcd5b2
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67428658"
---
# <a name="odata-expression-syntax-reference-for-azure-search"></a>Referenz zur OData-Ausdruckssyntax für Azure Search

Azure Search verwendet [OData-Ausdrücke](https://docs.oasis-open.org/odata/odata/v4.01/odata-v4.01-part2-url-conventions.html) als Parameter in der gesamten API. Am häufigsten werden OData-Ausdrücke für die Parameter `$orderby` und `$filter` verwendet. Die Ausdrücke können mehrere Klauseln, Funktionen und Operatoren enthalten und daher komplex sein. Allerdings werden selbst einfache OData-Ausdrücke wie Eigenschaftspfade in vielen Bereichen der Azure Search-REST-API verwendet. So werden in der gesamten API Path-Ausdrücke verwendet, um auf Unterfelder von komplexen Feldern zu verweisen, beispielsweise zum Auflisten von Unterfeldern in einer [Vorschlagsfunktion](index-add-suggesters.md), einer [Bewertungsfunktion](index-add-scoring-profiles.md), dem `$select`-Parameter oder sogar der [feldbezogenen Suche in Lucene-Abfragen](query-lucene-syntax.md).

In diesem Artikel werden alle Formen von OData-Ausdrücken anhand einer formalen Grammatik beschrieben. Es gibt auch ein [interaktives Diagramm](#syntax-diagram), das Ihnen hilft, die Grammatik visuell zu erkunden.

## <a name="formal-grammar"></a>Formale Grammatik

Die Teilmenge der von Azure Search unterstützten OData-Programmiersprache kann mit einer EBNF-Grammatik ([Extended Backus-Naur Form](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)) beschrieben werden. Regeln sind von oben nach unten aufgeführt. Die Liste beginnt mit den komplexesten Ausdrücken, auf die die einfacheren Ausdrücke folgen. Am Anfang befinden sich die Grammatikregeln, die den spezifischen Parametern der Azure Search-REST-API entsprechen:

- [`$filter`](search-query-odata-filter.md) – wird durch die `filter_expression`-Regel definiert.
- [`$orderby`](search-query-odata-orderby.md) – wird durch die `order_by_expression`-Regel definiert.
- [`$select`](search-query-odata-select.md) – wird durch die `select_expression`-Regel definiert.
- Feldpfade – werden durch die `field_path`-Regel definiert. Feldpfade werden in der gesamten API verwendet. Sie können entweder auf Felder der obersten Ebene eines Indexes oder auf Unterfelder mit einem oder mehreren Vorgängern in Form [komplexer Felder](search-howto-complex-data-types.md) verweisen.

Im Anschluss an die EBNF folgt ein frei zugängliches [Syntaxdiagramm](https://en.wikipedia.org/wiki/Syntax_diagram), in dem Sie die Grammatik und die Beziehungen zwischen den Regeln interaktiv erkunden können.

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
/* Top-level rules */

filter_expression ::= boolean_expression

order_by_expression ::= order_by_clause(',' order_by_clause)*

select_expression ::= '*' | field_path(',' field_path)*

field_path ::= identifier('/'identifier)*


/* Shared base rules */

identifier ::= [a-zA-Z_][a-zA-Z_0-9]*


/* Rules for $orderby */

order_by_clause ::= (field_path | sortable_function) ('asc' | 'desc')?

sortable_function ::= geo_distance_call | 'search.score()'


/* Rules for $filter */

boolean_expression ::=
    collection_filter_expression
    | logical_expression
    | comparison_expression
    | boolean_literal
    | boolean_function_call
    | '(' boolean_expression ')'
    | variable

/* This can be a range variable in the case of a lambda, or a field path. */
variable ::= identifier | field_path

collection_filter_expression ::=
    field_path'/all(' lambda_expression ')'
    | field_path'/any(' lambda_expression ')'
    | field_path'/any()'

lambda_expression ::= identifier ':' boolean_expression

logical_expression ::=
    boolean_expression ('and' | 'or') boolean_expression
    | 'not' boolean_expression

comparison_expression ::= 
    variable_or_function comparison_operator constant | 
    constant comparison_operator variable_or_function

variable_or_function ::= variable | function_call

comparison_operator ::= 'gt' | 'lt' | 'ge' | 'le' | 'eq' | 'ne'


/* Rules for constants and literals */

constant ::=
    string_literal
    | date_time_offset_literal
    | integer_literal
    | float_literal
    | boolean_literal
    | 'null'

string_literal ::= "'"([^'] | "''")*"'"

date_time_offset_literal ::= date_part'T'time_part time_zone

date_part ::= year'-'month'-'day

time_part ::= hour':'minute(':'second('.'fractional_seconds)?)?

zero_to_fifty_nine ::= [0-5]digit

digit ::= [0-9]

year ::= digit digit digit digit

month ::= '0'[1-9] | '1'[0-2]

day ::= '0'[1-9] | [1-2]digit | '3'[0-1]

hour ::= [0-1]digit | '2'[0-3]

minute ::= zero_to_fifty_nine

second ::= zero_to_fifty_nine

fractional_seconds ::= integer_literal

time_zone ::= 'Z' | sign hour':'minute

sign ::= '+' | '-'

/* In practice integer literals are limited in length to the precision of
the corresponding EDM data type. */
integer_literal ::= digit+

float_literal ::=
    sign? whole_part fractional_part? exponent?
    | 'NaN'
    | '-INF'
    | 'INF'

whole_part ::= integer_literal

fractional_part ::= '.'integer_literal

exponent ::= 'e' sign? integer_literal

boolean_literal ::= 'true' | 'false'


/* Rules for functions */

function_call ::=
    geo_distance_call |
    boolean_function_call

geo_distance_call ::=
    'geo.distance(' variable ',' geo_point ')'
    | 'geo.distance(' geo_point ',' variable ')'

geo_point ::= "geography'POINT(" lon_lat ")'"

lon_lat ::= float_literal ' ' float_literal

boolean_function_call ::=
    geo_intersects_call |
    search_in_call |
    search_is_match_call

geo_intersects_call ::=
    'geo.intersects(' variable ',' geo_polygon ')'

/* You need at least four points to form a polygon, where the first and
last points are the same. */
geo_polygon ::=
    "geography'POLYGON((" lon_lat ',' lon_lat ',' lon_lat ',' lon_lat_list "))'"

lon_lat_list ::= lon_lat(',' lon_lat)*

search_in_call ::=
    'search.in(' variable ',' string_literal(',' string_literal)? ')'

/* Note that it is illegal to call search.ismatch or search.ismatchscoring
from inside a lambda expression. */
search_is_match_call ::=
    'search.ismatch'('scoring')?'(' search_is_match_parameters ')'

search_is_match_parameters ::=
    string_literal(',' string_literal(',' query_type ',' search_mode)?)?

query_type ::= "'full'" | "'simple'"

search_mode ::= "'any'" | "'all'"
```

## <a name="syntax-diagram"></a>Syntaxdiagramm

Nutzen Sie das interaktive Syntaxdiagramm, um die von Azure Search unterstützte Grammatik der OData-Programmiersprache visuell zu erkunden:

> [!div class="nextstepaction"]
> [OData-Syntaxdiagramm für Azure Search](https://azuresearch.github.io/odata-syntax-diagram/)

## <a name="see-also"></a>Weitere Informationen  

- [Filter in Azure Search](search-filters.md)
- [Search Documents &#40;Azure Search Service REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) (Suchen nach Dokumenten: REST-API für den Azure Search-Dienst)
- [Lucene-Abfragesyntax](query-lucene-syntax.md)
- [Simple query syntax in Azure Search](query-simple-syntax.md) (Einfache Abfragesyntax in Azure Search)
