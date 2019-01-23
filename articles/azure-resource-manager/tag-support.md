---
title: 'Azure Resource Manager: Tagunterstützung für Ressourcen'
description: Zeigt, welche Azure-Ressourcentypen Tags unterstützen. Enthält Details für alle Azure-Dienste.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 1/11/2019
ms.author: tomfitz
ms.openlocfilehash: 4899409b225298c521935f2eeaf5e8e59a6833b3
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/14/2019
ms.locfileid: "54267452"
---
# <a name="tag-support-for-azure-resources"></a>Tagunterstützung für Azure-Ressourcen
In diesem Artikel erfahren Sie, ob ein Ressourcentyp [Tags](resource-group-using-tags.md) unterstützt.

## <a name="microsoftaad"></a>Microsoft.AAD
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| DomainServices | JA | 
| DomainServices/oucontainer | Nein  | 

## <a name="microsoftaadiam"></a>microsoft.aadiam
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| diagnosticSettings | Nein  | 
| diagnosticSettingsCategories | Nein  | 

## <a name="microsoftaddons"></a>Microsoft.Addons
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| supportProviders | Nein  | 

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| aadsupportcases | Nein  | 
| addsservices | Nein  | 
| agents | Nein  | 
| anonymousapiusers | Nein  | 
| Konfiguration | Nein  | 
| Protokolle | Nein  | 
| reports | Nein  | 
| services | Nein  | 

## <a name="microsoftadvisor"></a>Microsoft.Advisor
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| Konfigurationen | Nein  | 
| generateRecommendations | Nein  | 
| empfehlungen | Nein  | 
| suppressions | Nein  | 

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| actionRules | Nein  | 
| alerts | Nein  | 
| alertsList | Nein  | 
| alertsSummary | Nein  | 
| alertsSummaryList | Nein  | 
| smartDetectorAlertRules | Nein  | 
| smartDetectorRuntimeEnvironments | Nein  | 
| smartGroups | Nein  | 

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| servers | JA | 

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| reportFeedback | Nein  | 
| service | JA | 
| validateServiceName | Nein  | 

## <a name="microsoftattestation"></a>Microsoft.Attestation
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| attestationProviders | Nein  | 

## <a name="microsoftauthorization"></a>Microsoft.Authorization
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| classicAdministrators | Nein  | 
| denyAssignments | Nein  | 
| elevateAccess | Nein  | 
| locks | Nein  | 
| Berechtigungen | Nein  | 
| policyAssignments | Nein  | 
| policyDefinitions | Nein  | 
| policySetDefinitions | Nein  | 
| providerOperations | Nein  | 
| roleAssignments | Nein  | 
| roleDefinitions | Nein  | 

## <a name="microsoftautomation"></a>Microsoft.Automation
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| automationAccounts | JA | 
| automationAccounts/configurations | JA | 
| automationAccounts/jobs | Nein  | 
| automationAccounts/runbooks | JA | 
| automationAccounts/softwareUpdateConfigurations | Nein  | 
| automationAccounts/webhooks | Nein  | 

## <a name="microsoftazuregeneva"></a>Microsoft.Azure.Geneva
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| environments | Nein  | 
| environments/accounts | Nein  | 
| environments/accounts/namespaces | Nein  | 
| environments/accounts/namespaces/configurations | Nein  | 

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| b2cDirectories | JA | 

## <a name="microsoftazurestack"></a>Microsoft.AzureStack
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| registrations | JA | 
| registrations/customerSubscriptions | Nein  | 
| registrations/products | Nein  | 

## <a name="microsoftbatch"></a>Microsoft.Batch
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| batchAccounts | JA | 

