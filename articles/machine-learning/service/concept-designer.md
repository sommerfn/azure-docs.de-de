---
title: Entwickeln von ML-Modellen mit dem Designer
titleSuffix: Azure Machine Learning
description: Hier erhalten Sie Informationen zu Terminologie, Konzepten und Workflows des Designers für Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 11/04/2019
ms.openlocfilehash: ee97322e58fe7ab3a1474f55c6294822b8ce90da
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73511831"
---
# <a name="what-is-azure-machine-learning-designer-preview"></a>Was ist der Azure Machine Learning-Designer (Vorschau)? 
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-enterprise-sku.md)]

Der Designer für Azure Machine Learning unterstützt Sie bei der Datenaufbereitung für und beim Trainieren, Testen, Bereitstellen, Verwalten und Überwachen von Machine Learning-Modellen – ganz ohne Programmierung.

Für das Erstellen Ihres Modells ist keine Programmierung erforderlich, sondern einfach nur das visuelle Verbinden von [Datasets](#datasets) und [Modulen](#module).

Der Designer verwendet Ihren Azure Machine Learning-[Arbeitsbereich](concept-workspace.md) zu folgenden Zwecken:

+ Erstellen, Bearbeiten und Ausführen von [Pipelines](#pipeline) im Arbeitsbereich
+ Zugriff auf [Datasets](#datasets)
+ Verwenden der [Computeressourcen](#compute) im Arbeitsbereich, um die Pipeline auszuführen 
+ Registrieren von [Modellen](concept-azure-machine-learning-architecture.md#models)
+ [Veröffentlichen](#publish) von Pipelines als REST-Endpunkte
+ [Bereitstellen](#deployment) von Modellen als Pipelineendpunkte (für Batchrückschlüsse) oder Echtzeitendpunkte für Computeressourcen im Arbeitsbereich

![Übersicht über den Designer](media/ui-concept-visual-interface/overview.png)

## <a name="workflow"></a>Workflow

Der Designer stellt eine interaktive, visuelle Canvas bereit, um ein Modell schnell erstellen, testen und durchlaufen zu können. 

+ [Datasets](#datasets) und [Module](#module) können per Drag & Drop-Verfahren auf der Canvas platziert werden.
+ Verbinden Sie die Module miteinander, um eine [Pipeline](#pipeline) zu erstellen.
+ Führen Sie die Pipeline mithilfe der Computeressource des Machine Learning Service-Arbeitsbereichs aus.
+ Durchlaufen Sie Ihren Modellentwurf, indem Sie die Pipeline bearbeiten und erneut ausführen.
+ Wenn Sie dazu bereit sind, konvertieren Sie Ihre **Trainingspipeline** in eine **Rückschlusspipeline**.
+ [Veröffentlichen](#publish) Sie Ihre Pipeline als REST-Endpunkt, wenn Sie sie ohne den zur Erstellung verwendeten Python-Code erneut übermitteln möchten.
+ Führen Sie eine [Bereitstellung](#deployment) der Rückschlusspipeline als Pipelineendpunkt oder Echtzeitendpunkt durch, damit andere Benutzer auf Ihr Modell zugreifen können.

## <a name="pipeline"></a>Pipeline

Sie können eine ML-[Pipeline](concept-azure-machine-learning-architecture.md#ml-pipelines) von Grund auf neu erstellen, oder Sie können eine vorhandene Beispielpipeline als Vorlage verwenden. Jedes Mal, wenn Sie eine Pipeline ausführen, werden Artefakte in Ihrem Arbeitsbereich gespeichert. Pipelineausführungen werden in [Experimenten](concept-azure-machine-learning-architecture.md#experiments) gruppiert.

Eine Pipeline besteht aus Datasets und Analysemodulen, die Sie miteinander verbinden, um ein Modell zu konstruieren. Eine gültige Pipeline weist diese Merkmale auf:

* Datasets können nur mit Modulen verbunden sein.
* Module können entweder mit Datasets oder mit anderen Modulen verbunden sein.
* Alle Eingangsports für Module müssen eine Verbindung zum Datenfluss aufweisen.
* Alle erforderlichen Parameter der einzelnen Module müssen festgelegt sein.


Informationen zu den ersten Schritten mit dem Designer finden Sie hier: [Tutorial: Prognostizieren von Automobilpreisen mit dem Designer](tutorial-designer-automobile-price-train-score.md).

## <a name="datasets"></a>Datasets

Ein Azure Machine Learning-Dataset erleichtert Ihnen den Zugriff auf Ihre Daten und die Arbeit damit. Im Designer sind einige Beispieldatasets enthalten, mit denen Sie experimentieren können. Sie können bei Bedarf weitere Datasets [registrieren](./how-to-create-register-datasets.md).

## <a name="module"></a>Modul

Ein Modul ist ein Algorithmus, den Sie auf Ihre Daten anwenden können. Der Designer verfügt über eine Reihe von Modulen, die von Funktionen für die Datenerfassung bis zu Trainings-, Bewertungs- und Überprüfungsvorgängen reichen.

Ein Modul kann eine Reihe von Parametern haben, die Sie zum Konfigurieren der internen Algorithmen des Moduls einsetzen können. Wenn Sie ein Modul auf der Canvas auswählen, werden dessen Parameter rechts neben der Canvas im Bereich „Eigenschaften“ angezeigt. Sie können die Parameter in diesem Bereich zur Abstimmung Ihres Modells verändern.

![Moduleigenschaften](media/ui-concept-visual-interface/properties.png)

Unterstützung bei der Navigation durch die Bibliothek der verfügbaren Machine Learning-Algorithmen finden Sie unter [Algorithmen und Module: Referenzübersicht](../algorithm-module-reference/module-reference.md).

## <a name="compute"></a> Computeressourcen

Verwenden Sie Computeressourcen aus Ihrem Arbeitsbereich, um Ihre Pipeline auszuführen und Ihre bereitgestellten Modelle als Echtzeitendpunkte oder Pipelineendpunkte (für Batchrückschlüsse) zu hosten. Die unterstützten Computeziele lauten:

| Computeziel | Training | Bereitstellung |
| ---- |:----:|:----:|
| Azure Machine Learning Compute | ✓ | |
| Azure Kubernetes Service | | ✓ |

Computeziele sind in Ihren Machine Learning-[Workspace](concept-workspace.md) integriert. Ihre Computeziele verwalten Sie in Ihrem Arbeitsbereich im [Azure Machine Learning-Studio](https://ml.azure.com).

## <a name="publish"></a>Veröffentlichen

Sobald Ihre Pipeline bereit ist, können Sie sie als REST-Endpunkt veröffentlichen. Eine [PublishedPipeline](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.graph.publishedpipeline?view=azure-ml-py) kann ohne den für ihre Erstellung verwendeten Python-Code übermittelt werden.

Zusätzlich kann eine PublishedPipeline verwendet werden, um eine Pipeline mit anderen PipelineParameter-Werten und -Eingaben erneut zu übermitteln.

## <a name="deployment"></a>Bereitstellung

Sobald Ihr Vorhersagemodell bereit ist, können Sie es direkt aus dem Designer als Pipelineendpunkt oder Echtzeitendpunkt bereitstellen.

Der Pipelineendpunkt ist eine PublishedPipeline, mit der Sie für Batchrückschlüsse eine Pipelineausführung mit anderen PipelineParameter-Werten und -Eingaben übermitteln können.

Der Echtzeitendpunkt stellt eine Schnittstelle zwischen einer Anwendung und Ihrem Bewertungsmodell zur Verfügung. Eine externe Anwendung kann in Echtzeit mit einem Bewertungsmodell kommunizieren. Durch den Aufruf eines Echtzeitendpunkts werden Vorhersageergebnisse an eine externe Anwendung zurückgegeben. Für einen Aufruf eines Echtzeitendpunkts übergeben Sie einen API-Schlüssel, der beim Bereitstellen des Endpunkts erstellt wurde. Der Endpunkt basiert auf REST, einer gängigen Architektur für Webprogrammierungsprojekte.

Informationen dazu, wie Sie Ihr Modell bereitstellen, finden Sie unter [Tutorial: Bereitstellen eines Machine Learning-Modells mit dem Designer](tutorial-designer-automobile-price-deploy.md).

## <a name="moving-from-the-visual-interface-to-the-designer"></a>Die grafische Benutzeroberfläche wird zum Designer

Die grafische Benutzeroberfläche (Vorschau) wurde aktualisiert und ist jetzt der Azure Machine Learning-Designer (Vorschau). Der Designer wurde umgestaltet und verwendet jetzt ein pipelinebasiertes Back-End, das vollständig in die weiteren Features von Azure Machine Learning integriert ist. 

Aufgrund dieser Aktualisierungen wurden einige Konzepte und Begriffe für die grafische Benutzeroberfläche geändert oder umbenannt. Die nachstehende Tabelle zeigt die wichtigsten konzeptionellen Änderungen. 

| Konzept im Designer | Bisher in der grafischen Benutzeroberfläche |
| ---- |:----:|
| Pipelineentwurf | Experiment |
| Echtzeitendpunkt | Webdienst |

### <a name="migrating-to-the-designer"></a>Migration zum Designer

Sie können vorhandene Experimente für die grafische Benutzeroberfläche und Webdienste in Pipelines und Echtzeitendpunkte im Designer konvertieren. Migrieren Sie Ihre Ressourcen für die grafische Benutzeroberfläche mithilfe der folgenden Schritte:

[!INCLUDE [migrate from the visual interface](../../../includes/aml-vi-designer-migration.md)]


## <a name="next-steps"></a>Nächste Schritte

* Die Grundlagen von Predictive Analytics und maschinellem Lernen werden hier erläutert: [Tutorial: Prognostizieren von Automobilpreisen mit dem Designer](tutorial-designer-automobile-price-train-score.md).
* Verwenden Sie eines der Beispiele und passen Sie es an Ihre Anforderungen an:

- [Beispiel 1 – Regression: Vorhersagen des Preises eines Autos](how-to-designer-sample-regression-automobile-price-basic.md)
- [Beispiel 2 – Regression: Vergleichen von Algorithmen für die Vorhersage von Autopreisen](how-to-designer-sample-regression-automobile-price-compare-algorithms.md)
- [Beispiel 3 – Klassifizierung mit Featureauswahl: Vorhersage des Einkommens](how-to-designer-sample-classification-predict-income.md)
- [Beispiel 4 – Klassifizierung: Vorhersagen des Kreditrisikos (kostensensibel)](how-to-designer-sample-classification-credit-risk-cost-sensitive.md)
- [Beispiel 5 – Klassifizierung: Vorhersage der Kundenabwanderung](how-to-designer-sample-classification-churn.md)
- [Beispiel 6 – Klassifizierung: Vorhersage von Flugverspätungen](how-to-designer-sample-classification-flight-delay.md)
- [Beispiel 7 – Textklassifizierung: Wikipedia SP 500-Dataset](how-to-designer-sample-text-classification.md)

