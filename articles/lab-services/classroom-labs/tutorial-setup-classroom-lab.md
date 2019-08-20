---
title: Einrichten eines Classroom-Labs in Azure Lab Services | Microsoft-Dokumentation
description: In diesem Tutorial richten Sie ein Lab für die Verwendung in einem Classroom ein.
services: devtest-lab, lab-services, virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 06/11/2019
ms.author: spelluru
ms.openlocfilehash: 964ecca015e440439885bbbd85cb720a3abd10a9
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2019
ms.locfileid: "68883512"
---
# <a name="tutorial-set-up-a-classroom-lab"></a>Tutorial: Einrichten eines Classroom-Labs 
In diesem Tutorial richten Sie ein Classroom-Lab mit virtuellen Computern ein, die von den Teilnehmern im Classroom verwendet werden.  

In diesem Tutorial führen Sie die folgenden Aktionen aus:

> [!div class="checklist"]
> * Erstellen eines Classroom-Labs
> * Hinzufügen von Benutzern zum Lab
> * Senden eines Registrierungslinks an Teilnehmer

## <a name="prerequisites"></a>Voraussetzungen
Zum Einrichten eines Classroom-Labs in einem Labkonto müssen Sie Mitglied einer der folgenden Rollen für das Labkonto sein: „Besitzer“, „Ersteller des Labs“ oder „Mitwirkender“. Das zum Erstellen eines Labkontos verwendete Konto wird automatisch der Rolle „Besitzer“ hinzugefügt.

