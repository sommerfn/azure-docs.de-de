---
title: Modellinterpretierbarkeit beim automatisierten maschinellen Lernen
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie mithilfe des Azure Machine Learning SDK erklären können, warum Ihr Modell für automatisches ML Vorhersagen trifft. Es kann während des Trainings und für Rückschlüsse verwendet werden, um zu verstehen, wie Ihr Modell die Featurerelevanz bestimmt und Vorhersagen trifft.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: mesameki
author: mesameki
ms.reviewer: trbye
ms.date: 10/25/2019
ms.openlocfilehash: 2c9df55eb319dd45281eca4684c79d83dc6ef933
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73511051"
---
# <a name="model-interpretability-for-automated-ml-models"></a>Interpretierbarkeit von Modellen für automatisiertes ML

[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

In diesem Artikel erfahren Sie, wie Sie die Interpretierbarkeitsfunktion für automatisiertes ML mithilfe von Azure Machine Learning Service aktivieren. Automatisiertes ML ermöglicht es Ihnen, die Relevanz von Rohfeatures und entwickelten Features zu verstehen. Zur Verwendung der Modellinterpretierbarkeit legen Sie `model_explainability=True` im `AutoMLConfig`-Objekt fest.  

In diesem Artikel lernen Sie Folgendes:

* Interpretierbarkeit während des Trainings nach dem besten oder einem beliebigen Modell
* Aktivieren von Visualisierungen, die Ihnen bei der Erkennung von Mustern in Daten und Erklärungen helfen
* Interpretierbarkeit beim Ziehen von Rückschlüssen oder bei der Bewertung

## <a name="prerequisites"></a>Voraussetzungen

* Führen Sie `pip install azureml-interpret azureml-contrib-interpret` aus, um die erforderlichen Pakete für die Interpretierbarkeitsfeatures abzurufen.
* In diesem Artikel werden Vorkenntnisse in Bezug auf die Erstellung von Experimenten für automatisiertes ML vorausgesetzt. Informationen zur Verwendung von automatisiertem ML im SDK finden Sie im [Tutorial](tutorial-auto-train-models.md) oder in der [Anleitung](how-to-configure-auto-train.md).
 
## <a name="interpretability-during-training-for-the-best-model"></a>Interpretierbarkeit während des Trainings nach dem besten Modell 

Rufen Sie die Erklärung aus `best_run` ab, darin sind Erklärungen für entwickelte Features und Rohfeatures enthalten. 

### <a name="download-engineered-feature-importance-from-artifact-store"></a>Herunterladen der Relevanz entwickelter Features aus dem Artefaktspeicher

Sie können mithilfe von `ExplanationClient` Erklärungen zu entwickelten Features aus dem Artefaktspeicher von „best_run“ herunterladen. Legen Sie `raw=True` fest, um die Erklärungen für die Rohfeatures abzurufen. 

```python
from azureml.contrib.interpret.explanation.explanation_client import ExplanationClient

client = ExplanationClient.from_run(best_run)
engineered_explanations = client.download_model_explanation(raw=False)
print(engineered_explanations.get_feature_importance_dict())
```

## <a name="interpretability-during-training-for-any-model"></a>Interpretierbarkeit während des Trainings nach einem beliebigen Modell 

In diesem Abschnitt erfahren Sie, wie Modellerklärungen berechnet und die Erklärungen visualisiert werden können. Neben dem Abruf einer vorhandenen Modellerklärung für ein Modell für automatisiertes ML können Sie Ihr Modell auch mit anderen Testdaten erklären. Anhand der folgenden Schritte können Sie die Relevanz von entwickelten Features und Rohfeatures basierend auf Ihren Testdaten berechnen und visualisieren.

### <a name="retrieve-any-other-automl-model-from-training"></a>Abrufen eines anderen AutoML-Modells aus dem Training

```python
automl_run, fitted_model = local_run.get_output(metric='r2_score')
```

### <a name="setup-the-model-explanations"></a>Einrichten der Modellerklärungen

Über „fitted_model“ können die folgenden Elemente generiert werden, die zum Abrufen der Erklärungen für entwickelte Features und Rohfeatures über „automl_setup_model_explanations“ verwendet werden:

* Als Features erkannte Daten aus Trainingsbeispielen/Testbeispielen
* Namenslisten für entwickelte Features und Rohfeatures
* Auffinden der Klassen in Ihrer gekennzeichneten Spalte in Klassifizierungsszenarien

„automl_explainer_setup_obj" enthalt alle oben aufgeführten Strukturen.

```python
from azureml.train.automl.automl_explain_utilities import AutoMLExplainerSetupClass, automl_setup_model_explanations

automl_explainer_setup_obj = automl_setup_model_explanations(fitted_model, X=X_train, 
                                                             X_test=X_test, y=y_train, 
                                                             task='classification')
```
### <a name="initialize-the-mimic-explainer-for-feature-importance"></a>Initialisieren von MimicExplainer für Featurerelevanz

Verwenden Sie zum Erklären der AutoML-Modelle die Klasse `MimicWrapper`. Die MimicWrapper-Klasse kann mit Parametern für das explainer_setup-Objekt, Ihren Arbeitsbereich und ein LightGBM-Modell initialisiert werden, das als Surrogatmodell zum Erklären des Modells für automatisiertes ML fungiert (hier: fitted_model). Die MimicWrapper-Klasse verwendet außerdem das automl_run-Objekt, in das die Erklärungen für Rohfeatures und entwickelte Features hochgeladen werden.

```python
from azureml.interpret.mimic.models.lightgbm_model import LGBMExplainableModel
from azureml.interpret.mimic_wrapper import MimicWrapper

explainer = MimicWrapper(ws, automl_explainer_setup_obj.automl_estimator, LGBMExplainableModel, 
                         init_dataset=automl_explainer_setup_obj.X_transform, run=automl_run,
                         features=automl_explainer_setup_obj.engineered_feature_names, 
                         feature_maps=[automl_explainer_setup_obj.feature_map],
                         classes=automl_explainer_setup_obj.classes)
```

### <a name="use-mimicexplainer-for-computing-and-visualizing-engineered-feature-importance"></a>Verwenden von MimicExplainer zum Berechnen und Visualisieren der Relevanz von entwickelten Features

Die explain()-Methode in MimicWrapper kann mit den transformierten Testbeispielen aufgerufen werden, um die Featurerelevanz für die generierten entwickelten Features abzurufen. Sie können außerdem ExplanationDashboard verwenden, um eine Dashboardvisualisierung der Relevanzwerte für die über Featurizer für automatisiertes ML generierten entwickelten Features anzuzeigen.

```python
from azureml.contrib.interpret.visualize import ExplanationDashboard
engineered_explanations = explainer.explain(['local', 'global'],              
                                            eval_dataset=automl_explainer_setup_obj.X_test_transform)

print(engineered_explanations.get_feature_importance_dict())
ExplanationDashboard(engineered_explanations, automl_explainer_setup_obj.automl_estimator, automl_explainer_setup_obj.X_test_transform)
```
### <a name="use-mimic-explainer-for-computing-and-visualizing-raw-feature-importance"></a>Verwenden von MimicExplainer zum Berechnen und Visualisieren der Relevanz von Rohfeatures

Die explain()-Methode in MimicWrapper kann erneut mit den transformierten Testbeispielen und mit Festlegung von `get_raw` auf TRUE aufgerufen werden, um die Featurerelevanz für die Rohfeatures abzurufen. Darüber hinaus können Sie mit ExplanationDashboard eine Dashboardvisualisierung der Relevanzwerte für die Rohfeatures anzeigen.

```python
from azureml.contrib.interpret.visualize import ExplanationDashboard

raw_explanations = explainer.explain(['local', 'global'], get_raw=True, 
                                     raw_feature_names=automl_explainer_setup_obj.raw_feature_names,
                                     eval_dataset=automl_explainer_setup_obj.X_test_transform)

print(raw_explanations.get_feature_importance_dict())
ExplanationDashboard(raw_explanations, automl_explainer_setup_obj.automl_pipeline, automl_explainer_setup_obj.X_test_raw)
```

### <a name="interpretability-during-inference"></a>Interpretierbarkeit beim Ziehen von Rückschlüssen

In diesem Abschnitt erfahren Sie, wie ein Modell für automatisiertes ML mit dem Erklärmodul operationalisiert wird, der im vorherigen Abschnitt zum Berechnen der Erklärungen verwendet wurde.

### <a name="register-the-model-and-the-scoring-explainer"></a>Registrieren des Modells und des Erklärmoduls für die Bewertung

Verwenden Sie den `TreeScoringExplainer`, um das Erklärmodul für die Bewertung zu erstellen. Mit diesem werden beim Ziehen von Rückschlüssen die Relevanzwerte für die Rohfeatures und die entwickelten Features berechnet. Beachten Sie, dass Sie das Erklärmodul für die Bewertung mit dem zuvor berechneten feature_map-Element initialisieren. Das feature_map-Element wird vom Erklärmodul für die Bewertung verwendet, um die Relevanz für Rohfeatures zurückzugeben.

Im Code unten speichern Sie das Erklärmodul für die Bewertung und registrieren das Modell und das Erklärmodul für die Bewertung mit dem Modellverwaltungsdienst.

```python
from azureml.interpret.scoring.scoring_explainer import TreeScoringExplainer, save

# Initialize the ScoringExplainer
scoring_explainer = TreeScoringExplainer(explainer.explainer, feature_maps=[automl_explainer_setup_obj.feature_map])

# Pickle scoring explainer locally
save(scoring_explainer, exist_ok=True)

# Register trained automl model present in the 'outputs' folder in the artifacts
original_model = automl_run.register_model(model_name='automl_model', 
                                           model_path='outputs/model.pkl')

# Register scoring explainer
automl_run.upload_file('scoring_explainer.pkl', 'scoring_explainer.pkl')
scoring_explainer_model = automl_run.register_model(model_name='scoring_explainer', model_path='scoring_explainer.pkl')
```

### <a name="create-the-conda-dependencies-for-setting-up-the-service"></a>Erstellen der conda-Abhängigkeiten zum Einrichten des Diensts

Als Nächstes erstellen Sie die Umgebungsabhängigkeiten, die im Container für das bereitgestellte Modell benötigt werden.

```python
from azureml.core.conda_dependencies import CondaDependencies 

azureml_pip_packages = [
    'azureml-interpret', 'azureml-train-automl', 'azureml-defaults'
]

myenv = CondaDependencies.create(conda_packages=['scikit-learn', 'pandas', 'numpy', 'py-xgboost<=0.80'],
                                 pip_packages=azureml_pip_packages,
                                 pin_sdk_version=True)

with open("myenv.yml","w") as f:
    f.write(myenv.serialize_to_string())

with open("myenv.yml","r") as f:
    print(f.read())

```

### <a name="deploy-the-service"></a>Bereitstellen des Diensts

Stellen Sie den Dienst unter Verwendung der conda-Datei und der Bewertungsdatei aus den vorherigen Schritten bereit.

```python
from azureml.core.webservice import Webservice
from azureml.core.model import InferenceConfig
from azureml.core.webservice import AciWebservice
from azureml.core.model import Model

aciconfig = AciWebservice.deploy_configuration(cpu_cores=1, 
                                               memory_gb=1, 
                                               tags={"data": "Bank Marketing",  
                                                     "method" : "local_explanation"}, 
                                               description='Get local explanations for Bank marketing test data')

inference_config = InferenceConfig(runtime= "python", 
                                   entry_script="score_local_explain.py",
                                   conda_file="myenv.yml")

# Use configs and models generated above
service = Model.deploy(ws, 'model-scoring', [scoring_explainer_model, original_model], inference_config, aciconfig)
service.wait_for_deployment(show_output=True)
```

### <a name="inference-using-test-data"></a>Rückschlüsse mithilfe von Testdaten

Ziehen Sie Rückschlüsse mithilfe einiger Testdaten, um die vorhergesagten Werte aus dem Modell für automatisiertes ML sowie die Relevanz für entwickelte Features und Rohfeatures für den jeweiligen vorhergesagten Wert anzuzeigen.

```python
if service.state == 'Healthy':
    # Serialize the first row of the test data into json
    X_test_json = X_test[:1].to_json(orient='records')
    print(X_test_json)
    # Call the service to get the predictions and the engineered and raw explanations
    output = service.run(X_test_json)
    # Print the predicted value
    print(output['predictions'])
    # Print the engineered feature importances for the predicted value
    print(output['engineered_local_importance_values'])
    # Print the raw feature importances for the predicted value
    print(output['raw_local_importance_values'])
```

### <a name="visualizations-to-aid-you-in-the-discovery-of-patterns-in-data-and-explanations-at-training-time"></a>Visualisierungen zum Erkennen von Mustern in Daten und Erklärungen während des Trainings

Sie können das Diagramm zur Featurerelevanz auch in Ihrem Arbeitsbereich in [Azure Machine Learning-Studio](https://ml.azure.com) anzeigen. Nach Abschluss Ihrer automatisierten ML-Ausführung müssen Sie auf „Modelldetails anzeigen“ klicken, wodurch Sie zu einer spezifischen Ausführung gelangen. Klicken Sie dann auf die Registerkarte „Erklärungen“, um das Dashboard zur Erklärungsvisualisierung anzuzeigen. 

[![Architektur von Machine Learning Interpretability](./media/machine-learning-interpretability-explainability/automl-explainability.png)](./media/machine-learning-interpretability-explainability/automl-explainability.png#lightbox)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen dazu, wie Modellerklärungen und Featurewichtigkeit in anderen Bereichen des SDK außerhalb des automatisierten Machine Learnings aktiviert werden können, finden Sie im Artikel [Modellinterpretierbarkeit mit Azure Machine Learning Service](how-to-machine-learning-interpretability.md).