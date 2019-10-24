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
ms.date: 10/12/2019
ms.author: spelluru
ms.openlocfilehash: 691907d1c221283f99ba59f0937cfbaf673f427a
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72324392"
---
# <a name="add-and-manage-lab-users"></a>Hinzufügen und Verwalten von Labbenutzern
In diesem Artikel wird beschrieben, wie Sie dem Lab Benutzer hinzufügen, diese beim Lab registrieren, die Anzahl der Stunden steuern, für die sie den virtuellen Computer verwenden können, und vieles mehr. 


## <a name="add-users-to-the-lab"></a>Hinzufügen von Benutzern zum Lab

1. Klicken Sie im linken Menü auf **Benutzer**. Die Option **Zugriff beschränken** ist standardmäßig aktiviert. Wenn diese Einstellung aktiviert ist, kann sich ein Benutzer auch dann nicht beim Lab registrieren, wenn er über den Registrierungslink verfügt. Um sich zu registrieren, muss er in der Liste der Benutzer enthalten sein. Nur Benutzer in der Liste können sich über den von Ihnen gesendeten Registrierungslink beim Lab registrieren. In diesem Verfahren fügen Sie der Liste Benutzer hinzu. Alternativ können Sie **Zugriff beschränken** deaktivieren. In diesem Fall können sich Benutzer beim Lab registrieren, wenn sie über den Registrierungslink verfügen. 
2. Wählen Sie auf der Symbolleiste die Option **Benutzer hinzufügen** und anschließend **Add by email addresses** (Nach E-Mail-Adressen hinzufügen) aus. 

    ![Schaltfläche „Benutzer hinzufügen“](../media/how-to-configure-student-usage/add-users-button.png)
1. Geben Sie auf der Seite **Benutzer hinzufügen** die E-Mail-Adressen von Benutzern in separaten Zeilen oder durch Semikolons getrennt in einer einzelnen Zeile ein. 

    ![Hinzufügen der E-Mail-Adressen von Benutzern](../media/how-to-configure-student-usage/add-users-email-addresses.png)
4. Wählen Sie **Speichern** aus. In der Liste werden die E-Mail-Adressen und der Status (registriert oder nicht registriert) von Benutzern angezeigt. 

    ![Benutzerliste](../media/how-to-configure-student-usage/users-list-new.png)

### <a name="add-users-by-uploading-a-csv-file"></a>Hinzufügen von Benutzern durch Hochladen einer CSV-Datei
Sie können auch eine CSV-Datei mit E-Mail-Adressen von Benutzern hochladen, um Benutzer hinzuzufügen.

1. Erstellen Sie eine CSV-Datei mit E-Mail-Adressen von Benutzern in einer Spalte.

    ![CSV-Datei mit Benutzern](../media/how-to-configure-student-usage/csv-file-with-users.png)
2. Wählen Sie auf der Seite **Benutzer** des Labs auf der Symbolleiste die Option **Benutzer hinzufügen** und dann **CSV hochladen** aus.

    ![Schaltfläche „CSV hochladen“](../media/how-to-configure-student-usage/upload-csv-button.png)
3. Wählen Sie die CSV-Datei mit den Benutzer-E-Mail-Adressen aus. Wenn Sie nach Auswahl der CSV-Datei den Befehl **Öffnen** wählen, sehen Sie das folgende Fenster **Benutzer hinzufügen**. Die Liste der E-Mail-Adressen wird mit E-Mail-Adressen aus der CSV-Datei aufgefüllt. 

    ![Mit E-Mail-Adressen aus der CSV-Datei aufgefülltes Fenster „Benutzer hinzufügen“](../media/how-to-configure-student-usage/add-users-window.png)
4. Klicken Sie im Fenster **Benutzer hinzufügen** auf **Speichern**. 
5. Vergewissern Sie sich, dass in der Benutzerliste Benutzer angezeigt werden. 

    ![Liste der hinzugefügten Benutzer](../media/how-to-configure-student-usage/list-of-added-users.png)

## <a name="send-invitations-to-users"></a>Senden von Einladungen an Benutzer
Um den Registrierungslink an Kursteilnehmer zu senden, verwenden Sie eine der folgenden Methoden. Die erste Methode zeigt, wie Sie E-Mails mit dem Registrierungslink und einer optionalen Nachricht an Kursteilnehmer senden können. Die zweite Methode zeigt, wie Sie den Registrierungslink erhalten, den Sie mit anderen auf beliebige Weise teilen können. 

Wenn die **Zugriffsbeschränkung** für das Lab aktiviert ist, können sich nur Benutzer in der Liste der Benutzer über den Registrierungslink beim Lab registrieren. Diese Option ist standardmäßig aktiviert. 

### <a name="invite-all-users"></a>Einladen aller Benutzer

1. Wechseln Sie zur Seite **Benutzer** des Labs. 
2. Wählen Sie auf der Symbolleiste **Invite all** (Alle einladen) aus. 
3. Geben Sie eine **Nachricht** an die Benutzer ein. Dieser Schritt ist optional.
4. Wählen Sie dann **Senden** aus.

    ![Einladen aller Benutzer](../media/how-to-configure-student-usage/invite-all.png)

    Der Status dieses Vorgangs wird in der Liste **Benutzer** in der Spalte **Einladung** angezeigt. Die Einladungs-E-Mail enthält den Registrierungslink, über den sich Benutzer beim Lab registrieren können. 

### <a name="invite-selected-users"></a>Einladen ausgewählter Benutzer

1. Wählen Sie einen oder mehrere Benutzer in der Liste aus. 
2. Wählen Sie dann in der ausgewählten Zeile den **Briefumschlag** oder auf der Symbolleiste die Option **Einladen** aus. 

    ![Einladen ausgewählter Benutzer](../media/how-to-configure-student-usage/invite-selected-users.png)
