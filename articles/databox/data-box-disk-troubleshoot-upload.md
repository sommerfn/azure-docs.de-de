---
title: Behandeln von Problemen beim Hochladen von Daten in Azure Data Box Disk mithilfe von Protokollen | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Protokolle verwenden und Probleme beim Hochladen von Daten in Azure Data Box Disk beheben.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: article
ms.date: 06/14/2019
ms.author: alkohli
ms.openlocfilehash: 6f3ac38c3eac968bd2f7ec2aada435466d3ff279
ms.sourcegitcommit: 72f1d1210980d2f75e490f879521bc73d76a17e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2019
ms.locfileid: "67148122"
---
# <a name="understand-logs-to-troubleshoot-data-upload-issues-in-azure-data-box-disk"></a>Grundlegendes zu Protokollen für die Behandlung von Problemen beim Hochladen von Daten in Azure Data Box Disk

Dieser Artikel gilt für Microsoft Azure Data Box Disk. Er beschreibt die Problemen, die beim Hochladen von Daten in Azure auftreten können.

## <a name="data-upload-logs"></a>Protokolle zum Datenupload

Beim Hochladen von Daten in Azure im Rechenzentrum werden die Dateien `_error.xml` und `_verbose.xml` generiert. Diese Protokolle werden in dasselbe Speicherkonto hochgeladen, das auch zum Hochladen der Daten verwendet wurde. Ein Beispiel für die Datei `_error.xml` wird nachfolgend gezeigt.
    
```xml
 <?xml version="1.0" encoding="utf-8"?>
    <DriveLog Version="2018-10-01">
      <DriveId>18041C582D7E</DriveId>
      <Summary>
     <!--Summary for validation and data copy to Azure -->
        <ValidationErrors>
          <None Count="8" />
        </ValidationErrors>
        <CopyErrors>
          <Completed Count="5" Description="No errors encountered" />
          <ContainerRenamed Count="3" Description="Renamed the container as the original container name does not follow Azure conventions." />
        </CopyErrors>
      </Summary>
    <!--List of renamed containers with the new names, new file path in Azure, original invalid file path, and size -->
      <Blob Status="ContainerRenamed">
        <BlobPath>databox-c2073fd1cc379d83e03d6b7acce23a6cf29d1eef/private.vhd</BlobPath>
        <OriginalFilePath>\PageBlob\pageblob test\private.vhd</OriginalFilePath>
        <SizeInBytes>10490880</SizeInBytes>
      </Blob>
      <Blob Status="ContainerRenamed">
        <BlobPath>databox-c2073fd1cc379d83e03d6b7acce23a6cf29d1eef/resource.vhd</BlobPath>
        <OriginalFilePath>\PageBlob\pageblob test\resource.vhd</OriginalFilePath>
        <SizeInBytes>71528448</SizeInBytes>
      </Blob>
      <Blob Status="ContainerRenamed">
        <BlobPath>databox-c2073fd1cc379d83e03d6b7acce23a6cf29d1eef/role.vhd</BlobPath>
        <OriginalFilePath>\PageBlob\pageblob test\role.vhd</OriginalFilePath>
        <SizeInBytes>10490880</SizeInBytes>
      </Blob>
      <Status>CompletedWithErrors</Status>
    </DriveLog>
```

## <a name="download-logs"></a>Herunterladen von Protokollen

Es gibt zwei Möglichkeiten, die Diagnoseprotokolle zu suchen und herunterzuladen.

- Wenn beim Hochladen der Daten in Azure Fehler auftreten, zeigt das Portal einen Pfad zu dem Ordner an, in dem sich die Diagnoseprotokolle befinden.

    ![Link zu Protokollen im Portal](./media/data-box-disk-troubleshoot-upload/data-box-disk-portal-logs.png)

- Navigieren Sie zu dem Speicherkonto, das Ihrem Data Box-Auftrag zugeordnet ist. Navigieren Sie zu **Blob-Dienst > Blobs durchsuchen**, und suchen Sie nach dem Blob, das dem Speicherkonto entspricht. Navigieren Sie zu **waies**.

    ![Kopieren von Protokollen 2](./media/data-box-disk-troubleshoot/data-box-disk-copy-logs2.png)

Es werden in jedem Fall die Fehlerprotokolle und die ausführlichen Protokolle angezeigt. Wählen Sie die einzelnen Protokolle aus, und laden Sie eine lokale Kopie herunter.


## <a name="data-upload-errors"></a>Fehler beim Hochladen von Daten

