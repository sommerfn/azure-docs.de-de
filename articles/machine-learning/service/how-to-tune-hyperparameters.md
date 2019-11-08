---
title: Optimieren von Hyperparametern für Ihr Modell
titleSuffix: Azure Machine Learning
description: Mit Azure Machine Learning können Sie die Hyperparameter für Ihr Deep Learning-/Machine Learning-Modell effizient optimieren. Im Folgenden erfahren Sie, wie Sie den Suchbereich für Parameter definieren, eine zu optimierende primäre Metrik festlegen und Läufe mit schlechter Leistung vorzeitig beenden.
ms.author: swatig
author: swatig007
ms.reviewer: sgilley
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 11/04/2019
ms.custom: seodec18
ms.openlocfilehash: a7b0276ca41e1b9342b3602a67dea0517c60f66a
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73489339"
---
# <a name="tune-hyperparameters-for-your-model-with-azure-machine-learning"></a>Optimieren von Hyperparametern für Ihr Modell mit Azure Machine Learning
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

Mit Azure Machine Learning können Sie die Hyperparameter für Ihr Modell effizient optimieren.  Die Optimierung von Hyperparametern umfasst die folgenden Schritte:

* Definieren des Suchbereichs für Parameter
* Festlegen einer zu optimierenden primären Metrik  
* Festlegen von Kriterien für die vorzeitige Beendigung von Läufen mit schlechter Leistung
* Zuweisen von Ressourcen für die Optimierung der Hyperparameter
* Starten eines Experiments mit der oben dargestellten Konfiguration
* Visualisieren der Trainingsläufe
* Auswählen der Konfiguration mit der besten Leistung für Ihr Modell

## <a name="what-are-hyperparameters"></a>Was sind Hyperparameter?

Hyperparameter sind anpassbare Parameter, die den eigentlichen Trainingsprozess steuern und von Ihnen zum Trainieren eines Modells ausgewählt werden. Beispielsweise legen Sie zum Trainieren eines Deep Neural Networks die Anzahl verborgener Schichten im Netzwerk und die Anzahl von Knoten in jeder Schicht fest, bevor Sie das Modell trainieren. Diese Werte bleiben normalerweise während des Trainingsprozesses konstant.

In Deep Learning-/Machine Learning-Szenarien hängt die Leistung von Modellen stark von den gewählten Werten der Hyperparameter ab. Das Ziel der Untersuchung von Hyperparametern besteht darin, verschiedene Hyperparameterkonfigurationen zu durchsuchen, um eine Konfiguration zu finden, die zur optimalen Leistung führt. Normalerweise ist die Untersuchung von Hyperparametern ein mühevoller manueller Vorgang, angesichts des riesigen Suchbereichs und der hohen Kosten für die Bewertung der einzelnen Konfigurationen.

In Azure Machine Learning können Sie die Untersuchung von Hyperparametern in effizienter Weise automatisieren, was in erheblichem Umfang Zeit und Ressourcen spart. Sie können den Bereich der Hyperparameterwerte und eine maximale Anzahl von Trainingsläufen angeben. Das System startet dann automatisch mehrere gleichzeitige Läufe mit verschiedenen Parameterkonfigurationen und ermittelt die Konfiguration, die gemessen an der von Ihnen ausgewählten Metrik die beste Leistung bietet. Trainingsläufe mit schlechter Leistung werden automatisch frühzeitig beendet, um die Vergeudung von Computeressourcen zu verringern. Stattdessen werden diese Ressourcen dazu verwendet, andere Hyperparameterkonfigurationen zu untersuchen.


## <a name="define-search-space"></a>Definieren des Suchbereichs

Optimieren Sie Hyperparameter automatisch durch Untersuchen des für jeden Hyperparameter definierten Wertebereichs.

### <a name="types-of-hyperparameters"></a>Typen von Hyperparametern

Jeder Hyperparameter kann diskret oder kontinuierlich sein und weist eine Verteilung von Werten auf, die durch einen [Parameterausdruck](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive.parameter_expressions?view=azure-ml-py) beschrieben wird.

#### <a name="discrete-hyperparameters"></a>Diskrete Hyperparameter 

Diskrete Hyperparameter werden als eine `choice` unter diskreten Werten angegeben. `choice` kann Folgendes sein:

* Ein oder mehrere durch Trennzeichen getrennte Werte
* Ein `range`-Objekt
* Ein beliebiges `list`-Objekt


```Python
    {
        "batch_size": choice(16, 32, 64, 128)
        "number_of_hidden_layers": choice(range(1,5))
    }
```

