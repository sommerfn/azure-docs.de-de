---
title: Löschen und Wiederherstellen eines Azure Log Analytics-Arbeitsbereichs | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie einen Log Analytics-Arbeitsbereich löschen, wenn Sie einen Arbeitsbereich in einem persönlichen Abonnement erstellt haben oder Ihr Arbeitsbereichsmodell neu strukturieren.
ms.service: azure-monitor
ms.subservice: logs
ms.topic: conceptual
author: MGoedtel
ms.author: magoedte
ms.date: 10/28/2019
ms.openlocfilehash: 709d63b2c764049a698bc538d9ec451b4e75feaa
ms.sourcegitcommit: 38251963cf3b8c9373929e071b50fd9049942b37
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73044243"
---
# <a name="delete-and-restore-azure-log-analytics-workspace"></a>Löschen und Wiederherstellen eines Azure Log Analytics-Arbeitsbereichs

In diesem Artikel werden das Konzept des vorläufigen Löschens eines Azure Log Analytics-Arbeitsbereichs und die Wiederherstellung eines gelöschten Arbeitsbereichs erläutert. 

## <a name="considerations-when-deleting-a-workspace"></a>Überlegungen zum Löschen eines Arbeitsbereichs

Wenn Sie einen Log Analytics-Arbeitsbereich löschen, wird ein vorläufiger Löschvorgang durchgeführt, um die Wiederherstellung des Arbeitsbereichs einschließlich der zugehörigen Daten und verbundenen Agents innerhalb von 14 Tagen zu ermöglichen, unabhängig davon, ob der Löschvorgang versehentlich oder gezielt durchgeführt wurde. Nach dem Zeitraum des vorläufigen Löschens können der Arbeitsbereich und die zugehörigen Daten nicht mehr wiederhergestellt werden. Sie werden in die Warteschlange zum dauerhaften Löschen innerhalb von 30 Tagen eingereiht.

Gehen Sie beim Löschen eines Arbeitsbereichs vorsichtig vor, da er unter Umständen wichtige Daten und Konfigurationen enthält, deren Löschung sich negativ auf den Dienstvorgang auswirken kann. Überprüfen Sie die Agents, Lösungen und anderen Azure-Dienste und Quellen, deren Daten in Log Analytics gespeichert werden, z. B.:

* Verwaltungslösungen
* Azure-Automatisierung
* Auf virtuellen Windows- und Linux-Computern ausgeführte Agents
* Auf Windows- und Linux-Computern in Ihrer Umgebung ausgeführte Agents
* System Center Operations Manager

Beim vorläufigen Löschvorgang wird die Arbeitsbereichsressource gelöscht und die Berechtigung aller zugeordneten Benutzer aufgehoben. Wenn Benutzer anderen Arbeitsbereichen zugeordnet sind, können sie Log Analytics mit diesen Arbeitsbereichen weiter nutzen.

## <a name="soft-delete-behavior"></a>Verhalten des vorläufigen Löschens

Mit dem Löschvorgang des Arbeitsbereichs wird die Resource Manager-Ressource des Arbeitsbereichs gelöscht, die zugehörige Konfiguration und die zugehörigen Daten werden jedoch 14 Tage lang beibehalten, obwohl der Arbeitsbereich dem Anschein nach gelöscht ist. Alle für die Berichterstellung im Arbeitsbereich konfigurierten Agents und System Center Operations Manager-Verwaltungsgruppen werden während des Zeitraums des vorläufigen Löschens in einem verwaisten Status beibehalten. Der Dienst umfasst darüber hinaus einen Mechanismus zur Wiederherstellung des gelöschten Arbeitsbereichs einschließlich der zugehörigen Daten und verbundenen Ressourcen, bei dem der Löschvorgang im Wesentlichen rückgängig gemacht wird.

> [!NOTE] 
> Installierte Lösungen und verknüpfte Dienste wie Ihr Azure Automation-Konto werden zum Zeitpunkt der Löschung dauerhaft aus dem Arbeitsbereich entfernt und können nicht wiederhergestellt werden. Diese sollten nach dem Wiederherstellungsvorgang neu konfiguriert werden, um den vorherigen konfigurierten Zustand des Arbeitsbereichs wiederherzustellen.

