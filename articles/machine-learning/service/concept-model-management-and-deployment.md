---
title: 'MLOps: Verwalten, Bereitstellen und Überwachen von ML-Modellen'
titleSuffix: Azure Machine Learning service
description: Erfahren Sie, wie Sie mit Azure Machine Learning Services für MLOps Ihre Modelle bereitstellen, verwalten und überwachen, um sie kontinuierlich zu verbessern. Sie können die Modelle, die Sie mit Azure Machine Learning Services trainiert haben, auf Ihrem lokalen Computer oder aus anderen Quellen bereitstellen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: chris-lauren
ms.author: clauren
ms.date: 05/02/2019
ms.custom: seodec18
ms.openlocfilehash: 416bebc070cfcad52c6180e65f0066c46c826cbe
ms.sourcegitcommit: 16cb78a0766f9b3efbaf12426519ddab2774b815
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/17/2019
ms.locfileid: "65849643"
---
# <a name="mlops-manage-deploy-and-monitor-models-with-azure-machine-learning-service"></a>MLOps: Verwalten, Bereitstellen und Überwachen von Modellen mit Azure Machine Learning Services

In diesem Artikel lernen Sie, wie Sie mit Azure Machine Learning Services Ihre Modelle bereitstellen, verwalten und überwachen, um sie kontinuierlich zu verbessern. Sie können die Modelle, die Sie mit Azure Machine Learning trainiert haben, auf Ihrem lokalen Computer oder aus anderen Quellen bereitstellen. 

