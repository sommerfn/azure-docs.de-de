---
title: Erstellen von Richtlinien mithilfe des Azure Data Explorer C# SDK
description: In diesem Artikel erfahren Sie, wie Sie Richtlinien mithilfe von C# erstellen.
author: lucygoldbergmicrosoft
ms.author: lugoldbe
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 09/24/2019
ms.openlocfilehash: fa2dd4993dbf70bcbc6ea97726f8cd7254123429
ms.sourcegitcommit: 9f330c3393a283faedaf9aa75b9fcfc06118b124
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/07/2019
ms.locfileid: "71997275"
---
# <a name="create-database-and-table-policies-for-azure-data-explorer-using-c"></a>Erstellen von Datenbank- und Tabellenrichtlinien für Azure Data Explorer mithilfe von C#

> [!div class="op_single_selector"]
> * [C#](database-table-policies-csharp.md)
> * [Python](database-table-policies-python.md)
>

Azure-Daten-Explorer ist ein schneller und hochgradig skalierbarer Dienst zur Untersuchung von Daten (Protokoll- und Telemetriedaten). In diesem Artikel erstellen Sie Datenbank- und Tabellenrichtlinien für Azure Data Explorer mithilfe von C#.

## <a name="prerequisites"></a>Voraussetzungen

* Falls Sie Visual Studio 2019 nicht installiert haben, können Sie die **kostenlose** [Visual Studio 2019 Community-Edition](https://www.visualstudio.com/downloads/) herunterladen und verwenden. Aktivieren Sie beim Setup von Visual Studio die Option **Azure-Entwicklung**.

* Wenn Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

* [Einen Testcluster und eine Testdatenbank](create-cluster-database-csharp.md)

* [Eine Testtabelle](net-standard-ingest-data.md#create-a-table-on-your-test-cluster)

## <a name="install-c-nuget"></a>Installieren eines C#-NuGet-Pakets

* Installieren Sie das [Azure Data Explorer-NuGet-Paket (Kusto)](https://www.nuget.org/packages/Microsoft.Azure.Management.Kusto/).

* Installieren Sie das [NuGet-Paket Microsoft.Azure.Kusto.Data.NETStandard](https://www.nuget.org/packages/Microsoft.Azure.Kusto.Data.NETStandard/) (optional, zum Ändern von Richtlinien der Tabelle).

* Installieren Sie das [NuGet-Paket „Microsoft.IdentityModel.Clients.ActiveDirectory“](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) für die Authentifizierung.

## <a name="authentication"></a>Authentication
Zum Ausführen der Beispiele in diesem Artikel benötigen wir eine Azure AD-Anwendung und einen Dienstprinzipal, der auf Ressourcen zugreifen kann. Sie können dieselbe Azure AD-Anwendung für die Authentifizierung aus einem [Testcluster und einer -datenbank](create-cluster-database-csharp.md#authentication) verwenden. Wenn Sie eine andere Azure AD Anwendung verwenden möchten, finden Sie weitere Informationen unter [Erstellen einer Azure AD-Anwendung](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal), um eine kostenlose Azure AD-Anwendung zu erstellen und die Rollenzuweisung im Abonnementbereich hinzuzufügen. Außerdem wird gezeigt, wie Sie die Angaben `Directory (tenant) ID`, `Application ID` und `Client Secret` abrufen. Möglicherweise müssen Sie die neue Azure AD-Anwendung als Prinzipal in der-Datenbank hinzufügen, siehe [Verwalten von Azure Data Explorer-Datenbankberechtigungen](https://docs.microsoft.com/azure/data-explorer/manage-database-permissions).   

## <a name="alter-database-retention-policy"></a>Ändern der Aufbewahrungsrichtlinie für die Datenbank
Legt eine Aufbewahrungsrichtlinie mit einem vorläufigen Löschzeitraum von 10 Tagen fest.
    
```csharp
var tenantId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Directory (tenant) ID
var clientId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Application ID
var clientSecret = "xxxxxxxxxxxxxx";//Client Secret
var subscriptionId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";
var authenticationContext = new AuthenticationContext($"https://login.windows.net/{tenantId}");
var credential = new ClientCredential(clientId, clientSecret);
var result = await authenticationContext.AcquireTokenAsync(resource: "https://management.core.windows.net/", clientCredential: credential);

var credentials = new TokenCredentials(result.AccessToken, result.AccessTokenType);

var kustoManagementClient = new KustoManagementClient(credentials)
{
    SubscriptionId = subscriptionId
};

var resourceGroupName = "testrg";
//The cluster and database that are created as part of the Prerequisites
var clusterName = "mykustocluster";
var databaseName = "mykustodatabase";
await kustoManagementClient.Databases.UpdateAsync(resourceGroupName, clusterName, databaseName, new DatabaseUpdate(softDeletePeriod: TimeSpan.FromDays(10)));
```

## <a name="alter-database-cache-policy"></a>Ändern der Richtlinie für die Datenbankzwischenspeicherung
Legt eine Cacherichtlinie für die Datenbank fest, dass sich die Daten der letzten fünf Tage auf dem Cluster-SSD befinden.

```csharp
var tenantId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Directory (tenant) ID
var clientId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Application ID
var clientSecret = "xxxxxxxxxxxxxx";//Client Secret
var subscriptionId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";
var authenticationContext = new AuthenticationContext($"https://login.windows.net/{tenantId}");
var credential = new ClientCredential(clientId, clientSecret);
var result = await authenticationContext.AcquireTokenAsync(resource: "https://management.core.windows.net/", clientCredential: credential);

var credentials = new TokenCredentials(result.AccessToken, result.AccessTokenType);

var kustoManagementClient = new KustoManagementClient(credentials)
{
    SubscriptionId = subscriptionId
};

var resourceGroupName = "testrg";
//The cluster and database that are created as part of the Prerequisites
var clusterName = "mykustocluster";
var databaseName = "mykustodatabase";
await kustoManagementClient.Databases.UpdateAsync(resourceGroupName, clusterName, databaseName, new DatabaseUpdate(hotCachePeriod: TimeSpan.FromDays(5)));
```

## <a name="alter-table-cache-policy"></a>Ändern der Richtlinie für die Tabellenzwischenspeicherung
Legt eine Cacherichtlinie für die Tabelle fest, dass sich die Daten der letzten fünf Tage auf dem Cluster-SSD befinden.

```csharp
var kustoUri = "https://<ClusterName>.<Region>.kusto.windows.net:443/";
var databaseName = "<DatabaseName>";
var tenantId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Directory (tenant) ID
var clientId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Application ID
var clientSecret = "xxxxxxxxxxxxxx";//Client Secret
var tableName = "<TableName>"

var kustoConnectionStringBuilder =
    new KustoConnectionStringBuilder(kustoUri)
    {
        FederatedSecurity = true,
        InitialCatalog = databaseName,
        ApplicationClientId = clientId,
        ApplicationKey = clientSecret,
        Authority = tenantId
    };

using (var kustoClient = KustoClientFactory.CreateCslAdminProvider(kustoConnectionStringBuilder))
{
    //dataHotSpan and indexHotSpan should have the same value
    var hotSpan = TimeSpan.FromDays(5);
    var command1 = CslCommandGenerator.GenerateAlterTableCachingPolicyCommand(tableName: tableName,
                    dataHotSpan: hotSpan, indexHotSpan: hotSpan);

    kustoClient.ExecuteControlCommand(command);
}
```

## <a name="add-a-new-principal-for-database"></a>Hinzufügen eines neuen Prinzipals für die Datenbank
Hinzufügen einer neuen Azure AD-Anwendung als Administratorprinzipal für die Datenbank

```csharp
var tenantId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Directory (tenant) ID
var clientId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Application ID
var clientSecret = "xxxxxxxxxxxxxx";//Client Secret
var clientIdToAdd = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Application ID
var subscriptionId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";
var authenticationContext = new AuthenticationContext($"https://login.windows.net/{tenantId}");
var credential = new ClientCredential(clientId, clientSecret);
var result = await authenticationContext.AcquireTokenAsync(resource: "https://management.core.windows.net/", clientCredential: credential);

var credentials = new TokenCredentials(result.AccessToken, result.AccessTokenType);

var kustoManagementClient = new KustoManagementClient(credentials)
{
    SubscriptionId = subscriptionId
};

var resourceGroupName = "testrg";
//The cluster and database that are created as part of the Prerequisites
var clusterName = "mykustocluster";
var databaseName = "mykustodatabase";
await kustoManagementClient.Databases.AddPrincipalsAsync(resourceGroupName, clusterName, databaseName,
                new DatabasePrincipalListRequest()
                {
                    Value = new List<DatabasePrincipal>()
                    {
                        new DatabasePrincipal("Admin", "<database_principle_name>", "App", appId: clientIdToAdd, tenantName:tenantId)
                    }
                });
```
## <a name="next-steps"></a>Nächste Schritte

* [Weitere Informationen zu Datenbank- und Tabellenrichtlinien](https://docs.microsoft.com/azure/kusto/management/policies)
