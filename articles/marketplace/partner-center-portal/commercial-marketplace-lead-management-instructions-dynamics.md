---
title: Leadverwaltung für Dynamics 365 for Customer Engagement | Azure Marketplace
description: Konfigurieren der Leadverwaltung für Dynamics 365 for Customer Engagement.
services: Azure, Marketplace, commercial marketplace, Partner Center
author: qianw211
ms.service: marketplace
ms.topic: conceptual
ms.date: 07/30/2019
ms.author: evansma
ms.openlocfilehash: 181a3f3a8d3cabb2fdf6caf79cef16201fab0c68
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72177805"
---
# <a name="configure-lead-management-for-dynamics-365-for-customer-engagement"></a>Konfigurieren der Leadverwaltung für Dynamics 365 for Customer Engagement

In diesem Artikel wird beschrieben, wie Sie Dynamics 365 for Customer Engagement (bisher Dynamics CRM Online) einrichten. Zur Verarbeitung von Vertriebsleads von Ihrem Marketplace-Angebot finden Sie [hier](https://docs.microsoft.com/dynamics365/customerengagement/on-premises/admin/on-prem-server-based-sharepoint-online) weitere Informationen zu der Änderung. 

>[!Note]
>Diese Anweisungen gelten speziell für die von Microsoft gehostete Dynamics 365 for Customer Engagement-Cloudumgebung. Eine direkte Verbindung mit einer lokalen Dynamics-Umgebung wird derzeit nicht unterstützt. Es gibt andere Möglichkeiten, Leads zu erhalten, z. B. Konfigurieren eines [HTTPS-Endpunkts](./commercial-marketplace-lead-management-instructions-https.md) oder einer [Azure-Tabelle](./commercial-marketplace-lead-management-instructions-azure-table.md) zum Empfangen von Leads.

## <a name="prerequisites"></a>Voraussetzungen

Für die Schritte in diesem Artikel sind die folgenden Benutzerberechtigungen erforderlich:

* Sie müssen Administrator für Ihre Dynamics 365 for Customer Engagement-Instanz sein, um eine Lösung installieren und diese Anweisungen befolgen zu können.
* Sie müssen Mandantenadministrator sein, um ein neues Dienstkonto für den Leaddienst zu erstellen, mit dem Leads von Marketplace-Angeboten gesendet werden.
* Sie benötigen Zugriff auf das Office 365-Verwaltungsportal.
* Sie benötigen Zugriff auf das Azure-Portal.

## <a name="install-the-solution"></a>Installieren der Lösung

1.  Laden Sie die Lösung [Microsoft Marketplace Lead Writer](https://mpsapiprodwus.blob.core.windows.net/documentation/MicrosoftMarketplacesLeadIntegrationSolution_1_0_0_0_target_CRM_6.1_managed.zip) herunter, und speichern Sie sie lokal auf Ihrem Computer.

2.  Öffnen Sie Dynamics 365 for Customer Engagement, indem Sie zur URL Ihrer Dynamics-Instanz navigieren (z. B. `https://tenant.crm.dynamics.com`).

3.  Wählen Sie das Zahnradsymbol und dann **Erweiterte Einstellungen** auf der oberen Navigationsleiste aus, um auf „Einstellungen“ zuzugreifen.
 
    ![Dynamics – Erweiterte Einstellungen](./media/commercial-marketplace-lead-management-instructions-dynamics/dynamics-advanced-settings.png)

4.  Greifen Sie auf der Seite „Einstellungen“ auf der oberen Navigationsleiste auf das Menü „Einstellungen“ zu, und wählen Sie **Lösungen** aus.

    >[!Note]
    >Wenn die Optionen der nächsten Bildschirmaufnahme nicht angezeigt werden, verfügen Sie nicht über die zum Fortfahren erforderlichen Berechtigungen. Wenden Sie sich an einen Administrator für Ihre Dynamics 365 for Customer Engagement-Instanz.

    ![Dynamics 365 – Lösungen](./media/commercial-marketplace-lead-management-instructions-dynamics/dynamics-solutions.png)

5. Wählen Sie auf der Seite „Lösungen“ die Option **Importieren** aus, und navigieren Sie zum Speicherort der Lösung *Microsoft Marketplace Lead Writer*, die Sie in Schritt 1 heruntergeladen haben.

    ![Dynamics 365 for Customer Engagement – Importieren](./media/commercial-marketplace-lead-management-instructions-dynamics/dynamics-crm-import.png)

6. Schließen Sie den Import der Lösung ab, indem Sie die Anweisungen des Assistenten „Lösung importieren“ befolgen.

## <a name="configure-user-permissions"></a>Konfigurieren von Benutzerberechtigungen

Zum Schreiben von Leads in Ihre Dynamics 365 for Customer Engagement-Instanz müssen Sie ein Dienstkonto für uns freigeben und Berechtigungen für das Konto konfigurieren.

Führen Sie die folgenden Schritte aus, um das Dienstkonto zu erstellen und Berechtigungen zuzuweisen. Sie können **Azure Active Directory** oder **Office 365** verwenden.

>[!Note]
>Je nach der ausgewählten Authentifizierungsoption können Sie mit den entsprechenden Anweisungen fortfahren. Siehe [Azure Active Directory](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics#azure-active-directory) oder [Office 365](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics#office-365).

### <a name="azure-active-directory"></a>Azure Active Directory

Diese Option wird empfohlen, da sie den Vorteil hat, dass Sie Ihren Benutzernamen und Ihr Kennwort nie aktualisieren müssen, um weiterhin Leads zu erhalten. Zur Verwendung der Azure Active Directory-Option geben Sie die App-ID, den Anwendungsschlüssel und die Verzeichnis-ID aus Ihrer Active Directory-Anwendung an.

Führen Sie die folgenden Schritte aus, um Azure Active Directory für Dynamics 365 for Customer Engagement zu konfigurieren.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, und wählen Sie dann im linken Navigationsbereich den Azure Active Directory-Dienst aus.

2. Wählen Sie im linken Azure Active Directory-Navigationsbereich die Option **Eigenschaften** aus, und kopieren Sie den Wert der **Verzeichnis-ID** auf dieser Seite. Speichern Sie diesen Wert für die *Verzeichnis-ID*. Diesen Wert müssen Sie im Veröffentlichungsportal angeben, um Leads für Ihr Marketplace-Angebot zu erhalten.

    ![Azure Active Directory – Eigenschaften](./media/commercial-marketplace-lead-management-instructions-dynamics/aad-properties.png)

3. Wählen Sie im linken Azure Active Directory-Navigationsbereich die Option **App-Registrierungen** und anschließend **Neue Registrierung** auf dieser Seite aus.
4. Geben Sie einen Namen für die Anwendung ein. Geben Sie einen aussagekräftigen Anwendungsnamen an.
5. Wählen Sie unter „Unterstützte Kontotypen“ die Option **Konten in einem beliebigen Organisationsverzeichnis** aus.
6. Wählen Sie unter „Umleitungs-URI“ die Option **Web** aus, und geben Sie einen URI an (z. B. `https://contosoapp1/auth`). 
7. Wählen Sie **Registrieren**.

    ![Registrieren einer Anwendung](./media/commercial-marketplace-lead-management-instructions-dynamics/register-an-application.png)

8. Nachdem Sie die Anwendung registriert haben, greifen Sie auf die Übersichtsseite der Anwendung zu, und kopieren Sie den Wert für die **Anwendungs-ID (Client)** auf dieser Seite. Speichern Sie diesen Wert für die *Anwendungs-ID (Client)* . Diesen Wert müssen Sie im Veröffentlichungsportal und in Dynamics angeben, um Leads für Ihr Marketplace-Angebot zu erhalten.

    ![Anwendungs-ID (Client)](./media/commercial-marketplace-lead-management-instructions-dynamics/application-id.png)

9. Wählen Sie im linken Navigationsbereich der App die Option **Zertifikate und Geheimnisse** und dann auf dieser Seite die Option **Neuer geheimer Clientschlüssel** aus. Geben Sie eine aussagekräftige Beschreibung für den geheimen Clientschlüssel ein, und wählen Sie unter „Läuft ab“ die Option **Nie** aus. Wählen Sie **Hinzufügen** aus, um den geheimen Clientschlüssel zu erstellen.

    ![Anwendung – Zertifikate und Geheimnisse](./media/commercial-marketplace-lead-management-instructions-dynamics/aad-certificates-secrets.png)

10. Nachdem der geheime Clientschlüssel erfolgreich erstellt wurde, **kopieren Sie den zugehörigen Wert**. Der Wert kann nicht mehr abgerufen werden, nachdem Sie die Seite verlassen haben. Speichern Sie diesen Wert für den *geheimen Clientschlüssel*. Diesen Wert müssen Sie im Veröffentlichungsportal angeben, um Leads für Ihr Marketplace-Angebot zu erhalten. 
11. Wählen Sie im linken Navigationsbereich der App die Option **API-Berechtigungen** und dann **Berechtigung hinzufügen** aus.
12. Wählen Sie „Microsoft-APIs“ und dann **Dynamics CRM** als API aus.
13. Achten Sie darauf, dass unter *Welche Art von Berechtigungen sind für Ihre Anwendung erforderlich?* die Option **Delegierte Berechtigungen** ausgewählt ist. Aktivieren Sie die Berechtigung für **user_impersonation** *Access Common Data Service as organization users* (Als Organisationsbenutzer auf Common Data Service zugreifen). Wählen Sie **Berechtigungen hinzufügen** aus.

    ![Hinzufügen von Berechtigungen](./media/commercial-marketplace-lead-management-instructions-dynamics/api-permissions.png)

14. Nachdem Sie die Schritte 1 bis 13 im Azure-Portal ausgeführt haben, navigieren Sie zu Ihrer Dynamics 365 for Customer Engagement-Instanz, indem Sie zur entsprechenden URL navigieren (z. B. `https://tenant.crm.dynamics.com`).
15. Wählen Sie das Zahnradsymbol und dann **Erweiterte Einstellungen** auf der oberen Navigationsleiste aus, um auf „Einstellungen“ zuzugreifen.
16. Greifen Sie auf der Seite „Einstellungen“ auf der oberen Navigationsleiste auf das Menü „Einstellungen“ zu, und wählen Sie **Sicherheit** aus.
17. Wählen Sie auf der Seite „Sicherheit“ die Option **Benutzer** aus.  Wählen Sie auf der Seite „Benutzer“ die Dropdownliste „Aktivierte Benutzer“ aus, um zu **Anwendungsbenutzer** zu wechseln.
18. Wählen Sie **Neu**, um einen neuen Benutzer zu erstellen. 

    ![Erstellen eines neuen Benutzers](./media/commercial-marketplace-lead-management-instructions-dynamics/application-users.png)

19. Stellen Sie unter **Neuer Benutzer** sicher, dass „BENUTZER: ANWENDUNGSBENUTZER ausgewählt ist. Geben Sie einen Benutzernamen, einen vollständigen Namen und eine E-Mail-Adresse für den Benutzer an, der für diese Verbindung verwendet werden soll. Fügen Sie außerdem die **Anwendungs-ID** für die App ein, die Sie in Schritt 8 im Azure-Portal erstellt haben. Wählen Sie **Speichern und schließen** aus, um das Hinzufügen des Benutzers abzuschließen.

    ![Neuer Benutzer](./media/commercial-marketplace-lead-management-instructions-dynamics/new-user-info.png)

20. Navigieren Sie zu „Sicherheitseinstellungen“ in diesem Artikel, um die Konfiguration der Verbindung für diesen Benutzer abzuschließen.

### <a name="office-365"></a>Office 365

Wenn Sie Azure Active Directory nicht verwenden möchten, können Sie im *Microsoft 365 Admin Center* einen neuen Benutzer registrieren. Sie müssen Ihren Benutzernamen/Ihr Kennwort alle 90 Tage aktualisieren, um weiterhin Leads zu erhalten.

Führen Sie die folgenden Schritte aus, um Office 365 für Dynamics 365 for Customer Engagement zu konfigurieren.

1. Melden Sie sich beim [Microsoft 365 Admin Center](https://admin.microsoft.com) an.

2. Wählen Sie **Benutzer hinzufügen**.

    ![Microsoft 365 Admin Center – Benutzer hinzufügen](./media/commercial-marketplace-lead-management-instructions-dynamics/ms-365-add-user.png)

4. Erstellen Sie einen neuen Benutzer für den Leadschreibddienst. Konfigurieren Sie die folgenden Einstellungen:

    * Geben Sie einen Benutzernamen an.
    * Geben Sie ein Kennwort an, und deaktivieren Sie die Option „Diesen Benutzer veranlassen, bei der ersten Anmeldung sein Kennwort zu ändern“.
    * Wählen Sie die Rolle „Benutzer (kein Administratorzugriff)“ für den Benutzer aus.
    * Wählen Sie wie im nächsten Screenshot gezeigt „Dynamics 365 Customer Engagement-Plan“ als Produktlizenz aus. Die gewählte Lizenz wird Ihnen in Rechnung gestellt. 

Speichern Sie diese Werte, da sie die Werte für *Benutzername und Kennwort* im Veröffentlichungsportal angeben müssen, um Leads für Ihr Marketplace-Angebot zu erhalten.

![Microsoft 365 Admin Center – Neuer Benutzer](./media/commercial-marketplace-lead-management-instructions-dynamics/ms-365-new-user.png)

## <a name="security-settings"></a>Sicherheitseinstellungen

Der letzte Schritt ist das Aktivieren des erstellten Benutzers für das Schreiben der Leads.

1. Öffnen Sie Dynamics 365 for Customer Engagement, indem Sie zur URL Ihrer Dynamics-Instanz navigieren (z. B. `https://tenant.crm.dynamics.com`).
2. Wählen Sie das Zahnradsymbol und dann **Erweiterte Einstellungen** auf der oberen Navigationsleiste aus, um auf „Einstellungen“ zuzugreifen.
3. Greifen Sie auf der Seite „Einstellungen“ auf der oberen Navigationsleiste auf das Menü „Einstellungen“ zu, und wählen Sie **Sicherheit** aus.
4. Wählen Sie auf der Seite „Sicherheit“ die Option **Benutzer**, anschließend den Benutzer, den Sie im Abschnitt „Konfigurieren von Benutzerberechtigungen“ in diesem Artikel erstellt haben, und dann **Rollen verwalten** aus. 

    ![Verwalten von Rollen](./media/commercial-marketplace-lead-management-instructions-dynamics/security-manage-roles.png)

5. Suchen Sie die Rolle „Microsoft Marketplace Lead Writer“, und wählen Sie sie aus, um dem Benutzer diese Rolle zuzuweisen.

    ![Verwalten von Benutzerrollen](./media/commercial-marketplace-lead-management-instructions-dynamics/security-manage-user-roles.png)

    >[!Note]
    >Diese Rolle wird von der importierten Lösung erstellt und verfügt nur über die Berechtigungen zum Schreiben der Leads und zum Nachverfolgen der Lösungsversion, um die Kompatibilität sicherzustellen.

6. Navigieren Sie zurück zur Seite „Sicherheit“, und wählen Sie **Sicherheitsrollen** aus. Suchen Sie die Rolle „Microsoft Marketplace Lead Writer“, und wählen Sie sie aus.

    ![Sicherheitsrollen](./media/commercial-marketplace-lead-management-instructions-dynamics/security-roles.png)

7. Wählen Sie in der Sicherheitsrolle die Registerkarte **Kerndatensätze** aus. Suchen Sie die Entität „UI-Einstellungen für Benutzerentität“, und aktivieren Sie die Berechtigungen zum Erstellen, Lesen und Schreiben für diese Entität (gelber Viertelkreis), indem Sie einmal auf die entsprechenden Kreise klicken.

    ![Microsoft Marketplace Lead Writer – Kerndatensätze](./media/commercial-marketplace-lead-management-instructions-dynamics/marketplace-lead-writer.png)

8. Navigieren Sie zur Registerkarte **Anpassung**. Suchen Sie die Entität „Systemauftrag“, und aktivieren Sie die Berechtigungen zum Lesen, Schreiben und Anfügen für die Organisation (grüne Kreise) für diese Entität, indem Sie viermal in jeden der entsprechenden Kreise klicken.

    ![Microsoft Marketplace Lead Writer – Anpassung](./media/commercial-marketplace-lead-management-instructions-dynamics/marketplace-lead-writer-customization.png)

9. Klicken Sie auf **Speichern und schließen**.

## <a name="configure-your-offer-to-send-leads-to-dynamics-365-for-customer-engagement"></a>Konfigurieren Ihres Angebots zum Senden von Leads an Dynamics 365 for Customer Engagement

Führen Sie die folgenden Schritte aus, um die Leadverwaltungsinformationen für Ihr Angebot im Veröffentlichungsportal zu konfigurieren:

1. Navigieren Sie zur Seite **Angebotseinrichtung** für Ihr Angebot.
2. Wählen Sie im Abschnitt „Leadverwaltung“ die Option **Verbinden** aus.

    ![Verbinden mit der Leadverwaltung](./media/commercial-marketplace-lead-management-instructions-dynamics/connect-lead-management.png)

3. Wählen Sie im Popupfenster „Verbindungsdetails“ die Option **Dynamics 365 for Customer Engagement** als Leadziel aus.

    ![Verbindungsdetails – Leadziel](./media/commercial-marketplace-lead-management-instructions-dynamics/connection-details-lead-destination.png)

4. Geben Sie die **Dynamics 365-Instanz-URL** an, z. B. `https://contoso.crm4.dynamics.com`.
5. Wählen Sie die Methode der **Authentifizierung** aus: „Azure Active Directory“ oder „Office 365“. 
6. Wenn Sie „Azure Active Directory“ auswählen, geben Sie die **Anwendungs-ID (Client)** (z. B. `23456052-aaaa-bbbb-8662-1234df56788f`), die **Verzeichnis-ID** (z. B. `12345678-8af1-4asf-1234-12234d01db47`) und den **geheimen Clientschlüssel** (z. B. `1234ABCDEDFRZ/G/FdY0aUABCEDcqhbLn/ST122345nBc=`) an.

    ![Verbindungsdetails – Azure Active Directory](./media/commercial-marketplace-lead-management-instructions-dynamics/connection-details-application-id.png)

7. Wenn Sie „Office 365“ auswählen, geben Sie den **Benutzernamen** (z. B. `contoso@contoso.onmicrosoft.com`) und das Kennwort (z. B. `P@ssw0rd`) an.

    ![Verbindungsdetails – Benutzername](./media/commercial-marketplace-lead-management-instructions-dynamics/connection-details-authentication.png)

>[!Note]
>Sie müssen die Konfiguration der übrigen Einstellungen des Angebots abschließen und veröffentlichen, damit Sie Leads für das Angebot erhalten.
