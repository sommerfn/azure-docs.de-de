---
title: Attribute der Forschungsbereichsentität – Academic Knowledge-API
titlesuffix: Azure Cognitive Services
description: Erfahren Sie mehr über die Attribute, die Sie für die Forschungsbereichsentität in der Academic Knowledge-API verwenden können.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/31/2017
ms.author: alch
ROBOTS: NOINDEX
ms.openlocfilehash: d54f3c72462e6702b09068092b7c18ea50f12048
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2019
ms.locfileid: "68704994"
---
# <a name="field-of-study-entity"></a>Forschungsbereichsentität

<sub> * Die folgenden Attribute sind spezifisch für die Forschungsbereichsentität. (Ty = '6') </sub>

NAME    |BESCHREIBUNG                            |type       | Vorgänge
------- | ------------------------------------- | --------- | ----------------------------
Id      |Entitäts-ID                              |Int64      |Equals
FN      |Normalisierter Name des Forschungsbereichs         |Zeichenfolge     |Equals
DFN     |Anzeigename des Forschungsbereichs            |Zeichenfolge     |none
CC      |Gesamtzitatanzahl des Forschungsbereichs    |Int32      |none  
ECC     |Geschätzte Gesamtzitatanzahl des Forschungsbereichs|Int32      |none
FL      |Ebene in der Hierarchie des Forschungsbereichs     |Int32      |Equals, <br/>IsBetween
FP.FN   |Name für übergeordneten Forschungsbereich             |Zeichenfolge     |Equals
FP.FId  |ID für übergeordneten Forschungsbereich               |Int64      |Equals
FC.FN   |Name für untergeordneten Forschungsbereich              |Zeichenfolge     |Equals
FC.FId  |ID für untergeordneten Forschungsbereich                |Int64      |Equals
