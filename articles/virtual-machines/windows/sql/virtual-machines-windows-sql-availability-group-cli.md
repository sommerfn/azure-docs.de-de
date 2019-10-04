---
title: Konfigurieren einer Always On-Verfügbarkeitsgruppe für SQL Server auf einem virtuellen Azure-Computer mithilfe der Azure CLI
description: Verwenden Sie die Azure CLI, um den Windows-Failovercluster, den Verfügbarkeitsgruppenlistener und den internen Lastenausgleich auf einer SQL Server-VM in Azure zu erstellen.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 02/12/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 58174704051709a720950ac51591a1d53b9d01bb
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70100565"
---
# <a name="use-the-azure-cli-to-configure-an-always-on-availability-group-for-sql-server-on-an-azure-vm"></a>Konfigurieren einer Always On-Verfügbarkeitsgruppe für SQL Server auf einem virtuellen Azure-Computer mithilfe der Azure CLI
In diesem Artikel wird die Verwendung der [Azure CLI](/cli/azure/sql/vm?view=azure-cli-latest/) zum Bereitstellen eines Windows-Failoverclusters, zum Hinzufügen von virtuellen SQL Server-Computern zum Cluster sowie zum Erstellen des internen Lastenausgleichs und Listeners für eine Always On-Verfügbarkeitsgruppe beschrieben. Die Bereitstellung der Always On-Verfügbarkeitsgruppe erfolgt weiterhin manuell über SQL Server Management Studio (SSMS). 

