---
title: Konfigurieren von Nutzungseinstellungen in Classroom-Labs von Azure Lab Services | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Anzahl der Benutzer für das Lab konfigurieren, die Benutzer beim Lab registrieren, die Anzahl der Stunden steuern, in denen sie den virtuellen Computer verwenden können, und vieles mehr.
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
ms.date: 06/11/2019
ms.author: spelluru
ms.openlocfilehash: 67faf268d265fd045c21b75b6f64840511a371d3
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67067315"
---
# <a name="configure-usage-settings-and-policies"></a>Konfigurieren von Nutzungseinstellungen und Richtlinien
In diesem Artikel wird beschrieben, wie Sie dem Lab Benutzer hinzufügen, diese beim Lab registrieren, die Anzahl der Stunden steuern, für die sie den virtuellen Computer verwenden können, und vieles mehr. 


## <a name="add-users-to-the-lab"></a>Hinzufügen von Benutzern zum Lab
Wenn Sie **Zugriff beschränken** aktiviert haben, fügen Sie der Liste Benutzer (E-Mail-Adressen) hinzu.

1. Klicken Sie im linken Menü auf **Benutzer**.
2. Klicken Sie auf der Symbolleiste auf **Benutzer hinzufügen**. 

    ![Schaltfläche „Benutzer hinzufügen“](../media/how-to-configure-student-usage/add-users-button.png)
1. Geben Sie auf der Seite **Benutzer hinzufügen** die E-Mail-Adressen von Benutzern in separaten Zeilen oder durch Semikolons getrennt in einer einzelnen Zeile ein. 

    ![Hinzufügen der E-Mail-Adressen von Benutzern](../media/how-to-configure-student-usage/add-users-email-addresses.png)
4. Wählen Sie **Speichern** aus. In der Liste werden die E-Mail-Adressen und der Status (registriert oder nicht registriert) von Benutzern angezeigt. 

    ![Benutzerliste](../media/how-to-configure-student-usage/users-list-new.png)

## <a name="share-registration-link-with-students"></a>Teilen eines Registrierungslinks mit Kursteilnehmern
Um den Registrierungslink an Kursteilnehmer zu senden, verwenden Sie eine der folgenden Methoden. Die erste Methode zeigt, wie Sie E-Mails mit dem Registrierungslink und einer optionalen Nachricht an Kursteilnehmer senden können. Die zweite Methode zeigt, wie Sie den Registrierungslink erhalten, den Sie mit anderen auf beliebige Weise teilen können. 

Wenn die **Zugriffsbeschränkung** für das Lab aktiviert ist, können sich nur Benutzer in der Liste der Benutzer über den Registrierungslink beim Lab registrieren. Diese Option ist standardmäßig aktiviert. 

### <a name="send-email-to-users"></a>E-Mail an Benutzer senden
Azure Lab Services ermöglicht es Kursleitern, Lab-Einladungen per E-Mail an alle oder ausgewählte Kursteilnehmer zu senden, ohne einen anderen E-Mail-Client verwenden zu müssen. Kursleiter können den Mauszeiger über einzelne Kursteilnehmer in der Liste bewegen, um das E-Mail-Symbol für die einzelnen Kursteilnehmer anzuzeigen, oder einen oder mehrere Kursteilnehmer auszuwählen, und **Sent invitation** (Einladung senden) in der Symbolleiste verwenden. Dieses Feature sendet eine E-Mail mit einem Registrierungslink und einer Nachricht (falls vorhanden), die vom Kursleiter hinzugefügt wurde. Nachdem die Einladung gesendet wurde, ändert sich der Einladungsstatus in **Invitation sent** (Einladung gesendet), sodass Kursleiter verfolgen können, welche Kursteilnehmer den Registrierungslink bereits erhalten haben und an welchem Datum der Link gesendet wurde.

1. Wechseln Sie zur Ansicht **Benutzer**, falls Sie sich noch nicht auf der Seite befinden. 
2. Wählen Sie bestimmte oder alle Benutzer in der Liste aus. Aktivieren Sie zum Auswählen bestimmter Benutzer die entsprechenden Kontrollkästchen in der ersten Spalte der Liste. Aktivieren Sie zum Auswählen aller Benutzer das Kontrollkästchen vor dem Titel der ersten Spalte (**Name**), oder aktivieren Sie alle Kontrollkästchen für alle Benutzer in der Liste. In dieser Liste wird der **Einladungsstatus** angezeigt.  In der folgenden Abbildung ist der Einladungsstatus für alle Kursteilnehmer auf **Invitation not sent** (Einladung nicht gesendet) festgelegt. 

    ![Auswählen von Kursteilnehmern](../media/tutorial-setup-classroom-lab/select-students.png)
1. Wählen Sie in einer Zeile das **E-Mail-Symbol (Briefumschlag)** oder auf der Symbolleiste die Option **Einladung senden** aus. Sie können mit dem Mauszeiger auch auf den Namen eines Kursteilnehmers in der Liste zeigen, um das E-Mail-Symbol anzuzeigen. 

    ![Senden eines Registrierungslinks per E-Mail](../media/tutorial-setup-classroom-lab/send-email.png)
4. Führen Sie auf der Seite **Send registration link by email** (Registrierungslink per E-Mail senden) die folgenden Schritte aus: 
    1. Geben Sie eine **optionale Nachricht** ein, die an die Kursteilnehmer gesendet werden soll. Die E-Mail enthält automatisch den Registrierungslink. 
    2. Wählen Sie auf der Seite **Send registration link by email** (Registrierungslink per E-Mail senden) die Option **Senden** aus. Der Status der Einladung wird in **Einladung wird gesendet** und anschließend in **Die Einladung wurde gesendet** geändert. 
        
        ![Gesendete Einladungen](../media/tutorial-setup-classroom-lab/invitations-sent.png)

## <a name="get-registration-link"></a>Abrufen des Registrierungslinks
1. Wählen Sie im linken Menü die Option **Benutzer** aus, um zur Ansicht **Benutzer** zu wechseln. 
2. Wählen Sie die Kachel **Get registration link** (Registrierungslink abrufen) aus.

    ![Registrierungslink für Teilnehmer](../media/tutorial-setup-classroom-lab/dashboard-user-registration-link.png)
1. Klicken Sie im Dialogfeld **Benutzerregistrierung** auf die Schaltfläche **Kopieren**. Der Link wird in die Zwischenablage kopiert. Fügen Sie ihn in einen E-Mail-Editor ein, und senden Sie eine E-Mail an den Teilnehmer. 

    ![Registrierungslink für Teilnehmer](../media/tutorial-setup-classroom-lab/registration-link.png)
2. Wählen Sie im Dialogfeld **Benutzerregistrierung** die Option **Schließen**. 
4. Teilen Sie den **Registrierungslink** mit einem Kursteilnehmer, damit diese Person sich für die Klasse registrieren kann. 

## <a name="view-users-registered-with-the-lab"></a>Anzeigen der beim Lab registrierten Benutzer

Wählen Sie **Benutzer** im linken Menü, um die Liste der Benutzer anzuzeigen, die beim Lab registriert sind. 

![Liste der beim Lab registrierten Benutzer](../media/how-to-configure-student-usage/users-list-new.png)

## <a name="set-quotas-per-user"></a>Festlegen von Kontingenten pro Benutzer
Mithilfe der folgenden Schritte können Sie Kontingente pro Benutzer festlegen: 

1. Klicken Sie im linken Menü auf **Benutzer**.
2. Wählen Sie auf der Symbolleiste die Option **Kontingent pro Benutzer:** aus. 
3. Geben Sie auf der Seite **Kontingent pro Benutzer** die Anzahl der Stunden an, die Sie jedem Benutzer (Kursteilnehmer) zuteilen möchten: 
    1. **0 Stunden (nur Zeitplan)** . Benutzer können ihre VMs nur während der geplanten Zeit nutzen, oder wenn Sie als Lab-Besitzer die VMs für sie einschalten.

        ![Null Stunden – nur geplante Zeit](../media/how-to-configure-student-usage/zero-hours.png)
    1. **Gesamtzahl der Lab-Stunden pro Benutzer**. Benutzer können ihre VMs **zusätzlich zur geplanten Zeit** für die festgelegte (in diesem Feld angegebene) Anzahl von Stunden nutzen. Wenn Sie diese Option auswählen, geben Sie die **Anzahl der Stunden** in das Textfeld ein. 

        ![Anzahl von Stunden pro Benutzer](../media/how-to-configure-student-usage/number-of-hours-per-user.png)
    4. Wählen Sie **Speichern** aus. 
5. Jetzt sehen Sie auf der Symbolleiste die geänderten Werte: **Kontingent pro Benutzer: &lt;Anzahl Stunden&gt;** . 

    ![Kontingent pro Benutzer](../media/how-to-configure-student-usage/quota-per-user.png)



