---
title: 'Tutorial: Erstellen einer End-to-End-ETL-Pipeline (Extrahieren, Transformieren und Laden) zum Gewinnen von Erkenntnissen aus Vertriebsdaten'
description: Hier erfahren Sie, wie Sie ETL-Pipelines mit Azure HDInsight erstellen, um mithilfe von bedarfsgesteuerten Spark-Clustern und Power BI Erkenntnisse aus Vertriebsdaten zu gewinnen.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: tutorial
ms.date: 09/30/2019
ms.author: hrasheed
ms.openlocfilehash: 52509f886ac9882c372de401ca163ccef418837f
ms.sourcegitcommit: 8bae7afb0011a98e82cbd76c50bc9f08be9ebe06
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2019
ms.locfileid: "71695705"
---
# <a name="tutorial-create-an-end-to-end-data-pipeline-to-derive-sales-insights"></a>Tutorial: Erstellen einer End-to-End-Datenpipeline zum Gewinnen von Erkenntnissen aus Vertriebsdaten

In diesem Tutorial erstellen Sie eine End-to-End-Datenpipeline, die Extrahierungs-, Transformations- und Ladevorgänge durchführt. Die Pipeline verwendet in Azure HDInsight ausgeführte Apache Spark- und Apache Hive-Cluster zum Abfragen und Bearbeiten der Daten. Weitere verwendete Technologien sind Data Lake Storage Gen2 für die Datenspeicherung und Power BI für die Visualisierung.

Die Datenpipeline kombiniert die Daten aus all den unterschiedlichen Speichern, entfernt unerwünschte Daten, fügt neue Daten an und lädt sie dann wieder in den Speicher, um geschäftliche Erkenntnisse zu visualisieren. Weitere Informationen zu ETL-Pipelines finden Sie unter [Bedarfsorientiertes Extrahieren, Transformieren und Laden (ETL)](./hadoop/apache-hadoop-etl-at-scale.md).

![ETL-Architektur](./media/hdinsight-sales-insights-etl/architecture.png)

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

Laden Sie [Power BI Desktop](https://www.microsoft.com/download/details.aspx?id=45331) herunter, um am Ende dieses Tutorials geschäftliche Erkenntnisse zu visualisieren.

## <a name="create-resources"></a>Erstellen von Ressourcen

### <a name="clone-the-repository-with-scripts-and-data"></a>Klonen des Repositorys mit Skripts und Daten

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)
1. Öffnen Sie Cloud Shell über die obere Menüleiste. Wählen Sie Ihr Abonnement zum Erstellen einer Dateifreigabe aus, wenn Sie von Azure Cloud Shell dazu aufgefordert werden.

    ![Öffnen von Azure Cloud Shell](./media/hdinsight-sales-insights-etl/hdinsight-sales-insights-etl-click-cloud-shell.png)
1. Wählen Sie im Dropdownmenü „Umgebung auswählen“ die Option „Bash“ aus.
1. Melden Sie sich bei Ihrem Azure-Konto an, und wählen Sie das Abonnement aus. 
1. Richten Sie die Ressourcengruppe für das Projekt ein.
    1. Wählen Sie einen eindeutigen Namen für die Ressourcengruppe aus.
    1. Führen Sie den folgenden Codeausschnitt in Azure Cloud Shell aus, um Variablen festzulegen, die in späteren Schritten verwendet werden.

        ```azurecli-interactive 
        resourceGroup="<RESOURCE GROUP NAME>"
        subscriptionID="<SUBSCRIPTION ID>"
        
        az account set --subscription $subscriptionID
        az group create --name $resourceGroup --location westus
        ```

