---
title: Protokollierung in MSAL-Anwendungen | Microsoft Identity Platform
description: Hier finden Sie Informationen zur Protokollierung in Microsoft Authentication Library-Anwendungen (MSAL-Anwendungen).
services: active-directory
documentationcenter: dev-center-name
author: TylerMSFT
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/28/2019
ms.author: twhitney
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4dad8a276cd40b1ff04bbced833b5d70cec4fc87
ms.sourcegitcommit: 263a69b70949099457620037c988dc590d7c7854
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "71268582"
---
# <a name="logging-in-msal-applications"></a>Protokollierung in MSAL-Anwendungen

Mit Microsoft Authentication Library-Apps (MSAL-Apps) werden Protokollmeldungen generiert, die Sie beim Diagnostizieren von Problemen unterstützen. Eine App kann die Protokollierung mit wenigen Codezeilen konfigurieren und hat benutzerdefinierte Kontrolle über den Detailgrad und darüber, ob personenbezogene Daten und Organisationsdaten protokolliert werden oder nicht. Es wird empfohlen, einen MSAL-Protokollierungsrückruf festzulegen und Benutzern eine Möglichkeit zu bieten, Protokolle zu übermitteln, wenn Authentifizierungsprobleme auftreten.

## <a name="logging-levels"></a>Protokolliergrade

MSAL bietet mehrere Grade für Protokollierungsdetails:

- Fehler Gibt an, dass ein Problem aufgetreten ist und ein Fehler generiert wurde. Verwenden Sie diesen Grad zum Debuggen und Identifizieren von Problemen.
- Warnung: Es muss nicht unbedingt zu einem Fehler oder Ausfall gekommen sein. Dieser Grad ist für das Diagnostizieren und Ermitteln von Problemen vorgesehen.
- Info: MSAL protokolliert Ereignisse, die für Informationszwecke und nicht unbedingt für das Debuggen vorgesehen sind.
- Verbose: Standard. Msal protokolliert die vollständigen Details des Bibliotheksverhaltens.

## <a name="personal-and-organizational-data"></a>Personenbezogene Daten und Organisationsdaten

Die MSAL-Protokollierung erfasst standardmäßig keine streng vertraulichen personenbezogenen Daten oder Organisationsdaten. Die Bibliothek bietet jedoch die Option, die Protokollierung von personenbezogenen Daten und Organisationsdaten zu aktivieren.

## <a name="logging-in-msalnet"></a>Protokollierung in MSAL.NET

 > [!NOTE]
 > Weitere Informationen zu MSAL.NET finden Sie im [Wiki zu MSAL.NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki). Hier finden Sie Beispiele zur MSAL.NET-Protokollierung und mehr.
 
In MSAL 3.x wird die Protokollierung bei der App-Erstellung pro Anwendung mit dem Generatormodifizierer `.WithLogging` festgelegt. Bei dieser Methode können optionale Parameter verwendet werden:

- Mit *Level* können Sie den gewünschten Protokolliergrad festlegen. Wenn Sie den Parameter auf „Errors“ festlegen, werden nur Fehler protokolliert.
- Mit *PiiLoggingEnabled* können Sie personenbezogene Daten und Organisationsdaten protokollieren, wenn der Parameter auf „true“ festgelegt ist. Dieser Parameter ist standardmäßig auf FALSE festgelegt, damit Ihre Anwendung keine personenbezogenen Daten protokolliert.
- *LogCallback* ist auf einen Delegaten festgelegt, der die Protokollierung ausführt. Wenn *PiiLoggingEnabled* auf „true“ festgelegt ist, empfängt diese Methode die Nachrichten zweimal: einmal mit dem Parameter *containsPii* gleich „false“ mit der Nachricht ohne personenbezogene Daten und ein zweites Mal mit dem Parameter *containsPii* gleich „true“ mit der Nachricht, die personenbezogene Daten enthalten kann. In einigen Fällen (wenn die Nachricht keine personenbezogenen Daten enthält) ist die Nachricht dieselbe.
- *DefaultLoggingEnabled* aktiviert die Standardprotokollierung für die Plattform. Der Parameter ist standardmäßig auf „false“ festgelegt. Wenn Sie den Parameter auf „true“ festlegen, wird in Desktop-/UWP-Anwendungen die Ereignisablaufverfolgung, unter iOS NSLog und unter Android Logcat verwendet.

```csharp
class Program
 {
  private static void Log(LogLevel level, string message, bool containsPii)
  {
     if (containsPii)
     {
        Console.ForegroundColor = ConsoleColor.Red;
     }
     Console.WriteLine($"{level} {message}");
     Console.ResetColor();
  }

  static void Main(string[] args)
  {
    var scopes = new string[] { "User.Read" };

    var application = PublicClientApplicationBuilder.Create("<clientID>")
                      .WithLogging(Log, LogLevel.Info, true)
                      .Build();

    AuthenticationResult result = application.AcquireTokenInteractive(scopes)
                                             .ExecuteAsync().Result;
  }
 }
 ```

 ## <a name="logging-in-msaljs"></a>Protokollierung in MSAL.js

 Sie können die Protokollierung in MSAL.js aktivieren, indem Sie bei der Konfiguration ein Protokollierungsobjekt zum Erstellen einer `UserAgentApplication`-Instanz übergeben. Dieses Protokollierungsobjekt weist die folgenden Eigenschaften auf:

- `localCallback`: Eine Rückrufinstanz, die vom Entwickler bereitgestellt werden kann, um Protokolle benutzerdefiniert zu verarbeiten und zu veröffentlichen. Implementieren Sie die „localCallback“-Methode abhängig davon, wie Sie Protokolle umleiten möchten.

- `level` (optional): Der konfigurierbare Protokolliergrad. Die folgenden Protokollebenen werden unterstützt: Error, Warning, Info und Verbose. Der Standardwert lautet „Info“.

- `piiLoggingEnabled` (optional): Mit diesem Parameter können Sie personenbezogene Daten und Organisationsdaten protokollieren, wenn er auf TRUE festgelegt ist. Dieser Parameter ist standardmäßig auf FALSE festgelegt, damit Ihre Anwendung keine personenbezogenen Daten protokolliert. Protokolle mit personenbezogenen Daten werden nie in Standardausgaben wie die Konsole, Logcat oder NSLog geschrieben. Standardmäßig ist „false“ festgelegt.

- `correlationId` (optional): Ein eindeutiger Bezeichner, der die Anforderung der Antwort zu Debugzwecken zuordnet. Standardmäßig wird ein in RFC 4122 definierter GUID der Version 4 (128 Bit) verwendet.

```javascript
function loggerCallback(logLevel, message, containsPii) {
   console.log(message);
}

var msalConfig = {
    auth: {
        clientId: “abcd-ef12-gh34-ikkl-ashdjhlhsdg”,
    },
     system: {
             logger: new Msal.Logger(
                                loggerCallback ,{
                                     level: Msal.LogLevel.Verbose,
                                     piiLoggingEnabled: false,
                                     correlationId: '1234'
                                }
                        )
     }
}

var UserAgentApplication = new Msal.UserAgentApplication(msalConfig);
```

## <a name="logging-in-msal-for-ios-and-macos"></a>Protokollierung in MSAL für iOS und macOS

Legen Sie einen Rückruf fest, um die MSAL-Protokollierung zu erfassen und in die Protokollierung Ihrer eigenen Anwendung zu integrieren. Die Signatur für den Rückruf sieht folgendermaßen aus:

```objc
/*!
    The LogCallback block for the MSAL logger
 
    @param  level           The level of the log message
    @param  message         The message being logged
    @param  containsPII     If the message might contain Personally Identifiable Information (PII)
                            this will be true. Log messages possibly containing PII will not be
                            sent to the callback unless PIllLoggingEnabled is set to YES on the
                            logger.

 */
typedef void (^MSALLogCallback)(MSALLogLevel level, NSString *message, BOOL containsPII);
```

Beispiel:

Objective-C
```objc
[MSALGlobalConfig.loggerConfig setLogCallback:^(MSALLogLevel level, NSString *message, BOOL containsPII)
    {
        if (!containsPII)
        {
#if DEBUG
            // IMPORTANT: MSAL logs may contain sensitive information. Never output MSAL logs with NSLog, or print, directly unless you're running your application in debug mode. If you're writing MSAL logs to file, you must store the file securely.
            NSLog(@"MSAL log: %@", message);
#endif
        }
    }];
```

Swift
```swift
MSALGlobalConfig.loggerConfig.setLogCallback { (level, message, containsPII) in
    if let message = message, !containsPII
    {
#if DEBUG
    // IMPORTANT: MSAL logs may contain sensitive information. Never output MSAL logs with NSLog, or print, directly unless you're running your application in debug mode. If you're writing MSAL logs to file, you must store the file securely.
    print("MSAL log: \(message)")
#endif
    }
}
```

### <a name="personal-identifiable-information-pii"></a>Personenbezogene Informationen (Personal Identifiable Information, PII)

Standardmäßig erfasst und protokolliert MSAL keine PII. Die Bibliothek erlaubt App-Entwicklern jedoch, diese Möglichkeit über eine Eigenschaft in der MSALLogger-Klasse zu aktivieren. Nach der Aktivierung von personenbezogenen Informationen ist die App für die sichere Verarbeitung hochgradig sensibler Daten und die Einhaltung von gesetzlichen Anforderungen verantwortlich.

Objective-C
```objc
// By default, the `MSALLogger` doesn't capture any PII

// PII will be logged
MSALGlobalConfig.loggerConfig.piiEnabled = YES;

// PII will NOT be logged
MSALGlobalConfig.loggerConfig.piiEnabled = NO;
```

Swift
```swift
// By default, the `MSALLogger` doesn't capture any PII

// PII will be logged
MSALGlobalConfig.loggerConfig.piiEnabled = true

// PII will NOT be logged
MSALGlobalConfig.loggerConfig.piiEnabled = false
```

### <a name="logging-levels"></a>Protokolliergrade

Verwenden Sie einen der folgenden Werte, um den Protokolliergrad festzulegen, wenn Sie die Protokollierung mit MSAL für iOS und macOS ausführen:

|Level  |BESCHREIBUNG |
|---------|---------|
| `MSALLogLevelNothing`| Deaktivieren der gesamten Protokollierung |
| `MSALLogLevelError` | Standardgrad, gibt Informationen nur aus, wenn Fehler auftreten. |
| `MSALLogLevelWarning` | Warnungen |
| `MSALLogLevelInfo` |  Bibliothekseinstiegspunkte mit Parametern und verschiedenen Keychainvorgängen. |
|`MSALLogLevelVerbose`     |  API-Ablaufverfolgung       |

Beispiel:

Objective-C
```objc
MSALGlobalConfig.loggerConfig.logLevel = MSALLogLevelVerbose;
 ```
 
 Swift
```swift
MSALGlobalConfig.loggerConfig.logLevel = .verbose
 ```

### <a name="log-message-format"></a>Protokollieren des Nachrichtenformats

Der Nachrichtenteil der MSAL-Protokollmeldungen weist das Format `TID = <thread_id> MSAL <sdk_ver> <OS> <OS_ver> [timestamp - correlation_id] message` auf.

Beispiel:

`TID = 551563 MSAL 0.2.0 iOS Sim 12.0 [2018-09-24 00:36:38 - 36764181-EF53-4E4E-B3E5-16FE362CFC44] acquireToken returning with error: (MSALErrorDomain, -42400) User cancelled the authorization session.`

Das Bereitstellen von Korrelations-IDs und Zeitstempeln ist hilfreich zum Nachverfolgen von Problemen. Informationen zum Zeitstempel und zur Korrelations-ID sind in der Protokollmeldung verfügbar. Der einzige zuverlässige Ort zum Abrufen dieser Informationen sind die MSAL-Protokollierungsmeldungen.
