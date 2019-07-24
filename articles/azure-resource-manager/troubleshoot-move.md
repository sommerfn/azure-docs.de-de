---
title: Beheben von Problemen beim Verschieben von Azure-Ressourcen in neue Abonnements oder Ressourcengruppen
description: Verwenden Sie Azure Resource Manager, um Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement verschieben.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: tomfitz
ms.openlocfilehash: e23d7c571a010e5bfb42e5f15368e0194272ed53
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2019
ms.locfileid: "67723342"
---
# <a name="troubleshoot-moving-azure-resources-to-new-resource-group-or-subscription"></a>Beheben von Problemen beim Verschieben von Azure-Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement

In diesem Artikel finden Sie Vorschläge zum Beheben von Problemen beim Verschieben von Ressourcen.

## <a name="upgrade-a-subscription"></a>Aktualisieren eines Abonnements

Wenn Sie ein Upgrade für Ihr Azure-Abonnement durchführen möchten (z.B. Wechsel vom kostenlosen Abonnement zur nutzungsbasierten Bezahlung), müssen Sie Ihr Abonnement konvertieren.

* Falls Sie für eine kostenlose Testversion ein Upgrade durchführen möchten, helfen Ihnen die Informationen unter [Aktualisieren Ihrer kostenlosen Testversion oder Ihres Microsoft Imagine Azure-Abonnements auf nutzungsbasierte Bezahlung](../billing/billing-upgrade-azure-subscription.md) weiter.
* Informationen zum Ändern eines Kontos mit nutzungsbasierter Bezahlung finden Sie unter [Ändern Ihres Azure-Abonnements mit nutzungsbasierter Bezahlung in ein anderes Angebot](../billing/billing-how-to-switch-azure-offer.md).

Wenn Sie das Abonnement nicht konvertieren können, können Sie [eine Azure-Supportanfrage erstellen](../azure-supportability/how-to-create-azure-support-request.md). Wählen Sie **Abonnementverwaltung** als Problemtyp aus.

## <a name="service-limitations"></a>Diensteinschränkungen

Einige Dienste erfordern zusätzliche Überlegungen beim Verschieben von Ressourcen. Wenn Sie die folgenden Dienste verschieben, sollten Sie unbedingt die Anleitungen und Einschränkungen überprüfen.

* [App Services](./move-limitations/app-service-move-limitations.md)
* [Azure DevOps Services](/azure/devops/organizations/billing/change-azure-subscription?toc=/azure/azure-resource-manager/toc.json)
* [Klassisches Bereitstellungsmodell](./move-limitations/classic-model-move-limitations.md)
* [Recovery Services](../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json)
* [Virtuelle Computer](./move-limitations/virtual-machines-move-limitations.md)
* [Virtuelle Netzwerke](./move-limitations/virtual-network-move-limitations.md)

## <a name="large-requests"></a>Umfangreiche Anforderungen

Unterteilen Sie große Verschiebevorgänge nach Möglichkeit in separate Verschiebevorgänge. Resource Manager gibt sofort einen Fehler aus, wenn ein einziger Vorgang mehr als 800 Ressourcen umfasst. Beim Verschieben von weniger als 800 Ressourcen kann jedoch ebenfalls ein Fehler durch ein Timeout auftreten.

## <a name="next-steps"></a>Nächste Schritte

Befehle zum Verschieben von Ressourcen finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](resource-group-move-resources.md).
