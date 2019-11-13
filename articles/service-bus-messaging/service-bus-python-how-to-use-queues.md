---
title: 'Schnellstart: Verwenden von Azure Service Bus-Warteschlangen mit Python'
description: Erfahren Sie, wie Azure Service Bus-Warteschlangen mit Python verwendet werden.
services: service-bus-messaging
documentationcenter: python
author: axisc
manager: timlt
editor: spelluru
ms.assetid: b95ee5cd-3b31-459c-a7f3-cf8bcf77858b
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: quickstart
ms.date: 11/05/2019
ms.author: aschhab
ms.custom: seo-python-october2019
ms.openlocfilehash: d0f579fcd82860380f1aaa651a61c0259d075a0d
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73748530"
---
# <a name="quickstart-use-azure-service-bus-queues-with-python"></a>Schnellstart: Verwenden von Azure Service Bus-Warteschlangen mit Python

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

In diesem Artikel erfahren Sie, wie Sie mithilfe von Python Azure Service Bus-Warteschlangen erstellen, Nachrichten an sie senden und Nachrichten von ihnen empfangen können. 

Weitere Informationen zu den Python-Bibliotheken von Azure Service Bus finden Sie unter [Service Bus-Bibliotheken für Python-](/python/api/overview/azure/servicebus?view=azure-python).

## <a name="prerequisites"></a>Voraussetzungen
- Ein Azure-Abonnement. Sie können [Ihre Visual Studio-oder MSDN-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A85619ABF) oder [sich für ein kostenloses Konto anmelden](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF).
- Ein Service Bus-Namespace, der mithilfe der Schritte unter [Schnellstart: Verwenden des Azure-Portals zum Erstellen eines Service Bus-Themas und von Abonnements](service-bus-quickstart-topics-subscriptions-portal.md) erstellt wurde. Kopieren Sie die primäre Verbindungszeichenfolge aus dem Bildschirm **SAS-Richtlinien**, um sie später in diesem Artikel zu verwenden. 
- Python 3.4x oder höher mit installiertem [Python Azure Service Bus][Python Azure Service Bus package]-Paket. Weitere Informationen finden Sie im [Python-Installationshandbuch](/azure/python/python-sdk-azure-install). 

## <a name="create-a-queue"></a>Erstellen einer Warteschlange

Ein **ServiceBusClient**-Objekt ermöglicht es Ihnen, mit Warteschlangen zu arbeiten. Um programmgesteuert auf Service Bus zuzugreifen, fügen Sie die folgende Zeile oben in Ihrer Python-Datei ein:

```python
from azure.servicebus import ServiceBusClient
```

Fügen Sie den folgenden Code hinzu, um ein **ServiceBusClient**-Objekt zu erstellen. Ersetzen Sie `<connectionstring>` durch den Wert der primären Service Bus-Verbindungszeichenfolge. Sie finden diesen Wert unter **SAS-Richtlinien** in Ihrem Service Bus-Namespace im [Azure-Portal][Azure portal].

```python
sb_client = ServiceBusClient.from_connection_string('<connectionstring>')
```

Der folgende Code verwendet die Methode `create_queue` von **ServiceBusClient**, um eine Warteschlange namens `taskqueue` mit Standardeinstellungen zu erstellen:

```python
sb_client.create_queue("taskqueue")
```

Mithilfe von Optionen können Sie die Warteschlangenstandardeinstellungen überschreiben, z.B. die Gültigkeitsdauer (Time to Live, TTL) oder die maximale Themagröße. Der folgende Code erstellt eine Warteschlange mit dem Namen `taskqueue` mit einer maximalen Warteschlangengröße von 5 GB und einem TTL-Wert von einer Minute:

```python
sb_client.create_queue("taskqueue", max_size_in_megabytes=5120,
                       default_message_time_to_live=datetime.timedelta(minutes=1))
```

## <a name="send-messages-to-a-queue"></a>Senden von Nachrichten an eine Warteschlange

Um eine Nachricht an eine Service Bus-Warteschlange zu senden, ruft eine Anwendung die Methode `send` für das **ServiceBusClient**-Objekt auf. Im folgenden Codebeispiel wird ein Warteschlangenclient erstellt und eine Testnachricht an die `taskqueue`-Warteschlange gesendet. Ersetzen Sie `<connectionstring>` durch den Wert der primären Service Bus-Verbindungszeichenfolge. 

```python
from azure.servicebus import QueueClient, Message

# Create the QueueClient
queue_client = QueueClient.from_connection_string("<connectionstring>", "taskqueue")

# Send a test message to the queue
msg = Message(b'Test Message')
queue_client.send(msg)
```

### <a name="message-size-limits-and-quotas"></a>Größenbeschränkungen und Kontingente für Nachrichten

Service Bus-Warteschlangen unterstützen eine maximale Nachrichtengröße von 256 KB für den [Standard-Tarif](service-bus-premium-messaging.md) und 1 MB für den [Premium-Tarif](service-bus-premium-messaging.md). Der Header, der die standardmäßigen und benutzerdefinierten Anwendungseigenschaften enthält, kann eine maximale Größe von 64 KB haben. Bei der Anzahl der Nachrichten, die in einer Warteschlange enthalten sein können, besteht keine Beschränkung. Allerdings gilt eine Obergrenze für die Gesamtgröße der Nachrichten, die eine Warteschlange enthalten kann. Sie können die Warteschlangengröße bei der Erstellung festlegen. Dabei gilt eine Obergrenze von 5 GB. 

Weitere Informationen zu Kontingenten finden Sie unter [Service Bus-Kontingente][Service Bus quotas].

## <a name="receive-messages-from-a-queue"></a>Empfangen von Nachrichten aus einer Warteschlange

