---
title: Übereinstimmungsbedingungen der Regel-Engine für Azure CDN Premium von Verizon | Microsoft-Dokumentation
description: Referenzdokumentation zu den Übereinstimmungsbedingungen der Regel-Engine für Azure Content Delivery Network Premium von Verizon.
services: cdn
author: mdgattuso
ms.service: azure-cdn
ms.topic: article
ms.date: 05/31/2019
ms.author: magattus
ms.openlocfilehash: 1660dca34b2f128ef5889145fcdeed0d2523b9bb
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2019
ms.locfileid: "67593198"
---
# <a name="azure-cdn-from-verizon-premium-rules-engine-match-conditions"></a>Übereinstimmungsbedingungen der Regel-Engine für Azure CDN Premium von Verizon

Dieser Artikel enthält ausführliche Beschreibungen der verfügbaren Übereinstimmungsbedingungen für die [Regel-Engine](cdn-verizon-premium-rules-engine.md) des Azure Content Delivery Network (CDN) Premium von Verizon.

Der zweite Teil einer Regel ist die Übereinstimmungsbedingung. Eine Übereinstimmungsbedingung gibt bestimmte Typen von Anforderungen an, für die verschiedene sog. Features ausgeführt werden.

Sie können eine Übereinstimmungsbedingung z.B. für Folgendes verwenden:

- Filtern von Anforderungen für Inhalte an einem bestimmten Standort
- Filtern von Anforderungen, die von einer bestimmten IP-Adresse oder einem bestimmten Land bzw. einer bestimmten Region generiert wurden
- Filtern von Anforderungen nach Headerinformationen

## <a name="always-match-condition"></a>Übereinstimmungsbedingung „Always“

Die Übereinstimmungsbedingung „Always“ wendet einen Standardsatz von Features auf alle Anforderungen an.

