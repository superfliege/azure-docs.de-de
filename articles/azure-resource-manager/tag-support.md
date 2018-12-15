---
title: 'Azure Resource Manager: Tagunterstützung für Ressourcen'
description: Zeigt, welche Azure-Ressourcentypen Tags unterstützen. Enthält Details für alle Azure-Dienste.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 11/20/2018
ms.author: tomfitz
ms.openlocfilehash: 580955d3c6fd7a33c152e49e601d8078eb169a22
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2018
ms.locfileid: "53409665"
---
# <a name="tag-support-for-azure-resources"></a>Tagunterstützung für Azure-Ressourcen
In diesem Artikel erfahren Sie, ob ein Ressourcentyp [Tags](resource-group-using-tags.md) unterstützt.

## <a name="aad-domain-services"></a>AAD Domain Services
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| domains | Nein  | 

## <a name="ad-hybrid-health-service"></a>AD Hybrid Health Service
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| services | Nein  | 
| addsservices | Nein  | 
| Konfiguration | Nein  | 
| agents | Nein  | 
| aadsupportcases | Nein  | 
| reports | Nein  | 
| servicehealthmetrics | Nein  | 
| Protokolle | Nein  | 
| anonymousapiusers | Nein  | 

## <a name="analysis-services"></a>Analysis Services
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| servers | JA | 

## <a name="api-hubs"></a>API-Hubs
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| apiManagementAccounts | Nein  | 
| apiManagementAccounts/connectionProviders | Nein  | 
| apiManagementAccounts/connections | Nein  | 
| apiManagementAccounts/connectionAcls | Nein  | 
| apiManagementAccounts/connectionProviderAcls | Nein  | 
| apiManagementAccounts/apis | Nein  | 

## <a name="api-management"></a>API Management
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| service | JA | 

## <a name="automation"></a>Automation
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| automationAccounts | JA | 
| automationAccounts/runbooks | JA | 
| automationAccounts/configurations | JA | 
| automationAccounts/webhooks | Nein  | 
| automationAccounts/softwareUpdateConfigurations | Nein  | 
| automationAccounts/jobs | Nein  | 

## <a name="batch"></a>Batch
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| batchAccounts | JA | 

## <a name="bing-maps"></a>Bing Maps
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| mapApis | JA | 

## <a name="biztalk-services"></a>BizTalk Services
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| BizTalk | JA | 

## <a name="cache"></a>Cache
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| Redis | JA | 

## <a name="cdn"></a>CDN
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| Profile | JA | 
| profiles/endpoints | JA | 
| profiles/endpoints/origins | Nein  | 
| profiles/endpoints/customdomains | Nein  | 
| validateProbe | Nein  | 
| edgenodes | Nein  | 

## <a name="classic-compute"></a>Klassische Computeressourcen
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| domainNames | Nein  | 
| domainNames/slots | Nein  | 
| domainNames/slots/roles | Nein  | 
| virtualMachines | Nein  | 
| virtualMachines/diagnosticSettings | Nein  | 
| virtualMachines/metricDefinitions | Nein  | 
| virtualMachines/metrics | Nein  | 

## <a name="classic-infrastructure-migrate"></a>Klassische Infrastrukturmigration
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| classicInfrastructureResources | Nein  | 

## <a name="classic-network"></a>Klassisches Netzwerk
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| virtualNetworks | Nein  | 
| virtualNetworks/virtualNetworkPeerings | Nein  | 
| virtualNetworks/remoteVirtualNetworkPeeringProxies | Nein  | 

## <a name="classic-storage"></a>Klassischer Speicher
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| storageAccounts/services | Nein  | 
| storageAccounts/services/diagnosticSettings | Nein  | 

## <a name="compute"></a>Compute
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| availabilitySets | JA | 
| virtualMachines | JA | 
| virtualMachines/extensions | JA | 
| virtualMachineScaleSets | JA | 
| virtualMachineScaleSets/extensions | Nein  | 
| virtualMachineScaleSets/virtualMachines | Nein  | 
| virtualMachineScaleSets/networkInterfaces | Nein  | 
| virtualMachineScaleSets/virtualMachines/networkInterfaces | Nein  | 
| virtualMachineScaleSets/publicIPAddresses | Nein  | 
| restorePointCollections | JA | 
| restorePointCollections/restorePoints | Nein  | 
| virtualMachines/diagnosticSettings | Nein  | 
| virtualMachines/metricDefinitions | Nein  | 
| sharedVMImages | JA | 
| sharedVMImages/versions | JA | 
| disks | JA | 
| snapshots | JA | 
| images | JA | 

