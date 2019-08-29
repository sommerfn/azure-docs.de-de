---
title: Durchführen eines Betriebssystemupgrades für SAP HANA in Azure (große Instanzen) | Microsoft-Dokumentation
description: Durchführen von Betriebssystemupgrades für SAP HANA in Azure (große Instanzen)
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: gwallace
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/04/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6341f58791c2fad71a65650e32cff02fb52d78c0
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70098683"
---
# <a name="operating-system-upgrade"></a>Betriebssystemupgrade
In diesem Dokument werden die Details zu Betriebssystemupgrades für SAP HANA (große Instanzen) beschrieben.

>[!NOTE]
>Das Betriebssystemupgrade ist Zuständigkeit des Kunden. Der Microsoft Operations-Support kann Sie dabei auf wichtige Punkte hinweisen, die beim Upgrade zu beachten sind. Bevor Sie Planungen für ein Upgrade treffen, sollten Sie auch den Hersteller Ihres Betriebssystems zurate ziehen.

Das Microsoft Operations-Team installiert das Betriebssystem bei der Bereitstellung der HLI-Einheiten. Im Lauf der Zeit müssen Sie Wartungsvorgänge für das Betriebssystem für die HLI-Einheit vornehmen (z.B. Patches erstellen, optimieren, Upgrades durchführen).

Bevor Sie umfassende Änderungen am Betriebssystem vornehmen (z.B. ein Upgrade von SP1 auf SP2), müssen Sie sich zunächst an das Microsoft Operations-Team wenden, indem Sie ein Supportticket öffnen.

Geben Sie folgende Informationen in Ihrem Ticket an:

* Ihre HLI-Abonnement-ID
* Ihren Servernamen
* Die Patchebene, die Sie anwenden möchten
* Das Datum, an dem Sie diese Änderung planen 

Es empfiehlt sich, dieses Ticket mindestens eine Woche vor dem gewünschten Datum des Upgrades zu öffnen, damit das Operations-Team überprüfen kann, ob ein Firmwareupgrade auf Ihrer Serverblade erforderlich ist.


Die Supportmatrix der anderen SAP HANA-Versionen mit den verschiedenen Linux-Versionen finden Sie im [SAP-Hinweis 2235581](https://launchpad.support.sap.com/#/notes/2235581).


## <a name="known-issues"></a>Bekannte Probleme

Im Folgenden werden einige bekannte Probleme bei Upgrades erläutert:
- Bei SKUs von SKU-Typ II wird Software Foundation Software (SFS) nach der Durchführung eines Betriebssystemupgrades entfernt. Nach dem Betriebssystemupgrade müssen Sie die kompatible SFS-Version neu installieren.
- Für Ethernet-Kartentreiber (ENIC und FNIC) wird ein Rollback auf die ältere Version ausgeführt. Nach dem Upgrade müssen Sie die kompatible Version der Treiber neu installieren.

## <a name="next-steps"></a>Nächste Schritte
- Informationen zur SKU von Typ I für Betriebssystemsicherungen finden Sie unter [Sicherung und Wiederherstellung](hana-overview-high-availability-disaster-recovery.md).
- Unter [OS-Sicherung und -Wiederherstellung für Typ-II-SKUs](os-backup-type-ii-skus.md) finden Sie weitere Informationen zu SKU-Klassen des Typs II.
