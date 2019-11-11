---
title: Was ist Unterhaltungstranskription (Vorschau)? – Spracherkennungsdienst
titleSuffix: Azure Cognitive Services
description: Die Unterhaltungstranskription ist eine Spracherkennungslösung, die Spracherkennung, Sprecheridentifikation und Satzzuordnung zu jedem Sprecher (auch als Diarisierung bezeichnet) kombiniert, um eine Echtzeit- und/oder asynchrone Transkription einer beliebigen Unterhaltung zu ermöglichen. Die Unterhaltungstranskription schließt alle Beteiligten in Unterhaltungen ein, z. B. taube und schwerhörige Teilnehmer.
services: cognitive-services
author: markamos
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: erhopf
ms.openlocfilehash: f4b7591d5ddb138db0abbdb673cadc6afd71f315
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73608010"
---
# <a name="what-is-conversation-transcription-preview"></a>Was ist Unterhaltungstranskription (Vorschau)?

Die Unterhaltungstranskription ist eine [Spracherkennungslösung](speech-to-text.md), die Spracherkennung, Sprecheridentifikation und Satzzuordnung zu jedem Sprecher (auch als _Diarisierung_ bezeichnet) kombiniert, um eine Echtzeit- und/oder asynchrone Transkription einer beliebigen Unterhaltung zu ermöglichen. Die Unterhaltungstranskription unterscheidet Sprecher in einer Unterhaltung, um festzustellen, wer was wann gesagt hat, und vereinfacht es für Entwickler, ihren Anwendungen, die eine Diarisierung mit mehreren Sprechern durchführen, eine Spracherkennung hinzuzufügen.

## <a name="key-features"></a>Wichtige Features

- **Zeitstempel**: Jede Äußerung eines Sprechers weist einen Zeitstempel auf, sodass Sie leicht erkennen können, wann ein Ausdruck geäußert wurde.
- **Lesbare Transkriptionen**: Transkripte werden automatisch mit Formatierung und Interpunktion versehen, um sicherzustellen, dass der Text genau dem entspricht, was gesagt wurde.
- **Benutzerprofile**: Benutzerprofile werden generiert, indem Sprachbeispiele von Benutzern erfasst und an die Signaturgenerierung gesendet werden.
- **Sprecheridentifikation**: Sprecher werden über Benutzerprofile identifiziert und jedem Sprecher ist ein _Sprecherbezeichner_ zugeordnet.
- **Diarisierung mehrerer Sprecher**: Ermittlung, wer was gesagt hat, durch Synthetisieren des Audiostreams mit den einzelnen Sprecherbezeichnern.
- **Echtzeittranskription**: Bereitstellung von Live-Transkripten darüber, wer was und wann sagt, während die Unterhaltung stattfindet.
- **Asynchrone Transkription**: Bereitstellung von Transkripten mit höherer Genauigkeit durch Verwendung eines Mehrkanalaudiostreams.

> [!NOTE]
> Obwohl die Unterhaltungstranskription die Anzahl der Sprecher im Raum nicht begrenzt, ist sie für 2 bis 10 Sprecher pro Sitzung optimiert.

## <a name="use-cases"></a>Anwendungsfälle

### <a name="inclusive-meetings"></a>Inklusive Besprechungen

Um Besprechungen für alle Beteiligten, z. B. taube und schwerhörige Teilnehmer, zu ermöglichen, ist es wichtig, dass die Transkription in Echtzeit erfolgt. Die Unterhaltungstranskription im Echtzeitmodus nimmt den Audiostream von Besprechungen auf und bestimmt, wer was sagt, sodass alle Besprechungsteilnehmer der Transkription folgen und ohne Verzögerung an der Besprechung teilnehmen können.

### <a name="improved-efficiency"></a>Verbesserte Effizienz

Die Besprechungsteilnehmer können sich auf die Besprechung konzentrieren und das Aufzeichnen der Notizen der Unterhaltungstranskription überlassen. Die Teilnehmer können sich aktiv an der Besprechung beteiligen und schnell die nächsten Schritte verfolgen, indem sie das Transkript verwenden, anstatt Notizen zu machen und möglicherweise etwas während der Besprechung zu verpassen.

