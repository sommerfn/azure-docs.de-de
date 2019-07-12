---
title: Erstellen Ihrer ersten Funktion in Azure mit Visual Studio Code
description: Hier wird beschrieben, wie Sie mit der Azure Functions-Erweiterung in Visual Studio Code eine einfache per HTTP ausgelöste Funktion in Azure erstellen und veröffentlichen.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: Azure Functions, Funktionen, Ereignisverarbeitung, Compute, serverlose Architektur
ms.service: azure-functions
ms.devlang: multiple
ms.topic: quickstart
ms.date: 06/25/2019
ms.author: glenga
ms.custom: mvc, devcenter
ms.openlocfilehash: fcf9f1d6420dbbde359d386bc3b67a0866aca30d
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67444627"
---
# <a name="create-your-first-function-using-visual-studio-code"></a>Erstellen Ihrer ersten Funktion mit Visual Studio Code

Mit Azure Functions können Sie Code in einer [serverlosen](https://azure.microsoft.com/solutions/serverless/) Umgebung ausführen, ohne vorher eine VM erstellen oder eine Webanwendung veröffentlichen zu müssen.

In diesem Artikel erfahren Sie, wie Sie die [Azure Functions-Erweiterung für Visual Studio Code] verwenden, um auf Ihrem lokalen Computer mithilfe von Visual Studio Code die Funktion „Hello World“ zu erstellen und testen. Anschließend veröffentlichen Sie den Funktionscode über Visual Studio Code in Azure.

![Azure Functions-Code in einem Visual Studio-Projekt](./media/functions-create-first-function-vs-code/functions-vscode-intro.png)

Die Erweiterung bietet derzeit Unterstützung für C#-, JavaScript- und Java-Funktionen. Die Unterstützung für Python befindet sich derzeit in der Vorschauphase. Bei den Schritten in diesem und dem folgenden Artikel werden nur JavaScript- und C#-Funktionen unterstützt. Weitere Informationen zur Verwendung von Visual Studio Code zum Erstellen und Veröffentlichen von Python-Funktionen finden Sie unter [Bereitstellen von Python für Azure Functions](https://code.visualstudio.com/docs/python/tutorial-azure-functions). Weitere Informationen zur Verwendung von Visual Studio Code zum Erstellen und Veröffentlichen von PowerShell-Funktionen finden Sie unter [Erstellen Ihrer ersten PowerShell-Funktion in Azure](functions-create-first-function-powershell.md). 

Die Erweiterung befindet sich derzeit in der Vorschauphase. Weitere Informationen finden Sie auf der Seite mit der [Azure Functions-Erweiterung für Visual Studio Code].

## <a name="prerequisites"></a>Voraussetzungen

So führen Sie diesen Schnellstart durch:

* Installieren Sie [Visual Studio Code](https://code.visualstudio.com/) auf einer der [unterstützten Plattformen](https://code.visualstudio.com/docs/supporting/requirements#_platforms).

* Installieren Sie die Version 2.x von [Azure Functions Core Tools](functions-run-local.md#v2).

* Installieren Sie die erforderlichen Komponenten für die ausgewählte Sprache:

    | Sprache | Anforderung |
    | -------- | --------- |
    | **C#** | [C#-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)  |
    | **JavaScript** | [Node.js](https://nodejs.org/)<sup>*</sup> | 
 
    <sup>*</sup>Active LTS- und Maintenance LTS-Versionen (8.11.1 und 10.14.1 empfohlen).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [functions-install-vs-code-extension](../../includes/functions-install-vs-code-extension.md)]

[!INCLUDE [functions-create-function-app-vs-code](../../includes/functions-create-function-app-vs-code.md)]

[!INCLUDE [functions-run-function-test-local-vs-code](../../includes/functions-run-function-test-local-vs-code.md)]

Nachdem Sie sichergestellt haben, dass die Funktion auf Ihrem lokalen Computer richtig ausgeführt wird, können Sie das Projekt in Azure veröffentlichen.

[!INCLUDE [functions-sign-in-vs-code](../../includes/functions-sign-in-vs-code.md)]

[!INCLUDE [functions-publish-project-vscode](../../includes/functions-publish-project-vscode.md)]

## <a name="run-the-function-in-azure"></a>Ausführen der Funktion in Azure

1. Kopieren Sie die URL des HTTP-Triggers im Bereich **Ausgabe**. Stellen Sie wie zuvor sicher, dass Sie die Abfragezeichenfolge `?name=<yourname>` am Ende dieser URL anfügen und die Anforderung ausführen.

    Die URL, über die Ihre per HTTP ausgelöste Funktion aufgerufen wird, sollte das folgende Format haben:

        http://<functionappname>.azurewebsites.net/api/<functionname>?name=<yourname> 

1. Fügen Sie diese neue URL für die HTTP-Anforderung in die Adresszeile des Browsers ein. Hier ist die Antwort des Browsers auf die von der Funktion zurückgegebene GET-Remoteanforderung abgebildet: 

    ![Funktionsantwort im Browser](./media/functions-create-first-function-vs-code/functions-test-remote-browser.png)

## <a name="next-steps"></a>Nächste Schritte

Sie haben Visual Studio Code genutzt, um eine Funktions-App mit einer einfachen Funktion zu erstellen, die über HTTP ausgelöst wird. Im nächsten Artikel erweitern Sie diese Funktion durch Hinzufügen einer Ausgabebindung. Diese Bindung schreibt die Zeichenfolge aus der HTTP-Anforderung in eine Nachricht in einer Azure Queue Storage-Warteschlange. Der nächste Artikel zeigt Ihnen auch, wie Sie diese neuen Azure-Ressourcen bereinigen können, indem Sie die von Ihnen erstellte Ressourcengruppe entfernen.

> [!div class="nextstepaction"]
> [Hinzufügen einer Azure Storage-Warteschlangenbindung zu Ihrer Funktion](functions-add-output-binding-storage-queue-vs-code.md)

[Azure Functions Core Tools]: functions-run-local.md
[Azure Functions-Erweiterung für Visual Studio Code]: https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions
