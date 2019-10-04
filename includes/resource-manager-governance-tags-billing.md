---
title: include file
description: include file
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/19/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: e7fcb72cecbfad2de80b844ed5281267d5e4c0c3
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2019
ms.locfileid: "67178023"
---
Nachdem Sie Tags auf Ressourcen angewendet haben, können Sie die Kosten für Ressourcen mit diesen Tags anzeigen. Es dauert eine Weile, bis die Kostenanalyse die jüngste Nutzung anzeigt, daher werden möglicherweise noch keine Kosten angezeigt. Wenn die Kosten verfügbar sind, können Sie diese für Ressourcen in verschiedenen Ressourcengruppen Ihres Abonnements anzeigen. Benutzer müssen über [Zugriff auf Abrechnungsinformationen auf Abonnementebene](../articles/billing/billing-manage-access.md) verfügen, um die Kosten sehen zu können.

Um Kosten nach Tag im Portal anzuzeigen, wählen Sie Ihr Abonnement und danach **Kostenanalyse** aus.

![Kostenanalyse](./media/resource-manager-governance-tags-billing/select-cost-analysis.png)

Filtern Sie die Anzeige nach Tagwert, und wählen Sie **Anwenden** aus.

![Anzeigen von Kosten nach Tag](./media/resource-manager-governance-tags-billing/view-costs-by-tag.png)

Sie können auch die [Azure-Abrechnungs-APIs](../articles/billing/billing-usage-rate-card-overview.md) verwenden, um Kosten programmgesteuert anzuzeigen.
