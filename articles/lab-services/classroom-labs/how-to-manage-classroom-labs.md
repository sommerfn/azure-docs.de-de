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
ms.date: 10/12/2019
ms.author: spelluru
ms.openlocfilehash: 85a3a9f7afac8250b225d42462f6b29042e34a2a
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72330275"
---
# <a name="manage-classroom-labs-in-azure-lab-services"></a>Verwalten von Classroom-Labs in Azure Lab Services 
In diesem Artikel erfahren Sie, wie Sie ein Classroom-Lab erstellen und löschen. Außerdem erfahren Sie, wie Sie alle Classroom-Labs in einem Labkonto anzeigen. 

## <a name="prerequisites"></a>Voraussetzungen
Zum Einrichten eines Classroom-Labs in einem Labkonto müssen Sie Mitglied der Rolle **Ersteller des Labs** für das Labkonto sein. Das zum Erstellen eines Lab-Kontos verwendete Konto wird dieser Rolle automatisch hinzugefügt. Ein Besitzer des Labs kann der Rolle „Ersteller des Labs“ weitere Benutzer hinzufügen, indem er die Schritte im folgenden Artikel ausführt: [Hinzufügen eines Benutzers zur Rolle „Ersteller des Labs“](tutorial-setup-lab-account.md#add-a-user-to-the-lab-creator-role).

## <a name="create-a-classroom-lab"></a>Erstellen eines Classroom-Labs

1. Navigieren Sie zur Website [Azure Lab Services](https://labs.azure.com). Beachten Sie den Hinweis, dass Internet Explorer 11 noch nicht unterstützt wird. 
2. Wählen Sie **Anmelden**, und geben Sie Ihre Anmeldeinformationen ein. Wählen Sie eine **Benutzer-ID** aus (oder geben Sie eine Benutzer-ID ein), die der Rolle **Lab-Ersteller** im Labkonto angehört, und geben Sie das Kennwort ein. Azure Lab Services unterstützt Geschäfts-, Schul- oder Unikonten und Microsoft-Konten. 
3. Wählen Sie **Neues Lab** aus. 
    
    ![Erstellen eines Classroom-Labs](../media/tutorial-setup-classroom-lab/new-lab-button.png)
3. Führen Sie im Fenster **Neues Lab** die folgenden Aktionen aus: 
    1. Geben Sie unter **Name** einen Namen für Ihr Lab an. 
    2. Wählen Sie die **Größe der virtuellen Computer** aus, die Sie für die Klasse benötigen. Die Liste der verfügbaren Größen finden Sie im Abschnitt [VM-Größen ](#vm-sizes). 
    3. Wählen Sie das **VM-Image** aus, das Sie für das Classroom-Lab verwenden möchten. Bei Auswahl eines Linux-Images wird eine Option zum Aktivieren der Remotedesktopverbindung angezeigt. Ausführliche Informationen finden Sie unter [Aktivieren der Remotedesktopverbindung für Linux](how-to-enable-remote-desktop-linux.md).
    4. Überprüfen Sie den **Gesamtpreis pro Stunde** , der auf der Seite angezeigt wird. 
    6. Wählen Sie **Speichern** aus.

        ![Fenster „Neues Lab“](../media/tutorial-setup-classroom-lab/new-lab-window.png)
4. Geben Sie auf der Seite mit den **VM-Anmeldeinformationen** die Standardanmeldeinformationen für alle VMs im Lab an.
    1. Geben Sie den **Namen des Benutzers** für alle VMs im Lab an.
    2. Geben Sie das **Kennwort** für den Benutzer an. 

        > [!IMPORTANT]
        > Notieren Sie sich den Benutzernamen und das Kennwort. Diese Angaben werden nicht noch einmal angezeigt.
    3. Deaktivieren Sie die Option **Use same password for all virtual machines** (Dasselbe Kennwort für alle virtuellen Computer verwenden), wenn Sie möchten, dass die Kursteilnehmer ihre eigenen Kennwörter festlegen. Dieser Schritt ist **optional**. 

        Ein Kursleiter kann auch dasselbe Kennwort für alle VMs im Lab verwenden, oder den Kursteilnehmern erlauben, die Kennwörter für ihre virtuellen Computer selbst festzulegen. Standardmäßig ist diese Einstellung für alle Windows- und Linux-Images mit Ausnahme von Ubuntu aktiviert. Bei der Auswahl einer **Ubuntu**-VM ist diese Einstellung deaktiviert, sodass die Kursteilnehmer bei der ersten Anmeldung aufgefordert werden, ein Kennwort festzulegen.  

        ![Fenster „Neues Lab“](../media/tutorial-setup-classroom-lab/virtual-machine-credentials.png)
        > [!IMPORTANT]
        > Notieren Sie sich den Benutzernamen und das Kennwort. Diese Angaben werden nicht noch einmal angezeigt.    
    4. Wählen Sie dann **Weiter** auf der Seite **VM-Anmeldeinformationen** aus. 
5. Geben Sie auf der Seite mit den **Labrichtlinien** die Anzahl von Stunden ein, die jedem Benutzer (**Kontingent pro Benutzer**) außerhalb der geplanten Zeit für das Lab zugeordnet sind, und wählen Sie anschließend **Fertig stellen** aus. 

    ![Kontingent pro Benutzer](../media/tutorial-setup-classroom-lab/quota-for-each-user.png)
5. Es sollte der folgende Bildschirm angezeigt werden, auf dem der Status der VM-Vorlagenerstellung angegeben ist. Die Erstellung der Vorlage im Lab dauert bis zu 20 Minuten. 

    ![Status der VM-Vorlagenerstellung](../media/tutorial-setup-classroom-lab/create-template-vm-progress.png)
8. Führen Sie auf der Seite **Vorlage** die folgenden Schritte aus: Diese Schritte sind für das Tutorial **optional**.

    2. Stellen Sie eine Verbindung mit der Vorlage für virtuelle Computer her, indem Sie **Verbinden** wählen. Handelt es sich um einen virtuellen Computer mit Linux-Vorlage, legen Sie fest, ob eine Verbindung per SSH oder RDP (sofern RDP aktiviert ist) hergestellt werden soll.
    1. Klicken Sie auf **Kennwort zurücksetzen**, um das Kennwort für die VM zurückzusetzen. 
    1. Installieren und konfigurieren Sie die Software in der Vorlage für virtuelle Computer. 
    1. **Beenden** Sie den virtuellen Computer.  
    1. Geben Sie eine **Beschreibung** für die Vorlage ein.
10. Wählen Sie auf der Seite **Vorlage** in der Symbolleiste die Option **Veröffentlichen** aus. 

    ![Schaltfläche „Vorlage veröffentlichen“](../media/tutorial-setup-classroom-lab/template-page-publish-button.png)

    > [!WARNING]
    > Nachdem die Veröffentlichung erfolgt ist, kann sie nicht mehr rückgängig gemacht werden. 
8. Geben Sie auf der Seite **Vorlage veröffentlichen** die Anzahl von virtuellen Computern ein, die Sie im Lab erstellen möchten, und wählen Sie anschließend die Option **Veröffentlichen** aus. 

    ![Vorlage veröffentlichen: Anzahl von VMs](../media/tutorial-setup-classroom-lab/publish-template-number-vms.png)
11. Auf der Seite wird der **Veröffentlichungsstatus** der Vorlage angezeigt. Dieser Vorgang kann bis zu einer Stunde dauern. 

    ![Veröffentlichen der Vorlage – Status](../media/tutorial-setup-classroom-lab/publish-template-progress.png)
4. Wechseln Sie zur Seite **VM-Pool**, indem Sie im linken Menü die Option „Virtuelle Computer“ bzw. die entsprechende Kachel auswählen. Vergewissern Sie sich, dass virtuelle Computer mit dem Status **Nicht zugewiesen** angezeigt werden. Diese virtuellen Computer sind noch keinen Teilnehmern zugewiesen. Sie sollten den Status **Beendet** aufweisen. Auf dieser Seite können Sie einen virtuellen Computer für einen Teilnehmer starten, eine Verbindung damit herstellen und ihn beenden und löschen. Sie können virtuelle Computer auf dieser Seite starten oder sie von Ihren Teilnehmern starten lassen. 

    ![Virtuelle Computer im Status „Beendet“](../media/tutorial-setup-classroom-lab/virtual-machines-stopped.png)

    Auf dieser Seite führen Sie die folgenden Aufgaben aus (Führen Sie diese Schritte nicht für das Tutorial aus. Sie dienen ausschließlich zu Informationszwecken.): 
    
    1. Wählen Sie in der Symbolleiste die Option **Lab capacity** (Labkapazität) aus, um sie zu ändern (Anzahl von VMs im Lab).
    2. Wählen Sie in der Symbolleiste **Start all** (Alle starten) aus, um alle VMs auf einmal zu starten. 
    3. Wählen Sie zum Starten einer bestimmten VM unter **Status** den Abwärtspfeil und dann die Option **Starten** aus. Sie können eine VM auch starten, indem Sie sie in der ersten Spalte auswählen und in der Symbolleiste dann die Option **Starten** auswählen.                

### <a name="vm-sizes"></a>VM-Größen  

| Size | Kerne | RAM | BESCHREIBUNG | 
| ---- | ----- | --- | ----------- | 
| Klein | 2 | 3,5 GB | Diese Größe eignet sich am besten für die Befehlszeile, das Öffnen von Webbrowsern, Webserver mit geringem Datenverkehr und kleine bis mittelgroße Datenbanken. |
| Mittel | 4 | 7 GB | Diese Größe eignet sich am besten für relationale Datenbanken, speicherinternes Caching und Analysen. | 
| Mittel (geschachtelte Virtualisierung) | 4 | 16 GB | Diese Größe eignet sich am besten für relationale Datenbanken, speicherinternes Caching und Analysen. Sie unterstützt auch die geschachtelte Virtualisierung. <p>Diese Größe kann in Szenarien verwendet werden, in denen jeder Kursteilnehmer mehrere virtuelle Computer benötigt. Die Kursleiter können die geschachtelte Virtualisierung nutzen, um einige kleine geschachtelte virtuelle Computer innerhalb des virtuellen Computers einzurichten. </p> |
| Groß | 8 | 32 GB | Diese Größe eignet sich am besten für Anwendungen, die schnellere CPUs, eine bessere lokale Datenträgerleistung, große Datenbanken und große Caches benötigen. Sie unterstützt auch die geschachtelte Virtualisierung. |  
| Kleine GPU (Visualisierung) | 6 | 56 GB | Diese Größe eignet sich am besten für Remotevisualisierung, Streaming, Spiele und Codierung mit Frameworks wie OpenGL und DirectX. | 
| Kleine GPU (Compute) | 6 | 56 GB | Diese Größe eignet sich am besten für rechenintensive und netzwerkintensive Anwendungen wie künstliche Intelligenz und Deep Learning-Anwendungen. | 
| Mittlere GPU (Visualisierung) | 12 | 112 GB | Diese Größe eignet sich am besten für Remotevisualisierung, Streaming, Spiele und Codierung mit Frameworks wie OpenGL und DirectX. | 

## <a name="view-all-classroom-labs"></a>Anzeigen aller Classroom-Labs
1. Navigieren Sie zum [Azure Lab Services-Portal](https://labs.azure.com).
2. Wählen Sie **Anmelden**. Wählen Sie eine **Benutzer-ID** aus (oder geben Sie eine Benutzer-ID ein), die der Rolle **Lab-Ersteller** im Labkonto angehört, und geben Sie das Kennwort ein. Azure Lab Services unterstützt Geschäfts-, Schul- oder Unikonten und Microsoft-Konten. 
3. Stellen Sie sicher, dass alle Labs im ausgewählten Lab-Konto angezeigt werden. Auf der Kachel des Labs sehen Sie die Anzahl der virtuellen Computer im Lab und das Kontingent für jeden Benutzer (außerhalb der geplanten Zeit).

    ![Alle Labs](../media/how-to-manage-classroom-labs/all-labs.png)
3. Verwenden Sie die Dropdownliste im oberen Bereich, um ein anderes Lab-Konto auszuwählen. Die Labs im ausgewählten Lab-Konto werden angezeigt. 

## <a name="delete-a-classroom-lab"></a>Löschen eines Classroom-Labs
1. Wählen Sie auf der Kachel für das Lab die drei Punkte (...) in der Ecke und dann **Löschen** aus. 

    ![Schaltfläche „Löschen“](../media/how-to-manage-classroom-labs/delete-button.png)
3. Wählen Sie im Dialogfeld **Lab löschen** den Befehl **Löschen** aus, um mit dem Löschvorgang fortzufahren. 

## <a name="switch-to-another-classroom-lab"></a>Zu einem anderen Classroom-Lab wechseln
Um aus dem aktuellen zu einem anderen Classroom-Lab zu wechseln, treffen Sie im Lab-Konto oben in der Dropdownliste mit den Labs eine Wahl.

![Wählen Sie das Lab in der Dropdownliste oben aus.](../media/how-to-manage-classroom-labs/switch-lab.png)

Sie können auch ein neues Lab erstellen, indem Sie in dieser Dropdownliste **Neues Lab** verwenden. 


## <a name="next-steps"></a>Nächste Schritte
Entsprechende Informationen finden Sie in den folgenden Artikeln:

- [Einrichten und Veröffentlichen von Vorlagen als Labbesitzer](how-to-create-manage-template.md)
- [Konfigurieren und Steuern der Nutzung eines Labs als Labbesitzer](how-to-configure-student-usage.md)
- [Zugreifen auf ein Classroom-Lab in Azure Lab Services](how-to-use-classroom-lab.md) (als Labbenutzer)

