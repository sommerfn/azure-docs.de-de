---
title: 'Algorithmen und Module: Referenz'
titleSuffix: Azure Machine Learning service
description: Erfahren Sie mehr über die Module, die in der grafischen Benutzeroberfläche von Azure Machine Learning verfügbar sind.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ms.openlocfilehash: 2d81c407c17c50c34afd15a99d2f8ac2f8c5361e
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72692840"
---
# <a name="algorithm--module-reference-overview"></a>Algorithmen und Module: Referenzübersicht

Dieser Referenzinhalt bietet den technischen Hintergrund zu den einzelnen Machine Learning-Algorithmen und -Modulen, die in der grafischen Benutzeroberfläche (Vorschau) von Azure Machine Learning Service verfügbar sind.

Jedes Modul stellt eine Codegruppe dar, die unabhängig ausgeführt werden und eine Machine Learning-Aufgabe ausführen kann, wenn die erforderlichen Eingaben erfolgen. Die Module enthalten möglicherweise bestimmte Algorithmen oder führen Aufgaben aus, die beim Machine Learning wichtig sind, z.B. Ersatz eines fehlenden Werts oder statistische Analyse.

> [!TIP]
> In allen Pipelines auf der grafischen Benutzeroberfläche können Sie Informationen zu einem bestimmten Modul abrufen. Wählen Sie das Modul und dann den **Weitere Hilfe**-Link im Bereich **Direkthilfe** aus.

## <a name="modules"></a>Module

Module sind nach Funktionalität organisiert:

| Funktionalität | BESCHREIBUNG | Modul |
| --- |--- | ---- |
| Datenformatkonvertierungen | Konvertieren von Daten in verschiedene, beim maschinellen Lernen verwendete Dateiformate | [Konvertieren in CSV](convert-to-csv.md) |
| Dateneingabe und -ausgabe | Verschieben Sie Daten aus Cloudquellen in Ihr Experiment. Sie können Ihre Ergebnisse oder Zwischendaten während der Ausführung eines Experiments in Azure Storage, eine SQL-Datenbank oder Hive schreiben oder Cloudspeicher zum Datenaustausch zwischen Experimenten verwenden.  | [Daten importieren](import-data.md)<br/>[Daten exportieren](export-data.md)<br/>[Manuelles Eingeben von Daten](enter-data-manually.md) |
| Datentransformation | Vorgänge für Daten, die für maschinelles Lernen typisch sind, z. B. Normalisieren oder Quantisierung von Daten, Featureauswahl und Verringerung der Dimensionalität.| [Auswählen von Spalten im Dataset](select-columns-in-dataset.md) <br/> [Bearbeiten von Metadaten](edit-metadata.md) <br/> [Bereinigen fehlender Daten](clean-missing-data.md) <br/>  [Feature Hashing](feature-hashing.md) <br/>  [Extrahieren von N-Gramm-Funktionen aus Text](extract-n-gram-features-from-text.md) <br/> [Hinzufügen von Spalten](add-columns.md) <br/> [Hinzufügen von Zeilen](add-rows.md) <br/> [Entfernen doppelter Zeilen](remove-duplicate-rows.md) <br/> [Vorverarbeiten von Text](preprocess-text.md) <br/> [Verknüpfen von Daten](join-data.md) <br/> [Aufteilen von Daten](split-data.md) <br/> [Normalisieren von Daten](normalize-data.md) <br/> [Partition und Beispiel](partition-and-sample.md) <br/> [Beschneiden von Werten](clip-values.md) <br/> [Anwenden der SQL-Transformation](apply-sql-transformation.md)|
| Python- und R-Module | Schreiben Sie Code, und betten Sie ihn in ein Modul ein, um Python und R in Ihr Experiment zu integrieren. | [Ausführen von Python-Skripts](execute-python-script.md)   <br/> [Erstellen eines Python-Modells](create-python-model.md) <br/> [Ausführen von R-Skripts](execute-r-script.md)
|  | **Machine Learning-Algorithmen**: | |
| Classification | Sagen Sie eine Klasse vorher.  Wählen Sie aus Binäralgorithmen (zwei Klassen) oder Multiklassenalgorithmen.| [Entscheidungswald mit mehreren Klassen](multiclass-decision-forest.md) <br/> [Verstärkte Entscheidungsstruktur mit mehreren Klassen](multiclass-boosted-decision-tree.md) <br/> [Logistische Regression mit mehreren Klassen](multiclass-logistic-regression.md)  <br/> [Mehrklassiges neuronales Netzwerk](multiclass-neural-network.md)  <br/>  [Logistische Regression mit zwei Klassen](two-class-logistic-regression.md)  <br/>[Gemitteltes Perzeptron mit zwei Klassen](two-class-averaged-perceptron.md) <br/> [Um zwei&nbsp;Klassen&nbsp;verstärkte&nbsp;Entscheidungsstruktur](two-class-boosted-decision-tree.md)  <br/> [Entscheidungswald mit zwei Klassen](two-class-decision-forest.md)  <br/> [Zweiklassiges neuronales Netzwerk](two-class-neural-network.md)  <br/> [Zweiklassige&nbsp;Support&nbsp;Vector&nbsp;Machine](two-class-support-vector-machine.md) 
| Clustering | Gruppieren Sie Daten.| [K-Means-Clustering](k-means-clustering.md)
| Regression | Sagen Sie einen Wert vorher. | [Lineare Regression](linear-regression.md)  <br/> [Regression mit neuronalen Netzwerken](neural-network-regression.md)  <br/> [Entscheidungswaldregression](decision-forest-regression.md)  <br/> [Regression&nbsp;bei&nbsp;verstärkter&nbsp;Entscheidungsstruktur](boosted-decision-tree-regression.md)
|  | **Erstellen und Bewerten von Modellen**: | |
| Trainieren   | Führen Sie Daten über den Algorithmus aus. | [Train Model](train-model.md) (Modell trainieren)  <br/> [Trainieren des Clusteringmodells](train-clustering-model.md)    |
| Bewerten eines Modells | Bewerten Sie die Genauigkeit des trainierten Modells |  [Auswertungsmodell](evaluate-model.md)
| Punkte | Rufen Sie Vorhersagen aus dem Modell ab, das Sie gerade trainiert haben. | [Anwenden der Transformation](apply-transformation.md)<br/>[Zuweisen von&nbsp;Daten&nbsp;zu&nbsp;Clustern](assign-data-to-clusters.md) <br/>[Score Model](score-model.md) (Modell bewerten)
| Statistische Funktionen | Stellen eine Vielzahl von numerischen Methoden bereit, die mit Data Science verknüpft sind. | [Anwenden einer mathematischen Operation](apply-math-operation.md) <br/> [Zusammenfassen von Daten](summarize-data.md)|

## <a name="error-messages"></a>Fehlermeldungen

Erfahren Sie mehr über [Fehlermeldungen und Ausnahmecodes](machine-learning-module-error-codes.md), die bei der Verwendung von Modulen auf der grafischen Benutzeroberfläche in Azure Machine Learning Service auftreten können.
