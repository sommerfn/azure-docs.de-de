---
title: Reagieren auf Azure Blob Storage-Ereignisse | Microsoft-Dokumentation
description: Abonnieren Sie Blob Storage-Ereignisse mit Azure Event Grid.
author: normesta
ms.author: normesta
ms.date: 01/30/2018
ms.topic: conceptual
ms.service: storage
ms.subservice: blobs
ms.reviewer: cbrooks
ms.openlocfilehash: 13eef9beb6c86683c56efc744dc42b4614b84fe9
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/10/2019
ms.locfileid: "68946482"
---
# <a name="reacting-to-blob-storage-events"></a>Reaktion auf Blob Storage-Ereignisse

Azure Storage-Ereignisse bieten Anwendungen die Möglichkeit, auf Ereignisse wie das Erstellen und Löschen von Blobs mithilfe moderner serverloser Architekturen zu reagieren. Dies geschieht ohne komplizierten Code oder teure und ineffiziente Abrufdienste.

Stattdessen werden Ereignisse über [Azure Event Grid](https://azure.microsoft.com/services/event-grid/) an Abonnenten wie Azure Functions, Azure Logic Apps oder sogar Ihren eigenen benutzerdefinierten HTTP-Listener gepusht, während Sie nur für das zahlen, was Sie tatsächliche nutzen.

Blob Storage-Ereignisse werden an den Event Grid-Dienst gesendet, der dank umfangreicher Wiederholungsrichtlinien und der Zustellung unzustellbarer Nachrichten zuverlässige Zustelldienste für Ihre Anwendungen bietet.

Gängige Blob Storage-Ereignisszenarien enthalten Bild- oder Videobearbeitung, Suchindizierung oder dateiorientierte Workflows. Asynchrone Dateiuploads eignen sich hervorragend für Ereignisse. Wenn Änderungen selten sind, aber Ihr Szenario die sofortige Reaktion erfordert, kann die ereignisbasierte Architektur besonders effizient sein.

Wenn Sie diese jetzt ausprobieren möchten, informieren Sie sich in folgenden Schnellstartartikeln:

|Gewünschtes Tool:    |In diesem Artikel finden Sie weitere Informationen: |
|--|-|
|Azure-Portal    |[Schnellstart: Weiterleiten von Blob Storage-Ereignissen an einen Webendpunkt über das Azure-Portal](https://docs.microsoft.com/azure/event-grid/blob-event-quickstart-portal?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|PowerShell    |[Schnellstart: Weiterleiten von Speicherereignissen an einen Webendpunkt per PowerShell](https://docs.microsoft.com/azure/storage/blobs/storage-blob-event-quickstart-powershell?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Azure-Befehlszeilenschnittstelle    |[Schnellstart: Weiterleiten von Speicherereignissen an einen Webendpunkt per Azure CLI](https://docs.microsoft.com/azure/storage/blobs/storage-blob-event-quickstart?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|

## <a name="the-event-model"></a>Ereignismodell

Event Grid verwendet [Ereignisabonnements](../../event-grid/concepts.md#event-subscriptions) zum Weiterleiten von Ereignisnachrichten an Abonnenten. Diese Abbildung veranschaulicht die Beziehung zwischen Ereignisherausgebern, Ereignisabonnements und Ereignishandlern.

![Event Grid-Modell](./media/storage-blob-event-overview/event-grid-functional-model.png)

Abonnieren Sie zunächst einen Endpunkt für ein Ereignis. Sobald ein Ereignis ausgelöst wird, sendet der Event Grid-Dienst Daten zum Ereignis an den Endpunkt.

Im Artikel zum [Blob Storage-Ereignisschema](../../event-grid/event-schema-blob-storage.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) finden Sie:

> [!div class="checklist"]
> * Eine vollständige Liste der Blob Storage-Ereignisse und Informationen dazu, wie jedes Ereignis ausgelöst wird
> * Ein Beispiel dafür, welche Daten von Event Grid zu den einzelnen Ereignissen gesendet werden
> * Angaben zum Zweck der jeweiligen Schlüssel-Wert-Paare, die in den Daten enthalten sind

## <a name="filtering-events"></a>Filtern von Ereignissen

Blobereignisabonnements können basierend auf dem Ereignistyp sowie auf dem Container- und Blobnamen des Objekts, das erstellt oder gelöscht wurde, gefiltert werden.  Auf Ereignisabonnements können während der [Erstellung](/cli/azure/eventgrid/event-subscription?view=azure-cli-latest) des Ereignisabonnements oder [zu einem späteren Zeitpunkt](/cli/azure/eventgrid/event-subscription?view=azure-cli-latest) Filter angewendet werden. Betrefffilter funktionieren in Event Grid auf Grundlage von „beginnt mit“- und „endet mit“-Übereinstimmungen, sodass Ereignisse mit einem übereinstimmenden Betreff an den Abonnenten übermittelt werden.

Weitere Informationen zum Anwenden von Filtern finden Sie unter [Filtern von Ereignissen für Event Grid](https://docs.microsoft.com/azure/event-grid/how-to-filter-events).

Der Betreff von Blob Storage-Ereignissen verwendet das Format:

```
/blobServices/default/containers/<containername>/blobs/<blobname>
```

Zur Übereinstimmung mit allen Ereignissen für ein Speicherkonto können Sie den Betrefffilter leer lassen.

Verwenden Sie zur Übereinstimmung mit Ereignissen aus Blobs, die in einem Satz von Containern erstellt wurden und ein gemeinsames Präfix verwenden, einen `subjectBeginsWith`-Filter wie:

```
/blobServices/default/containers/containerprefix
```

Verwenden Sie zur Übereinstimmung mit Ereignissen aus Blobs, die in einem bestimmten Container erstellt wurden, einen `subjectBeginsWith`-Filter wie:

```
/blobServices/default/containers/containername/
```

Verwenden Sie zur Übereinstimmung mit Ereignissen aus Blobs, die in einem bestimmten Container erstellt wurden und ein gemeinsames Blobnamenspräfix verwenden, einen `subjectBeginsWith`-Filter wie:

```
/blobServices/default/containers/containername/blobs/blobprefix
```

Verwenden Sie zur Übereinstimmung mit Ereignissen aus Blobs, die in einem bestimmten Container erstellt wurden und ein gemeinsames Blobsuffix verwenden, einen `subjectEndsWith`-Filter wie „.log“ oder „.jpg“. Weitere Informationen finden Sie unter [Begriffe in Azure Event Grid](../../event-grid/concepts.md#event-subscriptions).

## <a name="practices-for-consuming-events"></a>Methoden zum Nutzen von Ereignissen

Anwendungen, die Blob Storage-Ereignisse behandeln, sollten einige bewährte Methoden nutzen:
> [!div class="checklist"]
> * Da mehrere Abonnements zum Weiterleiten von Ereignissen an den gleichen Ereignishandler konfiguriert werden können, ist es wichtig, nicht davon auszugehen, dass Ereignisse aus einer bestimmten Quelle stammen, sondern das Thema der Nachricht zu überprüfen, um sicherzustellen, dass es aus dem Speicherkonto stammt, das Sie erwarten.
> * Überprüfen Sie auf ähnliche Weise, ob Sie auf die Verarbeitung des eventType vorbereitet sind, und gehen Sie nicht davon aus, dass alle Ereignisse, die Sie empfangen, den von Ihnen erwarteten Typen entsprechen.
> * Da Nachrichten in falscher Reihenfolge und mit Verzögerung eintreffen können, verwenden Sie die etag-Felder, um zu verstehen, ob Ihre Informationen zu Objekten weiterhin auf dem neuesten Stand ist.  Verwenden Sie auch die sequencer-Felder, um die Reihenfolge der Ereignisse für ein bestimmtes Objekt zu verstehen.
> * Verwenden Sie das blobType-Feld, um zu verstehen, welche Arten von Vorgängen für das Blob zulässig sind, und welche Typen von Clientbibliotheken Sie für den Zugriff auf das Blob verwenden sollten. Gültige Werte sind `BlockBlob` oder `PageBlob`. 
> * Verwenden Sie das url-Feld mit `CloudBlockBlob`- und `CloudAppendBlob`-Konstruktor für den Zugriff auf das Blob.
> * Ignorieren Sie Felder, die Sie nicht verstehen. So müssen Sie sich nicht mit neuen Features auseinandersetzen, die in der Zukunft hinzugefügt werden könnten.
> * Wenn Sie sicherstellen möchten, dass das **Microsoft.Storage.BlobCreated**-Ereignis nur ausgelöst wird, nachdem ein Blockblob vollständig committed wurde, filtern Sie das Ereignis nach den REST-API-Aufrufen `CopyBlob`, `PutBlob`, `PutBlockList` oder `FlushWithClose`. Diese API-Aufrufe lösen das **Microsoft.Storage.BlobCreated**-Ereignis erst aus, nachdem Daten vollständig in einem Blockblob committed wurden. Informationen zum Erstellen eines Filters finden Sie unter [Filtern von Ereignissen für Event Grid](https://docs.microsoft.com/azure/event-grid/how-to-filter-events).


## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über Event Grid , und probieren Sie Blob Storage-Ereignisse aus:

- [Einführung in Azure Event Grid](../../event-grid/overview.md)
- [Weiterleiten von Blob Storage-Ereignissen an einen benutzerdefinierten Webendpunkt](storage-blob-event-quickstart.md)
