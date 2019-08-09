---
title: Verwenden von MLflow mit Azure Machine Learning Service
titleSuffix: Azure Machine Learning service
description: Mit dem Azure Machine Learning Service können Sie Metriken und Artefakte protokollieren und Modelle für die Produktion mit MLflow bereitstellen.
services: machine-learning
author: rastala
ms.author: roastala
ms.service: machine-learning
ms.subservice: core
ms.reviewer: nibaccam
ms.topic: conceptual
ms.date: 07/15/2019
ms.custom: seodec18
ms.openlocfilehash: 55722c35dddcbf8a20f4f51958170938225e87e5
ms.sourcegitcommit: fecb6bae3f29633c222f0b2680475f8f7d7a8885
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68668390"
---
# <a name="track-metrics-and-deploy-models-with-mlflow-and-azure-machine-learning-service-preview"></a>Mit MLflow und Azure Machine Learning Service Metriken nachverfolgen und Modelle bereitstellen (Vorschauversion)

In diesem Artikel wird veranschaulicht, wie Sie den Nachverfolgungs-URI und die Protokollierungs-API von MLflow, auch bekannt als [MLflow Tracking](https://mlflow.org/docs/latest/quickstart.html#using-the-tracking-api), mit Azure Machine Learning Service nutzen. Dies ermöglicht Ihnen Folgendes:

+ Sie können die Metriken und Artefakte Ihrer Experimente im [Azure Machine Learning Service-Arbeitsbereich](https://docs.microsoft.com/azure/machine-learning/service/concept-azure-machine-learning-architecture#workspaces) nachverfolgen und protokollieren. Falls Sie MLflow-Tracking bereits für Ihre Experimente verwenden, ist der Arbeitsbereich ein zentraler, sicherer und skalierbarer Ort zum Speichern Ihrer Trainingsmetriken und -modelle.

+ Stellen Sie Ihre MLflow-Experimente als Azure Machine Learning-Webdienst bereit. Durch die Bereitstellung als Webdienst können Sie die Funktionen von Azure Machine Learning zur Überwachung und Erkennung von Dateiabweichungen für Ihre Produktionsmodelle einsetzen. 

[MLflow](https://www.mlflow.org) ist eine Open-Source-Bibliothek zum Verwalten des Lebenszyklus Ihrer Machine Learning-Experimente. MLFlow-Tracking ist eine Komponente von MLflow, die die Metrik Ihrer Trainingsausführungen und Modellartefakte protokolliert und nachverfolgt – unabhängig von der Umgebung Ihres Experiments. Dies ist lokal, auf einem virtuellen Computer, einem Remotecomputecluster oder sogar auf Azure Databricks möglich.

![MLflow mit Azure Machine Learning: Diagramm](media/how-to-use-mlflow/mlflow-diagram-track.png)

## <a name="compare-mlflow-and-azure-machine-learning-clients"></a>Vergleich von MLflow mit Azure Machine Learning-Clients

 In der Tabelle unten sind die unterschiedlichen Clients, für die Azure Machine Learning Service genutzt werden kann, und die entsprechenden Funktionen zusammengefasst.

 MLflow-Tracking verfügt über Funktionen für Metrikprotokollierung und Artefaktspeicherung, die sonst nur über das [Azure Machine Learning-SDK für Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) zur Verfügung stehen.


| | MLflow-Tracking und ‑Bereitstellung | Python-SDK für Azure Machine Learning |  Azure Machine Learning-CLI | Azure-Portal|
|---|---|---|---|---|
| Verwalten des Arbeitsbereichs |   | ✓ | ✓ | ✓ |
| Verwenden von Datenspeichern  |   | ✓ | ✓ | |
| Protokollieren von Metriken      | ✓ | ✓ |   | |
| Hochladen von Artefakten | ✓ | ✓ |   | |
| Anzeigen von Metriken     | ✓ | ✓ | ✓ | ✓ |
| Verwalten von Compute   |   | ✓ | ✓ | ✓ |
| Bereitstellen von Modellen    | ✓ | ✓ | ✓ | ✓ |
|Überwachung der Modellleistung||✓|  |   |
| Erkennen von Datenabweichungen |   | ✓ |   | ✓ |

## <a name="prerequisites"></a>Voraussetzungen

* [Installieren Sie MLflow](https://mlflow.org/docs/latest/quickstart.html).
* [Installieren Sie das Azure Machine Learning-SDK für Python auf Ihrem lokalen Computer, und erstellen Sie einen Azure Machine Learning-Arbeitsbereich](setup-create-workspace.md#sdk). Das SDK stellt die Konnektivität für MLflow für den Zugriff auf Ihren Arbeitsbereich bereit.

## <a name="track-experiment-runs"></a>Nachverfolgen von Experimentausführungen

Mit MLflow-Tracking mit Azure Machine Learning Service können Sie die protokollierten Metriken und Artefakte aus Ihren lokalen und Remoteausführungen in Ihrem Azure Machine Learning-Arbeitsbereich speichern.

### <a name="local-runs"></a>Lokale Ausführungen

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

### <a name="remote-runs"></a>Remoteausführungen

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

Verwenden Sie bei dieser Konfiguration für Computevorgänge und Trainingsausführungen die `Experiment.submit('train.py')`-Methode, um eine Ausführung zu übermitteln. Hiermit wird der MLflow Tracking-URI automatisch festgelegt und die Protokollierung von MLflow an Ihren Arbeitsbereich geleitet.

```Python
run = exp.submit(src)
```

### <a name="mlflow-with-azure-databricks-runs"></a>MLflow mit Azure Databricks-Ausführungen

Um Ihre MLflow-Experimente mit Azure Databricks durchzuführen, müssen Sie zunächst einen [Azure Databricks-Arbeitsbereichcluster](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal) erstellen. Installieren Sie in Ihrem Cluster die*azureml-mlflow*-Bibliothek von PyPi, um sicherzustellen, dass Ihr Cluster Zugriff auf die notwendigen Funktionen und Klassen hat.

#### <a name="install-libraries"></a>Installieren von Bibliotheken

Um Bibliotheken auf Ihrem Cluster zu installieren, navigieren Sie zur Registerkarte **Bibliotheken** und klicken Sie auf **Neu installieren**.

 ![MLflow mit Azure Machine Learning: Diagramm](media/how-to-use-mlflow/azure-databricks-cluster-libraries.png)

Geben Sie im Feld **Paket** „azureml-mlflow“ ein und klicken Sie dann auf „Installieren“. Wiederholen Sie diesen Schritt bei Bedarf, um zusätzliche Pakete für Ihr Experiment in Ihrem Cluster zu installieren.

 ![MLflow mit Azure Machine Learning: Diagramm](media/how-to-use-mlflow/install-libraries.png)

#### <a name="notebook-and-workspace-set-up"></a>Einrichtung von Notebook und Arbeitsbereich

Sobald Ihr Cluster eingerichtet ist, importieren Sie Ihr Experimentnotebook, öffnen es und hängen Ihren Cluster daran an.

Der folgende Code sollte sich in Ihrem Experimentnotebook befinden. Dadurch werden die Details Ihres Azure-Abonnements abgerufen, um Ihren Arbeitsbereich zu instanziieren. Dies setzt voraus, dass Sie eine vorhandene Ressourcengruppe und den Azure Machine Learning-Arbeitsbereich eingerichtet haben. Andernfalls können Sie [diese erstellen](setup-create-workspace.md#portal). 

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
#### <a name="set-mlflow-tracking-uri"></a>MLflow-Tracking URI einrichten
Nachdem Sie Ihren Arbeitsbereich instanziiert haben, legen Sie die MLflow-Tracking URI fest. Auf diese Weise verknüpfen Sie das MLflow-Tracking mit dem Azure Machine Learning-Arbeitsbereich. Danach werden alle Ihre Experimente in den verwalteten Azure Machine Learning Tracking Service verschoben.

```python
uri = ws.get_mlflow_tracking_uri()
mlflow.set_tracking_uri(uri)
```

Importieren Sie mlflow in Ihr Trainingsskript, um die MLflow-Protokollierungs-APIs zu verwenden, und starten Sie die Protokollierung Ihrer Ausführungsmetriken. Das folgende Beispiel protokolliert die Epochenverlustmetrik. 

```python
import mlflow 
mlflow.log_metric('epoch_loss', loss.item()) 
```

## <a name="view-metrics-and-artifacts-in-your-workspace"></a>Anzeigen von Metriken und Artefakten in Ihrem Arbeitsbereich

Die Metriken und Artefakte aus dem MLflow-Protokoll werden in Ihrem Arbeitsbereich gespeichert. Um sie jederzeit einsehen zu können, navigieren Sie zu Ihrem Arbeitsbereich und suchen Sie nach dem Namen des Experiments auf dem [Azure-Portal](https://portal.azure.com). Führen Sie alternativ den folgenden Code aus. 

```python
run.get_metrics()
ws.get_details()
```

## <a name="deploy-mlflow-models-as-a-web-service"></a>MLflow-Modelle als Webservice bereitstellen

Durch Bereitstellung Ihrer MLflow-Experimente als Azure Machine Learning-Webdienst können Sie die Funktionen der Azure Machine Learning-Modellverwaltung und Datenabweichungserkennung nutzen und auf Ihre Produktionsmodelle anwenden.

![MLflow mit Azure Machine Learning: Diagramm](media/how-to-use-mlflow/mlflow-diagram-deploy.png)

### <a name="log-your-model"></a>Protokollieren Sie Ihr Modell
Bevor wir die Bereitstellung durchführen können, stellen Sie sicher, dass Ihr Modell gespeichert ist, damit Sie während der Bereitstellung auf das Modell und seinen Pfad verweisen können. In Ihrem Trainingsskript sollte es einen Code ähnlich der folgenden [mlflow.sklearn.log_model()](https://www.mlflow.org/docs/latest/python_api/mlflow.sklearn.html)-Methode geben, der Ihren Modell im angegebenen Ausgabeverzeichnis speichert. 

```python
# change sklearn to pytorch, tensorflow, etc. based on your experiment's framework 
import mlflow.sklearn

# Save the model to the outputs directory for capture
mlflow.sklearn.log_model(regression_model, model_save_path)
```
>[!NOTE]
> Fügen Sie den Parameter „`conda_env`“ hinzu, um eine Wörterbuchdarstellung der Abhängigkeiten und der Umgebung zu übergeben, in der dieses Modell ausgeführt werden soll.

### <a name="retrieve-model-from-previous-run"></a>Modell aus der vorherigen Ausführung abrufen

Um die gewünschte Ausführung abzurufen, benötigen wir die Ausführungs-ID und den Pfad in des Ausführungsverlaufs, in dem das Modell gespeichert wurde. 

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

Die Funktion `mlflow.azureml.build_image()` erstellt ein Docker-Image aus dem gespeicherten Modell für ein Framework. Sie erstellt automatisch den frameworkspezifischen, hergeleiteten Wrapper-Code und spezifiziert die Paketabhängigkeiten für Sie. Geben Sie den Modellpfad, Ihren Arbeitsbereich, die Ausführungs-ID und andere Parameter an.

Im folgenden Code erstellen wir ein Docker-Image mit *runs:/<run.id>/model* als model_uri-Pfad für ein Experiment mit Scikit-learn.

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

Nachdem das Bild erstellt wurde, verwenden Sie das Azure Machine Learning-SDK, um das Image als Webdienst bereitzustellen.

Geben Sie zunächst die Bereitstellungskonfiguration an. Azure Container Instance (ACI) ist eine geeignete Wahl für eine schnelle Bereitstellung von Dev-Tests, während Azure Kubernetes Service (AKS) für skalierbare Produktionsumgebungen geeignet ist.

#### <a name="deploy-to-aci"></a>Bereitstellen für ACI

Richten Sie Ihre Bereitstellungskonfiguration mit der [deploy_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice?view=azure-ml-py#deploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none--dns-name-label-none-)-Methode ein. Sie können auch Tags und Beschreibungen hinzufügen, um den Überblick über Ihren Webdienst zu behalten.

```python
from azureml.core.webservice import AciWebservice, Webservice

# Configure 
aci_config = AciWebservice.deploy_configuration(cpu_cores=1, 
                                                memory_gb=1, 
                                                tags={'method' : 'sklearn'}, 
                                                description='Diabetes model',
                                                location='eastus2')
```

Stellen Sie dann das Image mit der [deploy_from_im_image()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice(class)?view=azure-ml-py#deploy-from-image-workspace--name--image--deployment-config-none--deployment-target-none-)-Methode des Azure Machine Learning-SDK bereit. 

```python
webservice = Webservice.deploy_from_image( image=azure_image, 
                                           workspace=ws, 
                                           name='diabetes-model-1', 
                                           deployment_config=aci_config)

webservice.wait_for_deployment(show_output=True)
```
#### <a name="deploy-to-aks"></a>Bereitstellen für AKS

Um in AKS bereitzustellen, müssen Sie einen AKS-Cluster erstellen und das Docker-Image, das Sie bereitstellen möchten, übernehmen. Für dieses Beispiel übernehmen wir das zuvor erstellte Image aus unserer ACI-Bereitstellung.

Um das Image aus der vorherigen ACI-Bereitstellung zu erhalten, verwenden wir die Klasse [Image](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image.image.image?view=azure-ml-py). 

```python
from azureml.core.image import Image

# Get the image by name, you can change this based on the image you want to deploy
myimage = Image(workspace=ws, name='sklearn-image') 
```

Erstellen Sie die AKS-Berechnung. Es kann 20–25 Minuten dauern, bis ein neuer Cluster erstellt wurde.

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
Richten Sie Ihre Bereitstellungskonfiguration mit der [deploy_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice?view=azure-ml-py#deploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none--dns-name-label-none-)-Methode ein. Sie können auch Tags und Beschreibungen hinzufügen, um den Überblick über Ihren Webdienst zu behalten.

```python
from azureml.core.webservice import Webservice, AksWebservice
from azureml.core.image import ContainerImage

# Set the web service configuration (using default here with app insights)
aks_config = AksWebservice.deploy_configuration(enable_app_insights=True)

# Unique service name
service_name ='aks-service'
```

Stellen Sie dann das Image mit der [deploy_from_im_image()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice(class)?view=azure-ml-py#deploy-from-image-workspace--name--image--deployment-config-none--deployment-target-none-)-Methode des Azure Machine Learning-SDK bereit. 

```python
# Webservice creation using single command
aks_service = Webservice.deploy_from_image( workspace=ws, 
                                            name=service_name,
                                            deployment_config = aks_config
                                            image = myimage,
                                            deployment_target = aks_target)

aks_service.wait_for_deployment(show_output=True)
```

Die Bereitstellung des Dienstes kann mehrere Minuten dauern.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Beachten Sie Folgendes, falls Sie nicht planen, die protokollierten Metriken und Artefakte in Ihrem Arbeitsbereich zu verwenden: Das Löschen einzelner Einträge ist derzeit nicht möglich. Löschen Sie stattdessen die Ressourcengruppe, die das Speicherkonto und den Arbeitsbereich enthält, damit hierfür keine Gebühren anfallen:

1. Wählen Sie ganz links im Azure-Portal **Ressourcengruppen** aus.

   ![Löschen im Azure-Portal](media/how-to-use-mlflow/delete-resources.png)

1. Wählen Sie in der Liste die Ressourcengruppe aus, die Sie erstellt haben.

1. Wählen Sie die Option **Ressourcengruppe löschen**.

1. Geben Sie den Ressourcengruppennamen ein. Wählen Sie anschließend die Option **Löschen**.


## <a name="example-notebooks"></a>Beispielnotebooks

Die [„MLflow mit Azure ML“-Notebooks](https://aka.ms/azureml-mlflow-examples) demonstrieren und erklären die in diesem Artikel vorgestellten Konzepte.

## <a name="next-steps"></a>Nächste Schritte
* [Verwalten Sie Ihre Modelle](concept-model-management-and-deployment.md).
* Überwachen Sie Ihre Produktionsmodelle auf [Datenabweichungen](how-to-monitor-data-drift.md).
