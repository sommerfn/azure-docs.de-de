---
title: Neuigkeiten – Language Understanding (LUIS)
titleSuffix: Azure Cognitive Services
description: In diesem Artikel werden Neuerungen zu Language Understanding beschrieben.
author: diberry
manager: nitinme
ms.custom: experiment-luis-0519
services: cognitive-services
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: diberry
ms.openlocfilehash: 0788df1c05d70c01d4de7f7a6ba62a476252f75a
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73498926"
---
# <a name="whats-new-in-language-understanding"></a>Neuerungen in Language Understanding

Informieren Sie sich über die Neuerungen im Dienst. Dabei kann es sich um Versionshinweise, Videos, Blogbeiträge und andere Informationen handeln. Legen Sie ein Lesezeichen für diese Seite an, um über den Dienst auf dem Laufenden zu bleiben.  

## <a name="release-notes"></a>Versionshinweise 

### <a name="november-4-2019---ignite"></a>4\. November 2019 – Ignite

* Verbesserte Produktivität von Entwicklern
    * Allgemeine Verfügbarkeit unseres [Vorhersageendpunkts V3](luis-migration-api-v3.md). 
    * Möglichkeit zum Importieren und Exportieren von Apps mit dem LU-Format ([LUDown](https://github.com/microsoft/botbuilder-tools/tree/master/packages/Ludown)). Dies ebnet den Weg für einen effektiven CI/CD-Prozess. 
* Spracherweiterung
    * [Arabisch und Hindi](luis-language-support.md) in der öffentlichen Vorschau.
* Vordefinierte Modelle
    * [Vordefinierte Domänen](luis-reference-prebuilt-domains.md) sind jetzt allgemein verfügbar (GA)
    * Japanische [vordefinierte Entitäten](luis-reference-prebuilt-entities.md#japanese-entity-support) Alter, Währung, Anzahl, Prozentsatz: werden in V3 nicht unterstützt.
    * Italienische [vordefinierte Entitäten](luis-reference-prebuilt-entities.md#italian-entity-support) Alter, Währung, Dimension, Anzahl, Prozentsatz: die Auflösung wurde gegenüber V2 geändert.
* Verbesserte Benutzeroberfläche in [preview.luis.ai portal](https://preview.luis.ai): neu gestaltete Bezeichungsoberfläche, um das Erstellen und Debuggen komplexer Modelle zu ermöglichen.
* Erweiterte Funktionen zum Sprachverständnis: [Erstellen ausgereifter Sprachmodelle](luis-concept-entity-types.md) mit weniger Aufwand. 
* Definition von Machine Learning-Funktionen auf Modellebene und Ermöglichen der Verwendung von Modellen als Signale für andere Modelle, wie das Verwenden von Entitäten als Features für Absichten und für andere Entitäten.
* Neue, erweiterte [Grenzwerte](luis-boundaries.md): höheres Maximum für Ausdruckslisten und ganze Ausdrücke, neues Modell als Featuregrenzwert
* Extrahieren von Informationen aus Text im Format einer tiefen Hierarchiestruktur, wodurch Dialoganwendungen noch leistungsfähiger werden.

    ![Abbildung einer durch maschinelles Lernen erworbenen Entität](./media/whats-new/deep-entity-extraction-example.png)

### <a name="september-3-2019"></a>3\. September 2019

* Azure-Erstellungsressource: [jetzt migrieren](luis-migration-authoring.md).
    * 500 Apps pro Azure-Ressource
    * 100 Versionen pro App
* Unterstützung für vordefinierte Entitäten in Türkisch
* Unterstützung für datetimeV2 in Italienisch

### <a name="july-23-2019"></a>23. Juli 2019

* Aktualisierung von [Recognizers-Text](https://github.com/microsoft/Recognizers-Text/releases/tag/dotnet-v1.2.3) auf 1.2.3
    * Alters-, Temperatur-, Dimensions-und Währungserkennungen auf Italienisch.
    * Verbesserung der Feiertagserkennung auf Englisch zur richtigen Berechnung von Datumsangaben, die auf Thanksgiving basieren.
    * Verbesserungen bei DateTime auf Französisch, um falsche positive Ergebnisse von anderen Entitäten als Datum und Uhrzeit zu verringern.
    * Unterstützung für Kalender-/Schul-/Geschäftsjahr und Akronyme bei DateRange auf Englisch.
    * Verbesserte PhoneNumber-Erkennung auf Chinesisch und Japanisch.
    * Verbesserte Unterstützung für NumberRange auf Englisch.
    * Leistungsverbesserungen.

### <a name="june-24-2019"></a>24. Juni 2019

* [Vordefinierte OrdinalV2-Entität](luis-reference-prebuilt-ordinal-v2.md) zur Unterstützung der Sortierung, z. B. „Nächste“, „Vorherige“ und „Letzte“. Gilt nur für die englische Kultur.

### <a name="may-6-2019---build-conference"></a>6\. Mai 2019 – //Build-Konferenz

Die folgenden Features wurden bei der Build 2019-Konferenz veröffentlicht:

* [Migrationsanleitung für die Vorschauversion der V3-API](luis-migration-api-v3.md)
* [Verbessertes Analytics-Dashboard](luis-how-to-use-dashboard.md)
* [Verbesserte vordefinierte Domänen](luis-reference-prebuilt-domains.md) 
* [Entitäten vom Typ „dynamische Liste“](luis-migration-api-v3.md#dynamic-lists-passed-in-at-prediction-time)
* [Externe Entitäten](luis-migration-api-v3.md#external-entities-passed-in-at-prediction-time)

## <a name="blogs"></a>Blogs

[Bot Framework](https://blog.botframework.com/)

## <a name="videos"></a>Videos

### <a name="2019-build-videos"></a>Build 2019 – Videos

[How to use Azure Conversational AI to scale your business for the next generation (Bereiten Sie Ihr Unternehmen mit Azure Conversational AI auf die nächste Generation vor)](https://www.youtube.com/watch?v=_k97jd-csuk&feature=youtu.be)

## <a name="service-updates"></a>Dienstupdates

[Azure-Updateankündigungen für Cognitive Services](https://azure.microsoft.com/updates/?product=cognitive-services)
