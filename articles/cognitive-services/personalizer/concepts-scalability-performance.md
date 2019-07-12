---
title: Vertiefendes Lernen – Personalisierung
titleSuffix: Azure Cognitive Services
description: 'Bei hochleistungsfähigen und stark frequentierten Websites und Anwendungen müssen beim Dienst „Personalisierung“ hinsichtlich Skalierbarkeit und Leistung zwei Hauptfaktoren berücksichtigt werden: Latenz und Trainingsdurchsatz.'
services: cognitive-services
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: overview
ms.date: 06/07/2019
ms.author: edjez
ms.openlocfilehash: 242cc1b96c08cd79dc3e2ef5efbbe96a934b8ad3
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67068301"
---
# <a name="scalability-and-performance"></a>Skalierbarkeit und Leistung

Bei hochleistungsfähigen und stark frequentierten Websites und Anwendungen müssen beim Dienst „Personalisierung“ hinsichtlich Skalierbarkeit und Leistung zwei Hauptfaktoren berücksichtigt werden:

* Bei Aufrufen der Rangfolge-API die Wartezeit kurz halten
* Sicherstellen, dass der Trainingsdurchsatz mit der Ereigniseingabe Schritt hält

Die Personalisierung kann sehr schnell einen Rang zurückgeben, wobei der größte Teil der Aufrufdauer der Kommunikation über die REST-API vorbehalten ist. Azure skaliert automatisch die Fähigkeit zur schnellen Reaktion auf Anforderungen.

##  <a name="low-latency-scenarios"></a>Szenarien mit kurzer Wartezeit

Einige Anwendungen erfordern bei der Rückgabe eines Rangs kurze Wartezeiten. Dies ist auf folgenden Gründen erforderlich:

* Um zu verhindern, dass der Benutzer eine spürbare Zeitspanne auf die Anzeige von bewerteten Inhalten wartet.
* Um einem Server mit extrem hohen Datenverkehr beim Vermeiden zu helfen, knappe Computezeit und Netzwerkverbindungen zu binden.

<!--

If your web site is scaled on your infrastructure, you can avoid making HTTP calls by hosting the Personalizer API in your own servers running a Docker container.

This change would be transparent to your application, other than using an endpoint URL referring to the running docker instances as opposed to an online service in the cloud.



### Extreme Low Latency Scenarios

If you require latencies under a millisecond, and have already tested using Personalizer via containers, please contact our support team so we can assess your scenario and provide guidance suited to your needs.

-->

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