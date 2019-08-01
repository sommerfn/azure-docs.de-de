---
title: Trainieren von Genauigkeitsmetriken beim automatisierten Machine Learning
titleSuffix: Azure Machine Learning service
description: Informationen zu Genauigkeitsmetriken beim automatisierten Machine Learning für alle Ihrer Ausführungen
author: j-martens
ms.author: jmartens
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 07/16/2019
ms.openlocfilehash: dc147fd0252b2b5ec4ce334d6c1c464d9cde8ef5
ms.sourcegitcommit: a8b638322d494739f7463db4f0ea465496c689c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2019
ms.locfileid: "68297891"
---
# <a name="evaluate-training-accuracy-in-automated-ml-with-metrics"></a>Bewerten der Trainingsgenauigkeit beim automatisierten Machine Learning mithilfe von Metriken

In diesem Artikel lernen Sie die verschiedenen verfügbaren Metriken für automatisierte ML-Modelle in Azure Machine Learning kennen. 

Es gibt mehrere Möglichkeiten, die Metriken zur Trainingsgenauigkeit für jede Ausführungsiteration anzuzeigen.
* Verwenden eines [Jupyter-Widgets](how-to-track-experiments.md#view-run-details)
* Verwenden [der Funktion `get_metrics()`](how-to-track-experiments.md#query-run-metrics) für jedes `Run`-Objekt
* Anzeigen [der Experimentmetriken im Azure-Portal](how-to-track-experiments.md#view-the-experiment-in-the-azure-portal)

## <a name="prerequisites"></a>Voraussetzungen
 
* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie noch heute die [kostenlose oder kostenpflichtige Version von Azure Machine Learning Service](https://aka.ms/AMLFree) aus.
 
* Erstellen Sie ein automatisiertes Machine Learning-Experiment, entweder mit dem SDK oder über das Azure-Portal.
 
    * Verwenden Sie das SDK zum Erstellen eines [Klassifizierungsmodells](how-to-auto-train-remote.md) oder [Regressionsmodells](tutorial-auto-train-models.md)
    * Verwenden Sie das [Azure-Portal](how-to-create-portal-experiments.md) zum Erstellen eines Klassifizierungs- oder Regressionsmodells, indem Sie die entsprechenden Daten hochladen.

## <a name="classification-metrics"></a>Klassifizierungsmetrik

Bei jeder Ausführungsiteration werden die unten angegebenen Metriken für eine Klassifizierungsaufgabe gespeichert.

|Metrik|BESCHREIBUNG|Berechnung|Zusätzliche Parameter
--|--|--|--|
AUC_Macro| AUC ist die Fläche unter der ROC-Kurve (Receiver Operating Characteristic Curve). Macro ist das arithmetische Mittel der AUC für jede Klasse.  | [Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html) | average="macro"|
AUC_Micro| AUC ist die Fläche unter der ROC-Kurve (Receiver Operating Characteristic Curve). Micro wird global durch die Kombination der echt positiven und der falsch positiven Ergebnisse aus jeder Klasse berechnet.| [Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html) | average="micro"|
AUC_Weighted  | AUC ist die Fläche unter der ROC-Kurve (Receiver Operating Characteristic Curve). Der gewichtete Wert ist das arithmetische Mittel des Ergebnisses für jede Klasse, gewichtet gemäß der Anzahl der TRUE-Instanzen in jeder Klasse.| [Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html)|average="weighted"
accuracy|Die Genauigkeit ist der Prozentsatz der prognostizierten Bezeichnungen, die den TRUE-Bezeichnungen genau entsprechen. |[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html) |Keine|
average_precision_score_macro|Die durchschnittliche Genauigkeit fasst eine Precision-Recall-Kurve als gewichteten Mittelwert der bei jedem Schwellenwert erzielten Genauigkeiten zusammen, wobei die Zunahme beim Recall aus dem vorherigen Schwellenwert als Gewichtung verwendet wird. „Macro“ ist das arithmetische Mittel des durchschnittlichen Genauigkeitswerts jeder Klasse.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|average="macro"|
average_precision_score_micro|Die durchschnittliche Genauigkeit fasst eine Precision-Recall-Kurve als gewichteten Mittelwert der bei jedem Schwellenwert erzielten Genauigkeiten zusammen, wobei die Zunahme beim Recall aus dem vorherigen Schwellenwert als Gewichtung verwendet wird. „Micro“ wird global durch Kombinieren der echt positiven und der falsch positiven Ergebnisse bei jedem Cutoff berechnet.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|average="micro"|
average_precision_score_weighted|Die durchschnittliche Genauigkeit fasst eine Precision-Recall-Kurve als gewichteten Mittelwert der bei jedem Schwellenwert erzielten Genauigkeiten zusammen, wobei die Zunahme beim Recall aus dem vorherigen Schwellenwert als Gewichtung verwendet wird. Der gewichtete Wert ist das arithmetische Mittel der durchschnittlichen Genauigkeit für jede Klasse, gewichtet gemäß der Anzahl der TRUE-Instanzen in jeder Klasse.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|average="weighted"|
balanced_accuracy|„Balanced accuracy“ ist das arithmetische Mittel des Recalls für jede Klasse.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|average="macro"|
f1_score_macro|„F1 score“ ist das harmonische Mittel aus Genauigkeit und Recall. „Macro“ ist das arithmetische Mittel des F1-Ergebnisses für jede Klasse.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|average="macro"|
f1_score_micro|„F1 score“ ist das harmonische Mittel aus Genauigkeit und Recall. „Micro“ wird global durch Zählen der insgesamt echt positiven, falsch negativen und falsch positiven Ergebnisse berechnet.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|average="micro"|
f1_score_weighted|„F1 score“ ist das harmonische Mittel aus Genauigkeit und Recall. Gewichteter Mittelwert nach Klassenhäufigkeit des F1-Ergebnisses für jede Klasse|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|average="weighted"|
log_loss|Dies ist die Verlustfunktion, die bei der (multinomialen) logistischen Regression und deren Erweiterungen wie z. B. neuronalen Netzen verwendet wird. Sie ist definiert als die Negativ-Log-Wahrscheinlichkeit der True-Bezeichnungen bei den Vorhersagen eines probabilistischen Klassifizierers. Für eine einzelne Stichprobe mit der true-Bezeichnung yt in {0,1} und der geschätzten Wahrscheinlichkeit yp, dass yt = 1, beträgt der logarithmische Verlust -log P(yt&#124;yp) = -(yt log(yp) + (1 - yt) log(1 - yp)).|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.log_loss.html)|Keine|
norm_macro_recall|Der normalisierte Makro-Recall wird normalisiert, damit die zufällige Leistung ein Ergebnis von 0 und die ideale Leistung einen Wert von 1 liefert. Dies kann erzielt werden durch norm_macro_recall := (recall_score_macro - R)/(1 - R), wobei R der erwartete Wert von recall_score_macro für zufällige Vorhersagen ist (d. h. R=0,5 für die binäre Klassifizierung und R=(1/C) für C-Klassen-Klassifizierungsprobleme)|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|average = "macro" und danach (recall_score_macro - R)/(1 - R), wobei R der erwartete Wert von recall_score_macro für zufällige Vorhersagen ist (d. h., R=0,5 für die binäre Klassifizierung und R=(1/C) für C-Klassen-Klassifizierungsprobleme)|
precision_score_macro|Genauigkeit ist der Prozentsatz der Elemente, die als bestimmte Klasse bezeichnet sind und sich tatsächlich in dieser Klasse befinden. „Macro“ ist das arithmetische Mittel der Genauigkeit für jede Klasse.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|average="macro"|
precision_score_micro|Genauigkeit ist der Prozentsatz der Elemente, die als bestimmte Klasse bezeichnet sind und sich tatsächlich in dieser Klasse befinden. „Micro“ wird global durch Zählen der insgesamt echt positiven und falsch positiven Ergebnisse berechnet.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|average="micro"|
precision_score_weighted|Genauigkeit ist der Prozentsatz der Elemente, die als bestimmte Klasse bezeichnet sind und sich tatsächlich in dieser Klasse befinden. Der gewichtete Wert ist das arithmetische Mittel der Genauigkeit für jede Klasse, gewichtet gemäß der Anzahl der TRUE-Instanzen in jeder Klasse.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|average="weighted"|
recall_score_macro|Recall ist der prozentuale Anteil der Elemente, die sich tatsächlich in einer bestimmten Klasse befinden und richtig bezeichnet sind. Macro ist das arithmetische Mittel des Recalls für jede Klasse.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|average="macro"|
recall_score_micro|Recall ist der prozentuale Anteil der Elemente, die sich tatsächlich in einer bestimmten Klasse befinden und richtig bezeichnet sind. „Micro“ wird global durch Zählen der insgesamt echt positiven und falsch negativen Ergebnisse berechnet.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|average="micro"|
recall_score_weighted|Recall ist der prozentuale Anteil der Elemente, die sich tatsächlich in einer bestimmten Klasse befinden und richtig bezeichnet sind. Der gewichtete Wert ist das arithmetische Mittel des Recalls für jede Klasse, gewichtet gemäß der Anzahl der TRUE-Instanzen in jeder Klasse.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|average="weighted"|
weighted_accuracy|Gewichtete Genauigkeit ist die Genauigkeit, bei der die Gewichtung, die jedem Beispiel zugewiesen wird, gleich dem Anteil der TRUE-Instanzen in der TRUE-Klasse dieses Beispiels ist.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html)|sample_weight ist ein Vektor gleich dem Anteil der betreffenden Klasse für jedes Element im Ziel.|

## <a name="regression-and-forecasting-metrics"></a>Regressions- und Vorhersagemetrik

Bei jeder Ausführungsiteration werden die unten angegebenen Metriken für eine Regressions- oder Vorhersageaufgabe gespeichert.

|Metrik|BESCHREIBUNG|Berechnung|Zusätzliche Parameter
--|--|--|--|
explained_variance|Die erläuterte Varianz ist der Anteil, mit dem ein mathematisches Modell für die Variation eines bestimmten Datasets verantwortlich ist. Hierbei handelt es sich um den prozentualen Rückgang der Varianz der ursprünglichen Daten im Vergleich zur Varianz der Fehler. Wenn der Mittelwert der Fehler 0 beträgt, entspricht er der erläuterten Varianz.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.explained_variance_score.html)|Keine|
r2_score|R2 ist der Ermittlungskoeffizient bzw. die prozentuale Reduzierung der quadratischen Fehler im Vergleich zu einem Baseline-Modell, das den Mittelwert ausgibt. Wenn der Mittelwert der Fehler 0 beträgt, entspricht er der erläuterten Varianz.|[Berechnung](https://scikit-learn.org/0.16/modules/generated/sklearn.metrics.r2_score.html)|Keine|
spearman_correlation|Die Spearman-Korrelation ist ein nicht parametrisches Maß für die Monotonie der Beziehung zwischen zwei Datasets. Im Gegensatz zur Pearson-Korrelation geht die Spearman-Korrelation nicht davon aus, dass beide Datasets normal verteilt sind. Wie bei anderen Korrelationskoeffizienten variiert dieser Wert zwischen -1 und +1, wobei 0 für keine Korrelation steht. Korrelationen von -1 oder +1 implizieren eine exakt monotone Beziehung. Positive Korrelationen implizieren, dass sich x ebenso wie y erhöht. Negative Korrelationen implizieren, dass sich x erhöht und y niedriger wird.|[Berechnung](https://docs.scipy.org/doc/scipy-0.16.1/reference/generated/scipy.stats.spearmanr.html)|Keine|
mean_absolute_error|Der mittlere absolute Fehler ist der erwartete Wert des absoluten Differenzwerts zwischen dem Ziel und der Vorhersage.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_absolute_error.html)|Keine|
normalized_mean_absolute_error|Der normalisierte mittlere absolute Fehler ist der mittlere absolute Fehler, dividiert durch den Datenbereich.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_absolute_error.html)|Division durch den Datenbereich|
median_absolute_error|Der mittlere absolute Fehler ist der Median aller absoluten Differenzen zwischen dem Ziel und der Vorhersage. Dieser Verlust ist stabil in Bezug auf Ausreißer.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.median_absolute_error.html)|Keine|
normalized_median_absolute_error|Der normalisierte mittlere absolute Fehler ist der mittlere absolute Fehler, dividiert durch den Datenbereich.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.median_absolute_error.html)|Division durch den Datenbereich|
root_mean_squared_error|Die Wurzel aus dem mittleren quadratischen Fehler ist die Quadratwurzel der erwarteten quadratischen Differenz zwischen dem Ziel und der Vorhersage.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html)|Keine|
normalized_root_mean_squared_error|Die normalisierte Wurzel aus dem mittleren quadratischen Fehler ist die Wurzel aus dem mittleren quadratischen Fehler, dividiert durch den Datenbereich.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html)|Division durch den Datenbereich|
root_mean_squared_log_error|Die Wurzel aus dem mittleren quadratischen logarithmischen Fehler ist die Quadratwurzel des erwarteten quadratischen logarithmischen Fehlers.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_log_error.html)|Keine|
normalized_root_mean_squared_log_error|Die normalisierte Wurzel aus dem mittleren quadratischen logarithmischen Fehler ist die Wurzel aus dem mittleren quadratischen logarithmischen Fehler dividiert durch den Datenbereich.|[Berechnung](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_log_error.html)|Division durch den Datenbereich|

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über [automatisiertes Machine Learning](concept-automated-ml.md) in Azure Machine Learning.