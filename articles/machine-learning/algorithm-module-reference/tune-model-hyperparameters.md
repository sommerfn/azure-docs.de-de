---
title: Tune Model Hyperparameters
titleSuffix: Azure Machine Learning service
description: Es wird beschrieben, wie Sie das Modul „Tune Model Hyperparameters“ (Optimieren von Modellhyperparametern) in Azure Machine Learning Service verwenden, um eine Parameterbereinigung (Parameter Sweep) für ein Modell auszuführen und so die optimalen Parametereinstellungen zu bestimmen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/16/2019
ms.openlocfilehash: fd796297bafeb437b55eca7f38cbd7ae55e19b93
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73716724"
---
# <a name="tune-model-hyperparameters"></a>Tune Model Hyperparameters

In diesem Artikel wird die Verwendung des Moduls „Tune Model Hyperparameters“ im Azure Machine Learning-Designer (Vorschauversion) beschrieben. Das Ziel besteht darin, die optimalen Hyperparameter für ein Machine Learning-Modell zu ermitteln. Vom Modul werden mehrere Modelle erstellt und getestet, indem verschiedene Kombinationen von Einstellungen verwendet werden. Die Metriken werden über alle Modelle hinweg verglichen, um die Kombinationen der Einstellungen zu erhalten. 

Die Begriffe *Parameter* und *Hyperparameter* können verwirrend sein. Die *Parameter* des Modells sind die Optionen, die Sie im Eigenschaftenbereich festgelegt haben. Im Grunde genommen führt dieses Modul eine *Parameterbereinigung* für die angegebenen Parametereinstellungen durch. Es wird ein optimaler Satz mit _Hyperparametern_ erlernt. Diese können sich für jede spezifische Entscheidungsstruktur, jedes Dataset oder jede Regressionsmethode unterscheiden. Der Prozess zum Finden der optimalen Konfiguration wird manchmal als *Optimierung* bezeichnet. 

Das Modul unterstützt die folgende Methode zum Ermitteln der optimalen Einstellungen für ein Modell: *Integriertes Trainieren und Optimieren*. Bei dieser Methode konfigurieren Sie einen Satz mit Parametern zur späteren Verwendung. Anschließend können Sie für das Modul den Durchlauf mit mehreren Kombinationen durchführen. Das Modul misst die Genauigkeit, bis das „beste“ Modell ermittelt wurde. Bei den meisten Lernmodulen können Sie wählen, welche Parameter während des Trainingsprozesses geändert werden und welche unverändert bleiben sollen.

Abhängig davon, wie lange der Optimierungsprozess ausgeführt werden soll, treffen Sie unter Umständen die Entscheidung, alle Kombinationen eingehend zu testen. Sie können den Prozess auch verkürzen, indem Sie ein Raster mit Parameterkombinationen einrichten und eine zufällig ausgewählte Teilmenge des Parameterrasters testen.

Diese Methode generiert ein trainiertes Modell, das Sie zur Wiederverwendung speichern können.  

> [!TIP] 
> Sie können eine entsprechende Aufgabe ausführen. Vor Beginn einer Optimierung sollten Sie eine Auswahl von Merkmalen (Featureauswahl) anwenden, um die Spalten oder Variablen zu bestimmen, die den größten Informationswert haben.

## <a name="how-to-configure-tune-model-hyperparameters"></a>Konfigurieren von „Tune Model Hyperparameters“  

Zum Erlernen der optimalen Hyperparameter für ein Machine Learning-Modell ist eine Pipelinenutzung in erheblichem Maße erforderlich.

### <a name="train-a-model-by-using-a-parameter-sweep"></a>Trainieren eines Modells mit einer Parameterbereinigung  

In diesem Abschnitt wird beschrieben, wie eine einfache Parameterbereinigung ausgeführt wird, bei der ein Modell mit dem Modul „Tune Model Hyperparameters“ trainiert wird.

1.  Fügen Sie das Modul „Tune Model Hyperparameters“ Ihrer Pipeline im Designer hinzu.

