---
title: Löschung des vollständigen Modus von Azure Resource Manager nach Ressourcentyp
description: Zeigt, wie die Ressourcentypen die Löschung des vollständigen Modus in Azure Resource Manager-Vorlagen verarbeiten.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 10/03/2019
ms.author: tomfitz
ms.openlocfilehash: cdc00ccc61e20865e993a18c72b930d9570896f2
ms.sourcegitcommit: f2d9d5133ec616857fb5adfb223df01ff0c96d0a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/03/2019
ms.locfileid: "71937052"
---
# <a name="deletion-of-azure-resources-for-complete-mode-deployments"></a>Löschen von Azure-Ressourcen für Bereitstellungen im vollständigen Modus

Dieser Artikel beschreibt, wie Ressourcentypen das Löschen handhaben, wenn sie sich nicht in einer Vorlage befinden, die im vollständigen Modus bereitgestellt wird.

Die mit **Ja** markierten Ressourcentypen werden gelöscht, wenn sich der Typ nicht in der Vorlage befindet, die im vollständigen Modus bereitgestellt wird.

Die mit **Nein** markierten Ressourcentypen werden nicht automatisch gelöscht, wenn sie sich nicht in der Vorlage befinden. Sie werden jedoch gelöscht, wenn die übergeordnete Ressource gelöscht wird. Eine vollständige Beschreibung des Verhaltens finden Sie unter [Azure Resource Manager-Bereitstellungsmodi](deployment-modes.md).

Navigieren Sie direkt zu einem Ressourcenanbieter-Namespace:
> [!div class="op_single_selector"]
> - [Microsoft.AAD](#microsoftaad)
> - [Microsoft.Addons](#microsoftaddons)
> - [Microsoft.ADHybridHealthService](#microsoftadhybridhealthservice)
> - [Microsoft.Advisor](#microsoftadvisor)
> - [Microsoft.AlertsManagement](#microsoftalertsmanagement)
> - [Microsoft.AnalysisServices](#microsoftanalysisservices)
> - [Microsoft.ApiManagement](#microsoftapimanagement)
> - [Microsoft.AppConfiguration](#microsoftappconfiguration)
> - [Microsoft.AppPlatform](#microsoftappplatform)
> - [Microsoft.Attestation](#microsoftattestation)
> - [Microsoft.Authorization](#microsoftauthorization)
> - [Microsoft.Automation](#microsoftautomation)
> - [Microsoft.Azconfig](#microsoftazconfig)
> - [Microsoft.Azure.Geneva](#microsoftazuregeneva)
> - [Microsoft.AzureActiveDirectory](#microsoftazureactivedirectory)
> - [Microsoft.AzureData](#microsoftazuredata)
> - [Microsoft.AzureStack](#microsoftazurestack)
> - [Microsoft.Batch](#microsoftbatch)
> - [Microsoft.Billing](#microsoftbilling)
> - [Microsoft.BingMaps](#microsoftbingmaps)
> - [Microsoft.Blockchain](#microsoftblockchain)
> - [Microsoft.Blueprint](#microsoftblueprint)
> - [Microsoft.BotService](#microsoftbotservice)
> - [Microsoft.Cache](#microsoftcache)
> - [Microsoft.Capacity](#microsoftcapacity)
> - [Microsoft.Cdn](#microsoftcdn)
> - [Microsoft.CertificateRegistration](#microsoftcertificateregistration)
> - [Microsoft.ClassicCompute](#microsoftclassiccompute)
> - [Microsoft.ClassicInfrastructureMigrate](#microsoftclassicinfrastructuremigrate)
> - [Microsoft.ClassicNetwork](#microsoftclassicnetwork)
> - [Microsoft.ClassicStorage](#microsoftclassicstorage)
> - [Microsoft.CognitiveServices](#microsoftcognitiveservices)
> - [Microsoft.Commerce](#microsoftcommerce)
> - [Microsoft.Compute](#microsoftcompute)
> - [Microsoft.Consumption](#microsoftconsumption)
> - [Microsoft.ContainerInstance](#microsoftcontainerinstance)
> - [Microsoft.ContainerRegistry](#microsoftcontainerregistry)
> - [Microsoft.ContainerService](#microsoftcontainerservice)
> - [Microsoft.ContentModerator](#microsoftcontentmoderator)
> - [Microsoft.CortanaAnalytics](#microsoftcortanaanalytics)
> - [Microsoft.CostManagement](#microsoftcostmanagement)
> - [Microsoft.CustomerLockbox](#microsoftcustomerlockbox)
> - [Microsoft.CustomProviders](#microsoftcustomproviders)
> - [Microsoft.DataBox](#microsoftdatabox)
> - [Microsoft.DataBoxEdge](#microsoftdataboxedge)
> - [Microsoft.Databricks](#microsoftdatabricks)
> - [Microsoft.DataCatalog](#microsoftdatacatalog)
> - [Microsoft.DataFactory](#microsoftdatafactory)
> - [Microsoft.DataLakeAnalytics](#microsoftdatalakeanalytics)
> - [Microsoft.DataLakeStore](#microsoftdatalakestore)
> - [Microsoft.DataMigration](#microsoftdatamigration)
> - [Microsoft.DataShare](#microsoftdatashare)
> - [Microsoft.DBforMariaDB](#microsoftdbformariadb)
> - [Microsoft.DBforMySQL](#microsoftdbformysql)
> - [Microsoft.DBforPostgreSQL](#microsoftdbforpostgresql)
> - [Microsoft.DeploymentManager](#microsoftdeploymentmanager)
> - [Microsoft.DesktopVirtualization](#microsoftdesktopvirtualization)
> - [Microsoft.Devices](#microsoftdevices)
> - [Microsoft.DevOps](#microsoftdevops)
> - [Microsoft.DevSpaces](#microsoftdevspaces)
> - [Microsoft.DevTestLab](#microsoftdevtestlab)
> - [Microsoft.DocumentDB](#microsoftdocumentdb)
> - [Microsoft.DomainRegistration](#microsoftdomainregistration)
> - [Microsoft.DynamicsLcs](#microsoftdynamicslcs)
> - [Microsoft.EnterpriseKnowledgeGraph](#microsoftenterpriseknowledgegraph)
> - [Microsoft.EventGrid](#microsofteventgrid)
> - [Microsoft.EventHub](#microsofteventhub)
> - [Microsoft.Features](#microsoftfeatures)
> - [Microsoft.Gallery](#microsoftgallery)
> - [Microsoft.Genomics](#microsoftgenomics)
> - [Microsoft.GuestConfiguration](#microsoftguestconfiguration)
> - [Microsoft.HanaOnAzure](#microsofthanaonazure)
> - [Microsoft.HardwareSecurityModules](#microsofthardwaresecuritymodules)
> - [Microsoft.HDInsight](#microsofthdinsight)
> - [Microsoft.HealthcareApis](#microsofthealthcareapis)
> - [Microsoft.HybridCompute](#microsofthybridcompute)
> - [Microsoft.HybridData](#microsofthybriddata)
> - [Microsoft.Hydra](#microsofthydra)
> - [Microsoft.ImportExport](#microsoftimportexport)
> - [Microsoft.Intune](#microsoftintune)
> - [Microsoft.IoTCentral](#microsoftiotcentral)
> - [Microsoft.IoTSpaces](#microsoftiotspaces)
> - [Microsoft.KeyVault](#microsoftkeyvault)
> - [Microsoft.Kusto](#microsoftkusto)
> - [Microsoft.LabServices](#microsoftlabservices)
> - [Microsoft.Logic](#microsoftlogic)
> - [Microsoft.MachineLearning](#microsoftmachinelearning)
> - [Microsoft.MachineLearningServices](#microsoftmachinelearningservices)
> - [Microsoft.ManagedIdentity](#microsoftmanagedidentity)
> - [Microsoft.ManagedServices](#microsoftmanagedservices)
> - [Microsoft.Management](#microsoftmanagement)
> - [Microsoft.Maps](#microsoftmaps)
> - [Microsoft.Marketplace](#microsoftmarketplace)
> - [Microsoft.MarketplaceApps](#microsoftmarketplaceapps)
> - [Microsoft.MarketplaceOrdering](#microsoftmarketplaceordering)
> - [Microsoft.Media](#microsoftmedia)
> - [Microsoft.Microservices4Spring](#microsoftmicroservices4spring)
> - [Microsoft.Migrate](#microsoftmigrate)
> - [Microsoft.MixedReality](#microsoftmixedreality)
> - [Microsoft.NetApp](#microsoftnetapp)
> - [Microsoft.Network](#microsoftnetwork)
> - [Microsoft.NotificationHubs](#microsoftnotificationhubs)
> - [Microsoft.ObjectStore](#microsoftobjectstore)
> - [Microsoft.OffAzure](#microsoftoffazure)
> - [Microsoft.OperationalInsights](#microsoftoperationalinsights)
> - [Microsoft.OperationsManagement](#microsoftoperationsmanagement)
> - [Microsoft.Peering](#microsoftpeering)
> - [Microsoft.PolicyInsights](#microsoftpolicyinsights)
> - [Microsoft.Portal](#microsoftportal)
> - [Microsoft.PowerBI](#microsoftpowerbi)
> - [Microsoft.PowerBIDedicated](#microsoftpowerbidedicated)
> - [Microsoft.RecoveryServices](#microsoftrecoveryservices)
> - [Microsoft.Relay](#microsoftrelay)
> - [Microsoft.RemoteApp](#microsoftremoteapp)
> - [Microsoft.ResourceGraph](#microsoftresourcegraph)
> - [Microsoft.ResourceHealth](#microsoftresourcehealth)
> - [Microsoft.Resources](#microsoftresources)
> - [Microsoft.SaaS](#microsoftsaas)
> - [Microsoft.Scheduler](#microsoftscheduler)
> - [Microsoft.Search](#microsoftsearch)
> - [Microsoft.Security](#microsoftsecurity)
> - [Microsoft.SecurityGraph](#microsoftsecuritygraph)
> - [Microsoft.SecurityInsights](#microsoftsecurityinsights)
> - [Microsoft.ServiceBus](#microsoftservicebus)
> - [Microsoft.ServiceFabric](#microsoftservicefabric)
> - [Microsoft.ServiceFabricMesh](#microsoftservicefabricmesh)
> - [Microsoft.Services](#microsoftservices)
> - [Microsoft.SignalRService](#microsoftsignalrservice)
> - [Microsoft.SiteRecovery](#microsoftsiterecovery)
> - [Microsoft.SoftwarePlan](#microsoftsoftwareplan)
> - [Microsoft.Solutions](#microsoftsolutions)
> - [Microsoft.SQL](#microsoftsql)
> - [Microsoft.SqlVirtualMachine](#microsoftsqlvirtualmachine)
> - [Microsoft.Storage](#microsoftstorage)
> - [Microsoft.StorageCache](#microsoftstoragecache)
> - [Microsoft.StorageReplication](#microsoftstoragereplication)
> - [Microsoft.StorageSync](#microsoftstoragesync)
> - [Microsoft.StorageSyncDev](#microsoftstoragesyncdev)
> - [Microsoft.StorageSyncInt](#microsoftstoragesyncint)
> - [Microsoft.StorSimple](#microsoftstorsimple)
> - [Microsoft.StreamAnalytics](#microsoftstreamanalytics)
> - [Microsoft.Subscription](#microsoftsubscription)
> - [Microsoft.TimeSeriesInsights](#microsofttimeseriesinsights)
> - [Microsoft.VMwareCloudSimple](#microsoftvmwarecloudsimple)
> - [Microsoft.Web](#microsoftweb)
> - [Microsoft.WindowsDefenderATP](#microsoftwindowsdefenderatp)
> - [Microsoft.WindowsIoT](#microsoftwindowsiot)
> - [Microsoft.WorkloadMonitor](#microsoftworkloadmonitor)

## <a name="microsoftaad"></a>Microsoft.AAD

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | DomainServices | Ja |
> | DomainServices/oucontainer | Nein |
> | DomainServices/ReplicaSets | Ja |

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
> | servicehealthmetrics | Nein |
> | services | Nein |

## <a name="microsoftadvisor"></a>Microsoft.Advisor

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | Konfigurationen | Nein |
> | generateRecommendations | Nein |
> | metadata | Nein |
> | empfehlungen | Nein |
> | suppressions | Nein |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | actionRules | Ja |
> | alerts | Nein |
> | alertsList | Nein |
> | alertsMetaData | Nein |
> | alertsSummary | Nein |
> | alertsSummaryList | Nein |
> | feedback | Nein |
> | smartDetectorAlertRules | Ja |
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

## <a name="microsoftappconfiguration"></a>Microsoft.AppConfiguration

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | configurationStores | Ja |
> | configurationStores/eventGridFilters | Nein |

## <a name="microsoftappplatform"></a>Microsoft.AppPlatform

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | Spring | Ja |

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
> | dataAliases | Nein |
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

## <a name="microsoftazconfig"></a>Microsoft.Azconfig

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | configurationStores | Ja |
> | configurationStores/eventGridFilters | Nein |

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
> | b2ctenants | Nein |

## <a name="microsoftazuredata"></a>Microsoft.AzureData

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | hybridDataManagers | Ja |
> | postgresInstances | Ja |
> | sqlBigDataClusters | Ja |
> | sqlInstances | Ja |
> | sqlServerRegistrations | Ja |
> | sqlServerRegistrations/sqlServers | Nein |

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
> | billingAccounts/agreements | Nein |
> | billingAccounts/billingPermissions | Nein |
> | billingAccounts/billingProfiles | Nein |
> | billingAccounts/billingProfiles/billingPermissions | Nein |
> | billingAccounts/billingProfiles/billingRoleAssignments | Nein |
> | billingAccounts/billingProfiles/billingRoleDefinitions | Nein |
> | billingAccounts/billingProfiles/billingSubscriptions | Nein |
> | billingAccounts/billingProfiles/createBillingRoleAssignment | Nein |
> | billingAccounts/billingProfiles/customers | Nein |
> | billingAccounts/billingProfiles/invoices | Nein |
> | billingAccounts/billingProfiles/invoices/pricesheet | Nein |
> | billingAccounts/billingProfiles/invoiceSections | Nein |
> | billingAccounts/billingProfiles/invoiceSections/billingPermissions | Nein |
> | billingAccounts/billingProfiles/invoiceSections/billingRoleAssignments | Nein |
> | billingAccounts/billingProfiles/invoiceSections/billingRoleDefinitions | Nein |
> | billingAccounts/billingProfiles/invoiceSections/billingSubscriptions | Nein |
> | billingAccounts/billingProfiles/invoiceSections/createBillingRoleAssignment | Nein |
> | billingAccounts/billingProfiles/invoiceSections/initiateTransfer | Nein |
> | billingAccounts/billingProfiles/invoiceSections/products | Nein |
> | billingAccounts/billingProfiles/invoiceSections/products/transfer | Nein |
> | billingAccounts/billingProfiles/invoiceSections/products/updateAutoRenew | Nein |
> | billingAccounts/billingProfiles/invoiceSections/transactions | Nein |
> | billingAccounts/billingProfiles/invoiceSections/transfers | Nein |
> | billingAccounts/BillingProfiles/patchOperations | Nein |
> | billingAccounts/billingProfiles/paymentMethods | Nein |
> | billingAccounts/billingProfiles/policies | Nein |
> | billingAccounts/billingProfiles/pricesheet | Nein |
> | billingAccounts/billingProfiles/pricesheetDownloadOperations | Nein |
> | billingAccounts/billingProfiles/products | Nein |
> | billingAccounts/billingProfiles/transactions | Nein |
> | billingAccounts/billingRoleAssignments | Nein |
> | billingAccounts/billingRoleDefinitions | Nein |
> | billingAccounts/billingSubscriptions | Nein |
> | billingAccounts/createBillingRoleAssignment | Nein |
> | billingAccounts/createInvoiceSectionOperations | Nein |
> | billingAccounts/customers | Nein |
> | billingAccounts/customers/billingSubscriptions | Nein |
> | billingAccounts/customers/initiateTransfer | Nein |
> | billingAccounts/customers/policies | Nein |
> | billingAccounts/customers/products | Nein |
> | billingAccounts/customers/transactions | Nein |
> | billingAccounts/customers/transfers | Nein |
> | billingAccounts/departments | Nein |
> | billingAccounts/enrollmentAccounts | Nein |
> | billingAccounts/invoices | Nein |
> | billingAccounts/invoiceSections | Nein |
> | billingAccounts/invoiceSections/billingSubscriptionMoveOperations | Nein |
> | billingAccounts/invoiceSections/billingSubscriptions | Nein |
> | billingAccounts/invoiceSections/billingSubscriptions/transfer | Nein |
> | billingAccounts/invoiceSections/elevate | Nein |
> | billingAccounts/invoiceSections/initiateTransfer | Nein |
> | billingAccounts/invoiceSections/patchOperations | Nein |
> | billingAccounts/invoiceSections/productMoveOperations | Nein |
> | billingAccounts/invoiceSections/products | Nein |
> | billingAccounts/invoiceSections/products/transfer | Nein |
> | billingAccounts/invoiceSections/products/updateAutoRenew | Nein |
> | billingAccounts/invoiceSections/transactions | Nein |
> | billingAccounts/invoiceSections/transfers | Nein |
> | billingAccounts/lineOfCredit | Nein |
> | billingAccounts/patchOperations | Nein |
> | billingAccounts/paymentMethods | Nein |
> | billingAccounts/products | Nein |
> | billingAccounts/transactions | Nein |
> | billingPeriods | Nein |
> | billingPermissions | Nein |
> | billingProperty | Nein |
> | billingRoleAssignments | Nein |
> | billingRoleDefinitions | Nein |
> | createBillingRoleAssignment | Nein |
> | departments | Nein |
> | enrollmentAccounts | Nein |
> | invoices | Nein |
> | transfers | Nein |
> | transfers/acceptTransfer | Nein |
> | transfers/declineTransfer | Nein |
> | transfers/operationStatus | Nein |
> | transfers/validateTransfer | Nein |
> | validateAddress | Nein |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | mapApis | Ja |
> | updateCommunicationPreference | Nein |

## <a name="microsoftblockchain"></a>Microsoft.Blockchain

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | blockchainMembers | Ja |
> | watchers | Ja |

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
> | languages | Nein |
> | Vorlagen | Nein |

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
> | calculateExchange | Nein |
> | calculatePrice | Nein |
> | calculatePurchasePrice | Nein |
> | catalogs | Nein |
> | commercialReservationOrders | Nein |
> | Börse | Nein |
> | placePurchaseOrder | Nein |
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
> | CdnWebApplicationFirewallManagedRuleSets | Nein |
> | CdnWebApplicationFirewallPolicies | Ja |
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
> | domainNames | Ja |
> | domainNames/capabilities | Nein |
> | domainNames/internalLoadBalancers | Nein |
> | domainNames/serviceCertificates | Nein |
> | domainNames/slots | Nein |
> | domainNames/slots/roles | Nein |
> | domainNames/slots/roles/metricDefinitions | Nein |
> | domainNames/slots/roles/metrics | Nein |
> | moveSubscriptionResources | Nein |
> | operatingSystemFamilies | Nein |
> | operatingSystems | Nein |
> | quotas | Nein |
> | resourceTypes | Nein |
> | validateSubscriptionMoveAvailability | Nein |
> | virtualMachines | Ja |
> | virtualMachines/diagnosticSettings | Nein |
> | virtualMachines/metricDefinitions | Nein |
> | virtualMachines/metrics | Nein |

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
> | networkSecurityGroups | Ja |
> | quotas | Nein |
> | reservedIps | Ja |
> | virtualNetworks | Ja |
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
> | storageAccounts | Ja |
> | storageAccounts/blobServices | Nein |
> | storageAccounts/fileServices | Nein |
> | storageAccounts/metricDefinitions | Nein |
> | storageAccounts/metrics | Nein |
> | storageAccounts/queueServices | Nein |
> | storageAccounts/services | Nein |
> | storageAccounts/services/diagnosticSettings | Nein |
> | storageAccounts/services/metricDefinitions | Nein |
> | storageAccounts/services/metrics | Nein |
> | storageAccounts/tableServices | Nein |
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
> | diskEncryptionSets | Ja |
> | disks | Ja |
> | galleries | Ja |
> | galleries/applications | Nein |
> | galleries/applications/versions | Nein |
> | galleries/images | Nein |
> | galleries/images/versions | Nein |
> | hostGroups | Ja |
> | hostGroups/hosts | Ja |
> | images | Ja |
> | proximityPlacementGroups | Ja |
> | restorePointCollections | Ja |
> | restorePointCollections/restorePoints | Nein |
> | sharedVMImages | Ja |
> | sharedVMImages/versions | Nein |
> | snapshots | Ja |
> | virtualMachines | Ja |
> | virtualMachines/extensions | Ja |
> | virtualMachines/metricDefinitions | Nein |
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
> | tenants | Nein |
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
> | openShiftManagedClusters | Ja |

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
> | Budgets | Nein |
> | CloudConnectors | Nein |
> | Connectors | Ja |
> | Departments | Nein |
> | Dimensionen | Nein |
> | EnrollmentAccounts | Nein |
> | Exports | Nein |
> | ExternalBillingAccounts | Nein |
> | ExternalBillingAccounts/Alerts | Nein |
> | ExternalBillingAccounts/Dimensions | Nein |
> | ExternalBillingAccounts/Forecast | Nein |
> | ExternalBillingAccounts/Query | Nein |
> | ExternalSubscriptions | Nein |
> | ExternalSubscriptions/Alerts | Nein |
> | ExternalSubscriptions/Dimensions | Nein |
> | ExternalSubscriptions/Forecast | Nein |
> | ExternalSubscriptions/Query | Nein |
> | Forecast | Nein |
> | Abfragen | Nein |
> | Registrieren | Nein |
> | Reportconfigs | Nein |
> | Berichte | Nein |
> | Einstellungen | Nein |
> | showbackRules | Nein |
> | Ansichten | Nein |

## <a name="microsoftcustomerlockbox"></a>Microsoft.CustomerLockbox

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | requests | Nein |

## <a name="microsoftcustomproviders"></a>Microsoft.CustomProviders

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | associations | Nein |
> | resourceProviders | Ja |

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
> | datacatalogs | Ja |
> | datacatalogs/datasources | Nein |
> | datacatalogs/datasources/scans | Nein |
> | datacatalogs/datasources/scans/datasets | Nein |
> | datacatalogs/datasources/scans/triggers | Nein |
> | datacatalogs/scantargets | Nein |
> | datacatalogs/scantargets/datasets | Nein |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | dataFactories | Ja |
> | dataFactories/diagnosticSettings | Nein |
> | dataFactories/metricDefinitions | Nein |
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
> | accounts/transferAnalyticsUnits | Nein |

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

## <a name="microsoftdatashare"></a>Microsoft.DataShare

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | accounts | Ja |
> | accounts/shares | Nein |
> | accounts/shares/datasets | Nein |
> | accounts/shares/invitations | Nein |
> | accounts/shares/providersharesubscriptions | Nein |
> | accounts/shares/synchronizationSettings | Nein |
> | accounts/sharesubscriptions | Nein |
> | accounts/sharesubscriptions/consumerSourceDataSets | Nein |
> | accounts/sharesubscriptions/datasetmappings | Nein |
> | accounts/sharesubscriptions/triggers | Nein |

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB

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

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

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

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | serverGroups | Ja |
> | servers | Ja |
> | servers/advisors | Nein |
> | servers/privateEndpointConnectionProxies | Nein |
> | servers/privateEndpointConnections | Nein |
> | servers/privateLinkResources | Nein |
> | servers/queryTexts | Nein |
> | servers/recoverableServers | Nein |
> | servers/topQueryStatistics | Nein |
> | servers/virtualNetworkRules | Nein |
> | servers/waitStatistics | Nein |
> | serversv2 | Ja |

## <a name="microsoftdeploymentmanager"></a>Microsoft.DeploymentManager

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | artifactSources | Ja |
> | rollouts | Ja |
> | serviceTopologies | Ja |
> | serviceTopologies/services | Ja |
> | serviceTopologies/services/serviceUnits | Ja |
> | steps | Ja |

## <a name="microsoftdesktopvirtualization"></a>Microsoft.DesktopVirtualization

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | applicationgroups | Ja |
> | applicationgroups/applications | Nein |
> | applicationgroups/startmenuitems | Nein |
> | hostpools | Ja |
> | hostpools/sessionhosts | Nein |
> | hostpools/sessionhosts/usersessions | Nein |
> | hostpools/usersessions | Nein |
> | workspaces | Ja |

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | ElasticPools | Ja |
> | ElasticPools/IotHubTenants | Ja |
> | IotHubs | Ja |
> | IotHubs/eventGridFilters | Nein |
> | ProvisioningServices | Ja |
> | usages | Nein |

## <a name="microsoftdevops"></a>Microsoft.DevOps

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | pipelines | Ja |

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | Controller | Ja |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | labcenters | Ja |
> | labs | Ja |
> | labs/environments | Ja |
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

## <a name="microsoftenterpriseknowledgegraph"></a>Microsoft.EnterpriseKnowledgeGraph

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | services | Ja |

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
> | namespaces/networkrulesets | Nein |

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

## <a name="microsoftgenomics"></a>Microsoft.Genomics

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | accounts | Ja |

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | autoManagedVmConfigurationProfiles | Ja |
> | guestConfigurationAssignments | Nein |
> | software | Nein |
> | softwareUpdateProfile | Nein |
> | softwareUpdates | Nein |

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | hanaInstances | Ja |
> | sapMonitors | Ja |

## <a name="microsofthardwaresecuritymodules"></a>Microsoft.HardwareSecurityModules

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | dedicatedHSMs | Ja |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | clusters | Ja |
> | clusters/applications | Nein |

## <a name="microsofthealthcareapis"></a>Microsoft.HealthcareApis

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | services | Ja |

## <a name="microsofthybridcompute"></a>Microsoft.HybridCompute

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | machines | Ja |

## <a name="microsofthybriddata"></a>Microsoft.HybridData

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | dataManagers | Ja |

## <a name="microsofthydra"></a>Microsoft.Hydra

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | components | Ja |
> | networkScopes | Ja |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | jobs | Ja |

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
> | appTemplates | Nein |
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
> | hsmPools | Ja |
> | vaults | Ja |
> | vaults/accessPolicies | Nein |
> | vaults/eventGridFilters | Nein |
> | vaults/secrets | Nein |

## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | clusters | Ja |
> | clusters/attacheddatabaseconfigurations | Nein |
> | clusters/databases | Nein |
> | clusters/databases/dataconnections | Nein |
> | clusters/databases/eventhubconnections | Nein |
> | Cluster/sharedidentities | Nein |

## <a name="microsoftlabservices"></a>Microsoft.LabServices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | labaccounts | Ja |
> | users | Nein |

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | hostingEnvironments | Ja |
> | integrationAccounts | Ja |
> | integrationServiceEnvironments | Ja |
> | integrationServiceEnvironments/managedApis | Ja |
> | isolatedEnvironments | Ja |
> | workflows | Ja |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | commitmentPlans | Ja |
> | webServices | Ja |
> | Arbeitsbereiche | Ja |

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

## <a name="microsoftmanagedservices"></a>Microsoft.ManagedServices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | marketplaceRegistrationDefinitions | Nein |
> | registrationAssignments | Nein |
> | registrationDefinitions | Nein |

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
> | publishers | Nein |
> | publishers/offers | Nein |
> | publishers/offers/amendments | Nein |

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

## <a name="microsoftmicroservices4spring"></a>Microsoft.Microservices4Spring

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | appClusters | Ja |

## <a name="microsoftmigrate"></a>Microsoft.Migrate

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | assessmentProjects | Ja |
> | migrateprojects | Ja |
> | projects | Ja |

## <a name="microsoftmixedreality"></a>Microsoft.MixedReality

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | holographicsBroadcastAccounts | Ja |
> | objectUnderstandingAccounts | Ja |
> | remoteRenderingAccounts | Ja |
> | spatialAnchorsAccounts | Ja |
> | surfaceReconstructionAccounts | Ja |

## <a name="microsoftnetapp"></a>Microsoft.NetApp

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | netAppAccounts | Ja |
> | netAppAccounts/backupPolicies | Ja |
> | netAppAccounts/capacityPools | Ja |
> | netAppAccounts/capacityPools/volumes | Ja |
> | netAppAccounts/capacityPools/volumes/backups | Nein |
> | netAppAccounts/capacityPools/volumes/mountTargets | Ja |
> | netAppAccounts/capacityPools/volumes/snapshots | Ja |
> | netAppAccounts/vaults | Nein |
## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | applicationGateways | Ja |
> | applicationGatewayWebApplicationFirewallPolicies | Ja |
> | applicationSecurityGroups | Ja |
> | azureFirewallFqdnTags | Nein |
> | azureFirewalls | Ja |
> | bastionHosts | Ja |
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
> | expressRouteCrossConnections | Ja |
> | expressRouteGateways | Ja |
> | expressRoutePorts | Ja |
> | expressRouteServiceProviders | Nein |
> | firewallPolicies | Ja |
> | frontdoors | Ja |
> | frontdoorWebApplicationFirewallManagedRuleSets | Nein |
> | frontdoorWebApplicationFirewallPolicies | Ja |
> | getDnsResourceReference | Nein |
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
> | p2sVpnGateways | Ja |
> | privateDnsOperationStatuses | Nein |
> | privateDnsZones | Ja |
> | privateDnsZones/A | Nein |
> | privateDnsZones/AAAA | Nein |
> | privateDnsZones/all | Nein |
> | privateDnsZones/CNAME | Nein |
> | privateDnsZones/MX | Nein |
> | privateDnsZones/PTR | Nein |
> | privateDnsZones/SOA | Nein |
> | privateDnsZones/SRV | Nein |
> | privateDnsZones/TXT | Nein |
> | privateDnsZones/virtualNetworkLinks | Ja |
> | privateEndpoints | Ja |
> | privateLinkServices | Ja |
> | publicIPAddresses | Ja |
> | publicIPPrefixes | Ja |
> | routeFilters | Ja |
> | routeTables | Ja |
> | secureGateways | Ja |
> | serviceEndpointPolicies | Ja |
> | trafficManagerGeographicHierarchies | Nein |
> | trafficmanagerprofiles | Ja |
> | trafficmanagerprofiles/heatMaps | Nein |
> | trafficManagerUserMetricsKeys | Nein |
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

## <a name="microsoftobjectstore"></a>Microsoft.ObjectStore

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | osNamespaces | Ja |

## <a name="microsoftoffazure"></a>Microsoft.OffAzure

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | HyperVSites | Ja |
> | ImportSites | Ja |
> | ServerSites | Ja |
> | VMwareSites | Ja |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | clusters | Ja |
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

## <a name="microsoftpeering"></a>Microsoft.Peering

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | legacyPeerings | Nein |
> | peerAsns | Nein |
> | peerings | Ja |
> | peeringServiceProviders | Nein |
> | peeringServices | Ja |

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

## <a name="microsoftremoteapp"></a>Microsoft.RemoteApp

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | accounts | Nein |
> | collections | Ja |
> | collections/applications | Nein |
> | collections/securityprincipals | Nein |
> | templateImages | Nein |

## <a name="microsoftresourcegraph"></a>Microsoft.ResourceGraph

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | Abfragen | Ja |
> | resourceChangeDetails | Nein |
> | resourceChanges | Nein |
> | ressourcen | Nein |
> | resourcesHistory | Nein |
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
> | metadata | Nein |
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
> | adaptiveNetworkHardenings | Nein |
> | advancedThreatProtectionSettings | Nein |
> | alerts | Nein |
> | allowedConnections | Nein |
> | applicationWhitelistings | Nein |
> | assessmentMetadata | Nein |
> | assessments | Nein |
> | AutoProvisioningSettings | Nein |
> | Compliances | Nein |
> | dataCollectionAgents | Nein |
> | deviceSecurityGroups | Nein |
> | discoveredSecuritySolutions | Nein |
> | externalSecuritySolutions | Nein |
> | InformationProtectionPolicies | Nein |
> | iotSecuritySolutions | Ja |
> | iotSecuritySolutions/analyticsModels | Nein |
> | iotSecuritySolutions/analyticsModels/aggregatedAlerts | Nein |
> | iotSecuritySolutions/analyticsModels/aggregatedRecommendations | Nein |
> | jitNetworkAccessPolicies | Nein |
> | networkData | Nein |
> | playbookConfigurations | Ja |
> | Richtlinien | Nein |
> | pricings | Nein |
> | regulatoryComplianceStandards | Nein |
> | regulatoryComplianceStandards/regulatoryComplianceControls | Nein |
> | regulatoryComplianceStandards/regulatoryComplianceControls/regulatoryComplianceAssessments | Nein |
> | securityContacts | Nein |
> | securitySolutions | Nein |
> | securitySolutionsReferenceData | Nein |
> | securityStatuses | Nein |
> | securityStatusesSummaries | Nein |
> | serverVulnerabilityAssessments | Nein |
> | settings | Nein |
> | subAssessments | Nein |
> | Tasks | Nein |
> | topologies | Nein |
> | workspaceSettings | Nein |

## <a name="microsoftsecuritygraph"></a>Microsoft.SecurityGraph

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | diagnosticSettings | Nein |
> | diagnosticSettingsCategories | Nein |

## <a name="microsoftsecurityinsights"></a>Microsoft.SecurityInsights

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | aggregations | Nein |
> | alertRules | Nein |
> | alertRuleTemplates | Nein |
> | bookmarks | Nein |
> | cases | Nein |
> | dataConnectors | Nein |
> | entities | Nein |
> | entityQueries | Nein |
> | officeConsents | Nein |
> | settings | Nein |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | namespaces | Ja |
> | namespaces/authorizationrules | Nein |
> | namespaces/disasterrecoveryconfigs | Nein |
> | namespaces/eventgridfilters | Nein |
> | namespaces/networkrulesets | Nein |
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
> | applications | Ja |
> | clusters | Ja |
> | clusters/applications | Nein |
> | containerGroups | Ja |
> | containerGroupSets | Ja |
> | edgeclusters | Ja |
> | edgeclusters/applications | Nein |
> | networks | Ja |
> | secretstores | Ja |
> | secretstores/certificates | Nein |
> | secretstores/secrets | Nein |
> | volumes | Ja |

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | applications | Ja |
> | containerGroups | Ja |
> | gateways | Ja |
> | networks | Ja |
> | secrets | Ja |
> | volumes | Ja |

## <a name="microsoftservices"></a>Microsoft.Services

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | providerRegistrations | Nein |
> | providerRegistrations/resourceTypeRegistrations | Nein |
> | rollouts | Ja |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | SignalR | Ja |
> | SignalR/eventGridFilters | Nein |

## <a name="microsoftsiterecovery"></a>Microsoft.SiteRecovery

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | SiteRecoveryVault | Ja |

## <a name="microsoftsoftwareplan"></a>Microsoft.SoftwarePlan

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | hybridUseBenefits | Nein |

## <a name="microsoftsolutions"></a>Microsoft.Solutions

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
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
> | virtualClusters | Nein |

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
> | storageAccounts/services/metricDefinitions | Nein |
> | storageAccounts/tableServices | Nein |
> | usages | Nein |

## <a name="microsoftstoragecache"></a>Microsoft.StorageCache

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | caches | Ja |
> | caches/storageTargets | Nein |
> | usageModels | Nein |

## <a name="microsoftstoragereplication"></a>Microsoft.StorageReplication

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | replicationGroups | Nein |

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

## <a name="microsoftstoragesyncdev"></a>Microsoft.StorageSyncDev

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | storageSyncServices | Ja |
> | storageSyncServices/registeredServers | Nein |
> | storageSyncServices/syncGroups | Nein |
> | storageSyncServices/syncGroups/cloudEndpoints | Nein |
> | storageSyncServices/syncGroups/serverEndpoints | Nein |
> | storageSyncServices/workflows | Nein |

## <a name="microsoftstoragesyncint"></a>Microsoft.StorageSyncInt

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

## <a name="microsoftsubscription"></a>Microsoft.Subscription

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | cancel | Nein |
> | CreateSubscription | Nein |
> | enable | Nein |
> | rename | Nein |
> | SubscriptionDefinitions | Nein |
> | SubscriptionOperations | Nein |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | environments | Ja |
> | environments/accessPolicies | Nein |
> | environments/eventsources | Ja |
> | environments/referenceDataSets | Ja |

## <a name="microsoftvmwarecloudsimple"></a>Microsoft.VMwareCloudSimple

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Löschung des vollständigen Modus |
> | ------------- | ----------- |
> | dedicatedCloudNodes | Ja |
> | dedicatedCloudServices | Ja |
> | virtualMachines | Ja |

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
> | hostingEnvironments/workerPools | Nein |
> | publishingUsers | Nein |
> | empfehlungen | Nein |
> | resourceHealthMetadata | Nein |
> | runtimes | Nein |
> | serverFarms | Ja |
> | serverFarms/eventGridFilters | Nein |
> | sites | Ja |
> | sites/config  | Nein |
> | sites/eventGridFilters | Nein |
> | sites/hostNameBindings | Nein |
> | sites/networkConfig | Nein |
> | sites/premieraddons | Ja |
> | sites/slots | Ja |
> | sites/slots/eventGridFilters | Nein |
> | sites/slots/hostNameBindings | Nein |
> | sites/slots/networkConfig | Nein |
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
