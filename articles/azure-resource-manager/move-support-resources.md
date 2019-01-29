---
title: Unterstützung des Verschiebevorgangs je nach Azure-Ressourcentyp | Microsoft-Dokumentation
description: Eine Liste von Azure-Ressourcentypen, die in eine neue Ressourcengruppe oder ein neues Abonnement verschoben werden können.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: tomfitz
ms.openlocfilehash: c16a0eeb674e712ec1c3678a2e0002a8ddcfc329
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54464705"
---
# <a name="move-operation-support-for-resources"></a>Unterstützung des Verschiebevorgangs für Ressourcen

In diesem Artikel wird aufgeführt, für welche Azure-Ressourcentypen der Verschiebevorgang unterstützt wird. Auch wenn ein Ressourcentyp den Verschiebevorgang unterstützt, kann dies bestimmten Bedingungen unterliegen, die das Verschieben der Ressource verhindern. Ausführliche Informationen über Bedingungen, die sich auf Verschiebevorgänge auswirken, finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](resource-group-move-resources.md).

## <a name="find-resource-provider-and-resource-type"></a>Suchen des Ressourcenanbieters und Ressourcentyps

Sie müssen zunächst den entsprechenden Ressourcenanbieter und -typ ermitteln, um bestimmen zu können, ob eine Ressource verschoben werden kann.

Verwenden Sie für PowerShell Folgendes:

```azurepowershell-interactive
Get-AzureRmResource -ResourceGroupName demogroup | Select Name, ResourceType | Format-table
```

Verwenden Sie für die Azure-Befehlszeilenschnittstelle den folgenden Befehl:

```azurecli-interactive
az resource list -g demogroup --query '[].{name:name, resourceType:type}' --output table
```

Der Ressourcentyp wird im Format `<resource-provider>/<resource-type-name>` zurückgegeben. Der Wert `Microsoft.OperationalInsights/workspaces` verfügt also über den Ressourcenanbieter **Microsoft.OperationalInsights** und weist einen Ressourcentyp namens **workspaces** auf.

Wenn Sie den Ressourcenanbieter und den Ressourcentyp gefunden haben, verwenden Sie die Tabellen in diesem Artikel, um zu ermitteln, ob der Verschiebevorgang für den jeweiligen Ressourcentyp unterstützt wird.

## <a name="microsoftaad"></a>Microsoft.AAD

| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | --------------- | ----------- |
| domainservices | Nein  | Nein  |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| servers | JA | JA |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | --------------- | ----------- |
| service | JA | JA |

## <a name="microsoftauthorization"></a>Microsoft.Authorization
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | --------------- | ----------- |
| policyassignments | Nein  | Nein  |

## <a name="microsoftautomation"></a>Microsoft.Automation
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| automationaccounts | JA | JA |
| automationaccounts/configurations | JA | JA |
| automationaccounts/runbooks | JA | JA |

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| b2cdirectories | JA | JA |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| registrations | JA | JA |

## <a name="microsoftbackup"></a>Microsoft.Backup
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| backupvault | Nein  | Nein  |

## <a name="microsoftbatch"></a>Microsoft.Batch
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| batchaccounts | JA | JA |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| mapapis | Nein  | Nein  |

## <a name="microsoftbiztalkservices"></a>Microsoft.BizTalkServices
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| biztalk | JA | JA |

## <a name="microsoftblueprint"></a>Microsoft.Blueprint
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| blueprintassignments | Nein  | Nein  |

## <a name="microsoftbotservice"></a>Microsoft.BotService
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| botservices | JA | JA |

## <a name="microsoftcache"></a>Microsoft.Cache
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| redis | JA | JA |

## <a name="microsoftcdn"></a>Microsoft.Cdn
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| profiles | JA | JA |
| profiles/endpoints | JA | JA |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| certificateorders | JA | JA |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| domainnames | JA | Nein  |
| virtualmachines | JA | Nein  |

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| networksecuritygroups | Nein  | Nein  |
| reservedips | Nein  | Nein  |
| virtualnetworks | Nein  | Nein  |

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| storageaccounts | JA | Nein  |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| accounts | JA | JA |

