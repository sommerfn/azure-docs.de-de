---
title: Attribute der Zuordnungsentität in der Academic Knowledge-API
titlesuffix: Azure Cognitive Services
description: Erfahren Sie mehr über die Attribute, die Sie für die Zuordnungsentität in der Academic Knowledge-API verwenden können.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ROBOTS: NOINDEX
ms.openlocfilehash: 333875472d9b859196c4d828061301b9d32c4d5a
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2019
ms.locfileid: "68705113"
---
# <a name="affiliation-entity"></a>Zuordnungsentität

<sub> * Die folgenden Attribute sind spezifisch für die Zuordnungsentität. (Ty = '5') </sub>

NAME    |BESCHREIBUNG                            |type       | Vorgänge
------- | ------------------------------------- | --------- | ----------------------------
Id      |Entitäts-ID                              |Int64      |Equals
AfN     |Normalisierter Name der Zuordnung        |Zeichenfolge     |Equals
DAfN    |Anzeigename der Zuordnung       |Zeichenfolge     |none
CC      |Gesamtzitatanzahl der Konferenzinstanz der Zuordnung           |Int32      |none  
ECC     |Geschätzte Gesamtzitatanzahl der Zuordnung |Int32      |none

## <a name="extended-metadata-attributes"></a>Erweiterte Metadatenattribute ##

NAME    | BESCHREIBUNG               
--------|---------------------------    
PC      |Dokumentanzahl der Zuordnung
