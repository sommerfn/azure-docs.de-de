---
title: Übereinstimmungsbedingungen der Microsoft Standardregel-Engine für Azure CDN | Microsoft-Dokumentation
description: Referenzdokumentation zu den Übereinstimmungsbedingungen der Microsoft Standardregel-Engine für Azure Content Delivery Network.
services: cdn
author: mdgattuso
ms.service: azure-cdn
ms.topic: article
ms.date: 11/01/2019
ms.author: magattus
ms.openlocfilehash: a72452d37b152a9463a5aee0e199fd42ea852236
ms.sourcegitcommit: b2fb32ae73b12cf2d180e6e4ffffa13a31aa4c6f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73615841"
---
# <a name="azure-cdn-from-microsoft-standard-rules-engine-match-conditions"></a>Übereinstimmungsbedingungen der Microsoft Standardregel-Engine für Azure CDN

Dieser Artikel enthält ausführliche Beschreibungen der verfügbaren Übereinstimmungsbedingungen der Microsoft [Standardregel-Engine](cdn-standard-rules-engine.md) für das Azure Content Delivery Network (CDN).

Der erste Teil einer Regel besteht aus mehreren Übereinstimmungsbedingungen. Jede Regel kann bis zu 4 Übereinstimmungsbedingungen enthalten. Eine Übereinstimmungsbedingung gibt bestimmte Typen von Anforderungen an, für die die in der Regel definierten Aktionen ausgeführt werden. Wenn Sie mehrere Übereinstimmungsbedingungen verwenden, werden diese mit UND-Logik gruppiert.

Sie können eine Übereinstimmungsbedingung z.B. für Folgendes verwenden:

- Filtern von Anforderungen, die von einer bestimmten IP-Adresse oder einem bestimmten Land bzw. einer bestimmten Region generiert wurden
- Filtern von Anforderungen nach Headerinformationen
- Filtern von Anforderungen von Mobil- oder Desktopgeräten

## <a name="match-conditions"></a>Übereinstimmungsbedingungen

Die folgenden Übereinstimmungsbedingungen können verwendet werden. 

### <a name="device-type"></a>Gerätetyp 

Die Übereinstimmungsbedingung für den Gerätetyp identifiziert Anforderungen von einem Mobil- oder Desktopgerät anhand der jeweiligen Eigenschaften.  

**Erforderliche Felder**

Operator | Unterstützter Wert
---------|----------------
Gleich, Ungleich | Mobil, Desktop


### <a name="http-version"></a>HTTP-Version

Die Übereinstimmungsbedingung für die HTTP-Version identifiziert Anforderungen anhand der HTTP-Version, mit der die Anforderung eingeht.

**Erforderliche Felder**

Operator | Unterstützter Wert
---------|----------------
Gleich, Ungleich | 2.0, 1.1, 1.0, 0.9, All


### <a name="request-cookies"></a>Anforderungscookies

Die Übereinstimmungsbedingung für Anforderungscookies identifiziert Anforderungen anhand der Cookieinformationen in der eingehenden Anforderung.

**Erforderliche Felder**