## <a name="microsoftbilling"></a>Microsoft.Billing
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| billingAccounts | Nein  | 
| billingAccounts/billingProfiles | Nein  | 
| billingAccounts/billingProfiles/billingSubscriptions | Nein  | 
| billingAccounts/billingProfiles/invoices | Nein  | 
| billingAccounts/billingProfiles/invoices/pricesheet | Nein  | 
| billingAccounts/billingProfiles/operationStatus | Nein  | 
| billingAccounts/billingProfiles/paymentMethods | Nein  | 
| billingAccounts/billingProfiles/policies | Nein  | 
| billingAccounts/billingProfiles/pricesheet | Nein  | 
| billingAccounts/billingProfiles/products | Nein  | 
| billingAccounts/billingProfiles/transactions | Nein  | 
| billingAccounts/billingSubscriptions | Nein  | 
| billingAccounts/departments | Nein  | 
| billingAccounts/eligibleOffers | Nein  | 
| billingAccounts/enrollmentAccounts | Nein  | 
| billingAccounts/invoices | Nein  | 
| billingAccounts/invoiceSections | Nein  | 
| billingAccounts/invoiceSections/billingSubscriptions | Nein  | 
| billingAccounts/invoiceSections/billingSubscriptions/transfer | Nein  | 
| billingAccounts/invoiceSections/importRequests | Nein  | 
| billingAccounts/invoiceSections/initiateImportRequest | Nein  | 
| billingAccounts/invoiceSections/initiateTransfer | Nein  | 
| billingAccounts/invoiceSections/operationStatus | Nein  | 
| billingAccounts/invoiceSections/products | Nein  | 
| billingAccounts/invoiceSections/transfers | Nein  | 
| billingAccounts/products | Nein  | 
| billingAccounts/projects | Nein  | 
| billingAccounts/projects/billingSubscriptions | Nein  | 
| billingAccounts/projects/importRequests | Nein  | 
| billingAccounts/projects/initiateImportRequest | Nein  | 
| billingAccounts/projects/operationStatus | Nein  | 
| billingAccounts/projects/products | Nein  | 
| billingAccounts/transactions | Nein  | 
| billingPeriods | Nein  | 
| BillingPermissions | Nein  | 
| billingProperty | Nein  | 
| BillingRoleAssignments | Nein  | 
| BillingRoleDefinitions | Nein  | 
| CreateBillingRoleAssignment | Nein  | 
| departments | Nein  | 
| enrollmentAccounts | Nein  | 
| importRequests | Nein  | 
| importRequests/acceptImportRequest | Nein  | 
| importRequests/declineImportRequest | Nein  | 
| invoices | Nein  | 
| transfers | Nein  | 
| transfers/acceptTransfer | Nein  | 
| transfers/declineTransfer | Nein  | 
| transfers/operationStatus | Nein  | 
| usagePlans | Nein  | 

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| mapApis | JA | 
| updateCommunicationPreference | Nein  | 

## <a name="microsoftbiztalkservices"></a>Microsoft.BizTalkServices
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| BizTalk | JA | 

## <a name="microsoftblueprint"></a>Microsoft.Blueprint
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| blueprintAssignments | Nein  | 
| blueprintAssignments/assignmentOperations | Nein  | 
| blueprintAssignments/operations | Nein  | 
| blueprints | Nein  | 
| blueprints/artifacts | Nein  | 
| blueprints/versions | Nein  | 
| blueprints/versions/artifacts | Nein  | 

## <a name="microsoftbotservice"></a>Microsoft.BotService
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| botServices | JA | 
| botServices/channels | Nein  | 
| botServices/connections | Nein  | 

## <a name="microsoftcache"></a>Microsoft.Cache
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| Redis | JA | 
| RedisConfigDefinition | Nein  | 

## <a name="microsoftcapacity"></a>Microsoft.Capacity
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| appliedReservations | Nein  | 
| calculatePrice | Nein  | 
| catalogs | Nein  | 
| commercialReservationOrders | Nein  | 
| reservationOrders | Nein  | 
| reservationOrders/calculateRefund | Nein  | 
| reservationOrders/merge | Nein  | 
| reservationOrders/reservations | Nein  | 
| reservationOrders/reservations/revisions | Nein  | 
| reservationOrders/return | Nein  | 
| reservationOrders/split | Nein  | 
| reservationOrders/swap | Nein  | 
| reservations | Nein  | 
| ressourcen | Nein  | 
| validateReservationOrder | Nein  | 

## <a name="microsoftcdn"></a>Microsoft.Cdn
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| edgenodes | Nein  | 
| Profile | JA | 
| profiles/endpoints | JA | 
| profiles/endpoints/customdomains | Nein  | 
| profiles/endpoints/origins | Nein  | 
| validateProbe | Nein  | 

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| certificateOrders | JA | 
| certificateOrders/certificates | Nein  | 
| validateCertificateRegistrationInformation | Nein  | 

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| capabilities | Nein  | 
| domainNames | Nein  | 
| domainNames/capabilities | Nein  | 
| domainNames/internalLoadBalancers | Nein  | 
| domainNames/serviceCertificates | Nein  | 
| domainNames/slots | Nein  | 
| domainNames/slots/roles | Nein  | 
| moveSubscriptionResources | Nein  | 
| operatingSystemFamilies | Nein  | 
| operatingSystems | Nein  | 
| quotas | Nein  | 
| resourceTypes | Nein  | 
| validateSubscriptionMoveAvailability | Nein  | 
| virtualMachines | Nein  | 
| virtualMachines/diagnosticSettings | Nein  | 

