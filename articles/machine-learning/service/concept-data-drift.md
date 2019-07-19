---
title: Überwachung von Datenabweichungen (Vorschauversion)
titleSuffix: Azure Machine Learning service
description: Es wird beschrieben, wie Sie mit Azure Machine Learning Service eine Überwachung von Datenabweichungen durchführen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: cody-dkdc
ms.author: copeters
ms.date: 06/20/2019
ms.openlocfilehash: a03e3124647869e7148f271810bb523986a851c6
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67442384"
---
# <a name="what-is-data-drift-monitoring-preview"></a>Was ist die Überwachung von Datenabweichungen (Vorschauversion)?

Bei Datenabweichungen ändert sich die Verteilung von Daten. Im Kontext des maschinellen Lernens kann es für trainierte Machine Learning-Modelle aufgrund von Datenabweichungen zu einer Beeinträchtigung der Vorhersageleistung kommen. Die Überwachung der Abweichung zwischen Trainingsdaten und Daten, die für Vorhersagen verwendet werden, kann zur Erkennung von Leistungsproblemen beitragen.

Machine Learning-Modelle sind nur so gut wie die Daten, die zum Trainieren verwendet werden. Die Bereitstellung von Modellen in der Produktion ohne Überwachung der Leistung kann unerkannte Probleme und negative Auswirkungen nach sich ziehen. Mit der Überwachung von Datenabweichungen können Sie diese erkennen und entsprechende Anpassungen vornehmen. 

## <a name="when-to-monitor-for-data-drift"></a>Wann sollte die Überwachung von Datenabweichungen durchgeführt werden?

Beispiele für Metriken, die überwacht werden können:

+ Größenordnung der Abweichung (Abweichungskoeffizient)
+ Ursache der Abweichung (Abweichungsbeitrag nach Funktion)
+ Abstandsmetriken (Wasserstein, Energie usw.)

Bei Verwendung dieser Überwachung können Warnungen oder Aktionen eingerichtet werden, wenn Abweichungen erkannt werden, und der Data Scientist kann die Grundursache des Problems untersuchen. 

Wenn Sie vermuten, dass sich die Eingabedaten für Ihr bereitgestelltes Modell ggf. ändern, sollten Sie die Verwendung einer Erkennung von Datenabweichungen erwägen.

## <a name="how-data-drift-is-monitored-in-azure-machine-learning-service"></a>Überwachen von Datenabweichungen in Azure Machine Learning Service

Mit **Azure Machine Learning Service** werden Datenabweichungen anhand von Datasets oder Bereitstellungen überwacht. Zum Überwachen von Datenabweichungen wird ein Baseline-Dataset angegeben. Dies ist normalerweise das Trainingsdataset für ein Modell. Ein zweites Dataset – normalerweise gesammelte Modelleingabedaten einer Bereitstellung – wird basierend auf dem Baseline-Dataset getestet. Für beide Datasets wird eine [Profilerstellung](how-to-explore-prepare-data.md#explore-with-summary-statistics) durchgeführt, und sie werden in den Dienst für die Überwachung von Datenabweichungen eingegeben. Ein Machine Learning-Modell wird trainiert, um Unterschiede zwischen den beiden Datasets zu ermitteln. Die Leistung des Modells wird in den Abweichungskoeffizienten konvertiert, mit dem die Größenordnung der Abweichung zwischen den beiden Datasets gemessen wird. Mit der [Modellinterpretierbarkeit](machine-learning-interpretability-explainability.md) werden die Features berechnet, die zum Abweichungskoeffizienten beigetragen haben. Über das Datasetprofil werden statistische Informationen zu jedem Feature nachverfolgt. 

## <a name="data-drift-metric-output"></a>Ausgabe der Metriken für Datenabweichungen

Es gibt mehrere Möglichkeiten, Metriken für Datenabweichungen anzuzeigen:

* Verwenden Sie das Jupyter-Widget.
* Verwenden Sie die Funktion `get_metrics()` für jedes `datadriftRun`-Objekt.
* Anzeigen der Metriken für Ihr Modell im Azure-Portal

Bei jeder Ausführungsiteration werden die unten angegebenen Metriken für eine Datenabweichungsaufgabe gespeichert.

|Metrik|BESCHREIBUNG|
--|--|
wasserstein_distance|Statistischer Abstand, der für die eindimensionale numerische Verteilung definiert ist.|
energy_distance|Statistischer Abstand, der für die eindimensionale numerische Verteilung definiert ist.|
datadrift_coefficient|Formal der Matthews-Korrelationskoeffizient (eine reelle Zahl zwischen -1 und 1). Im Kontext von Datenabweichungen steht „0“ für keine Abweichung und „1“ für die maximale Abweichung.|
datadrift_contribution|Gewichtung der Features, die zur Abweichung beitragen.|

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die Beispiele an, und informieren Sie sich über das Überwachen von Datenabweichungen:

+ [Informieren Sie sich über das Überwachen von Datenabweichungen für Modelle, die per Azure Kubernetes Service (AKS) bereitgestellt werden](how-to-monitor-data-drift.md).
+ Testen Sie [Jupyter Notebook-Beispiele](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/data-drift/).