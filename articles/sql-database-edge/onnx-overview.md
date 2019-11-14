---
title: Maschinelles Lernen und KI mit ONNX in Azure SQL Database Edge (Vorschauversion) | Microsoft-Dokumentation
description: Beim maschinellen Lernen in Azure SQL Database Edge (Vorschauversion) werden Modelle im ONNX-Format (Open Neural Network Exchange) unterstützt. ONNX ist ein offenes Format, das Sie zum Austauschen von Modellen zwischen verschiedenen Frameworks und Tools für maschinelles Lernen verwenden können.
keywords: Bereitstellen von SQL Database Edge
services: sql-database-edge
ms.service: sql-database-edge
ms.subservice: machine-learning
ms.topic: conceptual
author: ronychatterjee
ms.author: achatter
ms.reviewer: davidph
ms.date: 11/07/2019
ms.openlocfilehash: 976c849f9cb48e1c197f70d10e911216a6a7425c
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73822843"
---
# <a name="machine-learning-and-ai-with-onnx-in-sql-database-edge-preview"></a>Maschinelles Lernen und KI mit ONNX in SQL Database Edge (Vorschauversion)

Beim maschinellen Lernen in Azure SQL Database Edge (Vorschauversion) werden Modelle im [ONNX-Format (Open Neural Network Exchange)](https://onnx.ai/) unterstützt. ONNX ist ein offenes Format, das Sie zum Austauschen von Modellen zwischen verschiedenen [Frameworks und Tools für maschinelles Lernen](https://onnx.ai/supported-tools) verwenden können.

## <a name="overview"></a>Übersicht

Zum Ableiten von Machine Learning-Modellen in Azure SQL Database Edge benötigen Sie zunächst ein Modell. Hierbei kann es sich um ein vorab trainiertes Modell oder ein benutzerdefiniertes Modell handeln, das mit Ihrem bevorzugten Framework trainiert wurde. Azure SQL Database Edge unterstützt das ONNX-Format. Sie müssen das Modell in dieses Format konvertieren. Dies sollte keinerlei Auswirkungen auf die Modellgenauigkeit haben. Nachdem Sie über das ONNX-Modell verfügen, können Sie es in Azure SQL Database Edge bereitstellen und die [native Bewertung mit der T-SQL-Funktion „PREDICT“](/sql/advanced-analytics/sql-native-scoring/) verwenden.

## <a name="get-onnx-models"></a>Abrufen von ONNX-Modellen

Es gibt mehrere Möglichkeiten für das Abrufen eines Modell im ONNX-Format:

- [ONNX-Modellzoo:](https://github.com/onnx/models) Enthält viele vorab trainierte ONNX-Modelle für unterschiedliche Arten von Aufgaben, die heruntergeladen werden können und sofort einsatzbereit sind.

- [Nativer Export aus ML-Trainingsframeworks](https://onnx.ai/supported-tools): Mehrere Trainingsframeworks unterstützen native Funktionen für den Export in das ONNX-Format, sodass Sie Ihr trainiertes Modell in einer bestimmten Version des ONNX-Formats speichern können, z. B. [PyTorch](https://pytorch.org/docs/stable/onnx.html), Chainer und Caffe2. Darüber hinaus ermöglichen Modellerstellungsdienste, z. B. das [automatisierte Machine Learning-Feature in Azure Machine Learning](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-bank-marketing-all-features/auto-ml-classification-bank-marketing-all-features.ipynb) und der [Custom Vision-Dienst von Azure](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/getting-started-build-a-classifier), den ONNX-Export.

- [Konvertieren vorhandener Modelle](https://github.com/onnx/tutorials#converting-to-onnx-format): Für Frameworks, die den nativen Export nicht unterstützen, sind eigenständige Pakete zum Konvertieren der Modelle in das ONNX-Format verfügbar. Beispiele und Tutorials finden Sie unter [Konvertieren in das ONNX-Format](https://github.com/onnx/tutorials#converting-to-onnx-format). 

### <a name="supported-frameworks"></a>Unterstützte Frameworks

Mit ONNX-Konvertern können Sie Modelle, die mit anderen Machine Learning-Frameworks trainiert wurden, in das ONNX-Format konvertieren. Häufig verwendete Konverter: 

* [PyTorch](http://pytorch.org/docs/master/onnx.html)
* [TensorFlow](https://github.com/onnx/tensorflow-onnx)
* [Keras](https://github.com/onnx/keras-onnx)
* [scikit-learn](https://github.com/onnx/sklearn-onnx)
* [Core ML](https://github.com/onnx/onnxmltools)

Die vollständige Liste mit den unterstützten Frameworks finden Sie unter [Konvertieren in das ONNX-Format](https://github.com/onnx/tutorials#converting-to-onnx-format).

## <a name="limitations"></a>Einschränkungen

Derzeit werden nicht alle ONNX-Modelle von Azure SQL Database Edge unterstützt. Die Unterstützung ist auf Modelle mit **numerischen Datentypen** beschränkt:

- [int und bigint](https://docs.microsoft.com/sql/t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql5)
- [real und float](https://docs.microsoft.com/sql/t-sql/data-types/float-and-real-transact-sql).
  
Andere numerische Typen können mithilfe von [CAST und CONVERT](https://docs.microsoft.com/sql/t-sql/functions/cast-and-convert-transact-sql) in unterstützte Typen konvertiert werden.

Die Modelleingaben sollten so strukturiert werden, dass jede Eingabe in das Modell einer einzelnen Spalte in einer Tabelle entspricht. Wenn Sie z. B. einen Pandas-Datenrahmen zum Trainieren eines Modells verwenden, sollte jede Eingabe eine separate Spalte für das Modell sein.

## <a name="next-steps"></a>Nächste Schritte

- [Bereitstellen von SQL Database Edge über das Azure-Portal](deploy-portal.md)
- [Bereitstellen eines ONNX-Modells in Azure SQL Database Edge (Vorschauversion)](deploy-onnx.md)
