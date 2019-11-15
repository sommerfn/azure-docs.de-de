---
title: 'Computeziele: Ort zum Trainieren und Bereitstellen von Modellen'
titleSuffix: Azure Machine Learning
description: Legen Sie fest, wo Sie Ihr Modell mit Azure Machine Learning trainieren oder bereitstellen möchten.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 11/04/2019
ms.openlocfilehash: 75e789ac1f9c161bc67ba90062be39f986f5b140
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73580582"
---
#  <a name="what-are-compute-targets-in-azure-machine-learning"></a>Was sind Computeziele in Azure Machine Learning? 

Ein **Computeziel** ist eine festgelegte Computeressource/-Umgebung, in der Ihr Trainingsskript ausgeführt oder Ihre Dienstbereitstellung gehostet wird. Hierbei kann es sich um Ihren lokalen Computer oder eine cloudbasierte Computeressource handeln. Mithilfe von Computezielen können Sie Ihre Compute-Umgebung später problemlos ändern, ohne den Code anpassen zu müssen.  

In einem typischen Modellentwicklungslebenszyklus gehen Sie unter Umständen wie folgt vor:
1. Sie entwickeln und experimentieren zunächst mit einer kleinen Datenmenge. In dieser Phase wird empfohlen, Ihre lokale Umgebung (etwa Ihren lokalen Computer oder einen cloudbasierten virtuellen Computer) als Computeziel zu verwenden. 
2. Skalieren Sie Ihre Umgebung für größere Datenmengen zentral hoch, oder führen Sie ein verteiltes Training mithilfe eines dieser [Trainingscomputeziele](#train) durch.  
3. Wenn Ihr Modell bereit ist, stellen Sie es in einer Webhostingumgebung oder auf einem IoT-Gerät mit einem [dieser Bereitstellungscomputeziele](#deploy) bereit.

Die Computeressourcen, die Sie für Ihre Computeziele verwenden, werden an einen [Arbeitsbereich](concept-workspace.md) angefügt. Andere Computeressourcen als der lokale Computer werden von Benutzern des Arbeitsbereichs gemeinsam genutzt.

## <a name="train"></a> Trainieren von Computezielen

Azure Machine Learning bietet unterschiedliche Unterstützung für verschiedene Computeressourcen.  Sie können auch Ihre eigene Computeressource anfügen. Die Unterstützung für verschiedene Szenarien kann jedoch variieren.

[!INCLUDE [aml-compute-target-train](../../../includes/aml-compute-target-train.md)]

Erfahren Sie mehr über das [Einrichten und Verwenden eines Computeziels für das Modelltraining](how-to-set-up-training-targets.md).

## <a name="deploy"></a>Bereitstellungsziele

Die folgenden Computeressourcen können zum Hosten Ihrer Modellimplementierung verwendet werden.

[!INCLUDE [aml-compute-target-deploy](../../../includes/aml-compute-target-deploy.md)]

Erfahren Sie, [wo und wie Sie Ihr Modell auf einem Computeziel bereitstellen](how-to-deploy-and-where.md).

<a name="amlcompute"></a>
## <a name="azure-machine-learning-compute-managed"></a>Azure Machine Learning-Computeressource (verwaltet)

Eine verwaltete Computeressource wird von Azure Machine Learning erstellt und verwaltet. Diese Computeressource ist für Machine Learning-Workloads optimiert. Azure Machine Learning-Computecluster. Möglicherweise werden künftig weitere verwaltete Computeressourcen hinzugefügt.

Sie können Azure Machine Learning-Computecluster in Azure Machine Learning Studio, im Azure-Portal, mit dem SDK, mit einer Resource Manager-Vorlage oder mit der CLI erstellen.

Nach der Erstellung sind diese Computeressourcen im Gegensatz zu anderen Arten von Computezielen automatisch Teil Ihres Arbeitsbereichs.

Sie können Azure Machine Learning-Computecluster für zum Trainieren und für Batchrückschlüsse (Vorschau) verwenden.  Diese Computeressource bietet Folgendes:

* Cluster mit einem oder mehreren Knoten
* Automatische Skalierung bei jedem Übermitteln einer Ausführung 
* Automatische Clusterverwaltung und Auftragsplanung 
* Unterstützt CPU- und GPU-Ressourcen



## <a name="unmanaged-compute"></a>Nicht verwaltete Computeressourcen

Ein nicht verwaltetes Computeziel wird *nicht* von Azure Machine Learning verwaltet. Sie erstellen diese Art von Computeziel außerhalb von Azure Machine Learning und fügen es anschließend an Ihren Arbeitsbereich an. Für nicht verwaltete Computeressourcen sind möglicherweise weitere Schritte erforderlich, um die Leistung von Machine Learning-Workloads beizubehalten oder zu verbessern.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel werden folgende Themen erläutert:
* [Einrichten eines Computeziels für das Modelltraining](how-to-set-up-training-targets.md)
* [Bereitstellen Ihres Modells auf einem Computeziel](how-to-deploy-and-where.md)