## <a name="microsoftclassicinfrastructuremigrate"></a>Microsoft.ClassicInfrastructureMigrate
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| classicInfrastructureResources | Nein  | 

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| capabilities | Nein  | 
| expressRouteCrossConnections | Nein  | 
| expressRouteCrossConnections/peerings | Nein  | 
| gatewaySupportedDevices | Nein  | 
| networkSecurityGroups | Nein  | 
| quotas | Nein  | 
| reservedIps | Nein  | 
| virtualNetworks | Nein  | 
| virtualNetworks/remoteVirtualNetworkPeeringProxies | Nein  | 
| virtualNetworks/virtualNetworkPeerings | Nein  | 

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| capabilities | Nein  | 
| disks | Nein  | 
| images | Nein  | 
| osImages | Nein  | 
| osPlatformImages | Nein  | 
| publicImages | Nein  | 
| quotas | Nein  | 
| storageAccounts | Nein  | 
| storageAccounts/services | Nein  | 
| storageAccounts/services/diagnosticSettings | Nein  | 
| storageAccounts/vmImages | Nein  | 
| vmImages | Nein  | 

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| accounts | JA | 

## <a name="microsoftcommerce"></a>Microsoft.Commerce
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| RateCard | Nein  | 
| UsageAggregates | Nein  | 

## <a name="microsoftcompute"></a>Microsoft.Compute
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| availabilitySets | JA | 
| disks | JA | 
| images | JA | 
| restorePointCollections | JA | 
| restorePointCollections/restorePoints | Nein  | 
| sharedVMImages | JA | 
| sharedVMImages/versions | JA | 
| snapshots | JA | 
| virtualMachines | JA | 
| virtualMachines/diagnosticSettings | Nein  | 
| virtualMachines/extensions | JA | 
| virtualMachineScaleSets | JA | 
| virtualMachineScaleSets/extensions | Nein  | 
| virtualMachineScaleSets/networkInterfaces | Nein  | 
| virtualMachineScaleSets/publicIPAddresses | Nein  | 
| virtualMachineScaleSets/virtualMachines | Nein  | 
| virtualMachineScaleSets/virtualMachines/networkInterfaces | Nein  | 

## <a name="microsoftconsumption"></a>Microsoft.Consumption
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| AggregatedCost | Nein  | 
| Bilanzen | Nein  | 
| Budgets | Nein  | 
| Charges | Nein  | 
| CostTags | Nein  | 
| credits | Nein  | 
| events | Nein  | 
| Vorhersagen | Nein  | 
| lots | Nein  | 
| Marketplaces | Nein  | 
| Pricesheets | Nein  | 
| products | Nein  | 
| ReservationDetails | Nein  | 
| ReservationRecommendations | Nein  | 
| ReservationSummaries | Nein  | 
| ReservationTransactions | Nein  | 
| Tags | Nein  | 
| Begriffe | Nein  | 
| UsageDetails | Nein  | 

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| containerGroups | JA | 
| serviceAssociationLinks | Nein  | 

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| registries | JA | 
| registries/builds | Nein  | 
| registries/builds/cancel | Nein  | 
| registries/builds/getLogLink | Nein  | 
| registries/buildTasks | JA | 
| registries/buildTasks/steps | Nein  | 
| registries/eventGridFilters | Nein  | 
| registries/getBuildSourceUploadUrl | Nein  | 
| registries/GetCredentials | Nein  | 
| registries/importImage | Nein  | 
| registries/queueBuild | Nein  | 
| registries/regenerateCredential | Nein  | 
| registries/regenerateCredentials | Nein  | 
| registries/replications | JA | 
| registries/runs | Nein  | 
| registries/runs/cancel | Nein  | 
| registries/scheduleRun | Nein  | 
| registries/tasks | JA | 
| registries/updatePolicies | Nein  | 
| registries/webhooks | JA | 
| registries/webhooks/getCallbackConfig | Nein  | 
| registries/webhooks/ping | Nein  | 

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| containerServices | JA | 
| managedClusters | JA | 

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| Anwendungen | JA | 
| updateCommunicationPreference | Nein  | 

## <a name="microsoftcortanaanalytics"></a>Microsoft.CortanaAnalytics
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| accounts | JA | 

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| Alerts | Nein  | 
| BillingAccounts | Nein  | 
| Connectors | JA | 
| Departments | Nein  | 
| Dimensionen | Nein  | 
| EnrollmentAccounts | Nein  | 
| Abfragen | Nein  | 
| Registrieren | Nein  | 
| Reportconfigs | Nein  | 
| Berichte | Nein  | 

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| hubs | JA | 
| hubs/authorizationPolicies | Nein  | 
| hubs/connectors | Nein  | 
| hubs/connectors/mappings | Nein  | 
| hubs/interactions | Nein  | 
| hubs/kpi | Nein  | 
| hubs/links | Nein  | 
| hubs/profiles | Nein  | 
| hubs/roleAssignments | Nein  | 
| hubs/roles | Nein  | 
| hubs/suggestTypeSchema | Nein  | 
| hubs/views | Nein  | 
| hubs/widgetTypes | Nein  | 

## <a name="microsoftdatabox"></a>Microsoft.DataBox
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| jobs | JA | 

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| DataBoxEdgeDevices | JA | 

## <a name="microsoftdatabricks"></a>Microsoft.Databricks
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| workspaces | JA | 
| workspaces/virtualNetworkPeerings | Nein  | 

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| catalogs | JA | 

## <a name="microsoftdataconnect"></a>Microsoft.DataConnect
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| connectionManagers | JA | 

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| dataFactories | JA | 
| dataFactories/diagnosticSettings | Nein  | 
| dataFactorySchema | Nein  | 
| factories | JA | 
| factories/integrationRuntimes | Nein  | 

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| accounts | JA | 
| accounts/dataLakeStoreAccounts | Nein  | 
| accounts/storageAccounts | Nein  | 
| accounts/storageAccounts/containers | Nein  | 

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| accounts | JA | 
| accounts/eventGridFilters | Nein  | 
| accounts/firewallRules | Nein  | 

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| services | JA | 
| services/projects | JA | 

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| servers | JA | 
| servers/recoverableServers | Nein  | 
| servers/virtualNetworkRules | Nein  | 

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| servers | JA | 
| servers/recoverableServers | Nein  | 
| servers/virtualNetworkRules | Nein  | 

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| servers | JA | 
| servers/advisors | Nein  | 
| servers/queryTexts | Nein  | 
| servers/recoverableServers | Nein  | 
| servers/topQueryStatistics | Nein  | 
| servers/virtualNetworkRules | Nein  | 
| servers/waitStatistics | Nein  | 

## <a name="microsoftdevices"></a>Microsoft.Devices
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| IotHubs | JA | 
| IotHubs/eventGridFilters | Nein  | 
| ProvisioningServices | JA | 
| usages | Nein  | 

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| Controller | JA | 

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| labs | JA | 
| labs/serviceRunners | JA | 
| labs/virtualMachines | JA | 
| schedules | JA | 

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| databaseAccountNames | Nein  | 
| databaseAccounts | JA | 

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| domains | JA | 
| domains/domainOwnershipIdentifiers | Nein  | 
| generateSsoRequest | Nein  | 
| topLevelDomains | Nein  | 
| validateDomainRegistrationInformation | Nein  | 

## <a name="microsoftdynamicslcs"></a>Microsoft.DynamicsLcs
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| lcsprojects | Nein  | 
| lcsprojects/clouddeployments | Nein  | 
| lcsprojects/connectors | Nein  | 

## <a name="microsofteventgrid"></a>Microsoft.EventGrid
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| domains | JA | 
| domains/topics | Nein  | 
| eventSubscriptions | Nein  | 
| extensionTopics | Nein  | 
| topics | JA | 
| topicTypes | Nein  | 

## <a name="microsofteventhub"></a>Microsoft.EventHub
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| clusters | JA | 
| namespaces | JA | 
| namespaces/authorizationrules | Nein  | 
| namespaces/disasterrecoveryconfigs | Nein  | 
| namespaces/eventhubs | Nein  | 
| namespaces/eventhubs/authorizationrules | Nein  | 
| namespaces/eventhubs/consumergroups | Nein  | 

## <a name="microsoftfeatures"></a>Microsoft.Features
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| Features | Nein  | 
| providers | Nein  | 

## <a name="microsoftgallery"></a>Microsoft.Gallery
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| enroll | Nein  | 
| galleryitems | Nein  | 
| generateartifactaccessuri | Nein  | 
| myareas | Nein  | 
| myareas/areas | Nein  | 
| myareas/areas/areas | Nein  | 
| myareas/areas/areas/galleryitems | Nein  | 
| myareas/areas/galleryitems | Nein  | 
| myareas/galleryitems | Nein  | 
| Registrieren | Nein  | 
| ressourcen | Nein  | 
| retrieveresourcesbyid | Nein  | 

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| guestConfigurationAssignments | Nein  | 
| software | Nein  | 

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| hanaInstances | JA | 

