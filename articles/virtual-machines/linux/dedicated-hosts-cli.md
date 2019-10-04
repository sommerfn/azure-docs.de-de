---
title: Bereitstellen von dedizierten Azure-Hosts über die Befehlszeilenschnittstelle | Microsoft-Dokumentation
description: Stellen Sie VMs auf dedizierten Hosts über die Azure-Befehlszeilenschnittstelle bereit.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/29/2019
ms.author: cynthn
ms.openlocfilehash: 0c060e2ab94c0a57d4d4dc897702e115cfabd9a0
ms.sourcegitcommit: 3073581d81253558f89ef560ffdf71db7e0b592b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2019
ms.locfileid: "68827295"
---
# <a name="preview-deploy-vms-to-dedicated-hosts-using-the-azure-cli"></a>Vorschau: Bereitstellen von VMs auf dedizierten Hosts über die Azure-Befehlszeilenschnittstelle
 

Dieser Artikel führt Sie durch die Erstellung eines [dedizierten Azure-Hosts](dedicated-hosts.md) zum Hosten Ihrer virtuellen Computer (VMs). 

Stellen Sie sicher, dass mindestens Version 2.0.70 der Azure-Befehlszeilenschnittstelle installiert ist und Sie mit `az login` bei einem Azure-Konto angemeldet wurden. 

> [!IMPORTANT]
> Azure Dedicated Host – der Dienst für dedizierte Azure-Hosts – ist derzeit als Public Preview verfügbar.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>
> **Bekannte Einschränkungen der Vorschau**
> - VM-Skalierungsgruppen werden zurzeit auf dedizierten Hosts nicht unterstützt.
> - Das erste Vorschaurelease unterstützt die folgende VM-Serien: DSv3 und ESv3. 
 

## <a name="create-resource-group"></a>Ressourcengruppe erstellen 
Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. Erstellen Sie die Ressourcengruppe mithilfe von „az group create“. Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *myDHResourceGroup* am Standort *USA, Osten* erstellt.

```bash
az group create --name myDHResourceGroup --location eastus 
```
 
## <a name="create-a-host-group"></a>Erstellen einer Hostgruppe 

Eine **Hostgruppe** ist eine Ressource, die eine Sammlung dedizierter Hosts darstellt. Sie erstellen eine Hostgruppe in einer Region und einer Verfügbarkeitszone und fügen ihr Hosts hinzu. Bei der Planung für Hochverfügbarkeit stehen zusätzliche Optionen zur Verfügung. Sie können eine oder beide der folgenden Optionen mit Ihren dedizierten Hosts verwenden: 
- Ausdehnen über mehrere Verfügbarkeitszonen. In diesem Fall muss in jeder Zone, die Sie verwenden möchten, eine Hostgruppe vorhanden sein.
- Ausdehnen über mehrere Fehlerdomänen, die physischen Racks zugeordnet sind. 
 
In beiden Fällen müssen Sie die Anzahl der Fehlerdomänen für Ihre Hostgruppe angeben. Wenn Ihre Gruppe keine Fehlerdomänen umfassen soll, verwenden Sie die Anzahl 1 von Fehlerdomänen. 

Sie können auch sowohl Verfügbarkeitszonen als auch Fehlerdomänen verwenden. 

