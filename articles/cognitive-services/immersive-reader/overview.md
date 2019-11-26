---
title: Was ist die API „Plastischer Reader“?
titleSuffix: Azure Cognitive Services
description: Bei der Plastischer Reader-API handelt es sich um ein Tool, das verwendet werden kann, um Personen mit Lernschwierigkeiten zu unterstützen oder neuen Lesern und Sprachanfängern zu helfen.
services: cognitive-services
author: metanMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: overview
ms.date: 06/20/2019
ms.author: metan
ms.openlocfilehash: 3e6889cd5d26b88cedd6b110510cb89777252f54
ms.sourcegitcommit: bc193bc4df4b85d3f05538b5e7274df2138a4574
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2019
ms.locfileid: "73904369"
---
# <a name="what-is-immersive-reader"></a>Worum handelt es sich beim plastischen Reader?

Der [plastische Reader](https://www.onenote.com/learningtools) ist ein inklusiv konzipiertes Tool, das bewährte Techniken implementiert, um das Leseverständnis von Leseanfängern, Sprachenlernenden und Personen mit Lernunterschieden, wie z.B. Dyslexie, zu verbessern.

Implementieren Sie den plastischen Reader in Ihrer Webanwendung mithilfe des Immersive Reader SDK.

## <a name="what-does-immersive-reader-do"></a>Was macht der plastische Reader?

Der plastische Reader wurde entwickelt, um das Lesen für jedermann zugänglicher zu machen.

* Zeigt den Inhalt in einer minimalen Leseansicht an.

  ![Plastischer Reader](./media/immersive-reader.png)

* Zeigt Bilder von häufig verwendeten Wörtern an.

  ![Bildwörterbuch](./media/picture-dictionary.png)

* Hebt Substantive, Verben, Adjektive und Adverbien hervor.

  ![Wortarten](./media/parts-of-speech.png)

* Liest den Inhalt laut vor.

  ![Lautes Vorlesen](./media/read-aloud.png)

* Übersetzt Ihre Inhalte in einer anderen Sprache.

  ![Sprachübersetzung](./media/translation.png)

* Unterteilt Wörter in einzelne Silben.

  ![Silbentrennung](./media/syllabification.png)

## <a name="how-does-immersive-reader-work"></a>Wie funktioniert der plastische Reader?

Der plastische Reader ist eine eigenständige Webanwendung, die beim Aufruf mit dem JavaScript SDK für den plastischen Reader über Ihrer bestehenden Webanwendung über ein `iframe` angezeigt wird. Wenn Sie die API zum Starten des plastischen Readers aufrufen, geben Sie die Inhalte an, die Sie im plastischen Reader anzeigen möchten. Unser SDK übernimmt die Erstellung und Gestaltung des `iframe` und die Kommunikation mit dem Back-End-Service des plastischen Readers, der die Inhalte für Wortarten, Sprachsynthese, Übersetzung und so weiter verarbeitet.

## <a name="next-steps"></a>Nächste Schritte

Erste Schritte mit dem plastischen Reader:

* Wechseln Sie zum [Schnellstart](./quickstart.md).
* Schauen Sie sich das [SDK für den plastischen Reader auf GitHub](https://github.com/microsoft/immersive-reader-sdk) an.
* Lesen Sie die [Referenz für das SDK für den plastischen Reader](./reference.md).