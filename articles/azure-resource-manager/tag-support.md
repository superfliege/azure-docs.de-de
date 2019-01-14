---
title: 'Azure Resource Manager: Tagunterstützung für Ressourcen'
description: Zeigt, welche Azure-Ressourcentypen Tags unterstützen. Enthält Details für alle Azure-Dienste.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 01/02/2019
ms.author: tomfitz
ms.openlocfilehash: 50ea7a2446b5560bd208b2da128fa877068ce452
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/03/2019
ms.locfileid: "54000290"
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
| addsservices | Nein  |
| aadsupportcases | Nein  | 
| agents | Nein  | 
| anonymousapiusers | Nein  | 
| Konfiguration | Nein  | 
| Protokolle | Nein  | 
| reports | Nein  | 
| services | Nein  | 
| servicehealthmetrics | Nein  | 

## <a name="aks"></a>AKS
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| managedClusters | JA | 

## <a name="analysis-services"></a>Analysis Services
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| servers | JA | 

## <a name="api-hubs"></a>API-Hubs
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| apiManagementAccounts | Nein  | 
| apiManagementAccounts/apis | Nein  | 
| apiManagementAccounts/connectionAcls | Nein  | 
| apiManagementAccounts/connectionProviders | Nein  | 
| apiManagementAccounts/connectionProviderAcls | Nein  | 
| apiManagementAccounts/connections | Nein  | 

## <a name="api-management"></a>API Management
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| service | JA | 

## <a name="automation"></a>Automation
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| automationAccounts | JA | 
| automationAccounts/configurations | JA | 
| automationAccounts/jobs | Nein  | 
| automationAccounts/runbooks | JA | 
| automationAccounts/softwareUpdateConfigurations | Nein  | 
| automationAccounts/webhooks | Nein  | 

## <a name="azure-database-for-mariadb"></a>Azure Database for MariaDB
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| servers | JA | 
| servers/configurations | Nein  |
| servers/databases | Nein  |
| servers/firewallRules | Nein  |
| servers/recoverableServers | Nein  | 
| servers/securityAlertPolicies | Nein  |
| servers/virtualNetworkRules | Nein  | 

## <a name="azure-database-for-mysql"></a>Azure Database for MySQL
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| servers | JA | 
| servers/configurations | Nein  |
| servers/databases | Nein  |
| servers/firewallRules | Nein  |
| servers/recoverableServers | Nein  | 
| servers/securityAlertPolicies | Nein  |
| servers/virtualNetworkRules | Nein  | 

## <a name="azure-database-for-postgresql"></a>Azure Database for PostgreSQL
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| servers | JA | 
| servers/advisors | Nein  | 
| servers/configurations | Nein  |
| servers/databases | Nein  |
| servers/firewallRules | Nein  |
| servers/queryTexts | Nein  | 
| servers/recoverableServers | Nein  | 
| servers/securityAlertPolicies | Nein  |
| servers/topQueryStatistics | Nein  | 
| servers/virtualNetworkRules | Nein  | 
| servers/waitStatistics | Nein  | 

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
| edgenodes | Nein  | 
| Profile | JA | 
| profiles/endpoints | JA | 
| profiles/endpoints/customdomains | Nein  | 
| profiles/endpoints/origins | Nein  | 
| validateProbe | Nein  | 

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
| virtualNetworks/remoteVirtualNetworkPeeringProxies | Nein  | 
| virtualNetworks/virtualNetworkPeerings | Nein  | 

## <a name="classic-storage"></a>Klassischer Speicher
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| storageAccounts/services | Nein  | 
| storageAccounts/services/diagnosticSettings | Nein  | 

## <a name="compute"></a>Compute
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
| virtualMachines/metricDefinitions | Nein  | 
| virtualMachineScaleSets | JA | 
| virtualMachineScaleSets/extensions | Nein  | 
| virtualMachineScaleSets/networkInterfaces | Nein  | 
| virtualMachineScaleSets/publicIPAddresses | Nein  | 
| virtualMachineScaleSets/virtualMachines | Nein  | 
| virtualMachineScaleSets/virtualMachines/networkInterfaces | Nein  | 

## <a name="container"></a>Container
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| containerGroups | JA | 

## <a name="container-instance"></a>Containerinstanz
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| containerGroups | JA | 
| serviceAssociationLinks | Nein  | 

## <a name="container-registry"></a>Containerregistrierung
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| registries | JA | 
| registries/replications | JA |
| registries/tasks | JA |
| registries/webhooks | JA |

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

## <a name="data-box"></a>Data Box
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| jobs | JA | 

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
| dataFactories/diagnosticSettings | Nein  | 
| dataFactories/metricDefinitions | Nein  | 
| dataFactorySchema | Nein  | 
| factories | JA | 
| factories/integrationRuntimes | Nein  | 

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
| labs/artifactsources | JA |
| labs/costs | JA |
| labs/customimages | JA |
| labs/formulas | JA |
| labs/notificationchannels | JA |
| labs/policysets/policies | JA |
| labs/schedules | JA |
| labs/serviceRunners | JA | 
| labs/users | JA |
| labs/users/disks | JA |
| labs/users/environments | JA |
| labs/users/secrets | JA |
| labs/users/servicefabrics | JA |
| labs/users/servicefabrics/schedules | JA |
| labs/virtualMachines | JA | 
| labs/virtualmachines/schedules | JA |
| labs/virtualnetworks | JA |
| schedules | JA | 

