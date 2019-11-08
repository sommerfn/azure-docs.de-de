---
title: 'Azure-Toolkit für Eclipse: Erstellen von Scala-Apps für HDInsight Spark'
description: Verwenden Sie die HDInsight Tools im Azure-Toolkit für Eclipse, um in Scala geschriebene Spark-Anwendungen zu entwickeln und direkt in der integrierten Eclipse-Entwicklungsumgebung (IDE) an einen HDInsight Spark-Cluster zu übermitteln.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/30/2017
ms.openlocfilehash: 7e320b4a0166ffad3ba62c80ad7667c626acb136
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73494682"
---
# <a name="use-azure-toolkit-for-eclipse-to-create-apache-spark-applications-for-an-hdinsight-cluster"></a>Erstellen von Apache Spark-Anwendungen für HDInsight-Cluster mit dem Azure-Toolkit für Eclipse

Verwenden Sie die HDInsight-Tools im Azure-Toolkit für [Eclipse](https://www.eclipse.org/), um in [Scala](https://www.scala-lang.org/) geschriebene [Apache Spark](https://spark.apache.org/)-Anwendungen zu entwickeln und diese direkt aus der Eclipse-IDE an einen Azure HDInsight Spark-Cluster zu senden. Sie können das HDInsight-Tools-Plug-In auf verschiedene Weise verwenden:

* Zum Entwickeln und Übermitteln einer Scala Spark-Anwendung an einen HDInsight Spark-Cluster
* Zum Zugreifen auf Ihre Azure HDInsight Spark-Clusterressourcen
* Zum Entwickeln und lokalen Ausführen einer Scala Spark-Anwendung

> [!IMPORTANT]  
> Sie können dieses Tool nur zum Erstellen und Übermitteln von Anwendungen für einen HDInsight Spark-Cluster unter Linux verwenden.

## <a name="prerequisites"></a>Voraussetzungen

* Apache Spark-Cluster in HDInsight. Eine Anleitung finden Sie unter [Erstellen von Apache Spark-Clustern in Azure HDInsight](apache-spark-jupyter-spark-sql.md).
* Oracle Java Development Kit, Version 8, das für die Runtime der Eclipse-IDE verwendet wird. Sie können es von der [Oracle-Website](https://aka.ms/azure-jdks) herunterladen.
* Eclipse-IDE. Dieser Artikel verwendet Eclipse Neon. Sie können es von der [Eclipse-Website](https://www.eclipse.org/downloads/) aus installieren.

## <a name="install-hdinsight-tools-in-azure-toolkit-for-eclipse-and-the-scala-plug-in"></a>Installieren der HDInsight Tools im Azure-Toolkit für Eclipse und des Scala-Plug-Ins

### <a name="install-azure-toolkit-for-eclipse"></a>Installieren des Azure-Toolkits für Eclipse

Die HDInsight-Tools für Eclipse sind als Teil des Azure-Toolkits für Eclipse verfügbar. Installationsanweisungen finden Sie unter [Installieren des Azure-Toolkits für Eclipse](https://docs.microsoft.com/java/azure/eclipse/azure-toolkit-for-eclipse-installation).

### <a name="install-the-scala-plug-in"></a>Installieren des Scala-Plug-Ins

Wenn Sie Eclipse öffnen, erkennen HDInsight Tools automatisch, ob das Scala-Plug-In installiert ist. Klicken Sie auf **OK** um fortzufahren, und folgen Sie dann den Anweisungen zum Installieren des Plug-Ins vom Eclipse-Marketplace.

![Automatische Installation des Scala-Plug-Ins](./media/apache-spark-eclipse-tool-plugin/auto-installation-scala1.png)

Der Benutzer kann sich entweder [beim Azure-Abonnement anmelden](#sign-in-to-your-azure-subscription) oder [einen HDInsight-Cluster verknüpfen](#link-a-cluster). Dabei kann er entweder eine Kombination aus Ambari-Benutzername und -Kennwort oder Anmeldeinformationen verwenden, die in eine Domäne eingebunden sind.

## <a name="sign-in-to-your-azure-subscription"></a>Melden Sie sich bei Ihrem Azure-Abonnement an.

1. Starten Sie die Eclipse-IDE, und öffnen Sie den Azure Explorer. Wählen Sie im Menü **Window** (Fenster) die Option **Show View** (Ansicht anzeigen) und dann **Other** (Sonstiges) aus. Erweitern Sie im daraufhin angezeigten Dialogfeld den Eintrag **Azure**, klicken Sie auf **Azure Explorer** und dann auf **OK**.

   ![Apache Spark Eclipse – Ansicht anzeigen](./media/apache-spark-eclipse-tool-plugin/eclipse-view-explorer1.png)

1. Klicken Sie mit der rechten Maustaste auf den Knoten **Azure**, und wählen Sie dann **Sign in** (Anmelden) aus.
1. Wählen Sie im Dialogfeld **Azure Sign In** (Azure-Anmeldung) die Authentifizierungsmethode aus, klicken Sie auf **Sign in** (Anmelden), und geben Sie Ihre Azure-Anmeldeinformationen ein.

   ![Apache Spark Eclipse – Azure-Anmeldung](./media/apache-spark-eclipse-tool-plugin/eclipse-view-explorer2.png)

1. Nachdem Sie sich angemeldet haben, werden im Dialogfeld **Abonnements auswählen** alle Azure-Abonnements aufgelistet, die den Anmeldeinformationen zugeordnet sind. Klicken Sie im Dialogfeld auf **Auswählen**, um es zu schließen.

   ![Dialogfeld zum Auswählen von Abonnements](./media/apache-spark-eclipse-tool-plugin/Select-Subscriptions.png)

1. Erweitern Sie auf der Registerkarte **Azure Explorer** die Option **HDInsight**, um die HDInsight Spark-Cluster in Ihrem Abonnement anzuzeigen.

   ![HDInsight Spark-Cluster im Azure Explorer – 3](./media/apache-spark-eclipse-tool-plugin/eclipse-view-explorer3.png)

1. Sie können einen Clusternamenknoten noch einmal erweitern, um die dem Cluster zugeordneten Ressourcen (z.B. Speicherkonten) anzuzeigen.

   ![Erweitern eines Clusternamens zum Anzeigen der Ressourcen](./media/apache-spark-eclipse-tool-plugin/eclipse-view-explorer4.png)

## <a name="link-a-cluster"></a>Verknüpfen eines Clusters

Sie können einen normalen Cluster mithilfe des verwalteten Ambari-Benutzernamens verknüpfen. In ähnlicher Weise können Sie einen in eine Domäne eingebundenen HDInsight-Cluster unter Verwendung von Domäne und Benutzername verknüpfen, wie etwa `user1@contoso.com`.

1. Wählen Sie im **Azure-Explorer** den Befehl **Cluster verknüpfen** aus.

   ![Azure-Explorer – Menü „Cluster verknüpfen“](./media/apache-spark-eclipse-tool-plugin/link-a-cluster-context-menu.png)

1. Geben Sie **Clustername**, **Benutzername** und **Kennwort** ein, und klicken Sie anschließend auf „OK“, um den Cluster zu verknüpfen. Geben Sie optional das Speicherkonto und den Speicherschlüssel ein, und wählen Sie anschließend den Speichercontainer aus, damit der Speicher-Explorer in der linken Strukturansicht funktioniert.

   ![Dialogfeld zum Verknüpfen eines neuen HDInsight-Clusters](./media/apache-spark-eclipse-tool-plugin/link-cluster-dialog1.png)

   > [!NOTE]  
   > Wir verwenden den verknüpften Speicherschlüssel, den Benutzernamen und das Kennwort, wenn der Cluster im Azure-Abonnement angemeldet ist und einen Cluster verknüpft hat.
   > ![Azure-Explorer – Speicherkonten](./media/apache-spark-eclipse-tool-plugin/storage-explorer-in-Eclipse.png)

1. Ein verknüpfter Cluster wird im Knoten **HDInsight** angezeigt, nachdem Sie auf die Schaltfläche „OK“ geklickt haben, wenn die eingegebenen Informationen richtig sind. Jetzt können Sie eine Anwendung an diesen verknüpften Cluster übermitteln.

   ![Azure-Explorer – verknüpfter HDI-Cluster](./media/apache-spark-eclipse-tool-plugin/hdinsight-linked-cluster.png)

1. Sie können die Verknüpfung eines Clusters im **Azure-Explorer** auch aufheben.

   ![Azure-Explorer – nicht verknüpfter Cluster](./media/apache-spark-eclipse-tool-plugin/hdi-unlinked-cluster.png)

## <a name="set-up-a-spark-scala-project-for-an-hdinsight-spark-cluster"></a>Einrichten eines Spark Scala-Projekts für einen HDInsight Spark-Cluster

1. Wählen Sie im Arbeitsbereich der Eclipse-IDE **File** (Datei), **New** (Neu) und dann **Project** (Projekt) aus.

1. Erweitern Sie im Assistenten zum Erstellen eines neuen Projekts die Option **HDInsight**, wählen Sie **Spark on HDInsight (Scala)** (Spark für HDInsight [Scala]) aus, und klicken Sie dann auf **Next** (Weiter).

   ![Auswählen des Projekts „Spark für HDInsight (Scala)“](./media/apache-spark-eclipse-tool-plugin/create-hdi-scala-app-2.png)

1. Der Scala-Projekterstellungs-Assistent erkennt automatisch, ob das Scala-Plug-In installiert ist. Klicken Sie auf **OK**, um den Download des Scala-Plug-Ins fortzusetzen, und folgen Sie dann den Anweisungen, um Eclipse neu zu starten.

   ![Installation des fehlenden Scala-Plug-Ins](./media/apache-spark-eclipse-tool-plugin/auto-installation-scala2.png)

1. Geben Sie im Dialogfeld **New HDInsight Scala Project** (Neues HDInsight Scala-Projekt) die folgenden Werte an, und klicken Sie dann auf **Next** (Weiter):
   * Geben Sie einen Namen für das Projekt ein.
   * Achten Sie darauf, dass im Bereich **JRE** für **Ausführungsumgebungs-JRE verwenden** die Option **JavaSE-1.7** oder höher festgelegt ist.
   * Im Bereich **Spark-Bibliothek** können Sie die Option **Spark-SDK mit Maven konfigurieren** auswählen.  Unser Tool integriert die richtige Version für das Spark-SDK und das Scala-SDK. Sie können auch die Option **Spark-SDK manuell hinzufügen** auswählen, um das Spark-SDK manuell herunterzuladen und hinzuzufügen.

   ![Dialogfeld für ein neues HDInsight Scala-Projekt](./media/apache-spark-eclipse-tool-plugin/create-hdi-scala-app-3.png)

1. Wählen Sie im nächsten Dialogfeld **Fertig stellen** aus.

## <a name="create-a-scala-application-for-an-hdinsight-spark-cluster"></a>Erstellen einer Scala-Anwendung für einen HDInsight Spark-Cluster

1. Erweitern Sie in der Eclipse-IDE im Paket-Explorer das Projekt, das Sie zuvor erstellt haben, klicken Sie mit der rechten Maustaste auf **src**, zeigen Sie auf **New** (Neu) und dann auf **Other** (Sonstiges).

1. Erweitern Sie im Dialogfeld **Select a wizard** (Assistenten auswählen) die Option **Scala Wizards** (Scala-Assistenten), wählen Sie **Scala Object** (Scala-Objekt) aus, und klicken Sie dann auf **Next** (Weiter).

   ![Auswahl eines Assistenten zum Erstellen eines Scala-Objekts](./media/apache-spark-eclipse-tool-plugin/create-scala-project1.png)
1. Geben Sie im Dialogfeld **Create New File** (Neue Datei erstellen) einen Namen für das Objekt ein, und klicken Sie dann auf **Finish** (Fertig stellen).

   ![Assistent für neue Dateien – „Neue Datei erstellen“](./media/apache-spark-eclipse-tool-plugin/create-scala-project2.png)
1. Fügen Sie folgenden Code in den Text-Editor ein:

    ```scala
    import org.apache.spark.SparkConf
    import org.apache.spark.SparkContext

    object MyClusterApp{
        def main (arg: Array[String]): Unit = {
        val conf = new SparkConf().setAppName("MyClusterApp")
        val sc = new SparkContext(conf)

        val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        //find the rows that have only one digit in the seventh column in the CSV
        val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)

        rdd1.saveAsTextFile("wasb:///HVACOut")
        }        
    }
    ```

1. Führen Sie die Anwendung in einem HDInsight Spark-Cluster aus:

   a. Klicken Sie im Paket-Explorer mit der rechten Maustaste auf den Projektnamen, und wählen Sie dann **Submit Spark Application to HDInsight** (Spark-Anwendung an HDInsight senden) aus.

   b. Geben Sie im Dialogfeld **Spark Submission** (Spark-Übermittlung) die folgenden Werte ein, und wählen Sie dann **Submit** (Übermitteln) aus:

   * Wählen Sie für **Cluster Name**den HDInsight Spark-Cluster aus, auf dem Sie Ihre Anwendung ausführen möchten.
   * Wählen Sie ein Artefakt aus dem Eclipse-Projekt oder von der Festplatte aus. Der Standardwert hängt von dem Element ab, auf das Sie im Paket-Explorer mit der rechten Maustaste klicken.
   * In der Dropdownliste **Main class name** (Hauptklassenname) zeigt der Übermittlungs-Assistent alle Objektnamen aus Ihrem Projekt an. Wählen Sie ein Objekt aus, oder geben Sie den Namen des Objekts ein, das Sie ausführen möchten. Wenn Sie ein Artefakt von einer Festplatte ausgewählt haben, müssen Sie den Namen der Hauptklasse manuell eingeben. 
   * Weil der Anwendungscode in diesem Beispiel keine Befehlszeilenargumente, Referenz-JARs oder Referenzdateien erfordert, können Sie die restlichen Textfelder leer lassen.

     ![Dialogfeld für die Apache Spark-Übermittlung](./media/apache-spark-eclipse-tool-plugin/create-scala-project3.png)

1. Auf der Registerkarte **Spark Submission** sollte nun der Fortschritt angezeigt werden. Sie können die Anwendung anhalten, indem Sie im Fenster **Spark Submission** (Spark-Übermittlung) auf die rote Schaltfläche klicken. Sie können auch die Protokolle für die Anwendungsausführung anzeigen, indem Sie auf das Globussymbol klicken (in der Abbildung gekennzeichnet durch das blaue Feld).

   ![Fenster für die Apache Spark-Übermittlung](./media/apache-spark-eclipse-tool-plugin/create-scala-project4.png)

## <a name="access-and-manage-hdinsight-spark-clusters-by-using-hdinsight-tools-in-azure-toolkit-for-eclipse"></a>Zugreifen auf und Verwalten von HDInsight Spark-Clustern mithilfe der HDInsight-Tools im Azure-Toolkit für Eclipse

Sie können mithilfe der HDInsight-Tools verschiedene Vorgänge durchführen, z.B. auf die Auftragsausgabe zugreifen.

### <a name="access-the-job-view"></a>Zugreifen auf die Auftragsansicht

1. Erweitern Sie im Azure Explorer die Option **HDInsight** und den Namen des Spark-Clusters, und wählen Sie dann **Jobs** (Aufträge) aus.

   ![Azure-Explorer – Ansicht des Eclipse-Auftragsknotens](./media/apache-spark-eclipse-tool-plugin/eclipse-job-view-node.png)

1. Wählen Sie den Knoten **Jobs** (Aufträge) aus. Wenn die Java-Version niedriger als **1.8** ist, erinnern Sie HDInsight Tools automatisch daran, das **E(fx)clipse**-Plug-In zu installieren. Klicken Sie auf **OK**, um fortzufahren, befolgen Sie dann die Anweisungen des Assistenten zum Installieren über den Eclipse Marketplace, und starten Sie Eclipse neu.

   ![Installation des fehlenden E(fx)clipse-Plug-Ins](./media/apache-spark-eclipse-tool-plugin/auto-install-efxclipse.png)

1. Öffnen Sie in der Auftragsansicht den Knoten **Jobs** (Aufträge). Im rechten Bereich werden auf der Registerkarte **Spark Job View** (Spark-Auftragsansicht) alle Anwendungen angezeigt, die in dem Cluster ausgeführt wurden. Wählen Sie den Namen der Anwendung aus, zu der Sie weitere Details anzeigen möchten.

   ![Apache Eclipse – Anzeigen von Auftragsprotokolldetails](./media/apache-spark-eclipse-tool-plugin/eclipse-view-job-logs.png)

   Sie können dann eine der folgenden Aktionen ausführen:

   * Zeigen Sie auf den Auftragsgraphen. Dadurch werden grundlegende Informationen zum ausgeführten Auftrag angezeigt. Wenn Sie den Auftragsgraphen auswählen, werden die Phasen und die von den einzelnen Aufträgen generierten Informationen angezeigt.

     ![Apache Spark – Info zu Phasen im Auftragsdiagramm](./media/apache-spark-eclipse-tool-plugin/Job-graph-stage-info.png)

   * Klicken Sie auf die Registerkarte **Log** (Protokoll), um häufig verwendete Protokolle anzuzeigen, z. B. **Driver Stderr**, **Driver Stdout** und **Directory Info**.

     ![Apache Spark Eclipse – Info zum Auftragsprotokoll](./media/apache-spark-eclipse-tool-plugin/eclipse-job-log-info.png)

   * Öffnen Sie die Spark-Verlaufsbenutzeroberfläche und die Apache Hadoop YARN-Benutzeroberfläche (auf Anwendungsebene), indem Sie auf die Links im oberen Bereich des Fensters klicken.

### <a name="access-the-storage-container-for-the-cluster"></a>Zugreifen auf den Speichercontainer des Clusters

1. Erweitern Sie im Azure Explorer den Stammknoten **HDInsight**, um eine Liste der verfügbaren HDInsight Spark-Cluster anzuzeigen.

1. Erweitern Sie den Namen des Clusters, um das Speicherkonto und den Standardspeichercontainer des Clusters anzuzeigen.

   ![Standardspeicherkonto und -container](./media/apache-spark-eclipse-tool-plugin/eclipse-view-explorer5.png)

1. Wählen Sie den Namen des Speichercontainers aus, der dem Cluster zugeordnet ist. Doppelklicken Sie im rechten Bereich auf den Ordner **HVACOut**. Öffnen Sie eine der **part**-Dateien, um die Ausgabe der Anwendung anzuzeigen.

### <a name="access-the-spark-history-server"></a>Zugreifen auf den Spark-Verlaufsserver

1. Klicken Sie im Azure Explorer mit der rechten Maustaste auf den Namen Ihres Spark-Clusters, und wählen Sie dann **Open Spark History UI** (Spark-Verlaufsbenutzeroberfläche öffnen) aus. Geben Sie die Anmeldeinformationen für den Cluster ein, wenn Sie dazu aufgefordert werden. Diese Anmeldeinformationen haben Sie bei der Bereitstellung des Clusters angegeben.

1. Im Dashboard des Spark-Verlaufsservers können Sie die Anwendung, deren Ausführung Sie gerade beendet haben, anhand des Anwendungsnamens suchen. Im obigen Code legen Sie den Namen der Anwendung mit `val conf = new SparkConf().setAppName("MyClusterApp")` fest. Daher lautete der Name Ihrer Spark-Anwendung **MyClusterApp**.

### <a name="start-the-apache-ambari-portal"></a>Starten des Apache Ambari-Portals

1. Klicken Sie im Azure Explorer mit der rechten Maustaste auf den Namen Ihres Spark-Clusters, und wählen Sie dann **Open Cluster Management Portal (Ambari)** (Clusterverwaltungsportal [Ambari] öffnen) aus.

1. Geben Sie die Anmeldeinformationen für den Cluster ein, wenn Sie dazu aufgefordert werden. Diese Anmeldeinformationen haben Sie bei der Bereitstellung des Clusters angegeben.

### <a name="manage-azure-subscriptions"></a>Verwalten von Azure-Abonnements

Standardmäßig führen HDInsight Tools im Azure-Toolkit für Eclipse die Spark-Cluster in all Ihren Azure-Abonnements auf. Bei Bedarf können Sie die Abonnements angeben, für die Sie auf den Cluster zugreifen möchten.

1. Klicken Sie im Azure Explorer mit der rechten Maustaste auf den Hauptknoten **Azure**, und wählen Sie dann **Abonnements verwalten** aus.

1. Deaktivieren Sie im Dialogfeld die Kontrollkästchen für die Abonnements, auf die Sie nicht zugreifen möchten, und klicken Sie dann auf **Close** (Schließen). Sie können auch **Abmelden** auswählen, wenn Sie sich von Ihrem Azure-Abonnement abmelden möchten.

## <a name="run-a-spark-scala-application-locally"></a>Lokales Ausführen einer Spark Scala-Anwendung

Mithilfe der HDInsight-Tools-im Azure-Toolkit für Eclipse können Sie Spark Scala-Anwendungen lokal auf Ihrer Arbeitsstation ausführen. In der Regel müssen solche Anwendungen nicht auf Clusterressourcen wie den Speichercontainer zugreifen und können lokal ausgeführt und getestet werden.

### <a name="prerequisite"></a>Voraussetzung

Beim Ausführen der lokalen Spark Scala-Anwendung auf einem Windows-Computer wird möglicherweise eine Ausnahme wie unter [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356) beschrieben ausgelöst. Diese Ausnahme tritt auf, weil **WinUtils.exe** in Windows fehlt.

Um diesen Fehler zu beheben, müssen Sie [die ausführbare Datei herunterladen](https://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) und an einem Speicherort wie **C:\WinUtils\bin** ablegen. Fügen Sie dann die Umgebungsvariable **HADOOP_HOME** hinzu, und legen Sie den Wert der Variable auf **C\WinUtils** fest.

### <a name="run-a-local-spark-scala-application"></a>Ausführen einer lokalen Spark Scala-Anwendung

1. Starten Sie Eclipse, und erstellen Sie ein Projekt. Nehmen Sie im Dialogfeld **New Project** (Neues Projekt) die folgenden Einstellungen vor, und klicken Sie anschließend auf **Next** (Weiter).

   * Wählen Sie im linken Bereich **HDInsight** aus.
   * Wählen Sie im rechten Bereich **Spark on HDInsight Local Run Sample (Scala)** (Spark auf HDInsight – Beispiel für lokale Ausführung [Scala]) aus.

   ![Dialogfeld „Neues Projekt“ – Auswählen eines Assistenten](./media/apache-spark-eclipse-tool-plugin/hdi-spark-app-local-run.png)

1. Um die Projektdetails anzugeben, führen Sie die Schritte 3 bis 6 wie oben im Abschnitt [Einrichten eines Spark Scala-Projekts für einen HDInsight Spark-Cluster](#set-up-a-spark-scala-project-for-an-hdinsight-spark-cluster) gezeigt durch.

1. Die Vorlage fügt einen Beispielcode (**LogQuery**) unter dem Ordner **src** hinzu, der lokal auf Ihrem Computer ausgeführt werden kann.

   ![Speicherort der lokalen Scala-Anwendung „LogQuery“](./media/apache-spark-eclipse-tool-plugin/local-scala-application.png)

1. Klicken Sie mit der rechten Maustaste auf die Anwendung **LogQuery**, zeigen Sie auf **Run As** (Ausführen als), und klicken Sie dann auf **1 Scala Application** (1 Scala-Anwendung). Auf der Registerkarte **Console** (Konsole) wird eine Ausgabe der folgenden Art angezeigt:

   ![Ergebnis der lokalen Ausführung der Spark-Anwendung](./media/apache-spark-eclipse-tool-plugin/hdi-spark-app-local-run-result.png)

## <a name="reader-only-role"></a>Rolle nur mit Leseberechtigung

Wenn Benutzer, die über eine Rolle nur mit Leseberechtigung verfügen, einen Auftrag an einen Cluster übermitteln, sind Ambari-Anmeldeinformationen erforderlich.

### <a name="link-cluster-from-context-menu"></a>Kontextmenü zum Verknüpfen eines Clusters

1. Melden Sie sich mit einem Konto an, das über eine Rolle nur mit Leseberechtigung verfügt.

2. Erweitern Sie im **Azure Explorer** die Option **HDInsight**, um die HDInsight-Cluster anzuzeigen, die sich in Ihrem Abonnement befinden. Cluster mit der Kennzeichnung **"Role: Reader"** verfügen über eine Rolle nur mit Leseberechtigung.

    ![HDInsight Spark-Cluster im Azure Explorer – Rollenleser](./media/apache-spark-eclipse-tool-plugin/eclipse-view-explorer6.png)

3. Klicken Sie mit der rechten Maustaste auf den Cluster, der über eine Rolle nur mit Leseberechtigung verfügt. Wählen Sie aus dem Kontextmenü den Eintrag **Link this cluster** (Diesen Cluster verknüpfen) aus, um den Cluster zu verknüpfen. Geben Sie den Benutzernamen und das Kennwort für Ambari ein.

    ![HDInsight Spark-Cluster im Azure Explorer – Link](./media/apache-spark-eclipse-tool-plugin/eclipse-view-explorer7.png)

4. Wenn der Cluster erfolgreich verknüpft wurde, wird HDInsight aktualisiert.
   Der Status des Clusters ändert sich zu „Linked“ (Verknüpft).
  
    ![HDInsight Spark-Cluster im Azure Explorer – verknüpft](./media/apache-spark-eclipse-tool-plugin/eclipse-view-explorer8.png)

### <a name="link-cluster-by-expanding-jobs-node"></a>Verknüpfen eines Clusters durch Erweitern des Knotens „Jobs“ (Aufträge)

1. Klicken Sie auf den Knoten **Jobs** (Aufträge). Das Fenster **Cluster Job Access Denied** (Zugriff auf Clusterauftrag verweigert) wird angezeigt.

2. Klicken Sie auf **Link this cluster** (Diesen Cluster verknüpfen), um den Cluster zu verknüpfen.

    ![HDInsight Spark-Cluster im Azure Explorer – 9](./media/apache-spark-eclipse-tool-plugin/eclipse-view-explorer9.png)

### <a name="link-cluster-from-spark-submission-window"></a>Verknüpfen des Clusters aus dem Fenster für die Spark-Übermittlung

1. Erstellen Sie ein HDInsight-Projekt.

2. Klicken Sie mit der rechten Maustaste auf das Paket. Wählen Sie dann **Submit Spark Application to HDInsight** (Spark-Anwendung an HDInsight senden) aus.

   ![HDInsight Spark-Cluster im Azure Explorer – übermitteln](./media/apache-spark-eclipse-tool-plugin/eclipse-view-explorer11.png)

3. Wählen Sie einen Cluster aus, der über eine Rolle nur mit Leseberechtigung für **Cluster Name** (Clustername) verfügt. Es wird eine Warnmeldung angezeigt. Sie können auf **Link this cluster** (Diesen Cluster verknüpfen) klicken, um den Cluster zu verknüpfen.

   ![HDInsight Spark-Cluster im Azure Explorer – dies verknüpfen](./media/apache-spark-eclipse-tool-plugin/eclipse-view-explorer15.png)

### <a name="view-storage-accounts"></a>Anzeigen von Speicherkonten

* Klicken Sie in einem Cluster, der über eine Rolle nur mit Leseberechtigung verfügt, auf den Knoten **Storage Accounts** (Speicherkonten). Das Fenster **Storage Access Denied** (Zugriff auf Speicher verweigert) wird angezeigt.

   ![HDInsight Spark-Cluster im Azure Explorer – Speicher](./media/apache-spark-eclipse-tool-plugin/eclipse-view-explorer13.png)

   ![HDInsight Spark-Cluster im Azure Explorer – abgelehnt](./media/apache-spark-eclipse-tool-plugin/eclipse-view-explorer12.png)

* Klicken Sie in einem verknüpften Cluster auf den Knoten **Storage Accounts** (Speicherkonten). Das Fenster **Storage Access Denied** (Zugriff auf Speicher verweigert) wird angezeigt.

   ![HDInsight Spark-Cluster im Azure Explorer – abgelehnt 2](./media/apache-spark-eclipse-tool-plugin/eclipse-view-explorer14.png)

## <a name="known-problems"></a>Bekannte Probleme

Beim Verknüpfen eines Clusters würde ich empfehlen, die Anmeldeinformationen des Speichers anzugeben.

![Verknüpfen eines Clusters mit Speicheranmeldeinformationen – Eclipse](./media/apache-spark-eclipse-tool-plugin/link-cluster-with-storage-credential-eclipse.png)

Es gibt zwei Modi zum Übermitteln der Aufträge. Wenn Speicheranmeldeinformationen bereitgestellt werden, wird der Auftrag im Batchmodus übermittelt. Andernfalls wird der interaktive Modus verwendet. Wenn der Cluster ausgelastet ist, erhalten Sie möglicherweise die folgende Fehlermeldung.

![Ecplipse-Fehler beim Abrufen bei ausgelastetem Cluster](./media/apache-spark-eclipse-tool-plugin/eclipse-interactive-cluster-busy-upload.png "Ecplipse-Fehler beim Abrufen bei ausgelastetem Cluster")

![Ecplipse-Fehler beim Abrufen bei ausgelastetem Cluster – Yarn](./media/apache-spark-eclipse-tool-plugin/eclipse-interactive-cluster-busy-submit.png "Ecplipse-Fehler beim Abrufen bei ausgelastetem Cluster – Yarn")

## <a name="seealso"></a>Weitere Informationen

* [Übersicht: Apache Spark in Azure HDInsight](apache-spark-overview.md)

### <a name="scenarios"></a>Szenarien

* [Apache Spark mit BI: Durchführen interaktiver Datenanalysen mithilfe von Spark in HDInsight mit BI-Tools](apache-spark-use-bi-tools.md)
* [Apache Spark mit Machine Learning: Analysieren von Gebäudetemperaturen mithilfe von Spark in HDInsight und HVAC-Daten](apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark mit Machine Learning: Vorhersage von Lebensmittelkontrollergebnissen mithilfe von Spark in HDInsight](apache-spark-machine-learning-mllib-ipython.md)
* [Websiteprotokollanalyse mithilfe von Apache Spark in HDInsight](apache-spark-custom-library-website-log-analysis.md)

### <a name="creating-and-running-applications"></a>Erstellen und Ausführen von Anwendungen

* [Erstellen einer eigenständigen Anwendung mit Scala](apache-spark-create-standalone-application.md)
* [Ausführen von Remoteaufträgen in einem Apache Spark-Cluster mithilfe von Apache Livy](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Tools und Erweiterungen

* [Verwenden des Azure-Toolkits für IntelliJ zum Erstellen und Übermitteln von Spark Scala-Anwendungen](apache-spark-intellij-tool-plugin.md)
* [Verwenden des Azure-Toolkits für IntelliJ zum Remotedebuggen von Apache Spark-Anwendungen über VPN](../hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Verwenden des Azure-Toolkits für IntelliJ zum Remotedebuggen von Apache Spark-Anwendungen über SSH](../hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Verwenden von Apache Zeppelin Notebooks mit einem Apache Spark-Cluster unter HDInsight](apache-spark-zeppelin-notebook.md)
* [Kernel für Jupyter Notebook in Apache Spark-Clustern für HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Verwenden von externen Paketen mit Jupyter Notebooks](apache-spark-jupyter-notebook-use-external-packages.md)
* [Installieren von Jupyter Notebook auf Ihrem Computer und Herstellen einer Verbindung zum Apache Spark-Cluster in Azure HDInsight (Vorschau)](apache-spark-jupyter-notebook-install-locally.md)

### <a name="managing-resources"></a>Verwalten von Ressourcen

* [Verwalten von Ressourcen für den Apache Spark-Cluster in Azure HDInsight](apache-spark-resource-manager.md)
* [Track and debug jobs running on an Apache Spark cluster in HDInsight (Nachverfolgen und Debuggen von Aufträgen in einem Apache Spark-Cluster unter HDInsight)](apache-spark-job-debugging.md)
