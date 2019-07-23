---
title: 'Häufig gestellte Fragen: VMware-Lösung von CloudSimple'
description: Häufig gestellte Fragen (FAQ) zu Azure VMware-Lösung von CloudSimple
author: sharaths-cs
ms.author: b-shsury
ms.date: 05/24/19
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: a8cc6cf834c54ca25c12a6d66675e4290fd66136
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2019
ms.locfileid: "67165813"
---
# <a name="frequently-asked-questions-about-vmware-solution-by-cloudsimple"></a>Häufig gestellte Fragen (FAQ) zu VMware-Lösung von CloudSimple

Häufig gestellte Fragen mit Antworten zu Azure-VMware-Lösung von CloudSimple, die Ihnen dabei helfen, den Dienst und seine Verwendungsweise zu verstehen. Die Fragen und Antworten sind in folgende drei Kategorien unterteilt:

* CloudSimple-Dienst
* Konnektivität
* Netzwerk
* Sicherheit
* Compute
* Storage
* VMware
* Azure-Integration
 
## <a name="cloudsimple-service"></a>CloudSimple-Dienst

**Was ist Azure-VMware-Lösung von CloudSimple?**

Azure VMware-Lösung von CloudSimple transformiert und erweitert VMware-Workloads in Minuten in private, dedizierte Clouds in Azure. Die Lösung stellt die Infrastruktur bereit, verwaltet diese und orchestriert Workloads zwischen lokalen Standorten und Azure. Da Ihre Apps sowohl lokal als auch in Azure exakt gleich ausgeführt werden, profitieren Sie von der Flexibilität und den Diensten der Cloud, ohne die Komplexität eines Neuentwurfs Ihrer App-Architektur. CloudSimple verringert Ihre Gesamtkosten mit einem Cloudverbrauchsmodell, das bedarfsgesteuerte Bereitstellung, nutzungsbasierte Bezahlung und Kapazitätsoptimierung bereitstellt. Informationen zu Funktionen, Vorteilen und Szenarien finden Sie unter [Was ist Azure-VMware-Lösung von CloudSimple?](cloudsimple-vmware-solutions-overview.md).

**Was ist eine private CloudSimple-Cloud?**

Sie stellen eine private, dedizierte Cloud bereit, die aus einer Hochleistungsumgebung für Compute, Speicher und Netzwerk besteht, die in der Microsoft Azure-Infrastruktur (Hardware und Datencenterplatz) an Azure-Standorten bereitgestellt wird. Die private Cloud stellt eine native VMware-Plattform als Dienst bereit. Formuliert in VMware-Begriffen: Jede private Cloud enthält genau eine Instanz von vCenter Server. vCenter Server verwaltet mehrere ESXi-Knoten, die sich in einem oder mehreren vSphere-Clustern befinden, zusammen mit dem entsprechenden vSAN-Speicher. Ein CloudSimple-Dienst kann mehrere private Clouds in Ihrem Azure-Abonnement enthalten. Weitere Informationen zu privaten Clouds finden Sie unter [Übersicht über private CloudSimple-Clouds](cloudsimple-private-cloud.md).

**Wo ist der CloudSimple-Dienst verfügbar?**

CloudSimple ist in den Regionen „USA, Osten“ und „USA, Westen“ verfügbar.

**Wie aktiviere ich mein Abonnement für CloudSimple?**

Wenden Sie sich an Ihren Microsoft-Kundenbetreuer unter [azurevmwaresales@microsoft.com](mailto:azurevmwaresales@microsoft.com), um Ihr Abonnement für den CloudSimple-Dienst zu aktivieren. Geben Sie in der E-Mail Ihre Abonnement-ID an, für die der CloudSimple-Dienst aktiviert werden soll. 

**Wie greife ich auf das CloudSimple-Portal zu?**

