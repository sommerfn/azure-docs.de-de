---
title: 'Senden und Empfangen von Ereignissen mit Python: Azure Event Hubs'
description: Diese exemplarische Vorgehensweise zeigt, wie Sie Python-Skripts erstellen und ausführen, die Ereignisse an Azure Event Hubs senden oder von Azure Event Hubs empfangen.
services: event-hubs
author: ShubhaVijayasarathy
manager: femila
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 10/11/2019
ms.author: shvija
ms.openlocfilehash: 330a7f5dc325c707b5be7ce9f9b3242a1d4c9547
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72428884"
---
# <a name="send-and-receive-events-with-event-hubs-using-python"></a>Senden und Empfangen von Ereignissen mit Azure Event Hubs mithilfe von Python

Azure Event Hubs ist eine Big Data-Streamingplattform und ein Ereigniserfassungsdienst, der Millionen von Ereignissen pro Sekunde empfangen und verarbeiten kann. Event Hubs kann Ereignisse, Daten oder Telemetriedaten von verteilter Software und verteilten Geräten verarbeiten und speichern. An einen Event Hub gesendete Daten können transformiert und mit einem beliebigen Echtzeitanalyse-Anbieter oder Batchverarbeitungs-/Speicheradapter gespeichert werden. Weitere Informationen zu Event Hubs finden Sie unter [Azure Event Hubs](event-hubs-about.md) und [Features und Terminologie in Azure Event Hubs](event-hubs-features.md).

Dieser Schnellstart zeigt, wie Sie Python-Anwendungen erstellen, die Ereignisse an einen Event Hub senden und von diesem empfangen. 

