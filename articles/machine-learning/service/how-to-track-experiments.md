---
title: Protokollieren von ML-Experimenten und -Metriken
titleSuffix: Azure Machine Learning
description: Überwachen Sie Ihre ML-Experimente, und überwachen Sie Ausführungsmetriken, um den Modellerstellungsprozess zu verbessern. Fügen Sie Ihrem Trainingsskript Protokollierung hinzu, und zeigen Sie die protokollierten Ergebnisse einer Ausführung an.  Verwenden Sie „run.log“, „Run.start_logging“ oder „ScriptRunConfig“.
services: machine-learning
author: sdgilley
ms.author: sgilley
ms.reviewer: sgilley
ms.service: machine-learning
ms.subservice: core
ms.workload: data-services
ms.topic: conceptual
ms.date: 09/11/2019
ms.custom: seodec18
ms.openlocfilehash: d8a2c456c725a3170bc940bf17dec6b0c4ad2c3e
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73584523"
---
# <a name="monitor-azure-ml-experiment-runs-and-metrics"></a>Überwachen von Azure ML-Experimentausführungen und -metriken
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

Verbessern Sie den Modellerstellungsvorgang, indem Sie Ihre Experimente nachverfolgen und Ausführungsmetriken überwachen. In diesem Artikel erfahren Sie, wie Sie in Azure Machine Learning Protokollierungscode zu Ihrem Trainingsskript hinzufügen, eine Experimentausführung übermitteln, diese Ausführung überwachen und die Ergebnisse untersuchen.

> [!NOTE]
> Azure Machine Learning kann während des Trainings auch Informationen aus anderen Quellen protokollieren, etwa aus automatisierten Machine Learning-Ausführungen oder aus dem Docker-Container, in dem der Trainingsauftrag ausgeführt wird. Diese Protokolle sind nicht dokumentiert. Wenn Sie Probleme haben und sich an den Microsoft-Support wenden, können diese Protokolle möglicherweise bei der Problembehandlung verwendet werden.

> [!TIP]
> Die Informationen in diesem Dokument sind hauptsächlich für Datenanalysten und Entwickler gedacht, die den Modelltrainingsprozess überwachen möchten. Wenn Sie Administrator sind und sich für die Überwachung der Nutzung und Ereignisse von Azure Machine Learning (etwa Kontingente, abgeschlossene Trainingsausführungen oder abgeschlossene Modellimplementierungen) interessieren, lesen Sie den Artikel [Überwachen von Azure Machine Learning](monitor-azure-machine-learning.md).

## <a name="available-metrics-to-track"></a>Zur Nachverfolgung verfügbare Metriken

Die folgenden Metriken können während des Trainings eines Experiments zu einem Durchlauf hinzugefügt werden. Ausführliche Informationen dazu, was Sie bei einer Ausführung nachverfolgen können, finden Sie in der [Referenzdokumentation zur Run-Klasse](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py).

