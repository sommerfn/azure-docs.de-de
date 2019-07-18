---
title: Streamen von Azure-Diagnoseprotokollen an einen Log Analytics-Arbeitsbereich in Azure Monitor
description: Hier erfahren Sie, wie Sie Azure-Diagnoseprotokolle an einen Log Analytics-Arbeitsbereich in Azure Monitor streamen.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/18/2019
ms.author: johnkem
ms.subservice: logs
ms.openlocfilehash: 13eb1a8fcea2f74cda5921a51b8c2e8816be975f
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2019
ms.locfileid: "67303708"
---
# <a name="stream-azure-diagnostic-logs-to-log-analytics-workspace-in-azure-monitor"></a>Streamen von Azure-Diagnoseprotokollen an einen Log Analytics-Arbeitsbereich in Azure Monitor

**[Azure-Diagnose-Protokolle](diagnostic-logs-overview.md)** können über das Portal, über PowerShell-Cmdlets oder die Azure CLI nahezu in Echtzeit an einen Log Analytics-Arbeitsbereich in Azure Monitor gestreamt werden.

## <a name="what-you-can-do-with-diagnostics-logs-in-a-log-analytics-workspace"></a>Verwendungsmöglichkeiten für Diagnoseprotokolle in einem Log Analytics-Arbeitsbereich

Azure Monitor bietet ein flexibles Tool zur Protokollabfrage und -analyse, mit dem Sie einen Einblick in die unformatierten Protokolldaten erhalten, die aus den Azure-Ressourcen generiert werden. Zu den Funktionen gehören:

* **Protokollabfrage**: Schreiben Sie erweiterte Abfragen für Ihre Protokolldaten, stellen Sie Zusammenhänge zwischen Protokollen aus verschiedenen Quellen her, und generieren Sie Diagramme, die an Ihr Azure-Dashboard angeheftet werden können.
* **Warnungen**: Stellen Sie Übereinstimmungen von Ereignissen mit einer bestimmten Abfrage fest, und lassen Sie sich per E-Mail oder Webhookaufruf mithilfe von Azure Monitor-Warnungen benachrichtigen.
* **Erweiterte Analysen**: Wenden Sie Machine Learning- und Musterabgleichsalgorithmen an, um mögliche Probleme zu ermitteln, die durch Ihre Protokolle aufgedeckt werden.

## <a name="enable-streaming-of-diagnostic-logs-to-log-analytics-workspace"></a>Aktivieren des Streamens von Diagnoseprotokollen an einen Log Analytics-Arbeitsbereich

Das Streamen von Diagnoseprotokollen kann programmgesteuert, über das Portal oder mithilfe der [Azure Monitor-REST-APIs](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings)aktiviert werden. In jedem Fall erstellen Sie eine Diagnoseeinstellung, in der Sie einen Log Analytics-Arbeitsbereich sowie die Protokollkategorien und Metriken angeben, die an den Arbeitsbereich gesendet werden sollen. Eine **Diagnoseprotokollkategorie** ist ein Protokolltyp, der von einer Ressource bereitgestellt werden kann.

Der Log Analytics-Arbeitsbereich muss sich nicht unter demselben Abonnement befinden wie die Ressource, die Protokolle ausgibt, sofern der Benutzer, der die Einstellung konfiguriert, den entsprechenden RBAC-Zugriff auf beide Abonnements hat.

> [!NOTE]
> Das Senden mehrdimensionaler Metriken über die Diagnoseeinstellungen wird derzeit nicht unterstützt. Metriken mit Dimensionen werden als vereinfachte eindimensionale Metriken exportiert und dimensionswertübergreifend aggregiert.
>
> *Beispiel*: Die Metrik „Eingehende Nachrichten“ in einem Event Hub kann auf Warteschlangenebene untersucht und in einem Diagramm dargestellt werden. Wenn Sie die Metrik allerdings über die Diagnoseeinstellungen exportieren, umfasst die Darstellung alle eingehenden Nachrichten für alle Warteschlangen im Event Hub.
>
>

## <a name="stream-diagnostic-logs-using-the-portal"></a>Streamen von Diagnoseprotokollen mithilfe des Portals
1. Navigieren Sie im Portal zu Azure Monitor, und klicken Sie im Menü **Einstellungen** auf **Diagnoseeinstellungen**.


2. Filtern Sie optional die Liste nach Ressourcengruppe oder Ressourcentyp, und klicken Sie auf die Ressource, für die Sie eine Diagnoseeinstellung festlegen möchten.

