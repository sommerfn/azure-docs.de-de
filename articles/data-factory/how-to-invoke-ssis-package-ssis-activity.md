---
title: Ausführen eines SSIS-Pakets mit der Aktivität „SSIS-Paket ausführen“ – Azure | Microsoft-Dokumentation
description: In diesem Artikel wird das Ausführen eines SSIS-Pakets (SQL Server Integration Services) in einer Azure Data Factory-Pipeline mithilfe der Aktivität „SSIS-Paket ausführen“ beschrieben.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 07/01/2019
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: f33259fff17633cc4864a342609f747ebb9902ba
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/01/2019
ms.locfileid: "67484915"
---
# <a name="run-an-ssis-package-with-the-execute-ssis-package-activity-in-azure-data-factory"></a>Ausführen eines SSIS-Pakets mit der Aktivität „SSIS-Paket ausführen“ in Azure Data Factory
In diesem Artikel wird das Ausführen eines SSIS-Pakets (SQL Server Integration Services) in einer ADF-Pipeline (Azure Data Factory) mithilfe der Aktivität „SSIS-Paket ausführen“ beschrieben. 

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Erstellen Sie eine Azure-SSIS Integration Runtime (IR), falls Sie noch keine besitzen. Befolgen Sie dazu die ausführliche Anleitung unter [Tutorial: Bereitstellen der Azure-SSIS Integration Runtime in Azure Data Factory](tutorial-create-azure-ssis-runtime-portal.md).

## <a name="run-a-package-in-the-azure-portal"></a>Ausführen eines Pakets im Azure-Portal
In diesem Abschnitt erstellen Sie mithilfe der ADF-Benutzeroberfläche (UI)/-App eine ADF-Pipeline mit einer Aktivität „SSIS-Paket ausführen“, die ein SSIS-Paket ausführt.

### <a name="create-a-pipeline-with-an-execute-ssis-package-activity"></a>Erstellen einer Pipeline mit einer Aktivität „SSIS-Paket ausführen“
In diesem Schritt erstellen Sie über die ADF-Benutzeroberfläche/-App eine Pipeline. Sie fügen eine Aktivität „SSIS-Paket ausführen“ zur Pipeline hinzu und konfigurieren sie für die Ausführung des SSIS-Pakets. 

1. Klicken Sie in der ADF-Übersicht/auf der Startseite im Azure-Portal auf die Kachel **Erstellen und überwachen**, um die ADF-Benutzeroberfläche/-App auf einer separaten Registerkarte zu starten. 

   ![Data Factory-Startseite](./media/how-to-invoke-ssis-package-stored-procedure-activity/data-factory-home-page.png)

   Klicken Sie auf der Seite **Erste Schritte** auf die Option **Pipeline erstellen**: 

   ![Seite für die ersten Schritte](./media/how-to-invoke-ssis-package-stored-procedure-activity/get-started-page.png)

2. Erweitern Sie in der Toolbox **Aktivitäten** die Option **Allgemein**, und ziehen Sie die Aktivität **SSIS-Paket ausführen** auf die Oberfläche des Pipeline-Designers. 

   ![Ziehen und Ablegen der Aktivität „SSIS-Paket ausführen“ auf der Oberfläche des Designers](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-designer.png) 

3. Geben Sie auf der Registerkarte **Allgemein** für die Aktivität „SSIS-Paket ausführen“ einen Namen und eine Beschreibung für die Aktivität an. Legen Sie ein optionales Timeout und Wiederholungswerte fest.

   ![Festlegen von Eigenschaften auf der Registerkarte „Allgemein“](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)

