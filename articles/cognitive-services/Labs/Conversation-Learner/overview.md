---
title: Was ist das Unterhaltungslernmodul? – Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Hier erhalten Sie Informationen zum Unterhaltungslernmodul und zu dessen Funktionsweise.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 725a1d3628fb320a58e073fe2d825af23b02c0bd
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2019
ms.locfileid: "68707022"
---
# <a name="what-is-conversation-learner"></a>Was ist das Unterhaltungslernmodul?

Mit dem Unterhaltungslernmodul können Sie Unterhaltungsschnittstellen erstellen und trainieren, die anhand von Beispielinteraktionen lernen. 

Im Gegensatz zu herkömmlichen Konzepten berücksichtigt das Unterhaltungslernmodul den End-to-End-Kontext eines Dialogs, um die Antworten zu verbessern und überzeugendere Benutzererlebnisse zu ermöglichen. Conversation Learner umfasst eine breite Palette von aufgabenorientierten Anwendungsfällen und wendet maschinelles Lernen im Hintergrund an, um dafür zu sorgen, dass Bots und intelligente Agents die Benutzer weniger frustrieren, keine zusätzlichen Kundendienstkosten verursachen und eine intuitivere Interaktion ermöglichen.

Entwickler geben zunächst Prototypdialoge ein, die sie imitieren möchten. Das Modell lernt anhand weiterer eingegebener Dialoge. Sobald das Modell gut funktioniert, kann der Bot für Endbenutzer bereitgestellt werden. Conversation Learner protokolliert Unterhaltungen mit Benutzern, und der Entwickler kann diese Unterhaltungen überprüfen. Wenn Fehler entdeckt werden, kann der Entwickler direkt eine Korrektur vornehmen. Das Modell wird neu trainiert und steht sofort wieder zur Verfügung.

Dieser Ansatz reduziert die manuelle Codierung der Dialogsteuerungslogik und ermöglicht Geschäftsinhabern oder Domänenexperten die Mitwirkung an einer Unterhaltungsschnittstelle ohne vorherige Kenntnisse des maschinellen Lernens. Unabhängig davon, ob das Unterhaltungslernmodul als Teil eines Bot, eines intelligenten Geräts oder eines intelligenten Agents bereitgestellt wird, kann es neue Fähigkeiten, Verhaltensweisen oder Kompetenzen schnell iterieren und deren Qualität schnell verbessern. 

Das Unterhaltungslernmodul ermöglicht es Entwicklern, die Markteinführung zu beschleunigen und erfolgreiche Dialoge über mehrere Unterhaltungskanäle über das Microsoft Bot Framework oder eigenständig über die eigene Infrastruktur zu führen.

Zusammenfassung und Highlights:

- Das Unterhaltungslernmodul bietet eine erstklassige KI-Methode der Erstellung aufgabenorientierter Bots.

- Es basiert auf einem wiederkehrenden neuronalen End-to-End-Netzwerk (LSTM) und lernt direkt anhand von Unterhaltungsbeispielen mit mehreren Sequenzen. 

- Es ermöglicht Designern, Entwicklern, Geschäftsbenutzern und Call Center-Mitarbeitern das Erstellen und Verwalten von Bots. 

- Es bietet die Möglichkeit, Geschäftsregeln und gesunden Menschenverstand in Code auszudrücken.

- In den Trainingssitzungen wird das neuronale Netzwerkmodell verwendet, um den nächsten Satz von erwarteten Aktionen in der Unterhaltung zu bewerten. Der Botentwickler kann dann die richtige Aktion auswählen und das Netzwerk trainieren, um die richtige Antwort bereitzustellen.
 
- Nach Abschluss des Trainings kann der Entwickler anhand der protokollierten Dialoge der Benutzerinteraktionen Korrekturen an den Botantworten vornehmen und das Modell neu trainieren. 

- Zur Ausführung der Aufgaben können domänenspezifische und Drittanbieter-APIs aufgerufen werden.

