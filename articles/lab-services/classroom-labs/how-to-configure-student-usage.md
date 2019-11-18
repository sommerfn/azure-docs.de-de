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
ms.openlocfilehash: 0ab8d8688c7856eeae7d75527620c2b77ae78029
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73584137"
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

    > [!NOTE]
    > Nach der Registrierung für das Lab werden die Namen von Benutzern in der Liste angezeigt. Der in der Liste angezeigte Name wird aus Vor- und Nachnamen des Benutzers im Azure Active Directory zusammengesetzt. 

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
1. Wechseln Sie zur Ansicht **Benutzer**, falls Sie sich nicht bereits auf der Seite befinden, und wählen Sie in der Symbolleiste die Option **Invite all** (Alle einladen) aus. 

    ![Auswählen von Kursteilnehmern](../media/tutorial-setup-classroom-lab/invite-all-button.png)

1. Geben Sie auf der Seite **Send invitation by email** (Einladung per E-Mail senden) eine optionale Nachricht ein, und wählen Sie anschließend die Option **Senden** aus. Die E-Mail enthält automatisch den Registrierungslink. Sie erhalten diesen Registrierungslink, indem Sie in der Symbolleiste die Option **...** (Auslassungszeichen) und dann **Registrierungslink** auswählen. 

    ![Senden eines Registrierungslinks per E-Mail](../media/tutorial-setup-classroom-lab/send-email.png)
4. Der Status der **Einladung** wird in der Liste **Benutzer** angezeigt. Der Status sollte sich in **Wird gesendet** und dann in **Gesendet am \<Datum>** ändern. 

    Weitere Informationen zum Hinzufügen von Kursteilnehmern zu einer Klasse sowie zum Verwalten der Verwendung des Labs finden Sie unter [Hinzufügen und Verwalten von Labbenutzern](how-to-configure-student-usage.md).

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
2. Wählen Sie auf der Symbolleiste die Option **Kontingent pro Benutzer:\< Anzahl > Stunden** aus. 
3. Geben Sie auf der Seite **Kontingent pro Benutzer** die Anzahl der Stunden an, die Sie jedem Benutzer (Kursteilnehmer) außerhalb der planmäßigen Kursstunden zuteilen möchten, und wählen Sie dann **Speichern** aus.

    ![Kontingent pro Benutzer](../media/how-to-configure-student-usage/quota-per-user.png)    
5. Jetzt sehen Sie auf der Symbolleiste die geänderten Werte: **Kontingent pro Benutzer: \<Anzahl der Stunden>** . 

    ![Kontingent pro Benutzer: nach der Änderung](../media/how-to-configure-student-usage/quot-per-user-after.png)

    > [!IMPORTANT]
    > Die geplante Ausführungszeit von virtuellen Computern wird nicht mit dem [Kontingent eines Benutzers](how-to-create-schedules.md) verrechnet. Das Kontingent dient für die Zeiten außerhalb der Stunden im Zeitplan, die ein Kursteilnehmer mit VMs verbringt. 

## <a name="set-additional-quota-for-a-specific-user"></a>Festlegen eines zusätzlichen Kontingents für einen bestimmten Benutzer
Sie können für einen Benutzer ein zusätzliches Kontingent angeben. Dieses Kontingent gilt zusätzlich zu dem im vorherigen Abschnitt für alle Benutzer festgelegten gemeinsamen Kontingent. Wenn Sie beispielsweise (als Dozent) das Kontingent für alle Benutzer auf 10 Stunden und ein zusätzliches Kontingent von 5 Stunden für einen bestimmten Benutzer festlegen, erhält dieser Benutzer ein Kontingent von 15 (10 + 5) Stunden. Wenn Sie das gemeinsame Kontingent später auf z. B. 15 ändern, dann erhält der Benutzer ein Kontingent von 20 (15 + 5) Stunden. Beachten Sie, dass dieses Gesamtkontingent außerhalb der planmäßigen Kursstunden liegt. Die Zeit, die der Kursteilnehmer während der planmäßigen Kursstunden an einer Lab-VM verbringt, wird nicht auf dieses Kontingent angerechnet. 

Gehen Sie dazu folgendermaßen vor:

1. Wählen Sie auf der Seite **Benutzer** in der Liste der Benutzer einen Benutzer (Kursteilnehmer) aus.
2. Wählen Sie dann auf der Symbolleiste **Adjust quota** (Kontingent anpassen) aus. 

    ![Schaltfläche „Adjust quota“ (Kontingent anpassen)](../media/how-to-configure-student-usage/adjust-quota-button.png)
3. Geben Sie unter **Additional hours** (Zusätzliche Stunden) die zusätzlichen Stunden für den ausgewählten Benutzer ein, und wählen Sie dann **Anwenden** aus. 

    ![Zusätzliches Kontingent für einen Benutzer](../media/how-to-configure-student-usage/additional-quota.png)
