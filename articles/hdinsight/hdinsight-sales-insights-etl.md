---
title: 'Tutorial: Erstellen einer End-to-End-ETL-Pipeline zum Gewinnen von Erkenntnissen aus Vertriebsdaten'
description: Hier erfahren Sie, wie Sie ETL-Pipelines mit Azure HDInsight erstellen, um mithilfe von bedarfsgesteuerten Spark-Clustern und Power BI Erkenntnisse aus Vertriebsdaten zu gewinnen.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: tutorial
ms.date: 09/30/2019
ms.author: hrasheed
ms.openlocfilehash: b9bcaf4b7497e8beba377eb7e47a44a6eb061299
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72178007"
---
# <a name="tutorial-create-an-end-to-end-data-pipeline-to-derive-sales-insights"></a>Tutorial: Erstellen einer End-to-End-Datenpipeline zum Gewinnen von Erkenntnissen aus Vertriebsdaten

In diesem Tutorial erstellen Sie eine End-to-End-Datenpipeline, die Extrahierungs-, Transformations- und Ladevorgänge (ETL) durchführt. Die Pipeline verwendet in Azure HDInsight ausgeführte Apache Spark- und Apache Hive-Cluster zum Abfragen und Bearbeiten der Daten. Sie verwenden auch Technologien wie Azure Data Lake Storage Gen2 für die Datenspeicherung und Power BI für die Visualisierung.

Diese Datenpipeline kombiniert die Daten aus unterschiedlichen Speichern, entfernt unerwünschte Daten, fügt neue Daten an und lädt sie dann alle wieder in den Speicher, um geschäftliche Erkenntnisse zu visualisieren. Weitere Informationen zu ETL-Pipelines finden Sie unter [Bedarfsorientiertes Extrahieren, Transformieren und Laden (ETL)](./hadoop/apache-hadoop-etl-at-scale.md).

![ETL-Architektur](./media/hdinsight-sales-insights-etl/architecture.png)

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