3. Wenn keine Einstellungen für die Ressource vorhanden sind, die Sie ausgewählt haben, werden Sie aufgefordert, eine Einstellung zu erstellen. Klicken Sie auf „Diagnose aktivieren“.

   ![Diagnoseeinstellung hinzufügen – keine Einstellungen vorhanden](media/diagnostic-logs-stream-log-store/diagnostic-settings-none.png)

   Wenn Einstellungen für die Ressource vorhanden sind, sehen Sie eine Liste der Einstellungen, die bereits für diese Ressource konfiguriert sind. Klicken Sie auf „Diagnoseeinstellung hinzufügen“.

   ![Diagnoseeinstellung hinzufügen – Einstellungen vorhanden](media/diagnostic-logs-stream-log-store/diagnostic-settings-multiple.png)

3. Legen Sie einen Namen für Ihre Einstellung fest, und aktivieren Sie das Kontrollkästchen **An Log Analytics senden**. Wählen Sie dann einen Log Analytics-Arbeitsbereich aus.

   ![Diagnoseeinstellung hinzufügen – Einstellungen vorhanden](media/diagnostic-logs-stream-log-store/diagnostic-settings-configure.png)

4. Klicken Sie auf **Speichern**.

Nach einigen Augenblicken wird die neue Einstellung in der Liste der Einstellungen für diese Ressource angezeigt, und Diagnoseprotokolle werden an diesen Arbeitsbereich gestreamt, sobald neue Ereignisdaten generiert werden. Möglicherweise vergehen bis zu 15 Minuten zwischen der Ausgabe eines Ereignisses und dessen Anzeige in Log Analytics.

### <a name="via-powershell-cmdlets"></a>Verwenden von PowerShell-Cmdlets

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Wenn Sie das Streaming über die [Azure PowerShell-Cmdlets](../../azure-monitor/platform/powershell-quickstart-samples.md) aktivieren möchten, können Sie das Cmdlet `Set-AzDiagnosticSetting` mit folgenden Parametern verwenden:

```powershell
Set-AzDiagnosticSetting -ResourceId [your resource ID] -WorkspaceID [resource ID of the Log Analytics workspace] -Categories [list of log categories] -Enabled $true
```

Beachten Sie, dass die workspaceID-Eigenschaft die gesamte Azure-Ressourcen-ID des Arbeitsbereichs benötigt, nicht die im Log Analytics-Portal angezeigte Kombination aus Arbeitsbereichs-ID und Schlüssel.

### <a name="via-azure-cli"></a>Verwenden der Azure-Befehlszeilenschnittstelle

