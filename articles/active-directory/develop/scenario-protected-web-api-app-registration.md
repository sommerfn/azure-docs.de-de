---
title: Geschützte Web-API – App-Registrierung | Azure
description: Erfahren Sie, wie Sie eine geschützte Web-API erstellen und die Informationen, die Sie zur Registrierung der App benötigen.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: e4622cffedc159ce85166eafe571ccb26c2c1b4d
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2019
ms.locfileid: "67536853"
---
# <a name="protected-web-api-app-registration"></a>Geschützte Web-API: App-Registrierung

Dieser Artikel erklärt die Besonderheiten der App-Registrierung für eine geschützte Web-API.

Weitere Informationen finden Sie unter [Schnellstart: Registrieren Sie eine Anwendung bei der Microsoft Identitätsplattform ](quickstart-register-app.md)für die allgemeinen Schritte zur Registrierung einer App.

## <a name="accepted-token-version"></a>Akzeptierte Tokenversion

Der Microsoft Identity Platform-Endpunkt kann zwei Arten von Token ausgeben: v1.0-Token und v2.0-Token. Weitere Informationen zu diesen Token finden Sie unter [Zugriff auf Token](access-tokens.md). Die akzeptierte Tokenversion hängt von den **unterstützten Kontotypen** ab, die Sie bei Erstellung Ihrer Anwendung ausgewählt haben:

- Wenn der Wert von **Unterstützte Kontotypen** auf **Konten in allen Organisationsverzeichnissen und persönliche Microsoft-Konten (z.B. Skype, Xbox, Outlook.com)** festgelegt ist, wird v2.0 als Tokenversion akzeptiert.
- Andernfalls wird v1.0 als Tokenversion akzeptiert.

Nachdem Sie die Anwendung erstellt haben, können Sie die akzeptierte Token-Version ermitteln oder ändern, indem Sie diese Schritte befolgen:

1. Wählen Sie Ihre App im Azure-Portal aus, und wählen Sie dann das **Manifest** für Ihre App aus.
2. Suchen Sie im Manifest nach **"accessTokenAcceptedVersion"** . Beachten Sie, dass sein Wert **2** ist. Diese Eigenschaft legt für Azure Active Directory (Azure AD) fest, dass die Web-API v2.0-Token akzeptiert. Wenn der Wert **Null** ist, ist die akzeptierte Token-Version v1.0.
3. Wenn Sie die Token-Version geändert haben, wählen Sie **Speichern**.

> [!NOTE]
> Die Web-API gibt an, welche Token-Version (v1.0 oder v2.0) sie akzeptiert. Wenn Clients einen Token für Ihre Web-API von der Microsoft Identitätsplattform v2.0 Endpunkt anfordern, erhalten sie einen Token, der angibt, welche Version von der Web-API akzeptiert wird.

## <a name="no-redirect-uri"></a>Kein Umleitungs-URI

Web-APIs müssen keine Redirect-URIs registrieren, da kein Benutzer interaktiv angemeldet ist.

## <a name="expose-an-api"></a>Verfügbarmachen einer API

Weitere für Web-APIs spezifische Einstellungen sind die verfügbar gemachte API und die verfügbar gemachten Bereiche.

### <a name="resource-uri-and-scopes"></a>Ressourcen-URI und Bereiche

Bereiche liegen in der Regel im Format `resourceURI/scopeName` vor. Für Microsoft Graph haben die Bereiche Abkürzungen wie `User.Read`. Diese Zeichenkette ist eine Abkürzung für `https://graph.microsoft.com/user.read`.

Bei der Registrierung der App müssen Sie diese Parameter definieren:

- Ressourcen-URI. Standardmäßig empfiehlt das Portal für die Anwendungsregistrierung, dass Sie `api://{clientId}` verwenden. Dieser Ressourcen-URI ist eindeutig, aber nicht lesbar. Sie können ihn ändern, aber stellen Sie sicher, dass der neue Wert eindeutig ist.
- Eine oder mehrere *Bereiche*. (Client-Anwendungen werden als *delegierte Berechtigungen* für Ihre Web-API angezeigt.)
- Eine oder mehrere *App-Rollen*. (Client-Anwendungen werden als *delegierte Berechtigungen* für Ihre Web-API angezeigt.)

Die Bereiche werden auch auf dem Zustimmungsbildschirm angezeigt, der den Endbenutzern Ihrer App angezeigt wird. Daher müssen Sie die entsprechenden Zeichenketten angeben, die den Bereich beschreiben:

- Wie er den Endbenutzern angezeigt wird.
- Wie er dem Mandantenadministrator angezeigt wird, der die Administratoreinwilligung erteilen kann.

### <a name="exposing-delegated-permissions-scopes"></a>Bereiche und delegierte Berechtigungen verfügbar machen (Bereiche)

1. Wählen Sie in der Anwendungsregistrierung den Abschnitt **Ein API freigeben**.
1. Wählen Sie **Bereich hinzufügen**.
1. Wenn Sie dazu aufgefordert werden, akzeptieren Sie die vorgeschlagene URI der Anwendungs-ID (`api://{clientId}`), indem Sie **Speichern und Fortfahren** wählen.
1. Geben Sie diese Parameter ein:
      - Für **Bereichsname**, verwenden Sie **Access_as_user**.
      - Für **Zustimmung für Person**, stellen Sie sicher, dass **Admins und Benutzer** ausgewählt ist.
      - Geben Sie in **Administratorzustimmung Anzeigename** **Access TodoListService als Benutzer** ein.
      - Geben Sie in der **Administratorzustimmung-Beschreibung** **Accesses the TodoListService Web API als Benutzer ein**.
      - Geben Sie in **Anzeigename der Benutzereinwilligung** den **Access TodoListService als Benutzer ein**.
      - Geben Sie in der **Beschreibung der Benutzereinwilligung** **Accesses the TodoListService Web API als Benutzer ein**.
      - Lassen Sie **Zustand** auf **Aktiviert** festgelegt.
      - Wählen Sie **Bereich hinzufügen** aus.

### <a name="if-your-web-api-is-called-by-a-daemon-app"></a>Wenn Ihre Web-API von einer Daemon-App aufgerufen wird

In diesem Abschnitt erfahren Sie, wie Sie Ihre geschützte Web-API registrieren, damit sie von Daemon-Anwendungen sicher aufgerufen werden kann.

- Sie müssen *Anwendungsberechtigungen* freigeben. Sie werden nur Anwendungsberechtigungen deklarieren, da Daemon-Apps nicht mit Benutzern interagieren, so dass delegierte Berechtigungen keinen Sinn ergeben würden.
- Mandantenadministratoren können von Azure Active Directory (Azure AD) verlangen, dass sie Token für Ihre Web-API nur an Anwendungen ausgeben, die sich für den Zugriff auf eine der Anwendungsberechtigungen der Web-API registriert haben.

#### <a name="exposing-application-permissions-app-roles"></a>Freigeben von Anwendungsberechtigungen (App-Rollen)

Um Anwendungsberechtigungen freizugeben, müssen Sie das Manifest bearbeiten.

1. Wählen Sie in der Anwendungsregistrierung für Ihre Anwendung **Manifest**.
1. Bearbeiten Sie das Manifest, indem Sie die `appRoles`-Einstellung suchen und eine oder mehrere Anwendungsrollen hinzufügen. Die Rollendefinition ist im folgenden Beispiel eines JSON-Blocks enthalten. Lassen Sie `allowedMemberTypes` auf `"Application"` eingestellt. Stellen Sie sicher, dass es sich bei `id` um eine eindeutige GUID handelt und dass `displayName` und `value` keine Leerzeichen enthalten.
1. Speichern Sie das Manifest.

