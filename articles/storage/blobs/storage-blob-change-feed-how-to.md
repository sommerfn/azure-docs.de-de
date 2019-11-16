---
title: Verarbeiten von Änderungsfeeds in Azure Blob Storage (Vorschau) | Microsoft-Dokumentation
description: Erfahren Sie, wie Änderungsfeedprotokolle in einer .NET-Clientanwendung verarbeitet werden.
author: normesta
ms.author: normesta
ms.date: 11/04/2019
ms.topic: article
ms.service: storage
ms.subservice: blobs
ms.reviewer: sadodd
ms.openlocfilehash: 75995eeb3f8255cb4c60d5be267f9c343edfea89
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74111858"
---
# <a name="process-change-feed-in-azure-blob-storage-preview"></a>Verarbeiten von Änderungsfeeds in Azure Blob Storage (Vorschau)

Der Änderungsfeed stellt Transaktionsprotokolle für alle Änderungen bereit, die in den Blobs und den Blobmetadaten in Ihrem Speicherkonto auftreten. In diesem Artikel wird gezeigt, wie Sie Änderungsfeeddatensätze mithilfe der Change Feed Processor-Bibliothek für Blobs lesen.

Weitere Informationen zu Änderungsfeeds finden Sie unter [Änderungsfeed in Azure Blob Storage (Vorschau)](storage-blob-change-feed.md).

> [!NOTE]
> Der Änderungsfeed befindet sich in der öffentlichen Vorschauphase und ist in den Regionen **westcentralus** und **westus2** verfügbar. Weitere Informationen zu dieser Funktion sowie bekannten Problemen und Einschränkungen finden Sie unter [Änderungsfeedunterstützung in Azure Blob Storage](storage-blob-change-feed.md). Die Change Feed Processor-Bibliothek kann sich ab dem jetzigen Zeitpunkt bis zum Zeitpunkt der allgemeinen Verfügbarkeit dieser Bibliothek ändern.

## <a name="get-the-blob-change-feed-processor-library"></a>Anfordern der Change Feed Processor-Bibliothek

