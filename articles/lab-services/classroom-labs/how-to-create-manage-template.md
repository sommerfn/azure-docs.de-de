---
title: Verwalten einer Vorlage eines Classroom-Labs in Azure Lab Services | Microsoft-Dokumentation
description: Informationen zum Erstellen und Verwalten einer Vorlage eines Classroom-Labs in Azure Lab Services.
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
ms.date: 10/12/2019
ms.author: spelluru
ms.openlocfilehash: f0fc1e143ce7d271d5faaa8dda0eb40cdfc9e006
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72332743"
---
# <a name="create-and-manage-a-classroom-template-in-azure-lab-services"></a>Erstellen und Verwalten einer Classroom-Vorlage in Azure Lab Services
Eine Vorlage in einem Lab ist ein VM-Basisimage und dient zur Erstellung der virtuellen Computer aller Benutzer. Richten Sie den virtuellen Computer der Vorlage so ein, dass er genau das enthält, was Sie den Lab-Benutzern zur Verfügung stellen möchten. Sie können einen Namen und eine Beschreibung der Vorlage angeben, die den Lab-Benutzern angezeigt werden. Anschließend veröffentlichen Sie die Vorlage, um Instanzen der Vorlagen-VM für Ihre Lab-Benutzer zur Verfügung zu stellen. Wenn Sie eine Vorlage veröffentlichen, werden von Azure Lab Services im Lab mithilfe der Vorlage virtuelle Computer erstellt. Die Anzahl der in diesem Vorgang erstellten virtuellen Computer entspricht der maximalen Anzahl von Benutzern im Lab, die Sie in der Nutzungsrichtlinie des Labs festlegen können. Alle virtuellen Computer haben die gleiche Konfiguration wie die Vorlage.

Dieser Artikel beschreibt das Erstellen und Verwalten einer Vorlagen-VM in einem Classroom-Lab von Azure Lab Services. 

