---
title: Modelltrainingsmethoden
titleSuffix: Azure Machine Learning
description: Erfahren Sie, mit welchen Methoden Sie das Modell mit Azure Machine Learning trainieren können. Estimators bieten eine einfache Möglichkeit, mit beliebten Frameworks wie Scikit-learn, TensorFlow, Keras, PyTorch und Chainer zu arbeiten. Machine Learning-Pipelines erleichtern die Planung unbeaufsichtigter Ausführungen, die Nutzung heterogener Computeumgebungen und die Wiederverwendung von Teilen Ihres Workflows. Außerdem bieten Laufzeitkonfigurationen eine differenzierte Kontrolle der Computeziele, auf denen der Trainingsprozess ausgeführt wird.
services: machine-learning
ms.service: machine-learning
author: Blackmist
ms.author: larryfr
ms.subservice: core
ms.topic: conceptual
ms.date: 09/18/2019
ms.openlocfilehash: a755fe1607e581cb0a25eb9bd90c2ba223829a46
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71350604"
---
# <a name="train-models-with-azure-machine-learning"></a>Trainieren von Modellen mit Azure Machine Learning

Azure Machine Learning bietet verschiedene Methoden zum Trainieren von Modellen, von Code First-Lösungen mit dem SDK bis zu Low-Code-Lösungen wie automatisiertes ML und die grafische Benutzeroberfläche. Anhand der folgenden Liste können Sie ermitteln, welche Trainingsmethode für Sie die richtige ist:

+ [Azure Machine Learning SDK für Python](#python-sdk): Das Python SDK bietet verschiedene Möglichkeiten, Modelle mit jeweils unterschiedlichen Funktionen zu trainieren.

    | Trainingsmethode | BESCHREIBUNG |
    | ----- | ----- |
    | [Laufzeitkonfiguration](#run-configuration) | Eine **generische Möglichkeit zum Trainieren von Modellen** besteht darin, ein Trainingsskript und eine Laufzeitkonfiguration zu verwenden. Die Laufzeitkonfiguration stellt die zum Konfigurieren der Trainingsumgebung, erforderlichen Informationen bereit, in der Ihr Modell trainiert wird. Sie können eine Laufzeitkonfiguration, Ihr Trainingsskript und ein Computeziel (die Trainingsumgebung) verwenden und einen Trainingsauftrag ausführen. |
    | [Automatisiertes maschinelles Lernen](#automated-machine-learning) | Mithilfe des automatisierten maschinellen Lernens können Sie **Modelle ohne umfassende Data Science- oder Programmierkenntnisse trainieren**. Für Personen mit Data Science- und Programmierungshintergrund bietet es eine Möglichkeit, Zeit und Ressourcen zu sparen, indem die Algorithmusauswahl und die Hyperparameteroptimierung automatisiert werden. Bei der Verwendung des automatisierten maschinellen Lernens müssen Sie sich nicht um die Definition einer Laufzeitkonfiguration kümmern. |
    | [Schätzfunktionen](#estimators) | Mithilfe der Estimator-Klassen können **Modelle auf Grundlage beliebter Frameworks für maschinelles Lernen einfach trainiert werden**. Es gibt Estimator-Klassen für **Scikit-learn**, **PyTorch**, **TensorFlow** und **Chainer**. Es gibt auch einen allgemeinen Estimator, der mit Frameworks verwendet werden kann, die noch keine dedizierte Estimator-Klasse aufweisen. Sie müssen sich nicht um die Definition einer Laufzeitkonfiguration kümmern, wenn Sie Estimators verwenden. |
    | [Machine Learning-Pipeline](#machine-learning-pipeline) | Pipelines sind keine andere Trainingsmethode, sondern eine **Möglichkeit, einen Workflow mit modularen, wiederverwendbaren Schritten zu definieren**, die Training als Teil des Workflows enthalten können. Machine Learning-Pipelines unterstützen die Verwendung des automatisierten maschinellen Lernens sowie die Verwendung von Estimators und der Laufzeitkonfiguration zum Trainieren von Modellen. Da Pipelines nicht speziell auf das Training ausgerichtet sind, variieren die Gründe für den Einsatz einer Pipeline stärker als die anderen Trainingsmethoden. Im Allgemeinen können Sie eine Pipeline in folgenden Situationen verwenden:<br>* Sie möchten **unbeaufsichtigte Prozesse planen**, z. B. zeitintensive Trainingsaufträge oder Datenaufbereitungen.<br>* Sie möchten **mehrere Schritte** verwenden, die über heterogene Computeressourcen und Speicherorte hinweg koordiniert sind.<br>* Sie möchten die Pipeline als **wiederverwendbare Vorlage** für bestimmte Szenarien verwenden, z. B. für erneutes Training oder Batchbewertungen.<br>* **Nachverfolgung und Versionierung von Datenquellen, Eingaben und Ausgaben** für Ihren Workflow.<br>* Ihr Workflow wird von **verschiedenen Teams implementiert, die unabhängig voneinander an bestimmten Schritten arbeiten**. Die Schritte können dann in einer Pipeline zusammengeführt werden, um den Workflow zu implementieren. |

+ **Grafische Benutzeroberfläche**: Die __grafische Benutzeroberfläche__ von Azure Machine Learning bietet einen einfachen Einstiegspunkt in das maschinelle Lernen zum Erstellen von Proof of Concepts oder für Benutzer mit wenig Programmiererfahrung. Sie ermöglicht es Ihnen, Modelle per Drag & Drop über eine webbasierte Benutzeroberfläche zu trainieren. Sie können Python-Code als Teil des Designs verwenden oder Modelle trainieren, ohne Code zu schreiben.

+ **CLI**: Die Befehlszeilenschnittstelle für das maschinelle Lernen stellt Befehle für häufige Aufgaben mit Azure Machine Learning bereit und wird häufig für **Skripting- und Automatisierungsaufgaben** verwendet. Nachdem Sie z. B. ein Trainingsskript oder eine Pipeline erstellt haben, können Sie mit der Befehlszeilenschnittstelle einen Trainingsdurchlauf starten, der sich nach einem Zeitplan oder nach der Aktualisierung der für das Training verwendeten Datendateien richtet. Für Trainingsmodelle werden Befehle bereitgestellt, die Trainingsaufträge übermitteln. Sie kann Aufträge über Laufzeitkonfigurationen oder Pipelines übermitteln.

Jede dieser Trainingsmethoden kann verschiedene Arten von Computeressourcen für das Training verwenden. Zusammenfassend werden diese Ressourcen als [__Computeziele__](concept-azure-machine-learning-architecture.md#compute-targets) bezeichnet. Ein Computeziel kann ein lokaler Computer oder eine Cloudressource sein, wie beispielsweise Azure Machine Learning Compute, Azure HDInsight oder ein virtueller Remotecomputer.

## <a name="python-sdk"></a>Python SDK

Das Azure Machine Learning SDK für Python ermöglicht es Ihnen, Workflows für maschinelles Lernen mit Azure Machine Learning zu erstellen und auszuführen. Sie können mit dem Dienst über eine interaktive Python-Sitzung, Jupyter Notebooks, Visual Studio Code oder eine andere integrierte Entwicklungsumgebung (IDE) interagieren.

* [Was ist das Azure Machine Learning SDK für Python?](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)
* [Installieren/Aktualisieren des SDKs](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py)
* [Konfigurieren einer Entwicklungsumgebung für Azure Machine Learning](how-to-configure-environment.md)

### <a name="run-configuration"></a>Laufzeitkonfiguration

Ein allgemeiner Trainingsauftrag mit Azure Machine Learning kann mit der [Laufzeitkonfiguration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfiguration?view=azure-ml-py) definiert werden. Die Laufzeitkonfiguration wird dann zusammen mit Ihren Trainingsskripts verwendet, um ein Modell auf einem Computeziel zu trainieren.

Sie können mit einer Laufzeitkonfiguration für Ihren lokalen Computer beginnen und dann bei Bedarf zu einer Laufzeitkonfiguration für ein cloudbasiertes Computeziel wechseln. Wenn Sie das Computeziel ändern, wird nur die von Ihnen verwendete Laufzeitkonfiguration geändert. Eine Ausführung protokolliert auch Informationen zum Trainingsauftrag wie Eingaben, Ausgaben und Protokolle.

* [Was ist eine Laufzeitkonfiguration?](concept-azure-machine-learning-architecture.md#run-configurations)
* [Tutorial: Trainieren Ihres ersten ML-Modells](tutorial-1st-experiment-sdk-train.md)
* [Beispiele: Jupyter Notebook-Beispiele für Trainingsmodelle](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training)
* [Gewusst wie: Einrichten und Verwenden von Computezielen für das Modelltraining](how-to-set-up-training-targets.md)

### <a name="automated-machine-learning"></a>Automatisiertes maschinelles Lernen

Definieren Sie Iterationen, Hyperparametereinstellungen, Featurebereitstellungen und andere Einstellungen. Während des Trainings testet Azure Machine Learning verschiedene Algorithmen und Parameter gleichzeitig. Das Training wird beendet, sobald es die von Ihnen definierten Beendigungskriterien erfüllt. Sie müssen sich nicht um die Definition einer Laufzeitkonfiguration kümmern, wenn Sie Estimators verwenden.

> [!TIP]
> Zusätzlich zum Python SDK können Sie das automatisierte maschinelle Lernen auch über die [Landing Page des Arbeitsbereichs (Vorschau)](https://ml.azure.com) verwenden.

* [Was ist automatisiertes maschinelles Lernen?](concept-automated-ml.md)
* [Tutorial: Erstellen Ihres ersten Klassifizierungsmodells mit automatisiertem maschinellen Lernen](tutorial-first-experiment-automated-ml.md)
* [Tutorial: Vorhersagen von Preisen für Taxifahrten mit automatisiertem maschinellen Lernen](tutorial-auto-train-models.md)
* [Beispiele: Jupyter Notebook-Beispiele für automatisiertes maschinelles Lernen](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning)
* [Gewusst wie: Konfigurieren automatisierter ML-Experimente in Python](how-to-configure-auto-train.md)
* [Gewusst wie: Automatisches Trainieren eines Modells für die Zeitreihenprognose](how-to-auto-train-forecast.md)
* [Gewusst wie: Erstellen, Untersuchen und Bereitstellen von automatisierten Machine Learning-Experimenten über die Landing Page des Arbeitsbereichs von Azure Machine Learning (Vorschau)](how-to-create-portal-experiments.md)

### <a name="estimators"></a>Schätzfunktionen

Estimators vereinfachen das Training von Modellen mit beliebten ML-Frameworks. Wenn Sie **Scikit-learn**, **PyTorch**, **TensorFlow** oder **Chainer** verwenden, sollten Sie einen Estimator für das Training verwenden. Es gibt auch einen allgemeinen Estimator, der mit Frameworks verwendet werden kann, die noch keine dedizierte Estimator-Klasse aufweisen. Sie müssen sich nicht um die Definition einer Laufzeitkonfiguration kümmern, wenn Sie Estimators verwenden.

* [Was sind Estimators?](concept-azure-machine-learning-architecture.md#estimators)
* [Tutorial: Trainieren von Bildklassifikationsmodellen mit MNIST-Daten und Scikit-learn mithilfe von Azure Machine Learning](tutorial-train-models-with-aml.md)
* [Beispiele: Jupyter Notebook-Beispiele zur Verwendung von Estimators](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning)
* [Gewusst wie: Erstellen von Estimators im Training](how-to-train-ml-models.md)

### <a name="machine-learning-pipeline"></a>Machine Learning-Pipeline

Machine Learning-Pipelines können die zuvor genannten Trainingsmethoden (Laufzeitkonfiguration, Estimators und automatisiertes maschinelles Lernen) nutzen. Bei Pipelines geht es mehr um die Erstellung eines Workflows, daher umfassen sie mehr als nur das Training von Modellen. In einer Pipeline können Sie ein Modell mit automatisiertem maschinellen Lernen, Estimators oder Laufzeitkonfigurationen trainieren.

* [Was sind ML-Pipelines in Azure Machine Learning?](concept-ml-pipelines.md)
* [Erstellen und Ausführen von Machine Learning-Pipelines mit dem Azure Machine Learning SDK](how-to-create-your-first-pipeline.md)
* [Tutorial: Verwenden von Azure Machine Learning-Pipelines für die Batchbewertung](tutorial-pipeline-batch-scoring-classification.md)
* [Beispiele: Jupyter Notebook-Beispiele für Machine Learning-Pipelines](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/machine-learning-pipelines)
* [Beispiele: Pipeline mit automatisiertem maschinellen Lernen](https://aka.ms/pl-automl)
* [Beispiele: Pipeline mit Estimators](https://aka.ms/pl-estimator)

## <a name="visual-interface"></a>Grafische Benutzeroberfläche

Die grafische Benutzeroberfläche (Vorschauversion) ermöglicht es Ihnen, Modelle per Drag & Drop in Ihrem Webbrowser zu trainieren.

+ [Was ist die grafische Benutzeroberfläche?](ui-concept-visual-interface.md)
+ [Tutorial: Prognostizieren von Fahrzeugpreisen](ui-tutorial-automobile-price-train-score.md)
+ [Regression: Preisprognose](how-to-ui-sample-regression-predict-automobile-price-basic.md)
+ [Klassifizierung: Vorhersagen des Kreditrisikos](how-to-ui-sample-classification-predict-credit-risk-basic.md)
+ [Klassifizierung: Vorhersage von Kundenabwanderung, Kauflust und Up-Selling](how-to-ui-sample-classification-predict-churn.md)

## <a name="cli"></a>Befehlszeilenschnittstelle (CLI)

Die Machine Learning-Befehlszeilenschnittstelle ist eine Erweiterung für die Azure-Befehlszeilenschnittstelle. Es bietet plattformübergreifende CLI-Befehle für die Arbeit mit Azure Machine Learning. In der Regel verwenden Sie die Befehlszeilenschnittstelle zur Automatisierung von Aufgaben, z. B. das Training eines Machine Learning-Modells.

* [Verwenden der CLI-Erweiterung für Azure Machine Learning](reference-azure-machine-learning-cli.md)
* [MLOps unter Azure](https://github.com/microsoft/MLOps)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum [Einrichten von Trainingsumgebungen](how-to-set-up-training-targets.md).
