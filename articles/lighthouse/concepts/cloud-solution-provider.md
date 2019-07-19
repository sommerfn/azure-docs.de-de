---
title: Azure Lighthouse und das Programm für Cloud-Lösungsanbieter
description: Bei der Verwendung der delegierten Azure-Ressourcenverwaltung ist es wichtig, die Sicherheits-und Zugriffssteuerung zu beachten.
author: JnHs
ms.service: lighthouse
ms.author: jenhayes
ms.date: 07/11/2019
ms.topic: overview
manager: carmonm
ms.openlocfilehash: 09552c66d2dc841cff8b5cb52e26dc657568e08f
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67810994"
---
# <a name="azure-lighthouse-and-the-cloud-solution-provider-program"></a>Azure Lighthouse und das Programm für Cloud-Lösungsanbieter

Als [CSP](https://docs.microsoft.com/partner-center/csp-overview)-Partner (Cloud Solution Provider) können Sie auf die durch das CSP-Programm für Ihre Kunden erstellten Azure-Abonnements zugreifen, indem Sie die Funktion [AOBO (Administer On Behalf Of, Verwalten im Namen von)](https://channel9.msdn.com/Series/cspdev/Module-11-Admin-On-Behalf-Of-AOBO) verwenden. Dieser Zugriff gestattet es Ihnen, die Abonnements Ihrer Kunden direkt zu unterstützen, zu konfigurieren und zu verwalten.

Der AOBO-Mechanismus gewährt vollständigen Zugriff auf Kundenumgebungen. Die Verwendung der delegierten Azure-Ressourcenverwaltung in Verbindung mit AOBO hilft dabei, die Sicherheit zu verbessern, indem Sie unnötigen Zugriff verringern können, indem Sie präzisere Berechtigungen für Ihre Benutzer aktivieren. 

## <a name="administer-on-behalf-of-aobo"></a>Verwalten im Namen von (AOBO)

Mit AOBO erhält jeder Benutzer mit der Rolle [Administrator-Agent](https://docs.microsoft.com/partner-center/permissions-overview#manage-commercial-transactions-in-partner-center-azure-ad-and-csp-roles) in Ihrem Mandanten AOBO-Zugriff auf Azure-Abonnements, die Sie durch das CSP-Programm erstellen. Alle Benutzer, die Zugriff auf Abonnements von Kunden benötigen, müssen Mitglied dieser Gruppe sein. AOBO lässt nicht die Flexibilität zu, unterschiedliche Gruppen zu erstellen, die mit verschiedenen Kunden zusammenarbeiten, oder verschiedene Rollen für Gruppen oder Benutzer zu aktivieren.

![Mandantenverwaltung mittels AOBO](../media/csp-1.jpg)

## <a name="azure-delegated-resource-management"></a>Delegierte Azure-Ressourcenverwaltung

Mithilfe der delegierten Azure-Ressourcenverwaltung können Sie verschiedenen Kunden oder Rollen unterschiedliche Gruppen zuweisen, wie im folgenden Diagramm dargestellt. Da Benutzer durch die delegierte Azure-Ressourcenverwaltung über die geeignete Zugriffsebene verfügen, können Sie die Anzahl der Benutzer verringern, die über die Rolle „Administrator-Agent“ verfügen (und somit über AOBO-Vollzugriff verfügen). Dies bietet mehr Sicherheit durch Einschränkung des unnötigen Zugriffs auf die Ressourcen ihrer Kunden. Es verleiht Ihnen außerdem mehr Flexibilität, um mehrere Kunden skaliert zu verwalten.

Das Onboarding eines Abonnements, das Sie über das CSP-Programm erstellt haben, erfolgt gemäß den Schritten, die in [Onboarding eines Abonnements in die delegierte Azure-Ressourcenverwaltung](../how-to/onboard-customer.md) beschrieben sind. Jeder Benutzer, der in Ihrem Mandanten über die Rolle „Administrator-Agent“ verfügt, kann dieses Onboarding durchführen.

Beachten Sie, dass Supportanfrage für Abonnements, die durch CSP-Programme erstellt wurden, nur von Benutzern generiert werden können, die die Rolle „Administrator-Agent“ im Mandanten des Dienstanbieters besitzen. Benutzer, die über die delegierte Azure-Ressourcenverwaltung hinzugefügt wurden, können in diesen Abonnements keine Supportanfragen für delegierte Ressourcen öffnen.

![Mandantenverwaltung mithilfe von AOBO und delegierter Azure-Ressourcenverwaltung](../media/csp-2.jpg)

## <a name="partner-admin-link"></a>Partneradministratorlink

Sie können Ihre MPN-ID (Microsoft Partner Network) mit ihren integrierten Abonnements verknüpfen, um ihre Wirksamkeit hinsichtlich der Kundenbindung zu verfolgen.

Wenn Sie [ein Angebot für verwaltete Dienste im Azure Marketplace veröffentlichen](../how-to/publish-managed-services-offers.md), wird Ihre MPN-ID Ihrem Herausgeberprofil und automatisch dem Angebot zugeordnet. Der von Azure-Ressourcen über dieses Angebot generierte Umsatz wird dann Ihrer Organisation zugeordnet. In Partnerberichterstattungssystemen wie Partner Center oder MPN wird die Zuordnung als Partneradministratorlink (PAL) angezeigt.

Wenn Sie [Kunden für die delegierte Azure-Ressourcenverwaltung mithilfe von Azure Resource Manager-Vorlagen integrieren](../how-to/onboard-customer.md), können Sie Ihre MPN-ID dennoch zuordnen, um Anerkennung für Ihre Wirksamkeit hinsichtlich der Kundenbindung zu erhalten, aber Sie müssen dies manuell durchführen. Weitere Informationen finden Sie unter [Verknüpfen einer Partner-ID mit Ihren Azure-Konten](https://docs.microsoft.com/azure/billing/billing-partner-admin-link-started). 

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie über [Mandantenübergreifende Verwaltungsmöglichkeiten](cross-tenant-management-experience.md).
- Erhalten Sie Informationen über das [Programm für Cloud-Lösungsanbieter](https://docs.microsoft.com/partner-center/csp-overview).