## <a name="prerequisites"></a>Voraussetzungen
Wenn Sie die Einrichtung einer Always On-Verfügbarkeitsgruppe mithilfe der Azure CLI automatisieren möchten, muss Folgendes vorhanden sein: 
- Ein [Azure-Abonnement](https://azure.microsoft.com/free/).
- Eine Ressourcengruppe mit einem Domänencontroller. 
- Mindestens ein in eine Domäne eingebundener [virtueller Computer in Azure mit der Enterprise Edition von SQL Server 2016 (oder höher)](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision), der sich in der *gleichen Verfügbarkeitsgruppe oder anderen Verfügbarkeitszonen* befindet, die [beim SQL-VM-Ressourcenanbieter registriert wurden](virtual-machines-windows-sql-register-with-resource-provider.md).  
- Die [Azure CLI](/cli/azure/install-azure-cli) 
- Zwei verfügbare (nicht von einer Entität verwendete) IP-Adressen. Eine wird für den internen Lastenausgleich verwendet. Die andere ist für den Verfügbarkeitsgruppenlistener innerhalb des Subnetzes vorgesehen, in dem sich auch die Verfügbarkeitsgruppe befindet. Wenn ein vorhandener Lastenausgleich verwendet wird, wird nur eine verfügbare IP-Adresse für den Verfügbarkeitsgruppenlistener benötigt. 

## <a name="permissions"></a>Berechtigungen
Sie benötigen die folgenden Kontoberechtigungen, um die Always On-Verfügbarkeitsgruppe mithilfe der Azure CLI zu konfigurieren: 

- Ein vorhandenes Domänenbenutzerkonto mit der Berechtigung zum **Erstellen von Computerobjekten** in der Domäne. Beispielsweise verfügt ein Domänenadministratorkonto in der Regel über ausreichende Berechtigungen (Beispiel: account@domain.com). _Dieses Konto muss auch Teil der lokalen Administratorgruppe auf allen virtuellen Computern sein, um den Cluster zu erstellen._
- Das Domänenbenutzerkonto, das den SQL Server-Dienst steuert. 
 
## <a name="step-1-create-a-storage-account-as-a-cloud-witness"></a>Schritt 1: Erstellen eines Speicherkontos als Cloudzeuge
Der Cluster benötigt ein Speicherkonto, das als Cloudzeuge fungieren kann. Sie können ein beliebiges vorhandenes Speicherkonto verwenden oder ein neues Speicherkonto erstellen. Wenn Sie ein vorhandenes Speicherkonto verwenden möchten, fahren Sie mit dem nächsten Absatz fort. 

Das Speicherkonto wird mit dem folgenden Codeausschnitt erstellt: 
```azurecli-interactive
# Create the storage account
# example: az storage account create -n 'cloudwitness' -g SQLVM-RG -l 'West US' `
#  --sku Standard_LRS --kind StorageV2 --access-tier Hot --https-only true

az storage account create -n <name> -g <resource group name> -l <region ex:eastus> `
  --sku Standard_LRS --kind StorageV2 --access-tier Hot --https-only true
```

>[!TIP]
> Möglicherweise wird der Fehler `az sql: 'vm' is not in the 'az sql' command group` angezeigt, wenn Sie eine veraltete Version der Azure CLI verwenden. Laden Sie die [neueste Version von Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli-windows?view=azure-cli-latest) herunter, um diesen Fehler zu überwinden.

## <a name="step-2-define-windows-failover-cluster-metadata"></a>Schritt 2: Definieren von Metadaten für den Windows-Failovercluster
Die Azure CLI-Befehlsgruppe [az sql vm group](https://docs.microsoft.com/cli/azure/sql/vm/group?view=azure-cli-latest) verwaltet die Metadaten des Windows Server-Failoverclusterdiensts (WSFC), der die Verfügbarkeitsgruppe hostet. Zu den Clustermetadaten gehören die Active Directory-Domäne, Clusterkonten, als Cloudzeugen zu verwendende Speicherkonten und die SQL Server-Version. Verwenden Sie [az sql vm group create](https://docs.microsoft.com/cli/azure/sql/vm/group?view=azure-cli-latest#az-sql-vm-group-create), um die Metadaten für den WSFC zu definieren, sodass der Cluster beim Hinzufügen der ersten SQL Server-VM wie definiert erstellt wird. 

Im folgenden Codeausschnitt werden die Metadaten für den Cluster definiert:
```azurecli-interactive
# Define the cluster metadata
# example: az sql vm group create -n Cluster -l 'West US' -g SQLVM-RG `
#  --image-offer SQL2017-WS2016 --image-sku Enterprise --domain-fqdn domain.com `
#  --operator-acc vmadmin@domain.com --bootstrap-acc vmadmin@domain.com --service-acc sqlservice@domain.com `
#  --sa-key '4Z4/i1Dn8/bpbseyWX' `
#  --storage-account 'https://cloudwitness.blob.core.windows.net/'

az sql vm group create -n <cluster name> -l <region ex:eastus> -g <resource group name> `
  --image-offer <SQL2016-WS2016 or SQL2017-WS2016> --image-sku Enterprise --domain-fqdn <FQDN ex: domain.com> `
  --operator-acc <domain account ex: testop@domain.com> --bootstrap-acc <domain account ex:bootacc@domain.com> `
  --service-acc <service account ex: testservice@domain.com> `
  --sa-key '<PublicKey>' `
  --storage-account '<ex:https://cloudwitness.blob.core.windows.net/>'
```

## <a name="step-3-add-sql-server-vms-to-the-cluster"></a>Schritt 3: Hinzufügen von SQL Server-VMs zum Cluster
Beim Hinzufügen der ersten SQL Server-VM zum Cluster wird der Cluster erstellt. Der Befehl [az sql vm add-to-group](https://docs.microsoft.com/cli/azure/sql/vm?view=azure-cli-latest#az-sql-vm-add-to-group) erstellt den Cluster mit dem zuvor gegebenen Namen, installiert die Clusterrolle in den SQL Server-VMs und fügt sie dem Cluster hinzu. Nachfolgende Verwendungen des Befehls `az sql vm add-to-group` fügen dem neu erstellten Cluster weitere SQL Server-VMs hinzu. 

Der folgende Codeausschnitt erstellt den Cluster und fügt ihm die erste SQL Server-VM hinzu: 

```azurecli-interactive
# Add SQL Server VMs to cluster
# example: az sql vm add-to-group -n SQLVM1 -g SQLVM-RG --sqlvm-group Cluster `
#  -b Str0ngAzur3P@ssword! -p Str0ngAzur3P@ssword! -s Str0ngAzur3P@ssword!
# example: az sql vm add-to-group -n SQLVM2 -g SQLVM-RG --sqlvm-group Cluster `
#  -b Str0ngAzur3P@ssword! -p Str0ngAzur3P@ssword! -s Str0ngAzur3P@ssword!

az sql vm add-to-group -n <VM1 Name> -g <Resource Group Name> --sqlvm-group <cluster name> `
  -b <bootstrap account password> -p <operator account password> -s <service account password>
az sql vm add-to-group -n <VM2 Name> -g <Resource Group Name> --sqlvm-group <cluster name> `
  -b <bootstrap account password> -p <operator account password> -s <service account password>
```
Verwenden Sie diesen Befehl, um dem Cluster weitere SQL Server-VMs hinzuzufügen. Ändern Sie nur den Parameter `-n` für den Namen der SQL Server-VM. 

## <a name="step-4-create-the-availability-group"></a>Schritt 4: Erstellen der Verfügbarkeitsgruppe
Erstellen Sie die Verfügbarkeitsgruppe wie gewohnt manuell mithilfe von [SQL Server Management Studio](/sql/database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio), [PowerShell](/sql/database-engine/availability-groups/windows/create-an-availability-group-sql-server-powershell) oder [Transact-SQL](/sql/database-engine/availability-groups/windows/create-an-availability-group-transact-sql). 

>[!IMPORTANT]
> Erstellen Sie zu diesem Zeitpunkt *keinen* Listener, da dieser Schritt in den folgenden Abschnitten mithilfe der Azure CLI ausgeführt wird.  

## <a name="step-5-create-the-internal-load-balancer"></a>Schritt 5: Erstellen des internen Lastenausgleichs

Für den Always On-Verfügbarkeitsgruppenlistener ist eine interne Azure Load Balancer-Instanz erforderlich. Der interne Lastenausgleich stellt eine Floating IP-Adresse für den Verfügbarkeitsgruppenlistener bereit, um Failovervorgänge und Verbindungswiederherstellungen zu beschleunigen. Wenn die SQL Server-VMs in einer Verfügbarkeitsgruppe Teil des gleichen Verfügbarkeitssatzes sind, können Sie einen Lastenausgleich im Tarif „Basic“ verwenden. Andernfalls benötigen einen Lastenausgleich im Tarif „Standard“.  

> [!NOTE]
> Der interne Lastenausgleich muss sich im selben virtuellen Netzwerk befinden wie die SQL Server-VM-Instanzen. 

Der folgende Codeausschnitt erstellt den internen Lastenausgleich:

```azurecli-interactive
# Create the internal load balancer
# example: az network lb create --name sqlILB -g SQLVM-RG --sku Standard `
# --vnet-name SQLVMvNet --subnet default

az network lb create --name sqlILB -g <resource group name> --sku Standard `
  --vnet-name <VNet Name> --subnet <subnet name>
```

>[!IMPORTANT]
> Die öffentliche IP-Ressource für die einzelnen SQL Server-VMs muss über eine Standard-SKU verfügen, um mit dem Lastenausgleich im Tarif „Standard“ kompatibel zu sein. Um die SKU der öffentlichen IP-Ressource Ihrer VM zu ermitteln, navigieren Sie zu **Ressourcengruppe**, und wählen Sie die Ressource **Öffentliche IP-Adresse** für die gewünschte SQL Server-VM aus. Der Wert befindet sich im Bereich **Übersicht** unter **SKU**.  

## <a name="step-6-create-the-availability-group-listener"></a>Schritt 6: Erstellen des Verfügbarkeitsgruppenlisteners
Nachdem die Verfügbarkeitsgruppe manuell erstellt wurde, können Sie den Listener mithilfe von [az sql vm ag-listener](/cli/azure/sql/vm/group/ag-listener?view=azure-cli-latest#az-sql-vm-group-ag-listener-create) erstellen. 

Die *Subnetzressourcen-ID* ist der Wert von `/subnets/<subnetname>`, der an die Ressourcen-ID der VNET-Ressource angehängt wurde. So bestimmen Sie die Subnetzressourcen-ID:
   1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrer Ressourcengruppe. 
   1. Wählen Sie die Ressource für das virtuelle Netzwerk aus. 
   1. Wählen Sie im Bereich **Einstellungen** **Eigenschaften** aus. 
   1. Bestimmen Sie die Ressourcen-ID des virtuellen Netzwerks, und hängen Sie am Ende `/subnets/<subnetname>` an, um die Subnetzressourcen-ID zu erstellen. Beispiel:
      - Die Ressourcen-ID des virtuellen Netzwerks lautet `/subscriptions/a1a1-1a11a/resourceGroups/SQLVM-RG/providers/Microsoft.Network/virtualNetworks/SQLVMvNet`.
      - Ihr Subnetzname ist `default`.
      - Daher heißt Ihre Subnetzressourcen-ID `/subscriptions/a1a1-1a11a/resourceGroups/SQLVM-RG/providers/Microsoft.Network/virtualNetworks/SQLVMvNet/subnets/default`.


Der Verfügbarkeitsgruppenlistener wird mithilfe des folgenden Codeausschnitts erstellt:

```azurecli-interactive
# Create the availability group listener
# example: az sql vm group ag-listener create -n AGListener -g SQLVM-RG `
#  --ag-name SQLAG --group-name Cluster --ip-address 10.0.0.27 `
#  --load-balancer sqlilb --probe-port 59999  `
#  --subnet /subscriptions/a1a1-1a11a/resourceGroups/SQLVM-RG/providers/Microsoft.Network/virtualNetworks/SQLVMvNet/subnets/default `
#  --sqlvms sqlvm1 sqlvm2

az sql vm group ag-listener create -n <listener name> -g <resource group name> `
  --ag-name <availability group name> --group-name <cluster name> --ip-address <ag listener IP address> `
  --load-balancer <lbname> --probe-port <Load Balancer probe port, default 59999>  `
  --subnet <subnet resource id> `
  --sqlvms <names of SQL VM's hosting AG replicas, ex: sqlvm1 sqlvm2>
```

## <a name="modify-the-number-of-replicas-in-an-availability-group"></a>Ändern der Anzahl von Replikaten in einer Verfügbarkeitsgruppe
Beim Bereitstellen einer Verfügbarkeitsgruppe auf SQL Server-VMs, die in Azure gehostet sind, muss zusätzliche Komplexität berücksichtigt werden. Der Ressourcenanbieter und die Gruppe der virtuellen Computer verwalten nun die Ressourcen. Beim Hinzufügen oder Entfernen von Replikaten zur bzw. aus der Verfügbarkeitsgruppe ist daher das Aktualisieren der Metadaten des Listeners mit Informationen zu den SQL Server-VMs als zusätzlicher Schritt auszuführen. Beim Ändern der Anzahl von Replikaten in der Verfügbarkeitsgruppe muss außerdem der Befehl [az sql vm group ag-listener update](/cli/azure/sql/vm/group/ag-listener?view=azure-cli-2018-03-01-hybrid#az-sql-vm-group-ag-listener-update) verwendet werden, um den Listener mit den Metadaten der SQL Server-VMs zu aktualisieren. 


### <a name="add-a-replica"></a>Hinzufügen eines Replikats

So fügen Sie der Verfügbarkeitsgruppe ein neues Replikat hinzu:

1. Fügen Sie dem Cluster die SQL Server-VM hinzu:
   ```azurecli-interactive
   # Add the SQL Server VM to the cluster
   # example: az sql vm add-to-group -n SQLVM3 -g SQLVM-RG --sqlvm-group Cluster `
   # -b Str0ngAzur3P@ssword! -p Str0ngAzur3P@ssword! -s Str0ngAzur3P@ssword!

   az sql vm add-to-group -n <VM3 Name> -g <Resource Group Name> --sqlvm-group <cluster name> `
   -b <bootstrap account password> -p <operator account password> -s <service account password>
   ```
1. Verwenden Sie SQL Server Management Studio, um die SQL Server-Instanz innerhalb der Verfügbarkeitsgruppe als Replikat hinzuzufügen.
1. Fügen Sie dem Listener die SQL Server-VM-Metadaten hinzu:
   ```azurecli-interactive
   # Update the listener metadata with the new VM
   # example: az sql vm group ag-listener update -n AGListener `
   # -g sqlvm-rg --group-name Cluster --sqlvms sqlvm1 sqlvm2 sqlvm3

   az sql vm group ag-listener update -n <Listener> `
   -g <RG name> --group-name <cluster name> --sqlvms <SQL VMs, along with new SQL VM>
   ```

### <a name="remove-a-replica"></a>Entfernung eines Replikats

So entfernen Sie ein Replikat aus der Verfügbarkeitsgruppe:

1. Entfernen Sie das Replikat mithilfe von SQL Server Management Studio aus der Verfügbarkeitsgruppe. 
1. Entfernen Sie die SQL Server-VM-Metadaten aus dem Listener:
   ```azurecli-interactive
   # Update the listener metadata by removing the VM from the SQLVMs list
   # example: az sql vm group ag-listener update -n AGListener `
   # -g sqlvm-rg --group-name Cluster --sqlvms sqlvm1 sqlvm2

   az sql vm group ag-listener update -n <Listener> `
   -g <RG name> --group-name <cluster name> --sqlvms <SQL VMs that remain>
   ```
1. Entfernen Sie die SQL Server-VM aus dem Cluster:
   ```azurecli-interactive
   # Remove the SQL VM from the cluster
   # example: az sql vm remove-from-group --name SQLVM3 --resource-group SQLVM-RG

   az sql vm remove-from-group --name <SQL VM name> --resource-group <RG name> 
   ```

## <a name="remove-the-availability-group-listener"></a>Entfernen des Verfügbarkeitsgruppenlisteners
Wenn Sie den mit der Azure CLI konfigurierten Verfügbarkeitsgruppenlistener wieder entfernen möchten, müssen Sie dies über den SQL-VM-Ressourcenanbieter tun. Da der Listener über den SQL-VM-Ressourcenanbieter registriert wurde, reicht das Löschen über SQL Server Management Studio nicht aus. 

Die beste Methode besteht darin, ihn über den SQL-VM-Ressourcenanbieter zu löschen, indem Sie in der Azure CLI den folgenden Codeausschnitt verwenden. Dadurch werden die Metadaten des Verfügbarkeitsgruppenlisteners aus dem SQL-VM-Ressourcenanbieter entfernt. Außerdem wird der Listener physisch aus der Verfügbarkeitsgruppe gelöscht. 

```azurecli-interactive
# Remove the availability group listener
# example: az sql vm group ag-listener delete --group-name Cluster --name AGListener --resource-group SQLVM-RG

az sql vm group ag-listener delete --group-name <cluster name> --name <listener name > --resource-group <resource group name>
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Artikeln: 

* [Was ist SQL Server auf virtuellen Azure-Computern? (Windows)](virtual-machines-windows-sql-server-iaas-overview.md)
* [Häufig gestellte Fragen zu SQL Server auf virtuellen Windows-Computern in Azure](virtual-machines-windows-sql-server-iaas-faq.md)
* [SQL Server auf Azure-VMs – Versionshinweise](virtual-machines-windows-sql-server-iaas-release-notes.md)
* [Ändern des Lizenzierungsmodells für eine SQL Server-VM in Azure](virtual-machines-windows-sql-ahb.md)
* [Übersicht über Always On-Verfügbarkeitsgruppen (SQL Server)](/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server)   
* [Aktivieren des Features für Always On-Verfügbarkeitsgruppen für eine SQL Server-Instanz](/sql/database-engine/availability-groups/windows/configuration-of-a-server-instance-for-always-on-availability-groups-sql-server)   
* [Verwalten einer Verfügbarkeitsgruppe](/sql/database-engine/availability-groups/windows/administration-of-an-availability-group-sql-server)   
* [Tools zum Überwachen von Always On-Verfügbarkeitsgruppen](/sql/database-engine/availability-groups/windows/monitoring-of-availability-groups-sql-server)
* [Transact-SQL-Anweisungen für Always On-Verfügbarkeitsgruppen](/sql/database-engine/availability-groups/windows/transact-sql-statements-for-always-on-availability-groups)   
* [Übersicht über PowerShell-Cmdlets für Always On-Verfügbarkeitsgruppen](/sql/database-engine/availability-groups/windows/overview-of-powershell-cmdlets-for-always-on-availability-groups-sql-server)  