2.  Verbinden Sie ein untrainiertes Modell mit der am weitesten links befindlichen Eingabe. 

3. Legen Sie die Option **Create trainer mode** (Trainermodus erstellen) auf **Parameter Range** (Parameterbereich) fest. Verwenden Sie **Range Builder** (Bereichs-Generator), um einen Wertebereich für die Verwendung bei der Parameterbereinigung anzugeben.  

    Fast alle Klassifizierungs- und Regressionsmodule unterstützen eine integrierte Parameterbereinigung. Für Lernmodule, bei denen das Konfigurieren eines Parameterbereichs nicht unterstützt wird, können Sie auch nur die verfügbaren Parameterwerte testen.

    Sie können den Wert für einen oder mehrere Parameter manuell festlegen und die Bereinigung dann für die restlichen Parameter ausführen. Dadurch lässt sich möglicherweise einige Zeit sparen.

4.  Fügen Sie das Dataset hinzu, das Sie für das Training verwenden möchten, und verbinden Sie es mit der mittleren Eingabe von „Tune Model Hyperparameters“.  

    Wenn Sie ein mit Tags versehenes Dataset haben, können Sie es optional mit dem am weitesten rechts befindlichen Eingabeport (**Optional validation dataset**, „Optionales Validierungsdataset“) verbinden. Dies ermöglicht es Ihnen, die Genauigkeit beim Training und bei der Optimierung zu messen.

5.  Wählen Sie im Bereich **Properties** (Eigenschaften) von „Tune Model Hyperparameters“ einen Wert für **Parameter sweeping mode** (Parameterbereinigungsmodus) aus. Mit dieser Option wird gesteuert, wie die Parameter ausgewählt werden.

    - **Entire grid** (Gesamtes Raster): Wenn Sie diese Option auswählen, durchläuft das Modul ein vom System vordefiniertes Raster in einer Schleife, um unterschiedliche Kombinationen auszuprobieren und das beste Lernmodul zu ermitteln. Diese Option ist nützlich, falls Sie die besten Parametereinstellungen nicht kennen und alle möglichen Kombinationen von Werten ausprobieren möchten.

    - **Random sweep** (Zufällige Bereinigung): Wenn Sie diese Option auswählen, wählt das Modul nach dem Zufallsprinzip Parameterwerte für einen vom System definierten Bereich aus. Sie müssen die maximale Anzahl von Ausführungen angeben, die das Modul ausführen soll. Diese Option ist nützlich, wenn Sie die Modellleistung mit den von Ihnen gewünschten Metriken steigern, aber weiterhin Computeressourcen sparen möchten.    

6.  Öffnen Sie für **Label column** (Bezeichnungsspalte) die Spaltenauswahl, um eine einzelne Bezeichnungsspalte auszuwählen.

7.  Auswählen der Anzahl von Ausführungen:

    1. **Maximum number of runs on random sweep**: Wenn Sie eine zufällige Bereinigung auswählen, können Sie angeben, wie oft das Modell mit einer zufälligen Kombination von Parameterwerten trainiert werden soll.

    2. **Maximum number of runs on random grid** (Maximale Anzahl von Ausführungen für Zufallsraster): Mit dieser Option wird auch die Anzahl von Iterationen für zufällige Stichproben von Parameterwerten gesteuert, aber die Werte werden nicht zufällig aus dem angegebenen Bereich generiert. Stattdessen wird vom Modul eine Matrix mit allen möglichen Kombinationen von Parameterwerten erstellt. Anschließend wird eine zufällige Stichprobe für die Matrix genommen. Diese Methode ist effizienter und weniger anfällig für regionale Überquotierung oder Unterquotierung.

