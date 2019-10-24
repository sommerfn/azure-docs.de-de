---
title: Erstellen automatisierter ML-Experimente
titleSuffix: Azure Machine Learning
description: Das automatisierte maschinelle Lernen wählt einen Algorithmus für Sie aus und generiert ein Modell, das bereitgestellt werden kann. Lernen Sie die Optionen kennen, mit denen Sie Experimente mit automatisiertem maschinellem Lernen konfigurieren können.
author: nacharya1
ms.author: nilesha
ms.reviewer: sgilley
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 07/10/2019
ms.custom: seodec18
ms.openlocfilehash: 04753ca4c9b14d7ccc265cfcf971b3fd63c861ae
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72384156"
---
# <a name="configure-automated-ml-experiments-in-python"></a>Konfigurieren automatisierter ML-Experimente in Python

In diesem Leitfaden erfahren Sie, wie Sie verschiedene Konfigurationseinstellungen der automatisierten Machine Learning-Experimente mit dem [Azure Machine Learning-SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) definieren. Das automatisierte Machine Learning wählt einen Algorithmus und Hyperparameter für Sie aus und generiert ein Modell, das bereitgestellt werden kann. Es gibt verschiedene Optionen, mit denen Sie automatisierte Machine Learning-Experimente konfigurieren können.

Beispiele automatisierter Machine Learning-Experimente finden Sie in den Tutorials zum [ Trainieren eines Klassifizierungsmodells mit automatisiertem maschinellem Lernen](tutorial-auto-train-models.md) und zum [Trainieren von Modellen mit automatisiertem maschinellem Lernen in der Cloud](how-to-auto-train-remote.md).

Für das automatisierte maschinelle Lernen sind folgende Konfigurationsoptionen verfügbar:

* Wählen Sie die Experimentart aus: Klassifizierung, Regression oder Zeitreihenvorhersage
* Datenquelle, Datenformate und Abrufen von Daten
* Wählen Sie das Computeziel aus: lokal oder remote.
* Einstellungen für automatisierte Machine Learning-Experimente
* Ausführen eines automatisierten Machine Learning-Experiments
* Untersuchen von Modellmetriken
* Registrieren und Bereitstellen von Modellen

Wenn Sie lieber ohne Code arbeiten, informieren Sie sich unter [Erstellen, Untersuchen und Bereitstellen automatisierter Experimente mit maschinellem Lernen im Azure-Portal](how-to-create-portal-experiments.md).

## <a name="select-your-experiment-type"></a>Auswählen der Experimentart

Legen Sie vor Experimentbeginn fest, welche Art von Problem des maschinellen Lernens Sie lösen möchten. Das automatisierte Machine Learning unterstützt die Aufgabentypen Klassifizierung, Regression und Vorhersage.

Das automatisierte Machine Learning unterstützt während des Automatisierungs- und Optimierungsprozesses die folgenden Algorithmen. Als Benutzer müssen Sie den Algorithmus nicht angeben.

