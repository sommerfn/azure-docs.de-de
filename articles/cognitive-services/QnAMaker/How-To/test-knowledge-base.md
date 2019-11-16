---
title: 'Testen einer Wissensdatenbank: QnA Maker'
titleSuffix: Azure Cognitive Services
description: Das Testen Ihrer QnA Maker-Wissensdatenbank ist ein wichtiger Teil eines iterativen Prozesses zur Verbesserung der Genauigkeit zurückgegebener Antworten. Sie können die Wissensdatenbank über eine verbesserte Chatschnittstelle testen, die Ihnen auch Änderungen ermöglicht.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 11/14/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: c139d3a740067e3cecaff90d3171d7b0cb3d52c7
ms.sourcegitcommit: a170b69b592e6e7e5cc816dabc0246f97897cb0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2019
ms.locfileid: "74091756"
---
# <a name="test-your-knowledge-base-in-qna-maker"></a>Testen Ihrer Wissensdatenbank in QnA Maker

Das Testen Ihrer QnA Maker-Wissensdatenbank ist ein wichtiger Teil eines iterativen Prozesses zur Verbesserung der Genauigkeit zurückgegebener Antworten. Sie können die Wissensdatenbank über eine verbesserte Chatschnittstelle testen, die Ihnen auch Änderungen ermöglicht.

## <a name="interactively-test-in-qna-maker-portal"></a>Interaktives Testen im QnA Maker-Portal

1. Greifen Sie auf Ihre Wissensdatenbank zu, indem Sie den Namen auf der Seite **Meine Wissensdatenbanken** auswählen.
1. Um auf den einblendbaren Bereich „Test“ zuzugreifen, wählen Sie im oberen Bereich Ihrer Anwendung **Test** aus.
1. Geben Sie eine Abfrage in das Textfeld ein, und drücken Sie die EINGABETASTE.
1. Die am besten passende Antwort aus der Wissensdatenbank wird als Antwort zurückgegeben.

### <a name="clear-test-panel"></a>Löschen des Bereichs „Test“

Um alle eingegebenen Testabfragen und ihre Ergebnisse aus der Testkonsole zu löschen, wählen Sie in der linken oberen Ecke des Bereichs „Test“ die Option **Neu beginnen** aus.

### <a name="close-test-panel"></a>Schließen des Bereichs „Test“

Um den Bereich „Test“ zu schließen, wählen Sie erneut die Schaltfläche **Test** aus. Während der Bereich „Test“ geöffnet ist, können Sie den Inhalt der Wissensdatenbank nicht bearbeiten.

### <a name="inspect-score"></a>Überprüfen der Bewertung

Sie überprüfen die Details des Testergebnisses im Bereich „Überprüfen“.

1.  Wählen Sie bei geöffnetem Bereich „Test“ die Option **Überprüfen** aus, um weitere Details zu dieser Antwort zu erhalten.

    ![Überprüfen von Antworten](../media/qnamaker-how-to-test-kb/inspect.png)

2.  Der Bereich „Überprüfung“ wird angezeigt. Dieser Bereich enthält die Absicht mit der höchsten Bewertung sowie alle identifizierten Entitäten. Das Ergebnis der ausgewählten Äußerung wird im Bereich angezeigt.

### <a name="correct-the-top-scoring-answer"></a>Korrigieren der Antwort mit der höchsten Bewertung

Wenn die Antwort mit der höchsten Bewertung falsch ist, wählen Sie die richtige Antwort aus der Liste aus, und wählen Sie dann **Speichern und trainieren** aus.

![Korrigieren der Antwort mit der höchsten Bewertung](../media/qnamaker-how-to-test-kb/choose-answer.png)

### <a name="add-alternate-questions"></a>Hinzufügen alternativer Fragen

Sie können alternative Formen einer Frage für eine bestimmte Antwort hinzufügen. Geben Sie die alternativen Antworten in das Textfeld ein, und drücken Sie die EINGABETASTE, um sie hinzuzufügen. Wählen Sie **Speichern und trainieren** aus, um die Aktualisierungen zu speichern.

![Hinzufügen alternativer Fragen](../media/qnamaker-how-to-test-kb/add-alternate-question.png)

### <a name="add-a-new-answer"></a>Hinzufügen einer neuen Antwort

Sie können eine neue Antwort hinzufügen, wenn eine der als passend zurückgegebenen Antworten falsch ist oder die Antwort nicht in der Wissensdatenbank enthalten ist (keine passende Übereinstimmung in der Wissensdatenbank gefunden wurde). 

Verwenden Sie das Textfeld unten in der Antwortliste, um eine neue Antwort einzugeben, und drücken Sie die Eingabetaste, um sie hinzuzufügen. 

Wählen Sie **Speichern und trainieren** aus, um diese Antwort dauerhaft zu speichern. Es ist nun ein neues Frage-Antwort-Paar zu Ihrer Wissensdatenbank hinzugefügt. 

> [!NOTE]
> Alle Änderungen an der Wissensdatenbank werden nur gespeichert, wenn Sie auf die Schaltfläche **Speichern und trainieren** klicken.

### <a name="test-the-published-knowledge-base"></a>Testen der veröffentlichten Wissensdatenbank

Sie können die veröffentlichte Version der Wissensdatenbank im Testbereich testen. Nachdem Sie die Wissensdatenbank veröffentlicht haben, aktivieren Sie das Kontrollkästchen **Veröffentlichte Wissensdatenbank**, und senden Sie eine Abfrage, um Ergebnisse von der veröffentlichten Wissensdatenbank zu erhalten.

![Testen einer veröffentlichten Wissensdatenbank](../media/qnamaker-how-to-test-kb/test-against-published-kb.png)

## <a name="batch-test-with-tool"></a>Batchtest mit Tool

Verwenden Sie das Batchtesttool für Folgendes:

* Ermitteln der besten Antwort und des Scores für eine Reihe von Fragen
* Überprüfen der erwarteten Antwort auf eine Reihe von Fragen

Batchtests werden über das Batchtesttool bereitgestellt. Dieses Tool ist zum Download als [komprimierte ausführbare Datei](https://aka.ms/qnamakerbatchtestingtool) oder als [C#-Quellcode](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp/tree/master/documentation-samples/batchtesting) verfügbar. 

Die [Referenzdokumentation zu dem Tool](../reference-tsv-format-batch-testing.md) umfasst Folgendes:

* Befehlszeilenbeispiel für das Tool
* Format für TSV-Eingabe- und -Ausgabedateien 

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Veröffentlichen einer Wissensdatenbank](./publish-knowledge-base.md)
