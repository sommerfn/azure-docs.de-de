---
title: Verstehen von automatisierten ML-Ergebnissen
titleSuffix: Azure Machine Learning
description: Es wird beschrieben, wie Sie Diagramme und Metriken für Ihre verschiedenen automatisierten Machine Learning-Ausführungen anzeigen und sich damit vertraut machen.
services: machine-learning
author: nilesha
ms.author: nilesha
ms.reviewer: sgilley
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 07/22/2019
ms.openlocfilehash: b0024bc12f29a76da02c9f7e62af7727b9af7249
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71350646"
---
# <a name="understand-automated-machine-learning-results"></a>Grundlagen von Ergebnissen des automatisierten maschinellen Lernens

In diesem Artikel wird beschrieben, wie Sie Diagramme und Metriken für Ihre verschiedenen automatisierten Machine Learning-Ausführungen anzeigen und sich damit vertraut machen. 

Weitere Informationen:
+ [Metriken, Diagramme und Kurven für Klassifizierungsmodelle](#classification)
+ [Metriken, Diagramme und Graphen für Regressionsmodelle](#regression)
+ [Interpretierbarkeit von Modellen und Featurepriorität](#explain-model)

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie die [kostenlose oder kostenpflichtige Version von Azure Machine Learning](https://aka.ms/AMLFree) noch heute aus.

* Erstellen Sie ein Experiment für Ihre Ausführung des automatisierten maschinellen Lernens, entweder mit dem SDK, im Azure-Portal oder über die Landing Page Ihres Arbeitsbereichs (Vorschau).

    * Verwenden Sie das SDK zum Erstellen eines [Klassifizierungsmodells](how-to-auto-train-remote.md) oder [Regressionsmodells](tutorial-auto-train-models.md)
    * Verwenden Sie das [Azure-Portal oder die Landing Page Ihres Arbeitsbereichs (Vorschau)](how-to-create-portal-experiments.md) zum Erstellen eines Klassifizierungs- oder Regressionsmodells, indem Sie die entsprechenden Daten hochladen.

## <a name="view-the-run"></a>Anzeigen der Ausführung

Nach dem Durchführen eines Experiments für automatisiertes maschinelles Lernen finden Sie einen Verlauf der Ausführungen in Ihrem Machine Learning Service-Arbeitsbereich. 

1. Wechseln Sie zu Ihrem Arbeitsbereich.

1. Wählen Sie links im Arbeitsbereich die Option **Experimente**.

   ![Screenshot des Menüs „Experiment“](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-menu.png)

1. Wählen Sie in der Liste mit den Experimenten den gewünschten Eintrag aus.

   [![Experimentliste](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-list.png)](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-list-expanded.png)

1. Wählen Sie in der unteren Tabelle die Option **Ausführungsanzahl**.

   [![Experimentausführung](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-run.png)](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-run-expanded.png)

1. Wählen Sie in der Tabelle „Iterationen“ die **Iterationsnummer** für das Modell aus, das Sie weiter untersuchen möchten.

   [![Experimentmodell](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-model.png)](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-experiment-model-expanded.png)

Diese Ergebnisse werden auch während einer Ausführung angezeigt, wenn Sie das `RunDetails`[Jupyter-Widget](https://docs.microsoft.com/python/api/azureml-widgets/azureml.widgets?view=azure-ml-py) verwenden.

## <a name="classification"></a> Klassifizierungsergebnisse

Die folgenden Metriken und Diagramme sind für jedes Klassifizierungsmodell verfügbar, das Sie mit den automatisierten Machine Learning-Funktionen von Azure Machine Learning erstellen:

+ [Metriken](#classification-metrics)
+ [Konfusionsmatrix](#confusion-matrix)
+ [Genauigkeit-Trefferquote-Diagramm](#precision-recall-chart)
+ [ROC-Kurve (Receiver Operating Characteristics)](#roc)
+ [Prognosegütekurve](#lift-curve)
+ [Gewinnkurve](#gains-curve)
+ [Kalibrierungsdiagramm](#calibration-plot)

### <a name="classification-metrics"></a>Klassifizierungsmetrik

Bei jeder Ausführungsiteration werden die unten angegebenen Metriken für eine Klassifizierungsaufgabe gespeichert.

|Metrik|BESCHREIBUNG|Berechnung|Zusätzliche Parameter
--|--|--|--|
AUC_Macro| AUC ist die Fläche unter der ROC-Kurve (Receiver Operating Characteristic Curve). Macro ist das arithmetische Mittel der AUC für jede Klasse.  | [Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html) | average="macro"|
AUC_Micro| AUC ist die Fläche unter der ROC-Kurve (Receiver Operating Characteristic Curve). „Micro“ wird global durch die Kombination der echt positiven und der falsch positiven Ergebnisse aus jeder Klasse berechnet.| [Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html) | average="micro"|
AUC_Weighted  | AUC ist die Fläche unter der ROC-Kurve (Receiver Operating Characteristic Curve). Der gewichtete Wert ist das arithmetische Mittel des Ergebnisses für jede Klasse, gewichtet gemäß der Anzahl der TRUE-Instanzen in jeder Klasse.| [Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html)|average="weighted"
accuracy|Die Genauigkeit ist der Prozentsatz der prognostizierten Bezeichnungen, die den TRUE-Bezeichnungen genau entsprechen. |[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html) |Keine|
average_precision_score_macro|Die durchschnittliche Genauigkeit fasst eine Precision-Recall-Kurve als gewichteten Mittelwert der bei jedem Schwellenwert erzielten Genauigkeiten zusammen, wobei die Zunahme beim Recall aus dem vorherigen Schwellenwert als Gewichtung verwendet wird. „Macro“ ist das arithmetische Mittel des durchschnittlichen Genauigkeitswerts jeder Klasse.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|average="macro"|
average_precision_score_micro|Die durchschnittliche Genauigkeit fasst eine Precision-Recall-Kurve als gewichteten Mittelwert der bei jedem Schwellenwert erzielten Genauigkeiten zusammen, wobei die Zunahme beim Recall aus dem vorherigen Schwellenwert als Gewichtung verwendet wird. „Micro“ wird global durch Kombinieren der echt positiven und der falsch positiven Ergebnisse bei jedem Cutoff berechnet.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|average="micro"|
average_precision_score_weighted|Die durchschnittliche Genauigkeit fasst eine Precision-Recall-Kurve als gewichteten Mittelwert der bei jedem Schwellenwert erzielten Genauigkeiten zusammen, wobei die Zunahme beim Recall aus dem vorherigen Schwellenwert als Gewichtung verwendet wird. Der gewichtete Wert ist das arithmetische Mittel der durchschnittlichen Genauigkeit für jede Klasse, gewichtet gemäß der Anzahl der TRUE-Instanzen in jeder Klasse.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|average="weighted"|
balanced_accuracy|„Balanced accuracy“ ist das arithmetische Mittel des Recalls für jede Klasse.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|average="macro"|
f1_score_macro|„F1 score“ ist das harmonische Mittel aus Genauigkeit und Recall. „Macro“ ist das arithmetische Mittel des F1-Ergebnisses für jede Klasse.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|average="macro"|
f1_score_micro|„F1 score“ ist das harmonische Mittel aus Genauigkeit und Recall. „Micro“ wird global durch Zählen der insgesamt echt positiven, falsch negativen und falsch positiven Ergebnisse berechnet.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|average="micro"|
f1_score_weighted|„F1 score“ ist das harmonische Mittel aus Genauigkeit und Recall. Gewichteter Mittelwert nach Klassenhäufigkeit des F1-Ergebnisses für jede Klasse|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|average="weighted"|
log_loss|Dies ist die Verlustfunktion, die bei der (multinomialen) logistischen Regression und deren Erweiterungen wie z. B. neuronalen Netzen verwendet wird. Sie ist definiert als die Negativ-Log-Wahrscheinlichkeit der True-Bezeichnungen bei den Vorhersagen eines probabilistischen Klassifizierers. Für eine einzelne Stichprobe mit der TRUE-Bezeichnung „yt“ in {0,1} und der geschätzten Wahrscheinlichkeit yp, dass yt = 1 lautet der logarithmische Verlust -log P(yt&#124;yp) = -(yt log(yp) + (1 - yt) log(1 - yp)).|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.log_loss.html)|Keine|
norm_macro_recall|Der normalisierte Makro-Recall wird normalisiert, damit die zufällige Leistung ein Ergebnis von 0 und die ideale Leistung einen Wert von 1 liefert. Dies kann erzielt werden durch norm_macro_recall := (recall_score_macro - R)/(1 - R), wobei R der erwartete Wert von recall_score_macro für zufällige Vorhersagen ist (d. h. R=0,5 für die binäre Klassifizierung und R=(1/C) für C-Klassen-Klassifizierungsprobleme).|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|average = "macro" |
precision_score_macro|Genauigkeit ist der Prozentsatz der Elemente, die als bestimmte Klasse bezeichnet sind und sich tatsächlich in dieser Klasse befinden. „Macro“ ist das arithmetische Mittel der Genauigkeit für jede Klasse.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|average="macro"|
precision_score_micro|Genauigkeit ist der Prozentsatz der Elemente, die als bestimmte Klasse bezeichnet sind und sich tatsächlich in dieser Klasse befinden. „Micro“ wird global durch Zählen der insgesamt echt positiven und falsch positiven Ergebnisse berechnet.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|average="micro"|
precision_score_weighted|Genauigkeit ist der Prozentsatz der Elemente, die als bestimmte Klasse bezeichnet sind und sich tatsächlich in dieser Klasse befinden. Der gewichtete Wert ist das arithmetische Mittel der Genauigkeit für jede Klasse, gewichtet gemäß der Anzahl der TRUE-Instanzen in jeder Klasse.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|average="weighted"|
recall_score_macro|Recall ist der prozentuale Anteil der Elemente, die sich tatsächlich in einer bestimmten Klasse befinden und richtig bezeichnet sind. „Macro“ ist das arithmetische Mittel des Recalls für jede Klasse.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|average="macro"|
recall_score_micro|Recall ist der prozentuale Anteil der Elemente, die sich tatsächlich in einer bestimmten Klasse befinden und richtig bezeichnet sind. „Micro“ wird global durch Zählen der insgesamt echt positiven, falsch negativen und falsch positiven Ergebnisse berechnet.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|average="micro"|
recall_score_weighted|Recall ist der prozentuale Anteil der Elemente, die sich tatsächlich in einer bestimmten Klasse befinden und richtig bezeichnet sind. Der gewichtete Wert ist das arithmetische Mittel des Recalls für jede Klasse, gewichtet gemäß der Anzahl der TRUE-Instanzen in jeder Klasse.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|average="weighted"|
weighted_accuracy|Die gewichtete Genauigkeit ist die Genauigkeit, bei der die Gewichtung, die jedem Beispiel zugewiesen wird, gleich dem Anteil der TRUE-Instanzen in der TRUE-Klasse dieses Beispiels ist.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html)|sample_weight ist ein Vektor gleich dem Anteil der betreffenden Klasse für jedes Element im Ziel.|

### <a name="confusion-matrix"></a>Konfusionsmatrix

Eine Konfusionsmatrix wird verwendet, um die Leistung eines Klassifizierungsmodells zu beschreiben. Jede Zeile zeigt die Instanzen der wahren Klasse an, und jede Spalte repräsentiert die Instanzen der vorhergesagten Klasse. Die Konfusionsmatrix zeigt die richtig klassifizierten Bezeichnungen und die falsch klassifizierten Bezeichnungen für ein bestimmtes Modell.

Für Klassifizierungsprobleme stellt Azure Machine Learning automatisch eine Konfusionsmatrix für jedes Modell zur Verfügung, das erstellt wird. Von automatisiertem Machine Learning wird für jede Konfusionsmatrix die Häufigkeit der einzelnen vorhergesagten Bezeichnungen und der einzelnen tatsächlichen Bezeichnungsüberschneidungen angezeigt. Je dunkler die Farbe, desto höher die Anzahl in diesem bestimmten Teil der Matrix. Im Idealfall verlaufen die dunkelsten Farben entlang der Diagonalen der Matrix. 

Beispiel 1: Ein Klassifizierungsmodell mit schlechter Genauigkeit ![Ein Klassifizierungsmodell mit schlechter Genauigkeit](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-confusion-matrix1.png)

Beispiel 2: Ein Klassifizierungsmodell mit hoher Genauigkeit (ideal) ![Ein Klassifizierungsmodell mit hoher Genauigkeit](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-confusion-matrix2.png)


### <a name="precision-recall-chart"></a>Genauigkeit-Trefferquote-Diagramm

Mit diesem Diagramm können Sie die Genauigkeit-Trefferquote-Kurven für jedes Modell vergleichen, um festzustellen, welches Modell eine akzeptable Beziehung zwischen Genauigkeit und Trefferquote für Ihr spezielles Geschäftsproblem aufweist. Dieses Diagramm zeigt die durchschnittliche Makro-Genauigkeit-Trefferquote, die durchschnittliche Mikro-Genauigkeit-Trefferquote und die Genauigkeit-Trefferquote, die allen Klassen für ein Modell zugeordnet ist.

Der Begriff „Genauigkeit“ steht für die Fähigkeit eines Klassifizierers, alle Instanzen richtig zu kennzeichnen. „Trefferquote“ stellt die Möglichkeit für einen Klassifizierer dar, alle Instanzen einer bestimmten Bezeichnung zu finden. Die Genauigkeit-Trefferquote-Kurve zeigt die Beziehung zwischen diesen beiden Konzepten. Im Idealfall würde das Modell 100 % Genauigkeit und 100 % Trefferquote aufweisen.

Beispiel 1: Ein Klassifizierungsmodell mit geringer Genauigkeit und geringer Trefferquote ![Ein Klassifizierungsmodell mit geringer Genauigkeit und geringer Trefferquote](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-precision-recall1.png)

Beispiel 2: Ein Klassifizierungsmodell mit ca. 100 % Genauigkeit und ca. 100 % Trefferquote (ideal) ![Ein Klassifizierungsmodell mit ca. 100 % Genauigkeit und ca. 100 % Trefferquote](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-precision-recall2.png)

### <a name="roc"></a>ROC

Receiver Operating Characteristic (oder ROC) ist ein Diagramm der richtig klassifizierten Bezeichnungen im Vergleich zu den falsch klassifizierten Bezeichnungen für ein bestimmtes Modell. Die ROC-Kurve kann weniger aussagekräftig sein, wenn Modelle mit Datasets mit einem großen Bias trainiert werden, da sie die falsch-positiven Bezeichnungen nicht anzeigt.

Beispiel 1: Ein Klassifizierungsmodell mit geringen wahren Bezeichnungen und hohen falschen Bezeichnungen ![Ein Klassifizierungsmodell mit geringen wahren Bezeichnungen und hohen falschen Bezeichnungen](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-roc-1.png)

Beispiel 2: Ein Klassifizierungsmodell mit hohen wahren Bezeichnungen und geringen falschen Bezeichnungen ![Ein Klassifizierungsmodell mit hohen wahren Bezeichnungen und geringen falschen Bezeichnungen](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-roc-2.png)

### <a name="lift-curve"></a>Prognosegütekurve

Sie können die Prognosegüte des automatisch mit Azure Machine Learning erstellten Modells mit der Baseline vergleichen, um den Wertzuwachs dieses bestimmten Modells anzuzeigen.

Prognosegütediagramme werden verwendet, um die Leistung eines Klassifizierungsmodells auszuwerten. Sie zeigen, wie viel bessere Ergebnisse bei der Verwendung eines Modells im Vergleich dazu zu erwarten sind, wenn kein Modell verwendet wird. 

Beispiel 1: Das Modell schneidet schlechter ab als ein Zufallsauswahlmodell ![Ein Klassifizierungsmodell, das schlechter abschneidet als ein Zufallsauswahlmodell](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-lift-curve1.png)

Beispiel 2: Das Modell schneidet besser ab als ein Zufallsauswahlmodell ![Ein Klassifizierungsmodell, das besser abschneidet](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-lift-curve2.png)

### <a name="gains-curve"></a>Gewinnkurve

Ein Gewinndiagramm wertet die Leistung eines Klassifizierungsmodells anhand jedes Teils der Daten aus. Es zeigt für jedes Perzentil des Datensatzes an, wie viel besser Sie im Vergleich zu einem Zufallsauswahlmodell abschneiden können.

Verwenden Sie das kumulierte Gewinndiagramm, um die Auswahl der Klassifizierungsgrenze anhand eines Prozentsatzes zu erleichtern, der einem gewünschten Gewinn aus dem Modell entspricht. Diese Informationen bieten eine weitere Möglichkeit, die Ergebnisse im zugehörigen Prognosegütediagramm zu untersuchen.

Beispiel 1: Ein Klassifizierungsmodell mit einem minimalen Gewinn ![Ein Klassifizierungsmodell mit einem minimalen Gewinn](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-gains-curve1.png)

Beispiel 2: Ein Klassifizierungsmodell mit einem erheblichen Gewinn ![Ein Klassifizierungsmodell mit einem erheblichen Gewinn](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-gains-curve2.png)

### <a name="calibration-plot"></a>Kalibrierungsdiagramm

Für alle Klassifizierungsprobleme können Sie die Kalibrierungskurve auf Mikrodurchschnitt, Makrodurchschnitt und jede Klasse in einem bestimmten Vorhersagemodell überprüfen. 

Ein Kalibrierungsdiagramm wird verwendet, um die Sicherheit eines Vorhersagemodells anzuzeigen. Dies geschieht, indem es die Beziehung zwischen der vorhergesagten Wahrscheinlichkeit und der tatsächlichen Wahrscheinlichkeit zeigt, wobei die „Wahrscheinlichkeit“ die Wahrscheinlichkeit darstellt, dass eine bestimmte Instanz zu einer bestimmten Bezeichnung gehört. Ein gut kalibriertes Modell richtet sich nach der y=x-Linie aus, wo es in Bezug auf seine Vorhersagen recht zuverlässig ist. Ein übermäßig zuverlässiges Modell richtet sich nach der y=0-Linie aus, wobei die vorhergesagte Wahrscheinlichkeit vorhanden ist, aber keine tatsächliche Wahrscheinlichkeit vorliegt.

Beispiel 1: Ein gut kalibriertes Modell ![ Ein gut kalibriertes Modell](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-calib-curve1.png)

Beispiel 2: Ein übermäßig zuverlässiges Modell ![Ein übermäßig zuverlässiges Modell](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-calib-curve2.png)

## <a name="regression"></a> Regressionsergebnisse

Die folgenden Metriken und Diagramme sind für jedes Regressionsmodell verfügbar, das Sie mit den Funktionen für automatisiertes maschinelles Lernen von Azure Machine Learning erstellen:

+ [Metriken](#reg-metrics)
+ [Vorhergesagt im Vergleich zu TRUE](#pvt)
+ [Residualhistogramm](#histo)


### <a name="reg-metrics"></a> Regressionsmetriken

Bei jeder Ausführungsiteration werden die unten angegebenen Metriken für eine Regressions- oder Vorhersageaufgabe gespeichert.

|Metrik|BESCHREIBUNG|Berechnung|Zusätzliche Parameter
--|--|--|--|
explained_variance|Die erläuterte Varianz ist der Anteil, mit dem ein mathematisches Modell für die Variation eines bestimmten Datasets verantwortlich ist. Hierbei handelt es sich um den prozentualen Rückgang der Varianz der ursprünglichen Daten im Vergleich zur Varianz der Fehler. Wenn der Mittelwert der Fehler 0 beträgt, entspricht er der erläuterten Varianz.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.explained_variance_score.html)|Keine|
r2_score|R2 ist der Ermittlungskoeffizient bzw. die prozentuale Reduzierung der quadratischen Fehler im Vergleich zu einem Baseline-Modell, das den Mittelwert ausgibt. |[Berechnung](https://scikit-learn.org/0.16/modules/generated/sklearn.metrics.r2_score.html)|Keine|
spearman_correlation|Die Spearman-Korrelation ist ein nicht parametrisches Maß für die Monotonie der Beziehung zwischen zwei Datasets. Im Gegensatz zur Pearson-Korrelation geht die Spearman-Korrelation nicht davon aus, dass beide Datasets normal verteilt sind. Wie bei anderen Korrelationskoeffizienten variiert dieser Wert zwischen -1 und +1, wobei 0 für keine Korrelation steht. Korrelationen von -1 oder +1 implizieren eine exakt monotone Beziehung. Positive Korrelationen implizieren, dass sich x ebenso wie y erhöht. Negative Korrelationen implizieren, dass sich x erhöht und y niedriger wird.|[Berechnung](https://docs.scipy.org/doc/scipy-0.16.1/reference/generated/scipy.stats.spearmanr.html)|Keine|
mean_absolute_error|Der mittlere absolute Fehler ist der erwartete Wert des absoluten Differenzwerts zwischen dem Ziel und der Vorhersage.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_absolute_error.html)|Keine|
normalized_mean_absolute_error|Der normalisierte mittlere absolute Fehler ist der mittlere absolute Fehler, dividiert durch den Datenbereich.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_absolute_error.html)|Division durch den Datenbereich|
median_absolute_error|Der mittlere absolute Fehler ist der Median aller absoluten Differenzen zwischen dem Ziel und der Vorhersage. Dieser Verlust ist stabil in Bezug auf Ausreißer.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.median_absolute_error.html)|Keine|
normalized_median_absolute_error|Der normalisierte mittlere absolute Fehler ist der mittlere absolute Fehler, dividiert durch den Datenbereich.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.median_absolute_error.html)|Division durch den Datenbereich|
root_mean_squared_error|Die Wurzel aus dem mittleren quadratischen Fehler ist die Quadratwurzel der erwarteten quadratischen Differenz zwischen dem Ziel und der Vorhersage.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html)|Keine|
normalized_root_mean_squared_error|Die normalisierte Wurzel aus dem mittleren quadratischen Fehler ist die Wurzel aus dem mittleren quadratischen Fehler, dividiert durch den Datenbereich.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html)|Division durch den Datenbereich|
root_mean_squared_log_error|Die Wurzel aus dem mittleren quadratischen logarithmischen Fehler ist die Quadratwurzel des erwarteten quadratischen logarithmischen Fehlers.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_log_error.html)|Keine|
normalized_root_mean_squared_log_error|Die normalisierte Wurzel aus dem mittleren quadratischen logarithmischen Fehler ist die Wurzel aus dem mittleren quadratischen logarithmischen Fehler dividiert durch den Datenbereich.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_log_error.html)|Division durch den Datenbereich|

### <a name="pvt"></a> Vorhergesagt im Vergleich zu True

Vorhergesagt im Vergleich zu TRUE zeigt die Beziehung zwischen einem vorhergesagten Wert und seinem korrelierenden wahren Wert für ein Regressionsproblem. Dieses Diagramm kann verwendet werden, um die Leistung eines Modells zu messen, da Folgendes gilt: Je näher die vorhergesagten Werte an der y=x-Linie liegen, desto besser ist die Genauigkeit eines Vorhersagemodells.

Nach jeder Ausführung wird für jedes Regressionsmodell ein Diagramm mit den vorhergesagten im Vergleich zu den wahren Werten angezeigt. Zum Schutz der Privatsphäre werden die Werte in Behältern zusammengeführt, und die Größe jedes Behälters wird als Balkendiagramm im unteren Teil des Diagrammbereichs angezeigt. Sie können das Vorhersagemodell (mit dem helleren Farbbereich, der die Fehlergrenzen anzeigt) mit dem Idealwert des Modells vergleichen.

Beispiel 1: Ein Regressionsmodell mit niedriger Genauigkeit bei Vorhersagen ![Ein Regressionsmodell mit niedriger Genauigkeit bei Vorhersagen](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-regression1.png)

Beispiel 2: Ein Regressionsmodell mit hoher Genauigkeit bei seinen Vorhersagen [![Ein Regressionsmodell mit hoher Genauigkeit bei seinen Vorhersagen](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-regression2.png)](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-regression2-expanded.png)



### <a name="histo"></a> Residualhistogramm

Ein Residual stellt ein beobachtetes y dar: das vorhergesagte y. Um eine Fehlerspanne mit geringem Bias darzustellen, sollte das Histogramm der Residualwerte als Glockenkurve geformt sein, die um 0 zentriert ist. 

Beispiel 1: Ein Regressionsmodell mit einem Bias in seinen Fehlern ![SA-Regressionsmodell mit einem Bias in seinen Fehlern](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-regression3.png)

Beispiel 2: Ein Regressionsmodell mit einer gleichmäßigeren Verteilung von Fehlern ![Ein Regressionsmodell mit einer gleichmäßigeren Verteilung von Fehlern](./media/how-to-understand-automated-ml/azure-machine-learning-auto-ml-regression4.png)

## <a name="explain-model"></a> Interpretierbarkeit von Modellen und Featurepriorität

Mit der Featurepriorität können Sie anzeigen, wie wertvoll jedes Feature bei der Erstellung eines Modells war. Diese Berechnung ist standardmäßig deaktiviert, da sie zu einer erheblichen Verlängerung der Laufzeit führen kann.   Sie können die Modellerklärung für alle Modelle oder nur für das am besten geeignete Modell aktivieren.

Sie können die Featuregewichtung für das Modell insgesamt sowie pro Klasse für ein Vorhersagemodell überprüfen. Sie können pro Feature erkennen, wie sich die Gewichtung im Vergleich zu jeder Klasse und insgesamt darstellt.

![Featureerklärbarkeit](./media/how-to-understand-automated-ml/feature-importance.gif)

Weitere Informationen zum Aktivieren von Features der Interpretierbarkeit finden Sie unter [Konfigurieren automatisierter ML-Experimente in Python](how-to-configure-auto-train.md#explain-the-model-interpretability).  Ein Beispiel zur Erklärung des besten Modells finden Sie unter [Erklärung des besten Modells](how-to-auto-train-remote.md#explain).

## <a name="next-steps"></a>Nächste Schritte

+ Erfahren Sie mehr über [automatisiertes Machine Learning](concept-automated-ml.md) in Azure Machine Learning.
+ Probieren Sie das Beispielnotebook für die [Modellerklärung zum automatisierten maschinellen Lernen](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning/model-explanation) aus.
