---
title: 'Aktive und inaktive Ereignisse: Personalisierung'
titleSuffix: Azure Cognitive Services
description: In diesem Artikel wird beschrieben, wie Sie aktive und inaktive Ereignisse, Lerneinstellungen und Lernrichtlinien im Personalisierungsdienst verwenden.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: diberry
ms.openlocfilehash: 1641a1020193395d7d2ddb9c4893bd7bc89cdcd0
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73681859"
---
# <a name="active-and-inactive-events"></a>Aktive und inaktive Ereignisse

Wenn Ihre Anwendung die Rank-API aufruft, erhalten Sie die Angabe, welche Aktion die Anwendung im Feld **rewardActionId** anzeigen soll.  Von diesem Moment an erwartet die Personalisierung einen Reward-Aufruf mit dieser Ereignis-ID. Der Relevanzscore wird zum Trainieren des Modells für zukünftige Rank-Aufrufe verwendet. Wenn für die Ereignis-ID kein Reward-Aufruf empfangen wird, wird eine Standardrelevanz angewandt. Standardrelevanzen werden im Azure-Portal festgelegt.

In einigen Szenarien muss die Anwendung möglicherweise Rank aufrufen, bevor sie weiß, ob das Ergebnis verwendet oder dem Benutzer angezeigt wird. Dies kann beispielsweise geschehen, wenn das Seitenrendering von beworbenen Inhalten durch eine Marketingkampagne überschrieben wird. Wenn das Ergebnis des Rank-Aufrufs niemals verwendet und dem Benutzer niemals angezeigt wurde, übermitteln Sie keinen zugehörigen Reward-Aufruf.

In der Regel treten diese Szenarien in folgenden Fällen ein:

* Sie rendern vorab die Benutzeroberfläche, die dem Benutzer möglicherweise gar nicht angezeigt wird. 
* Ihre Anwendung führt eine vorausschauende Personalisierung durch, bei der Rank-Aufrufe mit wenig Echtzeitkontext ausgeführt werden, und die Ausgabe wird von der Anwendung unter Umständen gar nicht genutzt. 

In diesen Fällen verwenden Sie die Personalisierung im Aufruf von Rank mit der Anforderung, dass das Ereignis _inaktiv_ sein muss. Die Personalisierung erwartet für dieses Ereignis keine Relevanz und wendet auch keine Standardrelevanz an. Wenn die Anwendung später in Ihrer Geschäftslogik die Informationen des Rank-Aufrufs verwendet, müssen Sie das Ereignis lediglich _aktivieren_. Sobald das Ereignis aktiv ist, erwartet die Personalisierung eine Ereignisrelevanz. Wenn kein expliziter Aufruf der Reward-API erfolgt, wendet die Personalisierung eine Standardrelevanz an.

## <a name="inactive-events"></a>Inaktive Ereignisse

Um das Training für ein Ereignis zu deaktivieren, rufen Sie Rank mit `learningEnabled = False` auf. Für ein inaktives Ereignis wird das Lernen implizit aktiviert, wenn Sie für die Ereignis-ID eine Relevanz senden oder die `activate`-API aufrufen.

## <a name="learning-settings"></a>Lerneinstellungen

Die Lerneinstellungen legen die *Hyperparameter* des Modelltrainings fest. Zwei Modelle, die anhand der gleichen Daten mit unterschiedlichen Einstellungen trainiert werden, sind im Ergebnis verschieden.

### <a name="import-and-export-learning-policies"></a>Importieren und Exportieren von Lernrichtlinien

Sie können Lernrichtliniendateien über das Azure-Portal importieren und exportieren. Auf diese Weise können Sie vorhandene Richtlinien speichern, testen, ersetzen und zur späteren Referenz und Überprüfung als Artefakte in Ihrer Quellcodeverwaltung archivieren.

### <a name="understand-learning-policy-settings"></a>Grundlegendes zu Lernrichtlinieneinstellungen

Die Einstellungen in der Lernrichtlinie sollten nicht geändert werden. Ändern Sie die Einstellungen nur, wenn Sie genau wissen, wie sich die Änderungen auf die Personalisierung auswirken. Ohne dieses Wissen könnten Sie Probleme verursachen, einschließlich der Invalidierung von Personalisierungsmodellen.

### <a name="compare-learning-policies"></a>Vergleichen von Lernrichtlinien

Mithilfe von [Offlineauswertungen](concepts-offline-evaluation.md) können Sie das Abschneiden verschiedener Lernrichtlinien anhand von Vergangenheitsdaten in Personalisierungsprotokollen vergleichen.

[Laden Sie Ihre eigenen Lernrichtlinien hoch](how-to-offline-evaluation.md), um sie mit der aktuellen Lernrichtlinie zu vergleichen.

### <a name="optimize-learning-policies"></a>Optimieren von Lernrichtlinien

Mithilfe einer [Offlineauswertung](how-to-offline-evaluation.md) kann die Personalisierung eine optimierte Lernrichtlinie erstellen. Eine optimierte Lernrichtlinie zeichnet sich bei einer Offlineauswertung durch eine höhere Relevanz aus und liefert bessere Ergebnisse, wenn sie online bei der Personalisierung verwendet wird.

Nachdem Sie eine Lernrichtlinie optimiert haben, können Sie sie direkt auf die Personalisierung anwenden und damit die aktuelle Richtlinie sofort ersetzen. Sie können die optimierte Richtlinie aber auch zur weiteren Auswertung speichern und später entscheiden, ob sie verworfen, gespeichert oder angewandt werden soll.
