---
title: Aktivieren von Umgebungen mit mehreren VMs in Azure Lab Services | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Umgebung mit mehreren virtuellen Computern (Virtual Machines, VMs) innerhalb einer Vorlage für virtuelle Computer in einem Classroom-Lab in Azure Lab Services erstellen.
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
ms.date: 10/13/2019
ms.author: spelluru
ms.openlocfilehash: 9a86ba803f899e78b2ba9640e6cc317966969e64
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72332323"
---
# <a name="create-an-environment-with-multiple-vms-inside-a-template-vm-of-a-classroom-lab"></a>Erstellen einer Umgebung mit mehreren VMs in einer Vorlage für virtuelle Computer in einem Classroom-Lab
Momentan ermöglicht Azure Lab Services Ihnen, in einem Lab eine Vorlage für virtuelle Computer einzurichten und jedem Ihrer Benutzer jeweils eine Kopie zur Verfügung zu stellen. Wenn Sie aber ein IT-Dozent sind, der eine Klasse in der Einrichtung von Firewalls und Servern unterrichtet, müssen Sie möglicherweise für jeden Ihrer Kursteilnehmer eine Umgebung bereitstellen, in der mehrere virtuelle Computer über ein Netzwerk miteinander kommunizieren können.

Eine geschachtelte Virtualisierung ermöglicht Ihnen, innerhalb einer Vorlage für virtuelle Computer in einem Lab eine Umgebung mit mehreren virtuellen Computern zu erstellen. Wenn diese Vorlage veröffentlicht wird, wird für jeden Benutzer im Lab ein eingerichteter virtueller Computer bereitgestellt, in dem weitere virtuelle Computer eingeschlossen sind.

## <a name="what-is-nested-virtualization"></a>Was ist geschachtelte Virtualisierung?
Die geschachtelte Virtualisierung ermöglicht es Ihnen, virtuelle Computer innerhalb eines virtuellen Computers zu erstellen. Geschachtelte Virtualisierung erfolgt über Hyper-V, und steht nur für Windows-VMs zur Verfügung.

Weitere Informationen zur geschachtelten Virtualisierung finden Sie in den folgenden Artikeln:

- [Nested Virtualization in Azure (Geschachtelte Virtualisierung in Azure)](https://azure.microsoft.com/blog/nested-virtualization-in-azure/)
- [Aktivieren der geschachtelten Virtualisierung auf einer Azure-VM](../../virtual-machines/windows/nested-virtualization.md)

## <a name="use-nested-virtualization-in-azure-lab-services"></a>Verwenden der geschachtelten Virtualisierung in Azure Lab Services
Wichtig sind die folgenden Schritte:

1. Erstellen Sie für das Lab einen **Windows-Vorlagencomputer** mit der Größe **Groß**. 
2. Stellen Sie eine Verbindung zu ihm her und [aktivieren Sie die geschachtelte Virtualisierung](../../virtual-machines/windows/nested-virtualization.md).


Die folgende Prozedur enthält die ausführlichen Schritte: 

1. Erstellen Sie ein Labkonto, falls Sie noch keines besitzen. Anweisungen dazu finden Sie unter: [Tutorial: Einrichten eines Labkontos mit Azure Lab Services](tutorial-setup-lab-account.md).
1. Navigieren Sie zur Website [Azure Lab Services](https://labs.azure.com). Beachten Sie den Hinweis, dass Internet Explorer 11 noch nicht unterstützt wird. 
2. Wählen Sie **Anmelden**, und geben Sie Ihre Anmeldeinformationen ein. Azure Lab Services unterstützt Geschäfts-, Schul- oder Unikonten und Microsoft-Konten. 
3. Wählen Sie **Neues Lab** aus. 
    
    ![Erstellen eines Classroom-Labs](../media/tutorial-setup-classroom-lab/new-lab-button.png)
4. Führen Sie im Fenster **Neues Lab** die folgenden Aktionen aus: 
    1. Geben Sie unter **Name** einen Namen für Ihr Lab an. 
    2. Wählen Sie unter **Größe des virtuellen Computers** die Option **Large (nested virtualization)** (Groß (geschachtelte Virtualisierung)) oder **Mittel (geschachtelte Virtualisierung)** aus.
    6. Wählen Sie das gewünschte Windows-**Image** aus. Die geschachtelte Virtualisierung ist nur für Windows-Computer verfügbar. 
    4. Klicken Sie anschließend auf **Weiter**. 

        ![Erstellen eines Classroom-Labs](../media/how-to-enable-multi-vm-environment/new-lab-window.png)
    1. Geben Sie auf der Seite mit den **VM-Anmeldeinformationen** die Standardanmeldeinformationen für alle VMs im Lab an. Geben Sie **Name** und **Kennwort** für den Benutzer an, und wählen Sie anschließend **Weiter** aus.  

        ![Fenster „Neues Lab“](../media/tutorial-setup-classroom-lab/virtual-machine-credentials.png)

        > [!IMPORTANT]
        > Notieren Sie sich den Benutzernamen und das Kennwort. Diese Angaben werden nicht noch einmal angezeigt.
    3. Geben Sie auf der Seite mit den **Labrichtlinien** die Anzahl von Stunden ein, die jedem Benutzer (**Kontingent pro Benutzer**) außerhalb der geplanten Zeit für das Lab zugeordnet sind, und wählen Sie anschließend **Fertig stellen** aus. 

        ![Kontingent pro Benutzer](../media/tutorial-setup-classroom-lab/quota-for-each-user.png)
5. Der folgende Bildschirm sollte angezeigt werden, auf dem der Status der VM-Vorlagenerstellung angegeben ist. Die Erstellung der Vorlage im Lab dauert bis zu 20 Minuten. 

    ![Status der VM-Vorlagenerstellung](../media/tutorial-setup-classroom-lab/create-template-vm-progress.png)
1. Wählen Sie auf der Seite **Vorlage** auf der Symbolleiste die Option**Customize template** (Vorlage anpassen) aus. 

    ![Schaltfläche „Customize template“ (Vorlage anpassen)](../media/how-to-create-manage-template/customize-template-button.png)
2. Wählen Sie im Dialogfeld **Customize template** (Vorlage anpassen) die Option **Weiter** aus. Wenn Sie die Vorlage gestartet und Änderungen vorgenommen haben, hat sie nicht mehr dasselbe Setup wie die virtuellen Computer, die zuletzt für die Benutzer veröffentlicht wurden. Vorlagenänderungen werden erst nach der erneuten Veröffentlichung auf den vorhandenen virtuellen Computern der Benutzer widergespiegelt.

    ![Dialogfeld „Anpassen“](../media/how-to-create-manage-template/customize-template-dialog.png)
1. Wählen Sie auf der Symbolleiste die Schaltfläche **Connect to template** (Mit Vorlage verbinden) aus, um eine Verbindung mit der Vorlagen-VM herzustellen und die geschachtelte Virtualisierung zu konfigurieren. Befolgen Sie dazu die Anweisungen. Handelt es sich um einen Windows-Computer, wird eine Option zum Herunterladen der RDP-Datei angezeigt. 

    ![Herstellen einer Verbindung mit der Vorlage für virtuelle Computer](../media/how-to-create-manage-template/connect-template-vm.png) 
9. Richten Sie innerhalb des virtuellen Vorlagencomputers die geschachtelte Virtualisierung ein, und konfigurieren Sie ein virtuelles Netzwerk mit mehreren virtuellen Computern. Eine detaillierte Schritt-für-Schritt-Anleitung finden Sie unter [Aktivieren der geschachtelten Virtualisierung auf einer Azure-VM](../../virtual-machines/windows/nested-virtualization.md). Kurze Zusammenfassung der Schritte: 
    1. Aktivieren des Hyper-V-Features im virtuellen Vorlagencomputer
    2. Einrichten als internes virtuelles Netzwerk mit Internetverbindung für die geschachtelten virtuellen Computern
    3. Erstellen von virtuellen Computern über den Hyper-V-Manager
    4. Zuweisen einer IP-Adresse zu den virtuellen Computern
10. Wählen Sie auf der Seite **Vorlage** auf der Symbolleiste die Option **Veröffentlichen** aus. 

    ![Schaltfläche „Vorlage veröffentlichen“](../media/tutorial-setup-classroom-lab/template-page-publish-button.png)

    > [!WARNING]
    > Nachdem die Veröffentlichung erfolgt ist, kann sie nicht mehr rückgängig gemacht werden. 
8. Geben Sie auf der Seite **Vorlage veröffentlichen** die Anzahl von virtuellen Computern ein, die Sie im Lab erstellen möchten, und wählen Sie anschließend die Option **Veröffentlichen** aus. 

    ![Vorlage veröffentlichen: Anzahl von VMs](../media/tutorial-setup-classroom-lab/publish-template-number-vms.png)
11. Auf der Seite wird der **Veröffentlichungsstatus** der Vorlage angezeigt. Dieser Vorgang kann bis zu einer Stunde dauern. 

    ![Veröffentlichen der Vorlage – Status](../media/tutorial-setup-classroom-lab/publish-template-progress.png)


## <a name="next-steps"></a>Nächste Schritte

Jeder Benutzer erhält nun einen einzelnen virtuellen Computer, der eine Umgebung mit mehreren VMs enthält. Informationen zum Hinzufügen von Benutzern zum Lab und wie diesen der Registrierungslink gesendet wird, finden Sie im folgenden Artikel: [Hinzufügen von Benutzern zum Lab](tutorial-setup-classroom-lab.md#add-users-to-the-lab).