## <a name="microsofthdinsight"></a>Microsoft.HDInsight
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| clusters | JA | 
| clusters/applications | Nein  | 

## <a name="microsoftimportexport"></a>Microsoft.ImportExport
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| jobs | JA | 

## <a name="microsoftinformationprotection"></a>Microsoft.InformationProtection
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| labelGroups | Nein  | 
| labelGroups/labels | Nein  | 
| labelGroups/labels/conditions | Nein  | 
| labelGroups/labels/subLabels | Nein  | 
| labelGroups/labels/subLabels/conditions | Nein  | 

## <a name="microsoftinsights"></a>microsoft.insights
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| actiongroups | JA | 
| activityLogAlerts | JA | 
| alertrules | JA | 
| automatedExportSettings | Nein  | 
| autoscalesettings | JA | 
| baseline | Nein  | 
| calculatebaseline | Nein  | 
| components | JA | 
| components/events | Nein  | 
| components/pricingPlans | Nein  | 
| components/query | Nein  | 
| diagnosticSettings | Nein  | 
| diagnosticSettingsCategories | Nein  | 
| eventCategories | Nein  | 
| eventtypes | Nein  | 
| extendedDiagnosticSettings | Nein  | 
| logDefinitions | Nein  | 
| logprofiles | Nein  | 
| Protokolle | Nein  | 
| migrateToNewPricingModel | Nein  | 
| myWorkbooks | Nein  | 
| Abfragen | Nein  | 
| rollbackToLegacyPricingModel | Nein  | 
| scheduledqueryrules | JA | 
| vmInsightsOnboardingStatuses | Nein  | 
| webtests | JA | 
| workbooks | JA | 

## <a name="microsoftintune"></a>Microsoft.Intune
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| diagnosticSettings | Nein  | 
| diagnosticSettingsCategories | Nein  | 

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| IoTApps | JA | 

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| Graph | JA | 

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| deletedVaults | Nein  | 
| Tresore | JA | 
| vaults/accessPolicies | Nein  | 
| vaults/secrets | Nein  | 

## <a name="microsoftkusto"></a>Microsoft.Kusto
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| clusters | JA | 
| clusters/databases | Nein  | 
| clusters/databases/dataconnections | Nein  | 
| clusters/databases/eventhubconnections | Nein  | 

## <a name="microsoftlabservices"></a>Microsoft.LabServices
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| labaccounts | JA | 
| users | Nein  | 

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| accounts | JA | 

## <a name="microsoftlocationservices"></a>Microsoft.LocationServices
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| accounts | JA | 

## <a name="microsoftloganalytics"></a>Microsoft.LogAnalytics
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| Protokolle | Nein  | 

## <a name="microsoftlogic"></a>Microsoft.Logic
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| integrationAccounts | JA | 
| Workflows | JA | 

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| commitmentPlans | JA | 
| webServices | JA | 
| Arbeitsbereiche | JA | 

## <a name="microsoftmachinelearningexperimentation"></a>Microsoft.MachineLearningExperimentation
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| accounts | JA | 
| accounts/workspaces | JA | 
| accounts/workspaces/projects | JA | 
| teamAccounts | JA | 
| teamAccounts/workspaces | JA | 
| teamAccounts/workspaces/projects | JA | 

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| accounts | JA | 

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| workspaces | JA | 
| workspaces/computes | Nein  | 

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| Identities | Nein  | 
| userAssignedIdentities | JA | 

## <a name="microsoftmanagement"></a>Microsoft.Management
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| getEntities | Nein  | 
| managementGroups | Nein  | 
| ressourcen | Nein  | 
| startTenantBackfill | Nein  | 
| tenantBackfillStatus | Nein  | 

## <a name="microsoftmaps"></a>Microsoft.Maps
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| accounts | JA | 
| accounts/eventGridFilters | Nein  | 

## <a name="microsoftmarketplace"></a>Microsoft.Marketplace
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| offers | Nein  | 
| offerTypes | Nein  | 
| offerTypes/publishers | Nein  | 
| offerTypes/publishers/offers | Nein  | 
| offerTypes/publishers/offers/plans | Nein  | 
| offerTypes/publishers/offers/plans/agreements | Nein  | 
| offerTypes/publishers/offers/plans/configs | Nein  | 
| offerTypes/publishers/offers/plans/configs/importImage | Nein  | 
| privategalleryitems | Nein  | 
| products | Nein  | 

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| classicDevServices | JA | 
| updateCommunicationPreference | Nein  | 

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| agreements | Nein  | 
| offertypes | Nein  | 