In diesem Fall wird für `batch_size` einer der Werte [16, 32, 64, 128] verwendet und für `number_of_hidden_layers` einer der Werte [1, 2, 3, 4].

Erweiterte diskrete Hyperparameter können ferner mithilfe einer Verteilung angegeben werden. Die folgenden Verteilungen werden unterstützt:

* `quniform(low, high, q)`: Gibt einen Wert wie „round(uniform(low, high) / q) * q“ zurück
* `qloguniform(low, high, q)`: Gibt einen Wert wie „round(exp(uniform(low, high)) / q) * q“ zurück
* `qnormal(mu, sigma, q)`: Gibt einen Wert wie „round(normal(mu, sigma) / q) * q“ zurück
* `qlognormal(mu, sigma, q)`: Gibt einen Wert wie „round(exp(normal(mu, sigma)) / q) * q“ zurück

#### <a name="continuous-hyperparameters"></a>Kontinuierliche Hyperparameter 

Kontinuierliche Hyperparameter werden als Verteilung über einen kontinuierlichen Wertebereich angegeben. Folgende Verteilungen werden u. a. unterstützt:

* `uniform(low, high)`:Gibt einen gleichmäßig zwischen niedrig und hoch verteilen Wert zurück
* `loguniform(low, high)`: Gibt einen gemäß „exp(uniform(low, high))“ ermittelten Wert zurück, sodass der Logarithmus des Rückgabewerts gleichmäßig verteilt ist
* `normal(mu, sigma)`: Gibt einen realen Wert zurück, der mit einem Mittelwert mu und der Standardabweichung sigma normalverteilt ist
* `lognormal(mu, sigma)`: Gibt einen gemäß „exp(normal(mu, sigma))“ ermittelten Wert zurück, sodass der Logarithmus des Rückgabewerts normalverteilt ist

Im Folgenden sehen Sie ein Beispiel für die Definition des Parameterbereichs:

```Python
    {    
        "learning_rate": normal(10, 3),
        "keep_probability": uniform(0.05, 0.1)
    }
```

Dieser Code definiert einen Suchbereich mit zwei Parametern: `learning_rate` und `keep_probability`. `learning_rate` weist eine Normalverteilung mit dem Mittelwert 10 und der Standardabweichung 3 auf. `keep_probability` weist eine gleichmäßige Verteilung mit dem Mindestwert 0,05 und dem Höchstwert 0,1 auf.

### <a name="sampling-the-hyperparameter-space"></a>Stichprobenentnahme im Hyperparameterraum

Sie können außerdem die für die Definition des Hyperparameterbereichs zu verwendende Parametersamplingmethode (Stichprobenentnahme) festlegen. Azure Machine Learning unterstützt Zufallssampling, Rastersampling und Bayessches Sampling.

#### <a name="picking-a-sampling-method"></a>Wählen einer Samplingmethode

* Rastersampling kann verwendet werden, wenn Ihr Hyperparameterraum als eine Wahl zwischen diskreten Werten definiert werden kann und wenn Sie über ein ausreichendes Budget verfügen, um alle Werte im definierten Suchraum vollständig durchzusuchen. Darüber hinaus können Sie die automatisierte vorzeitige Beendigung leistungsschwacher Läufe nutzen, wodurch die Vergeudung von Ressourcen reduziert wird.
* Beim Zufallssampling darf der Hyperparameterraum sowohl diskrete als auch kontinuierliche Hyperparameter enthalten. In der Praxis führt dies meist zu guten Ergebnissen und ermöglicht auch die vorzeitige automatisierte Beendigung leistungsschwacher Läufe. Einige Benutzer führen eine erste Suche mit Zufallssampling durch und verfeinern anschließend den Suchraum iterativ, um Ergebnisse zu verbessern.
* Bayessches Sampling nutzt Kenntnisse früherer Stichproben bei der Wahl von Hyperparameterwerten und versucht, die gemeldete primäre Metrik effektiv zu verbessern. Bayessches Sampling wird empfohlen, wenn Sie über ein ausreichendes Budget verfügen, um den Hyperparameterraum zu erkunden. Für beste Ergebnisse beim bayesschen Sampling empfehlen wir eine maximale Anzahl von Läufen, die größer oder gleich dem 20-fachen der Anzahl der zu optimierenden Hyperparameter ist. Beim bayesschen Sampling wird zurzeit keine Richtlinie für frühzeitige Beendigung unterstützt.

#### <a name="random-sampling"></a>Zufallssampling

