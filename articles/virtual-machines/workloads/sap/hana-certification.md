---
title: Zertifizierung von SAP HANA in Azure (große Instanzen) | Microsoft-Dokumentation
description: Zertifizierung von SAP HANA in Azure (große Instanzen)
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: gwallace
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/04/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d623a189da5c5eccd73018e1ae0b7e5564fe09ab
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2019
ms.locfileid: "67709683"
---
# <a name="certification"></a>Zertifizierung

Neben der NetWeaver-Zertifizierung erfordert SAP eine spezielle Zertifizierung für SAP HANA, damit SAP HANA auf bestimmten Infrastrukturen wie Azure IaaS unterstützt wird.

Der wichtigste SAP-Hinweis zu NetWeaver und in gewissem Maße zur SAP HANA-Zertifizierung ist [SAP-Hinweis 1928533 – SAP applications on Azure: Supported products and Azure VM types](https://launchpad.support.sap.com/#/notes/1928533) (SAP-Anwendungen unter Azure: Unterstützte Produkte und Azure-VM-Typen).

Die Zertifizierungsdatensätze für SAP HANA-Einheiten in Azure (große Instanzen) finden Sie auf der Website [Zertifizierte SAP HANA-IaaS-Plattformen](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure). 

Die SAP HANA-Typen in Azure (große Instanzen), auf die auf der Website „Zertifizierte SAP HANA-IaaS-Plattformen“ Bezug genommen wird, bieten Microsoft- und SAP-Kunden die Möglichkeit, große SAP Business Suite-, SAP BW-, S/4 HANA-, BW/4HANA- oder andere SAP HANA-Workloads in Azure bereitzustellen. Die Lösung beruht auf dem dedizierten SAP HANA-zertifizierten Hardwareumfeld ([SAP HANA Tailored Datacenter Integration – TDI](https://scn.sap.com/docs/DOC-63140)). Wenn Sie eine für SAP HANA TDI konfigurierte Lösung ausführen, werden alle SAP HANA-basierten Anwendungen (beispielsweise SAP Business Suite unter SAP HANA, SAP BW unter SAP HANA, S4/HANA und BW4/HANA) auf der Hardwareinfrastruktur ausgeführt.

Diese Lösung bietet im Vergleich zur Ausführung von SAP HANA auf VMs einen Vorteil: Es sind deutlich größere Speichervolumes verfügbar. Um diese Lösung nutzen zu können, müssen Sie die folgenden wichtigen Aspekte verstehen:

- Die SAP-Anwendungsschicht und Nicht-SAP-Anwendungen werden auf VMs ausgeführt, die in den üblichen Azure-Hardwareumfeldern gehostet werden.
- Die lokale(n) Infrastruktur, Rechenzentren und Anwendungsbereitstellungen des Kunden werden über ExpressRoute (empfohlen) oder ein virtuelles privates Netzwerk (VPN) mit der Cloudplattform verbunden. Auch Active Directory und DNS werden auf Azure erweitert.
- Die SAP HANA-Datenbankinstanz für HANA-Workload wird auf SAP HANA in Azure (große Instanzen) ausgeführt. Das Umfeld von HANA (große Instanz) ist mit dem Azure-Netzwerk verbunden, sodass auf VMs ausgeführte Software mit der HANA-Instanz interagieren kann, die in HANA (große Instanz) ausgeführt wird.
- Hardware von SAP HANA in Azure (große Instanzen) ist dedizierte Hardware, die in einer IaaS mit vorinstalliertem SUSE Linux Enterprise Server oder Red Hat Enterprise Linux bereitgestellt wird. Wie bei VMs sind Sie für weitere Updates und die Wartung des Betriebssystems zuständig.
- Die Installation von HANA oder zusätzlichen Komponenten, die zum Ausführen von SAP HANA auf Einheiten von HANA (große Instanz) erforderlich sind, liegt in Ihrer Verantwortung. Sie sind auch für den gesamten laufenden Betrieb und die laufende Verwaltung von SAP HANA in Azure zuständig.
- Zusätzlich zu den hier beschriebenen Lösungen können Sie andere Komponenten in Ihrem Azure-Abonnement installieren, das eine Verbindung mit SAP HANA in Azure (große Instanzen) herstellt. Hierzu zählen beispielsweise Komponenten, die die Kommunikation oder direkte Verbindung mit der SAP HANA-Datenbank ermöglichen (beispielsweise Jumpserver, RDP-Server, SAP HANA Studio, SAP Data Services für SAP BI-Szenarien oder Netzwerküberwachungslösungen).
- Wie in Azure bietet HANA (große Instanz) Unterstützung für Funktionen für Hochverfügbarkeit und Notfallwiederherstellung.

**Nächste Schritte**
- Lesen Sie [Verfügbare SKUs für HLI](hana-available-skus.md). 