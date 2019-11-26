---
title: Automatisches Erkennen und Transkribieren mehrsprachiger Inhalte mit Video Indexer
titleSuffix: Azure Media Services
description: In diesem Thema wird das automatische Erkennen und Transkribieren mehrsprachiger Inhalte mit Video Indexer beschrieben.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 09/01/2019
ms.author: juliako
ms.openlocfilehash: f0dede42891069bb5d01ddc33f3797c20c5493d7
ms.sourcegitcommit: b1c94635078a53eb558d0eb276a5faca1020f835
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2019
ms.locfileid: "72968745"
---
# <a name="automatically-identify-and-transcribe-multi-language-content-preview"></a>Automatisches Erkennen und Transkribieren mehrsprachiger Inhalte (Vorschauversion)

Video Indexer unterstützt die automatische Spracherkennung und die Transkription in mehrsprachigen Inhalten. Dieser Prozess beinhaltet das automatische Erkennen der gesprochenen Sprache in verschiedenen Audiosegmenten, das Versenden der einzelnen Segmente der zu transkribierenden Mediendatei und das Zusammenführen der Transkription in einer einzelnen Transkription. 

## <a name="choosing-multilingual-identification-on-indexing-with-portal"></a>Auswählen der mehrsprachigen Erkennung bei der Indizierung mit dem Portal

Wenn Sie Ihr Video hochladen und indizieren, können Sie die **mehrsprachige Erkennung** auswählen. Alternativ können Sie die **mehrsprachige Erkennung** auch bei der Neuindizierung Ihres Videos auswählen. Die folgenden Schritte beschreiben, wie Sie eine Neuindizierung durchführen:

1. Navigieren Sie zur [Video Indexer](https://vi.microsoft.com/)-Website, und melden Sie sich an.
1. Navigieren Sie zur Seite **Bibliothek**, und zeigen Sie auf den Namen des Videos, das Sie erneut indizieren möchten. 
1. Klicken Sie unten rechts auf die Schaltfläche **Video erneut indizieren**. 
1. Wählen Sie im Dialogfeld **Video erneut indizieren** im Dropdownfeld **Ausgangssprache des Videos** die Option **multi-language detection** (Mehrsprachige Erkennung) aus.

    * Wenn ein Video als mehrsprachig indiziert ist, enthält die Seite „Insight“ diese Option, und ein zusätzlicher Insighttyp erscheint, mit dem der Benutzer anzeigen kann, welches Segment in welcher Sprache als „gesprochene Sprache“ transkribiert wird.
    * Die Übersetzung in alle Sprachen ist über das mehrsprachige Transkript vollständig verfügbar.
    * Alle weiteren Insights erscheinen in der erkannten Hauptsprache – das ist die Sprache, die in der Audiodatei zum größten Teil enthalten war.
    * Untertitel sind im Player ebenfalls in mehreren Sprachen verfügbar.

![Portalfunktion](./media/multi-language-identification-transcription/portal-experience.png)

## <a name="choosing-multilingual-identification-on-indexing-with-api"></a>Auswählen der mehrsprachigen Erkennung bei der Indizierung mit der API

Wählen Sie bei der Indizierung bzw. der [Neuindizierung](https://api-portal.videoindexer.ai/docs/services/operations/operations/Re-Index-Video?) eines Videos mithilfe der API die Option `multi-language detection` im Parameter `sourceLanguage` aus.

### <a name="model-output"></a>Modellausgabe

Das Modell ruft alle im Video erkannten Sprachen in einer Liste ab.

```json
"sourceLanguage": null,
"sourceLanguages": [
    "es-ES",
    "en-US"
],
```

Darüber hinaus enthält jede Instanz im Transkriptionsabschnitt die Sprache, in der sie transkribiert wurde.

```json
{
  "id": 136,
  "text": "I remember well when my youth Minister took me to hear Doctor King I was a teenager.",
  "confidence": 0.9343,
  "speakerId": 1,
  "language": "en-US",
  "instances": [
    {
       "adjustedStart": "0:21:10.42",
       "adjustedEnd": "0:21:17.48",
       "start": "0:21:10.42",
       "end": "0:21:17.48"
    }
  ]
},
```

## <a name="guidelines-and-limitations"></a>Richtlinien und Einschränkungen

* Liste der unterstützten Sprachen: Englisch, Französisch, Deutsch, Spanisch.
* Unterstützung für mehrsprachige Inhalte mit bis zu drei unterstützten Sprachen.
* Falls die Audiodaten andere Sprachen enthalten, die nicht in der obigen Liste mit den unterstützten Sprachen aufgeführt sind, kommt es zu einem unerwarteten Ergebnis.
* Die Mindestsegmentlänge für die Erkennung der einzelnen Sprachen beträgt 15 Sekunden.
* Der Offset der Spracherkennung beträgt im Durchschnitt drei Sekunden.
* Die Sprache sollte fortlaufend gleich sein. Häufige Wechsel zwischen Sprachen wirken sich unter Umständen auf die Leistung des Modells aus.
* Die Leistung des Modells wird ggf. auch beeinträchtigt, wenn der Sprecher kein Muttersprachler ist (wenn ein Sprecher beispielsweise zuerst in seiner Muttersprache spricht und dann zu einer anderen Sprache wechselt).
* Das Modell ist so konzipiert, dass eine spontane Unterhaltung mit einer angemessenen Audioakustik erkannt werden kann (keine Sprachbefehle, Gesang usw.).
* Die Projekterstellung und -bearbeitung ist für mehrsprachige Videos derzeit nicht verfügbar.
* Benutzerdefinierte Sprachmodelle sind bei Verwendung der mehrsprachigen Erkennung nicht verfügbar.
* Das Hinzufügen von Schlüsselwörtern wird nicht unterstützt.
* Beim Exportieren von Untertiteldateien erscheint keine Sprachanzeige.
* Die API zum Aktualisieren von Transkripts unterstützt keine mehrsprachigen Dateien.

## <a name="next-steps"></a>Nächste Schritte

[What is Video Indexer? (preview)](video-indexer-overview.md) (Was ist Video Indexer? (Vorschauversion))
