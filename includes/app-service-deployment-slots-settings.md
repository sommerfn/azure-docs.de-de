---
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 06/18/2019
ms.author: cephalin
ms.openlocfilehash: 0691b1a531ffebbb2c368bdb37dd4d8025fb4a4e
ms.sourcegitcommit: 55e0c33b84f2579b7aad48a420a21141854bc9e3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2019
ms.locfileid: "69623706"
---
Wenn Sie die Konfiguration von einem anderen Bereitstellungsslot klonen, kann die geklonte Konfiguration bearbeitet werden. Bei einem Austausch werden einige Konfigurationselemente zusammen mit dem Inhalt überführt (nicht slotspezifisch), während andere Konfigurationselemente nach einem Austausch im gleichen Slot verbleiben (slotspezifisch). Im Anschluss sind die Einstellungen aufgeführt, die sich beim Austauschen der Slots ändert.

**Einstellungen, die ausgetauscht werden**:

* Allgemeine Einstellungen (z. B. Framework-Version, 32/64-Bit-Angabe, WebSockets)
* App-Einstellungen (können so konfiguriert werden, dass sie beim Slot verbleiben)
* Verbindungszeichenfolgen (können so konfiguriert werden, dass sie beim Slot verbleiben)
* Handlerzuordnungen
* Überwachungs- und Diagnoseeinstellungen
* Öffentliche Zertifikate
* WebJobs-Inhalte
* Hybridverbindungen *
* Virtual Network-Integration*
* Dienstendpunkte*
* Azure Content Delivery Network*

Für mit einem Sternchen (*) gekennzeichnete Features ist eine feste Slotzuordnung geplant. 

**Einstellungen, die nicht ausgetauscht werden**:

* Veröffentlichungsendpunkte
* Benutzerdefinierte Domänennamen
* Private Zertifikate und SSL-Bindungen
* Skalierungseinstellungen
* WebJobs-Planer
* IP-Einschränkungen
* Always On
* Protokolleinstellungen (HTTPS, TLS-Version, Clientzertifikate)
* Einstellungen für das Diagnoseprotokoll
* Ressourcenfreigabe zwischen verschiedenen Ursprüngen (Cross-Origin Resource Sharing, CORS)

<!-- VNET and hybrid connections not yet sticky to slot -->