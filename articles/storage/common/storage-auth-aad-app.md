---
title: Authentifizieren des Zugriffs auf Blobs und Warteschlangen mit Azure Active Directory aus einer Clientanwendung – Azure Storage
description: Verwenden Sie Azure Active Directory zum Authentifizieren aus einer Clientanwendung, zum Abrufen eines OAuth 2.0-Tokens und zum Autorisieren von Anforderungen an den Azure-Blob- und -Warteschlangenspeicher.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 07/18/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: ca6b055b5d3702cea4ca1986ad1c81b59f76cee3
ms.sourcegitcommit: 8b44498b922f7d7d34e4de7189b3ad5a9ba1488b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2019
ms.locfileid: "72299639"
---
# <a name="authorize-access-to-blobs-and-queues-with-azure-active-directory-from-a-client-application"></a>Authentifizieren des Zugriffs auf Blobs und Warteschlangen mit Azure Active Directory aus einer Clientanwendung

Ein wesentlicher Vorteil bei der Verwendung von Azure Active Directory (Azure AD) mit dem Azure-Blob- und -Warteschlangenspeicher besteht darin, dass Ihre Anmeldeinformationen nicht mehr im Code gespeichert werden müssen. Stattdessen können Sie ein OAuth 2.0-Zugriffstoken von Microsoft Identity Platform (früher Azure AD) anfordern. Azure AD übernimmt die Authentifizierung des Sicherheitsprinzipals (Benutzer, Gruppe oder Dienstprinzipal), der die Anwendung ausführt. Wenn die Authentifizierung erfolgreich ist, gibt Azure AD das Zugriffstoken an die Anwendung zurück, und die Anwendung kann dann das Zugriffstoken zum Autorisieren von Anforderungen an den Azure-Blob- oder -Warteschlangenspeicher verwenden.

In diesem Artikel wird gezeigt, wie Sie Ihre native Anwendung oder Webanwendung für die Authentifizierung mit Microsoft Identity Platform 2.0 konfigurieren. Im Codebeispiel wird .NET verwendet. Der Ansatz ist bei anderen Sprachen aber ähnlich. Weitere Informationen zu Microsoft Identity Platform 2.0 finden Sie unter [Übersicht über Microsoft Identity Platform (v2.0)](../../active-directory/develop/v2-overview.md).

Eine Übersicht über den Datenfluss für OAuth 2.0-Codeberechtigungen finden Sie unter [Autorisieren des Zugriffs auf Azure Active Directory-Webanwendungen mit dem Flow zum Erteilen des OAuth 2.0-Codes](../../active-directory/develop/v2-oauth2-auth-code-flow.md).

## <a name="assign-a-role-to-an-azure-ad-security-principal"></a>Zuweisen einer Rolle zu einem Azure AD-Sicherheitsprinzipal

Um einen Sicherheitsprinzipal aus Ihrer Azure Storage-Anwendung authentifizieren zu können, müssen Sie zuerst die Einstellungen der rollenbasierten Zugriffskontrolle (Role-Based Access Control, RBAC) für diesen Sicherheitsprinzipal konfigurieren. Azure Storage definiert integrierte RBAC-Rollen, die Berechtigungen für Container und Warteschlangen beinhalten. Wird die RBAC-Rolle einem Sicherheitsprinzipal zugewiesen, wird diesem Sicherheitsprinzipal Zugriff auf die Ressource gewährt. Weitere Informationen finden Sie unter [Verwalten von Zugriffsrechten für Blob- und Warteschlangendaten mit RBAC](storage-auth-aad-rbac.md).

## <a name="register-your-application-with-an-azure-ad-tenant"></a>Registrieren der Anwendung bei einem Azure AD-Mandanten

