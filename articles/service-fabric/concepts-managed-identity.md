---
title: 'Service Fabric: Übersicht über die verwaltete Identität | Microsoft-Dokumentation'
description: Dieser Artikel bietet eine Übersicht über die verwaltete Identität.
services: service-fabric
author: athinanthny
ms.service: service-fabric
ms.topic: conceptual
ms.date: 07/25/2019
ms.author: atsenthi
ms.openlocfilehash: e2f05ba28109a9b3c88d80eb218bf94014f0f082
ms.sourcegitcommit: 3486e2d4eb02d06475f26fbdc321e8f5090a7fac
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2019
ms.locfileid: "73243025"
---
# <a name="managed-identity-for-service-fabric-application-preview"></a>Verwaltete Identität für die Service Fabric-Anwendung (Vorschau)

Eine gängige Herausforderung beim Erstellen von Cloudanwendungen ist die Verwaltung der Anmeldeinformationen im Code, die für die Authentifizierung bei Clouddiensten erforderlich sind. Der Schutz von Anmeldeinformationen ist wichtig, da sie nie auf Entwicklerarbeitsstationen angezeigt und nicht in die Quellcodeverwaltung eingecheckt werden. Die Funktion „Verwaltete Identität“ für Azure-Ressourcen in Azure Active Directory (Azure AD) löst dieses Problem. Dieses Feature stellt für Azure-Dienste eine automatisch verwaltete Identität in Azure AD bereit. Sie können diese Identität für die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung unterstützt, einschließlich Key Vault. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein.

Die Funktion „Verwaltete Identität“ für Azure-Ressourcen ist in Azure AD mit einem Azure-Abonnement kostenlos. Es fallen keine zusätzlichen Kosten an.

> [!NOTE]
> „Verwaltete Identität für Azure-Ressourcen“ ist der neue Name des Diensts, der früher als „Verwaltete Dienstidentität“ (Managed Service Identity, MSI) bezeichnet wurde.

## <a name="terminology"></a>Begriff

Die folgenden Begriffe werden in der gesamten Dokumentation für die verwaltete Identität für Azure-Ressourcen verwendet:

