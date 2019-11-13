---
title: 'Cross Validate Model: Modulreferenz'
titleSuffix: Azure Machine Learning service
description: Es wird beschrieben, wie Sie das Modul „Cross Validate Model“ in Azure Machine Learning Service verwenden, um Parameterschätzungen für Klassifizierungs- oder Regressionsmodelle durch Partitionieren der Daten per Kreuzvalidierung überprüfen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/10/2019
ms.openlocfilehash: d83a9b5df7acc9d626613e53369f483367e55a54
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73717239"
---
# <a name="cross-validate-model"></a>Durchführen einer Kreuzvalidierung für ein Modell

In diesem Artikel wird die Verwendung des Moduls „Cross Validate Model“ im Azure Machine Learning-Designer (Vorschauversion) beschrieben. Die *Kreuzvalidierung* ist ein Verfahren, das häufig beim maschinellen Lernen verwendet wird, um gleichzeitig die Variabilität eines Datasets und die Zuverlässigkeit eines mit diesen Daten trainierten Modells zu bewerten.  

Für das Modul „Cross Validate Model“ wird als Eingabe ein bezeichnetes Dataset und ein untrainiertes Klassifizierungs- oder Regressionsmodell verwendet. Das Dataset wird in eine bestimmte Anzahl von Teilmengen (*Folds*) aufgeteilt, für jeden Fold wird ein Modell erstellt, und anschließend wird ein Satz von Genauigkeitsstatistiken für jeden Fold zurückgegeben. Indem Sie die Genauigkeitsstatistiken für alle Folds vergleichen, können Sie die Qualität des Datasets interpretieren. Sie können dann ermitteln, ob das Modell anfällig für Datenabweichungen ist.  

Beim Modul „Cross Validate Model“ werden auch vorhergesagte Ergebnisse und Wahrscheinlichkeiten für das Dataset zurückgegeben, damit Sie die Zuverlässigkeit der Vorhersagen beurteilen können.  

### <a name="how-cross-validation-works"></a>Funktionsweise der Kreuzvalidierung

1. Bei der Kreuzvalidierung werden Trainingsdaten nach dem Zufallsprinzip in Folds unterteilt. 

   Der Algorithmus ist standardmäßig auf 10 Folds eingestellt, wenn Sie das Dataset zuvor nicht partitioniert haben. Wenn Sie das Dataset in eine andere Anzahl von Folds aufteilen möchten, können Sie das Modul [Partition and Sample](partition-and-sample.md) verwenden und angeben, wie viele Folds verwendet werden sollen.  

2.  Das Modul hält die Daten in Fold 1 für die Validierung bereit. (Wird auch als *Fold mit zurückgehaltenen Daten* bezeichnet.) Die verbleibenden Folds werden vom Modul zum Trainieren eines Modells verwendet. 

    Wenn Sie beispielsweise fünf Folds erstellen, werden vom Modul während der Kreuzvalidierung fünf Modelle generiert. Das Modul trainiert jedes Modell, indem vier Fünftel der Daten verwendet werden. Jedes Modell wird mit dem verbleibenden Fünftel getestet.  

3.  Beim Testen des Modells für jeden Fold wertet das Modul mehrere Genauigkeitsstatistiken aus. Welche Statistik das Modul verwendet, hängt vom Typ des ausgewerteten Modells ab. Zum Auswerten von Klassifizierungs- und Regressionsmodellen werden unterschiedliche Statistiken verwendet.  

4.  Wenn der Erstellungs- und Evaluierungsvorgang für alle Folds abgeschlossen ist, generiert „Cross Validate Model“ einen Satz mit Leistungsmetriken und bewerteten Ergebnissen für alle Daten. Überprüfen Sie diese Metriken, um festzustellen, ob ein einzelner Fold eine hohe oder niedrige Genauigkeit aufweist. 

### <a name="advantages-of-cross-validation"></a>Vorteile der Kreuzvalidierung

Eine andere gängige Methode zum Auswerten eines Modells besteht darin, die Daten mithilfe von [Split Data](split-data.md) in einen Trainings- und einen Testsatz zu unterteilen und das Modell dann mit den Trainingsdaten zu überprüfen. Die Kreuzvalidierung bietet aber einige Vorteile:  

