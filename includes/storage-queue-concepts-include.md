---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.custom: seo-python-october2019
ms.openlocfilehash: 92f638666d9ac832ee5e6a7d4dccf9a9e669f908
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72427978"
---
## <a name="what-is-queue-storage"></a>Was ist der Warteschlangenspeicher?

Die Warteschlangenspeicherung in Azure ist ein Dienst zur Speicherung großer Anzahlen von Nachrichten, auf die von überall auf der Welt mit authentifizierten Anrufen über HTTP oder HTTPS zugegriffen werden kann. Eine einzelne Warteschlangennachricht kann bis zu 64 KB groß sein, und eine Warteschlange kann Millionen von Nachrichten enthalten. Deren Anzahl ist nur durch die Kapazität des Speicherkontos begrenzt. Warteschlangenspeicher wird häufig verwendet, um ein Arbeits-Backlog zur asynchronen Verarbeitung zu erstellen.

## <a name="queue-service-concepts"></a>Konzepte des Warteschlangendiensts

Der Azure-Warteschlangendienst umfasst die folgenden Komponenten:

![Azure-Warteschlangendienst – Komponenten](./media/storage-queue-concepts-include/azure-queue-service-components.png)

* **URL-Format:** Warteschlangen können über das folgende URL-Format aufgerufen werden:   
    http://`<storage account>`.queue.core.windows.net/`<queue>` 
  
    Mit der folgenden URL kann eine der Warteschlangen im Diagramm adressiert werden:  
  
    `http://myaccount.queue.core.windows.net/images-to-download`

* **Speicherkonto:** Alle Zugriffe auf den Azure-Speicher erfolgen über ein Speicherkonto. Weitere Informationen zur Kapazität der Speicherkonten finden Sie unter [Azure Storage Scalability and Performance Targets](../articles/storage/common/storage-scalability-targets.md) (Skalierbarkeits- und Leistungsziele für Microsoft Azure-Speicher, in englischer Sprache).
* **Warteschlange:** Eine Warteschlange enthält einen Satz von Nachrichten. Alle Nachrichten müssen sich in Warteschlangen befinden. Beachten Sie, dass der Warteschlangenname nur aus Kleinbuchstaben bestehen darf. Informationen zum Benennen von Warteschlangen finden Sie unter [Benennen von Warteschlangen und Metadaten](https://msdn.microsoft.com/library/azure/dd179349.aspx).
* **Nachricht:** Eine Nachricht in einem beliebigen Format und mit einer Größe von bis zu 64 KB. Eine Nachricht kann maximal sieben Tage in der Warteschlange verbleiben.

