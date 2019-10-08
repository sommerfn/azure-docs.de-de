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
ms.openlocfilehash: ee8a711a867f8abdc831b0d1d9d0b504b1104955
ms.sourcegitcommit: 0486aba120c284157dfebbdaf6e23e038c8a5a15
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2019
ms.locfileid: "71310129"
---
# <a name="creating-an-incremental-snapshot-preview-for-managed-disks"></a>Erstellen einer inkrementellen Momentaufnahme (Vorschau) für verwaltete Datenträger

Inkrementelle Momentaufnahmen (Vorschau) sind Sicherungen zu einem bestimmten Zeitpunkt für verwaltete Datenträger, die im Erfassungszustand nur aus allen Änderungen seit der letzten Momentaufnahme bestehen. Wenn Sie versuchen, eine inkrementelle Momentaufnahme herunterzuladen oder anderweitig zu verwenden, wird die vollständige VHD verwendet. Diese neue Funktion für Momentaufnahmen verwalteter Datenträger kann es potenziell ermöglichen, dass diese kostengünstiger sind, da Sie nicht mehr den gesamten Datenträger mit jeder einzelnen Momentaufnahme speichern müssen, es sei denn, Sie entscheiden sich dazu. Ebenso wie reguläre Momentaufnahmen können inkrementelle Momentaufnahmen verwendet werden, um einen vollständigen verwalteten Datenträger oder eine reguläre Momentaufnahme zu erstellen.

Es gibt einige Unterschiede zwischen einer inkrementellen Momentaufnahme und einer regulären Momentaufnahme. Inkrementelle Momentaufnahmen verwenden immer Festplattenlaufwerk-Standardspeicher, und zwar unabhängig vom Speichertyp des Datenträgers, wohingegen reguläre Momentaufnahmen SSD Premium verwenden können. Wenn Sie reguläre Momentaufnahmen auf Storage Premium zum zentralen Hochskalieren von VM-Bereitstellungen verwenden, empfiehlt es sich, benutzerdefinierte Images im Standardspeicher in [Shared Image Gallery](../articles/virtual-machines/linux/shared-image-galleries.md) zu verwenden. Dies unterstützt Sie dabei, eine größere Skalierung mit geringeren Kosten zu erzielen. Außerdem bieten inkrementelle Momentaufnahmen möglicherweise bessere Zuverlässigkeit durch [zonenredundanten Speicher](../articles/storage/common/storage-redundancy-zrs.md) (ZRS). Wenn ZRS in der ausgewählten Region verfügbar ist, verwendet eine inkrementelle Momentaufnahme automatisch ZRS. Wenn ZRS nicht in der Region verfügbar ist, wird für die Momentaufnahme standardmäßig [lokal redundanter Speicher](../articles/storage/common/storage-redundancy-lrs.md) (LRS) verwendet. Sie können dieses Verhalten außer Kraft setzen und manuell ein anderes Verhalten auswählen. Dies wird jedoch nicht empfohlen.

Inkrementelle Momentaufnahmen bieten außerdem eine differenzielle Funktion, die für verwaltete Datenträger exklusiv verfügbar ist. Sie ermöglichen es Ihnen, die Änderungen zwischen zwei inkrementellen Momentaufnahmen der gleichen verwalteten Datenträger bis hinunter zur Blockebene abzurufen. Sie können diese Funktion verwenden, um den Datenbedarf beim regionsübergreifenden Kopieren von Momentaufnahmen zu verringern.

Wenn Sie sich noch nicht für die Vorschau registriert haben und mit der Verwendung von inkrementellen Momentaufnahmen beginnen möchten, senden Sie eine E-Mail an AzureDisks@microsoft.com, um Zugriff auf die öffentliche Vorschau zu erhalten.

## <a name="restrictions"></a>Einschränkungen

