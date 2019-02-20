---
title: Erstellen einer Routingtabelle für einen virtuellen Hub für Azure Virtual WAN zum Steuern des Datenverkehrs zu einem virtuellen Netzwerkgerät | Microsoft-Dokumentation
description: Informationen zu einer Routingtabelle für einen virtuellen Hub für Virtual WAN zum Steuern des Datenverkehrs zu einem virtuellen Netzwerkgerät.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to work with routing tables for NVA.
ms.openlocfilehash: 45e5c43cf5eb8df1df5b26ffae50d2881bb086e4
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2019
ms.locfileid: "56115197"
---
# <a name="create-a-virtual-hub-route-table-to-steer-traffic-to-a-network-virtual-appliance"></a>Erstellen einer Routingtabelle für einen virtuellen Hub für Virtual WAN zum Steuern des Datenverkehrs zu einem virtuellen Netzwerkgerät

In diesem Artikel wird veranschaulicht, wie Sie Datenverkehr von einem virtuellen Hub zu einem virtuellen Netzwerkgerät steuern. 

![Virtual WAN-Diagramm](./media/virtual-wan-route-table/vwanroute.png)

In diesem Artikel wird Folgendes behandelt:

* Erstellen eines WAN
* Erstellen eines Hubs
* Erstellen von Netzwerkverbindungen mit einem virtuellen Hub
* Erstellen einer Hubroute
* Erstellen einer Routingtabelle
* Anwenden der Routingtabelle

## <a name="before-you-begin"></a>Voraussetzungen

Vergewissern Sie sich, dass die folgenden Kriterien erfüllt sind:

1. Sie haben ein virtuelles Netzwerkgerät. Dies ist eine Drittanbietersoftware Ihrer Wahl, die in der Regel über Azure Marketplace (Link) in einem virtuellen Netzwerk bereitgestellt wird.
2. Sie haben der Netzwerkschnittstelle des virtuellen Netzwerkgeräts eine private IP-Adresse zugewiesen. 
3. Das virtuelle Netzwerkgerät kann nicht im virtuellen Hub bereitgestellt werden. Es muss in einem gesonderten VNET bereitgestellt werden. In diesem Artikel wird das NVA-VNET als „DMZ VNET“ bezeichnet.
4. Mit „DMZ VNET“ kann mindestens ein virtuelles Netzwerk verbunden sein. In diesem Artikel wird dieses VNET als „Indirektes Spoke-VNET“ bezeichnet. Diese VNETs können mithilfe von VNET-Peering mit DMZ VNET verbunden werden.
5. Bestätigen Sie, dass Sie bereits 2 VNETs erstellt haben. Diese werden als Spoke-VNETs verwendet. In diesem Artikel sind die VNET-Spoke-Adressräume 10.0.2.0/24 und 10.0.3.0/24. Informationen zum Erstellen eine VNET finden Sie unter [Erstellen eines virtuellen Netzwerks mit PowerShell](../virtual-network/quick-create-powershell.md).
6. Stellen Sie sicher, dass in den VNETs keine Gateways für virtuelle Netzwerke vorhanden sind.

## <a name="signin"></a>1. Anmelden

Installieren Sie die neueste Version der PowerShell-Cmdlets für Resource Manager. Weitere Informationen zum Installieren von PowerShell-Cmdlets finden Sie unter [Overview of Azure PowerShell](/powershell/azure/azurerm/overview) (Übersicht über Azure PowerShell). Dies ist wichtig, da frühere Versionen der Cmdlets nicht die aktuellen Werte enthalten, die Sie für diese Übung benötigen. Die Module in den folgenden Beispielen stammen aus Azure-RM. Der Artikel wird demnächst für Azure Az aktualisiert.

1. Öffnen Sie die PowerShell-Konsole mit erhöhten Rechten, und melden Sie sich bei Ihrem Azure-Konto an. Das Cmdlet fordert Sie zur Eingabe der Anmeldeinformationen auf. Nach der Anmeldung werden Ihre Kontoeinstellungen heruntergeladen, damit sie Azure PowerShell zur Verfügung stehen.

  ```powershell
  Connect-AzureRmAccount
  ```
