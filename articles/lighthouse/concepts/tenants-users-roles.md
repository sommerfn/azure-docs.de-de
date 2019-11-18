---
title: Mandanten, Rollen und Benutzer in Azure Lighthouse-Szenarien
description: Erfahren Sie mehr über die Konzepte von Azure Active Directory-Mandanten, -Benutzern und -Rollen sowie deren Verwendung in Azure Lighthouse-Szenarien.
ms.date: 11/05/2019
ms.topic: overview
ms.openlocfilehash: 73c5cd592f07a23edaad23796e498ea9243c5d26
ms.sourcegitcommit: 2d3740e2670ff193f3e031c1e22dcd9e072d3ad9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2019
ms.locfileid: "74131347"
---
# <a name="tenants-roles-and-users-in-azure-lighthouse-scenarios"></a>Mandanten, Rollen und Benutzer in Azure Lighthouse-Szenarien

Vor dem Onboarding von Kunden für die [delegierte Azure-Ressourcenverwaltung](azure-delegated-resource-management.md) ist es wichtig, die Funktionsweise von Azure Active Directory-Mandanten, -Benutzern und -Rollen sowie deren Verwendung in Azure Lighthouse-Szenarien zu verstehen.

Ein *Mandant* ist eine dedizierte und vertrauenswürdige Instanz von Azure Active Directory (Azure AD). Typischerweise repräsentiert jeder Mandant eine einzelne Organisation. Die delegierte Azure-Ressourcenverwaltung ermöglicht die logische Projektion von Ressourcen von einem Mandanten auf einen anderen Mandanten. Dies ermöglicht es Benutzern im verwaltenden Mandanten (z. B. dem Mandanten eines Dienstanbieters), auf delegierte Ressourcen in einem Kundenmandanten zuzugreifen. [Unternehmen mit mehreren Mandanten können auf diese Weise ihre Verwaltungsabläufe zentralisieren](enterprise.md).

Um diese logische Projektion zu erreichen, muss für ein Abonnement (oder mindestens eine Ressourcengruppe in einem Abonnement) im Kundenmandanten ein *Onboarding* für die delegierte Azure-Ressourcenverwaltung durchgeführt werden. Dieser Onboardingprozess kann entweder [über Azure Resource Manager-Vorlagen](../how-to/onboard-customer.md) oder durch das [Veröffentlichen eines öffentlichen oder privaten Angebots im Azure Marketplace](../how-to/publish-managed-services-offers.md) erfolgen.

Unabhängig von der gewählten Onboardingmethode müssen Sie *Autorisierungen* definieren. Jede Autorisierung gibt ein Benutzerkonto im verwaltenden Mandanten an, der Zugriff auf die delegierten Ressourcen erhält. Außerdem wird eine integrierte Rolle angegeben, mit der die Berechtigungen dieser Benutzer für die Ressourcen festgelegt werden.

## <a name="role-support-for-azure-delegated-resource-management"></a>Rollenunterstützung für die delegierte Azure-Ressourcenverwaltung

Beim Definieren einer Autorisierung muss jedem Benutzerkonto eine der [integrierten Rollen für die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) zugewiesen werden. Benutzerdefinierte Rollen und [klassische Abonnementadministratorrollen](https://docs.microsoft.com/azure/role-based-access-control/classic-administrators) werden nicht unterstützt.

Für die delegierte Azure-Ressourcenverwaltung werden aktuell alle [integrierten Rollen](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) unterstützt. Dabei gelten die folgenden Ausnahmen:

- Die Rolle [Besitzer](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#owner) wird nicht unterstützt.
- Integrierte Rollen mit der Berechtigung [DataActions](https://docs.microsoft.com/azure/role-based-access-control/role-definitions#dataactions) werden nicht unterstützt.
- Die integrierte Rolle [Benutzerzugriffsadministrator](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#user-access-administrator) wird unterstützt, aber nur für den eingeschränkten Zweck der [Zuweisung von Rollen zu einer verwalteten Identität im Kundenmandanten](../how-to/deploy-policy-remediation.md#create-a-user-who-can-assign-roles-to-a-managed-identity-in-the-customer-tenant). Weitere Berechtigungen, die üblicherweise über diese Rolle gewährt werden, werden nicht angewendet. Wenn Sie einen Benutzer mit dieser Rolle definieren, müssen Sie auch die integrierten Rollen angeben, die dieser Benutzer verwalteten Identitäten zuweisen kann.

## <a name="best-practices-for-defining-users-and-roles"></a>Best Practices für das Definieren von Benutzern und Rollen

Beim Erstellen Ihrer Autorisierungen empfehlen wir die Einhaltung der folgenden Best Practices:

- In den meisten Fällen sollten Sie einer Azure AD-Benutzergruppe oder einem Dienstprinzipal Berechtigungen zuzuweisen, anstatt eine Reihe einzelner Benutzerkonten. Auf diese Weise können Sie den Zugriff für einzelne Benutzer hinzufügen oder entfernen, ohne den Plan aktualisieren und erneut veröffentlichen zu müssen, wenn sich ihre Zugriffsanforderungen ändern.
- Beachten Sie, dass Sie das Prinzip der geringsten Rechte befolgen, damit Benutzer nur über die Berechtigungen verfügen, die zum Durchführen Ihrer Aufgaben erforderlich sind, um die Wahrscheinlichkeit von unbeabsichtigten Fehlern zu verringern. Weitere Informationen finden Sie unter [Empfohlene Sicherheitsmaßnahmen](../concepts/recommended-security-practices.md).
- Fügen Sie einen Benutzer mit der [Rolle „Registrierungszuweisung für verwaltete Dienste löschen“](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#managed-services-registration-assignment-delete-role) hinzu, damit Sie den [Zugriff auf die Delegierung](../how-to/onboard-customer.md#remove-access-to-a-delegation) später bei Bedarf entfernen können. Wenn diese Rolle nicht zugewiesen wird, können delegierte Ressourcen nur durch einen Benutzer im Kundenmandanten entfernt werden.
- Stellen Sie sicher, dass allen Benutzern, die die [Seite „Meine Kunden“ im Azure-Portal anzeigen](../how-to/view-manage-customers.md) müssen, die Rolle [Leser](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#reader) (oder eine andere integrierte Rolle mit Lesezugriff) zugewiesen wird.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [empfohlene Sicherheitsmaßnahmen für die delegierte Azure-Ressourcenverwaltung](recommended-security-practices.md).
- Onboarding Ihrer Kunden in die delegierte Azure-Ressourcenverwaltung, entweder unter [Verwendung von Azure Resource Manager-Vorlagen](../how-to/onboard-customer.md) oder mittels [Veröffentlichung eines privaten oder öffentlichen Angebots für verwaltete Dienste im Azure Marketplace](../how-to/publish-managed-services-offers.md).
