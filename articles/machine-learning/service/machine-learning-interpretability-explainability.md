---
title: Interpretierbarkeit von Modellen
titleSuffix: Azure Machine Learning service
description: Erfahren Sie, wie Sie mithilfe des Azure Machine Learning SDK erklären können, warum Ihr Modell Vorhersagen trifft. Es kann während des Trainings und der Herleitung von Rückschlüssen verwendet werden, um zu verstehen, wie Ihr Modell Vorhersagen trifft.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: mesameki
author: mesameki
ms.reviewer: larryfr
ms.date: 06/21/2019
ms.openlocfilehash: 2e8eb79c4baebebb1974a977394215545ef944db
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2019
ms.locfileid: "69872386"
---
# <a name="model-interpretability-with-azure-machine-learning-service"></a>Modellinterpretierbarkeit mit Azure Machine Learning Service

In diesem Artikel erfahren Sie, wie Sie mithilfe der verschiedenen Interpretierbarkeitspakete des Python-SDK für Azure Machine Learning erklären können, wie Ihr Modell zu den getroffenen Vorhersagen gelangt.

Durch Verwenden der Klassen und Methoden im SDK erhalten Sie Folgendes:
+ Relevanzwerte sowohl für unformatierte als auch für verarbeitete Merkmale
+ Interpretierbarkeit realer Datasets in beliebigem Umfang während des Trainings und in der Rückschlussphase
+ Interaktive Visualisierungen, die Ihnen bei der Erkennung von Muster in Daten und bei der Erklärung zum Zeitpunkt der Schulung helfen

Während der Trainingsphase des Entwicklungszyklus können Modellentwickler und -bewerter die Interpretierbarkeitsausgabe eines Modells verwenden, um Hypothesen zu überprüfen und Vertrauen bei den Beteiligten aufzubauen.  Sie verwenden außerdem die Erkenntnisse aus dem Modell zum Debuggen, Überprüfen des Modellverhaltens auf die Erfüllung der Ziele und Suchen nach Trends.

Beim maschinellen Lernen sind **Merkmale** diejenigen Datenfelder, die zum Vorhersagen eines Zieldatenpunkts verwendet werden. Um beispielsweise ein Kreditrisiko vorherzusagen, werden möglicherweise Datenfelder für Alter, Kontogröße und Kontoalter verwendet. In diesem Fall sind Alter, Kontogröße und Kontoalter **Merkmale**. Die Relevanz eines Merkmals informiert darüber, wie sich jedes Datenfeld auf die Vorhersagen des Modells ausgewirkt hat. Das Alter spielt möglicherweise bei der Vorhersage eine schwerer wiegende Rolle, wohingegen Kontogröße und Kontoalter sich nur unwesentlich auf die Vorhersagegenauigkeit auswirken. Dank dieses Prozesses können Data Scientists die getroffenen Vorhersagen erklären, sodass alle Beteiligten einen Einblick darin erhalten, welche Datenpunkte die wichtigsten im Modell sind.

Verwenden Sie diese Tools mit benutzerfreundlichen und skalierbaren modernsten Technologien, um Machine Learning-Modelle **global für alle Daten** oder **lokal für einen bestimmten Datenpunkt** zu erklären.

Die Interpretierbarkeitsklassen werden in mehreren SDK-Paketen zur Verfügung gestellt. Erfahren Sie, wie Sie [SDK-Pakete für Azure Machine Learning installieren](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).