## <a name="microsoftmedia"></a>Microsoft.Media
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| mediaservices | JA | 
| mediaservices/accountFilters | Nein  | 
| mediaservices/assets | Nein  | 
| mediaservices/assets/assetFilters | Nein  | 
| mediaservices/contentKeyPolicies | Nein  | 
| mediaservices/eventGridFilters | Nein  | 
| mediaservices/liveEventOperations | Nein  | 
| mediaservices/liveEvents | JA | 
| mediaservices/liveEvents/liveOutputs | Nein  | 
| mediaservices/liveOutputOperations | Nein  | 
| mediaservices/streamingEndpointOperations | Nein  | 
| mediaservices/streamingEndpoints | JA | 
| mediaservices/streamingLocators | Nein  | 
| mediaservices/streamingPolicies | Nein  | 
| mediaservices/transforms | Nein  | 
| mediaservices/transforms/jobs | Nein  | 

## <a name="microsoftmigrate"></a>Microsoft.Migrate
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| projects | JA | 

## <a name="microsoftnetwork"></a>Microsoft.Network
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| applicationGateways | JA | 
| applicationSecurityGroups | JA | 
| azureFirewallFqdnTags | Nein  | 
| azureFirewalls | JA | 
| bgpServiceCommunities | Nein  | 
| connections | JA | 
| ddosCustomPolicies | JA | 
| ddosProtectionPlans | JA | 
| dnsOperationStatuses | Nein  | 
| dnszones | JA | 
| dnszones/A | Nein  | 
| dnszones/AAAA | Nein  | 
| dnszones/all | Nein  | 
| dnszones/CAA | Nein  | 
| dnszones/CNAME | Nein  | 
| dnszones/MX | Nein  | 
| dnszones/NS | Nein  | 
| dnszones/PTR | Nein  | 
| dnszones/recordsets | Nein  | 
| dnszones/SOA | Nein  | 
| dnszones/SRV | Nein  | 
| dnszones/TXT | Nein  | 
| expressRouteCircuits | JA | 
| expressRouteServiceProviders | Nein  | 
| frontdoors | JA | 
| frontdoorWebApplicationFirewallPolicies | JA | 
| getDnsResourceReference | Nein  | 
| interfaceEndpoints | JA | 
| internalNotify | Nein  | 
| loadBalancers | JA | 
| localNetworkGateways | JA | 
| natGateways | JA | 
| networkIntentPolicies | JA | 
| networkInterfaces | JA | 
| networkProfiles | JA | 
| networkSecurityGroups | JA | 
| networkWatchers | JA | 
| networkWatchers/connectionMonitors | JA | 
| networkWatchers/lenses | JA | 
| networkWatchers/pingMeshes | JA | 
| privateLinkServices | JA | 
| publicIPAddresses | JA | 
| publicIPPrefixes | JA | 
| routeFilters | JA | 
| routeTables | JA | 
| serviceEndpointPolicies | JA | 
| trafficManagerGeographicHierarchies | Nein  | 
| trafficmanagerprofiles | JA | 
| trafficmanagerprofiles/heatMaps | Nein  | 
| virtualHubs | JA | 
| virtualNetworkGateways | JA | 
| virtualNetworks | JA | 
| virtualNetworkTaps | JA | 
| virtualWans | JA | 
| vpnGateways | JA | 
| vpnSites | JA | 
| webApplicationFirewallPolicies | JA | 

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| namespaces | JA | 
| namespaces/notificationHubs | JA | 

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| devices | Nein  | 
| linkTargets | Nein  | 
| storageInsightConfigs | Nein  | 
| workspaces | JA | 
| workspaces/dataSources | Nein  | 
| workspaces/linkedServices | Nein  | 
| workspaces/query | Nein  | 

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| managementassociations | Nein  | 
| managementconfigurations | JA | 
| solutions | JA | 
| Sichten | JA | 

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| policyEvents | Nein  | 
| policyStates | Nein  | 
| policyTrackedResources | Nein  | 
| remediations | Nein  | 

## <a name="microsoftportal"></a>Microsoft.Portal
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| consoles | Nein  | 
| dashboards | JA | 
| userSettings | Nein  | 

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| workspaceCollections | JA | 

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| capacities | JA | 

## <a name="microsoftprojectoxford"></a>Microsoft.ProjectOxford
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| accounts | JA | 

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| backupProtectedItems | Nein  | 
| Tresore | JA | 

## <a name="microsoftrelay"></a>Microsoft.Relay
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| namespaces | JA | 
| namespaces/authorizationrules | Nein  | 
| namespaces/hybridconnections | Nein  | 
| namespaces/hybridconnections/authorizationrules | Nein  | 
| namespaces/wcfrelays | Nein  | 
| namespaces/wcfrelays/authorizationrules | Nein  | 

