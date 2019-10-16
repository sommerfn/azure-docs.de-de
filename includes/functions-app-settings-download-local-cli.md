---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/26/2019
ms.author: glenga
ms.openlocfilehash: 801613f4e3fb1e754856d716c6815895ea5da3aa
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/03/2019
ms.locfileid: "71839158"
---
Sie haben bereits eine Funktions-App in Azure zusammen mit dem erforderlichen Storage-Konto erstellt. Die Verbindungszeichenfolge für dieses Konto wird sicher in App-Einstellungen in Azure gespeichert. In diesem Artikel schreiben Sie Nachrichten in eine Speicherwarteschlange in demselben Konto. Um eine Verbindung mit Ihrem Speicherkonto herzustellen, wenn die Funktion lokal ausgeführt wird, müssen Sie App-Einstellungen in die Datei „local.settings.json“ herunterladen. Führen Sie den folgenden Azure Functions Core Tools-Befehl aus, um Einstellungen in „local.settings.json“ herunterzuladen, und ersetzen Sie dabei `<APP_NAME>` durch den Namen Ihrer Funktions-App aus dem vorherigen Artikel:

```bash
func azure functionapp fetch-app-settings <APP_NAME>
```

Möglicherweise müssen Sie sich bei Ihrem Azure-Konto anmelden.

> [!IMPORTANT]  
> Da sie Geheimnisse enthält, wird die lokale Datei „local.settings.json“ nie veröffentlicht, und sie sollte auch aus der Quellcodeverwaltung ausgeschlossen werden.

Sie benötigen den Wert `AzureWebJobsStorage`, bei dem es sich um die Verbindungszeichenfolge des Speicherkontos handelt. Sie verwenden diese Verbindung, um sicherzustellen, dass die Ausgabebindung wie erwartet funktioniert.
