---
title: 'Schnellstart: Verwenden von Azure Service Bus-Themen und -Abonnements mit Python'
description: Erfahren Sie mehr über die Verwendung von Azure Service Bus-Themen und -Abonnements über Python.
services: service-bus-messaging
documentationcenter: python
author: axisc
manager: timlt
editor: spelluru
ms.assetid: c4f1d76c-7567-4b33-9193-3788f82934e4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: quickstart
ms.date: 11/05/2019
ms.author: aschhab
ms.openlocfilehash: 94a49b31139947c6323ab391b78ecd03ee911e0a
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73748490"
---
# <a name="quickstart-use-service-bus-topics-and-subscriptions-with-python"></a>Schnellstart: Verwenden von Service Bus-Themen und -Abonnements mit Python

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

In diesem Artikel erfahren Sie, wie Sie Python mit Azure Service Bus-Themen und -Abonnements verwenden. In den Beispielen wird das [Azure Python SDK][Azure Python package]-Paket für Folgendes verwendet: 

- Erstellen von Themen und Abonnements von Themen
- Erstellen von Abonnementfiltern und -aktionen
- Senden von Nachrichten an Themen 
- Empfangen von Nachrichten aus Abonnements
- Löschen von Themen und Abonnements

## <a name="prerequisites"></a>Voraussetzungen
- Ein Azure-Abonnement. Sie können [Ihre Visual Studio-oder MSDN-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A85619ABF) oder [sich für ein kostenloses Konto anmelden](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF).
- Ein Service Bus-Namespace, der mithilfe der Schritte unter [Schnellstart: Verwenden des Azure-Portals zum Erstellen eines Service Bus-Themas und von Abonnements](service-bus-quickstart-topics-subscriptions-portal.md) erstellt wurde. Kopieren Sie den Namespacenamen, den Namen des SAS-Schlüssels und den Wert für den Primärschlüssel aus dem Bildschirm **SAS-Richtlinien** für die spätere Verwendung in diesem Schnellstart. 
- Python 3.4x oder höher mit installiertem [Azure Python SDK][Azure Python package]-Paket. Weitere Informationen finden Sie im [Python-Installationshandbuch](/azure/python/python-sdk-azure-install).

## <a name="create-a-servicebusservice-object"></a>Erstellen eines ServiceBusService-Objekts

Ein **ServiceBusService**-Objekt ermöglicht Ihnen das Arbeiten mit Themen und Abonnements von Themen. Um programmgesteuert auf Service Bus zuzugreifen, fügen Sie die folgende Zeile oben in Ihrer Python-Datei ein:

```python
from azure.servicebus.control_client import ServiceBusService, Message, Topic, Rule, DEFAULT_RULE_NAME
```

Fügen Sie den folgenden Code hinzu, um ein **ServiceBusService**-Objekt zu erstellen. Ersetzen Sie `<namespace>`, `<sharedaccesskeyname>` und `<sharedaccesskeyvalue>` durch Ihren Service Bus-Namespacenamen, den Namen des SAS-Schlüssels (Shared Access Signature) und den Wert des Primärschlüssels. Sie finden diese Werte unter **SAS-Richtlinien** in Ihrem Service Bus-Namespace im [Azure-Portal][Azure portal].

```python
bus_service = ServiceBusService(
    service_namespace='<namespace>',
    shared_access_key_name='<sharedaccesskeyname>',
    shared_access_key_value='<sharedaccesskeyvalue>')
```

## <a name="create-a-topic"></a>Erstellen eines Themas

Der folgende Code verwendet die Methode `create_topic`, um ein Service Bus-Thema namens `mytopic` mit Standardeinstellungen zu erstellen:

```python
bus_service.create_topic('mytopic')
```

Mithilfe von Themaoptionen können Sie die Standardthemaeinstellungen überschreiben, z.B. die Gültigkeitsdauer (Time to Live, TTL) oder die maximale Themagröße. Im folgenden Beispiel wird ein Thema mit dem Namen `mytopic` mit einer maximalen Themagröße von 5 GB und einer standardmäßigen Nachrichtengültigkeitsdauer von einer Minute erstellt:

```python
topic_options = Topic()
topic_options.max_size_in_megabytes = '5120'
topic_options.default_message_time_to_live = 'PT1M'

bus_service.create_topic('mytopic', topic_options)
```

## <a name="create-subscriptions"></a>Erstellen von Abonnements

Sie verwenden das **ServiceBusService**-Objekt auch, um Abonnements von Themen zu erstellen. Ein Abonnement kann über einen Filter verfügen, um den an die virtuelle Warteschlange übermittelten Nachrichtensatz einzuschränken. Wenn Sie keinen Filter angeben, verwenden neue Abonnements den Standardfilter **MatchAll**, der alle für das Thema veröffentlichten Nachrichten in die virtuelle Warteschlange des Abonnements einreiht. Mit dem folgenden Beispiel wird ein Abonnement von `mytopic` namens `AllMessages` erstellt, für das der Filter **MatchAll** verwendet wird:

```python
bus_service.create_subscription('mytopic', 'AllMessages')
```

### <a name="use-filters-with-subscriptions"></a>Verwenden von Filtern mit Abonnements

Verwenden Sie die `create_rule`-Methode des **ServiceBusService**-Objekts, um die Nachrichten zu filtern, die in einem Abonnement angezeigt werden. Sie können Regeln angeben, wenn Sie das Abonnement erstellen, oder Sie können vorhandenen Abonnements Regeln hinzufügen.

Der flexibelste Filtertyp ist ein **SqlFilter**, der eine Teilmenge von SQL-92 verwendet. SQL-Filter werden basierend auf den Eigenschaften von Nachrichten angewendet, die für das Thema veröffentlicht werden. Weitere Informationen zu den Ausdrücken, die mit einem SQL-Filter verwendet werden können, finden Sie unter der Syntax [SqlFilter.SqlExpression][SqlFilter.SqlExpression].

Da der Standardfilter **MatchAll** automatisch für alle neuen Abonnements gilt, müssen Sie ihn aus den Abonnements entfernen, die Sie filtern möchten, andernfalls überschreibt **MatchAll** alle anderen von Ihnen angegebenen Filter. Die Standardregel kann mithilfe der Methode `delete_rule` des Objekts **ServiceBusService** entfernt werden.

Im folgenden Beispiel wird ein Abonnement von `mytopic` mit dem Namen `HighMessages` mit einer **SqlFilter**-Regel namens `HighMessageFilter` erstellt. Mit der `HighMessageFilter`-Regel werden nur Nachrichten ausgewählt, deren benutzerdefinierte `messageposition`-Eigenschaft größer als 3 ist:

```python
bus_service.create_subscription('mytopic', 'HighMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messageposition > 3'

bus_service.create_rule('mytopic', 'HighMessages', 'HighMessageFilter', rule)
bus_service.delete_rule('mytopic', 'HighMessages', DEFAULT_RULE_NAME)
```

Im folgenden Beispiel wird ein Abonnement von `mytopic` mit dem Namen `LowMessages` mit einer **SqlFilter**-Regel namens `LowMessageFilter` erstellt. Mit der `LowMessageFilter`-Regel werden nur Nachrichten ausgewählt, deren `messageposition`-Eigenschaft kleiner als oder gleich 3 ist:

```python
bus_service.create_subscription('mytopic', 'LowMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messageposition <= 3'

bus_service.create_rule('mytopic', 'LowMessages', 'LowMessageFilter', rule)
bus_service.delete_rule('mytopic', 'LowMessages', DEFAULT_RULE_NAME)
```

Wenn `AllMessages`, `HighMessages`und `LowMessages` alle wirksam sind, werden an `mytopic` gesendete Nachrichten stets an Empfänger des `AllMessages`-Abonnements übermittelt. Nachrichten werden abhängig vom `messageposition`-Eigenschaftswert auch selektiv an das `HighMessages`- oder `LowMessages`-Abonnement übermittelt. 

## <a name="send-messages-to-a-topic"></a>Senden von Nachrichten an ein Thema

Anwendungen verwenden die `send_topic_message`-Methode des **ServiceBusService**-Objekts, um Nachrichten an ein Service Bus-Thema zu senden.

