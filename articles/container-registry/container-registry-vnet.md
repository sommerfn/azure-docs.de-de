---
title: Beschränken des Zugriffs auf eine Azure-Containerregistrierung auf ein virtuelles Netzwerk
description: Erlauben Sie den Zugriff auf eine Azure-Containerregistrierung nur von Ressourcen in einem virtuellen Azure-Netzwerk oder von öffentlichen IP-Adressbereichen.
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: article
ms.date: 07/01/2019
ms.author: danlep
ms.openlocfilehash: 3050a52da4d39657bd7b2fb38e235b9bd418faf4
ms.sourcegitcommit: 08d3a5827065d04a2dc62371e605d4d89cf6564f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2019
ms.locfileid: "68619886"
---
# <a name="restrict-access-to-an-azure-container-registry-using-an-azure-virtual-network-or-firewall-rules"></a>Beschränken des Zugriffs auf eine Azure-Containerregistrierung mithilfe eines virtuellen Azure-Netzwerks oder mit Firewallregeln

Das virtuelle Azure-Netzwerk ([Azure Virtual Network](../virtual-network/virtual-networks-overview.md)) stellt ein sicheres, privates Netzwerk für Ihre Azure- und lokalen Ressourcen bereit. Indem Sie den Zugriff auf Ihre private Azure-Containerregistrierung nur von einem virtuellen Azure-Netzwerk aus zulassen, gewährleisten Sie, dass nur Ressourcen im virtuellen Netzwerk Zugriff auf die Registrierung haben. Bei standortübergreifenden Szenarios können Sie außerdem Firewallregeln konfigurieren, um den Zugriff auf die Registrierung nur von bestimmten IP-Adressen zuzulassen.

In diesem Artikel werden zwei Szenarien zum Konfigurieren von Zugriffsregeln für eingehenden Netzwerkdatenverkehr in einer Containerregistrierung vorgestellt: Zugriff von einem in einem virtuellen Netzwerk bereitgestellten virtuellen Computer und Zugriff von der öffentlichen IP-Adresse eines virtuellen Computers.

> [!IMPORTANT]
> Dieses Feature befindet sich derzeit in der Vorschauphase. Es gelten einige [Einschränkungen](#preview-limitations). Vorschauversionen werden Ihnen zur Verfügung gestellt, wenn Sie die [zusätzlichen Nutzungsbedingungen][terms-of-use] akzeptieren. Einige Aspekte dieses Features werden bis zur allgemeinen Verfügbarkeit unter Umständen noch geändert.
>

Wenn Sie stattdessen Zugriffsregeln für Ressourcen einrichten müssen, um von einem Standort hinter einer Firewall auf eine Containerregistrierung zuzugreifen, lesen Sie die Informationen unter [Configure rules to access an Azure container registry behind a firewall](container-registry-firewall-access-rules.md) (Konfigurieren von Regeln für den Zugriff auf eine Azure-Containerregistrierung hinter einer Firewall).


## <a name="preview-limitations"></a>Einschränkungen der Vorschau

* Nur im Tarif **Premium** kann die Containerregistrierung mit Netzwerkregeln konfiguriert werden. Informationen zu den Tarifen des Registrierungsdiensts finden Sie unter [Azure Container Registry-SKUs](container-registry-skus.md). 

* Nur ein [Azure Kubernetes Service](../aks/intro-kubernetes.md)-Cluster oder ein [virtueller Azure-Computer](../virtual-machines/linux/overview.md) kann in einem virtuellen Netzwerk als Host für den Zugriff auf die Containerregistrierung verwendet werden. *Andere Azure-Dienste einschließlich Azure Container Instances werden derzeit nicht unterstützt.*

* [ACR Tasks](container-registry-tasks-overview.md)-Vorgänge werden derzeit in einer Containerregistrierung, auf die in einem virtuellen Netzwerk zugegriffen wird, nicht unterstützt.

* Jede Registrierung unterstützt bis zu 100 Regeln für virtuelle Netzwerke.

## <a name="prerequisites"></a>Voraussetzungen

* Um in diesem Artikel die Schritte an der Azure-Befehlszeilenschnittstelle (CLI) ausführen zu können, ist Azure CLI-Version 2.0.58 oder höher erforderlich. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI][azure-cli].

