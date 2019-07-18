---
title: Löschung des vollständigen Modus von Azure Resource Manager nach Ressourcentyp
description: Zeigt, wie die Ressourcentypen die Löschung des vollständigen Modus in Azure Resource Manager-Vorlagen verarbeiten.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 04/24/2019
ms.author: tomfitz
ms.openlocfilehash: 5ac442c0ae1e397fd1e8b58fdbcf61eb8712046c
ms.sourcegitcommit: af58483a9c574a10edc546f2737939a93af87b73
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2019
ms.locfileid: "68302838"
---
# <a name="deletion-of-azure-resources-for-complete-mode-deployments"></a>Löschen von Azure-Ressourcen für Bereitstellungen im vollständigen Modus
Dieser Artikel beschreibt, wie Ressourcentypen das Löschen handhaben, wenn sie sich nicht in einer Vorlage befinden, die im vollständigen Modus bereitgestellt wird.

Die mit `Yes` markierten Ressourcentypen werden gelöscht, wenn sich der Typ nicht in der Vorlage befindet, die im vollständigen Modus bereitgestellt wird. 

Die mit `No` markierten Ressourcentypen werden nicht automatisch gelöscht, wenn sie sich nicht in der Vorlage befinden. Sie werden jedoch gelöscht, wenn die übergeordnete Ressource gelöscht wird. Eine vollständige Beschreibung des Verhaltens finden Sie unter [Azure Resource Manager-Bereitstellungsmodi](deployment-modes.md).


## <a name="microsoftaad"></a>Microsoft.AAD

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | DomainServices | Ja | 
> | DomainServices/oucontainer | Nein | 

## <a name="microsoftaadiam"></a>microsoft.aadiam

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | diagnosticSettings | Nein | 
> | diagnosticSettingsCategories | Nein | 

## <a name="microsoftaddons"></a>Microsoft.Addons

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | supportProviders | Nein | 

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | aadsupportcases | Nein | 
> | addsservices | Nein | 
> | agents | Nein | 
> | anonymousapiusers | Nein | 
> | Konfiguration | Nein | 
> | logs | Nein | 
> | reports | Nein | 
> | services | Nein | 

## <a name="microsoftadvisor"></a>Microsoft.Advisor

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | Konfigurationen | Nein | 
> | generateRecommendations | Nein | 
> | empfehlungen | Nein | 
> | suppressions | Nein | 

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | actionRules | Nein | 
> | alerts | Nein | 
> | alertsList | Nein | 
> | alertsSummary | Nein | 
> | alertsSummaryList | Nein | 
> | smartDetectorAlertRules | Nein | 
> | smartDetectorRuntimeEnvironments | Nein | 
> | smartGroups | Nein | 

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | servers | Ja | 

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | reportFeedback | Nein | 
> | service | Ja | 
> | validateServiceName | Nein | 

## <a name="microsoftattestation"></a>Microsoft.Attestation

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | attestationProviders | Nein | 

## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | classicAdministrators | Nein | 
> | denyAssignments | Nein | 
> | elevateAccess | Nein | 
> | locks | Nein | 
> | Berechtigungen | Nein | 
> | policyAssignments | Nein | 
> | policyDefinitions | Nein | 
> | policySetDefinitions | Nein | 
> | providerOperations | Nein | 
> | roleAssignments | Nein | 
> | roleDefinitions | Nein | 

## <a name="microsoftautomation"></a>Microsoft.Automation

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | automationAccounts | Ja | 
> | automationAccounts/configurations | Ja | 
> | automationAccounts/jobs | Nein | 
> | automationAccounts/runbooks | Ja | 
> | automationAccounts/softwareUpdateConfigurations | Nein | 
> | automationAccounts/webhooks | Nein | 

## <a name="microsoftazuregeneva"></a>Microsoft.Azure.Geneva

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | environments | Nein | 
> | environments/accounts | Nein | 
> | environments/accounts/namespaces | Nein | 
> | environments/accounts/namespaces/configurations | Nein | 

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | b2cDirectories | Ja | 

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | registrations | Ja | 
> | registrations/customerSubscriptions | Nein | 
> | registrations/products | Nein | 

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | batchAccounts | Ja | 

