---
title: 'Schnellstart: Lesen erfasster Daten von der Python-App – Azure Event Hubs'
description: 'Schnellstart: Skripts, die das Azure Python SDK verwenden, um die Funktion Event Hubs Capture zu veranschaulichen.'
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: bdff820c-5b38-4054-a06a-d1de207f01f6
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.custom: seodec18
ms.date: 11/05/2019
ms.author: shvija
ms.openlocfilehash: ade4aa79b2de005bfecd7a5882f06cb491ea4e6d
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73717841"
---
# <a name="quickstart-event-hubs-capture-walkthrough-python"></a>Schnellstart: Exemplarische Vorgehensweise für Event Hubs Capture: Python

Capture ist ein Feature von Azure Event Hubs. Mit Capture können Sie Streamingdaten in Ihrem Event Hub automatisch an ein Azure Blob Storage-Konto Ihrer Wahl übermitteln. Diese Funktion erleichtert die Batchverarbeitung von Echtzeit-Streamingdaten. In diesem Artikel erfahren Sie, wie Sie die Event Hubs-Erfassung mit Python verwenden. Weitere Informationen zu Event Hubs Capture finden Sie unter [Erfassen von Ereignissen über Azure Event Hubs ][Overview of Event Hubs Capture].

Diese exemplarische Vorgehensweise verwendet das [Azure Python SDK](https://azure.microsoft.com/develop/python/), um das Capture-Feature zu veranschaulichen. Mit dem Programm *sender.py* werden simulierte Telemetriedaten der Umgebung im JSON-Format an Event Hubs gesendet. Der Event Hub verwendet das Capture-Feature, um diese Daten in Batches in den Blobspeicher zu schreiben. Die *capturereader.py*-App liest diese Blobs, erstellt eine Anfügedatei für jedes Gerät und schreibt die Daten in *CSV*-Dateien auf jedem Gerät.

In dieser exemplarischen Vorgehensweise führen Sie folgende Aktionen aus: 

> [!div class="checklist"]
> * Erstellen Sie im Azure-Portal ein Azure Blob Storage-Konto und einen Container.
> * Aktivieren Sie Event Hubs Capture, und verweisen Sie das Feature auf Ihr Speicherkonto.
> * Senden Sie Daten mithilfe eines Python-Skripts an den Event Hub.
> * Lesen und Verarbeiten Sie Dateien aus Event Hubs Capture mithilfe eines weiteren Python-Skripts.

## <a name="prerequisites"></a>Voraussetzungen

- Python 3.4 oder höher mit installiertem und aktualisiertem `pip`.
  
- Ein Azure-Abonnement. Falls Sie kein Abonnement besitzen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/), bevor Sie beginnen.
  
- Einen aktiven Event Hubs-Namespace und Event Hub, erstellt durch Befolgen der Anweisungen unter [Schnellstart: Erstellen eines Event Hubs mithilfe des Azure-Portals](event-hubs-create.md). Notieren Sie sich die Namen Ihres Namespace und Event Hub, weil sie später in dieser exemplarischen Vorgehensweise verwendet werden. 
  
  > [!NOTE]
  > Wenn Sie bereits über einen Speichercontainer verfügen, den Sie nutzen können, können Sie Capture aktivieren und den Speichercontainer auswählen, wenn Sie den Event Hub erstellen. 
  > 
  
- Der Name des freigegebenen Zugriffsschlüssels von Event Hubs und der Wert des Primärschlüssels. Suchen oder erstellen Sie diese Werte unter **SAS-Richtlinien** auf der Event Hubs-Seite. Der Standardname des Zugriffsschlüssels lautet **RootManageSharedAccessKey**. Kopieren Sie den Namen des Zugriffsschlüssels und den Wert des Primärschlüssels, um beide Angaben später in dieser exemplarischen Vorgehensweise zu verwenden. 

## <a name="create-an-azure-blob-storage-account-and-container"></a>Erstellen eines Azure Blob Storage-Kontos und eines Containers

Erstellen Sie ein Speicherkonto und einen Container für die Erfassung. 

