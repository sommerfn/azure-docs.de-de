---
title: Interpretieren Ihrer Scorecard | Microsoft-Dokumentation
description: Häufig gestellte Fragen zu Azure Internet Analyzer.
services: internet-analyzer
author: mattcalder
ms.service: internet-analyzer
ms.topic: guide
ms.date: 10/16/2019
ms.author: mebeatty
ms.openlocfilehash: a05c704f62d93bdc110d8a5943dd266ca22eb5f4
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510299"
---
# <a name="interpreting-your-scorecard"></a>Interpretieren Ihrer Scorecard

Die Registerkarte „Scorecard“ enthält die aggregierten und analysierten Ergebnisse Ihrer Tests. Für jeden Test gibt es eigene Scorecards. Scorecards bieten eine schnelle und aussagekräftige Zusammenfassung von Messergebnissen, um für Ihre Netzwerkanforderungen datengesteuerte Lösungen zu liefern. Azure Internet Analyzer kümmert sich um die Analyse, sodass Sie sich auf die Entscheidung konzentrieren können.

Die Registerkarte „Scorecard“ finden Sie im Internet Analyzer-Menü „Ressourcen“. 


## <a name="filters"></a>Filter

* ***Test (Testen):*** Wählen Sie den Test aus, dessen Ergebnisse Sie anzeigen möchten. Für jeden Test gibt es eine eigene Scorecard. Testdaten werden angezeigt, sobald genügend Daten vorhanden sind, um die Analyse abzuschließen – in den meisten Fällen innerhalb von 24 Stunden. 
* ***Zeitraum und Enddatum***: Pro Tag werden drei Scorecards generiert, von denen jede zu einem anderen Aggregationszeitraum gehört: 24 Stunden vorher (Tag), sieben Tage vorher (Woche) und 30 Tage vorher (Monat). Verwenden Sie den Filter „Enddatum“, um den letzten Tag des Zeitraums auszuwählen, den Sie anzeigen möchten. 
* ***Land:*** Für jedes Land, in dem Sie Endbenutzer haben, wird eine Scorecard generiert. Der globale Filter enthält alle Endbenutzer.

## <a name="measurement-count"></a>Anzahl der Messungen

Die Anzahl der Messungen wirkt sich auf die Zuverlässigkeit der Analyse aus. Je höher die Anzahl, desto genauer das Ergebnis. Pro Tag und Endpunkt sollten mindestens 100 Messungen das Ziel sein. Wenn die Anzahl der Messungen zu niedrig ist, konfigurieren Sie den JavaScript-Client so, dass er in Ihrer Anwendung häufiger ausgeführt wird. Die Anzahl der Messungen für die Endpunkte A und B sollte sehr ähnlich sein, obwohl kleine Unterschiede zu erwarten und zulässig sind. Bei großen Unterschieden darf den Ergebnissen nicht vertraut werden.

## <a name="percentiles"></a>Perzentile

Die in Millisekunden gemessene Latenz ist eine beliebte Metrik zur Messung von Geschwindigkeit zwischen einer Quelle und einem Ziel im Internet. Latenzdaten sind nicht normal verteilt (folgen also keiner „Glockenkurve“), da es einen sog. „Long Tail“ mit hohen Latenzwerten gibt, die die Ergebnisse bei der Verwendung von Statistiken wie dem arithmetischen Mittel verzerren. Alternativ bieten Perzentile eine „verteilungsfreie“ Möglichkeit, Daten zu analysieren. Als Beispiel fasst der Median oder das 50. Perzentil die Mitte der Verteilung zusammen, wobei die Hälfte der Werte darüber und die andere Hälfte darunter liegt. Ein 75. Perzentilwert bedeutet, dass er größer als 75 % aller Werte in der Verteilung ist. Internet Analyzer bezeichnet Perzentile in der Kurzform P50, P75 und P95.

Internet Analyzer-Perzentile sind _Beispielmetriken_. Dies steht im Gegensatz zur tatsächlichen _Populationsmetrik_. Beispielsweise ist die tägliche mittlere Latenz der echten Population zwischen Studenten der University of Southern California und Microsoft der mittlere Latenzwert aller Anforderungen im Verlauf dieses Tages. In der Praxis ist die Messung des Werts aller Anforderungen undurchführbar, sodass wir davon ausgehen, dass eine relativ große Stichprobe repräsentativ für die tatsächliche Population ist.

Zu Analysezwecken ist P50 (Median) als erwarteter Wert für eine Latenzverteilung nützlich. Höhere Perzentile, wie z. B. P95, sind nützlich, um festzustellen, wie hoch die Latenz den ungünstigsten Fällen ist. Wenn Sie daran interessiert sind, die Kundenlatenz im Allgemeinen zu verstehen, ist P50 die richtige Metrik, auf die Sie sich konzentrieren sollten. Wenn es Ihnen darum geht, die Leistung der Kunden mit der schlechtesten Leistung zu verstehen, dann sollte P95 im Mittelpunkt stehen. P75 ist ein Mittelweg zwischen diesen beiden.


## <a name="deltas"></a>Deltas

Ein Delta ist der Unterschied bei Metrikwerten für die Endpunkte A und B. Deltas werden berechnet, um den Vorteil von B gegenüber A darzustellen. Positive Werte bedeuten, dass B besser abgeschnitten hat als A, während negative Werte darauf hinweisen, dass die Leistung von B schlechter ist. Deltas können absolut (z. B. 10 Millisekunden) oder relativ (5 %) sein.

## <a name="confidence-interval"></a>Konfidenzintervall 

Konfidenzintervalle sind eine Reihe von Werten, die mit hoher Wahrscheinlichkeit die Populationsmetrik enthalten, wie z. B. Median, P75 oder Durchschnitt. Wir folgen der allgemeinen statistischen Konvention der Verwendung des Konfidenzintervall 95 %.

Für Internet Analyzer ist ein schmales Konfidenzintervall gut, da es zeigt, dass die Stichprobenmetrik wahrscheinlich sehr nahe an der tatsächlichen Populationsmetrik liegt. Ein breites Konfidenzintervall bedeutet weniger Gewissheit, dass unsere Stichprobenmetrik die wahre Populationsmetrik widerspiegelt. Der beste Weg, das Konfidenzintervall zu verbessern, ist die Erhöhung der Anzahl der Messungen.

## <a name="time-series"></a>Zeitreihe 

Eine Zeitreihe zeigt, wie sich eine Metrik im Laufe der Zeit ändert. Im Internet gibt es viele zeitliche Faktoren, die sich auf die Leistung auswirken, wie z.B. Datenverkehrsspitzen, Unterschiede bei der Population zwischen Wochentagen und Wochenenden sowie Feiertage.


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in unserer [Übersicht über Internet Analyzer](internet-analyzer-overview.md).
