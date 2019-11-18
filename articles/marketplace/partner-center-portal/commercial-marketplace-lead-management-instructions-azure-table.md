---
title: Azure Table | Azure Marketplace
description: Konfigurieren der Leadverwaltung mit Azure-Tabellen
services: Azure, Marketplace, commercial marketplace, Partner Center
author: qianw211
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 07/30/2019
ms.author: evansma
ms.openlocfilehash: 9b24e6eb714c531b49ba08591bf4ed33d0f10101
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73812340"
---
# <a name="configure-lead-management-using-an-azure-table"></a>Konfigurieren der Leadverwaltung mit Azure-Tabellen

Wenn Ihr CRM-System (Customer Relationship Management) im Partner Center nicht explizit für den Empfang von Azure Marketplace- und AppSource-Leads unterstützt wird, können Sie zum Behandeln dieser Leads eine Azure-Tabelle verwenden. Sie können dann die Daten exportieren und in Ihr CRM-System importieren. Die Anweisungen in diesem Artikel führen Sie durch die Schritte zum Erstellen eines Azure Storage-Kontos und einer Azure-Tabelle unter diesem Konto. Außerdem können Sie mit Microsoft Flow einen neuen Flow erstellen, um eine E-Mail-Benachrichtigung zu senden, wenn Ihr Angebot einen Lead erhält.

## <a name="configure-azure-table"></a>Konfigurieren einer Azure-Tabelle