- **Client-ID:** ein eindeutiger, in Azure AD generierter Bezeichner, der während der ersten Bereitstellung an eine Anwendung und einen Dienstprinzipal gebunden wird (siehe auch [Anwendungs-ID](/azure/active-directory/develop/developer-glossary#application-id-client-id))

- **Prinzipal-ID:** die Objekt-ID des Dienstprinzipalobjekts für die verwaltete Identität, mit der der rollenbasierte Zugriff auf eine Azure-Ressource gewährt wird

- **Dienstprinzipal:** ein Azure Active Directory-Objekt, das die Projektion einer AAD-Anwendung in einem bestimmten Mandanten darstellt (siehe auch [Dienstprinzipalobjekt](../active-directory/develop/developer-glossary.md#service-principal-object))


## <a name="about-managed-identities-in-azure"></a>Informationen zu verwalteten Identitäten in Azure

- [Typen der verwalteten Identität (MI) in Azure](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-work)

- [Funktionsweise einer vom System zugewiesenen verwalteten Identität in Azure](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview#how-a-system-assigned-managed-identity-works-with-an-azure-vm)

- [Funktionsweise einer vom Benutzer zugewiesenen verwalteten Identität in Azure](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview#how-a-user-assigned-managed-identity-works-with-an-azure-vm)


## <a name="supported-scenarios-for-service-fabric-applications"></a>Unterstützte Szenarien für Service Fabric-Anwendungen

Verwaltete Identitäten für Service Fabric werden nur in Service Fabric-Clustern unterstützt, die in Azure bereitgestellt werden, und auch nur für Anwendungen, die als Azure-Ressourcen bereitgestellt werden. Einer Anwendung, die nicht als Azure-Ressourcen bereitgestellt wird, kann keine Identität zugewiesen werden. Konzeptionell besteht die Unterstützung für verwaltete Identitäten in Azure Service Fabric-Clustern aus zwei Phasen:

1. Zuweisen mindestens einer verwalteten Identität zu der Anwendungsressource. Einer Anwendung können jeweils eine einzelne vom System zugewiesene Identität und/oder bis zu 32 vom Benutzer zugewiesene Identitäten zugewiesen werden.

2. Zuordnen einer der Identitäten, die der Anwendung zugewiesen sind, zu einem einzelnen Dienst, der die Anwendung enthält, in der Anwendungsdefinition

Die vom System zugewiesene Identität einer Anwendung ist eindeutig für diese Anwendung. Eine vom Benutzer zugewiesene Identität ist eine eigenständige Ressource, die mehreren Anwendungen zugewiesen werden kann. Innerhalb einer Anwendung kann eine einzelne (vom System oder vom Benutzer zugewiesene) Identität mehreren Diensten der Anwendung zugewiesen werden, jedem einzelnen Dienst kann jedoch nur eine einzige Identität zugewiesen werden. Schließlich muss einem Dienst explizit eine Identität zugewiesen werden, um auf diese Funktion zugreifen zu können. Tatsächlich ermöglicht die Zuordnung der Identitäten einer Anwendung zu den zugehörigen einzelnen Diensten eine Isolation in der Anwendung. In einem Dienst kann nur die ihm zugewiesene Identität verwendet werden (und überhaupt keine Identität, wenn dem Dienst keine Identität explizit zugewiesen wurde).  

Folgende Szenarien werden für die Vorschauversion unterstützt:

   - Bereitstellen einer neuen Anwendung mit einem oder mehreren Diensten und einer oder mehreren zugewiesenen Identitäten

   - Zuweisen einer oder mehrerer verwalteten Identitäten zu einer vorhandenen Anwendung, um auf Azure-Ressourcen zuzugreifen. Dabei muss die Anwendung selbst als Azure-Ressource bereitgestellt werden.


Die folgenden Szenarien werden nicht unterstützt oder nicht empfohlen. Beachten Sie, dass diese Aktionen möglicherweise nicht blockiert werden, aber zu Ausfällen in Ihren Anwendungen führen können:

   - Entfernen oder Ändern der Identitäten, die einer Anwendung zugewiesen sind. Wenn Sie Änderungen vornehmen möchten, übermitteln Sie separate Bereitstellungen, um zunächst die Zuweisung einer neuen Identität hinzuzufügen und dann eine zuvor zugewiesene Identität zu entfernen. Das Entfernen einer Identität aus einer vorhandenen Anwendung kann unerwünschte Auswirkungen haben und z. B. die Anwendung in einen nicht aktualisierbaren Zustand bringen. Die Anwendung kann vollständig gelöscht werden, wenn das Entfernen einer Identität erforderlich ist. Beachten Sie, dass dadurch die der Anwendung zugeordnete vom System zugewiesene Identität gelöscht wird (sofern definiert) und alle Zuordnungen mit den der Anwendung vom Benutzer zugewiesenen Identitäten entfernt werden.

   - Die SF-Unterstützung für verwaltete Identitäten ist gegenwärtig nicht in [AzureServiceTokenProvider](../key-vault/service-to-service-authentication.md) integriert. Die Integration wird bis zum Ende der Vorschauphase für das Feature „Verwaltete Identität“ implementiert.

>
> [!NOTE]
>
> Diese Funktion befindet sich in der Vorschauphase. Daher kann sie häufigen Änderungen unterliegen und ist möglicherweise nicht für Produktionsbereitstellungen geeignet.

## <a name="next-steps"></a>Nächste Schritte
* [Bereitstellen eines neuen Azure Service Fabric-Clusters mit Unterstützung für verwaltete Identitäten](./configure-new-azure-service-fabric-enable-managed-identity.md) 
* [Aktivieren der Unterstützung für verwaltete Identitäten in einem vorhandenen Azure Service Fabric-Cluster](./configure-existing-cluster-enable-managed-identity-token-service.md)
* [Bereitstellen einer Azure Service Fabric-Anwendung mit einer systemseitig zugewiesenen verwalteten Identität](./how-to-deploy-service-fabric-application-system-assigned-managed-identity.md)
* [Bereitstellen einer Azure Service Fabric-Anwendung mit einer benutzerseitig zugewiesenen verwalteten Identität](./how-to-deploy-service-fabric-application-user-assigned-managed-identity.md)
* [Nutzen der verwalteten Identität einer Service Fabric-Anwendung aus dem Dienstcode](./how-to-managed-identity-service-fabric-app-code.md)
* [Gewähren des Zugriffs auf andere Azure-Ressourcen für eine Azure Service Fabric-Anwendung](./how-to-grant-access-other-resources.md)
* [Deklarieren und Verwenden von Anwendungsgeheimnissen als KeyVaultReferences](./service-fabric-keyvault-references.md) 
