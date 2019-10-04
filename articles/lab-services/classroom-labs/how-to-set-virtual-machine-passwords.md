---
title: Festlegen von Kennwörtern für VMs in Azure Lab Services | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Kennwörter für virtuelle Computer (VMs) in Classroom-Labs mit Azure Lab Services festlegen und zurücksetzen.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/20/2019
ms.author: spelluru
ms.openlocfilehash: a4cb2abec429a790f493f95d3d16b2ff7b3eb445
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2019
ms.locfileid: "69645028"
---
# <a name="set-or-reset-password-for-virtual-machines-in-classroom-labs-instructor"></a>Festlegen oder Zurücksetzen von Kennwörtern für virtuelle Computer in Classroom-Labs (Kursleiter)
Ein Lab-Besitzer (Kursleiter) kann die Kennwörter für virtuelle Computer zum Zeitpunkt der Erstellung des Labs (Assistent für Lab-Erstellung) oder nach dem Erstellen des Labs (auf dem Dashboard) festlegen und zurücksetzen. 

## <a name="set-password-at-the-time-of-lab-creation"></a>Festlegen des Kennworts zum Zeitpunkt der Lab-Erstellung
Ein Lab-Besitzer (Kursleiter) kann auf der Seite **Anmeldeinformationen festlegen** im Assistenten zur Lab-Erstellung ein Kennwort für die virtuellen Computer im Lab festlegen.

![Festlegen von Anmeldeinformationen](../media/tutorial-setup-classroom-lab/set-credentials.png)

Durch das Aktivieren/Deaktivieren der Option **Use same password for all virtual machines** (Dasselbe Kennwort für alle virtuellen Computer verwenden) auf dieser Seite der Kursleiter festlegen, ob für alle virtuellen Computer im Lab dasselbe Kennwort verwendet werden soll oder ob die Kursteilnehmer eigene Kennwörter für ihre virtuellen Computer festlegen können. Standardmäßig ist diese Einstellung für alle Images mit den Betriebssystemen Windows und Linux mit Ausnahme von Ubuntu aktiviert. Wenn diese Einstellung deaktiviert ist, werden die Kursteilnehmer bei der ersten Verbindung mit der VM aufgefordert, ein Kennwort festzulegen. 

Der Lab-Besitzer kann dieses Kennwort (bei Bedarf) auf der Seite **Vorlage konfigurieren** im Assistenten zur Lab-Erstellung zurücksetzen. 

![Seite „Vorlage konfigurieren“ nach Abschluss des Vorgangs](../media/tutorial-setup-classroom-lab/configure-template-after-complete.png)

Der Lab-Besitzer kann das Kennwort auch nach der Lab-Erstellung auf dem Dashboard zurücksetzen. 

## <a name="reset-password-on-the-dashboard"></a>Zurücksetzen des Kennworts auf dem Dashboard

1. Wählen Sie das Überlaufmenü (vertikale drei Punkte) auf der Lab-Kachel und dann **Kennwort zurücksetzen** aus. 

    ![Zurücksetzen des Kennworts im Menü auf der Startseite](../media/how-to-set-virtual-machine-passwords/reset-password-menu-dashboard.png)
1. Geben Sie im Dialogfeld **Kennwort festlegen** ein Kennwort ein, und wählen Sie **Kennwort festlegen** aus.
    
    ![Dialogfeld „Kennwort festlegen“](../media/how-to-set-virtual-machine-passwords/set-password.png)

## <a name="next-steps"></a>Nächste Schritte
Informationen zu anderen Verwendungsoptionen für Kursteilnehmer, die Sie (als Lab-Besitzer) konfigurieren können, finden Sie im folgenden Artikel: [Konfigurieren von Nutzungseinstellungen und Richtlinien](how-to-configure-student-usage.md).

Informationen dazu, wie Kursteilnehmer Kennwörter für ihre virtuellen Computer zurücksetzen können, finden Sie unter [Festlegen oder Zurücksetzen von Kennwörtern für virtuelle Computer in Classroom-Labs (Kursteilnehmer)](how-to-set-virtual-machine-passwords-student.md).