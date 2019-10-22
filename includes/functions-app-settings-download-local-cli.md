---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/26/2019
ms.author: glenga
ms.openlocfilehash: e10de8093bf152b75cc6f262a142ff07c3d5b0d3
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72329593"
---
Sie haben bereits eine Funktions-App in Azure zusammen mit dem erforderlichen Storage-Konto erstellt. Die Verbindungszeichenfolge für dieses Konto wird sicher in App-Einstellungen in Azure gespeichert. In diesem Artikel schreiben Sie Nachrichten in eine Speicherwarteschlange in demselben Konto. Um eine Verbindung mit Ihrem Speicherkonto herzustellen, wenn die Funktion lokal ausgeführt wird, müssen Sie App-Einstellungen in die Datei „local.settings.json“ herunterladen. 

Führen Sie im Stammverzeichnis des Projekts den folgenden Azure Functions Core Tools-Befehl aus, um Einstellungen in „local.settings.json“ herunterzuladen, und ersetzen Sie dabei `<APP_NAME>` durch den Namen Ihrer Funktions-App aus dem vorherigen Artikel:

```bash
func azure functionapp fetch-app-settings <APP_NAME>
```

Möglicherweise müssen Sie sich bei Ihrem Azure-Konto anmelden.

> [!IMPORTANT]  
> Dieser Befehl überschreibt alle vorhandenen Einstellungen mit Werten aus ihrer Funktions-App in Azure.  
>
> Da sie Geheimnisse enthält, wird die lokale Datei „local.settings.json“ nie veröffentlicht, und sie sollte auch aus der Quellcodeverwaltung ausgeschlossen werden.  
> 

Sie benötigen den Wert `AzureWebJobsStorage`, bei dem es sich um die Verbindungszeichenfolge des Speicherkontos handelt. Sie verwenden diese Verbindung, um sicherzustellen, dass die Ausgabebindung wie erwartet funktioniert.
