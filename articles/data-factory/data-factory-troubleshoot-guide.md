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
ms.openlocfilehash: 0026aa377a58f6b766a400860692a35440deb962
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73748361"
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

- **Meldung**: `Forbidden. ACL verification failed. Either the resource does not exist or the user is not authorized to perform the requested operation.<br/>  <br/>  User is   not able to access Data Lake Store.  <br/>  <br/>  User is  not authorized to use Data Lake Analytics.`

- **Ursache:**  Der angegebene Dienstprinzipal oder das angegebene Zertifikat hat keinen Zugriff auf die Datei im Speicher.

- **Empfehlung**: Stellen Sie sicher, dass der Dienstprinzipal oder das Zertifikat, das der Benutzer für Data Lake Analytics-Aufträge bereitstellt, Zugriff auf das Data Lake Analytics-Konto und die Data Lake Storage-Standardinstanz aus dem Stammordner hat.


### <a name="error-code--2711"></a>Fehlercode:  2711

- **Meldung**: `Forbidden. ACL verification failed. Either the resource does not exist or the user is not authorized to perform the requested operation.<br/>  <br/>  User is   not able to access Data Lake Store.  <br/>  <br/>  User is  not authorized to use Data Lake Analytics.`

- **Ursache:**  Der angegebene Dienstprinzipal oder das angegebene Zertifikat hat keinen Zugriff auf die Datei im Speicher.

- **Empfehlung**: Stellen Sie sicher, dass der Dienstprinzipal oder das Zertifikat, das der Benutzer für Data Lake Analytics-Aufträge bereitstellt, Zugriff auf das Data Lake Analytics-Konto und die Data Lake Storage-Standardinstanz aus dem Stammordner hat.

<br/>  

- **Meldung**: `Cannot find the 'Azure Data Lake Store' file or folder.`

- **Ursache:**  The path to the U-SQL file is wrong, or the linked service credentials don't have access. (Der Pfad zur USQL-Datei ist falsch, oder die Anmeldeinformationen des verknüpften Diensts haben keinen Zugriff.)

- **Empfehlung**: Überprüfen Sie den Pfad und die im verknüpften Dienst angegebenen Anmeldeinformationen.


### <a name="error-code--2704"></a>Fehlercode:  2704

- **Meldung**: `Forbidden. ACL verification failed. Either the resource does not exist or the user is not authorized to perform the requested operation.<br/>  <br/>  User is   not able to access Data Lake Store.  <br/>  <br/>  User is  not authorized to use Data Lake Analytics.`

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

- **Meldung**: `Invalid HttpMethod: '%method;'.`

- **Ursache:** Die in der Aktivitätsnutzlast angegebene HTTP-Methode wird von der Azure-Funktionsaktivität nicht unterstützt.

- **Empfehlung**:  Die unterstützten HTTP-Methoden sind PUT, POST, GET, DELETE, OPTIONS, HEAD und TRACE.


### <a name="error-code--3603"></a>Fehlercode:  3603

- **Meldung**: `Response Content is not a valid JObject.`

- **Ursache:** Die aufgerufene Azure-Funktion hat keine JSON-Nutzlast in der Antwort zurückgegeben. Die ADF-Aktivität „Azure-Funktion“ unterstützt nur JSON-Antwortinhalte.

- **Empfehlung**:  Aktualisieren Sie die Azure-Funktion so, dass eine gültige JSON-Nutzlast zurückgegeben wird. Eine C#-Funktion kann beispielsweise Folgendes zurückgeben: (ActionResult)new OkObjectResult("{\"Id\":\"123\"}");.


### <a name="error-code--3606"></a>Fehlercode:  3606

- **Meldung**: Azure function activity missing function key. (In der Aktivität der Azure-Funktion fehlt der Funktionsschlüssel.)

- **Ursache:** Die Definition der Azure-Funktionsaktivität ist nicht vollständig.

- **Empfehlung**:  Überprüfen Sie, ob die JSON-Definition der Azure-Funktionsaktivität eine Eigenschaft mit dem Namen „functionKey“ aufweist.


### <a name="error-code--3607"></a>Fehlercode:  3607

- **Meldung**: `Azure function activity missing function name.`

- **Ursache:** Die Definition der Azure-Funktionsaktivität ist nicht vollständig.

- **Empfehlung**:  Überprüfen Sie, ob die JSON-Definition der Azure-Funktionsaktivität eine Eigenschaft mit dem Namen „functionName“ aufweist.


### <a name="error-code--3608"></a>Fehlercode:  3608

- **Meldung**: `Call to provided Azure function '%FunctionName;' failed with status-'%statusCode;' and message - '%message;'.`

- **Ursache:** Die Details der Azure-Funktion in der Aktivitätsdefinition sind möglicherweise falsch.

- **Empfehlung**:  Korrigieren Sie die Details der Azure-Funktion, und versuchen Sie es noch mal.


### <a name="error-code--3609"></a>Fehlercode:  3609

- **Meldung**: `Azure function activity missing functionAppUrl.`

- **Ursache:** Die Definition der Azure-Funktionsaktivität ist nicht vollständig.

- **Empfehlung**:  Überprüfen Sie, ob die JSON-Definition der Azure-Funktionsaktivität eine Eigenschaft mit dem Namen „functionAppUrl“ aufweist.


### <a name="error-code--3610"></a>Fehlercode:  3610

- **Meldung**: `There was an error while calling endpoint.`

- **Ursache:** Die Funktions-URL ist unter Umständen nicht korrekt.

- **Empfehlung**:  Stellen Sie sicher, dass der Wert für „functionAppUrl“ im JSON-Code der Aktivität richtig ist, und wiederholen Sie den Vorgang.


### <a name="error-code--3611"></a>Fehlercode:  3611

- **Meldung**: `Azure function activity missing Method in JSON.`

- **Ursache:** Die Definition der Azure-Funktionsaktivität ist nicht vollständig.

