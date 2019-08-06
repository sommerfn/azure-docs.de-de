---
title: Verbinden von Funktionen mit Azure Storage mithilfe von Visual Studio Code
description: Erfahren Sie, wie Sie eine Ausgabebindung hinzufügen, um Funktionen mithilfe von Visual Studio Code mit einer Azure Storage-Warteschlange zu verbinden.
author: ggailey777
ms.author: glenga
ms.date: 06/25/2019
ms.topic: quickstart
ms.service: azure-functions
ms.custom: mvc
manager: jeconnoc
ms.openlocfilehash: 40a912a94dc61342c04528e902bb0e084546904d
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2019
ms.locfileid: "68592818"
---
# <a name="connect-functions-to-azure-storage-using-visual-studio-code"></a>Verbinden von Funktionen mit Azure Storage mithilfe von Visual Studio Code

Mit Azure Functions können Sie Funktionen mit Azure-Diensten und anderen Ressourcen verbinden, ohne dass Sie Ihren eigenen Integrationscode schreiben müssen. Diese *Bindungen*, die sowohl Eingabe als auch Ausgabe darstellen, werden innerhalb der Funktionsdefinition deklariert. Daten von Bindungen werden der Funktion als Parameter bereitgestellt. Ein Trigger ist ein spezieller Typ Eingabebindung. Während eine Funktion nur einen Trigger hat, kann sie mehrere Ein- und Ausgabebindungen haben. Weitere Informationen finden Sie unter [Konzepte der Trigger und Bindungen in Azure Functions](functions-triggers-bindings.md).

In diesem Artikel wird beschrieben, wie Sie die Funktion, die Sie im [vorherigen Schnellstartartikel](functions-create-first-function-vs-code.md) erstellt haben, mithilfe von Visual Studio Code mit Azure Storage verbinden. Die Ausgabebindung, die Sie dieser Funktion hinzufügen, schreibt Daten aus der HTTP-Anforderung in eine Nachricht in einer Azure Queue Storage-Warteschlange. 

Die meisten Bindungen erfordern eine gespeicherte Verbindungszeichenfolge, die Functions verwendet, um auf den gebundenen Dienst zuzugreifen. Um dies zu vereinfachen, verwenden Sie das Speicherkonto, das Sie mit Ihrer Funktions-App erstellt haben. Die Verbindung mit diesem Konto ist bereits in einer App-Einstellung namens `AzureWebJobsStorage` gespeichert.  

## <a name="prerequisites"></a>Voraussetzungen

Zunächst müssen Sie die folgenden Schritte ausführen:

