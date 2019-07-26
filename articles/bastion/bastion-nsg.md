---
title: Arbeiten mit virtuellen Computern und Netzwerksicherheitsgruppen in Azure Bastion | Microsoft-Dokumentation
description: Dieser Artikel beschreibt die Integration des NSG-Zugriffs in Azure Bastion.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: cherylmc
ms.openlocfilehash: e7210b2b1be072f5326070d768d5fe12c386ee0b
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2019
ms.locfileid: "67191269"
---
# <a name="working-with-nsg-access-and-azure-bastion-preview"></a>Arbeiten mit NSG-Zugriff und Azure Bastion (Vorschau)

Bei der Arbeit mit Azure Bastion können Sie Netzwerksicherheitsgruppen (NSGs) verwenden. Weitere Informationen finden Sie unter [Sicherheitsgruppen](../virtual-network/security-overview.md). 

> [!IMPORTANT]
> Diese öffentliche Vorschauversion wird ohne Servicelevelvereinbarung bereitgestellt und sollte nicht für Produktionsworkloads verwendet werden. Unter Umständen werden bestimmte Features nicht unterstützt, verfügen über eingeschränkte Funktionen und sind nicht an allen Azure-Standorten verfügbar. Weitere Informationen finden Sie unter [Ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

![Architecture](./media/bastion-nsg/nsg_architecture.png)

In diesem Diagramm:

* Der Bastion-Host wird im virtuellen Netzwerk bereitgestellt.
* Der Benutzer stellt in einem HTML5-Browser eine Verbindung mit dem Azure-Portal her.
* Der Benutzer wählt den virtuellen Computer für die Verbindung aus.
* Mit nur einem Klick wird die RDP- oder SSH-Sitzung im Browser geöffnet.
* Für den virtuellen Azure-Computer ist keine öffentliche IP-Adresse erforderlich.

## <a name="nsg"></a>Netzwerksicherheitsgruppen

* **AzureBastionSubnet:** Azure Bastion wird im bestimmten AzureBastionSubnet bereitgestellt.  
    * **Eingehender Datenverkehr über das öffentliche Internet:** Azure Bastion erstellt eine öffentliche IP-Adresse, für die Port 443 auf der öffentlichen IP-Adresse für den eingehenden Datenverkehr aktiviert sein muss. Ports 3389/22 müssen NICHT auf dem AzureBastionSubnet geöffnet sein.
    * **Ausgehender Datenverkehr zu Ziel-VMs:** Azure Bastion erreicht die Ziel-VMs über die private IP-Adresse. Die Netzwerksicherheitsgruppen müssen den ausgehenden Datenverkehr zu anderen Ziel-VM-Subnetzen ermöglichen.
* **Ziel-VM-Subnetz:** Dieses Subnetz enthält die Ziel-VM, mit der Sie eine RDP-/SSH-Verbindung herstellen möchten.
    * **Eingehender Datenverkehr von Azure Bastion:** Azure Bastion erreicht die Ziel-VM über die private IP-Adresse. RDP/SSH-Ports (Port 3389 und 22) müssen über die private IP-Adresse auf der Ziel-VM-Seite geöffnet werden.

## <a name="apply"></a>Anwenden von NSGs auf AzureBastionSubnet

Wenn Sie NSGs auf das **AzureBastionSubnet** anwenden, lassen Sie die folgenden beiden Diensttags für Azure-Steuerungsebene und -Infrastruktur zu:

* **GatewayManager (nur Resource Manager)** : Dieses Tag gibt die Adresspräfixe des Azure Gateway Manager-Diensts an. Wenn Sie GatewayManager als Wert angeben, wird der Datenverkehr für GatewayManager zugelassen oder verweigert.

* **AzureCloud (nur Resource Manager)** : Dieses Tag gibt den IP-Adressraum für Azure an, einschließlich aller öffentlichen IP-Adressen für das Datencenter. Wenn Sie AzureCloud als Wert angeben, wird der Datenverkehr für öffentliche Azure-IP-Adressen zugelassen oder verweigert. Falls Sie den Zugriff auf AzureCloud nur für eine bestimmte Region zulassen möchten, können Sie die Region angeben. Falls Sie den Zugriff auf AzureCloud von Azure beispielsweise nur für die Region „USA, Osten“ zulassen möchten, können Sie AzureCloud.EastUS als Diensttag angeben.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen über Azure Bastion finden Sie in den [FAQ](bastion-faq.md).