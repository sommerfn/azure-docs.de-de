---
title: 'Bewertungsmodell: Modulreferenz'
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie das Modul „Score Model“ (Bewertungsmodell) in Azure Machine Learning zum Generieren von Vorhersagen mit einem trainierten Klassifizierungs- oder Regressionsmodell verwenden.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 10/22/2019
ms.openlocfilehash: e7ab953a7ac0907244ebaab70b3b86cbe6f0f4d9
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73497641"
---
# <a name="score-model-module"></a>Modul Modell bewerten

In diesem Artikel wird ein Modul in Azure Machine Learning-Designer (Vorschauversion) beschrieben.

Verwenden Sie dieses Modul zum Generieren von Vorhersagen mithilfe eines trainierten Klassifizierungs- oder Regressionsmodells.

## <a name="how-to-use"></a>Verwendung

1. Fügen Sie das Modul **Bewertungsmodell** Ihrer Pipeline hinzu.

2. Fügen Sie ein trainiertes Modell und ein Dataset mit neuen Eingabedaten an. 

    Die Daten sollten in einem Format vorliegen, dass mit dem Typ des trainierten Modells kompatibel ist, das Sie verwenden. Das Schema des Eingabedatasets sollte auch generell mit dem Schema der zum Trainieren des Modells verwendeten Daten übereinstimmen.

3. Ausführen der Pipeline.

## <a name="results"></a>Ergebnisse

Nachdem Sie eine Reihe von Bewertungen mit dem [Bewertungsmodell](./score-model.md) generiert haben:

+ Um einen Satz von Metriken zu generieren, die zum Auswerten der Genauigkeit (Leistung) des Modells dienen,  können Sie das bewertete Dataset mit dem [Auswertungsmodell](./evaluate-model.md) verbinden, 
+ mit der rechten Maustaste auf das Modul klicken und **Visualize** (Visualisieren) auswählen, um ein Beispiel der Ergebnisse anzuzeigen.
+ Speichern Sie die Ergebnisse in einem Dataset.

Das Ergebnis, oder der vorhergesagte Wert, kann je nach Modell und Eingabedaten in vielen verschiedenen Formaten vorliegen:

- Für Klassifizierungsmodelle gibt [Bewertungsmodell](./score-model.md) einen vorhergesagten Wert für die Klasse sowie die Wahrscheinlichkeit des vorhergesagten Werts aus.
- Für Regressionsmodelle generiert [Bewertungsmodell](./score-model.md) lediglich den vorhergesagten numerischen Wert.
- Für Bildklassifizierungsmodelle könnte die Bewertung die Klasse des Objekts im Bild oder ein boolescher Wert sein, der angibt, ob ein bestimmtes Feature gefunden wurde.

## <a name="publish-scores-as-a-web-service"></a>Veröffentlichen eines Modells als Webdienst

Eine häufige Verwendung der Bewertung ist die Rückgabe der Ausgabe als Teil eines Vorhersagewebdiensts. Weitere Informationen finden Sie im Tutorial zum Erstellen eines Webdiensts basierend auf einer Pipeline in Azure Machine Learning:

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die [Gruppe der verfügbaren Module](module-reference.md) für Azure Machine Learning an. 