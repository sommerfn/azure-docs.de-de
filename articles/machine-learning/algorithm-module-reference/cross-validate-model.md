---
title: 'Cross Validate Model: Modulreferenz'
titleSuffix: Azure Machine Learning service
description: Erfahren Sie, wie Sie das Modul Cross Validate Model in Azure Machine Learning Service verwenden, um Parameterschätzungen für Klassifizierungs- oder Regressionsmodelle durch Partitionieren der Daten per Kreuzvalidierung überprüfen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/10/2019
ms.openlocfilehash: a5eea61ee8284010531e80e17bf1110ab470d04c
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510287"
---
# <a name="cross-validate-model"></a>Durchführen einer Kreuzvalidierung für ein Modell

Dieser Artikel beschreibt die Verwendung des Moduls **Cross Validate Model** im Azure Machine Learning-Designer (Vorschauversion). Die *Kreuzvalidierung* ist eine wichtige Technik, die häufig beim maschinellen Lernen verwendet wird, um gleichzeitig die Variabilität eines Datasets und die Zuverlässigkeit eines mit diesen Daten trainierten Modells zu bewerten.  

Das Modul **Cross Validate Model** nimmt als Eingabe ein bezeichnetes Dataset und ein untrainiertes Klassifizierungs- oder Regressionsmodell entgegen. Das Dataset wird in eine bestimmte Anzahl von Teilmengen (*Folds*) aufgeteilt, für jeden Fold wird ein Modell erstellt, und anschließend wird ein Satz von Genauigkeitsstatistiken für jeden Fold zurückgegeben. Durch Vergleichen der Genauigkeitsstatistik für alle Folds können Sie die Qualität des Datasets interpretieren und beurteilen, ob das Modell für Schwankungen in den Daten anfällig ist.  

Bei der Kreuzvalidierung werden auch vorhergesagte Ergebnisse und Wahrscheinlichkeiten für das Dataset zurückgegeben, damit Sie die Zuverlässigkeit der Vorhersagen beurteilen können.  

### <a name="how-cross-validation-works"></a>Funktionsweise der Kreuzvalidierung

1. Bei der Kreuzvalidierung werden die Trainingsdaten nach dem Zufallsprinzip in eine Reihe von Partitionen aufgeteilt, die auch als *Folds* bezeichnet werden. 

    + Der Algorithmus ist standardmäßig auf 10 Folds eingestellt, wenn Sie das Dataset zuvor nicht partitioniert haben. 
    + Wenn Sie das Dataset in eine andere Anzahl von Folds aufteilen möchten, können Sie das Modul [Partition and Sample](partition-and-sample.md) verwenden und angeben, wie viele Folds verwendet werden sollen.  

2.  Das Modul stellt die Daten in Fold 1 für die Validierung zurück (manchmal als *Zurückhaltungsfold* bezeichnet) und verwendet die verbleibenden Folds zum Trainieren eines Modells. 

    Wenn Sie z. B. fünf Folds erstellen, generiert das Modul bei der Kreuzvalidierung fünf Modelle, wobei jedes Modell mit 4/5 der Daten trainiert und mit dem verbleibenden 1/5 getestet wird.  

3.  Beim Testen des Modells für jeden Fold werden mehrere Genauigkeitsstatistiken ausgewertet. Welche Statistiken verwendet werden, hängt vom Typ des ausgewerteten Modells ab. Zum Auswerten von Klassifizierungsmodellen und von Regressionsmodellen werden unterschiedliche Statistiken verwendet.  

4.  Wenn der Erstellungs- und Evaluierungsvorgang für alle Folds abgeschlossen ist, generiert **Cross Validate Model** einen Satz von Leistungsmetriken und bewerteten Ergebnissen für alle Daten. Sie sollten diese Metriken überprüfen, um festzustellen, ob ein einzelner Fold eine besonders hohe oder niedrige Genauigkeit aufweist. 

### <a name="advantages-of-cross-validation"></a>Vorteile der Kreuzvalidierung

Eine andere und gängige Methode zum Auswerten eines Modells besteht darin, die Daten mithilfe von [Split Data](split-data.md) in einen Trainings- und einen Testsatz zu unterteilen und das Modell dann mit den Trainingsdaten zu überprüfen. Die Kreuzvalidierung bietet jedoch einige Vorteile:  