1. Melden Sie sich beim [Azure-Portal][Azure portal] an.
2. Wählen Sie im linken Navigationsbereich **Speicherkonten** aus, und wählen Sie dann auf dem Bildschirm **Speicherkonten** die Option **Hinzufügen** aus.
3. Wählen Sie auf dem Bildschirm zum Erstellen von Speicherkonten ein Abonnement und eine Ressourcengruppe aus, und benennen Sie das Speicherkonto. Für die anderen Auswahlmöglichkeiten können Sie die Standardeinstellungen beibehalten. Wählen Sie **Überprüfen und erstellen** aus. überprüfen Sie die Einstellungen, und wählen Sie dann die Option **Erstellen** aus. 
   
   ![Speicherkonto erstellen][1]
   
4. Wenn die Bereitstellung abgeschlossen ist, wählen Sie **Zu Ressource wechseln** aus, und wählen Sie dann auf dem Speicherkontobildschirm **Übersicht** die Option **Container** aus.
5. Wählen Sie auf dem Bildschirm **Container** die Option **+ Container** aus. 
6. Benennen Sie auf dem Bildschirm **Neuer Container** den Container, und wählen Sie dann **OK** aus. Notieren Sie sich den Namen des Containers, der später in der exemplarischen Vorgehensweise verwendet wird. 
7. Wählen Sie im linken Navigationsbereich des Bildschirms **Container** die Option **Zugriffsschlüssel** aus. Kopieren Sie den **Speicherkontonamen** und den Wert von **Schlüssel** unter **key1**, um diese Angaben später in der exemplarischen Vorgehensweise zu verwenden.
 
## <a name="enable-event-hubs-capture"></a>Aktivieren von Event Hubs Capture

1. Navigieren Sie im Azure-Portal zu Ihrem Event Hub, indem Sie seinen Event Hubs-Namenspace unter **Alle Ressourcen** auswählen, im linken Navigationsbereich **Event Hubs** auswählen und dann Ihren Event Hub auswählen. 
2. Wählen Sie auf dem Bildschirm **Übersicht** des Event Hub die Option **Capture-Ereignisse**  aus.
3. Wählen Sie auf dem Bildschirm **Capture** die Option **Ein** aus. Wählen Sie dann unter **Azure Storage Container** die Option **Container auswählen** aus. 
4. Wählen Sie auf dem Bildschirm **Container** den Speichercontainer aus, den Sie verwenden möchten, und wählen Sie dann **Auswählen** aus. 
5. Wählen Sie auf dem Bildschirm **Capture** die Option **Änderungen speichern** aus. 

## <a name="create-a-python-script-to-send-events-to-event-hub"></a>Erstellen eines Python-Skripts zum Senden von Ereignissen an Event Hub
Dieses Skript sendet 200 Ereignisse an Ihren Event Hub. Bei den Ereignissen handelt es sich um einfache Umweltdaten im JSON-Format.

1. Öffnen Sie Ihren bevorzugten Python-Editor, z.B. [Visual Studio Code][Visual Studio Code].
2. Erstellen Sie eine neue Datei mit dem Namen *sender.py*. 
3. Fügen Sie den folgenden Code in *sender.py* ein. Ersetzen Sie die Angaben \<namespace>, \<AccessKeyName>, \<primary key value> und \<eventhub> von Event Hubs durch Ihre eigenen Werte.
   
   ```python
   import uuid
   import datetime
   import random
   import json
   from azure.servicebus.control_client import ServiceBusService
   
   sbs = ServiceBusService(service_namespace='<namespace>', shared_access_key_name='<AccessKeyName>', shared_access_key_value='<primary key value>')
   devices = []
   for x in range(0, 10):
       devices.append(str(uuid.uuid4()))
   
   for y in range(0,20):
       for dev in devices:
           reading = {'id': dev, 'timestamp': str(datetime.datetime.utcnow()), 'uv': random.random(), 'temperature': random.randint(70, 100), 'humidity': random.randint(70, 100)}
           s = json.dumps(reading)
           sbs.send_event('<eventhub>', s)
       print(y)
   ```
4. Speichern Sie die Datei .

## <a name="create-a-python-script-to-read-capture-files"></a>Erstellen eines Python-Skripts zum Lesen Ihrer Capture-Dateien

