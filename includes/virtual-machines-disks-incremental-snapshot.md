---
title: include file
description: include file
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 09/23/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: e39f294f7902eabef401d4c8145f4f19a07f267f
ms.sourcegitcommit: 3fa4384af35c64f6674f40e0d4128e1274083487
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2019
ms.locfileid: "71224579"
---
# <a name="creating-an-incremental-snapshot-preview-for-managed-disks"></a>Erstellen einer inkrementellen Momentaufnahme (Vorschau) für verwaltete Datenträger

Inkrementelle Momentaufnahmen (Vorschau) sind Sicherungen zu einem bestimmten Zeitpunkt für verwaltete Datenträger, die im Erfassungszustand nur aus allen Änderungen seit der letzten Momentaufnahme bestehen. Wenn Sie versuchen, eine inkrementelle Momentaufnahme herunterzuladen oder anderweitig zu verwenden, wird die vollständige VHD verwendet. Diese neue Funktion für Momentaufnahmen verwalteter Datenträger kann es potenziell ermöglichen, dass diese kostengünstiger sind, da Sie nicht mehr den gesamten Datenträger mit jeder einzelnen Momentaufnahme speichern müssen, es sei denn, Sie entscheiden sich dazu. Ebenso wie reguläre Momentaufnahmen können inkrementelle Momentaufnahmen verwendet werden, um einen vollständigen verwalteten Datenträger oder eine reguläre Momentaufnahme zu erstellen.

Es gibt einige Unterschiede zwischen einer inkrementellen Momentaufnahme und einer regulären Momentaufnahme. Inkrementelle Momentaufnahmen verwenden immer Festplattenlaufwerk-Standardspeicher, und zwar unabhängig vom Speichertyp des Datenträgers, wohingegen reguläre Momentaufnahmen SSD Premium verwenden können. Wenn Sie reguläre Momentaufnahmen auf Storage Premium zum zentralen Hochskalieren von VM-Bereitstellungen verwenden, empfiehlt es sich, benutzerdefinierte Images im Standardspeicher in [Shared Image Gallery](../articles/virtual-machines/linux/shared-image-galleries.md) zu verwenden. Dies unterstützt Sie dabei, eine größere Skalierung mit geringeren Kosten zu erzielen. Außerdem bieten inkrementelle Momentaufnahmen möglicherweise bessere Zuverlässigkeit durch [zonenredundanten Speicher](../articles/storage/common/storage-redundancy-zrs.md) (ZRS). Wenn ZRS in der ausgewählten Region verfügbar ist, verwendet eine inkrementelle Momentaufnahme automatisch ZRS. Wenn ZRS nicht in der Region verfügbar ist, wird für die Momentaufnahme standardmäßig [lokal redundanter Speicher](../articles/storage/common/storage-redundancy-lrs.md) (LRS) verwendet. Sie können dieses Verhalten außer Kraft setzen und manuell ein anderes Verhalten auswählen. Dies wird jedoch nicht empfohlen.

Inkrementelle Momentaufnahmen bieten außerdem eine differenzielle Funktion, die für verwaltete Datenträger exklusiv verfügbar ist. Sie ermöglichen es Ihnen, die Änderungen zwischen zwei inkrementellen Momentaufnahmen der gleichen verwalteten Datenträger bis hinunter zur Blockebene abzurufen. Sie können diese Funktion verwenden, um den Datenbedarf beim regionsübergreifenden Kopieren von Momentaufnahmen zu verringern.

Wenn Sie sich noch nicht für die Vorschau registriert haben und mit der Verwendung von inkrementellen Momentaufnahmen beginnen möchten, senden Sie eine E-Mail an AzureDisks@microsoft.com, um Zugriff auf die öffentliche Vorschau zu erhalten.

## <a name="restrictions"></a>Einschränkungen

- Inkrementelle Momentaufnahmen können derzeit nicht erstellt werden, nachdem die Größe eines Datenträgers geändert wurde.
- Inkrementelle Momentaufnahmen können derzeit nicht zwischen Abonnements verschoben werden.
- Sie können zurzeit nur SAS-URIs mit jeweils bis zu fünf Momentaufnahmen einer bestimmten Momentaufnahmefamilie generieren.
- Sie können keine inkrementelle Momentaufnahme für einen bestimmten Datenträger außerhalb des Abonnements dieses Datenträgers erstellen.
- Bis zu sieben inkrementelle Momentaufnahmen pro Datenträger können alle fünf Minuten erstellt werden.
- Für einen einzelnen Datenträger können insgesamt 200 inkrementelle Momentaufnahmen erstellt werden.

## <a name="powershell"></a>PowerShell

Sie können Azure PowerShell verwenden, um eine inkrementelle Momentaufnahme zu erstellen. Sie können die neueste Version von PowerShell lokal installieren. Sie benötigen die aktuelle Version von Azure PowerShell. Mit dem folgenden Befehl wird sie installiert oder Ihre vorhandene Installation auf die neueste Version aktualisiert:

```PowerShell
Install-Module -Name Az -AllowClobber -Scope CurrentUser
```

Melden Sie sich nach der Installation bei der PowerShell-Sitzung mit `az login` an.

