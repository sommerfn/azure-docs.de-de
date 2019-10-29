---
title: Onboarding eines Kunden für delegierte Azure-Ressourcenverwaltung durchführen
description: Erfahren Sie, wie Sie einen Kunden für delegierte Azure-Ressourcenverwaltung integrieren, sodass Sie von Ihrem eigenen Mandanten aus auf dessen Ressourcen zugreifen und sie verwalten können.
author: JnHs
ms.author: jenhayes
ms.service: lighthouse
ms.date: 10/17/2019
ms.topic: overview
manager: carmonm
ms.openlocfilehash: 882afb83aa2a9bad9633df43b29e00b43162bf87
ms.sourcegitcommit: b4f201a633775fee96c7e13e176946f6e0e5dd85
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2019
ms.locfileid: "72595657"
---
# <a name="onboard-a-customer-to-azure-delegated-resource-management"></a>Onboarding eines Kunden für delegierte Azure-Ressourcenverwaltung durchführen

In diesem Artikel wird erläutert, wie Sie als Dienstanbieter einen Kunden in die delegierte Azure-Ressourcenverwaltung integrieren können, sodass Sie über Ihren eigenen Azure Active Directory-Mandanten (Azure AD) auf deren delegierte Ressourcen (Abonnements und/oder Ressourcengruppen) zugreifen und sie verwalten können. Zwar beziehen wir uns hier auf Dienstanbieter und Kunden, doch können Unternehmen, die mehrere Mandanten verwalten, denselben Prozess verwenden, um ihre Verwaltungserfahrung zu konsolidieren.

Sie können diesen Vorgang wiederholen, wenn Sie Ressourcen für mehrere Kunden verwalten. Wenn sich dann ein autorisierter Benutzer bei Ihrem Mandanten anmeldet, kann dieser Benutzer über Kundenmandantenbereiche hinweg autorisiert werden, Verwaltungsvorgänge durchzuführen, ohne sich bei jedem einzelnen Kundenmandanten anmelden zu müssen.

