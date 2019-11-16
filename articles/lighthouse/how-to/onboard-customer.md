---
title: Onboarding eines Kunden für delegierte Azure-Ressourcenverwaltung durchführen
description: Erfahren Sie, wie Sie einen Kunden für delegierte Azure-Ressourcenverwaltung integrieren, sodass Sie von Ihrem eigenen Mandanten aus auf dessen Ressourcen zugreifen und sie verwalten können.
ms.date: 11/7/2019
ms.topic: overview
ms.openlocfilehash: 61b639a65d42d64a828db896b7c815c3d8f84734
ms.sourcegitcommit: 2d3740e2670ff193f3e031c1e22dcd9e072d3ad9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2019
ms.locfileid: "74131254"
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

Als Dienstanbieter können Sie mehrere Aufgaben für einen einzelnen Kunden ausführen, die für unterschiedliche Bereiche einen anderen Zugriff erfordern. Sie können so viele Autorisierungen definieren, wie Sie benötigen, um Benutzern in Ihrem Mandanten [integrierte Rollen der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC)](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) zuzuweisen.

Um die Verwaltung zu vereinfachen, empfiehlt es sich, für jede Rolle Azure AD-Benutzergruppen zu verwenden, was es Ihnen erlaubt, der Gruppe einzelne Benutzer hinzuzufügen oder daraus zu entfernen, anstatt diesem Benutzer Berechtigungen direkt zuzuweisen. Sie können außerdem einem Dienstprinzipal Rollen zuweisen. Achten Sie darauf, das Prinzip der geringsten Rechte zu befolgen, damit Benutzer nur über die Berechtigungen verfügen, die zum Durchführen ihrer Aufgaben erforderlich sind. Empfehlungen und Informationen zu unterstützten Rollen finden Sie unter [Mandanten, Benutzer und Rollen in Azure Lighthouse-Szenarien](../concepts/tenants-users-roles.md).

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
> [!TIP]
> Beim Onboarding eines Kunden empfiehlt es sich, die [Rolle „Registrierungszuweisung für verwaltete Dienste“](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#managed-services-registration-assignment-delete-role) zuzuweisen, damit Benutzer in Ihrem Mandanten später bei Bedarf [den Zugriff auf die Delegierung entfernen](#remove-access-to-a-delegation) können. Wenn diese Rolle nicht zugewiesen wird, können delegierte Ressourcen nur durch einen Benutzer im Kundenmandanten entfernt werden.

## <a name="create-an-azure-resource-manager-template"></a>Erstellen einer Azure Resource Manager-Vorlage

Um Ihren Kunden zu integrieren, müssen Sie eine [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/)-Vorlage für das Angebot erstellen, die folgende Informationen enthält: Die Werte von **mspOfferName** und **mspOfferDescription** sind für den Kunden sichtbar, wenn Angebotsdetails auf der Seite [Dienstanbieter](view-manage-service-providers.md) des Azure-Portals angezeigt werden.

|Feld  |Definition  |
|---------|---------|
|**mspOfferName**     |Ein Name, der diese Definition beschreibt. Dieser Wert wird für den Kunden als Titel des Angebots angezeigt.         |
|**mspOfferDescription**     |Eine kurze Beschreibung Ihres Angebots (z. B. „Angebot für die Verwaltung von virtuellen Contoso-Computern“)      |
|**managedByTenantId**     |Ihre Mandanten-ID         |
|**authorizations**     |Die **principalId**-Werte für die Benutzer/Gruppen/SPNs Ihres Mandanten, jeweils mit einem **principalIdDisplayName**, um Ihrem Kunden zu helfen, den Zweck der Autorisierung zu verstehen, und einem integrierten **roleDefinitionId**-Wert zugeordnet, um die Zugriffsebene anzugeben.         |

Verwenden Sie für das Onboarding des Abonnements eines Kunden die entsprechende Azure Resource Manager-Vorlage, die wir in unserem [Beispielrepository](https://github.com/Azure/Azure-Lighthouse-samples/) bereitstellen, zusammen mit einer zugehörigen Parameterdatei, die Sie so ändern, dass Sie Ihrer Konfiguration entspricht und Ihre Autorisierungen definiert. Je nachdem, ob Sie ein gesamtes Abonnement, eine Ressourcengruppe oder mehrere Ressourcengruppen innerhalb eines Abonnements integrieren, werden separate Vorlagen bereitgestellt. Wir stellen auch eine Vorlage bereit, die für Kunden verwendet werden kann, die ein Angebot für verwaltete Dienste gekauft haben, das Sie im Azure Marketplace veröffentlicht haben, wenn Sie deren Abonnement(s) lieber auf diese Weise integrieren möchten.

|So führen Sie das Onboarding durch  |Verwenden Sie diese Azure Resource Manager-Vorlage:  |Ändern Sie außerdem diese Parameterdatei: |
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
Die letzte Autorisierung im obigen Beispiel fügt eine **prinzipalId** mit der Rolle „Benutzerzugriffsadministrator“ hinzu (18d7d88d-d35e-4b5-a5c3-7773c20a72d9). Wenn Sie diese Rolle zuweisen, müssen Sie die **delegatedRoleDefinitionIds**-Eigenschaft und mindestens eine integrierte Rolle einschließen. Der in dieser Autorisierung erstellte Benutzer kann diese integrierten Rollen [verwalteten Identitäten](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) zuweisen. Dies ist erforderlich, um [Richtlinien bereitzustellen, die gewartet werden können](deploy-policy-remediation.md). Für diesen Benutzer gelten keine anderen Berechtigungen, die normalerweise der Rolle „Benutzerzugriffsadministrator“ zugeordnet sind.

## <a name="deploy-the-azure-resource-manager-templates"></a>Bereitstellen der Azure Resource Manager-Vorlagen

Nachdem Sie Ihre Parameterdatei aktualisiert haben, muss der Kunde die Azure Resource Manager-Vorlage im Mandanten seines Kunden auf Abonnementebene bereitstellen. Eine gesonderte Bereitstellung ist für jedes Abonnement erforderlich, das Sie in die delegierte Azure-Ressourcenverwaltung integrieren möchten (oder für jedes Abonnement, das Ressourcengruppen enthält, die Sie integrieren möchten).

Da es sich hierbei um eine Bereitstellung auf Abonnementebene handelt, kann sie nicht im Azure-Portal initiiert werden. Die Bereitstellung kann mithilfe von PowerShell oder der Azure-Befehlszeilenschnittstelle erfolgen, wie unten gezeigt.

> [!IMPORTANT]
> Die Bereitstellung muss von einem Nicht-Gastkonto im Mandanten des Kunden durchgeführt werden, das über die [integrierte Rolle „Besitzer“](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#owner) für das Abonnement verfügt, das integriert wird (oder das die Ressourcengruppen enthält, die integriert werden). Um alle Benutzer anzuzeigen, die das Abonnement delegieren können, kann ein Benutzer im Mandanten des Kunden das Abonnement im Azure-Portal auswählen, **Zugriffssteuerung (IAM)** öffnen und [alle Benutzer mit der Rolle „Besitzer“ anzeigen](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal#view-roles-and-permissions).

### <a name="powershell"></a>PowerShell

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

> [!IMPORTANT]
> Um das delegierte Abonnement in [Meine Kunden](view-manage-customers.md) anzuzeigen, muss den Benutzern im Mandanten des Dienstanbieters beim Onboarding des Abonnements für die delegierte Azure-Ressourcenverwaltung die Rolle [Leser](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#reader) (oder eine andere integrierte Rolle mit Lesezugriff) erteilt worden sein.

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

## <a name="remove-access-to-a-delegation"></a>Entfernen des Zugriffs auf eine Delegierung

Standardmäßig kann ein Benutzer im Mandanten des Kunden, der über die entsprechenden Berechtigungen verfügt, den Zugriff auf Ressourcen entfernen, die auf der Seite [Dienstanbieter](view-manage-service-providers.md#add-or-remove-service-provider-offers) des Azure-Portals an einen Dienstanbieter delegiert wurden.

Wenn Sie beim Onboarding des Kunden für die delegierte Azure-Ressourcenverwaltung Benutzer mit der Rolle [Löschen von Zuweisungen für Registrierungen bei verwalteten Diensten](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#managed-services-registration-assignment-delete-role) eingeschlossen haben, können diese Benutzer in Ihrem Mandanten auch die Delegierung entfernen. In diesem Fall können keine Benutzer im Mandanten des Dienstanbieters auf die zuvor delegierten Ressourcen zugreifen.

Im Beispiel unten wird eine Zuweisung gezeigt, die die Rolle **Löschen von Zuweisungen für Registrierungen bei verwalteten Diensten** erteilt. Diese kann in einer Parameterdatei enthalten sein:

```json
    "authorizations": [ 
        { 
            "principalId": "cfa7496e-a619-4a14-a740-85c5ad2063bb", 
            "principalIdDisplayName": "MSP Operators", 
            "roleDefinitionId": "91c1777a-f3dc-4fae-b103-61d183457e46" 
        } 
    ] 
```

Ein Benutzer mit dieser Berechtigung kann eine Delegierung auf eine der folgenden Arten entfernen.

### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
# Log in first with Connect-AzAccount if you're not using Cloud Shell

# Sign in as a user from the managing tenant directory 

Login-AzAccount

# Select the subscription that is delegated - or contains the delegated resource group(s)

Select-AzSubscription -SubscriptionName "<subscriptionName>"

# Get the registration assignment

Get-AzManagedServicesAssignment -Scope "/subscriptions/{delegatedSubscriptionId}"

# Delete the registration assignment

Remove-AzManagedServicesAssignment -ResourceId "/subscriptions/{delegatedSubscriptionId}/providers/Microsoft.ManagedServices/registrationAssignments/{assignmentGuid}"
```

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

```azurecli-interactive
# Log in first with az login if you're not using Cloud Shell

# Sign in as a user from the managing tenant directory

az login

# Select the subscription that is delegated – or contains the delegated resource group(s)

az account set -s <subscriptionId/name>

# List registration assignments

az managedservices assignment list

# Delete the registration assignment

az managedservices assignment delete –assignment <id or full resourceId>
```

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie über [Mandantenübergreifende Verwaltungsmöglichkeiten](../concepts/cross-tenant-management-experience.md).
- [Anzeigen und Verwalten von Kunden](view-manage-customers.md), indem sie im Azure-Portal zu **Meine Kunden** navigieren.
