---
title: Azure-Tabellenspeicher | Azure Marketplace
description: Hier erfahren Sie, wie Sie die Leadverwaltung in Azure-Tabellenspeicher konfigurieren.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 05/22/2019
ms.author: pabutler
ms.openlocfilehash: 21105d72ccd288faf0fed58019e67afe2e1c9d01
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73825269"
---
# <a name="lead-management-instructions-for-table-storage"></a>Leadverwaltungsanleitung für Tabellenspeicher

In diesem Artikel erfahren Sie, wie Sie Azure-Tabellenspeicher zum Verwalten von Vertriebsleads konfigurieren. Der Tabellenspeicher unterstützt Sie beim Speichern und Ändern von Kundeninformationen.

## <a name="configure-table-storage"></a>Konfigurieren des Tabellenspeichers

1. Sollten Sie über kein Azure-Konto verfügen, [erstellen Sie ein kostenloses Testkonto](https://azure.microsoft.com/pricing/free-trial/).
1. Melden Sie sich nach der Aktivierung Ihres Kontos beim [Azure-Portal](https://portal.azure.com) an.
1. Gehen Sie im Azure-Portal wie folgt vor:  
    1. Wählen Sie im Bereich auf der linken Seite die Option **+Ressource erstellen** aus. Daraufhin wird der Bereich **Neu** geöffnet.
    1. Wählen Sie im Bereich **Neu** die Option **Speicher** aus. Daraufhin wird auf der rechten Seite die Liste **Ausgewählte** geöffnet.
    1. Wählen Sie **Speicherkonto** aus. Gehen Sie gemäß der Anleitung unter [Speicherkonto erstellen](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal) vor.

    ![Erstellen eines Azure-Speicherkontos](./media/cloud-partner-portal-lead-management-instructions-azure-table/azurestoragecreate.png)

    Weitere Informationen zu Speicherkonten finden Sie in den [Schnellstarttutorials](https://docs.microsoft.com/azure/storage/). Preisinformationen finden Sie in der [Übersicht über die Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

1. Warten Sie, bis Ihr Speicherkonto bereitgestellt wurde. Dies kann einige Minuten dauern. Greifen Sie dann von der Startseite des Azure-Portals aus auf das Konto zu: Wählen Sie **All Ihre Ressourcen anzeigen** oder im Navigationsbereich die Option **Alle Ressourcen** aus.

    ![Zugriff auf Ihr Azure-Speicherkonto](./media/cloud-partner-portal-lead-management-instructions-azure-table/azure-storage-access.png)

1. Kopieren Sie im Bereich Ihres Speicherkontos die Speicherkonto-Verbindungszeichenfolge für den Schlüssel. Fügen Sie sie in das Feld **Verbindungszeichenfolge** für das Speicherkonto im Cloud-Partnerportal ein.

    Exemplarische Verbindungszeichenfolge:

    ```sql
    DefaultEndpointsProtocol=https;AccountName=myAccountName;AccountKey=myAccountKey;EndpointSuffix=core.windows.net
    ```

      ![Azure-Speicherschlüssel](./media/cloud-partner-portal-lead-management-instructions-azure-table/azurestoragekeys.png)

Die Daten in Ihrem Tabellenspeicher können im [Azure Storage-Explorer](https://azurestorageexplorer.codeplex.com/) oder in einem ähnlichen Tool angezeigt werden. Außerdem können damit auch Daten exportiert werden.

## <a name="use-microsoft-flow-with-table-storage-optional"></a>Verwenden von Microsoft Flow mit Tabellenspeicher (*optional*)

Mit [Microsoft Flow](https://docs.microsoft.com/flow/) können automatisch Benachrichtigungen gesendet werden, wenn Ihrem Tabellenspeicher ein Lead hinzugefügt wird. Sollten Sie noch kein Microsoft Flow-Konto besitzen, [registrieren Sie sich für ein kostenloses Konto](https://flow.microsoft.com/).

### <a name="lead-notification-example"></a>Beispiel zu Leadbenachrichtigung

In diesem Beispiel wird die Erstellung eines einfachen Flows gezeigt. Der Flow sendet stündlich eine automatische E-Mail-Benachrichtigung, wenn Ihrem Tabellenspeicher neue Leads hinzugefügt werden.

1. Melden Sie sich bei Ihrem Microsoft Flow-Konto an.
1. Klicken Sie im Navigationsbereich auf der linken Seite auf **Meine Flows**.
1. Wählen Sie auf der oberen Navigationsleiste die Option **+Neu** aus.  
1. Wählen Sie in der Dropdownliste die Option **Ohne Vorlage erstellen** aus.
1. Wählen Sie unter **Flow ohne Vorlage erstellen** die Option **Ohne Vorlage erstellen** aus.

   ![Neuen Flow ohne Vorlage erstellen](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-create-from-blank.png)

1. Wählen Sie auf der Seite „Connectors und Trigger durchsuchen“ die Option **Trigger** aus.
1. Wählen Sie unter **Trigger** die Option **Wiederholung** aus.
1. Übernehmen Sie im Fenster **Wiederholung** für **Intervall** die Standardeinstellung **1**. Wählen Sie in der Dropdownliste **Häufigkeit** die Option **Stunde** aus.

   >[!NOTE] 
   >In diesem Beispiel wird ein einstündiges Intervall verwendet. Sie können aber auch ein Intervall und eine Häufigkeit auswählen, die Ihre Geschäftsanforderungen am besten erfüllen.

   ![Festlegen einer einstündigen Wiederholungshäufigkeit](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-recurrence-dropdown.png)

1. Wählen Sie **+Neuer Schritt** aus.
1. Suchen Sie nach **Vergangene Zeit abrufen**, und wählen Sie anschließend unter **Aktionen** die Option **Vergangene Zeit abrufen** aus.

    ![Suchen und Auswählen der Aktion „Vergangene Zeit abrufen“](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-search-getpasttime.png)

1. Legen Sie im Fenster **Vergangene Zeit abrufen** den Wert für **Intervall** auf **1** fest.  Wählen Sie in der Dropdownliste **Zeiteinheit** die Einheit **Stunde** aus.
    >[!IMPORTANT] 
    >Vergewissern Sie sich, dass die Werte für **Intervall** und **Zeiteinheit** mit dem Intervall und der Häufigkeit übereinstimmen, die Sie in Schritt 8 für die Wiederholung konfiguriert haben.

    ![Festlegen des Intervalls für „Vergangene Zeit abrufen“](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-getpast-time.png)

    >[!TIP] 
    >Sie können Ihren Flow jederzeit überprüfen, um sich zu vergewissern, dass die einzelnen Schritte ordnungsgemäß konfiguriert sind: Wählen Sie auf der Flow-Menüleiste die Option **Flowprüfung** aus.

In den nächsten Schritten wird eine Verbindung mit Ihrer Speichertabelle hergestellt und die Verarbeitungslogik für neue Leads eingerichtet.

1. Wählen Sie nach dem Schritt **Vergangene Zeit abrufen** die Option **+Neuer Schritt** aus, und suchen Sie nach **Entitäten abrufen**.
1. Wählen Sie unter **Aktionen** die Option **Entitäten abrufen** aus, und wählen Sie dann **Erweiterte Optionen anzeigen** aus.
1. Füllen Sie im Fenster **Entitäten abrufen** die folgenden Felder aus:

   - **Tabelle**: Der Name Ihres Tabellenspeichers. In der folgenden Abbildung wurde „MarketPlaceLeads“ eingegeben:

     ![Benutzerdefinierten Wert für den Azure-Tabellennamen auswählen](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-getentities-table-name.png)

   - **Abfrage filtern**: Wenn Sie dieses Feld auswählen, wird das Symbol **Vergangene Zeit abrufen** in einem Popupfenster angezeigt. Wählen Sie **Vergangene Zeit** aus, um diesen Wert als Zeitstempel zum Filtern der Abfrage zu verwenden. Alternativ können Sie die folgende Funktion in das Feld einfügen:
   
      `CreatedTime Timestamp gt datetime'@{body('Get_past_time')}'` 

     ![Einrichten der Funktion zum Filtern der Abfrage](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-getentities-filterquery.png)

1. Wählen Sie **Neuer Schritt** aus, um eine Bedingung hinzuzufügen, mit der Ihr Tabellenspeicher auf neue Leads überprüft wird.

   ![Verwenden von „Neuer Schritt“, um eine Bedingung für die Überprüfung des Tabellenspeichers hinzuzufügen](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-add-filterquery-new-step.png)

1. Wählen Sie im Fenster **Aktion auswählen** die Option **Aktionen** und anschließend **Condition Control** (Bedingungs-Steuerelement) aus.

     ![Hinzufügen eines Bedingungssteuerelements](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-action-condition-control.png)

1. Wählen Sie im Fenster **Bedingung** die Option **Wert auswählen** und anschließend im Popupfenster die Option **Ausdruck** aus.
1. Fügen Sie `length(body('Get_entities')?['value'])` in das Feld ***fx*** ein. Wählen Sie **OK** aus, um diese Funktion hinzuzufügen. 



     ![Eine Funktion zur Bedingung hinzufügen](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-condition-fx0.png)

1. Richten Sie die Aktion ein, die entsprechend dem Ergebnis der Bedingung ausgeführt werden soll.

    1. Wählen Sie in der Dropdownliste die Option **ist größer als** aus.
   1. Geben Sie **0** als Wert ein.

     ![Einrichten einer Aktion auf der Grundlage von Bedingungsergebnissen](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-condition-pick-action.png)

1. Hat die Bedingung das Ergebnis „Wenn nein“, soll keine Aktion ausgeführt werden.

    Hat die Bedingung das Ergebnis „Wenn ja“, soll eine Aktion ausgelöst werden, die eine Verbindung mit Ihrem Office 365-Konto herstellt, um eine E-Mail zu senden:
   1. Wählen Sie **Aktion hinzufügen**aus.
   1. Wählen Sie **E-Mail senden** aus.
   1. Geben Sie im Fenster **E-Mail senden** Informationen in die folgenden Felder ein:

      - **An**: Geben Sie eine E-Mail-Adresse für jede Person ein, die die Benachrichtigung erhalten soll.
      - **Betreff**: Geben Sie einen Betreff für die E-Mail ein. Beispiel:  *Neue Leads!*
      - **Text**: Geben Sie den Text ein, der in jeder E-Mail enthalten sein soll (optional). Fügen Sie außerdem `body('Get_entities')?['value']` als Funktion zum Einfügen von Leadinformationen ein.

        >[!NOTE] 
        >In den Text der E-Mail können weitere statische oder dynamische Datenpunkte eingefügt werden.

      ![E-Mail für Leadbenachrichtigung einrichten](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-emailbody-fx.png)

1. Wählen Sie **Speichern** aus, um den Flow zu speichern. Microsoft Flow testet den Flow automatisch auf Fehler. Sind keine Fehler vorhanden, wird Ihr Flow gestartet, nachdem er gespeichert wurde.

    Die folgende Abbildung zeigt anhand eines Beispiels, wie der fertige Flow aussehen sollte.

    [![Fertige Flowsequenz](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-end-to-end-thmb.png)](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-end-to-end.png)

    (*Wählen Sie die Abbildung aus, um sie zu vergrößern.* )

### <a name="manage-your-flow"></a>Verwalten Ihres Flows

Ist der Flow erst einmal aktiv, lässt er sich ganz einfach verwalten. Sie haben vollständige Kontrolle über den Flow. Sie können ihn z. B. beenden und bearbeiten, und Sie können einen Ausführungsverlauf anzeigen und Analysen abrufen. Die folgende Abbildung zeigt die Optionen für die Flowverwaltung:

 ![Optionen für die Flowverwaltung](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-manage-completed.png)

Der Flow wird ausgeführt, bis Sie **Flow deaktivieren** auswählen.

Sollten Sie keine E-Mail-Benachrichtigungen zu Leads erhalten, wurden Ihrem Tabellenspeicher keine neuen Leads hinzugefügt.
Im Falle eines Flowfehlers erhalten Sie eine E-Mail wie die folgende:

 ![E-Mail-Benachrichtigung zu Flowfehler](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-failure-note.png)

## <a name="next-steps"></a>Nächste Schritte

[Konfigurieren von Kundenleads](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-get-customer-leads)