Sie können Ihre MPN-ID (Microsoft Partner Network) mit ihren integrierten Abonnements verknüpfen, um ihre Wirksamkeit hinsichtlich der Kundenbindung zu verfolgen und Anerkennung zu erhalten. Weitere Informationen finden Sie unter [Verknüpfen einer Partner-ID mit Ihren Azure-Konten](https://docs.microsoft.com/azure/billing/billing-partner-admin-link-started). Hinweis: Diese Zuordnung müssen Sie in Ihrem Dienstanbietermandanten ausführen.

> [!NOTE]
> Kunden können auch integriert werden, wenn sie ein Angebot für verwaltete Dienste (öffentlich oder privat) kaufen, das Sie in Azure Marketplace veröffentlicht haben. Weitere Informationen finden Sie unter [Veröffentlichen von Angeboten für verwaltete Dienste im Azure Marketplace](publish-managed-services-offers.md). Sie können den hier beschriebenen Onboardingprozess auch mit einem in Azure Marketplace veröffentlichten Angebot verwenden.

Der Onboardingprozess erfordert, dass Aktionen sowohl innerhalb des Mandanten des Dienstanbieters als auch vom Mandanten des Kunden aus durchgeführt werden. Alle diese Schritte werden in diesem Artikel beschrieben.

> [!IMPORTANT]
> Derzeit ist es nicht möglich, ein Abonnement (oder eine Ressourcengruppe innerhalb eines Abonnements) für die delegierte Azure-Ressourcenverwaltung zu integrieren, wenn das Abonnement Azure Databricks verwendet. Ähnlich können Sie, wenn ein Abonnement für das Onboarding mit dem **Microsoft.ManagedServices**-Ressourcenanbieter registriert wurde, zu diesem Zeitpunkt auch keinen Databricks-Arbeitsbereich für dieses Abonnement erstellen.

## <a name="gather-tenant-and-subscription-details"></a>Sammeln von Mandanten- und Abonnementinformationen

Für das Onboarding des Mandanten eines Kunden muss dieser ein aktives Azure-Abonnement besitzen. Sie müssen Folgendes wissen:

- Die Mandanten-ID des Mandanten des Dienstanbieters (wo Sie die Ressourcen des Kunden verwalten werden)
- Die Mandanten-ID des Mandanten des Kunden (der über Ressourcen verfügt, die vom Dienstanbieter verwaltet werden)
- Die Abonnement-IDs für jedes einzelne Abonnement im Mandanten des Kunden, das vom Dienstanbieter verwaltet wird (oder der die Ressourcengruppen enthält, die vom Dienstanbieter verwaltet werden).

Wenn Sie diese Informationen nicht schon besitzen, können Sie sie mit einer der folgenden Methoden abrufen.

### <a name="azure-portal"></a>Azure-Portal

Ihre Mandanten-ID können Sie anzeigen, indem Sie rechts oben im Azure-Portal auf Ihren Kontonamen zeigen, oder indem Sie **Verzeichnis wechseln** auswählen. Um Ihre Mandanten-ID auszuwählen und zu kopieren, suchen Sie im Portal nach „Azure Active Directory“, wählen Sie dann **Eigenschaften** aus, und kopieren Sie den im Feld **Verzeichnis-ID** angezeigten Wert. Um die ID eines Abonnements zu ermitteln, suchen Sie nach „Abonnements“, und wählen Sie dann die entsprechende Abonnement-ID aus.

### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
# Log in first with Connect-AzAccount if you're not using Cloud Shell

Select-AzSubscription <subscriptionId>
```

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

```azurecli-interactive
# Log in first with az login if you're not using Cloud Shell

az account set --subscription <subscriptionId/name>
az account show
```

> [!NOTE]
> Wenn Sie ein Abonnement (oder mindestens eine Ressourcengruppe innerhalb eines Abonnements) mithilfe des hier beschriebenen Prozesses integrieren, wird der Ressourcenanbieter **Microsoft.ManagedServices** für dieses Abonnement registriert.

## <a name="define-roles-and-permissions"></a>Definieren von Rolle und Berechtigungen

Als Dienstanbieter können Sie mehrere Angebote für einen einzelnen Kunden verwenden, die für unterschiedliche Bereiche einen anderen Zugriff benötigen.

Um die Verwaltung zu vereinfachen, empfiehlt es sich, für jede Rolle Azure AD-Benutzergruppen zu verwenden, was es Ihnen erlaubt, der Gruppe einzelne Benutzer hinzuzufügen oder daraus zu entfernen, anstatt diesem Benutzer Berechtigungen direkt zuzuweisen. Sie können außerdem einem Dienstprinzipal Rollen zuweisen. Beachten Sie, dass Sie das Prinzip der geringsten Rechte befolgen, damit Benutzer nur über die Berechtigungen verfügen, die zum Durchführen Ihrer Aufgaben erforderlich sind, um die Wahrscheinlichkeit von unbeabsichtigten Fehlern zu verringern. Weitere Informationen finden Sie unter [Empfohlene Sicherheitsmaßnahmen](../concepts/recommended-security-practices.md).

> [!NOTE]
> Rollenzuweisungen müssen [integrierte Rollen](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) für die rollenbasierte Zugriffssteuerung (RBAC) verwenden. Alle integrierten Rollen werden derzeit mit der delegierten Azure-Ressourcenverwaltung unterstützt, mit Ausnahme von „Besitzer“ und allen integrierten Rollen mit der [DataActions](https://docs.microsoft.com/azure/role-based-access-control/role-definitions#dataactions)-Berechtigung. Die integrierte Rolle „Benutzerzugriffsadministrator“ wird für die eingeschränkte Verwendung, wie unten beschrieben, unterstützt. Benutzerdefinierte Rollen und [klassische Abonnementadministratorrollen](https://docs.microsoft.com/azure/role-based-access-control/classic-administrators) werden ebenfalls nicht unterstützt.

Um Autorisierungen zu definieren, müssen Sie die ID-Werte für jeden Benutzer, jede Benutzergruppe oder jeden Dienstprinzipal kennen, dem/der Sie Zugriff gewähren möchten. Außerdem benötigen Sie die Rollendefinitions-ID für jede integrierten Rolle, die Sie zuweisen möchten. Wenn Sie diese Informationen nicht schon besitzen, können Sie sie mit einer der folgenden Methoden abrufen.

### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
# Log in first with Connect-AzAccount if you're not using Cloud Shell

# To retrieve the objectId for an Azure AD group
(Get-AzADGroup -DisplayName '<yourGroupName>').id

# To retrieve the objectId for an Azure AD user
(Get-AzADUser -UserPrincipalName '<yourUPN>').id

# To retrieve the objectId for an SPN
(Get-AzADApplication -DisplayName '<appDisplayName>').objectId

# To retrieve role definition IDs
(Get-AzRoleDefinition -Name '<roleName>').id
```

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

```azurecli-interactive
# Log in first with az login if you're not using Cloud Shell

# To retrieve the objectId for an Azure AD group
az ad group list --query "[?displayName == '<yourGroupName>'].objectId" --output tsv

# To retrieve the objectId for an Azure AD user
az ad user show --upn-or-object-id "<yourUPN>" –-query "objectId" --output tsv

# To retrieve the objectId for an SPN
az ad sp list --query "[?displayName == '<spDisplayName>'].objectId" --output tsv

# To retrieve role definition IDs
az role definition list --name "<roleName>" | grep name
```

## <a name="create-an-azure-resource-manager-template"></a>Erstellen einer Azure Resource Manager-Vorlage

Um Ihren Kunden zu integrieren, müssen Sie eine [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/)-Vorlage für das Angebot erstellen, die folgende Informationen enthält: Die Werte von **mspOfferName** und **mspOfferDescription** sind für den Kunden sichtbar, wenn Angebotsdetails auf der Seite [Dienstanbieter](view-manage-service-providers.md) des Azure-Portals angezeigt werden.

|Feld  |Definition  |
|---------|---------|
|**mspOfferName**     |Ein Name, der diese Definition beschreibt. Dieser Wert wird für den Kunden als Titel des Angebots angezeigt.         |
|**mspOfferDescription**     |Eine kurze Beschreibung Ihres Angebots (z. B. „Angebot für die Verwaltung von virtuellen Contoso-Computern“)      |
|**managedByTenantId**     |Ihre Mandanten-ID         |
|**authorizations**     |Die **principalId**-Werte für die Benutzer/Gruppen/SPNs Ihres Mandanten, jeweils mit einem **principalIdDisplayName**, um Ihrem Kunden zu helfen, den Zweck der Autorisierung zu verstehen, und einem integrierten **roleDefinitionId**-Wert zugeordnet, um die Zugriffsebene anzugeben.         |

Verwenden Sie für das Onboarding des Abonnements eines Kunden die entsprechende Azure Resource Manager-Vorlage, die wir in unserem [Beispielrepository](https://github.com/Azure/Azure-Lighthouse-samples/) bereitstellen, zusammen mit einer zugehörigen Parameterdatei, die Sie so ändern, dass Sie Ihrer Konfiguration entspricht und Ihre Autorisierungen definiert. Je nachdem, ob Sie ein gesamtes Abonnement, eine Ressourcengruppe oder mehrere Ressourcengruppen innerhalb eines Abonnements integrieren, werden separate Vorlagen bereitgestellt. Wir stellen auch eine Vorlage bereit, die für Kunden verwendet werden kann, die ein Angebot für verwaltete Dienste gekauft haben, das Sie im Azure Marketplace veröffentlicht haben, wenn Sie deren Abonnement(s) lieber auf diese Weise integrieren möchten.

|**So führen Sie das Onboarding durch**  |**Verwenden Sie diese Azure Resource Manager-Vorlage**  |**Und ändern Sie diese Parameterdatei** |
|---------|---------|---------|
|Subscription   |[delegatedResourceManagement.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/Azure-Delegated-Resource-Management/templates/delegated-resource-management/delegatedResourceManagement.json)  |[delegatedResourceManagement.parameters.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/Azure-Delegated-Resource-Management/templates/delegated-resource-management/delegatedResourceManagement.parameters.json)    |
|Resource group   |[rgDelegatedResourceManagement.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/Azure-Delegated-Resource-Management/templates/rg-delegated-resource-management/rgDelegatedResourceManagement.json)  |[rgDelegatedResourceManagement.parameters.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/Azure-Delegated-Resource-Management/templates/rg-delegated-resource-management/rgDelegatedResourceManagement.parameters.json)    |
|Mehrere Ressourcengruppen in einem Abonnement   |[multipleRgDelegatedResourceManagement.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/Azure-Delegated-Resource-Management/templates/rg-delegated-resource-management/multipleRgDelegatedResourceManagement.json)  |[multipleRgDelegatedResourceManagement.parameters.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/Azure-Delegated-Resource-Management/templates/rg-delegated-resource-management/multipleRgDelegatedResourceManagement.parameters.json)    |
|Abonnement (bei Verwendung eines in Azure Marketplace veröffentlichten Angebots)   |[marketplaceDelegatedResourceManagement.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/Azure-Delegated-Resource-Management/templates/marketplace-delegated-resource-management/marketplaceDelegatedResourceManagement.json)  |[marketplaceDelegatedResourceManagement.parameters.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/Azure-Delegated-Resource-Management/templates/marketplace-delegated-resource-management/marketplaceDelegatedResourceManagement.parameters.json)    |

> [!IMPORTANT]
> Der hier beschriebene Prozess erfordert für jedes Abonnement, das integriert wird, eine separate Bereitstellung. Gesonderte Bereitstellungen sind auch erforderlich, wenn Sie mehrere Ressourcengruppen innerhalb verschiedener Abonnements integrieren. Das Onboarding mehrerer Ressourcengruppen innerhalb eines einzelnen Abonnements kann jedoch in einer einzigen Bereitstellung erfolgen.
>
> Separate Bereitstellungen sind auch erforderlich für mehrere Angebote, die auf dasselbe Abonnement (oder dieselben Ressourcengruppen in einem Abonnement) angewendet werden. Für jedes angewendete Angebot muss ein anderer **mspOfferName** verwendet werden.

Das folgende Beispiel zeigt die Datei **delegatedResourceManagement.parameters.json**, die für das Onboarding eines Abonnements verwendet wird. Die Ressourcengruppen-Parameterdateien (im Ordner [rg-delegated-resource-management](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/rg-delegated-resource-management)) sind ähnlich, enthalten aber auch einen **rgName**-Parameter, um die spezifische(n) Ressourcengruppe(n) zu identifizieren, die integriert werden soll(en).

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "mspOfferName": {
            "value": "Fabrikam Managed Services - Interstellar"
        },
        "mspOfferDescription": {
            "value": "Fabrikam Managed Services - Interstellar"
        },
        "managedByTenantId": {
            "value": "df4602a3-920c-435f-98c4-49ff031b9ef6"
        },
        "authorizations": {
            "value": [
                {
                    "principalId": "0019bcfb-6d35-48c1-a491-a701cf73b419",
                    "principalIdDisplayName": "Tier 1 Support",
                    "roleDefinitionId": "b24988ac-6180-42a0-ab88-20f7382dd24c"
                },
                {
                    "principalId": "0019bcfb-6d35-48c1-a491-a701cf73b419",
                    "principalIdDisplayName": "Tier 1 Support",
                    "roleDefinitionId": "36243c78-bf99-498c-9df9-86d9f8d28608"
                },
                {
                    "principalId": "0afd8497-7bff-4873-a7ff-b19a6b7b332c",
                    "principalIdDisplayName": "Tier 2 Support",
                    "roleDefinitionId": "acdd72a7-3385-48ef-bd42-f606fba81ae7"
                },
                {
                    "principalId": "9fe47fff-5655-4779-b726-2cf02b07c7c7",
                    "principalIdDisplayName": "Service Automation Account",
                    "roleDefinitionId": "b24988ac-6180-42a0-ab88-20f7382dd24c"
                },
                {
                    "principalId": "3kl47fff-5655-4779-b726-2cf02b05c7c4",
                    "principalIdDisplayName": "Policy Automation Account",
                    "roleDefinitionId": "18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
                    "delegatedRoleDefinitionIds": [
                        "b24988ac-6180-42a0-ab88-20f7382dd24c",
                        "92aaf0da-9dab-42b6-94a3-d43ce8d16293"
                    ]
                }
            ]
        }
    }
}
```
Die letzte Autorisierung im obigen Beispiel fügt eine **prinzipalId** mit der Rolle „Benutzerzugriffsadministrator“ hinzu (18d7d88d-d35e-4b5-a5c3-7773c20a72d9). Wenn Sie diese Rolle zuweisen, müssen Sie die **delegatedRoleDefinitionIds**-Eigenschaft und mindestens eine integrierte Rolle einschließen. Der in dieser Autorisierung erstellte Benutzer kann diese integrierten Rollen [verwalteten Identitäten](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) zuweisen. Beachten Sie, dass für diesen Benutzer keine anderen Berechtigungen gelten, die normalerweise der Rolle „Benutzerzugriffsadministrator“ zugeordnet sind.

## <a name="deploy-the-azure-resource-manager-templates"></a>Bereitstellen der Azure Resource Manager-Vorlagen

Nachdem Sie Ihre Parameterdatei aktualisiert haben, muss der Kunde die Resource Manager-Vorlage im Mandanten seines Kunden als Bereitstellung auf Abonnementebene bereitstellen. Eine gesonderte Bereitstellung ist für jedes Abonnement erforderlich, das Sie in die delegierte Azure-Ressourcenverwaltung integrieren möchten (oder für jedes Abonnement, das Ressourcengruppen enthält, die Sie integrieren möchten).

> [!IMPORTANT]
> Die Bereitstellung muss von einem Nicht-Gastkonto im Mandanten des Kunden durchgeführt werden, das über die [integrierte Rolle „Besitzer“](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#owner) für das Abonnement verfügt, das integriert wird (oder das die Ressourcengruppen enthält, die integriert werden). Um alle Benutzer anzuzeigen, die das Abonnement delegieren können, kann ein Benutzer im Mandanten des Kunden das Abonnement im Azure-Portal auswählen, **Zugriffssteuerung (IAM)** öffnen und [alle Benutzer mit der Rolle „Besitzer“ anzeigen](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal#view-roles-and-permissions).


```azurepowershell-interactive
# Log in first with Connect-AzAccount if you're not using Cloud Shell

