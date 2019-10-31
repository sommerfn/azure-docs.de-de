---
title: 'Grafische Benutzeroberfläche: Beispiel Nr. 3: Regression zum Vorhersagen des Preises und Vergleichen von Algorithmen'
titleSuffix: Azure Machine Learning
description: In diesem Artikel wird gezeigt, wie Sie über die grafische Benutzeroberfläche eine komplexe Machine Learning-Pipeline erstellen, ohne eine einzige Codezeile zu schreiben. Erfahren Sie, wie Sie mehrere Regressionsmodelle trainieren und vergleichen, um den Preis eines Fahrzeugs anhand technischer Merkmale vorherzusagen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: xiaoharper
ms.author: zhanxia
ms.reviewer: sgilley
ms.date: 05/10/2019
ms.openlocfilehash: 651644eaae910792aac2144531d09afc4cde7153
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72692784"
---
# <a name="sample-2---regression-predict-price-and-compare-algorithms"></a>Beispiel 2 – Regression: Vorhersagen des Preises und Vergleichen von Algorithmen

Hier erfahren Sie, wie Sie über die grafische Benutzeroberfläche eine komplexe Machine Learning-Pipeline erstellen, ohne eine einzige Codezeile zu schreiben. In diesem Beispiel werden mehrere Regressionsmodelle trainiert und verglichen, um den Preis eines Fahrzeugs anhand seiner technischen Merkmale vorherzusagen. Wir stellen die Gründe für die Entscheidungen bereit, die in dieser Pipeline getroffen wurden, damit Sie Ihre eigenen Probleme durch maschinelles Lernen lösen können.

Wenn Sie noch keine Erfahrung mit maschinellem Lernen haben, sehen Sie sich zunächst die [Basisversion](how-to-ui-sample-regression-predict-automobile-price-basic.md) dieser Pipeline an.

Der fertige Graph für diese Pipeline sieht wie folgt aus:

[![Graph der Pipeline](media/how-to-ui-sample-regression-predict-automobile-price-compare-algorithms/graph.png)](media/how-to-ui-sample-classification-predict-credit-risk-cost-sensitive/graph.png#lightbox)

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [aml-ui-prereq](../../../includes/aml-ui-prereq.md)]

4. Wählen Sie die Schaltfläche **Öffnen** für die zweite Beispielpipeline aus:

    ![Öffnen der Pipeline](media/how-to-ui-sample-regression-predict-automobile-price-compare-algorithms/open-sample2.png)

## <a name="pipeline-summary"></a>Pipelineübersicht

Führen Sie die folgenden Schritte aus, um die Machine Learning-Pipeline zu erstellen:

1. Abrufen der Daten.
1. Vorverarbeiten der Daten.
1. Trainieren des Modells.
1. Testen, Auswerten und Vergleichen der Modelle.

## <a name="get-the-data"></a>Abrufen von Daten

In diesem Beispiel wird das Dataset **Automobile Price Data (Raw)** (Automobilpreisdaten (Rohdaten)) aus dem UCI Machine Learning-Repository verwendet. Dieses Dataset enthält 26 Spalten mit Informationen zu Automobilen, darunter Marke, Modell, Preis, Fahrzeugmerkmale (etwa die Anzahl der Zylinder), Treibstoffverbrauch und eine Versicherungsrisikobewertung.

## <a name="pre-process-the-data"></a>Vorverarbeiten der Daten

Zu den Hauptaufgaben der Datenaufbereitung gehören die Datenbereinigung, Integration, Transformation, Reduktion und Diskretisierung oder Quantisierung. In der grafischen Benutzeroberfläche finden Sie Module zur Durchführung dieser Vorgänge und anderer Aufgaben der Datenvorverarbeitung in der Gruppe **Data Transformation** (Datentransformation) im linken Bereich.

Verwenden Sie das Modul **Select Columns in Dataset** (Spalten in Dataset auswählen), um normalisierte Verluste auszuschließen, die viele fehlende Werte aufweisen. Anschließend verwenden wir **Clean Missing Data** (Fehlende Daten bereinigen), um die Zeilen zu entfernen, die fehlende Werte aufweisen. Dies hilft, einen bereinigten Satz von Trainingsdaten zu erstellen.

![Datenvorverarbeitung](media/how-to-ui-sample-regression-predict-automobile-price-compare-algorithms/data-processing.png)

## <a name="train-the-model"></a>Modelltraining

Machine Learning-Probleme sind vielfältig. Machine Learning-Aufgaben sind z.B. Klassifizierung, Clustering, Regression und Empfehlungssysteme, die möglicherweise jeweils einen anderen Algorithmus erfordern. Die Auswahl des Algorithmus hängt häufig von den Anforderungen des Anwendungsfalls ab. Nachdem Sie einen Algorithmus ausgewählt haben, müssen Sie seine Parameter optimieren, um ein genaueres Modell zu trainieren. Sie müssen alle Modelle basierend auf Metriken wie Genauigkeit, Verständlichkeit und Effizienz auswerten.

Da das Ziel dieser Pipeline darin besteht, Automobilpreise vorherzusagen, und die Bezeichnungsspalte (Preis) reelle Zahlen enthält, ist ein Regressionsmodell eine gute Wahl. In Anbetracht der Tatsache, dass die Anzahl der Features relativ klein ist (weniger als 100) und diese Features nicht spärlich sind, ist es wahrscheinlich, dass die Entscheidungsgrenze nichtlinear ist.

Um die Leistung verschiedener Algorithmen zu vergleichen, verwenden wir zwei nichtlineare Algorithmen (**Boosted Decision Tree Regression** und **Decision Forest Regression**), um Modelle zu erstellen. Beide Algorithmen haben Parameter, die Sie ändern können, aber in diesem Beispiel werden die Standardwerte für diese Pipeline verwendet.

Verwenden Sie das Modul **Split Data** (Daten teilen), um nach dem Zufallsprinzip die eingegebenen Daten so aufzuteilen, dass das Trainingsdataset 70 % der ursprünglichen Daten enthält und das Testdataset 30 % der ursprünglichen Daten.

## <a name="test-evaluate-and-compare-the-models"></a>Testen, Auswerten und Vergleichen der Modelle

Sie verwenden zwei verschiedene Sätze von zufällig ausgewählten Daten zum Trainieren und anschließenden Testen des Modells, wie im vorherigen Abschnitt beschrieben. Teilen Sie das Dataset auf, und verwenden Sie unterschiedliche Datasets zum Trainieren und Testen des Modells, um die Auswertung des Modells objektiver zu gestalten.

Nachdem das Modell trainiert wurde, verwenden Sie die Module **Score Model** (Modell bewerten) und **Evaluate Model** (Modell auswerten), um die vorhergesagten Ergebnisse zu generieren und die Modelle auszuwerten. **Score Model** generiert Vorhersagen für das Testdataset mithilfe des trainierten Modells. Übergeben Sie die Bewertungen dann an **Evaluate Model**, um Auswertungsmetriken zu generieren.

In dieser Pipeline werden zwei Instanzen von **Evaluate Model** (Modell auswerten) verwendet, um zwei Modellpaare zu vergleichen.

Im ersten Vergleich vergleichen Sie zwei Algorithmen für das Trainingsdataset.
Im zweiten Vergleich vergleichen Sie zwei Algorithmen für das Testdataset.

Hier sehen Sie die Ergebnisse:

![Vergleichen der Ergebnisse](media/how-to-ui-sample-regression-predict-automobile-price-compare-algorithms/result.png)

Diese Ergebnisse zeigen, dass das Modell, das mit **Boosted Decision Tree Regression** erstellt wurde, einen geringeren mittleren quadratischen Fehler als das Modell aufweist, das mit **Decision Forest Regression** erstellt wurde.

Beide Algorithmen weisen einen geringeren Fehlerwert für das Trainingsdataset als für das unsichtbare Testdataset auf.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte

Untersuchen Sie die anderen Beispiele, die für die grafische Benutzeroberfläche zur Verfügung stehen:

- [Beispiel 1 – Regression: Vorhersagen des Preises eines Autos](how-to-ui-sample-regression-predict-automobile-price-basic.md)
- [Beispiel 3 – Klassifizierung: Vorhersagen des Kreditrisikos](how-to-ui-sample-classification-predict-credit-risk-basic.md)
- [Beispiel 4 – Klassifizierung: Vorhersagen des Kreditrisikos (kostensensibel)](how-to-ui-sample-classification-predict-credit-risk-cost-sensitive.md)
- [Beispiel 5 – Klassifizierung: Vorhersage der Kundenabwanderung](how-to-ui-sample-classification-predict-churn.md)
- [Beispiel 6 – Klassifizierung: Vorhersage von Flugverspätungen](how-to-ui-sample-classification-predict-flight-delay.md)
- [Beispiel 7 – Textklassifizierung: Buchrezensionen](how-to-ui-sample-text-classification.md)
