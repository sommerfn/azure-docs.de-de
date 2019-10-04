---
title: Implementieren von Azure Databricks mit einem Cosmos DB-Endpunkt
description: Dieses Tutorial beschreibt, wie Sie Azure Databricks mit einem für Cosmos DB aktivierten Dienstendpunkt in ein virtuelles Netzwerk implementieren.
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.topic: tutorial
ms.date: 04/17/2019
ms.openlocfilehash: d1268ea2cfc22e6350edb32230588a497be8bc79
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67054476"
---
# <a name="tutorial-implement-azure-databricks-with-a-cosmos-db-endpoint"></a>Tutorial: Implementieren von Azure Databricks mit einem Cosmos DB-Endpunkt

Dieses Tutorial beschreibt, wie Sie eine Databricks-Umgebung mit eingeschleustem VNET mithilfe eines für Cosmos DB aktivierten Dienstendpunkts implementieren.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines Azure Databricks-Arbeitsbereichs in einem virtuellen Netzwerk
> * Erstellen eines Cosmos DB-Dienstendpunkts
> * Erstellen eines Cosmos DB-Kontos und Importieren von Daten
> * Erstellen eines Azure Databricks-Clusters
> * Abfragen von Cosmos DB über ein Azure Databricks-Notebook

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie zunächst folgende Schritte aus:

* Erstellen Sie einen [Azure Databricks-Arbeitsbereich in einem virtuellen Netzwerk](quickstart-create-databricks-workspace-vnet-injection.md).

