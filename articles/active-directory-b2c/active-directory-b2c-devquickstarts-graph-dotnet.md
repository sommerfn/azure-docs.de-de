---
title: Verwenden der Graph-API in Azure Active Directory B2C
description: Informationen zum Verwalten von Benutzern in einem Azure AD B2C-Mandanten durch Aufrufen der Azure AD Graph-API und Automatisieren des Vorgangs mithilfe einer Anwendungsidentität.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 2585b47d049047cc191bfc284c4486361917f1ed
ms.sourcegitcommit: 4f3f502447ca8ea9b932b8b7402ce557f21ebe5a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2019
ms.locfileid: "71802062"
---
# <a name="azure-ad-b2c-use-the-azure-ad-graph-api"></a>Azure AD B2C: Verwenden der Azure AD-Graph-API

Azure Active Directory B2C (Azure AD B2C)-Mandanten können Tausende oder Millionen von Benutzern enthalten. Das bedeutet, dass viele allgemeine Aufgaben zur Mandantenverwaltung programmgesteuert durchgeführt werden müssen. Ein gutes Beispiel ist die Benutzerverwaltung.

Unter Umständen müssen Sie einen vorhandenen Benutzerspeicher zu einem B2C-Mandanten migrieren. Sie möchten die Benutzerregistrierung vielleicht auf Ihrer eigenen Seite hosten und Benutzerkonten im Azure AD B2C-Verzeichnis im Hintergrund erstellen. Diese Aufgabenarten setzen die Möglichkeit zum Erstellen, Lesen, Aktualisieren und Löschen von Benutzerkonten voraus. Solche Aufgaben können Sie mit der Azure AD Graph-API ausführen.

Für B2C-Mandanten gibt es zwei primäre Modi für die Kommunikation mit der Graph-API:

* Für **interaktive** Aufgaben mit einmaliger Ausführung sollten Sie beim Ausführen der Aufgaben ein Administratorkonto im B2C-Mandanten verwenden. In diesem Modus muss sich ein Administrator mit Anmeldeinformationen anmelden, bevor er alle Aufrufe der Graph-API ausführen kann.
* Für **automatische**, kontinuierliche Aufgaben sollte eine Art von Dienstkonto verwendet werden, dem Sie die benötigten Rechte zum Ausführen von Verwaltungsaufgaben gewähren. In Azure AD registrieren Sie dazu eine Anwendung und authentifizieren sie bei Azure AD. Dies geschieht mithilfe einer *Anwendungs-ID* , die die [OAuth 2.0-Clientanmeldeinformationen](../active-directory/develop/service-to-service.md)verwendet. In diesem Fall verhält sich die Anwendung beim Aufrufen der Graph-API wie sie selbst und nicht wie ein Benutzer.

In diesem Artikel wird die Ausführung des automatischen Anwendungsfalls erläutert. Sie erstellen das .NET 4.5-Element `B2CGraphClient`, das Benutzervorgänge zum Erstellen, Lesen, Aktualisieren und Löschen (CRUD) ausführt. Der Client verfügt über eine Windows-Befehlszeilenschnittstelle, über die Sie verschiedene Methoden aufrufen können. Der Code ist jedoch so geschrieben, dass er sich auf nicht interaktive, automatisierte Weise verhält.

