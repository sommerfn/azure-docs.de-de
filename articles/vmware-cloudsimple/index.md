---
title: Azure-VMware-Lösung von CloudSimple
description: Dokumentationsportal für Azure-VMware-Lösung von CloudSimple.
author: sharaths-cs
ms.author: b-mashar
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 936a9eb4d9b33b45800fab213fc72a306a04146e
ms.sourcegitcommit: 47b00a15ef112c8b513046c668a33e20fd3b3119
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2019
ms.locfileid: "69972739"
---
# <a name="azure-vmware-solution-by-cloudsimple"></a>Azure-VMware-Lösung von CloudSimple

Willkommen beim zentralen Portal, in dem Sie Hilfe zur Azure-VMware-Lösung von CloudSimple erhalten.
Auf der Dokumentationswebsite finden Sie Informationen zu den folgenden Themen:

## <a name="overview"></a>Übersicht

Erfahren Sie mehr zur Azure-VMware-Lösung von CloudSimple

* Informationen zu den Features, Vorteilen und Nutzungsszenarien finden Sie unter [Was ist die Azure-VMware-Lösung von CloudSimple?](cloudsimple-vmware-solutions-overview.md).
* Sehen Sie sich die [Schlüsselkonzepte für die Verwaltung](key-concepts.md) an.

## <a name="quickstart"></a>Schnellstart

Erfahren Sie mehr zu den ersten Schritten mit der Lösung.

* Machen Sie sich mit dem [Initialisieren und Bereitstellen von Kapazität](quickstart-create-cloudsimple-service.md) vertraut.
* Informieren Sie sich unter [Konfigurieren einer Private Cloud-Umgebung](quickstart-create-private-cloud.md) darüber, wie Sie eine neue VMware-Umgebung erstellen.
* Erfahren Sie mehr zur Vereinheitlichung der Verwaltung für VMware und Azure, indem Sie den Artikel [Nutzen von virtuellen VMware-Computern in Azure](quickstart-create-vmware-virtual-machine.md) lesen.

## <a name="concepts"></a>Konzepte

Weitere Informationen zu den folgenden Konzepten

* [CloudSimple-Dienst](cloudsimple-service.md) (auch als „Azure-VMware-Lösung von CloudSimple – Dienst“ bezeichnet). Diese Ressource muss einmal pro Region erstellt werden.
* Erwerben Sie Kapazität für Ihre Umgebung, indem Sie eine oder mehrere Ressourcen vom Typ [CloudSimple-Knoten](cloudsimple-node.md) erstellen. Diese Ressourcen werden auch als „Azure VMware-Lösung von CloudSimple – Knoten“ bezeichnet.
* Initialisieren und konfigurieren Sie die VMware-Umgebung mit den [privaten Clouds](cloudsimple-private-cloud.md).
* Vereinheitlichen Sie die Verwaltung, indem Sie [virtuelle CloudSimple-Computer](cloudsimple-virtual-machines.md) verwenden (auch als „Azure VMware-Lösung von CloudSimple – Virtueller Computer“ bezeichnet).
* Entwerfen Sie das zugrunde liegende Netzwerk mit [VLANs/Subnetzen](cloudsimple-vlans-subnets.md).
* Segmentieren und schützen Sie Ihr zugrunde liegendes Netzwerk mit der Ressource [Firewalltabelle](cloudsimple-firewall-tables.md).
* Verwenden Sie [VPN Gateway-Instanzen](cloudsimple-vpn-gateways.md), um sicheren Zugriff auf Ihre VMware-Umgebungen per WAN zu erhalten.
* Ermöglichen Sie den öffentlichen Zugriff auf Workloads per [öffentlicher IP-Adresse](cloudsimple-public-ip-address.md).
* Richten Sie die Konnektivität mit virtuellen Azure-Netzwerken und lokalen Netzwerken per [Azure-Netzwerkverbindung](cloudsimple-azure-network-connection.md) ein.
* Konfigurieren Sie Ziele für Benachrichtigungs-E-Mails per [Kontoverwaltung](cloudsimple-account.md).
* Zeigen Sie Protokolle von Benutzer- und Systemaktivitäten über die Fenster der [Aktivitätenverwaltung](cloudsimple-activity.md) an.
* Machen Sie sich mit den verschiedenen [VMware-Komponenten](vmware-components.md) vertraut.

