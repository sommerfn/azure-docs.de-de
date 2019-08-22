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
ms.openlocfilehash: c5a6f957cd3f799f9d7eff1df22df2200779ac30
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/10/2019
ms.locfileid: "68946044"
---
| Computeziel | Verwendung | GPU-Unterstützung | FPGA-Unterstützung | BESCHREIBUNG |
| ----- | ----- | ----- | ----- | ----- |
| [Lokaler&nbsp;&nbsp;Webservice](../articles/machine-learning/service/how-to-deploy-and-where.md#local) | Testen/Debuggen | Vielleicht | &nbsp; | Geeignet für eingeschränkte Tests und Problembehandlung. Die Hardwarebeschleunigung hängt von der Verwendung von Bibliotheken im lokalen System ab.
| [Notebook-VM&nbsp;Web&nbsp;dienst](../articles/machine-learning/service/how-to-deploy-and-where.md#notebookvm) | Testen/Debuggen | Vielleicht | &nbsp; | Geeignet für eingeschränkte Tests und Problembehandlung. 
| [Azure Kubernetes Service (AKS)](../articles/machine-learning/service/how-to-deploy-and-where.md#aks) | Echtzeitrückschluss |  [Ja](../articles/machine-learning/service/how-to-deploy-inferencing-gpus.md)  | [Ja](../articles/machine-learning/service/how-to-deploy-fpga-web-service.md)   |Geeignet für hochgradig skalierbare Produktionsbereitstellungen. Bietet schnelle Antwortzeiten und die automatische Skalierung von bereitgestellten Diensten. Die automatische Skalierung von Clustern wird nicht vom Azure Machine Learning SDK unterstützt. Die Knoten in Ihrem AKS-Cluster können Sie über die entsprechende Benutzeroberfläche im Azure-Portal ändern. AKS ist die einzige für die visuelle Schnittstelle verfügbare Option. |
| [Azure Container Instances (ACI)](../articles/machine-learning/service/how-to-deploy-and-where.md#aci) | Testen oder Entwickeln | &nbsp;  | &nbsp; | Ideal für CPU-basierte Workloads mit geringer Skalierung, die weniger als 48 GB RAM beanspruchen |
| [Azure Machine Learning Compute](../articles/machine-learning/service/how-to-run-batch-predictions.md) | (Vorschau) &nbsp;Batchrückschluss | Ja | &nbsp;  | Ausführen von Batchbewertungen auf serverlosen Computezielen. Unterstützt virtuelle Computer mit normaler und niedriger Priorität. |
| [Azure IoT Edge](../articles/machine-learning/service/how-to-deploy-and-where.md#iotedge) | (Vorschau) IoT-&nbsp;Modul |  &nbsp; | &nbsp; | Bereitstellen und Verarbeiten von ML-Modellen auf IoT-Geräten. |
| [Azure Data Box Edge](../articles/databox-online/data-box-edge-overview.md)   | über IoT Edge |  &nbsp; | Ja | Bereitstellen und Verarbeiten von ML-Modellen auf IoT-Geräten. |
