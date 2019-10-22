---
title: Herstellen einer Verbindung zwischen Ihrer Java-Funktion und Azure Storage
description: Hier erfahren Sie, wie Sie eine per HTTP ausgelöste Java-Funktion mithilfe einer Queue Storage-Ausgabebindung mit Azure Storage verbinden.
author: ggailey777
ms.author: glenga
ms.date: 10/14/2019
ms.topic: quickstart
ms.service: azure-functions
manager: gwallace
ms.openlocfilehash: c78630af7d09cc911862c8e823c5dfeee9cabbd9
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72333402"
---
# <a name="connect-your-java-function-to-azure-storage"></a>Herstellen einer Verbindung zwischen Ihrer Java-Funktion und Azure Storage

[!INCLUDE [functions-add-storage-binding-intro](../../includes/functions-add-storage-binding-intro.md)]

In diesem Artikel erfahren Sie, wie Sie die Funktion, die Sie im [vorherigen Schnellstartartikel](functions-create-first-java-maven.md) erstellt haben, mit einer Azure Storage-Warteschlange integrieren. Die Ausgabebindung, die Sie dieser Funktion hinzufügen, schreibt Daten aus einer HTTP-Anforderung in eine Nachricht in der Warteschlange.

Die meisten Bindungen erfordern eine gespeicherte Verbindungszeichenfolge, die Functions verwendet, um auf den gebundenen Dienst zuzugreifen. Um diese Verbindung zu vereinfachen, verwenden Sie das Storage-Konto, das Sie mit Ihrer Funktions-App erstellt haben. Die Verbindung mit diesem Konto ist bereits in einer App-Einstellung namens `AzureWebJobsStorage` gespeichert.  

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die Schritte in [Teil 1 der Java-Schnellstartanleitung](functions-create-first-java-maven.md) aus, bevor Sie mit diesem Artikel beginnen.

## <a name="download-the-function-app-settings"></a>Herunterladen der Funktions-App-Einstellungen

[!INCLUDE [functions-app-settings-download-cli](../../includes/functions-app-settings-download-local-cli.md)]

## <a name="enable-extension-bundles"></a>Aktivieren von Erweiterungsbundles

[!INCLUDE [functions-extension-bundles](../../includes/functions-extension-bundles.md)]

Jetzt können Sie dem Projekt die Storage-Ausgabebindung hinzufügen.

## <a name="add-an-output-binding"></a>Hinzufügen einer Ausgabebindung

In einem Java-Projekt werden die Bindungen als Bindungsanmerkungen für die Funktionsmethode definiert. Die Datei *function.json* wird dann automatisch auf der Grundlage dieser Anmerkungen generiert.

Navigieren Sie zum Speicherort Ihres Funktionscodes (unter _src/main/java_), öffnen Sie die Projektdatei *Function.java*, und fügen Sie der Definition der Methode `run` die folgenden Parameter hinzu:

```java
@QueueOutput(name = "msg", queueName = "outqueue", connection = "AzureWebJobsStorage") OutputBinding<String> msg
```

Der Parameter `msg` ist ein [`OutputBinding<T>`](/java/api/com.microsoft.azure.functions.outputbinding)-Typ. Dieser stellt eine Sammlung von Zeichenfolgen dar, die als Nachrichten in eine Ausgabebindung geschrieben werden, wenn die Funktion abgeschlossen wird. In diesem Fall ist die Ausgabe eine Speicherwarteschlange mit dem Namen `outqueue`. Die Verbindungszeichenfolge für das Storage-Konto wird durch die Methode `connection` festgelegt. Übergeben Sie anstelle der eigentlichen Verbindungszeichenfolge die Anwendungseinstellung, die die Verbindungszeichenfolge für das Storage-Konto enthält.

Die Definition der Methode `run` sollte nun wie im folgenden Beispiel aussehen:  

```java
@FunctionName("HttpTrigger-Java")
public HttpResponseMessage run(
        @HttpTrigger(name = "req", methods = {HttpMethod.GET, HttpMethod.POST}, authLevel = AuthorizationLevel.FUNCTION)  
        HttpRequestMessage<Optional<String>> request, 
        @QueueOutput(name = "msg", queueName = "outqueue", connection = "AzureWebJobsStorage") 
        OutputBinding<String> msg, final ExecutionContext context) {
    ...
}
```

## <a name="add-code-that-uses-the-output-binding"></a>Hinzufügen von Code, der die Ausgabebindung verwendet

Nun können Sie den neuen Parameter `msg` verwenden, um von Ihrem Funktionscode aus in die Ausgabebindung zu schreiben. Fügen Sie vor der Erfolgsantwort die folgende Codezeile hinzu, um der Ausgabebindung `msg` den Wert `name` hinzuzufügen:

```java
msg.setValue(name);
```

Bei Verwendung einer Ausgabebindung müssen Sie weder den Azure Storage SDK-Code für die Authentifizierung verwenden noch einen Warteschlangenverweis abrufen oder Daten schreiben. Die Functions-Runtime und die Warteschlangenausgabebindung übernehmen diese Aufgaben für Sie.

Die Methode `run` sollte bei Ihnen nun wie im folgenden Beispiel aussehen:

```java
@FunctionName("HttpTrigger-Java")
public HttpResponseMessage run(
        @HttpTrigger(name = "req", methods = {HttpMethod.GET, HttpMethod.POST}, authLevel = AuthorizationLevel.FUNCTION) HttpRequestMessage<Optional<String>> request, 
        @QueueOutput(name = "msg", queueName = "outqueue", connection = "AzureWebJobsStorage") 
        OutputBinding<String> msg, final ExecutionContext context) {
    context.getLogger().info("Java HTTP trigger processed a request.");

    // Parse query parameter
    String query = request.getQueryParameters().get("name");
    String name = request.getBody().orElse(query);

    if (name == null) {
        return request.createResponseBuilder(HttpStatus.BAD_REQUEST).body("Please pass a name on the query string or in the request body").build();
    } else {
        // Write the name to the message queue. 
        msg.setValue(name);

        return request.createResponseBuilder(HttpStatus.OK).body("Hello, " + name).build();
    }
}
```

## <a name="update-the-tests"></a>Aktualisieren der Tests

Da der Archetyp auch eine Reihe von Tests erstellt, müssen Sie diese Tests aktualisieren, um den neuen Parameter `msg` in der Signatur der Methode `run` zu behandeln.  

Navigieren Sie zum Speicherort Ihres Testcodes (unter _src/test/java_), öffnen Sie die Projektdatei *Function.java*, und ersetzen Sie die Codezeile unter `//Invoke` durch den folgenden Code:

```java
@SuppressWarnings("unchecked")
final OutputBinding<String> msg = (OutputBinding<String>)mock(OutputBinding.class);

// Invoke
final HttpResponseMessage ret = new Function().run(req, msg, context);
``` 

Nun können Sie die neue Ausgabebindung lokal testen.

## <a name="run-the-function-locally"></a>Lokales Ausführen der Funktion

Verwenden Sie wie zuvor den folgenden Befehl, um das Projekt zu erstellen und die Functions-Runtime lokal zu starten:

```bash
mvn clean package 
mvn azure-functions:run
```

> [!NOTE]  
> Da Sie Erweiterungsbündel in der Datei „host.json“ aktiviert haben, wurde die [Storage-Bindungserweiterung](functions-bindings-storage-blob.md#packages---functions-2x) zusammen mit den übrigen Microsoft-Bindungserweiterungen während des Starts für Sie heruntergeladen und installiert.

Lösen Sie die Funktion wie zuvor über die Befehlszeile aus. Verwenden Sie dazu cURL in einem neuen Terminalfenster:

```CMD
curl -w "\n" http://localhost:7071/api/HttpTrigger-Java --data AzureFunctions
```

Diesmal erstellt die Ausgabebindung außerdem eine Warteschlange namens `outqueue` in Ihrem Speicherkonto und fügt eine Nachricht mit dieser selben Zeichenfolge hinzu.

Als Nächstes verwenden Sie die Azure CLI, um die neue Warteschlange anzuzeigen und sicherzustellen, dass eine Nachricht hinzugefügt wurde. Sie können Ihre Warteschlange auch mithilfe des [Microsoft Azure Storage-Explorers][Azure Storage Explorer] oder im [Azure-Portal](https://portal.azure.com) anzeigen.

[!INCLUDE [functions-storage-account-set-cli](../../includes/functions-storage-account-set-cli.md)]

[!INCLUDE [functions-query-storage-cli](../../includes/functions-query-storage-cli.md)]

### <a name="redeploy-the-project"></a>Erneutes Bereitstellen des Projekts 

Führen Sie zum Aktualisieren Ihrer veröffentlichten App den folgenden Befehl erneut aus:  

```azurecli
mvn azure-functions:deploy
```

Sie können auch wieder cURL verwenden, um die bereitgestellte Funktion zu testen. Übergeben Sie wie zuvor den Wert `AzureFunctions` im Text der POST-Anforderung an die URL. Beispiel:

```bash
curl -w "\n" https://fabrikam-functions-20190929094703749.azurewebsites.net/api/HttpTrigger-Java?code=zYRohsTwBlZ68YF.... --data AzureFunctions
```

Sie können [die Storage-Warteschlangennachricht erneut untersuchen](#query-the-storage-queue), um zu überprüfen, ob die Ausgabebindung wie erwartet eine neue Nachricht in der Warteschlange generiert.

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

Sie haben Ihre mittels HTTP ausgelöste Funktion so aktualisiert, dass sie Daten in eine Storage-Warteschlange schreibt. Weitere Informationen zum Entwickeln von Azure-Funktionen mit Java finden Sie im [Java-Entwicklerhandbuch für Azure Functions](functions-reference-java.md) sowie unter [Azure Functions-Trigger und -Bindungen](functions-triggers-bindings.md). Beispiele für vollständige Funktionsprojekte in Java finden Sie in den [Functions-Beispielen für Java](/samples/browse/?products=azure-functions&languages=Java). 

Als Nächstes sollten Sie die Application Insights-Überwachung für Ihre Funktions-App aktivieren:

> [!div class="nextstepaction"]
> [Aktivieren der Application Insights-Integration](functions-monitoring.md#manually-connect-an-app-insights-resource)


[Azure Storage Explorer]: https://storageexplorer.com/