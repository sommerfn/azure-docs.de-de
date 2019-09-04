---
title: Einführung in Azure Data Science Virtual Machine für Linux und Windows | Microsoft-Dokumentation
description: Wichtige Analyseszenarien und -komponenten für Windows und Linux Data Science Virtual Machines.
keywords: Data Science-Tools, virtuelle Computer für Data Science, Tools für Data Science, Linux Data Science
services: machine-learning
documentationcenter: ''
author: vijetajo
manager: cgronlun
ms.assetid: d4f91270-dbd2-4290-ab2b-b7bfad0b2703
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 02/22/2019
ms.author: vijetaj
ms.openlocfilehash: 22192d67a946f9f466848b65b9cd763b16e6c5f0
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70099381"
---
# <a name="what-is-the-azure-data-science-virtual-machine-for-linux-and-windows"></a>Was ist Azure Data Science Virtual Machine für Linux und Windows?

Data Science Virtual Machine (DSVM) ist ein angepasstes, auf der Azure-Cloudplattform basierendes VM-Image, das speziell für Data Science erstellt wurde. Es hat viele beliebte Data Science und andere Tools vorinstalliert und vorkonfiguriert, damit Sie sofort intelligente Anwendungen für die erweiterte Analyse erstellen können. 

Diese Toolkonfigurationen werden sowohl von Datenspezialisten und Entwicklern bei Microsoft als auch von der Data Science-Community gründlich getestet. Diese Tests tragen dazu bei, die Stabilität und allgemeine Verwendbarkeit zu gewährleisten.

Die DSVM-Instanz ist für folgende Betriebssysteme verfügbar:
+ Windows Server 2016, Windows Server 2012
+ Ubuntu 16.04 LTS und CentOS 7.4

> [!NOTE]
> Alle VM-Tools für Deep Learning wurden in Data Science Virtual Machine integriert. 


## <a name="what-can-i-do-with-the-dsvm"></a>Welche Möglichkeiten bietet DSVM?
Das Ziel von Data Science Virtual Machine besteht darin, Datenexperten aller Art eine branchenübergreifende, reibungslos funktionierende, vorkonfigurierte und vollständig integrierte Data Science-Umgebung bereitzustellen. Sie können also eine DSVM-Instanz bereitstellen, anstatt selbst einen vergleichbaren Arbeitsbereich erstellen zu müssen. Dadurch können Sie Tage oder sogar _Wochen_ bei der Installation, Konfiguration und Paketverwaltung sparen. Nachdem Ihre DSVM zugeordnet wurde, können Sie sofort mit der Arbeit an Ihren Data Science-Projekten beginnen.

DSVM ist für ein breites Spektrum von Verwendungsszenarien konzipiert und konfiguriert. Sie können Ihre Umgebung vertikal hoch- oder herunterskalieren, wenn sich Ihre Anforderungen ändern. Außerdem können Sie Ihre bevorzugte Sprache zum Programmieren von Data Science-Aufgaben verwenden und weitere Tools installieren, um das System an Ihre Anforderungen anzupassen.

### <a name="preconfigured-analytics-desktop-in-the-cloud"></a>Vorkonfigurierter Analyse-Desktop in der Cloud
DSVM stellt eine Basiskonfiguration für Data Science-Teams bereit, die ihre lokalen Desktops durch einen verwalteten Clouddesktop ersetzen möchten. Diese Grundlage stellt sicher, dass alle Datenanalysten in einem Team ein konsistentes Setup zum Überprüfen von Experimenten und besserer Zusammenarbeit haben. Sie trägt ferner zu einer Kostenreduzierung durch eine Verringerung des Aufwands für die Systemverwaltung. Dies wiederum führt zu einer Zeitersparnis bei der Evaluierung, Installation und Verwaltung von Softwarepaketen für erweiterte Analysen.

### <a name="data-science-training-and-education"></a>Data Science-Schulung und -Ausbildung
Ausbilder in Unternehmen und Dozenten, die Data Science-Kurse leiten, stellen in der Regel ein Image eines virtuellen Computers bereit. Durch das Image wird gewährleistet, dass für die Kursteilnehmer eine konsistente Umgebung eingerichtet wird und die Beispiele erwartungsgemäß funktionieren. 

