---
title: High Performance Computing auf VMs der H-Serie – Azure Virtual Machines | Microsoft-Dokumentation
description: Enthält eine Beschreibung der Features und Funktionen von VMs der H-Serie, die für HPC optimiert sind.
services: virtual-machines
documentationcenter: ''
author: vermagit
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: overview
ms.date: 07/02/2019
ms.author: amverma
ms.openlocfilehash: d6e857a87e4c7df8ffb2be1eefb7a0290da5b10a
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67799005"
---
# <a name="high-performance-computing-on-h-series-vms"></a>High Performance Computing auf VMs der H-Serie

Mit High Performance Computing (HPC) auf VMs der HB- und HC-Serie kann für alle VMs unter Azure eine HPC-Leistung mit dem höchsten Optimierungsgrad erzielt werden. Für HPC optimierte VMs werden verwendet, um einige der schwierigsten mathematischen Probleme zu lösen, z. B. Strömungsmechanik, Simulationen für Öl- und Gasunternehmen und Wettermodelle.

In diesem Artikel werden einige wichtige Features von VMs der HB- und HC-Serie beschrieben. Sie erfahren, warum diese VMs bei HPC-Szenarien eine gute Leistung aufweisen und welche ersten Schritte ausgeführt werden müssen.

## <a name="features-and-capabilities"></a>Features und Funktionen

VMs der HB- und HC-Serie sind so konzipiert, dass sie die beste HPC-Leistung, MPI-Skalierbarkeit (Message Passing Interface) und Kosteneffizienz für HPC-Workloads aufweisen.

### <a name="message-passing-interface"></a>Message Passing Interface

Für die HB- und HC-Serie werden fast alle MPI-Typen und -Versionen unterstützt. Einige der häufigsten unterstützten MPI-Typen sind: OpenMPI, MVAPICH2, Platform MPI, Intel MPI und alle Verben für Remotezugriff auf den direkten Speicher (Remote Direct Memory Access, RDMA). Weitere Informationen finden Sie unter [Einrichten von Message Passing Interface für HPC](setup-mpi.md).

### <a name="rdma-and-infiniband"></a>RDMA und InfiniBand

Die RDMA-Schnittstelle ist auf VMs der HB- und HC-Serie Standard. RDMA-fähige Instanzen kommunizieren über ein InfiniBand-Netzwerk, das mit erweiterten Datenraten (Enhanced Data Rates, EDR) für virtuelle Computer der HB- und HC-Serie betrieben wird. Mit RDMA-fähigen Instanzen können die Skalierbarkeit und Leistung einiger MPI-Anwendungen deutlich erhöht werden.

Bei der InfiniBand-Konfiguration zur Unterstützung von VMs der HB- und HC-Serie handelt es sich um blockierungsfreie Fat Trees mit einem Design mit geringem Durchmesser, um eine einheitliche RDMA-Leistung zu erzielen.

Weitere Informationen zur Einrichtung von InfiniBand auf Ihren VMs der HB- und HC-Serie finden Sie unter [Aktivieren von InfiniBand](enable-infiniband.md).

## <a name="get-started"></a>Erste Schritte

Entscheiden Sie zuerst, welche VM der H-Serie Sie verwenden möchten. Ausführlichere Informationen zu VMs, die für HPC optimiert sind, finden Sie unter der [Übersicht zur HB-Serie](hb-series-overview.md) und der [Übersicht zur HC-Serie](hc-series-overview.md). Informationen zur Spezifikation finden Sie unter [High Performance Computing-VM-Größen](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-hpc).

Nachdem Sie eine VM für Ihre Anwendung ausgewählt und erstellt haben, müssen Sie sie konfigurieren, indem Sie InfiniBand aktivieren. Informationen dazu, wie Sie InfiniBand auf Windows- und Linux-VMs aktivieren, finden Sie unter [Aktivieren von InfiniBand](enable-infiniband.md).

Eine wichtige Komponente von HPC-Workloads ist MPI. Für die HB- und HC-Serie werden fast alle MPI-Typen und -Versionen unterstützt. Weitere Informationen finden Sie unter [Einrichten von Message Passing Interface für HPC](setup-mpi.md).

Nachdem Sie Ihre VM-Serie ausgewählt haben, richten Sie Infiniband und MPI ein. Anschließend ist alles bereit, um mit der Erstellung Ihrer HPC-Workloads zu beginnen.

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie die [Übersicht zur HB-Serie](hb-series-overview.md) und die [Übersicht zur HC-Serie](hc-series-overview.md), um sich über die wichtigen Unterschiede und Spezifikationen zu informieren.

- Eine allgemeinere Übersicht über die Architektur für die Ausführung von HPC-Workloads finden Sie unter [High Performance Computing (HPC) in Azure](https://docs.microsoft.com/azure/architecture/topics/high-performance-computing/).