## <a name="microsoftcompute"></a>Microsoft.Compute
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| availabilitysets | JA | JA |
| disks | JA | JA |
| galleries | Nein  | Nein  |
| galleries/images | Nein  | Nein  |
| galleries/images/versions | Nein  | Nein  |
| images | JA | JA |
| restorepointcollections | Nein  | Nein  |
| sharedvmimages | Nein  | Nein  |
| sharedvmimages/versions | Nein  | Nein  |
| snapshots | JA | JA |
| virtualmachines | JA | JA |
| virtualmachines/extensions | JA | JA |
| virtualmachinescalesets | JA | JA |

## <a name="microsoftcontainer"></a>Microsoft.Container
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| containergroups | Nein  | Nein  |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| containergroups | Nein  | Nein  |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| registries | JA | JA |
| registries/buildtasks | JA | JA |
| registries/replications | Nein  | Nein  |
| registries/tasks | JA | JA |
| registries/webhooks | JA | JA |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| containerservices | Nein  | Nein  |
| managedclusters | Nein  | Nein  |
| openshiftmanagedclusters | Nein  | Nein  |

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| applications | JA | JA |

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| connectors | JA | JA |

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| hubs | JA | JA |

## <a name="microsoftdatabox"></a>Microsoft.DataBox
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| jobs | Nein  | Nein  |

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| databoxedgedevices | Nein  | Nein  |

## <a name="microsoftdatabricks"></a>Microsoft.Databricks
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| workspaces | Nein  | Nein  |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| catalogs | JA | JA |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| datafactories | JA | JA |
| factories | JA | JA |

## <a name="microsoftdatalake"></a>Microsoft.DataLake
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| datalakeaccounts | Nein  | Nein  |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| accounts | JA | JA |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| accounts | JA | JA |

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| services | Nein  | Nein  |
| services/projects | Nein  | Nein  |
| slots | Nein  | Nein  |

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| servers | Nein  | Nein  |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| servers | JA | JA |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| servergroups | Nein  | Nein  |
| servers | JA | JA |

## <a name="microsoftdeploymentmanager"></a>Microsoft.DeploymentManager
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| artifactsources | Nein  | Nein  |
| rollouts | Nein  | Nein  |
| servicetopologies | Nein  | Nein  |
| servicetopologies/services | Nein  | Nein  |
| servicetopologies/services/serviceunits | Nein  | Nein  |

## <a name="microsoftdevices"></a>Microsoft.Devices
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| iothubs | JA | JA |
| provisioningservices | JA | JA |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| labcenters | Nein  | Nein  |
| labs | JA | Nein  |
| labs/servicerunners | JA | JA |
| labs/virtualmachines | JA | Nein  |
| schedules | Nein  | Nein  |

## <a name="microsoftdns"></a>microsoft.dns
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| dnszones | Nein  | Nein  |
| dnszones/a | Nein  | Nein  |
| dnszones/aaaa | Nein  | Nein  |
| dnszones/cname | Nein  | Nein  |
| dnszones/mx | Nein  | Nein  |
| dnszones/ptr | Nein  | Nein  |
| dnszones/srv | Nein  | Nein  |
| dnszones/txt | Nein  | Nein  |
| trafficmanagerprofiles | Nein  | Nein  |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| databaseaccounts | JA | JA |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| domains | JA | JA |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| topics | JA | JA |

## <a name="microsofteventhub"></a>Microsoft.EventHub
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| clusters | JA | JA |
| namespaces | JA | JA |

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| hanainstances | JA | JA |

## <a name="microsofthardwaresecuritymodules"></a>Microsoft.HardwareSecurityModules
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| dedicatedhsms | Nein  | Nein  |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| clusters | JA | JA |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| jobs | JA | JA |

