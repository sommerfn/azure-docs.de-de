---
title: include file
description: include file
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: include
ms.date: 05/31/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 88a4fe8b9f0b477ed851a03742a9957c08b7cbf0
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67455193"
---
## <a name="run-the-function-locally"></a>Lokales Ausführen der Funktion

Mit Azure Functions Core-Tools können Sie ein Azure Functions-Projekt auf dem lokalen Entwicklungscomputer ausführen.

1. Legen Sie zum Testen der Funktion einen Breakpoint im Funktionscode fest, und drücken Sie F5, um das Funktions-App-Projekt zu starten. Die Ausgabe der Core Tools wird im Bereich **Terminal** angezeigt.

1. Kopieren Sie im Bereich **Terminal** den URL-Endpunkt Ihrer über HTTP ausgelösten Funktion. Diese URL enthält den Funktionsschlüssel, der an den `code`-Abfrageparameter übergeben wird.

    ![Lokale Azure-Ausgabe](./media/functions-run-function-test-local-vs-code/functions-vscode-f5.png)

1. Fügen Sie die URL der HTTP-Anforderung in die Adresszeile des Browsers ein. Hängen Sie anschließend die Abfragezeichenfolge `?name=<yourname>` an diese URL an, und führen Sie die Anforderung aus. Die Ausführung wird angehalten, wenn der Breakpoint erreicht wird.

1. Wenn Sie die Ausführung fortsetzen, sehen Sie nachfolgend die Antwort des Browsers auf die GET-Anforderung:

    ![localhost-Antwort der Funktion im Browser](./media/functions-run-function-test-local-vs-code/functions-test-local-browser.png)

1. Drücken Sie UMSCHALT+F5, um das Debuggen zu beenden.