2. Rufen Sie eine Liste Ihrer Azure-Abonnements ab.

  ```powershell
  Get-AzureRmSubscription
  ```
3. Geben Sie das Abonnement an, das Sie verwenden möchten.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Name of subscription"
  ```

## <a name="rg"></a>2. Erstellen von Ressourcen

1. Erstellen Sie eine Ressourcengruppe.

  ```powershell
  New-AzureRmResourceGroup -Location "West US" -Name "testRG"
  ```
2. Erstellen Sie ein virtuelles WAN.

  ```powershell
  $virtualWan = New-AzureRmVirtualWan -ResourceGroupName "testRG" -Name "myVirtualWAN" -Location "West US"
  ```
3. Erstellen Sie einen virtuellen Hub.

  ```powershell
  New-AzureRmVirtualHub -VirtualWan $virtualWan -ResourceGroupName "testRG" -Name "westushub" -AddressPrefix "10.0.1.0/24"
  ```

## <a name="connections"></a>3. Erstellen von Verbindungen

Erstellen von Netzwerkverbindungen mit einem virtuellen Hub aus „Indirektes Spoke-VNET“ und „DMZ VNET“.

  ```powershell
  $remoteVirtualNetwork1= Get-AzureRmVirtualNetwork -Name “indirectspoke1” -ResourceGroupName “testRG”
  $remoteVirtualNetwork2= Get-AzureRmVirtualNetwork -Name “indirectspoke2” -ResourceGroupName “testRG”
  $remoteVirtualNetwork3= Get-AzureRmVirtualNetwork -Name “dmzvnet” -ResourceGroupName “testRG”

  New-AzureRmVirtualHubVnetConnection -ResourceGroupName “testRG” -VirtualHubName “westushub” -Name  “testvnetconnection1” -RemoteVirtualNetwork $remoteVirtualNetwork1
  New-AzureRmVirtualHubVnetConnection -ResourceGroupName “testRG” -VirtualHubName “westushub” -Name  “testvnetconnection2” -RemoteVirtualNetwork $remoteVirtualNetwork2
  New-AzureRmVirtualHubVnetConnection -ResourceGroupName “testRG” -VirtualHubName “westushub” -Name  “testvnetconnection3” -RemoteVirtualNetwork $remoteVirtualNetwork3
  ```

## <a name="route"></a>4. Erstellen einer virtuellen Hubroute

In diesem Artikel sind die Adressräume für „Indirektes Spoke-VNET“ 10.0.2.0/24 und 10.0.3.0/24. Die private IP-Adresse der Netzwerkschnittstelle von „DMZ VNET“ ist 10.0.4.5.

```powershell
$route1 = New-AzureRmVirtualHubRoute -AddressPrefix @("10.0.2.0/24", "10.0.3.0/24") -NextHopIpAddress "10.0.4.5"
```

## <a name="applyroute"></a>5. Erstellen einer Routingtabelle für den virtuellen Hub

Erstellen Sie eine Routingtabelle für den virtuellen Hub, und wenden Sie dann die erstellte Route darauf an.
 
```powershell
$routeTable = New-AzureRmVirtualHubRouteTable -Route @($route1)
```

## <a name="commit"></a>6. Übergeben der Änderungen

Führen Sie für die Änderungen am virtuellen Hub einen Commit aus.

```powershell
Update-AzureRmVirtualHub -VirtualWanId $virtualWan.Id -ResourceGroupName "testRG" -Name "westushub” -RouteTable $routeTable
```

## <a name="cleanup"></a>Bereinigen von Ressourcen

Wenn Sie diese Ressourcen nicht mehr benötigen, können Sie den Befehl [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) verwenden, um die Ressourcengruppe und alle darin enthaltenen Ressourcen zu entfernen. Ersetzen Sie „myResourceGroup“ durch den Namen Ihrer Ressourcengruppe, und führen Sie den folgenden PowerShell-Befehl aus:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Virtual WAN finden Sie auf der Seite mit der [Übersicht über Virtual WAN](virtual-wan-about.md).
