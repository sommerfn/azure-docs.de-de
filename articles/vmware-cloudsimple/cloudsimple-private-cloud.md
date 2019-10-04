---
title: Azure-VMware-Lösung von CloudSimple – private Clouds
description: Erfahren Sie mehr über private CloudSimple-Clouds und CloudSimple-Konzepte.
author: sharaths-cs
ms.author: dikamath
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 4fb930603455ed1a5df5d357fcab669f41a0c28c
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2019
ms.locfileid: "69877947"
---
# <a name="cloudsimple-private-cloud-overview"></a>Übersicht über private CloudSimple-Clouds

CloudSimple transformiert und erweitert VMware-Workloads in Minuten in öffentliche Clouds. Durch Verwenden des CloudSimple-Diensts können Sie VMware nativ in einer Azure-Bare-Metal-Infrastruktur bereitstellen. Ihre Bereitstellung befindet sich an Azure-Standorten und ist vollständig in den Rest der Azure-Cloud integriert.

Die CloudSimple-Lösung bietet vollständige VMware-Betriebskontinuität. Diese Lösung eröffnet Ihnen die folgenden Vorteile der öffentlichen Cloud:

* Elastizität
* Innovation
* Effizienz

Mit CloudSimple profitieren Sie von einem Cloudnutzungsmodell, mit dem Ihre Gesamtkosten verringert werden können. CloudSimple bietet außerdem bedarfsgesteuerte Bereitstellung, nutzungsbasierte Bezahlung und Kapazitätsoptimierung.

CloudSimple ist vollständig kompatibel mit Folgendem:

* Vorhandene Tools
* Fähigkeiten
* Prozesse

Dank dieser Kompatibilität können Ihre Teams, Workloads in der Azure-Cloud verwalten, ohne die folgenden Arten von Richtlinien zu beeinträchtigen:

* Netzwerk
* Sicherheit  
* Datenschutz  
* Audit

CloudSimple verwaltet die Infrastruktur und alle erforderlichen Netzwerk- und Verwaltungsdienste. Mit dem CloudSimple-Dienst kann sich Ihr das Team auf Folgendes konzentrieren:

* Geschäftswert
* Bereitstellung von Anwendungen
* Geschäftskontinuität
* Support
* Durchsetzung von Richtlinien

## <a name="private-cloud-environment-overview"></a>Übersicht über Umgebungen für private Clouds

Eine private Cloud ist ein isolierter VMware-Stapel, der Folgendes unterstützt:

* ESXi-Hosts
* vCenter
* vSAN
* NSX

Private Clouds werden über das CloudSimple-Portal verwaltet. Sie verfügen über einen eigenen vCenter-Server in einer eigenen Verwaltungsdomäne.

Der Stapel wird auf folgenden Komponenten ausgeführt:

* Dedizierte Knoten
* Isolierte Bare-Metal-Hardwareknoten

Benutzer nutzen den Stapel über systemeigene VMware-Tools, wozu folgende Tools gehören:

* vCenter
* NSX Manager

Sie können dedizierte Knoten in Azure-Standorten bereitstellen. Anschließend können Sie diese mit Azure und CloudSimple verwalten. Eine private Cloud besteht aus einem oder mehreren vSphere-Clustern, und jeder Cluster enthält 3 bis 16 Knoten.

Sie können eine private Cloud mit erworbenen Knoten mit nutzungsbasierter Bezahlung oder mit reservierten, dedizierten Knoten erstellen.

Sie können die private Cloud über die folgenden Verbindungen mit Ihrer lokalen Umgebung und dem Azure-Netzwerk verbinden:

* Schützen
* Privates VPN
* Azure ExpressRoute

Die Umgebung einer privaten Cloud ist so konzipiert, dass sie keinen Single Point of Failure aufweist:

* ESXi-Cluster sind mit vSphere Hochverfügbarkeit konfiguriert und in der Größe so ausgelegt, dass sie mindestens einen Ersatzknoten für Resilienz haben.
* vSAN stellt redundanten primären Speicher bereit. vSAN erfordert mindestens drei Knoten, um Schutz vor einem Ausfall eines einzelnen Knotens zu bieten. Sie können vSAN so konfigurieren, dass es eine höhere Resilienz für größere Cluster bietet.
* Sie können vCenter-, PSC- und NSX Manager-VMs mit RAID-10-Speicherrichtlinien konfigurieren, um sie vor Speicherausfällen zu schützen. vSphere-Hochverfügbarkeit schützt vor Knoten- und Netzausfällen.

## <a name="scenarios-for-deploying-a-private-cloud"></a>Szenarien für die Bereitstellung einer privaten Cloud

Im Folgenden finden Sie einige Anwendungsfälle für die Bereitstellung einer privaten Cloud.

