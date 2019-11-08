---
title: 'Machine Learning: Spickzettel für Algorithmen'
titleSuffix: Azure Machine Learning
description: Eine druckbare Version des Spickzettels für Machine Learning-Algorithmen, mit dem Sie den richtigen Algorithmus für Ihr Vorhersagemodell im Azure Machine Learning-Designer auswählen können.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: FrancescaLazzeri
ms.author: lazzeri
ms.date: 11/04/2019
ms.openlocfilehash: 10714e155bebdd7acc5d7febb345c17fa81bab13
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73511347"
---
# <a name="machine-learning-algorithm-cheat-sheet-for-azure-machine-learning-designer"></a>Spickzettel mit Machine Learning-Algorithmen für Azure Machine Learning-Designer

Mithilfe des **Cheat Sheet für Azure Machine Learning-Algorithmen** können Sie den richtigen Algorithmus für ein Predictive Analytics-Modell wählen.

Azure Machine Learning bietet eine umfangreiche Bibliothek von Algorithmen der Typen ***Klassifizierung***, ***Empfehlungssystem***, ***Clustering***, ***Anomalieerkennung***, ***Regression*** und ***Textanalyse***. Jede ist speziell auf eine andere Art von Machine Learning-Problem ausgelegt.

## <a name="download-machine-learning-algorithm-cheat-sheet"></a>Download: Spickzettel für Machine Learning-Algorithmen

**Laden Sie das Cheat Sheet hier herunter: [Machine Learning – Cheat Sheet für Algorithmen (11x17 Zoll)](https://download.microsoft.com/download/3/5/b/35bb997f-a8c7-485d-8c56-19444dafd757/azure-machine-learning-algorithm-cheat-sheet-nov2019.pdf?WT.mc_id=docs-article-lazzeri)**

![Spickzettel für Machine Learning-Algorithmen: Erfahren Sie, wie ein Machine Learning-Algorithmus ausgewählt wird.](./media/algorithm-cheat-sheet/machine-learning-algorithm-cheat-sheet.svg)

Sie können das Cheat Sheet für Machine Learning-Algorithmen im Kleinformat herunterladen und drucken, um praktische Hilfe bei der Wahl eines Algorithmus zu erhalten.

## <a name="how-to-use-the-machine-learning-algorithm-cheat-sheet"></a>Verwenden des Spickzettels für Machine Learning-Algorithmen

Die Vorschläge in diesem Cheat Sheet für Algorithmen stellen nur grobe Richtlinien dar. Einige können leicht abgeändert werden und andere sogar stark überarbeitet. Sie dienen lediglich als Ausgangspunkt und Anregung. Sie sollten auch direkte Vergleiche zwischen verschiedenen Algorithmen mit Ihren Daten durchführen. Es gibt keinen Ersatz für das grundsätzliche Verständnis der einzelnen Algorithmen und des Systems, das Ihre Daten generiert hat.

Jeder Algorithmus für maschinelles Lernen hat seinen eigenen Stil oder induktiven Bias. Für ein bestimmtes Problem können verschiedene Algorithmen geeignet sein, aber ein Algorithmus passt möglicherweise besser als andere. Es ist jedoch nicht immer schon im Vorfeld klar, welches der am besten geeignete Algorithmus ist. In diesen Fällen werden im Cheat Sheet mehrere Algorithmen zusammen angegeben. Eine geeignete Strategie bestünde darin, einen Algorithmus zu testen und bei nicht zufriedenstellenden Ergebnissen einen anderen zu versuchen. 

Um mehr über die Algorithmen in Azure Machine Learning zu erfahren, rufen Sie die [Referenz zu Algorithmen und Modulen](algorithm-module-reference/module-reference.md) auf.

## <a name="kinds-of-machine-learning"></a>Arten des maschinellen Lernens

Es gibt drei Arten von maschinellem Lernen: *beaufsichtigtes Lernen*, *unbeaufsichtigtes Lernen* und *vertiefendes Lernen*.

### <a name="supervised-learning"></a>Beaufsichtigtes Lernen

Beim beaufsichtigten Lernen wird jeder Datenpunkt bezeichnet oder einer Kategorie oder einem Wert von Interesse zugeordnet. Ein Beispiel für eine Kategoriebezeichnung ist die Zuordnung eines Bildes zu "Katze" oder "Hund". Ein Beispiel für eine Wertbezeichnung ist der Verkaufspreis für ein gebrauchtes Auto. Ziel beim beaufsichtigten Lernen ist es, viele bezeichnete Beispiele wie diese zu untersuchen und dann Vorhersagen zu zukünftigen Datenpunkten zu treffen. Zum Beispiel, um neue Fotos mit dem richtigen Tier zu identifizieren oder präzise Verkaufspreise für gebrauchte Pkw festzulegen. Dies ist eine häufige und nützliche Verwendung für das maschinelle Lernen.

### <a name="unsupervised-learning"></a>Unbeaufsichtigtes Lernen

Beim unbeaufsichtigten Lernen sind Datenpunkten keine Bezeichnungen zugeordnet. Stattdessen besteht das Ziel von Algorithmen zum unbeaufsichtigten Lernen im Organisieren der Daten in einer bestimmten Form oder in der Beschreibung ihrer Struktur. Dies kann das Gruppieren in Clustern bedeuten, wie bei K-Means, oder das Suchen nach unterschiedlichen Möglichkeiten zur Darstellung komplexer Daten in einfacherer Form.

### <a name="reinforcement-learning"></a>Vertiefendes Lernen

Beim vertiefenden Lernen wählt der Algorithmus eine Aktion als Reaktion auf jeden Datenpunkt aus. Diese Vorgehensweise wird häufig in der Robotik angewendet. Dabei ist der Satz der Sensorenwerte zu einem bestimmten Zeitpunkt ein Datenpunkt, und der Algorithmus muss dann die nächste Aktion des Roboters auswählen. Diese Methode eignet sich auch für Anwendungen im Zusammenhang mit dem Internet der Dinge. Der Lernalgorithmus erhält außerdem kurz danach ein Erfolgssignal, das angibt, wie gut die Entscheidung war. Auf dieser Grundlage ändert der Algorithmus die Strategie zum Erreichen des bestmöglichen Ergebnisses. 

## <a name="next-steps"></a>Nächste Schritte

* [Erfahren Sie mehr zu Studio in Azure Machine Learning und zum Azure-Portal](service/overview-what-is-azure-ml.md).

* In der [Referenz zu Algorithmen und Modulen](algorithm-module-reference/module-reference.md) finden Sie eine Liste mit Algorithmen und Modulen.

* [Tutorial: Erstellen eines Vorhersagemodells im Azure Machine Learning-Designer](service/ui-tutorial-automobile-price-train-score.md).

* [Weitere Informationen zu Deep Learning im Vergleich zum maschinellen Lernen](service/concept-deep-learning-vs-machine-learning.md).
