---
title: Azure Lighthouse-Beispiele und -Vorlagen
description: Diese Beispiele und Azure Resource Manager-Vorlagen veranschaulichen, wie Sie das Onboarding von Kunden für die delegierte Azure-Ressourcenverwaltung durchführen und Azure Lighthouse-Szenarien unterstützen.
ms.topic: sample
ms.date: 10/17/2019
ms.openlocfilehash: 20c1fc62c83202f97ed9cff68fd285280d94a321
ms.sourcegitcommit: 2d3740e2670ff193f3e031c1e22dcd9e072d3ad9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2019
ms.locfileid: "74132166"
---
# <a name="azure-lighthouse-samples"></a>Azure Lighthouse-Beispiele

Die folgende Tabelle enthält Links zu wichtigen Azure Resource Manager-Vorlagen für Azure Lighthouse. Diese und weitere Dateien finden Sie auch im [Azure Lighthouse-Beispielrepository](https://github.com/Azure/Azure-Lighthouse-samples/).

## <a name="onboarding-customers-to-azure-delegated-resource-management"></a>Onboarding von Kunden für delegierte Azure-Ressourcenverwaltung durchführen

Wir stellen verschiedene Vorlagen bereit, um bestimmte Onboardingszenarien zu behandeln. Wählen Sie die am besten geeignete Option aus, und achten Sie darauf, die Parameterdatei an Ihre Umgebung anzupassen. Weitere Informationen zur Verwendung dieser Dateien in der Bereitstellung finden Sie unter [Onboarding eines Kunden für delegierte Azure-Ressourcenverwaltung durchführen](../how-to/onboard-customer.md).

| **Vorlage** | **Beschreibung** |
|---------|---------|
| [delegated-resource-management](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/delegated-resource-management) | Führt das Onboarding des Abonnements eines Kunden der delegierten Azure-Ressourcenverwaltung durch. Für jedes Abonnement muss eine separate Bereitstellung durchgeführt werden. |
| [rg-delegated-resource-management](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/rg-delegated-resource-management) | Führt das Onboarding einer oder mehrerer Ressourcengruppen eines Kunden für die delegierte Azure-Ressourcenverwaltung durch. Verwenden Sie **rgDelegatedResourceManagement**  für eine einzelne Ressourcengruppe oder **multipleRgDelegatedResourceManagement**, um das Onboarding für mehrere Ressourcengruppen im selben Abonnement durchzuführen. |
| [marketplace-delegated-resource-management](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/marketplace-delegated-resource-management) | Wenn Sie [ein Angebot für verwaltete Dienste im Azure Marketplace](../how-to/publish-managed-services-offers.md) veröffentlicht haben, können Sie diese Vorlage optional zum Onboarding von Ressourcen für Kunden verwenden, die das Angebot angenommen haben. Die Marketplace-Werte in der Parameterdatei müssen mit den Werten identisch sein, die Sie beim Veröffentlichen Ihres Angebots verwendet haben. |

In der Regel ist für jedes Abonnement, für das ein Onboarding durchgeführt wird, eine separate Bereitstellung erforderlich. Sie können jedoch auch Vorlagen für mehrere Abonnements bereitstellen.

| **Vorlage** | **Beschreibung** |
|---------|---------|
| [cross-subscription-deployment](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/cross-subscription-deployment) | Stellt Azure Resource Manager-Vorlagen in mehreren Abonnements bereit. |

## <a name="azure-policy"></a>Azure Policy

Diese Beispiele zeigen, wie Sie Azure Policy mit Abonnements verwenden, für die ein Onboarding für die delegierte Azure-Ressourcenverwaltung durchgeführt wurde.

| **Vorlage** | **Beschreibung** |
|---------|---------|
| [policy-add-or-replace-tag](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/policy-add-or-replace-tag) | Weist eine Richtlinie zu, die ein Tag (mit dem Modify-Effekt) einem delegierten Abonnement hinzufügt oder es daraus entfernt. Weitere Informationen finden Sie unter [Bereitstellen einer Richtlinie, die innerhalb eines delegierten Abonnements gewartet werden kann](../how-to/deploy-policy-remediation.md). |
| [policy-audit-delegation](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/policy-audit-delegation) | Weist eine Richtlinie zu, die Überprüfungen auf Delegierungszuweisungen durchführt. |
| [policy-enforce-keyvault-monitoring](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/policy-enforce-keyvault-monitoring) | Weist eine Richtlinie zu, die die Diagnose für Azure Key Vault-Ressourcen in einem delegierten Abonnement aktiviert (mit dem deployIfNotExists-Effekt). Weitere Informationen finden Sie unter [Bereitstellen einer Richtlinie, die innerhalb eines delegierten Abonnements gewartet werden kann](../how-to/deploy-policy-remediation.md). |
| [policy-enforce-sub-monitoring](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/policy-enforce-sub-monitoring) | Weist mehrere Richtlinien zum Aktivieren der Diagnose für ein delegiertes Abonnement zu und verbindet alle virtuellen Windows- und Linux-Computer mit dem von der Richtlinie erstellten Log Analytics-Arbeitsbereich. Weitere Informationen finden Sie unter [Bereitstellen einer Richtlinie, die innerhalb eines delegierten Abonnements gewartet werden kann](../how-to/deploy-policy-remediation.md). |
| [policy-initiative](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/policy-initiative) | Wendet eine Initiative (mehrere verwandte Richtliniendefinitionen) auf ein delegiertes Abonnement an. |

## <a name="azure-monitor"></a>Azure Monitor

Diese Beispiele zeigen, wie Sie Azure Monitor zum Erstellen von Warnungen für Abonnements verwenden, für die ein Onboarding für die delegierte Azure-Ressourcenverwaltung durchgeführt wurde.

| **Vorlage** | **Beschreibung** |
|---------|---------|
| [alert-using-actiongroup](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/alert-using-actiongroup) | Mit dieser Vorlage werden eine Azure-Warnung erstellt und eine Verbindung mit einer vorhandenen Aktionsgruppe hergestellt.|
| [multiple-loganalytics-alerts](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/multiple-loganalytics-alerts) | Erstellt mehrere Protokollwarnungen basierend auf Kusto-Abfragen.|
| [delegation-alert-for-customer](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/delegation-alert-for-customer) | Stellt eine Warnung in einem Mandanten bereit, wenn ein Benutzer ein Abonnement an einen verwalteten Mandanten delegiert.|

## <a name="additional-cross-tenant-scenarios"></a>Weitere mandantenübergreifende Szenarien

Diese Beispiele veranschaulichen verschiedene Aufgaben, die in mandantenübergreifenden Verwaltungsszenarien ausgeführt werden können.

| **Vorlage** | **Beschreibung** |
|---------|---------|
| [cross-rg-deployment](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/cross-rg-deployment) | Stellt Speicherkonten in zwei verschiedenen Ressourcengruppen bereit.|
| [deploy-azure-mgmt-services](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/deploy-azure-mgmt-services) | Erstellt Azure-Verwaltungsdienste, verknüpft sie miteinander und stellt zusätzliche Lösungen bereit. Verwenden Sie für eine End-to-End-Bereitstellung die Vorlage **rgWithAzureMgmt.json**. |
| [deploy-azure-security-center](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/deploy-azure-security-center) | Aktiviert und konfiguriert das Azure Security Center innerhalb des gewünschten Azure-Abonnements. |
| [deploy-azure-sentinel](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/deploy-azure-sentinel) | Stellt Azure Sentinel für einen vorhandenen Log Analytics-Arbeitsbereich in einem delegierten Abonnement bereit und aktiviert die Lösung. |
| [deploy-log-analytics-vm-extensions](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/deploy-log-analytics-vm-extensions) | Mithilfe dieser Vorlagen können Sie Log Analytics-Erweiterung für virtuelle Computer für Ihre Windows- und Linux-VMs bereitstellen und sie mit dem vorgesehenen Log Analytics-Arbeitsbereich verbinden. |

## <a name="next-steps"></a>Nächste Schritte

- Informationen zur [delegierten Azure-Ressourcenverwaltung](../concepts/azure-delegated-resource-management.md)
- Erkunden Sie das [Repository mit Azure Lighthouse-Beispielen](https://github.com/Azure/Azure-Lighthouse-samples/).
