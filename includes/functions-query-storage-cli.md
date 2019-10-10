---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/26/2019
ms.author: glenga
ms.openlocfilehash: 03ed1b28b4cabc054301e11c2b4d0f9e632abe02
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/03/2019
ms.locfileid: "71839052"
---
Sie können den Befehl [`az storage queue list`](/cli/azure/storage/queue#az-storage-queue-list) verwenden, um die Speicherwarteschlangen in Ihrem Konto anzuzeigen, wie im folgenden Beispiel gezeigt:

```azurecli-interactive
az storage queue list --output tsv
```

Die Ausgabe dieses Befehls umfasst eine Warteschlange namens `outqueue`, wobei es sich um die Warteschlange handelt, die bei Ausführung der Funktion erstellt wurde.

Als Nächstes verwenden Sie den Befehl [`az storage message peek`](/cli/azure/storage/message#az-storage-message-peek), um die Nachrichten in dieser Warteschlange anzuzeigen, wie im folgenden Beispiel zu sehen:

```azurecli-interactive
echo `echo $(az storage message peek --queue-name outqueue -o tsv --query '[].{Message:content}') | base64 --decode`
```

Die zurückgegebene Zeichenfolge sollte mit der Nachricht, die Sie zum Testen der Funktion gesendet haben, identisch sein.

> [!NOTE]  
> Im vorherigen Beispiel wird die zurückgegebene Zeichenfolge aus base64 decodiert. Der Grund hierfür ist, dass die Warteschlangenspeicherbindungen als [base64-Zeichenfolgen](../articles/azure-functions/functions-bindings-storage-queue.md#encoding) in Azure Storage geschrieben und daraus gelesen werden.