In diesem Beispiel verwenden wir [az vm host group create](/cli/azure/vm/host/group#az-vm-host-group-create), um eine Hostgruppe mit Verfügbarkeitszonen und Fehlerdomänen zu erstellen. 

```bash
az vm host group create \
   --name myHostGroup \
   -g myDHResourceGroup \
   -z 1 \
   --platform-fault-domain-count 2 
``` 

### <a name="other-examples"></a>Weitere Beispiele

Sie können auch [az vm host group create](/cli/azure/vm/host/group#az-vm-host-group-create) verwenden, um eine Hostgruppe in Verfügbarkeitszone 1 (und ohne Fehlerdomänen) zu erstellen.

```bash
az vm host group create \
   --name myAZHostGroup \
   -g myDHResourceGroup \
   -z 1 \
   --platform-fault-domain-count 1 
```
 
Im Folgenden wird mit [az vm host group create](/cli/azure/vm/host/group#az-vm-host-group-create) eine Hostgruppe nur mit Fehlerdomänen erstellt. (Diese kann in Regionen verwendet werden, in denen keine Verfügbarkeitszonen unterstützt werden.) 

```bash
az vm host group create \
   --name myFDHostGroup \
   -g myDHResourceGroup \
   --platform-fault-domain-count 2 
```
 
## <a name="create-a-host"></a>Erstellen eines Hosts 

Nun erstellen Sie einen dedizierten Host in der Hostgruppe. Zusätzlich zu einem Namen müssen Sie die SKU für den Host angeben. Die Host-SKU erfasst die unterstützte VM-Serie sowie die Hardwaregenerierung für Ihren dedizierten Host.  Während der Vorschau werden die folgenden Host-SKU-Werte unterstützt: „DSv3_Type1“ und „ESv3_Type1“.


Weitere Informationen zu den Host-SKUs und Preisen finden Sie unter [Azure Dedicated Host – Preise](https://aka.ms/ADHPricing).

Erstellen Sie mit [az vm host create](/cli/azure/vm/host#az-vm-host-create) einen Host. Beim Festlegen der Anzahl der Fehlerdomänen für die Hostgruppe werden Sie aufgefordert, die Fehlerdomäne für den Host anzugeben.  

```bash
az vm host create \
   --host-group myHostGroup \
   --name myHost \
   --sku DSv3-Type1 \
   --platform-fault-domain 1 \
   -g myDHResourceGroup
```


 
## <a name="create-a-virtual-machine"></a>Erstellen eines virtuellen Computers 
Erstellen Sie mithilfe von [az vm create](/cli/azure/vm#az-vm-create) einen virtuellen Computer in einem dedizierten Host. Wenn Sie beim Erstellen der Hostgruppe eine Verfügbarkeitszone angegeben haben, müssen Sie beim Erstellen des virtuellen Computers dieselbe Zone verwenden.

```bash
az vm create \
   -n myVM \
   --image debian \
   --generate-ssh-keys \
   --host-group myHostGroup \
   --host myHost \
   --generate-ssh-keys \
   --size Standard_D4s_v3 \
   -g myDHResourceGroup \
   --zone 1
```
 
> [!WARNING]
> Wenn Sie einen virtuellen Computer auf einem Host erstellen, der nicht über ausreichende Ressourcen verfügt, wird der virtuelle Computer im Zustand „Fehler“ erstellt. 


## <a name="check-the-status-of-the-host"></a>Überprüfen des Status des Hosts

Sie können mit [az vm host get-instance-view](/cli/azure/vm/host#az-vm-host-get-instance-view) den Integritätsstatus des Hosts sowie die Anzahl von virtuellen Computern überprüfen, die Sie noch auf dem Host bereitstellen können.

```bash
az vm host get-instance-view \
   -g myDHResourceGroup \
   --host-group myHostGroup \
   --name myHost
```
 Die Ausgabe sieht etwa wie folgt aus:
 
```json
{
  "autoReplaceOnFailure": true,
  "hostId": "6de80643-0f45-4e94-9a4c-c49d5c777b62",
  "id": "/subscriptions/10101010-1010-1010-1010-101010101010/resourceGroups/myDHResourceGroup/providers/Microsoft.Compute/hostGroups/myHostGroup/hosts/myHost",
  "instanceView": {
    "assetId": "12345678-1234-1234-abcd-abc123456789",
    "availableCapacity": {
      "allocatableVms": [
        {
          "count": 31.0,
          "vmSize": "Standard_D2s_v3"
        },
        {
          "count": 15.0,
          "vmSize": "Standard_D4s_v3"
        },
        {
          "count": 7.0,
          "vmSize": "Standard_D8s_v3"
        },
        {
          "count": 3.0,
          "vmSize": "Standard_D16s_v3"
        },
        {
          "count": 1.0,
          "vmSize": "Standard_D32-8s_v3"
        },
        {
          "count": 1.0,
          "vmSize": "Standard_D32-16s_v3"
        },
        {
          "count": 1.0,
          "vmSize": "Standard_D32s_v3"
        },
        {
          "count": 1.0,
          "vmSize": "Standard_D48s_v3"
        },
        {
          "count": 0.0,
          "vmSize": "Standard_D64-16s_v3"
        },
        {
          "count": 0.0,
          "vmSize": "Standard_D64-32s_v3"
        },
        {
          "count": 0.0,
          "vmSize": "Standard_D64s_v3"
        }
      ]
    },
    "statuses": [
      {
        "code": "ProvisioningState/succeeded",
        "displayStatus": "Provisioning succeeded",
        "level": "Info",
        "message": null,
        "time": "2019-07-24T21:22:40.604754+00:00"
      },
      {
        "code": "HealthState/available",
        "displayStatus": "Host available",
        "level": "Info",
        "message": null,
        "time": null
      }
    ]
  },
  "licenseType": null,
  "location": "eastus2",
  "name": "myHost",
  "platformFaultDomain": 1,
  "provisioningState": "Succeeded",
  "provisioningTime": "2019-07-24T21:22:40.604754+00:00",
  "resourceGroup": "myDHResourceGroup",
  "sku": {
    "capacity": null,
    "name": "DSv3-Type1",
    "tier": null
  },
  "tags": null,
  "type": null,
  "virtualMachines": [
    {
      "id": "/subscriptions/10101010-1010-1010-1010-101010101010/resourceGroups/MYDHRESOURCEGROUP/providers/Microsoft.Compute/virtualMachines/MYVM",
      "resourceGroup": "MYDHRESOURCEGROUP"
    }
  ]
}

```
 
## <a name="export-as-a-template"></a>Exportieren als Vorlage 
Sie können eine Vorlage exportieren, wenn Sie eine weitere Entwicklungsumgebung mit den gleichen Parametern oder eine entsprechende Produktionsumgebung erstellen möchten. Der Resource Manager verwendet die JSON-Vorlagen, die alle Parameter für Ihre Umgebung definieren. Sie erstellen ganze Umgebungen durch Verweisen auf diese JSON-Vorlage. Sie können JSON-Vorlagen manuell erstellen oder eine vorhandene Umgebung exportieren, um die JSON-Vorlage zu erstellen. Verwenden Sie [az group export](/cli/azure/group#az-group-export), um Ihre Ressourcengruppe zu exportieren.

```bash
az group export --name myDHResourceGroup > myDHResourceGroup.json 
```

Dieser Befehl erstellt die `myDHResourceGroup.json`-Datei in Ihrem aktuellen Arbeitsverzeichnis. Wenn Sie eine Umgebung anhand dieser Vorlage erstellen, werden Sie aufgefordert, alle Ressourcennamen anzugeben. Sie können diese Namen in Ihre Vorlagendatei eintragen, indem Sie den Parameter `--include-parameter-default-value` dem Befehl `az group export` hinzufügen. Bearbeiten Sie die JSON-Vorlage zum Angeben der Ressourcennamen, oder erstellen Sie eine Datei „parameters.json“, in der die Ressourcennamen angegeben werden.
 
Verwenden Sie zum Erstellen einer Umgebung aus Ihrer Vorlage [az group deployment create](/cli/azure/group/deployment#az-group-deployment-create).

```bash
az group deployment create \ 
    --resource-group myNewResourceGroup \ 
    --template-file myDHResourceGroup.json 
```


## <a name="clean-up"></a>Bereinigen 

Ihre dedizierten Hosts werden Ihnen auch dann in Rechnung gestellt, wenn keine virtuellen Computer bereitgestellt sind. Sie sollten, um Kosten zu sparen, alle Hosts löschen, die Sie zurzeit nicht verwenden.  

Ein Host kann nur gelöscht werden, wenn er nicht mehr von virtuellen Computern verwendet wird. Löschen Sie die VMs mit [az vm delete](/cli/azure/vm#az-vm-delete).

```bash
az vm delete -n myVM -g myDHResourceGroup
```

Nachdem Sie die VMs gelöscht haben, können Sie den Host mithilfe von [az vm host delete](/cli/azure/vm/host#az-vm-host-delete) löschen.

```bash
az vm host delete -g myDHResourceGroup --host-group myHostGroup --name myHost 
```
 
Wenn Sie alle Hosts gelöscht haben, können Sie die Hostgruppe mit [az vm host group delete](/cli/azure/vm/host/group#az-vm-host-group-delete) löschen.  
 
```bash
az vm host group delete -g myDHResourceGroup --host-group myHostGroup  
```
 
Sie können auch die gesamte Ressourcengruppe mit einem einzigen Befehl löschen. Dabei werden alle in der Gruppe erstellten Ressourcen gelöscht, einschließlich aller VMs, Hosts und Hostgruppen.
 
```bash
az group delete -n myDHResourceGroup 
```

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen finden Sie in der Übersicht zu [dedizierten Hosts](dedicated-hosts.md).

- Sie können dedizierte Hosts auch über das [Azure-Portal](dedicated-hosts-portal.md) erstellen.

- [Hier](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vm-dedicated-hosts/README.md) finden Sie eine Beispielvorlage, die sowohl Zonen als auch Fehlerdomänen für maximale Resilienz in einer Region verwendet.