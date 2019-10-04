---
title: Attribute der Konferenzinstanzentität – Academic Knowledge-API
titlesuffix: Azure Cognitive Services
description: Erfahren Sie mehr über die Attribute, die Sie für die Konferenzinstanzentität in der Academic Knowledge-API verwenden können.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ROBOTS: NOINDEX
ms.openlocfilehash: 37a353fbb86ca199b2316dcfba5904f4b46b0276
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2019
ms.locfileid: "68705057"
---
# <a name="conference-instance-entity"></a>Konferenzinstanzentität

<sub> * Die folgenden Attribute sind spezifisch für die Konferenzinstanzentität. (Ty = '4') </sub>

NAME    |BESCHREIBUNG                            |type       | Vorgänge
------- | ------------------------------------- | --------- | ----------------------------
Id      |Entitäts-ID                              |Int64      |Equals
CIN     |Normalisierter Name der Konferenzinstanz ({ConferenceSeriesNormalizedName} {ConferenceInstanceYear})        |Zeichenfolge     |Equals
DCN     |Anzeigename der Konferenzinstanz ({ConferenceSeriesNormalizedName} {ConferenceInstanceYear})       |Zeichenfolge     |none
CIL     |Speicherort der Konferenzinstanz    |Zeichenfolge     |Equals,<br/>StartsWith
CISD    |Startdatum der Konferenzinstanz  |Date       |Equals,<br/>IsBetween
CIED    |Enddatum der Konferenzinstanz    |Date       |Equals,<br/>IsBetween
CIARD   |Fälligkeitsdatum für die Abstraktregistrierung der Konferenzinstanz  |Date       |Equals,<br/>IsBetween
CISDD   |Fälligkeitsdatum für die Übermittlung der Konferenzinstanz     |Date       |Equals,<br/>IsBetween
CIFVD   |Fälligkeitsdatum der endgültigen Version der Konferenzinstanz  |Date       |Equals,<br/>IsBetween
CINDD   |Benachrichtigungsdatum der Konferenzinstanz   |Date       |Equals,<br/>IsBetween
CD.T    |Titel des Konferenzinstanzereignisses   |Date       |Equals,<br/>IsBetween
CD.D    |Datum des Konferenzinstanzereignisses    |Date       |Equals,<br/>IsBetween
PCS.CN  |Name der Konferenzreihe der Instanz |Zeichenfolge     |Equals
PCS.CId |ID der Konferenzreihe der Instanz |Int64    |Equals
CC      |Gesamtzitatanzahl der Konferenzinstanz           |Int32      |none  
ECC     |Geschätzte Gesamtzitatanzahl der Konferenzinstanz |Int32      |none


## <a name="extended-metadata-attributes"></a>Erweiterte Metadatenattribute ##

NAME    | BESCHREIBUNG               
--------|---------------------------    
FN      | Vollständiger Name der Konferenzinstanz