## <a name="microsoftbilling"></a>Microsoft.Billing

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | billingAccounts | Nein | 
> | billingAccounts/billingProfiles | Nein | 
> | billingAccounts/billingProfiles/billingSubscriptions | Nein | 
> | billingAccounts/billingProfiles/invoices | Nein | 
> | billingAccounts/billingProfiles/invoices/pricesheet | Nein | 
> | billingAccounts/billingProfiles/operationStatus | Nein | 
> | billingAccounts/billingProfiles/paymentMethods | Nein | 
> | billingAccounts/billingProfiles/policies | Nein | 
> | billingAccounts/billingProfiles/pricesheet | Nein | 
> | billingAccounts/billingProfiles/products | Nein | 
> | billingAccounts/billingProfiles/transactions | Nein | 
> | billingAccounts/billingSubscriptions | Nein | 
> | billingAccounts/departments | Nein | 
> | billingAccounts/eligibleOffers | Nein | 
> | billingAccounts/enrollmentAccounts | Nein | 
> | billingAccounts/invoices | Nein | 
> | billingAccounts/invoiceSections | Nein | 
> | billingAccounts/invoiceSections/billingSubscriptions | Nein | 
> | billingAccounts/invoiceSections/billingSubscriptions/transfer | Nein | 
> | billingAccounts/invoiceSections/importRequests | Nein | 
> | billingAccounts/invoiceSections/initiateImportRequest | Nein | 
> | billingAccounts/invoiceSections/initiateTransfer | Nein | 
> | billingAccounts/invoiceSections/operationStatus | Nein | 
> | billingAccounts/invoiceSections/products | Nein | 
> | billingAccounts/invoiceSections/transfers | Nein | 
> | billingAccounts/products | Nein | 
> | billingAccounts/projects | Nein | 
> | billingAccounts/projects/billingSubscriptions | Nein | 
> | billingAccounts/projects/importRequests | Nein | 
> | billingAccounts/projects/initiateImportRequest | Nein | 
> | billingAccounts/projects/operationStatus | Nein | 
> | billingAccounts/projects/products | Nein | 
> | billingAccounts/transactions | Nein | 
> | billingPeriods | Nein | 
> | BillingPermissions | Nein | 
> | billingProperty | Nein | 
> | BillingRoleAssignments | Nein | 
> | BillingRoleDefinitions | Nein | 
> | CreateBillingRoleAssignment | Nein | 
> | departments | Nein | 
> | enrollmentAccounts | Nein | 
> | importRequests | Nein | 
> | importRequests/acceptImportRequest | Nein | 
> | importRequests/declineImportRequest | Nein | 
> | invoices | Nein | 
> | transfers | Nein | 
> | transfers/acceptTransfer | Nein | 
> | transfers/declineTransfer | Nein | 
> | transfers/operationStatus | Nein | 
> | usagePlans | Nein | 

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | mapApis | Ja | 
> | updateCommunicationPreference | Nein | 

## <a name="microsoftbiztalkservices"></a>Microsoft.BizTalkServices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | BizTalk | Ja | 

## <a name="microsoftblueprint"></a>Microsoft.Blueprint

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | blueprintAssignments | Nein | 
> | blueprintAssignments/assignmentOperations | Nein | 
> | blueprintAssignments/operations | Nein | 
> | blueprints | Nein | 
> | blueprints/artifacts | Nein | 
> | blueprints/versions | Nein | 
> | blueprints/versions/artifacts | Nein | 

## <a name="microsoftbotservice"></a>Microsoft.BotService

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | botServices | Ja | 
> | botServices/channels | Nein | 
> | botServices/connections | Nein | 

## <a name="microsoftcache"></a>Microsoft.Cache

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | Redis | Ja | 
> | RedisConfigDefinition | Nein | 

## <a name="microsoftcapacity"></a>Microsoft.Capacity

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | appliedReservations | Nein | 
> | calculatePrice | Nein | 
> | catalogs | Nein | 
> | commercialReservationOrders | Nein | 
> | reservationOrders | Nein | 
> | reservationOrders/calculateRefund | Nein | 
> | reservationOrders/merge | Nein | 
> | reservationOrders/reservations | Nein | 
> | reservationOrders/reservations/revisions | Nein | 
> | reservationOrders/return | Nein | 
> | reservationOrders/split | Nein | 
> | reservationOrders/swap | Nein | 
> | reservations | Nein | 
> | ressourcen | Nein | 
> | validateReservationOrder | Nein | 

## <a name="microsoftcdn"></a>Microsoft.Cdn

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | edgenodes | Nein | 
> | profiles | Ja | 
> | profiles/endpoints | Ja | 
> | profiles/endpoints/customdomains | Nein | 
> | profiles/endpoints/origins | Nein | 
> | validateProbe | Nein | 

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | certificateOrders | Ja | 
> | certificateOrders/certificates | Nein | 
> | validateCertificateRegistrationInformation | Nein | 

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | capabilities | Nein | 
> | domainNames | Nein | 
> | domainNames/capabilities | Nein | 
> | domainNames/internalLoadBalancers | Nein | 
> | domainNames/serviceCertificates | Nein | 
> | domainNames/slots | Nein | 
> | domainNames/slots/roles | Nein | 
> | moveSubscriptionResources | Nein | 
> | operatingSystemFamilies | Nein | 
> | operatingSystems | Nein | 
> | quotas | Nein | 
> | resourceTypes | Nein | 
> | validateSubscriptionMoveAvailability | Nein | 
> | virtualMachines | Nein | 
> | virtualMachines/diagnosticSettings | Nein | 

