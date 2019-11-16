---
title: Erkennen von Datendrift in AKS-Bereitstellungen
titleSuffix: Azure Machine Learning
description: Erkennen von Datendrift (Vorschau) bei in Azure Kubernetes Service bereitgestellten Modellen in Azure Machine Learning
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: copeters
author: cody-dkdc
ms.date: 11/04/2019
ms.openlocfilehash: bf82714011754ba516fa38444b1019b9cc1aa732
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74111884"
---
# <a name="detect-data-drift-preview-on-models-deployed-to-azure-kubernetes-service-aks"></a>Erkennen von Datenabweichungen (Vorschauversion) bei in Azure Kubernetes Service (AKS) bereitgestellten Modellen
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-enterprise-sku.md)]

In diesem Artikel erfahren Sie, wie Datenabweichungen zwischen dem Trainingsdataset und den Rückschlussdaten eines bereitgestellten Modells überwacht werden. Im Kontext des maschinellen Lernens kann es für trainierte Machine Learning-Modelle aufgrund von Datenabweichungen zu einer Beeinträchtigung der Vorhersageleistung kommen. Mit Azure Machine Learning können Sie Datenabweichungen überwachen, und der Dienst kann eine E-Mail-Benachrichtigung an Sie senden, wenn eine Abweichung festgestellt wird.

## <a name="what-is-data-drift"></a>Was sind Datenabweichungen?

Im Zusammenhang mit maschinellem Lernen ist die Datenabweichung die Änderung der Modelleingabedaten, die zu einem Abfallen der Modellleistung führt. Sie sind einer der wichtigsten Gründe für die Abnahme der Modellgenauigkeit im Zeitverlauf, und das Überwachen auf Datenabweichungen hilft bei der Erkennung von Problemen bei der Leistung eines Modells. 

## <a name="what-can-i-monitor"></a>Was kann ich überwachen?

Mit Azure Machine Learning können Sie die Eingaben in ein auf AKS bereitgestelltes Modell überwachen und diese Daten mit dem Trainingsdataset für das Modell vergleichen. In regelmäßigen Abständen werden [Schnappschüsse und Profile](how-to-explore-prepare-data.md) der Rückschlussdaten erstellt, und dann erfolgt eine Berechnung gegen den Basisdatensatz, um eine Datenabweichungsanalyse zu erstellen, die Folgendes umfasst: 

+ Messen des Ausmaßes der Datenabweichung, das als Abweichungskoeffizient bezeichnet wird.
+ Messen des Beitrags zur Datenabweichung nach Feature und Informieren, welche Features Datenabweichungen verursacht haben.
+ Messen von Abstandsmetriken. Derzeit werden Wasserstein und Energieabstand berechnet.
+ Messen von Featureverteilungen. Derzeit werden Kerneldichteschätzung und Histogramme berechnet.
+ Senden von Warnungen über Datenabweichungen per E-Mail.

