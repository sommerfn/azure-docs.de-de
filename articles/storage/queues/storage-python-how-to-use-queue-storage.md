---
title: 'Verwenden von Azure Queue Storage mit Python: Azure Storage'
description: Erfahren Sie, wie Sie mit dem Azure-Warteschlangendienst mit Python Warteschlangen erstellen und löschen sowie Nachrichten einfügen, abrufen und löschen können.
author: mhopkins-msft
ms.service: storage
ms.author: mhopkins
ms.date: 09/17/2019
ms.subservice: queues
ms.topic: conceptual
ms.reviewer: cbrooks
ms.custom: seo-javascript-october2019
ms.openlocfilehash: 242ba7dbe4bfcc003899e95e76dc57d809dbc95a
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72428008"
---
# <a name="how-to-use-azure-queue-storage-from-python"></a>Verwenden von Azure Queue Storage mit Python

[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

In diesem Artikel werden häufige Szenarien für die Verwendung des Azure Queue Storage-Diensts veranschaulicht. Zu den Szenarien gehören das Einfügen, Einsehen, Abrufen und Löschen von Warteschlangennachrichten sowie das Erstellen und Löschen von Warteschlangen.

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Übersicht

Die Beispiele in diesem Artikel sind in Python geschrieben. Darüber hinaus wird in den Beispielen das [Microsoft Azure Storage-SDK für Python] verwendet. Weitere Informationen zu Warteschlangen finden Sie im Abschnitt [Nächste Schritte](#next-steps).

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="download-and-install-azure-storage-sdk-for-python"></a>Herunterladen und Installieren des Azure Storage SDKs für Python

Das [Azure Storage SDK für Python](https://github.com/azure/azure-storage-python) erfordert Python Version 2.7, 3.3 oder höher.
 
### <a name="install-via-pypi"></a>Installation über PyPI

Geben Sie für die Installation über Python Package Index (PyPI) folgenden Befehl ein:

```bash
pip install azure-storage-queue
```

> [!NOTE]
> Wenn Sie ein Upgrade aus dem Azure Storage SDK für Python Version 0.36 oder früher vornehmen, deinstallieren Sie das ältere SDK mit `pip uninstall azure-storage`, bevor Sie das neueste Paket installieren.

Informationen zu alternativen Installationsmethoden finden Sie unter [Azure Storage SDK für Python](https://github.com/Azure/azure-storage-python/).

## <a name="view-the-sample-application"></a>Anzeigen der Beispielanwendung

Um eine Beispielanwendung anzuzeigen und auszuführen, die zeigt, wie Python mit Azure Queues verwendet wird, finden Sie unter [Azure Storage: Erste Schritte mit Azure-Warteschlangen in Python](https://github.com/Azure-Samples/storage-queue-python-getting-started). 

Um die Beispielanwendung auszuführen, stellen Sie sicher, dass Sie die `azure-storage-queue`- und `azure-storage-common`-Pakete beide installiert haben.

## <a name="create-a-queue"></a>Erstellen einer Warteschlange

Das Objekt [QueueService](/python/api/azure-storage-queue/azure.storage.queue.queueservice.queueservice) ermöglicht es Ihnen, mit Warteschlangen zu arbeiten. Der folgende Code erstellt ein `QueueService`-Objekt. Fügen Sie am Anfang jeder Python-Datei, in der Sie programmgesteuert auf Azure-Speicher zugreifen möchten, den folgenden Code hinzu:

```python
from azure.storage.queue import QueueService
```

Mit dem folgenden Code wird unter Verwendung des Speicherkontonamens und Kontoschlüssels ein `QueueService`-Objekt erstellt. Ersetzen Sie *myaccount* und *mykey* durch Ihren Kontonamen und Schlüssel.

```python
queue_service = QueueService(account_name='myaccount', account_key='mykey')

queue_service.create_queue('taskqueue')
```

## <a name="insert-a-message-into-a-queue"></a>Einfügen einer Nachricht in eine Warteschlange

Zum Einfügen einer Nachricht in eine Warteschlange verwenden Sie die [put_message](/python/api/azure-storage-queue/azure.storage.queue.queueservice.queueservice#put-message-queue-name--content--visibility-timeout-none--time-to-live-none--timeout-none-)-Methode, um eine neue Nachricht zu erstellen und zur Warteschlange hinzuzufügen.

```python
queue_service.put_message('taskqueue', u'Hello World')
```

Azure-Warteschlangennachrichten werden als Text gespeichert. Wenn Sie Binärdaten speichern möchten, richten Sie die Base64-Codierungs- und -Decodierungsfunktionen für das Warteschlangendienstobjekt ein, bevor Sie eine Nachricht in die Warteschlange stellen.

```python
# setup queue Base64 encoding and decoding functions
queue_service.encode_function = QueueMessageFormat.binary_base64encode
queue_service.decode_function = QueueMessageFormat.binary_base64decode
```

## <a name="peek-at-the-next-message"></a>Einsehen der nächsten Nachricht

Sie können einen Blick auf die Nachricht am Anfang einer Warteschlange werfen, ohne sie aus der Warteschlange zu entfernen, indem Sie die [peek_messages](/python/api/azure-storage-queue/azure.storage.queue.queueservice.queueservice#peek-messages-queue-name--num-messages-none--timeout-none-)-Methode aufrufen. Standardmäßig wird von `peek_messages` jeweils nur eine Nachricht angeschaut.

```python
messages = queue_service.peek_messages('taskqueue')
for message in messages:
    print(message.content)
```

## <a name="dequeue-messages"></a>Entfernen von Nachrichten aus Warteschlangen

Dieser Code entfernt eine Nachricht in zwei Schritten aus der Warteschlange. Wenn Sie [get_messages](/python/api/azure-storage-queue/azure.storage.queue.queueservice.queueservice#get-messages-queue-name--num-messages-none--visibility-timeout-none--timeout-none-) aufrufen, wird standardmäßig die nächste Nachricht aus der Warteschlange abgerufen. Die für `get_messages` zurückgegebene Nachricht ist für anderen Code, mit dem Nachrichten aus dieser Warteschlange gelesen werden, nicht mehr sichtbar. Standardmäßig bleibt die Nachricht 30 Sekunden lang unsichtbar. Um die Nachricht endgültig aus der Warteschlange zu entfernen, müssen Sie außerdem [delete_message](/python/api/azure-storage-queue/azure.storage.queue.queueservice.queueservice#delete-message-queue-name--message-id--pop-receipt--timeout-none-) aufrufen. Dieser zweistufige Prozess zum Entfernen von Nachrichten stellt sicher, dass eine andere Codeinstanz dieselbe Nachricht erneut abrufen kann, falls die Verarbeitung aufgrund eines Hardware- oder Softwarefehlers fehlschlägt. In Ihrem Code wird `delete_message` direkt nach der Verarbeitung der Nachricht aufgerufen.

```python
messages = queue_service.get_messages('taskqueue')
for message in messages:
    print(message.content)
    queue_service.delete_message('taskqueue', message.id, message.pop_receipt)
```

Es gibt zwei Möglichkeiten, wie Sie das Abrufen von Nachrichten aus der Warteschlange anpassen können. Erstens können Sie einen Nachrichtenstapel abrufen (bis zu 32). Zweitens können Sie das Unsichtbarkeits-Zeitlimit verkürzen oder verlängern, sodass der Code mehr oder weniger Zeit zur vollständigen Verarbeitung jeder Nachricht benötigt. Im folgenden Codebeispiel wird `get_messages` verwendet, um 16 Nachrichten mit einem Aufruf abzurufen. Anschließend wird jede Nachricht mithilfe einer for Schleife verarbeitet. Außerdem wird das Unsichtbarkeits-Zeitlimit auf fünf Minuten pro Nachricht festgelegt.

```python
messages = queue_service.get_messages(
    'taskqueue', num_messages=16, visibility_timeout=5*60)
for message in messages:
    print(message.content)
    queue_service.delete_message('taskqueue', message.id, message.pop_receipt)
```

## <a name="change-the-contents-of-a-queued-message"></a>Ändern des Inhalts von Nachrichten in der Warteschlange

Sie können den Inhalt einer Nachricht vor Ort in der Warteschlange ändern. Wenn die Nachricht eine Arbeitsaufgabe darstellt, können Sie diese Funktion verwenden, um den Status der Aufgabe zu aktualisieren. Der folgende Code verwendet die [update_message](/python/api/azure-storage-queue/azure.storage.queue.queueservice.queueservice#update-message-queue-name--message-id--pop-receipt--visibility-timeout--content-none--timeout-none-)-Methode zum Aktualisieren einer Nachricht. Das Sichtbarkeitstimeout ist auf 0 festgelegt, d. h., die Nachricht wird sofort angezeigt, und der Inhalt wird aktualisiert.

```python
messages = queue_service.get_messages('taskqueue')
for message in messages:
    queue_service.update_message(
        'taskqueue', message.id, message.pop_receipt, 0, u'Hello World Again')
```

## <a name="get-the-queue-length"></a>Abrufen der Warteschlangenlänge

Sie können die Anzahl der Nachrichten in einer Warteschlange schätzen lassen. Die Methode [get_queue_metadata](/python/api/azure-storage-queue/azure.storage.queue.queueservice.queueservice#get-queue-metadata-queue-name--timeout-none-) ruft Metadaten zur Warteschlange und zu `approximate_message_count` vom Warteschlangendienst ab. Die Anzahl ist nur ein ungefährer Wert, da seit der Antwort des Warteschlangendienstes möglicherweise bereits Nachrichten hinzugefügt oder gelöscht wurden.

```python
metadata = queue_service.get_queue_metadata('taskqueue')
count = metadata.approximate_message_count
```

## <a name="delete-a-queue"></a>Löschen einer Warteschlange

Zum Löschen einer Warteschlange und aller darin enthaltenen Nachrichten rufen Sie die [delete_queue](/python/api/azure-storage-queue/azure.storage.queue.queueservice.queueservice#delete-queue-queue-name--fail-not-exist-false--timeout-none-)-Methode auf.

```python
queue_service.delete_queue('taskqueue')
```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich nun mit den Grundlagen von Queue Storage vertraut gemacht haben, lesen Sie die folgenden Artikel, um mehr zu erfahren.

* [API-Referenz zu Azure-Warteschlangen für Python](/python/api/azure-storage-queue)
* [Python Developer Center](https://azure.microsoft.com/develop/python/)
* [REST-API für Azure-Speicherdienste](https://msdn.microsoft.com/library/azure/dd179355)

[Azure Storage Team Blog]: https://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure Storage-SDK für Python]: https://github.com/Azure/azure-storage-python