Cookiename | Operator | Cookiewert | Umwandlung der Groß-/Kleinschreibung
------------|----------|--------------|---------------
Zeichenfolge | [Liste der Standardoperatoren](#standard-operator-list) | String, Int | Keine Umwandlung, in Großbuchstaben, in Kleinbuchstaben

Wichtige Informationen
- Platzhalterwerte, z. B. Sternchen (*) werden beim Angeben eines Cookienamens nicht unterstützt. Daher können nur exakte Übereinstimmungen von Cookienamen für Vergleiche herangezogen werden.
- Für jede Instanz dieser Übereinstimmungsbedingung kann nur ein einziger Cookiename angegeben werden.
- Beim Vergleichen von Cookienamen wird die Groß-/Kleinschreibung nicht beachtet.
- Wenn Sie mehrere Cookiewerte angeben möchten, trennen Sie diese durch ein einzelnes Leerzeichen voneinander. 
- Für Cookiewerte können Platzhalterwerte verwendet werden.
- Wenn kein Platzhalterwert angegeben wurde, wird diese Übereinstimmungsbedingung nur bei einer exakten Übereinstimmung erfüllt. Durch die Angabe von „Wert“ wird eine Übereinstimmung mit „Wert“ erzielt, aber nicht mit „Wert1“. 

### <a name="post-argument"></a>POST-Argument

**Erforderliche Felder**

Argumentname | Operator | Argumentwert | Umwandlung der Groß-/Kleinschreibung
--------------|----------|----------------|---------------
Zeichenfolge | [Liste der Standardoperatoren](#standard-operator-list) | String, Int | Keine Umwandlung, in Großbuchstaben, in Kleinbuchstaben

### <a name="query-string"></a>Abfragezeichenfolge

Die Übereinstimmungsbedingungen für die Abfragezeichenfolge identifizieren Anforderungen, die einen angegebenen Abfragezeichenfolgenparameter enthalten. Dieser Parameter wird auf einen Wert festgelegt, der mit einem angegebenen Muster übereinstimmt. Abfragezeichenfolgenparameter (z.B. parameter=value) in der Anforderungs-URL bestimmen, ob die Bedingung erfüllt ist. Mit dieser Übereinstimmungsbedingung wird ein Abfragezeichenfolgenparameter anhand des Namens identifiziert, und für den Parameterwert werden ein oder mehrere Werte akzeptiert.

**Erforderliche Felder**

Operator | Abfragezeichenfolge | Umwandlung der Groß-/Kleinschreibung
---------|--------------|---------------
[Liste der Standardoperatoren](#standard-operator-list) | String, Int | Keine Umwandlung, in Großbuchstaben, in Kleinbuchstaben

### <a name="remote-address"></a>Remoteadresse

Die Übereinstimmungsbedingung für die Remoteadresse identifizieren Anforderungen anhand des Standorts oder anhand der IP-Adresse des Anfordernden.

**Erforderliche Felder**

Operator | Unterstützte Werte
---------|-----------------
Any | –
Geografische Übereinstimmung | Landeskennzahlen
IP-Übereinstimmung | IP-Adressen (durch Leerzeichen getrennt)
Keine | –
Keine geografische Übereinstimmung | Landeskennzahlen
Keine IP-Übereinstimmung | IP-Adressen (durch Leerzeichen getrennt)

Wichtige Informationen:

- Verwenden Sie die CIDR-Notation.
- Wenn Sie mehrere IP-Adressen und/oder IP-Adressblöcke angeben, trennen Sie diese mit einem einzelnen Leerzeichen. Beispiel:
  - **IPv4-Beispiel:** Die Angabe „1.2.3.4 10.20.30.40“ führt zur Übereinstimmung mit eingehenden Anforderungen von der Adresse 1.2.3.4 oder 10.20.30.40.
  - **IPv6-Beispiel:** Die Angabe „1:2:3:4:5:6:7:8 10:20:30:40:50:60:70:80“ führt zur Übereinstimmung mit eingehenden Anforderungen von der Adresse 1:2:3:4:5:6:7:8 oder 10:20:30:40:50:60:70:80.
- Die Syntax für einen IP-Adressblock besteht aus der IP-Basisadresse, gefolgt von einem Schrägstrich und der Präfixgröße. Beispiel:
  - **IPv4-Beispiel:** Die Angabe „5.5.5.64/26“ führt zur Übereinstimmung mit eingehenden Anforderungen von den Adressen 5.5.5.64 bis 5.5.5.127.
  - **IPv6-Beispiel:** Die Angabe „1:2:3:/48“ führt zur Übereinstimmung mit eingehenden Anforderungen von den Adressen 1:2:3:0:0:0:0:0 bis 1:2:3:ffff:ffff:ffff:ffff:ffff.

### <a name="request-body"></a>Anforderungstext

**Erforderliche Felder**

Operator | Anforderungstext | Umwandlung der Groß-/Kleinschreibung
---------|--------------|---------------
[Liste der Standardoperatoren](#standard-operator-list) | String, Int | Keine Umwandlung, in Großbuchstaben, in Kleinbuchstaben

### <a name="request-header"></a>Anforderungsheader

**Erforderliche Felder**
Headername | Operator | Headerwert | Umwandlung der Groß-/Kleinschreibung
------------|----------|--------------|---------------
Zeichenfolge | [Liste der Standardoperatoren](#standard-operator-list) | String, Int | Keine Umwandlung, in Großbuchstaben, in Kleinbuchstaben

### <a name="request-method"></a>Request Method

**Erforderliche Felder**

Operator | Unterstützter Wert
---------|----------------
Gleich, Ungleich | GET, POST, PUT, DELETE, HEAD, OPTIONS, TRACE

Wichtige Informationen:

- Nur die GET-Anforderungsmethode zwischengespeicherten Inhalt im CDN generieren. Alle anderen Anforderungsmethoden werden per Proxy durch das Netzwerk gesendet. 

### <a name="request-protocol"></a>Anforderungsprotokoll

**Erforderliche Felder**

Operator | Unterstützter Wert
---------|----------------
Gleich, Ungleich | HTTP, HTTPS

### <a name="request-url"></a>Anfrage-URL

**Erforderliche Felder**

Operator | Anfrage-URL | Umwandlung der Groß-/Kleinschreibung
---------|-------------|---------------
[Liste der Standardoperatoren](#standard-operator-list) | String, Int | Keine Umwandlung, in Großbuchstaben, in Kleinbuchstaben

Wichtige Informationen:

- Achten Sie beim Eingeben der Anforderungs-URL darauf, dass Sie Protokollinformationen einbeziehen, z. B. "https://www.[yourdomain].com".

### <a name="url-file-extension"></a>URL-Dateierweiterung

**Erforderliche Felder**

Operator | Durchwahl | Umwandlung der Groß-/Kleinschreibung
---------|-----------|---------------
[Liste der Standardoperatoren](#standard-operator-list) | String, Int | Keine Umwandlung, in Großbuchstaben, in Kleinbuchstaben

Wichtige Informationen:

- Lassen Sie bei der Erweiterung den vorangestellten Punkt weg. Verwenden Sie also „htm“ anstelle von „.htm“.

### <a name="url-file-name"></a>URL-Dateiname

**Erforderliche Felder**

Operator | Dateiname | Umwandlung der Groß-/Kleinschreibung
---------|-----------|---------------
[Liste der Standardoperatoren](#standard-operator-list) | String, Int | Keine Umwandlung, in Großbuchstaben, in Kleinbuchstaben

Wichtige Informationen:

- Um mehrere Dateinamen anzugeben, trennen Sie die einzelnen Dateinamen jeweils durch ein einzelnes Leerzeichen. 

### <a name="url-path"></a>URL-Pfad

**Erforderliche Felder**

Operator | Wert | Umwandlung der Groß-/Kleinschreibung
---------|-------|---------------
[Liste der Standardoperatoren](#standard-operator-list) | String, Int | Keine Umwandlung, in Großbuchstaben, in Kleinbuchstaben

Wichtige Informationen:

- Für Dateinamen können Platzhalterwerte verwendet werden. Beispielsweise kann jedes Dateinamensmuster ein oder mehrere Sternchen (*) enthalten, wobei jedes Sternchen für ein Zeichen oder eine Folge von Zeichen steht.

## <a name="reference-for-rules-engine-match-conditions"></a>Referenz für Übereinstimmungsbedingungen der Regel-Engine

### <a name="standard-operator-list"></a>Liste der Standardoperatoren

Für Regeln, die die Liste der Standardoperatoren enthalten, sind die folgenden Operatoren gültig:

- Any
- Equals 
- Contains 
- Beginnt mit 
- Endet mit 
- Kleiner als
- Kleiner als oder gleich
- Größer als
- Größer als oder gleich
- Keine
- Enthält nicht
- Beginnt nicht mit 
- Endet nicht mit 
- Nicht kleiner als
- Nicht kleiner als oder gleich
- Nicht größer als
- Nicht größer als oder gleich

Für numerische Operatoren wie „Kleiner als“ oder „Größer als oder gleich“ basiert der verwendete Vergleich auf der Länge. In diesem Fall muss der Wert in der Übereinstimmungsbedingung eine ganze Zahl sein, die der Länge entspricht, die Sie vergleichen möchten. 

---

[Nach oben](#match-conditions)

</br>

## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über Azure Content Delivery Network](cdn-overview.md)
- [Regel-Engine – Referenz](cdn-standard-rules-engine-reference.md)
- [Aktionen der Regel-Engine](cdn-standard-rules-engine-actions.md)
- [Erzwingen von HTTPS mithilfe der Standardregel-Engine](cdn-standard-rules-engine.md)
