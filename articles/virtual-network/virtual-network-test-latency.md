---
title: Testen der Netzwerklatenz von virtuellen Azure-Computern in einem virtuellen Azure-Netzwerk | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Netzwerklatenz zwischen virtuellen Azure-Computern in einem virtuellen Netzwerk testen.
services: virtual-network
documentationcenter: na
author: steveesp
manager: Marina Lipshteyn
editor: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/29/2019
ms.author: steveesp
ms.openlocfilehash: 760a181b4459db28d3a6eee5022cc0f984c4bee0
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73587083"
---
# <a name="testing-network-latency"></a>Testen der Netzwerklatenz

Das Messen der Netzwerklatenz mit einem Tool, das für diese Aufgabe entwickelt wurde, liefert die genauesten Ergebnisse. Öffentlich verfügbare Tools wie SockPerf (für Linux) oder Latte (für Windows) sind Beispiele für Tools, mit denen die Netzwerklatenz isoliert und gemessen werden kann, während andere Arten von Latenz, z. B. die Anwendungslatenz, ausgeschlossen werden. Diese Tools konzentrieren sich auf die Typen von Netzwerkdatenverkehr, die sich auf die Anwendungsleistung auswirken: TCP und UDP. Andere gängige Konnektivitätstools wie Ping können mitunter zum Messen der Latenz verwendet werden, deren Ergebnisse entsprechen jedoch möglicherweise nicht dem Netzwerkdatenverkehr in echten Workloads. Das liegt daran, dass die meisten dieser Tools das ICMP-Protokoll verwenden, das anders als Anwendungsdatenverkehr behandelt werden kann. Die Ergebnisse sind daher möglicherweise nicht auf Workloads anwendbar, die TCP und UDP verwenden. Für genaue Tests der Netzwerklatenz von Protokollen, die von den meisten Anwendungen verwendet werden, liefern SockPerf (für Linux) und Latte (für Windows) die relevantesten Ergebnisse. In diesem Dokument werden diese beiden Tools behandelt.

## <a name="overview"></a>Übersicht

Mithilfe von zwei VMs, einer als Sender und einer als Empfänger, wird ein bidirektionaler Kommunikationskanal erstellt, um Pakete in beiden Richtungen zu senden und zu empfangen und die Streckengesamtzeit (Round-Trip Time, RTT) zu messen.

Mithilfe dieser Schritte kann die Netzwerklatenz zwischen zwei virtuellen Computern (VMs) oder sogar zwischen zwei physischen Computern gemessen werden. Latenzmessungen können in den folgenden Szenarien nützlich sein:

- Festlegen eines Benchmarks für die Netzwerklatenz zwischen den bereitgestellten VMs
- Vergleichen der Auswirkungen von Änderungen an der Netzwerklatenz nach Änderungen an folgenden Elementen:
  - Betriebssystem oder Netzwerkstapel-Software, einschließlich Konfigurationsänderungen
  - VM-Bereitstellungsmethode, z. B. Bereitstellung in einer Zone oder PPG
  - VM-Eigenschaften, z. B. beschleunigter Netzwerkbetrieb oder Größenänderungen
  - Virtuelles Netzwerk, z. B. Routing- oder Filteränderungen

### <a name="tools-for-testing"></a>Tools zum Testen
Zum Messen der Latenz stehen zwei Optionen zur Verfügung: eine für Windows-basierte Systeme und eine für Linux-basierte Systeme.