Beim Zufallssampling werden Hyperparameterwerte zufällig aus dem definierten Suchbereich ausgewählt. Beim [Zufallssampling](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive.randomparametersampling?view=azure-ml-py) darf der Suchraum sowohl diskrete als auch kontinuierliche Hyperparameter enthalten.

```Python
from azureml.train.hyperdrive import RandomParameterSampling
param_sampling = RandomParameterSampling( {
        "learning_rate": normal(10, 3),
        "keep_probability": uniform(0.05, 0.1),
        "batch_size": choice(16, 32, 64, 128)
    }
)
```

#### <a name="grid-sampling"></a>Rastersampling

Beim [Rastersampling](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive.gridparametersampling?view=azure-ml-py) wird eine einfache Rastersuche über alle zulässigen Werte im definierten Suchraum ausgeführt. Es kann nur für Hyperparameter verwendet werden, die mithilfe von `choice` angegeben wurden. Der folgende Bereich enthält beispielsweise insgesamt sechs Stichproben:

```Python
from azureml.train.hyperdrive import GridParameterSampling
param_sampling = GridParameterSampling( {
        "num_hidden_layers": choice(1, 2, 3),
        "batch_size": choice(16, 32)
    }
)
```

#### <a name="bayesian-sampling"></a>Bayessches Sampling

[Bayessches Sampling](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive.bayesianparametersampling?view=azure-ml-py) basiert auf dem bayesschen Optimierungsalgorithmus und trifft intelligente Entscheidungen zu den als nächstes zu sampelnden Hyperparameterwerten. Bei diesem Verfahren wird die Stichprobe basierend auf der Leistung der vorherigen Stichprobe so ausgewählt, dass die neue Stichprobe die gemeldete primäre Metrik verbessert.

Beim Bayesschen Sampling wirkt sich die Anzahl gleichzeitiger Läufe auf die Effektivität des Optimierungsprozesses aus. Normalerweise kann eine kleinere Anzahl gleichzeitiger Läufe zu einer besseren Samplingkonvergenz führen, da durch den geringeren Parallelitätsgrad die Anzahl von Läufen zunimmt, die von zuvor abgeschlossenen Läufen profitieren.

Bayessches Sampling unterstützt nur `choice`-, `uniform`- und `quniform`-Verteilungen über den Suchraum.

```Python
from azureml.train.hyperdrive import BayesianParameterSampling
param_sampling = BayesianParameterSampling( {
        "learning_rate": uniform(0.05, 0.1),
        "batch_size": choice(16, 32, 64, 128)
    }
)
```

