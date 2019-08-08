---
title: Attribute der Journalentität – Academic Knowledge-API
titlesuffix: Azure Cognitive Services
description: Erfahren Sie mehr über die Attribute, die Sie für die Journalentität in der Academic Knowledge-API in Cognitive Services verwenden können.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ROBOTS: NOINDEX
ms.openlocfilehash: 93868bb014809693e7614e74bde7db864de95c04
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2019
ms.locfileid: "68704965"
---
# <a name="journal-entity"></a>Journalentität

<sub> * Die folgenden Attribute sind spezifisch für die Journalentität. (Ty = '2') </sub>

NAME    |BESCHREIBUNG                            |type       | Vorgänge
------- | ------------------------------------- | --------- | ----------------------------
Id      |Entitäts-ID                              |Int64      |Equals
DJN     |Normalisierter Journalname                |Zeichenfolge     |none
JN      |Journalanzeigename                   |Zeichenfolge     |Equals
CC      |Gesamtzitatanzahl des Journals           |Int32      |none  
ECC     |Geschätzte Gesamtzitatanzahl des Journals |Int32      |none