1. Wenn Sie nicht über ein Azure-Konto verfügen, können Sie ein [kostenloses Testkonto erstellen](https://azure.microsoft.com/pricing/free-trial/).
2. Nachdem Ihr Azure-Konto aktiviert wurde, melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
3. Gehen Sie folgendermaßen vor, um im Azure-Portal ein Speicherkonto zu erstellen.  
    1. Wählen Sie im Menü auf der linken Seite **+ Ressource erstellen** aus.  Auf der rechten Seite wird der Bereich (das Blatt) **Neu** angezeigt.
    2. Wählen Sie im Bereich **Neu** die Option **Speicher** aus.  Auf der rechten Seite wird eine Liste **Empfohlen** angezeigt.
    3. Wählen Sie **Speicherkonto** aus, um mit der Kontoerstellung zu beginnen.  Befolgen Sie die Anweisungen im Artikel [Erstellen eines Speicherkontos](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal).

        ![Schritte zum Erstellen eines Azure Storage-Kontos](./media/commercial-marketplace-lead-management-instructions-azure-table/azure-storage-create.png)

        Weitere Informationen zu Speicherkonten finden Sie im [Schnellstarttutorial](https://docs.microsoft.com/azure/storage/).  Weitere Informationen zu den Preisen für Storage finden Sie unter [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

4. Warten Sie, bis Ihr Speicherkonto bereitgestellt wird. Dieser Prozess nimmt in der Regel einige Minuten in Anspruch.  Greifen Sie über die **Startseite** des Azure-Portals auf Ihr Speicherkonto zu, indem Sie **All Ihre Ressourcen anzeigen** oder im linken Navigationsbereich des Azure-Portals **Alle Ressourcen** auswählen.

    ![Zugriff auf Ihr Azure-Speicherkonto](./media/commercial-marketplace-lead-management-instructions-azure-table/azure-storage-access.png)

5. Wählen Sie im Speicherkontobereich **Zugriffsschlüssel** aus, und kopieren Sie den Wert von *Verbindungszeichenfolge* für den Schlüssel. Speichern Sie diesen Wert, da dies der Wert von *Verbindungszeichenfolge für Speicherkonto* ist, den Sie im Veröffentlichungsportal angeben müssen, um Leads für Ihr Marketplace-Angebot zu erhalten. 

    Beispiel für eine Verbindungszeichenfolge:

    ```sql
    DefaultEndpointsProtocol=https;AccountName=myAccountName;AccountKey=myAccountKey;EndpointSuffix=core.windows.net
    ```

    ![Azure-Speicherschlüssel](./media/commercial-marketplace-lead-management-instructions-azure-table/azure-storage-keys.png)

6. Wählen Sie im Speicherkontobereich **Tabellen** und dann **+ Tabelle** aus, um eine Tabelle zu erstellen. Geben Sie einen Namen für die Tabelle ein, und wählen Sie **OK** aus. Speichern Sie diesen Wert, da Sie ihn benötigen, wenn Sie einen MS Flow für den Empfang von E-Mail-Benachrichtigungen beim Erhalt von Leads konfigurieren möchten.

    ![Azure-Tabellen](./media/commercial-marketplace-lead-management-instructions-azure-table/azure-tables.png)

    Sie können die Daten in Ihrer Speichertabelle im [Azure Storage-Explorer](https://archive.codeplex.com/?p=azurestorageexplorer) oder einem anderen Tool anzeigen. Sie können die Daten auch in die Azure-Tabelle exportieren. 

## <a name="optional-use-microsoft-flow-with-an-azure-table"></a>(Optional) Verwenden von Microsoft Flow mit einer Azure-Tabelle  

Sie können [Microsoft Flow](https://docs.microsoft.com/flow/) verwenden, um automatisch jedes Mal, wenn ein Lead zur Azure-Tabelle hinzugefügt wird, eine Benachrichtigung zu senden. Wenn Sie kein Konto besitzen, können Sie sich [für ein kostenloses Konto registrieren](https://flow.microsoft.com/).

### <a name="lead-notification-example"></a>Beispiel zu Leadbenachrichtigung

Verwenden Sie dieses Beispiel als Leitfaden, um einen einfachen Flow zu erstellen, in dem automatisch eine E-Mail-Benachrichtigung gesendet wird, wenn ein neuer Lead zu einer Azure-Tabelle hinzugefügt wird. In diesem Beispiel wird eine Wiederholung eingerichtet, um stündlich Leadinformationen zu senden, wenn der Tabellenspeicher aktualisiert wird.

1. Melden Sie sich bei Ihrem Microsoft Flow-Konto an.
2. Wählen Sie in der linken Navigationsleiste die Option **Meine Flows** aus.
3. Wählen Sie in der oberen Navigationsleiste die Option **+ Neu** aus.  
4. Wählen Sie in der Dropdownliste den Eintrag **+ Scheduled - from blank** (+ Geplant – Ohne Vorlage) aus.

   ![Meine Flows **+ Scheduled - from blank** (+ Geplant – Ohne Vorlage)](./media/commercial-marketplace-lead-management-instructions-azure-table/ms-flow-scheduled-from-blank.png)

5.  Wählen Sie im Fenster *Geplanten Flow erstellen* unter *Wiederholen alle* für das Intervall „1“ und für die Häufigkeit „Stunde“ aus. Wenn Sie möchten, können Sie den Flow benennen. Klicken Sie auf **Erstellen**.

    >[!Note]
    >Obwohl in diesem Beispiel ein 1-Stunden-Intervall verwendet wird, können Sie das Intervall und die Häufigkeit auswählen, die für Ihre Geschäftsanforderungen am besten geeignet sind.

    ![Geplanten Flow erstellen](./media/commercial-marketplace-lead-management-instructions-azure-table/build-scheduled-flow.png)

6. Wählen Sie **+ Neuer Schritt**aus.
7. Suchen Sie im Fenster *Aktion auswählen*  nach „Vergangene Zeit abrufen“, und wählen Sie dann unter „Aktionen“ **Vergangene Zeit abrufen** aus.

   ![Aktion auswählen](./media/commercial-marketplace-lead-management-instructions-azure-table/choose-an-action.png)

8. Legen Sie im Fenster **Vergangene Zeit abrufen** das **Intervall** auf „1“ fest. Wählen Sie in der Dropdownliste **Zeiteinheit** die Einheit **Stunde** aus.

    >[!Important]
    >Stellen Sie sicher, dass dieses Intervall und diese Zeiteinheit mit dem Intervall und der Häufigkeit übereinstimmen, die Sie in Schritt 5 für „Wiederholung“ konfiguriert haben.

    ![Intervall für „Vergangene Zeit abrufen“ festlegen](./media/commercial-marketplace-lead-management-instructions-azure-table/ms-flow-getpast-time.png)

>[!TIP] 
>Sie können Ihren Flow jederzeit überprüfen, um sich zu vergewissern, dass jeder Schritt ordnungsgemäß konfiguriert ist. Um Ihren Flow zu überprüfen, wählen Sie **Flowprüfung** in der Menüleiste „Flow“ aus.

In den nächsten Schritten stellen Sie eine Verbindung mit Ihrer Azure-Tabelle her und richten die Verarbeitungslogik ein, gemäß der neue Leads verarbeitet werden sollen.

9. Wählen Sie nach dem Schritt „Vergangene Zeit abrufen“ die Option **+ Neuer Schritt** aus, und suchen Sie dann im Fenster *Aktion auswählen* nach „Entitäten abrufen“.
10. Wählen Sie unter **Aktionen** die Option **Get entities (Azure Table Storage)** (Entitäten abrufen (Azure-Tabellenspeicher)) aus.
11. Geben Sie im Fenster **Azure-Tabellenspeicher** Informationen für die folgenden Felder an, und wählen Sie **Erstellen** aus:

    * *Verbindungsname*: Geben Sie einen aussagekräftigen Namen für die Verbindung ein, die Sie zwischen diesem Flow und der Azure-Tabelle einrichten.
    * *Speicherkontoname*: Geben Sie den Namen des Speicherkontos für die Azure-Tabelle ein. Diesen finden Sie auf der Seite **Zugriffsschlüssel** des Speicherkontos.
    * *Freigegebener Speicherschlüssel*: Geben Sie den Schlüsselwert des Speicherkontos für die Azure-Tabelle an. Diesen finden Sie auf der Seite **Zugriffsschlüssel** des Speicherkontos.

        ![Azure Table Storage.](./media/commercial-marketplace-lead-management-instructions-azure-table/azure-table-storage.png)

    Nachdem Sie auf „Erstellen“ geklickt haben, wird das Fenster *Entitäten abrufen* angezeigt. Wählen Sie **Erweiterte Optionen anzeigen** aus, und geben Sie Informationen für die folgenden Felder an:

       * *Tabelle*: Wählen Sie den Namen des Azure-Tabellenspeichers aus (aus Schritt 6 der Anleitung zum Konfigurieren einer Azure-Tabelle). Der nächste Screenshot zeigt die Eingabeaufforderung, wenn für dieses Beispiel die Tabelle „marketplaceleads“ ausgewählt ist.

            ![Azure-Tabelle – Entitäten abrufen](./media/commercial-marketplace-lead-management-instructions-azure-table/azure-table-get-entities.png)

        * *Filterabfrage*: Wählen Sie dieses Feld aus, und fügen Sie diese Funktion in das Feld ein: `Timestamp gt datetime'@{body('Get_past_time')}'`

            ![Azure-Tabelle – Entitäten abrufen – Filterabfrage](./media/commercial-marketplace-lead-management-instructions-azure-table/azure-table-get-entities-filter-query.png)

12. Nachdem Sie die Verbindung mit der Azure-Tabelle eingerichtet haben, wählen Sie **Neuer Schritt** aus, um eine Bedingung für das Durchsuchen der Azure-Tabelle nach neuen Leads hinzuzufügen. 

13. Wählen Sie im Fenster **Aktion auswählen** die Option **Aktionen** aus, und wählen Sie dann das Steuerelement **Bedingung** aus.

    ![Azure-Tabelle – Aktion auswählen](./media/commercial-marketplace-lead-management-instructions-azure-table/azure-table-choose-an-action.png)

14. Wählen Sie im Fenster **Bedingung** das Feld **Wert auswählen** aus, und wählen Sie dann **Ausdruck** im Popupenster aus.

15. Fügen Sie `length(body('Get_entities')?['value'])` in das Feld ***fx*** ein. Wählen Sie **OK** aus, um diese Funktion hinzuzufügen. 

16. So schließen Sie das Einrichten der Bedingung ab:
    1. Wählen Sie „ist größer als“ in der Dropdownliste aus.
    2. Geben Sie „0“ als Wert ein.

        ![Azure-Tabelle – Bedingung](./media/commercial-marketplace-lead-management-instructions-azure-table/azure-table-condition.png)

In den nächsten Schritten richten Sie die Aktion ein, die entsprechend dem Ergebnis der Bedingung ausgeführt werden soll.

* Hat die Bedingung das Ergebnis **Wenn nein**, soll keine Aktion ausgeführt werden.
* Hat die Bedingung das Ergebnis **Wenn ja**, lösen Sie eine Aktion aus, in der eine E-Mail an Ihr Office 365-Konto gesendet wird. 

17. Wählen Sie unter **Wenn ja** die Option **Aktion hinzufügen** aus.

    ![Azure-Tabelle – Bedingung, **Wenn ja**](./media/commercial-marketplace-lead-management-instructions-azure-table/azure-table-condition-if-yes.png)

18. Wählen Sie **E-Mail senden (Office 365 Outlook)** aus.

    ![Azure-Tabelle – Bedingung, **Wenn ja**, E-Mail senden](./media/commercial-marketplace-lead-management-instructions-azure-table/azure-table-condition-if-yes-send-email.png)

    >[!Note]
    >Wenn Sie einen anderen E-Mail-Anbieter verwenden möchten, suchen Sie stattdessen „E-Mail-Benachrichtigung senden (E-Mail)“, und wählen Sie diese Aktion aus. In den Anweisungen wird erläutert, wie Sie die Konfiguration mit Office 365 Outlook durchführen, die Anweisungen gelten jedoch auch für andere E-Mail-Anbieter.

19. Geben Sie im Fenster **Office 365 Outlook** Informationen in die folgenden Felder ein:

    1. **An**: Geben Sie eine E-Mail-Adresse für jede Person ein, die diese Benachrichtigung erhalten soll.
    1. **Betreff**: Geben Sie einen Betreff für die E-Mail ein. Beispiel:  Neue Leads!
    1. **Nachrichtentext**: Fügen Sie den Text hinzu, der in jede E-Mail einbezogen werden soll (optional), und fügen Sie dann den Text `body('Get_entities')?['value']` ein.

    >[!Note]
    >Sie können weitere statische oder dynamische-Datenpunkte in den Text dieser E-Mail einfügen.

    ![Azure-Tabelle – Bedingung, **Wenn ja**, Fenster „Office 365 Outlook“](./media/commercial-marketplace-lead-management-instructions-azure-table/azure-table-condition-if-yes-outlook.png)

20. Wählen Sie **Speichern** aus, um den Flow zu speichern. Microsoft Flow testet den Flow automatisch auf Fehler. Sind keine Fehler vorhanden, wird Ihr Flow gestartet, nachdem er gespeichert wurde.

Die nächste Bildschirmaufnahme zeigt ein Beispiel, wie der fertige Flow aussehen soll.

![Ein Beispiel für den abschließenden Flow](./media/commercial-marketplace-lead-management-instructions-azure-table/final-flow.png)

### <a name="manage-your-flow"></a>Verwalten Ihres Flows

Das Verwalten Ihres Flows nach dessen Start ist einfach. Sie haben vollständige Kontrolle über den Flow. Sie können ihn z. B. beenden und bearbeiten, und Sie können einen Ausführungsverlauf anzeigen und Analysen abrufen. Die nächste Bildschirmaufnahme zeigt die Optionen, die zum Verwalten eines Flows verfügbar sind. 

 ![Verwalten eines Flows](./media/commercial-marketplace-lead-management-instructions-azure-table/ms-flow-manage-completed.png)

Der Flow wird solange ausgeführt, bis Sie ihn mit der Option **Flow deaktivieren** beendet haben.

Erhalten Sie keine E-Mail-Benachrichtigungen zu Leads, wurden der Azure-Tabelle keine neuen Leads hinzugefügt. Sind Flowfehler vorhanden, erhalten Sie eine E-Mail, die wie das Beispiel im nächsten Screenshot aussieht.

 ![E-Mail-Benachrichtigung zu Flowfehler](./media/commercial-marketplace-lead-management-instructions-azure-table/ms-flow-failure-note.png)

## <a name="configure-your-offer-to-send-leads-to-the-azure-table"></a>Konfigurieren des Angebots, sodass Leads an die Azure-Tabelle gesendet werden

Führen Sie die folgenden Schritte aus, um die Leadverwaltungsinformationen für Ihr Angebot im Veröffentlichungsportal zu konfigurieren:

1. Navigieren Sie zur Seite **Angebotseinrichtung** für Ihr Angebot.
2. Wählen Sie im Abschnitt „Leadverwaltung“ die Option **Verbinden** aus.
3. Wählen Sie im Fenster „Verbindungsdetails“ **Azure-Tabelle** als **Leadzielgruppe** aus, und fügen Sie in das Feld **Verbindungszeichenfolge für Speicherkonto** die Verbindungszeichenfolge aus dem Azure Storage-Konto ein, die Sie in den vorherigen Schritten erstellt haben.
4. Wählen Sie **Speichern** aus. 

>[!Note]
>Sie müssen die Konfiguration der übrigen Einstellungen des Angebots abschließen und veröffentlichen, damit Sie Leads für das Angebot erhalten.

Wenn Leads generiert werden, werden sie von Microsoft an die Azure-Tabelle gesendet. Wenn Sie einen Flow konfiguriert haben, wird auch eine E-Mail an die von Ihnen konfigurierte E-Mail-Adresse gesendet.

![Leadverwaltung](./media/commercial-marketplace-lead-management-instructions-azure-table/lead-management.png)

![Leadverwaltung – Verbindungsdetails](./media/commercial-marketplace-lead-management-instructions-azure-table/connection-details.png)

![Lead Verwaltung – Verbindungsdetails für Speicherkonto](./media/commercial-marketplace-lead-management-instructions-azure-table/azure-table-connection-details.png)

