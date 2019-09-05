---
title: Verzweigungen in Azure Data Factory-Pipeline | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie den Datenfluss in Azure Data Factory, durch die Verkettung und Verzweigung von Aktivitäten steuern.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: tutorial
ms.date: 02/20/2019
ms.openlocfilehash: 264d8e049cc7b714e00aaa77441cdc81a1e0a0c9
ms.sourcegitcommit: d200cd7f4de113291fbd57e573ada042a393e545
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2019
ms.locfileid: "70140733"
---
# <a name="branching-and-chaining-activities-in-a-data-factory-pipeline"></a>Verzweigen und Verketten von Aktivitäten in einer Data Factory-Pipeline

In diesem Tutorial erstellen Sie eine Data Factory-Pipeline, die einige Ablaufsteuerungsfunktionen vorstellt. Diese Pipeline führt eine einfache Kopieraktivität aus einem Container in Azure Blob Storage in einen anderen Container im selben Speicherkonto durch. War die Kopieraktivität erfolgreich, wollen Sie die Details zum erfolgreichen Kopiervorgang (z.B. die geschriebene Datenmenge) in einer Erfolgsmail senden. Schlägt die Kopieraktivität fehl, wollen Sie die Details zum Fehler beim Kopieren (z.B. die Fehlermeldung) in einer Fehler-E-Mail senden. In diesem Tutorial erfahren Sie, wie Sie Parameter übergeben.

Eine allgemeine Übersicht über das Szenario: ![Übersicht](media/tutorial-control-flow/overview.png)

In diesem Tutorial führen Sie die folgenden Schritte aus:

> [!div class="checklist"]
> * Erstellen einer Data Factory.
> * Erstellen eines verknüpften Azure Storage Diensts.
> * Erstellen eines Azure-Blobdatasets.
> * Erstellen einer Pipeline, die eine Kopieraktivität und eine Webaktivität enthält.
> * Senden von Aktivitätsausgaben an nachfolgende Aktivitäten.
> * Verwenden von Parameterübergabe und Systemvariablen.
> * Starten einer Pipelineausführung.
> * Überwachen der Pipeline- und Aktivitätsausführungen.

In diesem Tutorial wird „.NET SDK“ verwendet. Andere Mechanismen zur Interaktion mit Azure Data Factory, finden Sie unter "Schnellstart" im Inhaltsverzeichnis.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

