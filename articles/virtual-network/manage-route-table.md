---
title: Erstellen, Ändern oder Löschen einer Azure-Routingtabelle | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie eine Routingtabelle erstellen, ändern oder löschen.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/09/2018
ms.author: jdial
ms.openlocfilehash: 7ef57960b81e13eefebfab4430eec4db0c1eb2e8
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2018
ms.locfileid: "43344892"
---
# <a name="create-change-or-delete-a-route-table"></a>Erstellen, Ändern oder Löschen einer Routingtabelle

Azure führt für Datenverkehr automatisch das Routing zwischen Azure-Subnetzen, virtuellen Netzwerken und lokalen Netzwerken durch. Wenn Sie das Standardrouting von Azure ändern möchten, erstellen Sie eine Routingtabelle. Wenn Sie mit dem Routing in virtuellen Netzwerken noch nicht vertraut sind, lesen Sie mehr darüber in der [Übersicht über das Routing](virtual-networks-udr-overview.md), oder arbeiten Sie ein [Tutorial](tutorial-create-route-table-portal.md) durch.

## <a name="before-you-begin"></a>Voraussetzungen

Führen Sie zuerst die folgenden Aufgaben aus, ehe Sie die Schritte in den Abschnitten dieses Artikels durchführen:

- Falls Sie noch nicht über ein Azure-Konto verfügen, können Sie sich für ein [kostenloses Testkonto](https://azure.microsoft.com/free) registrieren.
- Öffnen Sie bei Verwendung des Portals https://portal.azure.com, und melden Sie sich mit Ihrem Azure-Konto an.
- Wenn Sie PowerShell-Befehle zum Durchführen von Aufgaben in diesem Artikel verwenden, führen Sie die Befehle entweder in [Azure Cloud Shell](https://shell.azure.com/powershell) oder durch Ausführen von PowerShell auf Ihrem Computer aus. Azure Cloud Shell ist eine kostenlose interaktive Shell, mit der Sie die Schritte in diesem Artikel ausführen können. Sie verfügt über allgemeine vorinstallierte Tools und ist für die Verwendung mit Ihrem Konto konfiguriert. Für dieses Tutorial ist das Azure PowerShell-Modul Version 5.7.0 oder höher erforderlich. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die installierte Version zu ermitteln. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen.
- Wenn Sie Befehle der Azure-Befehlszeilenschnittstelle (CLI) zum Durchführen von Aufgaben in diesem Artikel verwenden, führen Sie die Befehle entweder in [Azure Cloud Shell](https://shell.azure.com/bash) oder durch Ausführen der CLI auf Ihrem Computer aus. Für dieses Tutorial ist die Azure CLI-Version 2.0.31 oder höher erforderlich. Führen Sie `az --version` aus, um die installierte Version zu ermitteln. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0](/cli/azure/install-azure-cli) Informationen dazu. Wenn Sie die Azure CLI lokal ausführen, müssen Sie auch `az login` ausführen, um eine Verbindung mit Azure herzustellen.

Das Konto, bei dem Sie sich anmelden oder das Sie zum Herstellen einer Verbindung mit Azure verwenden, muss der Rolle [Netzwerkmitwirkender](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) oder einer [benutzerdefinierten Rolle](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) zugewiesen sein, der die entsprechenden, unter [Berechtigungen](#permissions) aufgeführten Aktionen zugewiesen wurden.

## <a name="create-a-route-table"></a>Erstellen einer Routingtabelle

Die Anzahl von Routingtabellen, die Sie pro Azure-Standort und -Abonnement erstellen können, ist begrenzt. Ausführliche Informationen finden Sie im Artikel zu den [Einschränkungen für Azure-Abonnements](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

1. Wählen Sie links oben im Portal **+ Ressource erstellen** aus.
2. Wählen Sie die Option **Netzwerk** und dann **Routingtabelle**.
3. Geben Sie unter **Name** einen Namen für die Routingtabelle ein, wählen Sie Ihr **Abonnement**, erstellen Sie eine neue **Ressourcengruppe**, oder wählen Sie eine vorhandene Ressourcengruppe aus, wählen Sie einen **Standort**, und wählen Sie dann die Option **Erstellen**. Wenn Sie die Routingtabelle einem Subnetz eines virtuellen Netzwerks, das über ein VPN-Gateway mit Ihrem lokalen Netzwerk verbunden ist, zuordnen möchten und die **BGP-Routenverteilung** deaktivieren, werden Ihre lokalen Routen nicht an die Netzwerkschnittstellen in diesem Subnetz übergeben.

**Befehle**

- Azure CLI: [az network route-table create](/cli/azure/network/route-table/route#az_network_route_table_create)
- PowerShell: [New-AzureRmRouteTable](/powershell/module/azurerm.network/new-azurermroutetable)

## <a name="view-route-tables"></a>Anzeigen von Routingtabellen

Geben Sie im Suchfeld oben im Portal den Suchbegriff *Routingtabellen* ein. Wählen Sie **Routingtabellen** aus, wenn dieser Begriff in den Suchergebnissen angezeigt wird. Die in Ihrem Abonnement enthaltenen Routingtabellen werden aufgeführt.

**Befehle**

- Azure CLI: [az network route-table list](/cli/azure/network/route-table/route#az_network_route_table_list)
- PowerShell: [Get-AzureRmRouteTable](/powershell/module/azurerm.network/get-azurermroutetable)

## <a name="view-details-of-a-route-table"></a>Anzeigen von Details einer Routingtabelle

1. Geben Sie im Suchfeld oben im Portal den Suchbegriff *Routingtabellen* ein. Wählen Sie **Routingtabellen** aus, wenn dieser Begriff in den Suchergebnissen angezeigt wird.
2. Wählen Sie in der Liste die Routingtabelle aus, für die Sie Details anzeigen möchten. Unter **EINSTELLUNGEN** können Sie die **Routen** in der Routingtabelle und die **Subnetze** anzeigen, denen die Routingtabelle zugeordnet ist.
3. Weitere Informationen zu allgemeinen Azure-Einstellungen finden Sie in den folgenden Artikeln:
    *   [Aktivitätsprotokoll](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#activity-logs)
    *   [Zugriffssteuerung (IAM)](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#access-control)
    *   [Tags](../azure-resource-manager/resource-group-using-tags.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
    *   [Sperren](../azure-resource-manager/resource-group-lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
    *   [Automatisierungsskript](../azure-resource-manager/resource-manager-export-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json#export-the-template-from-resource-group)

**Befehle**

- Azure CLI: [az network route-table show](/cli/azure/network/route-table/route#az_network_route_table_show)
- PowerShell: [Get-AzureRmRouteTable](/powershell/module/azurerm.network/get-azurermroutetable)

## <a name="change-a-route-table"></a>Ändern einer Routingtabelle

1. Geben Sie im Suchfeld oben im Portal den Suchbegriff *Routingtabellen* ein. Wählen Sie **Routingtabellen** aus, wenn dieser Begriff in den Suchergebnissen angezeigt wird.
2. Wählen Sie die Routingtabelle aus, die Sie ändern möchten. Die häufigsten Änderungen sind das [Hinzufügen](#create-a-route) oder [Entfernen](#delete-a-route) von Routen und das [Zuordnen](#associate-a-route-table-to-a-subnet) von Routingtabellen zu Subnetzen bzw. das [Aufheben der Zuordnung](#dissociate-a-route-table-from-a-subnet).

**Befehle**

- Azure CLI: [az network route-table update](/cli/azure/network/route-table/route#az_network_route_table_update)
- PowerShell: [Set-AzureRmRouteTable](/powershell/module/azurerm.network/set-azurermroutetable)

## <a name="associate-a-route-table-to-a-subnet"></a>Zuordnen einer Routingtabelle zu einem Subnetz

Jedem Subnetz können null oder eine Routingtabelle zugeordnet werden. Eine Routingtabelle kann keinem oder mehreren Subnetzen zugeordnet werden. Da Routingtabellen nicht virtuellen Netzwerken zugeordnet sind, müssen Sie jedem Subnetz, für das eine Zuordnung bestehen soll, eine Routingtabelle zuordnen. Der gesamte Datenverkehr, der das Subnetz verlässt, wird anhand von Routen, die Sie in Routingtabellen erstellt haben, [Standardrouten](virtual-networks-udr-overview.md#default) und Routen weitergeleitet, die über ein lokales Netzwerk verteilt werden – sofern das virtuelle Netzwerk mit einem Gateway für das virtuelle Azure-Netzwerk verbunden ist (ExpressRoute oder VPN, falls BGP mit einem VPN-Gateway verwendet wird). Sie können eine Routingtabelle nur Subnetzen in virtuellen Netzwerken zuordnen, die an demselben Azure-Standort und unter demselben Abonnement wie die Routingtabelle vorhanden sind.

1. Geben Sie oben im Portal *Virtuelle Netzwerke* in das Suchfeld ein. Wenn **Virtuelle Netzwerke** in den Suchergebnissen angezeigt wird, können Sie den Begriff auswählen.
2. Wählen Sie das virtuelle Netzwerk in der Liste aus, in der das Subnetz enthalten ist, dem Sie eine Routingtabelle zuordnen möchten.
3. Wählen Sie unter **EINSTELLUNGEN** die Option **Subnetze**.
4. Wählen Sie das Subnetz aus, dem Sie die Routingtabelle zuordnen möchten.
5. Wählen Sie die Option **Routingtabelle**, wählen Sie die Routingtabelle aus, die Sie dem Subnetz zuordnen möchten, und wählen Sie anschließend **Speichern**.

Wenn Ihr virtuelles Netzwerk mit einem Azure-VPN-Gateway verbunden ist, ordnen Sie dem [Gatewaysubnetz](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub) keine Routingtabelle zu, die eine Route mit dem Ziel 0.0.0.0/0 enthält. Andernfalls ist die ordnungsgemäße Funktion des Gateways gefährdet. Weitere Informationen zur Verwendung von 0.0.0.0/0 in einer Route finden Sie unter [Routing von Datenverkehr für virtuelle Netzwerke](virtual-networks-udr-overview.md#default-route).

**Befehle**

- Azure CLI: [az network vnet subnet update](/cli/azure/network/vnet/subnet?view=azure-cli-latest#az_network_vnet_subnet_update)
- PowerShell: [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig)

## <a name="dissociate-a-route-table-from-a-subnet"></a>Aufheben der Zuordnung einer Routingtabelle zu einem Subnetz

Wenn Sie die Zuordnung einer Routingtabelle zu einem Subnetz aufheben, wird Datenverkehr von Azure basierend auf den [Standardrouten](virtual-networks-udr-overview.md#default) weitergeleitet.

1. Geben Sie oben im Portal *Virtuelle Netzwerke* in das Suchfeld ein. Wenn **Virtuelle Netzwerke** in den Suchergebnissen angezeigt wird, können Sie den Begriff auswählen.
2. Wählen Sie das virtuelle Netzwerk aus, in der das Subnetz enthalten ist, für das Sie die Zuordnung einer Routingtabelle aufheben möchten.
3. Wählen Sie unter **EINSTELLUNGEN** die Option **Subnetze**.
4. Wählen Sie das Subnetz aus, für das Sie die Zuordnung der Routingtabelle aufheben möchten.
5. Wählen Sie **Routingtabelle**, **Keine** und dann **Speichern**.

**Befehle**

- Azure CLI: [az network vnet subnet update](/cli/azure/network/vnet/subnet?view=azure-cli-latest#az_network_vnet_subnet_update)
- PowerShell: [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) 

## <a name="delete-a-route-table"></a>Löschen einer Routingtabelle

Wenn eine Routingtabelle Subnetzen zugeordnet ist, kann sie nicht gelöscht werden. [Heben Sie die Zuordnung einer Routingtabelle zu allen Subnetzen auf](#dissociate-a-route-table-from-a-subnet), bevor Sie versuchen, das Löschen durchzuführen.

1. Geben Sie im Suchfeld oben im Portal den Suchbegriff *Routingtabellen* ein. Wählen Sie **Routingtabellen** aus, wenn dieser Begriff in den Suchergebnissen angezeigt wird.
2. Wählen Sie rechts von der Routingtabelle, die Sie löschen möchten, die Auslassungszeichen (**...**).
3. Wählen Sie **Löschen** und dann **Ja**.

**Befehle**

- Azure CLI: [az network route-table delete](/cli/azure/network/route-table/route#az_network_route_table_delete)
- PowerShell: [Remove-AzureRmRouteTable](https://docs.microsoft.com/powershell/module/azurerm.network/remove-azurermroutetable?view=azurermps-6.8.1) 

## <a name="create-a-route"></a>Erstellen einer Route

Die Anzahl von Routen pro Routingtabelle, die Sie pro Azure-Standort und -Abonnement erstellen können, ist begrenzt. Ausführliche Informationen finden Sie im Artikel zu den [Einschränkungen für Azure-Abonnements](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

1. Geben Sie im Suchfeld oben im Portal den Suchbegriff *Routingtabellen* ein. Wählen Sie **Routingtabellen** aus, wenn dieser Begriff in den Suchergebnissen angezeigt wird.
2. Wählen Sie in der Liste die Routingtabelle aus, der Sie eine Route hinzufügen möchten.
3. Wählen Sie unter **EINSTELLUNGEN** die Option **Routen**.
4. Wählen Sie **+ Hinzufügen**.
5. Geben Sie unter **Name** einen Namen für die Route der Routingtabelle ein.
6. Geben Sie das **Adresspräfix** in CIDR-Notation ein, an das Sie Datenverkehr leiten möchten. Das Präfix kann in der Routingtabelle nicht für mehr als eine Route dupliziert werden, aber das Präfix kann in einem anderen Präfix enthalten sein. Wenn Sie beispielsweise 10.0.0.0/16 als Präfix in einer Route definiert haben, können Sie trotzdem eine andere Route mit dem Adresspräfix 10.0.0.0/24 definieren. In Azure wird eine Route für Datenverkehr basierend auf der längsten Präfixübereinstimmung ausgewählt. Weitere Informationen zur Routenauswahl in Azure finden Sie unter [Routing von Datenverkehr für virtuelle Netzwerke](virtual-networks-udr-overview.md#how-azure-selects-a-route).
7. Wählen Sie einen **Typ des nächsten Hops** aus. Eine ausführliche Beschreibung aller Typen des nächsten Hops finden Sie unter [Routing von Datenverkehr für virtuelle Netzwerke](virtual-networks-udr-overview.md).
8. Geben Sie eine IP-Adresse für **Adresse des nächsten Hops** ein. Sie können nur dann eine Adresse eingeben, wenn Sie als **Typ des nächsten Hops** die Option *Virtuelles Gerät* ausgewählt haben.
9. Klicken Sie auf **OK**.

**Befehle**

- Azure CLI: [az network route-table route create](/cli/azure/network/route-table/route?view=azure-cli-latest#az_network_route_table_route_create)
- PowerShell: [New-AzureRmRouteConfig](/powershell/module/azurerm.network/new-azurermrouteconfig)

## <a name="view-routes"></a>Anzeigen von Routen

Eine Routingtabelle enthält null oder mehrere Routen. Weitere Informationen zu den Details, die beim Anzeigen von Routen aufgeführt werden, finden Sie unter [Routing von Datenverkehr für virtuelle Netzwerke](virtual-networks-udr-overview.md).

1. Geben Sie im Suchfeld oben im Portal den Suchbegriff *Routingtabellen* ein. Wählen Sie **Routingtabellen** aus, wenn dieser Begriff in den Suchergebnissen angezeigt wird.
2. Wählen Sie in der Liste die Routingtabelle aus, für die Sie Routen anzeigen möchten.
3. Wählen Sie unter **EINSTELLUNGEN** die Option **Routen**.

**Befehle**

- Azure CLI: [az network route-table route list](/cli/azure/network/route-table/route?view=azure-cli-latest#az_network_route_table_route_list)
- PowerShell: [Get-AzureRmRouteConfig](/powershell/module/azurerm.network/get-azurermrouteconfig)

## <a name="view-details-of-a-route"></a>Anzeigen von Details einer Route

1. Geben Sie im Suchfeld oben im Portal den Suchbegriff *Routingtabellen* ein. Wählen Sie **Routingtabellen** aus, wenn dieser Begriff in den Suchergebnissen angezeigt wird.
2. Wählen Sie die Routingtabelle aus, für die Sie Details einer Route anzeigen möchten.
3. Wählen Sie die Option **Routen**.
4. Wählen Sie die Route aus, für die Sie Details anzeigen möchten.

**Befehle**

- Azure CLI: [az network route-table route show](/cli/azure/network/route-table/route?view=azure-cli-latest#az_network_route_table_route_show)
- PowerShell: [Get-AzureRmRouteConfig](/powershell/module/azurerm.network/get-azurermrouteconfig)

## <a name="change-a-route"></a>Ändern einer Route

1. Geben Sie im Suchfeld oben im Portal den Suchbegriff *Routingtabellen* ein. Wählen Sie **Routingtabellen** aus, wenn dieser Begriff in den Suchergebnissen angezeigt wird.
2. Wählen Sie die Routingtabelle aus, für die Sie eine Route ändern möchten.
3. Wählen Sie die Option **Routen**.
4. Wählen Sie die Route aus, die Sie ändern möchten.
5. Ändern Sie die vorhandenen Einstellungen in die neuen Einstellungen, und wählen Sie anschließend **Speichern**.

**Befehle**

- Azure CLI: [az network route-table route update](/cli/azure/network/route-table/route?view=azure-cli-latest#az_network_route_table_route_update)
- PowerShell: [Set-AzureRmRouteConfig](/powershell/module/azurerm.network/set-azurermrouteconfig)

## <a name="delete-a-route"></a>Löschen einer Route

1. Geben Sie im Suchfeld oben im Portal den Suchbegriff *Routingtabellen* ein. Wählen Sie **Routingtabellen** aus, wenn dieser Begriff in den Suchergebnissen angezeigt wird.
2. Wählen Sie die Routingtabelle aus, für die Sie eine Route löschen möchten.
3. Wählen Sie die Option **Routen**.
4. Wählen Sie in der Liste mit den Routen die Auslassungszeichen (**...**) rechts von der Route, die Sie löschen möchten.
5. Wählen Sie **Löschen** und dann **Ja**.

**Befehle**

- Azure CLI: [az network route-table route delete](/cli/azure/network/route-table/route?view=azure-cli-latest#az_network_route_table_route_delete)
- PowerShell: [Remove-AzureRmRouteConfig](/powershell/module/azurerm.network/remove-azurermrouteconfig)

## <a name="view-effective-routes"></a>Anzeigen effektiver Routen

Die effektiven Routen für jede Netzwerkschnittstelle, die an einen virtuellen Computer angefügt ist, sind eine Kombination der von Ihnen erstellten Routingtabellen, der Standardrouten von Azure und der Routen, die von lokalen Netzwerken per BGP über das Gateway eines virtuellen Azure-Netzwerks verteilt werden. Es ist für die Behandlung von Routingproblemen hilfreich, wenn Sie sich mit den effektiven Routen einer Netzwerkschnittstelle auskennen. Sie können die effektiven Routen für alle Netzwerkschnittstellen anzeigen, die an einen ausgeführten virtuellen Computer angefügt sind.

1. Geben Sie im Suchfeld oben im Portal den Namen eines virtuellen Computers ein, für den Sie effektive Routen anzeigen möchten. Wenn Sie den Namen eines virtuellen Computers kennen, geben Sie im Suchfeld *Virtuelle Computer* ein. Wählen Sie **Virtuelle Computer** aus, wenn der Begriff in den Suchergebnissen angezeigt wird, und wählen Sie in der Liste einen virtuellen Computer aus.
2. Wählen Sie unter **EINSTELLUNGEN** die Option **Netzwerk**.
3. Wählen Sie den Namen einer Netzwerkschnittstelle aus.
4. Wählen Sie unter **SUPPORT + PROBLEMBEHANDLUNG** die Option **Effektive Routen**.
5. Prüfen Sie die Liste mit den effektiven Routen, um zu ermitteln, ob die richtige Route für den Ort vorhanden ist, an den Sie Datenverkehr leiten möchten. Weitere Informationen zu den Typen des nächsten Hops, die in dieser Liste angezeigt werden, finden Sie unter [Routing von Datenverkehr für virtuelle Netzwerke](virtual-networks-udr-overview.md).

**Befehle**

- Azure CLI: [az network nic show-effective-route-table](/cli/azure/network/nic?view=azure-cli-latest#az_network_nic_show_effective_route_table)
- PowerShell: [Get-AzureRmEffectiveRouteTable](/powershell/module/azurerm.network/get-azurermeffectiveroutetable) 

## <a name="validate-routing-between-two-endpoints"></a>Überprüfen des Routings zwischen zwei Endpunkten

Sie können den Typ des nächsten Hops zwischen einem virtuellen Computer und der IP-Adresse einer anderen Azure-Ressource, einer lokalen Ressource oder einer Ressource im Internet ermitteln. Die Ermittlung des Azure-Routings ist für die Behandlung von Routingproblemen hilfreich. Zur Durchführung dieser Aufgabe muss eine Network Watcher-Instanz vorhanden sein. Falls Sie noch nicht über eine Network Watcher-Instanz verfügen, können Sie eine erstellen, indem Sie die Schritte unter [Erstellen einer Network Watcher-Instanz](../network-watcher/network-watcher-create.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ausführen.

1. Geben Sie im Suchfeld oben im Portal den Suchbegriff *Network Watcher* ein. Wählen Sie **Network Watcher** aus, wenn der Begriff in den Suchergebnissen angezeigt wird.
2. Wählen Sie unter **NETZWERKDIAGNOSETOOLS** die Option **Nächster Hop**.
3. Wählen Sie Ihr **Abonnement** und die **Ressourcengruppe** des virtuellen Quellcomputers aus, über den Sie das Routing überprüfen möchten.
4. Wählen Sie den **virtuellen Computer**, die **Netzwerkschnittstelle**, die an den virtuellen Computer angefügt ist, und die **Quell-IP-Adresse** der Netzwerkschnittstelle aus, über die Sie das Routing überprüfen möchten.
5. Geben Sie die **Ziel-IP-Adresse** ein, die als Ziel für die Überprüfung des Routings dienen soll.
6. Wählen Sie **Nächster Hop**.
7. Nach einer kurzen Wartezeit werden Informationen zurückgegeben, die den Typ des nächsten Hops und die ID der Route für die Weiterleitung des Datenverkehrs umfassen. Weitere Informationen zu den Typen des nächsten Hops, die zurückgegeben werden, finden Sie unter [Routing von Datenverkehr für virtuelle Netzwerke](virtual-networks-udr-overview.md).

**Befehle**

- Azure CLI: [az network watcher show-next-hop](/cli/azure/network/watcher?view=azure-cli-latest#az_network_watcher_show_next_hop)
- PowerShell: [Get-AzureRmNetworkWatcherNextHop](/powershell/module/azurerm.network/get-azurermnetworkwatchernexthop) 

## <a name="permissions"></a>Berechtigungen

Zum Durchführen von Aufgaben für Routingtabellen und Routen muss Ihr Konto der Rolle [Netzwerkmitwirkender](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) oder einer [benutzerdefinierten](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) Rolle zugewiesen sein, der die entsprechenden, in der folgenden Tabelle aufgeführten Aktionen zugewiesen wurden:

| Aktion                                                          |   NAME                                                  |
|--------------------------------------------------------------   |   -------------------------------------------           |
| Microsoft.Network/routeTables/read                              |   Lesen einer Routingtabelle                                    |
| Microsoft.Network/routeTables/write                             |   Erstellen/Aktualisieren einer Routingtabelle                        |
| Microsoft.Network/routeTables/delete                            |   Löschen einer Routingtabelle                                  |
| Microsoft.Network/routeTables/join/action                       |   Zuordnen einer Routingtabelle zu einem Subnetz                   |
| Microsoft.Network/routeTables/routes/read                       |   Lesen einer Route                                          |
| Microsoft.Network/routeTables/routes/write                      |   Erstellen oder Aktualisieren einer Route                              |
| Microsoft.Network/routeTables/routes/delete                     |   Löschen einer Route                                        |
| Microsoft.Network/networkInterfaces/effectiveRouteTable/action  |   Abrufen der effektiven Routingtabelle für eine Netzwerkschnittstelle |
| Microsoft.Network/networkWatchers/nextHop/action                |   Nächsten Hop von einer VM abrufen                           |

## <a name="next-steps"></a>Nächste Schritte

- Erstellen einer Routingtabelle mithilfe von [PowerShell](powershell-samples.md)- oder [Azure CLI](cli-samples.md)-Beispielskripts oder mithilfe von Azure [Resource Manager-Vorlagen](template-samples.md)
- Erstellen und Anwenden einer [Azure-Richtlinie](policy-samples.md) für virtuelle Netzwerke