Im folgenden Beispiel werden fünf Testnachrichten an das `mytopic`-Thema gesendet. Der benutzerdefinierte `messageposition`-Eigenschaftswert hängt von der Iteration der Schleife ab und bestimmt, welche Abonnements die Nachrichten empfangen. 

```python
for i in range(5):
    msg = Message('Msg {0}'.format(i).encode('utf-8'),
                  custom_properties={'messageposition': i})
    bus_service.send_topic_message('mytopic', msg)
```

### <a name="message-size-limits-and-quotas"></a>Größenbeschränkungen und Kontingente für Nachrichten

Service Bus-Themen unterstützen eine maximale Nachrichtengröße von 256 KB im [Standard-Tarif](service-bus-premium-messaging.md) und 1 MB im [Premium-Tarif](service-bus-premium-messaging.md). Der Header, der die standardmäßigen und benutzerdefinierten Anwendungseigenschaften enthält, kann eine maximale Größe von 64 KB haben. Bei der Anzahl der Nachrichten, die in einem Thema enthalten sein können, besteht keine Beschränkung. Allerdings gilt eine Obergrenze für die Gesamtgröße der Nachrichten, die ein Thema enthalten kann. Sie können die Themagröße bei der Erstellung festlegen. Dabei gilt eine Obergrenze von 5 GB. 

Weitere Informationen zu Kontingenten finden Sie unter [Service Bus-Kontingente][Service Bus quotas].

## <a name="receive-messages-from-a-subscription"></a>Empfangen von Nachrichten aus einem Abonnement

Anwendungen verwenden die `receive_subscription_message`-Methode für das **ServiceBusService**-Objekt, um Nachrichten von einem Abonnement zu empfangen. Im folgenden Beispiel werden Nachrichten vom `LowMessages`-Abonnement empfangen und nach dem Lesen gelöscht:

```python
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=False)
print(msg.body)
```

Der optionale `peek_lock`-Parameter von `receive_subscription_message` bestimmt, ob Service Bus Nachrichten aus dem Abonnement löscht, nachdem sie gelesen wurden. Der Standardmodus für den Empfang von Nachrichten ist *PeekLock* oder `peek_lock` auf **TRUE** festgelegt. Dabei werden Nachrichten gelesen („peek“) und gesperrt, ohne sie aus dem Abonnement zu löschen. Anschließend muss jede Nachricht explizit abgeschlossen werden, um sie aus dem Abonnement zu entfernen.

Zum Löschen von Nachrichten aus dem Abonnement nach dem Lesen können Sie den `peek_lock`-Parameter auf **FALSE**festlegen, wie im vorherigen Beispiel gezeigt. Das Löschen von Nachrichten im Rahmen des Empfangsvorgangs ist das einfachste Modell und funktioniert gut, wenn die Anwendung fehlende Nachrichten im Falle eines Fehlers tolerieren kann. Um dieses Verfahren zu verstehen, stellen Sie sich ein Szenario vor, in dem die Anwendung die Empfangsanforderung ausstellt und dann abstürzt, bevor diese verarbeitet wurde. Wenn die Nachricht beim Empfang gelöscht wurde, hat die Anwendung beim Neustart und der Wiederaufnahme der Nachrichtenverarbeitung die Nachricht verpasst, die sie vor dem Absturz empfangen hat.

Wenn Ihre Anwendung fehlende Nachrichten nicht tolerieren kann, wird der Empfangsvorgang zu einem zweistufigen Vorgang. PeekLock ermittelt die nächste zu verarbeitende Nachricht, sperrt diese, um zu verhindern, dass andere Consumer sie erhalten, und sendet sie dann an die Anwendung zurück. Nach der Verarbeitung oder Speicherung der Nachricht schließt die Anwendung die zweite Stufe des Empfangsprozesses ab, indem sie die `complete`-Methode für das **Message**-Objekt aufruft.  Die Methode `complete` markiert die Nachricht als verarbeitet und entfernt sie aus dem Abonnement.

Das folgende Beispiel veranschaulicht ein PeekLock-Szenario:

```python
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=True)
if msg.body is not None:
    print(msg.body)
    msg.complete()
```

