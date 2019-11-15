---
title: HTTPS-Endpunkt | Azure Marketplace
description: Konfigurieren Sie die Leadverwaltung für einen HTTPS-Endpunkt.
services: Azure, Marketplace, commercial marketplace, Partner Center
author: qianw211
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 07/31/2019
ms.author: evansma
ms.openlocfilehash: 83efb9cfd1ee7464a334ebc4064dbfaa20ab30de
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73812286"
---
# <a name="configure-lead-management-using-an-https-endpoint"></a>Konfigurieren der Leadverwaltung zur Verwendung eines HTTPS-Endpunkts

Wenn Ihr CRM-System (Customer Relationship Management) im Partner Center nicht explizit für den Empfang von Azure Marketplace- und AppSource-Leads unterstützt wird, können Sie zum Behandeln dieser Leads einen HTTPS-Endpunkt in Microsoft Flow verwenden. Mit einem HTTPS-Endpunkt können diese Leads per E-Mail-Benachrichtigung versendet oder in ein von Microsoft Flow unterstütztes CRM-System (Customer Relationship Management) geschrieben werden. Die Anweisungen in diesem Artikel führen Sie schrittweise durch den grundlegenden Prozess des Erstellens eines neuen Flows mithilfe von Microsoft Flow. Dabei wird die HTTP POST-URL erstellt, die Sie im Veröffentlichungsportal für das Feld „Leadverwaltung“ > **HTTPS-Endpunkt-URL** eingeben. Außerdem finden Sie hier Anweisungen dazu, wie Sie Ihren Flow mithilfe des Tools [Postman](https://www.getpostman.com/downloads/) testen können, das online zugänglich ist.

## <a name="create-a-flow-using-microsoft-flow"></a>Erstellen eines Flows mit Microsoft Flow

1. Öffnen Sie die [Flow](https://flow.microsoft.com/)-Webseite. Wählen Sie **Anmelden** aus, oder – falls Sie noch kein Konto besitzen – wählen Sie **Kostenlos registrieren** aus, um ein kostenloses Flow-Konto zu erstellen.

2. Melden Sie sich an, und wählen Sie **Meine Flows** in der Menüleiste aus.

3. Wählen Sie **+Automatisiert – ohne Vorlage** aus.

    ![Meine Flows und +Automatisiert – ohne Vorlage](./media/commercial-marketplace-lead-management-instructions-https/my-flows-automated.png)

4. Wählen Sie im Fenster *Automatisierten Flow erstellen* die Option **Überspringen** aus. 

    ![Automatisierten Flow erstellen – Überspringen](./media/commercial-marketplace-lead-management-instructions-https/build-automated-flow.png)

5. Geben Sie im Feld **Connectors und Trigger durchsuchen** den Suchbegriff „Anforderung“ ein, um den Anforderungsconnector zu finden.
6. Wählen Sie unter *Trigger* die Option **Beim Empfang einer HTTP-Anforderung** aus. 

    ![Anforderungsconnector – Trigger](./media/commercial-marketplace-lead-management-instructions-https/request-connector.png)

7. Kopieren Sie im Fenster *Beim Empfang einer HTTP-Anforderung* das JSON-Schema, und fügen Sie es unten im Textfeld **JSON-Schema für Anforderungstext** ein. In diesem Schema werden in Microsoft die Leaddaten eingeschlossen.

    ![Anforderungsconnector – Trigger](./media/commercial-marketplace-lead-management-instructions-https/https-request-received.png)

    **JSON-Schema**

    ```JSON
    {
      "$schema": "https://json-schema.org/draft-04/schema#",
      "definitions": {},
      "id": "http://example.com/example.json",
      "properties": {
        "ActionCode": {
          "id": "/properties/ActionCode",
          "type": "string"
        },
        "OfferTitle": {
          "id": "/properties/OfferTitle",
          "type": "string"
        },
        "LeadSource": {
          "id": "/properties/LeadSource",
          "type": "string"
        },
        "UserDetails": {
          "id": "/properties/UserDetails",
          "properties": {
            "Company": {
              "id": "/properties/UserDetails/properties/Company",
              "type": "string"
            },
            "Country": {
              "id": "/properties/UserDetails/properties/Country",
              "type": "string"
            },
            "Email": {
              "id": "/properties/UserDetails/properties/Email",
              "type": "string"
            },
            "FirstName": {
              "id": "/properties/UserDetails/properties/FirstName",
              "type": "string"
            },
            "LastName": {
              "id": "/properties/UserDetails/properties/LastName",
              "type": "string"
            },
            "Phone": {
              "id": "/properties/UserDetails/properties/Phone",
              "type": "string"
            },
            "Title": {
              "id": "/properties/UserDetails/properties/Title",
              "type": "string"
            }
          },
          "type": "object"
        }
      },
      "type": "object"
    }
    ```

>[!Note]
>An diesem Punkt in der Konfiguration können Sie auswählen, ob Sie eine Verbindung mit einem CRM-System herstellen oder eine E-Mail-Benachrichtigung konfigurieren. Befolgen Sie die übrigen Anweisungen entsprechend Ihrer Entscheidung.

### <a name="to-connect-to-a-crm-system"></a>So stellen Sie eine Verbindung mit einem CRM-System her

1. Wählen Sie **+ Neuer Schritt**aus.
2. Wählen Sie das gewünschte CRM-System aus. Suchen Sie unter *Connectors und Aktionen durchsuchen* danach, und wählen Sie es im Abschnitt *Aktionen* mit der Option zum Erstellen eines neuen Datensatzes aus. Auf dem folgenden Screenshot wird beispielsweise **Dynamics 365 – Neuen Datensatz erstellen** verwendet.

    ![Create a new record (Neuen Datensatz erstellen)](./media/commercial-marketplace-lead-management-instructions-https/create-new-record.png)

3. Geben Sie den **Organisationsnamen** für das CRM-System an. Wählen Sie in der Dropdownliste **Entitätsname** die Option **Leads** aus.

    ![Leads auswählen](./media/commercial-marketplace-lead-management-instructions-https/select-leads.png)

4. Flow zeigt ein Formular für die Eingabe von Leadinformationen an. Sie können Elemente aus der Eingabeanforderung zuordnen, indem Sie auswählen, dass dynamischer Inhalt hinzugefügt werden soll. Im folgenden Screenshot wird beispielsweise **OfferTitle** verwendet.

    ![Dynamischen Inhalt hinzufügen](./media/commercial-marketplace-lead-management-instructions-https/add-dynamic-content.png)

5. Ordnen Sie die gewünschten Felder zu, und wählen Sie dann **Speichern** aus, um den Flow zu speichern. Eine HTTP-POST-URL wird erstellt. Diese ist im Fenster *Beim Empfang einer HTTP-Anforderung* zugänglich. Kopieren Sie diese URL mithilfe des Steuerelements zum Kopieren, das sich rechts neben der HTTP-POST-URL befindet – dies ist wichtig, damit die URL vollständig ist und Sie keinen Teil davon übersehen. Speichern Sie diese URL, da Sie sie beim Konfigurieren der Leadverwaltung im Veröffentlichungsportal benötigen.

    ![Beim Empfang einer HTTP-Anforderung](./media/commercial-marketplace-lead-management-instructions-https/when-http-request-received.png)

### <a name="to-set-up-email-notification"></a>So richten Sie E-Mail-Benachrichtigungen ein

1. Wählen Sie nach dem Abschließen des JSON-Schemas die Option **+ Neuer Schritt** aus.
2. Wählen Sie unter **Aktion auswählen** die Option **Aktionen** aus.
3. Wählen Sie unter **Aktionen** die Option **E-Mail senden (Office 365 Outlook)** aus.

    >[!Note]
    >Wenn Sie einen anderen E-Mail-Anbieter verwenden möchten, suchen Sie stattdessen *E-Mail-Benachrichtigung senden (E-Mail)* , und wählen Sie diese Aktion aus.

    ![Hinzufügen einer E-Mail-Aktion](./media/commercial-marketplace-lead-management-instructions-https/https-request-received-send-email.png)

4. Konfigurieren Sie im Fenster **E-Mail senden** die folgenden Pflichtfelder:

   - **An**: Geben Sie mindestens eine gültige E-Mail-Adresse ein, an welche die Leads gesendet werden.
   - **Betreff**: Flow ermöglicht das Hinzufügen dynamischer Inhalte (wie etwa **LeadSource** im folgenden Screenshot). Geben Sie zunächst einen Feldnamen ein, und klicken Sie anschließend im Popupfenster auf die Auswahlliste „Dynamischer Inhalt“. 

        >[!Note] 
        > Beim Hinzufügen von Feldnamen können Sie nach jedem einen Doppelpunkt (:) eingeben und dann EINGABE drücken, um eine neue Zeile zu erstellen. Nachdem Sie die Feldnamen hinzugefügt haben, können Sie die einzelnen zugeordneten Parameter aus der dynamischen Auswahlliste hinzufügen.

        ![Hinzufügen einer E-Mail-Aktion mit dynamischem Inhalt](./media/commercial-marketplace-lead-management-instructions-https/add-email-using-dynamic-content.png)

   - **Text**: Fügen Sie über die Auswahlliste „Dynamischer Inhalt“ die gewünschten Informationen in den E-Mail-Text ein. Verwenden Sie beispielsweise „LastName“ (Nachname), „FirstName“ (Vorname), „Email“ (E-Mail-Adresse) und „Company“ (Unternehmen). <br> <br> Ein Beispiel für eine fertig eingerichtete E-Mail-Benachrichtigung sehen Sie auf dem folgenden Screenshot.


       ![Hinzufügen einer E-Mail-Aktion](./media/commercial-marketplace-lead-management-instructions-https/send-an-email.png)

5. Wählen Sie **Speichern** aus, um den Flow fertigzustellen. Eine HTTP-POST-URL wird erstellt. Diese ist im Fenster *Beim Empfang einer HTTP-Anforderung* zugänglich. Kopieren Sie diese URL mithilfe des Steuerelements zum Kopieren, das sich rechts neben der HTTP-POST-URL befindet – dies ist wichtig, damit die URL vollständig ist und Sie keinen Teil davon übersehen. Speichern Sie diese URL, da Sie sie beim Konfigurieren der Leadverwaltung im Veröffentlichungsportal benötigen.

   ![HTTP-POST-URL ](./media/commercial-marketplace-lead-management-instructions-https/http-post-url.png)

### <a name="testing"></a>Testen

Durch Ausführen der folgenden Schritte können Sie mithilfe des Tools [Postman](https://app.getpostman.com/app/download/win64) die ordnungsgemäße Funktionsweise testen. Dieses Tool kann online heruntergeladen werden. Es ist für Windows verfügbar. 

1. Starten Sie Postman, und wählen Sie **New** > **Request** (Neu > Anforderung) aus, um das Testtool einzurichten. 

   ![Anforderung zum Einrichten des Testtools](./media/commercial-marketplace-lead-management-instructions-https/postman-request.png)

2. Füllen Sie das Formular *Save Request* (Anforderung speichern) aus, und wählen Sie dann **Save** (Speichern) aus, um sie im erstellten Ordner zu speichern.

   ![Save Request (Anforderung speichern)](./media/commercial-marketplace-lead-management-instructions-https/postman-save-to-test.png)

3. Wählen Sie in der Dropdownliste den Eintrag **POST** aus. 

   ![Meinen Flow testen](./media/commercial-marketplace-lead-management-instructions-https/test-my-flow.png)

4. Fügen Sie bei der Aufforderung *Anforderungs-URL eingeben* die HTTP-POST-URL aus dem Flow ein, den Sie in Microsoft Flow erstellt haben.

   ![HTTP-POST-URL einfügen](./media/commercial-marketplace-lead-management-instructions-https/paste-http-post-url.png)

5. Wechseln Sie zurück zu [Flow](https://flow.microsoft.com/), und suchen Sie nach dem erstellten Flow zum Senden von Leads, indem Sie auf der Flow-Menüleiste **Meine Flows** auswählen.  Wählen Sie die drei Punkte neben dem Flownamen aus, und wählen Sie **Bearbeiten** aus.

   ![Meine Flows – Bearbeiten](./media/commercial-marketplace-lead-management-instructions-https/my-flows-edit.png)

6. Wählen Sie in der oberen rechten Ecke **Testen** aus. Wählen Sie „Ich führe die Triggeraktion durch“ und anschließend **Testen** aus. Am oberen Bildschirmrand wird ein Hinweis angezeigt, der besagt, dass der Test gestartet wurde.

   ![Flow testen – Trigger](./media/commercial-marketplace-lead-management-instructions-https/test-flow-trigger-action.png)

7. Wechseln Sie zurück zur Postman-App, und wählen Sie neben dem Feld, in dem Sie die HTTPS-URL eingefügt haben, **Send** (Senden)aus.

   ![Meinen Flow testen – Senden](./media/commercial-marketplace-lead-management-instructions-https/postman-send.png)

8. Wechseln Sie zurück zu Ihrem Flow, und überprüfen Sie das Ergebnis. Wenn alles wie erwartet funktioniert, wird eine Meldung mit dem Hinweis angezeigt, dass der Vorgang erfolgreich abgeschlossen wurde.

   ![Flow – Ergebnisse überprüfen](./media/commercial-marketplace-lead-management-instructions-https/my-flow-check-results.png)

9. Außerdem sollten Sie eine E-Mail erhalten haben. Überprüfen Sie Ihren Posteingang. 

    >[!Note] 
    >Wenn keine E-Mail aus dem Test angezeigt wird, überprüfen Sie Ihren Spamordner. Unten sehen Sie nur die Feldbezeichnungen, die Sie beim Konfigurieren der Benachrichtigung hinzugefügt haben. Wäre dies ein tatsächlicher, von Ihrem Angebot generierter Lead, würden Sie zudem im Text und in der Betreffzeile die tatsächlichen Informationen aus dem Leadkontakt sehen.

   ![Empfangene E-Mail](./media/commercial-marketplace-lead-management-instructions-https/email-received.png)

## <a name="configure-your-offer-to-send-leads-to-the-https-endpoint"></a>Konfigurieren Sie Ihr Angebot so, dass Leads an den HTTPS-Endpunkt gesendet werden

Führen Sie die folgenden Schritte aus, um die Leadverwaltungsinformationen für Ihr Angebot im Veröffentlichungsportal zu konfigurieren:

1. Navigieren Sie zur Seite **Angebotseinrichtung** für Ihr Angebot.
2. Wählen Sie im Abschnitt „Leadverwaltung“ die Option **Verbinden** aus.
3. Wählen Sie im Popupfenster „Verbindungsdetails“ **HTTPS-Endpunkt** für **Leadziel** aus, und fügen Sie die HTTP-POST-URL aus dem zuvor erstellten Flow im Feld **HTTPS-Endpunkt-URL** ein.
4. Wählen Sie **Speichern** aus. 

>[!Note] 
>Sie müssen die Konfiguration der übrigen Einstellungen des Angebots abschließen und veröffentlichen, damit Sie Leads für das Angebot erhalten.

Generierte Leads werden von Microsoft an den Flow gesendet und anschließend an das konfigurierte CRM-System oder an die konfigurierte E-Mail-Adresse weitergeleitet.

![Leadverwaltung – Verbinden](./media/commercial-marketplace-lead-management-instructions-https/lead-management-connect.png)

![Verbindungsdetails](./media/commercial-marketplace-lead-management-instructions-https/connection-details.png)

![Verbindungsdetails](./media/commercial-marketplace-lead-management-instructions-https/connection-details-1.png)