Classification | Regression | Zeitreihe und Vorhersage
|-- |-- |--
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
[Naive Bayes](https://scikit-learn.org/stable/modules/naive_bayes.html#bernoulli-naive-bayes)|
[Stochastisches Gradientenabstiegsverfahren (SGD)](https://scikit-learn.org/stable/modules/sgd.html#sgd)|

Verwenden Sie den `task`-Parameter im `AutoMLConfig`-Konstruktor, um Ihren Experiment-Typ anzugeben.

```python
from azureml.train.automl import AutoMLConfig

# task can be one of classification, regression, forecasting
automl_config = AutoMLConfig(task = "classification")
```

## <a name="data-source-and-format"></a>Datenquelle und Datenformat

Das automatisierte Machine Learning unterstützt Daten, die sich auf dem lokalen Desktop oder in der Cloud (z. B. in Azure Blob Storage) befinden. Die Daten können in einen Pandas DataFrame oder ein Azure Machine Learning-Dataset eingelesen werden. In den folgenden Codebeispielen wird veranschaulicht, wie Daten in diesen Formaten gespeichert werden. [Weitere Informationen zu Datasets](https://github.com/MicrosoftDocs/azure-docs-pr/pull/how-to-create-register-datasets.md)

* TabularDataset
* Pandas-Datenrahmen

>[!Important]
> Anforderungen für Trainingsdaten:
>* Die Daten müssen in Tabellenform vorliegen.
>* Der Wert, den Sie vorhersagen möchten (Zielspalte), muss in den Daten vorhanden sein.

Beispiele:

* TabularDataset
```python
    from azureml.core.dataset import Dataset

    tabular_dataset = Dataset.Tabular.from_delimited_files("https://automldemods.blob.core.windows.net/datasets/PlayaEvents2016,_1.6MB,_3.4k-rows.cleaned.2.tsv")
    train_dataset, test_dataset = tabular_dataset.random_split(percentage = 0.1, seed = 42)
    label = "Label"
```

*   Pandas-Datenrahmen

    ```python
    import pandas as pd
    from sklearn.model_selection import train_test_split

    df = pd.read_csv("https://automldemods.blob.core.windows.net/datasets/PlayaEvents2016,_1.6MB,_3.4k-rows.cleaned.2.tsv", delimiter="\t", quotechar='"')
    train_data, test_data = train_test_split(df, test_size = 0.1, random_state = 42)
    label = "Label"
    ```

## <a name="fetch-data-for-running-experiment-on-remote-compute"></a>Abrufen von Daten zum Ausführen von Experimenten auf einem Remotecomputeziel

Bei Remoteausführungen muss auf die Trainingsdaten vom Remotecomputeziel aus zugegriffen werden können. Die Klasse [`Datasets`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py) im SDK stellt Funktionen für Folgendes bereit:

* Einfaches übertragen von Daten aus statischen Dateien oder URL-Quellen in Ihren Arbeitsbereich
* Bereitstellen Ihrer Daten für Trainingsskripts, wenn die Ausführung mit Cloudcomputeressourcen erfolgt

Ein Beispiel für die Verwendung der `Dataset`-Klasse zum Einbinden von Daten in das Computeziel finden Sie in dieser [Vorgehensweise](how-to-train-with-datasets.md#option-2--mount-files-to-a-remote-compute-target).

## <a name="train-and-validation-data"></a>Trainings- und Überprüfungsdaten

Sie können separate Datensätze für Training und Überprüfung direkt im `AutoMLConfig`-Konstruktor angeben.

### <a name="k-folds-cross-validation"></a>K-fache Kreuzvalidierung

Verwenden Sie die Einstellung `n_cross_validations`, um die Anzahl von Kreuzvalidierungen festzulegen. Das Trainingsdataset wird nach dem Zufallsprinzip in gleich große `n_cross_validations`-Teilmengen aufgeteilt. Bei jedem Kreuzvalidierungsdurchlauf wird eine Teilmenge zum Überprüfen des Modells verwendet – der Rest dient zu Trainingszwecken. Dieser Vorgang wird für die `n_cross_validations`-Durchläufe wiederholt, bis jede Teilmenge einmal als Validierungsset gedient hat. Die Durchschnittswerte für alle `n_cross_validations`-Durchläufe werden ermittelt, und das entsprechende Modell wird anhand des gesamten Trainingsdatasets erneut trainiert.

### <a name="monte-carlo-cross-validation-repeated-random-sub-sampling"></a>Monte Carlo-Kreuzvalidierung (wiederholte Zufallsunterauswahl)

Verwenden Sie `validation_size`, um den Prozentsatz des Trainingsdatasets anzugeben, der zum Überprüfen verwendet werden soll, und geben Sie mit `n_cross_validations` die Anzahl von Kreuzvalidierungen an. Bei jedem Kreuzvalidierungs-Durchlauf wird eine Teilmenge der Größe `validation_size` zufällig ausgewählt, um das Modell zu testen. Die restlichen Daten dienen zu Trainingszwecken. Schließlich werden die Durchschnittswerte für alle `n_cross_validations`-Durchläufe ermittelt, und das entsprechende Modell wird anhand des gesamten Trainingsdatasets erneut trainiert. Monte Carlo wird für die Vorhersagen von Zeitreihen nicht unterstützt.

### <a name="custom-validation-dataset"></a>Benutzerdefiniertes Validierungsdataset

Verwenden Sie benutzerdefinierte Validierungsdatasets, wenn eine zufällige Aufteilung nicht akzeptabel ist (i.d.R. bei Zeitreihendaten oder unausgeglichenen Daten). Sie können einen eigenen Validierungsdataset angeben. Das Modell wird anhand des angegebenen Validierungsdatasets bewertet – anstelle des zufälligen Datasets.

## <a name="compute-to-run-experiment"></a>Computeziel zum Ausführen des Experiments

Legen Sie als Nächstes die Instanz fest, auf der das Modell trainiert werden soll. Ein automatisiertes Machine Learning-Trainingsexperiment kann unter folgenden Computeoptionen ausgeführt werden:
*   Lokaler Computer (z.B. lokaler Desktop oder Laptop): Diese Option wird i.d.R. für kleine Datasets und während der Untersuchungsphase verwendet.
*   Ein Remotecomputer in der Cloud: [Azure Machine Learning Managed Compute](concept-compute-target.md#amlcompute) ist ein verwalteter Dienst, mit dem Machine Learning-Modelle in Clustern virtueller Azure-Computer trainiert werden können.

Beispiel-Notebooks mit lokalen und Remotecomputezielen enthält die [GitHub-Website](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning).

*   Ein Azure Databricks-Cluster im Azure-Abonnement. Weitere Informationen finden Sie hier: [Azure Databricks](how-to-configure-environment.md#azure-databricks).

Beispielnotebooks mit Azure Databricks finden Sie auf der [GitHub-Website](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/azure-databricks/automl).

<a name='configure-experiment'></a>

## <a name="configure-your-experiment-settings"></a>Konfigurieren der Experimenteinstellungen

Es gibt verschiedene Optionen für das Konfigurieren Ihrer automatisierten Machine Learning-Experimente. Diese Parameter werden beim Instanziieren eines `AutoMLConfig`-Objekts festgelegt. Unter [AutoMLConfig-Klasse](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlconfig?view=azure-ml-py) finden Sie eine vollständige Liste der Parameter.

Beispiele hierfür sind:

1.  Klassifizierungsexperimente mit dem primären Metrikwert „AUC_weighted“ und einer maximalen Zeit von 12.000 Sekunden pro Iteration, die nach 50 Iterationen und 2 Kreuzvalidierungen enden.

    ```python
    automl_classifier=AutoMLConfig(
        task='classification',
        primary_metric='AUC_weighted',
        max_time_sec=12000,
        iterations=50,
        blacklist_models='XGBoostClassifier',
        training_data=train_data,
        label_column_name=label,
        n_cross_validations=2)
    ```
2.  Hier sehen Sie ein Beispiel für ein Regressionsexperiment, das nach 100 Iterationen endet, wobei jede Iteration bis zu 600 Sekunden dauert und 5 Kreuzvalidierungen auftreten.

    ```python
    automl_regressor = AutoMLConfig(
        task='regression',
        max_time_sec=600,
        iterations=100,
        whitelist_models='kNN regressor'
        primary_metric='r2_score',
        training_data=train_data,
        label_column_name=label,
        n_cross_validations=5)
    ```

Die drei unterschiedlichen `task`-Parameterwerte (der dritte Tasktyp ist `forecasting` und verwendet denselben Algorithmuspool wie die `regression`-Tasks) bestimmen die Liste der anzuwendenden Modelle. Verwenden Sie die Parameter `whitelist` oder `blacklist`, um Iterationen mit den verfügbaren Modellen, die eingeschlossen oder ausgeschlossen werden sollen, weiter zu ändern. Die Liste der unterstützten Modelle finden Sie unter der [SupportedModels-Klasse](https://docs.microsoft.com/en-us/python/api/azureml-train-automl/azureml.train.automl.constants.supportedmodels?view=azure-ml-py).

### <a name="primary-metric"></a>Primäre Metrik
Die primäre Metrik bestimmt die Metrik, die während des Modelltrainings für die Optimierung verwendet werden soll. Die verfügbaren Metriken, die Sie auswählen können, werden vom ausgewählten Tasktyp bestimmt. In der folgenden Tabelle werden gültige primäre Metriken für jeden Tasktyp aufgeführt.

|Classification | Regression | Zeitreihe und Vorhersage
|-- |-- |--
|accuracy| spearman_correlation | spearman_correlation
|AUC_weighted | normalized_root_mean_squared_error | normalized_root_mean_squared_error
|average_precision_score_weighted | r2_score | r2_score
|norm_macro_recall | normalized_mean_absolute_error | normalized_mean_absolute_error
|precision_score_weighted |

Informationen zu den speziellen Definitionen dieser Metriken finden Sie unter [Grundlagen von Ergebnissen des automatisierten maschinellen Lernens](how-to-understand-automated-ml.md).

### <a name="data-preprocessing--featurization"></a>Vorverarbeiten von Daten und Featurebereitstellung

In jedem automatisierten Machine Learning-Experiment werden Ihre Daten [automatisch skaliert und normalisiert](concept-automated-ml.md#preprocess), um *bestimmte* Algorithmen zu unterstützen, die auf Funktionen mit unterschiedlichen Skalierungen sensibel reagieren.  Sie können jedoch zusätzliche Vorverarbeitung/Featurebereitstellung wie z.B. Imputation fehlender Werte, Codierung und Transformationen aktivieren. [Weitere Informationen zur enthaltenen Featurebereitstellung](how-to-create-portal-experiments.md#preprocess).

Legen Sie zum Aktivieren dieser Featurebereitstellung `"preprocess": True` für die [`AutoMLConfig`-Klasse](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlconfig?view=azure-ml-py) fest.

> [!NOTE]
> Die Schritte zur Vorverarbeitung bei automatisiertem maschinellen Lernen (Featurenormalisierung, Behandlung fehlender Daten, Umwandlung von Text in numerische Daten usw.) werden Teil des zugrunde liegenden Modells. Bei Verwendung des Modells für Vorhersagen werden die während des Trainings angewandten Vorverarbeitungsschritte automatisch auf Ihre Eingabedaten angewandt.

### <a name="time-series-forecasting"></a>Zeitreihe und Vorhersage
Der `forecasting`-Zeitreihentask erfordert zusätzliche Parameter im Konfigurationsobjekt:

1. `time_column_name`: Dies ist ein erforderlicher Parameter, der den Namen der Spalte in Ihren Trainingsdaten mit einer gültigen Zeitreihe definiert.
1. `max_horizon`: Definiert die Zeitspanne, die Sie basierend auf der Periodizität der Trainingsdaten vorhersagen möchten. Wenn z. B. Trainingsdaten mit täglichen Aggregationsintervallen vorliegen, legen Sie fest, für wie viele Tage das Modell im voraus trainieren soll.
1. `grain_column_names`: Definiert den Namen von Spalten, die in Ihren Trainingsdaten einzelne Zeitreihendaten enthalten. Wenn Sie z. B. den Umsatz einer bestimmten Marke nach Filialen vorhersagen, würden Sie Filial- und Markenspalten als Aggregationsspalten definieren. Für jede Körnung/Gruppierung werden separate Zeitreihen und Prognosen erstellt. 

Beispiele für die unten verwendeten Einstellungen finden Sie im [Beispiel-Notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-orange-juice-sales/auto-ml-forecasting-orange-juice-sales.ipynb).

```python
# Setting Store and Brand as grains for training.
grain_column_names = ['Store', 'Brand']
nseries = data.groupby(grain_column_names).ngroups

# View the number of time series data with defined grains
print('Data contains {0} individual time-series.'.format(nseries))
```

```python
time_series_settings = {
    'time_column_name': time_column_name,
    'grain_column_names': grain_column_names,
    'drop_column_names': ['logQuantity'],
    'max_horizon': n_test_periods
}

automl_config = AutoMLConfig(task = 'forecasting',
                             debug_log='automl_oj_sales_errors.log',
                             primary_metric='normalized_root_mean_squared_error',
                             iterations=10,
                             training_data=train_data,
                             label_column_name=label,
                             n_cross_validations=5,
                             path=project_folder,
                             verbosity=logging.INFO,
                             **time_series_settings)
```

### <a name="ensemble"></a> Ensemble-Konfiguration

Ensemble-Modelle sind standardmäßig aktiviert und treten als abschließende Ausführungsiterationen in einem automatisierten Machine Learning-Lauf auf. Aktuell unterstützte Ensemble-Methoden sind „voting“ (Abstimmen) und „stacking“ (Stapeln). Voting wird als soft-voting mittels gewichteter Durchschnittswerte implementiert, und die stacking-Implementierung verwendet eine 2-Ebenen-Implementierung, bei der die erste Ebene dieselben Modelle wie das voting-Ensemble besitzt, und das Modell der zweiten Ebene verwendet wird, um die optimale Kombination der Modelle aus der ersten Ebene zu ermitteln. Wenn Sie ONNX-Modelle verwenden **oder** die Modellerklärung aktiviert haben, wird stacking deaktiviert und nur voting wird verwendet.

Es gibt mehrere Standardargumente, die als `kwargs` in einem `AutoMLConfig`-Objekt bereitgestellt werden können, um das Standardverhalten des stacking-Ensembles zu verändern.

* `stack_meta_learner_type`: Der meta-learner ist ein mit der Ausgabe der einzelnen heterogenen Modelle trainiertes Modell. Standard-meta-learner sind `LogisticRegression` für Klassifizierungsaufgaben (oder `LogisticRegressionCV`, wenn die Kreuzvalidierung aktiviert ist) und `ElasticNet` für Regressions-/Vorhersage-Aufgaben (oder `ElasticNetCV`, wenn die Kreuzvalidierung aktiviert ist). Dieser Parameter kann ein der folgenden Zeichenfolgen sein: `LogisticRegression`, `LogisticRegressionCV`, `LightGBMClassifier`, `ElasticNet`, `ElasticNetCV`, `LightGBMRegressor` oder `LinearRegression`.
* `stack_meta_learner_train_percentage`: gibt den Teil des Trainingssatzes an (bei Auswahl des Trainings- und Validierungstyps des Trainings), der für das Training des meta-learners reserviert werden soll. Der Standardwert ist `0.2`.
* `stack_meta_learner_kwargs`: optionale Parameter, die an den Initialisierer des meta-learners übergeben werden sollen. Diese Parameter und Parametertypen spiegeln die Werte des entsprechenden Modellkonstruktors wider und werden an den Modellkonstruktor weitergeleitet.

Der folgende Code zeigt ein Beispiel für das Angeben des benutzerdefinierten Ensemble-Verhaltens in einem `AutoMLConfig`-Objekt.

```python
ensemble_settings = {
    "stack_meta_learner_type": "LogisticRegressionCV",
    "stack_meta_learner_train_percentage": 0.3,
    "stack_meta_learner_kwargs": {
        "refit": True,
        "fit_intercept": False,
        "class_weight": "balanced",
        "multi_class": "auto",
        "n_jobs": -1
    }
}

automl_classifier = AutoMLConfig(
        task='classification',
        primary_metric='AUC_weighted',
        iterations=20,
        training_data=train_data,
        label_column_name=label,
        n_cross_validations=5,
        **ensemble_settings
        )
```

Das Ensemble-Training ist standardmäßig aktiviert, kann jedoch mit den booleschen Parametern `enable_voting_ensemble` und `enable_stack_ensemble` deaktiviert werden.

```python
automl_classifier = AutoMLConfig(
        task='classification',
        primary_metric='AUC_weighted',
        iterations=20,
        training_data=data_train,
        label_column_name=label,
        n_cross_validations=5,
        enable_voting_ensemble=False,
        enable_stack_ensemble=False
        )
```

## <a name="run-experiment"></a>Ausführen des Experiments

Für automatisierte ML-Experimente erstellen Sie ein `Experiment`-Objekt, wobei es sich um ein benanntes Objekt in einem `Workspace` handelt, das zur Ausführung von Experimenten verwendet wird.

```python
from azureml.core.experiment import Experiment

ws = Workspace.from_config()

# Choose a name for the experiment and specify the project folder.
experiment_name = 'automl-classification'
project_folder = './sample_projects/automl-classification'

experiment = Experiment(ws, experiment_name)
```

Übermitteln Sie das auszuführende Experiment, um ein Modell zu generieren. Übergeben Sie die Methode `AutoMLConfig` an die `submit`-Methode, um das Modell zu generieren.

```python
run = experiment.submit(automl_config, show_output=True)
```

>[!NOTE]
>Abhängigkeiten werden zunächst auf einem neuen Computer installiert.  Es dauert bis zu 10 Minuten, bevor die Ausgabe angezeigt wird.
>Wenn Sie `show_output` auf `True` festlegen, wird die Ausgabe auf der Konsole angezeigt.

### <a name="exit-criteria"></a>Beendigungskriterium
Es gibt einige Optionen, die Sie definieren können, um Ihr Experiment zu beenden.
1. Keine Kriterien: Wenn Sie keine Beendigungsparameter definieren, wird das Experiment fortgesetzt, bis kein weiterer Fortschritt bei Ihrer primären Metrik erzielt wird.
1. Anzahl der Iterationen: Sie definieren die Anzahl der Iterationen, die das Experiment ausführen soll. Sie können optional `iteration_timeout_minutes` hinzufügen, um ein Zeitlimit in Minuten pro Iteration zu definieren.
1. Nach einer Zeitspanne beenden: Wenn Sie `experiment_timeout_minutes` in Ihren Einstellungen verwenden, können Sie definieren, wie lange ein Experiment in Minuten ausgeführt werden soll.
1. Beenden, nachdem ein Score erreicht wurde: Wenn Sie `experiment_exit_score` verwenden, wird das Experiment abgeschlossen, nachdem ein primärer Metrikscore erreicht wurde.

### <a name="explore-model-metrics"></a>Untersuchen von Modellmetriken

Sie können Ihre Trainingsergebnisse in einem Widget oder in der Inlineansicht anzeigen, wenn Sie ein Notebook verwenden. Weitere Details finden Sie unter [Verfolgen und Auswerten von Modellen](how-to-track-experiments.md#view-run-details).

## <a name="understand-automated-ml-models"></a>Verstehen von automatisierten ML-Modellen

Jedes unter Verwendung des automatisierten ML produzierte Modell umfasst die folgenden Schritte:
+ Automatisierte Featureentwicklung (wenn „preprocess=True“)
+ Skalierung/Normalisierung und Algorithmus mit Hyperparameterwerten

Wir machen das Abrufen dieser Informationen über die fitted_model-Ausgabe aus automatisiertem ML transparent.

```python
automl_config = AutoMLConfig(…)
automl_run = experiment.submit(automl_config …)
best_run, fitted_model = automl_run.get_output()
```

### <a name="automated-feature-engineering"></a>Automatisierte Featureentwicklung

Beachten Sie die Liste der Vorverarbeitung und [automatisierten Featureentwicklung](concept-automated-ml.md#preprocess) bei „preprocess=True“.

Betrachten Sie dieses Beispiel:
+ Es gibt 4 Eingabefeatures: A (numerisch), B (numerisch), C (numerisch), D (DateTime)
+ Das numerische Feature C wird gelöscht, da es sich um eine ID-Spalte mit ausschließlich eindeutigen Werten handelt.
+ Die Werte der numerischen Features A und B fehlen und werden daher als Mittelwerte zugerechnet.
+ DateTime-Feature D wird in 11 verschiedenen entwickelten Features bereitgestellt.

Verwenden Sie diese 2 APIs im ersten Schritt des angepassten Modells, um mehr zu verstehen.  Siehe [dieses Beispielnotebook](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand).

+ API 1: `get_engineered_feature_names()` gibt eine Liste von Namen entwickelter Features zurück.

  Syntax:
  ```python
  fitted_model.named_steps['timeseriestransformer']. get_engineered_feature_names ()
  ```

  ```
  Output: ['A', 'B', 'A_WASNULL', 'B_WASNULL', 'year', 'half', 'quarter', 'month', 'day', 'hour', 'am_pm', 'hour12', 'wday', 'qday', 'week']
  ```

  Diese Liste enthält alle Namen entwickelter Features.

  >[!Note]
  >Verwenden Sie „timeseriestransformer“ für „task=’forecasting‘“, verwenden Sie ansonsten „datatransformer“ für die Aufgabe „regression“ oder „classification“.

+ API 2: `get_featurization_summary()` gibt die Zusammenfassung der Featurebereitstellung für alle Eingabefeatures zurück.

  Syntax:
  ```python
  fitted_model.named_steps['timeseriestransformer'].get_featurization_summary()
  ```

  >[!Note]
  >Verwenden Sie „timeseriestransformer“ für „task=’forecasting‘“, verwenden Sie ansonsten „datatransformer“ für die Aufgabe „regression“ oder „classification“.

  Ausgabe:
  ```
  [{'RawFeatureName': 'A',
    'TypeDetected': 'Numeric',
    'Dropped': 'No',
    'EngineeredFeatureCount': 2,
    'Tranformations': ['MeanImputer', 'ImputationMarker']},
   {'RawFeatureName': 'B',
    'TypeDetected': 'Numeric',
    'Dropped': 'No',
    'EngineeredFeatureCount': 2,
    'Tranformations': ['MeanImputer', 'ImputationMarker']},
   {'RawFeatureName': 'C',
    'TypeDetected': 'Numeric',
    'Dropped': 'Yes',
    'EngineeredFeatureCount': 0,
    'Tranformations': []},
   {'RawFeatureName': 'D',
    'TypeDetected': 'DateTime',
    'Dropped': 'No',
    'EngineeredFeatureCount': 11,
    'Tranformations': ['DateTime','DateTime','DateTime','DateTime','DateTime','DateTime','DateTime','DateTime','DateTime','DateTime','DateTime']}]
  ```

   Hierbei gilt:

   |Output|Definition|
   |----|--------|
   |RawFeatureName|Eingabefeature-/Spaltenname aus dem bereitgestellten Dataset.|
   |TypeDetected|Erkannter Datentyp des Eingabefeatures.|
   |Dropped|Gibt an, ob das Eingabefeature gelöscht oder verwendet wurde.|
   |EngineeringFeatureCount|Anzahl von Features, die über automatisierte Featureentwicklungstransformationen generiert wurden.|
   |Transformationen|Liste der Transformationen, die zum Generieren entwickelter Features auf Eingabefeatures angewendet wurden.|

### <a name="scalingnormalization-and-algorithm-with-hyperparameter-values"></a>Skalierung/Normalisierung und Algorithmus mit Hyperparameterwerten:

Um die Skalierung/Normalisierung- und Algorithmus/Hyperparameter-Werte für eine Pipeline zu verstehen, verwenden Sie „fitted_model.steps“. [Erfahren Sie mehr über Skalierung/Normalisierung des Durchsatzes](concept-automated-ml.md#preprocess). Hier ist eine Beispielausgabe:

```
[('RobustScaler', RobustScaler(copy=True, quantile_range=[10, 90], with_centering=True, with_scaling=True)), ('LogisticRegression', LogisticRegression(C=0.18420699693267145, class_weight='balanced', dual=False, fit_intercept=True, intercept_scaling=1, max_iter=100, multi_class='multinomial', n_jobs=1, penalty='l2', random_state=None, solver='newton-cg', tol=0.0001, verbose=0, warm_start=False))
```

Um weitere Informationen zu erhalten, verwenden Sie diese Hilfsfunktion, die in [diesem Beispielnotebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification/auto-ml-classification.ipynb) gezeigt wird.

```python
from pprint import pprint


def print_model(model, prefix=""):
    for step in model.steps:
        print(prefix + step[0])
        if hasattr(step[1], 'estimators') and hasattr(step[1], 'weights'):
            pprint({'estimators': list(
                e[0] for e in step[1].estimators), 'weights': step[1].weights})
            print()
            for estimator in step[1].estimators:
                print_model(estimator[1], estimator[0] + ' - ')
        else:
            pprint(step[1].get_params())
            print()


print_model(fitted_model)
```

Die folgende Ausgabe ist eine Beispielausgabe für eine Pipeline mit einem bestimmten Algorithmus (LogisticRegression mit RobustScaler in diesem Fall).

```
RobustScaler
{'copy': True,
'quantile_range': [10, 90],
'with_centering': True,
'with_scaling': True}

LogisticRegression
{'C': 0.18420699693267145,
'class_weight': 'balanced',
'dual': False,
'fit_intercept': True,
'intercept_scaling': 1,
'max_iter': 100,
'multi_class': 'multinomial',
'n_jobs': 1,
'penalty': 'l2',
'random_state': None,
'solver': 'newton-cg',
'tol': 0.0001,
'verbose': 0,
'warm_start': False}
```

<a name="explain"></a>

## <a name="explain-the-model-interpretability"></a>Erläutern des Modells (Interpretierbarkeit)

Mit dem automatisierten Machine Learning können Sie die Featurewichtigkeit verstehen.  Während des Trainingsprozesses können Sie die globale Featurewichtigkeit für das Modell abrufen.  Für Klassifizierungsszenarios können Sie auch die Featurewichtigkeit auf Klassenebene abrufen.  Sie müssen ein Validierungsdataset (validierungsdaten) bereitstellen, um die Featurewichtigkeit abzurufen.

Es gibt zwei Möglichkeiten, eine Featurewichtigkeit zu generieren.

*   Nachdem ein Experiment abgeschlossen ist, können Sie die `explain_model`-Methode für eine beliebige Iteration verwenden.

    ```python
    from azureml.train.automl.automlexplainer import explain_model

    shap_values, expected_values, overall_summary, overall_imp, per_class_summary, per_class_imp = \
        explain_model(fitted_model, train_data, test_data)

    #Overall feature importance
    print(overall_imp)
    print(overall_summary)

    #Class-level feature importance
    print(per_class_imp)
    print(per_class_summary)
    ```

*   Um die Featurewichtigkeit für alle Iterationen anzuzeigen, legen Sie das Flag `model_explainability` in AutoMLConfig auf `True` fest.

    ```python
    automl_config = AutoMLConfig(task='classification',
                                 debug_log='automl_errors.log',
                                 primary_metric='AUC_weighted',
                                 max_time_sec=12000,
                                 iterations=10,
                                 verbosity=logging.INFO,
                                 training_data=train_data,
                                 label_column_name=y_train,
                                 validation_data=test_data,
                                 model_explainability=True,
                                 path=project_folder)
    ```

    Danach können Sie die retrieve_model_explanation-Methode verwenden, um die Featurewichtigkeit für eine bestimmte Iteration abzurufen.

    ```python
    from azureml.train.automl.automlexplainer import retrieve_model_explanation

    shap_values, expected_values, overall_summary, overall_imp, per_class_summary, per_class_imp = \
        retrieve_model_explanation(best_run)

    #Overall feature importance
    print(overall_imp)
    print(overall_summary)

    #Class-level feature importance
    print(per_class_imp)
    print(per_class_summary)
    ```

Zeigen Sie die URL an, um die Featurewichtigkeit mit dem run-Objekt anzuzeigen:

```
automl_run.get_portal_url()
```

Sie können das Diagramm für die Featurewichtigkeit in Ihrem Arbeitsbereich im Azure-Portal oder über [die Landing Page Ihres Arbeitsbereichs (Vorschau)](https://ml.azure.com) anzeigen. Das Diagramm wird auch angezeigt, wenn Sie das [Jupyter-Widget](https://docs.microsoft.com/python/api/azureml-widgets/azureml.widgets?view=azure-ml-py) `RunDetails` in einem Notebook verwenden. Weitere Informationen über Diagramme finden Sie unter [Grundlagen von Ergebnissen des automatisierten maschinellen Lernens](how-to-understand-automated-ml.md).

```Python
from azureml.widgets import RunDetails
RunDetails(automl_run).show()
```

![Featurewichtigkeitsdiagramm](./media/how-to-configure-auto-train/feature-importance.png)

Weitere Informationen dazu, wie Modellerklärungen und Featurewichtigkeit in anderen Bereichen des SDK außerhalb des automatisierten Machine Learnings aktiviert werden können, finden Sie im Artikel [Modellinterpretierbarkeit mit Azure Machine Learning Service](machine-learning-interpretability-explainability.md).

## <a name="next-steps"></a>Nächste Schritte

Lernen Sie, [wie und wo Sie Modelle bereitstellen](how-to-deploy-and-where.md) können.

Erfahren Sie mehr über das [Trainieren eines Regressionsmodells mit automatisiertem maschinellem Lernen](tutorial-auto-train-models.md) und das [Trainieren mit automatisiertem maschinellem Lernen auf einer Remoteressource](how-to-auto-train-remote.md).
