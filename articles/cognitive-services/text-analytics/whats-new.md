---
title: Neuigkeiten bei der Textanalyse-API
titlesuffix: Text Analytics - Azure Cognitive Services
description: Erfahren Sie etwas über neue Entwicklungen beim Textanalysedienst.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: aahi
ms.openlocfilehash: 44ef6fb118be4d1110a693faded6c57bc8b4e2fd
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73499961"
---
# <a name="whats-new-in-the-text-analytics-api"></a>Neuigkeiten bei der Textanalyse-API

Die Textanalyse-API wird fortlaufend aktualisiert. Damit Sie bezüglich der aktuellen Entwicklungen auf dem neuesten Stand bleiben, bietet dieser Artikel Informationen zu neuen Releases und Funktionen.

## <a name="named-entity-recognition-v3-public-preview---october-2019"></a>Named Entity Recognition v3 – öffentliche Vorschau: Oktober 2019

Die nächste Version von Named Entity Recognition (NER, Erkennung benannter Entitäten) ist jetzt als öffentliche Vorschauversion erhältlich und bietet erweiterte Erkennung und Kategorisierung von in Text gefundenen Entitäten. Die Lösung umfasst Folgendes:

* Erkennung der folgenden neuen Entitätstypen:
    * Telefonnummer
    * IP-Adresse

* Einen neuen Endpunkt für das Erkennen von Entitätstypen für personenbezogene Informationen (nur für die englische Sprache)
* Separate Endpunkte für die Entitätserkennung und Entitätsverknüpfung.

Die Entitätsverknüpfung unterstützt Englisch und Spanisch. Die NER-Sprachunterstützung ist je nach Entitätstyp unterschiedlich. Weitere Informationen finden Sie über den Link unten. 

> [!div class="nextstepaction"]
> [Weitere Informationen zu Named Entity Recognition v3](how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-v3-public-preview)

## <a name="sentiment-analysis-v3-public-preview---october-2019"></a>Standpunktanalyse v3 – Öffentliche Vorschau: Oktober 2019

Die nächste Version der Standpunktanalyse ist jetzt als Public Preview verfügbar und bietet erhebliche Verbesserungen bei Genauigkeit und Detailgrad der Textkategorisierung und -bewertung der API. Außerdem bietet sie Folgendes:

* Automatische Bezeichnung für verschiedene Stimmungen im Text
* Standpunktanalyse und Ausgabe auf Dokument- und Satzebene 

Unterstützt derzeit Englisch (`en`), Japanisch (`ja`), vereinfachtes Chinesisch (`zh-Hans`), traditionelles Chinesisch (`zh-Hant`), Französisch (`fr`), Italienisch (`it`), Spanisch (`es`), Niederländisch (`nl`), Portugiesisch (`pt`) und Deutsch (`de`) und ist in den folgenden Regionen erhältlich: `Australia East`, `Central Canada`, `Central US`, `East Asia`, `East US`, `East US 2`, `North Europe`, `Southeast Asia`, `South Central US`, `UK South`, `West Europe` und `West US 2`.

> [!div class="nextstepaction"]
> [Weitere Informationen zur Standpunktanalyse v3](how-tos/text-analytics-how-to-sentiment-analysis.md#sentiment-analysis-v3-public-preview)

## <a name="next-steps"></a>Nächste Schritte

* [Worum handelt es sich bei der Textanalyse-API?](overview.md)  
* [Beispielbenutzerszenarien](text-analytics-user-scenarios.md)
* [Standpunktanalyse](how-tos/text-analytics-how-to-sentiment-analysis.md)
* [Sprachenerkennung](how-tos/text-analytics-how-to-language-detection.md)
* [Entitätserkennung](how-tos/text-analytics-how-to-entity-linking.md)
* [Schlüsselbegriffserkennung](how-tos/text-analytics-how-to-keyword-extraction.md)
