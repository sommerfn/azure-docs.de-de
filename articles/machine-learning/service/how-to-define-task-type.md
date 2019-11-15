---
title: Definieren einer Aufgabe für maschinelles Lernen für eine Ausführung von automatisiertem maschinellem Lernen
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie eine Aufgabe für maschinelles Lernen für eine Ausführung von automatisiertem maschinellem Lernen definieren.
services: machine-learning
author: RachelKellam
ms.author: rakellam
ms.reviewer: nibaccam
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.date: 11/04/2019
ms.openlocfilehash: c34a8e999267a77fa4aabbd9dc3a73b2c4f5a0ef
ms.sourcegitcommit: 359930a9387dd3d15d39abd97ad2b8cb69b8c18b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73647209"
---
# <a name="how-to-define-a-machine-learning-task"></a>Definieren einer Aufgabe für maschinelles Lernen 
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

In diesem Artikel sind die unterstützten Aufgaben für maschinelles Lernen aufgeführt und erfahren Sie, wie Sie diese für eine Ausführung eines Experiments für automatisiertes maschinelles Lernen (automatisiertes ML) definieren.


## <a name="What is a machine learning task?"></a> Was ist eine Aufgabe für maschinelles Lernen?

Eine Aufgabe für maschinelles Lernen entspricht dem Typ des Problems, das durch Erstellen eines Vorhersagemodells gelöst werden soll. Automatisiertes ML unterstützt die drei Aufgabentypen Klassifizierung, Regression und Zeitreihenvorhersage.

Aufgabentyp| BESCHREIBUNG| Beispiel
----|----|----
Classification | Aufgabe zum Vorhersagen der Kategorie einer bestimmten Zeile in einem Dataset | Betrugserkennung für eine Kreditkarte. Für die Zielspalte würde ein **Erkennen von Betrug** mit den Kategorien *Wahr* und *Falsch* ausgeführt. In diesem Fall wird jede Zeile der Daten entweder als „Wahr“ oder „Falsch“ klassifiziert.
Regression | Aufgabe zum Vorhersagen einer kontinuierlichen Mengenausgabe. | Automobilkosten entsprechend den Merkmalen des Automobils; die Zielspalte wäre **Preis**.
Vorhersagen |Aufgabe zum Erstellen von fundierten Schätzungen, um die Richtung zukünftiger Trends zu ermitteln| Vorhersage des Energiebedarfs für die nächsten 48 Stunden. Die Zielspalte wäre **Bedarf**, und die vorhergesagten Werte würden verwendet, um Muster im Energiebedarf zu zeigen.

Das automatisierte maschinelle Lernen unterstützt die folgenden Algorithmen beim Automatisierungs- und Optimierungsprozess. Als Benutzer müssen Sie den Algorithmus nicht angeben.

Classification | Regression | Zeitreihe und Vorhersage
-- |-- |--
[Logistische Regression](https://scikit-learn.org/stable/modules/linear_model.html#logistic-regression)| [Elastisches Netz](https://scikit-learn.org/stable/modules/linear_model.html#elastic-net)| [Elastisches Netz](https://scikit-learn.org/stable/modules/linear_model.html#elastic-net)
[Light GBM](https://lightgbm.readthedocs.io/en/latest/index.html)|[Light GBM](https://lightgbm.readthedocs.io/en/latest/index.html)|[Light GBM](https://lightgbm.readthedocs.io/en/latest/index.html)
[Gradient Boosting](https://scikit-learn.org/stable/modules/ensemble.html#classification)|[Gradient Boosting](https://scikit-learn.org/stable/modules/ensemble.html#regression)|[Gradient Boosting](https://scikit-learn.org/stable/modules/ensemble.html#regression)
[Entscheidungsstruktur](https://scikit-learn.org/stable/modules/tree.html#decision-trees)|[Entscheidungsstruktur](https://scikit-learn.org/stable/modules/tree.html#regression)|[Entscheidungsstruktur](https://scikit-learn.org/stable/modules/tree.html#regression)
[K nächste Nachbarn](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)|[K nächste Nachbarn](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)|[K nächste Nachbarn](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)
[Lineare SVC](https://scikit-learn.org/stable/modules/svm.html#classification)|[LARS Lasso](https://scikit-learn.org/stable/modules/linear_model.html#lars-lasso)|[LARS Lasso](https://scikit-learn.org/stable/modules/linear_model.html#lars-lasso)
[C-Support-Vektor-Klassifizierung (SVC)](https://scikit-learn.org/stable/modules/svm.html#classification)|[Stochastisches Gradientenabstiegsverfahren (SGD)](https://scikit-learn.org/stable/modules/sgd.html#regression)|[Stochastisches Gradientenabstiegsverfahren (SGD)](https://scikit-learn.org/stable/modules/sgd.html#regression)
[Random Forest](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)|[Random Forest](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)|[Random Forest](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)
[Extremely Randomized Trees](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)|[Extremely Randomized Trees](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)|[Extremely Randomized Trees](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)
[Xgboost](https://xgboost.readthedocs.io/en/latest/parameter.html)|[Xgboost](https://xgboost.readthedocs.io/en/latest/parameter.html)| [Xgboost](https://xgboost.readthedocs.io/en/latest/parameter.html)
[DNN-Klassifizierer](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNClassifier)|[DNN-Regressor](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNRegressor) | [DNN-Regressor](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNRegressor)|
[Linearer DNN-Klassifizierer](https://www.tensorflow.org/api_docs/python/tf/estimator/LinearClassifier)|[Linearer Regressor](https://www.tensorflow.org/api_docs/python/tf/estimator/LinearRegressor)|[Linearer Regressor](https://www.tensorflow.org/api_docs/python/tf/estimator/LinearRegressor)
[Naive Bayes](https://scikit-learn.org/stable/modules/naive_bayes.html#bernoulli-naive-bayes)||
[Stochastisches Gradientenabstiegsverfahren (SGD)](https://scikit-learn.org/stable/modules/sgd.html#sgd)||


### <a name="set-the-task-type"></a>Festlegen des Aufgabentyps
Sie können den Aufgabentyp für Ihre Experimente für automatisiertes ML mit dem SDK oder dem Azure Machine Learning-Studio festlegen.

Verwenden Sie den `task`-Parameter im `AutoMLConfig`-Konstruktor, um Ihren Experiment-Typ anzugeben.

```python
from azureml.train.automl import AutoMLConfig

# task can be one of classification, regression, forecasting
automl_config = AutoMLConfig(task="classification")
```

Sie können den Aufgabentyp festlegen, wenn Sie im Azure Machine Learning-Studio die Ausführung für Ihr Experiment für automatisiertes ML erstellen. 

[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-enterprise-sku-inline.md)]

![Auswahl des Aufgabentyps](./media/how-to-define-task-type/task-type.png)


## <a name="next-steps"></a>Nächste Schritte

+ Erfahren Sie mehr über [automatisiertes Machine Learning](concept-automated-ml.md) in Azure Machine Learning.
+ Erfahren Sie mehr über [Automatisches Trainieren eines Modells für die Zeitreihenprognose](how-to-auto-train-forecast.md) in Azure Machine Learning.
+ Arbeiten Sie das Tutorial [Automated Machine Learning Classification](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning/model-explanation) durch.
+ Arbeiten Sie das Tutorial [Automated Machine Learning Regression](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning/model-explanation) durch.

