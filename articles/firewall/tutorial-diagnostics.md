---
title: 'Tutorial: Überwachen von Azure Firewall-Protokollen und -Metriken'
description: In diesem Tutorial erfahren Sie, wie Sie Azure Firewall-Protokolle und -Metriken aktivieren und verwalten.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: tutorial
ms.date: 10/24/2018
ms.author: victorh
ms.openlocfilehash: 393441e7ff620f3795e42c2cb376f99f8763f25b
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/12/2019
ms.locfileid: "60193185"
---
# <a name="tutorial-monitor-azure-firewall-logs-and-metrics"></a>Tutorial: Überwachen von Azure Firewall-Protokollen und -Metriken

Azure Firewall kann mithilfe von Firewallprotokollen überwacht werden. Sie können aber auch Aktivitätsprotokolle verwenden, um Vorgänge für Azure Firewall-Ressourcen zu überwachen. Mithilfe von Metriken können Sie Leistungsindikatoren im Portal anzeigen. 

Sie können auf einige dieser Protokolle über das Portal zugreifen. Protokolle können an [Azure Monitor-Protokolle](../azure-monitor/insights/azure-networking-analytics.md), Storage und Event Hubs gesendet und in Azure Monitor-Protokollen oder durch andere Tools wie Excel oder Power BI analysiert werden.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Ermöglichen der Protokollierung über das Azure-Portal
> * Aktivieren der Protokollierung mit PowerShell
> * Anzeigen und Analysieren des Aktivitätsprotokolls
> * Anzeigen und Analysieren der Netzwerk- und Anwendungsregelprotokolle
> * Anzeigen von Metriken


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Voraussetzungen

Lesen Sie vor Beginn dieses Tutorials die Informationen zu [Azure Firewall-Protokollen und -Metriken](logs-and-metrics.md), um einen Überblick über die für Azure Firewall verfügbaren Protokolle und Metriken zu erhalten.


## <a name="enable-diagnostic-logging-through-the-azure-portal"></a>Aktivieren der Diagnoseprotokollierung über das Azure-Portal

Nach dem Aktivieren der Diagnoseprotokollierung kann es noch einige Minuten dauern, bis die Daten in Ihren Protokollen erscheinen. Sollte zunächst nichts angezeigt werden, warten Sie noch ein paar Minuten, und versuchen Sie es dann erneut.

1. Öffnen Sie im Azure-Portal die Ressourcengruppe Ihrer Firewall, und klicken Sie auf die Firewall.
2. Klicken Sie unter **Überwachen** auf **Diagnoseeinstellungen**.

   Für Azure Firewall sind zwei dienstspezifische Protokolle verfügbar:

   * AzureFirewallApplicationRule
   * AzureFirewallNetworkRule

3. Klicken Sie auf **Diagnose aktivieren** , um die Erfassung von Daten zu starten.
4. Auf der Seite **Diagnoseeinstellungen** befinden sich die Einstellungen für die Diagnoseprotokolle. 
5. Da die Protokolle in diesem Beispiel in Azure Monitor-Protokollen gespeichert werden, geben Sie **Firewall log analytics** als Name ein.
6. Klicken Sie auf **An Log Analytics senden**, um Ihren Arbeitsbereich zu konfigurieren. Sie können auch Event Hubs und ein Speicherkonto verwenden, um die Diagnoseprotokolle zu speichern.
7. Klicken Sie unter **Log Analytics** auf **Konfigurieren**.
8. Klicken Sie auf der Seite „Log Analytics-Arbeitsbereiche“ auf **Neuen Arbeitsbereich erstellen**.
9. Geben Sie auf der Seite **Log Analytics-Arbeitsbereich** den Namen **firewall-oms** für den neuen **Log Analytics-Arbeitsbereich** ein.
10. Wählen Sie Ihr Abonnement aus, verwenden Sie die vorhandene Ressourcengruppe für die Firewall (**Test-FW-RG**), und wählen Sie den Standort **USA, Osten** sowie den Tarif **Free** aus.
11. Klicken Sie auf **OK**.
   ![Starten des Konfigurationsprozesses][1] OMS-Arbeitsbereiche werden jetzt als Log Analytics-Arbeitsbereiche bezeichnet.  
12. Klicken Sie unter **Protokoll** auf **AzureFirewallApplicationRule** und **AzureFirewallNetworkRule**, um Protokolle für Anwendungs- und Netzwerkregeln zu erfassen.
   ![Speichern der Diagnoseeinstellungen][2]
13. Klicken Sie auf **Speichern**.

## <a name="enable-logging-with-powershell"></a>Aktivieren der Protokollierung mit PowerShell

Die Aktivitätsprotokollierung ist automatisch für alle Resource Manager-Ressourcen aktiviert. Die Diagnoseprotokollierung muss aktiviert werden, um mit der Erfassung der Daten zu beginnen, die über diese Protokolle bereitgestellt werden.

