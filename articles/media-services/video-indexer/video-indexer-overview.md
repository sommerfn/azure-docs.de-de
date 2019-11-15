---
title: Was ist Video Indexer?
titleSuffix: Azure Media Services
description: Eine Übersicht über den Azure Media Services Video Indexer-Dienst.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 09/23/2019
ms.author: juliako
ms.openlocfilehash: 9f6a5fa96034e0bf43ddd573a425de8d114d47ce
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73575110"
---
# <a name="what-is-video-indexer"></a>Was ist Video Indexer?

Video Indexer (VI) ist die KI-Lösung von Azure Media Services und Teil der Marke Azure Cognitive Services. Video Indexer bietet die Möglichkeit, tiefe Einblicke zu gewinnen (ohne Datenanalyse oder Programmierkenntnisse), indem es Machine Learning-Modelle verwendet, die auf mehreren Kanälen (Sprache, Gesang, Bild) basieren. Sie können die Modelle weiter anpassen und trainieren. Der Dienst ermöglicht eine intensive Suche, reduziert die Betriebskosten, ermöglicht neue Monetarisierungsmöglichkeiten und bringt neue Benutzererfahrungen für umfangreiche Videoarchive (mit niedrigen Eintrittsbarrieren).

Um mit dem Extrahieren von Erkenntnissen mit Video Indexer zu beginnen, müssen Sie ein Konto erstellen und Videos hochladen. Wenn Sie Ihre Videos zu Video Indexer hochladen, analysiert es durch Ausführen verschiedener KI-Modelle sowohl visuelle als auch akustische Informationen. Während Video Indexer Ihr Video analysiert, werden die Erkenntnisse von den KI-Modellen extrahiert.

Das folgende Diagramm ist eine Darstellung und keine technische Erläuterung, wie Video Indexer im Back-End funktioniert.

![Flussdiagramm zu Azure Media Services Video Indexer](./media/video-indexer-overview/model-chart.png)

## <a name="what-can-i-do-with-video-indexer"></a>Welche Möglichkeiten bietet Video Indexer?

Die Erkenntnisse von Video Indexer können auf viele Szenarien angewendet werden, darunter auch die Folgenden:

* *Intensivsuche*: Verwenden Sie die aus dem Video extrahierten Erkenntnisse, um die Suchfunktion für eine Videobibliothek zu verbessern. Beispielsweise kann die Indizierung von gesprochenem Text und Gesichtern die Suche nach Stellen in einem Video ermöglichen, in denen eine Person bestimmte Wörter benutzt hat oder zwei Personen zusammen gezeigt werden. Die Suche auf Grundlage solcher Erkenntnisse ist für Nachrichtenagenturen, Bildungseinrichtungen, Rundfunkanstalten, Besitzer von Unterhaltungsinhalten, Branchenanwendungen und im Allgemeinen für alle Branchen von Nutzen, die über eine Videobibliothek verfügen, in der Benutzer eine Suche durchführen müssen.
* *Inhaltserstellung*: Erstellen Sie Trailer, Highlightkompilationen, Inhalte von sozialen Medien oder Nachrichtenclips basierend auf den Erkenntnissen, die Video Indexer aus Ihren Inhalten extrahiert. Keyframes, Szenenmarkierungen und Zeitstempel für die Personen- und Bezeichnungsdarstellungen gestalten den Erstellungsprozess viel reibungsloser und einfacher und ermöglichen es Ihnen, zu den Teilen des Videos zu gelangen, die Sie für die von Ihnen erstellten Inhalte benötigen.
* *Barrierefreiheit*: Unabhängig davon, ob Sie Ihre Inhalte für Menschen mit Behinderungen zur Verfügung stellen möchten oder ob Sie wünschen, dass Ihre Inhalte in verschiedenen Regionen mit verschiedenen Sprachen verteilt werden, können Sie die Transkription und Übersetzung nutzen, die von Video Indexer in mehreren Sprachen bereitgestellt wird.
* *Monetarisierung*: Video Indexer kann Sie dabei unterstützen, den kommerziellen Wert von Videos zu steigern. Beispielsweise können Unternehmen aus Branchen, die auf Werbeeinnahmen angewiesen sind (Nachrichtenmedien, soziale Medien usw.) wirkungsvolle Anzeigen schalten, indem sie die extrahierten Erkenntnisse als zusätzliche Signale für den Anzeigenserver nutzen.
* *Inhaltsmoderation*: Verwenden Sie textbasierte und visuelle Moderationsmodelle, um Ihre Benutzer vor unangemessenen Inhalten zu schützen und zu überprüfen, ob die von Ihnen veröffentlichten Inhalte den Werten Ihrer Organisation entsprechen. Sie können bestimmte Videos automatisch blockieren oder Ihre Benutzer vor dem Inhalt warnen.
* *Empfehlungen*: Die aus Videos gewonnenen Erkenntnisse können genutzt werden, um die Kundenbindung zu verbessern, indem für die Benutzer relevante Stellen in einem Video hervorgehoben werden. Indem Sie jedes Video mit zusätzlichen Metadaten kennzeichnen, können Sie den Benutzern die relevantesten Videos empfehlen und den Teil des Videos hervorheben, der ihren Anforderungen entspricht.