> [!NOTE]
> Beim Bayesschen Sampling wird keine Richtlinie für vorzeitige Beendigung unterstützt (siehe dazu [Festlegen einer Richtlinie für vorzeitige Beendigung](#specify-early-termination-policy)). Legen Sie bei der Verwendung des Bayesschen Parametersamplings `early_termination_policy = None` fest, oder lassen Sie den `early_termination_policy`-Parameter weg.

<a name='specify-primary-metric-to-optimize'/>

## <a name="specify-primary-metric"></a>Festlegen der primären Metrik

Legen Sie die [primäre Metrik](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive.primarymetricgoal?view=azure-ml-py) fest, die durch das Experiment zur Hyperparameteroptimierung optimiert werden soll. Jeder Trainingslauf wird im Hinblick auf die primäre Metrik ausgewertet. Läufe mit schlechter Leistung (bei denen die primäre Metrik nicht den von der Richtlinie für vorzeitige Beendigung festgelegten Kriterien entspricht) werden beendet. Neben dem Namen der primären Metrik geben Sie das Ziel der Optimierung an, d. h. ob die primäre Metrik maximiert oder minimiert werden soll.

* `primary_metric_name`: Der Name der zu optimierenden primären Metrik. Der Name der primären Metrik muss exakt mit dem Namen der vom Trainingsskript protokollierten Metrik übereinstimmen. Weitere Informationen finden Sie unter [Protokollieren von Metriken für die Hyperparameteroptimierung](#log-metrics-for-hyperparameter-tuning).
* `primary_metric_goal`: Kann entweder `PrimaryMetricGoal.MAXIMIZE` oder `PrimaryMetricGoal.MINIMIZE` sein und bestimmt, ob die primäre Metrik beim Auswerten der Läufe maximiert oder minimiert wird. 

```Python
primary_metric_name="accuracy",
primary_metric_goal=PrimaryMetricGoal.MAXIMIZE
```

Die Läufe werden zur Maximierung von „accuracy“ (Genauigkeit) optimiert.  Stellen Sie sicher, dass dieser Wert in Ihrem Trainingsskript protokolliert wird.

<a name='log-metrics-for-hyperparameter-tuning'/>

### <a name="log-metrics-for-hyperparameter-tuning"></a>Protokollieren von Metriken für die Hyperparameteroptimierung

Das Trainingsskript für Ihr Modell muss die relevanten Metriken während des Modelltrainings protokollieren. Beim Konfigurieren der Hyperparameteroptimierung geben Sie die primäre Metrik für die Bewertung der Leistung von Läufen an. (Siehe [Festlegen einer zu optimierenden primären Metrik](#specify-primary-metric-to-optimize).)  Sie müssen diese Metrik in Ihrem Trainingsskript protokollieren, damit sie für den Prozess zur Hyperparameteroptimierung verfügbar ist.

Protokollieren Sie diese Metrik mit dem folgenden Beispielcodeausschnitt in Ihrem Trainingsskript:

```Python
from azureml.core.run import Run
run_logger = Run.get_context()
run_logger.log("accuracy", float(val_accuracy))
```

Das Trainingsskript berechnet den `val_accuracy`-Wert und protokolliert ihn als „accuracy“ (der als primäre Metrik verwendete Wert). Jedes Mal, wenn die Metrik protokolliert wird, wird der Wert vom Dienst zur Hyperparameteroptimierung empfangen. Die Entscheidung, wie häufig diese Metrik gemeldet werden soll, liegt beim Entwickler des Modells.

<a name='specify-early-termination-policy'/>

## <a name="specify-early-termination-policy"></a>Festlegen einer Richtlinie für vorzeitige Beendigung

Sie können Läufe mit schlechter Leistung automatisch mit einer Richtlinie für vorzeitige Beendigung beenden. Durch die Beendigung wird die Vergeudung von Ressourcen verringert, die stattdessen zur Untersuchung weiterer Parameterkonfigurationen eingesetzt werden.

Bei der Verwendung einer Richtlinie für vorzeitige Beendigung können Sie die folgenden Parameter konfigurieren, die steuern, wann eine Richtlinie angewendet wird:

* `evaluation_interval`: die Anwendungshäufigkeit der Richtlinie. Jede Protokollierung der primären Metrik durch das Trainingsskript zählt als ein Intervall. Also wird beim Wert 1 für `evaluation_interval` die Richtlinie jedes Mal angewendet, wenn das Trainingsskript die primäre Metrik meldet. Beim Wert 2 für `evaluation_interval` wird die Richtlinie bei jeder zweiten Meldung der primären Metrik durch das Trainingsskript angewendet. Wenn kein Intervall angegeben wird, wird `evaluation_interval` standardmäßig auf 1 festgelegt.
* `delay_evaluation`: Verzögert die erste Auswertung der Richtlinie für eine angegebene Anzahl Intervalle. Dies ist ein optionaler Parameter, der die Ausführung aller Konfigurationen für eine anfängliche Mindestanzahl von Intervallen ermöglicht, um die vorzeitige Beendigung von Trainingsläufen zu verhindern. Wenn er angegeben wird, wird dir Richtlinie bei jedem Vielfachen von evaluation_interval angewendet, das größer als oder gleich groß wie delay_evaluation ist.

Azure Machine Learning unterstützt die folgenden Richtlinien für vorzeitige Beendigung.

### <a name="bandit-policy"></a>Banditenrichtlinie

Die [Banditenrichtlinie](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive.banditpolicy?view=azure-ml-py#definition) ist eine Beendigungsrichtlinie, die auf einem Pufferbereich/Pufferbetrag und einem Auswertungsintervall basiert. Die Richtlinie beendet vorzeitig alle Läufe, bei denen die primäre Metrik bezogen auf den Trainingslauf mit der besten Leistung nicht dem angegebenen Pufferbereich/Pufferbetrag entspricht. Sie akzeptiert die folgenden Konfigurationsparameter:

* `slack_factor` oder `slack_amount`: Der bezogen auf den Trainingslauf mit der besten Leistung zulässige Puffer. `slack_factor` gibt den zulässigen Puffer als Verhältnis an. `slack_amount` gibt den zulässigen Puffer als absoluten Betrag anstelle eines Verhältnisses an.

    Betrachten Sie als Beispiel die Anwendung einer Banditenrichtlinie auf das Intervall 10. Angenommen, der Lauf mit der besten Leistung in Intervall 10 hat für das Ziel, die primäre Metrik zu maximieren, die primäre Metrik 0,8 gemeldet. Wenn die Richtlinie mit einem `slack_factor` von 0,2 angegeben wurde, werden alle Trainingsläufe, deren beste Metrik im Intervall 10 kleiner als 0,66 (0,8/(1+`slack_factor`)) ist, beendet. Wenn die Richtlinie stattdessen mit einem `slack_amount` von 0,2 angegeben wurde, werden alle Trainingsläufe, deren beste Metrik im Intervall 10 kleiner als 0,6 (0,8/(1+`slack_amount`)) ist, beendet.
* `evaluation_interval`: die Anwendungshäufigkeit der Richtlinie (optionaler Parameter).
* `delay_evaluation`: verzögert die erste Auswertung der Richtlinie für eine angegebene Anzahl Intervalle (optionaler Parameter).


```Python
from azureml.train.hyperdrive import BanditPolicy
early_termination_policy = BanditPolicy(slack_factor = 0.1, evaluation_interval=1, delay_evaluation=5)
```

In diesem Beispiel wird die Richtlinie zur frühzeitigen Beendigung bei jedem Intervall angewendet, wenn Metriken gemeldet werden, beginnend bei Auswertungsintervall 5. Jeder Lauf, dessen beste Metrik kleiner als (1/(1+0,1) oder 91 % des Laufs mit der besten Leistung ist, wird beendet.

### <a name="median-stopping-policy"></a>Medianstopprichtlinie

Die [Medianstopprichtlinie](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive.medianstoppingpolicy?view=azure-ml-py) ist eine Richtlinie für vorzeitige Beendigung, die auf dem gleitenden Durchschnitt der von den Ausführungen gemeldeten primären Metriken basiert. Diese Richtlinie berechnet den gleitenden Durchschnitt über alle Trainingsläufe und beendet Läufe, deren Leistung schlechter als der Median der gleitenden Durchschnittswerte ist. Diese Richtlinie akzeptiert die folgenden Konfigurationsparameter:
* `evaluation_interval`: die Anwendungshäufigkeit der Richtlinie (optionaler Parameter).
* `delay_evaluation`: verzögert die erste Auswertung der Richtlinie für eine angegebene Anzahl Intervalle (optionaler Parameter).


```Python
from azureml.train.hyperdrive import MedianStoppingPolicy
early_termination_policy = MedianStoppingPolicy(evaluation_interval=1, delay_evaluation=5)
```

In diesem Beispiel wird die Richtlinie zur frühzeitigen Beendigung bei jedem Intervall angewendet, beginnend bei Auswertungsintervall 5. Ein Lauf wird in Intervall 5 beendet, wenn seine beste primäre Metrik schlechter als der Median des gleitenden Durchschnitts der Intervalle 1:5 über alle Trainingsläufe ist.

### <a name="truncation-selection-policy"></a>Kürzungsauswahlrichtlinie

Die [Kürzungsauswahlrichtlinie](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive.truncationselectionpolicy?view=azure-ml-py) bricht bei jedem Auswertungsintervall einen angegebenen Prozentsatz der Ausführungen mit der schlechtesten Leistung ab. Die Läufe werden auf der Grundlage ihrer Leistung bezogen auf die primäre Metrik verglichen, und die niedrigsten X % werden beendet. Sie akzeptiert die folgenden Konfigurationsparameter:

* `truncation_percentage`: der Prozentsatz der Läufe mit der schwächsten Leistung, die bei jedem Auswertungsintervall beendet werden sollen. Geben Sie eine ganze Zahl zwischen 1 und 99 ein.
* `evaluation_interval`: die Anwendungshäufigkeit der Richtlinie (optionaler Parameter).
* `delay_evaluation`: verzögert die erste Auswertung der Richtlinie für eine angegebene Anzahl Intervalle (optionaler Parameter).


```Python
from azureml.train.hyperdrive import TruncationSelectionPolicy
early_termination_policy = TruncationSelectionPolicy(evaluation_interval=1, truncation_percentage=20, delay_evaluation=5)
```

In diesem Beispiel wird die Richtlinie zur frühzeitigen Beendigung bei jedem Intervall angewendet, beginnend bei Auswertungsintervall 5. Ein Lauf wird bei Intervall 5 beendet, wenn seine Leistung in den unteren 20 % der Leistung aller Läufe bei Intervall 5 liegt.

### <a name="no-termination-policy"></a>Keine Beendigungsrichtlinie

Wenn Sie alle Trainingsläufe bis zum Abschluss ausführen lassen möchten, legen Sie die Richtlinie auf „None“ fest. Das hat die Wirkung, dass keine Richtlinie zur frühzeitigen Beendigung angewendet wird.

```Python
policy=None
```

### <a name="default-policy"></a>Standardrichtlinie

Ist keine Richtlinie angegeben, lässt der Dienst zur Optimierung von Hyperparametern alle Trainingsläufe bis zum Abschluss ausführen.

### <a name="picking-an-early-termination-policy"></a>Wählen einer Richtlinie für frühzeitige Beendigung

* Wenn Sie eine konservative Richtlinie möchten, die eine Ersparnis ohne Beendigung vielversprechender Aufträge ermöglicht, können Sie eine Medianstopprichtlinie mit den Werten 1 für `evaluation_interval` und 5 für `delay_evaluation` verwenden. Dies ist eine konservative Einstellung, die annähernd 25 %–35 % Ersparnis ohne Verluste bei der primären Metrik erbringen kann (bezogen auf unsere Auswertungsdaten).
* Wenn Sie nach aggressiveren Einsparungen durch vorzeitige Beendigung suchen, können Sie entweder die sog. Bandit-Richtlinie mit einem strengeren (kleineren) zulässigen Puffer oder die Kürzungsauswahlrichtlinie mit einem höheren Kürzungsprozentsatz verwenden.

## <a name="allocate-resources"></a>Zuordnen von Ressourcen

Sie können das Ressourcenbudget für Ihr Experiment zur Hyperparameteroptimierung kontrollieren, indem Sie die maximale Gesamtzahl von Trainingsläufen festlegen.  Optional können Sie die maximale Dauer für Ihr Experiment zur Hyperparameteroptimierung festlegen.

* `max_total_runs`: Die maximale Gesamtzahl von Trainingsläufen, die erstellt werden. Dies ist eine Obergrenze. Es können weniger Läufe stattfinden, beispielsweise wenn der Hyperparameterbereich endlich ist und weniger Stichproben umfasst. Dies muss eine Zahl zwischen 1 und 1000 sein.
* `max_duration_minutes`: Die maximale Dauer des Experiments zur Hyperparameteroptimierung in Minuten. Dies ist ein optionaler Parameter. Wird er angegeben, werden alle Läufe, die nach Ablauf dieses Zeitraums ausgeführt würden, automatisch abgebrochen.

>[!NOTE] 
>Wenn sowohl `max_total_runs` als auch `max_duration_minutes` angegeben werden, wird das Experiment zur Hyperparameteroptimierung beim Erreichen des ersten dieser beiden Schwellenwerte beendet.

Darüber hinaus können Sie die maximale Anzahl von Trainingsläufen festlegen, die während der Suche zur Hyperparameteroptimierung gleichzeitig ausgeführt werden.

* `max_concurrent_runs`: Die maximale Anzahl gleichzeitiger Läufe zu jedem beliebigen Zeitpunkt. Ohne diese Angabe werden alle `max_total_runs` parallel gestartet. Wenn der Wert angegeben wird, muss er eine Zahl zwischen 1 und 100 sein.

>[!NOTE] 
>Die Anzahl der gleichzeitigen Läufe wird durch die im angegebenen Computeziel verfügbaren Ressourcen beschränkt. Daher müssen Sie sicherstellen, dass das Computeziel die verfügbaren Ressourcen für die gewünschte Parallelität aufweist.

Zuordnen von Ressourcen für die Hyperparameteroptimierung:

```Python
max_total_runs=20,
max_concurrent_runs=4
```

Dieser Code konfiguriert das Experiment zur Hyperparameteroptimierung für eine Gesamtanzahl von maximal 20 Läufen, in denen jeweils vier Konfigurationen ausgeführt werden.

## <a name="configure-experiment"></a>Konfigurieren des Experiments

[Konfigurieren Sie Ihr Experiment zur Hyperparameteroptimierung](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive.hyperdriverunconfig?view=azure-ml-py) wie in den obigen Abschnitten beschrieben mithilfe des definierten Suchbereichs für Hyperparameter, der Richtlinie für vorzeitige Beendigung, der primären Metrik und der Ressourcenzuordnung. Geben Sie außerdem einen `estimator` an, der mit den als Stichprobe verwendeten Hyperparametern aufgerufen wird. Der `estimator` beschreibt das auszuführende Trainingsskript, die Ressourcen pro Auftrag (einzelne oder mehrere GPUs) und das zu verwendende Computeziel. Da die Parallelität für Ihr Experiment zur Hyperparameteroptimierung durch die verfügbaren Ressourcen beschränkt ist, müssen Sie sicherstellen, dass das von Ihnen im `estimator` angegebene Computeziel über ausreichende Ressourcen für die gewünschte Parallelität verfügt. (Weitere Informationen zu Schätzungen finden Sie unter [Trainieren von Modellen](how-to-train-ml-models.md).)

Konfigurieren Sie Ihr Experiment zur Hyperparameteroptimierung:

```Python
from azureml.train.hyperdrive import HyperDriveConfig
hyperdrive_run_config = HyperDriveConfig(estimator=estimator,
                          hyperparameter_sampling=param_sampling, 
                          policy=early_termination_policy,
                          primary_metric_name="accuracy", 
                          primary_metric_goal=PrimaryMetricGoal.MAXIMIZE,
                          max_total_runs=100,
                          max_concurrent_runs=4)
```

## <a name="submit-experiment"></a>Senden eines Experiments

Nachdem Sie Ihre Konfiguration für die Hyperparameteroptimierung definiert haben, [übermitteln Sie ein Experiment](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment%28class%29?view=azure-ml-py#submit-config--tags-none----kwargs-):

```Python
from azureml.core.experiment import Experiment
experiment = Experiment(workspace, experiment_name)
hyperdrive_run = experiment.submit(hyperdrive_run_config)
```

`experiment_name` ist der Name, den Sie Ihrem Experiment zur Hyperparameteroptimierung zuweisen, und `workspace` ist der Arbeitsbereich, in dem Sie das Experiment erstellen möchten (weitere Informationen zu Experimenten finden Sie unter [Wie funktioniert Azure Machine Learning?](concept-azure-machine-learning-architecture.md)).

## <a name="warm-start-your-hyperparameter-tuning-experiment-optional"></a>Warmstart Ihres Experiments zur Optimierung von Hyperparametern (optional)

Häufig kann das Finden der besten Hyperparameterwerte für Ihr Modell ein iterativer Prozess sein, der mehrere Optimierungsläufe erfordert, die von vorherigen Läufen zur Hyperparameteroptimierung lernen. Durch die Wiederverwendung des Wissens aus diesen vorherigen Läufen wird der Prozess der Hyperparameteroptimierung beschleunigt. Dadurch werden die Kosten für die Optimierung des Modells reduziert und die primäre Metrik des resultierenden Modells möglicherweise verbessert. Beim Warmstart eines Experiments zur Hyperparameteroptimierung mit bayesschem Sampling werden Versuche aus dem vorherigen Lauf als Vorkenntnisse verwendet, um intelligent neue Stichproben mit dem Ziel auszuwählen, die primäre Metrik zu verbessern. Darüber hinaus werden beim Zufalls- oder Rastersampling bei vorzeitigen Beendigungsentscheidungen Metriken aus den vorangegangenen Läufen herangezogen, um schlecht abschneidende Trainingsläufe zu ermitteln. 

Azure Machine Learning ermöglicht Ihnen einen Warmstart Ihres Laufs zur Hyperparameteroptimierung, indem Sie das Wissen von bis zu 5 zuvor abgeschlossenen/abgebrochenen übergeordneten Läufen zur Hyperparameteroptimierung nutzen. Sie können die Liste der übergeordneten Läufe für einen Warmstart angeben, indem Sie diesen Codeausschnitt verwenden:

```Python
from azureml.train.hyperdrive import HyperDriveRun

warmstart_parent_1 = HyperDriveRun(experiment, "warmstart_parent_run_ID_1")
warmstart_parent_2 = HyperDriveRun(experiment, "warmstart_parent_run_ID_2")
warmstart_parents_to_resume_from = [warmstart_parent_1, warmstart_parent_2]
```

Darüber hinaus kann es vorkommen, dass einzelne Trainingsläufe eines Experiments zur Hyperparameteroptimierung aus Budgetgründen abgebrochen werden oder aus anderen Gründen nicht stattfinden. Es ist nun möglich, solche individuellen Trainingsläufe ab dem letzten Prüfpunkt fortzusetzen (vorausgesetzt, Ihr Trainingsskript unterstützt Prüfpunkte). Wenn Sie einen einzelnen Trainingslauf fortsetzen, wird die gleiche Hyperparameterkonfiguration verwendet und der Ordner mit den Ausgaben eingebunden, der für diesen Lauf verwendet wird. Das Trainingsskript muss das Argument `resume-from` akzeptieren, das die Prüfpunkt- oder Modelldateien enthält, mit denen der Trainingslauf fortgesetzt werden kann. Mithilfe des folgenden Codeausschnitts können Sie einzelne Trainingsläufe fortsetzen:

```Python
from azureml.core.run import Run

resume_child_run_1 = Run(experiment, "resume_child_run_ID_1")
resume_child_run_2 = Run(experiment, "resume_child_run_ID_2")
child_runs_to_resume = [resume_child_run_1, resume_child_run_2]
```

Sie können Ihr Experiment zur Hyperparameteroptimierung so konfigurieren, dass es einen Warmstart aus einem früheren Experiment ermöglicht, oder einzelne Trainingsläufe mithilfe der optionalen Parameter `resume_from` und `resume_child_runs` in der Konfiguration fortsetzen:

```Python
from azureml.train.hyperdrive import HyperDriveConfig

hyperdrive_run_config = HyperDriveConfig(estimator=estimator,
                          hyperparameter_sampling=param_sampling, 
                          policy=early_termination_policy,
                          resume_from=warmstart_parents_to_resume_from, 
                          resume_child_runs=child_runs_to_resume,
                          primary_metric_name="accuracy", 
                          primary_metric_goal=PrimaryMetricGoal.MAXIMIZE,
                          max_total_runs=100,
                          max_concurrent_runs=4)
```

## <a name="visualize-experiment"></a>Visualisieren des Experiments

Das Azure Machine Learning SDK stellt ein [Notebook-Widget](https://docs.microsoft.com/python/api/azureml-widgets/azureml.widgets.rundetails?view=azure-ml-py) bereit, das den Fortschritt Ihrer Trainingsausführungen visualisiert. Der folgende Codeausschnitt visualisiert alle Läufe zur Hyperparameteroptimierung in einem Jupyter Notebook:

```Python
from azureml.widgets import RunDetails
RunDetails(hyperdrive_run).show()
```

Dieser Code zeigt eine Tabelle mit Details zu den Trainingsläufen für jede der Hyperparameterkonfigurationen an.

![Optimierungstabelle für Hyperparameter](media/how-to-tune-hyperparameters/HyperparameterTuningTable.png)

Ferner können Sie die Leistung jedes dieser Läufe während des fortschreitenden Trainings visualisieren. 

![Hyperparameter-Optimierungsplot](media/how-to-tune-hyperparameters/HyperparameterTuningPlot.png)

Außerdem können Sie die Korrelation zwischen der Leistung und den Werten einzelner Hyperparameter mithilfe eines Plots mit parallelen Koordinaten visuell darstellen. 

[![Parallele Koordinaten einer Hyperparameteroptimierung](media/how-to-tune-hyperparameters/HyperparameterTuningParallelCoordinates.png)](media/how-to-tune-hyperparameters/hyperparameter-tuning-parallel-coordinates-expanded.png)

Alternativ können Sie alle Läufe zur Hyperparameteroptimierung auch im Azure-Webportal visualisieren. Weitere Informationen zum Anzeigen von Experimenten im Webportal finden Sie unter [Nachverfolgen von Experimenten](how-to-track-experiments.md#view-the-experiment-in-the-web-portal).

## <a name="find-the-best-model"></a>Ermitteln des besten Modells

Nach Abschluss aller Ausführungen der Hyperparameteroptimierung [ermitteln Sie die Konfiguration mit der besten Leistung](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.hyperdrive.hyperdriverun?view=azure-ml-py#get-best-run-by-primary-metric-include-failed-true--include-canceled-true-) und die entsprechenden Hyperparameterwerte:

```Python
best_run = hyperdrive_run.get_best_run_by_primary_metric()
best_run_metrics = best_run.get_metrics()
parameter_values = best_run.get_details()['runDefinition']['Arguments']

print('Best Run Id: ', best_run.id)
print('\n Accuracy:', best_run_metrics['accuracy'])
print('\n learning rate:',parameter_values[3])
print('\n keep probability:',parameter_values[5])
print('\n batch size:',parameter_values[7])
```

## <a name="sample-notebook"></a>Beispielnotebook
Weitere Informationen finden Sie in den Notebooks zum Trainieren von Hyperparametern im folgenden Ordner:
* [how-to-use-azureml/training-with-deep-learning](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning)

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Nächste Schritte
* [Nachverfolgen eines Experiments](how-to-track-experiments.md)
* [Bereitstellen eines trainierten Modells](how-to-deploy-and-where.md)
