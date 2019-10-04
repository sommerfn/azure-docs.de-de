---
title: Auswählen des Modus für die Bing-Spracheingabe | Microsoft-Dokumentation
titlesuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie den besten Erkennungsmodus in der Bing-Spracheingabe auswählen.
services: cognitive-services
author: nitinme
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: nitinme
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 1860acb571c837a5eb4c75be69a96d1b22682118
ms.sourcegitcommit: fbea2708aab06c19524583f7fbdf35e73274f657
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2019
ms.locfileid: "70965677"
---
# <a name="bing-speech-recognition-modes"></a>Erkennungsmodi für die Bing-Spracheingabe

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-bing-speech-api-deprecation-note.md)]

Die Bing-Spracherkennungs-APIs unterstützen mehrere Spracherkennungsmodi. Wählen Sie den Modus, in dem die besten Erkennungsergebnisse für Ihre Anwendung erzeugt werden.

| Mode | BESCHREIBUNG |
|---|---|
| *Interaktiv* | Erkennung vom Typ „Befehl und Steuerung“ für interaktive Benutzeranwendungsszenarien. Benutzer äußern kurze Ausdrücke als Befehle für eine Anwendung. |
| *Diktieren* | Fortlaufende Erkennung für Diktierszenarien. Benutzer äußern längere Sätze, die als Text angezeigt werden. Hierbei verwenden die Benutzer eine formellere Sprache. |
| *Unterhaltung* | Fortlaufende Erkennung zum Transkribieren von Unterhaltungen zwischen Menschen. Benutzer wählen eine weniger formelle Sprache und können zwischen längeren Sätzen und kürzeren Ausdrücken wechseln.

> [!NOTE]
> Diese Modi gelten bei Verwendung der [REST-APIs](../GetStarted/GetStartedREST.md). Die [Clientbibliotheken](../GetStarted/GetStartedClientLibraries.md) verwenden verschiedene Parameter, um den Erkennungsmodus anzugeben. Weitere Informationen finden Sie in der Clientbibliothek Ihrer Wahl.

Weitere Informationen finden Sie auf der Seite [Erkennungsmodi](../concepts.md#recognition-modes).
