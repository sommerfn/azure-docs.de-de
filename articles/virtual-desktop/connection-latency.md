---
title: Windows Virtual Desktop – Verbindungswartezeit für Benutzer – Azure
description: Verbindungswartezeit für Benutzer von Windows Virtual Desktop.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: helohr
ms.openlocfilehash: 7ef35bdf6c7470d425826d7a30755cc216e69158
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73164068"
---
# <a name="determine-user-connection-latency-in-windows-virtual-desktop"></a>Ermitteln der Verbindungswartezeit für Benutzer in Windows Virtual Desktop

Windows Virtual Desktop ist global verfügbar. Administratoren können virtuelle Computer (Virtual Machines, VMS) in jeder gewünschten Azure-Region erstellen. Die Verbindungswartezeit hängt vom Standort der Benutzer und der virtuellen Computer ab. Windows Virtual Desktop-Dienste werden fortlaufend in neuen geografischen Regionen eingeführt, um die Wartezeit zu verbessern. 
 
Mit dem [Tool „Einschätzung der Servicequalität für Windows Virtual Desktop“](https://azure.microsoft.com/services/virtual-desktop/assessment/) können Sie den besten Standort zum Optimieren der Wartezeit Ihrer virtuellen Computer ermitteln. Es wird empfohlen, das Tool alle zwei bis drei Monate zu verwenden, um sicherzustellen, dass sich der optimale Standort nicht geändert hat, während Windows Virtual Desktop in neuen Bereichen eingeführt wird. 

## <a name="azure-traffic-manager"></a>Azure Traffic Manager

Windows Virtual Desktop verwendet den Azure Traffic Manager, der den Standort des DNS-Servers des Benutzers prüft, um die nächstgelegene Windows Virtual Desktop-Dienstinstanz zu finden. Administratoren wird empfohlen, diesen Standort zu überprüfen, bevor sie den Standort für die virtuellen Computer auswählen.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum besten Standort für eine optimale Wartezeit finden Sie unter [Tool „Einschätzung der Servicequalität für Windows Virtual Desktop“](https://azure.microsoft.com/services/virtual-desktop/assessment/).
- Tarife finden Sie unter [Windows Virtual Desktop – Preise](https://azure.microsoft.com/pricing/details/virtual-desktop/).
- Die ersten Schritte mit Ihrer Windows Virtual Desktop-Bereitstellung finden Sie in [unserem Tutorial](tenant-setup-azure-active-directory.md).