* Wenn Sie noch keine Containerregistrierung haben, können Sie eine Registrierung erstellen (Premium-SKU erforderlich) und mithilfe von Push ein Beispielimage wie `hello-world` vom Docker-Hub übertragen. Zum Erstellen einer Registrierung können Sie das [Azure-Portal][quickstart-portal] oder die [Azure-Befehlszeilenschnittstelle][quickstart-cli] verwenden. 

* Wenn Sie den Registrierungszugriff mithilfe eines virtuellen Netzwerks in einem anderen Azure-Abonnement einschränken möchten, müssen Sie den Ressourcenanbieter für Azure Container Registry in diesem Abonnement registrieren. Beispiel:

  ```azurecli
  az account set --subscription <Name or ID of subscription of virtual network>

  az provider register --namespace Microsoft.ContainerRegistry
  ``` 

## <a name="about-network-rules-for-a-container-registry"></a>Informationen zu Netzwerkregeln für eine Containerregistrierung

Eine Azure-Containerregistrierung akzeptiert standardmäßig Verbindungen über das Internet von Hosts in beliebigen Netzwerken. Mit einem virtuellen Netzwerk können Sie nur Azure-Ressourcen wie einem AKS-Cluster oder einem virtuellen Azure-Computer den sicheren Zugriff auf die Registrierung erlauben, ohne dass die Grenzen des Netzwerks überschritten werden. Sie können auch Firewallregeln für das Netzwerk konfigurieren, um bestimmte öffentliche Internet-IP-Adressbereiche auf die Whitelist zu setzen. 

