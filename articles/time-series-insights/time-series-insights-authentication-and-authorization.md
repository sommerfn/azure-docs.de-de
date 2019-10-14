---
title: Authentifizieren und Autorisieren mithilfe einer API in Azure Time Series Insights | Microsoft-Dokumentation
description: In diesem Artikel wird die Konfiguration der Authentifizierung und Autorisierung für eine benutzerdefinierte Anwendung erläutert, die die Azure Time Series Insights-API aufruft.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 09/23/2019
ms.custom: seodec18
ms.openlocfilehash: e98c004b802711c83558bf4d7ec86c418679836b
ms.sourcegitcommit: d7689ff43ef1395e61101b718501bab181aca1fa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/06/2019
ms.locfileid: "71981150"
---
# <a name="authentication-and-authorization-for-azure-time-series-insights-api"></a>Authentifizierung und Autorisierung für die Azure Time Series Insights-API

Dieses Dokument beschreibt die Registrierung einer App in Azure Active Directory mit dem neuen Azure Active Directory-Blatt. Apps, die in Azure Active Directory registriert sind, ermöglichen es Benutzern, sich für die Azure Time Series Insights-API in einer Time Series Insights-Umgebung authentifizieren und autorisieren zu lassen.

## <a name="service-principal"></a>Dienstprinzipal

In den folgenden Abschnitten wird erläutert, wie Sie eine Anwendung für den Zugriff auf die Azure Time Series Insights-API für die Anwendung konfigurieren. Die Anwendung kann dann anhand der eigenen Anwendungsanmeldeinformationen über Azure Active Directory in der Time Series Insights-Umgebung Referenzdaten abfragen oder veröffentlichen.

## <a name="summary-and-best-practices"></a>Zusammenfassung und bewährte Methoden

Der Ablauf der Azure Active Directory-App-Registrierung besteht aus drei wichtigen Schritten.