-   Bei der Kreuzvalidierung werden mehr Testdaten verwendet.

    Bei der Kreuzvalidierung wird die Leistung des Modells mit den angegebenen Parametern in einem größeren Datenraum gemessen. Das heißt, dass bei der Kreuzvalidierung das gesamte Trainingsdataset für Training und Auswertung verwendet wird (und nicht nur ein Teil). Wenn Sie ein Modell dagegen mithilfe von Daten validieren, die aus einer zufälligen Unterteilung generiert werden, beruht die Auswertung des Modells in der Regel nur auf höchstens 30 Prozent der verfügbaren Daten.  

    Da das Modell bei der Kreuzvalidierung aber mehrere Male mit einem größeren Dataset trainiert wird, ist der Computeaufwand deutlich höher. Dieser Vorgang dauert wesentlich länger als die Validierung bei einer zufälligen Aufteilung.  

-   Bei der Kreuzvalidierung werden sowohl das Dataset als auch das Modell ausgewertet.

    Die Kreuzvalidierung umfasst nicht nur die Messung der Genauigkeit eines Modells. Sie erhalten auch einen Eindruck, wie repräsentativ das Dataset ist und wie sensibel das Modell ggf. auf Datenabweichungen reagiert.  

## <a name="how-to-use-cross-validate-model"></a>Verwenden von Cross Validate Model

Die Kreuzvalidierung kann viel Zeit in Anspruch nehmen, wenn Ihr Dataset groß ist.  Es kann also ratsam sein, das Modul „Cross Validate Model“ in der anfänglichen Erstellungs- und Testphase Ihres Modells zu verwenden. In dieser Phase können Sie die Eignung der Modellparameter auswerten (vorausgesetzt, die Computedauer ist akzeptabel). Sie können Ihr Modell dann trainieren und auswerten, indem Sie die vorhandenen Parameter mit den Modulen [Train Model](train-model.md) (Modell trainieren) und [Evaluate Model](evaluate-model.md) (Modell auswerten) verwenden.

In diesem Szenario verwenden Sie „Cross Validate Model“, um das Modell zu trainieren und zu testen.

1. Fügen Sie das Modul „Cross Validate Model“ Ihrer Pipeline hinzu. Sie finden es im Azure Machine Learning-Designer in der Kategorie **Model Scoring & Evaluation** (Modellbewertung und -auswertung). 

2. Verbinden Sie die Ausgabe eines beliebigen Klassifizierungs- oder Regressionsmodells. 

    Wenn Sie beispielsweise **Two Class Bayes Point Machine** für die Klassifizierung verwenden, sollten Sie das Modell mit den gewünschten Parametern konfigurieren. Ziehen Sie anschließend einen Connector vom Port **Untrainiertes Modell** zum Klassifizierer des passenden Ports von „Cross Validate Model“. 

    > [!TIP] 
    > Sie müssen das Modell nicht trainieren, weil dies von „Cross Validate Model“ im Rahmen der Auswertung automatisch durchgeführt wird.  
3.  Verbinden Sie den Port **Dataset** von „Cross Validate Model“ mit einem gekennzeichneten Trainingsdataset.  

4.  Wählen Sie im Bereich mit den **Eigenschaften** von „Cross Validate Model“ die Option **Launch column selector** (Spaltenauswahl starten). Wählen Sie die einzelne Spalte mit der Klassenbezeichnung oder den vorhersagbaren Wert aus. 

5. Legen Sie einen Wert für den Parameter **Zufälliger Ausgangswert** fest, wenn Sie die Ergebnisse der Kreuzvalidierung über aufeinanderfolgende Ausführungen für dieselben Daten hinweg wiederholen möchten.  

6. Ausführen der Pipeline.

