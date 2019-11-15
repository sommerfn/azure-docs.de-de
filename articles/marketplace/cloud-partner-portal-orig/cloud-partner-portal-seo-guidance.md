---
title: Azure Marketplace-Leitfaden zur Suchmaschinenoptimierung (SEO)
description: Enthält Anleitungen zu Verbesserungen der Suchmaschinenoptimierung (Search Engine Optimization, SEO).
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/09/2019
ms.author: pabutler
ms.openlocfilehash: 7115798faadc3209413d22a384433417ec0ddff0
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73819582"
---
# <a name="azure-marketplace-seo-guidance"></a>Azure Marketplace-Leitfaden zur Suchmaschinenoptimierung (SEO)

In diesem Artikel wird erläutert, wie Sie die Auffindbarkeit Ihres Angebots durch die Suchfunktionen in [Azure Marketplace](https://azuremarketplace.microsoft.com) und [AppSource](https://appsource.microsoft.com) maximieren können. 


## <a name="general-explanation-of-algorithm"></a>Allgemeine Erläuterung des Algorithmus

Microsoft Marketplace verwendet Azure Cognitive Search, um die Suchfunktionen der Website zu unterstützen. Der Algorithmus basiert auf dem Maß für Vorkommenshäufigkeit–inverse Dokumenthäufigkeit (term frequency–inverse document frequency, [Tf-idf-Maß](https://en.wikipedia.org/wiki/Tf–idf)). Es wird die Standardversion des [Lucene-Analysetools](https://lucene.apache.org/core/) verwendet.

Grundsätzlich werden alle Textfelder, Kategorien und Branchen in die Gewichtung der Relevanz einbezogen. Spezielle Begriffe, die von Apps selten, in Ihrer App aber häufig verwendet werden, führen zu einer höheren Trefferquote bei einer Suche. Das heißt, ein Einbeziehen von Begriffen wie „VM“ würde nur wenige Vorteile bieten, wogegen „Azure Search“ sehr viel spezieller wäre.
Nachstehend sind die wichtigsten zu berücksichtigenden Felder aufgeführt.

 
|  Feld                   | Wichtigkeit | Anleitungen                                                                                            |
|  --------------------    | ----------                   | ---------------                                                                   |
| Angebotsname               |  Hoch      | Eine genaue oder nahezu vollständige Übereinstimmung mit der Suchabfrage ergibt ein hohe Platzierung.                       |
| Name des Herausgebers           |  Hoch      | Eine genaue oder nahezu vollständige Übereinstimmung mit der Suchabfrage ergibt ein hohe Platzierung.                       |
| Kurze Beschreibung        |  Mittel    | Ein Angeben der Benennung von Apps und Herausgebern garantiert meist eine hohe Platzierung, ist aber möglicherweise nicht der relevanteste Aspekt. In diesem Fall ist eine kurze Beschreibung entscheidend. Der Text sollte so möglichst prägnant und präzise sein. Zum Erzielen bester Ergebnisse sollten Schlüsselwörter und erwartete Suchbegriffe einbezogen werden.  Zum Beispiel ist „Dies ist das beste Einzelhandels-KS, das vollständig auf Dynamics 365 aufsetzt“ weniger effektiv als „Einzelhandels-KS (Kassensystem) für Dynamics 365“.  | 
| Lange Beschreibung         |  Niedrig       | Die Beschreibung bietet eine Möglichkeit, mehr in die Tiefe zu gehen. Am effektivsten ist es, Beschreibungen mit vernünftiger Länge sowie Schlüsselwörter zu verwenden.  Eine punktgenaue Beschreibung, in der Schlüsselwörter verwendet werden, bringt mehr Vorteile als ein langatmiger Text. Achten Sie darauf, dass Schlüsselbegriffe, z. B. „IoT“, in der Beschreibung enthalten sind.  |
| Produktkategorien       | Mittel     |  Produktkategorien werden durch eine Kombination aus Herausgeberauswahl und Microsoft-Klassifizierung bestimmt. Sie sollten die jeweils geeigneten Kategorien auswählen, damit Benutzer die Apps problemlos in der richtigen Kategorie finden können. |
|  |  |  |


## <a name="other-tips"></a>Weitere Tipps

-   Suchvorschläge führen zu hoher Benutzeraktivität. Dabei werden Übereinstimmungen mit App-Name/Herausgeber priorisiert. „Kurze Beschreibung“ wird das Schlüsselfeld, wenn der Suchbegriff nicht genau mit dem Herausgeber-/App-Namen übereinstimmt.
-   Dokumente zum Herunterladen werden nicht in die Suchgewichtung einbezogen.
-   Der tatsächliche Erwerb und die tatsächliche Nutzung Ihrer App wirkt sich ebenfalls auf die Suchplatzierung aus. Bei zwei gleichwertigen Apps erhält beispielsweise die App, die erheblich mehr Benutzer hat, eine höhere Platzierung.