Die Fehler, die beim Hochladen von Daten in Azure generiert werden, sind in der folgenden Tabelle zusammengefasst.

| Fehlercode | BESCHREIBUNG                        |
|-------------|------------------------------|
|`None` |  Erfolgreich abgeschlossen.           |
|`Renamed` | Das Blob wurde erfolgreich umbenannt.  |                                                            |
|`CompletedWithErrors` | Der Upload wurde mit Fehlern abgeschlossen. Die Details der fehlerhaften Dateien sind in der Protokolldatei enthalten.  |
|`Corrupted`|Der während der Datenerfassung berechnete CRC-Wert stimmt nicht mit dem während des Uploads berechneten CRC-Wert überein.  |  
|`StorageRequestFailed` | Fehler bei der Azure-Speicheranforderung.   |     |
|`LeasePresent` | Dieses Element ist geleast und wird von einem anderen Benutzer verwendet. |
|`StorageRequestForbidden` |Konnten aufgrund von Authentifizierungsproblemen nicht hochgeladen werden. |
|`ManagedDiskCreationTerminalFailure` | Konnten nicht als verwaltete Datenträger hochgeladen werden. Die Dateien sind im Stagingspeicherkonto als Seitenblobs verfügbar. Sie können Seitenblobs manuell in verwaltete Datenträger konvertieren.  |
|`DiskConversionNotStartedTierInfoMissing` | Da die VHD-Datei außerhalb der vorab erstellten Ebenenordner kopiert wurde, wurde kein verwalteter Datenträger erstellt. Die Datei wird als Seitenblob in das Stagingspeicherkonto hochgeladen, wie es bei der Auftragserstellung angegeben wurde. Sie kann manuell in einen verwalteten Datenträger konvertiert werden.|
|`InvalidWorkitem` | Die Daten konnten nicht hochgeladen werden, da sie nicht den Azure-Konventionen für Benennung und Grenzwerte entsprechen.|
|`InvalidPageBlobUploadAsBlockBlob` | Als Blockblobs in einen Container mit dem Präfix `databoxdisk-invalid-pb-` hochgeladen.|
|`InvalidAzureFileUploadAsBlockBlob` | Als Blockblobs in einen Container mit dem Präfix `databoxdisk-invalid-af`- hochgeladen.|
|`InvalidManagedDiskUploadAsBlockBlob` | Als Blockblobs in einen Container mit dem Präfix `databoxdisk-invalid-md`- hochgeladen.|
|`InvalidManagedDiskUploadAsPageBlob` |Als Seitenblobs in einen Container mit dem Präfix `databoxdisk-invalid-md-` hochgeladen. |
|`MovedToOverflowShare` |Die Dateien wurden in eine neue Freigabe hochgeladen, da die Größe der ursprünglichen Freigabe die maximale Azure-Größenbegenzung übersteigt. Der Name der neuen Dateifreigabe entspricht dem ursprünglichen Namen mit dem Suffix `-2`.   |
|`MovedToDefaultAzureShare` |Es wurden Dateien hochgeladen, die nicht Teil eines Ordners für eine Standardfreigabe waren. Der Freigabename beginnt mit `databox-`. |
|`ContainerRenamed` |Der Container für diese Dateien hat den Azure-Benennungskonventionen nicht entsprochen und wird umbenannt. Der neue Name beginnt mit `databox-` und hat den SHA1-Hash des ursprünglichen Namens als Suffix. |
|`ShareRenamed` |Die Freigabe für diese Dateien hat den Azure-Benennungskonventionen nicht entsprochen und wird umbenannt. Der neue Name beginnt mit `databox-` und hat den SHA1-Hash des ursprünglichen Namens als Suffix. |
|`BlobRenamed` |Diese Dateien haben den Azure-Benennungskonventionen nicht entsprochen und wurden umbenannt. Den neuen Namen finden Sie im Feld `BlobPath`. |
|`FileRenamed` |Diese Dateien haben den Azure-Benennungskonventionen nicht entsprochen und wurden umbenannt. Den neuen Namen finden Sie im Feld `FileStoragePath`. |
|`DiskRenamed` |Diese Dateien haben den Azure-Benennungskonventionen nicht entsprochen und wurden umbenannt. Den neuen Namen finden Sie im Feld `BlobPath`. |


## <a name="next-steps"></a>Nächste Schritte

- [Öffnen Sie ein Supportticket für Data Box Disk-Probleme](data-box-disk-contact-microsoft-support.md).
