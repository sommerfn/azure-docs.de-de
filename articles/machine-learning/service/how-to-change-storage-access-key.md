---
title: Zugriffsschlüssel für Speicherkonten ändern
titleSuffix: Azure Machine Learning service
description: Erfahren Sie, wie Sie die Zugriffsschlüssel für das von Ihrem Arbeitsbereich verwendete Azure Storage-Konto ändern können. Der Azure Machine Learning Service verwendet ein Azure Storage-Konto, um Daten und Modelle zu speichern. Wenn Sie den Zugriffsschlüssel für das Speicherkonto neu generieren, müssen Sie den Dienst Azure Machine Learning aktualisieren, um die neuen Schlüssel zu verwenden.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 05/17/2019
ms.openlocfilehash: 0721542811709e9b938fea3f31bc2a0a28ecdc74
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/19/2019
ms.locfileid: "68358775"
---
# <a name="regenerate-storage-account-access-keys"></a>Zugriffsschlüssel für Speicherkonten neu generieren

Erfahren Sie, wie Sie die Zugriffsschlüssel für Azure Storage-Konten ändern, die vom Azure Machine Learning-Dienst verwendet werden. Azure Machine Learning kann Speicherkonten verwenden, um Daten oder trainierte Modelle zu speichern.

Aus Sicherheitsgründen müssen Sie möglicherweise die Zugriffsschlüssel für ein Azure Storage-Konto ändern. Wenn Sie den Zugriffsschlüssel neu generieren, muss Azure Machine Learning aktualisiert werden, um den neuen Schlüssel zu verwenden. Azure Machine Learning kann das Speicherkonto sowohl für die Modellspeicherung als auch als Datenspeicher verwenden.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure Machine Learning-Dienstbereich. Weitere Informationen finden Sie unter [Erstellen eines Workspace](setup-create-workspace.md)-Artikels.

* [Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).

* Die [CLI-Erweiterung Azure Machine Learning](reference-azure-machine-learning-cli.md).

<a id="whattoupdate"></a> 

## <a name="what-needs-to-be-updated"></a>Was aktualisiert werden muss

Speicherkonten können vom Azure Machine Learning Service-Arbeitsbereich (Speichern von Protokollen, Modellen, Snapshots usw.) und als Datenspeicher verwendet werden. Der Prozess zur Aktualisierung des Arbeitsbereichs ist ein einzelner Azure CLI-Befehl und kann nach der Aktualisierung des Speicherschlüssels ausgeführt werden. Der Prozess der Aktualisierung von Datenspeichern ist aufwändiger und erfordert es, herauszufinden, welche Datenspeicher derzeit das Speicherkonto verwenden und sie dann neu zu registrieren.

> [!IMPORTANT]
> Aktualisieren Sie den Arbeitsbereich mit der Azure CLI und die Datenspeicher mit Python gleichzeitig. Die Aktualisierung nur des einen oder anderen ist nicht ausreichend und kann zu Fehlern führen, bis beide aktualisiert werden.

Um die Speicherkonten zu ermitteln, die von Ihren Datenspeichern verwendet werden, verwenden Sie den folgenden Code:

```python
import azureml.core
from azureml.core import Workspace, Datastore

ws = Workspace.from_config()

default_ds = ws.get_default_datastore()
print("Default datstore: " + default_ds.name + ", storage account name: " +
      default_ds.account_name + ", container name: " + ds.container_name)

datastores = ws.datastores
for name, ds in datastores.items():
    if ds.datastore_type == "AzureBlob" or ds.datastore_type == "AzureFile":
        print("datastore name: " + name + ", storage account name: " +
              ds.account_name + ", container name: " + ds.container_name)
```

Dieser Code sucht nach allen registrierten Datenspeichern, die Azure Storage verwenden, und listet die folgenden Informationen auf:

* Datenspeicher: Der Name des Datenspeichers, unter dem das Speicherkonto registriert ist.
* Azure Storage-Kontoname: Der Name des Azure Storage-Kontos.
* Container: Der Container im Speicherkonto, der bei dieser Registrierung verwendet wird.

Wenn ein Eintrag für das Speicherkonto existiert, für das Sie eine Neugenerierung der Zugriffsschlüssel planen, speichern Sie den Namen des Datenspeichers, den Namen des Speicherkonto-Namens und den Containernamen.

## <a name="update-the-access-key"></a>Aktualisieren Sie den Zugriffsschlüssel

Um den Azure Machine Learning-Dienst auf die Verwendung des neuen Schlüssels zu aktualisieren, gehen Sie wie folgt vor:

> [!IMPORTANT]
> Führen Sie alle Schritte aus, indem Sie sowohl den Arbeitsbereich über die CLI als auch die Datenspeicher über Python aktualisieren. Die Aktualisierung nur des einen oder anderen kann zu Fehlern führen, bis beide aktualisiert werden.

1. Wiederherstellung des Schlüssels. Informationen zur Neugenerierung eines Zugriffsschlüssels finden Sie im Artikel [Verwaltung eines Lagerkontos](/azure/storage/common/storage-account-manage#access-keys). Speichern Sie den neuen Schlüssel.

1. Um den Arbeitsbereich auf die Verwendung des neuen Schlüssels zu aktualisieren, gehen Sie wie folgt vor:

    1. Um sich bei dem Azure-Abonnement anzumelden, das Ihren Arbeitsbereich enthält, verwenden Sie den folgenden Befehl Azure CLI:

        ```azurecli-interactive
        az login
        ```

    1. Um die Erweiterung Azure Machine Learning zu installieren, verwenden Sie den folgenden Befehl:

        ```azurecli-interactive
        az extension add -n azure-cli-ml 
        ```

    1. Um den Arbeitsbereich auf die Verwendung des neuen Schlüssels zu aktualisieren, verwenden Sie den folgenden Befehl. Ersetzen `myworkspace` Sie durch den Namen Ihres Azure Machine Learning-Arbeitsbereichs und ersetzen Sie `myresourcegroup` durch den Namen der Azure-Ressourcengruppe, die den Arbeitsbereich enthält.

        ```azurecli-interactive
        az ml workspace sync-keys -w myworkspace -g myresourcegroup
        ```

        Dieser Befehl synchronisiert automatisch die neuen Schlüssel für das vom Arbeitsbereich verwendete Azure-Speicherkonto.

1. Um Datenspeicher, die das Speicherkonto verwenden, neu zu registrieren, verwenden Sie die Werte aus dem Abschnitt [Was muss aktualisiert werden](#whattoupdate) und den Schlüssel aus Schritt 1 mit dem folgenden Code:

    ```python
    ds = Datastore.register_azure_blob_container(workspace=ws, 
                                              datastore_name='your datastore name', 
                                              container_name='your container name',
                                              account_name='your storage account name', 
                                              account_key='new storage account key',
                                              overwrite=True)
    ```

    Da `overwrite=True` spezifiziert ist, überschreibt dieser Code die bestehende Registrierung und aktualisiert sie, um den neuen Schlüssel zu verwenden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Registrierung von Datenspeichern finden Sie in der [`Datastore`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py) Klassenreferenz.