- Inkrementelle Momentaufnahmen sind zurzeit nur in der Region „USA, Westen-Mitte“ verfügbar.
- Inkrementelle Momentaufnahmen können derzeit nicht erstellt werden, nachdem die Größe eines Datenträgers geändert wurde.
- Inkrementelle Momentaufnahmen können derzeit nicht zwischen Abonnements verschoben werden.
- Sie können zurzeit nur SAS-URIs mit jeweils bis zu fünf Momentaufnahmen einer bestimmten Momentaufnahmefamilie generieren.
- Sie können keine inkrementelle Momentaufnahme für einen bestimmten Datenträger außerhalb des Abonnements dieses Datenträgers erstellen.
- Bis zu sieben inkrementelle Momentaufnahmen pro Datenträger können alle fünf Minuten erstellt werden.
- Für einen einzelnen Datenträger können insgesamt 200 inkrementelle Momentaufnahmen erstellt werden.

## <a name="powershell"></a>PowerShell

Sie können Azure PowerShell verwenden, um eine inkrementelle Momentaufnahme zu erstellen. Sie benötigen die aktuelle Version von Azure PowerShell. Mit dem folgenden Befehl wird sie installiert oder Ihre vorhandene Installation auf die neueste Version aktualisiert:

```PowerShell
Install-Module -Name Az -AllowClobber -Scope CurrentUser
```

Melden Sie sich nach der Installation bei der PowerShell-Sitzung mit `az login` an.

