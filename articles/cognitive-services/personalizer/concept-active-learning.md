---
title: 'Aktive und inaktive Ereignisse: Personalisierung'
titleSuffix: Azure Cognitive Services
description: ''
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: diberry
ms.openlocfilehash: aa6f53901f21dcb0726454d641a4a2a66007f9e0
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72429041"
---
# <a name="active-and-inactive-events"></a>Aktive und inaktive Ereignisse

Wenn Ihre Anwendung die Rank-API (Bewertungs-API) aufruft, erhalten Sie die Information, welche Aktion die Anwendung im Feld rewardActionId anzeigen soll.  Von diesem Moment an erwartet die Personalisierung einen Reward-Aufruf mit dieser Ereignis-ID. Der Relevanzpunktestand wird zum Trainieren des Modells verwendet, das für zukünftige Aufrufe der Bewertungs-API verwendet wird. Wenn für die Ereignis-ID kein Reward-Aufruf empfangen wird, wird eine Standardrelevanz angewendet. Standardrelevanzen werden im Azure-Portal eingerichtet.

In einigen Fällen muss die Anwendung möglicherweise Rank aufrufen, bevor sie weiß, ob das Ergebnis verwendet oder dem Benutzer angezeigt wird. Dies kann beispielsweise geschehen, wenn das Seitenrendering von beworbenen Inhalten mit einer Marketingkampagne überschrieben wird. Wenn das Ergebnis des Rank-Aufrufs nie verwendet wurde und der Benutzer es nie zu Gesicht bekommen hat, wäre es falsch, es überhaupt mit einer Relevanz zu trainieren, nicht einmal mit einer Nullrelevanz.
Dies geschieht normalerweise in diesen Fällen:

* Ein Element der Benutzeroberfläche wird vorab gerendert, dem Benutzer aber unter Umständen gar nicht angezeigt. 
* Ihre Anwendung führt eine vorausschauende Personalisierung durch, bei der Rangfolgeaufrufe mit weniger Echtzeitkontext verwendet werden, und die Ausgabe wird von der Anwendung unter Umständen gar nicht genutzt. 

In diesen Fällen besteht die richtige Verwendung der Personalisierung im Aufruf von Rank mit der Anforderung, dass das Ereignis _inaktiv_ sein muss. Die Personalisierung erwartet für dieses Ereignis keine Relevanz und wendet auch keine Standardrelevanz an. Wenn die Anwendung später in Ihrer Geschäftslogik die Informationen des Rangfolgeaufrufs verwendet, müssen Sie das Ereignis lediglich _aktivieren_. Von dem Moment an, da das Ereignis aktiv ist, erwartet die Personalisierung eine Relevanz für das Ereignis oder wendet eine Standardrelevanz an, wenn keine expliziten Aufrufe an die Reward API (Relevanz-API) erfolgen.

## <a name="get-inactive-events"></a>Abrufen inaktiver Ereignisse

Um das Training für ein Ereignis zu deaktivieren, rufen Sie Rank mit `learningEnabled = False` auf.

Das Lernen für ein inaktives Ereignis wird implizit aktiviert, wenn Sie für die Ereignis-ID eine Relevanz senden oder die `activate`-API aufrufen.

## <a name="learning-settings"></a>Lerneinstellungen

Die Lerneinstellungen bestimmen die spezifischen *Hyperparameter* des Modelltrainings. Zwei Modelle, die über die gleichen Daten verfügen, aber mit unterschiedlichen Einstellungen trainiert wurden, sind im Ergebnis verschieden.

### <a name="import-and-export-learning-policies"></a>Importieren und Exportieren von Lernrichtlinien

Sie können Lernrichtliniendateien über das Azure-Portal importieren und exportieren. Dadurch können Sie vorhandene Richtlinien speichern, testen, ersetzen und zur späteren Referenz und Überprüfung als Artefakte in Ihrer Quellcodeverwaltung archivieren.

### <a name="learning-policy-settings"></a>Lernrichtlinieneinstellungen

Die Einstellungen in der **Lernrichtlinie** sollten nicht geändert werden. Ändern Sie die Einstellungen nur, wenn Sie deren Auswirkungen auf die Personalisierung verstehen. Andernfalls können Nebenwirkungen auftreten (bis hin zum Verlust der Gültigkeit von Personalisierungsmodellen).

### <a name="comparing-effectiveness-of-learning-policies"></a>Vergleichen der Effektivität von Lernrichtlinien

Mithilfe von [Offlineauswertungen](concepts-offline-evaluation.md) können Sie das Abschneiden verschiedener Lernrichtlinien anhand von Vergangenheitsdaten in Personalisierungsprotokollen vergleichen.

[Laden Sie Ihre eigenen Lernrichtlinien hoch](how-to-offline-evaluation.md), um sie mit der aktuellen Lernrichtlinie zu vergleichen.

### <a name="discovery-of-optimized-learning-policies"></a>Ermittlung optimierter Lernrichtlinien

Mittels einer [Offlineauswertung](how-to-offline-evaluation.md) kann die Personalisierung eine besser optimierte Lernrichtlinie erstellen. Eine besser optimierte Lernrichtlinie zeichnet sich in einer Offlineauswertung durch eine höhere Relevanz aus und liefert bessere Ergebnisse, wenn sie online bei der Personalisierung verwendet wird.

Die erstellte optimierte Lernrichtlinie kann direkt auf die Personalisierung angewendet werden, um die aktuelle Richtlinie umgehend zu ersetzen. Alternativ können Sie sie zur weiteren Auswertung speichern und später entscheiden, ob Sie sie verwerfen, speichern oder anwenden möchten.
