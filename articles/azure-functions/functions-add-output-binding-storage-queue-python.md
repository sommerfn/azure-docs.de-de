---
title: Hinzufügen einer Azure Storage-Warteschlangenbindung zu Ihrer Python-Funktion
description: Erfahren Sie, wie Sie eine Azure Storage-Warteschlangenausgabebindung zu Ihrer Python-Funktion hinzufügen.
author: ggailey777
ms.author: glenga
ms.date: 10/02/2019
ms.topic: quickstart
ms.service: azure-functions
manager: gwallace
ms.openlocfilehash: 2307a296453247a5deee082aadb474f3641cce88
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72329729"
---
# <a name="add-an-azure-storage-queue-binding-to-your-python-function"></a>Hinzufügen einer Azure Storage-Warteschlangenbindung zu Ihrer Python-Funktion

[!INCLUDE [functions-add-storage-binding-intro](../../includes/functions-add-storage-binding-intro.md)]

In diesem Artikel erfahren Sie, wie Sie die Funktion, die Sie im [vorherigen Schnellstartartikel](functions-create-first-function-python.md) erstellt haben, mit einer Azure Storage-Warteschlange integrieren. Die Ausgabebindung, die Sie dieser Funktion hinzufügen, schreibt Daten aus einer HTTP-Anforderung in eine Nachricht in der Warteschlange.

Die meisten Bindungen erfordern eine gespeicherte Verbindungszeichenfolge, die Functions verwendet, um auf den gebundenen Dienst zuzugreifen. Um diese Verbindung zu vereinfachen, verwenden Sie das Storage-Konto, das Sie mit Ihrer Funktions-App erstellt haben. Die Verbindung mit diesem Konto ist bereits in einer App-Einstellung namens `AzureWebJobsStorage` gespeichert.  

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit diesem Artikel beginnen, führen Sie die Schritte in [Teil 1 des Python-Schnellstarts](functions-create-first-function-python.md) aus.

[!INCLUDE [functions-cloud-shell-note](../../includes/functions-cloud-shell-note.md)]

## <a name="download-the-function-app-settings"></a>Herunterladen der Funktions-App-Einstellungen

[!INCLUDE [functions-app-settings-download-cli](../../includes/functions-app-settings-download-local-cli.md)]

## <a name="enable-extension-bundles"></a>Aktivieren von Erweiterungsbundles

[!INCLUDE [functions-extension-bundles](../../includes/functions-extension-bundles.md)]

Jetzt können Sie dem Projekt die Storage-Ausgabebindung hinzufügen.

## <a name="add-an-output-binding"></a>Hinzufügen einer Ausgabebindung

In Functions muss für jeden Typ von Bindung eine `direction`, ein `type` und ein eindeutiger `name` in der Datei „function.json“ definiert werden. Wie Sie diese Attribute definieren, hängt von der Sprache der Funktions-App ab.

[!INCLUDE [functions-add-output-binding-json](../../includes/functions-add-output-binding-json.md)]

## <a name="add-code-that-uses-the-output-binding"></a>Hinzufügen von Code, der die Ausgabebindung verwendet

[!INCLUDE [functions-add-output-binding-python](../../includes/functions-add-output-binding-python.md)]

Bei Verwendung einer Ausgabebindung müssen Sie weder den Azure Storage SDK-Code für die Authentifizierung verwenden noch einen Warteschlangenverweis abrufen oder Daten schreiben. Die Functions-Runtime und die Warteschlangenausgabebindung übernehmen diese Aufgaben für Sie.

## <a name="run-the-function-locally"></a>Lokales Ausführen der Funktion

Wie zuvor verwenden Sie den folgenden Befehl, um die Functions-Runtime lokal zu starten:

```bash
func host start
```

> [!NOTE]  
> Da Sie Erweiterungsbündel in der Datei „host.json“ aktiviert haben, wurde die [Storage-Bindungserweiterung](functions-bindings-storage-blob.md#packages---functions-2x) zusammen mit den übrigen Microsoft-Bindungserweiterungen während des Starts für Sie heruntergeladen und installiert.

Kopieren Sie die URL Ihrer `HttpTrigger`-Funktion aus der Runtimeausgabe, und fügen Sie sie in die Adressleiste Ihres Browsers ein. Hängen Sie anschließend die Abfragezeichenfolge `?name=<yourname>` an diese URL an, und führen Sie die Anforderung aus. Sie sollten dieselbe Antwort im Browser sehen, wie bereits im vorherigen Artikel.

Diesmal erstellt die Ausgabebindung außerdem eine Warteschlange namens `outqueue` in Ihrem Speicherkonto und fügt eine Nachricht mit dieser selben Zeichenfolge hinzu.

Als Nächstes verwenden Sie die Azure CLI, um die neue Warteschlange anzuzeigen und sicherzustellen, dass eine Nachricht hinzugefügt wurde. Sie können Ihre Warteschlange auch mithilfe des [Microsoft Azure Storage-Explorers][Azure Storage Explorer] oder im [Azure-Portal](https://portal.azure.com) anzeigen.

[!INCLUDE [functions-storage-account-set-cli](../../includes/functions-storage-account-set-cli.md)]

[!INCLUDE [functions-query-storage-cli](../../includes/functions-query-storage-cli.md)]

### <a name="redeploy-the-project"></a>Erneutes Bereitstellen des Projekts 

Um Ihre veröffentlichte App zu aktualisieren, verwenden Sie den Core Tools-Befehl [`func azure functionapp publish`](functions-run-local.md#project-file-deployment), um Ihren Projektcode in Azure bereitzustellen. Ersetzen Sie in diesem Beispiel `<APP_NAME>` durch den Namen Ihrer App.

```command
func azure functionapp publish <APP_NAME> --build remote
```

Wieder können Sie cURL oder einen Browser verwenden, um die bereitgestellte Funktion zu testen. Fügen Sie wie zuvor die Abfragezeichenfolge `&name=<yourname>` an die URL an, wie im folgenden Beispiel zu sehen:

```bash
curl https://myfunctionapp.azurewebsites.net/api/httptrigger?code=cCr8sAxfBiow548FBDLS1....&name=<yourname>
```

Sie können [die Storage-Warteschlangennachricht erneut untersuchen](#query-the-storage-queue), um zu überprüfen, ob die Ausgabebindung wie erwartet eine neue Nachricht in der Warteschlange generiert.

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

Sie haben Ihre mittels HTTP ausgelöste Funktion so aktualisiert, dass sie Daten in eine Storage-Warteschlange schreibt. Weitere Informationen zum Entwickeln von Azure Functions mithilfe von Python finden Sie im [Python-Entwicklerhandbuch für Azure Functions](functions-reference-python.md) und unter [Azure Functions-Trigger und -Bindungen](functions-triggers-bindings.md). Beispiele für vollständige Funktionsprojekte in Python finden Sie in den [Functions-Beispielen für Python](/samples/browse/?products=azure-functions&languages=python). 

Als Nächstes sollten Sie die Application Insights-Überwachung für Ihre Funktions-App aktivieren:

> [!div class="nextstepaction"]
> [Aktivieren der Application Insights-Integration](functions-monitoring.md#manually-connect-an-app-insights-resource)

[Azure Storage Explorer]: https://storageexplorer.com/