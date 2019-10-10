---
title: 'Azure Media Services Video Indexer: Versionshinweise | Microsoft-Dokumentation'
description: Damit Sie bezüglich der aktuellen Entwicklungen auf dem neuesten Stand bleiben, bietet dieser Artikel Informationen zu Azure Media Services Video Indexer.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.subservice: video-indexer
ms.workload: na
ms.topic: article
ms.date: 09/26/2019
ms.author: juliako
ms.openlocfilehash: 4953160a30e45b9be1ff249b6cd1f74e01bb14f3
ms.sourcegitcommit: 2d9a9079dd0a701b4bbe7289e8126a167cfcb450
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/29/2019
ms.locfileid: "71672657"
---
# <a name="azure-media-services-video-indexer-release-notes"></a>Azure Media Services Video Indexer: Versionshinweise

Damit Sie bezüglich der aktuellen Entwicklungen auf dem neuesten Stand bleiben, bietet dieser Artikel Informationen zu Folgendem:

* Neueste Versionen
* Bekannte Probleme
* Fehlerbehebungen
* Veraltete Funktionen

## <a name="september-2019"></a>September 2019
 
Auf der IBC 2019 wurden mehrere Verbesserungen angekündigt:
 
* Erkennung animierter Figuren (öffentliche Vorschauversion)

    Die Fähigkeit zum Auffinden, Gruppieren und Erkennen von Figuren in animierten Inhalten mithilfe der Integration in Custom Vision. Weitere Informationen finden Sie unter [Erkennung animierter Figuren](animated-characters-recognition.md).
* Mehrsprachige Identifikation (öffentliche Vorschauversion)

    Erkennen von Segmenten in mehreren Sprachen in der Audiospur und Erstellen eines mehrsprachigen Transkripts auf ihrer Grundlage. Zu Anfang unterstützt: Englisch, Spanisch, Deutsch und Französisch. Weitere Informationen finden Sie unter [Automatisches Erkennen und Transkribieren mehrsprachiger Inhalte](multi-language-identification-transcription.md).
* Extraktion benannter Entitäten für Personen und Ort

    Extrahiert mithilfe der Verarbeitung natürlicher Sprache (Natural Language Processing, NLP) Marken, Standorte und Personen aus Sprache und visuellem Text.
