---
title: 'Erstellen genehmigungsbasierter automatisierter Workflows: Azure Logic Apps'
description: 'Tutorial: Erstellen eines genehmigungsbasierten automatisierten Workflows, mit dem Adressenlistenabonnements mithilfe von Azure Logic Apps verarbeitet werden'
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.manager: carmonm
ms.reviewer: klam, LADocs
ms.topic: tutorial
ms.custom: mvc
ms.date: 09/20/2019
ms.openlocfilehash: f720e22542533d17fc7ab581f8ba8d9c03a89570
ms.sourcegitcommit: d47a30e54c5c9e65255f7ef3f7194a07931c27df
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73025576"
---
# <a name="tutorial-create-automated-approval-based-workflows-by-using-azure-logic-apps"></a>Tutorial: Erstellen automatisierter genehmigungsbasierter Workflows mithilfe von Azure Logic Apps

In diesem Tutorial wird gezeigt, wie Sie eine [Logik-App](../logic-apps/logic-apps-overview.md) erstellen, die einen genehmigungsbasierten Workflow automatisiert. Diese Logik-App verarbeitet gezielt Abonnementanforderungen für eine Adressenliste, die von dem Dienst [MailChimp](https://mailchimp.com/) verwaltet wird. Die Logik-App überwacht ein E-Mail-Konto auf diese Anforderungen, sendet sie zur Genehmigung und fügt genehmigte Mitglieder der Adressenliste hinzu.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer leeren Logik-App
> * Hinzufügen eines Triggers, der E-Mails auf Abonnementanforderungen überwacht
> * Hinzufügen einer Aktion, die E-Mails zur Genehmigung/Ablehnung dieser Anforderungen sendet
> * Hinzufügen einer Bedingung, die die Genehmigungsantwort überprüft
> * Hinzufügen einer Aktion, die genehmigte Mitglieder der Adressenliste hinzufügt
> * Hinzufügen einer Bedingung, die überprüft, ob die Mitglieder der Liste hinzugefügt wurden
> * Hinzufügen einer Aktion zum Senden von E-Mails, die darüber informieren, ob die Mitglieder der Liste hinzugefügt wurden

Am Ende entspricht Ihre Logik-App grob dem folgenden Workflow:

![Allgemeine Übersicht über die fertige Logik-App](./media/tutorial-process-mailing-list-subscriptions-workflow/tutorial-high-level-overview.png)

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Sollten Sie über kein Abonnement verfügen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/), bevor Sie beginnen.

* Ein MailChimp-Konto, das eine Liste namens „test-members-ML“ enthält, der Ihre Logik-App E-Mail-Adressen für genehmigte Mitglieder hinzufügen kann. Falls Sie über kein Konto verfügen, [registrieren Sie sich für ein kostenloses Konto](https://login.mailchimp.com/signup/), und informieren Sie sich dann darüber, [wie Sie eine MailChimp-Liste erstellen](https://us17.admin.mailchimp.com/lists/#).

* Ein E-Mail-Konto bei Office 365 Outlook oder Outlook.com, das Genehmigungsworkflows unterstützt. In diesem Artikel wird Office 365 Outlook verwendet. Bei Verwendung eines anderen E-Mail-Kontos bleiben die allgemeinen Schritte zwar gleich, die Benutzeroberfläche sieht aber unter Umständen etwas anders aus.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich mit den Anmeldeinformationen Ihres Azure-Kontos beim [Azure-Portal](https://portal.azure.com) an.

## <a name="create-your-logic-app"></a>Erstellen Ihrer Logik-App

1. Klicken Sie im Hauptmenü von Azure auf **Ressource erstellen** > **Integration** > **Logik-App**.

   ![Erstellen einer neuen Logik-App-Ressource](./media/tutorial-process-mailing-list-subscriptions-workflow/create-new-logic-app-resource.png)

1. Geben Sie unter **Logik-App erstellen** die Informationen wie beschrieben ein. Wählen Sie **Erstellen**, wenn Sie fertig sind.

   ![Angeben von Informationen zu Ihrer Logik-App](./media/tutorial-process-mailing-list-subscriptions-workflow/create-logic-app-settings.png)

   | Eigenschaft | Wert | BESCHREIBUNG |
   |----------|-------|-------------|
   | **Name** | LA-MailingList | Der Name Ihrer Logik-App. Er darf nur Buchstaben, Ziffern, Bindestriche (`-`), Unterstriche (`_`), Klammern (`(`, `)`) und Punkte (`.`) enthalten. In diesem Beispiel wird „LA-MailingList“ verwendet. |
   | **Abonnement** | <*Name Ihres Azure Abonnements*> | Der Name Ihres Azure-Abonnements |
   | **Ressourcengruppe** | LA-MailingList-RG | Der Name der [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md), die zum Organisieren verwandter Ressourcen verwendet wird. In diesem Beispiel wird „LA-MailingList-RG“ verwendet. |
   | **Location** | USA (Westen) | Die Region, in der die Informationen zu Ihrer Logik-App gespeichert werden sollen. In diesem Beispiel wird „USA, Westen“ verwendet. |
   | **Log Analytics** | Aus | Behalten Sie die Einstellung **Aus** für die Diagnoseprotokollierung bei. |
   ||||

1. Wählen Sie nach der Bereitstellung Ihrer App durch Azure für Ihre bereitgestellte Logik-App auf der Azure-Symbolleiste **Benachrichtigungen** > **Zu Ressource wechseln** aus.

   ![Navigieren zu Ihrer neuen Logik-App-Ressource](./media/tutorial-process-mailing-list-subscriptions-workflow/go-to-logic-app-resource.png)

   Sie können zum Suchen und Auswählen Ihrer Logik-App auch den Namen in das Suchfeld eingeben.

   Der Designer für Logik-Apps wird geöffnet. Es wird eine Seite mit einem Einführungsvideo sowie mit häufig verwendeten Triggern und Logik-App-Mustern angezeigt. Wählen Sie unter **Vorlagen** die Option **Leere Logik-App**.

   ![Auswählen der Vorlage „Leere Logik-App“](./media/tutorial-process-mailing-list-subscriptions-workflow/select-logic-app-template.png)

Fügen Sie als Nächstes einen [Trigger](../logic-apps/logic-apps-overview.md#logic-app-concepts) hinzu, der auf eingehende E-Mails mit Abonnementanforderungen lauscht. Jede Logik-App muss mit einem Trigger beginnen, der ausgelöst wird, wenn ein bestimmtes Ereignis eintritt oder neue Daten eine bestimmte Bedingung erfüllen. Weitere Informationen finden Sie unter [Erstellen Ihrer ersten Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="add-trigger-to-monitor-emails"></a>Hinzufügen eines Triggers zum Überwachen von E-Mails

1. Geben Sie im Designer für Logik-Apps `when email arrives` als Filter in das Suchfeld ein. Wählen Sie in der Liste **Trigger** den Trigger **Wenn eine neue E-Mail empfangen wird** für Ihren E-Mail-Anbieter aus.

   In diesem Beispiel wird der Trigger „Office 365 Outlook“ verwendet:

   ![Auswählen des Triggers „Wenn eine neue E-Mail empfangen wird“ für den E-Mail-Anbieter](./media/tutorial-process-mailing-list-subscriptions-workflow/add-trigger-new-email.png)

   * Wählen Sie für Geschäfts-, Schul- oder Unikonten für Azure die Option „Office 365 Outlook“.
   * Wählen Sie für persönliche Microsoft-Konten die Option „Outlook.com“.

1. Melden Sie sich bei entsprechender Aufforderung mit Ihren Anmeldeinformationen bei Ihrem E-Mail-Konto an, damit Logic Apps eine Verbindung mit Ihrem E-Mail-Konto erstellen kann.

1. Geben Sie im Trigger die Kriterien für die Überprüfung aller neuen E-Mails an.

   1. Geben Sie den Ordner, das Intervall und die Häufigkeit für die E-Mail-Überprüfung an.

      ![Angeben von Ordner, Intervall und Häufigkeit für die E-Mail-Überprüfung](./media/tutorial-process-mailing-list-subscriptions-workflow/add-trigger-set-up-email.png)

      | Eigenschaft | Wert | BESCHREIBUNG |
      |----------|-------|-------------|
      | **Ordner** | `Inbox` | Der zu überwachende E-Mail-Ordner. |
      | **Intervall** | `1` | Die Anzahl von Warteintervallen zwischen Überprüfungen |
      | **Frequency** | `Hour` | Die Zeiteinheit für die Wiederholung |
      ||||

   1. Fügen Sie dem Trigger nun eine weitere Eigenschaft hinzu, damit Sie nach E-Mail-Betreffzeile filtern können. Öffnen Sie die Liste **Neuen Parameter hinzufügen**, und wählen Sie die Eigenschaft **Betrefffilter** aus.

      ![Hinzufügen der Eigenschaft „Betrefffilter“ zum Trigger](./media/tutorial-process-mailing-list-subscriptions-workflow/add-trigger-add-properties.png)

      Weitere Informationen zu den Eigenschaften dieses Triggers finden Sie in der [Referenz zum Office 365 Outlook-Connector](https://docs.microsoft.com/connectors/office365/) oder in der [Referenz zum Outlook.com-Connector](https://docs.microsoft.com/connectors/outlook/).

   1. Wenn die Eigenschaft im Trigger angezeigt wird, geben Sie den folgenden Text ein: `subscribe-test-members-ML`.

      ![Eingeben von Text für die Eigenschaft „Betrefffilter“](./media/tutorial-process-mailing-list-subscriptions-workflow/add-trigger-subject-filter-property.png)

1. Klicken Sie auf die Titelleiste des Triggers, um die Triggerdetails vorerst auszublenden.

   ![Ausblenden der Details durch Reduzieren des Bereichs](./media/tutorial-process-mailing-list-subscriptions-workflow/collapse-trigger-shape.png)

1. Speichern Sie Ihre Logik-App. Wählen Sie auf der Symbolleiste des Designers **Speichern** aus.

Ihre Logik-App befindet sich jetzt im Livemodus, überprüft bislang aber lediglich Ihre eingehenden E-Mails. Fügen Sie daher eine Aktion hinzu, die reagiert, wenn der Trigger ausgelöst wird.

## <a name="send-approval-email"></a>Genehmigungs-E-Mail senden

Nachdem Sie nun über einen Trigger verfügen, fügen Sie als Nächstes eine [Aktion](../logic-apps/logic-apps-overview.md#logic-app-concepts) hinzu, die eine E-Mail zur Genehmigung oder Ablehnung der Anforderung sendet.

1. Wählen Sie unter dem Trigger die Option **Neuer Schritt** aus. 

1. Geben Sie unter **Aktion auswählen** im Suchfeld `approval` als Filter ein. Wählen Sie in der Liste der Aktionen die Aktion **Genehmigungs-E-Mail senden** für Ihren E-Mail-Anbieter aus. 

   In diesem Beispiel wird die Aktion „Office 365 Outlook“ verwendet:

   ![Auswählen der Aktion „Genehmigungs-E-Mail senden“](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-send-approval-email.png)

1. Geben Sie die Informationen zu dieser Aktion wie beschrieben an: 

   ![Eigenschaften für „Genehmigungs-E-Mail senden“](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-approval-email-settings.png)

   | Eigenschaft | Wert | BESCHREIBUNG |
   |----------|-------|-------------|
   | **To** | <*Ihre E-Mail-Adresse*> | Die E-Mail-Adresse der genehmigenden Person. Zu Testzwecken können Sie hier Ihre eigene Adresse angeben. In diesem Beispiel wird die fiktive E-Mail-Adresse „sophia.owen@fabrikam.com“ verwendet. |
   | **Subject** | `Approve member request for test-members-ML` | Ein aussagekräftiger E-Mail-Betreff. |
   | **Benutzeroptionen** | `Approve, Reject` | Die Antwortoptionen, zwischen denen die genehmigende Person wählen kann. Standardmäßig kann die genehmigende Person zwischen „Genehmigen“ und „Ablehnen“ wählen. |
   ||||

   Ignorieren Sie vorerst die Liste mit den dynamischen Inhalten, die beim Klicken auf bestimmte Bearbeitungsfelder angezeigt wird. In dieser Liste können Sie verfügbare Ausgaben aus vorherigen Aktionen auswählen, die als Eingabe in Ihrem Workflow verwendet werden können.

   Weitere Informationen zu den Eigenschaften dieser Aktion finden Sie in der [Referenz zum Office 365 Outlook-Connector](https://docs.microsoft.com/connectors/office365/) oder in der [Referenz zum Outlook.com-Connector](https://docs.microsoft.com/connectors/outlook/).
 
1. Speichern Sie Ihre Logik-App.

Fügen Sie als Nächstes eine Bedingung hinzu, die überprüft, welche Antwort die genehmigende Person gewählt hat.

## <a name="check-approval-response"></a>Überprüfen der Genehmigungsantwort

1. Wählen Sie unter der Aktion **Genehmigungs-E-Mail senden** die Option **Neuer Schritt** aus.

1. Wählen Sie unter **Aktion auswählen** die Option **Integriert** aus. Geben Sie im Suchfeld den Begriff `condition` als Filter ein. Wählen Sie in der Liste „Aktionen“ die Aktion **Bedingung** aus.

   ![Suchen und Auswählen der Aktion „Bedingung“](./media/tutorial-process-mailing-list-subscriptions-workflow/select-condition-action.png)

1. Versehen Sie die Bedingung mit einem aussagekräftigeren Namen.

   1. Klicken Sie auf der Titelleiste der Bedingung auf die **Schaltfläche mit den Auslassungspunkten** ( **...** ) und anschließend auf **Umbenennen**.

      ![Umbenennen der Beschreibung der Bedingung](./media/tutorial-process-mailing-list-subscriptions-workflow/rename-condition-description.png)

   1. Nennen Sie Ihre Bedingung wie folgt: `If request approved`

1. Erstellen Sie eine Bedingung, die überprüft, ob die genehmigende Person **Genehmigen** ausgewählt hat.

   1. Klicken Sie in der Bedingung auf das Feld **Wert auswählen** (links von der Bedingung).

   1. Wählen Sie in der angezeigten Liste mit dynamischen Inhalten unter **Genehmigungs-E-Mail senden** die Eigenschaft **SelectedOption** aus.

      ![Auswählen von „SelectedOption“ aus der Liste mit dynamischen Inhalten](./media/tutorial-process-mailing-list-subscriptions-workflow/build-condition-check-approval-response.png)

   1. Wählen Sie im mittleren Vergleichsfeld den Operator **Ist gleich** aus.

   1. Geben Sie auf der rechten Seite der Bedingung im Feld **Wert auswählen** den folgenden Text ein: `Approve`.

      Danach sieht die Bedingung wie im folgenden Beispiel aus:

      ![Fertige Bedingung für Beispiel „Genehmigt“](./media/tutorial-process-mailing-list-subscriptions-workflow/build-condition-check-approval-response-2.png)

1. Speichern Sie Ihre Logik-App.

Geben Sie als Nächstes die Aktion an, die Ihre Logik-App ausführen soll, wenn der Prüfer die Anforderung genehmigt. 

## <a name="add-member-to-mailchimp-list"></a>Hinzufügen eines Mitglieds zur MailChimp-Liste

Fügen Sie nun eine Aktion hinzu, die das genehmigte Mitglied Ihrer Adressenliste hinzufügt.

1. Wählen Sie in der Bedingungsverzweigung **Bei TRUE** die Option **Aktion hinzufügen** aus.

1. Geben Sie unter **Aktion auswählen** `mailchimp` als Filter ein, und wählen Sie die Aktion **Mitglied zu Liste hinzufügen** aus.

   ![Auswählen der Aktion „Mitglied zu Liste hinzufügen“](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-mailchimp-add-member.png)

1. Wenn Sie zum Zugriff auf Ihr MailChimp-Konto aufgefordert werden, melden Sie sich mit Ihren MailChimp-Anmeldeinformationen an.

1. Geben Sie Informationen zu dieser Aktion wie hier dargestellt und beschrieben an:

   ![Angeben der Informationen für „Mitglied zu Liste hinzufügen“](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-mailchimp-add-member-settings.png)

   | Eigenschaft | Erforderlich | Value | BESCHREIBUNG |
   |----------|----------|-------|-------------|
   | **Listen-ID** | Ja | `test-members-ML` | Der Name Ihrer MailChimp-Adressenliste. In diesem Beispiel wird „test-members-ML“ verwendet. |
   | **Status** | Ja | `subscribed` | Wählen Sie den Abonnementstatus für das neue Mitglied aus. In diesem Beispiel wird „Abonniert“ verwendet. <p>Weitere Informationen finden Sie unter [Manage Subscribers with the MailChimp API](https://developer.mailchimp.com/documentation/mailchimp/guides/manage-subscribers-with-the-mailchimp-api/) (Verwalten von Abonnenten mit der MailChimp-API). |
   | **E-Mail-Adresse** | Ja | <*E-Mail-Adresse des neuen Mitglieds*> | Wählen Sie in der Liste mit den dynamischen Inhalten unter **Wenn eine neue E-Mail empfangen wird** das Feld **Von** aus. Dadurch wird die E-Mail-Adresse des neuen Mitglieds übergeben. |
   ||||

   Weitere Informationen zu den Eigenschaften dieser Aktion finden Sie in der [Referenz zum MailChimp-Connector](https://docs.microsoft.com/connectors/mailchimp/).

1. Speichern Sie Ihre Logik-App.

Fügen Sie als Nächstes eine Bedingung hinzu, um zu prüfen, ob das neue Mitglied der Adressenliste hinzugefügt wurde. So erfahren Sie, ob der Vorgang erfolgreich war.

## <a name="check-for-success-or-failure"></a>Überprüfen, ob der Vorgang erfolgreich war

1. Wählen Sie in der Verzweigung **Bei TRUE** unter der Aktion **Mitglied zu Liste hinzufügen** die Option **Aktion hinzufügen** aus.

1. Wählen Sie unter **Aktion auswählen** die Option **Integriert** aus. Geben Sie im Suchfeld den Begriff `condition` als Filter ein. Wählen Sie in der Liste mit den Aktionen **Bedingung** aus.

1. Nennen Sie die Bedingung wie folgt: `If add member succeeded`

1. Erstellen Sie eine Bedingung, die überprüft, ob das genehmigte Mitglied der Adressenliste hinzugefügt wurde:

   1. Klicken Sie in der Bedingung auf das Feld **Wert auswählen** (links von der Bedingung). Wählen Sie in der Liste mit den dynamischen Inhalten unter **Mitglied zu Liste hinzufügen** die Eigenschaft **Status** aus.

      Ihre Bedingung sieht etwa wie im folgendem Beispiel aus:

      ![Auswählen von „Status“ unter „Mitglied zu Liste hinzufügen“](./media/tutorial-process-mailing-list-subscriptions-workflow/build-condition-check-added-member.png)

   1. Wählen Sie im mittleren Vergleichsfeld den Operator **Ist gleich** aus.

   1. Geben Sie auf der rechten Seite der Bedingung im Feld **Wert auswählen** den folgenden Text ein: `subscribed`.

      Danach sieht die Bedingung wie im folgenden Beispiel aus:

      ![Fertige Bedingung für Beispiel „Abonniert“](./media/tutorial-process-mailing-list-subscriptions-workflow/build-condition-check-added-member-2.png)

Richten Sie als Nächstes die E-Mails ein, die gesendet werden sollen, wenn das genehmigte Mitglied erfolgreich der Adressenliste hinzugefügt wurde oder wenn das Hinzufügen nicht erfolgreich war.

## <a name="send-email-if-member-added"></a>Senden einer E-Mail, wenn das Mitglied hinzugefügt wurde

1. Wählen Sie in der Verzweigung **Bei TRUE** für die Bedingung **If add member succeeded** die Option **Aktion hinzufügen** aus.

   ![Auswählen von „Aktion hinzufügen“ im Branch „Bei TRUE“](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-email-success.png)

1. Geben Sie unter **Aktion auswählen** im Suchfeld den Text `outlook send email` als Filter ein, und wählen Sie die Aktion **E-Mail senden** aus.

   ![Hinzufügen der Aktion „E-Mail senden“](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-email-success-2.png)

1. Nennen Sie die Aktion wie folgt: `Send email on success`

1. Geben Sie Informationen für diese Aktion wie hier dargestellt und beschrieben an:

   ![Angeben der Informationen für die Erfolgs-E-Mail](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-email-success-settings.png)

   | Eigenschaft | Erforderlich | Value | BESCHREIBUNG |
   |----------|----------|-------|-------------|
   | **To** | Ja | <*Ihre E-Mail-Adresse*> | Die E-Mail-Adresse, an die die Erfolgs-E-Mail gesendet werden soll. Zu Testzwecken können Sie hier Ihre eigene E-Mail-Adresse angeben. |
   | **Subject** | Ja | <*Betreff der Erfolgs-E-Mail*> | Der Betreff der Erfolgs-E-Mail. Geben Sie für dieses Tutorial den folgenden Text ein: <p>`Success! Member added to "test-members-ML": ` <p>Wählen Sie in der Liste mit den dynamischen Inhalten unter **Mitglied zu Liste hinzufügen** die Eigenschaft **E-Mail-Adresse** aus. |
   | **Text** | Ja | <*Text für Erfolgs-E-Mail*> | Der Inhalt der Erfolgs-E-Mail. Geben Sie für dieses Tutorial den folgenden Text ein: <p>`New member has joined "test-members-ML":` <p>Wählen Sie in der Liste mit den dynamischen Inhalten die Eigenschaft **E-Mail-Adresse** aus. <p>Geben Sie in der nächsten Zeile den folgenden Text ein: `Member opt-in status: `. <p> Wählen Sie in der Liste mit den dynamischen Inhalten unter **Mitglied zu Liste hinzufügen** die Eigenschaft **Status** aus. |
   |||||

1. Speichern Sie Ihre Logik-App.

## <a name="send-email-if-member-not-added"></a>Senden einer E-Mail, wenn das Mitglied nicht hinzugefügt wurde

1. Wählen Sie in der Verzweigung **Bei FALSE** für die Bedingung **If add member succeeded** die Option **Aktion hinzufügen** aus.

   ![Auswählen von „Aktion hinzufügen“ im Branch „Bei FALSE“](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-email-failed.png)

1. Geben Sie unter **Aktion auswählen** im Suchfeld den Text `outlook send email` als Filter ein, und wählen Sie die Aktion **E-Mail senden** aus.

   ![Hinzufügen einer Aktion für „E-Mail senden“](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-email-failed-2.png)

1. Nennen Sie die Aktion wie folgt: `Send email on failure`

1. Geben Sie Informationen zu dieser Aktion wie hier dargestellt und beschrieben an:

   ![Angeben der Informationen für die Fehler-E-Mail](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-email-failed-settings.png)

   | Eigenschaft | Erforderlich | Value | BESCHREIBUNG |
   |----------|----------|-------|-------------|
   | **To** | Ja | <*Ihre E-Mail-Adresse*> | Die E-Mail-Adresse, an die die Fehler-E-Mail gesendet werden soll. Zu Testzwecken können Sie hier Ihre eigene E-Mail-Adresse angeben. |
   | **Subject** | Ja | <*Betreff für Fehler-E-Mail*> | Der Betreff der Fehler-E-Mail. Geben Sie für dieses Tutorial den folgenden Text ein: <p>`Failed, member not added to "test-members-ML": ` <p>Wählen Sie in der Liste mit den dynamischen Inhalten unter **Mitglied zu Liste hinzufügen** die Eigenschaft **E-Mail-Adresse** aus. |
   | **Text** | Ja | <*Text für Fehler-E-Mail*> | Der Inhalt der Fehler-E-Mail. Geben Sie für dieses Tutorial den folgenden Text ein: <p>`Member might already exist. Check your MailChimp account.` |
   |||||

1. Speichern Sie Ihre Logik-App. 

Testen Sie als Nächstes Ihre Logik-App, die nun in etwa wie folgt aussieht:

![Beispiel für fertigen Logik-App-Workflow](./media/tutorial-process-mailing-list-subscriptions-workflow/tutorial-high-level-completed.png)

## <a name="run-your-logic-app"></a>Ausführen Ihrer Logik-App

1. Senden Sie sich selbst eine E-Mail mit der Anforderung, in Ihre Adressenliste aufgenommen zu werden. Warten Sie, bis die Anforderung in Ihrem Posteingang eingeht.

1. Wählen Sie auf der Symbolleiste des Designers die Option **Ausführen** aus, um Ihre Logik-App manuell zu starten. 

   Wenn der Betreff Ihrer E-Mail dem Betrefffilter des Triggers entspricht, sendet Ihre Logik-App Ihnen eine E-Mail zur Genehmigung der Abonnementanforderung.

1. Wählen Sie in der Genehmigungs-E-Mail die Option **Genehmigen** aus.

1. Wenn die E-Mail-Adresse des Abonnenten in Ihrer Adressenliste nicht vorhanden ist, fügt Ihre Logik-App die entsprechende E-Mail-Adresse hinzu und sendet Ihnen eine E-Mail. Beispiel:

   ![Beispiel-E-Mail: erfolgreiches Abonnement](./media/tutorial-process-mailing-list-subscriptions-workflow/add-member-mailing-list-success.png)

   Falls Ihre Logik-App den Abonnenten nicht hinzufügen kann, erhalten Sie eine E-Mail wie die folgende:

   ![Beispiel-E-Mail: fehlerhaftes Abonnement](./media/tutorial-process-mailing-list-subscriptions-workflow/add-member-mailing-list-failed.png)

   Sollten Sie keine E-Mails erhalten, überprüfen Sie Ihren Ordner für Junk-E-Mails. E-Mails dieser Art werden unter Umständen durch Ihren Junk-E-Mail-Filter umgeleitet. Wenn Sie unsicher sind, ob Ihre Logik-App richtig ausgeführt wurde, helfen Ihnen die Informationen unter [Diagnostizieren von Fehlern bei Logik-Apps](../logic-apps/logic-apps-diagnosing-failures.md) weiter.

Geschafft! Sie haben eine Logik-App erstellt und ausgeführt, die Informationen über Azure, Microsoft-Dienste und andere SaaS-Apps hinweg integriert.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie diese exemplarische Logik-App nicht mehr benötigen, löschen Sie die Ressourcengruppe, die Ihre Logik-App und die zugehörigen Ressourcen enthält. 

1. Wechseln Sie im Azure-Hauptmenü zu **Ressourcengruppen**, und wählen Sie die Ressourcengruppe für Ihre Logik-App aus.

1. Wählen Sie im Ressourcengruppenmenü **Übersicht** > **Ressourcengruppe löschen** aus. 

   ![„Übersicht“ > „Ressourcengruppe löschen“](./media/tutorial-process-mailing-list-subscriptions-workflow/delete-resource-group.png)

1. Geben Sie zur Bestätigung den Ressourcengruppennamen ein, und wählen Sie **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine Logik-App erstellt, die Genehmigungen für Adressenlistenanforderungen verwaltet. Erfahren Sie als Nächstes, wie Sie eine Logik-App erstellen, die E-Mail-Anlagen verarbeitet und speichert und Azure-Dienste wie Azure Storage und Azure Functions integriert.

> [!div class="nextstepaction"]
> [Verarbeiten von E-Mail-Anlagen](../logic-apps/tutorial-process-email-attachments-workflow.md)
