---
title: Lokales Installieren von Jupyter und Herstellen einer Verbindung mit Spark in Azure HDInsight
description: Erfahren Sie, wie Sie Jupyter Notebook auf Ihrem Computer installieren und eine Verbindung mit einem Apache Spark-Cluster herstellen.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/06/2019
ms.openlocfilehash: 46164cfc0c2baff919808a831a67180b65a23ff7
ms.sourcegitcommit: e1b6a40a9c9341b33df384aa607ae359e4ab0f53
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71337651"
---
# <a name="install-jupyter-notebook-on-your-computer-and-connect-to-apache-spark-on-hdinsight"></a>Installieren von Jupyter Notebook auf Ihrem Computer und Herstellen einer Verbindung mit Apache Spark in HDInsight

In diesem Artikel erfahren Sie, wie Sie Jupyter Notebook mit den benutzerdefinierten Kerneln PySpark (für Python) und Apache Spark (für Scala) mit Spark Magic installieren und das Notebook mit einem HDInsight-Cluster verbinden. Es kann eine Reihe von Gründen geben, Jupyter auf dem lokalen Computer zu installieren, und es auch können auch einige Probleme auftreten. Weitere Informationen hierzu finden Sie im Abschnitt [Warum sollte ich Jupyter auf meinem Computer installieren?](#why-should-i-install-jupyter-on-my-computer) am Ende dieses Artikels.

Es gibt vier wichtige Schritte zum Installieren von Jupyter und Herstellen einer Verbindung mit Apache Spark in HDInsight.

* Konfigurieren Sie den Spark-Cluster.
* Installieren Sie Jupyter Notebook.
* Installieren Sie die PySpark- und Spark-Kernel mit Spark Magic.
* Konfigurieren Sie Spark Magic für den Zugriff auf den Spark-Cluster in HDInsight.

Weitere Informationen zu den benutzerdefinierten Kerneln und Spark Magic für Jupyter-Notebooks mit HDInsight-Clustern finden Sie unter [Verfügbare Kernels für Jupyter-Notebooks mit Apache Spark-Linux-Clustern in HDInsight](apache-spark-jupyter-notebook-kernels.md).

## <a name="prerequisites"></a>Voraussetzungen

Die hier aufgeführten Voraussetzungen gelten nicht für die Installation von Jupyter. Sie gelten für die Herstellung einer Verbindung von einem Jupyter Notebook zu einem HDInsight-Cluster, sobald das Notebook installiert wurde.

* Ein Apache Spark-Cluster unter HDInsight. Eine Anleitung finden Sie unter [Erstellen von Apache Spark-Clustern in Azure HDInsight](apache-spark-jupyter-spark-sql.md).

## <a name="install-jupyter-notebook-on-your-computer"></a>Installieren von Jupyter Notebook auf Ihrem Computer

Sie müssen Python installieren, bevor Sie Jupyter Notebooks installieren können. Die [Anaconda-Distribution](https://www.anaconda.com/download/) installiert sowohl Python als auch Jupyter Notebook.

Laden Sie das [Anaconda-Installationsprogramm](https://www.anaconda.com/download/) für Ihre Plattform herunter, und führen Sie das Setup aus. Stellen Sie sicher, dass während der Ausführung des Setup-Assistenten die Option für das Hinzufügen von Anaconda zu Ihrer PATH-Variablen aktiviert ist.  Siehe auch [Installieren von Jupyter mit Anaconda](https://jupyter.readthedocs.io/en/latest/install.html).

## <a name="install-spark-magic"></a>Installieren von SparkMagic

1. Geben Sie einen der folgenden Befehle ein, um SparkMagic zu installieren. Weitere Informationen finden Sie in der [SparkMagic-Dokumentation](https://github.com/jupyter-incubator/sparkmagic#installation).

    |Clusterversion | Installationsbefehl |
    |---|---|
    |Version 3.6 und 3.5 |`pip install sparkmagic==0.12.7`|
    |Version 3.4|`pip install sparkmagic==0.2.3`|

1. Führen Sie den folgenden Befehl aus, um sicherzustellen, dass `ipywidgets` ordnungsgemäß installiert ist:

    ```cmd
    jupyter nbextension enable --py --sys-prefix widgetsnbextension
    ```

## <a name="install-pyspark-and-spark-kernels"></a>Installieren der PySpark- und Spark-Kernel

1. Ermitteln Sie mithilfe des folgenden Befehls, wo `sparkmagic` installiert ist:

    ```cmd
    pip show sparkmagic
    ```

    Ändern Sie dann Ihr Arbeitsverzeichnis in den Speicherort, den Sie mit dem obigen Befehl ermittelt haben.

1. Geben Sie im neuen Arbeitsverzeichnis einen oder mehrere der unten aufgeführten Befehle ein, um die gewünschten Kernel zu installieren:

    |Kernel | Get-Help |
    |---|---|
    |Spark|`jupyter-kernelspec install sparkmagic/kernels/sparkkernel`|
    |SparkR|`jupyter-kernelspec install sparkmagic/kernels/sparkrkernel`|
    |PySpark|`jupyter-kernelspec install sparkmagic/kernels/pysparkkernel`|
    |PySpark3|`jupyter-kernelspec install sparkmagic/kernels/pyspark3kernel`|

1. Optional. Geben Sie den folgenden Befehl ein, um die Servererweiterung zu aktivieren:

    ```cmd
    jupyter serverextension enable --py sparkmagic
    ```

## <a name="configure-spark-magic-to-connect-to-hdinsight-spark-cluster"></a>Konfigurieren von Spark Magic zum Herstellen der Verbindung mit dem Spark-Cluster in HDInsight

In diesem Abschnitt lernen Sie, SparkMagic nach der Installation zu konfigurieren, um eine Verbindung mit einem Apache Spark-Cluster herzustellen.

1. Starten Sie die Python-Shell mit dem folgenden Befehl:

    ```cmd
    python
    ```

2. Die Konfigurationsinformationen für Jupyter werden in der Regel im Basisverzeichnis des Benutzers gespeichert. Geben Sie den folgenden Befehl ein, um das Basisverzeichnis zu ermitteln, und erstellen Sie den Ordner **.sparkmagic**.  Der vollständige Pfad wird ausgegeben.

    ```python
    import os
    path = os.path.expanduser('~') + "\\.sparkmagic"
    os.makedirs(path)
    print(path)
    exit()
    ```

3. Erstellen Sie im Ordner `.sparkmagic` die Datei **config.json**, und fügen Sie den folgenden JSON-Codeausschnitt darin ein.  

    ```json
    {
      "kernel_python_credentials" : {
        "username": "{USERNAME}",
        "base64_password": "{BASE64ENCODEDPASSWORD}",
        "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
      },

      "kernel_scala_credentials" : {
        "username": "{USERNAME}",
        "base64_password": "{BASE64ENCODEDPASSWORD}",
        "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
      },

      "heartbeat_refresh_seconds": 5,
      "livy_server_heartbeat_timeout_seconds": 60,
      "heartbeat_retry_seconds": 1
    }
    ```

4. Nehmen Sie die folgenden Änderungen an der Datei vor:

    |Vorlagenwert | Neuer Wert |
    |---|---|
    |{BENUTZERNAME}|Clusteranmeldung, Standardwert: `admin`.|
    |{CLUSTER-DNS-NAME}|Clustername|
    |{BASE64-CODIERTES-KENNWORT}|Ein Base64-codiertes Kennwort für Ihr eigentliches Kennwort.  Sie können unter [https://www.url-encode-decode.com/base64-encode-decode/](https://www.url-encode-decode.com/base64-encode-decode/) ein Base64-Kennwort generieren.|
    |`"livy_server_heartbeat_timeout_seconds": 60`|Behalten Sie den Wert bei, wenn Sie `sparkmagic 0.12.7` verwenden (Cluster der Versionen 3.5 und 3.6).  Wenn Sie `sparkmagic 0.2.3` verwenden (Cluster der Version 3.4), ersetzen Sie den Wert durch `"should_heartbeat": true`.|

    Eine vollständige Beispieldatei finden Sie im [Beispiel für „config.json“](https://github.com/jupyter-incubator/sparkmagic/blob/master/sparkmagic/example_config.json).

   > [!TIP]  
   > Takte werden gesendet, um sicherzustellen, dass bei Sitzungen keine Verluste auftreten. Wenn ein Computer in den Ruhezustand versetzt oder heruntergefahren wird, wird der Takt nicht gesendet, und die Sitzung wird bereinigt. Für Cluster der Version 3.4 gilt: Wenn Sie dieses Verhalten deaktivieren möchten, können Sie die Livy-Konfiguration `livy.server.interactive.heartbeat.timeout` über die Ambari-Benutzeroberfläche auf `0` festlegen. Für Cluster der Version 3.5 gilt: Wenn Sie die obige 3.5-Konfiguration nicht festlegen, wird die Sitzung nicht gelöscht.

5. Starten Sie Jupyter. Geben Sie in der Eingabeaufforderung folgenden Befehl ein:

    ```cmd
    jupyter notebook
    ```

6. Stellen Sie sicher, dass Sie die verfügbare Spark Magic-Version mit den Kerneln verwenden können. Führen Sie die folgenden Schritte aus:

    a. Erstellen Sie ein neues Notebook. Wählen Sie in der rechten Ecke **Neu** aus. Daraufhin sollten der **Python 2**- oder **Python 3**-Standardkernel und die von Ihnen installierten Kernel angezeigt werden. Die tatsächlichen Werte variieren je nach Ihren Installationsentscheidungen.  Wählen Sie **PySpark** aus.

    ![Verfügbare Kernels in Jupyter Notebooks](./media/apache-spark-jupyter-notebook-install-locally/jupyter-kernels-notebook.png "Kernels in Jupyter Notebooks")

    > [!IMPORTANT]  
    > Überprüfen Sie Ihre Shell nach der Auswahl von **Neu** auf Fehler.  Wenn der Fehler `TypeError: __init__() got an unexpected keyword argument 'io_loop'` angezeigt wird, ist möglicherweise ein bekanntes Problem mit bestimmten Versionen von Tornado aufgetreten.  Wenn dies der Fall ist, beenden Sie den Kernel, und stufen Sie anschließend die Tornado-Installation mit dem folgenden Befehl herab: `pip install tornado==4.5.3`.

    b. Führen Sie den folgenden Codeausschnitt aus.

    ```sql
    %%sql
    SELECT * FROM hivesampletable LIMIT 5
    ```  

    Wenn Sie die Ausgabe erfolgreich abrufen können, wird Ihre Verbindung zu dem HDInsight-Cluster getestet.

    Wenn Sie die Notebookkonfiguration für die Verbindung mit einem anderen Cluster aktualisieren möchten, aktualisieren Sie die Datei „config.json“ mit einem neuen Satz von Werten, wie in Schritt 3 oben dargestellt.

## <a name="why-should-i-install-jupyter-on-my-computer"></a>Warum sollte ich Jupyter auf meinem Computer installieren?

Es kann zahlreiche Gründe geben, warum Sie Jupyter auf dem Computer installieren und mit einem Apache Spark-Cluster in HDInsight verbinden möchten.

* Obwohl die Jupyter Notebooks bereits im Spark-Cluster in Azure HDInsight verfügbar sind, bietet Ihnen die Installation von Jupyter auf Ihrem Computer die Option, Ihre Notebooks lokal zu erstellen, Ihre Anwendung in einem aktiven Cluster zu testen und die Notebooks anschließend in den Cluster hochzuladen. Sie können die Notebooks entweder mithilfe des im Cluster ausgeführten Jupyter Notebooks hochladen oder sie im Ordner „/HdiNotebooks“ in dem dem Cluster zugeordneten Speicherkonto speichern. Weitere Informationen zum Speichern von Notebooks im Cluster finden Sie unter [Wo werden die Notebooks gespeichert?](apache-spark-jupyter-notebook-kernels.md#where-are-the-notebooks-stored)
* Wenn Notebooks lokal verfügbar sind, können Sie basierend auf den Anwendungsanforderungen Verbindungen mit unterschiedlichen Spark-Clustern herstellen.
* Sie können GitHub verwenden, um ein Quellcodeverwaltungssystem zu implementieren und Versionskontrolle für die Notebooks zu ermöglichen. Sie können auch eine Umgebung für Zusammenarbeit einrichten, in der mehrere Benutzer mit dem gleichen Notebook arbeiten können.
* Sie können mit Notebooks lokal ohne einen Cluster arbeiten. Sie benötigen einen Cluster nur zum Testen der Notebooks, nicht für das manuelle Verwalten von Notebooks oder einer Entwicklungsumgebung.
* Es ist möglicherweise einfacher, Ihre eigene lokale Entwicklungsumgebung zu konfigurieren, statt die Jupyter-Installation im Cluster zu konfigurieren.  Sie können sämtliche Software nutzen, die Sie lokal installiert haben, ohne Remotecluster zu konfigurieren.

> [!WARNING]  
> Wenn Jupyter auf dem lokalen Computer installiert ist, können mehrere Benutzer gleichzeitig das gleiche Notebook im gleichen Spark-Cluster ausführen. In diesem Fall werden mehrere Livy-Sitzungen erstellt. Wenn ein Problem auftritt, das Sie debuggen möchten, ist es eine komplexe Aufgabe nachzuverfolgen, welche Livy-Sitzung welchem Benutzer gehört.  

## <a name="next-steps"></a>Nächste Schritte

* [Übersicht: Apache Spark in Azure HDInsight](apache-spark-overview.md)
* [Apache Spark mit BI: Durchführen interaktiver Datenanalysen mithilfe von Spark in HDInsight mit BI-Tools](apache-spark-use-bi-tools.md)
* [Apache Spark mit Machine Learning: Analysieren von Gebäudetemperaturen mithilfe von Spark in HDInsight und HVAC-Daten](apache-spark-ipython-notebook-machine-learning.md)
