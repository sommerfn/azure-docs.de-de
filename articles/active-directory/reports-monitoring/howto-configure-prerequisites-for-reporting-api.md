---
title: Voraussetzungen für den Zugriff auf die Azure Active Directory-Berichterstellungs-API | Microsoft-Dokumentation
description: Erfahren Sie, welche Voraussetzungen für den Zugriff auf die Azure AD-Berichterstellungs-API gelten.
services: active-directory
documentationcenter: ''
author: cawrites
manager: daveba
editor: ''
ms.assetid: ada19f69-665c-452a-8452-701029bf4252
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 08/30/2019
ms.author: chadam
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: f7b6fab4a4a36691bbdeb11975c7a93b97ab86cb
ms.sourcegitcommit: 6794fb51b58d2a7eb6475c9456d55eb1267f8d40
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70241487"
---
# <a name="prerequisites-to-access-the-azure-active-directory-reporting-api"></a>Voraussetzungen für den Zugriff auf die Azure Active Directory-Berichterstellungs-API

Die [Berichtserstellungs-APIs von Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) bieten Ihnen über eine Gruppe von REST-basierten APIs programmgesteuerten Zugriff auf die Daten. Sie können diese APIs über Programmiersprachen und Tools aufrufen.

Die Berichterstellungs-API verwendet [OAuth](https://docs.microsoft.com/azure/api-management/api-management-howto-protect-backend-with-aad) zum Autorisieren des Zugriffs auf die Web-APIs.

Um auf die Berichterstellungs-API zugreifen zu können, müssen Sie folgende Schritte ausführen:

1. [Zuweisen von Rollen](#assign-roles)
2. [Registrieren einer Anwendung](#register-an-application)
3. [Erteilen von Berechtigungen](#grant-permissions)
4. [Erfassen von Konfigurationseinstellungen](#gather-configuration-settings)

## <a name="assign-roles"></a>Zuweisen von Rollen

Um mithilfe der API auf die Berichtsdaten zuzugreifen, müssen Ihnen eine der folgenden Rollen zugewiesen sein:

- Sicherheitsleseberechtigter

- Sicherheitsadministrator

- Globaler Administrator


## <a name="register-an-application"></a>Registrieren einer Anwendung

Eine Registrierung ist auch dann erforderlich, wenn Sie mithilfe eines Skripts auf die Berichterstellungs-API zugreifen. Bei der Registrierung wird Ihnen eine **Anwendungs-ID** zugewiesen, die für die Autorisierungsaufrufe erforderlich ist und den Empfang von Token über Ihren Code ermöglicht.

Um Ihr Verzeichnis für den Zugriff auf die Azure AD-Berichterstellungs-API zu konfigurieren, müssen Sie sich beim [Azure-Portal](https://portal.azure.com) mit dem Konto eines Azure-Administrators anmelden, das auch der Verzeichnisrolle **Globaler Administrator** Ihres Azure AD-Mandanten angehört.

> [!IMPORTANT]
> Anwendungen, die unter Anmeldeinformationen mit Administratorberechtigungen ausgeführt werden, können äußerst einflussreich sein. Sorgen Sie also unbedingt dafür, dass die ID und geheimen Anmeldeinformationen der Anwendung an einem sicheren Ort aufbewahrt werden.
> 

**Registrieren Sie eine Azure AD-Anwendung wie folgt:**

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) im linken Navigationsbereich die Option **Azure Active Directory**.
   
    ![Register application (Anwendung registrieren)](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. Wählen Sie auf der Seite **Azure Active Directory** die Option **App-Registrierungen**.

    ![Register application (Anwendung registrieren)](./media/howto-configure-prerequisites-for-reporting-api/02.png) 

3. Wählen Sie auf der Seite **App-Registrierungen** die Option **Registrierung einer neuen Anwendung** aus.

    ![Register application (Anwendung registrieren)](./media/howto-configure-prerequisites-for-reporting-api/03.png)

4. Die Seite **Anwendung registrieren**:

    ![Register application (Anwendung registrieren)](./media/howto-configure-prerequisites-for-reporting-api/04.png)

    a. Geben Sie im Textfeld **Name** Folgendes ein: `Reporting API application`.

    b. Wählen Sie für **Unterstützte Kontotypen** die Option **Nur Konten in diesem Organisationsverzeichnis** aus.

    c. Wählen Sie unter **Umleitungs-URL** das Textfeld **Web** aus, und geben Sie `https://localhost` ein.

    d. Wählen Sie **Registrieren**. 


## <a name="grant-permissions"></a>Erteilen von Berechtigungen 

Abhängig von der API, auf die Sie zugreifen möchten, müssen Sie Ihrer App die folgenden Berechtigungen erteilen:  

| API | Berechtigung |
| --- | --- |
| Microsoft Azure Active Directory | Verzeichnisdaten lesen |
| Microsoft Graph | Alle Überwachungsprotokolldaten lesen |


![Register application (Anwendung registrieren)](./media/howto-configure-prerequisites-for-reporting-api/36.png)

Im folgenden Abschnitt werden die Schritte für beide APIs aufgelistet. Wenn Sie nicht auf eine der APIs zugreifen möchten, können Sie die zugehörigen Schritte überspringen.

**So erteilen Sie Ihrer Anwendung die Berechtigung zur Verwendung der APIs**:


1. Wählen Sie **API-Berechtigungen** und dann **Berechtigung hinzufügen** aus. 

    ![Register application (Anwendung registrieren)](./media/howto-configure-prerequisites-for-reporting-api/05.png)

2. Suchen Sie auf der Seite **API-Berechtigungen anfordern** unter **Unterstützte Legacy-APIs** nach **Azure Active Directory Graph**. 

    ![Register application (Anwendung registrieren)](./media/howto-configure-prerequisites-for-reporting-api/06.png)

3. Wählen Sie auf der Seite **Erforderliche Berechtigungen** die Option **Anwendungsberechtigungen** aus, erweitern Sie **Verzeichnis**, und aktivieren Sie dann das Kontrollkästchen **Directory.ReadAll**.  Wählen Sie **Berechtigungen hinzufügen** aus.

    ![Register application (Anwendung registrieren)](./media/howto-configure-prerequisites-for-reporting-api/07.png)

4. Wählen Sie auf der Seite **Berichterstellungs-API-Anwendung – API-Berechtigungen** die Option **Administratoreinwilligung erteilen** aus. 

    ![Register application (Anwendung registrieren)](./media/howto-configure-prerequisites-for-reporting-api/08.png)

5. Hinweis: **Microsoft Graph** wird bei der API-Registrierung standardmäßig hinzugefügt.

    ![Register application (Anwendung registrieren)](./media/howto-configure-prerequisites-for-reporting-api/15.png)

## <a name="gather-configuration-settings"></a>Erfassen von Konfigurationseinstellungen 

In diesem Abschnitt wird gezeigt, wie Sie die folgenden Einstellungen aus Ihrem Verzeichnis abrufen:

- Domänenname
- Client-ID
- Geheimer Clientschlüssel

Sie benötigen diese Werte, um Aufrufe an die Berichterstellungs-API zu konfigurieren. 

### <a name="get-your-domain-name"></a>Ermitteln des Domänennamens

**So ermitteln Sie den Domänennamen:**

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) im linken Navigationsbereich die Option **Azure Active Directory**.
   
    ![Register application (Anwendung registrieren)](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. Wählen Sie auf der Seite **Azure Active Directory** die Option **Benutzerdefinierte Domänennamen**.

    ![Register application (Anwendung registrieren)](./media/howto-configure-prerequisites-for-reporting-api/09.png) 

3. Kopieren Sie Ihren Domänennamen aus der Liste der Domänen.


### <a name="get-your-applications-client-id"></a>Abrufen der Client-ID der Anwendung

**So rufen Sie die Client-ID der Anwendung ab:**

1. Klicken Sie im linken Navigationsbereich des [Azure-Portals](https://portal.azure.com) auf **Azure Active Directory**.
   
    ![Register application (Anwendung registrieren)](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2. Wählen Sie Ihre Anwendung auf der Seite **App-Registrierungen** aus.

3. Navigieren Sie auf der Anwendungsseite zu **Anwendungs-ID**, und wählen Sie die Option **Click to copy** (Zum Kopieren klicken).

    ![Register application (Anwendung registrieren)](./media/howto-configure-prerequisites-for-reporting-api/11.png) 


### <a name="get-your-applications-client-secret"></a>Abrufen des geheimen Clientschlüssels der Anwendung
 Vermeiden Sie Fehler beim Zugreifen auf Überwachungsprotokolle oder beim Anmelden mithilfe der API.

**So rufen Sie den geheimen Clientschlüssel der Anwendung ab:**

1. Klicken Sie im linken Navigationsbereich des [Azure-Portals](https://portal.azure.com) auf **Azure Active Directory**.
   
    ![Register application (Anwendung registrieren)](./media/howto-configure-prerequisites-for-reporting-api/01.png) 

2.  Wählen Sie Ihre Anwendung auf der Seite **App-Registrierungen** aus.

3.  Wählen Sie auf der Seite **API-Anwendung** die Option **Zertifikate und Geheimnisse** aus, und klicken Sie im Abschnitt **Geheime Clientschlüssel** auf **+ Neuer geheimer Clientschlüssel**. 

    ![Register application (Anwendung registrieren)](./media/howto-configure-prerequisites-for-reporting-api/12.png)

5. Führen Sie auf der Seite **Geheimen Clientschlüssel hinzufügen** die folgenden Aktionen aus:

    a. Geben Sie im Textfeld **Beschreibung** Folgendes ein: `Reporting API`.

    b. Wählen Sie für **Läuft ab** die Option **In 2 Jahren** aus.

    c. Klicken Sie auf **Speichern**.

    d. Kopieren Sie den Schlüsselwert.

## <a name="troubleshoot-errors-in-the-reporting-api"></a>Beheben von Fehlern in der Berichterstellungs-API

In diesem Abschnitt werden die häufigsten Fehlermeldungen aufgeführt, die beim Zugreifen auf Aktivitätsberichte über die Microsoft Graph-API auftreten können, sowie Schritte zu deren Behebung.

### <a name="500-http-internal-server-error-while-accessing-microsoft-graph-v2-endpoint"></a>500 HTTP: Interner Serverfehler beim Zugriff auf den Microsoft Graph V2-Endpunkt.

Der Microsoft Graph v2-Endpunkt wird zurzeit nicht unterstützt. Stellen Sie sicher, dass Sie auf die Aktivitätsprotokolle mit dem Microsoft Graph v1-Endpunkt zugreifen.

### <a name="error-failed-to-get-user-roles-from-ad-graph"></a>Fehler Fehler beim Abrufen von Benutzerrollen aus AD Graph

 Melden Sie sich auf der Graph-Tester-Benutzeroberfläche über beide Anmeldeschaltflächen bei Ihrem Konto an, um zu vermeiden, dass bei der Anmeldung mit dem Graph-Tester ein Fehler ausgegeben wird. 

![Graph-Tester](./media/troubleshoot-graph-api/graph-explorer.png)

### <a name="error-failed-to-do-premium-license-check-from-ad-graph"></a>Fehler Fehler beim Überprüfen der Premium-Lizenz aus AD Graph 

Wenn Sie diese Fehlermeldung beim Versuch erhalten, auf Anmeldungen mithilfe des Graph-Explorers zuzugreifen, wählen Sie **Berechtigungen ändern** unter Ihrem Konto im linken Navigationsbereich aus, und wählen Sie dann **Tasks.ReadWrite** und **Directory.Read.All** aus. 

![Benutzeroberfläche zum Ändern von Berechtigungen](./media/troubleshoot-graph-api/modify-permissions.png)


### <a name="error-tenant-is-not-b2c-or-tenant-doesnt-have-premium-license"></a>Fehler Der Mandant ist nicht B2C, oder der Mandant besitzt keine Premium-Lizenz

Für den Zugriff auf Anmeldeberichte ist eine Azure Active Directory Premium 1-Lizenz (P1) erforderlich. Wenn diese Fehlermeldung beim Zugriff auf Anmeldungen angezeigt wird, stellen Sie sicher, dass Ihr Mandant mit einer Azure AD P1-Lizenz lizenziert ist.

### <a name="error-the-allowed-roles-does-not-include-user"></a>Fehler Die zulässigen Rollen enthalten nicht „Benutzer“. 

 Vermeiden Sie Fehler beim Zugreifen auf Überwachungsprotokolle oder beim Anmelden mithilfe der API. Stellen Sie sicher, dass Ihr Konto zu der Rolle **Sicherheitsleseberechtigter** oder **Berichtsleser** in Ihrem Azure Active Directory-Mandanten gehört.

### <a name="error-application-missing-aad-read-directory-data-permission"></a>Fehler Der Anwendung fehlt die AAD-Berechtigung „Verzeichnisdaten lesen“ 

### <a name="error-application-missing-msgraph-api-read-all-audit-log-data-permission"></a>Fehler Der Anwendung fehlt die Berechtigung „Alle Überwachungsprotokolldaten lesen“ der MSGraph-API

Führen Sie die unter [Voraussetzungen für den Zugriff auf die Azure Active Directory-Berichterstellungs-API](howto-configure-prerequisites-for-reporting-api.md) aufgeführten Schritte aus, um sicherzustellen, dass Ihre Anwendung mit dem richtigen Berechtigungssatz ausgeführt wird. 

## <a name="next-steps"></a>Nächste Schritte

* [Abrufen von Daten per Berichtserstellungs-API von Azure Active Directory mit Zertifikaten](tutorial-access-api-with-certificates.md)
* [Referenz zur Überwachungs-API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) 
* [Referenz zur Anmeldeaktivitätsbericht-API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)
