---
title: Kopieren von Daten aus SQL Server in Blob Storage mithilfe von Azure Data Factory
description: Hier erfahren Sie, wie Sie mithilfe einer selbstgehosteten Integration Runtime in Azure Data Factory Daten aus einem lokalen Datenspeicher in die Cloud kopieren.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: tutorial
ms.date: 01/11/2018
ms.author: abnarain
ms.openlocfilehash: 09768e3b9bd1c2e6c9d4a5dbe95bb270b07266c0
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73683536"
---
# <a name="copy-data-from-an-on-premises-sql-server-database-to-azure-blob-storage"></a>Kopieren von Daten aus einer lokalen SQL Server-Datenbank nach Azure Blob Storage
In diesem Tutorial verwenden Sie die Benutzeroberfläche (User Interface, UI) von Azure Data Factory, um eine Data Factory-Pipeline zu erstellen, mit der Daten aus einer lokalen SQL Server-Datenbank nach Azure Blob Storage kopiert werden. Sie erstellen und verwenden eine selbstgehostete Integration Runtime, die Daten zwischen lokalen Speichern und Clouddatenspeichern verschiebt.

> [!NOTE]
> Dieser Artikel enthält keine ausführliche Einführung in Data Factory. Weitere Informationen finden Sie unter [Einführung in Azure Data Factory](introduction.md). 

In diesem Tutorial führen Sie die folgenden Schritte aus:

> [!div class="checklist"]
> * Erstellen einer Data Factory.
> * Erstellen Sie eine selbstgehostete Integration Runtime.
> * Erstellen von verknüpften SQL Server- und Azure Storage-Diensten 
> * Erstellen von SQL Server- und Azure Blob-Datasets
> * Erstellen einer Pipeline mit Kopieraktivität zum Verschieben der Daten
> * Starten einer Pipelineausführung
> * Überwachen der Pipelineausführung.

## <a name="prerequisites"></a>Voraussetzungen
### <a name="azure-subscription"></a>Azure-Abonnement
Wenn Sie nicht bereits ein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

### <a name="azure-roles"></a>Azure-Rollen
Damit Sie Data Factory-Instanzen erstellen können, muss dem Benutzerkonto, mit dem Sie sich bei Azure anmelden, die Rolle *Mitwirkender* oder *Besitzer* zugewiesen sein, oder es muss ein *Administrator* des Azure-Abonnements sein. 

Die Berechtigungen, über die Sie im Abonnement verfügen, können Sie im Azure-Portal einsehen. Wählen Sie oben rechts Ihren Benutzernamen und dann in der Liste **Berechtigungen** aus. Wenn Sie Zugriff auf mehrere Abonnements besitzen, wählen Sie das entsprechende Abonnement aus. Beispielanleitungen zum Hinzufügen eines Benutzers zu einer Rolle, finden Sie unter [Verwalten des Zugriffs mit RBAC und dem Azure-Portal](../role-based-access-control/role-assignments-portal.md).

### <a name="sql-server-2014-2016-and-2017"></a>SQL Server 2014, 2016 und 2017
In diesem Tutorial verwenden Sie eine lokale SQL Server-Datenbank als *Quelldatenspeicher*. Die Pipeline in der in diesem Tutorial erstellten Data Factory kopiert Daten aus dieser lokalen SQL Server-Datenbank (Quelle) in Blob Storage (Senke). Anschließend erstellen Sie eine Tabelle mit dem Namen **emp** in Ihrer SQL Server-Datenbank und fügen einige Beispieleinträge in die Tabelle ein. 

1. Starten Sie SQL Server Management Studio. Falls die Suite auf Ihrem Computer noch nicht installiert wurde, rufen Sie [Herunterladen von SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) auf. 

1. Stellen Sie eine Verbindung mit Ihrer SQL Server-Instanz her, indem Sie Ihre Anmeldeinformationen verwenden. 

1. Erstellen Sie eine Beispieldatenbank. Klicken Sie in der Strukturansicht mit der rechten Maustaste auf **Datenbanken**, und wählen Sie anschließend **Neue Datenbank**. 
1. Geben Sie im Fenster **Neue Datenbank** einen Namen für die Datenbank ein, und wählen Sie dann **OK**. 