1. [Registrieren einer Anwendung](#azure-active-directory-app-registration) in Azure Active Directory.
1. Autorisieren dieser Anwendung zum [Zugriff auf Daten der Time Series Insights-Umgebung](#granting-data-access).
1. Verwenden der **Anwendungs-ID** und des **Clientgeheimnisses**, um ein Token von `https://api.timeseries.azure.com/` in Ihrer [Client-App](#client-app-initialization) abzurufen. Mit dem Token kann dann die Time Series Insights-API aufgerufen werden.

Wenn Sie in **Schritt 3** die Anmeldeinformationen Ihrer Anwendung und Ihrer Benutzer trennen, ist Folgendes möglich:

* Zuweisen von Berechtigungen zur App-Identität, die sich von Ihren eigenen Berechtigungen unterscheiden. In der Regel sind diese Berechtigungen auf diejenigen beschränkt, die für die App erforderlich sind. Beispielsweise können Sie festlegen, dass die App nur in einer bestimmten Time Series Insights-Umgebung Daten lesen darf.
* Isolieren der Sicherheit der App von den Authentifizierungsinformationen des erstellenden Benutzers, indem Sie ein **Clientgeheimnis** oder ein Sicherheitszertifikat verwenden. Danach sind die Anmeldeinformationen der Anwendung nicht mehr von den Anmeldeinformationen eines bestimmten Benutzers abhängig. Wenn sich die Rolle des Benutzers ändert, erfordert die Anwendung nicht unbedingt neue Anmeldeinformationen oder weitere Konfiguration. Wenn der Benutzer sein Kennwort ändert, müssen für Zugriffe auf die Anwendung keine neuen Anmeldeinformationen oder Schlüssel erstellt werden.
* Ausführen eines unbeaufsichtigten Skripts mit einem **Clientgeheimnis** oder einen Sicherheitszertifikat anstatt mit bestimmten Benutzeranmeldeinformationen (für die der Benutzer anwesend sein muss).
* Verwenden eines Sicherheitszertifikats anstelle eines Kennworts, um den Zugriff auf Ihre Azure Time Series Insights-API zu sichern.

> [!IMPORTANT]
> Halten Sie sich an das Konzept der **Trennung der Belange** (wie für dieses Szenario oben beschrieben), wenn Sie ihre Azure Time Series Insights-Sicherheitsrichtlinien konfigurieren.

> [!NOTE]
> * Der Schwerpunkt des Artikels liegt dabei auf einer Anwendung mit nur einem Mandanten, die zur Ausführung in nur einer einzigen Organisation vorgesehen ist.
> * Sie setzen Anwendungen mit nur einem Mandanten in der Regel für Branchenanwendungen ein, die innerhalb Ihrer Organisation ausgeführt werden.

## <a name="detailed-setup"></a>Setup – detailliert

### <a name="azure-active-directory-app-registration"></a>Azure Active Directory-App-Registrierung

[!INCLUDE [Azure Active Directory app registration](../../includes/time-series-insights-aad-registration.md)]

### <a name="granting-data-access"></a>Gewähren von Datenzugriff

1. Wählen Sie für die Time Series Insights-Umgebung **Datenzugriffsrichtlinien** und dann **Hinzufügen** aus.

   [![Hinzufügen einer neuen Datenzugriffsrichtlinie zur Time Series Insights-Umgebung](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png#lightbox)

1. Fügen Sie im Dialogfeld **Benutzer auswählen** entweder den **Anwendungsnamen** oder die **Anwendungs-ID** aus dem Abschnitt „Azure Active Directory-App-Registrierung“ ein.

   [![Suchen einer Anwendung im Dialogfeld „Benutzer auswählen“](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png#lightbox)

1. Wählen Sie die Rolle aus. Wählen Sie **Leser** aus, um Daten abzufragen oder **Mitwirkender**, um Daten abzufragen und Referenzdaten zu ändern. Klicken Sie auf **OK**.

   [![Auswählen von „Leser“ oder „Mitwirkender“ im Dialogfeld „Benutzerrolle auswählen“](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png#lightbox)

1. Speichern Sie die Richtlinie mit **OK**.

   > [!TIP]
   > Lesen Sie Informationen zum [Gewähren von Datenzugriff](./time-series-insights-data-access.md) auf Ihre Time Series Insights-Umgebung in Azure Active Directory.

### <a name="client-app-initialization"></a>Initialisierung der Client-App

1. Verwenden Sie die **Anwendungs-ID** und das **Clientgeheimnis** (Anwendungsschlüssel) aus der Azure Active Directory-App-Registrierung, um das Token für die Anwendung abzurufen.

    In C# können Sie das Token für die Anwendung mit dem folgenden Code abrufen. Ein vollständiges Beispiel finden Sie unter [Abfragen von Daten aus einer Azure Time Series Insights-Umgebung mit C#](time-series-insights-query-data-csharp.md).

    ```csharp
    // Enter your Active Directory tenant domain name
    var tenant = "YOUR_AD_TENANT.onmicrosoft.com";
    var authenticationContext = new AuthenticationContext(
        $"https://login.microsoftonline.com/{tenant}",
        TokenCache.DefaultShared);

    AuthenticationResult token = await authenticationContext.AcquireTokenAsync(
        // Set the resource URI to the Azure Time Series Insights API
        resource: "https://api.timeseries.azure.com/",
        clientCredential: new ClientCredential(
            // Application ID of application registered in Azure Active Directory
            clientId: "YOUR_APPLICATION_ID",
            // Application key of the application that's registered in Azure Active Directory
            clientSecret: "YOUR_CLIENT_APPLICATION_KEY"));

    string accessToken = token.AccessToken;
    ```

1. Das Token kann dann im `Authorization`-Header übergeben werden, wenn die Anwendung die Time Series Insights-API aufruft.

## <a name="common-headers-and-parameters"></a>Allgemeine Parameter und Header

In diesem Abschnitt werden allgemeine HTTP-Anforderungsheader und Parameter beschrieben, mit denen Abfragen der Time Series Insights GA- und Vorschau-APIs durchgeführt werden. API-spezifische Anforderungen werden ausführlicher in der [Referenzdokumentation zur Time Series Insights-REST-API](https://docs.microsoft.com/rest/api/time-series-insights/) behandelt.

### <a name="authentication"></a>Authentication

Um authentifizierte Abfragen der [Time Series Insights-REST-APIs](https://docs.microsoft.com/rest/api/time-series-insights/) durchzuführen, muss ein gültiges OAuth 2.0-Bearertoken im [Autorisierungsheader](/rest/api/apimanagement/2019-01-01/authorizationserver/createorupdate) mit einem REST-Client Ihrer Wahl (Postman, JavaScript, C#) übergeben werden. 

> [!IMPORTANT]
> Das Token muss spezifisch für die `https://api.timeseries.azure.com/`-Ressource (auch als „Zielgruppe“ des Tokens bezeichnet) ausgestellt werden.
> * Die [Postman](https://www.getpostman.com/)-**AuthURL** ist daher konform mit: `https://login.microsoftonline.com/microsoft.onmicrosoft.com/oauth2/authorize?resource=https://api.timeseries.azure.com/`

> [!TIP]
> Weitere Informationen zur programmgesteuerten Authentifizierung mit den APIs von Time Series Insights unter Verwendung des [JavaScript Client SDK](https://github.com/microsoft/tsiclient/blob/master/docs/API.md) zusammen mit Diagrammen und Grafiken finden Sie in der gehosteten [Beispielvisualisierung des Azure Time Series Insights-Client-SDKs](https://tsiclientsample.azurewebsites.net/).

### <a name="http-headers"></a>HTTP-Header

Erforderliche Anforderungsheader:

- `Authorization`: Für die Authentifizierung und Autorisierung muss im Autorisierungsheader ein gültiges OAuth 2.0-Bearertoken übergeben werden. Das Token muss spezifisch für die `https://api.timeseries.azure.com/`-Ressource (auch als „Zielgruppe“ des Tokens bezeichnet) ausgestellt werden.

Optionale Anforderungsheader:

- `Content-type`: Nur `application/json` wird unterstützt.
- `x-ms-client-request-id`: Eine Clientanforderungs-ID. Der Dienst zeichnet diesen Wert auf. Ermöglicht es dem Dienst, den Vorgang dienstübergreifend nachzuverfolgen.
- `x-ms-client-session-id`: Eine Clientsitzungs-ID. Der Dienst zeichnet diesen Wert auf. Ermöglicht es dem Dienst, eine Gruppe verwandter Vorgänge dienstübergreifend nachzuverfolgen.
- `x-ms-client-application-name`: Der Name der Anwendung, die diese Anforderung generiert hat. Der Dienst zeichnet diesen Wert auf.

Antwortheader:

- `Content-type`: Nur `application/json` wird unterstützt.
- `x-ms-request-id`: Vom Server generierte Anforderungs-ID. Kann zum Kontaktieren von Microsoft verwendet werden, um eine Anforderung zu untersuchen.

### <a name="http-parameters"></a>HTTP-Parameter

Erforderliche URL-Abfragezeichenfolgeparameter:

- `api-version=2016-12-12`
- `api-version=2018-11-01-preview`

Optionale URL-Abfragezeichenfolgeparameter:

- `timeout=<timeout>`: Das serverseitige Timeout für die Anforderungsausführung. Gilt nur für die APIs zum [Abrufen von Umgebungsereignissen](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api#get-environment-events-api) und [Abrufen von Umgebungsaggregaten](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api#get-environment-aggregates-api). Der Timeoutwert muss das ISO 8601-Format für die Dauer aufweisen (z.B. `"PT20S"`) und sollte im Bereich `1-30 s` liegen. Der Standardwert ist `30 s`.

## <a name="next-steps"></a>Nächste Schritte

- Beispielcode, in dem die GA Time Series Insights-API aufgerufen wird, finden Sie unter [Abfragen von Daten mit C#](./time-series-insights-query-data-csharp.md).

- Codebeispiele für die Vorschauversion der Time Series Insights-API finden Sie unter [Abfragen von Vorschaudaten mit C#](./time-series-insights-update-query-data-csharp.md).

- API-Referenzinformationen finden Sie in der Referenz zur [Abfrage-API](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api).

- Informieren Sie sich, wie Sie [einen Dienstprinzipal erstellen](../active-directory/develop/howto-create-service-principal-portal.md).