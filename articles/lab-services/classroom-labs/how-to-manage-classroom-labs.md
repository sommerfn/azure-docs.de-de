---
title: Verwalten von Classroom-Labs in Azure Lab Services | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie ein Classroom-Lab erstellen, alle Classroom-Labs anzeigen, den Registrierungslink für einen Lab-Benutzer freigeben oder ein Lab löschen.
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
ms.date: 06/07/2019
ms.author: spelluru
ms.openlocfilehash: 6ba41132c93ebdb2578bafb100416ca3fe579298
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67123280"
---
# <a name="manage-classroom-labs-in-azure-lab-services"></a>Verwalten von Classroom-Labs in Azure Lab Services 
In diesem Artikel erfahren Sie, wie Sie ein Classroom-Lab erstellen und löschen. Außerdem erfahren Sie, wie Sie alle Classroom-Labs in einem Labkonto anzeigen. 

## <a name="prerequisites"></a>Voraussetzungen
Zum Einrichten eines Classroom-Labs in einem Labkonto müssen Sie Mitglied der Rolle **Ersteller des Labs** für das Labkonto sein. Das zum Erstellen eines Lab-Kontos verwendete Konto wird dieser Rolle automatisch hinzugefügt. Ein Besitzer des Labs kann der Rolle „Ersteller des Labs“ weitere Benutzer hinzufügen, indem er die Schritte im folgenden Artikel ausführt: [Hinzufügen eines Benutzers zur Rolle „Ersteller des Labs“](tutorial-setup-lab-account.md#add-a-user-to-the-lab-creator-role).

## <a name="create-a-classroom-lab"></a>Erstellen eines Classroom-Labs

1. Navigieren Sie zur Website [Azure Lab Services](https://labs.azure.com). Beachten Sie den Hinweis, dass Internet Explorer 11 noch nicht unterstützt wird. 
2. Wählen Sie **Anmelden**. Wählen Sie eine **Benutzer-ID** aus (oder geben Sie eine Benutzer-ID ein), die der Rolle **Lab-Ersteller** im Labkonto angehört, und geben Sie das Kennwort ein. Azure Lab Services unterstützt Geschäfts-, Schul- oder Unikonten und Microsoft-Konten. 
3. Führen Sie im Fenster **Neues Lab** die folgenden Aktionen aus: 
    1. Geben Sie unter **Name** einen Namen für Ihr Lab an. 
    2. Geben Sie die maximale **Anzahl von virtuellen Computern** im Lab an. Sie können die Anzahl der virtuellen Computer im Lab später erhöhen oder verringern. 
    6. Wählen Sie **Speichern** aus.

        ![Erstellen eines Classroom-Labs](../media/tutorial-setup-classroom-lab/new-lab-window.png)
4. Führen Sie auf der Seite **Select virtual machine specifications** (Spezifikationen für virtuellen Computer auswählen) die folgenden Schritte aus:
    1. Wählen Sie eine **Größe** für die virtuellen Computer (VMs), die im Lab erstellt werden. Derzeit sind die folgenden Größen zulässig: **Klein**, **Mittel**, **Mittel (Virtualisierung)** , **Groß** und **GPU**. Weitere Informationen finden Sie im Abschnitt [VM-Größen](#vm-sizes).
    1. Wählen Sie die **Region** aus, in der die VMs erstellt werden sollen. 
    1. Wählen Sie das **VM-Image** aus, das zum Erstellen von VMs im Lab verwendet werden soll. Bei Auswahl eines Linux-Images wird eine Option zum Aktivieren der Remotedesktopverbindung angezeigt. Ausführliche Informationen finden Sie unter [Aktivieren der Remotedesktopverbindung für Linux](how-to-enable-remote-desktop-linux.md).
    1. Klicken Sie auf **Weiter**.

        ![Angeben von VM-Spezifikationen](../media/tutorial-setup-classroom-lab/select-vm-specifications.png)    
5. Geben Sie auf der Seite **Anmeldeinformationen festlegen** die Standardanmeldeinformationen für alle VMs im Lab an. 
    1. Geben Sie den **Namen des Benutzers** für alle VMs im Lab an.
    2. Geben Sie das **Kennwort** für den Benutzer an. 

        > [!IMPORTANT]
        > Notieren Sie sich den Benutzernamen und das Kennwort. Diese Angaben werden nicht noch einmal angezeigt.
    3. Deaktivieren Sie die Option **Use same password for all virtual machines** (Dasselbe Kennwort für alle virtuellen Computer verwenden), wenn Sie möchten, dass die Kursteilnehmer ihre eigenen Kennwörter festlegen. Dieser Schritt ist **optional**. 

        Ein Kursleiter kann auch dasselbe Kennwort für alle VMs im Lab verwenden, oder den Kursteilnehmern erlauben, die Kennwörter für ihre virtuellen Computer selbst festzulegen. Standardmäßig ist diese Einstellung für alle Windows- und Linux-Images mit Ausnahme von Ubuntu aktiviert. Bei der Auswahl einer **Ubuntu**-VM ist diese Einstellung deaktiviert, sodass die Kursteilnehmer bei der ersten Anmeldung aufgefordert werden, ein Kennwort festzulegen.
    1. Klicken Sie auf **Erstellen**. 

        ![Festlegen von Anmeldeinformationen](../media/tutorial-setup-classroom-lab/set-credentials.png)
6. Auf der Seite **Vorlage konfigurieren** wird der Status für den Prozess der Laberstellung angezeigt. Die Erstellung der Vorlage im Lab dauert bis zu 20 Minuten. Eine Vorlage in einem Lab ist ein VM-Basisimage und dient zur Erstellung der virtuellen Computer aller Benutzer. Richten Sie den virtuellen Computer der Vorlage so ein, dass er genau das enthält, was Sie den Lab-Benutzern zur Verfügung stellen möchten.  

    ![Konfigurieren der Vorlage](../media/tutorial-setup-classroom-lab/configure-template.png)
7. Nachdem die Konfiguration der Vorlage abgeschlossen ist, wird die folgende Seite angezeigt: 

    ![Seite „Vorlage konfigurieren“ nach Abschluss des Vorgangs](../media/tutorial-setup-classroom-lab/configure-template-after-complete.png)
8. Die folgenden Schritte sind in diesem Tutorial optional: 
    2. Stellen Sie eine Verbindung mit der Vorlage für virtuelle Computer her, indem Sie **Verbinden** wählen. Handelt es sich um einen virtuellen Computer mit Linux-Vorlage, legen Sie fest, ob eine Verbindung per SSH oder RDP (sofern RDP aktiviert ist) hergestellt werden soll.
    1. Klicken Sie auf **Kennwort zurücksetzen**, um das Kennwort für die VM zurückzusetzen. 
    1. Installieren und konfigurieren Sie die Software in der Vorlage für virtuelle Computer. 
    1. **Beenden** Sie den virtuellen Computer.  
    1. Geben Sie eine **Beschreibung** für die Vorlage ein.
9. Wählen Sie auf der Vorlagenseite die Option **Weiter**. 
10. Führen Sie auf der Seite **Vorlage veröffentlichen** die folgenden Aktionen durch. 
    1. Aktivieren Sie zum sofortigen Veröffentlichen der Vorlage das Kontrollkästchen *I understand I can't modify the template after publishing. This process can only be done once and can take up to an hour* (Ich weiß, dass die Vorlage nach dem Veröffentlichen nicht mehr geändert werden kann. Dieser Prozess kann nur einmal durchgeführt werden und bis zu einer Stunde dauern.), und wählen Sie **Veröffentlichen**.  Veröffentlichen Sie die Vorlage, um Instanzen der Vorlage für virtuelle Computer für Ihre Lab-Benutzer zur Verfügung zu stellen.

        > [!WARNING]
        > Nachdem die Veröffentlichung erfolgt ist, kann sie nicht mehr rückgängig gemacht werden. 
    2. Wählen Sie **Für später speichern**, wenn Sie die Veröffentlichung später durchführen möchten. Sie können die Vorlage für virtuelle Computer veröffentlichen, nachdem der Assistent abgeschlossen wurde. Weitere Informationen zum Konfigurieren und Veröffentlichen nach Abschluss des Assistenten finden Sie im Abschnitt „Veröffentlichen der Vorlage“ des Artikels [Verwalten von Classroom-Labs](how-to-manage-classroom-labs.md).

        ![Vorlage veröffentlichen](../media/tutorial-setup-classroom-lab/publish-template.png)
11. Für die Vorlage wird der **Status der Veröffentlichung** angezeigt. Dieser Vorgang kann bis zu einer Stunde dauern. 

    ![Veröffentlichen der Vorlage – Status](../media/tutorial-setup-classroom-lab/publish-template-progress.png)
12. Die folgende Seite wird angezeigt, wenn die Veröffentlichung der Vorlage erfolgreich war. Wählen Sie **Fertig**aus.

    ![Veröffentlichen der Vorlage – Erfolg](../media/tutorial-setup-classroom-lab/publish-success.png)
1. Das **Dashboard** für das Lab wird angezeigt. 
    
    ![Dashboard für Classroom-Lab](../media/tutorial-setup-classroom-lab/classroom-lab-home-page.png)
4. Wechseln Sie zur Seite **Virtuelle Computer**, und vergewissern Sie sich, dass virtuelle Computer mit dem Status **Nicht zugewiesen** angezeigt werden. Diese virtuellen Computer sind noch keinen Teilnehmern zugewiesen. Sie sollten den Status **Beendet** aufweisen. Auf dieser Seite können Sie einen virtuellen Computer für einen Teilnehmer starten, eine Verbindung damit herstellen und ihn beenden und löschen. Sie können virtuelle Computer auf dieser Seite starten oder sie von Ihren Teilnehmern starten lassen. 

    ![Virtuelle Computer im Status „Beendet“](../media/tutorial-setup-classroom-lab/virtual-machines-stopped.png)

### <a name="vm-sizes"></a>VM-Größen  

| Größe | Kerne | RAM | BESCHREIBUNG | 
| ---- | ----- | --- | ----------- | 
| Klein | 2 | 3,5 GB | Diese Größe eignet sich am besten für die Befehlszeile, das Öffnen von Webbrowsern, Webserver mit geringem Datenverkehr und kleine bis mittelgroße Datenbanken. |
| Mittel | 4 | 7 GB | Diese Größe eignet sich am besten für relationale Datenbanken, speicherinternes Caching und Analysen. | 
| Mittel (geschachtelte Virtualisierung) | 4 | 16 GB | Diese Größe eignet sich am besten für relationale Datenbanken, speicherinternes Caching und Analysen. Sie unterstützt auch die geschachtelte Virtualisierung. <p>Diese Größe kann in Szenarien verwendet werden, in denen jeder Kursteilnehmer mehrere virtuelle Computer benötigt. Die Kursleiter können die geschachtelte Virtualisierung nutzen, um einige kleine geschachtelte virtuelle Computer innerhalb des virtuellen Computers einzurichten. </p> |
| Groß | 8 | 32 GB | Diese Größe eignet sich am besten für Anwendungen, die schnellere CPUs, eine bessere lokale Datenträgerleistung, große Datenbanken und große Caches benötigen. Sie unterstützt auch die geschachtelte Virtualisierung. |  
| GPU | 12 | 112 GB | Diese Größe eignet sich am besten für rechenintensive, grafikintensive und visualisierungsorientierte Workloads. | 

## <a name="view-all-classroom-labs"></a>Anzeigen aller Classroom-Labs
1. Navigieren Sie zum [Azure Lab Services-Portal](https://labs.azure.com).
2. Wählen Sie **Anmelden**. Wählen Sie eine **Benutzer-ID** aus (oder geben Sie eine Benutzer-ID ein), die der Rolle **Lab-Ersteller** im Labkonto angehört, und geben Sie das Kennwort ein. Azure Lab Services unterstützt Geschäfts-, Schul- oder Unikonten und Microsoft-Konten. 
3. Stellen Sie sicher, dass alle Labs im ausgewählten Lab-Konto angezeigt werden. 

    ![Alle Labs](../media/how-to-manage-classroom-labs/all-labs.png)
3. Verwenden Sie die Dropdownliste im oberen Bereich, um ein anderes Lab-Konto auszuwählen. Die Labs im ausgewählten Lab-Konto werden angezeigt. 

## <a name="delete-a-classroom-lab"></a>Löschen eines Classroom-Labs
1. Wählen Sie auf der Kachel für das Lab die drei Punkte (...) in der Ecke aus. 

    ![Auswählen des Labs](../media/how-to-manage-classroom-labs/select-three-dots.png)
2. Klicken Sie auf **Löschen**. 

    ![Schaltfläche „Löschen“](../media/how-to-manage-classroom-labs/delete-button.png)
3. Klicken Sie im Dialogfeld **Lab löschen** auf **Löschen**. 

    ![Dialogfeld „Löschen“](../media/how-to-manage-classroom-labs/delete-lab-dialog-box.png)

## <a name="switch-to-another-classroom-lab"></a>Zu einem anderen Classroom-Lab wechseln
Um aus dem aktuellen zu einem anderen Classroom-Lab zu wechseln, treffen Sie im Lab-Konto oben in der Dropdownliste mit den Labs eine Wahl.

![Wählen Sie das Lab in der Dropdownliste oben aus.](../media/how-to-manage-classroom-labs/switch-lab.png)


## <a name="next-steps"></a>Nächste Schritte
Entsprechende Informationen finden Sie in den folgenden Artikeln:

- [Einrichten und Veröffentlichen von Vorlagen als Labbesitzer](how-to-create-manage-template.md)
- [Konfigurieren und Steuern der Nutzung eines Labs als Labbesitzer](how-to-configure-student-usage.md)
- [Zugreifen auf ein Classroom-Lab in Azure Lab Services](how-to-use-classroom-lab.md) (als Labbenutzer)