## <a name="publish-a-template-while-creating-a-classroom-lab"></a>Veröffentlichen einer Vorlage beim Erstellen eines Classroom-Labs
Informationen zum Veröffentlichen einer Vorlage während der Erstellung eines Classroom-Labs finden Sie unter [Verwalten von Classroom-Labs in Azure Lab Services](how-to-manage-classroom-labs.md#create-a-classroom-lab).
 
## <a name="set-or-update-template-title-and-description"></a>Festlegen oder Aktualisieren von Vorlagentitel und -beschreibung
Gehen Sie wie folgt vor, um Titel und Beschreibung erstmals festzulegen und später zu aktualisieren. 

1. Geben Sie auf der Seite **Vorlage** den neuen **Titel** für das Lab ein.  
2. Geben Sie die neue **Beschreibung** für die Vorlage ein. Wenn Sie den Fokus aus dem Textfeld verschieben, wird es automatisch gespeichert. 

    ![Vorlagenname und -beschreibung](../media/how-to-create-manage-template/template-name-description.png)

## <a name="update-a-template-vm"></a>Aktualisieren einer Vorlage für virtuelle Computer
Gehen Sie wie folgt vor, um eine Vorlage für virtuelle Computer zu aktualisieren:  

1. Wählen Sie auf der Seite **Vorlage** auf der Symbolleiste die Option**Customize template** (Vorlage anpassen) aus. 

    ![Schaltfläche „Customize template“ (Vorlage anpassen)](../media/how-to-create-manage-template/customize-template-button.png)
2. Wählen Sie im Dialogfeld **Customize template** (Vorlage anpassen) die Option **Weiter** aus. Wenn Sie die Vorlage gestartet und Änderungen vorgenommen haben, hat sie nicht mehr dasselbe Setup wie die virtuellen Computer, die zuletzt für die Benutzer veröffentlicht wurden. Vorlagenänderungen werden erst nach der erneuten Veröffentlichung auf den vorhandenen virtuellen Computern der Benutzer widergespiegelt.

    ![Dialogfeld „Anpassen“](../media/how-to-create-manage-template/customize-template-dialog.png)
1. Wählen Sie auf der Symbolleiste die Schaltfläche **Connect to template** (Mit Vorlage verbinden) aus, um eine Verbindung mit der Vorlage für virtuelle Computer herzustellen, und befolgen Sie die Anweisungen. Handelt es sich um einen Windows-Computer, wird eine Option zum Herunterladen der RDP-Datei angezeigt. 

    ![Herstellen einer Verbindung mit der Vorlage für virtuelle Computer](../media/how-to-create-manage-template/connect-template-vm.png)
1. Installieren Sie die Software, die die Teilnehmer für das Lab benötigen (z.B. Visual Studio, Azure Storage-Explorer usw.). 
2. Trennen Sie die Verbindung mit der Vorlage für virtuelle Computer (indem Sie die Remotedesktopsitzung schließen). 
3. **Beenden** Sie die Vorlage für virtuelle Computer, indem Sie **Stop template** (Vorlage beenden) auswählen. 
4. Führen Sie die Schritte im nächsten Abschnitt aus, um die aktualisierte Vorlage für virtuelle Computer zu **veröffentlichen**. 

## <a name="publish-the-template-vm"></a>Veröffentlichen der Vorlage für virtuelle Computer  
Wenn Sie die Vorlage nicht beim Erstellen des Labs veröffentlichen, können Sie dies später nachholen. Vor der Veröffentlichung empfiehlt es sich, eine Verbindung mit der Vorlagen-VM herzustellen und sie ggf. mit Software zu aktualisieren. Wenn Sie eine Vorlage veröffentlichen, werden von Azure Lab Services im Lab mithilfe der Vorlage virtuelle Computer erstellt. Die Anzahl der in diesem Vorgang erstellten virtuellen Computer ist die Anzahl der virtuellen Computer, die Sie bei der ersten Veröffentlichung bzw. auf der Seite „VM-Pool“ angegeben haben. Alle virtuellen Computer haben die gleiche Konfiguration wie die Vorlage. 

1. Wählen Sie auf der Seite **Vorlage** auf der Symbolleiste die Option **Veröffentlichen** aus. 
1. Prüfen Sie die Meldung im Meldungsfeld **Vorlage veröffentlichen**, und wählen Sie **Veröffentlichen**. Dieser Vorgang kann einige Zeit dauern, je nachdem, wie viele virtuelle Computer erstellt werden.

    ![Schaltfläche "Veröffentlichen"](../media/how-to-create-manage-template/publish-button.png)

    > [!IMPORTANT]
    > Sobald eine Vorlage veröffentlicht wurde, kann dies nicht mehr rückgängig gemacht werden. Sie können die Vorlage jedoch erneut veröffentlichen. 
4. Sie können den Status des Veröffentlichungsvorgangs auf der Seite „Vorlage“ verfolgen. Warten Sie, bis der Status der Vorlage in **Veröffentlicht** geändert wird. 

    ![Veröffentlichungsstatus](../media/how-to-create-manage-template/publish-status.png)
1. Wechseln Sie zur Seite **Virtuelle Computer**, und vergewissern Sie sich, dass virtuelle Computer mit dem Status **Nicht zugewiesen** angezeigt werden. Diese virtuellen Computer sind noch keinen Teilnehmern zugewiesen. Warten Sie, bis die virtuellen Computer erstellt wurden. Sie sollten den Status **Beendet** aufweisen. Auf dieser Seite können Sie einen virtuellen Computer für einen Teilnehmer starten, eine Verbindung damit herstellen und ihn beenden und löschen. Sie können virtuelle Computer auf dieser Seite starten oder sie von Ihren Kursteilnehmern starten lassen. 

    ![Virtuelle Computer im Status „Beendet“](../media/tutorial-setup-classroom-lab/virtual-machines-stopped.png)


## <a name="next-steps"></a>Nächste Schritte
Entsprechende Informationen finden Sie in den folgenden Artikeln:

- [Erstellen und Verwalten von Labkonten als Administrator](how-to-manage-lab-accounts.md)
- [Erstellen und Verwalten von Labs als Labbesitzer](how-to-manage-classroom-labs.md)
- [Konfigurieren und Steuern der Nutzung eines Labs als Labbesitzer](how-to-configure-student-usage.md)
- [Zugreifen auf ein Classroom-Lab in Azure Lab Services](how-to-use-classroom-lab.md) (als Labbenutzer)