DSVM erstellt eine bedarfsgerechte Umgebung mit einem konsistenten Setup, das den Support erleichtert und Inkompatibilitätsprobleme vermeidet. Wenn diese Umgebungen häufig bereitgestellt werden müssen, insbesondere für kürzere Schulungen, bringt dies erhebliche Vorteile.

### <a name="on-demand-elastic-capacity-for-large-scale-projects"></a>Bei Bedarf flexible Kapazität für umfangreiche Projekte
Für Data Science-Hackathons/-Wettbewerbe oder umfangreiche Datenmodelle und Auswertungen wird in der Regel für kurze Zeit eine hochskalierte Hardwarekapazität benötigt. DSVM kann dazu beitragen, die Data Science-Umgebung bei Bedarf schnell auf horizontal skalierten Servern zu replizieren, auf denen Experimente ausgeführt werden können, die leistungsstarke Computingressourcen erfordern.

### <a name="custom-compute-power-for-azure-notebooks"></a>Benutzerdefinierte Computeleistung für Azure Notebooks
[Azure Notebooks](../../notebooks/azure-notebooks-overview.md) ist ein kostenloser gehosteter Dienst zum Entwickeln, Ausführen und Freigeben von Jupyter Notebooks in der Cloud ohne Installation. Die kostenlose Dienstebene ist auf 4 GB Arbeitsspeicher und 1 GB Daten beschränkt. 