### <a name="data-center-retirement-or-migration"></a>Außerbetriebnahme oder Migration eines Rechenzentrums

* Buchen Sie zusätzliche Kapazität, wenn Sie die Grenzen Ihres vorhandenen Rechenzentrums erreichen oder Hardware aktualisieren.
* Fügen Sie erforderliche Kapazität in der Cloud hinzu, und eliminieren Sie die Probleme, die beim Verwalten von Hardwareaktualisierungen auftreten können.
* Verringern Sie das Risiko und die Kosten von Cloudmigrationen im Vergleich zu zeitaufwändigen Konvertierungen oder Umstrukturierungen.
* Verwenden Sie vertraute VMware-Tools und Fähigkeiten, um Cloudmigrationen zu beschleunigen. Verwenden Sie in der Cloud Azure-Dienste, um Ihre Anwendungen in Ihrem Tempo zu modernisieren.

### <a name="expand-on-demand"></a>Bedarfsgesteuerte Erweiterung

* Erweitern Sie in die Cloud, um unerwartete Anforderungen erfüllen zu können, z. B. neue Entwicklungsumgebungen oder saisonale Kapazitätsspitzen.
* Erstellen Sie neue Kapazität nach Bedarf, und behalten Sie diese nur so lange, wie sie benötigt wird.
* Verringern Sie Ihre Vorabinvestitionen, beschleunigen Sie die Geschwindigkeit der Bereitstellung, und verringern Sie die Komplexität mit derselben Architektur und denselben Richtlinien sowohl lokal als auch in der Cloud.

### <a name="disaster-recovery-and-virtual-desktops-in-the-azure-cloud"></a>Notfallwiederherstellung und virtuelle Desktops in der Azure-Cloud

* Richten Sie Remotezugriff auf Daten, Apps und Desktops in der Azure-Cloud ein. Über Verbindungen mit hoher Bandbreite können Sie Daten schnell hoch- bzw. herunterladen, um nach Vorfällen eine Wiederherstellung vorzunehmen. Netzwerke mit niedriger Wartezeit bieten Ihnen schnelle Reaktionszeiten, die Benutzer von einer Desktop-App erwarten.

* Replizieren Sie Ihre gesamten Richtlinien und Netzwerke in der Cloud über das CloudSimple-Portal und vertraute VMware-Tools. Die Replikation verringert den Aufwand und das Risiko beim Erstellen und Verwalten von Notfallwiederherstellungs- und VDI-Implementierungen.

### <a name="high-performance-applications-and-databases"></a>Leistungsstarke Anwendungen und Datenbanken

* Führen Sie Ihre anspruchsvollsten Workloads mit der hyperkonvergenten Architektur von CloudSimple aus.
* Führen Sie Oracle, Microsoft SQL Server, Middleware-Systeme und leistungsstarke NoSQL-Datenbanken aus.
* Erleben Sie die Cloud als Ihr eigenes Rechenzentrum mit äußerst schnellen 25-Gbit/s-Netzwerkverbindungen. Hochgeschwindigkeitsverbindungen ermöglichen es Ihnen, hybride Apps auszuführen, die sich über lokale, VMware in Azure- und private Azure-Workloads erstrecken, ohne dass die Leistung beeinträchtigt wird.

### <a name="true-hybrid"></a>Eine echte Hybridlösung

* Vereinheitlichen Sie DevOps über VMware- und Azure-Dienste hinweg.
* Optimieren Sie die VMware-Verwaltung für Azure-Dienste und -Lösungen, die auf alle Workloads angewendet werden können.
* Greifen Sie auf Dienste für öffentliche Clouds zu, ohne Ihr Rechenzentrum erweitern oder Ihre Anwendungen neu strukturieren zu müssen.
* Zentralisieren Sie Identitäten, Zugriffssteuerungsrichtlinien, Protokollierung und Überwachung für VMware-Anwendungen in Azure.

## <a name="limits"></a>Einschränkungen

In der folgenden Tabelle sind die Knotenbeschränkungen für Ressourcen einer privaten Cloud aufgeführt.

| Resource | Begrenzung |
|----------|-------|
| Mindestanzahl von Knoten zum Erstellen einer privaten Cloud | 3 |
| Maximale Anzahl von Knoten in einem Cluster in einer privaten Cloud | 16 |
| Maximale Anzahl von Knoten in einer privaten Cloud | 64 |
| Mindestanzahl von Knoten in einem neuem Cluster | 3 |

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über das [Erstellen einer privaten Cloud](create-private-cloud.md).
* Erfahren Sie mehr über das [Konfigurieren einer privaten Cloudumgebung](quickstart-create-private-cloud.md).