Laden Sie [Power BI Desktop](https://www.microsoft.com/download/details.aspx?id=45331) herunter, um am Ende dieses Tutorials geschäftliche Erkenntnisse zu visualisieren.

## <a name="create-resources"></a>Erstellen von Ressourcen

### <a name="clone-the-repository-with-scripts-and-data"></a>Klonen des Repositorys mit Skripts und Daten

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Öffnen Sie Azure Cloud Shell über die obere Menüleiste. Wählen Sie Ihr Abonnement zum Erstellen einer Dateifreigabe aus, wenn Sie von Cloud Shell dazu aufgefordert werden.

   ![Öffnen von Azure Cloud Shell](./media/hdinsight-sales-insights-etl/hdinsight-sales-insights-etl-click-cloud-shell.png)
1. Wählen Sie im Dropdownmenü **Umgebung auswählen** die Option **Bash** aus.
1. Melden Sie sich bei Ihrem Azure-Konto an, und wählen Sie das Abonnement aus. 
1. Richten Sie die Ressourcengruppe für das Projekt ein.
   1. Wählen Sie einen eindeutigen Namen für die Ressourcengruppe aus.
   1. Führen Sie den folgenden Codeausschnitt in Cloud Shell aus, um Variablen festzulegen, die in späteren Schritten verwendet werden:

       ```azurecli-interactive 
       resourceGroup="<RESOURCE GROUP NAME>"
       subscriptionID="<SUBSCRIPTION ID>"
        
       az account set --subscription $subscriptionID
       az group create --name $resourceGroup --location westus
       ```

1. Laden Sie die Daten und Skripts für dieses Tutorial aus dem [Repository für das HDInsight-ETL-Szenario mit Vertriebsdaten](https://github.com/Azure-Samples/hdinsight-sales-insights-etl) herunter, indem Sie die folgenden Befehle in Cloud Shell eingeben:

    ```azurecli-interactive 
    git clone https://github.com/Azure-Samples/hdinsight-sales-insights-etl.git
    cd hdinsight-sales-insights-etl
    ```

1. Geben Sie an der Shell-Eingabeaufforderung `ls` ein, um zu überprüfen, ob die folgenden Dateien und Verzeichnisse erstellt wurden:

   ```output
   /salesdata/
   /scripts/
   /templates/
   ```

### <a name="deploy-azure-resources-needed-for-the-pipeline"></a>Bereitstellen der erforderlichen Azure-Ressourcen für die Pipeline 

1. Fügen Sie Ausführungsberechtigungen für das `chmod +x scripts/*.sh`-Skript hinzu.
1. Führen Sie das Skript mit dem Befehl `./scripts/resources.sh <RESOURCE_GROUP_NAME> <LOCATION>` aus, um die folgenden Ressourcen in Azure bereitzustellen:

   1. Ein Azure Blob Storage-Konto. Dieses Konto enthält die Vertriebsdaten des Unternehmens.
   2. Ein Azure Data Lake Storage Gen2-Konto. Dieses Konto dient als Speicherkonto für beide HDInsight-Cluster. Weitere Informationen zu HDInsight und Data Lake Storage Gen2 finden Sie unter [Azure HDInsight-Integration in Data Lake Storage Gen2](https://azure.microsoft.com/blog/azure-hdinsight-integration-with-data-lake-storage-gen-2-preview-acl-and-security-update/).
   3. Eine vom Benutzer zugewiesene verwaltete Identität. Dieses Konto gewährt den HDInsight-Clustern Zugriff auf das Data Lake Storage Gen2-Konto.
   4. Einen Apache Spark-Cluster. Dieser Cluster wird zum Bereinigen und Transformieren der Rohdaten verwendet.
   5. Einen Interactive Query-Cluster in Apache Hive. Mit diesem Cluster können die Vertriebsdaten abgefragt und mit Power BI visualisiert werden.
   6. Ein virtuelles Azure-Netzwerk, das von NSG-Regeln (Netzwerksicherheitsgruppe) unterstützt wird. Dieses virtuelle Netzwerk ermöglicht die Kommunikation der Cluster und sichert zudem ihre Kommunikation. 

Die Erstellung des Clusters dauert ungefähr 20 Minuten.

Das `resources.sh`-Skript enthält den folgenden Befehl. Dieser Befehl erstellt mithilfe einer Resource Manager-Vorlage (`resourcestemplate.json`) die angegebenen Ressourcen mit der gewünschten Konfiguration.

```azurecli-interactive 
az group deployment create --name ResourcesDeployment \
    --resource-group $resourceGroup \
    --template-file resourcestemplate.json \
    --parameters "@resourceparameters.json"
```

Das `resources.sh`-Skript lädt mit dem folgenden Befehl auch die CSV-Dateien mit den Vertriebsdaten in das neu erstellte Blob-Speicherkonto hoch:

```
az storage blob upload-batch -d rawdata \
    --account-name <BLOB STORAGE NAME> -s ./ --pattern *.csv
```

Das Standardkennwort für den SSH-Zugriff auf die Cluster ist `Thisisapassword1`. Wenn Sie das Kennwort ändern möchten, rufen Sie die `resourcesparameters.json` auf, und ändern Sie das Kennwort für die Parameter `sparksshPassword`, `sparkClusterLoginPassword`, `llapClusterLoginPassword` und `llapsshPassword`.

### <a name="verify-deployment-and-collect-resource-information"></a>Überprüfen der Bereitstellung und Sammeln von Ressourceninformationen

1. Um den Status der Bereitstellung zu überprüfen, navigieren Sie im Azure-Portal zur Ressourcengruppe. Wählen Sie unter **Einstellungen** die Option **Bereitstellungen** aus. Wählen Sie den Namen Ihrer Bereitstellung (`ResourcesDeployment`) aus. Hier sehen Sie die Ressourcen, die erfolgreich bereitgestellt wurden, und jene, deren Bereitstellung noch läuft.
1. Öffnen Sie nach Abschluss der Bereitstellung das Azure-Portal, und navigieren Sie zu **Ressourcengruppen** > <RESSOURCENGRUPPENNAME>.
1. Suchen Sie nach dem neuen Azure-Speicherkonto, das zum Speichern der Vertriebsdateien erstellt wurde. Der Name des Speicherkontos beginnt mit `blob`, gefolgt von einer zufälligen Zeichenfolge. Gehen Sie wie folgt vor:
   1. Notieren Sie den Namen des Speicherkontos zur späteren Verwendung.
   1. Wähle Sie den Namen des Blob-Speicherkontos aus.
   1. Wählen Sie links im Portal unter **Einstellungen** die Option **Zugriffsschlüssel** aus.
   1. Kopieren Sie die Zeichenfolge im Feld **Schlüssel1**, und speichern Sie sie zur späteren Verwendung.
1. Suchen Sie nach dem Data Lake Storage Gen2-Konto, das als Speicher für die HDInsight-Cluster erstellt wurde. Dieses Konto befindet sich in derselben Ressourcengruppe wie das Blob Storage-Konto, beginnt jedoch mit `adlsgen2`. Gehen Sie wie folgt vor:
   1. Notieren Sie den Namen des Data Lake Storage Gen2-Kontos.
   1. Wählen Sie auf den Namen des Data Lake Storage Gen2-Kontos aus.
   1. Wählen Sie links im Portal unter **Einstellungen** die Option **Zugriffsschlüssel** aus.
   1. Kopieren Sie die Zeichenfolge im Feld **Schlüssel1**, und speichern Sie sie zur späteren Verwendung.

> [!Note]
> Sobald Sie die Namen der Speicherkonten kennen, können Sie die Kontoschlüssel abrufen. Führen Sie dazu den folgenden Befehl an der Azure Cloud Shell-Eingabeaufforderung aus:
> ```azurecli-interactive
> az storage account keys list \
>    --account-name <STORAGE NAME> \
>    --resource-group $rg \
>    --output table
> ```

### <a name="create-a-data-factory"></a>Erstellen einer Data Factory

Azure Data Factory (ADF) ist ein Tool, mit dem Sie Azure Pipelines automatisieren können. Es ist nicht die einzige Möglichkeit, um diese Aufgaben durchzuführen, eignet sich aber hervorragend zum Automatisieren der Prozesse. Weitere Informationen zu Azure Data Factory finden Sie in der [Dokumentation zu Azure Data Factory](https://azure.microsoft.com/en-us/services/data-factory/). 

Diese Data Factory enthält eine Pipeline mit zwei Aktivitäten: 

- Die erste Aktivität kopiert die Daten aus dem Azure Blob Storage-Konto in das Data Lake Storage Gen2-Speicherkonto, um die Datenerfassung zu simulieren.
- Die zweite Aktivität transformiert die Daten im Spark-Cluster. Das Skript transformiert die Daten, indem unerwünschte Spalten entfernt werden. Es fügt auch eine neue Spalte an, die den durch eine einzelne Transaktion generierten Umsatz berechnet.

Führen Sie das `adf.sh`-Skript aus, um Ihre Azure Data Factory-Pipeline einzurichten:

1. Fügen Sie mithilfe von `chmod +x adf.sh` Ausführungsberechtigungen für die Datei hinzu.
1. Führen Sie das Skript mithilfe von `./adf.sh` aus. 

Das Skript führt folgende Schritte aus:

1. Es erstellt einen Dienstprinzipal mit `Storage Blob Data Contributor`-Berechtigungen für das Data Lake Storage Gen2-Speicherkonto.
1. Es ruft ein Authentifizierungstoken zum Autorisieren von POST-Anforderungen an die [REST-API für das Data Lake Storage Gen2-Dateisystem](https://docs.microsoft.com/rest/api/storageservices/datalakestoragegen2/filesystem/create) ab.
1. Es trägt in den Dateien `sparktransform.py` und `query.hql` den tatsächlichen Namen Ihres Data Lake Storage Gen2-Speicherkontos ein.
1. Es ruft Speicherschlüssel für die Data Lake Storage Gen2- und Blob Storage-Konten ab.
1. Es erstellt eine weitere Ressourcenbereitstellung, um eine Azure Data Factory-Pipeline mit den zugehörigen verknüpften Diensten und Aktivitäten zu erstellen. Es übergibt die Speicherschlüssel als Parameter an die Vorlagendatei, sodass die verknüpften Dienste korrekt auf die Speicherkonten zugreifen können.

Die Data Factory-Pipeline wird mit dem folgenden Befehl bereitgestellt:

```azurecli-interactive
az group deployment create --name ADFDeployment \
    --resource-group $resourceGroup \
    --template-file adftemplate.json \
    --parameters "@adfparameters.json"
```

## <a name="run-the-data-pipeline"></a>Ausführen der Datenpipeline

### <a name="trigger-the-data-factory-activities"></a>Auslösen der Data Factory-Aktivitäten

Die erste Aktivität in der von Ihnen erstellten Data Factory-Pipeline verschiebt die Daten aus Blob Storage in Data Lake Storage Gen2. Die zweite Aktivität wendet die Spark-Transformationen auf die Daten an und speichert die transformierten CSV-Dateien an einem neuen Speicherort. Die Ausführung der gesamten Pipeline kann einige Minuten dauern.

Sie können die Pipelines wie folgt auslösen:

- Führen Sie die folgenden Befehle in PowerShell aus, um die Data Factory-Pipelines auszulösen: 

    ```powershell
    Invoke-AzDataFactoryV2Pipeline -DataFactory $df -PipelineName "CopyPipeline_k8z" 
    Invoke-AzDataFactoryV2Pipeline -DataFactory $df -PipelineName "sparkTransformPipeline"
    ```

- Öffnen Sie die Data Factory, und wählen Sie **Erstellen und überwachen** aus. Lösen Sie die Kopierpipeline und dann die Spark-Pipeline über das Portal aus. Informationen zum Auslösen von Pipelines über das Portal finden Sie unter [Erstellen bedarfsgesteuerter Apache Hadoop-Cluster in HDInsight mit Azure Data Factory](hdinsight-hadoop-create-linux-clusters-adf.md#trigger-a-pipeline).

Zum Überprüfen, ob die Pipelines ausgeführt wurden, können Sie einen der beiden folgenden Schritte ausführen:

- Rufen Sie im Portal den Abschnitt **Überwachen** in Ihrer Data Factory auf.
- Rufen Sie im Azure Storage-Explorer Ihr Data Lake Storage Gen2-Speicherkonto auf. Wechseln Sie zum Dateisystem `files`, navigieren Sie zum Ordner `transformed`, und überprüfen Sie den Inhalt, um festzustellen, ob die Pipeline erfolgreich ausgeführt wurde.

Weitere Möglichkeiten zum Transformieren von Daten mithilfe von HDInsight finden Sie in [diesem Artikel zur Verwendung von Jupyter Notebook](/azure/hdinsight/spark/apache-spark-load-data-run-query).

### <a name="create-a-table-on-the-interactive-query-cluster-to-view-data-on-power-bi"></a>Erstellen einer Tabelle im Interactive Query-Cluster zum Anzeigen von Daten in Power BI

1. Kopieren Sie die Datei `query.hql` mit SCP in den LLAP-Cluster:

    ```
    scp scripts/query.hql sshuser@<clustername>-ssh.azurehdinsight.net:/home/sshuser/
    ```

2. Verwenden Sie SSH für den Zugriff auf den LLAP-Cluster, indem Sie den folgenden Befehl ausführen und dann Ihr Kennwort eingeben. Wenn Sie die Datei `resourcesparameters.json` nicht geändert haben, lautet das Kennwort `Thisisapassword1`.

    ```
    ssh sshuser@<clustername>-ssh.azurehdinsight.net
    ```

3. Verwenden Sie den folgenden Befehl, um das Skript auszuführen:

    ```
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -f query.hql
    ```

Dieses Skript erstellt eine verwaltete Tabelle im Interactive Query-Cluster, auf die Sie über Power BI zugreifen können. 

### <a name="create-a-power-bi-dashboard-from-sales-data"></a>Erstellen eines Power BI-Dashboards anhand von Vertriebsdaten

1. Öffnen Sie Power BI Desktop.
1. Wählen Sie **Daten abrufen** aus.
1. Suchen Sie nach **HDInsight-Interactive Query-Cluster**.
1. Fügen Sie dort den URI für Ihren Cluster ein. Sie sollte folgendes Format aufweisen: `https://<LLAP CLUSTER NAME>.azurehdinsight.net`.

   Geben Sie `default` als Datenbank ein.
1. Geben Sie den Benutzernamen und das Kennwort für den Zugriff auf den Cluster ein.

Nachdem die Daten geladen wurden, können Sie mit dem Dashboard experimentieren, das Sie erstellen möchten. Informationen zu den ersten Schritten mit Power BI-Dashboards finden Sie unter den folgenden Links:

* [Einführung in Dashboards für Power BI-Designer](https://docs.microsoft.com/power-bi/service-dashboards)
* [Tutorial: Erste Schritte mit dem Power BI-Dienst](https://docs.microsoft.com/power-bi/service-get-started)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie diese Anwendung nicht weiterverwenden möchten, löschen Sie mit dem folgenden Befehl alle Ressourcen, damit sie Ihnen nicht in Rechnung gestellt werden.

```azurecli-interactive 
az group delete -n $resourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Bedarfsorientiertes Extrahieren, Transformieren und Laden (ETL)](./hadoop/apache-hadoop-etl-at-scale.md)
