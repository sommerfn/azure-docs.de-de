---
title: Modellinterpretierbarkeit für lokale Ausführungen und Remoteausführungen
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie mithilfe des Azure Machine Learning SDK erklären können, warum Ihr Modell Vorhersagen trifft. Es kann während des Trainings und für Rückschlüsse verwendet werden, um zu verstehen, wie Ihr Modell die Featurerelevanz bestimmt und Vorhersagen trifft.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: mesameki
author: mesameki
ms.reviewer: trbye
ms.date: 10/25/2019
ms.openlocfilehash: a2b71a10606b7cd20f06b2497515b758426833a9
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73511043"
---
# <a name="model-interpretability-for-local-and-remote-runs"></a>Modellinterpretierbarkeit für lokale Ausführungen und Remoteausführungen

[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

In diesem Artikel erfahren Sie, wie Sie mithilfe des Interpretierbarkeitspakets des Python-SDK für Azure Machine Learning erklären können, wie Ihr Modell zu den getroffenen Vorhersagen gelangt ist. Sie lernen Folgendes:

* Interpretieren von Machine Learning-Modellen, die sowohl lokal als auch auf Remotecomputeressourcen trainiert wurden
* Speichern lokaler und globaler Erklärungen im Azure-Ausführungsverlauf
* Anzeigen von Visualisierungen zur Interpretierbarkeit in [Azure Machine Learning-Studio](https://ml.azure.com)
* Bereitstellen eines Erklärmoduls für die Bewertung mit Ihrem Modell

Weitere Informationen zur Modellinterpretierbarkeit finden Sie im [Konzeptartikel](how-to-machine-learning-interpretability.md).

## <a name="local-interpretability"></a>Lokale Interpretierbarkeit

Das folgende Beispiel zeigt, wie Sie das Interpretierbarkeitspaket lokal verwenden, ohne auf Azure-Dienste zurückzugreifen. Führen Sie `pip install azureml-interpret` aus, um das Interpretierbarkeitspaket abzurufen.

1. Trainieren Sie ein Beispielmodell in einem lokalen Jupyter-Notebook.

    ```python
    # load breast cancer dataset, a well-known small dataset that comes with scikit-learn
    from sklearn.datasets import load_breast_cancer
    from sklearn import svm
    from sklearn.model_selection import train_test_split
    breast_cancer_data = load_breast_cancer()
    classes = breast_cancer_data.target_names.tolist()
    
    # split data into train and test
    from sklearn.model_selection import train_test_split
    x_train, x_test, y_train, y_test = train_test_split(breast_cancer_data.data,            
                                                        breast_cancer_data.target,  
                                                        test_size=0.2,
                                                        random_state=0)
    clf = svm.SVC(gamma=0.001, C=100., probability=True)
    model = clf.fit(x_train, y_train)
    ```

2. Rufen Sie das Erklärmodul lokal auf: Um ein Explainer-Objekt zu initialisieren, müssen Sie das Modell und einige Trainingsdaten an den Konstruktor des Explainers übergeben. Sie können optional auch Namen von Features und Ausgabeklassen übergeben (während der Klassifizierung), mit denen Ihre Erklärungen und Visualisierungen informativer gestaltet werden. Es folgen Beispiele für das Instanziieren eines Explainer-Objekts bei lokaler Verwendung von `TabularExplainer`, `MimicExplainer` und `PFIExplainer`. `TabularExplainer` ruft einen der drei darunter liegenden SHAP Explainers (`TreeExplainer`, `DeepExplainer` oder `KernelExplainer`) auf und wählt automatisch den aus, der für Ihren Anwendungsfall am besten geeignet ist. Sie können jedoch jedes der drei zugrunde liegenden Explainer-Objekte direkt aufrufen.

    ```python
    from interpret.ext.blackbox import TabularExplainer

    # "features" and "classes" fields are optional
    explainer = TabularExplainer(model, 
                                 x_train, 
                                 features=breast_cancer_data.feature_names, 
                                 classes=classes)
    ```

    oder

    ```python

    from interpret.ext.blackbox import MimicExplainer
    
    # you can use one of the following four interpretable models as a global surrogate to the black box model
    
    from interpret.ext.glassbox import LGBMExplainableModel
    from interpret.ext.glassbox import LinearExplainableModel
    from interpret.ext.glassbox import SGDExplainableModel
    from interpret.ext.glassbox import DecisionTreeExplainableModel

    # "features" and "classes" fields are optional
    # augment_data is optional and if true, oversamples the initialization examples to improve surrogate model accuracy to fit original model.  Useful for high-dimensional data where the number of rows is less than the number of columns. 
    # max_num_of_augmentations is optional and defines max number of times we can increase the input data size.
    # LGBMExplainableModel can be replaced with LinearExplainableModel, SGDExplainableModel, or DecisionTreeExplainableModel
    explainer = MimicExplainer(model, 
                               x_train, 
                               LGBMExplainableModel, 
                               augment_data=True, 
                               max_num_of_augmentations=10, 
                               features=breast_cancer_data.feature_names, 
                               classes=classes)
    ```
   oder

    ```python
    from interpret.ext.blackbox import PFIExplainer 
    
    # "features" and "classes" fields are optional
    explainer = PFIExplainer(model, 
                             features=breast_cancer_data.feature_names, 
                             classes=classes)
    ```

### <a name="overall-global-feature-importance-values"></a>Relevanzwerte für Features auf Gesamtebene (global)

Rufen Sie die Wichtigkeitswerte globaler Features ab.
    
```python

# you can use the training data or the test data here
global_explanation = explainer.explain_global(x_train)

# if you used the PFIExplainer in the previous step, use the next line of code instead
# global_explanation = explainer.explain_global(x_train, true_labels=y_test)

# sorted feature importance values and feature names
sorted_global_importance_values = global_explanation.get_ranked_global_values()
sorted_global_importance_names = global_explanation.get_ranked_global_names()
dict(zip(sorted_global_importance_names, sorted_global_importance_values))

# alternatively, you can print out a dictionary that holds the top K feature names and values
global_explanation.get_feature_importance_dict()
```

### <a name="instance-level-local-feature-importance-values"></a>Relevanzwerte für Features auf Instanzebene (lokal)

Abrufen der Wichtigkeitswerte lokaler Features: Verwenden Sie die folgenden Funktionsaufrufe, um eine einzelne Instanz oder eine Gruppe von Instanzen zu erklären. Beachten Sie, dass PFIExplainer keine lokalen Erklärungen unterstützt.

```python
# explain the first data point in the test set
local_explanation = explainer.explain_local(x_test[0:5])

# sorted feature importance values and feature names
sorted_local_importance_names = local_explanation.get_ranked_local_names()
sorted_local_importance_values = local_explanation.get_ranked_local_values()
```

## <a name="interpretability-for-remote-runs"></a>Interpretierbarkeit für Remoteausführungen

Dieses Beispiel zeigt die Verwendung der `ExplanationClient`-Klasse zum Aktivieren der Modellinterpretierbarkeit für Remoteausführungen. Das Konzept ähnelt dem aus dem vorherigen Abschnitt, aber Sie verwenden `ExplanationClient` in der Remoteausführung, um den Interpretierbarkeitskontext hochzuladen. Anschließend können Sie den Kontext in einer lokalen Umgebung herunterladen. Verwenden Sie `pip install azureml-contrib-interpret`, um das erforderliche Paket abzurufen.

1. Erstellen Sie ein Trainingsskript in einem lokalen Jupyter-Notebook (Beispiel: „train_explain.py“).

    ```python
    from azureml.contrib.interpret.explanation.explanation_client import ExplanationClient
    from azureml.core.run import Run
    from interpret.ext.blackbox import TabularExplainer

    run = Run.get_context()
    client = ExplanationClient.from_run(run)

    # write code to get and split your data into train and test sets here
    # write code to train your model here 

    # explain predictions on your local machine
    # "features" and "classes" fields are optional
    explainer = TabularExplainer(model, 
                                 x_train, 
                                 features=feature_names, 
                                 classes=classes)

    # explain overall model predictions (global explanation)
    global_explanation = explainer.explain_global(x_test)
    
    # uploading global model explanation data for storage or visualization in webUX
    # the explanation can then be downloaded on any compute
    # multiple explanations can be uploaded
    client.upload_model_explanation(global_explanation, comment='global explanation: all features')
    # or you can only upload the explanation object with the top k feature info
    #client.upload_model_explanation(global_explanation, top_k=2, comment='global explanation: Only top 2 features')
    ```

1. Befolgen Sie die Anweisungen unter [Einrichten von Computezielen für das Modelltraining](how-to-set-up-training-targets.md#amlcompute), um eine Azure Machine Learning-Computeressource als Computeziel einzurichten und die Trainingsausführung zu übermitteln. Sie können auch die [Beispielnotebooks](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/explain-model/azure-integration/remote-explanation) anzeigen.

1. Laden Sie die Erklärung in Ihr lokales Jupyter-Notebook herunter.

    ```python
    from azureml.contrib.interpret.explanation.explanation_client import ExplanationClient
    
    client = ExplanationClient.from_run(run)
    
    # get model explanation data
    explanation = client.download_model_explanation()
    # or only get the top k (e.g., 4) most important features with their importance values
    explanation = client.download_model_explanation(top_k=4)
    
    global_importance_values = explanation.get_ranked_global_values()
    global_importance_names = explanation.get_ranked_global_names()
    print('global importance values: {}'.format(global_importance_values))
    print('global importance names: {}'.format(global_importance_names))
    ```

## <a name="raw-feature-transformations"></a>Rohfeaturetransformationen

Optional können Sie Ihre Featuretransformations-Pipeline an das Erklärmodul (in „train_explain.py“) übergeben, um Erklärungen in Bezug auf die Rohfeatures vor der Transformation (im Gegensatz zu entwickelte Features) zu empfangen. Wenn Sie diesen Schritt überspringen, bietet der Explainer Erklärungen in Bezug auf entwickelte Features.

Das Format der unterstützten Transformationen entspricht dem unter [sklearn-pandas](https://github.com/scikit-learn-contrib/sklearn-pandas) beschriebenen. Grundsätzlich werden alle Transformationen unterstützt, solange sie eine einzelne Spalte verarbeiten und daher eine 1:n-Transformation darstellen. 

Rohfeatures können entweder mithilfe von `sklearn.compose.ColumnTransformer` oder unter Verwendung einer Liste passender Transformationstupel erklärt werden. Im Code unten wird `sklearn.compose.ColumnTransformer` verwendet. 


```python
from sklearn.compose import ColumnTransformer

numeric_transformer = Pipeline(steps=[
    ('imputer', SimpleImputer(strategy='median')),
    ('scaler', StandardScaler())])

categorical_transformer = Pipeline(steps=[
    ('imputer', SimpleImputer(strategy='constant', fill_value='missing')),
    ('onehot', OneHotEncoder(handle_unknown='ignore'))])

preprocessor = ColumnTransformer(
    transformers=[
        ('num', numeric_transformer, numeric_features),
        ('cat', categorical_transformer, categorical_features)])

# append classifier to preprocessing pipeline.
# now we have a full prediction pipeline.
clf = Pipeline(steps=[('preprocessor', preprocessor),
                      ('classifier', LogisticRegression(solver='lbfgs'))])


# clf.steps[-1][1] returns the trained classification model
# pass transformation as an input to create the explanation object
# "features" and "classes" fields are optional
tabular_explainer = TabularExplainer(clf.steps[-1][1],
                                     initialization_examples=x_train,
                                     features=dataset_feature_names,
                                     classes=dataset_classes,
                                     transformations=preprocessor)
```

Verwenden Sie den folgenden Code, falls Sie das Beispiel mit einer Liste passender Transformationstupel ausführen möchten.

```python
from sklearn.pipeline import Pipeline
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import StandardScaler, OneHotEncoder
from sklearn.linear_model import LogisticRegression
from sklearn_pandas import DataFrameMapper

# assume that we have created two arrays, numerical and categorical, which holds the numerical and categorical feature names

numeric_transformations = [([f], Pipeline(steps=[('imputer', SimpleImputer(
    strategy='median')), ('scaler', StandardScaler())])) for f in numerical]

categorical_transformations = [([f], OneHotEncoder(
    handle_unknown='ignore', sparse=False)) for f in categorical]

transformations = numeric_transformations + categorical_transformations

# append model to preprocessing pipeline.
# now we have a full prediction pipeline.
clf = Pipeline(steps=[('preprocessor', DataFrameMapper(transformations)),
                      ('classifier', LogisticRegression(solver='lbfgs'))])

# clf.steps[-1][1] returns the trained classification model
# pass transformation as an input to create the explanation object
# "features" and "classes" fields are optional
tabular_explainer = TabularExplainer(clf.steps[-1][1],
                                     initialization_examples=x_train,
                                     features=dataset_feature_names,
                                     classes=dataset_classes,
                                     transformations=transformations)
```

## <a name="visualizations"></a>Visualisierungen

Nachdem Sie die Erklärungen in Ihr lokales Jupyter-Notebook heruntergeladen haben, können Sie das Visualisierungsdashboard zum Auswerten und Interpretieren Ihres Modells verwenden.

### <a name="global-visualizations"></a>Globale Visualisierungen

Die folgenden Plots bieten eine globale Ansicht des trainierten Modells mit den zugehörigen Vorhersagen und Erläuterungen.

|Plot|BESCHREIBUNG|
|----|-----------|
|Durchsuchen von Daten| Eine Übersicht über das Dataset zusammen mit Vorhersagewerten|
|Globale Relevanz|Zeigt die K wichtigsten globalen Features (K konfigurierbar). Dieses Diagramm ist nützlich, um das globale Verhalten des zugrunde liegenden Modells zu verstehen.|
|Erläuterungsuntersuchung|Veranschaulicht, wie ein Feature für eine Änderung der Vorhersagewerte des Modells (oder der Wahrscheinlichkeit von Vorhersagewerten) sorgt. Darüber hinaus wird gezeigt, wie zwei Features interagieren und die Vorhersagen beeinflussen.|
|Zusammenfassung der Relevanz| Verwendet Relevanzwerte für lokale Features mit Vorzeichen über alle Datenpunkte, um die Verteilung der Auswirkungen jedes Features auf den Vorhersagewert zu beschreiben.|

[![Globales Visualisierungsdashboard](./media/machine-learning-interpretability-explainability/global-charts.png)](./media/machine-learning-interpretability-explainability/global-charts.png#lightbox)

### <a name="local-visualizations"></a>Lokale Visualisierungen

Klicken Sie auf einen beliebigen Datenpunkt in einem der vorstehenden Diagramme, um das Diagramm zur lokalen Featurerelevanz für den angegebenen Datenpunkt zu laden.

|Plot|BESCHREIBUNG|
|----|-----------|
|Lokale Relevanz|Zeigt die K wichtigsten globalen Features (K konfigurierbar). Dieses Diagramm ist nützlich, um das lokale Verhalten des zugrunde liegenden Modells an einem bestimmten Datenpunkt zu verstehen.|
|Perturbation Exploration|Ermöglicht Ihnen die Änderung von Featurewerten des gewählten Datenpunkts und die Verfolgung, wie sich diese Änderungen auf den Vorhersagewert auswirken.|
|Individual Conditional Expectation (ICE)| Ermöglicht Ihnen das Ändern eines Featurewerts von einem Mindestwert in einen Höchstwert, um zu ermitteln, wie sich bei einer Änderung des Features die Vorhersage des Datenpunkts ändert.|

[![Visualisierungsdashboard: Wichtigkeit lokaler Features](./media/machine-learning-interpretability-explainability/local-charts.png)](./media/machine-learning-interpretability-explainability/local-charts.png#lightbox)


[![Visualisierungsdashboard: Störung von Features](./media/machine-learning-interpretability-explainability/perturbation.gif)](./media/machine-learning-interpretability-explainability/perturbation.gif#lightbox)


[![Visualisierungsdashboard: ICE-Plots](./media/machine-learning-interpretability-explainability/ice-plot.png)](./media/machine-learning-interpretability-explainability/ice-plot.png#lightbox)

Beachten Sie Folgendes: Sie müssen die Widget-Erweiterungen des Visualisierungsdashboards aktiviert haben, bevor der Jupyter-Kernel gestartet wird.

* Jupyter Notebooks

    ```shell
    jupyter nbextension install --py --sys-prefix azureml.contrib.interpret.visualize
    jupyter nbextension enable --py --sys-prefix azureml.contrib.interpret.visualize
    ```



* Jupyter-Labs

    ```shell
    jupyter labextension install @jupyter-widgets/jupyterlab-manager
    jupyter labextension install microsoft-mli-widget
    ```

Verwenden Sie den folgenden Code, um das Visualisierungsdashboard zu laden.

```python
from azureml.contrib.interpret.visualize import ExplanationDashboard

ExplanationDashboard(global_explanation, model, x_test)
```

### <a name="visualization-in-azure-machine-learning-studio"></a>Visualisierung in Azure Machine Learning-Studio

Wenn Sie die Schritte im Abschnitt [Remoteinterpretierbarkeit](how-to-machine-learning-interpretability-aml.md#interpretability-for-remote-runs) befolgen, können Sie das Visualisierungsdashboard in [Azure Machine Learning-Studio](https://ml.azure.com) anzeigen. Das in Azure Machine Learning-Studio angezeigte Dashboard ist eine vereinfachte Version des oben beschriebenen Visualisierungsdashboards und unterstützt nur die folgenden zwei Registerkarten.

|Plot|BESCHREIBUNG|
|----|-----------|
|Globale Relevanz|Zeigt die K wichtigsten globalen Features (K konfigurierbar). Dieses Diagramm ist nützlich, um das globale Verhalten des zugrunde liegenden Modells zu verstehen.|
|Zusammenfassung der Relevanz| Verwendet Relevanzwerte für lokale Features mit Vorzeichen über alle Datenpunkte, um die Verteilung der Auswirkungen jedes Features auf den Vorhersagewert zu beschreiben.|

Wenn sowohl globale als auch lokale Erklärungen verfügbar sind, werden beide Registerkarten mit Daten aufgefüllt. Sind nur globale Erklärungen verfügbar, wird die zweite Registerkarte deaktiviert.

Sie haben zwei Möglichkeiten zum Zugriff auf das Visualisierungsdashboard in Azure Machine Learning-Studio:

1. Registerkarte „Experimente“ (Vorschau): Wenn Sie auf die Registerkarte „Experimente“ klicken, wird Ihnen eine Liste mit Experimenten angezeigt, die Sie in Azure Machine Learning Service ausgeführt haben. In dieser Liste können Sie ein bestimmtes Experiment auswählen, um auf eine Seite mit allen Ausführungen für den ausgewählten Experimentnamen umgeleitet zu werden. Wenn Sie auf eine Ausführung und die zugehörige Registerkarte „Erklärungen“ klicken, wird das Visualisierungsdashboard für Erklärungen angezeigt.


[![Visualisierungsdashboard: Wichtigkeit lokaler Features](./media/machine-learning-interpretability-explainability/amlstudio-experiments.png)](./media/machine-learning-interpretability-explainability/amlstudio-experiments.png#lightbox)


2. Registerkarte „Modelle“: Wenn Sie Ihr ursprüngliches Modell mithilfe der Schritte in [Bereitstellen von Modellen mit Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/service/how-to-deploy-and-where) registriert haben, wird Ihr Modell in der Liste auf der Registerkarte „Modelle“ angezeigt. Wenn Sie auf ein Modell und die zugehörige Registerkarte „Erklärungen“ klicken, wird das Visualisierungsdashboard für Erklärungen angezeigt.

## <a name="interpretability-at-inference-time"></a>Interpretierbarkeit beim Ziehen von Rückschlüssen

Das Erklärmodul kann zusammen mit dem ursprünglichen Modell bereitgestellt werden und beim Ziehen von Rückschlüssen die lokalen Erklärungsinformationen bereitstellen. Wir bieten auch einfachere Erklärmodule für die Bewertung, um die Leistung der Interpretierbarkeit beim Ziehen von Rückschlüssen zu verbessern. Der Bereitstellungsvorgang für einen einfacheren Bewertungsexplainer ähnelt der Bereitstellung eines Modells und umfasst die folgenden Schritte:


1. Erstellen eines Erklärungsobjekts (z. B. per TabularExplainer):

   ```python
    from interpret.ext.blackbox import TabularExplainer


   explainer = TabularExplainer(model, 
                                initialization_examples=x_train, 
                                features=dataset_feature_names, 
                                classes=dataset_classes, 
                                transformations=transformations)
   ```

1. Erstellen eines Bewertungsexplainers unter Verwendung des Erläuterungsobjekts:

   ```python
   from azureml.contrib.interpret.scoring.scoring_explainer import KernelScoringExplainer, save

   # create a lightweight explainer at scoring time
   scoring_explainer = KernelScoringExplainer(explainer)

   # pickle scoring explainer
   # pickle scoring explainer locally
   OUTPUT_DIR = 'my_directory'
   save(scoring_explainer, directory=OUTPUT_DIR, exist_ok=True)
   ```

1. Konfigurieren und Registrieren eines Images, das das Bewertungsexplainermodell verwendet

   ```python
   # register explainer model using the path from ScoringExplainer.save - could be done on remote compute
   # scoring_explainer.pkl is the filename on disk, while my_scoring_explainer.pkl will be the filename in cloud storage
   run.upload_file('my_scoring_explainer.pkl', os.path.join(OUTPUT_DIR, 'scoring_explainer.pkl'))
   
   scoring_explainer_model = run.register_model(model_name='my_scoring_explainer', 
                                                model_path='my_scoring_explainer.pkl')
   print(scoring_explainer_model.name, scoring_explainer_model.id, scoring_explainer_model.version, sep = '\t')
   ```

1. [Optional:] Abrufen des Bewertungsexplainers aus der Cloud und Testen der Erläuterungen

   ```python
   from azureml.contrib.interpret.scoring.scoring_explainer import load

   # retrieve the scoring explainer model from cloud"
   scoring_explainer_model = Model(ws, 'my_scoring_explainer')
   scoring_explainer_model_path = scoring_explainer_model.download(target_dir=os.getcwd(), exist_ok=True)

   # load scoring explainer from disk
   scoring_explainer = load(scoring_explainer_model_path)

   # test scoring explainer locally
   preds = scoring_explainer.explain(x_test)
   print(preds)
   ```

1. Bereitstellen des Images für ein Computeziel:

   1. Erstellen einer Bewertungsdatei (führen Sie vor diesem Schritt die Schritte unter [Bereitstellen von Modellen mit Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/service/how-to-deploy-and-where) zum Registrieren Ihres ursprünglichen Vorhersagemodells aus)

        ```python
        %%writefile score.py
        import json
        import numpy as np
        import pandas as pd
        import os
        import pickle
        from sklearn.externals import joblib
        from sklearn.linear_model import LogisticRegression
        from azureml.core.model import Model

        def init():

            global original_model
            global scoring_model

            # retrieve the path to the model file using the model name
            # assume original model is named original_prediction_model
            original_model_path = Model.get_model_path('original_prediction_model')
            scoring_explainer_path = Model.get_model_path('my_scoring_explainer')

            original_model = joblib.load(original_model_path)
            scoring_explainer = joblib.load(scoring_explainer_path)

        def run(raw_data):
            # get predictions and explanations for each data point
            data = pd.read_json(raw_data)
            # make prediction
            predictions = original_model.predict(data)
            # retrieve model explanations
            local_importance_values = scoring_explainer.explain(data)
            # you can return any data type as long as it is JSON-serializable
            return {'predictions': predictions.tolist(), 'local_importance_values': local_importance_values}
        ```

   1. Definieren der Bereitstellungskonfiguration. (Diese Konfiguration hängt von den Anforderungen Ihres Modells ab. Das folgende Beispiel definiert eine Konfiguration, die einen CPU-Kern und 1 GB Arbeitsspeicher verwendet.)

        ```python
        from azureml.core.webservice import AciWebservice

        aciconfig = AciWebservice.deploy_configuration(cpu_cores=1,
                                                       memory_gb=1,
                                                       tags={"data": "NAME_OF_THE_DATASET",
                                                             "method" : "local_explanation"},
                                                       description='Get local explanations for NAME_OF_THE_PROBLEM')
        ```

   1. Erstellen einer Datei mit Umgebungsabhängigkeiten

        ```python
        from azureml.core.conda_dependencies import CondaDependencies

        # WARNING: to install this, g++ needs to be available on the Docker image and is not by default (look at the next cell)

        azureml_pip_packages = ['azureml-defaults', 'azureml-contrib-interpret', 'azureml-core', 'azureml-telemetry', 'azureml-interpret']
 

        # specify CondaDependencies obj
        myenv = CondaDependencies.create(conda_packages=['scikit-learn', 'pandas'],
                                         pip_packages=['sklearn-pandas'] + azureml_pip_packages,
                                         pin_sdk_version=False)


        with open("myenv.yml","w") as f:
            f.write(myenv.serialize_to_string())

        with open("myenv.yml","r") as f:
            print(f.read())
        ```

   1. Erstellen eines benutzerdefinierten Dockerfiles mit installiertem g++

        ```python
        %%writefile dockerfile
        RUN apt-get update && apt-get install -y g++
        ```

   1. Bereitstellen des erstellten Images (geschätzte Zeit: 5 Minuten)

        ```python
        from azureml.core.webservice import Webservice
        from azureml.core.image import ContainerImage

        # use the custom scoring, docker, and conda files we created above
        image_config = ContainerImage.image_configuration(execution_script="score.py",
                                                        docker_file="dockerfile",
                                                        runtime="python",
                                                        conda_file="myenv.yml")

        # use configs and models generated above
        service = Webservice.deploy_from_model(workspace=ws,
                                            name='model-scoring-service',
                                            deployment_config=aciconfig,
                                            models=[scoring_explainer_model, original_model],
                                            image_config=image_config)

        service.wait_for_deployment(show_output=True)
        ```

1. Testen der Bereitstellung

    ```python
    import requests

    # create data to test service with
    examples = x_list[:4]
    input_data = examples.to_json()

    headers = {'Content-Type':'application/json'}

    # send request to service
    resp = requests.post(service.scoring_uri, input_data, headers=headers)

    print("POST to url", service.scoring_uri)
    # can covert back to Python objects from json string if desired
    print("prediction:", resp.text)
    ```

1. Bereinigen: Verwenden Sie zum Löschen eines bereitgestellten Webdiensts `service.delete()`.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Modellinterpretierbarkeit finden Sie im [Konzeptartikel](how-to-machine-learning-interpretability.md).
