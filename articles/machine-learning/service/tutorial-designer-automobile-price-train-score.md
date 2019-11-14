---
title: 'Tutorial: Prognostizieren von Automobilpreisen mit dem Designer'
titleSuffix: Azure Machine Learning
description: Hier erfahren Sie, wie Sie ein Machine Learning-Modell mithilfe einer Drag & Drop-Benutzeroberfläche trainieren, bewerten und bereitstellen. Dieses Tutorial ist der erste Teil einer zweiteiligen Reihe über das Prognostizieren von Automobilpreisen mithilfe der linearen Regression.
author: peterclu
ms.author: peterlu
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.date: 11/04/2019
ms.openlocfilehash: 3df1a0430983b52d8a791acabbd03efe19055697
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73721774"
---
# <a name="tutorial-predict-automobile-price-with-the-designer-preview"></a>Tutorial: Prognostizieren von Automobilpreisen mit dem Designer (Vorschau)
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-enterprise-sku.md)]

In diesem zweiteiligen Tutorial erfahren Sie, wie Sie mit dem Designer von Azure Machine Learning eine Predictive Analytics-Lösung entwickeln und bereitstellen, mit der der Preis eines beliebigen Autos prognostiziert werden kann. 

Im ersten Teil richten Sie Ihre Umgebung ein, fügen einer interaktiven Canvas per Drag & Drop Module hinzu und verknüpfen sie zu einer Azure Machine Learning-Pipeline.

Im ersten Teil des Tutorials lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer neuen Pipeline
> * Daten importieren
> * Vorbereiten von Daten
> * Trainieren eines Machine Learning-Modells
> * Auswerten eines Machine Learning-Modells

Im [zweiten Teil](tutorial-designer-automobile-price-deploy.md) des Tutorials erfahren Sie, wie Sie Ihr Vorhersagemodell als Echtzeit-Rückschlussendpunkt bereitstellen, um damit den Preis eines beliebigen Autos basierend auf den von Ihnen an die Lösung gesendeten technischen Spezifikationen zu prognostizieren. 

> [!Note]
>Eine fertige Version dieses Tutorials ist als Beispielpipeline verfügbar.
>
>Diese finden Sie im **Designer in Ihrem Arbeitsbereich**. Wählen Sie im Abschnitt **Neue Pipeline** die Option **Sample 1 - Regression: Automobile Price Prediction(Basic)** (Beispiel 1: Regression: Automobilpreisvorhersage (Standard)) aus.

## <a name="create-a-new-pipeline"></a>Erstellen einer neuen Pipeline

Azure Machine Learning-Pipelines fassen mehrere abhängige Machine Learning- und Datenverarbeitungsschritte in einer einzelnen Ressource zusammen. Pipelines unterstützen Sie bei der Strukturierung, Verwaltung und Wiederverwendung komplexer Machine Learning-Workflows für verschiedene Projekte und Benutzer. Für die Erstellung einer Azure Machine Learning-Pipeline benötigen Sie einen Azure Machine Learning-Arbeitsbereich. In diesem Abschnitt erfahren Sie, wie Sie diese beiden Ressourcen erstellen.

### <a name="create-a-new-workspace"></a>Erstellen eines neuen Arbeitsbereichs

Wenn Sie über einen Azure Machine Learning-Arbeitsbereich mit einer **Enterprise Edition** verfügen, [fahren Sie mit dem nächsten Abschnitt fort](#create-the-pipeline).

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal-enterprise.md)]

### <a name="create-the-pipeline"></a>Erstellen der Pipeline

