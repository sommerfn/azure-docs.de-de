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
ms.openlocfilehash: 6bddfa048e4cb9cb7728c236417d4672df6f25b3
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73476093"
---
1. [Erstellen Sie einen Azure Machine Learning-Arbeitsbereich](../articles/machine-learning/service/how-to-manage-workspace.md).

1. Klonen Sie das [GitHub-Repository](https://aka.ms/aml-notebooks).

    ```CLI
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Fügen Sie mit einer der folgenden Methoden dem geklonten Verzeichnis eine Datei für die Arbeitsbereichskonfiguration hinzu:

    * Wählen Sie im [Azure-Portal](https://ms.portal.azure.com) im Abschnitt **Übersicht** Ihres Arbeitsbereichs **„config.json“ herunterladen** aus. 

    ![Herunterladen von „config.json“](./media/aml-dsvm-server/download-config.png)

    * Erstellen Sie einen neuen Arbeitsbereich, indem Sie den Code im Notebook [configuration.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/configuration.ipynb) in Ihrem geklonten Verzeichnis verwenden.

1. Starten Sie den Notebook-Server aus Ihrem geklonten Verzeichnis.

    ```shell
    jupyter notebook
    ```