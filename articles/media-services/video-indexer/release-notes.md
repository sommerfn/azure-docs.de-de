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
ms.date: 08/29/2019
ms.author: juliako
ms.openlocfilehash: a7d178972a8c8b413e3c6336c403348f1eb78abb
ms.sourcegitcommit: 2aefdf92db8950ff02c94d8b0535bf4096021b11
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/03/2019
ms.locfileid: "70232594"
---
# <a name="azure-media-services-video-indexer-release-notes"></a>Azure Media Services Video Indexer: Versionshinweise

Damit Sie bezüglich der aktuellen Entwicklungen auf dem neuesten Stand bleiben, bietet dieser Artikel Informationen zu Folgendem:

* Neueste Versionen
* Bekannte Probleme
* Fehlerbehebungen
* Veraltete Funktionen

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
