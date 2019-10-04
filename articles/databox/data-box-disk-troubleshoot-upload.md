---
title: Behandeln von Problemen beim Hochladen von Daten in Azure Data Box Disk mithilfe von Protokollen | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Protokolle verwenden und Probleme beim Hochladen von Daten in Azure Data Box Disk beheben.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: article
ms.date: 06/17/2019
ms.author: alkohli
ms.openlocfilehash: deaa9a220ee4d765650779b40742225e300ffdb7
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807494"
---
# <a name="understand-logs-to-troubleshoot-data-upload-issues-in-azure-data-box-disk"></a>Grundlegendes zu Protokollen für die Behandlung von Problemen beim Hochladen von Daten in Azure Data Box Disk

Dieser Artikel gilt für Microsoft Azure Data Box Disk. Er beschreibt die Problemen, die beim Hochladen von Daten in Azure auftreten können.

## <a name="about-upload-logs"></a>Informationen zu Uploadprotokollen

Beim Hochladen der Daten auf Azure im Rechenzentrum werden die Dateien `_error.xml` und `_verbose.xml` für jedes Speicherkonto generiert. Diese Protokolle werden in dasselbe Speicherkonto hochgeladen, das auch zum Hochladen der Daten verwendet wurde. 

Beide Protokolle weisen dasselbe Format auf und enthalten XML-Beschreibungen der Ereignisse, die aufgetreten sind, während die Daten vom Datenträger in das Azure Storage-Konto kopiert wurden.

Das ausführliche Protokoll enthält vollständige Informationen zum Status des Kopiervorgangs für jedes Blob oder jede Datei. Das Fehlerprotokoll enthält dagegen nur die Informationen zu Blobs oder Dateien, bei denen während des Hochladens Fehler aufgetreten sind.

Das Fehlerprotokoll hat die gleiche Struktur wie das ausführliche Protokoll; allerdings werden erfolgreiche Vorgänge herausgefiltert.

## <a name="download-logs"></a>Herunterladen von Protokollen

Führen Sie die folgenden Schritte aus, um die Uploadprotokolle zu suchen.

1. Wenn beim Hochladen der Daten in Azure Fehler auftreten, zeigt das Portal einen Pfad zu dem Ordner an, in dem sich die Diagnoseprotokolle befinden.

    ![Link zu Protokollen im Portal](./media/data-box-disk-troubleshoot-upload/data-box-disk-portal-logs.png)

2. Navigieren Sie zu **waies**.

    ![Fehlerprotokolle und ausführliche Protokolle](./media/data-box-disk-troubleshoot-upload/data-box-disk-portal-logs-1.png)

Es werden in jedem Fall die Fehlerprotokolle und die ausführlichen Protokolle angezeigt. Wählen Sie die einzelnen Protokolle aus, und laden Sie eine lokale Kopie herunter.

## <a name="sample-upload-logs"></a>Beispiel für Uploadprotokolle

Ein Beispiel für die Datei `_verbose.xml` wird nachfolgend gezeigt. In diesem Fall wurde der Auftrag ohne Fehler erfolgreich abgeschlossen.