Ersetzen Sie `<yourDiskNameHere>`, `<yourResourceGroupNameHere>` und `<yourDesiredSnapShotNameHere>` durch Ihre Werte. Anschließend können Sie das folgende Skript verwenden, um eine inkrementelle Momentaufnahme zu erstellen:

```PowerShell
# Get the disk that you need to backup by creating an incremental snapshot
$yourDisk = Get-AzDisk -DiskName <yourDiskNameHere> -ResourceGroupName <yourResourceGroupNameHere>

# Create an incremental snapshot by setting:
# 1. Incremental property
# 2. SourceUri property with the value of the Id property of the disk
$snapshotConfig=New-AzSnapshotConfig -SourceUri $yourDisk.Id -Location $yourDisk.Location -CreateOption Copy -Incremental 
New-AzSnapshot -ResourceGroupName <yourResourceGroupNameHere> -SnapshotName <yourDesiredSnapshotNameHere> -Snapshot $snapshotConfig 

# You can identify incremental snapshots of the same disk by using the SourceResourceId and SourceUniqueId properties of snapshots. 
# SourceResourceId is the Azure Resource Manager resource ID of the parent disk. 
# SourceUniqueId is the value inherited from the UniqueId property of the disk. If you delete a disk and then create a disk with the same name, the value of the UniqueId property will change. 
# Following script shows how to get all the incremental snapshots in a resource group of same disk
$snapshots = Get-AzSnapshot -ResourceGroupName <yourResourceGroupNameHere>

$incrementalSnapshots = New-Object System.Collections.ArrayList
foreach ($snapshot in $snapshots)
{
    
    if($snapshot.Incremental -and $snapshot.CreationData.SourceResourceId -eq $yourDisk.Id -and $snapshot.CreationData.SourceUniqueId -eq $yourDisk.UniqueId){

        $incrementalSnapshots.Add($snapshot)
    }
}

$incrementalSnapshots
```

## <a name="resource-manager-template"></a>Resource Manager-Vorlage

Sie können auch Azure Resource Manager-Vorlagen verwenden, um eine inkrementelle Momentaufnahme zu erstellen. Sie müssen sicherstellen, dass die apiVersion auf **2019-03-01** festgelegt ist. Die inkrementelle Eigenschaft muss ebenfalls auf TRUE festgelegt sein. Der folgende Codeausschnitt ist ein Beispiel dafür, wie Sie eine inkrementelle Momentaufnahme mit Resource Manager-Vorlagen erstellen:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "diskName": {
      "type": "string",
      "defaultValue": "contosodisk1"
    },
  "diskResourceId": {
    "defaultValue": "<your_managed_disk_resource_ID>",
    "type": "String"
  }
  }, 
  "resources": [
  {
    "type": "Microsoft.Compute/snapshots",
    "name": "[concat( parameters('diskName'),'_snapshot1')]",
    "location": "[resourceGroup().location]",
    "apiVersion": "2019-03-01",
    "properties": {
      "creationData": {
        "createOption": "Copy",
        "sourceResourceId": "[parameters('diskResourceId')]"
      },
      "incremental": true
    }
  }
  ]
}
```

## <a name="cli"></a>Befehlszeilenschnittstelle (CLI)

Sie können eine inkrementelle Momentaufnahme mit der Azure CLI erstellen, indem Sie [az snapshot create](https://docs.microsoft.com/cli/azure/snapshot?view=azure-cli-latest#az-snapshot-create) verwenden. Ein Beispielbefehl würde wie folgt aussehen:

```bash
az snapshot create -g <exampleResourceGroup> \
-n <exampleSnapshotName> \
-l <exampleLocation> \
--source <exampleVMId> \
--incremental
```

Mithilfe des Parameters `--query` für [az snapshot show](https://docs.microsoft.com/cli/azure/snapshot?view=azure-cli-latest#az-snapshot-show) können Sie auch ermitteln, welche Momentaufnahmen inkrementelle Momentaufnahmen in der CLI sind. Sie können diesen Parameter verwenden, um die Eigenschaften **SourceResourceId** und **SourceUniqueId** von Momentaufnahmen direkt abzufragen. SourceResourceId ist die Azure Resource Manager-Ressourcen-ID des übergeordneten Datenträgers. **SourceUniqueId** ist der Wert, der von der **UniqueId**-Eigenschaft des Datenträgers geerbt wird. Wenn Sie einen Datenträger löschen und dann einen Datenträger mit demselben Namen erstellen, ändert sich der Wert der Eigenschaft **UniqueId**.

Beispiele für beide Abfragen würden wie folgt aussehen:

```bash
az snapshot show -g <exampleResourceGroup> \
-n <yourSnapShotName> \
--query [creationData.sourceResourceId] -o tsv

az snapshot show -g <exampleResourceGroup> \
-n <yourSnapShotName> \
--query [creationData.sourceUniqueId] -o tsv
```

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie sich noch nicht für die Vorschau registriert haben und mit der Verwendung von inkrementellen Momentaufnahmen beginnen möchten, senden Sie eine E-Mail an AzureDisks@microsoft.com, um Zugriff auf die öffentliche Vorschau zu erhalten.
