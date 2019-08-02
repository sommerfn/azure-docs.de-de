---
title: Szenarioauswertung – Personalisierung
titleSuffix: Azure Cognitive Services
description: Personalisierung kann in jeder Situation angewendet werden, in der Ihre Anwendung das richtige Element oder Produkt bzw. die richtige Aktion auswählen kann, das/die angezeigt werden soll – um die Erfahrung zu verbessern, bessere Geschäftsergebnisse zu erzielen oder die Produktivität zu steigern.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: 465fba3a466aceaf9ef7b71e4b1957bfdbcad766
ms.sourcegitcommit: e3b0fb00b27e6d2696acf0b73c6ba05b74efcd85
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68663640"
---
# <a name="where-can-you-use-personalizer"></a>Wo können Sie Personalisierung verwenden?

Verwenden Sie Personalisierung in jeder Situation, in der Ihre Anwendung das richtige Element oder Produkt bzw. die richtige Aktion auswählen muss, das/die angezeigt werden soll – um die Erfahrung zu verbessern, bessere Geschäftsergebnisse zu erzielen oder die Produktivität zu steigern. 

Personalisierung verwendet maschinelles Lernen, um die dem Benutzer anzuzeigende Aktion auszuwählen. Die Auswahl kann drastisch variieren, abhängig von der Menge, Qualität und Verteilung der an den Dienst gesendeten Daten.

### <a name="checklist-for-applying-personalizer"></a>Checkliste für die Anwendung von Personalisierung


Sie können Personalisierung in folgenden Situationen anwenden:

* Sie haben einen Unternehmens- oder Benutzerfreundlichkeitsziel für Ihre Anwendung.
* Sie haben eine Stelle in Ihrer Anwendung, wo das Treffen einer kontextbezogenen Entscheidung, was den Benutzern angezeigt wird, dieses Ziel verbessern wird.
* Die beste Auswahl kann und sollte aus dem kollektiven Benutzerverhalten und der Belohnungsgesamtbewertung abgeleitet werden.
* Die Verwendung von maschinellem Lernen für die Personalisierung befolgt die [Leitlinien für die verantwortungsvolle Verwendung](ethics-responsible-use.md) und die von Ihnen ausgewählten Optionen.
* Die kontextbezogene Entscheidung kann als Rangfolge der besten Option (Aktion) aus einer begrenzten Anzahl von Auswahlmöglichkeiten ausgedrückt werden.
* Wie gut die nach Rang sortierte Auswahl für Ihre Anwendung funktioniert hat, kann bestimmt werden, indem ein Aspekt des Benutzerverhaltens gemessen und als _Belohnungsbewertung_ zum Ausdruck kommt. Dies ist eine Zahl zwischen -1 und 1.
* Die Belohnungsbewertung führt nicht zu viele störende oder externe Faktoren ein. Die Experimentdauer ist kurz genug, sodass die Belohnungsbewertung berechnet werden kann, während sie noch relevant ist.
* Sie können den Kontext für die Rangfolge als eine Liste von mindestens 5 [Merkmalen](concepts-features.md) ausdrücken, von denen Sie glauben, dass sie beim Treffen der richtigen Auswahl hilfreich sind, und die keine personenbezogenen Informationen enthalten (PII).
* Sie besitzen Informationen über jede Inhaltsauswahl, _Aktion_, als Liste von mindestens 5 [Merkmalen](concepts-features.md), von denen Sie glauben, dass sie der Personalisierung beim Treffen der richtigen Auswahl helfen werden.
* Ihre Anwendung kann Daten lange genug behalten, um einen Verlauf von mindestens 100.000 Interkationen zu sammeln.

## <a name="machine-learning-considerations-for-applying-personalizer"></a>Erwägungen zum maschinellen Lernen bei der Anwendung der Personalisierung

Personalisierung basiert auf vertiefendem Lernen, einem Ansatz für maschinelles Lernen, der über von Ihnen gegebenes Feedback gelernt wird. 

