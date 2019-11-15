---
title: SaaS-Fulfillment-API v2 | Azure Marketplace
description: In diesem Artikel wird erläutert, wie Sie ein SaaS-Angebot im Azure Marketplace und in AppSource mit den zugehörigen Fulfillment-APIs der Version 2 erstellen und verwalten.
services: Azure, Marketplace, Cloud Partner Portal,
author: qianw211
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
ms.date: 10/18/2019
ms.author: evansma
ms.openlocfilehash: 4c73a59352422626ec3c6012607009995479d0cc
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73816606"
---
# <a name="saas-fulfillment-apis-version-2"></a>SaaS-Fulfillment-APIs, Version 2 

In diesem Artikel werden die APIs ausführlich beschrieben, mit der Partner ihre SaaS-Anwendungen im AppSource-Marketplace und im Azure Marketplace verkaufen können. Diese APIs stellen eine Voraussetzung für transaktionsfähige SaaS-Angebote in AppSource und im Azure Marketplace dar.

## <a name="managing-the-saas-subscription-life-cycle"></a>Verwalten des Lebenszyklus von SaaS-Abonnements

Azure-SaaS verwaltet den gesamten Lebenszyklus von abgeschlossenen SaaS-Abonnements. Die Fulfillment-APIs werden dabei als Mechanismus für die tatsächliche Auftragserfüllung, zum Ändern von Plänen und zum Löschen von Abonnements von Partnern verwendet. Die Abrechnung basiert auf dem Status des SaaS-Abonnements, das Microsoft verwaltet. Das folgende Diagramm zeigt die Zustände und die Vorgänge, durch die Änderungen des jeweiligen Status gesteuert werden.

![Die verschiedenen Status im Lebenszyklus von SaaS-Abonnements](./media/saas-subscription-lifecycle-api-v2.png)


### <a name="states-of-a-saas-subscription"></a>Zustände eines SaaS-Abonnements

Die folgende Tabelle enthält die Bereitstellungszustände für ein SaaS-Abonnement, einschließlich einer Beschreibung und eines Sequenzdiagramms für den jeweiligen Status (sofern zutreffend). 

#### <a name="provisioning"></a>Bereitstellung

Wenn ein Kunde einen Kauf initiiert, empfängt der Partner diese Information in Form eines Authentifizierungscodes auf einer interaktiven Webseite für Kunden, die einen URL-Parameter verwendet. Z. B. `https://contoso.com/signup?token=..`, wobei die URL der Angebotsseite im Partner Center `https://contoso.com/signup` ist. Der Authentifizierungscode kann über einen Aufruf der Auflösungs-API überprüft und gegen die Details zum Bereitstellungsdienst ausgetauscht werden.  Wenn der SaaS-Dienst die Bereitstellung abschließt, sendet er einen Aktivierungsaufruf, um zu signalisieren, dass der Auftrag ausgeführt wurde und dem Kunden eine Rechnung gestellt werden kann. 

Das folgende Diagramm zeigt die Abfolge der API-Aufrufe für ein Bereitstellungsszenario.  

![API-Aufrufe zum Bereitstellen eines SaaS-Diensts](./media/saas-post-provisioning-api-v2-calls.png)

#### <a name="provisioned"></a>Bereitgestellt

Dieser Status ist der stabile Zustand eines bereitgestellten Diensts.

##### <a name="provisioning-for-update"></a>Bereitstellung für Aktualisierung 

Dieser Status gibt an, dass ein Update für einen vorhandenen Dienst aussteht. Ein solches Update kann durch den Kunden entweder über den Marketplace oder über den SaaS-Dienst (nur bei Transaktionen, die direkt an den Kunden gehen) initiiert werden.

##### <a name="provisioning-for-update-when-its-initiated-from-the-marketplace"></a>Bereitstellen eines Updates (wenn dieses über den Marketplace initiiert wird)

Das folgende Diagramm zeigt die Aktionssequenzen für Updates, die über den Marketplace initiiert werden.

![API-Aufrufe, wenn das Update über den Marketplace initiiert wird](./media/saas-update-api-v2-calls-from-marketplace-a.png)

##### <a name="provisioning-for-update-when-its-initiated-from-the-saas-service"></a>Bereitstellen eines Updates (wenn dieses über den SaaS-Dienst initiiert wird)

Das folgende Diagramm zeigt die Aktionen im Rahmen eines Updates, das im SaaS-Dienst initiiert wird. (Der Webhookaufruf wird durch ein vom SaaS-Dienst initiiertes Update des Abonnements ersetzt.) 

![API-Aufrufe, wenn das Update vom SaaS-Dienst initiiert wird](./media/saas-update-api-v2-calls-from-saas-service-a.png) 

#### <a name="suspended"></a>Ausgesetzt

Dieser Status gibt an, dass die Zahlung des Kunden noch nicht eingegangen ist. Richtliniengemäß gewähren wir dem Kunden eine Karenzzeit, bevor wir das Abonnement kündigen. Wenn sich ein Abonnement in diesem Zustand befindet, gibt es folgende Möglichkeiten: 

