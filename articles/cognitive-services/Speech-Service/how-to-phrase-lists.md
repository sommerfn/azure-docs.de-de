---
title: Begriffslisten – Speech Service
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie den Speech-Diensten mithilfe des `PhraseListGrammar`-Objekts eine Begriffsliste zur Verfügung stellen, um die Ergebnisse der Spracherkennung zu verbessern.
services: cognitive-services
author: rhurey
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: rhurey
ms.openlocfilehash: 61d3e4d2de6b8707ee7433815f8002e5d5e3e3d6
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73464546"
---
# <a name="phrase-lists-for-speech-to-text"></a>Begriffslisten für Spracherkennung

Indem Sie den Speech-Diensten eine Begriffsliste zur Verfügung stellen, können Sie die Genauigkeit der Spracherkennung verbessern. Begriffslisten dienen zur Identifizierung bekannter Begriffe in Audiodaten, wie beispielsweise den Namen einer Person oder einen bestimmten Ort.

Wenn Sie beispielsweise den Befehl „Move to“ und „Ward“ als mögliches Ziel haben, das gesprochen werden kann, können Sie den Eintrag „Move to Ward“ hinzufügen. Das Hinzufügen eines Begriffs erhöht die Wahrscheinlichkeit, dass bei der Audioerkennung „Move to Ward“ anstelle von „Move toward“ erkannt wird.

Einer Begriffsliste können einzelne Wörter oder ganze Phrasen hinzugefügt werden. Bei der Erkennung wird ein Eintrag in einer Begriffsliste verwendet, wenn in der Audiodatei eine genaue Übereinstimmung enthalten ist. Wenn die Begriffsliste aufbauend auf dem vorherigen Beispiel „Move to Ward“ enthält und die erfassten Töne ähnlich wie „Move toward“ und „Move to Ward“ klingt, dann wird das Erkennungsergebnis eher als „Move to Ward slowly“ erkannt.

>[!Note]
> Derzeit unterstützen Begriffslisten nur Englisch für die Spracherkennung.

## <a name="how-to-use-phrase-lists"></a>Verwenden von Begriffslisten

Die folgenden Beispiele veranschaulichen das Erstellen einer Begriffsliste mithilfe des `PhraseListGrammar`-Objekts.

```C++
auto phraselist = PhraseListGrammar::FromRecognizer(recognizer);
phraselist->AddPhrase("Move to Ward");
phraselist->AddPhrase("Move to Bill");
phraselist->AddPhrase("Move to Ted");
```

```cs
PhraseListGrammar phraseList = PhraseListGrammar.FromRecognizer(recognizer);
phraseList.AddPhrase("Move to Ward");
phraseList.AddPhrase("Move to Bill");
phraseList.AddPhrase("Move to Ted");
```

```Python
phrase_list_grammar = speechsdk.PhraseListGrammar.from_recognizer(reco)
phrase_list_grammar.addPhrase("Move to Ward")
phrase_list_grammar.addPhrase("Move to Bill")
phrase_list_grammar.addPhrase("Move to Ted")
```

```JavaScript
var phraseListGrammar = SpeechSDK.PhraseListGrammar.fromRecognizer(reco);
phraseListGrammar.addPhrase("Move to Ward");
phraseListGrammar.addPhrase("Move to Bill");
phraseListGrammar.addPhrase("Move to Ted");
```

```Java
PhraseListGrammar phraseListGrammar = PhraseListGrammar.fromRecognizer(recognizer);
phraseListGrammar.addPhrase("Move to Ward");
phraseListGrammar.addPhrase("Move to Bill");
phraseListGrammar.addPhrase("Move to Ted");
```

>[!Note]
> Die maximale Anzahl von Begriffslisten, die der Speech-Dienst zum Abgleich verwenden kann, beträgt 1024 Begriffe.

Sie können die dem `PhraseListGrammar`-Objekt zugeordneten Begriffe auch löschen, indem Sie „clear()“ aufrufen.

```C++
phraselist->Clear();
```

```cs
phraseList.Clear();
```

```Python
phrase_list_grammar.clear()
```

```JavaScript
phraseListGrammar.clear();
```

```Java
phraseListGrammar.clear();
```

> [!NOTE]
> Änderungen an einem `PhraseListGrammar`-Objekt werden bei der nächsten Erkennung oder nach einer erneuten Verbindung zu den Speech-Diensten wirksam.

## <a name="next-steps"></a>Nächste Schritte

* [Speech SDK-Referenzdokumentation](speech-sdk.md)