- **Empfehlung**:  Überprüfen Sie, ob die JSON-Definition der Azure-Funktionsaktivität eine Eigenschaft mit dem Namen „method“ aufweist.


### <a name="error-code--3612"></a>Fehlercode:  3612

- **Meldung**: `Azure function activity missing LinkedService definition in JSON.`

- **Ursache:** Die Definition der Azure-Funktionsaktivität ist nicht vollständig.

- **Empfehlung**:  Überprüfen Sie, ob die JSON-Definition der AzureFunction-Eingabeaktivität verknüpfte Dienstdetails aufweist.



## <a name="azure-machine-learning"></a>Azure Machine Learning

### <a name="error-code--4101"></a>Fehlercode:  4101

- **Meldung**: `AzureMLExecutePipeline activity '%activityName;' has invalid value for property '%propertyName;'.`

- **Ursache:** Ungültiges Format oder fehlende Definition der Eigenschaft „%propertyName;“

- **Empfehlung**:  Überprüfen Sie, ob für die Aktivität „%activityName;“ die Eigenschaft „%propertyName;“ mit den richtigen Daten definiert ist.


### <a name="error-code--4110"></a>Fehlercode:  4110

- **Meldung**: `AzureMLExecutePipeline activity missing LinkedService definition in JSON.`

- **Ursache:** Die AzureMLExecutePipeline-Aktivitätsdefinition ist nicht vollständig.

- **Empfehlung**:  Überprüfen Sie, ob die JSON-Definition der AzureMLExecutePipeline-Aktivität verknüpfte Dienstdetails aufweist.


### <a name="error-code--4111"></a>Fehlercode:  4111

- **Meldung**: `AzureMLExecutePipeline activity has wrong LinkedService type in JSON. Expected LinkedService type: '%expectedLinkedServiceType;', current LinkedService type: Expected LinkedService type: '%currentLinkedServiceType;'.`

- **Ursache:** Falsche Aktivitätsdefinition.

- **Empfehlung**:  Überprüfen Sie, ob die JSON-Definition der AzureMLExecutePipeline-Aktivität die korrekten verknüpften Dienstdetails aufweist.


### <a name="error-code--4112"></a>Fehlercode:  4112

- **Meldung**: `AzureMLService linked service has invalid value for property '%propertyName;'.`

- **Ursache:** Ungültiges Format oder fehlende Definition der Eigenschaft „%propertyName;“

- **Empfehlung**:  Überprüfen Sie, ob für den verknüpften Dienst die Eigenschaft „%propertyName;“ mit den richtigen Daten definiert ist.


### <a name="error-code--4121"></a>Fehlercode:  4121

- **Meldung**: `Request sent to Azure ML Service for operation '%operation;' failed with http status code '%statusCode;'. Error message from Azure ML Service: '%externalMessage;'.`

- **Ursache:** Die für den Zugriff auf Azure ML Service verwendeten Anmeldeinformationen sind abgelaufen.

- **Empfehlung**:  Überprüfen Sie, ob die Anmeldeinformationen gültig sind, und versuchen Sie es noch mal.


### <a name="error-code--4122"></a>Fehlercode:  4122

- **Meldung**: `Request sent to Azure ML Service for operation '%operation;' failed with http status code '%statusCode;'. Error message from Azure ML Service: '%externalMessage;'.`

- **Ursache:** Die im verknüpften Dienst für Azure ML Service angegebenen Anmeldeinformationen sind ungültig oder verfügen über keine Berechtigungen für diesen Vorgang.

- **Empfehlung**:  Überprüfen Sie, ob die Anmeldeinformationen im verknüpften Dienst gültig sind und Berechtigungen zum Zugriff auf Azure ML Service vorhanden sind.


### <a name="error-code--4123"></a>Fehlercode:  4123

- **Meldung**: `Request sent to Azure ML Service for operation '%operation;' failed with http status code '%statusCode;'. Error message from Azure ML Service: '%externalMessage;'.`

- **Ursache**: `Properties of the activity such as pipelineParamters are invalid for the Azure ML pipeline.`

- **Empfehlung**:  Überprüfen Sie den Wert der Aktivitätseigenschaften, um die erwartete Nutzlast der im verknüpften Dienst angegebenen veröffentlichten Azure ML-Pipeline abzugleichen.


### <a name="error-code--4124"></a>Fehlercode:  4124

- **Meldung**: `Request sent to Azure ML Service for operation '%operation;' failed with http status code '%statusCode;'. Error message from Azure ML Service: '%externalMessage;'.`

- **Ursache:** Der veröffentlichte Azure ML-Pipeline-Endpunkt ist nicht vorhanden.

- **Empfehlung**:  Überprüfen Sie, ob der im verknüpften Dienst angegebene veröffentlichte Azure ML-Pipeline-Endpunkt in Azure ML Service vorhanden ist.


### <a name="error-code--4125"></a>Fehlercode:  4125

- **Meldung**: `Request sent to Azure ML Service for operation '%operation;' failed with http status code '%statusCode;'. Error message from Azure ML Service: '%externalMessage;'.`

- **Ursache:** Serverfehler in Azure ML Service.

- **Empfehlung**:  Versuchen Sie es später noch mal. Wenn das Problem weiterhin besteht, wenden Sie sich an das Azure ML Service-Team.


### <a name="error-code--4126"></a>Fehlercode:  4126

- **Meldung**: `Azure ML pipeline run failed with status: '%amlPipelineRunStatus;'. Azure ML pipeline run Id: '%amlPipelineRunId;'. Please check in Azure ML Service for more error logs.`

- **Ursache:** Fehler beim Ausführen der Azure ML-Pipeline

- **Empfehlung**:  Überprüfen Sie in Azure ML Service, ob weitere Fehlerprotokolle vorhanden sind, und korrigieren Sie die ML-Pipeline.



## <a name="common"></a>Common

### <a name="error-code--2103"></a>Fehlercode:  2103

- **Meldung**: `Please provide value for the required property '%propertyName;'.`

