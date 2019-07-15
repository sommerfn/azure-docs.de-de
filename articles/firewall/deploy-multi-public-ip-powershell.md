---
title: Bereitstellen der Azure Firewall mit mehreren öffentlichen IP-Adressen mithilfe von Azure PowerShell
description: In diesem Artikel erfahren Sie, wie Sie eine Azure Firewall mit mehreren öffentlichen IP-Adressen mit Hilfe von Azure PowerShell bereitstellen.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 7/2/2019
ms.author: victorh
ms.openlocfilehash: a5a53766df3338bb36913b589ebda970de55ec94
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/01/2019
ms.locfileid: "67491933"
---
# <a name="deploy-an-azure-firewall-with-multiple-public-ip-addresses-using-azure-powershell"></a>Bereitstellen einer Azure Firewall mit mehreren öffentlichen IP-Adressen mithilfe von Azure PowerShell

> [!IMPORTANT]
> Azure Firewall mit mehreren öffentlichen IP-Adressen ist derzeit in der öffentlichen Vorschau.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Sie können eine Azure Firewall mit bis zu 100 öffentlichen IP-Adressen bereitstellen.

Diese Funktion ermöglicht die folgenden Szenarien:

- **DNAT** – Sie können mehrere Standard-Port-Instanzen auf Ihre Backend-Server übersetzen. Wenn Sie beispielsweise zwei öffentliche IP-Adressen haben, können Sie den TCP-Port 3389 (RDP) für beide IP-Adressen übersetzen.
- **SNAT**- Für ausgehende SNAT-Verbindungen stehen zusätzliche Ports zur Verfügung, was das Potenzial für die Überlastung des SNAT-Ports reduziert. Zu diesem Zeitpunkt wählt Azure-Firewall nach dem Zufallsprinzip die öffentliche IP-Quelladresse, die für eine Verbindung verwendet werden soll. Wenn Sie in Ihrem Netzwerk über eine nachgeschaltete Filterung verfügen, müssen Sie alle öffentlichen IP-Adressen zulassen, die mit Ihrer Firewall verbunden sind.

Die folgenden Azure PowerShell-Beispiele zeigen, wie Sie öffentliche IP-Adressen für die Azure Firewall hinzufügen, entfernen und konfigurieren können.

> [!NOTE]
> Wenn Sie während der öffentlichen Vorschau einer laufenden Firewall eine öffentliche IP-Adresse hinzufügen oder entfernen, funktioniert die bestehende Inbound-Konnektivität mit DNAT-Regeln möglicherweise 40 - 120 Sekunden lang nicht. Sie können die erste öffentliche IP-Adresse, die der Firewall zugewiesen wurde, nicht entfernen, es sei denn, die Firewall ist nicht zugewiesen oder gelöscht.

## <a name="add-a-public-ip-address-to-an-existing-firewall"></a>Fügen Sie eine öffentliche IP-Adresse zu einer vorhandenen Firewall hinzu

In diesem Beispiel ist die öffentliche IP-Adresse *azFwPublicIp1*, die an die Firewall angehängt ist.

```azurepowershell
$pip = New-AzPublicIpAddress `
  -Name "azFwPublicIp1" `
  -ResourceGroupName "rg" `
  -Sku "Standard" `
  -Location "centralus" `
  -AllocationMethod Static

$azFw = Get-AzFirewall `
  -Name "AzureFirewall" `
  -ResourceGroupName "rg"

$azFw.AddPublicIpAddress($pip)

$azFw | Set-AzFirewall
```

## <a name="remove-a-public-ip-address-from-an-existing-firewall"></a>Entfernen einer öffentlichen IP-Adresse aus einer bestehenden Firewall

In diesem Beispiel wird die öffentliche IP-Adresse *azFwPublicIp1* von der Firewall getrennt.

```azurepowershell
$pip = Get-AzPublicIpAddress `
  -Name "azFwPublicIp1" `
  -ResourceGroupName "rg"

$azFw = Get-AzFirewall `
  -Name "AzureFirewall" `
  -ResourceGroupName "rg"

$azFw.RemovePublicIpAddress($pip)

$azFw | Set-AzFirewall
```

## <a name="create-a-firewall-with-two-or-more-public-ip-addresses"></a>Erstellen Sie eine Firewall mit mindestens zwei öffentliche IP-Adressen

In diesem Beispiel wird eine Firewall erstellt, die an das virtuelle Netzwerk *vnet* mit zwei öffentlichen IP-Adressen angeschlossen ist.

```azurepowershell
$rgName = "resourceGroupName"

$vnet = Get-AzVirtualNetwork `
  -Name "vnet" `
  -ResourceGroupName $rgName

$pip1 = New-AzPublicIpAddress `
  -Name "AzFwPublicIp1" `
  -ResourceGroupName "rg" `
  -Sku "Standard" `
  -Location "centralus" `
  -AllocationMethod Static

$pip2 = New-AzPublicIpAddress `
  -Name "AzFwPublicIp2" `
  -ResourceGroupName "rg" `
  -Sku "Standard" `
  -Location "centralus" `
  -AllocationMethod Static

New-AzFirewall `
  -Name "azFw" `
  -ResourceGroupName $rgName `
  -Location centralus `
  -VirtualNetwork $vnet `
  -PublicIpAddress @($pip1, $pip2)
```

## <a name="next-steps"></a>Nächste Schritte

* [Tutorial: Überwachen von Azure Firewall-Protokollen](./tutorial-diagnostics.md)
