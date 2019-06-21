---
title: Computeziele
titleSuffix: Azure Machine Learning service
description: Mit einem Computeziel können Sie die Computeressource angeben, auf der Ihr Trainingsskript ausgeführt oder Ihre Dienstbereitstellung gehostet wird. Hierbei kann es sich um Ihren lokalen Computer oder eine cloudbasierte Computeressource handeln.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 05/30/2019
ms.openlocfilehash: 42c0f5460a63b781aafdd43410761e2d7b17944d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "66755474"
---
#  <a name="what-is-a-compute-target-in-azure-machine-learning-service"></a>Was ist ein Computeziel in Azure Machine Learning Service? 

Mit einem Computeziel können Sie die Computeressource angeben, auf der Ihr Trainingsskript ausgeführt oder Ihre Dienstbereitstellung gehostet wird. Hierbei kann es sich um Ihren lokalen Computer oder eine cloudbasierte Computeressource handeln.

Mithilfe von Computezielen können Sie Ihre Compute-Umgebung ohne Änderung des Codes problemlos ändern.  Ein typischer Modellentwicklungslebenszyklus läuft folgendermaßen ab:

* Beginnen Sie mit der Entwicklung/Experimenten mit einer kleinen Menge von Daten. In dieser Phase empfehlen wir die Verwendung einer lokalen Umgebung. Verwenden Sie beispielsweise Ihren lokalen Computer oder eine cloudbasierte VM.
* Skalieren Sie Ihr Training für größere Datasets zentral hoch, oder führen Sie ein verteiltes Training mithilfe eines der [Trainingsziele](#train) durch.  
* Sorgen Sie für eine Bereitstellung in mehreren Webhostingumgebungen oder auf IoT-Geräten mithilfe eines der [Bereitstellungsziele](#deploy).

Die Computeressourcen, die Sie für Ihre Computeziele verwenden, werden an einen [Arbeitsbereich](concept-workspace.md) angefügt. Andere Computeressourcen als der lokale Computer werden von Benutzern des Arbeitsbereichs gemeinsam genutzt.

## <a name="train"></a> Trainingsziele

Azure Machine Learning Service bietet unterschiedliche Unterstützung für die verschiedenen Computeressourcen.  Sie können auch Ihre eigene Computeressource anfügen. Die Unterstützung für verschiedene Szenarien variiert jedoch wie unten beschrieben:

[!INCLUDE [aml-compute-target-train](../../../includes/aml-compute-target-train.md)]


## <a name="deploy"></a>Bereitstellungsziele

Die folgenden Computeressourcen können zum Hosten Ihrer Modellimplementierung verwendet werden.

[!INCLUDE [aml-compute-target-deploy](../../../includes/aml-compute-target-deploy.md)]


## <a name="managed-compute"></a>Verwaltete Computeressourcen

Eine verwaltete Computeressource wird von Azure Machine Learning Service erstellt und verwaltet. Diese Computeressource ist für Machine Learning-Workloads optimiert. Azure Machine Learning Compute ist ab dem 30. Mai 2019 die einzige verwaltete Computeressource. Möglicherweise werden künftig weitere verwaltete Computeressourcen hinzugefügt.

### <a name="amlcompute"></a> Azure Machine Learning Compute

Sie können Azure Machine Learning Compute für das Training und für Batchrückschlüsse (Vorschau) verwenden.  Diese Computeressource bietet Folgendes:

* Cluster mit einem oder mehreren Knoten
* Automatische Skalierung bei jedem Übermitteln einer Ausführung 
* Automatische Clusterverwaltung und Auftragsplanung 
* Unterstützt CPU- und GPU-Ressourcen

Sie können Azure Machine Learning Compute-Instanzen mit einer der folgenden Komponenten erstellen:

* Das Azure-Portal
* Azure Machine Learning SDK
* Die Azure-CLI

Alle anderen Computeressourcen müssen außerhalb des Arbeitsbereichs erstellt und dann angefügt werden.

## <a name="unmanaged-compute"></a>Nicht verwaltete Computeressourcen

Eine nicht verwaltete Computeressource wird von Azure Machine Learning Service *nicht* verwaltet. Sie erstellen diese Art von Computeressource außerhalb von Azure Machine Learning und fügen sie anschließend an Ihren Arbeitsbereich an. Für nicht verwaltete Computeressourcen sind möglicherweise weitere Schritte erforderlich, um die Leistung von Machine Learning-Workloads beizubehalten oder zu verbessern.

## <a name="next-steps"></a>Nächste Schritte

* [Einrichten von Computezielen für das Modelltraining](how-to-set-up-training-targets.md)
* [Bereitstellen von Modellen mit Azure Machine Learning Service](how-to-deploy-and-where.md)