Dieses Skript liest die erfassten Dateien und erstellt für jedes Ihrer Geräte eine Datei, um nur die Daten für das jeweilige Gerät zu schreiben.

1. Erstellen Sie in Ihrem Python-Editor eine neue Datei namens *capturereader.py*. 
2. Fügen Sie den folgenden Code in *capturereader.py* ein. Ersetzen Sie \<storageaccount>, \<storage account access key> und \<storagecontainer> durch Ihre gespeicherten Werte.
   
   ```python
   import os
   import string
   import json
   import avro.schema
   from avro.datafile import DataFileReader, DataFileWriter
   from avro.io import DatumReader, DatumWriter
   from azure.storage.blob import BlockBlobService
   
   def processBlob(filename):
       reader = DataFileReader(open(filename, 'rb'), DatumReader())
       dict = {}
       for reading in reader:
           parsed_json = json.loads(reading["Body"])
           if not 'id' in parsed_json:
               return
           if not parsed_json['id'] in dict:
               list = []
               dict[parsed_json['id']] = list
           else:
               list = dict[parsed_json['id']]
               list.append(parsed_json)
       reader.close()
       for device in dict.keys():
           deviceFile = open(device + '.csv', "a")
           for r in dict[device]:
               deviceFile.write(", ".join([str(r[x]) for x in r.keys()])+'\n')
   
   def startProcessing(accountName, key, container):
       print('Processor started using path: ' + os.getcwd())
       block_blob_service = BlockBlobService(account_name=accountName, account_key=key)
       generator = block_blob_service.list_blobs(container)
       for blob in generator:
           #content_length == 508 is an empty file, so only process content_length > 508 (skip empty files)
           if blob.properties.content_length > 508:
               print('Downloaded a non empty blob: ' + blob.name)
               cleanName = str.replace(blob.name, '/', '_')
               block_blob_service.get_blob_to_path(container, blob.name, cleanName)
               processBlob(cleanName)
               os.remove(cleanName)
           block_blob_service.delete_blob(container, blob.name)
   startProcessing('<storageaccount>', '<storage account access key>', '<storagecontainer>')
   ```

## <a name="run-the-python-scripts"></a>Ausführen der Python-Skripts

1. Öffnen Sie eine Eingabeaufforderung, deren Pfad Python enthält, und führen Sie diese Befehle aus, um die Python-Pakete mit den erforderlichen Komponenten zu installieren:
   
   ```cmd
   pip install azure-storage
   pip install azure-servicebus
   pip install avro-python3
   ```
   
   Wenn Sie eine frühere Version von `azure-storage` oder `azure` verwenden, müssen Sie möglicherweise die `--upgrade`-Option verwenden.
   
   Möglicherweise müssen Sie auch den folgenden Befehl ausführen. Die Ausführung dieses Befehls ist bei den meisten Systemen nicht erforderlich. 
   
   ```cmd
   pip install cryptography
   ```
   
2. Führen Sie diesen Befehl aus dem Verzeichnis aus, in dem Sie *sender.py* und *capturereader.py* gespeichert haben:
   
   ```cmd
   start python sender.py
   ```
   
   Der Befehl startet einen neuen Python-Prozess zum Ausführen des Senders.
   
3. Wenn die Ausführung der Erfassung abgeschlossen ist, führen Sie den folgenden Befehl aus:
   
   ```cmd
   python capturereader.py
   ```

   Der Erfassungsprozessor lädt alle nicht leeren Blobs aus dem Speicherkontocontainer herunter und schreibt die Ergebnisse als *CSV*-Dateien in das lokale Verzeichnis. 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Event Hubs finden Sie in den folgenden Artikeln: 

* [Übersicht über Event Hubs Capture][Overview of Event Hubs Capture]
* [Beispielanwendungen mit Event Hubs](https://github.com/Azure/azure-event-hubs/tree/master/samples)
* [Übersicht über Event Hubs][Event Hubs overview]

[Azure portal]: https://portal.azure.com/
[Overview of Event Hubs Capture]: event-hubs-capture-overview.md
[1]: ./media/event-hubs-archive-python/event-hubs-python1.png
[About Azure storage accounts]:../storage/common/storage-create-storage-account.md
[Visual Studio Code]: https://code.visualstudio.com/
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
