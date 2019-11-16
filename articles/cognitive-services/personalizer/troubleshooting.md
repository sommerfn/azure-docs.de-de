---
title: 'Problembehandlung: Personalisierung'
titleSuffix: Azure Cognitive Services
description: Dieser Artikel enthält Antworten auf Fragen zur Behandlung von Problemen mit Personalizer.
author: diberry
manager: nitinme
services: cognitive-services
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 06/15/2019
ms.author: diberry
ms.openlocfilehash: 5911cba54d7dd0eb1a5621112b41e2fc40fa68eb
ms.sourcegitcommit: 44c2a964fb8521f9961928f6f7457ae3ed362694
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2019
ms.locfileid: "73953163"
---
# <a name="personalizer-troubleshooting"></a>Troubleshooting für Personalizer

Dieser Artikel enthält Antworten auf Fragen zur Behandlung von Problemen mit Personalizer.

## <a name="learning-loop"></a>Lernschleife

### <a name="the-learning-loop-doesnt-seem-to-learn-how-do-i-fix-this"></a>Die Lernschleife funktioniert anscheinend nicht. Wie kann ich dies korrigieren?

Die Lernschleife benötigt einige Tausend Belohnungsaufrufe, bevor Priorisierungsaufrufe effektiv priorisiert werden. 

Wenn Sie sich nicht sicher sind, wie Ihre Lernschleife sich derzeit verhält, führen Sie eine [Offlineauswertung](concepts-offline-evaluation.md) aus, und wenden Sie die korrigierte Lernrichtlinie an. 

## <a name="next-steps"></a>Nächste Schritte

[Configure the model update frequency (Ändern der Häufigkeit der Modellaktualisierung)](how-to-settings.md#model-update-frequency)