Sie können einen Arbeitsbereich mithilfe von [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/remove-azurermoperationalinsightsworkspace?view=azurermps-6.13.0), über die [REST-API](https://docs.microsoft.com/rest/api/loganalytics/workspaces/delete) oder im [Azure-Portal](https://portal.azure.com) löschen.

### <a name="delete-workspace-in-azure-portal"></a>Löschen eines Arbeitsbereichs im Azure-Portal

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com), und melden Sie sich an. 
2. Wählen Sie im Azure-Portal **Alle Dienste** aus. Geben Sie in der Liste mit den Ressourcen **Log Analytics** ein. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Wählen Sie **Log Analytics-Arbeitsbereiche** aus.
3. Wählen Sie in der Liste der Log Analytics-Arbeitsbereiche einen Arbeitsbereich aus, und klicken Sie dann oben im mittleren Bereich auf **Löschen**.
   ![Löschoption im Bereich mit den Arbeitsbereichseigenschaften](media/delete-workspace/log-analytics-delete-workspace.png)
4. Wenn das Fenster mit einer Meldung angezeigt wird, in der Sie zum Bestätigen der Löschung des Arbeitsbereichs aufgefordert werden, klicken Sie auf **Ja**.
   ![Bestätigen des Löschvorgangs für den Arbeitsbereich](media/delete-workspace/log-analytics-delete-workspace-confirm.png)

## <a name="recover-workspace"></a>Wiederherstellen eines Arbeitsbereichs

Wenn Sie über Berechtigungen vom Typ „Mitwirkender“ für das Abonnement und die Ressourcengruppe verfügen, denen der Arbeitsbereich vor dem vorläufigen Löschvorgang zugeordnet war, können Sie den Arbeitsbereich einschließlich der zugehörigen Daten, der Konfiguration und der verbundenen Agents während des Zeitraums des vorläufigen Löschens wiederherstellen. Nach dem Zeitraum des vorläufigen Löschens kann der Arbeitsbereich nicht mehr wiederhergestellt werden und wird zum dauerhaften Löschen zugewiesen. Die Namen von gelöschten Arbeitsbereichen werden während des Zeitraums des vorläufigen Löschens beibehalten und können bei dem Versuch, einen neuen Arbeitsbereich zu erstellen, nicht verwendet werden.  

Sie können einen Arbeitsbereich wiederherstellen, indem Sie ihn mithilfe einer der folgenden Methoden zum Erstellen von Arbeitsbereichen neu erstellen: [PowerShell](https://docs.microsoft.com/powershell/module/az.operationalinsights/New-AzOperationalInsightsWorkspace) oder [REST-API]( https://docs.microsoft.com/rest/api/loganalytics/workspaces/createorupdate), sofern die folgenden Eigenschaften mit den Details des gelöschten Arbeitsbereichs gefüllt sind:

* Abonnement-ID
* Ressourcengruppenname
* Arbeitsbereichname
* Region

Der Arbeitsbereich und alle zugehörigen Daten sind nach dem Wiederherstellungsvorgang wieder vorhanden. Lösungen und verknüpfte Dienste wurden dauerhaft aus dem Arbeitsbereich entfernt, als dieser gelöscht wurde, und sollten daher neu konfiguriert werden, um den zuvor konfigurierten Zustand des Arbeitsbereichs wiederherzustellen. Einige Daten können nach der Wiederherstellung des Arbeitsbereichs möglicherweise nicht abgefragt werden, ehe die zugehörigen Lösungen neu installiert und deren Schemas dem Arbeitsbereich hinzugefügt wurden.

> [!NOTE]
> * Die Wiederherstellung eines Arbeitsbereichs wird im [Azure-Portal](https://portal.azure.com) nicht unterstützt. 
> * Beim erneuten Erstellen eines Arbeitsbereichs während des Zeitraums des vorläufigen Löschens wird darauf hingewiesen, dass der Name des Arbeitsbereichs bereits verwendet wird. 
> 
