---
title: 'Referenz: DSVM unter Windows'
description: Details zu den Tools, die in der Data Science VM für Windows enthalten sind
author: gvashishtha
ms.service: machine-learning
ms.subservice: data-science-vm
ms.author: gopalv
ms.date: 09/11/2019
ms.topic: reference
ms.openlocfilehash: be4397de477891f2a698c9f7dcb131da79479529
ms.sourcegitcommit: 8a717170b04df64bd1ddd521e899ac7749627350
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71200017"
---
# <a name="reference-windows-data-science-virtual-machine"></a>Referenz: Data Science Virtual Machine für Windows

Nachstehend finden Sie eine Liste der Tools, die auf Ihrer Data Science Virtual Machine für Windows verfügbar sind. 

## <a name="microsoft-machine-learning-server-developer-edition"></a>Microsoft Machine Learning Server Developer Edition

Sie können Microsoft Enterprise Library für Ihre Analysen verwenden, da auf dem virtuellen Computer die Developer Edition von Machine Learning Server installiert ist. Machine Learning Server (ehemals Microsoft R Server) ist eine Analyseplattform mit einem breiten Bereitstellungsspektrum. Die Lösung ist skalierbar und wird kommerziell unterstützt.

Machine Learning Server unterstützt verschiedene Aufgaben für Big Data-Statistiken, Vorhersagemodellierung und Machine Learning. Die gesamte Bandbreite der Analytik wird unterstützt: Durchsuchen, Analyse, Visualisierung und Modellierung. Durch die Verwendung und Erweiterung der Open-Source-Sprachen R und Python ist Machine Learning Server kompatibel mit R- und Python-Skripts und -Funktionen. Darüber hinaus ist es auch mit CRAN-, Pip- und Conda-Paketen kompatibel, um Daten auf Unternehmensebene zu analysieren.

Mit der Möglichkeit, Daten parallel und in großen Stücken verarbeiten zu können, hebt Machine Learning Server die Speicherbeschränkungen der Open-Source-Sprache R auf. Somit können Sie Analysen von Daten, deren Größe die des Hauptspeichers bei weitem überschreitet, ausführen. 

