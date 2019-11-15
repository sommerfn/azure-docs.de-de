---
title: 'Designer: Vorhersagen des Kreditrisikos (kostensensibel)'
titleSuffix: Azure Machine Learning
description: In diesem Artikel wird gezeigt, wie über den Designer (Vorschauversion) eine komplexe Pipeline für maschinelles Lernen erstellt werden kann. Sie erfahren, wie benutzerdefinierte Python-Skripts implementiert und mehrere Modelle verglichen werden, um die beste Option auszuwählen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: xiaoharper
ms.author: zhanxia
ms.reviewer: peterlu
ms.date: 11/04/2019
ms.openlocfilehash: 7af0ee31c7d7e5dae4a38db7f6c74ff3e5f964bb
ms.sourcegitcommit: 359930a9387dd3d15d39abd97ad2b8cb69b8c18b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73647985"
---
# <a name="sample-4---classification-with-custom-python-script-predict-credit-risk"></a>Beispiel 4: Klassifizierung mit benutzerdefiniertem Python-Skript: Vorhersagen des Kreditrisikos
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-enterprise-sku.md)]

In diesem Artikel wird gezeigt, wie über den Designer (Vorschauversion) eine komplexe Pipeline für maschinelles Lernen erstellt werden kann. Sie erfahren, wie benutzerdefinierte Logik mit Python-Skripts implementiert wird und mehrere Modelle verglichen werden, um die beste Option auszuwählen.

In diesem Beispiel wird eine Klassifizierung trainiert, um das Kreditrisiko anhand von Kreditantragsinformationen wie Kreditgeschichte, Alter und Anzahl der Kreditkarten vorherzusagen. Sie können die Konzepte in diesem Artikel aber auch anwenden, um Ihre eigenen Probleme durch maschinelles Lernen zu lösen.

Der fertige Graph für diese Pipeline sieht wie folgt aus:

[![Graph der Pipeline](media/how-to-ui-sample-classification-predict-credit-risk-cost-sensitive/graph.png)](media/how-to-ui-sample-classification-predict-credit-risk-cost-sensitive/graph.png#lightbox)

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [aml-ui-prereq](../../../includes/aml-ui-prereq.md)]

4. Klicken Sie auf Beispiel 4, um es zu öffnen.

## <a name="data"></a>Data

