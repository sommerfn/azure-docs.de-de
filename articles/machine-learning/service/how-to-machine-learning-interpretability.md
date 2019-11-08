---
title: Modellinterpretierbarkeit in Azure Machine Learning Service
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie mithilfe des Azure Machine Learning SDK erklären können, warum Ihr Modell Vorhersagen trifft. Es kann während des Trainings und der Herleitung von Rückschlüssen verwendet werden, um zu verstehen, wie Ihr Modell Vorhersagen trifft.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: mesameki
author: mesameki
ms.reviewer: trbye
ms.date: 10/25/2019
ms.openlocfilehash: 5f1008e8fcbbf7b82a694fd151a9dea9ca7f001e
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73511055"
---
# <a name="model-interpretability-in-azure-machine-learning-service"></a>Modellinterpretierbarkeit in Azure Machine Learning Service
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

## <a name="overview-of-model-interpretability"></a>Übersicht über die Modellinterpretierbarkeit

Die Interpretierbarkeit ist wichtig für Data Scientists und Entscheidungsträger im Unternehmen, um die Einhaltung von Unternehmensrichtlinien, Industriestandards und behördlichen Vorschriften zu gewährleisten:
+ Data Scientists müssen die Möglichkeit haben, ihre Modelle für Führungskräfte und Projektbeteiligte zu erklären, damit diese den Wert und die Genauigkeit ihrer Ergebnisse verstehen können. 
+ Entscheidungsträger im Unternehmen müssen zu ihrer Entlastung Transparenz für Endbenutzer bereitstellen können, um sich ihr Vertrauen dauerhaft zu erarbeiten.

Die Möglichkeit, ein Machine Learning-Modell erklären zu können, ist in zwei Hauptphasen der Modellentwicklung wichtig:
+ Während der Trainingsphase des Machine Learning-Modellentwicklungszyklus. Modellentwickler und -bewerter können die Interpretierbarkeitsausgabe eines Modells verwenden, um Hypothesen zu überprüfen und Vertrauen bei den Beteiligten aufzubauen. Sie verwenden außerdem die Erkenntnisse aus dem Modell zum Debuggen, zum Überprüfen des Modellverhaltens auf die Erfüllung der Ziele und zum Suchen nach Trends oder nicht signifikanten Features.
+ Während der Rückschlussphase können Führungskräfte bei bereitgestellten Modellen mithilfe von Transparenz im Zusammenhang mit der Modellbereitstellung wissen, wie das Modell bei der Bereitstellung funktioniert und wie sich seine Entscheidungen in echten Situationen auf Menschen auswirken. 

## <a name="interpretability-with-azure-machine-learning"></a>Interpretierbarkeit mit Azure Machine Learning

In diesem Artikel erfahren Sie, wie Konzepte der Modellinterpretierbarkeit im SDK implementiert werden.

Durch Verwenden der Klassen und Methoden im SDK erhalten Sie Folgendes:
+ Relevanzwerte sowohl für unformatierte als auch für verarbeitete Merkmale
+ Interpretierbarkeit realer Datasets in beliebigem Umfang während des Trainings und in der Rückschlussphase
+ Interaktive Visualisierungen, die Ihnen bei der Erkennung von Muster in Daten und bei der Erklärung zum Zeitpunkt der Schulung helfen


Beim maschinellen Lernen sind **Merkmale** diejenigen Datenfelder, die zum Vorhersagen eines Zieldatenpunkts verwendet werden. Um beispielsweise ein Kreditrisiko vorherzusagen, werden möglicherweise Datenfelder für Alter, Kontogröße und Kontoalter verwendet. In diesem Fall sind Alter, Kontogröße und Kontoalter **Merkmale**. Die Relevanz eines Merkmals informiert darüber, wie sich jedes Datenfeld auf die Vorhersagen des Modells ausgewirkt hat. Das Alter spielt möglicherweise bei der Vorhersage eine schwerer wiegende Rolle, wohingegen Kontogröße und Kontoalter sich nur unwesentlich auf die Vorhersagegenauigkeit auswirken. Dank dieses Prozesses können Data Scientists die getroffenen Vorhersagen erklären, sodass alle Beteiligten einen Einblick darin erhalten, welche Datenpunkte die wichtigsten im Modell sind.