1. Laden Sie die Daten und Skripts für dieses Tutorial aus dem [Repository für das HDInsight-ETL-Szenario mit Vertriebsdaten](https://github.com/Azure-Samples/hdinsight-sales-insights-etl) herunter, indem Sie die folgenden Befehle an der Cloud Shell-Eingabeaufforderung eingeben:

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

1. Fügen Sie Ausführungsberechtigungen für das Skript `chmod +x scripts/*.sh` hinzu.
1. Führen Sie das Skript mit dem Befehl `./scripts/resources.sh <RESOURCE_GROUP_NAME> <LOCATION>` aus, um die folgenden Ressourcen in Azure bereitzustellen:

    1. Ein Azure Blob Storage-Konto: Dieses Konto enthält die Vertriebsdaten des Unternehmens.
    2. Ein Azure Data Lake Storage Gen2-Konto: Dieses Konto dient als Speicherkonto für beide HDInsight-Cluster. Weitere Informationen zu HDInsight und Data Lake Storage Gen2 finden Sie unter [Azure HDInsight-Integration in Data Lake Storage Gen2](https://azure.microsoft.com/blog/azure-hdinsight-integration-with-data-lake-storage-gen-2-preview-acl-and-security-update/).
    3. Eine vom Benutzer zugewiesene verwaltete Identität: Dieses Konto gewährt den HDInsight-Clustern Zugriff auf das Data Lake Storage Gen2-Konto.
    4. Einen Apache Spark-Cluster: Dieser Cluster wird zum Bereinigen und Transformieren der Rohdaten verwendet.
    5. Einen Interactive Query-Cluster in Apache Hive: Mit diesem Cluster können die Vertriebsdaten abgefragt und mit Power BI visualisiert werden.
    6. Ein virtuelles Azure-Netzwerk, das von NSG-Regeln (Netzwerksicherheitsgruppe) unterstützt wird: Dieses virtuelle Netzwerk ermöglicht die Kommunikation der Cluster und sichert zudem ihre Kommunikation. 

Die Erstellung des Clusters dauert ungefähr 20 Minuten.

Das Skript `resources.sh` enthält den folgenden Befehl, der mithilfe einer Resource Manager-Vorlage (`resourcestemplate.json`) die angegebenen Ressourcen mit der gewünschten Konfiguration erstellt.

```azurecli-interactive 
az group deployment create --name ResourcesDeployment \
    --resource-group $resourceGroup \
    --template-file resourcestemplate.json \
    --parameters "@resourceparameters.json"
```

Das Skript `resources.sh` lädt mit dem folgenden Befehl auch die CSV-Dateien mit den Vertriebsdaten in das neu erstellte Blob Storage-Konto hoch.

```
az storage blob upload-batch -d rawdata \
    --account-name <BLOB STORAGE NAME> -s ./ --pattern *.csv
```

Das Standardkennwort für den SSH-Zugriff auf die Cluster ist `Thisisapassword1`. Wenn Sie das Kennwort ändern möchten, navigieren Sie zur Datei `resourcesparameters.json`, und ändern Sie das Kennwort für die Parameter `sparksshPassword`, `sparkClusterLoginPassword`, `llapClusterLoginPassword` und `llapsshPassword`.

### <a name="verify-deployment-and-collect-resource-information"></a>Überprüfen der Bereitstellung und Sammeln von Ressourceninformationen

1. Um den Status der Bereitstellung zu überprüfen, navigieren Sie im Azure-Portal zur Ressourcengruppe. Klicken Sie unter **Einstellungen** auf **Bereitstellungen**. Klicken Sie auf den Namen Ihrer Bereitstellung (`ResourcesDeployment`). Hier sehen Sie die Ressourcen, die erfolgreich bereitgestellt wurden, und die Ressourcen, deren Bereitstellung noch läuft.
1. Öffnen Sie nach Abschluss der Bereitstellung das Azure-Portal, und navigieren Sie zu **Ressourcengruppen** > <RESSOURCENGRUPPENNAME>.
1. Suchen Sie nach dem neuen Azure-Speicherkonto, das zum Speichern der Vertriebsdateien erstellt wurde. Der Name des Speicherkontos beginnt mit `blob`, gefolgt von einer zufälligen Zeichenfolge. 
    1. Notieren Sie den Namen des Speicherkontos zur späteren Verwendung.
    1. Klicken Sie auf den Namen des Blob Storage-Kontos.
    1. Klicken Sie links im Portal unter **Einstellungen** auf **Zugriffsschlüssel**.
    1. Kopieren Sie die Zeichenfolge im Feld **Schlüssel1**, und speichern Sie sie zur späteren Verwendung.
1. Suchen Sie nach dem Data Lake Storage Gen2-Konto, das als Speicher für die HDInsight-Cluster erstellt wurde. Dieses Konto befindet sich in derselben Ressourcengruppe wie das Blob Storage-Konto, beginnt jedoch mit `adlsgen2`.
    1. Notieren Sie den Namen des Data Lake Storage Gen2-Kontos.
    1. Klicken Sie auf den Namen des Data Lake Storage Gen2-Kontos.
    1. Klicken Sie links im Portal unter **Einstellungen** auf **Zugriffsschlüssel**.
    1. Kopieren Sie die Zeichenfolge im Feld **Schlüssel1**, und speichern Sie sie zur späteren Verwendung.

> [!Note]
> Wenn Sie die Namen der Speicherkonten kennen, können Sie auch die Kontoschlüssel abrufen. Führen Sie dazu den folgenden Befehl an der Azure Cloud Shell-Eingabeaufforderung aus:
> ```azurecli-interactive
> az storage account keys list \
>    --account-name <STORAGE NAME> \
>    --resource-group $rg \
>    --output table
> ```

### <a name="create-an-azure-data-factory"></a>Erstellen einer Azure Data Factory-Instanz

Azure Data Factory (ADF) ist ein Tool, mit dem Sie Azure Pipelines automatisieren können. Es ist nicht die einzige Möglichkeit, um diese Aufgaben durchzuführen, eignet sich aber hervorragend zum Automatisieren dieser Prozesse. Weitere Informationen zu Azure Data Factory finden Sie in der [Dokumentation zu Azure Data Factory](https://azure.microsoft.com/services/data-factory/). 

Diese Azure Data Factory-Instanz enthält eine Pipeline mit zwei Aktivitäten: 

1. Die erste Aktivität kopiert die Daten aus dem Azure Blob Storage-Konto in das Data Lake Storage Gen2-Speicherkonto, um die Datenerfassung zu simulieren.
2. Die zweite Aktivität transformiert die Daten im Spark-Cluster. Das Skript transformiert die Daten, indem unerwünschte Spalten entfernt werden und eine neue Spalte angefügt wird, die den durch eine einzelne Transaktion generierten Umsatz berechnet.

Führen Sie das Skript `adf.sh` aus, um Ihre Azure Data Factory-Pipeline einzurichten:

1. Fügen Sie mithilfe von `chmod +x adf.sh` Ausführungsberechtigungen für die Datei hinzu.
1. Führen Sie das Skript mit `./adf.sh` aus. 

Das Skript führt folgende Schritte aus:

1. Es erstellt einen Dienstprinzipal mit `Storage Blob Data Contributor`-Berechtigungen für das Data Lake Storage Gen2-Speicherkonto.
1. Es ruft ein Authentifizierungstoken zum Autorisieren von POST-Anforderungen an die [REST-API für das Data Lake Storage Gen2-Dateisystem](https://docs.microsoft.com/rest/api/storageservices/datalakestoragegen2/filesystem/create) ab.
1. Es trägt in den Dateien `sparktransform.py` und `query.hql` den tatsächlichen Namen Ihres Data Lake Storage Gen2-Speicherkontos ein.
1. Es ruft Speicherschlüssel für die Data Lake Storage Gen2- und Blob Storage-Konten ab.
1. Es erstellt eine weitere Ressourcenbereitstellung, um eine Azure Data Factory-Pipeline mit den zugehörigen verknüpften Diensten und Aktivitäten zu erstellen. Es übergibt die Speicherschlüssel als Parameter an die Vorlagendatei, sodass die verknüpften Dienste korrekt auf die Speicherkonten zugreifen können.

Die ADF-Pipeline wird mit dem folgenden Befehl bereitgestellt:

```azurecli-interactive
az group deployment create --name ADFDeployment \
    --resource-group $resourceGroup \
    --template-file adftemplate.json \
    --parameters "@adfparameters.json"
```

## <a name="run-the-data-pipeline"></a>Ausführen der Datenpipeline

### <a name="trigger-the-adf-activities"></a>Auslösen der ADF-Aktivitäten

Die erste Aktivität in der von Ihnen erstellten ADF-Pipeline verschiebt die Daten aus Blobspeicher in Data Lake Storage Gen2. Die zweite Aktivität wendet die Spark-Transformationen auf die Daten an und speichert die transformierten CSV-Dateien an einem neuen Speicherort. Die Ausführung der gesamten Pipeline kann einige Minuten dauern.

Sie können die Pipelines wie folgt auslösen:

1. Führen Sie die folgenden Befehle in PowerShell aus, um die ADF-Pipelines auszulösen. 

    ```powershell
    Invoke-AzDataFactoryV2Pipeline -DataFactory $df -PipelineName "CopyPipeline_k8z" 
    Invoke-AzDataFactoryV2Pipeline -DataFactory $df -PipelineName "sparkTransformPipeline"
    ```

1. Sie können auch im Portal die Data Factory öffnen, „Erstellen und überwachen“ auswählen und dann die Kopierpipeline und anschließend die Spark-Pipeline auslösen. Informationen zum Auslösen von Pipelines über das Portal finden Sie unter [Erstellen bedarfsgesteuerter Apache Hadoop-Cluster in HDInsight mit Azure Data Factory](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-create-linux-clusters-adf#trigger-a-pipeline).

Um zu überprüfen, ob die Pipelines ausgeführt wurden, können Sie einen der beiden folgenden Schritte ausführen:

1. Navigieren Sie im Portal zum Abschnitt **Überwachen** in Ihrer Azure Data Factory-Instanz.
2. Wechseln Sie im Speicher-Explorer zu Ihrem Data Lake Storage Gen2-Speicherkonto, navigieren Sie zum Dateisystem `files` und dann zum Ordner `transformed`, und überprüfen Sie den Inhalt, um festzustellen, ob die Pipeline erfolgreich ausgeführt wurde.

Weitere Möglichkeiten zum Transformieren von Daten mithilfe von HDInsight finden Sie in diesem Artikel zur Verwendung von [Jupyter Notebook](https://docs.microsoft.com/azure/hdinsight/spark/apache-spark-load-data-run-query).

### <a name="create-a-table-on-the-interactive-query-cluster-to-view-data-on-power-bi"></a>Erstellen einer Tabelle im Interactive Query-Cluster zum Anzeigen von Daten in Power BI

1. Kopieren Sie die Datei „query.hql“ mit SCP in den LLAP-Cluster:

    ```
    scp scripts/query.hql sshuser@<clustername>-ssh.azurehdinsight.net:/home/sshuser/
    ```

2. Stellen Sie mit dem folgenden Befehl eine SSH-Verbindung mit dem LLAP-Cluster her, und geben Sie dann Ihr Kennwort ein. Wenn Sie die Datei `resourcesparameters.json` nicht geändert haben, lautet das Kennwort `Thisisapassword1`.

    ```
    ssh sshuser@<clustername>-ssh.azurehdinsight.net
    ```

3. Geben Sie den folgenden Befehl ein, um das Skript auszuführen.

    ```
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -f query.hql
    ```

Dieses Skript erstellt eine verwaltete Tabelle im Interactive Query-Cluster, auf die Sie über Power BI zugreifen können. 

### <a name="create-a-power-bi-dashboard-from-sales-data"></a>Erstellen eines Power BI-Dashboards anhand von Vertriebsdaten

1. Öffnen Sie Power BI Desktop.
1. Wählen Sie **Daten abrufen** aus.
1. Suchen Sie nach **HDInsight-Interactive Query-Cluster**.
1. Fügen Sie dort den URI für Ihren Cluster ein. Er sollte das Format `https://<LLAP CLUSTER NAME>.azurehdinsight.net` haben. Geben Sie `default` für die Datenbank ein.
1. Geben Sie den Benutzernamen und das Kennwort für den Zugriff auf den Cluster ein.

Nachdem die Daten geladen wurden, können Sie mit dem Dashboard experimentieren, das Sie erstellen möchten. Informationen zu den ersten Schritten mit Power BI-Dashboards finden Sie unter den folgenden Links:

* [Einführung in Dashboards für Power BI-Designer](https://docs.microsoft.com/power-bi/service-dashboards)
* [Tutorial: Erste Schritte mit dem Power BI-Dienst](https://docs.microsoft.com/power-bi/service-get-started)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie diese Anwendung nicht weiterverwenden möchten, löschen Sie wie folgt alle zugehörigen Ressourcen, damit sie Ihnen nicht in Rechnung gestellt werden.

```azurecli-interactive 
az group delete -n $resourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Bedarfsorientiertes Extrahieren, Transformieren und Laden (ETL)](./hadoop/apache-hadoop-etl-at-scale.md)
