---
title: Kopieren von Blobs aus einem Speicherkonto in ein Azure Media Services-Medienobjekt | Microsoft-Dokumentation
description: In diesem Thema wird erläutert, wie ein vorhandener Blob in ein Media Services-Medienobjekt kopiert wird. Im folgenden Beispiel werden Azure Media Services .NET SDK-Erweiterungen verwendet.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: a1da207a295b40f8d455635d687083bf69e90fdf
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67068892"
---
# <a name="copying-existing-blobs-into-a-media-services-asset"></a>Kopieren vorhandener Blobs in ein Media Services-Medienobjekt

> [!NOTE]
> Media Services v2 werden derzeit keine neuen Features oder Funktionen hinzugefügt. <br/>Sehen Sie sich die neuste Version – [Media Services v3](https://docs.microsoft.com/azure/media-services/latest/) – an. Lesen Sie außerdem die [Hinweise zur Migration von v2 zu v3](../latest/migrate-from-v2-to-v3.md).

In diesem Artikel wird erläutert, wie Blobs mithilfe von [Azure Media Services .NET SDK-Erweiterungen](https://github.com/Azure/azure-sdk-for-media-services-extensions/) aus einem Speicherkonto in ein neues Azure Media Services-Medienobjekt (AMS) kopiert werden.

Sie sollten nicht versuchen, den Inhalt von BLOB-Containern, die mit Media Services generiert wurden, ohne die Verwendung von Media Service-APIs zu ändern.

Die Erweiterungsmethoden können mit Folgendem verwendet werden:

- Normalen Medienobjekten
- Livearchivobjekten (FragBlob-Format).
- Quell- und Zielmedienobjekten, die zu unterschiedlichen Media Services-Konten (sogar in verschiedenen Rechenzentren) gehören Allerdings fallen dabei möglicherweise Kosten an. Die Preisübersicht und weitere Informationen finden Sie unter [Azure-Preise](https://azure.microsoft.com/pricing/#header-11).

In diesem Artikel werden zwei Codebeispiele gezeigt:

1. Kopieren von Blobs aus einem Medienobjekt in einem AMS-Konto in ein neues Medienobjekt in einem anderen AMS-Konto
2. Kopieren von Blobs aus einem Speicherkonto in ein neues Medienobjekt in einem AMS-Konto

## <a name="copy-blobs-between-two-ams-accounts"></a>Kopieren von Blobs zwischen zwei AMS-Konten  

### <a name="prerequisites"></a>Voraussetzungen

Zwei Media Services-Konten Weitere Informationen finden Sie unter [Erstellen eines Media Services-Kontos](media-services-portal-create-account.md).

### <a name="download-sample"></a>Beispiel herunterladen
Sie können die Schritte in diesem Artikel befolgen oder ein Beispiel mit dem in diesem Artikel beschriebenen Code [hier](https://azure.microsoft.com/documentation/samples/media-services-dotnet-copy-blob-into-asset/) herunterladen.

### <a name="set-up-your-project"></a>Einrichten des Projekts

1. Richten Sie die Entwicklungsumgebung wie unter [Media Services-Entwicklung mit .NET](media-services-dotnet-how-to-use.md) beschrieben ein. 
2. Fügen Sie der CONFIG-Datei den Abschnitt „appSettings“ hinzu, und ändern Sie die Werte entsprechend Ihren Media Services-Konten, dem Zielspeicherkonto und der Quellmedienobjekt-ID.  

```xml
<appSettings>
    <add key="AMSSourceAADTenantDomain" value="tenant"/>
    <add key="AMSSourceRESTAPIEndpoint" value="endpoint"/>

    <add key="SourceAMSClientId" value="clientID"/>
    <add key="SourceAMSClientSecret" value="clientSecret"/>

    <add key="SourceAssetID" value="nb:cid:UUID:<id>"/>

    <add key="AMSDestAADTenantDomain" value="tenant"/>
    <add key="AMSDestRESTAPIEndpoint" value="endpoint"/>

    <add key="DestAMSClientId" value="clientID"/>
    <add key="DestAMSClientSecret" value="clientSecret"/>

    <add key="DestStorageAccountName" value="name"/>
    <add key="DestStorageAccountKey" value="key"/>

</appSettings>
```

### <a name="copy-blobs-from-an-asset-in-one-ams-account-into-an-asset-in-another-ams-account"></a>Kopieren von Blobs aus einem Medienobjekt in einem AMS-Konto in ein Medienobjekt in einem anderen AMS-Konto

Der folgende Code verwendet die Erweiterungsmethode **IAsset.Copy**, um alle Dateien im Quellmedienobjekt mithilfe einer einzelnen Erweiterung in das Zielmedienobjekt zu kopieren.

```csharp
using System;
using Microsoft.WindowsAzure.MediaServices.Client;
using System.Linq;
using System.Configuration;
using Microsoft.WindowsAzure.Storage.Auth;

namespace CopyExistingBlobsIntoAsset
{
    class Program
    {
        static string _sourceAADTenantDomain = 
            ConfigurationManager.AppSettings["AMSSourceAADTenantDomain"];
        static string _sourceRESTAPIEndpoint = 
            ConfigurationManager.AppSettings["AMSSourceRESTAPIEndpoint"];
        static string _sourceClientId = 
            ConfigurationManager.AppSettings["SourceAMSClientId"];
        static string _sourceClientSecret = 
            ConfigurationManager.AppSettings["SourceAMSClientSecret"];

        static string _destAADTenantDomain = 
            ConfigurationManager.AppSettings["AMSDestAADTenantDomain"];
        static string _destRESTAPIEndpoint = 
            ConfigurationManager.AppSettings["AMSDestRESTAPIEndpoint"];
        static string _destClientId = 
            ConfigurationManager.AppSettings["DestAMSClientId"];
        static string _destClientSecret = 
            ConfigurationManager.AppSettings["DestAMSClientSecret"];

        static string _destStorageAccountName = 
            ConfigurationManager.AppSettings["DestStorageAccountName"];
        static string _destStorageAccountKey = 
            ConfigurationManager.AppSettings["DestStorageAccountKey"];
        static string _sourceAssetID = 
            ConfigurationManager.AppSettings["SourceAssetID"];

        private static CloudMediaContext _sourceContext = null;
        private static CloudMediaContext _destContext = null;

        static void Main(string[] args)
        {
            AzureAdTokenCredentials tokenCredentials1 = new AzureAdTokenCredentials(_sourceAADTenantDomain,
                   new AzureAdClientSymmetricKey(_sourceClientId, _sourceClientSecret),
                   AzureEnvironments.AzureCloudEnvironment);

            AzureAdTokenCredentials tokenCredentials2 = new AzureAdTokenCredentials(_destAADTenantDomain,
                   new AzureAdClientSymmetricKey(_destClientId, _destClientSecret),
                   AzureEnvironments.AzureCloudEnvironment);

            var tokenProvider1 = new AzureAdTokenProvider(tokenCredentials1);
            var tokenProvider2 = new AzureAdTokenProvider(tokenCredentials2);

            // Create the context for your source Media Services account.
            _sourceContext = new CloudMediaContext(new Uri(_sourceRESTAPIEndpoint), tokenProvider1);

            // Create the context for your destination Media Services account.
            _destContext = new CloudMediaContext(new Uri(_destRESTAPIEndpoint), tokenProvider2);

            // Get the credentials of the default Storage account bound to your destination Media Services account.
            StorageCredentials destinationStorageCredentials =
                new StorageCredentials(_destStorageAccountName, _destStorageAccountKey);

            // Get a reference to the source asset in the source context.
            IAsset sourceAsset = _sourceContext.Assets.Where(asset => asset.Id == _sourceAssetID).First();

            // Create an empty destination asset in the destination context.
            IAsset destinationAsset = _destContext.Assets.Create(sourceAsset.Name, AssetCreationOptions.None);

            // Copy the files in the source asset instance into the destination asset instance.
            // There is an additional overload with async support.
            sourceAsset.Copy(destinationAsset, destinationStorageCredentials);

            Console.WriteLine("Done");
        }
    }
}
```

## <a name="copy-blobs-from-a-storage-account-into-an-ams-account"></a>Kopieren von Blobs aus einem Speicherkonto in ein AMS-Konto 

### <a name="prerequisites"></a>Voraussetzungen

- Ein Speicherkonto, aus dem Blobs kopiert werden sollen
- Ein AMS-Konto, in das Blobs kopiert werden sollen

### <a name="set-up-your-project"></a>Einrichten des Projekts

1. Richten Sie die Entwicklungsumgebung wie unter [Media Services-Entwicklung mit .NET](media-services-dotnet-how-to-use.md) beschrieben ein. 
2. Fügen Sie der CONFIG-Datei den appSettings-Abschnitt hinzu, und ändern Sie die Werte basierend auf dem Quellspeicher und den AMS-Zielkonten.

```xml
<appSettings>
    <add key="SourceStorageAccountName" value="name" />
    <add key="SourceStorageAccountKey" value="key" />
    <add key="NameOfBlobContainerYouWantToCopy" value="BlobContainerName"/>
    
    <add key="AMSAADTenantDomain" value="tenant"/>
    <add key="AMSRESTAPIEndpoint" value="endpoint"/>
    <add key="AMSClientId" value="clientID"/>
    <add key="AMSClientSecret" value="clientSecret"/>
    <add key="AMSStorageAccountName" value="name"/>
    <add key="AMSStorageAccountKey" value="key"/>
</appSettings>
```

### <a name="copy-blobs-from-some-storage-account-into-a-new-asset-in-an-ams-account"></a>Kopieren von Blobs aus einem Speicherkonto in ein neues Medienobjekt in einem AMS-Konto

Der folgende Code kopiert Blobs aus einem Speicherkonto in ein Media Services-Medienobjekt. 

>[!NOTE]
>Es gilt ein Grenzwert von 1.000.000 Richtlinien für verschiedene AMS-Richtlinien (z.B. für die Locator-Richtlinie oder für ContentKeyAuthorizationPolicy). Wenn Sie immer die gleichen Tage/Zugriffsberechtigungen verwenden, z.B. Richtlinien für Locator, die für einen längeren Zeitraum vorgesehen sind (Richtlinien ohne Upload), sollten Sie dieselbe Richtlinien-ID verwenden. Weitere Informationen dazu finden Sie in [diesem Artikel](media-services-dotnet-manage-entities.md#limit-access-policies).

```csharp
using System;
using System.Configuration;
using System.Linq;
using Microsoft.WindowsAzure.MediaServices.Client;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
    
namespace CopyExistingBlobsIntoAsset
{
    class Program
    {
        // Read values from the App.config file.
        private static readonly string _sourceStorageAccountName =
            ConfigurationManager.AppSettings["SourceStorageAccountName"];
        private static readonly string _sourceStorageAccountKey =
            ConfigurationManager.AppSettings["SourceStorageAccountKey"];
        private static readonly string _NameOfBlobContainerYouWantToCopy =
            ConfigurationManager.AppSettings["NameOfBlobContainerYouWantToCopy"];

        private static readonly string _AMSAADTenantDomain =
            ConfigurationManager.AppSettings["AMSAADTenantDomain"];
        private static readonly string _AMSRESTAPIEndpoint =
            ConfigurationManager.AppSettings["AMSRESTAPIEndpoint"];
        private static readonly string _AMSClientId =
            ConfigurationManager.AppSettings["AMSClientId"];
        private static readonly string _AMSClientSecret =
            ConfigurationManager.AppSettings["AMSClientSecret"];
        private static readonly string _AMSStorageAccountName =
            ConfigurationManager.AppSettings["AMSStorageAccountName"];
        private static readonly string _AMSStorageAccountKey =
            ConfigurationManager.AppSettings["AMSStorageAccountKey"];

        // Field for service context.
        private static CloudMediaContext _context = null;
        private static CloudStorageAccount _sourceStorageAccount = null;
        private static CloudStorageAccount _destinationStorageAccount = null;

        static void Main(string[] args)
        {
            AzureAdTokenCredentials tokenCredentials = new AzureAdTokenCredentials(_AMSAADTenantDomain,
               new AzureAdClientSymmetricKey(_AMSClientId, _AMSClientSecret),
               AzureEnvironments.AzureCloudEnvironment);
            
            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            // Create the context for your source Media Services account.
            _context = new CloudMediaContext(new Uri(_AMSRESTAPIEndpoint), tokenProvider);

            _sourceStorageAccount =
                new CloudStorageAccount(new StorageCredentials(_sourceStorageAccountName,
                    _sourceStorageAccountKey), true);

            _destinationStorageAccount =
                new CloudStorageAccount(new StorageCredentials(_AMSStorageAccountName,
                    _AMSStorageAccountKey), true);

            CloudBlobClient sourceCloudBlobClient =
                _sourceStorageAccount.CreateCloudBlobClient();
            CloudBlobContainer sourceContainer =
                sourceCloudBlobClient.GetContainerReference(_NameOfBlobContainerYouWantToCopy);

            CreateAssetFromExistingBlobs(sourceContainer);

            Console.WriteLine("Done");
        }

        static private IAsset CreateAssetFromExistingBlobs(CloudBlobContainer sourceBlobContainer)
        {
            CloudBlobClient destBlobStorage = _destinationStorageAccount.CreateCloudBlobClient();

            // Create a new asset. 
            IAsset asset = _context.Assets.Create("NewAsset_" + Guid.NewGuid(), AssetCreationOptions.None);

            IAccessPolicy writePolicy = _context.AccessPolicies.Create("writePolicy",
                TimeSpan.FromHours(24), AccessPermissions.Write);

            ILocator destinationLocator =
                _context.Locators.CreateLocator(LocatorType.Sas, asset, writePolicy);

            // Get the asset container URI and Blob copy from mediaContainer to assetContainer. 
            CloudBlobContainer destAssetContainer =
                destBlobStorage.GetContainerReference((new Uri(destinationLocator.Path)).Segments[1]);

            if (destAssetContainer.CreateIfNotExists())
            {
                destAssetContainer.SetPermissions(new BlobContainerPermissions
                {
                    PublicAccess = BlobContainerPublicAccessType.Blob
                });
            }

            var blobList = sourceBlobContainer.ListBlobs();

            foreach (CloudBlockBlob sourceBlob in blobList)
            {
                var assetFile = asset.AssetFiles.Create((sourceBlob as ICloudBlob).Name);

                ICloudBlob destinationBlob = destAssetContainer.GetBlockBlobReference(assetFile.Name);

                CopyBlob(sourceBlob, destAssetContainer);
                
                sourceBlob.FetchAttributes();
                assetFile.ContentFileSize = (sourceBlob as ICloudBlob).Properties.Length;
                assetFile.Update();
                Console.WriteLine("File {0} is of {1} size", assetFile.Name, assetFile.ContentFileSize);
            }

            asset.Update();

            destinationLocator.Delete();
            writePolicy.Delete();

            // Set the primary asset file.
            // If, for example, we copied a set of Smooth Streaming files, 
            // set the .ism file to be the primary file. 
            // If we, for example, copied an .mp4, then the mp4 would be the primary file. 
            var ismAssetFile = asset.AssetFiles.ToList().
                Where(f => f.Name.EndsWith(".ism", StringComparison.OrdinalIgnoreCase)).FirstOrDefault();

            // The following code assigns the first .ism file as the primary file in the asset.
            // An asset should have one .ism file.  
            if (ismAssetFile != null)
            {
                ismAssetFile.IsPrimary = true;
                ismAssetFile.Update();
            }

            return asset;
        }

        /// <summary>
        /// Copies the specified blob into the specified container.
        /// </summary>
        /// <param name="sourceBlob">The source container.</param>
        /// <param name="destinationContainer">The destination container.</param>
        static private void CopyBlob(ICloudBlob sourceBlob, CloudBlobContainer destinationContainer)
        {
            var signature = sourceBlob.GetSharedAccessSignature(new SharedAccessBlobPolicy
            {
                Permissions = SharedAccessBlobPermissions.Read,
                SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24)
            });

            var destinationBlob = destinationContainer.GetBlockBlobReference(sourceBlob.Name);

            if (destinationBlob.Exists())
            {
                Console.WriteLine(string.Format("Destination blob '{0}' already exists. Skipping.", destinationBlob.Uri));
            }
            else
            {

                // Display the size of the source blob.
                Console.WriteLine(sourceBlob.Properties.Length);

                Console.WriteLine(string.Format("Copy blob '{0}' to '{1}'", sourceBlob.Uri, destinationBlob.Uri));
                destinationBlob.StartCopy(new Uri(sourceBlob.Uri.AbsoluteUri + signature));

                while (true)
                {
                    // The StartCopyFromBlob is an async operation, 
                    // so we want to check if the copy operation is completed before proceeding. 
                    // To do that, we call FetchAttributes on the blob and check the CopyStatus. 
                    destinationBlob.FetchAttributes();
                    if (destinationBlob.CopyState.Status != CopyStatus.Pending)
                    {
                        break;
                    }
                    //It's still not completed. So wait for some time.
                    System.Threading.Thread.Sleep(1000);
                }

                // Display the size of the destination blob.
                Console.WriteLine(destinationBlob.Properties.Length);

            }
        }
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

Sie können nun Ihre hochgeladenen Medienobjekte codieren. Weitere Informationen finden Sie unter [Codieren von Medienobjekten](media-services-portal-encode.md).

Sie können auch mithilfe von Azure Functions einen Codierungsauftrag auslösen, wenn eine Datei im konfigurierten Container eingeht. Weitere Informationen finden Sie in [diesem Beispiel](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).

## <a name="media-services-learning-paths"></a>Media Services-Lernpfade
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

