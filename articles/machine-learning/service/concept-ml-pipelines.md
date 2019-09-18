---
title: Beschreibung von ML-Pipelines
titleSuffix: Azure Machine Learning service
description: In diesem Artikel lernen Sie die Machine Learning-Pipelines, die Sie mit dem Azure Machine Learning SDK für Python erstellen können, und die Vorteile der Verwendung von Pipelines kennen. Machine Learning-Pipelines werden von Data Scientists verwendet, um Workflows für das maschinelle Lernen zu erstellen, zu optimieren und zu verwalten.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: sanpil
author: sanpil
ms.date: 08/08/2019
ms.custom: seodec18
ms.openlocfilehash: c40fd4f776e0372d7368d182d30cfb2e01a4b0e2
ms.sourcegitcommit: 65131f6188a02efe1704d92f0fd473b21c760d08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2019
ms.locfileid: "70858823"
---
# <a name="what-are-ml-pipelines-in-azure-machine-learning-service"></a>Was sind ML-Pipelines in Azure Machine Learning Service?

Erfahren Sie etwas über die Machine Learning-Pipelines, die Sie mit Azure Machine Learning Service erstellen und verwalten können. 

Mithilfe von Pipelines für maschinelles Lernen können Datenanalysten, Dateningenieure und IT-Experten an den Schritten für Folgendes zusammenarbeiten:
+ Datenaufbereitung, z.B. Normalisierung und Transformation
+ Modelltraining
+ Modellauswertung
+ Bereitstellung

Erfahren Sie, wie Sie [Ihre erste Pipeline erstellen](how-to-create-your-first-pipeline.md).

![Machine Learning-Pipelines im Azure Machine Learning Service](./media/concept-ml-pipelines/pipeline-flow.png)

<a name="compare"></a>
### <a name="which-azure-pipeline-technology-should-i-use"></a>Welche Azure-Pipelinetechnologie sollte ich verwenden?

Die Azure-Cloud bietet mehrere andere Pipelines, die jeweils einem anderen Zweck dienen. Die folgende Tabelle listet die verschiedenen Pipelines und ihren Verwendungszweck auf:

| Pipeline | Funktionsbeschreibung | Kanonische Pipe |
| ---- | ---- | ---- |
| Azure Machine Learning-Pipelines | Definiert wiederverwendbare Machine Learning-Workflows, die als Vorlage für Ihre Machine Learning-Szenarien verwendet werden können. | Daten -> Modell |
| [Azure Data Factory-Pipelines](https://docs.microsoft.com/azure/data-factory/concepts-pipelines-activities) | Gruppieren Datenverschiebungs-, Transformations- und Steuerungsaktivitäten, die für das Ausführen einer Aufgabe erforderlich sind.  | Daten -> Daten |
| [Azure Pipelines](https://azure.microsoft.com/services/devops/pipelines/) | Continuous Integration und Delivery Ihrer Anwendung auf jeder Plattform und in jeder Cloud  | Code -> App/Dienst |

## <a name="why-build-pipelines-with-azure-machine-learning"></a>Erstellen von Pipelines mit Azure Machine Learning

Machine Learning-Pipelines optimieren Ihren Workflow für Geschwindigkeit, Portabilität und Wiederverwendung, sodass Sie sich auf Ihr Kenntnisse im maschinellen Lernen konzentrieren können, anstatt sich um Infrastruktur und Automatisierung kümmern zu müssen.

Pipelines werden in mehreren **Schritten** erstellt, bei denen es sich um unterschiedliche Recheneinheiten in der Pipeline handelt. Jeder Schritt kann unabhängig ausgeführt werden und isolierte Computeressourcen verwenden.
Durch unabhängige Schritte können mehrere Datenanalysten gleichzeitig an derselben Pipeline arbeiten, ohne die Computeressourcen zu überlasten. Außerdem ist es so einfacher, für jeden Schritt verschiedene Computetypen/-größen zu verwenden.

Nachdem die Pipeline entworfen wurde, wird ihr Trainingsprozess in der Regel weiter optimiert. Wenn Sie eine Pipeline erneut ausführen, springt die Ausführung zu den einzelnen Schritten, die wiederholt werden müssen, z. B. zu einem aktualisierten Trainingsskript, und überspringt die, bei denen keine Änderungen aufgetreten sind. Das gleiche gilt für unveränderte Skripts, die zum Ausführen des Schritts verwendet werden. Diese Wiederverwendungsfunktion hilft bei der Ausführung kostspieliger und zeitintensiver Schritte wie der Datenerfassung und -transformation, wenn die zugrunde liegenden Daten unverändert sind.

Mit Azure Machine Learning können Sie verschiedene Toolkits und Frameworks wie PyTorch oder TensorFlow für jeden Pipelineschritt verwenden. Azure koordiniert zwischen den verschiedenen [Computezielen](concept-azure-machine-learning-architecture.md), die Sie verwenden, sodass Ihre Zwischendaten problemlos für die nachgelagerten Computeziele freigegeben werden können.

Für das [Nachverfolgen der Metriken für Ihre Pipelineexperimente](https://docs.microsoft.com/azure/machine-learning/service/how-to-track-experiments) haben Sie zwei Möglichkeiten: Nachverfolgung direkt im Azure-Portal oder über die [Landing Page Ihres Arbeitsbereichs (Vorschau)](https://ml.azure.com). Nach dem Veröffentlichen einer Pipeline können Sie einen REST-Endpunkt konfigurieren, mit dem Sie die Pipeline über eine beliebige Plattform bzw. einen beliebigen Stapel erneut ausführen können.

## <a name="key-advantages"></a>Hauptvorteile

Die wichtigsten Argumente für das Verwenden von Pipelines für Ihre Workflows mit maschinellem Lernen sind die folgenden:

|Vorteil|BESCHREIBUNG|
|:-------:|-----------|
|**Unbeaufsichtigte&nbsp;Ausführung**|Planen Sie Schritte, die zuverlässig und unbeaufsichtigt parallel oder nacheinander ausgeführt werden können. Die Datenvorbereitung und -modellierung kann Tage oder Wochen in Anspruch nehmen, und mit Pipelines können Sie sich auf andere Aufgaben konzentrieren, während der Prozess ausgeführt wird. |
|**Heterogenes Compute**|Verwenden Sie mehrere Pipelines, die zuverlässig über heterogene und skalierbare Computeressourcen und Speicherorte koordiniert werden. Um die verfügbaren Computeoptionen effizient zu nutzen, können einzelne Pipelineschritte auf verschiedenen Computezielen ausgeführt werden, z. B. HDInsight, Data Science-VMs mit GPU und Databricks.|
|**Wiederverwendbarkeit**|Erstellen Sie Pipelinevorlagen für bestimmte Szenarien wie erneutes Training und Batchbewertung. Lösen Sie veröffentlichte Pipelines von externen Systemen über einfache REST-Aufrufe aus.|
|**Nachverfolgung und Versionierung**|Statt Daten- und Ergebnispfade bei der Iteration manuell zu verfolgen, verwenden Sie das Pipelines SDK, um Ihre Datenquellen, Eingaben und Ausgaben explizit zu benennen und Versionen zu verwalten. Sie können Skripts und Daten auch separat verwalten, um die Produktivität zu steigern.|
|**Kollaboration**|Pipelines ermöglichen Data Scientists, in allen Bereichen des Entwurfsprozesses für maschinelles Lernen zusammenzuarbeiten, während sie gleichzeitig an Pipelineschritten arbeiten können.|

## <a name="the-python-sdk-for-pipelines"></a>Python SDK für Pipelines

[Verwenden Sie das Python SDK](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py) zum Erstellen Ihrer ML-Pipelines in Ihrer bevorzugten integrierten Entwicklungsumgebung (Integrated Development Environment, IDE) bzw. Ihren bevorzugten Jupyter Notebooks. Das Azure Machine Learning SDK bietet imperative Konstrukte zum Sequenzieren und Parallelisieren der Pipelineschritte, wenn keine Datenabhängigkeit besteht. 

Mit deklarativen Datenabhängigkeiten können Sie Ihre Aufgaben optimieren. Das SDK enthält ein Framework von vordefinierten Modulen für allgemeine Aufgaben wie Datenübertragung und Modellveröffentlichung. Sie können das Framework erweitern, um eigene Konventionen zu modellieren. Implementieren Sie dafür benutzerdefinierte Schritte, die über Pipelines hinweg wiederverwendbar sind. Sie können Computeziele und Speicherressourcen ebenfalls direkt über das SDK verwalten.

Speichern Sie Ihre Pipelines als Vorlagen, und stellen Sie sie auf einem REST-Endpunkt für Batchbewertungen und Aufträge zum erneuten Training bereit.

Es gibt zwei Python-Pakete für Pipelines mit Azure Machine Learning: [azureml-pipelines-core](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py) und [azureml-pipeline-steps](https://docs.microsoft.com/python/api/azureml-pipeline-steps/?view=azure-ml-py). Verwenden Sie zum schnellen Einstieg eines der vordefinierten Module, z. B.:

* Ausführen eines Python-Skripts in einem Schritt mit [PythonScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.python_script_step.pythonscriptstep)
* Übertragen von Daten zwischen Speicheroptionen mit [DataTransferStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.datatransferstep)
* Erstellen eines AutoML-Pipelineschritts mit [AutoMLStep](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlstep)

## <a name="next-steps"></a>Nächste Schritte

+ Erfahren Sie, wie Sie [Ihre erste Pipeline erstellen](how-to-create-your-first-pipeline.md).

+ Erfahren Sie, wie Sie [Batchvorhersagen für große Datenmengen ausführen](how-to-run-batch-predictions.md).

+ Weitere Informationen finden Sie in der [SDK-Referenzdokumentation für Pipelines](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py).

+ Testen Sie Beispiele für Jupyter Notebooks, die [Azure Machine Learning-Pipelines](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines) veranschaulichen. Erfahren Sie, wie Sie [Notebooks ausführen, um diesen Dienst zu untersuchen](samples-notebooks.md).
