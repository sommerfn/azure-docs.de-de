---
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: include
ms.date: 07/11/2019
ms.author: alkohli
ms.openlocfilehash: 4a3925752d1af5e43d5984b06c0a68aa9faa214b
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67839762"
---
| Port-Nr.| ein oder aus | Portbereich| Erforderlich| Notizen |   |
|--------|-----|-----|-----------|----------|-----------|
| TCP 80 (HTTP)|Geben Sie in|LAN|Ja|Dieser Port wird zum Herstellen einer Verbindung mit REST-APIs des Data Box-Blobspeichers über HTTP verwendet. Wenn keine Verbindung zu REST-APIs hergestellt wird, wird automatisch eine Umleitung zur lokalen Webbenutzeroberfläche über Port 8443 durchgeführt. |
| TCP 443 (HTTPS)|Geben Sie in|LAN|Ja|Dieser Port wird zum Herstellen einer Verbindung mit REST-APIs des Data Box-Blobspeichers über HTTPS verwendet. Wenn keine Verbindung zu REST-APIs hergestellt wird, wird automatisch eine Umleitung zur lokalen Webbenutzeroberfläche über Port 8443 durchgeführt. |
| TCP 8443 (HTTPS-Alt)|Geben Sie in|LAN|Ja|Dies ist ein alternativer Port für HTTPS, der verwendet wird, wenn eine Verbindung mit der lokalen Webbenutzeroberfläche für die Geräteverwaltung hergestellt wird. |
| TCP 445 (SMB)|Aus/Ein|LAN|In einigen Fällen<br>Siehe Hinweise|Dieser Port ist nur dann erforderlich, wenn Sie eine Verbindung über SMB herstellen. |
| TCP 2049 (NFS)|Aus/Ein|LAN|In einigen Fällen<br>Siehe Hinweise|Dieser Port ist nur dann erforderlich, wenn Sie eine Verbindung über NFS herstellen. |
| TCP 111 (NFS)|Aus/Ein|LAN|In einigen Fällen<br>Siehe Hinweise|Dieser Port wird für rpcbind-/Portzuordnung verwendet und ist nur erforderlich, wenn Sie eine Verbindung per NFS herstellen.  |