Der Warteschlangenclient empfängt Nachrichten aus einer Warteschlange mithilfe der `get_receiver`-Methode für das **ServiceBusClient**-Objekt. Im folgenden Codebeispiel wird ein Warteschlangenclient erstellt und eine Testnachricht aus der `taskqueue`-Warteschlange empfangen. Ersetzen Sie `<connectionstring>` durch den Wert der primären Service Bus-Verbindungszeichenfolge. 

```python
from azure.servicebus import QueueClient, Message

# Create the QueueClient
queue_client = QueueClient.from_connection_string("<connectionstring>", "taskqueue")

# Receive the message from the queue
with queue_client.get_receiver() as queue_receiver:
    messages = queue_receiver.fetch_next(timeout=3)
    for message in messages:
        print(message)
        message.complete()
```

### <a name="use-the-peek_lock-parameter"></a>Verwenden des peek_lock-Parameters

Der optionale `peek_lock`-Parameter von `get_receiver` bestimmt, ob Service Bus Nachrichten aus der Warteschlange löscht, nachdem sie gelesen wurden. Der Standardmodus für den Empfang von Nachrichten ist *PeekLock* oder `peek_lock` auf **TRUE** festgelegt. Dabei werden Nachrichten gelesen („peek“) und gesperrt, ohne sie aus der Warteschlange zu löschen. Anschließend muss jede Nachricht explizit abgeschlossen werden, um sie aus der Warteschlange zu entfernen.

Zum Löschen von Nachrichten aus der Warteschlange nach dem Lesen können Sie den `peek_lock`-Parameter von `get_receiver` auf **FALSE**festlegen. Das Löschen von Nachrichten im Rahmen des Empfangsvorgangs ist das einfachste Modell, es funktioniert aber nur, wenn die Anwendung fehlende Nachrichten im Falle eines Fehlers tolerieren kann. Um dieses Verfahren zu verstehen, stellen Sie sich ein Szenario vor, in dem der Consumer eine Empfangsanforderung ausstellt und dann abstürzt, bevor diese verarbeitet wird. Wenn die Nachricht beim Empfang gelöscht wurde, hat die Anwendung beim Neustart und der Wiederaufnahme der Nachrichtenverarbeitung die Nachricht verpasst, die sie vor dem Absturz empfangen hat.

Wenn Ihre Anwendung fehlende Nachrichten nicht tolerieren kann, ist der Empfangsvorgang ein zweistufiger Vorgang. PeekLock ermittelt die nächste zu verarbeitende Nachricht, sperrt diese, um zu verhindern, dass andere Consumer sie erhalten, und sendet sie dann an die Anwendung zurück. Nach der Verarbeitung oder Speicherung der Nachricht schließt die Anwendung die zweite Stufe des Empfangsprozesses ab, indem sie die `complete`-Methode für das **Message**-Objekt aufruft.  Die Methode `complete` markiert die Nachricht als verarbeitet und entfernt sie aus der Warteschlange.

## <a name="handle-application-crashes-and-unreadable-messages"></a>Umgang mit Anwendungsabstürzen und nicht lesbaren Nachrichten

Service Bus stellt Funktionen zur Verfügung, die Sie bei der ordnungsgemäßen Behandlung von Fehlern in der Anwendung oder bei Problemen beim Verarbeiten einer Nachricht unterstützen. Wenn eine Empfängeranwendung eine Nachricht aus einem bestimmten Grund nicht verarbeiten kann, kann sie die Methode `unlock` für das **Message**-Objekt aufrufen. Service Bus entsperrt die Nachricht innerhalb der Warteschlange und stellt sie zum erneuten Empfang bereit, entweder von derselben oder einer anderen verarbeitenden Anwendung.

Außerdem gibt es ein Timeout für Nachrichten, die innerhalb der Warteschlange gesperrt sind. Wenn eine Anwendung eine Nachricht nicht verarbeiten kann, bevor das Timeout der Sperre abläuft (z.B. wenn die Anwendung abstürzt), entsperrt Service Bus die Nachricht automatisch und macht sie verfügbar, sodass sie erneut empfangen werden kann.

Falls die Anwendung nach der Verarbeitung der Nachricht, aber vor dem Aufruf der Methode `complete` abstürzt, wird die Nachricht der Anwendung erneut zugestellt, wenn diese neu gestartet wird. Dies wird häufig als *At-Least-Once Processing* (Mindestens einmalige Verarbeitung) bezeichnet. Jede Nachricht wird mindestens ein Mal verarbeitet, wobei eine Nachricht in bestimmten Situationen unter Umständen erneut zugestellt wird. Wenn Ihr Szenario keine doppelte Verarbeitung tolerieren kann, können Sie die Eigenschaft **MessageId** der Nachricht verwenden, die über die Zustellversuche hinweg konstant bleibt, um die doppelte Zustellung von Nachrichten zu verarbeiten. 

> [!TIP]
> Sie können Service Bus-Ressourcen mit dem [Service Bus-Explorer](https://github.com/paolosalvatori/ServiceBusExplorer/) verwalten. Mit dem Service Bus-Explorer können Sie eine Verbindung mit einem Service Bus-Namespace herstellen und Messagingentitäten einfach verwalten. Das Tool stellt erweiterte Features wie Import-/Exportfunktionen und Testmöglichkeiten für Themen, Warteschlangen, Abonnements, Relaydienste, Notification Hubs und Event Hubs zur Verfügung.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun mit den Grundlagen von Service Bus-Warteschlangen vertraut sind, finden Sie weitere Informationen unter [Warteschlangen, Themen und Abonnements][Queues, topics, and subscriptions].

[Azure portal]: https://portal.azure.com
[Python Azure Service Bus package]: https://pypi.python.org/pypi/azure-servicebus  
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[Service Bus quotas]: service-bus-quotas.md