3. Geben Sie im Fenster **Send invitation by email** (Einladung per E-Mail senden) eine optionale **Nachricht** ein, und wählen Sie anschließend die Option **Senden** aus. 

    ![Senden einer E-Mail an ausgewählte Benutzer](../media/how-to-configure-student-usage/send-invitation-to-selected-users.png)

    Der Status dieses Vorgangs wird in der Liste **Benutzer** in der Spalte **Einladung** angezeigt. Die Einladungs-E-Mail enthält den Registrierungslink, über den sich Benutzer beim Lab registrieren können.

1. Wechseln Sie zur Ansicht **Benutzer**, falls Sie sich noch nicht auf der Seite befinden. 

## <a name="get-registration-link"></a>Abrufen des Registrierungslinks
Sie können den Registrierungslink auch über das Portal abrufen und mithilfe Ihrer eigenen E-Mail-Clientanwendung senden. 

1. Wählen Sie im linken Menü die Option **Benutzer** aus, um zur Ansicht **Benutzer** zu wechseln. 
2. Wählen Sie auf der Symbolleiste die Auslassungspunkte ( **...** ) und dann **Registration link** (Registrierungslink) aus.

    ![Registrierungslink für Teilnehmer](../media/how-to-configure-student-usage/registration-link-button.png)
1. Wählen Sie im Dialogfeld **Benutzerregistrierung** die Schaltfläche **Kopieren** aus. Der Link wird in die Zwischenablage kopiert. Fügen Sie ihn in einen E-Mail-Editor ein, und senden Sie eine E-Mail an den Teilnehmer. 

    ![Registrierungslink für Teilnehmer](../media/how-to-configure-student-usage/registration-link.png)
2. Wählen Sie im Dialogfeld **Benutzerregistrierung** die Option **Fertig** aus. 
4. Senden Sie den **Registrierungslink** an einen Kursteilnehmer, damit sich diese Person für die Klasse registrieren kann. 

## <a name="view-users-registered-with-the-lab"></a>Anzeigen der beim Lab registrierten Benutzer

Wählen Sie **Benutzer** im linken Menü, um die Liste der Benutzer anzuzeigen, die beim Lab registriert sind. 

![Liste der beim Lab registrierten Benutzer](../media/how-to-configure-student-usage/students-registered.png)

## <a name="set-quotas-for-users"></a>Festlegen von Kontingenten für Benutzer
Mithilfe der folgenden Schritte können Sie Kontingente pro Benutzer festlegen: 

1. Wählen Sie im linken Menü **Benutzer** aus, wenn die Seite noch nicht aktiv ist. 
2. Wählen Sie auf der Symbolleiste die Option **Kontingent pro Benutzer: &lt;Anzahl&gt; Stunden** aus. 
3. Geben Sie auf der Seite **Kontingent pro Benutzer** die Anzahl der Stunden an, die Sie jedem Benutzer (Kursteilnehmer) außerhalb der planmäßigen Kursstunden zuteilen möchten, und wählen Sie dann **Speichern** aus.

    ![Kontingent pro Benutzer](../media/how-to-configure-student-usage/quota-per-user.png)    
5. Jetzt sehen Sie auf der Symbolleiste die geänderten Werte: **Kontingent pro Benutzer: &lt;Anzahl Stunden&gt;** . 

    ![Kontingent pro Benutzer: nach der Änderung](../media/how-to-configure-student-usage/quot-per-user-after.png)

    > [!IMPORTANT]
    > Die geplante Ausführungszeit von virtuellen Computern wird nicht mit dem [Kontingent eines Benutzers](how-to-create-schedules.md) verrechnet. Das Kontingent dient für die Zeiten außerhalb der Stunden im Zeitplan, die ein Kursteilnehmer mit VMs verbringt. 

## <a name="set-additional-quota-for-a-specific-user"></a>Festlegen eines zusätzlichen Kontingents für einen bestimmten Benutzer
Sie können für einen Benutzer ein separates Kontingent festlegen. Gehen Sie dazu folgendermaßen vor:

1. Wählen Sie auf der Seite **Benutzer** in der Liste der Benutzer einen Benutzer (Kursteilnehmer) aus.
2. Wählen Sie dann auf der Symbolleiste **Adjust quota** (Kontingent anpassen) aus. 

    ![Schaltfläche „Adjust quota“ (Kontingent anpassen)](../media/how-to-configure-student-usage/adjust-quota-button.png)
3. Geben Sie unter **Additional hours** (Zusätzliche Stunden) die zusätzlichen Stunden für den ausgewählten Benutzer ein, und wählen Sie dann **Anwenden** aus. 

    ![Zusätzliches Kontingent für einen Benutzer](../media/how-to-configure-student-usage/additional-quota.png)
4. Die aktualisierte Nutzung für den Benutzer wird in der Spalte **Nutzung** angezeigt. 

    ![Neue Nutzung für den Benutzer](../media/how-to-configure-student-usage/new-usage-hours.png)


## <a name="next-steps"></a>Nächste Schritte
Entsprechende Informationen finden Sie in den folgenden Artikeln:

- [Erstellen und Verwalten von Labkonten als Administrator](how-to-manage-lab-accounts.md)
- [Erstellen und Verwalten von Labs als Labbesitzer](how-to-manage-classroom-labs.md)
- [Einrichten und Veröffentlichen von Vorlagen als Labbesitzer](how-to-create-manage-template.md)
- [Zugreifen auf ein Classroom-Lab in Azure Lab Services](how-to-use-classroom-lab.md) (als Labbenutzer)
