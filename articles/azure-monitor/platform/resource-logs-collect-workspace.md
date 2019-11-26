---
title: Erfassen von Azure-Ressourcenprotokollen im Log Analytics-Arbeitsbereich
description: Hier erfahren Sie, wie Sie Azure-Ressourcenprotokolle an einen Log Analytics-Arbeitsbereich in Azure Monitor streamen.
author: bwren
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/20/2019
ms.author: bwren
ms.subservice: logs
ms.openlocfilehash: 92de47041791c8b6c540844adb62391268b81c34
ms.sourcegitcommit: fa5ce8924930f56bcac17f6c2a359c1a5b9660c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2019
ms.locfileid: "73200501"
---
# <a name="collect-azure-resource-logs-in-log-analytics-workspace-in-azure-monitor"></a>Erfassen von Azure-Ressourcenprotokollen im Log Analytics-Arbeitsbereich in Azure Monitor
[Ressourcenprotokolle](resource-logs-overview.md) in Azure liefern regelmäßig umfassende Daten zum internen Betrieb einer Azure-Ressource. In diesem Artikel wird beschrieben, wie Sie Ressourcenprotokolle in einem Log Analytics-Arbeitsbereich erfassen. Dies gibt Ihnen die Möglichkeit, diese Daten zusammen mit anderen Überwachungsdaten, die mithilfe leistungsstarker Protokollabfragen in Azure Monitor-Protokollen erfasst werden, zu analysieren sowie andere Azure Monitor-Features wie Warnungen und Visualisierungen zu nutzen. 


## <a name="what-you-can-do-with-resource-logs-in-a-workspace"></a>Verwendungsmöglichkeiten für Ressourcenprotokolle in einem Arbeitsbereich
Durch das Erfassen von Ressourcenprotokollen in einem Log Analytics-Arbeitsbereich können Sie die Protokolle aller Azure-Ressourcen zusammen analysieren und die gesamte Bandbreite von Funktionen nutzen, die für [Azure Monitor-Protokolle](data-platform-logs.md) bereitstehen. Dazu gehört Folgendes:

* **Protokollabfragen**: Erstellen Sie [Protokollabfragen](../log-query/log-query-overview.md) mithilfe einer leistungsfähigen Abfragesprache, um Ihre Diagnosedaten schnell zu analysieren, Erkenntnisse aus diesen Daten zu gewinnen und sie zusammen mit Daten, die aus anderen Quellen in Azure Monitor erfasst werden, zu analysieren.
* **Warnungen**: Erhalten Sie mithilfe von [Protokollwarnungen in Azure Monitor](alerts-log.md) proaktive Benachrichtigungen über kritische Bedingungen und Muster, die in Ihren Ressourcenprotokollen erkannt werden.
* **Visualisierungen**: Heften Sie die Ergebnisse einer Protokollabfrage an ein Azure-Dashboard an, oder fügen Sie diese als Teil eines interaktiven Berichts in eine Arbeitsmappe ein.

## <a name="prerequisites"></a>Voraussetzungen
Falls Sie noch nicht über einen Arbeitsbereich verfügen, müssen Sie [einen neuen Arbeitsbereich erstellen](../learn/quick-create-workspace.md). Der Arbeitsbereich muss sich nicht in demselben Abonnement befinden wie die Ressource, die Protokolle sendet, sofern der Benutzer, der die Einstellung konfiguriert, den entsprechenden RBAC-Zugriff auf beide Abonnements besitzt.

## <a name="create-a-diagnostic-setting"></a>Erstellen einer Diagnoseeinstellung
Ressourcenprotokolle werden standardmäßig nicht erfasst. Erfassen Sie diese in einem Log Analytics-Arbeitsbereich und anderen Zielen, indem Sie eine Diagnoseeinstellung für eine Azure-Ressource erstellen. Weitere Informationen finden Sie unter [Erstellen einer Diagnoseeinstellung zum Erfassen von Protokollen und Metriken in Azure](diagnostic-settings.md).

## <a name="collection-mode"></a>Sammlungsmodus
In einem Log Analytics-Arbeitsbereich gesammelte Daten werden in Tabellen gespeichert, wie es unter [Struktur von Azure Monitor-Protokollen](../log-query/logs-structure.md) beschrieben ist. Welche Tabellen von Ressourcenprotokollen verwendet werden, hängt von dem von der Ressource verwendeten Sammlungstyp ab:

- Azure-Diagnose: Alle Daten werden in die Tabelle _AzureDiagnostics_ geschrieben.
- Ressourcenspezifisch: Daten werden für jede Kategorie der Ressource in eine individuelle Tabelle geschrieben.

