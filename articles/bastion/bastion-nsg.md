---
title: Arbeiten mit virtuellen Computern und Netzwerksicherheitsgruppen in Azure Bastion | Microsoft-Dokumentation
description: Dieser Artikel beschreibt die Integration des NSG-Zugriffs in Azure Bastion.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 10/16/2019
ms.author: cherylmc
ms.openlocfilehash: 24279ff81daf0a350aa5234e78f27a99b7e4a03e
ms.sourcegitcommit: f29fec8ec945921cc3a89a6e7086127cc1bc1759
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72527996"
---
# <a name="working-with-nsg-access-and-azure-bastion"></a>Verwenden von NSG-Zugriff und Azure Bastion

Bei der Arbeit mit Azure Bastion können Sie Netzwerksicherheitsgruppen (NSGs) verwenden. Weitere Informationen finden Sie unter [Sicherheitsgruppen](../virtual-network/security-overview.md). 

![Architecture](./media/bastion-nsg/nsg-architecture.png)

In diesem Diagramm:

* Der Bastionhost wird im virtuellen Netzwerk bereitgestellt.
* Der Benutzer stellt in einem HTML5-Browser eine Verbindung mit dem Azure-Portal her.
* Der Benutzer navigiert zum virtuellen Azure-Computer, um eine RDP-/SSH-Verbindung herzustellen.
* Verbindungsintegration: RDP-/SSH-Sitzung innerhalb des Browsers mit nur einem Klick
* Für den virtuellen Azure-Computer ist keine öffentliche IP-Adresse erforderlich.

## <a name="nsg"></a>Netzwerksicherheitsgruppen

In diesem Abschnitt wird der Netzwerkdatenverkehr zwischen dem Benutzer und Azure Bastion und weiter zu virtuellen Zielcomputern in Ihrem virtuellen Netzwerk gezeigt:

### <a name="azurebastionsubnet"></a>AzureBastionSubnet

Azure Bastion wird speziell im Azure Bastion-Subnetz (AzureBastionSubnet) bereitgestellt.

* **Eingehender Datenverkehr:**

   * **Eingehender Datenverkehr über das öffentliche Internet:** Azure Bastion erstellt eine öffentliche IP-Adresse, für die Port 443 auf der öffentlichen IP-Adresse für den eingehenden Datenverkehr aktiviert sein muss. Ports 3389/22 müssen NICHT auf dem AzureBastionSubnet geöffnet sein.
   * **Eingehender Datenverkehr von der Azure Bastion-Steuerungsebene:** Aktivieren Sie für Steuerungsebenenkonnektivität den Eingangsport 443 über das Diensttag **GatewayManager**. Dadurch kann die Steuerungsebene (also der Gateway-Manager) mit Azure Bastion kommunizieren.

* **Ausgehender Datenverkehr:**

   * **Ausgehender Datenverkehr für virtuelle Zielcomputer:** Azure Bastion erreicht die Ziel-VMs über die private IP-Adresse. Die Netzwerksicherheitsgruppen müssen ausgehenden Datenverkehr für andere Ziel-VM-Subnetze für die Ports 3389 und 22 zulassen.
   * **Ausgehender Datenverkehr für andere öffentliche Endpunkte in Azure:** Azure Bastion muss eine Verbindung mit verschiedenen öffentlichen Endpunkten in Azure herstellen können, um beispielsweise Diagnose- und Messprotokolle zu speichern. Aus diesem Grund benötigt Azure Bastion ausgehende Konnektivität über den Port 443 mit dem Diensttag **AzureCloud**.

* **Ziel-VM-Subnetz:** Dieses Subnetz enthält die Ziel-VM, mit der Sie eine RDP-/SSH-Verbindung herstellen möchten.

   * **Eingehender Datenverkehr von Azure Bastion:** Azure Bastion erreicht die Ziel-VM über die private IP-Adresse. RDP-/SSH-Ports (Port 3389 bzw. 22) müssen aufseiten des virtuellen Zielcomputers für die private IP-Adresse geöffnet werden. Es empfiehlt sich, in dieser Regel den IP-Adressbereich des Azure Bastion-Subnetzes hinzuzufügen, damit diese Ports auf den virtuellen Zielcomputern in Ihrem Ziel-VM-Subnetz nur von Bastion geöffnet werden können.

## <a name="apply"></a>Anwenden von NSGs auf AzureBastionSubnet

Wenn Sie eine NSG erstellen und auf ***AzureBastionSubnet*** anwenden, stellen Sie sicher, dass Sie die folgenden Regeln in Ihrer NSG hinzugefügt haben. Wenn Sie diese Regeln nicht hinzufügen, ist die Erstellung/Aktualisierung der NSG nicht erfolgreich:

* **Steuerungsebenenkonnektivität:** Eingehend am Port 443 von „GatewayManager“
* **Diagnoseprotokollierung und Sonstiges:** Ausgehend am Port 443 an „AzureCloud“. Regionale Tags innerhalb dieses Diensttags werden noch nicht unterstützt.
* **Virtueller Zielcomputer:** Ausgehend für die Ports 3389 und 22 an „VirtualNetwork“

Eine exemplarische NSG-Regel finden Sie in der [Schnellstartvorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/101-azure-bastion).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure Bastion finden Sie in den [häufig gestellten Fragen](bastion-faq.md).