---
title: Problembehandlung für Azure Data Factory | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie in Azure Data Factory Fehler bei externen Steuerungsaktivitäten beheben können.
services: data-factory
author: nabhishek
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 8/26/2019
ms.author: abnarain
ms.reviewer: craigg
ms.openlocfilehash: ed466b072a771c3aa288a96fa4a0037c31b875f9
ms.sourcegitcommit: c79aa93d87d4db04ecc4e3eb68a75b349448cd17
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2019
ms.locfileid: "71091992"
---
# <a name="troubleshoot-azure-data-factory"></a>Problembehandlung für Azure Data Factory

In diesem Artikel werden gängige Methoden zur Problembehandlung für externe Steuerungsaktivitäten in Azure Data Factory beschrieben.

## <a name="connector-and-copy-activity"></a>Connector und Kopieraktivität

Informationen zu Connectorproblemen, z. B. Auftreten eines Fehlers bei der Kopieraktivität, finden Sie unter [Problembehandlung für Azure Data Factory-Connectors](connector-troubleshoot-guide.md).

## <a name="azure-databricks"></a>Azure Databricks

### <a name="error-code--3200"></a>Fehlercode:  3200

- **Meldung**: Fehler 403.

- **Ursache**: `The Databricks access token has expired.`

- **Empfehlung**: Das Azure Databricks-Zugriffstoken ist standardmäßig 90 Tage lang gültig. Erstellen Sie ein neues Token, und aktualisieren Sie den verknüpften Dienst.


### <a name="error-code--3201"></a>Fehlercode:  3201

- **Meldung**: `Missing required field: settings.task.notebook_task.notebook_path.`

- **Ursache**: `Bad authoring: Notebook path not specified correctly.`

- **Empfehlung**: Geben Sie den Notebook-Pfad in der Databricks-Aktivität an.

<br/>

- **Meldung**: `Cluster   ... does not exist.`

- **Ursache**: `Authoring error: Databricks cluster does not exist or has been deleted.`

- **Empfehlung**: Überprüfen Sie, ob der Databricks-Cluster vorhanden ist.

<br/>

- **Meldung**: `Invalid Python file URI.... Please visit Databricks user guide for supported URI schemes.`

- **Ursache**: `Bad authoring.`

- **Empfehlung**: Geben Sie entweder absolute Pfade für die Adressierungsschemas der Arbeitsbereiche oder `dbfs:/folder/subfolder/foo.py` für im Databricks-Dateisystem gespeicherte Dateien an.

<br/>

- **Meldung**: `{0} LinkedService should have domain and accessToken as required properties.`

- **Ursache**: `Bad authoring.`

