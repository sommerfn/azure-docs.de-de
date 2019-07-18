---
title: Wie man ein Modell mit einem benutzerdefinierten Docker-Image bereitstellt
titleSuffix: Azure Machine Learning service
description: Erfahren Sie, wie Sie bei der Bereitstellung Ihrer Azure Machine Learning Servicemodelle ein benutzerdefiniertes Docker-Image verwenden können. Bei der Bereitstellung eines trainierten Modells wird ein Docker-Image erstellt, das das Image, den Webserver und andere Komponenten enthält, die für die Ausführung des Dienstes erforderlich sind. Während der Azure Machine Learning Service ein Standardbild für Sie bereitstellt, können Sie auch Ihr eigenes Bild verwenden.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: jordane
author: jpe316
ms.reviewer: larryfr
ms.date: 07/11/2019
ms.openlocfilehash: b8591fe750d4bb1441cdc28c488b2c860eb0bccb
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67840070"
---
# <a name="deploy-a-model-using-a-custom-docker-image"></a>Bereitstellen eines Modells mithilfe eines benutzerdefinierten Docker-Images

Erfahren Sie, wie Sie ein benutzerdefiniertes Docker-Image verwenden können, wenn Sie trainierte Modelle mit dem Azure Machine Learning Service einsetzen.

Wenn Sie ein trainiertes Modell für einen Webdienst oder ein IoT Edge-Gerät bereitstellen, wird ein Docker-Image erstellt. Dieses Bild enthält das Modell, die Conda-Umgebung und die für die Verwendung des Modells erforderlichen Assets. Es enthält auch einen Webserver zur Bearbeitung eingehender Anfragen, wenn er als Webservice bereitgestellt wird, und Komponenten, die für die Arbeit mit dem Azure IoT Hub erforderlich sind.

Der Azure Machine Learning Service bietet ein Standard-Docker-Bild, so dass Sie sich keine Sorgen um die Erstellung machen müssen. Sie können auch ein benutzerdefiniertes Bild verwenden, das Sie als _Basisbild_ erstellen. Ein Basis-Image wird als Ausgangspunkt verwendet, wenn ein Image für eine Bereitstellung erstellt wird. Es stellt das zugrunde liegende Betriebssystem und die Komponenten zur Verfügung. Der Bereitstellungsprozess fügt dann dem Image zusätzliche Komponenten wie Ihr Modell, Ihre Conda-Umgebung und andere Assets hinzu, bevor es bereitgestellt wird.

In der Regel erstellen Sie ein benutzerdefiniertes Image, wenn Sie Komponentenversionen kontrollieren oder Zeit bei der Bereitstellung sparen möchten. Beispielsweise können Sie auf eine bestimmte Version von Python, Conda oder einer anderen Komponente standardisieren. Möglicherweise möchten Sie auch die von Ihrem Modell benötigte Software installieren, bei der der Installationsprozess sehr lange dauert. Die Installation der Software beim Erstellen des Basis-Images bedeutet, dass Sie diese nicht für jede Bereitstellung installieren müssen.

> [!IMPORTANT]
> Bei der Bereitstellung eines Modells können Sie Kernkomponenten wie den Webserver oder IoT Edge-Komponenten nicht überschreiben. Diese Komponenten bieten eine bekannte Arbeitsumgebung, die von Microsoft getestet und unterstützt wird.

> [!WARNING]
> Microsoft kann möglicherweise nicht helfen, Probleme zu beheben, die durch ein benutzerdefiniertes Image verursacht werden. Wenn Sie auf Probleme stoßen, werden Sie möglicherweise aufgefordert, das Standardbild oder eines der von Microsoft bereitgestellten Bilder zu verwenden, um zu sehen, ob das Problem spezifisch für Ihr Bild ist.

Dieser Artikel ist in zwei Abschnitte unterteilt:

* Erstellen eines benutzerdefinierten Images: Bietet Administratoren und DevOps Informationen zur Erstellung eines benutzerdefinierten Images und zur Konfiguration der Authentifizierung in einer Azure Container Registry unter Verwendung der Azure CLI und Machine Learning CLI.
* Verwenden eines benutzerdefinierten Images: Bietet Data Scientists und DevOps/MLOps Informationen zur Verwendung benutzerdefinierter Bilder beim Bereitstellen eines trainierten Modells aus dem Python SDK oder ML CLI.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure Machine Learning Service-Arbeitsgruppe. Weitere Informationen finden Sie unter [Erstellen eines Workspace](setup-create-workspace.md)-Artikels.
* Azure Machine Learning SDK. Weitere Informationen finden Sie im Abschnitt Python SDK des Artikels [Create a Workspace](setup-create-workspace.md#sdk).
* Die [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
* Die [CLI-Erweiterung für Azure Machine Learning](reference-azure-machine-learning-cli.md).
* Ein [Azure Container Registry](/azure/container-registry) oder andere Docker-Registrierung, die über das Internet zugänglich ist.
* Die Schritte in diesem Dokument gehen davon aus, dass Sie mit dem Erstellen und Verwenden eines __Inferenzkonfigurations__-Objekts im Rahmen der Modellbereitstellung vertraut sind. Weitere Informationen finden Sie im Abschnitt "Vorbereitung auf die Bereitstellung" unter [Wo und wie wird die Bereitstellung durchgeführt](how-to-deploy-and-where.md#prepare-to-deploy).

## <a name="create-a-custom-image"></a>Erstellen eines benutzerdefinierten Images

Die Informationen in diesem Abschnitt gehen davon aus, dass Sie eine Azure Container Registry verwenden, um Docker-Bilder zu speichern. Verwenden Sie die folgende Checkliste, wenn Sie planen, benutzerdefinierte Bilder für den Azure Machine Learning Service zu erstellen:

* Werden Sie die Azure Container Registry, die für den Azure Machine Learning Service Workspace erstellt wurde, oder eine eigenständige Azure Container Registry verwenden?

    Wenn Sie Bilder verwenden, die in der __Container-Registrierung für den Arbeitsbereich__ gespeichert sind, müssen Sie sich nicht bei der Registrierung authentifizieren. Authentifizierung erfolgt über den Arbeitsbereich.

    > [!WARNING]
    > Die Azure Container Rzegistry für Ihren Arbeitsbereich wird __erstellt, wenn Sie zum ersten Mal ein Modell__ über den Arbeitsbereich trainieren oder bereitstellen. Wenn Sie einen neuen Arbeitsbereich erstellt, aber kein Training oder Modell erstellt haben, existiert für den Arbeitsbereich keine Azure Container Registry.

    Informationen zum Abrufen des Namens der Azure Container Registry für Ihren Arbeitsbereich finden Sie im Abschnitt [Get container registry name](#getname) in diesem Artikel.

    Wenn Sie Bilder verwenden, die in einer __eigenständigen Containerregistrierung__ gespeichert sind, müssen Sie ein Service-Prinzip konfigurieren, das mindestens Leserechte hat. Sie geben dann die Haupt-ID (Benutzername) und das Passwort an jeden weiter, der Bilder aus der Registrierung verwendet. Die Ausnahme ist, wenn Sie die Container-Registry öffentlich zugänglich machen.

    Informationen zum Erstellen einer privaten Azure Container Registry finden Sie unter [Erstellen einer privaten Container Registry](/azure/container-registry/container-registry-get-started-azure-cli).

    Informationen zur Verwendung von Service-Prinzipien mit der Azure Container Registry finden Sie unter [Authentifizierung der Azure Container Registry mit Service-Prinzipien](/azure/container-registry/container-registry-auth-service-principal).

* Azure Container Registry und Bildinformationen: Geben Sie den Bildnamen an jeden weiter, der ihn verwenden muss. ZBeispielsweise wird ein Image namens `myimage`, das in einer Registry namens `myregistry`, als `myregistry.azurecr.io/myimage` referenziert, wenn das Image für die Modellbereitstellung verwendet wird

* Anforderungen an Images: Der Azure Machine Learning Service unterstützt nur Docker-Bilder, die die folgende Software enthalten:

    * Ubuntu 16.04 oder höher.
    * Conda 4.5.# oder höher.
    * Python 3.5# oder 3.6#.

<a id="getname"></a>

### <a name="get-container-registry-information"></a>Abrufen von Informationen aus der Container Registry

In diesem Abschnitt erfahren Sie, wie Sie den Namen der Azure Container Registry für Ihren Azure Machine Learning Service-Arbeitsbereich erhalten.

> [!WARNING]
> Die Azure Container Registry für Ihren Arbeitsbereich wird __erstellt, wenn Sie ein Modell zum ersten Mal über den Arbeitsbereich trainieren oder bereitstellen__. Wenn Sie einen neuen Arbeitsbereich erstellt, aber kein Training oder Modell erstellt haben, existiert für den Arbeitsbereich keine Azure Container Registry.

Wenn Sie bereits Modelle mit dem Azure Machine Learning-Dienst trainiert oder bereitgestellt haben, wurde eine Container-Registry für Ihren Arbeitsbereich erstellt. Um den Namen dieser Container-Registrierung zu finden, führen Sie die folgenden Schritte aus:

1. Öffnen Sie eine neue Shell oder Eingabeaufforderung und verwenden Sie den folgenden Befehl, um sich bei Ihrem Azure-Abonnement zu authentifizieren:

    ```azurecli-interactive
    az login
    ```

    Folgen Sie den Anweisungen, um sich am Abonnement zu authentifizieren.

2. Verwenden Sie den folgenden Befehl, um die Container-Registry für den Arbeitsbereich aufzulisten. Ersetzen Sie `<myworkspace>` durch Ihren Azure Machine Learning Service-Arbeitsbereich. Ersetzen Sie `<resourcegroup>` durch die Azure-Ressourcengruppe, die Ihren Arbeitsbereich enthält:

    ```azurecli-interactive
    az ml workspace show -w <myworkspace> -g <resourcegroup> --query containerRegistry
    ```

    Die zurückgegebenen Informationen ähneln dem folgenden Text:

    ```text
    /subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.ContainerRegistry/registries/<registry_name>
    ```

    Die `<registry_name>` Wert ist der Name des Azure Container Registry für Ihren Arbeitsbereich.

### <a name="build-a-custom-image"></a>Verwenden eines benutzerdefinierten Bildes

Die Schritte in diesem Abschnitt führen zur Erstellung eines benutzerdefinierten Docker-Bildes in Ihrer Azure Container Registry.

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

2. Führen Sie von einer Shell oder Eingabeaufforderung aus die folgenden Schritte durch, um sich bei der Azure Container Registry zu authentifizieren. Ersetzen Sie das `<registry_name>` durch den Namen der Container-Registry, in der Sie das Bild speichern möchten:

    ```azurecli-interactive
    az acr login --name <registry_name>
    ```

3. Um die Dockerdatei hochzuladen und zu erstellen, verwenden Sie den folgenden Befehl. Ersetzen Sie `<registry_name>` durch den Namen der Container-Registry, in der Sie das Bild speichern möchten:

    ```azurecli-interactive
    az acr build --image myimage:v1 --registry <registry_name> --file Dockerfile .
    ```

    Während des Erstellungsprozesses werden Informationen gestreamt, um sie an die Befehlszeile zurückzugeben. Wenn die Erstellung erfolgreich ist, erhalten Sie eine Meldung ähnlich dem folgenden Text:

    ```text
    Run ID: cda was successful after 2m56s
    ```

Weitere Informationen zum Erstellen von Images mit einer Azure Container Registry finden Sie unter [Erstellen und Ausführen eines Container-Images mit Azure Container Registry Tasks](https://docs.microsoft.com/azure/container-registry/container-registry-quickstart-task-cli)

Weitere Informationen zum Hochladen vorhandener Bilder in eine Azure Container Registry finden Sie unter [Senden Sie ihr erstes Bild an eine private Docker Container Registry](/azure/container-registry/container-registry-get-started-docker-cli).

## <a name="use-a-custom-image"></a>Verwenden eines benutzerdefinierten Images

Um ein benutzerdefiniertes Bild zu verwenden, benötigen Sie die folgenden Informationen:

* Der __Bildname__. Zum Beispiel ist `mcr.microsoft.com/azureml/o16n-sample-user-base/ubuntu-miniconda` der Pfad zu einem einfachen Docker-Bild von Microsoft.
* Wenn sich das Bild in einem __privaten Repository__ befindet, benötigen Sie die folgenden Informationen:

    * Die Registrierung __Adresse__. Beispiel: `myregistry.azureecr.io`.
    * Ein __Benutzername__ und ein __Passwort__ für den Dienstprinzipal, der Lesezugriff auf die Registrierung hat.

    Wenn Sie diese Informationen nicht haben, wenden Sie sich an den Administrator der Azure Container Registry, die Ihr Bild enthält.

### <a name="publicly-available-images"></a>Öffentlich verfügbare Bilder

Microsoft stellt mehrere Docker-Bilder auf einem öffentlich zugänglichen Repository zur Verfügung, die mit dem Anwenden der Schritte in diesem Abschnitt verwendet werden können:

| Image | BESCHREIBUNG |
| ----- | ----- |
| `mcr.microsoft.com/azureml/o16n-sample-user-base/ubuntu-miniconda` | Basisbild für den Azure Machine Learning Service |
| `mcr.microsoft.com/azureml/onnxruntime:v0.4.0` | Enthält die ONNX-Laufzeit. |
| `mcr.microsoft.com/azureml/onnxruntime:v0.4.0-cuda10.0-cudnn7` | Enthält die ONNX-Runtime und die CUDA-Komponenten. |
| `mcr.microsoft.com/azureml/onnxruntime:v0.4.0-tensorrt19.03` | Enthält ONNX-Laufzeit und TensorRT. |

> [!TIP]
> Da diese Bilder öffentlich zugänglich sind, müssen Sie bei der Verwendung keine Adresse, Benutzernamen oder Passwort angeben.

> [!IMPORTANT]
> Microsoft-Images, die CUDA oder TensorRT verwenden, dürfen nur für Microsoft Azure-Dienste verwendet werden.

> [!TIP]
>__Wenn Ihr Modell auf Azure Machine Learning Compute__ mit __Version 1.0.22 oder höher__ des Azure Machine Learning SDK trainiert wird, wird während des Trainings ein Bild erstellt. Um den Namen dieses Bildes zu ermitteln, verwenden Sie `run.properties["AzureML.DerivedImageName"]`. Die Verwendung dieses Images wird im folgenden Beispiel veranschaulicht:
>
> ```python
> # Use an image built during training with SDK 1.0.22 or greater
> image_config.base_image = run.properties["AzureML.DerivedImageName"]
> ```

### <a name="use-an-image-with-the-azure-machine-learning-sdk"></a>Verwenden Sie ein Bild mit dem Azure Machine Learning SDK

Um ein benutzerdefiniertes Bild zu verwenden, setzen Sie die `base_image` Eigenschaft [des Inferenz-Konfigurationsobjekts](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py) auf die Adresse des Bildes:

```python
# use an image from a registry named 'myregistry'
inference_config.base_image = "myregistry.azurecr.io/myimage:v1"
```

Dieses Format funktioniert sowohl für Bilder, die in der Azure Container Registry für Ihren Arbeitsbereich gespeichert sind, als auch für Containerregistrierungen, die öffentlich zugänglich sind. Der folgende Code verwendet beispielsweise ein von Microsoft bereitgestelltes Standardbild:

```python
# use an image available in public Container Registry without authentication
inference_config.base_image = "mcr.microsoft.com/azureml/o16n-sample-user-base/ubuntu-miniconda"
```

Um ein Bild aus einer __privaten Container-Registry__ zu verwenden, das sich nicht in Ihrem Arbeitsbereich befindet, müssen Sie die Adresse des Repositories sowie einen Benutzernamen und ein Passwort angeben:

```python
# Use an image available in a private Container Registry
inference_config.base_image = "myregistry.azurecr.io/mycustomimage:1.0"
inference_config.base_image_registry.address = "myregistry.azurecr.io"
inference_config.base_image_registry.username = "username"
inference_config.base_image_registry.password = "password"
```

### <a name="use-an-image-with-the-machine-learning-cli"></a>Verwenden Sie ein Bild mit dem Azure Machine Learning CLI

> [!IMPORTANT]
> Derzeit kann das Machine Learning CLI Bilder aus dem Azure Container Registry für Ihren Arbeitsbereich oder öffentlich zugängliche Repositorien verwenden. Es können keine Bilder aus eigenständigen privaten Registrierungsstellen verwendet werden.

Wenn Sie ein Modell mit dem Machine Learning CLI bereitstellen, stellen Sie eine Inferenz-Konfigurationsdatei zur Verfügung, die auf das benutzerdefinierte Bild verweist. Das folgende JSON-Dokument zeigt, wie man ein Bild in einer öffentlichen Container-Registry referenziert:

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

Diese Datei wird mit dem `az ml model deploy` Befehl verwendet. Der `--ic`-Parameter wird verwendet, um die Inferenz-Konfigurationsdatei anzugeben.

```azurecli
az ml model deploy -n myservice -m mymodel:1 --ic inferenceconfig.json --dc deploymentconfig.json --ct akscomputetarget
```

Weitere Informationen zur Bereitstellung eines Modells mit dem ML CLI finden Sie im Abschnitt "Modellregistrierung, Profilerstellung und Bereitstellung" der [CLI-Erweiterung für den Serviceartikel Azure Machine Learning](reference-azure-machine-learning-cli.md#model-registration-profiling-deployment).

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr darüber, [wo und wie Sie es bereitstellen können](how-to-deploy-and-where.md).
* Erfahren Sie, wie Sie [maschinelle Lernmodelle mit Hilfe von Azure Pipelines trainieren und einsetzen können](/azure/devops/pipelines/targets/azure-machine-learning?view=azure-devops).
