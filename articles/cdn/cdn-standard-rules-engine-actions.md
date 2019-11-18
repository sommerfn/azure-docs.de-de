---
title: Aktionen der Microsoft Standardregel-Engine für Azure CDN | Microsoft-Dokumentation
description: Referenzdokumentation zu den Aktionen der Standardregel-Engine für Azure CDN.
services: cdn
author: mdgattuso
ms.service: azure-cdn
ms.topic: article
ms.date: 11/01/2019
ms.author: magattus
ms.openlocfilehash: dbde93cc7ffd21e341653407e6e4f910e4620974
ms.sourcegitcommit: b2fb32ae73b12cf2d180e6e4ffffa13a31aa4c6f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73615849"
---
# <a name="azure-cdn-from-microsoft-standard-rules-engine-actions"></a>Aktionen der Microsoft Standardregel-Engine für Azure CDN

Dieser Artikel enthält ausführliche Beschreibungen der verfügbaren Aktionen der Microsoft [Standardregel-Engine](cdn-standard-rules-engine.md) für das Azure Content Delivery Network (CDN).

Der zweite Teil einer Regel ist eine Aktion. Eine Aktion definiert das Verhalten, das auf den Anforderungstyp angewendet wird, der anhand verschiedener Übereinstimmungsbedingungen bestimmt wurde.

## <a name="actions"></a>Aktionen

Die folgenden Aktionen können verwendet werden. 

## <a name="cache-expiration"></a>Cacheablauf

Mit dieser Aktion können Sie die Gültigkeitsdauer (TTL) des Endpunkts für Anforderungen überschreiben, die von den Bedingungen für die Regelübereinstimmung angegeben werden.

**Erforderliche Felder**

Cacheverhalten |                
---------------|----------------
Bypass Cache | Wenn diese Option ausgewählt ist und die Regel übereinstimmt, wird der Inhalt nicht zwischengespeichert.
Überschreiben | Wenn diese Option ausgewählt ist und die Regel übereinstimmt, wird der vom Ursprung zurückgegebene TTL-Wert mit dem in der Aktion angegebenen Wert überschrieben.
Bei Fehlen festlegen | Wenn diese Option ausgewählt ist, die Regel übereinstimmt und vom Ursprung kein TTL-Wert zurückgegeben wurde, legt die Regel den TTL-Wert auf den in der Aktion angegebenen Wert fest.

**Zusätzliche Felder**

Days | Stunden | Minuten | Sekunden
-----|-------|---------|--------
Int | Int | Int | Int 

## <a name="cache-key-query-string"></a>Abfragezeichenfolge für Cacheschlüssel

Mit dieser Aktion können Sie den Cacheschlüssel basierend auf Abfragezeichenfolgen ändern.

**Erforderliche Felder**

Verhalten | BESCHREIBUNG
---------|------------
Include | Wenn diese Option ausgewählt ist und die Regel übereinstimmt, werden die in den Parametern angegebenen Abfragezeichenfolgen beim Generieren des Cacheschlüssels einbezogen. 
Zwischenspeichern jeder eindeutigen URL | Wenn diese Option ausgewählt ist und die Regel übereinstimmt, erhält jede eindeutige URL ihren eigenen Cacheschlüssel. 
Exclude | Wenn diese Option ausgewählt ist und die Regel übereinstimmt, werden die in den Parametern angegebenen Abfragezeichenfolgen beim Generieren des Cacheschlüssels ausgeschlossen.
Ignorieren von Abfragezeichenfolgen | Wenn diese Option ausgewählt ist und die Regel übereinstimmt, werden Abfragezeichenfolgen beim Generieren des Cacheschlüssels ignoriert. 

## <a name="modify-request-header"></a>Anforderungsheader ändern

Diese Aktion ermöglicht das Ändern von Headern in den an ihren Ursprung gesendeten Anforderungen.

**Erforderliche Felder**

Aktion | HTTP-Headername | Wert
-------|------------------|------
Anfügen | Wenn diese Option ausgewählt ist und die Regel übereinstimmt, wird der im Headernamen angegebene Header mit dem angegebenen Wert der Anforderung hinzugefügt. Wenn der Header bereits vorhanden ist, wird der Wert an den vorhandenen Wert angefügt. | Zeichenfolge
Überschreiben | Wenn diese Option ausgewählt ist und die Regel übereinstimmt, wird der im Headernamen angegebene Header mit dem angegebenen Wert der Anforderung hinzugefügt. Wenn der Header bereits vorhanden ist, überschreibt der Wert den vorhandenen Wert. | Zeichenfolge
Löschen | Wenn diese Option ausgewählt ist, die Regel übereinstimmt und der in der Regel angegebene Header vorhanden ist, wird er aus der Anforderung gelöscht. | Zeichenfolge

