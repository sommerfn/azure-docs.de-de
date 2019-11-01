---
title: Event Grid-Trigger für Azure Functions
description: Behandlung von Event Grid-Ereignissen in Azure Functions.
services: functions
documentationcenter: na
author: craigshoemaker
manager: gwallace
keywords: ''
ms.service: azure-functions
ms.topic: reference
ms.date: 09/04/2018
ms.author: cshoe
ms.openlocfilehash: ff21a1325e341f119fc8615ca03b41b8068aca19
ms.sourcegitcommit: 92d42c04e0585a353668067910b1a6afaf07c709
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/28/2019
ms.locfileid: "72991421"
---
# <a name="event-grid-trigger-for-azure-functions"></a>Event Grid-Trigger für Azure Functions

In diesem Artikel wird die Behandlung von [Event Grid](../event-grid/overview.md)-Ereignissen in Azure Functions erläutert.

Event Grid ist ein Azure-Dienst, mit dem HTTP-Anforderungen gesendet werden, um Sie über Ereignisse zu benachrichtigen, die in *Herausgebern* erfolgen. Ein Herausgeber ist der Dienst oder die Ressource, von dem bzw. der das Ereignis stammt. Ein Azure Blob Storage-Konto ist beispielsweise ein Herausgeber, [der Upload oder die Löschung eines Blobs ist dagegen ein Ereignis](../storage/blobs/storage-blob-event-overview.md). Einige [Azure-Dienste bieten eine integrierte Unterstützung zum Veröffentlichen von Ereignissen in Event Grid](../event-grid/overview.md#event-sources).

*Ereignishandler* empfangen und verarbeiten Ereignisse. Azure Functions ist einer von mehreren [Azure-Diensten, die eine integrierte Unterstützung für die Behandlung von Event Grid-Ereignissen bieten](../event-grid/overview.md#event-handlers). In diesem Artikel wird beschrieben, wie Sie mithilfe eines Event Grid-Triggers eine Funktion aufrufen, wenn ein Ereignis von Event Grid empfangen wird.

Bei Bedarf können Sie einen HTTP-Trigger zur Behandlung von Event Grid-Ereignissen verwenden. Informationen dazu finden Sie weiter unten in diesem Artikel unter [Verwenden eines HTTP-Triggers als Event Grid-Trigger](#use-an-http-trigger-as-an-event-grid-trigger). Momentan kann für eine Azure Functions-App kein Event Grid-Trigger verwendet werden, wenn das Ereignis im [CloudEvents-Schema](../event-grid/cloudevents-schema.md) übermittelt wird. Verwenden Sie stattdessen einen HTTP-Trigger.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-2x"></a>Pakete: Functions 2.x

Der Event Grid-Trigger wird im NuGet-Paket [Microsoft.Azure.WebJobs.Extensions.EventGrid](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventGrid), Version 2.x bereitgestellt. Den Quellcode für das Paket finden Sie im GitHub-Repository [azure-functions-eventgrid-extension](https://github.com/Azure/azure-functions-eventgrid-extension/tree/v2.x).

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="packages---functions-1x"></a>Pakete: Functions 1.x

Der Event Grid-Trigger wird im NuGet-Paket [Microsoft.Azure.WebJobs.Extensions.EventGrid](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventGrid), Version 1.x bereitgestellt. Den Quellcode für das Paket finden Sie im GitHub-Repository [azure-functions-eventgrid-extension](https://github.com/Azure/azure-functions-eventgrid-extension/tree/master).

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="example"></a>Beispiel

Siehe das jeweilige sprachspezifische Beispiel für einen Event Grid-Trigger:

* C#
* [C#-Skript (.csx)](#c-script-example)
* [Java](#trigger---java-examples)
* [JavaScript](#javascript-example)
* [Python](#python-example)

Ein Beispiel für einen HTTP-Trigger finden Sie unter [Verwenden eines HTTP-Triggers als Event Grid-Trigger](#use-an-http-trigger-as-an-event-grid-trigger) weiter unten in diesem Artikel.

### <a name="c-2x"></a>C# (2.x)

Das folgende Beispiel zeigt eine Functions 2.x-[C#-Funktion](functions-dotnet-class-library.md) für die Bindung an `EventGridEvent`:

```cs
using Microsoft.Azure.EventGrid.Models;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.EventGrid;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;

namespace Company.Function
{
    public static class EventGridTriggerCSharp
    {
        [FunctionName("EventGridTest")]
        public static void EventGridTest([EventGridTrigger]EventGridEvent eventGridEvent, ILogger log)
        {
            log.LogInformation(eventGridEvent.Data.ToString());
        }
    }
}
```

Weitere Informationen finden Sie unter Pakete, [Attribute](#attributes), [Konfiguration](#configuration) und [Verwendung](#usage).

### <a name="c-version-1x"></a>C# (Version 1.x)

Das folgende Beispiel zeigt eine Functions 1.x-[C#-Funktion](functions-dotnet-class-library.md) für die Bindung an `JObject`:

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.EventGrid;
using Microsoft.Azure.WebJobs.Host;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Microsoft.Extensions.Logging;

namespace Company.Function
{
    public static class EventGridTriggerCSharp
    {
        [FunctionName("EventGridTriggerCSharp")]
        public static void Run([EventGridTrigger]JObject eventGridEvent, ILogger log)
        {
            log.LogInformation(eventGridEvent.ToString(Formatting.Indented));
        }
    }
}
```

### <a name="c-script-example"></a>C#-Skriptbeispiel

Das folgende Beispiel zeigt eine Triggerbindung in einer Datei *function.json* sowie eine [C#-Skriptfunktion](functions-reference-csharp.md), die die Bindung verwendet.

Bindungsdaten in der Datei *function.json*:

```json
{
  "bindings": [
    {
      "type": "eventGridTrigger",
      "name": "eventGridEvent",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

#### <a name="c-script-version-2x"></a>C#-Skript (Version 2.x)

Functions 2.x-C#-Skriptcode für die Bindung an `EventGridEvent`:

```csharp
#r "Microsoft.Azure.EventGrid"
using Microsoft.Azure.EventGrid.Models;
using Microsoft.Extensions.Logging;

public static void Run(EventGridEvent eventGridEvent, ILogger log)
{
    log.LogInformation(eventGridEvent.Data.ToString());
}
```

Weitere Informationen finden Sie unter Pakete, [Attribute](#attributes), [Konfiguration](#configuration) und [Verwendung](#usage).

#### <a name="c-script-version-1x"></a>C#-Skript (Version 1.x)

Functions 1.x-C#-Skriptcode für die Bindung an `JObject`:

```cs
#r "Newtonsoft.Json"

using Newtonsoft.Json;
using Newtonsoft.Json.Linq;

public static void Run(JObject eventGridEvent, TraceWriter log)
{
    log.Info(eventGridEvent.ToString(Formatting.Indented));
}
```

### <a name="javascript-example"></a>JavaScript-Beispiel

Das folgende Beispiel zeigt eine Triggerbindung in einer Datei *function.json* sowie eine [JavaScript-Funktion](functions-reference-node.md), die die Bindung verwendet.

Bindungsdaten in der Datei *function.json*:

```json
{
  "bindings": [
    {
      "type": "eventGridTrigger",
      "name": "eventGridEvent",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

Der JavaScript-Code sieht wie folgt aus:

```javascript
module.exports = function (context, eventGridEvent) {
    context.log("JavaScript Event Grid function processed a request.");
    context.log("Subject: " + eventGridEvent.subject);
    context.log("Time: " + eventGridEvent.eventTime);
    context.log("Data: " + JSON.stringify(eventGridEvent.data));
    context.done();
};
```

### <a name="python-example"></a>Beispiel für Python

Das folgende Beispiel zeigt eine Triggerbindung in einer Datei *function.json* sowie eine [Python-Funktion](functions-reference-python.md), die die Bindung verwendet.

Bindungsdaten in der Datei *function.json*:

```json
{
  "bindings": [
    {
      "type": "eventGridTrigger",
      "name": "event",
      "direction": "in"
    }
  ],
  "disabled": false,
  "scriptFile": "__init__.py"
}
```

Dies ist der Python-Code:

```python
import logging
import azure.functions as func


def main(event: func.EventGridEvent):
    logging.info("Python Event Grid function processed a request.")
    logging.info("  Subject: %s", event.subject)
    logging.info("  Time: %s", event.event_time)
    logging.info("  Data: %s", event.get_json())
```

### <a name="trigger---java-examples"></a>Trigger: Java-Beispiele

Dieser Abschnitt enthält folgende Beispiele:

* [Event Grid-Trigger, String-Parameter](#event-grid-trigger-string-parameter-java)
* [Event Grid-Trigger, POJO-Parameter](#event-grid-trigger-pojo-parameter-java)

Die folgenden Beispiele zeigen die Triggerbindung in einer *function.json*-Datei und [Java-Funktionen](functions-reference-java.md), die die Bindung verwenden und ein Ereignis ausgeben. Dabei erhalten sie das Ereignis erst als ```String``` und dann als POJO.

```json
{
  "bindings": [
    {
      "type": "eventGridTrigger",
      "name": "eventGridEvent",
      "direction": "in"
    }
  ]
}
```

#### <a name="event-grid-trigger-string-parameter-java"></a>Event Grid-Trigger, String-Parameter (Java)

```java
  @FunctionName("eventGridMonitorString")
  public void logEvent(
    @EventGridTrigger(
      name = "event"
    ) 
    String content, 
    final ExecutionContext context) {
      context.getLogger().info("Event content: " + content);      
  }
```

#### <a name="event-grid-trigger-pojo-parameter-java"></a>Event Grid-Trigger, POJO-Parameter (Java)

In diesem Beispiel werden die folgenden POJO verwendet, die die Eigenschaften der obersten Ebene eines Event Grid-Ereignisses darstellen:

```java
import java.util.Date;
import java.util.Map;

public class EventSchema {

  public String topic;
  public String subject;
  public String eventType;
  public Date eventTime;
  public String id;
  public String dataVersion;
  public String metadataVersion;
  public Map<String, Object> data;

}
```

Beim Eingang wird die JSON-Nutzlast des Ereignisses in das ```EventSchema```-POJO deserialisiert, damit dieses von der Funktion verwendet werden kann. Dadurch kann die Funktion auf die Ereigniseigenschaften auf objektorientierte Weise zugreifen.

```java
  @FunctionName("eventGridMonitor")
  public void logEvent(
    @EventGridTrigger(
      name = "event"
    ) 
    EventSchema event, 
    final ExecutionContext context) {
      context.getLogger().info("Event content: ");
      context.getLogger().info("Subject: " + event.subject);
      context.getLogger().info("Time: " + event.eventTime); // automatically converted to Date by the runtime
      context.getLogger().info("Id: " + event.id);
      context.getLogger().info("Data: " + event.data);
  }
```

Verwenden Sie die `EventGridTrigger`-Anmerkung in der [Laufzeitbibliothek für Java-Funktionen](/java/api/overview/azure/functions/runtime) für Parameter, deren Wert von Event Grid empfangen wird. Parameter mit diesen Anmerkungen führen dazu, dass die Funktion ausgeführt wird, wenn ein Ereignis empfangen wird.  Diese Anmerkung kann mit nativen Java-Typen, POJOs oder Werten mit `Optional<T>`, die NULL-Werte annehmen können, verwendet werden.

## <a name="attributes"></a>Attribute

In [C#-Klassenbibliotheken](functions-dotnet-class-library.md) verwenden Sie das [EventGridTrigger](https://github.com/Azure/azure-functions-eventgrid-extension/blob/master/src/EventGridExtension/TriggerBinding/EventGridTriggerAttribute.cs)-Attribut.

Dies ist ein `EventGridTrigger`-Attribut in einer Methodensignatur:

```csharp
[FunctionName("EventGridTest")]
public static void EventGridTest([EventGridTrigger] JObject eventGridEvent, ILogger log)
{
    ...
}
```

Ein vollständiges Beispiel finden Sie unter „C#-Beispiel“.

## <a name="configuration"></a>Konfiguration

Die folgende Tabelle gibt Aufschluss über die Bindungskonfigurationseigenschaften, die Sie in der Datei *function.json* festlegen. Im Attribut `EventGridTrigger` müssen keine Konstruktorparameter oder -eigenschaften festgelegt werden.

|Eigenschaft von „function.json“ |BESCHREIBUNG|
|---------|---------|
| **type** | Erforderlich – muss auf `eventGridTrigger` festgelegt sein. |
| **direction** | Erforderlich – muss auf `in` festgelegt sein. |
| **name** | Erforderlich – der Variablenname, der im Funktionscode für den Parameter verwendet wird, der die Ereignisdaten empfängt. |

## <a name="usage"></a>Verwendung

In C#- und F#-Funktionen in Azure Functions 1.x können Sie die folgenden Parametertypen für den Event Grid-Trigger verwenden:

* `JObject`
* `string`

In C#- und F#-Funktionen in Azure Functions 2.x können Sie optional auch den folgenden Parametertyp für den Event Grid-Trigger verwenden:

* `Microsoft.Azure.EventGrid.Models.EventGridEvent` definiert Eigenschaften für die Felder, die für alle Ereignistypen gelten.

> [!NOTE]
> Wenn Sie in Functions v1 versuchen, eine Bindung an `Microsoft.Azure.WebJobs.Extensions.EventGrid.EventGridEvent` herzustellen, zeigt der Compiler eine „Veraltet“-Meldung an, und weist Sie an, stattdessen `Microsoft.Azure.EventGrid.Models.EventGridEvent` zu verwenden. Verweisen Sie zum Verwenden des neueren Typs auf das [Microsoft.Azure.EventGrid](https://www.nuget.org/packages/Microsoft.Azure.EventGrid)-NuGet-Paket, und qualifizieren Sie den `EventGridEvent`-Typnamen vollständig, indem Sie ihm das Präfix `Microsoft.Azure.EventGrid.Models` voranstellen. Informationen zum Verweisen auf NuGet-Pakete in einer C#-Skriptfunktion finden Sie unter [Verwenden von NuGet-Paketen](functions-reference-csharp.md#using-nuget-packages).

Bei JavaScript-Funktionen enthält der durch die `name`-Eigenschaft in *function.json* benannte Parameter einen Verweis auf das Ereignisobjekt.

## <a name="event-schema"></a>Ereignisschema

Daten für ein Event Grid-Ereignis werden als JSON-Objekt im Text einer HTTP-Anforderung empfangen. Das JSON-Objekt ähnelt dem folgenden Beispiel:

```json
[{
  "topic": "/subscriptions/{subscriptionid}/resourceGroups/eg0122/providers/Microsoft.Storage/storageAccounts/egblobstore",
  "subject": "/blobServices/default/containers/{containername}/blobs/blobname.jpg",
  "eventType": "Microsoft.Storage.BlobCreated",
  "eventTime": "2018-01-23T17:02:19.6069787Z",
  "id": "{guid}",
  "data": {
    "api": "PutBlockList",
    "clientRequestId": "{guid}",
    "requestId": "{guid}",
    "eTag": "0x8D562831044DDD0",
    "contentType": "application/octet-stream",
    "contentLength": 2248,
    "blobType": "BlockBlob",
    "url": "https://egblobstore.blob.core.windows.net/{containername}/blobname.jpg",
    "sequencer": "000000000000272D000000000003D60F",
    "storageDiagnostics": {
      "batchId": "{guid}"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

Das Beispiel stellt ein Array mit einem Element dar. Event Grid sendet immer ein Array und kann mehrere Ereignisse im Array senden. Die Runtime ruft die Funktion einmal für jedes Arrayelement auf.

Die Eigenschaften der obersten Ebene in den JSON-Ereignisdaten sind für alle Ereignistypen identisch, die Inhalte der Eigenschaft `data` sind dagegen für jeden Ereignistyp spezifisch. Das Beispiel oben bezieht sich auf ein Blob Storage-Ereignis.

Erläuterungen zu den allgemeinen und ereignisspezifischen Eigenschaften finden Sie unter [Ereigniseigenschaften](../event-grid/event-schema.md#event-properties) in der Event Grid-Dokumentation.

Mit dem Typ `EventGridEvent` werden lediglich die Eigenschaften der obersten Ebene definiert. Bei der `Data`-Eigenschaft handelt es sich um ein `JObject`.

## <a name="create-a-subscription"></a>Erstellen eines Abonnements

Erstellen Sie für den Empfang von HTTP-Anforderungen in Event Grid ein Event Grid-Abonnement, das die Endpunkt-URL angibt, über die die Funktion aufgerufen wird.

### <a name="azure-portal"></a>Azure-Portal

Wählen Sie für Funktionen, die Sie im Azure-Portal mit dem Event Grid-Trigger entwickeln, die Option **Event Grid-Abonnement hinzufügen** aus.

![Erstellen eines Abonnements im Portal](media/functions-bindings-event-grid/portal-sub-create.png)

Bei Auswahl dieses Links wird das Portal mit der Seite **Ereignisabonnement erstellen** geöffnet, auf der die Endpunkt-URL bereits ausgefüllt ist.

![Bereits ausgefüllte Endpunkt-URL](media/functions-bindings-event-grid/endpoint-url.png)

Weitere Informationen zum Erstellen von Abonnements über das Azure-Portal finden Sie unter [Erstellen eines benutzerdefinierten Ereignisses – Azure-Portal](../event-grid/custom-event-quickstart-portal.md) in der Event Grid-Dokumentation.

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Verwenden Sie den Befehl [az eventgrid event-subscription create](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az-eventgrid-event-subscription-create), um ein Abonnement über die [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest) zu erstellen.

Für den Befehl ist die Endpunkt-URL erforderlich, über die die Funktion aufgerufen wird. Das folgende Beispiel zeigt das versionsabhängige URL-Muster:

#### <a name="version-2x-runtime"></a>Laufzeit der Version 2.x

    https://{functionappname}.azurewebsites.net/runtime/webhooks/eventgrid?functionName={functionname}&code={systemkey}

#### <a name="version-1x-runtime"></a>Laufzeit der Version 1.x

    https://{functionappname}.azurewebsites.net/admin/extensions/EventGridExtensionConfig?functionName={functionname}&code={systemkey}

Der Systemschlüssel ist ein Autorisierungsschlüssel, der in der Endpunkt-URL für einen Event Grid-Trigger eingefügt werden muss. Im folgenden Abschnitt wird erläutert, wie der Systemschlüssel abgerufen wird.

Beispiel, in dem ein Blob Storage-Konto abonniert wird (mit einem Platzhalter für den Systemschlüssel):

#### <a name="version-2x-runtime"></a>Laufzeit der Version 2.x

```azurecli
az eventgrid resource event-subscription create -g myResourceGroup \
--provider-namespace Microsoft.Storage --resource-type storageAccounts \
--resource-name myblobstorage12345 --name myFuncSub  \
--included-event-types Microsoft.Storage.BlobCreated \
--subject-begins-with /blobServices/default/containers/images/blobs/ \
--endpoint https://mystoragetriggeredfunction.azurewebsites.net/runtime/webhooks/eventgrid?functionName=imageresizefunc&code=<key>
```

#### <a name="version-1x-runtime"></a>Laufzeit der Version 1.x

```azurecli
az eventgrid resource event-subscription create -g myResourceGroup \
--provider-namespace Microsoft.Storage --resource-type storageAccounts \
--resource-name myblobstorage12345 --name myFuncSub  \
--included-event-types Microsoft.Storage.BlobCreated \
--subject-begins-with /blobServices/default/containers/images/blobs/ \
--endpoint https://mystoragetriggeredfunction.azurewebsites.net/admin/extensions/EventGridExtensionConfig?functionName=imageresizefunc&code=<key>
```

Weitere Informationen zum Erstellen eines Abonnements finden Sie im [Schnellstart für Blob Storage](../storage/blobs/storage-blob-event-quickstart.md#subscribe-to-your-storage-account) oder in den anderen Schnellstarts für Event Grid.

### <a name="get-the-system-key"></a>Abrufen des Systemschlüssels

Sie können den Systemschlüssel mithilfe der folgenden API (HTTP GET) abrufen:

#### <a name="version-2x-runtime"></a>Laufzeit der Version 2.x

```
http://{functionappname}.azurewebsites.net/admin/host/systemkeys/eventgrid_extension?code={masterkey}
```

#### <a name="version-1x-runtime"></a>Laufzeit der Version 1.x

```
http://{functionappname}.azurewebsites.net/admin/host/systemkeys/eventgridextensionconfig_extension?code={masterkey}
```

Dabei handelt es sich um eine Admin-API, für die der [Masterschlüssel](functions-bindings-http-webhook.md#authorization-keys) Ihrer Funktion erforderlich ist. Verwechseln Sie den Systemschlüssel (zum Aufrufen einer Event Grid-Triggerfunktion) nicht mit dem Masterschlüssel (zum Ausführen von Verwaltungsaufgaben in der Funktions-App). Verwenden Sie zum Abonnieren eines Event Grid-Themas immer den Systemschlüssel.

Beispiel für die Antwort, die den Systemschlüssel enthält:

```
{
  "name": "eventgridextensionconfig_extension",
  "value": "{the system key for the function}",
  "links": [
    {
      "rel": "self",
      "href": "{the URL for the function, without the system key}"
    }
  ]
}
```

Sie erhalten den Hauptschlüssel für Ihre Funktionen-App über die Registerkarte **Funktionen-App-Einstellungen** im Portal.

> [!IMPORTANT]
> Der Hauptschlüssel bietet Administratorzugriff auf Ihre Funktionen-App. Geben Sie diesen Schlüssel nicht an Dritte weiter und verteilen Sie ihn nicht in nativen Clientanwendungen.

Weitere Informationen finden Sie unter [Autorisierungsschlüssel](functions-bindings-http-webhook.md#authorization-keys) im Referenzartikel zu HTTP-Triggern.

Alternativ können Sie eine HTTP-PUT-Anforderung senden, um den Schlüsselwert selbst anzugeben.

## <a name="local-testing-with-viewer-web-app"></a>Lokales Testen mit Viewer-Web-App

Zum lokalen Testen eines Event Grid-Triggers müssen Event Grid-HTTP-Anforderungen von ihrem Ursprung in der Cloud an Ihren lokalen Computer gesendet werden. Dies kann erfolgen, indem Anforderungen online erfasst und manuell erneut an Ihren lokalen Computer gesendet werden:

1. [Erstellen Sie eine Viewer-Web-App](#create-a-viewer-web-app), die Ereignisnachrichten erfasst.
1. [Erstellen Sie ein Event Grid-Abonnement](#create-an-event-grid-subscription), mit dem Ereignisse an die Viewer-App gesendet werden.
1. [Generieren Sie eine Anforderung](#generate-a-request), und kopieren Sie den Anforderungstext von der Viewer-App.
1. [Stellen Sie die Anforderung manuell](#manually-post-the-request) für die localhost-URL der Event Grid-Triggerfunktion bereit.

Nach Abschluss der Tests können Sie dasselbe Abonnement für die Produktion verwenden, indem Sie den Endpunkt aktualisieren. Verwenden Sie dazu den Azure CLI-Befehl [az eventgrid event-subscription update](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az-eventgrid-event-subscription-update).

### <a name="create-a-viewer-web-app"></a>Erstellen einer Viewer-Web-App

Zum einfachen Erfassen von Ereignisnachrichten können Sie eine [vorgefertigte Web-App](https://github.com/Azure-Samples/azure-event-grid-viewer) bereitstellen, die die Ereignisnachrichten anzeigt. Die bereitgestellte Lösung umfasst einen App Service-Plan, eine App Service-Web-App und Quellcode von GitHub.

Wählen Sie **Deploy to Azure** (In Azure bereitstellen), um die Lösung für Ihr Abonnement bereitzustellen. Geben Sie im Azure-Portal Werte für die Parameter an.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fazure-event-grid-viewer%2Fmaster%2Fazuredeploy.json" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"/></a>

Die Bereitstellung kann einige Minuten dauern. Nach erfolgreichem Abschluss der Bereitstellung können Sie Ihre Web-App anzeigen und sich vergewissern, dass sie ausgeführt wird. Navigieren Sie hierzu in einem Webbrowser zu `https://<your-site-name>.azurewebsites.net`.

Die Website wird angezeigt, aber es wurden noch keine Ereignisse bereitgestellt.

![Anzeigen der neuen Website](media/functions-bindings-event-grid/view-site.png)

### <a name="create-an-event-grid-subscription"></a>Erstellen eines Event Grid-Abonnements

Erstellen Sie ein Event Grid-Abonnement des Typs, den Sie testen möchten, und geben Sie die URL Ihrer Web-App als Endpunkt für die Ereignisbenachrichtigung an. Der Endpunkt für Ihre Web-App muss das Suffix `/api/updates/` enthalten. Daher lautet die vollständige URL `https://<your-site-name>.azurewebsites.net/api/updates`.

Informationen zum Erstellen von Abonnements über das Azure-Portal finden Sie unter [Erstellen eines benutzerdefinierten Ereignisses – Azure-Portal](../event-grid/custom-event-quickstart-portal.md) in der Event Grid-Dokumentation.

### <a name="generate-a-request"></a>Generieren einer Anforderung

Lösen Sie ein Ereignis aus, mit dem HTTP-Datenverkehr für Ihren Web-App-Endpunkt generiert wird.  Wenn Sie z.B. ein Blob Storage-Abonnement erstellt haben, können Sie ein Blob hochladen oder löschen. Wenn eine Anforderung in Ihrer Web-App angezeigt wird, kopieren Sie den Anforderungstext.

Die Anforderung zur Abonnementüberprüfung wird zuerst empfangen. Ignorieren Sie sämtliche Überprüfungsanforderungen, und kopieren Sie die Ereignisanforderung.

![Kopieren des Anforderungstexts aus Web-App](media/functions-bindings-event-grid/view-results.png)

### <a name="manually-post-the-request"></a>Manuelles Bereitstellen der Anforderung

Führen Sie die Event Grid-Funktion lokal aus.

Verwenden Sie ein Tool wie z.B. [Postman](https://www.getpostman.com/) oder [cURL](https://curl.haxx.se/docs/httpscripting.html), um eine HTTP POST-Anforderung zu erstellen:

* Legen Sie einen `Content-Type: application/json`-Header fest.
* Legen Sie einen `aeg-event-type: Notification`-Header fest.
* Fügen Sie die RequestBin-Daten im Anforderungstext ein.
* Stellen Sie die Anforderung für die URL der Event Grid-Triggerfunktion bereit.
  * Verwenden Sie für 2.x das folgende Muster:

    ```
    http://localhost:7071/runtime/webhooks/eventgrid?functionName={FUNCTION_NAME}
    ```

  * Verwenden Sie für 1.x Folgendes:

    ```
    http://localhost:7071/admin/extensions/EventGridExtensionConfig?functionName={FUNCTION_NAME}
    ```

Der `functionName`-Parameter muss der im `FunctionName`-Attribut angegebene Name sein.

In den folgenden Screenshots sind die Header und der Anforderungstext in Postman dargestellt:

![Header in Postman](media/functions-bindings-event-grid/postman2.png)

![Anforderungstext in Postman](media/functions-bindings-event-grid/postman.png)

Mit der Event Grid-Triggerfunktion werden Protokolle ähnlich denen im folgenden Beispiel ausgeführt und angezeigt:

![Beispielprotokolle der Event Grid-Triggerfunktion](media/functions-bindings-event-grid/eg-output.png)

## <a name="local-testing-with-ngrok"></a>Lokales Testen mit ngrok

Eine weitere Möglichkeit zum lokalen Testen eines Event Grid-Triggers ist die Automatisierung der HTTP-Verbindung zwischen dem Internet und dem Entwicklungscomputer. Dies können Sie mit einem Tool wie [ngrok](https://ngrok.com/) tun:

1. [Erstellen Sie einen ngrok-Endpunkt](#create-an-ngrok-endpoint).
1. [Führen Sie die Event Grid-Triggerfunktion aus](#run-the-event-grid-trigger-function).
1. [Erstellen Sie ein Event Grid-Abonnement](#create-a-subscription), mit dem Ereignisse an den ngrok-Endpunkt gesendet werden.
1. [Lösen Sie ein Ereignis aus](#trigger-an-event).

Nach Abschluss der Tests können Sie dasselbe Abonnement für die Produktion verwenden, indem Sie den Endpunkt aktualisieren. Verwenden Sie dazu den Azure CLI-Befehl [az eventgrid event-subscription update](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az-eventgrid-event-subscription-update).

### <a name="create-an-ngrok-endpoint"></a>Erstellen eines ngrok-Endpunkts

Laden Sie die Datei *ngrok.exe* von [ngrok](https://ngrok.com/) herunter, und führen Sie sie mit dem folgenden Befehl aus:

```
ngrok http -host-header=localhost 7071
```

Der Parameter „-host-header“ ist erforderlich, da die Functions-Runtime Anforderungen von „localhost“ erwartet, wenn sie auf „localhost“ ausgeführt wird. Wenn die Runtime lokal ausgeführt wird, ist 7071 die Standardportnummer.

Mit dem Befehl wird eine Ausgabe ähnlich der folgenden erstellt:

```
Session Status                online
Version                       2.2.8
Region                        United States (us)
Web Interface                 http://127.0.0.1:4040
Forwarding                    http://263db807.ngrok.io -> localhost:7071
Forwarding                    https://263db807.ngrok.io -> localhost:7071

Connections                   ttl     opn     rt1     rt5     p50     p90
                              0       0       0.00    0.00    0.00    0.00
```

Verwenden Sie die URL `https://{subdomain}.ngrok.io` für Ihr Event Grid-Abonnement.

### <a name="run-the-event-grid-trigger-function"></a>Ausführen der Event Grid-Triggerfunktion

Die ngrok-URL erhält in Event Grid keine spezielle Behandlung, daher muss die Funktion bei der Erstellung des Abonnements lokal ausgeführt werden. Wenn dies nicht der Fall ist, wird die Überprüfungsantwort nicht gesendet, und bei der Erstellung des Abonnements treten Fehler auf.

### <a name="create-a-subscription"></a>Erstellen eines Abonnements

Erstellen Sie ein Event Grid-Abonnement des Abonnementtyps, der getestet werden soll, und legen Sie für das Abonnement Ihren ngrok-Endpunkt fest.

Verwenden Sie dieses Endpunktmuster für Functions 2.x:

```
https://{SUBDOMAIN}.ngrok.io/runtime/webhooks/eventgrid?functionName={FUNCTION_NAME}
```

Verwenden Sie dieses Endpunktmuster für Functions 1.x:

```
https://{SUBDOMAIN}.ngrok.io/admin/extensions/EventGridExtensionConfig?functionName={FUNCTION_NAME}
```

Der `{FUNCTION_NAME}`-Parameter muss der im `FunctionName`-Attribut angegebene Name sein.

Beispiel für die Verwendung der Azure-Befehlszeilenschnittstelle:

```azurecli
az eventgrid event-subscription create --resource-id /subscriptions/aeb4b7cb-b7cb-b7cb-b7cb-b7cbb6607f30/resourceGroups/eg0122/providers/Microsoft.Storage/storageAccounts/egblobstor0122 --name egblobsub0126 --endpoint https://263db807.ngrok.io/runtime/webhooks/eventgrid?functionName=EventGridTrigger
```

Informationen zum Erstellen eines Abonnements finden Sie unter [Erstellen eines Abonnements](#create-a-subscription) weiter oben in diesem Artikel.

### <a name="trigger-an-event"></a>Auslösen eines Ereignisses

Lösen Sie ein Ereignis aus, mit dem HTTP-Datenverkehr für den ngrok-Endpunkt generiert wird.  Wenn Sie z.B. ein Blob Storage-Abonnement erstellt haben, können Sie ein Blob hochladen oder löschen.

Mit der Event Grid-Triggerfunktion werden Protokolle ähnlich denen im folgenden Beispiel ausgeführt und angezeigt:

![Beispielprotokolle der Event Grid-Triggerfunktion](media/functions-bindings-event-grid/eg-output.png)

## <a name="use-an-http-trigger-as-an-event-grid-trigger"></a>Verwenden eines HTTP-Triggers als Event Grid-Trigger

Event Grid-Ereignisse werden als HTTP-Anforderungen empfangen. Somit können Ereignisse unter Verwendung eines HTTP-Triggers anstelle eines Event Grid-Triggers verarbeitet werden. Ein möglicher Grund dafür ist die bessere Steuerung der Endpunkt-URL, über die die Funktion aufgerufen wird. Ein weiterer Grund ist, wenn Sie Ereignisse im [CloudEvents-Schema](../event-grid/cloudevents-schema.md) empfangen müssen. Das CloudEvents-Schema wird vom Event Grid-Trigger derzeit nicht unterstützt. Die Beispiele in diesem Abschnitt zeigen Lösungen für das Event Grid-Schema und für das CloudEvents-Schema auf.

Bei Verwendung eines HTTP-Triggers müssen Sie Code für folgende Vorgänge schreiben, die über den Event Grid-Trigger automatisch ausgeführt werden:

* Senden einer Überprüfungsantwort an eine [Anforderung zur Abonnementüberprüfung](../event-grid/security-authentication.md#webhook-event-delivery)
* Aufrufen der Funktion einmal pro Element des im Anforderungstext enthaltenen Ereignisarrays

Informationen zu der zum lokalen Aufrufen der Funktion verwendeten URL oder ihrer Ausführung in Azure finden Sie in der [Referenzdokumentation zu HTTP-Triggerbindungen](functions-bindings-http-webhook.md).

### <a name="event-grid-schema"></a>Event Grid-Schema

Mit dem folgenden C#-Beispielcode für einen HTTP-Trigger wird das Verhalten eines Event Grid-Triggers simuliert. Verwenden Sie dieses Beispiel für Ereignisse, die im Event Grid-Schema bereitgestellt werden.

```csharp
[FunctionName("HttpTrigger")]
public static async Task<HttpResponseMessage> Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]HttpRequestMessage req,
    ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    var messages = await req.Content.ReadAsAsync<JArray>();

    // If the request is for subscription validation, send back the validation code.
    if (messages.Count > 0 && string.Equals((string)messages[0]["eventType"],
        "Microsoft.EventGrid.SubscriptionValidationEvent",
        System.StringComparison.OrdinalIgnoreCase))
    {
        log.LogInformation("Validate request received");
        return req.CreateResponse<object>(new
        {
            validationResponse = messages[0]["data"]["validationCode"]
        });
    }

    // The request is not for subscription validation, so it's for one or more events.
    foreach (JObject message in messages)
    {
        // Handle one event.
        EventGridEvent eventGridEvent = message.ToObject<EventGridEvent>();
        log.LogInformation($"Subject: {eventGridEvent.Subject}");
        log.LogInformation($"Time: {eventGridEvent.EventTime}");
        log.LogInformation($"Event data: {eventGridEvent.Data.ToString()}");
    }

    return req.CreateResponse(HttpStatusCode.OK);
}
```

Mit dem folgenden JavaScript-Beispielcode für einen HTTP-Trigger wird das Verhalten eines Event Grid-Triggers simuliert. Verwenden Sie dieses Beispiel für Ereignisse, die im Event Grid-Schema bereitgestellt werden.

```javascript
module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');

    var messages = req.body;
    // If the request is for subscription validation, send back the validation code.
    if (messages.length > 0 && messages[0].eventType == "Microsoft.EventGrid.SubscriptionValidationEvent") {
        context.log('Validate request received');
        var code = messages[0].data.validationCode;
        context.res = { status: 200, body: { "ValidationResponse": code } };
    }
    else {
        // The request is not for subscription validation, so it's for one or more events.
        // Event Grid schema delivers events in an array.
        for (var i = 0; i < messages.length; i++) {
            // Handle one event.
            var message = messages[i];
            context.log('Subject: ' + message.subject);
            context.log('Time: ' + message.eventTime);
            context.log('Data: ' + JSON.stringify(message.data));
        }
    }
    context.done();
};
```

Der Code zur Ereignisbehandlung wird über das `messages`-Array in die Schleife eingefügt.

### <a name="cloudevents-schema"></a>CloudEvents-Schema

Mit dem folgenden C#-Beispielcode für einen HTTP-Trigger wird das Verhalten eines Event Grid-Triggers simuliert.  Verwenden Sie dieses Beispiel für Ereignisse, die im CloudEvents-Schema bereitgestellt werden.

```csharp
[FunctionName("HttpTrigger")]
public static async Task<HttpResponseMessage> Run([HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]HttpRequestMessage req, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    var requestmessage = await req.Content.ReadAsStringAsync();
    var message = JToken.Parse(requestmessage);

    if (message.Type == JTokenType.Array)
    {
        // If the request is for subscription validation, send back the validation code.
        if (string.Equals((string)message[0]["eventType"],
        "Microsoft.EventGrid.SubscriptionValidationEvent",
        System.StringComparison.OrdinalIgnoreCase))
        {
            log.LogInformation("Validate request received");
            return req.CreateResponse<object>(new
            {
                validationResponse = message[0]["data"]["validationCode"]
            });
        }
    }
    else
    {
        // The request is not for subscription validation, so it's for an event.
        // CloudEvents schema delivers one event at a time.
        log.LogInformation($"Source: {message["source"]}");
        log.LogInformation($"Time: {message["eventTime"]}");
        log.LogInformation($"Event data: {message["data"].ToString()}");
    }

    return req.CreateResponse(HttpStatusCode.OK);
}
```

Mit dem folgenden JavaScript-Beispielcode für einen HTTP-Trigger wird das Verhalten eines Event Grid-Triggers simuliert. Verwenden Sie dieses Beispiel für Ereignisse, die im CloudEvents-Schema bereitgestellt werden.

```javascript
module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');

    var message = req.body;
    // If the request is for subscription validation, send back the validation code.
    if (message.length > 0 && message[0].eventType == "Microsoft.EventGrid.SubscriptionValidationEvent") {
        context.log('Validate request received');
        var code = message[0].data.validationCode;
        context.res = { status: 200, body: { "ValidationResponse": code } };
    }
    else {
        // The request is not for subscription validation, so it's for an event.
        // CloudEvents schema delivers one event at a time.
        var event = JSON.parse(message);
        context.log('Source: ' + event.source);
        context.log('Time: ' + event.eventTime);
        context.log('Data: ' + JSON.stringify(event.data));
    }
    context.done();
};
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Konzepte für Azure Functions-Trigger und -Bindungen](functions-triggers-bindings.md)

> [!div class="nextstepaction"]
> [Weitere Informationen zu Event Grid](../event-grid/overview.md)
