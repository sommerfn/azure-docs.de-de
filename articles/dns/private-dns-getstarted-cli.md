---
title: 'Schnellstart: Erstellen einer privaten Azure DNS-Zone mit der Azure-Befehlszeilenschnittstelle'
description: In diesem Schnellstart erstellen und testen Sie eine private DNS-Zone und einen Eintrag in Azure DNS. Dies ist eine Schritt-für-Schritt-Anleitung zum Erstellen und Verwalten Ihrer ersten privaten DNS-Zone und Ihres ersten DNS-Eintrags mithilfe der Azure CLI.
services: dns
author: vhorne
ms.service: dns
ms.topic: quickstart
ms.date: 10/05/2019
ms.author: victorh
ms.openlocfilehash: 8cd5986d61765680698b6c682567dd6388d80a2a
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73158863"
---
# <a name="quickstart-create-an-azure-private-dns-zone-using-the-azure-cli"></a>Schnellstart: Erstellen einer privaten Azure DNS-Zone mit der Azure-Befehlszeilenschnittstelle

In diesem Schnellstart wird Schritt für Schritt gezeigt, wie Sie mit der Azure-Befehlszeilenschnittstelle (Azure CLI) Ihre erste private DNS-Zone und Ihren ersten DNS-Eintrag erstellen.

Eine DNS-Zone wird zum Hosten der DNS-Einträge für eine bestimmte Domäne verwendet. Wenn Sie eine Domäne in Azure DNS hosten möchten, müssen Sie eine DNS-Zone für diesen Domänennamen erstellen. Jeder DNS-Eintrag für Ihre Domäne wird dann in dieser DNS-Zone erstellt. Um eine private DNS-Zone in Ihrem virtuellen Netzwerk zu veröffentlichen, geben Sie die Liste mit den virtuellen Netzwerken an, für die das Auflösen von Einträgen in der Zone zulässig ist.  Diese werden als *verknüpfte* virtuelle Netzwerke bezeichnet. Wenn die automatische Registrierung aktiviert ist, aktualisiert Azure DNS auch die Zoneneinträge, sobald ein virtueller Computer erstellt, seine IP-Adresse geändert oder der virtuelle Computer gelöscht wird.

In dieser Schnellstartanleitung wird Folgendes vermittelt:

> [!div class="checklist"]
> * Erstellen einer privaten DNS-Zone
> * Erstellen von virtuellen Testcomputern
> * Erstellen eines zusätzlichen DNS-Eintrags
> * Testen der privaten Zone

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

Sie können für diesen Schnellstart auch [Azure PowerShell](private-dns-getstarted-powershell.md) verwenden.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-the-resource-group"></a>Ressourcengruppe erstellen

Erstellen Sie zunächst eine Ressourcengruppe für die DNS-Zone: 

```azurecli
az group create --name MyAzureResourceGroup --location "East US"
```

## <a name="create-a-private-dns-zone"></a>Erstellen einer privaten DNS-Zone

Im folgenden Beispiel wird ein virtuelles Netzwerk namens **myAzureVNet** erstellt. Anschließend wird eine DNS-Zone mit dem Namen **private.contoso.com** in der Ressourcengruppe **MyAzureResourceGroup** erstellt, die DNS-Zone mit dem virtuellen Netzwerk **MyAzureVnet** verknüpft und die automatische Registrierung aktiviert.

```azurecli
az network vnet create \
  --name myAzureVNet \
  --resource-group MyAzureResourceGroup \
  --location eastus \
  --address-prefix 10.2.0.0/16 \
  --subnet-name backendSubnet \
  --subnet-prefixes 10.2.0.0/24

az network private-dns zone create -g MyAzureResourceGroup \
   -n private.contoso.com

az network private-dns link vnet create -g MyAzureResourceGroup -n MyDNSLink \
   -z private.contoso.com -v myAzureVNet -e true
```

Wenn Sie eine Zone nur für die Namensauflösung erstellen möchten (keine automatische Hostnamenregistrierung), könnten Sie den Parameter `-e false` auslassen.

### <a name="list-dns-private-zones"></a>Auflisten von privaten DNS-Zonen

Verwenden Sie zum Aufzählen von DNS-Zonen `az network private-dns zone list`. Entsprechende Hilfeinformationen finden Sie unter `az network dns zone list --help`.

Wenn Sie die Ressourcengruppe angeben, werden nur die Zonen innerhalb der Ressourcengruppe aufgelistet:

```azurecli
az network private-dns zone list \
  -g MyAzureResourceGroup
```

Wenn Sie die Ressourcengruppe weglassen, werden alle Zonen im Abonnement aufgelistet:

```azurecli
az network private-dns zone list 
```

## <a name="create-the-test-virtual-machines"></a>Erstellen der virtuellen Testcomputer