## <a name="dynamics-lcs"></a>Dynamics LCS
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| lcsprojects | Nein  | 
| lcsprojects/connectors | Nein  | 
| lcsprojects/clouddeployments | Nein  | 

## <a name="event-grid"></a>Event Grid
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| domains | JA | 
| domains/topics | Nein  | 
| eventSubscriptions | Nein  | 
| extensionTopics | Nein  | 
| topics | JA | 
| topicTypes | Nein  | 

## <a name="event-hub"></a>Event Hub
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| clusters | JA | 
| namespaces | JA | 
| namespaces/AuthorizationRules | Nein  |
| namespaces/disasterRecoveryConfigs | Nein  |
| namespaces/eventhubs | Nein  |
| namespaces/eventhubs/authorizationRules | Nein  |
| namespaces/eventhubs/consumergroups | Nein  |

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
| actionGroups | JA |
| activityLogAlerts | JA |
| alertrules | JA |
| automatedExportSettings | Nein  | 
| components | JA | 
| components/events | Nein  | 
| components/metrics | Nein  | 
| components/pricingPlans | Nein  | 
| components/query | Nein  | 
| Protokolle | Nein  | 
| metricAlerts | JA |
| migrateToNewPricingModel | Nein  | 
| myWorkbooks | Nein  | 
| Abfragen | Nein  | 
| rollbackToLegacyPricingModel | Nein  | 
| scheduledqueryrules | JA | 
| webtests | JA | 
| workbooks | JA | 

## <a name="key-vault"></a>Key Vault
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| deletedVaults | Nein  | 
| Tresore | JA | 
| vaults/accessPolicies | Nein  | 
| vaults/secrets | Nein  | 

## <a name="log-analytics"></a>Log Analytics
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| Protokolle | Nein  | 

## <a name="logic"></a>Logik
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| integrationAccounts | JA | 
| Workflows | JA | 

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
| mediaservices/accountFilters | Nein  | 
| mediaservices/assets | Nein  | 
| mediaservices/assets/assetFilters | Nein  | 
| mediaservices/contentKeyPolicies | Nein  | 
| mediaservices/eventGridFilters | Nein  | 
| mediaservices/liveEventOperations | Nein  | 
| mediaservices/liveEvents | JA | 
| mediaservices/liveEvents/liveOutputs | Nein  | 
| mediaservices/liveOutputOperations | Nein  | 
| mediaservices/streamingEndpoints | JA | 
| mediaservices/streamingEndpointOperations | Nein  | 
| mediaservices/streamingLocators | Nein  | 
| mediaservices/streamingPolicies | Nein  | 
| mediaservices/transforms | Nein  | 
| mediaservices/transforms/jobs | Nein  | 

## <a name="network"></a>Netzwerk
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| applicationGateways | JA | 
| applicationSecurityGroups | JA | 
| azureFirewalls | JA | 
| connections | JA | 
| ddosProtectionPlans | JA | 
| expressRouteCircuits | JA | 
| frontdoors | JA | 
| frontdoorWebApplicationFirewallPolicies | JA | 
| interfaceEndpoints | JA | 
| loadBalancers | JA | 
| localNetworkGateways | JA | 
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
| virtualHubs | JA | 
| virtualNetworks | JA | 
| virtualNetworkGateways | JA | 
| virtualNetworkTaps | JA | 
| virtualWans | JA | 
| vpnGateways | JA | 
| vpnSites | JA | 
| webApplicationFirewallPolicies | JA | 

## <a name="notification-hubs"></a>Notification Hubs
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| namespaces | JA | 
| namespaces/notificationHubs | JA | 

## <a name="operational-insights"></a>Operational Insights
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| workspaces | JA |
| workspaces/dataSources | JA |
| workspaces/linkedServices | JA |
| workspaces/savedSearches | Nein  |
| workspaces/storageInsightConfigs | JA |

## <a name="operations-management"></a>Operations Management
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| solutions | Nein  |

## <a name="portal"></a>Portal
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| dashboards | JA | 

## <a name="portal-sdk"></a>Portal SDK
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| rootResources | JA | 

## <a name="power-bi"></a>Power BI
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| workspaceCollections | JA | 

## <a name="recovery-services"></a>Recovery Services
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| backupProtectedItems | Nein  | 
| Tresore | JA | 

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
| resourceHealthMetadata | Nein  | 
| searchServices | JA | 

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