4. Wählen Sie auf der Registerkarte **Einstellungen** für die Aktivität „SSIS-Paket ausführen“ eine Azure-SSIS Integration Runtime-Instanz aus, in der Sie Ihr Paket ausführen möchten. Wenn Ihr Paket die Windows-Authentifizierung verwendet, um auf Datenspeicher (z. B. lokale SQL Server-Computer/Dateifreigaben, Azure Files oder Ähnliches) zuzugreifen, aktivieren Sie das Kontrollkästchen **Windows-Authentifizierung**, und geben Sie die Anmeldeinformationen für die Paketausführung ein (**Domäne**/**Benutzername**/**Kennwort**). Alternativ hierzu können Sie in Ihrer Azure Key Vault-Instanz (AKV) gespeicherte Geheimnisse als Werte verwenden. Klicken Sie hierzu neben der entsprechenden Anmeldeinformation auf das Kontrollkästchen **AZURE KEY VAULT**, wählen/bearbeiten Sie Ihren vorhandenen verknüpften AKV-Dienst (oder erstellen Sie einen neuen), und wählen Sie dann den Namen bzw. die Version des Geheimnisses für Ihren Anmeldeinformationswert aus.  Wenn Sie Ihren verknüpften AKV-Dienst erstellen/bearbeiten, können Sie Ihren vorhandenen AKV auswählen/bearbeiten (oder einen neuen erstellen). Es ist aber zu empfehlen, für AKV den Zugriff auf die per ADF verwaltete Identität zu gewähren, falls Sie dies nicht bereits getan haben. Sie können Ihre Geheimnisse auch direkt im folgenden Format eingeben: `<AKV linked service name>/<secret name>/<secret version>`. Wenn Ihr Paket die 32-Bit-Runtime zum Ausführen benötigt, aktivieren Sie das Kontrollkästchen **32-Bit-Runtime**. 

   Wählen Sie unter **Paketspeicherort** entweder **SSISDB**, **Dateisystem (Paket)** oder **Dateisystem (Projekt)** aus. Wenn Sie **SSISDB** als Paketspeicherort auswählen, müssen Sie das auszuführende Paket angeben, das in SSISDB bereitgestellt wurde. Die Option „SSISDB“ wird automatisch ausgewählt, wenn Azure-SSIS Integration Runtime mit SSIS-Katalog (SSISDB) bereitgestellt wurde und dieser vom Azure SQL-Datenbank-Server bzw. von der verwalteten Instanz gehostet wird. Wenn Ihre Azure-SSIS Integration Runtime-Instanz ausgeführt wird und das Kontrollkästchen **Manuelle Einträge** nicht aktiviert ist, können Sie Ihre vorhandenen Ordner/Projekte/Pakete/Umgebungen aus SSISDB durchsuchen und auswählen. Klicken Sie auf die Schaltfläche **Aktualisieren**, um Ihre neu hinzugefügten Ordner/Projekte/Pakete/Umgebungen aus SSISDB abzurufen, sodass sie zum Durchsuchen und Auswählen verfügbar sind. 
   
   Wählen Sie für **Protokolliergrad** einen vordefinierten Bereich für die Protokollierung Ihrer Paketausführung. Aktivieren Sie das Kontrollkästchen **Angepasst**, wenn Sie stattdessen einen angepassten Namen für die Protokollierung eingeben möchten. 

   ![Festlegen von Eigenschaften auf der Registerkarte „Einstellungen“ – Automatisch](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-settings.png)

   Wenn Ihre Azure-SSIS Integration Runtime-Instanz nicht ausgeführt wird oder das Kontrollkästchen **Manuelle Einträge** aktiviert ist, können Sie Ihre Paket- und Umgebungspfade aus SSISDB direkt in den folgenden Formaten eingeben: `<folder name>/<project name>/<package name>.dtsx` und `<folder name>/<environment name>`.

   ![Festlegen von Eigenschaften auf der Registerkarte „Einstellungen“ – Manuell](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-settings2.png)

   Wenn Sie **Dateisystem (Paket)** als Paketspeicherort auswählen, müssen Sie zur Angabe des auszuführenden Pakets unter **Paketpfad** einen UNC-Pfad (Universal Naming Convention) zu Ihrer Paketdatei (`.dtsx`) angeben. Die Option „Dateisystem (Paket)“ wird automatisch ausgewählt, wenn Azure-SSIS Integration Runtime ohne SSISDB bereitgestellt wurde. Wenn Sie Ihr Paket also beispielsweise in Azure Files speichern, lautet der Paketpfad `\\<storage account name>.file.core.windows.net\<file share name>\<package name>.dtsx`. 
   
   Wenn Sie Ihr Paket in einer separaten Datei konfigurieren, müssen Sie außerdem unter **Konfigurationspfad** einen UNC-Pfad zu Ihrer Konfigurationsdatei (`.dtsConfig`) angeben. Wenn Sie Ihre Konfiguration also beispielsweise in Azure Files speichern, lautet der Konfigurationspfad `\\<storage account name>.file.core.windows.net\<file share name>\<configuration name>.dtsConfig`.

   ![Festlegen von Eigenschaften auf der Registerkarte „Einstellungen“ – Manuell](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-settings3.png)

   Wenn Sie **Dateisystem (Projekt)** als Paketspeicherort auswählen, müssen Sie zur Angabe des auszuführenden Pakets unter **Projektpfad** einen UNC-Pfad (Universal Naming Convention) zu Ihrer Projektdatei (`.ispac`) und unter **Paketname** eine Paketdatei (`.dtsx`) aus Ihrem Projekt angeben. Wenn Sie Ihr Projekt also beispielsweise in Azure Files speichern, lautet der Projektpfad `\\<storage account name>.file.core.windows.net\<file share name>\<project name>.ispac`.

   ![Festlegen von Eigenschaften auf der Registerkarte „Einstellungen“ – Manuell](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-settings4.png)

   Als Nächstes müssen Sie die Anmeldeinformationen für den Zugriff auf Ihre Projekt-/Paket-/Konfigurationsdateien angeben. Wenn Sie wie weiter oben beschrieben die Werte der Anmeldeinformationen für die Paketausführung eingegeben haben, können Sie diese Werte wiederverwenden, indem Sie das Kontrollkästchen **Same as package execution credentials** (Identisch mit den Anmeldeinformationen für die Paketausführung) aktivieren. Andernfalls müssen Sie Ihre Anmeldeinformationen für den Paketzugriff eingeben (**Domäne**/**Benutzername**/**Kennwort**). Wenn Sie Ihr Projekt/Ihr Paket/Ihre Konfiguration also beispielsweise in Azure Files speichern, lauten die Werte wie folgt: **Domäne:** `Azure`; **Benutzername:** `<storage account name>`; **Kennwort:** `<storage account key>`. Alternativ können Sie in Ihrer AKV-Instanz gespeicherte Geheimnisse als Werte verwenden (wie weiter oben beschrieben). Diese Anmeldeinformationen werden sowohl für den Zugriff auf Ihr Paket und Ihre Unterpakete in der Paketausführungsaufgabe (jeweils unter ihrem eigenen Pfad/dem gleichen Projekt) als auch für den Zugriff auf Konfigurationen verwendet (einschließlich der Konfigurationen, die in Ihren Paketen angegeben sind). 
   
   Wenn Sie bei der Paketerstellung über SQL Server Data Tools (SSDT) die Schutzebene **EncryptAllWithPassword**/**EncryptSensitiveWithPassword** verwendet haben, müssen Sie unter **Verschlüsselungskennwort** den Wert für Ihr Kennwort eingeben. Alternativ können Sie ein in Ihrer AKV-Instanz gespeichertes Geheimnis als Wert verwenden (wie weiter oben beschrieben). Wenn Sie die Schutzebene **EncryptSensitiveWithUserKey** verwendet haben, müssen Sie Ihre vertraulichen Werte in Konfigurationsdateien oder auf den Registerkarten **SSIS-Parameter**/**Verbindungs-Manager**/**Eigenschaftenüberschreibungen** erneut eingeben (wie weiter unten beschrieben). Die Schutzebene **EncryptAllWithUserKey** wird nicht unterstützt. Sollten Sie diese Schutzebene verwendet haben, müssen Sie Ihr Paket über SSDT oder mithilfe des Befehlszeilentools `dtutil` mit einer anderen Schutzebene konfigurieren. 
   
   Wählen Sie für **Protokolliergrad** einen vordefinierten Bereich für die Protokollierung Ihrer Paketausführung. Aktivieren Sie das Kontrollkästchen **Angepasst**, wenn Sie stattdessen einen angepassten Namen für die Protokollierung eingeben möchten. Wenn Sie für Ihre Paketausführungen eine Protokollierung verwenden möchten, die über die Standardprotokollanbieter hinausgeht, die in Ihrem Paket angegeben werden können, müssen Sie unter **Protokollierungspfad** den UNC-Pfad Ihres Protokollordners angeben. Wenn Sie Ihre Protokolle also beispielsweise in Azure Files speichern, lautet Ihr Protokollierungspfad `\\<storage account name>.file.core.windows.net\<file share name>\<log folder name>`. An diesem Pfad wird für jedes ausgeführte Einzelpaket ein Unterordner erstellt und mit der Aktivitätsausführungs-ID von „SSIS-Paket ausführen“ benannt. In diesem Unterordner werden alle fünf Minuten Protokolldateien generiert. 
   
   Abschließend müssen Sie auch die Anmeldeinformationen für den Zugriff auf Ihren Protokollordner angeben. Wenn Sie wie weiter oben beschrieben die Werte der Anmeldeinformationen für den Paketzugriff eingegeben haben, können Sie diese Werte wiederverwenden, indem Sie das Kontrollkästchen **Same as package access credentials** (Identisch mit den Anmeldeinformationen für den Paketzugriff) aktivieren. Andernfalls müssen Sie Ihre Anmeldeinformationen für den Protokollierungszugriff eingeben (**Domäne**/**Benutzername**/**Kennwort**). Wenn Sie Ihre Protokolle also beispielsweise in Azure Files speichern, lauten die Werte wie folgt: **Domäne:** `Azure`; **Benutzername:** `<storage account name>`; **Kennwort:** `<storage account key>`. Alternativ können Sie in Ihrer AKV-Instanz gespeicherte Geheimnisse als Werte verwenden (wie weiter oben beschrieben). Diese Anmeldeinformationen werden verwendet, um Ihre Protokolle zu speichern. 
   
   Bei allen oben angegebenen UNC-Pfaden muss der vollqualifizierte Dateiname kürzer als 260 Zeichen und der Verzeichnisname kürzer als 248 Zeichen sein.

5. Wenn Ihre Azure-SSIS Integration Runtime-Instanz ausgeführt wird, die Option **SSISDB** als Paketspeicherort ausgewählt ist und das Kontrollkästchen **Manuelle Einträge** auf der Registerkarte **Einstellungen** deaktiviert ist, werden auf der Registerkarte **SSIS-Parameter** der Aktivität „SSIS-Paket ausführen“ die vorhandenen SSIS-Parameter im ausgewählten Projekt/Paket aus SSISDB angezeigt, damit Sie ihnen Werte zuweisen können. Andernfalls können Sie sie einzeln eingeben, um ihnen manuell Werte zuzuweisen. Bitte stellen Sie sicher, dass sie vorhanden und korrekt eingegeben sind, damit Ihre Paketausführung erfolgreich ist. 
   
   Wenn Sie bei der Paketerstellung über SSDT die Schutzebene **EncryptSensitiveWithUserKey** verwendet haben und **Dateisystem (Paket)** /**Dateisystem (Projekt)** als Paketspeicherort ausgewählt ist, müssen Sie Ihre vertraulichen Parameter erneut eingeben, um ihnen in Konfigurationsdateien oder auf dieser Registerkarte Werte zuzuweisen. 
   
   Beim Zuweisen von Werten für Ihre Parameter können Sie Ausdrücke, Funktionen, ADF-Systemvariablen und ADF-Pipelineparameter/-Variablen verwenden, um dynamische Inhalte hinzuzufügen. Alternativ können Sie in Ihrer AKV-Instanz gespeicherte Geheimnisse als Werte verwenden (wie weiter oben beschrieben).

   ![Festlegen von Eigenschaften auf der Registerkarte „SSIS-Parameter“](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-ssis-parameters.png)

6. Wenn Ihre Azure-SSIS Integration Runtime-Instanz ausgeführt wird, die Option **SSISDB** als Paketspeicherort ausgewählt ist und das Kontrollkästchen **Manuelle Einträge** auf der Registerkarte **Einstellungen** deaktiviert ist, werden auf der Registerkarte **Verbindungs-Manager** der Aktivität „SSIS-Paket ausführen“ die vorhandenen Verbindungs-Manager im ausgewählten Projekt/Paket aus SSISDB angezeigt, damit Sie ihren Eigenschaften Werte zuweisen können. Andernfalls können Sie sie einzeln eingeben, um den Eigenschaften manuell Werte zuzuweisen. Stellen Sie sicher, dass sie vorhanden und korrekt eingegeben sind, damit Ihre Paketausführung erfolgreich ist. 
   
   Wenn Sie bei der Paketerstellung über SSDT die Schutzebene **EncryptSensitiveWithUserKey** verwendet haben und **Dateisystem (Paket)** /**Dateisystem (Projekt)** als Paketspeicherort ausgewählt ist, müssen Sie Ihre vertraulichen Verbindungs-Manager-Eigenschaften erneut eingeben, um ihnen in Konfigurationsdateien oder auf dieser Registerkarte Werte zuzuweisen. 
   
   Beim Zuweisen von Werten für Ihre Verbindungs-Manager-Eigenschaften können Sie Ausdrücke, Funktionen, ADF-Systemvariablen und ADF-Pipelineparameter/-Variablen verwenden, um dynamische Inhalte hinzuzufügen. Alternativ können Sie in Ihrer AKV-Instanz gespeicherte Geheimnisse als Werte verwenden (wie weiter oben beschrieben).

   ![Festlegen von Eigenschaften auf der Registerkarte „Verbindungs-Manager“](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-connection-managers.png)

7. Auf der Registerkarte **Eigenschaftsüberschreibungen** der Aktivität „SSIS-Paket ausführen“ können Sie die Pfade der vorhandenen Eigenschaften in dem von Ihnen ausgewählten Paket einzeln eingeben, um ihnen manuell Werte zuzuweisen. Vergewissern Sie sich, dass sie vorhanden sind und korrekt eingegeben wurden, damit Ihre Paketausführung erfolgreich ist. Geben Sie also beispielsweise zum Überschreiben des Werts Ihrer Benutzervariablen den zugehörigen Pfad im folgenden Format ein: `\Package.Variables[User::<variable name>].Value`. 
   
   Wenn Sie bei der Paketerstellung über SSDT die Schutzebene **EncryptSensitiveWithUserKey** verwendet haben und **Dateisystem (Paket)** /**Dateisystem (Projekt)** als Paketspeicherort ausgewählt ist, müssen Sie Ihre vertraulichen Eigenschaften erneut eingeben, um ihnen in Konfigurationsdateien oder auf dieser Registerkarte Werte zuzuweisen. 
   
   Beim Zuweisen von Werten für Ihre Eigenschaften können Sie Ausdrücke, Funktionen, ADF-Systemvariablen und ADF-Pipelineparameter/-Variablen verwenden, um dynamische Inhalte hinzuzufügen.

   ![Festlegen von Eigenschaften auf der Registerkarte „Eigenschaftenüberschreibungen“](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-property-overrides.png)

   Die in Konfigurationsdateien und auf der Registerkarte *SSIS-Parameter* zugewiesenen Werte können auf der Registerkarte **Verbindungs-Manager**/**Eigenschaftenüberschreibungen** überschrieben werden. Auf der Registerkarte **Verbindungs-Manager** zugewiesene Werte können außerdem auf der Registerkarte **Eigenschaftenüberschreibungen** überschrieben werden.

