---
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: include
ms.date: 06/18/2019
ms.author: alkohli
ms.openlocfilehash: 0975df8ee9da4a239dbd72e468b967c88ab3feed
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2019
ms.locfileid: "67277261"
---
Dies sind die Grenzwerte für die Größe der Daten, die in das Speicherkonto kopiert werden. Stellen Sie sicher, dass die von Ihnen hochgeladenen Daten diesen Grenzwerten entsprechen. Die neuesten Informationen zu diesen Einschränkungen finden Sie unter [Skalierbarkeitsziele für Azure-Blobspeicher](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets#azure-blob-storage-scale-targets) und [Skalierbarkeitsziele für Azure Files](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets#azure-files-scale-targets).

| Größe der in das Azure-Speicherkonto kopierten Daten                      | Standardlimit          |
|---------------------------------------------------------------------|------------------------|
| Blockblob und Seitenblob                                            | 2 PB für USA und Europa.<br>500 TB für alle anderen Regionen, einschließlich VK.  <br> Dies schließt Daten aus allen Quellen einschließlich Data Box ein.|
| Azure Files                                                          | 5 TB pro Freigabe.<br> Alle Ordner unter *StorageAccount_AzureFiles* müssen diese Beschränkung einhalten.       |
