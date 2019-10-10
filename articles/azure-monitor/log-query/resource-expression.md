---
title: resource()-Ausdruck in Azure Monitor-Protokollabfragen | Microsoft-Dokumentation
description: Der resource-Ausdruck wird in einer Azure Monitor-Protokollabfrage mit Ressourcenschwerpunkt verwendet, um Daten von mehreren Ressourcen abzurufen.
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
ms.date: 09/10/2018
ms.author: bwren
ms.openlocfilehash: deca6e7ef1c231a82a73067971d86a6e9cdd0599
ms.sourcegitcommit: 80da36d4df7991628fd5a3df4b3aa92d55cc5ade
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2019
ms.locfileid: "71817373"
---
# <a name="resource-expression-in-azure-monitor-log-query"></a>resource()-Ausdruck in Azure Monitor-Protokollabfragen

Der `resource`-Ausdruck wird in einer Azure Monitor-Abfrage [mit dem Umfang einer Ressource](scope.md#query-scope) verwendet, um Daten von anderen Ressourcen abzurufen. 


## <a name="syntax"></a>Syntax

`resource(`*Bezeichner*`)`

## <a name="arguments"></a>Argumente

- *Bezeichner*: Die Ressourcen-ID einer Ressource.

| Bezeichner | BESCHREIBUNG | Beispiel
|:---|:---|:---|
| Resource | Schließt Daten für die Ressource ein. | resource("/subscriptions/xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcesgroups/myresourcegroup/providers/microsoft.compute/virtualmachines/myvm") |
| Ressourcengruppe oder Abonnement | Enthält Daten für die Ressource und alle darin enthaltenen Ressourcen.  | resource("/subscriptions/xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcesgroups/myresourcegroup) |


## <a name="notes"></a>Notizen

* Sie benötigen Lesezugriff auf die Ressource.


## <a name="examples"></a>Beispiele

```Kusto
union (Heartbeat),(resource("/subscriptions/xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcesgroups/myresourcegroup/providers/microsoft.compute/virtualmachines/myvm").Heartbeat) | summarize count() by _ResourceId, TenantId
```
```Kusto
union (Heartbeat),(resource("/subscriptions/xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcesgroups/myresourcegroup).Heartbeat) | summarize count() by _ResourceId, TenantId
```


## <a name="next-steps"></a>Nächste Schritte

- Details zum Abfragebereich finden Sie unter [Protokollabfragebereich und Zeitbereich in Azure Monitor Log Analytics](scope.md).
- Greifen Sie auf die vollständige Dokumentation für die [Abfragesprache Kusto](/azure/kusto/query/) zu.