Um eine inkrementelle Momentaufnahme mit Azure PowerShell zu erstellen, legen Sie die Konfiguration mit [New-AzSnapShotConfig](https://docs.microsoft.com/en-us/powershell/module/az.compute/new-azsnapshotconfig?view=azps-2.7.0) mit dem Parameter `-Incremental` fest und übergeben diese dann als Variable über den Parameter `-Snapshot` an [New-AzSnapshot](https://docs.microsoft.com/en-us/powershell/module/az.compute/new-azsnapshot?view=azps-2.7.0).

Ersetzen Sie `<yourDiskNameHere>`, `<yourResourceGroupNameHere>` und `<yourDesiredSnapShotNameHere>` durch Ihre Werte. Anschließend können Sie das folgende Skript verwenden, um eine inkrementelle Momentaufnahme zu erstellen:

```PowerShell
# Get the disk that you need to backup by creating an incremental snapshot
$yourDisk = Get-AzDisk -DiskName <yourDiskNameHere> -ResourceGroupName <yourResourceGroupNameHere>

# Create an incremental snapshot by setting the SourceUri property with the value of the Id property of the disk
$snapshotConfig=New-AzSnapshotConfig -SourceUri $yourDisk.Id -Location $yourDisk.Location -CreateOption Copy -Incremental 
New-AzSnapshot -ResourceGroupName <yourResourceGroupNameHere> -SnapshotName <yourDesiredSnapshotNameHere> -Snapshot $snapshotConfig 
```

Sie können inkrementelle Momentaufnahmen auf dem gleichen Datenträger mit den Eigenschaften `SourceResourceId` und `SourceUniqueId` der Momentaufnahmen identifizieren. `SourceResourceId` ist die Azure Resource Manager-Ressourcen-ID des übergeordneten Datenträgers. `SourceUniqueId` ist der Wert, der von der `UniqueId`-Eigenschaft des Datenträgers geerbt wird. Wenn Sie einen Datenträger löschen und dann einen Datenträger mit demselben Namen erstellen, ändert sich der Wert der `UniqueId`-Eigenschaft.

Sie können `SourceResourceId` und `SourceUniqueId` verwenden, um eine Liste aller Momentaufnahmen zu erstellen, die einem bestimmten Datenträger zugeordnet sind. Ersetzen Sie `<yourResourceGroupNameHere>` durch Ihren Wert. Anschließend können Sie das folgende Beispiel verwenden, um Ihre vorhandenen inkrementellen Momentaufnahmen aufzulisten:

```PowerShell
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

## <a name="cli"></a>Befehlszeilenschnittstelle (CLI)

Sie können eine inkrementelle Momentaufnahme mit der Azure CLI erstellen. Sie benötigen dazu die aktuelle Version der Azure CLI. Mit dem folgenden Befehl wird sie installiert oder Ihre vorhandene Installation auf die neueste Version aktualisiert:

```PowerShell
Invoke-WebRequest -Uri https://aka.ms/installazurecliwindows -OutFile .\AzureCLI.msi; Start-Process msiexec.exe -Wait -ArgumentList '/I AzureCLI.msi /quiet'
```

Um eine inkrementelle Momentaufnahme zu erstellen, verwenden Sie [az snapshot create](https://docs.microsoft.com/cli/azure/snapshot?view=azure-cli-latest#az-snapshot-create) mit dem Parameter `--incremental`.

Das folgende Beispiel erstellt eine inkrementelle Momentaufnahme, ersetzt `<yourDesiredSnapShotNameHere>`, `<yourResourceGroupNameHere>`, `<exampleDiskName>` und `<exampleLocation>` durch Ihre eigenen Werte und führt das Beispiel dann aus:

```bash
sourceResourceId=$(az disk show -g <yourResourceGroupNameHere> -n <exampleDiskName> --query '[id]' -o tsv)

az snapshot create -g <yourResourceGroupNameHere> \
-n <yourDesiredSnapShotNameHere> \
-l <exampleLocation> \
--source "$sourceResourceId" \
--incremental
```

Sie können inkrementelle Momentaufnahmen auf dem gleichen Datenträger mit den Eigenschaften `SourceResourceId` und `SourceUniqueId` der Momentaufnahmen identifizieren. `SourceResourceId` ist die Azure Resource Manager-Ressourcen-ID des übergeordneten Datenträgers. `SourceUniqueId` ist der Wert, der von der `UniqueId`-Eigenschaft des Datenträgers geerbt wird. Wenn Sie einen Datenträger löschen und dann einen Datenträger mit demselben Namen erstellen, ändert sich der Wert der `UniqueId`-Eigenschaft.

Sie können `SourceResourceId` und `SourceUniqueId` verwenden, um eine Liste aller Momentaufnahmen zu erstellen, die einem bestimmten Datenträger zugeordnet sind. Im folgenden Beispiel werden alle inkrementellen Momentaufnahmen aufgelistet, die einem bestimmten Datenträger zugeordnet sind. Dafür ist jedoch ein Setup erforderlich.

In diesem Beispiel wird jq zum Abfragen der Daten verwendet. Um das Beispiel auszuführen, müssen Sie [jq installieren](https://stedolan.github.io/jq/download/).

Ersetzen Sie `<yourResourceGroupNameHere>` und `<exampleDiskName>` durch Ihren Wert. Anschließend können Sie das folgende Beispiel verwenden, um Ihre vorhandenen inkrementellen Momentaufnahmen aufzulisten, wenn jq installiert ist:

```bash
sourceUniqueId=$(az disk show -g <yourResourceGroupNameHere> -n <exampleDiskName> --query '[uniqueId]' -o tsv)

 
sourceResourceId=$(az disk show -g <yourResourceGroupNameHere> -n <exampleDiskName> --query '[id]' -o tsv)

az snapshot list -g <yourResourceGroupNameHere> -o json \
| jq -cr --arg SUID "$sourceUniqueId" --arg SRID "$sourceResourceId" '.[] | select(.incremental==true and .creationData.sourceUniqueId==$SUID and .creationData.sourceResourceId==$SRID)'
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

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie sich noch nicht für die Vorschau registriert haben und mit der Verwendung von inkrementellen Momentaufnahmen beginnen möchten, senden Sie eine E-Mail an AzureDisks@microsoft.com, um Zugriff auf die öffentliche Vorschau zu erhalten.