* **Azure Storage-Konto**. Sie verwenden den Blob Storage als **Quelldatenspeicher**. Wenn Sie kein Azure Storage-Konto besitzen, finden Sie im Artikel [Erstellen eines Speicherkontos](../storage/common/storage-quickstart-create-account.md) Schritte zum Erstellen eines solchen Kontos.
* **Azure SQL-Datenbank**. Sie verwenden die Datenbank als **Senkendatenspeicher**. Wenn Sie keine Azure SQL-Datenbank besitzen, finden Sie die Anleitung zum Erstellen einer solchen im Artikel [Create an Azure SQL database (Erstellen einer Azure SQL-Datenbank)](../sql-database/sql-database-get-started-portal.md).
* **Visual Studio** 2013, 2015 oder 2017 Die Vorgehensweise in diesem Artikel verwendet Visual Studio 2017.
* **Laden und installieren Sie [Azure .NET SDK](https://azure.microsoft.com/downloads/)** .
* **Create an application in Azure Active Directory (Erstellen Sie eine Anwendung in Azure Active Directory)** , indem Sie [these instructions (diese Anweisungen)](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application) befolgen. Notieren Sie sich die folgenden Werte, die Sie in späteren Schritten verwenden: **Anwendungs-ID**, **Authentifizierungsschlüssel** und **Mandanten-ID**. Weisen Sie die Anwendung der Rolle „**Mitwirkender**“ zu, indem Sie die Anweisungen im gleichen Artikel befolgen.

### <a name="create-blob-table"></a>Erstellen Sie eine Blob-Tabelle

1. Starten Sie den Editor. Kopieren Sie den folgenden Text, und speichern Sie ihn als **input.txt**-Datei auf Ihrem Datenträger.

    ```
    John|Doe
    Jane|Doe
    ```

2. Verwenden Sie Tools wie [Azure Storage-Explorer](https://storageexplorer.com/), um den Container **adfv2branch** zu erstellen und die Datei **input.txt** in den Container hochzuladen.

## <a name="create-visual-studio-project"></a>Erstellen eines Visual Studio-Projekts

Erstellen Sie mithilfe von Visual Studio 2015/2017 eine C# .NET-Konsolenanwendung.

1. Starten Sie **Visual Studio**.
2. Klicken Sie auf **Datei**, zeigen Sie auf **Neu**, und klicken Sie auf **Projekt**. .NET-Version 4.5.2 oder höher ist erforderlich.
3. Wählen Sie in der Liste der Projekttypen auf der rechten Seite **Visual C#**  -> **Konsolen-App (.NET Framework)** aus.
4. Geben Sie **ADFv2BranchTutorial** für den Namen ein.
5. Klicken Sie auf **OK**, um das Projekt zu erstellen.

## <a name="install-nuget-packages"></a>Installieren von NuGet-Paketen

1. Klicken Sie auf **Tools** -> **NuGet-Paket-Manager** -> **Paket-Manager-Konsole**.
2. Führen Sie in der **Paket-Manager-Konsole** die folgenden Befehle zum Installieren von Paketen aus. Ausführliche Informationen finden Sie auf der Seite zum [Microsoft.Azure.Management.DataFactory-NuGet-Paket](https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactory/).

    ```powershell
    Install-Package Microsoft.Azure.Management.DataFactory
    Install-Package Microsoft.Azure.Management.ResourceManager
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
    ```

## <a name="create-a-data-factory-client"></a>Erstellen eines Data Factory-Clients

1. Öffnen Sie **Program.cs**, und fügen Sie die folgenden Anweisungen ein, um Verweise auf Namespaces hinzuzufügen.

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using Microsoft.Rest;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.DataFactory;
    using Microsoft.Azure.Management.DataFactory.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    ```

2. Fügen Sie diese statischen Variablen der **Program**-Klasse hinzu. Ersetzen Sie Platzhalter durch Ihre eigenen Werte. Eine Liste der Azure-Regionen, in denen Data Factory derzeit verfügbar ist, finden Sie, indem Sie die für Sie interessanten Regionen auf der folgenden Seite auswählen und dann **Analysen** erweitern, um **Data Factory** zu finden: [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/). Die von der Data Factory verwendeten Datenspeicher (Azure Storage, Azure SQL-Datenbank usw.) und Computedienste (HDInsight usw.) können sich in anderen Regionen befinden.

    ```csharp
        // Set variables
        static string tenantID = "<tenant ID>";
        static string applicationId = "<application ID>";
        static string authenticationKey = "<Authentication key for your application>";
        static string subscriptionId = "<Azure subscription ID>";
        static string resourceGroup = "<Azure resource group name>";

        static string region = "East US";
        static string dataFactoryName = "<Data factory name>";

        // Specify the source Azure Blob information
        static string storageAccount = "<Azure Storage account name>";
        static string storageKey = "<Azure Storage account key>";
        // confirm that you have the input.txt file placed in th input folder of the adfv2branch container. 
        static string inputBlobPath = "adfv2branch/input";
        static string inputBlobName = "input.txt";
        static string outputBlobPath = "adfv2branch/output";
        static string emailReceiver = "<specify email address of the receiver>";

        static string storageLinkedServiceName = "AzureStorageLinkedService";
        static string blobSourceDatasetName = "SourceStorageDataset";
        static string blobSinkDatasetName = "SinkStorageDataset";
        static string pipelineName = "Adfv2TutorialBranchCopy";

        static string copyBlobActivity = "CopyBlobtoBlob";
        static string sendFailEmailActivity = "SendFailEmailActivity";
        static string sendSuccessEmailActivity = "SendSuccessEmailActivity";
    
    ```

3. Fügen Sie der **Main**-Methode den folgenden Code hinzu, der eine Instanz der **DataFactoryManagementClient**-Klasse erstellt. Sie verwenden dieses Objekt, um eine Data Factory, einen verknüpften Dienst, Datasets und eine Pipeline zu erstellen. Sie verwenden dieses Objekt ebenfalls zum Überwachen der Ausführungsdetails der Pipeline.

    ```csharp
    // Authenticate and create a data factory management client
    var context = new AuthenticationContext("https://login.windows.net/" + tenantID);
    ClientCredential cc = new ClientCredential(applicationId, authenticationKey);
    AuthenticationResult result = context.AcquireTokenAsync("https://management.azure.com/", cc).Result;
    ServiceClientCredentials cred = new TokenCredentials(result.AccessToken);
    var client = new DataFactoryManagementClient(cred) { SubscriptionId = subscriptionId };
    ```

## <a name="create-a-data-factory"></a>Erstellen einer Data Factory

Erstellen Sie eine Funktion „CreateOrUpdateDataFactory“ in Ihrer „Program.cs“-Datei:

```csharp
static Factory CreateOrUpdateDataFactory(DataFactoryManagementClient client)
{
    Console.WriteLine("Creating data factory " + dataFactoryName + "...");
    Factory resource = new Factory
    {
        Location = region
    };
    Console.WriteLine(SafeJsonConvert.SerializeObject(resource, client.SerializationSettings));

    Factory response;
    {
        response = client.Factories.CreateOrUpdate(resourceGroup, dataFactoryName, resource);
    }

    while (client.Factories.Get(resourceGroup, dataFactoryName).ProvisioningState == "PendingCreation")
    {
        System.Threading.Thread.Sleep(1000);
    }
    return response;
}
```



Fügen Sie der **Main**-Methode den folgenden Code hinzu, der eine **Data Factory** erstellt. 

```csharp
Factory df = CreateOrUpdateDataFactory(client);
```

## <a name="create-an-azure-storage-linked-service"></a>Erstellen eines verknüpften Azure Storage-Diensts

Erstellen Sie eine Funktion „StorageLinkedServiceDefinition“ in Ihrer Datei „Program.cs“:

```csharp
static LinkedServiceResource StorageLinkedServiceDefinition(DataFactoryManagementClient client)
{
    Console.WriteLine("Creating linked service " + storageLinkedServiceName + "...");
    AzureStorageLinkedService storageLinkedService = new AzureStorageLinkedService
    {
        ConnectionString = new SecureString("DefaultEndpointsProtocol=https;AccountName=" + storageAccount + ";AccountKey=" + storageKey)
    };
    Console.WriteLine(SafeJsonConvert.SerializeObject(storageLinkedService, client.SerializationSettings));
    LinkedServiceResource linkedService = new LinkedServiceResource(storageLinkedService, name:storageLinkedServiceName);
    return linkedService;
}
```

Fügen Sie der **Main**-Methode den folgenden Code hinzu, der einen **verknüpften Azure Storage-Dienst** erstellt. Unter [Eigenschaften von verknüpften Azure Blobdiensten](connector-azure-blob-storage.md#linked-service-properties) erfahren Sie mehr über unterstützte Eigenschaften und Details.

```csharp
client.LinkedServices.CreateOrUpdate(resourceGroup, dataFactoryName, storageLinkedServiceName, StorageLinkedServiceDefinition(client));
```

## <a name="create-datasets"></a>Erstellen von Datasets

In diesem Abschnitt erstellen Sie zwei Datasets: eines für die Quelle und das andere für die Senke. 

### <a name="create-a-dataset-for-source-azure-blob"></a>Erstellen eines Datasets für das Azure-Quellblob

Fügen Sie der **Main**-Methode den folgenden Code hinzu, der ein **Azure blob dataset (Azure-Blobdataset)** erstellt. Erfahren Sie mehr in [Azure Blob dataset properties(Azure Blobdataset-Eigenschaften)](connector-azure-blob-storage.md#dataset-properties) zum Thema unterstützte Eigenschaften und Details.

Sie definieren ein Dataset, das die Quelldaten im Azure-Blob darstellt. Dieses Blobdataset verweist auf den verknüpften Azure Storage-Dienst, den Sie im vorherigen Schritt erstellen und beschreibt:

- Den Speicherort des Blobs, aus dem kopiert werden soll: **FolderPath** (Ordnerpfad) und **FileName** (Dateiname).
- Beachten Sie die Verwendung von Parametern für den Ordnerpfad. „SourceBlobContainer“ ist der Name des Parameters, und der Ausdruck wird mit den Werten, die während der Pipelineausführung übergeben werden ersetzt. Die Syntax zum Definieren von Parametern ist `@pipeline().parameters.<parameterName>`.

Erstellen Sie eine Funktion „SourceBlobDatasetDefinition” in Ihrer Datei „Program.cs“.

```csharp
static DatasetResource SourceBlobDatasetDefinition(DataFactoryManagementClient client)
{
    Console.WriteLine("Creating dataset " + blobSourceDatasetName + "...");
    AzureBlobDataset blobDataset = new AzureBlobDataset
    { 
        FolderPath = new Expression { Value = "@pipeline().parameters.sourceBlobContainer" },
        FileName = inputBlobName,
        LinkedServiceName = new LinkedServiceReference
        {
            ReferenceName = storageLinkedServiceName
        }
    };
    Console.WriteLine(SafeJsonConvert.SerializeObject(blobDataset, client.SerializationSettings));
    DatasetResource dataset = new DatasetResource(blobDataset, name:blobSourceDatasetName);
    return dataset;
}
```

### <a name="create-a-dataset-for-sink-azure-blob"></a>Erstellen eines Datasets für eine Azure Blobsenke

Erstellen Sie eine Funktion „SourceBlobDatasetDefinition” in Ihrer Datei „Program.cs“.

```csharp
static DatasetResource SinkBlobDatasetDefinition(DataFactoryManagementClient client)
{
    Console.WriteLine("Creating dataset " + blobSinkDatasetName + "...");
    AzureBlobDataset blobDataset = new AzureBlobDataset
    {
        FolderPath = new Expression { Value = "@pipeline().parameters.sinkBlobContainer" },
        LinkedServiceName = new LinkedServiceReference
        {
            ReferenceName = storageLinkedServiceName
        }
    };
    Console.WriteLine(SafeJsonConvert.SerializeObject(blobDataset, client.SerializationSettings));
    DatasetResource dataset = new DatasetResource(blobDataset, name: blobSinkDatasetName);
    return dataset;
}
```

Fügen Sie der **Main**-Methode den folgenden Code hinzu, der sowohl ein Azure Blob-Quelldataset als auch ein -Senkendataset erstellt. 

```csharp
client.Datasets.CreateOrUpdate(resourceGroup, dataFactoryName, blobSourceDatasetName, SourceBlobDatasetDefinition(client));

client.Datasets.CreateOrUpdate(resourceGroup, dataFactoryName, blobSinkDatasetName, SinkBlobDatasetDefinition(client));
```

## <a name="create-a-c-class-emailrequest"></a>Erstellen einer C#-Klasse: EmailRequest

Erstellen Sie im C#-Projekt eine Klasse mit dem Namen **EmailRequest**(E-Mail-Anforderung). Hiermit wird definiert, welche Eigenschaften die Pipeline, in der Textanforderung, beim Versenden einer E-Mail sendet. In diesem Tutorial sendet die Pipeline vier Eigenschaften von der Pipeline an die E-Mail:

- **Nachricht**:E-Mail-Text Im Falle eines erfolgreichen Kopiervorgangs enthält diese Eigenschaft die Details der Ausführung (Anzahl der geschriebenen Daten). Im Falle einer fehlgeschlagenen Kopie enthält diese Eigenschaft die Fehlerdetails.
- **Data factory name** (Data Factory-Name): Name der Data Factory
- **Pipeline name** (Pipeline-Name): Name der Pipeline
- **Receiver** (Empfänger): Parameter, der übergeben wird. Diese Eigenschaft gibt den Empfänger der E-Mail an.

```csharp
    class EmailRequest
    {
        [Newtonsoft.Json.JsonProperty(PropertyName = "message")]
        public string message;

        [Newtonsoft.Json.JsonProperty(PropertyName = "dataFactoryName")]
        public string dataFactoryName;

        [Newtonsoft.Json.JsonProperty(PropertyName = "pipelineName")]
        public string pipelineName;

        [Newtonsoft.Json.JsonProperty(PropertyName = "receiver")]
        public string receiver;

        public EmailRequest(string input, string df, string pipeline, string receiverName)
        {
            message = input;
            dataFactoryName = df;
            pipelineName = pipeline;
            receiver = receiverName;
        }
    }
```

## <a name="create-email-workflow-endpoints"></a>Erstellen von E-Mail-Workflow-Endpunkten

Um das Senden einer E-Mail auszulösen, verwenden Sie [Logic Apps](../logic-apps/logic-apps-overview.md) zum Definieren des Workflows. Ausführliche Informationen zum Erstellen eines Logic-App-Workflows finden Sie unter [How to create a logic app (Vorgehensweise: Erstellen einer Logic-App)](../logic-apps/quickstart-create-first-logic-app-workflow.md). 

### <a name="success-email-workflow"></a>Erfolgs-E-Mail-Workflow 

Erstellen Sie einen Logik-App-Workflow mit dem Namen `CopySuccessEmail`. Definieren Sie den Workflow-Trigger als `When an HTTP request is received`, und fügen Sie eine Aktion von `Office 365 Outlook – Send an email` hinzu.

![Erfolgs-E-Mail-Workflow](media/tutorial-control-flow/success-email-workflow.png)

Geben Sie für Ihren Anforderungstrigger das `Request Body JSON Schema` mit dem folgenden JSON ein:

```json
{
    "properties": {
        "dataFactoryName": {
            "type": "string"
        },
        "message": {
            "type": "string"
        },
        "pipelineName": {
            "type": "string"
        },
        "receiver": {
            "type": "string"
        }
    },
    "type": "object"
}
```

Dieses entspricht der **EmailRequest**(E-Mail Anforderungs)-Klasse, die Sie im vorherigen Abschnitt erstellt haben. 

Ihre Anforderung sollte im Logik-App-Designer wie folgt aussehen:

![Logik-App-Designer: Anforderung](media/tutorial-control-flow/logic-app-designer-request.png)

Für die **Send Email (E-Mail senden)** -Aktion, passen Sie die Formatierungseinstellungen der E-Mail Ihren Bedürfnissen an, indem Sie die Eigenschaften nutzen, die in der Textkörper JSON-Schema-Anforderung übergeben wurden. Beispiel:

![Logik-App-Designer: E-Mail-Sendeaktion](media/tutorial-control-flow/send-email-action.png)

Notieren Sie sich die HTTP-Post-Anforderungs-URL für Ihren Erfolgs-E-Mail-Workflow:

```
//Success Request Url
https://prodxxx.eastus.logic.azure.com:443/workflows/000000/triggers/manual/paths/invoke?api-version=2016-10-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=000000
```

## <a name="fail-email-workflow"></a>Fehler-E-Mail-Workflow 

Klonen Sie Ihre **CopySuccessEmail** (E-Mail über den Kopiererfolg), und erstellen Sie einen anderen Logik-Apps Workflow von **CopyFailEmail (E-Mail über den fehlgeschlagenen Kopiervorgang)** . Im Anforderungs-Trigger ist `Request Body JSON schema` gleich. Ändern Sie einfach das Format Ihrer E-Mail wie den `Subject`, um diesen an eine Fehler-E-Mail anzupassen. Beispiel:

![Logik-App-Designer: Fehler-E-Mail-Workflow](media/tutorial-control-flow/fail-email-workflow.png)

Notieren Sie sich die HTTP-Post-Anforderungs-URL für Ihren Fehler-E-Mail-Workflow:

```
//Fail Request Url
https://prodxxx.eastus.logic.azure.com:443/workflows/000000/triggers/manual/paths/invoke?api-version=2016-10-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=000000
```

Sie sollten jetzt über zwei Workflow-URLs verfügen:

```
//Success Request Url
https://prodxxx.eastus.logic.azure.com:443/workflows/000000/triggers/manual/paths/invoke?api-version=2016-10-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=000000

//Fail Request Url
https://prodxxx.eastus.logic.azure.com:443/workflows/000000/triggers/manual/paths/invoke?api-version=2016-10-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=000000
```

## <a name="create-a-pipeline"></a>Erstellen einer Pipeline

Fügen Sie der „Main“-Methode den folgenden Code hinzu, der eine Pipeline mit einer Kopieraktivität und einer „dependsOn“-Eigenschaft erstellt. In diesem Tutorial enthält die Pipeline eine einzige Aktivität: die Kopieraktivität, die das Blobdataset als eine Quelle und ein anderes Blobdataset als eine Senke umfasst. Bei erfolgreicher und fehlgeschlagener Kopieraktivität ruft sie unterschiedliche E-Mail-Aufgaben ab.

In dieser Pipeline verwenden Sie die folgenden Funktionen:

- Parameter
- Webaktivität
- Aktivitätsabhängigkeit
- Verwenden der Ausgabe aus einer Aktivität als eine Eingabe für die nachfolgende Aktivität.

Nehmen wir nun die Aufschlüsselung der folgenden Pipeline, Abschnitt für Abschnitt vor:

```csharp

static PipelineResource PipelineDefinition(DataFactoryManagementClient client)
        {
            Console.WriteLine("Creating pipeline " + pipelineName + "...");
            PipelineResource resource = new PipelineResource
            {
                Parameters = new Dictionary<string, ParameterSpecification>
                {
                    { "sourceBlobContainer", new ParameterSpecification { Type = ParameterType.String } },
                    { "sinkBlobContainer", new ParameterSpecification { Type = ParameterType.String } },
                    { "receiver", new ParameterSpecification { Type = ParameterType.String } }

                },
                Activities = new List<Activity>
                {
                    new CopyActivity
                    {
                        Name = copyBlobActivity,
                        Inputs = new List<DatasetReference>
                        {
                            new DatasetReference
                            {
                                ReferenceName = blobSourceDatasetName
                            }
                        },
                        Outputs = new List<DatasetReference>
                        {
                            new DatasetReference
                            {
                                ReferenceName = blobSinkDatasetName
                            }
                        },
                        Source = new BlobSource { },
                        Sink = new BlobSink { }
                    },
                    new WebActivity
                    {
                        Name = sendSuccessEmailActivity,
                        Method = WebActivityMethod.POST,
                        Url = "https://prodxxx.eastus.logic.azure.com:443/workflows/00000000000000000000000000000000000/triggers/manual/paths/invoke?api-version=2016-10-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=0000000000000000000000000000000000000000000000",
                        Body = new EmailRequest("@{activity('CopyBlobtoBlob').output.dataWritten}", "@{pipeline().DataFactory}", "@{pipeline().Pipeline}", "@pipeline().parameters.receiver"),
                        DependsOn = new List<ActivityDependency>
                        {
                            new ActivityDependency
                            {
                                Activity = copyBlobActivity,
                                DependencyConditions = new List<String> { "Succeeded" }
                            }
                        }
                    },
                    new WebActivity
                    {
                        Name = sendFailEmailActivity,
                        Method =WebActivityMethod.POST,
                        Url = "https://prodxxx.eastus.logic.azure.com:443/workflows/000000000000000000000000000000000/triggers/manual/paths/invoke?api-version=2016-10-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=0000000000000000000000000000000000000000000",
                        Body = new EmailRequest("@{activity('CopyBlobtoBlob').error.message}", "@{pipeline().DataFactory}", "@{pipeline().Pipeline}", "@pipeline().parameters.receiver"),
                        DependsOn = new List<ActivityDependency>
                        {
                            new ActivityDependency
                            {
                                Activity = copyBlobActivity,
                                DependencyConditions = new List<String> { "Failed" }
                            }
                        }
                    }
                }
            };
            Console.WriteLine(SafeJsonConvert.SerializeObject(resource, client.SerializationSettings));
            return resource;
        }
```

Fügen Sie der **Main**-Methode den folgenden Code hinzu, der eine Pipeline erstellt:

```
client.Pipelines.CreateOrUpdate(resourceGroup, dataFactoryName, pipelineName, PipelineDefinition(client));
```

### <a name="parameters"></a>Parameter

Der erste Abschnitt unserer Pipeline definiert Parameter. 

- SourceBlobContainer: Parameter in der Pipeline, die vom Quell-Blobdataset verbraucht werden.
- SinkBlobContainer: Parameter in der Pipeline, die vom Senken-Blobdataset verbraucht werden.
- receiver: Dieser Parameter wird von den beiden Webaktivitäten in der Pipeline verwendet, die Erfolgs- oder Fehlermeldungen an den Empfänger senden, dessen E-Mail-Adresse durch diesen Parameter angegeben ist.


```csharp
Parameters = new Dictionary<string, ParameterSpecification>
    {
        { "sourceBlobContainer", new ParameterSpecification { Type = ParameterType.String } },
        { "sinkBlobContainer", new ParameterSpecification { Type = ParameterType.String } },
        { "receiver", new ParameterSpecification { Type = ParameterType.String } }
    },
```

### <a name="web-activity"></a>Webaktivität

Die Webaktivität ermöglicht einen Aufruf an jeden beliebigen REST-Endpunkt. Weitere Informationen zur Aktivität finden Sie unter [Web Activity (Webaktivität)](control-flow-web-activity.md). Diese Pipeline verwendet eine Webaktivität, um den Logik-Apps-E-Mail-Workflow aufzurufen. Sie erstellen zwei Webaktivitäten: eine, die Aufrufe an den **CopySuccessEmail** (E-Mail über den Kopiererfolg)-Workflow sendet und eine, die den **CopyFailWorkFlow** (E-Mail über den fehlgeschlagenen Kopiervorgang-Workflow) aufruft.

```csharp
        new WebActivity
        {
            Name = sendCopyEmailActivity,
            Method = WebActivityMethod.POST,
            Url = "https://prodxxx.eastus.logic.azure.com:443/workflows/12345",
            Body = new EmailRequest("@{activity('CopyBlobtoBlob').output.dataWritten}", "@{pipeline().DataFactory}", "@{pipeline().Pipeline}", "@pipeline().parameters.receiver"),
            DependsOn = new List<ActivityDependency>
            {
                new ActivityDependency
                {
                    Activity = copyBlobActivity,
                    DependencyConditions = new List<String> { "Succeeded" }
                }
            }
        }
```

Fügen Sie in der „Url“-Eigenschaft die Anforderungs-URL-Endpunkte aus Ihrem Logic-Apps-Workflow entsprechend ein. Übergeben Sie in der Eigenschaft „Text“ eine Instanz der „EmailRequest"-Klasse. Die E-Mail-Anforderung enthält die folgenden Eigenschaften:

- Nachricht: Übergibt den Wert von `@{activity('CopyBlobtoBlob').output.dataWritten`. Greift auf eine Eigenschaft der vorherigen Kopieraktivität zurück, und übergibt den Wert von DataWritten. Für den Fehlerfall, übergeben Sie die Fehlerausgabe anstelle von `@{activity('CopyBlobtoBlob').error.message`.
- Data Factory-Name: Übergibt den Wert von `@{pipeline().DataFactory}`. Dies ist eine Systemvariable, die Ihnen den Zugriff auf den Namen der entsprechenden Data Factory ermöglicht. Eine Liste der Systemvariablen finden Sie im Artikel [Systemvariablen](control-flow-system-variables.md)
- Pipeline-Name: Übergibt den Wert von `@{pipeline().Pipeline}`. Dies ist auch eine Systemvariable, die Ihnen den Zugriff auf den entsprechenden Pipelinenamen ermöglicht. 
- Empfänger: Übergibt den Wert von „\@pipeline().parameters.receiver“). Zugriff auf die Pipeline-Parameter
 
Dieser Code erstellt eine neue Aktivitätsabhängigkeit, abhängig von der vorherigen Kopieraktivität, die erfolgreich abgeschlossen wird.

## <a name="create-a-pipeline-run"></a>Erstellen einer Pipelineausführung

Fügen Sie der **Main**-Methode den folgenden Code hinzu, der **eine Pipelineausführung auslöst**.

```csharp
// Create a pipeline run
Console.WriteLine("Creating pipeline run...");
Dictionary<string, object> arguments = new Dictionary<string, object>
{
    { "sourceBlobContainer", inputBlobPath },
    { "sinkBlobContainer", outputBlobPath },
    { "receiver", emailReceiver }
};
 
CreateRunResponse runResponse = client.Pipelines.CreateRunWithHttpMessagesAsync(resourceGroup, dataFactoryName, pipelineName, arguments).Result.Body;
Console.WriteLine("Pipeline run ID: " + runResponse.RunId);
```

## <a name="main-class"></a>Main-Klasse 

Die endgültige Main-Methode sollte wie folgt aussehen. Erstellen Sie Ihr Programm und führen Sie es aus, um die Ausführung einer Pipeline auszulösen!

```csharp
// Authenticate and create a data factory management client
var context = new AuthenticationContext("https://login.windows.net/" + tenantID);
ClientCredential cc = new ClientCredential(applicationId, authenticationKey);
AuthenticationResult result = context.AcquireTokenAsync("https://management.azure.com/", cc).Result;
ServiceClientCredentials cred = new TokenCredentials(result.AccessToken);
var client = new DataFactoryManagementClient(cred) { SubscriptionId = subscriptionId };

Factory df = CreateOrUpdateDataFactory(client);

client.LinkedServices.CreateOrUpdate(resourceGroup, dataFactoryName, storageLinkedServiceName, StorageLinkedServiceDefinition(client));
client.Datasets.CreateOrUpdate(resourceGroup, dataFactoryName, blobSourceDatasetName, SourceBlobDatasetDefinition(client));
client.Datasets.CreateOrUpdate(resourceGroup, dataFactoryName, blobSinkDatasetName, SinkBlobDatasetDefinition(client));

client.Pipelines.CreateOrUpdate(resourceGroup, dataFactoryName, pipelineName, PipelineDefinition(client));

Console.WriteLine("Creating pipeline run...");
Dictionary<string, object> arguments = new Dictionary<string, object>
{
    { "sourceBlobContainer", inputBlobPath },
    { "sinkBlobContainer", outputBlobPath },
    { "receiver", emailReceiver }
};

CreateRunResponse runResponse = client.Pipelines.CreateRunWithHttpMessagesAsync(resourceGroup, dataFactoryName, pipelineName, arguments).Result.Body;
Console.WriteLine("Pipeline run ID: " + runResponse.RunId);
```

## <a name="monitor-a-pipeline-run"></a>Überwachen einer Pipelineausführung

1. Fügen Sie der **Main**-Methode den folgenden Code hinzu, um den Status der Pipelineausführung kontinuierlich zu überwachen, bis die Pipeline das Kopieren der Daten beendet hat.

    ```csharp
    // Monitor the pipeline run
    Console.WriteLine("Checking pipeline run status...");
    PipelineRun pipelineRun;
    while (true)
    {
        pipelineRun = client.PipelineRuns.Get(resourceGroup, dataFactoryName, runResponse.RunId);
        Console.WriteLine("Status: " + pipelineRun.Status);
        if (pipelineRun.Status == "InProgress")
            System.Threading.Thread.Sleep(15000);
        else
            break;
    }
    ```

2. Fügen Sie der **Main**-Methode den folgenden Code hinzu, der Ausführungsdetails zur Kopieraktivität abruft, z.B. die Größe der gelesenen/geschriebenen Daten.

    ```csharp
    // Check the copy activity run details
    Console.WriteLine("Checking copy activity run details...");

    List<ActivityRun> activityRuns = client.ActivityRuns.ListByPipelineRun(
    resourceGroup, dataFactoryName, runResponse.RunId, DateTime.UtcNow.AddMinutes(-10), DateTime.UtcNow.AddMinutes(10)).ToList(); 
 
    if (pipelineRun.Status == "Succeeded")
    {
        Console.WriteLine(activityRuns.First().Output);
        //SaveToJson(SafeJsonConvert.SerializeObject(activityRuns.First().Output, client.SerializationSettings), "ActivityRunResult.json", folderForJsons);
    }
    else
        Console.WriteLine(activityRuns.First().Error);

    Console.WriteLine("\nPress any key to exit...");
    Console.ReadKey();
    ```

## <a name="run-the-code"></a>Ausführen des Codes

Erstellen und starten Sie die Anwendung, und überprüfen Sie dann die Pipelineausführung.
Die Konsole druckt den Status der Erstellung der Data Factory, des verknüpften Diensts, der Datasets, der Pipeline und der Pipelineausführung aus. Danach wird der Status der Pipelineausführung überprüft. Warten Sie, bis Sie die Ausführungsdetails der Kopieraktivität mit der Größe der gelesenen/geschriebenen Daten sehen. Verwenden Sie anschließend Tools wie z.B. Azure Storage-Explorer, um zu überprüfen, ob die Blobs wie von Ihnen in den Variablen angegeben von „InputBlobPath“ nach „OutputBlobPath“ kopiert werden.

**Beispielausgabe:**

```json
Creating data factory DFTutorialTest...
{
  "location": "East US"
}
Creating linked service AzureStorageLinkedService...
{
  "type": "AzureStorage",
  "typeProperties": {
    "connectionString": {
      "type": "SecureString",
      "value": "DefaultEndpointsProtocol=https;AccountName=***;AccountKey=***"
    }
  }
}
Creating dataset SourceStorageDataset...
{
  "type": "AzureBlob",
  "typeProperties": {
    "folderPath": {
      "type": "Expression",
      "value": "@pipeline().parameters.sourceBlobContainer"
    },
    "fileName": "input.txt"
  },
  "linkedServiceName": {
    "type": "LinkedServiceReference",
    "referenceName": "AzureStorageLinkedService"
  }
}
Creating dataset SinkStorageDataset...
{
  "type": "AzureBlob",
  "typeProperties": {
    "folderPath": {
      "type": "Expression",
      "value": "@pipeline().parameters.sinkBlobContainer"
    }
  },
  "linkedServiceName": {
    "type": "LinkedServiceReference",
    "referenceName": "AzureStorageLinkedService"
  }
}
Creating pipeline Adfv2TutorialBranchCopy...
{
  "properties": {
    "activities": [
      {
        "type": "Copy",
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "BlobSink"
          }
        },
        "inputs": [
          {
            "type": "DatasetReference",
            "referenceName": "SourceStorageDataset"
          }
        ],
        "outputs": [
          {
            "type": "DatasetReference",
            "referenceName": "SinkStorageDataset"
          }
        ],
        "name": "CopyBlobtoBlob"
      },
      {
        "type": "WebActivity",
        "typeProperties": {
          "method": "POST",
          "url": "https://xxxx.eastus.logic.azure.com:443/workflows/... ",
          "body": {
            "message": "@{activity('CopyBlobtoBlob').output.dataWritten}",
            "dataFactoryName": "@{pipeline().DataFactory}",
            "pipelineName": "@{pipeline().Pipeline}",
            "receiver": "@pipeline().parameters.receiver"
          }
        },
        "name": "SendSuccessEmailActivity",
        "dependsOn": [
          {
            "activity": "CopyBlobtoBlob",
            "dependencyConditions": [
              "Succeeded"
            ]
          }
        ]
      },
      {
        "type": "WebActivity",
        "typeProperties": {
          "method": "POST",
          "url": "https://xxx.eastus.logic.azure.com:443/workflows/... ",
          "body": {
            "message": "@{activity('CopyBlobtoBlob').error.message}",
            "dataFactoryName": "@{pipeline().DataFactory}",
            "pipelineName": "@{pipeline().Pipeline}",
            "receiver": "@pipeline().parameters.receiver"
          }
        },
        "name": "SendFailEmailActivity",
        "dependsOn": [
          {
            "activity": "CopyBlobtoBlob",
            "dependencyConditions": [
              "Failed"
            ]
          }
        ]
      }
    ],
    "parameters": {
      "sourceBlobContainer": {
        "type": "String"
      },
      "sinkBlobContainer": {
        "type": "String"
      },
      "receiver": {
        "type": "String"
      }
    }
  }
}
Creating pipeline run...
Pipeline run ID: 00000000-0000-0000-0000-0000000000000
Checking pipeline run status...
Status: InProgress
Status: InProgress
Status: Succeeded
Checking copy activity run details...
{
  "dataRead": 20,
  "dataWritten": 20,
  "copyDuration": 4,
  "throughput": 0.01,
  "errors": [],
  "effectiveIntegrationRuntime": "DefaultIntegrationRuntime (East US)"
}
{}

Press any key to exit...
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie die folgenden Schritte ausgeführt: 

> [!div class="checklist"]
> * Erstellen einer Data Factory.
> * Erstellen eines verknüpften Azure Storage Diensts.
> * Erstellen eines Azure-Blobdatasets.
> * Erstellen einer Pipeline, die eine Kopieraktivität und eine Webaktivität enthält.
> * Senden von Aktivitätsausgaben an nachfolgende Aktivitäten.
> * Verwenden von Parameterübergabe und Systemvariablen.
> * Starten einer Pipelineausführung.
> * Überwachen der Pipeline- und Aktivitätsausführungen.

Sie können jetzt mit dem Abschnitt „Konzepte“ fortfahren, um weitere Informationen zu Azure Data Factory zu erhalten.
> [!div class="nextstepaction"]
>[Pipelines und Aktivitäten](concepts-pipelines-activities.md)