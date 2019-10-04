---
title: Erstellen einer Diagnoseeinstellung zum Erfassen von Protokollen und Metriken in Azure | Microsoft-Dokumentation
description: Erstellen Sie Diagnoseeinstellungen, um Protokolle der Azure-Plattform an Azure Monitor-Protokolle, Azure Storage oder Azure Event Hubs weiterzuleiten.
author: bwren
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 07/31/2019
ms.author: bwren
ms.subservice: logs
ms.openlocfilehash: ec1842d534dcb1e9ddef149d3ae879677b29e715
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "71263025"
---
# <a name="create-diagnostic-setting-to-collect-platform-logs-and-metrics-in-azure"></a>Erstellen einer Diagnoseeinstellung zum Erfassen von Plattformprotokollen und Metriken in Azure
[Plattformprotokolle](resource-logs-overview.md) in Azure liefern detaillierte Diagnose- und Überwachungsinformationen für Azure-Ressourcen und die Azure-Plattform, von der sie abhängen. Dieser Artikel enthält Details zum Erstellen und Konfigurieren von Diagnoseeinstellungen, um Plattformprotokolle an verschiedenen Zielen zu erfassen.

Jede Azure-Ressource erfordert eine eigene Diagnoseeinstellung. Mit der Diagnoseeinstellung wird Folgendes für die jeweilige Ressource definiert:

- Kategorien der Protokolle und Metrikdaten, die an die in der Einstellung definierten Ziele gesendet werden. Die verfügbaren Kategorien sind je nach Ressourcentyp verschieden.
- Ein oder mehrere Ziele zum Senden der Protokolle. Zu den aktuellen Zielen gehören der Log Analytics-Arbeitsbereich, Event Hubs und Azure Storage.
- Aufbewahrungsrichtlinie für Daten, die in Azure Storage gespeichert werden.
 
Mit einer einzelnen Diagnoseeinstellung kann eines der Ziele definiert werden. Wenn Sie Daten an mehrere Ziele eines bestimmten Typs senden möchten (z.B. zwei verschiedene Log Analytics-Arbeitsbereiche), dann erstellen Sie mehrere Einstellungen. Jede Ressource kann bis zu fünf Diagnoseeinstellungen aufweisen.

