---
title: Einschränkungen und bekannte Probleme beim Import von APIs in Azure API Management | Microsoft-Dokumentation
description: Hier erhalten Sie Informationen zu bekannten Problemen und Einschränkungen beim Import in Azure API Management bei Verwendung der Formate Open API, WSDL oder WADL.
services: api-management
documentationcenter: ''
author: vladvino
manager: vlvinogr
editor: ''
ms.assetid: 7a5a63f0-3e72-49d3-a28c-1bb23ab495e2
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 06/26/2019
ms.author: apimpm
ms.openlocfilehash: 0bf52b45bc315874c58be50bccbffa49090d554d
ms.sourcegitcommit: 8b44498b922f7d7d34e4de7189b3ad5a9ba1488b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2019
ms.locfileid: "72294343"
---
# <a name="api-import-restrictions-and-known-issues"></a>Einschränkungen und bekannte Probleme beim Import von APIs

## <a name="about-this-list"></a>Informationen zu dieser Liste

Beim Importieren einer API stoßen Sie unter Umständen auf Einschränkungen oder Probleme, die behoben werden müssen, damit der Import erfolgreich ausgeführt werden kann. Diese Probleme werden nachfolgend unter dem jeweiligen Importformat der API beschrieben.

## <a name="open-api"> </a>OpenAPI/Swagger

Wenn Sie beim Importieren Ihres OpenAPI-Dokuments Fehlermeldungen erhalten, stellen Sie sicher, dass Sie das Dokument zuvor überprüft haben. Dazu können Sie entweder im Azure-Portal den Designer (Entwurf – Front-End – OpenAPI-Spezifikations-Editor) oder ein Tool eines Drittanbieters (z. B. <a href="https://editor.swagger.io">Swagger Editor</a>) verwenden.

### <a name="open-api-general"> </a>Allgemein

-   Erforderliche Parameter für Pfad und Abfrage müssen eindeutige Namen besitzen. (In OpenAPI muss ein Parametername nur an einem Ort (beispielsweise im Pfad, in der Abfrage oder im Header) eindeutig sein. In API Management können Vorgänge jedoch sowohl nach Pfad- als auch nach Abfrageparametern unterschieden werden (dies wird von OpenAPI nicht unterstützt). Daher müssen Parameternamen innerhalb der gesamten URL-Vorlage eindeutig sein.)
-   Mit **\$ref**-Zeigern kann nicht auf externe Dateien verwiesen werden.
-   **x-ms-paths** und **x-servers** sind die einzigen unterstützten Erweiterungen.
-   Benutzerdefinierte Erweiterungen werden beim Import ignoriert und nicht für den Export gespeichert oder aufbewahrt.
-   **Rekursion**: API Management unterstützt keine Definitionen, die rekursiv definiert sind (z. B. Schemas, die auf sich selbst verweisen).
-   Die Quelldatei-URL (sofern vorhanden) wird auf relative Server-URLs angewendet.
-   Sicherheitsdefinitionen werden ignoriert.
-   Inlineschemadefinitionen für API-Vorgänge werden nicht unterstützt. Schemadefinitionen werden im API-Bereich definiert, und auf sie kann in Anforderungs- oder Antwortbereichen von API-Vorgängen verwiesen werden.

### <a name="open-api-v2"> </a>OpenAPI, Version 2

-   Nur das JSON-Format wird unterstützt.

### <a name="open-api-v3"> </a>OpenAPI, Version 3

-   Wenn viele **Server** angegeben sind, versucht API Management, die erste HTTPS-URL auszuwählen. Wenn keine HTTPS-URLs vorhanden sind, wird die erste HTTP-URL ausgewählt. Wenn keine HTTP-URLs vorhanden sind, bleibt die Server-URL leer.
-   **Examples** wird nicht unterstützt, aber **example** wird unterstützt.
-   **Multipart/form-data** wird nicht unterstützt.

## <a name="wsdl"></a>WSDL

WSDL-Dateien werden verwendet, um SOAP-Passthrough- und SOAP-to-REST-APIs zu erstellen.

-   **SOAP-Bindungen**: Es werden nur SOAP-Bindungen unterstützt, die im Format von „document“- oder „literal“-Codierung vorliegen. Das „rpc“-Format und SOAP-Codierung werden nicht unterstützt.
-   **WSDL:Import**: Dieses Attribut wird nicht unterstützt. Kunden sollten die zu importierenden Elemente in einem Dokument zusammenführen.
-   **Mehrteilige Nachrichten**: Diese Art von Nachrichten wird nicht unterstützt.
-   **WCF wsHttpBinding**: Für SOAP-Dienste, die mit Windows Communication Foundation erstellt wurden, muss basicHttpBinding verwendet werden – wsHttpBinding wird nicht unterstützt.
-   **MTOM**: Dienste, die MTOM verwenden, funktionieren <em>möglicherweise</em>. Eine offizielle Unterstützung wird derzeit nicht angeboten.
-   **Rekursion**: Typen, die rekursiv definiert sind (beispielsweise auf ein eigenes Array verweisen), werden von APIM nicht unterstützt.
-   **Mehrere Namespaces**: In einem Schema können zwar mehrere Namespaces verwendet werden, aber nur der Zielnamespace kann zum Definieren von Nachrichtenteilen verwendet werden. Namespaces, die zum Definieren anderer Ein- oder Ausgabeelememente verwendet werden und bei denen es sich nicht um den Zielnamespace handelt, werden nicht beibehalten. Ein solches WSDL-Dokument kann zwar importiert werden, beim Exportieren haben jedoch alle Nachrichtenteile den WSDL-Zielnamespace.
-   **Arrays**: Bei der SOAP-to-REST-Transformation werden nur umschlossene Arrays unterstützt. Dies ist im Beispiel unten dargestellt:

```xml
    <complexType name="arrayTypeName">
        <sequence>
            <element name="arrayElementValue" type="arrayElementType" minOccurs="0" maxOccurs="unbounded"/>
        </sequence>
    </complexType>
    <complexType name="typeName">
        <sequence>
            <element name="element1" type="someTypeName" minOccurs="1" maxOccurs="1"/>
            <element name="element2" type="someOtherTypeName" minOccurs="0" maxOccurs="1" nillable="true"/>
            <element name="arrayElement" type="arrayTypeName" minOccurs="1" maxOccurs="1"/>
        </sequence>
    </complexType>
```

## <a name="wadl"></a>WADL

Derzeit sind keine Probleme beim Import im Format WADL bekannt.