Verwenden Sie diese Tools mit benutzerfreundlichen und skalierbaren modernsten Technologien, um Machine Learning-Modelle **global für alle Daten** oder **lokal für einen bestimmten Datenpunkt** zu erklären.

Die Interpretierbarkeitsklassen werden in mehreren SDK-Paketen zur Verfügung gestellt. Erfahren Sie, wie Sie [SDK-Pakete für Azure Machine Learning installieren](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).

* `azureml.interpret`, das Hauptpaket, enthält von Microsoft unterstützte Funktionen.

* `azureml.contrib.interpret`: Vorschau- und experimentelle Funktionen, die Sie ausprobieren können.

* `azureml.train.automl.automlexplainer`-Paket zum Interpretieren automatisierter Machine Learning-Modelle.

> [!IMPORTANT]
> Der Inhalt im `contrib`-Namespace wird nicht vollständig unterstützt. Sobald die experimentellen Funktionen ausgereift sind, werden sie schrittweise in den Hauptnamespace verlagert.

## <a name="how-to-interpret-your-model"></a>Interpretieren des Modells

Die Interpretierbarkeitsklassen und -methoden können angewandt werden, um das globale Verhalten des Modells oder bestimmte Vorhersagen zu verstehen. Erstere wird als globale Erklärung und letztere als lokale Erklärung bezeichnet.

Die Methoden können auch danach kategorisiert werden, ob die Methode modellunabhängig oder modellspezifisch ist. Einige Methoden zielen auf bestimmte Arten von Modellen ab. Der Tree Explainer von SHAP eignet sich nur für baumstrukturbasierte Modelle. Einige Methoden behandeln das Modell als Blackbox, wie z.B. der Mimic Explainer oder der Kernel Explainer von SHAP. Das Paket `interpret` verfolgt diese verschiedenen Ansätze basierend auf Datasets, Modelltypen und Anwendungsfällen.

Die Ausgabe ist ein Satz von Informationen dazu, wie ein angegebenes Modell Vorhersagen trifft, z. B.:
* Global/lokale relative Featurepriorität
* Global/lokale Beziehung zwischen Feature und Vorhersage

### <a name="explainers"></a>Explainer

