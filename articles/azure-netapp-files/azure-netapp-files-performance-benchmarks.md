---
title: Testergebnisse von Leistungsbenchmarks für Azure NetApp Files | Microsoft-Dokumentation
description: Beschreibt die Ergebnisse von Leistungsbenchmarktests für Azure NetApp Files auf Volumenebene.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/07/2019
ms.author: b-juche
ms.openlocfilehash: 1d6b43110046f26d8c8070b19587366588eee7b6
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2019
ms.locfileid: "68881756"
---
# <a name="performance-benchmark-test-results-for-azure-netapp-files"></a>Testergebnisse von Leistungsbenchmarks für Azure NetApp Files

In diesem Artikel werden die Ergebnisse von Leistungsbenchmarktests für Azure NetApp Files auf Volumenebene beschrieben. 

## <a name="sample-application-used-for-the-tests"></a>Beispielanwendung für die Tests

Die Leistungstests wurden mit einer Beispielanwendung unter Verwendung von Azure NetApp Files durchgeführt. Die Anwendung weist die folgenden Merkmale auf: 

* Eine für die Cloud entwickelte Linux-basierte Anwendung
* Kann mit zusätzlichen virtuellen Computern (VMs) linear skaliert werden, um die Computeleistung bei Bedarf zu erhöhen
* Erfordert schnellen Zugriff auf den Data Lake
* Weist E/A-Muster auf, die manchmal zufällig und manchmal sequenziell sind 
    * Ein zufälliges Muster erfordert eine geringe Wartezeit für große E/A-Mengen 
    * Ein sequenzielles Muster erfordert eine große Bandbreite 

## <a name="about-the-workload-generator"></a>Informationen zum Workload-Generator