Der erste Schritt bei der Verwendung von Azure AD zum Autorisieren des Zugriffs auf Speicherressourcen ist die Registrierung Ihrer Clientanwendung mit einem Azure AD-Mandanten im [Azure-Portal](https://portal.azure.com). Wenn Sie Ihre Clientanwendung registrieren, liefern Sie Azure AD Informationen zu Ihrer Anwendung. Azure AD stellt dann eine Client-ID (auch als *Anwendungs-ID* bezeichnet) bereit, mit der Sie Ihre Anwendung zur Laufzeit Azure AD zuordnen. Weitere Informationen zur Client-ID finden Sie unter [Anwendungs- und Dienstprinzipalobjekte in Azure Active Directory (Azure AD)](../../active-directory/develop/app-objects-and-service-principals.md).

Um Ihre Azure Storage-Anwendung zu registrieren, führen Sie die in der folgenden Anleitung gezeigten Schritte aus [Schnellstart: Registrieren einer Anwendung bei der Microsoft Identity Platform](../../active-directory/develop/quickstart-configure-app-access-web-apis.md). Die folgende Abbildung zeigt die allgemeinen Einstellungen zum Registrieren einer Webanwendung:

![Screenshot, der das Registrieren Ihrer Speicheranwendung bei Azure AD zeigt](./media/storage-auth-aad-app/app-registration.png)

> [!NOTE]
> Wenn Sie Ihre Anwendung als native Anwendung registrieren, können Sie jeden gültigen URI als **Umleitungs-URI** angeben. Bei nativen Anwendungen muss dieser Wert keine echte URL sein. Bei Webanwendungen muss der Umleitungs-URI ein gültiger URI sein, da er die URL definiert, für die Token bereitgestellt werden.

Nachdem Sie Ihre Anwendung registriert haben, wird die Anwendungs-ID (oder Client-ID) unter **Einstellungen** angezeigt:

![Screenshot der Client-ID](./media/storage-auth-aad-app/app-registration-client-id.png)

Ausführlichere Informationen zum Registrieren einer Anwendung bei Azure AD finden Sie unter [Integrieren von Anwendungen in Azure Active Directory](../../active-directory/develop/quickstart-v2-register-an-app.md).

## <a name="grant-your-registered-app-permissions-to-azure-storage"></a>Erteilen von Berechtigungen für die registrierte App für Azure Storage

Erteilen Sie als Nächstes Ihrer Anwendung Berechtigungen zum Aufrufen von Azure Storage-APIs. Dieser Schritt ermöglicht Ihrer Anwendung, Anforderungen an Azure Storage mit Azure AD zu autorisieren.

1. Wählen Sie auf der Seite **Übersicht** für Ihre registrierte Anwendung die Option **API-Berechtigungen anzeigen** aus.
1. Wählen Sie im Abschnitt **API-Berechtigungen** die Option **Berechtigung hinzufügen** und dann **Microsoft-APIs** aus.
1. Wählen Sie aus der Liste der Ergebnisse **Azure Storage** aus, um den Bereich **API-Berechtigungen anfordern** anzuzeigen.
1. Beachten Sie, dass unter **What type of permissions does your application require?** (Welche Art von Berechtigungen sind für Ihre Anwendung erforderlich?) als verfügbarer Berechtigungstyp **Delegated permissions** (Delegierte Berechtigungen) angezeigt wird. Diese Option ist standardmäßig für Sie aktiviert.
1. Aktivieren Sie im Bereich **API-Berechtigungen anfordern** im Abschnitt **Berechtigungen auswählen** das Kontrollkästchen neben **user_impersonation**, und klicken Sie dann auf **Berechtigungen hinzufügen**.

    ![Screenshot mit Berechtigungen für den Speicher](media/storage-auth-aad-app/registered-app-permissions-1.png)

Im Bereich **API-Berechtigungen** wird jetzt angezeigt, dass Ihre registrierte Azure AD-Anwendung Zugriff auf Microsoft Graph und Azure Storage hat. Berechtigungen für Microsoft Graph werden automatisch gewährt, wenn Sie Ihre App zum ersten Mal bei Azure AD registrieren.

![Screenshot mit Berechtigungen der registrierten App](media/storage-auth-aad-app/registered-app-permissions-2.png)

## <a name="create-a-client-secret"></a>Erstellen eines Clientgeheimnisses

Die Anwendung benötigt zum Beweis ihrer Identität einen geheimen Clientschlüssel, wenn sie ein Token anfordert. Führen Sie folgende Schritte aus, um den geheimen Clientschlüssel hinzuzufügen:

1. Navigieren Sie im Azure-Portal zu Ihrer App-Registrierung.
1. Klicken Sie auf die Einstellung **Certificates & secrets** (Zertifikate & Geheime Schlüssel).
1. Klicken Sie unter **Client secrets** (Geheime Clientschlüssel) auf **New client secret** (Neuer geheimer Clientschlüssel), um einen neuen geheimen Schlüssel zu erstellen.
1. Geben Sie eine Beschreibung für den geheimen Schlüssel an, und wählen Sie das gewünschte Ablaufintervall aus.
1. Kopieren Sie den Wert des neuen geheimen Schlüssels sofort, und legen Sie die Kopie an einem sicheren Speicherort ab. Der vollständige Wert wird Ihnen nur einmal angezeigt.

    ![Screenshot: geheimer Clientschlüssel](media/storage-auth-aad-app/client-secret.png)

## <a name="client-libraries-for-token-acquisition"></a>Clientbibliotheken für die Tokenbeschaffung

Sobald Sie Ihre Anwendung registriert und ihr Berechtigungen für den Zugriff auf Daten im Azure-Blob- oder -Warteschlangenspeicher erteilt haben, können Sie Ihrer Anwendung Code zum Authentifizieren eines Sicherheitsprinzipals und zum Beschaffen eines OAuth 2.0-Tokens hinzufügen. Zum Authentifizieren und Abrufen des Tokens können Sie entweder eine der [Authentifizierungsbibliotheken von Microsoft Identity Platform](../../active-directory/develop/reference-v2-libraries.md) oder eine andere Open-Source-Bibliothek verwenden, die OpenID Connect 1.0 unterstützt. Ihre Anwendung kann dann mit dem Zugriffstoken eine Anforderung an den Azure-Blob- oder -Warteschlangenspeicher autorisieren.

Eine Liste der Szenarien, für die das Abrufen von Token unterstützt wird, finden Sie im Abschnitt zu [Authentifizierungsflows](/en-us/azure/active-directory/develop/msal-authentication-flows) in der [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview).

## <a name="well-known-values-for-authentication-with-azure-ad"></a>Bekannte Werte für die Authentifizierung mit Azure AD

Um einen Sicherheitsprinzipal bei Azure AD zu authentifizieren, müssen Sie einige bekannte Werte in den Code aufnehmen.

### <a name="azure-ad-authority"></a>Azure AD-Autorität

Für die öffentliche Microsoft-Cloud lautet die Azure AD-Autorität wie unten angegeben. Dabei steht *tenant-id* für Ihre Active Directory-Mandanten-ID (oder Verzeichnis-ID):

`https://login.microsoftonline.com/<tenant-id>/`

Die Mandanten-ID identifiziert den zur Authentifizierung zu verwendenden Azure AD-Mandanten. Er wird auch als Verzeichnis-ID bezeichnet. Navigieren Sie zur Seite **Übersicht** Ihrer App-Registrierung im Azure-Portal, und kopieren Sie den Wert von dort, um die Mandanten-ID abzurufen.

### <a name="azure-storage-resource-id"></a>Azure Storage-Ressourcen-ID

[!INCLUDE [storage-resource-id-include](../../../includes/storage-resource-id-include.md)]

## <a name="net-code-example-create-a-block-blob"></a>Codebeispiel für .NET: Erstellen eines Blockblobs

Das Codebeispiel veranschaulicht, wie Sie ein Zugriffstoken aus Azure AD abrufen. Mit dem Zugriffstoken wird der angegebene Benutzer authentifiziert und anschließend eine Anforderung zum Erstellen eines Blockblobs autorisiert. Damit dieses Beispiel funktioniert, führen Sie zunächst die in den vorherigen Abschnitten beschriebenen Schritte aus.

Sie benötigen die folgenden Werte aus der App-Registrierung, um das Token anzufordern:

- Den Namen Ihrer Azure AD-Domäne. Diesen Wert können Sie über die **Übersicht** Ihrer Azure Active Directory-Instanz abrufen.
- Die ID des Mandanten (oder des Verzeichnisses). Diesen Wert können Sie über die **Übersicht** Ihrer App-Registrierung abrufen.
- Die Client-ID (oder Anwendungs-ID). Diesen Wert können Sie über die **Übersicht** Ihrer App-Registrierung abrufen.
- Der Clientumleitungs-URI. Diesen Wert können Sie über die Einstellungen zur **Authentication** (Authentifizierung) für Ihre App-Registrierung abrufen.
- Den Wert des geheimen Clientschlüssels. Diesen Wert können Sie aus dem Speicherort abrufen, an dem Sie den Wert zuvor abgelegt haben.

### <a name="create-a-storage-account-and-container"></a>Erstellen eines Speicherkontos und Containers

Erstellen Sie mit demselben Abonnement, das Sie auch für Azure Active Directory verwenden, ein Speicherkonto, um das Codebeispiel auszuführen. Erstellen Sie anschließend einen Container innerhalb dieses Speicherkontos. Der Beispielcode wird in diesem Container einen Blockblob erstellen.

Weisen Sie anschließend die Rolle **Storage Blob Data Contributor** (Mitwirkender an Speicherblobdaten) explizit dem Benutzerkonto zu, unter dem Sie den Beispielcode ausführen werden. Informationen zum Zuweisen dieser Rolle im Azure-Portal finden Sie unter [Gewähren von Zugriff auf Azure-Blob- und -Warteschlangendaten mit RBAC über das Azure-Portal](storage-auth-aad-rbac-portal.md).

> [!NOTE]
> Wenn Sie ein Azure Storage-Konto erstellen, erhalten Sie nicht automatisch Berechtigungen für den Zugriff auf Daten über Azure AD. Sie müssen sich selbst explizit eine RBAC-Rolle für Azure Storage zuweisen. Sie können sie auf Ebene Ihres Abonnements, einer Ressourcengruppe, eines Speicherkontos oder eines Containers oder einer Warteschlange zuordnen.

### <a name="create-a-web-application-that-authorizes-access-to-blob-storage-with-azure-ad"></a>Erstellen einer Webanwendung, die Zugriff auf Blob Storage über Azure AD gewährt

Wenn Ihre Anwendung auf Azure Storage zugreift, dann im Namen des Benutzers. Der Zugriff auf Blob- oder Warteschlangenressourcen erfolgt also unter Verwendung der Berechtigungen des angemeldeten Benutzers. Sie benötigen zum Testen dieses Codebeispiels eine Webanwendung, die den Benutzer auffordert, sich mit einer Azure AD-Identität anzumelden. Sie können Ihre eigene oder die von Microsoft bereitgestellte Beispielanwendung verwenden.

Eine vollständige Beispielwebanwendung, die ein Token abruft und zum Erstellen eines Blobs in Azure Storage verwendet, steht unter [GitHub](https://aka.ms/aadstorage) zur Verfügung. Durch Überprüfen und Ausführen des vollständigen Beispiels können Sie die Codebeispiele möglicherweise besser verstehen. Informationen zum Ausführen des vollständigen Beispiels finden Sie im Abschnitt [View and run the completed sample (Anzeigen und Ausführen des vollständigen Beispiels)](#view-and-run-the-completed-sample).

#### <a name="add-references-and-using-statements"></a>Hinzufügen von Verweisen und using-Anweisungen  

Installieren Sie von Visual Studio aus die Azure Storage-Clientbibliothek. Wählen Sie im Menü **Extras** den Eintrag **NuGet-Paket-Manager** und danach **Paket-Manager-Konsole** aus. Geben Sie im Konsolenfenster die folgenden Befehle ein, um die erforderlichen Pakete aus der Azure Storage-Clientbibliothek für .NET zu installieren:

```console
Install-Package Microsoft.Azure.Storage.Blob
Install-Package Microsoft.Azure.Storage.Common
```

Fügen Sie anschließend der Datei „HomeController.cs“ die folgenden using-Anweisungen hinzu:

```csharp
using Microsoft.Identity.Client; //MSAL library for getting the access token
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Blob;
```

#### <a name="create-a-block-blob"></a>Erstellen eines Blockblobs

Fügen Sie den folgenden Codeausschnitt hinzu, um einen Blockblob zu erstellen:

```csharp
private static async Task<string> CreateBlob(string accessToken)
{
    // Create a blob on behalf of the user
    TokenCredential tokenCredential = new TokenCredential(accessToken);
    StorageCredentials storageCredentials = new StorageCredentials(tokenCredential);

    // Replace the URL below with your storage account URL
    CloudBlockBlob blob =
        new CloudBlockBlob(
            new Uri("https://<storage-account>.blob.core.windows.net/<container>/Blob1.txt"),
            storageCredentials);
    await blob.UploadTextAsync("Blob created by Azure AD authenticated user.");
    return "Blob successfully created";
}
```

> [!NOTE]
> Um Blob- und Warteschlangenvorgänge mit einem OAuth 2.0-Token zu autorisieren, müssen Sie HTTPS verwenden.

Im obigen Beispiel wickelt die .NET-Clientbibliothek die Autorisierung der Anforderung für die Erstellung des Blockblobs ab. Die Autorisierung der Anforderung wird auch von Azure Storage-Clientbibliotheken für andere Sprachen für Sie abgewickelt. Wenn Sie jedoch über die REST-API einen Azure Storage-Vorgang mit einem OAuth-Token aufrufen, muss die Anforderung unter Verwendung des OAuth-Tokens autorisiert werden.

Wenn Sie Blob- und Warteschlangendienstvorgänge mit OAuth-Zugriffstoken aufrufen möchten, übergeben Sie das Zugriffstoken im **Autorisierungsheader** unter Verwendung des Schemas **Bearer**, und geben Sie mindestens die Dienstversion 2017-11-09 an, wie im folgenden Beispiel zu sehen:

```https
GET /container/file.txt HTTP/1.1
Host: mystorageaccount.blob.core.windows.net
x-ms-version: 2017-11-09
Authorization: Bearer eyJ0eXAiOnJKV1...Xd6j
```

#### <a name="get-an-oauth-token-from-azure-ad"></a>Abrufen eines OAuth-Tokens von Azure AD

Fügen Sie als Nächstes eine Methode hinzu, die im Namen des Benutzers ein Token von Azure AD anfordert. Diese Methode definiert den Bereich, für den Berechtigungen erteilt werden sollen. Weitere Informationen zu Berechtigungen und Bereichen finden Sie unter [Berechtigungen und Zustimmung im Microsoft Identity Platform-Endpunkt](../../active-directory/develop/v2-permissions-and-consent.md).

Verwenden Sie die Ressourcen-ID, um den Bereich zu erstellen, für den das Token abgerufen werden soll. Im Beispiel wird der Bereich mit der Ressourcen-ID und dem integrierten `user_impersonation`-Bereich erstellt, der angibt, dass das Token im Namen des Benutzers angefordert wird.

Beachten Sie, dass Sie dem Benutzer möglicherweise eine Benutzeroberfläche zur Verfügung stellen müssen, die ihm ermöglicht, die Zustimmung zur Anforderung des Tokens in seinem Namen zu erteilen. Wenn eine Zustimmung erforderlich ist, fängt das Beispiel die **MsalUiRequiredException** ab und ruft eine andere Methode auf, um die Anforderung für die Zustimmung zu vereinfachen:

```csharp
public async Task<IActionResult> Blob()
{
    var scopes = new string[] { "https://storage.azure.com/user_impersonation" };
    try
    {
        var accessToken =
            await _tokenAcquisition.GetAccessTokenOnBehalfOfUser(HttpContext, scopes);
        ViewData["Message"] = await CreateBlob(accessToken);
        return View();
    }
    catch (MsalUiRequiredException ex)
    {
        AuthenticationProperties properties =
            BuildAuthenticationPropertiesForIncrementalConsent(scopes, ex);
        return Challenge(properties);
    }
}
```

Einwilligung ist der Prozess, mit dem ein Benutzer einer Anwendung die Autorisierung für den Zugriff auf geschützte Ressourcen in seinem Auftrag gewährt. Microsoft Identity Platform 2.0 unterstützt die inkrementelle Zustimmung. Das heißt, ein Sicherheitsprinzipal kann zunächst einen Mindestsatz von Berechtigungen anfordern und im Laufe der Zeit und nach Bedarf Berechtigungen hinzufügen. Wenn Ihr Code ein Zugriffstoken anfordert, geben Sie mit dem `scope`-Parameter den Umfang der Berechtigungen an, die Ihre App jederzeit braucht. Weitere Informationen zur inkrementellen Zustimmung finden Sie im Abschnitt **Inkrementelle und dynamische Zustimmung** des Artikels [Gründe für eine Aktualisierung auf die Microsoft Identity Platform (v2.0)](../../active-directory/develop/azure-ad-endpoint-comparison.md#incremental-and-dynamic-consent).

Mit der folgenden Methode werden die Authentifizierungseigenschaften für die Anforderung der inkrementellen Zustimmung erstellt:

```csharp
private AuthenticationProperties BuildAuthenticationPropertiesForIncrementalConsent(string[] scopes,
                                                                                    MsalUiRequiredException ex)
{
    AuthenticationProperties properties = new AuthenticationProperties();

    // Set the scopes, including the scopes that ADAL.NET or MSAL.NET need for the Token cache.
    string[] additionalBuildInScopes = new string[] { "openid", "offline_access", "profile" };
    properties.SetParameter<ICollection<string>>(OpenIdConnectParameterNames.Scope,
                                                 scopes.Union(additionalBuildInScopes).ToList());

    // Attempt to set the login_hint so that the logged-in user is not presented
    // with an account selection dialog.
    string loginHint = HttpContext.User.GetLoginHint();
    if (!string.IsNullOrWhiteSpace(loginHint))
    {
        properties.SetParameter<string>(OpenIdConnectParameterNames.LoginHint, loginHint);

        string domainHint = HttpContext.User.GetDomainHint();
        properties.SetParameter<string>(OpenIdConnectParameterNames.DomainHint, domainHint);
    }

    // Specify any additional claims that are required (for instance, MFA).
    if (!string.IsNullOrEmpty(ex.Claims))
    {
        properties.Items.Add("claims", ex.Claims);
    }

    return properties;
}
```

## <a name="view-and-run-the-completed-sample"></a>Anzeigen und Ausführen des vollständigen Beispiels

Zum Ausführen der Beispielanwendung klonen Sie diese zunächst, oder laden Sie diese aus [GitHub](https://github.com/Azure-Samples/storage-dotnet-azure-ad-msal) herunter. Aktualisieren Sie die Anwendung anschließend wie in den folgenden Abschnitten beschrieben.

### <a name="provide-values-in-the-settings-file"></a>Bereitstellen von Werten in der Einstellungsdatei

Aktualisieren Sie dann die Datei *appsettings.json* wie folgt mit Ihren eigenen Werten:

```json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "Domain": "<azure-ad-domain-name>.onmicrosoft.com",
    "TenantId": "<tenant-id>",
    "ClientId": "<client-id>",
    "CallbackPath": "/signin-oidc",
    "SignedOutCallbackPath ": "/signout-callback-oidc",

    // To call an API
    "ClientSecret": "<client-secret>"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Warning"
    }
  },
  "AllowedHosts": "*"
}
```

### <a name="update-the-storage-account-and-container-name"></a>Aktualisieren des Speicherkonto- und Containernamens

Aktualisieren Sie in der Datei *HomeController.cs* den URI, der auf den Blockblob verweist, damit dieser den Namen Ihres Speicherkontos und Containers verwendet:

```csharp
CloudBlockBlob blob = new CloudBlockBlob(
                      new Uri("https://<storage-account>.blob.core.windows.net/<container>/Blob1.txt"),
                      storageCredentials);
```

### <a name="enable-implicit-grant-flow"></a>Aktivieren des Flows zur impliziten Genehmigung

Sie müssen für Ihre App-Registrierung möglicherweise den Ablauf zur impliziten Genehmigung konfigurieren, um das Beispiel auszuführen. Folgen Sie diesen Schritten:

1. Navigieren Sie im Azure-Portal zu Ihrer App-Registrierung.
1. Wählen Sie im Abschnitt „Manage“ (Verwalten) die Einstellung **Authentication** (Authentifizierung) aus.
1. Aktivieren Sie wie in der folgenden Abbildung dargestellt die Kontrollkästchen unter **Erweiterte Einstellungen** im Bereich **Implicit grant** (Implizite Genehmigung), um Zugriffstoken und ID-Token zu aktivieren:

    ![Screenshot: Aktivieren der Einstellungen für den Flow zur impliziten Genehmigung](media/storage-auth-aad-app/enable-implicit-grant-flow.png)

### <a name="update-the-port-used-by-localhost"></a>Aktualisieren des von localhost verwendeten Ports

Wenn Sie das Beispiel ausführen, müssen Sie möglicherweise den in Ihrer App-Registrierung angegebenen Umleitungs-URI aktualisieren, um den zur Laufzeit zugewiesenen *localhost*-Port zu verwenden. Gehen Sie wie folgt vor, um den Umleitungs-URI so zu aktualisieren, dass dieser den zugewiesenen Port verwendet:

1. Navigieren Sie im Azure-Portal zu Ihrer App-Registrierung.
1. Wählen Sie im Abschnitt „Manage“ (Verwalten) die Einstellung **Authentication** (Authentifizierung) aus.
1. Bearbeiten Sie den Port unter **Umleitungs-URIs** wie in der folgenden Abbildung dargestellt, damit dieser mit dem von der Beispielanwendung verwendeten übereinstimmt:

    ![Screenshot: Umleitungs-URIs für die App-Registrierung](media/storage-auth-aad-app/redirect-uri.png)

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu Microsoft Identity Platform finden Sie unter [Microsoft Identity Platform](https://docs.microsoft.com/azure/active-directory/develop/).
- Weitere Informationen zu RBAC-Rollen für Azure Storage finden Sie unter [Verwalten der Zugriffsrechte für Speicherdaten mit RBAC](storage-auth-aad-rbac.md).
- Informationen zur Verwendung verwalteter Identitäten für Azure-Ressourcen mit Azure Storage finden Sie unter [Authentifizieren des Zugriffs auf Blobs und Warteschlangen mit Azure Active Directory und verwalteten Identitäten für Azure-Ressourcen](storage-auth-aad-msi.md).
