---
title: Azure VMware-Lösung von CloudSimple – VPN-Gateways
description: Erfahren Sie mehr über die CloudSimple-Konzepte für Site-to-Site- und Point-to-Site-VPNs.
author: sharaths-cs
ms.author: dikamath
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 662fa342b3a18f726b418c496ff3fda937445301
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2019
ms.locfileid: "72244768"
---
# <a name="vpn-gateways-overview"></a>Übersicht über VPN-Gateways

Ein VPN-Gateway wird verwendet, um verschlüsselten Datenverkehr zwischen einem CloudSimple-Regionsnetzwerk und einem lokalen Standort oder einem Computer über das öffentliche Internet zu senden.  Jede Region kann über ein VPN-Gateway verfügen, das mehrere Verbindungen unterstützen kann. Wenn Sie mehrere Verbindungen mit dem gleichen VPN-Gateway herstellen, wird die für das Gateway zur Verfügung stehende Bandbreite auf alle VPN-Tunnel aufgeteilt.

CloudSimple stellt zwei Arten von VPN-Gateways bereit:

* Site-to-Site-VPN-Gateway
* Point-to-Site-VPN Gateway

## <a name="site-to-site-vpn-gateway"></a>Site-to-Site-VPN-Gateway

Ein Site-to-Site-VPN-Gateway wird verwendet, um verschlüsselten Datenverkehr zwischen einem CloudSimple-Regionsnetzwerk und einem lokalen Rechenzentrum zu senden. Verwenden Sie diese Verbindung, um die Subnetze oder den CIDR-Bereich für Netzwerkdatenverkehr zwischen Ihrem lokalen Netzwerk und dem CloudSimple-Regionsnetzwerk zu definieren.

Das VPN-Gateway ermöglicht es Ihnen, Dienste vom lokalen Standort in Ihrer privaten Cloud und Dienste in Ihrer privaten Cloud aus dem lokalen Netzwerk zu nutzen.  CloudSimple stellt einen richtlinienbasierten VPN-Server bereit, über den Sie die Verbindung aus Ihrem lokalen Netzwerk herstellen können.

Anwendungsfälle für ein Site-to-Site-VPN:

* Zugriff auf das vCenter Ihrer privaten Cloud von jeder Arbeitsstation in Ihrem lokalen Netzwerk.
* Verwenden Ihres lokalen Active Directory Domain Services als vCenter-Identitätsquelle.
* Einfache Übertragung von VM-Vorlagen, ISO-Dateien und anderen Dateien aus Ihren lokalen Ressourcen in das vCenter Ihrer privaten Cloud.
* Zugriff auf Workloads, die in Ihrer privaten Cloud ausgeführt werden, aus Ihrem lokalen Netzwerk.

![Topologie einer Site-to-Site-VPN-Verbindung](media/cloudsimple-site-to-site-vpn-connection.png)

### <a name="cryptographic-parameters"></a>Kryptografische Parameter

Für eine Site-to-Site-VPN-Verbindung werden die folgenden kryptografischen Standardparameter verwendet, um eine sichere Verbindung herzustellen.  Wenn Sie eine Verbindung von Ihrem lokalen VPN-Gerät aus herstellen, können Sie jeden der folgenden Parameter verwenden, die von Ihrem lokalen VPN-Gateway unterstützt werden.

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


> [!IMPORTANT]
> Legen Sie TCP MSS-Clamping für das VPN-Gerät auf 1200 fest. Wenn Ihre VPN-Geräte MSS-Clamping nicht unterstützen, können Sie stattdessen auch den MTU-Wert der Tunnelschnittstelle auf 1240 Bytes festlegen.

## <a name="point-to-site-vpn-gateway"></a>Point-to-Site-VPN Gateway

Ein Point-to-Site-VPN-Gateway wird verwendet, um verschlüsselten Datenverkehr zwischen einem CloudSimple-Regionsnetzwerk und einem Clientcomputer zu senden.  Ein Point-to-Site-VPN ist die einfachste Möglichkeit, auf das Netzwerk Ihrer privaten Cloud zuzugreifen, einschließlich vCenter und Workload-VMs Ihrer privaten Cloud.  Nutzen Sie die Point-to-Site-VPN-Konnektivität, wenn Sie eine Remoteverbindung mit der privaten Cloud herstellen.

## <a name="next-steps"></a>Nächste Schritte

* [Einrichten eines VPN-Gateways](vpn-gateway.md)
