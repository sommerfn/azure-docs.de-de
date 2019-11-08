---
title: 'Sondieren: Personalisierung'
titleSuffix: Azure Cognitive Services
description: Durch das Sondieren kann die Personalisierung auch dann weiterhin gute Ergebnisse liefern, wenn sich das Benutzerverhalten ändert. Die Auswahl der Einstellung für das Sondieren ist eine geschäftliche Entscheidung über den Anteil der Benutzerinteraktionen für die Untersuchung, um das Modell zu verbessern.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: diberry
ms.openlocfilehash: 0b69c1fb070431ad61858322dce461f6496c35d7
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73490824"
---
# <a name="exploration-and-exploitation"></a>Sondieren und Einsetzen

Durch das Sondieren kann die Personalisierung auch dann weiterhin gute Ergebnisse liefern, wenn sich das Benutzerverhalten ändert.

Wenn die Personalisierung einen Aufruf zum Zuweisen eines Rangs empfängt, gibt sie eine RewardActionID zurück, für die eine der beiden folgenden Aussagen gilt:
* Das wahrscheinlichste Benutzerverhalten wird durch Einsetzen des aktuellen Machine Learning-Modells ermittelt.
* Das Verhalten wird durch Sondieren ermittelt. Dabei wird nicht zwangsläufig die Aktion gefunden, deren Rang die höchste Wahrscheinlichkeit aufweist.

Die Personalisierung verwendet für das Sondieren derzeit den *Epsilon-Greedy-Algorithmus*. 

## <a name="choosing-an-exploration-setting"></a>Auswählen einer Sondierungseinstellung

Sie konfigurieren den Prozentsatz des Datenverkehrs für die Sondierung im Azure-Portal auf der Seite **Konfiguration** für die Personalisierung. Diese Einstellung bestimmt den Prozentsatz von Rangaufrufen, bei denen eine Sondierung erfolgt. 

Die Personalisierung ermittelt dann anhand dieser Wahrscheinlichkeit bei jedem Priorisierungsaufruf, ob sie sondiert oder das derzeitige Modell einsetzt. Dies unterscheidet sich vom Verhalten bei einigen A/B-Frameworks, in denen eine Vorgehensweise für bestimmte Benutzer-IDs festgelegt ist.

## <a name="best-practices-for-choosing-an-exploration-setting"></a>Best Practices für das Auswählen einer Sondierungseinstellung

Die Auswahl der Einstellung für das Sondieren ist eine geschäftliche Entscheidung über den Anteil der Benutzerinteraktionen für die Untersuchung, um das Modell zu verbessern. 

Mit der Einstellung 0 (null) gehen viele Vorteile der Personalisierung verloren. Mit dieser Einstellung verwendet die Personalisierung keine Benutzerinteraktionen, um bessere Benutzerinteraktionen zu ermitteln. Dies führt zu Stagnation, Abweichung (Concept Drift) und letztlich zu einer geringeren Leistung des Modells.

Eine zu hohe Einstellung negiert die Vorteile des Lernens vom Benutzerverhalten. Die Festlegung auf 100 % bedeutet eine konstante Randomisierung, sodass gelerntes Verhalten von Benutzern keinen Einfluss auf das Ergebnis mehr hat.

Es ist wichtig, das Verhalten der Anwendung nicht zu ändern, wenn Sie erkennen, dass die Personalisierung sondiert oder einsetzt. Dies würde zu einer Beeinflussung des Lernens führen und damit letztendlich die potenzielle Leistung verringern.

## <a name="next-steps"></a>Nächste Schritte

[Vertiefendes Lernen](concepts-reinforcement-learning.md) 