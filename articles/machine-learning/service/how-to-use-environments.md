---
title: Erstellen von wiederverwendbaren ML-Umgebungen
titleSuffix: Azure Machine Learning
description: Erstellen und Verwalten von Umgebungen für Modelltraining und -bereitstellung. Verwalten von Python-Paketen und anderen Einstellungen für die Umgebung.
services: machine-learning
author: rastala
ms.author: roastala
ms.reviewer: nibaccam
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 09/27/2019
ms.openlocfilehash: 4d4d83e12d284ce760b8a7e87fd42e6c8ebb4850
ms.sourcegitcommit: be344deef6b37661e2c496f75a6cf14f805d7381
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/07/2019
ms.locfileid: "72001216"
---
# <a name="create-and-manage-reusable-environments-for-training-and-deployment-with-azure-machine-learning"></a>Erstellen und verwalten Sie wiederverwendbare Umgebungen für Trainings und Bereitstellungen mit Azure Machine Learning.

In diesem Artikel erfahren Sie, wie Sie Azure Machine Learning-[Umgebungen](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py) erstellen und verwalten, sodass Sie die Softwareabhängigkeiten ihrer Projekte nachverfolgen und reproduzieren können, wenn diese sich weiterentwickeln.

Die Verwaltung von Softwareabhängigkeiten ist eine gängige Aufgabe für Entwickler. Sie möchten sicherstellen können, dass Builds ohne eine Menge manueller Softwarekonfiguration reproduzierbar sind. Mit Lösungen für die lokale Entwicklung, wie z. B. pip und Conda, stellt die Environments-Klasse von Azure Machine Learning eine Lösung für sowohl lokale als auch verteilte Cloudentwicklung bereit.

Die Beispiele in diesem Artikel veranschaulichen Folgendes:

* Erstellen einer Umgebung und Angeben von Paketabhängigkeiten
* Abrufen und Aktualisieren von Umgebungen
* Verwenden der Umgebung für Training
* Verwenden der Umgebung für die Webdienstbereitstellung

## <a name="what-are-environments"></a>Was sind Umgebungen?

Umgebungen geben die Python-Pakete, Umgebungsvariablen und Softwareeinstellungen bezüglich Ihrer Trainings- und Bewertungsskripts sowie die Laufzeiten (Python, Spark oder Docker) an. Dabei handelt es sich um verwaltete und versionsverwaltete Entitäten innerhalb Ihres Azure Machine Learning-Arbeitsbereichs, die reproduzierbare, überprüfbare und portierbare Machine Learning-Workflows über verschiedene Computeziele hinweg ermöglichen.

Sie können ein Umgebungsobjekt auf Ihrem lokalen Compute verwenden, um Ihr Trainingsskript zu entwickeln, dieselbe Umgebung auf Azure Machine Learning Compute für das skalierte Modelltraining wiederverwenden und Ihr Modell sogar mit dieser selben Umgebung bereitstellen.

Im Folgenden wird veranschaulicht, dass dasselbe Umgebungsobjekt sowohl in Ihrer Laufzeitkonfiguration für das Training als auch in der Rückschluss- und Bereitstellungskonfiguration für Webdienstbereitstellungen verwendet werden kann.

![Diagramm der Umgebung im Machine Learning-Workflow](./media/how-to-use-environments/ml-environment.png)

### <a name="types-of-environments"></a>Typen von Umgebungen

Umgebungen lassen sich weitgehend in drei Kategorien unterteilen: **zusammengestellt**, **benutzerverwaltet** und **systemverwaltet**.

Zusammengestellte Umgebungen werden von Azure Machine Learning bereitgestellt und sind standardmäßig in Ihrem Arbeitsbereich verfügbar. Sie enthalten Sammlungen von Python-Paketen und -Einstellungen, die Ihnen helfen, verschiedene Machine Learning-Frameworks zu starten. 

Bei benutzerverwalteten Umgebungen sind Sie für die Einrichtung Ihrer Umgebung und die Installation aller Pakete, die Ihr Trainingsskript auf dem Computeziel benötigt, selbst verantwortlich. Conda wird Ihre Umgebung nicht überprüfen oder Installationen für Sie vornehmen. 

Systemverwaltete Umgebungen werden verwendet, wenn Sie möchten, dass [Conda](https://conda.io/docs/) die Python-Umgebung und die Skriptabhängigkeiten für Sie verwalten soll. Der Dienst setzt diesen Typ von Umgebung standardmäßig voraus, da er für Remotecomputeziele, die nicht manuell konfiguriert werden können, sehr praktisch ist.

## <a name="prerequisites"></a>Voraussetzungen

* [Installiertes](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py) Azure Machine Learning SDK für Python.
* Ein [Azure Machine Learning-Arbeitsbereich](how-to-manage-workspace.md).


## <a name="create-an-environment"></a>Erstellen einer Umgebung

Es gibt mehrere Möglichkeiten zum Erstellen einer Umgebung für Ihre Experimente.

### <a name="use-curated-environment"></a>Verwenden einer zusammengestellten Umgebung

Sie können zunächst eine der zusammengestellten Umgebungen auswählen. 

* Die __AzureML-Minimal__-Umgebung enthält einen minimalen Satz von Paketen, um die Nachverfolgung von Durchläufen und das Hochladen von Ressourcen zu ermöglichen. Sie können sie als Ausgangspunkt für Ihre eigene Umgebung verwenden.

* Die __AzureML-Tutorial__-Umgebung enthält allgemeine Data Science-Pakete wie Scikit-Learn, Pandas und Matplotlib sowie einen größeren Satz von azureml-sdk-Paketen.

Zusammengestellte Umgebungen werden durch zwischengespeicherte Docker-Images unterstützt, wodurch die Kosten für die Vorbereitung der Ausführung reduziert werden.

Verwenden Sie die __Environment.get__-Methode, um eine der zusammengestellten Umgebungen auszuwählen:

```python
from azureml.core import Workspace, Environment

ws = Workspace.from_config()
env = Environment.get(workspace=ws, name="AzureML-Minimal")
```

Sie können die zusammengestellten Umgebungen und deren Pakete mit folgendem Code auflisten:
```python
envs = Environment.list(workspace=ws)

for env in envs:
    if env.startswith("AzureML"):
        print("Name",env)
        print("packages", envs[env].python.conda_dependencies.serialize_to_string())
```

> [!WARNING]
>  Beginnen Sie den Namen Ihrer eigenen Umgebung nicht mit dem Präfix _AzureML_. Es ist für zusammengestellte Umgebungen reserviert.

### <a name="instantiate-an-environment-object"></a>Instanziieren eines Umgebungsobjekts

Um manuell eine Umgebung zu erstellen, importieren Sie die Environment-Klasse aus dem SDK, und instanziieren Sie ein Umgebungsobjekt mit dem folgenden Code.

```python
from azureml.core import Environment
Environment(name="myenv")
```

### <a name="conda-and-pip-specification-files"></a>Conda- und pip-Spezifikationsdateien

Sie können eine Umgebung auch aus einer Conda-Spezifikations- oder einer pip-Anforderungsdatei erstellen.
Verwenden Sie die Methode [from_conda_specification()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#from-conda-specification-name--file-path-) oder [from_pip_requirements()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#from-pip-requirements-name--file-path-), und nehmen Sie den Namen Ihrer Umgebung sowie den Dateipfad der gewünschten Datei in das Argument der Methode auf.

```python
# From a Conda specification file
myenv = Environment.from_conda_specification(name = "myenv",
                                             file_path = "path-to-conda-specification-file")

#From a pip requirements file
myenv = Environment.from_pip_requirements(name = "myenv"
                                          file_path = "path-to-pip-requirements-file")
```

### <a name="existing-conda-environment"></a>Vorhandene Conda-Umgebung

Wenn Sie über eine vorhandene Conda-Umgebung auf Ihrem lokalen Computer verfügen, bietet der Dienst eine Lösung für die Erstellung eines Umgebungsobjekts daraus. Auf diese Weise können Sie Ihre lokale interaktive Umgebung bei Remoteausführungen wiederverwenden.

Der folgende Code erstellt ein Umgebungsobjekt aus der vorhandenen Conda-Umgebung `mycondaenv` mit der [from_existing_conda_environment()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#from-existing-conda-environment-name--conda-environment-name-)-Methode.

``` python
myenv = Environment.from_existing_conda_environment(name = "myenv",
                                                    conda_environment_name = "mycondaenv")
```

### <a name="automatically-create-environments"></a>Automatisches Erstellen von Umgebungen

Sie erstellen automatisch eine Umgebung, indem Sie einen Trainingslauf mit der submit()-Methode übermitteln. Wenn Sie einen Trainingslauf übermitteln, kann das Erstellen der neuen Umgebung einige Minuten dauern, je nach der Größe der erforderlichen Abhängigkeiten. 

Wenn Sie in Ihrer Ausführungskonfiguration keine Umgebung angeben, bevor Sie den Lauf übermitteln, wird eine Standardumgebung für Sie erstellt.

```python
from azureml.core import ScriptRunConfig, Experiment, Environment
# Create experiment 
myexp = Experiment(workspace=ws, name = "environment-example")

# Attaches training script and compute target to run config
runconfig = ScriptRunConfig(source_directory=".", script="example.py")
runconfig.run_config.target = "local"

# Submit the run
run = myexp.submit(config=runconfig)

# Shows each step of run 
run.wait_for_completion(show_output=True)
```

Ähnlich können Sie, wenn Sie ein [`Estimator`](https://docs.microsoft.com//python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py)-Objekt für das Training verwenden, die Estimator-Instanz direkt als Lauf übermitteln, ohne eine Umgebung angeben zu müssen. Das `Estimator`-Objekt kapselt die Umgebung und das Computeziel bereits ein.


## <a name="add-packages-to-an-environment"></a>Hinzufügen von Paketen zu einer Umgebung

Pakete fügen Sie einer Umgebung mit Conda-, pip- oder Private Wheels-Dateien hinzu. Geben Sie jede Paketabhängigkeit mithilfe der [CondaDependency-Klasse](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py) an, und fügen Sie sie dem PythonSection der Umgebung hinzu.

### <a name="conda-and-pip-packages"></a>Conda- und pip-Pakete

Wenn ein Paket in einem Conda-Paketrepository verfügbar ist, wird empfohlen, die Conda-Installation gegenüber der pip-Installation zu bevorzugen. Der Grund hierfür ist, dass Conda-Pakete in der Regel über vorgefertigte Binärdateien verfügen, die die Installation zuverlässiger machen.

Im folgenden Beispiel werden die Pakete `scikit-learn`, spezifisch Version 0.21.3, und `pillow` mit den Methoden [`add_conda_package()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py#add-conda-package-conda-package-) und [`add_pip_package()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py#add-pip-package-pip-package-) zur Umgebung `myenv` hinzugefügt.

```python
from azureml.core import Environment
from azureml.core.environment import CondaDependencies

myenv = Environment(name="myenv")
conda_dep = CondaDependencies()

# Installs scikit-learn version 0.21.3 conda package
conda_dep.add_conda_package("scikit-learn==0.21.3")

# Adds dependencies to PythonSection of myenv
myenv.python.conda_dependencies=conda_dep
```

### <a name="private-wheel-files"></a>Private Wheel-Dateien

Sie können private Wheel-Dateien von pip verwenden, indem Sie sie zuerst mithilfe der statischen [`add_private_pip_wheel()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#add-private-pip-wheel-workspace--file-path--exist-ok-false-)-Methode in ihren Arbeitsbereichsspeicher hochladen, dann die Speicher-URL erfassen und die URL an die `add_pip_package()`-Methode übergeben.

```python
# During environment creation the service replaces the URL by secure SAS URL, so your wheel file is kept private and secure
whl_url = Environment.add_private_pip_wheel(workspace=ws,file_path = "my-custom.whl")
myenv = Environment(name="myenv")
conda_dep = CondaDependencies()
conda_dep.add_pip_package(whl_url)
myenv.python.conda_dependencies=conda_dep
```

## <a name="manage-environments"></a>Verwalten von Umgebungen

Verwalten Sie Umgebungen, damit Sie sie über Computeziele hinweg und mit andere Benutzern des Arbeitsbereichs zusammen aktualisieren, nachverfolgen und wiederverwenden können.

### <a name="register-environments"></a>Registrieren von Umgebungen

Die Umgebung wird automatisch bei Ihrem Arbeitsbereich registriert, wenn Sie einen Lauf übermitteln oder einen Webdienst bereitstellen. Sie können die Umgebung auch manuell mithilfe der [register()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py#register-workspace-)-Methode registrieren. Durch diesen Vorgang wird die Umgebung zu einer Entität, die in der Cloud nachverfolgt und versionsverwaltet wird und von Arbeitsbereichsbenutzern gemeinsam genutzt werden kann.

Mit folgendem Code wird die Umgebung `myenv` bei dem Arbeitsbereich `ws` registriert.

```python
myenv.register(workspace=ws)
```

Wenn die Umgebung zum ersten Mal beim Training oder der Bereitstellung verwendet wird, wird sie bei dem Arbeitsbereich registriert, erstellt und auf dem Computeziel bereitgestellt. Die Umgebungen werden vom Dienst zwischengespeichert. Die Wiederverwendung einer zwischengespeicherten Umgebung braucht viel weniger Zeit als die Verwendung eines neuen oder aktualisierten Diensts.

### <a name="get-existing-environments"></a>Abrufen vorhandener Umgebungen

Die Environment-Klasse bietet Methoden, mit denen Sie vorhandene Umgebungen in Ihrem Arbeitsbereich anhand des Namens, als Liste oder durch einen spezifischen Trainingslauf zu Zwecken der Problembehandlung oder Überwachung sowie zur Reproduzierbarkeit abrufen können.

#### <a name="view-list-of-environments"></a>Anzeigen einer Liste der Umgebungen

Die Umgebungen in Ihrem Arbeitsbereich können Sie mit [`Environment.list(workspace="workspace_name")`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py#list-workspace-) anzeigen und dann eine wiederzuverwendende auswählen.

#### <a name="get-environment-by-name"></a>Abrufen einer Umgebung anhand des Namens

Sie können auch eine bestimmte Umgebung mittels Name und Version abrufen.
Der folgende Code verwendet die [get()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py#get-workspace--name--version-none-)-Methode, um Version `1` der Umgebung `myenv` in den Arbeitsbereich `ws` abzurufen.

```python
restored_environment = Environment.get(workspace=ws,name="myenv",version="1")
```

#### <a name="training-run-specific-environment"></a>Trainingslauf für eine spezifische Umgebung

Um die für einen bestimmten Lauf verwendete Umgebung nach Abschluss des Trainings abzurufen, verwenden Sie die [get_environment()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#get-environment--)-Methode in der Run-Klasse.

```python
from azureml.core import Run
Run.get_environment()
```

### <a name="update-an-existing-environment"></a>Aktualisieren einer vorhandenen Umgebung

Wenn Sie Änderungen an einer vorhandenen Umgebung vornehmen, z.B. ein Python-Paket hinzufügen, wird eine neue Version der Umgebung erstellt, wenn Sie entweder einen Lauf übermitteln, das Modell bereitstellen oder die Umgebung manuell registrieren. Mithilfe der Versionsverwaltung können Sie Änderungen an der Umgebung im zeitlichen Verlauf anzeigen.

Um die Version eines Python-Pakets einer vorhandenen Umgebung zu aktualisieren, geben Sie die genaue Versionsnummer für dieses Paket an. Andernfalls verwendet Azure Machine Learning die vorhandene Umgebung mit Paketversionen des Zeitpunkts wieder, an dem die Umgebung erstellt wurde.

### <a name="debug-the-image-build"></a>Debuggen der Imageerstellung

In diesem Beispiel wird die [build()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py#build-workspace-)-Methode verwendet, um eine Umgebung manuell als Docker-Image zu erstellen, und die Ausgabeprotokolle der Imageerstellung werden mithilfe von [wait_for_completion()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image(class)?view=azure-ml-py#wait-for-creation-show-output-false-) überwacht. Das erstellte Image wird dann unter der Azure Container Registry des Arbeitsbereichs angezeigt, was für das Debuggen hilfreich ist.

```python
from azureml.core import Image
build = env.build(workspace=ws)
build.wait_for_completion(show_output=True)
```

## <a name="docker-and-environments"></a>Docker und Umgebungen

 Der [DockerSection](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.dockersection?view=azure-ml-py) der `Environments`-Klasse von Azure Machine Learning ermöglicht es Ihnen, das Gastbetriebssystem, in dem Ihr Trainingslauf ausgeführt wird, im Detail anzupassen und zu steuern.

Wenn Sie Docker aktivieren (`enable`), erstellt der Dienst ein Docker-Image sowie eine Python-Umgebung mit Ihren Spezifikationen in diesem Docker-Container. Dies bietet zusätzliche Isolation und Reproduzierbarkeit für Ihre Trainingsläufe.

```python
# Creates the environment inside a Docker container.
myenv.docker.enabled = True
```

Nachdem das Docker-Image erstellt ist, wird es in der Azure Container Registry angezeigt, die dem Arbeitsbereich standard zugeordnet ist.  Der Repositoryname hat das Format *azureml/azureml_\<uuid\>* . Der Teil des eindeutigen Bezeichners (*UUID*) entspricht einem Hash, der aus der Umgebungskonfiguration berechnet wurde. Dadurch kann der Dienst ermitteln, ob ein Image, das der jeweiligen Umgebung entspricht, bereits zur Wiederverwendung vorhanden ist.

Darüber hinaus verwendet der Dienst automatisch eines der Ubuntu Linux-basierten [Basisimages](https://github.com/Azure/AzureML-Containers) und installiert die angegebenen Python-Pakete. Das Basisimage verfügt über CPU- und GPU-Versionen. Azure Machine Learning Service erkennt automatisch, welche Version verwendet werden soll.

```python
# Specify custom Docker base image and registry, if you don't want to use the defaults
myenv.docker.base_image="your_base-image"
myenv.docker.base_image_registry="your_registry_location"
```

> [!NOTE]
> Wenn Sie bei Verwendung eines benutzerdefinierten Docker-Images `environment.python.user_managed_dependencies=False` angeben, erstellt der Dienst eine Conda-Umgebung innerhalb des Images und führt die den Lauf in dieser Umgebung aus, anstatt Python-Bibliotheken zu verwenden, die Sie möglicherweise im Basisimage installiert haben. Legen Sie den Parameter auf `True` fest, um Ihre eigenen installierten Pakete zu verwenden.

## <a name="using-environments-for-training"></a>Verwenden von Umgebungen für das Training

Zum Übermitteln eines Trainingslaufs müssen Sie Ihre Umgebung, das [Computeziel](concept-compute-target.md) und das Python-Trainingsskript in einer Ausführungskonfiguration kombinieren – einem Wrapper-Objekt, das zum Übermitteln von Läufen verwendet wird.

Wenn Sie einen Trainingslauf übermitteln, kann das Erstellen einer neuen Umgebung einige Minuten dauern, je nach der Größe der erforderlichen Abhängigkeiten. Die Umgebungen werden vom Dienst zwischengespeichert. Daher fällt die vollständige Einrichtungsdauer nur einmal an, solange die Umgebungsdefinition unverändert bleibt.

Im Folgenden finden Sie ein Beispiel für die Ausführung eines lokalen Skripts, wobei Sie [ScriptRunConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.script_run_config.scriptrunconfig?view=azure-ml-py) als Wrapperobjekt verwenden würden.

```python
from azureml.core import Environment, ScriptRunConfig, Experiment

exp = Experiment(name="myexp", workspace = ws)
# Instantiate environment
myenv = Environment(name="myenv")

# Add training script to run config
runconfig = ScriptRunConfig(source_directory=".", script="train.py")

# Attach compute target to run config
runconfig.run_config.target = "local"

# Attach environment to run config
runconfig.run_config.environment = myenv

# Submit run 
run = exp.submit(runconfig)
```

> [!NOTE]
> Um den Ausführungsverlauf oder Momentaufnahmen der Ausführung zu deaktivieren, verwenden Sie die Einstellung unter `ScriptRunConfig.run_config.history`.

Wenn Sie in Ihrer Ausführungskonfiguration keine Umgebung angeben, erstellt der Dienst eine Standardumgebung für Sie, wenn Sie Ihren Lauf übermitteln.

### <a name="train-with-an-estimator"></a>Trainieren mit einem Estimator

Wenn Sie einen [Estimator](how-to-train-ml-models.md) für das Training verwenden, können Sie einfach die Estimator-Instanz direkt übermitteln, da Sie die Umgebung und das Computeziel bereits einkapselt.

Im folgenden Code wird ein Estimator für einen Trainingslauf mit einem einzelnen Knoten auf einem Remotecompute für ein scikit-learn-Modell verwendet, wobei ein zuvor erstelltes Computezielobjekt namens `compute_target` und das Datenspeicherobjekt `ds` vorausgesetzt werden.

```python
from azureml.train.estimator import Estimator

script_params = {
    '--data-folder': ds.as_mount(),
    '--regularization': 0.8
}

sk_est = Estimator(source_directory='./my-sklearn-proj',
                   script_params=script_params,
                   compute_target=compute_target,
                   entry_script='train.py',
                   conda_packages=['scikit-learn'])

# Submit the run 
run = experiment.submit(sk_est)
```

## <a name="using-environments-for-web-service-deployment"></a>Verwenden von Umgebungen für die Webdienstbereitstellung

Sie können Umgebungen verwenden, wenn Sie Ihr Modell als Webdienst bereitstellen. Dies ermöglicht einen reproduzierbaren, verbundenen Workflow, in dem Sie Ihr Modell trainieren, testen und bereitstellen können, indem Sie genau dieselben Bibliotheken sowohl in ihrem Trainings- als auch im Rückschlusscompute verwenden.

Zum Bereitstellen eines Webdiensts kombinieren Sie die Umgebung, das Rückschlusscompute, das Bewertungsskript und das registrierte Modell in Ihrem Bereitstellungs Objekt [deploy()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#deploy-workspace--name--models--inference-config--deployment-config-none--deployment-target-none-). Weitere Informationen zum [Bereitstellen von Webdiensten](how-to-deploy-and-where.md).

Nehmen Sie in diesem Beispiel an, dass Sie einen Trainingslauf abgeschlossen haben und dieses Modell in einer Azure-Containerinstanz bereitstellen möchten. Beim Erstellen des Webdiensts werden die Modell- und Bewertungsdateien in das Image eingebunden, und der Azure Machine Learning-Rückschlussstapel wird dem Image hinzugefügt.

```python
from azureml.core.model import InferenceConfig, Model
from azureml.core.webservice import AciWebservice, Webservice

# Register the model to deploy
model = run.register_model(model_name = "mymodel", model_path = "outputs/model.pkl")

# Combine scoring script & environment in Inference configuration
inference_config = InferenceConfig(entry_script="score.py", environment=myenv)

# Set deployment configuration
deployment_config = AciWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)

# Define the model, inference & deployment configuration and web service name and location to deploy
service = Model.deploy(
    workspace = ws,
    name = "my_web_service",
    models = [model],
    inference_config = inference_config,
    deployment_config = deployment_config)
```

## <a name="example-notebooks"></a>Beispielnotebooks

Dieses [Beispiel-Notebook](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training/using-environments) geht tiefer auf Konzepte und Methoden ein, die in diesem Artikel demonstriert wurden.

## <a name="next-steps"></a>Nächste Schritte

* [Tutorial: Trainieren eines Modells](tutorial-train-models-with-aml.md) verwendet ein verwaltetes Computeziel zum Trainieren eines Modells.
* Erfahren Sie nach der Erstellung eines trainierten Modells, [wie und wo Modelle bereitgestellt werden](how-to-deploy-and-where.md).
* Lesen Sie die SDK-Referenz zur [Environment-Klasse](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py).
