---
title: Entscheidung zwischen Azure-Blobs, Azure Files und Azure-Datenträger
description: In diesem Artikel erfahren Sie, wie Sie Daten auf unterschiedliche Weise in Azure speichern und darauf zugreifen können. So wird Ihnen die Entscheidung erleichtert, welche Technologie Sie verwenden sollten.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 11/28/2018
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 702627a6307e1a6644dc41aeee947d33eb76522d
ms.sourcegitcommit: a0b37e18b8823025e64427c26fae9fb7a3fe355a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/25/2019
ms.locfileid: "68501335"
---
# <a name="deciding-when-to-use-azure-blobs-azure-files-or-azure-disks"></a>Entscheidung zwischen Azure-Blobs, Azure Files und Azure-Datenträger

Microsoft Azure bietet verschiedenen Funktionen in Azure Storage zum Speichern und Zugreifen auf Ihre Daten in der Cloud. Dieser Artikel befasst sich mit Azure Files, Blobs und Datenträger und soll Sie bei der Entscheidung zwischen diesen Funktionen unterstützen.

## <a name="scenarios"></a>Szenarien

In der folgenden Tabelle werden Files, Blobs und Datenträger miteinander verglichen. Darüber hinaus werden jeweils passende Beispielszenarios gegeben.

| Feature | BESCHREIBUNG | Einsatzgebiete |
|--------------|-------------|-------------|
| **Azure Files** | Bietet eine SMB-Schnittstelle, Clientbibliotheken und eine [REST-Schnittstelle](/rest/api/storageservices/file-service-rest-api), mit denen Sie von überall auf gespeicherte Dateien zugreifen können. | Wenn Sie eine Anwendung mit der „Lift and Shift“-Methode in die Cloud verschieben möchten, die bereits die nativen Dateisystem-APIs verwendet, um Daten für andere in Azure ausgeführte Anwendungen freizugeben.<br/><br/>Wenn Sie Tools zum Entwickeln und Debuggen speichern möchten, auf die von vielen virtuellen Computern zugegriffen werden muss. |
| **Azure-Blobs** | Bietet Clientbibliotheken und eine [REST-Schnittstelle](/rest/api/storageservices/blob-service-rest-api), der große Mengen an unstrukturierten Daten in Blockblobs gespeichert und abgerufen werden können.<br/><br/>Blob Storage unterstützt darüber hinaus [Azure Data Lake Storage Gen2](../blobs/data-lake-storage-introduction.md) für Big Data-Analyselösungen auf Unternehmensniveau. | Wenn Sie möchten, dass Ihre Anwendung Szenarios für das Streaming und den zufälligen Zugriff unterstützt.<br/><br/>Wenn Sie die Möglichkeit haben möchten, von überall auf Anwendungsdaten zugreifen zu können.<br/><br/>Sie möchten einen Unternehmens-Data Lake in Azure aufbauen und Big Data-Analysen durchführen. |
| **Azure-Datenträger** | Bietet Clientbibliotheken und eine [REST-Schnittstelle](/rest/api/compute/manageddisks/disks/disks-rest-api), mit der Sie Daten beständig von einer angefügten virtuellen Festplatte speichern und abrufen können. | Wenn Sie eine Anwendung mit der „Lift and Shift“-Methode verschieben möchten, die native Dateisystem-APIs verwenden, um Daten in beständigen Datenträgern zu lesen und dort hinein zu schreiben.<br/><br/>Wenn Sie Daten speichern möchten, auf die nicht von außerhalb des virtuellen Computers zugegriffen werden muss, an den der Datenträger angefügt ist. |


## <a name="next-steps"></a>Nächste Schritte

Bei der Entscheidung für die Art und Weise, wie Sie Ihre Daten speichern und auf diese zugreifen möchten, sollten Sie auch die Kosten berücksichtigen. Weitere Informationen finden Sie unter [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/).
  
Einige SMB-Funktion sind nicht in der Cloud verfügbar. Weitere Informationen finden Sie unter [Features not supported by the Azure File service (Funktionen, die vom Azure-Dateidienst nicht unterstützt werden)](/rest/api/storageservices/features-not-supported-by-the-azure-file-service).
 
Weitere Informationen zu Azure-Blobs finden Sie im Artikel [Was ist Azure Blob Storage?](../blobs/storage-blobs-overview.md).

Weitere Informationen zu Disk Storage finden Sie in der [Einführung in verwaltete Azure-Datenträger](../../virtual-machines/windows/managed-disks-overview.md).

Weitere Informationen zu Azure Files finden Sie im Artikel [Planung für eine Azure Files-Bereitstellung](../files/storage-files-planning.md).