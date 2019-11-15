---
title: 'Designer: Klassifizieren, Vorhersage von Flugverspätungen'
titleSuffix: Azure Machine Learning
description: In diesem Artikel wird veranschaulicht, wie Sie ein Machine Learning-Modell für die Vorhersage von Flugverspätungen mit dem Drag & Drop-Designer und benutzerdefiniertem R-Code erstellen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: xiaoharper
ms.author: zhanxia
ms.reviewer: peterlu
ms.date: 11/04/2019
ms.openlocfilehash: 10b3b78b4878eeddf2f786fda90ab9709e4bd2b4
ms.sourcegitcommit: 359930a9387dd3d15d39abd97ad2b8cb69b8c18b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73647197"
---
# <a name="sample-6---classification-predict-flight-delays-using-r"></a>Beispiel 6 – Klassifizierung: Vorhersage von Flugverspätungen mit R-Code
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-enterprise-sku.md)]

Diese Pipeline verwendet historische Flug- und Wetterdaten, um vorherzusagen, ob sich ein Linienflug um mehr als 15 Minuten verspäten wird. Dieses Problem kann als Klassifizierungsproblem angegangen werden, indem zwei Klassen vorhergesagt werden: verzögert oder pünktlich.

So sieht der endgültige Graph der Pipeline für dieses Beispiel aus:

[![Graph der Pipeline](media/how-to-ui-sample-classification-predict-flight-delay/pipeline-graph.png)](media/how-to-ui-sample-classification-predict-credit-risk-cost-sensitive/graph.png#lightbox)

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [aml-ui-prereq](../../../includes/aml-ui-prereq.md)]

4. Klicken Sie auf Beispiel 6, um es zu öffnen.

## <a name="get-the-data"></a>Abrufen von Daten

Dieses Beispiel verwendet das Dataset **Flight Delays Data**. Es gehört zur TranStats-Datensammlung des US-Verkehrsministeriums. Das Dataset enthält Informationen zu Flugverspätungen von April bis Oktober 2013. Das Dataset wurde wie folgt vorverarbeitet:

* Sie wurden gefiltert, um die 70 verkehrsreichsten Flughäfen in den Vereinigten Staaten zu enthalten.
* Ausgefallene Flüge wurden als um mehr als 15 Minuten verspätet gekennzeichnet.
* Umgeleitete Flüge wurden herausgefiltert.
* Es wurden 14 Spalten ausgewählt.

Zur Ergänzung der Flugdaten wurde das **Dataset „Weather“** verwendet. Die Wetterdaten enthalten stündliche flächenbasierte Wetterbeobachtungen der NOAA und stellen Beobachtungen von Wetterstationen auf Flughäfen für den gleichen Zeitraum dar, den auch das Dataset mit den Flugdaten umfasst. Die Daten wurden wie folgt vorverarbeitet:

* Die IDs der Wetterstationen wurden den entsprechenden Flughafen-IDs zugeordnet.
* Wetterstationen, die nicht zu den 70 verkehrsreichsten Flughäfen gehören, wurden entfernt.
* Die Datumsspalte wurde in separate Spalten aufgeteilt: „Year“, „Month“ und „Day“.
* Es wurden 26 Spalten ausgewählt.

## <a name="pre-process-the-data"></a>Vorverarbeiten der Daten

Datasets müssen vor der Analyse normalerweise vorverarbeitet werden.

![Datenverarbeitung](media/how-to-ui-sample-classification-predict-flight-delay/data-process.png)

### <a name="flight-data"></a>Flugdaten

Die Spalten**Carrier**, **OriginAirportID** und **DestAirportID** werden als ganze Zahlen gespeichert. Allerdings sind sie kategorische Attribute. Verwenden Sie das Modul **Edit Metadata**, um sie in kategorische Werte umzuwandeln.

![edit-metadata](media/how-to-ui-sample-classification-predict-flight-delay/edit-metadata.png)

Verwenden Sie dann das Modul **Select Columns** im Dataset, um aus dem Dataset Spalten auszuschließen, die mögliche Zielleaker sind: **DepDelay**, **DepDel15**, **ArrDelay**, **Canceled**, **Year**. 

Um die Flugdatensätze mit den stündlichen Wetterdatensätzen zu verknüpfen, verwenden Sie die geplante Abflugzeit als einen der Verknüpfungsschlüssel. Um die Verknüpfung durchzuführen, muss die Spalte CSRDepTime auf die nächste Stunde abgerundet werden. Dies geschieht im Modul **Execute R Script**. 

### <a name="weather-data"></a>Wetterdaten

Spalten, in denen ein großer Teil der Werte fehlt, werden mit dem Modul **Project Columns** ausgeschlossen. Diese Spalten enthalten alle Spalten mit einem Zeichenfolgenwert: **ValueForWindCharacter**, **WetBulbFarenheit**, **WetBulbCelsius**, **PressureTendency**, **PressureChange**, **SeaLevelPressure** und **StationPressure**.

Das Modul **Clean Missing Data** wird dann für die restlichen Spalten angewendet, um Zeilen mit fehlenden Daten zu entfernen.