- **Empfehlung**: Überprüfen Sie die [Definition des verknüpften Diensts](compute-linked-services.md#azure-databricks-linked-service).

<br/>

- **Meldung**: `{0} LinkedService should specify either existing cluster ID or new cluster information for creation.`

- **Ursache**: `Bad authoring.`

- **Empfehlung**: Überprüfen Sie die [Definition des verknüpften Diensts](compute-linked-services.md#azure-databricks-linked-service).

<br/>

- **Meldung**: `Node type Standard_D16S_v3 is not supported. Supported node types:   Standard_DS3_v2, Standard_DS4_v2, Standard_DS5_v2, Standard_D8s_v3,   Standard_D16s_v3, Standard_D32s_v3, Standard_D64s_v3, Standard_D3_v2,   Standard_D8_v3, Standard_D16_v3, Standard_D32_v3, Standard_D64_v3,   Standard_D12_v2, Standard_D13_v2, Standard_D14_v2, Standard_D15_v2,   Standard_DS12_v2, Standard_DS13_v2, Standard_DS14_v2, Standard_DS15_v2,   Standard_E8s_v3, Standard_E16s_v3, Standard_E32s_v3, Standard_E64s_v3,   Standard_L4s, Standard_L8s, Standard_L16s, Standard_L32s, Standard_F4s,   Standard_F8s, Standard_F16s, Standard_H16, Standard_F4s_v2, Standard_F8s_v2,   Standard_F16s_v2, Standard_F32s_v2, Standard_F64s_v2, Standard_F72s_v2,   Standard_NC12, Standard_NC24, Standard_NC6s_v3, Standard_NC12s_v3,   Standard_NC24s_v3, Standard_L8s_v2, Standard_L16s_v2, Standard_L32s_v2,   Standard_L64s_v2, Standard_L80s_v2.`

- **Ursache**: `Bad authoring.`

- **Empfehlung**: Weitere Informationen erhalten Sie in der Fehlermeldung. 

<br/>

### <a name="error-code--3202"></a>Fehlercode:  3202

- **Meldung**: `There were already 1000 jobs created in past 3600 seconds, exceeding rate limit:   1000 job creations per 3600 seconds.`

- **Ursache**: `Too many Databricks runs in an hour.`

- **Empfehlung**: Überprüfen Sie alle Pipelines, die den Databricks-Arbeitsbereich verwenden, auf ihre Auftragserstellungsrate.  Wenn Pipelines insgesamt zu viele Databricks-Ausführungen gestartet haben, migrieren Sie einige Pipelines zu einem neuen Arbeitsbereich.

<br/>

- **Meldung**: `Could not parse request object: Expected 'key' and 'value' to be set for JSON map field base_parameters, got 'key: "..."' instead.`

- **Ursache**: `Authoring error: No value provided for the parameter.`

- **Empfehlung**: Überprüfen Sie den JSON-Code der Pipeline, und vergewissern Sie sich, dass für alle Parameter im Notebook „baseParameters“ ein nicht leerer Wert angegeben ist.

<br/>

- **Meldung**: `User: `SimpleUserContext{userId=..., name=user@company.com, orgId=...}` is not   authorized to access cluster.`

- **Ursache:** Der Benutzer, der das Zugriffstoken generiert hat, ist nicht berechtigt, auf den im verknüpften Dienst angegebenen Databricks-Cluster zuzugreifen.

- **Empfehlung**: Stellen Sie sicher, dass der Benutzer über die erforderlichen Berechtigungen im Arbeitsbereich verfügt.


### <a name="error-code--3203"></a>Fehlercode:  3203

- **Meldung**: `The cluster is in Terminated state, not available to receive jobs. Please fix the cluster or retry later.`

- **Ursache:** Der Cluster wurde beendet. Für interaktive Cluster ist dies möglicherweise eine Racebedingung.

- **Empfehlung**: Die beste Möglichkeit, dies zu vermeiden, ist die Verwendung von Auftragsclustern.


### <a name="error-code--3204"></a>Fehlercode:  3204

- **Meldung**: `Job execution failed.`

- **Ursache:**  Fehlermeldungen weisen auf verschiedene Probleme hin, z. B. einen unerwarteten Clusterzustand oder eine bestimmte Aktivität. Meistens erscheint überhaupt keine Fehlermeldung. 

- **Empfehlung**: –


## <a name="azure-data-lake-analytics"></a>Azure Data Lake Analytics

Die folgende Tabelle gilt für U-SQL.


### <a name="error-code--2709"></a>Fehlercode:  2709

- **Meldung**: `The access token is from the wrong tenant.`

- **Ursache:**  Fehlerhafter Azure Active Directory-Mandant (Azure AD).

- **Empfehlung**: Fehlerhafter Azure Active Directory-Mandant (Azure AD).

<br/>

- **Meldung**: `We cannot accept your job at this moment. The maximum number of queued jobs for   your account is 200. `

- **Ursache:**  Dieser Fehler wird durch die Drosselung von Data Lake Analytics verursacht.

- **Empfehlung**: Reduzieren Sie die Zahl der an Data Lake Analytics übermittelten Aufträge, indem Sie die Data Factory-Trigger und die Parallelitätseinstellungen für Aktivitäten ändern. Alternativ können Sie auch die Grenzwerte für Data Lake Analytics erhöhen.

<br/>

- **Meldung**: `This job was rejected because it requires 24 AUs. This account's administrator-defined policy prevents a job from using more than 5 AUs.`

- **Ursache:**  Dieser Fehler wird durch die Drosselung von Data Lake Analytics verursacht. 

- **Empfehlung**: Reduzieren Sie die Zahl der an Data Lake Analytics übermittelten Aufträge, indem Sie die Data Factory-Trigger und die Parallelitätseinstellungen für Aktivitäten ändern. Alternativ können Sie auch die Grenzwerte für Data Lake Analytics erhöhen.


### <a name="error-code--2705"></a>Fehlercode:  2705

- **Meldung**: `Forbidden. ACL verification failed. Either the resource does not exist or the user is not authorized to perform the requested operation.<br/><br/>User is   not able to access Data Lake Store.  <br/><br/>User is  not authorized to use Data Lake Analytics.`

- **Ursache:**  Der angegebene Dienstprinzipal oder das angegebene Zertifikat hat keinen Zugriff auf die Datei im Speicher.

- **Empfehlung**: Stellen Sie sicher, dass der Dienstprinzipal oder das Zertifikat, das der Benutzer für Data Lake Analytics-Aufträge bereitstellt, Zugriff auf das Data Lake Analytics-Konto und die Data Lake Storage-Standardinstanz aus dem Stammordner hat.


### <a name="error-code--2711"></a>Fehlercode:  2711

- **Meldung**: `Forbidden. ACL verification failed. Either the resource does not exist or the user is not authorized to perform the requested operation.<br/><br/>User is   not able to access Data Lake Store.  <br/><br/>User is  not authorized to use Data Lake Analytics.`

- **Ursache:**  Der angegebene Dienstprinzipal oder das angegebene Zertifikat hat keinen Zugriff auf die Datei im Speicher.

- **Empfehlung**: Stellen Sie sicher, dass der Dienstprinzipal oder das Zertifikat, das der Benutzer für Data Lake Analytics-Aufträge bereitstellt, Zugriff auf das Data Lake Analytics-Konto und die Data Lake Storage-Standardinstanz aus dem Stammordner hat.

<br/>

- **Meldung**: `Cannot find the 'Azure Data Lake Store' file or folder.`

- **Ursache:**  The path to the U-SQL file is wrong, or the linked service credentials don't have access. (Der Pfad zur USQL-Datei ist falsch, oder die Anmeldeinformationen des verknüpften Diensts haben keinen Zugriff.)

- **Empfehlung**: Überprüfen Sie den Pfad und die im verknüpften Dienst angegebenen Anmeldeinformationen.


### <a name="error-code--2704"></a>Fehlercode:  2704

- **Meldung**: `Forbidden. ACL verification failed. Either the resource does not exist or the user is not authorized to perform the requested operation.<br/><br/>User is   not able to access Data Lake Store.  <br/><br/>User is  not authorized to use Data Lake Analytics.`

- **Ursache:**  Der angegebene Dienstprinzipal oder das angegebene Zertifikat hat keinen Zugriff auf die Datei im Speicher.

- **Empfehlung**: Stellen Sie sicher, dass der Dienstprinzipal oder das Zertifikat, das der Benutzer für Data Lake Analytics-Aufträge bereitstellt, Zugriff auf das Data Lake Analytics-Konto und die Data Lake Storage-Standardinstanz aus dem Stammordner hat.


### <a name="error-code--2707"></a>Fehlercode:  2707

- **Meldung**: `Cannot resolve the account of AzureDataLakeAnalytics. Please check 'AccountName' and   'DataLakeAnalyticsUri'.`

- **Ursache:**  Das Data Lake Analytics-Konto im verlinkten Dienst ist falsch.

- **Empfehlung**: Stellen Sie sicher, dass das richtige Konto angegeben wurde.


### <a name="error-code--2703"></a>Fehlercode:  2703

- **Meldung**: `Error Id: E_CQO_SYSTEM_INTERNAL_ERROR (or any error that starts with "Error   Id:").`

- **Ursache:**  Dieser Fehler stammt aus Data Lake Analytics. 

- **Empfehlung**: Alle Fehlermeldungen, die dem Beispiel ähneln, geben an, dass der Auftrag an Data Lake Analytics übermittelt wurde und dort für das Skript Fehler aufgetreten sind. Untersuchen Sie dies in Data Lake Analytics. Navigieren Sie im Portal zum Data Lake Analytics-Konto, und suchen Sie nach dem Auftrag, indem Sie die Aktivitätsausführungs-ID von Data Factory verwenden (nicht die Pipelineausführungs-ID). Unter diesem Auftrag finden Sie weitere Informationen zum Fehler und zur entsprechenden Problembehandlung. Wenn die Lösung nicht klar ist, sollten Sie sich an das Data Lake Analytics-Supportteam wenden und die Auftrags-ID mit Ihrem Kontonamen und der Auftrags-ID angeben.



## <a name="azure-functions"></a>Azure-Funktionen

### <a name="error-code--3602"></a>Fehlercode:  3602

- **Meldung**: `Invalid HttpMethod: {method}.`

- **Ursache:** Die in der Aktivitätsnutzlast angegebene HTTP-Methode wird von der Azure-Funktionsaktivität nicht unterstützt. 

- **Empfehlung**: Die unterstützten HTTP-Methoden sind PUT, POST, GET, DELETE, OPTIONS, HEAD und TRACE.


### <a name="error-code--3603"></a>Fehlercode:  3603

- **Meldung**: `Response content is not a valid JObject.`

- **Ursache:** Die aufgerufene Azure-Funktion hat keine JSON-Nutzlast in der Antwort zurückgegeben. Die Aktivität „Azure-Funktion“ in Data Factory unterstützt nur JSON-Antwortinhalte. 

- **Empfehlung**: Aktualisieren Sie die Azure-Funktion, um eine gültige JSON-Payload zurückzugeben. Beispielsweise kann eine C#-Funktion `(ActionResult)new<OkObjectResult("{`\"Id\":\"123\"`}");` zurückgeben.


### <a name="error-code--3606"></a>Fehlercode:  3606

- **Meldung**: `Azure function activity missing function key.`

- **Ursache:** Die Definition der Azure-Funktionsaktivität ist nicht vollständig. 

- **Empfehlung**: Überprüfen Sie, ob die JSON-Definition der Azure-Funktionsaktivität eine Eigenschaft mit dem Namen „functionKey“ aufweist.


### <a name="error-code--3607"></a>Fehlercode:  3607

- **Meldung**: `Azure function activity missing function name.`

- **Ursache:** Die Definition der Azure-Funktionsaktivität ist nicht vollständig. 

- **Empfehlung**: Überprüfen Sie, ob die JSON-Definition der Azure-Funktionsaktivität eine Eigenschaft mit dem Namen „functionName“ aufweist.


### <a name="error-code--3608"></a>Fehlercode:  3608

- **Meldung**: `Call to provided Azure function '{FunctionName}' failed with status-'{statusCode}' and message - '{message}'.` 

- **Ursache:** Die Details der Azure-Funktion in der Aktivitätsdefinition sind möglicherweise falsch. 

- **Empfehlung**: Korrigieren Sie die Details der Azure-Funktion, und versuchen Sie es noch mal.


### <a name="error-code--3609"></a>Fehlercode:  3609

- **Meldung**: `Azure function activity missing functionAppUrl.` 

- **Ursache:** Die Definition der Azure-Funktionsaktivität ist nicht vollständig. 

- **Empfehlung**: Überprüfen Sie, ob die JSON-Definition der Azure-Funktionsaktivität eine Eigenschaft mit dem Namen „functionAppUrl“ aufweist.


### <a name="error-code--3610"></a>Fehlercode:  3610

- **Meldung**: `There was an error while calling endpoint.`

- **Ursache:** Die Funktions-URL ist unter Umständen nicht korrekt.

- **Empfehlung**: Stellen Sie sicher, dass der Wert für „functionAppUrl“ im JSON-Code der Aktivität richtig ist, und wiederholen Sie den Vorgang.


### <a name="error-code--3611"></a>Fehlercode:  3611

- **Meldung**: `Azure function activity missing Method in JSON.` 

- **Ursache:** Die Definition der Azure-Funktionsaktivität ist nicht vollständig.

- **Empfehlung**: Überprüfen Sie, ob die JSON-Definition der Azure-Funktionsaktivität eine Eigenschaft mit dem Namen „method“ aufweist.


### <a name="error-code--3612"></a>Fehlercode:  3612

- **Meldung**: `Azure function activity missing LinkedService definition in JSON.`

- **Ursache:** Die Definition der Azure-Funktionsaktivität ist unter Umständen nicht vollständig.

- **Empfehlung**: Überprüfen Sie, ob die JSON-Definition der AzureFunction-Eingabeaktivität verknüpfte Dienstdetails aufweist.


## <a name="azure-machine-learning"></a>Azure Machine Learning


### <a name="error-code--4101"></a>Fehlercode:  4101

- **Meldung**: `AzureMLExecutePipeline activity '%activityName;' has invalid value for property '%propertyName;'.`

- **Ursache:** Ungültiges Format oder fehlende Definition einer Eigenschaft.

- **Empfehlung**:  Überprüfen Sie, ob die Aktivität mit den richtigen Daten definiert ist.


### <a name="error-code--4110"></a>Fehlercode:  4110

- **Meldung**: In der AzureMLExecutePipeline-Aktivität fehlt die LinkedService-Definition in JSON.

- **Ursache:** Die AzureMLExecutePipeline-Aktivitätsdefinition ist nicht vollständig.

- **Empfehlung**:  Überprüfen Sie, ob die JSON-Definition der AzureMLExecutePipeline-Aktivität verknüpfte Dienstdetails aufweist.


### <a name="error-code--4111"></a>Fehlercode:  4111

- **Meldung**: `AzureMLExecutePipeline activity has wrong LinkedService type in JSON. Expected LinkedService type: '%expectedLinkedServiceType;', current LinkedService type: Expected LinkedService type: '%currentLinkedServiceType;'.`

- **Ursache:** Falsche Aktivitätsdefinition.

- **Empfehlung**:  Überprüfen Sie, ob die JSON-Definition der AzureMLExecutePipeline-Aktivität die korrekten verknüpften Dienstdetails aufweist.


### <a name="error-code--4112"></a>Fehlercode:  4112

- **Meldung**: `AzureMLService linked service has invalid value for property '%propertyName;'.`

- **Ursache:** Ungültiges Format oder fehlende Definition einer Eigenschaft.

- **Empfehlung**:  Überprüfen Sie, ob die Definition für den verknüpften Dienst die richtigen Daten aufweist.


### <a name="error-code--4121"></a>Fehlercode:  4121

- **Meldung**: `Request sent to AzureML Service for operation '%operation;' failed with http status code '%statusCode;'. Error message from AzureML Service: '%externalMessage;'.`

- **Ursache:** Die für den Zugriff auf Azure ML Service verwendeten Anmeldeinformationen sind abgelaufen.

- **Empfehlung**:  Überprüfen Sie, ob die Anmeldeinformationen gültig sind, und versuchen Sie es noch mal.


### <a name="error-code--4122"></a>Fehlercode:  4122

- **Meldung**: `Request sent to AzureML Service for operation '%operation;' failed with http status code '%statusCode;'. Error message from AzureML Service: '%externalMessage;'.`

- **Ursache:** Die im verknüpften Dienst für Azure ML Service angegebenen Anmeldeinformationen sind ungültig oder verfügen über keine Berechtigungen für diesen Vorgang.

- **Empfehlung**:  Überprüfen Sie, ob die Anmeldeinformationen im verknüpften Dienst gültig sind und Berechtigungen zum Zugriff auf Azure ML Service vorhanden sind.


### <a name="error-code--4123"></a>Fehlercode:  4123

- **Meldung**: `Request sent to AzureML Service for operation '%operation;' failed with http status code '%statusCode;'. Error message from AzureML Service: '%externalMessage;'.`

- **Ursache**: `Properties of the activity such as pipelineParamters are invalid for the Azure ML pipeline.`

- **Empfehlung**:  Überprüfen Sie den Wert der Aktivitätseigenschaften, um die erwartete Nutzlast der im verknüpften Dienst angegebenen veröffentlichten Azure ML-Pipeline abzugleichen.


### <a name="error-code--4124"></a>Fehlercode:  4124

- **Meldung**: `Request sent to AzureML Service for operation '%operation;' failed with http status code '%statusCode;'. Error message from AzureML Service: '%externalMessage;'.`

- **Ursache:** Der veröffentlichte Azure ML-Pipeline-Endpunkt ist nicht vorhanden.

- **Empfehlung**:  Überprüfen Sie, ob der im verknüpften Dienst angegebene veröffentlichte Azure ML-Pipeline-Endpunkt in Azure ML Service vorhanden ist.


### <a name="error-code--4125"></a>Fehlercode:  4125

- **Meldung**: `Request sent to AzureML Service for operation '%operation;' failed with http status code '%statusCode;'. Error message from AzureML Service: '%externalMessage;'.`

- **Ursache:** Serverfehler in Azure ML Service.

- **Empfehlung**:  Versuchen Sie es später noch mal. Wenn das Problem weiterhin besteht, wenden Sie sich an das Azure ML Service-Team.


### <a name="error-code--4126"></a>Fehlercode:  4126

- **Meldung**: `AzureML pipeline run failed with status: '%amlPipelineRunStatus;'. Azure ML pipeline run Id: '%amlPipelineRunId;'. Please check in AzureMLService for more error loggings.`

- **Ursache:** Fehler beim Ausführen der AzureML-Pipeline.

- **Empfehlung**:  Überprüfen Sie in AzureMLService, ob weitere Fehlerprotokolle vorhanden sind, und korrigieren Sie die ML-Pipeline.


## <a name="custom"></a>Benutzerdefiniert

Die folgende Tabelle gilt für Azure Batch.


### <a name="error-code--2500"></a>Fehlercode:  2500

- **Meldung**: `Hit unexpected exception and execution failed.`

- **Ursache**: `Can't launch command, or the program returned an error code.`

- **Empfehlung**:  Stellen Sie sicher, dass die ausführbare Datei vorhanden ist. Wenn das Programm gestartet wurde, sollten Sie sicherstellen, dass die Dateien *stdout.txt* und *stderr.txt* in das Speicherkonto hochgeladen wurden. Es empfiehlt sich, im Code umfangreiche Protokolle zum Debuggen auszugeben.


### <a name="error-code--2501"></a>Fehlercode:  2501

- **Meldung**: `Cannot access user batch account; please check batch account settings.`

- **Ursache:** Falscher Batch-Zugriffsschlüssel oder Poolname.

- **Empfehlung**: Überprüfen Sie den Poolnamen und den Batch-Zugriffsschlüssel im verknüpften Dienst.


### <a name="error-code--2502"></a>Fehlercode:  2502

- **Meldung**: `Cannot access user storage account; please check storage account settings.`

- **Ursache:** Falscher Speicherkontoname oder Zugriffsschlüssel.

- **Empfehlung**: Überprüfen Sie den Speicherkontonamen und den Zugriffsschlüssel im verknüpften Dienst.


### <a name="error-code--2504"></a>Fehlercode:  2504

- **Meldung**: `Operation returned an invalid status code 'BadRequest'.` 

- **Ursache:** Zu viele Dateien in „folderPath“ in der benutzerdefinierten Aktivität. Die Gesamtgröße von „resourceFiles“ darf nicht mehr als 32.768 Zeichen betragen.

- **Empfehlung**: Entfernen Sie nicht benötigte Dateien. Oder zippen Sie sie, und fügen Sie einen Befehl zum Entpacken hinzu, um sie zu extrahieren. Verwenden Sie beispielsweise `powershell.exe -nologo -noprofile   -command "& { Add-Type -A 'System.IO.Compression.FileSystem';   [IO.Compression.ZipFile]::ExtractToDirectory($zipFile, $folder); }" ;  $folder\yourProgram.exe`.


### <a name="error-code--2505"></a>Fehlercode:  2505

- **Meldung**: `Cannot create Shared Access Signature unless Account Key credentials are used.`

- **Ursache:** Benutzerdefinierte Aktivitäten unterstützen nur Speicherkonten, die einen Zugriffsschlüssel verwenden.

- **Empfehlung**: Weitere Informationen erhalten Sie in der Fehlerbeschreibung.


### <a name="error-code--2507"></a>Fehlercode:  2507

- **Meldung**: `The folder path does not exist or is empty: ....`

- **Ursache:** Unter dem angegebenen Pfad sind im Speicherkonto keine Dateien vorhanden.

- **Empfehlung**: „folderPath“ muss die ausführbaren Dateien enthalten, die Sie ausführen möchten.


### <a name="error-code--2508"></a>Fehlercode:  2508

- **Meldung**: `There are duplicate files in the resource folder.`

- **Ursache:** Mehrere Dateien mit dem gleichen Namen sind in verschiedenen Unterordnern von „folderPath“ vorhanden.

- **Empfehlung**: Benutzerdefinierte Aktivitäten vereinfachen die Ordnerstruktur unter „folderPath“.  Wenn Sie die Ordnerstruktur beibehalten möchten, zippen Sie die Dateien, und extrahieren Sie sie in Azure Batch mit einem Befehl zum Entzippen. Verwenden Sie beispielsweise `powershell.exe -nologo -noprofile   -command "& { Add-Type -A 'System.IO.Compression.FileSystem';   [IO.Compression.ZipFile]::ExtractToDirectory($zipFile, $folder); }" ;   $folder\yourProgram.exe`.


### <a name="error-code--2509"></a>Fehlercode:  2509

- **Meldung**: `Batch   url ... is invalid; it must be in Uri format.` 

- **Ursache:** Batch-URLs müssen `https://mybatchaccount.eastus.batch.azure.com` ähneln.

- **Empfehlung**: Weitere Informationen erhalten Sie in der Fehlerbeschreibung.


### <a name="error-code--2510"></a>Fehlercode:  2510

- **Meldung**: `An   error occurred while sending the request.`

- **Ursache:** Die Batch-URL ist ungültig. 

- **Empfehlung**: Überprüfen Sie die Batch-URL.


## <a name="hdinsight"></a>HDInsight

Die folgende Tabelle gilt für Spark, Hive, MapReduce, Pig und Hadoop Streaming.


### <a name="error-code--2300"></a>Fehlercode:  2.300

- **Meldung**: `Hadoop job submission failed. Error: The remote name could not be resolved. <br/><br/>The cluster is not found.`

- **Ursache:** Der angegebene Cluster-URI ist ungültig. 

- **Empfehlung**:  Stellen Sie sicher, dass der Cluster nicht gelöscht wurde und der angegebene URI richtig ist. Wenn Sie den URI in einem Browser öffnen, sollte die Ambari-Benutzeroberfläche angezeigt werden. Wenn sich der Cluster in einem virtuellen Netzwerk befindet, sollte der URI der private URI sein. Verwenden Sie zum Öffnen eine VM, die Teil desselben virtuellen Netzwerks ist. Weitere Informationen finden Sie unter [Herstellen einer direkten Verbindung mit Apache Hadoop-Diensten](https://docs.microsoft.com/azure/hdinsight/hdinsight-extend-hadoop-virtual-network#directly-connect-to-apache-hadoop-services).

<br/>

- **Meldung**: `Hadoop job submission failed. Job: …, Cluster: …/. Error: A task was canceled.`

- **Ursache:** Die Auftragserteilung war zeitlich begrenzt. 

- **Empfehlung**: Das Problem kann entweder ein allgemeines HDInsight-Verbindungs- oder ein Netzwerkverbindungsproblem sein. Bestätigen Sie zunächst, dass die HDInsight Ambari-Benutzeroberfläche von jedem Browser aus verfügbar ist. Bestätigen Sie, dass Ihre Anmeldeinformationen noch gültig sind. Wenn Sie eine selbst gehostete integrierte Laufzeit (IR) verwenden, sollten Sie sicherstellen, dass dies von der VM oder dem Computer aus geschieht, auf denen die selbstgehostete IR installiert ist. Übermitteln Sie den Auftrag dann erneut über Data Factory. Wenn weiterhin Fehler auftreten, sollten Sie sich an das Data Factory-Team wenden.


- **Meldung**: `Unauthorized: Ambari user name or password is incorrect  <br/><br/>Unauthorized: User admin is locked out in Ambari.   <br/><br/>403 - Forbidden: Access is denied.`

- **Ursache:** Die Anmeldeinformationen für HDInsight sind falsch oder abgelaufen.

- **Empfehlung**: Korrigieren Sie die Anmeldeinformation, und stellen Sie den verknüpften Dienst erneut bereit. Stellen Sie zunächst sicher, dass die Anmeldeinformationen in HDInsight funktionieren. Öffnen Sie dazu den Cluster-URI in einem Browser, und versuchen Sie, sich anzumelden. Wenn die Anmeldeinformationen nicht funktionieren, können Sie sie im Azure-Portal zurücksetzen.

<br/>

- **Meldung**: `502 - Web server received an invalid response while acting as a gateway or proxy server. <br/>Bad gateway.`

- **Ursache:** Dieser Fehler stammt von HDInsight.

- **Empfehlung**: Dieser Fehler stammt vom HDInsight-Cluster. Weitere Informationen finden Sie auf den Seiten zu den Themen [Ambari-Benutzeroberfläche Fehler 502](https://hdinsight.github.io/ambari/ambari-ui-502-error.html), [Fehler 502 bei der Verbindung zum Spark Thrift Server](https://hdinsight.github.io/spark/spark-thriftserver-errors.html) und [Fehlerbehebung bei ungültigen Gateway-Fehlern im Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-troubleshooting-502).

<br/>

- **Meldung**: `Hadoop job submission failed. Job: …, Cluster: ... Error:   {\"error\":\"Unable to service the submit job request as   templeton service is busy with too many submit job requests. Please wait for some time before retrying the operation. Please refer to the config   templeton.parallellism.job.submit to configure concurrent requests. <br/><br/>Hadoop job submission failed. Job: xx, Cluster: name.   Error: {\"error\":\"java.io.IOException:   org.apache.hadoop.yarn.exceptions.YarnException: Failed to submit   application_1561147195099_3730 to YARN :   org.apache.hadoop.security.AccessControlException: Queue root.joblauncher already has 500 applications, cannot accept submission of application:   application_1561147195099_3730\`

- **Ursache:** Zu viele Aufträge werden gleichzeitig an HDInsight übermittelt.

- **Empfehlung**: Ziehen Sie in Betracht, die Anzahl von Aufträgen, die gleichzeitig an HDInsight übermittelt werden, zu begrenzen. Wenn die Aufträge über die gleiche Aktivität übermittelt werden, helfen Ihnen die entsprechenden Informationen in der Data Factory-Aktivität für Parallelität weiter. Ändern Sie die Trigger so, dass die gleichzeitigen Pipelineausführungen über die Zeit verteilt werden. Weitere Informationen über die Anpassung von `templeton.parallellism.job.submit`, wie im Fehler angegeben, finden Sie in der HDInsight-Dokumentation.


### <a name="error-code--2303"></a>Fehlercode:  2303

- **Meldung**: `Hadoop job failed with exit code '5'. See   'wasbs://adfjobs@xx.blob.core.windows.net/StreamingJobs/da4afc6d-7836-444e-bbd5-635fce315997/18_06_2019_05_36_05_050/stderr' for more details. <br/><br/>Hive execution failed with error code 'UserErrorHiveOdbcCommandExecutionFailure'.   See 'wasbs://adfjobs@xx.blob.core.windows.net/HiveQueryJobs/16439742-edd5-4efe-adf6-9b8ff5770beb/18_06_2019_07_37_50_477/Status/hive.out' for more details.`

- **Ursache:** Der Auftrag wurde an HDInsight übermittelt, und in HDInsight ist ein Fehler aufgetreten.

- **Empfehlung**: Der Auftrag wurde erfolgreich an HDInsight übermittelt. Bei seiner Ausführung ist ein Fehler im Cluster aufgetreten. Öffnen Sie den Auftrag entweder in der HDInsight Ambari-Benutzeroberfläche, oder öffnen Sie die Datei im Speicher, wie in der Fehlermeldung angegeben. In der Datei werden die Fehlerdetails angezeigt.


### <a name="error-code--2310"></a>Fehlercode:  2310

- **Meldung**: `Hadoop job submission failed. Error: The remote name could not be resolved. <br/><br/>The cluster is not found.`

- **Ursache:** Der angegebene Cluster-URI ist ungültig. 

- **Empfehlung**:  Stellen Sie sicher, dass der Cluster nicht gelöscht wurde und der angegebene URI richtig ist. Wenn Sie den URI in einem Browser öffnen, sollte die Ambari-Benutzeroberfläche angezeigt werden. Wenn sich der Cluster in einem virtuellen Netzwerk befindet, sollte der URI der private URI sein. Verwenden Sie zum Öffnen eine VM, die Teil desselben virtuellen Netzwerks ist. Weitere Informationen finden Sie unter [Herstellen einer direkten Verbindung mit Apache Hadoop-Diensten](https://docs.microsoft.com/azure/hdinsight/hdinsight-extend-hadoop-virtual-network#directly-connect-to-apache-hadoop-services).

<br/>

- **Meldung**: `502 - Web server received an invalid response while acting as a gateway or proxy server. <br/>Bad gateway.`

- **Ursache:** Dieser Fehler stammt von HDInsight.

- **Empfehlung**: Dieser Fehler stammt vom HDInsight-Cluster. Weitere Informationen finden Sie auf den Seiten zu den Themen [Ambari-Benutzeroberfläche Fehler 502](https://hdinsight.github.io/ambari/ambari-ui-502-error.html), [Fehler 502 bei der Verbindung zum Spark Thrift Server](https://hdinsight.github.io/spark/spark-thriftserver-errors.html) und [Fehlerbehebung bei ungültigen Gateway-Fehlern im Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-troubleshooting-502).

<br/>

- **Meldung**: `java.lang.NullPointerException`

- **Ursache:** Dieser Fehler tritt auf, wenn der Auftrag an einen Spark-Cluster gesendet wird. 

- **Empfehlung**: Diese Ausnahme kommt von HDInsight. Sie verbirgt das eigentliche Problem. Wenden Sie sich an das HDInsight-Team, um Unterstützung zu erhalten. Informieren Sie das Team über den Clusternamen und den Zeitbereich der Aktivitätsausführung.


### <a name="error-code--2347"></a>Fehlercode:  2347

- **Meldung**: `Hadoop job failed with exit code '5'. See 'wasbs://adfjobs@xx.blob.core.windows.net/StreamingJobs/da4afc6d-7836-444e-bbd5-635fce315997/18_06_2019_05_36_05_050/stderr' for more details. <br/><br/>Hive execution failed with error code 'UserErrorHiveOdbcCommandExecutionFailure'.   See 'wasbs://adfjobs@xx.blob.core.windows.net/HiveQueryJobs/16439742-edd5-4efe-adf6-9b8ff5770beb/18_06_2019_07_37_50_477/Status/hive.out' for more details.`

- **Ursache:** Der Auftrag wurde an HDInsight übermittelt, und in HDInsight ist ein Fehler aufgetreten.

- **Empfehlung**: Der Auftrag wurde erfolgreich an HDInsight übermittelt. Bei seiner Ausführung ist ein Fehler im Cluster aufgetreten. Öffnen Sie den Auftrag entweder in der HDInsight Ambari-Benutzeroberfläche, oder öffnen Sie die Datei im Speicher, wie in der Fehlermeldung angegeben. In der Datei werden die Fehlerdetails angezeigt.


### <a name="error-code--2328"></a>Fehlercode:  2328

- **Meldung**: `Internal server error occurred while processing the request. Please retry the request or contact support. `

- **Ursache:** Dieser Fehler tritt bei HDInsight (bedarfsgesteuert) auf.

- **Empfehlung**: Dieser Fehler stammt vom HDInsight-Dienst, wenn bei der HDInsight-Bereitstellung Fehler aufgetreten sind. Wenden Sie sich an das HDInsight-Team, und geben Sie den Namen des bedarfsgesteuerten Clusters an.



## <a name="web-activity"></a>Webaktivität

### <a name="error-code--2108"></a>Fehlercode:  2108

- **Meldung**: `Invalid HttpMethod: '...'.`

- **Ursache:** Die Webaktivität unterstützt die HTTP-Methode, die in der Aktivitätsnutzlast angegeben ist, nicht.

- **Empfehlung**:  Die unterstützten HTTP-Methoden sind „PUT“, „POST“, „GET“ und „DELETE“.

<br/>

- **Meldung**: `Invalid Server Error 500.`

- **Ursache:** Interner Fehler am Endpunkt.

- **Empfehlung**:  Verwenden Sie Fiddler/Postman, um die Funktionalität für die URL zu überprüfen.

<br/>

- **Meldung**: `Unauthorized 401.`

- **Ursache:** Fehlende gültige Authentifizierung für die Anforderung.

- **Empfehlung**:  Das Token ist möglicherweise abgelaufen. Stellen Sie eine gültige Authentifizierungsmethode zur Verfügung. Verwenden Sie Fiddler/Postman, um die Funktionalität für die URL zu überprüfen.

<br/>

- **Meldung**: `Forbidden 403.`

- **Ursache:** Fehlende erforderliche Berechtigungen.

- **Empfehlung**:  Überprüfen Sie die Benutzerberechtigungen für die Ressource, auf die zugegriffen wird. Verwenden Sie Fiddler/Postman, um die Funktionalität für die URL zu überprüfen.

<br/>

- **Meldung**: `Bad Request 400.`

- **Ursache:** Ungültige HTTP-Anforderung.

- **Empfehlung**:   Überprüfen Sie die URL, das Verb und den Text der Anforderung. Verwenden Sie Fiddler oder Postman, um die Anforderung zu überprüfen.

<br/>

- **Meldung**: `Not found 404.` 

- **Ursache:** Die Ressource wurde nicht gefunden.   

- **Empfehlung**:  Verwenden Sie Fiddler oder Postman, um die Anforderung zu überprüfen.

<br/>

- **Meldung**: `Service unavailable.`

- **Ursache:** Der Dienst ist nicht verfügbar.

- **Empfehlung**:  Verwenden Sie Fiddler oder Postman, um die Anforderung zu überprüfen.

<br/>

- **Meldung**: `Unsupported Media Type.`

- **Ursache:** Der Inhaltstyp stimmt nicht mit dem Text der Webaktivität überein.

- **Empfehlung**:  Geben Sie den Inhaltstyp an, der dem Nutzlastformat entspricht. Verwenden Sie Fiddler oder Postman, um die Anforderung zu überprüfen.

<br/>

- **Meldung**: `The resource you are looking for has been removed, has had its name changed, or is temporarily unavailable.`

- **Ursache:** Die Ressource ist nicht verfügbar. 

- **Empfehlung**:  Verwenden Sie Fiddler oder Postman, um den Endpunkt zu überprüfen.

<br/>

- **Meldung**: `The page you are looking for cannot be displayed because an invalid method (HTTP verb) is being used.`

- **Ursache:** Eine falsche Webaktivitätsmethode wurde in der Anforderung angegeben.

- **Empfehlung**:  Verwenden Sie Fiddler oder Postman, um den Endpunkt zu überprüfen.

<br/>

- **Meldung**: `invalid_payload`

- **Ursache:** The Web Activity body is incorrect. (Falscher Text für die Webaktivität.)

- **Empfehlung**:  Verwenden Sie Fiddler oder Postman, um den Endpunkt zu überprüfen.


### <a name="error-code--2208"></a>Fehlercode:  2208

- **Meldung**: `Invoking Web Activity failed with HttpStatusCode - {0}.`

- **Ursache:** Der Zieldienst hat einen Fehlerstatus zurückgegeben.

- **Empfehlung**:  Verwenden Sie Fiddler/Postman, um die Anforderung zu überprüfen.


### <a name="error-code--2308"></a>Fehlercode:  2308

- **Meldung**: `No response from the endpoint. Possible causes: network connectivity, DNS failure, server certificate validation or timeout.`

- **Ursache:** Dieser Fehler kann mehrere Ursachen haben, wie etwa die Netzwerkkonnektivität, DNS-Fehler, die Überprüfung des Serverzertifikats oder Zeitlimits.

- **Empfehlung**:  Verwenden Sie Fiddler/Postman, um die Anforderung zu überprüfen.


Verwenden Sie Fiddler zum Erstellen einer HTTP-Sitzung der überwachten Webanwendung:

1. Laden Sie [Fiddler](https://www.telerik.com/download/fiddler) herunter, und installieren und öffnen Sie die Anwendung.

1. Wenn Ihre Webanwendung HTTPS verwendet, wechseln Sie zu **Tools** > **Fiddler-Optionen** > **HTTPS**. Wählen Sie **Capture HTTPS CONNECTs** (HTTPS CONNECTs erfassen) und **Decrypt HTTPS Traffic** (HTTPS-Datenverkehr entschlüsseln) aus. 
   
   ![Fiddler-Optionen](media/data-factory-troubleshoot-guide/fiddler-options.png)

1. Wenn in der Anwendung SSL-Zertifikate verwendet werden, fügen Sie das Fiddler-Zertifikat auf Ihrem Gerät hinzu. Wechseln Sie zu **Tools** > **Fiddler-Optionen** > **HTTPS** > **Aktionen** > **Stammzertifikat auf Desktop exportieren**.

1. Deaktivieren Sie die Erfassung, indem Sie zu **Datei** > **Datenverkehr erfassen** wechseln. Oder drücken Sie **F12**.

1. Löschen Sie den Browsercache, sodass alle zwischengespeicherten Elemente gelöscht werden und erneut heruntergeladen werden müssen.

1. Erstellen Sie eine Anforderung: 

   a. Wählen Sie die Registerkarte **Composer** aus.

   b. Legen Sie die HTTP-Methode und die HTTP-URL fest.

   c. Fügen Sie bei Bedarf Header und den Anforderungstext hinzu.

   d. Wählen Sie **Execute**(Ausführen).

9. Starten Sie die Erfassung des Datenverkehrs wieder, und schließen Sie die entsprechende Transaktion auf Ihrer Seite ab.

10. Navigieren Sie anschließend zu **Datei** > **Speichern** > **Alle Sitzungen**.

Weitere Informationen finden Sie unter [Erste Schritte mit Fiddler](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/ConfigureFiddler).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Problembehandlung finden Sie in diesen Ressourcen:

*  [Data Factory-Blog](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Data Factory-Funktionsanfragen](https://feedback.azure.com/forums/270578-data-factory)
*  [Azure-Videos](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [MSDN-Forum](https://social.msdn.microsoft.com/Forums/home?sort=relevancedesc&brandIgnore=True&searchTerm=data+factory)
*  [Stack Overflow-Forum für Data Factory](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Twitter-Informationen über Data Factory](https://twitter.com/hashtag/DataFactory)



