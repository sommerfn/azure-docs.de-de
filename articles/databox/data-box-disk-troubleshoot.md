---
title: Problembehandlung des Azure Data Box-Datenträgers | Microsoft Docs
description: Beschreibt, wie Sie Probleme mit dem Azure Data Box-Datenträger beheben können.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: article
ms.date: 06/14/2019
ms.author: alkohli
ms.openlocfilehash: f725f38a335972ae8e0a8b8402a99202caa54a70
ms.sourcegitcommit: 72f1d1210980d2f75e490f879521bc73d76a17e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2019
ms.locfileid: "67147080"
---
# <a name="use-logs-to-troubleshoot-validation-issues-in-azure-data-box-disk"></a>Verwenden von Protokollen zur Behandlung von Problemen bei der Überprüfung in Azure Data Box Disk

Dieser Artikel gilt für Microsoft Azure Data Box Disk. Er beschreibt, wie Sie mithilfe der Protokolle Probleme bei der Überprüfung behandeln, die bei Bereitstellung dieser Lösung auftreten können.

## <a name="validation-tool-log-files"></a>Protokolldateien des Überprüfungstools

Wenn Sie die Daten auf den Datenträgern mit dem [Überprüfungstool](data-box-disk-deploy-copy-data.md#validate-data) überprüfen, wird eine Datei *error.xml* zum Protokollieren von Fehlern generiert. Die Protokolldatei befindet sich im Ordner `Drive:\DataBoxDiskImport\logs` Ihres Laufwerks. Ein Link zum Fehlerprotokoll wird beim Ausführen der Überprüfung bereitgestellt.

<!--![Validation tool with link to error log](media/data-box-disk-troubleshoot/validation-tool-link-error-log.png)-->

Wenn Sie mehrere Sitzungen zur Überprüfung ausführen, wird ein Fehlerprotokoll pro Sitzung generiert.

- Nachfolgend sehen Sie ein Beispiel für das Fehlerprotokoll, wenn die in den Ordner `PageBlob` geladenen Daten nicht einem ganzzahligen Vielfachen von 512 Bytes entsprechen. Alle in PageBlob hochgeladenen Daten müssen einem ganzzahligen Vielfachen von 512 Bytes entsprechen, z.B. VHD oder VHDX. Die Fehler in dieser Datei befinden sich in `<Errors>` und Warnungen in `<Warnings>`.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
        <ErrorLog Version="2018-10-01">
            <SessionId>session#1</SessionId>
            <ItemType>PageBlob</ItemType>
            <SourceDirectory>D:\Dataset\TestDirectory</SourceDirectory>
            <Errors>
                <Error Code="Not512Aligned">
                    <Description>The file is not 512 bytes aligned.</Description>
                    <List>
                        <File Path="\Practice\myScript.ps1" />
                    </List>
                    <Count>1</Count>
                </Error>
            </Errors>
            <Warnings />
        </ErrorLog>
    ```

- Es folgt ein Beispiel für das Fehlerprotokoll, wenn der Containername nicht gültig ist. Der Ordner, den Sie unter den Ordnern `BlockBlob`, `PageBlob` oder `AzureFile` auf dem Datenträger erstellen, wird zu einem Container in Ihrem Azure Storage-Konto. Der Name des Containers muss den [Azur-Benennungskonventionen](data-box-disk-limits.md#azure-block-blob-page-blob-and-file-naming-conventions) entsprechen.

    ```xml
        <?xml version="1.0" encoding="utf-8"?>
        <ErrorLog Version="2018-10-01">
          <SessionId>bbsession</SessionId>
          <ItemType>BlockBlob</ItemType>
          <SourceDirectory>E:\BlockBlob</SourceDirectory>
          <Errors>
            <Error Code="InvalidShareContainerFormat">
              <List>
                <Container Name="Azu-reFile" />
                <Container Name="bbcont ainer1" />
              </List>
              <Count>2</Count>
            </Error>
          </Errors>
          <Warnings />
    </ErrorLog>
    ```

## <a name="validation-tool-errors"></a>Fehler im Überprüfungstool

In der folgenden Tabelle sind die Fehler, die in *error.xml* enthalten sind, zusammen mit den jeweiligen empfohlenen Aktionen zusammengefasst.

| Fehlercode| BESCHREIBUNG                       | Empfohlene Aktionen               |
|------------|--------------------------|-----------------------------------|
| `None` | Die Daten wurden erfolgreich überprüft. | Es ist keine Aktion erforderlich. |
| `InvalidXmlCharsInPath` |Es konnte keine Manifestdatei erstellt werden, da der Dateipfad ungültige Zeichen enthält. | Entfernen Sie diese Zeichen, um fortzufahren.  |
| `OpenFileForReadFailed`| Die Datei konnte nicht verarbeitet werden. Die Ursache hierfür kann ein Zugriffsproblem oder eine Beschädigung des Dateisystems sein.|Die Datei konnte aufgrund eines Fehlers nicht gelesen werden. Die Fehlerdetails befinden sich in der Ausnahme. |
| `Not512Aligned` | Diese Datei weist kein gültiges Format für den Ordner „PageBlob“ auf.| Laden Sie nur Daten, die einem ganzzahligen Vielfachen von 512 Bytes entsprechen, in den Ordner `PageBlob` hoch. Entfernen Sie die Datei aus dem Ordner „PageBlob“, oder verschieben Sie sie in den Ordner „BlockBlob“. Wiederholen Sie die Überprüfung.|
| `InvalidBlobPath` | Der Dateipfad ist keinem gültigen Blobpfad in der Cloud gemäß den Benennungskonventionen für Azure-Blobs zugeordnet.|Folgen Sie den Azure Benennungsrichtlinien zum Umbenennen des Dateipfads. |
| `EnumerationError` | Die Datei konnte nicht für die Überprüfung aufgezählt werden. |Es kann mehrere Gründe für diesen Fehler geben. Eine sehr wahrscheinliche Ursache ist der Zugriff auf die Datei. |
| `ShareSizeExceeded` | Diese Datei hat bewirkt, dass die Größe der Azure-Dateifreigabe den Azure-Grenzwert von 5 TB übersteigt.|Verringern Sie die Größe der Daten in der Freigabe, sodass sie den [Größenbeschränkungen für das Azure-Objekt](data-box-disk-limits.md#azure-object-size-limits) entspricht. Wiederholen Sie die Überprüfung. |
| `AzureFileSizeExceeded` | Die Dateigröße überschreitet die Größenbeschränkungen für Azure-Dateien.| Verringern Sie die Größe der Datei oder der Daten, sodass sie den [Größenbeschränkungen für das Azure-Objekt](data-box-disk-limits.md#azure-object-size-limits) entspricht. Wiederholen Sie die Überprüfung.|
| `BlockBlobSizeExceeded` | Die Dateigröße überschreitet die Größenbeschränkungen für Azure-Blockblobs. | Verringern Sie die Größe der Datei oder der Daten, sodass sie den [Größenbeschränkungen für das Azure-Objekt](data-box-disk-limits.md#azure-object-size-limits) entspricht. Wiederholen Sie die Überprüfung. |
| `ManagedDiskSizeExceeded` | Die Dateigröße überschreitet die Größenbeschränkungen für verwaltete Azure-Datenträger. | Verringern Sie die Größe der Datei oder der Daten, sodass sie den [Größenbeschränkungen für das Azure-Objekt](data-box-disk-limits.md#azure-object-size-limits) entspricht. Wiederholen Sie die Überprüfung. |
| `PageBlobSizeExceeded` | Die Dateigröße überschreitet die Größenbeschränkungen für verwaltete Azure-Datenträger. | Verringern Sie die Größe der Datei oder der Daten, sodass sie den [Größenbeschränkungen für das Azure-Objekt](data-box-disk-limits.md#azure-object-size-limits) entspricht. Wiederholen Sie die Überprüfung. |
| `InvalidShareContainerFormat`  |Die Verzeichnisnamen entsprechen nicht den Azure-Benennungskonventionen für Container oder Freigaben.         |Der erste Ordner, der unter den bereits vorhandenen Ordnern auf dem Datenträger erstellt wird, wird zu einem Container in Ihrem Speicherkonto. Dieser Freigabe- oder Containername entspricht nicht den Azure-Benennungskonventionen. Benennen Sie die Datei um, damit sie den [Azure-Benennungskonventionen](data-box-disk-limits.md#azure-block-blob-page-blob-and-file-naming-conventions) entspricht. Wiederholen Sie die Überprüfung.   |
| `InvalidBlobNameFormat` | Der Dateipfad ist keinem gültigen Blobpfad in der Cloud gemäß den Benennungskonventionen für Azure-Blobs zugeordnet.|Benennen Sie die Datei um, damit sie den [Azure-Benennungskonventionen](data-box-disk-limits.md#azure-block-blob-page-blob-and-file-naming-conventions) entspricht. Wiederholen Sie die Überprüfung. |
| `InvalidFileNameFormat` | Der Dateipfad ist keinem gültigen Dateipfad in der Cloud gemäß den Benennungskonventionen für Azure-Dateien zugeordnet. |Benennen Sie die Datei um, damit sie den [Azure-Benennungskonventionen](data-box-disk-limits.md#azure-block-blob-page-blob-and-file-naming-conventions) entspricht. Wiederholen Sie die Überprüfung. |
| `InvalidDiskNameFormat` | Der Dateipfad ist keinem gültigen Datenträgernamen in der Cloud gemäß den Benennungskonventionen für verwaltete Azure-Datenträger zugeordnet. |Benennen Sie die Datei um, damit sie den [Azure-Benennungskonventionen](data-box-disk-limits.md#azure-block-blob-page-blob-and-file-naming-conventions) entspricht. Wiederholen Sie die Überprüfung.       |
| `NotPartOfFileShare` | Die Dateien konnten nicht hochgeladen werden, da der Uploadpfad nicht gültig ist. Laden Sie die Dateien in einen Ordner in Azure Files hoch.   | Entfernen Sie die fehlerhaften Dateien, und laden Sie diese Dateien in einer vorab erstellten Ordner hoch. Wiederholen Sie die Überprüfung. |
| `NonVhdFileNotSupportedForManagedDisk` | Eine andere Datei als VHD kann nicht als ein verwalteter Datenträger hochgeladen werden. |Entfernen Sie die anderen Dateien, die nicht VHD sind, da diese nicht unterstützt werden. Wiederholen Sie die Überprüfung. |


## <a name="next-steps"></a>Nächste Schritte

- Behandeln Sie [Fehler beim Hochladen von Daten](data-box-disk-troubleshoot-upload.md).
