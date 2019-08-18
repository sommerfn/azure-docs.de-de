---
title: Azure Monitor – Unterschiede in der Protokollabfragesprache | Microsoft-Dokumentation
description: Referenzinformationen für die in Azure Monitor verwendete Abfragesprache Kusto. Enthält zusätzliche Elemente, die spezifisch für Azure Monitor sind, und Elemente, die in Azure Monitor-Protokollabfragen nicht unterstützt werden.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 08/13/2019
ms.author: bwren
ms.openlocfilehash: 9892b8884d901033a80fae4168f25da97edb5a68
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2019
ms.locfileid: "68990185"
---
# <a name="azure-monitor-log-query-language-differences"></a>Azure Monitor – Unterschiede in der Protokollabfragesprache

Zwar basieren [Protokolle in Azure Monitor](log-query-overview.md) auf [Azure Data Explorer](/azure/data-explorer) und verwenden die gleiche [Abfragesprache Kusto](/azure/kusto/query), die Sprachversion bringt jedoch einige Unterschiede mit sich. In diesem Artikel werden Elemente benannt, die sich in der für Data Explorer und der für Azure Monitor-Protokollabfragen verwendeten Version der Sprache unterscheiden.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="kql-elements-not-supported-in-azure-monitor"></a>In Azure Monitor nicht unterstützte Elemente der Abfragesprache Kusto
In den folgenden Abschnitten werden Elemente der Abfragesprache Kusto beschrieben, die in Azure Monitor nicht unterstützt werden.

### <a name="statements-not-supported-in-azure-monitor"></a>In Azure Monitor nicht unterstützte Anweisungen

* [Alias](/azure/kusto/query/aliasstatement)
* [Abfrageparameter](/azure/kusto/query/queryparametersstatement)

### <a name="functions-not-supported-in-azure-monitor"></a>In Azure Monitor nicht unterstützte Funktionen

* [cluster()](/azure/kusto/query/clusterfunction)
* [cursor_after()](/azure/kusto/query/cursorafterfunction)
* [cursor_before_or_at()](/azure/kusto/query/cursorbeforeoratfunction)
* [cursor_current(), current_cursor()](/azure/kusto/query/cursorcurrent)
* [database()](/azure/kusto/query/databasefunction)
* [current_principal()](/azure/kusto/query/current-principalfunction)
* [extent_id()](/azure/kusto/query/extentidfunction)
* [extent_tags()](/azure/kusto/query/extenttagsfunction)

### <a name="operators-not-supported-in-azure-monitor"></a>In Azure Monitor nicht unterstützte Operatoren

* [Clusterübergreifender Join-Vorgang](/azure/kusto/query/joincrosscluster)
* [externaldata-Operator](/azure/kusto/query/externaldata-operator)

### <a name="plugins-not-supported-in-azure-monitor"></a>In Azure Monitor nicht unterstützte Plug-Ins

* [Python-Plug-In](/azure/kusto/query/pythonplugin)
* [Plug-In „sql_request“](/azure/kusto/query/sqlrequestplugin)


## <a name="additional-operators-in-azure-monitor"></a>Zusätzliche Operatoren in Azure Monitor
Die folgenden Operatoren unterstützen für Azure Monitor spezifische Features und stehen außerhalb von Azure Monitor nicht zur Verfügung.

* [app()](app-expression.md)
* [workspace()](workspace-expression.md)

## <a name="next-steps"></a>Nächste Schritte

- Abrufen von Verweisen auf verschiedene [Ressourcen zum Schreiben von Azure Monitor-Protokollabfragen](query-language.md).
- Greifen Sie auf die vollständige [Referenzdokumentation für die Abfragesprache Kusto](/azure/kusto/query/) zu.
