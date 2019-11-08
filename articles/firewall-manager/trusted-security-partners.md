---
title: Was sind vertrauenswürdige Sicherheitspartner in Azure Firewall Manager (Vorschau)?
description: Erfahren Sie mehr über vertrauenswürdige Sicherheitspartner in Azure Firewall Manager.
author: vhorne
ms.service: firewall-manager
services: firewall-manager
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: victorh
ms.openlocfilehash: cb8a2fdd14cfa7d361e3d78a64f3aaf60ea7676d
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510051"
---
# <a name="what-are-trusted-security-partners-preview"></a>Was sind vertrauenswürdige Sicherheitspartner (Vorschau)?

> [!IMPORTANT]
> Diese öffentliche Vorschauversion wird ohne Servicelevelvereinbarung bereitgestellt und sollte nicht für Produktionsworkloads verwendet werden. Unter Umständen werden bestimmte Features nicht unterstützt, verfügen über eingeschränkte Funktionen und sind nicht an allen Azure-Standorten verfügbar. Weitere Informationen finden Sie unter [Ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Mit dem Feature *Vertrauenswürdiger Sicherheitspartner (Vorschau)* in Azure Firewall Manager können Sie Ihre vertrauten erstklassigen SECaaS (Security-as-a-Service)-Angebote von Drittanbietern verwenden, um den Internetzugriff für Ihre Benutzer zu schützen.

Mit einigen schnellen Konfigurationsschritten können Sie einen Hub mit einem unterstützten Sicherheitspartner schützen und den Internetdatenverkehr von Ihren virtuellen Netzwerken (VNETs) oder Filialstandorten innerhalb einer Region weiterleiten und filtern. Dies geschieht mithilfe der automatisierten Routenverwaltung, ohne dass benutzerdefinierte Routen (User Defined Routes, UDRs) eingerichtet und verwaltet werden müssen.

Sie können geschützte, mithilfe eines Sicherheitspartners Ihrer Wahl konfigurierte Hubs in mehreren Azure-Regionen bereitstellen, um Konnektivität und Sicherheit für Benutzer in diesen Regionen rund um die Welt zu gewährleisten. Dank der Möglichkeit, das Angebot des Sicherheitspartners für den Internet-/SaaS-Anwendungsdatenverkehr und die Azure Firewall für privaten Datenverkehr in den geschützten Hubs zu nutzen, können Sie jetzt Ihre Sicherheitsperipherie in Azure aufbauen, die sich in der Nähe ihrer weltweit verteilten Benutzer und Anwendungen befindet.

Die für diese Vorschau unterstützten Sicherheitspartner sind **ZScaler** und **iboss**. Die unterstützten Regionen lauten „WestCentralUS“, „NorthCentralUS“, „WestUS“, „WestUS2“ und „EastUS“.

![Vertrauenswürdige Sicherheitspartner](media/trusted-security-partners/trusted-security-partners.png)

## <a name="key-scenarios"></a>Wichtige Szenarien

Mithilfe der Sicherheitspartner können Sie den Internetdatenverkehr in folgenden Szenarien filtern:

- Virtuelles Netzwerk (VNET) zu Internet

   Nutzen Sie erweiterten, benutzerfähigen Internetschutz für Ihre in Azure ausgeführten Cloudworkloads.

- Filiale zu Internet

   Nutzen Sie die Azure-Konnektivität und die globale Verteilung, um auf einfache Weise eine NSaaS-Drittanbieterfilterung für Szenarien vom Typ „Filiale-zu-Internet“ hinzuzufügen. Sie können Ihr globales Übertragungsnetzwerk und die Sicherheitsperipherie mithilfe von Azure Virtual WAN erstellen.

Folgende Szenarios werden unterstützt:
-   „VNET zu Internet“ über das Partnerangebot eines Drittanbieters
-   „Filiale zu Internet“ über das Partnerangebot eines Drittanbieters
-   „Filiale zu Internet“ über das Partnerangebot eines Drittanbieters – der übrige private Datenverkehr (Spoke-to-Spoke, Spoke-to-Branches, Branch-to-Spokes) über Azure Firewall

Das folgende Szenario wird nicht unterstützt:

- „VNET zu Internet“ über ein Partnerangebot kann nicht mit Azure Firewall für privaten Datenverkehr kombiniert werden. Siehe die folgenden Einschränkungen.

## <a name="current-limitations"></a>Aktuelle Einschränkungen

- Für „VNET zu Internet“ können Azure Firewall für privaten Datenverkehr und ein Partnerangebot für den Internetdatenverkehr nicht kombiniert werden. Sie können Internetdatenverkehr entweder an die Azure Firewall oder an die Sicherheitslösung eines Drittanbieters im geschützten virtuellen Hub senden, aber nicht beides. 
- Sie können maximal einen Sicherheitspartner pro virtuellem Hub bereitstellen. Wenn Sie den Anbieter ändern möchten, müssen Sie den vorhandenen Partner entfernen und einen neuen hinzufügen.

## <a name="best-practices-for-internet-traffic-filtering-in-secured-virtual-hubs"></a>Best Practices für das Filtern von Internetdatenverkehr in geschützten virtuellen Hubs

Der Internetdatenverkehr umfasst in der Regel Webdatenverkehr. Er beinhaltet aber auch Datenverkehr, der an SaaS-Anwendungen wie Office 365 (O365) und öffentliche Azure-PaaS-Dienste wie Azure Storage, Azure SQL usw. gerichtet ist. Im Folgenden finden Sie empfohlene Best Practices zur Behandlung des Datenverkehrs für diese Dienste:

### <a name="handling-azure-paas-traffic"></a>Behandeln von Azure-PaaS-Datenverkehr
 
- Schützen Sie sich mit der Azure Firewall, wenn der Datenverkehr meist aus Azure-PaaS-Datenverkehr besteht und der Ressourcenzugriff für Ihre Anwendungen mit IP-Adressen, FQDNs, Diensttags oder FQDN-Tags gefiltert werden kann.

- Verwenden Sie die Partnerlösung eines Drittanbieters in Ihren Hubs, wenn der Datenverkehr aus SaaS-Anwendungszugriffen besteht oder Sie (beispielsweise für Ihre VDI [Virtual Desktop Infrastructure]-Workloads) eine benutzerfähige Filterung oder erweiterte Filterfunktionen für das Internet benötigen.

![Alle Szenarien für Azure Firewall Manager](media/trusted-security-partners/all-scenarios.png)

## <a name="handling-office-365-o365-traffic"></a>Behandlung von Office 365 (O365)-Datenverkehr

In global verteilten Filialszenarien sollten Sie Office 365-Datenverkehr direkt am Filialstandort umleiten, bevor Sie den verbleibenden Internetdatenverkehr an den geschützten Azure-Hub senden.

Bei Office 365 sind Netzwerklatenz und -leistung für eine positive Benutzererfahrung von entscheidender Bedeutung. Um diese optimale Leistung und Benutzerfreundlichkeit zu erzielen, müssen Kunden Office 365 direkt und lokal implementieren, bevor sie es in Betracht ziehen, den übrigen Internetdatenverkehr über Azure weiterzuleiten.

Gemäß den [Office 365-Prinzipien für Netzwerkkonnektivität](https://docs.microsoft.com/office365/enterprise/office-365-network-connectivity-principles) müssen wichtige Office 365-Verbindungen lokal vom Filialstandort oder Mobilgerät des Benutzers direkt über das Internet an den nächstgelegenen Netzwerk-POP (Point-of-Presence) von Microsoft weitergeleitet werden.

Darüber hinaus werden Office 365-Verbindungen aus Gründen des Datenschutzes stark verschlüsselt und nutzen aus Leistungsgründen effiziente, proprietäre Protokolle. Daher ist es weder praktisch noch wirkungsvoll, diese Verbindungen mit herkömmlichen Sicherheitslösungen auf Netzwerkebene zu schützen. Aus diesen Gründen wird dringend empfohlen, dass Kunden Office 365-Datenverkehr direkt von Filialstandorten senden, bevor der übrige Datenverkehr über Azure übertragen wird. Microsoft verfügt über Partnerschaften mit mehreren Anbietern von SD-WAN-Lösungen, die in Azure und Office 365 integriert sind und es Kunden einfach machen, das direkte und lokale Internetbreakout in Office 365 zu aktivieren. Ausführliche Informationen finden Sie unter [Wie richte ich meine Office 365-Richtlinien über Virtual WAN ein?](https://docs.microsoft.com/azure/virtual-wan/virtual-wan-office365-overview#how-do-i-set-my-o365-policies-via-virtual-wan).


## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen des Angebots eines vertrauenswürdigen Sicherheitspartners in einem geschützten Hub unter Verwendung von Azure Firewall Manager](deploy-trusted-security-partner.md)
