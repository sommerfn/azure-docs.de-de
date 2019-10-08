---
title: Auf der DSVM-Instanz enthaltene Tools
titleSuffix: Azure Data Science Virtual Machine tools
description: Hier finden Sie eine Liste der Tools, die in den Windows- und Ubuntu-DSVM-Images enthalten sind.
keywords: Data Science-Tools, virtuelle Computer für Data Science, Tools für Data Science, Linux Data Science
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: gvashishtha
ms.author: gopalv
ms.topic: overview
ms.date: 09/27/2019
ms.openlocfilehash: 79de3406e47b84a6120496acce5bba948fbb1a6d
ms.sourcegitcommit: 4f3f502447ca8ea9b932b8b7402ce557f21ebe5a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2019
ms.locfileid: "71803546"
---
# <a name="what-tools-are-included-on-the-azure-data-science-virtual-machine"></a>Welche Tools sind in Azure Data Science Virtual Machine enthalten?

Im Folgenden finden Sie eine aktuelle Liste der in Data Science Virtual Machine enthaltenen Tools sowie Links, unter denen die Konfiguration der einzelnen Tools erläutert wird.


| **Tool**                                                           | **DSVM unter Windows** | **DSVM unter Linux** | **Hinweise zur Verwendung** |
| :------------------------------------------------------------------ |:-------------------:|:------------------:|:------:|
| [Microsoft R Open](https://mran.microsoft.com/open/) mit verbreiteten vorinstallierten Paketen   |<span class='green-check'>&#9989;</span>                     |<span class='green-check'>&#9989;</span>  | [Verwenden von R in DSVM](./dsvm-languages.md#r)           |
| [Microsoft Machine Learning Server (R, Python)](https://docs.microsoft.com/machine-learning-server/) Developer Edition enthält: <br />  &nbsp;&nbsp;&nbsp;&nbsp; [RevoScaleR/revoscalepy](https://docs.microsoft.com/machine-learning-server/r/concept-what-is-revoscaler): paralleles und verteiltes Hochleistungsframework (R und Python)<br />  &nbsp;&nbsp;&nbsp;&nbsp; [MicrosoftML](https://docs.microsoft.com/machine-learning-server/r/concept-what-is-the-microsoftml-package): neue moderne Machine Learning-Algorithmen von Microsoft <br />  &nbsp;&nbsp;&nbsp;&nbsp; [R- und Python-Operationalisierung](https://docs.microsoft.com/machine-learning-server/what-is-operationalization)                                            |<span class='green-check'>&#9989;</span>                       |<span class='green-check'>&#9989;</span> | |
| [Microsoft Office](https://products.office.com/business/office-365-proplus-business-software) ProPlus mit gemeinsamer Aktivierung: Excel, Word und PowerPoint   |<span class='green-check'>&#9989;</span>                       |<span class='red-x'>&#10060;</span>              | |
| [Anaconda Python](https://www.continuum.io/) 2.7 und 3.5 mit beliebten vorinstallierten Paketen    |<span class='green-check'>&#9989;</span>                       |<span class='green-check'>&#9989;</span>               | |
| [JuliaPro](https://juliacomputing.com/products/juliapro.html) mit beliebten vorinstallierten Paketen                         |<span class='green-check'>&#9989;</span>                       |<span class='green-check'>&#9989;</span>               | [Verwenden von Julia in DSVM](./dsvm-languages.md#julia) |
| Relationale Datenbanken                                                            | [SQL Server 2017](https://www.microsoft.com/sql-server/sql-server-2017) <br/> Developer Edition| [PostgreSQL](https://www.postgresql.org/) (CentOS),<br/>[SQL Server 2017](https://www.microsoft.com/sql-server/sql-server-2017) <br/> Developer Edition (Ubuntu) | (./dsvm-data-platforms#sql-server-2016-developer-edition.md) |
| Datenbanktools                                                       |  SQL Server Management Studio <br/> SQL Server Integration Services<br/> [bcp, sqlcmd](https://docs.microsoft.com/sql/tools/command-prompt-utility-reference-database-engine)<br />  ODBC/JDBC-Treiber|  [SQuirreL SQL](http://squirrel-sql.sourceforge.net/) (Abfragetool), <br />  bcp, sqlcmd <br />  ODBC/JDBC-Treiber| |
| Skalierbare datenbankinterne Analysen mit SQL Server Machine Learning Services (R, Python) |<span class='green-check'>&#9989;</span>    | <span class='red-x'>&#10060;</span>                | |
| [Jupyter Notebook-Server](https://jupyter.org/) mit folgenden Kernels:                                  |<span class='green-check'>&#9989;</span>     |<span class='green-check'>&#9989;</span> | [Jupyter Notebook-Beispiele](./dsvm-samples-and-walkthroughs.md) | 
|     &nbsp;&nbsp;&nbsp;&nbsp; R |<span class='green-check'>&#9989;</span> |<span class='green-check'>&#9989;</span> | [R Jupyter-Beispiele](./dsvm-samples-and-walkthroughs.md#r-language) |
|     &nbsp;&nbsp;&nbsp;&nbsp; Python |<span class='green-check'>&#9989;</span> |<span class='green-check'>&#9989;</span> | [Python Jupyter-Beispiele](./dsvm-samples-and-walkthroughs.md#python-language) |
|     &nbsp;&nbsp;&nbsp;&nbsp; Julia |<span class='green-check'>&#9989;</span> |<span class='green-check'>&#9989;</span> | [Julia Jupyter-Beispiele](./dsvm-samples-and-walkthroughs.md#julia-language) |
|     &nbsp;&nbsp;&nbsp;&nbsp; PySpark |<span class='green-check'>&#9989;</span> |<span class='green-check'>&#9989;</span> | [pySpark Jupyter-Beispiele](./dsvm-samples-and-walkthroughs.md#sparkml)
|     &nbsp;&nbsp;&nbsp;&nbsp; [Sparkmagic](https://github.com/jupyter-incubator/sparkmagic) |<span class='red-x'>&#10060;</span>   |<span class='green-check'>&#9989;</span> (nur Ubuntu) | |
|     &nbsp;&nbsp;&nbsp;&nbsp; SparkR     |<span class='red-x'>&#10060;</span>   |<span class='green-check'>&#9989;</span> | |
| JupyterHub (Notebook-Server für mehrere Benutzer)|<span class='red-x'>&#10060;</span>   |<span class='green-check'>&#9989;</span> | |
| JupyterLab (Notebook-Server für mehrere Benutzer) |<span class='red-x'>&#10060;</span>   |<span class='green-check'>&#9989;</span> (nur Ubuntu) | |
| Entwicklungstools, IDEs und Code-Editoren:| | |
| &nbsp;&nbsp;&nbsp;&nbsp; [Visual Studio 2019 (Community Edition)](https://www.visualstudio.com/community/) mit Git-Plug-In, Azure HDInsight (Hadoop), Azure Data Lake, SQL Server Data Tools, [Node.js](https://github.com/Microsoft/nodejstools), [Python](https://aka.ms/ptvs) und [R Tools für Visual Studio (RTVS)](https://microsoft.github.io/RTVS-docs/) |<span class='green-check'>&#9989;</span> |<span class='red-x'>&#10060;</span>   | [Visual Studio 2019 in DSVM](./dsvm-tools-development.md#visual-studio-2019) |
| &nbsp;&nbsp;&nbsp;&nbsp; [Visual Studio Code](https://code.visualstudio.com/) |<span class='green-check'>&#9989;</span> |<span class='green-check'>&#9989;</span> | [Visual Studio Code in DSVM](./dsvm-tools-development.md#visual-studio-code) |
| &nbsp;&nbsp;&nbsp;&nbsp; [RStudio Desktop](https://www.rstudio.com/products/rstudio/#Desktop) |<span class='green-check'>&#9989;</span> |<span class='green-check'>&#9989;</span> | [RStudio Desktop in DSVM](./dsvm-tools-development.md#rstudio--desktop) |
| &nbsp;&nbsp;&nbsp;&nbsp; [RStudio Server](https://www.rstudio.com/products/rstudio/#Server) |<span class='red-x'>&#10060;</span>   |<span class='green-check'>&#9989;</span> | [RStudio Server in DSVM](./dsvm-tools-development.md#rstudio--server)
| &nbsp;&nbsp;&nbsp;&nbsp; [PyCharm Community Edition](https://www.jetbrains.com/pycharm/) |<span class='red-x'>&#10060;</span>   |<span class='green-check'>&#9989;</span> | [PyCharm in DSVM](./dsvm-tools-development.md#pycharm)
| &nbsp;&nbsp;&nbsp;&nbsp; [Atom](https://atom.io/) |<span class='red-x'>&#10060;</span>   |<span class='green-check'>&#9989;</span> | |
| &nbsp;&nbsp;&nbsp;&nbsp; [Juno (Julia IDE)](https://junolab.org/)|<span class='green-check'>&#9989;</span> |<span class='green-check'>&#9989;</span> | [Juno in DSVM](./dsvm-tools-development.md#juno)
| &nbsp;&nbsp;&nbsp;&nbsp; Vim und Emacs |<span class='green-check'>&#9989;</span> |<span class='green-check'>&#9989;</span> | |
| &nbsp;&nbsp;&nbsp;&nbsp; Git und Git Bash |<span class='green-check'>&#9989;</span> |<span class='green-check'>&#9989;</span> | |
| &nbsp;&nbsp;&nbsp;&nbsp; OpenJDK |<span class='green-check'>&#9989;</span> |<span class='green-check'>&#9989;</span> | |
| &nbsp;&nbsp;&nbsp;&nbsp; .NET Framework |<span class='green-check'>&#9989;</span> |<span class='red-x'>&#10060;</span>   | |
| Power BI Desktop |<span class='green-check'>&#9989;</span> |<span class='red-x'>&#10060;</span>   |
| SDKs zum Zugriff auf Azure und Cortana Intelligence Sammlung von Diensten |<span class='green-check'>&#9989;</span> |<span class='green-check'>&#9989;</span> | [Power BI Desktop in DSVM](./dsvm-tools-development.md#power-bi-desktop) |
| Tools für Datenverschiebung und -verwaltung: | | |
| &nbsp;&nbsp;&nbsp;&nbsp; Azure Storage Explorer |<span class='green-check'>&#9989;</span> |<span class='green-check'>&#9989;</span> | |
| &nbsp;&nbsp;&nbsp;&nbsp; [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure) |<span class='green-check'>&#9989;</span> |<span class='green-check'>&#9989;</span> | |
| &nbsp;&nbsp;&nbsp;&nbsp; Azure PowerShell |<span class='green-check'>&#9989;</span> |<span class='red-x'>&#10060;</span>   | |
| &nbsp;&nbsp;&nbsp;&nbsp; [AzCopy](https://docs.microsoft.com/azure/storage/storage-use-azcopy) |<span class='green-check'>&#9989;</span> |<span class='red-x'>&#10060;</span>   | [Azcopy in DSVM](./dsvm-tools-ingestion.md#azcopy)
| &nbsp;&nbsp;&nbsp;&nbsp; [Blob-FUSE-Treiber](https://github.com/Azure/azure-storage-fuse) |<span class='red-x'>&#10060;</span>   |<span class='green-check'>&#9989;</span> | [blobfuse in DSVM](./dsvm-tools-ingestion.md#blobfuse)
| &nbsp;&nbsp;&nbsp;&nbsp; [AdlCopy (Azure Data Lake Storage)](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-copy-data-azure-storage-blob) |<span class='green-check'>&#9989;</span> |<span class='red-x'>&#10060;</span>   | [Adlcopy in DSVM](./dsvm-tools-ingestion.md#adlcopy)
| &nbsp;&nbsp;&nbsp;&nbsp; [Azure Cosmos DB: Datenmigrationstool](https://docs.microsoft.com/azure/documentdb/documentdb-import-data) |<span class='green-check'>&#9989;</span> |<span class='red-x'>&#10060;</span>   | [Cosmos DB in DSVM](./dsvm-tools-ingestion.md#azure-cosmos-db-data-migration-tool) |
| &nbsp;&nbsp;&nbsp;&nbsp; [Microsoft-Datenverwaltungsgateway](https://msdn.microsoft.com/library/dn879362.aspx): Verschieben von Daten zwischen der lokalen Umgebung und der Cloud |<span class='green-check'>&#9989;</span> |<span class='red-x'>&#10060;</span>   | [DSVM-Verwendung](./dsvm-tools-ingestion.md#microsoft-data-management-gateway) |
| &nbsp;&nbsp;&nbsp;&nbsp; Unix/Linux-Befehlszeilentools |<span class='green-check'>&#9989;</span> |<span class='green-check'>&#9989;</span> | |
| [Apache Drill](https://drill.apache.org) für die Erkundung von Daten |<span class='green-check'>&#9989;</span> |<span class='green-check'>&#9989;</span> | [Apache Drill in DSVM](./dsvm-tools-explore-and-visualize.md#apache-drill)
| Tools für maschinelles Lernen: ||||
| &nbsp;&nbsp;&nbsp;&nbsp; Integration mit [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) (R, Python) |<span class='green-check'>&#9989;</span> |<span class='green-check'>&#9989;</span> | [Azure ML-SDK](./dsvm-ml-data-science-tools.md#azure-machine-learning-sdk-for-python)
| &nbsp;&nbsp;&nbsp;&nbsp; [XGBoost](https://github.com/dmlc/xgboost) |<span class='green-check'>&#9989;</span> |<span class='green-check'>&#9989;</span> | [XGBoost in DSVM](./dsvm-ml-data-science-tools.md#xgboost)
| &nbsp;&nbsp;&nbsp;&nbsp; [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit) |<span class='green-check'>&#9989;</span> |<span class='green-check'>&#9989;</span> | [Vowpal Wabbit in DSVM](./dsvm-ml-data-science-tools.md#vowpal-wabbit)
| &nbsp;&nbsp;&nbsp;&nbsp; [Weka](https://www.cs.waikato.ac.nz/ml/weka/) |<span class='green-check'>&#9989;</span> |<span class='green-check'>&#9989;</span> |
| &nbsp;&nbsp;&nbsp;&nbsp; [Rattle](https://togaware.com/rattle/) |<span class='green-check'>&#9989;</span> |<span class='green-check'>&#9989;</span> | [Weka in DSVM](./dsvm-ml-data-science-tools.md#weka) |
| &nbsp;&nbsp;&nbsp;&nbsp; [LightGBM](https://github.com/Microsoft/LightGBM) |<span class='red-x'>&#10060;</span>   |<span class='green-check'>&#9989;</span> (nur Ubuntu) | [LightGBM in DSVM](./dsvm-ml-data-science-tools.md#lightgbm)
| &nbsp;&nbsp;&nbsp;&nbsp; [CatBoost](https://tech.yandex.com/catboost/) |<span class='red-x'>&#10060;</span>   |<span class='green-check'>&#9989;</span> (nur Ubuntu) | |
| &nbsp;&nbsp;&nbsp;&nbsp; [H2O](https://www.h2o.ai/h2o/), [Sparkling Water](https://www.h2o.ai/sparkling-water/) |<span class='red-x'>&#10060;</span>   |<span class='green-check'>&#9989;</span> (nur Ubuntu) | [H20 in DSVM](./dsvm-ml-data-science-tools.md#h2o) |
| Deep Learning-Tools, die auf einer GPU oder CPU funktionieren: |  |  | Klicken Sie unten auf den Namen des Tools, um weitere Informationen zur Verwendung zu erhalten. |
| &nbsp;&nbsp;&nbsp;&nbsp; [Microsoft Cognitive Toolkit (CNTK)](./dsvm-deep-learning-ai-frameworks.md#microsoft-cognitive-toolkit-cntk) (Windows 2016) |<span class='green-check'>&#9989;</span> |<span class='green-check'>&#9989;</span> | |
| &nbsp;&nbsp;&nbsp;&nbsp; [TensorFlow](./dsvm-deep-learning-ai-frameworks.md#tensorflow) |<span class='green-check'>&#9989;</span> (Windows 2016) |<span class='green-check'>&#9989;</span> | |
| &nbsp;&nbsp;&nbsp;&nbsp; [Horovod](./dsvm-deep-learning-ai-frameworks.md#horovod) |<span class='red-x'>&#10060;</span>   |<span class='green-check'>&#9989;</span> (Ubuntu) | |
| &nbsp;&nbsp;&nbsp;&nbsp; [Caffe und Caffe2](./dsvm-deep-learning-ai-frameworks.md#caffe2) |<span class='red-x'>&#10060;</span>   |<span class='green-check'>&#9989;</span> | |
| &nbsp;&nbsp;&nbsp;&nbsp; [Chainer](./dsvm-deep-learning-ai-frameworks.md#chainer) |<span class='red-x'>&#10060;</span>   |<span class='green-check'>&#9989;</span> | |
| &nbsp;&nbsp;&nbsp;&nbsp; [Keras](./dsvm-deep-learning-ai-frameworks.md#keras)|<span class='red-x'>&#10060;</span>   |<span class='green-check'>&#9989;</span> | |
| &nbsp;&nbsp;&nbsp;&nbsp; [PyTorch](./dsvm-deep-learning-ai-frameworks.md#pytorch)|<span class='red-x'>&#10060;</span>   |<span class='green-check'>&#9989;</span> | |
| &nbsp;&nbsp;&nbsp;&nbsp; [TensorFlow Serving](./dsvm-deep-learning-ai-frameworks.md#tensorflow-serving) |<span class='red-x'>&#10060;</span>   |<span class='green-check'>&#9989;</span> | |
| &nbsp;&nbsp;&nbsp;&nbsp; [CUDA-, cuDNN-, NVIDIA-Treiber](./dsvm-deep-learning-ai-frameworks.md#cuda-cudnn-nvidia-driver) |<span class='green-check'>&#9989;</span> |<span class='green-check'>&#9989;</span> | |