- Partner können den Zugriff des Benutzers auf den Dienst herabstufen oder sperren.
- Das Abonnement muss in einem wiederherstellbaren Zustand gespeichert werden, damit die vollständige Funktionalität ohne Verlust von Daten oder Einstellungen wiederhergestellt werden kann. 
- Sie erhalten sehr wahrscheinlich über die Fulfillment-API eine Reaktivierungsanforderung für dieses Abonnement oder, am Ende der Karenzzeit, eine Anforderung zum Aufheben der Bereitstellung. 

#### <a name="unsubscribed"></a>Abonnement gekündigt 

Abonnements erreichen diesen Status infolge einer expliziten Kundenanforderung oder bei Nichtbezahlung der fälligen Beträge. Der Partner erwartet, dass die Kundendaten für eine Wiederherstellung auf Anfrage für eine bestimmte Anzahl von Tagen gespeichert und dann gelöscht werden. 


## <a name="api-reference"></a>API-Referenz

In diesem Abschnitt werden die SaaS-*Abonnement-API* und die -*Vorgangs-API* dokumentiert.  Der Wert des Parameters `api-version` für Version 2 der APIs lautet `2018-08-31`.  


### <a name="parameter-and-entity-definitions"></a>Parameter- und Entitätsdefinitionen

Die folgende Tabelle enthält die Definitionen für allgemeine Parameter und Entitäten, die von Fulfillment-APIs verwendet werden.

|     Entität/Parameter     |     Definition                         |
|     ----------------     |     ----------                         |
| `subscriptionId`         | Der GUID-Bezeichner für eine SaaS-Ressource  |
| `name`                   | Ein vom Kunden für diese Ressource angegebene Anzeigename |
| `publisherId`            | Ein eindeutiger Zeichenfolgenbezeichner für jeden Herausgeber, z. B. „Contoso“ |
| `offerId`                | Ein eindeutiger Zeichenfolgenbezeichner für jedes Angebot, z. B. „Angebot1“  |
| `planId`                 | Ein eindeutiger Zeichenfolgenbezeichner für jeden Plan bzw. jede SKU, z. B. „Silber“ |
| `operationId`            | Der GUID-Bezeichner für einen bestimmten Vorgang  |
|  `action`                | Die Aktion, die für eine Ressource ausgeführt wird: `Unsubscribe`, `Suspend`, `Reinstate`, oder `ChangePlan`, `ChangeQuantity`, `Transfer`. |
|   |   |

