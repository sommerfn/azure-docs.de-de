---
title: Erstellen eines privaten Azure-Endpunkts mit Azure CLI | Microsoft-Dokumentation
description: Informationen zum privaten Endpunkt in Azure
services: private-link
author: KumudD
ms.service: private-link
ms.topic: article
ms.date: 09/16/2019
ms.author: kumud
ms.openlocfilehash: 477f7d4824d3165357228d200dca9e556a072744
ms.sourcegitcommit: 87efc325493b1cae546e4cc4b89d9a5e3df94d31
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73053506"
---
# <a name="create-a-private-endpoint-using-azure-cli"></a>Erstellen eines privaten Endpunkts mit Azure CLI
Ein privater Endpunkt ist der grundlegende Baustein für Private Link in Azure. Mit ihm können Azure-Ressourcen wie virtuelle Computer (VMs) privat mit Private Link-Ressourcen kommunizieren. In diesem Schnellstart erfahren Sie, wie Sie einen virtuellen Computer in einem virtuellen Netzwerk und einen SQL-Datenbank-Server mit einem privaten Endpunkt unter Verwendung von Azure CLI erstellen. Anschließend können Sie auf den virtuellen Computer zugreifen und sicher auf die Private Link-Ressource zugreifen (in diesem Beispiel auf eine private Azure SQL-Datenbank-Server-Instanz). 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie stattdessen entscheiden, Azure CLI lokal zu installieren und zu verwenden möchten, müssen Sie für diese Schnellstartanleitung mindestens die Azure CLI-Version 2.0.28 verwenden. Führen Sie `az --version` aus, um die installierte Version zu ermitteln. Informationen zum Ausführen einer Installation oder eines Upgrades finden Sie unter [Installieren der Azure CLI](/cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Bevor Sie eine Ressource erstellen können, müssen Sie eine Ressourcengruppe zum Hosten des virtuellen Netzwerks erstellen. Erstellen Sie mit [az group create](/cli/azure/group) eine Ressourcengruppe. Bei diesem Beispiel wird am Standort *westcentralus* eine Ressourcengruppe namens *myResourceGroup* erstellt:

```azurecli-interactive
az group create --name myResourceGroup --location westcentralus
```

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks
Erstellen Sie mit [az network vnet create](/cli/azure/network/vnet) ein virtuelles Netzwerk. Bei diesem Beispiel wird ein virtuelles Standardnetzwerk mit dem Namen *myVirtualNetwork* und dem Subnetz *mySubnet* erstellt:

```azurecli-interactive
az network vnet create \
 --name myVirtualNetwork \
 --resource-group myResourceGroup \
 --subnet-name mySubnet
```
## <a name="disable-subnet-private-endpoint-policies"></a>Deaktivieren von Richtlinien für den privaten Endpunkt im Subnetz 
Azure stellt Ressourcen für ein Subnetz innerhalb eines virtuellen Netzwerks bereit. Daher müssen Sie das Subnetz erstellen oder aktualisieren, um die Netzwerkrichtlinien für den privaten Endpunkt im Subnetz zu deaktivieren. Aktualisieren Sie eine Subnetzkonfiguration mit dem Namen *mySubnet* mit [az network vnet subnet update](https://docs.microsoft.com/cli/azure/network/vnet/subnet?view=azure-cli-latest#az-network-vnet-subnet-update):

```azurecli-interactive
az network vnet subnet update \
 --name mySubnet \
 --resource-group myResourceGroup \
 --vnet-name myVirtualNetwork \
 --disable-private-endpoint-network-policies true
```
## <a name="create-the-vm"></a>Erstellen des virtuellen Computers 
Erstellen Sie mit „az vm create“ einen virtuellen Computer. Geben Sie bei entsprechender Aufforderung ein Kennwort als Anmeldeinformation für den virtuellen Computer ein. In diesem Beispiel wird ein virtueller Computer mit dem Namen *myVm* erstellt: 
```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVm \
  --image Win2019Datacenter
```
 Notieren Sie die öffentliche IP-Adresse der VM. Sie verwenden diese Adresse im nächsten Schritt zum Herstellen einer Verbindung mit dem virtuellen Computer über das Internet.

## <a name="create-a-sql-database-server"></a>Erstellen einer SQL-Datenbank-Server-Instanz 
Erstellen Sie mit dem Befehl „az sql server create“ eine SQL-Datenbank-Server-Instanz. Denken Sie daran, dass der Name Ihrer SQL Server-Instanz innerhalb von Azure eindeutig sein muss, ersetzen Sie daher den Platzhalterwert in Klammern durch Ihren eigenen eindeutigen Wert: 

```azurecli-interactive
# Create a logical server in the resource group 
az sql server create \ 
    --name "myserver"\ 
    --resource-group myResourceGroup \ 
    --location WestUS \ 
    --admin-user "sqladmin" \ 
    --admin-password "CHANGE_PASSWORD_1" 
 
# Create a database in the server with zone redundancy as false 
az sql db create \ 
    --resource-group myResourceGroup  \ 
    --server myserver \ 
    --name mySampleDatabase \ 
    --sample-name AdventureWorksLT \ 
    --edition GeneralPurpose \ 
    --family Gen4 \ 
    --capacity 1 
```

Beachten Sie, dass die SQL Server-ID ähnlich ist wie ```/subscriptions/subscriptionId/resourceGroups/myResourceGroup/providers/Microsoft.Sql/servers/myserver.``` Sie verwenden die SQL Server-ID im nächsten Schritt. 

## <a name="create-the-private-endpoint"></a>Erstellen des privaten Endpunkts 
Erstellen Sie einen privaten Endpunkt für den SQL-Datenbank-Server in Ihrem virtuellen Netzwerk: 
```azurecli-interactive
az network private-endpoint create \  
    --name myPrivateEndpoint \  
    --resource-group myResourceGroup \  
    --vnet-name myVirtualNetwork  \  
    --subnet mySubnet \  
    --private-connection-resource-id "<SQL Server ID>" \  
    --group-ids sqlServer \  
    --connection-name myConnection  
 ```
## <a name="configure-the-private-dns-zone"></a>Konfigurieren der privaten DNS-Zone 
Erstellen Sie eine private DNS-Zone für die SQL-Datenbank-Server-Domäne, und erstellen Sie eine Zuordnungsverknüpfung mit dem virtuellen Netzwerk. 
```azurecli-interactive
az network private-dns zone create --resource-group myResourceGroup \ 
   --name  "privatelink.database.windows.net" 
az network private-dns link vnet create --resource-group myResourceGroup \ 
   --zone-name  "privatelink.database.windows.net"\ 
   --name MyDNSLink \ 
   --virtual-network myVirtualNetwork \ 
   --registration-enabled false 

#Query for the network interface ID  
networkInterfaceId=$(az network private-endpoint show --name myPrivateEndpoint --resource-group myResourceGroup --query 'networkInterfaces[0].id' -o tsv)
 
 
az resource show --ids $networkInterfaceId --api-version 2019-04-01 -o json 
# Copy the content for privateIPAddress and FQDN matching the SQL server name 
 
 
#Create DNS records 
az network private-dns record-set a create --name myserver --zone-name privatelink.database.windows.net --resource-group myResourceGroup  
az network private-dns record-set a add-record --record-set-name myserver --zone-name privatelink.database.windows.net --resource-group myResourceGroup -a <Private IP Address>
```

## <a name="connect-to-a-vm-from-the-internet"></a>Herstellen einer Verbindung mit einem virtuellen Computer über das Internet

Stellen Sie wie folgt eine Verbindung mit dem virtuellen Computer *myVm* aus dem Internet her:

1. Geben Sie in der Suchleiste des Portals *myVm* ein.

1. Wählen Sie die Schaltfläche **Verbinden** aus. Nach dem Auswählen der Schaltfläche **Verbinden** wird **Verbindung mit virtuellem Computer herstellen** geöffnet.

1. Wählen Sie **RDP-Datei herunterladen** aus. Azure erstellt eine Remotedesktopprotokoll-Datei (*RDP*) und lädt sie auf Ihren Computer herunter.

1. Öffnen Sie die heruntergeladene RDP*-Datei.

    1. Wenn Sie dazu aufgefordert werden, wählen Sie **Verbinden** aus.

    1. Geben Sie den Benutzernamen und das Kennwort ein, den/das Sie beim Erstellen des virtuellen Computers angegeben haben.

        > [!NOTE]
        > Unter Umständen müssen Sie **Weitere Optionen** > **Anderes Konto verwenden** auswählen, um die Anmeldeinformationen anzugeben, die Sie beim Erstellen des virtuellen Computers eingegeben haben.

1. Klicken Sie auf **OK**.

1. Während des Anmeldevorgangs wird unter Umständen eine Zertifikatwarnung angezeigt. Wenn Sie eine Zertifikatwarnung erhalten, wählen Sie **Ja** oder **Weiter** aus.

1. Sobald der VM-Desktop angezeigt wird, minimieren Sie ihn, um zu Ihrem lokalen Desktop zurückzukehren.  

## <a name="access-sql-database-server-privately-from-the-vm"></a>Privates Zugreifen auf den SQL-Datenbank-Server vom virtuellen Computer

In diesem Abschnitt stellen Sie eine Verbindung mit dem SQL-Datenbank-Server von der VM her, indem Sie den privaten Endpunkt verwenden.

 1. Öffnen Sie auf dem Remotedesktop von *myVM* PowerShell.
 2. Geben Sie „nslookup MyServer.Database.Windows.net“ ein. Sie erhalten eine Meldung wie die folgende: 

```
      Server:  UnKnown 
      Address:  168.63.129.16 
      Non-authoritative answer: 
      Name:    myserver.privatelink.database.windows.net 
      Address:  10.0.0.5 
      Aliases:  myserver.database.windows.net 
```
 3. Installieren von SQL Server Management Studio 
 4. Geben Sie in „Mit Server verbinden“ diese Informationen ein, oder wählen Sie sie aus: Servertyp: Wählen Sie „Datenbank-Engine“ aus.
 Servername: Wählen Sie „myserver.database.windows.net“ aus. Benutzername: Geben Sie einen während der Erstellung angegebenen Benutzernamen ein.
 Password (Kennwort): Geben Sie ein während der Erstellung angegebenes Kennwort ein.
 Kennwort speichern: Wählen Sie „Ja“ aus.
 
 5. Wählen Sie **Verbinden**aus.
 6. Durchsuchen Sie **Datenbanken** im linken Menü.
 7. (Optional) Erstellen oder Abfragen von Informationen aus *mydatabase*
 8. Schließen Sie die Remotedesktopverbindung mit *myVm*.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen 
Wenn die Ressourcengruppe und alle enthaltenen Ressourcen nicht mehr benötigt werden, können Sie sie mit „az group delete“ entfernen: 

```azurecli-interactive
az group delete --name myResourceGroup --yes 
```

## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zu [Azure Private Link](private-link-overview.md).
 