Erstellen Sie nun zwei virtuelle Computer, um die private DNS-Zone zu testen:

```azurecli
az vm create \
 -n myVM01 \
 --admin-username AzureAdmin \
 -g MyAzureResourceGroup \
 -l eastus \
 --subnet backendSubnet \
 --vnet-name myAzureVnet \
 --nsg NSG01 \
 --nsg-rule RDP \
 --image win2016datacenter

az vm create \
 -n myVM02 \
 --admin-username AzureAdmin \
 -g MyAzureResourceGroup \
 -l eastus \
 --subnet backendSubnet \
 --vnet-name myAzureVnet \
 --nsg NSG01 \
 --nsg-rule RDP \
 --image win2016datacenter
```

Dies nimmt einige Minuten in Anspruch.

## <a name="create-an-additional-dns-record"></a>Erstellen eines zusätzlichen DNS-Eintrags

Verwenden Sie zum Erstellen eines DNS-Eintrags den Befehl `az network private-dns record-set [record type] add-record`. Hilfe zum Hinzufügen zu A-Einträgen finden Sie unter `az network private-dns record-set A add-record --help`.

 Im folgenden Beispiel wird ein Eintrag mit dem relativen Namen **db** in der DNS-Zone **private.contoso.com** in der Ressourcengruppe **MyAzureResourceGroup** erstellt. Der vollqualifizierte Name des Ressourceneintragssatzes lautet **db.private.contoso.com**. Der Eintragstyp lautet „A“ mit der IP-Adresse „10.2.0.4“.

```azurecli
az network private-dns record-set a add-record \
  -g MyAzureResourceGroup \
  -z private.contoso.com \
  -n db \
  -a 10.2.0.4
```

### <a name="view-dns-records"></a>Anzeigen von DNS-Einträgen

Listen Sie die DNS-Einträge in Ihrer Zone mit diesem Befehl auf:

```azurecli
az network private-dns record-set list \
  -g MyAzureResourceGroup \
  -z private.contoso.com
```

## <a name="test-the-private-zone"></a>Testen der privaten Zone

Nun können Sie die Namensauflösung für die private Zone **private.contoso.com** testen.

### <a name="configure-vms-to-allow-inbound-icmp"></a>Konfigurieren von virtuellen Computern zum Zulassen eingehender ICMP

Sie können den Ping-Befehl zum Testen der Namensauflösung verwenden. Konfigurieren Sie daher die Firewall auf beiden virtuellen Computern, um eingehende ICMP-Pakete zuzulassen.

1. Stellen Sie eine Verbindung mit „myVM01“ her, und öffnen Sie ein Windows PowerShell-Fenster mit Administratorrechten.
2. Führen Sie den folgenden Befehl aus:

   ```powershell
   New-NetFirewallRule –DisplayName "Allow ICMPv4-In" –Protocol ICMPv4
   ```

Wiederholen Sie den Schritt für „myVM02“.

### <a name="ping-the-vms-by-name"></a>Pingen der virtuellen Computer anhand des Namens

1. Führen Sie an der Windows PowerShell-Eingabeaufforderung von „myVM02“ einen Ping-Befehl für „myVM01“ aus. Verwenden Sie dazu den automatisch registrierten Hostnamen:

   ```
   ping myVM01.private.contoso.com
   ```

   Eine ähnliche Ausgabe wie die folgende sollte angezeigt werden:

   ```
   PS C:\> ping myvm01.private.contoso.com

   Pinging myvm01.private.contoso.com [10.2.0.4] with 32 bytes of data:
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time=1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128

   Ping statistics for 10.2.0.4:
       Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
   Approximate round trip times in milli-seconds:
       Minimum = 0ms, Maximum = 1ms, Average = 0ms
   PS C:\>
   ```

2. Pingen Sie jetzt den Namen **db**, den Sie zuvor erstellt haben:

   ```
   ping db.private.contoso.com
   ```

   Eine ähnliche Ausgabe wie die folgende sollte angezeigt werden:

   ```
   PS C:\> ping db.private.contoso.com

   Pinging db.private.contoso.com [10.2.0.4] with 32 bytes of data:
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128

   Ping statistics for 10.2.0.4:
       Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
   Approximate round trip times in milli-seconds:
       Minimum = 0ms, Maximum = 0ms, Average = 0ms
   PS C:\>
   ```

## <a name="delete-all-resources"></a>Löschen aller Ressourcen

Löschen Sie die Ressourcengruppe **MyAzureResourceGroup**, um die in diesem Schnellstart erstellten Ressourcen zu löschen, wenn Sie sie nicht mehr benötigen.

```azurecli
az group delete --name MyAzureResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Azure DNS Private Zones-Szenarien](private-dns-scenarios.md)

