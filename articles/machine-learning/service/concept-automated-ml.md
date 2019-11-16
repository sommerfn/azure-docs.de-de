---
title: Was ist automatisiertes maschinelles Lernen?/ automl
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Azure Machine Learning automatisch einen Algorithmus für Sie auswählen und ein Modell daraus generieren kann, um Ihnen Zeit zu sparen, indem er die von Ihnen angegebenen Parameter und Kriterien verwendet, um den besten Algorithmus für Ihr Modell auszuwählen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: cartacioS
ms.author: sacartac
ms.date: 11/04/2019
ms.openlocfilehash: f8a83fccefe3310fe1a582ef44d72cfbef7e9469
ms.sourcegitcommit: 2d3740e2670ff193f3e031c1e22dcd9e072d3ad9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2019
ms.locfileid: "74133075"
---
# <a name="what-is-automated-machine-learning"></a>Was ist automatisiertes maschinelles Lernen?

Automatisiertes maschinelles Lernen, auch als automatisiertes ML bezeichnet, ist der Prozess des Automatisierens der zeitaufwändigen, iterativen Aufgaben der Entwicklung eines Machine Learning-Modells. Es versetzt Data Scientists, Analysten und Entwickler in die Lage, ML-Modelle mit hoher Skalierbarkeit, Effizienz und Produktivität zu erstellen und gleichzeitig die Modellqualität zu erhalten. Automatisiertes Machine Learning basiert auf einem Durchbruch der [Microsoft Research-Abteilung](https://arxiv.org/abs/1705.05355).

Die Entwicklung traditioneller Machine Learning-Modelle ist ressourcenintensiv und erfordert viel Fachwissen und Zeit, um Dutzende von Modellen zu erstellen und zu vergleichen. Sie arbeiten mit automatisiertem ML, wenn Sie möchten, dass Azure Machine Learning mit der von Ihnen angegebenen Zielmetrik ein Modell für Sie trainiert und optimiert. Der Dienst durchläuft die ML-Algorithmen dann iterativ im Zusammenspiel mit Featureauswahlen, wobei für jede Iteration ein Modell mit einer Trainingsbewertung erzeugt wird. Je höher die Bewertung ist, desto besser wird das Modell als „passend“ für Ihre Daten angesehen.

Mit automatisiertem maschinellem Lernen verkürzen Sie die Zeit, die benötigt wird, um produktionsbereite ML-Modelle mit großer Leichtigkeit und Effizienz zu erhalten.

## <a name="when-to-use-automated-ml"></a>Nutzung von automatisiertem Machine Learning

Automatisiertes ML demokratisiert den Entwicklungsprozess eines Machine Learning-Modells und befähigt seine Benutzer, unabhängig von deren Data Science-Kenntnissen, eine durchgängige Machine Learning-Pipeline für jedes Problem zu bestimmen.

Data Scientists, Analysten und Entwickler aus den verschiedensten Branchen können automatisiertes ML für Folgendes verwenden:

+ Implementieren von Machine Learning-Lösungen ohne umfangreiche Programmierkenntnisse
+ Sparen von Zeit und Ressourcen
+ Nutzen von bewährten Methoden aus der Data Science
+ Bereitstellen flexibler Problemlösungen

In der folgenden Tabelle sind gängige Anwendungsfälle für automatisiertes maschinelles Lernen aufgeführt. 

Classification| Regression | Zeitreihenvorhersage
---|---|---
[Betrugserkennung](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-credit-card-fraud/auto-ml-classification-credit-card-fraud.ipynb)|[CPU-Leistungsvorhersage](https://github.com/Azure/MachineLearningNotebooks/blob/37541b10714f9337dbbae721bea494272dc7d151/how-to-use-azureml/automated-machine-learning/regression-hardware-performance-explanation-and-featurization/auto-ml-regression-hardware-performance-explanation-and-featurization.ipynb) |[Bedarfsvorhersage](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand/auto-ml-forecasting-energy-demand.ipynb)|
[Marketingvorhersage](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-bank-marketing-all-features/auto-ml-classification-bank-marketing-all-features.ipynb)|[Vorhersage der Materialdauerhaftigkeit](https://github.com/Azure/MachineLearningNotebooks/blob/37541b10714f9337dbbae721bea494272dc7d151/how-to-use-azureml/automated-machine-learning/regression-concrete-strength/auto-ml-regression-concrete-strength.ipynb)|[Verkaufsvorhersage](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-orange-juice-sales/auto-ml-forecasting-orange-juice-sales.ipynb)

## <a name="how-automated-ml-works"></a>Funktionsweise von automatisiertem ML

Mithilfe von **Azure Machine Learning** können Sie automatisierte ML-Trainingsexperimente mit den folgenden Schritten entwerfen und ausführen:

1. **Identifizieren des ML-Problems**, das gelöst werden soll: Klassifizierung, Vorhersage oder Regression.

1. **Angeben der Quelle und des Formats der bezeichneten Trainingsdaten**: NumPy-Arrays oder Pandas-Datenrahmen.

1. **Konfigurieren des Computeziels für das Modelltraining**, z.B. [lokaler Computer, Azure Machine Learning Computes, Remote-VMs oder Azure Databricks](how-to-set-up-training-targets.md).  Weitere Informationen zu automatisiertem Training [für eine Remoteressource](how-to-auto-train-remote.md).

1. **Konfigurieren der automatisierten Machine Learning-Parameter**, die die Anzahl der Iterationen über verschiedene Modelle, die Hyperparametereinstellungen, erweiterte Vorverarbeitung/Featurebereitstellung und die Metriken bestimmen, die bei der Ermittlung des besten Modells zu berücksichtigen sind.  Sie können die Einstellungen für ein automatisches Trainingsexperiment [in Azure Machine Learning Studio](https://ml.azure.com) oder [mit dem SDK](how-to-configure-auto-train.md) konfigurieren. 

    [!INCLUDE [aml-applies-to-enterprise-sku](../../../includes/aml-applies-to-enterprise-sku-inline.md)]

1. **Übermitteln der Trainingsausführung.**

  ![Automatisiertes maschinelles Lernen](./media/how-to-automated-ml/automl-concept-diagram2.png)

Während des Trainings erstellt Azure Machine Learning eine Reihe von parallelen Pipelines, die unterschiedliche Algorithmen und Parametern ausprobieren. Die Ausführung wird beendet, sobald die im Experiment definierten Beendigungskriterien erreicht werden.

Sie können die protokollierten Ausführungsinformationen auch untersuchen. Sie enthalten die während der Ausführung [erfassten Metriken](how-to-understand-automated-ml.md). Bei der Trainingsausführung wird ein serialisiertes Python-Objekt (`.pkl`-Datei) generiert, das die Vorabverarbeitung des Modells und der Daten enthält.

Obwohl die Modellerstellung automatisiert ist, können Sie auch [ermitteln, wie wichtig oder relevant Features für die generierten Modelle sind](how-to-configure-auto-train.md#explain).

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2Xc9t]

<a name="preprocess"></a>

## <a name="preprocessing"></a>Vorverarbeitung

In jedem automatisierten Machine Learning-Experiment werden Ihre Daten mit den Standardmethoden und optional durch eine erweiterte Vorverarbeitung vorverarbeitet.

> [!NOTE]
> Die Schritte zur Vorverarbeitung bei automatisiertem maschinellen Lernen (Featurenormalisierung, Behandlung fehlender Daten, Umwandlung von Text in numerische Daten usw.) werden Teil des zugrunde liegenden Modells. Bei Verwendung des Modells für Vorhersagen werden die während des Trainings angewendeten Vorverarbeitungsschritte automatisch auf Ihre Eingabedaten angewendet.

### <a name="automatic-preprocessing-standard"></a>Automatische Vorverarbeitung (Standard)

In jedem automatisierten Machine Learning-Experiment werden Ihre Daten automatisch skaliert oder normalisiert, damit die Algorithmen gut funktionieren.  Während des Modelltrainings wird eine der folgenden Skalierungs- oder Normalisierungstechniken auf jedes Modell angewendet.

|Skalierung&nbsp;&&nbsp;Normalisierung| BESCHREIBUNG |
| ------------- | ------------- |
| [StandardScaleWrapper](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html)  | Standardisieren von Features durch Entfernen des Mittelwerts und Skalierung auf Einheitenvarianz.  |
| [MinMaxScalar](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MinMaxScaler.html)  | Transformieren von Features durch Skalieren jedes Features anhand des Mindest- und Maximalwerts dieser Spalte.  |
| [MaxAbsScaler](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MaxAbsScaler.html#sklearn.preprocessing.MaxAbsScaler) |Skalieren jedes Features anhand seines maximalen Absolutwerts. |
| [RobustScalar](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.RobustScaler.html) |Diese Skalarfunktionen skaliert Features nach ihren Quantilbereich. |
| [PCA](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html) |Lineare Dimensionalitätsreduzierung unter Verwendung von Singulärwertzerlegung der Daten, um sie auf einen niedrigeren Dimensionsbereich zu projizieren. |
| [TruncatedSVDWrapper](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.TruncatedSVD.html) |Diese Transformation führt Dimensionalitätsreduzierung durch abgeschnittene Singulärwertzerlegung (SVD) durch. Im Gegensatz zu PCA zentriert diese Schätzfunktion die Daten nicht vor der Berechnung der Singulärwertzerlegung. Dies bedeutet, dass sie mit scipy.sparse-Matrizen effizient arbeiten kann. |
| [SparseNormalizer](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.Normalizer.html) | Jede Stichprobe (d.h. jede Zeile der Datenmatrix) mit mindestens einer Komponente ungleich Null wird unabhängig von anderen Stichproben neu skaliert, sodass ihre Norm (l1 oder l2) gleich eins ist. |

### <a name="advanced-preprocessing-optional-featurization"></a>Erweiterte Vorverarbeitung: optionale Featurebereitstellung

Zusätzliche erweiterte Vorverarbeitung und Featurebereitstellung sind ebenfalls verfügbar, z.B. Imputation fehlender Werte, Codierung und Transformationen. [Weitere Informationen zur enthaltenen Featurebereitstellung](how-to-create-portal-experiments.md#preprocess). Diese Einstellung kann aktiviert werden über:

+ Azure Machine Learning Studio: Auswählen von **Einstellungen für die Merkmalserstellung anzeigen** im Abschnitt **Configuration Run** (Konfigurationsausführung) [über diese Schritte](how-to-create-portal-experiments.md).

+ Python SDK: Angeben von `"feauturization": auto' / 'off' / FeaturizationConfig` für die [`AutoMLConfig`-Klasse](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlconfig?view=azure-ml-py).


## <a name="time-series-forecasting"></a>Zeitreihenvorhersagen
Die Erstellung von Vorhersagen ist ein integraler Bestandteil jedes Unternehmens, unabhängig davon, ob es sich um Einnahmen, Lagerbestände, Umsätze oder Kundennachfrage handelt. Sie können automatisiertes maschinelles Lernen verwenden, um verschiedene Techniken und Ansätze zu kombinieren. Außerdem erhalten Sie dabei eine beliebte und hochwertige Zeitreihenprognose.

Automatisierte Zeitreihenexperimente werden als multivariate Regressionsprobleme behandelt. Zeitreihenwerte aus der Vergangenheit werden pivotiert und dienen so zusammen mit anderen Vorhersageelementen als zusätzliche Dimensionen für den Regressor. Dieser Ansatz hat im Gegensatz zu klassischen Zeitreihenmethoden den Vorteil, dass mehrere kontextbezogene Variablen und deren Beziehungen zueinander beim Training auf natürliche Weise integriert werden. Beim automatisierten maschinellen Lernen wird ein zwar einfaches, aber häufig in interne Verzweigungen unterteiltes Modell für alle Elemente im Dataset und in den Vorhersagehorizonten erlernt. Dadurch sind mehr Daten verfügbar, um Modellparameter zu schätzen, und die Generalisierung von unbekannten Reihen wird möglich.

Hier erhalten Sie weitere Informationen und ein Beispiel zum [automatisierten maschinellen Lernen für die Zeitreihenvorhersage](how-to-auto-train-forecast.md). Oder sehen Sie sich das Notebook zu Energiebedarf ([auto-ml-forecasting-energy-demand.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand/auto-ml-forecasting-energy-demand.ipynb)) an. Dort finden Sie ausführliche Codebeispiele zu einer erweiterten Vorhersagekonfiguration, einschließlich:

* Feiertagserkennung und Erstellen zusätzlicher Merkmale (Featurization)
* Zeitreihen und DNN-Lernmodule (Auto-ARIMA, Prophet, ForecastTCN)
* Unterstützung vieler Modelle mit Gruppierungen
* Kreuzvalidierung mit rollierendem Ursprung (Rolling Origin Validation)
* Konfigurierbare Verzögerungen (Lags)
* Aggregierte Zeitfenstermerkmale (Rolling Window Features)

## <a name="ensemble"></a> Ensemblemodelle

Automatisiertes Machine Learning unterstützt Ensemblemodelle, die standardmäßig aktiviert sind. Das Lernen mit Ensembles verbessert die Ergebnisse des maschinellen Lernens und die Vorhersageleistung, da nicht einzelne Modelle verwendet, sondern mehrere Modelle kombiniert werden. Die Ensemble-Iterationen erfolgen als abschließende Iterationen Ihrer Ausführung. Automatisiertes Machine Learning verwendet die beiden Ensemble-Methoden „voting“ (Abstimmen) und „stacking“ (Stapeln) gemeinsam, um Modelle zu kombinieren:

* **Voting**: Trifft Vorhersagen auf Grundlage des gewichteten Durchschnitts der vorhergesagten Klassenwahrscheinlichkeiten (für Klassifizierungsaufgaben) oder auf Grundlage der vorhergesagten Regressionsziele (für Regressionsaufgaben).
* **Stacking**: Stacking kombiniert heterogene Modelle und trainiert ein Metamodell, basierend auf der Ausgabe der einzelnen Modelle. Die aktuellen Standardmetamodelle sind LogisticRegression für Klassifizierungsaufgaben und ElasticNet für Regressions-/Vorhersageaufgaben.

Der [Caruana-Algorithmus für die Ensembleauswahl](http://www.niculescu-mizil.org/papers/shotgun.icml04.revised.rev2.pdf) mit sortierter Ensembleinitialisierung wird verwendet, um zu entscheiden, welche Modell innerhalb des Ensembles verwendet werden sollen. Generell initialisiert dieser Algorithmus das Ensemble mit bis zu 5 Modellen mit den besten Einzelbewertungen und überprüft, ob diese Modelle innerhalb des 5 %-Schwellenwerts der besten Bewertung liegen, um ein schlechtes Ausgangsensemble zu vermeiden. Dann wird für jede Ensemble-Iteration ein neues Modell zum vorhandenen Ensemble hinzugefügt, und die resultierende Bewertung wird berechnet. Wenn ein neues Modell die vorhandene Ensemblebewertung verbessert hat, wird das Ensemble so aktualisiert, dass es das neue Modell aufnimmt.

Informationen zum Ändern der Standard-Ensembleeinstellungen beim automatisierten Machine Learning finden Sie unter [Gewusst wie:](how-to-configure-auto-train.md#ensemble).

## <a name="imbalance"></a> Unausgeglichene Daten

Unausgeglichene Daten finden sich häufig in Daten für Klassifizierungsszenarien des maschinellen Lernens und beziehen sich auf Daten, die ein überproportionales Verhältnis von Beobachtungen in den einzelnen Klassen enthalten. Diese Unausgeglichenheit kann zu einem falsch wahrgenommenen positiven Effekt der Genauigkeit eines Modells führen, da die Eingabedaten eine Abweichung zu einer Klasse aufweisen, was dazu führt, dass das trainierte Modell diese Abweichung imitiert. 

Als Teil seines Ziels, den Workflow des maschinellen Lernens zu vereinfachen, verfügt das automatisierte maschinelle Lernen über integrierte Funktionen, die bei der Verarbeitung unausgeglichener Daten helfen. Beispiel: 

- Eine **Gewichtungsspalte**: Das automatisierte maschinelle Lernen unterstützt eine gewichtete Spalte als Eingabe, wodurch Zeilen in den Daten eine höhere oder niedrigere Gewichtung erhalten. Auf diese Weise wird eine Klasse mehr oder weniger „wichtig“. Schauen Sie sich dieses [Notebookbeispiel](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/sample-weight/auto-ml-sample-weight.ipynb) an. 

- Die vom automatisierten maschinellen Lernen verwendeten Algorithmen können eine Unausgeglichenheit von bis zu 20:1 verarbeiten. Das bedeutet, dass die häufigste Klasse 20 mal mehr Zeilen in den Daten aufweisen kann als die am wenigsten häufige Klasse.

### <a name="identify-models-with-imbalanced-data"></a>Identifizieren von Modellen mit unausgeglichenen Daten

Da Klassifizierungsalgorithmen häufig nach Genauigkeit bewertet werden, ist die Überprüfung der Genauigkeitsbewertung eines Modells eine geeignete Möglichkeit, um festzustellen, ob es von unausgeglichenen Daten beeinflusst wurde. Hat es für bestimmte Klassen über eine wirklich hohe oder über eine wirklich niedrige Genauigkeit verfügt?

Darüber hinaus generieren Durchläufe des automatisierten maschinellen Lernens automatisch die folgenden Diagramme, die Ihnen helfen können, die Richtigkeit der Klassifizierungen Ihres Modells zu verstehen und Modelle zu identifizieren, die möglicherweise von nicht ausgeglichenen Daten beeinflusst werden.

Diagramm| BESCHREIBUNG
---|---
[Konfusionsmatrix](how-to-understand-automated-ml.md#confusion-matrix)| Bewertet die ordnungsgemäß klassifizierten Bezeichnungen anhand der tatsächlichen Bezeichnungen der Daten. 
[Genauigkeit-Trefferquote](how-to-understand-automated-ml.md#precision-recall-chart)| Bewertet das Verhältnis der richtigen Bezeichnungen anhand des Verhältnisses der gefundenen Bezeichnungsinstanzen der Daten. 
[ROC-Kurven](how-to-understand-automated-ml.md#roc)| Bewertet das Verhältnis von richtigen Bezeichnungen anhand des Verhältnisses von falsch positiven Bezeichnungen.

### <a name="handle-imbalanced-data"></a>Behandeln von unausgeglichenen Daten 

Die folgenden Verfahren sind zusätzliche Optionen zum Behandeln von unausgeglichenen Daten außerhalb des automatisierten maschinellen Lernens. 

- Erneutes Sampling zum Ausgleichen der Unausgeglichenheit von Klassen, entweder durch Upsampling der kleineren Klassen oder durch Downsampling der größeren Klassen. Diese Methoden erfordern Fachwissen, um sie zu verarbeiten und zu analysieren.

- Verwenden Sie eine Leistungsmetrik, die besser mit unausgeglichenen Daten umgeht. So ist z. B. der F1-Score ein gewichteter Mittelwert aus Genauigkeit und Trefferquote. Die Genauigkeit misst die Präzision eines Klassifizierers. Eine niedrige Genauigkeit zeigt eine hohe Anzahl von falsch positiven Werten an. Die Trefferquote misst hingegen die Vollständigkeit eines Klassifizierers. Eine niedrige Trefferquote zeigt eine hohe Anzahl von falsch negativen Werten an. 

## <a name="use-with-onnx-in-c-apps"></a>Verwenden von ONNX in C#-Apps

Mit Azure Machine Learning können Sie automatisiertes ML verwenden, um ein Python-Modell zu erstellen und in das ONNX-Format zu konvertieren. Die ONNX-Runtime unterstützt C#, sodass Sie das erstellte Modell automatisch in Ihren C#-Apps verwenden können, ohne dass Sie es neu codieren oder die Netzwerklatenzen in Kauf nehmen müssen, die REST-Endpunkte mit sich bringen. Testen Sie ein Beispiel für diese Vorgehensweise [in diesem Jupyter Notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-with-onnx/auto-ml-classification-with-onnx.ipynb).

## <a name="automated-ml-across-microsoft"></a>Automatisiertes ML bei Microsoft

Automatisiertes ML ist auch in anderen Lösungen von Microsoft verfügbar:

|Integrationen|BESCHREIBUNG|
|------------|-----------|
|[ML.NET](https://docs.microsoft.com/dotnet/machine-learning/automl-overview)|Automatische Modellauswahl und Training in .NET-Apps mithilfe von Visual Studio und Visual Studio Code (mit Machine Learning mit Automatisierung per ML.NET (Vorschauversion)).|
|[HDInsight](../../hdinsight/spark/apache-spark-run-machine-learning-automl.md)|Sie können Ihre automatisierten ML-Trainingsaufträge in Spark in HDInsight-Clustern parallel horizontal hochskalieren.|
|[Power BI](https://docs.microsoft.com/power-bi/service-machine-learning-automated)|Sie können Machine Learning-Modelle direkt in Power BI (Vorschauversion) aufrufen.|
|[SQL Server](https://cloudblogs.microsoft.com/sqlserver/2019/01/09/how-to-automate-machine-learning-on-sql-server-2019-big-data-clusters/)|Erstellen Sie neue Modelle für das maschinelle Lernen über Ihre Daten in Big Data-Clustern von SQL Server 2019.|

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Modelle mithilfe des automatisierten maschinellen Lernens erstellt werden, und sehen Sie sich Beispiele dazu an:

+ [Tutorial: Automatisches Trainieren eines Regressionsmodell mithilfe von Azure Automated Machine Learning](tutorial-auto-train-models.md)

+ Konfigurieren Sie die Einstellungen für ein automatisches Trainingsexperiment:
  + Verwenden Sie in Azure Machine Learning Studio [diese Schritte](how-to-create-portal-experiments.md).
  + Mit dem Python SDK [verwenden Sie diese Schritte](how-to-configure-auto-train.md).

+ Erfahren Sie, wie automatisches Training mithilfe von Zeitreihendaten funktioniert, indem Sie [diese Schritte verwenden](how-to-auto-train-forecast.md).

+ Testen Sie [Jupyter Notebook-Beispiele für automatisiertes maschinelles Lernen](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/)