8. Klicken Sie zum Überprüfen der Pipelinekonfiguration in der Symbolleiste auf **Überprüfen**. Klicken Sie zum Schließen des **Pipeline Validation Report** (Pipelineüberprüfungsbericht) auf **>>** .

9. Veröffentlichen Sie die Pipeline in der ADF, indem Sie auf die Schaltfläche **Alle veröffentlichen** klicken. 

### <a name="run-the-pipeline"></a>Führen Sie die Pipeline aus.
In diesem Schritt lösen Sie eine Pipelineausführung aus. 

1. Klicken Sie auf der Symbolleiste auf **Trigger** und anschließend auf **Jetzt auslösen**, um eine Pipelineausführung auszulösen. 

   ![Manuelles Auslösen](./media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-trigger.png)

2. Wählen Sie im Fenster **Pipelineausführung** die Option **Fertig stellen** aus. 

### <a name="monitor-the-pipeline"></a>Überwachen der Pipeline

1. Wechseln Sie im linken Bereich zur Registerkarte **Überwachen**. Es werden die Pipelineausführung, der zugehörige Status sowie weitere Informationen (z.B. Startzeit der Ausführung) angezeigt. Klicken Sie zum Aktualisieren der Ansicht auf **Aktualisieren**.

   ![Pipelineausführungen](./media/how-to-invoke-ssis-package-stored-procedure-activity/pipeline-runs.png)

