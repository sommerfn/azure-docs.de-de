---
title: Hinzufügen von Azure-Portal-URLs | Microsoft-Dokumentation
description: Hinzufügen dieser URL zu einer Proxyserverumgehung für die Kommunikation mit dem Azure-Portal und dessen Dienste
services: azure-portal
keywords: ''
author: kfollis
ms.author: kfollis
ms.date: 09/13/2019
ms.topic: conceptual
ms.service: azure-portal
manager: mtillman
ms.openlocfilehash: 78c752423d20d183c561d5bcf0bb95246b84ab49
ms.sourcegitcommit: 8ef0a2ddaece5e7b2ac678a73b605b2073b76e88
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/17/2019
ms.locfileid: "71076672"
---
# <a name="safelist-the-azure-portal-urls-on-your-firewall-or-proxy-server"></a>Hinzufügen der Azure-Portal-URLs zu einer Liste sicherer Adressen für Ihre Firewall oder Ihren Proxyserver

Damit eine gute Leistung und die Konnektivität Ihres lokalen Netzwerks (Local area network, LAN) oder Ihres Fernnetzes (Wide area Network, WAN) gewährleistet ist, konfigurieren Sie Ihre lokalen Sicherheitsgeräte so, dass sie Sicherheitseinschränkungen für die Azure-Portal-URLs umgehen. Netzwerkadministratoren stellen häufig Proxyserver, Firewalls oder andere Geräte bereit, um den Internetzugriff durch die Benutzer zu sichern und zu steuern. Allerdings können Regeln, die die Benutzer schützen sollen, gerechtfertigten und geschäftsbezogenen Internetdatenverkehr blockieren oder verlangsamen. Dies betrifft z. B. auch Ihre Kommunikation mit Azure. Zur Optimierung der Konnektivität zwischen Ihrem Netzwerk und dem Azure-Portal und dessen Diensten wird empfohlen, Azure-Portal-URLS zu Ihrer Liste mit sicheren Adressen hinzuzufügen.

## <a name="azure-portal-urls-for-proxy-bypass"></a>Azure-Portal-URLs für Proxyumgehung

Die für das Azure-Portal in die Liste mit sicheren Adressen aufzunehmenden URL-Endpunkte sind für die Azure-Cloud spezifisch, in der Ihre Organisation bereitgestellt wird. Wählen Sie Ihre Cloud aus, und fügen Sie dann die Liste an URLs zu Ihrem Proxyserver oder Ihrer Firewall hinzu, um Datenverkehr für diese Endpunkte zuzulassen und Einschränkungen zu umgehen.

#### <a name="public-cloudtabpublic-cloud"></a>[Öffentliche Cloud](#tab/public-cloud)
```
*.aadcdn.microsoftonline-p.com
*.aka.ms
*.applicationinsights.io
*.azure.com
*.azure.net
*.azureafd.net
*.azure-api.net
*.azuredatalakestore.net
*.azureedge.net
*.loganalytics.io
*.microsoft.com
*.microsoftonline.com
*.microsoftonline-p.com
*.msauth.net
*.msftauth.net
*.trafficmanager.net
*.visualstudio.com
*.windows.net
*.windows-int.net
```

#### <a name="us-government-cloudtabus-government-cloud"></a>[US- Government Cloud](#tab/us-government-cloud)
```
*.azure.us
*.loganalytics.us
*.microsoft.us
*.microsoftonline.us
*.msauth.net
*.usgovcloudapi.net
*.usgovtrafficmanager.net
*.windowsazure.us
```

#### <a name="china-government-cloudtabchina-government-cloud"></a>[China-Government Cloud](#tab/china-government-cloud)
```
*.azure.cn
*.microsoft.cn
*.microsoftonline.cn
*.chinacloudapi.cn
*.trafficmanager.cn
*.chinacloudsites.cn
*.windowsazure.cn
```
---

> [!NOTE]
> Der Datenverkehr für diese Endpunkte verwendet TCP-Standardports für HTTP (80) und HTTPS (443).
>
>
## <a name="next-steps"></a>Nächste Schritte

Sie möchten IP-Adressen auf eine Liste mit sicheren Adressen setzen? Laden Sie die Liste der IP-Bereiche der Microsoft Azure-Rechenzentren für Ihre Cloud herunter:

* [Weltweit](https://www.microsoft.com/download/details.aspx?id=56519)
* [US- Government](http://www.microsoft.com/download/details.aspx?id=57063)
* [Deutschland](http://www.microsoft.com/download/details.aspx?id=57064)
* [China](http://www.microsoft.com/download/details.aspx?id=57062)

Andere Microsoft-Dienste verwenden zusätzliche URLs und IP-Adressen für die Konnektivität. Weitere Informationen zum Optimieren der Netzwerkkonnektivität für Microsoft 365-Dienste finden Sie unter [Set up your network for Office 365 (Einrichten Ihres Netzwerks für Office 365)](/office365/enterprise/set-up-network-for-office-365).