### <a name="azure-diagnostics-mode"></a>Modus „Azure-Diagnose“ 
In diesem Modus werden alle Daten aus einer beliebigen [Diagnoseeinstellung](diagnostic-settings.md) in der Tabelle _AzureDiagnostics_ gesammelt. Dies ist die Legacymethode, die heute von den meisten Azure-Diensten genutzt wird.

Da mehrere Ressourcentypen Daten an dieselbe Tabelle senden, stellt deren Schema die Obermenge der Schemas aller erfassten einzelnen Datentypen dar.

Sehen Sie sich das folgende Beispiel an, in dem Diagnoseeinstellungen für die folgenden Datentypen im gleichen Arbeitsbereich gesammelt werden:

- Überwachungsprotokolle von Dienst 1 (mit einem Schema, das aus den Spalten A, B und C besteht)  
- Fehlerprotokolle von Dienst 1 (mit einem Schema, das aus den Spalten D, E und F besteht)  
- Überwachungsprotokolle von Dienst 2 (mit einem Schema, das aus den Spalten G, H und I besteht)  

Die Tabelle „AzureDiagnostics“ sieht dann wie folgt aus:  

| ResourceProvider    | Category     | Eine Datei  | b  | C  | D  | E  | F  | G  | H  | I  |
| -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- |
| Microsoft.Service1 | AuditLogs    | x1 | y1 | z1 |    |    |    |    |    |    |
| Microsoft.Service1 | ErrorLogs    |    |    |    | q1 | w1 | e1 |    |    |    |
| Microsoft.Service2 | AuditLogs    |    |    |    |    |    |    | j1 | k1 | l1 |
| Microsoft.Service1 | ErrorLogs    |    |    |    | q2 | w2 | e2 |    |    |    |
| Microsoft.Service2 | AuditLogs    |    |    |    |    |    |    | j3 | k3 | l3 |
| Microsoft.Service1 | AuditLogs    | x5 | y5 | z5 |    |    |    |    |    |    |
| ... |

### <a name="resource-specific"></a>Modus „Ressourcenspezifisch“
In diesem Modus werden individuelle Tabellen in dem ausgewählten Arbeitsbereich für die einzelnen Kategorien erstellt, die in der Diagnoseeinstellung ausgewählt werden. Diese Methode wird empfohlen, da sie ein einfacheres Arbeiten mit den Daten in Protokollabfragen ermöglicht, bessere Auffindbarkeit von Schemas und deren Struktur bietet, die Leistung sowohl in Hinsicht auf die Erfassungslatenz als auch Abfragezeiten verbessert und die Möglichkeit bietet, RBAC-Rechte für eine bestimmte Tabelle zu gewähren. Alle Azure-Dienste werden letztendlich in den Modus „Ressourcenspezifisch“ migriert. 

Im obigen Beispiel hätte das zur Folge, dass drei Tabellen erstellt werden:
 
- Tabelle *Service1AuditLogs* wie folgt:

    | Ressourcenanbieter | Category | Eine Datei | b | C |
    | -- | -- | -- | -- | -- |
    | Service1 | AuditLogs | x1 | y1 | z1 |
    | Service1 | AuditLogs | x5 | y5 | z5 |
    | ... |

- Tabelle *Service1ErrorLogs* wie folgt:  

    | Ressourcenanbieter | Category | D | E | F |
    | -- | -- | -- | -- | -- | 
    | Service1 | ErrorLogs |  q1 | w1 | e1 |
    | Service1 | ErrorLogs |  q2 | w2 | e2 |
    | ... |

- Tabelle *Service2AuditLogs* wie folgt:  

    | Ressourcenanbieter | Category | G | H | I |
    | -- | -- | -- | -- | -- |
    | Service2 | AuditLogs | j1 | k1 | l1|
    | Service2 | AuditLogs | j3 | k3 | l3|
    | ... |



### <a name="select-the-collection-mode"></a>Auswählen des Sammlungsmodus
Die meisten Azure-Ressourcen schreiben Daten entweder im Modus **Azure-Diagnose** oder im Modus **Ressourcenspezifisch** in den Arbeitsbereich, ohne dass Sie eine Auswahl treffen können. Ausführliche Informationen zum jeweils verwendeten Modus finden Sie in der [Dokumentation zu den einzelnen Diensten](diagnostic-logs-schema.md). Alle Azure-Dienste verwenden letztendlich den Modus „Ressourcenspezifisch“. Im Rahmen dieses Übergangs können Sie bei einigen Ressourcen einen Modus in der Diagnoseeinstellung auswählen. Sie sollten den Modus „Ressourcenspezifisch“ für alle neuen Diagnoseeinstellungen angeben, da die Daten dadurch einfacher zu verwalten sind und komplexe Migrationen zu einem späteren Zeitpunkt vermieden werden können.
  
   ![Auswahl eines Modus in den Diagnose-Einstellungen](media/resource-logs-collect-workspace/diagnostic-settings-mode-selector.png)




