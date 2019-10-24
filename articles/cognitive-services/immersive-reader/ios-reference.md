---
title: Referenz zum iOS SDK für den plastischen Reader
titleSuffix: Azure Cognitive Services
description: Referenz zum iOS SDK für den plastischen Reader
services: cognitive-services
author: metanMSFT
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: reference
ms.date: 08/01/2019
ms.author: metan
ms.openlocfilehash: 1f85740c358bd0949fed9c954537f0926538995d
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72388113"
---
# <a name="immersive-reader-sdk-reference-for-ios"></a>SDK für plastischen Reader: Referenz für iOS

Das iOS SDK für den plastischen Reader ist eine Swift CocoaPod-Instanz, die es Ihnen ermöglicht, den plastischen Reader in Ihre Webanwendung zu integrieren.

## <a name="functions"></a>Functions

Das SDK macht eine einzige Funktion, `launchImmersiveReader(navController, token, subdomain, content, options, onSuccess, onFailure)`, verfügbar.

### <a name="launchimmersivereader"></a>launchImmersiveReader

Startet den plastischen Reader, indem in Ihrer iOS-Anwendung ein View Controller gestartet wird.

```swift
public func launchImmersiveReader(navController: UINavigationController, token: String, subdomain: String, content: Content, options: Options?, onSuccess: @escaping () -> Void, onFailure: @escaping (_ error: Error) -> Void)
```

#### <a name="parameters"></a>Parameter

| NAME | type | BESCHREIBUNG |
| ---- | ---- |------------ |
| `navController` | UINavigationController | Der Navigation Controller für die iOS-Anwendung, mit der die Funktion aufgerufen wird. |
| `token` | Zeichenfolge | Das Azure AD-Authentifizierungstoken. Informationen finden Sie in der [Anleitung zur Azure AD-Authentifizierung](./azure-active-directory-authentication.md). |
| `subdomain` | Zeichenfolge | Die benutzerdefinierte Unterdomäne ihrer Plastischer Reader-Ressource in Azure. Informationen finden Sie in der [Anleitung zur Azure AD-Authentifizierung](./azure-active-directory-authentication.md). |
| `content` | [Inhalt](#content) | Ein Objekt mit dem Inhalt, der im Plastischen Reader angezeigt werden soll. |
| `options` | [Optionen](#options) | Optionen zum Konfigurieren bestimmter Verhaltensweisen des Plastischen Readers. Optional. |
| `onSuccess` | () -> Void | Ein Abschluss, der aufgerufen wird, wenn der plastische Reader erfolgreich gestartet wurde. |
| `onFailure` | (_ error: [Error](#error)) -> Void | Ein Abschluss, der aufgerufen wird, wenn der plastische Reader nicht geladen werden kann. Bei diesem Abschluss wird ein [`Error`](#error)-Objekt zurückgegeben, das einen Fehlercode und eine Fehlermeldung zum Fehler darstellt. Weitere Informationen finden Sie in den [Fehlercodes](#error-codes). |

## <a name="types"></a>Typen

### <a name="content"></a>Inhalt

Enthält den Inhalt, der im Plastischen Reader angezeigt werden soll.

```swift
struct Content: Encodable {
    var title: String
    var chunks: [Chunk]
}
```

#### <a name="supported-mime-types"></a>Unterstützte MIME-Typen

| MIME-Typ | BESCHREIBUNG |
| --------- | ----------- |
| text/plain | Nur-Text. |
| application/mathml+xml | Mathematische Markupsprache (MathML). [Weitere Informationen](https://developer.mozilla.org/en-US/docs/Web/MathML)

### <a name="options"></a>Optionen

Enthält Eigenschaften, die bestimmte Verhaltensweisen des Plastischen Readers konfigurieren.

```swift
struct Options {
    var uiLang: String?
    var timeout: TimeInterval?
}
```

### <a name="error"></a>Error

Enthält Informationen zum Fehler.

```swift
struct Error {
    var code: String
    var message: String
}
```

#### <a name="error-codes"></a>Fehlercodes

| Code | BESCHREIBUNG |
| ---- | ----------- |
| BadArgument | Das angegebene Argument ist ungültig. Details finden Sie unter `message`. |
| Timeout | Der Plastische Reader konnte innerhalb des angegebenen Zeitlimits nicht geladen werden. |
| TokenExpired | Das angegebene Token ist abgelaufen. |
| Gedrosselt | Das Limit für die Aufrufrate wurde überschritten. |
| InternalError | Im View Controller des plastischen Readers ist ein interner Fehler aufgetreten. Details hierzu finden Sie unter `message`.|

## <a name="os-version-support"></a>Unterstützung der Betriebssystemversion

Das iOS SDK für den plastischen Reader wird für iOS 9.0 oder höher auf dem iPad und dem iPhone unterstützt.

## <a name="next-steps"></a>Nächste Schritte

* Erkunden Sie das [iOS SDK für den plastischen Reader auf GitHub](https://github.com/microsoft/immersive-reader-sdk/tree/master/iOS).
* [Schnellstart: Erstellen einer iOS-App zum Starten von Plastischer Reader (Swift)](./ios-quickstart.md)