---
title: include file
description: include file
services: virtual-machines-windows
author: cynthn
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 02/11/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 12f053d7255c21441d78dab8c7937bcf45ec48c2
ms.sourcegitcommit: 6e6813f8e5fa1f6f4661a640a49dc4c864f8a6cb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2019
ms.locfileid: "67151816"
---
## <a name="supported-operating-systems-and-drivers"></a>Unterstützte Betriebssysteme und Treiber

### <a name="nvidia-tesla-cuda-drivers"></a>NVIDIA Tesla-Treiber (CUDA)

NVIDIA Tesla-Treiber CUDA für VMs der Serien NC, NCv2, NCv3, ND und NDv2 (optional für NV-Serie) werden nur unter den in der folgenden Tabelle aufgeführten Betriebssystemen unterstützt. Die Links zum Herunterladen der Treiber sind zum Zeitpunkt der Veröffentlichung aktuell. Die neuesten Treiber finden Sie auf der [NVIDIA](http://www.nvidia.com/)-Website.

> [!TIP]
> Alternativ zur manuellen CUDA-Treiberinstallation auf einem virtuellen Windows Server-Computer können Sie ein [Azure Data Science Virtual Machine](../articles/machine-learning/data-science-virtual-machine/overview.md)-Image bereitstellen. Bei den DSVM-Editionen für Windows Server 2016 sind NVIDIA CUDA-Treiber, die CUDA Deep Neural Network-Bibliothek und weitere Tools vorinstalliert.


| Betriebssystem | Treiber |
| -------- |------------- |
| Windows Server 2016 | [398.75](http://us.download.nvidia.com/Windows/Quadro_Certified/398.75/398.75-tesla-desktop-winserver2016-international.exe) (EXE-Datei) |
| Windows Server 2012 R2 | [398.75](http://us.download.nvidia.com/Windows/Quadro_Certified/398.75/398.75-tesla-desktop-winserver2008-2012r2-64bit-international.exe) (EXE-Datei) |

### <a name="nvidia-grid-drivers"></a>NVIDIA GRID-Treiber

Microsoft vertreibt Installationsprogramme für NVIDIA GRID-Treiber für virtuelle Computer der NV- und NVv2-Serie, die als virtuelle Arbeitsstationen oder für virtuelle Anwendungen verwendet werden. Installieren Sie nur diese GRID-Treiber auf Azure NV-VMs, und zwar nur unter den in der folgenden Tabelle aufgeführten Betriebssystemen. In diesen Treibern ist die Lizenzierung für virtuelle GRID-GPU-Software in Azure enthalten. Sie müssen keinen NVIDIA vGPU-Softwarelizenzserver einrichten.

| Betriebssystem | Treiber |
| -------- |------------- |
| Windows Server 2019<br/><br/>Windows Server 2016<br/><br/>Windows 10 (bis zu Version 1809). Ungepatcht/Patchebene 0 von 1809 wird nicht unterstützt.) | [GRID 8.0 (425.31)](https://go.microsoft.com/fwlink/?linkid=874181) (EXE-Datei) |
| Windows Server 2012 R2 | [GRID 8.0 (425.31)](https://go.microsoft.com/fwlink/?linkid=874184) (EXE-Datei)  |