> [!NOTE]
> Derzeit können der Modus **Azure-Diagnose** und der Modus **Ressourcenspezifisch** nur beim Konfigurieren der Diagnoseeinstellung im Azure-Portal ausgewählt werden. Wenn Sie die Einstellung mithilfe der CLI, PowerShell oder der Rest-API konfigurieren, wird standardmäßig **Azure-Diagnose** verwendet.

Sie können eine vorhandene Diagnoseeinstellung in den Modus „Ressourcenspezifisch“ ändern. In diesem Fall verbleiben Daten, die bereits gesammelt wurden, in der Tabelle _AzureDiagnostics_, bis sie gemäß Ihrer Aufbewahrungseinstellung für den Arbeitsbereich entfernt werden. Neue Daten werden in der dedizierten Tabelle gesammelt. Verwenden Sie den [union](https://docs.microsoft.com/azure/kusto/query/unionoperator)-Operator, um Daten in beiden Tabellen abzufragen.

Sehen Sie regelmäßig im Blog [Azure-Updates](https://azure.microsoft.com/updates/) nach, ob Ankündigungen für Azure-Dienste vorliegen, die den Modus „Ressourcenspezifisch“ unterstützen.

### <a name="column-limit-in-azurediagnostics"></a>Spaltenlimit in AzureDiagnostics
Für jede Tabelle in Azure Monitor-Protokollen besteht ein Eigenschaftenlimit von 500. Sobald dieses Limit erreicht ist, werden alle Zeilen, die Daten mit einer Eigenschaft außerhalb der ersten 500 enthalten, zum Zeitpunkt der Erfassung gelöscht. Die Tabelle *AzureDiagnostics* ist besonders anfällig für dieses Limit, da sie Eigenschaften für alle Azure-Dienste enthält, die in diese Tabelle schreiben.

Wenn Sie Diagnoseprotokolle von mehreren Diensten erfassen, kann _AzureDiagnostics_ diesen Grenzwert überschreiten, wodurch Daten fehlen. Bis alle Azure-Dienste den Modus „Ressourcenspezifisch“ unterstützen, sollten Sie Ressourcen so konfigurieren, dass sie in mehrere Arbeitsbereiche schreiben, um die Wahrscheinlichkeit zu verringern, dass das Limit von 500 Spalten erreicht wird.

### <a name="azure-data-factory"></a>Azure Data Factory
Azure Data Factory ist aufgrund eines sehr detaillierten Satzes von Protokollen ein Dienst, der bekanntermaßen eine große Anzahl von Spalten schreibt und möglicherweise dazu führt, dass _AzureDiagnostics_ den Grenzwert überschreitet. Für alle Diagnoseeinstellungen, die vor dem Aktivieren des Modus „Ressourcenspezifisch“ konfiguriert wurden, wird eine neue Spalte für jeden Benutzerparameter mit eindeutigem Namen zu jeder Aktivität erstellt. Aufgrund der Ausführlichkeit von Aktivitätseingaben und -ausgaben werden weitere Spalten erstellt.
 
Sie sollten Ihre Protokolle so schnell wie möglich zum Modus „Ressourcenspezifisch“ migrieren. Wenn Sie dafür nicht sofort eine Gelegenheit finden, können Sie als Zwischenlösung Azure Data Factory-Protokolle in einem eigenen Arbeitsbereich isolieren, um die Wahrscheinlichkeit zu minimieren, dass diese Protokolle Auswirkungen auf andere Protokolltypen haben, die in Ihren Arbeitsbereichen erfasst werden.


## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Azure-Ressourcenprotokollen finden Sie unter [Übersicht über Azure-Ressourcenprotokolle](resource-logs-overview.md).
* Informationen zum Erstellen einer Diagnoseeinstellung für das Erfassen von Ressourcenprotokollen in einem Log Analytics-Arbeitsbereich finden Sie unter [Erstellen einer Diagnoseeinstellung zum Erfassen von Protokollen und Metriken in Azure](diagnostic-settings.md).
