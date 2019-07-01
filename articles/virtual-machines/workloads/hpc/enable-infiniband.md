---
title: Aktivieren von InifinBand mit SR-IOV – Azure Virtual Machines | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie InfiniBand mit SR-IOV aktivieren.
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
ms.openlocfilehash: 81acb804ed2ebb9e88bc7d8281a7fa52359d4455
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "66809798"
---
# <a name="enable-infiniband-with-sr-iov"></a>Aktivieren von InfiniBand mit SR-IOV


Am einfachsten können Sie Ihr benutzerdefiniertes VM-Image mit InfiniBand (IB) konfigurieren, indem Sie Ihrer Bereitstellung die VM-Erweiterung „InfiniBandDriverLinux“ oder „InfiniBandDriverWindows“ hinzufügen. Dies ist auch die empfohlene Vorgehensweise.
Informationen zur Verwendung dieser VM-Erweiterungen finden Sie [hier](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-hpc#rdma-capable-instances) (Linux) bzw. [hier](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-hpc#rdma-capable-instances) (Windows).

Wenn Sie InfiniBand manuell auf SR-IOV-fähigen virtuellen Computern (derzeit aus der HB- und HC-Serie) konfigurieren möchten, führen Sie die folgenden Schritte aus. Diese Schritte gelten nur für RHEL/CentOS. Für Ubuntu (16.04 und 18.04) und SLES (12 SP4 und 15) reichen die integrierten Treiber aus. Für Ubuntu: 


## <a name="manually-install-ofed"></a>Manuelles Installieren von OFED

Installieren Sie die neuesten MLNX_OFED-Treiber für ConnectX-5 von [Mellanox](http://www.mellanox.com/page/products_dyn?product_family=26).

RHEL/CentOS (Beispiel für 7.6):
```bash
sudo yum install -y kernel-devel python-devel
sudo yum install -y redhat-rpm-config rpm-build gcc-gfortran gcc-c++
sudo yum install -y gtk2 atk cairo tcl tk createrepo
wget --retry-connrefused --tries=3 --waitretry=5 http://content.mellanox.com/ofed/MLNX_OFED-4.5-1.0.1.0/MLNX_OFED_LINUX-4.5-1.0.1.0-rhel7.6-x86_64.tgz
tar zxvf MLNX_OFED_LINUX-4.5-1.0.1.0-rhel7.6-x86_64.tgz
sudo ./MLNX_OFED_LINUX-4.5-1.0.1.0-rhel7.6-x86_64/mlnxofedinstall --add-kernel-support
```

Laden Sie für Windows die WinOF-2-Treiber für ConnectX-5 von [Mellanox](http://www.mellanox.com/page/products_dyn?product_family=32&menu_section=34) herunter, und installieren Sie sie.

## <a name="assign-an-ip-address"></a>Zuweisen einer IP-Adresse

Weisen Sie der ib0-Schnittstelle eine IP-Adresse zu. Hierzu haben Sie zwei Möglichkeiten:

- Weisen Sie die IP-Adresse manuell der ib0-Schnittstelle zu (als root-Benutzer).

    ```bash
    ifconfig ib0 $(sed '/rdmaIPv4Address=/!d;s/.*rdmaIPv4Address="\([0-9.]*\)".*/\1/' /var/lib/waagent/SharedConfig.xml)/16
    ```

OR

- Verwenden Sie WALinuxAgent, um die IP-Adresse dauerhaft zuzuweisen.

    ```bash
    yum install -y epel-release
    yum install -y python-pip
    python -m pip install --upgrade pip setuptools wheel
    wget "https://github.com/Azure/WALinuxAgent/archive/release-2.2.36.zip"
    unzip release-2.2.36.zip
    cd WALinuxAgent*
    python setup.py install --register-service --force
    sed -i -e 's/# OS.EnableRDMA=y/OS.EnableRDMA=y/g' /etc/waagent.conf
    sed -i -e 's/# AutoUpdate.Enabled=y/AutoUpdate.Enabled=y/g' /etc/waagent.conf
    systemctl restart waagent
    ```

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich ausführlicher über [HPC](https://docs.microsoft.com/azure/architecture/topics/high-performance-computing/) in Azure.
