---
title: VPN-Gateways in VMware-Lösung von CloudSimple – Azure
description: Erfahren Sie mehr über die CloudSimple-Konzepte für Site-to-Site-VPN (Standort-zu-Standort-VPN) und Punkt-zu-Standort-VPN.
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 2eae81f357904bd5034d7409ef42b681d1085930
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2019
ms.locfileid: "67695217"
---
# <a name="vpn-gateways-overview"></a>Übersicht über VPN-Gateways

Ein VPN-Gateway wird verwendet, um verschlüsselten Datenverkehr zwischen einem CloudSimple-Regionsnetzwerk und einem lokalen Standort oder einem Computer über das öffentliche Internet zu senden.  Jede Region kann nur ein einziges VPN-Gateway haben. Sie können jedoch mehrere Verbindungen mit dem gleichen VPN-Gateway herstellen. Wenn Sie mehrere Verbindungen mit dem gleichen VPN-Gateway herstellen, wird die für das Gateway zur Verfügung stehende Bandbreite auf alle VPN-Tunnel aufgeteilt.

CloudSimple stellt zwei Arten von VPN-Gateways bereit:

* Standort-zu-Standort-VPN-Gateway
* Punkt-zu-Standort-VPN-Gateway

## <a name="site-to-site-vpn-gateway"></a>Standort-zu-Standort-VPN-Gateway

Ein Standort-zu-Standort-VPN-Gateway (Site-to-Site-VPN-Gateway) wird verwendet, um verschlüsselten Datenverkehr zwischen einem CloudSimple-Regionsnetzwerk und einem lokalen Rechenzentrum zu senden. Verwenden Sie diese Verbindung, um die Subnetze oder den CIDR-Bereich für die Kommunikation zwischen Ihrem lokalen Netzwerk und dem CloudSimple-Regionsnetzwerk zu definieren.

Das VPN-Gateway ermöglicht es Ihnen, Dienste vom lokalen Standort in Ihrer privaten Cloud und Dienste in Ihrer privaten Cloud aus dem lokalen Netzwerk zu nutzen.  CloudSimple stellt einen richtlinienbasierten VPN-Server bereit, über den Sie Verbindungen aus Ihrem lokalen Netzwerk herstellen können.

Zu den Anwendungsfällen von Site-to-Site-VPN gehören:

* Zugriff auf das vCenter-Tool Ihrer privaten Cloud von jeder Arbeitsstation in Ihrem lokalen Netzwerk.
* Verwenden Ihres lokalen Active Directory Domain Services als vCenter-Identitätsquelle.
* Einfache Übertragung von VM-Vorlagen, ISO-Dateien und andere Dateien aus Ihren lokalen Ressourcen in das vCenter Ihrer privaten Cloud.
* Zugriff auf Workloads, die in Ihrer privaten Cloud ausgeführt werden, aus Ihrem lokalen Netzwerk.

![Topologie einer Site-to-Site-VPN-Verbindung](media/cloudsimple-site-to-site-vpn-connection.png)

> [!IMPORTANT]
> Darüber hinaus müssen Sie TCP MSS mit höchstens 1078 Byte verknüpfen. Wenn Ihre VPN-Geräte MSS-Clamping nicht unterstützen, können Sie stattdessen auch den MTU-Wert der Tunnelschnittstelle auf 1118 Byte festlegen. 

### <a name="cryptographic-parameters"></a>Kryptografische Parameter

Für eine Standort-zu-Standort-VPN-Verbindung werden die folgenden kryptografischen Standardparameter verwendet, um eine sichere Verbindung herzustellen.  Wenn Sie eine Verbindung von einem lokalen VPN-Gerät herstellen, verwenden Sie irgendeinen der folgenden Parameter, der von Ihrem lokalen VPN-Gateway unterstützt wird.

#### <a name="phase-1-proposals"></a>Vorschläge in Phase 1

| Parameter | Vorschlag 1 | Vorschlag 2 | Vorschlag 3 |
|-----------|------------|------------|------------|
| IKE-Version | IKEv1 | IKEv1 | IKEv1 |
| Verschlüsselung | AES 128 | AES 256 | AES 256 |
| Hashalgorithmus| SHA 256 | SHA 256 | SHA 1 |
| Diffie-Hellman-Gruppe (DH-Gruppe) | 2 | 2 | 2 |
| Lebensdauer | 28.800 Sekunden | 28.800 Sekunden | 28.800 Sekunden |
| Datengröße | 4 GB | 4 GB | 4 GB |


#### <a name="phase-2-proposals"></a>Vorschläge in Phase 2 

| Parameter | Vorschlag 1 | Vorschlag 2 | Vorschlag 3 |
|-----------|------------|------------|------------|
| Verschlüsselung | AES 128 | AES 256 | AES 256 |
| Hashalgorithmus| SHA 256 | SHA 256 | SHA 1 |
| Perfect Forward Secrecy-Gruppe (PFS-Gruppe) | Keine | Keine | Keine |
| Lebensdauer | 1\.800 Sekunden | 1\.800 Sekunden | 1\.800 Sekunden |
| Datengröße | 4 GB | 4 GB | 4 GB |

## <a name="point-to-site-vpn-gateway"></a>Point-to-Site-VPN-Gateway

Ein Punkt-zu-Standort-VPN-Gateway (Point-to-Site-VPN-Gateway) wird verwendet, um verschlüsselten Datenverkehr zwischen einem CloudSimple-Regionsnetzwerk und einem Clientcomputer zu senden.  Punkt-zu-Standort-VPN ist die einfachste Möglichkeit, um auf das Netzwerk Ihrer privaten Cloud zuzugreifen, einschließlich vCenter und Workload-VMs Ihrer privaten Cloud.  Verwenden Sie eine Punkt-zu-Standort-VPN-Verbindung, wenn Sie eine Remoteverbindung mit der privaten Cloud herstellen.

## <a name="next-steps"></a>Nächste Schritte

* [Einrichten eines VPN-Gateways](https://docs.azure.cloudsimple.com/vpn-gateway/)