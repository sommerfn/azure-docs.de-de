---
title: include file
description: include file
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: include
ms.date: 06/10/2018
ms.author: raynew
ms.custom: include file
ms.openlocfilehash: d77269c1e965d5bca1e32b756ef26e2c694e5c81
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73747844"
---
**Anforderungen an den Konfigurations- und Prozessserver**


## <a name="hardware-requirements"></a>Hardwareanforderungen

**Komponente** | **Anforderung** 
--- | ---
CPU-Kerne | 8 
RAM | 16 GB
Anzahl der Datenträger | 3, einschließlich Betriebssystem-Datenträger, Prozessservercache-Datenträger und Aufbewahrungslaufwerk für Failback 
Freier Speicherplatz (Prozessservercache) | 600 GB
Freier Speicherplatz (Aufbewahrungslaufwerk) | 600 GB
 | 

## <a name="software-requirements"></a>Softwareanforderungen

**Komponente** | **Anforderung** 
--- | ---
Betriebssystem | Windows Server 2012 R2 <br> Windows Server 2016
Gebietsschema des Betriebssystems | Englisch (en-us)
Windows Server-Rollen | Aktivieren Sie die folgenden Rollen nicht: <br> - Active Directory Domain Services <br>- Internetinformationsdienste <br> - Hyper-V 
Gruppenrichtlinien | Aktivieren Sie die folgenden Gruppenrichtlinien nicht: <br> - Zugriff auf Eingabeaufforderung verhindern <br> - Zugriff auf Programme zum Bearbeiten der Registrierung verhindern <br> - Vertrauenslogik für Dateianlagen <br> - Skriptausführung aktivieren <br> [Weitere Informationen](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)
IIS | - Keine bereits vorhandene Standardwebsite <br> - Keine bereits vorhandene Website oder Anwendung, die an Port 443 lauscht <br>- Aktivierte [anonyme Authentifizierung](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx) <br> - Aktivierte Einstellung [FastCGI](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx)
| 

## <a name="network-requirements"></a>Netzwerkanforderungen

**Komponente** | **Anforderung** 
--- | --- 
Art der IP-Adresse | statischen 
Ports | 443 (Steuerkanalorchestrierung)<br>9443 (Datentransport) 
NIC-Typ | VMXNET3 (wenn der Konfigurationsserver eine VMware-VM ist)
 |
**Internetzugriff** (der Server benötigt Zugriff auf diese URLs – direkt oder über einen Proxy):|
\*.backup.windowsazure.com | Wird für die Übertragung und Koordinierung replizierter Daten verwendet
\*.store.core.windows.net | Wird für die Übertragung und Koordinierung replizierter Daten verwendet
\*.blob.core.windows.net | Wird für den Zugriff auf das Speicherkonto verwendet, in dem replizierte Daten gespeichert werden
\*.hypervrecoverymanager.windowsazure.com | Für Vorgänge und Koordinierung der Replikationsverwaltung verwendet
https:\//management.azure.com | Für Vorgänge und Koordinierung der Replikationsverwaltung verwendet 
*.services.visualstudio.com | Wird für Telemetriezwecke (optional) verwendet
time.nist.gov | Wird zum Überprüfen der Zeitsynchronisierung zwischen der Systemzeit und der globalen Zeit verwendet
time.windows.com | Wird zum Überprüfen der Zeitsynchronisierung zwischen der Systemzeit und der globalen Zeit verwendet
| <ul> <li> https:\//login.microsoftonline.com </li><li> https:\//secure.aadcdn.microsoftonline-p.com </li><li> https:\//login.live.com </li><li> https:\//graph.windows.net </li><li> https:\//login.windows.net </li><li> https:\//www.live.com </li><li> https:\//www.microsoft.com </li></ul> | Bei der OVF-Einrichtung ist Zugriff auf diese URLs erforderlich. Die URLs werden für die Zugriffssteuerung und die Identitätsverwaltung durch Azure Active Directory verwendet.
https:\//dev.mysql.com/get/Downloads/MySQLInstaller/mysql-installer-community-5.7.20.0.msi  | So schließen Sie den MySQL-Download ab </br> In einigen Regionen wird der Download möglicherweise an die CDN-URL umgeleitet. Stellen Sie ggf. sicher, dass die CDN-URL auch in der Whitelist enthalten ist.
|

## <a name="required-software"></a>Erforderliche Software

**Komponente** | **Anforderung** 
--- | ---
VMware vSphere PowerCLI | [PowerCLI Version 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1) sollte installiert werden, wenn der Konfigurationsserver auf einer VMware-VM ausgeführt wird.
MySQL | MySQL sollte installiert sein. Sie können diese Komponente manuell installieren oder durch Azure Site Recovery installieren lassen. (Weitere Informationen finden Sie unter [Konfigurieren von Einstellungen](../articles/site-recovery/vmware-azure-deploy-configuration-server.md#configure-settings).)

## <a name="sizing-and-capacity-requirements"></a>Anforderungen an Größenanpassung und Kapazität

Die folgende Tabelle enthält die Kapazitätsanforderungen für den Konfigurationsserver. Wenn Sie mehrere virtuelle VMware-Computer replizieren, lesen Sie sich den Artikel [Überlegungen zur Kapazitätsplanung](../articles/site-recovery/site-recovery-plan-capacity-vmware.md) durch, und führen Sie das Tool [Azure Site Recovery-Bereitstellungsplaner](../articles/site-recovery/site-recovery-deployment-planner.md) aus.


**CPU** | **Memory** | **Cachedatenträger** | **Datenänderungsrate** | **Replizierte Computer**
--- | --- | --- | --- | ---
8 vCPUs<br/><br/> 2 Sockets * 4 Kerne \@ 2,5GHz | 16 GB | 300 GB | 500 GB oder weniger | < 100 Computer
12 vCPUs<br/><br/> 2 Sockets * 6 Kerne \@ 2,5GHz | 18 GB | 600 GB | 500 GB bis 1 TB | 100 bis 150 Computer
16 vCPUs<br/><br/> 2 Sockets * 8 Kerne \@ 2,5GHz | 32 GB | 1 TB | 1 bis 2 TB | 150 bis 200 Computer