## <a name="container"></a>Container
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| containerGroups | JA | 

## <a name="container-instance"></a>Containerinstanz
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| containerGroups | JA | 
| serviceAssociationLinks | Nein  | 

## <a name="container-service"></a>Container Service
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| containerServices | JA | 

## <a name="cortana-analytics"></a>Cortana Analytics Suite
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| accounts | JA | 

## <a name="cosmos-db"></a>Cosmos DB
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| databaseAccounts | JA | 
| databaseAccountNames | Nein  | 

## <a name="cost-management"></a>Cost Management
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| Connectors | JA | 

## <a name="data-box-edge"></a>Data Box Edge
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| DataBoxEdgeDevices | JA | 

## <a name="data-catalog"></a>Data Catalog
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| catalogs | JA | 

## <a name="data-connect"></a>Datenverbindungen
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| connectionManagers | JA | 

## <a name="data-factory"></a>Data Factory
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| dataFactories | JA | 
| factories | JA | 
| factories/integrationRuntimes | Nein  | 
| dataFactories/diagnosticSettings | Nein  | 
| dataFactories/metricDefinitions | Nein  | 
| dataFactorySchema | Nein  | 

## <a name="devices"></a>Geräte
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| IotHubs | JA | 
| IotHubs/eventGridFilters | Nein  | 
| ProvisioningServices | JA | 

## <a name="devspaces"></a>Devspaces
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| Controller | JA | 

## <a name="devtest-lab"></a>DevTest Lab
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| labs | JA | 
| schedules | JA | 
| labs/virtualMachines | JA | 
| labs/serviceRunners | JA | 

## <a name="dynamics-lcs"></a>Dynamics LCS
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| lcsprojects | Nein  | 
| lcsprojects/connectors | Nein  | 
| lcsprojects/clouddeployments | Nein  | 

## <a name="event-grid"></a>Event Grid
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| eventSubscriptions | Nein  | 
| topics | JA | 
| domains | JA | 
| domains/topics | Nein  | 
| topicTypes | Nein  | 
| extensionTopics | Nein  | 

## <a name="event-hub"></a>Event Hub
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| namespaces | JA | 
| clusters | JA | 

## <a name="hana-on-azure"></a>Hana in Azure
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| hanaInstances | JA | 

## <a name="hdinsight"></a>HDInsight
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| clusters | JA | 
| clusters/applications | Nein  | 

## <a name="import-export"></a>Import/Export
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| jobs | JA | 

## <a name="insights"></a>Einblicke
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| components | JA | 
| components/query | Nein  | 
| components/metrics | Nein  | 
| components/events | Nein  | 
| webtests | JA | 
| Abfragen | Nein  | 
| scheduledqueryrules | JA | 
| components/pricingPlans | Nein  | 
| migrateToNewPricingModel | Nein  | 
| rollbackToLegacyPricingModel | Nein  | 
| automatedExportSettings | Nein  | 
| workbooks | JA | 
| myWorkbooks | Nein  | 
| Protokolle | Nein  | 

## <a name="key-vault"></a>Key Vault
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| Tresore | JA | 
| vaults/secrets | Nein  | 
| vaults/accessPolicies | Nein  | 
| deletedVaults | Nein  | 

## <a name="log-analytics"></a>Log Analytics
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| Protokolle | Nein  | 

## <a name="logic"></a>Logik
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| Workflows | JA | 
| integrationAccounts | JA | 

## <a name="machine-learning-services"></a>Machine Learning Services
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| workspaces | JA | 
| workspaces/computes | Nein  | 

## <a name="managed-identity"></a>Verwaltete Identität
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| Identities | Nein  | 
| userAssignedIdentities | JA | 

## <a name="mariadb"></a>MariaDB
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| servers | JA | 
| servers/recoverableServers | Nein  | 
| servers/virtualNetworkRules | Nein  | 