Dieses Paket verwendet die Interpretierbarkeitstechniken, die in [Interpret-Community](https://github.com/interpretml/interpret-community/) entwickelt wurden, einem Open-Source-Python-Paket zum Trainieren von interpretierbaren Modellen und zur Unterstützung bei der Erläuterung von Blackbox-KI-Systemen. [Interpret-Community](https://github.com/interpretml/interpret-community/) fungiert als Host für die unterstützten Explainer dieses SDK und unterstützt derzeit die folgenden Interpretierbarkeitstechniken:

* **SHAP Tree Explainer**: Der Tree Explainer von [SHAP](https://github.com/slundberg/shap), der sich auf den schnellen Wertschätzalgorithmus von SHAP für die Polynomialzeit konzentriert, der für Baumstrukturen und Gruppen von Baumstrukturen spezifisch ist.
* **SHAP Deep Explainer**: Basierend auf der Erläuterung von [SHAP](https://github.com/slundberg/shap) ist Deep Explainer „ein schneller Näherungsalgorithmus für SHAP-Werte in Deep-Learning-Modellen, der auf einer im [SHAP NIPS-Papier](https://papers.nips.cc/paper/7062-a-unified-approach-to-interpreting-model-predictions) beschriebenen Verbindung mit DeepLIFT aufbaut. TensorFlow- und Keras-Modelle, die das TensorFlow-Back-End verwenden, werden unterstützt (es gibt auch eine vorläufige Unterstützung für PyTorch)“.
* **SHAP Linear Explainer:** Der Linear Explainer von [SHAP](https://github.com/slundberg/shap) berechnet SHAP-Werte für ein lineares Modell, wobei optional die Korrelation zwischen Features berücksichtigt wird.

* **SHAP Kernel Explainer**: Der Kernel Explainer von [SHAP](https://github.com/slundberg/shap) verwendet eine speziell gewichtete lokale lineare Regression, um SHAP-Werte für jedes Modell zu schätzen.
* **Mimic Explainer**: Der Mimic Explainer basiert auf der Idee des Trainierens von [globalen Surrogatmodellen](https://christophm.github.io/interpretable-ml-book/global.html) zur Nachahmung von Blackboxmodellen. Ein globales Surrogatmodell ist ein eigenständig interpretierbares Modell, das trainiert wird, um die Vorhersagen eines Blackbox-Modells so genau wie möglich zu treffen. Der Data Scientist kann das Surrogatmodell interpretieren, um Rückschlüsse in Bezug auf das Blackbox-Modell zu ziehen. Sie können eines der folgenden interpretierbaren Modelle als Surrogatmodell verwenden: LightGBM (LGBMExplainableModel), Linear Regression (LinearExplainableModel), Stochastic Gradient Descent explainable model (SGDExplainableModel) und Decision Tree (DecisionTreeExplainableModel).


* **Permutation Feature Importance Explainer**: Permutation Feature Importance ist ein Verfahren zum Erklären von Klassifizierungs- und Regressionsmodellen, das auf dem [Random Forests Paper von Breiman](https://www.stat.berkeley.edu/%7Ebreiman/randomforest2001.pdf) (siehe Abschnitt 10) basiert. Die grobe Funktionsweise besteht darin, nach dem Zufallsprinzip die Daten des gesamten Datasets für jeweils ein Feature zu vermischen und dann zu berechnen, wie stark sich die gewünschte Leistungsmetrik ändert. Je größer die Änderung, desto wichtiger ist das Feature.

* **LIME Explainer** (`contrib`): Der auf [LIME](https://github.com/marcotcr/lime) basierende LIME Explainer verwendet den hochmodernen LIME-Algorithmus (Local Interpretable Model-Agnostic Explanations), um lokale Surrogatmodelle zu erstellen. Im Gegensatz zu den globalen Surrogatmodellen konzentriert sich LIME auf das Training lokaler Surrogatmodelle, um individuelle Vorhersagen zu erklären.
* **HAN Text Explainer** (`contrib`): Der HAN Text Explainer verwendet ein Hierarchical Attention Network, um Modellerklärungen aus Textdaten zu einem bestimmten Blackbox-Textmodell zu erhalten. Er trainiert das HAN-Surrogatmodell anhand der vorhergesagten Ergebnisse eines gegebenen Blackboxmodells. Nach einem globalen Training über den gesamten Textkorpus fügt er einen Optimierungsschritt für ein spezifisches Dokument hinzu, um die Genauigkeit der Erklärungen zu verbessern. HAN verwendet ein rekurrentes neuronales Netz (RNN) mit zwei Aufmerksamkeitsebenen hinsichtlich Satz und Wort. Sobald das DNN (Deep Neural Network) am Blackboxmodell trainiert und für ein bestimmtes Dokument optimiert wurde, kann der Benutzer die Wichtigkeiten von Wörtern aus den Aufmerksamkeitsebenen extrahieren. HAN ist nachweisbar bei Textdaten genauer als LIME oder SHAP, aber auch aufwendiger in Bezug auf die Trainingszeit. Es wurden Verbesserungen vorgenommen, sodass Benutzer das Netzwerk mit GloVe-Worteinbettungen initialisieren können, um die Trainingszeit zu verkürzen. Die Trainingszeit kann durch den Einsatz von HAN auf einer Azure GPU-Remote-VM deutlich verbessert werden. [„Hierarchical Attention Networks for Document Classification (Yang u. a., 2016)“](https://www.researchgate.net/publication/305334401_Hierarchical_Attention_Networks_for_Document_Classification) enthält eine Beschreibung der Implementierung von HAN.


* **Tabular Explainer:** `TabularExplainer` verwendet die folgende Logik, um die direkten [SHAP](https://github.com/slundberg/shap)-Explainer aufzurufen:

    1. Wenn es sich um ein baumstrukturbasiertes Modell handelt, SHAP `TreeExplainer` anwenden, andernfalls:
    2. Wenn es sich um ein DNN-Modell handelt, SHAP `DeepExplainer` anwenden, andernfalls:
    3. Wenn es sich um ein lineares Modell handelt, SHAP `LinearExplainer` anwenden, andernfalls:
    3. Als Blackbox-Modell behandeln und SHAP `KernelExplainer` anwenden


`TabularExplainer` hat auch signifikante Feature- und Leistungsverbesserungen gegenüber den direkten SHAP-Explainern zu verzeichnen:

* **Zusammenfassung des Initialisierungsdatasets**. In Fällen, in denen die Erklärungsgeschwindigkeit am wichtigsten ist, fassen wir das Initialisierungsdataset zusammen und erzeugen einen kleinen Satz repräsentativer Stichproben, was sowohl die globale als auch die lokale Erklärung beschleunigt.
* **Stichprobenentnahme im Auswertungsdataset**. Wenn der Benutzer eine Vielzahl von Auswertungsstichproben übergibt, aber nicht alle davon ausgewertet werden müssen, kann der Stichprobenparameter auf TRUE festgelegt werden, um die globale Erklärung zu beschleunigen.

Im folgenden Diagramm ist die aktuelle Struktur von direkten Explainers und Meta Explainers dargestellt.

[![Architektur von Machine Learning Interpretability](./media/machine-learning-interpretability-explainability/interpretability-architecture.png)](./media/machine-learning-interpretability-explainability/interpretability-architecture.png#lightbox)


### <a name="models-supported"></a>Unterstützte Modelle

Alle Modelle, die mit Datasets in den Python-Formaten `numpy.array`, `pandas.DataFrame`, `iml.datatypes.DenseData` oder `scipy.sparse.csr_matrix` trainiert wurden, werden vom Paket `explain` für Interpretierbarkeit im SDK unterstützt.

Die Erklärungsunktionen akzeptieren sowohl Modelle als auch Pipelines als Eingabe. Wenn ein Modell bereitgestellt wird, muss das Modell die Vorhersagefunktion `predict` oder `predict_proba` implementieren, die der Scikit-Konvention entspricht. Wenn eine Pipeline (Name des Pipelineskripts) angegeben wird, nimmt die Erklärungsfunktion an, dass das ausgeführte Pipelineskript eine Vorhersage zurückgibt. Wir unterstützen Modelle, die mit PyTorch-, TensorFlow- und Keras-Deep Learning-Frameworks trainiert werden.

### <a name="local-and-remote-compute-target"></a>Lokales- und Remotecomputeziel

Das Paket `explain` funktioniert mit lokalen und Remotecomputezielen. Bei lokaler Ausführung werden über die SDK-Funktionen keine Verbindungen mit Azure-Diensten hergestellt. Sie können die Erklärung per Remotezugriff in Azure Machine Learning Compute ausführen und die Erklärungsinformationen im Ausführungsverlauf von Azure Machine Learning Services protokollieren. Nachdem diese Informationen protokolliert wurden, stehen Berichte und Visualisierungen aus der Erklärung im Portal mit den Azure Machine Learning-Arbeitsbereichen für die Analyse durch den Benutzer bereit.


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Aktivierung der Interpretierbarkeit für das Modelltraining, sowohl lokal als auch auf Azure Machine Learning-Remotecomputeressourcen, finden Sie in der [Schrittanleitung](how-to-machine-learning-interpretability-aml.md). Weitere Szenarien finden Sie in den [Beispielnotebooks](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/explain-model/tabular-data).