## <a name="microsoftclassicinfrastructuremigrate"></a>Microsoft.ClassicInfrastructureMigrate

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | classicInfrastructureResources | Nein | 

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | capabilities | Nein | 
> | expressRouteCrossConnections | Nein | 
> | expressRouteCrossConnections/peerings | Nein | 
> | gatewaySupportedDevices | Nein | 
> | networkSecurityGroups | Nein | 
> | quotas | Nein | 
> | reservedIps | Nein | 
> | virtualNetworks | Nein | 
> | virtualNetworks/remoteVirtualNetworkPeeringProxies | Nein | 
> | virtualNetworks/virtualNetworkPeerings | Nein | 

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | capabilities | Nein | 
> | disks | Nein | 
> | images | Nein | 
> | osImages | Nein | 
> | osPlatformImages | Nein | 
> | publicImages | Nein | 
> | quotas | Nein | 
> | storageAccounts | Nein | 
> | storageAccounts/services | Nein | 
> | storageAccounts/services/diagnosticSettings | Nein | 
> | storageAccounts/vmImages | Nein | 
> | vmImages | Nein | 

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | accounts | Ja | 

## <a name="microsoftcommerce"></a>Microsoft.Commerce

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | RateCard | Nein | 
> | UsageAggregates | Nein | 

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | availabilitySets | Ja | 
> | disks | Ja | 
> | images | Ja | 
> | restorePointCollections | Ja | 
> | restorePointCollections/restorePoints | Nein | 
> | sharedVMImages | Ja | 
> | sharedVMImages/versions | Ja | 
> | snapshots | Ja | 
> | virtualMachines | Ja | 
> | virtualMachines/diagnosticSettings | Nein | 
> | virtualMachines/extensions | Ja | 
> | virtualMachineScaleSets | Ja | 
> | virtualMachineScaleSets/extensions | Nein | 
> | virtualMachineScaleSets/networkInterfaces | Nein | 
> | virtualMachineScaleSets/publicIPAddresses | Nein | 
> | virtualMachineScaleSets/virtualMachines | Nein | 
> | virtualMachineScaleSets/virtualMachines/networkInterfaces | Nein | 

## <a name="microsoftconsumption"></a>Microsoft.Consumption

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | AggregatedCost | Nein | 
> | Bilanzen | Nein | 
> | Budgets | Nein | 
> | Charges | Nein | 
> | CostTags | Nein | 
> | credits | Nein | 
> | events | Nein | 
> | Vorhersagen | Nein | 
> | lots | Nein | 
> | Marketplaces | Nein | 
> | Pricesheets | Nein | 
> | products | Nein | 
> | ReservationDetails | Nein | 
> | ReservationRecommendations | Nein | 
> | ReservationSummaries | Nein | 
> | ReservationTransactions | Nein | 
> | `Tags` | Nein | 
> | Begriffe | Nein | 
> | UsageDetails | Nein | 

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | containerGroups | Ja | 
> | serviceAssociationLinks | Nein | 

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | registries | Ja | 
> | registries/builds | Nein | 
> | registries/builds/cancel | Nein | 
> | registries/builds/getLogLink | Nein | 
> | registries/buildTasks | Ja | 
> | registries/buildTasks/steps | Nein | 
> | registries/eventGridFilters | Nein | 
> | registries/getBuildSourceUploadUrl | Nein | 
> | registries/GetCredentials | Nein | 
> | registries/importImage | Nein | 
> | registries/queueBuild | Nein | 
> | registries/regenerateCredential | Nein | 
> | registries/regenerateCredentials | Nein | 
> | registries/replications | Ja | 
> | registries/runs | Nein | 
> | registries/runs/cancel | Nein | 
> | registries/scheduleRun | Nein | 
> | registries/tasks | Ja | 
> | registries/updatePolicies | Nein | 
> | registries/webhooks | Ja | 
> | registries/webhooks/getCallbackConfig | Nein | 
> | registries/webhooks/ping | Nein | 

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | containerServices | Ja | 
> | managedClusters | Ja | 

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | applications | Ja | 
> | updateCommunicationPreference | Nein | 

