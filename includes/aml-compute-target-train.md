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
ms.date: 05/30/2019
ms.openlocfilehash: 50905eb987defac612f1055b450b682726f0a56f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "66752961"
---
|Trainingsziele| GPU-Unterstützung |[Automatisiertes Machine Learning](../articles/machine-learning/service/concept-automated-ml.md) | [ML-Pipelines](../articles/machine-learning/service/concept-ml-pipelines.md) | [Grafische Benutzeroberfläche](../articles/machine-learning/service/ui-concept-visual-interface.md)
|----|:----:|:----:|:----:|:----:|
|[Lokaler Computer](../articles/machine-learning/service/how-to-set-up-training-targets.md#local)| Vielleicht | Ja | &nbsp; | &nbsp; |
|[Azure Machine Learning Compute](../articles/machine-learning/service/how-to-set-up-training-targets.md#amlcompute)| Ja | Ja & <br/>Hyperparameteroptimierung&nbsp; | Ja | Ja |
|[Remote-VM](../articles/machine-learning/service/how-to-set-up-training-targets.md#vm) |Ja | Ja & <br/>Hyperparameteroptimierung | Ja | &nbsp; |
|[Azure&nbsp;Databricks](../articles/machine-learning/service/how-to-create-your-first-pipeline.md#databricks)| &nbsp; | Ja | Ja | &nbsp; |
|[Azure Data Lake Analytics](../articles/machine-learning/service/how-to-create-your-first-pipeline.md#adla)| &nbsp; | &nbsp; | Ja | &nbsp; |
|[Azure HDInsight](../articles/machine-learning/service/how-to-set-up-training-targets.md#hdinsight)| &nbsp; | &nbsp; | Ja | &nbsp; |
|[Azure Batch](../articles/machine-learning/service/how-to-set-up-training-targets.md#azbatch)| &nbsp; | &nbsp; | Ja | &nbsp; |

**Alle Computeziele können für mehrere Trainingsaufträge wiederverwendet werden**. Beispielsweise können Sie einen virtuellen Remotecomputer, den Sie an Ihren Arbeitsbereich anfügen, für mehrere Aufträge wiederverwenden.