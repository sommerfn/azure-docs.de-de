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
ms.openlocfilehash: 8b585a47d3950d232eb3e8047c12ee8949030c95
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "68780219"
---
Wenn Sie mit dem Azure-Portal ein VNET im Resource Manager-Bereitstellungsmodell erstellen, führen Sie die Schritte unten aus. Verwenden Sie die **Beispielwerte**, wenn Sie diese Schritte für ein Tutorial ausführen. Wenn Sie diese Schritte nicht als Tutorial ausführen, achten Sie darauf, die Werte durch eigene Werte zu ersetzen. Weitere Informationen zur Arbeit mit virtuellen Netzwerken finden Sie unter [Virtuelle Netzwerke im Überblick](../articles/virtual-network/virtual-networks-overview.md).

>[!NOTE]
>Damit für dieses VNET eine Verbindung mit einem lokalen Speicherort hergestellt werden kann, müssen Sie in Zusammenarbeit mit dem Administrator des lokalen Netzwerks einen IP-Adressbereich festlegen, den Sie speziell für dieses virtuelle Netzwerk verwenden können. Falls ein Adressbereich auf beiden Seiten der VPN-Verbindung und somit doppelt vorhanden ist, wird Datenverkehr unter Umständen nicht wie erwartet weitergeleitet. Wenn Sie für dieses VNet eine Verbindung mit einem anderen VNet herstellen möchten, darf sich der Adressraum außerdem nicht mit dem anderen VNet überlappen. Achten Sie darauf, dass Sie Ihre Netzwerkkonfiguration entsprechend planen.
>

1. Navigieren Sie in einem Browser zum [Azure-Portal](https://portal.azure.com) , und melden Sie sich mit Ihrem Azure-Konto an.
2. Klicken Sie auf **Ressource erstellen**. Geben Sie im Feld **Marketplace durchsuchen** die Zeichenfolge „Virtuelles Netzwerk“ ein. Klicken Sie in der zurückgegebenen Liste auf **Virtuelles Netzwerk**, um die Seite **Virtual Network** zu öffnen.
3. Klicken Sie auf **Create**. Die Seite **Virtuelles Netzwerk erstellen** wird geöffnet.
4. Konfigurieren Sie auf der Seite **Virtuelles Netzwerk erstellen** die VNET-Einstellungen. Beim Ausfüllen der Felder ändert sich das rote Ausrufezeichen in ein grünes Häkchen, sofern die in das Feld eingegebenen Zeichen zulässig sind. Verwenden Sie die folgenden Werte:

   - **Name**: VNet1
   - **Adressraum:** 10.1.0.0/16
   - **Abonnement**: Vergewissern Sie sich, dass das aufgelistete Abonnement das Abonnement ist, das Sie verwenden möchten. Das Abonnement kann über die Dropdownliste geändert werden.
   - **Ressourcengruppe**: TestRG1 (Klicken Sie auf **Neu erstellen**, um eine neue Gruppe zu erstellen.)
   - **Standort**: East US
   - **Subnetz**: Front-End
   - **Adressbereich**: 10.1.0.0/24

   ![Seite „Virtuelles Netzwerk erstellen“](./media/vpn-gateway-create-virtual-network-portal-include/create-virtual-network1.png)
5. Behalten Sie für „DDoS“ die Einstellung „Basic“, für „Dienstendpunkte“ die Einstellung „Deaktiviert“ und für „Firewall“ die Einstellung „Deaktiviert“ bei.
6. Klicken Sie auf **Erstellen**, um das VNet zu erstellen.