7. Eine Beschreibung der Berichte finden Sie im Abschnitt [Ergebnisse](#results).

    Um eine Kopie des Modells zur späteren Wiederverwendung zu erhalten, klicken Sie mit der rechten Maustaste auf die Ausgabe des Moduls, das den Algorithmus enthält (z. B. **Two Class Bayes Point Machine**). Wählen Sie anschließend die Option **Save as Trained Model** (Als trainiertes Modell speichern) aus.

## <a name="results"></a>Ergebnisse

Nach Abschluss aller Iterationen werden von „Cross Validate Model“ Bewertungen für das gesamte Dataset erstellt. Außerdem werden Leistungsmetriken erstellt, die Sie zum Bewerten der Qualität des Modells verwenden können.

### <a name="scored-results"></a>Bewertete Ergebnisse

Die erste Ausgabe des Moduls stellt die Quelldaten für jede Zeile zusammen mit einigen vorhergesagten Werten und den zugehörigen Wahrscheinlichkeiten bereit. 

Klicken Sie zum Anzeigen dieser Ergebnisse in der Pipeline mit der rechten Maustaste auf das Modul „Cross Validate Model“. Wählen Sie **Scored results** (Bewertete Ergebnisse) und dann **Visualize** (Visualisieren) aus.

| Neuer Spaltenname      | BESCHREIBUNG                              |
| -------------------- | ---------------------------------------- |
| Scored Labels (Bewertete Bezeichnungen)        | Diese Spalte wird am Ende des Datasets hinzugefügt. Sie enthält den vorhergesagten Wert für jede Zeile. |
| Scored Probabilities (Bewertete Wahrscheinlichkeiten) | Diese Spalte wird am Ende des Datasets hinzugefügt. Sie gibt die geschätzte Wahrscheinlichkeit des Werts in **Scored Labels** (Bewertete Bezeichnungen) an. |
| Fold Number (Foldnummer)          | Gibt den bei Null beginnenden Index des Folds an, dem die jeweiligen Datenzeilen bei der Kreuzvalidierung zugewiesen wurden. |

 ### <a name="evaluation-results"></a>Auswertung der Ergebnisse

Der zweite Bericht wird nach Folds gruppiert. Denken Sie daran, dass „Cross Validate Model“ die Trainingsdaten bei der Ausführung nach dem Zufallsprinzip in *n* Folds unterteilt (standardmäßig 10). Bei jeder Iteration des Datasets wird von „Cross Validate Model“ ein Fold als Validierungsdataset genutzt. Die verbleibenden *n-1* Folds werden zum Trainieren eines Modells verwendet. Jedes der *n* Modelle wird mit den Daten in allen anderen Folds getestet.

In diesem Bericht werden die Folds nach Indexwert in aufsteigender Reihenfolge aufgelistet.  Wenn Sie nach einer anderen Spalte sortieren möchten, können Sie die Ergebnisse als Dataset speichern.

Klicken Sie zum Anzeigen dieser Ergebnisse in der Pipeline mit der rechten Maustaste auf das Modul „Cross Validate Model“. Wählen Sie die Option **Evaluation results by fold** (Auswertungsergebnisse nach Fold) und dann **Visualize** (Visualisieren) aus.


|Spaltenname| BESCHREIBUNG|
|----|----|
|Fold number (Foldnummer)| Ein Bezeichner für jeden Fold. Wenn Sie fünf Folds erstellt haben, sind fünf Teilmengen mit Daten vorhanden (nummeriert von 0 bis 4).
|Number of examples in fold (Anzahl von Beispielen im Fold)|Die Anzahl der jedem Fold zugewiesenen Zeilen. Diese sollten ungefähr gleich sein. |


Darüber enthält das Modul für jeden Fold die folgenden Metriken abhängig vom Typ des ausgewerteten Modells: 

+ **Klassifizierungsmodelle:** Präzision, Rückruf, F-Score, AUC, Genauigkeit  

+ **Regressionsmodelle:** Mittlerer absoluter Fehler, effektiver absoluter Fehler, relativer absoluter Fehler, relativer quadratischer Fehler und Bestimmungskoeffizient


## <a name="technical-notes"></a>Technische Hinweise  

+ Es empfiehlt sich, Datasets vor der Verwendung für die Kreuzvalidierung zu normalisieren. 

+ Für „Cross Validate Model“ fällt ein deutlich höherer Computeaufwand an, und der Vorgang dauert länger, als wenn Sie das Modell mit einem Dataset mit zufälliger Unterteilung validieren. Der Grund ist, dass das Modell mit „Cross Validate Model“ mehrfach trainiert und validiert wird.

+ Das Dataset muss nicht in Trainings- und Testsätze aufgeteilt werden, wenn Sie die Genauigkeit des Modells mithilfe der Kreuzvalidierung messen. 


## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die [Gruppe der verfügbaren Module](module-reference.md) für den Azure Machine Learning Service an. 

