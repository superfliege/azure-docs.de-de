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
ms.openlocfilehash: dfed8ce93a97b31380b44f0ac7057c0cff60901e
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55497280"
---
# <a name="move-operation-support-for-resources"></a>Unterstützung des Verschiebevorgangs für Ressourcen

In diesem Artikel wird aufgeführt, für welche Azure-Ressourcentypen der Verschiebevorgang unterstützt wird. Auch wenn ein Ressourcentyp den Verschiebevorgang unterstützt, kann dies bestimmten Bedingungen unterliegen, die das Verschieben der Ressource verhindern. Ausführliche Informationen über Bedingungen, die sich auf Verschiebevorgänge auswirken, finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](resource-group-move-resources.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="find-resource-provider-and-resource-type"></a>Suchen des Ressourcenanbieters und Ressourcentyps

Sie müssen zunächst den entsprechenden Ressourcenanbieter und -typ ermitteln, um bestimmen zu können, ob eine Ressource verschoben werden kann.

Verwenden Sie für PowerShell Folgendes:

```azurepowershell-interactive
Get-AzResource -ResourceGroupName demogroup | Select Name, ResourceType | Format-table
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
| servers | Ja | Ja |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | --------------- | ----------- |
| service | Ja | Ja |

## <a name="microsoftauthorization"></a>Microsoft.Authorization
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | --------------- | ----------- |
| policyassignments | Nein  | Nein  |

## <a name="microsoftautomation"></a>Microsoft.Automation
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| automationaccounts | Ja | Ja |
| automationaccounts/configurations | Ja | Ja |
| automationaccounts/runbooks | Ja | Ja |

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| b2cdirectories | Ja | Ja |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| registrations | Ja | Ja |

## <a name="microsoftbackup"></a>Microsoft.Backup
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| backupvault | Nein  | Nein  |

## <a name="microsoftbatch"></a>Microsoft.Batch
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| batchaccounts | Ja | Ja |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| mapapis | Nein  | Nein  |

## <a name="microsoftbiztalkservices"></a>Microsoft.BizTalkServices
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| biztalk | Ja | Ja |

## <a name="microsoftblueprint"></a>Microsoft.Blueprint
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| blueprintassignments | Nein  | Nein  |

## <a name="microsoftbotservice"></a>Microsoft.BotService
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| botservices | Ja | Ja |

## <a name="microsoftcache"></a>Microsoft.Cache
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| redis | Ja | Ja |

## <a name="microsoftcdn"></a>Microsoft.Cdn
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| profiles | JA | Ja |
| profiles/endpoints | Ja | Ja |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| certificateorders | Ja | Ja |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| domainnames | Ja | Nein  |
| virtualmachines | Ja | Nein  |

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| networksecuritygroups | Nein  | Nein  |
| reservedips | Nein  | Nein  |
| virtualnetworks | Nein  | Nein  |

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| storageaccounts | Ja | Nein  |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| accounts | Ja | Ja |

## <a name="microsoftcompute"></a>Microsoft.Compute
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| availabilitysets | Ja | Ja |
| disks | Ja | Ja |
| galleries | Nein  | Nein  |
| galleries/images | Nein  | Nein  |
| galleries/images/versions | Nein  | Nein  |
| images | Ja | Ja |
| restorepointcollections | Nein  | Nein  |
| sharedvmimages | Nein  | Nein  |
| sharedvmimages/versions | Nein  | Nein  |
| snapshots | Ja | Ja |
| virtualmachines | Ja | Ja |
| virtualmachines/extensions | Ja | Ja |
| virtualmachinescalesets | Ja | Ja |

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
| registries | Ja | Ja |
| registries/buildtasks | Ja | Ja |
| registries/replications | Nein  | Nein  |
| registries/tasks | Ja | Ja |
| registries/webhooks | Ja | Ja |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| containerservices | Nein  | Nein  |
| managedclusters | Nein  | Nein  |
| openshiftmanagedclusters | Nein  | Nein  |

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| applications | JA | Ja |

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| connectors | JA | Ja |

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| hubs | Ja | Ja |

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
| catalogs | Ja | Ja |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| datafactories | Ja | Ja |
| factories | Ja | Ja |

## <a name="microsoftdatalake"></a>Microsoft.DataLake
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| datalakeaccounts | Nein  | Nein  |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| accounts | Ja | Ja |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| accounts | Ja | Ja |

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
| servers | Ja | Ja |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| servergroups | Nein  | Nein  |
| servers | Ja | Ja |

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
| iothubs | Ja | Ja |
| provisioningservices | Ja | Ja |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| labcenters | Nein  | Nein  |
| labs | Ja | Nein  |
| labs/servicerunners | Ja | Ja |
| labs/virtualmachines | Ja | Nein  |
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
| databaseaccounts | Ja | Ja |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| domains | Ja | Ja |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| topics | Ja | Ja |

## <a name="microsofteventhub"></a>Microsoft.EventHub
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| clusters | Ja | Ja |
| namespaces | Ja | Ja |

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| hanainstances | Ja | Ja |

## <a name="microsofthardwaresecuritymodules"></a>Microsoft.HardwareSecurityModules
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| dedicatedhsms | Nein  | Nein  |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| clusters | Ja | Ja |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| jobs | Ja | Ja |

## <a name="microsoftinsights"></a>microsoft.insights
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| actiongroups | Ja | Ja |
| activitylogalerts | Nein  | Nein  |
| alertrules | Ja | Ja |
| autoscalesettings | Ja | Ja |
| components | Ja | Ja |
| metricalerts | Nein  | Nein  |
| scheduledqueryrules | Ja | Ja |
| webtests | Ja | Ja |
| workbooks | Ja | Ja |

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| iotapps | Ja | Ja |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| vaults | Ja | Ja |

## <a name="microsoftkusto"></a>Microsoft.Kusto
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| clusters | Ja | Ja |

## <a name="microsoftlabservices"></a>Microsoft.LabServices
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| labaccounts | Ja | Ja |

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| accounts | Ja | Ja |

## <a name="microsoftlocationservices"></a>Microsoft.LocationServices
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| accounts | Ja | Ja |

## <a name="microsoftlogic"></a>Microsoft.Logic
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| integrationaccounts | Ja | Ja |
| workflows | Ja | Ja |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| commitmentplans | Ja | Ja |
| webservices | Ja | Nein  |
| workspaces | Ja | Ja |

## <a name="microsoftmachinelearningcompute"></a>Microsoft.MachineLearningCompute
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| operationalizationclusters | Ja | Ja |

## <a name="microsoftmachinelearningexperimentation"></a>Microsoft.MachineLearningExperimentation
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| accounts | Ja | Ja |
| accounts/workspaces | Ja | Ja |
| accounts/workspaces/projects | Ja | Ja |
| teamaccounts | Ja | Ja |
| teamaccounts/workspaces | Ja | Ja |
| teamaccounts/workspaces/projects | Ja | Ja |

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| accounts | Ja | Ja |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| workspaces | Ja | Ja |

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| userassignedidentities | Ja | Ja |

## <a name="microsoftmaps"></a>Microsoft.Maps
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| accounts | Ja | Ja |

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| classicdevservices | Nein  | Nein  |

## <a name="microsoftmedia"></a>Microsoft.Media
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| mediaservices | Ja | Ja |
| mediaservices/liveevents | Ja | Ja |
| mediaservices/streamingendpoints | Ja | Ja |

## <a name="microsoftmigrate"></a>Microsoft.Migrate
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| projects | Nein  | Nein  |

## <a name="microsoftnetwork"></a>Microsoft.Network
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| applicationgateways | Nein  | Nein  |
| applicationsecuritygroups | Ja | Ja |
| azurefirewalls | Nein  | Nein  |
| connections | Ja | Ja |
| ddosprotectionplans | Nein  | Nein  |
| dnszones | Ja | Ja |
| expressroutecircuits | Nein  | Nein  |
| expressroutecrossconnections | Nein  | Nein  |
| expressroutegateways | Nein  | Nein  |
| expressrouteports | Nein  | Nein  |
| frontdoors | Ja | Ja |
| frontdoorwebapplicationfirewallpolicies | Ja | Ja |
| interfaceendpoints | Nein  | Nein  |
| loadbalancers | Ja | Ja |
| localnetworkgateways | Ja | Ja |
| networkintentpolicies | Ja | Ja |
| networkinterfaces | Ja | Ja |
| networkprofiles | Nein  | Nein  |
| networksecuritygroups | Ja | Ja |
| networkwatchers | Ja | Ja |
| networkwatchers/connectionmonitors | Ja | Ja |
| networkwatchers/lenses | Ja | Ja |
| networkwatchers/pingmeshes | Ja | Ja |
| publicipaddresses | Ja | Ja |
| publicipprefixes | Ja | Ja |
| routefilters | Nein  | Nein  |
| routetables | Ja | Ja |
| serviceendpointpolicies | Ja | Ja |
| trafficmanagerprofiles | Ja | Ja |
| virtualhubs | Ja | Ja |
| virtualnetworkgateways | Ja | Ja |
| virtualnetworks | Ja | Ja |
| virtualnetworktaps | Nein  | Nein  |
| virtualwans | Ja | Ja |
| vpngateways | Ja | Ja |
| vpnsites | Ja | Ja |
| webapplicationfirewallpolicies | Ja | Ja |

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| namespaces | Ja | Ja |
| namespaces/notificationhubs | Ja | Ja |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| workspaces | Ja | Ja |

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| managementconfigurations | Ja | Ja |
| solutions | Ja | JA |
| views | JA | Ja |

## <a name="microsoftportal"></a>Microsoft.Portal
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| dashboards | Ja | Ja |

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| workspacecollections | Ja | Ja |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| capacities | Ja | Ja |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| vaults | Ja | Ja |

## <a name="microsoftrelay"></a>Microsoft.Relay
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| namespaces | Ja | Ja |

## <a name="microsoftsaas"></a>Microsoft.SaaS
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| applications | JA | Nein  |

## <a name="microsoftscheduler"></a>Microsoft.Scheduler
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| flows | Ja | Ja |
| jobcollections | Ja | Ja |

## <a name="microsoftsearch"></a>Microsoft.Search
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| searchservices | Ja | Ja |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| namespaces | Ja | Ja |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| clusters | Ja | Ja |

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| applications | JA | JA |
| networks | Ja | Ja |
| volumes | Ja | Ja |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| signalr | Ja | Ja |

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
| managedinstances | Ja | Ja |
| managedinstances/databases | Ja | Ja |
| servers | Ja | Ja |
| servers/databases | Ja | Ja |
| servers/elasticpools | Ja | Ja |
| virtualclusters | Ja | Ja |

## <a name="microsoftstorage"></a>Microsoft.Storage
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| storageaccounts | Ja | Ja |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| storagesyncservices | Ja | Ja |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| managers | Nein  | Nein  |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| streamingjobs | Ja | Ja |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| environments | Ja | Ja |
| environments/eventsources | Ja | Ja |
| environments/referencedatasets | Ja | Ja |

## <a name="microsoftvisualstudio"></a>microsoft.visualstudio
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| account | JA | Ja |
| account/extension | Ja | Ja |
| account/project | Ja | Ja |

## <a name="microsoftweb"></a>Microsoft.Web
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| certificates | Nein  | Ja |
| classicmobileservices | Nein  | Nein  |
| connectiongateways | Ja | Ja |
| connections | Ja | Ja |
| customapis | Ja | Ja |
| hostingenvironments | Nein  | Nein  |
| serverfarms | Ja | Ja |
| sites | Ja | Ja |
| sites/premieraddons | Ja | Ja |
| sites/slots | Ja | Ja |

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT
| Ressourcentyp | Ressourcengruppe | Abonnement |
| ------------- | -------------- | ------------ |
| deviceservices | Ja | Ja |


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
