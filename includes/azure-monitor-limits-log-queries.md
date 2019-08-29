---
title: include file
description: include file
services: azure-monitor
author: rboucher
tags: azure-service-management
ms.topic: include
ms.date: 07/22/2019
ms.author: bwren
ms.custom: include file
ms.openlocfilehash: f007cf0d46d6cbee39a950b9784bbc9bde702ff5
ms.sourcegitcommit: 36e9cbd767b3f12d3524fadc2b50b281458122dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2019
ms.locfileid: "69657781"
---
| Begrenzung | BESCHREIBUNG |
|:---|:---|
| Abfragesprache | Azure Monitor verwendet dieselbe [Kusto-Abfragesprache](/azure/kusto/query/) wie Azure Data Explorer. Weitere Informationen finden Sie unter [Azure Monitor – Unterschiede in der Protokollabfragesprache](../articles/azure-monitor/log-query/data-explorer-difference.md) für KQL-Sprachelemente, die in Azure Monitor nicht unterstützt werden. |
| Azure-Regionen | Protokollabfragen können zu einem übermäßigen Aufwand führen, wenn sich die Daten über Log Analytics-Arbeitsbereiche in mehreren Azure-Regionen erstrecken. Weitere Informationen finden Sie unter [Abfragegrenzwerte](../articles/azure-monitor/log-query/scope.md#query-limits). |
| Ressourcenübergreifende Abfragen | Die maximale Anzahl von Application Insights-Ressourcen und Log Analytics-Arbeitsbereichen in einer einzelnen Abfrage ist auf 100 beschränkt.<br>Ressourcenübergreifende Abfrage wird im View Designer nicht unterstützt.<br>Eine ressourcenübergreifende Abfrage in Protokollwarnungen wird in der neuen scheduledQueryRules-API unterstützt.<br>Ausführliche Informationen finden Sie unter [Ressourcenübergreifende Abfragelimits](../articles/azure-monitor/log-query/cross-workspace-query.md#cross-resource-query-limits). |