## <a name="microsoftinsights"></a>microsoft.insights
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| actiongroups | JA | JA |
| activitylogalerts | Nein  | Nein  |
| alertrules | JA | JA |
| autoscalesettings | JA | JA |
| components | JA | JA |
| metricalerts | Nein  | Nein  |
| scheduledqueryrules | JA | JA |
| webtests | JA | JA |
| workbooks | JA | JA |

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| iotapps | JA | JA |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| vaults | JA | JA |

## <a name="microsoftkusto"></a>Microsoft.Kusto
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| clusters | JA | JA |

## <a name="microsoftlabservices"></a>Microsoft.LabServices
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| labaccounts | JA | JA |

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| accounts | JA | JA |

## <a name="microsoftlocationservices"></a>Microsoft.LocationServices
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| accounts | JA | JA |

## <a name="microsoftlogic"></a>Microsoft.Logic
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| integrationaccounts | JA | JA |
| workflows | JA | JA |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| commitmentplans | JA | JA |
| webservices | JA | Nein  |
| workspaces | JA | JA |

## <a name="microsoftmachinelearningcompute"></a>Microsoft.MachineLearningCompute
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| operationalizationclusters | JA | JA |

## <a name="microsoftmachinelearningexperimentation"></a>Microsoft.MachineLearningExperimentation
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| accounts | JA | JA |
| accounts/workspaces | JA | JA |
| accounts/workspaces/projects | JA | JA |
| teamaccounts | JA | JA |
| teamaccounts/workspaces | JA | JA |
| teamaccounts/workspaces/projects | JA | JA |

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| accounts | JA | JA |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| workspaces | JA | JA |

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| userassignedidentities | JA | JA |

## <a name="microsoftmaps"></a>Microsoft.Maps
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| accounts | JA | JA |

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| classicdevservices | Nein  | Nein  |

## <a name="microsoftmedia"></a>Microsoft.Media
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| mediaservices | JA | JA |
| mediaservices/liveevents | JA | JA |
| mediaservices/streamingendpoints | JA | JA |

## <a name="microsoftmigrate"></a>Microsoft.Migrate
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| projects | Nein  | Nein  |

## <a name="microsoftnetwork"></a>Microsoft.Network
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| applicationgateways | Nein  | Nein  |
| applicationsecuritygroups | JA | JA |
| azurefirewalls | Nein  | Nein  |
| connections | JA | JA |
| ddosprotectionplans | Nein  | Nein  |
| dnszones | JA | JA |
| expressroutecircuits | Nein  | Nein  |
| expressroutecrossconnections | Nein  | Nein  |
| expressroutegateways | Nein  | Nein  |
| expressrouteports | Nein  | Nein  |
| frontdoors | JA | JA |
| frontdoorwebapplicationfirewallpolicies | JA | JA |
| interfaceendpoints | Nein  | Nein  |
| loadbalancers | JA | JA |
| localnetworkgateways | JA | JA |
| networkintentpolicies | JA | JA |
| networkinterfaces | JA | JA |
| networkprofiles | Nein  | Nein  |
| networksecuritygroups | JA | JA |
| networkwatchers | JA | JA |
| networkwatchers/connectionmonitors | JA | JA |
| networkwatchers/lenses | JA | JA |
| networkwatchers/pingmeshes | JA | JA |
| publicipaddresses | JA | JA |
| publicipprefixes | JA | JA |
| routefilters | Nein  | Nein  |
| routetables | JA | JA |
| serviceendpointpolicies | JA | JA |
| trafficmanagerprofiles | JA | JA |
| virtualhubs | JA | JA |
| virtualnetworkgateways | JA | JA |
| virtualnetworks | JA | JA |
| virtualnetworktaps | Nein  | Nein  |
| virtualwans | JA | JA |
| vpngateways | JA | JA |
| vpnsites | JA | JA |
| webapplicationfirewallpolicies | JA | JA |

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| namespaces | JA | JA |
| namespaces/notificationhubs | JA | JA |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| workspaces | JA | JA |

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| managementconfigurations | JA | JA |
| solutions | JA | JA |
| views | JA | JA |

