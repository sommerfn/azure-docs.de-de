---
title: 'Algorithmen und Module: Referenz'
description: Erfahren Sie mehr über die Module, die in Azure Machine Learning-Designer (Vorschauversion) verfügbar sind.
titleSuffix: Azure Machine Learning
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 10/22/2019
ms.openlocfilehash: 2da567a8f5ebd0161e41bf5a0aeb83b0d3b4ba4c
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73466069"
---
# <a name="algorithm--module-reference-overview"></a>Algorithmen und Module: Referenzübersicht

Dieser Referenzinhalt bietet den technischen Hintergrund zu den einzelnen Machine Learning-Algorithmen und -Modulen, die in Azure Machine Learning-Designer (Vorschauversion) verfügbar sind.

Jedes Modul stellt eine Codegruppe dar, die unabhängig ausgeführt werden und eine Machine Learning-Aufgabe ausführen kann, wenn die erforderlichen Eingaben erfolgen. Die Module enthalten möglicherweise bestimmte Algorithmen oder führen Aufgaben aus, die beim Machine Learning wichtig sind, z.B. Ersatz eines fehlenden Werts oder statistische Analyse.

> [!TIP]
> In allen Pipelines im Designer können Sie Informationen zu einem bestimmten Modul abrufen. Wählen Sie das Modul und dann den **Weitere Hilfe**-Link im Bereich **Direkthilfe** aus.

## <a name="modules"></a>Module

Module sind nach Funktionalität organisiert:

| Funktionalität | BESCHREIBUNG | Modul |
| --- |--- | ---- |
| Dateneingabe und -ausgabe | Verschieben Sie Daten aus Cloudquellen in Ihre Pipeline. Schreiben Sie Ihre Ergebnisse oder Zwischendaten, während Sie eine Pipeline ausführen, in Azure Storage, in eine SQL-Datenbank oder in Hive, oder verwenden Sie Cloudspeicher, um Daten zwischen Pipelines austauschen.  | [Daten importieren](import-data.md) <br/> [Manuelles Eingeben von Daten](enter-data-manually.md) <br/>[Daten exportieren](export-data.md) |
| Datentransformation | Vorgänge für Daten, die für maschinelles Lernen typisch sind, z. B. Normalisieren oder Quantisierung von Daten, Verringerung der Dimensionalität und Konvertierung von Daten zwischen verschiedenen Dateiformaten.| [Hinzufügen von Spalten](add-columns.md) <br/> [Hinzufügen von Zeilen](add-rows.md) <br/> [Bereinigen fehlender Daten](clean-missing-data.md) <br/> [Konvertieren in CSV](convert-to-csv.md) <br/> [Konvertieren in ein Dataset](convert-to-dataset.md) <br/> [Bearbeiten von Metadaten](edit-metadata.md) <br/> [Verknüpfen von Daten](join-data.md) <br/> [Normalisieren von Daten](normalize-data.md) <br/> [Partition und Beispiel](partition-and-sample.md) <br/> [Entfernen doppelter Zeilen](remove-duplicate-rows.md) <br/> [Auswählen der Spaltentransformation](select-columns-transform.md) <br/> [Auswählen von Spalten im Dataset](select-columns-in-dataset.md) <br/> [SMOTE](smote.md) <br/> [Aufteilen von Daten](split-data.md) |
| Featureauswahl | Wählen Sie eine Teilmenge relevanter, nützlicher Features aus, die beim Erstellen eines analytischen Modells verwendet werden sollen. | [Filterbasierte Featureauswahl](filter-based-feature-selection.md) <br/> [Permutation Feature Importance](permutation-feature-importance.md) |
| Python- und R-Module | Schreiben Sie Code, und betten Sie ihn in ein Modul ein, um Python und R in Ihre Pipeline zu integrieren. | [Erstellen eines Python-Modells](create-python-model.md) <br/> [Ausführen von Python-Skripts](execute-python-script.md)   <br/>  [Ausführen von R-Skripts](execute-r-script.md)
| Textanalyse | Stellen Sie spezielle Berechnungstools zum Arbeiten mit strukturiertem und unstrukturiertem Text bereit. | [Extrahieren von N-Gramm-Funktionen aus Text](extract-n-gram-features-from-text.md) <br/> [Feature Hashing](feature-hashing.md) <br/> [Vorverarbeiten von Text](preprocess-text.md) |
|  | **Machine Learning-Algorithmen**: | |
| Classification | Sagen Sie eine Klasse vorher.  Wählen Sie aus Binäralgorithmen (zwei Klassen) oder Multiklassenalgorithmen.| [Entscheidungswald mit mehreren Klassen](multiclass-decision-forest.md) <br/> [Verstärkte Entscheidungsstruktur mit mehreren Klassen](multiclass-boosted-decision-tree.md) <br/> [Logistische Regression mit mehreren Klassen](multiclass-logistic-regression.md)  <br/> [Mehrklassiges neuronales Netzwerk](multiclass-neural-network.md) <br/> [One-vs- All-Multiklasse](one-vs-all-multiclass.md) <br/>  [Logistische Regression mit zwei Klassen](two-class-logistic-regression.md)  <br/>[Gemitteltes Perzeptron mit zwei Klassen](two-class-averaged-perceptron.md) <br/> [Verstärkter Entscheidungsbaum mit zwei Klassen](two-class-boosted-decision-tree.md)  <br/> [Entscheidungswald mit zwei Klassen](two-class-decision-forest.md)  <br/> [Zweiklassiges neuronales Netzwerk](two-class-neural-network.md) <br/> [Zweiklassige Support Vector Machine](two-class-support-vector-machine.md) | 
| Clustering | Gruppieren Sie Daten.| [K-Means-Clustering](k-means-clustering.md)
| Regression | Sagen Sie einen Wert vorher. | [Regression bei verstärktem Entscheidungsbaum](boosted-decision-tree-regression.md) <br/> [Entscheidungswaldregression](decision-forest-regression.md) <br/> [Lineare Regression](linear-regression.md)  <br/> [Regression mit neuronalen Netzwerken](neural-network-regression.md)  <br/> |
| Empfehlung (Recommender) | Erstellen Sie Empfehlungsmodelle. | [Evaluate Recommender](evaluate-recommender.md) <br/> [Score SVD Recommender](score-svd-recommender.md) <br/> [Train SVD Recommender](train-SVD-recommender.md) |
|  | **Erstellen und Bewerten von Modellen**: | |
| Trainieren   | Führen Sie Daten über den Algorithmus aus. | [Cross Validate Model](cross-validate-model.md) <br/> [Train Model](train-model.md) (Modell trainieren)  <br/> [Trainieren des Clusteringmodells](train-clustering-model.md) <br/>  [Tune Model Hyperparameters](tune-model-hyperparameters.md) |
| Bewerten eines Modells | Bewerten Sie die Genauigkeit des trainierten Modells |  [Auswertungsmodell](evaluate-model.md) |
| Punkte | Rufen Sie Vorhersagen aus dem Modell ab, das Sie gerade trainiert haben. | [Anwenden der Transformation](apply-transformation.md)<br/>[Assign Data to Clusters](assign-data-to-clusters.md) (Zuweisen von Daten zu Clustern) <br/>[Score Model](score-model.md) (Modell bewerten) |
| Statistische Funktionen | Stellen eine Vielzahl von numerischen Methoden bereit, die mit Data Science verknüpft sind. | [Anwenden einer mathematischen Operation](apply-math-operation.md) <br/> [Zusammenfassen von Daten](summarize-data.md)|

## <a name="error-messages"></a>Fehlermeldungen

Erfahren Sie mehr über [Fehlermeldungen und Ausnahmecodes](machine-learning-module-error-codes.md), die bei der Verwendung von Modulen in Azure Machine Learning-Designer auftreten können.
