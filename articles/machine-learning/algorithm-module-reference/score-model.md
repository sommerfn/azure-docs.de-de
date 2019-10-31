---
title: 'Bewertungsmodell: Modulreferenz'
titleSuffix: Azure Machine Learning service
description: Erfahren Sie, wie Sie das Modul „Bewertungsmodell“ in Azure Machine Learning Service zum Generieren von Vorhersagen mit einem trainierten Klassifizierungs- oder Regressionsmodell verwenden.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ms.openlocfilehash: f046b41d02ac392d003a9ecd15f25b43e5449cd0
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72692671"
---
# <a name="score-model-module"></a>Modul Modell bewerten

In diesem Artikel wird ein Modul der grafischen Benutzeroberfläche (Vorschau) für Azure Machine Learning Service beschrieben.

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

Sehen Sie sich die [Gruppe der verfügbaren Module](module-reference.md) für Azure Machine Learning Service an. 