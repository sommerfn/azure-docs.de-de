---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: 8a6ceee0f7db4b7b368dce1a28c8edc0b2b11180
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2019
ms.locfileid: "67178733"
---
Für ruhende Daten:

- BitLocker XTS-AES-256-Bit-Verschlüsselung wird verwendet, um lokale Daten zu schützen.
- Der Zugriff auf Daten in Dateifreigaben ist beschränkt.

    - SMB-Clients, die auf Freigabedaten zugreifen, benötigen Benutzeranmeldeinformationen, die der Freigabe zugeordnet sind. Diese Anmeldeinformationen werden definiert, wenn die Freigabe erstellt wird.
    - Die IP-Adressen von NFS-Clients, die auf eine Freigabe zugreifen, müssen hinzugefügt werden, wenn die Freigabe erstellt wird.