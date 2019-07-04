---
title: Einrichten von Message Passing Interface für HPC – Azure Virtual Machines | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie MPI für HPC in Azure einrichten.
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
ms.openlocfilehash: 5356a033dbc3d989dd27019f03b1fe36035ff9a4
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67441649"
---
# <a name="set-up-message-passing-interface-for-hpc"></a>Einrichten von Message Passing Interface für HPC

MPI-Workloads (Message Passing Interface) machen einen Großteil der herkömmlichen HPC-Workloads aus. Die SR-IOV-fähigen VM-Größen in Azure ermöglichen die Verwendung nahezu aller MPI-Varianten. 

Zur Ausführung von MPI-Aufträgen auf virtuellen Computern müssen innerhalb eines Mandanten Partitionsschlüssel eingerichtet werden. Eine ausführliche Anleitung zum Ermitteln der Partitionsschlüsselwerte finden Sie unter [Ermitteln von Partitionsschlüsseln](#discover-partition-keys).

## <a name="ucx"></a>UCX

[UCX](https://github.com/openucx/ucx) bietet die beste Leistung mit IB und kann mit MPICH und OpenMPI verwendet werden.

```bash
wget https://github.com/openucx/ucx/releases/download/v1.4.0/ucx-1.4.0.tar.gz
tar -xvf ucx-1.4.0.tar.gz
cd ucx-1.4.0
./configure --prefix=<ucx-install-path>
make -j 8 && make install
```

## <a name="openmpi"></a>OpenMPI

Installieren Sie UCX, wie weiter oben beschrieben.

```bash
sudo yum install –y openmpi
```

Erstellen Sie OpenMPI.

```bash
wget https://download.open-mpi.org/release/open-mpi/v4.0/openmpi-4.0.0.tar.gz
tar -xvf openmpi-4.0.0.tar.gz
cd openmpi-4.0.0
./configure --with-ucx=<ucx-install-path> --prefix=<ompi-install-path>
make -j 8 && make install
```

Führen Sie OpenMPI aus.

```bash
<ompi-install-path>/bin/mpirun -np 2 --map-by node --hostfile ~/hostfile -mca pml ucx --mca btl ^vader,tcp,openib -x UCX_NET_DEVICES=mlx5_0:1  -x UCX_IB_PKEY=0x0003  ./osu_latency
```

Überprüfen Sie Ihren Partitionsschlüssel, wie weiter oben erwähnt.

## <a name="mpich"></a>MPICH

Installieren Sie UCX, wie weiter oben beschrieben.

Erstellen Sie MPICH.

```bash
wget https://www.mpich.org/static/downloads/3.3/mpich-3.3.tar.gz
tar -xvf mpich-3.3.tar.gz
cd mpich-3.3
./configure --with-ucx=<ucx-install-path> --prefix=<mpich-install-path> --with-device=ch4:ucx
make -j 8 && make install
```

Führen Sie MPICH aus.

```bash
<mpich-install-path>/bin/mpiexec -n 2 -hostfile ~/hostfile -env UCX_IB_PKEY=0x0003 -bind-to hwthread ./osu_latency
```

Überprüfen Sie Ihren Partitionsschlüssel, wie weiter oben erwähnt.

## <a name="mvapich2"></a>MVAPICH2

Erstellen Sie MVAPICH2.

```bash
wget http://mvapich.cse.ohio-state.edu/download/mvapich/mv2/mvapich2-2.3.tar.gz
tar -xv mvapich2-2.3.tar.gz
cd mvapich2-2.3
./configure --prefix=<mvapich2-install-path>
make -j 8 && make install
```

Führen Sie MVAPICH2 aus.

```bash
<mvapich2-install-path>/bin/mpirun_rsh -np 2 -hostfile ~/hostfile MV2_CPU_MAPPING=48 ./osu_latency
```

## <a name="platform-mpi-community-edition"></a>Platform MPI (Community Edition)

Installieren Sie die erforderlichen Pakete für Platform MPI.

```bash
sudo yum install libstdc++.i686
sudo yum install glibc.i686
Download platform MPI at https://www.ibm.com/developerworks/downloads/im/mpi/index.html 
sudo ./platform_mpi-09.01.04.03r-ce.bin
```

Führen Sie die Schritte des Installationsprozesses aus.

## <a name="intel-mpi"></a>Intel MPI

[Laden Sie Intel MPI herunter.](https://software.intel.com/mpi-library/choose-download)

Ändern Sie die Umgebungsvariable „I_MPI_FABRICS“ abhängig von der Version. Verwenden Sie `I_MPI_FABRICS=shm:ofa` für Intel MPI 2018 und `I_MPI_FABRICS=shm:ofi` für Intel MPI 2019.

Die feste Prozesszuordnung (Process Pinning) funktioniert standardmäßig für 15, 30 und 60 PPN.

## <a name="osu-mpi-benchmarks"></a>OSU-MPI-Benchmarks

Laden Sie die OSU-MPI-Benchmarks herunter ([http://mvapich.cse.ohio-state.edu/benchmarks/](http://mvapich.cse.ohio-state.edu/benchmarks/)), und entpacken Sie sie.

```bash
wget http://mvapich.cse.ohio-state.edu/download/mvapich/osu-micro-benchmarks-5.5.tar.gz
tar –xvf osu-micro-benchmarks-5.5.tar.gz
cd osu-micro-benchmarks-5.5
```

Erstellen Sie Benchmarks unter Verwendung einer bestimmten MPI-Bibliothek:

```bash
CC=<mpi-install-path/bin/mpicc>CXX=<mpi-install-path/bin/mpicxx> ./configure 
make
```

MPI-Benchmarks befinden sich im Ordner `mpi/`.


## <a name="discover-partition-keys"></a>Ermitteln von Partitionsschlüsseln

Ermitteln Sie Partitionsschlüssel für die Kommunikation mit anderen virtuellen Computern.

```bash
/sys/class/infiniband/mlx5_0/ports/1/pkeys/0
/sys/class/infiniband/mlx5_0/ports/1/pkeys/1
```

Der größere der beiden ist der Mandantenschlüssel, der mit MPI verwendet werden muss. Beispiel: Von den folgenden Partitionsschlüsseln muss „0x800b“ mit MPI verwendet werden:

```bash
cat /sys/class/infiniband/mlx5_0/ports/1/pkeys/0
0x800b
cat /sys/class/infiniband/mlx5_0/ports/1/pkeys/1
0x7fff
```

Verwenden Sie die Partition, bei der es sich nicht um den Standardpartitionsschlüssel (0x7fff) handelt. Für UCX muss das MSB des Partitionsschlüssels gelöscht werden. Legen Sie also beispielsweise „UCX_IB_PKEY“ für „0x800b“ auf „0x000b“ fest.


## <a name="set-up-user-limits-for-mpi"></a>Einrichten von Benutzergrenzwerten für MPI

Richten Sie Benutzergrenzwerte für MPI ein.

```bash
cat << EOF >> /etc/security/limits.conf
*               hard    memlock         unlimited
*               soft    memlock         unlimited
*               hard    nofile          65535
*               soft    nofile          65535
EOF
```


## <a name="set-up-ssh-keys-for-mpi"></a>Einrichten von SSH-Schlüsseln für MPI

Richten Sie SSH-Schlüssel für MPI-Typen ein, bei denen dies erforderlich ist.

```bash
ssh-keygen -f /home/$USER/.ssh/id_rsa -t rsa -N ''
cat << EOF > /home/$USER/.ssh/config
Host *
    StrictHostKeyChecking no
EOF
cat /home/$USER/.ssh/id_rsa.pub >> /home/$USER/.ssh/authorized_keys
chmod 644 /home/$USER/.ssh/config
```

In der obigen Syntax wird von einem gemeinsam genutzten Basisverzeichnis ausgegangen. Andernfalls muss das SSH-Verzeichnis an jeden Knoten kopiert werden.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich ausführlicher über [HPC](https://docs.microsoft.com/azure/architecture/topics/high-performance-computing/) in Azure.
