---
title: Verschieben der Azure Site Recovery-Konfiguration in eine andere Azure-Region | Microsoft-Dokumentation
description: Leitfaden zum Verschieben einer Site Recovery-Konfiguration in eine andere Azure-Region
services: site-recovery
author: rajani-janaki-ram
ms.service: site-recovery
ms.topic: tutorial
ms.date: 07/31/2019
ms.author: rajanaki
ms.custom: MVC
ms.openlocfilehash: ba0e2577d6fb8bac66322917936fe06825af0d96
ms.sourcegitcommit: 6ad03fa28a0f60cb6dce6144f728c2ceb56ff6e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2019
ms.locfileid: "68708463"
---
# <a name="moving-recovery-services-vault-and--azure-site-recovery-configuration-to-another-azure-region"></a>Verschieben eines Recovery Services-Tresors und der Azure Site Recovery-Konfiguration in eine andere Azure-Region

Es kann verschiedene Szenarien geben, in denen Sie Ihre vorhandenen Azure-Ressourcen aus einer Region in eine andere verschieben möchten. Die Gründe hierfür können eine bessere Verwaltbarkeit, Governance oder Fusionen bzw. Übernahmen oder ähnliche Fälle sein. Eine der zugehörigen Ressourcen, die Sie bei einer Verschiebung Ihrer Azure-VMs ggf. ebenfalls verschieben sollten, ist die entsprechende Konfiguration der Notfallwiederherstellung. Es gibt keine allgemeingültige Möglichkeit, eine vorhandene Konfiguration für die Notfallwiederherstellung aus einer Region in eine andere zu verschieben. Der Hauptgrund hierfür ist, dass Sie Ihre Zielregion unter Umständen basierend auf Ihrer VM-Quellregion konfiguriert haben. Wenn Sie sich für eine Änderung dieser Konfiguration entscheiden, können die vorherigen Konfigurationen der Zielregion nicht wiederverwendet werden und müssen zurückgesetzt werden. In diesem Artikel wird der Schritt-für-Schritt-Prozess zum erneuten Konfigurieren der Einrichtung für die Notfallwiederherstellung und Verschieben in eine andere Region beschrieben.

In diesem Dokument erfahren Sie Folgendes:

> [!div class="checklist"]
> * Überprüfen der Voraussetzungen für das Verschieben
> * Identifizieren der Ressourcen, die von Azure Site Recovery verwendet wurden 
> * Deaktivieren der Replikation
> * Löschen der Ressourcen 
> * Richten Sie die Sitewiederherstellung basierend auf der neuen Quellregion für die VMs neu ein.

> [!IMPORTANT]
> Derzeit gibt es keine allgemeingültige Möglichkeit zum Verschieben eines Recovery Services-Tresors und der Konfiguration für die Notfallwiederherstellung im unveränderten Zustand in eine andere Region. In diesem Dokument wird beschrieben, wie die Replikation deaktiviert und in der neuen Region dann erneut eingerichtet wird.

## <a name="prerequisites"></a>Voraussetzungen

- Stellen Sie sicher, dass Sie die Konfiguration für die Notfallwiederherstellung entfernen und löschen, bevor Sie versuchen, die Azure-VMs in eine andere Region zu verschieben. 

> [!NOTE]
> Wenn Ihre neue Zielregion für die Azure-VM der Zielregion für die Notfallwiederherstellung entspricht, können Sie Ihre vorhandene Replikationskonfiguration verwenden und mit den [hier angegebenen Schritten](azure-to-azure-tutorial-migrate.md) verschieben. 

- Stellen Sie sicher, dass Sie eine fundierte Entscheidung treffen und dass alle Beteiligten informiert sind. Ihre VM ist nämlich erst wieder vor Notfällen geschützt, nachdem die Verschiebung der VM abgeschlossen ist. 

## <a name="identify-the-resources-that-were-used-by-azure-site-recovery"></a>Identifizieren der Ressourcen, die von Azure Site Recovery verwendet wurden
Wir empfehlen Ihnen, diesen Schritt auszuführen, bevor Sie mit dem nächsten Schritt fortfahren. Es ist einfacher, die relevanten Ressourcen zu identifizieren, während die VMs noch repliziert werden.

Navigieren Sie für alle Azure-VMs, die repliziert werden, zu **Geschützte Elemente** > **Replizierte Elemente**>**Eigenschaften**, und identifizieren Sie die folgenden Ressourcen.

- Zielressourcengruppe
- Cachespeicherkonto
- Zielspeicherkonto (bei nicht verwalteter datenträgerbasierter Azure-VM) 
- Zielnetzwerk


## <a name="disable-existing-disaster-recovery-configuration"></a>Deaktivieren der vorhandenen Konfiguration für die Notfallwiederherstellung

1. Navigieren Sie zum **Recovery Services-Tresor**. 
2.  Klicken Sie unter **Geschützte Elemente** > **Replizierte Elemente** mit der rechten Maustaste auf den Computer, und klicken Sie dann auf **Replikation deaktivieren**.
3. Wiederholen Sie diesen Schritt für alle VMs, die Sie verschieben möchten.
> [!NOTE]
> Der Mobility Service wird nicht von den geschützten Servern deinstalliert, Sie müssen ihn manuell deinstallieren. Wenn Sie planen, den Server erneut zu schützen, können Sie die Deinstallation des Mobility Service überspringen.

## <a name="delete-the-resources"></a>Löschen der Ressourcen

1. Navigieren Sie zum **Recovery Services-Tresor**.
2. Klicken Sie auf **Löschen**.
3. Fahren Sie mit dem Löschen aller anderen Ressourcen fort, die im [vorherigen Schritt](#identify-the-resources-that-were-used-by-azure-site-recovery) identifiziert wurden.
 
## <a name="move-azure-vms-to-the-new-target-region"></a>Verschieben von Azure-VMs in die neue Zielregion

Führen Sie die unten angegebenen Schritte je nach Bedarf aus, um Azure-VMs in die Zielregion zu verschieben.

- [Verschieben virtueller Azure-Computer in eine andere Region](azure-to-azure-tutorial-migrate.md)
- [Move Azure VMs into Availability Zones](move-azure-VMs-AVset-Azone.md) (Verschieben virtueller Azure-Computer in Verfügbarkeitszonen)

## <a name="re-set-up-site-recovery-based-on-the-new-source-region-for-the-vms"></a>Richten Sie Site Recovery basierend auf der neuen Quellregion für die VMs neu ein.

Konfigurieren Sie die Notfallwiederherstellung für die Azure-VMs, die in die neue Region verschoben wurden, indem Sie [diese Schritte](azure-to-azure-tutorial-enable-replication.md) ausführen.