## <a name="microsoftportal"></a>Microsoft.Portal
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| dashboards | JA | JA |

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| workspacecollections | JA | JA |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| capacities | JA | JA |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| vaults | JA | JA |

## <a name="microsoftrelay"></a>Microsoft.Relay
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| namespaces | JA | JA |

## <a name="microsoftsaas"></a>Microsoft.SaaS
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| applications | JA | Nein  |

## <a name="microsoftscheduler"></a>Microsoft.Scheduler
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| flows | JA | JA |
| jobcollections | JA | JA |

## <a name="microsoftsearch"></a>Microsoft.Search
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| searchservices | JA | JA |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| namespaces | JA | JA |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| clusters | JA | JA |

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| applications | JA | JA |
| networks | JA | JA |
| volumes | JA | JA |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| signalr | JA | JA |

## <a name="microsoftsiterecovery"></a>Microsoft.SiteRecovery
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| siterecoveryvault | Nein  | Nein  |

## <a name="microsoftsolutions"></a>Microsoft.Solutions
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| appliancedefinitions | Nein  | Nein  |
| appliances | Nein  | Nein  |
| applicationdefinitions | Nein  | Nein  |
| applications | Nein  | Nein  |

## <a name="microsoftsql"></a>Microsoft.Sql
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| managedinstances | JA | JA |
| managedinstances/databases | JA | JA |
| servers | JA | JA |
| servers/databases | JA | JA |
| servers/elasticpools | JA | JA |
| virtualclusters | JA | JA |

## <a name="microsoftstorage"></a>Microsoft.Storage
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| storageaccounts | JA | JA |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| storagesyncservices | JA | JA |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| managers | Nein  | Nein  |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| streamingjobs | JA | JA |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| environments | JA | JA |
| environments/eventsources | JA | JA |
| environments/referencedatasets | JA | JA |

## <a name="microsoftvisualstudio"></a>microsoft.visualstudio
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| account | JA | JA |
| account/extension | JA | JA |
| account/project | JA | JA |

## <a name="microsoftweb"></a>Microsoft.Web
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| certificates | Nein  | JA |
| classicmobileservices | Nein  | Nein  |
| connectiongateways | JA | JA |
| connections | JA | JA |
| customapis | JA | JA |
| hostingenvironments | Nein  | Nein  |
| serverfarms | JA | JA |
| sites | JA | JA |
| sites/premieraddons | JA | JA |
| sites/slots | JA | JA |

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| deviceservices | JA | JA |


## <a name="third-party-services"></a>Dienste von Drittanbietern

Derzeit wird der Verschiebevorgang nicht für Drittanbieterdienste unterstützt. Im Folgenden werden die Ressourcenanbieter aufgeführt:

* 84codes.CloudAMQP
* AppDynamics.APM
* Aspera.Transfers
* Auth0.Cloud
* Citrix.Cloud
* Citrix.Services
* CloudSimple.PrivateCloudIaaS
* Cloudyn.Analytics
* Conexlink.MyCloudIT
* Crypteron.DataSecurity
* Dynatrace.DynatraceSaaS
* Dynatrace.Ruxit
* LiveArena.Broadcast
* Lombiq.DotNest
* Mailjet.Email
* Myget.PackageManagement
* NewRelic.APM
* nuubit.nextgencdn
* Paraleap.CloudMonix
* Pokitdok.Platform
* RavenHq.Db
* Raygun.CrashReporting
* RevAPM.MobileCDN
* Sendgrid.Email
* Sparkpost.Basic
* stackify.retrace
* SuccessBricks.ClearDB
* TrendMicro.DeepSecurity
* U2uconsult.TheIdentityHub


## <a name="next-steps"></a>Nächste Schritte

* Befehle zum Verschieben von Ressourcen finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](resource-group-move-resources.md).
