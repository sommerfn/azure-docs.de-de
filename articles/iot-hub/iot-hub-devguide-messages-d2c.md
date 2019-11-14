---
title: Grundlegendes zum Azure IoT Hub-Nachrichtenrouting | Microsoft-Dokumentation
description: 'Entwicklerhandbuch: Verwenden des Nachrichtenroutings zum Senden von D2C-Nachrichten. Enthält Informationen zum Senden von Telemetriedaten und anderen Daten.'
author: ash2017
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 05/15/2019
ms.author: asrastog
ms.openlocfilehash: ff50d972ad9590fb70dbcf67e21f8b5dc8c32fad
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73748063"
---
# <a name="use-iot-hub-message-routing-to-send-device-to-cloud-messages-to-different-endpoints"></a>Verwenden des IoT Hub-Nachrichtenroutings zum Senden von D2C-Nachrichten an verschiedene Endpunkte

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Das Nachrichtenrouting ermöglicht es Ihnen, Nachrichten automatisiert, skalierbar und zuverlässig von Ihren Geräten an Clouddienste zu senden. Das Nachrichtenrouting kann für Folgendes verwendet werden: 

* **Senden von Gerätetelemetrienachrichten sowie von Ereignissen** – insbesondere von Ereignissen im Gerätelebenszyklus und Änderungsereignissen für Gerätezwillinge im integrierten Endpunkt und in benutzerdefinierten Endpunkten. Erfahren Sie mehr über [Routingendpunkte](#routing-endpoints).

* **Filtern von Daten vor dem Weiterleiten an verschiedene Endpunkte** durch Anwenden umfassender Abfragen. Mithilfe des Nachrichtenroutings können Sie Abfragen für Nachrichteneigenschaften und Nachrichtentext sowie für Gerätezwillingstags und Gerätezwillingseigenschaften durchführen. Erfahren Sie mehr über die Verwendung von [Abfragen im Nachrichtenrouting](iot-hub-devguide-routing-query-syntax.md).

IoT Hub benötigt Schreibzugriff auf diese Dienstendpunkte, damit das Nachrichtenrouting funktioniert. Wenn Sie Ihre Endpunkte über das Azure-Portal konfigurieren, werden die erforderlichen Berechtigungen für Sie hinzugefügt. Stellen Sie sicher, dass Sie Ihre Dienste zur Unterstützung des erwarteten Durchsatzes konfigurieren. Wenn Sie z. B. Event Hubs als benutzerdefinierten Endpunkt verwenden, müssen Sie die **Durchsatzeinheiten** für den Event Hub so konfigurieren, dass er den Eingang von Ereignissen behandeln kann, die über IoT Hub-Nachrichtenrouting gesendet werden sollen. Wenn Sie eine Service Bus-Warteschlange als Endpunkt verwenden, müssen Sie auch die **maximale Größe** konfigurieren, damit die Warteschlange alle eingegangenen Daten aufnehmen kann, bis sie aus Consumern ausgehen. Nach der Erstkonfiguration Ihrer IoT-Lösung müssen Sie möglicherweise Ihre zusätzlichen Endpunkte überwachen und ggf. Anpassungen an die tatsächliche Last vornehmen.

Der IoT Hub definiert ein [gemeinsames Format](iot-hub-devguide-messages-construct.md) für alle Gerät-zu-Cloud-Nachrichten, um Interoperabilität zwischen Protokollen zu ermöglichen. Wenn eine Nachricht mehreren Routen entspricht, die auf den gleichen Endpunkt verweisen, übermittelt IoT Hub die Nachricht nur einmal an diesen Endpunkt. Aus diesem Grund müssen Sie keine Deduplizierung für Ihre Service Bus-Warteschlange oder Ihr Service Bus-Thema konfigurieren. In partitionierten Warteschlangen garantiert die Partitionsaffinität die Nachrichtensortierung. In diesem Tutorial lernen Sie, wie Sie das [Nachrichtenrouting konfigurieren](tutorial-routing.md).

## <a name="routing-endpoints"></a>Routingendpunkte

Ein IoT Hub verfügt über einen standardmäßigen integrierten Endpunkt (**messages/events**), der mit Event Hubs kompatibel ist. Sie können [benutzerdefinierte Endpunkte](iot-hub-devguide-endpoints.md#custom-endpoints) für die Weiterleitung von Nachrichten erstellen, indem Sie andere Dienste Ihres Abonnements mit dem IoT Hub verknüpfen. 

Jede Nachricht wird an alle Endpunkte weitergeleitet, die mit ihren Routingabfragen übereinstimmen. Anders ausgedrückt: Eine Nachricht kann an mehrere Endpunkte weitergeleitet werden.

IoT Hub unterstützt derzeit folgende Dienste als benutzerdefinierte Endpunkte:

### <a name="built-in-endpoint"></a>Integrierter Endpunkt

Sie können standardmäßige [Event Hubs-Integration und -SDKs](iot-hub-devguide-messages-read-builtin.md) zum Empfangen von D2C-Nachrichten vom integrierten Endpunkt (**messages/events**) verwenden. Sobald eine Route erstellt wird, werden keine Daten mehr an den integrierten Endpunkt gesendet, es sei denn, eine Route zu diesem Endpunkt wird erstellt.

### <a name="azure-storage"></a>Azure Storage

Es gibt zwei Speicherdienste, an die IoT Hub Nachrichten weiterleiten kann: [Azure Blob Storage](../storage/blobs/storage-blobs-introduction.md)- und [Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-introduction.md) (ADLS Gen2)-Konten. Azure Data Lake Storage-Konten sind Speicherkonten mit aktivierten [hierarchischen Namespaces](../storage/blobs/data-lake-storage-namespace.md), die auf Blob-Speicher basieren. Beide verwenden Blobs als Speicher.

IoT Hub unterstützt das Schreiben von Daten in Azure Storage in den Formaten [Apache Avro](https://avro.apache.org/) und „JSON“. Standardwert: AVRO. Das Codierungsformat kann nur festgelegt werden, wenn der Endpunkt des Blobspeichers konfiguriert ist. Das Format kann nicht für einen vorhandenen Endpunkt bearbeitet werden. Wenn Sie die JSON-Codierung verwenden, müssen Sie in der Nachricht [Systemeigenschaften](iot-hub-devguide-routing-query-syntax.md#system-properties) „contentType“ auf **application/json** und „contentEncoding“ auf **UTF-8** festlegen. Bei diesen beiden Werten wird die Groß-/Kleinschreibung nicht beachtet. Wenn die Inhaltscodierung nicht festgelegt ist, schreibt IoT Hub die Nachrichten in Base64-codiertem Format. Sie können das Codierungsformat über die IoT Hub-REST-API „Create“ oder „Update“ auswählen – insbesondere [RoutingStorageContainerProperties](https://docs.microsoft.com/rest/api/iothub/iothubresource/createorupdate#routingstoragecontainerproperties), das Azure-Portal, die [Azure CLI](https://docs.microsoft.com/cli/azure/iot/hub/routing-endpoint?view=azure-cli-latest) oder die [Azure Powershell](https://docs.microsoft.com/powershell/module/az.iothub/add-aziothubroutingendpoint?view=azps-1.3.0). Das folgende Diagramm zeigt, wie Sie das Codierungsformat im Azure-Portal auswählen.

![Endpunktcodierung für Blobspeicher](./media/iot-hub-devguide-messages-d2c/blobencoding.png)

IoT Hub verarbeitet Nachrichten batchweise und schreibt Daten in den Speicher, wenn der Batch eine bestimmte Größe erreicht hat oder ein bestimmter Zeitraum verstrichen ist. IoT Hub folgt standardmäßig der nachstehenden Dateibenennungskonvention: 

```
{iothub}/{partition}/{YYYY}/{MM}/{DD}/{HH}/{mm}
```

Sie können eine beliebige Dateibenennungskonvention verwenden, müssen jedoch alle aufgelisteten Tokens verwenden. IoT Hub schreibt in ein leeres Blob, wenn keine Daten zum Schreiben vorhanden sind.

Wir empfehlen, die Blobs oder Dateien aufzulisten und anschließend zu durchlaufen, um sicherzustellen, dass alle Blobs oder Dateien gelesen werden, ohne dass eine Partition vorhanden ist. Der Partitionsbereich könnte sich möglicherweise bei einem [von Microsoft initiierten Failover](iot-hub-ha-dr.md#microsoft-initiated-failover) oder einem [manuellen Failover](iot-hub-ha-dr.md#manual-failover) in Zusammenhang mit IoT Hub ändern. Sie können die Liste der Blobs oder die [Liste der ADLS Gen2-API](https://docs.microsoft.com/rest/api/storageservices/list-blobs) mithilfe der [List Blobs-API](https://docs.microsoft.com/rest/api/storageservices/datalakestoragegen2/path/list) aufzählen, um die gewünschte Liste von Dateien zu erhalten. Das folgende Beispiel dient als Anleitung.

```csharp
public void ListBlobsInContainer(string containerName, string iothub)
{
    var storageAccount = CloudStorageAccount.Parse(this.blobConnectionString);
    var cloudBlobContainer = storageAccount.CreateCloudBlobClient().GetContainerReference(containerName);
    if (cloudBlobContainer.Exists())
    {
        var results = cloudBlobContainer.ListBlobs(prefix: $"{iothub}/");
        foreach (IListBlobItem item in results)
        {
            Console.WriteLine(item.Uri);
        }
    }
}
```

Wenn Sie ein Azure Data Lake Gen2-kompatibles Speicherkonto erstellen möchten, erstellen Sie ein neues V2-Speicherkonto, und wählen Sie auf der Registerkarte *Erweitert* im Feld *Hierarchischer Namespace* die Option **aktiviert** aus, wie in der folgenden Abbildung gezeigt wird:

![Auswählen von Azure Data Lake Gen2-Speicher](./media/iot-hub-devguide-messages-d2c/selectadls2storage.png)


### <a name="service-bus-queues-and-service-bus-topics"></a>Service Bus-Warteschlangen und Service Bus-Themen

Für Service Bus-Warteschlangen und -Themen, die als IoT Hub-Endpunkte verwendet werden, dürfen **Sitzungen** oder **Duplikaterkennung** nicht aktiviert werden. Wenn eine dieser Optionen aktiviert ist, wird der Endpunkt im Azure-Portal als **Nicht erreichbar** angezeigt.

### <a name="event-hubs"></a>Event Hubs

Sie können Daten nicht nur an den mit Event Hubs kompatiblen integrierten Endpunkt, sondern auch an benutzerdefinierte Endpunkte vom Typ „Event Hubs“ weiterleiten. 

## <a name="reading-data-that-has-been-routed"></a>Lesen weitergeleiteter Daten

Sie können mit diesem [Tutorial](tutorial-routing.md) eine Route konfigurieren.

Verwenden Sie die folgenden Tutorials, um zu erfahren, wie Sie Nachrichten aus einem Endpunkt auslesen.

* Lesen vom [integrierten Endpunkt](quickstart-send-telemetry-node.md)

* Lesen aus [Blob Storage](../storage/blobs/storage-blob-event-quickstart.md)

* Lesen aus [Event Hubs](../event-hubs/event-hubs-dotnet-standard-getstarted-send.md)

* Lesen aus [Service Bus-Warteschlangen](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md)

* Lesen aus [Service Bus-Themen](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions)

## <a name="fallback-route"></a>Fallbackroute

Die Fallbackroute sendet alle Nachrichten, die die Abfragebedingungen in einer der vorhandenen Routen nicht erfüllen, an den integrierten Endpunkt (**messages/events**), der mit [Event Hubs](/azure/event-hubs/) kompatibel ist. Wenn das Nachrichtenrouting aktiviert ist, können Sie die Funktion der Fallbackroute verwenden. Sobald eine Route erstellt wird, werden keine Daten mehr an den integrierten Endpunkt gesendet, es sei denn, eine Route zu diesem Endpunkt wird erstellt. Wenn keine Routen zum integrierten Endpunkt vorhanden sind und eine Fallbackroute aktiviert ist, werden nur Nachrichten an den integrierten Endpunkt gesendet, die keinen Abfragebedingungen in Routen entsprechen. Wenn alle vorhandenen Routen gelöscht wurden, muss eine Fallbackroute aktiviert werden, um alle Daten im integrierten Endpunkt zu empfangen.

Sie können die Fallbackroute im Azure-Portal auf dem Blatt „Nachrichtenrouting“ aktivieren und deaktivieren. Sie können auch Azure Resource Manager verwenden, um [FallbackRouteProperties](/rest/api/iothub/iothubresource/createorupdate#fallbackrouteproperties) für die Nutzung eines benutzerdefinierten Endpunkts für die Fallbackroute festzulegen.

## <a name="non-telemetry-events"></a>Nicht telemetriebezogene Ereignisse

Das Nachrichtenrouting ermöglicht zusätzlich zum Weiterleiten von Gerätetelemetriedaten auch das Senden von Änderungsereignissen bei Gerätezwillingen, Ereignissen im Gerätelebenszyklus und Änderungsereignissen bei digitalen Zwillingen (in der öffentlichen Vorschau). Wenn beispielsweise eine Route erstellt wird, deren Datenquelle auf **Änderungsereignisse für Gerätezwillinge** festgelegt ist, sendet IoT Hub Nachrichten an den Endpunkt, der die Änderung im Gerätezwilling enthält. Ebenso gilt: Wenn eine Route erstellt wird, deren Datenquelle auf **Ereignisse im Gerätelebenszyklus** festgelegt ist, sendet IoT Hub eine Nachricht, die mitteilt, ob das Gerät erstellt oder gelöscht wurde. Und schließlich kann ein Entwickler als Teil der [Public Preview von IoT Plug & Play](../iot-pnp/overview-iot-plug-and-play.md) Routen erstellen, deren Datenquelle auf **Änderungsereignisse bei digitalen Zwillingen** festgelegt wird, und IoT Hub sendet immer dann Nachrichten, wenn eine [Eigenschaft](../iot-pnp/iot-plug-and-play-glossary.md) des digitalen Zwillings festgelegt oder geändert wird, ein [digitaler Zwilling](../iot-pnp/iot-plug-and-play-glossary.md) ersetzt wird oder wenn es bei dem zugrunde liegenden Gerätezwilling zu einem Änderungsereignis kommt.

[IoT Hub lässt sich auch in Azure Event Grid integrieren](iot-hub-event-grid.md), um Geräteereignisse zu veröffentlichen und so Echtzeitintegrationen und die Automatisierung von Workflows basierend auf diesen Ereignissen zu unterstützen. Informationen dazu, welche Methode sich am besten für Ihr Szenario eignet, finden Sie unter [Vergleichen von Nachrichtenweiterleitung und Event Grid für IoT Hub](iot-hub-event-grid-routing-comparison.md).

## <a name="testing-routes"></a>Testen von Routen

Wenn Sie eine neue Route erstellen oder eine vorhandene Route bearbeiten, sollten Sie die Routenabfrage mit einer Beispielnachricht testen. Sie können einzelne Routen oder alle Routen gleichzeitig testen. Während des Tests werden keine Nachrichten an die Endpunkte weitergeleitet. Zum Testen können Sie das Azure-Portal, Azure Resource Manager, Azure PowerShell oder die Azure CLI verwenden. Anhand der Ausgaben können Sie herausfinden, ob die Beispielnachricht der Abfrage entspricht oder nicht oder ob der Test nicht ausgeführt werden konnte, weil Beispielnachricht oder Abfragesyntax falsch sind. Weitere Informationen finden Sie unter [Testen einer Route](/rest/api/iothub/iothubresource/testroute) und [Testen aller Routen](/rest/api/iothub/iothubresource/testallroutes).

## <a name="ordering-guarantees-with-at-least-once-delivery"></a>Reihenfolgengarantien mit mindestens einer Übermittlung

Das IoT Hub-Nachrichtenrouting garantiert eine geordnete und mindestens einmal erfolgende Übermittlung von Nachrichten an die Endpunkte. Dies bedeutet, dass es doppelte Nachrichten geben und eine Reihe von Nachrichten erneut übertragen werden kann, wobei die ursprüngliche Nachrichtensortierung berücksichtigt wird. Wenn die ursprüngliche Reihenfolge der Nachrichten z.B. „[1,2,3,4]“ ist, könnten Sie eine Nachrichtensequenz wie „[1,2,1,2,3,1,2,3,4]“ erhalten. Die Reihenfolgengarantie legt fest, dass auf die von Ihnen empfangene Nachricht „[1]“ immer „[2,3,4]“ folgen.

Zum Verarbeiten von Nachrichtenduplikaten empfiehlt es sich, einen eindeutigen Bezeichner in den Anwendungseigenschaften der Nachricht am Ursprung zu stempeln, bei dem es sich normalerweise um ein Gerät oder ein Modul handelt. Der Dienst zur Verarbeitung der Nachrichten kann mithilfe dieses Bezeichners doppelte Nachrichten verarbeiten.

## <a name="latency"></a>Latency

Wenn Sie Geräte-zu-Cloud-Telemetrienachrichten über integrierte Endpunkte weiterleiten, kommt es nach der Erstellung der ersten Route zu einer leichten Erhöhung der Gesamtlatenz.

In den meisten Fällen beträgt der durchschnittliche Latenzanstieg weniger als 500 ms. Sie können die Latenz mithilfe der IoT Hub-Metriken **Routing: Nachrichtenlatenz für „messages/events“** oder **d2c.endpoints.latency.builtIn.events** überwachen. Das Erstellen oder Löschen einer Route nach der ersten hat keinen Einfluss auf die Gesamtlatenz.

## <a name="monitoring-and-troubleshooting"></a>Überwachung und Problembehandlung

IoT Hub bietet mehrere Metriken in Bezug auf Routing und Endpunkte, um Ihnen einen Überblick über die Integrität Ihres Hubs und der gesendeten Nachrichten zu verschaffen. Sie können Informationen aus mehreren Metriken kombinieren, um die Grundursache eines Problems zu ermitteln. Verwenden Sie z. B. die Metriken **Routing: verworfene Telemetrienachrichten** oder **d2c.telemetry.egress.dropped**, um herauszufinden, wie viele Nachrichten verworfen wurden, weil sie keiner Abfrage in einer der Routen entsprachen und die Fallbackroute deaktiviert war. Unter [IoT Hub-Metriken](iot-hub-metrics.md) werden alle Metriken aufgeführt, die standardmäßig für Ihren IoT Hub aktiviert sind.

Sie können die REST-API [Get Endpoint Health](https://docs.microsoft.com/rest/api/iothub/iothubresource/getendpointhealth#iothubresource_getendpointhealth) (Endpunktintegrität abrufen) verwenden, um den [Integritätsstatus](iot-hub-devguide-endpoints.md#custom-endpoints) der Endpunkte abzurufen. Es empfiehlt sich, die [IoT Hub-Metriken](iot-hub-metrics.md), die sich auf die Routinglatenz von Nachrichten beziehen, zu verwenden, um Fehler bei beschädigter Endpunktintegrität zu erkennen und zu beheben. Sie können z. B. für Endpunkttyp-Event Hubs **d2c.endpoints.latency.eventHubs** überwachen. Der Status eines fehlerhaften Endpunkts wird zu fehlerfrei aktualisiert, wenn IoT Hub einen möglicherweise konsistenten Integritätsstatus erreicht hat.

Mithilfe der Diagnoseprotokolle für **Routen** in den Azure Monitor-[Diagnoseeinstellungen](../iot-hub/iot-hub-monitor-resource-health.md) können Sie Fehler bei der Auswertung einer Routingabfrage und der Endpunktintegrität nachverfolgen, die von IoT Hub registriert werden, wenn beispielsweise ein Endpunkt nicht erreichbar ist. Diese Diagnoseprotokolle können zur benutzerdefinierten Verarbeitung an Azure Monitor-Protokolle, Event Hubs oder Azure Storage gesendet werden.

## <a name="next-steps"></a>Nächste Schritte

* Wie Sie Nachrichtenrouten erstellen, erfahren Sie unter [Verarbeiten von IoT Hub-D2C-Nachrichten mit Routen](tutorial-routing.md).

* [Senden von D2C-Nachrichten](quickstart-send-telemetry-node.md)

* Informationen zu den SDKs, die Sie zum Senden von D2C-Nachrichten verwenden können, finden Sie unter [Azure IoT-SDKs](iot-hub-devguide-sdks.md).
