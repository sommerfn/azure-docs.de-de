---
title: Maschinelles Lernen und KI mit ONNX in Azure SQL Database Edge (Vorschauversion) | Microsoft-Dokumentation
description: Beim maschinellen Lernen in Azure SQL Database Edge (Vorschauversion) werden Modelle im ONNX-Format (Open Neural Network Exchange) unterstützt. ONNX ist ein offenes Format, das Sie zum Austauschen von Modellen zwischen verschiedenen Frameworks und Tools für maschinelles Lernen verwenden können.
keywords: Bereitstellen von SQL Database Edge
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: conceptual
author: ronychatterjee
ms.author: achatter
ms.reviewer: davidph
ms.date: 11/04/2019
ms.openlocfilehash: c4c87f7f6f8735c9a50c61b0e083c77b915e0d98
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510647"
---
# <a name="machine-learning-and-ai-with-onnx-in-sql-database-edge-preview"></a>Maschinelles Lernen und KI mit ONNX in SQL Database Edge (Vorschauversion)

Beim maschinellen Lernen in Azure SQL Database Edge (Vorschauversion) werden Modelle im [ONNX-Format (Open Neural Network Exchange)](https://onnx.ai/) unterstützt. ONNX ist ein offenes Format, das Sie zum Austauschen von Modellen zwischen verschiedenen [Frameworks und Tools für maschinelles Lernen](https://onnx.ai/supported-tools) verwenden können.

## <a name="supported-tool-kits"></a>Unterstützte Toolkits

ONNXMLTools ermöglicht das Konvertieren von Modellen aus verschiedenen Toolkits zum maschinellen Lernen in ein ONNX-Modell. Für numerische Datentypen und einspaltige Eingaben werden derzeit die folgenden Toolkits unterstützt:

* [scikit-learn](https://github.com/onnx/sklearn-onnx)
* [TensorFlow](https://github.com/onnx/tensorflow-onnx)
* [Keras](https://github.com/onnx/keras-onnx)
* [Core ML](https://github.com/onnx/onnxmltools)
* [Spark ML (experimentell)](https://github.com/onnx/onnxmltools/tree/master/onnxmltools/convert/sparkml)
* [LightGBM](https://github.com/onnx/onnxmltools)
* [libsvm](https://github.com/onnx/onnxmltools)
* [XGBoost](https://github.com/onnx/onnxmltools)

## <a name="get-onnx-models"></a>Abrufen von ONNX-Modellen

Es gibt mehrere Möglichkeiten für das Abrufen eines Modell im ONNX-Format:

- [ONNX-Modellzoo:](https://github.com/onnx/models) Enthält mehrere vortrainierte ONNX-Modelle für verschiedene Arten von Aufgaben. Sie können Versionen, die von Windows Machine Learning unterstützt werden, herunterladen und verwenden.

- [Nativer Export aus einem Trainingsframework für maschinelles Lernen:](https://onnx.ai/supported-tools) Mehrere Trainingsframeworks unterstützen native Exportfunktionen in das ONNX-Format, sodass Sie Ihr trainiertes Modell in einer bestimmten Version des ONNX-Formats speichern können. Beispiele: Chainer, Caffee2 und PyTorch. Außerdem bieten auch Dienste wie [Azure Machine Learning Service](https://azure.microsoft.com/services/machine-learning-service/) und [Azure Custom Vision](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/getting-started-build-a-classifier) einen nativen ONNX-Export.

  - Informationen zum Trainieren und Exportieren eines ONNX-Modells in der Cloud mithilfe von Custom Vision finden Sie unter [Tutorial: Verwenden eines ONNX-Modells von Custom Vision mit Windows ML (Vorschauversion)](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/custom-vision-onnx-windows-ml).

- [Konvertieren vorhandener Modelle mithilfe von WinMLTools:](https://docs.microsoft.com/windows/ai/windows-ml/convert-model-winmltools) Dieses Python-Paket ermöglicht das Konvertieren von Modellen aus Formaten verschiedener Trainingsframework in das ONNX-Format. Sie können je nach den Windows-Builds, auf die Ihre Anwendung abzielt, die ONNX-Zielversion angeben, in die das Modell konvertiert werden soll. Wenn Sie nicht mit Python vertraut sind, können Sie das [Windows Machine Learning-Dashboard mit Benutzeroberfläche](https://github.com/Microsoft/Windows-Machine-Learning/tree/master/Tools/WinMLDashboard) verwenden, um Ihre Modelle zu konvertieren.

> [!IMPORTANT]
> Nicht alle ONNX-Versionen werden von Azure SQL Database Edge unterstützt. Derzeit wird nur die Vorhersage numerischer Datentypen durch das ONNX-Modell unterstützt.

Wenn Sie über ein ONNX-Modell verfügen, können Sie es in Azure SQL Database Edge bereitstellen. Anschließend können Sie die [native Bewertung mit der T-SQL-Funktion PREDICT](/sql/advanced-analytics/sql-native-scoring/) verwenden.

## <a name="limitations"></a>Einschränkungen

Derzeit ist diese Unterstützung auf Modelle mit **numerischen Datentypen** beschränkt:

- [int und bigint](https://docs.microsoft.com/sql/t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql5)
- [real und float](https://docs.microsoft.com/sql/t-sql/data-types/float-and-real-transact-sql).
  
Andere numerische Typen können mithilfe von [CAST und CONVERT](https://docs.microsoft.com/sql/t-sql/functions/cast-and-convert-transact-sql) in unterstützte Typen konvertiert werden.

Die Modelleingaben sollten so strukturiert werden, dass jede Eingabe in das Modell einer einzelnen Spalte in einer Tabelle entspricht. Wenn Sie z. B. einen Pandas-Datenrahmen zum Trainieren eines Modells verwenden, sollte jede Eingabe eine separate Spalte für das Modell sein.

## <a name="next-steps"></a>Nächste Schritte

- [Bereitstellen von SQL Database Edge über das Azure-Portal](deploy-portal.md)
- [Bereitstellen eines ONNX-Modells in Azure SQL Database Edge (Vorschauversion)](deploy-onnx.md)