---
title: 'Skalierbarkeit und Leistung: Personalisierung'
titleSuffix: Azure Cognitive Services
description: 'Bei hochleistungsfähigen und stark frequentierten Websites und Anwendungen müssen beim Dienst „Personalisierung“ hinsichtlich Skalierbarkeit und Leistung zwei Hauptfaktoren berücksichtigt werden: Latenz und Trainingsdurchsatz.'
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 10/24/2019
ms.author: diberry
ms.openlocfilehash: 5ac9a870cb05328f040febd0f8161a97f0982e09
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73490788"
---
# <a name="scalability-and-performance"></a>Skalierbarkeit und Leistung

Bei hochleistungsfähigen und stark frequentierten Websites und Anwendungen müssen beim Dienst „Personalisierung“ hinsichtlich Skalierbarkeit und Leistung zwei Hauptfaktoren berücksichtigt werden:

* Bei Aufrufen der Rangfolge-API die Wartezeit kurz halten
* Sicherstellen, dass der Trainingsdurchsatz mit der Ereigniseingabe Schritt hält

Die Personalisierung kann schnell einen Rang zurückgeben, wobei der größte Teil der Aufrufdauer der Kommunikation über die REST-API vorbehalten ist. Azure skaliert automatisch die Fähigkeit zur schnellen Reaktion auf Anforderungen.

##  <a name="low-latency-scenarios"></a>Szenarien mit kurzer Wartezeit

Einige Anwendungen erfordern bei der Rückgabe eines Rangs kurze Wartezeiten. Geringe Latenzzeiten sind erforderlich:

* Um zu verhindern, dass der Benutzer eine spürbare Zeitspanne auf die Anzeige von bewerteten Inhalten wartet.
* Um einem Server mit extrem hohen Datenverkehr beim Vermeiden zu helfen, knappe Computezeit und Netzwerkverbindungen zu binden.


## <a name="scalability-and-training-throughput"></a>Skalierbarkeit und Trainingsdurchsatz

Die Personalisierung funktioniert, indem ein Modell aktualisiert wird, das basierend auf von der Personalisierung asynchron gesendeten Nachrichten gemäß Rangfolgen- und Relevanz-API neu trainiert wird. Diese Nachrichten werden über eine Azure Event Hub-Instanz für die Anwendung gesendet.

 Es ist unwahrscheinlich, dass die meisten Anwendungen den maximalen Teilnehmer- und Trainingsdurchsatz des Diensts „Personalisierung“ erreichen. Wenngleich das Erreichen dieses Maximums die Anwendung nicht bremst, würde es bedeuten, dass Event Hub-Warteschlangen intern schneller gefüllt werden, als sie bereinigt werden können.

## <a name="how-to-estimate-your-throughput-requirements"></a>Schätzen Ihrer Durchsatzanforderungen

* Schätzen Sie die durchschnittliche Anzahl von Bytes pro bewertetem Ereignis, indem Sie die Länge der JSON-Dokumente für Kontext und Aktion addieren.
* Dividieren Sie 20 MB/s durch diese geschätzte durchschnittliche Anzahl von Bytes.

Wenn Ihre durchschnittliche Nutzlast beispielsweise 500 Merkmale aufweist und jedes davon geschätzte 20 Zeichen lang ist, dann hat jedes Ereignis eine Größe von etwa 10 KB. Bei dieser Schätzungen gilt: 20.000.000 : 10.000 = 2.000 Ereignisse/s, also ca. 173 Mio. Ereignisse pro Tag. 

Wenn Sie diese Grenzwerte erreichen, wenden Sie sich zwecks Architekturberatung an unser Supportteam.

## <a name="next-steps"></a>Nächste Schritte

[Erstellen und Konfigurieren des Diensts „Personalisierung“](how-to-settings.md)