-   Bei der Kreuzvalidierung werden mehr Testdaten verwendet.

     Bei der Kreuzvalidierung wird die Leistung des Modells mit den angegebenen Parametern in einem größeren Datenraum gemessen. Das heißt, bei der Kreuzvalidierung wird das gesamte Trainingsdataset für Training und Auswertung verwendet und nicht nur ein Teil. Wenn Sie ein Modell dagegen mithilfe von Daten validieren, die aus einer zufälligen Unterteilung generiert werden, beruht die Auswertung des Modells in der Regel nur auf höchstens 30 % der verfügbaren Daten.  

     Da jedoch bei der Kreuzvalidierung das Modell mehrmals mit einem größeren Dataset trainiert und überprüft wird, ist dieser Vorgang weitaus rechenintensiver und erfordert wesentlich mehr Zeit als die Validierung mit einer zufälligen Aufteilung.  

-   Bei der Kreuzvalidierung werden sowohl das Dataset als auch das Modell ausgewertet.

     Es wird bei der Kreuzvalidierung nicht nur die Genauigkeit eines Modells gemessen, sondern auch erläutert, wie repräsentativ das Dataset ist und wie empfindlich das Modell möglicherweise gegenüber Schwankungen in den Daten ist.  

## <a name="how-to-use-cross-validate-model"></a>Verwenden von Cross Validate Model

Die Kreuzvalidierung kann viel Zeit in Anspruch nehmen, wenn Ihr Dataset groß ist.  Daher können Sie **Cross Validate Model** in der Anfangsphase des Erstellen und Testens Ihres Modells verwenden, um die Eignung der Modellparameter auszuwerten (sofern die Berechnungszeit hingenommen werden kann). Anschließend trainieren Sie das Modell und werten es anhand der etablierten Parameter mit den Modulen [Train Model](train-model.md) und [Evaluate Model](evaluate-model.md) aus.

In diesem Szenario trainieren und testen Sie das Modell mit **Cross Validate Model**.

1. Fügen Sie das Modul **Cross Validate Model** Ihrer Pipeline hinzu. Sie finden es im Azure Machine Learning-Designer in der Kategorie **Model Scoring & Evaluation** (Modellbewertung und -auswertung). 

2. Verbinden Sie die Ausgabe eines beliebigen **Klassifizierungs-** oder **Regressionsmodells**. 

    Wenn Sie z. B. eine **Bayes-Punktmaschine mit zwei Klassen** für die Klassifizierung verwenden, konfigurieren Sie das Modell mit den gewünschten Parametern und ziehen dann einen Connector aus dem Port **Untrained Model** des Klassifizierers auf den entsprechenden Port von **Cross Validate Model**. 

    > [!TIP] 
    > Das Modell muss nicht trainiert werden, da **Cross Validate Model** das Modell im Rahmen der Auswertung automatisch trainiert.  
3.  Verbinden Sie den Port **Dataset** von **Cross Validate Model** mit einem gekennzeichneten Trainingsdataset.  

4.  Klicken Sie im Bereich **Eigenschaften** von **Cross Validate Model** auf **Launch column selector** (Spaltenauswahl starten), und wählen Sie die einzelne Spalte aus, die die Klassenbezeichnung oder den vorhersagbaren Wert enthält. 

5. Legen Sie einen Wert für den Parameter **Zufälliger Ausgangswert** fest, wenn Sie die Ergebnisse der Kreuzvalidierung über aufeinanderfolgende Ausführungen für dieselben Daten hinweg wiederholen möchten.  

6.  Ausführen der Pipeline.