# Deploy Azure Resource Manager template using template and parameter file locally
New-AzDeployment -Name <deploymentName> `
                 -Location <AzureRegion> `
                 -TemplateFile <pathToTemplateFile> `
                 -TemplateParameterFile <pathToParameterFile> `
                 -Verbose

# Deploy Azure Resource Manager template that is located externally
New-AzDeployment -Name <deploymentName> `
                 -Location <AzureRegion> `
                 -TemplateUri <templateUri> `
                 -TemplateParameterUri <parameterUri> `
                 -Verbose
```

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

```azurecli-interactive
# Log in first with az login if you're not using Cloud Shell

# Deploy Azure Resource Manager template using template and parameter file locally
az deployment create –-name <deploymentName> \
                     --location <AzureRegion> \
                     --template-file <pathToTemplateFile> \
                     --parameters <parameters/parameterFile> \
                     --verbose

# Deploy external Azure Resource Manager template, with local parameter file
az deployment create –-name <deploymentName \
                     –-location <AzureRegion> \
                     --template-uri <templateUri> \
                     --parameters <parameterFile> \
                     --verbose
```

## <a name="confirm-successful-onboarding"></a>Bestätigen des erfolgreichen Onboardings

Wenn ein Kundenabonnement erfolgreich in die delegierte Azure-Ressourcenverwaltung integriert wurde, können Benutzer im Mandanten des Dienstanbieters das Abonnement und seine Ressourcen sehen (wenn Ihnen mithilfe des oben geschilderten Vorgangs der Zugriff gewährt wurde, entweder einzeln oder als Mitglied einer Azure AD-Gruppe mit den entsprechenden Berechtigungen). Um dies zu bestätigen, überprüfen Sie auf eine der folgenden Arten, ob das Abonnement angezeigt wird.  

### <a name="azure-portal"></a>Azure-Portal

Im Mandanten des Dienstanbieters:

1. Navigieren Sie zur Seite [Meine Kunden](view-manage-customers.md).
2. Wählen Sie **Kunden** aus.
3. Vergewissern Sie sich, dass die Abonnements mit dem Angebotsnamen angezeigt werden, den Sie in der Resource Manager-Vorlage angegeben haben.

Im Mandanten des Kunden:

1. Navigieren Sie zur Seite [Dienstanbieter](view-manage-service-providers.md).
2. Wählen Sie **Dienstanbieterangebote** aus.
3. Vergewissern Sie sich, dass die Abonnements mit dem Angebotsnamen angezeigt werden, den Sie in der Resource Manager-Vorlage angegeben haben.

> [!NOTE]
> Nach dem Abschluss Ihrer Bereitstellung kann es noch einige Minuten dauern, bis die Aktualisierungen im Azure-Portal zu sehen sind.

### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
# Log in first with Connect-AzAccount if you're not using Cloud Shell

Get-AzContext
```

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

```azurecli-interactive
# Log in first with az login if you're not using Cloud Shell

az account list
```

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie über [Mandantenübergreifende Verwaltungsmöglichkeiten](../concepts/cross-tenant-management-experience.md).
- [Anzeigen und Verwalten von Kunden](view-manage-customers.md), indem sie im Azure-Portal zu **Meine Kunden** navigieren.
