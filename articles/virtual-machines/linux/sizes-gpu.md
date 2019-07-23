---
title: Größen von virtuellen Azure Linux-Computern – beschleunigtes Compute | Microsoft-Dokumentation
description: Auflistung der verschiedenen verfügbaren GPU-optimierten Größen für virtuelle Linux-Computer in Azure. Dieser Artikel listet Informationen zur Anzahl von vCPUs, Datenträgern und Netzwerkschnittstellenkarten sowie zum Speicherdurchsatz und zur Netzwerkbandbreite für Größen dieser Serie auf.
services: virtual-machines-linux
documentationcenter: ''
author: jonbeck7
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/11/2019
ms.author: jonbeck
ms.openlocfilehash: 3c59c73914a78374da0b8d2f9aeb18b9005e00ac
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67070007"
---
# <a name="gpu-optimized-virtual-machine-sizes"></a>Für GPU optimierte VM-Größen

[!INCLUDE [virtual-machines-common-sizes-gpu](../../../includes/virtual-machines-common-sizes-gpu.md)]


[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="supported-distributions-and-drivers"></a>Unterstützte Verteilungen und Treiber

Um die GPU-Funktionen von virtuellen Azure-Computern der N-Serie unter Linux nutzen zu können, müssen NVIDIA-GPU-Treiber installiert werden. Mit der [NVIDIA-GPU-Treibererweiterung](../extensions/hpccompute-gpu-linux.md) werden entsprechende NVIDIA-CUDA- oder GRID-Treiber auf einem virtuellen Computer der N-Serie installiert. Installieren oder verwalten Sie die Erweiterung mithilfe des Azure-Portals oder mit Tools wie der Azure-Befehlszeilenschnittstelle oder Azure Resource Manager-Vorlagen. Informationen zu unterstützten Distributionen und Bereitstellungsschritten finden Sie in der [Dokumentation zur NVIDIA-GPU-Treibererweiterung](../extensions/hpccompute-gpu-linux.md). Allgemeine Informationen zu VM-Erweiterungen finden Sie unter [Erweiterungen und Features für virtuelle Azure-Computer](../extensions/overview.md).

Wenn Sie NVIDIA-GPU-Treiber manuell installieren möchten, finden Sie Informationen zu unterstützten Distributionen und Treibern sowie Schritte zur Installation und Überprüfung unter [Einrichten von GPU-Treibern der N-Serie für Linux](n-series-driver-setup.md).


[!INCLUDE [virtual-machines-n-series-considerations](../../../includes/virtual-machines-n-series-considerations.md)]

* Sie sollten nicht X-Server oder andere Systeme installieren, die den `Nouveau`-Treiber auf Ubuntu-NC-VMs verwenden. Vor der Installation von NVIDIA-GPU-Treibern müssen Sie den `Nouveau`-Treiber deaktivieren.  

## <a name="other-sizes"></a>Andere Größen
- [Allgemeiner Zweck](sizes-general.md)
- [Computeoptimiert](sizes-compute.md)
- [Arbeitsspeicheroptimiert](sizes-memory.md)
- [Speicheroptimiert](sizes-storage.md)
- [High Performance Computing](sizes-hpc.md)
- [Vorherige Generationen](sizes-previous-gen.md)

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen dazu, wie Sie mit [Azure-Computeeinheiten (ACU)](acu.md) die Computeleistung von Azure-SKUs vergleichen können.