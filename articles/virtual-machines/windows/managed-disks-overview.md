---
title: Übersicht über verwaltete Azure Disk Storage-Datenträger für Windows-VMs
description: Enthält eine Übersicht über verwaltete Azure-Datenträger. Hiermit werden bei Verwendung von Azure-Windows-VMs die Speicherkonten für Sie verwaltet.
author: roygara
ms.service: virtual-machines-windows
ms.topic: overview
ms.date: 08/15/2019
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: c23cfbc418cca82393a0a66b0ceace622b2833f5
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74038155"
---
# <a name="introduction-to-azure-managed-disks"></a>Einführung in verwaltete Azure-Datenträger

Ein verwalteter Azure-Datenträger ist eine virtuelle Festplatte (Virtual Hard Disk, VHD). Sie können sich dies wie einen physischen Datenträger in einem lokalen Server vorstellen, aber der Unterschied besteht darin, dass er virtualisiert ist. Verwaltete Azure-Datenträger werden als Seitenblobs gespeichert, bei denen es sich in Azure um ein zufälliges E/A-Speicherobjekt handelt. Wir bezeichnen einen verwalteten Datenträger als „verwaltet“, weil es eine Abstraktion ist, die sich über Seitenblobs, Blobcontainer und Azure-Speicherkonten erstreckt. Bei verwalteten Datenträgern müssen Sie nicht mehr tun, als den Datenträger bereitzustellen. Azure übernimmt dann den Rest.

Wenn Sie sich für die Verwendung von verwalteten Azure-Datenträgern für Ihre Workloads entscheiden, führt Azure die Erstellung und Verwaltung des Datenträgers für Sie durch. Die verfügbaren Datenträgertypen sind Ultra-Datenträger, SSD Premium, SSD Standard und Standard Hard Disk Drives (HDD). Weitere Informationen zu den einzelnen Datenträgertypen finden Sie unter [Auswählen eines Datenträgertyps für virtuelle IaaS-Computer](disks-types.md).

[!INCLUDE [virtual-machines-managed-disks-overview.md](../../../includes/virtual-machines-managed-disks-overview.md)]

> [!div class="nextstepaction"]
> [Auswählen eines Datenträgertyps für virtuelle IaaS-Computer](disks-types.md)