Das auf dem virtuellen Computer befindliche Visual Studio Community Sie enthält die R-Tools für Visual Studio und die Python-Tools für die Visual Studio-Erweiterung (PTVS), die eine vollständige IDE für das Arbeiten mit R oder Python bieten. Auf dem virtuellen Computer werden auch noch andere IDEs bereitgestellt, etwa [RStudio](https://www.rstudio.com) und [PyCharm Community Edition](https://www.jetbrains.com/pycharm/).

## <a name="python"></a>Python

Für die Entwicklung mit Python wurden die Anaconda Python-Distributionen 2.7 und 3.6 installiert. Diese Distributionen enthalten die Python-Basisversion sowie etwa 300 der beliebtesten Pakete für Mathematik, Entwicklung und Datenanalysen. Sie können PTVS verwenden, das in Visual Studio Community 2017 installiert ist. Alternativ können Sie eine der mit Anaconda gebündelten IDEs verwenden (beispielsweise IDLE oder Spyder). Suchen Sie nach einem dieser Pakete (Windows-Logo-Taste+S), und öffnen Sie es.

> [!NOTE]
> Um unter Anaconda Python 2.7 auf Python Tools für Visual Studio zu verweisen, müssen Sie für jede Version eine angepasste Umgebung erstellen. Um diese Umgebungspfade in Visual Studio 2017 Community zu erstellen, navigieren Sie zu **Tools** > **Python-Tools** > **Python-Umgebungen**. Wählen Sie dann **+ Benutzerdefiniert** aus.

Anaconda Python 3.6 wird unter „C:\Anaconda“ installiert. Anaconda Python 2.7wird unter „C:\Anaconda\envs\python2“ installiert. Ausführliche Schritte finden Sie in der [PTVS-Dokumentation](https://docs.microsoft.com/visualstudio/python/installing-python-interpreters).

## <a name="the-jupyter-notebook"></a>Das Jupyter Notebook

Zur Anaconda-Distribution gehört außerdem Jupyter Notebook – eine Umgebung zum Freigeben von Code und Analysen. Der Jupyter Notebook-Server wurde bereits mit Python 2.7, Python 3.x, PySpark, Julia und R-Kernels konfiguriert. Verwenden Sie das Desktopsymbol **Jupyter Notebook**, um den Jupyter-Server und den Browser für den Zugriff auf den Notebookserver zu starten.

In den Python- und R-Paketen sind mehrere Beispiele für Notebooks enthalten. Nach dem Zugriff auf Jupyter zeigen die Notebooks, wie man mit den folgenden Technologien arbeitet:

* Machine Learning Server
* SQL Server Machine Learning Services, datenbankinterne Analysen
* Python
* Microsoft Cognitive Toolkit
* TensorFlow
* Andere Azure-Technologien

Der Link zu den Beispielen wird auf der Startseite für die Notebooks angezeigt, nachdem Sie sich mit dem im vorherigen Schritt festgelegten Kennwort bei Jupyter Notebook authentifiziert haben.

## <a name="visual-studio-community-2017"></a>Visual Studio Community 2017

Die DSVM enthält Visual Studio Community. Dies ist eine kostenlose Version der beliebten IDE von Microsoft, die Sie zur Evaluierung und für kleine Teams verwenden können. Weitere Informationen finden Sie in den [Microsoft-Softwarelizenzbedingungen](https://www.visualstudio.com/support/legal/mt171547).

Öffnen Sie Visual Studio über das Desktopsymbol oder das **Startmenü**. Suchen Sie nach Programmen (Windows-Logo-Taste+S) und anschließend nach **Visual Studio**. Von dort aus können Sie Projekte in Sprachen wie C#, Python, R und Node.js erstellen. Installierte Plug-Ins erleichtert die Arbeit mit den folgenden Azure-Diensten:

* Azure Data Catalog
* Azure HDInsight für Hadoop und Spark
* Azure Data Lake

Ein Plug-In namens „Azure Machine Learning for Visual Studio Code“ wird ebenfalls in Azure Machine Learning integriert und unterstützt Sie bei der schnellen Erstellung von KI-Anwendungen.

> [!NOTE]
> Es kann eine Meldung angezeigt werden, die besagt, dass der Evaluierungszeitraum abgelaufen ist. Geben Sie die Anmeldeinformationen Ihres Microsoft-Kontos ein. Oder erstellen Sie ein neues kostenloses Konto für den Zugriff auf Visual Studio Community.

## <a name="sql-server-2017-developer-edition"></a>SQL Server 2017 Developer Edition

Die DSVM wird mit einer Entwicklerversion von SQL Server 2017 mit Machine Learning Services ausgeliefert. Diese SQL Server-Edition ist entweder in R oder Python erhältlich und kann datenbankinterne Analysen ausführen. 

Machine Learning Services bietet eine Plattform zum Entwickeln und Bereitstellen von intelligenten Anwendungen. Sie können diese Sprachen sowie viele Pakete aus der Community zum Erstellen von Modellen und Generieren von Vorhersagen mithilfe Ihrer SQL Server-Daten nutzen. So ist die Analytics-Komponente nah an den Daten angeordnet, weil Machine Learning Services (datenbankintern) die Sprachen R und Python in SQL Server integriert. Durch diese Integration vermeiden Sie die Kosten und Sicherheitsrisiken, die mit einer Datenverschiebung verbunden sind.

> [!NOTE]
> Die Developer Edition von SQL Server ist nur für Entwicklungs- und Testzwecke bestimmt. Sie benötigen eine Lizenz für die Ausführung in der Produktion.

Sie können auf SQL Server zugreifen, indem Sie Microsoft SQL Server Management Studio öffnen. Der Name Ihres virtuellen Computers wird als **Servername** angegeben. Verwenden Sie die Windows-Authentifizierung, wenn Sie sich als Administrator unter Windows anmelden. Wenn SQL Server Management Studio gestartet wurde, können Sie andere Benutzer erstellen, Datenbanken erstellen, Daten importieren und SQL-Abfragen ausführen.

Um datenbankinterne Analysen mit SQL Server Machine Learning Services zu ermöglichen, führen Sie nach der Anmeldung als Serveradministrator in SQL Server Management Studio einmal den folgenden Befehl aus:

```
CREATE LOGIN [%COMPUTERNAME%\SQLRUserGroup] FROM WINDOWS
```

Ersetzen Sie `%COMPUTERNAME%` durch den VM-Namen.

## <a name="azure"></a>Azure

Mehrere Azure-Tools werden auf dem virtuellen Computer installiert:

* Über eine Desktopverknüpfung gelangen Sie zur Azure SDK-Dokumentation.
* Verwenden Sie „AzCopy“, um Daten in Ihr und aus Ihrem Azure -Speicherkonto zu kopieren. Um die Nutzung anzuzeigen, geben Sie an einer Eingabeaufforderung **Azcopy** ein.
* Verwenden Sie Azure Storage Explorer, um durch die in Ihrem Azure-Speicherkonto gespeicherten Objekte zu navigieren. Außerdem können Sie Daten in den und aus dem Azure Storage kopieren. Um auf dieses Tool zuzugreifen, geben Sie **Storage-Explorer** in das Feld **Suche** ein. Oder Sie suchen das Tool im Windows-**Startmenü**.
* „AdlCopy“ kopiert Daten in Azure Data Lake. Geben Sie **adlcopy** in einer Befehlszeile ein, um die Nutzung anzuzeigen.
* Das dtui-Tool kopiert Daten in und aus Azure Cosmos DB (eine NoSQL-Datenbank in der Cloud). Geben Sie an einer Eingabeaufforderung **dtui** ein.
* Integration Runtime kopiert Daten zwischen lokalen Datenquellen und der Cloud. Es wird in Tools wie Azure Data Factory verwendet.
* Verwenden Sie Azure PowerShell zur Verwaltung Ihrer Azure-Ressourcen in der PowerShell-Skriptsprache. Es ist auch auf Ihrem virtuellen Computer installiert.

## <a name="power-bi"></a>Power BI

In der DSVM-Instanz ist Power BI Desktop installiert, um Sie bei der Erstellung von Dashboards und Visualisierungen zu unterstützen. Verwenden Sie dieses Tool, um Daten aus verschiedenen Quellen abzurufen, Dashboards und Berichte zu erstellen und diese in der Cloud zu veröffentlichen. Weitere Informationen finden Sie auf der [Power BI-Website](https://powerbi.microsoft.com). Sie finden den Power BI Desktop im **Startmenü**.

> [!NOTE]
> Für den Zugriff auf Power BI benötigen Sie ein Microsoft Office 365-Konto.

## <a name="azure-machine-learning-sdk-for-python"></a>Azure Machine Learning SDK für Python

Das Azure Machine Learning-SDK für Python wird von Data Scientists und KI-Entwicklern zum Erstellen und Ausführen von Machine Learning-Workflows mit [Azure Machine Learning Service](../service/overview-what-is-azure-ml.md) verwendet. Sie können mit dem Dienst in Jupyter Notebooks oder anderen Python-IDE anhand von Open Source-Frameworks wie TensorFlow und scikit-learn interagieren.

Das Python-SDK ist in Microsoft Data Science Virtual Machine vorinstalliert. Informationen zu den ersten Schritten mit dem Python SDK finden Sie unter [Verwenden von Python zum Einstieg in Azure Machine Learning](../service/quickstart-create-workspace-with-python.md).

## <a name="more-microsoft-development-tools"></a>Weitere Microsoft-Entwicklungstools

Sie können den [Microsoft-Webplattform-Installer](https://www.microsoft.com/web/downloads/platform.aspx) verwenden, um weitere Microsoft-Entwicklungstools zu suchen und herunterzuladen. Es gibt auch eine Verknüpfung mit dem Tool auf dem Desktop der Microsoft Data Science Virtual Machine.  

## <a name="important-directories-on-the-virtual-machine"></a>Wichtige Verzeichnisse auf dem virtuellen Computer

| Item | Verzeichnis |
| --- | --- |
| Jupyter Notebook-Serverkonfigurationen | C:\ProgramData\jupyter |
| Basisverzeichnis für Jupyter Notebook-Beispiele | C:\dsvm\notebooks und c:\benutzer\\<benutzername\>\notebooks |
| Weitere Beispiele | C:\dsvm\samples |
| Anaconda, Standard: Python 3.6 | C:\Anaconda |
| Anaconda Python 2.7-Umgebung | C:\Anaconda\envs\python2 |
| Microsoft Machine Learning Server (eigenständig) für Python | C:\Programme\Microsoft\ML Server\PYTHON_SERVER |
| R-Standardinstanz, Machine Learning Server (eigenständig) | C:\Programme\Microsoft\ML Server\R_SERVER |
| SQL Server Machine Learning Services-Instanzverzeichnis (datenbankintern) | C:\Programme\Microsoft SQL Server\MSSQL14.MSSQLSERVER |
| Verschiedene Tools | C:\dsvm\tools |

> [!NOTE]
> Unter der Windows Server 2012-Edition der DSVM-Instanz und der Windows Server 2016-Edition vor März 2018 ist Python 2.7 die Anaconda-Standardumgebung. Die sekundäre Umgebung ist Python 3.5 (unter „C:\Anaconda\envs\py35“).

## <a name="next-steps"></a>Nächste Schritte

Haben Sie weitere Fragen? Erstellen Sie ggf. ein [Supportticket](https://azure.microsoft.com/support/create-ticket/).