## <a name="microsoftcortanaanalytics"></a>Microsoft.CortanaAnalytics

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | accounts | Ja | 

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | Alerts | Nein | 
> | BillingAccounts | Nein | 
> | Connectors | Ja | 
> | Departments | Nein | 
> | Dimensionen | Nein | 
> | EnrollmentAccounts | Nein | 
> | Abfragen | Nein | 
> | Registrieren | Nein | 
> | Reportconfigs | Nein | 
> | Berichte | Nein | 

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | hubs | Ja | 
> | hubs/authorizationPolicies | Nein | 
> | hubs/connectors | Nein | 
> | hubs/connectors/mappings | Nein | 
> | hubs/interactions | Nein | 
> | hubs/kpi | Nein | 
> | hubs/links | Nein | 
> | hubs/profiles | Nein | 
> | hubs/roleAssignments | Nein | 
> | hubs/roles | Nein | 
> | hubs/suggestTypeSchema | Nein | 
> | hubs/views | Nein | 
> | hubs/widgetTypes | Nein | 

## <a name="microsoftdatabox"></a>Microsoft.DataBox

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | jobs | Ja | 

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | DataBoxEdgeDevices | Ja | 

## <a name="microsoftdatabricks"></a>Microsoft.Databricks

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | workspaces | Ja | 
> | workspaces/virtualNetworkPeerings | Nein | 

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | catalogs | Ja | 

## <a name="microsoftdataconnect"></a>Microsoft.DataConnect

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | connectionManagers | Ja | 

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | dataFactories | Ja | 
> | dataFactories/diagnosticSettings | Nein | 
> | dataFactorySchema | Nein | 
> | factories | Ja | 
> | factories/integrationRuntimes | Nein | 

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | accounts | Ja | 
> | accounts/dataLakeStoreAccounts | Nein | 
> | accounts/storageAccounts | Nein | 
> | accounts/storageAccounts/containers | Nein | 

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | accounts | Ja | 
> | accounts/eventGridFilters | Nein | 
> | accounts/firewallRules | Nein | 

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | services | Ja | 
> | services/projects | Ja | 

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | servers | Ja | 
> | servers/recoverableServers | Nein | 
> | servers/virtualNetworkRules | Nein | 

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | servers | Ja | 
> | servers/recoverableServers | Nein | 
> | servers/virtualNetworkRules | Nein | 

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | servers | Ja | 
> | servers/advisors | Nein | 
> | servers/queryTexts | Nein | 
> | servers/recoverableServers | Nein | 
> | servers/topQueryStatistics | Nein | 
> | servers/virtualNetworkRules | Nein | 
> | servers/waitStatistics | Nein | 

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | IotHubs | Ja | 
> | IotHubs/eventGridFilters | Nein | 
> | ProvisioningServices | Ja | 
> | usages | Nein | 

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | Controller | Ja | 

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | labs | Ja | 
> | labs/serviceRunners | Ja | 
> | labs/virtualMachines | Ja | 
> | schedules | Ja | 

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | databaseAccountNames | Nein | 
> | databaseAccounts | Ja | 

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | domains | Ja | 
> | domains/domainOwnershipIdentifiers | Nein | 
> | generateSsoRequest | Nein | 
> | topLevelDomains | Nein | 
> | validateDomainRegistrationInformation | Nein | 

## <a name="microsoftdynamicslcs"></a>Microsoft.DynamicsLcs

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | lcsprojects | Nein | 
> | lcsprojects/clouddeployments | Nein | 
> | lcsprojects/connectors | Nein | 

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | domains | Ja | 
> | domains/topics | Nein | 
> | eventSubscriptions | Nein | 
> | extensionTopics | Nein | 
> | topics | Ja | 
> | topicTypes | Nein | 

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | clusters | Ja | 
> | namespaces | Ja | 
> | namespaces/authorizationrules | Nein | 
> | namespaces/disasterrecoveryconfigs | Nein | 
> | namespaces/eventhubs | Nein | 
> | namespaces/eventhubs/authorizationrules | Nein | 
> | namespaces/eventhubs/consumergroups | Nein | 

## <a name="microsoftfeatures"></a>Microsoft.Features

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | Features | Nein | 
> | providers | Nein | 

## <a name="microsoftgallery"></a>Microsoft.Gallery

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | enroll | Nein | 
> | galleryitems | Nein | 
> | generateartifactaccessuri | Nein | 
> | myareas | Nein | 
> | myareas/areas | Nein | 
> | myareas/areas/areas | Nein | 
> | myareas/areas/areas/galleryitems | Nein | 
> | myareas/areas/galleryitems | Nein | 
> | myareas/galleryitems | Nein | 
> | Registrieren | Nein | 
> | ressourcen | Nein | 
> | retrieveresourcesbyid | Nein | 

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | guestConfigurationAssignments | Nein | 
> | software | Nein | 

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | hanaInstances | Ja | 

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | clusters | Ja | 
> | clusters/applications | Nein | 

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | jobs | Ja | 

