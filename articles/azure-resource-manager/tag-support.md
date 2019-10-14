---
title: 'Azure Resource Manager: Tagunterstützung für Ressourcen'
description: Zeigt, welche Azure-Ressourcentypen Tags unterstützen. Enthält Details für alle Azure-Dienste.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 10/03/2019
ms.author: tomfitz
ms.openlocfilehash: 06028224379bd42e1e0ea21b684ce443abd5992a
ms.sourcegitcommit: f2d9d5133ec616857fb5adfb223df01ff0c96d0a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/03/2019
ms.locfileid: "71937011"
---
# <a name="tag-support-for-azure-resources"></a>Tagunterstützung für Azure-Ressourcen
In diesem Artikel erfahren Sie, ob ein Ressourcentyp [Tags](resource-group-using-tags.md) unterstützt. Die Spalte mit der Bezeichnung **Tagunterstützung** zeigt an, ob der Ressourcentyp eine Eigenschaft für das Tag hat. Die Spalte mit der Bezeichnung **Tag in Kostenbericht** gibt an, ob dieser Ressourcentyp das Tag an den Kostenbericht übergibt.

Um die Daten als Datei mit durch Trennzeichen getrennten Werten abzurufen, laden Sie [tag-support.csv](https://github.com/tfitzmac/resource-capabilities/blob/master/tag-support.csv) herunter.

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
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | DomainServices | Ja |Ja |
> | DomainServices/oucontainer | Nein |Nein |
> | DomainServices/ReplicaSets | Ja |Ja |

## <a name="microsoftaddons"></a>Microsoft.Addons

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | supportProviders | Nein |Nein |

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | aadsupportcases | Nein |Nein |
> | addsservices | Nein |Nein |
> | agents | Nein |Nein |
> | anonymousapiusers | Nein |Nein |
> | Konfiguration | Nein |Nein |
> | logs | Nein |Nein |
> | reports | Nein |Nein |
> | servicehealthmetrics | Nein |Nein |
> | services | Nein |Nein |

## <a name="microsoftadvisor"></a>Microsoft.Advisor

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | Konfigurationen | Nein |Nein |
> | generateRecommendations | Nein |Nein |
> | metadata | Nein |Nein |
> | empfehlungen | Nein |Nein |
> | suppressions | Nein |Nein |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | actionRules | Ja |Ja |
> | alerts | Nein |Nein |
> | alertsList | Nein |Nein |
> | alertsMetaData | Nein |Nein |
> | alertsSummary | Nein |Nein |
> | alertsSummaryList | Nein |Nein |
> | feedback | Nein |Nein |
> | smartDetectorAlertRules | Ja |Ja |
> | smartDetectorRuntimeEnvironments | Nein |Nein |
> | smartGroups | Nein |Nein |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | servers | Ja |Ja |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | reportFeedback | Nein |Nein |
> | service | Ja |Ja |
> | validateServiceName | Nein |Nein |

## <a name="microsoftappconfiguration"></a>Microsoft.AppConfiguration

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | configurationStores | Ja |Ja |
> | configurationStores/eventGridFilters | Nein |Nein |

## <a name="microsoftappplatform"></a>Microsoft.AppPlatform

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | Spring | Ja |Ja |

## <a name="microsoftattestation"></a>Microsoft.Attestation

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | attestationProviders | Nein |Nein |

## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | classicAdministrators | Nein |Nein |
> | dataAliases | Nein |Nein |
> | denyAssignments | Nein |Nein |
> | elevateAccess | Nein |Nein |
> | locks | Nein |Nein |
> | Berechtigungen | Nein |Nein |
> | policyAssignments | Nein |Nein |
> | policyDefinitions | Nein |Nein |
> | policySetDefinitions | Nein |Nein |
> | providerOperations | Nein |Nein |
> | roleAssignments | Nein |Nein |
> | roleDefinitions | Nein |Nein |

## <a name="microsoftautomation"></a>Microsoft.Automation

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | automationAccounts | Ja |Ja |
> | automationAccounts/configurations | Ja |Ja |
> | automationAccounts/jobs | Nein |Nein |
> | automationAccounts/runbooks | Ja |Ja |
> | automationAccounts/softwareUpdateConfigurations | Nein |Nein |
> | automationAccounts/webhooks | Nein |Nein |

## <a name="microsoftazconfig"></a>Microsoft.Azconfig

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | configurationStores | Ja |Ja |
> | configurationStores/eventGridFilters | Nein |Nein |

## <a name="microsoftazuregeneva"></a>Microsoft.Azure.Geneva

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | environments | Nein |Nein |
> | environments/accounts | Nein |Nein |
> | environments/accounts/namespaces | Nein |Nein |
> | environments/accounts/namespaces/configurations | Nein |Nein |

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | b2cDirectories | Ja |Nein |
> | b2ctenants | Nein |Nein |

## <a name="microsoftazuredata"></a>Microsoft.AzureData

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | hybridDataManagers | Ja |Ja |
> | postgresInstances | Ja |Ja |
> | sqlBigDataClusters | Ja |Ja |
> | sqlInstances | Ja |Ja |
> | sqlServerRegistrations | Ja |Ja |
> | sqlServerRegistrations/sqlServers | Nein |Nein |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | registrations | Ja |Ja |
> | registrations/customerSubscriptions | Nein |Nein |
> | registrations/products | Nein |Nein |

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | batchAccounts | Ja |Ja |

## <a name="microsoftbilling"></a>Microsoft.Billing

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | billingAccounts | Nein |Nein |
> | billingAccounts/agreements | Nein |Nein |
> | billingAccounts/billingPermissions | Nein |Nein |
> | billingAccounts/billingProfiles | Nein |Nein |
> | billingAccounts/billingProfiles/billingPermissions | Nein |Nein |
> | billingAccounts/billingProfiles/billingRoleAssignments | Nein |Nein |
> | billingAccounts/billingProfiles/billingRoleDefinitions | Nein |Nein |
> | billingAccounts/billingProfiles/billingSubscriptions | Nein |Nein |
> | billingAccounts/billingProfiles/createBillingRoleAssignment | Nein |Nein |
> | billingAccounts/billingProfiles/customers | Nein |Nein |
> | billingAccounts/billingProfiles/invoices | Nein |Nein |
> | billingAccounts/billingProfiles/invoices/pricesheet | Nein |Nein |
> | billingAccounts/billingProfiles/invoiceSections | Nein |Nein |
> | billingAccounts/billingProfiles/invoiceSections/billingPermissions | Nein |Nein |
> | billingAccounts/billingProfiles/invoiceSections/billingRoleAssignments | Nein |Nein |
> | billingAccounts/billingProfiles/invoiceSections/billingRoleDefinitions | Nein |Nein |
> | billingAccounts/billingProfiles/invoiceSections/billingSubscriptions | Nein |Nein |
> | billingAccounts/billingProfiles/invoiceSections/createBillingRoleAssignment | Nein |Nein |
> | billingAccounts/billingProfiles/invoiceSections/initiateTransfer | Nein |Nein |
> | billingAccounts/billingProfiles/invoiceSections/products | Nein |Nein |
> | billingAccounts/billingProfiles/invoiceSections/products/transfer | Nein |Nein |
> | billingAccounts/billingProfiles/invoiceSections/products/updateAutoRenew | Nein |Nein |
> | billingAccounts/billingProfiles/invoiceSections/transactions | Nein |Nein |
> | billingAccounts/billingProfiles/invoiceSections/transfers | Nein |Nein |
> | billingAccounts/BillingProfiles/patchOperations | Nein |Nein |
> | billingAccounts/billingProfiles/paymentMethods | Nein |Nein |
> | billingAccounts/billingProfiles/policies | Nein |Nein |
> | billingAccounts/billingProfiles/pricesheet | Nein |Nein |
> | billingAccounts/billingProfiles/pricesheetDownloadOperations | Nein |Nein |
> | billingAccounts/billingProfiles/products | Nein |Nein |
> | billingAccounts/billingProfiles/transactions | Nein |Nein |
> | billingAccounts/billingRoleAssignments | Nein |Nein |
> | billingAccounts/billingRoleDefinitions | Nein |Nein |
> | billingAccounts/billingSubscriptions | Nein |Nein |
> | billingAccounts/createBillingRoleAssignment | Nein |Nein |
> | billingAccounts/createInvoiceSectionOperations | Nein |Nein |
> | billingAccounts/customers | Nein |Nein |
> | billingAccounts/customers/billingSubscriptions | Nein |Nein |
> | billingAccounts/customers/initiateTransfer | Nein |Nein |
> | billingAccounts/customers/policies | Nein |Nein |
> | billingAccounts/customers/products | Nein |Nein |
> | billingAccounts/customers/transactions | Nein |Nein |
> | billingAccounts/customers/transfers | Nein |Nein |
> | billingAccounts/departments | Nein |Nein |
> | billingAccounts/enrollmentAccounts | Nein |Nein |
> | billingAccounts/invoices | Nein |Nein |
> | billingAccounts/invoiceSections | Nein |Nein |
> | billingAccounts/invoiceSections/billingSubscriptionMoveOperations | Nein |Nein |
> | billingAccounts/invoiceSections/billingSubscriptions | Nein |Nein |
> | billingAccounts/invoiceSections/billingSubscriptions/transfer | Nein |Nein |
> | billingAccounts/invoiceSections/elevate | Nein |Nein |
> | billingAccounts/invoiceSections/initiateTransfer | Nein |Nein |
> | billingAccounts/invoiceSections/patchOperations | Nein |Nein |
> | billingAccounts/invoiceSections/productMoveOperations | Nein |Nein |
> | billingAccounts/invoiceSections/products | Nein |Nein |
> | billingAccounts/invoiceSections/products/transfer | Nein |Nein |
> | billingAccounts/invoiceSections/products/updateAutoRenew | Nein |Nein |
> | billingAccounts/invoiceSections/transactions | Nein |Nein |
> | billingAccounts/invoiceSections/transfers | Nein |Nein |
> | billingAccounts/lineOfCredit | Nein |Nein |
> | billingAccounts/patchOperations | Nein |Nein |
> | billingAccounts/paymentMethods | Nein |Nein |
> | billingAccounts/products | Nein |Nein |
> | billingAccounts/transactions | Nein |Nein |
> | billingPeriods | Nein |Nein |
> | billingPermissions | Nein |Nein |
> | billingProperty | Nein |Nein |
> | billingRoleAssignments | Nein |Nein |
> | billingRoleDefinitions | Nein |Nein |
> | createBillingRoleAssignment | Nein |Nein |
> | departments | Nein |Nein |
> | enrollmentAccounts | Nein |Nein |
> | invoices | Nein |Nein |
> | transfers | Nein |Nein |
> | transfers/acceptTransfer | Nein |Nein |
> | transfers/declineTransfer | Nein |Nein |
> | transfers/operationStatus | Nein |Nein |
> | transfers/validateTransfer | Nein |Nein |
> | validateAddress | Nein |Nein |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | mapApis | Ja |Ja |
> | updateCommunicationPreference | Nein |Nein |

## <a name="microsoftblockchain"></a>Microsoft.Blockchain

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | blockchainMembers | Ja |Ja |
> | watchers | Ja |Ja |

## <a name="microsoftblueprint"></a>Microsoft.Blueprint

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | blueprintAssignments | Nein |Nein |
> | blueprintAssignments/assignmentOperations | Nein |Nein |
> | blueprintAssignments/operations | Nein |Nein |
> | blueprints | Nein |Nein |
> | blueprints/artifacts | Nein |Nein |
> | blueprints/versions | Nein |Nein |
> | blueprints/versions/artifacts | Nein |Nein |

## <a name="microsoftbotservice"></a>Microsoft.BotService

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | botServices | Ja |Ja |
> | botServices/channels | Nein |Nein |
> | botServices/connections | Nein |Nein |
> | languages | Nein |Nein |
> | Vorlagen | Nein |Nein |

## <a name="microsoftcache"></a>Microsoft.Cache

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | Redis | Ja |Ja |
> | RedisConfigDefinition | Nein |Nein |

## <a name="microsoftcapacity"></a>Microsoft.Capacity

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | appliedReservations | Nein |Nein |
> | calculateExchange | Nein |Nein |
> | calculatePrice | Nein |Nein |
> | calculatePurchasePrice | Nein |Nein |
> | catalogs | Nein |Nein |
> | commercialReservationOrders | Nein |Nein |
> | Börse | Nein |Nein |
> | placePurchaseOrder | Nein |Nein |
> | reservationOrders | Nein |Nein |
> | reservationOrders/calculateRefund | Nein |Nein |
> | reservationOrders/merge | Nein |Nein |
> | reservationOrders/reservations | Nein |Nein |
> | reservationOrders/reservations/revisions | Nein |Nein |
> | reservationOrders/return | Nein |Nein |
> | reservationOrders/split | Nein |Nein |
> | reservationOrders/swap | Nein |Nein |
> | reservations | Nein |Nein |
> | ressourcen | Nein |Nein |
> | validateReservationOrder | Nein |Nein |

## <a name="microsoftcdn"></a>Microsoft.Cdn

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | CdnWebApplicationFirewallManagedRuleSets | Nein |Nein |
> | CdnWebApplicationFirewallPolicies | Ja |Ja |
> | edgenodes | Nein |Nein |
> | profiles | JA |Ja |
> | profiles/endpoints | Ja |Ja |
> | profiles/endpoints/customdomains | Nein |Nein |
> | profiles/endpoints/origins | Nein |Nein |
> | validateProbe | Nein |Nein |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | certificateOrders | Ja |Ja |
> | certificateOrders/certificates | Nein |Nein |
> | validateCertificateRegistrationInformation | Nein |Nein |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | capabilities | Nein |Nein |
> | domainNames | Nein |Nein |
> | domainNames/capabilities | Nein |Nein |
> | domainNames/internalLoadBalancers | Nein |Nein |
> | domainNames/serviceCertificates | Nein |Nein |
> | domainNames/slots | Nein |Nein |
> | domainNames/slots/roles | Nein |Nein |
> | domainNames/slots/roles/metricDefinitions | Nein |Nein |
> | domainNames/slots/roles/metrics | Nein |Nein |
> | moveSubscriptionResources | Nein |Nein |
> | operatingSystemFamilies | Nein |Nein |
> | operatingSystems | Nein |Nein |
> | quotas | Nein |Nein |
> | resourceTypes | Nein |Nein |
> | validateSubscriptionMoveAvailability | Nein |Nein |
> | virtualMachines | Nein |Nein |
> | virtualMachines/diagnosticSettings | Nein |Nein |
> | virtualMachines/metricDefinitions | Nein |Nein |
> | virtualMachines/metrics | Nein |Nein |

## <a name="microsoftclassicinfrastructuremigrate"></a>Microsoft.ClassicInfrastructureMigrate

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | classicInfrastructureResources | Nein |Nein |

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | capabilities | Nein |Nein |
> | expressRouteCrossConnections | Nein |Nein |
> | expressRouteCrossConnections/peerings | Nein |Nein |
> | gatewaySupportedDevices | Nein |Nein |
> | networkSecurityGroups | Nein |Nein |
> | quotas | Nein |Nein |
> | reservedIps | Nein |Nein |
> | virtualNetworks | Nein |Nein |
> | virtualNetworks/remoteVirtualNetworkPeeringProxies | Nein |Nein |
> | virtualNetworks/virtualNetworkPeerings | Nein |Nein |

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | capabilities | Nein |Nein |
> | disks | Nein |Nein |
> | images | Nein |Nein |
> | osImages | Nein |Nein |
> | osPlatformImages | Nein |Nein |
> | publicImages | Nein |Nein |
> | quotas | Nein |Nein |
> | storageAccounts | Nein |Nein |
> | storageAccounts/blobServices | Nein |Nein |
> | storageAccounts/fileServices | Nein |Nein |
> | storageAccounts/metricDefinitions | Nein |Nein |
> | storageAccounts/metrics | Nein |Nein |
> | storageAccounts/queueServices | Nein |Nein |
> | storageAccounts/services | Nein |Nein |
> | storageAccounts/services/diagnosticSettings | Nein |Nein |
> | storageAccounts/services/metricDefinitions | Nein |Nein |
> | storageAccounts/services/metrics | Nein |Nein |
> | storageAccounts/tableServices | Nein |Nein |
> | storageAccounts/vmImages | Nein |Nein |
> | vmImages | Nein |Nein |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | accounts | Ja |Ja |

## <a name="microsoftcommerce"></a>Microsoft.Commerce

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | RateCard | Nein |Nein |
> | UsageAggregates | Nein |Nein |

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | availabilitySets | Ja |Ja |
> | diskEncryptionSets | Ja |Ja |
> | disks | Ja |Ja |
> | galleries | Ja |Ja |
> | galleries/applications | Nein |Nein |
> | galleries/applications/versions | Nein |Nein |
> | galleries/images | Nein |Nein |
> | galleries/images/versions | Nein |Nein |
> | hostGroups | Ja |Ja |
> | hostGroups/hosts | Ja |Ja |
> | images | Ja |Ja |
> | proximityPlacementGroups | Ja |Ja |
> | restorePointCollections | Ja |Ja |
> | restorePointCollections/restorePoints | Nein |Nein |
> | sharedVMImages | Ja |Ja |
> | sharedVMImages/versions | Nein |Nein |
> | snapshots | Ja |Ja |
> | virtualMachines | Ja |Ja |
> | virtualMachines/extensions | Ja |Ja |
> | virtualMachines/metricDefinitions | Nein |Nein |
> | virtualMachineScaleSets | Ja |Ja |
> | virtualMachineScaleSets/extensions | Nein |Nein |
> | virtualMachineScaleSets/networkInterfaces | Nein |Nein |
> | virtualMachineScaleSets/publicIPAddresses | Nein |Nein |
> | virtualMachineScaleSets/virtualMachines | Nein |Nein |
> | virtualMachineScaleSets/virtualMachines/networkInterfaces | Nein |Nein |

## <a name="microsoftconsumption"></a>Microsoft.Consumption

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | AggregatedCost | Nein |Nein |
> | Bilanzen | Nein |Nein |
> | Budgets | Nein |Nein |
> | Charges | Nein |Nein |
> | CostTags | Nein |Nein |
> | credits | Nein |Nein |
> | events | Nein |Nein |
> | Vorhersagen | Nein |Nein |
> | lots | Nein |Nein |
> | Marketplaces | Nein |Nein |
> | Pricesheets | Nein |Nein |
> | products | Nein |Nein |
> | ReservationDetails | Nein |Nein |
> | ReservationRecommendations | Nein |Nein |
> | ReservationSummaries | Nein |Nein |
> | ReservationTransactions | Nein |Nein |
> | `Tags` | Nein |Nein |
> | tenants | Nein |Nein |
> | Begriffe | Nein |Nein |
> | UsageDetails | Nein |Nein |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | containerGroups | Ja |Ja |
> | serviceAssociationLinks | Nein |Nein |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | registries | Ja |Ja |
> | registries/builds | Nein |Nein |
> | registries/builds/cancel | Nein |Nein |
> | registries/builds/getLogLink | Nein |Nein |
> | registries/buildTasks | Ja |Ja |
> | registries/buildTasks/steps | Nein |Nein |
> | registries/eventGridFilters | Nein |Nein |
> | registries/getBuildSourceUploadUrl | Nein |Nein |
> | registries/GetCredentials | Nein |Nein |
> | registries/importImage | Nein |Nein |
> | registries/queueBuild | Nein |Nein |
> | registries/regenerateCredential | Nein |Nein |
> | registries/regenerateCredentials | Nein |Nein |
> | registries/replications | Ja |Ja |
> | registries/runs | Nein |Nein |
> | registries/runs/cancel | Nein |Nein |
> | registries/scheduleRun | Nein |Nein |
> | registries/tasks | Ja |Ja |
> | registries/updatePolicies | Nein |Nein |
> | registries/webhooks | Ja |Ja |
> | registries/webhooks/getCallbackConfig | Nein |Nein |
> | registries/webhooks/ping | Nein |Nein |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | containerServices | Ja |Ja |
> | managedClusters | Ja |Ja |
> | openShiftManagedClusters | Ja |Ja |

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | applications | JA |Ja |
> | updateCommunicationPreference | Nein |Nein |

## <a name="microsoftcortanaanalytics"></a>Microsoft.CortanaAnalytics

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | accounts | Ja |Ja |

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | Alerts | Nein |Nein |
> | BillingAccounts | Nein |Nein |
> | Budgets | Nein |Nein |
> | CloudConnectors | Nein |Nein |
> | Connectors | Ja |Ja |
> | Departments | Nein |Nein |
> | Dimensionen | Nein |Nein |
> | EnrollmentAccounts | Nein |Nein |
> | Exports | Nein |Nein |
> | ExternalBillingAccounts | Nein |Nein |
> | ExternalBillingAccounts/Alerts | Nein |Nein |
> | ExternalBillingAccounts/Dimensions | Nein |Nein |
> | ExternalBillingAccounts/Forecast | Nein |Nein |
> | ExternalBillingAccounts/Query | Nein |Nein |
> | ExternalSubscriptions | Nein |Nein |
> | ExternalSubscriptions/Alerts | Nein |Nein |
> | ExternalSubscriptions/Dimensions | Nein |Nein |
> | ExternalSubscriptions/Forecast | Nein |Nein |
> | ExternalSubscriptions/Query | Nein |Nein |
> | Forecast | Nein |Nein |
> | Abfragen | Nein |Nein |
> | Registrieren | Nein |Nein |
> | Reportconfigs | Nein |Nein |
> | Berichte | Nein |Nein |
> | Einstellungen | Nein |Nein |
> | showbackRules | Nein |Nein |
> | Ansichten | Nein |Nein |

## <a name="microsoftcustomerlockbox"></a>Microsoft.CustomerLockbox

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | requests | Nein |Nein |

## <a name="microsoftcustomproviders"></a>Microsoft.CustomProviders

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | associations | Nein |Nein |
> | resourceProviders | Ja |Ja |

## <a name="microsoftdatabox"></a>Microsoft.DataBox

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | jobs | Ja |Ja |

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | DataBoxEdgeDevices | Ja |Ja |

## <a name="microsoftdatabricks"></a>Microsoft.Databricks

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | workspaces | Ja |Nein |
> | workspaces/virtualNetworkPeerings | Nein |Nein |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | catalogs | Ja |Ja |
> | datacatalogs | Ja |Ja |
> | datacatalogs/datasources | Nein |Nein |
> | datacatalogs/datasources/scans | Nein |Nein |
> | datacatalogs/datasources/scans/datasets | Nein |Nein |
> | datacatalogs/datasources/scans/triggers | Nein |Nein |
> | datacatalogs/scantargets | Nein |Nein |
> | datacatalogs/scantargets/datasets | Nein |Nein |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | dataFactories | Ja |Nein |
> | dataFactories/diagnosticSettings | Nein |Nein |
> | dataFactories/metricDefinitions | Nein |Nein |
> | dataFactorySchema | Nein |Nein |
> | factories | Ja |Nein |
> | factories/integrationRuntimes | Nein |Nein |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | accounts | Ja |Ja |
> | accounts/dataLakeStoreAccounts | Nein |Nein |
> | accounts/storageAccounts | Nein |Nein |
> | accounts/storageAccounts/containers | Nein |Nein |
> | accounts/transferAnalyticsUnits | Nein |Nein |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | accounts | Ja |Ja |
> | accounts/eventGridFilters | Nein |Nein |
> | accounts/firewallRules | Nein |Nein |

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | services | Nein |Nein |
> | services/projects | Nein |Nein |

## <a name="microsoftdatashare"></a>Microsoft.DataShare

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | accounts | Ja |Ja |
> | accounts/shares | Nein |Nein |
> | accounts/shares/datasets | Nein |Nein |
> | accounts/shares/invitations | Nein |Nein |
> | accounts/shares/providersharesubscriptions | Nein |Nein |
> | accounts/shares/synchronizationSettings | Nein |Nein |
> | accounts/sharesubscriptions | Nein |Nein |
> | accounts/sharesubscriptions/consumerSourceDataSets | Nein |Nein |
> | accounts/sharesubscriptions/datasetmappings | Nein |Nein |
> | accounts/sharesubscriptions/triggers | Nein |Nein |

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | servers | Ja |Ja |
> | servers/advisors | Nein |Nein |
> | servers/queryTexts | Nein |Nein |
> | servers/recoverableServers | Nein |Nein |
> | servers/topQueryStatistics | Nein |Nein |
> | servers/virtualNetworkRules | Nein |Nein |
> | servers/waitStatistics | Nein |Nein |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | servers | Ja |Ja |
> | servers/advisors | Nein |Nein |
> | servers/queryTexts | Nein |Nein |
> | servers/recoverableServers | Nein |Nein |
> | servers/topQueryStatistics | Nein |Nein |
> | servers/virtualNetworkRules | Nein |Nein |
> | servers/waitStatistics | Nein |Nein |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | serverGroups | Ja |Ja |
> | servers | Ja |Ja |
> | servers/advisors | Nein |Nein |
> | servers/privateEndpointConnectionProxies | Nein |Nein |
> | servers/privateEndpointConnections | Nein |Nein |
> | servers/privateLinkResources | Nein |Nein |
> | servers/queryTexts | Nein |Nein |
> | servers/recoverableServers | Nein |Nein |
> | servers/topQueryStatistics | Nein |Nein |
> | servers/virtualNetworkRules | Nein |Nein |
> | servers/waitStatistics | Nein |Nein |
> | serversv2 | Ja |Ja |

## <a name="microsoftdeploymentmanager"></a>Microsoft.DeploymentManager

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | artifactSources | Ja |Ja |
> | rollouts | Ja |Ja |
> | serviceTopologies | Ja |Ja |
> | serviceTopologies/services | Ja |Ja |
> | serviceTopologies/services/serviceUnits | Ja |Ja |
> | steps | Ja |Ja |

## <a name="microsoftdesktopvirtualization"></a>Microsoft.DesktopVirtualization

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | applicationgroups | Ja |Ja |
> | applicationgroups/applications | Nein |Nein |
> | applicationgroups/startmenuitems | Nein |Nein |
> | hostpools | Ja |Ja |
> | hostpools/sessionhosts | Nein |Nein |
> | hostpools/sessionhosts/usersessions | Nein |Nein |
> | hostpools/usersessions | Nein |Nein |
> | workspaces | Ja |Ja |

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | ElasticPools | Ja |Ja |
> | ElasticPools/IotHubTenants | Ja |Ja |
> | IotHubs | Ja |Ja |
> | IotHubs/eventGridFilters | Nein |Nein |
> | ProvisioningServices | Ja |Ja |
> | usages | Nein |Nein |

## <a name="microsoftdevops"></a>Microsoft.DevOps

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | pipelines | Ja |Ja |

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | Controller | Ja |Ja |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | labcenters | Ja |Ja |
> | labs | Ja |Ja |
> | labs/environments | Ja |Ja |
> | labs/serviceRunners | Ja |Ja |
> | labs/virtualMachines | Ja |Ja |
> | schedules | Ja |Ja |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | databaseAccountNames | Nein |Nein |
> | databaseAccounts | Ja |Ja |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | domains | Ja |Ja |
> | domains/domainOwnershipIdentifiers | Nein |Nein |
> | generateSsoRequest | Nein |Nein |
> | topLevelDomains | Nein |Nein |
> | validateDomainRegistrationInformation | Nein |Nein |

## <a name="microsoftdynamicslcs"></a>Microsoft.DynamicsLcs

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | lcsprojects | Nein |Nein |
> | lcsprojects/clouddeployments | Nein |Nein |
> | lcsprojects/connectors | Nein |Nein |

## <a name="microsoftenterpriseknowledgegraph"></a>Microsoft.EnterpriseKnowledgeGraph

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | services | Ja |Ja |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | domains | Ja |Ja |
> | domains/topics | Nein |Nein |
> | eventSubscriptions | Nein |Nein |
> | extensionTopics | Nein |Nein |
> | topics | Ja |Ja |
> | topicTypes | Nein |Nein |

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | clusters | Ja |Ja |
> | namespaces | Ja |Ja |
> | namespaces/authorizationrules | Nein |Nein |
> | namespaces/disasterrecoveryconfigs | Nein |Nein |
> | namespaces/eventhubs | Nein |Nein |
> | namespaces/eventhubs/authorizationrules | Nein |Nein |
> | namespaces/eventhubs/consumergroups | Nein |Nein |
> | namespaces/networkrulesets | Nein |Nein |

## <a name="microsoftfeatures"></a>Microsoft.Features

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | Features | Nein |Nein |
> | providers | Nein |Nein |

## <a name="microsoftgallery"></a>Microsoft.Gallery

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | enroll | Nein |Nein |
> | galleryitems | Nein |Nein |
> | generateartifactaccessuri | Nein |Nein |
> | myareas | Nein |Nein |
> | myareas/areas | Nein |Nein |
> | myareas/areas/areas | Nein |Nein |
> | myareas/areas/areas/galleryitems | Nein |Nein |
> | myareas/areas/galleryitems | Nein |Nein |
> | myareas/galleryitems | Nein |Nein |
> | Registrieren | Nein |Nein |
> | ressourcen | Nein |Nein |
> | retrieveresourcesbyid | Nein |Nein |

## <a name="microsoftgenomics"></a>Microsoft.Genomics

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | accounts | Ja |Ja |

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | autoManagedVmConfigurationProfiles | Ja |Ja |
> | guestConfigurationAssignments | Nein |Nein |
> | software | Nein |Nein |
> | softwareUpdateProfile | Nein |Nein |
> | softwareUpdates | Nein |Nein |

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | hanaInstances | Ja |Ja |
> | sapMonitors | Ja |Ja |

## <a name="microsofthardwaresecuritymodules"></a>Microsoft.HardwareSecurityModules

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | dedicatedHSMs | Ja |Ja |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | clusters | Ja |Ja |
> | clusters/applications | Nein |Nein |

## <a name="microsofthealthcareapis"></a>Microsoft.HealthcareApis

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | services | Ja |Ja |

## <a name="microsofthybridcompute"></a>Microsoft.HybridCompute

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | machines | Ja |Ja |

## <a name="microsofthybriddata"></a>Microsoft.HybridData

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | dataManagers | Ja |Ja |

## <a name="microsofthydra"></a>Microsoft.Hydra

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | components | Ja |Ja |
> | networkScopes | Ja |Ja |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | jobs | Ja |Ja |

## <a name="microsoftintune"></a>Microsoft.Intune

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | diagnosticSettings | Nein |Nein |
> | diagnosticSettingsCategories | Nein |Nein |

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | appTemplates | Nein |Nein |
> | IoTApps | Ja |Ja |

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | Graph | Ja |Ja |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | deletedVaults | Nein |Nein |
> | hsmPools | Ja |Ja |
> | vaults | Ja |Ja |
> | vaults/accessPolicies | Nein |Nein |
> | vaults/eventGridFilters | Nein |Nein |
> | vaults/secrets | Nein |Nein |

## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | clusters | Ja |Ja |
> | clusters/attacheddatabaseconfigurations | Nein |Nein |
> | clusters/databases | Nein |Nein |
> | clusters/databases/dataconnections | Nein |Nein |
> | clusters/databases/eventhubconnections | Nein |Nein |
> | Cluster/sharedidentities | Nein |Nein |

## <a name="microsoftlabservices"></a>Microsoft.LabServices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | labaccounts | Ja |Ja |
> | users | Nein |Nein |

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | hostingEnvironments | Ja |Ja |
> | integrationAccounts | Ja |Ja |
> | integrationServiceEnvironments | Ja |Ja |
> | integrationServiceEnvironments/managedApis | Ja |Ja |
> | isolatedEnvironments | Ja |Ja |
> | workflows | Ja |Ja |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | commitmentPlans | Ja |Ja |
> | webServices | Ja |Ja |
> | Arbeitsbereiche | Ja |Ja |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | workspaces | Ja |Ja |
> | workspaces/computes | Nein |Nein |

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | Identities | Nein |Nein |
> | userAssignedIdentities | Ja |Ja |

## <a name="microsoftmanagedservices"></a>Microsoft.ManagedServices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | marketplaceRegistrationDefinitions | Nein |Nein |
> | registrationAssignments | Nein |Nein |
> | registrationDefinitions | Nein |Nein |

## <a name="microsoftmanagement"></a>Microsoft.Management

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | getEntities | Nein |Nein |
> | managementGroups | Nein |Nein |
> | ressourcen | Nein |Nein |
> | startTenantBackfill | Nein |Nein |
> | tenantBackfillStatus | Nein |Nein |

## <a name="microsoftmaps"></a>Microsoft.Maps

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | accounts | Ja |Ja |
> | accounts/eventGridFilters | Nein |Nein |

## <a name="microsoftmarketplace"></a>Microsoft.Marketplace

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | offers | Nein |Nein |
> | offerTypes | Nein |Nein |
> | offerTypes/publishers | Nein |Nein |
> | offerTypes/publishers/offers | Nein |Nein |
> | offerTypes/publishers/offers/plans | Nein |Nein |
> | offerTypes/publishers/offers/plans/agreements | Nein |Nein |
> | offerTypes/publishers/offers/plans/configs | Nein |Nein |
> | offerTypes/publishers/offers/plans/configs/importImage | Nein |Nein |
> | privategalleryitems | Nein |Nein |
> | products | Nein |Nein |
> | publishers | Nein |Nein |
> | publishers/offers | Nein |Nein |
> | publishers/offers/amendments | Nein |Nein |

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | classicDevServices | Ja |Ja |
> | updateCommunicationPreference | Nein |Nein |

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | agreements | Nein |Nein |
> | offertypes | Nein |Nein |

## <a name="microsoftmedia"></a>Microsoft.Media

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | mediaservices | Ja |Ja |
> | mediaservices/accountFilters | Nein |Nein |
> | mediaservices/assets | Nein |Nein |
> | mediaservices/assets/assetFilters | Nein |Nein |
> | mediaservices/contentKeyPolicies | Nein |Nein |
> | mediaservices/eventGridFilters | Nein |Nein |
> | mediaservices/liveEventOperations | Nein |Nein |
> | mediaservices/liveEvents | Ja |Ja |
> | mediaservices/liveEvents/liveOutputs | Nein |Nein |
> | mediaservices/liveOutputOperations | Nein |Nein |
> | mediaservices/streamingEndpointOperations | Nein |Nein |
> | mediaservices/streamingEndpoints | Ja |Ja |
> | mediaservices/streamingLocators | Nein |Nein |
> | mediaservices/streamingPolicies | Nein |Nein |
> | mediaservices/transforms | Nein |Nein |
> | mediaservices/transforms/jobs | Nein |Nein |

## <a name="microsoftmicroservices4spring"></a>Microsoft.Microservices4Spring

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | appClusters | Ja |Ja |

## <a name="microsoftmigrate"></a>Microsoft.Migrate

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | assessmentProjects | Ja |Ja |
> | migrateprojects | Ja |Ja |
> | projects | Ja |Ja |

## <a name="microsoftmixedreality"></a>Microsoft.MixedReality

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | holographicsBroadcastAccounts | Ja |Ja |
> | objectUnderstandingAccounts | Ja |Ja |
> | remoteRenderingAccounts | Ja |Ja |
> | spatialAnchorsAccounts | Ja |Ja |
> | surfaceReconstructionAccounts | Ja |Ja |

## <a name="microsoftnetapp"></a>Microsoft.NetApp

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | netAppAccounts | Ja |Ja |
> | netAppAccounts/backupPolicies | Ja |Ja |
> | netAppAccounts/capacityPools | Ja |Ja |
> | netAppAccounts/capacityPools/volumes | Ja |Ja |
> | netAppAccounts/capacityPools/volumes/backups | Nein |Nein |
> | netAppAccounts/capacityPools/volumes/mountTargets | Ja |Ja |
> | netAppAccounts/capacityPools/volumes/snapshots | Ja |Ja |
> | netAppAccounts/vaults | Nein |Nein |
## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | applicationGateways | Ja |Ja |
> | applicationGatewayWebApplicationFirewallPolicies | Ja |Ja |
> | applicationSecurityGroups | Ja |Ja |
> | azureFirewallFqdnTags | Nein |Nein |
> | azureFirewalls | Ja |Ja |
> | bastionHosts | Ja |Ja |
> | bgpServiceCommunities | Nein |Nein |
> | connections | Ja |Ja |
> | ddosCustomPolicies | Ja |Ja |
> | ddosProtectionPlans | Ja |Ja |
> | dnsOperationStatuses | Nein |Nein |
> | dnszones | Ja |Ja |
> | dnszones/A | Nein |Nein |
> | dnszones/AAAA | Nein |Nein |
> | dnszones/all | Nein |Nein |
> | dnszones/CAA | Nein |Nein |
> | dnszones/CNAME | Nein |Nein |
> | dnszones/MX | Nein |Nein |
> | dnszones/NS | Nein |Nein |
> | dnszones/PTR | Nein |Nein |
> | dnszones/recordsets | Nein |Nein |
> | dnszones/SOA | Nein |Nein |
> | dnszones/SRV | Nein |Nein |
> | dnszones/TXT | Nein |Nein |
> | expressRouteCircuits | Ja |Ja |
> | expressRouteCrossConnections | Ja |Ja |
> | expressRouteGateways | Ja |Ja |
> | expressRoutePorts | Ja |Ja |
> | expressRouteServiceProviders | Nein |Nein |
> | firewallPolicies | Ja |Ja |
> | frontdoors | Ja, aber nur eingeschränkt (siehe [Hinweis weiter unten](#frontdoor)) |Ja |
> | frontdoorWebApplicationFirewallManagedRuleSets | Ja, aber nur eingeschränkt (siehe [Hinweis weiter unten](#frontdoor)) |Nein |
> | frontdoorWebApplicationFirewallPolicies | Ja, aber nur eingeschränkt (siehe [Hinweis weiter unten](#frontdoor)) |Ja |
> | getDnsResourceReference | Nein |Nein |
> | internalNotify | Nein |Nein |
> | loadBalancers | Ja |Nein |
> | localNetworkGateways | Ja |Ja |
> | natGateways | Ja |Ja |
> | networkIntentPolicies | Ja |Ja |
> | networkInterfaces | Ja |Ja |
> | networkProfiles | Ja |Ja |
> | networkSecurityGroups | Ja |Ja |
> | networkWatchers | Ja |Nein |
> | networkWatchers/connectionMonitors | Ja |Nein |
> | networkWatchers/lenses | Ja |Nein |
> | networkWatchers/pingMeshes | Ja |Nein |
> | p2sVpnGateways | Ja |Ja |
> | privateDnsOperationStatuses | Nein |Nein |
> | privateDnsZones | Ja |Ja |
> | privateDnsZones/A | Nein |Nein |
> | privateDnsZones/AAAA | Nein |Nein |
> | privateDnsZones/all | Nein |Nein |
> | privateDnsZones/CNAME | Nein |Nein |
> | privateDnsZones/MX | Nein |Nein |
> | privateDnsZones/PTR | Nein |Nein |
> | privateDnsZones/SOA | Nein |Nein |
> | privateDnsZones/SRV | Nein |Nein |
> | privateDnsZones/TXT | Nein |Nein |
> | privateDnsZones/virtualNetworkLinks | Ja |Ja |
> | privateEndpoints | Ja |Ja |
> | privateLinkServices | Ja |Ja |
> | publicIPAddresses | Ja |Ja |
> | publicIPPrefixes | Ja |Ja |
> | routeFilters | Ja |Ja |
> | routeTables | Ja |Ja |
> | secureGateways | Ja |Ja |
> | serviceEndpointPolicies | Ja |Ja |
> | trafficManagerGeographicHierarchies | Nein |Nein |
> | trafficmanagerprofiles | Ja |Ja |
> | trafficmanagerprofiles/heatMaps | Nein |Nein |
> | trafficManagerUserMetricsKeys | Nein |Nein |
> | virtualHubs | Ja |Ja |
> | virtualNetworkGateways | Ja |Ja |
> | virtualNetworks | Ja |Ja |
> | virtualNetworkTaps | Ja |Ja |
> | virtualWans | Ja |Ja |
> | vpnGateways | Ja |Nein |
> | vpnSites | Ja |Ja |
> | webApplicationFirewallPolicies | Ja |Ja |

<a id="frontdoor" />

> [!NOTE]
> Für Azure Front Door Service können Sie beim Erstellen von Ressourcen zwar Tags anwenden, aber das Aktualisieren und Hinzufügen von Tags wird derzeit nicht unterstützt.


## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | namespaces | Ja |Nein |
> | namespaces/notificationHubs | Ja |Nein |

## <a name="microsoftobjectstore"></a>Microsoft.ObjectStore

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | osNamespaces | Ja |Ja |

## <a name="microsoftoffazure"></a>Microsoft.OffAzure

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | HyperVSites | Ja |Ja |
> | ImportSites | Ja |Ja |
> | ServerSites | Ja |Ja |
> | VMwareSites | Ja |Ja |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | clusters | Ja |Ja |
> | devices | Nein |Nein |
> | linkTargets | Nein |Nein |
> | storageInsightConfigs | Nein |Nein |
> | workspaces | Ja |Ja |
> | workspaces/dataSources | Nein |Nein |
> | workspaces/linkedServices | Nein |Nein |
> | workspaces/query | Nein |Nein |

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | managementassociations | Nein |Nein |
> | managementconfigurations | Ja |Ja |
> | solutions | Ja |JA |
> | views | JA |Ja |

## <a name="microsoftpeering"></a>Microsoft.Peering

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | legacyPeerings | Nein |Nein |
> | peerAsns | Nein |Nein |
> | peerings | Ja |Ja |
> | peeringServiceProviders | Nein |Nein |
> | peeringServices | Ja |Ja |

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | policyEvents | Nein |Nein |
> | policyStates | Nein |Nein |
> | policyTrackedResources | Nein |Nein |
> | remediations | Nein |Nein |

## <a name="microsoftportal"></a>Microsoft.Portal

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | consoles | Nein |Nein |
> | dashboards | Ja |Ja |
> | userSettings | Nein |Nein |

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | workspaceCollections | Ja |Ja |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | capacities | Ja |Ja |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | backupProtectedItems | Nein |Nein |
> | vaults | Ja |Ja |

## <a name="microsoftrelay"></a>Microsoft.Relay

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | namespaces | Ja |Ja |
> | namespaces/authorizationrules | Nein |Nein |
> | namespaces/hybridconnections | Nein |Nein |
> | namespaces/hybridconnections/authorizationrules | Nein |Nein |
> | namespaces/wcfrelays | Nein |Nein |
> | namespaces/wcfrelays/authorizationrules | Nein |Nein |

## <a name="microsoftremoteapp"></a>Microsoft.RemoteApp

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | accounts | Nein |Nein |
> | collections | Ja |Ja |
> | collections/applications | Nein |Nein |
> | collections/securityprincipals | Nein |Nein |
> | templateImages | Nein |Nein |

## <a name="microsoftresourcegraph"></a>Microsoft.ResourceGraph

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | Abfragen | Ja |Ja |
> | resourceChangeDetails | Nein |Nein |
> | resourceChanges | Nein |Nein |
> | ressourcen | Nein |Nein |
> | resourcesHistory | Nein |Nein |
> | subscriptionsStatus | Nein |Nein |

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | availabilityStatuses | Nein |Nein |
> | childAvailabilityStatuses | Nein |Nein |
> | childResources | Nein |Nein |
> | events | Nein |Nein |
> | impactedResources | Nein |Nein |
> | metadata | Nein |Nein |
> | Benachrichtigungen | Nein |Nein |

## <a name="microsoftresources"></a>Microsoft.Resources

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | deployments | Ja |Nein |
> | deployments/operations | Nein |Nein |
> | links | Nein |Nein |
> | notifyResourceJobs | Nein |Nein |
> | providers | Nein |Nein |
> | resourceGroups | Ja |Nein |
> | ressourcen | Nein |Nein |
> | subscriptions | Nein |Nein |
> | subscriptions/providers | Nein |Nein |
> | subscriptions/resourceGroups | Nein |Nein |
> | subscriptions/resourcegroups/resources | Nein |Nein |
> | subscriptions/resources | Nein |Nein |
> | subscriptions/tagnames | Nein |Nein |
> | subscriptions/tagNames/tagValues | Nein |Nein |
> | tenants | Nein |Nein |

## <a name="microsoftsaas"></a>Microsoft.SaaS

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | applications | JA |Ja |
> | saasresources | Nein |Nein |

## <a name="microsoftscheduler"></a>Microsoft.Scheduler

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | jobcollections | Ja |Ja |

## <a name="microsoftsearch"></a>Microsoft.Search

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | resourceHealthMetadata | Nein |Nein |
> | searchServices | Ja |Ja |

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | adaptiveNetworkHardenings | Nein |Nein |
> | advancedThreatProtectionSettings | Nein |Nein |
> | alerts | Nein |Nein |
> | allowedConnections | Nein |Nein |
> | applicationWhitelistings | Nein |Nein |
> | assessmentMetadata | Nein |Nein |
> | assessments | Nein |Nein |
> | AutoProvisioningSettings | Nein |Nein |
> | Compliances | Nein |Nein |
> | dataCollectionAgents | Nein |Nein |
> | deviceSecurityGroups | Nein |Nein |
> | discoveredSecuritySolutions | Nein |Nein |
> | externalSecuritySolutions | Nein |Nein |
> | InformationProtectionPolicies | Nein |Nein |
> | iotSecuritySolutions | Ja |Ja |
> | iotSecuritySolutions/analyticsModels | Nein |Nein |
> | iotSecuritySolutions/analyticsModels/aggregatedAlerts | Nein |Nein |
> | iotSecuritySolutions/analyticsModels/aggregatedRecommendations | Nein |Nein |
> | jitNetworkAccessPolicies | Nein |Nein |
> | networkData | Nein |Nein |
> | playbookConfigurations | Ja |Ja |
> | Richtlinien | Nein |Nein |
> | pricings | Nein |Nein |
> | regulatoryComplianceStandards | Nein |Nein |
> | regulatoryComplianceStandards/regulatoryComplianceControls | Nein |Nein |
> | regulatoryComplianceStandards/regulatoryComplianceControls/regulatoryComplianceAssessments | Nein |Nein |
> | securityContacts | Nein |Nein |
> | securitySolutions | Nein |Nein |
> | securitySolutionsReferenceData | Nein |Nein |
> | securityStatuses | Nein |Nein |
> | securityStatusesSummaries | Nein |Nein |
> | serverVulnerabilityAssessments | Nein |Nein |
> | settings | Nein |Nein |
> | subAssessments | Nein |Nein |
> | Tasks | Nein |Nein |
> | topologies | Nein |Nein |
> | workspaceSettings | Nein |Nein |

## <a name="microsoftsecuritygraph"></a>Microsoft.SecurityGraph

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | diagnosticSettings | Nein |Nein |
> | diagnosticSettingsCategories | Nein |Nein |

## <a name="microsoftsecurityinsights"></a>Microsoft.SecurityInsights

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | aggregations | Nein |Nein |
> | alertRules | Nein |Nein |
> | alertRuleTemplates | Nein |Nein |
> | bookmarks | Nein |Nein |
> | cases | Nein |Nein |
> | dataConnectors | Nein |Nein |
> | entities | Nein |Nein |
> | entityQueries | Nein |Nein |
> | officeConsents | Nein |Nein |
> | settings | Nein |Nein |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | namespaces | Ja |Nein |
> | namespaces/authorizationrules | Nein |Nein |
> | namespaces/disasterrecoveryconfigs | Nein |Nein |
> | namespaces/eventgridfilters | Nein |Nein |
> | namespaces/networkrulesets | Nein |Nein |
> | namespaces/queues | Nein |Nein |
> | namespaces/queues/authorizationrules | Nein |Nein |
> | namespaces/topics | Nein |Nein |
> | namespaces/topics/authorizationrules | Nein |Nein |
> | namespaces/topics/subscriptions | Nein |Nein |
> | namespaces/topics/subscriptions/rules | Nein |Nein |
> | premiumMessagingRegions | Nein |Nein |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | applications | JA |Ja |
> | clusters | Ja |Ja |
> | clusters/applications | Nein |Nein |
> | containerGroups | Ja |Ja |
> | containerGroupSets | Ja |Ja |
> | edgeclusters | Ja |Ja |
> | edgeclusters/applications | Nein |Nein |
> | networks | Ja |Ja |
> | secretstores | Ja |Ja |
> | secretstores/certificates | Nein |Nein |
> | secretstores/secrets | Nein |Nein |
> | volumes | Ja |Ja |

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | applications | JA |Ja |
> | containerGroups | Ja |Ja |
> | gateways | Ja |JA |
> | networks | Ja |Ja |
> | secrets | Ja |Ja |
> | volumes | Ja |Ja |

## <a name="microsoftservices"></a>Microsoft.Services

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | providerRegistrations | Nein |Nein |
> | providerRegistrations/resourceTypeRegistrations | Nein |Nein |
> | rollouts | Ja |Ja |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | SignalR | Ja |Ja |
> | SignalR/eventGridFilters | Nein |Nein |

## <a name="microsoftsiterecovery"></a>Microsoft.SiteRecovery

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | SiteRecoveryVault | Ja |Ja |

## <a name="microsoftsoftwareplan"></a>Microsoft.SoftwarePlan

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | hybridUseBenefits | Nein |Nein |

## <a name="microsoftsolutions"></a>Microsoft.Solutions

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | applicationDefinitions | Ja |Ja |
> | applications | JA |Ja |
> | jitRequests | Ja |Ja |

## <a name="microsoftsql"></a>Microsoft.SQL

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | managedInstances | Ja | Ja |
> | managedInstances/databases | Ja (siehe [Hinweis unten](#sqlnote)) | Ja |
> | managedInstances/databases/backupShortTermRetentionPolicies | Nein | Nein |
> | managedInstances/databases/schemas/tables/columns/sensitivityLabels | Nein | Nein |
> | managedInstances/databases/vulnerabilityAssessments | Nein | Nein |
> | managedInstances/databases/vulnerabilityAssessments/rules/baselines | Nein | Nein |
> | managedInstances/encryptionProtector | Nein | Nein |
> | managedInstances/keys | Nein | Nein |
> | managedInstances/restorableDroppedDatabases/backupShortTermRetentionPolicies | Nein | Nein |
> | managedInstances/vulnerabilityAssessments | Nein | Nein |
> | servers | Ja | Ja |
> | servers/administrators | Nein | Nein |
> | servers/communicationLinks | Nein | Nein |
> | servers/databases | Ja (siehe [Hinweis unten](#sqlnote)) | Ja |
> | servers/encryptionProtector | Nein | Nein |
> | servers/firewallRules | Nein | Nein |
> | servers/keys | Nein | Nein |
> | servers/restorableDroppedDatabases | Nein | Nein |
> | servers/serviceobjectives | Nein | Nein |
> | servers/tdeCertificates | Nein | Nein |
> | virtualClusters | Nein | Nein |

<a id="sqlnote" />

> [!NOTE]
> Die Masterdatenbank unterstützt keine Tags, diese werden jedoch in anderen Datenbanken, z.B. Azure SQL Data Warehouse-Datenbanken, unterstützt. Azure SQL Data Warehouse-Datenbanken müssen sich im aktiven (nicht angehaltenen) Zustand befinden.

## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | SqlVirtualMachineGroups | Ja |Ja |
> | SqlVirtualMachineGroups/AvailabilityGroupListeners | Nein |Nein |
> | SqlVirtualMachines | Ja |Ja |

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | storageAccounts | Ja |Ja |
> | storageAccounts/blobServices | Nein |Nein |
> | storageAccounts/fileServices | Nein |Nein |
> | storageAccounts/queueServices | Nein |Nein |
> | storageAccounts/services | Nein |Nein |
> | storageAccounts/services/metricDefinitions | Nein |Nein |
> | storageAccounts/tableServices | Nein |Nein |
> | usages | Nein |Nein |

## <a name="microsoftstoragecache"></a>Microsoft.StorageCache

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | caches | Ja |Ja |
> | caches/storageTargets | Nein |Nein |
> | usageModels | Nein |Nein |

## <a name="microsoftstoragereplication"></a>Microsoft.StorageReplication

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | replicationGroups | Nein |Nein |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | storageSyncServices | Ja |Ja |
> | storageSyncServices/registeredServers | Nein |Nein |
> | storageSyncServices/syncGroups | Nein |Nein |
> | storageSyncServices/syncGroups/cloudEndpoints | Nein |Nein |
> | storageSyncServices/syncGroups/serverEndpoints | Nein |Nein |
> | storageSyncServices/workflows | Nein |Nein |

## <a name="microsoftstoragesyncdev"></a>Microsoft.StorageSyncDev

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | storageSyncServices | Ja |Ja |
> | storageSyncServices/registeredServers | Nein |Nein |
> | storageSyncServices/syncGroups | Nein |Nein |
> | storageSyncServices/syncGroups/cloudEndpoints | Nein |Nein |
> | storageSyncServices/syncGroups/serverEndpoints | Nein |Nein |
> | storageSyncServices/workflows | Nein |Nein |

## <a name="microsoftstoragesyncint"></a>Microsoft.StorageSyncInt

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | storageSyncServices | Ja |Ja |
> | storageSyncServices/registeredServers | Nein |Nein |
> | storageSyncServices/syncGroups | Nein |Nein |
> | storageSyncServices/syncGroups/cloudEndpoints | Nein |Nein |
> | storageSyncServices/syncGroups/serverEndpoints | Nein |Nein |
> | storageSyncServices/workflows | Nein |Nein |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | managers | Ja |Ja |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | streamingjobs | Ja (siehe Hinweis unten) |Ja |

> [!NOTE]
> Sie können kein Tag hinzufügen, wenn Streamingaufträge ausgeführt werden. Beenden Sie die Ressource, um ein Tag hinzuzufügen.

## <a name="microsoftsubscription"></a>Microsoft.Subscription

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | cancel | Nein |Nein |
> | CreateSubscription | Nein |Nein |
> | enable | Nein |Nein |
> | rename | Nein |Nein |
> | SubscriptionDefinitions | Nein |Nein |
> | SubscriptionOperations | Nein |Nein |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | environments | Ja |Nein |
> | environments/accessPolicies | Nein |Nein |
> | environments/eventsources | Ja |Nein |
> | environments/referenceDataSets | Ja |Nein |

## <a name="microsoftvmwarecloudsimple"></a>Microsoft.VMwareCloudSimple

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | dedicatedCloudNodes | Ja |Ja |
> | dedicatedCloudServices | Ja |Ja |
> | virtualMachines | Ja |Ja |

## <a name="microsoftweb"></a>Microsoft.Web

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | apiManagementAccounts | Nein |Nein |
> | apiManagementAccounts/apiAcls | Nein |Nein |
> | apiManagementAccounts/apis | Nein |Nein |
> | apiManagementAccounts/apis/apiAcls | Nein |Nein |
> | apiManagementAccounts/apis/connectionAcls | Nein |Nein |
> | apiManagementAccounts/apis/connections | Nein |Nein |
> | apiManagementAccounts/apis/connections/connectionAcls | Nein |Nein |
> | apiManagementAccounts/apis/localizedDefinitions | Nein |Nein |
> | apiManagementAccounts/connectionAcls | Nein |Nein |
> | apiManagementAccounts/connections | Nein |Nein |
> | billingMeters | Nein |Nein |
> | certificates | Ja |Ja |
> | connectionGateways | Ja |Ja |
> | connections | Ja |Ja |
> | customApis | Ja |Ja |
> | deletedSites | Nein |Nein |
> | functions | Nein |Nein |
> | hostingEnvironments | Ja |Ja |
> | hostingEnvironments/multiRolePools | Nein |Nein |
> | hostingEnvironments/workerPools | Nein |Nein |
> | publishingUsers | Nein |Nein |
> | empfehlungen | Nein |Nein |
> | resourceHealthMetadata | Nein |Nein |
> | runtimes | Nein |Nein |
> | serverFarms | Ja |Ja |
> | serverFarms/eventGridFilters | Nein |Nein |
> | sites | Ja |Ja |
> | sites/config  | Nein | Nein |
> | sites/eventGridFilters | Nein |Nein |
> | sites/hostNameBindings | Nein |Nein |
> | sites/networkConfig | Nein |Nein |
> | sites/premieraddons | Ja |Ja |
> | sites/slots | Ja |Ja |
> | sites/slots/eventGridFilters | Nein |Nein |
> | sites/slots/hostNameBindings | Nein |Nein |
> | sites/slots/networkConfig | Nein |Nein |
> | sourceControls | Nein |Nein |
> | validate | Nein |Nein |
> | verifyHostingEnvironmentVnet | Nein |Nein |

## <a name="microsoftwindowsdefenderatp"></a>Microsoft.WindowsDefenderATP

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | diagnosticSettings | Nein |Nein |
> | diagnosticSettingsCategories | Nein |Nein |

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | DeviceServices | Ja |Ja |

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor

> [!div class="mx-tableFixed"]
> | Ressourcentyp | Tagunterstützung | Tag in Kostenbericht |
> | ------------- | ----------- | ----------- |
> | components | Nein |Nein |
> | componentsSummary | Nein |Nein |
> | monitorInstances | Nein |Nein |
> | monitorInstancesSummary | Nein |Nein |
> | monitors | Nein |Nein |
> | notificationSettings | Nein |Nein |

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Anwenden von Tags auf Ressourcen finden Sie unter [Verwenden von Tags zum Organisieren von Azure-Ressourcen](resource-group-using-tags.md).
