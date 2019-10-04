---
title: include file
description: include file
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 01/25/2019
ms.openlocfilehash: 6c93d1243db1b3c4277a54cf71e10f6bbc648d26
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/08/2019
ms.locfileid: "68846020"
---
- Installiertes Azure Machine Learning SDK für Python. Verwenden Sie die Anleitungen unter [Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py) für Folgendes:


1. Verwenden Sie die Anleitungen unter [Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py) für Folgendes:
    * Erstellen einer Miniconda-Umgebung[Erstellen und Verwalten von Azure Machine Learning Service-Arbeitsbereichen]
    * Installieren des Azure Machine Learning SDK für Python

1. Erstellen Sie einen [Azure Machine Learning Service-Arbeitsbereich](../articles/machine-learning/service/how-to-manage-workspace.md).

1. Schreiben Sie eine [Konfigurationsdatei](../articles/machine-learning/service/how-to-configure-environment.md#workspace) (**aml_config/config.json**).

1. Klonen Sie das [GitHub-Repository](https://aka.ms/aml-notebooks).

    ```CLI
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Starten Sie den Notebook-Server aus Ihrem geklonten Verzeichnis.

    ```shell
    jupyter notebook
    ```