Das folgende Beispiel zeigt den Inhalt von `appRoles`. (`id` kann eine beliebige eindeutige GUID sein.)

```JSon
"appRoles": [
    {
    "allowedMemberTypes": [ "Application" ],
    "description": "Accesses the TodoListService-Cert as an application.",
    "displayName": "access_as_application",
    "id": "ccf784a6-fd0c-45f2-9c08-2f9d162a0628",
    "isEnabled": true,
    "lang": null,
    "origin": "Application",
    "value": "access_as_application"
    }
],
```

#### <a name="ensuring-that-azure-ad-issues-tokens-for-your-web-api-to-only-allowed-clients"></a>Sicherstellen, dass Azure AD Token für Ihre Web-API nur an zugelassene Clients ausgibt

Die Web-API überprüft die App-Rolle. (Das ist die Methode des Entwicklers, um Anwendungsrechte freizugeben.) Sie können Azure AD aber auch so konfigurieren, dass ein Token für Ihre Web-API nur für Anwendungen ausgegeben wird, die vom Mandantenadministrator für den Zugriff auf Ihre API genehmigt wurden. Um diese erhöhte Sicherheit hinzuzufügen:

1. Wählen Sie auf der Seite App-**Übersicht** für Ihre App-Registrierung den Link mit dem Namen Ihrer App unter **Verwaltete Anwendung im lokalen Verzeichnis**. Der Titel für dieses Feld kann abgeschnitten werden. Sie können z.B. **verwaltete Anwendung in ...**  sehen

   > [!NOTE]
   >
   > Wenn Sie auf diesen Link klicken, gelangen Sie zur Seite **Enterprise Application Overview**, die mit dem Service-Prinzipal für Ihre Anwendung in dem Mandanten verknüpft ist, in dem Sie sie erstellt haben. Sie können über die Zurück-Schaltfläche Ihres Browsers zurück zur Registrierungsseite der App navigieren.

1. Wählen Sie die Seite **Properties** im Abschnitt **Manage** der Enterprise Anwendungsseiten.
1. Wenn Sie möchten, dass Azure AD den Zugriff auf Ihre Web-API nur von bestimmten Clients aus zulässt, stellen Sie die **Benutzerzuordnung erforderlich?** auf **Ja**.

   > [!IMPORTANT]
   >
   > Wenn Sie die **Benutzerzuordnung erforderlich?** auf **Ja** setzen, überprüft Azure AD die Zuordnungen der App-Rollen von Clients, wenn sie ein Zugriffstoken für die Web-API anfordern. Wenn der Client keinen App-Rollen zugeordnet ist, gibt Azure AD den Fehler zurück`invalid_client: AADSTS501051: Application <application name> is not assigned to a role for the <web API>`.
   >
   > Wenn Sie die **Benutzerzuordnung beibehalten?** auf **Nein** setzen, überprüft *Azure AD die Zuordnungen der App-Rollen nicht, wenn ein Client ein Zugriffstoken für Ihre Web-API anfordert*. Jeder Daemon-Client (d.h. jeder Client, der den Ablauf der Client-Anmeldeinformationen verwendet) kann ein Zugriffstoken für die API erhalten, indem er seine Zielgruppe angibt. Jede Anwendung kann auf die API zugreifen, ohne dafür Berechtigungen anfordern zu müssen. Ihre Web-API kann jedoch, wie im vorherigen Abschnitt erläutert, immer überprüfen, ob die Anwendung die richtige Rolle hat (die vom Mieteradministrator autorisiert ist). Die API führt diese Überprüfung durch, indem sie überprüft, ob das Zugriffstoken einen Rollenanspruch hat und ob der Wert für diesen Anspruch korrekt ist. (In unserem Fall ist der Wert `access_as_application`.)

1. Wählen Sie **Speichern** aus.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Codekonfiguration der App](scenario-protected-web-api-app-configuration.md)