> [!NOTE]
> Das Aktivitätsprotokoll kann an die gleichen Ziele wie die anderen Plattformprotokolle weitergeleitet werden, ist jedoch nicht mit Diagnoseeinstellungen konfiguriert. Ausführliche Informationen dazu finden Sie unter [Übersicht über Plattformprotokolle in Azure](platform-logs-overview.md#destinations).

> [!NOTE]
> [Plattformmetriken](metrics-supported.md) werden automatisch in den [Azure Monitor-Metriken](data-platform-metrics.md) erfasst. Mit Diagnoseeinstellungen können Metriken für bestimmte Azure-Dienste in Azure Monitor-Protokollen erfasst werden, um dann mit anderen Überwachungsdaten anhand von [Protokollabfragen](../log-query/log-query-overview.md) analysiert zu werden.

## <a name="destinations"></a>Destinations 
Plattformprotokolle können an die Ziele in der folgenden Tabelle gesendet werden. Die Konfiguration für jedes Ziel wird mithilfe desselben Verfahrens zum Erstellen von Diagnoseeinstellungen vorgenommen, das in diesem Artikel beschrieben ist. Folgen Sie den einzelnen Links in der folgenden Tabelle, um ausführliche Informationen zum Senden von Daten an das jeweilige Ziel zu erhalten.

| Destination | BESCHREIBUNG |
|:---|:---|
| [Log Analytics-Arbeitsbereich](resource-logs-collect-workspace.md) | Das Erfassen von Protokollen in einem Log Analytics-Arbeitsbereich gibt Ihnen die Möglichkeit, diese Daten zusammen mit anderen Überwachungsdaten, die mithilfe leistungsstarker Protokollabfragen von Azure Monitor erfasst werden, zu analysieren sowie andere Azure Monitor-Features wie Warnungen und Visualisierungen zu nutzen. |
| [Event Hubs](resource-logs-stream-event-hubs.md) | Das Senden von Protokollen an Event Hubs ermöglicht Ihnen das Streamen von Daten an externe Systeme, z.B. SIEMs und andere Protokollanalyselösungen von Drittanbietern. |
| [Azure-Speicherkonto](resource-logs-collect-storage.md) | Das Archivieren von Protokollen in einem Azure-Speicherkonto ist für die Überwachung, statische Analyse oder Sicherung nützlich. |




## <a name="create-diagnostic-settings-in-azure-portal"></a>Erstellen von Diagnoseeinstellungen im Azure-Portal
Sie können Diagnoseeinstellungen im-Azure-Portal entweder über das Azure Monitor-Menü oder über das Menü für die Ressource konfigurieren.

1. Klicken Sie im Azure Monitor-Menü des Azure-Portals unter **Einstellungen** auf **Diagnoseeinstellungen**, und klicken Sie dann auf die Ressource.

    ![Diagnoseeinstellungen](media/diagnostic-settings/menu-monitor.png)

    Sie können auch im Ressourcenmenü des Azure-Portals unter **Überwachen** auf **Diagnoseeinstellungen** klicken.

    ![Diagnoseeinstellungen](media/diagnostic-settings/menu-resource.png)

2. Wenn keine Einstellungen für die Ressource vorhanden sind, die Sie ausgewählt haben, werden Sie aufgefordert, eine Einstellung zu erstellen. Klicken Sie auf **Diagnose aktivieren**.

   ![Diagnoseeinstellung hinzufügen – keine Einstellungen vorhanden](media/diagnostic-settings/add-setting.png)

   Wenn Einstellungen für die Ressource vorhanden sind, sehen Sie eine Liste der bereits konfigurierten Einstellungen. Klicken Sie entweder auf **Diagnoseeinstellung hinzufügen**, um eine neue Einstellung hinzuzufügen, oder auf **Einstellung bearbeiten**, um eine vorhandene Einstellung zu bearbeiten. Jede Einstellung kann höchstens einen der Zieltypen aufweisen.

   ![Diagnoseeinstellung hinzufügen – Einstellungen vorhanden](media/diagnostic-settings/edit-setting.png)

3. Wenn noch kein Name für die Einstellung vorhanden ist, geben Sie ihr einen Namen.
4. Aktivieren Sie das Kontrollkästchen für jedes Ziel, um die Protokolle zu senden. Klicken Sie auf **Konfigurieren**, um die jeweiligen Einstellungen anzugeben, die in der folgenden Tabelle beschrieben sind.

    | Einstellung | BESCHREIBUNG |
    |:---|:---|
    | Log Analytics-Arbeitsbereich | Name des Arbeitsbereichs. |
    | Speicherkonto | Name des Speicherkontos. |
    | Event Hub-Namespace | Der Namespace für die Event Hub-Erstellung (falls Sie erstmals Protokolle streamen) oder für das Streaming (falls bereits Ressourcen vorhanden sind, die diese Protokollkategorie an diesen Namespace streamen).
    | Event Hub-Name | Geben Sie optional einen Event Hub-Namen an, um alle Daten in der Einstellung zu senden. Wenn Sie keinen Namen angeben, wird eine Event Hub für jede Protokollkategorie erstellt. Wenn Sie mehrere Kategorien senden, können Sie einen Namen angeben, um die Anzahl der erstellten Event Hubs zu beschränken. Ausführliche Informationen dazu finden Sie unter [Kontingente und Grenzwerte in Azure Event Hubs](../../event-hubs/event-hubs-quotas.md). |
    | Event Hub-Richtlinienname | Definiert die Berechtigungen für den Streamingmechanismus. |

    ![Diagnoseeinstellung hinzufügen – Einstellungen vorhanden](media/diagnostic-settings/setting-details.png)

5. Aktivieren Sie die Kontrollkästchen für die einzelnen Datenkategorien zum Senden an die angegebenen Ziele. Wenn Sie die Option **In einem Speicherkonto archivieren** ausgewählt haben, müssen Sie auch den [Aufbewahrungszeitraum](resource-logs-collect-storage.md#data-retention) angeben.



> [!NOTE]
> Das Senden mehrdimensionaler Metriken über die Diagnoseeinstellungen wird derzeit nicht unterstützt. Metriken mit Dimensionen werden als vereinfachte eindimensionale Metriken exportiert und dimensionswertübergreifend aggregiert.
>
> *Beispiel*: Die Metrik „Eingehende Nachrichten“ in einem Event Hub kann auf Warteschlangenebene untersucht und in einem Diagramm dargestellt werden. Wenn Sie die Metrik allerdings über die Diagnoseeinstellungen exportieren, umfasst die Darstellung alle eingehenden Nachrichten für alle Warteschlangen im Event Hub.

4. Klicken Sie auf **Speichern**.

Nach einigen Augenblicken wird die neue Einstellung in der Liste der Einstellungen für diese Ressource angezeigt, und Protokolle werden an die angegebenen Ziele gestreamt, sobald neue Ereignisdaten generiert werden. Beachten Sie, dass zwischen der Ausgabe eines Ereignisses und der [Anzeige in einem Log Analytics-Arbeitsbereich](data-ingestion-time.md) möglicherweise bis zu 15 Minuten vergehen.



## <a name="create-diagnostic-settings-using-powershell"></a>Erstellen von Diagnoseeinstellungen mithilfe von PowerShell
Verwenden Sie das Cmdlet [Set-AzDiagnosticSetting](https://docs.microsoft.com/en-us/powershell/module/az.monitor/set-azdiagnosticsetting), um eine Diagnoseeinstellung mit [Azure PowerShell](powershell-quickstart-samples.md) zu erstellen. Beschreibungen der zugehörigen Parameter finden Sie in der Dokumentation zu diesem Cmdlet.

Es folgt ein Beispiel für ein PowerShell-Cmdlet zum Erstellen einer Diagnoseeinstellung, die alle drei Ziele verwendet.


```powershell
Set-AzDiagnosticSetting -Name KeyVault-Diagnostics -ResourceId /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.KeyVault/vaults/mykeyvault -Category AuditEvent -MetricCategory AllMetrics -Enabled $true -StorageAccountId /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount -RetentionEnabled $true -RetentionInDays 7 -WorkspaceId /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/myworkspace  -EventHubAuthorizationRuleId /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.EventHub/namespaces/myeventhub/authorizationrules/RootManageSharedAccessKey
```


## <a name="create-diagnostic-settings-using-azure-cli"></a>Erstellen von Diagnoseeinstellungen mithilfe der Azure CLI
Verwenden Sie den Befehl [az monitor diagnostic-settings create](https://docs.microsoft.com/cli/azure/monitor/diagnostic-settings?view=azure-cli-latest#az-monitor-diagnostic-settings-create), um eine Diagnoseeinstellung mit der [Azure CLI](https://docs.microsoft.com/cli/azure/monitor?view=azure-cli-latest) zu erstellen. Beschreibungen der zugehörigen Parameter finden Sie in der Dokumentation zu diesem Befehl.

Es folgt ein Beispiel für einen CLI-Befehl zum Erstellen einer Diagnoseeinstellung, die alle drei Ziele verwendet.



```azurecli
az monitor diagnostic-settings create  \
--name KeyVault-Diagnostics \
--resource /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.KeyVault/vaults/mykeyvault \
--logs    '[{"category": "AuditEvent","enabled": true,"retentionPolicy": {"days": 7,"enabled": true}}]' \
--metrics '[{"category": "AllMetrics","enabled": true,"retentionPolicy": {"days": 7,"enabled": true}}]' \
--storage-account /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount \
--workspace /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/myworkspace \
--event-hub-rule /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myresourcegroup/providers/Microsoft.EventHub/namespaces/myeventhub/authorizationrules/RootManageSharedAccessKey
```

### <a name="configure-diagnostic-settings-using-rest-api"></a>Konfigurieren von Diagnoseeinstellungen mithilfe der REST-API
Informationen zum Erstellen oder Aktualisieren von Diagnoseeinstellungen mithilfe der [Azure Monitor-REST-API](https://docs.microsoft.com/rest/api/monitor/) finden Sie unter [Diagnoseeinstellungen](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings).


### <a name="configure-diagnostic-settings-using-resource-manager-template"></a>Konfigurieren von Diagnoseeinstellungen mithilfe einer Resource Manager-Vorlage
Informationen zum Erstellen oder Aktualisieren von Diagnoseeinstellungen mit einer Resource Manager-Vorlage finden Sie unter [Automatisches Aktivieren von Diagnoseeinstellungen bei der Ressourcenerstellung mithilfe einer Resource Manager-Vorlage](diagnostic-settings-template.md).

## <a name="next-steps"></a>Nächste Schritte

* [Weitere Informationen zu Protokollen der Azure-Plattform](resource-logs-overview.md)