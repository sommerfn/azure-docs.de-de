---
title: Autorisieren des Zugriffs mit Azure Active Directory
description: Dieser Artikel bietet Informationen zum Autorisieren des Zugriffs auf Event Hubs-Ressourcen mit Azure Active Directory.
services: event-hubs
ms.service: event-hubs
documentationcenter: ''
author: spelluru
ms.topic: conceptual
ms.date: 08/22/2019
ms.author: spelluru
ms.openlocfilehash: 0d8d1b37e7f5ebb6eef1c76e4324041c48ab8986
ms.sourcegitcommit: 1752581945226a748b3c7141bffeb1c0616ad720
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/14/2019
ms.locfileid: "70995744"
---
# <a name="authorize-access-to-event-hubs-resources-using-azure-active-directory"></a>Autorisieren des Zugriffs auf Event Hubs-Ressourcen mit Azure Active Directory
Azure Event Hubs unterstützt die Verwendung von Azure Active Directory (Azure AD) zum Autorisieren von Anforderungen an Event Hubs-Ressourcen. Mit Azure AD können Sie die rollenbasierte Zugriffssteuerung (RBAC) zum Gewähren von Berechtigungen für einen Sicherheitsprinzipal verwenden, bei dem es sich um einen Benutzer oder einen Anwendungsdienstprinzipal handeln kann. Weitere Informationen zu Rollen und Rollenzuweisungen finden Sie unter [Grundlegendes zu den verschiedenen Rollen](../role-based-access-control/overview.md).

## <a name="overview"></a>Übersicht
Wenn ein Sicherheitsprinzipal (ein Benutzer oder eine Anwendung) versucht, auf eine Event Hubs-Ressource zuzugreifen, muss die Anforderung autorisiert werden. Mit Azure AD ist der Zugriff auf eine Ressource ein zweistufiger Prozess. 

 1. Zunächst wird die Identität des Sicherheitsprinzipals authentifiziert, und ein OAuth 2.0-Token wird zurückgegeben. Der Ressourcenname zum Anfordern eines Tokens ist `https://eventhubs.azure.net/`.
 1. Anschließend wird das Token als Teil einer Anforderung an den Event Hubs-Dienst übergeben, um den Zugriff auf die angegebene Ressource zu autorisieren.

Für den Authentifizierungsschritt ist es erforderlich, dass eine Anwendungsanforderung zur Laufzeit ein OAuth 2.0-Zugriffstoken enthält. Wenn eine Anwendung in einer Azure-Entität, z.B. einem virtuellen Azure-Computer, einer VM-Skalierungsgruppe oder einer Azure Functions-App, ausgeführt wird, kann der Zugriff auf die Ressourcen über eine verwaltete Identität erfolgen. Informationen zum Authentifizieren von Anforderungen, die von einer verwalteten Identität an den Event Hubs-Dienst übermittelt werden, finden Sie unter [Authentifizieren des Zugriffs auf Azure Event Hubs-Ressourcen mit Azure Active Directory und verwalteten Identitäten für Azure-Ressourcen](authenticate-managed-identity.md). 

