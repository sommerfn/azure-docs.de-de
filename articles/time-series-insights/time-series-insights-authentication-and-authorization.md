---
title: Authentifizieren und Autorisieren mithilfe der API in Azure Time Series Insights | Microsoft-Dokumentation
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
ms.date: 06/29/2019
ms.custom: seodec18
ms.openlocfilehash: bdf0fbfb2b40e932f9e3627c3bc0356eb0afb472
ms.sourcegitcommit: 13d5eb9657adf1c69cc8df12486470e66361224e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2019
ms.locfileid: "68677932"
---
# <a name="authentication-and-authorization-for-azure-time-series-insights-api"></a>Authentifizierung und Autorisierung für die Azure Time Series Insights-API

Dieses Dokument bespricht die Registrierung einer App in Azure Active Directory mit dem neuen Azure Active Directory-Blatt. Apps, die in Azure Active Directory registriert sind, ermöglichen es Benutzern, sich für die Azure Time Series Insights-API in einer Time Series Insights-Umgebung authentifizieren und autorisieren zu lassen.

## <a name="service-principal"></a>Dienstprinzipal

Im folgenden Abschnitten wird erläutert, wie Sie eine Anwendung für den Zugriff auf die Azure Time Series Insights-API für die Anwendung konfigurieren. Die Anwendung kann dann anhand der eigenen Anwendungsanmeldeinformationen über Azure Active Directory in der Time Series Insights-Umgebung abfragen oder veröffentlichen.

## <a name="summary-and-best-practices"></a>Zusammenfassung und bewährte Methoden

Der Ablauf der Azure Active Directory-Appregistrierung besteht aus drei wichtigen Schritten.

1. [Registrieren Sie eine Anwendung](#azure-active-directory-app-registration) in Azure Active Directory.
1. Autorisieren dieser Anwendung zum [Zugriff auf Daten der Time Series Insights-Umgebung](#granting-data-access).
1. Verwenden Sie die **Anwendungs-ID** und den **geheimen Clientschlüssel**, um einen Token von `https://api.timeseries.azure.com/` in Ihrer [Client-App](#client-app-initialization) zu erhalten. Mit dem Token kann dann die Time Series Insights-API aufgerufen werden.

Wenn Sie in **Schritt 3** die Anmeldeinformationen Ihrer Anwendung und Ihrer Benutzer trennen, können Sie so:

* App-Identitätsberechtigungen zuweisen, die sich von Ihren eigenen Berechtigungen unterscheiden. In der Regel sind diese Berechtigungen auf diejenigen beschränkt, die für die App erforderlich sind. Beispielsweise können Sie festlegen, dass die App nur in einer bestimmten Time Series Insights-Umgebung Daten lesen darf.
* Isolieren Sie die Sicherheit der App von den Authentifizierungsinformationen des Erstellers, indem Sie einen **Clientschlüssel** oder ein Sicherheitszertifikat verwenden. Danach sind die Anmeldeinformationen der Anwendung nicht mehr von den Anmeldeinformationen eines bestimmten Benutzers abhängig. Wenn sich die Rolle des Benutzers ändert, dann benötigt die Anwendung nicht unbedingt neue Anmeldeinformationen oder eine neue Konfiguration. Wenn der Benutzer sein Kennwort ändert, dann müssen für die Zugriffe auf die Anwendung keine neuen Anmeldeinformationen oder Schlüssel erstellt werden.
* Führen Sie ein unbeaufsichtigtes Skript mit einem **geheimen Clientschlüssel** oder einen Sicherheitszertifikat aus, statt mit bestimmten Benutzeranmeldeinformationen (für die der Benutzer anwesend sein muss).
* Setzen Sie statt einem Kennwort ein Sicherheitszertifikat ein, um den Zugriff auf Ihre Azure Time Series Insights-API zu sichern.

> [!IMPORTANT]
> Halten Sie sich an das Konzept der **Trennung von Zuständigkeiten** (wie im Szenario oben beschrieben), wenn Sie ihre Azure Time Series Insights-Sicherheitsrichtlinien konfigurieren.

> [!NOTE]
> * Der Schwerpunkt des Artikels liegt dabei auf einer Anwendung mit nur einem Mandanten, die zur Ausführung in nur einer einzigen Organisation vorgesehen ist.
> * Sie setzen Anwendungen mit nur einem Mandanten in der Regel für Branchenanwendungen ein, die innerhalb Ihrer Organisation ausgeführt werden.

## <a name="detailed-setup"></a>Setup – detailliert

### <a name="azure-active-directory-app-registration"></a>Azure Active Directory-Appregistrierung

[!INCLUDE [Azure Active Directory app registration](../../includes/time-series-insights-aad-registration.md)]

### <a name="granting-data-access"></a>Gewähren von Datenzugriff

1. Wählen Sie für die Time Series Insights-Umgebung **„Datenzugriffsrichtlinien“** aus, und wählen Sie **„Hinzufügen“** .

   [![Hinzufügen einer neuen Datenzugriffsrichtlinie zur Time Series Insights-Umgebung](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png#lightbox)

1. Fügen Sie in der Dialogfeld **„Benutzer auswählen“** entweder den **Anwendungsnamen** oder die **Anwendungs-ID** aus der Azure Active Directory-Appregistrierung.

   [![Suchen einer Anwendung im Dialogfeld „Benutzer auswählen“](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png#lightbox)

1. Wählen Sie die Rolle aus. Wählen Sie **„Leser“** aus, um Daten abzufragen oder **„Mitwirkender“** , um Daten abzufragen und Referenzdaten zu ändern. Klicken Sie auf **OK**.

   [![Auswählen von „Leser“ oder „Mitwirkender“ im Dialogfeld „Rolle auswählen“](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png#lightbox)

1. Speichern Sie die Richtlinie mit **„OK“** .

   > [!TIP]
   > Lesen Sie Informationen zum [Gewähren von Datenzugriff](./time-series-insights-data-access.md) auf Ihre Time Series Insights-Umgebung in Azure Active Directory.

### <a name="client-app-initialization"></a>Initialisierung der Client-App

1. Verwenden Sie die **Anwendungs-ID** und den **geheimen Clientschlüssel** (Anwendungsschlüssel) aus der Azure Active Directory-Appregistrierung, um einen Token für die Anwendung abzurufen.

    In C# können Sie mit dem folgenden Code das Token für die Anwendung abrufen. Ein vollständiges Beispiel finden Sie unter [Abfragen von Daten aus einer Azure Time Series Insights-Umgebung mit C#](time-series-insights-query-data-csharp.md).

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

## <a name="next-steps"></a>Nächste Schritte

- Beispielcode, in dem die GA Time Series Insights-API aufgerufen wird, finden Sie unter [Abfragen von Daten aus einer Azure Time Series Insights-Umgebung mit C#](./time-series-insights-query-data-csharp.md).

- Codebeispiel für die Vorschauversion der Time Series Insights API finden Sie unter [„Abfragevorschau von Daten mit C#“](./time-series-insights-update-query-data-csharp.md).

- API-Referenzinformationen finden Sie in der Referenz zur [Abfrage-API](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api).

- Informieren Sie sich, wie Sie [einen Dienstprinzipal erstellen](../active-directory/develop/howto-create-service-principal-portal.md).