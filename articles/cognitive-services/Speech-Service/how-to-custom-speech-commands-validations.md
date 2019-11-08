---
title: 'Anleitung: Hinzufügen von Validierungen zu Parametern benutzerdefinierter Befehle (Vorschau)'
titleSuffix: Azure Cognitive Services
description: In diesem Artikel fügen Sie Validierungen zu den Parametern für benutzerdefinierte Befehle hinzu.
services: cognitive-services
author: donkim
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/09/2019
ms.author: donkim
ms.openlocfilehash: 64e092405686caca7baeaf58f19d577a3f80e169
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73506477"
---
# <a name="how-to-add-validations-to-custom-command-parameters-preview"></a>Anleitung: Hinzufügen von Validierungen zu Parametern benutzerdefinierter Befehle (Vorschau)

In diesem Artikel erfahren Sie, wie Sie Validierungen zu Parametern hinzufügen und zur Korrektur auffordern können.

## <a name="prerequisites"></a>Voraussetzungen

Sie müssen die Schritte in den folgenden Artikeln durchgeführt haben:

- [Schnellstart: Erstellen eines benutzerdefinierten Befehls (Vorschau)](./quickstart-custom-speech-commands-create-new.md)
- [Schnellstart: Erstellen eines benutzerdefinierten Befehls mit Parametern (Vorschau)](./quickstart-custom-speech-commands-create-parameters.md)

## <a name="create-a-settemperature-command"></a>Erstellen eines SetTemperature-Befehls

Um Validierungen zu veranschaulichen, erstellen wir einen neuen Befehl, mit dem der Benutzer die Temperatur einstellen kann.

1. Öffnen Sie Ihre zuvor erstellte Anwendung für benutzerdefinierte Befehle in [Speech Studio](https://speech.microsoft.com/).
1. Erstellen eines neuen **SetTemperature**-Befehls
1. Hinzufügen eines Parameters für die Zieltemperatur
1. Hinzufügen einer Validierung für den Temperaturparameter
   > [!div class="mx-imgBorder"]
   > ![Hinzufügen einer Bereichsvalidierung](media/custom-speech-commands/validations-add-temperature.png)

   | Einstellung           | Empfohlener Wert                                          | BESCHREIBUNG                                                                                      |
   | ----------------- | -------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
   | NAME              | Temperatur                                              | Ein aussagekräftiger Name für Ihren Befehlsparameter.                                                    |
   | Erforderlich          | true                                                     | Kontrollkästchen, das angibt, ob ein Wert für diesen Parameter vor dem Abschließen des Befehls erforderlich ist. |
   | Antwortvorlage | „Welche Temperatur hätten Sie gerne?“                       | Eine Aufforderung, nach dem Wert dieses Parameters zu fragen, wenn er nicht bekannt ist.                              |
   | type              | Number                                                   | Der Typ des Parameters, z. B. Zahl, Zeichenfolge oder Datum/Uhrzeit.                                      |
   | Überprüfen        | Mindestwert: 60, Maximalwert: 80                             | Bei Zahlenparametern der zulässige Wertebereich für den Parameter                              |
   | Antwortvorlage | „Es kann nur ein Wert zwischen 16 und 27 Grad eingestellt werden.“        | Aufforderung zur Eingabe eines aktualisierten Werts, wenn bei der Validierung ein Fehler auftritt.                                       |

1. Hinzufügen einiger Beispielsätze

   ```
   set the temperature to {Temperature} degrees
   change the temperature to {Temperature}
   set the temperature
   change the temperature
   ```

1. Hinzufügen einer Vervollständigungsregel zur Bestätigung des Ergebnisses

   | Einstellung    | Empfohlener Wert                                         | BESCHREIBUNG                                        |
   | ---------- | ------------------------------------------------------- | -------------------------------------------------- |
   | Regelname  | Bestätigungsmeldung                                    | Ein Name, der den Zweck der Regel beschreibt.          |
   | Bedingungen | Erforderlicher Parameter – Temperature                        | Bedingungen, die bestimmen, wann die Regel ausgeführt werden kann.    |
   | Aktionen    | SpeechResponse – „OK, stelle {Temperature} Grad ein“ | Die durchzuführende Aktion, wenn die Bedingung der Regel erfüllt ist (true). |

> [!TIP]
> In diesem Beispiel wird eine Sprachantwort verwendet, um das Ergebnis zu bestätigen. Beispiele für das Vervollständigen des Befehls mit einer Clientaktion finden Sie unter: [How To: Ausführen der Befehle auf dem Client mit dem Speech SDK (Vorschau)](./how-to-custom-speech-commands-fulfill-sdk.md)

## <a name="try-it-out"></a>Ausprobieren

Wählen Sie den Testbereich aus, und testen Sie einige Interaktionen.

- Eingabe: Stelle die Temperatur auf 22 Grad ein
- Ausgabe: „OK, Einstellung auf 22 Grad“

- Eingabe: Stelle die Temperatur auf 7 Grad ein
- Ausgabe: „Es kann nur ein Wert zwischen 16 und 27 Grad eingestellt werden.“
- Eingabe: Stelle stattdessen 22 Grad ein
- Ausgabe: „OK, Einstellung auf 22 Grad“
