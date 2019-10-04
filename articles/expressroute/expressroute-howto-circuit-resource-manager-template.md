---
title: 'Erstellen einer ExpressRoute-Verbindung – Resource Manager-Vorlage: Azure | Microsoft-Dokumentation'
description: Erstellen, Bereitstellen, Löschen und Aufheben der Bereitstellung einer ExpressRoute-Verbindung.
services: expressroute;azure-resource-manager
author: cherylmc
ms.service: expressroute
ms.topic: article
ms.date: 07/05/2019
ms.author: cherylmc
ms.reviewer: ganesr
ms.openlocfilehash: 103c61b6ad244bf4b140f897c070ce5bfd54cded
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2019
ms.locfileid: "67849220"
---
# <a name="create-an-expressroute-circuit-by-using-azure-resource-manager-template"></a>Erstellen einer ExpressRoute-Verbindung mithilfe einer Azure Resource Manager-Vorlage

> [!div class="op_single_selector"]
> * [Azure-Portal](expressroute-howto-circuit-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-circuit-arm.md)
> * [Azure-Befehlszeilenschnittstelle](howto-circuit-cli.md)
> * [Azure Resource Manager-Vorlage](expressroute-howto-circuit-resource-manager-template.md)
> * [Video – Azure-Portal](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (klassisch)](expressroute-howto-circuit-classic.md)
>

Erfahren Sie, wie Sie eine ExpressRoute-Verbindung erstellen, indem Sie eine Azure Resource Manager-Vorlage mit Azure PowerShell bereitstellen. Weitere Informationen zur Entwicklung von Resource Manager-Vorlagen finden Sie in der [Resource Manager-Dokumentation](/azure/azure-resource-manager/) und der [Vorlagenreferenz](/azure/templates/microsoft.network/expressroutecircuits).

## <a name="before-you-begin"></a>Voraussetzungen

* Lesen Sie vor Beginn der Konfiguration die Seiten zu den [Voraussetzungen](expressroute-prerequisites.md) und [Workflows](expressroute-workflows.md).
* Stellen Sie sicher, dass Sie über die notwendigen Berechtigungen verfügen, um neue Netzwerkressourcen zu erstellen. Wenden Sie sich an Ihren Kontoadministrator, wenn Sie nicht über die richtigen Berechtigungen verfügen.
* Sie können sich das [Video ansehen](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit), bevor Sie beginnen, um die Schritte besser zu verstehen.

## <a name="create"></a>Erstellen und Bereitstellen einer ExpressRoute-Verbindung

[Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/) bieten eine gute Sammlung von Resource Manager-Vorlagen. Verwenden Sie eine der [vorhandenen Vorlagen](https://azure.microsoft.com/resources/templates/101-expressroute-circuit-create/) zum Erstellen einer ExpressRoute-Verbindung.

[!code-json[create-azure-expressroute-circuit](~/quickstart-templates/101-expressroute-circuit-create/azuredeploy.json)]

[Hier](https://azure.microsoft.com/resources/templates/?term=expressroute) finden Sie weitere verwandte Vorlagen.

So erstellen Sie eine ExpressRoute-Verbindung durch Bereitstellen einer Vorlage:

1. Wählen Sie im folgenden Codeblock **Ausprobieren** aus, und folgen Sie dann den Anweisungen, um sich bei Azure Cloud Shell anzumelden.

    ```azurepowershell-interactive
    $circuitName = Read-Host -Prompt "Enter a circuit name"
    $location = Read-Host -Prompt "Enter the location (i.e. centralus)"
    $resourceGroupName = "${circuitName}rg"
    $templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-expressroute-circuit-create/azuredeploy.json"

    $serviceProviderName = "Equinix"
    $peeringLocation = "Silicon Valley"
    $bandwidthInMbps = 500
    $sku_tier = "Premium"
    $sku_family = "MeteredData"

    New-AzResourceGroup -Name $resourceGroupName -Location $location
    New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -circuitName $circuitName -serviceProviderName $serviceProviderName -peeringLocation $peeringLocation -bandwidthInMbps $bandwidthInMbps -sku_tier $sku_tier -sku_family $sku_family

    Write-Host "Press [ENTER] to continue ..."
    ```

   * Der **Tarif** bestimmt, ob ein ExpressRoute Standard- oder ein ExpressRoute Premium-Add-On aktiviert wird. Sie können **Standard** für die Standard-SKU bzw. **Premium** für das Premium-Add-On angeben.

   * Der **Peeringort** ist der physische Standort, an dem Ihr Peering mit Microsoft stattfindet.

     > [!IMPORTANT]
     > Der Peeringstandort entspricht dem [physischen Standort](expressroute-locations.md), an dem Ihr Peering mit Microsoft stattfindet. Dieser ist **nicht** mit der Eigenschaft „Standort“ verknüpft, die sich auf den geografischen Standort des Azure-Netzwerkressourcenanbieters befindet. Obgleich sie nicht miteinander in Zusammenhang stehen, sollten Sie einen Netzwerkressourcenanbieter in geografischer Nähe des Peeringstandorts der Verbindung wählen.

    Der Ressourcengruppenname entspricht dem Service Bus-Namespace mit dem Zusatz **rg**.

2. Wählen Sie **Kopieren**, um das PowerShell-Skript zu kopieren.
3. Klicken Sie mit der rechten Maustaste auf die Shellkonsole, und wählen Sie **Einfügen** aus.

Es dauert einen Augenblick, einen Event Hub zu erstellen.

In diesem Tutorial wird Azure PowerShell verwendet, um die Vorlage bereitzustellen. Weitere Methoden zum Bereitstellen von Vorlagen finden Sie unter:

* [Über das Azure-Portal](../azure-resource-manager/resource-group-template-deploy-portal.md).
* [Mit der Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md).
* [Mit der REST-API](../azure-resource-manager/resource-group-template-deploy-rest.md).

## <a name="delete"></a>Aufheben der Bereitstellung und Löschen einer ExpressRoute-Verbindung

Sie können Ihre ExpressRoute-Verbindung löschen. Wählen Sie dazu das Symbol **Löschen** aus. Beachten Sie die folgenden Informationen:

* Sie müssen die Verknüpfung aller virtuellen Netzwerke mit der ExpressRoute-Verbindung aufheben. Falls dieser Vorgang nicht erfolgreich ist, überprüfen Sie, ob noch virtuelle Netzwerke mit der Verbindung verknüpft sind.
* Wenn der Bereitstellungsstatus des ExpressRoute-Verbindungsdienstanbieters **Bereitstellung** oder **Bereitgestellt** lautet, arbeiten Sie mit Ihrem Dienstanbieter zusammen, um die Verbindungsbereitstellung auf Anbieterseite aufzuheben. Microsoft reserviert weiterhin Ressourcen für Sie und stellt Ihnen dies in Rechnung, bis der Dienstanbieter die Aufhebung der Verbindungsbereitstellung abgeschlossen hat und uns benachrichtigt.
* Wenn der Dienstanbieter die Bereitstellung der Verbindung aufgehoben hat (Bereitstellungsstatus des Dienstanbieters lautet **Nicht bereitgestellt**), können Sie die Verbindung löschen. Damit wird die Abrechnung für die Verbindung beendet.

Sie können die ExpressRoute-Verbindung löschen, indem Sie den folgenden PowerShell-Befehl ausführen:

```azurepowershell-interactive
$circuitName = Read-Host -Prompt "Enter the same circuit name that you used earlier"
$resourceGroupName = "${circuitName}rg"

Remove-AzExpressRouteCircuit -ResourceGroupName $resourceGroupName -Name $circuitName
```

## <a name="next-steps"></a>Nächste Schritte

Führen Sie nach dem Erstellen Ihrer Verbindung folgende Aufgaben durch:

* [Erstellen und Ändern des Routings für Ihre ExpressRoute-Verbindung](expressroute-howto-routing-portal-resource-manager.md)
* [Verknüpfen Ihres virtuelles Netzwerks mit Ihrer ExpressRoute-Verbindung](expressroute-howto-linkvnet-arm.md)
