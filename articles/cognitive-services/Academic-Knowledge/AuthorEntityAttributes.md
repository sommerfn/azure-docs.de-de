---
title: Attribute der Autorentität – Academic Knowledge-API
titlesuffix: Azure Cognitive Services
description: Erfahren Sie mehr über die Attribute, die Sie für die Autorentität in der Academic Knowledge-API verwenden können.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ROBOTS: NOINDEX
ms.openlocfilehash: e63e9d3f8f17a2473caf3d31b83e318ddb132b43
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2019
ms.locfileid: "68705086"
---
# <a name="author-entity"></a>Autorentität
<sub> * Die folgenden Attribute sind spezifisch für die Autorentität. (Ty = '1') </sub>

NAME    |BESCHREIBUNG                            |type       | Vorgänge
------- | ------------------------------------- | --------- | ----------------------------
Id      |Entitäts-ID                              |Int64      |Equals
AuN     |Normalisierter Autorname                 |Zeichenfolge     |Equals
DAuN    |Autoranzeigename                    |Zeichenfolge     |none
CC      |Gesamtzitatanzahl des Autors            |Int32      |none  
ECC     |Geschätzte Gesamtzitatanzahl des Autors  |Int32      |none
E       |Erweiterte Metadaten (siehe Tabelle „Erweiterte Metadatenattribute“)  |Zeichenfolge     |none  


## <a name="extended-metadata-attributes"></a>Erweiterte Metadatenattribute ##

NAME    | BESCHREIBUNG               
--------|---------------------------    
LKA.Afn     | Der mit dem Autoren verknüpfte Anzeigename der Zuordnung  
LKA.AfId        | Die mit dem Autoren verknüpfte Entitäts-ID der Zuordnung