## <a name="marketplace-apps"></a>Marketplace-App
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| classicDevServices | JA | 

## <a name="marketplace-ordering"></a>Marketplace-Bestellungen
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| agreements | Nein  | 
| offertypes | Nein  | 

## <a name="media"></a>Medien
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| mediaservices | JA | 
| mediaservices/assets | Nein  | 
| mediaservices/contentKeyPolicies | Nein  | 
| mediaservices/streamingLocators | Nein  | 
| mediaservices/streamingPolicies | Nein  | 
| mediaservices/eventGridFilters | Nein  | 
| mediaservices/transforms | Nein  | 
| mediaservices/transforms/jobs | Nein  | 
| mediaservices/streamingEndpoints | JA | 
| mediaservices/liveEvents | JA | 
| mediaservices/liveEvents/liveOutputs | Nein  | 
| mediaservices/streamingEndpointOperations | Nein  | 
| mediaservices/liveEventOperations | Nein  | 
| mediaservices/liveOutputOperations | Nein  | 
| mediaservices/assets/assetFilters | Nein  | 
| mediaservices/accountFilters | Nein  | 

## <a name="mysql"></a>MySQL
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| servers | JA | 
| servers/recoverableServers | Nein  | 
| servers/virtualNetworkRules | Nein  | 

## <a name="network"></a>Netzwerk
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| virtualNetworks | JA | 
| publicIPAddresses | JA | 
| networkInterfaces | JA | 
| interfaceEndpoints | JA | 
| loadBalancers | JA | 
| networkSecurityGroups | JA | 
| applicationSecurityGroups | JA | 
| serviceEndpointPolicies | JA | 
| networkIntentPolicies | JA | 
| routeTables | JA | 
| publicIPPrefixes | JA | 
| networkWatchers | JA | 
| networkWatchers/connectionMonitors | JA | 
| networkWatchers/lenses | JA | 
| networkWatchers/pingMeshes | JA | 
| virtualNetworkGateways | JA | 
| localNetworkGateways | JA | 
| connections | JA | 
| applicationGateways | JA | 
| expressRouteCircuits | JA | 
| routeFilters | JA | 
| virtualWans | JA | 
| vpnSites | JA | 
| virtualHubs | JA | 
| vpnGateways | JA | 
| azureFirewalls | JA | 
| virtualNetworkTaps | JA | 
| privateLinkServices | JA | 
| ddosProtectionPlans | JA | 
| networkProfiles | JA | 
| frontdoors | JA | 
| frontdoorWebApplicationFirewallPolicies | JA | 
| webApplicationFirewallPolicies | JA | 

## <a name="notification-hubs"></a>Notification Hubs
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| namespaces | JA | 
| namespaces/notificationHubs | JA | 

## <a name="portal"></a>Portal
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| dashboards | JA | 

## <a name="portal-sdk"></a>Portal SDK
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| rootResources | JA | 

## <a name="postgresql"></a>PostgreSQL
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| servers | JA | 
| servers/recoverableServers | Nein  | 
| servers/virtualNetworkRules | Nein  | 
| servers/topQueryStatistics | Nein  | 
| servers/queryTexts | Nein  | 
| servers/waitStatistics | Nein  | 
| servers/advisors | Nein  | 

## <a name="power-bi"></a>Power BI
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| workspaceCollections | JA | 

## <a name="recovery-services"></a>Recovery Services
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| Tresore | JA | 
| backupProtectedItems | Nein  | 

## <a name="relay"></a>Relay
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| namespaces | JA | 

## <a name="resources"></a>Ressourcen
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| resourceGroups | JA | 
| subscriptions/resourceGroups | JA | 

## <a name="scheduler"></a>Scheduler
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| jobcollections | JA | 
| flows | JA | 

## <a name="search"></a>Suchen,
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| searchServices | JA | 
| resourceHealthMetadata | Nein  | 

## <a name="security"></a>Sicherheit
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| dataCollectionAgents | Nein  | 

## <a name="service-bus"></a>Service Bus
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| namespaces | JA | 
| namespaces/eventgridfilters | Nein  | 

## <a name="service-fabric"></a>Service Fabric
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| clusters | JA | 
| clusters/applications | Nein  | 

