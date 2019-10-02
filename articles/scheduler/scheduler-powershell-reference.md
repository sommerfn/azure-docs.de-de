---
title: PowerShell-Cmdlets-Referenz – Azure Scheduler
description: Informationen zu PowerShell-Cmdlets für den Azure Scheduler
services: scheduler
ms.service: scheduler
author: derek1ee
ms.author: deli
ms.reviewer: klam
ms.assetid: 9a26c457-d7a1-4e4a-bc79-f26592155218
ms.topic: article
ms.date: 08/18/2016
ms.openlocfilehash: 1ad1d6b9f59fa51b9e27fe5b70ce4e2a5d36f3ad
ms.sourcegitcommit: 29880cf2e4ba9e441f7334c67c7e6a994df21cfe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2019
ms.locfileid: "71300879"
---
# <a name="powershell-cmdlets-reference-for-azure-scheduler"></a>PowerShell-Cmdlets-Referenz für den Azure Scheduler

> [!IMPORTANT]
> Azure Scheduler wird [eingestellt](../scheduler/migrate-from-scheduler-to-logic-apps.md#retire-date) und durch [Azure Logic Apps](../logic-apps/logic-apps-overview.md) ersetzt. Wenn Sie weiterhin mit den Aufträgen arbeiten möchten, die Sie in Scheduler eingerichtet haben, sollten Sie so bald wie möglich [zu Azure Logic Apps migrieren](../scheduler/migrate-from-scheduler-to-logic-apps.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Um Skripts zum Erstellen und Verwalten von Scheduler-Aufträgen und Auftragssammlungen zu erstellen, können Sie PowerShell-Cmdlets verwenden. Dieser Artikel enthält die wichtigsten PowerShell-Cmdlets für den Azure Scheduler mit Links zu den jeweiligen Referenzartikeln. Um Azure PowerShell für Ihr Azure-Abonnement zu installieren, lesen Sie [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview). Weitere Informationen zu [Azure Resource Manager-Cmdlets](/powershell/azure/overview) finden Sie unter [Verwenden von Azure PowerShell mit Azure Resource Manager](../powershell-azure-resource-manager.md).

| Cmdlet | BESCHREIBUNG |
|--------|-------------|
| [Disable-AzSchedulerJobCollection](/powershell/module/azurerm.scheduler/disable-azurermschedulerjobcollection) |Deaktiviert eine Auftragssammlung. |
| [Enable-AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/enable-azurermschedulerjobcollection) |Aktiviert eine Auftragssammlung. |
| [Get-AzSchedulerJob](/powershell/module/azurerm.scheduler/get-azurermschedulerjob) |Ruft Scheduler-Aufträge ab. |
| [Get-AzSchedulerJobCollection](/powershell/module/azurerm.scheduler/get-azurermschedulerjobcollection) |Ruft Auftragssammlungen ab. |
| [Get-AzSchedulerJobHistory](/powershell/module/azurerm.scheduler/get-azurermschedulerjobhistory) |Ruft den Auftragsverlauf ab. |
| [New-AzSchedulerHttpJob](/powershell/module/azurerm.scheduler/new-azurermschedulerhttpjob) |Erstellt einen HTTP-Auftrag. |
| [New-AzSchedulerJobCollection](/powershell/module/azurerm.scheduler/new-azurermschedulerjobcollection) |Erstellt eine Auftragssammlung. |
| [New-AzSchedulerServiceBusQueueJob](/powershell/module/azurerm.scheduler/new-azurermschedulerservicebusqueuejob) | Erstellt einen Service Bus-Warteschlangenauftrag. |
| [New-AzSchedulerServiceBusTopicJob](/powershell/module/azurerm.scheduler/new-azurermschedulerservicebustopicjob) |Erstellt einen Service Bus-Themenauftrag. |
| [New-AzSchedulerStorageQueueJob](/powershell/module/azurerm.scheduler/new-azurermschedulerstoragequeuejob) |Erstellt einen Storage-Warteschlangenauftrag. |
| [Remove-AzSchedulerJob](/powershell/module/azurerm.scheduler/remove-azurermschedulerjob) |Entfernt einen Scheduler-Auftrag. |
| [Remove-AzSchedulerJobCollection](/powershell/module/azurerm.scheduler/remove-azurermschedulerjobcollection) |Entfernt eine Auftragssammlung. |
| [Set-AzSchedulerHttpJob](/powershell/module/azurerm.scheduler/set-azurermschedulerhttpjob) |Ändert einen Scheduler-HTTP-Auftrag. |
| [Set-AzSchedulerJobCollection](/powershell/module/azurerm.scheduler/set-azurermschedulerjobcollection) |Ändert eine Auftragssammlung. |
| [Set-AzSchedulerServiceBusQueueJob](/powershell/module/azurerm.scheduler/set-azurermschedulerservicebusqueuejob) |Ändert einen Service Bus-Warteschlangenauftrag. |
| [Set-AzSchedulerServiceBusTopicJob](/powershell/module/azurerm.scheduler/set-azurermschedulerservicebustopicjob) |Ändert einen Service Bus-Themenauftrag. |
| [Set-AzSchedulerStorageQueueJob](/powershell/module/azurerm.scheduler/set-azurermschedulerstoragequeuejob) |Ändert einen Storage-Warteschlangenauftrag. |
||| 

Um weitere Informationen zu erhalten, können Sie eines dieser Cmdlets ausführen: 

```
Get-Help <cmdlet name> -Detailed
Get-Help <cmdlet name> -Examples
Get-Help <cmdlet name> -Full
```

## <a name="see-also"></a>Weitere Informationen

* [Was ist der Azure Scheduler?](scheduler-intro.md)
* [Konzepte, Terminologie und Entitätshierarchie](scheduler-concepts-terms.md)
* [Erstellen und Planen des ersten Auftrags – Azure-Portal](scheduler-get-started-portal.md)
* [Azure Scheduler-REST-API – Referenz](https://msdn.microsoft.com/library/mt629143)
