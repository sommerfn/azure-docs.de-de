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
ms.date: 08/23/2019
ms.openlocfilehash: e402989c6bdbbc90ae10047fcf19f4f2d74328a6
ms.sourcegitcommit: dcf3e03ef228fcbdaf0c83ae1ec2ba996a4b1892
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/23/2019
ms.locfileid: "70014766"
---
| Computeziel | Verwendung | GPU-Unterstützung | FPGA-Unterstützung | BESCHREIBUNG |
| ----- | ----- | ----- | ----- | ----- |
| [Lokaler&nbsp;&nbsp;Webservice](../articles/machine-learning/service/how-to-deploy-and-where.md#local) | Testen/Debuggen | &nbsp; | &nbsp; | Geeignet für eingeschränkte Tests und Problembehandlung. Die Hardwarebeschleunigung hängt von der Verwendung von Bibliotheken im lokalen System ab.
| [Notebook-VM&nbsp;Web&nbsp;dienst](../articles/machine-learning/service/how-to-deploy-and-where.md#notebookvm) | Testen/Debuggen | &nbsp; | &nbsp; | Geeignet für eingeschränkte Tests und Problembehandlung. 
| [Azure Kubernetes Service (AKS)](../articles/machine-learning/service/how-to-deploy-and-where.md#aks) | Echtzeitrückschluss |  [Ja](../articles/machine-learning/service/how-to-deploy-inferencing-gpus.md)  | [Ja](../articles/machine-learning/service/how-to-deploy-fpga-web-service.md)   |Geeignet für hochgradig skalierbare Produktionsbereitstellungen. Bietet schnelle Antwortzeiten und die automatische Skalierung von bereitgestellten Diensten. Die automatische Skalierung von Clustern wird nicht vom Azure Machine Learning SDK unterstützt. Die Knoten in Ihrem AKS-Cluster können Sie über die entsprechende Benutzeroberfläche im Azure-Portal ändern. AKS ist die einzige für die visuelle Schnittstelle verfügbare Option. |
| [Azure Container Instances (ACI)](../articles/machine-learning/service/how-to-deploy-and-where.md#aci) | Testen oder Entwickeln | &nbsp;  | &nbsp; | Ideal für CPU-basierte Workloads mit geringer Skalierung, die weniger als 48 GB RAM beanspruchen |
| [Azure Machine Learning Compute](../articles/machine-learning/service/how-to-run-batch-predictions.md) | (Vorschau) &nbsp;Batchrückschluss | &nbsp; | &nbsp;  | Ausführen von Batchbewertungen auf serverlosen Computezielen. Unterstützt virtuelle Computer mit normaler und niedriger Priorität. |
| [Azure IoT Edge](../articles/machine-learning/service/how-to-deploy-and-where.md#iotedge) | (Vorschau) IoT-&nbsp;Modul |  &nbsp; | &nbsp; | Bereitstellen und Verarbeiten von ML-Modellen auf IoT-Geräten. |
| [Azure Data Box Edge](../articles/databox-online/data-box-edge-overview.md)   | über IoT Edge |  &nbsp; | Ja | Bereitstellen und Verarbeiten von ML-Modellen auf IoT-Geräten. |

> [!NOTE]
> Zwar unterstützen Computeziele wie lokal, Notebook-VM und Azure Machine Learning Compute GPU für Training und Experimente, doch wird die Verwendung von GPU für Rückschlüsse nur mit dem Azure Kubernetes Service unterstützt.