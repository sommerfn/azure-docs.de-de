---
title: 'Evaluate Model: Modulreferenz'
titleSuffix: Azure Machine Learning service
description: Erfahren Sie, wie Sie das Modul „Evaluate Model“ (Auswerten des Modells) in Azure Machine Learning Service verwenden können, um die Qualität eines trainierten Modells zu messen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/06/2019
ms.openlocfilehash: 0ad4ceedf9c1d65339c9e4aabebc0a47475ed568
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72693799"
---
# <a name="evaluate-model-module"></a>Modul „Evaluate Model“

In diesem Artikel wird ein Modul der grafischen Benutzeroberfläche (Vorschau) für Azure Machine Learning Service beschrieben.

Mithilfe dieses Moduls können Sie die Genauigkeit eines trainierten Modells messen. Sie stellen ein Dataset zur Verfügung, das die anhand eines Modells generierten Ergebnisse enthält. Anschließend berechnet das Modul **Evaluate Model** eine Reihe branchenüblicher Auswertungsmetriken.
  
 Die Metriken, die vom Modul **Evaluate Model** zurückgegeben werden, hängen vom Typ des Modells ab, das Sie auswerten möchten:  
  
-   **Klassifizierungsmodelle**    
-   **Regressionsmodelle**    



> [!TIP]
> Wenn Sie mit der Modellauswertung noch nicht vertraut sind, empfehlen wir Ihnen die Videoreihe von Dr. Stephen Elston im Rahmen des [Kurses zum maschinellen Lernen](https://blogs.technet.microsoft.com/machinelearning/2015/09/08/new-edx-course-data-science-machine-learning-essentials/) von EdX. 


Es gibt drei Möglichkeiten zum Verwenden des Moduls **Evaluate Model**:

+ Generieren Sie Ergebnisse zu Ihren Trainingsdaten, und werten Sie das Modell anhand dieser Ergebnisse aus.
+ Generieren Sie Ergebnisse für das Modell, vergleichen Sie diese aber mit Ergebnissen für ein reserviertes Testdataset.
+ Vergleichen Sie die Ergebnisse für zwei verschiedene, aber zusammengehörige Modelle unter Verwendung desselben Datasets.

## <a name="use-the-training-data"></a>Verwenden der Trainingsdaten

Zur Auswertung eines Modells müssen Sie eine Verbindung mit einem Dataset herstellen, das mehrere Eingabespalten und Ergebnisse enthält.  Wenn keine anderen Daten verfügbar sind, können Sie Ihr ursprüngliches Dataset verwenden.

1. Verbinden Sie die Ausgabe **Scored dataset** des Moduls [Score Model](./score-model.md) mit der Eingabe des Moduls **Evaluate Model**. 
2. Klicken Sie auf das Modul **Evaluate Model**, und führen Sie die Pipeline aus, um die Auswertungsergebnisse zu generieren.

## <a name="use-testing-data"></a>Verwenden von Testdaten

Ein typisches Szenario beim maschinellen Lernen ist die Aufteilung Ihres ursprünglichen Datasets in Trainings- und Testdatasets unter Verwendung des Moduls [Split](./split-data.md) (Aufteilen) oder des Moduls [Partition and Sample](./partition-and-sample.md) (Partitionieren und Stichproben nehmen). 

1. Verbinden Sie die Ausgabe **Scored dataset** des Moduls [Score Model](score-model.md) mit der Eingabe des Moduls **Evaluate Model**. 
2. Verbinden Sie die Ausgabe des Moduls „Split“, das die Testdaten enthält, mit der rechten Eingabe von **Evaluate Model**.
2. Klicken Sie auf das Modul **Evaluate Model** und dann auf **Run selected** (Ausgewählte ausführen), um die Auswertungsergebnisse zu generieren.

## <a name="compare-scores-from-two-models"></a>Vergleichen von Ergebnissen zweier Modelle

Sie können auch eine zweite Menge von Ergebnissen mit **Evaluate Model** verbinden.  Die Ergebnisse können eine gemeinsame Bewertungsmenge mit bekannten Ergebnissen oder eine Menge von Ergebnissen aus einem anderen Modell für dieselben Daten sein.

Dieses Feature ist nützlich, da Sie die Ergebnisse zweier verschiedener Modelle auf der Grundlage derselben Daten einfach vergleichen können. Sie können auch Ergebnisse zweier verschiedener Ausführungen über dieselben Daten mit unterschiedlichen Parametern vergleichen.

1. Verbinden Sie die Ausgabe **Scored dataset** des Moduls [Score Model](score-model.md) mit der Eingabe des Moduls **Evaluate Model**. 
2. Verbinden Sie die Ausgabe des Moduls „Score Model“ für das zweite Modell mit der rechten Eingabe des **Moduls „Evaluate Model“** .
3. Klicken Sie mit der rechten Maustaste auf **Evaluate Model** und dann auf **Run selected** (Ausgewählte ausführen), um die Auswertungsergebnisse zu generieren.

## <a name="results"></a>Ergebnisse

Klicken Sie nach der Ausführung von **Evaluate Model** mit der rechten Maustaste auf das Modul, und wählen Sie **Evaluation results** (Auswertungsergebnisse) aus, um die Ergebnisse anzuzeigen. Ihre Möglichkeiten:

+ Speichern der Ergebnisse als Dataset, damit Sie sie mit anderen Tools einfacher analysieren können
+ Generieren einer Visualisierung auf der Benutzeroberfläche

Wenn Sie Datasets mit beiden Eingaben von **Evaluate Model** verbinden, enthalten die Ergebnisse Metriken für beide Datasets bzw. beide Modelle.
Das Modell oder die Daten, die an den linken Port angefügt wurden, werden zuerst im Bericht dargestellt, gefolgt von den Metriken für das Dataset oder das Modell, das an den rechten Port angefügt wurde.  

So stellt beispielsweise die folgende Abbildung einen Vergleich der Ergebnisse zweier Clusteringmodelle dar, die anhand derselben Daten, aber mit unterschiedlichen Parametern erstellt wurden.  

![AML&#95;Comparing2Models](media/module/aml-comparing2models.png "AML_Comparing2Models")  

Da es sich um ein Clusteringmodell handelt, sind die Auswertungsergebnisse anders als wenn Sie Ergebnisse zweier Regressionsmodelle vergleichen oder zwei Klassifizierungsmodelle miteinander vergleichen. Die Gesamtpräsentation ist jedoch identisch. 

## <a name="metrics"></a>metrics

Dieser Abschnitt beschreibt die Metriken, die für die bestimmten Arten von Modellen zurückgegeben werden, die für den Einsatz mit **Evaluate Model** unterstützt werden:

+ [Klassifizierungsmodelle](#bkmk_classification)
+ [Regressionsmodelle](#bkmk_regression)

###  <a name="bkmk_classification"></a> Metriken für Klassifizierungsmodelle

Die folgenden Metriken werden bei der Auswertung von Klassifizierungsmodellen erfasst. Wenn Sie Modelle vergleichen, werden sie anhand der Metrik eingestuft, die Sie für die Auswertung auswählen.  
  
-   **Accuracy** (Treffergenauigkeit) misst die Güte eines Klassifizierungsmodells als das Verhältnis der wahren Ergebnisse zur Gesamtheit der Fälle.  
  
-   **Precision** (Genauigkeit) ist der Anteil wahrer Ergebnisse an allen positiven Ergebnissen.  
  
-   **Recall** (Trefferquote) ist der Anteil aller richtigen Ergebnisse, die vom Modell zurückgegeben werden.  
  
-   **F-Score** (F-Maß) wird als gewichteter Durchschnitt von Genauigkeit und Trefferquote von 0 bis 1 berechnet, wobei 1 der ideale Wert für F-Maß ist.  
  
-   **AUC** (Fläche unter der Kurve) misst den Bereich unter der Kurve, der mit wahren positiven Werten auf der y-Achse und falschen positiven Werten auf der x-Achse gezeichnet wurde. Diese Metrik ist nützlich, da sie einen einzelnen Wert liefert, mit dem Sie Modelle verschiedener Typen vergleichen können.  
  
- **Average log loss** (logarithmische Durchschnittsdämpfung) ist ein Einzelwert, der verwendet wird, um die Strafterme für falsche Ergebnisse auszudrücken. Er wird als Differenz zwischen zwei Wahrscheinlichkeitsverteilungen berechnet – der tatsächlichen und der im Modell.  
  
- **Training log loss** (logarithmische Trainingsdämpfung) ist ein einzelner Wert, der den Vorteil des Klassifizierers gegenüber einer Zufallsvorhersage wiedergibt. Die Trainingsdämpfung misst die Unsicherheit Ihres Modells, indem die ausgegebenen Wahrscheinlichkeiten mit den bekannten Werten (Ground Truth) in den Bezeichnungen verglichen werden. Die Trainingsdämpfung sollte für das Modell als Ganzes minimiert werden.

##  <a name="bkmk_regression"></a> Metriken für Regressionsmodelle
 
Die für Regressionsmodelle zurückgegebenen Metriken sind im Allgemeinen so gestaltet, dass sie die Fehlerquote schätzen.  Ein Modell passt gut zu den Daten, wenn der Unterschied zwischen beobachteten und vorhergesagten Werten gering ist. Wenn Sie sich jedoch das Muster der Residuen (die Differenz zwischen einem beliebigen vorhergesagten Punkt und seinem entsprechenden Istwert) ansehen, können Sie viel über eine mögliche Verzerrung im Modell erfahren.  
  
 Die folgenden Metriken werden für die Auswertung von Regressionsmodellen herangezogen. Wenn Sie Modelle vergleichen, werden sie nach der Metrik eingestuft, die Sie für die Auswertung auswählen.  
  
- **Mean absolute error (MAE)** (mittlerer absoluter Fehler) misst, wie nah die Vorhersagen an den tatsächlichen Ergebnissen sind, weshalb ein niedrigerer Wert besser ist.  
  
- **Root mean squared error (RMSE)** (mittlere quadratische Abweichung ) erzeugt einen Einzelwert, der den Fehler im Modell zusammenfasst. Durch die Quadrierung der Differenz ignoriert die Metrik den Unterschied zwischen Über- und Unterprognose.  
  
- **Relative absolute error (RAE)** (relativer absoluter Fehler) ist die relative absolute Differenz zwischen erwartetem und tatsächlichem Wert; relativ, weil die mittlere Differenz durch das arithmetische Mittel dividiert wird.  
  
- **Relative squared error (RSE)** (relativer quadratischer Fehler) normalisiert ebenfalls den gesamten quadrierten Fehler der vorhergesagten Werte durch Division durch den gesamten quadrierten Fehler der Istwerte.  
  
- **Mean Zero One Error (MZOE)** (mittlerer absoluter Fehler) gibt an, ob die Vorhersage richtig war oder nicht.  Mit anderen Worten: `ZeroOneLoss(x,y) = 1` bei `x!=y`, andernfalls `0`.
  
- **Coefficient of determination** (Bestimmtheitsmaß), oft auch als R<sup>2</sup> bezeichnet, stellt die Vorhersagekraft des Modells als Wert von 0 bis 1 dar. 0 bedeutet, dass das Modell zufällig ist (also nichts erklärt). 1 bedeutet, dass es eine perfekte Anpassung gibt. Bei der Interpretation der R<sup>2</sup>-Werte ist jedoch Vorsicht geboten, da niedrige Werte völlig normal und hohe Werte verdächtig sein können.
  

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die [Gruppe der verfügbaren Module](module-reference.md) für Azure Machine Learning Service an. 