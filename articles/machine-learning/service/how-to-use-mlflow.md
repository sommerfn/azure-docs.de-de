---
title: MLflow-Tracking für ML-Experimente
titleSuffix: Azure Machine Learning
description: Richten Sie MLflow mit Azure Machine Learning ein, um Metriken und Artefakte zu protokollieren und Modelle aus Databricks, Ihrer lokalen Umgebung oder Ihrer VM-Umgebung bereitzustellen.
services: machine-learning
author: rastala
ms.author: roastala
ms.service: machine-learning
ms.subservice: core
ms.reviewer: nibaccam
ms.topic: conceptual
ms.date: 09/23/2019
ms.custom: seodec18
ms.openlocfilehash: 946350af0c1a4e8140fbf7f926061aae250e9969
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73716483"
---
# <a name="track-metrics-and-deploy-models-with-mlflow-and-azure-machine-learning-preview"></a>Nachverfolgen von Metriken und Bereitstellen von Modellen mit MLflow und Azure Machine Learning (Vorschauversion)
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

In diesem Artikel wird veranschaulicht, wie Sie den Nachverfolgungs-URI und die Protokollierungs-API von MLflow, auch bekannt als [MLflow Tracking](https://mlflow.org/docs/latest/quickstart.html#using-the-tracking-api), mit Azure Machine Learning nutzen. Dies ermöglicht Ihnen Folgendes:

+ Sie können die Metriken und Artefakte von Experimenten im [Azure Machine Learning-Arbeitsbereich](https://docs.microsoft.com/azure/machine-learning/service/concept-azure-machine-learning-architecture#workspaces) nachverfolgen und protokollieren. Falls Sie MLflow-Tracking bereits für Ihre Experimente verwenden, ist der Arbeitsbereich ein zentraler, sicherer und skalierbarer Ort zum Speichern von Trainingsmetriken und -modellen.

+ Stellen Sie Ihre MLflow-Experimente als Azure Machine Learning-Webdienst bereit. Durch die Bereitstellung als Webdienst können Sie die Funktionen von Azure Machine Learning zur Überwachung und Erkennung von Datenabweichungen für Ihre Produktionsmodelle einsetzen. 

[MLflow](https://www.mlflow.org) ist eine Open-Source-Bibliothek zum Verwalten des Lebenszyklus Ihrer Machine Learning-Experimente. MLFlow-Tracking ist eine Komponente von MLflow, die Ihre Trainingsausführungsmetriken und Modellartefakte unabhängig von der Umgebung Ihres Experiments protokolliert und nachverfolgt: auf einem Remotecomputeziel, auf einem virtuellen Computer, lokal auf Ihrem Computer oder in einem Azure Databricks-Cluster.

Im folgenden Diagramm ist dargestellt, wie Sie mit MLflow-Tracking die Ausführungsmetriken eines Experiments nachverfolgen und Modellartefakte in Ihrem Azure Machine Learning-Arbeitsbereich speichern.

![MLflow mit Azure Machine Learning: Diagramm](media/how-to-use-mlflow/mlflow-diagram-track.png)

> [!TIP]
> Die Informationen in diesem Dokument sind hauptsächlich für Datenanalysten und Entwickler gedacht, die den Modelltrainingsprozess überwachen möchten. Wenn Sie Administrator sind und sich für die Überwachung der Nutzung und Ereignisse von Azure Machine Learning (z. B. Kontingente, abgeschlossene Trainingsausführungen oder abgeschlossene Modellimplementierungen) interessieren, helfen Ihnen die Informationen im Artikel [Überwachen von Azure Machine Learning](monitor-azure-machine-learning.md) weiter.

## <a name="compare-mlflow-and-azure-machine-learning-clients"></a>Vergleich von MLflow mit Azure Machine Learning-Clients

 In der Tabelle unten sind die unterschiedlichen Clients, für die Azure Machine Learning genutzt werden kann, und die entsprechenden Funktionen zusammengefasst.

 MLflow-Tracking verfügt über Funktionen für Metrikprotokollierung und Artefaktspeicherung, die sonst nur über das [Azure Machine Learning-SDK für Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) zur Verfügung stehen.


| | MLflow-Tracking und -Bereitstellung | Python-SDK für Azure Machine Learning |  Azure Machine Learning-CLI | Azure Machine Learning Studio|
|---|---|---|---|---|
| Verwalten des Arbeitsbereichs |   | ✓ | ✓ | ✓ |
| Verwenden von Datenspeichern  |   | ✓ | ✓ | |
| Protokollieren von Metriken      | ✓ | ✓ |   | |
| Hochladen von Artefakten | ✓ | ✓ |   | |
| Anzeigen von Metriken     | ✓ | ✓ | ✓ | ✓ |
| Verwalten von Compute   |   | ✓ | ✓ | ✓ |
| Bereitstellen von Modellen    | ✓ | ✓ | ✓ | ✓ |
|Überwachen der Modellleistung||✓|  |   |
| Erkennen von Datenabweichungen |   | ✓ |   | ✓ |

## <a name="prerequisites"></a>Voraussetzungen

* [Installieren Sie MLflow](https://mlflow.org/docs/latest/quickstart.html).
* [Installieren Sie das Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py) auf Ihrem lokalen Computer. Das SDK stellt die Konnektivität für MLflow für den Zugriff auf Ihren Arbeitsbereich bereit.
* [Erstellen Sie einen Azure Machine Learning-Arbeitsbereich.](how-to-manage-workspace.md)

## <a name="track-local-runs"></a>Nachverfolgen von lokalen Ausführungen

Mit MLflow-Tracking mit Azure Machine Learning können Sie die protokollierten Metriken und Artefakte aus Ihren lokalen Ausführungen in Ihrem Azure Machine Learning-Arbeitsbereich speichern.

Installieren Sie das Paket `azureml-contrib-run`, um MLflow-Tracking mit Azure Machine Learning für Ihre Experimente zu nutzen, die lokal in einem Jupyter Notebook oder Code-Editor ausgeführt werden.

```shell
pip install azureml-contrib-run
```

>[!NOTE]
>Der Namespace „azureml.contrib“ ändert sich häufig, während wir an der Verbesserung des Diensts arbeiten. Daher sollte alles in diesem Namespace als Vorschau und als von Microsoft nicht vollständig unterstützt betrachtet werden.

Importieren Sie die Klassen `mlflow` und [`Workspace`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py), um auf den Tracking-URI von MLflow zuzugreifen und Ihren Arbeitsbereich zu konfigurieren.

Im folgenden Code wird mit der `get_mlflow_tracking_uri()`-Methode dem Arbeitsbereich `ws` eine eindeutige Tracking-URI-Adresse zugewiesen, und mit `set_tracking_uri()` wird für den Tracking-URI von MLflow auf diese Adresse verwiesen.

```Python
import mlflow
from azureml.core import Workspace

ws = Workspace.from_config()

mlflow.set_tracking_uri(ws.get_mlflow_tracking_uri())
```

>[!NOTE]
>Der Tracking-URI ist maximal eine Stunde lang gültig. Wenn Sie Ihr Skript nach einer Leerlaufzeit neu starten, sollten Sie die get_mlflow_tracking_uri-API verwenden, um einen neuen URI abzurufen.

Legen Sie den Namen des MLflow-Experiments mit `set_experiment()` fest, und starten Sie Ihre Trainingsausführung mit `start_run()`. Verwenden Sie anschließend `log_metric()`, um die API für die MLflow-Protokollierung zu aktivieren, und beginnen Sie mit dem Protokollieren Ihrer Metriken für die Trainingsausführungen.

```Python
experiment_name = 'experiment_with_mlflow'
mlflow.set_experiment(experiment_name)

with mlflow.start_run():
    mlflow.log_metric('alpha', 0.03)
```

## <a name="track-remote-runs"></a>Nachverfolgen von Remoteausführungen

Mit MLflow-Tracking mit Azure Machine Learning können Sie die protokollierten Metriken und Artefakte aus Ihren Remoteausführungen in Ihrem Azure Machine Learning-Arbeitsbereich speichern.

Bei Remoteausführungen können Sie für das Trainieren Ihrer Modelle leistungsfähigere Computeumgebungen, z. B. GPU-fähige virtuelle Computer, oder Machine Learning Compute-Cluster verwenden. Informationen zu den verschiedenen Computeoptionen finden Sie unter [Einrichten von Computezielen für das Modelltraining](how-to-set-up-training-targets.md).

Konfigurieren Sie Ihre Umgebung für Computevorgänge und Trainingsausführungen mit der [`Environment`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py)-Klasse. Fügen Sie die PIP-Pakete `mlflow` und `azure-contrib-run` in den Abschnitt [`CondaDependencies`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py) der Umgebung ein. Erstellen Sie anschließend [`ScriptRunConfig`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.script_run_config.scriptrunconfig?view=azure-ml-py) mit Ihrer Remotecompute-Umgebung als Computeziel.

```Python
from azureml.core import Environment
from azureml.core.conda_dependencies import CondaDependencies
from azureml.core import ScriptRunConfig

exp = Experiment(workspace = 'my_workspace',
                 name='my_experiment')

mlflow_env = Environment(name='mlflow-env')

cd = CondaDependencies.create(pip_packages=['mlflow', 'azureml-contrib-run'])

mlflow_env.python.conda_dependencies = cd

src = ScriptRunConfig(source_directory='./my_script_location', script='my_training_script.py')

src.run_config.target = 'my-remote-compute-compute'
src.run_config.environment = mlflow_env
```

Importieren Sie in Ihrem Trainingsskript `mlflow`, um die APIs für die MLflow-Protokollierung zu verwenden und mit dem Protokollieren Ihrer Ausführungsmetriken zu beginnen.

```Python
import mlflow

with mlflow.start_run():
    mlflow.log_metric('example', 1.23)
```

Verwenden Sie bei dieser Konfiguration für Computevorgänge und Trainingsausführungen die `Experiment.submit('train.py')`-Methode, um eine Ausführung zu übermitteln. Mit dieser Methode wird der MLflow Tracking-URI automatisch festgelegt und die Protokollierung von MLflow an Ihren Arbeitsbereich geleitet.

```Python
run = exp.submit(src)
```

## <a name="track-azure-databricks-runs"></a>Nachverfolgen von Azure Databricks-Ausführungen

Mit MLflow-Tracking mit Azure Machine Learning können Sie die protokollierten Metriken und Artefakte aus Ihren Databricks-Ausführungen in Ihrem Azure Machine Learning-Arbeitsbereich speichern.

Um Ihre MLflow-Experimente mit Azure Databricks durchzuführen, müssen Sie zunächst einen [Azure Databricks-Arbeitsbereich und -Cluster](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal) erstellen.

Installieren Sie in Ihrem Cluster die Bibliothek *azureml-mlflow* von PyPi, um sicherzustellen, dass Ihr Cluster Zugriff auf die notwendigen Funktionen und Klassen hat.
Importieren Sie daraus Ihr Experimentnotebook, ordnen Sie dem Notebook Ihren Cluster zu, und führen Sie Ihr Experiment aus. 

### <a name="install-libraries"></a>Installieren von Bibliotheken

Navigieren Sie zum Installieren von Bibliotheken in Ihrem Cluster zur Registerkarte **Bibliotheken**, und klicken Sie auf **Neue installieren**.

 ![MLflow mit Azure Machine Learning: Diagramm](media/how-to-use-mlflow/azure-databricks-cluster-libraries.png)

Geben Sie im Feld **Paket** den Namen „azureml-mlflow“ ein, und klicken Sie dann auf „Installieren“. Wiederholen Sie diesen Schritt bei Bedarf, um für Ihr Experiment zusätzliche Pakete in Ihrem Cluster zu installieren.

 ![MLflow mit Azure Machine Learning: Diagramm](media/how-to-use-mlflow/install-libraries.png)

### <a name="set-up-your-notebook-and-workspace"></a>Einrichten von Notebook und Arbeitsbereich

Importieren Sie nach der Einrichtung Ihres Clusters Ihr Experimentnotebook, öffnen Sie es, und fügen Sie einen Cluster an.

Der folgende Code sollte in Ihrem Experimentnotebook enthalten sein. Mit diesem Code werden die Details Ihres Azure-Abonnements abgerufen, um Ihren Arbeitsbereich zu instanziieren. Dieser Code setzt voraus, dass Sie über eine Ressourcengruppe und einen Azure Machine Learning-Arbeitsbereich verfügen. Andernfalls können Sie diese [erstellen](how-to-manage-workspace.md). 

```python
import mlflow
import mlflow.azureml
import azureml.mlflow
import azureml.core

from azureml.core import Workspace
from azureml.mlflow import get_portal_url

subscription_id = 'subscription_id'

# Azure Machine Learning resource group NOT the managed resource group
resource_group = 'resource_group_name' 

#Azure Machine Learning workspace name, NOT Azure Databricks workspace
workspace_name = 'workspace_name'  

# Instantiate Azure Machine Learning workspace
ws = Workspace.get(name=workspace_name,
                   subscription_id=subscription_id,
                   resource_group=resource_group)
```

#### <a name="connect-your-azure-databricks-and-azure-machine-learning-workspaces"></a>Verbinden Ihres Azure Databricks- und Ihres Azure Machine Learning-Arbeitsbereichs

Im [Azure-Portal](https://ms.portal.azure.com) können Sie Ihren Azure Databricks-Arbeitsbereich (ADB-Arbeitsbereich) mit einem neuen oder vorhandenen Azure Machine Learning-Arbeitsbereich verknüpfen. Navigieren Sie zu diesem Zweck zu Ihrem ADB-Arbeitsbereich, und wählen Sie unten rechts die Schaltfläche **Link Azure Machine Learning workspace** (Azure Machine Learning-Arbeitsbereich verknüpfen) aus. Das Verknüpfen Ihrer Arbeitsbereiche ermöglicht es Ihnen, Ihre Experimentdaten im Azure Machine Learning-Arbeitsbereich nachzuverfolgen. 

### <a name="link-mlflow-tracking-to-your-workspace"></a>Verknüpfen des MLflow-Trackings mit Ihrem Arbeitsbereich

Nachdem Sie Ihren Arbeitsbereich instanziiert haben, legen Sie den MLflow-Tracking-URI fest. Dabei verknüpfen Sie das MLflow-Tracking mit dem Azure Machine Learning-Arbeitsbereich. Nach dem Verknüpfen werden Ihre gesamten Experimente in den verwalteten Azure Machine Learning-Nachverfolgungsdienst verschoben.

#### <a name="directly-set-mlflow-tracking-in-your-notebook"></a>Direktes Festlegen von MLflow-Nachverfolgung in Ihrem Notebook

```python
uri = ws.get_mlflow_tracking_uri()
mlflow.set_tracking_uri(uri)
```

Importieren Sie in Ihrem Trainingsskript mlflow, um die MLflow-Protokollierungs-APIs zu verwenden, und starten Sie die Protokollierung Ihrer Ausführungsmetriken. Das folgende Beispiel protokolliert die Epochenverlustmetrik. 

```python
import mlflow 
mlflow.log_metric('epoch_loss', loss.item()) 
```

#### <a name="automate-setting-mlflow-tracking"></a>Automatisieren der Einstellung für MLflow-Nachverfolgung

Anstatt Ihren Nachverfolgungs-URI in jeder späteren Sitzung des Experimentnotebooks für Ihre Cluster manuell festzulegen, können Sie dies automatisch vornehmen, indem Sie dieses [Azure Machine Learning Tracking Cluster-Initialisierungsskript](https://github.com/Azure/MachineLearningNotebooks/blob/3ce779063b000e0670bdd1acc6bc3a4ee707ec13/how-to-use-azureml/azure-databricks/linking/README.md) verwenden.

Wenn die Konfiguration ordnungsgemäß ist, können Sie Ihre MLflow-Nachverfolgungsdaten in der REST-API von Azure Machine Learning und in allen Clients sowie in Azure Databricks über die MLflow-Benutzeroberfläche oder über den MLflow-Client sehen.

## <a name="view-metrics-and-artifacts-in-your-workspace"></a>Anzeigen von Metriken und Artefakten in Ihrem Arbeitsbereich

Die Metriken und Artefakte aus dem MLflow-Protokoll werden in Ihrem Arbeitsbereich gespeichert. Sie können sie jederzeit anzeigen. Navigieren Sie dazu zu Ihrem Arbeitsbereich, und suchen Sie über den Namen nach dem Experiment in Ihrem Arbeitsbereich in [Azure Machine Learning-Studio](https://ml.azure.com).  Führen Sie alternativ den folgenden Befehl aus: 

```python
run.get_metrics()
ws.get_details()
```

## <a name="deploy-mlflow-models-as-a-web-service"></a>Bereitstellen von MLflow-Modellen als Webdienst

Durch die Bereitstellung Ihrer MLflow-Experimente als Azure Machine Learning-Webdienst können Sie die Azure Machine Learning-Funktionen zur Modellverwaltung und Datenabweichungserkennung nutzen und auf Ihre Produktionsmodelle anwenden.

Das folgende Diagramm veranschaulicht, wie Sie mit der MLflow-Bereitstellungs-API vorhandene MLflow-Modelle unabhängig vom Framework (PyTorch, Tensorflow, scikit-learn, ONNX usw.) als Azure Machine Learning-Webdienst bereitstellen und Ihre Produktionsmodelle im Arbeitsbereich verwalten können.

![MLflow mit Azure Machine Learning: Diagramm](media/how-to-use-mlflow/mlflow-diagram-deploy.png)

### <a name="log-your-model"></a>Protokollieren Ihres Modells

Bevor Sie die Bereitstellung durchführen können, stellen Sie sicher, dass Sie Ihr Modell gespeichert haben, damit Sie während der Bereitstellung auf das Modell und seinen Pfad verweisen können. Ihr Trainingsskript sollte Code enthalten, der der folgenden [mlflow.sklearn.log_model()](https://www.mlflow.org/docs/latest/python_api/mlflow.sklearn.html)-Methode ähnelt und Ihr Modell im angegebenen Ausgabeverzeichnis speichert. 

```python
# change sklearn to pytorch, tensorflow, etc. based on your experiment's framework 
import mlflow.sklearn

# Save the model to the outputs directory for capture
mlflow.sklearn.log_model(regression_model, model_save_path)
```
>[!NOTE]
> Fügen Sie den Parameter `conda_env` hinzu, um eine Wörterbuchdarstellung der Abhängigkeiten und der Umgebung zu übergeben, in der dieses Modell ausgeführt werden soll.

### <a name="retrieve-model-from-previous-run"></a>Abrufen eines Modells aus der vorherigen Ausführung

Um die Ausführung abzurufen, benötigen Sie die Ausführungs-ID und den Pfad des Ausführungsverlaufs, unter dem das Modell gespeichert wurde. 

```python
# gets the list of runs for your experiment as an array
experiment_name = 'experiment-with-mlflow'
exp = ws.experiments[experiment_name]
runs = list(exp.get_runs())

# get the run ID and the path in run history
runid = runs[0].id
model_save_path = 'model'
```

### <a name="create-docker-image"></a>Erstellen des Docker-Images

Die Funktion `mlflow.azureml.build_image()` erstellt ein Docker-Image aus dem gespeicherten Modell für ein Framework. Sie erstellt automatisch den frameworkspezifischen, hergeleiteten Wrapper-Code und gibt die Paketabhängigkeiten für Sie an. Geben Sie den Modellpfad, Ihren Arbeitsbereich, die Ausführungs-ID und andere Parameter an.

Mit dem folgenden Code wird ein Docker-Image mit *runs:/<run.id>/model* als model_uri-Pfad für ein Experiment mit Scikit-learn erstellt.

```python
import mlflow.azureml

azure_image, azure_model = mlflow.azureml.build_image(model_uri='runs:/{}/{}'.format(runid, model_save_path),
                                                      workspace=ws,
                                                      model_name='sklearn-model',
                                                      image_name='sklearn-image',
                                                      synchronous=True)
```
Die Erstellung des Docker-Images kann einige Minuten dauern. 

### <a name="deploy-the-docker-image"></a>Bereitstellen des Docker-Images 

Verwenden Sie nach der Erstellung des Images das Azure Machine Learning SDK, um das Image als Webdienst bereitzustellen.

Geben Sie zunächst die Bereitstellungskonfiguration an. Azure Container Instance (ACI) eignet sich gut für eine schnelle Dev-Test-Bereitstellung, Azure Kubernetes Service (AKS) hingegen für skalierbare Produktionsumgebungen.

#### <a name="deploy-to-aci"></a>Bereitstellen für ACI

Richten Sie Ihre Bereitstellungskonfiguration mit der Methode [deploy_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice?view=azure-ml-py#deploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none--dns-name-label-none-) ein. Sie können auch Tags und Beschreibungen hinzufügen, um den Überblick über Ihren Webdienst zu behalten.

```python
from azureml.core.webservice import AciWebservice, Webservice

# Configure 
aci_config = AciWebservice.deploy_configuration(cpu_cores=1, 
                                                memory_gb=1, 
                                                tags={'method' : 'sklearn'}, 
                                                description='Diabetes model',
                                                location='eastus2')
```

Stellen Sie dann das Image mit der Methode [deploy_from_im_image()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice(class)?view=azure-ml-py#deploy-from-image-workspace--name--image--deployment-config-none--deployment-target-none-) des Azure Machine Learning SDK bereit. 

```python
webservice = Webservice.deploy_from_image( image=azure_image, 
                                           workspace=ws, 
                                           name='diabetes-model-1', 
                                           deployment_config=aci_config)

webservice.wait_for_deployment(show_output=True)
```
#### <a name="deploy-to-aks"></a>Bereitstellen für AKS

Für die Bereitstellung in AKS erstellen Sie zunächst einen AKS-Cluster und übertragen das bereitzustellende Docker-Image. Für dieses Beispiel übernehmen Sie das zuvor erstellte Image aus der ACI-Bereitstellung.

Um das Image aus der vorherigen ACI-Bereitstellung abzurufen, verwenden Sie die Klasse [Image](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image.image.image?view=azure-ml-py). 

```python
from azureml.core.image import Image

# Get the image by name, you can change this based on the image you want to deploy
myimage = Image(workspace=ws, name='sklearn-image') 
```

Erstellen Sie mit der [ComputeTarget.create()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.computetarget?view=azure-ml-py#create-workspace--name--provisioning-configuration-)-Methode einen AKS-Cluster. Die Erstellung eines neuen Clusters kann 20 bis 25 Minuten dauern.

```python
from azureml.core.compute import AksCompute, ComputeTarget

# Use the default configuration (can also provide parameters to customize)
prov_config = AksCompute.provisioning_configuration()

aks_name = 'aks-mlflow' 

# Create the cluster
aks_target = ComputeTarget.create(workspace=ws, 
                                  name=aks_name, 
                                  provisioning_configuration=prov_config)

aks_target.wait_for_completion(show_output = True)

print(aks_target.provisioning_state)
print(aks_target.provisioning_errors)
```
Richten Sie Ihre Bereitstellungskonfiguration mit der Methode [deploy_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice?view=azure-ml-py#deploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none--dns-name-label-none-) ein. Sie können auch Tags und Beschreibungen hinzufügen, um den Überblick über Ihren Webdienst zu behalten.

```python
from azureml.core.webservice import Webservice, AksWebservice
from azureml.core.image import ContainerImage

# Set the web service configuration (using default here with app insights)
aks_config = AksWebservice.deploy_configuration(enable_app_insights=True)

# Unique service name
service_name ='aks-service'
```

Stellen Sie dann das Image mit der Methode [deploy_from_im_image()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice(class)?view=azure-ml-py#deploy-from-image-workspace--name--image--deployment-config-none--deployment-target-none-) des Azure Machine Learning SDK bereit. 

```python
# Webservice creation using single command
aks_service = Webservice.deploy_from_image( workspace=ws, 
                                            name=service_name,
                                            deployment_config = aks_config
                                            image = myimage,
                                            deployment_target = aks_target)

aks_service.wait_for_deployment(show_output=True)
```

Die Bereitstellung des Diensts kann mehrere Minuten dauern.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Beachten Sie Folgendes, falls Sie nicht planen, die protokollierten Metriken und Artefakte in Ihrem Arbeitsbereich zu verwenden: Das Löschen einzelner Einträge ist derzeit nicht möglich. Löschen Sie stattdessen die Ressourcengruppe, die das Speicherkonto und den Arbeitsbereich enthält, damit hierfür keine Gebühren anfallen:

1. Wählen Sie ganz links im Azure-Portal **Ressourcengruppen** aus.

   ![Löschen im Azure-Portal](media/how-to-use-mlflow/delete-resources.png)

1. Wählen Sie in der Liste die Ressourcengruppe aus, die Sie erstellt haben.

1. Wählen Sie die Option **Ressourcengruppe löschen**.

1. Geben Sie den Ressourcengruppennamen ein. Wählen Sie anschließend die Option **Löschen**.


## <a name="example-notebooks"></a>Beispielnotebooks

Die [Notebooks „MLflow mit Azure ML“](https://aka.ms/azureml-mlflow-examples) demonstrieren und erklären die in diesem Artikel vorgestellten Konzepte.

## <a name="next-steps"></a>Nächste Schritte
* [Verwalten Ihrer Modelle](concept-model-management-and-deployment.md)
* Überwachen Ihrer Produktionsmodelle auf [Datenabweichungen](how-to-monitor-data-drift.md)