- **Ursache:** Der Wert für die Eigenschaft wurde nicht angegeben, er ist jedoch für das Szenario erforderlich.

- **Empfehlung**:  Geben Sie den Wert aus der Meldung an, und versuchen Sie es noch mal.


### <a name="error-code--2104"></a>Fehlercode:  2104

- **Meldung**: `The type of the property '%propertyName;' is incorrect.`

- **Ursache:** Die angegebene Eigenschaft verfügt nicht über den erwarteten Typ.

- **Empfehlung**:  Korrigieren Sie den Typ der Eigenschaft, und versuchen Sie es noch mal.


### <a name="error-code--2105"></a>Fehlercode:  2105

- **Meldung**: `An invalid json is provided for property '%propertyName;'. Encountered an error while trying to parse: '%message;'.`

- **Ursache:** Der Wert für die Eigenschaft ist ungültig oder weist nicht das erwartete Format auf.

- **Empfehlung**:  Sehen Sie in der Dokumentation zur Eigenschaft nach,und stellen Sie sicher, dass der angegebene Wert dem erwarteten Format und Typ entspricht.


### <a name="error-code--2106"></a>Fehlercode:  2106

- **Meldung**: `The storage connection string is invalid. %errorMessage;`

- **Ursache:** Die Verbindungszeichenfolge für den Speicher ist ungültig oder hat ein falsches Format.

- **Empfehlung**:  Navigieren Sie im Azure-Portal zu Ihrem Speicher, kopieren Sie die Verbindungszeichenfolge, und fügen Sie sie in den verknüpften Dienst ein. Wiederholen Sie anschließend den Vorgang.


### <a name="error-code--2108"></a>Fehlercode:  2108

- **Meldung**: `Error calling the endpoint '%url;'. Response status code: '%code;'`

- **Ursache:** Die Anforderung war aufgrund eines Problems (Netzwerkkonnektivität, DNS-Fehler, Überprüfung des Serverzertifikats, Timeout oder Ähnliches) nicht erfolgreich.

- **Empfehlung**:  Verwenden Sie Fiddler/Postman, um die Anforderung zu überprüfen.


### <a name="error-code--2110"></a>Fehlercode:  2110

- **Meldung**: `The linked service type '%linkedServiceType;' is not supported for '%executorType;' activities.`

- **Ursache:** Der in der Aktivität angegebene verknüpfte Dienst war falsch.

- **Empfehlung**:  Stellen Sie sicher, dass als Typ des verknüpften Diensts einer der unterstützten Typen für die Aktivität verwendet wird. Für HDI-Aktivitäten kann als Typ des verknüpften Diensts beispielsweise „HDInsight“ oder „HDInsightOnDemand“ verwendet werden.


### <a name="error-code--2111"></a>Fehlercode:  2111

- **Meldung**: `The type of the property '%propertyName;' is incorrect. The expected type is %expectedType;.`

- **Ursache:** Die angegebene Eigenschaft verfügt nicht über den erwarteten Typ.

- **Empfehlung**:  Korrigieren Sie den Typ der Eigenschaft, und versuchen Sie es noch mal.


### <a name="error-code--2112"></a>Fehlercode:  2112

- **Meldung**: `The cloud type is unsupported or could not be determined for storage from the EndpointSuffix '%endpointSuffix;'.`

- **Ursache:** Der Cloudtyp wird nicht unterstützt oder konnte für den Speicher nicht über EndpointSuffix bestimmt werden.

- **Empfehlung**:  Verwenden Sie einen Speicher in einer anderen Cloud, und versuchen Sie es noch mal.


### <a name="error-code--2128"></a>Fehlercode:  2128

- **Meldung**: `No response from the endpoint. Possible causes: network connectivity, DNS failure, server certificate validation or timeout.`

- **Ursache:** Netzwerkkonnektivität, DNS-Fehler, Überprüfung des Serverzertifikats oder Timeout

- **Empfehlung**:  Überprüfen Sie, ob der Endpunkt, den Sie erreichen möchten, auf Anforderungen reagiert. Dazu können Sie Tools wie Fiddler oder Postman verwenden.



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

### <a name="error-code--200"></a>Fehlercode:  200

- **Meldung**: `Unexpected error happened: '%error;'.`

- **Ursache:** Ein internes Dienstproblem ist aufgetreten.

- **Empfehlung**:  Wenn Sie weitere Unterstützung benötigen, wenden Sie sich an den ADF-Support.


### <a name="error-code--201"></a>Fehlercode:  201

- **Meldung**: `JobType %jobType; is not found.`

- **Ursache:** Es ist ein neuer Auftragstyp vorhanden, der von ADF nicht unterstützt wird.

- **Empfehlung**:  Wenn Sie weitere Unterstützung benötigen, wenden Sie sich an das ADF-Supportteam.


### <a name="error-code--202"></a>Fehlercode:  202

- **Meldung**: `Failed to create on demand HDI cluster. Cluster name or linked service name: '%clusterName;', error: '%message;'`

- **Ursache:** Die Fehlermeldung sollte Einzelheiten zum Fehler enthalten.

- **Empfehlung**:  Sie sollten das Problem mithilfe der Fehlermeldung beheben können. Enthält sie nicht genügend Informationen, wenden Sie sich an den ADF-Support, um weitere Unterstützung zu erhalten.


### <a name="error-code--203"></a>Fehlercode:  203

- **Meldung**: `Failed to delete on demand HDI cluster. Cluster name or linked service name: '%clusterName;', error: '%message;'`

- **Ursache:** Die Fehlermeldung sollte Einzelheiten zum Fehler enthalten.

- **Empfehlung**:  Sie sollten das Problem mithilfe der Fehlermeldung beheben können. Enthält sie nicht genügend Informationen, wenden Sie sich an den ADF-Support, um weitere Unterstützung zu erhalten.


### <a name="error-code--204"></a>Fehlercode:  204

- **Meldung**: `The resumption token is missing for runId '%runId;'.`