## <a name="features"></a>Features

In der folgenden Liste sind die Erkenntnisse aufgeführt, die Sie aus Ihren Videos mithilfe von Video- und Audiomodellen von Video Indexer abrufen können:

### <a name="video-insights"></a>Erkenntnisse aus Videoaufnahmen

* **Gesichtserkennung:** erkennt und gruppiert im Video gezeigte Gesichter.
* **Prominentenerkennung:** Video Indexer kann automatisch mehr als eine Million Prominente erkennen, etwa wichtige Politiker/Politikerinnen, Schauspieler/Schauspielerinnen, Athleten/Athletinnen, Forscher/Forscherinnen und Führungskräfte aus dem Wirtschafts- und Technologiebereich. Die Daten zu diesen Prominenten sind auch auf verschiedenen Websites (IMDB, Wikipedia usw.) zu finden.
* **Kontobasierte Gesichtsidentifikation:** Video Indexer trainiert ein Modell für ein bestimmtes Konto. Gesichter im Video werden dann auf Grundlage des trainierten Modells erkannt. Weitere Informationen finden Sie unter [Anpassen eines Personenmodells mit der Video Indexer-Website](customize-person-model-with-website.md) und [Anpassen eines Personenmodells mit der Video Indexer-API](customize-person-model-with-api.md).
* **Extraktion von Gesichtern als Miniaturbild** („bestes Gesicht“): Identifiziert automatisch das am besten aufgenommene Gesicht in der jeweiligen Gruppe von Gesichtern (basierend auf Qualität, Größe und frontaler Position) und extrahiert es als Bildobjekt.
* **Optische Zeichenerkennung** (OCR): extrahiert im Video angezeigten Text.
* **Moderation visueller Inhalte:** erkennt nicht jugendfreie bzw. anzügliche visuelle Inhalte.
* **Identifikation von Beschriftungen:** identifiziert angezeigte visuelle Objekte und Aktionen.
* **Szenensegmentierung**: Bestimmt anhand visueller Hinweise, wann sich eine Szene im Video ändert. Eine Szene stellt ein einzelnes Ereignis dar und besteht aus einer Reihe von aufeinanderfolgenden Aufnahmen, die semantisch zusammenhängen.
* **Szenenwechselerkennung:** Bestimmt anhand visueller Hinweise, wann ein Szenenwechsel im Video erfolgt. Eine Aufnahme (Szenenwechsel) umfasst ist eine Reihe von Bildern, die von derselben Filmkamera aufgenommen wurden. Weitere Informationen finden Sie unter [Szenen, Aufnahmen und Keyframes](scenes-shots-keyframes.md).
* **Erkennung schwarzer Frames:** erkennt schwarze Frames im Video.
* **Extraktion von Keyframes:** erkennt stabile Keyframes in einem Video.
* **Durchlaufender Abspann**: Identifiziert den Anfang und das Ende des durchlaufenden Abspanns am Ende von Fernsehsendungen und Filmen.
* **Erkennung animierter Figuren** (Vorschauversion): Bestimmung, Gruppierung und Erkennung von Figuren in animierten Inhalten mittels Integration von [Cognitive Services Custom Vision](https://azure.microsoft.com/services/cognitive-services/custom-vision-service/). Weitere Informationen finden Sie unter [Erkennung animierter Figuren](animated-characters-recognition.md).
* **Erkennung von redaktionellen Aufnahmetypen**: Tagging (Kategorisierung) von Aufnahmen auf der Grundlage ihres Typs (etwa Totale, Halbtotale, Nahaufnahme, extreme Nahaufnahme, zwei Personen, mehrere Personen, Außenaufnahme, Innenaufnahme usw.). Weitere Informationen finden Sie unter [Erkennung von redaktionellen Aufnahmetypen](scenes-shots-keyframes.md#editorial-shot-type-detection).

### <a name="audio-insights"></a>Erkenntnisse aus Audioaufnahmen

* **Automatische Sprachenerkennung:** Die Lösung identifiziert automatisch die vorherrschend gesprochene Sprache. Unterstützt werden Chinesisch (vereinfacht), Deutsch, Englisch, Französisch, Italienisch, Japanisch, Portugiesisch (Brasilien), Russisch und Spanisch. Falls die Sprache nicht zuverlässig identifiziert werden kann, geht Video Indexer davon aus, dass die gesprochene Sprache Englisch ist. Weitere Informationen finden Sie unter [Automatisches Identifizieren von gesprochener Sprache mit dem Modell zur Sprachenerkennung](language-identification-model.md).
* **Spracherkennung und Transkription für mehrere Sprachen** (Vorschau): Erkennt die gesprochene Sprache in verschiedenen Segmenten automatisch anhand der Audiodaten. Die Funktion sendet jedes Segment der zu transkribierenden Mediendatei und kombiniert die Transkriptionen dann wieder zu einer einzigen Transkription. Weitere Informationen finden Sie unter [Automatisches Erkennen und Transkribieren mehrsprachiger Inhalte](multi-language-identification-transcription.md).
* **Audiotranskription:** konvertiert Sprache in Text in 12 Sprachen und lässt Erweiterungen zu. Unterstützt werden Arabisch, Chinesisch (vereinfacht), Deutsch, Englisch, Französisch, Italienisch, Japanisch, brasilianisches Portugiesisch, Spanisch, Russisch, Hindi und Koreanisch.
* **Untertitelung:** erstellt Untertitel in drei Formaten: VTT, TTML und SRT.
* **Verarbeitung von zwei Kanälen:** erkennt automatisch ein getrenntes Transkript und sorgt für eine Zusammenführung auf einer einzelnen Zeitachse.
* **Rauschunterdrückung:** bereinigt (basierend auf Skype-Filtern) Telefonaudio oder verrauschte Aufnahmen.
* **Transkriptanpassung** (CRIS): ermöglicht das Trainieren benutzerdefinierter Spracherkennungsmodelle zur Erstellung branchenspezifischer Transkripte. Weitere Informationen finden Sie unter [Anpassen eines Sprachmodells mit der Video Indexer-Website](customize-language-model-with-website.md) und [Anpassen eines Sprachmodells mit den Video Indexer-APIs](customize-language-model-with-api.md).
* **Sprecheraufzählung:** kann erkennen und zuordnen, welcher Sprecher wann was gesagt hat.
* **Sprecherstatistiken:** bietet Statistiken zum Verhältnis zwischen Sprechern und ihrem Anteil an der Konversation.
* **Moderation von Textinhalten:** erkennt im Transkript des Audios anstößigen Text.
* **Audioeffekte:** erkennt Audioeffekte wie Händeklatschen, Sprache und Stille.
* **Emotionserkennung:** erkennt Emotionen basierend auf Sprache (was gesagt wird) und Tonalität der Stimme (wie etwas gesagt wird). Bei den Emotionen kann es sich um Freude, Trauer, Wut oder Angst handeln.
* **Übersetzung:** erstellt Übersetzungen des Audiotranskripts in 54 Sprachen.

### <a name="audio-and-video-insights-multi-channels"></a>Erkenntnisse aus Audio- und Videoaufnahmen (mehrere Kanäle)

Bei Indizierung nach einem Kanal steht ein Teilergebnis für die folgenden Modelle zur Verfügung.

* **Extraktion von Schlüsselwörtern:** extrahiert Stichwörter aus Sprache und sichtbarem Text.
* **Extraktion benannter Entitäten**: Extrahiert mithilfe der Verarbeitung natürlicher Sprache (Natural Language Processing, NLP) Marken, Standorte und Personen aus Sprache und visuellem Text.
* **Themenerschließung:** zieht in den Transkripten einen Rückschluss auf Hauptthemen. Eine IPTC-Taxonomie zweiter Ebene ist eingeschlossen.
* **Artefakte**: Extrahiert für jedes der Modelle eine umfangreiche Menge von Artefakten mit höherem Detailgrad.
* **Standpunktanalyse:** erkennt anhand von Sprache und sichtbarem Text positive, negative und neutrale Stimmungen.

## <a name="how-can-i-get-started-with-video-indexer"></a>Wie kann ich mich mit Video Indexer vertraut machen?

Es gibt drei Möglichkeiten, auf die Funktionen von Video Indexer zuzugreifen:

* Video Indexer-Portal: eine benutzerfreundliche Lösung, mit der Sie das Produkt bewerten, das Konto verwalten und Modelle anpassen können.

    Weitere Informationen zum Portal finden Sie unter [Erste Schritte mit der Video Indexer-Website](video-indexer-get-started.md).  

* API-Integration: Alle Funktionen von Video Indexer sind über eine REST-API verfügbar, über die Sie die Lösung in Ihre Apps und Infrastruktur integrieren können.

    Informationen zu den ersten Schritten als Entwickler finden Sie unter  [Verwenden der Video Indexer-REST-API](video-indexer-use-apis.md).

* Einbettbares Widget: Ermöglicht es Ihnen, die Video Indexer-Erkenntnisse-, die Player- und die Editor-Umgebung in Ihre App einzubetten.

    Weitere Informationen finden Sie unter  [Einbetten von visuellen Widgets in Ihre Anwendung](video-indexer-embed-widgets.md).

Wenn Sie die Website nutzen, werden die Erkenntnisse als Metadaten hinzugefügt und im Portal angezeigt. Wenn Sie APIs verwenden, sind die Erkenntnisse als JSON-Datei verfügbar.

## <a name="next-steps"></a>Nächste Schritte

Sind Sie bereit für die ersten Schritte mit Video Indexer? Weitere Informationen finden Sie in den folgenden Artikeln:

- [Erste Schritte mit der Video Indexer-Website](video-indexer-get-started.md)
- [Verarbeiten von Inhalten mit der Video Indexer-REST-API](video-indexer-use-apis.md)
- [Einbetten von visuellen Widgets in Ihre Anwendung](video-indexer-embed-widgets.md)