* Redaktionelle Typklassifizierung für Aufnahmen

    Markierung von Aufnahmen mit redaktionellen Typen wie Nahaufnahme, Halbtotale, zwei Personen, innen, außen usw. Weitere Informationen finden Sie unter [Erkennung von redaktionellen Aufnahmetypen](scenes-shots-keyframes.md#editorial-shot-type-detection).
* Verbesserte Themendeduktion – jetzt mit Abdeckung von Stufe 2
    
    Das Themendeduktionsmodell unterstützt jetzt mehr Detailebenen der IPTC-Taxonomie. Sämtliche Details finden Sie unter [Azure Media Services new AI-powered innovation](https://azure.microsoft.com/blog/azure-media-services-new-ai-powered-innovation/) (Neue, KI-gestützte Innovationen in Azure Media Services).

## <a name="august-2019"></a>August 2019
 
### <a name="video-indexer-deployed-in-uk-south"></a>Video Indexer bereitgestellt für „Vereinigtes Königreich, Süden“

Sie können jetzt ein kostenpflichtiges Video Indexer-Konto in der Region „Vereinigtes Königreich, Süden“ erstellen.

### <a name="new-editorial-shot-type-insights-available"></a>Neue redaktionelle Aufnahmearten verfügbar

Neue Tags, die Videoaufnahmen hinzugefügt werden, bieten redaktionelle „Aufnahmearten“, um sie mit gängigen redaktionellen Phrasen zu kennzeichnen, die im Workflow der Inhaltserstellung verwendet werden, z.B.: extreme Nahaufnahme (extreme closeup), Nahaufnahme (closeup), Weitwinkel (wide), Medium, Zwei-Personen-Aufnahme (two shot), Outdoor, Indoor, linkes Gesicht (left face) und rechtes Gesicht (right face) (verfügbar im JSON).

### <a name="new-people-and-locations-entities-extraction-available"></a>Neue Extraktion von Personen- und Standortentitäten verfügbar

Video Indexer kennzeichnet benannte Orte und Personen über Verarbeitung natürlicher Sprache (Natural Language Processing, NLP) aus der OCR und der Transkription des Videos. Video Indexer verwendet einen Machine Learning-Algorithmus, um zu erkennen, wann bestimmte Standorte (z.B. der Eiffelturm) oder Personen (z.B. John Doe) in einem Video ausgesprochen werden.

### <a name="keyframes-extraction-in-native-resolution"></a>Extraktion von Keyframes in nativer Auflösung

Keyframes, die von Video Indexer extrahiert wurden, sind in der ursprünglichen Auflösung des Videos verfügbar.
 
### <a name="ga-for-training-custom-face-models-from-images"></a>Allgemeine Verfügbarkeit für Trainieren von benutzerdefinierten Gesichtern aus Bildern

Das Trainieren von Gesichtern aus Bildern wurde vom Vorschaumodus in die allgemeine Verfügbarkeit überführt (verfügbar über eine API und im Portal).

> [!NOTE]
> Der Übergang von „Vorschauversion in allgemeine Verfügbarkeit“ hat keine Auswirkungen auf die Preisgestaltung.

### <a name="hide-gallery-toggle-option"></a>Option zum Ausblenden von „Katalog“

Sie können nun die Registerkarte „Katalog“ im Portal ausblenden (ähnlich dem Ausblenden der Registerkarte „Beispiele“).
 
### <a name="maximum-url-size-increased"></a>Maximale URL-Größe erhöht

Unterstützung für URL-Abfragezeichenfolgen von 4096 (anstelle von 2048) bei der Indizierung eines Videos.
 
### <a name="support-for-multi-lingual-projects"></a>Unterstützung für mehrsprachige Projekte

Projekte können nun auf Basis von Videos erstellt werden, die in verschiedenen Sprachen indiziert sind (nur API).

## <a name="july-2019"></a>Juli 2019

### <a name="editor-as-a-widget"></a>Editor als Widget

Der Video Indexer-KI-Editor ist nun als Widget verfügbar und kann in Kundenanwendungen eingebettet werden.

### <a name="update-custom-language-model-from-closed-caption-file-from-the-portal"></a>Aktualisieren des benutzerdefinierten Sprachmodells aus der Untertiteldatei über das Portal

Kunden können im Portal auf der Seite zur Anpassung als Eingabe für Sprachmodelle die Dateiformate VTT, SRT und TTML angeben.

## <a name="june-2019"></a>Juni 2019

### <a name="video-indexer-deployed-to-japan-east"></a>Video Indexer bereitgestellt für „Japan, Osten“

Sie können jetzt ein kostenpflichtiges Video Indexer-Konto in der Region „Japan, Osten“ erstellen.

### <a name="create-and-repair-account-api-preview"></a>Erstellen und Reparieren der Konto-API (Vorschau)

Eine neue API wurde hinzugefügt, die es Ihnen ermöglicht, [den Azure Media Services-Verbindungsendpunkt oder -Schlüssel zu aktualisieren](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Update-Paid-Account-Azure-Media-Services?&groupBy=tag).

### <a name="improve-error-handling-on-upload"></a>Verbesserung der Fehlerbehandlung beim Hochladen 

Eine beschreibende Meldung wird im Falle einer Fehlkonfiguration des zugrunde liegenden Azure Media Services-Kontos zurückgegeben.

### <a name="player-timeline-keyframes-preview"></a>Vorschau der Keyframes in der Player-Zeitachse 

Sie können jetzt eine Bildvorschau für jeden Zeitpunkt auf der Zeitachse des Players anzeigen.

### <a name="editor-semi-select"></a>Editor-Teilauswahl

Sie können jetzt eine Vorschau aller Erkenntnisse anzeigen, die als Ergebnis der Auswahl eines bestimmten Erkenntniszeitraums im Editor ausgewählt sind.

## <a name="may-2019"></a>Mai 2019

### <a name="update-custom-language-model-from-closed-caption-file"></a>Aktualisieren des benutzerdefinierte Sprachmodells aus der Untertiteldatei

Die APIs [Benutzerdefiniertes Sprachmodell erstellen](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Create-Language-Model?&groupBy=tag) und [Benutzerdefiniertes Sprachmodell aktualisieren](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Update-Language-Model?&groupBy=tag) unterstützen jetzt VTT-, SRT-, und TTML-Dateiformate als Eingabe für Sprachmodelle.

Beim Aufrufen der [API „Video-Transkript aktualisieren“](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Update-Video-Transcript?&pattern=transcript) wird die Transkription automatisch hinzugefügt. Das dem Video zugeordnete Trainingsmodell wird ebenfalls automatisch aktualisiert. Informationen zum Anpassen und Trainieren Ihres Sprachmodells finden Sie unter [Anpassen eines Sprachmodells mit Video Indexer](customize-language-model-overview.md).

### <a name="new-download-transcript-formats--txt-and-csv"></a>Neue Transkriptformate zum Herunterladen – TXT und CSV

Zusätzlich zum bereits unterstützten Untertitelformat (SRT VTT und TTML) unterstützt Video Indexer jetzt das Herunterladen des Transkripts im TXT- und CSV-Format.

## <a name="next-steps"></a>Nächste Schritte

[Übersicht](video-indexer-overview.md)
