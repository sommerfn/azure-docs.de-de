---
title: Unterstützte Betriebssystem, Containerengines – Azure IoT Edge | Microsoft-Dokumentation
description: Erfahren Sie, welche Betriebssysteme zur Ausführung von Azure IoT Edge-Daemon und -Runtime verwendet werden können, und welche Containerengines für Ihre Produktionsgeräte unterstützt werden.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 07/22/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 62b8ed553e3b4cec3750dae4f0426b6f0dd38855
ms.sourcegitcommit: c556477e031f8f82022a8638ca2aec32e79f6fd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2019
ms.locfileid: "68414393"
---
# <a name="azure-iot-edge-supported-systems"></a>Von Azure IoT Edge unterstützte Systeme

In diesem Artikel erhalten Sie Informationen darüber, welche Systeme und Komponenten (offiziell oder als Vorschauversion) von IoT Edge unterstützt werden. 

Wenn bei der Verwendung von Azure IoT Edge Probleme auftreten, gibt es mehrere Möglichkeiten, um Unterstützung zu erhalten. Versuchen Sie es mit einer der folgenden Supportoptionen:

**Melden von Fehlern:** Der Großteil der Entwicklungsarbeiten für Azure IoT Edge findet im Rahmen des entsprechenden Open-Source-Projekts statt. Fehler können auf der [Problemseite](https://github.com/azure/iotedge/issues) des Projekts gemeldet werden. Fehlerbehebungen aus dem Projekt werden zeitnah in Produktupdates integriert.

**Microsoft-Kundendienstteam:** Benutzer mit einem [Supportplan](https://azure.microsoft.com/support/plans/) können direkt über das [Azure-Portal](https://ms.portal.azure.com/signin/index/?feature.settingsportalinstance=mpac) ein Supportticket erstellen, um sich an das Microsoft-Kundendienstteam zu wenden.

**Funktionsanfragen:** Angefragte Funktionen werden von Azure IoT Edge über die [User Voice-Seite](https://feedback.azure.com/forums/907045-azure-iot-edge) des Produkts verfolgt.

## <a name="container-engines"></a>Container-Engines

Azure IoT Edge-Module werden als Container implementiert, für deren Start IoT Edge eine Containerengine benötigt. Mit „moby-engine“ stellt Microsoft eine Container-Engine bereit, um diese Anforderung zu erfüllen. Diese Containerengine basiert auf dem Open-Source-Projekt „Moby“. Weitere gängige Container-Engines sind Docker CE und Docker EE. Diese basieren ebenfalls auf dem Open-Source-Projekt „Moby“ und sind mit Azure IoT Edge kompatibel. Microsoft bietet bestmöglichen Support für Systeme mit diesen Containerengines, kann aber keine Fehlerbehebungen für diese bereitstellen. Aus diesem Grund empfiehlt Microsoft, für Produktionssysteme die Moby-Engine zu verwenden.

<br>
<center>

![Moby als Containerruntime](./media/support/only-moby-for-production.png)
</center>

## <a name="operating-systems"></a>Betriebssysteme
Azure IoT Edge ist für die meisten containerfähigen Betriebssysteme geeignet. Allerdings werden nicht alle diese Systeme gleich unterstützt. Betriebssysteme sind in Ebenen unterteilt, die jeweils den Grad der zu erwartenden Unterstützung angeben.
* Systeme der Ebene 1 werden unterstützt. Bei Systemen der Ebene 1, Microsoft:
    * Microsoft führt für dieses Betriebssystem automatisierte Tests durch.
    * Microsoft stellt Installationspakete für sie bereit.
* Systeme der Ebene 2 sind mit Azure IoT Edge kompatibel und relativ einfach zu verwenden. Bei Systemen der Ebene 2:
    * Microsoft hat Ad-hoc-Tests auf den Plattformen durchgeführt oder weiß, dass ein Partner Azure IoT Edge erfolgreich auf der Plattform ausführt.
    * Installationspakete für andere Plattformen können ggf. auch für diese Plattformen verwendet werden.
    
Die Familie des Hostbetriebssystems muss mit der Familie des Gastbetriebssystems, das im Container eines Moduls verwendet wird, immer übereinstimmen. Mit anderen Worten: Sie können unter Linux nur Linux-Container und unter Windows nur Windows-Container verwenden. Bei Verwendung von Windows werden nur Container mit Prozessisolation unterstützt, keine Container mit Hyper-V-Isolation.  

<br>
<center>

![Hostbetriebssystem stimmt mit Gastbetriebssystem überein.](./media/support/edge-on-device.png)
</center>

### <a name="tier-1"></a>Ebene 1

Die in der folgenden Tabelle aufgeführten Systeme (entweder allgemein verfügbare Versionen oder Vorschauversionen) werden von Microsoft unterstützt und mit jedem neuen Release getestet. 

| Betriebssystem | AMD64 | ARM32v7 | ARM64 |
| ---------------- | ----- | ------- | ----- |
| Raspbian-stretch |  | ![Raspbian Stretch + ARM32v7](./media/tutorial-c-module/green-check.png) | Öffentliche Vorschau |
| Ubuntu Server 16.04 | ![Ubuntu Server 16.04 + AMD64](./media/tutorial-c-module/green-check.png) |  |  |
| Ubuntu Server 18.04 | ![Ubuntu Server 18.04 + AMD64](./media/tutorial-c-module/green-check.png) |  |  |
| Windows 10 IoT Enterprise, Build 17763 | ![Windows 10 IoT Enterprise + AMD64](./media/tutorial-c-module/green-check.png) |  |  |
| Windows Server 2019, Build 17763 | ![Windows Server 2019 + AMD64](./media/tutorial-c-module/green-check.png) |  |  |
| Windows Server IoT 2019, Build 17763 | ![Windows Server IoT 2019 + AMD64](./media/tutorial-c-module/green-check.png) |  |  |
| Windows 10 IoT Core, Build 17763 | Öffentliche Vorschau |  |  |


Die oben aufgeführten Windows-Betriebssysteme stellen die Anforderungen für Geräte dar, die Windows-Container unter Windows ausführen. Dies ist die einzige unterstützte Konfiguration für die Produktion. Die Azure IoT Edge-Installationspakete für Windows ermöglichen die Verwendung von Linux-Containern unter Windows, diese Konfiguration ist jedoch nur für Entwicklungs- und Testzwecke ausgelegt. Die Verwendung von Linux-Containern unter Windows ist keine unterstützte Konfiguration für die Produktion. Jede Version von Windows 10, Build 14393 oder höher, und Windows Server 2016 oder höher kann für dieses Entwicklungsszenario verwendet werden.

### <a name="tier-2"></a>Ebene 2

Die in der folgenden Tabelle aufgeführten Systeme gelten als kompatibel mit Azure IoT Edge, werden jedoch nicht aktiv getestet oder verwaltet. 

| Betriebssystem | AMD64 | ARM32v7 | ARM64 |
| ---------------- | ----- | ------- | ----- |
| CentOS 7.5 | ![CentOS + AMD64](./media/tutorial-c-module/green-check.png) | ![CentOS + ARM32v7](./media/tutorial-c-module/green-check.png) | ![CentOS + ARM64](./media/tutorial-c-module/green-check.png) |
| Debian 8 | ![Debian 8 + AMD64](./media/tutorial-c-module/green-check.png) | ![Debian 8 + ARM32v7](./media/tutorial-c-module/green-check.png) | ![Debian 8 + ARM64](./media/tutorial-c-module/green-check.png) |
| Debian 9 | ![Debian 9 + AMD64](./media/tutorial-c-module/green-check.png) | ![Debian 9 + ARM32v7](./media/tutorial-c-module/green-check.png) | ![Debian 9 + ARM64](./media/tutorial-c-module/green-check.png) |
| Debian 10<sup>1</sup> | ![Debian 10 + AMD64](./media/tutorial-c-module/green-check.png) | ![Debian 10 + ARM32v7](./media/tutorial-c-module/green-check.png) | ![Debian 10 + ARM64](./media/tutorial-c-module/green-check.png) |
| RHEL 7.5 | ![RHEL 7.5 + AMD64](./media/tutorial-c-module/green-check.png) | ![RHEL 7.5 + ARM32v7](./media/tutorial-c-module/green-check.png) | ![RHEL 7.5 + ARM64](./media/tutorial-c-module/green-check.png) |
| Ubuntu 16.04 | ![Ubuntu 16.04 + AMD64](./media/tutorial-c-module/green-check.png) | ![Ubuntu 16.04 + ARM32v7](./media/tutorial-c-module/green-check.png) | ![Ubuntu 16.04 + ARM64](./media/tutorial-c-module/green-check.png) |
| Ubuntu 18.04 | ![Ubuntu 18.04 + AMD64](./media/tutorial-c-module/green-check.png) | ![Ubuntu 18.04 + ARM32v7](./media/tutorial-c-module/green-check.png) | ![Ubuntu 18.04 + ARM64](./media/tutorial-c-module/green-check.png) |
| Wind River 8 | ![Wind River 8 + AMD64](./media/tutorial-c-module/green-check.png) |  |  |
| Yocto | ![Yocto + AMD64](./media/tutorial-c-module/green-check.png) | ![Yocto + ARM32v7](./media/tutorial-c-module/green-check.png) | ![Yocto + ARM64](./media/tutorial-c-module/green-check.png) |
| Raspbian Buster<sup>1</sup> |  | ![Raspbian Buster + ARM32v7](./media/tutorial-c-module/green-check.png) | ![Raspbian Buster + ARM64](./media/tutorial-c-module/green-check.png) |

<sup>1</sup> Debian 10-Systeme, einschließlich Raspian Buster, verwenden eine OpenSSL-Version, die IoT Edge nicht unterstützt. Verwenden Sie den folgenden Befehl, um vor der Installation von IoT Edge eine frühere Version zu installieren: 

```bash
sudo apt-get install libssl1.0.2
```

## <a name="virtual-machines"></a>Virtual Machines
Azure IoT Edge kann auf virtuellen Computern ausgeführt werden. Das Verwenden eines virtuellen Computers als IoT Edge-Gerät ist eine verbreitete Vorgehensweise, wenn Kunden eine vorhandene Infrastruktur um Edgeintelligenz erweitern möchten. Die Familie des Host-VM-Betriebssystems muss mit der Familie des Gastbetriebssystems übereinstimmen, das im Container eines Moduls verwendet wird. Diese Anforderung gilt auch, wenn Azure IoT Edge direkt auf einem Gerät ausgeführt wird. Azure IoT Edge ist unabhängig von der zugrunde liegenden Virtualisierungstechnologie und funktioniert auf virtuellen Computern, die von Plattformen wie Hyper-V und vSphere unterstützt werden.

<br>
<center>

![Azure IoT Edge auf einem virtuellen Computer](./media/support/edge-on-vm.png)
</center>

## <a name="minimum-system-requirements"></a>Mindestsystemanforderungen
Azure IoT Edge funktioniert einwandfrei auf Geräten, die klein wie ein Raspberry Pi3 sind, bis hin zur Größe von Serverhardware. Die Auswahl der richtigen Hardware für Ihr Szenario hängt von den Workloads ab, die Sie ausführen möchten. Die endgültige Entscheidung für ein Gerät zu treffen, kann kompliziert sein; allerdings können Sie mit der Prototyperstellung einer Lösung auf herkömmlichen Laptops oder Desktops problemlos beginnen.

Das Sammeln von Erfahrungen während der Prototyperstellung wird Sie bei Ihrer endgültigen Geräteauswahl leiten. Zu den Fragen, die Sie berücksichtigen sollten, gehören: 

* Wie viele Module umfasst Ihre Workload?
* Wie viele Schichten werden von den Containern in Ihren Modulen gemeinsam genutzt?
* In welcher Sprache werden Ihre Module geschrieben? 
* Wie viele Daten sollen Ihre Module verarbeiten?
* Benötigen Ihre Module spezielle Hardware zum Beschleunigen ihrer Workloads?
* Was sind die gewünschten Leistungsmerkmale Ihrer Lösung?
* Wie hoch ist Ihr Hardwarebudget?
