---
title: 'Schnellstart: Erstellen einer Windows-Instanz'
titleSuffix: Azure Data Science Virtual Machine
description: Konfigurieren und erstellen Sie für Analysen und Machine Learning einen virtuellen Computer für Data Science in Azure.
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: vijetajo
ms.author: vijetaj
ms.topic: quickstart
ms.date: 02/22/2019
ms.openlocfilehash: ac4b9b4d32d05083ceabd41207243eb483648baa
ms.sourcegitcommit: 32242bf7144c98a7d357712e75b1aefcf93a40cc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70278593"
---
# <a name="quickstart-set-up-a-windows-data-science-virtual-machine-on-azure"></a>Schnellstart: Einrichten einer Data Science Virtual Machine-Instanz für Linux in Azure

Die Microsoft Windows Data Science Virtual Machine (DSVM) ist ein Windows Server 2016-VM-Image in Azure. Es ist vorinstalliert und mit verschiedenen Tools für Datenanalyse und Machine Learning konfiguriert.

## <a name="included-data-science-tools"></a>Enthaltene Data Science-Tools

Die folgenden Tools sind in einer DSVM enthalten:

* Python SDK für [Azure Machine Learning Service](../index.yml).
* [Microsoft Machine Learning Server](https://docs.microsoft.com/machine-learning-server/index) Developer Edition.
* Anaconda Python Distribution.
* Jupyter Notebook mit R, Python und PySpark-Kernels.
* Microsoft Visual Studio Community.
* Microsoft Power BI Desktop.
* Microsoft SQL Server 2017 Developer Edition.
* Eine eigenständige Apache Spark-Instanz für lokale Entwicklung und Tests.
* [JuliaPro](https://juliacomputing.com/products/juliapro.html).
* Machine Learning und Datenanalysetools:
  * Deep Learning-Frameworks: Die KI-Frameworks [TensorFlow](https://www.tensorflow.org/), [Chainer](https://chainer.org/), MXNet und Keras sind auf dem virtuellen Computer vorinstalliert.
  * [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): Ein schnelles Machine Learning-System, das verschiedene Verfahren unterstützt, wie z. B. Online, Hashing, Allreduce, Reductions, Learning2Search sowie Active und Interactive Learning.
  * [XGBoost](https://xgboost.readthedocs.org/en/latest/): Ein Tool, das eine schnelle und präzise Boosted Tree-Implementierung ermöglicht.
  * [Rattle](https://togaware.com/rattle/): Das R-Analysetool für die ersten Schritte mit Datenanalysen und Machine Learning in R. Es umfasst eine GUI-basierte Datenuntersuchung und -modellierung mit automatischer R-Codegenerierung.
  * [Weka](https://www.cs.waikato.ac.nz/ml/weka/): Eine Software für visuelles Data Mining und Machine Learning in Java.
  * [Apache Drill](https://drill.apache.org/): Eine schemalose SQL-Abfrage-Engine für Apache Hadoop, NoSQL und Cloud Storage. Sie unterstützt ODBC- und JDBC-Schnittstellen für NoSQL- und Dateiabfragen mit BI-Standardtools wie Power BI, Microsoft Excel und Tableau.
* Bibliotheken in R und Python zur Verwendung in Azure Machine Learning und anderen Azure-Diensten.
* Git, einschließlich Git Bash, für die Arbeit mit Quellcoderepositorys wie GitHub und Azure DevOps. Git stellt mehrere gängige Linux-Befehlszeilentools bereit, auf die sowohl in Git Bash als auch über eine Eingabeaufforderung zugegriffen werden kann. Beispiele sind awk, sed, perl, grep, find, wget und curl.
* Entwicklungstools und Editoren (RStudio, PyCharm)

### <a name="about-data-science"></a>Informationen zu Data Science

Der Data Science-Vorgang umfasst eine Aufgabensequenz:

1. Suchen, Laden und Vorverarbeiten von Daten.
1. Kompilieren und testen von Modellen.
1. Bereitstellen der Modelle für die Nutzung in intelligenten Anwendungen.

Datenanalysten verwenden verschiedene Tools für diese Aufgaben. Es kann zeitaufwändig sein, die richtigen Softwareversionen zu finden und dann herunterzuladen und zu installieren. Die DSVM spart Zeit, indem sie ein einsatzbereites Image bietet, das mit vorinstallierten und konfigurierten gängigen Tools unter Azure bereitgestellt werden kann.

Die DSVM bringt Ihr Analyseprojekt entscheidend voran. Sie können in verschiedenen Sprachen arbeiten – unter anderem in R, Python, SQL und C#. Visual Studio bietet eine einfach zu bedienende integrierte Entwicklungsumgebung (IDE) zum Entwickeln und Testen des Codes. Mit dem Azure SDK des virtuellen Computers können Sie Ihre Anwendungen mit Diensten auf der Cloudplattform von Microsoft erstellen.

Für dieses DSVM-Image fallen keine Softwaregebühren an. Sie zahlen nur die Azure-Nutzungsgebühren. Diese hängen von der Größe des virtuellen Computers ab, den Sie bereitstellen. Weitere Informationen finden Sie unter [Data Science Virtual Machine - Windows 2016](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-dsvm.dsvm-windows).

### <a name="other-dsvm-versions"></a>Andere DSVM-Versionen

* Ein [Ubuntu](dsvm-ubuntu-intro.md)-Image. Es verfügt über ähnliche Tools wie DSVM sowie über einige Deep Learning-Frameworks.
* Ein [Linux CentOS](linux-dsvm-intro.md)-Image.
* Die [Windows Server 2012-Edition](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-dsvm.dsvm-windows) der Data Science VM. Einige Tools sind nur in der Windows Server 2016-Edition verfügbar. Ansonsten gilt dieser Artikel auch für die Windows Server 2012-Edition.

## <a name="prerequisite"></a>Voraussetzung

Um eine Microsoft Data Science Virtual Machine zu erstellen, benötigen Sie ein Azure-Abonnement. Siehe [Kostenlose Azure-Testversion](https://azure.com/free).

## <a name="create-your-dsvm"></a>Erstellen Ihrer DSVM

So erstellen Sie eine DSVM-Instanz:

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/#create/microsoft-dsvm.dsvm-windowsserver-2016)zur Auflistung der virtuellen Computer. Falls Sie sich noch nicht bei Ihrem Azure-Konto angemeldet haben, erhalten Sie ggf. eine entsprechende Aufforderung.
1. Wählen Sie unten auf dem Bildschirm die Option **Erstellen**.

   ![VM-Eintrag im Azure-Portal mit Schaltfläche „Erstellen“](./media/provision-vm/configure-data-science-virtual-machine.png)

1. Geben Sie die folgenden Informationen ein, um die einzelnen Schritte zu konfigurieren, die im rechten Bereich des Screenshots angezeigt werden:

   1. **Grundlagen**:
      * **Name**: Geben Sie den Namen der DSVM-Instanz ein.
      * **VM-Datenträgertyp**: Wählen Sie entweder **SSD** oder **HDD** aus. Wählen Sie für eine GPU-Instanz vom Typ „NC_v1“ (beispielsweise NVIDIA Tesla K80-basiert) **HDD** als Datenträgertyp aus.
      * **Benutzername**: Geben Sie die ID des Administratorkontos ein.
      * **Kennwort**: Geben Sie das Kennwort des Administratorkontos ein.
      * **Abonnement**: Falls Sie über mehrere Abonnements verfügen, wählen Sie das Abonnement aus, unter dem der Computer erstellt und abgerechnet werden soll.
      * **Ressourcengruppe**: Erstellen Sie eine neue Gruppe, oder verwenden Sie eine bereits vorhandene.
      * **Standort**: Wählen Sie das am besten geeignete Datencenter aus. Den schnellsten Netzwerkzugriff erreichen Sie mit dem Datencenter, in dem der größte Teil Ihrer Daten gespeichert ist oder das Ihrem physischen Standort am nächsten ist.
   1. **Größe**: Wählen Sie den Servertyp aus, der Ihre funktionalen Anforderungen erfüllt und Ihrem Kostenrahmen entspricht. Wählen Sie **Alle anzeigen** aus, um weitere Optionen für VM-Größen anzuzeigen.
   1. **Einstellungen**:  
      * **Managed Disks verwenden**. Wählen Sie **Verwaltet**, wenn Sie möchten, dass Azure die Datenträger für die VM verwaltet. Andernfalls müssen Sie ein neues oder vorhandenes Speicherkonto angeben.  
      * **Weitere Parameter**. Sie können die Standardwerte verwenden. Zeigen Sie mit der Maus auf den jeweiligen Informationslink, um Hilfe zu bestimmten Feldern anzuzeigen, falls Sie auch andere als die Standardwerte verwenden möchten.
   1. **Zusammenfassung**: Stellen Sie sicher, dass alle eingegebenen Informationen richtig sind. Klicken Sie auf **Erstellen**.

> [!NOTE]
> * Für den virtuellen Computer fallen lediglich die Computekosten für die Servergröße an, die Sie unter **Größe** ausgewählt haben.
> * Die Bereitstellung dauert zwischen 10 und 20 Minuten. Den Status Ihrer VM können Sie über das Azure-Portal einsehen.

## <a name="access-the-dsvm"></a>Zugreifen auf die DSVM-Instanz

Nachdem der virtuelle Computer erstellt und bereitgestellt wurde, können Sie über eine Remotedesktopverbindung darauf zugreifen. Verwenden Sie die Administratorkonto-Anmeldeinformationen, die Sie bei der VM-Erstellung im Schritt **Grundlagen** konfiguriert haben. 

Sie können nun beginnen, die auf der VM installierten und konfigurierten Tools zu verwenden. Auf viele der Tools kann über Kacheln im Menü **Start** sowie über Desktopsymbole zugegriffen werden.

Sie können eine DSVM-Instanz auch an Azure Notebooks anfügen, um Jupyter-Notebooks auf dem virtuellen Computer auszuführen und die Einschränkungen der kostenlosen Dienstebene zu umgehen. Weitere Informationen finden Sie unter [Computeebene](../../notebooks/configure-manage-azure-notebooks-projects.md#compute-tier).

<a name="tools"></a>

## <a name="tools-installed-on-the-dvsm"></a>In der DSVM-Instanz installierte Tools

In den folgenden Abschnitten erfahren Sie mehr über die Tools, die in der Data Science Virtual Machine-Instanz installiert sind.

### <a name="microsoft-machine-learning-server-developer-edition"></a>Microsoft Machine Learning Server Developer Edition

Sie können Microsoft Enterprise Library für Ihre Analysen verwenden, da auf dem virtuellen Computer die Developer Edition von Machine Learning Server installiert ist. Machine Learning Server (ehemals Microsoft R Server) ist eine Analyseplattform mit einem breiten Bereitstellungsspektrum. Die Lösung ist skalierbar und wird kommerziell unterstützt.

Machine Learning Server unterstützt verschiedene Aufgaben für Big Data-Statistiken, Vorhersagemodellierung und Machine Learning. Die gesamte Bandbreite der Analytik wird unterstützt: Durchsuchen, Analyse, Visualisierung und Modellierung. Durch die Verwendung und Erweiterung der Open-Source-Sprachen R und Python ist Machine Learning Server kompatibel mit R- und Python-Skripts und -Funktionen. Darüber hinaus ist es auch mit CRAN-, Pip- und Conda-Paketen kompatibel, um Daten auf Unternehmensebene zu analysieren.

Mit der Möglichkeit, Daten parallel und in großen Stücken verarbeiten zu können, hebt Machine Learning Server die Speicherbeschränkungen der Open-Source-Sprache R auf. Somit können Sie Analysen von Daten, deren Größe die des Hauptspeichers bei weitem überschreitet, ausführen. 

Das auf dem virtuellen Computer befindliche Visual Studio Community Sie enthält die R-Tools für Visual Studio und die Python-Tools für die Visual Studio-Erweiterung (PTVS), die eine vollständige IDE für das Arbeiten mit R oder Python bieten. Auf dem virtuellen Computer werden auch noch andere IDEs bereitgestellt, etwa [RStudio](https://www.rstudio.com) und [PyCharm Community Edition](https://www.jetbrains.com/pycharm/).

### <a name="python"></a>Python

Für die Entwicklung mit Python wurden die Anaconda Python-Distributionen 2.7 und 3.6 installiert. Diese Distributionen enthalten die Python-Basisversion sowie etwa 300 der beliebtesten Pakete für Mathematik, Entwicklung und Datenanalysen. Sie können PTVS verwenden, das in Visual Studio Community 2017 installiert ist. Alternativ können Sie eine der mit Anaconda gebündelten IDEs verwenden (beispielsweise IDLE oder Spyder). Suchen Sie nach einem dieser Pakete (Windows-Logo-Taste+S), und öffnen Sie es.

> [!NOTE]
> Um unter Anaconda Python 2.7 auf Python Tools für Visual Studio zu verweisen, müssen Sie für jede Version eine angepasste Umgebung erstellen. Um diese Umgebungspfade in Visual Studio 2017 Community zu erstellen, navigieren Sie zu **Tools** > **Python-Tools** > **Python-Umgebungen**. Wählen Sie dann **+ Benutzerdefiniert** aus.

Anaconda Python 3.6 wird unter „C:\Anaconda“ installiert. Anaconda Python 2.7wird unter „C:\Anaconda\envs\python2“ installiert. Ausführliche Schritte finden Sie in der [PTVS-Dokumentation](https://docs.microsoft.com/visualstudio/python/installing-python-interpreters).

### <a name="the-jupyter-notebook"></a>Das Jupyter Notebook

Zur Anaconda-Distribution gehört außerdem Jupyter Notebook – eine Umgebung zum Freigeben von Code und Analysen. Der Jupyter Notebook-Server wurde bereits mit Python 2.7, Python 3.x, PySpark, Julia und R-Kernels konfiguriert. Verwenden Sie das Desktopsymbol **Jupyter Notebook**, um den Jupyter-Server und den Browser für den Zugriff auf den Notebookserver zu starten.

In den Python- und R-Paketen sind mehrere Beispiele für Notebooks enthalten. Nach dem Zugriff auf Jupyter zeigen die Notebooks, wie man mit den folgenden Technologien arbeitet:

* Machine Learning Server
* SQL Server Machine Learning Services, datenbankinterne Analysen
* Python
* Microsoft Cognitive Toolkit
* TensorFlow
* Andere Azure-Technologien

Der Link zu den Beispielen wird auf der Startseite für die Notebooks angezeigt, nachdem Sie sich mit dem im vorherigen Schritt festgelegten Kennwort bei Jupyter Notebook authentifiziert haben.

### <a name="visual-studio-community-2017"></a>Visual Studio Community 2017

Die DSVM enthält Visual Studio Community. Dies ist eine kostenlose Version der beliebten IDE von Microsoft, die Sie zur Evaluierung und für kleine Teams verwenden können. Weitere Informationen finden Sie in den [Microsoft-Softwarelizenzbedingungen](https://www.visualstudio.com/support/legal/mt171547).

Öffnen Sie Visual Studio über das Desktopsymbol oder das **Startmenü**. Suchen Sie nach Programmen (Windows-Logo-Taste+S) und anschließend nach **Visual Studio**. Von dort aus können Sie Projekte in Sprachen wie C#, Python, R und Node.js erstellen. Installierte Plug-Ins erleichtert die Arbeit mit den folgenden Azure-Diensten:

* Azure Data Catalog
* Azure HDInsight für Hadoop und Spark
* Azure Data Lake

Ein Plug-In namens „Azure Machine Learning for Visual Studio Code“ wird ebenfalls in Azure Machine Learning integriert und unterstützt Sie bei der schnellen Erstellung von KI-Anwendungen.

> [!NOTE]
> Es kann eine Meldung angezeigt werden, die besagt, dass der Evaluierungszeitraum abgelaufen ist. Geben Sie die Anmeldeinformationen Ihres Microsoft-Kontos ein. Oder erstellen Sie ein neues kostenloses Konto für den Zugriff auf Visual Studio Community.

### <a name="sql-server-2017-developer-edition"></a>SQL Server 2017 Developer Edition

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

### <a name="azure"></a>Azure

Mehrere Azure-Tools werden auf dem virtuellen Computer installiert:

* Über eine Desktopverknüpfung gelangen Sie zur Azure SDK-Dokumentation.
* Verwenden Sie „AzCopy“, um Daten in Ihr und aus Ihrem Azure -Speicherkonto zu kopieren. Um die Nutzung anzuzeigen, geben Sie an einer Eingabeaufforderung **Azcopy** ein.
* Verwenden Sie Azure Storage Explorer, um durch die in Ihrem Azure-Speicherkonto gespeicherten Objekte zu navigieren. Außerdem können Sie Daten in den und aus dem Azure Storage kopieren. Um auf dieses Tool zuzugreifen, geben Sie **Storage-Explorer** in das Feld **Suche** ein. Oder Sie suchen das Tool im Windows-**Startmenü**.
* „AdlCopy“ kopiert Daten in Azure Data Lake. Geben Sie **adlcopy** in einer Befehlszeile ein, um die Nutzung anzuzeigen.
* Das dtui-Tool kopiert Daten in und aus Azure Cosmos DB (eine NoSQL-Datenbank in der Cloud). Geben Sie an einer Eingabeaufforderung **dtui** ein.
* Integration Runtime kopiert Daten zwischen lokalen Datenquellen und der Cloud. Es wird in Tools wie Azure Data Factory verwendet.
* Verwenden Sie Azure PowerShell zur Verwaltung Ihrer Azure-Ressourcen in der PowerShell-Skriptsprache. Es ist auch auf Ihrem virtuellen Computer installiert.

### <a name="power-bi"></a>Power BI

In der DSVM-Instanz ist Power BI Desktop installiert, um Sie bei der Erstellung von Dashboards und Visualisierungen zu unterstützen. Verwenden Sie dieses Tool, um Daten aus verschiedenen Quellen abzurufen, Dashboards und Berichte zu erstellen und diese in der Cloud zu veröffentlichen. Weitere Informationen finden Sie auf der [Power BI-Website](https://powerbi.microsoft.com). Sie finden den Power BI Desktop im **Startmenü**.

> [!NOTE]
> Für den Zugriff auf Power BI benötigen Sie ein Microsoft Office 365-Konto.

### <a name="azure-machine-learning-sdk-for-python"></a>Azure Machine Learning SDK für Python

Das Azure Machine Learning-SDK für Python wird von Data Scientists und KI-Entwicklern zum Erstellen und Ausführen von Machine Learning-Workflows mit [Azure Machine Learning Service](../service/overview-what-is-azure-ml.md) verwendet. Sie können mit dem Dienst in Jupyter Notebooks oder anderen Python-IDE anhand von Open Source-Frameworks wie TensorFlow und scikit-learn interagieren.

Das Python-SDK ist in Microsoft Data Science Virtual Machine vorinstalliert. Informationen zu den ersten Schritten mit dem Python SDK finden Sie unter [Verwenden von Python zum Einstieg in Azure Machine Learning](../service/quickstart-create-workspace-with-python.md).

## <a name="more-microsoft-development-tools"></a>Weitere Microsoft-Entwicklungstools

Sie können den [Microsoft-Webplattform-Installer](https://www.microsoft.com/web/downloads/platform.aspx) verwenden, um weitere Microsoft-Entwicklungstools zu suchen und herunterzuladen. Es gibt auch eine Verknüpfung mit dem Tool auf dem Desktop der Microsoft Data Science Virtual Machine.  

## <a name="important-directories-on-the-vm"></a>Wichtige Verzeichnisse auf dem virtuellen Computer

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

* Erkunden Sie die Tools in der DSVM-Instanz über das Menü **Start**.
* Machen Sie sich mit Azure Machine Learning Service vertraut. Lesen Sie hierzu [Was ist Azure Machine Learning Service?](../service/overview-what-is-azure-ml.md), und probieren Sie die [Tutorials](../index.yml) aus.
* Navigieren Sie im Explorer zu „C:\Programme\Microsoft\ML Server\R_SERVER\library\RevoScaleR\demoScripts“, um Beispiele für die RevoScaleR-Bibliothek in R zu finden, die Datenanalysen auf Unternehmensebene unterstützt. 
* Lesen Sie den Artikel [Zehn Dinge, die Sie mit der Data Science Virtual Machine machen können](https://aka.ms/dsvmtenthings).
* Erfahren Sie, wie Sie mithilfe des [Team Data Science-Prozesses](../team-data-science-process/index.yml)systematisch vollständige Analyselösungen erstellen.
* Besuchen Sie den [Azure KI-Katalog](https://gallery.cortanaintelligence.com), um Beispiele zu Machine Learning und zur Datenanalyse zu erhalten, in denen Azure Machine Learning und dazugehörige Datendienste in Azure verwendet werden. Wir haben auch ein Symbol für diese Galerie im **Startmenü** und auf dem Desktop der virtuellen Maschine bereitgestellt.