Für den Autorisierungsschritt ist es erforderlich, dem Sicherheitsprinzipal mindestens eine RBAC-Rolle zuzuweisen. Azure Event Hubs stellt RBAC-Rollen bereit, die Berechtigungssätze für Event Hubs-Ressourcen enthalten. Die möglichen Berechtigungen eines Sicherheitsprinzipals sind durch die Rollen vorgegeben, die dem Prinzipal zugewiesen sind. Weitere Informationen zu RBAC-Rollen finden Sie unter [Integrierte RBAC-Rollen für Azure Event Hubs](#built-in-rbac-roles-for-azure-event-hubs). 

Native Anwendungen und Webanwendungen, die Anforderungen an Event Hubs senden, können die Autorisierung ebenfalls mit Azure AD durchführen. Informationen zum Anfordern eines Zugriffstokens und seiner Verwendung zum Autorisieren von Anforderungen für Event Hubs-Ressourcen finden Sie unter [Authentifizieren des Zugriffs auf Azure Event Hubs mit Azure AD über eine Anwendung](authenticate-application.md). 

## <a name="assign-rbac-roles-for-access-rights"></a>Zuweisen von RBAC-Rollen für Zugriffsrechte
Azure Active Directory (Azure AD) autorisiert Rechte für den Zugriff auf abgesicherte Ressourcen über die [rollenbasierte Zugriffssteuerung (RBAC)](../role-based-access-control/overview.md). Azure Event Hubs definiert eine Reihe von integrierten RBAC-Rollen, die allgemeine Berechtigungssätze für den Zugriff auf Event Hub-Daten umfassen. Sie können auch benutzerdefinierte Rollen für den Zugriff auf die Daten definieren.

Wenn einem Azure AD-Sicherheitsprinzipal eine RBAC-Rolle zugewiesen wird, gewährt Azure diesem Sicherheitsprinzipal Zugriff auf diese Ressourcen. Der Zugriff kann sich auf den Bereich des Abonnements, der Ressourcengruppe, des Event Hubs-Namespace oder jeder darin vorhandenen Ressource beziehen. Ein Azure AD-Sicherheitsprinzipal kann ein Benutzer, ein Anwendungsdienstprinzipal oder eine [verwaltete Identität für Azure-Ressourcen](../active-directory/managed-identities-azure-resources/overview.md) sein.

## <a name="built-in-rbac-roles-for-azure-event-hubs"></a>Integrierte RBAC-Rollen für Azure Event Hubs
Azure stellt die folgenden integrierten Rollen für die rollenbasierte Zugriffssteuerung zum Autorisieren des Zugriffs auf Event Hubs-Daten mittels Azure AD und OAuth bereit:

- [Azure Event Hubs-Datenbesitzer](../role-based-access-control/built-in-roles.md#azure-event-hubs-data-owner): Mit dieser Rolle gewähren Sie Vollzugriff auf Event Hubs-Ressourcen.
- [Azure Event Hubs-Datensender](../role-based-access-control/built-in-roles.md#azure-event-hubs-data-receiver): Mit dieser Rolle gewähren Sie Sendezugriff auf Event Hubs-Ressourcen.
- [Azure Event Hubs-Datenempfänger](../role-based-access-control/built-in-roles.md#azure-event-hubs-data-sender): Mit dieser Rolle gewähren Sie Verbrauchs-/Sendezugriff auf Event Hubs-Ressourcen.

## <a name="resource-scope"></a>Ressourcenumfang 
Bevor Sie einem Sicherheitsprinzipal eine RBAC-Rolle zuweisen, legen Sie den Zugriffsbereich fest, den der Sicherheitsprinzipal haben soll. Es hat sich als am besten bewährt, stets nur den kleinstmöglichen Umfang an Zugriffsrechten zu gewähren.

In der folgenden Liste werden die Ebenen beschrieben, auf denen Sie den Zugriff auf Event Hubs-Ressourcen einschränken können, beginnend mit dem kleinstmöglichen Bereich:

- **Consumergruppe**: Bei diesem Umfang gilt die Rollenzuweisung nur für diese Entität. Derzeit unterstützt das Azure-Portal keine Zuweisung einer RBAC-Rolle an einen Sicherheitsprinzipal auf dieser Ebene. 
- **Event Hub**: Die Rollenzuweisung gilt für die Event Hub-Entität und die darunter vorhandene Consumergruppe.
- **Namespace**: Die Rollenzuweisung umfasst die gesamte Topologie von Event Hubs unter dem Namespace sowie die ihm zugeordnete Consumergruppe.
- **Ressourcengruppe**: Die Rollenzuweisung gilt für alle Event Hubs-Ressourcen unter der Ressourcengruppe.
- **Abonnement**: Die Rollenzuweisung gilt für alle Event Hubs-Ressourcen in allen Ressourcengruppen im Abonnement.

> [!NOTE]
> Denken Sie daran, dass die Rollenzuweisung für die rollenbasierte Zugriffssteuerung bis zu fünf Minuten dauern kann. 

Weitere Informationen dazu, wie integrierte Rollen definiert sind, finden Sie unter [Grundlegendes zu Rollendefinitionen](../role-based-access-control/role-definitions.md#management-and-data-operations). Weitere Informationen zum Erstellen benutzerdefinierter RBAC-Rollen finden Sie unter [Erstellen benutzerdefinierter Rollen für die rollenbasierte Zugriffssteuerung in Azure](../role-based-access-control/custom-roles.md).

## <a name="next-steps"></a>Nächste Schritte
- Informationen zum Zuweisen einer integrierten RBAC-Rolle zu einem Sicherheitsprinzipal finden Sie unter [Authentifizieren des Zugriffs auf Event Hubs-Ressourcen mithilfe von Azure Active Directory](authenticate-application.md).
- Informationen zum [Erstellen benutzerdefinierter Rollen mit RBAC](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/Rbac/CustomRole).
- Informationen zum [Verwenden von Azure Active Directory mit EH](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/Rbac/AzureEventHubsSDK)

Informationen finden Sie in den folgenden verwandten Artikeln:

- [Authentifizieren von Anforderungen an Event Hubs über eine Anwendung mithilfe von Azure Active Directory](authenticate-application.md)
- [Authentifizieren einer verwalteten Identität mit Azure Active Directory für den Zugriff auf Event Hubs-Ressourcen](authenticate-managed-identity.md)
- [Authentifizieren von Anforderungen an Azure Event Hubs mithilfe von Shared Access Signature](authenticate-shared-access-signature.md)
- [Autorisieren des Zugriffs auf Event Hubs-Ressourcen mit Shared Access Signature](authorize-access-shared-access-signature.md)