7. Eine Beschreibung der Berichte finden Sie im Abschnitt [Ergebnisse](#results).

    Um eine Kopie des Modells zur späteren Wiederverwendung zu erhalten, klicken Sie mit der rechten Maustaste auf die Ausgabe des Moduls, das den Algorithmus enthält (z. B. die **Bayes-Punktmaschine mit zwei Klassen**), und klicken Sie dann auf **Save as Trained Model** (Als trainiertes Modell speichern).

## <a name="results"></a>Ergebnisse

Nachdem alle Iterationen abgeschlossen sind, erstellt **Cross Validate Model** Scores für das gesamte Dataset sowie Leistungsmetriken, mit denen Sie die Qualität des Modells bewerten können.

### <a name="scored-results"></a>Bewertete Ergebnisse

Die erste Ausgabe des Moduls stellt die Quelldaten für jede Zeile zusammen mit einigen vorhergesagten Werten und den zugehörigen Wahrscheinlichkeiten bereit. 

Klicken Sie zum Anzeigen dieser Ergebnisse in der Pipeline mit der rechten Maustaste auf das Modul **Cross Validate Model**, wählen Sie **Scored results** (Bewertete Ergebnisse) aus, und klicken Sie auf **Visualisieren**.

| Neuer Spaltenname      | BESCHREIBUNG                              |
| -------------------- | ---------------------------------------- |
| Scored Labels (Bewertete Bezeichnungen)        | Diese Spalte wird am Ende des Datasets hinzugefügt und enthält den vorhergesagten Wert für jede Zeile. |
| Scored Probabilities (Bewertete Wahrscheinlichkeiten) | Diese Spalte wird am Ende des Datasets hinzugefügt und gibt die geschätzte Wahrscheinlichkeit des Werts in **Scored Labels** (Bewertete Bezeichnungen) an. |
| Fold Number (Foldnummer)          | Gibt den Index des Folds beginnend bei 0 an, dem die jeweiligen Datenzeilen bei der Kreuzvalidierung zugewiesen wurden. |

 ### <a name="evaluation-results"></a>Auswertung der Ergebnisse

Der zweite Bericht wird nach Folds gruppiert. Denken Sie daran, dass **Cross Validate Model** bei der Ausführung die Trainingsdaten nach dem Zufallsprinzip in *n* Folds (standardmäßig 10) aufteilt. In jeder Iterationen über das Dataset verwendet **Cross Validate Model** einen Fold als Validierungsdataset und die verbleibenden *n-1* Folds zum Trainieren eines Modells. Jedes der *n* Modelle wird mit den Daten in allen anderen Folds getestet.

In diesem Bericht werden die Folds nach Indexwert in aufsteigender Reihenfolge aufgelistet.  Wenn Sie nach einer anderen Spalte sortieren möchten, können Sie die Ergebnisse als Dataset speichern.

Klicken Sie zum Anzeigen dieser Ergebnisse in der Pipeline mit der rechten Maustaste auf das Modul **Cross Validate Model**, wählen Sie **Evaluation results by fold** (Auswertungsergebnisse nach Fold) aus, und klicken Sie auf **Visualisieren**.


|Spaltenname| BESCHREIBUNG|
|----|----|
|Fold number (Foldnummer)| Ein Bezeichner für jeden Fold. Wenn Sie 5 Folds erstellt haben, gibt es 5 Teilmengen von Daten, die von 0 bis 4 nummeriert sind.
|Number of examples in fold (Anzahl von Beispielen im Fold)|Die Anzahl der jedem Fold zugewiesenen Zeilen. Diese sollten ungefähr gleich sein. |


Darüber hinaus sind für jeden Fold die folgenden Metriken abhängig vom Typ des ausgewerteten Modells enthalten. 

+ **Klassifizierungsmodelle:** Präzision, Rückruf, F-Score, AUC, Genauigkeit  

+ **Regressionsmodelle:** Mittlerer absoluter Fehler, effektiver absoluter Fehler, relativer absoluter Fehler, relativer quadratischer Fehler und Bestimmungskoeffizient


## <a name="technical-notes"></a>Technische Hinweise  

+ Es empfiehlt sich, Datasets vor der Verwendung für die Kreuzvalidierung zu normalisieren. 

+ Da bei **Cross Validate Model** das Modell mehrmals trainiert und überprüft wird, ist es wesentlich rechenintensiver und nimmt mehr Zeit in Anspruch, als wenn Sie das Modell mit einem zufällig aufgeteilten Dataset validieren. 

+ Das Dataset muss nicht in Trainings- und Testsätze aufgeteilt werden, wenn Sie die Genauigkeit des Modells mithilfe der Kreuzvalidierung messen. 


## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die [Gruppe der verfügbaren Module](module-reference.md) für Azure Machine Learning Service an. 