> [!NOTE]
> Anstatt den Schnellstart durchzuarbeiten, können Sie auch die [Beispiel-Apps](https://github.com/Azure/azure-event-hubs-python/tree/master/examples) von GitHub herunterladen und ausführen. Ersetzen Sie die Zeichenfolgen `EventHubConnectionString` und `EventHubName` durch Ihre Event Hub-Werte. 

## <a name="prerequisites"></a>Voraussetzungen

Zum Durchführen dieser Schnellstartanleitung benötigen Sie Folgendes:

- Ein Azure-Abonnement. Falls Sie kein Abonnement besitzen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/), bevor Sie beginnen.
- Einen aktiven Event Hubs-Namespace und Event Hub, erstellt durch Befolgen der Anweisungen unter [Schnellstart: Erstellen eines Event Hubs mithilfe des Azure-Portals](event-hubs-create.md). Notieren Sie sich die Namen des Namespace und Event Hub, weil sie später in dieser exemplarischen Vorgehensweise verwendet werden. 
- Den Namen des freigegebenen Zugriffsschlüssels und den Wert des Primärschlüssels für Ihren Event Hubs-Namespace. Rufen Sie den Namen und den Wert des Zugriffsschlüssels ab, indem Sie die Anweisungen unter [Abrufen der Verbindungszeichenfolge](event-hubs-get-connection-string.md#get-connection-string-from-the-portal) befolgen. Der Standardname des Zugriffsschlüssels lautet **RootManageSharedAccessKey**. Kopieren Sie den Namen des Zugriffsschlüssels und den Wert des Primärschlüssels, um beide Angaben später in dieser exemplarischen Vorgehensweise zu verwenden. 
- Python 3.4 oder höher mit installiertem und aktualisiertem `pip`.
- Das Python-Paket für Event Hubs. Um das Paket zu installieren, führen Sie diesen Befehl in einer Eingabeaufforderung aus, in deren Pfad Python enthalten ist: 
  
  ```cmd
  pip install azure-eventhub
  ```
  
  > [!NOTE]
  > Dieser Code in diesem Schnellstart verwendet die aktuelle stabile Version 1.3.1 des Event Hubs SDK. Beispielcode, der die Vorschauversion des SDK verwendet, finden Sie unter [https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/eventhub/azure-eventhubs/examples](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/eventhub/azure-eventhubs/examples).

## <a name="send-events"></a>Senden von Ereignissen

Um eine Python-Anwendung zu erstellen, die Ereignisse an einen Event Hub sendet:

1. Öffnen Sie Ihren bevorzugten Python-Editor, z. B. [Visual Studio Code](https://code.visualstudio.com/).
2. Erstellen Sie eine neue Datei mit dem Namen *send.py*. Dieses Skript sendet 100 Ereignisse an Ihren Event Hub.
3. Fügen Sie den folgenden Code in *send.py*  ein, und ersetzen Sie dabei die Angaben \<namespace>, \<eventhub>, \<AccessKeyName> und \<primary key value> von Event Hubs durch Ihre Werte: 
   
   ```python
   import sys
   import logging
   import datetime
   import time
   import os
   
   from azure.eventhub import EventHubClient, Sender, EventData
   
   logger = logging.getLogger("azure")
   
   # Address can be in either of these formats:
   # "amqps://<URL-encoded-SAS-policy>:<URL-encoded-SAS-key>@<namespace>.servicebus.windows.net/eventhub"
   # "amqps://<namespace>.servicebus.windows.net/<eventhub>"
   # SAS policy and key are not required if they are encoded in the URL
   
   ADDRESS = "amqps://<namespace>.servicebus.windows.net/<eventhub>"
   USER = "<AccessKeyName>"
   KEY = "<primary key value>"
   
   try:
       if not ADDRESS:
           raise ValueError("No EventHubs URL supplied.")
   
       # Create Event Hubs client
       client = EventHubClient(ADDRESS, debug=False, username=USER, password=KEY)
       sender = client.add_sender(partition="0")
       client.run()
       try:
           start_time = time.time()
           for i in range(100):
               print("Sending message: {}".format(i))
               message = "Message {}".format(i)
               sender.send(EventData(message))
       except:
           raise
       finally:
           end_time = time.time()
           client.stop()
           run_time = end_time - start_time
           logger.info("Runtime: {} seconds".format(run_time))
   
   except KeyboardInterrupt:
       pass
   ```
   
4. Speichern Sie die Datei . 

Um das Skript auszuführen, führen Sie den folgenden Befehl aus dem Verzeichnis aus, in dem Sie *send.py* gespeichert haben:

```cmd
start python send.py
```

Glückwunsch! Sie haben jetzt Nachrichten an einen Event Hub gesendet.

## <a name="receive-events"></a>Empfangen von Ereignissen

Um eine Python-Anwendung zu erstellen, die Ereignisse von einem Event Hub empfängt:

1. Erstellen Sie in Ihrem Python-Editor eine Datei namens *recv.py*.
2. Fügen Sie den folgenden Code in *recv.py*  ein, und ersetzen Sie dabei die Angaben \<namespace>, \<eventhub>, \<AccessKeyName> und \<primary key value> von Event Hubs durch Ihre Werte: 
   
   ```python
   import os
   import sys
   import logging
   import time
   from azure.eventhub import EventHubClient, Receiver, Offset
   
   logger = logging.getLogger("azure")
   
   # Address can be in either of these formats:
   # "amqps://<URL-encoded-SAS-policy>:<URL-encoded-SAS-key>@<mynamespace>.servicebus.windows.net/myeventhub"
   # "amqps://<namespace>.servicebus.windows.net/<eventhub>"
   # SAS policy and key are not required if they are encoded in the URL
   
   ADDRESS = "amqps://<namespace>.servicebus.windows.net/<eventhub>"
   USER = "<AccessKeyName>"
   KEY = "<primary key value>"
   
   
   CONSUMER_GROUP = "$default"
   OFFSET = Offset("-1")
   PARTITION = "0"
   
   total = 0
   last_sn = -1
   last_offset = "-1"
   client = EventHubClient(ADDRESS, debug=False, username=USER, password=KEY)
   try:
       receiver = client.add_receiver(
           CONSUMER_GROUP, PARTITION, prefetch=5000, offset=OFFSET)
       client.run()
       start_time = time.time()
       for event_data in receiver.receive(timeout=100):
           print("Received: {}".format(event_data.body_as_str(encoding='UTF-8')))
           total += 1
   
       end_time = time.time()
       client.stop()
       run_time = end_time - start_time
       print("Received {} messages in {} seconds".format(total, run_time))
   
   except KeyboardInterrupt:
       pass
   finally:
       client.stop()
   ```
   
4. Speichern Sie die Datei .

Um das Skript auszuführen, führen Sie den folgenden Befehl aus dem Verzeichnis aus, in dem Sie *recv.py* gespeichert haben:

```cmd
start python recv.py
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Event Hubs finden Sie in den folgenden Artikeln:

- [EventProcessorHost](event-hubs-event-processor-host.md)
- [Features und Terminologie in Azure Event Hubs](event-hubs-features.md)
- [Event Hubs – häufig gestellte Fragen](event-hubs-faq.md)

