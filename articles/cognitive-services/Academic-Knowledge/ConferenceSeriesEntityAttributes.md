---
title: Attribute der Konferenzreihenentität – Academic Knowledge-API
titlesuffix: Azure Cognitive Services
description: Erfahren Sie mehr über die Attribute, die Sie für die Konferenzreihenentität verwenden können.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ROBOTS: NOINDEX
ms.openlocfilehash: feed324202f6a75ceb7e9089875b899c51cd8ae6
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2019
ms.locfileid: "68705041"
---
# <a name="conference-series-entity"></a>Konferenzreihenentität

<sub> * Die folgenden Attribute sind spezifisch für die Konferenzreihenentität. (Ty = '3') </sub>

NAME    |BESCHREIBUNG                            |type       | Vorgänge
------- | ------------------------------------- | --------- | ----------------------------
Id      |Entitäts-ID                              |Int64      |Equals
CN      |Normalisierter Name der Konferenzreihe      |Zeichenfolge     |Equals
DCN     |Anzeigename der Konferenzreihe         |Zeichenfolge     |none
CC      |Gesamtzitatanzahl der Konferenzreihe         |Int32      |none  
ECC     |Geschätzte Gesamtzitatanzahl der Konferenzreihe   |Int32      |none
F.FId   |Die mit der Konferenzreihe verknüpfte Entitäts-ID des Forschungsbereichs |Int64  | Equals
F.FN    |Der mit der Konferenzreihe verknüpfte Name des Forschungsbereichs  | Equals,<br/>StartsWith