Ein Labbesitzer kann andere Benutzer zur Rolle **Ersteller des Labs** hinzufügen. Beispielsweise fügt ein Labbesitzer Lehrkräfte zur Rolle „Ersteller des Labs“ hinzu. Anschließend erstellen die Lehrkräfte Labs mit virtuellen Computern für ihre Klassen. Schüler/Studenten verwenden zum Registrieren für das Lab den Registrierungslink, den sie von Lehrkräften erhalten. Nach der Registrierung können sie virtuelle Computer in den Labs für Unterrichtsarbeit und Hausaufgaben nutzen. Ausführliche Schritte zum Hinzufügen von Benutzern zur Rolle „Ersteller des Labs“ finden Sie unter [Tutorial: Einrichten eines Lab-Kontos mit Azure Lab Services](tutorial-setup-lab-account.md#add-a-user-to-the-lab-creator-role).


## <a name="create-a-classroom-lab"></a>Erstellen eines Classroom-Labs

1. Navigieren Sie zur Website [Azure Lab Services](https://labs.azure.com). Beachten Sie den Hinweis, dass Internet Explorer 11 noch nicht unterstützt wird. 
2. Wählen Sie **Anmelden**, und geben Sie Ihre Anmeldeinformationen ein. Azure Lab Services unterstützt Geschäfts-, Schul- oder Unikonten und Microsoft-Konten. 
3. Führen Sie im Fenster **Neues Lab** die folgenden Aktionen aus: 
    1. Geben Sie unter **Name** einen Namen für Ihr Lab an. 
    2. Geben Sie die maximale **Anzahl von virtuellen Computern** im Lab an. Die Anzahl von virtuellen Computern kann nach der Lab-Erstellung oder in einem bereits vorhandenen Lab erhöht oder verringert werden. Weitere Informationen finden Sie unter [Aktualisieren der Anzahl von virtuellen Computern im Lab](how-to-configure-student-usage.md#update-number-of-virtual-machines-in-lab).
    6. Wählen Sie **Speichern** aus.

        ![Erstellen eines Classroom-Labs](../media/tutorial-setup-classroom-lab/new-lab-window.png)
4. Führen Sie auf der Seite **Select virtual machine specifications** (Spezifikationen für virtuellen Computer auswählen) die folgenden Schritte aus:
    1. Wählen Sie eine **Größe** für die virtuellen Computer (VMs), die im Lab erstellt werden. Derzeit sind die folgenden Größen zulässig: **Klein**, **Mittel**, **Mittel (Virtualisierung)** , **Groß** und **GPU**.
    3. Wählen Sie das **VM-Image** aus, das zum Erstellen von VMs im Lab verwendet werden soll. Bei Auswahl eines Linux-Images wird eine Option zum Aktivieren der Remotedesktopverbindung angezeigt. Ausführliche Informationen finden Sie unter [Aktivieren der Remotedesktopverbindung für Linux](how-to-enable-remote-desktop-linux.md).
    4. Klicken Sie auf **Weiter**.

        ![Angeben von VM-Spezifikationen](../media/tutorial-setup-classroom-lab/select-vm-specifications.png)    
5. Geben Sie auf der Seite **Anmeldeinformationen festlegen** die Standardanmeldeinformationen für alle VMs im Lab an. 
    1. Geben Sie den **Namen des Benutzers** für alle VMs im Lab an.
    2. Geben Sie das **Kennwort** für den Benutzer an. 

        > [!IMPORTANT]
        > Notieren Sie sich den Benutzernamen und das Kennwort. Diese Angaben werden nicht noch einmal angezeigt.
    3. Klicken Sie auf **Erstellen**. 

        ![Festlegen von Anmeldeinformationen](../media/tutorial-setup-classroom-lab/set-credentials.png)
6. Auf der Seite **Vorlage konfigurieren** wird der Status für den Prozess der Laberstellung angezeigt. Die Erstellung der Vorlage im Lab dauert bis zu 20 Minuten. 

    ![Konfigurieren der Vorlage](../media/tutorial-setup-classroom-lab/configure-template.png)
7. Nachdem die Konfiguration der Vorlage abgeschlossen ist, wird die folgende Seite angezeigt: 

    ![Seite „Vorlage konfigurieren“ nach Abschluss des Vorgangs](../media/tutorial-setup-classroom-lab/configure-template-after-complete.png)
8. Führen Sie auf der Seite **Vorlage konfigurieren** die folgenden Schritte aus: Diese Schritte sind für das Tutorial **optional**.
    2. Stellen Sie eine Verbindung mit der Vorlage für virtuelle Computer her, indem Sie **Verbinden** wählen. Handelt es sich um einen virtuellen Computer mit Linux-Vorlage, legen Sie fest, ob eine Verbindung per SSH oder RDP (sofern RDP aktiviert ist) hergestellt werden soll.
    1. Klicken Sie auf **Kennwort zurücksetzen**, um das Kennwort für die VM zurückzusetzen. 
    1. Installieren und konfigurieren Sie die Software in der Vorlage für virtuelle Computer. 
    1. **Beenden** Sie den virtuellen Computer.  
    1. Geben Sie eine **Beschreibung** für die Vorlage ein.
9. Wählen Sie auf der Vorlagenseite die Option **Weiter**. 
10. Führen Sie auf der Seite **Vorlage veröffentlichen** die folgenden Aktionen durch. 
    1. Wenn Sie die Vorlage sofort veröffentlichen möchten, wählen Sie **Veröffentlichen** aus.  

        > [!WARNING]
        > Nachdem die Veröffentlichung erfolgt ist, kann sie nicht mehr rückgängig gemacht werden. 
    2. Wählen Sie **Für später speichern**, wenn Sie die Veröffentlichung später durchführen möchten. Sie können die Vorlage für virtuelle Computer veröffentlichen, nachdem der Assistent abgeschlossen wurde. Weitere Informationen zum Konfigurieren und Veröffentlichen nach Abschluss des Assistenten finden Sie im Artikel [Verwalten von Classroom-Labs in Azure Lab Services](how-to-manage-classroom-labs.md) im Abschnitt [Veröffentlichen der Vorlage](how-to-create-manage-template.md#publish-the-template-vm).

        ![Vorlage veröffentlichen](../media/tutorial-setup-classroom-lab/publish-template.png)
11. Für die Vorlage wird der **Status der Veröffentlichung** angezeigt. Dieser Vorgang kann bis zu einer Stunde dauern. 

    ![Veröffentlichen der Vorlage – Status](../media/tutorial-setup-classroom-lab/publish-template-progress.png)
12. Die folgende Seite wird angezeigt, wenn die Veröffentlichung der Vorlage erfolgreich war. Wählen Sie **Fertig**aus.

    ![Veröffentlichen der Vorlage – Erfolg](../media/tutorial-setup-classroom-lab/publish-success.png)
1. Das **Dashboard** für das Lab wird angezeigt. 
    
    ![Dashboard für Classroom-Lab](../media/tutorial-setup-classroom-lab/classroom-lab-home-page.png)
4. Wählen Sie „Virtuelle Computer“ im linken Menü bzw. die Kachel „Virtuelle Computer“, um die Seite **Virtuelle Computer** aufzurufen Vergewissern Sie sich, dass virtuelle Computer mit dem Status **Nicht zugewiesen** angezeigt werden. Diese virtuellen Computer sind noch keinen Teilnehmern zugewiesen. Sie sollten den Status **Beendet** aufweisen. Auf dieser Seite können Sie einen virtuellen Computer für einen Teilnehmer starten, eine Verbindung damit herstellen und ihn beenden und löschen. Sie können virtuelle Computer auf dieser Seite starten oder sie von Ihren Teilnehmern starten lassen. 

    ![Virtuelle Computer im Status „Beendet“](../media/tutorial-setup-classroom-lab/virtual-machines-stopped.png)

## <a name="add-users-to-the-lab"></a>Hinzufügen von Benutzern zum Lab

1. Klicken Sie im linken Menü auf **Benutzer**. Die Option **Zugriff beschränken** ist standardmäßig aktiviert. Wenn diese Einstellung aktiviert ist, kann sich ein Benutzer auch dann nicht beim Lab registrieren, wenn er über den Registrierungslink verfügt. Um sich zu registrieren, muss er in der Liste der Benutzer enthalten sein. Nur Benutzer in der Liste können sich über den von Ihnen gesendeten Registrierungslink beim Lab registrieren. In diesem Verfahren fügen Sie der Liste Benutzer hinzu. Alternativ können Sie **Zugriff beschränken** deaktivieren. In diesem Fall können sich Benutzer beim Lab registrieren, wenn sie über den Registrierungslink verfügen. 
2. Klicken Sie auf der Symbolleiste auf **Benutzer hinzufügen**. 

    ![Schaltfläche „Benutzer hinzufügen“](../media/how-to-configure-student-usage/add-users-button.png)
1. Geben Sie auf der Seite **Benutzer hinzufügen** die E-Mail-Adressen von Benutzern in separaten Zeilen oder durch Semikolons getrennt in einer einzelnen Zeile ein. 

    ![Hinzufügen der E-Mail-Adressen von Benutzern](../media/how-to-configure-student-usage/add-users-email-addresses.png)
4. Wählen Sie **Speichern** aus. In der Liste werden die E-Mail-Adressen und der Status (registriert oder nicht registriert) von Benutzern angezeigt. 

    ![Benutzerliste](../media/how-to-configure-student-usage/users-list-new.png)

## <a name="set-quotas-for-users"></a>Festlegen von Kontingenten für Benutzer
Mithilfe der folgenden Schritte können Sie Kontingente pro Benutzer festlegen: 

1. Wählen Sie im linken Menü **Benutzer** aus, wenn die Seite noch nicht aktiv ist. 
2. Wählen Sie **Kontingent pro Benutzer 10 Stunden** auf der Symbolleiste aus. 
3. Geben Sie auf der Seite **Kontingent pro Benutzer** die Anzahl der Stunden an, die Sie jedem Benutzer (Kursteilnehmer) zuteilen möchten: 
    1. **Total number of lab hours per user** (Gesamtanzahl der Lab-Stunden pro Benutzer). Benutzer können ihre VMs **zusätzlich zur geplanten Zeit** für die festgelegte (in diesem Feld angegebene) Anzahl von Stunden nutzen. Wenn Sie diese Option auswählen, geben Sie die **Anzahl der Stunden** in das Textfeld ein. 

        ![Anzahl von Stunden pro Benutzer](../media/how-to-configure-student-usage/number-of-hours-per-user.png). 
    1. **0 Stunden (nur Zeitplan)** . Benutzer können ihre VMs nur während der geplanten Zeit nutzen, oder wenn Sie als Lab-Besitzer die VMs für sie einschalten.

        ![Null Stunden – nur geplante Zeit](../media/how-to-configure-student-usage/zero-hours.png)
    4. Wählen Sie **Speichern** aus. 
5. Jetzt sehen Sie auf der Symbolleiste die geänderten Werte: **Kontingent pro Benutzer: &lt;Anzahl Stunden&gt;** . 

    ![Kontingent pro Benutzer](../media/how-to-configure-student-usage/quota-per-user.png)

## <a name="set-a-schedule-for-the-lab"></a>Festlegen eines Zeitplans für das Lab
Wenn Sie die Kontingenteinstellung mit **0 hours (schedule only)** konfiguriert haben, müssen Sie einen Zeitplan für das Lab festlegen. In diesem Tutorial legen Sie den Zeitplan auf wöchentliche Wiederholung fest.

1. Wechseln Sie zur Seite **Zeitpläne**, und wählen Sie auf der Symbolleiste **Zeitplan hinzufügen** aus. 

    ![Schaltfläche „Zeitplan hinzufügen“ auf der Seite „Zeitpläne“](../media/how-to-create-schedules/add-schedule-button.png)
2. Wechseln Sie auf der Seite **Zeitplan hinzufügen** oben zu **Wöchentlich**. 
3. Wählen Sie unter **Schedule days (required)** (Tage planen (erforderlich)) die Tage aus, an denen der Zeitplan wirksam sein soll. Im folgenden Beispiel sind Montag bis Freitag ausgewählt. 
4. Geben Sie im Feld **Von** das **Startdatum des Zeitplans** ein, oder wählen Sie ein Datum aus, indem Sie die **Kalenderschaltfläche** auswählen. Dies ist ein Pflichtfeld. 
5. Geben Sie unter **Enddatum des Zeitplans** ein Enddatum ein, an dem die virtuellen Computer heruntergefahren werden sollen, oder wählen Sie eines aus. 
6. Wählen Sie für **Startzeit** die Zeit aus, zu der die virtuellen Computer gestartet werden sollen. Die Startzeit ist erforderlich, wenn keine Beendigungszeit festgelegt ist. Wählen Sie **Remove start event** (Startereignis entfernen) aus, wenn Sie nur die Beendigungszeit angeben möchten. Wenn die **Startzeit** deaktiviert ist, wählen Sie neben der Dropdownliste die Option **Add start event** (Startereignis hinzufügen) aus, um sie zu aktivieren. 
7. Wählen Sie für **Beendigungszeit** die Zeit aus, zu der die virtuellen Computer heruntergefahren werden sollen. Die Beendigungszeit ist erforderlich, wenn keine Startzeit festgelegt ist. Wählen Sie **Remove stop event** (Beendigungsereignis entfernen) aus, wenn Sie nur die Startzeit angeben möchten. Wenn die **Endzeit** deaktiviert ist, wählen Sie neben der Dropdownliste die Option **Add stop event** (Beendigungsereignis hinzufügen) aus, um sie zu aktivieren.
8. Wählen Sie für **Time zone (required)** (Zeitzone (erforderlich)) die Zeitzone für die angegebenen Start- und Beendigungszeiten aus.  
9. Geben Sie für **Hinweise** eine Beschreibung oder Anmerkungen zu diesem Zeitplan ein. 
10. Wählen Sie **Speichern** aus. 

    ![Wöchentlicher Zeitplan](../media/how-to-create-schedules/add-schedule-page-weekly.png)

## <a name="send-an-email-with-the-registration-link"></a>Senden einer E-Mail mit dem Registrierungslink

1. Wechseln Sie zur Ansicht **Benutzer**, falls Sie sich noch nicht auf der Seite befinden. 
2. Wählen Sie bestimmte oder alle Benutzer in der Liste aus. Aktivieren Sie zum Auswählen bestimmter Benutzer die entsprechenden Kontrollkästchen in der ersten Spalte der Liste. Aktivieren Sie zum Auswählen aller Benutzer das Kontrollkästchen vor dem Titel der ersten Spalte (**Name**), oder aktivieren Sie alle Kontrollkästchen für alle Benutzer in der Liste. In dieser Liste wird der **Einladungsstatus** angezeigt.  In der folgenden Abbildung ist der Einladungsstatus für alle Kursteilnehmer auf **Invitation not sent** (Einladung nicht gesendet) festgelegt. 

    ![Auswählen von Kursteilnehmern](../media/tutorial-setup-classroom-lab/select-students.png)
1. Wählen Sie in einer Zeile das **E-Mail-Symbol (Briefumschlag)** oder auf der Symbolleiste die Option **Einladung senden** aus. Sie können mit dem Mauszeiger auch auf den Namen eines Kursteilnehmers in der Liste zeigen, um das E-Mail-Symbol anzuzeigen. 

    ![Senden eines Registrierungslinks per E-Mail](../media/tutorial-setup-classroom-lab/send-email.png)
4. Führen Sie auf der Seite **Send registration link by email** (Registrierungslink per E-Mail senden) die folgenden Schritte aus: 
    1. Geben Sie eine **optionale Nachricht** ein, die an die Kursteilnehmer gesendet werden soll. Die E-Mail enthält automatisch den Registrierungslink. 
    2. Wählen Sie auf der Seite **Send registration link by email** (Registrierungslink per E-Mail senden) die Option **Senden** aus. Der Status der Einladung wird in **Einladung wird gesendet** und anschließend in **Die Einladung wurde gesendet** geändert. 
        
        ![Gesendete Einladungen](../media/tutorial-setup-classroom-lab/invitations-sent.png)

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie ein Classroom-Lab erstellt und konfiguriert. Um zu erfahren, wie ein Teilnehmer auf einen virtuellen Computer im Labor über den Registrierungslink zugreifen kann, fahren Sie mit nächsten Tutorial fort:

> [!div class="nextstepaction"]
> [Herstellen einer Verbindung mit einem virtuellen Computer im Classroom-Lab](tutorial-connect-virtual-machine-classroom-lab.md)