2. Klicken Sie in der Spalte **Aktionen** auf den Link **Aktivitätsausführungen anzeigen**. Sie sehen, dass nur eine Aktivität ausgeführt wird, da die Pipeline nur eine Aktivität (die Aktivität „SSIS-Paket ausführen“) enthält.

   ![Aktivitätsausführungen](./media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-runs.png)

3. Sie können auf Ihrem Azure SQL-Server die folgende **Abfrage** für die SSISDB-Datenbank ausführen, um zu überprüfen, ob das Paket ausgeführt wurde. 

   ```sql
   select * from catalog.executions
   ```

   ![Überprüfen von Paketausführungen](./media/how-to-invoke-ssis-package-stored-procedure-activity/verify-package-executions.png)

4. Sie können auch die SSISDB-Ausführungs-ID aus der Ausgabe der Pipeline-Aktivitätsausführung abrufen und anhand der ID umfangreichere Ausführungsprotokolle und Fehlermeldungen in SQL Server Management Studio (SSMS) überprüfen.

   ![Abrufen der Ausführungs-ID](media/how-to-invoke-ssis-package-ssis-activity/get-execution-id.png)

### <a name="schedule-the-pipeline-with-a-trigger"></a>Planen der Pipeline mit einem Trigger

Sie können auch einen geplanten Trigger für die Pipeline erstellen, damit die Pipeline basierend auf einem Zeitplan (stündlich, täglich usw.) ausgeführt wird. Ein Beispiel finden Sie unter [Erstellen einer Data Factory über die Azure Data Factory-Benutzeroberfläche](quickstart-create-data-factory-portal.md#trigger-the-pipeline-on-a-schedule).

## <a name="run-a-package-with-powershell"></a>Ausführen eines Pakets mit PowerShell
In diesem Abschnitt erstellen Sie mithilfe von Azure PowerShell eine ADF-Pipeline mit einer Aktivität „SSIS-Paket ausführen“, die ein SSIS-Paket ausführt. 

Installieren Sie die aktuellen Azure PowerShell-Module, indem Sie die Schritt-für-Schritt-Anweisungen unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/install-az-ps) befolgen.

