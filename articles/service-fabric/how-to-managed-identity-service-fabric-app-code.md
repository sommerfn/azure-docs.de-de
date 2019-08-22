---
title: 'Azure Service Fabric: Verwenden von verwalteten Identitäten mit Service Fabric-Anwendungen | Microsoft-Dokumentation'
description: Verwenden von verwalteten Identitäten in Service Fabric-Anwendungscode
services: service-fabric
author: athinanthny
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.date: 7/25/2019
ms.author: atsenthi
ms.openlocfilehash: 528e1b0a353cdcd716f9bca63c423af7a6f12641
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/12/2019
ms.locfileid: "68957515"
---
# <a name="how-to-leverage-a-service-fabric-applications-managed-identity-to-access-azure-services"></a>Nutzen der verwalteten Identität einer Service Fabric-Anwendung für den Zugriff auf Azure-Dienste

Service Fabric-Anwendungen können verwaltete Identitäten für den Zugriff auf andere Azure-Ressourcen verwenden, die die Azure Active Directory-basierte Authentifizierung unterstützen. Eine Anwendung kann ein [Zugriffstoken](../active-directory/develop/developer-glossary.md#access-token) abrufen, das ihre system- oder benutzerseitig zugewiesene Identität repräsentiert. Die Anwendung kann dieses Token als Bearertoken verwenden, um sich bei einem anderen Dienst zu authentifizieren – dieser wird auch als [geschützter Ressourcenserver](../active-directory/develop/developer-glossary.md#resource-server) bezeichnet. Das Token repräsentiert die Identität, die der Service Fabric-Anwendung zugewiesen wurde, und wird nur für Azure-Ressourcen (einschließlich Service Fabric-Anwendungen) ausgestellt, die diese Identität gemeinsam nutzen. Eine detaillierte Beschreibung verwalteter Identitäten sowie Informationen zur Unterscheidung zwischen system- und benutzerseitig zugewiesenen Identitäten finden Sie in der [Übersicht über verwaltete Identitäten](../active-directory/managed-identities-azure-resources/overview.md). Im vorliegenden Artikel wird eine Service Fabric-Anwendung, die verwaltete Identitäten nutzen kann, als [Clientanwendung](../active-directory/develop/developer-glossary.md#client-application) bezeichnet.

> [!IMPORTANT]
> Eine verwaltete Identität stellt die Zuordnung zwischen einer Azure-Ressource und einem Dienstprinzipal im entsprechenden Azure AD-Mandanten dar, der dem Abonnement zugeordnet ist, das die Ressource enthält. Im Kontext von Service Fabric werden verwaltete Identitäten daher nur für Anwendungen unterstützt, die als Azure-Ressourcen bereitgestellt wurden. 

> [!IMPORTANT]
> Der Clientanwendung muss Zugriff auf die geschützte Ressource gewährt werden, bevor diese Anwendung die verwaltete Identität einer Service Fabric-Anwendung nutzen kann. Informationen zur Unterstützung finden Sie in der Liste der [Azure-Dienste, die die Azure AD-Authentifizierung unterstützen](/active-directory/managed-identities-azure-resources/services-support-managed-identities#azure-services-that-support-managed-identities-for-azure-resources). In der Dokumentation des jeweiligen Diensts finden Sie dann die spezifischen Schritte, um einer Identität Zugriff auf relevante Ressourcen zu gewähren. 

## <a name="acquiring-an-access-token-using-rest-api"></a>Abrufen eines Zugriffstokens mithilfe der REST-API
In Clustern, die für die Nutzung verwalteter Identitäten eingerichtet wurden, macht die Service Fabric-Runtime einen Localhost-Endpunkt verfügbar, den Anwendungen zum Abrufen von Zugriffstoken verwenden können. Der Endpunkt ist auf jedem Knoten im Cluster verfügbar, und alle Entitäten auf den Knoten können darauf zugreifen. Autorisierte Aufrufer können Zugriffstoken durch Aufrufen dieses Endpunkts und Bereitstellen eines Authentifizierungscodes abrufen. Der Code wird von der Service Fabric-Runtime für jede einzelne Aktivierung eines Dienstcodepakets generiert und ist an die Lebensdauer des Prozesses gebunden, der dieses Dienstcodepaket hostet.

Die Umgebung eines Service Fabric-Diensts, der verwaltete Identitäten nutzen kann, wird zu Beginn mit folgenden Variablen eingerichtet:
- „MSI_ENDPOINT“: der Localhost-Endpunkt einschließlich Pfad, API-Version und Parametern, die der verwalteten Identität dieses Diensts entsprechen
- „MSI_SECRET“: ein Authentifizierungscode, bei dem es sich um eine nicht transparente Zeichenfolge handelt, die den Dienst auf dem aktuellen Knoten eindeutig repräsentiert

> [!NOTE]
> Die Namen „MSI_ENDPOINT“ und „MSI_SECRET“ beziehen sich auf die frühere Bezeichnung für verwaltete Identitäten („Managed Service Identity“ [Verwaltete Dienstidentität]), die jetzt als veraltet markiert ist. Die Namen stimmen auch mit den Namen der entsprechenden Umgebungsvariablen überein, die von anderen Azure-Diensten mit Unterstützung verwalteter Identitäten verwendet werden.

> [!IMPORTANT]
> Der Anwendungscode sollte den Wert der Umgebungsvariablen „MSI_SECRET“ als vertraulich behandeln. Dieser Wert darf nicht protokolliert oder anderweitig verteilt werden. Der Authentifizierungscode hat außerhalb des lokalen Knotens oder nach Beendigung des Prozesses, der den Dienst hostet, keinen Wert, aber er repräsentiert die Identität des Service Fabric-Diensts und sollte daher mit denselben Vorsichtsmaßnahmen behandelt werden wie das Zugriffstoken selbst.

Zum Abrufen eines Tokens führt der Client die folgenden Schritte aus:
- Erstellen eines URI durch Verketten des Endpunkts der verwalteten Identität (Wert von „MSI_ENDPOINT“) mit der API-Version und der Ressource (Zielgruppe), die für das Token erforderlich ist
- Erstellen einer HTTP-GET-Anforderung für den angegebenen URI
- Hinzufügen des Authentifizierungscodes (Wert von „MSI_SECRET“) zur Anforderung als Header
- Übermitteln der Anforderung

Eine erfolgreiche Antwort enthält eine JSON-Nutzlast, die das resultierende Zugriffstoken repräsentiert, sowie Metadaten mit einer Beschreibung. Eine nicht erfolgreiche Antwort enthält auch eine Erläuterung des Fehlers. Weitere Informationen zur Fehlerbehandlung finden Sie im Folgenden.

Zugriffstoken werden von Service Fabric auf verschiedenen Ebenen zwischengespeichert (Knoten, Cluster, Ressourcenanbieterdienst), daher impliziert eine erfolgreiche Antwort nicht notwendigerweise, dass das Token direkt als Reaktion auf die Anforderung der Benutzeranwendung ausgestellt wurde. Token werden für einen kürzeren Zeitraum als ihre Lebensdauer zwischengespeichert, sodass garantiert ist, dass eine Anwendung ein gültiges Token empfängt. Es empfiehlt sich, dass der Anwendungscode jedes abgerufene Zugriffstoken selbst zwischenspeichert. Der Cacheschlüssel sollte die Zielgruppe (bzw. eine Ableitung davon) enthalten. 


Beispiel für eine Anforderung:
```http
GET 'http://localhost:2377/metadata/identity/oauth2/token?api-version=2019-07-01-preview&resource=https://keyvault.azure.com/' HTTP/1.1 Secret: 912e4af7-77ba-4fa5-a737-56c8e3ace132
```
Hierbei gilt:

| Element | BESCHREIBUNG |
| ------- | ----------- |
| `GET` | Das HTTP-Verb, mit dem angegeben wird, dass Sie Daten vom Endpunkt abrufen möchten. In diesem Fall ist dies ein OAuth-Zugriffstoken. | 
| `http://localhost:2377/metadata/identity/oauth2/token` | Der Endpunkt für verwaltete Identitäten für Service Fabric-Anwendungen, bereitgestellt über die MSI_ENDPOINT-Umgebungsvariable. |
| `api-version` | Ein Abfragezeichenfolgenparameter, der die API-Version des Tokendiensts für verwaltete Identitäten angibt. Zurzeit ist `2019-07-01-preview` der einzige akzeptierte Wert. Änderungen vorbehalten. |
| `resource` | Ein Abfragezeichenfolgenparameter, der den App-ID-URI der Zielressource angibt. Dieser wird als `aud`-Anspruch (Zielgruppe) des ausgestellten Tokens angezeigt. Dieses Beispiel fordert ein Token für den Zugriff auf eine Azure Key Vault-Instanz mit dem App-ID-URI https://keyvault.azure.com/ an. |
| `Secret` | Ein HTTP-Anforderungsheaderfeld, das für den Service Fabric-Tokendienst für verwaltete Identitäten erforderlich ist, damit Service Fabric-Dienste den Aufrufer authentifizieren können. Dieser Wert wird von der Service Fabric-Runtime über die Umgebungsvariable „MSI_SECRET“ bereitgestellt. |


Beispiel für eine Antwort:
```json
HTTP/1.1 200 OK
Content-Type: application/json
{
    "token_type":  "Bearer",
    "access_token":  "eyJ0eXAiO...",
    "expires_on":  1565244611,
    "resource":  "https://keyvault.azure.com/"
}
```
Hierbei gilt:

| Element | BESCHREIBUNG |
| ------- | ----------- |
| `token_type` | Der Typ des Tokens, in diesem Fall ein Zugriffstoken des Typs „Bearer“. Dies bedeutet, dass das Element, das dieses Token vorlegt (der „Bearer“), der beabsichtigte Antragsteller des Tokens ist. |
| `access_token` | Das angeforderte Zugriffstoken. Beim Aufrufen einer geschützten REST-API wird das Token als „Bearertoken“ in das `Authorization`-Anforderungsheader-Feld eingebettet, damit der Aufrufer von der API authentifiziert werden kann. | 
| `expires_on` | Der Zeitstempel des Ablaufs des Zugriffstokens. Dieser wird als Anzahl von Sekunden ab „1970-01-01T0:0:0Z UTC“ dargestellt und entspricht dem `exp`-Anspruch des Tokens. In diesem Fall lautet der Ablaufzeitstempel des Tokens 2019-08-08T06:10:11+00:00 (in RFC 3339).|
| `resource` | Die Ressource, für die das Zugriffstoken ausgestellt wurde, angegeben über den Abfragezeichenfolgenparameter `resource` der Anforderung. Entspricht dem aud-Anspruch des Tokens. |


## <a name="acquiring-an-access-token-using-c"></a>Abrufen eines Zugriffstokens mithilfe von C#
Das oben erläuterte Beispiel sieht in C# folgendermaßen aus:

```C#
namespace Azure.ServiceFabric.ManagedIdentity.Samples
{
    using System;
    using System.Net.Http;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using System.Web;

    /// <summary>
    /// Sample class demonstrating access token acquisition using Managed Identity.
    /// </summary>
    public sealed class AccessTokenAcquirer
    {
        /// <summary>
        /// Acquire an access token.
        /// </summary>
        /// <returns>Access token</returns>
        public static async Task<string> AcquireAccessTokenAsync()
        {
            var managedIdentityEndpoint = Environment.GetEnvironmentVariable("MSI_ENDPOINT");
            var managedIdentityAuthenticationCode = Environment.GetEnvironmentVariable("MSI_SECRET");
            var managedIdentityAuthenticationHeader = "secret";
            var managedIdentityApiVersion = "2019-07-01-preview";
            var resource = "https://management.azure.com/";

            var requestUri = $"{managedIdentityEndpoint}?api-version={managedIdentityApiVersion}&resource={HttpUtility.UrlEncode(resource)}";

            var requestMessage = new HttpRequestMessage(HttpMethod.Get, requestUri);
            requestMessage.Headers.Add(managedIdentityAuthenticationHeader, managedIdentityAuthenticationCode);

            try
            {
                var response = await new HttpClient().SendAsync(requestMessage)
                    .ConfigureAwait(false);

                response.EnsureSuccessStatusCode();

                var accessToken = await response.Content.ReadAsStringAsync()
                    .ConfigureAwait(false);

                return accessToken.Trim('"');
            }
            catch (Exception ex)
            {
                string errorText = String.Format("{0} \n\n{1}", ex.Message, ex.InnerException != null ? ex.InnerException.Message : "Acquire token failed");

                Console.WriteLine(errorText);
            }

            return String.Empty;
        }
    } // class AccessTokenAcquirer
} // namespace Azure.ServiceFabric.ManagedIdentity.Samples
```
## <a name="accessing-key-vault-from-a-service-fabric-application-using-managed-identity"></a>Zugreifen auf Key Vault über eine Service Fabric-Anwendung mithilfe einer verwalteten Identität
Dieses Beispiel baut auf dem oben erläuterten auf und veranschaulicht den Zugriff mithilfe einer verwalteten Identität auf ein Geheimnis, das in einer Key Vault-Instanz gespeichert ist.

```C#
        /// <summary>
        /// Probe the specified secret, displaying metadata on success.  
        /// </summary>
        /// <param name="vault">vault name</param>
        /// <param name="secret">secret name</param>
        /// <param name="version">secret version id</param>
        /// <returns></returns>
        public async Task<string> ProbeSecretAsync(string vault, string secret, string version)
        {
            // initialize a KeyVault client with a managed identity-based authentication callback
            var kvClient = new Microsoft.Azure.KeyVault.KeyVaultClient(new Microsoft.Azure.KeyVault.KeyVaultClient.AuthenticationCallback((a, r, s) => { return AuthenticationCallbackAsync(a, r, s); }));

            Console.WriteLine($"\nRunning with configuration: \n\tobserved vault: {config.VaultName}\n\tobserved secret: {config.SecretName}\n\tMI endpoint: {config.ManagedIdentityEndpoint}\n\tMI auth code: {config.ManagedIdentityAuthenticationCode}\n\tMI auth header: {config.ManagedIdentityAuthenticationHeader}");
            string response = String.Empty;

            Console.WriteLine("\n== Probing secret...");
            try
            {
                var secretResponse = await kvClient.GetSecretWithHttpMessagesAsync(vault, secret, version)
                    .ConfigureAwait(false);

                if (secretResponse.Response.IsSuccessStatusCode)
                {
                    // use the secret: secretValue.Body.Value;
                    var secretMetadata = secretResponse.Body.ToString();
                    Console.WriteLine($"Successfully probed secret '{secret}' in vault '{vault}': {PrintSecretBundleMetadata(secretResponse.Body)}");
                }
                else
                {
                    Console.WriteLine($"Non-critical error encountered retrieving secret '{secret}' in vault '{vault}': {secretResponse.Response.ReasonPhrase} ({secretResponse.Response.StatusCode})");
                }
            }
            catch (Microsoft.Rest.ValidationException ve)
            {
                response = String.Format($"encountered REST validation exception 0x{ve.HResult.ToString("X")} trying to access '{secret}' in vault '{vault}' from {ve.Source}: {ve.Message}");
            }
            catch (KeyVaultErrorException kvee)
            {
                response = String.Format($"encountered KeyVault exception 0x{kvee.HResult.ToString("X")} trying to access '{secret}' in vault '{vault}': {kvee.Response.ReasonPhrase} ({kvee.Response.StatusCode})");
            }
            catch (Exception ex)
            {
                // handle generic errors here
                response = String.Format($"encountered exception 0x{ex.HResult.ToString("X")} trying to access '{secret}' in vault '{vault}': {ex.Message}");
            }

            Console.WriteLine(response);

            return response;
        }

        /// <summary>
        /// KV authentication callback, using the application's managed identity.
        /// </summary>
        /// <param name="authority"></param>
        /// <param name="resource"></param>
        /// <param name="scope"></param>
        /// <returns>Access token</returns>
        public async Task<string> AuthenticationCallbackAsync(string authority, string resource, string scope)
        {
            if (config.DoVerboseLogging)
                Console.WriteLine($"authentication callback invoked with: auth: {authority}, resource: {resource}, scope: {scope}");

            var requestUri = $"{config.ManagedIdentityEndpoint}?api-version={config.ManagedIdentityApiVersion}&resource={HttpUtility.UrlEncode(resource)}";
            if (config.DoVerboseLogging)
                Console.WriteLine($"request uri: {requestUri}");

            var requestMessage = new HttpRequestMessage(HttpMethod.Get, requestUri);
            requestMessage.Headers.Add(config.ManagedIdentityAuthenticationHeader, config.ManagedIdentityAuthenticationCode);
            if (config.DoVerboseLogging)
                Console.WriteLine($"added header '{config.ManagedIdentityAuthenticationHeader}': '{config.ManagedIdentityAuthenticationCode}'");

            var response = await httpClient.SendAsync(requestMessage)
                .ConfigureAwait(false);
            if (config.DoVerboseLogging)
                Console.WriteLine($"response status: success: {response.IsSuccessStatusCode}, status: {response.StatusCode}");

            response.EnsureSuccessStatusCode();

            var accessToken = await response.Content.ReadAsStringAsync()
                .ConfigureAwait(false);

            if (config.DoVerboseLogging)
                Console.WriteLine("returning access code..");

            return accessToken.Trim('"');
        }

        private string PrintSecretBundleMetadata(SecretBundle bundle)
        {
            StringBuilder strBuilder = new StringBuilder();

            strBuilder.AppendFormat($"\n\tid: {bundle.Id}\n");
            strBuilder.AppendFormat($"\tcontent type: {bundle.ContentType}\n");
            strBuilder.AppendFormat($"\tmanaged: {bundle.Managed}\n");
            strBuilder.AppendFormat($"\tattributes:\n");
            strBuilder.AppendFormat($"\t\tenabled: {bundle.Attributes.Enabled}\n");
            strBuilder.AppendFormat($"\t\tnbf: {bundle.Attributes.NotBefore}\n");
            strBuilder.AppendFormat($"\t\texp: {bundle.Attributes.Expires}\n");
            strBuilder.AppendFormat($"\t\tcreated: {bundle.Attributes.Created}\n");
            strBuilder.AppendFormat($"\t\tupdated: {bundle.Attributes.Updated}\n");
            strBuilder.AppendFormat($"\t\trecoveryLevel: {bundle.Attributes.RecoveryLevel}\n");

            return strBuilder.ToString();
        }
```

## <a name="error-handling"></a>Fehlerbehandlung
Das Statuscodefeld des HTTP-Anforderungsheaders gibt den Erfolgsstatus der Anforderung an: Der Status „200 OK“ weist auf eine erfolgreiche Ausführung hin, und die Antwort enthält das Zugriffstoken, wie oben beschrieben. Im Folgenden finden Sie eine kurze Auflistung möglicher Fehlerantworten.

| Statuscode | Fehlerursache | Fehlerbehandlung |
| ----------- | ------------ | ------------- |
| 404 – Nicht gefunden. | Es liegt ein unbekannter Authentifizierungscode vor, oder der Anwendung wurde keine verwaltete Identität zugewiesen. | Korrigieren Sie das Setup der Anwendung oder den Code für den Tokenabruf. |
| 429: Zu viele Anforderungen. |  Der von Azure Active Directory oder Service Fabric auferlegte Drosselungsgrenzwert wurde erreicht. | Wiederholungsversuch mit exponentiellem Backoff. Siehe Anleitung unten. |
| 4xx – Fehler in der Anforderung. | Mindestens einer der Anforderungsparameter war falsch. | Wiederholen Sie den Vorgang nicht.  Überprüfen Sie den Fehler, um weitere Informationen zu erhalten.  4xx-Fehler sind Fehler während der Entwurfszeit.|
| 5xx-Fehler aus dem Dienst. | Das Subsystem der verwalteten Identität oder Azure Active Directory hat einen vorübergehenden Fehler zurückgegeben. | Der Vorgang kann nach kurzer Zeit wiederholt werden. Möglicherweise tritt beim erneuten Versuch eine Drosselungsbedingung (429) auf.|

Wenn ein Fehler auftritt, enthält der entsprechende HTTP-Antworttext ein JSON-Objekt mit den Fehlerdetails:

| Element | BESCHREIBUNG |
| ------- | ----------- |
| code | Fehlercode |
| correlationId | Eine Korrelations-ID, die zum Debuggen verwendet werden kann. |
| message | Ausführliche Beschreibung des Fehlers. **Fehlerbeschreibungen können sich jederzeit ändern. Verlassen Sie sich nicht auf die Fehlermeldung selbst.**|

Beispielfehler:
```json
{"error":{"correlationId":"7f30f4d3-0f3a-41e0-a417-527f21b3848f","code":"SecretHeaderNotFound","message":"Secret is not found in the request headers."}}
```

Im Folgenden finden Sie eine Liste typischer Service Fabric-Fehler speziell für verwaltete Identitäten:

| Code | `Message` | BESCHREIBUNG | 
| ----------- | ----- | ----------------- |
| SecretHeaderNotFound | Das Geheimnis wurde in den Anforderungsheadern nicht gefunden. | Der Authentifizierungscode wurde nicht mit der Anforderung bereitgestellt. | 
| ManagedIdentityNotFound | Für den angegebenen Anwendungshost wurde keine verwaltete Identität gefunden. | Die Anwendung besitzt keine Identität, oder der Authentifizierungscode ist unbekannt. |
| ArgumentNullOrEmpty | Der Parameter „Ressource“ darf nicht NULL oder eine leere Zeichenfolge sein. | Die Ressource (Zielgruppe) wurde in der Anforderung nicht angegeben. |
| InvalidApiVersion | Die angegebene API-Version wird nicht unterstützt. Die unterstützte Version lautet „2019-07-01-preview“. | Fehlende oder nicht unterstützte API-Version im Anforderungs-URI. |
| InternalServerError | Ein Fehler ist aufgetreten. | Im Subsystem der verwalteten Identität ist ein Fehler aufgetreten, möglicherweise außerhalb des Service Fabric-Stapels. Die wahrscheinlichste Ursache ist ein falscher Wert für die Ressource (suchen Sie nach nachstehenden Schrägstrichen „/“). | 

## <a name="retry-guidance"></a>Informationen zur Wiederholung 

In der Regel ist der einzige Fehler, bei dem sich ein erneuter Versuch lohnt, der Fehler 429 (Zu viele Anforderungen). Bei internen Serverfehlern oder 5xx-Fehlercodes kann ein erneuter Versuch unternommen werden, aber die Ursache ist möglicherweise dauerhaft. 

Drosselungsgrenzwerte gelten für die Anzahl von Aufrufen, die vom Subsystem der verwalteten Identität empfangen werden – insbesondere für die Upstreamabhängigkeiten (den Azure-Dienst für verwaltete Identitäten oder den sicheren Tokendienst). Service Fabric speichert Token auf verschiedenen Ebenen in der Pipeline zwischen. Angesichts der verteilten Natur der beteiligten Komponenten erhält der Aufrufer möglicherweise inkonsistente Drosselungsantworten (beispielsweise kann bei der Anforderung eines Tokens für ein und dieselbe Identität auf einem Knoten bzw. in einer Instanz einer Anwendung eine Drosselung erfolgen, nicht jedoch auf einem anderen Knoten). Wenn die Drosselungsbedingung festgelegt ist, treten bei nachfolgenden Anforderungen aus der gleichen Anwendung möglicherweise Fehler mit den HTTP-Statuscode 429 (Zu viele Anforderungen) auf, bis die Bedingung bereinigt wird.  

Es empfiehlt sich, die Anforderungen, die aufgrund einer Drosselung nicht ausgeführt werden konnten, mit einem exponentiellen Backoff auf folgende Weise erneut zu versuchen: 

| Aufrufindex | Aktion bei Empfang von 429 | 
| --- | --- | 
| 1 | 1 Sekunde warten und erneut versuchen |
| 2 | 2 Sekunden warten und erneut versuchen |
| 3 | 4 Sekunden warten und erneut versuchen |
| 4 | 8 Sekunden warten und erneut versuchen |
| 4 | 8 Sekunden warten und erneut versuchen |
| 5 | 16 Sekunden warten und erneut versuchen |

## <a name="resource-ids-for-azure-services"></a>Ressourcen-IDs für Azure-Dienste
Eine Liste mit Ressourcen, die Azure AD unterstützen, und den dazugehörigen Ressourcen-IDs finden Sie unter [Azure-Dienste, die verwaltete Identitäten für Azure-Ressourcen unterstützen](../active-directory/managed-identities-azure-resources/services-support-msi.md).

## <a name="next-steps"></a>Nächste Schritte
* [Bereitstellen einer Azure Service Fabric-Anwendung mit einer systemseitig zugewiesenen verwalteten Identität](./how-to-deploy-service-fabric-application-system-assigned-managed-identity.md)
* [Bereitstellen einer Azure Service Fabric-Anwendung mit einer benutzerseitig zugewiesenen verwalteten Identität](./how-to-deploy-service-fabric-application-user-assigned-managed-identity.md)
* [Gewähren des Zugriffs auf andere Azure-Ressourcen für eine Azure Service Fabric-Anwendung](./how-to-grant-access-other-resources.md)

## <a name="see-also"></a>Weitere Informationen

* Überprüfen der [Unterstützung für verwaltete Identitäten](./concepts-managed-identity.md) in Azure Service Fabric

* [Bereitstellen eines neuen](./configure-new-azure-service-fabric-enable-managed-identity.md) Azure Service Fabric-Clusters mit Unterstützung verwalteter Identitäten 

* [Aktivieren von verwalteten Identitäten](./configure-existing-cluster-enable-managed-identity-token-service.md) in einem vorhandenen Azure Service Fabric-Cluster