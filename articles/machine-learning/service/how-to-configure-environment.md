---
title: Einrichten einer Python-Entwicklungsumgebung
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie Ihre Entwicklungsumgebung für Azure Machine Learning konfigurieren. Verwenden Sie Conda-Umgebungen, erstellen Sie Konfigurationsdateien und konfigurieren Sie Ihren eigenen cloudbasierten Notebook-Server sowie Ihre eigenen Jupyter Notebooks, Azure Databricks, IDEs und Code-Editoren und die Data Science Virtual Machine.
services: machine-learning
author: rastala
ms.author: roastala
ms.service: machine-learning
ms.subservice: core
ms.reviewer: larryfr
ms.topic: conceptual
ms.date: 10/25/2019
ms.custom: seodec18
ms.openlocfilehash: 19045b54b97fdb69f9fdab3d17066faa5dbcc435
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73580720"
---
# <a name="configure-a-development-environment-for-azure-machine-learning"></a>Konfigurieren einer Entwicklungsumgebung für Azure Machine Learning
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

In diesem Artikel erfahren Sie, wie Sie eine Entwicklungsumgebung für die Arbeit mit Azure Machine Learning konfigurieren. Azure Machine Learning ist plattformunabhängig. Die einzige zwingende Anforderung an die Entwicklungsumgebung ist Python 3. Außerdem wird eine isolierte Umgebung wie Anaconda oder Virtualenv empfohlen.

In der folgenden Tabelle sind die einzelnen in diesem Artikel behandelten Entwicklungsumgebungen zusammen mit den jeweiligen Vor- und Nachteilen aufgeführt.

