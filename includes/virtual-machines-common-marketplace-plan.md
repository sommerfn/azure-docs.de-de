---
title: include file
description: include file
services: virtual-machines-windows, virtual-machines-linux
author: cynthn
ms.service: multiple
ms.topic: include
ms.date: 10/09/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 8e0e549f88caf4a541642bab77faf54b5e536b29
ms.sourcegitcommit: f2771ec28b7d2d937eef81223980da8ea1a6a531
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/20/2019
ms.locfileid: "71174952"
---
## <a name="deploy-an-image-with-marketplace-terms"></a>Bereitstellen eines Images mit Marketplace-Nutzungsbedingungen

Für einige VM-Images im Azure Marketplace gelten zusätzliche Lizenz- und Kaufbedingungen, die Sie akzeptieren müssen, bevor Sie sie programmgesteuert bereitstellen können.  

Um einen virtuellen Computer über ein solches Image bereitzustellen, müssen Sie die Bedingungen des Images akzeptieren und die programmgesteuerte Bereitstellung aktivieren. Diese Aktionen müssen Sie pro Abonnement nur einmal durchführen. Anschließend müssen Sie jedes Mal, wenn Sie eine VM programmgesteuert über das Image bereitstellen, auch die Parameter des *Kaufplans* angeben.

In den folgenden Abschnitten werden diese Aktionen gezeigt:

* Herausfinden, ob für ein Marketplace-Image zusätzliche Lizenzbedingungen gelten 
* Programmgesteuertes Akzeptieren der Bedingungen
* Angeben von Parametern des Kaufplans, wenn Sie einen virtuellen Computer programmgesteuert bereitstellen

