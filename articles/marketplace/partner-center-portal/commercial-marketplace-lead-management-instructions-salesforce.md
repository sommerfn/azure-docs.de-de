---
title: Konfigurieren der Leadverwaltung in Salesforce | Azure Marketplace
description: Konfigurieren Sie die Leadverwaltung in Salesforce für Azure Marketplace-Kunden.
services: Azure, Marketplace, commercial marketplace, Partner Center
author: qianw211
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 08/01/2019
ms.author: evansma
ms.openlocfilehash: bdd3bb50fc69083c73eb01a84bf0fb0db82a8a65
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73812151"
---
# <a name="configure-lead-management-for-salesforce"></a>Konfigurieren der Leadverwaltung in Salesforce

In diesem Artikel ist beschrieben, wie Sie Ihr Salesforce-System einrichten, um Vertriebsleads aus dem Marketplace-Angebot zu verarbeiten.

## <a name="set-up-your-salesforce-system"></a>Einrichten des Salesforce-Systems

1. Melden Sie sich bei Salesforce an.
2. Wenn Sie die Salesforce Lighting Experience verwenden:
    1. Wählen Sie auf der Salesforce-Startseite **Setup** aus.
    ![Einrichtung von Salesforce](./media/commercial-marketplace-lead-management-instructions-salesforce/salesforce-1.png)

    1. Navigieren Sie auf der Seite „Setup“ über den linken Navigationsbereich zu **Platform Tools (Plattformtools) > Feature Settings (Featureeinstellungen) > Marketing > Web-to-Lead**.
    ![Salesforce Web-to-Lead](./media/commercial-marketplace-lead-management-instructions-salesforce/salesforce-2.png)

3. Wenn Sie die klassische Salesforce-Benutzeroberfläche verwenden:
    1. Wählen Sie auf der Salesforce-Startseite **Setup** aus.
    ![Klassisches Salesforce-Setup](./media/commercial-marketplace-lead-management-instructions-salesforce/salesforce-classic-setup.png)

    1. Navigieren Sie auf der Seite „Setup“ über den linken Navigationsbereich zu **Build (Erstellen) > Customize (Anpassen) > Leads > Web-to-Lead**.
    ![Klassisches Salesforce Web-to-Lead](./media/commercial-marketplace-lead-management-instructions-salesforce/salesforce-classic-web-to-lead.png)

Die restlichen Anweisungen sind identisch, unabhängig davon, welche Salesforce-Darstellung Sie verwenden.

4. Klicken Sie auf der Seite **Web-to-Lead Setup** (Web-to-Lead-Setup) auf **Create Web-to-Lead Form** (Web-to-Lead-Formular erstellen).
5. Wählen Sie auf **Web-to-Lead Setup** die Schaltfläche **Create Web-to-Lead Form** aus.
    ![Salesforce – Web-to-Lead-Setup](./media/commercial-marketplace-lead-management-instructions-salesforce/salesforce-3.png)

6. Stellen Sie sicher, dass auf der Seite **Create a Web-to-Lead Form** (Web-to-Lead-Formular erstellen) die Einstellung `the Include reCAPTCHA in HTML` deaktiviert ist, und wählen Sie **Generate** (Generieren) aus. 
    ![Salesforce – Web-to-Lead-Formular erstellen ](./media/commercial-marketplace-lead-management-instructions-salesforce/salesforce-4.png)

7. Es wird HTML-Text angezeigt. Suchen Sie den Text „oid“, kopieren Sie den **Wert „oid“**  aus dem HTML-Text (nur den Text in Anführungszeichen), und speichern Sie ihn. Sie fügen diesen Wert in das Feld **Organisations-ID** im Veröffentlichungsportal ein.
    ![Salesforce – Web-to-Lead-Formular erstellen ](./media/commercial-marketplace-lead-management-instructions-salesforce/salesforce-5.png)

8. Wählen Sie **Finished** (Abgeschlossen) aus.

## <a name="configure-your-offer-to-send-leads-to-salesforce"></a>Konfigurieren Ihres Angebots zum Senden von Leads zu Salesforce

Führen Sie die folgenden Schritte aus, um die Leadverwaltungsinformationen für Ihr Angebot im Veröffentlichungsportal zu konfigurieren:

1. Navigieren Sie zur Seite **Angebotseinrichtung** für Ihr Angebot.
1. Wählen Sie im Abschnitt „Leadverwaltung“ die Option **Verbinden** aus.
    ![Leadverwaltung – Verbinden](./media/commercial-marketplace-lead-management-instructions-salesforce/lead-management-connect.png)

1. Wählen Sie im Popupfenster „Verbindungsdetails“ **Salesforce** als **Leadzielgruppe** aus, und fügen Sie die `oid` aus dem in den vorherigen Schritten erstellten Web-to-Lead-Formular in das Feld **Organisations-ID** ein.

1. Wählen Sie **Speichern** aus. 

    >[!Note]
    >Sie müssen die Konfiguration der übrigen Einstellungen des Angebots abschließen und veröffentlichen, damit Sie Leads für das Angebot erhalten.

    ![Verbindungsdetails – Leadzielgruppe auswählen](./media/commercial-marketplace-lead-management-instructions-salesforce/choose-lead-destination.png)

    ![Verbindungsdetails – Leadzielgruppe auswählen](./media/commercial-marketplace-lead-management-instructions-salesforce/connection-details.png)