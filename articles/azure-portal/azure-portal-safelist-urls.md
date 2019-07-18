---
title: Hinzufügen von Azure-Portal-URLs | Microsoft-Dokumentation
description: Hinzufügen dieser URL zu einer Proxyserverumgehung für die Kommunikation mit dem Azure-Portal und dessen Dienste
services: azure-portal
keywords: ''
author: kfollis
ms.author: kfollis
ms.date: 06/13/2019
ms.topic: conceptual
ms.service: azure-portal
manager: mtillman
ms.openlocfilehash: 87ec600a2f6c4a560ec7cbb064b561fa76e2b615
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2019
ms.locfileid: "67304742"
---
# <a name="safelist-the-azure-portal-urls-on-your-firewall-or-proxy-server"></a>Hinzufügen der Azure-Portal-URLs zu einer Liste sicherer Adressen für Ihre Firewall oder Ihren Proxyserver

Damit eine gute Leistung und die Konnektivität Ihres lokalen Netzwerks (Local area network, LAN) oder Ihres Fernnetzes (Wide area Network, WAN) gewährleistet ist, konfigurieren Sie Ihre lokalen Sicherheitsgeräte so, dass sie Sicherheitseinschränkungen für die Azure-Portal-URLs umgehen. Netzwerkadministratoren stellen häufig Proxyserver, Firewalls oder andere Geräte bereit, um den Internetzugriff durch die Benutzer zu sichern und zu steuern. Allerdings können Regeln, die die Benutzer schützen sollen, gerechtfertigten und geschäftsbezogenen Internetdatenverkehr blockieren oder verlangsamen. Dies betrifft z. B. auch Ihre Kommunikation mit Azure. Zur Optimierung der Konnektivität zwischen Ihrem Netzwerk und dem Azure-Portal und dessen Diensten wird empfohlen, Azure-Portal-URLS zu Ihrer Liste mit sicheren Adressen hinzuzufügen.

## <a name="azure-portal-urls-for-proxy-bypass"></a>Azure-Portal-URLs für Proxyumgehung

Fügen Sie die folgende Liste an URLs zu Ihrem Proxyserver oder Ihrer Firewall hinzu, um Datenverkehr für diese Endpunkte zuzulassen und Einschränkungen zu umgehen:

* *.aadcdn.microsoftonline-p.com
* *.aimon.applicationinsights.io
* *.azure.com
* *.azuredatalakestore.net
* *.azureedge.net
* *.exp.azure.com
* *.ext.azure.com
* *.gfx.ms
* *.account.microsoft.com
* *.hosting.portal.azure.net
* *.marketplaceapi.microsoft.com
* *.microsoftonline.com
* *.msauth.net
* *.msftauth.net
* *.portal.azure.com
* *.portalext.visualstudio.com
* *.sts.microsoft.com
* *.vortex.data.microsoft.com
* *.vscommerce.visualstudio.com
* *.vssps.visualstudio.com
* *.wpc.azureedge.net

> [!NOTE]
> Der Datenverkehr für diese Endpunkte verwendet TCP-Standardports für HTTP (80) und HTTPS (443).
>
>
## <a name="next-steps"></a>Nächste Schritte

* Sie möchten IP-Adressen auf eine Liste mit sicheren Adressen setzen? Laden Sie die Liste der [IP-Bereiche der Microsoft Azure-Rechenzentren](https://www.microsoft.com/download/details.aspx?id=41653) herunter.
* Andere Microsoft-Dienste verwenden zusätzliche URLs und IP-Adressen für die Konnektivität. Weitere Informationen zum Optimieren der Netzwerkkonnektivität für Microsoft 365-Dienste finden Sie unter [Set up your network for Office 365 (Einrichten Ihres Netzwerks für Office 365)](/office365/enterprise/set-up-network-for-office-365).