## <a name="microsoftresourcegraph"></a>Microsoft.ResourceGraph
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| ressourcen | Nein  | 
| subscriptionsStatus | Nein  | 

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| availabilityStatuses | Nein  | 
| childAvailabilityStatuses | Nein  | 
| childResources | Nein  | 
| events | Nein  | 
| impactedResources | Nein  | 
| Benachrichtigungen | Nein  | 

## <a name="microsoftresources"></a>Microsoft.Resources
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| deployments | Nein  | 
| deployments/operations | Nein  | 
| links | Nein  | 
| notifyResourceJobs | Nein  | 
| providers | Nein  | 
| resourceGroups | Nein  | 
| ressourcen | Nein  | 
| subscriptions | Nein  | 
| subscriptions/providers | Nein  | 
| subscriptions/resourceGroups | Nein  | 
| subscriptions/resourcegroups/resources | Nein  | 
| subscriptions/resources | Nein  | 
| subscriptions/tagnames | Nein  | 
| subscriptions/tagNames/tagValues | Nein  | 
| tenants | Nein  | 

## <a name="microsoftsaas"></a>Microsoft.SaaS
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| Anwendungen | JA | 
| saasresources | Nein  | 

## <a name="microsoftscheduler"></a>Microsoft.Scheduler
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| flows | JA | 
| jobcollections | JA | 

## <a name="microsoftsearch"></a>Microsoft.Search
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| resourceHealthMetadata | Nein  | 
| searchServices | JA | 

## <a name="microsoftsecurity"></a>Microsoft.Security
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| advancedThreatProtectionSettings | Nein  | 
| alerts | Nein  | 
| allowedConnections | Nein  | 
| appliances | Nein  | 
| applicationWhitelistings | Nein  | 
| AutoProvisioningSettings | Nein  | 
| Compliances | Nein  | 
| dataCollectionAgents | Nein  | 
| discoveredSecuritySolutions | Nein  | 
| externalSecuritySolutions | Nein  | 
| InformationProtectionPolicies | Nein  | 
| jitNetworkAccessPolicies | Nein  | 
| monitoring | Nein  | 
| monitoring/antimalware | Nein  | 
| monitoring/baseline | Nein  | 
| monitoring/patch | Nein  | 
| Richtlinien | Nein  | 
| pricings | Nein  | 
| securityContacts | Nein  | 
| securitySolutions | Nein  | 
| securitySolutionsReferenceData | Nein  | 
| securityStatus | Nein  | 
| securityStatus/endpoints | Nein  | 
| securityStatus/subnets | Nein  | 
| securityStatus/virtualMachines | Nein  | 
| securityStatuses | Nein  | 
| securityStatusesSummaries | Nein  | 
| settings | Nein  | 
| Tasks | Nein  | 
| topologies | Nein  | 
| workspaceSettings | Nein  | 

## <a name="microsoftsecuritygraph"></a>Microsoft.SecurityGraph
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| diagnosticSettings | Nein  | 
| diagnosticSettingsCategories | Nein  | 

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| namespaces | JA | 
| namespaces/authorizationrules | Nein  | 
| namespaces/disasterrecoveryconfigs | Nein  | 
| namespaces/eventgridfilters | Nein  | 
| namespaces/queues | Nein  | 
| namespaces/queues/authorizationrules | Nein  | 
| namespaces/topics | Nein  | 
| namespaces/topics/authorizationrules | Nein  | 
| namespaces/topics/subscriptions | Nein  | 
| namespaces/topics/subscriptions/rules | Nein  | 
| premiumMessagingRegions | Nein  | 

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| clusters | JA | 
| clusters/applications | Nein  | 

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| Anwendungen | JA | 
| gateways | JA | 
| networks | JA | 
| secrets | JA | 
| volumes | JA | 

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| SignalR | JA | 

## <a name="microsoftsolutions"></a>Microsoft.Solutions
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| applianceDefinitions | JA | 
| appliances | JA | 
| applicationDefinitions | JA | 
| Anwendungen | JA | 
| jitRequests | JA | 

## <a name="microsoftsql"></a>Microsoft.SQL
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| servers | JA | 
| servers/administrators | Nein  | 
| servers/communicationLinks | Nein  | 
| servers/databases | JA | 
| servers/encryptionProtector | Nein  | 
| servers/keys | Nein  | 
| servers/restorableDroppedDatabases | Nein  | 
| servers/serviceobjectives | Nein  | 
| servers/tdeCertificates | Nein  | 

## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| SqlVirtualMachineGroups | JA | 
| SqlVirtualMachineGroups/AvailabilityGroupListeners | Nein  | 
| SqlVirtualMachines | JA | 

## <a name="microsoftstorage"></a>Microsoft.Storage
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| storageAccounts | JA | 
| storageAccounts/blobServices | Nein  | 
| storageAccounts/fileServices | Nein  | 
| storageAccounts/queueServices | Nein  | 
| storageAccounts/services | Nein  | 
| storageAccounts/tableServices | Nein  | 
| usages | Nein  | 

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| storageSyncServices | JA | 
| storageSyncServices/registeredServers | Nein  | 
| storageSyncServices/syncGroups | Nein  | 
| storageSyncServices/syncGroups/cloudEndpoints | Nein  | 
| storageSyncServices/syncGroups/serverEndpoints | Nein  | 
| storageSyncServices/workflows | Nein  | 

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| managers | JA | 

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| streamingjobs | JA | 
| streamingjobs/diagnosticSettings | Nein  | 

## <a name="microsoftsubscription"></a>Microsoft.Subscription
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| CreateSubscription | Nein  | 
| SubscriptionDefinitions | Nein  | 
| SubscriptionOperations | Nein  | 

## <a name="microsoftsupport"></a>microsoft.support
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| supporttickets | Nein  | 

## <a name="microsoftterraformoss"></a>Microsoft.TerraformOSS
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| providerRegistrations | JA | 
| ressourcen | JA | 

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| environments | JA | 
| environments/accessPolicies | Nein  | 
| environments/eventsources | JA | 
| environments/referenceDataSets | JA | 

## <a name="microsoftvisualstudio"></a>microsoft.visualstudio
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| Konto | JA | 
| account/extension | JA | 
| account/project | JA | 

## <a name="microsoftweb"></a>Microsoft.Web
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| apiManagementAccounts | Nein  | 
| apiManagementAccounts/apiAcls | Nein  | 
| apiManagementAccounts/apis | Nein  | 
| apiManagementAccounts/apis/apiAcls | Nein  | 
| apiManagementAccounts/apis/connectionAcls | Nein  | 
| apiManagementAccounts/apis/connections | Nein  | 
| apiManagementAccounts/apis/connections/connectionAcls | Nein  | 
| apiManagementAccounts/apis/localizedDefinitions | Nein  | 
| apiManagementAccounts/connectionAcls | Nein  | 
| apiManagementAccounts/connections | Nein  | 
| billingMeters | Nein  | 
| certificates | JA | 
| connectionGateways | JA | 
| connections | JA | 
| customApis | JA | 
| deletedSites | Nein  | 
| functions | Nein  | 
| hostingEnvironments | JA | 
| hostingEnvironments/multiRolePools | Nein  | 
| hostingEnvironments/multiRolePools/instances | Nein  | 
| hostingEnvironments/workerPools | Nein  | 
| hostingEnvironments/workerPools/instances | Nein  | 
| publishingUsers | Nein  | 
| empfehlungen | Nein  | 
| resourceHealthMetadata | Nein  | 
| runtimes | Nein  | 
| serverFarms | JA | 
| serverFarms/workers | Nein  | 
| sites | JA | 
| sites/domainOwnershipIdentifiers | Nein  | 
| sites/hostNameBindings | Nein  | 
| sites/instances | Nein  | 
| sites/instances/extensions | Nein  | 
| sites/premieraddons | JA | 
| sites/recommendations | Nein  | 
| sites/resourceHealthMetadata | Nein  | 
| sites/slots | JA | 
| sites/slots/hostNameBindings | Nein  | 
| sites/slots/instances | Nein  | 
| sites/slots/instances/extensions | Nein  | 
| sourceControls | Nein  | 
| validate | Nein  | 
| verifyHostingEnvironmentVnet | Nein  | 

## <a name="microsoftwindowsdefenderatp"></a>Microsoft.WindowsDefenderATP
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| diagnosticSettings | Nein  | 
| diagnosticSettingsCategories | Nein  | 

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| DeviceServices | JA | 

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| components | Nein  | 
| componentsSummary | Nein  | 
| monitorInstances | Nein  | 
| monitorInstancesSummary | Nein  | 
| monitors | Nein  | 
| notificationSettings | Nein  | 

## <a name="next-steps"></a>Nächste Schritte
Informationen zum Anwenden von Tags auf Ressourcen finden Sie unter [Verwenden von Tags zum Organisieren von Azure-Ressourcen](resource-group-using-tags.md).
