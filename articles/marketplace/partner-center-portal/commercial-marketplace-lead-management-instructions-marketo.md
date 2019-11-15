---
title: Konfigurieren der Leadverwaltung in Marketo | Azure Marketplace
description: Konfigurieren der Leadverwaltung in Marketo für Azure Marketplace-Kunden.
services: Azure, Marketplace, commercial marketplace, Partner Center
author: qianw211
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 08/01/2019
ms.author: evansma
ms.openlocfilehash: 8d13e8c3aeabf6d3fdea80ffddbae47b80adc139
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73812136"
---
# <a name="configure-lead-management-in-marketo"></a>Konfigurieren der Leadverwaltung in Marketo

In diesem Artikel ist beschrieben, wie Sie Ihr Marketo-CRM-System einrichten, um Vertriebsleads aus dem Marketplace-Angebot zu verarbeiten.

## <a name="set-up-your-marketo-crm-system"></a>Einrichten Ihres Marketo-CRM-Systems

1. Melden Sie sich bei Marketo an.
2. Wählen Sie **Design Studio** (Designstudio) aus.
    ![„Design Studio“ in Marketo](./media/commercial-marketplace-lead-management-instructions-marketo/marketo-1.png)

3.  Wählen Sie **New Form** (Neues Formular) aus.
    ![Neues Formular in Marketo](./media/commercial-marketplace-lead-management-instructions-marketo/marketo-2.png)

4.  Füllen Sie im Fenster „New Form“ (Neues Formular) die Pflichtfelder aus, und wählen Sie **Erstellen** aus.
    ![Erstellen eines neuen Formulars in Marketo](./media/commercial-marketplace-lead-management-instructions-marketo/marketo-3.png)

5.  Wählen Sie im Abschnitt „Field Details“ (Felddetails) **Fertig stellen** aus.
    ![Fertigstellen eines Formulars in Marketo](./media/commercial-marketplace-lead-management-instructions-marketo/marketo-4.png)

6.  Bestätigen Sie die Eingaben, und schließen Sie sie.

7. Wählen Sie auf der Registerkarte *MarketplaceLeadBacked* die Option **Embed Code** (Code einbetten) aus. 

    ![Einbettungscode](./media/commercial-marketplace-lead-management-instructions-marketo/marketo-6.png)

8. Der Marketo-Einbettungscode wird etwa folgendermaßen angezeigt:

    ```
    <form id="mktoForm_1179"></form>
    <script>MktoForms2.loadForm("("//app-ys12.marketo.com", "123-PQR-789", 1179);</script>
    ```

9. Kopieren Sie die Werte für die unten aufgeführten Felder, die im Einbettungscodeformular angezeigt werden. Sie verwenden diese Werte, um Ihr Angebot so zu konfigurieren, dass Sie im nächsten Schritt Leads erhalten. Das nächste Beispiel dient als Leitfaden zum Abrufen der benötigten IDs aus dem Marketo-Einbettungscode.

    - Server-ID: **ys12**
    - Munchkin-ID: **123-PQR-789**
    - Formular-ID: **1179**

    **Eine andere Möglichkeit zum Ermitteln dieser Werte**

    - Die Server-ID befindet sich in der URL Ihrer Marketo-Instanz. Beispiel: „`serverID.marketo.com`“.
    - Sie erhalten die Munchkin-ID Ihres Abonnements, indem Sie im Menü „Admin“ (Verwaltung) > „Munchkin“ das Feld „Munchkin Account ID“ (Munchkin-Konto-ID) anzeigen. Sie finden die ID auch im ersten Teil Ihrer Marketo-REST-API-Hostdomäne: `https://{Munchkin ID}.mktorest.com`.
    - Die Formular-ID ist die ID des Einbettungscodeformulars, das Sie in Schritt 7 erstellt haben, um Leads zu Ihrem Marketplace zu leiten.

## <a name="configure-your-offer-to-send-leads-to-marketo"></a>Konfigurieren Ihres Angebots zum Senden von Leads zu Marketo

Führen Sie die folgenden Schritte aus, um die Leadverwaltungsinformationen für Ihr Angebot im Veröffentlichungsportal zu konfigurieren: 

1. Navigieren Sie zur Seite **Angebotseinrichtung** für Ihr Angebot.
1. Wählen Sie im Abschnitt „Leadverwaltung“ die Option **Verbinden** aus. 

    ![Leadverwaltung – Verbinden](./media/commercial-marketplace-lead-management-instructions-marketo/lead-management-connect.png)

1. Wählen Sie im Popupfenster „Verbindungsdetails“ **Marketo** als Leadziel aus.

    ![Auswählen einer Leadzielgruppe](./media/commercial-marketplace-lead-management-instructions-marketo/choose-lead-destination.png)

4. Geben Sie die **Server-ID**, die **Munching-Konto-ID** und die **Formular-ID** an.

    >[!Note]
    >Sie müssen die Konfiguration der übrigen Einstellungen des Angebots abschließen und veröffentlichen, damit Sie Leads für das Angebot erhalten. 

