---
title: Problembehandlung
titleSuffix: Azure Cognitive Services
description: In diesem Artikel erhalten Sie Antworten auf Fragen zur Behandlung von Problemen mit Personalizer.
author: edjez
manager: nitinme
services: cognitive-services
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: article
ms.date: 06/15/2019
ms.author: edjez
ms.openlocfilehash: 5136bd295c12c4439a894b4dcf0b868d32ce43ca
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2019
ms.locfileid: "67312933"
---
# <a name="personalizer-troubleshooting"></a>Troubleshooting für Personalizer

Dieser Artikel enthält Antworten auf Fragen zur Behandlung von Problemen mit Personalizer.

## <a name="learning-loop"></a>Lernschleife

### <a name="the-learning-loop-doesnt-seem-to-learn-how-do-i-fix-this"></a>Die Lernschleife funktioniert anscheinend nicht. Wie kann ich dies korrigieren?

Die Lernschleife benötigt einige Tausend Belohnungsaufrufe, bevor Priorisierungsaufrufe effektiv priorisiert werden. 

Wenn Sie sich nicht sicher sind, wie Ihre Lernschleife sich derzeit verhält, führen Sie eine [Offlineauswertung](concepts-offline-evaluation.md) aus, und wenden Sie die korrigierte Lernrichtlinie an. 

## <a name="next-steps"></a>Nächste Schritte

[Configure the model update frequency (Ändern der Häufigkeit der Modellaktualisierung)](how-to-settings.md#model-update-frequency)