* [`azureml.explain.model`](https://docs.microsoft.com/python/api/azureml-explain-model/?view=azure-ml-py), das Hauptpaket, enthält von Microsoft unterstützte Funktionen.

* `azureml.contrib.explain.model`: Vorschau- und experimentelle Funktionen, die Sie ausprobieren können.

* `azureml.train.automl.automlexplainer`-Paket zum Interpretieren automatisierter Machine Learning-Modelle.

> [!IMPORTANT]
> Der Inhalt im `contrib`-Namespace wird nicht vollständig unterstützt. Sobald die experimentellen Funktionen ausgereift sind, werden sie schrittweise in den Hauptnamespace verlagert.

## <a name="how-to-interpret-your-model"></a>Interpretieren des Modells

Die Interpretierbarkeitsklassen und -methoden können angewandt werden, um das globale Verhalten des Modells oder bestimmte Vorhersagen zu verstehen. Erstere wird als globale Erklärung und letztere als lokale Erklärung bezeichnet.

Die Methoden können auch danach kategorisiert werden, ob die Methode modellunabhängig oder modellspezifisch ist. Einige Methoden zielen auf bestimmte Arten von Modellen ab. Der Tree Explainer von SHAP eignet sich nur für baumstrukturbasierte Modelle. Einige Methoden behandeln das Modell als Blackbox, wie z.B. der Mimic Explainer oder der Kernel Explainer von SHAP. Das Paket `explain` verfolgt diese verschiedenen Ansätze basierend auf Datasets, Modelltypen und Anwendungsfällen.

Die Ausgabe ist ein Satz von Informationen dazu, wie ein angegebenes Modell Vorhersagen trifft, z. B.:
* Global/lokale relative Featurepriorität

* Global/lokale Beziehung zwischen Feature und Vorhersage

### <a name="explainers"></a>Explainer

Es sind zwei Explainer vorhanden: Direkte Explainer und Metaexplainer im SDK.

__Direkte Explainer__ stammen aus integrierten Bibliotheken. Das SDK bündelt alle Explainer so, dass sie eine gemeinsame API und ein gemeinsames Ausgabeformat verfügbar machen. Wenn Sie diese Explainer direkt verwenden möchten, können Sie sie direkt aufrufen, anstatt die gemeinsame API und das gemeinsame Ausgabeformat zu verwenden. Es folgt eine Liste der direkten Explainer, die im SDK verfügbar sind:

* **SHAP Tree Explainer**: Der Tree Explainer von SHAP, der sich auf den schnellen Wertschätzalgorithmus von SHAP für die Polynomialzeit konzentriert, der für Baumstrukturen und Gruppen von Baumstrukturen spezifisch ist.
* **SHAP Deep Explainer**: Basierend auf der Erläuterung von SHAP ist Deep Explainer „ein schneller Näherungsalgorithmus für SHAP-Werte in Deep-Learning-Modellen, der auf einer im SHAP NIPS-Papier beschriebenen Verbindung mit DeepLIFT aufbaut. TensorFlow- und Keras-Modelle, die das TensorFlow-Back-End verwenden, werden unterstützt (es gibt auch eine vorläufige Unterstützung für PyTorch)“.
* **SHAP Kernel Explainer**: Der Kernel Explainer von SHAP verwendet eine speziell gewichtete lokale lineare Regression, um SHAP-Werte für jedes Modell zu schätzen.
* **Mimic Explainer**: Der Mimic Explainer basiert auf der Idee globaler Surrogatmodelle. Ein globales Surrogatmodell ist ein eigenständig interpretierbares Modell, das trainiert wird, um die Vorhersagen eines Blackbox-Modells so genau wie möglich zu treffen. Der Data Scientist kann das Surrogatmodell interpretieren, um Rückschlüsse in Bezug auf das Blackbox-Modell zu ziehen. Sie können eines der folgenden interpretierbaren Modelle als Surrogatmodell verwenden: LightGBM (LinearExplainableModel), Linear Regression (LinearExplainableModel), Stochastic Gradient Descent Explainable Model (SGDExplainableModel) und Decision Tree (DecisionTreeExplainableModel).


* **Permutation Feature Importance Explainer**: Permutation Feature Importance ist ein Verfahren zum Erklären von Klassifizierungs- und Regressionsmodellen, das auf dem [Random Forests Paper von Breiman](https://www.stat.berkeley.edu/%7Ebreiman/randomforest2001.pdf) (siehe Abschnitt 10) basiert. Die grobe Funktionsweise besteht darin, dass nach dem Zufallsprinzip die Daten des gesamten Datasets für jeweils ein Feature vermischt werden und dann berechnet wird, wie stark sich die gewünschte Leistungsmetrik verringert. Je größer die Änderung, desto wichtiger ist das Feature.

* **LIME Explainer** (`contrib`): Basierend auf LIME verwendet der LIME Explainer den hochmodernen LIME-Algorithmus (Local Interpretable Model-Agnostic Explanations), um lokale Surrogatmodelle zu erstellen. Im Gegensatz zu den globalen Surrogatmodellen konzentriert sich LIME auf das Training lokaler Surrogatmodelle, um individuelle Vorhersagen zu erklären.
* **HAN Text Explainer** (`contrib`): Der HAN Text Explainer verwendet ein Hierarchical Attention Network, um Modellerklärungen aus Textdaten zu einem bestimmten Blackbox-Textmodell zu erhalten. Wir trainieren das HAN-Surrogatmodell anhand der vorhergesagten Ergebnisse eines gegebenen Lehrermodells. Nach einem globalen Training über den gesamten Textkorpus haben wir einen Optimierungsschritt für ein spezifisches Dokument hinzugefügt, um die Genauigkeit der Erklärungen zu verbessern. HAN verwendet ein rekurrentes neuronales Netz (RNN) mit zwei Aufmerksamkeitsebenen hinsichtlich Satz und Wort. Sobald das DNN (Deep Neural Network) am Lehrermodell trainiert und für ein bestimmtes Dokument optimiert wurde, können wir die Wichtigkeiten von Worten aus den Aufmerksamkeitsebenen extrahieren. Wir haben festgestellt, dass HAN bei Textdaten genauer ist als LIME oder SHAP, aber auch aufwendiger in Bezug auf die Trainingszeit. Wir haben jedoch die Trainingszeit verbessert, indem wir dem Benutzer die Möglichkeit gegeben haben, das Netzwerk mit GloVe-Worteinbettungen zu initialisieren, obwohl es immer noch langsam ist. Die Trainingszeit kann durch den Einsatz von HAN auf einer Azure GPU-Remote-VM deutlich verbessert werden. [„Hierarchical Attention Networks for Document Classification (Yang u. a., 2016)“](https://www.researchgate.net/publication/305334401_Hierarchical_Attention_Networks_for_Document_Classification) enthält eine Beschreibung der Implementierung von HAN.


__Meta-Explainer__ wählen automatisch einen geeigneten direkten Explainer aus und generieren die besten Erklärungsinformationen basierend auf dem gegebenen Modell und den gegebenen Datasets. Die Meta-Explainer nutzen alle Bibliotheken (SHAP, LIME, Mimic usw.), die wir integriert oder entwickelt haben. Es folgt eine Liste der Meta-Explainer, die im SDK verfügbar sind:

* **Tabular Explainer**: Wird mit tabellarischen Datasets verwendet.
* **Text Explainer**: Wird mit Textdatasets verwendet.
* **Image Explainer**: Wird mit Bilddatasets verwendet.

Neben der Meta-Auswahl der direkten Explainer entwickeln Meta-Explainer zusätzliche Features mithilfe der zugrunde liegenden Bibliotheken und verbessern die Geschwindigkeit und Skalierbarkeit im Vergleich mit den direkten Explainern.

Derzeit verwendet `TabularExplainer` die folgende Logik, um die direkten SHAP Explainers aufzurufen:

1. Wenn es sich um ein baumstrukturbasiertes Modell handelt, sollten Sie SHAP `TreeExplainer` anwenden, andernfalls
2. Wenn es sich um eine DNN-Modell handelt, sollten Sie SHAP `DeepExplainer` anwenden, andernfalls
3. Als Blackbox-Modell behandeln und SHAP `KernelExplainer` anwenden

Die in `TabularExplainer` integrierte Intelligenz wird ausgefeilter werden, da mehr Bibliotheken in das SDK integriert werden und wir mehr über die Vor- und Nachteile der einzelnen Explainer erfahren.

`TabularExplainer` hat auch signifikante Feature- und Leistungsverbesserungen gegenüber den direkten Explainern zu verzeichnen:

* **Zusammenfassung des Initialisierungsdatasets**. In Fällen, in denen die Erklärungsgeschwindigkeit am wichtigsten ist, fassen wir das Initialisierungsdataset zusammen und erzeugen einen kleinen Satz repräsentativer Stichproben, was sowohl die globale als auch die lokale Erklärung beschleunigt.
* **Stichprobenentnahme im Auswertungsdataset**. Wenn der Benutzer eine Vielzahl von Auswertungsstichproben übergibt, aber nicht alle davon ausgewertet werden müssen, kann der Stichprobenparameter auf TRUE festgelegt werden, um die globale Erklärung zu beschleunigen.

Im folgenden Diagramm ist die aktuelle Struktur von direkten Explainers und Meta Explainers dargestellt.

[![Architektur von Machine Learning Interpretability](./media/machine-learning-interpretability-explainability/interpretability-architecture.png)](./media/machine-learning-interpretability-explainability/interpretability-architecture.png#lightbox)


### <a name="models-supported"></a>Unterstützte Modelle

Alle Modelle, die mit Datasets in den Python-Formaten `numpy.array`, `pandas.DataFrame`, `iml.datatypes.DenseData` oder `scipy.sparse.csr_matrix` trainiert wurden, werden vom Paket `explain` für Interpretierbarkeit im SDK unterstützt.

Die Erklärungsunktionen akzeptieren sowohl Modelle als auch Pipelines als Eingabe. Wenn ein Modell bereitgestellt wird, muss das Modell die Vorhersagefunktion `predict` oder `predict_proba` implementieren, die der Scikit-Konvention entspricht. Wenn eine Pipeline (Name des Pipelineskripts) angegeben wird, nimmt die Erklärungsfunktion an, dass das ausgeführte Pipelineskript eine Vorhersage zurückgibt. Wir unterstützen Modelle, die mit PyTorch-, TensorFlow- und Keras-Deep Learning-Frameworks trainiert werden.

### <a name="local-and-remote-compute-target"></a>Lokales- und Remotecomputeziel

Das Paket `explain` funktioniert mit lokalen und Remotecomputezielen. Bei lokaler Ausführung werden über die SDK-Funktionen keine Verbindungen mit Azure-Diensten hergestellt. Sie können die Erklärung per Remotezugriff in Azure Machine Learning Compute ausführen und die Erklärungsinformationen im Ausführungsverlauf von Azure Machine Learning Services protokollieren. Nachdem diese Informationen protokolliert wurden, stehen Berichte und Visualisierungen aus der Erklärung im Portal mit den Azure Machine Learning-Arbeitsbereichen für die Analyse durch den Benutzer bereit.

## <a name="interpretability-in-training"></a>Interpretierbarkeit beim Training

### <a name="train-and-explain-locally"></a>Lokales Trainieren und Erklären

1. Trainieren Sie Ihr Modell in einem lokalen Jupyter-Notebook.

    ```python
    # load breast cancer dataset, a well-known small dataset that comes with scikit-learn
    from sklearn.datasets import load_breast_cancer
    from sklearn import svm
    from sklearn.model_selection import train_test_split
    breast_cancer_data = load_breast_cancer()
    classes = breast_cancer_data.target_names.tolist()
    
    # split data into train and test
    from sklearn.model_selection import train_test_split
    x_train, x_test, y_train, y_test = train_test_split(breast_cancer_data.data,            
                                                        breast_cancer_data.target,  
                                                        test_size=0.2,
                                                        random_state=0)
    clf = svm.SVC(gamma=0.001, C=100., probability=True)
    model = clf.fit(x_train, y_train)
    ```

2. Rufen Sie den Explainer auf: Um ein Explainer-Objekt zu initialisieren, müssen Sie das Modell und einige Trainingsdaten an den Konstruktor des Explainers übergeben. Sie können optional auch Namen von Features und Ausgabeklassen übergeben (während der Klassifizierung), mit denen Ihre Erklärungen und Visualisierungen informativer gestaltet werden. Es folgen Beispiele für das Instanziieren eines Explainer-Objekts unter lokaler Verwendung von [TabularExplainer](https://docs.microsoft.com/python/api/azureml-explain-model/azureml.explain.model.tabularexplainer?view=azure-ml-py), [MimicExplainer](https://docs.microsoft.com/python/api/azureml-explain-model/azureml.explain.model.mimic.mimicexplainer?view=azure-ml-py) und [PFIExplainer](https://docs.microsoft.com/python/api/azureml-explain-model/azureml.explain.model.permutation.permutation_importance.pfiexplainer?view=azure-ml-py). `TabularExplainer` ruft einen der drei darunter liegenden SHAP Explainers (`TreeExplainer`, `DeepExplainer` oder `KernelExplainer`) auf und wählt automatisch den aus, der für Ihren Anwendungsfall am besten geeignet ist. Sie können jedoch jeden der drei zugrunde liegenden Explainer direkt aufrufen.

    ```python
    from azureml.explain.model.tabular_explainer import TabularExplainer
    # "features" and "classes" fields are optional
    explainer = TabularExplainer(model, 
                                 x_train, 
                                 features=breast_cancer_data.feature_names, 
                                 classes=classes)
    ```

    oder

    ```python
    from azureml.explain.model.mimic.mimic_explainer import MimicExplainer
    
    # you can use one of the following four interpretable models as a global surrogate to the black box model
    from azureml.explain.model.mimic.models.lightgbm_model import LGBMExplainableModel
    from azureml.explain.model.mimic.models.linear_model import LinearExplainableModel
    from azureml.explain.model.mimic.models.linear_model import SGDExplainableModel
    from azureml.explain.model.mimic.models.tree_model import DecisionTreeExplainableModel

    # "features" and "classes" fields are optional
    # augment_data is optional and if true, oversamples the initialization examples to improve surrogate model accuracy to fit original model.  Useful for high-dimensional data where the number of rows is less than the number of columns. 
    # max_num_of_augmentations is optional and defines max number of times we can increase the input data size.
    # LGBMExplainableModel can be replaced with LinearExplainableModel, SGDExplainableModel, or DecisionTreeExplainableModel
    explainer = MimicExplainer(model, 
                               x_train, 
                               LGBMExplainableModel, 
                               augment_data=True, 
                               max_num_of_augmentations=10, 
                               features=breast_cancer_data.feature_names, 
                               classes=classes)
    ```
   oder

    ```python
    from azureml.explain.model.permutation.permutation_importance import PFIExplainer 
    
    # "features" and "classes" fields are optional
    explainer = PFIExplainer(model, 
                             features=breast_cancer_data.feature_names, 
                             classes=classes)
    ```

3. Rufen Sie die Wichtigkeitswerte globaler Features ab.

    ```python
    # you can use the training data or the test data here
    global_explanation = explainer.explain_global(x_train)
    
    # if you used the PFIExplainer in the previous step, use the next line of code instead
    # global_explanation = explainer.explain_global(x_train, true_labels=y_test)

    # sorted feature importance values and feature names
    sorted_global_importance_values = global_explanation.get_ranked_global_values()
    sorted_global_importance_names = global_explanation.get_ranked_global_names()
    dict(zip(sorted_global_importance_names, sorted_global_importance_values))

    # alternatively, you can print out a dictionary that holds the top K feature names and values
    global_explanation.get_feature_importance_dict()
    ```

4. Abrufen der Wichtigkeitswerte lokaler Features: Verwenden Sie die folgenden Funktionsaufrufe, um eine einzelne Instanz oder eine Gruppe von Instanzen zu erklären. Beachten Sie, dass PFIExplainer keine lokalen Erklärungen unterstützt.

    ```python
    # explain the first data point in the test set
    local_explanation = explainer.explain_local(x_test[0])

    # sorted feature importance values and feature names
    sorted_local_importance_names = local_explanation.get_ranked_local_names()
    sorted_local_importance_values = local_explanation.get_ranked_local_values()
    ```

    oder

    ```python
    # explain the first five data points in the test set
    local_explanation = explainer.explain_local(x_test[0:4])

    # sorted feature importance values and feature names
    sorted_local_importance_names = local_explanation.get_ranked_local_names()
    sorted_local_importance_values = local_explanation.get_ranked_local_values()
    ```

### <a name="train-and-explain-remotely"></a>Remotetraining und -Erklärung

Sie können auf den verschiedenen von Azure Machine Learning Service unterstützten Computezielen trainieren. Das Beispiel in diesem Abschnitt zeigt, wie Sie dies mit einem Azure Machine Learning-Computeziel erfolgt.

1. Erstellen Sie ein Trainingsskript in einem lokalen Jupyter-Notebook (Beispiel: run_explainer.py).

    ```python
    from azureml.contrib.explain.model.explanation.explanation_client import ExplanationClient
    from azureml.core.run import Run

    run = Run.get_context()
    client = ExplanationClient.from_run(run)

    # write code to get and split your data into train and test sets here
    # write code to train your model here 

    # explain predictions on your local machine
    # "features" and "classes" fields are optional
    explainer = TabularExplainer(model, 
                                x_train, 
                                features=feature_names, 
                                classes=classes)

    # explain overall model predictions (global explanation)
    global_explanation = explainer.explain_global(x_test)
    
    # uploading global model explanation data for storage or visualization in webUX
    # the explanation can then be downloaded on any compute
    # multiple explanations can be uploaded
    client.upload_model_explanation(global_explanation, comment='global explanation: all features')
    # or you can only upload the explanation object with the top k feature info
    #client.upload_model_explanation(global_explanation, top_k=2, comment='global explanation: Only top 2 features')
    ```

2. Befolgen Sie die Anweisungen unter [Einrichten von Computezielen für das Modelltraining](how-to-set-up-training-targets.md#amlcompute), um eine Azure Machine Learning Compute-Instanz als Computeziel einzurichten und die Trainingsausführung zu übermitteln.

3. Laden Sie die Erklärung in Ihr lokales Jupyter-Notebook herunter.

    ```python
    from azureml.contrib.explain.model.explanation.explanation_client import ExplanationClient
    
    client = ExplanationClient.from_run(run)
    
    # get model explanation data
    explanation = client.download_model_explanation()
    # or only get the top k (e.g., 4) most important features with their importance values
    explanation = client.download_model_explanation(top_k=4)
    
    global_importance_values = explanation.get_ranked_global_values()
    global_importance_names = explanation.get_ranked_global_names()
    print('global importance values: {}'.format(global_importance_values))
    print('global importance names: {}'.format(global_importance_names))
    ```

## <a name="visualizations"></a>Visualisierungen

Verwenden Sie das Visualisierungsdashboard, um Ihr Modell zu verstehen und zu interpretieren:

### <a name="global-visualizations"></a>Globale Visualisierungen

Die folgenden Plots bieten eine globale Ansicht des trainierten Modells mit den zugehörigen Vorhersagen und Erläuterungen.

|Plot|BESCHREIBUNG|
|----|-----------|
|Durchsuchen von Daten| Eine Übersicht über das Dataset zusammen mit Vorhersagewerten|
|Globale Relevanz|Zeigt die K wichtigsten globalen Features (K konfigurierbar). Dieses Diagramm ist nützlich, um das globale Verhalten des zugrunde liegenden Modells zu verstehen.|
|Erläuterungsuntersuchung|Veranschaulicht, wie ein Feature für eine Änderung der Vorhersagewerte des Modells (oder der Wahrscheinlichkeit von Vorhersagewerten) sorgt. |
|Zusammenfassung| Verwendet Relevanzwerte für lokale Features mit Vorzeichen über alle Datenpunkte, um die Verteilung der Auswirkungen jedes Features auf den Vorhersagewert zu beschreiben.|

[![Globales Visualisierungsdashboard](./media/machine-learning-interpretability-explainability/global-charts.png)](./media/machine-learning-interpretability-explainability/global-charts.png#lightbox)

### <a name="local-visualizations"></a>Lokale Visualisierungen

Sie können an jedem Zeitpunkt der vorherigen Plots auf einen lokalen Datenpunkt klicken, um den Relevanzplot der lokalen Features für den angegebenen Datenpunkt zu laden.

|Plot|BESCHREIBUNG|
|----|-----------|
|Lokale Relevanz|Zeigt die K wichtigsten globalen Features (K konfigurierbar). Dieses Diagramm ist nützlich, um das lokale Verhalten des zugrunde liegenden Modells an einem bestimmten Datenpunkt zu verstehen.|
|Perturbation Exploration|Ermöglicht Ihnen die Änderung von Featurewerten des gewählten Datenpunkts und die Verfolgung, wie sich diese Änderungen auf den Vorhersagewert auswirken.|
|Individual Conditional Expectation (ICE)| Ermöglicht Ihnen das Ändern eines Featurewerts von einem Mindestwert in einen Höchstwert, um zu ermitteln, wie sich bei einer Änderung des Features die Vorhersage des Datenpunkts ändert.|

[![Visualisierungsdashboard: Wichtigkeit lokaler Features](./media/machine-learning-interpretability-explainability/local-charts.png)](./media/machine-learning-interpretability-explainability/local-charts.png#lightbox)


[![Visualisierungsdashboard: Störung von Features](./media/machine-learning-interpretability-explainability/perturbation.gif)](./media/machine-learning-interpretability-explainability/perturbation.gif#lightbox)


[![Visualisierungsdashboard: ICE-Plots](./media/machine-learning-interpretability-explainability/ice-plot.png)](./media/machine-learning-interpretability-explainability/ice-plot.png#lightbox)

Beachten Sie Folgendes: Sie müssen die Widget-Erweiterungen des Visualisierungsdashboards aktiviert haben, bevor der Jupyter-Kernel gestartet wird.

* Jupyter Notebooks

    ```shell
    jupyter nbextension install --py --sys-prefix azureml.contrib.explain.model.visualize
    jupyter nbextension enable --py --sys-prefix azureml.contrib.explain.model.visualize
    ```



* Jupyter-Labs

    ```shell
    jupyter labextension install @jupyter-widgets/jupyterlab-manager
    jupyter labextension install microsoft-mli-widget
    ```
Um das Visualisierungsdashboard zu laden, verwenden Sie den folgenden Code:

```python
from azureml.contrib.explain.model.visualize import ExplanationDashboard

ExplanationDashboard(global_explanation, model, x_test)
```

## <a name="raw-feature-transformations"></a>Rohfeaturetransformationen

Optional können Sie Ihre Featuretransformations-Pipeline an den Explainer übergeben, um Erklärungen in Bezug auf die Rohfeatures vor der Transformation (im Gegensatz zu entwickelte Features) zu empfangen. Wenn Sie diesen Schritt überspringen, bietet der Explainer Erklärungen in Bezug auf entwickelte Features.

Das Format der unterstützten Transformationen entspricht dem unter [sklearn-pandas](https://github.com/scikit-learn-contrib/sklearn-pandas) beschriebenen. Grundsätzlich werden alle Transformationen unterstützt, solange sie eine einzelne Spalte verarbeiten und daher eine 1:n-Transformation darstellen. 

Wir können unformatierte Features erklären, indem wir entweder einen `sklearn.compose.ColumnTransformer` oder eine Liste mit passenden Transformationstupeln verwenden. In der Zelle unten wird `sklearn.compose.ColumnTransformer` verwendet. 

```python
from sklearn.compose import ColumnTransformer

numeric_transformer = Pipeline(steps=[
    ('imputer', SimpleImputer(strategy='median')),
    ('scaler', StandardScaler())])

categorical_transformer = Pipeline(steps=[
    ('imputer', SimpleImputer(strategy='constant', fill_value='missing')),
    ('onehot', OneHotEncoder(handle_unknown='ignore'))])

preprocessor = ColumnTransformer(
    transformers=[
        ('num', numeric_transformer, numeric_features),
        ('cat', categorical_transformer, categorical_features)])

# append classifier to preprocessing pipeline.
# now we have a full prediction pipeline.
clf = Pipeline(steps=[('preprocessor', preprocessor),
                      ('classifier', LogisticRegression(solver='lbfgs'))])


# append classifier to preprocessing pipeline.
# now we have a full prediction pipeline.
clf = Pipeline(steps=[('preprocessor', preprocessor),
                      ('classifier', LogisticRegression(solver='lbfgs'))])


# clf.steps[-1][1] returns the trained classification model
# pass transformation as an input to create the explanation object
# "features" and "classes" fields are optional
tabular_explainer = TabularExplainer(clf.steps[-1][1],
                                     initialization_examples=x_train,
                                     features=dataset_feature_names,
                                     classes=dataset_classes,
                                     transformations=preprocessor)
```

Verwenden Sie den folgenden Code, falls Sie das Beispiel mit der Liste mit den passenden Transformationstupeln ausführen möchten: 
```python
from sklearn.pipeline import Pipeline
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import StandardScaler, OneHotEncoder
from sklearn.linear_model import LogisticRegression
from sklearn_pandas import DataFrameMapper

# assume that we have created two arrays, numerical and categorical, which holds the numerical and categorical feature names

numeric_transformations = [([f], Pipeline(steps=[('imputer', SimpleImputer(
    strategy='median')), ('scaler', StandardScaler())])) for f in numerical]

categorical_transformations = [([f], OneHotEncoder(
    handle_unknown='ignore', sparse=False)) for f in categorical]

transformations = numeric_transformations + categorical_transformations

# append model to preprocessing pipeline.
# now we have a full prediction pipeline.
clf = Pipeline(steps=[('preprocessor', DataFrameMapper(transformations)),
                      ('classifier', LogisticRegression(solver='lbfgs'))])

# clf.steps[-1][1] returns the trained classification model
# pass transformation as an input to create the explanation object
# "features" and "classes" fields are optional
tabular_explainer = TabularExplainer(clf.steps[-1][1],
                                     initialization_examples=x_train,
                                     features=dataset_feature_names,
                                     classes=dataset_classes,
                                     transformations=transformations)
```

## <a name="interpretability-at-inferencing-time"></a>Interpretierbarkeit bei Rückschlüssen

Der Explainer kann zusammen mit dem ursprünglichen Modell bereitgestellt werden und bei einer Bewertung die lokalen Erklärungsinformationen bereitstellen. Wir bieten auch einfachere Bewertungsexplainer an, um die Leistung der Interpretierbarkeit bei Rückschlüssen zu verbessern. Der Bereitstellungsvorgang für einen einfacheren Bewertungsexplainer ähnelt der Bereitstellung eines Modells und umfasst die folgenden Schritte:




1. Erstellen eines Erklärungsobjekts (z. B. per TabularExplainer):

   ```python
   from azureml.contrib.explain.model.tabular_explainer import TabularExplainer

   explainer = TabularExplainer(model, 
                                initialization_examples=x_train, 
                                features=dataset_feature_names, 
                                classes=dataset_classes, 
                                transformations=transformations)
   ```

1. Erstellen eines Bewertungsexplainers unter Verwendung des Erläuterungsobjekts:

   ```python
   from azureml.contrib.explain.model.scoring.scoring_explainer import KernelScoringExplainer, save

   # create a lightweight explainer at scoring time
   scoring_explainer = KernelScoringExplainer(explainer)

   # pickle scoring explainer
   # pickle scoring explainer locally
   OUTPUT_DIR = 'my_directory'
   save(scoring_explainer, directory=OUTPUT_DIR, exist_ok=True)
   ```

1. Konfigurieren und Registrieren eines Images, das das Bewertungsexplainermodell verwendet

   ```python
   # register explainer model using the path from ScoringExplainer.save - could be done on remote compute
   # scoring_explainer.pkl is the filename on disk, while my_scoring_explainer.pkl will be the filename in cloud storage
   run.upload_file('my_scoring_explainer.pkl', os.path.join(OUTPUT_DIR, 'scoring_explainer.pkl'))
   
   scoring_explainer_model = run.register_model(model_name='my_scoring_explainer', 
                                                model_path='my_scoring_explainer.pkl')
   print(scoring_explainer_model.name, scoring_explainer_model.id, scoring_explainer_model.version, sep = '\t')
   ```

1. [Optional:] Abrufen des Bewertungsexplainers aus der Cloud und Testen der Erläuterungen

   ```python
   from azureml.contrib.explain.model.scoring.scoring_explainer import load

   # retrieve the scoring explainer model from cloud"
   scoring_explainer_model = Model(ws, 'my_scoring_explainer')
   scoring_explainer_model_path = scoring_explainer_model.download(target_dir=os.getcwd(), exist_ok=True)

   # load scoring explainer from disk
   scoring_explainer = load(scoring_explainer_model_path)

   # test scoring explainer locally
   preds = scoring_explainer.explain(x_test)
   print(preds)
   ```

1. Bereitstellen des Images für ein Computeziel:

   1. Erstellen einer Bewertungsdatei (führen Sie vor diesem Schritt die Schritte unter [Bereitstellen von Modellen mit Azure Machine Learning Service](https://docs.microsoft.com/azure/machine-learning/service/how-to-deploy-and-where) zum Registrieren Ihres ursprünglichen Vorhersagemodells aus)

        ```python
        %%writefile score.py
        import json
        import numpy as np
        import pandas as pd
        import os
        import pickle
        from sklearn.externals import joblib
        from sklearn.linear_model import LogisticRegression
        from azureml.core.model import Model

        def init():

            global original_model
            global scoring_model

            # retrieve the path to the model file using the model name
            # assume original model is named original_prediction_model
            original_model_path = Model.get_model_path('original_prediction_model')
            scoring_explainer_path = Model.get_model_path('my_scoring_explainer')

            original_model = joblib.load(original_model_path)
            scoring_explainer = joblib.load(scoring_explainer_path)

        def run(raw_data):
            # get predictions and explanations for each data point
            data = pd.read_json(raw_data)
            # make prediction
            predictions = original_model.predict(data)
            # retrieve model explanations
            local_importance_values = scoring_explainer.explain(data)
            # you can return any data type as long as it is JSON-serializable
            return {'predictions': predictions.tolist(), 'local_importance_values': local_importance_values}
        ```

   1. Definieren der Bereitstellungskonfiguration. (Diese Konfiguration hängt von den Anforderungen Ihres Modells ab. Das folgende Beispiel definiert eine Konfiguration, die einen CPU-Kern und 1 GB Arbeitsspeicher verwendet.)

        ```python
        from azureml.core.webservice import AciWebservice

        aciconfig = AciWebservice.deploy_configuration(cpu_cores=1,
                                                       memory_gb=1,
                                                       tags={"data": "NAME_OF_THE_DATASET",
                                                             "method" : "local_explanation"},
                                                       description='Get local explanations for NAME_OF_THE_PROBLEM')
        ```

   1. Erstellen einer Datei mit Umgebungsabhängigkeiten

        ```python
        from azureml.core.conda_dependencies import CondaDependencies

        # WARNING: to install this, g++ needs to be available on the Docker image and is not by default (look at the next cell)

        azureml_pip_packages = ['azureml-defaults', 'azureml-contrib-explain-model', 'azureml-core', 'azureml-telemetry', 'azureml-explain-model']
 

        # specify CondaDependencies obj
        myenv = CondaDependencies.create(conda_packages=['scikit-learn', 'pandas'],
                                         pip_packages=['sklearn-pandas'] + azureml_pip_packages,
                                         pin_sdk_version=False)


        with open("myenv.yml","w") as f:
            f.write(myenv.serialize_to_string())

        with open("myenv.yml","r") as f:
            print(f.read())
        ```

   1. Erstellen eines benutzerdefinierten Dockerfiles mit installiertem g++

        ```python
        %%writefile dockerfile
        RUN apt-get update && apt-get install -y g++
        ```

   1. Bereitstellen des erstellten Images (geschätzte Zeit: 5 Minuten)

        ```python
        from azureml.core.webservice import Webservice
        from azureml.core.image import ContainerImage

        # use the custom scoring, docker, and conda files we created above
        image_config = ContainerImage.image_configuration(execution_script="score.py",
                                                        docker_file="dockerfile",
                                                        runtime="python",
                                                        conda_file="myenv.yml")

        # use configs and models generated above
        service = Webservice.deploy_from_model(workspace=ws,
                                            name='model-scoring-service',
                                            deployment_config=aciconfig,
                                            models=[scoring_explainer_model, original_model],
                                            image_config=image_config)

        service.wait_for_deployment(show_output=True)
        ```

1. Testen der Bereitstellung

    ```python
    import requests

    # create data to test service with
    examples = x_list[:4]
    input_data = examples.to_json()

    headers = {'Content-Type':'application/json'}

    # send request to service
    resp = requests.post(service.scoring_uri, input_data, headers=headers)

    print("POST to url", service.scoring_uri)
    # can covert back to Python objects from json string if desired
    print("prediction:", resp.text)
    ```

1. Bereinigen: Verwenden Sie zum Löschen eines bereitgestellten Webdiensts `service.delete()`.




## <a name="next-steps"></a>Nächste Schritte

Um eine Sammlung von Jupyter-Notebooks zu sehen, in denen die obigen Anweisungen demonstriert werden, lesen Sie die [Beispielnotebooks für Azure Machine Learning Interpretability](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/explain-model).
