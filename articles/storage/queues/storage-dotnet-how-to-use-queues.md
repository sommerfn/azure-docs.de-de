---
title: 'Erste Schritte mit Azure Queue Storage mit .NET: Azure Storage'
description: Azure-Warteschlangen ermöglichen zuverlässiges, asynchrones Messaging zwischen Anwendungskomponenten. Das Cloud-Messaging ermöglicht Ihren Anwendungskomponenten die unabhängige Skalierung.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 05/21/2019
ms.service: storage
ms.subservice: queues
ms.topic: conceptual
ms.reviewer: cbrooks
ms.openlocfilehash: aa92b72b09ed28b41d85ac7c7605077761657d40
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2019
ms.locfileid: "68721559"
---
# <a name="get-started-with-azure-queue-storage-using-net"></a>Erste Schritte mit Azure Queue Storage mit .NET

[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-check-out-samples-dotnet](../../../includes/storage-check-out-samples-dotnet.md)]

## <a name="overview"></a>Übersicht

Azure Queue Storage ermöglicht Cloud-Messaging zwischen Anwendungskomponenten. Bei der Entwicklung skalierbarer Anwendungen werden häufig einzelne Anwendungskomponenten entkoppelt, um eine unabhängige Skalierung zu ermöglichen. Queue Storage bietet asynchrones Messaging für die Kommunikation zwischen Anwendungskomponenten, egal ob diese in der Cloud, auf dem Desktop, auf einem lokalen Server oder einem mobilen Gerät ausgeführt werden. Queue Storage unterstützt auch die Verwaltung asynchroner Aufgaben und den Aufbau von Prozessworkflows.

### <a name="about-this-tutorial"></a>Informationen zu diesem Tutorial

In diesem Tutorial wird gezeigt, wie Sie .NET-Code für einige häufig verwendete Szenarien mit Azure Queue Storage schreiben. Zu den behandelten Szenarien zählen das Erstellen und Löschen von Warteschlangen sowie das Hinzufügen, Lesen und Löschen von Warteschlangennachrichten.

**Geschätzter Zeitaufwand:** 45 Minuten

