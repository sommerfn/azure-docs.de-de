---
title: 'Grafische Benutzeroberfläche: Beispiel Nr. 3: Klassifizierung zur Vorhersage des Kreditrisikos'
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie über die grafische Benutzeroberfläche eine Machine Learning-Klassifizierung (Klassifizierer) erstellen, ohne eine einzige Codezeile zu schreiben.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: xiaoharper
ms.author: zhanxia
ms.reviewer: sgilley
ms.date: 09/23/2019
ms.openlocfilehash: 4649303b8ee643130b8e254f01bfffbe8ad9eb2b
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72693500"
---
# <a name="sample-3---classification-predict-credit-risk"></a>Beispiel 3 – Klassifizierung: Vorhersagen des Kreditrisikos

Erfahren Sie, wie Sie über die grafische Benutzeroberfläche eine Machine Learning-Klassifizierung (Klassifizierer) erstellen, ohne eine einzige Codezeile zu schreiben. Die Pipeline in diesem Beispiel ist eine **verstärkte Entscheidungsstruktur mit zwei Klassen**, um anhand von Kreditantragsinformationen wie Kredithistorie, Alter und Anzahl der Kreditkarten das Kreditrisiko (hoch oder niedrig) vorherzusagen.

Weil die Frage „Welches Risiko?“ lautet, wird dies als Klassifikationsproblem bezeichnet. Sie können jedoch den gleichen elementaren Prozess anwenden, um jede Art von Problem für maschinelles Lernen zu bewältigen, sei es Regression, Klassifizierung, Clustering usw.

So sieht der endgültige Graph der Pipeline für dieses Beispiel aus:

![Graph der Pipeline](media/how-to-ui-sample-classification-predict-credit-risk-basic/overall-graph.png)

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [aml-ui-prereq](../../../includes/aml-ui-prereq.md)]

4. Wählen Sie die Schaltfläche **Öffnen** für die Pipeline des Beispiels Nr. 3 aus:

    ![Öffnen der Pipeline](media/how-to-ui-sample-classification-predict-credit-risk-basic/open-sample3.png)

## <a name="related-sample"></a>Ähnliches Beispiel

[Beispiel 4 – Klassifizierung: Prognose des Kreditrisikos (kostensensibel)](how-to-ui-sample-classification-predict-credit-risk-cost-sensitive.md) bietet eine erweiterte Pipeline, mit der das gleiche Problem wie bei diesem Beispiel gelöst wird. Es zeigt, wie Sie mit dem Modul **Execute Python Script** (Ausführen eines Python-Skripts) eine *kostensensible* Klassifizierung durchführen und die Leistung zweier binärer Klassifizierungsalgorithmen vergleichen können. Greifen Sie darauf zurück, wenn Sie mehr über das Erstellen von Klassifizierungspipelines erfahren möchten.


## <a name="data"></a>Data

In diesem Beispiel wird das Dataset „German Credit Card“ aus dem UC Irvine-Repository verwendet. Es enthält 1.000 Stichproben mit 20 Features und 1 Bezeichnung. Jede Stichprobe stellt eine Person dar. Die Features sind numerisch und kategorisch. Auf der [UCI-Website](https://archive.ics.uci.edu/ml/datasets/Statlog+%28German+Credit+Data%29) können Sie die Bedeutung der kategorischen Features nachschlagen. Die letzte Spalte ist die Bezeichnung, die das Kreditrisiko angibt und nur zwei mögliche Werte hat: hohes Kreditrisiko = 2 und niedriges Kreditrisiko = 1.

## <a name="pipeline-summary"></a>Pipelineübersicht

Führen Sie die folgenden Schritte aus, um die Pipeline zu erstellen:

1. Ziehen Sie das Modul mit dem Dataset „German Credit Card UCI Data“ in den Pipelinebereich.
1. Fügen Sie das Modul **Edit Metadata** (Metadaten bearbeiten) hinzu, damit wir für jede Spalte aussagekräftige Namen hinzufügen können.
1. Fügen Sie das Modul **Split Data** (Aufteilen von Daten) hinzu, um die Trainings- und Testdatasets zu erstellen. Legen Sie den Anteil der Zeilen im ersten Ausgabedataset auf 0,7 fest. Diese Einstellung bestimmt, dass 70 % der Daten an den linken Port des Moduls und der Rest an den rechten Port ausgegeben werden. Wir verwenden das linke Dataset für das Training und das rechte für Tests.
1. Fügen Sie das Modul **Two-Class Boosted Decision Tree** hinzu, um einen verstärkten Entscheidungsbaum mit zwei Klassen zu initialisieren.
1. Fügen Sie das Modul **Train Model** (Trainieren des Modells) hinzu. Verbinden Sie den Klassifizierer aus dem vorherigen Schritt mit dem linken Eingabeport des Moduls **Train Model**. Fügen Sie das Trainingsdataset (den linken Ausgabeport des Moduls **Split Data**) dem rechten Eingabeport des Moduls **Train Model** hinzu. Das Modul **Train Model** trainiert den Klassifizierer.
1. Fügen Sie das Modul **Score Model** (Bewerten des Modells) hinzu, und verbinden Sie es mit dem Modul **Train Model**. Fügen Sie dann das Testdataset (den rechten Port des Moduls **Split Data**) dem Modul **Score Model** hinzu. **Score Model** nimmt die Vorhersagen vor. Sie können den Ausgabeport auswählen, um die Vorhersagen und die Wahrscheinlichkeiten positiver Klassen anzuzeigen.
1. Fügen Sie das Modul **Evaluate Model** (Auswerten des Modells) hinzu, und verbinden Sie das bewertete Dataset mit seinem linken Eingabeport. Um die Auswertungsergebnisse anzuzeigen, klicken Sie auf den Ausgabeport des Moduls **Evaluate Model**, und wählen Sie **Visualize** (Visualisieren) aus.

## <a name="results"></a>Ergebnisse

![Auswertung der Ergebnisse](media/how-to-ui-sample-classification-predict-credit-risk-basic/evaluate-result.png)

In den Auswertungsergebnissen sehen Sie, dass der AUC-Wert (Area Under Curve, Fläche unter der Kurve) des Modells 0,776 beträgt. Beim Schwellenwert 0,5 betragen die Genauigkeit 0,621, die Trefferquote 0,456 und das F1-Maß 0,526.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte

Untersuchen Sie die anderen Beispiele, die für die grafische Benutzeroberfläche zur Verfügung stehen:

- [Beispiel 1 – Regression: Vorhersagen des Preises eines Autos](how-to-ui-sample-regression-predict-automobile-price-basic.md)
- [Beispiel 2 – Regression: Vergleichen von Algorithmen für die Vorhersage von Autopreisen](how-to-ui-sample-regression-predict-automobile-price-compare-algorithms.md)
- [Beispiel 4 – Klassifizierung: Vorhersagen des Kreditrisikos (kostensensibel)](how-to-ui-sample-classification-predict-credit-risk-cost-sensitive.md)
- [Beispiel 5 – Klassifizierung: Vorhersage der Kundenabwanderung](how-to-ui-sample-classification-predict-churn.md)
- [Beispiel 6 – Klassifizierung: Vorhersage von Flugverspätungen](how-to-ui-sample-classification-predict-flight-delay.md)
- [Beispiel 7 – Textklassifizierung: Buchrezensionen](how-to-ui-sample-text-classification.md)