---
title: Einbinden eines virtuellen Dateisystems in einen Pool – Azure Batch | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie ein virtuelles Dateisystem in einen Batch-Pool einbinden.
services: batch
documentationcenter: ''
author: laurenhughes
manager: gwallace
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/13/2019
ms.author: lahugh
ms.openlocfilehash: 3bbf8cf126c80b9111f2bcbe24353c2731995bc1
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/15/2019
ms.locfileid: "69037282"
---
# <a name="mount-a-virtual-file-system-on-a-batch-pool"></a>Einbinden eines virtuellen Dateisystems in einen Batch-Pool

Azure Batch unterstützt jetzt die Einbindung von Cloudspeicher oder eines externen Dateisystems auf Windows- oder Linux-Computeknoten in Ihren Batch-Pools. Wenn ein Computeknoten einem Pool beitritt, wird das virtuelle Dateisystem eingebunden und als lokales Laufwerk auf diesem Knoten behandelt. Sie können Dateisysteme wie Azure Files, Azure Blob Storage, Network File System (NFS) einschließlich eines [Avere vFXT-Caches](../avere-vfxt/avere-vfxt-overview.md) oder Common Internet File System (CIFS) einbinden.

In diesem Artikel erfahren Sie, wie Sie ein virtuelles Dateisystem mithilfe der [Azure Batch-Verwaltungsbibliothek für .NET](https://docs.microsoft.com/dotnet/api/overview/azure/batch?view=azure-dotnet) in einen Pool von Computeknoten einbinden.

> [!NOTE]
> Das Einbinden eines virtuellen Dateisystems wird in Batch-Pools unterstützt, die am oder nach dem 19.08.2019 erstellt wurden. Von vor dem 19.08.2019 erstellten Batch-Pools wird diese Funktion nicht unterstützt.
> 
> Die APIs für die Einbindung von Dateisystemen auf einem Computeknoten sind Teil der [Batch .NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch?view=azure-dotnet)-Bibliothek.

## <a name="benefits-of-mounting-on-a-pool"></a>Vorteile der Einbindung in einen Pool

Wenn Sie das Dateisystem in den Pool einbinden, statt Vorgänge eigene Daten aus einem großen Dataset abrufen zu lassen, können Aufgaben einfacher und effizienter auf die erforderlichen Daten zugreifen.

Stellen Sie sich ein Szenario vor, in dem mehrere Aufgaben auf einen gemeinsamen Satz von Daten zugreifen müssen. Ein Beispiel hierfür ist das Rendern eines Films. Jede Aufgabe rendert jeweils ein oder mehrere Frames aus den Szenendateien. Durch die Einbindung eines Laufwerks, das die Szenendateien enthält, können Computeknoten einfacher auf freigegebene Daten zugreifen. Darüber hinaus kann das zugrunde liegende Dateisystem basierend auf der Leistung und Skalierung (Durchsatz und IOPS) unabhängig ausgewählt und skaliert werden, die für die Anzahl von Computeknoten erforderlich ist, die gleichzeitig auf die Daten zugreifen. Beispielsweise kann ein verteilter speicherinterner [Avere vFXT](../avere-vfxt/avere-vfxt-overview.md)-Cache verwendet werden, um große Renderings im Kinofilmmaßstab mit Tausenden von gleichzeitigen Renderknoten zu unterstützen, die auf lokal gespeicherte Quelldaten zugreifen. Für Daten, die sich bereits in einem cloudbasierten Blobspeicher befinden, kann alternativ [blobfuse](../storage/blobs/storage-how-to-mount-container-linux.md) verwendet werden, um diese Daten als lokales Dateisystem einzubinden. Blobfuse ist nur auf Linux-Knoten verfügbar, [Azure Files](https://azure.microsoft.com/blog/a-new-era-for-azure-files-bigger-faster-better/) bietet jedoch einen ähnlichen Workflow und ist sowohl unter Windows als auch unter Linux verfügbar.

## <a name="mount-a-virtual-file-system-on-a-pool"></a>Einbinden eines virtuellen Dateisystems in einen Pool  

Durch die Einbindung eines virtuellen Dateisystems in einen Pool wird das Dateisystem für jeden Computeknoten im Pool verfügbar. Das Dateisystem wird entweder konfiguriert, wenn ein Computeknoten einem Pool beitritt (hinzugefügt wird) oder wenn der Knoten neu gestartet oder ein Reimaging durchgeführt wird.

Um ein Dateisystem in einen Pool einzubinden, erstellen Sie ein `MountConfiguration`-Objekt. Wählen Sie das Objekt aus, das Ihrem virtuellen Dateisystem entspricht: `AzureBlobFileSystemConfiguration`, `AzureFileShareConfiguration`, `NfsMountConfiguration` oder `CifsMountConfiguration`.

Für alle MountConfiguration-Objekte sind die folgenden Basisparameter erforderlich. Einige Einbindungskonfigurationen weisen Parameter auf, die für das verwendete Dateisystem spezifisch sind. Diese werden in den Codebeispielen ausführlicher erläutert.

- **Kontoname oder Kontoquelle**: Zum Einbinden einer virtuellen Dateifreigabe benötigen Sie den Namen des Speicherkontos oder die entsprechende Quelle.
- **Relativer Einbindungspfad oder Quelle**: Der Speicherort des auf dem Computeknoten eingebundenen Dateisystems, relativ zu dem `fsmounts`-Standardverzeichnis, auf das auf dem Knoten über `AZ_BATCH_NODE_MOUNTS_DIR` zugegriffen werden kann. Der genaue Speicherort hängt von dem auf dem Knoten verwendeten Betriebssystem ab. Beispielsweise ist der physische Speicherort auf einem Ubuntu-Knoten `mnt\batch\tasks\fsmounts` zugeordnet, und auf einem CentOS-Knoten ist er `mnt\resources\batch\tasks\fsmounts` zugeordnet.
- **Einbindungsoptionen oder blobfuse-Optionen**: Diese Optionen beschreiben bestimmte Parameter für die Einbindung eines Dateisystems.

Nachdem das `MountConfiguration`-Objekt erstellt wurde, können Sie dem Objekt die `MountConfigurationList`-Eigenschaft zuweisen, wenn Sie den Pool erstellen. Das Dateisystem wird entweder eingebunden, wenn ein Knoten einem Pool beitritt (hinzugefügt wird) oder wenn der Knoten neu gestartet oder ein Reimaging durchgeführt wird.

Beim Einbinden des Dateisystems wird eine Umgebungsvariable vom Typ `AZ_BATCH_NODE_MOUNTS_DIR` erstellt, die auf den Speicherort der eingebundenen Dateisysteme sowie auf Protokolldateien verweist, die für die Problembehandlung und das Debuggen hilfreich sind. Protokolldateien werden im Abschnitt [Diagnostizieren von Einbindungsfehlern](#diagnose-mount-errors) ausführlicher erläutert.  

> [!IMPORTANT]
> Die maximale Anzahl eingebundener Dateisysteme in einem Pool beträgt 10. Ausführliche Informationen und andere Grenzwerte finden Sie unter [Batch-Dienst – Kontingente und Limits](batch-quota-limit.md#other-limits).

## <a name="examples"></a>Beispiele

In den folgenden Codebeispielen wird das Einbinden verschiedener Dateifreigaben in einen Pool von Computeknoten veranschaulicht.

### <a name="azure-files-share"></a>Azure Files-Freigabe

Azure Files ist das Standardangebot für Azure-Clouddateisysteme. Weitere Informationen zum Abrufen eines der Parameter im Codebeispiel für die Einbindungskonfiguration finden Sie unter [Verwenden einer Azure Files-Freigabe](../storage/files/storage-how-to-use-files-windows.md).

```csharp
new PoolAddParameter
{
    Id = poolId,
    MountConfiguration = new[]
    {
        new MountConfiguration
        {
            AzureFileShareConfiguration = new AzureFileShareConfiguration
            {
                AccountName = "AccountName",
                AzureFileUrl = "AzureFileShareUrl",
                AccountKey = "StorageAccountKey",
                RelativeMountPath = "RelativeMountPath",
                MountOptions = "-o vers=3.0,dir_mode=0777,file_mode=0777,sec=ntlmssp"
            },
        }
    }
}
```

### <a name="azure-blob-file-system"></a>Azure-Blobdateisystem

Eine andere Option ist die Verwendung von Azure-Blobspeicher über [blobfuse](../storage/blobs/storage-how-to-mount-container-linux.md). Zum Einbinden eines Blobdateisystems ist ein `AccountKey` oder `SasKey` für Ihr Speicherkonto erforderlich. Weitere Informationen zum Abrufen dieser Schlüssel finden Sie unter [Anzeigen von Kontoschlüsseln](../storage/common/storage-account-manage.md#view-account-keys-and-connection-string) oder [Gewähren von eingeschränktem Zugriff auf Azure Storage-Ressourcen mithilfe von SAS (Shared Access Signature)](../storage/common/storage-dotnet-shared-access-signature-part-1.md). Weitere Informationen zur Verwendung von blobfuse finden Sie in den [häufig gestellten Fragen zur Problembehandlung](https://github.com/Azure/azure-storage-fuse/wiki/3.-Troubleshoot-FAQ) bei blobfuse. Um Standardzugriff auf das über blobfuse eingebundene Verzeichnis zu erhalten, müssen Sie die Aufgabe als **Administrator** ausführen. Blobfuse bindet das Verzeichnis im Benutzerbereich ein, und bei der Poolerstellung wird es als Stamm eingebunden. Unter Linux sind alle **Administratoraufgaben** stammbasiert. Eine Beschreibung aller Optionen für das FUSE-Modul finden Sie auf der [FUSE-Referenzseite](http://manpages.ubuntu.com/manpages/xenial/man8/mount.fuse.8.html).

Zusätzlich zum Leitfaden zur Problembehandlung stellen GitHub-Issues im blobfuse-Repository eine hilfreiche Möglichkeit dar, um sich über aktuelle blobfuse-Probleme und -Lösungen zu informieren. Weitere Informationen finden Sie auf der GitHub-Seite zu blobfuse auf der Registerkarte [„Issues“](https://github.com/Azure/azure-storage-fuse/issues).

> [!NOTE]
> Blobfuse wird unter Debian derzeit nicht unterstützt. Weitere Informationen finden Sie unter [Unterstützte SKUs](#supported-skus).

```csharp
new PoolAddParameter
{
    Id = poolId,
    MountConfiguration = new[]
    {
        new MountConfiguration
        {
            AzureBlobFileSystemConfiguration = new AzureBlobFileSystemConfiguration
            {
                AccountName = "StorageAccountName",
                ContainerName = "containerName",
                AccountKey = "StorageAccountKey",
                SasKey = "",
                RelativeMountPath = "RelativeMountPath",
                BlobfuseOptions = "-o attr_timeout=240 -o entry_timeout=240 -o negative_timeout=120 "
            },
        }
    }
}
```

### <a name="network-file-system"></a>Network File System

Network File System (NFS) kann auch auf Poolknoten eingebunden werden, sodass Azure Batch-Knoten problemlos auf herkömmliche Dateisysteme zugreifen können. Dabei kann es sich um einen einzelnen in der Cloud bereitgestellten NFS-Server oder einen lokalen NFS-Server handeln, auf den über ein virtuelles Netzwerk zugegriffen wird. Alternativ können Sie die verteilte speicherinterne [Avere vFXT](../avere-vfxt/avere-vfxt-overview.md)-Cachelösung nutzen, die eine nahtlose Verbindung mit lokalem Speicher ermöglicht, Daten bedarfsgesteuert in den Cache einliest sowie eine hohe Leistung und Skalierbarkeit cloudbasierter Computeknoten bietet.

```csharp
new PoolAddParameter
{
    Id = poolId,
    MountConfiguration = new[]
    {
        new MountConfiguration
        {
            NfsMountConfiguration = new NFSMountConfiguration
            {
                Source = "source",
                RelativeMountPath = "RelativeMountPath",
                MountOptions = "options ver=1.0"
            },
        }
    }
}
```

### <a name="common-internet-file-system"></a>Common Internet File System

Common Internet File System (CIFS) kann auch auf Poolknoten eingebunden werden, sodass Azure Batch-Knoten problemlos auf herkömmliche Dateisysteme zugreifen können. CIFS ist ein Dateifreigabeprotokoll, das einen offenen und plattformübergreifenden Mechanismus zum Anfordern von Netzwerkserverdateien und -diensten bietet. CIFS basiert auf der erweiterten Version des Server Message Block-Protokolls (SMB) von Microsoft für die Internet- und Intranetdateifreigabe und wird zum Einbinden externer Dateisysteme auf Windows-Knoten verwendet. Weitere Informationen zu SMB finden Sie unter [Dateiserver und SMB](https://docs.microsoft.com/windows-server/storage/file-server/file-server-smb-overview).

```csharp
new PoolAddParameter
{
    Id = poolId,
    MountConfiguration = new[]
    {
        new MountConfiguration
        {
            CifsMountConfiguration = new CIFSMountConfiguration
            {
                Username = "StorageAccountName",
                RelativeMountPath = "cifsmountpoint",
                Source = "source",
                Password = "StorageAccountKey",
                MountOptions = "-o vers=3.0,dir_mode=0777,file_mode=0777,serverino"
            },
        }
    }
}
```

## <a name="diagnose-mount-errors"></a>Diagnostizieren von Einbindungsfehlern

Wenn bei einer Einbindungskonfiguration ein Fehler auftritt, führt dies zu einem Fehler des Computeknotens im Pool, und der Knoten weist dann den Status „Nicht verwendbar“ auf. Überprüfen Sie zum Diagnostizieren eines Einbindungskonfigurationsfehlers die [`ComputeNodeError`](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenodeerror)-Eigenschaft auf Details zum Fehler.

Wenn Sie die Protokolldateien für das Debuggen erhalten möchten, verwenden Sie [OutputFiles](batch-task-output-files.md), um die Protokolldateien (`*.log`) hochzuladen. Die Protokolldateien (`*.log`) enthalten Informationen zur Dateisystemeinbindung am Speicherort `AZ_BATCH_NODE_MOUNTS_DIR`. Einbindungsprotokolldateien weisen für jede Einbindung das Format `<type>-<mountDirOrDrive>.log` auf. Die zugehörige Einbindungsprotokolldatei einer `cifs`-Einbindung im Einbindungsverzeichnis `test` heißt beispielsweise `cifs-test.log`.

## <a name="supported-skus"></a>Unterstützte SKUs

| Herausgeber | Angebot | SKU | Azure Files-Freigabe | blobfuse | NFS-Einbindung | CIFS-Einbindung |
|---|---|---|---|---|---|---|
| Batch | rendering-centos73 | Rendering | :heavy_check_mark: <br>Hinweis: Kompatibel mit CentOS 7.7</br>| :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Canonical | UbuntuServer | 16.04-LTS, 18.04-LTS | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Credativ | Debian | 8, 9 | :heavy_check_mark: | :x: | :heavy_check_mark: | :heavy_check_mark: |
| microsoft-ads | linux-data-science-vm | linuxdsvm | :heavy_check_mark: <br>Hinweis: Kompatibel mit CentOS 7.4 </br> | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| microsoft-azure-batch | centos-container | 7.6 | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| microsoft-azure-batch | centos-container-rdma | 7.4 | :heavy_check_mark: <br>Hinweis: Unterstützt A_8-/9-Speicher</br> | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| microsoft-azure-batch | ubuntu-server-container | 16.04-LTS | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| microsoft-dsvm | linux-data-science-vm-ubuntu | linuxdsvmubuntu | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| OpenLogic | CentOS | 7.6 | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| OpenLogic | CentOS-HPC | 7.4, 7.3, 7.1 | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Oracle | Oracle-Linux | 7.6 | :x: | :x: | :x: | :x: |
| Windows | Windows Server | 2012, 2016, 2019 | :heavy_check_mark: | :x: | :x: | :x: |

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über das Einbinden einer Azure Files-Freigabe mit [Windows](../storage/files/storage-how-to-use-files-windows.md) oder [Linux](../storage/files/storage-how-to-use-files-linux.md).
- Erfahren Sie mehr über das Verwenden und Einbinden virtueller [blobfuse](https://github.com/Azure/azure-storage-fuse)-Dateisysteme.
- Lesen Sie die [Übersicht über Network File System](https://docs.microsoft.com/windows-server/storage/nfs/nfs-overview), um mehr über NFS und die zugehörigen Anwendungen zu erfahren.
- Lesen Sie die [Übersicht über das Microsoft-SMB-Protokoll und das CIFS-Protokoll](https://docs.microsoft.com/windows/desktop/fileio/microsoft-smb-protocol-and-cifs-protocol-overview), um mehr über CIFS zu erfahren.
