---
title: High Performance Computing – Azure Virtual Machines | Microsoft-Dokumentation
description: Hier erfahren Sie mehr über High Performance Computing in Azure.
services: virtual-machines
documentationcenter: ''
author: vermagit
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 05/07/2019
ms.author: amverma
ms.openlocfilehash: 10549abfbdacf1fc1ae6b99f4cab20a290c32a2d
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2019
ms.locfileid: "67707829"
---
# <a name="optimization-for-linux"></a>Optimierung für Linux

In diesem Artikel werden einige wichtige Verfahren zum Optimieren des Betriebssystemimages veranschaulicht. Informieren Sie sich ausführlicher über die [Aktivierung von InfiniBand](enable-infiniband.md) und die Optimierung von Betriebssystemimages.

## <a name="update-lis"></a>Aktualisieren von LIS

Aktualisieren Sie bei der Bereitstellung unter Verwendung eines benutzerdefinierten Images (beispielsweise eines älteren Betriebssystems wie CentOS/RHEL 7.4 oder 7.5) LIS auf dem virtuellen Computer.

```bash
wget https://aka.ms/lis
tar xzf lis
pushd LISISO
./upgrade.sh
```

## <a name="reclaim-memory"></a>Freigeben von Arbeitsspeicher

Steigern Sie die Effizienz durch die automatische Freigabe von Arbeitsspeicher, um den Remotezugriff auf den Speicher zu vermeiden.

```bash
echo 1 >/proc/sys/vm/zone_reclaim_mode
```

So legen Sie fest, dass diese Einstellung auch nach VM-Neustarts gilt:

```bash
echo "vm.zone_reclaim_mode = 1" >> /etc/sysctl.conf sysctl -p
```

## <a name="disable-firewall-and-selinux"></a>Deaktivieren von Firewall und SELinux

Deaktivieren Sie die Firewall und SELinux.

```bash
systemctl stop iptables.service
systemctl disable iptables.service
systemctl mask firewalld
systemctl stop firewalld.service
systemctl disable firewalld.service
iptables -nL
sed -i -e's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
```

## <a name="disable-cpupower"></a>Deaktivieren von cpupower

Deaktivieren Sie cpupower.

```bash
service cpupower status
if enabled, disable it:
service cpupower stop
sudo systemctl disable cpupower
```

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich ausführlicher über die [Aktivierung von InfiniBand](enable-infiniband.md) und die Optimierung von Betriebssystemimages.

* Informieren Sie sich ausführlicher über [HPC](https://docs.microsoft.com/azure/architecture/topics/high-performance-computing/) in Azure.