|type| Python-Funktion | Notizen|
|----|:----|:----|
|Skalarwerte |Funktion:<br>`run.log(name, value, description='')`<br><br>Beispiel:<br>run.log("accuracy", 0.95) |Protokollieren Sie einen Zahlen- oder Zeichenfolgenwert für die Ausführung unter dem jeweiligen Namen. Wenn Sie eine Metrik für eine Ausführung protokollieren, wird diese Metrik in der Ausführungsaufzeichnung des Experiments gespeichert.  Sie können dieselbe Metrik innerhalb einer Ausführung mehrmals protokollieren, wobei das Ergebnis als Vektor dieser Metrik betrachtet wird.|
|Listen|Funktion:<br>`run.log_list(name, value, description='')`<br><br>Beispiel:<br>run.log_list("accuracies", [0.6, 0.7, 0.87]) | Protokollieren Sie eine Liste mit Werten für die Ausführung unter dem jeweiligen Namen.|
|Zeile|Funktion:<br>`run.log_row(name, description=None, **kwargs)`<br>Beispiel:<br>run.log_row("Y over X", x=1, y=0.4) | Mit *log_row* wird eine Metrik mit mehreren Spalten erstellt, wie in kwargs beschrieben. Jeder benannte Parameter erzeugt eine Spalte mit dem angegebenen Wert.  *log_row* kann einmal aufgerufen werden, um ein beliebiges Tupel zu protokollieren, oder mehrmals in einer Schleife, um eine vollständige Tabelle zu erzeugen.|
|Table|Funktion:<br>`run.log_table(name, value, description='')`<br><br>Beispiel:<br>run.log_table("Y over X", {"x":[1, 2, 3], "y":[0.6, 0.7, 0.89]}) | Protokollieren Sie ein Wörterbuchobjekt für die Ausführung unter dem jeweiligen Namen. |
|Bilder|Funktion:<br>`run.log_image(name, path=None, plot=None)`<br><br>Beispiel:<br>`run.log_image("ROC", plt)` | Protokollieren Sie ein Image für die Ausführungsaufzeichnung. Verwenden Sie log_image, um eine Imagedatei oder einen Matplotlib-Plot für die Ausführung zu protokollieren.  Diese Images werden angezeigt und können mit der Ausführungsaufzeichnung verglichen werden.|
|Ausführung kennzeichnen|Funktion:<br>`run.tag(key, value=None)`<br><br>Beispiel:<br>run.tag("selected", "yes") | Kennzeichnen Sie die Ausführung mit einem Zeichenfolgenschlüssel und einem optionalen Zeichenfolgenwert.|
|Datei oder Verzeichnis hochladen|Funktion:<br>`run.upload_file(name, path_or_stream)`<br> <br> Beispiel:<br>run.upload_file("best_model.pkl", "./model.pkl") | Laden Sie eine Datei in die Ausführungsaufzeichnung hoch. Ausführungen erfassen die Datei im angegebenen Ausgabeverzeichnis. Die ist für die meisten Ausführungstypen standardmäßig „./outputs“.  Verwenden Sie upload_file nur, wenn zusätzliche Dateien hochgeladen werden müssen, oder kein Ausgabeverzeichnis angegeben ist. Wir empfehlen, `outputs` zum Namen hinzuzufügen, damit das Hochladen in das Ausgabeverzeichnis erfolgt. Sie können alle Dateien, die dieser Ausführungsaufzeichnung zugeordnet sind, mit `run.get_file_names()` auflisten.|

> [!NOTE]
> Metriken für Skalare, Listen, Zeilen und Tabellen können den Typ „float“, „integer“ oder „string“ haben.

## <a name="choose-a-logging-option"></a>Auswählen einer Protokollierungsoption

Wenn Sie Ihr Experiment nachverfolgen oder überwachen möchten, müssen Sie Code hinzufügen, um die Protokollierung zu starten, wenn Sie die Ausführung übermitteln. Im Folgenden werden Möglichkeiten beschrieben, wie Sie die Übermittlung auslösen können:
* __Run.start_logging__ – Fügen Sie Ihrem Trainingsskript Protokollierungsfunktionen hinzu und starten Sie eine interaktive Protokollierungssitzung im angegebenen Experiment. **Start_logging** erstellt eine interaktive Ausführung für die Verwendung in Szenarien wie z.B. Notebooks. Alle Metriken, die während der Sitzung protokolliert werden, werden der Ausführungsaufzeichnung im Experiment hinzugefügt.
* __ScriptRunConfig__ – Fügen Sie Ihrem Trainingsskript Protokollierungsfunktionen hinzu und laden Sie den gesamten Skriptordner mit der Ausführung.  **ScriptRunConfig** ist eine Klasse für das Einrichten der Konfigurationen für Skriptausführungen. Mit dieser Option können Sie Überwachungscode hinzufügen, um über den Abschluss informiert zu werden oder um ein visuelles Widget zur Überwachung zu erhalten.

## <a name="set-up-the-workspace"></a>Arbeitsbereich einrichten
Bevor Sie die Protokollierung hinzufügen und ein Experiment übermitteln, müssen Sie den Arbeitsbereich einrichten.