Die Ergebnisse stammen aus Vdbench-Übersichtsdateien. [Vdbench](https://www.oracle.com/technetwork/server-storage/vdbench-downloads-1901681.html) ist ein Befehlszeilenprogramm, das Datenträger-E/A-Workloads zur Überprüfung der Speicherleistung erzeugt. Die verwendete Client-Server-Konfiguration ist skalierbar.  Sie umfasst einen einzelnen gemischten Master/Client und 14 dedizierte Client-VMs.

## <a name="about-the-tests"></a>Informationen zu den Tests

Die Tests wurden entwickelt, um die Grenzwerte, die die Beispielanwendung möglicherweise aufweist, und die Reaktionszeit zu ermitteln, die sich bis zu den Grenzwerten entwickelt.  

Die folgenden Tests wurden durchgeführt: 

* 100 % 8 KiB, zufälliger Lesevorgang
* 100 % 8 KiB, zufälliger Schreibvorgang
* 100 % 64 KiB, sequenzieller Lesevorgang
* 100 % 64 KiB, sequenzieller Schreibvorgang
* 50 % 64 KiB, sequenzieller Lesevorgang, 50 % 64 KiB, sequenzieller Schreibvorgang
* 50 % 8 KiB, zufälliger Lesevorgang, 50 % 8 KiB, zufälliger Schreibvorgang

## <a name="bandwidth"></a>Bandbreite

Azure NetApp Files bietet mehrere [Dienstebenen](azure-netapp-files-service-levels.md). Jede Dienstebene bietet eine andere Bandbreite pro TiB bereitgestellter Kapazität (Volumekontingent). Das Bandbreitenlimit für ein Volume wird basierend auf der Kombination aus Dienstebene und Volumekontingent bereitgestellt. Das Bandbreitenlimit ist nur ein Faktor bei der Bestimmung des tatsächlichen Durchsatzes, der erreicht wird.  

Derzeit sind 4.500 MiB der höchste Durchsatz, der von einer Workload mit nur einem Volume im Test erreicht wurde.  Mit der Dienstebene „Premium“ bietet ein Volumenkontingent von 70,31 TiB ausreichend Bandbreite, um diesen Durchsatz gemäß der folgenden Berechnung zu realisieren: 

![Bandbreitenformel](../media/azure-netapp-files/azure-netapp-files-bandwidth-formula.png)

![Kontingent und Dienstebene](../media/azure-netapp-files/azure-netapp-files-quota-service-level.png)

## <a name="throughput-intensive-workloads"></a>Durchsatzintensive Workloads

Der Durchsatztest erfolgte mit Vdbench und einer Kombination aus 12 x D32s V3 Speicher-VMs. Das Beispielvolume im Test erreichte folgende Durchsatzwerte:

![Durchsatztest](../media/azure-netapp-files/azure-netapp-files-throughput-test.png)

## <a name="io-intensive-workloads"></a>E/A-intensive Workloads

Der E/A-Test erfolgte mit Vdbench und einer Kombination aus 12 x D32s V3 Speicher-VMs. Das Beispielvolume im Test erreichte die folgenden E/A-Werte:

![E/A-Test](../media/azure-netapp-files/azure-netapp-files-io-test.png)

## <a name="latency"></a>Latency

Die Entfernung zwischen den Test-VMs und dem Azure NetApp Files-Volume hat Auswirkungen auf die E/A-Leistung.  Das folgende Diagramm vergleicht die IOPS- und Wartezeitantwortkurven für zwei verschiedene VM-Gruppen.  Ein VM-Gruppe befindet sich in der Nähe von Azure NetApp Files und die andere Gruppe ist weiter entfernt.  Die erhöhte Wartezeit für die weiter entfernte VM-Gruppe wirkt sich auf die IOPS-Menge aus, die bei einem bestimmten Parallelitätsgrad erreicht wird.  Unabhängig davon können die Messwerte für ein Volume 300.000 IOPS überschreiten, wie unten dargestellt: 

![Studie zur Wartezeit](../media/azure-netapp-files/azure-netapp-files-latency-study.png)

## <a name="summary"></a>Zusammenfassung

Wartezeitsensitive Workloads (Datenbanken) können eine Antwortzeit von einer Millisekunde aufweisen. Die Transaktionsleistung kann über 300.000 IOPS für ein einzelnes Volume betragen.

Durchsatzsensitive Anwendungen (für Streaming und Bildverarbeitung) können einen Durchsatz von 4,5 GiB/s aufweisen.

## <a name="example-scripts"></a>Beispielskripts

Die folgenden Beispielskripts dienen nur zu Demonstrationszwecken.  Sie dürfen nicht für Produktionszwecke verwendet werden.  

    #
    #This script makes the following assumptions about the environment
    #VM Naming Convention:
    #   VM naming convention: vdbench-vnet1-1 .. vdbench-vnet1-x
    #
    #VM Count:
    #   The script is written for 24 VM's, 
    #   If you wish to test with a different number of vm's replace {1..24} with {1..Some Number}
    #
    #Volume mount point:
    #    The volumes in this script are mounted at the following mount points on all virtual machines
    #    /mnt/vdb1 ... /mnt/vdb6
    #   
    #Virtual machines must have
    #   make sure that vdbench is present on all vms in the environment and that java has been installed on each vm as well as nfs-utils
    #
    #The following tunables were configured on all virtual machines in the environment
    #    No special tunables were used to extract the results identified in this paper.
    #    Even rsize and wsize were left at the default (64K)
    #
    #Special Notes (thread counts) 
    #   You can hone in on the amount of I/O you hope to achieve in your testing using littles law,
    #   Run each test with a thread count of 1 to determnine the best possible latency.
    #   Thread count equals latency in seconds * desired I/O rate.  
    #   If you find a minimum latency of 1.14ms for example, divide that by 1000 to convert 1.3ms to .0013 seconds.
    #   
    #   If your goal from a single machine is for example 3,500 IOPS, a thread count of 4.0 is required.
    
    #   thread count 4.0 == .0013s * 5,000ops
    #
    #    
    #Special Notes: Increased window size:
    #   As round-trip time increases; corresponding increases are required in TCP Window Size – think Littles law as shown above. 
    #   Check the values of the following and adjust as needed: net.core.rmem_max, net.core.rmem_default, net.ipv4.tcp_rmem
    #
    #Run the utility like this:
    #    vdbench -f vnet1-SeqMix-workload
    #
    #When the utility is finished its run, grep 'avg' from the summary file to see overall run performance.
     
    
    
    #LUN CONFIG FILES
    for vnet in 1; do
        echo "sd=default,size=693g" > vnet${vnet}-luns-nfs
        for vm in {1..24}; do
            for vol in {1..6}; do
                echo "sd=sd-${vm}-${vol},host=vm${vm},lun=/mnt/vdb${vol}/file-${vm}-${vol},openflags=o_direct" >> vnet${vnet}-luns-nfs
            done
        done
    done
    
    
    #HOST CONFIG FILES
    for vnet in 1; do
        echo "hd=default,jvms=1,shell=ssh" > vnet${vnet}-hosts-nfs
        for vm in {1..24}; do
            echo "hd=vm${vm},system=vdbench-vnet${vnet}-${vm},user=root" >> vnet${vnet}-hosts-nfs
        done
    done
    
    #VDBENCH WORK SCRIPTS
    for vnet in 1; do
        for pattern in FillWrite SeqRead SeqWrite RndRead RndWrite SeqMix RndMix; do
            echo "include=vnet${vnet}-hosts-nfs" > vnet${vnet}-${pattern}-workload
            echo "include=vnet${vnet}-luns-nfs" >> vnet${vnet}-${pattern}-workload
        done 
        #Fill Write File
        echo "wd=FillWrite,sd=sd*,rdpct=0,seekpct=eof,xfersize=64k" >> vnet${vnet}-FillWrite-workload
        echo "* Run Defaults" >> vnet${vnet}-FillWrite-workload
        echo 'rd=Initialize-LUNs-Full,wd=FillWrite,sd=("sd*"),iorate=max,interval=1,forthreads=1,elapsed=259200' >> vnet${vnet}-FillWrite-workload
    
        #The actual workload files - feel free to add more or use less.
        #   The thread counts shown below were used to generate the performance collateral, 
        #   feel free to change to suite your needs.
    
        echo "wd=SeqWrite,sd=sd*,rdpct=0,seekpct=0,xfersize=64k" >> vnet${vnet}-SeqWrite-workload
        echo "wd=SeqRead,sd=sd*,rdpct=100,seekpct=0,xfersize=64k" >> vnet${vnet}-SeqRead-workload
        echo "wd=RndRead,sd=sd*,rdpct=100,seekpct=100,xfersize=8k" >> vnet${vnet}-RndRead-workload
        echo "wd=RndWrite,sd=sd*,rdpct=0,seekpct=100,xfersize=8k" >> vnet${vnet}-RndWrite-workload
        echo "wd=SeqMix,sd=sd*,rdpct=50,seekpct=0,xfersize=64k" >> vnet${vnet}-SeqMix-workload
        echo "wd=RndMix,sd=sd*,rdpct=50,seekpct=100,xfersize=8k" >> vnet${vnet}-RndMix-workload
        for pattern in SeqRead SeqWrite RndRead RndWrite SeqMix RndMix; do
            echo "* Run Defaults" >> vnet${vnet}-${pattern}-workload
            #echo "rd=default,curve=(50,100),iorate=curve,warmup=60,elapsed=600,iorate=curve" >> vnet${vnet}-${pattern}-workload
            echo "rd=default,warmup=60,elapsed=600,iorate=max" >> vnet${vnet}-${pattern}-workload
            echo "* Run Definitions" >> vnet${vnet}-${pattern}-workload
        done
        echo 'rd=RndRead,wd=RndRead,sd=("sd*"),threads=25' >> vnet${vnet}-RndRead-workload
        echo 'rd=RndWrite,wd=RndWrite,sd=("sd*"),threads=20' >> vnet${vnet}-RndWrite-workload
        echo 'rd=SeqRead-4,wd=SeqRead,sd=("sd*"),threads=20' >> vnet${vnet}-SeqRead-workload
        echo 'rd=SeqWrite-26,wd=SeqWrite,sd=("sd*"),threads=26' >> vnet${vnet}-SeqWrite-workload
        echo 'rd=SeqWrite-32,wd=SeqWrite,sd=("sd*"),threads=32' >> vnet${vnet}-SeqWrite-workload
        echo 'rd=SeqWrite-40,wd=SeqWrite,sd=("sd*"),threads=40' >> vnet${vnet}-SeqWrite-workload
        echo 'rd=RndMix,wd=RndMix,sd=("sd*"),threads=25' >> vnet${vnet}-RndMix-workload
        echo 'rd=SeqMix-6,wd=SeqMix,sd=("sd*"),threads=6' >> vnet${vnet}-SeqMix-workload
        echo 'rd=SeqMix-7,wd=SeqMix,sd=("sd*"),threads=7' >> vnet${vnet}-SeqMix-workload
    done