Eindeutige Bezeichner (Globally Unique Identifier, [GUID](https://en.wikipedia.org/wiki/Universally_unique_identifier)) sind 128-Bit-Zahlen (32 hexadezimale Ziffern), die in der Regel automatisch generiert werden. 

#### <a name="resolve-a-subscription"></a>Auflösen eines Abonnements 

Der Auflösungsendpunkt ermöglicht Herausgebern das Auflösen eines Marketplace-Tokens in eine permanente Ressourcen-ID. Die Ressourcen-ID ist der eindeutige Bezeichner für ein SaaS-Abonnement. Wenn ein Benutzer zur Website eines Partners weitergeleitet wird, enthält die URL in den Abfrageparametern ein Token. Es wird erwartet, dass der Partner dieses Token verwendet und dessen Auflösung anfordert. Die Antwort enthält die eindeutige ID des SaaS-Abonnements, den Namen, die Angebots-ID und den Plan für die Ressource. Dieses Token ist nur eine Stunde gültig. 

##### <a name="postbrhttpsmarketplaceapimicrosoftcomapisaassubscriptionsresolveapi-versionapiversion"></a>Post<br>`https://marketplaceapi.microsoft.com/api/saas/subscriptions/resolve?api-version=<ApiVersion>`

*Abfrageparameter:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  Die Version des für diese Anforderung zu verwendenden Vorgangs.  |

*Anforderungsheader:*
 
|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Type      | `application/json` |
|  x-ms-requestid    |  Ein eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Client, vorzugsweise eine GUID. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt. |
|  x-ms-correlationid |  Ein eindeutiger Zeichenfolgenwert für den Vorgang auf dem Client. Dieser Parameter korreliert alle Ereignisse des Clientvorgangs mit serverseitigen Ereignissen. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.  |
|  authorization     |  [Abrufen eines Bearertokens basierend auf der Azure AD-App](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app) Beispiel: „`Bearer <access_token>`“. |
|  x-ms-marketplace-token  |  Der Tokenabfrageparameter in der URL, wenn der Benutzer von Azure zur Website des SaaS-Partners umgeleitet wird (z. B. `https://contoso.com/signup?token=..`). *Hinweis:* Führen Sie eine URL-Decodierung des Tokenwerts aus dem Browser durch, bevor Sie ihn verwenden.  |

*Antwortcodes:*

Code: 200<br>
Löst das nicht transparente Token in ein SaaS-Abonnement auf. Antworttext:
 

```json
{
    "id": "<guid>",  
    "subscriptionName": "Contoso Cloud Solution",
    "offerId": "offer1",
    "planId": "silver",
    "quantity": "20" 
}
```

Code: 400<br>
Ungültige Anforderung. x-ms-marketplace-token fehlt, ist falsch formatiert oder ist abgelaufen.

Code: 403<br>
Nicht autorisiert. Das Authentifizierungstoken wurde nicht bereitgestellt oder ist ungültig, oder die Anforderung versucht, auf einen Kauf zuzugreifen, der nicht zum aktuellen Herausgeber gehört.

Code: 404<br>
Nicht gefunden:

Code: 500<br>
Interner Serverfehler.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

### <a name="subscription-api"></a>Abonnement-API

Die Abonnement-API unterstützt die folgenden HTTPS-Vorgänge: **GET**, **POST**, **PATCH** und **DELETE**.


#### <a name="list-subscriptions"></a>Auflisten von Abonnements

Listet alle SaaS-Abonnements für einen Herausgeber auf.

##### <a name="getbrhttpsmarketplaceapimicrosoftcomapisaassubscriptionsapi-versionapiversion"></a>Get<br>`https://marketplaceapi.microsoft.com/api/saas/subscriptions?api-version=<ApiVersion>`

*Abfrageparameter:*

|             |                   |
|  --------   |  ---------------  |
| ApiVersion  |  Die Version des für diese Anforderung zu verwendenden Vorgangs.  |

*Anforderungsheader:*

|                    |                   |
|  ---------------   |  ---------------  |
| Content-Type       |  `application/json`  |
| x-ms-requestid     |  Ein eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Client, vorzugsweise eine GUID. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt. |
| x-ms-correlationid |  Ein eindeutiger Zeichenfolgenwert für den Vorgang auf dem Client. Dieser Parameter korreliert alle Ereignisse des Clientvorgangs mit serverseitigen Ereignissen. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.  |
| authorization      |  [Abrufen eines Bearertokens basierend auf der Azure AD-App](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app) Beispiel: „`Bearer <access_token>`“.  |

*Antwortcodes:*

Code: 200 <br/>
Ruft anhand des Authentifizierungstokens den Herausgeber und die entsprechenden Abonnements für alle Angebote des Herausgebers ab.

>[!Note]
>[Pseudo-APIs](#mock-apis) werden verwendet, wenn Sie das Angebot erstmalig entwickeln, während echte APIs verwendet werden müssen, wenn Sie das Angebot tatsächlich veröffentlichen.  Echte APIs und Pseudo-APIs unterscheiden sich in der ersten Codezeile.  In der echten API gibt es den Abschnitt `subscription`, während dieser Abschnitt in einer Pseudo-API nicht vorhanden ist.

Antwortnutzlast für Pseudo-API:<br>

```json
{
  [
      {
          "id": "<guid>",
          "name": "Contoso Cloud Solution",
          "publisherId": "contoso",
          "offerId": "offer1",
          "planId": "silver",
          "quantity": "10",
          "beneficiary": { // Tenant for which SaaS subscription is purchased.
              "tenantId": "<guid>"
          },
          "purchaser": { // Tenant that purchased the SaaS subscription. These could be different for reseller scenario
              "tenantId": "<guid>"
          },
            "term": {
                "startDate": "2019-05-31",
                "endDate": "2019-06-29",
                "termUnit": "P1M"
          },
          "allowedCustomerOperations": [
              "Read" // Possible Values: Read, Update, Delete.
          ], // Indicates operations allowed on the SaaS subscription. For CSP-initiated purchases, this will always be Read.
          "sessionMode": "None", // Possible Values: None, DryRun (Dry Run indicates all transactions run as Test-Mode in the commerce stack)
          "isFreeTrial": "true", // true - the customer subscription is currently in free trial, false - the customer subscription is not currently in free trial.
          "saasSubscriptionStatus": "Subscribed" // Indicates the status of the operation: [NotStarted, PendingFulfillmentStart, Subscribed, Suspended, Unsubscribed]
      }
  ],
  "continuationToken": ""
}
```
Und für eine echte API: <br>

```json
{
  "subscriptions": [
      {
          "id": "<guid>",
          "name": "Contoso Cloud Solution",
          "publisherId": "contoso",
          "offerId": "offer1",
          "planId": "silver",
          "quantity": "10",
          "beneficiary": { // Tenant, object id and email address for which SaaS subscription is purchased.
              "emailId": "<email>",
              "objectId": "<guid>",                     
              "tenantId": "<guid>"
          },
          "purchaser": { // Tenant, object id and email address that purchased the SaaS subscription. These could be different for reseller scenario
              "emailId": "<email>",
              "objectId": "<guid>",                      
              "tenantId": "<guid>"
          },
            "term": {
                "startDate": "2019-05-31",
                "endDate": "2019-06-29",
                "termUnit": "P1M"
          },
          "allowedCustomerOperations": [
              "Read" // Possible Values: Read, Update, Delete.
          ], // Indicates operations allowed on the SaaS subscription. For CSP-initiated purchases, this will always be Read.
          "sessionMode": "None", // Possible Values: None, DryRun (Dry Run indicates all transactions run as Test-Mode in the commerce stack)
          "isFreeTrial": true, // true - the customer subscription is currently in free trial, false - the customer subscription is not currently in free trial.(optional field - default false)
          "isTest": false, //indicating whether the current subscription is a test asset
          "sandboxType": "None", // Possible Values: None, Csp (Csp sandbox purchase)
          "saasSubscriptionStatus": "Subscribed" // Indicates the status of the operation: [NotStarted, PendingFulfillmentStart, Subscribed, Suspended, Unsubscribed]
      }
  ],
  "@nextLink": ""
}
```
Das Fortsetzungstoken ist nur vorhanden, wenn zusätzliche „Seiten“ von Plänen abgerufen werden sollen. 

Code: 403 <br>
Nicht autorisiert. Das Authentifizierungstoken wurde nicht bereitgestellt oder ist ungültig, oder die Anforderung versucht, auf einen Kauf zuzugreifen, der nicht zum aktuellen Herausgeber gehört. 

Code: 500<br>
Interner Serverfehler.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

#### <a name="get-subscription"></a>Abrufen eines Abonnements

Ruft das angegebene SaaS-Abonnement ab. Verwenden Sie diesen Aufruf zum Abrufen von Lizenzinformationen und Plan-/Tarifinformationen.

##### <a name="getbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidapi-versionapiversion"></a>Get<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId?api-version=<ApiVersion>`

*Abfrageparameter:*

|                    |                   |
|  ---------------   |  ---------------  |
| subscriptionId     |   Ein eindeutiger Bezeichner des SaaS-Abonnements, der nach dem Auflösen des Tokens über die Auflösungs-API abgerufen wird.   |
|  ApiVersion        |   Die Version des für diese Anforderung zu verwendenden Vorgangs.   |

*Anforderungsheader:*

|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Type      |  `application/json`  |
|  x-ms-requestid    |  Ein eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Client, vorzugsweise eine GUID. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt. |
|  x-ms-correlationid |  Ein eindeutiger Zeichenfolgenwert für den Vorgang auf dem Client. Dieser Parameter korreliert alle Ereignisse des Clientvorgangs mit serverseitigen Ereignissen. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.  |
|  authorization     |  [Abrufen eines Bearertokens basierend auf der Azure AD-App](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app) Beispiel: „`Bearer <access_token>`“.  |

*Antwortcodes:*

Code: 200<br>
Ruft das SaaS-Abonnement zur ID ab Antwortnutzlast:<br>

```json
Response Body:
{ 
        "id":"",
        "name":"Contoso Cloud Solution",
        "publisherId": "contoso",
        "offerId": "offer1",
        "planId": "silver",
        "quantity": "10",
          "beneficiary": { // Tenant for which SaaS subscription is purchased.
              "tenantId": "<guid>"
          },
          "purchaser": { // Tenant that purchased the SaaS subscription. These could be different for reseller scenario
              "tenantId": "<guid>"
          },
        "allowedCustomerOperations": ["Read"], // Indicates operations allowed on the SaaS subscription. For CSP-initiated purchases, this will always be Read.
        "sessionMode": "None", // Dry Run indicates all transactions run as Test-Mode in the commerce stack
        "isFreeTrial": "true", // true - customer subscription is currently in free trial, false - customer subscription is not currently in free trial.
        "status": "Subscribed", // Indicates the status of the operation.
          "term": { //This gives the free trial term start and end date
            "startDate": "2019-05-31",
            "endDate": "2019-06-29",
            "termUnit": "P1M" //where P1M: Monthly, P1Y: Yearly 
        },
}
```

Code: 403<br>
Nicht autorisiert. Das Authentifizierungstoken wurde nicht bereitgestellt oder ist ungültig, oder die Anforderung versucht, auf einen Kauf zuzugreifen, der nicht zum aktuellen Herausgeber gehört.

Code: 404<br>
Nicht gefunden:<br> 

Code: 500<br>
Interner Serverfehler.<br>

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }  
```

#### <a name="list-available-plans"></a>Auflisten verfügbarer Pläne

Verwenden Sie diesen Aufruf, um herauszufinden, ob es private oder öffentliche Angebote für den aktuellen Herausgeber gibt.

##### <a name="getbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidlistavailableplansapi-versionapiversion"></a>Get<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/listAvailablePlans?api-version=<ApiVersion>`

*Abfrageparameter:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |   Die Version des für diese Anforderung zu verwendenden Vorgangs.  |

*Anforderungsheader:*

|                    |                   |
|  ---------------   |  ---------------  |
|   Content-Type     |  `application/json` |
|   x-ms-requestid   |   Ein eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Client, vorzugsweise eine GUID. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt. |
|  x-ms-correlationid  | Ein eindeutiger Zeichenfolgenwert für den Vorgang auf dem Client. Dieser Parameter korreliert alle Ereignisse des Clientvorgangs mit serverseitigen Ereignissen. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt. |
|  authorization     |  [Abrufen eines Bearertokens basierend auf der Azure AD-App](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app)  Beispiel: „`Bearer <access_token>`“. |

*Antwortcodes:*

Code: 200<br>
Ruft eine Liste der verfügbaren Pläne für einen Kunden ab. Antworttext:

```json
{
    "plans": [{
        "planId": "Platinum001",
        "displayName": "Private platinum plan for Contoso",
        "isPrivate": true
    }]
}
```

Code: 404<br>
Nicht gefunden:<br> 

Code: 403<br>
Nicht autorisiert. Das Authentifizierungstoken wurde nicht bereitgestellt oder ist ungültig, oder die Anforderung versucht, auf einen Kauf zuzugreifen, der nicht zum aktuellen Herausgeber gehört. <br> 

Code: 500<br>
Interner Serverfehler.<br>

```json
{ 
    "error": { 
      "code": "UnexpectedError", 
      "message": "An unexpected error has occurred." 
    } 
```

#### <a name="activate-a-subscription"></a>Aktivieren eines Abonnements

##### <a name="postbrhttpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidactivateapi-versionapiversion"></a>Post<br>`https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/activate?api-version=<ApiVersion>`

*Abfrageparameter:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  Die Version des für diese Anforderung zu verwendenden Vorgangs.  |
| subscriptionId     | Ein eindeutiger Bezeichner des SaaS-Abonnements, der nach dem Auflösen des Tokens über die Auflösungs-API abgerufen wird.  |

*Anforderungsheader:*
 
|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Type      | `application/json`  |
|  x-ms-requestid    | Ein eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Client, vorzugsweise eine GUID. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.  |
|  x-ms-correlationid  | Ein eindeutiger Zeichenfolgenwert für den Vorgang auf dem Client. Diese Zeichenfolge korreliert alle Ereignisse des Clientvorgangs mit Ereignissen auf der Serverseite. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.  |
|  authorization     |  [Abrufen eines Bearertokens basierend auf der Azure AD-App](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app)  Beispiel: „`Bearer <access_token>`“. |

*Anforderungsnutzlast:*

```json
{
    "planId": "gold",
    "quantity": ""
}
```

*Antwortcodes:*

Code: 200<br>
Aktiviert das Abonnement.<br>

Code: 400<br>
Bad request: validation failures. (Ungültige Anforderung: Fehler bei der Überprüfung.)

Code: 403<br>
Nicht autorisiert. Das Authentifizierungstoken wurde nicht bereitgestellt oder ist ungültig, oder die Anforderung versucht, auf einen Kauf zuzugreifen, der nicht zum aktuellen Herausgeber gehört.

Code: 404<br>
Nicht gefunden:

Code: 500<br>
Interner Serverfehler.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

#### <a name="change-the-plan-on-the-subscription"></a>Ändern des Plans für das Abonnement

Aktualisieren Sie den Plan für das Abonnement.

##### <a name="patchbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidapi-versionapiversion"></a>Patch<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>?api-version=<ApiVersion>`

*Abfrageparameter:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  Die Version des für diese Anforderung zu verwendenden Vorgangs.  |
| subscriptionId     | Ein eindeutiger Bezeichner des SaaS-Abonnements, der nach dem Auflösen des Tokens über die Auflösungs-API abgerufen wird.  |

*Anforderungsheader:*

|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Type      | `application/json` |
|  x-ms-requestid    |   Ein eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Client, vorzugsweise eine GUID. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.  |
|  x-ms-correlationid  |  Ein eindeutiger Zeichenfolgenwert für den Vorgang auf dem Client. Dieser Parameter korreliert alle Ereignisse des Clientvorgangs mit serverseitigen Ereignissen. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.    |
| authorization      |  [Abrufen eines Bearertokens basierend auf der Azure AD-App](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app)  Beispiel: „`Bearer <access_token>`“.  |

*Anforderungsnutzlast:*

```json
Request Body:
{
    "planId": "gold"
}
```

*Anforderungsheader:*

|                    |                   |
|  ---------------   |  ---------------  |
| Operation-Location | Der Link zu einer Ressource, über den der Vorgangsstatus abgerufen werden kann.   |

*Antwortcodes:*

Code: 202<br>
Die Anforderung zum Ändern des Plans wurde akzeptiert. Vom Partner wird erwartet, dass er „Operation-Location“ abfragt, um zu ermitteln, ob der Vorgang erfolgreich war oder fehlgeschlagen ist. <br>

Code: 400<br>
Bad request: validation failures. (Ungültige Anforderung: Fehler bei der Überprüfung.)

Code: 403<br>
Nicht autorisiert. Das Authentifizierungstoken wurde nicht bereitgestellt oder ist ungültig, oder die Anforderung versucht, auf einen Kauf zuzugreifen, der nicht zum aktuellen Herausgeber gehört.

Code: 404<br>
Nicht gefunden:

Code: 500<br>
Interner Serverfehler.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

>[!Note]
>Es kann jeweils nur ein Plan oder eine Menge geändert werden, nicht beides gleichzeitig. Es wird ein Abonnement bearbeitet, bei dem **Aktualisieren** nicht zu den `allowedCustomerOperations` gehört.

#### <a name="change-the-quantity-on-the-subscription"></a>Ändern der Menge für das Abonnement

Aktualisieren Sie die Menge für das Abonnement.

##### <a name="patchbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidapi-versionapiversion"></a>Patch:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>?api-version=<ApiVersion>`

*Abfrageparameter:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  Die Version des für diese Anforderung zu verwendenden Vorgangs.  |
| subscriptionId     | Ein eindeutiger Bezeichner des SaaS-Abonnements, der nach dem Auflösen des Tokens über die Auflösungs-API abgerufen wird.  |

*Anforderungsheader:*

|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Type      | `application/json` |
|  x-ms-requestid    |   Ein eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Client, vorzugsweise eine GUID. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.  |
|  x-ms-correlationid  |  Ein eindeutiger Zeichenfolgenwert für den Vorgang auf dem Client. Dieser Parameter korreliert alle Ereignisse des Clientvorgangs mit serverseitigen Ereignissen. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.    |
| authorization      |  [Abrufen eines Bearertokens basierend auf der Azure AD-App](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app)  Beispiel: „`Bearer <access_token>`“.  |

*Anforderungsnutzlast:*

```json
Request Body:
{
    "quantity": 5
}
```

*Anforderungsheader:*

|                    |                   |
|  ---------------   |  ---------------  |
| Operation-Location | Link zu einer Ressource, um den Vorgangsstatus abzurufen.   |

*Antwortcodes:*

Code: 202<br>
Die Anforderung zum Ändern der Menge wurde akzeptiert. Vom Partner wird erwartet, dass er „Operation-Location“ abfragt, um zu ermitteln, ob der Vorgang erfolgreich war oder fehlgeschlagen ist. <br>

Code: 400<br>
Bad request: validation failures. (Ungültige Anforderung: Fehler bei der Überprüfung.)


Code: 403<br>
Nicht autorisiert. Das Authentifizierungstoken wurde nicht bereitgestellt oder ist ungültig, oder die Anforderung versucht, auf einen Kauf zuzugreifen, der nicht zum aktuellen Herausgeber gehört.

Code: 404<br>
Nicht gefunden:

Code: 500<br>
Interner Serverfehler.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

>[!Note]
>Es kann jeweils nur ein Plan oder eine Menge geändert werden, nicht beides gleichzeitig. Es wird ein Abonnement bearbeitet, bei dem **Aktualisieren** nicht zu den `allowedCustomerOperations` gehört.

#### <a name="delete-a-subscription"></a>Löschen eines Abonnements

Kündigen und löschen Sie das angegebene Abonnement.

##### <a name="deletebr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionid-api-versionapiversion"></a>Löschen<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId> ?api-version=<ApiVersion>`

*Abfrageparameter:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  Die Version des für diese Anforderung zu verwendenden Vorgangs.  |
| subscriptionId     | Ein eindeutiger Bezeichner des SaaS-Abonnements, der nach dem Auflösen des Tokens über die Auflösungs-API abgerufen wird.  |

*Anforderungsheader:*
 
|                    |                   |
|  ---------------   |  ---------------  |
|   Content-Type     |  `application/json` |
|  x-ms-requestid    |   Ein eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Client, vorzugsweise eine GUID. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.   |
|  x-ms-correlationid  |  Ein eindeutiger Zeichenfolgenwert für den Vorgang auf dem Client. Dieser Parameter korreliert alle Ereignisse des Clientvorgangs mit serverseitigen Ereignissen. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.   |
|  authorization     |  [Abrufen eines Bearertokens basierend auf der Azure AD-App](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app)  Beispiel: „`Bearer <access_token>`“.  |

*Antwortcodes:*

Code: 202<br>
The partner initiated a call to unsubscribe a SaaS subscription. (Der Partner hat einen Aufruf initiiert, um ein SaaS-Abonnement zu kündigen.)<br>

Code: 400<br>
Es wird ein Abonnement gelöscht, bei dem **DELETE** nicht zu den `allowedCustomerOperations` gehört.

Code: 403<br>
Nicht autorisiert. Das Authentifizierungstoken wurde nicht bereitgestellt oder ist ungültig, oder die Anforderung versucht, auf einen Kauf zuzugreifen, der nicht zum aktuellen Herausgeber gehört.

Code: 404<br>
Nicht gefunden:

Code: 500<br>
Interner Serverfehler.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```


### <a name="operations-api"></a>Vorgangs-API

Die Vorgangs-API unterstützt die folgenden PATCH- und GET-Vorgänge.

#### <a name="list-outstanding-operations"></a>Auflisten ausstehender Vorgänge 

Hiermit werden die ausstehenden Vorgänge für den aktuellen Herausgeber aufgelistet. 

##### <a name="getbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidoperationsapi-versionapiversion"></a>Get<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/operations?api-version=<ApiVersion>`

*Abfrageparameter:*

|             |        |
|  ---------------   |  ---------------  |
|    ApiVersion                |   Die Version des für diese Anforderung zu verwendenden Vorgangs.                |
| subscriptionId     | Ein eindeutiger Bezeichner des SaaS-Abonnements, der nach dem Auflösen des Tokens über die Auflösungs-API abgerufen wird.  |

*Anforderungsheader:*
 
|                    |                   |
|  ---------------   |  ---------------  |
|   Content-Type     |  `application/json` |
|  x-ms-requestid    |  Ein eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Client, vorzugsweise eine GUID. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.  |
|  x-ms-correlationid |  Ein eindeutiger Zeichenfolgenwert für den Vorgang auf dem Client. Dieser Parameter korreliert alle Ereignisse des Clientvorgangs mit serverseitigen Ereignissen. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.  |
|  authorization     |  [Abrufen eines Bearertokens basierend auf der Azure AD-App](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app)  Beispiel: „`Bearer <access_token>`“.  |

*Antwortcodes:*

Code: 200<br> Ruft die Liste der ausstehenden Vorgänge für ein Abonnement ab. Antwortnutzlast:

```json
[{
    "id": "<guid>",  
    "activityId": "<guid>",
    "subscriptionId": "<guid>",
    "offerId": "offer1",
    "publisherId": "contoso",  
    "planId": "silver",
    "quantity": "20",
    "action": "Convert",
    "timeStamp": "2018-12-01T00:00:00",  
    "status": "NotStarted"  
}]
```


Code: 400<br>
Bad request: validation failures. (Ungültige Anforderung: Fehler bei der Überprüfung.)

Code: 403<br>
Nicht autorisiert. Das Authentifizierungstoken wurde nicht bereitgestellt oder ist ungültig, oder die Anforderung versucht, auf einen Kauf zuzugreifen, der nicht zum aktuellen Herausgeber gehört.

Code: 404<br>
Nicht gefunden:

Code: 500<br>
Interner Serverfehler.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}

```

#### <a name="get-operation-status"></a>Abrufen des Vorgangsstatus

Ermöglicht dem Herausgeber das Nachverfolgen des Status des angegebenen ausgelösten asynchronen Vorgangs (z. B. `Subscribe`, `Unsubscribe`, `ChangePlan` oder `ChangeQuantity`).

##### <a name="getbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidoperationsoperationidapi-versionapiversion"></a>Get<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/operations/<operationId>?api-version=<ApiVersion>`

*Abfrageparameter:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  Die Version des für diese Anforderung zu verwendenden Vorgangs.  |

*Anforderungsheader:*

|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Type      |  `application/json`   |
|  x-ms-requestid    |   Ein eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Client, vorzugsweise eine GUID. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.  |
|  x-ms-correlationid |  Ein eindeutiger Zeichenfolgenwert für den Vorgang auf dem Client. Dieser Parameter korreliert alle Ereignisse des Clientvorgangs mit serverseitigen Ereignissen. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt.  |
|  authorization     |  [Abrufen eines Bearertokens basierend auf der Azure AD-App](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app) Beispiel: „`Bearer <access_token>`“.  |

*Antwortcodes:*<br>

Code: 200<br> Ruft den angegebenen ausstehenden SaaS-Vorgang ab Antwortnutzlast:

```json
Response body:
{
    "id  ": "<guid>",
    "activityId": "<guid>",
    "subscriptionId":"<guid>",
    "offerId": "offer1",
    "publisherId": "contoso",  
    "planId": "silver",
    "quantity": "20",
    "action": "Convert",
    "timeStamp": "2018-12-01T00:00:00",
    "status": "NotStarted"
}

```

Code: 400<br>
Bad request: validation failures. (Ungültige Anforderung: Fehler bei der Überprüfung.)

Code: 403<br>
Nicht autorisiert. Das Authentifizierungstoken wurde nicht bereitgestellt oder ist ungültig, oder die Anforderung versucht, auf einen Kauf zuzugreifen, der nicht zum aktuellen Herausgeber gehört.
 
Code: 404<br>
Nicht gefunden:

Code: 500<br> Interner Serverfehler.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}

