---
title: 'Permutation Feature Importance (PFI): Modulreferenz'
titleSuffix: Azure Machine Learning service
description: Erfahren Sie, wie Sie mit dem Modul „Permutation Feature Importance“ in Azure Machine Learning Service Bewertungen der Relevanz von Permutationsfeatures für Featurevariablen berechnen können, wenn ein trainiertes Modell und ein Testdataset vorliegen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/10/2019
ms.openlocfilehash: a1c3bec4b32a9d3450e307f42958bb19263c3d7a
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73717158"
---
# <a name="permutation-feature-importance"></a>Permutation Feature Importance (PFI)

In diesem Artikel wird beschrieben, wie das Modul „Permutation Feature Importance“ im Azure Machine Learning-Designer (Vorschau) eingesetzt werden kann, um einen Satz mit Bewertungen der Featurerelevanz für Ihr Dataset zu berechnen. Sie können anhand dieser Bewertungen die besten Features zur Verwendung in einem Modell bestimmen.

In diesem Modul werden Featurewerte Spalte für Spalte nach dem Zufallsprinzip neu angeordnet. Vor und nach diesem Vorgang wird die Leistung des Modells gemessen. Sie können hierbei eine der Standardmetriken zum Messen der Leistung auswählen.

Die vom Modul zurückgegebenen Bewertungen repräsentieren die *Änderung* in der Leistung eines trainierten Modells nach der Permutation. Bei wichtigen Features hat die Neuanordnung in der Regel eine größere Auswirkung und führt deshalb zu höheren Relevanzbewertungen. 

Dieser Artikel enthält eine Übersicht über das Permutationsfeature, die zugrunde liegende Theorie und seine Anwendung beim maschinellen Lernen: [Permutation Feature Importance](http://blogs.technet.com/b/machinelearning/archive/2015/04/14/permutation-feature-importance.aspx).  

## <a name="how-to-use-permutation-feature-importance"></a>Verwenden des PFI-Moduls

Um einen Satz mit Featurebewertungen zu generieren, müssen Sie bereits über ein trainiertes Modell sowie ein Testdataset verfügen.  

1.  Fügen Sie Ihrer Pipeline das Modul „Permutation Feature Importance“ hinzu. Sie finden dieses Modul in der Kategorie **Featureauswahl**. 

2.  Verbinden Sie ein trainiertes Modul mit der linken Eingabe. Es muss sich bei dem Modell um ein Regressions- oder Klassifizierungsmodell handeln.  

3.  Verbinden Sie ein Dataset mit der rechten Eingabe. Wählen Sie nach Möglichkeit nicht das Dataset aus, das Sie zum Trainieren des Modells verwendet haben, sondern ein anderes. Dieses Dataset wird für Bewertungen basierend auf dem trainierten Modell verwendet. Es wird auch zum Auswerten des Modells nach einer Änderung der Featurewerte verwendet.  

4.  Geben Sie für **Zufälliger Ausgangswert** einen Wert ein, der als Ausgangswert für die zufällige Anordnung verwendet werden soll. Wenn Sie den Wert 0 (den Standardwert) angeben, wird basierend auf der Systemzeit eine Zahl generiert.

     Der Ausgangswert ist optional, aber Sie sollten einen Wert angeben, wenn Sie eine Reproduzierbarkeit für die Ausführungen derselben Pipeline erzielen möchten.  

5.  Wählen Sie für **Metrik zur Leistungsmessung** eine einzelne Metrik aus, die zum Berechnen der Modellqualität nach Abschluss der Permutation verwendet werden soll.  

     Der Azure Machine Learning-Designer unterstützt abhängig davon, ob Sie ein Klassifizierungs- oder ein Regressionsmodell auswerten, die folgenden Metriken:  

    -   **Klassifizierung**

        Accuracy, Precision, Recall, Average Log Loss  

    -   **Regression**

        Precision, Recall, Mean Absolute Error, Root Mean Squared Error, Relative Absolute Error, Relative Squared Error, Coefficient of Determination  

     Eine ausführlichere Beschreibung dieser Metriken für die Auswertung und deren Berechnung finden Sie im Artikel zur [Modellauswertung](evaluate-model.md).  

6.  Ausführen der Pipeline.  

7.  Das Modul gibt eine Liste mit Featurespalten und den zugeordneten Bewertungen aus. Die Liste ist in absteigender Reihenfolge nach den Ergebnissen sortiert.  


##  <a name="technical-notes"></a>Technische Hinweise

Das PFI-Modul führt eine zufällige Änderung der Werte für jede Featurespalte durch (jeweils eine pro Vorgang). Anschließend wird das Modell ausgewertet. 

Die vom Modul bereitgestellten Rangfolgen unterscheiden sich häufig von denen, die Sie über [Filter Based Feature Selection](filter-based-feature-selection.md) erhalten. Bei „Filter Based Feature Selection“ werden die Bewertungen *vor* der Erstellung eines Modells berechnet. 

Der Grund für den Unterschied ist, dass bei „Permutation Feature Importance“ die Zuordnung zwischen einem Feature und einem Zielwert nicht gemessen wird. Stattdessen wird erfasst, wie viel Einfluss die einzelnen Features auf Vorhersagen des Modells haben.
  
## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die [Gruppe der verfügbaren Module](module-reference.md) für den Azure Machine Learning Service an. 