Die Wetterbeobachtungszeiten werden auf die nächste volle Stunde aufgerundet. Die geplanten Flugzeiten und die Wetterbeobachtungszeiten werden gegenläufig gerundet, um sicherzustellen, dass das Modell nur das Wetter vor der Flugzeit verwendet. 

Da die Wetterdaten in der Ortszeit angegeben sind, werden Zeitzonenunterschiede durch Subtraktion der Zeitzonenspalten von der geplanten Abfahrtszeit und der Wetterbeobachtungszeit berücksichtigt. Dies erfolgt mithilfe des Moduls **Execute R Script**.

### <a name="joining-datasets"></a>Verknüpfen von Datasets

Die Flugdatensätze werden mit den Wetterdaten am Ausgangspunkt des Fluges (**OriginAirportID**) über das Modul **Join Data** verknüpft.

 ![Verknüpfen von Flug- und Wetterdaten nach Ursprung](media/how-to-ui-sample-classification-predict-flight-delay/join-origin.png)


Flugdatensätze werden mit Wetterdaten anhand des Flugziels verknüpft (**DestAirportID**).

 ![Verknüpfen von Flug- und Wetterdaten nach Ziel](media/how-to-ui-sample-classification-predict-flight-delay/join-destination.png)

### <a name="preparing-training-and-test-samples"></a>Vorbereiten der Trainings- und Testbeispiele

Das Modul **Split Data** trennt die Daten in die Datensätze von April bis September für das Training und von Oktober für den Test.

 ![Trennen von Trainings- und Testdaten](media/how-to-ui-sample-classification-predict-flight-delay/split.png)

Die Spalten „Year“, „Month“ und „Timezone“ werden anhand des Moduls „Select Columns“ aus dem Trainingsdataset entfernt.

## <a name="define-features"></a>Definieren der Funktionen

Bei Machine Learning sind Funktionen einzeln messbare Eigenschaften des untersuchten Gesamtobjekts. Um leistungsstarke Funktionen zu finden, sind Experimente und Domänenkenntnisse erforderlich. Manche Funktionen eignen sich besser für die Vorhersage des Ziels als andere. Außerdem können einige Funktionen eine starke Korrelation mit anderen Funktionen aufweisen und dem Modell daher keine neuen Informationen hinzufügen. Diese Funktionen können entfernt werden.

Um ein Modell zu erstellen, können Sie alle verfügbaren Funktionen verwenden oder eine Teilmenge der Funktionen auswählen.

## <a name="choose-and-apply-a-learning-algorithm"></a>Auswählen und Anwenden eines Lernalgorithmus

Erstellen Sie mit dem Modul **Two-Class Logistic Regression** ein Modell, und trainieren Sie es mit den Trainingsdaten. 

Als Ergebnis des Moduls **Train Model** erhalten Sie ein trainiertes Klassifizierungsmodell, mit dem Sie neue Proben bewerten können, um Vorhersagen zu machen. Verwenden Sie das Testdataset zum Generieren von Bewertungen aus den trainierten Modellen. Verwenden Sie dann das Modul **Evaluate Model** zum Analysieren und Vergleichen der Qualität der Modelle.
Nach dem Ausführen der Pipeline können Sie die Ausgabe des Moduls **Score Model** anzeigen. Klicken Sie hierzu auf den Ausgabeport, und wählen Sie **Visualisieren** aus. Die Ausgabe enthält die bewerteten Bezeichnungen und deren Wahrscheinlichkeiten.

Um schließlich die Qualität der Ergebnisse zu überprüfen, ziehen Sie das Modul **Evaluate Model** in den Pipelinebereich, und verbinden Sie den linken Eingangsport mit der Ausgabe des Moduls „Score Model“. Führen Sie die Pipeline aus, und zeigen Sie die Ausgabe des Moduls **Evaluate Model** an. Klicken Sie hierzu auf den Ausgabeport, und wählen Sie **Visualisieren** aus.

## <a name="evaluate"></a>Evaluate
Das logistische Regressionsmodell verwendet für den Testsatz einen AUC-Wert von 0.631.

 ![Evaluieren](media/how-to-ui-sample-classification-predict-flight-delay/evaluate.png)

## <a name="next-steps"></a>Nächste Schritte

Erkunden Sie die anderen Beispiele, die für den Designer zur Verfügung stehen:

- [Beispiel 1 – Regression: Vorhersagen des Preises eines Autos](how-to-designer-sample-regression-automobile-price-basic.md)
- [Beispiel 2 – Regression: Vergleichen von Algorithmen für die Vorhersage von Autopreisen](how-to-designer-sample-regression-automobile-price-compare-algorithms.md)
- [Beispiel 3 – Klassifizierung mit Featureauswahl: Vorhersage des Einkommens](how-to-designer-sample-classification-predict-income.md)
- [Beispiel 4 – Klassifizierung: Vorhersagen des Kreditrisikos (kostensensibel)](how-to-designer-sample-classification-credit-risk-cost-sensitive.md)
- [Beispiel 5 – Klassifizierung: Vorhersage der Kundenabwanderung](how-to-designer-sample-classification-churn.md)
- [Beispiel 7 – Textklassifizierung: Wikipedia SP 500-Dataset](how-to-designer-sample-text-classification.md)