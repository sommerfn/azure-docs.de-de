---
title: Größen von virtuellen Azure Linux-Computern – HPC | Microsoft-Dokumentation
description: Auflistung der verschiedenen verfügbaren Größen für virtuelle Linux HPC-Computer (High Performance Computing) in Azure. Dieser Artikel listet Informationen zur Anzahl von vCPUs, Datenträgern und Netzwerkschnittstellenkarten sowie zum Speicherdurchsatz und zur Netzwerkbandbreite für Größen dieser Serie auf.
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
ms.date: 10/12/2018
ms.author: jonbeck
ms.openlocfilehash: 003a14174ff65bab253f27a458d4f3e2c0a1a6db
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67069995"
---
# <a name="high-performance-compute-virtual-machine-sizes"></a>High Performance Computing-VM-Größen

[!INCLUDE [virtual-machines-common-sizes-hpc](../../../includes/virtual-machines-common-sizes-hpc.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]


### <a name="mpi"></a>MPI 

Die SR-IOV-fähigen VM-Größen in Azure gestatten die Verwendung nahezu jeder Version von MPI.
Auf VMs, die nicht SR-IOV-fähig sind, werden nur Intel MPI 5.x-Versionen unterstützt. Höhere Versionen (2017, 2018) der Intel MPI-Laufzeitbibliothek sind möglicherweise mit den Azure-Linux-RDMA-Treibern kompatibel.


### <a name="supported-os-images"></a>Unterstützte Betriebssystemimages
 
Der Azure Marketplace enthält viele Linux-Distributionen, die RDMA-Konnektivität unterstützen:
  
* **CentOS-basiertes HPC**: Für VMs, die nicht SR-IOV-fähig sind, sind die CentOS-basierte Versionen 6.5 HPC oder höher bis hin zu 7.5 geeignet. Für virtuelle Computer der H-Serie werden die Versionen 7.1 bis 7.5 empfohlen. Auf der VM sind RDMA-Treiber und Intel MPI 5.1 installiert.
  Für SR-IOV-VMs ist CentOS-HPC 7.6 für RDMA-Treiber optimiert und enthält diese vorab geladen, außerdem sind bereits verschiedene MPI-Pakete installiert.
  Für andere RHEL/CentOS-VM-Images fügen Sie die Erweiterung „InfiniBandLinux“ hinzu, um InfiniBand zu aktivieren. Diese Linux-Erweiterung für VMs installiert Mellanox OFED-Treiber (auf VMs mit SR-IOV), um RDMA-Konnektivität herzustellen. Das folgende PowerShell-Cmdlet installiert die neueste Version (Version 1.0) der Erweiterung „InfiniBandDriverLinux“ auf einem vorhandenen, RDMA-fähigen virtuellen Computer. Die RDMA-fähige VM heißt *myVM* und wird in der Ressourcengruppe mit dem Namen *myResourceGroup* in der Region *USA, Westen* wie folgt bereitgestellt:

  ```powershell
  Set-AzVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "InfiniBandDriverLinux" -Publisher "Microsoft.HpcCompute" -Type "InfiniBandDriverLinux" -TypeHandlerVersion "1.0"
  ```
  Alternativ können VM-Erweiterungen zur einfachen Bereitstellung mithilfe des folgenden JSON-Elements in Azure Resource Manager-Vorlagen aufgenommen werden:
  ```json
  "properties":{
  "publisher": "Microsoft.HpcCompute",
  "type": "InfiniBandDriverLinux",
  "typeHandlerVersion": "1.0",
  } 
  ```
 
  > [!NOTE]
  > Bei den CentOS-basierten HPC-Images sind Kernel-Updates in der **yum** -Konfigurationsdatei deaktiviert. Der Grund: Linux RDMA-Treiber werden als RPM-Paket verteilt, und Treiberupdates funktionieren möglicherweise nicht, wenn der Kernel aktualisiert wird.
  >
  

* **SUSE Linux Enterprise Server** – SLES 12 SP3 for HPC, SLES 12 SP3 for HPC (Premium), SLES 12 SP1 for HPC, SLES 12 SP1 for HPC (Premium), SLES 12 SP4 und SLES 15. Auf der VM werden RDMA-Treiber installiert und Intel MPI-Pakete verteilt. Installieren Sie MPI mit dem folgenden Befehl:

  ```bash
  sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
  ```
  
* **Ubuntu**: Ubuntu Server 16.04 LTS, 18.04 LTS. Konfigurieren Sie RDMA-Treiber auf der VM, und registrieren Sie sich bei Intel, um Intel MPI herunterzuladen:

  [!INCLUDE [virtual-machines-common-ubuntu-rdma](../../../includes/virtual-machines-common-ubuntu-rdma.md)]  

  Weitere Details zum Aktivieren von InfiniBand mittels Einrichtung von MPI finden Sie unter [Aktivieren von InfiniBand](../workloads/hpc/enable-infiniband.md).


### <a name="cluster-configuration-options"></a>Konfigurationsoptionen für Cluster

Azure bietet mehrere Optionen zum Erstellen von Clustern von Linux-HPC-VMs, die über das RDMA-Netzwerk kommunizieren können, einschließlich der folgenden: 

* **Virtuelle Computer**: Stellen Sie die RDMA-fähigen HPC-VMs in der gleichen Verfügbarkeitsgruppe bereit (wenn Sie das Azure Resource Manager-Bereitstellungsmodell verwenden). Stellen Sie die VMs bei Verwendung des klassischen Bereitstellungsmodells im gleichen Clouddienst bereit. 

* **VM-Skalierungsgruppen**: Stellen Sie bei Verwendung einer VM-Skalierungsgruppe sicher, dass Sie die Bereitstellung auf eine einzelne Platzierungsgruppe beschränken. Legen Sie z. B. in einer Resource Manager-Vorlage die Eigenschaft `singlePlacementGroup` auf `true` fest. 

* **Azure CycleCloud**: Erstellen Sie in [Azure CycleCloud](/azure/cyclecloud/) einen HPC-Cluster zum Ausführen von MPI-Aufträgen auf Linux-Knoten.

* **Azure Batch**: Erstellen Sie einen [Azure Batch](/azure/batch/)-Pool zum Ausführen von MPI-Workloads auf Linux-Computeknoten. Weitere Informationen finden Sie unter [Verwenden RDMA-fähiger oder GPU-fähiger Instanzen in Batch-Pools](../../batch/batch-pool-compute-intensive-sizes.md). Informationen zum Ausführen containerbasierter Workloads in Batch finden Sie im Projekt [Batch Shipyard](https://github.com/Azure/batch-shipyard).

* **Microsoft HPC Pack** - [HPC Pack](https://docs.microsoft.com/powershell/high-performance-computing/overview) unterstützt das Ausführen von mehreren Linux-Distributionen auf Computeknoten, die auf RDMA-fähigen virtuellen Azure-Computern bereitgestellt und von einem Windows Server-Hauptknoten verwaltet werden. Eine Beispielbereitstellung finden Sie unter [Erstellen eines HPC Pack Linux RDMA-Clusters in Azure](https://docs.microsoft.com/powershell/high-performance-computing/hpcpack-linux-openfoam).


### <a name="network-considerations"></a>Netzwerküberlegungen
* Auf nicht SR-IOV-fähigen, RDMA-fähigen virtuellen Linux-Computern in Azure ist „eth1“ für RDMA-Netzwerkdatenverkehr reserviert. Ändern Sie keine eth1-Einstellungen oder anderen Informationen in der Konfigurationsdatei, die sich auf dieses Netzwerk beziehen.
* Auf SR-IOV-fähigen virtuellen Computern (HB- und HC-Serie) ist „ib0“ für RDMA-Netzwerkdatenverkehr reserviert.
* Das RDMA-Netzwerk in Azure reserviert sich den Adressbereich 172.16.0.0/16. Wenn Sie MPI-Anwendungen auf Instanzen ausführen möchten, die in einem virtuellen Azure-Netzwerk bereitgestellt wurden, vergewissern Sie sich, dass der Adressraum des virtuellen Netzwerks sich nicht mit dem RDMA-Netzwerk überschneidet.
* Abhängig von dem von Ihnen ausgewählten Clusterverwaltungstool sind für die Ausführung von MPI-Aufträgen möglicherweise zusätzliche Systemkonfigurationsschritte erforderlich. Beispielsweise müssen Sie in einem Cluster von virtuellen Computern möglicherweise eine Vertrauensstellung zwischen den Clusterknoten herstellen, indem Sie SSH-Schlüssel generieren oder SSH-Anmeldungen ohne Kennwort einrichten.


## <a name="other-sizes"></a>Andere Größen
- [Allgemeiner Zweck](sizes-general.md)
- [Computeoptimiert](sizes-compute.md)
- [Arbeitsspeicheroptimiert](sizes-memory.md)
- [Speicheroptimiert](sizes-storage.md)
- [GPU](../windows/sizes-gpu.md)
- [Vorherige Generationen](sizes-previous-gen.md)

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Einrichten, Optimieren und Skalieren von [HPC-Workloads](../workloads/hpc/configure.md) in Azure.
- Weitere Informationen dazu, wie Sie mit [Azure-Computeeinheiten (ACU)](acu.md) die Computeleistung von Azure-SKUs vergleichen können.