## <a name="how-it-works"></a>So funktioniert's

Dies ist eine allgemeine Übersicht darüber, wie die Unterhaltungstranskription funktioniert.

![Diagramm zur Einbindung der Unterhaltungstranskription](media/scenarios/conversation-transcription-service.png)

## <a name="expected-inputs"></a>Erwartete Eingaben

- **Mehrkanalaudiostream**: Weitere Informationen zu Spezifikationen und Design finden Sie unter den [Mikrofonempfehlungen für das Speech-Geräte-SDK von Microsoft](https://aka.ms/cts/microphone). Weitere Informationen zum Development Kit und dessen Erwerb finden Sie unter [Abrufen des Speech-Geräte-SDK von Microsoft](https://aka.ms/cts/getsdk).
- **Benutzersprachbeispiele**: Für die Unterhaltungstranskription sind vor der Unterhaltung Benutzerprofile erforderlich. Sie müssen Audioaufzeichnungen von jedem Benutzer sammeln und die Aufzeichnungen dann an den [Dienst für die Signaturgenerierung](https://aka.ms/cts/signaturegenservice) senden, um die Audioaufnahmen zu überprüfen und Benutzerprofile zu generieren.

## <a name="real-time-vs-asynchronous"></a>Echtzeitmodus im Vergleich zum asynchronen Modus

Die Unterhaltungstranskription bietet drei Transkriptionsmodi:

### <a name="real-time"></a>Echtzeit

Die Audiodaten werden live verarbeitet, um den Sprecherbezeichner und das Transkript zurückzugeben. Wählen Sie diesen Modus, wenn Ihre Transkriptionslösung den Teilnehmern der Unterhaltung eine Live-Transkriptansicht ihrer laufenden Unterhaltung bieten soll. Die Erstellung einer Anwendung, die Besprechungen für gehörlose und schwerhörige Teilnehmer einfacher zugänglich macht, ist z. B. ein idealer Anwendungsfall für die Echtzeittranskription.

### <a name="asynchronous"></a>Asynchron

Die Audiodaten werden als Batch verarbeitet, um den Sprecherbezeichner und das Transkript zurückzugeben. Wählen Sie diesen Modus aus, wenn Ihre Transkriptionslösung eine höhere Genauigkeit ohne Live-Transkriptansicht bieten soll. Wenn Sie z. B. eine Anwendung erstellen möchten, mit der die Besprechungsteilnehmer verpasste Besprechungen leicht nachvollziehen können, verwenden Sie den asynchronen Transkriptionsmodus, um Transkriptionsergebnisse mit hoher Genauigkeit zu erhalten.

### <a name="real-time-plus-asynchronous"></a>Echtzeit- und asynchroner Modus

Audiodaten werden live verarbeitet, um den Sprecherbezeichner und das Transkript zurückzugeben, und zusätzlich wird eine Anforderung erstellt, um auch ein Transkript mit hoher Genauigkeit durch asynchrone Verarbeitung zu erhalten. Wählen Sie diesen Modus aus, wenn Ihre Anwendung eine Echtzeittranskription benötigt, aber auch eine Transkription mit höherer Genauigkeit für die Verwendung nach der Unterhaltung oder Besprechung erfordert.

## <a name="language-support"></a>Sprachunterstützung

Die Unterhaltungstranskription unterstützt derzeit „en-US“ und „zh-CN“ in den folgenden Regionen: USA, Mitte ( *centralus*) und Asien, Osten ( *eastasia*). Wenn Sie die Unterstützung zusätzlicher Gebietsschemas benötigen, wenden Sie sich an das [Team für das Unterhaltungstranskriptionfeature](mailto:CTSFeatureCrew@microsoft.com).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Transkribieren von Unterhaltungen in Echtzeit](how-to-use-conversation-transcription-service.md)