- **Ursache:** Ein internes Dienstproblem ist aufgetreten.

- **Empfehlung**:  Wenn Sie weitere Unterstützung benötigen, wenden Sie sich an den ADF-Support.


### <a name="error-code--205"></a>Fehlercode:  205

- **Meldung**: `Failed to prepare cluster for LinkedService '%linkedServiceName;', the current resource status is '%status;'.`

- **Ursache:** Bei der Erstellung des HDI-On-Demand-Clusters ist ein Fehler aufgetreten.

- **Empfehlung**:  Wenn Sie weitere Unterstützung benötigen, wenden Sie sich an den ADF-Support.


### <a name="error-code--206"></a>Fehlercode:  206

- **Meldung**: `The batch ID for Spark job is invalid. Please retry your job, and if the problem persists, contact the ADF support for further assistance.`

- **Ursache:** Ein internes Problem mit dem Dienst hat diesen Fehler verursacht.

- **Empfehlung**:  Dies ist unter Umständen ein vorübergehendes Problem. Führen Sie den Auftrag erneut aus. Wenn das Problem weiterhin besteht, wenden Sie sich an den ADF-Support, um weitere Unterstützung zu erhalten.


### <a name="error-code--207"></a>Fehlercode:  207

- **Meldung**: `Could not determine the region from the provided storage account. Please try using another primary storage account for the on demand HDI or contact ADF support team and provide the activity run ID.`

- **Ursache:** Beim Ermitteln der Region aus dem primären Speicherkonto ist ein interner Fehler aufgetreten.

- **Empfehlung**:  Versuchen Sie es mit einem anderen Speicher. Ist dies keine akzeptable Lösung, wenden Sie sich an das ADF-Supportteam, um weitere Unterstützung zu erhalten.


### <a name="error-code--208"></a>Fehlercode:  208

- **Meldung**: `Service Principal or the MSI authenticator are not instantiated. Please consider providing a Service Principal in the HDI on demand linked service which has permissions to create an HDInsight cluster in the provided subscription and try again. In case if this is not an acceptable solution, contact ADF support team for further assistance.`

- **Ursache:** Beim Lesen des Dienstprinzipals bzw. beim Instanziieren der MSI-Authentifizierung ist ein interner Fehler aufgetreten.

- **Empfehlung**:  Ziehen Sie die Angabe eines Dienstprinzipals in Betracht, der über Berechtigungen zum Erstellen eines HDInsight-Clusters im angegebenen Abonnement verfügt, und wiederholen Sie den Vorgang. Ist dies keine akzeptable Lösung, wenden Sie sich an das ADF-Supportteam, um weitere Unterstützung zu erhalten.


### <a name="error-code--2300"></a>Fehlercode:  2.300

- **Meldung**: `Failed to submit the job '%jobId;' to the cluster '%cluster;'. Error: %errorMessage;.`

<br>

- **Ursache:** Lautet die Fehlermeldung etwa „Der Remotename konnte nicht aufgelöst werden.“, ist der angegebene Cluster-URI unter Umständen ungültig.