> [!IMPORTANT]
> Bevor der Registrierungslink an die Kursteilnehmer gesendet wird, müssen die Lehrkräfte entweder den Zeitplan für den Kurs festlegen, falls sie 0 Kontingentstunden auswählen, oder die Kontingentstunden für das Lab angeben.
>
> Die geplante Ausführungszeit von virtuellen Computern wird nicht mit dem [Kontingent eines Benutzers](how-to-create-schedules.md) verrechnet. Das Kontingent dient für die Zeiten außerhalb der Stunden im Zeitplan, die ein Kursteilnehmer mit VMs verbringt. 

### <a name="add-users-by-uploading-a-csv-file"></a>Hinzufügen von Benutzern durch Hochladen einer CSV-Datei
Sie können auch eine CSV-Datei mit E-Mail-Adressen von Benutzern hochladen, um Benutzer hinzuzufügen.

1. Erstellen Sie eine CSV-Datei mit E-Mail-Adressen von Benutzern in einer Spalte.

    ![Kontingent pro Benutzer](../media/how-to-configure-student-usage/csv-file-with-users.png)
2. Wählen Sie auf der Seite **Benutzer** des Labs auf der Symbolleiste die Option **CSV hochladen** aus.

    ![Schaltfläche „CSV hochladen“](../media/how-to-configure-student-usage/upload-csv-button.png)
3. Wählen Sie die CSV-Datei mit den Benutzer-E-Mail-Adressen aus. Wenn Sie nach Auswahl der CSV-Datei den Befehl **Öffnen** wählen, sehen Sie das folgende Fenster **Benutzer hinzufügen**. Die Liste der E-Mail-Adressen wird mit E-Mail-Adressen aus der CSV-Datei aufgefüllt. 

    ![Mit E-Mail-Adressen aus der CSV-Datei aufgefülltes Fenster „Benutzer hinzufügen“](../media/how-to-configure-student-usage/add-users-window.png)
4. Klicken Sie im Fenster **Benutzer hinzufügen** auf **Speichern**. 
5. Vergewissern Sie sich, dass in der Benutzerliste Benutzer angezeigt werden. 

    ![Liste der hinzugefügten Benutzer](../media/how-to-configure-student-usage/list-of-added-users.png)

## <a name="manage-user-vms"></a>Verwalten von Benutzer-VMs
Sobald sich die Studenten bei Azure Lab Services über den von Ihnen bereitgestellten Registrierungslink registrieren, sehen Sie die den Studenten zugewiesenen VMs auf der Registerkarte **Virtuelle Computer**. 

![Studenten zugewiesene virtuelle Computer](../media/how-to-manage-classroom-labs/virtual-machines-students.png)

Sie können folgende Aufgaben auf einer Studenten-VM ausführen: 

- Beenden einer VM, wenn diese ausgeführt wird. 
- Starten einer VM, wenn diese beendet wurde. 
- Stellen Sie eine Verbindung zur VM her. 
- Löschen Sie den virtuellen Computer. 
- Zeigen Sie die Anzahl der Stunden an, für die Benutzer den virtuellen Computer verwendet haben. 

## <a name="update-number-of-virtual-machines-in-lab"></a>Aktualisieren der Anzahl von virtuellen Computern im Lab
Um die Anzahl der virtuellen Computer im Lab zu aktualisieren, gehen Sie auf der Seite **Virtuelle Computer** folgendermaßen vor:

1. Wählen Sie im linken Menü **Virtuelle Computer** aus. 
2. Wählen Sie auf der Symbolleiste **Lab capacity (Labkapazität): &lt;Anzahl&gt; Computer** aus. 
3. Geben Sie die **Anzahl** von virtuellen Computern ein.
4. Wählen Sie **Speichern** aus.

    ![Virtuelle Computer im Lab](../media/how-to-configure-student-usage/number-virtual-machines.png)


## <a name="next-steps"></a>Nächste Schritte
Entsprechende Informationen finden Sie in den folgenden Artikeln:

- [Erstellen und Verwalten von Labkonten als Administrator](how-to-manage-lab-accounts.md)
- [Erstellen und Verwalten von Labs als Labbesitzer](how-to-manage-classroom-labs.md)
- [Einrichten und Veröffentlichen von Vorlagen als Labbesitzer](how-to-create-manage-template.md)
- [Zugreifen auf ein Classroom-Lab in Azure Lab Services](how-to-use-classroom-lab.md) (als Labbenutzer)
