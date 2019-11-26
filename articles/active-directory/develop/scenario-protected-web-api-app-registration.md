---
title: Geschützte Web-API – App-Registrierung
titleSuffix: Microsoft identity platform
description: Erfahren Sie, wie Sie eine geschützte Web-API erstellen und die App registrieren.
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
ms.openlocfilehash: 0c905dfd86fd80b9e55aa7bd5a9b9b03f277570c
ms.sourcegitcommit: be8e2e0a3eb2ad49ed5b996461d4bff7cba8a837
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72802546"
---
# <a name="protected-web-api-app-registration"></a>Geschützte Web-API: App-Registrierung

Dieser Artikel erläutert die Besonderheiten der App-Registrierung für eine geschützte Web-API.

Weitere Informationen finden Sie unter [Schnellstart: Registrieren einer Anwendung bei Microsoft Identity Platform](quickstart-register-app.md). Dort werden die allgemeinen Schritte zur Registrierung einer App beschrieben.

## <a name="accepted-token-version"></a>Akzeptierte Tokenversion

Der Microsoft Identity Platform-Endpunkt kann zwei Arten von Token ausgeben: v1.0-Token und v2.0-Token. Weitere Informationen zu diesen Token finden Sie unter [Zugriffstoken](access-tokens.md). Die akzeptierte Tokenversion hängt von den **unterstützten Kontotypen** ab, die Sie bei Erstellung Ihrer Anwendung ausgewählt haben:

- Wenn der Wert von **Unterstützte Kontotypen** auf **Konten in allen Organisationsverzeichnissen und persönliche Microsoft-Konten (z.B. Skype, Xbox, Outlook.com)** festgelegt ist, wird v2.0 als Tokenversion akzeptiert.
- Andernfalls wird v1.0 als Tokenversion akzeptiert.

Nachdem Sie die Anwendung erstellt haben, können Sie die akzeptierte Tokenversion ermitteln oder ändern, indem Sie die folgenden Schritte ausführen:

1. Wählen Sie Ihre App im Azure-Portal aus, und wählen Sie dann das **Manifest** für Ihre App aus.
2. Suchen Sie im Manifest nach **"accessTokenAcceptedVersion"** . Beachten Sie, dass der Wert **2** beträgt. Diese Eigenschaft gibt Azure Active Directory (Azure AD) an, dass die Web-API v2.0-Token akzeptiert. Ist der Wert **0** (Null), werden Token der Version 1.0 akzeptiert.
3. Wenn Sie die Tokenversion geändert haben, wählen Sie **Speichern** aus.

> [!NOTE]
> Die Web-API gibt an, welche Tokenversion (v1.0 oder v2.0) akzeptiert wird. Wenn Clients ein Token für Ihre Web-API vom Microsoft Identity Platform-Endpunkt (v2.0) anfordern, erhalten sie ein Token, das angibt, welche Version von der Web-API akzeptiert wird.

## <a name="no-redirect-uri"></a>Kein Umleitungs-URI

Web-APIs müssen keine Umleitungs-URIs registrieren, da kein Benutzer interaktiv angemeldet wird.

## <a name="expose-an-api"></a>Verfügbarmachen einer API

Weitere für Web-APIs spezifische Einstellungen sind die verfügbar gemachte API und die verfügbar gemachten Bereiche.

### <a name="resource-uri-and-scopes"></a>Ressourcen-URI und Bereiche

Bereiche liegen in der Regel im Format `resourceURI/scopeName` vor. Bei Microsoft Graph haben die Bereiche Verknüpfungen wie `User.Read`. Diese Zeichenfolge ist eine Verknüpfung für `https://graph.microsoft.com/user.read`.

Bei der App-Registrierung müssen Sie die folgenden Parameter definieren:

- Ressourcen-URI. Standardmäßig empfiehlt das Portal für die Anwendungsregistrierung die Verwendung von `api://{clientId}`. Dieser Ressourcen-URI ist eindeutig, aber nicht lesbar. Sie können ihn ändern, müssen aber sicherstellen, dass der neue Wert eindeutig ist.
- Ein oder mehrere *Bereiche*. (Bei Client-Anwendungen werden sie als *delegierte Berechtigungen* für Ihre Web-API angezeigt.)
- Eine oder mehrere *App-Rollen*. (Bei Client-Anwendungen werden sie als *Anwendungsberechtigungen* für Ihre Web-API angezeigt.)

Die Bereiche werden auch auf dem Zustimmungsbildschirm angezeigt, der den Endbenutzern Ihrer App angezeigt wird. Daher müssen Sie die entsprechenden Zeichenfolgen angeben, die den Bereich beschreiben:

- Wie er dem Endbenutzer angezeigt wird.
- Wie er dem Mandantenadministrator angezeigt wird, der die Administratoreinwilligung erteilen kann.

### <a name="exposing-delegated-permissions-scopes"></a>Verfügbarmachen delegierter Berechtigungen (Bereiche)

1. Wählen Sie in der Anwendungsregistrierung den Abschnitt **Eine API verfügbar machen** aus.
1. Wählen Sie **Bereich hinzufügen**.
1. Wenn Sie dazu aufgefordert werden, übernehmen Sie den vorgeschlagenen URI für die Anwendungs-ID (`api://{clientId}`), indem Sie **Speichern und Fortfahren** auswählen.
1. Geben Sie die folgenden Parameter ein:
      - Verwenden Sie **access_as_user** als **Bereichsname**.
      - Stellen Sie bei **Zum Einwilligen berechtigte Personen** sicher, dass **Admins und Benutzer** ausgewählt ist.
      - Geben Sie **Access TodoListService as a user** als **Anzeigename der Administratorzustimmung** ein.
      - Geben Sie **Accesses the TodoListService Web API as a user** als **Beschreibung der Administratorzustimmung** ein.
      - Geben Sie **Access TodoListService as a user** als **Anzeigename der Benutzereinwilligung** ein.
      - Geben Sie **Accesses the TodoListService Web API as a user** als **Beschreibung der Benutzereinwilligung** ein.
      - Lassen Sie **Zustand** auf **Aktiviert** festgelegt.
      - Wählen Sie **Bereich hinzufügen** aus.

