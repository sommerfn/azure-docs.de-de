---
title: Verschieben eines Azure Site Recovery-Tresors in eine andere Region
description: Es wird beschrieben, wie Sie einen Recovery Services-Tresor (Azure Site Recovery) in eine andere Azure-Region verschieben.
services: site-recovery
author: rajani-janaki-ram
ms.service: site-recovery
ms.topic: tutorial
ms.date: 07/31/2019
ms.author: rajanaki
ms.custom: MVC
ms.openlocfilehash: 32dff9a165125ab1949560ce36438ae266cd3036
ms.sourcegitcommit: a170b69b592e6e7e5cc816dabc0246f97897cb0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2019
ms.locfileid: "74090299"
---
# <a name="move-a-recovery-services-vault-and-azure-site-recovery-configuration-to-another-azure-region"></a>Verschieben eines Recovery Services-Tresors und der Azure Site Recovery-Konfiguration in eine andere Azure-Region

Es gibt verschiedene Szenarien, in denen Sie Ihre vorhandenen Azure-Ressourcen aus einer Region in eine andere verschieben möchten. Gründe dafür können Verwaltbarkeit, Governance oder Unternehmensfusionen und -übernahmen sein. Eine der zugehörigen Ressourcen, die Sie bei einer Verschiebung Ihrer Azure-VMs ggf. ebenfalls verschieben sollten, ist die Konfiguration der Notfallwiederherstellung. 

Es gibt keine allgemeingültige Möglichkeit, eine vorhandene Konfiguration für die Notfallwiederherstellung aus einer Region in eine andere zu verschieben. Das liegt daran, dass Sie Ihre Zielregion basierend auf der VM-Quellregion konfiguriert haben. Wenn Sie die Quellregion ändern möchten, können die zuvor vorhandenen Konfigurationen der Zielregion nicht wiederverwendet werden und müssen zurückgesetzt werden. In diesem Artikel wird der Schritt-für-Schritt-Prozess zum erneuten Konfigurieren der Einrichtung für die Notfallwiederherstellung und Verschieben in eine andere Region beschrieben.

In diesem Dokument erfahren Sie Folgendes:

> [!div class="checklist"]
> * Überprüfen der Voraussetzungen für das Verschieben
> * Identifizieren der Ressourcen, die von Azure Site Recovery verwendet wurden
> * Deaktivieren der Replikation
> * Löschen der Ressourcen
> * Einrichten von Site Recovery basierend auf der neuen Quellregion für die VMs

> [!IMPORTANT]
> Derzeit gibt es keine allgemeingültige Möglichkeit, einen Recovery Services-Tresor und die Konfiguration für die Notfallwiederherstellung in der vorliegenden Form in eine andere Region zu verschieben. In diesem Artikel wird die Vorgehensweise zum Deaktivieren der Replikation und zum Einrichten der Replikation in der neuen Region beschrieben.

## <a name="prerequisites"></a>Voraussetzungen

- Stellen Sie sicher, dass Sie die Konfiguration für die Notfallwiederherstellung entfernen und löschen, bevor Sie versuchen, die Azure-VMs in eine andere Region zu verschieben. 

  > [!NOTE]
  > Wenn Ihre neue Zielregion für die Azure-VM der Zielregion für die Notfallwiederherstellung entspricht, können Sie Ihre vorhandene Replikationskonfiguration verwenden und verschieben. Führen Sie die Schritte unter [Verschieben virtueller Azure-Computer in eine andere Region](azure-to-azure-tutorial-migrate.md) aus.

- Stellen Sie sicher, dass Sie eine fundierte Entscheidung treffen und alle Beteiligten informiert sind. Ihr virtueller Computer ist erst wieder vor Notfällen geschützt, wenn die Verschiebung abgeschlossen ist.

## <a name="identify-the-resources-that-were-used-by-azure-site-recovery"></a>Identifizieren der Ressourcen, die von Azure Site Recovery verwendet wurden
Es wird empfohlen, diesen Schritt auszuführen, bevor Sie mit dem nächsten Schritt fortfahren. Es ist einfacher, die relevanten Ressourcen zu identifizieren, während die VMs repliziert werden.

Navigieren Sie für alle Azure-VMs, die repliziert werden, zu **Geschützte Elemente** > **Replizierte Elemente** > **Eigenschaften**, und identifizieren Sie die folgenden Ressourcen:

- Zielressourcengruppe
- Cachespeicherkonto
- Zielspeicherkonto (bei nicht verwalteter datenträgerbasierter Azure-VM) 
- Zielnetzwerk


## <a name="disable-the-existing-disaster-recovery-configuration"></a>Deaktivieren der vorhandenen Konfiguration für die Notfallwiederherstellung

1. Navigieren Sie zum Recovery Services-Tresor.
2. Klicken Sie unter **Geschützte Elemente** > **Replizierte Elemente** mit der rechten Maustaste auf den Computer, und wählen Sie dann **Replikation deaktivieren** aus.
3. Wiederholen Sie diesen Schritt für alle VMs, die Sie verschieben möchten.

> [!NOTE]
> Der Mobilitätsdienst wird auf den geschützten Servern nicht deinstalliert. Sie müssen ihn manuell deinstallieren. Wenn Sie planen, den Server erneut zu schützen, können Sie die Deinstallation des Mobility Service überspringen.

## <a name="delete-the-resources"></a>Löschen der Ressourcen

1. Navigieren Sie zum Recovery Services-Tresor.
2. Klicken Sie auf **Löschen**.
3. Löschen Sie alle anderen Ressourcen, die Sie [zuvor identifiziert](#identify-the-resources-that-were-used-by-azure-site-recovery) haben.
 
## <a name="move-azure-vms-to-the-new-target-region"></a>Verschieben von Azure-VMs in die neue Zielregion

Führen Sie die Schritte in den folgenden Artikeln je nach Bedarf aus, um Azure-VMs in die Zielregion zu verschieben:

- [Verschieben virtueller Azure-Computer in eine andere Region](azure-to-azure-tutorial-migrate.md)
- [Move Azure VMs into Availability Zones](move-azure-VMs-AVset-Azone.md) (Verschieben virtueller Azure-Computer in Verfügbarkeitszonen)

## <a name="set-up-site-recovery-based-on-the-new-source-region-for-the-vms"></a>Einrichten von Site Recovery basierend auf der neuen Quellregion für die VMs

Konfigurieren Sie die Notfallwiederherstellung für die Azure-VMs, die in die neue Region verschoben wurden. Führen Sie dazu die Schritte unter [Einrichten der Notfallwiederherstellung für Azure-VMs](azure-to-azure-tutorial-enable-replication.md) aus.
