---
title: Verwenden eines lokalen Datengateways für Azure Virtual Network-Datenquellen | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie einen Server für die Verwendung eines Gateways für Datenquellen in einem VNET konfigurieren.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: d1993996c0f982624d6976b50871fa8c4592e2f6
ms.sourcegitcommit: 8b44498b922f7d7d34e4de7189b3ad5a9ba1488b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2019
ms.locfileid: "72301221"
---
# <a name="use-gateway-for-data-sources-on-an-azure-virtual-network-vnet"></a>Verwenden eines Gateways für Datenquellen in einer Azure Virtual Network-Instanz (VNET)

In diesem Artikel wird die Servereigenschaft **AlwaysUseGateway** beschrieben, die verwendet werden soll, wenn sich Datenquellen in einer [Azure Virtual Network-Instanz (VNET)](../virtual-network/virtual-networks-overview.md) befinden.

## <a name="server-access-to-vnet-data-sources"></a>Serverzugriff auf VNET-Datenquellen

Beim Zugriff auf Ihre Datenquellen über ein VNET muss Ihr Azure Analysis Services-Server mit diesen Datenquellen in Ihrer eigenen Umgebung eine Verbindung herstellen, als ob sie lokal wären. Sie können die Servereigenschaft **AlwaysUseGateway** so konfigurieren, dass der Server angewiesen wird, auf alle Datenquellen über ein [lokales Gateway](analysis-services-gateway.md) zuzugreifen. 

Datenquellen einer verwalteten Azure SQL-Datenbank-Instanz werden im Azure-VNET mit einer privaten IP-Adresse ausgeführt. Wenn für die-Instanz der öffentliche Endpunkt aktiviert ist, ist kein Gateway erforderlich. Wenn der öffentliche Endpunkt nicht aktiviert ist, ist ein lokales Datengateway erforderlich, und die AlwaysUseGateway-Eigenschaft muss auf TRUE festgelegt werden.

> [!NOTE]
> Diese Eigenschaft ist nur wirksam, wenn ein [lokales Datengateway](analysis-services-gateway.md) installiert und konfiguriert ist. Das Gateway kann sich im VNET befinden.

## <a name="configure-alwaysusegateway-property"></a>Konfigurieren der AlwaysUseGateway-Eigenschaft

1. Klicken Sie unter „SSMS“ > „Server“ > **Eigenschaften** > **Allgemein** auf **Show Advanced (All) Properties** (Erweiterte (alle Eigenschaften) anzeigen).
2. Legen Sie **ASPaaS\AlwaysUseGateway** auf **true** fest.

    ![AlwaysUseGateway-Eigenschaft](media/analysis-services-vnet-gateway/aas-ssms-always-property.png)


## <a name="see-also"></a>Weitere Informationen
[Herstellen einer Verbindung mit lokalen Datenquellen mit dem lokalen Azure-Datengateway](analysis-services-gateway.md)   
[Installieren und Konfigurieren eines lokalen Datengateways](analysis-services-gateway-install.md)   
[Was ist Azure Virtual Network?](../virtual-network/virtual-networks-overview.md)   

