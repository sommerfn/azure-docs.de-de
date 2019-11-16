---
title: Plastischer Reader-SDK – Referenz
titleSuffix: Azure Cognitive Services
description: Das Plastischer Reader-SDK ist eine JavaScript-Bibliothek, die es Ihnen ermöglicht, den Plastischen Reader in Ihre Webanwendung zu integrieren.
services: cognitive-services
author: metanMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: reference
ms.date: 06/20/2019
ms.author: metan
ms.openlocfilehash: 09244b634fa2603a7dc92af3c78d171f8d6bd9df
ms.sourcegitcommit: bc193bc4df4b85d3f05538b5e7274df2138a4574
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2019
ms.locfileid: "73903109"
---
# <a name="immersive-reader-sdk-reference-guide"></a>SDK für plastischen Reader: Referenzhandbuch

Das Plastischer Reader-SDK ist eine JavaScript-Bibliothek, die es Ihnen ermöglicht, den Plastischen Reader in Ihre Webanwendung zu integrieren.

## <a name="functions"></a>Functions

Das SDK macht folgende Funktionen verfügbar:

- [`ImmersiveReader.launchAsync(token, subdomain, content, options)`](#launchasync)

- [`ImmersiveReader.close()`](#close)

- [`ImmersiveReader.renderButtons(options)`](#renderbuttons)

## <a name="launchasync"></a>launchAsync

Startet den Plastischen Reader in einem `iframe` in Ihrer Webanwendung.

```typescript
launchAsync(token: string, subdomain: string, content: Content, options?: Options): Promise<HTMLDivElement>;
```

### <a name="parameters"></a>Parameter

| NAME | type | BESCHREIBUNG |
| ---- | ---- |------------ |
| `token` | Zeichenfolge | Das Azure AD-Authentifizierungstoken. Informationen finden Sie in der [Anleitung zur Azure AD-Authentifizierung](./azure-active-directory-authentication.md). |
| `subdomain` | Zeichenfolge | Die benutzerdefinierte Unterdomäne ihrer Plastischer Reader-Ressource in Azure. Informationen finden Sie in der [Anleitung zur Azure AD-Authentifizierung](./azure-active-directory-authentication.md). |
| `content` | [Inhalt](#content) | Ein Objekt mit dem Inhalt, der im Plastischen Reader angezeigt werden soll. |
| `options` | [Optionen](#options) | Optionen zum Konfigurieren bestimmter Verhaltensweisen des Plastischen Readers. Optional. |

### <a name="returns"></a>Rückgabe

Gibt ein `Promise<HTMLDivElement>` zurück, das beim Laden des Plastischen Readers aufgelöst wird. Das `Promise` löst ein `div`-Element auf, dessen einziges untergeordnetes Objekt ein `iframe`-Element mit der Seite „Plastischer Reader“ ist.

### <a name="exceptions"></a>Ausnahmen

Das zurückgegebene `Promise` wird mit einem [`Error`](#error)-Objekt zurückgewiesen, wenn das Laden des Plastischen Readers fehlschlägt. Weitere Informationen finden Sie in den [Fehlercodes](#error-codes).

## <a name="close"></a>close

Schließt den Plastischen Reader.

Ein Beispiel eines Anwendungsfalls für diese Funktion ist, wenn die Schaltfläche „Beenden“ durch die Einstellung ```hideExitButton: true``` in [Optionen](#options) ausgeblendet wird. Dann kann eine andere Schaltfläche (z. B. der Pfeil „Zurück“ eines mobilen Headers) diese ```close```-Funktion aufrufen, wenn sie angeklickt wird.

```typescript
close(): void;
```

## <a name="renderbuttons"></a>renderButtons

Diese Funktion formatiert und aktualisiert die Schaltflächenelemente für „Plastischer Reader“ des Dokuments. Wenn ```options.elements``` angegeben wird, rendert diese Funktion Schaltflächen in ```options.elements```. Andernfalls werden die Schaltflächen innerhalb der Elemente des Dokuments dargestellt, die die Klasse ```immersive-reader-button``` aufweisen.

Diese Funktion wird vom SDK automatisch beim Laden des Fensters aufgerufen.

Weitere Renderingoptionen finden Sie unter [Optionale Attribute](#optional-attributes).

```typescript
renderButtons(options?: RenderButtonsOptions): void;
```

### <a name="parameters"></a>Parameter

| NAME | type | BESCHREIBUNG |
| ---- | ---- |------------ |
| `options` | [RenderButtonsOptions](#renderbuttonsoptions) | Optionen zum Konfigurieren bestimmter Verhaltensweisen der Funktion „renderButtons“. Optional. |

## <a name="types"></a>Typen

### <a name="content"></a>Inhalt

Enthält den Inhalt, der im Plastischen Reader angezeigt werden soll.

```typescript
{
    title?: string;    // Title text shown at the top of the Immersive Reader (optional)
    chunks: Chunk[];   // Array of chunks
}
```

### <a name="chunk"></a>Block

Ein einzelner Datenblock, der in den Inhalt des Plastischen Readers übertragen wird.

```typescript
{
    content: string;        // Plain text string
    lang?: string;          // Language of the text, e.g. en, es-ES (optional). Language will be detected automatically if not specified.
    mimeType?: string;      // MIME type of the content (optional). Currently 'text/plain', 'application/mathml+xml', and 'text/html' are supported. Defaults to 'text/plain' if not specified.
}
```

#### <a name="supported-mime-types"></a>Unterstützte MIME-Typen

| MIME-Typ | BESCHREIBUNG |
| --------- | ----------- |
| text/plain | Nur-Text. |
| text/html | HTML-Inhalt. [Weitere Informationen](#html-support)|
| application/mathml+xml | Mathematische Markupsprache (MathML). [Weitere Informationen](https://developer.mozilla.org/en-US/docs/Web/MathML)
| application/vnd.openxmlformats-officedocument.wordprocessingml.document | Dokument im DOCX-Format von Microsoft Word.

### <a name="html-support"></a>HTML-Unterstützung
| HTML | Unterstützter Inhalt |
| --------- | ----------- |
| Schriftschnitte | Bold, Italic, Underline, Code, Strikethrough, Superscript, Subscript |
| Unsortierte Listen | Disc, Circle, Square |
| Sortierte Listen | Decimal, Upper-Alpha, Lower-Alpha, Upper-Roman, Lower-Roman |
| Links | In Kürze verfügbar |

Nicht unterstützte Tags werden vergleichbar gerendert. Bilder und Tabellen werden derzeit nicht unterstützt.

### <a name="options"></a>Optionen

Enthält Eigenschaften, die bestimmte Verhaltensweisen des Plastischen Readers konfigurieren.

```typescript
{
    uiLang?: string;           // Language of the UI, e.g. en, es-ES (optional). Defaults to browser language if not specified.
    timeout?: number;          // Duration (in milliseconds) before launchAsync fails with a timeout error (default is 15000 ms).
    uiZIndex?: number;         // Z-index of the iframe that will be created (default is 1000)
    useWebview?: boolean;      // Use a webview tag instead of an iframe, for compatibility with Chrome Apps (default is false).
    onExit?: () => any;        // Executes when the Immersive Reader exits
    customDomain?: string;     // Reserved for internal use. Custom domain where the Immersive Reader webapp is hosted (default is null).
    allowFullscreen?: boolean; // The ability to toggle fullscreen (default is true).
    hideExitButton?: boolean;  // Whether or not to hide the Immersive Reader's exit button arrow (default is false). This should only be true if there is an alternative mechanism provided to exit the Immersive Reader (e.g a mobile toolbar's back arrow).
}
```

### <a name="renderbuttonsoptions"></a>RenderButtonsOptions

Optionen für das Rendering der Schaltflächen des Plastischen Readers.

```typescript
{
    elements: HTMLDivElement[];    // Elements to render the Immersive Reader buttons in
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
| TokenExpired | Das angegebene Token ist abgelaufen. |
| Gedrosselt | Das Limit für die Aufrufrate wurde überschritten. |

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
| `data-locale` | Legt das Gebietsschema fest. Beispiel: `en-US` oder `fr-FR`. Der Standardwert ist „Englisch“ `en`. |
| `data-icon-px-size` | Legt die Größe des Symbols in Pixel fest. Der Standardwert ist „20px“. |

## <a name="browser-support"></a>Browserunterstützung

Verwenden Sie die neuesten Versionen der folgenden Browser, um mit dem Plastischen Reader optimale Ergebnisse zu erzielen.

* Microsoft Edge
* Internet Explorer 11
* Google Chrome
* Mozilla Firefox
* Apple Safari

## <a name="next-steps"></a>Nächste Schritte

* Untersuchen des [Plastischer Reader-SDKs auf GitHub](https://github.com/microsoft/immersive-reader-sdk)
* [Schnellstart: Erstellen einer Web-App zum Starten des Plastischen Readers (C#)](./quickstart.md)
