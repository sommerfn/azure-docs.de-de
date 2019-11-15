---
title: 'Ausführen eines SSIS-Pakets mit der Aktivität „SSIS-Paket ausführen“: Azure'
description: In diesem Artikel wird das Ausführen eines SSIS-Pakets (SQL Server Integration Services) in einer Azure Data Factory-Pipeline mithilfe der Aktivität „SSIS-Paket ausführen“ beschrieben.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 09/13/2019
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: b8ed0a04d2d13556f38873ef5f346d49ba4d1845
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73673748"
---
# <a name="run-an-ssis-package-with-the-execute-ssis-package-activity-in-azure-data-factory"></a>Ausführen eines SSIS-Pakets mit der Aktivität „SSIS-Paket ausführen“ in Azure Data Factory
In diesem Artikel wird das Ausführen eines SSIS-Pakets (SQL Server Integration Services) in einer Azure Data Factory-Pipeline mithilfe der Aktivität „SSIS-Paket ausführen“ beschrieben. 

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Erstellen Sie eine Azure-SSIS Integration Runtime-Instanz (IR), falls Sie noch keine besitzen. Befolgen Sie dazu die ausführliche Anleitung unter [Tutorial: Bereitstellen der Azure-SSIS Integration Runtime in Azure Data Factory](tutorial-create-azure-ssis-runtime-portal.md).

## <a name="run-a-package-in-the-azure-portal"></a>Ausführen eines Pakets im Azure-Portal
In diesem Abschnitt erstellen Sie mithilfe der Data Factory-Benutzeroberfläche oder -App eine Data Factory-Pipeline mit einer Aktivität „SSIS-Paket ausführen“, die Ihr SSIS-Paket ausführt.

### <a name="create-a-pipeline-with-an-execute-ssis-package-activity"></a>Erstellen einer Pipeline mit einer Aktivität „SSIS-Paket ausführen“
In diesem Schritt erstellen Sie über die Data Factory-Benutzeroberfläche oder -App eine Pipeline. Sie fügen eine Aktivität „SSIS-Paket ausführen“ zur Pipeline hinzu und konfigurieren sie für die Ausführung des SSIS-Pakets. 

1. Wählen Sie im Azure-Portal in der Übersicht oder auf der Startseite von Data Factory die Kachel **Erstellen und überwachen** aus, um die Data Factory-Benutzeroberfläche oder -App in einem separaten Tab zu öffnen. 

   ![Data Factory-Startseite](./media/how-to-invoke-ssis-package-stored-procedure-activity/data-factory-home-page.png)

   Wählen Sie auf der Seite **Erste Schritte** die Option **Pipeline erstellen** aus. 

   ![Seite für die ersten Schritte](./media/how-to-invoke-ssis-package-stored-procedure-activity/get-started-page.png)

1. Erweitern Sie in der Toolbox **Aktivitäten** die Option **Allgemein**. Ziehen Sie dann die Aktivität **SSIS-Paket ausführen** auf die Oberfläche des Pipeline-Designers. 

   ![Ziehen und Ablegen der Aktivität „SSIS-Paket ausführen“ auf der Oberfläche des Designers](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-designer.png) 

1. Geben Sie auf der Registerkarte **Allgemein** für die Aktivität „SSIS-Paket ausführen“ einen Namen und eine Beschreibung für die Aktivität an. Legen Sie optional Werte für **Timeout** und **Wiederholung** fest.

   ![Festlegen von Eigenschaften auf der Registerkarte „Allgemein“](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)