8.  Wählen Sie unter **Rang** eine Metrik aus, die zum Zuweisen eines Rangs für die Modelle verwendet werden soll.

    Wenn Sie eine Parameterbereinigung ausführen, werden vom Modul alle zutreffenden Metriken für den Modelltyp berechnet und im Bericht **Sweep results** (Bereinigungsergebnisse) zurückgegeben. Das Modul nutzt separate Metriken für Regressions- und Klassifizierungsmodelle.

    Die von Ihnen gewählte Metrik bestimmt aber, wie die Modelle eingestuft werden. Nur das oberste Modell, eingestuft entsprechend der gewählten Metrik, wird als trainiertes Modell ausgegeben, das für die Bewertung verwendet werden soll.

9.  Geben Sie für **Random seed** (Zufälliger Ausgangswert) eine Zahl ein, die zum Starten der Parameterbereinigung verwendet werden soll. 

10. Ausführen der Pipeline.

## <a name="results-of-hyperparameter-tuning"></a>Ergebnisse einer Hyperparameteroptimierung

Nach Abschluss des Trainings:

+ Um einen Satz von Genauigkeitsmetriken für das beste Modell anzuzeigen, klicken Sie mit der rechten Maustaste auf das Modul, wählen Sie **Sweep results** aus, und wählen Sie dann **Visualize** aus.

    Die Ausgabe enthält alle Genauigkeitsmetriken, die für den Modelltyp zutreffen. Aber anhand der Metrik, die Sie für die Einstufung ausgewählt haben, wird bestimmt, welches Modell als das „beste“ angesehen wird.

+ Falls das Modell für Bewertungen in anderen Pipelines verwendet werden soll, ohne den Optimierungsprozess wiederholen zu müssen, klicken Sie mit der rechten Maustaste auf die Modellausgabe und wählen **Save as Trained Model** (Als trainiertes Modell speichern) aus. 


## <a name="technical-notes"></a>Technische Hinweise

Dieser Abschnitt enthält Details und Tipps zur Implementierung.

### <a name="how-a-parameter-sweep-works"></a>Funktionsweise einer Parameterbereingung

Beim Einrichten einer Parameterbereinigung definieren Sie den Bereich Ihrer Suche. Bei der Suche kann eine endliche Anzahl von zufällig ausgewählten Parametern verwendet werden. Es kann auch eine umfassende Suche in einem von Ihnen definierten Parameterbereich durchgeführt werden.

+ **Random sweep** (Zufällige Bereinigung): Mit dieser Option wird ein Modell mit einer festgelegten Anzahl von Iterationen trainiert. 

  Sie geben einen Wertebereich an, der durchlaufen werden soll, und im Modul wird eine zufällig gewählte Teilmenge dieser Werte verwendet. Werte werden mit Ersetzung ausgewählt, d. h., dass zuvor zufällig ausgewählte Zahlen nicht aus dem Pool der verfügbaren Zahlen entfernt werden. Daher bleibt die Wahrscheinlichkeit für die Auswahl eines Werts in allen Durchläufen gleich.  

+ **Entire grid** (Gesamtes Raster): Die Option zum Verwenden des gesamten Rasters bedeutet, dass jede mögliche Kombination getestet wird. Diese Option ist die gründlichste, benötigt aber auch die meiste Zeit. 

### <a name="controlling-the-length-and-complexity-of-training"></a>Steuern der Länge und Komplexität eines Trainings

Ein Iterieren über viele Kombinationen von Einstellungen kann zeitaufwändig sein, daher bietet das Modul mehrere Möglichkeiten, den Prozess zu beschränken:

+ Begrenzen der Anzahl von Iterationen, die zum Testen eines Modells verwendet werden
+ Begrenzen des Parameterbereichs
+ Begrenzen der Anzahl von Iterationen und des Parameterbereichs

Es empfiehlt sich, mit den Einstellungen zu experimentieren, um die effizienteste Methode des Trainings für ein bestimmtes Dataset und Modell zu ermitteln.

### <a name="choosing-an-evaluation-metric"></a>Auswählen einer Auswertungsmetrik

