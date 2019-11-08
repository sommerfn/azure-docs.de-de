---
title: 'Schnellstart: Erstellen eines benutzerdefinierten Befehls (Vorschau)'
titleSuffix: Azure Cognitive Services
description: In diesem Artikel erstellen und testen Sie eine gehostete Anwendung für benutzerdefinierte Befehle.
services: cognitive-services
author: donkim
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/09/2019
ms.author: donkim
ms.openlocfilehash: bfe871ce6f7cd2fbd6ada4b825e41ebdf5ac3f12
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73506705"
---
# <a name="quickstart-create-a-custom-command-preview"></a>Schnellstart: Erstellen eines benutzerdefinierten Befehls (Vorschau)

In diesem Artikel erfahren Sie, wie Sie eine gehostete Anwendung für benutzerdefinierte Befehle erstellen und testen.
Die Anwendung erkennt eine Äußerung wie „Schalte Fernseher ein“ und antwortet mit einer einfachen Meldung „OK, schalte Fernseher ein“.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Speech-Abonnement. [Kostenloses Testen des Speech-Diensts](~/articles/cognitive-services/speech-service/get-started.md).

  > [!NOTE]
  > Während der Vorschau wird für Abonnementschlüssel nur die Region „westus2“ unterstützt.

- Ein LUIS-Erstellungsschlüssel ([Language Understanding](https://www.luis.ai/home)):
  1. Öffnen Sie einen Webbrowser, und navigieren Sie zum [Azure-Portal](https://portal.azure.com).
  1. Wählen Sie „Ressource erstellen“ aus.
  1. Suchen Sie nach [Language Understanding](https://aka.ms/sc-luis-all), und wählen Sie diese Option aus.
  1. Wählen Sie „Erstellung“ in den Erstellungsoptionen aus.
  1. Nachdem die Ressource bereitgestellt wurde, wechseln Sie zur Ressource, und kopieren Sie den Schlüssel aus dem Abschnitt „Schnellstart“ oder „Schlüssel“.

      > [!div class="mx-imgBorder"]
      > ![Erstellen einer Erstellungsressource](media/custom-speech-commands/resources-lu-authoring.png)

## <a name="go-to-the-speech-studio-for-custom-commands"></a>Wechseln Sie zum Speech Studio für benutzerdefinierte Befehle.

1. Öffnen Sie Ihren Webbrowser, und navigieren Sie zum [Speech Studio](https://speech.microsoft.com/).
1. Geben Sie Ihre Anmeldeinformationen ein, um sich beim Portal anzumelden.

   - Die Standardansicht ist Ihre Liste der Speech-Abonnements.
     > [!NOTE]
     > Wenn die Seite „Abonnement auswählen“ nicht angezeigt wird, können Sie dorthin navigieren, indem Sie „Speech-Ressourcen“ aus dem Einstellungsmenü in der oberen Leiste auswählen.

1. Wählen Sie Ihr Speech-Abonnement und dann **Zu Studio wechseln** aus.
1. Wählen Sie **Benutzerdefinierte Befehle (Vorschau)** aus.

Die Standardansicht ist eine Liste der von Ihnen erstellten Anwendungen für benutzerdefinierte Befehle.

## <a name="create-a-custom-commands-project"></a>Erstellen eines Projekts für benutzerdefinierte Befehle

1. Wählen Sie **Neues Projekt** aus, um ein neues Projekt zu erstellen.

   > [!div class="mx-imgBorder"]
   > ![Erstellen eines neuen Projekts](media/custom-speech-commands/create-new-project.png)

1. Geben Sie den Projektnamen und die Sprache ein, und wählen Sie dann **Weiter** aus, um den Vorgang fortzusetzen.
1. Geben Sie Ihren LUIS-Erstellungsschlüssel ein.
1. Wählen Sie nach der Erstellung Ihr Projekt aus.

Ihre Ansicht sollte jetzt eine Übersicht über Ihre Anwendung für benutzerdefinierte Befehle darstellen.

## <a name="create-a-new-command"></a>Erstellen eines neuen Befehls

Jetzt können Sie einen Befehl erstellen. Lassen Sie uns ein Beispiel verwenden, das eine einzelne Äußerung, `turn on the tv`, übernimmt und mit der Nachricht `Ok, turning on the TV` antwortet.

1. Erstellen Sie einen neuen Befehl, indem Sie das Symbol `+` neben den Befehlen auswählen und ihm den Namen `TurnOn` geben.
1. Wählen Sie **Speichern** aus.

> [!div class="mx-imgBorder"]
> ![Erstellen eines Befehls](media/custom-speech-commands/create-add-command.png)

Ein Befehl ist ein Satz von:

| Group            | BESCHREIBUNG                                                                                                                 |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------- |
| Beispielsätze | Beispieläußerungen, die der Benutzer äußern kann, um diesen Befehl auszulösen.                                                                 |
| Parameter       | Informationen, die zur Vervollständigung des Befehls erforderlich sind.                                                                                |
| Vervollständigungsregeln | Die Aktionen, die zur Erfüllung des Befehls durchzuführen sind. Beispielsweise, um dem Benutzer zu antworten oder mit einem anderen Webdienst zu kommunizieren. |
| Erweiterte Regeln   | Zusätzliche Regeln für den Umgang mit spezifischeren oder komplexeren Situationen.                                                              |

### <a name="add-a-sample-sentence"></a>Hinzufügen eines Beispielsatzes

Lassen Sie uns mit Beispielsätzen beginnen und ein Beispiel dafür bereitstellen, was der Benutzer sagen kann:

```
turn on the tv
```

Im Moment verfügen wir über keine Parameter, sodass wir zu den Vervollständigungsregeln übergehen können.

### <a name="add-a-completion-rule"></a>Hinzufügen einer Vervollständigungsregel

Fügen Sie jetzt eine Vervollständigungsregel hinzu, um dem Benutzer zu antworten, die anzeigt, dass eine Aktion ausgeführt wird.

> [!div class="mx-imgBorder"]
> ![Erstellen einer Vervollständigungsregel](media/custom-speech-commands/create-basic-completion-response-rule.png)

| Einstellung    | Empfohlener Wert                        | BESCHREIBUNG                                        |
| ---------- | -------------------------------------- | -------------------------------------------------- |
| Regelname  | „ConfirmationResponse“                 | Ein Name, der den Zweck der Regel beschreibt.          |
| Bedingungen | Keine                                   | Bedingungen, die bestimmen, wann die Regel ausgeführt werden kann.    |
| Aktionen    | SpeechResponse „OK, schalte Fernseher ein“ | Die durchzuführende Aktion, wenn die Bedingung der Regel erfüllt ist (true). |

## <a name="try-it-out"></a>Ausprobieren

Testen Sie das Verhalten über den Testchatbereich.

> [!div class="mx-imgBorder"]
> ![Testen mit Webchat](media/custom-speech-commands/create-basic-test-chat.png)

- Sie geben Folgendes ein: „Schalte Fernseher ein“
- Erwartete Antwort: „OK, schalte Fernseher ein“

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Schnellstart: Erstellen eines benutzerdefinierten Befehls mit Parametern (Vorschau)](./quickstart-custom-speech-commands-create-parameters.md)
