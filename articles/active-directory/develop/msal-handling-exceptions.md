---
title: Fehler und Ausnahmen (MSAL) | Microsoft Identity Platform
description: Erfahren Sie, wie Sie Fehler und Ausnahmen, den bedingten Zugriff und Anspruchsaufforderungen in MSAL-Anwendungen verarbeiten.
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
ms.date: 09/08/2019
ms.author: twhitney
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: 635793d18bf33752a2672788bb632571743410b7
ms.sourcegitcommit: 387da88b8262368c1b67fffea58fe881308db1c2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/07/2019
ms.locfileid: "71982762"
---
# <a name="handle-msal-exceptions-and-errors"></a>Verarbeiten von MSAL-Ausnahmen und -Fehlern

Dieser Artikel gibt einen Überblick über die verschiedenen Fehlerarten und Empfehlungen zur Behandlung von allgemeinen Anmeldefehlern.

## <a name="msal-error-handling-basics"></a>Grundlagen der MSAL-Fehlerbehandlung

Ausnahmen in der Microsoft-Authentifizierungsbibliothek (Microsoft Authentication Library, MSAL) sind für App-Entwickler vorgesehen, die eine Problembehandlung durchführen, und nicht für Endbenutzer. Ausnahmemeldungen sind nicht lokalisiert.

Bei der Verarbeitung von Ausnahmen und Fehlern können Sie den Ausnahmetyp selbst und den Fehlercode verwenden, um zwischen Ausnahmen zu unterscheiden.  Eine Liste der Fehlercodes finden Sie unter [Authentifizierungs- und Autorisierungsfehlercodes](reference-aadsts-error-codes.md).

Während des automatischen oder interaktiven Tokenabrufs können Apps während der Anmeldung auf Fehler stoßen, z.B. Fehler bei Zustimmungen, bedingtem Zugriff (MFA, Geräteverwaltung, standortbezogene Einschränkungen), Tokenausstellung und -einlösung sowie Benutzereigenschaften.

## <a name="msal-for-ios-and-macos-errors"></a>Fehler von MSAL für iOS und macOS

Die vollständige Liste der Fehler finden Sie unter [MSALError-Enumeration](https://github.com/AzureAD/microsoft-authentication-library-for-objc/blob/master/MSAL/src/public/MSALError.h#L128).

Alle von MSAL erzeugten Fehler werden mit der `MSALErrorDomain`-Domain zurückgegeben. 

Bei Systemfehlern gibt MSAL den ursprünglichen `NSError` von der System-API zurück. Wenn der Tokenabruf beispielsweise aufgrund fehlender Netzwerkkonnektivität fehlschlägt, gibt MSAL einen Fehler mit der `NSURLErrorDomain`-Domain und `NSURLErrorNotConnectedToInternet`-Code zurück.

Es wird empfohlen, mindestens die folgenden beiden MSAL-Fehler auf der Clientseite zu behandeln:

- `MSALErrorInteractionRequired`: Der Benutzer muss eine interaktive Anforderung ausführen. Es gibt viele Bedingungen, die zu diesem Fehler führen können, z.B. eine abgelaufene Authentifizierungssitzung oder die Notwendigkeit zusätzlicher Authentifizierungsanforderungen. Rufen Sie die interaktive MSAL-Tokenabruf-API auf, um die Wiederherstellung durchzuführen. 

- `MSALErrorServerDeclinedScopes`: Einige oder alle Bereiche wurden abgelehnt. Entscheiden Sie, ob der Vorgang mit nur den gewährten Bereichen fortgesetzt werden soll, oder beenden Sie den Anmeldevorgang.

> [!NOTE]
> Die `MSALInternalError`-Enumeration sollte nur zu Referenzzwecken und zum Debuggen verwendet werden. Versuchen Sie nicht, diese Fehler zur Laufzeit automatisch zu behandeln. Wenn Ihre App auf einen der Fehler stößt, die unter `MSALInternalError` fallen, sollten Sie eine generische Meldung für den Benutzer anzeigen, die beschreibt, was passiert ist.

`MSALInternalErrorBrokerResponseNotReceived` bedeutet beispielsweise, dass der Benutzer die Authentifizierung nicht fertiggestellt hat und manuell zur App zurückgekehrt ist. In diesem Fall sollte Ihre App eine generische Fehlermeldung anzeigen, die erläutert, dass die Authentifizierung nicht abgeschlossen wurde, und dem Benutzer empfehlen, sich erneut zu authentifizieren.

Der folgende Objective-C-Beispielcode zeigt bewährte Methoden für die Behandlung einiger allgemeiner Fehlerbedingungen:

Objective-C
```ObjC
    MSALInteractiveTokenParameters *interactiveParameters = ...;
    MSALSilentTokenParameters *silentParameters = ...;
    
    MSALCompletionBlock completionBlock;
    __block __weak MSALCompletionBlock weakCompletionBlock;
    
    weakCompletionBlock = completionBlock = ^(MSALResult *result, NSError *error)
    {
        if (!error)
        {
            // Use result.accessToken
            NSString *accessToken = result.accessToken;
            return;
        }
        
        if ([error.domain isEqualToString:MSALErrorDomain])
        {
            switch (error.code)
            {
                case MSALErrorInteractionRequired:
                {
                    // Interactive auth will be required
                    [application acquireTokenWithParameters:interactiveParameters
                                            completionBlock:weakCompletionBlock];
                    
                    break;
                }
                    
                case MSALErrorServerDeclinedScopes:
                {
                    // These are list of granted and declined scopes.
                    NSArray *grantedScopes = error.userInfo[MSALGrantedScopesKey];
                    NSArray *declinedScopes = error.userInfo[MSALDeclinedScopesKey];
                    
                    // To continue acquiring token for granted scopes only, do the following
                    silentParameters.scopes = grantedScopes;
                    [application acquireTokenSilentWithParameters:silentParameters
                                                  completionBlock:weakCompletionBlock];
                    
                    // Otherwise, instead, handle error fittingly to the application context
                    break;
                }
                    
                case MSALErrorServerProtectionPoliciesRequired:
                {
                    // Integrate the Intune SDK and call the
                    // remediateComplianceForIdentity:silent: API.
                    // Handle this error only if you integrated Intune SDK.
                    // See more info here: https://aka.ms/intuneMAMSDK
                    
                    break;
                }
                    
                case MSALErrorUserCanceled:
                {
                    // The user cancelled the web auth session.
                    // You may want to ask the user to try again.
                    // Handling of this error is optional.
                    
                    break;
                }
                    
                case MSALErrorInternal:
                {
                    // Log the error, then inspect the MSALInternalErrorCodeKey
                    // in the userInfo dictionary.
                    // Display generic error message to the end user
                    // More detailed information about the specific error
                    // under MSALInternalErrorCodeKey can be found in MSALInternalError enum.
                    NSLog(@"Failed with error %@", error);
                    
                    break;
                }
                    
                default:
                    NSLog(@"Failed with unknown MSAL error %@", error);
                    
                    break;
            }
            
            return;
        }
        
        // Handle no internet connection.
        if ([error.domain isEqualToString:NSURLErrorDomain] && error.code == NSURLErrorNotConnectedToInternet)
        {
            NSLog(@"No internet connection.");
            return;
        }
        
        // Other errors may require trying again later,
        // or reporting authentication problems to the user.
        NSLog(@"Failed with error %@", error);
    };
    
    // Acquire token silently
    [application acquireTokenSilentWithParameters:silentParameters
                                  completionBlock:completionBlock];

     // or acquire it interactively.
     [application acquireTokenWithParameters:interactiveParameters
                             completionBlock:completionBlock];
```

Swift
```swift
    let interactiveParameters: MSALInteractiveTokenParameters = ...
    let silentParameters: MSALSilentTokenParameters = ...
            
    var completionBlock: MSALCompletionBlock!
    completionBlock = { (result: MSALResult?, error: Error?) in
                
        if let result = result
        {
            // Use result.accessToken
            let accessToken = result.accessToken
            return
        }

        guard let error = error as NSError? else { return }

        if error.domain == MSALErrorDomain, let errorCode = MSALError(rawValue: error.code)
        {
            switch errorCode
            {
                case .interactionRequired:
                    // Interactive auth will be required
                    application.acquireToken(with: interactiveParameters, completionBlock: completionBlock)

                case .serverDeclinedScopes:
                    let grantedScopes = error.userInfo[MSALGrantedScopesKey]
                    let declinedScopes = error.userInfo[MSALDeclinedScopesKey]

                    if let scopes = grantedScopes as? [String] {
                        silentParameters.scopes = scopes
                        application.acquireTokenSilent(with: silentParameters, completionBlock: completionBlock)
                    }
                        
                    case .serverProtectionPoliciesRequired:
                        // Integrate the Intune SDK and call the
                        // remediateComplianceForIdentity:silent: API.
                        // Handle this error only if you integrated Intune SDK.
                        // See more info here: https://aka.ms/intuneMAMSDK
                        break
                        
                    case .userCanceled:
                       // The user cancelled the web auth session.
                       // You may want to ask the user to try again.
                       // Handling of this error is optional.
                       break
                        
                    case .internal:
                        // Log the error, then inspect the MSALInternalErrorCodeKey
                        // in the userInfo dictionary.
                        // Display generic error message to the end user
                        // More detailed information about the specific error
                        // under MSALInternalErrorCodeKey can be found in MSALInternalError enum.
                        print("Failed with error \(error)");
                        
                    default:
                        print("Failed with unknown MSAL error \(error)")
            }
        }
                
        // Handle no internet connection.
        if error.domain == NSURLErrorDomain && error.code == NSURLErrorNotConnectedToInternet
        {
            print("No internet connection.")
            return
        }
                
        // Other errors may require trying again later,
        // or reporting authentication problems to the user.
        print("Failed with error \(error)");    
    }
   
    // Acquire token silently
    application.acquireToken(with: interactiveParameters, completionBlock: completionBlock)
 
    // or acquire it interactively.
    application.acquireTokenSilent(with: silentParameters, completionBlock: completionBlock)
```

## <a name="net-exceptions"></a>.NET-Ausnahmen

Bei der Verarbeitung von Ausnahmen können Sie den Ausnahmetyp selbst und das `ErrorCode`-Element verwenden, um zwischen Ausnahmen zu unterscheiden. Die `ErrorCode`-Werte sind Konstanten des Typs [MsalError](/dotnet/api/microsoft.identity.client.msalerror?view=azure-dotnet).

Sie können auch die Felder von [MsalClientException](/dotnet/api/microsoft.identity.client.msalexception?view=azure-dotnet), [MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet) und [MsalUIRequiredException](/dotnet/api/microsoft.identity.client.msaluirequiredexception?view=azure-dotnet) überprüfen.

Wenn [MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet) ausgelöst wird, überprüfen Sie die [Authentifizierungs- und Autorisierungsfehlercodes](reference-aadsts-error-codes.md), um festzustellen, ob der Code dort aufgeführt wird.

### <a name="common-exceptions"></a>Allgemeine Ausnahmen

Dies sind die häufigsten Ausnahmen, die ausgelöst werden könnten, und einige mögliche Abhilfemaßnahmen:  

| Ausnahme | Fehlercode | Lösung|
| --- | --- | --- |
| [MsalUiRequiredException](/dotnet/api/microsoft.identity.client.msaluirequiredexception?view=azure-dotnet) | AADSTS65001: The user or administrator has not consented to use the application with ID '{appId}' named '{appName}'. (Der Benutzer oder Administrator hat nicht zugestimmt, die Anwendung mit der ID '{appId}' und dem Namen '{appName}' zu verwenden.) Send an interactive authorization request for this user and resource. (Senden Sie eine interaktive Autorisierungsanforderung für diesen Benutzer und die Ressource.)| Sie müssen zuerst die Zustimmung des Benutzers einholen. Wenn Sie nicht .NET Core verwenden (das keine Webbenutzeroberfläche aufweist), rufen Sie (einmalig) `AcquireTokeninteractive` auf. Wenn Sie .NET Core verwenden oder `AcquireTokenInteractive` nicht ausführen möchten, kann der Benutzer zu einer URL navigieren, um seine Zustimmung zu geben: https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id={clientId}&response_type=code&scope=user.read. Aufrufen von `AcquireTokenInteractive`: `app.AcquireTokenInteractive(scopes).WithAccount(account).WithClaims(ex.Claims).ExecuteAsync();`|
| [MsalUiRequiredException](/dotnet/api/microsoft.identity.client.msaluirequiredexception?view=azure-dotnet) | AADSTS50079: The user is required to use multi-factor authentication (MFA). (Der Benutzer muss mehrstufige Authentifizierung (MFA) verwenden.)| Es gibt keine Abhilfemaßnahme. Wenn MFA für Ihren Mandanten konfiguriert ist und Azure Active Directory (AAD) entscheidet, die Methode durchzusetzen, müssen Sie auf einen interaktiven Flow wie `AcquireTokenInteractive` oder `AcquireTokenByDeviceCode` ausweichen.|
| [MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet) |AADSTS90010: The grant type is not supported over the */common* or */consumers* endpoints. (Der Gewährungstyp wird für den /common- oder /consumers-Endpunkt nicht unterstützt.) Use the */organizations* or tenant-specific endpoint. (Verwenden Sie den /organizations-Endpunkt oder mandantenspezifischen Endpunkt.) You used */common*. (Sie haben /common verwendet.)| Wie in der Azure AD-Meldung erläutert, muss die Autorität über einen Mandanten oder ansonsten über einen */organizations*-Endpunkt verfügen.|
| [MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet) | AADSTS70002: The request body must contain the following parameter: `client_secret or client_assertion`. (Der Anforderungstext muss den folgenden Parameter enthalten:)| Diese Ausnahme kann ausgelöst werden, wenn die Anwendung in Azure AD nicht als öffentliche Clientanwendung registriert wurde. Bearbeiten Sie im Azure-Portal das Manifest für Ihre Anwendung, und legen Sie `allowPublicClient` auf `true` fest. |
| [MsalClientException](/dotnet/api/microsoft.identity.client.msalclientexception?view=azure-dotnet)| `unknown_user Message`: Could not identify logged in user. (Der angemeldete Benutzer konnte nicht identifiziert werden.)| Die Bibliothek war nicht in der Lage, den aktuellen, in Windows angemeldeten Benutzer abzufragen, oder der Benutzer ist nicht in AD oder AAD eingebunden (Benutzer, die über den Arbeitsplatz eingebunden sind, werden nicht unterstützt). Lösung 1: Vergewissern Sie sich auf der UWP, dass die Anwendung die folgenden Funktionen aufweist: Unternehmensauthentifizierung, private Netzwerke (Client und Server), Benutzerkonteninformationen. Lösung 2: Implementieren Sie eigene Logik zum Abrufen des Benutzernamens (z. B. john@contoso.com), und verwenden Sie die Methode in der Form `AcquireTokenByIntegratedWindowsAuth`, die den Benutzernamen annimmt.|
| [MsalClientException](/dotnet/api/microsoft.identity.client.msalclientexception?view=azure-dotnet)|integrated_windows_auth_not_supported_managed_user| Diese Methode basiert auf einem von Active Directory (AD) verfügbar gemachten Protokoll. Wenn ein Benutzer in Azure Active Directory ohne AD-Unterstützung („verwalteter Benutzer“) erstellt wurde, schlägt diese Methode fehl. Benutzer, die in AD erstellt und von AAD unterstützt werden („Partnerbenutzer“), können von dieser nicht interaktiven Authentifizierungsmethode profitieren. Lösung: Verwenden Sie die interaktive Authentifizierung.|

### `MsalUiRequiredException`

Einer der allgemeinen Statuscodes, die von MSAL.NET zurückgegeben werden, wenn beim Aufruf von `AcquireTokenSilent()` `MsalError.InvalidGrantError` zurückgegeben wird. Dieser Statuscode bedeutet, dass die Anwendung die Authentifizierungsbibliothek erneut aufrufen sollte, jedoch im interaktiven Modus (AcquireTokenInteractive oder AcquireTokenByDeviceCodeFlow für öffentliche Clientanwendungen und eine Abfrage in Web-Apps). Dies liegt daran, dass eine zusätzliche Benutzerinteraktion erforderlich ist, bevor ein Authentifizierungstoken ausgegeben werden kann.

Wenn bei `AcquireTokenSilent` ein Fehler auftritt, liegt dies in den meisten Fällen daran, dass der Tokencache keine Token für Ihre Anforderung hat. Zugriffstoken laufen innerhalb 1 Stunde ab, und `AcquireTokenSilent` wird versuchen, ein neues auf der Grundlage eines Aktualisierungstokens abzurufen (in OAuth2-Begriffen ist dies der „Tokenaktualisierungsflow“). Bei diesem Flow kann auch aus verschiedenen Gründen ein Fehler auftreten, wenn z. B. ein Mandantenadministrator strengere Anmelderichtlinien konfiguriert. 

Die Interaktion zielt darauf ab, dass der Benutzer eine Aktion durchführt. Einige dieser Bedingungen können von Benutzern leicht aufgelöst werden (z. B. Akzeptieren der Nutzungsbedingungen mit einem einzigen Mausklick), und einige dieser Bedingungen können mit der aktuellen Konfiguration nicht aufgelöst werden (z. B. muss der betreffende Computer eine Verbindung mit einem bestimmten Unternehmensnetzwerk herstellen). Einige helfen dem Benutzer beim Einrichten der mehrstufigen Authentifizierung oder Installieren von Microsoft Authenticator auf dem Gerät.

### <a name="msaluirequiredexception-classification-enumeration"></a>`MsalUiRequiredException` Klassifizierungsenumeration

MSAL macht ein `Classification`-Feld verfügbar, das Sie lesen können, um die Benutzerfreundlichkeit zu steigern, z. B. um dem Benutzer mitzuteilen, dass sein Kennwort abgelaufen ist, oder er der Verwendung einiger Ressourcen zustimmen muss. Die unterstützten Werte sind Teil der `UiRequiredExceptionClassification`-Enumeration:

| Classification    | Bedeutung           | Empfohlene Behandlung |
|-------------------|-------------------|----------------------|
| BasicAction | Die Bedingung kann während des interaktiven Authentifizierungsflows durch Benutzerinteraktion aufgelöst werden. | AcquireTokenInteractively() aufrufen. |
| AdditionalAction | Die Bedingung kann durch eine zusätzliche Abhilfemaßnahme mit dem System außerhalb des interaktiven Authentifizierungsflows aufgelöst werden. | AcquireTokenInteractively() aufrufen, um eine Nachricht anzuzeigen, die die Abhilfemaßnahme erklärt. Beim Aufrufen der Anwendung können Flows ausgeblendet werden, für die „additional_action“ erforderlich ist, wenn der Benutzer die Abhilfemaßnahme wahrscheinlich nicht ausführen kann. |
| MessageOnly      | Die Bedingung kann zu diesem Zeitpunkt nicht aufgelöst werden. Beim Starten des interaktiven Authentifizierungsflows wird eine Meldung angezeigt, in der die Bedingung erläutert wird. | AcquireTokenInteractively() aufrufen, um eine Nachricht anzuzeigen, die die Bedingung erklärt. AcquireTokenInteractively() gibt den UserCanceled-Fehler zurück, nachdem der Benutzer die Meldung gelesen und das Fenster geschlossen hat. Beim Aufrufen der Anwendung können Flows ausgeblendet werden, die zu „message_only“ führen, wenn der Benutzer von der Nachricht wahrscheinlich keinen Nutzen hat.|
| ConsentRequired  | Die Benutzerzustimmung ist nicht vorhanden oder wurde widerrufen. | AcquireTokenInteractively() aufrufen, damit der Benutzer seine Zustimmung erteilt. |
| UserPasswordExpired | Das Kennwort des Benutzers ist abgelaufen. | AcquireTokenInteractively() aufrufen, damit der Benutzer sein Kennwort zurücksetzen kann. |
| PromptNeverFailed| Die interaktive Authentifizierung wurde mit dem Parameter „prompt=never“ aufgerufen, wodurch MSAL gezwungen wird, sich auf Browsercookies zu verlassen und den Browser nicht anzuzeigen. Dabei ist ein Fehler aufgetreten. | AcquireTokenInteractively() ohne „Prompt.None“ aufrufen. |
| AcquireTokenSilentFailed | Das MSAL-SDK verfügt nicht über genügend Informationen, um ein Token aus dem Cache abzurufen. Dies kann daran liegen, dass keine Token im Cache vorhanden sind oder ein Konto nicht gefunden wurde. Weitere Informationen finden Sie in der Fehlermeldung.  | AcquireTokenInteractively() aufrufen. |
| Keine    | Es sind keine weiteren Informationen vorhanden. Die Bedingung könnte während des interaktiven Authentifizierungsflows durch Benutzerinteraktion aufgelöst werden. | AcquireTokenInteractively() aufrufen. |

## <a name="code-example"></a>Codebeispiel

```csharp
AuthenticationResult res;
try
{
 res = await application.AcquireTokenSilent(scopes, account)
        .ExecuteAsync();
}
catch (MsalUiRequiredException ex) when (ex.ErrorCode == MsalError.InvalidGrantError)
{
 switch (ex.Classification)
 {
  case UiRequiredExceptionClassification.None:
   break;
  case UiRequiredExceptionClassification.MessageOnly:
  // You might want to call AcquireTokenInteractive(). Azure AD will show a message
  // that explains the condition. AcquireTokenInteractively() will return UserCanceled error
  // after the user reads the message and closes the window. The calling application may choose
  // to hide features or data that result in message_only if the user is unlikely to benefit 
  // from the message
  try
  {
   res = await application.AcquireTokenInteractive(scopes)
                          .ExecuteAsync();
  }
  catch (MsalClientException ex2) when (ex2.ErrorCode == MsalError.AuthenticationCanceledError)
  {
   // Do nothing. The user has seen the message
  }
  break;

  case UiRequiredExceptionClassification.BasicAction:
  // Call AcquireTokenInteractive() so that the user can, for instance accept terms
  // and conditions

  case UiRequiredExceptionClassification.AdditionalAction:
  // You might want to call AcquireTokenInteractive() to show a message that explains the remedial action. 
  // The calling application may choose to hide flows that require additional_action if the user 
  // is unlikely to complete the remedial action (even if this means a degraded experience)

  case UiRequiredExceptionClassification.ConsentRequired:
  // Call AcquireTokenInteractive() for user to give consent.
  
  case UiRequiredExceptionClassification.UserPasswordExpired:
  // Call AcquireTokenInteractive() so that user can reset their password
  
  case UiRequiredExceptionClassification.PromptNeverFailed:
  // You used WithPrompt(Prompt.Never) and this failed
  
  case UiRequiredExceptionClassification.AcquireTokenSilentFailed:
  default:
  // May be resolved by user interaction during the interactive authentication flow.
  res = await application.AcquireTokenInteractive(scopes)
                         .ExecuteAsync(); break;
 }
}
```


## <a name="javascript-errors"></a>JavaScript-Fehler

MSAL.js stellt Fehlerobjekte bereit, mit denen die unterschiedlichen Typen allgemeiner Fehler abstrahiert und klassifiziert werden. Außerdem bietet es eine Schnittstelle für den Zugriff auf bestimmte Details der Fehler wie z. B. Fehlermeldungen, damit Sie sie entsprechend behandeln können.

### <a name="error-object"></a>Das Error-Objekt.

```javascript
export class AuthError extends Error {
    // This is a short code describing the error
    errorCode: string;
    // This is a descriptive string of the error,
    // and may also contain the mitigation strategy
    errorMessage: string;
    // Name of the error class
    this.name = "AuthError";
}
```

Durch die Erweiterung der Fehlerklasse haben Sie Zugriff auf die folgenden Eigenschaften:
* **AuthError.message:**  Identisch mit errorMessage
* **AuthError.stack:** Die Stapelüberwachung für ausgelöste Fehler. Sie ermöglicht die Ablaufverfolgung zum Ursprung des Fehlers.

### <a name="error-types"></a>Fehlertypen

Folgende Fehlertypen sind verfügbar:

- `AuthError`: Die grundlegende Fehlerklasse für die MSAL.js-Bibliothek. Sie wird auch für unerwartete Fehler verwendet.

- `ClientAuthError`: Die Fehlerklasse, die auf ein Problem mit der Clientauthentifizierung hinweist. Bei den meisten Fehlern, die von der Bibliothek gemeldet werden, handelt es sich um ClientAuthErrors. Diese Fehler resultieren z.B. aus dem Aufrufen einer Anmeldemethode, wenn die Anmeldung bereits ausgeführt wird, oder dem Abbrechen der Anmeldung durch den Benutzer.

- `ClientConfigurationError`: Fehlerklasse zur Erweiterung von `ClientAuthError`, wenn dieser Fehler ausgelöst wird, bevor Anforderungen ausgeführt werden, wenn die angegebenen Benutzerkonfigurationsparameter falsch formatiert oder nicht vorhanden sind.

- `ServerError`: Fehlerklasse, die die vom Authentifizierungsserver gesendeten Fehlerzeichenfolgen darstellt. Bei den Fehlern kann es sich um ungültige Anforderungsformate oder -parameter sowie um andere Fehler handeln, die den Server an der Authentifizierung oder Autorisierung des Benutzers hindern.

- `InteractionRequiredAuthError`: Fehlerklasse, die `ServerError` erweitert und Serverfehler darstellt, die einen interaktiven Aufruf erfordern. Dieser Fehler wird durch `acquireTokenSilent` ausgelöst, wenn der Benutzer mit dem Server interagieren muss, um Anmeldeinformationen anzugeben oder der Authentifizierung/Autorisierung zuzustimmen. Zu den Fehlercodes zählen `"interaction_required"`, `"login_required"` und `"consent_required"`.

Zur Fehlerbehandlung in Authentifizierungsflows mit Umleitungsmethoden (`loginRedirect`, `acquireTokenRedirect`) müssen Sie den Rückruf, der nach der Umleitung erfolgreich oder mit einem Fehler aufgerufen wird, mithilfe der `handleRedirectCallback()`-Methode wie folgt registrieren:

```javascript
function authCallback(error, response) {
    //handle redirect response
}

var myMSALObj = new Msal.UserAgentApplication(msalConfig);

// Register Callbacks for redirect flow
myMSALObj.handleRedirectCallback(authCallback);
myMSALObj.acquireTokenRedirect(request);
```

Die Methoden für Popupfunktionen (`loginPopup`, `acquireTokenPopup`) geben Zusagen zurück. Auf diese Weise können Sie das Zusagemuster („.then“ und „.catch“) verwenden, um die Methoden zu behandeln, wie im Folgenden dargestellt:

```javascript
myMSALObj.acquireTokenPopup(request).then(
    function (response) {
        // success response
    }).catch(function (error) {
        console.log(error);
    });
```

### <a name="interaction-required-errors"></a>Fehler, die eine Interaktion erfordern

Ein Fehler wird zurückgegeben, wenn Sie versuchen, eine nicht interaktive Methode (z.B. `acquireTokenSilent`) für den Tokenabruf zu verwenden, MSAL das Token aber nicht automatisch abrufen konnte.

Mögliche Gründe:

- Sie müssen sich anmelden.
- Sie müssen Ihre Zustimmung geben.
- Sie müssen eine mehrstufige Authentifizierung ausführen.

Die Lösung besteht darin, eine interaktive Methode wie `acquireTokenPopup` oder `acquireTokenRedirect` aufzurufen:

```javascript
// Request for Access Token
myMSALObj.acquireTokenSilent(request).then(function (response) {
    // call API
}).catch( function (error) {
    // call acquireTokenPopup in case of acquireTokenSilent failure
    // due to consent or interaction required
    if (error.errorCode === "consent_required"
    || error.errorCode === "interaction_required"
    || error.errorCode === "login_required") {
        myMSALObj.acquireTokenPopup(request).then(
            function (response) {
                // call API
            }).catch(function (error) {
                console.log(error);
            });
    }
});
```

## <a name="conditional-access-and-claims-challenges"></a>Bedingter Zugriff und Anspruchsaufforderungen

Beim automatischen Abruf von Token kann Ihre Anwendung Fehler empfangen, wenn für eine API, auf die Sie zugreifen möchten, eine [Anspruchsaufforderung für bedingten Zugriff](conditional-access-dev-guide.md), wie z. B. eine MFA-Richtlinie, erforderlich ist.

Sie behandeln diesen Fehler in der Regel dadurch, dass Sie mithilfe von MSAL ein Token interaktiv abrufen. Beim interaktiven Abruf eines Tokens erhält der Benutzer in Form einer Aufforderung die Möglichkeit, die erforderliche Richtlinie für bedingten Zugriff zu erfüllen.

In bestimmten Fällen können Sie beim Aufruf einer API, die bedingten Zugriff erfordert, in dem von der API ausgegebenen Fehler eine Anspruchsaufforderung erhalten. Wenn die Richtlinie für bedingten Zugriff beispielsweise ein verwaltetes Gerät (Intune) vorsieht, könnte eine mit der folgenden vergleichbare Fehlermeldung ausgegeben werden – [AADSTS53000: Für den Zugriff auf diese Ressource müssen Sie ein verwaltetes Gerät verwenden.](reference-aadsts-error-codes.md) In diesem Fall können Sie die Ansprüche im Aufruf für den Tokenabruf übergeben, damit der Benutzer aufgefordert wird, die entsprechende Richtlinie zu erfüllen.

### <a name="net"></a>.NET

Wenn Sie eine API, die den bedingten Zugriff erfordert, von MSAL.NET aufrufen, muss Ihre Anwendung Ausnahmen bei Anspruchsaufforderungen behandeln. Die Ausnahme tritt in Form einer [MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet) auf, deren [Claims](/dotnet/api/microsoft.identity.client.msalserviceexception.claims?view=azure-dotnet)-Eigenschaft nicht leer ist.

Um die Anspruchsaufforderung zu behandeln, verwenden Sie die `.WithClaim()`-Methode der `PublicClientApplicationBuilder`-Klasse.

### <a name="javascript"></a>JavaScript

Beim automatischen Abruf von Token mit MSAL.js (und `acquireTokenSilent`) kann Ihre Anwendung Fehlermeldungen empfangen, wenn für eine API, auf die Sie zugreifen möchten, eine [Anspruchsaufforderung für bedingten Zugriff](conditional-access-dev-guide.md), wie z. B. eine MFA-Richtlinie, erforderlich ist.

Sie behandeln diesen Fehler in der Regel dadurch, dass Sie einen interaktiven Aufruf ausführen, um ein Token in MSAL.js abzurufen, z. B. mit `acquireTokenPopup` oder `acquireTokenRedirect`. Siehe folgendes Beispiel:

```javascript
myMSALObj.acquireTokenSilent(accessTokenRequest).then(function (accessTokenResponse) {
    // call API
}).catch( function (error) {
    if (error instanceof InteractionRequiredAuthError) {
        // Extract claims from error message
        accessTokenRequest.claimsRequest = extractClaims(error.errorMessage);
        // call acquireTokenPopup in case of InteractionRequiredAuthError failure
        myMSALObj.acquireTokenPopup(accessTokenRequest).then(function (accessTokenResponse) {
            // call API
        }).catch(function (error) {
            console.log(error);
        });
    }
});
```

Beim interaktiven Abruf des Tokens erhält der Benutzer in Form einer Aufforderung die Möglichkeit, die erforderliche Richtlinie für bedingten Zugriff zu erfüllen.

Beim Aufruf einer API, die bedingten Zugriff erfordert, können Sie in dem von der API ausgegebenen Fehler eine Anspruchsaufforderung erhalten. In diesem Fall können Sie die im Fehler zurückgegebenen Ansprüche an das `claimsRequest`-Feld der `AuthenticationParameters.ts`-Klasse übergeben, um die entsprechende Richtlinie zu erfüllen. 

Weitere Details finden Sie unter [Anfordern zusätzlicher Ansprüche](active-directory-optional-claims.md).

### <a name="msal-for-ios-and-macos"></a>MSAL für iOS und macOS

MSAL für iOS und macOS ermöglicht es Ihnen, bestimmte Ansprüche sowohl in interaktiven als auch in automatischen Tokenabrufszenarien anzufordern.

Geben Sie `claimsRequest` in `MSALSilentTokenParameters` oder `MSALInteractiveTokenParameters` an, um benutzerdefinierte Ansprüche anzufordern.

Weitere Informationen finden Sie unter [Anfordern benutzerdefinierter Ansprüche mit MSAL für iOS und macOS](request-custom-claims.md).

## <a name="retrying-after-errors-and-exceptions"></a>Wiederholung nach Fehlern und Ausnahmen

Es wird erwartet, dass Sie beim Aufrufen von MSAL eigene Wiederholungsrichtlinien implementieren. MSAL richtet HTTP-Aufrufe an den AAD-Dienst, und gelegentlich können Ausfälle auftreten, z. B. kann das Netzwerk ausfallen, oder der Server ist überlastet.  

### <a name="http-error-codes-500-600"></a>HTTP-Fehlercodes 500–600

MSAL.NET implementiert einen einfachen einmaligen Wiederholungsmechanismus für Fehler mit den HTTP-Fehlercodes 500–600.

### <a name="http-429"></a>HTTP 429

Wenn der Diensttokenserver (Service Token Server, STS) durch zu viele Anforderungen überlastet ist, gibt er den HTTP-Fehler 429 mit einem Hinweis im `Retry-After`-Antwortfeld zu dem Zeitpunkt zurück, an dem Sie den Vorgang wiederholen können.

### <a name="net"></a>.NET

[MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet) gibt `System.Net.Http.Headers.HttpResponseHeaders` als `namedHeaders`-Eigenschaft aus. Sie können zusätzliche Informationen im Fehlercode nutzen, um die Zuverlässigkeit Ihrer Anwendungen zu verbessern. Im beschriebenen Fall können Sie die `RetryAfterproperty` (vom Typ `RetryConditionHeaderValue`) nutzen, um den für die Wiederholung geeigneten Zeitpunkt zu berechnen.

Im folgenden finden Sie ein Beispiel für eine Daemonanwendung, die den Clientanmeldeinformationenflow verwendet. Sie können dies an eine beliebige Methode zum Abrufen eines Tokens anpassen.

```csharp
do
{
 retry = false;
 TimeSpan? delay;
 try
 {
  result = await publicClientApplication.AcquireTokenForClient(scopes, account)
                                        .ExecuteAsync();
 }
 catch (MsalServiceException serviceException)
 {
  if (ex.ErrorCode == "temporarily_unavailable")
  {
   RetryConditionHeaderValue retryAfter = serviceException.Headers.RetryAfter;
   if (retryAfter.Delta.HasValue)
   {
    delay = retryAfter.Delta;
   }
   else if (retryAfter.Date.HasValue)
   {
    delay = retryAfter.Date.Value.Offset;
   }
  }
 }

    …
 if (delay.HasValue)
 {
  Thread.Sleep((int)delay.Value.TotalMilliseconds); // sleep or other
  retry = true;
 }
} while (retry);
```