Zum Ermöglichen des Streamings per [Azure CLI](../../azure-monitor/platform/cli-samples.md) können Sie den Befehl [az monitor diagnostic-settings create](/cli/azure/monitor/diagnostic-settings#az-monitor-diagnostic-settings-create) verwenden.

```azurecli
az monitor diagnostic-settings create --name <diagnostic name> \
    --workspace <log analytics name or object ID> \
    --resource <target resource object ID> \
    --resource-group <log analytics workspace resource group> \
    --logs '[
    {
        "category": <category name>,
        "enabled": true
    }
    ]'
```

Sie können das Diagnoseprotokoll um weitere Kategorien ergänzen, indem Sie dem JSON-Array, das als `--logs`-Parameter übergeben wird, Wörterbücher hinzufügen.

Das Argument `--resource-group` ist nur erforderlich, wenn `--workspace` keine Objekt-ID ist.

## <a name="how-do-i-query-the-data-from-a-log-analytics-workspace"></a>Wie frage ich die Daten aus einem Log Analytics-Arbeitsbereich ab?

Auf dem Blatt „Protokolle“ im Azure Monitor-Portal können Sie Diagnoseprotokolle im Rahmen der Protokollverwaltungslösung unter der AzureDiagnostics-Tabelle abfragen. Es gibt auch [verschiedene Überwachungslösungen für Azure-Ressourcen](../../azure-monitor/insights/solutions.md), die Sie installieren können, um sofort Informationen zu den an Azure Monitor gesendeten Protokolldaten zu erhalten.

## <a name="azure-diagnostics-vs-resource-specific"></a>Der Azure-Diagnose-Modus und der ressourcenspezifische Modus im Vergleich  
Sobald ein Log Analytics-Ziel in einer Azure-Diagnose-Konfiguration aktiviert wird, gibt es zwei Möglichkeiten für die Anzeige von Daten in Ihrem Arbeitsbereich:  
- **Azure-Diagnose:** Legacymethode, die heutzutage von den meisten Azure-Diensten genutzt wird. In diesem Modus werden alle Daten zu einer Diagnose-Einstellung, die auf einen bestimmten Arbeitsbereich verweisen, in der Tabelle _AzureDiagnostics_ angezeigt. 
<br><br>Da viele Ressourcen Daten an dieselbe Tabelle (_AzureDiagnostics_) senden, stellt das Schema dieser Tabelle die Obermenge der Schemas aller erfassten einzelnen Datentypen dar. Wenn Sie beispielsweise Diagnose-Einstellungen für die Sammlung der folgenden Datentypen erstellt haben, die alle an denselben Arbeitsbereich gesendet werden, wird Folgendes generiert:
    - Überwachungsprotokolle der Ressource 1 (mit einem Schema, das aus den Spalten A, B und C besteht)  
    - Fehlerprotokolle der Ressource 2 (mit einem Schema, das aus den Spalten D, E und F besteht)  
    - Datenflussprotokolle der Ressourcen 3 (mit einem Schema, das aus den Spalten G, H und I besteht)  

    Die Azure-Diagnosetabelle sieht mit einigen Beispieldaten wie folgt aus:  

    | ResourceProvider | Category (Kategorie) | Eine Datei | b | C | D | E | F | G | H | I |
    | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- |
    | Microsoft.Resource1 | AuditLogs | x1 | y1 | z1 |
    | Microsoft.Resource2 | ErrorLogs | | | | q1 | w1 | e1 |
    | Microsoft.Resource3 | DataFlowLogs | | | | | | | j1 | k1 | l1|
    | Microsoft.Resource2 | ErrorLogs | | | | q2 | w2 | e2 |
    | Microsoft.Resource3 | DataFlowLogs | | | | | | | j3 | k3 | l3|
    | Microsoft.Resource1 | AuditLogs | x5 | y5 | z5 |
    | ... |

- **Ressourcenspezifisch:** In diesem Modus werden individuelle Tabellen in dem ausgewählten Arbeitsbereich für die einzelnen Kategorien erstellt, die bei der Konfiguration der Diagnose-Einstellungen ausgewählt werden. Diese neuere Methode erleichtert die Suche nach Daten durch eine explizite Unterteilung in verschiedene Aspekte: Es gibt zu jeder Kategorie eine Tabelle. Außerdem werden dynamische Typen unterstützt. Für ausgewählte Azure-Ressourcentypen wie [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-analyze-activity-logs-log-analytics)- oder [Intune](https://docs.microsoft.com/intune/review-logs-using-azure-monitor)-Protokolle ist dieser Modus bereits verfügbar. Es wird erwartet, dass mit der Zeit sämtliche Datentypen zum ressourcenspezifischen Modus migrieren. 

    Im obigen Beispiel hätte das zur Folge, dass zwei Tabellen erstellt werden: 
    - Die Tabelle _AuditLogs_:

        | ResourceProvider | Category (Kategorie) | Eine Datei | b | C |
        | -- | -- | -- | -- | -- |
        | Microsoft.Resource1 | AuditLogs | x1 | y1 | z1 |
        | Microsoft.Resource1 | AuditLogs | x5 | y5 | z5 |
        | ... |

    - Die Tabelle _ErrorLogs_:  

        | ResourceProvider | Category (Kategorie) | D | E | F |
        | -- | -- | -- | -- | -- | 
        | Microsoft.Resource2 | ErrorLogs | q1 | w1 | e1 |
        | Microsoft.Resource2 | ErrorLogs | q2 | w2 | e2 |
        | ... |

    - Die Tabelle _DataFlowLogs_:  

        | ResourceProvider | Category (Kategorie) | G | H | I |
        | -- | -- | -- | -- | -- | 
        | Microsoft.Resource3 | DataFlowLogs | j1 | k1 | l1|
        | Microsoft.Resource3 | DataFlowLogs | j3 | k3 | l3|
        | ... |

    Zudem hat die Verwendung des ressourcenspezifischen Modus u. a. den Vorteil, dass die Leistung sowohl in Bezug auf die Latenz bei der Datenerfassung als auch auf die Abfragezeiten verbessert wird, dass Schemas und die zugehörigen Strukturen besser erkannt werden können und dass RBAC-Rechte für bestimmte Rollen vergeben werden können.

### <a name="selecting-azure-diagnostic-vs-resource-specific-mode"></a>Auswählen des Azure-Diagnose-Modus und des ressourcenspezifischen Modus im Vergleich
Bei den meisten Azure-Ressourcen können Sie nicht zwischen dem Azure-Diagnose-Modus oder dem ressourcenspezifischen Modus auswählen. Stattdessen wählt die jeweilige Ressource automatisch eine Methode für die mit ihr im Zusammenhang stehenden Daten aus. Wenn Sie wissen möchten, welcher Modus angewendet wird, finden Sie weitere Informationen in der Dokumentation zu der jeweiligen Ressource, die Sie für das Senden von Daten an Log Analytics ausgewählt haben. 

Wie bereits im vorherigen Abschnitt erwähnt, ist unser langfristiges Ziel, dass für alle Azure-Dienste unter Azure Monitor der ressourcenspezifische Modus verwendet wird. Damit dieser Übergang erleichtert wird und sichergestellt ist, dass in diesem Zusammenhang keine Daten verloren gehen, bieten Ihnen einige Azure-Dienste eine Auswahlmöglichkeit, sobald Sie ein Onboarding auf Log Analytics durchführen:  
   ![Auswahl eines Modus in den Diagnose-Einstellungen](media/diagnostic-logs-stream-log-store/diagnostic-settings-mode-selector.png)

Es wird **dringend** empfohlen, für jede neu erstellte Diagnose-Einstellung den ressourcenspezifischen Modus auszuwählen, um spätere komplizierte Migrationen zu vermeiden.  

Für bereits vorhandene Diagnose-Einstellungen können Sie rückwirkend vom Azure-Diagnose-Modus zum ressourcenspezifischen Modus wechseln, sobald diese Möglichkeit für die jeweilige Azure-Ressource aktiviert wurde. Bereits erfasste Daten sind weiterhin so lange über die Tabelle _AzureDiagnostics_ abrufbar, bis sie gemäß Ihren Einstellungen für die Datenaufbewahrung gelöscht werden. Jegliche neuen Daten werden hingegen an die jeweilige dedizierte Tabelle gesendet. Das bedeutet, dass für alle neuen Abfragen, die sowohl alte als auch neue Daten betreffen (bis die alten Daten gelöscht werden), ein [Union](https://docs.microsoft.com/azure/kusto/query/unionoperator)-Operator benötigt wird, um die beiden Datasets zu verknüpfen.

Informieren Sie sich im Blog zu [Azure-Updates](https://azure.microsoft.com/updates/) über Neuigkeiten zu Azure-Diensten, die Protokolle im ressourcenspezifischen Modus unterstützen.

### <a name="known-limitation-column-limit-in-azurediagnostics"></a>Bekannte Einschränkung: Spaltenlimit in Azure-Diagnose
Eine einzelne Azure-Protokolltabelle ist explizit auf maximal 500 Spalten begrenzt. Sobald dieser Wert erreicht ist, werden alle Zeilen, die Daten in einer Spalte außerhalb der ersten 500 Spalten enthalten, zum Zeitpunkt der Erfassung gelöscht. Die Azure-Diagnosetabelle ist besonders anfällig für diese Begrenzung. Üblicherweise wird sie relevant, wenn eine Vielzahl von Datenquellen oder mehrere umfangreiche Datenquellen an denselben Arbeitsbereich gesendet werden. 

#### <a name="azure-data-factory"></a>Azure Data Factory  
Azure Data Factory ist aufgrund sehr detaillierter Protokolle eine Ressource, die besonders von dieser Begrenzung betroffen ist. Insbesondere gilt Folgendes für sämtliche Diagnose-Einstellungen, die konfiguriert wurden, bevor der ressourcenspezifische Modus aktiviert oder aufgrund von Abwärtskompatibilität ausgewählt wurde:  
- *Für eine Aktivität in Ihrer Pipeline definierte Benutzerparameter*: Für jeden eindeutig benannten Benutzerparameter wird für jede Aktivität eine neue Spalte erstellt. 
- *Aktivitätseingaben und -ausgaben:* Diese variieren von Aktivität zu Aktivität und generieren eine hohe Anzahl von Spalten, weil sie sehr umfangreich sind. 
 
Wie in den umfassenderen Vorschlägen zur Problemumgehung beschrieben, wird empfohlen, Protokolle zu migrieren, um den ressourcenspezifischen Modus so schnell wie möglich verwenden zu können. Wenn Sie dafür nicht sofort eine Gelegenheit finden, können Sie als Zwischenlösung ADF-Protokolle in einem eigenen Arbeitsbereich isolieren, um die Wahrscheinlichkeit zu minimieren, dass diese Protokolle Auswirkungen auf andere Protokolltypen haben, die in Ihren Arbeitsbereichen erfasst werden. 
 
#### <a name="workarounds"></a>Problemumgehungen
Bis der ressourcenspezifische Modus für alle Azure-Dienste aktiviert ist, wird als kurzfristige Lösung für Dienste empfohlen, die den Modus noch nicht unterstützen, ausführliche Datentypen, die von diesen Diensten veröffentlicht werden, in separate Arbeitsbereiche zu unterteilen. Dadurch wird die Wahrscheinlichkeit verringert, dass die Grenzwerte erreicht werden.  
 
Längerfristig soll ein Übergang vom Azure-Diagnose-Modus hin zu allen Azure-Diensten erfolgen, die den ressourcenspezifischen Modus unterstützen. Es wird empfohlen, so schnell wie möglich zu diesem Modus zu wechseln, um Probleme aufgrund der Grenze von 500 Spalten zu vermeiden.  


## <a name="next-steps"></a>Nächste Schritte

* [Informieren Sie sich ausführlicher über Azure-Diagnoseprotokolle.](diagnostic-logs-overview.md)

