---
title: 'Azure HDInsight Tools: Verwenden von Visual Studio Code für Hive, LLAP oder PySpark'
description: Hier erfahren Sie, wie Sie mithilfe von Azure HDInsight Tools for Visual Studio Code Abfragen und Skripts erstellen und übermitteln.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/30/2019
ms.openlocfilehash: aadfae9a7b74986fd0ac8857669dd3ccaf62af1f
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2019
ms.locfileid: "67166134"
---
# <a name="use-azure-hdinsight-tools-for-visual-studio-code"></a>Verwenden von Azure HDInsight Tools for Visual Studio Code

Hier erfahren Sie, wie Sie Azure HDInsight Tools for Visual Studio Code verwenden, um Apache Hive-Batchaufträge, interaktive Hive-Abfragen und PySpark-Skripts für Apache Spark zu erstellen und zu übermitteln. Zunächst wird die Installation von HDInsight Tools in Visual Studio Code beschrieben, und dann werden die Schritte zum Übermitteln von Aufträgen an Hive und Spark erläutert.  

Azure HDInsight Tools kann auf den von Visual Studio Code unterstützten Plattformen installiert werden, darunter Windows, Linux und macOS. Nachstehend finden Sie die Voraussetzungen für die unterschiedlichen Plattformen.


## <a name="prerequisites"></a>Voraussetzungen

Für die Schritte in diesem Artikel ist Folgendes erforderlich:

- Einen HDInsight-Cluster Informationen zum Erstellen eines Clusters finden Sie unter [Hadoop-Tutorial: Erste Schritte bei der Verwendung von Hadoop in HDInsight](hadoop/apache-hadoop-linux-create-cluster-get-started-portal.md).
- [Visual Studio Code](https://code.visualstudio.com/).
- [Mono](https://www.mono-project.com/docs/getting-started/install/). Mono wird nur für Linux und macOS benötigt.
- Die [Azure-Kontoerweiterung](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) für Visual Studio Code
- [Einrichten einer interaktiven PySpark-Umgebung für Visual Studio Code](set-up-pyspark-interactive-environment.md)
- Ein lokales Verzeichnis mit dem Namen **HDexample**.  In diesem Artikel wird **C:\HD\HDexample** verwendet.

## <a name="install-azure-hdinsight-tools"></a>Installieren von Azure HDInsight Tools

Wenn die Voraussetzungen erfüllt sind, können Sie Azure HDInsight Tools for Visual Studio Code installieren.  Führen Sie die folgenden Schritte aus, um Azure HDInsight Tools zu installieren:

1. Öffnen Sie Visual Studio Code.

2. Navigieren Sie auf der Menüleiste zu **Ansicht** > **Erweiterungen**.

3. Geben Sie **HDInsight** in das Suchfeld ein.

4. Wählen Sie in den Suchergebnissen **Azure HDInsight Tools** aus, und wählen Sie dann **Installieren**.  

   ![HDInsight für Visual Studio Code Python-Installation](./media/hdinsight-for-vscode/install-hdInsight-plugin.png)

5. Wählen Sie **Erneut laden** aus, um die Erweiterung **Azure HDInsight Tools** nach der Installation zu aktivieren.


## <a name="open-hdinsight-work-folder"></a>Öffnen des HDInsight-Arbeitsordners

Führen Sie die folgenden Schritte aus, um einen Arbeitsordner zu öffnen und eine Datei in Visual Studio Code zu erstellen:

1. Navigieren Sie auf der Menüleiste zu **Datei** > **Ordner öffnen...**  > **C:\HD\HDexample**, und wählen Sie die Schaltfläche **Ordner auswählen**. Der Ordner wird in der **Explorer**-Ansicht auf der linken Seite angezeigt.

2. Wählen Sie in der **Explorer**-Ansicht den Ordner **HDexample** aus, und wählen Sie dann neben dem Arbeitsordner das Symbol **Neue Datei**.

   ![Neue Datei](./media/hdinsight-for-vscode/new-file.png)

3. Benennen Sie die neue Datei mit der Dateierweiterung `.hql` (Hive-Abfragen) oder `.py` (Spark-Skript).  In diesem Beispiel wird **HelloWorld.hql** verwendet.

## <a name="set-the-azure-environment"></a>Einrichten der Azure-Umgebung

1. [Stellen Sie eine Verbindung mit Ihrem Azure-Konto her](#connect-to-azure-account), oder verknüpfen Sie einen Cluster, sofern noch keine Verbindung besteht.

2. Navigieren Sie auf der Menüleiste zu **Ansicht** > **Befehlspalette...** , und geben Sie **HDInsight: Set Azure Environment** (Azure-Umgebung festlegen) ein.

3. Wählen Sie eine Umgebung als Standardanmeldeeintrag aus.

4. Inzwischen hat das Tool Ihren Standardanmeldeeintrag bereits unter **VSCode\settings.json** gespeichert. Sie können ihn auch direkt in dieser Konfigurationsdatei aktualisieren. 

   ![Festlegen der Konfiguration des Standardanmeldeeintrags](./media/hdinsight-for-vscode/set-default-login-entry-configuration.png)

## <a name="connect-to-azure-account"></a>Verbinden mit Azure-Konto

Bevor Sie Skripts aus Visual Studio Code an HDInsight-Cluster übermitteln können, müssen Sie entweder eine Verbindung mit Ihrem Azure-Konto herstellen oder einen Cluster verknüpfen (unter Verwendung des Ambari-Benutzernamens/-Kennworts oder eines in die Domäne eingebunden Kontos).  Führen Sie die folgenden Schritte aus, um eine Verbindung mit Azure herzustellen:

1. Navigieren Sie auf der Menüleiste zu **Ansicht** > **Befehlspalette...** , und geben Sie **HDInsight: Login** (HDInsight: Anmeldung) eingeben.

    ![HDInsight Tools for Visual Studio Code-Anmeldung](./media/hdinsight-for-vscode/hdinsight-for-vscode-extension-login.png)

2. Befolgen Sie Anmeldeanweisungen im Bereich **AUSGABE**.
    + Für die globale Azure-Umgebung löst der Befehl **HDInsight: Anmeldung** die Aktion **Anmelden bei Azure** im HDInsight-Explorer aus und umgekehrt.

        ![Anmeldeanweisungen für Azure](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-signin.png)

    + Befolgen Sie für andere Umgebungen die Anmeldeanweisungen.

        ![Anmeldeanweisungen für andere Umgebung](./media/hdinsight-for-vscode/hdi-azure-hdinsight-hdinsight-signin.png)

   Nach dem Herstellen der Verbindung wird Ihr Azure-Kontoname unten links im Visual Studio Code-Fenster auf der Statusleiste angezeigt.  
  

## <a name="link-a-cluster"></a>Verknüpfen eines Clusters

### <a name="link-azure-hdinsight"></a>Verknüpfung: Azure HDInsight

Sie können einen normalen Cluster verknüpfen, indem Sie einen per [Apache Ambari](https://ambari.apache.org/) verwalteten Benutzernamen verwenden, oder einen per Enterprise Security Pack geschützten Hadoop-Cluster, indem Sie einen Domänenbenutzernamen (z.B. user1@contoso.com) verwenden.

1. Navigieren Sie auf der Menüleiste zu **Ansicht** > **Befehlspalette...** , und geben Sie **HDInsight: Link a Cluster** (HDInsight: Cluster verknüpfen) ein.

   ![Befehl „Cluster verknüpfen“](./media/hdinsight-for-vscode/link-cluster-command.png)

2. Wählen Sie als Typ des verknüpften Clusters **Azure HDInsight** aus.

3. Geben Sie die HDInsight-Cluster-URL ein.

4. Geben Sie den Ambari-Benutzernamen ein. Der Standardwert lautet **Administrator**.

5. Geben Sie das Ambari-Kennwort ein.

6. Wählen Sie den Clustertyp aus.

7. Überprüfen Sie die Ansicht **AUSGABE** auf eine Bestätigung.

   > [!NOTE]  
   > Der verknüpfte Benutzername und das Kennwort werden verwendet, wenn der Cluster im Azure-Abonnement angemeldet ist und einen Cluster verknüpft hat.  



### <a name="link-generic-livy-endpoint"></a>Verknüpfung: Generic Livy Endpoint (Generischer Livy-Endpunkt)

1. Navigieren Sie auf der Menüleiste zu **Ansicht** > **Befehlspalette...** , und geben Sie **HDInsight: Link a Cluster** (HDInsight: Cluster verknüpfen) ein.

2. Wählen Sie als Typ des verknüpften Clusters **Generic Livy Endpoint** (Generischer Livy-Endpunkt) aus.

3. Geben Sie den generischen Livy-Endpunkt ein, z.B. http\://10.172.41.42:18080.

4. Wählen Sie als Autorisierungstyp **Standard** oder **Keine** aus.  Bei Auswahl von **Standard** gehen Sie wie folgt vor:  
    &emsp;a. Geben Sie den Ambari-Benutzernamen ein. Der Standardwert lautet **Administrator**.  
    &emsp;b. Geben Sie das Ambari-Kennwort ein.

5. Überprüfen Sie die Ansicht **AUSGABE** auf eine Bestätigung.

## <a name="list-hdinsight-clusters"></a>Auflisten von HDInsight-Clustern

1. Navigieren Sie auf der Menüleiste zu **Ansicht** > **Befehlspalette...** , und geben Sie **HDInsight: List Cluster** (HDInsight: Cluster auflisten) ein.

2. Wählen Sie das gewünschte Abonnement aus.

3. Überprüfen Sie die Ansicht **AUSGABE**.  In der Ansicht werden Ihr verknüpfter Cluster und alle Cluster in Ihrem Azure-Abonnement angezeigt.

    ![Festlegen einer Standardclusterkonfiguration](./media/hdinsight-for-vscode/list-cluster-result.png)

## <a name="set-default-cluster"></a>Festlegen des Standardclusters

1. Öffnen Sie erneut den [zuvor](#open-hdinsight-work-folder) erstellten Ordner **HDexample**, falls er geschlossen wurde.  

2. Wählen Sie die [zuvor](#open-hdinsight-work-folder) erstellte Datei **HelloWorld.hql** aus. Sie wird im Skript-Editor geöffnet.

3. Klicken Sie dann mit der rechten Maustaste auf den Skript-Editor, und wählen Sie **HDInsight: Set Default Cluster** (HDInsight: Standardcluster festlegen) aus.  

4. [Stellen Sie eine Verbindung mit Ihrem Azure-Konto her](#connect-to-azure-account), oder verknüpfen Sie einen Cluster, sofern noch keine Verbindung besteht.

5. Wählen Sie einen Cluster als Standardcluster für die aktuelle Skriptdatei aus. Die Konfigurationsdatei **VSCode\settings.json** wird automatisch aktualisiert. 

   ![Festlegen der Standardclusterkonfiguration](./media/hdinsight-for-vscode/set-default-cluster-configuration.png)


## <a name="submit-interactive-hive-queries-hive-batch-scripts"></a>Übermitteln von interaktiven Hive-Abfragen und Hive-Batchskripts

Mit HDInsight Tools for Visual Studio Code können Sie interaktive Hive-Abfragen und Hive-Batchskripts an HDInsight-Cluster übermitteln.

1. Öffnen Sie erneut den [zuvor](#open-hdinsight-work-folder) erstellten Ordner **HDexample**, falls er geschlossen wurde.  

2. Wählen Sie die [zuvor](#open-hdinsight-work-folder) erstellte Datei **HelloWorld.hql** aus. Sie wird im Skript-Editor geöffnet.


3. Kopieren Sie den folgenden Code, fügen Sie ihn in Ihre Hive-Datei ein, und speichern Sie sie.

    ```hiveql
    SELECT * FROM hivesampletable;
    ```

4. [Stellen Sie eine Verbindung mit Ihrem Azure-Konto her](#connect-to-azure-account), oder verknüpfen Sie einen Cluster, sofern noch keine Verbindung besteht.

5. Klicken Sie mit der rechten Maustaste auf den Skript-Editor, und wählen Sie **HDInsight: Hive Interactive** aus, um die Abfrage zu übermitteln, oder verwenden Sie die Tastenkombination **STRG+ALT+I**.  Wählen Sie **HDInsight Hive Batch** aus, um das Skript zu übermitteln, oder verwenden Sie die Tastenkombination **STRG+ALT+H**.  

6. Wählen Sie den Cluster aus, wenn Sie keinen Standardcluster angegeben haben. Über das Kontextmenü können Sie anstelle der gesamten Skriptdatei auch einen Codeblock übermitteln. Nach kurzer Zeit werden die Abfrageergebnisse auf einer neuen Registerkarte angezeigt.

   ![Interactive Hive-Ergebnis](./media/hdinsight-for-vscode/interactive-hive-result.png)

    - Bereich **ERGEBNISSE**: Sie können das gesamte Ergebnis als CSV-, JSON- oder Excel-Datei in einem lokalen Pfad speichern oder einfach mehrere Zeilen auswählen.

    - Bereich **MELDUNGEN**: Wenn Sie die **Zeilennummer** auswählen, gelangen Sie zur ersten Zeile des derzeit ausgeführten Skripts.

## <a name="submit-interactive-pyspark-queries"></a>Übermitteln interaktiver PySpark-Abfragen

Anhand der folgenden Schritte können Sie interaktive PySpark-Abfragen übermitteln:

1. Öffnen Sie erneut den [zuvor](#open-hdinsight-work-folder) erstellten Ordner **HDexample**, falls er geschlossen wurde.  

2. Erstellen Sie anhand der [zuvor](#open-hdinsight-work-folder) erläuterten Schritte eine neue Datei vom Typ **HelloWorld.py**.

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

4. [Stellen Sie eine Verbindung mit Ihrem Azure-Konto her](#connect-to-azure-account), oder verknüpfen Sie einen Cluster, sofern noch keine Verbindung besteht.

5. Wählen Sie den gesamten Code aus, und klicken Sie mit der rechten Maustaste auf den Skript-Editor, und wählen Sie **HDInsight: PySpark Interactive** (HDInsight: PySpark (interaktiv)) aus, um die Abfrage zu übermitteln, oder verwenden Sie die Tastenkombination **STRG+ALT+I**.

   ![Rechtsklick PySpark Interactive](./media/hdinsight-for-vscode/pyspark-interactive-right-click.png)

6. Wählen Sie den Cluster aus, wenn Sie keinen Standardcluster angegeben haben. Nach kurzer Zeit werden die **interaktiven Python-Ergebnisse** auf einer neuen Registerkarte angezeigt. Über das Kontextmenü können Sie anstelle der gesamten Skriptdatei auch einen Codeblock übermitteln. 

   ![Fenster mit PySpark Interactive und Python Interactive](./media/hdinsight-for-vscode/pyspark-interactive-python-interactive-window.png) 

7. Geben Sie **"%% Info"** ein, und drücken Sie dann die **UMSCHALT + EINGABETASTE**, um Auftragsinformationen anzuzeigen. (Optional)

   ![Auftragsinformationen anzeigen](./media/hdinsight-for-vscode/pyspark-interactive-view-job-information.png)

8. Das Tool unterstützt auch die **Spark SQL-Abfrage**.

   ![PySpark Interactive Ergebnis anzeigen](./media/hdinsight-for-vscode/pyspark-ineteractive-select-result.png)

   Der Übertragungsstatus wird beim Ausführen von Abfragen links neben der unteren Statusleiste angezeigt. Übermitteln Sie keine weiteren Abfragen, wenn der Status **PySpark Kernel (busy)** (PySpark-Kernel (ausgelastet)) lautet.  

   > [!NOTE] 
   >
   > Wenn das Kontrollkästchen für **Python-Erweiterung aktiviert** nicht aktiviert ist (die Standardeinstellung ist aktiviert), werden die übermittelten Ergebnisse für die Pyspark-Interaktion im alten Fenster angezeigt.
   >
   > ![Python-Erweiterung für PySpark Interactive deaktiviert](./media/hdinsight-for-vscode/pyspark-interactive-python-extension-disabled.png)


## <a name="submit-pyspark-batch-job"></a>Übermitteln eines PySpark-Batchauftrags

1. Öffnen Sie erneut den [zuvor](#open-hdinsight-work-folder) erstellten Ordner **HDexample**, falls er geschlossen wurde.  

2. Erstellen Sie anhand der [zuvor](#open-hdinsight-work-folder) erläuterten Schritte eine neue Datei vom Typ **BatchFile.py**.

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

4. [Stellen Sie eine Verbindung mit Ihrem Azure-Konto her](#connect-to-azure-account), oder verknüpfen Sie einen Cluster, sofern noch keine Verbindung besteht.

5. Klicken Sie mit der rechten Maustaste auf den Skript-Editor, und wählen Sie anschließend **HDInsight: PySpark Batch** aus, oder verwenden Sie die Tastenkombination **STRG+ALT+H**. 

6. Wählen Sie einen Cluster aus, an den Sie Ihren PySpark-Auftrag übermitteln möchten. 

   ![Übermitteln des Ergebnisses des Python-Auftrags](./media/hdinsight-for-vscode/submit-pythonjob-result.png) 

Nach dem Übermitteln eines Python-Auftrags werden die Übermittlungsprotokolle in Visual Studio Code im Fenster **AUSGABE** angezeigt. Die **URL der Spark-Benutzeroberfläche** und die **URL der Yarn-Benutzeroberfläche** werden ebenfalls angezeigt. Sie können die URL in einem Webbrowser öffnen, um den Auftragsstatus nachzuverfolgen.

## <a name="apache-livy-configuration"></a>Apache Livy-Konfiguration

Die **Apache Livy**-Konfiguration wird unterstützt. Entsprechende Einstellungen können Sie unter [.VSCode\settings.json](https://livy.incubator.apache.org/) im Arbeitsbereichsordner vornehmen. Derzeit wird in der Livy-Konfiguration nur das Python-Skript unterstützt. Weitere Details finden Sie in der [Livy-INFODATEI](https://github.com/cloudera/livy/blob/master/README.rst ).

<a id="triggerlivyconf"></a>**Auslösen der Livy-Konfiguration**

Methode 1  
1. Navigieren Sie auf der Menüleiste zu **Datei** > **Einstellungen** > **Einstellungen**.  
2. Geben Sie in das Suchfeld **Sucheinstellungen** die Zeichenfolge **HDInsight Job Submission: Livy Conf** (HDInsight-Auftragsübermittlung: Livy-Konfiguration) ein.  
3. Wählen Sie für das relevante Suchergebnis **Edit in settings.json** (In „settings.json“ bearbeiten) aus.

Methode 2   
Übermitteln Sie eine Datei. Beachten Sie dabei, dass der Ordner „.vscode“ automatisch dem Arbeitsordner hinzugefügt wird. Die Livy-Konfiguration finden Sie, indem Sie auf **.vscode\settings.json** klicken.

+ Die Projekteinstellungen:

    ![Livy-Konfiguration](./media/hdinsight-for-vscode/hdi-livyconfig.png)

>[!NOTE]
>Legen Sie für die Einstellungen **driverMomory** und **executorMomry** den Wert mit Einheit fest, z.B. „1g“ oder „1024m“. 

+ Die unterstützten Livy-Konfigurationen:   

    **POST /batches**   
    Anforderungstext

    | name | description | type | 
    | :- | :- | :- | 
    | file | Die Datei, die die auszuführende Anwendung enthält. | Pfad (erforderlich) | 
    | proxyUser | Der Benutzer, dessen Identität bei Auftragsausführung gewechselt wird. | Zeichenfolge | 
    | className | Die Java-/Spark-Hauptklasse der Anwendung. | Zeichenfolge |
    | args | Die Befehlszeilenargumente, die an die Anwendung übergeben werden sollen. | Liste von Zeichenfolgen | 
    | jars | JAR-Dateien, die in dieser Sitzung verwendet werden. | Liste von Zeichenfolgen | 
    | pyFiles | Python-Dateien, die in dieser Sitzung verwendet werden. | Liste von Zeichenfolgen |
    | files | Dateien, die in dieser Sitzung verwendet werden. | Liste von Zeichenfolgen |
    | driverMemory | Die Menge an Arbeitsspeicher, der für den Treiberprozess verwendet wird. | Zeichenfolge |
    | driverCores | Die Anzahl von Kernen, die für den Treiberprozess verwendet wird. | int |
    | executorMemory | Die Menge an Arbeitsspeicher, die pro Executorprozess verwendet wird. | Zeichenfolge |
    | executorCores | Die Anzahl von Kernen, die für jeden Executor verwendet wird. | int |
    | numExecutors | Die Anzahl von Executors, die für diese Sitzung gestartet werden. | int |
    | archives | Die Archive, die in dieser Sitzung verwendet werden. | Liste von Zeichenfolgen |
    | queue | Der Name der YARN-Warteschlange, an den gesendet wird. | Zeichenfolge |
    | name | Der Name dieser Sitzung. | Zeichenfolge |
    | conf | Eigenschaften der Spark-Konfiguration. | Zuordnung von Schlüssel=Wert |

    Antworttext   
    Das erstellte Batchobjekt

    | name | description | type | 
    | :- | :- | :- | 
    | id | Die Sitzungs-ID | int | 
    | appId | Die Anwendungs-ID dieser Sitzung |  string |
    | appInfo | Die detaillierten Anwendungsinformationen | Zuordnung von Schlüssel=Wert |
    | log | Die Protokollzeilen | Liste von Zeichenfolgen |
    | state |   Der Batchstatus | Zeichenfolge |

>[!NOTE]
>Die zugewiesene Livy-Konfiguration wird beim Übermitteln des Skripts im Ausgabebereich angezeigt.

## <a name="integrate-with-azure-hdinsight-from-explorer"></a>Integration mit Azure HDInsight per Explorer

**Azure HDInsight** wurde der Explorer-Ansicht hinzugefügt. Sie können Ihre Cluster direkt über **Azure HDInsight** durchsuchen und verwalten.

1. [Stellen Sie eine Verbindung mit Ihrem Azure-Konto her](#connect-to-azure-account), oder verknüpfen Sie einen Cluster, sofern noch keine Verbindung besteht.

2. Navigieren Sie auf der Menüleiste zu **Ansicht** > **Explorer**.

3. Erweitern Sie im linken Bereich **AZURE HDINSIGHT**.  Die verfügbaren Abonnements und Cluster (Spark, Hadoop und HBase werden unterstützt) werden aufgeführt. 

   ![Azure HDInsight-Abonnement](./media/hdinsight-for-vscode/hdi-azure-hdinsight-subscription.png)

4. Erweitern Sie den Cluster, um die Hive-Metadatendatenbank und das Hive-Tabellenschema anzuzeigen.

   ![Azure HDInsight-Cluster](./media/hdinsight-for-vscode/hdi-azure-hdinsight-cluster.png)


## <a name="preview-hive-table"></a>Hive-Tabelle – Vorschau
Sie können eine Vorschau der Hive-Tabelle in Ihren Clustern direkt über den **Azure HDInsight**-Explorer anzeigen.
1. [Stellen Sie eine Verbindung mit Ihrem Azure-Konto her](#connect-to-azure-account), sofern noch keine Verbindung besteht.

2. Klicken Sie auf das **Azure**-Symbol in der äußerst linken Spalte.

3. Erweitern Sie im linken Bereich AZURE HDINSIGHT. Die verfügbaren Abonnements und Cluster werden aufgeführt.

4. Erweitern Sie den Cluster, um die Hive-Metadatendatenbank und das Hive-Tabellenschema anzuzeigen.

5. Klicken Sie mit der rechten Maustaste auf die Hive-Tabelle, z.B. „hivesampletable“. Wählen Sie **Vorschau** aus. 

   ![HDInsight für VS Code-Vorschau – Hive-Tabelle](./media/hdinsight-for-vscode/hdinsight-for-vscode-preview-hive-table.png)

6. Das Fenster **Ergebnisvorschau** wird geöffnet.

   ![HDInsight für VS Code-Vorschau – Ergebnisfenster](./media/hdinsight-for-vscode/hdinsight-for-vscode-preview-results-window.png)
   
- Bereich **ERGEBNISSE**

   Sie können das gesamte Ergebnis als CSV-, JSON- oder Excel-Datei in einem lokalen Pfad speichern oder einfach mehrere Zeilen auswählen.

- Bereich **MELDUNGEN**
   1. Wenn die Tabelle mehr als 100 Zeilen enthält, wird die folgende Meldung angezeigt: **Die ersten 100 Zeilen werden für die Hive-Tabelle angezeigt**.
   2. Wenn die Tabelle weniger oder maximal als 100 Zeilen enthält, wird die folgende Meldung angezeigt: **60 Zeilen werden für die Hive-Tabelle angezeigt**.
   3. Wenn die Tabelle leer ist, wird die folgende Meldung angezeigt: **0 Zeilen werden für die Hive-Tabelle angezeigt**.

>[!NOTE]
>
>Installieren Sie unter Linux Xclip, um das Kopieren von Tabellendaten zu aktivieren.
>
>![HDInsight für VS Code unter Linux](./media/hdinsight-for-vscode/hdinsight-for-vscode-preview-linux-install-xclip.png)
## <a name="additional-features"></a>Zusätzliche Funktionen

HDInsight für Visual Studio Code unterstützt folgende Features:

- **IntelliSense-AutoVervollständigen**. Zeigt Vorschläge für Schlüsselwörter, Methoden, Variablen und Ähnliches an. Die unterschiedlichen Objekttypen werden durch entsprechende Symbole dargestellt.

    ![IntelliSense-Objekttypen der HDInsight Tools für Visual Studio Code](./media/hdinsight-for-vscode/hdinsight-for-vscode-auto-complete-objects.png)
- **IntelliSense-Fehlermarker**. Der Sprachdienst unterstreicht die Bearbeitungsfehler für das Hive-Skript.     
- **Syntaxmarkierungen**. Der Sprachdienst verwendet verschiedene Farben, um Variablen, Schlüsselwörter, Datentypen, Funktionen und Ähnliches zu unterscheiden. 

    ![Syntaxmarkierungen der HDInsight Tools für Visual Studio Code](./media/hdinsight-for-vscode/hdinsight-for-vscode-syntax-highlights.png)

## <a name="reader-only-role"></a>Rolle „Nur Leser“

Benutzer mit Cluster-**Rolle** **Nur** **Leser** können keinen Auftrag mehr an den HDInsight-Cluster übermitteln oder die Hive-Datenbank anzeigen. Sie müssen sich an den Clusteradministrator wenden, um die Rolle im [ Azure-Portal](https://ms.portal.azure.com/) auf [**HDInsight**-**Cluster**-**Operator**](https://docs.microsoft.com/azure/hdinsight/hdinsight-migrate-granular-access-cluster-configurations#add-the-hdinsight-cluster-operator-role-assignment-to-a-user) zu aktualisieren. Wenn Sie die Ambari-Anmeldeinformationen kennen, können Sie den Cluster mithilfe der folgenden Anweisung manuell verknüpfen.

### <a name="browse-hdinsight-cluster"></a>Durchsuchen des Azure HDInsight-Clusters  

Wenn Sie für den Cluster die Rolle „Nur Leser“ haben und auf den Azure HDInsight-Explorer klicken, um einen HDInsight-Cluster zu erweitern, werden Sie aufgefordert, den Cluster zu verknüpfen. Führen Sie die folgenden Schritte aus, um den Cluster per Ambari-Anmeldeinformationen zu verknüpfen. 

### <a name="submit-job-to-hdinsight-cluster"></a>Übermitteln eines Auftrags an den HDInsight-Cluster

Wenn Sie für den Cluster die Rolle „Nur Leser“ haben und einen Auftrag an den HDInsight-Cluster übermitteln, werden Sie aufgefordert, den Cluster zu verknüpfen. Führen Sie die folgenden Schritte aus, um den Cluster per Ambari-Anmeldeinformationen zu verknüpfen. 

### <a name="link-to-cluster"></a>Verknüpfen des Clusters

1.  Geben Sie den Benutzernamen für Ambari ein. 
2.  Geben Sie das Ambari-Benutzerkennwort ein.

   ![HDInsight Tools for Visual Studio Code – Benutzername](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-username.png)

   ![HDInsight Tools for Visual Studio Code – Kennwort](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-password.png)

> [!NOTE]
>
>Sie können HDInsight verwenden: Auflisten des Clusters zum Überprüfen des verknüpften Clusters.
>
>![HDInsight Tools for Visual Studio Code – Leser verknüpft](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-reader-linked.png)

## <a name="azure-data-lake-storage-gen2-adls-gen2"></a>Azure Data Lake Storage Gen2 (ADLS Gen2)

### <a name="browse-an-adls-gen2-account"></a>Durchsuchen eines ADLS Gen2-Kontos

Wenn Sie auf den Azure HDInsight-Explorer klicken, um ein ADLS Gen2-Konto zu erweitern, werden Sie aufgefordert, den **Zugriffsschlüssel** für den Speicher einzugeben, wenn Ihr Azure-Konto keinen Zugriff auf den Gen2-Speicher hat. Das ADLS Gen2-Konto werden automatisch erweitert, sobald der Zugriffsschlüssel erfolgreich überprüft wurde. 

### <a name="submit-jobs-to-hdinsight-cluster-with-adls-gen2"></a>Übermitteln von Aufträgen an HDInsight-Cluster mit ADLS Gen2

Wenn Sie einen Auftrag an einen HDInsight-Cluster mit ADLS Gen2 übermitteln, werden Sie aufgefordert, den **Zugriffsschlüssel** für den Speicher einzugeben, wenn Ihr Azure-Konto keinen Schreibzugriff auf den Gen2-Speicher hat.  Der Auftrag wird erfolgreich übermittelt, sobald der Zugriffsschlüssel erfolgreich überprüft wurde. 

![HDInsight Tools for Visual Studio Code – Zugriffsschlüssel](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-accesskey.png)   

> [!NOTE]
> 
>Den Zugriffsschlüssel für das Speicherkonto erhalten Sie über das Azure-Portal. Weitere Informationen finden Sie unter [Anzeigen und Kopieren von Zugriffsschlüsseln](https://docs.microsoft.com/azure/storage/common/storage-account-manage#view-and-copy-access-keys).

## <a name="unlink-cluster"></a>Aufheben einer Clusterverknüpfung

1. Navigieren Sie auf der Menüleiste zu **Ansicht** > **Befehlspalette...** , und geben Sie dann **HDInsight: Unlink a Cluster** (HDInsight: Verknüpfung eines Clusters aufheben) ein.  

2. Wählen Sie den Cluster aus, dessen Verknüpfung aufgehoben werden soll.  

3. Überprüfen Sie die Ansicht **AUSGABE** auf eine Bestätigung.  

## <a name="sign-out"></a>Abmelden  

Navigieren Sie auf der Menüleiste zu **Ansicht** > **Befehlspalette...** , und geben Sie dann **HDInsight: Logout** (HDInsight: Abmelden) ein.  In der Ecke unten rechts wird ein Popupelement mit dem Hinweis **Logout successfully!** (Abmeldung erfolgreich!) angezeigt.


## <a name="next-steps"></a>Nächste Schritte
Ein Demovideo zur Verwendung von HDInsight für Visual Studio Code finden Sie unter [HDInsight Tools for VSCode](https://go.microsoft.com/fwlink/?linkid=858706) (HDInsight Tools for Visual Studio Code).
