---
title: Übersicht über verwaltete Azure Disk Storage-Datenträger für Linux-VMs | Microsoft-Dokumentation
description: Enthält eine Übersicht über Azure Managed Disks. Hiermit werden bei Verwendung von Linux-VMs die Speicherkonten für Sie verwaltet.
author: roygara
ms.service: virtual-machines-linux
ms.topic: overview
ms.date: 08/15/2019
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 3ba797088eb23262c583906d7023f20fb7d408ba
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/15/2019
ms.locfileid: "69516153"
---
# <a name="introduction-to-azure-managed-disks"></a>Einführung in verwaltete Azure-Datenträger

Ein verwalteter Azure-Datenträger ist eine virtuelle Festplatte (Virtual Hard Disk, VHD). Sie können sich dies wie einen physischen Datenträger in einem lokalen Server vorstellen, aber der Unterschied besteht darin, dass er virtualisiert ist. Verwaltete Azure-Datenträger werden als Seitenblobs gespeichert, bei denen es sich in Azure um ein zufälliges E/A-Speicherobjekt handelt. Wir bezeichnen einen verwalteten Datenträger als „verwaltet“, weil es eine Abstraktion ist, die sich über Seitenblobs, Blobcontainer und Azure-Speicherkonten erstreckt. Bei verwalteten Datenträgern müssen Sie nicht mehr tun, als den Datenträger bereitzustellen. Azure übernimmt dann den Rest.

Wenn Sie sich für die Verwendung von verwalteten Azure-Datenträgern für Ihre Workloads entscheiden, führt Azure die Erstellung und Verwaltung des Datenträgers für Sie durch. Die verfügbaren Datenträgertypen sind Ultra-Datenträger, SSD Premium, SSD Standard und Standard Hard Disk Drives (HDD). Weitere Informationen zu den einzelnen Datenträgertypen finden Sie unter [Auswählen eines Datenträgertyps für virtuelle IaaS-Computer](disks-types.md).

[!INCLUDE [virtual-machines-managed-disks-overview.md](../../../includes/virtual-machines-managed-disks-overview.md)]

> [!div class="nextstepaction"]
> [Auswählen eines Datenträgertyps für virtuelle IaaS-Computer](disks-types.md)
