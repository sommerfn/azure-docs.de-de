---
title: 'Schnellstart: Erstellen eines benutzerdefinierten Befehls mit Parametern (Vorschau)'
titleSuffix: Azure Cognitive Services
description: In diesem Artikel fügen Sie einer Anwendung für benutzerdefinierte Befehle Parameter hinzu.
services: cognitive-services
author: donkim
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/09/2019
ms.author: donkim
ms.openlocfilehash: 010a9331bb4533bc84957261f312993c4cc7ffe8
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73506689"
---
# <a name="quickstart-create-a-custom-command-with-parameters-preview"></a>Schnellstart: Erstellen eines benutzerdefinierten Befehls mit Parametern (Vorschau)

Im [vorherigen Artikel](./quickstart-custom-speech-commands-create-new.md) wurde ein neues Projekt für benutzerdefinierte Befehle erstellt, das auf Befehle ohne Parameter antwortet.

In diesem Artikel wird diese Anwendung um Parameter erweitert, sodass sie das Ein- und Ausschalten mehrerer Geräte übernehmen kann.

## <a name="create-parameters"></a>Erstellen von Parametern

1. Öffnen Sie das Projekt, [das wir zuvor erstellt haben](./quickstart-custom-speech-commands-create-new.md).
1. Da der Befehl jetzt das Ein- und Ausschalten übernimmt, benennen Sie den Befehl in „TurnOnOff“ um.
   - Bewegen Sie den Mauszeiger über den Namen des Befehls, und wählen Sie das Bearbeitungssymbol aus, um den Namen zu ändern.
1. Erstellen Sie einen neuen Parameter, der angibt, ob der Benutzer das Gerät ein- oder ausschalten möchte.
   - Wählen Sie das Symbol `+` neben dem Parameterabschnitt aus.

   > [!div class="mx-imgBorder"]
   > ![Erstellen von Parametern](media/custom-speech-commands/create-on-off-parameter.png)

   | Einstellung            | Empfohlener Wert | BESCHREIBUNG                                                                                               |
   | ------------------ | --------------- | --------------------------------------------------------------------------------------------------------- |
   | NAME               | OnOff           | Ein aussagekräftiger Name für Ihren Parameter.                                                                     |
   | Is Global          | Deaktiviert       | Kontrollkästchen, das angibt, ob ein Wert für diesen Parameter global auf alle Befehle im Projekt angewendet wird. |
   | Erforderlich           | Aktiviert         | Kontrollkästchen, das angibt, ob ein Wert für diesen Parameter vor dem Abschließen des Befehls erforderlich ist.          |
   | Antwortvorlage  | „Ein“ oder „Aus“?      | Eine Aufforderung, nach dem Wert dieses Parameters zu fragen, wenn er nicht bekannt ist.                                       |
   | type               | Zeichenfolge          | Der Typ des Parameters, z. B. Zahl, Zeichenfolge oder Datum/Uhrzeit.                                               |
   | Konfiguration      | Zeichenfolgenliste     | Für Zeichenfolgen beschränkt eine Zeichenfolgenliste die Eingaben auf eine Reihe von möglichen Werten.                                      |
   | Zeichenfolgenlistenwerte | on, off         | Für einen Zeichenfolgenlisten-Parameter der Satz möglicher Werte und deren Synonyme.                                |

   - Als nächstes wählen Sie erneut das Symbol `+` aus, um einen zweiten Parameter hinzuzufügen, der den Namen der Geräte darstellt. Für dieses Beispiel ein Fernseher und ein Lüfter.

   | Einstellung            | Empfohlener Wert   | BESCHREIBUNG                                                                                               |
   | ------------------ | ----------------- | --------------------------------------------------------------------------------------------------------- |
   | NAME               | SubjectDevice     | Ein aussagekräftiger Name für Ihren Parameter.                                                                     |
   | Is Global          | Deaktiviert         | Kontrollkästchen, das angibt, ob ein Wert für diesen Parameter global auf alle Befehle im Projekt angewendet wird. |
   | Erforderlich           | Aktiviert           | Kontrollkästchen, das angibt, ob ein Wert für diesen Parameter vor dem Abschließen des Befehls erforderlich ist.          |
   | Antwortvorlage  | Welches Gerät?     | Eine Aufforderung, nach dem Wert dieses Parameters zu fragen, wenn er nicht bekannt ist.                                       |
   | type               | Zeichenfolge            | Der Typ des Parameters, z. B. Zahl, Zeichenfolge oder Datum/Uhrzeit.                                               |
   | Konfiguration      | Zeichenfolgenliste       | Für Zeichenfolgen beschränkt eine Zeichenfolgenliste die Eingaben auf eine Reihe von möglichen Werten.                                      |
   | Zeichenfolgenlistenwerte | Fernseher, Lüfter           | Für einen Zeichenfolgenlisten-Parameter der Satz möglicher Werte und deren Synonyme.                                |
   | Synonyme (Fernseher)      | Fernsehgerät, TV | Optionale Synonyme für jeden möglichen Wert eines Zeichenfolgenlistenparameters                                      |

## <a name="add-sample-sentences"></a>Hinzufügen von Beispielsätzen

Bei Parametern ist es hilfreich, Beispielsätze hinzuzufügen, die alle möglichen Kombinationen abdecken. Beispiel:

1. Vollständige Parameterinformationen – `"turn {OnOff} the {SubjectDevice}"`
1. Teilweise Parameterinformationen – `"turn it {OnOff}"`
1. Keine Parameterinformationen – `"turn something"`

Beispielsätze mit unterschiedlichem Informationsumfang ermöglichen es der Anwendung für benutzerdefinierte Befehle, sowohl einmalige als auch mehrere Lösungen mit Teilinformationen aufzulösen.

Bearbeiten Sie vor diesem Hintergrund die Beispielsätze, um die Parameter wie unten vorgeschlagen zu verwenden.

> [!TIP]
> Verwenden Sie im Editor für Beispielsätze geschweifte Klammern, um auf Ihre Parameter zu verweisen. - `turn {OnOff} the {SubjectDevice}` Verwenden Sie die Vervollständigung mit der TAB-TASTE, um auf zuvor erstellte Parameter zu verweisen.

> [!div class="mx-imgBorder"]
> ![Beispielsätze mit Parametern](media/custom-speech-commands/create-parameter-sentences.png)

```
turn {OnOff} the {SubjectDevice}
{SubjectDevice} {OnOff}
turn it {OnOff}
turn something {OnOff}
turn something
```

## <a name="add-parameters-to-completion-rule"></a>Hinzufügen von Parametern zur Vervollständigungsregel

Ändern Sie die Vervollständigungsregel, die Sie im [vorherigen Schnellstart](./quickstart-custom-speech-commands-create-new.md) erstellt haben:

1. Fügen Sie eine neue Bedingung hinzu, und wählen Sie den erforderlichen Parameter aus. Wählen Sie sowohl `OnOff` als auch `SubjectDevice` aus.
1. Bearbeiten Sie die Sprachantwortaktion, um `OnOff` und `SubjectDevice` zu verwenden:

   ```
   Ok, turning {OnOff} the {SubjectDevice}
   ```

## <a name="try-it-out"></a>Ausprobieren

Öffnen Sie den Testchatbereich, und testen Sie einige Interaktionen.

- Eingabe: Schalte Fernseher aus
- Ausgabe: OK, schalte Fernseher aus

- Eingabe: Schalte Fernsehgerät aus
- Ausgabe: OK, schalte Fernseher aus

- Eingabe: Schalte es aus
- Ausgabe: Welches Gerät?
- Eingabe: den Fernseher
- Ausgabe: OK, schalte Fernseher aus

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Schnellstart: Herstellen einer Verbindung mit einer Anwendung für benutzerdefinierte Befehle mit dem Speech SDK (Vorschau)](./quickstart-custom-speech-commands-speech-sdk.md)

