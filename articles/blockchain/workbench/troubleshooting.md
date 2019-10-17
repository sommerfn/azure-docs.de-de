---
title: Problembehandlung bei Azure Blockchain Workbench
description: So behandeln Sie Probleme mit einer Azure Blockchain Workbench-Anwendung (Vorschauversion).
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 10/14/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: 3032a81a4411cb162a335189ca65b845f8d6305f
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72329084"
---
# <a name="azure-blockchain-workbench-preview-troubleshooting"></a>Problembehandlung bei Azure Blockchain Workbench (Vorschauversion)

Für das Debuggen durch Entwickler und zur Unterstützung ist ein PowerShell-Skript verfügbar. Mithilfe des Skripts wird eine Zusammenfassung generiert und Details in Protokollen für die Problembehandlung gesammelt. Die gesammelten Protokolle umfassen Folgendes:

* Blockchain-Netzwerk wie Ethereum
* Blockchain Workbench-Microservices
* Application Insights
* Azure-Überwachung (Azure Monitor-Protokolle)

Anhand dieser Informationen können Sie die nächsten Schritte bestimmen und die Grundursache von Problemen ermitteln.

[!INCLUDE [Preview note](./includes/preview.md)]

## <a name="troubleshooting-script"></a>Skript für die Problembehandlung

Das PowerShell-Skript für die Problembehandlung ist auf GitHub verfügbar. [Laden Sie eine ZIP-Datei herunter](https://github.com/Azure-Samples/blockchain/archive/master.zip), oder klonen Sie das Beispiel aus GitHub.

```
git clone https://github.com/Azure-Samples/blockchain.git
```

## <a name="run-the-script"></a>Ausführen des Skripts
[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

Führen Sie das `collectBlockchainWorkbenchTroubleshooting.ps1`-Skript aus, um Protokolle zu sammeln und eine ZIP-Datei zu erstellen, die einen Ordner mit Informationen zur Problembehebung enthält. Beispiel:

``` powershell
collectBlockchainWorkbenchTroubleshooting.ps1 -SubscriptionID "<subscription_id>" -ResourceGroupName "workbench-resource-group-name"
```
Das Skript akzeptiert die folgenden Parameter:

| Parameter  | BESCHREIBUNG | Erforderlich |
|---------|---------|----|
| SubscriptionID | SubscriptionID zum Erstellen oder Suchen aller Ressourcen. | Ja |
| ResourceGroupName | Name der Azure-Ressourcengruppe, in der Blockchain Workbench bereitgestellt wurde. | Ja |
| OutputDirectory | Pfad zum Erstellen der ZIP-Ausgabedatei. Wenn nicht angegeben, wird standardmäßig das aktuelle Verzeichnis verwendet. | Nein |
| LookbackHours | Die Anzahl der beim Abrufen von Telemetriedaten zu verwendenden Stunden. Der Standardwert ist 24 Stunden. Der Höchstwert ist 90 Stunden. | Nein |
| OmsSubscriptionId | Die Abonnement-ID, für die Azure Monitor-Protokolle bereitgestellt wird. Übergeben Sie diesen Parameter nur, wenn Azure Monitor-Protokolle für das Blockchain-Netzwerk außerhalb der Blockchain Workbench-Ressourcengruppe bereitgestellt wird.| Nein |
| OmsResourceGroup |Die Ressourcengruppe, in der Azure Monitor-Protokolle bereitgestellt wird. Übergeben Sie diesen Parameter nur, wenn Azure Monitor-Protokolle für das Blockchain-Netzwerk außerhalb der Blockchain Workbench-Ressourcengruppe bereitgestellt wird.| Nein |
| OmsWorkspaceName | Name des Log Analytics-Arbeitsbereichs. Übergeben Sie diesen Parameter nur, wenn Azure Monitor-Protokolle für das Blockchain-Netzwerk außerhalb der Blockchain Workbench-Ressourcengruppe bereitgestellt wird. | Nein |

## <a name="what-is-collected"></a>Was wird gesammelt?

Die ZIP-Ausgabedatei enthält die folgende Ordnerstruktur:

| Ordner oder Datei | BESCHREIBUNG  |
|---------|---------|
| \Summary.txt | Zusammenfassung des Systems |
| \Metrics\blockchain | Metriken zu der Blockchain |
| \Metrics\Workbench | Metriken zu der Workbench |
| \Details\Blockchain | Detailprotokolle zu der Blockchain |
| \Details\Workbench | Detailprotokolle zu der Workbench |

Die Zusammenfassungsdatei gibt Ihnen einen Überblick über den Gesamtzustand und die Integrität der Anwendung. Die Zusammenfassung enthält empfohlene Aktionen sowie Metadaten zu laufenden Diensten und zeigt die wichtigsten Fehler auf.

Der Ordner **Metrics** enthält Metriken verschiedener Systemkomponenten über einen bestimmten Zeitraum. Die Ausgabedatei `\Details\Workbench\apiMetrics.txt` enthält z. B. eine Zusammenfassung der verschiedenen Antwortcodes und Reaktionszeiten während der gesamten Sammlungsdauer. Der Ordner **Details** enthält detaillierte Protokolle für die Behandlung bestimmter Probleme mit Workbench oder dem zugrunde liegenden Blockchain-Netzwerk. `\Details\Workbench\Exceptions.csv` enthält beispielsweise eine Liste der letzten im System aufgetretenen Ausnahmen, die für die Problembehandlung von Fehlern bei Smart Contracts oder Interaktionen mit der Blockchain nützlich ist. 

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Azure Blockchain Workbench Application Insights – Handbuch zur Problembehandlung](https://aka.ms/workbenchtroubleshooting)