## <a name="service-fabric-mesh"></a>Service Fabric Mesh
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| Anwendungen | JA | 
| networks | JA | 
| volumes | JA | 

## <a name="signalr-service"></a>SignalR Service
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| SignalR | JA | 

## <a name="site-recovery"></a>Site Recovery
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| SiteRecoveryVault | JA | 

## <a name="solutions"></a>Lösungen
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| Anwendungen | JA | 
| applicationDefinitions | JA | 
| jitRequests | JA | 

## <a name="sql-virtual-machine"></a>Virtueller SQL-Computer
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| DWVM | JA | 

## <a name="storage"></a>Storage
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| storageAccounts | JA | 
| storageAccounts/blobServices | Nein  | 
| storageAccounts/tableServices | Nein  | 
| storageAccounts/queueServices | Nein  | 
| storageAccounts/fileServices | Nein  | 
| storageAccounts/services | Nein  | 
| storageAccounts/services/metricDefinitions | Nein  | 

## <a name="storage-sync"></a>Speichersynchronisierung
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| storageSyncServices | JA | 
| storageSyncServices/syncGroups | Nein  | 
| storageSyncServices/syncGroups/cloudEndpoints | Nein  | 
| storageSyncServices/syncGroups/serverEndpoints | Nein  | 
| storageSyncServices/registeredServers | Nein  | 
| storageSyncServices/workflows | Nein  | 

## <a name="storsimple"></a>Storsimple
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| managers | JA | 

## <a name="stream-analytics"></a>Stream Analytics
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| streamingjobs | JA | 
| streamingjobs/diagnosticSettings | Nein  | 
| streamingjobs/metricDefinitions | Nein  | 

## <a name="subscription"></a>Abonnement
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| SubscriptionDefinitions | Nein  | 
| SubscriptionOperations | Nein  | 

## <a name="support"></a>Support
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| supporttickets | Nein  | 

## <a name="visual-studio"></a>Visual Studio
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| Konto | JA | 
| account/project | JA | 
| account/extension | JA | 
| Konto | JA | 
| account/project | JA | 
| account/extension | JA | 

## <a name="web"></a>Web
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| sites/instances | Nein  | 
| sites/slots/instances | Nein  | 
| sites/instances/extensions | Nein  | 
| sites/slots/instances/extensions | Nein  | 
| publishingUsers | Nein  | 
| validate | Nein  | 
| sourceControls | Nein  | 
| sites/hostNameBindings | Nein  | 
| sites/domainOwnershipIdentifiers | Nein  | 
| sites/slots/hostNameBindings | Nein  | 
| certificates | JA | 
| serverFarms | JA | 
| serverFarms/workers | Nein  | 
| sites | JA | 
| sites/slots | JA | 
| sites/metrics | Nein  | 
| sites/slots/metrics | Nein  | 
| sites/premieraddons | JA | 
| hostingEnvironments | JA | 
| hostingEnvironments/multiRolePools | Nein  | 
| hostingEnvironments/workerPools | Nein  | 
| hostingEnvironments/metrics | Nein  | 
| functions | Nein  | 
| deletedSites | Nein  | 
| apiManagementAccounts | Nein  | 
| apiManagementAccounts/connections | Nein  | 
| apiManagementAccounts/connectionAcls | Nein  | 
| apiManagementAccounts/apis/connections/connectionAcls | Nein  | 
| apiManagementAccounts/apis/connectionAcls | Nein  | 
| apiManagementAccounts/apiAcls | Nein  | 
| apiManagementAccounts/apis/apiAcls | Nein  | 
| apiManagementAccounts/apis | Nein  | 
| apiManagementAccounts/apis/localizedDefinitions | Nein  | 
| apiManagementAccounts/apis/connections | Nein  | 
| connections | JA | 
| customApis | JA | 
| connectionGateways | JA | 
| billingMeters | Nein  | 
| verifyHostingEnvironmentVnet | Nein  | 

## <a name="xrm"></a>XRM
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| organizations | Nein  | 

## <a name="next-steps"></a>Nächste Schritte
Informationen zum Anwenden von Tags auf Ressourcen finden Sie unter [Verwenden von Tags zum Organisieren von Azure-Ressourcen](resource-group-using-tags.md).