- **Empfehlung**: Stellen Sie sicher, dass der Cluster nicht gelöscht wurde und der angegebene URI richtig ist. Wenn Sie den URI in einem Browser öffnen, sollte die Ambari-Benutzeroberfläche angezeigt werden. Wenn sich der Cluster in einem virtuellen Netzwerk befindet, sollte der URI der private URI sein. Verwenden Sie zum Öffnen eine VM, die Teil desselben virtuellen Netzwerks ist. Weitere Informationen finden Sie [hier](https://docs.microsoft.com/azure/hdinsight/hdinsight-plan-virtual-network-deployment#directly-connect-to-apache-hadoop-services).
                  

<br>

- **Ursache:** Lautet die Fehlermeldung etwa „Eine Aufgabe wurde abgebrochen.“, bedeutet dies, dass bei der Auftragsübermittlung ein Timeout aufgetreten ist.

- **Empfehlung**:  Das Problem kann entweder ein allgemeines HDInsight-Verbindungsproblem oder ein Netzwerkverbindungsproblem sein. Vergewissern Sie sich zunächst, ob die HDInsight Ambari-Benutzeroberfläche in jedem Browser verfügbar ist. Bestätigen Sie, dass Ihre Anmeldeinformationen noch gültig sind. Wenn Sie eine selbstgehostete Integration Runtime (IR) verwenden, sollten Sie dies unbedingt auf der VM oder dem Computer überprüfen, auf der bzw. dem die selbstgehostete IR installiert ist. Übermitteln Sie den Auftrag dann erneut über Data Factory. Wenn weiterhin Fehler auftreten, wenden Sie sich an das Data Factory-Team.

<br>

- **Ursache:** Wenn die Fehlermeldung etwa „User admin is locked out in Ambari“ (Benutzeradministrator ist in Ambari gesperrt.) oder „Nicht autorisiert: Der Ambari-Benutzername oder das Kennwort ist falsch.“ lautet, bedeutet dies, dass die Anmeldeinformationen für HDInsight falsch oder abgelaufen sind.

- **Empfehlung**:  Korrigieren Sie die Anmeldeinformation, und stellen Sie den verknüpften Dienst erneut bereit. Stellen Sie zunächst sicher, dass die Anmeldeinformationen in HDInsight funktionieren. Öffnen Sie dazu den Cluster-URI in einem Browser, und versuchen Sie, sich anzumelden. Wenn die Anmeldeinformationen nicht funktionieren, können Sie sie im Azure-Portal zurücksetzen.

<br>

- **Ursache:** Lautet die Fehlermeldung etwa „502 – Webserver hat als Gateway oder Proxyserver eine ungültige Antwort erhalten.“, wird dieser Fehler vom HDInsight-Dienst zurückgegeben.


- **Empfehlung**: Sehen Sie sich die Dokumentation zur Azure HDInsight-Problembehandlung an, etwa https://hdinsight.github.io/ambari/ambari-ui-502-error.html, https://hdinsight.github.io/spark/spark-thriftserver-errors.html und https://docs.microsoft.com/azure/application-gateway/application-gateway-troubleshooting-502.
                  

<br>

- **Ursache:** Wenn die Fehlermeldung etwa „Unable to service the submit job request as templeton service is busy with too many submit job requests“ (Die Anforderung für den Übermittlungsauftrag kann nicht verarbeitet werden, da der Templeton-Dienst mit zu vielen Übermittlungsauftragsanforderungen ausgelastet ist.) oder „Warteschlange „root.joblauncher“ enthält bereits 500 Anwendungen, Übermittlung der Anwendung kann nicht angenommen werden.“ lautet, bedeutet dies, dass zu viele Aufträge gleichzeitig an HDInsight übermittelt werden.

- **Empfehlung**:  Begrenzen Sie die Anzahl von Aufträgen, die gleichzeitig an HDInsight übermittelt werden. Wenn die Aufträge über die gleiche Aktivität übermittelt werden, helfen Ihnen die entsprechenden Informationen in der Data Factory-Aktivität für Parallelität weiter. Ändern Sie die Trigger so, dass die gleichzeitigen Pipelineausführungen über die Zeit verteilt werden. Informationen zur Anpassung von „templeton.parallellism.job.submit“, wie in der Fehlermeldung empfohlen, finden Sie in der HDInsight-Dokumentation.


### <a name="error-code--2301"></a>Fehlercode:  2301

- **Meldung**: `Could not get the status of the application '%physicalJobId;' from the HDInsight service. Received the following error: %message;. Please refer to HDInsight troubleshooting documentation or contact their support for further assistance.`

- **Ursache:** Beim HDInsight-Cluster oder -Dienst treten Probleme auf.


- **Empfehlung**: Dieser Fehler tritt auf, wenn ADF beim Abrufen des Status des ausgeführten Auftrags keine Antwort vom HDInsight-Cluster erhält. Der Fehler kann durch Probleme mit dem Cluster selbst verursacht werden, oder der HDInsight-Dienst ist ausgefallen. Informationen finden Sie in der Dokumentation zur HDInsight-Problembehandlung unter https://docs.microsoft.com/azure/hdinsight/hdinsight-troubleshoot-guide, oder wenden Sie sich an den Support, um weitere Unterstützung zu erhalten.
                


### <a name="error-code--2302"></a>Fehlercode:  2.302

- **Meldung**: `Hadoop job failed with exit code '%exitCode;'. See '%logPath;/stderr' for more details. Alternatively, open the Ambari UI on the HDI cluster and find the logs for the job '%jobId;'. Contact HDInsight team for further support.`

- **Ursache:** Der Auftrag wurde an den HDI-Cluster übermittelt und ist dort fehlgeschlagen.

- **Empfehlung**:  Klicken Sie auf den Link für die Yarn-Protokolle in der Ausgabe der Aktivitätsausführung, und suchen Sie nach den Fehlern in der HDI-Ausgabe. Wenden Sie sich bei Bedarf an das HDInsight-Team.


### <a name="error-code--2303"></a>Fehlercode:  2303

- **Meldung**: `Hadoop job failed with transient exit code '%exitCode;'. See '%logPath;/stderr' for more details. Alternatively, open the Ambari UI on the HDI cluster and find the logs for the job '%jobId;'. Try again or contact HDInsight team for further support.`

- **Ursache:** Der Auftrag wurde an den HDI-Cluster übermittelt und ist dort fehlgeschlagen.

- **Empfehlung**:  Klicken Sie auf den Link für die Yarn-Protokolle in der Ausgabe der Aktivitätsausführung, und suchen Sie nach den Fehlern in der HDI-Ausgabe. Wiederholen Sie den Vorgang, oder wenden Sie sich bei Bedarf an das HDInsight-Team.


### <a name="error-code--2304"></a>Fehlercode:  2304

- **Meldung**: `MSI authentication is not supported on storages for HDI activities.`

- **Ursache:** Die im verknüpften HDI-Dienst oder in der HDI-Aktivität verwendeten verknüpften Speicherdienste sind mit MSI-Authentifizierung konfiguriert. Dies wird nicht unterstützt.

- **Empfehlung**:  Geben Sie vollständige Verbindungszeichenfolgen für Speicherkonten an, die im verknüpften HDI-Dienst oder in der HDI-Aktivität verwendet werden.


### <a name="error-code--2305"></a>Fehlercode:  2305

- **Meldung**: `Failed to initialize the HDInsight client for the cluster '%cluster;'. Error: '%message;'`

- **Ursache:** Die Verbindungsinformationen für den HDI-Cluster sind falsch, der angegebene Benutzer verfügt nicht über die Berechtigungen zum Ausführen der erforderlichen Aktion, oder beim HDInsight-Dienst sind Probleme beim Reagieren auf Anforderungen von ADF aufgetreten.

- **Empfehlung**:  Achten Sie darauf, richtige Benutzerinformationen anzugeben. Vergewissern Sie sich darüber hinaus, dass die Ambari-Benutzeroberfläche für den HDI-Cluster in einem Browser auf der VM, auf der die IR installiert ist (im Falle einer selbstgehosteten IR), oder auf einem beliebigen Computer (im Falle von Azure IR) öffnen können.


### <a name="error-code--2306"></a>Fehlercode:  2306

- **Meldung**: `An invalid json is provided for script action '%scriptActionName;'. Error: '%message;'`

- **Ursache:** Der für die Skriptaktion angegebene JSON-Code ist ungültig.


- **Empfehlung**: Sie sollten das Problem mithilfe der Fehlermeldung identifizieren können. Korrigieren Sie die JSON-Konfiguration, und versuchen Sie es noch mal. Unter https://docs.microsoft.com/azure/data-factory/compute-linked-services#azure-hdinsight-on-demand-linked-service finden Sie weitere Informationen.
                


### <a name="error-code--2310"></a>Fehlercode:  2310

- **Meldung**: `Failed to submit Spark job. Error: '%message;'`

- **Ursache:** ADF hat versucht, mithilfe der Livy-API einen Batch auf einem Spark-Cluster zu erstellen (Livy/Batch), es ist jedoch ein Fehler aufgetreten.

- **Empfehlung**:  Beheben Sie das Problem anhand der Informationen in der Fehlermeldung. Sind nicht genügend Informationen vorhanden, um das Problem zu beheben, wenden Sie sich an das HDI-Team, und geben Sie die Batch-ID und die Auftrags-ID an. Diese finden Sie auf der Seite zur ADF-Überwachung in der Ausgabe der Aktivitätsausführung.


### <a name="error-code--2312"></a>Fehlercode:  2312

- **Meldung**: `Spark job failed, batch id:%batchId;. Please follow the links in the activity run Output from ADF Monitoring page to troubleshoot the run on HDInsight Spark cluster. Please contact HDInsight support team for further assistance.`

- **Ursache:** Der Auftrag auf dem HDInsight Spark-Cluster war nicht erfolgreich.

- **Empfehlung**:  Verwenden Sie die Links in der Ausgabe der Aktivitätsausführung auf der Seite zur ADF-Überwachung, um Probleme mit der Ausführung auf dem HDInsight Spark-Cluster zu beheben. Wenn Sie weitere Unterstützung benötigen, wenden Sie sich an das HDInsight-Supportteam.


### <a name="error-code--2313"></a>Fehlercode:  2313

- **Meldung**: `The batch with ID '%batchId;' was not found on Spark cluster. Open the Spark History UI and try to find it there. Contact HDInsight support for further assistance.`

- **Ursache:** Der Batch wurde auf dem HDInsight Spark-Cluster gelöscht.

- **Empfehlung**:  Beheben Sie Probleme mit Batches auf dem HDInsight Spark-Cluster. Wenden Sie sich an den HDInsight-Support, um weitere Unterstützung zu erhalten. 


### <a name="error-code--2328"></a>Fehlercode:  2328

- **Meldung**: `Failed to create the on demand HDI cluster. Cluster or linked service name: '%clusterName;', error: '%message;'`

- **Ursache**: `The error message should show the details of what went wrong.`

- **Empfehlung**:  Sie sollten das Problem mithilfe der Fehlermeldung beheben können.


### <a name="error-code--2329"></a>Fehlercode:  2329

- **Meldung**: `Failed to delete the on demand HDI cluster. Cluster or linked service name: '%clusterName;', error: '%message;'`

- **Ursache:** Die Fehlermeldung sollte Einzelheiten zum Fehler enthalten.

- **Empfehlung**:  Sie sollten das Problem mithilfe der Fehlermeldung beheben können.


### <a name="error-code--2331"></a>Fehlercode:  2331

- **Meldung**: `The file path should not be null or empty.`

- **Ursache:** Der angegebene Dateipfad ist leer.

- **Empfehlung**:  Geben Sie einen Pfad für eine vorhandene Datei an.


### <a name="error-code--2340"></a>Fehlercode:  2340

- **Meldung**: `HDInsightOnDemand linked service does not support execution via SelfHosted IR. Your IR name is '%IRName;'. Please select an Azure IR instead.`

- **Ursache:** Der verknüpfte HDInsightOnDemand-Dienst unterstützt nicht die Ausführung über eine selbstgehostete IR.

- **Empfehlung**:  Wählen Sie eine Azure IR-Instanz aus, und versuchen Sie es noch mal.


### <a name="error-code--2341"></a>Fehlercode:  2341

- **Meldung**: `HDInsight cluster URL '%clusterUrl;' is incorrect, it must be in URI format and the scheme must be 'https'.`

- **Ursache:** Die angegebene URL hat nicht das richtige Format.

- **Empfehlung**:  Korrigieren Sie die Cluster-URL, und versuchen Sie es noch mal.


### <a name="error-code--2342"></a>Fehlercode:  2342

- **Meldung**: `Failed to connect to HDInsight cluster: '%errorMessage;'.`

- **Ursache:** Die angegebenen Anmeldeinformationen für den Cluster sind falsch, es ist ein Problem mit der Netzwerkkonfiguration oder -verbindung aufgetreten, oder die IR kann keine Verbindung mit dem Cluster herstellen.

- **Empfehlung**:  
      1. Überprüfen Sie, ob die Anmeldeinformationen richtig sind, indem Sie die Ambari-Benutzeroberfläche des HDInsight-Clusters in einem Browser öffnen.
      2. Wenn sich der Cluster in einem VNET befindet und eine selbstgehostete IR verwendet wird, sollte die HDI-URL die private URL in VNETs sein. Das bedeutet, dass sie nach dem Clusternamen „-int“ enthält. „https://mycluster.azurehdinsight.net/“ muss beispielsweise in „https://mycluster-int.azurehdinsight.net/“ geändert werden.
      2. Wenn sich der Cluster im VNET befindet, eine selbstgehostete IR verwendet wird und die private URL verwendet wurde, die Verbindung aber dennoch nicht erfolgreich hergestellt wurde, konnte die VM mit der installierten IR keine Verbindung mit dem HDI-Cluster herstellen. Stellen Sie eine Verbindung mit der VM her, auf der die IR installiert ist, und öffnen Sie die Ambari-Benutzeroberfläche in einem Browser. Verwenden Sie die private URL für den Cluster. Diese Verbindung sollte im Browser funktionieren. Ist dies nicht der Fall, wenden Sie sich an das HDInsight-Supportteam, um weitere Unterstützung zu erhalten.
      3. Wird keine selbstgehostete IR verwendet, sollte der HDI-Cluster öffentlich zugänglich sein. Öffnen Sie die Ambari-Benutzeroberfläche in einem Browser, und stellen Sie sicher, dass Sie tatsächlich angezeigt wird. Sollten Probleme mit dem Cluster oder mit den Diensten auf dem Cluster auftreten, wenden sie sich an das HDInsight-Supportteam.
      Im Allgemeinen muss die HDI-Cluster-URL, die im verknüpften ADF-Dienst verwendet wird, für die ADF IR (selbstgehostet oder Azure IR) zugänglich sein, damit die Testverbindung und Ausführungen erfolgreich sind. Dies lässt sich problemlos überprüfen, indem Sie diese URL entweder auf einer VM oder auf einem beliebigen öffentlichen Computer in einem Browser öffnen.
    


### <a name="error-code--2343"></a>Fehlercode:  2343

- **Meldung**: `User name and password cannot be null or empty to connect to the HDInsight cluster.`

- **Ursache:** Der Benutzername oder das Kennwort ist leer.

- **Empfehlung**:  Geben Sie korrekte Anmeldeinformationen für die Verbindung mit HDI an, und versuchen Sie es noch mal.


### <a name="error-code--2345"></a>Fehlercode:  2345

- **Meldung**: `Failed to read the content of the hive script. Error: '%message;'`

- **Ursache:** Die Skriptdatei ist nicht vorhanden, oder von ADF konnte keine Verbindung mit dem Speicherort des Skripts hergestellt werden.

- **Empfehlung**:  Vergewissern Sie sich, dass das Skript vorhanden ist und dass der zugehörige verknüpfte Dienst über die richtigen Anmeldeinformationen für die Verbindung verfügt.


### <a name="error-code--2346"></a>Fehlercode:  2346

- **Meldung**: `Failed to create ODBC connection to the HDI cluster with error message '%message;'.`

- **Ursache:** ADF hat versucht, eine ODBC-Verbindung mit dem HDI-Cluster herzustellen, und es ist ein Fehler aufgetreten.

- **Empfehlung**:  Die Fehlermeldung und der Fehlercode sollten Ihnen bei der Problembehandlung der ODBC-Verbindungsfehler helfen. Reichen diese Informationen für die Behebung des Problems nicht aus, wenden Sie sich an das Azure HDInsight-Team, um Unterstützung zu erhalten.


### <a name="error-code--2347"></a>Fehlercode:  2347

- **Meldung**: `Hive execution through ODBC failed with error message '%message;'.`

- **Ursache:** ADF hat das Hive-Skript über eine ODBC-Verbindung zur Ausführung an den HDI-Cluster gesendet, und für das Skript ist auf dem HDI-Cluster ein Fehler aufgetreten.

- **Empfehlung**:  Bei der Ausführung des Hive-Skripts auf dem HDI-Cluster ist ein Fehler aufgetreten. Die Fehlermeldung und der Fehlercode sollten bei der Problembehandlung hilfreich sein. Reichen diese Informationen für die Behebung des Problems nicht aus, wenden Sie sich an das Azure HDInsight-Team, um Unterstützung zu erhalten.


### <a name="error-code--2348"></a>Fehlercode:  2348

- **Meldung**: `The main storage has not been initialized. Please check the properties of the storage linked service in the HDI linked service.`

- **Ursache:** Die Eigenschaften des verknüpften Speicherdiensts sind nicht richtig festgelegt.

- **Empfehlung**:  Im verknüpften Hauptspeicherdienst für HDI-Aktivitäten werden nur vollständige Verbindungszeichenfolgen unterstützt. Verwenden Sie keinesfalls MSI-Authentifizierung oder -Anwendungen.


### <a name="error-code--2350"></a>Fehlercode:  2350

- **Meldung**: `Failed to prepare the files for the run '%jobId;'. HDI cluster: '%cluster;', Error: '%errorMessage;'`

- **Ursache:** Die Anmeldeinformationen, die zum Herstellen einer Verbindung mit dem Speicher angegeben wurden, in dem sich die Dateien befinden sollten, sind falsch, oder die Dateien sind dort nicht vorhanden.

- **Empfehlung**:  Dieser Fehler tritt auf, wenn von ADF Vorbereitungsschritte für HDI-Aktivitäten ausgeführt werden. ADF versucht, Dateien in den Hauptspeicher zu kopieren, bevor der Auftrag an HDI übermittelt wird. Vergewissern Sie sich, dass die Dateien am angegebenen Speicherort vorhanden sind und dass die Speicherverbindung richtig ist. ADF-HDI-Aktivitäten unterstützen MSI-Authentifizierung für Speicherkonten nicht, die mit HDI-Aktivitäten im Zusammenhang stehen. Stellen Sie daher sicher, dass diese verknüpften Dienste über vollständige Schlüssel verfügen oder Azure Key Vault verwenden.


### <a name="error-code--2351"></a>Fehlercode:  2351

- **Meldung**: `Could not open the file '%filePath;' in container/fileSystem '%container;'.`

- **Ursache:** Die Datei ist am angegebenen Pfad nicht vorhanden.

- **Empfehlung**:  Überprüfen Sie, ob die Datei tatsächlich vorhanden ist und ob der verknüpfte Dienst mit Verbindungsinformationen, die auf diese Datei verweisen, die richtigen Anmeldeinformationen enthält.


### <a name="error-code--2352"></a>Fehlercode:  2352

- **Meldung**: `The file storage has not been initialized. Please check the properties of the file storage linked service in the HDI activity.`

- **Ursache:** Die Eigenschaften des verknüpften Dateispeicherdiensts sind nicht richtig festgelegt.

- **Empfehlung**:  Stellen Sie sicher, dass die Eigenschaften des verknüpften Dateispeicherdiensts ordnungsgemäß konfiguriert sind.


### <a name="error-code--2353"></a>Fehlercode:  2353

- **Meldung**: `The script storage has not been initialized. Please check the properties of the script storage linked service in the HDI activity.`

- **Ursache:** Die Eigenschaften des verknüpften Skriptspeicherdiensts sind nicht richtig festgelegt.

- **Empfehlung**:  Stellen Sie sicher, dass die Eigenschaften des verknüpften Skriptspeicherdiensts ordnungsgemäß konfiguriert sind.


### <a name="error-code--2354"></a>Fehlercode:  2354

- **Meldung**: `The storage linked service type '%linkedServiceType;' is not supported for '%executorType;' activities for property '%linkedServicePropertyName;'.`

- **Ursache:** Der Typ des verknüpften Speicherdiensts wird von der Aktivität nicht unterstützt.

- **Empfehlung**:  Stellen Sie sicher, dass für den ausgewählten verknüpften Dienst einer der unterstützten Typen für die Aktivität verwendet wird. HDI-Aktivitäten unterstützen die verknüpften Dienste „AzureBlobStorage“ und „AzureBlobFSStorage“.


### <a name="error-code--2355"></a>Fehlercode:  2355

- **Meldung**: `The '%value' provided for commandEnvironment is incorrect. The expected value should be an array of strings where each string has the format CmdEnvVarName=CmdEnvVarValue.`

- **Ursache:** Der angegebene Wert für „commandEnvironment“ ist falsch.

- **Empfehlung**:  
      Stellen Sie sicher, dass der angegebene Wert etwa wie folgt aussieht: \"commandEnvironment\": [ \"variableName=variableValue\" ]. Jede Variable darf in der Liste nur einmal aufgeführt sein.
    


### <a name="error-code--2356"></a>Fehlercode:  2356

- **Meldung**: `The commandEnvironment already contains a variable named '%variableName;'.`

- **Ursache:** Die Variable wurde in „commandEnvironment“ zweimal angegeben.

- **Empfehlung**:  
      Stellen Sie sicher, dass der angegebene Wert etwa wie folgt aussieht: \"commandEnvironment\": [ \"variableName=variableValue\" ]. Jede Variable darf in der Liste nur einmal aufgeführt sein.
    


### <a name="error-code--2357"></a>Fehlercode:  2357

- **Meldung**: `The certificate or password is wrong for ADLS Gen 1 storage.`

- **Ursache:** Die bereitgestellten Anmeldeinformationen sind falsch.

- **Empfehlung**:  Überprüfen Sie die Verbindungsinformationen im verknüpften ADLS Gen 1-Dienst, und stellen Sie sicher, dass die Testverbindung erfolgreich ist.


### <a name="error-code--2358"></a>Fehlercode:  2358

- **Meldung**: `The value '%value;' for the required property 'TimeToLive' in the on demand HDInsight linked service '%linkedServiceName;' has invalid format. It should be a timespan between '00:05:00' and '24:00:00'.`

- **Ursache:** Der angegebene Wert für die erforderliche Eigenschaft „TimeToLive“ hat ein ungültiges Format. 

- **Empfehlung**:  Aktualisieren Sie den Wert, sodass er im vorgeschlagenen Bereich liegt, und wiederholen Sie den Vorgang.


### <a name="error-code--2359"></a>Fehlercode:  2359

- **Meldung**: `The value '%value;' for the property 'roles' is invalid. Expected types are 'zookeeper', 'headnode', and 'workernode'.`

- **Ursache:** Der angegebene Wert für die Eigenschaft „roles“ ist ungültig.

- **Empfehlung**:  Ersetzen Sie den Wert durch einen der vorgeschlagenen Werte, und wiederholen Sie den Vorgang.


### <a name="error-code--2360"></a>Fehlercode:  2360

- **Meldung**: `The connection string in HCatalogLinkedService is invalid. Encountered an error while trying to parse: '%message;'.`

- **Ursache:** Die angegebene Verbindungszeichenfolge für „HCatalogLinkedService“ ist ungültig.

- **Empfehlung**:  Ersetzen Sie den Wert durch eine korrekte Azure SQL-Verbindungszeichenfolge, und versuchen Sie es noch mal.


### <a name="error-code--2361"></a>Fehlercode:  2361

- **Meldung**: `Failed to create on demand HDI cluster. Cluster name is '%clusterName;'.`

- **Ursache:** Die Clustererstellung war nicht erfolgreich, und vom HDInsight-Dienst wurde kein Fehler an ADF ausgegeben.

- **Empfehlung**:  Öffnen Sie das Azure-Portal, suchen Sie die HDI-Ressource mit dem angegebenen Namen, und überprüfen Sie den Bereitstellungsstatus. Wenn Sie weitere Unterstützung benötigen, wenden Sie sich an das HDInsight-Supportteam.


### <a name="error-code--2362"></a>Fehlercode:  2362

- **Meldung**: `Only Azure Blob storage accounts are supported as additional storages for HDInsight on demand linked service.`

- **Ursache:** Der bereitgestellte zusätzliche Speicher war kein Azure-Blobspeicher.

- **Empfehlung**:  Geben Sie ein Azure-Blobspeicherkonto als zusätzlichen Speicher für den bedarfsgesteuerten verknüpften HDInsight-Dienst an.



## <a name="web-activity"></a>Webaktivität

### <a name="error-code--2128"></a>Fehlercode:  2128

- **Meldung**: `No response from the endpoint. Possible causes: network connectivity, DNS failure, server certificate validation or timeout.`

- **Ursache:** Netzwerkkonnektivität, DNS-Fehler, Überprüfung des Serverzertifikats oder Timeout

- **Empfehlung**:  Überprüfen Sie, ob der Endpunkt, den Sie erreichen möchten, auf Anforderungen reagiert. Dazu können Sie Tools wie Fiddler oder Postman verwenden.


### <a name="error-code--2108"></a>Fehlercode:  2108

- **Meldung**: `Error calling the endpoint '%url;'. Response status code: '%code;'`

- **Ursache:** Die Anforderung war aufgrund eines Problems (Netzwerkkonnektivität, DNS-Fehler, Überprüfung des Serverzertifikats, Timeout oder Ähnliches) nicht erfolgreich.

- **Empfehlung**:  Verwenden Sie Fiddler/Postman, um die Anforderung zu überprüfen.
<br>


#### <a name="more-details"></a>Weitere Informationen
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


            
