---
title: include file
description: include file
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 05/22/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 65c1011e6e005c190d1ae5d51fdd009f66a20956
ms.sourcegitcommit: 083aa7cc8fc958fc75365462aed542f1b5409623
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2019
ms.locfileid: "70919681"
---
|**SKU**   | **S2S/VNet-zu-VNet-<br>Tunnels** | **P2S<br> SSTP-Verbindungen** | **P2S<br>-IKEv2/OpenVPN-Verbindungen** | **Benchmark für<br>aggregierten Durchsatz** | **BGP** | **Zonenredundant** |
|---       | ---        | ---       | ---            | ---       | --- | --- |
|**Basic** | Maximal 10    | Maximal 128  | Nicht unterstützt  | 100 MBit/s  | Nicht unterstützt| Nein |
|**VpnGw1**| Maximal 30*   | Maximal 128  | Maximal 250       | 650 MBit/s  | Unterstützt | Nein |
|**VpnGw2**| Maximal 30*   | Maximal 128  | Maximal 500       | 1 GBit/s    | Unterstützt | Nein |
|**VpnGw3**| Maximal 30*   | Maximal 128  | Maximal 1000      | 1,25 GBit/s | Unterstützt | Nein |
|**VpnGw1AZ**| Maximal 30*   | Maximal 128  | Maximal 250       | 650 MBit/s  | Unterstützt | Ja |
|**VpnGw2AZ**| Maximal 30*   | Maximal 128  | Maximal 500       | 1 GBit/s    | Unterstützt | Ja |
|**VpnGw3AZ**| Maximal 30*   | Maximal 128  | Maximal 1000      | 1,25 GBit/s | Unterstützt | Ja |

(\*) Verwenden Sie [Virtual WAN](../articles/virtual-wan/virtual-wan-about.md), wenn Sie mehr als 30 S2S-VPN-Tunnel benötigen.

* Diese Verbindungsgrenzwerte sind voneinander getrennt. Sie können beispielsweise 128 SSTP-Verbindungen und 250 IKEv2-Verbindungen in der SKU „VpnGw1“ nutzen.

* Informationen zu den Preisen finden Sie auf der Seite [Preise](https://azure.microsoft.com/pricing/details/vpn-gateway) .

* Informationen zum SLA (Vereinbarung zum Servicelevel) finden Sie auf der [SLA](https://azure.microsoft.com/support/legal/sla/vpn-gateway/)-Seite.

* „VpnGw1“, „VpnGw2“, „VpnGw3“, „VpnGw1AZ“, „VpnGw2AZ“ und „VpnGw3AZ“ werden nur für VPN-Gateways unterstützt, die das Resource Manager-Bereitstellungsmodell verwenden.

* Die Basic-SKU wird als Legacy-SKU betrachtet. Für die Basic-SKU gelten bestimmte Featurebeschränkungen. Die Größe eines Gateways mit einer Basic-SKU kann nicht auf eine der neuen Gateway-SKUs festgelegt werden. Stattdessen müssen Sie zu einer neuen SKU wechseln und dazu Ihr VPN-Gateway löschen und neu erstellen. Vergewissern Sie sich vor der Verwendung der Basic-SKU, dass das von Ihnen benötigte Feature unterstützt wird.

* Der Benchmark für den aggregierten Durchsatz basiert auf Messungen von mehreren, durch ein Gateway aggregierten Tunneln. Der Benchmark für den aggregierten Durchsatz für ein VPN-Gateway besteht aus einer Kombination aus S2S und P2S. **Bei zahlreichen P2S-Verbindungen kann eine S2S-Verbindung aufgrund von Durchsatzeinschränkungen beeinträchtigt sein.** Aufgrund der Bedingungen für den Internetdatenverkehr und dem Verhalten Ihrer Anwendung kann der Benchmark für den aggregierten Durchsatz nicht garantiert werden.

* Damit unsere Kunden die relative Leistung von VpnGw-SKUs besser nachvollziehen können, haben wir zum Messen der Leistung die öffentlich verfügbaren Tools Iperf und CtsTraffic verwendet. Die folgende Tabelle enthält die Ergebnisse von Leistungstests mit verschiedenen Algorithmen. Wie Sie sehen, wird die beste Leistung erzielt, wenn sowohl für die IPSec-Verschlüsselung als auch für die Integrität der GCMAES256-Algorithmus verwendet wird. Bei Verwendung von AES256 für die IPSec-Verschlüsselung und SHA256 für die Integrität wurde eine durchschnittliche Leistung erzielt. Bei Verwendung von DES3 für die IPSec-Verschlüsselung und SHA256 für die Integrität wurde die geringste Leistung erzielt.

|**SKU**   | **Verwendete<br>Algorithmen** | **Ermittelter<br>Durchsatz** | **Ermittelte<br>Pakete pro Sekunde** |
|---       | ---                 | ---            | ---                    |
|**VpnGw1**| GCMAES256<br>AES256 und SHA256<br>DES3 und SHA256| 650 MBit/s<br>500 MBit/s<br>120 MBit/s   | 58.000<br>50.000<br>50.000|
|**VpnGw2**| GCMAES256<br>AES256 und SHA256<br>DES3 und SHA256| 1 GBit/s<br>500 MBit/s<br>120 MBit/s | 90.000<br>80.000<br>55.000|
|**VpnGw3**| GCMAES256<br>AES256 und SHA256<br>DES3 und SHA256| 1,25 GBit/s<br>550 MBit/s<br>120 MBit/s | 105.000<br>90.000<br>60.000|
|**VpnGw1AZ**| GCMAES256<br>AES256 und SHA256<br>DES3 und SHA256| 650 MBit/s<br>500 MBit/s<br>120 MBit/s   | 58.000<br>50.000<br>50.000|
|**VpnGw2AZ**| GCMAES256<br>AES256 und SHA256<br>DES3 und SHA256| 1 GBit/s<br>500 MBit/s<br>120 MBit/s | 90.000<br>80.000<br>55.000|
|**VpnGw3AZ**| GCMAES256<br>AES256 und SHA256<br>DES3 und SHA256| 1,25 GBit/s<br>550 MBit/s<br>120 MBit/s | 105.000<br>90.000<br>60.000|
