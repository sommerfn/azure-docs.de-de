---
title: Was ist ML Studio (klassisch)?
titleSuffix: Azure
description: Azure Machine Learning Studio (klassisch) ist ein Drag & Drop-Tool zum schnellen Erstellen von Modellen auf der Grundlage einer verwendungsbereiten Bibliothek mit Algorithmen und Modulen.
services: machine-learning
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: seodec18
ms.assetid: e65c8fe1-7991-4a2a-86ef-fd80a7a06269
ms.service: machine-learning
ms.subservice: studio
ms.topic: overview
ms.date: 10/17/2019
ms.openlocfilehash: 325278ee0222070ce7c02c1d72961f0431284677
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73670545"
---
# <a name="what-is-machine-learning-studio-classic"></a>Was ist Machine Learning Studio (klassisch)?

[!INCLUDE [Designer notice](../../../includes/designer-notice.md)]

Microsoft Azure Machine Learning Studio (klassisch) ist ein zusammenarbeitsorientiertes Drag & Drop-Tool, mit dem Sie Predictive Analytics-Lösungen für Ihre Daten erstellen, testen und bereitstellen können.  In der klassischen Version von Machine Learning Studio werden Modelle als Webdienste veröffentlicht, die problemlos von benutzerdefinierten Apps oder BI-Tools wie Excel genutzt werden können.

Machine Learning Studio (klassisch) vereint Data Science mit Predictive Analytics, Cloudressourcen und Ihren Daten.

## <a name="the-machine-learning-studio-classic--interactive-workspace"></a>Der interaktive Arbeitsbereich von Machine Learning Studio (klassisch)
Für die Entwicklung eines prädiktiven Analysemodells verwendet man typischerweise Daten aus einer oder mehreren Quellen, transformiert und analysiert diese Daten mithilfe verschiedener Datenbearbeitungen und Statistikfunktionen und generiert einen Ergebnissatz. Das Entwickeln eines solchen Modells ist ein iterativer Prozess. Sie ändern die verschiedenen Funktionen und deren Parameter, und die Ergebnisse nähern sich an, bis Sie der Ansicht sind, dass Sie ein trainiertes, effektives Modell erreicht haben.

In der klassischen Version von Azure Machine Learning Studio steht ein interaktiver, visueller Arbeitsbereich zur Verfügung, in dem Sie mühelos ein Predictive Analytics-Modell entwickeln, testen und durchlaufen können. Dazu ziehen Sie ***Datasets*** und ***Analysemodule*** per Drag & Drop auf eine interaktive Canvas und verbinden sie zu einem ***Experiment***, das Sie dann in Machine Learning Studio (klassisch) ausführen. Für die Iteration des Modelldesigns bearbeiten Sie das Experiment, speichern ggf. eine Kopie und führen es erneut aus. Wenn Sie bereit sind, können Sie Ihr ***Trainingsexperiment*** in ein ***Vorhersageexperiment*** konvertieren und anschließend als ***Webdienst*** veröffentlichen, damit andere Benutzer auf das Modell zugreifen können.

Es ist keinerlei Programmierung erforderlich. Sie müssen lediglich Datasets und Module auf der grafischen Benutzeroberfläche miteinander verbinden, um Ihr Predictive Analytics-Modell zu erstellen.

![Azure Machine Learning Studio-Diagramm: Erstellen von Experimenten, Lesen von Daten aus vielen Quellen, Schreiben der ausgewerteten Daten, Erstellen von Modellen.](./media/what-is-ml-studio/azure-ml-studio-diagram.jpg)

<a name="compare"></a>
## <a name="how-does-machine-learning-studio-classic-differ-from-azure-machine-learning"></a>Inwiefern unterscheidet sich Azure Machine Studio (klassisch) von Azure Machine Learning?

In [Azure Machine Learning](../service/overview-what-is-azure-ml.md) stehen SDKs **und** der Azure Machine Learning-Designer (Vorschauversion) zur Verfügung, um im Handumdrehen Daten vorzubereiten und Machine Learning-Modell zu trainieren und bereitzustellen. Der Designer bietet eine ähnliche Drag & Drop-Umgebung wie Studio (klassisch). Im Gegensatz zur proprietären Computeplattform von Studio (klassisch) nutzt der Designer jedoch Ihre eigenen Computeressourcen und ist vollständig in Azure Machine Learning integriert.

Im Anschluss finden Sie eine kurze Gegenüberstellung:

|| Machine Learning Studio (klassisch) | Azure Machine Learning |
|---| --- | --- |
| Drag & Drop-Oberfläche | Ja | Ja (siehe [Was ist der Azure Machine Learning-Designer (Vorschau)?](../service/concept-designer.md)) |
| Experiment | Skalierbar (Limit für Trainingsdaten: 10 GB) | Skalieren mit Computeziel |
| Module für die Drag & Drop-Oberfläche | Mehrere | Anfänglicher Satz beliebter [Module](../algorithm-module-reference/module-reference.md)|
|Trainieren von Computezielen| Proprietäres Computeziel, nur CPU-Unterstützung| Unterstützt Azure Machine Learning Compute (GPU oder CPU) und virtuelle Notebook-Computer.<br/>([Weitere Computeziele werden im SDK unterstützt.](../service/concept-compute-target.md#train))|
|Rückschlusscomputeziele| Proprietäres Webdienstformat, nicht anpassbar |  Azure Kubernetes Service und AML Compute <br/>([Weitere Computeziele werden im SDK unterstützt.](../service/how-to-deploy-and-where.md)) |
| ML-Pipeline | Nicht unterstützt | [Pipelines](../service/concept-ml-pipelines.md) unterstützt |
| MLOps | Einfache Modellverwaltung und -bereitstellung | Konfigurierbare Bereitstellung: Versionsverwaltung und Nachverfolgung für Modelle und Pipelines |
| Modellformat | Proprietäres Format, nur Studio | Standardformat (abhängig von der Art des Trainingsauftrags) |
|Automatisiertes Modelltraining und Optimieren von Hyperparametern | Nein | Noch nicht im Designer <br/> ([Wird im SDK und auf der Landing Page des Arbeitsbereichs unterstützt.](../service/concept-automated-ml.md)) | 

Testen Sie den Designer mit [Tutorial: Prognostizieren von Automobilpreisen mit dem Designer](../service/tutorial-designer-automobile-price-train-score.md).

> [!NOTE]
> In Studio (klassisch) erstellte Modelle können nicht von Azure Machine Learning bereitgestellt und verwaltet werden. Über den Designer erstellte und bereitgestellte Modelle können dagegen über den Azure Machine Learning-Arbeitsbereich verwaltet werden.

## <a name="download-the-machine-learning-studio-classic-overview-diagram"></a>Herunterladen des Übersichtsdiagramms für Machine Learning Studio (klassisch)
Laden Sie das **Diagramm mit der Übersicht über die Funktionen von Microsoft Azure Machine Learning Studio (klassisch)** herunter, und verschaffen Sie sich einen allgemeinen Überblick über die Funktionen von Machine Learning Studio (klassisch). Drucken Sie das Diagramm in Tabloid-Größe (28 x 43 cm, 11 x 17 Zoll) aus, um schnell darauf zugreifen zu können.

**Laden Sie das Diagramm hier herunter: [Übersicht über die Funktionen von Microsoft Azure Machine Learning Studio (klassisch)](https://download.microsoft.com/download/C/4/6/C4606116-522F-428A-BE04-B6D3213E9E52/ml_studio_overview_v1.1.pdf)** 
![Übersicht über die Funktionen von Microsoft Azure Machine Learning Studio (klassisch)](./media/what-is-ml-studio/ml_studio_overview_v1.1.png)


## <a name="components-of-a-studio-classic--experiment"></a>Komponenten eines Studio-Experiments (klassisch)
Ein Experiment besteht auf Datasets, die Daten für Analysemodule bereitstellen, die Sie miteinander verbinden und so ein Vorhersagemodell konstruieren. Ein gültiges Experiment hat spezifisch folgende Eigenschaften:

* Das Experiment besteht aus mindestens einem Dataset und einem Modul.
* Datasets können nur mit Modulen verbunden sein.
* Module können entweder mit Datasets oder mit anderen Modulen verbunden sein.
* Alle Eingangsports für Module müssen eine Verbindung zum Datenfluss aufweisen.
* Alle erforderlichen Parameter der einzelnen Module müssen festgelegt sein.

Sie können ein Experiment von Grund auf neu erstellen, oder Sie können ein vorhandenes Beispielexperiment als Vorlage verwenden. Weitere Informationen finden Sie unter [Kopieren von Beispielexperimenten zum Erstellen neuer Machine Learning-Experimente](sample-experiments.md).

Ein Beispiel für die Erstellung eines Experiments finden Sie unter [Erstellen eines einfachen Experiments in Azure Machine Learning Studio (klassisch)](create-experiment.md).

Eine ausführlichere exemplarische Vorgehensweise zum Erstellen einer Lösung für Vorhersageanalysen finden Sie unter [Entwickeln einer Vorhersagelösung mit Azure Machine Learning Studio (klassisch)](tutorial-part1-credit-risk.md).

### <a name="datasets"></a>Datasets
Bei einem Dataset handelt es sich um Daten, die in Machine Learning Studio (klassisch) hochgeladen wurden, damit sie für den Modellierungsprozess zur Verfügung stehen. Machine Learning Studio (klassisch) enthält eine Reihe von Beispieldatasets, mit denen Sie experimentieren können. Bei Bedarf können Sie außerdem weitere Datasets hochladen. Hier einige Beispiele der enthaltenen Datasets:

* **MPG-Daten für verschiedene Autos**: Meilen-pro-Gallone-Werte für Autos, die anhand der Anzahl von Zylindern, Motorleistung usw. aufgeführt sind.
* **Brustkrebsdaten:** diagnostische Brustkrebsdaten.
* **Waldbranddaten:** Größe von Waldbränden im nordöstlichen Portugal.

Beim Erstellen eines Experiments können Sie links neben dem Arbeitsbereich aus der Liste der Datasets auswählen.

Eine Liste der in Machine Learning Studio (klassisch) enthaltenen Beispieldatasets finden Sie unter [Verwenden von Beispieldatasets in Azure Machine Learning Studio (klassisch)](use-sample-datasets.md).

### <a name="modules"></a>Module
Ein Modul ist ein Algorithmus, den Sie auf Ihre Daten anwenden können.  Die klassische Version von Machine Learning Studio verfügt über eine Reihe von Modulen – von Dateneingangsfunktionen bis hin zu Trainings-, Bewertungs- und Überprüfungsvorgängen. Hier einige Beispiele der enthaltenen Module:

* [In ARFF konvertieren:][convert-to-arff] Konvertiert ein serialisiertes .NET-Dataset in ARFF (Attribute-Relation File Format).
* [Elementare Statistiken berechnen:][elementary-statistics] Berechnet elementare Statistiken wie Mittelwert, Standardabweichung usw.
* [Lineare Regression:][linear-regression] Erstellt ein lineares Regressionsmodell, das auf einem Modell für den Onlinegradientenabstieg beruht.
* [Bewertungsmodell:][score-model] Bewertet ein trainiertes Klassifizierungs- oder Regressionsmodell.

Beim Erstellen eines Experiments können Sie links neben dem Arbeitsbereich aus der Liste der Module auswählen.

Ein Modul kann eine Reihe von Parametern haben, die Sie zum Konfigurieren der internen Algorithmen des Moduls einsetzen können. Wenn Sie ein Modul im Arbeitsbereich auswählen, werden dessen Parameter rechts neben dem Arbeitsbereich im Bereich **Eigenschaften** angezeigt. Sie können die Parameter in diesem Bereich zur Abstimmung Ihres Modells verändern.

Hilfe zur Navigation durch die umfassende Bibliothek verfügbarer Machine Learning-Algorithmen finden Sie unter [Auswählen von Algorithmen für Microsoft Azure Machine Learning Studio (klassisch)](algorithm-choice.md).

## <a name="deploying-a-predictive-analytics-web-service"></a>Bereitstellen eines Predictive Analytics-Webdiensts
Wenn Ihr Predictive Analytics-Modell bereit ist, können Sie es als Webdienst direkt von Machine Learning Studio (klassisch) aus bereitstellen. Weitere Informationen zu diesem Verfahren finden Sie unter [Bereitstellen eines Azure Machine Learning-Webdiensts](deploy-a-machine-learning-web-service.md).

## <a name="next-steps"></a>Nächste Schritte
Die Grundlagen von Predictive Analytics und maschinellem Lernen werden anhand einer [Schritt-für-Schritt-Schnellstartanleitung](create-experiment.md) und mithilfe von [Beispielen](sample-experiments.md) vermittelt.

<!-- Module References -->
[convert-to-arff]: https://msdn.microsoft.com/library/azure/62d2cece-d832-4a7a-a0bd-f01f03af0960/
[elementary-statistics]: https://msdn.microsoft.com/library/azure/3086b8d4-c895-45ba-8aa9-34f0c944d4d3/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
