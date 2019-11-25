---
title: Konzepte – Azure Event Grid IoT Edge | Microsoft-Dokumentation
description: Konzepte in Event Grid in IoT Edge.
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/03/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 73309e10e88c11e639e6ac6fd3bb061e1b5c685b
ms.sourcegitcommit: 92d42c04e0585a353668067910b1a6afaf07c709
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/28/2019
ms.locfileid: "72991851"
---
# <a name="event-grid-concepts"></a>Event Grid-Konzepte

Dieser Artikel beschreibt die zentralen Begriffe in Azure Event Grid.

## <a name="events"></a>Events

Ein Ereignis ist die kleinste Informationsmenge, die einen Vorgang im System umfassend beschreibt. Jedes Ereignis enthält allgemeine Informationen wie Quelle des Ereignisses, Zeitpunkt, an dem das Ereignis aufgetreten ist, und den eindeutigen Bezeichner. Jedes Ereignis enthält auch spezielle Informationen, die nur für den jeweiligen Ereignistyp relevant sind. Die Unterstützung für ein Ereignis von einer Größe bis zu 1 MB ist derzeit in der Vorschauversion verfügbar.

Informationen zu den in ein Ereignis eingeschlossenen Ereignisse finden Sie unter [Azure Event Grid-Ereignisschema](event-schemas.md).

## <a name="publishers"></a>Herausgeber

Ein Herausgeber ist der Benutzer oder die Organisation, die Ereignisse an Event Grid sendet. Sie haben die Möglichkeit, Ereignisse über Ihre eigene Anwendung zu veröffentlichen.

## <a name="event-sources"></a>Ereignisquellen

Eine Ereignisquelle ist die Quelle, in der das Ereignis auftritt. Jede Ereignisquelle ist mit mindestens einem Ereignistyp verknüpft. Azure Storage ist z.B. die Ereignisquelle für durch Blobs erstellte Ereignisse. Ihre Anwendung ist die Ereignisquelle für benutzerdefinierte Ereignisse, die Sie definieren. Ereignisquellen sind dafür zuständig, Ereignisse an Event Grid zu senden.

## <a name="topics"></a>Themen

Das Event Grid-Thema stellt einen Endpunkt bereit, an den die Ereignisquelle Ereignisse sendet. Der Herausgeber erstellt das Thema und legt fest, ob eine Ereignisquelle ein Thema oder mehrere Themen benötigt. Ein Event Grid-Thema wird für eine Sammlung ähnlicher Ereignisse verwendet. Um auf bestimmte Arten von Ereignissen zu reagieren, legen Abonnenten fest, welche Themen sie abonnieren.

Beim Entwerfen Ihrer Anwendung können Sie flexibel entscheiden, wie viele Themen erstellt werden sollen. Erstellen Sie für große Lösungen ein benutzerdefiniertes Thema für jede Kategorie von verwandten Ereignissen. Denken Sie beispielsweise an eine Anwendung, die Ereignisse im Zusammenhang mit der Änderung von Benutzerkonten und der Verarbeitung von Bestellungen sendet. Es ist unwahrscheinlich, dass ein Ereignishandler beide Ereigniskategorien benötigt. Erstellen Sie zwei benutzerdefinierte Themen, und lassen Sie Ereignishandler das jeweils relevante Thema abonnieren. Für kleine Lösungen empfiehlt es sich ggf., alle Ereignisse an ein Thema zu senden. Ereignisabonnenten können nach den gewünschten Ereignistypen filtern.

Informationen zum Verwalten von Themen in Event Grid finden Sie unter [REST-API-Dokumentation](api.md).

## <a name="event-subscriptions"></a>Ereignisabonnements

Ein Abonnement informiert Event Grid über die Ereignisse, die Sie zu einem Thema erhalten möchten. Wenn Sie das Abonnement erstellen, stellen Sie einen Endpunkt für den Umgang mit dem Ereignis bereit. Sie können die Ereignisse filtern, die an den Endpunkt gesendet werden. 

Informationen zum Verwalten von Abonnements in Event Grid finden Sie unter [REST-API-Dokumentation](api.md).

## <a name="event-handlers"></a>Ereignishandler

In Bezug auf Event Grid ist ein Ereignishandler das Ziel, an den das Ereignis gesendet wird. Der Handler ergreift zur Verarbeitung des Ereignisses weitere Maßnahmen. Event Grid unterstützt verschiedene Handlertypen. Sie können einen unterstützten Azure-Dienst oder einen eigenen Webhook als Handler verwenden. Je nach Handlertyp übermittelt Event Grid die Ereignisse auf unterschiedliche Art und Weise. Wenn der Zieleventhandler ein HTTP-Webhook ist, wird das Ereignis wiederholt, wenn der Handler einen Statuscode von `200 – OK` zurückgibt. Der Edge Hub interpretiert das Ereignis, wenn es ohne eine Ausnahme übermittelt wird, als erfolgreich.

## <a name="security"></a>Sicherheit

Event Grid ermöglicht ein sicheres Abonnieren und Veröffentlichen von Themen. Weitere Informationen finden Sie unter [Event Grid – Sicherheit und Authentifizierung](security-authentication.md).

## <a name="event-delivery"></a>Ereignisbereitstellung

Wenn Event Grid nicht bestätigen kann, dass ein Ereignis beim Endpunkt des Abonnenten eingegangen ist, wird das Ereignis erneut übermittelt. Weitere Informationen finden Sie unter [Event Grid – Nachrichtenübermittlung und -wiederholung](delivery-retry.md).

## <a name="batching"></a>Batchverarbeitung

Wenn Sie ein benutzerdefiniertes Thema verwenden, müssen die Ereignisse immer in einem Array veröffentlicht werden. Bei Szenarios mit geringem Durchsatz hat das Array nur einen Wert. Für Anwendungsfälle mit hohem Volumen wird aber empfohlen, pro Veröffentlichung mehrere Ereignisse zu Batches zusammenzufassen, um eine höhere Effizienz zu erzielen. Batches können eine Größe von bis zu 1 MB haben. Für die einzelnen Ereignisse sollte trotzdem eine Größe von 1 MB (Preview) nicht überschritten werden.