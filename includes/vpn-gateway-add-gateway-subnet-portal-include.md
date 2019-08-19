---
title: include file
description: include file
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 08/02/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 52ab0413dffeee59cb9d34c6276a0c806a4d0322
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/05/2019
ms.locfileid: "68780095"
---
Das Gatewaysubnetz enthält die reservierten IP-Adressen, die von den Diensten des virtuellen Netzwerkgateways verwendet werden. Erstellen Sie ein Gatewaysubnetz.

1. Navigieren Sie im Portal zum virtuellen Netzwerk, für das Sie ein Gateway des virtuellen Netzwerks erstellen möchten.
2. Klicken Sie auf der Seite „Virtuelles Netzwerk“ auf **Subnetze**, um die Seite **VNet1 – Subnetze** zu erweitern.
3. Klicken Sie oben auf der Seite auf **+Gatewaysubnetz**, um die Seite **Subnetz hinzufügen** zu öffnen.

   ![Hinzufügen des Gatewaysubnetzes](./media/vpn-gateway-add-gateway-subnet-portal-include/gateway-subnet.png "Hinzufügen des Gatewaysubnetzes")
4. Als **Name** für Ihr Subnetz wird automatisch der erforderliche Wert „GatewaySubnet“ eingefügt. Passen Sie den automatisch ausgefüllten Wert für **Adressbereich (CIDR-Block)** wie folgt an:

   **Adressbereich (CIDR-Block)** : 10.1.255.0/27

   ![Hinzufügen des Gatewaysubnetzes](./media/vpn-gateway-add-gateway-subnet-portal-include/add-gateway-subnet1.png "Hinzufügen des Gatewaysubnetzes")
5. Behalten Sie für die restlichen Einstellungen die Standardeinstellung **Keine** oder **0 ausgewählt** bei. Klicken Sie anschließend auf **OK**, um das Gatewaysubnetz zu erstellen.