1. Wählen Sie auf der Registerkarte **Einstellungen** für die Aktivität „SSIS-Paket ausführen“ eine Azure-SSIS Integration Runtime-Instanz aus, in der Sie Ihr Paket ausführen möchten. Wenn Ihr Paket die Windows-Authentifizierung für den Zugriff auf Datenspeicher (beispielsweise lokale SQL-Server/Dateifreigaben oder Azure Files) verwendet, aktivieren Sie das Kontrollkästchen **Windows-Authentifizierung**. Geben Sie die Werte Ihrer Anmeldeinformationen für die Paketausführung in die Felder **Domäne**, **Benutzername** und **Kennwort** ein. 

    Alternativ können Sie in Ihrem Azure-Schlüsseltresor gespeicherte Geheimnisse als Werte verwenden. Aktivieren Sie hierzu das Kontrollkästchen **AZURE KEY VAULT** neben den entsprechenden Anmeldeinformationen. Sie können einen vorhandenen verknüpften Dienst für den Schlüsseltresor auswählen oder bearbeiten oder einen neuen Dienst erstellen. Wählen Sie anschließend den Geheimnisnamen oder die Geheimnisversion für den Wert Ihrer Anmeldeinformationen aus.

    Wenn Sie Ihren verknüpften Dienst für den Schlüsseltresor erstellen oder bearbeiten, können Sie den vorhandenen Schlüsseltresor auswählen und bearbeiten oder einen neuen erstellen. Stellen Sie sicher, dass die verwaltete Data Factory-Identität Zugriff auf Ihren Schlüsseltresor hat, sofern dies noch nicht der Fall ist. Sie können Ihre Geheimnisse auch direkt im folgenden Format eingeben: `<Key vault linked service name>/<secret name>/<secret version>`. Wenn Ihr Paket die 32-Bit-Runtime zum Ausführen benötigt, aktivieren Sie das Kontrollkästchen **32-Bit-Runtime**.

   Wählen Sie unter **Paketspeicherort** entweder **SSISDB**, **Dateisystem (Paket)** oder **Dateisystem (Projekt)** aus. Wenn Sie **SSISDB** als Paketspeicherort auswählen, geben Sie das auszuführende Paket an, das in SSISDB bereitgestellt wurde. Die Option „SSISDB“ wird automatisch ausgewählt, wenn Azure-SSIS Integration Runtime mit dem SSIS-Katalog (SSISDB) bereitgestellt wurde und dieser vom Azure SQL-Datenbank-Server bzw. von der verwalteten Instanz gehostet wird. 

    Wenn Ihre Azure-SSIS Integration Runtime-Instanz ausgeführt wird und das Kontrollkästchen **Manuelle Einträge** nicht aktiviert ist, können Sie Ihre vorhandenen Ordner, Projekte, Pakete oder Umgebungen über SSISDB durchsuchen und auswählen. Wählen Sie **Aktualisieren** aus, um Ihre neu hinzugefügten Ordner, Projekte, Pakete oder Umgebungen aus SSISDB abzurufen, sodass sie zum Durchsuchen und Auswählen verfügbar sind. Um die Umgebungen für Ihre Paketausführungen zu durchsuchen oder auszuwählen, müssen Sie Ihre Projekte zuvor so konfigurieren, dass sie diese Umgebungen als Referenzen aus denselben Ordnern unter SSISDB hinzufügen. Weitere Informationen finden Sie unter [Erstellen und Zuordnen einer Serverumgebung](https://docs.microsoft.com/sql/integration-services/create-and-map-a-server-environment?view=sql-server-2014).

   Wählen Sie für **Protokolliergrad** einen vordefinierten Bereich für die Protokollierung Ihrer Paketausführung. Aktivieren Sie das Kontrollkästchen **Angepasst**, wenn Sie stattdessen einen angepassten Namen für die Protokollierung eingeben möchten. 

   ![Festlegen von Eigenschaften auf der Registerkarte „Einstellungen“ – Automatisch](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-settings.png)

   Wenn Ihre Azure-SSIS Integration Runtime-Instanz nicht ausgeführt wird oder das Kontrollkästchen **Manuelle Einträge** aktiviert ist, geben Sie Ihre Paket- und Umgebungspfade aus SSISDB direkt in den folgenden Formaten ein: `<folder name>/<project name>/<package name>.dtsx` und `<folder name>/<environment name>`.

   ![Festlegen von Eigenschaften auf der Registerkarte „Einstellungen“ – Manuell](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-settings2.png)

   Wenn Sie **Dateisystem (Paket)** als Paketspeicherort auswählen, geben Sie zur Angabe des auszuführenden Pakets im Feld **Paketpfad** einen UNC-Pfad (Universal Naming Convention) zu Ihrer Paketdatei (`.dtsx`) an. Die Option „Dateisystem (Paket)“ wird automatisch ausgewählt, wenn Azure-SSIS Integration Runtime ohne SSISDB bereitgestellt wurde. Wenn Sie Ihr Paket also beispielsweise in Azure Files speichern, lautet der Paketpfad `\\<storage account name>.file.core.windows.net\<file share name>\<package name>.dtsx`. 
   
   Wenn Sie Ihr Paket in einer separaten Datei konfigurieren, müssen Sie außerdem im Feld **Konfigurationspfad** einen UNC-Pfad zu Ihrer Konfigurationsdatei (`.dtsConfig`) angeben. Wenn Sie Ihre Konfiguration also beispielsweise in Azure Files speichern, lautet der Konfigurationspfad `\\<storage account name>.file.core.windows.net\<file share name>\<configuration name>.dtsConfig`.

   ![Festlegen von Eigenschaften auf der Registerkarte „Einstellungen“ – Manuell](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-settings3.png)

   Wenn Sie **Dateisystem (Projekt)** als Paketspeicherort auswählen, geben Sie zur Angabe des auszuführenden Pakets im Feld **Projektpfad** einen UNC-Pfad (Universal Naming Convention) zu Ihrer Projektdatei (`.ispac`) und im Feld **Paketname** eine Paketdatei (`.dtsx`) aus Ihrem Projekt an. Wenn Sie Ihr Projekt also beispielsweise in Azure Files speichern, lautet der Projektpfad `\\<storage account name>.file.core.windows.net\<file share name>\<project name>.ispac`.

   ![Festlegen von Eigenschaften auf der Registerkarte „Einstellungen“ – Manuell](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-settings4.png)

   Als Nächstes geben Sie die Anmeldeinformationen für den Zugriff auf Ihre Projekt-, Paket- oder Konfigurationsdateien an. Wenn Sie wie weiter oben beschrieben die Werte der Anmeldeinformationen für die Paketausführung eingegeben haben, können Sie diese Werte wiederverwenden, indem Sie das Kontrollkästchen **Same as package execution credentials** (Identisch mit den Anmeldeinformationen für die Paketausführung) aktivieren. Geben Sie andernfalls die Werte Ihrer Anmeldeinformationen für den Paketzugriff in die Felder **Domäne**, **Benutzername** und **Kennwort** ein. Wenn Sie Ihr Projekt, Ihr Paket oder Ihre Konfiguration also beispielsweise in Azure Files speichern, lauten die Werte wie folgt: Domäne: `Azure`; Benutzername: `<storage account name>`; Kennwort: `<storage account key>`. 

   Alternativ können Sie in Ihrem Schlüsseltresor gespeicherte Geheimnisse als Werte verwenden (wie weiter oben beschrieben). Diese Anmeldeinformationen werden sowohl für den Zugriff auf Ihr Paket und Ihre Unterpakete in der Paketausführungsaufgabe (jeweils unter ihrem eigenen Pfad oder dem gleichen Projekt) als auch für den Zugriff auf Konfigurationen verwendet (einschließlich der Konfigurationen, die in Ihren Paketen angegeben sind). 
   
   Wenn Sie bei der Paketerstellung über SQL Server Data Tools die Schutzebene **EncryptAllWithPassword** oder **EncryptSensitiveWithPassword** verwendet haben, müssen Sie unter **Verschlüsselungskennwort** den Wert für Ihr Kennwort eingeben. Alternativ können Sie ein in Ihrem Schlüsseltresor gespeichertes Geheimnis als Wert verwenden (wie weiter oben beschrieben). Wenn Sie die Schutzebene **EncryptSensitiveWithUserKey** verwendet haben, geben Sie Ihre vertraulichen Werte in Konfigurationsdateien oder auf den Registerkarten **SSIS-Parameter**, **Verbindungs-Manager** oder **Eigenschaftenüberschreibungen** erneut ein (wie weiter unten beschrieben). 

   Die Verwendung der Schutzebene **EncryptAllWithUserKey** wird nicht unterstützt. Sie müssen Ihr Paket über SQL Server Data Tools oder das Befehlszeilenprogramm `dtutil` für die Verwendung einer anderen Schutzebene neu konfigurieren. 
   
   Wählen Sie für **Protokolliergrad** einen vordefinierten Bereich für die Protokollierung Ihrer Paketausführung. Aktivieren Sie das Kontrollkästchen **Angepasst**, wenn Sie stattdessen einen angepassten Namen für die Protokollierung eingeben möchten. Wenn Sie für Ihre Paketausführungen eine Protokollierung verwenden möchten, die über die Standardprotokollanbieter hinausgeht, die in Ihrem Paket angegeben werden können, geben Sie im Feld **Protokollierungspfad** den UNC-Pfad Ihres Protokollordners an. Wenn Sie Ihre Protokolle also beispielsweise in Azure Files speichern, lautet Ihr Protokollierungspfad `\\<storage account name>.file.core.windows.net\<file share name>\<log folder name>`. An diesem Pfad wird für jedes ausgeführte Einzelpaket ein Unterordner erstellt und mit der Aktivitätsausführungs-ID von „SSIS-Paket ausführen“ benannt. In diesem Unterordner werden alle fünf Minuten Protokolldateien generiert. 
   
   Abschließend geben Sie die Anmeldeinformationen für den Zugriff auf Ihren Protokollordner an. Wenn Sie wie weiter oben beschrieben die Werte der Anmeldeinformationen für den Paketzugriff eingegeben haben, können Sie diese Werte wiederverwenden, indem Sie das Kontrollkästchen **Same as package access credentials** (Identisch mit den Anmeldeinformationen für den Paketzugriff) aktivieren. Geben Sie andernfalls die Werte Ihrer Anmeldeinformationen für den Protokollierungszugriff in die Felder **Domäne**, **Benutzername** und **Kennwort** ein. Wenn Sie Ihre Protokolle also beispielsweise in Azure Files speichern, lauten die Werte wie folgt: Domäne: `Azure`; Benutzername: `<storage account name>`; Kennwort: `<storage account key>`. 

    Alternativ können Sie in Ihrem Schlüsseltresor gespeicherte Geheimnisse als Werte verwenden (wie weiter oben beschrieben). Diese Anmeldeinformationen werden zum Speichern Ihrer Protokolle verwendet. 
   
   Für alle zuvor erwähnten UNC-Pfade muss der vollqualifizierte Dateiname weniger als 260 Zeichen umfassen. Der Verzeichnisname muss weniger als 248 Zeichen umfassen.

1. Wenn Ihre Azure-SSIS Integration Runtime-Instanz ausgeführt wird, die Option **SSISDB** als Paketspeicherort ausgewählt ist und das Kontrollkästchen **Manuelle Einträge** auf der Registerkarte **Einstellungen** deaktiviert ist, werden auf der Registerkarte **SSIS-Parameter** der Aktivität „SSIS-Paket ausführen“ die vorhandenen SSIS-Parameter im ausgewählten Projekt oder Paket aus SSISDB angezeigt, damit Sie ihnen Werte zuweisen können. Andernfalls können Sie sie einzeln eingeben, um ihnen manuell Werte zuzuweisen. Vergewissern Sie sich, dass sie vorhanden sind und korrekt eingegeben wurden, damit Ihre Paketausführung erfolgreich ist. 
   
   Wenn Sie bei der Paketerstellung über SQL Server Data Tools die Schutzebene **EncryptSensitiveWithUserKey** verwendet haben und **Dateisystem (Paket)** oder **Dateisystem (Projekt)** als Paketspeicherort ausgewählt ist, müssen Sie Ihre vertraulichen Parameter erneut eingeben, um ihnen in Konfigurationsdateien oder auf dieser Registerkarte Werte zuzuweisen. 
   
   Beim Zuweisen von Werten für Ihre Parameter können Sie Ausdrücke, Funktionen, Data Factory-Systemvariablen und Data Factory-Pipelineparameter oder -Variablen verwenden, um dynamische Inhalte hinzuzufügen. Alternativ können Sie in Ihrem Schlüsseltresor gespeicherte Geheimnisse als Werte verwenden (wie weiter oben beschrieben).

   ![Festlegen von Eigenschaften auf der Registerkarte „SSIS-Parameter“](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-ssis-parameters.png)

1. Wenn Ihre Azure-SSIS Integration Runtime-Instanz ausgeführt wird, die Option **SSISDB** als Paketspeicherort ausgewählt ist und das Kontrollkästchen **Manuelle Einträge** auf der Registerkarte **Einstellungen** deaktiviert ist, werden auf der Registerkarte **Verbindungs-Manager** der Aktivität „SSIS-Paket ausführen“ die vorhandenen Verbindungs-Manager im ausgewählten Projekt oder Paket aus SSISDB angezeigt, damit Sie ihren Eigenschaften Werte zuweisen können. Andernfalls können Sie sie einzeln eingeben, um ihren Eigenschaften manuell Werte zuzuweisen. Vergewissern Sie sich, dass sie vorhanden sind und korrekt eingegeben wurden, damit Ihre Paketausführung erfolgreich ist. 
   
   Wenn Sie bei der Paketerstellung über SQL Server Data Tools die Schutzebene **EncryptSensitiveWithUserKey** verwendet haben und **Dateisystem (Paket)** oder **Dateisystem (Projekt)** als Paketspeicherort ausgewählt ist, müssen Sie auch Ihre vertraulichen Verbindungs-Manager-Eigenschaften erneut eingeben, um ihnen in Konfigurationsdateien oder auf dieser Registerkarte Werte zuzuweisen. 
   
   Beim Zuweisen von Werten für Ihre Verbindungs-Manager-Eigenschaften können Sie Ausdrücke, Funktionen, Data Factory-Systemvariablen und Data Factory-Pipelineparameter oder -Variablen verwenden, um dynamische Inhalte hinzuzufügen. Alternativ können Sie in Ihrem Schlüsseltresor gespeicherte Geheimnisse als Werte verwenden (wie weiter oben beschrieben).

   ![Festlegen von Eigenschaften auf der Registerkarte „Verbindungs-Manager“](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-connection-managers.png)

1. Geben Sie auf der Registerkarte **Eigenschaftenüberschreibungen** der Aktivität „SSIS-Paket ausführen“ die Pfade der vorhandenen Eigenschaften in dem von Ihnen ausgewählten Paket einzeln an, um ihnen manuell Werte zuzuweisen. Vergewissern Sie sich, dass sie vorhanden sind und korrekt eingegeben wurden, damit Ihre Paketausführung erfolgreich ist. Geben Sie also beispielsweise zum Überschreiben des Werts Ihrer Benutzervariablen den zugehörigen Pfad im folgenden Format ein: `\Package.Variables[User::<variable name>].Value`. 
   
   Wenn Sie bei der Paketerstellung über SQL Server Data Tools die Schutzebene **EncryptSensitiveWithUserKey** verwendet haben und **Dateisystem (Paket)** oder **Dateisystem (Projekt)** als Paketspeicherort ausgewählt ist, müssen Sie auch Ihre vertraulichen Parameter Eigenschaften eingeben, um ihnen in Konfigurationsdateien oder auf dieser Registerkarte Werte zuzuweisen. 
   
   Beim Zuweisen von Werten für Ihre Eigenschaften können Sie Ausdrücke, Funktionen, Data Factory-Systemvariablen und Data Factory-Pipelineparameter oder -Variablen verwenden, um dynamische Inhalte hinzuzufügen.

   ![Festlegen von Eigenschaften auf der Registerkarte „Eigenschaftenüberschreibungen“](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-property-overrides.png)

   Die in den Konfigurationsdateien und auf der Registerkarte **SSIS-Parameter** zugewiesenen Werte können mithilfe der Registerkarten **Verbindungs-Manager** oder **Eigenschaftenüberschreibungen** überschrieben werden. Die auf der Registerkarte **Verbindungs-Manager** zugewiesenen Werte können ebenfalls auf der Registerkarte **Eigenschaftenüberschreibungen** überschrieben werden.

1. Wählen Sie zum Überprüfen der Pipelinekonfiguration auf der Symbolleiste die Option **Überprüfen** aus. Wählen Sie zum Schließen von **Pipeline Validation Report** (Pipelineüberprüfungsbericht) die Option **>>** aus.

1. Veröffentlichen Sie die Pipeline in Data Factory, indem Sie die Schaltfläche **Alle veröffentlichen** auswählen. 

### <a name="run-the-pipeline"></a>Führen Sie die Pipeline aus.
In diesem Schritt lösen Sie eine Pipelineausführung aus. 

1. Wählen Sie zum Auslösen einer Pipelineausführung auf der Symbolleiste die Option **Trigger** (Auslösen) und dann **Trigger now** (Jetzt auslösen) aus. 

   ![Manuelles Auslösen](./media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-trigger.png)

2. Wählen Sie im Fenster **Pipelineausführung** die Option **Fertig stellen** aus. 

### <a name="monitor-the-pipeline"></a>Überwachen der Pipeline

1. Wechseln Sie im linken Bereich zur Registerkarte **Überwachen**. Die Pipelineausführung, der zugehörige Status sowie weitere Informationen (z. B. **Startzeit der Ausführung**) werden angezeigt. Klicken Sie zum Aktualisieren der Ansicht auf **Aktualisieren**.

   ![Pipelineausführungen](./media/how-to-invoke-ssis-package-stored-procedure-activity/pipeline-runs.png)

2. Klicken Sie in der Spalte **Aktionen** auf den Link **View Activity Runs** (Aktivitätsausführungen anzeigen). Sie sehen nur eine Aktivitätsausführung, da die Pipeline nur eine Aktivität enthält. Dabei handelt es sich um die Aktivität „SSIS-Paket ausführen“.

   ![Aktivitätsausführungen](./media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-runs.png)

3. Führen Sie auf Ihrem SQL-Server die folgende Abfrage für die SSISDB-Datenbank ausführen, um zu überprüfen, ob das Paket ausgeführt wurde. 

   ```sql
   select * from catalog.executions
   ```

   ![Überprüfen von Paketausführungen](./media/how-to-invoke-ssis-package-stored-procedure-activity/verify-package-executions.png)

4. Sie können auch die SSISDB-Ausführungs-ID aus der Ausgabe der Pipelineaktivitätsausführung abrufen und anhand der ID umfangreichere Ausführungsprotokolle und Fehlermeldungen in SQL Server Management Studio überprüfen.

   ![Abrufen der Ausführungs-ID](media/how-to-invoke-ssis-package-ssis-activity/get-execution-id.png)

### <a name="schedule-the-pipeline-with-a-trigger"></a>Planen der Pipeline mit einem Trigger

Sie können auch einen geplanten Trigger für die Pipeline erstellen, damit die Pipeline basierend auf einem Zeitplan (etwa stündlich oder täglich) ausgeführt wird. Ein Beispiel finden Sie unter [Erstellen einer Data Factory über die Azure Data Factory-Benutzeroberfläche](quickstart-create-data-factory-portal.md#trigger-the-pipeline-on-a-schedule).

## <a name="run-a-package-with-powershell"></a>Ausführen eines Pakets mit PowerShell
In diesem Abschnitt erstellen Sie mithilfe von Azure PowerShell eine Data Factory-Pipeline mit der Aktivität „SSIS-Paket ausführen“, die Ihr SSIS-Paket ausführt. 

Installieren Sie die aktuellen Azure PowerShell-Module, indem Sie die Schritt-für-Schritt-Anweisungen unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/install-az-ps) befolgen.

### <a name="create-a-data-factory-with-azure-ssis-ir"></a>Erstellen einer Data Factory mit Azure-SSIS IR
Sie können entweder eine vorhandene Data Factory verwenden, in der Azure-SSIS IR bereits bereitgestellt wurde, oder eine neue Data Factory mit Azure-SSIS IR erstellen. Befolgen Sie dann die Schrittanleitungen unter [Tutorial: Bereitstellen von SSIS-Paketen in Azure mit PowerShell](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure-powershell) folgen.

### <a name="create-a-pipeline-with-an-execute-ssis-package-activity"></a>Erstellen einer Pipeline mit einer Aktivität „SSIS-Paket ausführen“ 
In diesem Schritt erstellen Sie eine Pipeline mit einer Aktivität „SSIS-Paket ausführen“. Die Aktivität führt Ihr SSIS-Paket aus. 

1. Erstellen Sie im Ordner *C:\ADF\RunSSISPackage* eine JSON-Datei mit dem Namen *RunSSISPackagePipeline.json* und einem Inhalt wie im folgenden Beispiel:

   > [!IMPORTANT]
   > Bevor Sie die Datei speichern, ersetzen Sie Objektnamen, Beschreibungen, Pfade, Eigenschafts- oder Parameterwerte, Kennwörter und andere Variablenwerte. 
    
   ```json
   {
       "name": "RunSSISPackagePipeline",
       "properties": {
           "activities": [{
               "name": "MySSISActivity",
               "description": "My SSIS package/activity description",
               "type": "ExecuteSSISPackage",
               "typeProperties": {
                   "connectVia": {
                       "referenceName": "MyAzureSSISIR",
                       "type": "IntegrationRuntimeReference"
                   },
                   "executionCredential": {
                       "domain": "MyExecutionDomain",
                       "username": "MyExecutionUsername",
                       "password": {
                           "type": "SecureString",
                           "value": "MyExecutionPassword"
                       }
                   },
                   "runtime": "x64",
                   "loggingLevel": "Basic",
                   "packageLocation": {
                       "packagePath": "MyFolder/MyProject/MyPackage.dtsx",
                       "type": "SSISDB"
                   },
                   "environmentPath": "MyFolder/MyEnvironment",
                   "projectParameters": {
                       "project_param_1": {
                           "value": "123"
                       },
                       "project_param_2": {
                           "value": {
                               "value": "@pipeline().parameters.MyProjectParameter",
                               "type": "Expression"
                           }
                       }
                   },
                   "packageParameters": {
                       "package_param_1": {
                           "value": "345"
                       },
                       "package_param_2": {
                           "value": {
                               "type": "AzureKeyVaultSecret",
                               "store": {
                                   "referenceName": "myAKV",
                                   "type": "LinkedServiceReference"
                               },
                               "secretName": "MyPackageParameter"
                           }
                       }
                   },
                   "projectConnectionManagers": {
                       "MyAdonetCM": {
                           "username": {
                               "value": "MyConnectionUsername"
                           },
                           "password": {
                               "value": {
                                   "type": "SecureString",
                                   "value": "MyConnectionPassword"
                               }
                           }
                       }
                   },
                   "packageConnectionManagers": {
                       "MyOledbCM": {
                           "username": {
                               "value": {
                                   "value": "@pipeline().parameters.MyConnectionUsername",
                                   "type": "Expression"
                               }
                           },
                           "password": {
                               "value": {
                                   "type": "AzureKeyVaultSecret",
                                   "store": {
                                       "referenceName": "myAKV",
                                       "type": "LinkedServiceReference"
                                   },
                                   "secretName": "MyConnectionPassword",
                                   "secretVersion": "MyConnectionPasswordVersion"
                               }
                           }
                       }
                   },
                   "propertyOverrides": {
                       "\\Package.MaxConcurrentExecutables": {
                           "value": 8,
                           "isSensitive": false
                       }
                   }
               },
               "policy": {
                   "timeout": "0.01:00:00",
                   "retry": 0,
                   "retryIntervalInSeconds": 30
               }
           }]
       }
   }
   ```

   Geben Sie zum Ausführen von Paketen, die in Dateisystemen, Dateifreigaben oder Azure Files gespeichert sind, die Werte für Ihre Paket- oder Protokollspeicherort-Eigenschaften wie folgt ein:

   ```json
   {
       {
           {
               {
                   "packageLocation": {
                       "packagePath": "//MyStorageAccount.file.core.windows.net/MyFileShare/MyPackage.dtsx",
                       "type": "File",
                       "typeProperties": {
                           "packagePassword": {
                               "type": "SecureString",
                               "value": "MyEncryptionPassword"
                           },
                           "accessCredential": {
                               "domain": "Azure",
                               "username": "MyStorageAccount",
                               "password": {
                                   "type": "SecureString",
                                   "value": "MyAccountKey"
                               }
                           }
                       }
                   },
                   "logLocation": {
                       "logPath": "//MyStorageAccount.file.core.windows.net/MyFileShare/MyLogFolder",
                       "type": "File",
                       "typeProperties": {
                           "accessCredential": {
                               "domain": "Azure",
                               "username": "MyStorageAccount",
                               "password": {
                                   "type": "AzureKeyVaultSecret",
                                   "store": {
                                       "referenceName": "myAKV",
                                       "type": "LinkedServiceReference"
                           },
                                   "secretName": "MyAccountKey"
                               }
                           }
                       }
                   }
               }
           }
       }
   }
   ```

   Geben Sie zum Ausführen von Paketen in Projekten, die in Dateisystemen, Dateifreigaben oder Azure Files gespeichert sind, die Werte für Ihre Paketspeicherort-Eigenschaft wie folgt ein:

   ```json
   {
       {
           {
               {
                   "packageLocation": {
                       "packagePath": "//MyStorageAccount.file.core.windows.net/MyFileShare/MyProject.ispac:MyPackage.dtsx",
                       "type": "File",
                       "typeProperties": {
                           "packagePassword": {
                               "type": "SecureString",
                               "value": "MyEncryptionPassword"
                           },
                           "accessCredential": {
                               "domain": "Azure",
                               "userName": "MyStorageAccount",
                               "password": {
                                   "type": "SecureString",
                                   "value": "MyAccountKey"
                               }
                           }
                       }
                   }
               }
           }
       }
   }
   ```

2. Wechseln Sie in Azure PowerShell zum Ordner *C:\ADF\RunSSISPackage*.

3. Führen Sie das Cmdlet **Set-AzDataFactoryV2Pipeline** aus, um die Pipeline **RunSSISPackagePipeline** zu erstellen.

   ```powershell
   $DFPipeLine = Set-AzDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName `
                                                  -ResourceGroupName $ResGrp.ResourceGroupName `
                                                  -Name "RunSSISPackagePipeline"
                                                  -DefinitionFile ".\RunSSISPackagePipeline.json"
   ```

   Hier ist die Beispielausgabe:

   ```
   PipelineName      : Adfv2QuickStartPipeline
   ResourceGroupName : <resourceGroupName>
   DataFactoryName   : <dataFactoryName>
   Activities        : {CopyFromBlobToBlob}
   Parameters        : {[inputPath, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification], [outputPath, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification]}
   ```

### <a name="run-the-pipeline"></a>Führen Sie die Pipeline aus.
Verwenden Sie das Cmdlet **Invoke-AzDataFactoryV2Pipeline**, um die Pipeline auszuführen. Das Cmdlet gibt die ID der Pipelineausführung für die zukünftige Überwachung zurück.

```powershell
$RunId = Invoke-AzDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName `
                                             -ResourceGroupName $ResGrp.ResourceGroupName `
                                             -PipelineName $DFPipeLine.Name
```

### <a name="monitor-the-pipeline"></a>Überwachen der Pipeline

Führen Sie das folgende PowerShell-Skript aus, um den Status der Pipelineausführung kontinuierlich zu überwachen, bis das Kopieren der Daten beendet ist. Kopieren Sie das folgende Skript, fügen Sie es in das PowerShell-Fenster ein, und drücken Sie die EINGABETASTE. 

```powershell
while ($True) {
    $Run = Get-AzDataFactoryV2PipelineRun -ResourceGroupName $ResGrp.ResourceGroupName `
                                               -DataFactoryName $DataFactory.DataFactoryName `
                                               -PipelineRunId $RunId

    if ($Run) {
        if ($run.Status -ne 'InProgress') {
            Write-Output ("Pipeline run finished. The status is: " +  $Run.Status)
            $Run
            break
        }
        Write-Output  "Pipeline is running...status: InProgress"
    }

    Start-Sleep -Seconds 10
}   
```

Sie können die Pipeline auch mithilfe des Azure-Portals überwachen. Schritt-für-Schritt-Anweisungen finden Sie unter [Überwachen der Pipeline](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).

### <a name="schedule-the-pipeline-with-a-trigger"></a>Planen der Pipeline mit einem Trigger
Im vorherigen Schritt wurde die Pipeline auf Anforderung ausgeführt. Alternativ können Sie einen Zeitplantrigger erstellen, um die Pipeline nach Zeitplan (etwa stündlich oder täglich) auszuführen.

1. Erstellen Sie im Ordner *C:\ADF\RunSSISPackage* eine JSON-Datei mit dem Namen *MyTrigger.json* und dem folgenden Inhalt: 
        
   ```json
   {
       "properties": {
           "name": "MyTrigger",
           "type": "ScheduleTrigger",
           "typeProperties": {
               "recurrence": {
                   "frequency": "Hour",
                   "interval": 1,
                   "startTime": "2017-12-07T00:00:00-08:00",
                   "endTime": "2017-12-08T00:00:00-08:00"
               }
           },
           "pipelines": [{
               "pipelineReference": {
                   "type": "PipelineReference",
                   "referenceName": "RunSSISPackagePipeline"
               },
               "parameters": {}
           }]
       }
   }    
   ```
    
1. Wechseln Sie in Azure PowerShell zum Ordner *C:\ADF\RunSSISPackage*.
1. Führen Sie das Cmdlet **Set-AzDataFactoryV2Trigger** aus, um den Trigger zu erstellen. 

   ```powershell
   Set-AzDataFactoryV2Trigger -ResourceGroupName $ResGrp.ResourceGroupName `
                                   -DataFactoryName $DataFactory.DataFactoryName `
                                   -Name "MyTrigger" -DefinitionFile ".\MyTrigger.json"
   ```
1. Standardmäßig befindet sich der Trigger im beendeten Zustand. Starten Sie den Trigger durch Ausführen des Cmdlets **Start-AzDataFactoryV2Trigger**. 

   ```powershell
   Start-AzDataFactoryV2Trigger -ResourceGroupName $ResGrp.ResourceGroupName `
                                     -DataFactoryName $DataFactory.DataFactoryName `
                                     -Name "MyTrigger" 
   ```
1. Bestätigen Sie, dass der Trigger sich im gestarteten Zustand befindet, indem Sie das Cmdlet **Get-AzDataFactoryV2Trigger** ausführen. 

   ```powershell
   Get-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName `
                                   -DataFactoryName $DataFactoryName `
                                   -Name "MyTrigger"     
   ```    
1. Führen Sie nach der nächsten vollen Stunde den folgenden Befehl aus. Wenn die aktuelle Uhrzeit z.B. 15:25 UTC ist, führen Sie den Befehl um 16:00 UTC aus. 
    
   ```powershell
   Get-AzDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName `
                                      -DataFactoryName $DataFactoryName `
                                      -TriggerName "MyTrigger" `
                                      -TriggerRunStartedAfter "2017-12-06" `
                                      -TriggerRunStartedBefore "2017-12-09"
   ```

   Führen Sie auf Ihrem SQL-Server die folgende Abfrage für die SSISDB-Datenbank ausführen, um zu überprüfen, ob das Paket ausgeführt wurde. 

   ```sql
   select * from catalog.executions
   ```

## <a name="next-steps"></a>Nächste Schritte
Informationen finden Sie im folgenden Blogbeitrag:
- [Modernisieren und Erweitern von ETL/ELT-Workflows mit SSIS-Aktivitäten in Azure Data Factory-Pipelines](https://techcommunity.microsoft.com/t5/SQL-Server-Integration-Services/Modernize-and-Extend-Your-ETL-ELT-Workflows-with-SSIS-Activities/ba-p/388370)