## <a name="microsoftinformationprotection"></a>Microsoft.InformationProtection

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | labelGroups | Nein | 
> | labelGroups/labels | Nein | 
> | labelGroups/labels/conditions | Nein | 
> | labelGroups/labels/subLabels | Nein | 
> | labelGroups/labels/subLabels/conditions | Nein | 

## <a name="microsoftinsights"></a>microsoft.insights

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | actiongroups | Ja | 
> | activityLogAlerts | Ja | 
> | alertrules | Ja | 
> | automatedExportSettings | Nein | 
> | autoscalesettings | Ja | 
> | baseline | Nein | 
> | calculatebaseline | Nein | 
> | components | Ja | 
> | components/events | Nein | 
> | components/pricingPlans | Nein | 
> | components/query | Nein | 
> | diagnosticSettings | Nein | 
> | diagnosticSettingsCategories | Nein | 
> | eventCategories | Nein | 
> | eventtypes | Nein | 
> | extendedDiagnosticSettings | Nein | 
> | logDefinitions | Nein | 
> | logprofiles | Nein | 
> | logs | Nein | 
> | metricAlerts | Ja |
> | migrateToNewPricingModel | Nein | 
> | myWorkbooks | Nein | 
> | Abfragen | Nein | 
> | rollbackToLegacyPricingModel | Nein | 
> | scheduledqueryrules | Ja | 
> | vmInsightsOnboardingStatuses | Nein | 
> | webtests | Ja | 
> | workbooks | Ja | 

## <a name="microsoftintune"></a>Microsoft.Intune

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | diagnosticSettings | Nein | 
> | diagnosticSettingsCategories | Nein | 

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | IoTApps | Ja | 

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | Graph | Ja | 

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | deletedVaults | Nein | 
> | vaults | Ja | 
> | vaults/accessPolicies | Nein | 
> | vaults/secrets | Nein | 

## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | clusters | Ja | 
> | clusters/databases | Nein | 
> | clusters/databases/dataconnections | Nein | 
> | clusters/databases/eventhubconnections | Nein | 

## <a name="microsoftlabservices"></a>Microsoft.LabServices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | labaccounts | Ja | 
> | users | Nein | 

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | accounts | Ja | 

## <a name="microsoftlocationservices"></a>Microsoft.LocationServices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | accounts | Ja | 

## <a name="microsoftloganalytics"></a>Microsoft.LogAnalytics

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | logs | Nein | 

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | integrationAccounts | Ja | 
> | workflows | Ja | 

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | commitmentPlans | Ja | 
> | webServices | Ja | 
> | Arbeitsbereiche | Ja | 

## <a name="microsoftmachinelearningexperimentation"></a>Microsoft.MachineLearningExperimentation

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | accounts | Ja | 
> | accounts/workspaces | Ja | 
> | accounts/workspaces/projects | Ja | 
> | teamAccounts | Ja | 
> | teamAccounts/workspaces | Ja | 
> | teamAccounts/workspaces/projects | Ja | 

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | accounts | Ja | 

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices
> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | workspaces | Ja | 
> | workspaces/computes | Nein | 

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | Identities | Nein | 
> | userAssignedIdentities | Ja | 

## <a name="microsoftmanagement"></a>Microsoft.Management

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | getEntities | Nein | 
> | managementGroups | Nein | 
> | ressourcen | Nein | 
> | startTenantBackfill | Nein | 
> | tenantBackfillStatus | Nein | 

## <a name="microsoftmaps"></a>Microsoft.Maps

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | accounts | Ja | 
> | accounts/eventGridFilters | Nein | 

## <a name="microsoftmarketplace"></a>Microsoft.Marketplace

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | offers | Nein | 
> | offerTypes | Nein | 
> | offerTypes/publishers | Nein | 
> | offerTypes/publishers/offers | Nein | 
> | offerTypes/publishers/offers/plans | Nein | 
> | offerTypes/publishers/offers/plans/agreements | Nein | 
> | offerTypes/publishers/offers/plans/configs | Nein | 
> | offerTypes/publishers/offers/plans/configs/importImage | Nein | 
> | privategalleryitems | Nein | 
> | products | Nein | 

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | classicDevServices | Ja | 
> | updateCommunicationPreference | Nein | 

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | agreements | Nein | 
> | offertypes | Nein | 

## <a name="microsoftmedia"></a>Microsoft.Media

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | mediaservices | Ja | 
> | mediaservices/accountFilters | Nein | 
> | mediaservices/assets | Nein | 
> | mediaservices/assets/assetFilters | Nein | 
> | mediaservices/contentKeyPolicies | Nein | 
> | mediaservices/eventGridFilters | Nein | 
> | mediaservices/liveEventOperations | Nein | 
> | mediaservices/liveEvents | Ja | 
> | mediaservices/liveEvents/liveOutputs | Nein | 
> | mediaservices/liveOutputOperations | Nein | 
> | mediaservices/streamingEndpointOperations | Nein | 
> | mediaservices/streamingEndpoints | Ja | 
> | mediaservices/streamingLocators | Nein | 
> | mediaservices/streamingPolicies | Nein | 
> | mediaservices/transforms | Nein | 
> | mediaservices/transforms/jobs | Nein | 

## <a name="microsoftmigrate"></a>Microsoft.Migrate

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | projects | Ja | 

## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | applicationGateways | Ja | 
> | applicationSecurityGroups | Ja | 
> | azureFirewallFqdnTags | Nein | 
> | azureFirewalls | Ja | 
> | bgpServiceCommunities | Nein | 
> | connections | Ja | 
> | ddosCustomPolicies | Ja | 
> | ddosProtectionPlans | Ja | 
> | dnsOperationStatuses | Nein | 
> | dnszones | Ja | 
> | dnszones/A | Nein | 
> | dnszones/AAAA | Nein | 
> | dnszones/all | Nein | 
> | dnszones/CAA | Nein | 
> | dnszones/CNAME | Nein | 
> | dnszones/MX | Nein | 
> | dnszones/NS | Nein | 
> | dnszones/PTR | Nein | 
> | dnszones/recordsets | Nein | 
> | dnszones/SOA | Nein | 
> | dnszones/SRV | Nein | 
> | dnszones/TXT | Nein | 
> | expressRouteCircuits | Ja | 
> | expressRouteServiceProviders | Nein | 
> | frontdoors | Ja | 
> | frontdoorWebApplicationFirewallPolicies | Ja | 
> | getDnsResourceReference | Nein | 
> | interfaceEndpoints | Ja | 
> | internalNotify | Nein | 
> | loadBalancers | Ja | 
> | localNetworkGateways | Ja | 
> | natGateways | Ja | 
> | networkIntentPolicies | Ja | 
> | networkInterfaces | Ja | 
> | networkProfiles | Ja | 
> | networkSecurityGroups | Ja | 
> | networkWatchers | Ja | 
> | networkWatchers/connectionMonitors | Ja | 
> | networkWatchers/lenses | Ja | 
> | networkWatchers/pingMeshes | Ja | 
> | privateLinkServices | Ja | 
> | publicIPAddresses | Ja | 
> | publicIPPrefixes | Ja | 
> | routeFilters | Ja | 
> | routeTables | Ja | 
> | serviceEndpointPolicies | Ja | 
> | trafficManagerGeographicHierarchies | Nein | 
> | trafficmanagerprofiles | Ja | 
> | trafficmanagerprofiles/heatMaps | Nein | 
> | virtualHubs | Ja | 
> | virtualNetworkGateways | Ja | 
> | virtualNetworks | Ja | 
> | virtualNetworkTaps | Ja | 
> | virtualWans | Ja | 
> | vpnGateways | Ja | 
> | vpnSites | Ja | 
> | webApplicationFirewallPolicies | Ja | 

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | namespaces | Ja | 
> | namespaces/notificationHubs | Ja | 

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | devices | Nein | 
> | linkTargets | Nein | 
> | storageInsightConfigs | Nein | 
> | workspaces | Ja | 
> | workspaces/dataSources | Nein | 
> | workspaces/linkedServices | Nein | 
> | workspaces/query | Nein | 

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | managementassociations | Nein | 
> | managementconfigurations | Ja | 
> | solutions | Ja | 
> | views | Ja | 

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | policyEvents | Nein | 
> | policyStates | Nein | 
> | policyTrackedResources | Nein | 
> | remediations | Nein | 

## <a name="microsoftportal"></a>Microsoft.Portal

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | consoles | Nein | 
> | dashboards | Ja | 
> | userSettings | Nein | 

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | workspaceCollections | Ja | 

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | capacities | Ja | 

## <a name="microsoftprojectoxford"></a>Microsoft.ProjectOxford

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | accounts | Ja | 

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | backupProtectedItems | Nein | 
> | vaults | Ja | 

## <a name="microsoftrelay"></a>Microsoft.Relay

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | namespaces | Ja | 
> | namespaces/authorizationrules | Nein | 
> | namespaces/hybridconnections | Nein | 
> | namespaces/hybridconnections/authorizationrules | Nein | 
> | namespaces/wcfrelays | Nein | 
> | namespaces/wcfrelays/authorizationrules | Nein | 

## <a name="microsoftresourcegraph"></a>Microsoft.ResourceGraph

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | ressourcen | Nein | 
> | subscriptionsStatus | Nein | 

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | availabilityStatuses | Nein | 
> | childAvailabilityStatuses | Nein | 
> | childResources | Nein | 
> | events | Nein | 
> | impactedResources | Nein | 
> | Benachrichtigungen | Nein | 

