---
title: Skalieren eines Service Fabric-Clusters in Azure | Microsoft-Dokumentation
description: Dieses Tutorial enthält Informationen zum schnellen Skalieren eines Service Fabric-Clusters in Azure.
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
ms.date: 010/01/2018
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 54433aa9e618bdde7badec3be8a786f943f88198
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51236912"
---
# <a name="tutorial-scale-a-service-fabric-cluster-in-azure"></a>Tutorial: Skalieren eines Service Fabric-Clusters in Azure

Dieses Tutorial ist der zweite Teil einer Reihe und zeigt, wie Sie Ihren vorhandenen Cluster horizontal hoch- und herunterskalieren. In diesem Tutorial erfahren Sie, wie Sie Ihren Cluster skalieren und gegebenenfalls zurückgebliebene Ressourcen bereinigen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Lesen der Anzahl von Clusterknoten
> * Hinzufügen von Clusterknoten (horizontales Hochskalieren)
> * Entfernen von Clusterknoten (horizontales Herunterskalieren)

In dieser Tutorialserie lernen Sie Folgendes:
> [!div class="checklist"]
> * Erstellen eines sicheren [Windows-Clusters](service-fabric-tutorial-create-vnet-and-windows-cluster.md) oder [Linux-Clusters](service-fabric-tutorial-create-vnet-and-linux-cluster.md) in Azure mithilfe einer Vorlage
> * Horizontales Herunter- oder Hochskalieren eines Clusters
> * [Aktualisieren der Runtime eines Clusters](service-fabric-tutorial-upgrade-cluster.md)
> * [Löschen eines Clusters](service-fabric-tutorial-delete-cluster.md)

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit diesem Tutorial beginnen können, müssen Sie Folgendes tun:

* Wenn Sie kein Azure-Abonnement besitzen, erstellen Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Installieren Sie das [Azure PowerShell-Modul Version 4.1 oder höher](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) oder [Azure CLI](/cli/azure/install-azure-cli).
* Erstellen Sie einen sicheren [Windows-Cluster](service-fabric-tutorial-create-vnet-and-windows-cluster.md) oder [Linux-Cluster](service-fabric-tutorial-create-vnet-and-linux-cluster.md) in Azure.
* Wenn Sie einen Windows-Cluster bereitstellen, richten Sie eine Windows-Entwicklungsumgebung ein. Installieren Sie [Visual Studio 2017](https://www.visualstudio.com) und die Workloads für **Azure-Entwicklung**, **ASP.NET und Webentwicklung** und **Plattformübergreifende .NET Core-Entwicklung**.  Richten Sie dann eine [.NET-Entwicklungsumgebung](service-fabric-get-started.md) ein.
* Wenn Sie einen Linux-Cluster bereitstellen, richten Sie eine Java-Entwicklungsumgebung unter [Linux](service-fabric-get-started-linux.md) oder [macOS](service-fabric-get-started-mac.md) ein.  Installieren Sie die [Service Fabric CLI](service-fabric-cli.md).

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich bei Ihrem Azure-Konto an, und wählen Sie Ihr Abonnement aus, bevor Sie Azure-Befehle ausführen.

```powershell
Connect-AzureRmAccount
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

```azurecli
az login
az account set --subscription <guid>
```

## <a name="connect-to-the-cluster"></a>Verbinden mit dem Cluster

Für diesen Teil des Tutorials müssen Sie eine Verbindung mit dem Service Fabric-Cluster sowie mit der VM-Skalierungsgruppe herstellen, die als Host für den Cluster fungiert. Bei der VM-Skalierungsgruppe handelt es sich um die Azure-Ressource, die Service Fabric in Azure hostet.

Wenn Sie eine Verbindung mit einem Cluster herstellen, können Sie Informationen aus dem Cluster abfragen. Über den Cluster können Sie ermitteln, welche Knoten dem Cluster bekannt sind. Für die Verbindungsherstellung mit dem Cluster wird im folgenden Code das Zertifikat verwendet, das im ersten Teil dieser Reihe erstellt wurde. Legen Sie die Variablen `$endpoint` und `$thumbprint` auf Ihre Werte fest.

```powershell
$endpoint = "<mycluster>.southcentralus.cloudapp.azure.com:19000"
$thumbprint = "63EB5BA4BC2A3BADC42CA6F93D6F45E5AD98A1E4"

Connect-ServiceFabricCluster -ConnectionEndpoint $endpoint `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint $thumbprint `
          -FindType FindByThumbprint -FindValue $thumbprint `
          -StoreLocation CurrentUser -StoreName My

Get-ServiceFabricClusterHealth
```

```azurecli
sfctl cluster select --endpoint https://aztestcluster.southcentralus.cloudapp.azure.com:19080 \
--pem ./aztestcluster201709151446.pem --no-verify
```

Nach dem Herstellen der Verbindung können Sie mithilfe eines Befehls den Status der einzelnen Knoten im Cluster abrufen. Verwenden Sie für **PowerShell** den Befehl `Get-ServiceFabricClusterHealth` und für **sfctl** den Befehl `sfctl cluster select`.

## <a name="scale-out"></a>Horizontales Skalieren

Beim horizontalen Hochskalieren werden der Skalierungsgruppe mehr VM-Instanzen hinzugefügt. Diese Instanzen werden zu den Knoten, die Service Fabric verwendet. Service Fabric erkennt, wenn der Skalierungsgruppe durch horizontales Hochskalieren weitere Instanzen hinzugefügt werden, und reagiert automatisch. Der folgende Code ruft eine Skalierungsgruppe anhand ihres Namens ab und erhöht die **Kapazität** der Skalierungsgruppe um 1:

```powershell
$scaleset = Get-AzureRmVmss -ResourceGroupName SFCLUSTERTUTORIALGROUP -VMScaleSetName nt1vm
$scaleset.Sku.Capacity += 1

Update-AzureRmVmss -ResourceGroupName $scaleset.ResourceGroupName -VMScaleSetName $scaleset.Name -VirtualMachineScaleSet $scaleset
```

Der folgende Code legt die Kapazität auf 6 fest:

```azurecli
# Get the name of the node with
az vmss list-instances -n nt1vm -g sfclustertutorialgroup --query [*].name

# Use the name to scale
az vmss scale -g sfclustertutorialgroup -n nt1vm --new-capacity 6
```

## <a name="scale-in"></a>Horizontales Herunterskalieren

Horizontales Herunterskalieren ist mit horizontalem Hochskalieren vergleichbar. In diesem Fall wird jedoch ein niedrigerer Wert für **Kapazität** verwendet. Beim horizontalen Herunterskalieren einer Skalierungsgruppe werden VM-Instanzen aus der Skalierungsgruppe entfernt. Service Fabric erkennt üblicherweise nicht, was passiert ist, und geht davon aus, dass ein Knoten verloren gegangen ist. Daraufhin meldet Service Fabric einen fehlerhaften Zustand für den Cluster. Zur Vermeidung dieses Fehlerzustands müssen Sie Service Fabric auf den erwarteten Wegfall des Knotens vorbereiten.

### <a name="remove-the-service-fabric-node"></a>Entfernen des Service Fabric-Knotens

> [!NOTE]
> Dieser Teil ist nur für die Dauerhaftigkeitsstufe *Bronze* relevant. Weitere Informationen zur Dauerhaftigkeit finden Sie unter [Überlegungen zur Kapazitätsplanung für Service Fabric-Cluster][durability].

Der zuletzt erstellte Knoten sollte zuerst entfernt werden, damit die Knoten des Clusters über die Upgrade- und Fehlerdomänen gleichmäßig verteilt bleiben und die gleichmäßige Nutzung sichergestellt ist. Anders ausgedrückt: Die Knoten sollten in der umgekehrten Reihenfolge ihrer Erstellung entfernt werden. Der zuletzt erstellte Knoten verfügt über den höchsten `virtual machine scale set InstanceId`-Eigenschaftswert. In den unten angegebenen Codebeispielen wird der zuletzt erstellte Knoten zurückgegeben.

```powershell
Get-ServiceFabricNode | Sort-Object { $_.NodeName.Substring($_.NodeName.LastIndexOf('_') + 1) } -Descending | Select-Object -First 1
```

```azurecli
sfctl node list --query "sort_by(items[*], &name)[-1]"
```

Der Service Fabric-Cluster muss wissen, dass dieser Knoten entfernt wird. Hierzu sind drei Schritte erforderlich:

1. Deaktivieren Sie den Knoten, sodass es sich dabei nicht mehr um ein Replikat für Daten handelt.  
PowerShell: `Disable-ServiceFabricNode`  
sfctl: `sfctl node disable`

2. Beenden Sie den Knoten, damit die Service Fabric-Laufzeit richtig heruntergefahren wird und Ihre App eine Beendigungsanforderung erhält.  
PowerShell: `Start-ServiceFabricNodeTransition -Stop`  
sfctl: `sfctl node transition --node-transition-type Stop`

2. Entfernen Sie den Knoten aus dem Cluster.  
PowerShell: `Remove-ServiceFabricNodeState`  
sfctl: `sfctl node remove-state`

Nachdem diese drei Schritte für den Knoten ausgeführt wurden, kann er aus der Skalierungsgruppe entfernt werden. Bei Verwendung einer anderen Dauerhaftigkeitsstufe als [Bronze][durability] werden diese Schritte automatisch ausgeführt, wenn die Skalierungsgruppeninstanz entfernt wird.

Der folgende Codeblock ruft den zuletzt erstellten Knoten ab und deaktiviert, beendet und entfernt ihn aus dem Cluster.

```powershell
#### After you've connected.....
# Get the node that was created last
$node = Get-ServiceFabricNode | Sort-Object { $_.NodeName.Substring($_.NodeName.LastIndexOf('_') + 1) } -Descending | Select-Object -First 1

# Node details for the disable/stop process
$nodename = $node.NodeName
$nodeid = $node.NodeInstanceId

$loopTimeout = 10

# Run disable logic
Disable-ServiceFabricNode -NodeName $nodename -Intent RemoveNode -TimeoutSec 300 -Force

$state = Get-ServiceFabricNode | Where-Object NodeName -eq $nodename | Select-Object -ExpandProperty NodeStatus

while (($state -ne [System.Fabric.Query.NodeStatus]::Disabled) -and ($loopTimeout -ne 0))
{
    Start-Sleep 5
    $loopTimeout -= 1
    $state = Get-ServiceFabricNode | Where-Object NodeName -eq $nodename | Select-Object -ExpandProperty NodeStatus
    Write-Host "Checking state... $state found"
}

# Exit if the node was unable to be disabled
if ($state -ne [System.Fabric.Query.NodeStatus]::Disabled)
{
    Write-Error "Disable failed with state $state"
}
else
{
    # Stop node
    $stopid = New-Guid
    Start-ServiceFabricNodeTransition -Stop -OperationId $stopid -NodeName $nodename -NodeInstanceId $nodeid -StopDurationInSeconds 300

    $state = (Get-ServiceFabricNodeTransitionProgress -OperationId $stopid).State
    $loopTimeout = 10

    # Watch the transaction
    while (($state -eq [System.Fabric.TestCommandProgressState]::Running) -and ($loopTimeout -ne 0))
    {
        Start-Sleep 5
        $state = (Get-ServiceFabricNodeTransitionProgress -OperationId $stopid).State
        Write-Host "Checking state... $state found"
    }

    if ($state -ne [System.Fabric.TestCommandProgressState]::Completed)
    {
        Write-Error "Stop transaction failed with $state"
    }
    else
    {
        # Remove the node from the cluster
        Remove-ServiceFabricNodeState -NodeName $nodename -TimeoutSec 300 -Force
    }
}
```

Unten im **sfctl**-Code wird der folgende Befehl verwendet, um den Wert **node-name** des zuletzt erstellten Knotens abzurufen: `sfctl node list --query "sort_by(items[*], &name)[-1].name"`

```azurecli
# Inform the node that it is going to be removed
sfctl node disable --node-name _nt1vm_5 --deactivation-intent 4 -t 300

# Stop the node using a random guid as our operation id
sfctl node transition --node-instance-id 131541348482680775 --node-name _nt1vm_5 --node-transition-type Stop --operation-id c17bb4c5-9f6c-4eef-950f-3d03e1fef6fc --stop-duration-in-seconds 14400 -t 300

# Remove the node from the cluster
sfctl node remove-state --node-name _nt1vm_5
```

> [!TIP]
> Verwenden Sie die folgenden **sfctl**-Abfragen, um den Status der einzelnen Schritte zu überprüfen:
>
> **Überprüfen des Deaktivierungsstatus**
> `sfctl node list --query "sort_by(items[*], &name)[-1].nodeDeactivationInfo"`
>
> **Überprüfen des Beendigungsstatus**
> `sfctl node list --query "sort_by(items[*], &name)[-1].isStopped"`
>

### <a name="scale-in-the-scale-set"></a>Horizontales Herunterskalieren der Skalierungsgruppe

Nachdem der Service Fabric-Knoten aus dem Cluster entfernt wurde, kann die VM-Skalierungsgruppe horizontal herunterskaliert werden. Im folgenden Beispiel wird die Kapazität der Skalierungsgruppe um 1 verringert:

```powershell
$scaleset = Get-AzureRmVmss -ResourceGroupName SFCLUSTERTUTORIALGROUP -VMScaleSetName nt1vm
$scaleset.Sku.Capacity -= 1

Update-AzureRmVmss -ResourceGroupName SFCLUSTERTUTORIALGROUP -VMScaleSetName nt1vm -VirtualMachineScaleSet $scaleset
```

Der folgende Code legt die Kapazität auf 5 fest:

```azurecli
# Get the name of the node with
az vmss list-instances -n nt1vm -g sfclustertutorialgroup --query [*].name

# Use the name to scale
az vmss scale -g sfclustertutorialgroup -n nt1vm --new-capacity 5
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Lesen der Anzahl von Clusterknoten
> * Hinzufügen von Clusterknoten (horizontales Hochskalieren)
> * Entfernen von Clusterknoten (horizontales Herunterskalieren)

Fahren Sie mit dem folgenden Tutorial fort, um zu erfahren, wie Sie die Runtime eines Clusters aktualisieren:
> [!div class="nextstepaction"]
> [Aktualisieren der Runtime eines Service Fabric-Clusters](service-fabric-tutorial-upgrade-cluster.md)

[durability]: service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster
