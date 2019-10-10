---
title: Konversationen mit mehreren Turns – QnA Maker
titleSuffix: Azure Cognitive Services
description: Verwalten Sie für Ihren Bot von einer Frage zur nächsten die verschiedenen Durchläufe (Multi-Turn) mit Eingabeaufforderungen und Kontext. Mehrfachdurchläufe sind die Fähigkeit, eine wechselseitige Konversation zu führen, wobei der Kontext der vorherigen Frage die nächste Frage und die nächste Antwort beeinflusst.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 09/25/2019
ms.author: diberry
ms.openlocfilehash: dc99626e2341e180ba0ab191003cf3a6ba9b72e9
ms.sourcegitcommit: 8bae7afb0011a98e82cbd76c50bc9f08be9ebe06
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2019
ms.locfileid: "71695147"
---
# <a name="use-follow-up-prompts-to-create-multiple-turns-of-a-conversation"></a>Erstellen von Mehrfachdurchläufen einer Konversation mit Folgeaufforderungen

Verwalten Sie für Ihren Bot von einer Frage zur nächsten die Mehrfachdurchläufe (_Multi-Turn_) mit Folgeaufforderungen und Kontext.

Wenn Sie erfahren möchten, wie ein Mehrfachdurchlauf funktioniert, sehen Sie sich das folgende Demovideo an:

[![Konversationen mit Mehrfachdurchläufen in QnA Maker](../media/conversational-context/youtube-video.png)](https://aka.ms/multiturnexample)

## <a name="what-is-a-multi-turn-conversation"></a>Was ist eine Konversation mit Mehrfachdurchläufen?

Einige Fragen können nicht in einem Zug beantwortet werden. Wenn Sie Ihre Clientanwendung (Chatbot) für Konversationen entwerfen, könnte ein Benutzer eine Frage stellen, die gefiltert oder verfeinert werden muss, um die richtige Antwort zu bestimmen. Sie ermöglichen diesen Ablauf durch die Fragen, indem Sie dem Benutzer *Folgeaufforderungen* anzeigen.

Wenn ein Benutzer eine Frage stellt, gibt QnA Maker die Antwort _und_ alle Folgeaufforderungen zurück. Diese Antwort ermöglicht es Ihnen, Folgefragen als Auswahlmöglichkeiten zu präsentieren. 

## <a name="example-multi-turn-conversation-with-chat-bot"></a>Beispiel für Konversation mit Mehrfachdurchläufen mit Chatbot

Mithilfe von Mehrfachdurchlauf verwaltet ein Chatbot eine Konversation mit einem Benutzer, um die endgültige Antwort zu bestimmen, wie in der folgenden Abbildung gezeigt wird:

![Ein Mehrfachdurchlauf-Dialog mit Eingabeaufforderungen, die einen Benutzer durch eine Konversation leiten](../media/conversational-context/conversation-in-bot.png)

In der vorstehenden Abbildung hat ein Benutzer durch Eingabe von **My account** (Mein Konto) eine Konversation begonnen. Die Wissensdatenbank enthält drei verknüpfte Frage-und-Antwortpaare. Um die Antwort zu verfeinern, wählt der Benutzer eine der drei Auswahlmöglichkeiten in der Wissensdatenbank aus. Zur Frage (#1) gibt es drei Folgeaufforderungen, die im Chatbot als drei Optionen (#2) angezeigt werden. 

Wenn der Benutzer eine Option (#3) auswählt, wird die nächste Liste von Verfeinerungsoptionen (#4) angezeigt. Diese Sequenz wird so lange fortgesetzt (#5), bis der Benutzer die richtige, endgültige Antwort (#6) bestimmt.

> [!NOTE]
> In der vorstehenden Abbildung wurde das Kontrollkästchen **Enable multi-turn** (Mehrfachdurchlauf aktivieren) ausgewählt, um sicherzustellen, dass die Eingabeaufforderungen angezeigt werden. 

### <a name="use-multi-turn-in-a-bot"></a>Verwenden von Mehrfachdurchläufen in einem Bot

Ändern Sie zum Verwalten der kontextbezogenen Konversation Ihre Clientanwendung, indem Sie [Ihrem Bot Code hinzufügen](https://github.com/microsoft/BotBuilder-Samples/tree/master/experimental/qnamaker-prompting). Durch das Hinzufügen des Codes wird es ermöglicht, dass Benutzern die Eingabeaufforderungen angezeigt werden.  

## <a name="create-a-multi-turn-conversation-from-a-documents-structure"></a>Erstellen einer Mehrfachdurchlauf-Konversation anhand der Struktur eines Dokuments

Wenn Sie eine Wissensdatenbank erstellen, zeigt der Abschnitt **Populate your KB** (Auffüllen Ihrer Wissensdatenbank) ein Kontrollkästchen **Enable multi-turn extraction from URLs, .pdf or .docx files** (Mehrfachdurchlauf-Extrahierung aus URLs, PDF- oder DOCX-Dateien aktivieren). 

![Kontrollkästchen zum Aktivieren der Mehrfachdurchlauf-Extrahierung](../media/conversational-context/enable-multi-turn.png)

Wenn Sie diese Option auswählen, kann die Mehrfachdurchlauf-Konversation aus der Dokumentstruktur abgeleitet werden. Wenn diese Struktur vorhanden ist, erstellt QnA Maker im Rahmen des Importvorgangs die Folgeaufforderung, die Fragen und Antworten für Sie als Paare zusammenfügt. 

Die Mehrfachdurchlauf-Struktur kann nur aus URLs, PDF-Dateien oder DOCX-Dateien abgeleitet werden. Ein Beispiel für eine Struktur zeigt eine Abbildung aus einer [PDF-Datei eines Microsoft Surface-Benutzerhandbuchs](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/product-manual.pdf). Aufgrund der Größe dieser PDF-Datei erfordert die QnA Maker-Ressource als **Tarif für die Suche** mindestens **B** (15 Indizes). 

![![Beispiel für eine Struktur in einem Benutzerhandbuch](../media/conversational-context/import-file-with-conversational-structure.png)](../media/conversational-context/import-file-with-conversational-structure.png#lightbox)

### <a name="determine-multi-turn-structure-from-format"></a>Ermitteln der Struktur von Mehrfachdurchläufen aus dem Format

QnA Maker ermittelt die Struktur von Mehrfachdurchläufen wie folgt:

* Schriftgrad der Überschrift – wenn Sie Stil, Farbe oder einen anderen Mechanismus verwenden, um eine Struktur in Ihrem Dokument anzudeuten, extrahiert QnA Maker die Aufforderungen für Mehrfachdurchläufe nicht. 

Zu den Regeln für Überschriften gehören:

* Beenden Sie eine Überschrift nicht mit einem Fragezeichen, `?`. 

### <a name="add-file-with-multi-turn-prompts"></a>Hinzufügen einer Datei mit Aufforderungen für Mehrfachdurchläufe

Beim Hinzufügen eines Dokuments mit Mehrfachdurchläufen bestimmt QnA Maker Folgeaufforderungen anhand der Struktur, um den Konversationsablauf zu erstellen. 

1. Wählen Sie in QnA Maker eine vorhandene Wissensdatenbank aus, die mit aktivierter Option **Enable multi-turn extraction from URLs, .pdf or .docx files.** (Aktivieren der Mehrfachdurchlauf-Extrahierung von URLs, PDF- oder DOCX-Dateien) erstellt wurde. 
1. Wechseln Sie zur Seite **Einstellungen**, und wählen Sie die hinzuzufügende Datei oder URL aus. 
1. **Speichern und trainieren** Sie die Wissensdatenbank.

> [!Caution]
> Die Unterstützung für die Verwendung einer exportierten TSV- oder XLS-Wissensdatenbankdatei mit Mehrfachdurchläufen als Datenquelle für eine neue oder leere Wissensdatenbank wird nicht unterstützt. Sie müssen den Dateityp auf der Seite **Einstellungen** des QnA Maker-Portals **importieren**, um exportierte Aufforderungen für Mehrfachdurchläufe zu einer Wissensdatenbank hinzuzufügen.


## <a name="create-knowledge-base-with-multi-turn-prompts-with-the-create-api"></a>Erstellen einer Wissensdatenbank mit Eingabeaufforderungen mit Mehrfachdurchläufen mithilfe der Create-API

Sie können eine Wissensdatenbank mit Eingabeaufforderungen mit Mehrfachdurchläufen mithilfe der [Create-API von QnA Maker](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/create) erstellen. Die Aufforderungen werden im Array `prompts` der `context`-Eigenschaft hinzugefügt. 

## <a name="show-questions-and-answers-with-context"></a>Anzeigen von Fragen und Antworten mit Kontext

Reduzieren Sie die Anzahl der angezeigten Frage- und Antwortpaare auf nur diejenigen mit kontextbezogenen Konversationen. 

Wählen Sie **Optionen anzeigen** und dann **Show context (PREVIEW)** (Kontext anzeigen (VORSCHAU)) aus. Die Liste zeigt Frage- und Antwortpaare mit Folgeaufforderungen an. 

![Filtern von Frage- und Antwortpaaren nach kontextbezogenen Konversationen](../media/conversational-context/filter-question-and-answers-by-context.png)

Der Mehrfachdurchlauf-Kontext wird in der ersten Spalte angezeigt.

![![Die Spalte "Context (PREVIEW)"](../media/conversational-context/surface-manual-pdf-follow-up-prompt.png)](../media/conversational-context/surface-manual-pdf-follow-up-prompt.png#lightbox)

In der vorstehenden Abbildung zeigt **#1** fett formatierten Text in der Spalte und damit die aktuelle Frage an. Die übergeordnete Frage wird als oberstes Element in der Zeile angezeigt. Alle Fragen darunter sind die verknüpften Frage- und Antwortpaare. Diese Elemente können ausgewählt werden, damit Sie sofort zu den anderen Kontextelementen gelangen. 

## <a name="add-an-existing-question-and-answer-pair-as-a-follow-up-prompt"></a>Hinzufügen eines vorhandenen Frage- und Antwortpaars als Folgeaufforderung

Bei der ursprünglichen Frage, **My account** (Mein Konto), gibt es Folgeaufforderungen wie **Accounts and signing in** (Konten und Anmelden). 

![Die Antworten zu „Accounts and signing in“und Folgeaufforderungen](../media/conversational-context/detected-and-linked-follow-up-prompts.png)

Fügen Sie eine Folgeaufforderung zu einem vorhandenen Frage- und Antwortpaar hinzu, das derzeit noch nicht verknüpft ist. Weil die Frage nicht mit einem Frage- und Antwortpaar verknüpft ist, muss die aktuelle Einstellung für die Ansicht geändert werden.

1. Um ein vorhandenes Frage- und Antwortpaar als Folgeaufforderung zu verknüpfen, wählen Sie die Zeile für das betreffende Frage- und Antwortpaar aus. Für das Surface-Handbuch suchen Sie nach **Sign out** (Abmelden), um die Liste zu reduzieren.
1. Wählen Sie in der Zeile für **Sign out** in der Spalte **Answer** (Antwort) die Option **Add follow-up prompt** (Folgeaufforderung hinzufügen) aus.
1. Geben Sie in den Feldern des Popupfensters **Follow-up prompt (PREVIEW)** (Folgeaufforderung (VORSCHAU)) die folgenden Werte ein:

    |Feld|Wert|
    |--|--|
    |Anzeigetext|Geben Sie **Gerät ausschalten** ein. Dies ist benutzerdefinierter Text, der in der Folgeaufforderung angezeigt werden soll.|
    |Context-only (Nur Kontext)| Aktivieren Sie dieses Kontrollkästchen. Eine Antwort wird nur zurückgegeben, wenn die Frage einen Kontext angibt.|
    |Link to answer (Link zur Antwort)|Geben Sie **Anmeldebildschirm verwenden** ein, um das vorhandene Frage- und Antwortpaar zu finden.|


1.  Es wird eine Übereinstimmung zurückgegeben. Wählen Sie diese Antwort als Folgeaufforderung und dann **Save** (Speichern) aus. 

    ![Die Seite „Follow-up prompt (PREVIEW)“
(Folgeaufforderung (VORSCHAU))](../media/conversational-context/search-follow-up-prompt-for-existing-answer.png)

1. Nachdem Sie die Folgeaufforderung hinzugefügt haben, wählen Sie in der oberen Navigation **Save and train** (Speichern und trainieren) aus.
  
### <a name="edit-the-display-text"></a>Bearbeiten des Anzeigetexts 

Wenn eine Folgeaufforderung erstellt und ein vorhandenes Frage- und Antwortpaar als **Link to answer** (Link zur Antwort) eingegeben wird, können Sie neuen **Display text** (Anzeigetext) eingeben. Dieser Text ersetzt die vorhandene Frage nicht, und es wird keine neue alternative Frage hinzugefügt. Es ist von diesen Werten getrennt. 

1. Zum Bearbeiten des Anzeigetexts suchen Sie im Feld **Context** (Kontext) nach der Frage, und wählen Sie sie aus.
1. Wählen Sie in der Zeile für diese Frage die Folgeaufforderung in der Antwortspalte aus. 
1. Wählen Sie den Anzeigetext, den Sie bearbeiten möchten, und dann **Edit** (Bearbeiten) aus.

    ![Der Befehl „Edit“ für den Anzeigetext](../media/conversational-context/edit-existing-display-text.png)

1. Ändern Sie im Popupfenster **Follow-up prompt** (Folgeaufforderung) den vorhandenen Anzeigetext. 
1. Wenn Sie die Bearbeitung Ihres Anzeigetexts abgeschlossen haben, wählen Sie **Save** (Speichern) aus. 
1. Wählen Sie in der oberen Navigationsleiste **Save and train** (Speichern und trainieren) aus.


## <a name="add-a-new-question-and-answer-pair-as-a-follow-up-prompt"></a>Hinzufügen eines neuen Frage- und Antwortpaars als Folgeaufforderung

Wenn Sie der Wissensdatenbank ein neues Frage- und Antwortpaar hinzufügen, sollte jedes Paar mit einer vorhandenen Frage als Folgeaufforderung verknüpft werden.

1. Suchen Sie in der Symbolleiste der Wissensdatenbank nach dem vorhandenen Frage- und Antwortpaar für **Accounts and signing in** (Konten und Anmelden), und wählen Sie es aus. 

1. Wählen Sie in der Spalte **Antwort** für diese Frage **Add follow-up prompt** (Folgeaufforderung hinzufügen) aus. 
1. Erstellen Sie unter **Follow-up prompt (PREVIEW)** (Folgeaufforderung (VORSCHAU)) eine neue Folgeaufforderung, indem Sie folgende Werte eingeben: 

    |Feld|Wert|
    |--|--|
    |Anzeigetext|*Erstellen Sie ein Windows-Konto*. Dies ist der benutzerdefinierte Text, der in der Folgeaufforderung angezeigt werden soll.|
    |Context-only (Nur Kontext)|Aktivieren Sie dieses Kontrollkästchen. Diese Antwort wird nur zurückgegeben, wenn die Frage einen Kontext angibt.|
    |Link to answer (Link zur Antwort)|Geben Sie den folgenden Text als Antwort ein:<br>*[Erstellen Sie](https://account.microsoft.com/) ein Windows-Konto mit einem neuen oder vorhandenen E-Mail-Konto*.<br>Wenn Sie die Datenbank speichern und trainieren, wird dieser Text konvertiert. |
    |||

    ![Erstellen einer neuen Eingabeaufforderung für Frage und Antwort](../media/conversational-context/create-child-prompt-from-parent.png)


1. Wählen Sie **Create new** (Neu erstellen) und dann **Save** (Speichern) aus. 

    Durch diese Aktion wird ein neues Frage- und Antwortpaar erstellt, und die ausgewählte Frage wird als Folgeaufforderung verknüpft. In der Spalte **Context** (Kontext) wird für beide Fragen eine Folgeaufforderungsbeziehung angegeben. 

1. Wählen Sie **View options** (Optionen anzeigen) und dann [**Show context (PREVIEW)** ](#show-questions-and-answers-with-context) (Kontext anzeigen (VORSCHAU)) aus.

    Die neue Frage zeigt, wie sie verknüpft ist.

    ![Erstellen einer neuen Folgeaufforderung](../media/conversational-context/new-qna-follow-up-prompt.png)

    Die übergeordnete Frage zeigt eine neue Frage als eine der Auswahlmöglichkeiten an.

    ![![In der Spalte „Context“ wird für beide Fragen eine Folgeaufforderungsbeziehung angegeben](../media/conversational-context/child-prompt-created.png)](../media/conversational-context/child-prompt-created.png#lightbox)

1. Nachdem Sie die Folgeaufforderung hinzugefügt haben, wählen Sie in der oberen Navigationsleiste **Save and train** (Speichern und trainieren) aus.

## <a name="enable-multi-turn-during-testing-of-follow-up-prompts"></a>Aktivieren von Mehrfachdurchläufen während des Testens von Folgeaufforderungen

Wenn Sie die Frage mit Folgeaufforderungen im Bereich **Test** testen, wählen Sie **Enable multi-turn** (Mehrfachdurchlauf aktivieren) aus, und geben Sie Ihre Frage ein. Die Antwort enthält die Folgeaufforderungen.

![Die Antwort enthält die Folgeaufforderungen.](../media/conversational-context/test-pane-with-question-having-follow-up-prompts.png)

Wenn Sie Mehrfachdurchläufe nicht aktivieren, wird zwar die Antwort zurückgegeben, aber keine Folgeaufforderungen.

## <a name="a-json-request-to-return-an-initial-answer-and-follow-up-prompts"></a>Eine JSON-Anforderung zur Rückgabe einer ersten Antwort und der Folgeaufforderungen

Fordern Sie mit dem leeren `context`-Objekt die Antwort auf die Frage des Benutzers an, und schließen Sie Folgeaufforderungen ein. 

```JSON
{
  "question": "accounts and signing in",
  "top": 10,
  "userId": "Default",
  "isTest": false,
  "context": {}
}
```

## <a name="a-json-response-to-return-an-initial-answer-and-follow-up-prompts"></a>Eine JSON-Antwort zur Rückgabe einer ersten Antwort und der Folgeaufforderungen

Im vorstehenden Abschnitt wurde eine Antwort mit allen Folgeaufforderungen für **Accounts and signing in** (Konten und Anmelden) angefordert. Die Antwort enthält die Aufforderungsinformationen, die in *answers[0].context* stehen, und den Text, der dem Benutzer angezeigt werden soll. 

```JSON
{
    "answers": [
        {
            "questions": [
                "Accounts and signing in"
            ],
            "answer": "**Accounts and signing in**\n\nWhen you set up your Surface, an account is set up for you. You can create additional accounts later for family and friends, so each person using your Surface can set it up just the way he or she likes. For more info, see All about accounts on Surface.com. \n\nThere are several ways to sign in to your Surface Pro 4: ",
            "score": 100.0,
            "id": 15,
            "source": "product-manual.pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": [
                    {
                        "displayOrder": 0,
                        "qnaId": 16,
                        "qna": null,
                        "displayText": "Use the sign-in screen"
                    }
                ]
            }
        },
        {
            "questions": [
                "Sign out"
            ],
            "answer": "**Sign out**\n\nHere's how to sign out: \n\n Go to Start, and right-click your name. Then select Sign out. ",
            "score": 38.01,
            "id": 18,
            "source": "product-manual.pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": [
                    {
                        "displayOrder": 0,
                        "qnaId": 16,
                        "qna": null,
                        "displayText": "Turn off the device"
                    }
                ]
            }
        },
        {
            "questions": [
                "Use the sign-in screen"
            ],
            "answer": "**Use the sign-in screen**\n\n1.  \n\nTurn on or wake your Surface by pressing the power button. \n\n2.  \n\nSwipe up on the screen or tap a key on the keyboard. \n\n3.  \n\nIf you see your account name and account picture, enter your password and select the right arrow or press Enter on your keyboard. \n\n4.  \n\nIf you see a different account name, select your own account from the list at the left. Then enter your password and select the right arrow or press Enter on your keyboard. ",
            "score": 27.53,
            "id": 16,
            "source": "product-manual.pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": []
            }
        }
    ]
}
```

Das Array `prompts` stellt Text in der `displayText`-Eigenschaft und den Wert `qnaId` bereit. Sie können diese Antworten als die nächsten angezeigten Auswahlmöglichkeiten im Konversationsablauf anzeigen und dann die ausgewählte `qnaId` in der nachstehenden Anforderung an QnA Maker senden. 

<!--

The `promptsToDelete` array provides the ...

-->

## <a name="a-json-request-to-return-a-non-initial-answer-and-follow-up-prompts"></a>Eine JSON-Anforderung zur Rückgabe einer nachfolgenden Antwort und der Folgeaufforderungen

Füllen Sie das `context`-Objekt aus, um den vorherigen Kontext einzubeziehen.

In der folgenden JSON-Anforderung lautet die aktuelle Frage *Use Windows Hello to sign in* (Windows Hello zum Anmelden verwenden), und die vorherige Frage lautete *accounts und signing in* (Konten und Anmelden). 

```JSON
{
  "question": "Use Windows Hello to sign in",
  "top": 10,
  "userId": "Default",
  "isTest": false,
  "qnaId": 17,
  "context": {
    "previousQnAId": 15,
    "previousUserQuery": "accounts and signing in"
  }
}
``` 

##  <a name="a-json-response-to-return-a-non-initial-answer-and-follow-up-prompts"></a>Eine JSON-Antwort zur Rückgabe einer nachfolgenden Antwort und der Folgeaufforderungen

QnA Maker-JSON-Antwort _GenerateAnswer_ enthält die Folgeaufforderungen in der `context`-Eigenschaft des ersten Elements im `answers`-Objekt:

```JSON
{
    "answers": [
        {
            "questions": [
                "Use Windows Hello to sign in"
            ],
            "answer": "**Use Windows Hello to sign in**\n\nSince Surface Pro 4 has an infrared (IR) camera, you can set up Windows Hello to sign in just by looking at the screen. \n\nIf you have the Surface Pro 4 Type Cover with Fingerprint ID (sold separately), you can set up your Surface sign you in with a touch. \n\nFor more info, see What is Windows Hello? on Windows.com. ",
            "score": 100.0,
            "id": 17,
            "source": "product-manual.pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": []
            }
        },
        {
            "questions": [
                "Meet Surface Pro 4"
            ],
            "answer": "**Meet Surface Pro 4**\n\nGet acquainted with the features built in to your Surface Pro 4. \n\nHere’s a quick overview of Surface Pro 4 features: \n\n\n\n\n\n\n\nPower button \n\n\n\n\n\nPress the power button to turn your Surface Pro 4 on. You can also use the power button to put it to sleep and wake it when you’re ready to start working again. \n\n\n\n\n\n\n\nTouchscreen \n\n\n\n\n\nUse the 12.3” display, with its 3:2 aspect ratio and 2736 x 1824 resolution, to watch HD movies, browse the web, and use your favorite apps. \n\nThe new Surface G5 touch processor provides up to twice the touch accuracy of Surface Pro 3 and lets you use your fingers to select items, zoom in, and move things around. For more info, see Surface touchscreen on Surface.com. \n\n\n\n\n\n\n\nSurface Pen \n\n\n\n\n\nEnjoy a natural writing experience with a pen that feels like an actual pen. Use Surface Pen to launch Cortana in Windows or open OneNote and quickly jot down notes or take screenshots. \n\nSee Using Surface Pen (Surface Pro 4 version) on Surface.com for more info. \n\n\n\n\n\n\n\nKickstand \n\n\n\n\n\nFlip out the kickstand and work or play comfortably at your desk, on the couch, or while giving a hands-free presentation. \n\n\n\n\n\n\n\nWi-Fi and Bluetooth® \n\n\n\n\n\nSurface Pro 4 supports standard Wi-Fi protocols (802.11a/b/g/n/ac) and Bluetooth 4.0. Connect to a wireless network and use Bluetooth devices like mice, printers, and headsets. \n\nFor more info, see Add a Bluetooth device and Connect Surface to a wireless network on Surface.com. \n\n\n\n\n\n\n\nCameras \n\n\n\n\n\nSurface Pro 4 has two cameras for taking photos and recording video: an 8-megapixel rear-facing camera with autofocus and a 5-megapixel, high-resolution, front-facing camera. Both cameras record video in 1080p, with a 16:9 aspect ratio. Privacy lights are located on the right side of both cameras. \n\nSurface Pro 4 also has an infrared (IR) face-detection camera so you can sign in to Windows without typing a password. For more info, see Windows Hello on Surface.com. \n\nFor more camera info, see Take photos and videos with Surface and Using autofocus on Surface 3, Surface Pro 4, and Surface Book on Surface.com. \n\n\n\n\n\n\n\nMicrophones \n\n\n\n\n\nSurface Pro 4 has both a front and a back microphone. Use the front microphone for calls and recordings. Its noise-canceling feature is optimized for use with Skype and Cortana. \n\n\n\n\n\n\n\nStereo speakers \n\n\n\n\n\nStereo front speakers provide an immersive music and movie playback experience. To learn more, see Surface sound, volume, and audio accessories on Surface.com. \n\n\n\n\n",
            "score": 21.92,
            "id": 3,
            "source": "product-manual.pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": [
                    {
                        "displayOrder": 0,
                        "qnaId": 4,
                        "qna": null,
                        "displayText": "Ports and connectors"
                    }
                ]
            }
        },
        {
            "questions": [
                "Use the sign-in screen"
            ],
            "answer": "**Use the sign-in screen**\n\n1.  \n\nTurn on or wake your Surface by pressing the power button. \n\n2.  \n\nSwipe up on the screen or tap a key on the keyboard. \n\n3.  \n\nIf you see your account name and account picture, enter your password and select the right arrow or press Enter on your keyboard. \n\n4.  \n\nIf you see a different account name, select your own account from the list at the left. Then enter your password and select the right arrow or press Enter on your keyboard. ",
            "score": 19.04,
            "id": 16,
            "source": "product-manual.pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": []
            }
        }
    ]
}
```

## <a name="query-the-knowledge-base-with-the-qna-maker-id"></a>Abfragen der Wissensdatenbank mit der QnA Maker-ID

In der Antwort auf die erste Frage werden alle Folgeaufforderungen und zugehörigen `qnaId` zurückgegeben. Da Sie nun über die ID verfügen, können Sie diese im Anforderungstext der Folgeaufforderung übergeben. Wenn der Anforderungstext die `qnaId` und das Kontextobjekt (mit den vorherigen QnA Maker-Eigenschaften) enthält, gibt „GenerateAnswer“ die genaue Frage anhand der ID zurück, statt den Rangfolgenalgorithmus zu verwenden, um die Antwort anhand des Fragentexts zu suchen. 

## <a name="display-prompts-and-send-context-in-the-client-application"></a>Anzeigen von Eingabeaufforderungen und Senden von Kontext in der Clientanwendung 

Sie haben Eingabeaufforderungen zu Ihrer Wissensdatenbank hinzugefügt und den Datenfluss im Testbereich getestet. Jetzt müssen Sie diese Eingabeaufforderungen in der Clientanwendung verwenden. Bei Bot Framework werden die Eingabeaufforderungen in den Clientanwendungen nicht automatisch angezeigt. Sie können die Aufforderungen als vorgeschlagene Aktionen oder Schaltflächen als Teil der Antwort auf die Abfrage des Benutzers in Clientanwendungen anzeigen, indem Sie dieses [Bot Framework-Beispiel](https://aka.ms/qnamakermultiturnsample) in Ihren Code einbeziehen. Die Clientanwendung sollte die aktuelle QnA Maker-ID sowie die Benutzerabfrage speichern und für die nächste Benutzerabfrage an das [Kontextobjekt der GenerateAnswer-API](#a-json-request-to-return-a-non-initial-answer-and-follow-up-prompts) übergeben. 

## <a name="display-order-is-supported-in-the-update-api"></a>Die Anzeigereihenfolge wird in der Update-API unterstützt

Der in der JSON-Antwort zurückgegebene [Anzeigetext und die Anzeigereihenfolge](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/update#promptdto) können von der [Update-API](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/update) bearbeitet werden. 

## <a name="add-or-delete-multi-turn-prompts-with-the-update-api"></a>Hinzufügen oder Löschen von Eingabeaufforderungen mit Mehrfachdurchläufen mithilfe der Update-API

Sie können Eingabeaufforderungen mit Mehrfachdurchläufen mithilfe der [Update-API von QnA Maker](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/update) hinzufügen oder löschen.  Die Aufforderungen werden im Array `promptsToAdd` der `context`-Eigenschaft und im Array `promptsToDelete` hinzugefügt. 

## <a name="export-knowledge-base-for-version-control"></a>Exportieren der Wissensdatenbank für die Versionskontrolle

QnA Maker [unterstützt die Versionskontrolle](../concepts/development-lifecycle-knowledge-base.md#version-control-of-a-knowledge-base) im QnA Maker-Portal, indem es Schritte für Konversationen mit Mehrfachdurchläufen in die exportierte Datei einfügt.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über kontextbezogene Konversationen in diesem [Dialogbeispiel](https://aka.ms/qnamakermultiturnsample), oder informieren Sie sich über [konzeptionelles Bot-Design für Konversationen mit Mehrfachdurchläufen](https://docs.microsoft.com/azure/bot-service/bot-builder-conversations?view=azure-bot-service-4.0).

> [!div class="nextstepaction"]
> [Migrieren einer Wissensdatenbank](../Tutorials/migrate-knowledge-base.md)