## <a name="microsoftresources"></a>Microsoft.Resources

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | deployments | Nein | 
> | deployments/operations | Nein | 
> | links | Nein | 
> | notifyResourceJobs | Nein | 
> | providers | Nein | 
> | resourceGroups | Nein | 
> | ressourcen | Nein | 
> | subscriptions | Nein | 
> | subscriptions/providers | Nein | 
> | subscriptions/resourceGroups | Nein | 
> | subscriptions/resourcegroups/resources | Nein | 
> | subscriptions/resources | Nein | 
> | subscriptions/tagnames | Nein | 
> | subscriptions/tagNames/tagValues | Nein | 
> | tenants | Nein | 

## <a name="microsoftsaas"></a>Microsoft.SaaS

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | applications | Ja | 
> | saasresources | Nein | 

## <a name="microsoftscheduler"></a>Microsoft.Scheduler

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | flows | Ja | 
> | jobcollections | Ja | 

## <a name="microsoftsearch"></a>Microsoft.Search

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | resourceHealthMetadata | Nein | 
> | searchServices | Ja | 

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | advancedThreatProtectionSettings | Nein | 
> | alerts | Nein | 
> | allowedConnections | Nein | 
> | appliances | Nein | 
> | applicationWhitelistings | Nein | 
> | AutoProvisioningSettings | Nein | 
> | Compliances | Nein | 
> | dataCollectionAgents | Nein | 
> | discoveredSecuritySolutions | Nein | 
> | externalSecuritySolutions | Nein | 
> | InformationProtectionPolicies | Nein | 
> | jitNetworkAccessPolicies | Nein | 
> | monitoring | Nein | 
> | monitoring/antimalware | Nein | 
> | monitoring/baseline | Nein | 
> | monitoring/patch | Nein | 
> | Richtlinien | Nein | 
> | pricings | Nein | 
> | securityContacts | Nein | 
> | securitySolutions | Nein | 
> | securitySolutionsReferenceData | Nein | 
> | securityStatus | Nein | 
> | securityStatus/endpoints | Nein | 
> | securityStatus/subnets | Nein | 
> | securityStatus/virtualMachines | Nein | 
> | securityStatuses | Nein | 
> | securityStatusesSummaries | Nein | 
> | settings | Nein | 
> | Tasks | Nein | 
> | topologies | Nein | 
> | workspaceSettings | Nein | 

## <a name="microsoftsecuritygraph"></a>Microsoft.SecurityGraph

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | diagnosticSettings | Nein | 
> | diagnosticSettingsCategories | Nein | 

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | namespaces | Ja | 
> | namespaces/authorizationrules | Nein | 
> | namespaces/disasterrecoveryconfigs | Nein | 
> | namespaces/eventgridfilters | Nein | 
> | namespaces/queues | Nein | 
> | namespaces/queues/authorizationrules | Nein | 
> | namespaces/topics | Nein | 
> | namespaces/topics/authorizationrules | Nein | 
> | namespaces/topics/subscriptions | Nein | 
> | namespaces/topics/subscriptions/rules | Nein | 
> | premiumMessagingRegions | Nein | 

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | clusters | Ja | 
> | clusters/applications | Nein | 

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | applications | Ja | 
> | gateways | Ja | 
> | networks | Ja | 
> | secrets | Ja | 
> | volumes | Ja | 

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | SignalR | Ja | 

## <a name="microsoftsolutions"></a>Microsoft.Solutions

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | applianceDefinitions | Ja | 
> | appliances | Ja | 
> | applicationDefinitions | Ja | 
> | applications | Ja | 
> | jitRequests | Ja | 

## <a name="microsoftsql"></a>Microsoft.SQL

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | managedInstances | Ja |
> | managedInstances/databases | Ja |
> | managedInstances/databases/backupShortTermRetentionPolicies | Nein |
> | managedInstances/databases/schemas/tables/columns/sensitivityLabels | Nein |
> | managedInstances/databases/vulnerabilityAssessments | Nein |
> | managedInstances/databases/vulnerabilityAssessments/rules/baselines | Nein |
> | managedInstances/encryptionProtector | Nein |
> | managedInstances/keys | Nein |
> | managedInstances/restorableDroppedDatabases/backupShortTermRetentionPolicies | Nein |
> | managedInstances/vulnerabilityAssessments | Nein |
> | servers | Ja | 
> | servers/administrators | Nein | 
> | servers/communicationLinks | Nein | 
> | servers/databases | Ja | 
> | servers/encryptionProtector | Nein | 
> | servers/firewallRules | Nein | 
> | servers/keys | Nein | 
> | servers/restorableDroppedDatabases | Nein | 
> | servers/serviceobjectives | Nein | 
> | servers/tdeCertificates | Nein | 


## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | SqlVirtualMachineGroups | Ja | 
> | SqlVirtualMachineGroups/AvailabilityGroupListeners | Nein | 
> | SqlVirtualMachines | Ja | 

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | storageAccounts | Ja | 
> | storageAccounts/blobServices | Nein | 
> | storageAccounts/fileServices | Nein | 
> | storageAccounts/queueServices | Nein | 
> | storageAccounts/services | Nein | 
> | storageAccounts/tableServices | Nein | 
> | usages | Nein | 

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | storageSyncServices | Ja | 
> | storageSyncServices/registeredServers | Nein | 
> | storageSyncServices/syncGroups | Nein | 
> | storageSyncServices/syncGroups/cloudEndpoints | Nein | 
> | storageSyncServices/syncGroups/serverEndpoints | Nein | 
> | storageSyncServices/workflows | Nein | 

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | managers | Ja | 

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | streamingjobs | Ja | 
> | streamingjobs/diagnosticSettings | Nein | 

## <a name="microsoftsubscription"></a>Microsoft.Subscription

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | CreateSubscription | Nein | 
> | SubscriptionDefinitions | Nein | 
> | SubscriptionOperations | Nein | 

## <a name="microsoftsupport"></a>microsoft.support

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | supporttickets | Nein | 

## <a name="microsoftterraformoss"></a>Microsoft.TerraformOSS

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | providerRegistrations | Ja | 
> | ressourcen | Ja | 

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | environments | Ja | 
> | environments/accessPolicies | Nein | 
> | environments/eventsources | Ja | 
> | environments/referenceDataSets | Ja | 

## <a name="microsoftvisualstudio"></a>microsoft.visualstudio

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | account | Ja | 
> | account/extension | Ja | 
> | account/project | Ja | 

## <a name="microsoftweb"></a>Microsoft.Web

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | apiManagementAccounts | Nein | 
> | apiManagementAccounts/apiAcls | Nein | 
> | apiManagementAccounts/apis | Nein | 
> | apiManagementAccounts/apis/apiAcls | Nein | 
> | apiManagementAccounts/apis/connectionAcls | Nein | 
> | apiManagementAccounts/apis/connections | Nein | 
> | apiManagementAccounts/apis/connections/connectionAcls | Nein | 
> | apiManagementAccounts/apis/localizedDefinitions | Nein | 
> | apiManagementAccounts/connectionAcls | Nein | 
> | apiManagementAccounts/connections | Nein | 
> | billingMeters | Nein | 
> | certificates | Ja | 
> | connectionGateways | Ja | 
> | connections | Ja | 
> | customApis | Ja | 
> | deletedSites | Nein | 
> | functions | Nein | 
> | hostingEnvironments | Ja | 
> | hostingEnvironments/multiRolePools | Nein | 
> | hostingEnvironments/multiRolePools/instances | Nein | 
> | hostingEnvironments/workerPools | Nein | 
> | hostingEnvironments/workerPools/instances | Nein | 
> | publishingUsers | Nein | 
> | empfehlungen | Nein | 
> | resourceHealthMetadata | Nein | 
> | runtimes | Nein | 
> | serverFarms | Ja | 
> | serverFarms/workers | Nein | 
> | sites | Ja | 
> | sites/domainOwnershipIdentifiers | Nein | 
> | sites/hostNameBindings | Nein | 
> | sites/instances | Nein | 
> | sites/instances/extensions | Nein | 
> | sites/premieraddons | Ja | 
> | sites/recommendations | Nein | 
> | sites/resourceHealthMetadata | Nein | 
> | sites/slots | Ja | 
> | sites/slots/hostNameBindings | Nein | 
> | sites/slots/instances | Nein | 
> | sites/slots/instances/extensions | Nein | 
> | sourceControls | Nein | 
> | validate | Nein | 
> | verifyHostingEnvironmentVnet | Nein | 

## <a name="microsoftwindowsdefenderatp"></a>Microsoft.WindowsDefenderATP

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | diagnosticSettings | Nein | 
> | diagnosticSettingsCategories | Nein | 

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | DeviceServices | Ja | 

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | components | Nein | 
> | componentsSummary | Nein | 
> | monitorInstances | Nein | 
> | monitorInstancesSummary | Nein | 
> | monitors | Nein | 
> | notificationSettings | Nein | 

## <a name="next-steps"></a>Nächste Schritte

Um die gleichen Daten als Datei mit durch Trennzeichen getrennten Werten abzurufen, laden Sie [complete-mode-deletion.csv](https://github.com/tfitzmac/resource-capabilities/blob/master/complete-mode-deletion.csv) herunter.