Das folgende Diagramm veranschaulicht den vollständigen Bereitstellungsworkflow: [ ![Bereitstellungsworkflow für Azure Machine Learning](media/concept-model-management-and-deployment/deployment-pipeline.png)](media/concept-model-management-and-deployment/deployment-pipeline.png#lightbox)

Der MLOps-/Bereitstellungsworkflow umfasst die folgenden Schritte:
1. **Registrieren des Modells** in einer Registrierung, die in Ihrem Azure Machine Learning Services-Arbeitsbereich gehostet wird
1. **Verwenden** Sie das Modell in einem Webdienst in der Cloud, auf einem IoT-Gerät oder für Analysen mit Power BI.
1. **Überwachen und Sammeln von Daten**
1. **Aktualisieren** Sie eine Bereitstellung für die Verwendung eines neuen Image.

Jeder Schritt kann eigenständig oder als Teil eines einzelnen Befehls ausgeführt werden. Darüber hinaus können Sie einen **CI/CD-Workflow** erstellen, wie in dieser Grafik dargestellt.

[ ![ ‚Zyklus: Continuous Integration/Continuous Deployment (CI/CD) für Azure Machine Learning‘](media/concept-model-management-and-deployment/model-ci-cd.png)](media/concept-model-management-and-deployment/model-ci-cd.png#lightbox)

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2X1GX]

## <a name="step-1-register-model"></a>Schritt 1: Registrieren des Modells

Die Modellregistrierung ermöglicht es Ihnen, Ihre Modelle in der Azure-Cloud in Ihrem Arbeitsbereich zu speichern und zu versionieren. Die Modellregistrierung erleichtert das Organisieren und Verwalten von trainierten Modellen.

> [!TIP]
> Sie können auch Modelle registrieren, die außerhalb von Azure Machine Learning Service trainiert wurden.
 
Registrierte Modelle werden anhand des Namens und der Version identifiziert. Jedes Mal, wenn Sie ein Modell mit dem gleichen Namen wie ein bereits vorhandenes Modell registrieren, erhöht die Registrierung die Versionsnummer. Sie können beim Registrieren auch zusätzliche Metadatentags bereitstellen, die bei der Suche nach Modellen verwendet werden können. Azure Machine Learning Service unterstützt Modelle, die mit Python 3.5.2 oder höher geladen werden können.

Sie können keine Modelle löschen, die in einer aktiven Bereitstellung verwendet werden.

Weitere Informationen finden Sie im Abschnitt „Registrieren eines Modells“ von [Bereitstellen von Modellen](how-to-deploy-and-where.md#registermodel).

Ein Beispiel für das Registrieren eines im Pickle-Format gespeicherten Modells, finden Sie unter [Tutorial: Trainieren eines Imageklassifizierungsmodells](tutorial-deploy-models-with-aml.md).

## <a name="step-2-use-the-model"></a>Schritt 2: Verwenden des Modells

Machine Learning-Modelle können als Webdienst, auf IoT Edge-Geräten oder für die Analyse in Diensten wie Power BI verwendet werden.

### <a name="web-service"></a>Webdienst

Sie können Ihre Modelle in **Webdiensten** mit den folgenden Computezielen verwenden:

* Azure Container Instances
* Azure Kubernetes Service

Um das Modell als Webdienst bereitzustellen, müssen Sie Folgendes angeben:

* Das Modell oder ein Ensemble von Modellen.
* Abhängigkeiten, die zur Nutzung des Modells erforderlich sind. Beispielsweise ein Skript, das Anforderungen akzeptiert und das Modell aufruft, Conda-Abhängigkeiten, usw.
* Eine Bereitstellungskonfiguration, die beschreibt, wie und wo das Modell bereitgestellt werden soll.

Weitere Informationen finden Sie unter [Bereitstellen von Modellen](how-to-deploy-and-where.md).

### <a name="iot-edge-devices"></a>IoT Edge-Geräte

Sie können Modelle mit IoT-Geräten über **Azure IoT Edge-Module** verwenden. IoT Edge-Module werden auf Hardwaregeräten bereitgestellt, wodurch Rückschlüsse oder Modellbewertungen auf dem jeweiligen Gerät ermöglicht werden.

Weitere Informationen finden Sie unter [Bereitstellen von Modellen](how-to-deploy-and-where.md).

### <a name="analytics"></a>Analytics

Microsoft Power BI unterstützt die Verwendung von Machine Learning-Modellen für Datenanalysen. Weitere Informationen finden Sie unter [Integration von Azure Machine Learning in Power BI (Vorschau)](https://docs.microsoft.com/power-bi/service-machine-learning-integration).

## <a name="step-3-monitor-models-and-collect-data"></a>Schritt 3: Überwachen des Modells und Sammeln von Daten

Die Überwachung ermöglicht Ihnen zu verstehen, welche Daten an Ihr Modell gesendet werden und welche Vorhersagen es zurückgibt.

Diese Informationen helfen Ihnen zu verstehen, wie Ihr Modell genutzt wird. Die gesammelten Eingabedaten können auch zum Trainieren künftiger Versionen des Modells nützlich sein.

Weitere Informationen finden Sie unter [Gewusst wie: Aktivieren der Modelldatensammlung](how-to-enable-data-collection.md).

## <a name="step-4-update-the-deployment"></a>Schritt 4: Aktualisieren der Bereitstellung

Bereitstellungen müssen explizit aktualisiert werden. Weitere Informationen finden Sie im Abschnitt „Aktualisieren“ von [Bereitstellen von Modellen](how-to-deploy-and-where.md#update).

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr darüber [wie und auf welchen Instanzen Modelle mit dem Azure Machine Learning-Dienst bereitgestellt werden können](how-to-deploy-and-where.md). Ein Bereitstellungsbeispiel finden Sie unter [Tutorial: Bereitstellen eines Bildklassifizierungsmodells in Azure Container Instances](tutorial-deploy-models-with-aml.md).

Erfahren Sie, wie Sie [durchgängige Integration und Bereitstellung von ML-Modellen mit Azure Pipelines](/azure/devops/pipelines/targets/azure-machine-learning) erstellen. 

Erfahren Sie, wie Sie Clientanwendungen und -dienste erstellen, die [ein Modell verwenden, das als Webdienst bereitgestellt wird](how-to-consume-web-service.md).