Am Ende des Testvorgangs wird vom Modell ein Bericht angezeigt, in dem die Genauigkeit für jedes Modell angegeben ist, damit Sie die Metrikergebnisse überprüfen können:

- Für alle Modelle für die Binärklassifizierung wird ein einheitlicher Metriksatz verwendet.
- Die Genauigkeit wird für alle Klassifizierungsmodelle mit mehreren Klassen verwendet.
- Für Regressionsmodelle wird ein anderer Metriksatz genutzt. 

Während eines Trainings müssen Sie jedoch eine *einzige* Metrik auswählen, die für die Einstufung der Modelle verwendet werden soll, die während des Optimierungsprozesses generiert werden. Unter Umständen stellen Sie fest, dass die beste Metrik je nach Ihrem Geschäftsproblem und dem Aufwand für falsch positive und falsche negative Ergebnisse variiert.

#### <a name="metrics-used-for-binary-classification"></a>Metriken, die für Binärklassifizierung verwendet werden

-   **Accuracy** (Treffergenauigkeit): Anteil von richtigen Ergebnissen an den Gesamtfällen  

-   **Precision** (Genauigkeit): Anteil von richtigen Ergebnisse an allen positiven Ergebnissen  

-   **Recall** (Trefferquote): Anteil aller richtigen Ergebnisse an allen Ergebnissen  

-   **F-score** (F-Maß): Maß, das Genauigkeit und Trefferquote abgleicht  

-   **AUC**: Wert, der den Bereich unter der Kurve darstellt, wenn falsch positive Ergebnisse auf der x-Achse geplottet und richtig positive Ergebnisse auf der y-Achse geplottet werden  

-   **Average Log Loss** (Logarithmische Durchschnittsdämpfung): Differenz zwischen zwei Wahrscheinlichkeitsverteilungen – der tatsächlichen und der im Modell  

#### <a name="metrics-used-for-regression"></a>Metriken, die für Regression verwendet werden

-   **Mean absolute error** (Mittlerer absoluter Fehler): Mittelt den gesamten Fehler im Modell, wobei mit *Fehler* der Abstand des vorhergesagten Werts zum wahren Wert gemeint ist. Wird häufig als *MAE* abgekürzt.  

-   **Root mean squared error** (Standardfehler der Regression) misst den Durchschnitt der Quadrate der Fehler und berechnet dann die Quadratwurzel dieses Werts. Wird häufig als *RMSE* abgekürzt.  

-   **Relative absolute error** (Relativer absoluter Fehler) entspricht dem Fehler als Prozentsatz des richtigen Werts.  

-   **Relative squared error** (Relativer quadratischer Fehler): Normalisiert den gesamten quadratischen Fehler, indem dieser durch den gesamten quadratischen Fehler der vorhergesagten Werte dividiert wird.  

-   **Coefficient of determination** (Bestimmtheitsmaß): Einzelne Zahl, die angibt, wie gut Daten zu einem Modell passen. Der Wert „1“ bedeutet, dass das Modell genau mit den Daten übereinstimmt. Der Wert „0“ bedeutet, dass die Daten zufälliger Art sind oder aus anderen Gründen nicht an das Modell angepasst werden können. Häufig lautet die Bezeichnung hierfür *r<sup>2</sup>* , *R<sup>2</sup>* oder *r-squared* (R zum Quadrat).  

### <a name="modules-that-dont-support-a-parameter-sweep"></a>Module, die keine Parameterbereinigung unterstützen

Fast alle Lernmodule in Azure Machine Learning unterstützen Kreuzvalidierung mit einer integrierten Parameterbereinigung, mit der Sie die Parameter auswählen können, mit denen experimentiert werden soll. Ist es nicht möglich, für das Lernmodul einen Wertebereich festzulegen, können Sie es trotzdem in Kreuzvalidierungen verwenden. In diesem Fall wird ein Bereich mit zulässigen Werten für die Bereinigung ausgewählt. 


## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die [Gruppe der verfügbaren Module](module-reference.md) für den Azure Machine Learning Service an. 

