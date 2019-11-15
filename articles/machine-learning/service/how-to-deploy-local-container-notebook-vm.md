---
title: Bereitstellen von Modellen auf Notebook-VMs
titleSuffix: Azure Machine Learning
description: Hier erfahren Sie, wie Sie Ihre Azure Machine Learning-Modelle mithilfe von Notebook-VMs als Webdienst bereitstellen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: mnark
author: MrudulaN
ms.reviewer: larryfr
ms.date: 10/25/2019
ms.openlocfilehash: d4e37b02b3d7a21546a04c8948fbbfb7262bfa6a
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73584767"
---
# <a name="deploy-a-model-to-azure-machine-learning-notebook-vms"></a>Bereitstellen eines Modells auf Azure Machine Learning-Notebook-VMs

[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

Hier erfahren Sie, wie Sie mit Azure Machine Learning ein Modell als Webdienst auf Ihrer Azure Machine Learning-Notebook-VM bereitstellen. Verwenden Sie Notebook-VMs, wenn eine der folgenden Bedingungen zutrifft:

- Sie müssen Ihr Modell schnell bereitstellen und überprüfen.
- Sie testen ein Modell in der Entwicklungsphase.

> [!TIP]
> Bei der Bereitstellung eines Modells aus einer Jupyter Notebook-Instanz auf einer Notebook-VM in einem Webdienst auf der gleichen VM handelt es sich um eine _lokale Bereitstellung_. In diesem Fall ist der lokale Computer die Notebook-VM. Weitere Informationen zu Bereitstellungen finden Sie unter [Bereitstellen von Modellen mit Azure Machine Learning](how-to-deploy-and-where.md).

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure Machine Learning-Arbeitsbereich mit einer Notebook-VM, die ausgeführt wird. Weitere Informationen finden Sie unter [Tutorial: Einrichten der Umgebung und des Arbeitsbereichs](tutorial-1st-experiment-sdk-setup.md).

## <a name="deploy-to-the-notebook-vms"></a>Bereitstellen auf den Notebook-VMs

Ein Beispielnotebook mit einer Veranschaulichung lokaler Bereitstellungen ist in Ihrer Notebook-VM enthalten. Führen Sie die folgenden Schritte aus, um das Notebook zu laden und das Modell als Webdienst auf dem virtuellen Computer bereitzustellen:

1. Wählen Sie in [Azure Machine Learning Studio](https://ml.azure.com) Ihre Azure Machine Learning-Notebook-VMs aus.

1. Öffnen Sie das Unterverzeichnis `samples-*` und anschließend `how-to-use-azureml/deploy-to-local/register-model-deploy-local.ipynb`. Führen Sie dann das Notebook aus.

    ![Screenshot des ausgeführten lokalen Diensts auf dem Notebook](media/how-to-deploy-local-container-notebookvm/deploy-local-service.png)

1. Im Notebook werden die URL und der Port für den ausgeführten Dienst angezeigt. Beispiel: `https://localhost:6789`. Sie können zum Anzeigen des Ports auch die Zelle ausführen, die `print('Local service port: {}'.format(local_service.port))` enthält.

    ![Screenshot des Ports des ausgeführten lokalen Diensts](media/how-to-deploy-local-container-notebookvm/deploy-local-service-port.png)

1. Verwenden Sie die URL `https://localhost:<local_service.port>`, um den Dienst über die Notebook-VM zu testen. Rufen Sie zum Testen über einen Remoteclient die öffentliche URL des auf der Notebook-VM ausgeführten Diensts ab. Die öffentliche URL kann mit der folgenden Formel bestimmt werden: `https://<notebookvm_name>-<local_service_port>.<azure_region_of_notebook>.notebooks.azureml.net/score`. Beispiel: `https://mynotebookvm-6789.eastus2.notebooks.azureml.net/score`.

## <a name="test-the-service"></a>Testen des Diensts

Verwenden Sie den folgenden Code, um Beispieldaten an den ausgeführten Dienst zu übermitteln. Ersetzen Sie den Wert `service_url` durch die URL aus dem vorherigen Schritt:

```python
import requests
import json
test_sample = json.dumps({'data': [
    [1,2,3,4,5,6,7,8,9,10],
    [10,9,8,7,6,5,4,3,2,1]
]})
test_sample = bytes(test_sample,encoding = 'utf8')
access_token = "your bearer token"
headers = {'Content-Type':'application/json', 'Authorization': 'Bearer ' + access_token}
service_url = "https://vm-name-6789.northcentralus.notebooks.azureml.net/score"
# for a compute instance, the url would be https://vm-name-6789.northcentralus.instances.azureml.net/score
resp = requests.post(service_url, test_sample, headers=headers)
print("prediction:", resp.text)
```

## <a name="next-steps"></a>Nächste Schritte

* [Wie man ein Modell mit einem benutzerdefinierten Docker-Image bereitstellt](how-to-deploy-custom-docker-image.md)
* [Problembehandlung von Bereitstellungen von Azure Machine Learning Service mit AKS und ACI](how-to-troubleshoot-deployment.md)
* [Secure Azure Machine Learning web services with SSL (Sichere Azure Machine Learning-Webdienste mit SSL)](how-to-secure-web-service.md)
* [Consume a ML Model deployed as a web service (Nutzen eines als Webdienst bereitgestellten Azure Machine Learning-Modells)](how-to-consume-web-service.md).
* [Überwachen Ihrer Azure Machine Learning-Modelle mit Application Insights](how-to-enable-app-insights.md)
* [Sammeln von Daten für Modelle in der Produktion](how-to-enable-data-collection.md)