## <a name="sql"></a>SQL
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| locations/instanceFailoverGroups | Nein  |
| managedInstances | JA |
| managedInstances/databases | JA |
| managedInstances/databases/backupShortTermRetentionPolicies | Nein  |
| managedInstances/databases/schemas/tables/columns/sensitivityLabels | Nein  |
| managedInstances/databases/vulnerabilityAssessments | Nein  |
| managedInstances/databases/vulnerabilityAssessments/rules/baselines | Nein  |
| managedInstances/encryptionProtector | Nein  |
| managedInstances/keys | Nein  |
| managedInstances/restorableDroppedDatabases/backupShortTermRetentionPolicies | Nein  |
| managedInstances/vulnerabilityAssessments | Nein  |
| servers | JA |
| servers/administrators | Nein  |
| servers/advisors | Nein  |
| servers/auditingSettings | Nein  |
| servers/backupLongTermRetentionVaults | Nein  |
| servers/communicationLinks | Nein  |
| servers/connectionPolicies | Nein  |
| servers/databases | JA |
| servers/databases/advisors | Nein  |
| servers/databases/auditingSettings | Nein  |
| servers/databases/backupLongTermRetentionPolicies | Nein  |
| servers/databases/backupShortTermRetentionPolicies | Nein  |
| servers/databases/connectionPolicies | Nein  |
| servers/databases/dataMaskingPolicies | Nein  |
| servers/databases/dataMaskingPolicies/rules | Nein  |
| servers/databases/extendedAuditingSettings | Nein  |
| servers/databases/extensions | Nein  |
| servers/databases/geoBackupPolicies | Nein  |
| servers/databases/schemas/tables/columns/sensitivityLabels | Nein  |
| servers/databases/securityAlertPolicies | Nein  |
| servers/databases/syncGroups | Nein  |
| servers/databases/syncGroups/syncMembers | Nein  |
| servers/databases/transparentDataEncryption | Nein  |
| servers/databases/vulnerabilityAssessments | Nein  |
| servers/databases/vulnerabilityAssessments/rules/baselines | Nein  |
| servers/disasterRecoveryConfiguration | Nein  |
| servers/dnsAliases | Nein  |
| servers/elasticPools | JA |
| servers/encryptionProtector | Nein  |
| servers/extendedAuditingSettings | Nein  |
| servers/failoverGroups | JA |
| servers/firewallRules | Nein  |
| servers/jobAgents | JA |
| servers/jobAgents/credentials | Nein  |
| servers/jobAgents/jobs | Nein  |
| servers/jobAgents/jobs/executions | Nein  |
| servers/jobAgents/jobs/steps | Nein  |
| servers/jobAgents/targetGroups | Nein  |
| servers/keys | Nein  |
| servers/securityAlertPolicies | Nein  |
| servers/syncAgents | Nein  |
| servers/virtualNetworkRules | Nein  |
| servers/vulnerabilityAssessments | Nein  |

## <a name="sql-virtual-machine"></a>Virtueller SQL-Computer
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| DWVM | JA | 

## <a name="storage"></a>Storage
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| storageAccounts | JA | 
| storageAccounts/blobServices | Nein  | 
| storageAccounts/fileServices | Nein  | 
| storageAccounts/queueServices | Nein  | 
| storageAccounts/services | Nein  | 
| storageAccounts/services/metricDefinitions | Nein  | 
| storageAccounts/tableServices | Nein  | 

## <a name="storage-sync"></a>Speichersynchronisierung
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| storageSyncServices | JA | 
| storageSyncServices/registeredServers | Nein  | 
| storageSyncServices/syncGroups | Nein  | 
| storageSyncServices/syncGroups/cloudEndpoints | Nein  | 
| storageSyncServices/syncGroups/serverEndpoints | Nein  | 
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
| account/extension | JA | 
| account/project | JA | 

## <a name="web"></a>Web
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
| hostingEnvironments/metrics | Nein  | 
| hostingEnvironments/multiRolePools | Nein  | 
| hostingEnvironments/workerPools | Nein  | 
| publishingUsers | Nein  | 
| serverFarms | JA | 
| serverFarms/workers | Nein  | 
| sites | JA | 
| sites/domainOwnershipIdentifiers | Nein  | 
| sites/hostNameBindings | Nein  | 
| sites/instances | Nein  | 
| sites/instances/extensions | Nein  | 
| sites/metrics | Nein  | 
| sites/premieraddons | JA | 
| sites/slots | JA | 
| sites/slots/hostNameBindings | Nein  | 
| sites/slots/instances | Nein  | 
| sites/slots/instances/extensions | Nein  | 
| sites/slots/metrics | Nein  | 
| sourceControls | Nein  | 
| validate | Nein  | 
| verifyHostingEnvironmentVnet | Nein  | 

## <a name="xrm"></a>XRM
| Ressourcentyp | Tagunterstützung |
| ------------- | ----------- |
| organizations | Nein  | 

## <a name="next-steps"></a>Nächste Schritte
Informationen zum Anwenden von Tags auf Ressourcen finden Sie unter [Verwenden von Tags zum Organisieren von Azure-Ressourcen](resource-group-using-tags.md).
