---
title: Entwerfen von Azure-Funktionen für identische Eingaben
description: Erstellen idempotenter Azure-Funktionen
author: craigshoemaker
ms.author: cshoe
ms.date: 9/12/2019
ms.topic: article
ms.service: azure-functions
manager: gwallace
ms.openlocfilehash: 39e785a1ca7a158ddb90a3e6ba914582c405612a
ms.sourcegitcommit: 1752581945226a748b3c7141bffeb1c0616ad720
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/14/2019
ms.locfileid: "70997329"
---
# <a name="designing-azure-functions-for-identical-input"></a>Entwerfen von Azure-Funktionen für identische Eingaben

In einer ereignisgesteuerten und nachrichtenbasierten Architektur müssen identische Anforderungen akzeptiert werden können, ohne die Datenintegrität oder die Systemstabilität zu beeinträchtigen.

Stellen Sie sich zur Veranschaulichung die Ruftaste eines Aufzugs vor. Wenn Sie auf die Taste drücken, leuchtet sie auf, und der Aufzug wird in das entsprechende Stockwerk geschickt. Kurz darauf stellt sich eine andere Person neben Sie. Diese Person lächelt sie an und drückt die leuchtende Taste ein zweites Mal. Sie lächeln zurück und denken im Stillen, dass das Rufen eines Aufzugs idempotent ist.

Auch wenn die Ruftaste ein zweites, drittes oder viertes Mal gedrückt wird, bleibt das Endergebnis stets gleich: Nach dem Drücken der Taste wird der Aufzug in das entsprechende Stockwerk geschickt. Dabei spielt es keine Rolle, wie oft die Taste gedrückt wird. Bei idempotenten Systemen (etwa bei einem Aufzug) ist das Ergebnis immer gleich – unabhängig davon, wie oft identische Befehle ausgegeben werden.

Machen Sie sich im Zusammenhang mit der App-Erstellung Gedanken über folgende Szenarien:

- Was passiert, wenn Ihre Bestandsführungsanwendung versucht, das gleiche Produkt mehrmals zu löschen?
- Wie verhält sich Ihre Personalanwendung, wenn sie mehrere Anforderungen zur Erstellung eines Mitarbeiterdatensatzes für die gleiche Person erhält?
- Wohin geht das Geld, wenn Ihre Banking-App 100 Anforderungen für die gleiche Abhebung erhält?

Es gibt zahlreiche Situationen, in denen Anforderungen für eine Funktion identische Befehle erhalten können. Im Anschluss folgen einige Beispiele:

- Wiederholungsrichtlinien, die mehrmals die gleiche Anforderung senden
- Zwischengespeicherte Befehle, die für die Anwendung wiedergegeben werden
- Anwendungsfehler, durch die mehrere identische Anforderungen gesendet werden

Die Logik in einer idempotenten Anwendung kann folgende Verhaltensweisen beinhalten, um die Integrität der Daten und des Systems zu schützen:

- Überprüfung des Vorhandenseins von Daten vor einem Löschvorgang
- Überprüfung des Vorhandenseins von Daten vor einer Erstellungsaktion
- Abstimmungslogik zur Erzielung von letztlicher Datenkonsistenz
- Parallelitätskontrollen
- Duplikaterkennung
- Überprüfung der Datenaktualität
- Schutzlogik zur Überprüfung von Eingabedaten

Idempotenz wird letztendlich erzielt, indem sichergestellt wird, dass eine bestimmte Aktion möglich ist und nur einmal ausgeführt wird.
