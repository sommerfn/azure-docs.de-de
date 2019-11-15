---
title: Erstellen einer Azure Data Factory-Instanz über die Azure Data Factory-Benutzeroberfläche
description: Erstellen Sie eine Data Factory-Instanz mit einer Pipeline, die Daten von einem Speicherort im Azure Blob-Speicher an einen anderen Speicherort kopiert.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: quickstart
ms.date: 06/20/2018
ms.author: jingwang
ms.openlocfilehash: 642039e99df19939def5ac850f1abccca0e67222
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73684483"
---
# <a name="quickstart-create-a-data-factory-by-using-the-azure-data-factory-ui"></a>Schnellstart: Erstellen einer Data Factory über die Azure Data Factory-Benutzeroberfläche

> [!div class="op_single_selector" title1="Wählen Sie die von Ihnen verwendete Version des Data Factory-Diensts aus:"]
> * [Version 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Aktuelle Version](quickstart-create-data-factory-portal.md)

Diese Schnellstartanleitung beschreibt, wie Sie mithilfe der Azure Data Factory-Benutzeroberfläche eine Data Factory erstellen und überwachen. Die in dieser Data Factory erstellte Pipeline *kopiert* Daten aus einem Ordner in einen anderen Ordner in Azure Blob Storage. Ein Tutorial zum *Transformieren* von Daten mithilfe von Azure Data Factory finden Sie unter [Tutorial: Transformieren von Daten mithilfe von Spark](tutorial-transform-data-spark-portal.md).

> [!NOTE]
> Wenn Sie mit Azure Data Factory nicht vertraut sind, lesen Sie vor der Durchführung dieses Schnellstarts die Informationen unter [Einführung in Azure Data Factory](data-factory-introduction.md). 

[!INCLUDE [data-factory-quickstart-prerequisites](../../includes/data-factory-quickstart-prerequisites.md)] 

### <a name="video"></a>Video 
Dieses Video enthält Informationen zur Data Factory-Benutzeroberfläche: 
>[!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Visually-build-pipelines-for-Azure-Data-Factory-v2/Player]

## <a name="create-a-data-factory"></a>Erstellen einer Data Factory