```
#### <a name="update-the-status-of-an-operation"></a>Aktualisieren des Status eines Vorgangs

Aktualisieren Sie den Status eines Vorgangs unter Verwendung der bereitgestellten Werte, um anzugeben, ob der Vorgang erfolgreich war oder fehlgeschlagen ist.

##### <a name="patchbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidoperationsoperationidapi-versionapiversion"></a>Patch<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/operations/<operationId>?api-version=<ApiVersion>`

*Abfrageparameter:*

|                    |                   |
|  ---------------   |  ---------------  |
|   ApiVersion       |  Die Version des für diese Anforderung zu verwendenden Vorgangs.  |
| subscriptionId     | Ein eindeutiger Bezeichner des SaaS-Abonnements, der nach dem Auflösen des Tokens über die Auflösungs-API abgerufen wird.  |
|  operationId       | Der Vorgang, der abgeschlossen wird. |

*Anforderungsheader:*

|                    |                   |
|  ---------------   |  ---------------  |
|   Content-Type     | `application/json`   |
|   x-ms-requestid   |   Ein eindeutiger Zeichenfolgenwert für die Nachverfolgung der Anforderung vom Client, vorzugsweise eine GUID. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt. |
|  x-ms-correlationid |  Ein eindeutiger Zeichenfolgenwert für den Vorgang auf dem Client. Dieser Parameter korreliert alle Ereignisse des Clientvorgangs mit serverseitigen Ereignissen. Wenn dieser Wert nicht angegeben wird, wird einer generiert und in den Antwortheadern bereitgestellt. |
|  authorization     |  [Abrufen eines Bearertokens basierend auf der Azure AD-App](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app)  Beispiel: „`Bearer <access_token>`“.  |

