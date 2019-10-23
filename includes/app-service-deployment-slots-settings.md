---
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 09/18/2019
ms.author: cephalin
ms.openlocfilehash: e00db06346b19ef85eb77626eb2ed169d2224b6c
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "71129697"
---
Wenn Sie die Konfiguration von einem anderen Bereitstellungsslot klonen, kann die geklonte Konfiguration bearbeitet werden. Bei einem Austausch werden einige Konfigurationselemente zusammen mit dem Inhalt überführt (nicht slotspezifisch), während andere Konfigurationselemente nach einem Austausch im gleichen Slot verbleiben (slotspezifisch). Im Anschluss sind die Einstellungen aufgeführt, die sich beim Austauschen der Slots ändert.

**Einstellungen, die ausgetauscht werden**:

* Allgemeine Einstellungen (z. B. Framework-Version, 32/64-Bit-Angabe, WebSockets)
* App-Einstellungen (können so konfiguriert werden, dass sie beim Slot verbleiben)
* Verbindungszeichenfolgen (können so konfiguriert werden, dass sie beim Slot verbleiben)
* Handlerzuordnungen
* Öffentliche Zertifikate
* WebJobs-Inhalte
* Hybridverbindungen *
* Virtual Network-Integration*
* Dienstendpunkte*
* Azure Content Delivery Network*

Für mit einem Sternchen (*) gekennzeichnete Features ist eine Rückgängigmachung des Austauschs geplant. 

**Einstellungen, die nicht ausgetauscht werden**:

* Veröffentlichungsendpunkte
* Benutzerdefinierte Domänennamen
* Nicht öffentliche Zertifikate und TLS/SSL-Einstellungen
* Skalierungseinstellungen
* WebJobs-Planer
* IP-Einschränkungen
* Always On
* Einstellungen für das Diagnoseprotokoll
* Ressourcenfreigabe zwischen verschiedenen Ursprüngen (Cross-Origin Resource Sharing, CORS)

> [!NOTE]
> Bestimmte App-Einstellungen, die für nicht ausgetauschte Einstellungen gelten, werden ebenfalls nicht ausgetauscht. Da z. B. die Diagnoseprotokolleinstellungen nicht ausgetauscht werden, werden verwandte App-Einstellungen wie `WEBSITE_HTTPLOGGING_RETENTION_DAYS` und `DIAGNOSTICS_AZUREBLOBRETENTIONDAYS` ebenfalls nicht ausgetauscht, auch wenn Sie nicht als Sloteinstellungen angezeigt werden.
>