4. Die aktualisierte Nutzung für den Benutzer wird in der Spalte **Nutzung** angezeigt. 

    ![Neue Nutzung für den Benutzer](../media/how-to-configure-student-usage/new-usage-hours.png)

## <a name="student-accounts"></a>Konten für Kursteilnehmer
Zum Hinzufügen von Kursteilnehmern zu einem Classroom-Lab verwenden Sie deren E-Mail-Konten. Die folgenden Arten von E-Mail-Konten können verwendet werden:

- Ein E-Mail-Konto für Kursteilnehmer, das vom Office 365-Azure Active Directory (AAD) Ihrer Universität bereitgestellt wird 
- Ein Microsoft-E-Mail-Konto, z. B. `@outlook.com`, `@hotmail.com`, `@msn.com` oder `@live.com`
- Ein Microsoft-fremdes E-Mail-Konto, z. B. ein von Yahoo oder Google bereitgestelltes. Diese Kontotypen müssen jedoch mit einem Microsoft-Konto verknüpft werden.
- Ein GitHub-Konto. Dieses Konto muss mit einem Microsoft-Konto verknüpft werden.

### <a name="using-a-non-microsoft-email-account"></a>Verwenden eines Microsoft-fremden E-Mail-Kontos
Kursteilnehmer können Microsoft-fremde E-Mail-Konten verwenden, um sich für ein Classroom-Lab zu registrieren und sich bei diesem anzumelden.  Die Registrierung erfordert jedoch, dass die Kursteilnehmer zunächst ein Microsoft-Konto erstellen, das mit ihrer Microsoft-fremden E-Mail-Adresse verknüpft ist.

Viele Kursteilnehmer verfügen möglicherweise bereits über ein Microsoft-Konto, das mit ihren Microsoft-fremden E-Mail-Adressen verknüpft ist. Beispielsweise verfügen Kursteilnehmer bereits über ein Microsoft-Konto, wenn sie ihre E-Mail-Adresse mit anderen Microsoft-Produkten oder -Diensten wie Office, Skype, OneDrive oder Windows verwendet haben.  

Wenn ein Kursteilnehmer auf die Registrierungs-URL klickt, um sich für einen Classroom zu registrieren, wird er nach seiner E-Mail-Adresse und seinem Kennwort gefragt. Wenn der Kursteilnehmer versucht, sich mit einem Microsoft-fremden Konto anzumelden, das nicht mit einem Microsoft-Konto verknüpft ist, erhält er die folgende Fehlermeldung: 

![Fehlermeldung](../media/how-to-configure-student-usage/cant-find-account.png)

Um sich für ein Microsoft-Konto anzumelden, sollten Kursteilnehmer [http://signup.live.com](http://signup.live.com) besuchen.  

> [!IMPORTANT]
> Wenn sich Kursteilnehmer bei einem Classroom-Lab anmelden, können sie kein Microsoft-Konto erstellen. Daher empfehlen wir Ihnen, diesen Anmelde-Link in die Registrierungs-E-Mail für das Classroom-Lab einzufügen, die Sie an Kursteilnehmer senden, die Microsoft-fremde Konten verwenden.

### <a name="using-a-github-account"></a>Verwenden eines GitHub-Kontos
Kursteilnehmer können auch ein vorhandenes GitHub-Konto verwenden, um sich für ein Classroom-Lab zu registrieren und bei diesem anzumelden. Wenn der Kursteilnehmer bereits über ein Microsoft-Konto verfügt, das mit seinem GitHub-Konto verknüpft ist, kann er sich anmelden und sein Kennwort angeben, wie im vorherigen Abschnitt gezeigt. Wenn er sein GitHub-Konto noch nicht mit einem Microsoft-Konto verknüpft hat, sollte er **Anmeldeoptionen** auswählen:

![Link „Anmeldeoptionen“](../media/how-to-configure-student-usage/signin-options.png)

Auf der Seite **Anmeldeoptionen** wählt er die Option **Mit GitHub anmelden** aus.

![Link „Mit GitHub anmelden“](../media/how-to-configure-student-usage/signin-github.png)

Schließlich wird er aufgefordert, ein Microsoft-Konto zu erstellen, das mit seinem GitHub-Konto verknüpft ist. Dies geschieht automatisch, wenn der Kursteilnehmer **Weiter** auswählt.  Anschließend wird der Kursteilnehmer sofort angemeldet und mit dem Classroom-Lab verbunden.

## <a name="next-steps"></a>Nächste Schritte
Entsprechende Informationen finden Sie in den folgenden Artikeln:

- [Erstellen und Verwalten von Labkonten als Administrator](how-to-manage-lab-accounts.md)
- [Erstellen und Verwalten von Labs als Labbesitzer](how-to-manage-classroom-labs.md)
- [Einrichten und Veröffentlichen von Vorlagen als Labbesitzer](how-to-create-manage-template.md)
- [Zugreifen auf ein Classroom-Lab in Azure Lab Services](how-to-use-classroom-lab.md) (als Labbenutzer)