* Installieren Sie die [Azure Storage-Erweiterung für Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurestorage).
* Installieren Sie [Azure Storage-Explorer](https://storageexplorer.com/). Storage-Explorer ist ein Tool, mit dem Sie Warteschlangennachrichten überprüfen können, die von der Ausgabebindung generiert werden. Storage-Explorer wird unter macOS-, Windows- und Linux-basierten Betriebssystemen unterstützt.
* Installieren Sie [.NET Core-CLI-Tools](https://docs.microsoft.com/dotnet/core/tools/?tabs=netcore2x) (nur C#-Projekte).
* Führen Sie die Schritte in [Teil 1 des Schnellstarts für Visual Studio Code](functions-create-first-function-vs-code.md) aus. 

In diesem Artikel wird davon ausgegangen, dass Sie bereits über Visual Studio Code bei Ihrem Azure-Abonnement angemeldet sind. Sie können sich anmelden, indem Sie `Azure: Sign In` in der Befehlspalette ausführen. 

## <a name="download-the-function-app-settings"></a>Herunterladen der Funktions-App-Einstellungen

Im [vorherigen Schnellstartartikel](functions-create-first-function-vs-code.md) haben Sie zusammen mit dem erforderlichen Storage-Konto eine Funktions-App in Azure erstellt. Die Verbindungszeichenfolge für dieses Konto wird sicher in App-Einstellungen in Azure gespeichert. In diesem Artikel schreiben Sie Nachrichten in eine Speicherwarteschlange in demselben Konto. Um eine Verbindung mit Ihrem Speicherkonto herzustellen, wenn die Funktion lokal ausgeführt wird, müssen Sie App-Einstellungen in die Datei „local.settings.json“ herunterladen. 

1. Drücken Sie die F1-Taste, um die Befehlspalette zu öffnen. Suchen Sie dann den Befehl `Azure Functions: Download Remote Settings....`, und führen Sie ihn aus. 

1. Wählen Sie die im vorherigen Artikel erstellte Funktions-App aus. Wählen Sie **Ja, alle** aus, um die vorhandenen lokalen Einstellungen zu überschreiben. 

    > [!IMPORTANT]  
    > Da sie Geheimnisse enthält, wird die Datei „local.settings.json“ nie veröffentlicht und von der Quellcodeverwaltung ausgeschlossen.

1. Kopieren Sie den Wert `AzureWebJobsStorage`, bei dem es sich um den Schlüssel für den Verbindungszeichenfolgenwert des Storage-Kontos handelt. Sie verwenden diese Verbindung, um sicherzustellen, dass die Ausgabebindung wie erwartet funktioniert.

## <a name="register-binding-extensions"></a>Registrieren von Bindungserweiterungen

Da Sie eine Queue Storage-Ausgabebindung verwenden, müssen Sie vor dem Ausführen des Projekts die Storage-Bindungserweiterung installieren. 

### <a name="javascript"></a>JavaScript

[!INCLUDE [functions-extension-bundles](../../includes/functions-extension-bundles.md)]

### <a name="c-class-library"></a>C\#-Klassenbibliothek

Mit Ausnahme von HTTP- und Timertriggern werden Bindungen als Erweiterungspakete implementiert. Führen Sie den folgenden [dotnet add package](/dotnet/core/tools/dotnet-add-package)-Befehl im Terminalfenster aus, um Ihrem Projekt das Storage-Erweiterungspaket hinzuzufügen.

```bash
dotnet add package Microsoft.Azure.WebJobs.Extensions.Storage --version 3.0.4
```

Dann können Sie dem Projekt die Storage-Ausgabebindung hinzufügen.

## <a name="add-an-output-binding"></a>Hinzufügen einer Ausgabebindung

In Functions muss für jeden Typ von Bindung eine `direction`, ein `type` und ein eindeutiger `name` in der Datei „function.json“ definiert werden. Wie Sie diese Attribute definieren, hängt von der Sprache der Funktions-App ab.

### <a name="javascript"></a>JavaScript

Bindungsattribute werden direkt in der Datei „function.json“ definiert. Abhängig vom Bindungstyp sind möglicherweise zusätzliche Eigenschaften erforderlich. Die [Warteschlangenausgabekonfiguration](functions-bindings-storage-queue.md#output---configuration) beschreibt die Felder, die für eine Azure Storage-Warteschlangenbindung erforderlich sind. Mit der Erweiterung können Bindungen einfach in der Datei „function.json“ hinzugefügt werden. 

Klicken Sie zum Erstellen einer Bindung mit der rechten Maustaste (CTRL+Klicken unter macOS) auf die Datei `function.json` im Ordner „HttpTrigger“, und wählen Sie **Bindung hinzufügen** aus. Befolgen Sie die Anweisungen, um die folgenden Bindungseigenschaften für die neue Bindung zu definieren:

| Prompt | Wert | BESCHREIBUNG |
| -------- | ----- | ----------- |
| **Select binding direction** (Wählen Sie die Bindungsrichtung aus) | `out` | Die Bindung ist eine Ausgabebindung. |
| **Select binding with direction** (Wählen Sie die Bindung mit Richtung aus) | `Azure Queue Storage` | Die Bindung ist eine Azure Storage-Warteschlangenbindung. |
| **Der Name, der zum Identifizieren dieser Bindung in Ihrem Code verwendet wird** | `msg` | Name, der den Bindungsparameter identifiziert, auf den in Ihrem Code verwiesen wird. |
| **Die Warteschlange, an die die Nachricht gesendet wird** | `outqueue` | Der Name der Warteschlange, in den die Bindung schreibt. Wenn der *queueName* nicht vorhanden ist, erstellt die Bindung ihn bei der ersten Verwendung. |
| **Select setting from "local.setting.json"** (Wählen Sie eine Einstellung aus „local.setting.json“ aus) | `AzureWebJobsStorage` | Der Name einer Anwendungseinstellung, die die Verbindungszeichenfolge für das Storage-Konto enthält. Die Einstellung `AzureWebJobsStorage` enthält die Verbindungszeichenfolge für das Speicherkonto, das Sie mit der Funktions-App erstellt haben. |

Eine Bindung wird dem Array `bindings` in der Datei „function.json“ hinzugefügt, die nun wie das folgende Beispiel aussehen sollte:

```json
{
   ...

  "bindings": [
    {
      "authLevel": "function",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    },
    {
      "type": "queue",
      "direction": "out",
      "name": "msg",
      "queueName": "outqueue",
      "connection": "AzureWebJobsStorage"
    }
  ]
}
```

### <a name="c-class-library"></a>C\#-Klassenbibliothek

[!INCLUDE [functions-add-storage-binding-csharp-library](../../includes/functions-add-storage-binding-csharp-library.md)]

## <a name="add-code-that-uses-the-output-binding"></a>Hinzufügen von Code, der die Ausgabebindung verwendet

Nachdem die Bindung definiert wurde, können Sie den `name` der Bindung verwenden, um auf sie als Attribut in der Funktionssignatur zuzugreifen. Durch die Verwendung einer Ausgabebindung müssen Sie weder den Azure Storage-SDK-Code für die Authentifizierung verwenden, noch einen Warteschlangenverweis abrufen oder Daten schreiben. Die Functions-Runtime und die Warteschlangenausgabebindung übernehmen diese Aufgaben für Sie.

### <a name="javascript"></a>JavaScript

Fügen Sie Code hinzu, der das Ausgabebindungsobjekt `msg` für `context.bindings` verwendet, um eine Warteschlangennachricht zu erstellen. Fügen Sie diesen Code vor der `context.res`-Anweisung hinzu.

```javascript
// Add a message to the Storage queue.
context.bindings.msg = "Name passed to the function: " + 
(req.query.name || req.body.name);
```

Die Funktion sollte nun wie folgt aussehen:

```javascript
module.exports = async function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');

    if (req.query.name || (req.body && req.body.name)) {
        // Add a message to the Storage queue.
        context.bindings.msg = "Name passed to the function: " + 
        (req.query.name || req.body.name);
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
};
```

### <a name="c"></a>C\#

[!INCLUDE [functions-add-storage-binding-csharp-library-code](../../includes/functions-add-storage-binding-csharp-library-code.md)]

[!INCLUDE [functions-run-function-test-local-vs-code](../../includes/functions-run-function-test-local-vs-code.md)]

Eine neue Warteschlange mit dem Namen **outqueue** wird in Ihrem Speicherkonto von der Functions-Runtime erstellt, wenn die Ausgabebindung zum ersten Mal verwendet wird. Mit Storage-Explorer überprüfen Sie, ob die Warteschlange zusammen mit der neuen Nachricht erstellt wurde.

### <a name="connect-storage-explorer-to-your-account"></a>Herstellen einer Storage-Explorer-Verbindung mit Ihrem Konto

Diesen Abschnitt können Sie überspringen, wenn Sie Azure Storage-Explorer bereits installiert und mit Ihrem Azure-Konto verbunden haben.

1. Führen Sie das Tool [Azure Storage-Explorer] aus, und wählen Sie das Verbindungssymbol auf der linken Seite und dann **Konto hinzufügen** aus.

    ![Hinzufügen eines Azure-Kontos zu Microsoft Azure Storage-Explorer](./media/functions-add-output-binding-storage-queue-vs-code/storage-explorer-add-account.png)

1. Wählen Sie im Dialogfeld **Verbinden** die Option **Azure-Konto hinzufügen** aus. Wählen Sie Ihre **Azure-Umgebung** und dann **Anmelden** aus. 

    ![Anmelden bei Ihrem Azure-Konto](./media/functions-add-output-binding-storage-queue-vs-code/storage-explorer-connect-azure-account.png)

Nach der erfolgreichen Anmeldung bei Ihrem Konto werden alle mit dem Konto verbundenen Azure-Abonnements angezeigt.

### <a name="examine-the-output-queue"></a>Überprüfen der Ausgabewarteschlange

1. Drücken Sie in Visual Studio Code die F1-Taste, um die Befehlspalette zu öffnen. Suchen Sie dann den Befehl `Azure Storage: Open in Storage Explorer`, führen Sie ihn aus, und wählen Sie den Namen Ihres Storage-Kontos aus. Das Storage-Konto wird in Azure Storage-Explorer geöffnet.  

1. Erweitern Sie den Knoten **Warteschlangen**, und wählen Sie die Warteschlange mit dem Namen **outqueue** aus. 

   Die Warteschlange enthält die Meldung, die die Warteschlangen-Ausgabebindung erstellt hat, als Sie die per HTTP ausgelöste Funktion ausgeführt haben. Wenn Sie die Funktion mit dem `name`-Standardwert *Azure* aufgerufen haben, lautet die Warteschlangenmeldung *Name passed to the function: Azure* (An die Funktion übergebener Name: Azure).

    ![In Azure Storage-Explorer angezeigte Warteschlangennachricht](./media/functions-add-output-binding-storage-queue-vs-code/function-queue-storage-output-view-queue.png)

1. Führen Sie die Funktion erneut aus, und senden Sie eine andere Anforderung. Daraufhin wird eine neue Nachricht in der Warteschlange angezeigt.  

Nun ist es an der Zeit, die aktualisierte Funktions-App erneut in Azure zu veröffentlichen.

## <a name="redeploy-and-verify-the-updated-app"></a>Erneutes Bereitstellen und Überprüfen der aktualisierten App

1. Drücken Sie in Visual Studio Code die F1-Taste, um die Befehlspalette zu öffnen. Suchen Sie in der Befehlspalette den Befehl `Azure Functions: Deploy to function app...`, und wählen Sie ihn aus.

1. Wählen Sie die im ersten Artikel erstellte Funktions-App aus. Da Sie das Projekt für die gleiche App erneut bereitstellen, wählen Sie **Bereitstellen** aus, um die Warnung zum Überschreiben von Dateien zu schließen.

1. Nach Abschluss der Bereitstellung können Sie die erneut bereitgestellte Funktion wieder mithilfe von cURL oder in einem Browser testen. Fügen Sie wie zuvor die Abfragezeichenfolge `&name=<yourname>` an die URL an, wie im folgenden Beispiel zu sehen:

    ```bash
    curl https://myfunctionapp.azurewebsites.net/api/httptrigger?code=cCr8sAxfBiow548FBDLS1....&name=<yourname>
    ```

1. [Zeigen Sie wieder die Nachricht in der Speicherwarteschlange an](#examine-the-output-queue), um zu überprüfen, ob die Ausgabebindung erneut eine neue Nachricht in der Warteschlange generiert.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

*Ressourcen* bezieht sich im Zusammenhang mit Azure auf Funktions-Apps, Funktionen, Speicherkonten und Ähnliches. Sie werden in *Ressourcengruppen* zusammengefasst, und sämtliche Inhalte einer Gruppe können durch Löschen der Gruppe gelöscht werden.

Im Rahmen dieser Schnellstartanleitungen haben Sie Ressourcen erstellt. Für diese Ressourcen fallen je nach [Kontostatus](https://azure.microsoft.com/account/) und [Dienstpreisen](https://azure.microsoft.com/pricing/) unter Umständen Kosten an. Nicht mehr benötigte Ressourcen können wie folgt gelöscht werden:

1. Drücken Sie in Visual Studio Code die F1-Taste, um die Befehlspalette zu öffnen. Suchen Sie in der Befehlspalette den Befehl `Azure Functions: Open in portal`, und wählen Sie ihn aus.

1. Wählen Sie Ihre Funktions-App aus, und drücken Sie die EINGABETASTE. Die Seite der Funktions-App wird im [Azure-Portal](https://portal.azure.com) geöffnet.

1. Wählen Sie auf der Registerkarte **Übersicht** den benannten Link unter **Ressourcengruppe** aus.

    ![Wählen Sie auf der Seite „Funktions-App“ die zu löschende Ressourcengruppe aus.](./media/functions-add-output-binding-storage-queue-vs-code/functions-app-delete-resource-group.png)

1. Prüfen Sie auf der Seite **Ressourcengruppe** die Liste mit den enthaltenen Ressourcen, und vergewissern Sie sich, dass es sich dabei um die Ressourcen handelt, die Sie löschen möchten.
 
1. Klicken Sie auf **Ressourcengruppe löschen**, und folgen Sie den Anweisungen.

   Der Löschvorgang kann einige Minuten dauern. Nach Abschluss des Vorgangs wird kurz eine Benachrichtigung angezeigt. Sie können auch am oberen Seitenrand auf das Glockensymbol klicken, um die Benachrichtigung anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte

Sie haben Ihre mittels HTTP ausgelöste Funktion so aktualisiert, dass sie Daten in eine Speicherwarteschlange schreibt. Weitere Informationen zur Entwicklung von Functions finden Sie unter [Entwickeln von Azure Functions mithilfe von Visual Studio Code](functions-develop-vs-code.md).

Als Nächstes sollten Sie die Application Insights-Überwachung für Ihre Funktions-App aktivieren:

> [!div class="nextstepaction"]
> [Aktivieren der Application Insights-Integration](functions-monitoring.md#manually-connect-an-app-insights-resource)

[Azure Storage-Explorer]: https://storageexplorer.com/