| Environment | Vorteile | Nachteile |
| --- | --- | --- |
| [Cloudbasierte Azure Machine Learning-Notebook-VM](#notebookvm) | Einfachste Methode für den Einstieg. Das gesamte SDK ist bereits auf der Arbeitsbereichs-VM installiert. Die Notebooktutorials sind vorab geklont und bereit zur Ausführung. | Keine Kontrolle über Ihre Entwicklungsumgebung und die Abhängigkeiten. Zusätzliche Kosten für den virtuellen Linux-Computer (virtueller Computer kann bei Nichtverwendung beendet werden, um Gebühren zu vermeiden). Preisdetails finden Sie [hier](https://azure.microsoft.com/pricing/details/virtual-machines/linux/). |
| [Lokale Umgebung](#local) | Vollständige Kontrolle über Ihre Entwicklungsumgebung und die Abhängigkeiten. Ausführung mit einem Buildtool, einer Umgebung oder einer IDE Ihrer Wahl. | Der Einstieg dauert länger. Die erforderlichen SDK-Pakete müssen installiert werden. Außerdem muss eine Umgebung installiert werden, wenn noch keine vorhanden ist. |
| [Azure Databricks](#aml-databricks) | Ideal für die Ausführung umfangreicher Workflows mit maschinellem Lernen auf der skalierbaren Apache Spark-Plattform. | Übermaß für experimentelles maschinelles Lernen oder kleinere Experimente und Workflows. Zusätzliche Kosten für Azure Databricks. Preisdetails finden Sie [hier](https://azure.microsoft.com/pricing/details/databricks/). |
| [Data Science Virtual Machine (DSVM)](#dsvm) | Vergleichbar mit der cloudbasierten Notebook-VM (Python und das SDK sind vorinstalliert), aber mit zusätzlichen gängigen vorinstallierten Data Science- und Machine Learning-Tools. Einfache Skalierung und Kombination mit anderen benutzerdefinierten Tools und Workflows. | Langsamerer Einstieg im Vergleich zur cloudbasierten Notebook-VM |


Dieser Artikel enthält außerdem zusätzliche Anwendungstipps für die folgenden Tools:

* [Jupyter Notebooks](#jupyter): Wenn Sie bereits ein Jupyter Notebook verwenden, enthält das SDK einige zusätzliche Elemente, die Sie installieren sollten.

* [Visual Studio Code](#vscode): Wenn Sie Visual Studio Code verwenden, enthält die [Azure Machine Learning-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai) eine umfassende Sprachunterstützung für Python sowie Features, mit denen die Arbeit mit Azure Machine Learning wesentlich bequemer und produktiver wird.

## <a name="prerequisites"></a>Voraussetzungen

Ein Azure Machine Learning-Arbeitsbereich. Informationen zum Erstellen des Arbeitsbereichs finden Sie unter [Erstellen eines Azure Machine Learning-Arbeitsbereichs](how-to-manage-workspace.md). Ein Arbeitsbereich ist alles, was Sie zum Einstieg in Ihren eigenen [cloudbasierten Notebook-Server](#notebookvm), eine [DSVM](#dsvm) oder [Azure Databricks](#aml-databricks) benötigen.

Um die SDK-Umgebung für Ihren [lokalen Computer](#local), Ihren [Jupyter Notebook-Server](#jupyter) oder Ihr [Visual Studio Code](#vscode) zu installieren, benötigen Sie außerdem:

- Den Paket-Manger [Anaconda](https://www.anaconda.com/download/) oder [Miniconda](https://conda.io/miniconda.html).

- Unter Linux oder macOS benötigen Sie die Bash-Shell.

    > [!TIP]
    > Wenn Sie unter Linux oder macOS eine andere Shell als Bash verwenden (z.B. zsh), werden beim Ausführen einiger Befehle möglicherweise Fehler ausgegeben. Um dieses Problem zu umgehen, starten Sie mit dem Befehl `bash` eine neue Bash-Shell starten, und führen Sie die Befehle in dieser aus.

- Unter Windows benötigen Sie die Eingabeaufforderung oder die Anaconda-Eingabeaufforderung (installiert mit Anaconda und Miniconda).

## <a id="notebookvm"></a>Ihre eigene cloudbasierte Notebook-VM

Die Azure Machine Learning-Notebook-VM ist eine sichere, cloudbasierte Azure-Arbeitsstation, die einen Jupyter Notebook-Server, JupyterLab und eine vollständig vorbereitete Machine Learning-Umgebung für Datenanalysten bereitstellt.

Die Notebook-VM ist:

Für eine Compute-Instanz muss nichts installiert oder konfiguriert werden.  In Ihrem Azure Machine Learning-Arbeitsbereich können Sie jederzeit eine Notebook-VM erstellen. Geben Sie einfach einen Namen und einen Azure-VM-Typ an. Sie die Einrichtung unter [Tutorial: Einrichten der Umgebung und des Arbeitsbereichs](tutorial-1st-experiment-sdk-setup.md).


[Beenden Sie die Notebook-VM](tutorial-1st-experiment-sdk-train.md#clean-up-resources), um zu verhindern, dass weitere Computegebühren anfallen.

## <a id="dsvm"></a>Data Science Virtual Machine

Die DSVM ist ein benutzerdefiniertes VM-Image. Sie ist für Data Science-Aufgaben konzipiert, die mit Folgendem vorab konfiguriert wurden:

  - Pakete wie TensorFlow, PyTorch, Scikit-learn, XGBoost und das Azure Machine Learning SDK.
  - Beliebte Data Science-Tools wie Spark (eigenständig) und Drill.
  - Azure-Tools wie die Azure CLI, AzCopy und Storage-Explorer.
  - Integrierte Entwicklungsumgebungen (IDEs), z.B. Visual Studio Code und PyCharm.
  - Jupyter Notebook-Server

Das Azure Machine Learning SDK funktioniert mit der Ubuntu- und der Windows-Version der DSVM. Wenn Sie die DSVM jedoch als Computeziel verwenden möchten, wird nur Ubuntu unterstützt.

So verwenden Sie die DSVM als Entwicklungsumgebung

1. Erstellen Sie in einer der folgenden Umgebungen eine DSVM:

    * Azure-Portal:

        * [Erstellen einer Ubuntu-Data Science Virtual Machine](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro)

        * [Erstellen einer Windows-Data Science Virtual Machine](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/provision-vm)

    * Azure CLI:

        > [!IMPORTANT]
        > * Wenn Sie die Azure CLI verwenden, müssen Sie sich zuerst mit dem Befehl `az login` bei Ihrem Azure-Abonnement anmelden.
        >
        > * Wenn Sie die Befehle in diesem Schritt ausführen, müssen Sie einen Ressourcengruppennamen, einen Namen für die VM, einen Benutzernamen und ein Kennwort angeben.

        * Verwenden Sie zum Erstellen einer Data Science Virtual Machine-Instanz unter Ubuntu den folgenden Befehl:

            ```azurecli
            # create a Ubuntu DSVM in your resource group
            # note you need to be at least a contributor to the resource group in order to execute this command successfully
            # If you need to create a new resource group use: "az group create --name YOUR-RESOURCE-GROUP-NAME --location YOUR-REGION (For example: westus2)"
            az vm create --resource-group YOUR-RESOURCE-GROUP-NAME --name YOUR-VM-NAME --image microsoft-dsvm:linux-data-science-vm-ubuntu:linuxdsvmubuntu:latest --admin-username YOUR-USERNAME --admin-password YOUR-PASSWORD --generate-ssh-keys --authentication-type password
            ```

        * Verwenden Sie zum Erstellen einer Data Science Virtual Machine-Instanz unter Windows den folgenden Befehl:

            ```azurecli
            # create a Windows Server 2016 DSVM in your resource group
            # note you need to be at least a contributor to the resource group in order to execute this command successfully
            az vm create --resource-group YOUR-RESOURCE-GROUP-NAME --name YOUR-VM-NAME --image microsoft-dsvm:dsvm-windows:server-2016:latest --admin-username YOUR-USERNAME --admin-password YOUR-PASSWORD --authentication-type password
            ```

2. Das Azure Machine Learning SDK ist auf der DSVM bereits installiert. Um die Conda-Umgebung mit dem SDK zu verwenden, führen Sie einen der folgenden Befehle aus:

    * Für die DSVM unter Ubuntu:

        ```shell
        conda activate py36
        ```

    * Für die DSVM unter Windows:

        ```shell
        conda activate AzureML
        ```

1. Um sicherzustellen, dass Sie auf das SDK zugreifen und die Version überprüfen können, verwenden Sie den folgenden Python-Code:

    ```python
    import azureml.core
    print(azureml.core.VERSION)
    ```

1. Informationen zum Konfigurieren der DSVM für Ihren Azure Machine Learning-Arbeitsbereich finden Sie im Abschnitt [Erstellen einer Konfigurationsdatei für den Arbeitsbereich](#workspace).

Weitere Informationen finden Sie unter [Data Science Virtual Machines](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/).

## <a id="local"></a>Lokaler Computer

Wenn Sie einen lokalen Computer verwenden (dabei kann es sich auch um einen virtuellen Remotecomputer handeln), erstellen Sie eine Anaconda-Umgebung, und installieren Sie das SDK. Hier sehen Sie ein Beispiel:

1. Laden Sie [Anaconda](https://www.anaconda.com/distribution/#download-section) (Python Version 3.7) herunter, und installieren Sie es, falls dies noch nicht geschehen ist.

1. Öffnen Sie eine Anaconda-Eingabeaufforderung, und erstellen Sie mit den folgenden Befehlen eine Umgebung:

    Führen Sie den folgenden Befehl aus, um die Umgebung zu erstellen.

    ```shell
    conda create -n myenv python=3.6.5
    ```

    Aktivieren Sie anschließend die Umgebung.

    ```shell
    conda activate myenv
    ```

    In diesem Beispiel wird eine Umgebung mit Python 3.6.5 erstellt. Es kann jedoch auch eine andere Nebenversion verwendet werden. Bei bestimmten Hauptversionen kann die SDK-Kompatibilität nicht garantiert werden. Empfohlen wird daher die Version 3.5 oder höher. Wenn Fehler auftreten, sollten Sie außerdem eine andere Version oder Nebenversion in Ihrer Anaconda-Umgebung testen. Es dauert einige Minuten, bis die Komponenten und Pakete heruntergeladen wurden und die Umgebung erstellt wurde.

1. Führen Sie die folgenden Befehle in der neuen Umgebung aus, um umgebungsspezifische IPython-Kernel zu aktivieren. Dadurch wird das erwartete Kernel- und Paketimportverhalten beim Arbeiten mit Jupyter-Notebooks in Anaconda-Umgebungen sichergestellt:

    ```shell
    conda install notebook ipykernel
    ```

    Führen Sie nun den folgenden Befehl zum Erstellen des Kernels aus:

    ```shell
    ipython kernel install --user
    ```

1. Verwenden Sie die folgenden Befehle, um die Pakete zu installieren:

    Durch diesen Befehl wird das grundlegende Azure Machine Learning SDK mit Notebooks und zusätzlichen `automl`-Komponenten installiert. Da das `automl`-Zusatzpaket sehr umfangreich ist, müssen Sie dieses nicht innerhalb der Klammern angeben und installieren, wenn Sie keine automatisierten Machine Learning-Experimente durchführen möchten. Das `automl`-Zusatzpaket enthält standardmäßig auch das Azure Machine Learning Data Prep SDK als Abhängigkeit.

    ```shell
    pip install azureml-sdk[notebooks,automl]
    ```

   > [!NOTE]
   > * Wenn Sie eine Meldung mit dem Hinweis erhalten, dass PyYAML nicht deinstalliert werden kann, verwenden Sie stattdessen den folgenden Befehl:
   >
   >   `pip install --upgrade azureml-sdk[notebooks,automl] --ignore-installed PyYAML`
   >
   > * Ab macOS Catalina ist zsh (Z-Shell) der Standard für die Anmeldungsshell und die interaktive Shell. Verwenden Sie in zsh den folgenden Befehl, wobei eckige Klammern mit dem Escapezeichen „\\“ (umgekehrter Schrägstrich) versehen werden:
   >
   >   `pip install --upgrade azureml-sdk\[notebooks,automl\]`


   Die Installation des SDK nimmt mehrere Minuten in Anspruch. Weitere Informationen zu den Installationsoptionen finden Sie in der [Installationsanleitung](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).

1. Installieren Sie weitere Pakete für Ihre Machine Learning-Experimente.

    Verwenden Sie einen der folgenden Befehle, und ersetzen Sie *\<new package>* durch das zu installierende Paket: Wenn Sie mit `conda install` Pakete installieren, müssen diese Teil der aktuellen Kanäle sein. Neue Kanäle können in Anaconda Cloud hinzugefügt werden.

    ```shell
    conda install <new package>
    ```

    Alternativ können Sie Pakete mit `pip` installieren.

    ```shell
    pip install <new package>
    ```

### <a id="jupyter"></a>Jupyter Notebooks

Jupyter Notebooks sind Teil des [Jupyter-Projekts](https://jupyter.org/). Sie stellen eine interaktive Programmierumgebung bereit, mit der Sie Dokumente erstellen können, in denen Livecode mit beschreibendem Text und Grafiken gemischt ist. Jupyter Notebooks sind auch eine hervorragende Möglichkeit, Ihre Ergebnisse mit anderen Personen zu teilen, da Sie die Ausgabe Ihrer Codeabschnitte im Dokument speichern können. Sie können Jupyter Notebooks auf vielen unterschiedlichen Plattformen installieren.

Durch die Vorgehensweise im Abschnitt [Lokaler Computer](#local) werden die erforderlichen Komponenten zum Ausführen von Jupyter-Notebooks in einer Anaconda-Umgebung installiert.

So aktivieren Sie diese Komponenten in Ihrer Jupyter Notebook-Umgebung

1. Öffnen Sie eine Anaconda-Eingabeaufforderung, und aktivieren Sie Ihre Umgebung.

    ```shell
    conda activate myenv
    ```

1. Klonen Sie [das GitHub-Repository](https://aka.ms/aml-notebooks) für eine Reihe von Beispielnotebooks.

    ```CLI
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Starten Sie den Jupyter Notebook-Server mit dem folgenden Befehl:

    ```shell
    jupyter notebook
    ```

1. Erstellen Sie mit **New** (Neu) ein neues Notebook, wählen Sie **Python 3** als Kernel aus, und führen Sie dann folgenden Befehl in einer Notebookzelle aus, um zu überprüfen, ob Jupyter Notebook das SDK verwenden kann:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

1. Wenn beim Importieren von Modulen Probleme auftreten und `ModuleNotFoundError` angezeigt wird, müssen Sie sicherstellen, dass für den Jupyter-Kernel der richtige Umgebungspfad angegeben ist. Führen Sie dazu folgenden Code in einer Notebookzelle aus:

    ```python
    import sys
    sys.path
    ```

1. Informationen zum Konfigurieren des Jupyter Notebooks für Ihren Azure Machine Learning-Arbeitsbereich finden Sie im Abschnitt [Erstellen einer Konfigurationsdatei für den Arbeitsbereich](#workspace).


### <a id="vscode"></a>Visual Studio Code

Visual Studio Code ist ein sehr beliebter plattformübergreifender Code-Editor, der einen umfangreichen Satz von Programmiersprachen und Tools über Erweiterungen unterstützt, die im [Visual Studio Marketplace](https://marketplace.visualstudio.com/vscode) verfügbar sind. Mit der [Azure Machine Learning-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai) wird die [Python-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-python.python) für das Programmieren in allen Arten von Python-Umgebungen (virtuell, Anaconda usw.) installiert. Sie bietet außerdem praktische Features für die Arbeit mit Azure Machine Learning-Ressourcen und das Ausführen von Azure Machine Learning-Experimenten, ohne Visual Studio Code verlassen zu müssen.

So verwenden Visual Studio Code für die Entwicklung

1. Installieren Sie die Azure Machine Learning-Erweiterung für Visual Studio Code wie unter [Azure Machine Learning](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai) beschrieben.

    Weitere Informationen finden Sie unter [Verwenden von Azure Machine Learning für Visual Studio Code](how-to-vscode-tools.md).

1. Unter [Erste Schritte mit Python in VS Code](https://code.visualstudio.com/docs/python/python-tutorial) erfahren Sie, wie Sie Visual Studio Code für die Python-Entwicklung verwenden.

    - Um die Python-Umgebung mit dem SDK auszuwählen, öffnen Sie VS Code, und drücken Sie STRG+UMSCHALT+P (Linux und Windows) bzw. BEFEHL+UMSCHALT+P (Mac).
        - Die __Befehlspalette__ wird geöffnet.

    - Geben Sie __Python: Select Interpreter__ ein, und wählen Sie dann die gewünschte Umgebung aus.

1. Um sicherzustellen dass Sie das SDK verwenden können, erstellen Sie eine neue Python-Datei (.py) mit dem folgenden Code:

    ```python
    #%%
    import azureml.core
    azureml.core.VERSION
    ```
    Führen Sie diesen Code aus, indem Sie auf die CodeLens „Run cell“ (Zelle ausführen) klicken oder einfach UMSCHALT+EINGABETASTE drücken.
<a name="aml-databricks"></a>

## <a name="azure-databricks"></a>Azure Databricks
Azure Databricks ist eine Apache Spark-basierte Umgebung in der Azure-Cloud. Sie stellt eine Notebook-basierte Umgebung mit CPU- oder GPU-basierten Computeclustern für die Zusammenarbeit bereit.

Azure Databricks kann auf folgende Weisen mit Azure Machine Learning verwendet werden:
+ Sie können mit Spark MLlib ein Modell trainieren und es über Azure Databricks in ACI/AKS bereitstellen.
+ Sie können mithilfe eines speziellen Azure ML SDK außerdem Features für [automatisiertes Machine Learning](concept-automated-ml.md) mit Azure Databricks verwenden.
+ Sie können Azure Databricks über eine [Azure Machine Learning-Pipeline](concept-ml-pipelines.md) als Computeziel verwenden.

### <a name="set-up-your-databricks-cluster"></a>Einrichten des Databricks-Clusters

Erstellen Sie einen [Databricks-Cluster](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal). Einige Einstellungen sind nur erforderlich, wenn Sie das SDK für automatisiertes Machine Learning in Databricks verwenden.
**Die Erstellung des Clusters dauert einige Minuten.**

Verwenden Sie die folgenden Einstellungen:

| Einstellung |Anwendungsbereich| Wert |
|----|---|---|
| Clustername |immer| IhrClustername |
| Databricks-Laufzeit |immer|Non-ML Runtime 6.0 (Scala 2.11, Spark 2.4.3) |
| Python-Version |immer| 3 |
| Worker |immer| 2 oder mehr |
| VM-Typen des Workerknotens <br>(bestimmt die maximale Anzahl gleichzeitiger Iterationen) |Automatisiertes maschinelles Lernen<br>Machine Learning| Arbeitsspeicheroptimierte VM bevorzugt |
| Automatische Skalierung aktivieren |Automatisiertes maschinelles Lernen<br>Machine Learning| Deaktivieren |

Warten Sie, bis der Cluster ausgeführt wird, bevor Sie fortfahren.

### <a name="install-the-correct-sdk-into-a-databricks-library"></a>Installieren des richtigen SDK in einer Databricks-Bibliothek
Erstellen Sie nach der Ausführung des Clusters [eine Bibliothek](https://docs.databricks.com/user-guide/libraries.html#create-a-library), um das entsprechende Azure Machine Learning SDK-Paket Ihrem Cluster anzufügen.

1. Klicken Sie mit der rechten Maustaste auf den aktuellen Arbeitsbereichsordner, in dem Sie die Bibliothek speichern möchten. Wählen Sie **Bibliothek** > **erstellen** aus.

1. Wählen Sie **nur eine** Option aus (weitere SDK-Installationen werden nicht unterstützt).

   |Zusatzkomponenten für &nbsp;SDK-Paket&nbsp;|`Source`|&nbsp;PyPi-Name&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|
   |----|---|---|
   |Für Databricks| Python Egg oder PyPI hochladen | azureml-sdk[databricks]|
   |Für Databricks mit<br> Features für automatisiertes Machine Learning| Python Egg oder PyPI hochladen | azureml-sdk[automl]|

   > [!Warning]
   > Sie können keine weiteren SDK-Zusatzkomponenten installieren. Wählen Sie nur eine der zuvor beschriebenen Optionen [databricks] oder [automl] aus.

   * Wählen Sie nicht **Attach automatically to all clusters** (Automatisch an alle Cluster anfügen) aus.
   * Wählen Sie **Anfügen** neben dem Namen Ihres Clusters aus.

1. Der Status wird in **Angefügt** geändert. Dieser Vorgang kann einige Minuten in Anspruch nehmen. Überprüfen Sie währenddessen, ob Fehler auftreten.  Wenn bei diesem Schritt ein Fehler auftritt:

   Versuchen Sie, Ihren Cluster wie folgt neu zu starten:
   1. Wählen Sie im linken Bereich die Option **Cluster** aus.
   1. Wählen Sie in der Tabelle den Namen Ihres Clusters aus.
   1. Klicken Sie auf der Registerkarte **Bibliotheken** auf **Neu starten**.

   Berücksichtigen Sie außerdem Folgendes:
   + Fügen Sie in der AutoML-Konfiguration bei Verwendung von Azure Databricks die folgenden Parameter hinzu:
       1. ```max_concurrent_iterations``` basiert auf der Anzahl der Workerknoten in Ihrem Cluster.
        2. ```spark_context=sc``` basiert auf dem standardmäßigen Spark-Kontext.
   + Wenn Sie alternativ dazu eine alte SDK-Version nutzen, deaktivieren Sie diese in den installierten Bibliotheken des Clusters, und verschieben Sie sie in den Papierkorb. Installieren Sie die neue SDK-Version, und starten Sie den Cluster neu. Wenn nach dem Neustart ein Problem vorliegt, trennen Sie Ihren Cluster, und fügen Sie ihn wieder an.

Wenn die Installation erfolgreich war, sollte die importierte Bibliothek wie auf einem der folgenden Screenshots gezeigt aussehen:

SDK für Databricks **_ohne_** ![Azure Machine Learning SDK für Databricks](./media/how-to-configure-environment/amlsdk-withoutautoml.jpg) und automatisiertes Machine Learning

SDK für Databricks **mit** ![installiertem SDK für automatisiertes Machine Learning in Databricks](./media/how-to-configure-environment/automlonadb.png)

### <a name="start-exploring"></a>Ausprobieren

So können Sie Azure Databricks testen:
+ Von den vielen verfügbaren Beispielnotebooks können **nur [ganz bestimmte](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/azure-databricks) mit Azure Databricks verwendet werden.**

+ Importieren Sie diese Beispiele direkt aus Ihrem Arbeitsbereich. Siehe unten: ![„Importieren“ auswählen](media/how-to-configure-environment/azure-db-screenshot.png)
![Bereich für Importieren](media/how-to-configure-environment/azure-db-import.png)

+ Erfahren Sie, wie Sie [mit Databricks als Computeziel für das Trainieren von Modellen eine Pipeline erstellen](how-to-create-your-first-pipeline.md).

## <a id="workspace"></a>Erstellen einer Konfigurationsdatei für den Arbeitsbereich

Die Arbeitsbereichs-Konfigurationsdatei ist eine JSON-Datei, die das SDK zur Kommunikation mit Ihrem Azure Machine Learning-Arbeitsbereich instruiert. Die Datei weist den Namen *config.json* und das folgende Format auf:

```json
{
    "subscription_id": "<subscription-id>",
    "resource_group": "<resource-group>",
    "workspace_name": "<workspace-name>"
}
```

Diese JSON-Datei muss sich in der Verzeichnisstruktur befinden, die Ihre Python-Skripts oder Jupyter Notebooks enthält. Sie kann sich im selben Verzeichnis, in einem Unterverzeichnis namens *.azureml* oder in einem übergeordneten Verzeichnis befinden.

Um in Ihrem Code auf diese Datei zuzugreifen, verwenden Sie `ws=Workspace.from_config()`. Dieser Code lädt die Informationen aus der Datei und stellt eine Verbindung mit Ihrem Arbeitsbereich her.

Sie können die Konfigurationsdatei auf drei Arten erstellen:

* **Verwenden Sie [ws.write_config](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)** zum Schreiben einer Datei vom Typ *config.json*. Diese Datei enthält die Konfigurationsinformationen für Ihren Arbeitsbereich. Sie können die Datei *config.json* in andere Entwicklungsumgebungen herunterladen oder kopieren.

* **Datei herunterladen**: Wählen Sie im [Azure-Portal](https://ms.portal.azure.com) im Abschnitt **Übersicht** Ihres Arbeitsbereichs **config.json herunterladen** aus.

     ![Azure-Portal](./media/how-to-configure-environment/configure.png)

* **Erstellen Sie die Datei programmgesteuert**: Im folgenden Codeausschnitt stellen Sie eine Verbindung mit einem Arbeitsbereich her, indem Sie die Abonnement-ID, die Ressourcengruppe und den Arbeitsbereichsnamen angeben. Anschließend speichert der Code die Arbeitsbereichskonfiguration in dieser Datei:

    ```python
    from azureml.core import Workspace

    subscription_id = '<subscription-id>'
    resource_group  = '<resource-group>'
    workspace_name  = '<workspace-name>'

    try:
        ws = Workspace(subscription_id = subscription_id, resource_group = resource_group, workspace_name = workspace_name)
        ws.write_config()
        print('Library configuration succeeded')
    except:
        print('Workspace not found')
    ```

    Dieser Code schreibt die Konfigurationsdatei in die Datei *.azureml/config.json*.


## <a name="next-steps"></a>Nächste Schritte

- [Trainieren eines Modells](tutorial-train-models-with-aml.md) in Azure Machine Learning mit der MNIST-Datenbank
- Anzeigen der Referenz [Azure Machine Learning SDK für Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)
