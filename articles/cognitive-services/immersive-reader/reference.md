---
title: Plastischer Reader-SDK – Referenz
titleSuffix: Azure Cognitive Services
description: Referenz zum Plastischer Reader-SDK
services: cognitive-services
author: metanMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: reference
ms.date: 06/20/2019
ms.author: metan
ms.openlocfilehash: 485e8626af4266492e02d4f9fbe4af486e10c082
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2019
ms.locfileid: "67718389"
---
# <a name="immersive-reader-sdk-reference"></a>Plastischer Reader-SDK – Referenz

Das Plastischer Reader-SDK ist eine JavaScript-Bibliothek, die es Ihnen ermöglicht, den Plastischen Reader in Ihre Webanwendung zu integrieren.

## <a name="functions"></a>Functions

Das SDK macht eine einzige Funktion, `ImmersiveReader.launchAsync(token, resourceName, content, options)`, verfügbar.

### <a name="launchasync"></a>launchAsync

Startet den Plastischen Reader in einem `iframe` in Ihrer Webanwendung.

```typescript
launchAsync(token: string, resourceName: string, content: Content, options?: Options): Promise<HTMLDivElement>;
```

#### <a name="parameters"></a>Parameter

| NAME | type | BESCHREIBUNG |
| ---- | ---- |------------ |
| `token` | Zeichenfolge | Das Zugriffstoken, das aus dem Aufruf des `issueToken`-Endpunkts abgerufen wurde. |
| `resourceName` | Zeichenfolge | Reserviert. Muss auf `null` festgelegt sein. |
| `content` | [Inhalt](#content) | Ein Objekt mit dem Inhalt, der im Plastischen Reader angezeigt werden soll. |
| `options` | [Optionen](#options) | Optionen zum Konfigurieren bestimmter Verhaltensweisen des Plastischen Readers. Optional. |

#### <a name="returns"></a>Rückgabe

Gibt ein `Promise<HTMLDivElement>` zurück, das beim Laden des Plastischen Readers aufgelöst wird. Das `Promise` löst ein `div`-Element auf, dessen einziges untergeordnetes Objekt ein `iframe`-Element mit der Seite „Plastischer Reader“ ist.

#### <a name="exceptions"></a>Ausnahmen

Das zurückgegebene `Promise` wird mit einem [`Error`](#error)-Objekt zurückgewiesen, wenn das Laden des Plastischen Readers fehlschlägt. Weitere Informationen finden Sie in den [Fehlercodes](#error-codes).

## <a name="types"></a>Typen

### <a name="content"></a>Inhalt

Enthält den Inhalt, der im Plastischen Reader angezeigt werden soll.

```typescript
{
    title?: string;      // Title text shown at the top of the Immersive Reader (optional)
    chunks: [ {          // Array of chunks
        content: string; // Plain text string
        lang?: string;   // Language of the text, e.g. en, es-ES (optional). Language will be detected automatically if not specified.
        mimeType?: string; // MIME type of the content (optional). Defaults to 'text/plain' if not specified.
    } ];
}
```

#### <a name="supported-mime-types"></a>Unterstützte MIME-Typen

| MIME-Typ | BESCHREIBUNG |
| --------- | ----------- |
| text/plain | Nur-Text. |
| application/mathml+xml | Mathematische Markupsprache (MathML). [Weitere Informationen](https://developer.mozilla.org/en-US/docs/Web/MathML)

### <a name="options"></a>Optionen

Enthält Eigenschaften, die bestimmte Verhaltensweisen des Plastischen Readers konfigurieren.

```typescript
{
    uiLang?: string;   // Language of the UI, e.g. en, es-ES (optional). Defaults to browser language if not specified.
    timeout?: number;  // Duration (in milliseconds) before launchAsync fails with a timeout error (default is 15000 ms).
    uiZIndex?: number; // Z-index of the iframe that will be created (default is 1000)
    useWebview?: boolean; // Use a webview tag instead of an iframe, for compatibility with Chrome Apps (default is false).
}
```

### <a name="error"></a>Error

Enthält Informationen zum Fehler.

```typescript
{
    code: string;    // One of a set of error codes
    message: string; // Human-readable representation of the error
}
```

#### <a name="error-codes"></a>Fehlercodes

| Code | BESCHREIBUNG |
| ---- | ----------- |
| BadArgument | Das angegebene Argument ist ungültig. Details finden Sie unter `message`. |
| Timeout | Der Plastische Reader konnte innerhalb des angegebenen Zeitlimits nicht geladen werden. |
| TokenExpired| Das angegebene Token ist abgelaufen. |

## <a name="launching-the-immersive-reader"></a>Starten des Plastischen Readers

Das SDK stellt einen Standardstil für die Schaltfläche zum Starten des Plastischen Readers bereit. Verwenden Sie das Klassenattribut `immersive-reader-button` zum Aktivieren dieses Stils.

```html
<div class='immersive-reader-button'></div>
```

### <a name="optional-attributes"></a>Optionale Attribute

Verwenden Sie die folgenden Attribute, um das Aussehen und Verhalten der Schaltfläche zu konfigurieren.

| Attribut | BESCHREIBUNG |
| --------- | ----------- |
| `data-button-style` | Legt den Stil der Schaltfläche fest. Kann `icon`, `text` oder `iconAndText` sein. Der Standardwert lautet `icon`. |
| `data-locale` | Legt das Gebietsschema fest, z.B. `en-US`, `fr-FR`. Der Standardwert ist „Englisch“. |
| `data-icon-px-size` | Legt die Größe des Symbols in Pixel fest. Der Standardwert ist „20px“. |

## <a name="browser-support"></a>Browserunterstützung

Verwenden Sie die neuesten Versionen der folgenden Browser, um mit dem Plastischen Reader optimale Ergebnisse zu erzielen.

* Microsoft Edge
* Internet Explorer 11
* Google Chrome
* Mozilla Firefox
* Apple Safari

## <a name="next-steps"></a>Nächste Schritte

* Untersuchen des [Plastischer Reader-SDKs auf GitHub](https://github.com/Microsoft/immersive-reader-sdk)
* [Schnellstart: Erstellen einer Web-App zum Starten des Plastischen Readers (C#)](./quickstart.md)