NAME | Zweck
-----|--------
[Always](#always) | Wendet einen Standardsatz von Features auf alle Anforderungen an.

## <a name="device-match-condition"></a>Übereinstimmungsbedingung „Device“

Die Übereinstimmungsbedingung „Device“ bestimmt Anforderungen von einem Mobilgerät anhand seiner Eigenschaften.  

NAME | Zweck
-----|--------
[Device](#device) | Bestimmt Anforderungen von einem Mobilgerät anhand seiner Eigenschaften.

## <a name="location-match-conditions"></a>Location-Übereinstimmungsbedingungen

Die Location-Übereinstimmungsbedingungen bestimmen Anforderungen basierend auf dem Standort des Anfordernden.

NAME | Zweck
-----|--------
[AS Number](#as-number) | Bestimmt Anforderungen, die aus einem bestimmten Netzwerk stammen.
[Country](#country) | Bestimmt Anforderungen, die aus den angegebenen Ländern/Regionen stammen.

## <a name="origin-match-conditions"></a>Origin-Übereinstimmungsbedingungen

Die Origin-Übereinstimmungsbedingungen bestimmen Anforderungen, die auf CDN-Speicher oder einen Kundenursprungsserver zeigen.

NAME | Zweck
-----|--------
[CDN Origin](#cdn-origin) | Bestimmt Anforderungen für Inhalte, die in Content Delivery Network-Speicher gespeichert sind.
[Customer Origin](#customer-origin) | Bestimmt Anforderungen von Inhalten, die auf einem spezifischen Kundenursprungsserver gespeichert sind.

## <a name="request-match-conditions"></a>Request-Übereinstimmungsbedingungen

Die Request-Übereinstimmungsbedingungen bestimmen Anforderungen basierend auf ihren Eigenschaften.

NAME | Zweck
-----|--------
[Client IP Address](#client-ip-address) | Bestimmt Anforderungen, die von einer bestimmten IP-Adresse stammen.
[Cookie Parameter](#cookie-parameter) | Überprüft die Cookies, die jeder Anforderung für den angegebenen Wert zugeordnet sind.
[Cookie Parameter Regex](#cookie-parameter-regex) | Überprüft die Cookies, die jeder Anforderung zugeordnet sind, auf den angegebenen regulären Ausdruck.
[Edge Cname](#edge-cname) | Bestimmt Anforderungen, die auf einen bestimmten Edge-CNAME zeigen.
[Referring Domain](#referring-domain) | Bestimmt Anforderungen, auf die von den angegebenen Hostnamen verwiesen wurde.
[Request Header Literal](#request-header-literal) | Bestimmt Anforderungen, die den angegebenen auf einen bestimmten Wert festgelegten Header enthalten.
[Request Header Regex](#request-header-regex) | Bestimmt Anforderungen, die den angegebenen Header enthalten, der auf einen Wert festgelegt ist, der dem angegebenen regulären Ausdruck entspricht.
[Request Header Wildcard](#request-header-wildcard) | Bestimmt Anforderungen, die den angegebenen Header enthalten, der auf einen Wert festgelegt ist, der dem angegebenen Muster entspricht.
[Request Method](#request-method) | Bestimmt Anforderungen anhand ihrer HTTP-Methode.
[Request Scheme](#request-scheme) | Bestimmt Anforderungen anhand ihres HTTP-Protokolls.

## <a name="url-match-conditions"></a>URL-Übereinstimmungsbedingungen

Die URL-Übereinstimmungsbedingungen bestimmen Anforderungen basierend auf ihren URLs.

NAME | Zweck
-----|--------
[URL Path Directory](#url-path-directory) | Bestimmt Anforderungen anhand ihres relativen Pfads.
[URL Path Extension](#url-path-extension) | Bestimmt Anforderungen anhand ihrer Dateinamenerweiterung.
[URL Path Filename](#url-path-filename) | Bestimmt Anforderungen anhand ihres Dateinamens.
[URL Path Literal](#url-path-literal) | Vergleicht den relativen Pfad einer Anforderung mit dem angegebenen Wert.
[URL Path Regex](#url-path-regex) | Vergleicht den relativen Pfad einer Anforderung mit dem angegebenen regulären Ausdruck.
[URL Path Wildcard](#url-path-wildcard) | Vergleicht den relativen Pfad einer Anforderung mit dem angegebenen Muster.
[URL Query Literal](#url-query-literal) | Vergleicht die Abfragezeichenfolge einer Anforderung mit dem angegebenen Wert.
[URL Query Parameter](#url-query-parameter) | Bestimmt Anforderungen, die den angegebenen Abfragezeichenfolgen-Parameter enthalten, der auf einen Wert festgelegt ist, der einem angegebenen Muster entspricht.
[URL Query Regex](#url-query-regex) | Bestimmt Anforderungen, die den angegebenen Abfragezeichenfolgen-Parameter enthalten, der auf einen Wert festgelegt ist, der einem angegebenen regulären Ausdruck entspricht.
[URL Query Wildcard](#url-query-wildcard) | Vergleicht den angegebenen Wert mit der Abfragezeichenfolge der Anforderung.

## <a name="reference-for-rules-engine-match-conditions"></a>Referenz für Übereinstimmungsbedingungen der Regel-Engine

---

### <a name="always"></a>Always

Die Übereinstimmungsbedingung „Always“ wendet einen Standardsatz von Features auf alle Anforderungen an.

[Nach oben](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="as-number"></a>AS Number

Das AS Number-Netzwerk wird durch seine autonome Systemnummer (ASN) definiert. 

Mit der Option **Entspricht**/**Entspricht nicht** werden die Bedingungen bestimmt, unter denen die Übereinstimmungsbedingung „AS Number“ erfüllt ist:

- **Entspricht:** Für diese Option ist es erforderlich, dass die ASN des Clientnetzwerks mit einer der angegebenen ASNs übereinstimmt. 
- **Entspricht nicht:** Für diese Option ist es erforderlich, dass die ASN des Clientnetzwerks nicht mit einer der angegebenen ASNs übereinstimmt.

Wichtige Informationen:

- Wenn Sie mehrere ASNs angeben möchten, trennen Sie die Nummern durch ein einzelnes Leerzeichen voneinander. Die Angabe „64514 64515“ beispielsweise führt zur Übereinstimmung mit Anforderungen von 64514 oder 64515.
- Bestimmte Anforderungen geben möglicherweise keine gültige ASN zurück. Ein Fragezeichen (?) kennzeichnet Anforderungen, für die keine gültige ASN ermittelt werden konnte.
- Geben Sie die gesamte ASN für das gewünschte Netzwerk an. Für Teilwerte gibt es keine Übereinstimmung.
- Aufgrund der Art und Weise, in der Cacheeinstellungen nachverfolgt werden, ist diese Übereinstimmungsbedingung mit den folgenden Features nicht kompatibel:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Nach oben](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="cdn-origin"></a>CDN Origin

Die Übereinstimmungsbedingung „CDN Origin“ ist erfüllt, wenn beide folgenden Bedingungen erfüllt sind:

- Der Inhalt wurde aus dem CDN-Speicher angefordert.
- Der Anforderungs-URI verwendet den Typ des Inhaltszugriffspunkts (z.B. /000001), der in dieser Übereinstimmungsbedingung definiert ist:
  - CDN-URL: Der Anforderungs-URI muss den ausgewählten Inhaltszugriffspunkt enthalten.
  - Edge-CNAME-URL: Die entsprechende Edge-CNAME-Konfiguration muss auf den ausgewählten Inhaltszugriffspunkt zeigen.
  
Wichtige Informationen:

- Der Inhaltszugriffspunkt bestimmt den Dienst, der den angeforderten Inhalt bereitstellen soll.
- Verwenden Sie keine AND IF-Anweisung, um bestimmte Übereinstimmungsbedingungen zu kombinieren. Durch Kombination einer CDN Origin-Übereinstimmungsbedingung mit einer Customer Origin-Übereinstimmungsbedingung würde beispielsweise ein Übereinstimmungsmuster entstehen, für das nie eine Übereinstimmung gefunden werden kann. Aus diesem Grund können zwei CDN Origin-Übereinstimmungsbedingungen nicht mit einer AND IF-Anweisung kombiniert werden.

[Nach oben](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="client-ip-address"></a>Client IP Address

Mit der Option **Entspricht**/**Entspricht nicht** werden die Bedingungen bestimmt, unter denen die Übereinstimmungsbedingung „Client IP Address“ erfüllt ist:

- **Entspricht:** Für diese Option ist es erforderlich, dass die IP-Adresse des Clients mit einer der angegebenen IP-Adressen übereinstimmt. 
- **Entspricht nicht:** Für diese Option ist es erforderlich, dass die IP-Adresse des Clients nicht mit einer der angegebenen IP-Adressen übereinstimmt. 

Wichtige Informationen:

- Verwenden Sie die CIDR-Notation.
- Wenn Sie mehrere IP-Adressen und/oder IP-Adressblöcke angeben, trennen Sie diese mit einem einzelnen Leerzeichen. Beispiel:
  - **IPv4-Beispiel:** Die Angabe „1.2.3.4 10.20.30.40“ führt zur Übereinstimmung mit eingehenden Anforderungen von der Adresse 1.2.3.4 oder 10.20.30.40.
  - **IPv6-Beispiel:** Die Angabe „1:2:3:4:5:6:7:8 10:20:30:40:50:60:70:80“ führt zur Übereinstimmung mit eingehenden Anforderungen von der Adresse 1:2:3:4:5:6:7:8 oder 10:20:30:40:50:60:70:80.
- Die Syntax für einen IP-Adressblock besteht aus der IP-Basisadresse, gefolgt von einem Schrägstrich und der Präfixgröße. Beispiel:
  - **IPv4-Beispiel:** Die Angabe „5.5.5.64/26“ führt zur Übereinstimmung mit eingehenden Anforderungen von den Adressen 5.5.5.64 bis 5.5.5.127.
  - **IPv6-Beispiel:** Die Angabe „1:2:3:/48“ führt zur Übereinstimmung mit eingehenden Anforderungen von den Adressen 1:2:3:0:0:0:0:0 bis 1:2:3:ffff:ffff:ffff:ffff:ffff.
- Aufgrund der Art und Weise, in der Cacheeinstellungen nachverfolgt werden, ist diese Übereinstimmungsbedingung mit den folgenden Features nicht kompatibel:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Nach oben](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="cookie-parameter"></a>Cookie Parameter

Die Option **Entspricht**/**Entspricht nicht** bestimmt die Bedingungen, unter denen die Übereinstimmungsbedingung „Cookie Parameter“ erfüllt ist.

- **Entspricht:** Für diese Option ist es erforderlich, dass eine Anforderung das angegebene Cookie mit einem Wert enthält, der mindestens einem der in dieser Übereinstimmungsbedingung definierten Werte entspricht.
- **Entspricht nicht:** Für diese Option ist es erforderlich, dass die Anforderung eines der folgenden Kriterien erfüllt:
  - Die Anforderung enthält das angegebene Cookie nicht.
  - Die Anforderung enthält das angegebene Cookie, aber der Wert entspricht keinem der Werte, die in dieser Übereinstimmungsbedingung definiert sind.
  
Wichtige Informationen:

- Cookiename:
  - Da Platzhalterwerte, z.B. Sternchen (*), beim Angeben eines Cookienamens nicht unterstützt werden, können nur exakte Übereinstimmungen von Cookienamen für Vergleiche herangezogen werden.
  - Für jede Instanz dieser Übereinstimmungsbedingung kann nur ein einziger Cookiename angegeben werden.
  - Beim Vergleichen von Cookienamen wird die Groß-/Kleinschreibung nicht beachtet.
- Cookiewert:
  - Wenn Sie mehrere Cookiewerte angeben möchten, trennen Sie diese durch ein einzelnes Leerzeichen voneinander.
  - Für einen Cookiewert können [Platzhalterwerte](cdn-verizon-premium-rules-engine-reference.md#wildcard-values) verwendet werden.
  - Wenn kein Platzhalterwert angegeben wurde, ist diese Übereinstimmungsbedingung nur bei einer exakten Übereinstimmung erfüllt. Durch die Angabe von „Wert“ wird eine Übereinstimmung mit „Wert“ erzielt, aber nicht mit „Wert1“ oder „Wert2“.
  - Verwenden Sie die Option **Groß-/Kleinschreibung ignorieren**, um zu steuern, ob beim Vergleich des Cookiewerts einer Anforderung die Groß-/Kleinschreibung berücksichtigt wird.
- Aufgrund der Art und Weise, in der Cacheeinstellungen nachverfolgt werden, ist diese Übereinstimmungsbedingung mit den folgenden Features nicht kompatibel:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Nach oben](#reference-for-rules-engine-match-conditions)
</br>

---

### <a name="cookie-parameter-regex"></a>Cookie Parameter Regex

Die Übereinstimmungsbedingung „Cookie Parameter Regex“ definiert einen Cookienamen und einen Cookiewert. Sie können [reguläre Ausdrücke](cdn-verizon-premium-rules-engine-reference.md#regular-expressions) verwenden, um den gewünschten Cookiewert zu definieren.

Die Option **Entspricht**/**Entspricht nicht** bestimmt die Bedingungen, unter denen die Übereinstimmungsbedingung „Cookie Parameter Regex“ erfüllt ist.

- **Entspricht:** Für diese Option ist es erforderlich, dass Anforderungen das angegebene Cookie mit einem Wert enthalten, der dem angegebenen regulären Ausdruck entspricht.
- **Entspricht nicht:** Für diese Option ist es erforderlich, dass die Anforderung eines der folgenden Kriterien erfüllt:
  - Die Anforderung enthält das angegebene Cookie nicht.
  - Die Anforderung enthält das angegebene Cookie, aber der Wert entspricht nicht dem angegebenen regulären Ausdruck.
  
Wichtige Informationen:

- Cookiename:
  - Da reguläre Ausdrücke und Platzhalterwerte, z.B. Sternchen (*), beim Angeben eines Cookienamens nicht unterstützt werden, können nur exakte Übereinstimmungen von Cookienamen für Vergleiche herangezogen werden.
  - Für jede Instanz dieser Übereinstimmungsbedingung kann nur ein einziger Cookiename angegeben werden.
  - Beim Vergleichen von Cookienamen wird die Groß-/Kleinschreibung nicht beachtet.
- Cookiewert:
  - In Cookiewerten können reguläre Ausdrücke verwendet werden.
  - Verwenden Sie die Option **Groß-/Kleinschreibung ignorieren**, um zu steuern, ob beim Vergleich des Cookiewerts einer Anforderung die Groß-/Kleinschreibung berücksichtigt wird.
- Aufgrund der Art und Weise, in der Cacheeinstellungen nachverfolgt werden, ist diese Übereinstimmungsbedingung mit den folgenden Features nicht kompatibel:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Nach oben](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="country"></a>Country

Sie können ein Land anhand seines Ländercodes angeben. 

Die Option **Entspricht**/**Entspricht nicht** bestimmt die Bedingungen, unter denen die Übereinstimmungsbedingung „Country“ erfüllt ist:

- **Entspricht:** Für diese Option ist es erforderlich, dass die Anforderung die angegebenen Ländercodewerte enthält. 
- **Entspricht nicht:** Für diese Option ist es erforderlich, dass die Anforderung die angegebenen Ländercodewerte nicht enthält.

Wichtige Informationen:

- Wenn Sie mehrere Ländercodes angeben möchten, trennen Sie diese durch ein einzelnes Leerzeichen voneinander.
- Bei der Angabe von Ländercodes werden keine Platzhalterzeichen unterstützt.
- Die Ländercodes „EU“ und „AP“ umfassen nicht alle IP-Adressen in diesen Regionen.
- Bestimmte Anforderungen geben möglicherweise keinen gültigen Ländercode zurück. Ein Fragezeichen (?) kennzeichnet Anforderungen, für die kein gültiger Ländercode ermittelt werden konnte.
- Bei Ländercodes wird die Groß-/Kleinschreibung berücksichtigt.
- Aufgrund der Art und Weise, in der Cacheeinstellungen nachverfolgt werden, ist diese Übereinstimmungsbedingung mit den folgenden Features nicht kompatibel:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

#### <a name="implementing-country-filtering-by-using-the-rules-engine"></a>Implementieren des Länderfilters mit der Regel-Engine

Mit dieser Übereinstimmungsbedingung können Sie basierend auf dem Ursprungsstandort einer Anforderung eine Vielzahl von Anpassungen vornehmen. Das Verhalten des Features für die Länderfilterung kann beispielsweise mit der folgenden Konfiguration repliziert werden:

- URL Path Wildcard-Übereinstimmung: Legen Sie die [Übereinstimmungsbedingung „URL Path Wildcard“](#url-path-wildcard) auf das zu schützende Verzeichnis fest. 
    Fügen Sie am Ende des relativen Pfads ein Sternchen an, um sicherzustellen, dass der Zugriff auf alle untergeordneten Elemente durch diese Regel eingeschränkt wird.

- Country-Übereinstimmung: Legen Sie die Übereinstimmungsbedingung „Country“ auf die gewünschte Gruppe von Ländern fest.
  - Zulassen: Legen Sie die Übereinstimmungsbedingung „Country“ auf **Entspricht nicht** fest, um nur für die angegebenen Länder den Zugriff auf die gespeicherten Inhalte an dem Standort zuzulassen, der mit der Übereinstimmungsbedingung „URL Path Wildcard“ definiert wurde.
  - Blockieren: Legen Sie die Übereinstimmungsbedingung „Country“ auf **Entspricht** fest, um für die angegebenen Länder den Zugriff auf die gespeicherten Inhalte an dem Standort zu blockieren, der mit der Übereinstimmungsbedingung „URL Path Wildcard“ definiert wurde.

- Feature „Deny Access (403)“: Aktivieren Sie das [Feature „Deny Access (403)“](cdn-verizon-premium-rules-engine-reference-features.md#deny-access-403), um den Zulassen- bzw. Blockieren-Teil des Features für die Länderfilterung zu replizieren.

[Nach oben](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="customer-origin"></a>Customer Origin

Wichtige Informationen:

- Die Übereinstimmungsbedingung „Customer Origin“ wird unabhängig davon erfüllt, ob Inhalt über eine CDN-URL oder eine Edge-CNAME-URL angefordert wird, die auf den ausgewählten Kundenursprung verweist.
- Eine Kundenursprungskonfiguration, auf die von einer Regel verwiesen wird, kann von der Seite „Customer Origin“ nicht gelöscht werden. Bevor Sie versuchen, eine Kundenursprungskonfiguration zu löschen, stellen Sie sicher, dass die folgenden Konfigurationen nicht darauf verweisen:
  - Eine Customer Origin-Übereinstimmungsbedingung
  - Eine Edge-CNAME-Konfiguration
- Verwenden Sie keine AND IF-Anweisung, um bestimmte Übereinstimmungsbedingungen zu kombinieren. Durch Kombination einer Customer Origin-Übereinstimmungsbedingung mit einer CDN Origin-Übereinstimmungsbedingung würde beispielsweise ein Übereinstimmungsmuster entstehen, für das nie eine Übereinstimmung gefunden werden kann. Aus diesem Grund können zwei Customer Origin-Übereinstimmungsbedingungen nicht mit einer AND IF-Anweisung kombiniert werden.

[Nach oben](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="device"></a>Gerät

Die Übereinstimmungsbedingung „Device“ bestimmt Anforderungen von einem Mobilgerät anhand seiner Eigenschaften. Die Mobilgeräterkennung wird mithilfe von [WURFL](http://wurfl.sourceforge.net/) durchgeführt. 

Die Option **Entspricht**/**Entspricht nicht** bestimmt die Bedingungen, unter denen die Übereinstimmungsbedingung „Device“ erfüllt ist:

- **Entspricht:** Für diese Option ist es erforderlich, dass das Gerät des Anforderers mit dem angegebenen Wert übereinstimmt. 
- **Entspricht nicht:** Für diese Option ist es erforderlich, dass das Gerät des Anforderers nicht mit dem angegebenen Wert übereinstimmt.

Wichtige Informationen:

- Verwenden Sie die Option **Groß-/Kleinschreibung ignorieren**, um festzulegen, ob für den angegebenen Wert die Groß-/Kleinschreibung berücksichtigt wird.
- Aufgrund der Art und Weise, in der Cacheeinstellungen nachverfolgt werden, ist diese Übereinstimmungsbedingung mit den folgenden Features nicht kompatibel:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

#### <a name="string-type"></a>Zeichenfolgentyp

Eine WURFL-Funktion akzeptiert normalerweise eine beliebige Kombination von Zahlen, Buchstaben und Zeichen. Aufgrund der hohen Flexibilität dieser Funktion müssen Sie auswählen, wie der Wert, der dieser Übereinstimmungsbedingung zugeordnet ist, interpretiert wird. In der folgenden Tabelle sind die verfügbaren Optionen beschrieben:

type     | BESCHREIBUNG
---------|------------
Literal  | Wählen Sie diese Option, um zu verhindern, dass die meisten Zeichen eine spezielle Bedeutung erhalten, indem der [Literalwert](cdn-verizon-premium-rules-engine-reference.md#literal-values) verwendet wird.
Platzhalter | Wählen Sie diese Option, um alle [Platzhalterzeichen]([Platzhalterwerte](cdn-verizon-premium-rules-engine-reference.md#wildcard-values) zu nutzen.
RegEx    | Wählen Sie diese Option, um [reguläre Ausdrücke](cdn-verizon-premium-rules-engine-reference.md#regular-expressions) zu verwenden. Reguläre Ausdrücke sind hilfreich beim Definieren eines Musters für Zeichen.

#### <a name="wurfl-capabilities"></a>WURFL-Funktionen

Eine WURFL-Funktion bezieht sich auf eine Kategorie zur Beschreibung von mobilen Geräten. Mit der ausgewählten Funktion wird der Typ der Mobilgerätbeschreibung ermittelt, der zum Identifizieren von Anforderungen verwendet wird.

In der folgenden Tabelle sind WURFL-Funktionen und die zugehörigen Variablen für die Regel-Engine aufgeführt.

> [!NOTE]
> Die folgenden Variablen werden für die Features **Modify Client Request Header** und **Modify Client Response Header** unterstützt.

Funktion | Variable | BESCHREIBUNG | Beispielwerte
-----------|----------|-------------|----------------
Brand Name | %{wurfl_cap_brand_name} | Eine Zeichenfolge, die den Markennamen des Geräts angibt. | Samsung
Device OS | %{wurfl_cap_device_os} | Eine Zeichenfolge, die das auf dem Gerät installierte Betriebssystem angibt. | IOS
Device OS Version | %{wurfl_cap_device_os_version} | Eine Zeichenfolge, die die Versionsnummer des auf dem Gerät installierten Betriebssystems angibt. | 1.0.1
Dual Orientation | %{wurfl_cap_dual_orientation} | Ein boolescher Wert, der angibt, ob das Gerät die duale Ausrichtung unterstützt. | true
HTML Preferred DTD | %{wurfl_cap_html_preferred_dtd} | Eine Zeichenfolge, die die bevorzugte Dokumenttypdefinition (DTD) des mobilen Geräts für den HTML-Inhalt angibt. | none<br/>xhtml_basic<br/>html5
Image Inlining | %{wurfl_cap_image_inlining} | Ein boolescher Wert, der angibt, ob das Gerät Base64-codierte Images unterstützt. | false
Is Android | %{wurfl_vcap_is_android} | Ein boolescher Wert, der angibt, ob für das Gerät das Android-Betriebssystem verwendet wird. | true
Is IOS | %{wurfl_vcap_is_ios} | Ein boolescher Wert, der angibt, ob für das Gerät das iOS-Betriebssystem verwendet wird. | false
Is Smart TV | %{wurfl_cap_is_smarttv} | Ein boolescher Wert, der angibt, ob es sich um ein Smart TV-Gerät handelt. | false
Is Smartphone | %{wurfl_vcap_is_smartphone} | Ein boolescher Wert, der angibt, ob es sich beim Gerät um ein Smartphone handelt. | true
Is Tablet | %{wurfl_cap_is_tablet} | Ein boolescher Wert, der angibt, ob es sich beim Gerät um ein Tablet handelt. Diese Beschreibung ist unabhängig von Betriebssystem. | true
Is Wireless Device | %{wurfl_cap_is_wireless_device} | Ein boolescher Wert, der angibt, ob das Gerät als drahtloses Gerät betrachtet wird. | true
Marketing Name | %{wurfl_cap_marketing_name} | Eine Zeichenfolge, die den Marketingnamen des Geräts angibt. | BlackBerry 8100 Pearl
Mobile Browser | %{wurfl_cap_mobile_browser} | Eine Zeichenfolge, die den Browser angibt, der zum Anfordern des Inhalts vom Gerät verwendet wird. | Chrome
Mobile Browser Version | %{wurfl_cap_mobile_browser_version} | Eine Zeichenfolge, die die Version des Browsers angibt, der zum Anfordern des Inhalts vom Gerät verwendet wird. | 31
Model Name | %{wurfl_cap_model_name} | Eine Zeichenfolge, die den Modellnamen des Geräts angibt. | s3
Progressive Download | %{wurfl_cap_progressive_download} | Ein boolescher Wert, der angibt, ob das Gerät die Wiedergabe von Audio- und Videodaten während des Herunterladens unterstützt. | true
Release Date | %{wurfl_cap_release_date} | Eine Zeichenfolge, die das Jahr und den Monat angibt, in dem das Gerät der WURFL-Datenbank hinzugefügt wurde.<br/><br/>Format: `yyyy_mm` | 2013_december
Resolution Height | %{wurfl_cap_resolution_height} | Eine ganze Zahl, die für das Gerät die Höhe in Pixel angibt. | 768
Resolution Width | %{wurfl_cap_resolution_width} | Eine ganze Zahl, die für das Gerät die Breite in Pixel angibt. | 1024

[Nach oben](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="edge-cname"></a>Edge Cname

Wichtige Informationen:

- Die Liste mit den verfügbaren Edge-CNAMEs ist auf die Edge-CNAMEs beschränkt, die auf der Edge-CNAME-Seite der Plattform konfiguriert wurden, auf der die Regel-Engine konfiguriert wird.
- Bevor Sie versuchen, eine Edge-CNAME-Konfiguration zu löschen, stellen Sie sicher, dass keine Edge Cname-Übereinstimmungsbedingung darauf verweist. Edge CNAME-Konfigurationen, die in einer Regel definiert wurden, können nicht von der Edge-CNAME-Seite gelöscht werden.
- Verwenden Sie keine AND IF-Anweisung, um bestimmte Übereinstimmungsbedingungen zu kombinieren. Durch Kombination einer Edge Cname-Übereinstimmungsbedingung mit einer Customer Origin-Übereinstimmungsbedingung würde beispielsweise ein Übereinstimmungsmuster entstehen, für das nie eine Übereinstimmung gefunden werden kann. Aus diesem Grund können zwei Edge Cname-Übereinstimmungsbedingungen nicht mit einer AND IF-Anweisung kombiniert werden.
- Aufgrund der Art und Weise, in der Cacheeinstellungen nachverfolgt werden, ist diese Übereinstimmungsbedingung mit den folgenden Features nicht kompatibel:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Nach oben](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="referring-domain"></a>Referring Domain

Der Hostname des verweisenden Elements, über das der Inhalt angefordert wurde, bestimmt, ob die Referring Domain-Bedingung erfüllt wird.

Die Option **Entspricht**/**Entspricht nicht** bestimmt die Bedingungen, unter denen die Übereinstimmungsbedingung „Referring Domain“ erfüllt ist:

- **Entspricht:** Für diese Option ist es erforderlich, dass der Name des verweisenden Hosts mit den angegebenen Werten übereinstimmt. 
- **Entspricht nicht:** Für diese Option ist es erforderlich, dass der Name des verweisenden Hosts nicht mit dem angegebenen Wert übereinstimmt.

Wichtige Informationen:

- Wenn Sie mehrere Hostnamen angeben möchten, trennen Sie die Namen durch ein einzelnes Leerzeichen voneinander.
- Für diese Übereinstimmungsbedingung werden [Platzhalterwerte](cdn-verizon-premium-rules-engine-reference.md#wildcard-values) unterstützt.
- Wenn der angegebene Wert kein Sternchen enthält, muss er exakt mit dem Namen des verweisenden Hosts übereinstimmen. Bei der Angabe von „mydomain.com“ würde beispielsweise keine Übereinstimmung mit „www.mydomain.com“ erzielt.
- Verwenden Sie die Option **Groß-/Kleinschreibung ignorieren**, um zu steuern, ob ein Vergleich unter Berücksichtigung der Groß-/Kleinschreibung durchgeführt wird.
- Aufgrund der Art und Weise, in der Cacheeinstellungen nachverfolgt werden, ist diese Übereinstimmungsbedingung mit den folgenden Features nicht kompatibel:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Nach oben](#reference-for-rules-engine-match-conditions)

</br>

---  

### <a name="request-header-literal"></a>Request Header Literal

Die Option **Entspricht**/**Entspricht nicht** bestimmt die Bedingungen, unter denen die Übereinstimmungsbedingung „Request Header Literal“ erfüllt ist.

- **Entspricht:** Für diese Option ist es erforderlich, dass die Anforderung den angegebenen Header enthält. Der Wert der Anforderung muss dem Wert entsprechen, der in dieser Übereinstimmungsbedingung definiert ist.
- **Entspricht nicht:** Für diese Option ist es erforderlich, dass die Anforderung eines der folgenden Kriterien erfüllt:
  - Die Anforderung enthält den angegebenen Header nicht.
  - Die Anforderung enthält den angegebenen Header, aber der Wert entspricht nicht dem Wert, der in dieser Übereinstimmungsbedingung definiert ist.
  
Wichtige Informationen:

- Beim Vergleichen von Headernamen wird die Groß-/Kleinschreibung nie beachtet. Verwenden Sie die Option **Groß-/Kleinschreibung ignorieren**, um zu steuern, ob bei Vergleichen von Headerwerten die Groß-/Kleinschreibung berücksichtigt wird.
- Aufgrund der Art und Weise, in der Cacheeinstellungen nachverfolgt werden, ist diese Übereinstimmungsbedingung mit den folgenden Features nicht kompatibel:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Nach oben](#reference-for-rules-engine-match-conditions)

</br>

---  

### <a name="request-header-regex"></a>Request Header Regex

Die Option **Entspricht**/**Entspricht nicht** bestimmt die Bedingungen, unter denen die Übereinstimmungsbedingung „Request Header Regex“ erfüllt ist.

- **Entspricht:** Für diese Option ist es erforderlich, dass die Anforderung den angegebenen Header enthält. Der Wert der Anforderung muss dem Muster entsprechen, das im angegebenen [regulären Ausdruck](cdn-verizon-premium-rules-engine-reference.md#regular-expressions) definiert ist.
- **Entspricht nicht:** Für diese Option ist es erforderlich, dass die Anforderung eines der folgenden Kriterien erfüllt:
  - Die Anforderung enthält den angegebenen Header nicht.
  - Die Anforderung enthält den angegebenen Header, aber der Wert entspricht nicht dem angegebenen regulären Ausdruck.

Wichtige Informationen:

- Headername:
  - Beim Vergleichen von Headernamen wird die Groß-/Kleinschreibung nicht beachtet.
  - Ersetzen Sie Leerzeichen im Headernamen durch „%20“.
- Headerwert:
  - In Headerwerten können reguläre Ausdrücke verwendet werden.
  - Verwenden Sie die Option **Groß-/Kleinschreibung ignorieren**, um zu steuern, ob bei Vergleichen von Headerwerten die Groß-/Kleinschreibung berücksichtigt wird.
  - Die Übereinstimmungsbedingung ist nur erfüllt, wenn ein Headerwert mindestens einem der angegebenen Muster genau entspricht.
- Aufgrund der Art und Weise, in der Cacheeinstellungen nachverfolgt werden, ist diese Übereinstimmungsbedingung mit den folgenden Features nicht kompatibel:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Nach oben](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="request-header-wildcard"></a>Request Header Wildcard

Die Option **Entspricht**/**Entspricht nicht** bestimmt die Bedingungen, unter denen die Übereinstimmungsbedingung „Request Header Wildcard“ erfüllt ist.

- **Entspricht:** Für diese Option ist es erforderlich, dass die Anforderung den angegebenen Header enthält. Der Wert der Anforderung muss mindestens einem der Werte entsprechen, die in dieser Übereinstimmungsbedingung definiert sind.
- **Entspricht nicht:** Für diese Option ist es erforderlich, dass die Anforderung eines der folgenden Kriterien erfüllt:
  - Die Anforderung enthält den angegebenen Header nicht.
  - Die Anforderung enthält den angegebenen Header, aber der Wert entspricht keinem der angegebenen Werte.
  
Wichtige Informationen:

- Headername:
  - Beim Vergleichen von Headernamen wird die Groß-/Kleinschreibung nicht beachtet.
  - Leerzeichen im Headernamen sollten durch „%20“ ersetzt werden. Sie können diesen Wert auch verwenden, um Leerzeichen in einem Headerwert anzugeben.
- Headerwert:
  - Für einen Headerwert können [Platzhalterzeichen](cdn-verizon-premium-rules-engine-reference.md#wildcard-values) verwendet werden.
  - Verwenden Sie die Option **Groß-/Kleinschreibung ignorieren**, um zu steuern, ob bei Vergleichen von Headerwerten die Groß-/Kleinschreibung berücksichtigt wird.
  - Diese Übereinstimmungsbedingung ist erfüllt, wenn ein Headerwert mindestens einem der angegebenen Muster genau entspricht.
  - Wenn Sie mehrere Werte angeben möchten, trennen Sie diese durch ein einzelnes Leerzeichen voneinander.
- Aufgrund der Art und Weise, in der Cacheeinstellungen nachverfolgt werden, ist diese Übereinstimmungsbedingung mit den folgenden Features nicht kompatibel:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Nach oben](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="request-method"></a>Request Method

Die Übereinstimmungsbedingung „Request Method“ wird nur erfüllt, wenn Objekte über die ausgewählte Anforderungsmethode angefordert werden. Folgende Anforderungsmethoden sind verfügbar:

- GET
- HEAD
- POST
- OPTIONS
- PUT
- DELETE
- TRACE
- CONNECT

Wichtige Informationen:

- Standardmäßig kann nur die GET-Anforderungsmethode zwischengespeicherten Inhalt im Netzwerk generieren. Alle anderen Anforderungsmethoden werden per Proxy durch das Netzwerk gesendet.
- Aufgrund der Art und Weise, in der Cacheeinstellungen nachverfolgt werden, ist diese Übereinstimmungsbedingung mit den folgenden Features nicht kompatibel:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Nach oben](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="request-scheme"></a>Request Scheme

Die Übereinstimmungsbedingung „Request Scheme“ ist nur erfüllt, wenn Objekte über das ausgewählte Protokoll angefordert werden. Die verfügbaren Protokolle lauten:

- HTTP
- HTTPS

Wichtige Informationen:

- Aufgrund der Art und Weise, in der Cacheeinstellungen nachverfolgt werden, ist diese Übereinstimmungsbedingung mit den folgenden Features nicht kompatibel:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Nach oben](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="url-path-directory"></a>URL Path Directory

Identifiziert eine Anforderung anhand ihres relativen Pfads, also ohne den Dateinamen des angeforderten Objekts.

Die Option **Entspricht**/**Entspricht nicht** bestimmt die Bedingungen, unter denen die Übereinstimmungsbedingung „URL Path Directory“ erfüllt ist.

- **Entspricht:** Für diese Option ist es erforderlich, dass die Anforderung einen relativen URL-Pfad (ohne Dateiname) enthält, der mit dem angegebenen URL-Muster übereinstimmt.
- **Entspricht nicht:** Für diese Option ist es erforderlich, dass die Anforderung einen relativen URL-Pfad (ohne Dateiname) enthält, der nicht mit dem angegebenen URL-Muster übereinstimmt.

Wichtige Informationen:

- Verwenden Sie die Option **Relativ zu**, um anzugeben, ob der URL-Vergleich vor oder nach dem Inhaltszugriffspunkt beginnt. Der Inhaltszugriffspunkt ist der Teil des Pfads zwischen dem Verizon CDN-Hostnamen und dem relativen Pfad zum angeforderten Objekt (z.B. /800001/CustomerOrigin). Hiermit wird ein Standort anhand des Servertyps (z.B. CDN oder Kundenursprung) und Ihrer Kundenkontonummer identifiziert.

   Die folgenden Werte sind für die Option **Relativ zu** verfügbar:
  - **Stamm:** Gibt an, dass der URL-Vergleichspunkt direkt nach dem CDN-Hostnamen beginnt. 

  Beispiel: http:\//wpc.0001.&lt;Domäne&gt;/**800001/myorigin/myfolder**/index.htm

  - **Ursprung:** Gibt an, dass der URL-Vergleichspunkt nach dem Inhaltszugriffspunkt (z.B. /000001 oder /800001/myorigin) beginnt. Da der CNAME „\*.azureedge.net“ standardmäßig relativ zum Ursprungsverzeichnis des Verizon CDN-Hostnamens erstellt wird, sollten Azure CDN-Benutzer den Wert **Ursprung** verwenden. 

  Beispiel: https:\//&lt;Endpunkt&gt;.azureedge.net/**myfolder**/index.htm 

  Diese URL verweist auf den folgenden Verizon CDN-Hostnamen: http:\//wpc.0001.&lt;Domäne&gt;/800001/myorigin/**myfolder**/index.htm

- Vor dem URL-Vergleich wird eine Edge-CNAME-URL in eine CDN-URL umgeschrieben.

    Beispielsweise verweisen die beiden folgenden URLs auf dasselbe Objekt und verfügen daher über den gleichen URL-Pfad.
  - CDN-URL: http:\//wpc.0001.&lt;Domäne&gt;/800001/CustomerOrigin/path/asset.htm
    
  - Edge-CNAME-URL: http:\//&lt;Endpunkt&gt;.azureedge.net/path/asset.htm
    
    Weitere Informationen:
  - Benutzerdefinierte Domäne: https:\//my.domain.com/path/asset.htm
    
    - URL-Pfad (relativ zum Stamm): /800001/CustomerOrigin/path/
    
    - URL-Pfad (relativ zum Ursprung): /path/

- Der Teil der URL, der für den URL-Vergleich verwendet wird, endet direkt vor dem Dateinamen des angeforderten Objekts. Bei dieser Art von Pfad ist ein nachgestellter Schrägstrich das letzte Zeichen.

- Ersetzen Sie alle Leerstellen im URL-Pfadmuster durch „%20“.

- Jedes URL-Pfadmuster kann ein oder mehrere Sternchen (*) enthalten, wobei jedes Sternchen für ein Zeichen oder eine Folge von Zeichen steht.

- Geben Sie mehrere URL-Pfade im Muster an, indem Sie als Trennzeichen jeweils ein einzelnes Leerzeichen verwenden.

    Beispiel: */sales/ */marketing/

- Beim Angeben eines URL-Pfads können [Platzhalterwerte](cdn-verizon-premium-rules-engine-reference.md#wildcard-values) verwendet werden.

- Verwenden Sie die Option **Groß-/Kleinschreibung ignorieren**, um zu steuern, ob ein Vergleich unter Berücksichtigung der Groß-/Kleinschreibung durchgeführt wird.

[Nach oben](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="url-path-extension"></a>URL Path Extension

Identifiziert Anforderungen anhand der Dateierweiterung des angeforderten Objekts.

Die Option **Entspricht**/**Entspricht nicht** bestimmt die Bedingungen, unter denen die Übereinstimmungsbedingung „URL Path Extension“ erfüllt ist.

- **Entspricht:** Für diese Option ist es erforderlich, dass die URL der Anforderung eine Dateierweiterung enthält, die genau mit dem angegebenen Muster überstimmt.

   Beispiel: Wenn Sie „htm“ angeben, ergibt sich eine Übereinstimmung für „htm“-Objekte, aber nicht für „html“-Objekte.  

- **Entspricht nicht:** Für diese Option ist es erforderlich, dass die URL der Anforderung eine Dateierweiterung enthält, die nicht mit dem angegebenen Muster übereinstimmt.

Wichtige Informationen:

- Geben Sie die abzugleichenden Dateierweiterungen im Feld **Wert** ein. Lassen Sie den vorangestellten Punkt weg. Verwenden Sie also „htm“ anstelle von „.htm“.

- Verwenden Sie die Option **Groß-/Kleinschreibung ignorieren**, um zu steuern, ob ein Vergleich unter Berücksichtigung der Groß-/Kleinschreibung durchgeführt wird.

- Geben Sie mehrere Dateierweiterungen an, indem Sie als Trennzeichen jeweils eine einzelne Leerstelle verwenden. 

    Beispiel: htm html

- Wenn Sie „htm“ angeben, ergeben sich beispielsweise Übereinstimmungen für „htm“-Objekte, aber nicht für „html“-Objekte.

#### <a name="sample-scenario"></a>Beispielszenario

Bei der folgenden Beispielkonfiguration wird vorausgesetzt, dass diese Übereinstimmungsbedingung erfüllt ist, wenn sich für eine Anforderung eine Übereinstimmung mit einer der angegebenen Erweiterungen ergibt.

Wertangabe: asp aspx php html

Diese Übereinstimmungsbedingung ist erfüllt, wenn URLs gefunden werden, die auf die folgenden Erweiterungen enden:

- .asp
- .aspx
- .php
- .html

[Nach oben](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="url-path-filename"></a>URL Path Filename

Identifiziert Anforderungen anhand des Dateinamens des angeforderten Objekts. Bei dieser Übereinstimmungsbedingung besteht ein Dateiname aus dem Namen des angeforderten Objekts, einem Punkt und der Dateierweiterung (z.B. „index.html“).

Die Option **Entspricht**/**Entspricht nicht** bestimmt die Bedingungen, unter denen die Übereinstimmungsbedingung „URL Path Filename“ erfüllt ist.

- **Entspricht:** Für diese Option ist es erforderlich, dass die Anforderung im URL-Pfad einen Dateinamen enthält, der mit dem angegebenen Muster übereinstimmt.
- **Entspricht nicht:** Für diese Option ist es erforderlich, dass die Anforderung im URL-Pfad einen Dateinamen enthält, der nicht mit dem angegebenen Muster übereinstimmt.

Wichtige Informationen:

- Verwenden Sie die Option **Groß-/Kleinschreibung ignorieren**, um zu steuern, ob ein Vergleich unter Berücksichtigung der Groß-/Kleinschreibung durchgeführt wird.

- Geben Sie mehrere Dateinamen an, indem Sie als Trennzeichen jeweils eine einzelne Leerstelle verwenden.

    Beispiel: index.htm index.html

- Ersetzen Sie Leerzeichen in einem Dateinamen durch „%20“.

- Für einen Dateinamen können [Platzhalterwerte](cdn-verizon-premium-rules-engine-reference.md#wildcard-values) verwendet werden. Beispielsweise kann jedes Dateinamensmuster ein oder mehrere Sternchen (*) enthalten, wobei jedes Sternchen für ein Zeichen oder eine Folge von Zeichen steht.

- Wenn keine Platzhalterzeichen angegeben wurden, ist diese Übereinstimmungsbedingung nur bei einer exakten Entsprechung erfüllt.

    Wenn Sie beispielsweise „presentation.ppt“ angeben, ergibt sich eine Übereinstimmung für ein Objekt mit dem Namen „presentation.ppt“, aber nicht für „presentation.pptx“.

[Nach oben](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="url-path-literal"></a>URL Path Literal

Vergleicht den URL-Pfad einer Anforderung, einschließlich Dateiname, mit dem angegebenen Wert.

Die Option **Entspricht**/**Entspricht nicht** bestimmt die Bedingungen, unter denen die Übereinstimmungsbedingung „URL Path Literal“ erfüllt ist.

- **Entspricht:** Für diese Option ist es erforderlich, dass die Anforderung einen URL-Pfad enthält, der mit dem angegebenen Muster übereinstimmt.
- **Entspricht nicht:** Für diese Option ist es erforderlich, dass die Anforderung einen URL-Pfad enthält, der nicht mit dem angegebenen Muster übereinstimmt.

Wichtige Informationen:

- Verwenden Sie die Option **Relativ zu**, um anzugeben, ob der URL-Vergleichspunkt vor oder nach dem Inhaltszugriffspunkt beginnt. 

    Die folgenden Werte sind für die Option **Relativ zu** verfügbar:
  - **Stamm:** Gibt an, dass der URL-Vergleichspunkt direkt nach dem CDN-Hostnamen beginnt.

    Beispiel: http:\//wpc.0001.&lt;Domäne&gt;/**800001/myorigin/myfolder/index.htm**

  - **Ursprung:** Gibt an, dass der URL-Vergleichspunkt nach dem Inhaltszugriffspunkt (z.B. /000001 oder /800001/myorigin) beginnt. Da der CNAME „\*.azureedge.net“ standardmäßig relativ zum Ursprungsverzeichnis des Verizon CDN-Hostnamens erstellt wird, sollten Azure CDN-Benutzer den Wert **Ursprung** verwenden. 

    Beispiel: https:\//&lt;endpoint&gt;.azureedge.net/**myfolder/index.htm**

  Diese URL verweist auf den folgenden Verizon CDN-Hostnamen: http:\//wpc.0001.&lt;Domäne&gt;/800001/myorigin/**myfolder/index.htm**

- Vor einem URL-Vergleich wird eine Edge-CNAME-URL in eine CDN-URL umgeschrieben.

Beispielsweise verweisen die beiden folgenden URLs auf dasselbe Objekt und verfügen daher über den gleichen URL-Pfad:

- CDN-URL: http:\//wpc.0001.&lt;Domäne&gt;/800001/CustomerOrigin/path/asset.htm
- Edge-CNAME-URL: http:\//&lt;Endpunkt&gt;.azureedge.net/path/asset.htm

    Weitere Informationen:
    
    - URL-Pfad (relativ zum Stamm): /800001/CustomerOrigin/path/asset.htm
   
    - URL-Pfad (relativ zum Ursprung): /path/asset.htm

- Abfragezeichenfolgen in der URL werden ignoriert.
- Verwenden Sie die Option **Groß-/Kleinschreibung ignorieren**, um zu steuern, ob ein Vergleich unter Berücksichtigung der Groß-/Kleinschreibung durchgeführt wird.
- Der für diese Übereinstimmungsbedingung angegebene Wert wird mit dem relativen Pfad der exakten Anforderung verglichen, die vom Client gesendet wird.

- Verwenden Sie die Übereinstimmungsbedingung [URL Path Directory](#url-path-directory) oder [URL Path Wildcard](#url-path-wildcard), um alle Anforderungen abzugleichen, die an ein bestimmtes Verzeichnis gesendet werden.

[Nach oben](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="url-path-regex"></a>URL Path Regex

Vergleicht den URL-Pfad einer Anforderung mit dem angegebenen [regulären Ausdruck](cdn-verizon-premium-rules-engine-reference.md#regular-expressions).

Die Option **Entspricht**/**Entspricht nicht** bestimmt die Bedingungen, unter denen die Übereinstimmungsbedingung „URL Path Regex“ erfüllt ist.

- **Entspricht:** Für diese Option ist es erforderlich, dass die Anforderung einen URL-Pfad enthält, der mit dem angegebenen regulären Ausdruck übereinstimmt.
- **Entspricht nicht:** Für diese Option ist es erforderlich, dass die Anforderung einen URL-Pfad enthält, der nicht mit dem angegebenen regulären Ausdruck übereinstimmt.

Wichtige Informationen:

- Vor einem URL-Vergleich wird eine Edge-CNAME-URL in eine CDN-URL umgeschrieben.

    Beispielsweise verweisen hier beide URLs auf dasselbe Objekt und verfügen daher über den gleichen URL-Pfad.

     - CDN-URL: http:\//wpc.0001.&lt;Domäne&gt;/800001/CustomerOrigin/path/asset.htm

     - Edge-CNAME-URL: http:\//my.domain.com/path/asset.htm

    Weitere Informationen:
    
     - URL-Pfad: /800001/CustomerOrigin/path/asset.htm

- Abfragezeichenfolgen in der URL werden ignoriert.
    
- Verwenden Sie die Option **Groß-/Kleinschreibung ignorieren**, um zu steuern, ob ein Vergleich unter Berücksichtigung der Groß-/Kleinschreibung durchgeführt wird.
    
- Leerzeichen im URL-Pfad sollten durch „%20“ ersetzt werden.

[Nach oben](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="url-path-wildcard"></a>URL Path Wildcard

Vergleicht den URL-Pfad einer Anforderung mit dem angegebenen Platzhaltermuster.

Die Option **Entspricht**/**Entspricht nicht** bestimmt die Bedingungen, unter denen die Übereinstimmungsbedingung „URL Path Wildcard“ erfüllt ist.

- **Entspricht:** Für diese Option ist es erforderlich, dass die Anforderung einen URL-Pfad enthält, der mit dem angegebenen Platzhaltermuster übereinstimmt.
- **Entspricht nicht:** Für diese Option ist es erforderlich, dass die Anforderung einen URL-Pfad enthält, der nicht mit dem angegebenen Platzhaltermuster übereinstimmt.

Wichtige Informationen:

- Option **Relativ zu**: Mit dieser Option wird bestimmt, ob der URL-Vergleichspunkt vor oder nach dem Inhaltszugriffspunkt beginnt.

   Für diese Option können die folgenden Werte verwendet werden:
     - **Stamm:** Gibt an, dass der URL-Vergleichspunkt direkt nach dem CDN-Hostnamen beginnt.

       Beispiel: http:\//wpc.0001.&lt;Domäne&gt;/**800001/myorigin/myfolder/index.htm**

     - **Ursprung:** Gibt an, dass der URL-Vergleichspunkt nach dem Inhaltszugriffspunkt (z.B. /000001 oder /800001/myorigin) beginnt. Da der CNAME „\*.azureedge.net“ standardmäßig relativ zum Ursprungsverzeichnis des Verizon CDN-Hostnamens erstellt wird, sollten Azure CDN-Benutzer den Wert **Ursprung** verwenden. 

       Beispiel: https:\//&lt;endpoint&gt;.azureedge.net/**myfolder/index.htm**

     Diese URL verweist auf den folgenden Verizon CDN-Hostnamen: http:\//wpc.0001.&lt;Domäne&gt;/800001/myorigin/**myfolder/index.htm**

- Vor einem URL-Vergleich wird eine Edge-CNAME-URL in eine CDN-URL umgeschrieben.

    Beispielsweise verweisen die beiden folgenden URLs auf dasselbe Objekt und verfügen daher über den gleichen URL-Pfad:
     - CDN-URL: http://wpc.0001.&lt ;Domäne&gt; /800001/CustomerOrigin/path/asset.htm
     - Edge-CNAME-URL: http:\//&lt;Endpunkt&gt;.azureedge.net/path/asset.htm
    
    Weitere Informationen:
    
     - URL-Pfad (relativ zum Stamm): /800001/CustomerOrigin/path/asset.htm
    
     - URL-Pfad (relativ zum Ursprung): /path/asset.htm
    
- Sie können mehrere URL-Pfade angeben, indem Sie sie durch ein einzelnes Leerzeichen voneinander trennen.

   Beispiel: /marketing/asset.* /sales/\*.htm

- Abfragezeichenfolgen in der URL werden ignoriert.
    
- Verwenden Sie die Option **Groß-/Kleinschreibung ignorieren**, um zu steuern, ob ein Vergleich unter Berücksichtigung der Groß-/Kleinschreibung durchgeführt wird.
    
- Ersetzen Sie Leerzeichen im URL-Pfad durch „%20“.
    
- Für den Wert, der für einen URL-Pfad angegeben wird, können [Platzhalterwerte](cdn-verizon-premium-rules-engine-reference.md#wildcard-values) verwendet werden. Jedes URL-Pfadmuster kann ein oder mehrere Sternchen (*) enthalten, wobei jedes Sternchen für ein Zeichen oder eine Folge von Zeichen stehen kann.

#### <a name="sample-scenarios"></a>Beispielszenarien

Bei den Beispielkonfigurationen in der folgenden Tabelle wird vorausgesetzt, dass diese Übereinstimmungsbedingung erfüllt ist, wenn sich für eine Anforderung eine Übereinstimmung mit dem angegebenen URL-Muster ergibt:

Wert                   | Relativ zu    | Ergebnis 
------------------------|----------------|-------
\*/test.html \*/test.php  | Stamm oder Ursprung | Für dieses Muster ergeben sich in beliebigen Ordnern Übereinstimmungen für Anforderungen von Objekten mit dem Namen „test.html“ oder „test.php“.
/80ABCD/origin/text/*   | Stamm           | Für dieses Muster ergibt sich eine Übereinstimmung, wenn das angeforderte Objekt die folgenden Kriterien erfüllt: <br />- Es muss sich an einem Kundenursprungsort mit dem Namen „origin“ befinden. <br />- Der relative Pfad muss mit einem Ordner mit dem Namen „text“ beginnen. Dies bedeutet, dass sich das angeforderte Objekt entweder im Ordner „text“ oder in einem der rekursiven Unterordner befinden kann.
*/css/* */js/*          | Stamm oder Ursprung | Für dieses Muster ergeben sich Übereinstimmungen für alle CDN- oder Edge-CNAME-URLs, die den Ordner „css“ oder „js“ enthalten.
*.jpg *.gif *.png       | Stamm oder Ursprung | Für dieses Muster ergeben sich Übereinstimmungen für alle CDN- oder Edge-CNAME-URLs, die auf „.jpg“, „.gif“ oder „.png“ enden. Eine alternative Möglichkeit zum Angeben dieses Musters ist die Verwendung der [Übereinstimmungsbedingung „URL Path Extension“](#url-path-extension).
/images/\* /media/\*      | Origin         | Für dieses Muster ergibt sich eine Übereinstimmung für CDN- oder Edge-CNAME-URLs, deren relativer Pfad mit dem Ordner „images“ oder „media“ beginnt. <br />- CDN-URL: http:\//wpc.0001.&lt;Domäne&gt;/800001/myorigin/images/sales/event1.png<br />- Beispiel für Edge-CNAME-URL: http:\//cdn.mydomain.com/images/sales/event1.png

[Nach oben](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="url-query-literal"></a>URL Query Literal

Vergleicht die Abfragezeichenfolge einer Anforderung mit dem angegebenen Wert.

Die Option **Entspricht**/**Entspricht nicht** bestimmt die Bedingungen, unter denen die Übereinstimmungsbedingung „URL Query Literal“ erfüllt ist.

- **Entspricht:** Für diese Option ist es erforderlich, dass die Anforderung eine URL-Abfragezeichenfolge enthält, die mit der angegebenen Abfragezeichenfolge übereinstimmt.
- **Entspricht nicht:** Für diese Option ist es erforderlich, dass die Anforderung eine URL-Abfragezeichenfolge enthält, die nicht mit der angegebenen Abfragezeichenfolge übereinstimmt.

Wichtige Informationen:

- Diese Übereinstimmungsbedingung ist nur bei genauen Übereinstimmungen der Abfragezeichenfolge erfüllt.
    
- Verwenden Sie die Option **Groß-/Kleinschreibung ignorieren**, um zu steuern, ob bei Vergleichen von Abfragezeichenfolgen die Groß-/Kleinschreibung berücksichtigt wird.
    
- Fügen Sie in den Text des Abfragezeichenfolgen-Werts kein vorangestelltes Fragezeichen (?) ein.
    
- Für bestimmte Zeichen ist eine URL-Codierung erforderlich. Verwenden Sie das Prozentzeichen, um für die folgenden Zeichen die URL-Codierung durchzuführen:

   Zeichen | URL-Codierung
   ----------|---------
   Leerzeichen     | %20
   &         | %25

- Aufgrund der Art und Weise, in der Cacheeinstellungen nachverfolgt werden, ist diese Übereinstimmungsbedingung mit den folgenden Features nicht kompatibel:
   - Complete Cache Fill
   - Default Internal Max-Age
   - Force Internal Max-Age
   - Ignore Origin No-Cache
   - Internal Max-Stale

[Nach oben](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="url-query-parameter"></a>URL Query Parameter

Identifiziert Anforderungen, die den angegebenen Abfragezeichenfolgenparameter enthalten. Dieser Parameter wird auf einen Wert festgelegt, der mit einem angegebenen Muster übereinstimmt. Abfragezeichenfolgenparameter (z.B. parameter=value) in der Anforderungs-URL bestimmen, ob die Bedingung erfüllt ist. Mit dieser Übereinstimmungsbedingung wird ein Abfragezeichenfolgenparameter anhand des Namens identifiziert, und für den Parameterwert werden ein oder mehrere Werte akzeptiert. 

Die Option **Entspricht**/**Entspricht nicht** bestimmt die Bedingungen, unter denen die Übereinstimmungsbedingung „URL Query Parameter“ erfüllt ist.

- **Entspricht:** Für diese Option ist es erforderlich, dass eine Anforderung den angegebenen Parameter mit einem Wert enthält, der mindestens einem der in dieser Übereinstimmungsbedingung definierten Werte entspricht.
- **Entspricht nicht:** Für diese Option ist es erforderlich, dass die Anforderung eines der folgenden Kriterien erfüllt:
  - Die Anforderung enthält den angegebenen Parameter nicht.
  - Die Anforderung enthält den angegebenen Parameter, aber der Wert entspricht keinem der Werte, die in dieser Übereinstimmungsbedingung definiert sind.

Diese Übereinstimmungsbedingung ermöglicht das einfache Angeben von Kombinationen aus Parametername und -wert. Erwägen Sie die Nutzung der Übereinstimmungsbedingung [URL Query Wildcard](#url-query-wildcard), um für eine höhere Flexibilität beim Abgleichen eines Abfragezeichenfolgenparameters zu sorgen.

Wichtige Informationen:

- Für jede Instanz dieser Übereinstimmungsbedingung kann nur ein einzelner URL-Abfrageparameter angegeben werden.
    
- Da beim Angeben eines Parameternamens Platzhalterwerte nicht unterstützt werden, können nur genaue Übereinstimmungen von Parameternamen für Vergleiche herangezogen werden.
- Parameterwerte können [Platzhalterwerte](cdn-verizon-premium-rules-engine-reference.md#wildcard-values) enthalten.
   - Jedes Parameterwertmuster kann ein oder mehrere Sternchen (*) enthalten, wobei jedes Sternchen für ein Zeichen oder eine Folge von Zeichen stehen kann.
   - Für bestimmte Zeichen ist eine URL-Codierung erforderlich. Verwenden Sie das Prozentzeichen, um für die folgenden Zeichen die URL-Codierung durchzuführen:

       Zeichen | URL-Codierung
       ----------|---------
       Leerzeichen     | %20
       &         | %25

- Geben Sie mehrere Abfragezeichenfolgen-Parameterwerte an, indem Sie als Trennzeichen jeweils ein einzelnes Leerzeichen verwenden. Diese Übereinstimmungsbedingung ist erfüllt, wenn eine Anforderung eine der angegebenen Name-Wert-Kombinationen enthält.

   - Beispiel 1:

     - Konfiguration:

       ValueA ValueB

     - Diese Konfiguration entspricht den folgenden Abfragezeichenfolgenparametern:

       Parameter1=ValueA
    
       Parameter1=ValueB

   - Beispiel 2:

     - Konfiguration: 

        Value%20A Value%20B

     - Diese Konfiguration entspricht den folgenden Abfragezeichenfolgenparametern:

       Parameter1=Value%20A

       Parameter1=Value%20B

- Diese Übereinstimmungsbedingung ist nur erfüllt, wenn sich für mindestens eine der angegebenen Kombinationen aus Abfragezeichenfolgenname und -wert eine genaue Übereinstimmung ergibt.

   Für die Konfiguration aus dem vorherigen Beispiel wird die Kombination „Parameter1=ValueAdd“ aus Abfragezeichenfolgenname und -wert beispielsweise nicht als Übereinstimmung angesehen. Wenn Sie aber einen der folgenden Werte angeben, ergibt sich für diese Name-Wert-Kombination eine Übereinstimmung:

   - ValueA ValueB ValueAdd
   - ValueA* ValueB

- Verwenden Sie die Option **Groß-/Kleinschreibung ignorieren**, um zu steuern, ob bei Vergleichen von Abfragezeichenfolgen die Groß-/Kleinschreibung berücksichtigt wird.
    
- Aufgrund der Art und Weise, in der Cacheeinstellungen nachverfolgt werden, ist diese Übereinstimmungsbedingung mit den folgenden Features nicht kompatibel:
   - Complete Cache Fill
   - Default Internal Max-Age
   - Force Internal Max-Age
   - Ignore Origin No-Cache
   - Internal Max-Stale

#### <a name="sample-scenarios"></a>Beispielszenarien

Im folgenden Beispiel wird veranschaulicht, wie diese Option in bestimmten Situationen funktioniert:

NAME  | Wert |  Ergebnis
------|-------|--------
Benutzer  | Joe   | Für dieses Muster ergibt sich eine Übereinstimmung, wenn die Abfragezeichenfolge für eine angeforderte URL „?user=joe“ lautet.
Benutzer  | *     | Für dieses Muster ergibt sich eine Übereinstimmung, wenn die Abfragezeichenfolge für eine angeforderte URL den Parameter „User“ enthält.
E-Mail | Joe\* | Für dieses Muster ergibt sich eine Übereinstimmung, wenn die Abfragezeichenfolge für eine angeforderte URL einen Email-Parameter enthält, der mit „Joe“ beginnt.

[Nach oben](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="url-query-regex"></a>URL Query Regex

Identifiziert Anforderungen, die den angegebenen Abfragezeichenfolgenparameter enthalten. Dieser Parameter wird auf einen Wert festgelegt, der mit einem angegebenen [regulären Ausdruck](cdn-verizon-premium-rules-engine-reference.md#regular-expressions) übereinstimmt.

Die Option **Entspricht**/**Entspricht nicht** bestimmt die Bedingungen, unter denen die Übereinstimmungsbedingung „URL Query Regex“ erfüllt ist.

- **Entspricht:** Für diese Option ist es erforderlich, dass die Anforderung eine URL-Abfragezeichenfolge enthält, die mit dem angegebenen regulären Ausdruck übereinstimmt.
- **Entspricht nicht:** Für diese Option ist es erforderlich, dass die Anforderung eine URL-Abfragezeichenfolge enthält, die nicht mit dem angegebenen regulären Ausdruck übereinstimmt.

Wichtige Informationen:

- Diese Übereinstimmungsbedingung ist nur erfüllt, wenn sich genaue Übereinstimmungen für den angegebenen regulären Ausdruck ergeben.
    
- Verwenden Sie die Option **Groß-/Kleinschreibung ignorieren**, um zu steuern, ob bei Vergleichen von Abfragezeichenfolgen die Groß-/Kleinschreibung berücksichtigt wird.
    
- Bei dieser Option beginnt eine Abfragezeichenfolge mit dem ersten Zeichen nach dem Fragezeichen-Trennzeichen (?) für die Abfragezeichenfolge.
    
- Für bestimmte Zeichen ist eine URL-Codierung erforderlich. Verwenden Sie das Prozentzeichen, um für die folgenden Zeichen die URL-Codierung durchzuführen:

   Zeichen | URL-Codierung | Wert
   ----------|--------------|------
   Leerzeichen     | %20          | \%20
   &         | %25          | \%25

   Note that percentage symbols must be escaped.

- Double-escape special regular expression characters (for example, \^$.+) to include a backslash in the regular expression.

   For example:

   Value | Interpreted As 
   ------|---------------
   \\+    | +
   \\\\+   | \\+

- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
   - Complete Cache Fill
   - Default Internal Max-Age
   - Force Internal Max-Age
   - Ignore Origin No-Cache
   - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Query Wildcard

Compares the specified value(s) against the request's query string.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Query Wildcard match condition is met.

- **Matches**: Requires the request to contain a URL query string that matches the specified wildcard value.
- **Does Not Match**: Requires the request to contain a URL query string that does not match the specified wildcard value.

Key information:

- For the purposes of this option, a query string starts with the first character after the question mark (?) delimiter for the query string.
- Parameter values can include [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values):
   - Each parameter value pattern can consist of one or more asterisks (*), where each asterisk can match a sequence of one or more characters.
   - Certain characters require URL encoding. Use the percentage symbol to URL encode the following characters:

     Character | URL Encoding
     ----------|---------
     Space     | %20
     &         | %25

- Specify multiple values by delimiting each one with a single space.

   For example: *Parameter1=ValueA* *ValueB* *Parameter1=ValueC&Parameter2=ValueD*

- Only exact matches to at least one of the specified query string patterns satisfy this match condition.
    
- Use the **Ignore Case** option to control the case-sensitivity of query string comparisons.
    
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
   - Complete Cache Fill
   - Default Internal Max-Age
   - Force Internal Max-Age
   - Ignore Origin No-Cache
   - Internal Max-Stale

#### Sample scenarios

The following example demonstrates how this option works in specific situations:

 Name                 | Description
 ---------------------|------------
user=joe              | This pattern is matched when the query string for a requested URL is "?user=joe."
\*user=\* \*optout=\* | This pattern is matched when the CDN URL query contains either the user or optout parameter.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

## Next steps

- [Azure Content Delivery Network overview](cdn-overview.md)
- [Rules engine reference](cdn-verizon-premium-rules-engine-reference.md)
- [Rules engine conditional expressions](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md)
- [Rules engine features](cdn-verizon-premium-rules-engine-reference-features.md)
- [Overriding default HTTP behavior using the rules engine](cdn-verizon-premium-rules-engine.md)\`20
title: Azure CDN from Verizon Premium rules engine match conditions | Microsoft Docs
description: Reference documentation for Azure Content Delivery Network from Verizon Premium rules engine match conditions.
services: cdn
author: mdgattuso

ms.service: azure-cdn
ms.topic: article
ms.date: 05/31/2019
ms.author: magattus

---

# Azure CDN from Verizon Premium rules engine match conditions

This article lists detailed descriptions of the available match conditions for the Azure Content Delivery Network (CDN) from Verizon Premium [rules engine](cdn-verizon-premium-rules-engine.md).

The second part of a rule is the match condition. A match condition identifies specific types of requests for which a set of features will be performed.

For example, you can use a match condition to:

- Filter requests for content at a particular location.
- Filter requests generated from a particular IP address or country/region.
- Filter requests by header information.

## Always match condition

The Always match condition applies a default set of features to all requests.

Name | Purpose
-----|--------
[Always](#always) | Applies a default set of features to all requests.

## Device match condition

The Device match condition identifies requests made from a mobile device based on its properties.  

Name | Purpose
-----|--------
[Device](#device) | Identifies requests made from a mobile device based on its properties.

## Location match conditions

The Location match conditions identify requests based on the requester's location.

Name | Purpose
-----|--------
[AS Number](#as-number) | Identifies requests that originate from a particular network.
[Country](#country) | Identifies requests that originate from the specified countries/regions.

## Origin match conditions

The Origin match conditions identify requests that point to Content Delivery Network storage or a customer origin server.

Name | Purpose
-----|--------
[CDN Origin](#cdn-origin) | Identifies requests for content stored in Content Delivery Network storage.
[Customer Origin](#customer-origin) | Identifies requests for content stored on a specific customer origin server.

## Request match conditions

The Request match conditions identify requests based on their properties.

Name | Purpose
-----|--------
[Client IP Address](#client-ip-address) | Identifies requests that originate from a particular IP address.
[Cookie Parameter](#cookie-parameter) | Checks the cookies associated with each request for the specified value.
[Cookie Parameter Regex](#cookie-parameter-regex) | Checks the cookies associated with each request for the specified regular expression.
[Edge Cname](#edge-cname) | Identifies requests that point to a specific edge CNAME.
[Referring Domain](#referring-domain) | Identifies requests that were referred from the specified host names.
[Request Header Literal](#request-header-literal) | Identifies requests that contain the specified header set to a specified value.
[Request Header Regex](#request-header-regex) | Identifies requests that contain the specified header set to a value that matches the specified regular expression.
[Request Header Wildcard](#request-header-wildcard) | Identifies requests that contain the specified header set to a value that matches the specified pattern.
[Request Method](#request-method) | Identifies requests by their HTTP method.
[Request Scheme](#request-scheme) | Identifies requests by their HTTP protocol.

## URL match conditions

The URL match conditions identify requests based on their URLs.

Name | Purpose
-----|--------
[URL Path Directory](#url-path-directory) | Identifies requests by their relative path.
[URL Path Extension](#url-path-extension) | Identifies requests by their file name extension.
[URL Path Filename](#url-path-filename) | Identifies requests by their file name.
[URL Path Literal](#url-path-literal) | Compares a request's relative path to the specified value.
[URL Path Regex](#url-path-regex) | Compares a request's relative path to the specified regular expression.
[URL Path Wildcard](#url-path-wildcard) | Compares a request's relative path to the specified pattern.
[URL Query Literal](#url-query-literal) | Compares a request's query string to the specified value.
[URL Query Parameter](#url-query-parameter) | Identifies requests that contain the specified query string parameter set to a value that matches a specified pattern.
[URL Query Regex](#url-query-regex) | Identifies requests that contain the specified query string parameter set to a value that matches a specified regular expression.
[URL Query Wildcard](#url-query-wildcard) | Compares the specified value against the request's query string.

## Reference for rules engine match conditions

---

### Always

The Always match condition applies a default set of features to all requests.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### AS Number

The AS Number network is defined by its autonomous system number (ASN). 

The **Matches**/**Does Not Match** option determines the conditions under which the AS Number match condition is met:

- **Matches**: Requires that the ASN of the client network matches one of the specified ASNs. 
- **Does Not Match**: Requires that the ASN of the client network does not match any of the specified ASNs.

Key information:

- Specify multiple ASNs by delimiting each one with a single space. For example, 64514 64515 matches requests that arrive from either 64514 or 64515.
- Certain requests might not return a valid ASN. A question mark (?) will match requests for which a valid ASN could not be determined.
- Specify the entire ASN for the desired network. Partial values will not be matched.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### CDN Origin

The CDN Origin match condition is met when both of the following conditions are met:

- Content from CDN storage was requested.
- The request URI uses the type of content access point (for example, /000001) that's defined in this match condition:
  - CDN URL: The request URI must contain the selected content access point.
  - Edge CNAME URL: The corresponding edge CNAME configuration must point to the selected content access point.
  
Key information:

- The content access point identifies the service that should serve the requested content.
- Don't use an AND IF statement to combine certain match conditions. For example, combining a CDN Origin match condition with a Customer Origin match condition would create a match pattern that could never be matched. For this reason, two CDN Origin match conditions cannot be combined through an AND IF statement.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Client IP Address

The **Matches**/**Does Not Match** option determines the conditions under which the Client IP Address match condition is met:

- **Matches**: Requires that the client's IP address matches one of the specified IP addresses. 
- **Does Not Match**: Requires that the client's IP address does not match any of the specified IP addresses. 

Key information:

- Use CIDR notation.
- Specify multiple IP addresses and/or IP address blocks by delimiting each one with a single space. For example:
  - **IPv4 example**: 1.2.3.4 10.20.30.40 matches any requests that arrive from either address 1.2.3.4 or 10.20.30.40.
  - **IPv6 example**: 1:2:3:4:5:6:7:8 10:20:30:40:50:60:70:80 matches any requests that arrive from either address 1:2:3:4:5:6:7:8 or 10:20:30:40:50:60:70:80.
- The syntax for an IP address block is the base IP address followed by a forward slash and the prefix size. For example:
  - **IPv4 example**: 5.5.5.64/26 matches any requests that arrive from addresses 5.5.5.64 through 5.5.5.127.
  - **IPv6 example**: 1:2:3:/48 matches any requests that arrive from addresses 1:2:3:0:0:0:0:0 through 1:2:3:ffff:ffff:ffff:ffff:ffff.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Cookie Parameter

The **Matches**/**Does Not Match** option determines the conditions under which the Cookie Parameter match condition is met.

- **Matches**: Requires a request to contain the specified cookie with a value that matches at least one of the values that are defined in this match condition.
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified cookie.
  - It contains the specified cookie, but its value does not match any of the values that are defined in this match condition.
  
Key information:

- Cookie name:
  - Because wildcard values, including asterisks (*), are not supported when you're specifying a cookie name, only exact cookie name matches are eligible for comparison.
  - Only a single cookie name can be specified per instance of this match condition.
  - Cookie name comparisons are case-insensitive.
- Cookie value:
  - Specify multiple cookie values by delimiting each one with a single space.
  - A cookie value can take advantage of [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
  - If a wildcard value has not been specified, then only an exact match will satisfy this match condition. For example, specifying "Value" will match "Value," but not "Value1" or "Value2."
  - Use the **Ignore Case** option to control whether a case-sensitive comparison is made against the request's cookie value.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)
</br>

---

### Cookie Parameter Regex

The Cookie Parameter Regex match condition defines a cookie name and value. You can use [regular expressions](cdn-verizon-premium-rules-engine-reference.md#regular-expressions) to define the desired cookie value.

The **Matches**/**Does Not Match** option determines the conditions under which the Cookie Parameter Regex match condition is met.

- **Matches**: Requires a request to contain the specified cookie with a value that matches the specified regular expression.
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified cookie.
  - It contains the specified cookie, but its value does not match the specified regular expression.
  
Key information:

- Cookie name:
  - Because regular expressions and wildcard values, including asterisks (*), are not supported when you're specifying a cookie name, only exact cookie name matches are eligible for comparison.
  - Only a single cookie name can be specified per instance of this match condition.
  - Cookie name comparisons are case-insensitive.
- Cookie value:
  - A cookie value can take advantage of regular expressions.
  - Use the **Ignore Case** option to control whether a case-sensitive comparison is made against the request's cookie value.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Country

You can specify a country through its country code. 

The **Matches**/**Does Not Match** option determines the conditions under which the Country match condition is met:

- **Matches**: Requires the request to contain the specified country code values. 
- **Does Not Match**: Requires that the request does not contain the specified country code values.

Key information:

- Specify multiple country codes by delimiting each one with a single space.
- Wildcards are not supported when you're specifying a country code.
- The "EU" and "AP" country codes do not encompass all IP addresses in those regions.
- Certain requests might not return a valid country code. A question mark (?) will match requests for which a valid country code could not be determined.
- Country codes are case-sensitive.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

#### Implementing Country Filtering by using the rules engine

This match condition allows you to perform a multitude of customizations based on the location from which a request originated. For example, the behavior of the Country Filtering feature can be replicated through the following configuration:

- URL Path Wildcard match: Set the [URL Path Wildcard match condition](#url-path-wildcard) to the directory that will be secured. 
    Append an asterisk to the end of the relative path to ensure that access to all of its children will be restricted by this rule.

- Country match: Set the Country match condition to the desired set of countries.
  - Allow: Set the Country match condition to **Does Not Match** to allow only the specified countries access to content stored in the location defined by the URL Path Wildcard match condition.
  - Block: Set the Country match condition to **Matches** to block the specified countries from accessing content stored in the location defined by the URL Path Wildcard match condition.

- Deny Access (403) Feature: Enable the [Deny Access (403) feature](cdn-verizon-premium-rules-engine-reference-features.md#deny-access-403) to replicate the allow or block portion of the Country Filtering feature.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Customer Origin

Key information:

- The Customer Origin match condition is met regardless of whether content is requested through a CDN URL or an edge CNAME URL that points to the selected customer origin.
- A customer origin configuration that's referenced by a rule cannot be deleted from the Customer Origin page. Before you attempt to delete a customer origin configuration, make sure that the following configurations do not reference it:
  - A Customer Origin match condition
  - An edge CNAME configuration
- Don't use an AND IF statement to combine certain match conditions. For example, combining a Customer Origin match condition with a CDN Origin match condition would create a match pattern that could never be matched. For this reason, two Customer Origin match conditions cannot be combined through an AND IF statement.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Device

The Device match condition identifies requests made from a mobile device based on its properties. Mobile device detection is achieved through [WURFL](http://wurfl.sourceforge.net/). 

The **Matches**/**Does Not Match** option determines the conditions under which the Device match condition is met:

- **Matches**: Requires the requester's device to match the specified value. 
- **Does Not Match**: Requires that the requester's device does not match the specified value.

Key information:

- Use the **Ignore Case** option to specify whether the specified value is case-sensitive.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

#### String Type

A WURFL capability typically accepts any combination of numbers, letters, and symbols. Due to the flexible nature of this capability, you must choose how the value associated with this match condition is interpreted. The following table describes the available set of options:

Type     | Description
---------|------------
Literal  | Select this option to prevent most characters from taking on special meaning by using their [literal value](cdn-verizon-premium-rules-engine-reference.md#literal-values).
Wildcard | Select this option to take advantage of all [wildcard characters]([wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
Regex    | Select this option to use [regular expressions](cdn-verizon-premium-rules-engine-reference.md#regular-expressions). Regular expressions are useful for defining a pattern of characters.

#### WURFL capabilities

A WURFL capability refers to a category that describes mobile devices. The selected capability determines the type of mobile device description that is used to identify requests.

The following table lists WURFL capabilities and their variables for the rules engine.

> [!NOTE]
> The following variables are supported in the **Modify Client Request Header** and **Modify Client Response Header** features.

Capability | Variable | Description | Sample values
-----------|----------|-------------|----------------
Brand Name | %{wurfl_cap_brand_name} | A string that indicates the brand name of the device. | Samsung
Device OS | %{wurfl_cap_device_os} | A string that indicates the operating system installed on the device. | IOS
Device OS Version | %{wurfl_cap_device_os_version} | A string that indicates the version number of the operating system installed on the device. | 1.0.1
Dual Orientation | %{wurfl_cap_dual_orientation} | A Boolean that indicates whether the device supports dual orientation. | true
HTML Preferred DTD | %{wurfl_cap_html_preferred_dtd} | A string that indicates the mobile device's preferred document type definition (DTD) for HTML content. | none<br/>xhtml_basic<br/>html5
Image Inlining | %{wurfl_cap_image_inlining} | A Boolean that indicates whether the device supports Base64 encoded images. | false
Is Android | %{wurfl_vcap_is_android} | A Boolean that indicates whether the device uses the Android OS. | true
Is IOS | %{wurfl_vcap_is_ios} | A Boolean that indicates whether the device uses iOS. | false
Is Smart TV | %{wurfl_cap_is_smarttv} | A Boolean that indicates whether the device is a smart TV. | false
Is Smartphone | %{wurfl_vcap_is_smartphone} | A Boolean that indicates whether the device is a smartphone. | true
Is Tablet | %{wurfl_cap_is_tablet} | A Boolean that indicates whether the device is a tablet. This description is  OS-independent. | true
Is Wireless Device | %{wurfl_cap_is_wireless_device} | A Boolean that indicates whether the device is considered a wireless device. | true
Marketing Name | %{wurfl_cap_marketing_name} | A string that indicates the device's marketing name. | BlackBerry 8100 Pearl
Mobile Browser | %{wurfl_cap_mobile_browser} | A string that indicates the browser that's used to request content from the device. | Chrome
Mobile Browser Version | %{wurfl_cap_mobile_browser_version} | A string that indicates the version of the browser that's used to request content from the device. | 31
Model Name | %{wurfl_cap_model_name} | A string that indicates the device's model name. | s3
Progressive Download | %{wurfl_cap_progressive_download} | A Boolean that indicates whether the device supports the playback of audio and video while it is still being downloaded. | true
Release Date | %{wurfl_cap_release_date} | A string that indicates the year and month on which the device was added to the WURFL database.<br/><br/>Format: `yyyy_mm` | 2013_december
Resolution Height | %{wurfl_cap_resolution_height} | An integer that indicates the device's height in pixels. | 768
Resolution Width | %{wurfl_cap_resolution_width} | An integer that indicates the device's width in pixels. | 1024

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Edge Cname

Key information:

- The list of available edge CNAMEs is limited to those edge CNAMEs that have been configured on the Edge CNAMEs page for the platform on which the rules engine is being configured.
- Before you attempt to delete an edge CNAME configuration, make sure that an Edge Cname match condition does not reference it. Edge CNAME configurations that have been defined in a rule cannot be deleted from the Edge CNAMEs page.
- Don't use an AND IF statement to combine certain match conditions. For example, combining an Edge Cname match condition with a Customer Origin match condition would create a match pattern that could never be matched. For this reason, two Edge Cname match conditions cannot be combined through an AND IF statement.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Referring Domain

The host name associated with the referrer through which content was requested determines whether the Referring Domain condition is met.

The **Matches**/**Does Not Match** option determines the conditions under which the Referring Domain match condition is met:

- **Matches**: Requires the referring host name to match the specified values. 
- **Does Not Match**: Requires that the referring host name does not match the specified value.

Key information:

- Specify multiple host names by delimiting each one with a single space.
- This match condition supports [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
- If the specified value does not contain an asterisk, it must be an exact match for the referrer's host name. For example, specifying "mydomain.com" would not match "www.mydomain.com."
- Use the **Ignore Case** option to control whether a case-sensitive comparison is made.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---  

### Request Header Literal

The **Matches**/**Does Not Match** option determines the conditions under which the Request Header Literal match condition is met.

- **Matches**: Requires the request to contain the specified header. Its value must match the one that's defined in this match condition.
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified header.
  - It contains the specified header, but its value does not match the one that's defined in this match condition.
  
Key information:

- Header name comparisons are always case-insensitive. Use the **Ignore Case** option to control the case-sensitivity of header value comparisons.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---  

### Request Header Regex

The **Matches**/**Does Not Match** option determines the conditions under which the Request Header Regex match condition is met.

- **Matches**: Requires the request to contain the specified header. Its value must match the pattern that's defined in the specified [regular expression](cdn-verizon-premium-rules-engine-reference.md#regular-expressions).
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified header.
  - It contains the specified header, but its value does not match the specified regular expression.

Key information:

- Header name:
  - Header name comparisons are case-insensitive.
  - Replace spaces in the header name with "%20."
- Header value:
  - A header value can take advantage of regular expressions.
  - Use the **Ignore Case** option to control the case-sensitivity of header value comparisons.
  - The match condition is met only when a header value exactly matches at least one of the specified patterns.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Request Header Wildcard

The **Matches**/**Does Not Match** option determines the conditions under which the Request Header Wildcard match condition is met.

- **Matches**: Requires the request to contain the specified header. Its value must match at least one of the values that are defined in this match condition.
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified header.
  - It contains the specified header, but its value does not match any of the specified values.
  
Key information:

- Header name:
  - Header name comparisons are case-insensitive.
  - Spaces in the header name should be replaced with "%20." You can also use this value to specify spaces in a header value.
- Header value:
  - A header value can take advantage of [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
  - Use the **Ignore Case** option to control the case-sensitivity of header value comparisons.
  - This match condition is met when a header value exactly matches to at least one of the specified patterns.
  - Specify multiple values by delimiting each one with a single space.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Request Method

The Request Method match condition is met only when assets are requested through the selected request method. The available request methods are:

- GET
- HEAD
- POST
- OPTIONS
- PUT
- DELETE
- TRACE
- CONNECT

Key information:

- By default, only the GET request method can generate cached content on the network. All other request methods are proxied through the network.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Request Scheme

The Request Scheme match condition is met only when assets are requested through the selected protocol. The available protocols are:

- HTTP
- HTTPS

Key information:

- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Directory

Identifies a request by its relative path, which excludes the file name of the requested asset.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Directory match condition is met.

- **Matches**: Requires the request to contain a relative URL path, excluding the file name, that matches the specified URL pattern.
- **Does Not Match**: Requires the request to contain a relative URL path, excluding file name, that does not match the specified URL pattern.

Key information:

- Use the **Relative to** option to specify whether the URL comparison starts before or after the content access point. The content access point is the portion of the path that appears between the Verizon CDN hostname and the relative path to the requested asset (for example, /800001/CustomerOrigin). It identifies a location by server type (for example, CDN or customer origin) and your customer account number.

   The following values are available for the **Relative to** option:
  - **Root**: Indicates that the URL comparison point begins directly after the CDN hostname. 

  For example: http:\//wpc.0001.&lt;domain&gt;/**800001/myorigin/myfolder**/index.htm

  - **Origin**: Indicates that the URL comparison point begins after the content access point (for example, /000001 or /800001/myorigin). Because the \*.azureedge.net CNAME is created relative to the origin directory on the Verizon CDN hostname by default, Azure CDN users should use the **Origin** value. 

  For example: https:\//&lt;endpoint&gt;.azureedge.net/**myfolder**/index.htm 

  This URL points to the following Verizon CDN hostname: http:\//wpc.0001.&lt;domain&gt;/800001/myorigin/**myfolder**/index.htm

- An edge CNAME URL is rewritten to a CDN URL prior to the URL comparison.

    For example, both of the following URLs point to the same asset and therefore have the same URL path.
  - CDN URL: http:\//wpc.0001.&lt;domain&gt;/800001/CustomerOrigin/path/asset.htm
    
  - Edge CNAME URL: http:\//&lt;endpoint&gt;.azureedge.net/path/asset.htm
    
    Additional information:
  - Custom domain: https:\//my.domain.com/path/asset.htm
    
    - URL path (relative to root): /800001/CustomerOrigin/path/
    
    - URL path (relative to origin): /path/

- The portion of the URL that is used for the URL comparison ends just before the file name of the requested asset. A trailing forward slash is the last character in this type of path.

- Replace any spaces in the URL path pattern with "%20."

- Each URL path pattern can contain one or more asterisks (*), where each asterisk matches a sequence of one or more characters.

- Specify multiple URL paths in the pattern by delimiting each one with a single space.

    For example: */sales/ */marketing/

- A URL path specification can take advantage of [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).

- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Extension

Identifies requests by the file extension of the requested asset.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Extension match condition is met.

- **Matches**: Requires the URL of the request to contain a file extension that exactly matches the specified pattern.

   For example, if you specify "htm", "htm" assets are matched, but not "html" assets.  

- **Does Not Match**: Requires the URL request to contain a file extension that does not match the specified pattern.

Key information:

- Specify the file extensions to match in the **Value** box. Do not include a leading period; for example, use htm instead of .htm.

- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.

- Specify multiple file extensions by delimiting each extension with a single space. 

    For example: htm html

- For example, specifying "htm" matches "htm" assets, but not "html" assets.

#### Sample Scenario

The following sample configuration assumes that this match condition is met when a request matches one of the specified extensions.

Value specification: asp aspx php html

This match condition is met when it finds URLs that end with the following extensions:

- .asp
- .aspx
- .php
- .html

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Filename

Identifies requests by the file name of the requested asset. For the purposes of this match condition, a file name consists of the name of the requested asset, a period, and the file extension (for example, index.html).

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Filename match condition is met.

- **Matches**: Requires the request to contain a file name in its URL path that matches the specified pattern.
- **Does Not Match**: Requires the request to contain a file name in its URL path that does not match the specified pattern.

Key information:

- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.

- To specify multiple file extensions, separate each extension with a single space.

    For example: index.htm index.html

- Replace spaces in a file name value with "%20."

- A file name value can take advantage of [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values). For example, each file name pattern can consist of one or more asterisks (*), where each asterisk matches a sequence of one or more characters.

- If wildcard characters are not specified, then only an exact match will satisfy this match condition.

    For example, specifying "presentation.ppt" matches an asset named "presentation.ppt," but not one named "presentation.pptx."

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Literal

Compares a request's URL path, including file name, to the specified value.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Literal match condition is met.

- **Matches**: Requires the request to contain a URL path that matches the specified pattern.
- **Does Not Match**: Requires the request to contain a URL path that does not match the specified pattern.

Key information:

- Use the **Relative to** option to specify whether the URL comparison point begins before or after the content access point. 

    The following values are available for the **Relative to** option:
  - **Root**: Indicates that the URL comparison point begins directly after the CDN hostname.

    For example: http:\//wpc.0001.&lt;domain&gt;/**800001/myorigin/myfolder/index.htm**

  - **Origin**: Indicates that the URL comparison point begins after the content access point (for example, /000001 or /800001/myorigin). Because the \*.azureedge.net CNAME is created relative to the origin directory on the Verizon CDN hostname by default, Azure CDN users should use the **Origin** value. 

    For example: https:\//&lt;endpoint&gt;.azureedge.net/**myfolder/index.htm**

  This URL points to the following Verizon CDN hostname: http:\//wpc.0001.&lt;domain&gt;/800001/myorigin/**myfolder/index.htm**

- An edge CNAME URL is rewritten to a CDN URL prior to a URL comparison.

For example, both of the following URLs point to the same asset and therefore have the same URL path:

- CDN URL: http:\//wpc.0001.&lt;domain&gt;/800001/CustomerOrigin/path/asset.htm
- Edge CNAME URL: http:\//&lt;endpoint&gt;.azureedge.net/path/asset.htm

    Additional information:
    
    - URL path (relative to root): /800001/CustomerOrigin/path/asset.htm
   
    - URL path (relative to origin): /path/asset.htm

- Query strings in the URL are ignored.
- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.
- The value specified for this match condition is compared against the relative path of the exact request made by the client.

- To match all requests made to a particular directory, use the [URL Path Directory](#url-path-directory) or the [URL Path Wildcard](#url-path-wildcard) match condition.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Regex

Compares a request's URL path to the specified [regular expression](cdn-verizon-premium-rules-engine-reference.md#regular-expressions).

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Regex match condition is met.

- **Matches**: Requires the request to contain a URL path that matches the specified regular expression.
- **Does Not Match**: Requires the request to contain a URL path that does not match the specified regular expression.

Key information:

- An edge CNAME URL is rewritten to a CDN URL prior to URL comparison.

    For example, both URLs point to the same asset and therefore have the same URL path.

     - CDN URL: http:\//wpc.0001.&lt;domain&gt;/800001/CustomerOrigin/path/asset.htm

     - Edge CNAME URL: http:\//my.domain.com/path/asset.htm

    Additional information:
    
     - URL path: /800001/CustomerOrigin/path/asset.htm

- Query strings in the URL are ignored.
    
- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.
    
- Spaces in the URL path should be replaced with "%20."

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Wildcard

Compares a request's relative URL path to the specified wildcard pattern.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Wildcard match condition is met.

- **Matches**: Requires the request to contain a URL path that matches the specified wildcard pattern.
- **Does Not Match**: Requires the request to contain a URL path that does not match the specified wildcard pattern.

Key information:

- **Relative to** option: This option determines whether the URL comparison point begins before or after the content access point.

   This option can have the following values:
     - **Root**: Indicates that the URL comparison point begins directly after the CDN hostname.

       For example: http:\//wpc.0001.&lt;domain&gt;/**800001/myorigin/myfolder/index.htm**

     - **Origin**: Indicates that the URL comparison point begins after the content access point (for example, /000001 or /800001/myorigin). Because the \*.azureedge.net CNAME is created relative to the origin directory on the Verizon CDN hostname by default, Azure CDN users should use the **Origin** value. 

       For example: https:\//&lt;endpoint&gt;.azureedge.net/**myfolder/index.htm**

     This URL points to the following Verizon CDN hostname: http:\//wpc.0001.&lt;domain&gt;/800001/myorigin/**myfolder/index.htm**

- An edge CNAME URL is rewritten to a CDN URL prior to URL comparison.

    For example, both of the following URLs point to the same asset and therefore have the same URL path:
     - CDN URL: http://wpc.0001.&lt;domain&gt;/800001/CustomerOrigin/path/asset.htm
     - Edge CNAME URL: http:\//&lt;endpoint&gt;.azureedge.net/path/asset.htm
    
    Additional information:
    
     - URL path (relative to root): /800001/CustomerOrigin/path/asset.htm
    
     - URL path (relative to origin): /path/asset.htm
    
- Specify multiple URL paths by delimiting each one with a single space.

   For example: /marketing/asset.* /sales/*.htm

- Query strings in the URL are ignored.
    
- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.
    
- Replace spaces in the URL path with "%20."
    
- The value specified for a URL path can take advantage of [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values). Each URL path pattern can contain one or more asterisks (*), where each asterisk can match a sequence of one or more characters.

#### Sample Scenarios

The sample configurations in the following table assume that this match condition is met when a request matches the specified URL pattern:

Value                   | Relative to    | Result 
------------------------|----------------|-------
*/test.html */test.php  | Root or Origin | This pattern is matched by requests for assets named "test.html" or "test.php" in any folder.
/80ABCD/origin/text/*   | Root           | This pattern is matched when the requested asset meets the following criteria: <br />- It must reside on a customer origin called "origin." <br />- The relative path must start with a folder called "text." That is, the requested asset can either reside in the "text" folder or one of its recursive subfolders.
*/css/* */js/*          | Root or Origin | This pattern is matched by all CDN or edge CNAME URLs that contain a css or js folder.
*.jpg *.gif *.png       | Root or Origin | This pattern is matched by all CDN or edge CNAME URLs ending with .jpg, .gif, or .png. An alternative way to specify this pattern is with the [URL Path Extension match condition](#url-path-extension).
/images/* /media/*      | Origin         | This pattern is matched by CDN or edge CNAME URLs whose relative path starts with an "images" or "media" folder. <br />- CDN URL: http:\//wpc.0001.&lt;domain&gt;/800001/myorigin/images/sales/event1.png<br />- Sample edge CNAME URL: http:\//cdn.mydomain.com/images/sales/event1.png

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Query Literal

Compares a request's query string to the specified value.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Query Literal match condition is met.

- **Matches**: Requires the request to contain a URL query string that matches the specified query string.
- **Does Not Match**: Requires the request to contain a URL query string that does not match the specified query string.

Key information:

- Only exact query string matches satisfy this match condition.
    
- Use the **Ignore Case** option to control the case-sensitivity of query string comparisons.
    
- Do not include a leading question mark (?) in the query string value text.
    
- Certain characters require URL encoding. Use the percentage symbol to URL encode the following characters:

   Character | URL Encoding
   ----------|---------
   Space     | %20
   &         | %25

- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
   - Complete Cache Fill
   - Default Internal Max-Age
   - Force Internal Max-Age
   - Ignore Origin No-Cache
   - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Query Parameter

Identifies requests that contain the specified query string parameter. This parameter is set to a value that matches a specified pattern. Query string parameters (for example, parameter=value) in the request URL determine whether this condition is met. This match condition identifies a query string parameter by its name and accepts one or more values for the parameter value. 

The **Matches**/**Does Not Match** option determines the conditions under which the URL Query Parameter match condition is met.

- **Matches**: Requires a request to contain the specified parameter with a value that matches at least one of the values that are defined in this match condition.
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified parameter.
  - It contains the specified parameter, but its value does not match any of the values that are defined in this match condition.

This match condition provides an easy way to specify parameter name/value combinations. For more flexibility if you are matching a query string parameter, consider using the [URL Query Wildcard](#url-query-wildcard) match condition.

Key information:

- Only a single URL query parameter name can be specified per instance of this match condition.
    
- Because wildcard values are not supported when a parameter name is specified, only exact parameter name matches are eligible for comparison.
- Parameter value(s) can include [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
   - Each parameter value pattern can consist of one or more asterisks (*), where each asterisk can match a sequence of one or more characters.
   - Certain characters require URL encoding. Use the percentage symbol to URL encode the following characters:

       Character | URL Encoding
       ----------|---------
       Space     | %20
       &         | %25

- Specify multiple query string parameter values by delimiting each one with a single space. This match condition is met when a request contains one of the specified name/value combinations.

   - Example 1:

     - Configuration:

       ValueA ValueB

     - This configuration matches the following query string parameters:

       Parameter1=ValueA
    
       Parameter1=ValueB

   - Example 2:

     - Configuration: 

        Value%20A Value%20B

     - This configuration matches the following query string parameters:

       Parameter1=Value%20A

       Parameter1=Value%20B

- This match condition is met only when there is an exact match to at least one of the specified query string name/value combinations.

   For example, if you use the configuration in the previous example, the parameter name/value combination "Parameter1=ValueAdd" would not be considered a match. However, if you specify either of the following values, it will match that name/value combination:

   - ValueA ValueB ValueAdd
   - ValueA* ValueB

- Use the **Ignore Case** option to control the case-sensitivity of query string comparisons.
    
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
   - Complete Cache Fill
   - Default Internal Max-Age
   - Force Internal Max-Age
   - Ignore Origin No-Cache
   - Internal Max-Stale

#### Sample scenarios

The following example demonstrates how this option works in specific situations:

Name  | Value |  Result
------|-------|--------
User  | Joe   | This pattern is matched when the query string for a requested URL is "?user=joe."
User  | *     | This pattern is matched when the query string for a requested URL contains a User parameter.
Email | Joe\* | This pattern is matched when the query string for a requested URL contains an Email parameter that starts with "Joe."

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Query Regex

Identifies requests that contain the specified query string parameter. This parameter is set to a value that matches a specified [regular expression](cdn-verizon-premium-rules-engine-reference.md#regular-expressions).

The **Matches**/**Does Not Match** option determines the conditions under which the URL Query Regex match condition is met.

- **Matches**: Requires the request to contain a URL query string that matches the specified regular expression.
- **Does Not Match**: Requires the request to contain a URL query string that does not match the specified regular expression.

Key information:

- Only exact matches to the specified regular expression satisfy this match condition.
    
- Use the **Ignore Case** option to control the case-sensitivity of query string comparisons.
    
- For the purposes of this option, a query string starts with the first character after the question mark (?) delimiter for the query string.
    
- Certain characters require URL encoding. Use the percentage symbol to URL encode the following characters:

   Character | URL Encoding | Value
   ----------|--------------|------
   Space     | %20          | \%20
   &         | %25          | \%25

   Note that percentage symbols must be escaped.

- Double-escape special regular expression characters (for example, \^$.+) to include a backslash in the regular expression.

   For example:

   Value | Interpreted As 
   ------|---------------
   \\+    | +
   \\\\+   | \\+

- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
   - Complete Cache Fill
   - Default Internal Max-Age
   - Force Internal Max-Age
   - Ignore Origin No-Cache
   - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Query Wildcard

Compares the specified value(s) against the request's query string.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Query Wildcard match condition is met.

- **Matches**: Requires the request to contain a URL query string that matches the specified wildcard value.
- **Does Not Match**: Requires the request to contain a URL query string that does not match the specified wildcard value.

Key information:

- For the purposes of this option, a query string starts with the first character after the question mark (?) delimiter for the query string.
- Parameter values can include [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values):
   - Each parameter value pattern can consist of one or more asterisks (*), where each asterisk can match a sequence of one or more characters.
   - Certain characters require URL encoding. Use the percentage symbol to URL encode the following characters:

     Character | URL Encoding
     ----------|---------
     Space     | %20
     &         | %25

- Specify multiple values by delimiting each one with a single space.

   For example: *Parameter1=ValueA* *ValueB* *Parameter1=ValueC&Parameter2=ValueD*

- Only exact matches to at least one of the specified query string patterns satisfy this match condition.
    
- Use the **Ignore Case** option to control the case-sensitivity of query string comparisons.
    
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
   - Complete Cache Fill
   - Default Internal Max-Age
   - Force Internal Max-Age
   - Ignore Origin No-Cache
   - Internal Max-Stale

#### Sample scenarios

The following example demonstrates how this option works in specific situations:

 Name                 | Description
 ---------------------|------------
user=joe              | This pattern is matched when the query string for a requested URL is "?user=joe."
\*user=\* \*optout=\* | This pattern is matched when the CDN URL query contains either the user or optout parameter.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

## Next steps

- [Azure Content Delivery Network overview](cdn-overview.md)
- [Rules engine reference](cdn-verizon-premium-rules-engine-reference.md)
- [Rules engine conditional expressions](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md)
- [Rules engine features](cdn-verizon-premium-rules-engine-reference-features.md)
- [Overriding default HTTP behavior using the rules engine](cdn-verizon-premium-rules-engine.md)\`25
title: Azure CDN from Verizon Premium rules engine match conditions | Microsoft Docs
description: Reference documentation for Azure Content Delivery Network from Verizon Premium rules engine match conditions.
services: cdn
author: mdgattuso

ms.service: azure-cdn
ms.topic: article
ms.date: 05/31/2019
ms.author: magattus

---

# Azure CDN from Verizon Premium rules engine match conditions

This article lists detailed descriptions of the available match conditions for the Azure Content Delivery Network (CDN) from Verizon Premium [rules engine](cdn-verizon-premium-rules-engine.md).

The second part of a rule is the match condition. A match condition identifies specific types of requests for which a set of features will be performed.

For example, you can use a match condition to:

- Filter requests for content at a particular location.
- Filter requests generated from a particular IP address or country/region.
- Filter requests by header information.

## Always match condition

The Always match condition applies a default set of features to all requests.

Name | Purpose
-----|--------
[Always](#always) | Applies a default set of features to all requests.

## Device match condition

The Device match condition identifies requests made from a mobile device based on its properties.  

Name | Purpose
-----|--------
[Device](#device) | Identifies requests made from a mobile device based on its properties.

## Location match conditions

The Location match conditions identify requests based on the requester's location.

Name | Purpose
-----|--------
[AS Number](#as-number) | Identifies requests that originate from a particular network.
[Country](#country) | Identifies requests that originate from the specified countries/regions.

## Origin match conditions

The Origin match conditions identify requests that point to Content Delivery Network storage or a customer origin server.

Name | Purpose
-----|--------
[CDN Origin](#cdn-origin) | Identifies requests for content stored in Content Delivery Network storage.
[Customer Origin](#customer-origin) | Identifies requests for content stored on a specific customer origin server.

## Request match conditions

The Request match conditions identify requests based on their properties.

Name | Purpose
-----|--------
[Client IP Address](#client-ip-address) | Identifies requests that originate from a particular IP address.
[Cookie Parameter](#cookie-parameter) | Checks the cookies associated with each request for the specified value.
[Cookie Parameter Regex](#cookie-parameter-regex) | Checks the cookies associated with each request for the specified regular expression.
[Edge Cname](#edge-cname) | Identifies requests that point to a specific edge CNAME.
[Referring Domain](#referring-domain) | Identifies requests that were referred from the specified host names.
[Request Header Literal](#request-header-literal) | Identifies requests that contain the specified header set to a specified value.
[Request Header Regex](#request-header-regex) | Identifies requests that contain the specified header set to a value that matches the specified regular expression.
[Request Header Wildcard](#request-header-wildcard) | Identifies requests that contain the specified header set to a value that matches the specified pattern.
[Request Method](#request-method) | Identifies requests by their HTTP method.
[Request Scheme](#request-scheme) | Identifies requests by their HTTP protocol.

## URL match conditions

The URL match conditions identify requests based on their URLs.

Name | Purpose
-----|--------
[URL Path Directory](#url-path-directory) | Identifies requests by their relative path.
[URL Path Extension](#url-path-extension) | Identifies requests by their file name extension.
[URL Path Filename](#url-path-filename) | Identifies requests by their file name.
[URL Path Literal](#url-path-literal) | Compares a request's relative path to the specified value.
[URL Path Regex](#url-path-regex) | Compares a request's relative path to the specified regular expression.
[URL Path Wildcard](#url-path-wildcard) | Compares a request's relative path to the specified pattern.
[URL Query Literal](#url-query-literal) | Compares a request's query string to the specified value.
[URL Query Parameter](#url-query-parameter) | Identifies requests that contain the specified query string parameter set to a value that matches a specified pattern.
[URL Query Regex](#url-query-regex) | Identifies requests that contain the specified query string parameter set to a value that matches a specified regular expression.
[URL Query Wildcard](#url-query-wildcard) | Compares the specified value against the request's query string.

## Reference for rules engine match conditions

---

### Always

The Always match condition applies a default set of features to all requests.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### AS Number

The AS Number network is defined by its autonomous system number (ASN). 

The **Matches**/**Does Not Match** option determines the conditions under which the AS Number match condition is met:

- **Matches**: Requires that the ASN of the client network matches one of the specified ASNs. 
- **Does Not Match**: Requires that the ASN of the client network does not match any of the specified ASNs.

Key information:

- Specify multiple ASNs by delimiting each one with a single space. For example, 64514 64515 matches requests that arrive from either 64514 or 64515.
- Certain requests might not return a valid ASN. A question mark (?) will match requests for which a valid ASN could not be determined.
- Specify the entire ASN for the desired network. Partial values will not be matched.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### CDN Origin

The CDN Origin match condition is met when both of the following conditions are met:

- Content from CDN storage was requested.
- The request URI uses the type of content access point (for example, /000001) that's defined in this match condition:
  - CDN URL: The request URI must contain the selected content access point.
  - Edge CNAME URL: The corresponding edge CNAME configuration must point to the selected content access point.
  
Key information:

- The content access point identifies the service that should serve the requested content.
- Don't use an AND IF statement to combine certain match conditions. For example, combining a CDN Origin match condition with a Customer Origin match condition would create a match pattern that could never be matched. For this reason, two CDN Origin match conditions cannot be combined through an AND IF statement.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Client IP Address

The **Matches**/**Does Not Match** option determines the conditions under which the Client IP Address match condition is met:

- **Matches**: Requires that the client's IP address matches one of the specified IP addresses. 
- **Does Not Match**: Requires that the client's IP address does not match any of the specified IP addresses. 

Key information:

- Use CIDR notation.
- Specify multiple IP addresses and/or IP address blocks by delimiting each one with a single space. For example:
  - **IPv4 example**: 1.2.3.4 10.20.30.40 matches any requests that arrive from either address 1.2.3.4 or 10.20.30.40.
  - **IPv6 example**: 1:2:3:4:5:6:7:8 10:20:30:40:50:60:70:80 matches any requests that arrive from either address 1:2:3:4:5:6:7:8 or 10:20:30:40:50:60:70:80.
- The syntax for an IP address block is the base IP address followed by a forward slash and the prefix size. For example:
  - **IPv4 example**: 5.5.5.64/26 matches any requests that arrive from addresses 5.5.5.64 through 5.5.5.127.
  - **IPv6 example**: 1:2:3:/48 matches any requests that arrive from addresses 1:2:3:0:0:0:0:0 through 1:2:3:ffff:ffff:ffff:ffff:ffff.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Cookie Parameter

The **Matches**/**Does Not Match** option determines the conditions under which the Cookie Parameter match condition is met.

- **Matches**: Requires a request to contain the specified cookie with a value that matches at least one of the values that are defined in this match condition.
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified cookie.
  - It contains the specified cookie, but its value does not match any of the values that are defined in this match condition.
  
Key information:

- Cookie name:
  - Because wildcard values, including asterisks (*), are not supported when you're specifying a cookie name, only exact cookie name matches are eligible for comparison.
  - Only a single cookie name can be specified per instance of this match condition.
  - Cookie name comparisons are case-insensitive.
- Cookie value:
  - Specify multiple cookie values by delimiting each one with a single space.
  - A cookie value can take advantage of [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
  - If a wildcard value has not been specified, then only an exact match will satisfy this match condition. For example, specifying "Value" will match "Value," but not "Value1" or "Value2."
  - Use the **Ignore Case** option to control whether a case-sensitive comparison is made against the request's cookie value.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)
</br>

---

### Cookie Parameter Regex

The Cookie Parameter Regex match condition defines a cookie name and value. You can use [regular expressions](cdn-verizon-premium-rules-engine-reference.md#regular-expressions) to define the desired cookie value.

The **Matches**/**Does Not Match** option determines the conditions under which the Cookie Parameter Regex match condition is met.

- **Matches**: Requires a request to contain the specified cookie with a value that matches the specified regular expression.
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified cookie.
  - It contains the specified cookie, but its value does not match the specified regular expression.
  
Key information:

- Cookie name:
  - Because regular expressions and wildcard values, including asterisks (*), are not supported when you're specifying a cookie name, only exact cookie name matches are eligible for comparison.
  - Only a single cookie name can be specified per instance of this match condition.
  - Cookie name comparisons are case-insensitive.
- Cookie value:
  - A cookie value can take advantage of regular expressions.
  - Use the **Ignore Case** option to control whether a case-sensitive comparison is made against the request's cookie value.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Country

You can specify a country through its country code. 

The **Matches**/**Does Not Match** option determines the conditions under which the Country match condition is met:

- **Matches**: Requires the request to contain the specified country code values. 
- **Does Not Match**: Requires that the request does not contain the specified country code values.

Key information:

- Specify multiple country codes by delimiting each one with a single space.
- Wildcards are not supported when you're specifying a country code.
- The "EU" and "AP" country codes do not encompass all IP addresses in those regions.
- Certain requests might not return a valid country code. A question mark (?) will match requests for which a valid country code could not be determined.
- Country codes are case-sensitive.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

#### Implementing Country Filtering by using the rules engine

This match condition allows you to perform a multitude of customizations based on the location from which a request originated. For example, the behavior of the Country Filtering feature can be replicated through the following configuration:

- URL Path Wildcard match: Set the [URL Path Wildcard match condition](#url-path-wildcard) to the directory that will be secured. 
    Append an asterisk to the end of the relative path to ensure that access to all of its children will be restricted by this rule.

- Country match: Set the Country match condition to the desired set of countries.
  - Allow: Set the Country match condition to **Does Not Match** to allow only the specified countries access to content stored in the location defined by the URL Path Wildcard match condition.
  - Block: Set the Country match condition to **Matches** to block the specified countries from accessing content stored in the location defined by the URL Path Wildcard match condition.

- Deny Access (403) Feature: Enable the [Deny Access (403) feature](cdn-verizon-premium-rules-engine-reference-features.md#deny-access-403) to replicate the allow or block portion of the Country Filtering feature.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Customer Origin

Key information:

- The Customer Origin match condition is met regardless of whether content is requested through a CDN URL or an edge CNAME URL that points to the selected customer origin.
- A customer origin configuration that's referenced by a rule cannot be deleted from the Customer Origin page. Before you attempt to delete a customer origin configuration, make sure that the following configurations do not reference it:
  - A Customer Origin match condition
  - An edge CNAME configuration
- Don't use an AND IF statement to combine certain match conditions. For example, combining a Customer Origin match condition with a CDN Origin match condition would create a match pattern that could never be matched. For this reason, two Customer Origin match conditions cannot be combined through an AND IF statement.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Device

The Device match condition identifies requests made from a mobile device based on its properties. Mobile device detection is achieved through [WURFL](http://wurfl.sourceforge.net/). 

The **Matches**/**Does Not Match** option determines the conditions under which the Device match condition is met:

- **Matches**: Requires the requester's device to match the specified value. 
- **Does Not Match**: Requires that the requester's device does not match the specified value.

Key information:

- Use the **Ignore Case** option to specify whether the specified value is case-sensitive.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

#### String Type

A WURFL capability typically accepts any combination of numbers, letters, and symbols. Due to the flexible nature of this capability, you must choose how the value associated with this match condition is interpreted. The following table describes the available set of options:

Type     | Description
---------|------------
Literal  | Select this option to prevent most characters from taking on special meaning by using their [literal value](cdn-verizon-premium-rules-engine-reference.md#literal-values).
Wildcard | Select this option to take advantage of all [wildcard characters]([wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
Regex    | Select this option to use [regular expressions](cdn-verizon-premium-rules-engine-reference.md#regular-expressions). Regular expressions are useful for defining a pattern of characters.

#### WURFL capabilities

A WURFL capability refers to a category that describes mobile devices. The selected capability determines the type of mobile device description that is used to identify requests.

The following table lists WURFL capabilities and their variables for the rules engine.

> [!NOTE]
> The following variables are supported in the **Modify Client Request Header** and **Modify Client Response Header** features.

Capability | Variable | Description | Sample values
-----------|----------|-------------|----------------
Brand Name | %{wurfl_cap_brand_name} | A string that indicates the brand name of the device. | Samsung
Device OS | %{wurfl_cap_device_os} | A string that indicates the operating system installed on the device. | IOS
Device OS Version | %{wurfl_cap_device_os_version} | A string that indicates the version number of the operating system installed on the device. | 1.0.1
Dual Orientation | %{wurfl_cap_dual_orientation} | A Boolean that indicates whether the device supports dual orientation. | true
HTML Preferred DTD | %{wurfl_cap_html_preferred_dtd} | A string that indicates the mobile device's preferred document type definition (DTD) for HTML content. | none<br/>xhtml_basic<br/>html5
Image Inlining | %{wurfl_cap_image_inlining} | A Boolean that indicates whether the device supports Base64 encoded images. | false
Is Android | %{wurfl_vcap_is_android} | A Boolean that indicates whether the device uses the Android OS. | true
Is IOS | %{wurfl_vcap_is_ios} | A Boolean that indicates whether the device uses iOS. | false
Is Smart TV | %{wurfl_cap_is_smarttv} | A Boolean that indicates whether the device is a smart TV. | false
Is Smartphone | %{wurfl_vcap_is_smartphone} | A Boolean that indicates whether the device is a smartphone. | true
Is Tablet | %{wurfl_cap_is_tablet} | A Boolean that indicates whether the device is a tablet. This description is  OS-independent. | true
Is Wireless Device | %{wurfl_cap_is_wireless_device} | A Boolean that indicates whether the device is considered a wireless device. | true
Marketing Name | %{wurfl_cap_marketing_name} | A string that indicates the device's marketing name. | BlackBerry 8100 Pearl
Mobile Browser | %{wurfl_cap_mobile_browser} | A string that indicates the browser that's used to request content from the device. | Chrome
Mobile Browser Version | %{wurfl_cap_mobile_browser_version} | A string that indicates the version of the browser that's used to request content from the device. | 31
Model Name | %{wurfl_cap_model_name} | A string that indicates the device's model name. | s3
Progressive Download | %{wurfl_cap_progressive_download} | A Boolean that indicates whether the device supports the playback of audio and video while it is still being downloaded. | true
Release Date | %{wurfl_cap_release_date} | A string that indicates the year and month on which the device was added to the WURFL database.<br/><br/>Format: `yyyy_mm` | 2013_december
Resolution Height | %{wurfl_cap_resolution_height} | An integer that indicates the device's height in pixels. | 768
Resolution Width | %{wurfl_cap_resolution_width} | An integer that indicates the device's width in pixels. | 1024

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Edge Cname

Key information:

- The list of available edge CNAMEs is limited to those edge CNAMEs that have been configured on the Edge CNAMEs page for the platform on which the rules engine is being configured.
- Before you attempt to delete an edge CNAME configuration, make sure that an Edge Cname match condition does not reference it. Edge CNAME configurations that have been defined in a rule cannot be deleted from the Edge CNAMEs page.
- Don't use an AND IF statement to combine certain match conditions. For example, combining an Edge Cname match condition with a Customer Origin match condition would create a match pattern that could never be matched. For this reason, two Edge Cname match conditions cannot be combined through an AND IF statement.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Referring Domain

The host name associated with the referrer through which content was requested determines whether the Referring Domain condition is met.

The **Matches**/**Does Not Match** option determines the conditions under which the Referring Domain match condition is met:

- **Matches**: Requires the referring host name to match the specified values. 
- **Does Not Match**: Requires that the referring host name does not match the specified value.

Key information:

- Specify multiple host names by delimiting each one with a single space.
- This match condition supports [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
- If the specified value does not contain an asterisk, it must be an exact match for the referrer's host name. For example, specifying "mydomain.com" would not match "www.mydomain.com."
- Use the **Ignore Case** option to control whether a case-sensitive comparison is made.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---  

### Request Header Literal

The **Matches**/**Does Not Match** option determines the conditions under which the Request Header Literal match condition is met.

- **Matches**: Requires the request to contain the specified header. Its value must match the one that's defined in this match condition.
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified header.
  - It contains the specified header, but its value does not match the one that's defined in this match condition.
  
Key information:

- Header name comparisons are always case-insensitive. Use the **Ignore Case** option to control the case-sensitivity of header value comparisons.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---  

### Request Header Regex

The **Matches**/**Does Not Match** option determines the conditions under which the Request Header Regex match condition is met.

- **Matches**: Requires the request to contain the specified header. Its value must match the pattern that's defined in the specified [regular expression](cdn-verizon-premium-rules-engine-reference.md#regular-expressions).
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified header.
  - It contains the specified header, but its value does not match the specified regular expression.

Key information:

- Header name:
  - Header name comparisons are case-insensitive.
  - Replace spaces in the header name with "%20."
- Header value:
  - A header value can take advantage of regular expressions.
  - Use the **Ignore Case** option to control the case-sensitivity of header value comparisons.
  - The match condition is met only when a header value exactly matches at least one of the specified patterns.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Request Header Wildcard

The **Matches**/**Does Not Match** option determines the conditions under which the Request Header Wildcard match condition is met.

- **Matches**: Requires the request to contain the specified header. Its value must match at least one of the values that are defined in this match condition.
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified header.
  - It contains the specified header, but its value does not match any of the specified values.
  
Key information:

- Header name:
  - Header name comparisons are case-insensitive.
  - Spaces in the header name should be replaced with "%20." You can also use this value to specify spaces in a header value.
- Header value:
  - A header value can take advantage of [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
  - Use the **Ignore Case** option to control the case-sensitivity of header value comparisons.
  - This match condition is met when a header value exactly matches to at least one of the specified patterns.
  - Specify multiple values by delimiting each one with a single space.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Request Method

The Request Method match condition is met only when assets are requested through the selected request method. The available request methods are:

- GET
- HEAD
- POST
- OPTIONS
- PUT
- DELETE
- TRACE
- CONNECT

Key information:

- By default, only the GET request method can generate cached content on the network. All other request methods are proxied through the network.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Request Scheme

The Request Scheme match condition is met only when assets are requested through the selected protocol. The available protocols are:

- HTTP
- HTTPS

Key information:

- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Directory

Identifies a request by its relative path, which excludes the file name of the requested asset.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Directory match condition is met.

- **Matches**: Requires the request to contain a relative URL path, excluding the file name, that matches the specified URL pattern.
- **Does Not Match**: Requires the request to contain a relative URL path, excluding file name, that does not match the specified URL pattern.

Key information:

- Use the **Relative to** option to specify whether the URL comparison starts before or after the content access point. The content access point is the portion of the path that appears between the Verizon CDN hostname and the relative path to the requested asset (for example, /800001/CustomerOrigin). It identifies a location by server type (for example, CDN or customer origin) and your customer account number.

   The following values are available for the **Relative to** option:
  - **Root**: Indicates that the URL comparison point begins directly after the CDN hostname. 

  For example: http:\//wpc.0001.&lt;domain&gt;/**800001/myorigin/myfolder**/index.htm

  - **Origin**: Indicates that the URL comparison point begins after the content access point (for example, /000001 or /800001/myorigin). Because the \*.azureedge.net CNAME is created relative to the origin directory on the Verizon CDN hostname by default, Azure CDN users should use the **Origin** value. 

  For example: https:\//&lt;endpoint&gt;.azureedge.net/**myfolder**/index.htm 

  This URL points to the following Verizon CDN hostname: http:\//wpc.0001.&lt;domain&gt;/800001/myorigin/**myfolder**/index.htm

- An edge CNAME URL is rewritten to a CDN URL prior to the URL comparison.

    For example, both of the following URLs point to the same asset and therefore have the same URL path.
  - CDN URL: http:\//wpc.0001.&lt;domain&gt;/800001/CustomerOrigin/path/asset.htm
    
  - Edge CNAME URL: http:\//&lt;endpoint&gt;.azureedge.net/path/asset.htm
    
    Additional information:
  - Custom domain: https:\//my.domain.com/path/asset.htm
    
    - URL path (relative to root): /800001/CustomerOrigin/path/
    
    - URL path (relative to origin): /path/

- The portion of the URL that is used for the URL comparison ends just before the file name of the requested asset. A trailing forward slash is the last character in this type of path.

- Replace any spaces in the URL path pattern with "%20."

- Each URL path pattern can contain one or more asterisks (*), where each asterisk matches a sequence of one or more characters.

- Specify multiple URL paths in the pattern by delimiting each one with a single space.

    For example: */sales/ */marketing/

- A URL path specification can take advantage of [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).

- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Extension

Identifies requests by the file extension of the requested asset.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Extension match condition is met.

- **Matches**: Requires the URL of the request to contain a file extension that exactly matches the specified pattern.

   For example, if you specify "htm", "htm" assets are matched, but not "html" assets.  

- **Does Not Match**: Requires the URL request to contain a file extension that does not match the specified pattern.

Key information:

- Specify the file extensions to match in the **Value** box. Do not include a leading period; for example, use htm instead of .htm.

- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.

- Specify multiple file extensions by delimiting each extension with a single space. 

    For example: htm html

- For example, specifying "htm" matches "htm" assets, but not "html" assets.

#### Sample Scenario

The following sample configuration assumes that this match condition is met when a request matches one of the specified extensions.

Value specification: asp aspx php html

This match condition is met when it finds URLs that end with the following extensions:

- .asp
- .aspx
- .php
- .html

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Filename

Identifies requests by the file name of the requested asset. For the purposes of this match condition, a file name consists of the name of the requested asset, a period, and the file extension (for example, index.html).

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Filename match condition is met.

- **Matches**: Requires the request to contain a file name in its URL path that matches the specified pattern.
- **Does Not Match**: Requires the request to contain a file name in its URL path that does not match the specified pattern.

Key information:

- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.

- To specify multiple file extensions, separate each extension with a single space.

    For example: index.htm index.html

- Replace spaces in a file name value with "%20."

- A file name value can take advantage of [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values). For example, each file name pattern can consist of one or more asterisks (*), where each asterisk matches a sequence of one or more characters.

- If wildcard characters are not specified, then only an exact match will satisfy this match condition.

    For example, specifying "presentation.ppt" matches an asset named "presentation.ppt," but not one named "presentation.pptx."

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Literal

Compares a request's URL path, including file name, to the specified value.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Literal match condition is met.

- **Matches**: Requires the request to contain a URL path that matches the specified pattern.
- **Does Not Match**: Requires the request to contain a URL path that does not match the specified pattern.

Key information:

- Use the **Relative to** option to specify whether the URL comparison point begins before or after the content access point. 

    The following values are available for the **Relative to** option:
  - **Root**: Indicates that the URL comparison point begins directly after the CDN hostname.

    For example: http:\//wpc.0001.&lt;domain&gt;/**800001/myorigin/myfolder/index.htm**

  - **Origin**: Indicates that the URL comparison point begins after the content access point (for example, /000001 or /800001/myorigin). Because the \*.azureedge.net CNAME is created relative to the origin directory on the Verizon CDN hostname by default, Azure CDN users should use the **Origin** value. 

    For example: https:\//&lt;endpoint&gt;.azureedge.net/**myfolder/index.htm**

  This URL points to the following Verizon CDN hostname: http:\//wpc.0001.&lt;domain&gt;/800001/myorigin/**myfolder/index.htm**

- An edge CNAME URL is rewritten to a CDN URL prior to a URL comparison.

For example, both of the following URLs point to the same asset and therefore have the same URL path:

- CDN URL: http:\//wpc.0001.&lt;domain&gt;/800001/CustomerOrigin/path/asset.htm
- Edge CNAME URL: http:\//&lt;endpoint&gt;.azureedge.net/path/asset.htm

    Additional information:
    
    - URL path (relative to root): /800001/CustomerOrigin/path/asset.htm
   
    - URL path (relative to origin): /path/asset.htm

- Query strings in the URL are ignored.
- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.
- The value specified for this match condition is compared against the relative path of the exact request made by the client.

- To match all requests made to a particular directory, use the [URL Path Directory](#url-path-directory) or the [URL Path Wildcard](#url-path-wildcard) match condition.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Regex

Compares a request's URL path to the specified [regular expression](cdn-verizon-premium-rules-engine-reference.md#regular-expressions).

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Regex match condition is met.

- **Matches**: Requires the request to contain a URL path that matches the specified regular expression.
- **Does Not Match**: Requires the request to contain a URL path that does not match the specified regular expression.

Key information:

- An edge CNAME URL is rewritten to a CDN URL prior to URL comparison.

    For example, both URLs point to the same asset and therefore have the same URL path.

     - CDN URL: http:\//wpc.0001.&lt;domain&gt;/800001/CustomerOrigin/path/asset.htm

     - Edge CNAME URL: http:\//my.domain.com/path/asset.htm

    Additional information:
    
     - URL path: /800001/CustomerOrigin/path/asset.htm

- Query strings in the URL are ignored.
    
- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.
    
- Spaces in the URL path should be replaced with "%20."

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Wildcard

Compares a request's relative URL path to the specified wildcard pattern.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Wildcard match condition is met.

- **Matches**: Requires the request to contain a URL path that matches the specified wildcard pattern.
- **Does Not Match**: Requires the request to contain a URL path that does not match the specified wildcard pattern.

Key information:

- **Relative to** option: This option determines whether the URL comparison point begins before or after the content access point.

   This option can have the following values:
     - **Root**: Indicates that the URL comparison point begins directly after the CDN hostname.

       For example: http:\//wpc.0001.&lt;domain&gt;/**800001/myorigin/myfolder/index.htm**

     - **Origin**: Indicates that the URL comparison point begins after the content access point (for example, /000001 or /800001/myorigin). Because the \*.azureedge.net CNAME is created relative to the origin directory on the Verizon CDN hostname by default, Azure CDN users should use the **Origin** value. 

       For example: https:\//&lt;endpoint&gt;.azureedge.net/**myfolder/index.htm**

     This URL points to the following Verizon CDN hostname: http:\//wpc.0001.&lt;domain&gt;/800001/myorigin/**myfolder/index.htm**

- An edge CNAME URL is rewritten to a CDN URL prior to URL comparison.

    For example, both of the following URLs point to the same asset and therefore have the same URL path:
     - CDN URL: http://wpc.0001.&lt;domain&gt;/800001/CustomerOrigin/path/asset.htm
     - Edge CNAME URL: http:\//&lt;endpoint&gt;.azureedge.net/path/asset.htm
    
    Additional information:
    
     - URL path (relative to root): /800001/CustomerOrigin/path/asset.htm
    
     - URL path (relative to origin): /path/asset.htm
    
- Specify multiple URL paths by delimiting each one with a single space.

   For example: /marketing/asset.* /sales/*.htm

- Query strings in the URL are ignored.
    
- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.
    
- Replace spaces in the URL path with "%20."
    
- The value specified for a URL path can take advantage of [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values). Each URL path pattern can contain one or more asterisks (*), where each asterisk can match a sequence of one or more characters.

#### Sample Scenarios

The sample configurations in the following table assume that this match condition is met when a request matches the specified URL pattern:

Value                   | Relative to    | Result 
------------------------|----------------|-------
*/test.html */test.php  | Root or Origin | This pattern is matched by requests for assets named "test.html" or "test.php" in any folder.
/80ABCD/origin/text/*   | Root           | This pattern is matched when the requested asset meets the following criteria: <br />- It must reside on a customer origin called "origin." <br />- The relative path must start with a folder called "text." That is, the requested asset can either reside in the "text" folder or one of its recursive subfolders.
*/css/* */js/*          | Root or Origin | This pattern is matched by all CDN or edge CNAME URLs that contain a css or js folder.
*.jpg *.gif *.png       | Root or Origin | This pattern is matched by all CDN or edge CNAME URLs ending with .jpg, .gif, or .png. An alternative way to specify this pattern is with the [URL Path Extension match condition](#url-path-extension).
/images/* /media/*      | Origin         | This pattern is matched by CDN or edge CNAME URLs whose relative path starts with an "images" or "media" folder. <br />- CDN URL: http:\//wpc.0001.&lt;domain&gt;/800001/myorigin/images/sales/event1.png<br />- Sample edge CNAME URL: http:\//cdn.mydomain.com/images/sales/event1.png

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Query Literal

Compares a request's query string to the specified value.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Query Literal match condition is met.

- **Matches**: Requires the request to contain a URL query string that matches the specified query string.
- **Does Not Match**: Requires the request to contain a URL query string that does not match the specified query string.

Key information:

- Only exact query string matches satisfy this match condition.
    
- Use the **Ignore Case** option to control the case-sensitivity of query string comparisons.
    
- Do not include a leading question mark (?) in the query string value text.
    
- Certain characters require URL encoding. Use the percentage symbol to URL encode the following characters:

   Character | URL Encoding
   ----------|---------
   Space     | %20
   &         | %25

- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
   - Complete Cache Fill
   - Default Internal Max-Age
   - Force Internal Max-Age
   - Ignore Origin No-Cache
   - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Query Parameter

Identifies requests that contain the specified query string parameter. This parameter is set to a value that matches a specified pattern. Query string parameters (for example, parameter=value) in the request URL determine whether this condition is met. This match condition identifies a query string parameter by its name and accepts one or more values for the parameter value. 

The **Matches**/**Does Not Match** option determines the conditions under which the URL Query Parameter match condition is met.

- **Matches**: Requires a request to contain the specified parameter with a value that matches at least one of the values that are defined in this match condition.
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified parameter.
  - It contains the specified parameter, but its value does not match any of the values that are defined in this match condition.

This match condition provides an easy way to specify parameter name/value combinations. For more flexibility if you are matching a query string parameter, consider using the [URL Query Wildcard](#url-query-wildcard) match condition.

Key information:

- Only a single URL query parameter name can be specified per instance of this match condition.
    
- Because wildcard values are not supported when a parameter name is specified, only exact parameter name matches are eligible for comparison.
- Parameter value(s) can include [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
   - Each parameter value pattern can consist of one or more asterisks (*), where each asterisk can match a sequence of one or more characters.
   - Certain characters require URL encoding. Use the percentage symbol to URL encode the following characters:

       Character | URL Encoding
       ----------|---------
       Space     | %20
       &         | %25

- Specify multiple query string parameter values by delimiting each one with a single space. This match condition is met when a request contains one of the specified name/value combinations.

   - Example 1:

     - Configuration:

       ValueA ValueB

     - This configuration matches the following query string parameters:

       Parameter1=ValueA
    
       Parameter1=ValueB

   - Example 2:

     - Configuration: 

        Value%20A Value%20B

     - This configuration matches the following query string parameters:

       Parameter1=Value%20A

       Parameter1=Value%20B

- This match condition is met only when there is an exact match to at least one of the specified query string name/value combinations.

   For example, if you use the configuration in the previous example, the parameter name/value combination "Parameter1=ValueAdd" would not be considered a match. However, if you specify either of the following values, it will match that name/value combination:

   - ValueA ValueB ValueAdd
   - ValueA* ValueB

- Use the **Ignore Case** option to control the case-sensitivity of query string comparisons.
    
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
   - Complete Cache Fill
   - Default Internal Max-Age
   - Force Internal Max-Age
   - Ignore Origin No-Cache
   - Internal Max-Stale

#### Sample scenarios

The following example demonstrates how this option works in specific situations:

Name  | Value |  Result
------|-------|--------
User  | Joe   | This pattern is matched when the query string for a requested URL is "?user=joe."
User  | *     | This pattern is matched when the query string for a requested URL contains a User parameter.
Email | Joe\* | This pattern is matched when the query string for a requested URL contains an Email parameter that starts with "Joe."

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Query Regex

Identifies requests that contain the specified query string parameter. This parameter is set to a value that matches a specified [regular expression](cdn-verizon-premium-rules-engine-reference.md#regular-expressions).

The **Matches**/**Does Not Match** option determines the conditions under which the URL Query Regex match condition is met.

- **Matches**: Requires the request to contain a URL query string that matches the specified regular expression.
- **Does Not Match**: Requires the request to contain a URL query string that does not match the specified regular expression.

Key information:

- Only exact matches to the specified regular expression satisfy this match condition.
    
- Use the **Ignore Case** option to control the case-sensitivity of query string comparisons.
    
- For the purposes of this option, a query string starts with the first character after the question mark (?) delimiter for the query string.
    
- Certain characters require URL encoding. Use the percentage symbol to URL encode the following characters:

   Character | URL Encoding | Value
   ----------|--------------|------
   Space     | %20          | \%20
   &         | %25          | \%25

   Beachten Sie, dass Prozentzeichen mit Escapezeichen versehen werden müssen.

- Versehen Sie spezielle Zeichen regulärer Ausdrücke (z.B. \^$.+) mit doppelten Escapezeichen, um einen umgekehrten Schrägstrich in den regulären Ausdruck einzubinden.

   Beispiel:

   Wert | Interpretiert als 
   ------|---------------
   \\+    | +
   \\\\+   | \\+

- Aufgrund der Art und Weise, in der Cacheeinstellungen nachverfolgt werden, ist diese Übereinstimmungsbedingung mit den folgenden Features nicht kompatibel:
   - Complete Cache Fill
   - Default Internal Max-Age
   - Force Internal Max-Age
   - Ignore Origin No-Cache
   - Internal Max-Stale

[Nach oben](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="url-query-wildcard"></a>URL Query Wildcard

Vergleicht die angegebenen Werte mit der Abfragezeichenfolge der Anforderung.

Die Option **Entspricht**/**Entspricht nicht** bestimmt die Bedingungen, unter denen die Übereinstimmungsbedingung „URL Query Wildcard“ erfüllt ist.

- **Entspricht:** Für diese Option ist es erforderlich, dass die Anforderung eine URL-Abfragezeichenfolge enthält, die mit dem angegebenen Platzhalterwert übereinstimmt.
- **Entspricht nicht:** Für diese Option ist es erforderlich, dass die Anforderung eine URL-Abfragezeichenfolge enthält, die nicht mit dem angegebenen Platzhalterwert übereinstimmt.

Wichtige Informationen:

- Bei dieser Option beginnt eine Abfragezeichenfolge mit dem ersten Zeichen nach dem Fragezeichen-Trennzeichen (?) für die Abfragezeichenfolge.
- Parameterwerte können [Platzhalterwerte](cdn-verizon-premium-rules-engine-reference.md#wildcard-values) enthalten:
   - Jedes Parameterwertmuster kann ein oder mehrere Sternchen (*) enthalten, wobei jedes Sternchen für ein Zeichen oder eine Folge von Zeichen stehen kann.
   - Für bestimmte Zeichen ist eine URL-Codierung erforderlich. Verwenden Sie das Prozentzeichen, um für die folgenden Zeichen die URL-Codierung durchzuführen:

     Zeichen | URL-Codierung
     ----------|---------
     Leerzeichen     | %20
     &         | %25

- Wenn Sie mehrere Werte angeben möchten, trennen Sie diese durch ein einzelnes Leerzeichen voneinander.

   Beispiel:  *Parameter1=ValueA* *ValueB* *Parameter1=ValueC&Parameter2=ValueD*

- Diese Übereinstimmungsbedingung ist nur erfüllt, wenn sich genaue Übereinstimmungen mit mindestens einem der angegebenen Abfragezeichenfolgenmuster ergeben.
    
- Verwenden Sie die Option **Groß-/Kleinschreibung ignorieren**, um zu steuern, ob bei Vergleichen von Abfragezeichenfolgen die Groß-/Kleinschreibung berücksichtigt wird.
    
- Aufgrund der Art und Weise, in der Cacheeinstellungen nachverfolgt werden, ist diese Übereinstimmungsbedingung mit den folgenden Features nicht kompatibel:
   - Complete Cache Fill
   - Default Internal Max-Age
   - Force Internal Max-Age
   - Ignore Origin No-Cache
   - Internal Max-Stale

#### <a name="sample-scenarios"></a>Beispielszenarien

Im folgenden Beispiel wird veranschaulicht, wie diese Option in bestimmten Situationen funktioniert:

 NAME                 | BESCHREIBUNG
 ---------------------|------------
user=joe              | Für dieses Muster ergibt sich eine Übereinstimmung, wenn die Abfragezeichenfolge für eine angeforderte URL „?user=joe“ lautet.
\*user=\* \*optout=\* | Für dieses Muster ergibt sich eine Übereinstimmung, wenn die CDN-URL-Abfrage entweder den Parameter „user“ oder „optout“ enthält.

[Nach oben](#reference-for-rules-engine-match-conditions)

</br>

## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über Azure Content Delivery Network](cdn-overview.md)
- [Regel-Engine – Referenz](cdn-verizon-premium-rules-engine-reference.md)
- [Regel-Engine – bedingte Ausdrücke](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md)
- [Regel-Engine – Features](cdn-verizon-premium-rules-engine-reference-features.md)
- [Überschreiben des HTTP-Standardverhaltens mithilfe der Regel-Engine](cdn-verizon-premium-rules-engine.md)
