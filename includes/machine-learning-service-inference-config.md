---
author: larryfr
ms.service: machine-learning
ms.topic: include
ms.date: 07/19/2019
ms.author: larryfr
ms.openlocfilehash: 03b40b1f334816f08d7e169f8ff78bdb7c06c4de
ms.sourcegitcommit: e72073911f7635cdae6b75066b0a88ce00b9053b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/19/2019
ms.locfileid: "68348488"
---
Die Einträge im `inferenceconfig.json`-Dokument werden den Parametern für die [InferenceConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py)-Klasse zugeordnet. In der folgenden Tabelle wird die Zuordnung zwischen den Entitäten im JSON-Dokument und den Parametern für die Methode beschrieben:

| JSON-Entität | Methodenparameter | BESCHREIBUNG |
| ----- | ----- | ----- |
| `entryScript` | `entry_script` | Pfad zur lokalen Datei mit dem Code zur Ausführung des Images. |
| `runtime` | `runtime` | Die Runtime, die für das Image verwendet werden soll. Aktuelle unterstützte Runtimes sind „spark-py“ und „python“. |
| `condaFile` | `conda_file` | Optional. Pfad zur lokalen Datei, die eine für das Image zu verwendende Conda-Umgebungsdefinition enthält. |
| `extraDockerFileSteps` | `extra_docker_file_steps` | Optional. Pfad zur lokalen Datei mit zusätzlichen Docker-Schritten, die bei der Einrichtung des Images ausgeführt werden. |
| `sourceDirectory` | `source_directory` | Optional. Pfad zu Ordnern, die alle Dateien zum Erstellen des Images enthalten. |
| `enableGpu` | `enable_gpu` | Optional. Gibt an, ob die GPU-Unterstützung im Image aktiviert werden soll. Das GPU-Image muss für Microsoft Azure-Dienste verwendet werden. Beispielsweise Azure Container Instances, Azure Machine Learning Compute, Azure Virtual Machines und Azure Kubernetes Service. Der Standardwert lautet „False“. |
| `baseImage` | `base_image` | Optional. Ein benutzerdefiniertes Image, das als Basisimage verwendet werden soll. Wenn kein Basisimage angegeben wird, dann wird das Basisimage basierend auf dem angegebenen Runtimeparameter verwendet. |
| `baseImageRegistry` | `base_image_registry` | Optional. Imageregistrierung, die das Basisimage enthält. |
| `cudaVersion` | `cuda_version` | Optional. CUDA-Version, die für Images installiert wird, die GPU-Unterstützung benötigen. Das GPU-Image muss für Microsoft Azure-Dienste verwendet werden. Beispielsweise Azure Container Instances, Azure Machine Learning Compute, Azure Virtual Machines und Azure Kubernetes Service. Unterstützt werden die Versionen 9.0, 9.1 und 10.0. Wenn „enable_gpu“ festgelegt ist, wird standardmäßig „9.1“ unterstützt. |
| `description` | `description` |  Die Beschreibung dieses Images. |

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