---
title: Video Indexer-Konzepte
titleSuffix: Azure Media Services
description: In diesem Thema werden einige Konzepte des Video Indexer-Diensts beschrieben.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 05/15/2019
ms.author: juliako
ms.openlocfilehash: e3795b699f5518efdbef117fb24d118041ed3e78
ms.sourcegitcommit: 35715a7df8e476286e3fee954818ae1278cef1fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73839577"
---
# <a name="video-indexer-concepts"></a>Video Indexer-Konzepte
 
In diesem Artikel werden einige Konzepte des Video Indexer-Diensts vorgestellt.
    
## <a name="summarized-insights"></a>Zusammenfassung der Erkenntnisse

Die Zusammenfassung der Erkenntnisse enthält eine aggregierte Ansicht der Daten: Gesichter, Themen, Emotionen. Beispielsweise müssen nicht Tausende von Zeitbereichen durchlaufen werden, um zu überprüfen, welche Gesichter darin enthalten sind. Die zusammengefassten Erkenntnisse enthalten alle Gesichter und dazu jeweils die Zeitbereiche und den prozentualen Anteil der Sichtbarkeit.

## <a name="time-range-vs-adjusted-time-range"></a>Vergleich: Zeitbereich und angepasster Zeitbereich

„TimeRange“ ist der Zeitbereich im Originalvideo. „adjustedTimeRange“ ist der Zeitbereich im Verhältnis zur aktuellen Wiedergabeliste. Da Sie eine Wiedergabeliste aus verschiedenen Zeilen verschiedener Videos erstellen können, haben Sie die Möglichkeit, nur eine Zeile aus einem einstündigen Video zu verwenden, z.B. 10:00 - 10:15. In diesem Fall erhalten Sie eine Wiedergabeliste mit einer Zeile, in der der Zeitbereich 10:00 - 10: 15, der Wert für adjustedTimeRange aber 00:00 - 00:15 ist.
 
## <a name="blocks"></a>Blöcke

Die Blöcke sollen das Durchgehen der Daten vereinfachen. Die Unterteilung in Blöcke kann beispielsweise darauf basieren, dass sich der Sprecher ändert oder dass es zu einer längeren Pause kommt.

## <a name="next-steps"></a>Nächste Schritte

Informationen zu den ersten Schritten finden Sie unter [Registrieren und Hochladen Ihres ersten Videos](video-indexer-get-started.md).

## <a name="see-also"></a>Weitere Informationen

[What is Video Indexer? (preview)](video-indexer-overview.md) (Was ist Video Indexer? (Vorschauversion))