## <a name="tutorials"></a>Lernprogramme

Informieren Sie sich darüber, wie Sie häufige Aufgaben durchführen, z. B.:

* [Erstellen Sie einen CloudSimple-Dienst](create-cloudsimple-service.md) pro Region, in der Sie VMware-Umgebungen bereitstellen möchten.
* Verwalten Sie die Kerndienstfunktionalität im [CloudSimple-Portal](access-cloudsimple-portal.md).
* Aktivieren Sie die Kapazität, und optimieren Sie die Abrechnung für Ihre Infrastruktur, indem Sie [CloudSimple-Knoten erwerben](create-nodes.md).
* Verwalten von VMware-Umgebungskonfigurationen mit Private Clouds. Sie können Private Clouds [erstellen](create-private-cloud.md), [verwalten](manage-private-cloud.md), [erweitern](expand-private-cloud.md) und [verkleinern](shrink-private-cloud.md).
* Ermöglichen Sie die einheitliche Verwaltung, indem Sie [Azure-Abonnements zuordnen](azure-subscription-mapping.md).
* Überwachen Sie Benutzer- und Systemaktivitäten über die [Seiten mit den Aktivitäten](monitor-activity.md).
* Konfigurieren Sie das Netzwerk für Ihre Umgebungen, indem Sie [Subnetze erstellen und verwalten](create-vlan-subnet.md).
* Segmentieren und schützen Sie Ihre Umgebung, indem Sie [Firewalltabellen und -regeln](firewall.md) verwenden.
* Ermöglichen Sie den ein- und ausgehenden Internetzugriff für Workloads, indem Sie [öffentliche IP-Adressen zuordnen](public-ips.md).
* Aktivieren Sie die Konnektivität mit Ihren internen Netzwerken oder Clientarbeitsstationen, indem Sie die [VPN-Einrichtung durchführen](vpn-gateway.md).
* Ermöglichen Sie auch die Kommunikation aus Ihren [lokalen Umgebungen](on-premises-connection.md) mit [virtuellen Azure-Netzwerken](virtual-network-connection.md).
* Konfigurieren Sie Warnungsziele, und zeigen Sie die insgesamt erworbene Kapazität in der [Kontozusammenfassung](account.md) an.
* Zeigen Sie die [Benutzer](users.md) an, die auf das CloudSimple-Portal zugegriffen haben.
* Verwalten von virtuellen VMware-Computern über das Azure-Portal:
    * [Erstellen Sie virtuelle Computer](azure-create-vm.md) im Azure-Portal.
    * [Verwalten Sie virtuelle Computer](azure-manage-vm.md), die Sie erstellt haben.

## <a name="how-to-guides"></a>Anleitungen

In diesen Anleitungen werden Lösungen für folgende Ziele beschrieben:

* [Schützen Ihrer Umgebung](private-cloud-secure.md)
* Installieren Sie Tools von Drittanbietern, und aktivieren Sie zusätzliche Benutzer und eine Quelle für die externe Authentifizierung in vSphere, indem Sie die [Rechteausweitung](escalate-privileges.md) nutzen.
* Konfigurieren Sie den Zugriff auf verschiedene VMware-Dienste, indem Sie das [lokale DNS konfigurieren](on-premises-dns-setup.md).
* Aktivieren Sie die Zuordnung von Namen und Adressen für Ihre Workloads, indem Sie [DNS und DHCP für die Workload konfigurieren](dns-dhcp-setup.md).
* Informieren Sie sich, wie mit dem Dienst die Sicherheit und Funktionalität auf Ihrer Plattform mit [Dienstupdates und -upgrades](vmware-components.md#updates-and-upgrades) sichergestellt wird.
* Sparen Sie bei der Sicherung Gesamtkosten, indem Sie mit einer [Sicherungssoftware eines Drittanbieters, z. B. Veeam](backup-workloads-veeam.md), eine Beispielarchitektur für die Sicherung erstellen.
* Erstellen Sie eine sichere Umgebung, indem Sie mit [KMS-Verschlüsselungssoftware eines Drittanbieters](vsan-encryption.md) die Verschlüsselung ruhender Daten durchführen.
* Erweitern Sie die Azure AD-Verwaltung (Azure Active Directory) auf VMware, indem Sie die [Azure AD-Identitätsquelle](azure-ad.md) konfigurieren.