* Laden Sie den [Spark-Connector](https://search.maven.org/remotecontent?filepath=com/microsoft/azure/azure-cosmosdb-spark_2.4.0_2.11/1.3.4/azure-cosmosdb-spark_2.4.0_2.11-1.3.4-uber.jar) herunter.

* Laden Sie Beispieldaten von [NOAA National Centers for Environmental Information](https://www.ncdc.noaa.gov/stormevents/) herunter. Wählen Sie ein Bundesland oder einen Bereich aus, und wählen Sie dann **Suchen** aus. Übernehmen Sie auf der nächsten Seite die Standardeinstellungen, und wählen Sie **Suchen** aus. Wählen Sie dann links auf der Seite die Option **CSV-Download** aus, um die Ergebnisse herunterzuladen.

* Laden Sie die [vorkompilierte Binärdatei](https://aka.ms/csdmtool) des Azure Cosmos DB-Datenmigrationstools herunter.

## <a name="create-a-cosmos-db-service-endpoint"></a>Erstellen eines Cosmos DB-Dienstendpunkts

1. Nachdem Sie einen Azure Databricks-Arbeitsbereich in einem virtuellen Netzwerk bereitgestellt haben, navigieren Sie im [Azure-Portal](https://portal.azure.com) zu diesem virtuellen Netzwerk. Beachten Sie die öffentlichen und privaten Subnetze, die durch die Bereitstellung von Databricks erstellt wurden.

   ![Subnetze des virtuellen Netzwerks](./media/service-endpoint-cosmosdb/virtual-network-subnets.png)

2. Wählen Sie das *öffentliche Subnetz* aus, und erstellen Sie einen Cosmos DB-Dienstendpunkt. **Speichern** Sie anschließend.
   
   ![Hinzufügen eines Cosmos DB-Dienstendpunkts](./media/service-endpoint-cosmosdb/add-cosmosdb-service-endpoint.png)

## <a name="create-a-cosmos-db-account"></a>Erstellen eines Cosmos DB-Kontos

1. Öffnen Sie das Azure-Portal. Wählen Sie oben links auf dem Bildschirm die Option **Ressource erstellen > Datenbanken > Azure Cosmos DB**.

2. Füllen Sie die **Instanzdetails** auf der Registerkarte **Grundlagen** mit den folgenden Einstellungen aus:

   |Einstellung|Wert|
   |-------|-----|
   |Abonnement|*Ihr Abonnement*|
   |Ressourcengruppe|*Ihre Ressourcengruppe*|
   |Kontoname|db-vnet-service-endpoint|
   |API|Core (SQL)|
   |Location|USA (Westen)|
   |Georedundanz|Disable|
   |Schreibvorgänge in mehreren Regionen|Aktivieren|

   ![Hinzufügen eines Cosmos DB-Dienstendpunkts](./media/service-endpoint-cosmosdb/create-cosmosdb-account-basics.png)

3. Wählen Sie die Registerkarte **Netzwerk** aus, und konfigurieren Sie Ihr virtuelles Netzwerk. 

   a. Wählen Sie das virtuelle Netzwerk, das Sie als Voraussetzung erstellt haben, und wählen Sie dann *public-subnet* aus. Beachten Sie, dass *private-subnet* den Hinweis *Der Endpunkt „Microsoft AzureCosmosDB“ fehlt* aufweist. Dies liegt daran, dass Sie nur den Cosmos DB-Dienstendpunkt in *public-subnet* aktiviert haben.

   b. Vergewissern Sie sich, dass **Zulassen des Zugriffs vom Azure-Portal** aktiviert ist. Mit dieser Einstellung können Sie im Azure-Portal auf Ihr Cosmos DB-Konto zugreifen. Wenn diese Option auf **Verweigern** festgelegt ist, treten beim Versuch, auf Ihr Konto zuzugreifen, Fehler auf. 

   > [!NOTE]
   > Die Option ist für dieses Tutorial zwar nicht erforderlich, aber Sie können auch *Zugriff über meine IP-Adresse zulassen* aktivieren, wenn Sie über Ihren lokalen Computer auf Ihr Cosmos DB-Konto zugreifen möchten. Wenn Sie beispielsweise mithilfe des Cosmos DB SDK eine Verbindung mit Ihrem Konto herstellen, müssen Sie diese Einstellung aktivieren. Wenn sie deaktiviert ist, erhalten Sie Fehler vom Typ „Zugriff verweigert“.

   ![Netzwerkeinstellungen für Cosmos DB-Konto](./media/service-endpoint-cosmosdb/create-cosmosdb-account-network.png)

4. Wählen Sie **Überprüfen + erstellen** und dann **Erstellen**, um Ihr Cosmos DB-Konto innerhalb des virtuellen Netzwerks zu erstellen.

5. Sobald Ihr Cosmos DB-Konto erstellt wurde, navigieren Sie zu unter **Einstellungen** zu **Schlüssel**. Kopieren Sie die primäre Verbindungszeichenfolge, und speichern Sie sie zur späteren Verwendung in einem Texteditor.

    ![Schlüsselseite des Cosmos DB-Kontos](./media/service-endpoint-cosmosdb/cosmos-keys.png)

6. Wählen Sie **Data Explorer** und **neue Sammlung** aus, um Ihrem Cosmos DB-Konto eine neue Datenbank und Sammlung hinzuzufügen.

    ![Neue Cosmos DB-Sammlung](./media/service-endpoint-cosmosdb/new-collection.png)

## <a name="upload-data-to-cosmos-db"></a>Hochladen von Daten in Cosmos DB

1. Öffnen Sie die GUI-Version des [Datenmigrationstools für Cosmos DB](https://aka.ms/csdmtool), **Dtui.exe**.

    ![Datenmigrationstool von Cosmos DB](./media/service-endpoint-cosmosdb/cosmos-data-migration-tool.png)

2. Wählen Sie auf der Registerkarte **Quellinformationen** in der Dropdownliste **Importieren aus** die Option **CSV-Datei(en)** aus. Wählen Sie dann **Dateien hinzufügen** aus, und fügen Sie die Sturmdaten-CSV hinzu, die Sie als Voraussetzung heruntergeladen haben.

    ![Quellinformationen für Datenmigrationstool von Cosmos DB](./media/service-endpoint-cosmosdb/cosmos-source-information.png)

3. Geben Sie auf der Registerkarte **Zielinformationen** Ihre Verbindungszeichenfolge ein. Das Format der Verbindungszeichenfolge ist `AccountEndpoint=<URL>;AccountKey=<key>;Database=<database>`. „AccountEndpoint“ und „AccountKey“ sind in der primären Verbindungszeichenfolge enthalten, die Sie im vorherigen Abschnitt gespeichert haben. Fügen Sie `Database=<your database name>` am Ende der Verbindungszeichenfolge an, und wählen Sie **Überprüfen** aus. Fügen Sie anschließend den Sammlungsnamen und den Partitionsschlüssel hinzu.

    ![Zielinformationen für Datenmigrationstool von Cosmos DB](./media/service-endpoint-cosmosdb/cosmos-target-information.png)

4. Wählen Sie **Weiter** aus, bis Sie zur Zusammenfassungsseite gelangen. Wählen Sie dann **Importieren** aus.

## <a name="create-a-cluster-and-add-library"></a>Erstellen eines Clusters und Hinzufügen einer Bibliothek

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrer Instanz des Azure Databricks-Diensts, und wählen Sie **Arbeitsbereich starten** aus.

   ![Databricks-Arbeitsbereich starten](./media/service-endpoint-cosmosdb/launch-workspace.png)

2. Erstellen Sie einen neuen Cluster. Wählen Sie einen Clusternamen, und übernehmen Sie die restlichen Standardeinstellungen.

   ![Einstellungen des neuen Clusters](./media/service-endpoint-cosmosdb/create-cluster.png)

3. Nachdem der Cluster erstellt wurde, navigieren Sie zur Clusterseite und wählen die Registerkarte **Bibliotheken** aus. Wählen Sie **Neue installieren** aus, und laden Sie die JAR-Datei des Spark-Connectors hoch, um die Bibliothek zu installieren.

    ![Installieren der Spark-Connector-Bibliothek](./media/service-endpoint-cosmosdb/install-cosmos-connector-library.png)

    Auf der Registerkarte **Bibliotheken** können Sie überprüfen, ob die Bibliothek installiert wurde.

    ![Registerkarte „Bibliotheken“ des Databricks-Clusters](./media/service-endpoint-cosmosdb/installed-library.png)

## <a name="query-cosmos-db-from-a-databricks-notebook"></a>Abfragen von Cosmos DB über ein Databricks-Notebook

1. Wechseln Sie zu Ihrem Azure Databricks-Arbeitsbereich, und erstellen Sie ein neues Python-Notebook.

    ![Erstellen eines neuen Databricks-Notebooks](./media/service-endpoint-cosmosdb/new-python-notebook.png)

2. Führen Sie den folgenden Python Code aus, um die Konfiguration der Cosmos DB-Verbindung festzulegen. Ändern Sie den **Endpunkt**, den **Hauptschlüssel**, die **Datenbank** und die **Sammlung** entsprechend.

    ```python
    connectionConfig = {
      "Endpoint" : "https://<your Cosmos DB account name.documents.azure.com:443/",
      "Masterkey" : "<your Cosmos DB primary key>",
      "Database" : "<your database name>",
      "preferredRegions" : "West US 2",
      "Collection": "<your collection name>",
      "schema_samplesize" : "1000",
      "query_pagesize" : "200000",
      "query_custom" : "SELECT * FROM c"
    }
    ```

3. Verwenden Sie den folgenden Python-Code zum Laden der Daten und zum Erstellen einer temporäre Ansicht.

    ```python
    users = spark.read.format("com.microsoft.azure.cosmosdb.spark").options(**connectionConfig).load()
    users.createOrReplaceTempView("storm")
    ```

4. Verwenden Sie den folgenden Magic-Befehl zum Ausführen einer SQL­-Anweisung, die Daten zurückgibt.

    ```python
    %sql
    select * from storm
    ```

    Sie haben Ihren Databricks-Arbeitsbereich mit eingeschleustem VNET erfolgreich mit einer Dienstendpunkt-fähigen Cosmos DB-Ressource verbunden. Weitere Informationen zum Herstellen einer Verbindung mit Cosmos DB finden Sie unter [Azure Cosmos DB Connector for Apache Spark](https://github.com/Azure/azure-cosmosdb-spark) (Azure Cosmos DB-Connector für Apache Spark).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe, den Azure Databricks-Arbeitsbereich und alle dazugehörigen Ressourcen, wenn Sie sie nicht mehr benötigen. Das Löschen des Auftrags vermeidet unnötige Abrechnung. Wenn Sie planen, den Azure Databricks-Arbeitsbereich in Zukunft zu nutzen, können Sie den Cluster stoppen und später neu starten. Wenn Sie diesen Azure Databricks-Arbeitsbereich nicht mehr verwenden möchten, löschen Sie alle Ressourcen, die im Rahmen dieses Tutorials erstellt wurden, mithilfe der folgenden Schritte:

1. Klicken Sie im Azure-Portal im Menü auf der linken Seite auf **Ressourcengruppen**, und klicken Sie auf den Namen der erstellten Ressourcengruppe.

2. Klicken Sie auf der Seite mit Ihrer Ressourcengruppe auf **Löschen**, geben Sie im Textfeld den Namen der zu löschenden Ressource ein, und klicken Sie dann erneut auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie einen Azure Databricks-Arbeitsbereich in einem virtuellen Netzwerk bereitgestellt und den Cosmos DB Spark-Connector zum Abfragen von Cosmos DB-Daten von Databricks verwendet. Fahren Sie mit dem Tutorial zum Verwenden von SQL Server mit Azure Databricks fort, um mehr zum Arbeiten mit Azure Databricks in einem virtuellen Netzwerk zu erfahren.

> [!div class="nextstepaction"]
> [Tutorial: Abfragen eines Linux-Docker-Containers für SQL Server in einem virtuellen Netzwerk aus einem Azure Databricks-Notebook](vnet-injection-sql-server.md)