Windows: „latte.exe“ (Windows) [https://gallery.technet.microsoft.com/Latte-The-Windows-tool-for-ac33093b](https://gallery.technet.microsoft.com/Latte-The-Windows-tool-for-ac33093b)

Linux: SockPerf (Linux) [https://github.com/mellanox/sockperf](https://github.com/mellanox/sockperf)

Mit der Verwendung dieser Tools wird sichergestellt, dass nur die Bereitstellungszeiten für TCP- oder UDP-Nutzlasten gemessen werden, nicht ICMP (Ping) oder andere Pakettypen, die nicht von Anwendungen verwendet werden und deren Leistung nicht beeinträchtigen.

### <a name="tips-for-an-optimal-vm-configuration"></a>Tipps für eine optimale VM-Konfiguration:

- Verwenden Sie die neueste Version von Windows oder Linux.
- Aktivieren Sie für optimale Ergebnisse den beschleunigten Netzwerkbetrieb.
- Stellen Sie VMs mit [Azure-Näherungsplatzierungsgruppen](https://docs.microsoft.com/azure/virtual-machines/linux/co-location) bereit.
- Größere VMs erzielen im Allgemeinen eine bessere Leistung als kleinere.

### <a name="tips-for-analysis"></a>Tipps für die Analyse

- Erstellen Sie frühzeitig eine Baseline, sobald Bereitstellung, Konfiguration und Optimierung abgeschlossen sind.
- Vergleichen Sie neue Ergebnisse immer mit einer Baseline oder andernfalls einem anderen Test mit kontrollierten Änderungen.
- Wiederholen Sie Tests, wenn Änderungen beobachtet oder geplant werden.


## <a name="testing-vms-running-windows"></a>Testen von VMs mit Windows

## <a name="get-latteexe-onto-the-vms"></a>Laden von „latte.exe“ auf die VMs

Herunterladen der aktuellen Version: [https://gallery.technet.microsoft.com/Latte-The-Windows-tool-for-ac33093b](https://gallery.technet.microsoft.com/Latte-The-Windows-tool-for-ac33093b)

Legen Sie „latte.exe“ möglichst in einem separaten Ordner ab, z. B. „c:\tools“.

## <a name="allow-latteexe-through-the-windows-firewall"></a>Zulassen von „Latte.exe“ durch die Windows-Firewall

Erstellen Sie auf dem EMPFÄNGER eine Zulassungsregel in der Windows-Firewall, damit der Datenstrom von „latte.exe“ empfangen werden kann. Am einfachsten ist es, das gesamte „Latte.exe“-Programm nach Name anstatt nach bestimmten eingehenden TCP-Ports zuzulassen.

Lassen Sie „latte.exe“ durch die Windows-Firewall zu:

netsh advfirewall firewall add rule program=\<Pfad\>\\latte.exe name=&quot;Latte&quot; protocol=any dir=in action=allow enable=yes profile=ANY


Beispiel: Wenn Sie „latte.exe“ in den Ordner &quot;c:\\tools&quot; kopiert haben, lautet der Befehl wie folgt:

```cmd
netsh advfirewall firewall add rule program=c:\tools\latte.exe name="Latte" protocol=any dir=in action=allow enable=yes profile=ANY
```

## <a name="running-latency-tests"></a>Ausführen von Latenztests

Starten Sie „latte.exe“ auf dem EMPFÄNGER (Ausführen über CMD, nicht über PowerShell):

latte -a &lt;Empfänger-IP-Adresse&gt;:&lt;Port&gt; -i &lt;Iterationen&gt;

Etwa 65.000 Iterationen sollten repräsentative Ergebnisse liefern.

Jede verfügbare Portnummer ist in Ordnung.

Wenn der virtuelle Computer die IP-Adresse 10.0.0.4 aufweist, lautet der Befehl wie folgt:

```cmd
latte -a 10.0.0.4:5005 -i 65100
```

Starten Sie „latte.exe“ auf dem SENDER (Ausführen über CMD, nicht über PowerShell):

latte -c -a \<Empfänger-IP-Adresse\>:\<Port\> -i \<Iterationen\>


Der resultierende Befehl ist derselbe wie beim Empfänger, nur dass er den Zusatz &quot;-c&quot; enthält, um anzuzeigen, dass es sich um den &quot;Client&quot; oder Sender handelt:

```cmd
latte -c -a 10.0.0.4:5005 -i 65100
```

Warten Sie auf die Ergebnisse. Je nachdem, wie weit die VMs voneinander entfernt sind, kann es einige Minuten dauern, bis der Vorgang abgeschlossen ist. Erwägen Sie, mit weniger Iterationen zu beginnen, um den Erfolg zu testen, bevor Sie längere Tests durchführen.



## <a name="testing-vms-running-linux"></a>Testen von VMs mit Linux

Verwenden Sie SockPerf. Dieses Tool ist unter [https://github.com/mellanox/sockperf](https://github.com/mellanox/sockperf) verfügbar.

### <a name="install-sockperf-on-the-vms"></a>Installieren von SockPerf auf den virtuellen Computern

Führen Sie diese Befehle zum Vorbereiten von SockPerf auf Ihren virtuellen Computern (jeweils SENDER und EMPFÄNGER) auf den Linux-VMs aus: Es werden Befehle für die wichtigsten Distributionen bereitgestellt.

#### <a name="for-rhel--centos-follow-these-steps"></a>Gehen Sie für RHEL/CentOS folgendermaßen vor:
```bash
#CentOS / RHEL - Install GIT and other helpful tools
    sudo yum install gcc -y -q
    sudo yum install git -y -q
    sudo yum install gcc-c++ -y
    sudo yum install ncurses-devel -y
    sudo yum install -y automake
    sudo yum install -y autoconf
```

#### <a name="for-ubuntu-follow-these-steps"></a>Gehen Sie für Ubuntu folgendermaßen vor:
```bash
#Ubuntu - Install GIT and other helpful tools
    sudo apt-get install build-essential -y
    sudo apt-get install git -y -q
    sudo apt-get install -y autotools-dev
    sudo apt-get install -y automake
    sudo apt-get install -y autoconf
```

#### <a name="for-all-distros-copy-compile-and-install-sockperf-according-to-the-following-steps"></a>Kopieren, kompilieren und installieren Sie SockPerf für alle Distributionen anhand der folgenden Schritte:
```bash
#Bash - all distros

#From bash command line (assumes git is installed)
git clone https://github.com/mellanox/sockperf
cd sockperf/
./autogen.sh
./configure --prefix=

#make is slower, may take several minutes
make

#make install is fast
sudo make install
```

### <a name="run-sockperf-on-the-vms"></a>Ausführen von SockPerf auf den virtuellen Computern

Nachdem die SockPerf-Installation abgeschlossen ist, können die Latenztests auf den VMs ausgeführt werden. 

Starten Sie zunächst SockPerf auf dem EMPFÄNGER.

Jede verfügbare Portnummer ist in Ordnung. In diesem Beispiel wird Port 12345 verwendet:
```bash
#Server/Receiver - assumes server's IP is 10.0.0.4:
sudo sockperf sr --tcp -i 10.0.0.4 -p 12345
```
Nachdem der Server nun lauscht, kann der Client mit dem Senden von Paketen an den Server am entsprechenden Port beginnen, in diesem Fall 12345.

Ungefähr 100 Sekunden sind ausreichend, um repräsentative Ergebnisse zurückzugeben, wie im folgenden Beispiel gezeigt:
```bash
#Client/Sender - assumes server's IP is 10.0.0.4:
sockperf ping-pong -i 10.0.0.4 --tcp -m 350 -t 101 -p 12345  --full-rtt
```

Warten Sie auf die Ergebnisse. Abhängig von der Entfernung der virtuellen Computer zueinander unterscheidet sich die Anzahl der Iterationen. Erwägen Sie, mit kürzeren Tests von etwa 5 Sekunden zu beginnen, um den Erfolg zu testen, bevor Sie längere Tests durchführen.

In diesem SockPerf-Beispiel wird eine Nachrichtengröße von 350 Byte verwendet, da es sich dabei um ein typisches Paket mit durchschnittlicher Größe handelt. Die Nachrichtengröße kann höher oder niedriger angepasst werden, damit die erzielten Ergebnisse der ausgeführten Workload auf den virtuellen Computern genauer entsprechen.


## <a name="next-steps"></a>Nächste Schritte
* Verbessern der Latenz mit [Azure-Näherungsplatzierungsgruppen](https://docs.microsoft.com/azure/virtual-machines/linux/co-location)
* Erfahren Sie, wie Sie das [Netzwerk für virtuelle Computer für Ihr Szenario optimieren](../virtual-network/virtual-network-optimize-network-bandwidth.md).
* Lesen Sie mehr über die [Zuweisung von Bandbreite zu virtuellen Computern](../virtual-network/virtual-machine-network-throughput.md).
* Weitere Informationen finden Sie unter [Azure Virtual Network – häufig gestellte Fragen (FAQs)](../virtual-network/virtual-networks-faq.md).