>[!IMPORTANT]
> Sie **müssen** die [Azure AD Graph-API](../active-directory/develop/active-directory-graph-api-quickstart.md) verwenden, um Benutzer in einem Azure AD B2C-Verzeichnis zu verwalten. Die Azure AD Graph-API unterscheidet sich von der Microsoft Graph-API. Erfahren Sie mehr dazu in dem MSDN-Blogbeitrag [Microsoft Graph und Azure AD Graph](https://blogs.msdn.microsoft.com/aadgraphteam/2016/07/08/microsoft-graph-or-azure-ad-graph/).

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie Anwendungen oder Benutzer erstellen können, benötigen Sie einen Azure AD B2C-Mandanten. Wenn Sie noch keinen Mandanten haben, [erstellen Sie einen Azure Active Directory B2C-Mandanten](tutorial-create-tenant.md).

## <a name="register-an-application"></a>Registrieren einer Anwendung

Sobald Sie über einen Azure AD B2C-Mandanten verfügen, müssen Sie Ihre Verwaltungsanwendung über das [Azure-Portal](https://portal.azure.com) registrieren. Außerdem müssen Sie ihr die Berechtigungen erteilen, die für die Ausführung von Verwaltungsaufgaben im Auftrag des automatisierten Skripts oder der Verwaltungsanwendung erforderlich sind.

### <a name="register-application-in-azure-active-directory"></a>Registrieren einer Anwendung in Azure Active Directory

Um die Azure AD-Graph-API mit Ihrem B2C-Mandanten zu verwenden, müssen Sie eine Anwendung mit dem Azure Active Directory-Workflow zur App-Registrierungen registrieren.

[!INCLUDE [active-directory-b2c-appreg-mgmt](../../includes/active-directory-b2c-appreg-mgmt.md)]

### <a name="assign-api-access-permissions"></a>Zuweisen von API-Zugriffsberechtigungen

[!INCLUDE [active-directory-b2c-permissions-directory](../../includes/active-directory-b2c-permissions-directory.md)]

### <a name="create-client-secret"></a>Erstellen eines geheimen Clientschlüssels

[!INCLUDE [active-directory-b2c-client-secret](../../includes/active-directory-b2c-client-secret.md)]

Sie verfügen jetzt über eine Anwendung mit der Berechtigung zum *Erstellen*, *Lesen* und *Aktualisieren* von Benutzern in Ihrem Azure AD B2C-Mandanten. Fahren Sie mit dem nächsten Abschnitt fort, um die Berechtigungen *Benutzer löschen* und *Kennwort aktualisieren* hinzuzufügen.

## <a name="add-user-delete-and-password-update-permissions"></a>Hinzufügen der Berechtigungen zum Löschen von Benutzern und zum Aktualisieren von Kennwörtern

Die Berechtigung *Lese- und Schreibzugriff auf Verzeichnisdaten*, die Sie zuvor erteilt haben, beinhaltet **NICHT** die Möglichkeit, Benutzer zu löschen oder Benutzerkennwörter zu ändern.

Wenn Sie der Anwendung die Möglichkeit erteilen möchten, Benutzer zu löschen oder Kennwörter zu aktualisieren, müssen Sie Ihr die Rolle *Benutzeradministrator* zuweisen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und wechseln Sie zu dem Verzeichnis, das Ihren Azure AD B2C-Mandanten enthält.
1. Wählen Sie im linken Menü den Eintrag **Azure AD B2C** aus. Oder wählen Sie **Alle Dienste** aus, suchen Sie nach dem Eintrag **Azure AD B2C**, und wählen Sie ihn aus.
1. Wählen Sie unter **Verwalten** den Eintrag **Rollen und Administratoren** aus.
1. Wählen Sie die Rolle **Benutzeradministrator** aus.
1. Wählen Sie **Zuweisung hinzufügen** aus.
1. Geben Sie im Textfeld **Auswählen** den Namen der zuvor registrierten Anwendung ein, z. B. *managementapp1*. Wählen Sie Ihre Anwendung aus, wenn sie in den Suchergebnissen angezeigt wird.
1. Wählen Sie **Hinzufügen**. Es kann einige Minuten dauern, bis die Berechtigungen vollständig verteilt sind.

Ihre Azure AD B2C-Anwendung verfügt jetzt über die zusätzlichen Berechtigungen, die zum Löschen von Benutzern oder zum Aktualisieren ihrer Kennwörter in Ihrem Azure AD B2C-Mandanten erforderlich sind.

## <a name="get-the-sample-code"></a>Laden Sie den Beispielcode herunter

Das Codebeispiel ist eine .NET-Konsolenanwendung, die für die Interaktion mit der Azure AD Graph-API die [Active Directory Authentication Library (ADAL) ](../active-directory/develop/active-directory-authentication-libraries.md) verwendet. Der Code veranschaulicht, wie die API aufgerufen wird, um Benutzer in einem Azure AD B2C-Mandanten programmgesteuert zu verwalten.

Sie können das [Beispielarchiv herunterladen](https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet/archive/master.zip) (\*.zip) oder das GitHub-Repository klonen:

```cmd
git clone https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet.git
```

Nachdem Sie das Codebeispiel abgerufen haben, konfigurieren Sie es für Ihre Umgebung, und erstellen Sie dann das Projekt:

1. Öffnen Sie die Projektmappe `B2CGraphClient\B2CGraphClient.sln` in Visual Studio.
1. Öffnen Sie im Projekt **B2CGraphClient** die Datei *App.config*.
1. Ersetzen Sie den Abschnitt `<appSettings>` durch den folgenden XML-Code. Ersetzen Sie dann `{your-b2c-tenant}` durch den Namen Ihres Mandanten sowie `{Application ID}` und `{Client secret}` durch die Werte, die Sie zuvor notiert haben.

    ```xml
    <appSettings>
        <add key="b2c:Tenant" value="{your-b2c-tenant}.onmicrosoft.com" />
        <add key="b2c:ClientId" value="{Application ID}" />
        <add key="b2c:ClientSecret" value="{Client secret}" />
    </appSettings>
    ```

1. Erstellen Sie die Projektmappe. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf die Projektmappe **B2CGraphClient**, und wählen Sie dann **Projektmappe neu erstellen** aus.

Wenn der Build erfolgreich ist, befindet sich die Konsolenanwendung `B2C.exe` in `B2CGraphClient\bin\Debug`.

## <a name="review-the-sample-code"></a>Überprüfen des Beispielcodes

Öffnen Sie zum Verwenden von B2CGraphClient eine Befehlseingabeaufforderung (`cmd.exe`), und wechseln Sie in das Verzeichnis `Debug` des Projekts. Führen Sie anschließend den Befehl `B2C Help` aus.

```cmd
cd B2CGraphClient\bin\Debug
B2C Help
```

Der Befehl `B2C Help` zeigt eine kurze Beschreibung der verfügbaren Unterbefehle an. Jedes Mal, wenn Sie einen der Unterbefehle aufrufen, sendet `B2CGraphClient` eine Anforderung an die Azure AD Graph-API.

In den folgenden Abschnitten wird erläutert, wie der Code der Anwendung die Azure AD Graph-API aufruft.

### <a name="get-an-access-token"></a>Abrufen eines Zugriffstokens

Alle Anforderungen an die Azure AD Graph-API erfordern ein Zugriffstoken für die Authentifizierung. `B2CGraphClient` verwendet Open-Source-ADAL (Active Directory Authentication Library) für das Abrufen von Zugriffstoken. ADAL vereinfacht die Tokenbeschaffung, indem es eine Hilfs-API bereitstellt wird und für einige wichtige Details wie das Zwischenspeichern der Zugriffstoken sorgt. Sie müssen ADAL jedoch nicht zum Abrufen von Token verwenden. Sie können Token stattdessen durch manuelles Erstellen von HTTP-Anforderungen abrufen.

> [!NOTE]
> Sie müssen ADAL v2 oder höher nutzen, um Zugriffstoken zu erhalten, die mit der Azure AD Graph-API verwendet werden können. ADAL v1 kann nicht verwendet werden.

Bei der Ausführung von `B2CGraphClient` wird eine Instanz der `B2CGraphClient`-Klasse erstellt. Mit dem Konstruktor für diese Klasse wird das Authentifizierungsgerüst für ADAL eingerichtet:

```csharp
public B2CGraphClient(string clientId, string clientSecret, string tenant)
{
    // The client_id, client_secret, and tenant are provided in Program.cs, which pulls the values from App.config
    this.clientId = clientId;
    this.clientSecret = clientSecret;
    this.tenant = tenant;

    // The AuthenticationContext is ADAL's primary class, in which you indicate the tenant to use.
    this.authContext = new AuthenticationContext("https://login.microsoftonline.com/" + tenant);

    // The ClientCredential is where you pass in your client_id and client_secret, which are
    // provided to Azure AD in order to receive an access_token by using the app's identity.
    this.credential = new ClientCredential(clientId, clientSecret);
}
```

Verwenden Sie als Beispiel den Befehl `B2C Get-User`.

Wenn `B2C Get-User` ohne zusätzliche Argumente aufgerufen wird, ruft die Anwendung die `B2CGraphClient.GetAllUsers()`-Methode auf. `GetAllUsers()` ruft dann `B2CGraphClient.SendGraphGetRequest()` auf, wodurch eine HTTP-GET-Anforderung an die Azure AD Graph-API gesendet wird. Bevor `B2CGraphClient.SendGraphGetRequest()` die GET-Anforderung sendet, wird zuerst über ADAL ein Zugriffstoken abgerufen:

```csharp
public async Task<string> SendGraphGetRequest(string api, string query)
{
    // First, use ADAL to acquire a token by using the app's identity (the credential)
    // The first parameter is the resource we want an access_token for; in this case, the Graph API.
    AuthenticationResult result = authContext.AcquireToken("https://graph.windows.net", credential);
    ...
```

Sie können ein Zugriffstoken für die Graph-API abrufen, indem Sie die `AuthenticationContext.AcquireToken()` -Methode von ADAL aufrufen. ADAL gibt anschließend ein `access_token` zurück, das die Identität der Anwendung darstellt.

### <a name="read-users"></a>Lesen von Benutzern

Wenn Sie eine Liste mit Benutzern oder einen bestimmten Benutzer von der Azure AD Graph-API abrufen möchten, können Sie eine HTTP-`GET`-Anforderung an den Endpunkt `/users` senden. Eine Anforderung für alle Benutzer in einem Mandanten sieht folgendermaßen aus:

```HTTP
GET https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

Um diese Anforderung anzuzeigen, führen Sie Folgendes aus:

 ```cmd
 B2C Get-User
 ```

Hierbei sind zwei wichtige Punkte zu beachten:

* Das über ADAL abgerufene Zugriffstoken wird mithilfe des Schemas `Bearer` dem Header `Authorization` hinzugefügt.
* Für B2C-Mandanten müssen Sie den Abfrageparameter `api-version=1.6` verwenden.

Diese Details werden beide in der `B2CGraphClient.SendGraphGetRequest()` -Methode behandelt:

```csharp
public async Task<string> SendGraphGetRequest(string api, string query)
{
    ...

    // For B2C user management, be sure to use the 1.6 Graph API version.
    HttpClient http = new HttpClient();
    string url = "https://graph.windows.net/" + tenant + api + "?" + "api-version=1.6";
    if (!string.IsNullOrEmpty(query))
    {
        url += "&" + query;
    }

    // Append the access token for the Graph API to the Authorization header of the request by using the Bearer scheme.
    HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, url);
    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
    HttpResponseMessage response = await http.SendAsync(request);

    ...
```

### <a name="create-consumer-user-accounts"></a>Erstellen von Benutzerkonten für Consumer

Beim Erstellen von Benutzerkonten in Ihrem B2C-Mandanten können Sie eine HTTP-`POST`-Anforderung an den Endpunkt `/users` senden. Die folgende HTTP-`POST`-Anforderung enthält einen Beispielbenutzer, der im Mandanten erstellt werden soll.

Die meisten Eigenschaften in der folgenden Anforderung sind zum Erstellen von Consumerbenutzern erforderlich. Die `//`-Kommentare wurden zur Veranschaulichung eingefügt. Verwenden Sie diese nicht in einer tatsächlichen Anforderung.

```HTTP
POST https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 338

{
    // All of these properties are required to create consumer users.

    "accountEnabled": true,
    "signInNames": [                           // controls which identifier the user uses to sign in to the account
        {
            "type": "emailAddress",            // can be 'emailAddress' or 'userName'
            "value": "consumer@fabrikam.com"
        }
    ],
    "creationType": "LocalAccount",            // always set to 'LocalAccount'
    "displayName": "Consumer User",            // a value that can be used for displaying to the end user
    "mailNickname": "cuser",                   // an email alias for the user
    "passwordProfile": {
        "password": "P@ssword!",
        "forceChangePasswordNextLogin": false  // always set to false
    },
    "passwordPolicies": "DisablePasswordExpiration"
}
```

Führen Sie zum Anzeigen der Anforderung einen der folgenden Befehle aus:

```cmd
B2C Create-User ..\..\..\usertemplate-email.json
B2C Create-User ..\..\..\usertemplate-username.json
```

Der `Create-User`-Befehl verwendet als Eingabeparameter eine JSON-Datei, die eine JSON-Darstellung eines Benutzerobjekts enthält. Der Beispielcode enthält zwei JSON-Beispieldateien: `usertemplate-email.json` und `usertemplate-username.json`. Sie können diese Dateien Ihren Anforderungen entsprechend ändern. Zusätzlich zu den oben genannten erforderlichen Feldern enthalten diese Dateien auch verschiedene optionale Felder.

Weitere Informationen zu erforderlichen und optionalen Feldern finden Sie unter [Referenz für Entitäten und komplexe Typen](/previous-versions/azure/ad/graph/api/entity-and-complex-type-reference) im Graph-API-Referenzhandbuch.

Sie können sehen, wie die POST-Anforderung in `B2CGraphClient.SendGraphPostRequest()` aufgebaut ist:

* An den Header `Authorization` der Anforderung wird ein Zugriffstoken angefügt.
* `api-version=1.6`wird festgelegt.
* Das JSON-Benutzerobjekt wird in den Hauptteil der Anforderung eingefügt.

> [!NOTE]
> Wenn die Konten, die Sie aus einem vorhandenen Benutzerspeicher migrieren möchten, über eine geringere Kennwortsicherheit als die [von Azure AD B2C erzwungene hohe Kennwortsicherheit](active-directory-b2c-reference-password-complexity.md) verfügen, können Sie die Erzwingung sicherer Kennwörter deaktivieren. Verwenden Sie hierfür den Wert `DisableStrongPassword` in der `passwordPolicies`-Eigenschaft. Beispielsweise können Sie die vorherige Anforderung zum Erstellen eines Benutzers wie folgt ändern: `"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"`.

### <a name="update-consumer-user-accounts"></a>Aktualisieren von Benutzerkonten für Consumer

Die Aktualisierung von Benutzerobjekten ähnelt dem Prozess zum Erstellen von Benutzerobjekten, verwendet jedoch die HTTP-`PATCH`-Methode:

```HTTP
PATCH https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 37

{
    "displayName": "Joe Consumer"    // this request updates only the user's displayName
}
```

Versuchen Sie, einen Benutzer zu aktualisieren, indem Sie einige Werte in Ihren JSON-Dateien ändern, und verwenden Sie dann `B2CGraphClient`, um einen der folgenden Befehle auszuführen:

```cmd
B2C Update-User <user-object-id> ..\..\..\usertemplate-email.json
B2C Update-User <user-object-id> ..\..\..\usertemplate-username.json
```

Überprüfen Sie die `B2CGraphClient.SendGraphPatchRequest()` -Methode auf Details zum Senden dieser Anforderung.

### <a name="search-users"></a>Suchen nach Benutzern

Sie haben zwei Möglichkeiten, um in Ihrem B2C-Mandanten nach Benutzern zu suchen:

* Verweisen Sie auf die **Objekt-ID** des Benutzers.
* Verweisen Sie auf die Anmeldekennung (die Eigenschaft `signInNames`).

Führen Sie einen der folgenden Befehle aus, um nach einem Benutzer zu suchen:

```cmd
B2C Get-User <user-object-id>
B2C Get-User <filter-query-expression>
```

Beispiel:

```cmd
B2C Get-User 2bcf1067-90b6-4253-9991-7f16449c2d91
B2C Get-User $filter=signInNames/any(x:x/value%20eq%20%27consumer@fabrikam.com%27)
```

### <a name="delete-users"></a>Löschen von Benutzern

Verwenden Sie zum Löschen von Benutzern die HTTP-`DELETE`-Methode, und erstellen Sie die URL mit der Objekt-ID des Benutzers:

```HTTP
DELETE https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

Verwenden Sie zum Anzeigen eines Beispiels den folgenden Befehl, und sehen Sie sich die Löschanforderung an, die in der Konsole ausgegeben wird:

```cmd
B2C Delete-User <object-id-of-user>
```

Überprüfen Sie die `B2CGraphClient.SendGraphDeleteRequest()` -Methode auf Details zum Senden dieser Anforderung.

Sie können neben der Benutzerverwaltung zahlreiche weitere Aktionen mit der Azure AD Graph-API ausführen. Die [Referenz zur Azure AD Graph-API](/previous-versions/azure/ad/graph/api/api-catalog) enthält Details zu jeder Aktion und Beispielanforderungen.

## <a name="use-custom-attributes"></a>Verwenden von benutzerdefinierten Attributen

In den meisten Consumeranwendungen müssen bestimmte benutzerdefinierte Informationen zu einem Benutzerprofil gespeichert werden. Eine Möglichkeit dazu besteht im Definieren eines benutzerdefinierten Attributs in Ihrem B2C-Mandanten. Sie können dann dieses Attribut wie jede andere Eigenschaft eines Benutzerobjekts behandeln. Sie können das Attribut aktualisieren und löschen, nach dem Attribut abfragen, es in Anmeldetoken als Anspruch senden usw.

Informationen zum Definieren eines benutzerdefinierten Attributs in Ihrem B2C-Mandanten finden Sie unter [Azure Active Directory B2C-Vorschau: Verwenden benutzerdefinierter Attribute zum Erfassen von Informationen über Ihre Kunden](active-directory-b2c-reference-custom-attr.md).

Sie können die benutzerdefinierten Attribute, die in Ihrem B2C-Mandanten definiert sind, mit den folgenden `B2CGraphClient`-Befehlen anzeigen:

```cmd
B2C Get-B2C-Application
B2C Get-Extension-Attribute <object-id-in-the-output-of-the-above-command>
```

Die Ausgabe enthält die Details der einzelnen benutzerdefinierten Attribute. Beispiel:

```json
{
      "odata.type": "Microsoft.DirectoryServices.ExtensionProperty",
      "objectType": "ExtensionProperty",
      "objectId": "cec6391b-204d-42fe-8f7c-89c2b1964fca",
      "deletionTimestamp": null,
      "appDisplayName": "",
      "name": "extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number",
      "dataType": "Integer",
      "isSyncedFromOnPremises": false,
      "targetObjects": [
        "User"
      ]
}
```

Sie können den vollständigen Namen, z.B. `extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number`, als Eigenschaft in Ihren Benutzerobjekten verwenden. Aktualisieren Sie die JSON-Datei mit der neuen Eigenschaft und einem Wert für die Eigenschaft, und führen Sie dann Folgendes aus:

```cmd
B2C Update-User <object-id-of-user> <path-to-json-file>
```

## <a name="next-steps"></a>Nächste Schritte

Mit `B2CGraphClient`verfügen Sie über eine Dienstanwendung, mit der Ihre B2C-Mandantenbenutzer programmgesteuert verwaltet werden können. `B2CGraphClient` nutzt die eigene Anwendungsidentität für die Authentifizierung gegenüber der Azure AD Graph-API. Darüber hinaus werden Token mithilfe eines geheimen Clientschlüssels abgerufen.

Beachten Sie beim Einbinden dieser Funktionalität in Ihre eigene Anwendung einige wichtige Punkte für B2C-Anwendungen:

* Gewähren Sie der Anwendung die erforderlichen Berechtigungen im Mandanten.
* Derzeit müssen Sie zum Abrufen von Zugriffstoken ADAL (nicht MSAL) verwenden. (Sie können Protokollnachrichten auch direkt ohne Verwendung einer Bibliothek senden.)
* Verwenden Sie beim Aufrufen der Graph-API `api-version=1.6`.
* Beim Erstellen und Aktualisieren von Consumerbenutzern sind wie oben beschrieben einige Eigenschaften erforderlich.