> [!Note]
> Dieser Dienst ist als (Vorschauversion) verfügbar, und die Konfigurationsoptionen sind beschränkt. Details und Updates finden Sie in unserer [API-Dokumentation](https://docs.microsoft.com/python/api/azureml-datadrift/) und den [Versionshinweisen](azure-machine-learning-release-notes.md). 

### <a name="how-data-drift-is-monitored-in-azure-machine-learning"></a>Überwachen von Datenvariationen in Azure Machine Learning

Mit Azure Machine Learning werden Datenvariationen (Datenabweichungen) anhand von Datasets oder Bereitstellungen überwacht. Zum Überwachen von Datenabweichungen wird ein Baseline-Dataset angegeben. Dies ist normalerweise das Trainingsdataset für ein Modell. Ein zweites Dataset – normalerweise gesammelte Modelleingabedaten einer Bereitstellung – wird basierend auf dem Baseline-Dataset getestet. Für beide Datasets wird eine Profilerstellung durchgeführt, und sie werden in den Dienst für die Überwachung von Datenabweichungen eingegeben. Ein Machine Learning-Modell wird trainiert, um Unterschiede zwischen den beiden Datasets zu ermitteln. Die Leistung des Modells wird in den Abweichungskoeffizienten konvertiert, mit dem die Größenordnung der Abweichung zwischen den beiden Datasets gemessen wird. Mit der [Modellinterpretierbarkeit](how-to-machine-learning-interpretability.md) werden die Features berechnet, die zum Abweichungskoeffizienten beigetragen haben. Über das Datasetprofil werden statistische Informationen zu jedem Feature nachverfolgt. 

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Abonnement. Sollten Sie kein Abonnement besitzen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie die [kostenlose oder kostenpflichtige Version von Azure Machine Learning](https://aka.ms/AMLFree) noch heute aus.

- Installiertes Azure Machine Learning SDK für Python. Verwenden Sie die Anleitungen unter [Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py) für Folgendes:

    - Erstellen einer Miniconda-Umgebung
    - Installieren des Azure Machine Learning SDK für Python

- Ein [Azure Machine Learning-Arbeitsbereich](how-to-manage-workspace.md).

- Eine [Konfigurationsdatei](how-to-configure-environment.md#workspace) für den Arbeitsbereich.

- Verwenden Sie den folgenden Befehl, um das Datenabweichungen-SDK zu installieren:

    ```shell
    pip install azureml-datadrift
    ```

- Erstellen Sie ein [Dataset](how-to-create-register-datasets.md) aus den Trainingsdaten Ihres Modells.

- Geben Sie das Trainingsdataset bei der [Registrierung](concept-model-management-and-deployment.md) des Modells an. Das folgende Beispiel veranschaulicht die Verwendung des `datasets`-Parameters zum Angeben des Trainingsdatasets:

    ```python
    model = Model.register(model_path=model_file,
                        model_name=model_name,
                        workspace=ws,
                        datasets=datasets)

    print(model_name, image_name, service_name, model)
    ```

- [Aktivieren Sie die Modelldatenerfassung](how-to-enable-data-collection.md), um Daten aus der AKS-Bereitstellung des Modells zu sammeln, und vergewissern Sie sich, dass Daten im `modeldata`-Blobcontainer gesammelt werden.

## <a name="configure-data-drift"></a>Konfigurieren von Datenabweichungen
Importieren Sie die im folgenden Python-Beispiel gezeigten Abhängigkeiten, um die Datenabweichungen für Ihr Experiment zu konfigurieren. 

Das folgende Python-Beispiel veranschaulicht die Konfiguration des [`DataDriftDetector`](https://docs.microsoft.com/python/api/azureml-contrib-datadrift/azureml.contrib.datadrift.datadriftdetector.datadriftdetector?view=azure-ml-py)-Objekts:

```python
# Import Azure ML packages
from azureml.core import Experiment, Run, RunDetails
from azureml.datadrift import DataDriftDetector, AlertConfiguration

# if email address is specified, setup AlertConfiguration
alert_config = AlertConfiguration('your_email@contoso.com')

# create a new DatadriftDetector object
datadrift = DataDriftDetector.create(ws, model.name, model.version, services, frequency="Day", alert_config=alert_config)
    
print('Details of Datadrift Object:\n{}'.format(datadrift))
```

## <a name="submit-a-datadriftdetector-run"></a>Übermitteln einer DataDriftDetector-Ausführung

Wenn das `DataDriftDetector`-Objekt konfiguriert ist, können Sie an einem bestimmten Datum eine [Datenabweichungausführung](https://docs.microsoft.com/python/api/azureml-datadrift/azureml.datadrift.datadriftdetector.datadriftdetector#run-target-date--services-none--compute-target-none--create-compute-target-false--feature-list-none--drift-threshold-none-) für das Modell übermitteln. Aktivieren Sie als Teil der Ausführung DataDriftDetector-Warnungen, indem Sie den `drift_threshold`-Parameter festlegen. Wenn der [datadrift_coefficient](#visualize-drift-metrics) über dem festgelegten `drift_threshold` liegt, wird eine E-Mail gesendet.

```python
# adhoc run today
target_date = datetime.today()

# create a new compute - creates datadrift-server
run = datadrift.run(target_date, services, feature_list=feature_list, create_compute_target=True)

# or specify existing compute cluster
run = datadrift.run(target_date, services, feature_list=feature_list, compute_target='cpu-cluster')

# show details of the data drift run
exp = Experiment(ws, datadrift._id)
dd_run = Run(experiment=exp, run_id=run)
RunDetails(dd_run).show()
```

## <a name="visualize-drift-metrics"></a>Visualisieren der Abweichungsmetriken

<a name="metrics"></a>

Nachdem Sie die Ausführung von DataDriftDetector übermittelt haben, können Sie die Abweichungsmetriken anzeigen, die bei jeder Ausführungsiteration für eine Datenabweichungen-Aufgabe gespeichert werden:


|Metrik|BESCHREIBUNG|
--|--|
wasserstein_distance|Statistischer Abstand, der für die eindimensionale numerische Verteilung definiert ist.|
energy_distance|Statistischer Abstand, der für die eindimensionale numerische Verteilung definiert ist.|
datadrift_coefficient|Wird ähnlich wie der Matthews Korrelationskoeffizient berechnet, diese Ausgabe ist jedoch eine reelle Zahl im Bereich von 0 bis 1. Im Kontext von Datenabweichungen steht „0“ für keine Abweichung und „1“ für die maximale Abweichung.|
datadrift_contribution|Gewichtung der Features, die zur Abweichung beitragen.|

Es gibt mehrere Möglichkeiten, Metriken für Datenabweichungen anzuzeigen:

* Verwenden Sie das [Jupyter-Widget](https://docs.microsoft.com/python/api/azureml-widgets/azureml.widgets?view=azure-ml-py) `RunDetails`.
* Verwenden Sie die [`get_metrics()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py#get-metrics-name-none--recursive-false--run-type-none--populate-false-)-Funktion für jedes `datadrift`-Ausführungsobjekt.
* Zeigen Sie die Metriken aus dem **Modelle**-Abschnitt Ihres Arbeitsbereichs in [Azure Machine Learning-Studio](https://ml.azure.com) an.

Das folgende Python-Beispiel veranschaulicht das Plotten von relevanten Datenabweichungsmetriken. Sie können die zurückgegebenen Metriken verwenden, um benutzerdefinierte Visualisierungen zu erstellen:

```python
# start and end are datetime objects 
drift_metrics = datadrift.get_output(start_time=start, end_time=end)

# Show all data drift result figures, one per serivice.
# If setting with_details is False (by default), only the data drift magnitude will be shown; if it's True, all details will be shown.
drift_figures = datadrift.show(with_details=True)
```

![Anzeigen der von Azure Machine Learning erkannten Datenabweichungen](media/how-to-monitor-data-drift/drift_show.png)


## <a name="schedule-data-drift-scans"></a>Planen von Überprüfungen auf Datenabweichungen 

Wenn Sie die Erkennung von Datenabweichungen aktivieren, wird ein DataDriftDetector-Vorgang mit der angegebenen geplanten Häufigkeit ausgeführt. Wenn der „datadrift_coefficient“ den angegebenen `drift_threshold` erreicht, wird für jede geplante Ausführung eine E-Mail gesendet. 

```python
datadrift.enable_schedule()
datadrift.disable_schedule()
```

Die Konfiguration der Datendrifterkennung wird unter **Modelle** auf der Registerkarte **Details** in Ihrem Arbeitsbereich im [Azure Machine Learning-Studio](https://ml.azure.com) angezeigt.

[![Datendrift im Azure Machine Learning-Studio](media/how-to-monitor-data-drift/drift-config.png)](media/how-to-monitor-data-drift/drift-config-expanded.png)

## <a name="view-results-in-your-azure-machine-learning-studio"></a>Anzeigen von Ergebnissen in ihrem Azure Machine Learning-Studio

Um Ergebnisse in Ihrem Arbeitsbereich im [Azure Machine Learning-Studio](https://ml.azure.com) anzuzeigen, navigieren Sie zur Modellseite. Auf der Registerkarte „Details“ des Modells wird die Konfiguration der Datenabweichung angezeigt. Es ist nun die Registerkarte **Datenvariation** verfügbar, auf der Datenvariationsmetriken visualisiert werden. 

[![Datendrift im Azure Machine Learning-Studio](media/how-to-monitor-data-drift/drift-ui.png)](media/how-to-monitor-data-drift/drift-ui-expanded.png)

## <a name="receiving-drift-alerts"></a>Empfangen von Benachrichtigungen zu Abweichungen

Wenn Sie den Abweichungskoeffizienten für den Benachrichtigungsschwellenwert festlegen und eine E-Mail-Adresse angeben, wird automatisch eine [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview)-E-Mail-Benachrichtigung gesendet, wenn der Abweichungskoeffizient über dem Schwellenwert liegt. 

Damit Sie benutzerdefinierte Benachrichtigungen und Aktionen einrichten können, werden alle Datenabweichungsmetriken in der [Application Insights](how-to-enable-app-insights.md)-Ressource gespeichert, die zusammen mit dem Azure Machine Learning-Arbeitsbereich erstellt wurde. Über den Link in der E-Mail-Benachrichtigung gelangen Sie zur Application Insights-Abfrage.

![E-Mail-Benachrichtigung über Datenabweichungen](media/how-to-monitor-data-drift/drift_email.png)

## <a name="retrain-your-model-after-drift"></a>Erneutes Trainieren des Modells nach den Datenabweichungen

Wenn Datenabweichungen die Leistung des bereitgestellten Modells beeinträchtigen, muss das Modell erneut trainiert werden. Fahren Sie zu diesem Zweck mit den folgenden Schritten fort.

* Untersuchen Sie die gesammelten Daten, und bereiten Sie die Daten zum Trainieren des neuen Modells vor.
* Teilen Sie sie in Trainings- und Testdaten auf.
* Trainieren Sie das Modell mithilfe der neuen Daten erneut.
* Bewerten Sie die Leistung des neu generierten Modells.
* Stellen Sie das neue Modell bereit, wenn seine Leistung besser als die des Produktionsmodells ist.

## <a name="next-steps"></a>Nächste Schritte

* Ein vollständiges Beispiel der Verwendung von Datenabweichungen finden Sie im [Azure ML-Notebook zu Datenabweichungen](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/monitor-models/data-drift/drift-on-aks.ipynb). Dieses Jupyter Notebook veranschaulicht die Verwendung eines [öffentlichen Azure-Datasets](https://docs.microsoft.com/azure/open-datasets/overview-what-are-open-datasets) zum Trainieren eines Modells zur Wettervorhersage, die Bereitstellung des Modells AKS sowie die Überwachung auf Datenabweichungen. 

* Erkennen von Datendrift mit [Datasetmonitoren](how-to-monitor-datasets.md)

* Wir würden uns sehr über Ihre Fragen, Kommentare oder Vorschläge freuen, während die allgemeine Verfügbarkeit von Datenabweichungen näher rückt. Verwenden Sie die Schaltfläche für Produktfeedback unten! 