1. Laden Sie den Arbeitsbereich. Weitere Informationen zum Einrichten der Arbeitsbereichskonfiguration finden Sie in der [Datei mit der Arbeitsbereichskonfiguration](how-to-configure-environment.md#workspace).

   ```python
   from azureml.core import Experiment, Run, Workspace
   import azureml.core
  
   ws = Workspace.from_config()
   ```
  
## <a name="option-1-use-start_logging"></a>Option 1: Verwenden von start_logging

**Start_logging** erstellt eine interaktive Ausführung für die Verwendung in Szenarien wie z.B. Notebooks. Alle Metriken, die während der Sitzung protokolliert werden, werden der Ausführungsaufzeichnung im Experiment hinzugefügt.

Das folgende Beispiel trainiert ein einfaches Sklearn Ridge-Modell lokal in einem lokalen Jupyter-Notebook. Weitere Informationen zur Übermittlung von Experimenten in verschiedenen Umgebungen finden Sie unter [Einrichten und Verwenden von Computezielen für das Modelltraining](https://docs.microsoft.com/azure/machine-learning/service/how-to-set-up-training-targets).

1. Erstellen Sie ein Trainingsskript in einem lokalen Jupyter-Notebook. 

   ```python
   # load diabetes dataset, a well-known small dataset that comes with scikit-learn
   from sklearn.datasets import load_diabetes
   from sklearn.linear_model import Ridge
   from sklearn.metrics import mean_squared_error
   from sklearn.model_selection import train_test_split
   from sklearn.externals import joblib

   X, y = load_diabetes(return_X_y = True)
   columns = ['age', 'gender', 'bmi', 'bp', 's1', 's2', 's3', 's4', 's5', 's6']
   X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 0)
   data = {
      "train":{"X": X_train, "y": y_train},        
      "test":{"X": X_test, "y": y_test}
   }
   reg = Ridge(alpha = 0.03)
   reg.fit(data['train']['X'], data['train']['y'])
   preds = reg.predict(data['test']['X'])
   print('Mean Squared Error is', mean_squared_error(preds, data['test']['y']))
   joblib.dump(value = reg, filename = 'model.pkl');
   ```

2. Fügen Sie die Experimentnachverfolgung mit dem Azure Machine Learning SDK hinzu und laden Sie ein persistentes Modell in die Ausführungsaufzeichnung des Experiments hoch. Der folgende Code fügt Tags und Protokolle hinzu und lädt eine Modelldatei in die Ausführung des Experiments hoch.

   ```python
    # Get an experiment object from Azure Machine Learning
    experiment = Experiment(workspace=ws, name="train-within-notebook")
    
    # Create a run object in the experiment
    run =  experiment.start_logging()
    # Log the algorithm parameter alpha to the run
    run.log('alpha', 0.03)
    
    # Create, fit, and test the scikit-learn Ridge regression model
    regression_model = Ridge(alpha=0.03)
    regression_model.fit(data['train']['X'], data['train']['y'])
    preds = regression_model.predict(data['test']['X'])
    
    # Output the Mean Squared Error to the notebook and to the run
    print('Mean Squared Error is', mean_squared_error(data['test']['y'], preds))
    run.log('mse', mean_squared_error(data['test']['y'], preds))
    
    # Save the model to the outputs directory for capture
    model_file_name = 'outputs/model.pkl'
    
    joblib.dump(value = regression_model, filename = model_file_name)
    
    # upload the model file explicitly into artifacts 
    run.upload_file(name = model_file_name, path_or_stream = model_file_name)
    
    # Complete the run
    run.complete()
   ```

    Das Skript endet mit ```run.complete()```, wodurch angegeben wird, dass die Ausführung abgeschlossen ist.  Diese Funktion wird in der Regel in Szenarien mit einem interaktiven Notebook verwendet.

## <a name="option-2-use-scriptrunconfig"></a>Option 2: Verwenden von ScriptRunConfig

[**ScriptRunConfig**](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrunconfig?view=azure-ml-py) ist eine Klasse für das Einrichten der Konfigurationen für Skriptausführungen. Mit dieser Option können Sie Überwachungscode hinzufügen, um über den Abschluss informiert zu werden oder um ein visuelles Widget zur Überwachung zu erhalten.

Dieses Beispiel erweitert das grundlegende sklearn Ridge-Modell von oben. Es führt eine einfache Parametersuche durch, um Alphawerte des Modells zu überfliegen, um Metriken und trainierte Modelle in Ausführungen im Rahmen des Experiments zu erfassen. Das Beispiel wird lokal in einer von Benutzer verwalteten Umgebung ausgeführt. 

1. Erstellen Sie ein Trainingsskript`train.py`.

   ```python
   # train.py

   import os
   from sklearn.datasets import load_diabetes
   from sklearn.linear_model import Ridge
   from sklearn.metrics import mean_squared_error
   from sklearn.model_selection import train_test_split
   from azureml.core.run import Run
   from sklearn.externals import joblib

   import numpy as np

   #os.makedirs('./outputs', exist_ok = True)

   X, y = load_diabetes(return_X_y = True)

   run = Run.get_context()

   X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 0)
   data = {"train": {"X": X_train, "y": y_train},
          "test": {"X": X_test, "y": y_test}}

   # list of numbers from 0.0 to 1.0 with a 0.05 interval
   alphas = mylib.get_alphas()

   for alpha in alphas:
      # Use Ridge algorithm to create a regression model
      reg = Ridge(alpha = alpha)
      reg.fit(data["train"]["X"], data["train"]["y"])

      preds = reg.predict(data["test"]["X"])
      mse = mean_squared_error(preds, data["test"]["y"])
      # log the alpha and mse values
      run.log('alpha', alpha)
      run.log('mse', mse)

      model_file_name = 'ridge_{0:.2f}.pkl'.format(alpha)
      # save model in the outputs folder so it automatically get uploaded
      with open(model_file_name, "wb") as file:
          joblib.dump(value = reg, filename = model_file_name)

      # upload the model file explicitly into artifacts 
      run.upload_file(name = model_file_name, path_or_stream = model_file_name)

      # register the model
      #run.register_model(file_name = model_file_name)

      print('alpha is {0:.2f}, and mse is {1:0.2f}'.format(alpha, mse))
  
   ```

2. Über diese `train.py`-Skriptreferenz`mylib.py` erhalten Sie eine Liste der Alphawerte, die im Ridge-Modell verwendet werden sollen.

   ```python
   # mylib.py
  
   import numpy as np

   def get_alphas():
      # list of numbers from 0.0 to 1.0 with a 0.05 interval
      return np.arange(0.0, 1.0, 0.05)
   ```

3. Konfigurieren Sie eine vom Benutzer verwaltete lokale Umgebung.

   ```python
   from azureml.core import Environment
    
   # Editing a run configuration property on-fly.
   user_managed_env = Environment("user-managed-env")
    
   user_managed_env.python.user_managed_dependencies = True
    
   # You can choose a specific Python environment by pointing to a Python path 
   #user_managed_env.python.interpreter_path = '/home/johndoe/miniconda3/envs/myenv/bin/python'
   ```

4. Übermitteln Sie das ```train.py```-Skript, um in der vom Benutzer verwalteten Umgebung ausgeführt zu werden. Der gesamte Skriptordner wird für das Training übermittelt, einschließlich der ```mylib.py```-Datei.

   ```python
   from azureml.core import ScriptRunConfig
    
   exp = Experiment(workspace=ws, name="train-on-local")
   src = ScriptRunConfig(source_directory='./', script='train.py')
   src.run_config.environment = user_managed_env
   run = exp.submit(src)
   ```

## <a name="manage-a-run"></a>Verwalten einer Ausführung

Im Artikel [Starten, Überwachen und Abbrechen von Trainingsausführungen in Python](how-to-manage-runs.md) sind die speziellen Azure Machine Learning-Workflows beschrieben, in denen Sie Experimente verwalten.

## <a name="view-run-details"></a>Anzeigen von Ausführungsdetails

### <a name="monitor-run-with-jupyter-notebook-widget"></a>Überwachen der Ausführung mit einem Jupyter-Notebook-Widget
Wenn Sie Ausführungen mithilfe der Methode **ScriptRunConfig** übermitteln, können Sie den Fortschritt der Ausführung mit einem [Jupyter-Widget](https://docs.microsoft.com/python/api/azureml-widgets/azureml.widgets?view=azure-ml-py) überwachen. Ebenso wie die Übermittlung der Ausführung ist das Widget asynchron und stellt alle 10 bis 15 Sekunden Liveupdates bereit, bis der Auftrag abgeschlossen ist.

1. Zeigen Sie das Jupyter-Widget an, während Sie darauf warten, dass die Ausführung abgeschlossen wird.

   ```python
   from azureml.widgets import RunDetails
   RunDetails(run).show()
   ```

   ![Screenshot des Jupyter-Notebook-Widgets](./media/how-to-track-experiments/run-details-widget.png)

Sie können auch einen Link zur gleichen Anzeige in Ihrem Arbeitsbereich abrufen.

```python
print(run.get_portal_url())
```

2. **[Für automatisierte Machine Learning-Ausführungen]** Für den Zugriff auf die Diagramme aus einer vorherigen Ausführung. Ersetzen Sie `<<experiment_name>>` durch den entsprechenden Experimentnamen:

   ``` 
   from azureml.widgets import RunDetails
   from azureml.core.run import Run

   experiment = Experiment (workspace, <<experiment_name>>)
   run_id = 'autoML_my_runID' #replace with run_ID
   run = Run(experiment, run_id)
   RunDetails(run).show()
   ```

   ![Jupyter Notebook-Widget für automatisiertes Machine Learning](./media/how-to-track-experiments/azure-machine-learning-auto-ml-widget.png)


Zum Anzeigen weiterer Details zu einer Pipeline klicken Sie in der Tabelle auf die Pipeline, die Sie untersuchen möchten. Die Diagramme werden dann in einem Popupfenster aus Azure Machine Learning-Studio gerendert.

### <a name="get-log-results-upon-completion"></a>Abrufen von Protokollergebnissen nach Abschluss

Modelltraining und -überwachung erfolgen im Hintergrund, sodass Sie währenddessen andere Aufgaben ausführen können. Sie können auch warten, bis das Training des Modells abgeschlossen ist, bevor Sie weiteren Code ausführen. Bei der Verwendung von **ScriptRunConfig** können Sie ```run.wait_for_completion(show_output = True)``` einsetzen,um anzuzeigen, wenn das Modelltraining abgeschlossen ist. Mithilfe des ```show_output```-Flags erhalten Sie eine ausführlichen Ausgabe. 


### <a name="query-run-metrics"></a>Metriken der Abfrageausführung

Mit ```run.get_metrics()``` können Sie die Metriken eines trainierten Modells anzeigen. Sie können nun alle Metriken abrufen, die im obigen Beispiel protokolliert wurden, um das beste Modell zu ermitteln.

<a name="view-the-experiment-in-the-web-portal"></a>
## <a name="view-the-experiment-in-your-workspace-in-azure-machine-learning-studiohttpsmlazurecom"></a>Anzeigen des Experiments in Ihrem Arbeitsbereich in [Azure Machine Learning-Studio](https://ml.azure.com)

Wenn ein Experiment abgeschlossen ist, können Sie zu der aufgezeichneten Ausführungsaufzeichnung des Experiments navigieren. Sie können aus [Azure Machine Learning-Studio](https://ml.azure.com) auf den Verlauf zugreifen.

Navigieren Sie zur Registerkarte „Experimente“, und wählen Sie Ihr Experiment aus. Sie gelangen zum Dashboard „Experimentausführung“, auf dem Sie nachverfolgte Metriken und Diagramme sehen können, die für jede Ausführung protokolliert wurden. In diesem Fall protokollierten wir MSE und die Alphawerte.

  ![Ausführungsdetails im Azure Machine Learning-Studio](./media/how-to-track-experiments/experiment-dashboard.png)

Sie können einen Drilldown für eine bestimmte Ausführung ausführen, um deren Ausgaben oder Protokolle anzuzeigen, oder Sie können die Momentaufnahme des von Ihnen übermittelten Experiments herunterladen, damit Sie den Experimentordner mit anderen teilen können.

### <a name="viewing-charts-in-run-details"></a>Anzeigen von Diagrammen in Ausführungsdetails

Es gibt verschiedene Möglichkeiten, wie Sie die Protokollierungs-APIs während einer Ausführung zum Aufzeichnen verschiedener Arten von Metriken und für deren Anzeige als Diagramme in Azure Machine Learning-Studio nutzen können.

|Protokollierter Wert|Beispielcode| Anzeigen im Portal|
|----|----|----|
|Protokollieren eines Arrays mit numerischen Werten| `run.log_list(name='Fibonacci', value=[0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89])`|Liniendiagramm mit nur einer Variablen|
|Protokollieren eines einzelnen numerischen Werts mit mehrfacher Verwendung desselben Metriknamens (z.B. in einer for-Schleife)| `for i in tqdm(range(-10, 10)):    run.log(name='Sigmoid', value=1 / (1 + np.exp(-i))) angle = i / 2.0`| Liniendiagramm mit nur einer Variablen|
|Wiederholtes Protokollieren einer Zeile mit zwei numerischen Spalten|`run.log_row(name='Cosine Wave', angle=angle, cos=np.cos(angle))   sines['angle'].append(angle)      sines['sine'].append(np.sin(angle))`|Liniendiagramm mit zwei Variablen|
|Protokollieren einer Tabelle mit zwei numerischen Spalten|`run.log_table(name='Sine Wave', value=sines)`|Liniendiagramm mit zwei Variablen|


## <a name="example-notebooks"></a>Beispielnotebooks
Die folgenden Notebooks verdeutlichen die Konzepte in diesem Artikel:
* [how-to-use-azureml/training/train-within-notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-within-notebook)
* [how-to-use-azureml/training/train-on-local](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-on-local)
* [how-to-use-azureml/training/logging-api/logging-api.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/logging-api)

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Nächste Schritte

Probieren Sie diese nächsten Schritte aus, um zu erfahren, wie Sie das Azure Machine Learning SDK für Python verwenden können:

* Ein Beispiel für die Registrierung des besten Modells und dessen Bereitstellung finden Sie im Tutorial [Trainieren eines Bildklassifizierungsmodells mit Azure Machine Learning](tutorial-train-models-with-aml.md).

* Erfahren Sie mehr zum [Trainieren von PyTorch-Modellen mit Azure Machine Learning](how-to-train-pytorch.md).
