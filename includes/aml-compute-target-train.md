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
ms.openlocfilehash: aa1120db5451583153928bd2cbc383a46781a267
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73489589"
---
**Computeziele können für mehrere Trainingsaufträge wiederverwendet werden.** Beispielsweise können Sie einen virtuellen Remotecomputer, den Sie an Ihren Arbeitsbereich anfügen, für mehrere Aufträge wiederverwenden.

|Trainings&nbsp;ziele| GPU-Unterstützung |[Automatisiertes Machine Learning](../articles/machine-learning/service/concept-automated-ml.md) | [ML-Pipelines](../articles/machine-learning/service/concept-ml-pipelines.md) | [Azure Machine Learning-Designer](../articles/machine-learning/service/concept-designer.md)
|----|:----:|:----:|:----:|:----:|
|[Lokaler Computer](../articles/machine-learning/service/how-to-set-up-training-targets.md#local)| Vielleicht | Ja | &nbsp; | &nbsp; |
|[Azure Machine Learning Compute-Instanz](../articles/machine-learning/service/concept-compute-instance.md)| Ja | | Ja |  |
|[Azure Machine Learning Compute-Cluster](../articles/machine-learning/service/how-to-set-up-training-targets.md#amlcompute)| Ja | Ja & <br/>Hyperparameteroptimierung&nbsp; | Ja | Ja |
|[Remote-VM](../articles/machine-learning/service/how-to-set-up-training-targets.md#vm) |Ja | Ja & <br/>Hyperparameteroptimierung | Ja | &nbsp; |
|[Azure&nbsp;Databricks](../articles/machine-learning/service/how-to-create-your-first-pipeline.md#databricks)| &nbsp; | Ja | Ja | &nbsp; |
|[Azure Data Lake Analytics](../articles/machine-learning/service/how-to-create-your-first-pipeline.md#adla)| &nbsp; | &nbsp; | Ja | &nbsp; |
|[Azure HDInsight](../articles/machine-learning/service/how-to-set-up-training-targets.md#hdinsight)| &nbsp; | &nbsp; | Ja | &nbsp; |
|[Azure Batch](../articles/machine-learning/service/how-to-set-up-training-targets.md#azbatch)| &nbsp; | &nbsp; | Ja | &nbsp; |
