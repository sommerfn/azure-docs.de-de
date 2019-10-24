---
title: Azure HDInsight für Visual Studio Code
description: Es wird beschrieben, wie Sie die Spark- und Hive-Tools (Azure HDInsight) für Visual Studio Code verwenden, um Abfragen und Skripts zu erstellen und zu übermitteln.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 10/11/2019
ms.openlocfilehash: 03d0d26a21e710c07019d3ffcb13a1482a96af50
ms.sourcegitcommit: 9dec0358e5da3ceb0d0e9e234615456c850550f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2019
ms.locfileid: "72311730"
---
# <a name="use-spark--hive-tools-for-visual-studio-code"></a>Verwenden von Spark- und Hive-Tools für Visual Studio Code

Hier erfahren Sie, wie Sie Spark- und Hive-Tools für Visual Studio Code verwenden, um Apache Hive-Batchaufträge, interaktive Hive-Abfragen und PySpark-Skripts für Apache Spark zu erstellen und zu übermitteln. Zunächst wird die Installation von Spark- und Hive-Tools (Spark & Hive Tools) in Visual Studio Code beschrieben, und anschließend werden die Schritte zum Übermitteln von Aufträgen an Spark- und Hive-Tools erläutert.  

Spark- und Hive-Tools können auf den von Visual Studio Code unterstützten Plattformen installiert werden, z. B. Windows, Linux und macOS. Die folgenden Voraussetzungen gelten für die verschiedenen Plattformen.

## <a name="prerequisites"></a>Voraussetzungen

Für die Schritte in diesem Artikel ist Folgendes erforderlich:

- Ein Azure HDInsight-Cluster. Informationen zum Erstellen eines Clusters finden Sie unter [Hadoop-Tutorial: Erste Schritte bei der Verwendung von Hadoop in HDInsight](hadoop/apache-hadoop-linux-create-cluster-get-started-portal.md). Alternativ können Sie einen Spark- und Hive-Cluster verwenden, der einen Apache Livy-Endpunkt unterstützt.
- [Visual Studio Code](https://code.visualstudio.com/).
- [Mono](https://www.mono-project.com/docs/getting-started/install/). Mono wird nur für Linux und macOS benötigt.
- [Eine interaktive PySpark-Umgebung für Visual Studio Code](set-up-pyspark-interactive-environment.md).
- Ein lokales Verzeichnis. In diesem Artikel wird **C:\HD\HDexample** verwendet.

## <a name="install-spark--hive-tools"></a>Installieren von Spark- und Hive-Tools

Wenn die Voraussetzungen erfüllt sind, können Sie Spark- und Hive-Tools für Visual Studio Code installieren, indem Sie die folgenden Schritte ausführen:

1. Öffnen Sie Visual Studio Code.

2. Navigieren Sie auf der Menüleiste zu **Ansicht** > **Erweiterungen**.

3. Geben Sie im Suchfeld **Spark und Hive** ein.

4. Wählen Sie in den Suchergebnissen **Spark & Hive Tools** aus, und wählen Sie dann **Installieren** aus:

   ![Installation der Python-Erweiterung für Spark- und Hive-Tools für Visual Studio Code](./media/hdinsight-for-vscode/install-hdInsight-plugin.png)

5. Wählen Sie bei Bedarf **Erneut laden** aus.

## <a name="open-a-work-folder"></a>Öffnen eines Arbeitsordners

Um einen Arbeitsordner zu öffnen und eine Datei in Visual Studio Code zu erstellen, führen Sie die folgenden Schritte aus:

1. Navigieren Sie auf der Menüleiste zu **Datei** > **Ordner öffnen...**  > **C:\HD\HDexample**, und klicken Sie dann auf die Schaltfläche **Ordner auswählen**. Der Ordner wird in der **Explorer**-Ansicht auf der linken Seite angezeigt.

2. Wählen Sie in der **Explorer**-Ansicht den Ordner **HDexample** aus, und wählen Sie dann neben dem Arbeitsordner das Symbol **Neue Datei** aus:

   ![Visual Studio Code: Symbol „Neue Datei“](./media/hdinsight-for-vscode/visual-studio-code-new-file.png)

3. Benennen Sie die neue Datei mit der Dateierweiterung `.hql` (Hive-Abfragen) oder `.py` (Spark-Skript). In diesem Beispiel wird **HelloWorld.hql** verwendet.

## <a name="set-the-azure-environment"></a>Einrichten der Azure-Umgebung

Führen Sie für Benutzer der nationalen Cloud diese Schritte aus, um zunächst die Azure-Umgebung festzulegen, und verwenden Sie anschließend den Befehl **Azure: Anmelden**, um sich bei Azure anzumelden:

1. Navigieren Sie zu **Datei** > **Einstellungen** > **Einstellungen**.
2. Suchen Sie nach folgender Zeichenfolge: **Azure: Cloud**.
3. Wählen Sie in der Liste die nationale Cloud aus:

   ![Festlegen der Konfiguration des Standardanmeldeeintrags](./media/hdinsight-for-vscode/set-default-login-entry-configuration.png)

## <a name="connect-to-an-azure-account"></a>Verbinden mit einem Azure-Konto

Bevor Sie Skripts aus Visual Studio Code an Ihre Cluster übermitteln können, müssen Sie entweder eine Verbindung mit Ihrem Azure-Konto herstellen oder einen Cluster verknüpfen (mit Apache Ambari-Benutzername und -Kennwort als Anmeldeinformationen oder mit einem in die Domäne eingebundenen Konto). Führen Sie diese Schritte aus, um eine Verbindung mit Azure herzustellen:

1. Navigieren Sie auf der Menüleiste zu **Ansicht** > **Befehlspalette...** , und geben Sie **Azure: Anmelden** ein:

    ![Spark- und Hive-Tools für Visual Studio Code: Anmeldung](./media/hdinsight-for-vscode/hdinsight-for-vscode-extension-login.png)

2. Befolgen Sie die Anleitung für die Anmeldung, um sich bei Azure anzumelden. Nach dem Herstellen der Verbindung wird Ihr Azure-Kontoname unten im Visual Studio Code-Fenster in der Statusleiste angezeigt.  

## <a name="link-a-cluster"></a>Verknüpfen eines Clusters

### <a name="link-azure-hdinsight"></a>Verknüpfung: Azure HDInsight

Sie können einen normalen Cluster verknüpfen, indem Sie einen von [Apache Ambari](https://ambari.apache.org/) verwalteten Benutzernamen verwenden, oder Sie können einen per Enterprise Security Pack geschützten Hadoop-Cluster verknüpfen, indem Sie einen Domänenbenutzernamen (z.B. `user1@contoso.com`) verwenden.

1. Navigieren Sie auf der Menüleiste zu **Ansicht** > **Befehlspalette...** , und geben Sie **Spark/Hive: Link a Cluster** (HDInsight: Cluster verknüpfen) ein.

   ![Befehlspalette: Befehl für die Clusterverknüpfung](./media/hdinsight-for-vscode/link-cluster-command.png)

2. Wählen Sie als Typ des verknüpften Clusters **Azure HDInsight** aus.

3. Geben Sie die HDInsight-Cluster-URL ein.

4. Geben Sie Ihren Ambari-Benutzernamen ein. Der Standardname ist **admin**.

5. Geben Sie Ihr Ambari-Kennwort ein.

6. Wählen Sie den Clustertyp aus.

7. Legen Sie den Anzeigenamen des Clusters fest (optional).

8. Überprüfen Sie die Ansicht **AUSGABE** auf eine Bestätigung.

   > [!NOTE]  
   > Der verknüpfte Benutzername und das verknüpfte Kennwort werden verwendet, wenn der Cluster beim Azure-Abonnement angemeldet ist und einen Cluster verknüpft hat.  

### <a name="link-generic-livy-endpoint"></a>Verknüpfung: Generic Livy Endpoint (Generischer Livy-Endpunkt)

1. Navigieren Sie auf der Menüleiste zu **Ansicht** > **Befehlspalette...** , und geben Sie **Spark/Hive: Link a Cluster** (HDInsight: Cluster verknüpfen) ein.

2. Wählen Sie als Typ des verknüpften Clusters **Generic Livy Endpoint** (Generischer Livy-Endpunkt) aus.

3. Geben Sie den generischen Livy-Endpunkt ein. Beispiel: http\://10.172.41.42:18080.

4. Wählen Sie als Autorisierungstyp **Standard** oder **Keine** aus.  Wenn Sie **Standard** auswählen:  
    &emsp;a. Geben Sie Ihren Ambari-Benutzernamen ein. Der Standardname ist **admin**.  
    &emsp;b. Geben Sie Ihr Ambari-Kennwort ein.

5. Überprüfen Sie die Ansicht **AUSGABE** auf eine Bestätigung.

## <a name="list-clusters"></a>Auflisten von Clustern

1. Navigieren Sie auf der Menüleiste zu **Ansicht** > **Befehlspalette...** , und geben Sie **Spark/Hive: List Cluster** (HDInsight: Cluster auflisten) ein.

2. Wählen Sie das gewünschte Abonnement aus.

3. Überprüfen Sie die Ansicht **AUSGABE**. Diese Ansicht zeigt Ihre verknüpften Cluster und alle Cluster in Ihrem Azure-Abonnement:

    ![Festlegen einer Standardclusterkonfiguration](./media/hdinsight-for-vscode/list-cluster-result1.png)

## <a name="set-the-default-cluster"></a>Festlegen des Standardclusters

1. Öffnen Sie den Ordner **HDexample**, der [weiter oben](#open-a-work-folder) erläutert ist, erneut (sofern er geschlossen ist).  

2. Wählen Sie die Datei **HelloWorld.hql** aus, die [zuvor](#open-a-work-folder) erstellt wurde. Sie wird im Skript-Editor geöffnet.

3. Klicken Sie mit der rechten Maustaste auf den Skript-Editor, und wählen Sie dann **Spark / Hive: Set Default Cluster** (HDInsight: Standardcluster festlegen) aus.  

4. [Stellen Sie eine Verbindung mit Ihrem Azure-Konto her](#connect-to-an-azure-account), oder verknüpfen Sie einen Cluster, sofern dies noch nicht erfolgt ist.

5. Wählen Sie einen Cluster als Standardcluster für die aktuelle Skriptdatei aus. Die Tools aktualisieren die Konfigurationsdatei **.VSCode\settings.json** automatisch:

   ![Festlegen der Standardclusterkonfiguration](./media/hdinsight-for-vscode/set-default-cluster-configuration.png)

## <a name="submit-interactive-hive-queries-and-hive-batch-scripts"></a>Übermitteln von interaktiven Hive-Abfragen und von Hive-Batchskripts

Mit Spark- und Hive-Tools für Visual Studio Code können Sie interaktive Hive-Abfragen und Hive-Batchskripts an Ihre Cluster übermitteln.

1. Öffnen Sie den Ordner **HDexample**, der [weiter oben](#open-a-work-folder) erläutert ist, erneut (sofern er geschlossen ist).  

2. Wählen Sie die Datei **HelloWorld.hql** aus, die [zuvor](#open-a-work-folder) erstellt wurde. Sie wird im Skript-Editor geöffnet.

3. Kopieren Sie den folgenden Code, fügen Sie ihn in Ihre Hive-Datei ein, und speichern Sie diese dann:

    ```hiveql
    SELECT * FROM hivesampletable;
    ```

4. [Stellen Sie eine Verbindung mit Ihrem Azure-Konto her](#connect-to-an-azure-account), oder verknüpfen Sie einen Cluster, sofern dies noch nicht erfolgt ist.

5. Klicken Sie mit der rechten Maustaste auf den Skript-Editor, und wählen Sie **Hive: Interactive** aus, um die Abfrage zu übermitteln, oder verwenden Sie die Tastenkombination STRG+ALT+I.  Wählen Sie **Hive: Batch** aus, um das Skript zu übermitteln, oder verwenden Sie die Tastenkombination STRG+ALT+H.  

6. Wenn Sie keinen Standardcluster angegeben haben, wählen Sie einen Cluster aus. Die Tools unterstützen es auch, dass Sie über das Kontextmenü einen Codeblock anstelle der gesamten Skriptdatei übermitteln. Nach kurzer Zeit werden die Abfrageergebnisse auf einer neuen Registerkarte angezeigt:

   ![Ergebnis der interaktiven Apache Hive-Abfrage](./media/hdinsight-for-vscode/interactive-hive-result.png)

    - Bereich **ERGEBNISSE**: Sie können das gesamte Ergebnis als CSV-, JSON- oder Excel-Datei in einem lokalen Pfad speichern oder einfach mehrere Zeilen auswählen.

    - Bereich **MELDUNGEN**: Wenn Sie eine **Zeilennummer** auswählen, gelangen Sie zur ersten Zeile des Skripts, das derzeit ausgeführt wird.

## <a name="submit-interactive-pyspark-queries"></a>Übermitteln interaktiver PySpark-Abfragen

Wenn Sie interaktive PySpark-Abfragen übermitteln möchten, führen Sie die folgenden Schritte aus:

1. Öffnen Sie den Ordner **HDexample**, der [weiter oben](#open-a-work-folder) erläutert ist, erneut (sofern er geschlossen ist).  

2. Erstellen Sie mit den [zuvor](#open-a-work-folder) erläuterten Schritten eine neue **HelloWorld.py**-Datei.

3. Kopieren Sie den folgenden Code, und fügen Sie ihn in die Skriptdatei ein:

   ```python
   from operator import add
   lines = spark.read.text("/HdiSamples/HdiSamples/FoodInspectionData/README").rdd.map(lambda r: r[0])
   counters = lines.flatMap(lambda x: x.split(' ')) \
                .map(lambda x: (x, 1)) \
                .reduceByKey(add)

   coll = counters.collect()
   sortedCollection = sorted(coll, key = lambda r: r[1], reverse = True)

   for i in range(0, 5):
        print(sortedCollection[i])
   ```

4. [Stellen Sie eine Verbindung mit Ihrem Azure-Konto her](#connect-to-an-azure-account), oder verknüpfen Sie einen Cluster, sofern dies noch nicht erfolgt ist.

5. Wählen Sie den gesamten Code aus, klicken Sie mit der rechten Maustaste auf den Skript-Editor, und wählen Sie **Spark: PySpark Interactive** aus, um die Abfrage zu übermitteln. Oder verwenden Sie die Tastenkombination STRG+ALT+I.

   ![Kontextmenü von PySpark Interactive](./media/hdinsight-for-vscode/pyspark-interactive-right-click.png)

6. Wählen Sie den Cluster aus, wenn Sie keinen Standardcluster angegeben haben. Nach kurzer Zeit werden die **Python Interactive**-Ergebnisse auf einer neuen Registerkarte angezeigt. Die Tools unterstützen es auch, dass Sie über das Kontextmenü einen Codeblock anstelle der gesamten Skriptdatei übermitteln:

   ![Fenster mit PySpark Interactive und Python Interactive](./media/hdinsight-for-vscode/pyspark-interactive-python-interactive-window.png)

7. Geben Sie **%%info** ein, und drücken Sie dann UMSCHALT+EINGABETASTE, um die Auftragsinformationen anzuzeigen (optional):

   ![PySpark: Auftragsinformationen in der interaktiven Ansicht](./media/hdinsight-for-vscode/pyspark-interactive-view-job-information.png)

8. Das Tool unterstützt auch die **Spark SQL**-Abfrage:

   ![PySpark Interactive Ergebnis anzeigen](./media/hdinsight-for-vscode/pyspark-ineteractive-select-result.png)

   Der Übertragungsstatus wird beim Ausführen von Abfragen links neben der unteren Statusleiste angezeigt. Übermitteln Sie keine weiteren Abfragen, wenn der Status **PySpark Kernel (busy)** (PySpark-Kernel (ausgelastet)) lautet.  

   > [!NOTE]
   >
   > Wenn **Python Extension Enabled** in den Einstellungen deaktiviert ist (die Option ist standardmäßig aktiviert), werden die übermittelten Ergebnisse für die PySpark-Interaktion im alten Fenster angezeigt:
   >
   > ![Python-Erweiterung für PySpark Interactive deaktiviert](./media/hdinsight-for-vscode/pyspark-interactive-python-extension-disabled.png)

## <a name="submit-pyspark-batch-job"></a>Übermitteln eines PySpark-Batchauftrags

1. Öffnen Sie den Ordner **HDexample**, der [weiter oben](#open-a-work-folder) erläutert ist, erneut (sofern er geschlossen ist).  

2. Erstellen Sie mit den [zuvor](#open-a-work-folder) erläuterten Schritten eine neue **BatchFile.py**-Datei.

3. Kopieren Sie den folgenden Code, und fügen Sie ihn in die Skriptdatei ein:

    ```python
    from __future__ import print_function
    import sys
    from operator import add
    from pyspark.sql import SparkSession
    if __name__ == "__main__":
        spark = SparkSession\
            .builder\
            .appName("PythonWordCount")\
            .getOrCreate()
    
        lines = spark.read.text('/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv').rdd.map(lambda r: r[0])
        counts = lines.flatMap(lambda x: x.split(' '))\
                    .map(lambda x: (x, 1))\
                    .reduceByKey(add)
        output = counts.collect()
        for (word, count) in output:
            print("%s: %i" % (word, count))
        spark.stop()
    ```

4. [Stellen Sie eine Verbindung mit Ihrem Azure-Konto her](#connect-to-an-azure-account), oder verknüpfen Sie einen Cluster, sofern dies noch nicht erfolgt ist.

5. Klicken Sie mit der rechten Maustaste auf den Skript-Editor, und wählen Sie anschließend **Spark: PySpark Batch** aus, oder verwenden Sie die Tastenkombination STRG+ALT+H.

6. Wählen Sie einen Cluster aus, an den Ihr PySpark-Auftrag übermittelt werden soll:

   ![Ergebnisausgabe für die Übermittlung des Python-Auftrags](./media/hdinsight-for-vscode/submit-pythonjob-result.png)

Nach dem Übermitteln eines Python-Auftrags werden die Übermittlungsprotokolle in Visual Studio Code im Fenster **AUSGABE** angezeigt. Die URL der Spark-Benutzeroberfläche und die URL der Yarn-Benutzeroberfläche werden ebenfalls angezeigt. Sie können die URL in einem Webbrowser öffnen, um den Auftragsstatus nachzuverfolgen.

## <a name="apache-livy-configuration"></a>Apache Livy-Konfiguration

Die [Apache Livy](https://livy.incubator.apache.org/)-Konfiguration wird unterstützt. Sie können Sie in der Datei **.VSCode\settings.json** im Arbeitsbereichsordner konfigurieren. Derzeit wird in der Livy-Konfiguration nur das Python-Skript unterstützt. Ausführlichere Informationen finden Sie in der [Livy-README](https://github.com/cloudera/livy/blob/master/README.rst ).

<a id="triggerlivyconf"></a>**Auslösen der Livy-Konfiguration**

Methode 1  
1. Navigieren Sie auf der Menüleiste zu **Datei** > **Einstellungen** > **Einstellungen**.
2. Geben Sie in das Feld **Sucheinstellungen** die Zeichenfolge **HDInsight Job Submission: Livy Conf** (HDInsight-Auftragsübermittlung: Livy-Konfiguration) ein.  
3. Wählen Sie für das relevante Suchergebnis **Edit in settings.json** (In „settings.json“ bearbeiten) aus.

Methode 2: Übermitteln Sie eine Datei. Daraufhin wird der Ordner „.vscode“ automatisch dem Arbeitsordner hinzugefügt. Sie können die Livy-Konfiguration anzeigen, indem Sie **.vscode\settings.json** auswählen.

+ Die Projekteinstellungen:

    ![HDInsight Apache: Livy-Konfiguration](./media/hdinsight-for-vscode/hdi-apache-livy-config.png)

    >[!NOTE]
    >Legen Sie für die Einstellungen **driverMemory** und **executorMemory** den Wert und die Einheit fest. Beispiel:  1g oder 1024m.

+ Unterstützte Livy-Konfigurationen:

    **POST /batches** Anforderungstext

    | name | description | type |
    | :- | :- | :- |
    | file | Die Datei, die die auszuführende Anwendung enthält. | Pfad (erforderlich) |
    | proxyUser | Der Benutzer, dessen Identität bei Auftragsausführung gewechselt wird. | Zeichenfolge |
    | className | Die Java-/Spark-Hauptklasse der Anwendung. | Zeichenfolge |
    | args | Die Befehlszeilenargumente für die Anwendung | Liste von Zeichenfolgen |
    | jars | JAR-Dateien, die in dieser Sitzung verwendet werden | Liste von Zeichenfolgen | 
    | pyFiles | Python-Dateien, die in dieser Sitzung verwendet werden. | Liste von Zeichenfolgen |
    | files | Dateien, die in dieser Sitzung verwendet werden | Liste von Zeichenfolgen |
    | driverMemory | Die Menge an Arbeitsspeicher, der für den Treiberprozess verwendet wird. | Zeichenfolge |
    | driverCores | Die Anzahl von Kernen, die für den Treiberprozess verwendet wird. | Int |
    | executorMemory | Die Menge an Arbeitsspeicher, die pro Executorprozess verwendet wird. | Zeichenfolge |
    | executorCores | Die Anzahl von Kernen, die für jeden Executor verwendet wird. | Int |
    | numExecutors | Die Anzahl von Executors, die für diese Sitzung gestartet werden. | Int |
    | archives | Die Archive, die in dieser Sitzung verwendet werden. | Liste von Zeichenfolgen |
    | queue | Der Name der YARN-Warteschlange, an die gesendet wird| Zeichenfolge |
    | name | Der Name dieser Sitzung | Zeichenfolge |
    | conf | Eigenschaften der Spark-Konfiguration. | Zuordnung von Schlüssel=Wert |

    Antworttext: Das erstellte Batchobjekt.

    | name | description | type |
    | :- | :- | :- |
    | id | Sitzungs-ID | Int |
    | appId | Die Anwendungs-ID dieser Sitzung | Zeichenfolge |
    | appInfo | Detaillierte Anwendungsinformationen | Zuordnung von Schlüssel=Wert |
    | log | Protokollzeilen | Liste von Zeichenfolgen |
    | state |Batchstatus | Zeichenfolge |

    > [!NOTE]
    > Die zugewiesene Livy-Konfiguration wird im Ausgabebereich angezeigt, wenn Sie das Skript übermitteln.

## <a name="integrate-with-azure-hdinsight-from-explorer"></a>Integration mit Azure HDInsight per Explorer

Sie können eine Vorschau der Hive-Tabelle in Ihren Clustern direkt über den **Azure HDInsight**-Explorer anzeigen:

1. [Stellen Sie eine Verbindung mit Ihrem Azure-Konto her](#connect-to-an-azure-account), sofern noch keine Verbindung besteht.

2. Wählen Sie das **Azure**-Symbol in der äußerst linken Spalte aus.

3. Erweitern Sie im linken Bereich **AZURE: HDINSIGHT**. Die verfügbaren Abonnements und Cluster werden aufgelistet.

4. Erweitern Sie den Cluster, um die Hive-Metadatendatenbank und das Hive-Tabellenschema anzuzeigen.

5. Klicken Sie mit der rechten Maustaste auf die Hive-Tabelle. Zum Beispiel: **hivesampletable**. Wählen Sie **Vorschau** aus.

   ![Spark und Hive für Visual Studio Code: Vorschau für Hive-Tabelle](./media/hdinsight-for-vscode/hdinsight-for-vscode-preview-hive-table.png)

6. Das Fenster **Ergebnisvorschau** wird geöffnet:

   ![Spark und Hive für Visual Studio Code: Fenster „Ergebnisvorschau“](./media/hdinsight-for-vscode/hdinsight-for-vscode-preview-results-window.png)

- Bereich ERGEBNISSE

   Sie können das gesamte Ergebnis als CSV-, JSON- oder Excel-Datei in einem lokalen Pfad speichern, oder wählen Sie einfach mehrere Zeilen aus.

- Bereich MELDUNGEN
   1. Wenn die Tabelle mehr als 100 Zeilen enthält, sehen Sie die folgende Meldung: „Die ersten 100 Zeilen werden für die Hive-Tabelle angezeigt.“
   2. Wenn die Tabelle bis zu 100 Zeilen enthält, sehen Sie eine Meldung wie die folgende: „60 Zeilen werden für die Hive-Tabelle angezeigt.“
   3. Wenn die Tabelle keine Zeilen enthält, sehen Sie die folgende Meldung: „0 Zeilen werden für die Hive-Tabelle angezeigt.“

        >[!NOTE]
        >
        >Installieren Sie unter Linux xclip, um das Kopieren von Tabellendaten zu ermöglichen.
        >
        >![Spark und Hive für Visual Studio Code unter Linux](./media/hdinsight-for-vscode/hdinsight-for-vscode-preview-linux-install-xclip.png)

## <a name="additional-features"></a>Zusätzliche Funktionen

Spark und Hive für Visual Studio Code unterstützt auch die folgenden Features:

- **IntelliSense-AutoVervollständigen**. Es werden Vorschläge für Schlüsselwörter, Methoden, Variablen und weitere Programmierelemente aufgelistet. Die unterschiedlichen Objekttypen werden durch entsprechende Symbole dargestellt:

    ![Spark- und Hive-Tools für Visual Studio Code: IntelliSense-Objekte](./media/hdinsight-for-vscode/hdinsight-for-vscode-auto-complete-objects.png)

- **IntelliSense-Fehlermarker**. Der Sprachdienst unterstreicht Bearbeitungsfehler im Hive-Skript.     
- **Syntaxmarkierungen**. Der Sprachdienst verwendet verschiedene Farben, um Variablen, Schlüsselwörter, Datentypen, Funktionen und weitere Programmierelemente zu unterscheiden:

    ![Spark- und Hive-Tools für Visual Studio Code: Syntaxmarkierungen](./media/hdinsight-for-vscode/hdinsight-for-vscode-syntax-highlights.png)

## <a name="reader-only-role"></a>Rolle nur mit Leseberechtigung

Benutzer, denen für den Cluster die Rolle „Nur Leser“ (reader-only) zugewiesen ist, können keine Aufträge mehr an den HDInsight-Cluster übermitteln und können die Hive-Datenbank nicht mehr anzeigen. Wenden Sie sich an den Clusteradministrator, damit Ihre Rolle im [Azure-Portal](https://ms.portal.azure.com/) auf [**HDInsight-Clusteroperator**](https://docs.microsoft.com/azure/hdinsight/hdinsight-migrate-granular-access-cluster-configurations#add-the-hdinsight-cluster-operator-role-assignment-to-a-user) aktualisiert wird. Wenn Sie gültige Ambari-Anmeldeinformationen haben, können Sie manuell mit dem Cluster verknüpfen, indem Sie die weiter unten aufgeführten Anweisungen ausführen.

### <a name="browse-the-hdinsight-cluster"></a>Durchsuchen des HDInsight-Clusters  

Wenn Sie im Azure HDInsight-Explorer einen HDInsight-Cluster auswählen, um diesen zu erweitern, werden Sie zum Verknüpfen mit dem Cluster aufgefordert, wenn Sie für den Cluster die Rolle „Nur Leser“ haben. Verwenden Sie die folgende Methode, um über Ihre Ambari-Anmeldeinformationen die Verknüpfung mit dem Cluster herzustellen.

### <a name="submit-the-job-to-the-hdinsight-cluster"></a>Übermitteln des Auftrags an den HDInsight-Cluster

Wenn Sie für den Cluster die Rolle „Nur Leser“ haben und einen Auftrag an den HDInsight-Cluster übermitteln, werden Sie aufgefordert, mit dem Cluster zu verknüpfen. Führen Sie die folgenden Schritte aus, um über die Ambari-Anmeldeinformationen die Verknüpfung mit dem Cluster herzustellen.

### <a name="link-to-the-cluster"></a>Verknüpfen mit dem Cluster

1. Geben Sie einen gültigen Ambari-Benutzernamen ein.
2. Geben Sie ein gültiges Kennwort ein.

   ![Spark- und Hive-Tools für Visual Studio Code: Benutzername](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-username.png)

   ![Spark- und Hive-Tools für Visual Studio Code: Kennwort](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-password.png)

  > [!NOTE]
  >
  >Sie können `Spark / Hive: List Cluster` verwenden, um den verknüpften Cluster zu überprüfen:
  >
  >![Spark- und Hive-Tools für Visual Studio Code: Leser verknüpft](./media/hdinsight-for-vscode/list-cluster-result1.png)

## <a name="azure-data-lake-storage-gen2"></a>Azure Data Lake Storage Gen2

### <a name="browse-a-data-lake-storage-gen2-account"></a>Durchsuchen eines Data Lake Storage Gen2-Kontos

Wenn Sie den Azure HDInsight-Explorer auswählen, um ein Azure Data Lake Storage Gen2-Konto zu erweitern, werden Sie aufgefordert, den Zugriffsschlüssel für den Speicher einzugeben, wenn Ihr Azure-Konto keinen Zugriff auf den Gen2-Speicher hat. Sobald der Zugriffsschlüssel validiert ist, wird das Data Lake Storage Gen2-Konto automatisch erweitert.

### <a name="submit-jobs-to-an-hdinsight-cluster-with-data-lake-storage-gen2"></a>Übermitteln von Aufträgen an einen HDInsight-Cluster mit Data Lake Storage Gen2

Wenn Sie einen Auftrag über Azure Data Lake Storage Gen2 an einen HDInsight-Cluster übermitteln, werden Sie aufgefordert, den Zugriffsschlüssel für den Speicher einzugeben, wenn Ihr Azure-Konto keinen Schreibzugriff auf den Gen2-Speicher hat. Sobald der Zugriffsschlüssel validiert ist, wird der Auftrag erfolgreich übermittelt.

![Spark- und Hive-Tools für Visual Studio Code: Zugriffsschlüssel](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-accesskey.png)

> [!NOTE]
>
> Den Zugriffsschlüssel für das Speicherkonto erhalten Sie über das Azure-Portal. Weitere Informationen finden Sie unter [Anzeigen und Kopieren von Zugriffsschlüsseln](https://docs.microsoft.com/azure/storage/common/storage-account-manage#access-keys).

## <a name="unlink-cluster"></a>Aufheben einer Clusterverknüpfung

1. Navigieren Sie auf der Menüleiste zu **Ansicht** > **Befehlspalette**, und geben Sie dann **Spark / Hive: Unlink a Cluster** (HDInsight: Verknüpfung eines Clusters aufheben) ein.  

2. Wählen Sie den Cluster aus, dessen Verknüpfung aufgehoben werden soll.  

3. Sehen Sie sich die Ansicht **AUSGABE** zur Überprüfung an.  

## <a name="sign-out"></a>Abmelden  

Navigieren Sie auf der Menüleiste zu **Ansicht** > **Befehlspalette**, und geben Sie dann **Azure: Abmelden** ein.

## <a name="next-steps"></a>Nächste Schritte

Ein Video, in dem die Verwendung von Spark und Hive für Visual Studio Code gezeigt wird, finden Sie unter [Spark und Hive für Visual Studio Code](https://go.microsoft.com/fwlink/?linkid=858706).
