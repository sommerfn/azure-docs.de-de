---
title: Bekannte Probleme mit Azure Data Lake Storage Gen2 | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Einschränkungen und bekannten Probleme mit Azure Data Lake Storage Gen2.
services: storage
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 04/26/2019
ms.author: normesta
ms.openlocfilehash: daf9199104047f714d568bd2796490b836243952
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67443230"
---
# <a name="known-issues-with-azure-data-lake-storage-gen2"></a>Bekannte Probleme mit Azure Data Lake Storage Gen2

In diesem Artikel werden die Funktionen und Tools aufgelistet, die noch nicht oder nur teilweise für Speicherkonten unterstützt werden, die einen hierarchischen Namespace haben (Azure Data Lake Storage Gen2).

<a id="blob-apis-disabled" />

## <a name="blob-storage-apis"></a>Blob Storage-APIs

Blob Storage-APIs sind deaktiviert, um Probleme bei der Funktionsfähigkeit von Features zu vermeiden, die durch die fehlende Interoperabilität zwischen Blob Storage-APIs und Azure Data Lake Storage Gen2-APIs entstehen können.

### <a name="what-to-do-with-existing-tools-applications-and-services"></a>Vorgehensweise bei vorhandenen Tools, Anwendungen und Diensten

Wenn Sie über Tools, Anwendungen oder Dienste verfügen, die Blob-APIs verwenden, und diese für alle Inhalte einsetzen möchten, die Sie auf Ihr Konto hochladen, aktivieren Sie so lange keinen hierarchischen Namespace für Ihr Blob-Speicherkonto, bis die Interoperabiliät von Blob-APIs mit Azure Data Lake Gen2-APIs sichergestellt ist.

Die Verwendung eines Speicherkontos ohne einen hierarchischen Namespace bedeutet, dass Sie keinen Zugriff auf Data Lake Storage Gen2-spezifische Features wie Zugriffssteuerungslisten (ACLs) für das Verzeichnis- und Dateisystem haben.

### <a name="what-to-do-with-unmanaged-virtual-machine-vm-disks"></a>Vorgehensweise bei nicht verwalteten VM-Datenträgern

Nicht verwaltete VM-Datenträger hängen von den deaktivierten Blob Storage-APIs ab. Wenn Sie also einen hierarchischen Namespace für ein Speicherkonto aktivieren möchten, platzieren Sie nicht verwaltete VM-Datenträger ggf. in einem Speicherkonto, bei dem das Feature für hierarchische Namespaces nicht aktiviert ist.

### <a name="what-to-do-if-you-used-blob-apis-to-load-data-before-blob-apis-were-disabled"></a>Vorgehensweise, wenn Sie Blob-APIs zum Laden von Daten verwendet haben, bevor diese deaktiviert wurden

Falls Sie diese APIs vor der Deaktivierung zum Laden von Daten verwendet haben und produktionsbedingt Zugriff auf diese Daten benötigen, wenden Sie sich mit folgenden Informationen an den Microsoft-Support:

> [!div class="checklist"]
> * Abonnement-ID (die GUID, nicht der Name)
> * Speicherkontoname(n)
> * Ist Ihre Produktion direkt betroffen, und falls ja, für welche Speicherkonten?
> * Müssen die Daten in ein anderes Speicherkonto kopiert werden, und falls ja, warum? (Diese Frage ist auch relevant, wenn Ihre Produktion nicht direkt betroffen ist.)

Unter diesen Umständen können wir den Zugriff auf die Blob-API für einen begrenzten Zeitraum wiederherstellen, damit Sie diese Daten in ein Speicherkonto kopieren können, bei dem das Feature „hierarchischer Namespace“ nicht aktiviert ist.

## <a name="all-other-features-and-tools"></a>Alle anderen Features und Tools

In der folgenden Tabelle werden die Funktionen und Tools aufgelistet, die noch nicht oder nur teilweise für Speicherkonten unterstützt werden, die einen hierarchischen Namespace haben (Azure Data Lake Storage Gen2).

| Feature/Tool    | Weitere Informationen    |
|--------|-----------|
| **APIs für Data Lake Storage Gen2-Speicherkonten** | Teilweise unterstützt <br><br>Sie können Data Lake Storage Gen2-**REST**-APIs verwenden, aber APIs in anderen Blob-SDKs wie .NET-, Java- und Python-SDKs sind noch nicht verfügbar.|
| **AzCopy** | Versionsspezifische Unterstützung <br><br>Verwenden Sie nur die neueste Version von AzCopy ([AzCopy v10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2ftables%2ftoc.json)). Frühere Versionen von AzCopy wie z. B. AzCopy v8.1 werden nicht unterstützt.|
| **Richtlinien für die Azure Blob Storage-Lebenszyklusverwaltung** | Noch nicht unterstützt |
| **Azure Content Delivery Network (CDN)** | Noch nicht unterstützt|
| **Azure Search** |Noch nicht unterstützt|
| **Azure Storage-Explorer** | Versionsspezifische Unterstützung <br><br>Verwenden Sie nur Version `1.6.0` oder höher. <br>Version `1.6.0` steht zum [kostenlosen Download](https://azure.microsoft.com/features/storage-explorer/) zur Verfügung.|
| **Blobcontainer-ACLs** |Noch nicht unterstützt|
| **blobfuse** |Noch nicht unterstützt|
| **Benutzerdefinierte Domänen** |Noch nicht unterstützt|
| **Diagnoseprotokolle** |Noch nicht unterstützt|
| **Dateisystem-Explorer** | Eingeschränkte Unterstützung |
| **Unveränderlicher Speicher** |Noch nicht unterstützt <br><br>Durch unveränderlichen Speicher können Sie Daten in einem [WORM-Zustand (Write Once, Read Many)](https://docs.microsoft.com/azure/storage/blobs/storage-blob-immutable-storage) speichern.|
| **Objektebenen** |Noch nicht unterstützt <br><br>Beispiel:  die Ebenen „Premium“, „Hot“, „Cold“ und „Archive“.|
| **PowerShell- und CLI-Unterstützung** | Eingeschränkte Funktionalität <br><br>Sie können ein Konto mit PowerShell oder der CLI erstellen. Sie können Vorgänge durchführen oder ACLs in Dateisystemen, Verzeichnissen und Dateien festlegen.|
| **Statische Websites** |Noch nicht unterstützt <br><br>Insbesondere die Möglichkeit, Dateien an [statische Websites](https://docs.microsoft.com/azure/storage/blobs/storage-blob-static-website) zu senden.|
| **Drittanbieteranwendungen** | Eingeschränkte Unterstützung <br><br>Drittanbieteranwendungen, die REST-APIs verwenden, funktionieren auch weiterhin, wenn Sie sie mit Data Lake Storage Gen2 verwenden. <br>Wenn Sie eine Anwendung haben, die Blob-APIs verwendet, ist es sehr wahrscheinlich, dass es bei dieser Anwendung zu Problemen kommt, wenn Sie sie mit Data Lake Storage Gen2 verwenden. Weitere Informationen finden Sie im Abschnitt [Blob Storage-APIs](#blob-apis-disabled) in diesem Artikel.|
| **Versionsverwaltungsfunktionen** |Noch nicht unterstützt <br><br>Dazu zählen [Momentaufnahmen](https://docs.microsoft.com/rest/api/storageservices/creating-a-snapshot-of-a-blob) und [vorläufiges Löschen](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete).|
|

