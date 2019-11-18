---
title: Bekannte Probleme mit Azure Data Lake Storage Gen2 | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Einschränkungen und bekannten Probleme mit Azure Data Lake Storage Gen2.
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 11/03/2019
ms.author: normesta
ms.reviewer: jamesbak
ms.openlocfilehash: 95f2dbdbb34ff349d14be430b4e5a4fa84df0f5a
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73581483"
---
# <a name="known-issues-with-azure-data-lake-storage-gen2"></a>Bekannte Probleme mit Azure Data Lake Storage Gen2

In diesem Artikel werden die Funktionen und Tools aufgelistet, die noch nicht oder nur teilweise für Speicherkonten unterstützt werden, die einen hierarchischen Namespace haben (Azure Data Lake Storage Gen2).

<a id="blob-apis-disabled" />

## <a name="issues-and-limitations-with-using-blob-apis"></a>Probleme und Einschränkungen bei der Verwendung von Blob-APIs

Blob-APIs und Data Lake Storage Gen2-APIs können mit denselben Daten arbeiten.

In diesem Abschnitt werden Probleme und Einschränkungen bei der Verwendung von Blob-APIs und Data Lake Storage Gen2-APIs für dieselben Daten beschrieben.

* Es ist nicht möglich, sowohl Blob-APIs als auch Data Lake Storage-APIs zu verwenden, um in dieselbe Instanz einer Datei zu schreiben. Wenn Sie in eine Datei schreiben, indem Sie Data Lake Storage Gen2-APIs verwenden, sind die Blöcke dieser Datei für Aufrufe der [Get Block List](https://docs.microsoft.com/rest/api/storageservices/get-block-list)-Blob-API nicht sichtbar. Sie können eine Datei überschreiben, indem Sie entweder Data Lake Storage Gen2-APIs oder Blob-APIs verwenden. Dies wirkt sich nicht auf die Dateieigenschaften aus.

* Wenn Sie den Vorgang [List Blobs](https://docs.microsoft.com/rest/api/storageservices/list-blobs) verwenden, ohne ein Trennzeichen anzugeben, enthalten die Ergebnisse sowohl Verzeichnisse als auch Blobs. Wenn Sie sich für Trennzeichen entscheiden, sollten Sie nur einen Schrägstrich (`/`) verwenden. Dies ist das einzige Trennzeichen, das unterstützt wird.

* Wenn Sie die [Delete Blob](https://docs.microsoft.com/rest/api/storageservices/delete-blob)-API zum Löschen eines Verzeichnisses verwenden, wird es nur gelöscht, sofern es leer ist. Dies bedeutet, dass Sie die Blob-API zum Löschen von Verzeichnissen nicht rekursiv verwenden können.

Diese Blob-Rest-APIs werden nicht unterstützt:

* [Put Blob (Page)](https://docs.microsoft.com/rest/api/storageservices/put-blob)
* [Put Page](https://docs.microsoft.com/rest/api/storageservices/put-page)
* [Get Page Ranges](https://docs.microsoft.com/rest/api/storageservices/get-page-ranges)
* [Incremental Copy Blob](https://docs.microsoft.com/rest/api/storageservices/incremental-copy-blob)
* [Put Page from URL](https://docs.microsoft.com/rest/api/storageservices/put-page-from-url)
* [Put Blob (Append)](https://docs.microsoft.com/rest/api/storageservices/put-blob)
* [Append Block](https://docs.microsoft.com/rest/api/storageservices/append-block)
* [Append Block from URL](https://docs.microsoft.com/rest/api/storageservices/append-block-from-url)

Nicht verwaltete VM-Datenträger werden für Konten, die über einen hierarchischen Namespace verfügen, nicht unterstützt. Wenn Sie einen hierarchischen Namespace für ein Speicherkonto aktivieren möchten, sollten Sie verwaltete VM-Datenträger in einem Speicherkonto anordnen, für das die Funktion für hierarchische Namespaces nicht aktiviert ist.

## <a name="support-for-other-blob-storage-features"></a>Unterstützung anderer Blob Storage-Features

In der folgenden Tabelle werden die Funktionen und Tools aufgelistet, die noch nicht oder nur teilweise für Speicherkonten unterstützt werden, die einen hierarchischen Namespace haben (Azure Data Lake Storage Gen2).

| Feature/Tool    | Weitere Informationen    |
|--------|-----------|
| **Data Lake Storage Gen2-APIs** | Teilweise unterstützt <br><br>Im aktuellen Release können Sie Data Lake Storage Gen2-**REST**-APIs verwenden, um mit Verzeichnissen zu interagieren und Zugriffssteuerungslisten (Access Control Lists, ACLs) festzulegen, aber es gibt keine anderen SDKs (z. B. .NET, Java oder Python), um diese Aufgaben auszuführen. Um andere Aufgaben wie das Hochladen und Herunterladen von Dateien auszuführen, können Sie die Blob SDKs verwenden.  |
| **AzCopy** | Versionsspezifische Unterstützung <br><br>Verwenden Sie nur die neueste Version von AzCopy ([AzCopy v10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2ftables%2ftoc.json)). Frühere Versionen von AzCopy wie z. B. AzCopy v8.1 werden nicht unterstützt.|
| **Richtlinien für die Azure Blob Storage-Lebenszyklusverwaltung** | Alle Zugriffsebenen werden unterstützt. Die Archivzugriffsebene befindet sich derzeit in der Vorschauphase. Das Löschen von Blobmomentaufnahmen wird noch nicht unterstützt. |
| **Azure Content Delivery Network (CDN)** | Noch nicht unterstützt|
| **Azure Search** |Unterstützt (Vorschauversion)|
| **Azure Storage-Explorer** | Versionsspezifische Unterstützung <br><br>Verwenden Sie nur Version `1.6.0` oder höher. <br>Version `1.6.0` steht zum [kostenlosen Download](https://azure.microsoft.com/features/storage-explorer/) zur Verfügung.|
| **Blobcontainer-ACLs** |Noch nicht unterstützt|
| **blobfuse** |Noch nicht unterstützt|
| **Benutzerdefinierte Domänen** |Noch nicht unterstützt|
| **Storage-Explorer im Azure-Portal** | Eingeschränkte Unterstützung. ACLs werden noch nicht unterstützt. |
| **Diagnoseprotokollierung** |Diagnoseprotokolle werden unterstützt (Vorschau).<br><br>Das Aktivieren von Protokollen im Azure-Portal wird derzeit nicht unterstützt. Im folgenden finden Sie ein Beispiel für die Aktivierung der Protokolle mithilfe von PowerShell. <br><br>`$storageAccount = Get-AzStorageAccount -ResourceGroupName <resourceGroup> -Name <storageAccountName>`<br><br>`Set-AzStorageServiceLoggingProperty -Context $storageAccount.Context -ServiceType Blob -LoggingOperations read,write,delete -RetentionDays <days>`. <br><br>Stellen Sie sicher, dass Sie `Blob` als Wert des Parameters `-ServiceType` angeben, wie in diesem Beispiel gezeigt. <br><br>Derzeit kann Azure Storage-Explorer nicht zum Anzeigen von Diagnoseprotokollen verwendet werden. Verwenden Sie zum Anzeigen von Protokollen AzCopy oder SDKs.
| **Unveränderlicher Speicher** |Noch nicht unterstützt <br><br>Durch unveränderlichen Speicher können Sie Daten in einem [WORM-Zustand (Write Once, Read Many)](https://docs.microsoft.com/azure/storage/blobs/storage-blob-immutable-storage) speichern.|
| **Objektebenen** |Kalte und Archivebene werden unterstützt. Die Archivebene befindet sich in der Vorschauphase. Alle anderen Zugriffsebenen werden noch nicht unterstützt.|
| **PowerShell- und CLI-Unterstützung** | Eingeschränkte Funktionalität <br><br>Bloboperationen werden unterstützt. Das Verwenden von Verzeichnissen und Festlegen von Zugriffssteuerungslisten (ACLs) wird noch nicht unterstützt. |
| **Statische Websites** |Noch nicht unterstützt <br><br>Insbesondere die Möglichkeit, Dateien an [statische Websites](https://docs.microsoft.com/azure/storage/blobs/storage-blob-static-website) zu senden.|
| **Drittanbieteranwendungen** | Eingeschränkte Unterstützung <br><br>Drittanbieteranwendungen, die REST-APIs verwenden, funktionieren auch weiterhin, wenn Sie sie mit Data Lake Storage Gen2 verwenden. <br>Anwendungen, die Blob-APIs aufrufen, funktionieren wahrscheinlich.|
|**Vorläufiges Löschen** |Noch nicht unterstützt|
| **Versionsverwaltungsfunktionen** |Noch nicht unterstützt <br><br>Dies umfasst Funktionen wie [Vorläufiges Löschen](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete) und andere Versionsverwaltungsfeatures wie [Momentaufnahmen](https://docs.microsoft.com/rest/api/storageservices/creating-a-snapshot-of-a-blob).|


