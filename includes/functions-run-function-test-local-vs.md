---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 07/05/2019
ms.author: glenga
ms.openlocfilehash: 0493de7374cffcc40f0434d84facf50b6a708c7b
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2019
ms.locfileid: "68592857"
---
1. Drücken Sie **F5**, um Ihre Funktion auszuführen. Sie müssen möglicherweise eine Firewallausnahme aktivieren, damit die Tools HTTP-Anforderungen verarbeiten können. Autorisierungsstufen werden bei lokaler Ausführung nie erzwungen.

2. Kopieren Sie die URL Ihrer Funktion aus der Azure Functions-Laufzeitausgabe.

    ![Lokale Azure-Laufzeit](./media/functions-run-function-test-local-vs/functions-debug-local-vs.png)

3. Fügen Sie die URL der HTTP-Anforderung in die Adresszeile des Browsers ein. Hängen Sie anschließend die Abfragezeichenfolge `?name=<YOUR_NAME>` an diese URL an, und führen Sie die Anforderung aus. Hier ist die Antwort des Browsers auf die von der Funktion zurückgegebene lokale GET-Anforderung abgebildet: 

    ![localhost-Antwort der Funktion im Browser](./media/functions-run-function-test-local-vs/functions-run-browser-local-vs.png)

4. Drücken Sie **UMSCHALT+F5**, um das Debuggen zu beenden.