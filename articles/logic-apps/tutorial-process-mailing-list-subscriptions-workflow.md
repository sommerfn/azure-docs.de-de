---
title: Erstellen von Genehmigungsworkflows für die Verarbeitung von Adressenlistenanforderungen – Azure Logic Apps | Microsoft-Dokumentation
description: 'Tutorial: Erstellen automatisierter Genehmigungsworkflows für die Verarbeitung von Adressenlistenabonnements mit Azure Logic Apps'
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: tutorial
ms.custom: mvc
ms.date: 01/12/2018
ms.openlocfilehash: 2b28c38d2444f227d26df1f9ca2d70876ff41064
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2019
ms.locfileid: "68260597"
---
# <a name="manage-mailing-list-requests-with-azure-logic-apps"></a>Verwalten von Adressenlistenanforderungen mit Azure Logic Apps

Mit Azure Logic Apps können Sie Workflows automatisieren und Daten übergreifend in Azure-Dienste, Microsoft-Dienste, andere SaaS-Apps (Software-as-a-Service) und lokale Systeme integrieren. In diesem Tutorial erfahren Sie, wie Sie eine [Logik-App](../logic-apps/logic-apps-overview.md) erstellen, die Abonnementanforderungen für eine Adressenliste verarbeitet, die von dem Dienst [MailChimp](https://mailchimp.com/) verwaltet wird.
Die Logik-App überwacht ein E-Mail-Konto auf diese Anforderungen, sendet sie zur Genehmigung und fügt genehmigte Mitglieder der Adressenliste hinzu.

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

![Allgemeine Darstellung der fertigen Logik-App](./media/tutorial-process-mailing-list-subscriptions-workflow/tutorial-overview.png)

Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich <a href="https://azure.microsoft.com/free/" target="_blank">für ein kostenloses Azure-Konto registrieren</a>, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

* Ein MailChimp-Konto. Erstellen Sie eine Liste namens „test-members-ML“, der Ihre Logik-App E-Mail-Adressen für genehmigte Mitglieder hinzufügen kann. Falls Sie über kein Konto verfügen, [registrieren Sie sich für ein kostenloses Konto](https://login.mailchimp.com/signup/), und [informieren Sie sich, wie Sie eine Liste erstellen](https://us17.admin.mailchimp.com/lists/#). 

* Ein E-Mail-Konto bei Office 365 Outlook oder Outlook.com. (Beide unterstützen Genehmigungsworkflows.) In diesem Artikel wird Office 365 Outlook verwendet. Bei Verwendung eines anderen E-Mail-Kontos bleiben die allgemeinen Schritte zwar gleich, die Benutzeroberfläche sieht aber unter Umständen etwas anders aus.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich mit den Anmeldeinformationen Ihres Azure-Kontos beim <a href="https://portal.azure.com" target="_blank">Azure-Portal</a> an.

## <a name="create-your-logic-app"></a>Erstellen Ihrer Logik-App

1. Klicken Sie im Hauptmenü von Azure auf **Ressource erstellen** > **Unternehmensintegration** > **Logik-App**.

   ![Erstellen einer Logik-App](./media/tutorial-process-mailing-list-subscriptions-workflow/create-logic-app.png)

2. Geben Sie unter **Logik-App erstellen** die Informationen wie beschrieben ein. Wenn Sie fertig sind, wählen Sie **An Dashboard anheften** > **Erstellen** aus.

   ![Angeben von Informationen zur Logik-App](./media/tutorial-process-mailing-list-subscriptions-workflow/create-logic-app-settings.png)

   | Einstellung | Wert | BESCHREIBUNG | 
   | ------- | ----- | ----------- | 
   | **Name** | LA-MailingList | Der Name Ihrer Logik-App | 
   | **Abonnement** | <*Name Ihres Azure Abonnements*> | Der Name Ihres Azure-Abonnements | 
   | **Ressourcengruppe** | LA-MailingList-RG | Der Name der [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md), die zum Organisieren verwandter Ressourcen verwendet wird. | 
   | **Location** | USA (Ost) 2 | Die Region, in der die Informationen zu Ihrer Logik-App gespeichert werden sollen. | 
   | **Log Analytics** | Aus | Behalten Sie die Einstellung **Aus** für die Diagnoseprotokollierung bei. | 
   |||| 

3. Nachdem Ihre App von Azure bereitgestellt wurde, wird der Designer für Logik-Apps geöffnet und eine Seite mit einem Einführungsvideo und Vorlagen für allgemeine Logik-App-Mustern angezeigt. Wählen Sie unter **Vorlagen** die Option **Leere Logik-App**.

   ![Auswählen der Vorlage „Leere Logik-App“](./media/tutorial-process-mailing-list-subscriptions-workflow/choose-logic-app-template.png)

Fügen Sie als Nächstes einen [Trigger](../logic-apps/logic-apps-overview.md#logic-app-concepts) hinzu, der auf eingehende E-Mails mit Abonnementanforderungen lauscht.
Jede Logik-App beginnt mit einem Trigger, der ausgelöst wird, wenn ein bestimmtes Ereignis eintritt oder neue Daten eine bestimmte Bedingung erfüllen. Weitere Informationen finden Sie unter [Erstellen Ihrer ersten Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="add-trigger-to-monitor-emails"></a>Hinzufügen eines Triggers zum Überwachen von E-Mails

1. Geben Sie in das Suchfeld des Designers den Text „Wenn E-Mail empfangen wird“ ein. Wählen Sie den folgenden Trigger für Ihren E-Mail-Anbieter aus: **<*Ihr E-Mail-Anbieter*> - Wenn eine neue E-Mail empfangen wird**
   
   ![Wählen Sie den folgenden Trigger für den E-Mail-Anbieter aus: „When a new email arrives“ (Wenn eine neue E-Mail eingeht)](./media/tutorial-process-mailing-list-subscriptions-workflow/add-trigger-new-email.png)

   * Wählen Sie für Geschäfts-, Schul- oder Unikonten für Azure die Option „Office 365 Outlook“.
   * Wählen Sie für persönliche Microsoft-Konten die Option „Outlook.com“.

2. Falls Sie zur Eingabe von Anmeldeinformationen aufgefordert werden, melden Sie sich bei Ihrem E-Mail-Konto an, damit Logic Apps eine Verbindung mit Ihrem E-Mail-Konto erstellen kann.

3. Geben Sie nun die Kriterien an, die der Trigger in allen neuen E-Mails überprüfen soll.

   1. Geben Sie den Ordner, das Intervall und die Häufigkeit für die E-Mail-Überprüfung an.

      ![Angeben von Ordner, Intervall und Häufigkeit für die E-Mail-Überprüfung](./media/tutorial-process-mailing-list-subscriptions-workflow/add-trigger-set-up-email.png)

      | Einstellung | Wert | BESCHREIBUNG | 
      | ------- | ----- | ----------- | 
      | **Ordner** | Posteingang | Der zu überwachende E-Mail-Ordner. | 
      | **Intervall** | 1 | Die Anzahl von Warteintervallen zwischen Überprüfungen | 
      | **Frequency** | Hour | Die Zeiteinheit für die Intervalle zwischen Überprüfungen.  | 
      |  |  |  | 

   2. Klicken Sie auf **Erweiterte Optionen anzeigen**. Geben Sie im Feld **Filter für Betreff** den folgenden Text ein, auf den der Trigger den E-Mail-Betreff überprüfen soll: ```subscribe-test-members-ML```

      ![Festlegen erweiterter Optionen](./media/tutorial-process-mailing-list-subscriptions-workflow/add-trigger-set-advanced-options.png)

4. Klicken Sie auf die Titelleiste des Triggers, um die Triggerdetails vorerst auszublenden.

   ![Ausblenden der Details durch Reduzieren des Bereichs](./media/tutorial-process-mailing-list-subscriptions-workflow/collapse-trigger-shape.png)

5. Speichern Sie Ihre Logik-App. Wählen Sie auf der Symbolleiste des Designers **Speichern**.

   Ihre Logik-App befindet sich jetzt im Livemodus, überprüft bislang aber lediglich Ihre eingehenden E-Mails. 
   Fügen Sie daher eine Aktion hinzu, die reagiert, wenn der Trigger ausgelöst wird.

## <a name="send-approval-email"></a>Genehmigungs-E-Mail senden

Nachdem Sie nun über einen Trigger verfügen, fügen Sie als Nächstes eine [Aktion](../logic-apps/logic-apps-overview.md#logic-app-concepts) hinzu, die eine E-Mail zur Genehmigung oder Ablehnung der Anforderung sendet. 

1. Klicken Sie unter dem Trigger auf **+ Neuer Schritt** > **Aktion hinzufügen**. Suchen Sie nach „Genehmigung“, und wählen Sie die folgende Aktion aus: **<*Ihr E-Mail-Anbieter*> - Genehmigungs-E-Mail senden**

   ![Auswählen von „<Ihr-E-Mail-Anbieter> - Genehmigungs-E-Mail senden“](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-send-approval-email.png)

2. Geben Sie Informationen für diese Aktion wie hier dargestellt und beschrieben an: 

   ![Festlegen der Einstellungen für die Genehmigungs-E-Mail](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-approval-email-settings.png)

   | Einstellung | Wert | BESCHREIBUNG | 
   | ------- | ----- | ----------- | 
   | **To** | <*E-Mail-Adresse der genehmigenden Person*> | Die E-Mail-Adresse der genehmigenden Person. Zu Testzwecken können Sie hier Ihre eigene Adresse angeben. | 
   | **Benutzeroptionen** | Genehmigen, Ablehnen | Die Antwortoptionen, zwischen denen die genehmigende Person wählen kann. Standardmäßig kann die genehmigende Person zwischen „Genehmigen“ und „Ablehnen“ wählen. | 
   | **Subject** | Approve member request for test-members-ML | Ein aussagekräftiger E-Mail-Betreff. | 
   |  |  |  | 

   Ignorieren Sie vorerst die Liste mit den dynamischen Inhalten oder die Inlineparameterliste, die beim Klicken auf bestimmte Bearbeitungsfelder angezeigt wird. 
   In dieser Liste können Sie Parameter aus vorherigen Aktionen auswählen, die als Eingabe in Ihrem Workflow verwendet werden können. 
   Die Breite des Browserfensters bestimmt, welche Liste angezeigt wird. 
 
4. Speichern Sie Ihre Logik-App.

Fügen Sie als Nächstes eine Bedingung hinzu, die überprüft, welche Antwort die genehmigende Person gewählt hat.

## <a name="check-approval-response"></a>Überprüfen der Genehmigungsantwort

1. Klicken Sie unter der Aktion **Genehmigungs-E-Mail senden** auf **+ Neuer Schritt** > **Bedingung hinzufügen**.

   Daraufhin werden der Bedingungsbereich sowie alle verfügbaren Parameter angezeigt, die Sie als Eingabe in Ihren Workflow einbeziehen können. 

2. Versehen Sie die Bedingung mit einem aussagekräftigeren Namen.

   1. Klicken Sie auf der Titelleiste der Bedingung auf die **Schaltfläche mit den Auslassungspunkten** ( **...** ) und anschließend auf **Umbenennen**.

      Beispiel für die schmale Browseransicht:

      ![Umbenennen der Bedingung](./media/tutorial-process-mailing-list-subscriptions-workflow/condition-rename.png)

      Wenn Sie die breite Browseransicht verwenden und die Liste mit den dynamischen Inhalten die Schaltfläche mit den Auslassungspunkten verdeckt, klicken Sie innerhalb der Bedingung auf **Dynamischen Inhalt hinzufügen**, um die Liste zu schließen.

   2. Nennen Sie Ihre Bedingung wie folgt: ```If request approved```

3. Erstellen Sie eine Bedingung, die überprüft, ob die genehmigende Person **Genehmigen** ausgewählt hat:

   1. Klicken Sie in der Bedingung auf das Feld **Wert auswählen**. Dieses Feld befindet sich entweder links (breite Browseransicht) oder oben (schmale Browseransicht).
   Wählen Sie in der Parameterliste oder in der Liste mit den dynamischen Inhalten unter **Genehmigungs-E-Mail senden** das Feld **SelectedOption** aus.

      Bei Verwendung der breiten Ansicht sieht Ihre Bedingung wie im folgenden Beispiel aus:

      ![Auswählen von „SelectedOption“ unter „Genehmigung-E-Mail senden“](./media/tutorial-process-mailing-list-subscriptions-workflow/build-condition-check-approval-response.png)

   2. Wählen Sie im Vergleichsoperatorfeld den folgenden Operator aus: **ist gleich**

   3. Geben Sie rechts (breite Ansicht) oder unten (schmale Ansicht) im Feld **Wert auswählen** den folgenden Wert ein: ```Approve```

      Danach sieht Ihre Bedingung wie im folgenden Beispiel aus:

      ![Vollständige Bedingung](./media/tutorial-process-mailing-list-subscriptions-workflow/build-condition-check-approval-response-2.png)

4. Speichern Sie Ihre Logik-App.

Geben Sie als Nächstes die Aktion an, die Ihre Logik-App ausführen soll, wenn der Prüfer die Anforderung genehmigt. 

## <a name="add-member-to-mailchimp-list"></a>Hinzufügen eines Mitglieds zur MailChimp-Liste

Fügen Sie nun eine Aktion hinzu, die das genehmigte Mitglied Ihrer Adressenliste hinzufügt.

1. Klicken in der Bedingungsverzweigung **Bei TRUE** auf **Aktion hinzufügen**.
Suchen Sie nach „mailchimp“, und wählen Sie diese Aktion aus: **MailChimp - Mitglied zu Liste hinzufügen**

   ![Auswählen von „MailChimp - Mitglied zu Liste hinzufügen“](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-mailchimp-add-member.png)

3. Wenn Sie zur Anmeldung bei Ihrem MailChimp-Konto aufgefordert werden, melden Sie sich mit Ihren MailChimp-Anmeldeinformationen an.

4. Geben Sie Informationen für diese Aktion wie hier dargestellt und beschrieben an:

   ![Angeben der Informationen für „Mitglied zu Liste hinzufügen“](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-mailchimp-add-member-settings.png)

   | Einstellung | Wert | BESCHREIBUNG | 
   | ------- | ----- | ----------- | 
   | **Listen-ID** | test-members-ML | Der Name Ihrer MailChimp-Adressenliste. | 
   | **Status** | subscribed | Der Abonnementstatus für das neue Mitglied. Weitere Informationen finden Sie unter <a href="https://developer.mailchimp.com/documentation/mailchimp/guides/manage-subscribers-with-the-mailchimp-api/" target="_blank">Manage Subscribers with the MailChimp API</a> (Verwalten von Abonnenten mit der MailChimp-API). | 
   | **E-Mail-Adresse** | <*E-Mail-Adresse des neuen Mitglieds*> | Wählen Sie in der Parameterliste oder in der Liste mit den dynamischen Inhalten unter **Wenn eine neue E-Mail empfangen wird** das Feld **Von** aus. Dadurch wird die E-Mail-Adresse des neuen Mitglieds übergeben. 
   |  |  |  | 

5. Speichern Sie Ihre Logik-App.

Fügen Sie als Nächstes eine Bedingung hinzu, um zu prüfen, ob das neue Mitglied der Adressenliste hinzugefügt wurde. So erfahren Sie, ob der Vorgang erfolgreich war.

## <a name="check-for-success-or-failure"></a>Überprüfen, ob der Vorgang erfolgreich war

1. Klicken Sie in der Verzweigung **Bei TRUE** unter der Aktion **Mitglied zu Liste hinzufügen** auf **Mehr...**  > **Bedingung hinzufügen**.

2. Nennen Sie die Bedingung wie folgt: ```If add member succeeded```

3. Erstellen Sie eine Bedingung, die überprüft, ob das genehmigte Mitglied der Adressenliste hinzugefügt wurde:

   1. Klicken Sie in der Bedingung auf das Feld **Wert auswählen**. Dieses Feld befindet sich entweder links (breite Browseransicht) oder oben (schmale Browseransicht).
   Wählen Sie in der Parameterliste oder in der Liste mit den dynamischen Inhalten unter **Mitglied zu Liste hinzufügen** das Feld **Status** aus.

      Bei Verwendung der breiten Ansicht sieht Ihre Bedingung wie im folgenden Beispiel aus:

      ![Auswählen von „Status“ unter „Mitglied zu Liste hinzufügen“](./media/tutorial-process-mailing-list-subscriptions-workflow/build-condition-check-added-member.png)

   2. Wählen Sie im Vergleichsoperatorfeld den folgenden Operator aus: **ist gleich**

   3. Geben Sie rechts (breite Ansicht) oder unten (schmale Ansicht) im Feld **Wert auswählen** den folgenden Wert ein: ```subscribed```

   Danach sieht Ihre Bedingung wie im folgenden Beispiel aus:

   ![Fertige Bedingung](./media/tutorial-process-mailing-list-subscriptions-workflow/build-condition-check-added-member-2.png)

Richten Sie als Nächstes die E-Mails ein, die gesendet werden sollen, wenn das genehmigte Mitglied erfolgreich der Adressenliste hinzugefügt wurde oder wenn das Hinzufügen nicht erfolgreich war.

## <a name="send-email-if-member-added"></a>Senden einer E-Mail, wenn das Mitglied hinzugefügt wurde

1. Klicken Sie in der Verzweigung **Bei TRUE** für die Bedingung **If add member succeeded** auf **Aktion hinzufügen**.

   ![Klicken auf „Aktion hinzufügen“ in der Verzweigung „Bei TRUE“](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-email-success.png)

2. Suchen Sie nach „Outlook E-Mail senden“, und wählen Sie die folgende Aktion aus: **<*Ihr E-Mail-Anbieter*> - E-Mail senden**

   ![Hinzufügen einer Aktion für „E-Mail senden“](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-email-success-2.png)

3. Nennen Sie die Aktion wie folgt: ```Send email on success```

4. Geben Sie Informationen für diese Aktion wie hier dargestellt und beschrieben an:

   ![Angeben der Informationen für die Erfolgs-E-Mail](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-email-success-settings.png)

   | Einstellung | Wert | BESCHREIBUNG | 
   | ------- | ----- | ----------- | 
   | **To** | <*Ihre E-Mail-Adresse*> | Die E-Mail-Adresse, an die die Erfolgs-E-Mail gesendet werden soll. Zu Testzwecken können Sie hier Ihre eigene E-Mail-Adresse angeben. | 
   | **Subject** | <*Betreff der Erfolgs-E-Mail*> | Der Betreff der Erfolgs-E-Mail. Geben Sie für dieses Tutorial unter **Mitglied zu Liste hinzufügen** den folgenden Text ein, und wählen Sie in der Parameterliste oder in der Liste mit den dynamischen Inhalten das angegebene Feld aus: <p>Mitglied erfolgreich zu „test-members-ML“ hinzugefügt: **E-Mail-Adresse** | 
   | **Text** | <*Text für Erfolgs-E-Mail*> | Der Inhalt der Erfolgs-E-Mail. Geben Sie für dieses Tutorial unter **Mitglied zu Liste hinzufügen** den folgenden Text ein, und wählen Sie in der Parameterliste oder in der Liste mit den dynamischen Inhalten die angegebenen Felder aus:  <p>Ein neues Mitglied wurde in „test-members-ML“ aufgenommen: **E-Mail-Adresse**</br>Abonnementstatus des Mitglieds: **Status** | 
   | | | | 

5. Speichern Sie Ihre Logik-App.

## <a name="send-email-if-member-not-added"></a>Senden einer E-Mail, wenn das Mitglied nicht hinzugefügt wurde

1. Klicken Sie in der Verzweigung **Bei FALSE** für die Bedingung **If add member succeeded** auf **Aktion hinzufügen**.

   ![Klicken auf „Aktion hinzufügen“ in der Verzweigung „Bei FALSE“](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-email-failed.png)

2. Suchen Sie nach „Outlook E-Mail senden“, und wählen Sie die folgende Aktion aus: **<*Ihr E-Mail-Anbieter*> - E-Mail senden**

   ![Hinzufügen einer Aktion für „E-Mail senden“](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-email-failed-2.png)

3. Nennen Sie die Aktion wie folgt: ```Send email on failure```

4. Geben Sie Informationen für diese Aktion wie hier dargestellt und beschrieben an:

   ![Angeben der Informationen für die Fehler-E-Mail](./media/tutorial-process-mailing-list-subscriptions-workflow/add-action-email-failed-settings.png)

   | Einstellung | Wert | BESCHREIBUNG | 
   | ------- | ----- | ----------- | 
   | **To** | <*Ihre E-Mail-Adresse*> | Die E-Mail-Adresse, an die die Fehler-E-Mail gesendet werden soll. Zu Testzwecken können Sie hier Ihre eigene E-Mail-Adresse angeben. | 
   | **Subject** | <*Betreff für Fehler-E-Mail*> | Der Betreff der Fehler-E-Mail. Geben Sie für dieses Tutorial unter **Mitglied zu Liste hinzufügen** den folgenden Text ein, und wählen Sie in der Parameterliste oder in der Liste mit den dynamischen Inhalten das angegebene Feld aus: <p>Fehler: Mitglied wurde „test-members-ML“ nicht hinzugefügt: **E-Mail-Adresse** | 
   | **Text** | <*Text für Fehler-E-Mail*> | Der Inhalt der Fehler-E-Mail. Geben Sie für dieses Tutorial den folgenden Text ein: <p>Das Mitglied ist möglicherweise bereits vorhanden. Überprüfen Sie Ihr MailChimp-Konto. | 
   | | | | 

5. Speichern Sie Ihre Logik-App. 

Testen Sie als Nächstes Ihre Logik-App, die nun in etwa wie folgt aussieht:

 ![Fertiggestellte Logik-App](./media/tutorial-process-mailing-list-subscriptions-workflow/tutorial-complete.png)

## <a name="run-your-logic-app"></a>Ausführen Ihrer Logik-App

1. Senden Sie sich selbst eine E-Mail mit der Anforderung, in Ihre Adressenliste aufgenommen zu werden.
Warten Sie, bis die Anforderung in Ihrem Posteingang eingeht.

3. Wählen Sie in der Symbolleiste des Designers die Option **Ausführen**, um Ihre Logik-App manuell zu starten. 

   Wenn der Betreff Ihrer E-Mail dem Betrefffilter des Triggers entspricht, sendet Ihre Logik-App Ihnen eine E-Mail zur Genehmigung der Abonnementanforderung.

4. Wählen Sie in der Genehmigungs-E-Mail die Option **Genehmigen** aus.

5. Wenn die E-Mail-Adresse des Abonnenten in Ihrer Adressenliste nicht vorhanden ist, fügt Ihre Logik-App die entsprechende E-Mail-Adresse hinzu und sendet Ihnen eine E-Mail. Beispiel:

   ![Erfolgs-E-Mail](./media/tutorial-process-mailing-list-subscriptions-workflow/add-member-success.png)

   Falls Ihre Logik-App den Abonnenten nicht hinzufügen kann, erhalten Sie eine E-Mail wie die folgende:

   ![Fehler-E-Mail](./media/tutorial-process-mailing-list-subscriptions-workflow/add-member-failed.png)

   Sollten Sie keine E-Mails erhalten, überprüfen Sie Ihren Ordner für Junk-E-Mails. 
   E-Mails dieser Art werden unter Umständen durch Ihren Junk-E-Mail-Filter umgeleitet. 
   Wenn Sie unsicher sind, ob Ihre Logik-App richtig ausgeführt wurde, helfen Ihnen die Informationen unter [Diagnostizieren von Fehlern bei Logik-Apps](../logic-apps/logic-apps-diagnosing-failures.md) weiter.

Geschafft! Sie haben eine Logik-App erstellt und ausgeführt, die Informationen über Azure, Microsoft-Dienste und andere SaaS-Apps hinweg integriert.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie sie nicht mehr benötigen, löschen Sie die Ressourcengruppe mit Ihrer Logik-App und den dazugehörigen Ressourcen. Wechseln Sie im Azure-Hauptmenü zu **Ressourcengruppen**, und wählen Sie die Ressourcengruppe für Ihre Logik-App aus. Klicken Sie auf **Ressourcengruppe löschen**. Geben Sie zur Bestätigung den Ressourcengruppennamen ein, und klicken Sie auf **Löschen**.

![„Übersicht“ > „Ressourcengruppe löschen“](./media/tutorial-process-mailing-list-subscriptions-workflow/delete-resource-group.png)

## <a name="get-support"></a>Support

* Sollten Sie Fragen haben, besuchen Sie das [Azure Logic Apps-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Wenn Sie Features vorschlagen oder für Vorschläge abstimmen möchten, besuchen Sie die [Website für Logic Apps-Benutzerfeedback](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine Logik-App erstellt, die Genehmigungen für Adressenlistenanforderungen verwaltet. Erfahren Sie als Nächstes, wie Sie eine Logik-App erstellen, die E-Mail-Anlagen verarbeitet und speichert und Azure-Dienste wie Azure Storage und Azure Functions integriert.

> [!div class="nextstepaction"]
> [Verarbeiten von E-Mail-Anlagen](../logic-apps/tutorial-process-email-attachments-workflow.md)
