---
title: Upgraden der Service Fabric-Runtime in Azure | Microsoft-Dokumentation
description: In diesem Tutorial wird beschrieben, wie Sie mit PowerShell die Runtime eines in Azure gehosteten Service Fabric-Clusters aktualisieren.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/28/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: e34856704b474ac1b7d25d589be5f98be2b113cc
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/22/2019
ms.locfileid: "56671555"
---
# <a name="tutorial-upgrade-the-runtime-of-a-service-fabric-cluster-in-azure"></a>Tutorial: Upgraden der Runtime eines Service Fabric-Clusters in Azure

Dieses Tutorial ist der dritte Teil einer Reihe und zeigt, wie Sie die Service Fabric-Runtime in einem Azure Service Fabric-Cluster aktualisieren. Dieser Tutorialteil wurde für unter Azure ausgeführte Service Fabric-Cluster geschrieben und gilt nicht für eigenständige Service Fabric-Cluster.

> [!WARNING]
> Für diesen Teil des Tutorials ist PowerShell erforderlich. Das Upgrade der Clusterruntime wird von den Azure CLI-Tools noch nicht unterstützt. Alternativ kann ein Cluster auch im Portal aktualisiert werden. Weitere Informationen finden Sie unter [Aktualisieren eines Azure Service Fabric-Clusters](service-fabric-cluster-upgrade.md).

Wenn im Cluster bereits die neueste Service Fabric-Runtime ausgeführt wird, müssen Sie diesen Schritt nicht ausführen. Dieser Artikel kann jedoch zum Installieren aller unterstützten Runtimes in einem Azure Service Fabric-Cluster herangezogen werden.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Lesen der Clusterversion
> * Festlegen der Clusterversion

In dieser Tutorialserie lernen Sie Folgendes:
> [!div class="checklist"]
> * Erstellen eines sicheren [Windows-Clusters](service-fabric-tutorial-create-vnet-and-windows-cluster.md) in Azure mithilfe einer Vorlage
> * [Horizontales Herunter- oder Hochskalieren eines Clusters](service-fabric-tutorial-scale-cluster.md)
> * Aktualisieren der Runtime eines Service Fabric-Clusters
> * [Löschen eines Clusters](service-fabric-tutorial-delete-cluster.md)

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit diesem Tutorial beginnen können, müssen Sie Folgendes tun:

* Wenn Sie kein Azure-Abonnement besitzen, erstellen Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Installieren Sie das [Azure PowerShell-Modul Version 4.1 oder höher](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps) oder [Azure CLI](/cli/azure/install-azure-cli).
* Erstellen eines sicheren [Windows-Clusters](service-fabric-tutorial-create-vnet-and-windows-cluster.md) in Azure
* Einrichten einer Windows-Entwicklungsumgebung Installieren Sie [Visual Studio 2017](https://www.visualstudio.com) und die Workloads für **Azure-Entwicklung**, **ASP.NET und Webentwicklung** und **Plattformübergreifende .NET Core-Entwicklung**.  Richten Sie dann eine [.NET-Entwicklungsumgebung](service-fabric-get-started.md) ein.

### <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich bei Ihrem Azure-Konto an, und wählen Sie Ihr Abonnement aus, bevor Sie Azure-Befehle ausführen.

```powershell
Connect-AzureRmAccount
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

## <a name="get-the-runtime-version"></a>Abrufen der Runtimeversion

Nachdem Sie eine Verbindung mit Azure hergestellt und das Abonnement mit dem Service Fabric-Cluster ausgewählt haben, können Sie die Runtimeversion des Clusters abrufen.

```powershell
Get-AzureRmServiceFabricCluster -ResourceGroupName SFCLUSTERTUTORIALGROUP -Name aztestcluster `
    | Select-Object ClusterCodeVersion
```

Sie können auch einfach folgendermaßen eine Liste aller Cluster in Ihrem Abonnement abrufen:

```powershell
Get-AzureRmServiceFabricCluster | Select-Object Name, ClusterCodeVersion
```

Notieren Sie den Wert **ClusterCodeVersion**. Dieser Wert wird im nächsten Abschnitt verwendet.

## <a name="upgrade-the-runtime"></a>Aktualisieren der Runtime

Verwenden Sie den Wert **ClusterCodeVersion** aus dem vorherigen Abschnitt mit den Cmdlet `Get-ServiceFabricRuntimeUpgradeVersion`, um zu ermitteln, welche Versionen für ein Upgrade verfügbar sind. Dieses Cmdlet kann nur auf einem Computer ausgeführt werden, der mit dem Internet verbunden ist. Wenn Sie beispielsweise erfahren möchten, auf welche Runtimeversionen Sie von Version `5.7.198.9494` aktualisieren können, verwenden Sie den folgenden Befehl:

```powershell
Get-ServiceFabricRuntimeUpgradeVersion -BaseVersion "5.7.198.9494"
```

Mit einer Liste von Versionen können Sie den Azure Service Fabric-Cluster anweisen, ein Upgrade auf eine neuere Runtime durchzuführen. Wenn beispielsweise Version `6.0.219.9494` für ein Upgrade verfügbar ist, verwenden Sie den folgenden Befehl zum Aktualisieren des Clusters.

```powershell
Set-AzureRmServiceFabricUpgradeType -ResourceGroupName SFCLUSTERTUTORIALGROUP `
                                    -Name aztestcluster `
                                    -UpgradeMode Manual `
                                    -Version "6.0.219.9494"
```

> [!IMPORTANT]
> Das Upgrade der Clusterruntime kann eine lange Zeit dauern. PowerShell ist blockiert, während das Upgrade ausgeführt wird. Sie können eine andere PowerShell-Sitzung verwenden, um den Status des Upgrades zu überprüfen.

Der Status des Upgrades kann mit PowerShell oder der Azure Service Fabric-Befehlszeilenschnittstelle (sfctl) überwacht werden.

Stellen Sie zunächst mit dem im ersten Teil des Tutorials erstellten SSL-Zertifikat eine Verbindung mit dem Cluster her. Verwenden Sie das Cmdlet `Connect-ServiceFabricCluster` oder `sfctl cluster upgrade-status`.

```powershell
$endpoint = "<mycluster>.southcentralus.cloudapp.azure.com:19000"
$thumbprint = "63EB5BA4BC2A3BADC42CA6F93D6F45E5AD98A1E4"

Connect-ServiceFabricCluster -ConnectionEndpoint $endpoint `
                             -KeepAliveIntervalInSec 10 `
                             -X509Credential -ServerCertThumbprint $thumbprint `
                             -FindType FindByThumbprint -FindValue $thumbprint `
                             -StoreLocation CurrentUser -StoreName My
```

```azurecli
sfctl cluster select --endpoint https://aztestcluster.southcentralus.cloudapp.azure.com:19080 \
--pem ./aztestcluster201709151446.pem --no-verify
```

Verwenden Sie als Nächstes `Get-ServiceFabricClusterUpgrade` oder `sfctl cluster upgrade-status`, um den Status anzuzeigen. Das Ergebnis ähnelt dem folgenden.

```powershell
Get-ServiceFabricClusterUpgrade

TargetCodeVersion                          : 6.0.219.9494
TargetConfigVersion                        : 3
StartTimestampUtc                          : 11/28/2017 3:09:48 AM
UpgradeState                               : RollingForwardPending
UpgradeDuration                            : 00:09:00
CurrentUpgradeDomainDuration               : 00:09:00
NextUpgradeDomain                          : 1
UpgradeDomainsStatus                       : { "0" = "Completed";
                                             "1" = "Pending";
                                             "2" = "Pending";
                                             "3" = "Pending";
                                             "4" = "Pending" }
UpgradeKind                                : Rolling
RollingUpgradeMode                         : Monitored
FailureAction                              : Rollback
ForceRestart                               : False
UpgradeReplicaSetCheckTimeout              : 37201.09:59:01
HealthCheckWaitDuration                    : 00:05:00
HealthCheckStableDuration                  : 00:05:00
HealthCheckRetryTimeout                    : 00:45:00
UpgradeDomainTimeout                       : 02:00:00
UpgradeTimeout                             : 12:00:00
ConsiderWarningAsError                     : False
MaxPercentUnhealthyApplications            : 0
MaxPercentUnhealthyNodes                   : 100
ApplicationTypeHealthPolicyMap             : {}
EnableDeltaHealthEvaluation                : True
MaxPercentDeltaUnhealthyNodes              : 0
MaxPercentUpgradeDomainDeltaUnhealthyNodes : 0
ApplicationHealthPolicyMap                 : {}
```

```azurecli
sfctl cluster upgrade-status

{
  "codeVersion": "6.0.219.9494",
  "configVersion": "3",

... item cut to save space ...

  },
  "upgradeDomains": [
    {
      "name": "0",
      "state": "Completed"
    },
    {
      "name": "1",
      "state": "Pending"
    },
    {
      "name": "2",
      "state": "Pending"
    },
    {
      "name": "3",
      "state": "Pending"
    },
    {
      "name": "4",
      "state": "Pending"
    }
  ],
  "upgradeDurationInMilliseconds": "PT1H2M4.63889S",
  "upgradeState": "RollingForwardPending"
}
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Abrufen der Version der Clusterruntime
> * Aktualisieren der Clusterruntime
> * Überwachen des Upgrades

