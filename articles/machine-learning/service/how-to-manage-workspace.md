---
title: Erstellen von Azure ML-Arbeitsbereichen im Portal
titleSuffix: Azure Machine Learning service
description: Hier erfahren Sie, wie Sie Azure Machine Learning Service-Arbeitsbereiche im Azure-Portal erstellen, anzeigen und löschen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: shipatel
author: shivp950
ms.date: 05/10/2019
ms.custom: seodec18
ms.openlocfilehash: df015df3c20cdb99512167e3dc3e455da2762c52
ms.sourcegitcommit: 86d49daccdab383331fc4072b2b761876b73510e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70744788"
---
# <a name="create-and-manage-azure-machine-learning-service-workspaces-in-the-azure-portal"></a>Erstellen und Verwalten von Azure Machine Learning Service-Arbeitsbereichen im Portal

In diesem Artikel wird erläutert, wie Sie [**Azure Machine Learning Service-Arbeitsbereiche**](concept-workspace.md) im Azure-Portal für [Azure Machine Learning Service](overview-what-is-azure-ml.md) erstellen, anzeigen und löschen.  Das Portal eignet sich am besten für die ersten Schritte mit Arbeitsbereichen. Wenn sich Ihre Anforderungen ändern oder eine umfassendere Automatisierung erforderlich wird, können Sie Arbeitsbereiche aber auch mit der [CLI](reference-azure-machine-learning-cli.md), mit [Python-Code](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) oder über die [VS Code-Erweiterung](how-to-vscode-tools.md#get-started-with-azure-machine-learning) erstellen und löschen.

## <a name="create-a-workspace"></a>Erstellen eines Arbeitsbereichs

Zum Erstellen eines Arbeitsbereichs benötigen Sie ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie noch heute die [kostenlose oder kostenpflichtige Version von Azure Machine Learning Service](https://aka.ms/AMLFree) aus.

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

### <a name="download-a-configuration-file"></a>Herunterladen einer Konfigurationsdatei

1. Wenn Sie einen [virtuellen Notebook-Computer](tutorial-1st-experiment-sdk-setup.md#azure) erstellen, überspringen Sie diesen Schritt.

1. Wenn Sie Code in Ihrer lokalen Umgebung verwenden möchten, der auf diesen Arbeitsbereich verweist, wählen Sie im Abschnitt **Übersicht** des Arbeitsbereichs die Option **„config.json“ herunterladen** aus.  

   ![Herunterladen von „config.json“](./media/how-to-manage-workspace/configure.png)
   
   Legen Sie die Datei in der Verzeichnisstruktur mit Ihren Python-Skripts oder Jupyter Notebooks ab. Sie kann sich im selben Verzeichnis, in einem Unterverzeichnis namens *.azureml* oder in einem übergeordneten Verzeichnis befinden. Bei der Erstellung eines virtuellen Notebook-Computers wird diese Datei automatisch dem richtigen Verzeichnis auf dem virtuellen Computer hinzugefügt.


## <a name="view"></a>Anzeigen eines Arbeitsbereichs

1. Wählen Sie links oben im Portal die Option **Alle Dienste** aus.

1. Geben Sie **Machine Learning Service** in das Filterfeld **Alle Dienste** ein.  

1. Wählen Sie **Machine Learning Service-Arbeitsbereiche** aus.

   ![Suchen nach dem Azure Machine Learning Service-Arbeitsbereich](media/how-to-manage-workspace/all-services.png)

1. Durchsuchen Sie die Liste der gefundenen Arbeitsbereiche. Sie können basierend auf Abonnement, Ressourcengruppen und Standorten filtern.  

1. Wählen Sie einen Arbeitsbereich aus, um seine Eigenschaften anzuzeigen.
   ![Arbeitsbereichseigenschaften](media/how-to-manage-workspace/allservices_view_workspace_full.PNG)

## <a name="delete-a-workspace"></a>Löschen eines Arbeitsbereichs

Verwenden Sie die Schaltfläche „Löschen“ oben in dem Arbeitsbereich, den Sie löschen möchten.

  ![Schaltfläche „Löschen“](media/how-to-manage-workspace/delete-workspace.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Nächste Schritte

Schließen Sie das gesamte Tutorial ab, um zu erfahren, wie Sie einen Arbeitsbereich zum Erstellen, Trainieren und Bereitstellen von Modellen mit dem Azure Machine Learning-Dienst verwenden.

> [!div class="nextstepaction"]
> [Tutorial: Trainieren von Modellen](tutorial-train-models-with-aml.md)
