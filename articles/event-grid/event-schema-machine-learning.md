---
title: Azure Event Grid-Ereignisschema für Machine Learning
description: Beschreibt die Eigenschaften, die mit Azure Event Grid für Machine Learning-Arbeitsbereichsereignisse bereitgestellt werden.
services: event-grid
author: jenns
ms.service: event-grid
ms.topic: reference
ms.date: 10/18/2019
ms.author: jenns
ms.openlocfilehash: 6c2a8fa57bf6e3a552da57588bdbe752ef0d22e2
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73608594"
---
# <a name="azure-event-grid-event-schema-for-azure-machine-learning"></a>Azure Event Grid-Ereignisschema für Azure Machine Learning

In diesem Artikel werden die Eigenschaften und das Schema für Machine Learning-Arbeitsbereichsereignisse beschrieben. Eine Einführung in Ereignisschemas finden Sie unter [Azure Event Grid-Ereignisschema](event-schema.md).

Eine Liste mit Beispielskripts und Tutorials finden Sie unter [Ereignisquellen in AzureML](event-sources.md#azure-machine-learning).

## <a name="available-event-types"></a>Verfügbare Ereignistypen

Azure Machine Learning gibt die folgenden Ereignistypen aus:

| Ereignistypen | BESCHREIBUNG |
| ---------- | ----------- |
| Microsoft.MachineLearningServices.ModelRegistered | Wird ausgelöst, wenn ein neues Modell oder eine neue Modellversion erfolgreich registriert wurde. |
| Microsoft.MachineLearningServices.ModelDeployed | Wird ausgelöst, wenn Modelle erfolgreich auf einem Endpunkt bereitgestellt wurden. |
| Microsoft.MachineLearningServices.RunCompleted | Wird ausgelöst, wenn eine Ausführung erfolgreich abgeschlossen wurde. |
| Microsoft.MachineLearningServices.DatasetDriftDetected | Wird ausgelöst, wenn ein Datasetdriftmonitor eine Abweichung erkennt. |

## <a name="the-contents-of-an-event-response"></a>Der Inhalt einer Ereignisantwort

Wenn ein Ereignis ausgelöst wird, sendet der Event Grid-Dienst Daten zum Ereignis an den Endpunkt, der über ein entsprechendes Abonnement verfügt.

In diesem Abschnitt wird anhand eines Beispiels gezeigt, wie diese Daten für jedes Ereignis aussehen.

### <a name="microsoftmachinelearningservicesmodelregistered-event"></a>Microsoft.MachineLearningServices.ModelRegistered event

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearningServices/workspaces/{workspace-name}",
  "subject": "models/sklearn_regression_model:20",
  "eventType": "Microsoft.MachineLearningServices.ModelRegistered",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "ModelName": "sklearn_regression_model",
    "ModelVersion": 20,
    "ModelTags": {
        "area": "diabetes",
        "type": "regression"
    },
    "ModelProperties": {
        "type": "test"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

### <a name="microsoftmachinelearningservicesmodeldeployed-event"></a>Microsoft.MachineLearningServices.ModelDeployed event

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearningServices/workspaces/{workspace-name}",
  "subject": "endpoints/my-sklearn-service",
  "eventType": "Microsoft.MachineLearningServices.ModelDeployed",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "ServiceName": "my-sklearn-service",
    "ServiceComputeType": "ACI",
    "ModelIds": "sklearn_regression_model:1,sklearn_regression_model:2",
    "ServiceTags": {
        "area": "diabetes",
        "type": "regression"
    },
    "ServiceProperties": {
        "type": "test"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

### <a name="microsoftmachinelearningservicesruncompleted-event"></a>Microsoft.MachineLearningServices.RunCompleted event

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearningServices/workspaces/{workspace-name}",
  "subject": "experiments/0fa9dfaa-cba3-4fa7-b590-23e48548f5c1/runs/AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5",
  "eventType": "Microsoft.MachineLearningServices.RunCompleted",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "ExperimentId": "0fa9dfaa-cba3-4fa7-b590-23e48548f5c1",
    "ExperimentName": "automl-local-regression",
    "RunId": "AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5",
    "RunType": null,
    "RunTags": {},
    "RunProperties": {
        "runTemplate": "automl_child",
        "pipeline_id": "5adc0a4fe02504a586f09a4fcbb241f9a4012062",
        "pipeline_spec": "{\"objects\": [{\"class_name\": \"StandardScaler\", \"module\": \"sklearn.preprocessing\", \"param_args\": [], \"param_kwargs\": {\"with_mean\": true, \"with_std\": false}, \"prepared_kwargs\": {}, \"spec_class\": \"preproc\"}, {\"class_name\": \"LassoLars\", \"module\": \"sklearn.linear_model\", \"param_args\": [], \"param_kwargs\": {\"alpha\": 0.001, \"normalize\": true}, \"prepared_kwargs\": {}, \"spec_class\": \"sklearn\"}], \"pipeline_id\": \"5adc0a4fe02504a586f09a4fcbb241f9a4012062\"}",
        "training_percent": "100",
        "predicted_cost": "0.062226144097381045",
        "iteration": "5",
        "run_template": "automl_child",
        "run_preprocessor": "StandardScalerWrapper",
        "run_algorithm": "LassoLars",
        "conda_env_data_location": "aml://artifact/ExperimentRun/dcid.AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5/outputs/conda_env_v_1_0_0.yml",
        "model_name": "AutoMLad912b2d65",
        "scoring_data_location": "aml://artifact/ExperimentRun/dcid.AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5/outputs/scoring_file_v_1_0_0.py",
        "model_data_location": "aml://artifact/ExperimentRun/dcid.AutoML_ad912b2d-6467-4f32-a616-dbe4af6dd8fc_5/outputs/model.pkl"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

### <a name="microsoftmachinelearningservicesdatasetdriftdetected-event"></a>Microsoft.MachineLearningServices.DatasetDriftDetected event

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearningServices/workspaces/{workspace-name}",
  "subject": "datadrifts/{}/runs/{}",
  "eventType": "Microsoft.MachineLearningServices.DatasetDriftDetected",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "DataDriftId": "01d29aa4-e6a4-470a-9ef3-66660d21f8ef",
    "DataDriftName": "myDriftMonitor",
    "RunId": "01d29aa4-e6a4-470a-9ef3-66660d21f8ef_1571590300380",
    "BaseDatasetId": "3c56d136-0f64-4657-a0e8-5162089a88a3",
    "TargetDatasetId": "d7e74d2e-c972-4266-b5fb-6c9c182d2a74",
    "DriftCoefficient": 0.83503490684792081,
    "StartTime": "2019-07-04T00:00:00+00:00",
    "EndTime": "2019-07-05T00:00:00+00:00"
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

## <a name="event-properties"></a>Ereigniseigenschaften

Ein Ereignis weist die folgenden Daten auf oberster Ebene aus:

| Eigenschaft | Typ | BESCHREIBUNG |
| -------- | ---- | ----------- |
| topic | string | Vollständiger Ressourcenpfaf zur Ereignisquelle. Dieses Feld ist nicht beschreibbar. Dieser Wert wird von Event Grid bereitgestellt. |
| subject | string | Vom Herausgeber definierter Pfad zum Ereignisbetreff |
| eventType | string | Einer der registrierten Ereignistypen für die Ereignisquelle. |
| eventTime | string | Die Zeit, in der das Ereignis generiert wird, basierend auf der UTC-Zeit des Anbieters. |
| id | string | Eindeutiger Bezeichner für das Ereignis. |
| data | object | Ereignisdaten für Blob Storage. |
| dataVersion | string | Die Schemaversion des Datenobjekts. Der Herausgeber definiert die Schemaversion. |
| metadataVersion | string | Die Schemaversion der Ereignismetadaten. Event Grid definiert das Schema der Eigenschaften der obersten Ebene. Dieser Wert wird von Event Grid bereitgestellt. |

Das Datenobjekt weist für jeden Ereignistyp die folgenden Eigenschaften auf:

### <a name="microsoftmachinelearningservicesmodelregistered"></a>Microsoft.MachineLearningServices.ModelRegistered

| Eigenschaft | Typ | BESCHREIBUNG |
| -------- | ---- | ----------- |
| ModelName | Zeichenfolge | Der Name des Modells, das registriert wurde. |
| ModelVersion | int | Die Version des Modells, das registriert wurde. |
| ModelTags | object | Die Tags des Modells, das registriert wurde. |
| ModelProperties | object | Die Eigenschaften des Modells, das registriert wurde. |

### <a name="microsoftmachinelearningservicesmodeldeployed"></a>Microsoft.MachineLearningServices.ModelDeployed

| Eigenschaft | Typ | BESCHREIBUNG |
| -------- | ---- | ----------- |
| ServiceName | Zeichenfolge | Der Name des bereitgestellten Diensts. |
| ServiceComputeType | Zeichenfolge | Der Computetyp (z. B. ACI, AKS) des bereitgestellten Diensts. |
| ModelIds | Zeichenfolge | Eine durch Trennzeichen getrennte Liste der Modell-IDs. Die IDs der Modell, die im Dienst bereitgestellt werden. |
| ServiceTags | object | Die Tags des bereitgestellten Diensts. |
| ServiceProperties | object | Die Eigenschaften des bereitgestellten Diensts. |

### <a name="microsoftmachinelearningservicesruncompleted"></a>Microsoft.MachineLearningServices.RunCompleted

| Eigenschaft | Typ | BESCHREIBUNG |
| -------- | ---- | ----------- |
| ExperimentId | Zeichenfolge | Die ID des Experiments, zu der die Ausführung gehört. |
| ExperimentName | Zeichenfolge | Der Name des Experiments, zu der die Ausführung gehört. |
| RunId | Zeichenfolge | Die ID der Ausführung, die abgeschlossen wurde. |
| RunType | Zeichenfolge | Der Ausführungstyp der abgeschlossenen Ausführung. |
| RunTags | object | Die Tags der abgeschlossenen Ausführung. |
| RunProperties | object | Die Eigenschaften der abgeschlossenen Ausführung. |

### <a name="microsoftmachinelearningservicesdatasetdriftdetected"></a>Microsoft.MachineLearningServices.DatasetDriftDetected

| Eigenschaft | Typ | BESCHREIBUNG |
| -------- | ---- | ----------- |
| DataDriftId | Zeichenfolge | Die ID des Datendriftmonitors, der das Ereignis ausgelöst hat. |
| DataDriftName | Zeichenfolge | Der Name des Datendriftmonitors, der das Ereignis ausgelöst hat. |
| RunId | Zeichenfolge | Die ID der Ausführung, mit der die Datenabweichung erkannt wurde. |
| BaseDatasetId | Zeichenfolge | Die ID des Basisdatasets, mit der die Abweichung erkannt wurde. |
| TargetDatasetId | Zeichenfolge | Die ID des Zieldatasets, mit der die Abweichung erkannt wurde. |
| DriftCoefficient | double | Das Koeffizientenergebnis, das das Ereignis ausgelöst hat. |
| StartTime | datetime | Die Startzeit der Zieldataset-Zeitreihe, die zur Abweichungserkennung geführt hat.  |
| EndTime | datetime | Die Endzeit der Zieldataset-Zeitreihe, die zur Abweichungserkennung geführt hat. |


## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung zu Azure Event Grid finden Sie unter [Einführung in Azure Event Grid](overview.md).
* Weitere Informationen zum Erstellen eines Azure Event Grid-Abonnements finden Sie unter [Event Grid-Abonnementschema](subscription-creation-schema.md).
* Eine Einführung in die Verwendung von Azure Event Grid mit Azure Machine Learning finden Sie unter [Nutzen von Azure Machine Learning-Ereignissen](/azure/machine-learning/service/concept-event-grid-integration).
* Ein Beispiel zur Verwendung von Azure Event Grid mit Azure Machine Learning finden Sie unter [Erstellen ereignisgesteuerter Machine Learning-Workflows](/azure/machine-learning/service/event-schema-machine-learning).