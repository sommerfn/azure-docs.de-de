---
author: larryfr
ms.service: machine-learning
ms.topic: include
ms.date: 11/06/2019
ms.author: larryfr
ms.openlocfilehash: ab31d45808a8c77c53b895643eec63952201d9e4
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73799849"
---
Die Einträge im `inferenceconfig.json`-Dokument werden den Parametern für die [InferenceConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py)-Klasse zugeordnet. In der folgenden Tabelle wird die Zuordnung zwischen den Entitäten im JSON-Dokument und den Parametern für die Methode beschrieben:

| JSON-Entität | Methodenparameter | BESCHREIBUNG |
| ----- | ----- | ----- |
| `entryScript` | `entry_script` | Pfad zu einer lokalen Datei, die den Code zum Ausführen für das Image enthält. |
| `runtime` | `runtime` | Die Runtime, die für das Image verwendet werden soll. Aktuelle unterstützte Runtimes sind `spark-py` und `python`. |
| `condaFile` | `conda_file` | Optional. Pfad zu einer lokalen Datei, die eine für das Image zu verwendende Conda-Umgebungsdefinition enthält. |
| `extraDockerFileSteps` | `extra_docker_file_steps` | Optional. Pfad zu einer lokalen Datei mit zusätzlichen Docker-Schritten, die ausgeführt werden sollen, wenn das Image eingerichtet wird. |
| `sourceDirectory` | `source_directory` | Optional. Pfad zu Ordnern, die alle Dateien zum Erstellen des Images enthalten. |
| `enableGpu` | `enable_gpu` | Optional. Gibt an, ob die GPU-Unterstützung im Image aktiviert werden soll. Das GPU-Image muss in einem Azure-Dienst wie Azure Container Instances, Azure Machine Learning Compute, Azure Virtual Machines und Azure Kubernetes Service verwendet werden. Die Standardeinstellung lautet „false“. |
| `baseImage` | `base_image` | Optional. Ein benutzerdefiniertes Image, das als Basisimage verwendet werden soll. Ist kein Basisimage angegeben, basiert das Image auf dem angegebenen runtime-Parameter. |
| `baseImageRegistry` | `base_image_registry` | Optional. Imageregistrierung, die das Basisimage enthält. |
| `cudaVersion` | `cuda_version` | Optional. CUDA-Version, die für Images installiert wird, die GPU-Unterstützung benötigen. Das GPU-Image muss in einem Azure-Dienst wie Azure Container Instances, Azure Machine Learning Compute, Azure Virtual Machines und Azure Kubernetes Service verwendet werden. Unterstützt werden die Versionen 9.0, 9.1 und 10.0. Ist `enable_gpu` festgelegt, lautet der Standardwert „9.1“. |
| `description` | `description` | Eine Beschreibung für das Image. |

Der folgende JSON-Code ist ein Beispiel für eine Rückschlusskonfiguration für die Verwendung mit der CLI:

```json
{
    "entryScript": "score.py",
    "runtime": "python",
    "condaFile": "myenv.yml",
    "extraDockerfileSteps": null,
    "sourceDirectory": null,
    "enableGpu": false,
    "baseImage": null,
    "baseImageRegistry": null
}
```