---
title: Leistung von VM-Größen der HC-Serie – Azure Virtual Machines | Microsoft-Dokumentation
description: Hier finden Sie Leistungstestergebnisse für VM-Größen der HC-Serie in Azure.
services: virtual-machines
documentationcenter: ''
author: vermagit
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 05/15/2019
ms.author: amverma
ms.openlocfilehash: f931bfa486384a0f38a4964c2ed7b2446b40bfec
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "66809768"
---
# <a name="hc-series-virtual-machine-sizes"></a>VM-Größen der HC-Serie

Für die Größen der HC-Serie wurden zahlreiche Leistungstests durchgeführt. Im Anschluss finden Sie einige der Ergebnisse dieser Leistungstests:

| Workload                                        | HB                    |
|-------------------------------------------------|-----------------------|
| STREAM Triad                                    | ~190 GB/s (Intel MLC AVX-512)  |
| High-Performance Linpack (HPL)                  | ~3.520 GigaFLOPS (Rpeak), ~2.970 GigaFLOPS (Rmax) |
| RDMA-Wartezeit und -Bandbreite                        | 1,80 Mikrosekunden, 96,3 GBit/s   |
| FIO auf lokaler NVMe-SSD                           | ~1,3 GB/s (Lesen), ~900 MB/s (Schreiben) |  
| IOR auf vier Azure SSD Premium-Datenträgern (verwaltete Datenträger vom Typ „P30“, RAID0)**  | ~780 MB/s (Lesen), ~780 MB/s (Schreiben) |

## <a name="infiniband-send-latency"></a>InfiniBand-Wartezeit (Senden)

Mellanox Perftest

```azure-cli
numactl --physcpubind=[INSERT CORE #]  ib_send_lat -a
```

|  Bytes         | Iterationen     | t_min[Mikrosekunden]     | t_max[Mikrosekunden]     | t_typical[Mikrosekunden] | t_avg[Mikrosekunden]     | t_stdev[Mikrosekunden]   |
|-----------------|-----------------|-----------------|-----------------|-----------------|-----------------|-----------------|
| 2               | 1000            | 1,80            | 7,50            | 1,85            | 1,86            | 0,20            |
| 4               | 1000            | 1,79            | 6,06            | 1,83            | 1,84            | 0,20            |
| 8               | 1000            | 1,78            | 5.26            | 1,83            | 1,84            | 0,19            |
| 16              | 1000            | 1,79            | 6,21            | 1,83            | 1,84            | 0,22            |
| 32              | 1000            | 1,80            | 6,82            | 1,84            | 1,85            | 0,24            |
| 64              | 1000            | 1,85            | 5,47            | 1,88            | 1,86            | 0,12            |
| 128             | 1000            | 1,88            | 5,61            | 1,93            | 1,89            | 0,25            |
| 256             | 1000            | 2,24            | 6,39            | 2,28            | 2,02            | 0,18            |
| 512             | 1000            | 2,32            | 5,42            | 2,36            | 2,30            | 0,17            |
| 1024            | 1000            | 2,43            | 6,22            | 2,48            | 2,38            | 0,21            |
| 2048            | 1000            | 2.68            | 6,14            | 2.75            | 2.52            | 0,20            |
| 4096            | 1000            | 3,17            | 7,02            | 3,26            | 2.81            | 0,24            |

## <a name="osu-mpi-latency-test"></a>OSU-MPI-Wartezeittest

OSU MPI Latency Test v5.4.3

```azure-cli
./bin/mpirun_rsh -np 2 -hostfile ~/hostfile MV2_CPU_MAPPING=[INSERT CORE #] ./osu_latency 
```

| Bytes  | Wartezeit [Mikrosekunden] (MPICH 3.3 + CH4) | Wartezeit [Mikrosekunden] (OpenMPI 4.0.0) | Wartezeit [Mikrosekunden] (MVAPICH2 2.3) |
|------|----------|----------|----------|
| 2    | 1,84     | 1,78     | 2,08     |
| 4    | 1,84     | 1,79     | 2,08     |
| 8    | 1,85     | 1,79     | 2,05     |
| 16   | 1,85     | 1,79     | 2.1      |
| 32   | 1,87     | 1,82     | 2,12     |
| 64   | 2        | 1,95     | 2,13     |
| 128  | 2,05     | 2        | 2,18     |
| 256  | 2,48     | 2.44     | 2.75     |
| 512  | 2.57     | 2.52     | 2.81     |
| 1024 | 2.76     | 2.71     | 2,97     |
| 2048 | 3,09     | 3,11     | 3,34     |
| 4096 | 3.72     | 3,91     | 4.44     |

## <a name="mpi-bandwidth"></a>MPI-Bandbreite

OSU MPI Bandwidth Test v5.4.3

```azure-cli
./mvapich2-2.3.install/bin/mpirun_rsh -np 2 -hostfile ~/hostfile MV2_CPU_MAPPING=[INSERT CORE #] ./mvapich2-2.3/osu_benchmarks/mpi/pt2pt/osu_bw
```

| Größe   | Bandbreite (MB/s) | Bandbreite (GBit/s) |
|---------|------------------|------------------|
| 2       | 6,18             | 0,04944          |
| 4       | 13,27            | 0,10616          |
| 8       | 26,58            | 0,21264          |
| 16      | 53,51            | 0,42808          |
| 32      | 106,81           | 0,85448          |
| 64      | 211,24           | 1,68992          |
| 128     | 386,98           | 3,09584          |
| 256     | 756,32           | 6,05056          |
| 512     | 1\.434,2           | 11,4736          |
| 1024    | 2\.663,8           | 21,3104          |
| 2048    | 4\.396,99          | 35,17592         |
| 4096    | 6\.365,86          | 50,92688         |
| 8192    | 8\.137,9           | 65,1032          |
| 16384   | 9\.218,29          | 73,74632         |
| 32.768   | 10.564,61         | 84,51688         |
| 65536   | 11.275,6          | 90,2048          |
| 131.072  | 11.633,7          | 93,0696          |
| 262.144  | 11.856,27         | 94,85016         |
| 524.288  | 11.962,69         | 95,70152         |
| 1\.048.576 | 12.025,43         | 96,20344         |
| 2\.097.152 | 12.038,7          | 96,3096          |
| 4\.194.304 | 11.290,92         | 90,32736         |