1. Führen Sie das folgende Abfrageskript für die Datenbank aus, um die Tabelle **emp** zu erstellen und einige Beispieldaten einzufügen. Klicken Sie in der Strukturansicht mit der rechten Maustaste auf die von Ihnen erstellte Datenbank, und wählen Sie anschließend **Neue Abfrage**.

   ```
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50)
    )
    GO
    
    INSERT INTO emp (FirstName, LastName) VALUES ('John', 'Doe')
    INSERT INTO emp (FirstName, LastName) VALUES ('Jane', 'Doe')
    GO
   ```

### <a name="azure-storage-account"></a>Azure-Speicherkonto
Sie verwenden in diesem Tutorial ein Azure Storage-Allzweckkonto (Blob Storage) als Ziel-/Senkendatenspeicher. Falls Sie noch nicht über ein allgemeines Azure-Speicherkonto verfügen, helfen Ihnen die Informationen unter [Erstellen Sie ein Speicherkonto](../storage/common/storage-quickstart-create-account.md) weiter. Die Pipeline in der in diesem Tutorial erstellten Data Factory kopiert Daten aus der lokalen SQL Server-Datenbank (Quelle) in die Blob Storage-Instanz (Senke). 

#### <a name="get-the-storage-account-name-and-account-key"></a>Abrufen des Speicherkontonamens und des Kontoschlüssels
In diesem Tutorial verwenden Sie Name und Schlüssel Ihres Speicherkontos. Den Namen und den Schlüssel Ihres Speicherkontos ermitteln Sie anhand der folgenden Schritte: 

