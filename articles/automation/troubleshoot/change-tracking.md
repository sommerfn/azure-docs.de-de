---
title: Behandeln von Problemen mit der Azure-Änderungsnachverfolgung
description: Dieser Artikel enthält Informationen zur Behandlung von Problemen mit der Änderungsnachverfolgung.
services: automation
ms.service: automation
ms.subservice: change-inventory-management
author: bobbytreed
ms.author: robreed
ms.date: 01/31/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: beb0b89bdbf143c89a83c0813313a8bbda7235d4
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68564859"
---
# <a name="troubleshoot-change-tracking-and-inventory"></a>Problembehandlung bei Änderungsnachverfolgung und Inventur

## <a name="windows"></a>Windows

### <a name="records-not-showing-windows"></a>Szenario: Datensätze der Änderungsnachverfolgung werden für Windows-Computer nicht angezeigt

#### <a name="issue"></a>Problem

Es werden keine Ergebnisse für die Inventur oder Änderungsnachverfolgung für Windows-Computer angezeigt, die in die Änderungsnachverfolgung integriert sind.

#### <a name="cause"></a>Ursache

Dieser Fehler kann die folgenden Gründe haben:

1. **Microsoft Monitoring Agent** wird nicht ausgeführt.
2. Die Kommunikation an das Automation-Konto wird blockiert.
3. Die Management Packs für die Änderungsnachverfolgung wurden nicht heruntergeladen.
4. Die VM, die integriert wird, stammt möglicherweise von einem geklonten Cloudcomputer, dessen System nicht mit dem installierten Microsoft Monitoring Agent vorbereitet wurde.

#### <a name="resolution"></a>Lösung

1. Überprüfen Sie, ob **Microsoft Monitoring Agent** (HealthService.exe) auf dem Computer ausgeführt wird.
1. Überprüfen Sie die **Ereignisanzeige** auf dem Computer, und suchen Sie nach allen Ereignisse, die das Wort `changetracking` enthalten.
1. Sehen Sie sich den Abschnitt [Konfigurieren des Netzwerks](../automation-hybrid-runbook-worker.md#network-planning) an, um zu erfahren, welche Adressen und Ports zugelassen werden müssen, damit die Änderungsnachverfolgung funktioniert.
1. Überprüfen Sie, ob die Management Packs für die Änderungsnachverfolgung und Inventur lokal vorhanden sind:
    * Microsoft.IntelligencePacks.ChangeTrackingDirectAgent.*
    * Microsoft.IntelligencePacks.InventoryChangeTracking.*
    * Microsoft.IntelligencePacks.SingletonInventoryCollection.*
1. Wenn Sie ein geklontes Image verwenden, bereiten Sie mit Sysprep das Image zunächst vor, und installieren Sie Microsoft Monitoring Agent anschließend.

Wenn diese Lösungen das Problem nicht beheben und Sie sich an den Support wenden, können Sie die folgenden Befehle ausführen, um die Diagnoseinformationen im Agent zu erfassen.

Navigieren Sie auf dem Agent-Computer zu `C:\Program Files\Microsoft Monitoring Agent\Agent\Tools`, und führen Sie die folgenden Befehle aus:

```cmd
net stop healthservice
StopTracing.cmd
StartTracing.cmd VER
net start healthservice
```

> [!NOTE]
> Die Ablaufverfolgung für Fehler ist standardmäßig aktiviert. Wenn Sie ausführliche Fehlermeldungen wie im oben stehenden Beispiel aktivieren möchten, verwenden Sie den Parameter `VER`. Für Ablaufverfolgungen zur Information verwenden Sie `INF` beim Aufrufen von `StartTracing.cmd`.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Erhalten Sie Antworten von Azure-Experten über [Azure-Foren](https://azure.microsoft.com/support/forums/).
* Mit [@AzureSupport](https://twitter.com/azuresupport) verbinden – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit durch Verbinden der Azure-Community mit den richtigen Ressourcen: Antworten, Support und Experten.
* Wenn Sie weitere Hilfe benötigen, können Sie einen Azure-Supportvorgang anlegen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie **Support erhalten**aus.