1. Melden Sie sich bei [ml.azure.com](https://ml.azure.com) an, und wählen Sie den gewünschten Arbeitsbereich aus.

1. Wählen Sie **Designer**aus.

    ![Screenshot des visuellen Arbeitsbereichs: Vorgehensweise für den Zugriff auf den Designer](./media/ui-tutorial-automobile-price-train-score/launch-visual-interface.png)

1. Wählen Sie **Easy-to-use prebuilt modules** (Benutzerfreundliche vorgefertigte Module) aus.

1. Wählen Sie im oberen Bereich der Canvas den Standard-Pipelinenamen **Pipeline-Created-on ...** (Pipeline erstellt am ...) aus, und geben Sie einen aussagekräftigen Namen ein. Beispielsweise **Automobile price prediction** (Automobilpreisvorhersage). Der Name muss nicht eindeutig sein.

## <a name="import-data"></a>Daten importieren

Im Designer stehen mehrere Beispieldatasets zur Verfügung, mit denen Sie experimentieren können. Verwenden Sie für dieses Tutorial **Automobile Price Data (Raw)** (Automobilpreisdaten (Rohdaten)). 

1. Links neben der Pipelinecanvas befindet sich eine Palette mit Datasets und Modulen. Wählen Sie **Datasets** aus, und sehen Sie sich die verfügbaren Beispieldatasets im Abschnitt **Beispiele** an.

1. Wählen Sie das Dataset **Automobile Price Data (Raw)** (Automobilpreisdaten (Rohdaten)) aus, und ziehen Sie es auf die Canvas.

   ![Ziehen von Daten in den Experimentbereich](./media/ui-tutorial-automobile-price-train-score/drag-data.gif)

### <a name="visualize-the-data"></a>Visualisieren der Daten

Sie können die Daten visualisieren, um sich mit dem zu verwendenden Dataset vertraut zu machen.

1. Wählen Sie das Modul **Automobile price data (Raw)** (Automobilpreisdaten (Rohdaten)) aus.

1. Wählen Sie im Bereich **Eigenschaften** rechts neben der Canvas die Option **Ausgaben** aus.

1. Wählen Sie das Diagrammsymbol aus, um die Daten zu visualisieren.

    ![Visualisieren der Daten](./media/ui-tutorial-automobile-price-train-score/visualize-data.png)

1. Wählen Sie die verschiedenen Spalten im Datenfenster aus, um Informationen zur jeweiligen Spalte zu erhalten.

    Jede Zeile steht für ein Fahrzeug, und die Variablen, die den einzelnen Fahrzeugen zugeordnet sind, werden als Spalten angezeigt. Es gibt 205 Zeilen und 26 Spalten in diesem Dataset.

## <a name="prepare-data"></a>Vorbereiten von Daten

Für Datasets ist vor der Analyse in der Regel eine Vorverarbeitung erforderlich. Bei der Untersuchung des Datasets sind Ihnen unter Umständen einige fehlende Werte aufgefallen. Damit das Modell die Daten korrekt analysieren kann, müssen diese fehlenden Werte bereinigt werden.

### <a name="remove-a-column"></a>Entfernen einer Spalte

Wenn Sie ein Modell trainieren, müssen Sie etwas gegen fehlende Daten tun. In diesem Dataset fehlen in der Spalte **normalized-losses** zahlreiche Werte. Daher schließen wir diese Spalte ganz aus dem Modell aus.

1. Geben Sie im oberen Bereich der Palette **Select** (Auswählen) in das Suchfeld ein, um nach dem Modul **Select Columns in Dataset** (Spalten im Dataset auswählen) zu suchen.

1. Klicken Sie auf **Select Columns in Dataset** (Spalten im Dataset auswählen), und ziehen Sie das Modul auf die Canvas. Legen Sie das Modul unter dem Datasetmodul ab.

1. Verbinden Sie das Dataset **Automobile price data (Raw)** (Automobilpreisdaten (Rohdaten)) mit **Select Columns in Dataset** (Spalten in Dataset auswählen). Ziehen Sie eine Linie vom Ausgabeport des Datasets zum Eingabeport von **Select Columns in Dataset** (Spalten im Dataset auswählen) – also von dem kleinen Kreis am unteren Rand des Datasets auf der Canvas zu dem kleinen Kreis am oberen Rand des Moduls.

    > [!TIP]
    > Um einen Datenfluss für Ihre Pipeline zu erstellen, verbinden Sie den Ausgabeport eines Moduls mit dem Eingabeport eines anderen Moduls.
    >

    ![Verbinden von Modulen](./media/ui-tutorial-automobile-price-train-score/connect-modules.gif)

1. Wählen Sie das Modul **Select Columns in Dataset** aus.

1. Wählen Sie im Bereich **Eigenschaften** rechts neben der Canvas **Parameter**  > **Spalte bearbeiten** aus.

1. Wählen Sie **+** aus, um eine neue Regel hinzuzufügen.

1. Wählen Sie im Dropdownmenü **Ausschließen** und **Spaltennamen** aus.
    
1. Geben Sie **normalized-losses** in das Textfeld ein.

1. Wählen Sie rechts unten **Speichern** aus, um die Spaltenauswahl zu schließen.

    ![Ausschließen einer Spalte](./media/ui-tutorial-automobile-price-train-score/exclude-column.png)
        
    Der Eigenschaftenbereich zeigt, dass die Spalte **normalized-losses** ausgeschlossen wurde.

1. Wählen Sie das Modul **Select Columns in Dataset** aus. 

1. Navigieren Sie im Bereich **Eigenschaften** zu **Parameter** > **Kommentar**, und geben Sie „Exclude normalized losses.“ (Normalisierte Verluste ausschließen) ein.

### <a name="clean-missing-data"></a>Fehlende Daten bereinigen

Auch nach dem Entfernen der Spalte **normalized-losses** weist Ihr Dataset noch fehlende Werte auf. Die restlichen fehlenden Daten können mithilfe des Moduls **Clean Missing Data** (Fehlende Daten bereinigen) entfernt werden.

> [!TIP]
> Die Bereinigung fehlender Werte in den Eingabedaten wird bei den meisten Modulen im Designer vorausgesetzt.

1. Geben Sie **Clean** in das Suchfeld ein, um nach dem Modul **Clean Missing Data** (Fehlende Daten bereinigen) zu suchen.

1. Ziehen Sie das Modul **Clean Missing Data** (Fehlende Daten bereinigen) auf die Pipelinecanvas, und verbinden Sie es mit dem Modul **Select Columns in Dataset** (Spalten im Dataset auswählen). 

1. Klicken Sie im Eigenschaftenbereich unter **Cleaning mode** (Bereinigungsmodus) auf die Option **Remove entire row** (Gesamte Zeile entfernen).

1. Geben Sie im Eigenschaftenbereich den Text „Remove missing value rows.“ (Zeilen mit fehlenden Werten entfernen) in das Feld **Kommentar** ein.  

    Ihre Pipeline sollte nun in etwa wie folgt aussehen:
    
    ![select-column](./media/ui-tutorial-automobile-price-train-score/pipeline-clean.png)

## <a name="train-a-machine-learning-model"></a>Trainieren eines Machine Learning-Modells

Nach der Verarbeitung der Daten können Sie ein Vorhersagemodell trainieren.

### <a name="select-an-algorithm"></a>Auswählen eines Algorithmus

**Klassifizierung** und **Regression** sind zwei Algorithmen für beaufsichtigtes maschinelles Lernen. Mit der **Klassifizierung** wird eine Antwort aus einem definierten Satz von Kategorien vorhergesagt, z. B. eine Farbe (Rot, Blau oder Grün). Die **Regression** wird verwendet, um eine Zahl vorherzusagen.

Da Sie einen Preis (also eine Zahl) vorhersagen möchten, können Sie einen Regressionsalgorithmus verwenden. In diesem Beispiel verwenden Sie ein lineares Regressionsmodell.

### <a name="split-the-data"></a>Teilen der Daten

Unterteilen Sie Ihre Daten in zwei getrennte Datasets: eins zum Trainieren des Modells und eins zum Testen.

1. Geben Sie **split data** (Daten aufteilen) in das Suchfeld ein, um nach dem Modul **Split Data** (Daten aufteilen) zu suchen, und verbinden Sie es mit dem linken Port des Moduls **Clean Missing Data** (Fehlende Daten bereinigen).

1. Wählen Sie das Modul **Split Data** (Daten aufteilen) aus.

1. Legen Sie im Eigenschaftenbereich den Wert für **Fraction of rows in the first output dataset** (Anteil der Zeilen im ersten Ausgabedataset) auf „0,7“ fest.

    Dadurch werden 70 Prozent der Daten zum Trainieren des Modells und 30 Prozent zum Testen verwendet.

1. Geben Sie im Eigenschaftenbereich den Text „Split the dataset into training set(0.7) and test set(0.3).“ (Dataset in Trainingssatz (0,7) und Testsatz (0,3) unterteilen) in das Feld **Kommentar** ein.

### <a name="train-the-model"></a>Modelltraining

Trainieren Sie das Modell, indem Sie einen Datensatz mit dem Preis einspeisen. Das Modell überprüft die Daten und sucht nach Korrelationen zwischen den Features eines Autos und seinem Preis, um ein Modell zu konstruieren.

1. Löschen Sie zum Auswählen des Lernalgorithmus das Suchfeld der Modulpalette.

1. Erweitern Sie **Machine Learning-Algorithmen**.
    
    Daraufhin werden verschiedene Kategorien von Modulen angezeigt, die Sie zur Initialisierung von Lernalgorithmen verwenden können.

1. Wählen Sie **Regression** > **Linear Regression** (Lineare Regression) aus, und ziehen Sie sie auf die Pipelinecanvas.

1. Suchen Sie nach dem Modul **Train Model** (Modell trainieren), und ziehen Sie es auf die Pipelinecanvas. 

1. Verbinden Sie die Ausgabe des Moduls **Linear Regression** (Lineare Regression) mit der linken Eingabe des Moduls **Train Model** (Modell trainieren).

1. Verbinden Sie die Trainingsdatenausgabe (linker Port) des Moduls **Split Data** (Daten aufteilen) mit der rechten Eingabe des Moduls **Train Model** (Modell trainieren).

    ![Screenshot der richtigen Konfiguration des Moduls „Train Model“. Das Modul „Linear Regression“ ist mit dem linken Port des Moduls „Train Model“ verbunden, und das Modul „Split Data“ ist mit dem rechten Port von „Train Model“ verbunden.](./media/ui-tutorial-automobile-price-train-score/pipeline-train-model.png)

1. Wählen Sie das Modul **Train Model** (Modell trainieren) aus.

1. Wählen Sie im Eigenschaftenbereich die Option **Spalte bearbeiten** aus.

1. Erweitern Sie im Dialogfeld **Bezeichnungsspalte** das Dropdownmenü, und wählen Sie **Spaltennamen** aus. 

1. Geben Sie **price** (Preis) in das Textfeld ein. Der Preis ist der Wert, den das Modell vorhersagen soll.

    Ihre Pipeline sollte wie folgt aussehen:

    ![Screenshot: Korrekte Pipelinekonfiguration nach dem Hinzufügen des Moduls „Train Model“ (Modell trainieren)](./media/ui-tutorial-automobile-price-train-score/pipeline-train-graph.png)

## <a name="evaluate-a-machine-learning-model"></a>Auswerten eines Machine Learning-Modells

Nachdem Sie das Modell mit 70 Prozent der Daten trainiert haben, können Sie unter Verwendung der restlichen 30 Prozent bewerten, wie gut das Modell funktioniert.

1. Geben Sie **score model** (Modell bewerten) in das Suchfeld ein, um nach dem Modul **Score Model** (Modell bewerten) zu suchen, und ziehen Sie es auf die Pipelinecanvas. 

1. Verbinden Sie die Ausgabe des Moduls **Train Model** mit dem linken Eingabeport des Moduls **Score Model**. Verbinden Sie die Testdatenausgabe (den rechten Port) des Moduls **Split Data** mit dem rechten Eingabeport des Moduls **Score Model**.

1. Geben Sie **evaluate** (auswerten) in das Suchfeld ein, um nach dem Modul **Evaluate Model** (Modell auswerten) zu suchen, und ziehen Sie es auf die Pipelinecanvas. 

1. Verbinden Sie die Ausgabe des Moduls **Score Model** mit der linken Eingabe des Moduls **Evaluate Model**. 

    Die fertige Pipeline sollte in etwa wie folgt aussehen:

    ![Screenshot: Korrekte Konfiguration des der Pipeline](./media/ui-tutorial-automobile-price-train-score/pipeline-final-graph.png)

### <a name="run-the-pipeline"></a>Führen Sie die Pipeline aus.

[!INCLUDE [aml-ui-create-training-compute](../../../includes/aml-ui-create-training-compute.md)]

### <a name="view-results"></a>Anzeigen der Ergebnisse

Nach Abschluss der Ausführung können Sie sich die Ergebnisse ansehen. 

1. Wählen Sie das Modul **Score Model** (Modell bewerten) aus, um dessen Ausgabe anzuzeigen.

1. Wählen Sie im Bereich **Eigenschaften** Folgendes aus: **Ausgaben** > **Visualisieren**.

    Hier sehen Sie die vorhergesagten Preise und die tatsächlichen Preise aus den Testdaten.

    ![Screenshot der Ausgabevisualisierung mit hervorgehobener Spalte „Scored Labels“](./media/ui-tutorial-automobile-price-train-score/score-result.png)

1. Wählen Sie das Modul **Evaluate Model** (Modell auswerten) aus, um dessen Ausgabe anzuzeigen.

1. Wählen Sie im Bereich **Eigenschaften** Folgendes aus: **Ausgabe** > **Visualisieren**.

Die folgenden Statistiken werden für Ihr Modell angezeigt:

* **Mean Absolute Error (MAE)** (Mittlerer absoluter Fehler): Der Mittelwert der absoluten Fehler (ein Fehler ist die Differenz zwischen dem prognostizierten und dem tatsächlichen Wert).
* **Root Mean Squared Error (RMSE)** (Wurzel des mittleren quadratischen Fehlers): Die Quadratwurzel des Durchschnitts des Quadrats der Prognosefehler für das Testdataset.
* **Relative Absolute Error**: Der Mittelwert der absoluten Fehler relativ zur absoluten Differenz zwischen tatsächlichen Werten und dem Durchschnitt aller tatsächlichen Werte.
* **Relative Squared Error**: Der Durchschnitt der quadrierten Fehler relativ zur quadrierten Differenz zwischen tatsächlichen Werten und dem Durchschnitt aller tatsächlichen Werte.
* **Coefficient of Determination**: Dieser auch als „R-Quadrat“ bezeichnete Wert ist eine statistische Kenngröße, die angibt, wie gut ein Modell zu den Daten passt.

Für jede Fehlerstatistik sind kleinere Werte besser. Ein kleinerer Wert gibt an, dass die Vorhersagen näher bei den tatsächlichen Werten liegen. Für den Bestimmungskoeffizienten gilt Folgendes: Je näher der Bestimmungskoeffizient am Wert eins (1,0) liegt, desto besser die Vorhersage.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte

Im ersten Teil dieses Tutorial haben Sie folgende Aufgaben ausgeführt:

* Erstellen einer Pipeline
* Vorbereiten der Daten
* Modelltraining
* Bewerten und Auswerten des Modells

Im zweiten Teil erfahren Sie, wie Sie Ihr Modell als Echtzeitendpunkt bereitstellen.

> [!div class="nextstepaction"]
> [Mit dem Bereitstellen von Modellen fortfahren](tutorial-designer-automobile-price-deploy.md)