### <a name="prerequisites"></a>Voraussetzungen

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)
* [Allgemeine Azure Storage-Clientbibliothek für .NET](https://www.nuget.org/packages/Microsoft.Azure.Storage.Common/)
* [Azure Storage Queue-Clientbibliothek für .NET](https://www.nuget.org/packages/Microsoft.Azure.Storage.Queue/)
* [Azure Configuration Manager für .NET](https://www.nuget.org/packages/Microsoft.Azure.ConfigurationManager/)
* Ein [Azure Storage-Konto](../common/storage-quickstart-create-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="set-up-your-development-environment"></a>Einrichten der Entwicklungsumgebung

Richten Sie als Nächstes Ihre Entwicklungsumgebung in Visual Studio ein, damit Sie die Codebeispiele dieser Anleitung ausprobieren können.

### <a name="create-a-windows-console-application-project"></a>Erstellen eines Windows-Konsolenanwendungsprojekts

Erstellen Sie in Visual Studio eine neue Windows-Konsolenanwendung. In den folgenden Schritten wird veranschaulicht, wie Sie eine Konsolenanwendung in Visual Studio 2019 erstellen. Die Schritte sind in anderen Versionen von Visual Studio ähnlich.

1. Wählen Sie **Datei** > **Neu** > **Projekt**.
2. Wählen Sie **Plattform** > **Windows** aus.
3. Wählen Sie **Konsolen-App (.NET Framework)** .
4. Wählen Sie **Weiter** aus.
5. Geben Sie in das Feld **Projektname** einen Namen für Ihre Anwendung ein.
6. Klicken Sie auf **Erstellen**

Alle Codebeispiele in diesem Tutorial können in der Datei **Program.cs** Ihrer Konsolenanwendung der **Main()** -Methode hinzugefügt werden.

Sie können die Azure Storage-Clientbibliotheken in jeder Art von .NET-Anwendung nutzen, z.B. einem Azure-Clouddienst oder einer Azure-Web-App, einer Desktopanwendung oder einer mobilen Anwendung. In diesem Leitfaden verwenden wir der Einfachheit halber eine Konsolenanwendung.

### <a name="use-nuget-to-install-the-required-packages"></a>Verwenden von NuGet zum Installieren der erforderlichen Pakete

Sie müssen in Ihrem Projekt auf die folgenden drei Pakete verweisen, um dieses Tutorial abzuschließen:

* [Allgemeine Microsoft Azure Storage-Clientbibliothek für .NET](https://www.nuget.org/packages/Microsoft.Azure.Storage.Common/): Mit diesem Paket erhalten Sie programmgesteuerten Zugriff auf Datenressourcen in Ihrem Speicherkonto.
* [Microsoft Azure Storage Queue-Bibliothek für .NET](https://www.nuget.org/packages/Microsoft.Azure.Storage.Queue/): Diese Clientbibliothek ermöglicht das Arbeiten mit dem Microsoft Azure Queue Storage-Dienst zum Speichern von Nachrichten, auf die ein Client möglicherweise zugreift.
* [Microsoft Azure Configuration Manager-Bibliothek für .NET](https://www.nuget.org/packages/Microsoft.Azure.ConfigurationManager/): Mit diesem Paket wird eine Klasse zum Analysieren einer Verbindungszeichenfolge in einer Konfigurationsdatei bereitgestellt. Dies gilt unabhängig davon, wo die Anwendung ausgeführt wird.

Sie können diese Pakete über NuGet abrufen. Folgen Sie diesen Schritten:

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **NuGet-Pakete verwalten**.
2. Wählen Sie **Durchsuchen**.
3. Suchen Sie online nach „Microsoft.Azure.Storage.Queue“, und wählen Sie **Installieren** aus, um die Storage-Clientbibliothek und dazugehörigen Abhängigkeiten zu installieren. Dabei wird auch die Bibliothek „Microsoft.Azure.Storage.Common“ installiert, die eine Abhängigkeit der Warteschlangenbibliothek ist.
4. Suchen Sie online nach „Microsoft.Azure.ConfigurationManager“, und wählen Sie **Installieren** aus, um Azure Configuration Manager zu installieren.

> [!NOTE]
> Das Storage-Clientbibliothekspakete sind auch im [Azure SDK für .NET](https://azure.microsoft.com/downloads/) enthalten. Wir empfehlen Ihnen jedoch die Installation der Storage-Clientbibliotheken über NuGet, um sicherzustellen, dass Sie stets über die aktuellen Versionen verfügen.
>
> Die ODataLib-Abhängigkeiten in den Storage-Clientbibliotheken für .NET werden von den ODataLib-Paketen aufgelöst, die auf NuGet verfügbar sind, und nicht von WCF Data Services. Die ODataLib-Bibliotheken können direkt heruntergeladen werden, oder es wird über Ihr Codeprojekt durch NuGet darauf verwiesen. Die spezifischen ODataLib-Pakete, die von den Storage-Clientbibliotheken verwendet werden, lauten [OData](https://nuget.org/packages/Microsoft.Data.OData/), [Edm](https://nuget.org/packages/Microsoft.Data.Edm/) und [Spatial](https://nuget.org/packages/System.Spatial/). Diese Bibliotheken werden von Azure-Tabellenspeicherklassen verwendet und sind erforderliche Abhängigkeiten für die Programmierung mit den Storage-Clientbibliotheken.

### <a name="determine-your-target-environment"></a>Bestimmen der Zielumgebung

Es gibt für die Ausführung der Beispiele in diesem Leitfaden zwei Umgebungsoptionen:

* Sie können den Code über ein Azure Storage-Konto in der Cloud ausführen.
* Sie können den Code über den Azure-Speicheremulator ausführen. Der Speicheremulator ist eine lokale Umgebung, in der ein Azure Storage-Konto in der Cloud emuliert wird. Der Emulator ist eine kostenlose Option zum Testen und Debuggen von Code, während sich Ihre Anwendung in der Entwicklung befindet. Für den Emulator wird ein bekanntes Konto mit Schlüssel verwendet. Weitere Informationen finden Sie unter [Einsatz des Azure-Speicheremulators für Entwicklung und Tests](../common/storage-use-emulator.md).

Wenn Sie ein Speicherkonto in der Cloud verwenden möchten, müssen Sie den primären Zugriffsschlüssel für Ihr Speicherkonto aus dem Azure-Portal kopieren. Weitere Informationen finden Sie unter [Zugriffsschlüssel](../common/storage-account-manage.md#access-keys).

> [!NOTE]
> Sie können den Speicheremulator als Ziel festlegen, um jegliche Kosten im Zusammenhang mit Azure Storage zu vermeiden. Wenn jedoch ein Azure-Speicherkonto in der Cloud das Ziel sein soll, sind die Kosten für das Ausführen dieses Lernprogramms vernachlässigbar.

### <a name="configure-your-storage-connection-string"></a>Konfigurieren der Speicherverbindungszeichenfolge

Die Azure Storage-Clientbibliotheken für .NET unterstützen die Verwendung einer Speicherverbindungszeichenfolge zum Konfigurieren von Endpunkten und Anmeldeinformationen für den Zugriff auf Speicherdienste. Am einfachsten lässt sich die Speicherverbindungszeichenfolge in einer Konfigurationsdatei verwalten.

Weitere Informationen zu Verbindungszeichenfolgen finden Sie unter [Konfigurieren von Verbindungszeichenfolgen für Azure Storage](../common/storage-configure-connection-string.md).

> [!NOTE]
> Ihr Speicherkontoschlüssel ähnelt dem Stammkennwort für das Speicherkonto. Achten Sie darauf, den Speicherkontoschlüssel immer gut zu schützen. Geben Sie ihn nicht an andere Benutzer weiter, vermeiden Sie das Hartcodieren, und speichern Sie ihn nicht in einer Klartextdatei, auf die andere Benutzer zugreifen können. Generieren Sie Ihren Schlüssel mithilfe des Azure-Portals neu, wenn Sie der Meinung sind, dass er nicht mehr sicher ist.

Öffnen Sie zum Konfigurieren der Verbindungszeichenfolge in Visual Studio im Projektmappen-Explorer die Datei **app.config**. Fügen Sie den Inhalt des Elements **\<appSettings\>** (wie nachstehend gezeigt) hinzu. Ersetzen Sie *account-name* durch den Namen Ihres Speicherkonto und *account-key* durch Ihren Kontozugriffsschlüssel:

```xml
<configuration>
    <startup>
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
    </startup>
    <appSettings>
        <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key" />
    </appSettings>
</configuration>
```

Ihre Konfigurationseinstellung kann beispielsweise wie folgt lauten:

```xml
<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=GMuzNHjlB3S9itqZJHHCnRkrokLkcSyW7yK9BRbGp0ENePunLPwBgpxV1Z/pVo9zpem/2xSHXkMqTHHLcx8XRA==" />
```

Wenn Sie den Speicheremulator verwenden möchten, können Sie eine Verknüpfung nutzen, mit der eine Zuordnung zum bekannten Kontonamen und dem Schlüssel hergestellt wird. In diesem Fall lautet die Verbindungszeichenfolge wie folgt:

```xml
<add key="StorageConnectionString" value="UseDevelopmentStorage=true;" />
```

### <a name="add-using-directives"></a>Hinzufügen von using-Direktiven

Fügen Sie am Anfang der Datei `Program.cs` die folgende `using`-Anweisung hinzu:

```csharp
using Microsoft.Azure; // Namespace for CloudConfigurationManager
using Microsoft.Azure.Storage; // Namespace for CloudStorageAccount
using Microsoft.Azure.Storage.Queue; // Namespace for Queue storage types
```

### <a name="copy-your-credentials-from-the-azure-portal"></a>Kopieren Ihrer Anmeldeinformationen aus dem Azure-Portal

Der Beispielcode muss den Zugriff auf Ihr Speicherkonto autorisieren. Zur Autorisierung stellen Sie der Anwendung die Anmeldeinformationen für Ihr Speicherkonto in Form einer Verbindungszeichenfolge bereit. So zeigen Sie die Anmeldeinformationen für Ihr Speicherkonto an:

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com).
2. Suchen Sie nach Ihrem Speicherkonto.
3. Wählen Sie im Abschnitt **Einstellungen** der Speicherkontoübersicht die Option **Zugriffsschlüssel**. Daraufhin werden Ihre Zugriffsschlüssel zusammen mit der jeweiligen vollständigen Verbindungszeichenfolge angezeigt.
4. Suchen Sie unter **key1** nach dem Wert für die **Verbindungszeichenfolge**, und klicken Sie dann auf die Schaltfläche **Kopieren**, um die Verbindungszeichenfolge zu kopieren. Der Wert der Verbindungszeichenfolge wird in einem späteren Schritt einer Umgebungsvariablen hinzugefügt.

    ![Screenshot: Kopieren einer Verbindungszeichenfolge aus dem Azure-Portal](media/storage-dotnet-how-to-use-queues/portal-connection-string.png)

### <a name="parse-the-connection-string"></a>Analysieren der Verbindungszeichenfolge

[!INCLUDE [storage-cloud-configuration-manager-include](../../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-queue-service-client"></a>Erstellen des Warteschlangendienstclients

Mit der [CloudQueueClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.queue.cloudqueueclient?view=azure-dotnet) -Klasse können Sie im Warteschlangenspeicher gespeicherte Warteschlangen abrufen. Hier sehen Sie eine Möglichkeit zum Erstellen des Dienstclients:

```csharp
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
```

Jetzt können Sie Code schreiben, der Daten aus dem Warteschlangenspeicher liest und Daten in den Warteschlangenspeicher schreibt.

## <a name="create-a-queue"></a>Erstellen einer Warteschlange

Dieses Beispiel zeigt, wie Sie eine Warteschlange erstellen, falls sie nicht bereits vorhanden ist:

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a container.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Create the queue if it doesn't already exist
queue.CreateIfNotExists();
```

## <a name="insert-a-message-into-a-queue"></a>Einfügen einer Nachricht in eine Warteschlange

Um eine Nachricht in eine vorhandene Warteschlange einzufügen, erstellen Sie zuerst ein neues [CloudQueueMessage](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.queue.cloudqueuemessage?view=azure-dotnet)-Objekt. Anschließend rufen Sie die [AddMessage](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.queue.cloudqueue.addmessage?view=azure-dotnet) -Methode auf. Eine **CloudQueueMessage** kann entweder aus einer Zeichenfolge (im UTF-8-Format) oder aus einem **Bytearray** erstellt werden. Dieser Code erstellte eine Warteschlange (wenn sie noch nicht vorhanden ist) und fügt die Nachricht "Hello, World" ein:

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Create the queue if it doesn't already exist.
queue.CreateIfNotExists();

// Create a message and add it to the queue.
CloudQueueMessage message = new CloudQueueMessage("Hello, World");
queue.AddMessage(message);
```

## <a name="peek-at-the-next-message"></a>Einsehen der nächsten Nachricht

Sie können einen Blick auf die Nachricht am Anfang einer Warteschlange werfen, ohne sie aus der Warteschlange zu entfernen, indem Sie die Methode [PeekMessage](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.queue.cloudqueue.peekmessage?view=azure-dotnet) aufrufen.

```csharp
// Retrieve storage account from connection string
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Peek at the next message
CloudQueueMessage peekedMessage = queue.PeekMessage();

// Display message.
Console.WriteLine(peekedMessage.AsString);
```

## <a name="change-the-contents-of-a-queued-message"></a>Ändern des Inhalts von Nachrichten in der Warteschlange

Sie können den Inhalt einer Nachricht vor Ort in der Warteschlange ändern. Wenn die Nachricht eine Arbeitsaufgabe darstellt, können Sie diese Funktion verwenden, um den Status der Aufgabe zu aktualisieren. Mit dem folgenden Code wird die Warteschlangennachricht mit neuem Inhalt aktualisiert und das Sichtbarkeits-Zeitlimit um weitere 60 Sekunden verlängert. Dadurch wird der mit der Nachricht verknüpfte Arbeitsstatus gespeichert, und der Client erhält eine weitere Minute zur Bearbeitung der Nachricht. Sie können diese Technik verwenden, um Workflows mit mehreren Schritten in Warteschlangennachrichten zu verfolgen, ohne von vorn beginnen zu müssen, wenn ein Verarbeitungsschritt aufgrund eines Hardware- oder Softwarefehlers fehlschlägt. In der Regel behalten Sie auch die Anzahl der Wiederholungen bei, und wenn die Nachricht mehr als *n* Mal wiederholt wurde, wird sie gelöscht. Dies verhindert, dass eine Nachricht bei jeder Verarbeitung einen Anwendungsfehler auslöst.

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Get the message from the queue and update the message contents.
CloudQueueMessage message = queue.GetMessage();
message.SetMessageContent2("Updated contents.", false);
queue.UpdateMessage(message,
    TimeSpan.FromSeconds(60.0),  // Make it invisible for another 60 seconds.
    MessageUpdateFields.Content | MessageUpdateFields.Visibility);
```

## <a name="de-queue-the-next-message"></a>Entfernen der nächsten Nachricht aus der Warteschlange

Dieser Code entfernt eine Nachricht in zwei Schritten aus der Warteschlange. Wenn Sie [GetMessage](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.queue.cloudqueue.getmessage?view=azure-dotnet)aufrufen, wird die nächste Nachricht aus der Warteschlange abgerufen. Die für **GetMessage** zurückgegebene Nachricht ist für andere Codes nicht mehr sichtbar, die Nachrichten aus dieser Warteschlange lesen. Standardmäßig bleibt die Nachricht 30 Sekunden lang unsichtbar. Um die Nachricht endgültig aus der Warteschlange zu entfernen, müssen Sie außerdem [DeleteMessage](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.queue.cloudqueue.deletemessage?view=azure-dotnet)aufrufen. Dieser zweistufige Prozess zum Entfernen von Nachrichten stellt sicher, dass eine andere Codeinstanz dieselbe Nachricht erneut abrufen kann, falls die Verarbeitung aufgrund eines Hardware- oder Softwarefehlers fehlschlägt. Der Code ruft **DeleteMessage** direkt nach der Verarbeitung der Nachricht auf.

```csharp
// Retrieve storage account from connection string
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Get the next message
CloudQueueMessage retrievedMessage = queue.GetMessage();

//Process the message in less than 30 seconds, and then delete the message
queue.DeleteMessage(retrievedMessage);
```

## <a name="use-async-await-pattern-with-common-queue-storage-apis"></a>Verwenden des Async-Await-Musters mit allgemeinen Warteschlangenspeicher-APIs

In diesem Beispiel wird veranschaulicht, wie das Async-Await-Muster mit allgemeinen Warteschlangenspeicher-APIs verwendet wird. Im Beispiel werden jeweils die asynchronen Versionen der angegebenen Methoden aufgerufen, wie am Suffix *Async* der einzelnen Methoden erkennbar. Wenn eine asynchrone Methode verwendet wird, hält das Async-Await-Muster die lokale Ausführung an, bis der Aufruf abgeschlossen ist. Durch dieses Verhalten kann der aktuelle Thread eine andere Aktion ausführen, wodurch Leistungsengpässe vermieden werden und die allgemeine Reaktionsfähigkeit der Anwendung verbessert wird. Weitere Informationen zur Verwendung des Async-Await-Musters in .NET finden Sie unter [Async und Await (C# und Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)

```csharp
// Create the queue if it doesn't already exist
if(await queue.CreateIfNotExistsAsync())
{
    Console.WriteLine("Queue '{0}' Created", queue.Name);
}
else
{
    Console.WriteLine("Queue '{0}' Exists", queue.Name);
}

// Create a message to put in the queue
CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

// Async enqueue the message
await queue.AddMessageAsync(cloudQueueMessage);
Console.WriteLine("Message added");

// Async dequeue the message
CloudQueueMessage retrievedMessage = await queue.GetMessageAsync();
Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

// Async delete the message
await queue.DeleteMessageAsync(retrievedMessage);
Console.WriteLine("Deleted message");
```

## <a name="leverage-additional-options-for-de-queuing-messages"></a>Nutzen zusätzlicher Optionen für das Entfernen von Nachrichten aus der Warteschlange

Es gibt zwei Möglichkeiten, wie Sie das Abrufen von Nachrichten aus der Warteschlange anpassen können. Erstens können Sie einen Nachrichtenstapel abrufen (bis zu 32). Zweitens können Sie das Unsichtbarkeits-Zeitlimit verkürzen oder verlängern, sodass der Code mehr oder weniger Zeit zur vollständigen Verarbeitung jeder Nachricht benötigt. Im folgenden Codebeispiel wird [GetMessages](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.queue.cloudqueue.getmessages?view=azure-dotnet) verwendet, um 20 Nachrichten mit einem Aufruf abzurufen. Anschließend wird jede Nachricht mithilfe einer **foreach** -Schleife verarbeitet. Außerdem wird das Unsichtbarkeits-Zeitlimit auf fünf Minuten pro Nachricht festgelegt. Beachten Sie, dass die 5 Minuten für alle Nachrichten gleichzeitig beginnen, sodass 5 Minuten nach dem Aufruf von **GetMessages**alle Nachrichten, die nicht gelöscht wurden, wieder sichtbar werden.

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

foreach (CloudQueueMessage message in queue.GetMessages(20, TimeSpan.FromMinutes(5)))
{
    // Process all messages in less than 5 minutes, deleting each message after processing.
    queue.DeleteMessage(message);
}
```

## <a name="get-the-queue-length"></a>Abrufen der Warteschlangenlänge

Sie können die Anzahl der Nachrichten in einer Warteschlange schätzen lassen. Die Methode [FetchAttributes](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.queue.cloudqueue.fetchattributes?view=azure-dotnet) fordert den Warteschlangendienst auf, die Warteschlangenattribute einschließlich der Nachrichtenanzahl abzurufen. Die [ApproximateMessageCount](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.queue.cloudqueue.approximatemessagecount?view=azure-dotnet)-Eigenschaft gibt den letzten von der **FetchAttributes**-Methode abgerufenen Wert zurück, ohne den Warteschlangendienst aufzurufen.

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Fetch the queue attributes.
queue.FetchAttributes();

// Retrieve the cached approximate message count.
int? cachedMessageCount = queue.ApproximateMessageCount;

// Display number of messages.
Console.WriteLine("Number of messages in queue: " + cachedMessageCount);
```

## <a name="delete-a-queue"></a>Löschen einer Warteschlange

Zum Löschen einer Warteschlange und aller darin enthaltenen Nachrichten rufen Sie die Methode [Delete](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.queue.cloudqueue.delete?view=azure-dotnet) für das Warteschlangenobjekt auf.

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Delete the queue.
queue.Delete();
```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich nun mit den Grundlagen des Warteschlangenspeichers vertraut gemacht haben, folgen Sie diesen Links, um zu erfahren, wie komplexere Speicheraufgaben ausgeführt werden.

* Vollständige Informationen zu verfügbaren APIs finden Sie in der Warteschlangendienst-Referenzdokumentation:
  * [Referenz zur Storage-Clientbibliothek für .NET](https://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
  * [REST-API-Referenz](https://msdn.microsoft.com/library/azure/dd179355)
* Erfahren Sie, wie Sie mithilfe des [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/wiki)den geschriebenen Code so vereinfachen, dass er mit Azure Storage funktioniert.
* Weitere Informationen zu zusätzlichen Optionen für das Speichern von Daten in Azure finden Sie in den anderen Featureleitfäden.
  * [Erste Schritte mit Azure Table Storage mit .NET](../../cosmos-db/table-storage-how-to-use-dotnet.md) zum Speichern strukturierter Daten
  * [Erste Schritte mit Azure Blob Storage mit .NET](../blobs/storage-dotnet-how-to-use-blobs.md) zum Speichern unstrukturierter Daten
  * Informationen zum Speichern relationaler Daten finden Sie unter [Herstellen von Verbindungen mit SQL-Datenbank mithilfe von .NET (C#)](../../sql-database/sql-database-connect-query-dotnet-core.md).

[Download and install the Azure SDK for .NET]: /develop/net/
[.NET client library reference]: https://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
[Creating an Azure Project in Visual Studio]: https://msdn.microsoft.com/library/azure/ee405487.aspx
[Azure Storage Team Blog]: https://blogs.msdn.com/b/windowsazurestorage/
[OData]: https://nuget.org/packages/Microsoft.Data.OData/5.0.2
[Edm]: https://nuget.org/packages/Microsoft.Data.Edm/5.0.2
[Spatial]: https://nuget.org/packages/System.Spatial/5.0.2