## <a name="handle-application-crashes-and-unreadable-messages"></a>Umgang mit Anwendungsabstürzen und nicht lesbaren Nachrichten

Service Bus stellt Funktionen zur Verfügung, die Sie bei der ordnungsgemäßen Behandlung von Fehlern in der Anwendung oder bei Problemen beim Verarbeiten einer Nachricht unterstützen. Wenn eine Empfängeranwendung eine Nachricht aus einem bestimmten Grund nicht verarbeiten kann, kann sie die Methode `unlock` für das **Message**-Objekt aufrufen. Service Bus entsperrt die Nachricht innerhalb des Abonnements und stellt sie zum erneuten Empfang bereit, entweder von derselben oder einer anderen verarbeitenden Anwendung.

Außerdem gibt es ein Timeout für Nachrichten, die innerhalb des Abonnements gesperrt sind. Wenn eine Anwendung eine Nachricht nicht verarbeiten kann, bevor das Timeout der Sperre abläuft (z.B. wenn die Anwendung abstürzt), entsperrt Service Bus die Nachricht automatisch und macht sie verfügbar, sodass sie erneut empfangen werden kann.

Falls die Anwendung nach der Verarbeitung der Nachricht, aber vor dem Aufruf der Methode `complete` abstürzt, wird die Nachricht der Anwendung erneut zugestellt, wenn diese neu gestartet wird. Dies wird häufig als *At-Least-Once Processing* (mindestens einmalige Verarbeitung) bezeichnet. Jede Nachricht wird mindestens ein Mal verarbeitet, wobei eine Nachricht in bestimmten Situationen unter Umständen erneut zugestellt wird. Wenn Ihr Szenario keine doppelte Verarbeitung tolerieren kann, können Sie die Eigenschaft **MessageId** der Nachricht verwenden, die über die Zustellversuche hinweg konstant bleibt, um die doppelte Zustellung von Nachrichten zu verarbeiten. 

## <a name="delete-topics-and-subscriptions"></a>Löschen von Themen und Abonnements

Verwenden Sie zum Löschen von Themen und Abonnements das [Azure-Portal][Azure portal] oder die `delete_topic`-Methode. Der folgende Code löscht das Thema namens `mytopic`:

```python
bus_service.delete_topic('mytopic')
```

Beim Löschen eines Themas werden alle Abonnements für das Thema gelöscht. Abonnements können auch unabhängig voneinander gelöscht werden. Der folgende Code löscht das Abonnement mit dem Namen `HighMessages` aus dem `mytopic`-Thema:

```python
bus_service.delete_subscription('mytopic', 'HighMessages')
```

Standardmäßig sind Themen und Abonnements persistent und so lange vorhanden, bis Sie sie löschen. Um Abonnements nach Ablauf eines bestimmten Zeitraums automatisch zu löschen, können Sie den Parameter [auto_delete_on_idle](https://docs.microsoft.com/python/api/azure-mgmt-servicebus/azure.mgmt.servicebus.models.sbsubscription?view=azure-python) für das Abonnement festlegen. 

> [!TIP]
> Sie können Service Bus-Ressourcen mit dem [Service Bus-Explorer](https://github.com/paolosalvatori/ServiceBusExplorer/) verwalten. Mit dem Service Bus-Explorer können Sie eine Verbindung mit einem Service Bus-Namespace herstellen und Messagingentitäten einfach verwalten. Das Tool stellt erweiterte Features wie Import-/Exportfunktionen und Testmöglichkeiten für Themen, Warteschlangen, Abonnements, Relaydienste, Notification Hubs und Event Hubs zur Verfügung. 

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun mit den Grundlagen der Service Bus-Themen vertraut sind, finden Sie unter den folgenden Links weitere Informationen:

* [Service Bus-Warteschlangen, -Themen und -Abonnements][Queues, topics, and subscriptions]
* [SqlFilter.SqlExpression][SqlFilter.SqlExpression]-Referenz

[Azure portal]: https://portal.azure.com
[Azure Python package]: https://pypi.python.org/pypi/azure
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter.SqlExpression]: service-bus-messaging-sql-filter.md
[Service Bus quotas]: service-bus-quotas.md 
