---
title: Worum handelt es sich bei Custom Decision Service?
titlesuffix: Azure Cognitive Services
description: Dieser Artikel bietet eine Übersicht über den Custom Decision Service.
services: cognitive-services
author: alekh
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-decision-service
ms.topic: overview
ms.date: 05/08/2018
ms.author: slivkins
ROBOTS: NOINDEX
ms.openlocfilehash: 073b2bf6df21e05481cde043d8ddcfd49822e94f
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2019
ms.locfileid: "68704242"
---
# <a name="what-is-custom-decision-service"></a>Worum handelt es sich bei Custom Decision Service?

In einer typischen Web- oder Mobilanwendung, enthält eine Titelseite Links zu mehreren Artikeln oder anderen Inhaltstypen. Wenn die Titelseite geladen wird, kann sie bei Custom Decision Service anfordern, Artikel zum Einschließen auf dieser Titelseite mit einem Rang zu bewerten. Wenn ein Benutzer durch Klicken einen Artikel auswählt, kann so eine zweite Anforderung an Custom Decision Service gesendet werden, der das Ergebnis dieser Entscheidung des Benutzers protokolliert.

Custom Decision Service ist einfach zu verwenden, da er nur einen RSS-Feeds für Ihre Inhalte sowie einige Zeilen JavaScript erfordert, die Ihrer Anwendung hinzugefügt werden müssen.

Custom Decision Service konvertiert Ihre Inhalte in Features für maschinelles Lernen. Das System verwendet diese Features, um Ihre Inhalte im Hinblick auf Text, Bilder, Videos und die allgemeine Stimmung zu verstehen. Es verwendet verschiedene weitere [Microsoft Cognitive Services](https://www.microsoft.com/cognitive-services), z.B. [Entitätsverknüpfung](../entitylinking/home.md), [Textanalyse](../text-analytics/overview.md), [Emotionen](../emotion/home.md) und [Maschinelles Sehen](../computer-vision/home.md).

Einige häufige Anwendungsfälle für Custom Decision Service sind:

* Personalisieren von Artikeln auf einer Nachrichtenwebsite
* Personalisieren von Videoinhalten in einem Medienportal
* Optimieren von Anzeigen oder der Webseiten, zu denen eine Anzeige weiterleitet
* Zuweisen eine Rangs für die empfohlenen Elemente auf einer Verkaufswebsite

Custom Decision Service befindet sich derzeit in der *kostenlosen öffentlichen Vorschau*. Er kann eine Liste von Artikeln auf einer Website oder in einer App personalisieren. Die Featureextraktion funktioniert am besten für Inhalte in englischer Sprache. Für andere Sprachen, z.B. Spanisch, Französisch, Deutsch, Portugiesisch und Japanisch, wird eine [eingeschränkte Funktionalität](../text-analytics/overview.md) angeboten. Diese Dokumentation wird überarbeitet, sobald neue Funktionen zur Verfügung stehen.

Custom Decision Service kann in Anwendungen verwendet werden, die sich nicht in der Inhaltspersonalisierungsdomäne befinden. Diese Anwendungen sind möglicherweise gut für eine benutzerdefinierte Vorschau geeignet. [Kontaktieren Sie uns](https://azure.microsoft.com/overview/sales-number/), um mehr zu erfahren.

## <a name="api-usage-modes"></a>API-Nutzungsmodi

Custom Decision Service kann auf Webseiten und in mobilen Apps angewandt werden. Die APIs können aus einem Browser oder einer App aufgerufen werden. Die API-Nutzung ist bei beiden ähnlich, einige Details unterscheiden sich jedoch.

## <a name="glossary-of-terms"></a>Glossar

In dieser Dokumentation treten mehrere Begriffe häufig auf:

* **Aktionssatz**: Der Satz von Inhaltselementen, denen von Custom Decision Service ein Rang zugewiesen werden soll. Dieser Satz kann als *RSS*- oder *Atom*-Endpunkt angegeben werden.
* **Rangfolge**: In jeder Anforderung an Custom Decision Service wird mindestens ein Aktionssatz angegeben. Das System reagiert durch Auswahl aller Inhaltsoptionen aus diesen Sätzen und Rückgabe in der priorisierten Reihenfolge.
* **Rückruffunktion**: Diese von Ihnen angegebene Funktion rendert den Inhalt auf der Benutzeroberfläche. Der Inhalt ist nach der Rangfolge sortiert, die von Custom Decision Service zurückgegeben wurde.
* **Relevanz**: Dieser Wert gibt an, wie der Benutzer auf den gerenderten Inhalt reagiert hat. Custom Decision Service misst die Benutzerreaktion anhand von Klicks. Die Klicks werden an das System gemeldet, indem benutzerdefinierter Code in Ihre Anwendung eingefügt wird.

## <a name="next-steps"></a>Nächste Schritte

* [Registrieren Ihrer Anwendung](custom-decision-service-get-started-register.md) bei Custom Decision Service
* Erste Schritte zum Optimieren einer [Webseite](custom-decision-service-get-started-browser.md) oder einer [Smartphone-App](custom-decision-service-get-started-app.md).
* Weitere Informationen zu den bereitgestellten Funktionen finden Sie in der [API-Referenz](custom-decision-service-api-reference.md).
