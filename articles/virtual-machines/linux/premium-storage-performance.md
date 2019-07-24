---
title: 'Azure Storage Premium: Entwurf für hohe Leistung auf Windows-VMs | Microsoft-Dokumentation'
description: Entwerfen Sie Hochleistungsanwendungen mithilfe von Azure Storage Premium. Storage Premium bietet Datenträgerunterstützung für hohe Leistung mit geringer Latenz für E/A-intensive Workloads, die auf virtuellen Azure-Computern ausgeführt werden.
services: virtual-machines-linux,storage
author: roygara
ms.service: virtual-machines-linux
ms.tgt_pltfrm: linux
ms.topic: article
ms.date: 06/27/2017
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: c6de3144a87a5bfad38e1b33148f292b26c0f181
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2019
ms.locfileid: "67658249"
---
[!INCLUDE [virtual-machines-common-premium-storage-introduction](../../../includes/virtual-machines-common-premium-storage-introduction.md)]

> [!NOTE]
> Manchmal ist ein vermutetes Problem mit der Datenträgerleistung tatsächlich ein Engpass. In solchen Fällen sollten Sie Ihre [Netzwerkleistung](../../virtual-network/virtual-network-optimize-network-bandwidth.md) optimieren.
>
> Wenn Sie für Ihren Datenträger einen Benchmarktest durchführen möchten, finden Sie weitere Informationen in unserem Artikel [Benchmarktests für einen Datenträger](disks-benchmarks.md).
>
> Wenn Ihr virtueller Computer den beschleunigten Netzwerkbetrieb unterstützt, stellen Sie sicher, dass dieser aktiviert ist. Wenn er nicht aktiviert ist, können Sie ihn sowohl unter [Windows](../../virtual-network/create-vm-accelerated-networking-powershell.md#enable-accelerated-networking-on-existing-vms) als auch [Linux](../../virtual-network/create-vm-accelerated-networking-cli.md#enable-accelerated-networking-on-existing-vms) auf bereits bereitgestellten virtuellen Computern aktivieren.

Falls Sie noch nicht mit Storage Premium vertraut sind, lesen Sie zunächst die Artikel [Auswählen eines Azure-Datenträgertyps für IaaS VMs](disks-types.md) und [Azure Storage-Skalierbarkeits- und Leistungsziele für Speicherkonten](../../storage/common/storage-scalability-targets.md).


[!INCLUDE [virtual-machines-common-premium-storage-performance.md](../../../includes/virtual-machines-common-premium-storage-performance.md)]
