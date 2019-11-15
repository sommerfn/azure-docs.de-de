---
title: 'Designer: Klassifizieren, Einkommensprognose'
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie über den Designer (Vorschau) eine Machine Learning-Klassifizierung (Klassifizierer) erstellen, ohne eine einzige Codezeile zu schreiben.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: xiaoharper
ms.author: zhanxia
ms.reviewer: peterlu
ms.date: 11/04/2019
ms.openlocfilehash: 383cbc11955598505730a4613c50536afac75f95
ms.sourcegitcommit: 359930a9387dd3d15d39abd97ad2b8cb69b8c18b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73647963"
---
# <a name="sample-3---classification-with-feature-selection-income-prediction"></a>Beispiel 3 – Klassifizierung mit Featureauswahl: Vorhersage des Einkommens
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-enterprise-sku.md)]

Erfahren Sie, wie Sie über den Designer (Vorschau) eine Machine Learning-Klassifizierung (Klassifizierer) erstellen, ohne eine einzige Codezeile zu schreiben. In diesem Beispiel wird ein **verstärkter Entscheidungsbaum mit zwei Klassen** trainiert, um das Einkommen von Erwachsenen (>=50.000 oder <=50.000) mit der Erhebung vorherzusagen.

Weil die Frage „Welches Risiko?“ lautet, wird dies als Klassifikationsproblem bezeichnet. Sie können jedoch den gleichen elementaren Prozess anwenden, um jede Art von Problem für maschinelles Lernen zu bewältigen, sei es Regression, Klassifizierung, Clustering usw.

So sieht der endgültige Graph der Pipeline für dieses Beispiel aus:

![Graph der Pipeline](media/how-to-ui-sample-classification-predict-income/overall-graph.png)

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [aml-ui-prereq](../../../includes/aml-ui-prereq.md)]

4. Klicken Sie auf Beispiel 3, um es zu öffnen.



## <a name="data"></a>Data

Das Dataset enthält 14 Features und eine Bezeichnungsspalte. Es gibt mehrere Typen von Features, einschließlich numerischer und kategorischer Features. Das folgende Diagramm zeigt einen Auszug aus dem Dataset: ![Daten](media/how-to-ui-sample-classification-predict-income/data.png)



## <a name="pipeline-summary"></a>Pipelineübersicht

Führen Sie die folgenden Schritte aus, um die Pipeline zu erstellen:

1. Ziehen Sie das Datasetmodul „Adult Census Income Binary“ (Erhebung des Einkommens von Erwachsenen binär) in den Pipelinezeichenbereich.
1. Fügen Sie das Modul **Split Data** (Aufteilen von Daten) hinzu, um die Trainings- und Testdatasets zu erstellen. Legen Sie den Anteil der Zeilen im ersten Ausgabedataset auf 0,7 fest. Diese Einstellung bestimmt, dass 70 % der Daten an den linken Port des Moduls und der Rest an den rechten Port ausgegeben werden. Wir verwenden das linke Dataset für das Training und das rechte für Tests.
1. Fügen Sie das Modul **Filter Based Feature Selection** (filterbasierte Featureauswahl) hinzu, um 5 Features nach PearsonCorreclation auszuwählen. 
1. Fügen Sie das Modul **Two-Class Boosted Decision Tree** hinzu, um einen verstärkten Entscheidungsbaum mit zwei Klassen zu initialisieren.
1. Fügen Sie das Modul **Train Model** (Trainieren des Modells) hinzu. Verbinden Sie den Klassifizierer aus dem vorherigen Schritt mit dem linken Eingabeport des Moduls **Train Model**. Stellen Sie eine Verbindung des gefilterten Datasets aus dem Modul „Filter Based Feature Selection“ als Trainingsdataset her.  Das Modul **Train Model** trainiert den Klassifizierer.
1. Fügen Sie „Select Columns Transformation“ (Spaltentransformation auswählen) und das Modul „Apply Transformation“ (Transformation anwenden) hinzu, um die gleiche Transformation (Filter Based Feature Selection) auf das Testdataset anzuwenden.
![apply-transformation](media/how-to-ui-sample-classification-predict-income/transformation.png)
1. Fügen Sie das Modul **Score Model** (Bewerten des Modells) hinzu, und stellen Sie eine Verbindung des Moduls **Train Model** mit diesem Modul her. Fügen Sie dann das Testdataset (die Ausgabe des Moduls „Apply Transformation“, das die Featureauswahl auch auf das Testdataset anwendet) dem Modul **Score Model** hinzu. **Score Model** nimmt die Vorhersagen vor. Sie können den Ausgabeport auswählen, um die Vorhersagen und die Wahrscheinlichkeiten positiver Klassen anzuzeigen.


    Diese Pipeline verfügt über zwei Bewertungsmodule; das rechte Modul hat die Bezeichnungsspalte vor der Vorhersage ausgeschlossen. Dies ist die Vorbereitung für die Bereitstellung eines Echtzeitendpunkts, da die Webdiensteingabe nur Features ohne Bezeichnung erwartet. 

1. Fügen Sie das Modul **Evaluate Model** (Auswerten des Modells) hinzu, und verbinden Sie das bewertete Dataset mit seinem linken Eingabeport. Um die Auswertungsergebnisse anzuzeigen, klicken Sie auf den Ausgabeport des Moduls **Evaluate Model**, und wählen Sie **Visualize** (Visualisieren) aus.

## <a name="results"></a>Ergebnisse

![Auswertung der Ergebnisse](media/how-to-ui-sample-classification-predict-income/evaluate-result.png)

In den Auswertungsergebnissen können Sie Kurven wie ROC, Precision-Recall und Verwirrungsmetriken sehen. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte

Erkunden Sie die anderen Beispiele, die für den Designer zur Verfügung stehen:

- [Beispiel 1 – Regression: Vorhersagen des Preises eines Autos](how-to-designer-sample-regression-automobile-price-basic.md)
- [Beispiel 2 – Regression: Vergleichen von Algorithmen für die Vorhersage von Autopreisen](how-to-designer-sample-regression-automobile-price-compare-algorithms.md)
- [Beispiel 4 – Klassifizierung: Vorhersagen des Kreditrisikos (kostensensibel)](how-to-designer-sample-classification-credit-risk-cost-sensitive.md)
- [Beispiel 5 – Klassifizierung: Vorhersage der Kundenabwanderung](how-to-designer-sample-classification-churn.md)
- [Beispiel 6 – Klassifizierung: Vorhersage von Flugverspätungen](how-to-designer-sample-classification-flight-delay.md)
- [Beispiel 7 – Textklassifizierung: Wikipedia SP 500-Dataset](how-to-designer-sample-text-classification.md)
