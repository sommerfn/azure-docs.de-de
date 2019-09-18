---
title: Azure Cosmos DB-Ressourcentoken mit Gremlin
description: Hier erfahren Sie, wie Sie Ressourcentoken erstellen und damit auf die Graphdatenbank zugreifen.
author: olignat
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: overview
ms.date: 09/06/2019
ms.author: olignat
ms.openlocfilehash: fcb18fb14cf787713735da07ca2048d0853fa46c
ms.sourcegitcommit: b8578b14c8629c4e4dea4c2e90164e42393e8064
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/09/2019
ms.locfileid: "70807011"
---
# <a name="azure-cosmos-db-resource-tokens-with-gremlin"></a>Azure Cosmos DB-Ressourcentoken mit Gremlin
In diesem Artikel wird erläutert, wie Sie mithilfe von [Cosmos DB-Ressourcentoken](secure-access-to-data.md) über das Gremlin SDK auf die Graphdatenbank zugreifen.

## <a name="create-a-resource-token"></a>Erstellen eines Ressourcentoken

Das TinkerPop Gremlin SDK enthält keine API zum Erstellen von Ressourcentoken. Das Ressourcentoken ist ein Konzept von Cosmos DB. Laden Sie zum Erstellen von Ressourcentoken das [Azure Cosmos DB SDK](sql-api-sdk-dotnet.md) herunter. Muss Ihre Anwendung Ressourcentoken erstellen und damit auf die Graphdatenbank zugreifen, benötigen Sie zwei separate SDKs.

Objektmodellhierarchie über Ressourcentoken:
- **Cosmos DB-Konto**: Entität der höchsten Ebene, der DNS zugeordnet ist, etwa `contoso.gremlin.cosmos.azure.com`
  - **Cosmos DB-Datenbank**
    - **Benutzer**
      - **Berechtigung**
        - *Token*: Eine Eigenschaft des **Permission**-Objekts, die angibt, welche Aktionen zugelassen oder verweigert werden

Das Ressourcentoken hat das Format `"type=resource&ver=1&sig=<base64 string>;<base64 string>;"`. Diese Zeichenfolge ist für die Clients nicht transparent und sollte unverändert verwendet werden.

```csharp
// Notice that document client is created against .NET SDK end-point rather than Gremlin.
DocumentClient client = new DocumentClient(
  new Uri("https://contoso.documents.azure.com:443/"), 
  "<master key>", 
  new ConnectionPolicy 
  {
    EnableEndpointDiscovery = false, 
    ConnectionMode = ConnectionMode.Direct 
  });

  // Read specific permission to obtain a token.
  // Token will not be returned during ReadPermissionReedAsync() call.
  // This call will succeed only if database id, user id and permission id already exist. 
  // Note that <database id> is not a database name, it is a base64 string that represents database identifier, for example "KalVAA==".
  // Similar comment applies to <user id> and <permission id>
  Permission permission = await client.ReadPermissionAsync(UriFactory.CreatePermissionUri("<database id>", "<user id>", "<permission id>"));

  Console.WriteLine("Obtained token {0}", permission.Token);
}
```

## <a name="use-a-resource-token"></a>Verwenden eines Ressourcentoken
Ressourcentoken können beim Erstellen der Klasse `GremlinServer` direkt als Eigenschaft „password“ verwendet werden.

```csharp
// Gremlin application needs to be given a resource token. It can't discover the token on its own.
// Token can be obtained for a given permission using Cosmos DB SDK or passed into the application as command line argument or configuration value.
string resourceToken = GetResourceToken();

// Configure gremlin servier to use resource token rather than master key
GremlinServer server = new GremlinServer(
  "contoso.gremlin.cosmosdb.azure.com",
  port: 443,
  enableSsl: true,
  username: "/dbs/<database name>/colls/<collection name>",

  // Format of the token is "type=resource&ver=1&sig=<base64 string>;<base64 string>;"
  password: resourceToken);

  using (GremlinClient gremlinClient = new GremlinClient(server, new GraphSON2Reader(), new GraphSON2Writer(), GremlinClient.GraphSON2MimeType))
  {
      await gremlinClient.SubmitAsync("g.V().limit(1)");
  }
```

Der gleiche Ansatz funktioniert in allen TinkerPop Gremlin SDKs.

```java
Cluster.Builder builder = Cluster.build();

AuthProperties authenticationProperties = new AuthProperties();
authenticationProperties.with(AuthProperties.Property.USERNAME,
    String.format("/dbs/%s/colls/%s", "<database name>", "<collection name>"));

// Format of the token is "type=resource&ver=1&sig=<base64 string>;<base64 string>;"
authenticationProperties.with(AuthProperties.Property.PASSWORD, resourceToken);

builder.authProperties(authenticationProperties);
```

## <a name="limit"></a>Begrenzung

Ein einzelnes Gremlin-Konto kann eine unbegrenzte Anzahl von Token ausgeben. Innerhalb **einer Stunde** können jedoch nur bis zu **100** Token gleichzeitig verwendet werden. Wenn die Anwendung das Tokenlimit pro Stunde überschreitet, wird die Authentifizierungsanforderung mit der Fehlermeldung `"Exceeded allowed resource token limit of 100 that can be used concurrently"` verweigert. Das Schließen aktiver Verbindungen mit bestimmten Token zum Freigeben von Slots für neue Token ist nicht hilfreich. Die Cosmos DB-Gremlin-Datenbank-Engine erfasst individuelle Token in der Stunde vor der Authentifizierungsanforderung.

## <a name="permission"></a>Berechtigung

Bei der Verwendung von Ressourcentoken tritt bei Anwendungen häufig der Fehler `"Insufficient permissions provided in the authorization header for the corresponding request. Please retry with another authorization header."` auf. Dieser Fehler wird zurückgegeben, wenn beim Gremlin-Durchlauf versucht wird, eine Kante oder einen Scheitelpunkt zu schreiben, das Ressourcentoken jedoch nur Berechtigungen vom Typ `Read` erteilt. Überprüfen Sie, ob der Durchlauf einen der folgenden Schritte enthält: `.addV()`, `.addE()`, `.drop()` oder `.property()`.

## <a name="next-steps"></a>Nächste Schritte
* [Rollenbasierte Zugriffssteuerung in Azure Cosmos DB](role-based-access-control.md)
* [Sicherer Zugriff auf Daten in Azure Cosmos DB](secure-access-to-data.md)
