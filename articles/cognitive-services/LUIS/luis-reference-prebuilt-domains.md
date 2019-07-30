---
title: Vordefinierter Domänenverweis
titleSuffix: Azure
description: Referenz für die vordefinierten Domänen, die vordefinierte Sammlungen von Absichten und Entitäten aus LUIS (Language Understanding Intelligent Services) darstellen.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: b615185472ede25fd6ceef9cc3e7325375e16ac7
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2019
ms.locfileid: "67846679"
---
# <a name="prebuilt-domain-reference-for-your-luis-app"></a>Vordefinierter Domänenverweis für die LUIS-App
Diese Referenz enthält Informationen über die [vordefinierten Domänen](luis-how-to-use-prebuilt-domains.md), bei denen es sich um vordefinierte Sammlungen der von LUIS gebotenen Absichten und Entitäten handelt.

[Benutzerdefinierte Domänen](luis-how-to-start-new-app.md) weisen im Gegensatz dazu anfänglich keine Absichten und Modelle auf. Sie können einem benutzerdefinierten Modell beliebige Absichten und Entitäten aus einer vordefinierten Domäne hinzufügen.

# <a name="supported-domains-across-cultures"></a>Unterstützte kulturübergreifende Domänen

Die einzige unterstützte Kultur ist Englisch. 

<!--

The table below summarizes the currently supported domains. Support for English is usually more complete than others.


| Entity Type       | EN-US      | ZH-CN   | DE    | FR     | ES    | IT      | PT-BR |  JP  |      KO |        NL |    TR |
|:-----------------:|:-------:|:-------:|:-----:|:------:|:-----:|:-------:| :-------:| :-------:| :-------:| :-------:|  :-------:| 
| Calendar    | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | -      | -    | -    | -     | -  |
| Communication   | ✓    | -       | ✓    | ✓     | ✓     | ✓  | -  | -      | -    | -    | -     | -  |
| Email           | ✓    | ✓       | ✓   | ✓     | ✓     | ✓  | -  | -      | -    | -    | -     | -  |
| HomeAutomation           | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | -  | -      | -    | -    | -     | -  |
| Notes      | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | -  | -      | -    | -    | -     | -  |
| Places    | ✓    | -       | ✓    | ✓     | ✓     | ✓  | -  | -      | -    | -    | -     | -  |
| RestaurantReservation   | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | -  | -      | -    | -    | -     | -  |
| ToDo     | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | -  | -      | -    | -    | -     | -  |
| ToDo_IPA        | ✓    | ✓       | ✓    | ✓      | ✓     | ✓       | -  | -      | -    | -    | -     | -  |
| Utilities          | ✓    | ✓        | ✓    | ✓      | ✓     | ✓       | -  | -      | -    | -    | -     | -  |
| Weather        | ✓    | ✓        | ✓    | ✓      | ✓     | ✓       | -  | -      | -    | -    | -     | -  |
| Web    | ✓    | -        | ✓    | ✓      | ✓     | ✓       | -  | -      | -    | -    | -     | -  |
||||||||||||| 

-->

<br><br>

|Entitätstyp|description|
|--|--|
|Kalender|Der Kalender enthält alle persönliche Besprechungen und Termine, aber _keine_ öffentlichen Ereignisse (z. B. Zeitpläne für Weltmeisterschaften, den Veranstaltungskalender für Seattle) oder generischen Kalender (z. B. den aktuellen Tag, den Herbstanfang oder den Tag der Arbeit).|
|Kommunikation|Anforderungen für Aufrufe, das Senden von SMS- oder Sofortnachrichten, das Suchen und Hinzufügen von Kontakten und verschiedene andere kommunikationsbezogene Anfragen (im Allgemeinen ausgehend). Abfragen, die sich ausschließlich auf den Kontaktnamen beziehen, gehören nicht in die Domäne Kommunikation.|
|E-Mail|Die E-Mail-Adresse ist eine Unterdomäne der Domäne Kommunikation. Sie enthält hauptsächlich Anforderungen zum Senden und Empfangen von Nachrichten über E-Mails.|
|HomeAutomation|Die HomeAutomation-Domäne stellt Absichten und Entitäten zum Steuern von Geräten im Smarthome bereit. Sie dient hauptsächlich der Unterstützung von Steuerbefehlen für die Beleuchtung oder Klimaanlagen, sie umfasst aber auch einige allgemeine Funktionen für andere elektrische Anwendungen.|
|Notizen|Die Domäne Notizen stellt Absichten und Entitäten für das Erstellen von Notizen und das Schreiben von Elementen für Benutzer bereit.|
|Orte|Zu Orten gehören Geschäfte, Institutionen, Restaurants, öffentliche Räume und Adressen. Die Domäne unterstützt das Suchen von Orten und die Abfrage von Informationen zu einem öffentlichen Ort wie dem Standort, den Öffnungszeiten oder der Entfernung.|
|RestaurantReservation|Die Domäne RestaurantReservation unterstützt Absichten für die Behandlung von Reservierungen für Restaurants.|
|ToDo|Die ToDo-Domäne stellt Typen von Aufgabenlisten für Benutzer bereit, damit diese Aufgaben hinzufügen, markieren und löschen können.|
|Versorgungsunternehmen|Die Hilfsdomäne ist eine allgemeine Domäne für alle vordefinierten LUIS-Modelle, die allgemeine Absichten und Äußerungen in unterschiedlichen Szenarien enthält.|
|Weather|Der Schwerpunkt der Domäne Wetter liegt auf der Überprüfung der Wetterbedingungen und auf Empfehlungen für Ort und Uhrzeit oder das Überprüfen der Zeit anhand von Wetterbedingungen.|
|Web|Die Domäne Web stellt Absichten und Entitäten für die Suche von Websites bereit.|