1. Starten Sie den Webbrowser **Microsoft Edge** oder **Google Chrome**. Die Data Factory-Benutzeroberfläche wird zurzeit nur in den Webbrowsern Microsoft Edge und Google Chrome unterstützt.
1. Öffnen Sie das [Azure-Portal](https://portal.azure.com). 
1. Wählen Sie im Menü des Azure-Portals die Option **Ressource erstellen** aus.
   
   ![Auswählen von „Ressource erstellen“ im Menü des Azure-Portals](./media/doc-common-process/create-a-resource.png)
1. Wählen Sie **Analytics** und dann **Data Factory** aus. 
   
   ![Auswählen von „Data Factory“ im Bereich „Neu“](./media/doc-common-process/new-azure-data-factory-menu.png)
1. Geben Sie auf der Seite **Neue Data Factory** unter **Name** den Namen **ADFTutorialDataFactory** ein. 
 
   Der Name der Azure Data Factory muss *global eindeutig*sein. Sollte der folgende Fehler auftreten, ändern Sie den Namen der Data Factory (beispielsweise in **&lt;IhrName&gt;ADFTutorialDataFactory**), und wiederholen Sie den Vorgang. Benennungsregeln für Data Factory-Artefakte finden Sie im Artikel [Azure Data Factory – Benennungsregeln](naming-rules.md).
  
   ![Fehler, wenn ein Name nicht verfügbar ist](./media/doc-common-process/name-not-available-error.png)
1. Wählen Sie unter **Abonnement** Ihr Azure-Abonnement aus, in dem die Data Factory erstellt werden soll. 
1. Führen Sie unter **Ressourcengruppe** einen der folgenden Schritte aus:
     
   - Wählen Sie die Option **Use existing** (Vorhandene verwenden) und dann in der Dropdownliste eine vorhandene Ressourcengruppe. 
   - Wählen Sie **Neu erstellen**, und geben Sie den Namen einer Ressourcengruppe ein.   
         
   Weitere Informationen über Ressourcengruppen finden Sie unter [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen](../azure-resource-manager/resource-group-overview.md).  
1. Wählen Sie **V2** als **Version** aus.
1. Wählen Sie unter **Standort** den Standort für die Data Factory aus.

   In der Liste werden nur Standorte angezeigt, die von Data Factory unterstützt werden und an denen Ihre Azure Data Factory-Metadaten gespeichert werden. Die von Data Factory verwendeten zugeordneten Datenspeicher (z. B. Azure Storage und Azure SQL-Datenbank) und Computedienste (z. B. Azure HDInsight) können in anderen Regionen ausgeführt werden.

1. Klicken Sie auf **Erstellen**.

1. Nach Abschluss der Erstellung wird die Seite **Data Factory** angezeigt. Klicken Sie auf die Kachel **Erstellen und überwachen**, um die Anwendung für die Azure Data Factory-Benutzeroberfläche (User Interface, UI) auf einer separaten Registerkarte zu starten.
   
   ![Startseite der Data Factory mit der Kachel „Erstellen und überwachen“](./media/doc-common-process/data-factory-home-page.png)
1. Wechseln Sie auf der Seite **Erste Schritte** im linken Bereich zur Registerkarte **Autor**. 

    ![Seite „Erste Schritte“](./media/quickstart-create-data-factory-portal/get-started-page.png)

## <a name="create-a-linked-service"></a>Erstellen eines verknüpften Diensts
In diesem Verfahren erstellen Sie einen verknüpften Dienst, der Ihr Azure-Speicherkonto mit der Data Factory verbindet. Der verknüpfte Dienste enthält die Verbindungsinformationen, die der Data Factory-Dienst zur Laufzeit zur Verbindungsherstellung verwendet.

1. Wählen Sie **Verbindungen** und anschließend auf der Symbolleiste die Schaltfläche **Neu** aus. (Die Schaltfläche **Verbindungen** befindet sich unten in der linken Spalte unter **Factory Resources** (Factory-Ressourcen)). 

1. Wählen Sie auf der Seite **Neuer verknüpfter Dienst** die Option **Azure Blob Storage**, und klicken Sie dann auf **Weiter**. 

   ![Auswählen der Kachel „Azure Blob Storage“](./media/quickstart-create-data-factory-portal/select-azure-blob-linked-service.png)
1. Führen Sie auf der Seite „New Linked Service (Azure Blob Storage)“ (Neuer verknüpfter Dienst (Azure Blob Storage)) die folgenden Schritte aus: 

   a. Geben Sie unter **Name** den Namen **AzureStorageLinkedService** ein.

   b. Wählen Sie unter **Speicherkontoname** den Namen Ihres Azure-Speicherkontos aus.

   c. Klicken Sie auf **Verbindung testen**, um zu überprüfen, ob der Data Factory-Dienst eine Verbindung mit dem Speicherkonto herstellen kann. 

   d. Wählen Sie **Fertig stellen**, um den verknüpften Dienst zu speichern. 

## <a name="create-datasets"></a>Erstellen von Datasets
In diesem Verfahren erstellen Sie zwei Datasets: **InputDataset** und **OutputDataset**. Diese Datasets sind vom Typ **AzureBlob**. Sie verweisen auf den mit Azure Storage verknüpften Dienst, den Sie im vorherigen Abschnitt erstellt haben. 

Das Eingabedataset stellt die Quelldaten im Eingabeordner dar. In der Definition des Eingabedatasets geben Sie den Blobcontainer (**adftutorial**), den Ordner (**input**) und die Datei (**emp.txt**) mit den Quelldaten an. 

Das Ausgabedataset stellt die Daten dar, die zum Ziel kopiert werden. In der Definition des Ausgabedatasets geben Sie den Blobcontainer (**adftutorial**), den Ordner (**output**) und die Datei an, in die die Daten kopiert werden. Jeder Ausführung einer Pipeline wird eine eindeutige ID zugeordnet. Sie können auf diese ID mithilfe der Systemvariablen **RunId** zugreifen. Der Name der Ausgabedatei wird basierend auf der Ausführungs-ID der Pipeline dynamisch ausgewertet.   

In den Einstellungen des verknüpften Diensts haben Sie das Azure-Speicherkonto angegeben, das die Quelldaten enthält. In den Einstellungen des Quelldatasets geben Sie an, wo genau sich die Quelldaten befinden (Blobcontainer, Order und Datei). In den Einstellungen des Senkendatasets geben Sie an, wohin die Daten kopiert werden (Blobcontainer, Order und Datei). 
 
1. Klicken Sie auf die Schaltfläche **+** (Plus) und dann auf **Dataset**.

   ![Menü zum Erstellen eines Datasets](./media/quickstart-create-data-factory-portal/new-dataset-menu.png)
1. Wählen Sie auf der Seite **Neues Dataset** die Option **Azure Blob Storage** und dann **Weiter** aus. 

   ![Auswählen von „Azure Blob Storage“](./media/quickstart-create-data-factory-portal/select-azure-blob-dataset.png)
1. Wählen Sie auf der Seite **Format auswählen** den Formattyp Ihrer Daten und dann **Weiter** aus. Wählen Sie in diesem Fall **Binär** aus, wenn Dateien unverändert kopiert werden, ohne dass der Inhalt analysiert wird.

    ![Datenformattyp](./media/doc-common-process/select-binary.png)

1. Führen Sie auf der Seite **Einstellungen festlegen** die folgenden Schritte aus:

    a. Geben Sie unter **Name** den Namen **InputDataset** ein. 

    b. Wählen Sie unter **Verknüpfter Dienst** die Option **AzureStorageLinkedService**.

    c. Klicken Sie neben **Dateipfad** auf die Schaltfläche **Durchsuchen**.

    d. Navigieren Sie im Fenster **Choose a file or folder** (Datei oder Ordner auswählen) zum Ordner **input** im Container **adftutorial**, wählen Sie die Datei **emp.txt** aus, und klicken Sie auf **Fertig stellen**.
    
    e. Wählen Sie **Weiter**.   

    ![Festlegen der Eigenschaften für InputDataset](./media/quickstart-create-data-factory-portal/set-properties-for-inputdataset.png)
1. Wiederholen Sie die Schritte zum Erstellen des Ausgabedatasets:  

    a. Klicken Sie auf die Schaltfläche **+** (Plus) und dann auf **Dataset**.

    b. Wählen Sie auf der Seite **Neues Dataset** die Option **Azure Blob Storage** und dann **Weiter** aus.

    c. Wählen Sie auf der Seite **Format auswählen** den Formattyp Ihrer Daten und dann **Weiter** aus.

    d. Geben Sie auf der Seite **Eigenschaften festlegen** als Name **OutputDataset** ein. Wählen Sie **AzureStorageLinkedService** als verknüpften Dienst aus.

    e. Geben Sie unter **Dateipfad** den Pfad **adftutorial/output** ein. Wenn der Ordner **output** nicht vorhanden ist, wird er von der Copy-Aktivität zur Laufzeit erstellt.

    f. Wählen Sie **Weiter**.   

## <a name="create-a-pipeline"></a>Erstellen einer Pipeline 
In diesem Schritt erstellen und überprüfen Sie eine Pipeline mit einer Copy-Aktivität, die das Eingabe- und Ausgabedataset verwendet. Die Copy-Aktivität kopiert Daten aus der in den Einstellungen des Eingabedatasets angegebenen Datei in die Datei, die in den Einstellungen des Ausgabedatasets angegeben ist. Wenn das Eingabedataset nur einen Ordner (nicht den Dateinamen) angibt, kopiert die Copy-Aktivität alle Dateien im Quellordner ans Ziel. 

1. Klicken Sie auf die Schaltfläche **+** (Plus) und dann auf **Pipeline**. 

1. Geben Sie auf der Registerkarte **Allgemein** für **Name** den Namen **CopyPipeline** ein. 

1. Erweitern Sie in der Toolbox **Aktivitäten** die Option **Move & Transform** (Verschieben und transformieren). Ziehen Sie die **Copy Data**-Aktivität aus der Toolbox **Aktivitäten** auf die Oberfläche des Pipeline-Designers. Sie können in der Toolbox **Aktivitäten** auch nach Aktivitäten suchen. Geben Sie unter **Name** den Namen **CopyFromBlobToBlob** ein.

1. Wechseln Sie in den Einstellungen der Copy-Aktivität zur Registerkarte **Quelle**, und wählen Sie für **Quelldataset** die Option **InputDataset** aus.

1. Wechseln Sie in den Einstellungen der Copy-Aktivität zur Registerkarte **Senke**, und wählen Sie für **Senkendataset** die Option **OutputDataset** aus.

1. Klicken Sie zum Überprüfen der Pipelineeinstellungen oberhalb der Canvas auf der Symbolleiste für die Pipeline auf **Überprüfen**. Vergewissern Sie sich, dass die Pipeline überprüft wurde. Klicken Sie auf die Schaltfläche **>>** (Pfeil nach rechts), um die Ausgabe der Überprüfung zu schließen. 

## <a name="debug-the-pipeline"></a>Debuggen der Pipeline
In diesem Schritt debuggen Sie die Pipeline, bevor Sie sie in Data Factory bereitstellen. 

1. Klicken Sie oberhalb der Canvas auf der Symbolleiste für die Pipeline auf **Debuggen**, um einen Testlauf auszulösen. 
    
1. Überprüfen Sie, ob der Status der Pipelineausführung auf der Registerkarte **Ausgabe** der Pipelineeinstellungen unten angezeigt wird. 

1. Vergewissern Sie sich, dass im Ordner **output** des Containers **adftutorial** eine Ausgabedatei angezeigt wird. Ist der Ausgabeordner nicht vorhanden, wird er vom Data Factory-Dienst automatisch erstellt. 

## <a name="trigger-the-pipeline-manually"></a>Manuelles Auslösen der Pipeline
In diesem Verfahren stellen Sie Entitäten (verknüpfte Dienste, Datasets, Pipelines) in Azure Data Factory bereit. Anschließend lösen Sie manuell eine Pipelineausführung aus. 

1. Vor dem Auslösen einer Pipeline müssen Sie Entitäten in Data Factory veröffentlichen. Klicken Sie zum Veröffentlichen oben auf **Alle veröffentlichen**. 

   ![Schaltfläche "Veröffentlichen"](./media/quickstart-create-data-factory-portal/publish-button.png)
1. Klicken Sie zum manuellen Auslösen der Pipeline auf der Symbolleiste für die Pipeline auf **Trigger hinzufügen** und dann auf **Trigger Now** (Jetzt auslösen). Klicken Sie auf der Seite **Pipeline Run** (Pipelineausführung) auf **Fertig stellen**.

## <a name="monitor-the-pipeline"></a>Überwachen der Pipeline

1. Wechseln Sie im linken Bereich zur Registerkarte **Überwachen**. Aktualisieren Sie die Liste mithilfe der Schaltfläche **Aktualisieren**.

   ![Registerkarte zum Überwachen von Pipelineausführungen](./media/quickstart-create-data-factory-portal/monitor-trigger-now-pipeline.png)
1. Klicken Sie unter **Aktionen** auf den Link **View Activity Runs** (Aktivitätsausführungen anzeigen). Auf dieser Seite wird der Status der Ausführung der Copy-Aktivität angezeigt. 

1. Wenn Sie Details zum Kopiervorgang anzeigen möchten, klicken Sie in der Spalte **Aktionen** auf den Link **Details** (Brillensymbol). Einzelheiten zu den Eigenschaften finden Sie unter [Kopieraktivität in Azure Data Factory](copy-activity-overview.md). 

   ![Einzelheiten zum Kopiervorgang](./media/quickstart-create-data-factory-portal/copy-operation-details.png)
1. Vergewissern Sie sich, dass im Ordner **output** eine neue Datei enthalten ist. 
1. Wenn Sie von der Ansicht **Aktivitätsausführungen** zurück zur Ansicht **Pipelineausführungen** wechseln möchten, klicken Sie auf den Link **Pipelineausführungen**. 

## <a name="trigger-the-pipeline-on-a-schedule"></a>Auslösen der Pipeline nach einem Zeitplan
Dieser Schritt ist in diesem Tutorial optional. Sie können einen *Planer-Trigger* erstellen, um eine regelmäßige Ausführung der Pipeline (stündlich, täglich usw.) festzulegen. In diesem Schritt erstellen Sie einen Trigger, der bis zur angegebenen Endzeit (Datum und Uhrzeit) minütlich ausgeführt wird. 

1. Wechseln Sie zur Registerkarte **Autor**. 

1. Navigieren Sie zu Ihrer Pipeline, und wählen Sie auf der Symbolleiste für die Pipeline **Trigger hinzufügen** und dann **Neu/Bearbeiten** aus. 

1. Klicken Sie auf der Seite **Add Triggers** (Trigger hinzufügen) auf **Choose trigger** (Trigger auswählen) und dann auf **Neu**. 

1. Aktivieren Sie auf der Seite **Neuer Trigger** unter **Ende** die Option **On Date** (Am), geben Sie eine Endzeit an, die einige Minuten nach der aktuellen Zeit liegt, und klicken Sie auf **Übernehmen**. 

   Da für jede Pipelineausführung Gebühren anfallen, sollten zwischen Endzeit und Startzeit nur wenige Minuten liegen. Vergewissern Sie sich, dass der gleiche Tag festgelegt ist. Stellen Sie jedoch sicher, dass zwischen Veröffentlichungszeit und Endzeit ausreichend Zeit für die Pipelineausführung bleibt. Der Trigger wird erst wirksam, nachdem Sie die Lösung in Data Factory veröffentlicht haben, nicht beim Speichern des Triggers auf der Benutzeroberfläche. 

1. Aktivieren Sie auf der Seite **Neuer Trigger** das Kontrollkästchen **Aktiviert**, und wählen Sie dann **Speichern** aus. 

   ![Einstellung „Neuer Trigger“](./media/quickstart-create-data-factory-portal/trigger-settings-next.png)
1. Überprüfen Sie die Warnmeldung, und wählen Sie **Fertig stellen**.

1. Klicken Sie auf **Alle veröffentlichen**, um die Änderungen in Data Factory zu veröffentlichen. 

1. Wechseln Sie im linken Bereich zur Registerkarte **Überwachen**. Klicken Sie zum Aktualisieren der Liste auf **Aktualisieren**. Sie sehen, dass die Pipeline zwischen Veröffentlichungszeit und Endzeit minütlich ausgeführt wird. 

   Beachten Sie die Werte in der Spalte **Ausgelöst durch**. Die manuelle Triggerausführung stammt aus dem zuvor ausgeführten Schritt (**Trigger Now** (Jetzt auslösen)). 

1. Wechseln Sie zur Ansicht **Triggerausführungen**. 

1. Vergewissern Sie sich, dass für jede Pipelineausführung bis zur angegebenen Endzeit eine Ausgabedatei im Ordner **output** erstellt wird. 

## <a name="next-steps"></a>Nächste Schritte
Die Pipeline in diesem Beispiel kopiert Daten in Azure Blob Storage von einem Speicherort in einen anderen. Arbeiten Sie die [Tutorials](tutorial-copy-data-portal.md) durch, um zu erfahren, wie Sie Data Factory in anderen Szenarien verwenden können. 