1. Melden Sie sich mit Ihrem Azure-Benutzernamen und dem dazugehörigen Kennwort beim [Azure-Portal](https://portal.azure.com) an. 

1. Wählen Sie im linken Bereich **Alle Dienste** aus. Filtern Sie nach dem Schlüsselwort **Speicher**, und wählen Sie dann **Speicherkonten** aus.

    ![Speicherkontosuche](media/doc-common-process/search-storage-account.png)

1. Filtern Sie in der Liste mit den Speicherkonten ggf. nach Ihrem Speicherkonto. Wählen Sie dann Ihr Speicherkonto aus. 

1. Wählen Sie im Fenster **Speicherkonto** die Option **Zugriffsschlüssel**.

1. Kopieren Sie in den Feldern **Speicherkontoname** und **key1** die Werte, und fügen Sie sie zur späteren Verwendung im Tutorial in einen Editor ein. 

#### <a name="create-the-adftutorial-container"></a>Erstellen des Containers „adftutorial“ 
In diesem Abschnitt erstellen Sie einen Blobcontainer mit dem Namen **adftutorial** in Ihrer Blob Storage-Instanz. 

1. Wechseln Sie im Fenster **Speicherkonto** zu **Übersicht**, und wählen Sie die Option **Blobs**. 

    ![Option zum Auswählen von Blobs](media/tutorial-hybrid-copy-powershell/select-blobs.png)

1. Wählen Sie im Fenster **Blob-Dienst** die Option **Container**. 

1. Geben Sie im Fenster **Neuer Container** unter **Name** den Namen **adftutorial** ein. Wählen Sie dann **OK**aus. 

1. Wählen Sie in der Liste mit den Containern die Option **adftutorial**.

1. Lassen Sie das Fenster **Container** für **adftutorial** geöffnet. Sie überprüfen darauf am Ende des Tutorials die Ausgabe. Data Factory erstellt den Ausgabeordner automatisch in diesem Container, damit Sie ihn nicht selbst erstellen müssen.

## <a name="create-a-data-factory"></a>Erstellen einer Data Factory
In diesem Schritt erstellen Sie eine Data Factory und starten die Data Factory-Benutzeroberfläche, um eine Pipeline in der Data Factory zu erstellen. 

1. Öffnen Sie **Microsoft Edge** oder **Google Chrome**. Die Data Factory-Benutzeroberfläche wird zurzeit nur in den Webbrowsern Microsoft Edge und Google Chrome unterstützt.
1. Klicken Sie im Menü auf der linken Seite auf **+ Ressource erstellen** > **Analytics** > **Data Factory**:
   
   ![Auswählen von „Data Factory“ im Bereich „Neu“](./media/doc-common-process/new-azure-data-factory-menu.png)

1. Geben Sie auf der Seite **Neue Data Factory** unter **Name** den Namen **ADFTutorialDataFactory** ein. 

   Der Name der Data Factory muss *global eindeutig* sein. Sollte für das Feld „Name“ die folgende Fehlermeldung angezeigt werden, ändern Sie den Namen der Data Factory (beispielsweise in „<IhrName>ADFTutorialDataFactory“). Benennungsregeln für Data Factory-Artefakte finden Sie im Thema [Azure Data Factory – Benennungsregeln](naming-rules.md).

   ![Name der neuen Data Factory](./media/doc-common-process/name-not-available-error.png)

1. Wählen Sie das **Azure-Abonnement** aus, in dem die Data Factory erstellt werden soll.
1. Führen Sie unter **Ressourcengruppe** einen der folgenden Schritte aus:
   
   - Wählen Sie die Option **Use existing**(Vorhandene verwenden) und dann in der Dropdownliste eine vorhandene Ressourcengruppe.

   - Wählen Sie **Neu erstellen**, und geben Sie den Namen einer Ressourcengruppe ein.
        
     Weitere Informationen zu Ressourcengruppen finden Sie unter [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen](../azure-resource-manager/resource-group-overview.md).
1. Wählen Sie unter **Version** die Option **V2**.
1. Wählen Sie unter **Standort** den Standort für die Data Factory aus. In der Dropdownliste werden nur unterstützte Standorte angezeigt. Die Datenspeicher (etwa Storage und SQL-Datenbank) und Computeeinheiten (etwa Azure HDInsight), die von Data Factory genutzt werden, können sich in anderen Regionen befinden.
1. Klicken Sie auf **Erstellen**.

1. Nach Abschluss der Erstellung wird die Seite **Data Factory** wie in der Abbildung angezeigt:
   
    ![Data Factory-Startseite](./media/doc-common-process/data-factory-home-page.png)
1. Klicken Sie auf die Kachel **Erstellen und überwachen**, um die Data Factory-Benutzeroberfläche auf einer separaten Registerkarte zu starten. 


## <a name="create-a-pipeline"></a>Erstellen einer Pipeline

1. Wählen Sie auf der Seite **Erste Schritte** die Option **Pipeline erstellen** aus. Eine Pipeline wird automatisch für Sie erstellt. Die Pipeline wird in der Strukturansicht angezeigt, und der dazugehörige Editor wird geöffnet. 

   ![Seite „Erste Schritte“](./media/doc-common-process/get-started-page.png)

1. Geben Sie unten im Fenster **Eigenschaften** auf der Registerkarte **Allgemein** unter **Name** den Namen **SQLServerToBlobPipeline** ein.

   ![Pipelinename](./media/tutorial-hybrid-copy-portal/pipeline-name.png)

1. Erweitern Sie in der Toolbox **Aktivitäten** die Option **Move & Transform** (Verschieben und transformieren). Ziehen Sie die Aktivität **Kopieren** auf die Oberfläche zum Entwerfen von Pipelines. Legen Sie den Namen der Aktivität auf **CopySqlServerToAzureBlobActivity** fest.

1. Wechseln Sie im Fenster **Eigenschaften** zur Registerkarte **Quelle**, und klicken Sie auf **+ Neu**.

1. Suchen Sie im Dialogfeld **Neues Dataset** nach **SQL Server**. Wählen Sie **SQL Server** und dann **Weiter** aus. 

1. Geben Sie im Dialogfeld **Eigenschaften festlegen** unter **Name** den Namen **SqlServerDataset** ein. Wählen Sie unter **Verknüpfter Dienst** die Option **+ Neu** aus. In diesem Schritt erstellen Sie eine Verbindung mit dem Quelldatenspeicher (SQL Server-Datenbank). 

1. Fügen Sie im Dialogfeld **New Linked Service** (Neuer verknüpfter Dienst) als **Name** den Namen **SqlServerLinkedService** hinzu. Wählen Sie unter **Connect via integration runtime** (Verbindung per Integration Runtime herstellen) die Option **+Neu** aus.  In diesem Abschnitt erstellen Sie eine selbstgehostete Integration Runtime und ordnen sie einem lokalen Computer mit der SQL Server-Datenbank zu. Die selbstgehostete Integration Runtime ist die Komponente, die Daten aus der SQL Server-Datenbank auf Ihrem Computer in Blob Storage kopiert. 

1. Wählen Sie im Dialogfeld **Integration Runtime Setup** (Integration Runtime-Setup) die Option **Self-Hosted** (Selbstgehostet) und anschließend **Weiter** aus. 

1. Geben Sie unter „Name“ den Namen **TutorialIntegrationRuntime** ein. Klicken Sie anschließend auf **Weiter**.

1. Wählen Sie in den Einstellungen **Click here to launch the express setup for this computer** (Klicken Sie hier, um das Express-Setup für diesen Computer zu starten.) aus. Dadurch wird die Integration Runtime auf Ihrem Computer installiert und bei Data Factory registriert. Alternativ können Sie die Installationsdatei über die manuelle Setupoption herunterladen, die Datei ausführen und die Integration Runtime mithilfe des Schlüssels registrieren. 

1. Klicken Sie im Fenster **Express-Setup von Integration Runtime (selbstgehostet)** auf **Schließen**. 

    ![Express-Setup von Integration Runtime (selbstgehostet)](./media/tutorial-hybrid-copy-portal/integration-runtime-setup-successful.png)

1. Stellen Sie im Dialogfeld **New Linked Service** (Neuer verknüpfter Dienst) sicher, dass **TutorialIntegrationRuntime** unter **Connect via integration runtime** (Verbindung per Integration Runtime herstellen) ausgewählt ist. Führen Sie dann die folgenden Schritte aus:

    a. Geben Sie unter **Name** die Zeichenfolge **SqlServerLinkedService** ein.

    b. Geben Sie unter **Servername** den Namen Ihrer SQL Server-Instanz ein. 

    c. Geben Sie unter **Datenbankname** den Namen der Datenbank mit der Tabelle **emp** an.

    d. Wählen Sie unter **Authentifizierungstyp** einen geeigneten Authentifizierungstyp aus, den Data Factory beim Herstellen der Verbindung mit Ihrer SQL Server-Datenbank verwenden soll.

    e. Geben Sie unter **Benutzername** und **Kennwort** den Namen und das Kennwort ein. Wenn Ihr Benutzerkonto- oder Servername einen umgekehrten Schrägstrich (\\) enthält, ist es erforderlich, ein Escapezeichen (\\) voranzustellen. Verwenden Sie beispielsweise *mydomain\\\\myuser*.

    f. Klicken Sie auf **Verbindung testen**. Mit diesem Schritt vergewissern Sie sich, dass Data Factory über die selbstgehostete Integration Runtime, die Sie erstellt haben, eine Verbindung mit Ihrer SQL Server-Datenbank herstellen kann.

    g. Klicken Sie auf **Fertig stellen**, um den verknüpften Dienst zu speichern.

1. Sie sollten sich wieder im Fenster mit dem geöffneten Quelldataset befinden. Führen Sie im Fenster **Eigenschaften** auf der Registerkarte **Verbindung** die folgenden Schritte aus: 

    a. Vergewissern Sie sich, dass unter **Verknüpfter Dienst** die Zeichenfolge **SqlServerLinkedService** angezeigt wird.

    b. Wählen Sie unter **Tabelle** die Option **[dbo].[emp]** aus.

1. Wechseln Sie zur Registerkarte mit der Pipeline **SQLServerToBlobPipeline**, oder klicken Sie in der Strukturansicht auf **SQLServerToBlobPipeline**. 

1. Wechseln Sie unten im Fenster **Eigenschaften** zur Registerkarte **Senke**, und klicken Sie auf **+ Neu**. 

1. Wählen Sie im Dialogfeld **Neues Dataset** die Option **Azure Blob Storage** aus. Klicken Sie anschließend auf **Weiter**. 

1. Wählen Sie im Dialogfeld **Format auswählen** den Formattyp Ihrer Daten aus. Klicken Sie anschließend auf **Weiter**. 

    ![Auswählen des Datenformats](./media/doc-common-process/select-data-format.png)

1. Geben Sie im Dialogfeld **Eigenschaften festlegen** als Name **AzureBlobDataset** ein. Klicken Sie neben dem Textfeld **Verknüpfter Dienst** auf **+ Neu**.

1. Geben Sie im Dialogfeld **New Linked Service (Azure Blob Storage)** (Neuer verknüpfter Dienst (Azure Blob Storage)) als Name **AzureStorageLinkedService** ein, und wählen Sie in der Liste **Speicherkontoname** Ihr Speicherkonto aus. Testen Sie die Verbindung, und wählen Sie dann **Fertig stellen** aus, um den verknüpften Dienst bereitzustellen.
1. Nach der Erstellung des verknüpften Diensts wird wieder die Seite **Eigenschaften festlegen** angezeigt. Wählen Sie **Weiter**.

1. Sie sollten sich wieder im Fenster mit dem geöffneten Senkendataset befinden. Führen Sie auf der Registerkarte **Verbindung** folgende Schritte aus: 

    a. Vergewissern Sie sich, dass unter **Verknüpfter Dienst** die Option **AzureStorageLinkedService** ausgewählt ist.
  
    b. Geben Sie unter **Dateipfad** für den Teil **Container/Verzeichnis** die Zeichenfolge **adftutorial/fromonprem** ein. Sollte der Ausgabeordner im Container „adftutorial“ nicht vorhanden sein, wird der Ausgabeordner von Data Factory automatisch erstellt.
    
    c. Wählen Sie für den Teil **Datei** die Option **Dynamischen Inhalt hinzufügen** aus.
    ![Dynamischer Ausdruck zum Auflösen des Dateinamens](./media/tutorial-hybrid-copy-portal/file-name.png)

    d. Fügen Sie `@CONCAT(pipeline().RunId, '.txt')` hinzu, und wählen Sie dann **Fertig stellen** aus. Dadurch wird die Datei in „PipelineRunID.txt“ umbenannt. 

1. Wechseln Sie zur Registerkarte mit der geöffneten Pipeline, oder klicken Sie in der Strukturansicht auf die Pipeline. Vergewissern Sie sich, dass unter **Sink Dataset** (Senkendataset) die Option **AzureBlobDataset** ausgewählt ist.

1. Klicken Sie zum Überprüfen der Pipelineeinstellungen auf der Symbolleiste für die Pipeline auf **Überprüfen**. Klicken Sie zum Schließen des **Pipelineüberprüfungsbericht** auf **Schließen**. 

1. Klicken Sie zum Veröffentlichen der erstellten Entitäten in Data Factory auf **Alle veröffentlichen**.

1. Warten Sie, bis die Popupmeldung **Veröffentlichung erfolgreich** angezeigt wird. Wenn Sie den Veröffentlichungsstatus überprüfen möchten, klicken Sie oben im Fenster auf den Link **Benachrichtigungen anzeigen**. Klicken Sie zum Schließen des Fensters mit den Benachrichtigungen auf **Schließen**. 


## <a name="trigger-a-pipeline-run"></a>Auslösen einer Pipelineausführung
Wählen Sie auf der Symbolleiste für die Pipeline die Option **Trigger hinzufügen** und dann **Trigger Now** (Jetzt auslösen) aus.

## <a name="monitor-the-pipeline-run"></a>Überwachen der Pipelineausführung

1. Wechseln Sie zur Registerkarte **Überwachen**. Dort sehen Sie die Pipeline, die Sie im vorherigen Schritt manuell ausgelöst haben. 

    ![Überwachen der Pipelineausführungen](./media/tutorial-hybrid-copy-portal/pipeline-runs.png)
1. Klicken Sie in der Spalte **Aktionen** auf den Link **View Activity Runs** (Aktivitätsausführungen anzeigen), um mit der Pipelineausführung verknüpfte Aktivitätsausführungen anzuzeigen. Da die Pipeline nur eine einzelne Aktivität enthält, werden nur Aktivitätsausführungen angezeigt. Wenn Sie Details zum Kopiervorgang anzeigen möchten, klicken Sie auf den Link **Details** (Brillensymbol) in der Spalte **Aktionen**. Wählen Sie oben **Pipelineausführungen** aus, um zurück zur Ansicht mit den Pipelineausführungen zu wechseln.

## <a name="verify-the-output"></a>Überprüfen der Ausgabe
Die Pipeline erstellt den Ausgabeordner *fromonprem* automatisch im Blobcontainer `adftutorial`. Vergewissern Sie sich, dass die Datei *[pipeline().RunId].txt* im Ausgabeordner enthalten ist. 


## <a name="next-steps"></a>Nächste Schritte
Die Pipeline in diesem Beispiel kopiert Daten in Blob Storage von einem Speicherort an einen anderen. Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Erstellen einer Data Factory.
> * Erstellen Sie eine selbstgehostete Integration Runtime.
> * Erstellen von verknüpften SQL Server- und Storage-Diensten 
> * Erstellen von SQL Server- und Blob Storage-Datasets
> * Erstellen einer Pipeline mit Kopieraktivität zum Verschieben der Daten
> * Starten einer Pipelineausführung
> * Überwachen der Pipelineausführung.

Eine Liste mit den von Data Factory unterstützten Datenspeichern finden Sie unter [Unterstützte Datenspeicher und Formate](copy-activity-overview.md#supported-data-stores-and-formats).

Fahren Sie mit dem folgenden Tutorial fort, um mehr über das Kopieren von Daten per Massenvorgang aus einer Quelle in ein Ziel zu erfahren:

> [!div class="nextstepaction"]
>[Massenkopieren von Daten](tutorial-bulk-copy-portal.md)