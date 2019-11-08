---
title: Erstellen Ihrer ersten PowerShell-Funktion in Azure
description: Hier erfahren Sie, wie Sie in Azure Ihre erste PowerShell-Funktion mit Visual Studio Code erstellen.
author: joeyaiello
manager: gwallace
ms.author: jaiello
ms.reviewer: glenga
ms.date: 04/25/2019
ms.topic: quickstart
ms.service: azure-functions
ms.openlocfilehash: 1d6d641e141862b12fed40b800589aad70af2789
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73469411"
---
# <a name="create-your-first-powershell-function-in-azure"></a>Erstellen Ihrer ersten PowerShell-Funktion in Azure

In diesem Schnellstartartikel erfahren Sie Schritt für Schritt, wie Sie unter Verwendung von Visual Studio Code Ihre erste [serverlose](https://azure.com/serverless) PowerShell-Funktion erstellen.

![Azure Functions-Code in einem Visual Studio Code-Projekt](./media/functions-create-first-function-powershell/powershell-project-first-function.png)

Sie verwenden die [Azure Functions-Erweiterung für Visual Studio Code], um lokal eine PowerShell-Funktion zu erstellen und sie anschließend für eine neue Funktions-App in Azure bereitzustellen. Die Erweiterung befindet sich derzeit in der Vorschauphase. Weitere Informationen finden Sie auf der Seite mit der [Azure Functions-Erweiterung für Visual Studio Code].

> [!NOTE]  
> Die PowerShell-Unterstützung für die [Azure Functions-Erweiterung][azure functions-erweiterung für visual studio code] ist aktuell standardmäßig deaktiviert. Die Aktivierung der PowerShell-Unterstützung ist einer der Schritte in diesem Artikel.

Die folgenden Schritte werden unter macOS, Windows und Linux-basierten Betriebssystemen unterstützt.

## <a name="prerequisites"></a>Voraussetzungen

So führen Sie diesen Schnellstart durch:

* Installieren Sie [PowerShell Core](/powershell/scripting/install/installing-powershell-core-on-windows).

* Installieren Sie [Visual Studio Code](https://code.visualstudio.com/) auf einer der [unterstützten Plattformen](https://code.visualstudio.com/docs/supporting/requirements#_platforms). 

* Installieren Sie die [PowerShell-Erweiterung für Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.PowerShell).

* Installieren Sie [.NET Core SDK 2.2+](https://www.microsoft.com/net/download) (für Azure Functions Core Tools erforderlich und auf allen unterstützten Plattformen verfügbar).

* Installieren Sie die Version 2.x von [Azure Functions Core Tools](functions-run-local.md#v2).

* Außerdem benötigen Sie ein aktives Azure-Abonnement.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [functions-install-vs-code-extension](../../includes/functions-install-vs-code-extension.md)] 

## <a name="create-a-function-app-project"></a>Erstellen eines Funktionen-App-Projekts

Mit der Azure Functions-Projektvorlage in Visual Studio Code wird ein Projekt erstellt, das in einer Funktions-App in Azure veröffentlicht werden kann. Sie können mit einer Funktionen-App Funktionen zu logischen Einheiten gruppieren, um die Verwaltung, Bereitstellung und Freigabe von Ressourcen zu ermöglichen. 

1. Wählen Sie in Visual Studio Code das Azure-Logo aus, um den Bereich **Azure: Funktionen** anzuzeigen, und wählen Sie anschließend das Symbol „Neues Projekt erstellen“ aus.

    ![Erstellen eines Funktionen-App-Projekts](./media/functions-create-first-function-powershell/create-function-app-project.png)

1. Wählen Sie einen Speicherort für Ihren Functions-Projektarbeitsbereich und anschließend **Auswählen** aus.

    > [!NOTE]
    > Die Schritte in diesem Artikel sollten außerhalb eines Arbeitsbereichs ausgeführt werden. Wählen Sie in diesem Fall keinen Projektordner aus, der Teil eines Arbeitsbereichs ist.

1. Wählen Sie als Sprache für Ihr Funktions-App-Projekt **PowerShell** und anschließend **Azure Functions v2** aus.

1. Wählen Sie **HTTP-Trigger** als Vorlage für Ihre erste Funktion aus, verwenden Sie `HTTPTrigger` als Funktionsname, und wählen Sie die Autorisierungsstufe **Funktion** aus.

    > [!NOTE]
    > Die Autorisierungsstufe **Funktion** erfordert einen [Funktionsschlüssel](functions-bindings-http-webhook.md#authorization-keys), wenn der Funktionsendpunkt in Azure aufgerufen wird. Dadurch kann nicht jeder ohne Weiteres Ihre Funktion aufrufen.

1. Wählen Sie bei entsprechender Aufforderung die Option **Zu Arbeitsbereich hinzufügen** aus.

Visual Studio Code erstellt das PowerShell-basierte Funktions-App-Projekt in einem neuen Arbeitsbereich. Dieses Projekt enthält die Konfigurationsdateien [host.json](functions-host-json.md) und [local.settings.json](functions-run-local.md#local-settings-file), die für alle Funktionen im Projekt gelten. Dieses [PowerShell-Projekt](functions-reference-powershell.md#folder-structure) ist mit einer in Azure ausgeführten Funktions-App identisch.

## <a name="run-the-function-locally"></a>Lokales Ausführen der Funktion

Azure Functions Core Tools arbeitet mit Visual Studio Code zusammen, um das lokale Ausführen und Debuggen von Azure Functions-Projekten zu ermöglichen.  

1. Wenn Sie Ihre Funktion debuggen möchten, fügen Sie im Funktionscode einen Aufruf des Cmdlets [`Wait-Debugger`] vor der Stelle ein, an der Sie den Debugger anfügen möchten. Drücken Sie anschließend F5, um das Funktions-App-Projekt zu starten und den Debugger anzufügen. Die Ausgabe der Core Tools wird im Bereich **Terminal** angezeigt.

1. Kopieren Sie im Bereich **Terminal** den URL-Endpunkt Ihrer über HTTP ausgelösten Funktion.

    ![Lokale Azure-Ausgabe](./media/functions-create-first-function-powershell/functions-vscode-f5.png)

1. Fügen Sie anschließend die Abfragezeichenfolge `?name=<yourname>` an diese URL an, und verwenden Sie `Invoke-RestMethod`, um die Anforderung auszuführen:

    ```powershell
    PS > Invoke-RestMethod -Method Get -Uri http://localhost:7071/api/HttpTrigger?name=PowerShell
    Hello PowerShell
    ```

    Die GET-Anforderung kann auch über einen Browser ausgeführt werden.

    Wenn Sie den Endpunkt „HttpTrigger“ aufrufen, ohne einen Parameter vom Typ `name` zu übergeben (entweder als Abfrageparameter oder im Text), gibt die Funktion den Fehler „[HttpStatusCode]::BadRequest“ zurück. Wenn Sie den Code in „run.ps1“ überprüfen, sehen Sie, dass dieser Fehler beabsichtigt ist.

1. Drücken Sie UMSCHALT+F5, um das Debuggen zu beenden.

Nachdem Sie sichergestellt haben, dass die Funktion auf Ihrem lokalen Computer richtig ausgeführt wird, können Sie das Projekt in Azure veröffentlichen.

> [!NOTE]
> Denken Sie daran, alle Aufrufe von `Wait-Debugger` zu entfernen, bevor Sie Ihre Funktionen in Azure veröffentlichen. 
>
> Beim Erstellen einer Funktionen-App in Azure werden Sie lediglich zur Eingabe des Namens Ihrer Funktionen-App aufgefordert. Die anderen Werte werden für Sie definiert.
> Legen Sie `azureFunctions.advancedCreation` auf `true` fest, wenn auch für alle anderen Werte Aufforderungen angezeigt werden sollen.

[!INCLUDE [functions-publish-project-vscode](../../includes/functions-publish-project-vscode.md)]

## <a name="test"></a>Ausführen der Funktion in Azure

Führen Sie den folgenden PowerShell-Befehl aus, und ersetzen Sie dabei den Parameter `Uri` durch die URL der Funktion „HTTPTrigger“ aus dem vorherigen Schritt, um sich zu vergewissern, dass Ihre veröffentlichte Funktion in Azure ausgeführt wird. Fügen Sie wie zuvor die Abfragezeichenfolge `&name=<yourname>` an die URL an, wie im folgenden Beispiel zu sehen:

```powershell
PS > Invoke-WebRequest -Method Get -Uri "https://glengatest-vscode-powershell.azurewebsites.net/api/HttpTrigger?code=nrY05eZutfPqLo0som...&name=PowerShell"

StatusCode        : 200
StatusDescription : OK
Content           : Hello PowerShell
RawContent        : HTTP/1.1 200 OK
                    Content-Length: 16
                    Content-Type: text/plain; charset=utf-8
                    Date: Thu, 25 Apr 2019 16:01:22 GMT

                    Hello PowerShell
Forms             : {}
Headers           : {[Content-Length, 16], [Content-Type, text/plain; charset=utf-8], [Date, Thu, 25 Apr 2019 16:01:22 GMT]}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 16
```

## <a name="next-steps"></a>Nächste Schritte

Sie haben unter Verwendung von Visual Studio Code eine PowerShell-Funktions-App mit einer einfachen Funktion erstellt, die über HTTP ausgelöst wird. Als Nächstes können Sie sich ausführlicher über das [lokale Debuggen einer PowerShell-Funktion](functions-debug-powershell-local.md) mithilfe von Azure Functions Core Tools informieren. Machen Sie sich mit dem [PowerShell-Entwicklerhandbuch für Azure Functions](functions-reference-powershell.md) vertraut.

> [!div class="nextstepaction"]
> [Aktivieren der Application Insights-Integration](functions-monitoring.md#manually-connect-an-app-insights-resource)

[Azure portal]: https://portal.azure.com
[Azure Functions Core Tools]: functions-run-local.md
[Azure Functions-Erweiterung für Visual Studio Code]: https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions
[Wait-Debugger]: /powershell/module/microsoft.powershell.utility/wait-debugger?view=powershell-6