## <a name="modify-response-header"></a>Antwortheader ändern

Diese Aktion ermöglicht das Ändern von Headern in den an Ihre Endclients zurückgegebenen Antworten.

**Erforderliche Felder**

Aktion | HTTP-Headername | Wert
-------|------------------|------
Anfügen | Wenn diese Option ausgewählt ist und die Regel übereinstimmt, wird der im Headernamen angegebene Header mit dem angegebenen Wert der Antwort hinzugefügt. Wenn der Header bereits vorhanden ist, wird der Wert an den vorhandenen Wert angefügt. | Zeichenfolge
Überschreiben | Wenn diese Option ausgewählt ist und die Regel übereinstimmt, wird der im Headernamen angegebene Header mit dem angegebenen Wert der Antwort hinzugefügt. Wenn der Header bereits vorhanden ist, überschreibt der Wert den vorhandenen Wert. | Zeichenfolge
Löschen | Wenn diese Option ausgewählt ist, die Regel übereinstimmt und der in der Regel angegebene Header vorhanden ist, wird er aus der Antwort gelöscht. | Zeichenfolge

## <a name="url-redirect"></a>URL Redirect

Mit dieser Aktion können Sie Endclients an eine neue URL umleiten. 

**Erforderliche Felder**

Feld | BESCHREIBUNG 
------|------------
type | Wählen Sie den Antworttyp aus, der an den Anforderer zurückgegeben wird. Die verfügbaren Optionen sind: 302 Gefunden, 301 Verschoben, 307 Temporäre Umleitung und 308 Permanente Umleitung
Protocol | Übereinstimmungsanforderung, HTTP oder HTTPS
Hostname | Wählen Sie den Hostnamen aus, an den die Anforderung umgeleitet wird. Lassen Sie das Feld leer, um den eingehenden Host beizubehalten.
`Path` | Definieren Sie den bei der Umleitung zu verwendenden Pfad. Lassen Sie das Feld leer, um den eingehenden Pfad beizubehalten.  
Abfragezeichenfolge | Definieren Sie die bei der Umleitung zu verwendende Abfragezeichenfolge. Lassen Sie das Feld leer, um die eingehende Abfragezeichenfolge beizubehalten. 
Fragment | Definieren Sie das bei der Umleitung zu verwendende Fragment. Lassen Sie das Feld leer, um das eingehende Fragment beizubehalten. 

Es wird dringend empfohlen, eine absolute URL zu verwenden. Bei Verwendung einer relativen URL werden CDN-URLs möglicherweise an einen ungültigen Pfad umgeleitet. 

## <a name="url-rewrite"></a>URL Rewrite

Mit dieser Aktion können Sie den Pfad einer Anforderung auf dem Weg zu Ihrem Ursprung neu schreiben.

**Erforderliche Felder**

Feld | BESCHREIBUNG 
------|------------
Quellmuster | Definieren Sie das Quellmuster im URL-Pfad, das ersetzt werden soll. Derzeit verwendet das Quellmuster eine auf Präfixen basierende Übereinstimmung. Verwenden Sie „/“ als Quellmusterwert, um alle URL-Pfade abzugleichen.
Destination | Definieren Sie den Zielpfad, der für das erneute Generieren verwendet werden soll. Dadurch wird das Quellmuster überschrieben.
Nicht übereinstimmenden Pfad beibehalten | Wenn „Ja“ ausgewählt ist, wird der verbleibende Pfad nach dem Quellmuster an den neuen Zielpfad angefügt. 


[Nach oben](#actions)

</br>

## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über Azure Content Delivery Network](cdn-overview.md)
- [Regel-Engine – Referenz](cdn-standard-rules-engine-reference.md)
- [Übereinstimmungsbedingungen der Regel-Engine](cdn-standard-rules-engine-match-conditions.md)
- [Erzwingen von HTTPS mithilfe der Standardregel-Engine](cdn-standard-rules-engine.md)