1. Fügen Sie in Visual Studio die URL-`https://azuresdkartifacts.blob.core.windows.net/azuresdkpartnerdrops/index.json` zu den NuGet-Paketquellen hinzu. 

   Entsprechende Informationen finden Sie unter [Paketquellen](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-visual-studio#package-sources).

2. Suchen Sie im NuGet-Paket-Manager nach dem Paket **Microsoft.Azure.Storage.Changefeed** und installieren Sie es in Ihrem Projekt. 

   Weitere Informationen finden Sie unter [Suchen und Installieren eines Pakets](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-visual-studio#find-and-install-a-package).

## <a name="connect-to-the-storage-account"></a>Verbinden mit dem Speicherkonto

Analysieren Sie die Verbindungszeichenfolge, indem Sie die [CloudStorageAccount.TryParse](/dotnet/api/microsoft.azure.storage.cloudstorageaccount.tryparse)-Methode aufrufen. 

Erstellen Sie dann ein Objekt, das Blob Storage in Ihrem Speicherkonto darstellt, indem Sie die [CloudStorageAccount.CreateCloudBlobClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.blob.blobaccountextensions.createcloudblobclient)-Methode aufrufen.

```cs
public bool GetBlobClient(ref CloudBlobClient cloudBlobClient, string storageConnectionString)
{
    if (CloudStorageAccount.TryParse
        (storageConnectionString, out CloudStorageAccount storageAccount))
        {
            cloudBlobClient = storageAccount.CreateCloudBlobClient();

            return true;
        }
        else
        {
            return false;
        }
    }
}
```

## <a name="initialize-the-change-feed"></a>Änderungsfeed initialisieren

Fügen Sie die folgenden using-Anweisungen am Anfang Ihrer Codedatei ein. 

```csharp
using Avro.Generic;
using ChangeFeedClient;
```

Erstellen Sie dann eine Instanz der **ChangeFeed**-Klasse, indem Sie die **GetContainerReference**-Methode aufrufen. Übergeben Sie den Namen des Änderungsfeedcontainers.

```csharp
public async Task<ChangeFeed> GetChangeFeed(CloudBlobClient cloudBlobClient)
{
    CloudBlobContainer changeFeedContainer =
        cloudBlobClient.GetContainerReference("$blobchangefeed");

    ChangeFeed changeFeed = new ChangeFeed(changeFeedContainer);
    await changeFeed.InitializeAsync();

    return changeFeed;
}
```

## <a name="reading-records"></a>Lesen von Datensätzen

> [!NOTE]
> Der Änderungsfeed ist eine unveränderliche und schreibgeschützte Entität in Ihrem Speicherkonto. Mehrere Anwendungen können den Änderungsfeed gleichzeitig und unabhängig voneinander nach Belieben lesen und verarbeiten. Datensätze werden nicht aus dem Änderungsfeed entfernt, wenn sie von einer Anwendung gelesen werden. Der Lese- oder Iterationszustand jedes Readers ist unabhängig und wird nur von der jeweiligen Anwendung verwaltet.

Die einfachste Möglichkeit zum Lesen von Datensätzen ist das Erstellen einer Instanz der **ChangeFeedReader**-Klasse. 

In diesem Beispiel werden alle Datensätze im Änderungsfeed durchlaufen. Anschließend werden in der Konsole einige Werte der einzelnen Datensätze ausgegeben. 
 
```csharp
public async Task ProcessRecords(ChangeFeed changeFeed)
{
    ChangeFeedReader processor = await changeFeed.CreateChangeFeedReaderAsync();

    ChangeFeedRecord currentRecord = null;
    do
    {
        currentRecord = await processor.GetNextItemAsync();

        if (currentRecord != null)
        {
            string subject = currentRecord.record["subject"].ToString();
            string eventType = ((GenericEnum)currentRecord.record["eventType"]).Value;
            string api = ((GenericEnum)((GenericRecord)currentRecord.record["data"])["api"]).Value;

            Console.WriteLine("Subject: " + subject + "\n" +
                "Event Type: " + eventType + "\n" +
                "Api: " + api);
        }

    } while (currentRecord != null);
}
```

## <a name="resuming-reading-records-from-a-saved-position"></a>Fortsetzen des Lesens von Datensätzen ausgehend von einer gespeicherten Position

Sie können die Leseposition in Ihrem Änderungsfeed speichern und die Iteration der Datensätze zu einem späteren Zeitpunkt fortsetzen. Sie können den Iterationszustand des Änderungsfeeds jederzeit mit der **ChangeFeedReader.SerializeState()** -Methode speichern. Der Zustand ist eine **Zeichenfolge** und die Anwendung kann diesen Zustand abhängig von der Gestaltung der Anwendung speichern (z. B. in einer Datenbank oder einer Datei).

```csharp
    string currentReadState = processor.SerializeState();
```

Sie können die Iteration der Datensätze ab dem letzten Zustand fortsetzen, indem Sie **ChangeFeedReader** mit **CreateChangeFeedReaderFromPointerAsync**-Methode erstellen.

```csharp
public async Task ProcessRecordsFromLastPosition(ChangeFeed changeFeed, string lastReadState)
{
    ChangeFeedReader processor = await changeFeed.CreateChangeFeedReaderFromPointerAsync(lastReadState);

    ChangeFeedRecord currentRecord = null;
    do
    {
        currentRecord = await processor.GetNextItemAsync();

        if (currentRecord != null)
        {
            string subject = currentRecord.record["subject"].ToString();
            string eventType = ((GenericEnum)currentRecord.record["eventType"]).Value;
            string api = ((GenericEnum)((GenericRecord)currentRecord.record["data"])["api"]).Value;

            Console.WriteLine("Subject: " + subject + "\n" +
                "Event Type: " + eventType + "\n" +
                "Api: " + api);
        }

    } while (currentRecord != null);
}

```

## <a name="stream-processing-of-records"></a>Streamverarbeitung von Datensätzen

Sie können Änderungsfeeddatensätze verarbeiten, wenn sie ankommen. Weitere Informationen finden Sie unter [Spezifikationen](storage-blob-change-feed.md#specifications).

```csharp
public async Task ProcessRecordsStream(ChangeFeed changeFeed, int waitTimeMs)
{
    ChangeFeedReader processor = await changeFeed.CreateChangeFeedReaderAsync();

    ChangeFeedRecord currentRecord = null;
    while (true)
    {
        do
        {
            currentRecord = await processor.GetNextItemAsync();

            if (currentRecord != null)
            {
                string subject = currentRecord.record["subject"].ToString();
                string eventType = ((GenericEnum)currentRecord.record["eventType"]).Value;
                string api = ((GenericEnum)((GenericRecord)currentRecord.record["data"])["api"]).Value;

                Console.WriteLine("Subject: " + subject + "\n" +
                    "Event Type: " + eventType + "\n" +
                    "Api: " + api);
            }

        } while (currentRecord != null);

        await Task.Delay(waitTimeMs);
    }
}
```

## <a name="reading-records-within-a-time-range"></a>Lesen von Datensätzen innerhalb eines Zeitbereichs

Der Änderungsfeed wird anhand der Zeit, zu der die Änderung erfolgt, in stündliche Segmente organisiert. Weitere Informationen finden Sie unter [Spezifikationen](storage-blob-change-feed.md#specifications). Sie können Datensätze aus Änderungsfeedsegmenten lesen, die in einem bestimmten Zeitbereich liegen.

Im folgenden Beispiel werden die Startzeiten aller Segmente abgerufen. Anschließend wird die Liste durchlaufen, bis die Startzeit über die Zeit des letzten nutzbaren Segments oder über die Endzeit des gewünschten Bereichs hinausgeht. 

### <a name="selecting-segments-for-a-time-range"></a>Auswählen von Segmenten für einen Zeitbereich

```csharp
public async Task<List<DateTimeOffset>> GetChangeFeedSegmentRefsForTimeRange
    (ChangeFeed changeFeed, DateTimeOffset startTime, DateTimeOffset endTime)
{
    List<DateTimeOffset> result = new List<DateTimeOffset>();

    DateTimeOffset stAdj = startTime.AddMinutes(-15);
    DateTimeOffset enAdj = endTime.AddMinutes(15);

    DateTimeOffset lastConsumable = (DateTimeOffset)changeFeed.LastConsumable;

    List<DateTimeOffset> segments = 
        (await changeFeed.ListAvailableSegmentTimesAsync()).ToList();

    foreach (var segmentStart in segments)
    {
        if (lastConsumable.CompareTo(segmentStart) < 0)
        {
            break;
        }

        if (enAdj.CompareTo(segmentStart) < 0)
        {
            break;
        }

        DateTimeOffset segmentEnd = segmentStart.AddMinutes(60);

        bool overlaps = stAdj.CompareTo(segmentEnd) < 0 && 
            segmentStart.CompareTo(enAdj) < 0;

        if (overlaps)
        {
            result.Add(segmentStart);
        }
    }

    return result;
}
```

### <a name="reading-records-in-a-segment"></a>Lesen von Datensätzen in einem Segment

Sie können Datensätze aus einzelnen Segmenten oder Segmentbereichen lesen.

```csharp
public async Task ProcessRecordsInSegment(ChangeFeed changeFeed, DateTimeOffset segmentOffset)
{
    ChangeFeedSegment segment = new ChangeFeedSegment(segmentOffset, changeFeed);
    await segment.InitializeAsync();

    ChangeFeedSegmentReader processor = await segment.CreateChangeFeedSegmentReaderAsync();

    ChangeFeedRecord currentRecord = null;
    do
    {
        currentRecord = await processor.GetNextItemAsync();

        if (currentRecord != null)
        {
            string subject = currentRecord.record["subject"].ToString();
            string eventType = ((GenericEnum)currentRecord.record["eventType"]).Value;
            string api = ((GenericEnum)((GenericRecord)currentRecord.record["data"])["api"]).Value;

            Console.WriteLine("Subject: " + subject + "\n" +
                "Event Type: " + eventType + "\n" +
                "Api: " + api);
        }

    } while (currentRecord != null);
}
```

## <a name="read-records-starting-from-a-time"></a>Lesen von Datensätzen ab einem Zeitpunkt

Sie können die Datensätze des Änderungsfeeds ausgehend von einem Anfangssegment bis zum Ende lesen. Ähnlich wie beim Lesen von Datensätzen in einem Zeitbereich können Sie die Segmente auflisten und ein Segment auswählen, an dem die Iteration beginnen soll.

In diesem Beispiel wird der [DateTimeOffset](https://docs.microsoft.com/dotnet/api/system.datetimeoffset?view=netframework-4.8)-Wert des ersten zu verarbeitenden Segments abgerufen.

```csharp
public async Task<DateTimeOffset> GetChangeFeedSegmentRefAfterTime
    (ChangeFeed changeFeed, DateTimeOffset timestamp)
{
    DateTimeOffset result = new DateTimeOffset();

    DateTimeOffset lastConsumable = (DateTimeOffset)changeFeed.LastConsumable;
    DateTimeOffset lastConsumableEnd = lastConsumable.AddMinutes(60);

    DateTimeOffset timestampAdj = timestamp.AddMinutes(-15);

    if (lastConsumableEnd.CompareTo(timestampAdj) < 0)
    {
        return result;
    }

    List<DateTimeOffset> segments = (await changeFeed.ListAvailableSegmentTimesAsync()).ToList();
    foreach (var segmentStart in segments)
    {
        DateTimeOffset segmentEnd = segmentStart.AddMinutes(60);
        if (timestampAdj.CompareTo(segmentEnd) <= 0)
        {
            result = segmentStart;
            break;
        }
    }

    return result;
}
```

In diesem Beispiel werden Änderungsfeeddatensätze ab dem [DateTimeOffset](https://docs.microsoft.com/dotnet/api/system.datetimeoffset?view=netframework-4.8) eines Anfangssegments verarbeitet.

```csharp
public async Task ProcessRecordsStartingFromSegment(ChangeFeed changeFeed, DateTimeOffset segmentStart)
{
    TimeSpan waitTime = new TimeSpan(60 * 1000);

    ChangeFeedSegment segment = new ChangeFeedSegment(segmentStart, changeFeed);

    await segment.InitializeAsync();

    while (true)
    {
        while (!await IsSegmentConsumableAsync(changeFeed, segment))
        {
            await Task.Delay(waitTime);
        }

        ChangeFeedSegmentReader reader = await segment.CreateChangeFeedSegmentReaderAsync();

        do
        {
            await reader.CheckForFinalizationAsync();

            ChangeFeedRecord currentItem = null;
            do
            {
                currentItem = await reader.GetNextItemAsync();
                if (currentItem != null)
                {
                    string subject = currentItem.record["subject"].ToString();
                    string eventType = ((GenericEnum)currentItem.record["eventType"]).Value;
                    string api = ((GenericEnum)((GenericRecord)currentItem.record["data"])["api"]).Value;

                    Console.WriteLine("Subject: " + subject + "\n" +
                        "Event Type: " + eventType + "\n" +
                        "Api: " + api);
                }
            } while (currentItem != null);

            if (segment.timeWindowStatus != ChangefeedSegmentStatus.Finalized)
            {
                await Task.Delay(waitTime);
            }
        } while (segment.timeWindowStatus != ChangefeedSegmentStatus.Finalized);

        segment = await segment.GetNextSegmentAsync(); // TODO: What if next window doesn't yet exist?
        await segment.InitializeAsync(); // Should update status, shard list.
    }
}

private async Task<bool> IsSegmentConsumableAsync(ChangeFeed changeFeed, ChangeFeedSegment segment)
{
    if (changeFeed.LastConsumable >= segment.startTime)
    {
        return true;
    }
    await changeFeed.InitializeAsync();
    return changeFeed.LastConsumable >= segment.startTime;
}
```

>[!TIP]
> Ein Segment von kann Änderungsfeedprotokolle in einem oder mehreren *chunkFilePath*-Pfaden aufweisen. Bei mehreren *chunkFilePath*-Pfaden wurden die Datensätze vom System intern in mehrere Shards partitioniert, um den Veröffentlichungsdurchsatz zu verwalten. Es ist sichergestellt, dass jede Partition des Segments Änderungen für sich gegenseitig ausschließende Blobs enthält und ohne Missachtung der Reihenfolge unabhängig voneinander verarbeitet werden kann. Sie können die **ChangeFeedSegmentShardReader**-Klasse verwenden, um Datensätze auf Shardebene zu durchlaufen, wenn dies für das jeweilige Szenario am effizientesten ist.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über Änderungsfeedprotokolle. Siehe [Verarbeiten von Änderungsfeeds in Azure Blob Storage (Vorschau)](storage-blob-change-feed.md).