### <a name="create-an-adf-with-azure-ssis-ir"></a>Erstellen einer ADF mit Azure-SSIS IR
Sie können entweder einen vorhandenen ADF verwenden, für die bereits Azure-SSIS IR bereitgestellt wurde, oder eine neue ADF mit Azure-SSIS IR erstellen, indem Sie der Schritt-für-Schritt-Anleitung im [Tutorial: Bereitstellen von SSIS-Paketen in Azure mit PowerShell](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure-powershell) folgen.

### <a name="create-a-pipeline-with-an-execute-ssis-package-activity"></a>Erstellen einer Pipeline mit einer Aktivität „SSIS-Paket ausführen“ 
In diesem Schritt erstellen Sie eine Pipeline mit einer Aktivität „SSIS-Paket ausführen“. Die Aktivität führt Ihr SSIS-Paket aus. 

1. Erstellen Sie im Ordner **C:\ADF\RunSSISPackage** eine JSON-Datei mit dem Namen **RunSSISPackagePipeline.json** und einem Inhalt wie im folgenden Beispiel:

   > [!IMPORTANT]
   > Bevor Sie die Datei speichern, ersetzen Sie Objektnamen, Beschreibungen, Pfade, Eigenschafts- und Parameterwerte, Kennwörter und andere Variablenwerte. 

   ```json
   {
       "name": "RunSSISPackagePipeline",
       "properties": {
           "activities": [{
               "name": "mySSISActivity",
               "description": "My SSIS package/activity description",
               "type": "ExecuteSSISPackage",
               "typeProperties": {
                   "connectVia": {
                       "referenceName": "myAzureSSISIR",
                       "type": "IntegrationRuntimeReference"
                   },
                   "executionCredential": {
                       "domain": "MyDomain",
                       "userName": "MyUsername",
                       "password": {
                           "type": "SecureString",
                           "value": "**********"
                       }
                   },
                   "runtime": "x64",
                   "loggingLevel": "Basic",
                   "packageLocation": {
                       "packagePath": "FolderName/ProjectName/PackageName.dtsx"
                   },
                   "environmentPath": "FolderName/EnvironmentName",
                   "projectParameters": {
                       "project_param_1": {
                           "value": "123"
                       },
                       "project_param_2": {
                           "value": {
                               "value": "@pipeline().parameters.MyPipelineParameter",
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
                               "secretName": "MySecret"
                           }
                       }
                   },
                   "projectConnectionManagers": {
                       "MyAdonetCM": {
                           "userName": {
                               "value": "sa"
                           },
                           "passWord": {
                               "value": {
                                   "type": "SecureString",
                                   "value": "abc"
                               }
                           }
                       }
                   },
                   "packageConnectionManagers": {
                       "MyOledbCM": {
                           "userName": {
                               "value": {
                                   "value": "@pipeline().parameters.MyUsername",
                                   "type": "Expression"
                               }
                           },
                           "passWord": {
                               "value": {
                                   "type": "AzureKeyVaultSecret",
                                   "store": {
                                       "referenceName": "myAKV",
                                       "type": "LinkedServiceReference"
                                   },
                                   "secretName": "MyPassword",
                                   "secretVersion": "3a1b74e361bf4ef4a00e47053b872149"
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

2. Navigieren Sie in Azure PowerShell zum Ordner `C:\ADF\RunSSISPackage`.

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
Im vorherigen Schritt wurde die Pipeline auf Anforderung ausgeführt. Alternativ können Sie einen Zeitplantrigger erstellen, um die Pipeline nach Zeitplan (stündlich, täglich usw.) auszuführen.

1. Erstellen Sie im Ordner **C:\ADF\RunSSISPackage** eine JSON-Datei mit dem Namen **MyTrigger.json** und dem folgenden Inhalt: 

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
2. Wechseln Sie in **Azure PowerShell** zum Ordner **C:\ADF\RunSSISPackage**.
3. Führen Sie das Cmdlet **Set-AzDataFactoryV2Trigger** aus, um den Trigger zu erstellen. 

   ```powershell
   Set-AzDataFactoryV2Trigger -ResourceGroupName $ResGrp.ResourceGroupName `
                                   -DataFactoryName $DataFactory.DataFactoryName `
                                   -Name "MyTrigger" -DefinitionFile ".\MyTrigger.json"
   ```
4. Standardmäßig befindet sich der Trigger im beendeten Zustand. Starten Sie den Trigger durch Ausführen des Cmdlets **Start-AzDataFactoryV2Trigger**. 

   ```powershell
   Start-AzDataFactoryV2Trigger -ResourceGroupName $ResGrp.ResourceGroupName `
                                     -DataFactoryName $DataFactory.DataFactoryName `
                                     -Name "MyTrigger" 
   ```
5. Bestätigen Sie, dass der Trigger sich im gestarteten Zustand befindet, indem Sie das Cmdlet **Get-AzDataFactoryV2Trigger** ausführen. 

   ```powershell
   Get-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName `
                                   -DataFactoryName $DataFactoryName `
                                   -Name "MyTrigger"     
   ```    
6. Führen Sie nach der nächsten vollen Stunde den folgenden Befehl aus. Wenn die aktuelle Uhrzeit z.B. 15:25 UTC ist, führen Sie den Befehl um 16:00 UTC aus. 
    
   ```powershell
   Get-AzDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName `
                                      -DataFactoryName $DataFactoryName `
                                      -TriggerName "MyTrigger" `
                                      -TriggerRunStartedAfter "2017-12-06" `
                                      -TriggerRunStartedBefore "2017-12-09"
   ```

   Sie können in Ihrem Azure SQL Server die folgende Abfrage für die SSISDB-Datenbank ausführen, um zu überprüfen, ob das Paket ausgeführt wurde. 

   ```sql
   select * from catalog.executions
   ```

## <a name="next-steps"></a>Nächste Schritte
Informationen finden Sie im folgenden Blogbeitrag:
-   [Modernisieren und Erweitern von ETL/ELT-Workflows mit SSIS-Aktivitäten in ADF-Pipelines](https://blogs.msdn.microsoft.com/ssis/2018/05/23/modernize-and-extend-your-etlelt-workflows-with-ssis-activities-in-adf-pipelines/)
