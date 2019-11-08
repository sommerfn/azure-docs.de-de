---
title: 'Tutorial: Azure-Toolkit für IntelliJ: Spark-App: HDInsight'
description: 'Tutorial: Verwenden Sie das Azure Toolkit für IntelliJ, um in Scala geschriebene Spark-Anwendungen zu entwickeln und an einen HDInsight Spark-Cluster zu senden.'
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: tutorial
ms.date: 09/04/2019
ms.openlocfilehash: b2705f209b2acf1198ea555a5de2f79987a4d0e3
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73494246"
---
# <a name="tutorial-use-azure-toolkit-for-intellij-to-create-apache-spark-applications-for-hdinsight-cluster"></a>Tutorial: Erstellen von Apache Spark-Anwendungen für einen HDInsight-Cluster mit dem Azure-Toolkit für IntelliJ

Dieses Tutorial zeigt, wie Sie mit dem Plug-In „Azure-Toolkit für IntelliJ“ in [Scala](https://www.scala-lang.org/) geschriebene Apache Spark-Anwendungen entwickeln und dann direkt aus der IntelliJ-IDE (Integrated Development Environment, integrierte Entwicklungsumgebung) an einen HDInsight Spark-Cluster senden. Sie können das Plug-In auf mehrere Arten verwenden:

* Entwickeln und Übermitteln einer Scala Spark-Anwendung an einen HDInsight Spark-Cluster
* Zugreifen auf Ihre Azure HDInsight Spark-Clusterressourcen
* Entwickeln und lokales Ausführen einer Scala Spark-Anwendung

In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Verwenden des Plug-Ins „Azure-Toolkit für IntelliJ“
> * Entwickeln von Apache Spark-Anwendungen
> * Übermitteln der Anwendung an den Azure HDInsight-Cluster

## <a name="prerequisites"></a>Voraussetzungen

* Ein Apache Spark-Cluster unter HDInsight. Eine Anleitung finden Sie unter [Erstellen von Apache Spark-Clustern in Azure HDInsight](apache-spark-jupyter-spark-sql.md).

* [Oracle Java Development Kit.](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)  In diesem Tutorial wird die Java-Version 8.0.202 verwendet.

* IntelliJ IDEA. In diesem Artikel wird [IntelliJ IDEA Community  2018.3.4](https://www.jetbrains.com/idea/download/) verwendet.

* Azure-Toolkit für IntelliJ.  Weitere Informationen finden Sie unter [Installieren des Azure-Toolkits für IntelliJ](https://docs.microsoft.com/java/azure/intellij/azure-toolkit-for-intellij-installation?view=azure-java-stable).

## <a name="install-scala-plugin-for-intellij-idea"></a>Installieren des Scala-Plug-Ins für IntelliJ IDEA

Führen Sie die folgenden Schritte aus, um das Scala-Plug-In zu installieren:

1. Öffnen Sie IntelliJ IDEA.

2. Navigieren Sie auf der Willkommensseite zu **Konfigurieren** > **Plug-Ins**, um das Fenster **Plug-Ins** zu öffnen.

    ![IntelliJ IDEA: Aktivieren des Scala-Plug-Ins](./media/apache-spark-intellij-tool-plugin/enable-scala-plugin1.png)

3. Wählen Sie für das in dem neuen Fenster empfohlene Scala-Plug-In die Option **Installieren** aus.  

    ![IntelliJ IDEA: Installieren des Scala-Plug-Ins](./media/apache-spark-intellij-tool-plugin/install-scala-plugin.png)

4. Nach erfolgreicher Plug-In-Installation müssen Sie die IDE neu starten.

## <a name="create-a-spark-scala-application-for-an-hdinsight-spark-cluster"></a>Erstellen einer Spark Scala-Anwendung für einen HDInsight Spark-Cluster

1. Starten Sie IntelliJ IDEA, und wählen Sie **Create New Project** (Neues Projekt erstellen) aus, um das Fenster **New Project** (Neues Projekt) zu öffnen.

2. Wählen Sie im linken Bereich **Azure Spark/HDInsight** aus.

3. Wählen Sie im Hauptfenster **Spark Project (Scala)** (Spark-Projekt (Scala)) aus.

4. Wählen Sie in der Dropdownliste **Build tool** (Buildtool) eine der folgenden Optionen aus:
   * **Maven** für die Unterstützung des Scala-Projekterstellungs-Assistenten
   * **SBT** zum Verwalten von Abhängigkeiten und Erstellen für das Scala-Projekt

     ![IntelliJ IDEA: Dialogfeld „Neues Projekt“](./media/apache-spark-intellij-tool-plugin/create-hdi-scala-app.png)

5. Klicken Sie auf **Weiter**.

6. Geben Sie im Fenster **New Project** (Neues Projekt) die folgenden Informationen an:  

    |  Eigenschaft   | BESCHREIBUNG   |  
    | ----- | ----- |  
    |Projektname| Geben Sie einen Namen ein.  In diesem Tutorial wird `myApp` verwendet.|  
    |Project&nbsp;location (Projektspeicherort)| Geben Sie den gewünschten Speicherort für Ihr Projekt ein.|
    |Project SDK (Projekt-SDK)| Dieses Feld ist bei der erstmaligen Verwendung von IDEA möglicherweise leer.  Wählen Sie **New...** (Neu...) aus, und navigieren Sie zu Ihrem JDK.|
    |Spark-Version|Der Erstellungs-Assistent integriert die passende Version für das Spark-SDK und das Scala-SDK. Wenn die Spark-Clusterversion niedriger als 2.0 ist, wählen Sie **Spark 1.x** aus. Wählen Sie andernfalls **Spark 2.x** aus. In diesem Beispiel wird **Spark 2.3.0 (Scala 2.11.8)** verwendet.|

    ![Auswählen des Apache Spark SDK](./media/apache-spark-intellij-tool-plugin/intellij-new-project.png)

7. Wählen Sie **Fertig stellen** aus.  Es kann einige Minuten dauern, bis das Projekt verfügbar ist.

8. Das Spark-Projekt erstellt automatisch ein Artefakt für Sie. Um das Artefakt anzuzeigen, gehen Sie folgendermaßen vor:

   a. Navigieren Sie auf der Menüleiste zu **Datei** > **Projektstruktur**.

   b. Wählen Sie im Fenster **Projektstruktur** die Option **Artefakte** aus.  

   c. Wählen Sie **Abbrechen** aus, nachdem Sie das Artefakt angezeigt haben.

      ![Informationen zum Artefakt im Dialogfeld](./media/apache-spark-intellij-tool-plugin/default-artifact-dialog.png)

9. Fügen Sie den Quellcode der Anwendung wie folgt hinzu:

    a. Navigieren Sie in Project zu **myApp** > **src** > **main** > **scala**.  

    b. Klicken Sie mit der rechten Maustaste auf **scala**, und navigieren Sie dann zu **New** > **Scala Class** (Neu > Scala-Klasse).

   ![Befehle zum Erstellen einer Scala-Klasse über Project](./media/apache-spark-intellij-tool-plugin/hdi-spark-scala-code.png)

   c. Geben Sie im Dialogfeld **Create New Scala Class** (Neue Scala-Klasse erstellen) einen Namen ein. Wählen Sie im Dropdownfeld **Kind** (Art) die Option **Object** aus, und wählen Sie dann **OK** aus.

     ![Dialogfeld „Create New Scala Class“ (Neue Scala-Klasse erstellen)](./media/apache-spark-intellij-tool-plugin/hdi-spark-scala-code-object.png)

   d. Die Datei **myApp.scala** wird dann in der Hauptansicht geöffnet. Ersetzen Sie den Standardcode durch den folgenden Code:  

        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
    
        object myApp{
            def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("myApp")
            val sc = new SparkContext(conf)
    
            val rdd = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
    
            //find the rows that have only one digit in the seventh column in the CSV file
            val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)
    
            rdd1.saveAsTextFile("wasbs:///HVACOut")
            }
    
        }

    Der Code liest die Daten aus der Datei „HVAC.csv“ (die für alle HDInsight Spark-Cluster verfügbar ist), ruft die Zeilen ab, die nur eine Ziffer in der siebten Spalte der CSV-Datei enthalten, und schreibt die Ausgabe in `/HVACOut` unter dem Standardspeichercontainer für den Cluster.

## <a name="connect-to-your-hdinsight-cluster"></a>Herstellen einer Verbindung mit Ihrem HDInsight-Cluster
Der Benutzer kann sich entweder [beim Azure-Abonnement anmelden](#sign-in-to-your-azure-subscription) oder [einen HDInsight-Cluster verknüpfen](#link-a-cluster). Zum Herstellen einer Verbindung kann er entweder eine Kombination aus Ambari-Benutzername und -Kennwort oder Anmeldeinformationen verwenden, die in eine Domäne eingebunden sind.

### <a name="sign-in-to-your-azure-subscription"></a>Melden Sie sich bei Ihrem Azure-Abonnement an.

1. Navigieren Sie in der Menüleiste zu **Ansicht** > **Toolfenster** > **Azure Explorer**.

   ![IntelliJ IDEA: Anzeigen des Azure-Explorers](./media/apache-spark-intellij-tool-plugin/show-azure-explorer1.png)

2. Klicken Sie im Azure Explorer mit der rechten Maustaste auf den Knoten **Azure**, und wählen Sie dann **Anmelden** aus.

   ![IntelliJ IDEA-Explorer: Rechtsklick auf Azure](./media/apache-spark-intellij-tool-plugin/explorer-rightclick-azure.png)

3. Wählen Sie im Dialogfeld **Azure Sign In** (Azure-Anmeldung) die Option **Device Login** (Geräteanmeldung) aus, und klicken Sie dann auf **Sign in** (Anmelden).

    ![IntelliJ IDEA: Azure-Anmeldung > Geräteanmeldung](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer2.png)

4. Klicken Sie im Dialogfeld **Azure Device Login** (Azure-Geräteanmeldung) auf **Copy&Open** (Kopieren und öffnen).

   ![IntelliJ IDEA: Azure-Geräteanmeldung](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer5.png)

5. Fügen Sie den Code auf der Benutzeroberfläche des Browsers ein, und klicken Sie auf **Next** (Weiter).

   ![Microsoft-Dialogfeld zur Eingabe des Codes für HDI](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer6.png)

6. Geben Sie Ihre Azure-Anmeldeinformationen ein, und schließen Sie den Browser.

   ![Microsoft-Dialogfeld zur Eingabe der E-Mail für HDI](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer7.png)

7. Nachdem Sie sich angemeldet haben, werden im Dialogfeld **Abonnements auswählen** alle Azure-Abonnements aufgelistet, die den Anmeldeinformationen zugeordnet sind. Wählen Sie das Abonnement aus, und klicken Sie dann auf die Schaltfläche **Auswählen**.

    ![Dialogfeld zum Auswählen von Abonnements](./media/apache-spark-intellij-tool-plugin/Select-Subscriptions.png)

8. Erweitern Sie im **Azure Explorer** die Option **HDInsight**, um die HDInsight Spark-Cluster anzuzeigen, die sich in Ihren Abonnements befinden.

    ![IntelliJ IDEA: Hauptansicht im Azure-Explorer](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer3.png)

9. Um die dem Cluster zugeordneten Ressourcen (z.B. Speicherkonten) anzuzeigen, können Sie einen Clusternamenknoten noch einmal erweitern.

    ![Azure-Explorer: Speicherkonten](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer4.png)

### <a name="link-a-cluster"></a>Verknüpfen eines Clusters

Sie können einen HDInsight-Cluster mithilfe des verwalteten Apache Ambari-Benutzernamens verknüpfen. In ähnlicher Weise können Sie einen in eine Domäne eingebundenen HDInsight-Cluster unter Verwendung von Domäne und Benutzername verknüpfen, wie etwa `user1@contoso.com`. Sie können auch einen Livy-Dienstcluster verknüpfen.

1. Navigieren Sie in der Menüleiste zu **Ansicht** > **Toolfenster** > **Azure Explorer**.

1. Klicken Sie im Azure Explorer mit der rechten Maustaste auf den Knoten **HDInsight**, und wählen Sie dann **Cluster verknüpfen** aus.

   ![Azure-Explorer: Kontextmenü „Cluster verknüpfen“](./media/apache-spark-intellij-tool-plugin/link-a-cluster-context-menu.png)

1. Die verfügbaren Optionen im Fenster **Cluster verknüpfen** variieren je nach dem Wert, den Sie in der Dropdownliste **Ressourcentyp verknüpfen** auswählen.  Geben Sie Ihre Werte ein, und klicken Sie anschließend auf **OK**.

    * **HDInsight-Cluster**  
  
        |Eigenschaft |Wert |
        |----|----|
        |Ressourcentyp verknüpfen|Wählen Sie **HDInsight-Cluster** aus der Dropdownliste aus.|
        |Clustername/-URL| Geben Sie einen Clusternamen ein.|
        |Authentifizierungstyp| Lassen Sie die **Standardauthentifizierung** festgelegt.|
        |Benutzername| Geben Sie den Clusterbenutzernamen ein. Der Standardwert lautet „admin“.|
        |Kennwort| Geben Sie das Kennwort für den Benutzernamen ein.|

        ![IntelliJ IDEA-Dialogfeld: Cluster verknüpfen](./media/apache-spark-intellij-tool-plugin/link-hdinsight-cluster-dialog.png)

    * **Livy Service**  
  
        |Eigenschaft |Wert |
        |----|----|
        |Ressourcentyp verknüpfen|Wählen Sie **Livy Service** aus der Dropdownliste aus.|
        |Livy-Endpunkt| Livy-Endpunkt eingeben|
        |Clustername| Geben Sie einen Clusternamen ein.|
        |Yarn-Endpunkt|Optional.|
        |Authentifizierungstyp| Lassen Sie die **Standardauthentifizierung** festgelegt.|
        |Benutzername| Geben Sie den Clusterbenutzernamen ein. Der Standardwert lautet „admin“.|
        |Kennwort| Geben Sie das Kennwort für den Benutzernamen ein.|

        ![IntelliJ IDEA-Dialogfeld: Livy-Cluster verknüpfen](./media/apache-spark-intellij-tool-plugin/link-livy-cluster-dialog.png)

1. Sie finden Ihren verknüpften Cluster im Knoten **HDInsight**.

   ![Azure-Explorer: verknüpfter Cluster1](./media/apache-spark-intellij-tool-plugin/hdinsight-linked-cluster.png)

1. Sie können die Verknüpfung eines Clusters im **Azure-Explorer** auch aufheben.

   ![Azure-Explorer: nicht verknüpfter Cluster](./media/apache-spark-intellij-tool-plugin/hdi-unlinked-cluster.png)

## <a name="run-a-spark-scala-application-on-an-hdinsight-spark-cluster"></a>Ausführen einer Spark Scala-Anwendung in einem HDInsight Spark-Cluster

Nachdem Sie eine Scala-Anwendung erstellt haben, können Sie diese an den Cluster übermitteln.

1. Wechseln Sie in Project zu **myApp** > **src** > **main** > **scala** > **myApp**.  Klicken Sie mit der rechten Maustaste auf **myApp**, und wählen Sie **Submit Spark Application** (Spark-Anwendung übermitteln, wahrscheinlich am Listenende).

      ![Befehl „Submit Spark Application to HDInsight“](./media/apache-spark-intellij-tool-plugin/hdi-submit-spark-app-1.png)

2. Wählen Sie im Dialogfeld **Submit Spark Application** (Spark-Anwendung übermitteln) die Option **1. Spark für HDInsight** aus.

3. Geben Sie im Fenster **Edit configuration** (Konfiguration bearbeiten) die folgenden Werte an, und klicken Sie dann auf **OK**:

    |Eigenschaft |Wert |
    |----|----|
    |Spark-Cluster (nur Linux)|Wählen Sie den HDInsight Spark-Cluster aus, auf dem Sie Ihre Anwendung ausführen möchten.|
    |Select an Artifact to submit (Auswahl eines zu übermittelnden Artefakts)|Behalten Sie die Standardeinstellung bei.|
    |Main class name (Name der Hauptklasse)|Der Standardwert ist die Hauptklasse aus der ausgewählten Datei. Sie können die Klasse ändern, indem Sie auf die Auslassungspunkte ( **...** ) klicken und eine andere Klasse auswählen.|
    |Job configurations (Auftragskonfigurationen)|Sie können die Standardschlüssel und/oder Werte ändern. Weitere Informationen finden Sie unter [Apache Livy-REST-API](https://livy.incubator.apache.org./docs/latest/rest-api.html).|
    |Befehlszeilenargumente|Sie können bei Bedarf durch Leerzeichen getrennte Argumente für die Hauptklasse eingeben.|
    |Referenced Jars and Referenced Files („Referenzierte JARs“ und „Referenzierte Dateien“)|Sie können die Pfade für die referenzierten JARs und Dateien (sofern vorhanden) eingeben. Sie können auch Dateien im virtuellen Dateisystem von Azure durchsuchen, das derzeit nur ADLS Gen 2-Cluster unterstützt. Weitere Informationen finden Sie unter: [Apache Spark Configuration](https://spark.apache.org/docs/latest/configuration.html#runtime-environment) (Apache Spark-Konfiguration).  Lesen Sie auch die Informationen zum [Hochladen von Ressourcen in einen Cluster](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-storage-explorer).|
    |Job Upload Storage (Speicher für Auftragsupload)|Erweitern Sie die Option, um zusätzliche Optionen anzuzeigen.|
    |Speichertyp|Wählen Sie **Use Azure Blob to upload** (Azure-Blob für Upload verwenden) aus der Dropdownliste aus.|
    |Speicherkonto|Geben Sie Ihr Speicherkonto ein.|
    |Storage Key (Speicherschlüssel)|Geben Sie Ihren Speicherschlüssel ein.|
    |Speichercontainer|Wählen Sie Ihren Speichercontainer aus der Dropdownliste aus, nachdem Sie **Storage Account** (Speicherkonto) und **Storage Key** (Speicherschlüssel) eingegeben haben.|

    ![Dialogfeld für die Spark-Übermittlung](./media/apache-spark-intellij-tool-plugin/hdi-submit-spark-app-02.png)

4. Klicken Sie auf **SparkJobRun**, um das Projekt an den ausgewählten Cluster zu übermitteln. Auf der Registerkarte **Remote Spark Job in Cluster** (Spark-Remoteauftrag im Cluster) wird unten der Status der Auftragsausführung angezeigt. Sie können die Anwendung beenden, indem Sie auf die rote Schaltfläche klicken. Weiter unten in diesem Artikel im Abschnitt „Zugreifen auf und Verwalten von HDInsight Spark-Clustern mit dem Azure-Toolkit für IntelliJ“ wird beschrieben, wie Sie auf die Auftragsausgabe zugreifen.  

    ![Fenster für die Apache Spark-Übermittlung](./media/apache-spark-intellij-tool-plugin/hdi-spark-app-result.png)

## <a name="debug-apache-spark-applications-locally-or-remotely-on-an-hdinsight-cluster"></a>Lokales oder Remotedebuggen von Apache Spark-Anwendungen in einem HDInsight-Cluster

Es wird noch eine andere Möglichkeit der Übermittlung von Spark Anwendungen an den Cluster empfohlen. Dazu werden die Parameter in der IDE **Run/Debug Configurations** (Ausführen/Debugkonfigurationen) festgelegt. Weitere Informationen finden Sie unter [Lokales oder Remotedebuggen von Apache Spark-Anwendungen in einem HDInsight-Cluster mit dem Azure-Toolkit für IntelliJ über SSH](apache-spark-intellij-tool-debug-remotely-through-ssh.md).

## <a name="access-and-manage-hdinsight-spark-clusters-by-using-azure-toolkit-for-intellij"></a>Zugreifen auf und Verwalten von HDInsight Spark-Clustern mit dem Azure-Toolkit für IntelliJ

Sie können mit dem Azure-Toolkit für IntelliJ verschiedene Vorgänge durchführen.  Die meisten Vorgänge werden über den **Azure Explorer** gestartet.  Navigieren Sie in der Menüleiste zu **Ansicht** > **Toolfenster** > **Azure Explorer**.

### <a name="access-the-job-view"></a>Zugreifen auf die Auftragsansicht

1. Navigieren Sie im Azure Explorer zu **HDInsight** > \<Ihr Cluster > **Aufträge**.

    ![IntelliJ: Azure-Explorer – Knoten „Auftragsansicht“](./media/apache-spark-intellij-tool-plugin/intellij-job-view-node.png)

2. Im rechten Bereich werden auf der Registerkarte **Spark Job View** (Spark-Auftragsansicht) alle Anwendungen angezeigt, die in dem Cluster ausgeführt wurden. Wählen Sie den Namen der Anwendung aus, zu der Sie weitere Details anzeigen möchten.

    ![Spark-Auftrag: Anzeigen von Anwendungsdetails](./media/apache-spark-intellij-tool-plugin/intellij-view-job-logs.png)

3. Wenn Sie grundlegende Informationen zum ausgeführten Auftrag anzeigen möchten, zeigen Sie auf das Auftragsdiagramm. Um das Phasendiagramm und von den einzelnen Aufträgen generierte Informationen anzuzeigen, wählen Sie einen Knoten im Auftragsdiagramm aus.

    ![Spark-Auftrag: Anzeigen von Details zur Auftragsphase](./media/apache-spark-intellij-tool-plugin/Job-graph-stage-info.png)

4. Um häufig verwendete Protokolle anzuzeigen, z.B. *Driver Stderr*, *Driver Stdout* oder *Directory Info*, wählen Sie die Registerkarte **Log** (Protokoll) aus.

    ![Spark-Auftrag: Anzeigen von Protokolldetails](./media/apache-spark-intellij-tool-plugin/intellij-job-log-info.png)

5. Sie können auch die Spark History UI (Spark-Verlaufsbenutzeroberfläche) und die YARN UI (YARN-Benutzeroberfläche) (auf Anwendungsebene) anzeigen, indem Sie im oberen Bereich des Fensters einen Link auswählen.

### <a name="access-the-spark-history-server"></a>Zugreifen auf den Spark-Verlaufsserver

1. Erweitern Sie im Azure Explorer die Option **HDInsight**, klicken Sie mit der rechten Maustaste auf den Namen Ihres Spark-Clusters, und wählen Sie dann **Open Spark History UI** (Spark-Verlaufsbenutzeroberfläche öffnen).  
2. Wenn Sie dazu aufgefordert werden, geben Sie die Administratoranmeldeinformationen für den Cluster ein, die Sie beim Einrichten einer Verbindung mit dem Cluster angegeben haben.

3. Im Dashboard des Spark-Verlaufsservers können Sie anhand des Anwendungsnamens nach der Anwendung suchen, deren Ausführung Sie gerade beendet haben. Im obigen Code legen Sie den Namen der Anwendung mit `val conf = new SparkConf().setAppName("myApp")` fest. Daher lautet der Name Ihrer Spark-Anwendung **myApp**.

### <a name="start-the-ambari-portal"></a>Starten des Ambari-Portals

1. Erweitern Sie im Azure Explorer die Option **HDInsight**, klicken Sie mit der rechten Maustaste auf den Namen Ihres Spark-Clusters, und wählen Sie dann **Open Cluster Management Portal (Ambari)** (Clusterverwaltungsportal (Ambari) öffnen).  

2. Geben Sie die Anmeldeinformationen für den Cluster ein, wenn Sie dazu aufgefordert werden. Sie haben diese Anmeldeinformationen während der Einrichtung des Clusters angegeben.

### <a name="manage-azure-subscriptions"></a>Verwalten von Azure-Abonnements

Standardmäßig werden mit dem Azure-Toolkit für IntelliJ die Spark-Cluster Ihrer gesamten Azure-Abonnements aufgelistet. Bei Bedarf können Sie die Abonnements angeben, auf die Sie zugreifen möchten.  

1. Klicken Sie im Azure Explorer mit der rechten Maustaste auf den Hauptknoten **Azure**, und wählen Sie dann **Abonnements auswählen** aus.  

2. Deaktivieren Sie im Fenster **Abonnements auswählen** die Kontrollkästchen neben den Abonnements, auf die Sie nicht zugreifen möchten, und wählen Sie dann **Schließen** aus.

## <a name="spark-console"></a>Spark-Konsole

Sie können eine lokale Spark-Konsole (Scala) oder eine Spark-Konsole mit interaktiver Livy-Sitzung (Scala) ausführen.

### <a name="spark-local-consolescala"></a>Lokale Spark-Konsole (Scala)

Stellen Sie sicher, dass die WINUTILS.EXE-Voraussetzung erfüllt ist.

1. Navigieren Sie in der Menüleiste zu **Run** > **Edit Configurations** (Ausführen > Konfigurationen bearbeiten).

2. Navigieren Sie im Fenster **Run/Debug Configurations** (Konfigurationen ausführen/debuggen) im linken Bereich zu **Apache Spark on HDInsight** >  **[Spark on HDInsight] myApp**.

3. Wählen Sie im Hauptfenster die Registerkarte **Locally Run** (Lokale Ausführung) aus.

4. Geben Sie die folgenden Werte an, und wählen Sie anschließend **OK** aus:

    |Eigenschaft |Wert |
    |----|----|
    |Job main class (Hauptklasse des Auftrags)|Der Standardwert ist die Hauptklasse aus der ausgewählten Datei. Sie können die Klasse ändern, indem Sie auf die Auslassungspunkte ( **...** ) klicken und eine andere Klasse auswählen.|
    |Umgebungsvariablen|Stellen Sie sicher, dass der Wert für HADOOP_HOME richtig ist.|
    |WINUTILS.exe location (Speicherort von „WINUTILS.exe“)|Stellen Sie sicher, dass der Pfad richtig ist.|

    ![Lokale Konsole – Festlegen der Konfiguration](./media/apache-spark-intellij-tool-plugin/console-set-configuration.png)

5. Wechseln Sie in Project zu **myApp** > **src** > **main** > **scala** > **myApp**.  

6. Navigieren Sie in der Menüleiste zu **Tools** > **Spark Console** > **Run Spark Local Console(Scala)** (Extras > Spark-Konsole > Lokale Spark-Konsole ausführen (Scala)).

7. Möglicherweise werden zwei Dialogfelder angezeigt, in denen Sie gefragt werden, ob Sie die automatische Korrektur für die Abhängigkeiten durchführen möchten. Wenn dies der Fall ist, wählen Sie **Auto Fix** (Automatisch korrigieren) aus.

    ![IntelliJ IDEA: Spark-Dialogfeld1 „Auto Fix“ (Automatisch korrigieren)](./media/apache-spark-intellij-tool-plugin/intellij-console-autofix1.png)

    ![IntelliJ IDEA: Spark-Dialogfeld2 „Auto Fix“ (Automatisch korrigieren)](./media/apache-spark-intellij-tool-plugin/intellij-console-autofix2.png)

8. Die Konsole sollte der folgenden Abbildung ähneln. Geben Sie im Konsolenfenster `sc.appName` ein, und drücken Sie STRG + EINGABETASTE.  Das Ergebnis wird angezeigt. Sie können die lokale Konsole beenden, indem Sie auf die rote Schaltfläche klicken.

    ![IntelliJ IDEA: Ergebnis der lokalen Konsole](./media/apache-spark-intellij-tool-plugin/local-console-result.png)

### <a name="spark-livy-interactive-session-consolescala"></a>Spark-Konsole mit interaktiver Livy-Sitzung (Scala)

Dies wird nur für IntelliJ 2018.2 und 2018.3 unterstützt.

1. Navigieren Sie in der Menüleiste zu **Run** > **Edit Configurations** (Ausführen > Konfigurationen bearbeiten).

2. Navigieren Sie im Fenster **Run/Debug Configurations** (Konfigurationen ausführen/debuggen) im linken Bereich zu **Apache Spark on HDInsight** >  **[Spark on HDInsight] myApp**.

3. Wählen Sie im Hauptfenster die Registerkarte **Remotely Run in Cluster** (Im Cluster remote ausführen) aus.

4. Geben Sie die folgenden Werte an, und wählen Sie anschließend **OK** aus:

    |Eigenschaft |Wert |
    |----|----|
    |Spark-Cluster (nur Linux)|Wählen Sie den HDInsight Spark-Cluster aus, auf dem Sie Ihre Anwendung ausführen möchten.|
    |Main class name (Name der Hauptklasse)|Der Standardwert ist die Hauptklasse aus der ausgewählten Datei. Sie können die Klasse ändern, indem Sie auf die Auslassungspunkte ( **...** ) klicken und eine andere Klasse auswählen.|

    ![Interaktive Konsole – Festlegen der Konfiguration](./media/apache-spark-intellij-tool-plugin/interactive-console-configuration.png)

5. Wechseln Sie in Project zu **myApp** > **src** > **main** > **scala** > **myApp**.  

6. Navigieren Sie in der Menüleiste zu **Tools** > **Spark Console** > **Run Spark Livy Interactive Session Console (Scala)** (Extras > Spark-Konsole > Interaktive Spark Livy-Sitzungskonsole ausführen (Scala)) aus.

7. Die Konsole sollte der folgenden Abbildung ähneln. Geben Sie im Konsolenfenster `sc.appName` ein, und drücken Sie STRG + EINGABETASTE.  Das Ergebnis wird angezeigt. Sie können die lokale Konsole beenden, indem Sie auf die rote Schaltfläche klicken.

    ![IntelliJ IDEA: Ergebnis der interaktiven Konsole](./media/apache-spark-intellij-tool-plugin/interactive-console-result.png)

### <a name="send-selection-to-spark-console"></a>Senden einer Auswahl an die Spark-Konsole

Sie können das Skriptergebnis vorab erhalten, indem Sie einfach Code an die lokale Konsole oder die interaktive Livy-Sitzungskonsole (Scala) senden. Sie können Code in der Scala-Datei hervorheben und dann mit der rechten Maustaste auf **Send Selection To Spark Console** (Auswahl an Spark-Konsole senden) klicken. Der ausgewählte Code wird an die Konsole gesendet und ausgeführt. Das Ergebnis wird in der Konsole jeweils hinter dem Code angezeigt. Falls vorhanden, führt die Konsole eine Überprüfung der Fehler durch.  

   ![Senden einer Auswahl an die Spark-Konsole](./media/apache-spark-intellij-tool-plugin/send-selection-to-console.png)

## <a name="reader-only-role"></a>Rolle nur mit Leseberechtigung

Wenn Benutzer, die über eine Rolle nur mit Leseberechtigung verfügen, einen Auftrag an einen Cluster übermitteln, sind Ambari-Anmeldeinformationen erforderlich.

### <a name="link-cluster-from-context-menu"></a>Kontextmenü zum Verknüpfen eines Clusters

1. Melden Sie sich mit einem Konto an, das über eine Rolle nur mit Leseberechtigung verfügt.

2. Erweitern Sie im **Azure Explorer** die Option **HDInsight**, um die HDInsight-Cluster anzuzeigen, die sich in Ihrem Abonnement befinden. Cluster mit der Kennzeichnung **"Role: Reader"** verfügen über eine Rolle nur mit Leseberechtigung.

    ![IntelliJ: Azure-Explorer – Rolle: Leser](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer15.png)

3. Klicken Sie mit der rechten Maustaste auf den Cluster, der über eine Rolle nur mit Leseberechtigung verfügt. Wählen Sie aus dem Kontextmenü den Eintrag **Link this cluster** (Diesen Cluster verknüpfen) aus, um den Cluster zu verknüpfen. Geben Sie den Benutzernamen und das Kennwort für Ambari ein.

    ![IntelliJ: Azure-Explorer – Link this cluster (Diesen Cluster verknüpfen)](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer11.png)

4. Wenn der Cluster erfolgreich verknüpft wurde, wird HDInsight aktualisiert.
   Der Status des Clusters ändert sich zu „Linked“ (Verknüpft).
  
    ![IntelliJ: Azure-Explorer – Verknüpft](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer8.png)

### <a name="link-cluster-by-expanding-jobs-node"></a>Verknüpfen eines Clusters durch Erweitern des Knotens „Jobs“ (Aufträge)

1. Klicken Sie auf den Knoten **Jobs** (Aufträge). Das Fenster **Cluster Job Access Denied** (Zugriff auf Clusterauftrag verweigert) wird angezeigt.

2. Klicken Sie auf **Link this cluster** (Diesen Cluster verknüpfen), um den Cluster zu verknüpfen.

    ![Dialogfeld „Cluster Job Access Denied“ (Zugriff auf Clusterauftrag verweigert)](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer9.png)

### <a name="link-cluster-from-rundebug-configurations-window"></a>Verknüpfen eines Clusters über das Fenster „Run/Debug Configurations“ (Konfigurationen ausführen/debuggen)

1. Erstellen Sie eine HDInsight-Konfiguration. Wählen Sie dann **Remotely Run in Cluster** (Im Cluster remote ausführen) aus.

2. Wählen Sie unter **Spark clusters (Linux only)** (Spark-Cluster (nur Linux)) einen Cluster aus, der über eine Rolle nur mit Leseberechtigung verfügt. Es wird eine Warnmeldung angezeigt. Sie können auf **Link this cluster** (Diesen Cluster verknüpfen) klicken, um den Cluster zu verknüpfen.

   ![IntelliJ IDEA: Ausführen/Debuggen der Konfigurationserstellung](./media/apache-spark-intellij-tool-plugin/create-configuration.png)

### <a name="view-storage-accounts"></a>Anzeigen von Speicherkonten

* Klicken Sie in einem Cluster, der über eine Rolle nur mit Leseberechtigung verfügt, auf den Knoten **Storage Accounts** (Speicherkonten). Das Fenster **Storage Access Denied** (Zugriff auf Speicher verweigert) wird angezeigt. Sie können auf **Open Azure Storage Explorer** (Azure Storage-Explorer öffnen) klicken, um den Storage-Explorer zu öffnen.

   ![IntelliJ IDEA: Speicherzugriff verweigert](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer14.png)

   ![IntelliJ IDEA-Schaltfläche: Speicherzugriff verweigert](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer10.png)

* Klicken Sie in einem verknüpften Cluster auf den Knoten **Storage Accounts** (Speicherkonten). Das Fenster **Storage Access Denied** (Zugriff auf Speicher verweigert) wird angezeigt. Sie können auf **Open Azure Storage** (Azure Storage öffnen) klicken, um den Storage-Explorer zu öffnen.

   ![IntelliJ IDEA: Speicherzugriff verweigert2](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer13.png)

   ![IntelliJ IDEA-Schaltfläche: Speicherzugriff verweigert2](./media/apache-spark-intellij-tool-plugin/intellij-view-explorer12.png)

## <a name="convert-existing-intellij-idea-applications-to-use-azure-toolkit-for-intellij"></a>Konvertieren vorhandener IntelliJ IDEA-Anwendungen für die Verwendung des Azure-Toolkits für IntelliJ

Sie können die vorhandenen Spark Scala-Anwendungen, die Sie in IntelliJ IDEA erstellt haben, so umwandeln, dass sie mit dem Azure-Toolkit für IntelliJ kompatibel sind. Anschließend können Sie das Plug-In verwenden, um die Anwendungen an einen HDInsight Spark-Cluster zu übermitteln.

1. Öffnen Sie die zugehörige IML-Datei einer vorhandenen Spark Scala-Anwendung, die mit IntelliJ IDEA erstellt wurde.

2. Auf der Stammebene befindet sich ein **module**-Element wie das folgende:

        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4">

   Fügen Sie dem Element `UniqueKey="HDInsightTool"` hinzu, damit das **module** -Element wie folgt aussieht:

        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4" UniqueKey="HDInsightTool">

3. Speichern Sie die Änderungen. Ihre Anwendung sollte jetzt mit dem Azure-Toolkit für IntelliJ kompatibel sein. Sie können dies testen, indem Sie in Project mit der rechten Maustaste auf den Projektnamen klicken. Im Popupmenü ist jetzt die Option **Submit Spark Application to HDInsight** (Spark-Anwendung an HDInsight übermitteln) verfügbar.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie diese Anwendung nicht mehr benötigen, gehen Sie wie folgt vor, um den erstellten Cluster zu löschen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. Geben Sie oben im **Suchfeld** den Suchbegriff **HDInsight** ein.

1. Wählen Sie unter **Dienste** die Option **HDInsight-Cluster** aus.

1. Klicken Sie in der daraufhin angezeigten Liste mit den HDInsight-Clustern neben dem Cluster, den Sie für dieses Tutorial erstellt haben, auf die Auslassungspunkte ( **...** ).

1. Klicken Sie auf **Löschen**. Wählen Sie **Ja** aus.

![Azure-Portal: Löschen eines HDInsight-Clusters](./media/apache-spark-intellij-tool-plugin/hdinsight-azure-portal-delete-cluster.png "Löschen eines HDInsight-Clusters")

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie mit dem Plug-In „Azure-Toolkit für IntelliJ“ in [Scala](https://www.scala-lang.org/) geschriebene Apache Spark-Anwendungen entwickeln und dann direkt aus der IntelliJ-IDE (Integrated Development Environment, integrierte Entwicklungsumgebung) an einen HDInsight Spark-Cluster senden. Fahren Sie mit dem nächsten Artikel fort, um zu erfahren, wie die in Apache Spark registrierten Daten in ein BI-Analyse-Tool wie Power BI gezogen werden können.

> [!div class="nextstepaction"]
> [Analysieren von Daten mithilfe von BI-Tools](apache-spark-use-bi-tools.md)