In diesem Beispiel wird das Dataset „German Credit Card“ aus dem UC Irvine-Repository verwendet. Es enthält 1.000 Stichproben mit 20 Features und 1 Bezeichnung. Jede Stichprobe stellt eine Person dar. Die 20 Features enthalten numerische und kategorische Features. Weitere Informationen zu diesem Dataset finden Sie auf der [UCI-Website](https://archive.ics.uci.edu/ml/datasets/Statlog+%28German+Credit+Data%29). Die letzte Spalte ist die Bezeichnung, die das Kreditrisiko angibt und nur zwei mögliche Werte hat: hohes Kreditrisiko = 2 und niedriges Kreditrisiko = 1.

## <a name="pipeline-summary"></a>Pipelineübersicht

In dieser Pipeline vergleichen Sie zwei verschiedene Ansätze für die Generierung von Modellen zur Lösung des folgenden Problems:

- Training mit dem Originaldataset.
- Training mit einem replizierten Dataset.

Bei beiden Ansätzen werten Sie die Modelle aus, indem Sie das Testdataset mit Replikation verwenden, um sicherzustellen, dass die Ergebnisse auf die Kostenfunktion abgestimmt sind. Sie testen zwei Klassifizierer mit beiden Ansätzen: **Two-Class Support Vector Machine** und **Two-Class Boosted Decision Tree**.

Die Kosten für die Fehlklassifizierung einer Stichprobe mit geringem Risiko betragen 1, die Kosten für die Fehlklassifizierung einer Stichprobe mit hohem Risiko betragen 5. Wir verwenden ein Modul **Execute Python Script** (Python-Skript ausführen), um diese Fehlklassifizierungskosten zu berücksichtigen.

So sieht der Graph der Pipeline aus:

[![Graph der Pipeline](media/how-to-ui-sample-classification-predict-credit-risk-cost-sensitive/graph.png)](media/how-to-ui-sample-classification-predict-credit-risk-cost-sensitive/graph.png#lightbox)

## <a name="data-processing"></a>Datenverarbeitung

Beginnen Sie, indem Sie das Modul **Metadata Editor** (Metadaten-Editor) verwenden, um Spaltennamen hinzuzufügen und die standardmäßigen Spaltennamen durch aussagekräftigere Namen zu ersetzen, die aus der Datasetbeschreibung auf der UCI-Website entnommen wurden. Stellen Sie die neuen Spaltennamen als durch Trennzeichen getrennte Werte im Namensfeld **New column** (Neue Spalte) des **Metadaten-Editors** zur Verfügung.

Generieren Sie dann die Trainings- und Testdatasets, die zum Entwickeln des Risikovorhersagemodells verwendet werden. Teilen Sie das ursprüngliche Dataset mit dem Modul **Split Data** (Daten aufteilen) in ein Trainings- und ein Testdataset derselben Größe auf. Um identisch große Datasets zu erstellen, legen Sie die Option **Fraction of rows in the first output dataset** (Bruchteil der Zeilen im ersten Ausgabedataset) auf 0,7 fest.

### <a name="generate-the-new-dataset"></a>Generieren des neuen Datasets

Da die Kosten für die Unterschätzung des Risikos hoch sind, legen Sie die Kosten der Fehlklassifizierung wie folgt fest:

- Für Fälle mit hohem Risiko, die fälschlicherweise als risikoarm eingestuft werden: 5
- Für Fälle mit geringem Risiko, die fälschlicherweise als hohes Risiko eingestuft werden: 1

Um diese Kostenfunktion abzubilden, generieren Sie ein neues Dataset. Im neuen Dataset wird jede Stichprobe mit hohem Risiko fünf Mal repliziert, aber die Anzahl der Stichproben mit niedrigem Risiko ändert sich nicht. Teilen Sie die Daten vor der Replikation in Trainings- und Testdatasets auf, um zu verhindern, dass dieselbe Zeile in beiden Datasets enthalten ist.

Um die Hochrisikodaten zu replizieren, fügen Sie diesen Python-Code in das Modul **Execute Python Script** (Python-Skript ausführen) ein:

```Python
import pandas as pd

def azureml_main(dataframe1 = None, dataframe2 = None):

    df_label_1 = dataframe1[dataframe1.iloc[:, 20] == 1]
    df_label_2 = dataframe1[dataframe1.iloc[:, 20] == 2]

    result = df_label_1.append([df_label_2] * 5, ignore_index=True)
    return result,
```

Das Modul **Execute Python Script** repliziert sowohl die Trainings-als auch die Testdatasets.

### <a name="feature-engineering"></a>Featureentwicklung

Der Algorithmus **Two-Class Support Vector Machine** erfordert normalisierte Daten. Daher verwenden Sie das Modul **Normalize Data** (Daten normalisieren), um die Bereiche aller numerischen Features mit einer `tanh`-Transformation zu normalisieren. Eine `tanh`-Transformation konvertiert alle numerischen Features in Werte innerhalb eines Bereichs von 0 bis 1 unter Beibehaltung der Gesamtverteilung der Werte.

Das Modul **Two-Class Support Vector Machine** verarbeitet Zeichenfolgenfeatures und konvertiert sie in kategorische Merkmale und dann in binäre Merkmale mit einem Wert von 0 oder 1. Daher ist es nicht erforderlich, dass Sie diese Features normalisieren.

## <a name="models"></a>Modelle

Da Sie zwei Klassifizierer (**Two-Class Support Vector Machine** (SVM) und **Two-Class Boosted Decision Tree**) und auch zwei Datasets angewendet haben, generieren Sie insgesamt vier Modelle:

- SVM trainiert mit den ursprünglichen Daten.
- SVM trainiert mit den replizierten Daten.
- Boosted Decision Tree trainiert mit den ursprünglichen Daten.
- Boosted Decision Tree trainiert mit den replizierten Daten.

In diesem Beispiel wird der Data Science-Standardworkflow verwendet, um die Modelle zu erstellen, zu trainieren und zu testen:

1. Initialisieren Sie die Lernalgorithmen mit **Two-Class Support Vector Machine** und **Two-Class Boosted Decision Tree**.
1. Verwenden Sie **Train Model** (Modell trainieren), um den Algorithmus auf die Daten anzuwenden und das eigentliche Modell zu erstellen.
1. Verwenden Sie **Score Model** (Modell bewerten), um Bewertungen anhand der Testbeispiele zu generieren.

Der folgende Graph zeigt einen Ausschnitt dieser Pipeline, in dem das ursprüngliche und replizierte Trainingsdataset zum Trainieren von zwei verschiedenen SVM-Modellen verwendet wird. **Train Model** ist mit dem Trainingsdataset verbunden, **Score Model** mit dem Testdataset.

![Graph der Pipeline](media/how-to-ui-sample-classification-predict-credit-risk-cost-sensitive/score-part.png)

In der Auswertungsphase der Pipeline berechnen Sie jeweils die Genauigkeit der vier Modelle. Für diese Pipeline verwenden Sie **Evaluate Model**, um Beispiele zu vergleichen, die dieselben Fehlklassifizierungskosten aufweisen.

Das Modul **Evaluate Model** kann die Leistungsmetriken für zwei bewertete Modelle berechnen. Daher können Sie eine Instanz von **Evaluate Model** verwenden, um die beiden SVM-Modelle auszuwerten, und eine weitere Instanz von **Evaluate Model**, um die beiden Boosted Decision Tree-Modelle auszuwerten.

Beachten Sie, dass das replizierte Testdataset als Eingabe für **Score Model** verwendet wird. Mit anderen Worten: Die endgültigen Genauigkeitsbewertungen beinhalten die Kosten für die falsche Bewertung von Bezeichnungen.

## <a name="combine-multiple-results"></a>Kombinieren mehrerer Ergebnisse

Das Modul **Evaluate Model** generiert eine Tabelle mit einer einzelnen Zeile, die verschiedene Metriken enthält. Um einen einzigen Satz von Genauigkeitsergebnissen zu erstellen, verwenden wir zunächst **Add Rows**, um die Ergebnisse in einer einzigen Tabelle zu kombinieren. Wir verwenden dann das folgende Python-Skript im Modul **Execute Python Script**, um den Modellnamen und den Trainingsansatz für jede Zeile in der Ergebnistabelle hinzuzufügen:

```Python
import pandas as pd

def azureml_main(dataframe1 = None, dataframe2 = None):

    new_cols = pd.DataFrame(
            columns=["Algorithm","Training"],
            data=[
                ["SVM", "weighted"],
                ["SVM", "unweighted"],
                ["Boosted Decision Tree","weighted"],
                ["Boosted Decision Tree","unweighted"]
            ])

    result = pd.concat([new_cols, dataframe1], axis=1)
    return result,
```

## <a name="results"></a>Ergebnisse

Zum Anzeigen der Ergebnisse der Pipeline können Sie mit der rechten Maustaste auf die Visualisierungsausgabe des letzten Moduls **Select Columns in Dataset** klicken.

![Visualisieren der Ausgabe](media/how-to-ui-sample-classification-predict-credit-risk-cost-sensitive/result.png)

Die erste Spalte listet den Machine Learning-Algorithmus auf, der verwendet wurde, um das Modell zu generieren.

Die zweite Spalte gibt den Typ des Trainingsdatasets an.

Die dritte Spalte enthält den kostensensiblen Genauigkeitswert.

Aus diesen Ergebnissen können Sie ersehen, dass die beste Genauigkeit durch das Modell bereitgestellt wird, das mit **Two-Class Support Vector Machine** erstellt und mit dem replizierten Trainingsdataset trainiert wurde.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte

Erkunden Sie die anderen Beispiele, die für den Designer zur Verfügung stehen:

- [Beispiel 1 – Regression: Vorhersagen des Preises eines Autos](how-to-designer-sample-regression-automobile-price-basic.md)
- [Beispiel 2 – Regression: Vergleichen von Algorithmen für die Vorhersage von Autopreisen](how-to-designer-sample-regression-automobile-price-compare-algorithms.md)
- [Beispiel 3 – Klassifizierung mit Featureauswahl: Vorhersage des Einkommens](how-to-designer-sample-classification-predict-income.md)
- [Beispiel 5 – Klassifizierung: Vorhersage der Kundenabwanderung](how-to-designer-sample-classification-churn.md)
- [Beispiel 6 – Klassifizierung: Vorhersage von Flugverspätungen](how-to-designer-sample-classification-flight-delay.md)
- [Beispiel 7 – Textklassifizierung: Wikipedia SP 500-Dataset](how-to-designer-sample-text-classification.md)