Sie greifen aus dem Azure-Portal auf das CloudSimple-Portal zu. Informationen dazu, wie Sie auf das CloudSimple-Portal zugreifen, finden Sie unter [Access the VMware Solution by CloudSimple portal from the Azure portal](https://docs.azure.cloudsimple.com/access-cloudsimple-portal).

**Wie erhöhe ich die Kapazität für eine private Cloud?**

Sie stellen Knoten aus dem Azure-Portal bereit und erweitern Ihre private Cloud aus dem CloudSimple-Portal. Sie können Ihre private Cloud erweitern, indem Sie Knoten zu einem vorhandenen vSphere-Cluster hinzufügen, oder indem Sie einen neuen vSphere-Cluster erstellen. Informationen zur Vorgehensweise finden Sie unter [Expand a CloudSimple Private Cloud](https://docs.azure.cloudsimple.com/expand-private-cloud).

**Was geschieht mit meiner privaten Cloud während einer Wartung?**

CloudSimple sendet Tage vor einer geplanten Wartung entsprechende Benachrichtigungen. Die Wartung erfolgt auf unterbrechungsfreie Weise, um die Verfügbarkeit Ihrer privaten Cloud sicherzustellen. Die Wartung kann von einem der folgenden Typen sein:

- **CloudSimple-Infrastruktur**: Die CloudSimple-Infrastruktur ist auf Hochverfügbarkeit ausgelegt. Während einer Wartung werden Konnektivität und Verfügbarkeit Ihrer privaten Cloud sichergestellt, indem redundante Komponenten immer einzeln ohne Beeinträchtigung aktualisiert werden. Sie haben Zugriff auf die vCenter Server-Instanz Ihrer private Cloud, auf alle virtuellen Computer, auf die Internetverbindung aus Ihrer Private Cloud und auf die Verbindungen mit lokalen Standorten oder Azure.
- **CloudSimple-Portal**: Während einer Wartung sind einige Funktionen im CloudSimple-Portal möglicherweise nicht verfügbar oder möglicherweise deaktiviert. Die Wartungsbenachrichtigung enthält Informationen dazu, welche Aktionen im Portal ausgeführt werden können.

## <a name="connectivity"></a>Konnektivität

**Welche Optionen habe ich für die Konnektivität mit meinem CloudSimple-Regionsnetzwerk?**

CloudSimple stellt drei verschiedene Konnektivitätsoptionen für die Verbindung mit Ihrem CloudSimple-Regionsnetzwerk bereit. Alle drei Optionen können zusammen verwendet werden:

- Azure ExpressRoute-Verbindung von Ihrem lokalen Datencenter mit dem CloudSimple-Regionsnetzwerk: eine sichere private Verbindung mit hoher Geschwindigkeit und geringer Latenz, die Ihre lokale ExpressRoute-Leitung mit Ihrer CloudSimple ExpressRoute-Leitung verbindet, indem Global Reach verwendet wird. Informationen zum Einrichten der Verbindung finden Sie unter [Connect from on-premises to CloudSimple using ExpressRoute](https://docs.azure.cloudsimple.com/on-premises-connection) (Herstellen einer Verbindung von einem lokalen Datencenter mit CloudSimple über ExpressRoute).
- ExpressRoute-Verbindung von Ihrem virtuellen Azure-Netzwerk mit Ihrem CloudSimple-Regionsnetzwerk: eine sichere private Verbindung mit hoher Geschwindigkeit und geringer Latenz, die Ihr virtuelles Netzwerk in Azure mit Ihrer CloudSimple ExpressRoute-Leitung verbindet, indem virtuelle Netzwerkgateways verwendet werden. Informationen zum Einrichten der Verbindung finden Sie unter [Connect your CloudSimple Private Cloud environment to the Azure virtual network using ExpressRoute](https://docs.azure.cloudsimple.com/azure-expressroute-connection) (Verbinden der Umgebung Ihrer privaten CloudSimple-Cloud mit dem virtuellen Azure-Netzwerk über ExpressRoute).
- Site-to-Site-VPN-Verbindung von Ihrem lokalen Datencenter mit Ihrem CloudSimple-Regionsnetzwerk: eine sichere private Netzwerkverbindung von Ihrem lokalen VPN-Gerät mit Ihrer privaten CloudSimple-Cloudregion. Informationen zum Einrichten der Verbindung finden Sie unter [Configure a VPN connection to your CloudSimple Private Cloud](https://docs.azure.cloudsimple.com/set-up-vpn) (Konfigurieren einer VPN-Verbindung mit Ihrer privaten CloudSimple-Cloud).

**Wie stelle ich eine Verbindung mit einer privaten Cloud her?**

Details Ihrer privaten Cloud können Sie im CloudSimple-Portal anzeigen. Um eine Verbindung mit dem vCenter herzustellen, das Ihrer privaten Cloud entspricht, vergewissern Sie sich, dass eine Netzwerkverbindung über eine Site-to-Site-, Point-to-Site- oder ExpressRoute-Verbindung hergestellt wurde. Starten Sie dann das CloudSimple-Portal aus dem Azure-Portal. Wählen Sie **Launch vSphere client** auf der Startseite oder auf der Seite mit den Details der privaten Cloud aus.

**Worin besteht der Vorteil einer ExpressRoute-Leitung?**

Eine Azure ExpressRoute-Leitung bietet eine sichere Verbindung mit hoher Geschwindigkeit und niedriger Latenz. CloudSimple bietet eine dedizierte ExpressRoute-Leitung pro Region pro Kunde. Durch Verwenden dieser Leitung können Sie eine sichere Verbindung vom lokalen Datencenter und von Ihrem Azure-Abonnement herstellen.

**Wie hoch sind die Netzwerkkosten für Verbinden mit und von CloudSimple? Gibt es Gebühren für ausgehenden Datenverkehr von CloudSimple an Azure und umgekehrt? Gibt es Gebühren für ausgehenden Datenverkehr zwischen Regionen?**

Es gibt keine Gebühren für ausgehenden Netzwerkdatenverkehr. Für ausgehenden Datenverkehr aus Ihrem virtuellen Netzwerk oder von einer lokalen ExpressRoute-Leitung gelten die Standardtarife von Azure.

## <a name="networking"></a>Netzwerk

**Welche Netzwerkfunktionen sind für meine private Cloud verfügbar?**

Sie können VLANs und deren Subnetze und Firewalltabellen bereitstellen. Sie können öffentliche IP-Adressen zuweisen und diese einem virtuellen Computer zuordnen, der in Ihrer privaten Cloud ausgeführt wird. Weitere Informationen hierzu finden Sie unter [Übersicht über VLANs und Subnetze](cloudsimple-vlans-subnets.md), [Übersicht über Firewalltabellen](cloudsimple-firewall-tables.md) und [Öffentliche CloudSimple-IP-Adresse – Übersicht](cloudsimple-public-ip-address.md).

**Wie richte ich verschiedene Subnetze für meine Anwendungen in meiner privaten Cloud ein?**

Sie können VLANs in Ihrer privaten Cloud aus Ihrem CloudSimple-Portal erstellen. Nachdem Sie das VLAN erstellt haben, können Sie über das VLAN eine verteilte Portgruppe im vCenter Ihrer privaten Gruppe erstellen und virtuelle Computer erstellen, die mit der verteilten Portgruppe verbunden sind. Sie können eine Firewalltabelle für das VLAN oder Subnetz aktivieren und Firewallregeln definieren, um den Netzwerkdatenverkehr zu schützen.

**Welche Firewalleinstellungen sind für meine privaten Clouds verfügbar?**

Sie können Regeln für den Datenverkehr in Nord-Süd- und in Ost-West-Richtung konfigurieren. Die Regeln werden in einer Firewalltabelle definiert. Die Firewalltabelle kann VLANs in Ihrer privaten Cloud zugeordnet werden. Informationen über die Einrichtungsvorgehensweise finden Sie unter [Set up firewall tables and rules for Private Clouds](https://docs.azure.cloudsimple.com/firewall).

**Kann ich öffentliche IP-Adressen für virtuelle Computer in der Umgebung meiner privaten Cloud zuweisen?**

Im CloudSimple-Portal können Sie ganz einfach eine neue öffentliche IP-Adresse reservieren und sie einer privaten IP-Adresse Ihres virtuellen Computers oder einer Appliance zuordnen. Sie können auch neue Firewallregeln erstellen oder vorhandene Firewallregeln anwenden, um Datenverkehr von bestimmten Ports und bestimmten Sätzen von IP-Adressen im Portal zuzulassen. Informationen über die Einrichtungsvorgehensweise finden Sie unter [Allocate public IP addresses for Private Cloud environment](https://docs.azure.cloudsimple.com/public-ips) (Zuordnen von öffentlichen IP-Adressen für die Umgebung einer privaten Cloud).

## <a name="security"></a>Sicherheit

**Welche Sicherheitsoptionen habe ich bei CloudSimple?**

Eine private CloudSimple-Cloud bietet die folgenden Sicherheitsfunktionen zum Schützen der Umgebung Ihrer privaten Cloud:

- **Verschlüsselung für ruhende Daten:** Sie können ruhende Daten, die sich im vSAN-Speicher in Ihrer privaten Cloud befinden, verschlüsseln. vSAN unterstützt einen externen Schlüsselverwaltungsserver, der in Ihrem virtuellen Azure-Netzwerk oder in Ihrer lokalen Umgebung bereitgestellt werden kann. Weitere Informationen finden Sie unter [Configure vSAN encryption for your CloudSimple Private Cloud](https://docs.azure.cloudsimple.com/vsan-encryption) (Konfigurieren der vSAN-Verschlüsselung für Ihre private CloudSimple-Cloud).
- **Netzwerksicherheit:** Sie können Firewallregeln verwenden, um den gesamten Netzwerkdatenverkehr zu kontrollieren, also den internetbezogenen Netzwerkdatenverkehr für Ihre private Cloud, den lokalen Netzwerkdatenverkehr und den Netzwerkdatenverkehr in Subnetzen Ihrer privaten Cloud.
- **Sichere, private Verbindung:** Eine sichere private Verbindung zwischen Ihrem lokalen Netzwerk und Ihrem Azure-Abonnement.

## <a name="compute"></a>Compute

**Welche Arten von Hosts sind verfügbar?**

CloudSimple bietet zwei Hosttypen:

* **CS28-Knoten**: CPU: 2 x 2,2 GHz, 28 Kerne insgesamt, 48 HT. RAM: 256 GB. Speicher: 1600 GB NVMe-Cache, 5760 GB Data (All-Flash). Netzwerk: 2 x 25 GbE NIC.
* **CS36-Knoten**: CPU: 2 x 2,3 GHz, 36 Kerne insgesamt, 72 HT. RAM: 512 GB. Speicher: 3200 GB NVMe-Cache, 11.520 GB Data (All-Flash). Netzwerk: 2 x 25 GbE NIC.

**Wie werden Hardwarefehler behandelt?**

Die gesamte CloudSimple-Infrastruktur wird fortwährend von der CloudSimple-Plattform und deren Service Operations-Teams überwacht. Wenn ein Hardwarefehler erkannt wurde, wird Ihrer privaten Cloud ein neuer Knoten hinzugefügt. Der ausgefallene Knoten wird entfernt, um die Hochverfügbarkeit Ihrer privaten Cloud sicherzustellen.

## <a name="storage"></a>Storage

**Welche Arten von Speicher werden in einer privaten Cloud unterstützt?**

CloudSimple bietet **All-Flash VMware vSAN-Speicher** für jede private Cloud. Jeder vSphere wird mit seinem eigenen vSAN-Datenspeicher erstellt. Weitere Informationen finden Sie unter [Private Cloud VMware components – vSAN storage](https://docs.azure.cloudsimple.com/vmware-components/#vsan-storage).

**Wird die Verschlüsselung von Daten unterstützt?**
Ja. Sie können den vSAN-Speicher in Ihrer privaten Cloud so einrichten, dass er einen KMS (Key Management Server, Schlüsselverwaltungsserver) verwendet, der lokal oder in Azure bereitgestellt wird, um Daten zu verschlüsseln, die im vSAN gespeichert sind.

**Wie werden fehlerhafte Datenträger behandelt?**

Die CloudSimple-Überwachung überwacht kontinuierlich alle Hardwarekomponenten der privaten Cloud. Wenn Datenträgerfehler erkannt werden oder ein Datenträger anhand einer heuristischen Methode als fehlerhaft identifiziert wird, wird der privaten Cloud automatisch ein neuer Knoten hinzugefügt. Der Knoten mit dem fehlerhaften oder beschädigten Datenträger wird aus der privaten Cloud entfernt.

## <a name="vmware"></a>VMware

**Wie führe ich umfangreiches Hochladen und Migrieren von Anwendungen und Daten von einem lokalen Standort aus?**

CloudSimple stellt eine native VMware vSphere-Lösung bereit. Jedes Tool, das für die Migration von Massendaten verwendbar ist, kann mit CloudSimple Private Cloud verwendet werden. Einige der verfügbaren Optionen sind:

- VMware HCX für die Massenmigration von Daten.
- „Kalte“ Migration von Daten aus einem lokalen Standort zu CloudSimple über Storage vMotion.

**Kann ich alle VMware-Tools installieren?**

CloudSimple stellt eine native VMware vSphere-Lösung bereit. Jedes Tool, das zum Verwalten einer lokalen vSphere-Umgebung verwendet wird, kann in CloudSimple verwendet werden. CloudSimple unterstützt das BYOL-Modell (Bring-Your-Own-License) für die Installation von VMware-Tools.

**Wie werden Updated und Upgrades verwaltet?**

CloudSimple verwaltet und aktualisiert alle Komponenten der Infrastruktur Ihrer privaten Cloud auf nahtlose und unterbrechungsfreie Art. Alle von VMware oder dem Hersteller der Infrastruktur freigegebenen Updates oder Sicherheitspatches werden zur Aktualisierung eingeplant, sobald sie von CloudSimple als qualifiziert eingestuft wurden.

CloudSimple führt keine Upgrades oder Updates für installierte Anwendungen in der privaten Cloud durch.

## <a name="azure-integration"></a>Azure-Integration

**Welche Azure-Dienste werden unterstützt?**

CloudSimple stellt eine Azure ExpressRoute-Leitung für Ihr Abonnement in Azure bereit. Alle Dienste, die in Ihrem Abonnement ausgeführt werden, haben Netzwerkkonnektivität mit Ihrer privaten Cloud und können eine Verbindung mit Ihrer privaten Cloud herstellen. Beispiele:

- **Azure Active Directory:** Verwenden Sie Azure Active Directory als Identitätsanbieter für Ihr CloudSimple vCenter.
- **Azure Storage**: Verwenden Sie Storage zum Speichern von Sicherungen, Images und anderen Daten aus Ihrer privaten Cloud.
- **Hybridanwendungen**: Sie können eine Anwendungsarchitektur erstellen, die öffentliche und private Clouds umfasst. Beispielsweise können Sie Webserver in Azure erstellen, die auf Anwendungs- und Datenbankserver in einer privaten CloudSimple-Cloud zugreifen.
- **Azure Monitor** und **Azure Security Center**: Eine Workload, die unter VMware ausgeführt wird, kann Monitor und Security Center für Protokollierung, Leistungsmetriken und Sicherheitsverwaltung verwenden.

**Wie ordne ich meine VMware-Mandanten in Azure zu?**

CloudSimple bietet die einzigartige Möglichkeit, Ihre virtuellen VMware-Computer in einer privaten Cloud über das Azure-Portal verwalten zu können. Ein vCenter-Ressourcenpool, der mit den von Ihnen gewünschten Ressourceneinschränkungen konfiguriert ist, kann vom globalen Administrator Ihrem Abonnement zugeordnet werden. 

**Welche Lizenzierungsvorteile erhalte ich mit Azure?**

Mit CloudSimple können Sie vom Azure-Hybridvorteil profitieren und bis zu 90 Prozent bei Lizenzen sparen, um Ihre Investitionen in Microsoft-Lizenzen zu bewahren und gleichzeitig Ihre Gesamtkosten im Vergleich zu anderen Clouds senken. Außerdem erhalten Sie verlängerte Sicherheitsupdates für Windows Server 2008 und Microsoft SQL Server 2008. Halten Sie Ihre Kosten niedrig mit BYOL in der Cloud für bekannte Apps wie Veeam, Zerto und andere. 
