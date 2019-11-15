---
title: Beispiele und Definitionen für Überwachungsprotokolle in Azure Active Directory B2C
description: Leitfaden und Beispiele für den Zugriff auf die Azure AD B2C-Überwachungsprotokolle
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.date: 10/16/2019
ms.author: marsma
ms.subservice: B2C
ms.custom: fasttrack-edit
ms.openlocfilehash: b1070314767f280ec9d15390dc838fa80b5508e2
ms.sourcegitcommit: 359930a9387dd3d15d39abd97ad2b8cb69b8c18b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73643604"
---
# <a name="accessing-azure-ad-b2c-audit-logs"></a>Zugriff auf Active Directory B2C-Überwachungsprotokolle

Azure Active Directory B2C (Azure AD B2C) gibt Überwachungsprotokolle aus, die Aktivitätsinformationen über B2C-Ressourcen, ausgestellte Token und Administratorzugriff enthalten. Dieser Artikel bietet eine kurze Übersicht über die in Überwachungsprotokollen verfügbaren Informationen sowie Anweisungen zum Zugreifen auf diese Daten für Ihren Azure AD B2C-Mandanten.

Überwachungsprotokollereignisse werden nur **sieben Tage** lang aufbewahrt. Planen Sie den Download und die Speicherung Ihrer Protokolle mit einer der unten aufgeführten Methoden, wenn eine längere Aufbewahrungsdauer erforderlich ist.

> [!NOTE]
> Im Azure-Portal werden auf der Seite **Azure Active Directory** oder **Azure AD B2C** im Abschnitt **Benutzer** keine Benutzeranmeldungen für einzelne Azure AD B2C-Anwendungen angezeigt. Für die dort aufgeführten Anmeldeereignisse werden zwar Benutzeraktivitäten angezeigt, aber auf die jeweilige B2C-Anwendung, bei der sich der Benutzer angemeldet hat, sind keine Rückschlüsse möglich. Hierfür müssen Sie die Überwachungsprotokolle verwenden. Wie Sie dabei vorgehen, wird in diesem Artikel beschrieben.

## <a name="overview-of-activities-available-in-the-b2c-category-of-audit-logs"></a>Übersicht der verfügbaren Aktivitäten in der B2C-Kategorie von Überwachungsprotokollen

Die **B2C**-Kategorie in Überwachungsprotokollen umfasst die folgenden Aktivitätstypen:

|Aktivitätstyp |BESCHREIBUNG  |
|---------|---------|
|Authorization |Aktivitäten, die sich auf die Autorisierung eines Benutzers für den Zugriff auf B2C-Ressourcen beziehen (z. B. ein Administrator, der auf eine Liste von B2C-Richtlinien zugreift).         |
|Verzeichnis |Aktivitäten im Zusammenhang mit Verzeichnisattributen, die abgerufen werden, wenn sich ein Administrator über das Azure-Portal anmeldet. |
|Anwendung | Erstellungs-, Lese-, Aktualisierungs- und Löschvorgänge (Create, Read, Update, Delete, CRUD) für B2C-Anwendungen. |
|Schlüssel |CRUD-Vorgänge für in einem B2C-Schlüsselcontainer gespeicherte Schlüssel. |
|Resource |CRUD-Vorgänge für B2C-Ressourcen. Beispielsweise Richtlinien und Identitätsanbieter.
|Authentication |Überprüfung von Benutzeranmeldeinformationen und Tokenausstellung.|

Informationen zu den CRUD-Aktivitäten des Benutzerobjekts finden Sie in der Kategorie **Hauptverzeichnis**.

## <a name="example-activity"></a>Beispielaktivität

Im folgenden Beispiel aus dem Azure-Portal sehen Sie die Daten, die erfasst werden, wenn sich ein Benutzer bei einem externen Identitätsanbieter (in diesem Fall Facebook) anmeldet:

![Beispiel für die Seite der Aktivitätsdetails des Überwachungsprotokolls im Azure-Portal](./media/active-directory-b2c-reference-audit-logs/audit-logs-example.png)

Das Panel „Aktivitätsbereich“ enthält die folgenden wichtigen Informationen:

|`Section`|Feld|BESCHREIBUNG|
|-------|-----|-----------|
| Aktivität | NAME | Die ausgeführte Aktivität. Dies kann beispielsweise *Issue an id_token to the application* (ID-Token für die Anwendung ausstellen) sein, wodurch die Benutzeranmeldung abgeschlossen wird. |
| Initiiert von (Akteur) | ObjectId | Die **Objekt-ID** der B2C-Anwendung, bei der sich der Benutzer anmeldet. Dieser Bezeichner ist im Azure-Portal nicht sichtbar, kann aber über die Microsoft Graph-API aufgerufen werden. |
| Initiiert von (Akteur) | Spn | Die **Anwendungs-ID** der B2C-Anwendung, bei der sich der Benutzer anmeldet. |
| Ziel(e) | ObjectId | Die **Objekt-ID** des Benutzers, der sich anmeldet. |
| Weitere Details | TenantId | Die **Mandanten-ID** des Azure AD B2C-Mandanten. |
| Weitere Details | `PolicyId` | Die **Richtlinien-ID** für den Benutzerfluss (Richtlinie), der zur Anmeldung des Benutzers verwendet wird. |
| Weitere Details | ApplicationId | Die **Anwendungs-ID** der B2C-Anwendung, bei der sich der Benutzer anmeldet. |

## <a name="view-audit-logs-in-the-azure-portal"></a>Anzeigen von Überwachungsprotokollen im Azure-Portal

Das Azure-Portal bietet Zugriff auf die Überwachungsprotokollereignisse in Ihrem Azure AD B2C-Mandanten.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)
1. Wechseln Sie zu dem Verzeichnis, das Ihren Azure AD B2C-Mandanten enthält, und navigieren Sie dann zu **Azure AD B2C**.
1. Wählen Sie im linken Menü unter **Aktivitäten** die Option **Überwachungsprotokolle** aus.

Es wird eine Liste der Aktivitätsereignisse angezeigt, die in den letzten sieben Tagen protokolliert wurden.

![Beispiel für einen Filter mit zwei Aktivitätsereignissen im Azure-Portal](media/active-directory-b2c-reference-audit-logs/audit-logs-example-filter.png)

Es sind mehrere Filteroptionen verfügbar, einschließlich:

* **Aktivitätsressourcentyp**: Filtern Sie nach den Aktivitätstypen, die in der Tabelle im Abschnitt [Übersicht der verfügbaren Aktivitäten in der B2C-Kategorie von Überwachungsprotokollen](#overview-of-activities-available-in-the-b2c-category-of-audit-logs) aufgeführt sind.
* **Datum**: Filtern Sie den Datumsbereich der angezeigten Aktivitäten.

Wenn Sie eine Zeile in der Liste auswählen, werden die Aktivitätsdetails für das Ereignis angezeigt.

Zum Herunterladen der Liste der Aktivitätsereignisse in einer CSV-Datei wählen Sie **Herunterladen** aus.

## <a name="get-audit-logs-with-the-azure-ad-reporting-api"></a>Abrufen von Überwachungsprotokollen mit der Azure AD-Berichterstellungs-API

Überwachungsprotokolle werden in der gleichen Pipeline wie andere Aktivitäten für Azure Active Directory veröffentlicht, sodass auf sie über die [Azure Active Directory-Berichterstellungs-API](https://docs.microsoft.com/graph/api/directoryaudit-list) zugegriffen werden kann. Weitere Informationen finden Sie unter [Erste Schritte mit der Berichterstellungs-API von Azure Active Directory](../active-directory/reports-monitoring/concept-reporting-api.md).

### <a name="enable-reporting-api-access"></a>Aktivieren des Zugriffs auf die Berichterstellungs-API

Um den skript- oder anwendungsbasierten Zugriff auf die Azure AD-Berichterstellungs-API zu ermöglichen, benötigen Sie eine Azure Active Directory-Anwendung, die in Ihrem Azure AD B2C-Mandanten mit den folgenden API-Berechtigungen registriert ist:

* Microsoft Graph > Anwendungsberechtigungen > AuditLog.Read.All

Sie können diese Berechtigungen für eine vorhandene Azure Active Directory-Anwendungsregistrierung in Ihrem B2C-Mandanten aktivieren oder eine neue Anwendung erstellen, die speziell für die Verwendung mit der Überwachungsprotokollautomatisierung verwendet wird.

Führen Sie die folgenden Schritte zum Registrieren einer Anwendung aus, erteilen Sie ihr die erforderlichen Microsoft Graph-API-Berechtigungen, und erstellen Sie dann einen geheimen Clientschlüssel.

### <a name="register-application-in-azure-active-directory"></a>Registrieren einer Anwendung in Azure Active Directory

[!INCLUDE [active-directory-b2c-appreg-mgmt](../../includes/active-directory-b2c-appreg-mgmt.md)]

### <a name="assign-api-access-permissions"></a>Zuweisen von API-Zugriffsberechtigungen

#### <a name="applicationstabapplications"></a>[Anwendungen](#tab/applications/)

1. Wählen Sie auf der Übersichtsseite **Registrierte App** die Option **Einstellungen** aus.
1. Wählen Sie unter **API-ZUGRIFF** die Option **Erforderliche Berechtigungen** aus.
1. Wählen Sie **Hinzufügen** aus, und wählen Sie dann **Hiermit wählen Sie eine API aus** aus.
1. Wählen Sie **Microsoft Graph** aus, und wählen Sie dann **Auswählen** aus.
1. Wählen Sie unter **ANWENDUNGSBERECHTIGUNGEN** die Option **Alle Überwachungsprotokolldaten lesen** aus.
1. Wählen Sie die Schaltfläche **Auswählen** aus, und wählen Sie dann **Fertig** aus.
1. Wählen Sie **Berechtigungen erteilen** und dann **Ja** aus.

#### <a name="app-registrations-previewtabapp-reg-preview"></a>[App-Registrierungen (Vorschau)](#tab/app-reg-preview/)

1. Wählen Sie unter **Verwalten** die Option **API-Berechtigungen**.
1. Wählen Sie unter **Konfigurierte Berechtigungen** die Option **Berechtigung hinzufügen** aus.
1. Wählen Sie die Registerkarte **Microsoft-APIs** aus.
1. Wählen Sie **Microsoft Graph**.
1. Wählen Sie **Anwendungsberechtigungen**.
1. Erweitern Sie **AuditLog**, und aktivieren Sie dann das Kontrollkästchen **AuditLog.Read.All**.
1. Wählen Sie **Berechtigungen hinzufügen** aus. Warten Sie gemäß der Anweisung einige Minuten, bevor Sie mit dem nächsten Schritt fortfahren.
1. Wählen Sie **Administratorzustimmung für (Name Ihres Mandanten) erteilen** aus.
1. Wählen Sie Ihr aktuell angemeldetes Konto aus, wenn ihm die Rolle *Globaler Administrator* zugewiesen wurde, oder melden Sie sich mit einem Konto bei Ihrem Azure AD B2C-Mandanten an, dem die Rolle *Globaler Administrator* zugewiesen wurde.
1. Wählen Sie **Akzeptieren** aus.
1. Wählen Sie **Aktualisieren** aus, und vergewissern Sie sich, dass für die Berechtigung *AuditLog.Read.All* unter **Status** der Status „Gewährt für...“ angezeigt wird. Es kann einige Minuten dauern, bis die Berechtigungen weitergegeben wurden.

* * *

### <a name="create-client-secret"></a>Erstellen eines geheimen Clientschlüssels

[!INCLUDE [active-directory-b2c-client-secret](../../includes/active-directory-b2c-client-secret.md)]

Sie verfügen jetzt über eine Anwendung mit dem erforderlichen API-Zugriff, eine Anwendungs-ID und einen Schlüssel, die/den Sie in Ihren Automatisierungsskripts verwenden können. Ein Beispiel dafür, wie Sie Aktivitätsereignisse mit einem Skript abrufen, finden Sie im Abschnitt „PowerShell-Skript“ weiter unten in diesem Artikel.

### <a name="access-the-api"></a>Zugreifen auf die API

Wenn Sie Azure AD B2C-Überwachungsprotokollereignisse über die API herunterladen möchten, filtern Sie die Protokolle nach der Kategorie `B2C`. Verwenden Sie zum Filtern nach Kategorie den Abfragezeichenfolgenparameter `filter`, wenn Sie den Endpunkt der Azure AD-Berichterstellungs-API aufrufen.

```HTTP
https://graph.microsoft.com/v1.0/auditLogs/directoryAudits?$filter=loggedByService eq 'B2C' and activityDateTime gt 2019-09-10T02:28:17Z
```

### <a name="powershell-script"></a>PowerShell-Skript

Im folgenden PowerShell-Skript sehen Sie ein Beispiel für das Abfragen der Azure AD-Berichterstellungs-API. Nach dem Abfragen der API werden die protokollierten Ereignisse an die Standardausgabe ausgegeben, und dann wird die JSON-Ausgabe in eine Datei geschrieben.

Sie können dieses Skript in der [Azure Cloud Shell](../cloud-shell/overview.md) ausprobieren. Sie müssen es dann mit Ihrer Anwendungs-ID, dem Clientgeheimnis und dem Namen Ihres Azure AD B2C-Mandanten aktualisieren.

```powershell
# This script requires the registration of a Web Application in Azure Active Directory:
# https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-reporting-api

# Constants
$ClientID       = "your-client-application-id-here"       # Insert your application's client ID, a GUID (registered by Global Admin)
$ClientSecret   = "your-client-application-secret-here"   # Insert your application's client secret
$tenantdomain   = "your-b2c-tenant.onmicrosoft.com"       # Insert your Azure AD B2C tenant; for example, contoso.onmicrosoft.com
$loginURL       = "https://login.microsoftonline.com"
$resource       = "https://graph.microsoft.com"           # Microsoft Graph API resource URI
$7daysago       = "{0:s}" -f (get-date).AddDays(-7) + "Z" # Use 'AddMinutes(-5)' to decrement minutes, for example
Write-Output "Searching for events starting $7daysago"

# Create HTTP header, get an OAuth2 access token based on client id, secret and tenant domain
$body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
$oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

# Parse audit report items, save output to file(s): auditX.json, where X = 0 thru n for number of nextLink pages
if ($oauth.access_token -ne $null) {
    $i=0
    $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}
    $url = "https://graph.microsoft.com/v1.0/auditLogs/directoryAudits?`$filter=loggedByService eq 'B2C' and activityDateTime gt  " + $7daysago

    # loop through each query page (1 through n)
    Do {
        # display each event on the console window
        Write-Output "Fetching data using Uri: $url"
        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)
        foreach ($event in ($myReport.Content | ConvertFrom-Json).value) {
            Write-Output ($event | ConvertTo-Json)
        }

        # save the query page to an output file
        Write-Output "Save the output to a file audit$i.json"
        $myReport.Content | Out-File -FilePath audit$i.json -Force
        $url = ($myReport.Content | ConvertFrom-Json).'@odata.nextLink'
        $i = $i+1
    } while($url -ne $null)
} else {
    Write-Host "ERROR: No Access Token"
}
```

Nachfolgend sehen Sie die JSON-Darstellung des Beispielsaktivitätsereignisses, das weiter oben in diesem Artikel erörtert wurde:

```JSON
{
    "id": "B2C_DQO3J_4984536",
    "category": "Authentication",
    "correlationId": "00000000-0000-0000-0000-000000000000",
    "result": "success",
    "resultReason": "N/A",
    "activityDisplayName": "Issue an id_token to the application",
    "activityDateTime": "2019-09-14T18:13:17.0618117Z",
    "loggedByService": "B2C",
    "operationType": "",
    "initiatedBy": {
        "user": null,
        "app": {
            "appId": "00000000-0000-0000-0000-000000000000",
            "displayName": null,
            "servicePrincipalId": null,
            "servicePrincipalName": "00000000-0000-0000-0000-000000000000"
        }
    },
    "targetResources": [
        {
            "id": "00000000-0000-0000-0000-000000000000",
            "displayName": null,
            "type": "User",
            "userPrincipalName": null,
            "groupType": null,
            "modifiedProperties": []
        }
    ],
    "additionalDetails": [
        {
            "key": "TenantId",
            "value": "test.onmicrosoft.com"
        },
        {
            "key": "PolicyId",
            "value": "B2C_1A_signup_signin"
        },
        {
            "key": "ApplicationId",
            "value": "00000000-0000-0000-0000-000000000000"
        },
        {
            "key": "Client",
            "value": "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.132 Safari/537.36"
        },
        {
            "key": "IdentityProviderName",
            "value": "facebook"
        },
        {
            "key": "IdentityProviderApplicationId",
            "value": "0000000000000000"
        },
        {
            "key": "ClientIpAddress",
            "value": "127.0.0.1"
        }
    ]
}
```

## <a name="next-steps"></a>Nächste Schritte

Sie können weitere Verwaltungsaufgaben automatisieren, z. B. das [Verwalten von Benutzern mit .NET](active-directory-b2c-devquickstarts-graph-dotnet.md).