```xml

<?xml version="1.0" encoding="utf-8"?>
<DriveLog Version="2018-10-01">
  <DriveId>184020D95632</DriveId>
  <Blob Status="Completed">
    <BlobPath>$root/botetapageblob.vhd</BlobPath>
    <FilePath>\PageBlob\botetapageblob.vhd</FilePath>
    <Length>1073742336</Length>
    <ImportDisposition Status="Created">rename</ImportDisposition>
    <PageRangeList>
      <PageRange Offset="0" Length="4194304" Status="Completed" />
      <PageRange Offset="4194304" Length="4194304" Status="Completed" />
      <PageRange Offset="8388608" Length="4194304" Status="Completed" />
      --------CUT-------------------------------------------------------
      <PageRange Offset="1061158912" Length="4194304" Status="Completed" />
      <PageRange Offset="1065353216" Length="4194304" Status="Completed" />
      <PageRange Offset="1069547520" Length="4194304" Status="Completed" />
      <PageRange Offset="1073741824" Length="512" Status="Completed" />
    </PageRangeList>
  </Blob>
  <Blob Status="Completed">
    <BlobPath>$root/botetablockblob.txt</BlobPath>
    <FilePath>\BlockBlob\botetablockblob.txt</FilePath>
    <Length>19</Length>
    <ImportDisposition Status="Created">rename</ImportDisposition>
    <BlockList>
      <Block Offset="0" Length="19" Status="Completed" />
    </BlockList>
  </Blob>
  <File Status="Completed">
    <FileStoragePath>botetaazurefilesfolder/botetaazurefiles.txt</FileStoragePath>
    <FilePath>\AzureFile\botetaazurefilesfolder\botetaazurefiles.txt</FilePath>
    <Length>20</Length>
    <ImportDisposition Status="Created">rename</ImportDisposition>
    <FileRangeList>
      <FileRange Offset="0" Length="20" Status="Completed" />
    </FileRangeList>
  </File>
  <Status>Completed</Status>
</DriveLog>
```

Bei demselben Auftrag wird nachstehend ein Beispiel für die Datei `_error.xml` gezeigt.

```xml

<?xml version="1.0" encoding="utf-8"?>
<DriveLog Version="2018-10-01">
  <DriveId>184020D95632</DriveId>
  <Summary>
    <ValidationErrors>
      <None Count="3" />
    </ValidationErrors>
    <CopyErrors>
      <None Count="3" Description="No errors encountered" />
    </CopyErrors>
  </Summary>
  <Status>Completed</Status>
</DriveLog>
```

Ein Beispiel für die Datei `_error.xml` wird nachstehend gezeigt, in dem der Auftrag mit Fehlern abgeschlossen wurde. 

In diesem Fall enthält die Fehlerdatei einen Abschnitt `Summary` und einen weiteren Abschnitt mit allen Fehlern auf Dateiebene. 

Der Abschnitt `Summary` enthält die `ValidationErrors` und die `CopyErrors`. In diesem Fall wurden 8 Dateien oder Ordner auf Azure hochgeladen, und es gab keine Validierungsfehler. Beim Kopieren der Daten in das Azure Storage-Konto wurden 5 Dateien oder Ordner erfolgreich hochgeladen. Die übrigen 3 Dateien oder Ordner wurden entsprechend den Benennungskonventionen für Azure-Container umbenannt und dann erfolgreich auf Azure hochgeladen.

Die Status der Dateiebenen sind im Abschnitt `BlobStatus` enthalten, in dem alle Aktionen beschrieben werden, die zum Hochladen der Blobs ausgeführt wurden. In diesem Fall werden drei Container umbenannt, weil die Ordner, in die die Daten kopiert wurden, den Azure-Benennungskonventionen für Container nicht entsprochen haben. Für die in diesen Containern hochgeladenen Blobs werden der neue Containername, der Pfad des Blobs in Azure, der ursprüngliche ungültige Dateipfad und die Blobgröße einbezogen.
    
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

## <a name="data-upload-errors"></a>Fehler beim Hochladen von Daten

Die Fehler, die beim Hochladen von Daten in Azure generiert werden, sind in der folgenden Tabelle zusammengefasst.

| Fehlercode | BESCHREIBUNG                   |
|-------------|------------------------------|
|`None` |  Erfolgreich abgeschlossen.           |
|`Renamed` | Das Blob wurde erfolgreich umbenannt.   |
|`CompletedWithErrors` | Der Upload wurde mit Fehlern abgeschlossen. Die Details der fehlerhaften Dateien sind in der Protokolldatei enthalten.  |
|`Corrupted`|Der während der Datenerfassung berechnete CRC-Wert stimmt nicht mit dem während des Uploads berechneten CRC-Wert überein.  |  
|`StorageRequestFailed` | Fehler bei der Azure-Speicheranforderung.   |     
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