*Anforderungsnutzlast:*

```json
{
    "planId": "offer1",
    "quantity": "44",
    "status": "Success"    // Allowed Values: Success/Failure. Indicates the status of the operation.
}

```

*Antwortcodes:*

Code: 200<br> Ein Aufruf, um über den Abschluss eines Vorgangs auf der Seite des Partners zu informieren. Diese Antwort kann z. B. die Änderung der Anzahl der Arbeitsplätze oder Pläne anzeigen.

Code: 400<br>
Bad request: validation failures. (Ungültige Anforderung: Fehler bei der Überprüfung.)

Code: 403<br>
Nicht autorisiert. Das Authentifizierungstoken wurde nicht bereitgestellt oder ist ungültig, oder die Anforderung versucht, auf einen Kauf zuzugreifen, der nicht zum aktuellen Herausgeber gehört.

Code: 404<br>
Nicht gefunden:

Code: 409<br>
Konflikt. Beispiel: Eine neuere Transaktion wurde bereits abgeschlossen.

Code: 500<br> Interner Serverfehler.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}

```

## <a name="implementing-a-webhook-on-the-saas-service"></a>Implementieren eines Webhooks für den SaaS-Dienst

Der Herausgeber muss in diesen SaaS-Dienst einen Webhook implementieren, um proaktiv Benutzer über Änderungen beim Dienst zu benachrichtigen. Der SaaS-Dienst muss die Vorgangs-API zur Überprüfung und Autorisierung aufrufen, bevor er auf die Webhookbenachrichtigung reagieren kann.


```json
{
  "id": "<this is a GUID operation id, you can call operations API with this to get status>",
  "activityId": "<this is a Guid correlation id>",
  "subscriptionId": "<Guid to uniquely identify this resource>",
  "publisherId": "<this is the publisher's name>",
  "offerId": "<this is the offer name>",
  "planId": "<this is the plan id>",
  "quantity": "<the number of seats, will be null if not per-seat saas offer>",
  "timeStamp": "2019-04-15T20:17:31.7350641Z",
  "action": "Unsubscribe",
  "status": "NotStarted"  

}
```
Eine der folgenden Aktionen kann verwendet werden: 
- `Unsubscribe` (wenn die Ressource gelöscht wurde)
- `ChangePlan` (wenn der Vorgang zum Ändern des Plans abgeschlossen wurde)
- `ChangeQuantity` (wenn der Vorgang zum Ändern der Menge abgeschlossen wurde)
- `Suspend` (wenn die Ressource angehalten wurde)
- `Reinstate` (wenn die Ressource nach dem Anhalten reaktiviert wurde)

Folgende Statuswerte sind möglich: 
- **NotStarted** <br>
 - **InProgress** <br>
- **Erfolgreich** <br>
- **Fehler** <br>
- **Conflict** <br>

Handlungsrelevante Status in einer Webhookbenachrichtigung sind **Succeeded** und **Failed**. Der Lebenszyklus eines Vorgangs verläuft von **NotStarted** bis zu einem Endstatus wie **Succeeded**, **Failed** oder **Conflict**. Wenn **NotStarted** oder **InProgress** angezeigt wird, fordern Sie den Status so lange über eine GET-API an, bis der Vorgang einen Endstatus erreicht, bevor Sie Maßnahmen ergreifen. 

## <a name="mock-apis"></a>Modell-APIs

Für erste Entwicklungsschritte, insbesondere zum Erstellen von Prototypen und zum Testen von Projekten, können Sie unsere Modell-APIs verwenden. 

Hostendpunkt: `https://marketplaceapi.microsoft.com/api` (keine Authentifizierung erforderlich)<br/>
API-Version: `2018-09-15`<br/>
Beispiel-URI: `https://marketplaceapi.microsoft.com/api/saas/subscriptions?api-version=2018-09-15` <br/>

Die API-Endpunktpfade für Modell-APIs und echte APIs sind identisch, aber die API-Versionen unterscheiden sich. Für die Modellversion wird `2018-09-15` und für die Produktionsversion wird `2018-08-31` verwendet. 

Alle API-Aufrufe in diesem Artikel können an den Endpunkt des Modellhosts gerichtet werden. In der Regel werden Modelldaten als Antwort zurückgegeben. Für Aufrufe der Methoden zum Aktualisieren des Abonnements gibt die Modell-API immer 500 zurück. 

## <a name="next-steps"></a>Nächste Schritte

Entwicklern steht außerdem das programmgesteuerte Abrufen und Bearbeiten von Workloads, Angeboten und Herausgeberprofilen mit den [REST-APIs für das Cloud-Partnerportal](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview) zur Verfügung.
