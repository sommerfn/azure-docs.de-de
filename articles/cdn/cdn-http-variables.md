---
title: HTTP-Variablen für Azure CDN-Regel-Engine | Microsoft-Dokumentation
description: HTTP-Variablen ermöglichen Ihnen das Abrufen von HTTP-Anforderungs- und -Antwortmetadaten.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: magattus
ms.openlocfilehash: 53ad0c516547e17801bd57c2fd6b0d1704383797
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2019
ms.locfileid: "67593813"
---
# <a name="http-variables-for-azure-cdn-rules-engine"></a>HTTP-Variablen für Azure CDN-Regel-Engine
HTTP-Variablen stellen die Methoden für den Abruf von HTTP-Anforderungs- und -Antwortmetadaten bereit. Diese Metadaten können dann zur dynamischen Änderung einer Anforderung oder Antwort verwendet werden. Die Verwendung von HTTP-Variablen ist auf die folgenden Regel-Engine-Features beschränkt :

- [Cache-Key Rewrite](cdn-verizon-premium-rules-engine-reference-features.md#cache-key-rewrite)
- [Modify Client Request Header](cdn-verizon-premium-rules-engine-reference-features.md#modify-client-request-header)
- [Modify Client Response Header](cdn-verizon-premium-rules-engine-reference-features.md#modify-client-response-header)
- [URL Redirect](cdn-verizon-premium-rules-engine-reference-features.md#url-redirect)
- [URL Rewrite](cdn-verizon-premium-rules-engine-reference-features.md#url-rewrite)

## <a name="definitions"></a>Definitionen
In der folgenden Tabelle sind die unterstützten HTTP-Variablen beschrieben. Ein leerer Wert wird zurückgegeben, wenn GEO-Metadaten (z.B. Postleitzahl) für eine bestimmte Anforderung nicht verfügbar sind.


| NAME | Variable | BESCHREIBUNG | Beispielwert |
| ---- | -------- | ----------- | ------------ |
| ASN (Anfordernde Person) | %{geo_asnum} | Gibt die AS-Nummer der anfordernden Person an. <br /><br />**Veraltet:** %{virt_dst_asnum}. <br />Diese Variable wurde zugunsten von „%{geo_asnum}“ als veraltet markiert. Eine Regel mit dieser veralteten Variable funktioniert zwar weiterhin, Sie sollten sie jedoch so aktualisieren, dass die neue Variable verwendet wird. | AS15133 |
| Ort (Anfordernde Person) | %{geo_city} | Gibt den Ort der anfordernden Person an. | Los Angeles |
| Kontinent (Anfordernde Person) | %{geo_continent} | Gibt den Kontinent der anfordernden Person über die jeweilige Abkürzung an. <br />Gültige Werte sind: <br />AF: Afrika<br />AS: Asien<br />EU: Europa<br />NA: Nordamerika<br />OC: Ozeanien<br />SA: Südamerika<br /><br />**Veraltet:** %{virt_dst_continent}. <br />Diese Variable wurde zugunsten von „%{geo_continent}“ als veraltet markiert. <br />Eine Regel mit dieser veralteten Variable funktioniert zwar weiterhin, Sie sollten sie jedoch so aktualisieren, dass die neue Variable verwendet wird.| – |
| Cookiewert | %{cookie_Cookie} | Gibt den Wert zurück, der dem durch den Cookieausdruck identifizierten Cookieschlüssel entspricht. | Verwendungsbeispiel: <br />%{cookie__utma}<br /><br />Beispielwert:<br />111662281.2.10.1222100123 |
| Land (Anfordernde Person) | %{geo_country} | Gibt das Herkunftsland der anfordernden Person über den jeweiligen Ländercode an. <br />**Veraltet:** %{virt_dst_country}. <br /><br />Diese Variable wurde zugunsten von „%{geo_country}“ als veraltet markiert. Eine Regel mit dieser veralteten Variable funktioniert zwar weiterhin, Sie sollten sie jedoch so aktualisieren, dass die neue Variable verwendet wird. | US |
| Designated Market Area (Anfordernde Person) | %{geo_dma_code} |Gibt den Medienmarkt der anfordernden Person anhand des Regionscodes an. <br /><br />Dieses Feld gilt nur für Anforderungen, die aus den Vereinigten Staaten stammen.| 745 |
| HTTP-Anforderungsmethode | %{request_method} | Gibt die HTTP-Anforderungsmethode an. | GET |
| HTTP-Statuscode | %{status} | Gibt den HTTP-Statuscode für die Antwort an. | 200 |
| IP-Adresse (Anfordernde Person) | %{virt_dst_addr} | Gibt die IP-Adresse des Anforderers an. | 192.168.1.1 |
| Breitengrad (Anfordernde Person) | %{geo_latitude} | Gibt den Breitengrad der anfordernden Person an. | 34.0995 |
| Längengrad (Anfordernde Person) | %{geo_longitude} | Gibt den Längengrad der anfordernden Person an. | -118.4143 |
| Metropolitan Statistical Area (Anfordernde Person) | %{geo_metro_code} | Gibt die Metropolregion der anfordernden Person an. <br /><br />Dieses Feld gilt nur für Anforderungen, die aus den Vereinigten Staaten stammen.<br />| 745 |
| Port (Anfordernde Person) | %{virt_dst_port} | Gibt den kurzlebigen Port der anfordernden Person an. | 55885 |
| Postleitzahl (Anfordernde Person) | %{geo_postal_code} | Gibt die Postleitzahl der anfordernden Person an. | 90210 |
| Abfragezeichenfolge gefunden | %{is_args} | Der Wert für diese Variable variiert je nachdem, ob die Anforderung eine Abfragezeichenfolge enthält.<br /><br />– Abfragezeichenfolge gefunden: ?<br />- Keine Abfragezeichenfolge: NULL | ? |
| Abfragezeichenfolgen-Parameter gefunden | %{is_amp} | Der Wert für diese Variable variiert je nachdem, ob die Anforderung mindestens einen Abfragezeichenfolge-Parameter enthält.<br /><br />– Parameter gefunden: &<br />- Keine Parameter: NULL | & |
| Parameterwert der Abfragezeichenfolge | %{arg_&lt;parameter&gt;} | Gibt den Wert zurück, der dem durch den &lt;Parameter&gt;-Ausdruck identifizierten Abfragezeichenfolgen-Parameter entspricht. | Verwendungsbeispiel: <br />%{arg_language}<br /><br />Beispiel für Abfragezeichenfolge-Parameter: <br />?language=en<br /><br />Beispielwert: en |
| Abfragezeichenfolgenwert | %{query_string} | Gibt den vollständigen in der Anforderungs-URL definierten Abfragezeichenfolgenwert an. |key1=val1&key2=val2&key3=val3 |
| Verweisdomäne | %{referring_domain} | Gibt die im Verweisanforderungsheader definierte Domäne an. | <www.google.com> |
| Region (Anfordernde Person) | %{geo_region} | Gibt die Region der anfordernden Person (z.B. Bundesland oder Kanton) über deren alphanumerische Abkürzung an. | CA |
| Anforderungsheaderwert | %{http_RequestHeader} | Gibt den Wert zurück, der dem durch den RequestHeader-Ausdruck identifizierten Anforderungsheader entspricht. <br /><br />Wenn der Name des Anforderungsheaders einen Bindestrich (z.B. Benutzer-Agent) enthält, ersetzen Sie diesen durch einen Unterstrich (z.B. Benutzer_Agent).| Beispielverwendung: %{http_Connection}<br /><br />Beispielwert: Keep-Alive | 
| Anforderungshost | %{host} | Gibt den in der Anforderungs-URL definierten Host an. | <www.mydomain.com> |
| Anforderungsprotokoll | %{request_protocol} | Gibt das Anforderungsprotokoll an. | HTTP/1.1 |
| Request Scheme | %{scheme} | Gibt das Anforderungsschema an. |http |
| Anforderungs-URI (relativ) | %{request_uri} | Gibt den relativen Pfad, einschließlich der Abfragezeichenfolge, der im Anforderungs-URI definiert ist. | /marketing/foo.js?loggedin=true |
| Anforderungs-URI (relativ, ohne Abfragezeichenfolge) | %{uri} | Gibt den relativen Pfad zum angeforderten Inhalt an. <br /><br/>Wichtige Informationen:<br />– Dieser relative Pfad schließt die Abfragezeichenfolge aus.<br />– Dieser relative Pfad spiegelt erneute URL-Generierungen wider. Eine URL wird unter den folgenden Bedingungen neu generiert:<br />  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Feature „URL Rewrite“: Dieses Feature generiert den im Anforderungs-URI definierten relativen Pfad neu.<br />    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Edge-CNAME-URL: Diese Art von Anforderung wird in die entsprechende CDN-URL umgeschrieben. |/800001/corigin/rewrittendir/foo.js |
| Anforderungs-URI | %{request} | Beschreibt die Anforderung. <br />Syntax: &lt;HTTP-Methode&gt; &lt;relativer Pfad&gt; &lt;HTTP-Protokoll&gt; | GET /marketing/foo.js?loggedin=true HTTP/1.1 |
| Antwortheaderwert | %{resp_&lt;ResponseHeader&gt;} | Gibt den Wert zurück, der dem durch den &lt;ResponseHeader&gt;-Ausdruck identifizierten Antwortheader entspricht. <br /><br />Wenn der Name des Anwortheaders einen Bindestrich (z.B. Benutzer-Agent) enthält, ersetzen Sie diesen durch einen Unterstrich (z.B. Benutzer_Agent). | Beispielverwendung: %{resp_Content_Length}<br /><br />Beispielwert: 100 |

## <a name="usage"></a>Verwendung
In der folgenden Tabelle ist die richtige Syntax zum Angeben einer HTTP-Variable beschrieben.


| Syntax | Beispiel | BESCHREIBUNG |
| ------ | -------- | ---------- |
| %{&lt;HTTPVariable&gt;} | %{host} | Verwenden Sie diese Syntax zum Abrufen des gesamten Werts, welcher dem angegebenen &lt;HTTPVariable&gt;-Element entspricht. |
| %{&lt;HTTPVariableDelimiter&gt;} | %{host,} | Verwenden Sie diese Syntax zum Festlegen des Falls für den gesamten Wert, welcher dem angegebenen &lt;HTTPVariableDelimiter&gt;-Element entspricht. |
| %{&lt;HTTPVariableDelimiterExpression&gt;} | %{host/=^www\.([^\.]+)\.([^\.:]+)/cdn.$2.$3:80} | Verwenden Sie einen regulären Ausdruck für &lt;HTTPVariableDelimiterExpression&gt; um den Wert einer HTTP-Variablen zu ersetzen, zu löschen oder zu bearbeiten. |

HTTP-Variablennamen unterstützen nur alphabetische Zeichen und Unterstriche. Konvertieren Sie nicht unterstützte Zeichen in Unterstriche.

## <a name="delimiter-reference"></a>Trennzeichenverweis
Nach einer HTTP-Variablen kann ein Trennzeichen angegeben werden, um eine der folgenden Auswirkungen zu erzielen:

- Transformieren des der Variable zugeordneten Werts

     Beispiel: Konvertieren des gesamten Werts in Kleinbuchstaben.

- Löschen des der Variable zugeordneten Werts

- Bearbeiten des der Variable zugeordneten Werts

     Beispiel: Verwenden Sie reguläre Ausdrücke, um den der HTTP-Variable zugeordneten Wert zu ändern.

Die Trennzeichen sind in der folgenden Tabelle beschrieben.

| Trennzeichen | BESCHREIBUNG |
| --------- | ----------- |
| := | Gibt an, dass der Variablen ein Standardwert zugewiesen wird, wenn eine der folgenden Bedingungen auf sie zutrifft: <br />– Fehlt <br />– Auf NULL festgelegt. |
| :+ | Gibt an, dass der Variablen ein Standardwert zugewiesen wird, wenn ihr ein Wert zugewiesen wurde. |
| : | Gibt an, dass eine Teilzeichenfolge des Werts erweitert wird, die der Variablen zugewiesen ist. |
| # | Gibt an, dass das nach diesem Trennzeichen angegebene Muster gelöscht werden soll, wenn es sich am Anfang des der Variablen zugeordneten Werts befindet. |
| % | Gibt an, dass das nach diesem Trennzeichen angegebene Muster gelöscht werden soll, wenn es sich am Ende des der Variablen zugeordneten Werts befindet. <br />Diese Definition ist nur anwendbar, wenn das Symbol „%“ als Trennzeichen verwendet wird. |
| / | Begrenzt eine HTTP-Variable oder ein Muster. |
| // | Sucht und ersetzt alle Instanzen des angegebenen Musters. |
| /= | Sucht und kopiert alle Vorkommen des angegebenen Musters und generiert sie erneut. |
| , | Konvertiert den der HTTP-Variablen zugeordneten Wert in Kleinbuchstaben. |
| ^ | Konvertiert den der HTTP-Variablen zugeordneten Wert in Großbuchstaben. |
| ,, | Konvertiert alle Instanzen des angegebenen Zeichens in dem der HTTP-Variablen zugeordneten Wert in Kleinbuchstaben. |
| ^^ | Konvertiert alle Instanzen des angegebenen Zeichens in dem der HTTP-Variablen zugeordneten Wert in Großbuchstaben. |

## <a name="exceptions"></a>Ausnahmen
In der folgenden Tabelle sind Umstände beschrieben, unter denen der angegebene Text nicht als HTTP-Variable behandelt wird.

| Bedingung | BESCHREIBUNG | Beispiel |
| --------- | ----------- | --------|
| „%“ mit Escapezeichen versehen | Das Prozentzeichen kann durch die Verwendung eines umgekehrten Schrägstrichs mit einem Escapezeichen versehen werden. <br />Der Beispielwert auf der rechten Seite wird als Literalwert und nicht als HTTP-Variable behandelt.| \%{host} |
| Unknown variables | An empty string is always returned for unknown variables. | %{unknown_variable} |
| Invalid characters or syntax | Variables that contain invalid characters or syntax are treated as literal values. <br /><br />Example #1: The specified value contains an invalid character (for example, -). <br /><br />Example #2: The specified value contains a double set of curly braces. <br /><br />Example #3: The specified value is missing a closing curly brace.<br /> | Example #1: %{resp_user-agent} <br /><br />Example #2: %{{host}} <br /><br />Example #3: %{host |
| Missing variable name | A NULL value is always returned when a variable is not specified. | %{} |
| Trailing characters | Characters that trail a variable are treated as literal values. <br />The sample value to the right contains a trailing curly brace that will be treated as a literal value. | %{host}} |

## Setting default header values
A default value can be assigned to a header when it meets any of the following conditions:
- Missing/unset
- Set to NULL.

The following table describes how to define a default value.

| Condition | Syntax | Example | Description |
| --------- | ------ | --------| ----------- |
| Set a header to a default value when it meets any of the following conditions: <br /><br />- Missing Header <br /><br />- Header value is set to NULL.| %{Variable:=Value} | %{http_referrer:=unspecified} | The Referrer header will only be set to *unspecified* when it is either missing or set to NULL. No action will take place if it has been set. |
| Set a header to a default value when it is missing. | %{Variable=Value} | %{http_referrer=unspecified} | The Referrer header will only be set to *unspecified* when it is missing. No action will take place if it has been set. |
| Set the header to a default value when it does not meet any of the following conditions: <br /><br />- Missing<br /><br /> - Set to NULL. | %{Variable:+Value} | %{http_referrer:+unspecified} | The Referrer header will only be set to *unspecified* when a value has been assigned to it. No action will take place if it is either missing or set to NULL. |

## Manipulating variables
Variables can be manipulated in the following ways:

- Expanding substrings
- Removing patterns

### Substring expansion
By default, a variable will expand to its full value. Use the following syntax to only expand a substring of the variable's value.

`%<Variable>:<Offset>:<Length>}`

Key information:

- The value assigned to the Offset term determines the starting character of the substring:

     - Positive: The starting character of the substring is calculated from the first character in the string.
     - Zero: The starting character of the substring is the first character in the string.
     - Negative: The starting character of the substring is calculated from the last character in the string.

- The length of the substring is determined by the *Length* term:

     - Omitted: Omitting the Length term allows the substring to include all characters between the starting character and the end of the string.
     - Positive: Determines the length of the substring from the starting character to the right.
     - Negative: Determines the length of the substring from the starting character to the left.

#### Example:

The following example relies on the following sample request URL:

https:\//cdn.mydomain.com/folder/marketing/myconsultant/proposal.html

The following string demonstrates various methods for manipulating variables:

https:\//www%{http_host:3}/mobile/%{request_uri:7:10}/%{request_uri:-5:-8}.htm

Based on the sample request URL, the above variable manipulation will produce the following value:

https:\//www.mydomain.com/mobile/marketing/proposal.htm


### Pattern removal
Text that matches a specific pattern can be removed from either the beginning or the end of a variable's value.

| Syntax | Action |
| ------ | ------ |
| %{Variable#Pattern} | Remove text when the specified pattern is found at the beginning of a variable's value. |
| %{Variable%Pattern} | Remove text when the specified pattern is found at the end of a variable's value. |

#### Example:

In this sample scenario, the *request_uri* variable is set to:

`/800001/myorigin/marketing/product.html?language=en-US`

The following table demonstrates how this syntax works.

| Sample syntax | Results | |
| ------------- | ------- | --- |
| %{request_uri#/800001}/customerorigin | /customerorigin/myorigin/marketing/product.html?language=en-US | Because the variable starts with the pattern, it was replaced. |
| %{request_uri%html}htm | /800001/myorigin/marketing/product.html?language=en-US | Because the variable doesn't end with the pattern, there was no change.|

### Find and replace
The find and replace syntax is described in the following table.

| Syntax | Action |
| ------ | ------ |
| %{Variable/Find/Replace} | Find and replace first occurrence of the specified pattern. |
| %{Variable//Find/Replace} | Find and replace all occurrences of the specified pattern. |
| %{Variable^} |Convert the entire value to uppercase. |
| %{Variable^Find} | Convert the first occurrence of the specified pattern to uppercase. |
| %{Variable,} | Convert the entire value to lowercase. |
| %{Variable,Find} | Convert the first occurrence of the specified pattern to lowercase. |

### Find and rewrite
For a variation on find and replace, use the text that matches the specified pattern when rewriting it. The find and rewrite syntax is described in the following table.

| Syntax | Action |
| ------ | ------ |
| %{Variable/=Find/Rewrite} | Find, copy, and rewrite all occurrences of the specified pattern. |
| %{Variable/^Find/Rewrite} | Find, copy, and rewrite the specified pattern when it occurs at the start of the variable. |
| %{Variable/$Find/Rewrite} | Find, copy, and rewrite the specified pattern when it occurs at the end of the variable. |
| %{Variable/Find} | Find and delete all occurrences of the specified pattern. |

Key information:

- Expand text that matches the specified pattern by specifying a dollar sign followed by a whole integer (for example, $1).

- Multiple patterns can be specified. The order in which the pattern is specified determines the integer that will be assigned to it. In the following example, the first pattern matches "www.," the second pattern matches the second-level domain, and the third pattern matches the top-level domain:

    `%{host/=^www\.([^\.]+)\.([^\.:]+)/cdn.$2.$3:80}`

- The rewritten value can consist of any combination of text and these placeholders.

    In the previous example, the hostname is rewritten to `cdn.$2.$3:80` (for example, cdn.mydomain.com:80).

- The case of a pattern placeholder (for example, $1) can be modified through the following flags:
     - U: Uppercase the expanded value.

         Sample syntax:

         `%{host/=^www\.([^\.]+)\.([^\.:]+)/cdn.$U2.$3:80}`

     - L: Lowercase the expanded value.

         Sample syntax:

         `%{host/=^www\.([^\.]+)\.([^\.:]+)/cdn.$L2.$3:80}`

- An operator must be specified before the pattern. The specified operator determines the pattern-capturing behavior:

     - `=`: Indicates that all occurrences of the specified pattern must be captured and rewritten.
     - `^`: Indicates that only text that starts with the specified pattern will be captured.
     - `$`: Indicates that only text that ends with the specified pattern will be capture.
 
- If you omit the */Rewrite* value, the text that matches the pattern is deleted.

\`{host}le: HTTP variables for Azure CDN rules engine | Microsoft Docs
description: HTTP variables allow you to retrieve HTTP request and response metadata.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''

ms.assetid: 
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: magattus


---
# HTTP variables for Azure CDN rules engine
HTTP variables provide the means through which you can retrieve HTTP request and response metadata. This metadata can then be used to dynamically alter a request or a response. The use of HTTP variables is restricted to the following rules engine features:

- [Cache-Key Rewrite](cdn-verizon-premium-rules-engine-reference-features.md#cache-key-rewrite)
- [Modify Client Request Header](cdn-verizon-premium-rules-engine-reference-features.md#modify-client-request-header)
- [Modify Client Response Header](cdn-verizon-premium-rules-engine-reference-features.md#modify-client-response-header)
- [URL Redirect](cdn-verizon-premium-rules-engine-reference-features.md#url-redirect)
- [URL Rewrite](cdn-verizon-premium-rules-engine-reference-features.md#url-rewrite)

## Definitions
The following table describes the supported HTTP variables. A blank value is returned when GEO metadata (for example, postal code) is unavailable for a particular request.


| Name | Variable | Description | Sample value |
| ---- | -------- | ----------- | ------------ |
| ASN (Requester) | %{geo_asnum} | Indicates the requester's AS number. <br /><br />**Deprecated:** %{virt_dst_asnum}. <br />This variable has been deprecated in favor of %{geo_asnum}. Although a rule that uses this deprecated variable will continue to work, you should update it to use the new variable. | AS15133 |
| City (Requester) | %{geo_city} | Indicates the requester's city. | Los Angeles |
| Continent (Requester) | %{geo_continent} | Indicates the requester's continent through its abbreviation. <br />Valid values are: <br />AF: Africa<br />AS: Asia<br />EU: Europe<br />NA: North America<br />OC: Oceania<br />SA: South America<br /><br />**Deprecated:** %{virt_dst_continent}. <br />This variable has been deprecated in favor of %{geo_continent}. <br />Although a rule that uses this deprecated variable will continue to work, you should update it to use the new variable.| N/A |
| Cookie Value | %{cookie_Cookie} | Returns the value corresponding to the cookie key identified by the Cookie term. | Sample Usage: <br />%{cookie__utma}<br /><br />Sample Value:<br />111662281.2.10.1222100123 |
| Country (Requester) | %{geo_country} | Indicates the requester's country of origin through its country code. <br />**Deprecated:** %{virt_dst_country}. <br /><br />This variable has been deprecated in favor of %{geo_country}. Although a rule that uses this deprecated variable will continue to work, you should update it to use the new variable. | US |
| Designated Market Area (Requester) | %{geo_dma_code} |Indicates the requester's media market by its region code. <br /><br />This field is only applicable to requests that originate from the United States.| 745 |
| HTTP Request Method | %{request_method} | Indicates the HTTP request method. | GET |
| HTTP Status Code | %{status} | Indicates the HTTP status code for the response. | 200 |
| IP Address (Requester) | %{virt_dst_addr} | Indicates the requester's IP address. | 192.168.1.1 |
| Latitude (Requester) | %{geo_latitude} | Indicates the requester's latitude. | 34.0995 |
| Longitude (Requester) | %{geo_longitude} | Indicates the requester's longitude. | -118.4143 |
| Metropolitan Statistical Area (Requester) | %{geo_metro_code} | Indicates the requester's metropolitan area. <br /><br />This field is only applicable to requests that originate from the United States.<br />| 745 |
| Port (Requester) | %{virt_dst_port} | Indicates the requester's ephemeral port. | 55885 |
| Postal Code (Requester) | %{geo_postal_code} | Indicates the requester's postal code. | 90210 |
| Query String Found | %{is_args} | The value for this variable varies according to whether the request contains a query string.<br /><br />- Query String Found: ?<br />- No Query String: NULL | ? |
| Query String Parameter Found | %{is_amp} | The value for this variable varies according to whether the request contains at least one query string parameter.<br /><br />- Parameter Found: &<br />- No Parameters: NULL | & |
| Query String Parameter Value | %{arg_&lt;parameter&gt;} | Returns the value corresponding to the query string parameter identified by the &lt;parameter&gt; term. | Sample Usage: <br />%{arg_language}<br /><br />Sample Query String Parameter: <br />?language=en<br /><br />Sample Value: en |
| Query String Value | %{query_string} | Indicates the entire query string value defined in the request URL. |key1=val1&key2=val2&key3=val3 |
| Referrer Domain | %{referring_domain} | Indicates the domain defined in the Referrer request header. | <www.google.com> |
| Region (Requester) | %{geo_region} | Indicates the requester's region (for example, state or province) through its alphanumeric abbreviation. | CA |
| Request Header Value | %{http_RequestHeader} | Returns the value corresponding to the request header identified by the RequestHeader term. <br /><br />If the name of the request header contains a dash (for example, User-Agent), replace it with an underscore (for example, User_Agent).| Sample Usage: %{http_Connection}<br /><br />Sample Value: Keep-Alive | 
| Request Host | %{host} | Indicates the host defined in the request URL. | <www.mydomain.com> |
| Request Protocol | %{request_protocol} | Indicates the request protocol. | HTTP/1.1 |
| Request Scheme | %{scheme} | Indicates the request scheme. |http |
| Request URI (Relative) | %{request_uri} | Indicates the relative path, including the query string, defined in the request URI. | /marketing/foo.js?loggedin=true |
| Request URI (Relative without query string) | %{uri} | Indicates the relative path to the requested content. <br /><br/>Key information:<br />- This relative path excludes the query string.<br />- This relative path reflects URL rewrites. A URL will be rewritten under the following conditions:<br />  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- URL Rewrite Feature: This feature rewrites the relative path defined in the request URI.<br />    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Edge CNAME URL: This type of request is rewritten to the corresponding CDN URL. |/800001/corigin/rewrittendir/foo.js |
| Request URI | %{request} | Describes the request. <br />Syntax: &lt;HTTP method&gt; &lt;relative path&gt; &lt;HTTP protocol&gt; | GET /marketing/foo.js?loggedin=true HTTP/1.1 |
| Response Header Value | %{resp_&lt;ResponseHeader&gt;} | Returns the value corresponding to the response header identified by the &lt;ResponseHeader&gt; term. <br /><br />If the name of the response header contains a dash (for example, User-Agent), replace it with an underscore (for example, User_Agent). | Sample Usage: %{resp_Content_Length}<br /><br />Sample Value: 100 |

## Usage
The following table describes the proper syntax for specifying an HTTP variable.


| Syntax | Example | Description |
| ------ | -------- | ---------- |
| %{&lt;HTTPVariable&gt;} | %{host} | Use this syntax to get the entire value corresponding to the specified &lt;HTTPVariable&gt;. |
| %{&lt;HTTPVariableDelimiter&gt;} | %{host,} | Use this syntax to set the case for the entire value corresponding to the specified  &lt;HTTPVariableDelimiter&gt;. |
| %{&lt;HTTPVariableDelimiterExpression&gt;} | %{host/=^www\.([^\.]+)\.([^\.:]+)/cdn.$2.$3:80} | Use a regular expression for &lt;HTTPVariableDelimiterExpression&gt; to replace, delete, or manipulate an HTTP variable's value. |

HTTP variable names only support alphabetic characters and underscores. Convert unsupported characters to underscores.

## Delimiter reference
A delimiter can be specified after an HTTP variable to achieve any of the following effects:

- Transform the value associated with the variable.

     Example: Convert the entire value to lowercase.

- Delete the value associated with the variable.

- Manipulate the value associated with the variable.

     Example: Use regular expressions to change the value associated with the HTTP variable.

The delimiters are described in the following table.

| Delimiter | Description |
| --------- | ----------- |
| := | Indicates that a default value will be assigned to the variable when it is either: <br />- Missing <br />- Set to NULL. |
| :+ | Indicates that a default value will be assigned to the variable when a value has been assigned to it. |
| : | Indicates that a substring of the value assigned to the variable will be expanded. |
| # | Indicates that the pattern specified after this delimiter should be deleted when it is found at the beginning of the value associated with the variable. |
| % | Indicates that the pattern specified after this delimiter should be deleted when it is found at the end of the value associated with the variable. <br />This definition is only applicable when the % symbol is used as a delimiter. |
| / | Delimits an HTTP variable or a pattern. |
| // | Find and replace all instances of the specified pattern. |
| /= | Find, copy, and rewrite all occurrences of the specified pattern. |
| , | Convert the value associated with the HTTP variable to lowercase. |
| ^ | Convert the value associated with the HTTP variable to uppercase. |
| ,, | Convert all instances of the specified character in the value associated with the HTTP variable to lowercase. |
| ^^ | Convert all instances of the specified character in the value associated with the HTTP variable to uppercase. |

## Exceptions
The following table describes circumstances under which the specified text isn't treated as an HTTP variable.

| Condition | Description | Example |
| --------- | ----------- | --------|
| Escaping % symbol | The percentage symbol can be escaped through the use of a backslash. <br />The sample value to the right will be treated as a literal value and not as an HTTP variable.| \%{host} |
| Unbekannte Variablen | Für unbekannte Variablen wird immer eine leere Zeichenfolge zurückgegeben. | %{unknown_variable} |
| Ungültige Zeichen oder Syntax | Variablen, die ungültige Zeichen oder eine ungültige Syntax enthalten, werden als Literalwerte behandelt. <br /><br />Beispiel 1: Der angegebene Wert enthält ein ungültiges Zeichen (z. B. „-“). <br /><br />Beispiel 2: Der angegebene Wert enthält doppelte geschweifte Klammern. <br /><br />Beispiel 3: Beim angegebenen Wert fehlt eine schließende geschweifte Klammer.<br /> | Beispiel Nr. 1: %{resp_user-agent} <br /><br />Beispiel Nr. 2: %{{host}} <br /><br />Beispiel Nr. 3: %{host |
| Fehlender Variablenname | Ein NULL-Wert wird immer zurückgegeben, wenn eine Variable nicht angegeben ist. | %{} |
| Nachgestellte Zeichen | Zeichen, die am Ende einer Variable stehen, werden als Literalwerte behandelt. <br />Der Beispielwert auf der rechten Seite enthält eine nachgestellte geschweifte Klammer, die als Literalwert behandelt wird. | %{host}} |

## <a name="setting-default-header-values"></a>Festlegen von Standardwerten für Header
Einem Header kann ein Standardwert zugewiesen werden, wenn er eine der folgenden Bedingungen erfüllt:
- Fehlt/nicht festgelegt
- Auf NULL festgelegt.

In der folgenden Tabelle ist beschrieben, wie ein Standardwert definiert wird.

| Bedingung | Syntax | Beispiel | BESCHREIBUNG |
| --------- | ------ | --------| ----------- |
| Ein Header wird auf einen Standardwert festgelegt, wenn er eine der folgenden Bedingungen erfüllt: <br /><br />– Fehlender Header <br /><br />– Headerwert ist auf NULL festgelegt.| %{Variable:=Value} | %{http_referrer:=unspecified} | Der Verweisheader wird nur auf *nicht angegeben* festgelegt, wenn er entweder fehlt oder auf NULL festgelegt ist. Wenn er festgelegt wurde, erfolgt keine Aktion. |
| Ein Header wird auf einen Standardwert festgelegt, wenn er fehlt. | %{Variable=Value} | %{http_referrer=unspecified} | Der Verweisheader wird nur auf *nicht angegeben* festgelegt, wenn er fehlt. Wenn er festgelegt wurde, erfolgt keine Aktion. |
| Der Header wird auf einen Standardwert festgelegt, wenn keine der folgenden Bedingungen auf ihn zutrifft: <br /><br />– Fehlt<br /><br /> – Auf NULL festgelegt. | %{Variable:+Value} | %{http_referrer:+unspecified} | Der Verweisheader wird nur auf *nicht angegeben* festgelegt, wenn ihm ein Wert zugewiesen wurde. Wenn er fehlt oder auf NULL festgelegt ist, erfolgt keine Aktion. |

## <a name="manipulating-variables"></a>Bearbeiten von Variablen
Variablen können folgendermaßen bearbeitet werden:

- Erweitern von Teilzeichenfolgen
- Entfernen von Mustern

### <a name="substring-expansion"></a>Teilzeichenfolgenerweiterung
Eine Variable wird standardmäßig auf ihren vollständigen Wert erweitert. Verwenden Sie die folgende Syntax, um nur eine Teilzeichenfolge des Wert der Variablen zu erweitern.

`%<Variable>:<Offset>:<Length>}`

Wichtige Informationen:

- Der dem Offset-Ausdruck zugewiesene Wert bestimmt das Anfangszeichen der Teilzeichenfolge:

     - Positiv: Das Anfangszeichen der Teilzeichenfolge wird anhand des ersten Zeichens in der Zeichenfolge berechnet.
     - Null: Das Anfangszeichen der Teilzeichenfolge ist das erste Zeichen in der Zeichenfolge.
     - Negativ: Das Anfangszeichen der Teilzeichenfolge wird anhand des letzten Zeichens in der Zeichenfolge berechnet.

- Die Länge der Teilzeichenfolge richtet sich nach dem Ausdruck *Länge*:

     - Nicht angegeben: Durch Auslassen des Längenausdrucks kann die Teilzeichenfolge alle Zeichen zwischen dem Anfangszeichen und dem Ende der Zeichenfolge enthalten.
     - Positiv: Legt die Länge der Teilzeichenfolge ab dem Anfangszeichen nach rechts fest.
     - Negativ: Legt die Länge der Teilzeichenfolge ab dem Anfangszeichen nach links fest.

#### <a name="example"></a>Beispiel:

Das folgende Beispiel basiert auf der folgenden Beispielanforderungs-URL:

https:\//cdn.mydomain.com/folder/marketing/myconsultant/proposal.html

Die folgende Zeichenfolge veranschaulicht verschiedene Methoden zum Bearbeiten von Variablen:

https:\//www%{http_host:3}/mobile/%{request_uri:7:10}/%{request_uri:-5:-8}.htm

Basierend auf der Beispielanforderungs-URL erzeugt die Variablenbearbeitung oben den folgenden Wert:

https:\//www.mydomain.com/mobile/marketing/proposal.htm


### <a name="pattern-removal"></a>Musterentfernung
Text, der einem bestimmten Muster entspricht, kann vom Anfang oder Ende des Werts einer Variablen entfernt werden.

| Syntax | Aktion |
| ------ | ------ |
| %{Variable#Pattern} | Text wird entfernt, wenn sich das angegebene Muster am Anfang des Werts einer Variablen befindet. |
| %{Variable%Pattern} | Text wird entfernt, wenn sich das angegebene Muster am Ende des Werts einer Variablen befindet. |

#### <a name="example"></a>Beispiel:

In diesem Beispielszenario ist die Variable *request_uri* festgelegt auf:

`/800001/myorigin/marketing/product.html?language=en-US`

In der folgenden Tabelle wird veranschaulicht, wie diese Syntax funktioniert.

| Beispielsyntax | Ergebnisse | |
| ------------- | ------- | --- |
| %{request_uri#/800001}/customerorigin | /customerorigin/myorigin/marketing/product.html?language=en-US | Da die Variable mit dem Muster beginnt, wurde sie ersetzt. |
| %{request_uri%html}htm | /800001/myorigin/marketing/product.html?language=en-US | Da die Variable nicht mit dem Muster endet, wurde keine Änderung vorgenommen.|

### <a name="find-and-replace"></a>Suchen und Ersetzen
Die Syntax für Suchen und Ersetzen ist in der folgenden Tabelle beschrieben.

| Syntax | Aktion |
| ------ | ------ |
| %{Variable/Find/Replace} | Suchen und Ersetzen des ersten Vorkommens des angegebenen Musters. |
| %{Variable//Find/Replace} | Suchen und Ersetzen aller Vorkommen des angegebenen Musters. |
| %{Variable^} |Konvertieren des gesamten Werts in Großbuchstaben. |
| %{Variable^Find} | Konvertieren des ersten Vorkommens des angegebenen Musters in Großbuchstaben. |
| %{Variable,} | Konvertieren des gesamten Werts in Kleinbuchstaben. |
| %{Variable,Find} | Konvertieren des ersten Vorkommens des angegebenen Musters in Kleinbuchstaben. |

### <a name="find-and-rewrite"></a>Suchen und erneut generieren
Für eine Abwandlung des Vorgangs „Suchen und Ersetzen“ verwenden Sie den Text, der dem angegebenen Muster entspricht, wenn es neu generiert wird. Die Syntax für Suchen und erneutes Generieren ist in der folgenden Tabelle beschrieben.

| Syntax | Aktion |
| ------ | ------ |
| %{Variable/=Find/Rewrite} | Sucht und kopiert alle Vorkommen des angegebenen Musters und generiert sie erneut. |
| %{Variable/^Find/Rewrite} | Sucht und kopiert alle Vorkommen des angegebenen Musters, wenn es am Beginn der Variablen steht, und generiert sie erneut. |
| %{Variable/$Find/Rewrite} | Sucht und kopiert alle Vorkommen des angegebenen Musters, wenn es am Ende der Variablen steht, und generiert sie erneut. |
| %{Variable/Find} | Suchen und Löschen aller Vorkommen des angegebenen Musters. |

Wichtige Informationen:

- Erweitern Sie Text, der dem angegebenen Muster entspricht, indem Sie ein Dollarzeichen gefolgt von einer ganzen Zahl (z.B. $1) angeben.

- Mehrere Muster können angegeben werden. Die Reihenfolge, in der das Muster angegeben ist, bestimmt die ganze Zahl, die ihm zugewiesen wird. Im folgenden Beispiel entspricht das erste Muster „www.“, das zweite Muster entspricht der Domäne zweiter Ebene, und das dritte Muster entspricht der Domäne der obersten Ebene:

    `%{host/=^www\.([^\.]+)\.([^\.:]+)/cdn.$2.$3:80}`

- Der erneut generierte Wert kann aus einer beliebige Kombination aus Text und diesen Platzhalter bestehen.

    Im vorherigen Beispiel wird der Hostname in `cdn.$2.$3:80` umgeschrieben (z.B. cdn.mydomain.com:80).

- Die Groß-/Kleinschreibung eines Musterplatzhalters (z.B. $1) kann über die folgenden Flags geändert werden:
     - U: Der erweiterte Wert wird großgeschrieben.

         Beispielsyntax:

         `%{host/=^www\.([^\.]+)\.([^\.:]+)/cdn.$U2.$3:80}`

     - L: Der erweiterte Wert wird kleingeschrieben.

         Beispielsyntax:

         `%{host/=^www\.([^\.]+)\.([^\.:]+)/cdn.$L2.$3:80}`

- Vor dem Muster muss ein Operator angegeben werden. Der angegebene Operator bestimmt das Verhalten für die Mustererfassung:

     - `=`: Gibt an, dass alle Vorkommen des angegebenen Musters erfasst und erneut generiert werden müssen.
     - `^`: Gibt an, dass nur Text, der mit dem angegebenen Muster beginnt, erfasst wird.
     - `$`: Gibt an, dass nur Text, der mit dem angegebenen Muster endet, erfasst wird.
 
- Wenn Sie den Wert */Rewrite* weglassen, wird der Text, der dem Muster entspricht, gelöscht.


