---
title: include file
description: include file
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 11/30/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 42e983ead6f7562c6a31cf9ef4ad2d97d0ff9707
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2019
ms.locfileid: "67673353"
---
1. Wählen Sie im [Azure-Portal](https://portal.azure.com) das virtuelle Resource Manager-Netzwerk aus, für das Sie ein virtuelles Netzwerkgateway erstellen möchten.

2. Klicken Sie auf der Seite Ihres virtuellen Netzwerks im Abschnitt **Einstellungen** auf **Subnetze**, um die Seite **Subnetze** zu erweitern.

3. Klicken Sie auf der Seite **Subnetze** auf **Gatewaysubnetz**, um die Seite **Subnetz hinzufügen** zu öffnen.

   ![Hinzufügen des Gatewaysubnetzes](./media/vpn-gateway-add-gwsubnet-rm-portal-include/addgwsub.png "Hinzufügen des Gatewaysubnetzes")

4. Als **Name** für Ihr Subnetz wird automatisch der Wert *GatewaySubnet* eingefügt. Dieser Wert ist erforderlich, damit Azure das Subnetz als Gatewaysubnetz erkennt. Passen Sie die automatisch ausgefüllten Werte für **Adressbereich** gemäß Ihren Konfigurationsanforderungen an, und wählen Sie dann **OK** aus, um das Subnetz zu erstellen.

   ![Hinzufügen des Subnetzes](./media/vpn-gateway-add-gwsubnet-rm-portal-include/addsubnetgw.png "Hinzufügen des Subnetzes")