Gehen Sie wie folgt vor, um die Diagnoseprotokollierung zu aktivieren:

1. Notieren Sie sich die Ressourcen-ID Ihres Speicherkontos, unter dem die Protokolldaten gespeichert werden. Dieser Wert hat das folgende Format: */subscriptions/\<Abonnement-ID\>/resourceGroups/\<Ressourcengruppenname\>/providers/Microsoft.Storage/storageAccounts/\<Speicherkontoname\>* .

   Sie können jedes Speicherkonto Ihres Abonnements verwenden. Sie können das Azure-Portal verwenden, um nach diesen Informationen zu suchen. Die Informationen befinden sich auf der **Eigenschaftenseiteder** Ressource.

2. Notieren Sie sich die Ressourcen-ID Ihrer Firewall, für die die Protokollierung aktiviert wird. Dieser Wert hat das folgende Format: */subscriptions/\<Abonnement-ID\>/resourceGroups/\<Ressourcengruppenname\>/providers/Microsoft.Network/azureFirewalls/\<Firewallname\>* .

   Sie können das Portal verwenden, um nach diesen Informationen zu suchen.

3. Aktivieren Sie die Diagnoseprotokollierung mit dem folgenden PowerShell-Cmdlet:

    ```powershell
    Set-AzDiagnosticSetting  -ResourceId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Network/azureFirewalls/<Firewall name> `
   -StorageAccountId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/<storage account name> `
   -Enabled $true     
    ```
    
> [!TIP] 
>Für Diagnoseprotokolle ist kein separates Speicherkonto erforderlich. Für die Nutzung des Speichers für die Zugriffs- und Leistungsprotokollierung fallen Dienstgebühren an.

## <a name="view-and-analyze-the-activity-log"></a>Anzeigen und Analysieren des Aktivitätsprotokolls

Mit einer der folgenden Methoden können Sie die Aktivitätsprotokolldaten anzeigen und analysieren:

* **Azure-Tools:** Rufen Sie Informationen aus dem Aktivitätsprotokoll über Azure PowerShell, über die Azure-Befehlszeilenschnittstelle, mithilfe der Azure-REST-API oder über das Azure-Portal ab. Detaillierte Anleitungen für die einzelnen Methoden finden Sie im Artikel [Überwachen von Vorgängen mit dem Ressourcen-Manager](../azure-resource-manager/resource-group-audit.md) .
* **Power BI:** Falls Sie noch kein [Power BI](https://powerbi.microsoft.com/pricing)-Konto besitzen, können Sie es kostenlos testen. Mithilfe des [Azure Activity Logs Content Pack for Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/) können Sie Ihre Daten mit vorkonfigurierten Dashboards analysieren, die Sie im Istzustand oder angepasst verwenden können.

## <a name="view-and-analyze-the-network-and-application-rule-logs"></a>Anzeigen und Analysieren der Netzwerk- und Anwendungsregelprotokolle

[Azure Monitor-Protokolle](../azure-monitor/insights/azure-networking-analytics.md) erfassen die Leistungsindikator- und Ereignisprotokolldateien. Die Anwendung umfasst Visualisierungen und leistungsfähige Suchfunktionen zum Analysieren Ihrer Protokolle.

Log Analytics-Beispielabfragen für Azure Firewall finden Sie unter [Log Analytics-Beispiele für Azure Firewall](log-analytics-samples.md).

Sie können auch eine Verbindung mit Ihrem Speicherkonto herstellen und die JSON-Protokolleinträge für Zugriffs- und Leistungsprotokolle abrufen. Nachdem Sie die JSON-Dateien heruntergeladen haben, können Sie diese in das CSV-Format konvertieren oder in Excel, Power BI oder einem anderen Datenvisualisierungstool anzeigen.

> [!TIP]
> Wenn Sie mit Visual Studio und den grundlegenden Konzepten zum Ändern der Werte für Konstanten und Variablen in C# vertraut sind, können Sie die [Protokollkonvertierungstools](https://github.com/Azure-Samples/networking-dotnet-log-converter) von GitHub verwenden.

## <a name="view-metrics"></a>Anzeigen von Metriken
Navigieren Sie zu einer Azure Firewall-Instanz, und klicken Sie unter **Überwachung** auf **Metriken**. Um die verfügbaren Werte anzuzeigen, wählen Sie die Dropdownliste **METRIK** aus.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Ihre Firewall für die Erfassung von Protokollen konfiguriert haben, können Sie sich als Nächstes mit dem Anzeigen der Daten in Azure Monitor-Protokollen beschäftigen.

> [!div class="nextstepaction"]
> [Azure-Netzwerküberwachungslösungen in Azure Monitor-Protokollen](../azure-monitor/insights/azure-networking-analytics.md)

[1]: ./media/tutorial-diagnostics/figure1.png
[2]: ./media/tutorial-diagnostics/figure2.png
