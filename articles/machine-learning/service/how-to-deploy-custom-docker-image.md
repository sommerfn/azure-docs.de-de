---
title: Bereitstellen von Modellen mit einem benutzerdefinierten Docker-Basisimage
titleSuffix: Azure Machine Learning service
description: Erfahren Sie, wie Sie bei der Bereitstellung Ihrer Azure Machine Learning Service-Modelle ein benutzerdefiniertes Docker-Basisimage verwenden können. Beim Bereitstellen eines trainierten Modells wird ein Basiscontainerimage bereitgestellt, um Ihr Modell für einen Rückschluss auszuführen. Azure Machine Learning Service stellt ein Standardbasisimage für Sie bereit, Sie können aber auch Ihr eigenes Basisimage verwenden.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: jordane
author: jpe316
ms.reviewer: larryfr
ms.date: 08/22/2019
ms.openlocfilehash: a86dd021d8f9cfe275b3af3f0cb71b99857c26d7
ms.sourcegitcommit: 47b00a15ef112c8b513046c668a33e20fd3b3119
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2019
ms.locfileid: "69971517"
---
# <a name="deploy-a-model-using-a-custom-docker-base-image"></a>Bereitstellen eines Modells mithilfe eines benutzerdefinierten Docker-Basisimages

Erfahren Sie, wie Sie ein benutzerdefiniertes Docker-Basisimage verwenden können, wenn Sie trainierte Modelle mit dem Azure Machine Learning Service einsetzen.

Wenn Sie ein trainiertes Modell für einen Webdienst oder ein IoT Edge-Gerät bereitstellen, wird ein Paket erstellt, das einen Webserver enthält, der eingehende Anforderungen verarbeitet.

Azure Machine Learning Service stellt ein standardmäßiges Docker-Basisimage bereit, sodass es nicht erforderlich ist, dass Sie ein solches Image erstellen. Sie können aber auch ein benutzerdefiniertes Image verwenden, das Sie als _Basisimage_ erstellen. 

Ein Basisimage wird als Ausgangspunkt verwendet, wenn ein Image für eine Bereitstellung erstellt wird. Es stellt das zugrunde liegende Betriebssystem und die Komponenten zur Verfügung. Der Bereitstellungsprozess fügt dann dem Image zusätzliche Komponenten wie Ihr Modell, Ihre Conda-Umgebung und andere Objekte hinzu, bevor es bereitgestellt wird.

In der Regel erstellen Sie ein benutzerdefiniertes Basisimage, wenn Sie Docker dazu verwenden möchten, Ihre Abhängigkeiten zu verwalten, eine strengere Kontrolle über Komponentenversionen zu behalten oder während der Bereitstellung Zeit zu sparen. Beispielsweise können Sie auf eine bestimmte Version von Python, Conda oder einer anderen Komponente standardisieren. Möglicherweise möchten Sie auch die von Ihrem Modell benötigte Software installieren, bei der der Installationsprozess sehr lange dauert. Die Installation der Software beim Erstellen des Basisimages bedeutet, dass Sie diese nicht für jede Bereitstellung installieren müssen.

> [!IMPORTANT]
> Bei der Bereitstellung eines Modells können Sie Kernkomponenten wie den Webserver oder IoT Edge-Komponenten nicht überschreiben. Diese Komponenten bieten eine bekannte Arbeitsumgebung, die von Microsoft getestet und unterstützt wird.

> [!WARNING]
> Microsoft kann möglicherweise nicht helfen, Probleme zu beheben, die durch ein benutzerdefiniertes Image verursacht werden. Wenn Sie auf Probleme stoßen, werden Sie möglicherweise aufgefordert, das Standardimage oder eines der von Microsoft bereitgestellten Images zu verwenden, um zu sehen, ob das Problem von Ihrem Image verursacht wird.

Dieser Artikel ist in zwei Abschnitte unterteilt:

* Erstellen eines benutzerdefinierten Basisimages: Bietet Administratoren und DevOps Informationen zur Erstellung eines benutzerdefinierten Images und zur Konfiguration der Authentifizierung in einer Azure Container Registry unter Verwendung der Azure CLI und Machine Learning CLI.
* Bereitstellen eines Modells mithilfe eines benutzerdefinierten Basisimages: Bietet Data Scientists und DevOps/ML-Technikern Informationen zur Verwendung benutzerdefinierter Images, wenn ein trainiertes Modell aus dem Python SDK oder der ML-CLI bereitgestellt wird.

## <a name="prerequisites"></a>Voraussetzungen

* Eine Azure Machine Learning Service-Arbeitsgruppe. Weitere Informationen finden Sie im Artikel [Erstellen eines Workspace](how-to-manage-workspace.md).
* Das [Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py). 
* Die [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
* Die [CLI-Erweiterung für Azure Machine Learning](reference-azure-machine-learning-cli.md).
* Ein [Azure Container Registry](/azure/container-registry) oder eine andere Docker-Registrierung, die über das Internet zugänglich ist.
* Die Schritte in diesem Dokument gehen davon aus, dass Sie mit dem Erstellen und Verwenden eines __Rückschlusskonfiguration__-Objekts im Rahmen der Modellimplementierung vertraut sind. Weitere Informationen finden Sie im Abschnitt „Vorbereitung auf die Bereitstellung“ unter [Wo und wie wird die Bereitstellung durchgeführt](how-to-deploy-and-where.md#prepare-to-deploy).

## <a name="create-a-custom-base-image"></a>Erstellen eines benutzerdefinierten Basisimages

Die Informationen in diesem Abschnitt gehen davon aus, dass Sie eine Azure Container Registry verwenden, um Docker-Images zu speichern. Verwenden Sie die folgende Checkliste, wenn Sie planen, benutzerdefinierte Images für den Azure Machine Learning Service zu erstellen:

* Werden Sie die Azure Container Registry, die für den Azure Machine Learning Service-Arbeitsbereich erstellt wurde, oder eine eigenständige Azure Container Registry verwenden?

    Wenn Sie Images verwenden, die in der __Container-Registrierung für den Arbeitsbereich__ gespeichert sind, müssen Sie sich nicht bei der Registrierung authentifizieren. Authentifizierung erfolgt über den Arbeitsbereich.

    > [!WARNING]
    > Die Azure Container Registry für Ihren Arbeitsbereich wird __erstellt, wenn Sie zum ersten Mal ein Modell__ über den Arbeitsbereich trainieren oder bereitstellen. Wenn Sie einen neuen Arbeitsbereich, aber kein Training oder Modell erstellt haben, existiert für den Arbeitsbereich keine Azure Container Registry.

    Informationen zum Abrufen des Namens der Azure Container Registry für Ihren Arbeitsbereich finden Sie im Abschnitt [Abrufen von Informationen aus der Container Registry](#getname) in diesem Artikel.

    Wenn Sie Images verwenden, die in einer __eigenständigen Containerregistrierung__ gespeichert sind, müssen Sie einen Dienstprinzipal konfigurieren, der mindestens Leserechte hat. Sie geben dann die Dienstprinzipal-ID (Benutzername) und das Kennwort an jeden weiter, der Images aus der Registrierung verwendet. Die Ausnahme ist, wenn Sie die Containerregistrierung öffentlich zugänglich machen.

    Informationen zum Erstellen einer privaten Azure Container Registry finden Sie unter [Erstellen einer privaten Containerregistrierung](/azure/container-registry/container-registry-get-started-azure-cli).

    Informationen zur Verwendung von Dienstprinzipalen mit der Azure Container Registry finden Sie unter [Authentifizierung der Azure Container Registry mit Dienstprinzipalen](/azure/container-registry/container-registry-auth-service-principal).

* Azure Container Registry und Imageinformationen: Geben Sie den Imagenamen an jeden weiter, der ihn verwenden muss. Beispielsweise wird ein Image namens `myimage`, das in einer Registrierung namens `myregistry` gespeichert ist, als `myregistry.azurecr.io/myimage` referenziert, wenn das Image für die Modellimplementierung verwendet wird.

* Anforderungen an Images: Azure Machine Learning Service unterstützt nur Docker-Images, die die folgende Software enthalten:

    * Ubuntu 16.04 oder höher.
    * Conda 4.5.# oder höher.
    * Python 3.5# oder 3.6#.

<a id="getname"></a>

### <a name="get-container-registry-information"></a>Abrufen von Informationen aus der Container Registry

In diesem Abschnitt erfahren Sie, wie Sie den Namen der Azure Container Registry für Ihren Azure Machine Learning Service-Arbeitsbereich erhalten.

> [!WARNING]
> Die Azure Container Registry für Ihren Arbeitsbereich wird __erstellt, wenn Sie ein Modell zum ersten Mal über den Arbeitsbereich trainieren oder bereitstellen__. Wenn Sie einen neuen Arbeitsbereich, aber kein Training oder Modell erstellt haben, existiert für den Arbeitsbereich keine Azure Container Registry.

Wenn Sie bereits Modelle mit dem Azure Machine Learning Service trainiert oder bereitgestellt haben, wurde eine Containerregistrierung (Container Registry) für Ihren Arbeitsbereich erstellt. Um den Namen dieser Containerregistrierung zu finden, führen Sie die folgenden Schritte aus:

1. Öffnen Sie eine neue Shell oder Eingabeaufforderung, und verwenden Sie den folgenden Befehl, um sich bei Ihrem Azure-Abonnement zu authentifizieren:

    ```azurecli-interactive
    az login
    ```

    Folgen Sie den Anweisungen, um sich beim Abonnement zu authentifizieren.

2. Verwenden Sie den folgenden Befehl, um die Containerregistrierung für den Arbeitsbereich aufzulisten. Ersetzen Sie `<myworkspace>` durch Ihren Azure Machine Learning Service-Arbeitsbereich. Ersetzen Sie `<resourcegroup>` durch die Azure-Ressourcengruppe, die Ihren Arbeitsbereich enthält:

    ```azurecli-interactive
    az ml workspace show -w <myworkspace> -g <resourcegroup> --query containerRegistry
    ```

    [!INCLUDE [install extension](../../../includes/machine-learning-service-install-extension.md)]

    Die zurückgegebenen Informationen ähneln dem folgenden Text:

    ```text
    /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.ContainerRegistry/registries/<registry_name>
    ```

    Der `<registry_name>`-Wert ist der Name der Azure Container Registry für Ihren Arbeitsbereich.

### <a name="build-a-custom-base-image"></a>Erstellen eines benutzerdefinierten Basisimages

Die Schritte in diesem Abschnitt führen zur Erstellung eines benutzerdefinierten Docker-Images in Ihrer Azure Container Registry.

1. Erstellen Sie eine neue Textdatei mit dem Namen `Dockerfile`, und verwenden Sie den folgenden Text als Inhalt:

    ```text
    FROM ubuntu:16.04

    ARG CONDA_VERSION=4.5.12
    ARG PYTHON_VERSION=3.6

    ENV LANG=C.UTF-8 LC_ALL=C.UTF-8
    ENV PATH /opt/miniconda/bin:$PATH

    RUN apt-get update --fix-missing && \
        apt-get install -y wget bzip2 && \
        apt-get clean && \
        rm -rf /var/lib/apt/lists/*

    RUN wget --quiet https://repo.anaconda.com/miniconda/Miniconda3-${CONDA_VERSION}-Linux-x86_64.sh -O ~/miniconda.sh && \
        /bin/bash ~/miniconda.sh -b -p /opt/miniconda && \
        rm ~/miniconda.sh && \
        /opt/miniconda/bin/conda clean -tipsy

    RUN conda install -y conda=${CONDA_VERSION} python=${PYTHON_VERSION} && \
        conda clean -aqy && \
        rm -rf /opt/miniconda/pkgs && \
        find / -type d -name __pycache__ -prune -exec rm -rf {} \;
    ```

2. Führen Sie von einer Shell oder Eingabeaufforderung aus die folgenden Schritte durch, um sich bei der Azure Container Registry zu authentifizieren. Ersetzen Sie das `<registry_name>` durch den Namen der Containerregistrierung, in der Sie das Image speichern möchten:

    ```azurecli-interactive
    az acr login --name <registry_name>
    ```

3. Um das Dockerfile hochzuladen und zu erstellen, verwenden Sie den folgenden Befehl. Ersetzen Sie `<registry_name>` durch den Namen der Containerregistrierung, in der Sie das Image speichern möchten:

    ```azurecli-interactive
    az acr build --image myimage:v1 --registry <registry_name> --file Dockerfile .
    ```

    Während des Erstellungsprozesses werden Informationen gestreamt, um sie an die Befehlszeile zurückzugeben. Wenn die Erstellung erfolgreich war, erhalten Sie eine Meldung ähnlich dem folgenden Text:

    ```text
    Run ID: cda was successful after 2m56s
    ```

Weitere Informationen zum Erstellen von Images mit einer Azure Container Registry finden Sie unter [Erstellen und Ausführen eines Containerimages mit Azure Container Registry Tasks](https://docs.microsoft.com/azure/container-registry/container-registry-quickstart-task-cli).

Weitere Informationen zum Hochladen vorhandener Images in eine Azure Container Registry finden Sie unter [Senden Sie ihr erstes Bild an eine private Docker-Containerregistrierung](/azure/container-registry/container-registry-get-started-docker-cli).

## <a name="use-a-custom-base-image"></a>Verwenden eines benutzerdefinierten Basisimages

Um ein benutzerdefiniertes Image zu verwenden, benötigen Sie die folgenden Informationen:

* Der __Imagename__. Zum Beispiel ist `mcr.microsoft.com/azureml/o16n-sample-user-base/ubuntu-miniconda` der Pfad zu einem einfachen Docker-Image, das von Microsoft bereitgestellt wird.
* Wenn sich das Image in einem __privaten Repository__ befindet, benötigen Sie die folgenden Informationen:

    * Die __Registrierungsadresse__. Beispiel: `myregistry.azureecr.io`.
    * Ein __Benutzername__ und ein __Kennwort__ für den Dienstprinzipal, der Lesezugriff auf die Registrierung hat.

    Wenn Sie diese Informationen nicht haben, wenden Sie sich an den Administrator der Azure Container Registry, die Ihr Image enthält.

### <a name="publicly-available-base-images"></a>Öffentlich verfügbare Basisimages

Microsoft stellt mehrere Docker-Images in einem öffentlich zugänglichen Repository zur Verfügung, die in den Schritten in diesem Abschnitt verwendet werden können:

| Image | BESCHREIBUNG |
| ----- | ----- |
| `mcr.microsoft.com/azureml/o16n-sample-user-base/ubuntu-miniconda` | Basisimage für den Azure Machine Learning Service |
| `mcr.microsoft.com/azureml/onnxruntime:v0.4.0` | Enthält die ONNX-Runtime. |
| `mcr.microsoft.com/azureml/onnxruntime:v0.4.0-cuda10.0-cudnn7` | Enthält die ONNX-Runtime und die CUDA-Komponenten. |
| `mcr.microsoft.com/azureml/onnxruntime:v0.4.0-tensorrt19.03` | Enthält die ONNX-Runtime und TensorRT. |

> [!TIP]
> Da diese Images öffentlich zugänglich sind, müssen Sie bei der Verwendung keine Adresse, Benutzernamen oder Kennwort angeben.

> [!IMPORTANT]
> Microsoft-Images, die CUDA oder TensorRT verwenden, dürfen nur für Microsoft Azure-Dienste verwendet werden.

> [!TIP]
>__Wenn Ihr Modell auf Azure Machine Learning Compute__ mit __Version 1.0.22 oder höher__ des Azure Machine Learning SDK trainiert wird, wird während des Trainings ein Image erstellt. Um den Namen dieses Images zu ermitteln, verwenden Sie `run.properties["AzureML.DerivedImageName"]`. Die Verwendung dieses Images wird im folgenden Beispiel veranschaulicht:
>
> ```python
> # Use an image built during training with SDK 1.0.22 or greater
> image_config.base_image = run.properties["AzureML.DerivedImageName"]
> ```

### <a name="use-an-image-with-the-azure-machine-learning-sdk"></a>Verwenden eines Images mit dem Azure Machine Learning SDK

Um ein benutzerdefiniertes Image zu verwenden, legen Sie die `base_image`-Eigenschaft des [Rückschlusskonfigurationsobjekts](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py) auf die Adresse des Images fest:

```python
# use an image from a registry named 'myregistry'
inference_config.base_image = "myregistry.azurecr.io/myimage:v1"
```

Dieses Format funktioniert sowohl für Images, die in der Azure Container Registry für Ihren Arbeitsbereich gespeichert sind, als auch für Containerregistrierungen, die öffentlich zugänglich sind. Der folgende Code verwendet beispielsweise ein von Microsoft bereitgestelltes Standardimage:

```python
# use an image available in public Container Registry without authentication
inference_config.base_image = "mcr.microsoft.com/azureml/o16n-sample-user-base/ubuntu-miniconda"
```

Um ein Image aus einer __privaten Containerregistrierung__ zu verwenden, die sich nicht in Ihrem Arbeitsbereich befindet, müssen Sie die Adresse des Repositorys sowie einen Benutzernamen und ein Kennwort angeben:

```python
# Use an image available in a private Container Registry
inference_config.base_image = "myregistry.azurecr.io/mycustomimage:1.0"
inference_config.base_image_registry.address = "myregistry.azurecr.io"
inference_config.base_image_registry.username = "username"
inference_config.base_image_registry.password = "password"
```

### <a name="use-an-image-with-the-machine-learning-cli"></a>Verwenden eines Images mit der Machine Learning-CLI

> [!IMPORTANT]
> Derzeit kann die Machine Learning-CLI Images aus der Azure Container Registry für Ihren Arbeitsbereich oder aus öffentlich zugänglichen Repositorys verwenden. Sie kann keine Images aus eigenständigen privaten Registrierungen verwenden.

Wenn Sie ein Modell über die Machine Learning-CLI bereitstellen, stellen Sie eine Rückschlusskonfigurationsdatei zur Verfügung, die auf das benutzerdefinierte Image verweist. Das folgende JSON-Dokument zeigt, wie auf ein Image in einer öffentlichen Containerregistrierung verwiesen wird:

```json
{
   "entryScript": "score.py",
   "runtime": "python",
   "condaFile": "infenv.yml",
   "extraDockerfileSteps": null,
   "sourceDirectory": null,
   "enableGpu": false,
   "baseImage": "mcr.microsoft.com/azureml/o16n-sample-user-base/ubuntu-miniconda",
   "baseImageRegistry": "mcr.microsoft.com"
}
```

Diese Datei wird mit dem `az ml model deploy`-Befehl verwendet. Der `--ic`-Parameter wird verwendet, um die Rückschlusskonfigurationsdatei anzugeben.

```azurecli
az ml model deploy -n myservice -m mymodel:1 --ic inferenceconfig.json --dc deploymentconfig.json --ct akscomputetarget
```

Weitere Informationen zur Bereitstellung eines Modells mit der ML-CLI finden Sie im Abschnitt „Modellregistrierung, Profilerstellung und Bereitstellung“ des Artikels [Verwenden der CLI-Erweiterung für Azure Machine Learning Service](reference-azure-machine-learning-cli.md#model-registration-profiling-deployment).

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr darüber, [Wie und wo Modelle bereitgestellt werden](how-to-deploy-and-where.md).
* Erfahren Sie, wie ein [Trainieren und Bereitstellen von Machine Learning-Modellen mit Azure Pipelines](/azure/devops/pipelines/targets/azure-machine-learning?view=azure-devops) erfolgt.