Zum Beschränken des Zugriffs auf eine Registrierung müssen Sie zuerst die Standardaktion der Registrierung ändern, sodass die Registrierung alle Netzwerkverbindungen verweigert. Anschließend fügen Sie Netzwerkzugriffsregeln hinzu. Clients, denen mithilfe der Netzwerkregeln Zugriff gewährt wird, müssen sich weiterhin [bei der Containerregistrierung authentifizieren](https://docs.microsoft.com/azure/container-registry/container-registry-authentication) und für den Datenzugriff autorisiert sein.

### <a name="service-endpoint-for-subnets"></a>Dienstendpunkt für Subnetze

Um den Zugriff aus einem Subnetz in einem virtuellen Netzwerk zuzulassen, müssen Sie einen [Dienstendpunkt](../virtual-network/virtual-network-service-endpoints-overview.md) für den Azure Container Registry-Dienst hinzufügen. 

Dienste für mehrere Mandanten wie Azure Container Registry verwenden einen einzelnen Satz IP-Adressen für alle Kunden. Ein Dienstendpunkt weist einen Endpunkt für den Zugriff auf eine Registrierung zu. Dieser Endpunkt sorgt für eine optimale Datenverkehrsroute zu der Ressource über das Azure-Backbonenetzwerk. Mit jeder Anforderung wird zudem die Identität des virtuellen Netzwerks und des Subnetzes übertragen.

### <a name="firewall-rules"></a>Firewallregeln

Geben Sie für die IP-Netzwerkregeln zulässige Internetadressbereiche in CIDR-Notation im Format *16.17.18.0/24* oder als einzelne IP-Adressen (beispielsweise *16.17.18.19*) an. IP-Netzwerkregeln sind nur für *öffentliche* Internet-IP-Adressen zulässig. Für private Netzwerke reservierte IP-Adressbereiche (wie in RFC 1918 definiert) sind in IP-Adressregeln nicht zulässig.

## <a name="create-a-docker-enabled-virtual-machine"></a>Erstellen eines Docker-fähigen virtuellen Computers

In diesem Artikel wird ein Docker-fähiger virtueller Ubuntu-Computer für den Zugriff auf eine Azure-Containerregistrierung verwendet. Damit für die Registrierung die Azure Active Directory-Authentifizierung verwendet werden kann, installieren Sie auch die [Azure-Befehlszeilenschnittstelle (CLI)][azure-cli] auf dem virtuellen Computer. Wenn Sie bereits über einen virtuellen Azure-Computer verfügen, überspringen Sie diesen Erstellungsschritt.

Sie können die gleiche Ressourcengruppe für Ihren virtuellen Computer und Ihre Containerregistrierung verwenden. Dieses Setup vereinfacht die Bereinigung am Übungsende, ist aber nicht erforderlich. Wenn Sie eine separate Ressourcengruppe für den virtuellen Computer und das virtuelle Netzwerk erstellen möchten, führen Sie den Befehl [az group create][az-group-create] aus. Im folgenden Beispiel wird am Standort *westcentralus* eine Ressourcengruppe namens *myResourceGroup* erstellt:

```azurecli
az group create --name myResourceGroup --location westus
```

Stellen Sie nun mithilfe von [az vm create][az-vm-create] einen virtuellen Azure-Standardcomputer mit Ubuntu bereit. Im folgenden Beispiel wird ein virtueller Computer namens *myDockerVM* erstellt:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myDockerVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

Die Erstellung des virtuellen Computers dauert einige Minuten. Notieren Sie nach Abschluss der Befehlsausführung die von der Azure CLI angezeigte `publicIpAddress`. Verwenden Sie diese Adresse, um SSH-Verbindungen mit dem virtuellen Computer herzustellen (und optional für die spätere Einrichtung von Firewallregeln).

### <a name="install-docker-on-the-vm"></a>Installieren von Docker auf der VM

Wenn die VM ausgeführt wird, stellen Sie eine SSH-Verbindung mit der VM her. Ersetzen Sie *publicIpAddress* durch die öffentliche IP-Adresse Ihrer VM.

```bash
ssh azureuser@publicIpAddress
```

Führen Sie den folgenden Befehl aus, um Docker auf dem virtuellen Ubuntu-Computer zu installieren:

```bash
sudo apt install docker.io -y
```

Führen Sie nach der Installation den folgenden Befehl aus, um sicherzustellen, dass Docker ordnungsgemäß auf der VM ausgeführt wird:

```bash
sudo docker run -it hello-world
```

Ausgabe:

```
Hello from Docker!
This message shows that your installation appears to be working correctly.
[...]
```

### <a name="install-the-azure-cli"></a>Installieren der Azure CLI

Führen Sie die in [Installieren der Azure CLI mit apt](/cli/azure/install-azure-cli-apt?view=azure-cli-latest) beschriebenen Schritte aus, um die Azure CLI auf Ihrer Ubuntu-VM zu installieren. Für diesen Artikel müssen Sie sicherstellen, dass Sie Version 2.0.58 oder höher installieren.

Beenden Sie die SSH-Verbindung.

## <a name="allow-access-from-a-virtual-network"></a>Zulassen des Zugriffs von einem virtuellen Netzwerk

In diesem Abschnitt konfigurieren Sie Ihre Containerregistrierung für den Zugriff aus einem Subnetz in einem virtuellen Azure-Netzwerk. Die Schritte werden jeweils bei Verwendung der Azure-Befehlszeilenschnittstelle (CLI) bzw. des Azure-Portals erläutert.

### <a name="allow-access-from-a-virtual-network---cli"></a>Zulassen des Zugriffs von einem virtuellen Netzwerk – Befehlszeilenschnittstelle (CLI)

#### <a name="add-a-service-endpoint-to-a-subnet"></a>Hinzufügen eines Dienstendpunkts zu einem Subnetz

Wenn Sie einen virtuellen Computer erstellen, erstellt Azure standardmäßig in der gleichen Ressourcengruppe ein virtuelles Netzwerk. Der Name des virtuellen Netzwerks hängt von dem Namen des virtuellen Computers ab. Wenn Sie Ihren virtuellen Computer beispielsweise *MyDockerVM* nennen, lautet der Standardname des virtuellen Netzwerks *MyDockerVMVNET* mit einem Subnetz namens *MyDockerVMSubnet*. Dies können Sie im Azure-Portal oder mithilfe des Befehls [az network vnet list][az-network-vnet-list] überprüfen:

```azurecli
az network vnet list --resource-group myResourceGroup --query "[].{Name: name, Subnet: subnets[0].name}"
```

Ausgabe:

```console
[
  {
    "Name": "myDockerVMVNET",
    "Subnet": "myDockerVMSubnet"
  }
]
```

Verwenden Sie den Befehl [az network vnet subnet update][az-network-vnet-subnet-update], um Ihrem Subnetz einen **Microsoft.ContainerRegistry**-Dienstendpunkt hinzuzufügen. Ersetzen Sie im folgenden Befehl die Werte durch die Namen Ihres virtuellen Netzwerks und Ihres Subnetzes:

```azurecli
az network vnet subnet update \
  --name myDockerVMSubnet \
  --vnet-name myDockerVMVNET \
  --resource-group myResourceGroup \
  --service-endpoints Microsoft.ContainerRegistry
```

Rufen Sie mit dem Befehl [az network vnet subnet show][az-network-vnet-subnet-show] die Ressourcen-ID des Subnetzes ab. Diese benötigen Sie in einem späteren Schritt zum Konfigurieren einer Netzwerkzugriffsregel.

```azurecli
az network vnet subnet show \
  --name myDockerVMSubnet \
  --vnet-name myDockerVMVNET \
  --resource-group myResourceGroup \
  --query "id"
  --output tsv
``` 

Ausgabe:

```
/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myDockerVMVNET/subnets/myDockerVMSubnet
```

#### <a name="change-default-network-access-to-registry"></a>Ändern des Standardnetzwerkzugriffs auf die Registrierung

Eine Azure-Containerregistrierung lässt standardmäßig Verbindungen von Hosts in beliebigen Netzwerken zu. Um den Zugriff auf ein ausgewähltes Netzwerk zu beschränken, ändern Sie die Standardaktion so, dass der Zugriff verweigert wird. Ersetzen Sie im folgenden Befehl [az acr update][az-acr-update] den Wert durch den Namen Ihrer Registrierung:

```azurecli
az acr update --name myContainerRegistry --default-action Deny
```

#### <a name="add-network-rule-to-registry"></a>Hinzufügen einer Netzwerkregel zur Registrierung

Fügen Sie mit dem Befehl [az acr network-rule add][az-acr-network-rule-add] Ihrer Registrierung eine Netzwerkregel hinzu, die den Zugriff vom Subnetz des virtuellen Computers erlaubt. Ersetzen Sie im folgenden Befehl die Werte durch den Namen der Containerregistrierung und die Ressourcen-ID des Subnetzes: 

 ```azurecli
az acr network-rule add --name mycontainerregistry --subnet <subnet-resource-id>
```

[Prüfen Sie im nächsten Schritt den Zugriff auf die Registrierung](#verify-access-to-the-registry).

### <a name="allow-access-from-a-virtual-network---portal"></a>Zulassen des Zugriffs aus einem virtuellen Netzwerk – Azure-Portal

#### <a name="add-service-endpoint-to-subnet"></a>Hinzufügen eines Dienstendpunkts zu einem Subnetz

Wenn Sie einen virtuellen Computer erstellen, erstellt Azure standardmäßig in der gleichen Ressourcengruppe ein virtuelles Netzwerk. Der Name des virtuellen Netzwerks hängt von dem Namen des virtuellen Computers ab. Wenn Sie Ihren virtuellen Computer beispielsweise *MyDockerVM* nennen, lautet der Standardname des virtuellen Netzwerks *MyDockerVMVNET* mit einem Subnetz namens *MyDockerVMSubnet*.

So fügen Sie einem Subnetz einen Azure Container Registry-Dienstendpunkt hinzu

1. Geben Sie im Suchfeld oben im [Azure-Portal][azure-portal] den Suchbegriff *Virtuelle Netzwerke* ein. Wenn **Virtuelle Netzwerke** in den Suchergebnissen angezeigt wird, können Sie den Begriff auswählen.
1. Wählen Sie in der Liste der virtuellen Netzwerke das virtuelle Netzwerk aus, in dem Ihr virtueller Computer bereitgestellt wird (z.B. *myDockerVMVNET*).
1. Wählen Sie unter **Einstellungen** die Option **Subnetze** aus.
1. Wählen Sie das Subnetz aus, in dem Ihr virtueller Computer bereitgestellt wird (z.B. *myDockerVMSubnet*).
1. Wählen Sie unter **Dienstendpunkte** die Option **Microsoft.ContainerRegistry** aus.
1. Wählen Sie **Speichern** aus.

![Hinzufügen eines Dienstendpunkts zu einem Subnetz][acr-subnet-service-endpoint] 

#### <a name="configure-network-access-for-registry"></a>Konfigurieren des Netzwerkzugriffs für die Registrierung

Eine Azure-Containerregistrierung lässt standardmäßig Verbindungen von Hosts in beliebigen Netzwerken zu. So beschränken Sie den Zugriff auf das virtuelle Netzwerk

1. Navigieren Sie im Azure-Portal zu Ihrer Containerregistrierung.
1. Wählen Sie unter **Einstellungen** die Option **Firewall und virtuelle Netzwerke** aus.
1. Wenn der Zugriff standardmäßig verweigert werden soll, wählen Sie aus, dass Zugriff über **Ausgewählte Netzwerke** gewährt werden soll. 
1. Wählen Sie **Vorhandenes virtuelles Netzwerk hinzufügen** und dann das virtuelle Netzwerk und Subnetz aus, das Sie mit einem Dienstendpunkt konfiguriert haben. Wählen Sie **Hinzufügen**.
1. Wählen Sie **Speichern** aus.

![Konfigurieren des virtuellen Netzwerks für die Containerregistrierung][acr-vnet-portal]

[Prüfen Sie im nächsten Schritt den Zugriff auf die Registrierung](#verify-access-to-the-registry).

## <a name="allow-access-from-an-ip-address"></a>Zulassen des Zugriffs von einer IP-Adresse

In diesem Abschnitt konfigurieren Sie Ihre Containerregistrierung für den Zugriff von einer bestimmten IP-Adresse oder einem IP-Adressbereich. Die Schritte werden jeweils bei Verwendung der Azure-Befehlszeilenschnittstelle (CLI) bzw. des Azure-Portals erläutert.

### <a name="allow-access-from-an-ip-address---cli"></a>Zulassen des Zugriffs von einer IP-Adresse – Befehlszeilenschnittstelle (CLI)

#### <a name="change-default-network-access-to-registry"></a>Ändern des Standardnetzwerkzugriffs auf die Registrierung

Wenn Sie nicht bereits geschehen, aktualisieren Sie die Registrierungskonfiguration, um den Zugriff standardmäßig zu verweigern. Ersetzen Sie im folgenden Befehl [az acr update][az-acr-update] den Wert durch den Namen Ihrer Registrierung:

```azurecli
az acr update --name myContainerRegistry --default-action Deny
```

#### <a name="remove-network-rule-from-registry"></a>Entfernen der Netzwerkregel aus der Registrierung

Wenn Sie zuvor eine Netzwerkregel für den Zugriff vom Subnetz des virtuellen Computers hinzugefügt haben, entfernen Sie den Dienstendpunkt des Subnetzes und die Netzwerkregel. Ersetzen Sie im Befehl [az acr network-rule remove][az-acr-network-rule-remove] die Werte durch den Namen der Containerregistrierung und die Ressourcen-ID des Subnetzes, die Sie in einem früheren Schritt abgerufen haben: 

```azurecli
# Remove service endpoint

az network vnet subnet update \
  --name myDockerVMSubnet \
  --vnet-name myDockerVMVNET \
  --resource-group myResourceGroup \
  --service-endpoints ""

# Remove network rule

az acr network-rule remove --name mycontainerregistry --subnet <subnet-resource-id>
```

#### <a name="add-network-rule-to-registry"></a>Hinzufügen einer Netzwerkregel zur Registrierung

Fügen Sie mit dem Befehl [az acr network-rule add][az-acr-network-rule-add] Ihrer Registrierung eine Netzwerkregel hinzu, die den Zugriff von der IP-Adresse des virtuellen Computers erlaubt. Ersetzen Sie im folgenden Befehl die Werte durch den Namen der Containerregistrierung und die öffentliche IP-Adresse des virtuellen Computers.

```azurecli
az acr network-rule add --name mycontainerregistry --ip-address <public-IP-address>
```

[Prüfen Sie im nächsten Schritt den Zugriff auf die Registrierung](#verify-access-to-the-registry).

### <a name="allow-access-from-an-ip-address---portal"></a>Zulassen des Zugriffs von einer IP-Adresse – Azure-Portal

#### <a name="remove-existing-network-rule-from-registry"></a>Entfernen einer vorhandenen Netzwerkregel aus der Registrierung

Wenn Sie zuvor eine Netzwerkregel für den Zugriff vom Subnetz des virtuellen Computers hinzugefügt haben, entfernen Sie die vorhandene Regel. Überspringen Sie diesen Abschnitt, wenn Sie von einem anderen virtuellen Computer auf die Registrierung zugreifen möchten.

* Aktualisieren Sie die Subnetzeinstellungen, und entfernen Sie den Azure Container Registry-Dienstendpunkt für das Subnetz. 

  1. Navigieren Sie im [Azure-Portal][azure-portal] zu dem virtuellen Netzwerk, in dem Ihr virtueller Computer bereitgestellt wird.
  1. Wählen Sie unter **Einstellungen** die Option **Subnetze** aus.
  1. Wählen Sie das Subnetz aus, in dem Ihr virtueller Computer bereitgestellt wird.
  1. Deaktivieren Sie unter **Dienstendpunkte** das Kontrollkästchen **Microsoft.ContainerRegistry**. 
  1. Wählen Sie **Speichern** aus.

* Entfernen Sie die Netzwerkregel, die dem Subnetz den Zugriff auf die Registrierung erlaubt.

  1. Navigieren Sie im Azure-Portal zu Ihrer Containerregistrierung.
  1. Wählen Sie unter **Einstellungen** die Option **Firewall und virtuelle Netzwerke** aus.
  1. Wählen Sie unter **virtuelle Netzwerke** den Namen des virtuellen Netzwerks und dann **Entfernen** aus.
  1. Wählen Sie **Speichern** aus.

#### <a name="add-network-rule-to-registry"></a>Hinzufügen einer Netzwerkregel zur Registrierung

1. Navigieren Sie im Azure-Portal zu Ihrer Containerregistrierung.
1. Wählen Sie unter **Einstellungen** die Option **Firewall und virtuelle Netzwerke** aus.
1. Wenn nicht bereits geschehen, erlauben Sie den Zugriff von **ausgewählten Netzwerken**. 
1. Vergewissern Sie sich unter **Virtuelle Netzwerke**, dass kein Netzwerk ausgewählt ist.
1. Geben Sie unter **Firewall** die öffentliche IP-Adresse eines virtuellen Computers ein. Oder geben Sie einen Adressbereich in CIDR-Notation ein, der die IP-Adresse des virtuellen Computers enthält.
1. Wählen Sie **Speichern** aus.

![Konfigurieren einer Firewallregel für die Containerregistrierung][acr-vnet-firewall-portal]

[Prüfen Sie im nächsten Schritt den Zugriff auf die Registrierung](#verify-access-to-the-registry).

## <a name="verify-access-to-the-registry"></a>Prüfen des Zugriffs auf die Registrierung

Warten Sie einige Minuten, bis die Konfiguration aktualisiert wurde, und prüfen Sie dann, ob der virtuelle Computer auf die Containerregistrierung zugreifen kann. Stellen Sie eine SSH-Verbindung mit Ihrem virtuellen Computer her, und melden Sie sich mit dem Befehl [az acr login][az-acr-login] bei der Registrierung an. 

```bash
az acr login --name mycontainerregistry
```

Sie können Registerierungsvorgänge ausführen (z.B. `docker pull`), um mittels Pullvorgang ein Beispielimage aus der Registrierung abzurufen. Ersetzen Sie im Befehl die Werte durch den Image- und Tagwert, der Ihrer Registrierung entspricht, und verwenden Sie als Präfix den Namens des Anmeldeservers für die Registrierung (nur Kleinbuchstaben):

```bash
docker pull mycontainerregistry.azurecr.io/hello-world:v1
``` 

Docker ruft erfolgreich mittels Pullvorgang das Image auf den virtuellen Computer ab.

Dieses Beispiel zeigt, dass Sie über die Netzwerkzugriffsregel auf die private Containerregistrierung zugreifen können. Der Zugriff auf die Containerregistrierung kann jedoch nicht von einem anderen Anmeldehost erfolgen, für den keine Netzwerkzugriffsregel konfiguriert wurde. Wenn Sie versuchen, sich mit dem Befehl `az acr login` oder `docker login` von einem anderen Host anzumelden, sieht die Ausgabe in etwa wie folgt aus:

```Console
Error response from daemon: login attempt to https://xxxxxxx.azurecr.io/v2/ failed with status: 403 Forbidden
```

## <a name="restore-default-registry-access"></a>Wiederherstellen des Standardzugriffs auf die Registrierung

Zum Wiederherstellen des standardmäßigen Zugriffs auf die Registrierung müssen Sie alle konfigurierten Netzwerkregeln entfernen. Stellen Sie dann „Zugriff zulassen“ wieder als Standardaktion ein. Die Schritte werden jeweils bei Verwendung der Azure-Befehlszeilenschnittstelle (CLI) bzw. des Azure-Portals erläutert.

### <a name="restore-default-registry-access---cli"></a>Wiederherstellen des Standardzugriffs auf die Registrierung – Befehlszeilenschnittstelle (CLI)

#### <a name="remove-network-rules"></a>Entfernen von Netzwerkregeln

Um eine Liste der Netzwerkregeln anzuzeigen, die für Ihre Registrierung konfiguriert wurden, führen Sie den folgenden Befehl [az acr network-rule list][az-acr-network-rule-list] aus:

```azurecli
az acr network-rule list--name mycontainerregistry 
```

Führen Sie für jede konfigurierte Regel den Befehl [az acr network-rule remove][az-acr-network-rule-remove] aus, um die Regel zu entfernen. Beispiel:

```azurecli
# Remove a rule that allows access for a subnet. Substitute the subnet resource ID.

az acr network-rule remove \
  --name mycontainerregistry \
  --subnet /subscriptions/ \
  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myDockerVMVNET/subnets/myDockerVMSubnet

# Remove a rule that allows access for an IP address or CIDR range such as 23.45.1.0/24.

az acr network-rule remove \
  --name mycontainerregistry \
  --ip-address 23.45.1.0/24
```

#### <a name="allow-access"></a>Zugriff zulassen

Ersetzen Sie im folgenden Befehl [az acr update][az-acr-update] den Wert durch den Namen Ihrer Registrierung:
```azurecli
az acr update --name myContainerRegistry --default-action Allow
```

### <a name="restore-default-registry-access---portal"></a>Wiederherstellen des Standardzugriffs auf die Registrierung – Azure-Portal


1. Navigieren Sie im Azure-Portal zu Ihrer Containerregistrierung, und wählen Sie **Firewall und virtuelle Netzwerke** aus.
1. Wählen Sie unter **Virtuelle Netzwerke** jedes virtuelle Netzwerk und dann **Entfernen** aus.
1. Wählen Sie unter **Firewall** jeden Adressbereich und dann das Symbol „Löschen“ aus.
1. Wählen Sie unter **Zugriff erlauben von** den Eintrag **Alle Netzwerke** aus. 
1. Wählen Sie **Speichern** aus.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie alle Azure-Ressourcen in der gleichen Ressourcengruppe erstellt haben und sie nicht mehr benötigen, können Sie die Ressourcen optional mit einem einzigen Befehl [az group delete](/cli/azure/group) löschen:

```azurecli
az group delete --name myResourceGroup
```

Navigieren Sie zum Bereinigen von Ressourcen im Azure-Portal zu Ihrer Ressourcengruppe (z.B.„myResourceGroup“). Klicken Sie nach dem Laden der Ressourcengruppe auf **Ressourcengruppe löschen**, um die Ressourcengruppe und die dort gespeicherten Ressourcen zu entfernen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurden verschiedene Ressourcen und Features des virtuellen Netzwerks erläutert, wenn auch nur in Kürze. In der Dokumentation zu Azure Virtual Network wird ausführlich auf diese Themen eingegangen:

* [Virtuelles Netzwerk](https://docs.microsoft.com/azure/virtual-network/manage-virtual-network)
* [Subnetz](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-subnet)
* [Dienstendpunkte](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview)

<!-- IMAGES -->

[acr-subnet-service-endpoint]: ./media/container-registry-vnet/acr-subnet-service-endpoint.png
[acr-vnet-portal]: ./media/container-registry-vnet/acr-vnet-portal.png
[acr-vnet-firewall-portal]: ./media/container-registry-vnet/acr-vnet-firewall-portal.png

<!-- LINKS - External -->
[aci-helloworld]: https://hub.docker.com/r/microsoft/aci-helloworld/
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-create]: /cli/azure/acr#az-acr-create
[az-acr-show]: /cli/azure/acr#az-acr-show
[az-acr-repository-show]: /cli/azure/acr/repository#az-acr-repository-show
[az-acr-repository-list]: /cli/azure/acr/repository#az-acr-repository-list
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-network-rule-add]: /cli/azure/acr/network-rule/#az-acr-network-rule-add
[az-acr-network-rule-remove]: /cli/azure/acr/network-rule/#az-acr-network-rule-remove
[az-acr-network-rule-list]: /cli/azure/acr/network-rule/#az-acr-network-rule-list
[az-acr-run]: /cli/azure/acr#az-acr-run
[az-acr-update]: /cli/azure/acr#az-acr-update
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az-group-create]: /cli/azure/group
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[az-vm-create]: /cli/azure/vm#az-vm-create
[az-network-vnet-subnet-show]: /cli/azure/network/vnet/subnet/#az-network-vnet-subnet-show
[az-network-vnet-subnet-update]: /cli/azure/network/vnet/subnet/#az-network-vnet-subnet-update
[az-network-vnet-subnet-show]: /cli/azure/network/vnet/subnet/#az-network-vnet-subnet-show
[az-network-vnet-list]: /cli/azure/network/vnet/#az-network-vnet-list
[quickstart-portal]: container-registry-get-started-portal.md
[quickstart-cli]: container-registry-get-started-azure-cli.md
[azure-portal]: https://portal.azure.com