Personalisierung lernt in folgenden Situationen am besten:

* Es gibt genügend Ereignisse, um das Niveau optimaler Personalisierung halten zu können, wenn sich das Problem im Laufe der Zeit verschiebt (z. B. Vorlieben bei Nachrichten oder Mode). Personalisierung passt sich an kontinuierliche Änderungen in der realen Welt an, aber Ergebnisse werden nur dann optimal sein, wenn genügend Ereignisse und Daten zum Lernen vorhanden sind, um neue Muster zu ermitteln und sich auf diese einzustellen. Sie sollten einen Anwendungsfall auswählen, der häufig genug auftritt. Erwägen Sie, nach Anwendungsfällen zu suchen, die mindestens 500-mal am Tag auftreten.
* Kontext und Aktionen besitzen genug [Merkmale](concepts-features.md), um das Lernen zu fördern.
* Es gibt weniger als 50 Aktionen, denen pro Aufruf ein Rang zugewiesen werden muss.
* Ihre Datenaufbewahrungseinstellungen gestatten der Personalisierung das Sammeln ausreichender Daten, um Offlineauswertungen und Richtlinienoptimierungen durchzuführen. Hierbei handelt es sich normalerweise um mindestens 50.000 Datenpunkte.

## <a name="monitor-effectiveness-of-personalizer"></a>Überwachen der Effektivität der Personalisierung

Sie können die Wirksamkeit der Personalisierung in regelmäßigen Abständen überwachen, indem Sie [Offlineauswertungen](concepts-offline-evaluation.md) durchführen.

## <a name="use-personalizer-with-recommendation-engines"></a>Verwenden der Personalisierung mit Empfehlungs-Engines

Viele Unternehmen verwenden Empfehlungs-Engines, Marketing- und Kampagnentools, Benutzergruppensegmentierung und -clustering, kollaboratives Filtern und andere Mittel, um Kunden Produkte aus einem umfangreichen Katalog zu empfehlen.

Im [GitHub-Repository „Microsoft Recommenders“](https://github.com/Microsoft/Recommenders) finden Sie Beispiele und bewährte Methoden für den Aufbau von Empfehlungssystemen, bereitgestellt als Jupyter-Notebooks. Es stellt funktionierende Beispiele für die Vorbereitung von Daten, das Erstellen von Modellen, die Auswertung, die Optimierung und Operationalisierung der Empfehlungs-Engines für viele gängige Ansätze bereit, einschließlich xDeepFM, SAR, ALS, RBM, DKN.

Personalisierung kann mit einer Empfehlungs-Engine zusammenarbeiten, wenn sie vorhanden ist.

* Empfehlungs-Engines nutzen große Mengen von Elementen (z. B. 500.000) und empfehlen eine Teilmenge (z. B. die Top-20) von Hunderten oder Tausenden von Optionen.
* Personalisierung nutzt eine kleine Anzahl von Aktionen mit vielen Informationen über diese und weist diesen für einen vorgegebenen, umfangreichen Kontext in Echtzeit eine Rangfolge zu, während die meisten Empfehlungs-Engines nur ein paar Attribute von Benutzern, Produkten und deren Interaktionen verwenden.
* Personalisierung ist darauf ausgelegt, ständig Benutzervorlieben autonom zu untersuchen, was zu besseren Ergebnissen führt, wenn sich Inhalte schnell ändern, z. B. bei Nachrichten, Liveveranstaltungen, Liveinhalten von Communitys, Inhalten mit täglichen Aktualisierungen oder saisonalen Inhalten.

Ein gängiges Szenario besteht darin, die Ausgabe einer Empfehlungs-Engine zu nehmen (z. B. die Top-20 Produkte für einen bestimmten Kunden) und diese als Eingabeaktionen für die Personalisierung zu verwenden.

## <a name="next-steps"></a>Nächste Schritte

[Ethik und verantwortungsvolle Verwendung](ethics-responsible-use.md).