Um sämtliche Einschränkungen aufzuheben, können Sie an eine DSVM-Instanz oder an einen beliebigen anderen virtuellen Computer auf einem Jupyter-Server ein Notebooks-Projekt anfügen. Wenn Sie sich mit einem Konto über Azure Active Directory (z. B. einem Unternehmenskonto) bei Azure Notebooks anmelden, zeigt Notebooks automatisch DSVM-Instanzen in allen Abonnements an, die mit diesem Konto verknüpft sind. Sie können [eine DSVM-Instanz an Azure Notebooks anfügen](../../notebooks/configure-manage-azure-notebooks-projects.md#compute-tier), um die verfügbare Computeleistung zu erhöhen.

### <a name="short-term-experimentation-and-evaluation"></a>Kurzfristige Experimente und Auswertungen
Mit DSVM können Sie mit minimalem Einrichtungsaufwand Tools wie die folgenden evaluieren oder sich mit ihnen vertraut machen:

- Microsoft Machine Learning Server
- SQL Server
- Visual Studio-Tools
- Jupyter
- Toolkits für maschinelles Lernen und Deep Learning
- Neue, bei der Community beliebte Tools 

Dank der schnellen Einrichtung können Sie DSVM-Instanzen auch in anderen kurzfristigen Verwendungsszenarien verwenden. Hierzu zählen beispielsweise die Replikation veröffentlichter Experimente sowie Demos und exemplarische Vorgehensweisen in Onlinesitzungen und Konferenztutorials.

### <a name="deep-learning"></a>Deep Learning
In DSVM können Ihre Trainingsmodelle Deep Learning-Algorithmen auf Hardware verwenden, die auf Grafikprozessoren (Graphics Processing Units, GPUs) basiert. Dank der VM-Skalierungsfunktionen der Azure-Plattform unterstützt Sie DSVM bei der bedarfsgerechten Nutzung von GPU-basierter Hardware in der Cloud. Sie können zu einem GPU-basierten virtuellen Computer wechseln, wenn Sie große Modelle trainieren oder schnelle Berechnungen benötigen, und dabei den gleichen Betriebssystem-Datenträger beibehalten.  

Die Windows Server 2016-Edition von DSVM enthält vorinstallierte GPU-Treiber und -Frameworks sowie GPU-Versionen der Deep Learning-Frameworks. Unter der Linux-Edition ist GPU-basiertes Deep Learning sowohl auf der DSVM-Instanz mit CentOS als auch auf der DSVM-Instanz mit Ubuntu aktiviert. 

Sie können die Ubuntu-, CentOS- oder Windows 2016-Edition von DSVM auch auf einem virtuellen Azure-Computer bereitstellen, der nicht auf GPUs basiert. In diesem Fall werden aber alle Deep Learning-Frameworks auf den CPU-Modus zurückgesetzt.
 
[Informieren Sie sich ausführlicher über die verfügbaren Deep Learning- und KI-Frameworks.](dsvm-deep-learning-ai-frameworks.md)

<a name="included"></a>

## <a name="whats-included-on-the-dsvm"></a>Was ist in DSVM enthalten?
Der virtuelle Computer für Data Science hat viele beliebte Data Science- und Deep Learning-Tools bereits installiert und konfiguriert. Darüber hinaus enthält er Tools, die die Arbeit mit verschiedenen Azure-Daten und Analyse-Produkten erleichtern. Diese Produkte beinhalten Microsoft Machine Learning Server (R, Python) für die Erstellung von Vorhersagemodellen sowie SQL Server 2017 für die umfangreiche Erkundung von Datasets. DSVM enthält außerdem weitere Tools aus der Open-Source-Community und von Microsoft sowie [Beispielcode und Notebooks](dsvm-samples-and-walkthroughs.md). 

Im Anschluss folgt eine Liste der Tools und Plattformen:
+ [Unterstützte Programmiersprachen](dsvm-languages.md)

+ [Unterstützte Datenplattformen](dsvm-data-platforms.md)

+ [Entwicklungstools und IDEs](dsvm-tools-development.md)

+ [Deep Learning- und KI-Frameworks](dsvm-deep-learning-ai-frameworks.md)

+ [Machine Learning- und Data Science-Tools](dsvm-ml-data-science-tools.md)

+ [Datenerfassungstools](dsvm-tools-ingestion.md)

+ [Tools zum Durchsuchen und Visualisieren von Daten](dsvm-tools-explore-and-visualize.md)

Die folgende Tabelle enthält eine Aufzählung und einen Vergleich der wichtigsten Komponenten Windows- und Linux-Editionen des virtuellen Computers für Data Science.

| **Tool**                                                           | **Windows-Edition** | **Linux-Edition** |
| :------------------------------------------------------------------ |:-------------------:|:------------------:|
| [Microsoft R Open](https://mran.microsoft.com/open/) mit verbreiteten vorinstallierten Paketen   |J                      | J             |
| [Microsoft Machine Learning Server (R, Python)](https://docs.microsoft.com/machine-learning-server/) Developer Edition enthält: <br />  &nbsp;&nbsp;&nbsp;&nbsp; [RevoScaleR/revoscalepy](https://docs.microsoft.com/machine-learning-server/r/concept-what-is-revoscaler): paralleles und verteiltes Hochleistungsframework (R und Python)<br />  &nbsp;&nbsp;&nbsp;&nbsp; [MicrosoftML](https://docs.microsoft.com/machine-learning-server/r/concept-what-is-the-microsoftml-package): neue moderne Machine Learning-Algorithmen von Microsoft <br />  &nbsp;&nbsp;&nbsp;&nbsp; [R- und Python-Operationalisierung](https://docs.microsoft.com/machine-learning-server/what-is-operationalization)                                            |J                      | J |
| [Microsoft Office](https://products.office.com/business/office-365-proplus-business-software) ProPlus mit gemeinsamer Aktivierung: Excel, Word und PowerPoint   |J                      |N              |
| [Anaconda Python](https://www.continuum.io/) 2.7 und 3.5 mit beliebten vorinstallierten Paketen    |J                      |J              |
| [JuliaPro](https://juliacomputing.com/products/juliapro.html) mit beliebten vorinstallierten Paketen                         |J                      |J              |
| Relationale Datenbanken                                                            | [SQL Server 2017](https://www.microsoft.com/sql-server/sql-server-2017) <br/> Developer Edition| [PostgreSQL](https://www.postgresql.org/) (CentOS),<br/>[SQL Server 2017](https://www.microsoft.com/sql-server/sql-server-2017) <br/> Developer Edition (Ubuntu) |
| Datenbanktools                                                       |  SQL Server Management Studio <br/> SQL Server Integration Services<br/> [bcp, sqlcmd](https://docs.microsoft.com/sql/tools/command-prompt-utility-reference-database-engine)<br />  ODBC/JDBC-Treiber|  [SQuirreL SQL](http://squirrel-sql.sourceforge.net/) (Abfragetool), <br />  bcp, sqlcmd <br />  ODBC/JDBC-Treiber|
| Skalierbare datenbankinterne Analysen mit SQL Server Machine Learning Services (R, Python) | J     |N              |
| [Jupyter Notebook-Server](https://jupyter.org/) mit folgenden Kernels:                                  | J     | J |
|     &nbsp;&nbsp;&nbsp;&nbsp; R | J | J |
|     &nbsp;&nbsp;&nbsp;&nbsp; Python | J | J |
|     &nbsp;&nbsp;&nbsp;&nbsp; Julia | J | J |
|     &nbsp;&nbsp;&nbsp;&nbsp; PySpark | J | J |
|     &nbsp;&nbsp;&nbsp;&nbsp; [Sparkmagic](https://github.com/jupyter-incubator/sparkmagic) | N | Y (nur Ubuntu) |
|     &nbsp;&nbsp;&nbsp;&nbsp; SparkR     | N | J |
| JupyterHub (Notebook-Server für mehrere Benutzer)| N | J |
| JupyterLab (Notebook-Server für mehrere Benutzer) | N | Y (nur Ubuntu) |
| Entwicklungstools, IDEs und Code-Editoren:| | |
| &nbsp;&nbsp;&nbsp;&nbsp; [Visual Studio 2019 (Community Edition)](https://www.visualstudio.com/community/) mit Git-Plug-In, Azure HDInsight (Hadoop), Azure Data Lake, SQL Server Data Tools, [Node.js](https://github.com/Microsoft/nodejstools), [Python](https://aka.ms/ptvs) und [R Tools für Visual Studio (RTVS)](https://microsoft.github.io/RTVS-docs/) | J | N |
| &nbsp;&nbsp;&nbsp;&nbsp; [Visual Studio Code](https://code.visualstudio.com/) | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp; [RStudio Desktop](https://www.rstudio.com/products/rstudio/#Desktop) | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp; [RStudio Server](https://www.rstudio.com/products/rstudio/#Server) | N | J |
| &nbsp;&nbsp;&nbsp;&nbsp; [PyCharm Community Edition](https://www.jetbrains.com/pycharm/) | N | J |
| &nbsp;&nbsp;&nbsp;&nbsp; [Atom](https://atom.io/) | N | J |
| &nbsp;&nbsp;&nbsp;&nbsp; [Juno (Julia IDE)](https://junolab.org/)| J | J |
| &nbsp;&nbsp;&nbsp;&nbsp; Vim und Emacs | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp; Git und Git Bash | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp; OpenJDK | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp; .NET Framework | J | N |
| Power BI Desktop | J | N |
| SDKs zum Zugriff auf Azure und Cortana Intelligence Sammlung von Diensten | J | J |
| Tools für Datenverschiebung und -verwaltung: | | |
| &nbsp;&nbsp;&nbsp;&nbsp; Azure Storage Explorer | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp; [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure) | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp; Azure PowerShell | J | N |
| &nbsp;&nbsp;&nbsp;&nbsp; [AzCopy](https://docs.microsoft.com/azure/storage/storage-use-azcopy) | J | N |
| &nbsp;&nbsp;&nbsp;&nbsp; [Blob-FUSE-Treiber](https://github.com/Azure/azure-storage-fuse) | N | J |
| &nbsp;&nbsp;&nbsp;&nbsp; [AdlCopy (Azure Data Lake Storage)](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-copy-data-azure-storage-blob) | J | N |
| &nbsp;&nbsp;&nbsp;&nbsp; [Azure Cosmos DB: Datenmigrationstool](https://docs.microsoft.com/azure/documentdb/documentdb-import-data) | J | N |
| &nbsp;&nbsp;&nbsp;&nbsp; [Microsoft-Datenverwaltungsgateway](https://msdn.microsoft.com/library/dn879362.aspx): Verschieben von Daten zwischen der lokalen Umgebung und der Cloud | J | N |
| &nbsp;&nbsp;&nbsp;&nbsp; Unix/Linux-Befehlszeilentools | J | J |
| [Apache Drill](https://drill.apache.org) für die Erkundung von Daten | J | J |
| Tools für maschinelles Lernen: |||
| &nbsp;&nbsp;&nbsp;&nbsp; Integration mit [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) (R, Python) | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp; [XGBoost](https://github.com/dmlc/xgboost) | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp; [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit) | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp; [Weka](https://www.cs.waikato.ac.nz/ml/weka/) | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp; [Rattle](https://togaware.com/rattle/) | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp; [LightGBM](https://github.com/Microsoft/LightGBM) | N | Y (nur Ubuntu) |
| &nbsp;&nbsp;&nbsp;&nbsp; [CatBoost](https://tech.yandex.com/catboost/) | N | Y (nur Ubuntu) |
| &nbsp;&nbsp;&nbsp;&nbsp; [H2O](https://www.h2o.ai/h2o/), [Sparkling Water](https://www.h2o.ai/sparkling-water/) | N | Y (nur Ubuntu) |
| Deep Learning-Tools, die auf einer GPU oder CPU funktionieren: |  |  |
| &nbsp;&nbsp;&nbsp;&nbsp; [Microsoft Cognitive Toolkit (CNTK)](https://docs.microsoft.com/cognitive-toolkit/) (Windows 2016) | J | J |
| &nbsp;&nbsp;&nbsp;&nbsp; [TensorFlow](https://www.tensorflow.org/) | J (Windows 2016) | J |
| &nbsp;&nbsp;&nbsp;&nbsp; [Horovod](https://github.com/uber/horovod) | N | J (Ubuntu) |
| &nbsp;&nbsp;&nbsp;&nbsp; [MXNet](https://mxnet.io/) | J (Windows 2016) | J|
| &nbsp;&nbsp;&nbsp;&nbsp; [Caffe und Caffe2](https://github.com/caffe2/caffe2) | N | J |
| &nbsp;&nbsp;&nbsp;&nbsp; [Chainer](https://chainer.org/) | N | J |
| &nbsp;&nbsp;&nbsp;&nbsp; [Torch](http://torch.ch/) | N | J |
| &nbsp;&nbsp;&nbsp;&nbsp; [Theano](https://github.com/Theano/Theano) | N | J |
| &nbsp;&nbsp;&nbsp;&nbsp; [Keras](https://keras.io/)| N | J |
| &nbsp;&nbsp;&nbsp;&nbsp; [PyTorch](https://pytorch.org/)| N | J |
| &nbsp;&nbsp;&nbsp;&nbsp; [NVidia Digits](https://github.com/NVIDIA/DIGITS) | N | J |
| &nbsp;&nbsp;&nbsp;&nbsp; [MXNet Model Server](https://github.com/awslabs/mxnet-model-server) | N | J |
| &nbsp;&nbsp;&nbsp;&nbsp; [TensorFlow Serving](https://www.tensorflow.org/serving/) | N | J |
| &nbsp;&nbsp;&nbsp;&nbsp; [TensorRT](https://developer.nvidia.com/tensorrt) | N | J |
| &nbsp;&nbsp;&nbsp;&nbsp; [CUDA-, cuDNN-, NVIDIA-Treiber](https://developer.nvidia.com/cuda-toolkit) | J | J |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in diesen Artikeln:

+ Windows:
  + [Bereitstellen einer Data Science Virtual Machine für Windows in Azure](provision-vm.md)
  + [Zehn Dinge, die Sie mit der Windows Data Science Virtual Machine machen können](vm-do-ten-things.md)

+ Linux:
  + [Bereitstellen der Data Science Virtual Machine für Linux (Ubuntu)](dsvm-ubuntu-intro.md)
  + [Bereitstellen einer Linux CentOS Data Science-VM in Azure](linux-dsvm-intro.md)
  + [Data Science mit einer Linux Data Science Virtual Machine in Azure](linux-dsvm-walkthrough.md)
