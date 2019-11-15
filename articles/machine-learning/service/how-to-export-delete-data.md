---
title: Exportieren oder Löschen von Arbeitsbereichsdaten
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie Ihren Arbeitsbereich mit Azure Machine Learning Studio, der Befehlszeilenschnittstelle, dem SDK und authentifizierten REST-APIs exportieren oder löschen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: lobrien
ms.author: laobri
ms.date: 11/04/2019
ms.custom: seodec18
ms.openlocfilehash: 34ff3f9704b9c84a7daddcfd14fb9cd3e990f794
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73716510"
---
# <a name="export-or-delete-your-machine-learning-service-workspace-data"></a>Exportieren oder Löschen Ihrer Arbeitsbereichsdaten im Machine Learning-Dienst 
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

In Azure Machine Learning können Sie Ihre Arbeitsbereichsdaten mit der authentifizierten REST-API exportieren oder löschen. In diesem Artikel erfahren Sie, wie das geht.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="control-your-workspace-data"></a>Steuern Ihrer Arbeitsbereichsdaten
Im Produkt enthaltene und von Azure Machine Learning gespeicherte Daten stehen über Azure Machine Learning Studio, die Befehlszeilenschnittstelle, das SDK und authentifizierte REST-APIs zum Exportieren und Löschen zur Verfügung. Auf Telemetriedaten können Sie über das Azure Privacy-Portal zugreifen. 

In Azure Machine Learning bestehen personenbezogene Daten aus Benutzerinformationen in Laufverlaufsdokumenten und Telemetriedatensätzen einiger Benutzerinteraktionen mit dem Dienst.

## <a name="delete-workspace-data-with-the-rest-api"></a>Löschen von Arbeitsbereichsdaten mit der REST-API 

Um Daten zu löschen, können die folgenden API-Aufrufe mit dem HTTP DELETE-Verb vorgenommen werden. Diese werden durch einen `Authorization: Bearer <arm-token>`-Header in der Anforderung autorisiert, wobei `<arm-token>` das AAD-Zugriffstoken für den Endpunkt `https://management.core.windows.net/` ist.  

Wie Sie dieses Token erhalten und Azure-Endpunkte aufrufen, erfahren Sie in der [REST-API-Dokumentation von Azure](https://docs.microsoft.com/rest/api/azure/).  

In den folgenden Beispielen ersetzen Sie den Text in {} durch die Instanznamen, die die zugehörige Ressource bestimmen.

### <a name="delete-an-entire-workspace"></a>Löschen eines gesamten Arbeitsbereichs

Verwenden Sie diesen Aufruf zum Löschen eines ganzen Arbeitsbereichs.  
> [!WARNING]
> Alle Informationen werden gelöscht, und der Arbeitsbereich kann nicht mehr verwendet werden.

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}?api-version=2018-03-01-preview

### <a name="delete-models"></a>Löschen von Modellen

Verwenden Sie diesen Aufruf, um eine Liste der Modelle und ihrer IDs zu erhalten:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/models?api-version=2018-03-01-preview

Einzelne Modelle können wie folgt gelöscht werden:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/models/{id}?api-version=2018-03-01-preview

### <a name="delete-assets"></a>Löschen von Assets

Verwenden Sie diesen Aufruf, um eine Liste der Assets und ihrer IDs zu erhalten:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/assets?api-version=2018-03-01-preview

Einzelne Assets können wie folgt gelöscht werden:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/assets/{id}?api-version=2018-03-01-preview

### <a name="delete-images"></a>Löschen von Images

Verwenden Sie diesen Aufruf, um eine Liste der Images und ihrer IDs zu erhalten:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/images?api-version=2018-03-01-preview

Einzelne Images können wie folgt gelöscht werden:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/images/{id}?api-version=2018-03-01-preview

### <a name="delete-services"></a>Löschen von Diensten

Verwenden Sie diesen Aufruf, um eine Liste der Dienste und ihrer IDs zu erhalten:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/services?api-version=2018-03-01-preview

Einzelne Dienste können wie folgt gelöscht werden:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/services/{id}?api-version=2018-03-01-preview

## <a name="export-service-data-with-the-rest-api"></a>Exportieren von Dienstdaten mit der REST-API

Um Daten zu exportieren, können die folgenden API-Aufrufe mit dem HTTP GET-Verb vorgenommen werden. Diese werden durch einen `Authorization: Bearer <arm-token>`-Header in der Anforderung autorisiert, wobei `<arm-token>` das AAD-Zugriffstoken für den Endpunkt `https://management.core.windows.net/` ist.  

Wie Sie dieses Token erhalten und Azure-Endpunkte aufrufen, erfahren Sie in der [REST-API-Dokumentation von Azure](https://docs.microsoft.com/rest/api/azure/).   

In den folgenden Beispielen ersetzen Sie den Text in {} durch die Instanznamen, die die zugehörige Ressource bestimmen.

### <a name="export-workspace-information"></a>Exportieren von Arbeitsbereichsinformationen

Verwenden Sie diesen Aufruf, um eine Liste aller Arbeitsbereiche zu erhalten:

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces?api-version=2018-03-01-preview

Informationen zu einem einzelnen Arbeitsbereich können folgendermaßen abgerufen werden:

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}?api-version=2018-03-01-preview

### <a name="export-compute-information"></a>Exportieren von Compute-Informationen

Alle Computeziele, die einem Arbeitsbereich zugeordnet sind, können folgendermaßen abgerufen werden:

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroup/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/computes?api-version=2018-03-01-preview

Informationen zu einem einzigen Computeziel können folgendermaßen abgerufen werden:

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroup/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/computes/{computeName}?api-version=2018-03-01-preview

### <a name="export-run-history-data"></a>Exportieren von Ausführungsverlaufsdaten
Verwenden Sie diesen Aufruf, um eine Liste aller Experimente und ihrer Informationen zu erhalten:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/experiments 

Alle Ausführungen für ein bestimmtes Experiment können folgendermaßen abgerufen werden:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/experiments/{experimentName}/runs 

Elemente des Ausführungsverlaufs können wie folgt abgerufen werden:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/experiments/{experimentName}/runs/{runId} 

Alle Ausführungsmetriken für ein Experiment können folgendermaßen abgerufen werden:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/experiments/{experimentName}/metrics 

Eine einzelne Ausführungsmetrik kann wie folgt abgerufen werden:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/experiments/{experimentName}/metrics/{metricId}
    
### <a name="export-artifacts"></a>Exportieren von Artefakten

Verwenden Sie diesen Aufruf, um eine Liste der Artefakte und ihrer Pfade zu erhalten:

    https://{location}.experiments.azureml.net/artifact/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/artifacts/origins/ExperimentRun/containers/{runId}
    
### <a name="export-notifications"></a>Exportieren von Benachrichtigungen

Verwenden Sie diesen Aufruf, um eine Liste gespeicherter Aufgaben zu erhalten:     

    https://{location}.experiments.azureml.net/notification/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/tasks

Benachrichtigungen für eine einzelne Aufgabe können folgendermaßen abgerufen werden:

    https://{location}.experiments.azureml.net/notification/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}tasks/{taskId}

### <a name="export-data-stores"></a>Exportieren von Datenspeichern

Verwenden Sie diesen Aufruf, um eine Liste von Datenspeichern zu erhalten:

    https://{location}.experiments.azureml.net/datastore/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/datastores

Einzelne Datenspeicher können wie folgt abgerufen werden:

    https://{location}.experiments.azureml.net/datastore/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/datastores/{name}

### <a name="export-models"></a>Exportieren von Modellen

Verwenden Sie diesen Aufruf, um eine Liste der Modelle und ihrer IDs zu erhalten:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/models?api-version=2018-03-01-preview

Einzelne Modelle können wie folgt abgerufen werden:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/models/{id}?api-version=2018-03-01-preview

### <a name="export-assets"></a>Exportieren von Assets

Verwenden Sie diesen Aufruf, um eine Liste der Assets und ihrer IDs zu erhalten:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/assets?api-version=2018-03-01-preview

Einzelne Assets können wie folgt abgerufen werden:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/assets/{id}?api-version=2018-03-01-preview

### <a name="export-images"></a>Exportieren von Images

Verwenden Sie diesen Aufruf, um eine Liste der Images und ihrer IDs zu erhalten:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/images?api-version=2018-03-01-preview

Einzelne Images können wie folgt abgerufen werden:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/images/{id}?api-version=2018-03-01-preview

### <a name="export-services"></a>Exportieren von Diensten

Verwenden Sie diesen Aufruf, um eine Liste der Dienste und ihrer IDs zu erhalten:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/services?api-version=2018-03-01-preview

Einzelne Dienste können wie folgt abgerufen werden:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/services/{id}?api-version=2018-03-01-preview

### <a name="export-pipeline-experiments"></a>Exportieren von Pipeline-Experimenten

Einzelne Experimente können wie folgt abgerufen werden:

    https://{location}.aether.ms/api/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/Experiments/{experimentId}

### <a name="export-pipeline-graphs"></a>Exportieren von Pipelinediagrammen

Einzelne Diagramme können wie folgt abgerufen werden:

    https://{location}.aether.ms/api/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/Graphs/{graphId}

### <a name="export-pipeline-modules"></a>Exportieren von Pipelinemodulen

Module können folgendermaßen abgerufen werden:

    https://{location}.aether.ms/api/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/Modules/{id}

### <a name="export-pipeline-templates"></a>Exportieren von Pipelinevorlagen

Vorlagen können folgendermaßen abgerufen werden:

    https://{location}.aether.ms/api/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/Templates/{templateId}

### <a name="export-pipeline-data-sources"></a>Exportieren von Pipelinedatenquellen

Datenquellen können folgendermaßen abgerufen werden:

    https://{location}.aether.ms/api/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/DataSources/{id}

## <a name="delete-assets-in-the-designer"></a>Löschen von Ressourcen im Designer

Löschen Sie in dem Designer, in dem Sie Ihr Experiment erstellt haben, einzelne Ressourcen:

1. Wechseln zum Designer

    ![Löschen von Assets](media/how-to-export-delete-data.md/delete-experiment.png)

1. Wählen Sie in der Liste den zu löschenden Pipelineentwurf aus.

1. Klicken Sie auf **Löschen**.

### <a name="delete-datasets-in-the-designer"></a>Löschen von Datasets im Designer

Verwenden Sie zum Löschen von Datasets im Designer das Azure-Portal oder Storage-Explorer, um zu verbundenen Speicherkonten zu navigieren und die Datasets dort zu löschen. Durch die Aufhebung der Registrierung von Datasets im Designer wird nur der Referenzpunkt im Speicher entfernt. 

## <a name="export-data-in-the-designer"></a>Exportieren von Daten im Designer

Exportieren Sie in dem Designer, in dem Sie Ihr Experiment erstellt haben, von Ihnen hinzugefügte Daten:

1. Wählen Sie links die Option **Datasets** aus.

    ![Herunterladen der Daten](media/how-to-export-delete-data.md/unregister-dataset.png)
