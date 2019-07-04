---
title: Verhindern der Inhaltsübersetzung – Textübersetzungs-API
titlesuffix: Azure Cognitive Services
description: Verhindern Sie mit der Textübersetzungs-API die Übersetzung von Inhalten.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: 02c4e90879b81e29f3972d262f36d5c6b6d8e841
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67448283"
---
# <a name="how-to-prevent-translation-of-content-with-the-translator-text-api"></a>Verhindern der Inhaltsübersetzung mit der Textübersetzungs-API

Die Textübersetzungs-API ermöglicht es, Inhalte so zu kennzeichnen, dass sie nicht übersetzt werden. Sie können beispielsweise Code, einen Markennamen oder einzelne Wörter/Ausdrücke markieren, deren Übersetzung nicht sinnvoll ist.

## <a name="methods-for-preventing-translation"></a>Methoden zum Verhindern der Übersetzung
1. Verwenden Sie als Escapezeichen ein Twitter-Tag wie @somethingtopassthrough oder #somethingtopassthrough. Entfernen Sie das Escapezeichen nach der Übersetzung.

2. Kennzeichnen Sie Ihren Inhalt mit `notranslate`.

   Beispiel:

   ```html
   <div class="notranslate">This will not be translated.</div>
   <div>This will be translated. </div>
   ```

3. Verwenden Sie das [dynamische Wörterbuch](dynamic-dictionary.md), um eine bestimmte Übersetzung vorzuschreiben.

4. Übergeben Sie die Zeichenfolge nicht zur Übersetzung an die Textübersetzungs-API.

5. Benutzerdefinierter Translator: Verwenden Sie ein [Wörterbuch in „Benutzerdefinierter Translator“](custom-translator/what-is-dictionary.md), um die Übersetzung eines Ausdrucks mit einer Wahrscheinlichkeit von 100% vorzuschreiben.


## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Vermeiden einer Übersetzung in Ihrem Textübersetzungs-API-Aufruf](reference/v3-0-translate.md)
