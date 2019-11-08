---
title: 'Referenz: DSVM unter CentOS'
description: Details zu den Tools, die in der Data Science Virtual Machine für CentOS enthalten sind
author: gvashishtha
ms.service: machine-learning
ms.subservice: data-science-vm
ms.author: gopalv
ms.date: 09/11/2019
ms.topic: reference
ms.openlocfilehash: 0f71a8af2f7d2cfbfe43c0cfcc84cc7c08109c32
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73493715"
---
# <a name="reference-centos-linux-data-science-virtual-machine"></a>Referenz: Data Science Virtual Machine für CentOS (Linux)

Eine Data Science Virtual Machine-Instanz (DSVM) mit Linux ist ein CentOS-basierter virtueller Azure-Computer. Die Linux-DSVM-Instanz enthält eine Sammlung vorinstallierter Tools, die Sie für die Datenanalyse sowie für maschinelles Lernen verwenden können. 

Die wichtigsten in einer Linux-DSVM-Instanz enthaltenen Softwarekomponenten sind:

* Betriebssystem: Linux CentOS-Distribution
* Microsoft Machine Learning Server
* Anaconda Python-Distribution (Versionen 3.5 und 2.7), einschließlich gängiger Datenanalysebibliotheken
* JuliaPro, eine betreute Distribution der Sprache Julia mit gängigen Wissenschafts- und Datenanalysebibliotheken
* Eigenständige Spark-Instanz und Hadoop für einen einzelnen Knoten (HDFS, YARN)
* JupyterHub, ein Jupyter Notebook-Server für mehrere Benutzer mit Unterstützung von R-, Python-, PySpark- und Julia-Kernels
* Azure Storage-Explorer.
* Azure CLI, die Azure-Befehlszeilenschnittstelle für die Verwaltung von Azure-Ressourcen
* PostgreSQL-Datenbank
* Tools für maschinelles Lernen:
  * [Microsoft Cognitive Toolkit](https://github.com/Microsoft/CNTK) (CNTK), ein Deep Learning-Softwaretoolkit von Microsoft Research
  * [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit), ein schnelles System für maschinelles Lernen, das verschiedene Verfahren unterstützt, wie z.B. Online, Hashing, Allreduce, Reductions, Learning2Search sowie aktives und interaktives Lernen
  * [XGBoost](https://xgboost.readthedocs.org/en/latest/), ein Tool, das eine schnelle und präzise Boosted Tree-Implementierung ermöglicht
  * [Rattle](https://togaware.com/rattle/), ein Tool, das den Einstieg in Datenanalysen und maschinelles Lernen in R vereinfacht. Rattle ermöglicht ein GUI-basiertes Durchsuchen von Daten sowie das Modellieren mit automatischer R-Codegenerierung.
* Azure SDK in Java, Python, Node.js, Ruby und PHP
* Bibliotheken in R und Python zur Verwendung in Azure Machine Learning und anderen Azure-Diensten
* Entwicklungstools und -editoren (RStudio, PyCharm, IntelliJ, Emacs, gedit, vi)

Der Data Science-Vorgang umfasst eine Aufgabensequenz:

1. Suchen, Laden und Vorverarbeiten von Daten
1. Kompilieren und testen von Modellen.
1. Bereitstellen der Modelle für die Nutzung in intelligenten Anwendungen.

Datenanalysten verwenden verschiedene Tools zum Ausführen dieser Aufgaben. Es kann zeitaufwendig sein, die richtigen Versionen der Software zu finden und diese dann herunterzuladen, zu kompilieren und zu installieren.

Die Linux-DSVM-Instanz vereinfacht diesen Vorgang erheblich. Nutzen Sie die Linux-DSVM-Instanz für einen schnellen Einstieg in Ihr Analyseprojekt. Die Linux-DSVM-Instanz ermöglicht es Ihnen, in verschiedenen Sprachen zu arbeiten, u. a. in R, Python, SQL, Java und C++. Eclipse bietet eine benutzerfreundliche IDE zum Entwickeln und Testen von Code. Mit dem in DSVM integrierten Azure SDK können Sie Ihre Anwendungen mithilfe verschiedener Dienste unter Linux für die Microsoft-Cloudplattform erstellen. Andere Sprachen sind vorinstalliert, einschließlich Ruby, Perl, PHP und Node.js.

Für dieses DSVM-Image fallen keine Softwaregebühren an. Sie zahlen nur die Gebühren für die Azure-Hardwarenutzung, die basierend auf der Größe des virtuellen Computers berechnet werden, den Sie mit dem DSVM-Image bereitstellen. Weitere Informationen zu den Computegebühren finden Sie im Azure Marketplace unter [Data Science Virtual Machine für Linux (CentOS)](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm/).


## <a name="machine-learning-server"></a>Machine Learning Server

R ist eine der beliebtesten Sprachen für Datenanalyse und Machine Learning. Wenn Sie R für Ihre Analysen verwenden möchten, können Sie Machine Learning Server mit Microsoft R Open und Math Kernel Library in DSVM verwenden. Math Kernel Library optimiert gängige mathematische Vorgänge in Analysealgorithmen. R Open ist vollständig kompatibel mit CRAN R. Alle in CRAN veröffentlichten R-Bibliotheken können unter R Open installiert werden. 

Sie können Machine Learning Server zum Skalieren und Operationalisieren von R-Modellen in Webdienste verwenden. Sie können Ihre R-Programme in einem Standard-Editor wie RStudio, vi oder Emacs bearbeiten. Der Emacs-Editor ist in der DSVM-Instanz vorinstalliert. Das Emacs ESS-Paket (Emacs Speaks Statistics) vereinfacht die Verwendung von R-Dateien im Emacs-Editor.

Geben Sie zum Öffnen der R-Konsole in der Shell **R** ein. Über diesen Befehl gelangen Sie zu einer interaktiven Umgebung. Zum Entwickeln Ihres R-Programms verwenden Sie in der Regel einen Editor wie Emacs oder vi und führen dann die Skripts in R aus. Mit RStudio verfügen Sie über eine vollständige grafische IDE zum Entwickeln Ihres R-Programms.

Ein R-Skript, mit dem Sie die [beliebtesten 20 R-Pakete](https://www.kdnuggets.com/2015/06/top-20-r-packages.html) installieren können, ist in DSVM enthalten. Sie können dieses Skript ausführen, wenn Sie sich auf der interaktiven R-Oberfläche befinden. Geben Sie wie bereits erwähnt zum Öffnen der Schnittstelle in der Shell **R** ein.  

## <a name="python"></a>Python

Anaconda Python wird mit Python 3.5- und 2.7-Umgebungen installiert. Die 2.7-Umgebung wird als _root_ bezeichnet und die 3.5-Umgebung als _py35_. Diese Distribution enthält die Python-Basisversion sowie etwa 300 der beliebtesten Pakete für Mathematik, Entwicklung und Datenanalysen.

Standardmäßig wird die py35-Umgebung verwendet. Sie können die root-Umgebung (2.7) mit dem folgenden Befehl aktivieren:

```bash
source activate root
```

Verwenden Sie den folgenden Befehl zum erneuten Aktivieren der py35-Umgebung:

```bash
source activate py35
```

Geben Sie zum Aufrufen einer interaktiven Sitzung in der Shell **python** ein. 

Installieren Sie mithilfe von Conda oder pip weitere Python-Bibliotheken. Aktivieren Sie für pip zuerst die richtige Umgebung, wenn Sie die Standardeinstellung nicht verwenden möchten:

```bash
source activate root
pip install <package>
```

Oder geben Sie den vollständigen Pfad zu Pip an:

```bash
/anaconda/bin/pip install <package>
```

Für Conda müssen Sie immer den Umgebungsnamen angeben (py35 oder root):

```bash
conda install <package> -n py35
```

Wenn Sie sich auf einer grafischen Benutzeroberfläche befinden oder die X11-Weiterleitung eingerichtet haben, können Sie **pycharm** eingeben, um die PyCharm Python-IDE zu starten. Sie können die standardmäßigen Text-Editoren verwenden. Außerdem können Sie Spyder nutzen, eine Python-IDE, die als Bündel mit Anaconda Python-Distributionen bereitgestellt wird. Für Spyder wird ein grafischer Desktop oder die X11-Weiterleitung benötigt. Auf dem grafischen Desktop befindet sich eine Verknüpfung mit Spyder.

## <a name="jupyter-notebook"></a>Jupyter Notebook

Zur Anaconda-Distribution gehört außerdem Jupyter Notebook, eine Umgebung zum Freigeben von Code und Analysen. Der Zugriff auf Jupyter Notebook erfolgt über JupyterHub. Melden Sie sich mit Ihrem lokalen Linux-Benutzernamen und dem dazugehörigen Kennwort an.

Der Jupyter Notebook-Server ist mit Python 2-, Python 3- und R-Kernels vorkonfiguriert. Verwenden Sie das Desktopsymbol **Jupyter Notebook**, um den Browser zu öffnen und auf den Jupyter Notebook-Server zuzugreifen. Wenn Sie per SSH oder über den X2Go-Client auf DSVM zugreifen, erreichen Sie den Jupyter Notebook-Server auch über https:\//localhost:8000/ zugreifen.

> [!NOTE]
> Fahren Sie fort, falls Sie Zertifikatwarnungen erhalten.

Sie können von jedem Host aus auf den Jupyter Notebook-Server zugreifen. Geben Sie **https:\//\<DSVM-DNS-Name oder -IP-Adresse\>:8000/** ein.

> [!NOTE]
> Port 8000 wird standardmäßig in der Firewall geöffnet, wenn die DSVM-Instanz bereitgestellt wird. 

Microsoft hat einige Beispiel-Notebooks zusammengestellt – eins für Python und eins für R. Der Link zu den Beispielen wird Ihnen auf der Jupyter Notebook-Startseite angezeigt, nachdem Sie sich mit Ihrem lokalen Linux-Benutzernamen und dem dazugehörigen Kennwort Jupyter Notebook authentifiziert haben. Wenn Sie ein neues Notebook erstellen möchten, wählen Sie **Neu** und dann den zu verwendenden Sprachkernel aus. Wenn die Schaltfläche **New** (Neu) nicht angezeigt wird, wählen Sie oben links das **Jupyter**-Symbol aus, um die Startseite des Notebook-Servers zu öffnen.

## <a name="spark-standalone"></a>Eigenständige Spark-Instanz 

Eine Instanz des Modus für die eigenständige Spark-Instanz ist in der Linux-DSVM-Instanz vorinstalliert, damit Sie Spark-Anwendungen zunächst lokal entwickeln können, bevor Sie sie in großen Clustern testen und bereitstellen. 

Sie können PySpark-Programme über den Jupyter-Kernel ausführen. Wenn Sie Jupyter öffnen und die Schaltfläche **New** (Neu) auswählen, wird eine Liste mit verfügbaren Kernels angezeigt. **Spark – Python** ist der PySpark-Kernel, mit dem Sie Spark-Anwendungen in der Sprache Python erstellen können. Sie können auch eine Python-IDE wie PyCharm oder Spyder verwenden, um Spark-Programme zu erstellen. 

In dieser eigenständigen Instanz wird der Spark-Stapel im aufrufenden Clientprogramm ausgeführt. Dieses Feature führt dazu, dass sich Probleme schneller und einfacher beheben lassen als beim Entwickeln in einem Spark-Cluster.

Jupyter stellt ein PySpark-Beispielnotebook bereit. Sie finden dieses Notebook im Basisverzeichnis von Jupyter im Verzeichnis „SparkML“ ($HOME/Notebooks/SparkML/PySpark). 

Wenn Sie in R für Spark programmieren, können Sie Machine Learning Server, SparkR oder sparklyr verwenden. 

Vor dem Ausführen im Spark-Kontext in Machine Learning Server müssen Sie einen einmaligen Schritt zur Einrichtung ausführen, um eine lokale Hadoop HDFS- und YARN-Instanz für einen Knoten zu aktivieren. Standardmäßig gilt, dass die Hadoop-Dienste installiert, aber auf der DSVM deaktiviert sind. Führen Sie die folgenden Befehle zum Aktivieren von Hadoop-Diensten beim ersten Mal als root aus:

```bash
echo -e 'y\n' | ssh-keygen -t rsa -P '' -f ~hadoop/.ssh/id_rsa
cat ~hadoop/.ssh/id_rsa.pub >> ~hadoop/.ssh/authorized_keys
chmod 0600 ~hadoop/.ssh/authorized_keys
chown hadoop:hadoop ~hadoop/.ssh/id_rsa
chown hadoop:hadoop ~hadoop/.ssh/id_rsa.pub
chown hadoop:hadoop ~hadoop/.ssh/authorized_keys
systemctl start hadoop-namenode hadoop-datanode hadoop-yarn
```

Sie können die Hadoop-bezogenen Dienste anhalten, wenn Sie sie nicht benötigen. Führen Sie dazu `systemctl stop hadoop-namenode hadoop-datanode hadoop-yarn` aus.

Das Verzeichnis „/dsvm/samples/MRS“ umfasst ein Beispiel, in dem gezeigt wird, wie Machine Learning Server in einem Remote-Spark-Kontext (dies ist die eigenständige Spark-Instanz in der DSVM-Instanz) entwickelt und getestet wird.

## <a name="ides-and-editors"></a>IDEs und Editoren

Sie können aus mehreren Code-Editoren auswählen, einschließlich vi/VIM, Emacs, gedit, PyCharm, RStudio, Eclipse, LaTex und IntelliJ. 

* gedit, Eclipse, IntelliJ, R Studio und PyCharm sind grafische Editoren. Sie müssen bei einem grafischen Desktop angemeldet sein, um sie verwenden zu können. Sie können diese über Desktop- und Anwendungsmenüverknüpfungen öffnen.

* Vim und Emacs sind textbasierte Editoren. Bei Emacs erleichtert das ESS-Add-On-Paket die Arbeit mit R im Emacs-Editor. Weitere Informationen finden Sie auf der [ESS-Website](https://ess.r-project.org/).

* Eclipse ist eine erweiterbare Open-Source-IDE, die mehrere Sprachen unterstützt. Die Eclipse-IDE für Java-Entwickler ist die Version, die in der DSVM-Instanz installiert ist. Sie können Plug-Ins für mehrere beliebte Sprachen installieren, um die Umgebung zu erweitern. 

  Das Azure-Toolkit für Eclipse-Plug-In wird mit Eclipse in der DSVM-Instanz installiert. Sie können das Azure-Toolkit für Eclipse zum Erstellen, Entwickeln, Testen und Bereitstellen von Azure-Anwendungen nutzen, indem Sie die Eclipse-Entwicklungsumgebung verwenden, die Sprachen wie Java unterstützt.

  Das Azure SDK für Java wird auch mit dem Azure-Toolkit für Eclipse in der DSVM-Instanz installiert. Das Azure SDK für Java ermöglicht Ihnen den Zugriff auf unterschiedliche Azure-Dienste aus einer Java-Umgebung. 
  
  Weitere Informationen finden Sie unter [Azure-Toolkit für Eclipse](/java/azure/eclipse/azure-toolkit-for-eclipse).

* LaTeX wird über das texlive-Paket zusammen mit einem Emacs-Add-On-Paket namens [AUCTeX](https://www.gnu.org/software/auctex/manual/auctex/auctex.html) installiert. Dieses Paket vereinfacht das Erstellen von LaTeX-Dokumenten in Emacs. 

## <a name="databases"></a>Datenbanken

Die Linux-DSVM-Instanz bietet Ihnen Zugriff auf mehrere Datenbank- und Befehlszeilentools.

### <a name="postgressql"></a>PostgresSQL

Die Open-Source-Datenbank PostgresSQL ist in der DSVM-Instanz verfügbar. Die Dienste werden ausgeführt, und initdb ist bereits abgeschlossen. Sie müssen Datenbanken und Benutzer noch erstellen. Weitere Informationen finden Sie in der [PostgresSQL-Dokumentation](https://www.postgresql.org/docs/).  

### <a name="squirrel-sql"></a>SQuirreL SQL

SQuirreL SQL ist ein grafischer SQL-Client, der eine Verbindung mit verschiedenen Datenbanken (einschließlich SQL Server, PostgresSQL und MySQL) herstellen und SQL-Abfragen ausführen kann. Sie können SQuirreL SQL in einer grafischen Desktopsitzung (z. B. mithilfe des X2Go-Clients) über ein Desktopsymbol ausführen. Alternativ können Sie den Client mithilfe des folgenden Befehls in der Shell ausführen:

```bash
/usr/local/squirrel-sql-3.7/squirrel-sql.sh /usr/local/squirrel-sql-3.7/squirrel-sql.sh
```

Vor der ersten Verwendung müssen Sie Ihre Treiber und Datenbankaliase einrichten. Die JDBC-Treiber befinden sich unter „/usr/share/Java/jdbcdrivers“.

Weitere Informationen finden Sie unter [SQuirreL SQL](http://squirrel-sql.sourceforge.net/index.php?page=screenshots).

### <a name="command-line-tools-for-accessing-sql-server"></a>Befehlszeilentools für den Zugriff auf SQL Server

Das ODBC-Treiberpaket für SQL Server verfügt auch über zwei Befehlszeilentools:

* **bcp**: Das Tool bcp kopiert Daten per Massenkopiervorgang zwischen einer Instanz von SQL Server und einer Datendatei in einem vom Benutzer angegebenen Format. Das Tool bcp kann zum Importieren großer Mengen an neuen Zeilen in SQL Server-Tabellen oder zum Exportieren von Daten aus Tabellen in Datendateien verwendet werden. Zum Importieren von Daten in eine Tabelle müssen Sie eine für diese Tabelle erstellte Formatdatei verwenden. Alternativ müssen Sie die Struktur der Tabelle und die Datentypen, die für ihre Spalten gelten, verstehen.

  Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit bcp](https://msdn.microsoft.com/library/hh568446.aspx).

* **sqlcmd**: Mit dem Hilfsprogramm sqlcmd können Sie Transact-SQL-Anweisungen, Systemprozeduren und Skriptdateien an der Eingabeaufforderung eingeben. Für das Hilfsprogramm sqlcmd wird ODBC zum Ausführen von Transact-SQL-Batches verwendet.

  Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit sqlcmd](https://msdn.microsoft.com/library/hh568447.aspx).

  > [!NOTE]
  > Bei diesem Tool gibt es einige Unterschiede zwischen Linux- und Windows-Plattformen. Ausführliche Informationen dazu finden Sie in der -Dokumentation.

### <a name="database-access-libraries"></a>Bibliotheken für den Datenbankzugriff

Bibliotheken für den Datenbankzugriff sind in R und Python verfügbar:

* In R können Sie mit den Paketen RODBC oder dplyr SQL-Anweisungen auf dem Datenbankserver abfragen oder ausführen.
* In Python ermöglicht die Bibliothek pyodbc den Datenbankzugriff mit ODBC als zugrunde liegender Schicht.

## <a name="azure-tools"></a>Azure-Tools

Die folgenden Azure-Tools werden in der DSVM-Instanz installiert:

* **Azure CLI**: Mit der Befehlszeilenschnittstelle in Azure können Sie Azure-Ressourcen über Shellbefehle erstellen und verwalten. Geben Sie **azure help** ein, um die Azure-Tools zu öffnen. Weitere Informationen finden Sie auf der [Dokumentationsseite zur Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).
* **Azure Storage-Explorer**: Azure Storage-Explorer ist ein grafisches Tool zum Navigieren durch die Objekte, die Sie in Ihrem Azure-Speicherkonto gespeichert haben, sowie zum Hoch- und Herunterladen von Daten in und aus Azure-Blobs. Sie können über das Symbol der Desktopverknüpfung auf den Storage-Explorer zugreifen. Sie können ihn auch über eine Eingabeaufforderung der Shell öffnen, indem Sie **StorageExplorer**eingeben. Sie müssen über einen X2Go-Client angemeldet sein oder die X11-Weiterleitung eingerichtet haben.
* **Azure-Bibliotheken**: Die folgenden Bibliotheken sind in der DSVM-Instanz vorinstalliert:
  
  * **Python**: Die zu Azure gehörenden Bibliotheken in Python sind *azure*, *azureml*, *pydocumentdb* und *pyodbc*. Mit den ersten drei Bibliotheken können Sie auf Azure-Speicherdienste, Azure Machine Learning und Azure Cosmos DB (eine NoSQL-Datenbank in Azure) zugreifen. Mit der vierten Bibliothek, pyodbc (zusammen mit dem Microsoft ODBC-Treiber für SQL Server), können Sie unter Verwendung einer ODBC-Schnittstelle über Python auf SQL Server, Azure SQL-Datenbank und Azure SQL Data Warehouse zugreifen. Geben Sie **pip list** ein, um alle aufgeführten Bibliotheken anzuzeigen. Achten Sie darauf, dass dieser Befehl sowohl in der Python 2.7- als auch in der Python 3.5-Umgebung ausgeführt wird.
  * **R**: Die zu Azure gehörenden Bibliotheken in R sind AzureML und RODBC.
  * **Java**: Die Liste mit den Azure Java-Bibliotheken befindet sich im Verzeichnis „/dsvm/sdk/AzureSDKJava“ in der DSVM-Instanz. Die wichtigsten Bibliotheken sind Azure-Speicher- und -Verwaltungs-APIs, Azure Cosmos DB und JDBC-Treiber für SQL Server.  

Sie können über den vorinstallierten Firefox-Browser auf das [Azure-Portal](https://portal.azure.com) zugreifen. Im Azure-Portal können Sie Azure-Ressourcen erstellen, verwalten und überwachen.

## <a name="azure-machine-learning"></a>Azure Machine Learning

Azure Machine Learning ist ein vollständig verwalteter Clouddienst, mit dem Sie Predictive Analytics-Lösungen erstellen, bereitstellen und freigeben können. Sie erstellen Ihre Experimente und Modelle mit dem Azure Machine Learning Studio (klassisch). Wenn Sie auf Azure Machine Learning über einen Webbrowser der DSVM-Instanz zugreifen möchten, navigieren Sie zu [Microsoft Azure Machine Learning](https://studio.azureml.net).

Nachdem Sie sich bei Azure Machine Learning Studio (klassisch) angemeldet haben, haben Sie Zugriff auf einen Zeichenbereich zum Experimentieren, in dem Sie einen logischen Ablauf für die Algorithmen des maschinellen Lernens erstellen können. Sie haben außerdem auch Zugriff auf eine Jupyter Notebook-Instanz, die in Azure Machine Learning gehostet wird. Das Notebook kann problemlos mit den Experimenten in Azure Machine Learning Studio (klassisch) verwendet werden. 

Operationalisieren Sie die von Ihnen erstellten Machine Learning-Modelle, indem Sie sie mit einer Webdienstschnittstelle umschließen. Durch das Operationalisieren von Machine Learning-Modellen können Clients, die in einer beliebigen Sprache geschrieben sind, Vorhersagen aus diesen Modellen aufrufen. Weitere Informationen finden Sie in der [Dokumentation zu Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/).

Sie können Ihre Modelle auch in R oder Python in der DSVM-Instanz erstellen und diese dann in der Produktion in Azure Machine Learning bereitstellen. Microsoft hat Bibliotheken in R (**AzureML**) und Python (**azureml**) installiert, um diese Funktion zu unterstützen.

Informationen zum Bereitstellen von Modellen in R und Python für Azure Machine Learning finden Sie unter [Zehn Dinge, die Sie mit der Windows Data Science Virtual Machine machen können](vm-do-ten-things.md).

> [!NOTE]
> Die Anweisungen unter [Zehn Dinge, die Sie mit der Windows Data Science Virtual Machine machen können](vm-do-ten-things.md) wurden für die Windows-Version der DSVM-Instanz erstellt. Die Informationen zum Bereitstellen von Modellen für Azure Machine Learning gelten jedoch auch für die Linux-DSVM-Instanz.

## <a name="machine-learning-tools"></a>Machine Learning-Tools

DSVM enthält einige Machine Learning-Tools und -Algorithmen, die vorkompiliert und lokal vorinstalliert wurden. Das umfasst:

* **Microsoft Cognitive Toolkit**: Ein Deep Learning-Toolkit.
* **Vowpal Wabbit**: Ein Algorithmus für schnelles Onlinelernen.
* **XGBoost**: Ein Tool, das optimierte Boosted Tree-Algorithmen bereitstellt.
* **Python**: Anaconda Python wird als Paket mit Machine Learning-Algorithmen für Bibliotheken wie Scikit-learn bereitgestellt. Sie können andere Bibliotheken installieren, indem Sie den `pip install` -Befehl verwenden.
* **R**: Für R ist eine umfassende Bibliothek mit Machine Learning-Funktionen verfügbar. Zu den vorinstallierten Bibliotheken zählen u. a. lm, glm, randomForest und rpart. Sie können andere Bibliotheken installieren, indem Sie `install.packages(<lib name>)` ausführen.

Microsoft Cognitive Toolkit, Vowpal Wabbit und XGBoost werden in den nächsten Abschnitten ausführlicher erläutert.

### <a name="microsoft-cognitive-toolkit"></a>Microsoft Cognitive Toolkit

Microsoft Cognitive Toolkit ist ein Open-Source-Toolkit für Deep Learning. Es handelt sich um ein Befehlszeilentool (CNTK), das sich bereits im Pfad (PATH) befindet.

Um ein einfaches Beispiel auszuführen, verwenden Sie die folgenden Befehle in der Shell:

```bash
cd /home/[USERNAME]/notebooks/CNTK/HelloWorld-LogisticRegression
cntk configFile=lr_bs.cntk makeMode=false command=Train
```

Weitere Informationen finden Sie im [GitHub-CNTK-Repository](https://github.com/Microsoft/CNTK) und im [CNTK-Wiki](https://github.com/Microsoft/CNTK/wiki).

### <a name="vowpal-wabbit"></a>Vowpal Wabbit

Vowpal Wabbit ist ein Machine Learning-System, das verschiedene Verfahren einsetzt, etwa Online, Hashing, Allreduce, Reductions, Learning2Search sowie aktives und interaktives Lernen.

Verwenden Sie die folgenden Befehle, um das Tool für ein einfaches Beispiel auszuführen:

```bash
cp -r /dsvm/tools/VowpalWabbit/demo vwdemo
cd vwdemo
vw house_dataset
```

Das Vowpal Wabbit-Demoverzeichnis enthält andere, größere Demos. Weitere Informationen zu Vowpal Wabbit finden Sie im [GitHub-Vowpal Wabbit-Repository](https://github.com/JohnLangford/vowpal_wabbit) und im [Vowpal Wabbit-Wiki](https://github.com/JohnLangford/vowpal_wabbit/wiki).

### <a name="xgboost"></a>XGBoost

Die XGBoost-Bibliothek wurde für Boosted (Tree)-Algorithmen entworfen und optimiert. Das Ziel der XGBoost-Bibliothek besteht darin, die Rechenleistung von Computern erheblich zu verbessern, um Struktur-Boosting-Vorgänge in großem Umfang zu ermöglichen, die skalierbar, portabel und präzise sind.

XGBoost wird als Befehlszeile und als R-Bibliothek bereitgestellt.

Wenn Sie die XGBoost-Bibliothek in R verwenden möchten, starten Sie eine interaktive R-Sitzung (geben Sie in der Shell **R** ein), und laden Sie anschließend die Bibliothek.

Dies ist ein einfaches Beispiel, das Sie an der Eingabeaufforderung von R ausführen können:

```R
library(xgboost)

data(agaricus.train, package='xgboost')
data(agaricus.test, package='xgboost')
train <- agaricus.train
test <- agaricus.test
bst <- xgboost(data = train$data, label = train$label, max.depth = 2,
                eta = 1, nthread = 2, nround = 2, objective = "binary:logistic")
pred <- predict(bst, test$data)
```

Nutzen Sie die folgenden Befehle in der Shell, um die XGBoost-Befehlszeile in der Shell auszuführen:

```bash
cp -r /dsvm/tools/xgboost/demo/binary_classification/ xgboostdemo
cd xgboostdemo
xgboost mushroom.conf
```

Eine MODEL-Datei wird in das angegebene Verzeichnis geschrieben. Informationen zu diesem Demobeispiel auf GitHub finden Sie unter [Binärklassifizierung](https://github.com/dmlc/xgboost/tree/master/demo/binary_classification).

Weitere Informationen zu XGBoost finden Sie in der [XGBoost-Dokumentation](https://xgboost.readthedocs.org/en/latest/) und im [XGBoost-GitHub-Repository](https://github.com/dmlc/xgboost).

### <a name="rattle"></a>Rattle

Rattle (*R* *A*nalytical *T*ool *T*o *L*earn *E*asily) verwendet GUI-basierte Durchsuchungs- und Modellierungsvorgänge für Daten. Rattle:
- Zeigt statistische und visuelle Zusammenfassungen von Daten an.
- Transformiert Daten, die direkt modelliert werden können.
- Erstellt sowohl nicht überwachte als auch überwachte Modelle aus Daten.
- Zeigt die Leistung von Modellen grafisch an.
- Bewertet neue Datasets.
- Generiert R-Code.
- Repliziert Vorgänge in der UI, die direkt in R ausgeführt oder als Ausgangspunkt für weitere Analysen verwendet werden können.

Damit Sie Rattle ausführen können, müssen Sie bei einer grafischen Desktopsitzung angemeldet sein. Geben Sie in einem Terminal **R** ein, um die R-Umgebung zu öffnen. Geben Sie an der R-Eingabeaufforderung die folgenden Befehle ein:

```R
library(rattle)
rattle()
```

Eine grafische Benutzeroberfläche mit einer Reihe von Registerkarten wird geöffnet. Verwenden Sie die folgenden Schnellstartschritte in Rattle, um ein Beispiel-Wetterdataset zu verwenden und ein Modell zu erstellen. In einigen Schritten werden Sie zum automatischen Installieren und Laden einiger erforderlicher R-Pakete aufgefordert, die sich nicht bereits im System befinden.

> [!NOTE]
> Falls Sie über keine Berechtigungen für die Installation des Pakets im Systemverzeichnis (Standardeinstellung) haben, wird unter Umständen in Ihrem R-Konsolenfenster eine Aufforderung mit der Frage angezeigt, ob Pakete in Ihrer persönlichen Bibliothek installiert werden sollen. Geben Sie **y** ein, wenn diese Aufforderungen angezeigt werden.

1. Wählen Sie **Execute**(Ausführen).
1. In einem Dialogfeld werden Sie aufgefordert, das Beispielwetterdataset zu laden. Wählen Sie **Yes** (Ja) aus, um das Beispiel zu laden.
1. Wählen Sie die Registerkarte **Model** (Modell) aus.
1. Wählen Sie **Execute** (Ausführen) aus, um eine Entscheidungsstruktur zu erstellen.
1. Wählen Sie **Draw** (Zeichnen) aus, um die Entscheidungsstruktur anzuzeigen.
1. Wählen Sie die Option **Forest** (Gesamtstruktur) und anschließend **Execute** (Ausführen) aus, um eine zufällige Gesamtstruktur zu erstellen.
1. Wählen Sie die Registerkarte **Evaluate** (Auswerten) aus.
1. Wählen Sie die Option **Risk** (Risiko) und anschließend **Execute** (Ausführen) aus, um zwei Leistungsdarstellungen zu **Risk (Cumulative)** (Risiko (Kumulativ)) anzuzeigen.
1. Wählen Sie die Registerkarte **Log** (Protokoll) aus, um den generierten R-Code für die obigen Vorgänge anzuzeigen. (Aufgrund eines Fehlers in der aktuellen Version von Rattle müssen Sie im Text des Protokolls vor **Export this log** (Dieses Protokoll exportieren) das Zeichen **#** eingeben.)
1. Wählen Sie die Schaltfläche **Export** (Exportieren) aus, um die R-Skriptdatei *weather_script.R* im Basisordner zu speichern.

Sie können Rattle und R beenden. Nun können Sie das generierte R-Skript bearbeiten. Sie können das Skript auch unverändert verwenden und es jederzeit ausführen, um alle Aktionen auf der Rattle-Benutzeroberfläche zu wiederholen. Dies ist besonders für Einsteiger in R eine Möglichkeit, auf einer einfachen grafischen Benutzeroberfläche schnell Analysen und maschinelles Lernen durchzuführen, während in R automatisch Code zum Ändern bzw. Lernen generiert wird.

## <a name="next-steps"></a>Nächste Schritte

Haben Sie weitere Fragen? Erstellen Sie ggf. ein [Supportticket](https://azure.microsoft.com/support/create-ticket/).