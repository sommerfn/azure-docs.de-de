---
title: Git-Integration für Azure Machine Learning
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Azure Machine Learning in ein lokales Git-Repository integriert werden kann.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: jordane
author: jpe316
ms.date: 10/11/2019
ms.openlocfilehash: db96663ef3d901546e1b32362a9eb9c9ae09dd21
ms.sourcegitcommit: 0576bcb894031eb9e7ddb919e241e2e3c42f291d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72377375"
---
# <a name="git-integration-for-azure-machine-learning"></a>Git-Integration für Azure Machine Learning

[Git-](https://git-scm.com/) ist ein beliebtes Versionskontrollsystem, mit dem Sie Projekte freigeben und gemeinsam mit anderen an ihnen arbeiten können. Wenn beim Übermitteln eines Trainingsauftrags an Azure Machine Learning die Trainingsdateien in einem lokalen Git-Repository gespeichert sind, werden Informationen über das Repository als Teil des Trainingsprozesses nachverfolgt.

Da Azure Machine Learning Informationen aus einem lokalen Git-Repository nachverfolgt, ist es nicht an ein spezifisches zentrales Repository gebunden. Ihr Repository kann von GitHub, GitLab, Bitbucket, Azure DevOps oder einem beliebigen anderen, mit Git kompatiblen Dienst geklont sein.

## <a name="how-does-git-integration-work"></a>Wie funktioniert die Git-Integration?

Wenn Sie einen Trainingslauf vom Python SDK oder der Machine Learning-CLI übermitteln, werden die zum Trainieren des Modells erforderlichen Dateien in Ihren Arbeitsbereich hochgeladen. Wenn der `git`-Befehl in Ihrer Entwicklungsumgebung verfügbar ist, wird er vom Uploadvorgang verwendet, um zu überprüfen, ob die Dateien in einem Git-Repository gespeichert sind. Ist dies der Fall, werden die Informationen aus Ihrem Git-Repository ebenfalls als Bestandteil des Trainingslaufs hochgeladen. Diese Informationen sind in den folgenden Eigenschaften für den Trainingslauf gespeichert:

| Eigenschaft | BESCHREIBUNG |
| ----- | ----- |
| `azureml.git.repository_uri` | Der URI, von dem Ihr Repository geklont wurde. |
| `mlflow.source.git.repoURL` | Der URI, von dem Ihr Repository geklont wurde. |
| `azureml.git.branch` | Der aktive Branch bei der Übermittlung des Laufs. |
| `mlflow.source.git.branch` | Der aktive Branch bei der Übermittlung des Laufs. |
| `azureml.git.commit` | Der Commithash des Codes, der für den Lauf übermittelt wurde. |
| `mlflow.source.git.commit` | Der Commithash des Codes, der für den Lauf übermittelt wurde. |
| `azureml.git.dirty` | `True`, wenn der Commit geändert wurde („dirty“), andernfalls `false`. |

Diese Informationen werden für Läufe gesendet, die einen Estimator, eine Machine Learning-Pipeline oder eine Skriptausführung verwenden.

Wenn Ihre Trainingsdateien nicht in einem Git-Repository in Ihrer Entwicklungsumgebung gespeichert sind oder der `git`-Befehl nicht verfügbar ist, werden keine Git-bezogenen Informationen nachverfolgt.

## <a name="view-the-logged-information"></a>Anzeigen der protokollierten Informationen

Die Git-Informationen sind in den Eigenschaften für einen Trainingslauf gespeichert. Sie können diese Informationen mithilfe von Azure-Portal, Python SDK und CLI anzeigen. 

### <a name="azure-portal"></a>Azure-Portal

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) Ihren Arbeitsbereich aus.
1. Wählen Sie __Experimente__ und dann eins Ihrer Experimente aus.
1. Wählen Sie einen der Läufe in der Spalte __Ausführungsanzahl__ aus.
1. Wählen Sie __Protokolle__ aus, und erweitern Sie dann die Einträge __logs__ und __azureml__. Wählen Sie den Link aus, der mit __###\_azure__ beginnt.

Die protokollierten Informationen enthalten Text, der so ähnlich aussieht wie der folgende JSON-Code:

```json
"properties": {
    "_azureml.ComputeTargetType": "batchai",
    "ContentSnapshotId": "5ca66406-cbac-4d7d-bc95-f5a51dd3e57e",
    "azureml.git.repository_uri": "git@github.com:azure/machinelearningnotebooks",
    "mlflow.source.git.repoURL": "git@github.com:azure/machinelearningnotebooks",
    "azureml.git.branch": "master",
    "mlflow.source.git.branch": "master",
    "azureml.git.commit": "4d2b93784676893f8e346d5f0b9fb894a9cf0742",
    "mlflow.source.git.commit": "4d2b93784676893f8e346d5f0b9fb894a9cf0742",
    "azureml.git.dirty": "True",
    "AzureML.DerivedImageName": "azureml/azureml_9d3568242c6bfef9631879915768deaf",
    "ProcessInfoFile": "azureml-logs/process_info.json",
    "ProcessStatusFile": "azureml-logs/process_status.json"
}
```

### <a name="python-sdk"></a>Python SDK

Nach dem Übermitteln eines Trainingslaufs wird ein [Run](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py)-Objekt zurückgegeben. Das `properties`-Attribut dieses Objekts enthält die protokollierten Git-Informationen. Beispielsweise ruft der folgende Code den Commithash ab:

```python
run.properties['azureml.git.commit']
```

### <a name="cli"></a>Befehlszeilenschnittstelle (CLI)

Der CLI-Befehl `az ml run` kann verwendet werden, um die Eigenschaften eines Laufs abzurufen. Beispielsweise gibt der folgende Befehl die Eigenschaften für den letzten Lauf im Experiment mit dem Namen `train-on-amlcompute` zurück:

```azurecli-interactive
az ml run list -e train-on-amlcompute --last 1 -w myworkspace -g myresourcegroup --query '[].properties'
```

Weitere Informationen finden Sie in der Referenzdokumentation zu [az ml run](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest).

## <a name="next-steps"></a>Nächste Schritte

* Eine exemplarische Vorgehensweise zum Trainieren mit Azure Machine Learning in Visual Studio Code finden Sie im [-Tutorial: Trainieren von Modellen mit Azure Machine Learning](tutorial-train-models-with-aml.md).
* Eine exemplarische Vorgehensweise zum lokalen Bearbeiten, Ausführen und Debuggen von Code finden Sie im [Python-Hello World-Tutorial](https://code.visualstudio.com/docs/Python/Python-tutorial).