### <a name="if-your-web-api-is-called-by-a-daemon-app"></a>Aufrufen der Web-API von Daemon-Apps

In diesem Abschnitt erfahren Sie, wie Sie Ihre geschützte Web-API registrieren, damit sie von Daemon-Apps sicher aufgerufen werden kann.

- Sie müssen *Anwendungsberechtigungen* verfügbar machen. Sie deklarieren nur Anwendungsberechtigungen, da Daemon-Apps nicht mit Benutzern interagieren. Daher würden delegierte Berechtigungen keinen Sinn ergeben.
- Mandantenadministratoren können Azure Active Directory (Azure AD) auffordern, Token für Ihre Web-API nur an Anwendungen auszugeben, die für den Zugriff auf eine der Anwendungsberechtigungen der Web-API registriert wurden.

#### <a name="exposing-application-permissions-app-roles"></a>Verfügbarmachen von Anwendungsberechtigungen (App-Rollen)

Zum Verfügbarmachen von Anwendungsberechtigungen müssen Sie das Manifest bearbeiten.

1. Wählen Sie in der Anwendungsregistrierung für Ihre Anwendung **Manifest** aus.
1. Bearbeiten Sie das Manifest, indem Sie nach der Einstellung `appRoles` suchen und eine oder mehrere Anwendungsrollen hinzufügen. Die Rollendefinition ist im folgenden Beispiel eines JSON-Blocks enthalten. Lassen Sie `allowedMemberTypes` auf `"Application"` eingestellt. Stellen Sie sicher, dass es sich bei `id` um eine eindeutige GUID handelt und dass `displayName` und `value` keine Leerzeichen enthalten.
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

#### <a name="ensuring-that-azure-ad-issues-tokens-for-your-web-api-to-only-allowed-clients"></a>Sicherstellen der Tokenausgabe von Azure AD für Ihre Web-API nur an zugelassene Clients

Die Web-API überprüft die App-Rolle. (Das ist die Methode von Entwicklern zum Verfügbarmachen von Anwendungsberechtigungen.) Sie können Azure AD aber auch so konfigurieren, dass ein Token für Ihre Web-API nur an Anwendungen ausgegeben wird, die vom Mandantenadministrator für den Zugriff auf Ihre API genehmigt wurden. So fügen Sie diese erhöhte Sicherheit hinzu

1. Wählen Sie in der App auf der Seite **Übersicht** für Ihre App-Registrierung unter **Verwaltete Anwendung im lokalen Verzeichnis** den Link mit dem Namen Ihrer App aus. Der Titel für dieses Feld ist u.U. abgeschnitten. Möglicherweise wird nur **Verwaltete Anwendung im...**  angezeigt.

   > [!NOTE]
   >
   > Wenn Sie diesen Link auswählen, gelangen Sie zur Seite **Übersicht über Unternehmensanwendungen**, die mit dem Dienstprinzipal für Ihre Anwendung in dem für die Erstellung ausgewählten Mandanten verknüpft ist. Über die Schaltfläche „Zurück“ in Ihrem Browser können Sie wieder zur App-Registrierungsseite navigieren.

1. Wählen Sie im Abschnitt **Verwalten** der Unternehmensanwendungsseiten die Seite **Eigenschaften** aus.
1. Wenn Azure AD den Zugriff auf Ihre Web-API nur von bestimmten Clients aus zulassen soll, stellen Sie **Benutzerzuweisung erforderlich?** auf **Ja** ein.

   > [!IMPORTANT]
   >
   > Wenn Sie **Benutzerzuweisung erforderlich?** auf **Ja** einstellen, überprüft Azure AD die App-Rollenzuweisungen der Clients, wenn sie ein Zugriffstoken für die Web-API anfordern. Wenn dem Client keinen App-Rollen zugewiesen sind, gibt Azure AD den Fehler `invalid_client: AADSTS501051: Application <application name> is not assigned to a role for the <web API>` zurück.
   >
   > Wenn Sie **Benutzerzuweisung erforderlich?** auf **Nein** eingestellt lassen, *überprüft Azure AD die App-Rollenzuweisungen nicht, wenn ein Client ein Zugriffstoken für Ihre Web-API anfordert*. Jeder Daemon-Client (d.h. jeder Client, der den Clientanmeldeinformations-Flow verwendet) kann ein Zugriffstoken für die API erhalten, indem er lediglich seine Zielgruppe angibt. Jede Anwendung kann auf die API zugreifen, ohne dafür Berechtigungen anfordern zu müssen. Ihre Web-API kann jedoch, wie im vorherigen Abschnitt erläutert, immer überprüfen, ob die Anwendung die richtige Rolle (vom Mandantenadministrator autorisiert) hat. Die API führt diese Überprüfung durch, indem sie überprüft, ob das Zugriffstoken einen Rollenanspruch hat und ob der Wert für diesen Anspruch richtig ist. (In unserem Fall ist der Wert `access_as_application`.)

1. Wählen Sie **Speichern** aus.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Codekonfiguration der App](scenario-protected-web-api-app-configuration.md)
