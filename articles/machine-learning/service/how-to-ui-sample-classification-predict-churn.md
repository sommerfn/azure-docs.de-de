---
title: 'Grafische Benutzeroberfläche: Beispiel Nr. 5: Klassifizierung zum Vorhersagen von Kundenabwanderung, Kauflust und Up-Selling'
titleSuffix: Azure Machine Learning
description: Die Pipeline in diesem Beispiel für die grafische Benutzeroberfläche zeigt eine auf einem binären Klassifizierer basierende Vorhersage der Kundenabwanderung, eine übliche Aufgabe beim Customer Relationship Management (CRM).
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: xiaoharper
ms.author: zhanxia
ms.reviewer: sgilley
ms.date: 09/23/2019
ms.openlocfilehash: 82639779dde08bb1f71fb75dba62038dbf34d1b6
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72693544"
---
# <a name="sample-5---classification-predict-churn-appetency-and-up-selling"></a>Beispiel 5 – Klassifizierung: Vorhersage von Kundenabwanderung, Kauflust und Up-Selling 

Erfahren Sie, wie Sie über die grafische Benutzeroberfläche eine komplexe Pipeline für maschinelles Lernen erstellen, ohne eine einzige Codezeile zu schreiben.

In dieser Pipeline werden drei Klassifizierer des Typs **Verstärkte Entscheidungsstruktur mit zwei Klassen** trainiert, um allgemeine Aufgaben für CRM-Systeme (Customer Relationship Management) vorherzusagen: Kundenabwanderung, Kauflust und Up-Selling. Die Datenwerte und Bezeichnungen sind über mehrere Datenquellen verteilt und verschlüsselt, um Kundeninformationen zu anonymisieren. Trotzdem kann die grafische Benutzeroberfläche weiterhin verwendet werden, um Datasets zu kombinieren und ein Modell mit den unkenntlich gemachten Werten zu trainieren.

Weil Sie versuchen, die Frage „Welche?“ zu beantworten, wird dies als Klassifikationsproblem bezeichnet. Sie können jedoch dieselbe Logik in diesem Beispiel anwenden, um jede Art von Problem für maschinelles Lernen zu bewältigen, sei es Regression, Klassifizierung, Clustering usw.

Das vollständige Diagramm für diese Pipeline sieht wie folgt aus:

![Pipelinegraph](./media/how-to-ui-sample-classification-predict-churn/pipeline-graph.png)

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [aml-ui-prereq](../../../includes/aml-ui-prereq.md)]

4. Wählen Sie die Schaltfläche **Öffnen** für die Pipeline des Beispiels Nr. 5 aus.

    ![Öffnen der Pipeline](media/how-to-ui-sample-classification-predict-churn/open-sample5.png)

## <a name="data"></a>Data

Die Daten für diese Pipeline stammen vom KDD Cup 2009. Die Daten sind in 50.000 Zeilen und 230 Featurespalten (Merkmalspalten) enthalten. Die Aufgabe ist die Vorhersage der Kundenabwanderung, Kauflust und des Up-Sellings für Kunden, die diese Features nutzen. Weitere Informationen zu den Daten und zur Aufgabe finden Sie auf der [KDD-Website](https://www.kdd.org/kdd-cup/view/kdd-cup-2009).

## <a name="pipeline-summary"></a>Pipelineübersicht

Die Pipeline in diesem Beispiel für die grafische Benutzeroberfläche zeigt eine auf einem binären Klassifizierer basierende Vorhersage der Kundenabwanderung, Kauflust und des Up-Sellings, eine übliche Aufgabe beim Customer Relationship Management (CRM).

Zunächst eine einfache Datenverarbeitung.

- Das Dataset mit den Rohdaten enthält viele fehlende Werte. Verwenden Sie das Modul **Clean Missing Data** (Bereinigen fehlender Daten), um die fehlenden Werte durch 0 zu ersetzen.

    ![Bereinigen des Datasets](./media/how-to-ui-sample-classification-predict-churn/cleaned-dataset.png)

- Die Features und die zugehörigen Bezeichnungen für Kundenabwanderung, Kauflust und Up-Selling befinden sich in verschiedenen Datasets. Verwenden Sie das Modul **Add Columns** (Spalten hinzufügen), um die Bezeichnungsspalten an die Featurespalten anzufügen. Die erste Spalte, **Col1**, ist die Bezeichnungsspalte. Die übrigen Spalten, **Var1**, **Var2** usw., sind die Featurespalten.

    ![Hinzufügen des Spaltendatasets](./media/how-to-ui-sample-classification-predict-churn/added-column1.png)

- Wir verwenden das Modul **Split Data** (Daten aufteilen) zum Aufteilen des Datasets in Trainings- und Testdatasets.

- Verwenden Sie dann die binäre Klassifizierung „Boosted Decision Tree“ (Verstärkter Entscheidungsbaum) mit den Standardparametern, um die Vorhersagemodelle zu erstellen. Erstellen Sie ein Modell pro Aufgabe, d. h. jeweils ein Modell zur Vorhersage von Up-Selling, Kauflust und Kundenabwanderung.

## <a name="results"></a>Ergebnisse

Visualisieren Sie die Ausgabe des Moduls **Evaluate Model** (Auswerten des Modells), um die Leistung des Modells für das Testdataset zu ermitteln. Für die Up-Selling-Aufgabe zeigt die ROC-Kurve, dass das Modell besser abschneidet als ein Zufallsmodell. Die Fläche unter der Kurve (AUC) ist 0,857. Beim Schwellenwert 0,5 betragen die Genauigkeit 0,7, die Trefferquote 0,463 und das F1-Maß 0,545.

![Auswertung der Ergebnisse](./media/how-to-ui-sample-classification-predict-churn/evaluate-result.png)

 Sie können den Schieberegler **Threshold** (Schwellenwert) verschieben und prüfen, wie sich die Metriken für die binäre Klassifizierungsaufgabe ändern.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte

Untersuchen Sie die anderen Beispiele, die für die grafische Benutzeroberfläche zur Verfügung stehen:

- [Beispiel 1 – Regression: Vorhersagen des Preises eines Autos](how-to-ui-sample-regression-predict-automobile-price-basic.md)
- [Beispiel 2 – Regression: Vergleichen von Algorithmen für die Vorhersage von Autopreisen](how-to-ui-sample-regression-predict-automobile-price-compare-algorithms.md)
- [Beispiel 3 – Klassifizierung: Vorhersagen des Kreditrisikos](how-to-ui-sample-classification-predict-credit-risk-basic.md)
- [Beispiel 4 – Klassifizierung: Vorhersagen des Kreditrisikos (kostensensibel)](how-to-ui-sample-classification-predict-credit-risk-cost-sensitive.md)
- [Beispiel 6 – Klassifizierung: Vorhersage von Flugverspätungen](how-to-ui-sample-classification-predict-flight-delay.md)
- [Beispiel 7 – Textklassifizierung: Buchrezensionen](how-to-ui-sample-text-classification.md)
