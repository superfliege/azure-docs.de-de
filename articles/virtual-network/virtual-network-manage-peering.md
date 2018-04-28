---
title: Erstellen, Ändern oder Löschen eines Peerings virtueller Azure-Netzwerke | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie ein Peering virtueller Netzwerke erstellen, ändern oder löschen.
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
ms.author: jdial;anavin
ms.openlocfilehash: 52c910609930bbeecd21b75549c71ee9ed4e1e3b
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2018
---
# <a name="create-change-or-delete-a-virtual-network-peering"></a>Erstellen, Ändern oder Löschen eines Peerings virtueller Netzwerke

In diesem Artikel erfahren Sie, wie Sie ein Peering virtueller Netzwerke erstellen, ändern oder löschen. VNet-Peering ist ein Mechanismus, mit dem virtuelle Netzwerke (VNets) über das Azure-Backbonenetzwerk miteinander verbunden werden können. Nach dem Peering werden die virtuellen Netzwerke weiterhin als separate Ressourcen verwaltet. Wenn Sie nicht mit dem Peering virtueller Netzwerke vertraut sind, sollten Sie vor der Durchführung der Aufgaben den Artikel [Peering virtueller Netzwerke – Überblick](virtual-network-peering-overview.md) lesen und das Tutorial unter [Erstellen eines Peerings virtueller Netzwerke](tutorial-connect-virtual-networks-portal.md) absolvieren.

## <a name="before-you-begin"></a>Voraussetzungen

Führen Sie zuerst die folgenden Aufgaben aus, ehe Sie die Schritte in den Abschnitten dieses Artikels durchführen:

- Falls Sie noch nicht über ein Azure-Konto verfügen, können Sie sich für ein [kostenloses Testkonto](https://azure.microsoft.com/free) registrieren.
- Wenn Sie das Portal verwenden, öffnen Sie https://portal.azure.com, und melden Sie sich mit einem Konto an, das über die [erforderlichen Berechtigungen](#permissions) für die Verwendung von Peerings verfügt.
- Wenn Sie PowerShell-Befehle zum Durchführen von Aufgaben in diesem Artikel verwenden, führen Sie die Befehle entweder in [Azure Cloud Shell](https://shell.azure.com/powershell) oder durch Ausführen von PowerShell auf Ihrem Computer aus. Azure Cloud Shell ist eine kostenlose interaktive Shell, mit der Sie die Schritte in diesem Artikel ausführen können. Sie verfügt über allgemeine vorinstallierte Tools und ist für die Verwendung mit Ihrem Konto konfiguriert. Für dieses Tutorial ist das Azure PowerShell-Modul Version 5.5.0 oder höher erforderlich. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die installierte Version zu ermitteln. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie zum Herstellen einer Verbindung mit Azure auch `Connect-AzureRmAccount` mit einem Konto ausführen, das über die [erforderlichen Berechtigungen](#permissions) für die Verwendung von Peerings verfügt.
- Wenn Sie Befehle der Azure-Befehlszeilenschnittstelle (CLI) zum Durchführen von Aufgaben in diesem Artikel verwenden, führen Sie die Befehle entweder in [Azure Cloud Shell](https://shell.azure.com/bash) oder durch Ausführen der CLI auf Ihrem Computer aus. Für dieses Tutorial ist mindestens Version 2.0.29 der Azure CLI oder eine höhere Version erforderlich. Führen Sie `az --version` aus, um die installierte Version zu ermitteln. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0](/cli/azure/install-azure-cli) Informationen dazu. Wenn Sie die Azure CLI lokal ausführen, müssen Sie zum Herstellen einer Verbindung mit Azure auch `az login` mit einem Konto ausführen, das über die [erforderlichen Berechtigungen](#permissions) für die Verwendung von Peerings verfügt.

## <a name="create-a-peering"></a>Erstellen eines Peerings

Informieren Sie sich vor der Erstellung eines Peerings über die [Anforderungen und Einschränkungen](#requirements-and-contstraints) sowie die [erforderlichen Berechtigungen](#permissions).

1. Geben Sie oben im Azure-Portal *Virtuelle Netzwerke* in das Suchfeld ein. Wenn **Virtuelle Netzwerke** in den Suchergebnissen angezeigt wird, können Sie den Begriff auswählen. Aktivieren Sie nicht **Virtuelle Netzwerke (klassisch)**, wenn diese Option in der Liste angezeigt wird, da über das klassische Bereitstellungsmodell kein Peering von einem bereitgestellten virtuellen Netzwerk aus erstellt werden kann.
2. Wählen Sie das virtuelle Netzwerk in der Liste aus, für das Sie ein Peering erstellen möchten.
3. Wählen Sie in der Liste der virtuellen Netzwerke das virtuelle Netzwerk aus, für das Sie ein Peering erstellen möchten.
4. Wählen Sie unter **EINSTELLUNGEN** die Option **Peerings** aus.
5. Wählen Sie **+ Hinzufügen**. 
6. <a name="add-peering"></a>Geben Sie Werte für folgende Einstellungen ein, oder wählen Sie sie aus:
    - **Name**: Der Name für das Peering muss innerhalb des virtuellen Netzwerks eindeutig sein.
    - **Bereitstellungsmodell für das virtuelle Netzwerk**: Wählen Sie aus, über welches Bereitstellungsmodell das mittels Peering verknüpfte virtuelle Netzwerk bereitgestellt wurde.
    - **Ich kenne meine Ressourcen-ID**: Wenn Sie Lesezugriff auf das virtuelle Netzwerk haben, das mittels Peering verknüpft werden sollen, lassen Sie dieses Kontrollkästchen deaktiviert. Wenn Sie keinen Lesezugriff auf das virtuelle Netzwerk oder das Abonnement besitzen, das mittels Peering verknüpft werden soll, aktivieren Sie dieses Kontrollkästchen. Geben Sie in das Feld **Ressourcen-ID**, das bei Aktivierung des Kontrollkästchens angezeigt wird, die vollständige Ressourcen-ID des mittels Peering verknüpften virtuellen Netzwerks ein. Die von Ihnen eingegebene Ressourcen-ID muss für ein virtuelles Netzwerk bestimmt sein, das sich in der gleichen Azure-[Region](https://azure.microsoft.com/regions) wie dieses virtuelle Netzwerk oder in einer [anderen unterstützten Region](#requirements-and-constraints) befindet. Die vollständige Ressourcen-ID sieht in etwa wie folgt aus: „/subscriptions/<Id>/resourceGroups/<Ressourcengruppenname>/providers/Microsoft.Network/virtualNetworks/<Name des virtuellen Netzwerks>“. Sie können die Ressourcen-ID für ein virtuelles Netzwerk abrufen, indem Sie die Eigenschaften eines virtuellen Netzwerks anzeigen. Informationen zum Anzeigen der Eigenschaften eines virtuellen Netzwerks finden Sie im Artikel [Verwalten virtueller Netzwerke](manage-virtual-network.md#view-virtual-networks-and-settings).
    - **Abonnement**: Wählen Sie das [Abonnement](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) des virtuellen Netzwerks aus, das mittels Peering verknüpft werden soll. Je nachdem, auf wie viele Abonnements Ihr Konto Lesezugriff verfügt, werden ein oder mehrere Abonnements aufgeführt. Wenn Sie das Kontrollkästchen **Ressourcen-ID** aktivieren, ist diese Einstellung ist verfügbar.
    - **Virtuelles Netzwerk**: Wählen Sie das virtuelle Netzwerk aus, das mittels Peering verknüpft werden soll. Sie können ein virtuelles Netzwerk auswählen, das mit einem beliebigen Azure-Bereitstellungsmodell erstellt wurde. Sie können nur ein virtuelles Netzwerk in einer anderen Region auswählen, das [unterstützt](#cross-region) wird. Sie benötigen Lesezugriff auf das virtuelle Netzwerk, damit es in der Liste angezeigt wird. Wenn ein virtuelles Netzwerk aufgeführt wird, jedoch ausgegraut ist, überschneidet sich der Adressraum des virtuellen Netzwerks möglicherweise mit dem Adressraum dieses virtuellen Netzwerks. Wenn sich die Adressräume des virtuellen Netzwerks überschneidet, können diese nicht mittels Peering verknüpft werden. Wenn Sie das Kontrollkästchen **Ressourcen-ID** aktivieren, ist diese Einstellung ist verfügbar.
    - **Zugriff auf virtuelles Netzwerk zulassen**: Wählen Sie **Aktiviert** (Standard), wenn Kommunikation zwischen beiden virtuellen Netzwerken aktiviert werden soll. Durch die Aktivierung der Kommunikation zwischen virtuellen Netzwerken können Ressourcen, die mit einem der beiden virtuellen Netzwerke verbunden sind, mit derselben Bandbreite und Latenz kommunizieren, als ob sie mit demselben virtuellen Netzwerk verbunden wären. Die gesamte Kommunikation zwischen Ressourcen in den beiden virtuellen Netzwerken erfolgt über das private Azure-Netzwerk. Das Standardtag **VirtualNetwork** für Netzwerksicherheitsgruppen umfasst das virtuelle Netzwerk und das mittels Peering verknüpfte virtuelle Netzwerk. Weitere Informationen zu Standardtags von Netzwerksicherheitsgruppen finden Sie im Artikel [Netzwerksicherheitsgruppen – Übersicht](virtual-networks-nsg.md#default-tags).  Wählen Sie **Deaktiviert**, wenn der Datenverkehr nicht über das mittels Peering verknüpfte virtuelle Netzwerk weitergeleitet werden soll. Sie können **Deaktiviert** wählen, wenn Sie ein virtuelles Netzwerk mittels Peering mit einem anderen virtuellen Netzwerk verknüpft haben, jedoch die Option haben möchten, den Datenverkehr zwischen den beiden virtuellen Netzwerken gelegentlich zu deaktivieren. Es kann u.U. praktischer sein, die Option zu aktivieren bzw. zu deaktivieren statt Peerings zu löschen und neu zu erstellen. Wenn diese Einstellung deaktiviert ist, wird der Datenverkehr nicht zwischen den mittels Peering verknüpften virtuellen Netzwerken weitergeleitet.
    - **Weitergeleiteten Datenverkehr zulassen**: Aktivieren Sie dieses Kontrollkästchen, um zuzulassen, dass der von einem virtuellen Netzwerkgerät *weitergeleitete* Datenverkehr (der nicht von dem virtuellen Netzwerk generiert wird) mittels Peering über dieses virtuelle Netzwerk erfolgt. Sehen Sie sich beispielsweise die drei virtuellen Netzwerke mit dem Namen „Spoke1“, „Spoke2“ und „Hub“ an. Die einzelnen Spokes und der Hub des virtuellen Netzwerks sind mittels Peering miteinander verknüpft, jedoch nicht die Spokes des virtuellen Netzwerks untereinander. Im Hub des virtuellen Netzwerks wird eine virtuelle Netzwerkappliance bereitgestellt, während bei den einzelnen Spokes des virtuellen Netzwerks benutzerdefinierte Routen angewendet werden, die Datenverkehr zwischen den Subnetzen über die virtuelle Netzwerkappliance weiterleiten. Wenn dieses Kontrollkästchen für das Peering zwischen den einzelnen Spokes und dem Hub des virtuellen Netzwerks nicht aktiviert ist, wird der Datenverkehr nicht zwischen den Spokes des virtuellen Netzwerks, sondern vom Hub zwischen den virtuellen Netzwerken weitergeleitet. Durch die Aktivierung dieser Funktion wird zwar die Weiterleitung des Datenverkehrs mittels Peering ermöglicht, es werden jedoch keine benutzerdefinierten Routen oder virtuellen Netzwerkgeräte erstellt. Benutzerdefinierte Routen und virtuelle Netzwerkgeräte werden separat erstellt. Weitere Informationen hierzu finden Sie unter [Benutzerdefinierte Routen](virtual-networks-udr-overview.md#user-defined). Sie müssen diese Einstellung nicht überprüfen, wenn der Datenverkehr zwischen virtuellen Netzwerken über ein Azure VPN Gateway weitergeleitet wird.
    - **Gatewaytransit zulassen**: Aktivieren Sie dieses Kontrollkästchen, wenn ein Gateway für virtuelle Netzwerke mit diesem virtuellen Netzwerk verbunden ist und Sie zulassen möchten, dass Datenverkehr von mittels Peering verknüpften virtuellen Netzwerken über das Gateway geleitet werden soll. Dieses virtuelle Netzwerk kann z.B. über ein Gateway für virtuelle Netzwerke mit einem lokalen Netzwerk verbunden werden. Bei dem Gateway kann es sich um ein ExpressRoute- oder VPN-Gateway handeln. Wird dieses Kontrollkästchen aktiviert, kann Datenverkehr aus dem mittels Peering verknüpften virtuellen Netzwerk über das mit diesem virtuellen Netzwerk verbundene Gateway an das lokale Netzwerk weitergeleitet werden. Wenn Sie dieses Kontrollkästchen aktivieren, darf für das mittels Peering verknüpfte virtuelle Netzwerk kein Gateway konfiguriert sein. Wenn Sie das Peering vom anderen virtuellen Netzwerk mit diesem virtuellen Netzwerk einrichten, muss für das mittels Peering verknüpfte virtuelle Netzwerk das Kontrollkästchen **Remotegateways verwenden** aktiviert sein. Wenn Sie dieses Kontrollkästchen deaktiviert (Standard) lassen, wird der Datenverkehr trotzdem von dem mittels Peering verknüpften Netzwerk an dieses virtuelle Netzwerk weitergeleitet, kann jedoch nicht über ein mit diesem virtuellen Netzwerk verbundenes Gateway für virtuelle Netzwerke erfolgen. 
    
        Zusätzlich zum Weiterleiten von Datenverkehr an ein lokales Netzwerk kann ein VPN-Gateway Netzwerkdatenverkehr zwischen virtuellen Netzwerken weiterleiten, die mittels Peering mit dem virtuellen Netzwerk, in dem das Gateway enthalten ist, verknüpft sind. Die virtuellen Netzwerke müssen dabei nicht untereinander mittels Peering verknüpft werden. Dies ist nützlich, wenn Sie ein VPN-Gateway im Hub eines virtuellen Netzwerks verwenden möchten, um Datenverkehr zwischen den Spokes des virtuellen Netzwerks, die nicht untereinander verknüpft sind, weiterzuleiten. (Weitere Informationen finden Sie in dem beschriebenen Beispiel zur Option **Weitergeleiteten Datenverkehr zulassen** bezüglich der Hub-Spoke-Topologie.) Weitere Informationen hierzu finden Sie unter [Gateways für virtuelle Netzwerke](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#s2smulti). In diesem Szenario müssen benutzerdefinierte Routen implementiert werden, die das Gateway für virtuelle Netzwerke als Typ des nächsten Hops angeben. Weitere Informationen hierzu finden Sie unter [Benutzerdefinierte Routen](virtual-networks-udr-overview.md#user-defined). Als Typ des nächsten Hops in einer benutzerdefinierten Route können Sie nur ein VPN-Gateway angeben, jedoch kein ExpressRoute-Gateway.

        Diese Option kann nicht aktiviert werden, wenn Sie ein virtuelles Netzwerk (Resource Manager) mittels Peering mit einem virtuellen Netzwerk (klassisch) verbinden. Trotz des Datenflusses zwischen zwei virtuellen Netzwerken kann der Datenverkehr des virtuellen Netzwerks (klassisch) nicht über ein Netzwerkgateway erfolgen, das mit dem virtuellen Netzwerk (Resource Manager) verbunden ist. Diese Option kann nicht aktiviert werden, wenn Sie virtuelle Netzwerken in verschiedenen Regionen mittels Peering verbinden.

    - **Remotegateways verwenden**: Aktivieren Sie dieses Kontrollkästchen, wenn Sie zulassen möchten, dass Datenverkehr von diesen virtuellen Netzwerken über ein Gateway für virtuelle Netzwerke geleitet werden soll, das mit dem mittels Peering verknüpften virtuellen Netzwerk verbunden ist. Beispielsweise ist an das virtuelle Netzwerk, mit dem Sie mittels Peering eine Verbindung herstellen, ein VPN-Gateway verbunden, dass Kommunikation mit einem lokalen Netzwerk ermöglicht.  Wird dieses Kontrollkästchen aktiviert, kann Datenverkehr aus diesem virtuellen Netzwerk über das verbundene VPN-Gateway an dieses mittels Peering verknüpfte virtuelle Netzwerk weitergeleitet werden. Wenn Sie dieses Kontrollkästchen aktivieren, muss ein Gateway für virtuelle Netzwerke mit dem mittels Peering verknüpften virtuellen Netzwerk verbunden sein. Zudem muss das Kontrollkästchen **Gatewaytransit zulassen** aktiviert sein. Wenn Sie dieses Kontrollkästchen deaktiviert (Standard) lassen, kann der Datenverkehr trotzdem von dem mittels Peering verknüpften Netzwerk an dieses virtuelle Netzwerk weitergeleitet werden, kann jedoch nicht über ein mit diesem virtuellen Netzwerk verbundenes Gateway für virtuelle Netzwerke erfolgen. 
    Diese Einstellung kann nur bei einem Peering für dieses virtuelle Netzwerk aktiviert sein.

        Wenn bereits ein Gateway in Ihrem virtuellen Netzwerk konfiguriert ist, können keine Remotegateways verwendet werden.
        Diese Option kann nicht aktiviert werden, wenn Sie ein virtuelles Netzwerk (Resource Manager) mittels Peering mit einem virtuellen Netzwerk (klassisch) verbinden. Trotz des Datenverkehrsflusses zwischen zwei virtuellen Netzwerken kann der Datenverkehr des virtuellen Netzwerks (Resource Manager) nicht über ein Netzwerkgateway erfolgen, das mit dem virtuellen Netzwerk (klassisch) verbunden ist. Diese Option kann nicht aktiviert werden, wenn Sie virtuelle Netzwerken in verschiedenen Regionen mittels Peering verbinden.

7. Klicken Sie auf **OK**, um das Peering dem ausgewählten virtuellen Netzwerk hinzuzufügen.

Ausführliche Anweisungen zum Implementieren von Peerings zwischen virtuellen Netzwerken in verschiedenen Abonnements und Bereitstellungsmodellen finden Sie unter [Nächste Schritte](#next-steps). 


### <a name="commands"></a>Befehle

- **Azure CLI**: [az network vnet peering create](/cli/azure/network/vnet/peering#create)
- **PowerShell**: [Add-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/add-azurermvirtualnetworkpeering)

## <a name="view-or-change-peering-settings"></a>Anzeigen oder Ändern von Peeringeinstellungen

Informieren Sie sich vor der Änderung eines Peerings über die [Anforderungen und Einschränkungen](#requirements-and-contstraints) sowie die [erforderlichen Berechtigungen](#permissions).

1. Geben Sie oben im Portal *Virtuelle Netzwerke* in das Suchfeld ein. Wenn **Virtuelle Netzwerke** in den Suchergebnissen angezeigt wird, können Sie den Begriff auswählen. Aktivieren Sie nicht **Virtuelle Netzwerke (klassisch)**, wenn diese Option in der Liste angezeigt wird, da über das klassische Bereitstellungsmodell kein Peering von einem bereitgestellten virtuellen Netzwerk aus erstellt werden kann.
2. Wählen Sie das virtuelle Netzwerk in der Liste aus, für das Sie Peering-Einstellungen ändern möchten.
3. Wählen Sie in der Liste der virtuellen Netzwerke das virtuelle Netzwerk aus, für das Sie Peering-Einstellungen ändern möchten.
4. Wählen Sie unter **EINSTELLUNGEN** die Option **Peerings** aus.
5. Wählen Sie das Peering aus, dessen Einstellungen angezeigt oder geändert werden sollen.
6. Ändern Sie die entsprechende Einstellung. Lesen Sie die Informationen zu den Optionen für jede Einstellung in [Schritt 6](#add-peering) von „Erstellen eines Peerings“. 
7. Wählen Sie **Speichern**aus.

**Befehle**

- **Azure CLI**: [az network vnet peering list](/cli/azure/network/vnet/peering#az_network_vnet_peering_list) zum Auflisten von Peerings für ein virtuelles Netzwerk, [az network vnet peering show](/cli/azure/network/vnet/peering#az_network_vnet_peering_show) zum Anzeigen von Einstellungen für ein bestimmtes Peering und [az network vnet peering update](/cli/azure/network/vnet/peering#az_network_vnet_peering_update) zum Ändern von Peeringeinstellungen|
- **PowerShell**: [Get-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/get-azurermvirtualnetworkpeering) zum Abrufen bzw. Anzeigen von Peeringeinstellungen und [Set-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/set-azurermvirtualnetworkpeering) zum Ändern der Einstellungen

## <a name="delete-a-peering"></a>Löschen eines Peerings

Stellen Sie vor dem Löschen eines Peerings sicher, dass Ihr Konto über die [erforderlichen Berechtigungen](#permissions) verfügt.

Wenn ein Peering gelöscht wird, wird der Datenverkehr von einem virtuellen Netzwerk nicht mehr an das mittels Peering verknüpfte virtuelle Netzwerk weitergeleitet. Wenn bereitgestellte virtuelle Netzwerke mittels Peering über Resource Manager verbunden werden, weist jedes virtuelle Netzwerk ein Peering mit dem anderen virtuellen Netzwerk auf. Obwohl durch das Löschen des Peerings von einem virtuellen Netzwerk die Kommunikation zwischen den virtuellen Netzwerken deaktiviert wird, wird das Peering vom anderen virtuellen Netzwerk nicht gelöscht. Der Peeringstatus für das Peering, das im anderen virtuellen Netzwerk vorhanden ist, lautet **Getrennt**. Sie können das Peering erst neu erstellen, wenn Sie das Peering im ersten virtuellen Netzwerk neu erstellen, und der Peeringstatus bei beiden virtuellen Netzwerken zu *Verbunden* wechselt. 

Wenn Sie möchten, dass virtuelle Netzwerke gelegentlich, jedoch nicht immer, kommunizieren sollen, können Sie die Einstellung **Zugriff auf virtuelles Netzwerk zulassen** auf **Deaktiviert** festlegen, statt das Peering zu löschen. Informationen hierzu finden Sie in Schritt 6 des Abschnitts [Erstellen eines Peerings](#create-peering) dieses Artikels. Es kann ggf. einfacher sein, den Netzwerkzugriff zu deaktivieren und zu aktivieren, statt die Peerings zu löschen und neu zu erstellen.

1. Geben Sie oben im Portal *Virtuelle Netzwerke* in das Suchfeld ein. Wenn **Virtuelle Netzwerke** in den Suchergebnissen angezeigt wird, können Sie den Begriff auswählen. Aktivieren Sie nicht **Virtuelle Netzwerke (klassisch)**, wenn diese Option in der Liste angezeigt wird, da über das klassische Bereitstellungsmodell kein Peering von einem bereitgestellten virtuellen Netzwerk aus erstellt werden kann.
2. Wählen Sie das virtuelle Netzwerk in der Liste aus, für das Sie ein Peering löschen möchten.
3. Wählen Sie in der Liste der virtuellen Netzwerke das virtuelle Netzwerk aus, für das Sie ein Peering löschen möchten.
4. Wählen Sie unter **EINSTELLUNGEN** die Option **Peerings** aus.
5. Wählen Sie auf der rechten Seite des zu löschenden Peerings die Optionen **...**, **Löschen** und **Ja**, um das Peering aus dem ersten virtuellen Netzwerk löschen.
6. Führen Sie die vorherigen Schritte durch, um das Peering aus dem anderen virtuellen Netzwerk im Peering zu löschen.

**Befehle**

- **Azure CLI**: [az network vnet peering delete](/cli/azure/network/vnet/peering#az_network_vnet_peering_delete)
- **PowerShell**: [Remove-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/remove-azurermvirtualnetworkpeering)

## <a name="requirements-and-constraints"></a>Anforderungen und Einschränkungen 

- Die mittels Peering verknüpften virtuellen Netzwerke dürfen keine sich überschneidenden IP-Adressräume aufweisen.
- Sie können Adressbereiche zum Adressraum eines virtuellen Netzwerks hinzufügen oder aus diesem löschen, nachdem ein virtuelles Netzwerk per Peering mit einem anderen virtuellen Netzwerk verknüpft wurde. Um Adressbereiche hinzuzufügen oder zu entfernen, das Peering zu löschen oder Adressräume hinzuzufügen oder zu entfernen, erstellen Sie das Peering erneut. Informationen zum Hinzufügen oder Entfernen von Adressbereichen in virtuellen Netzwerken finden Sie unter [Verwalten virtueller Netzwerke](manage-virtual-network.md).
- Sie können zwei über Resource Manager bereitgestellte virtuelle Netzwerke oder ein über Resource Manager bereitgestelltes virtuelles Netzwerk per Peering mit einem virtuellen Netzwerk verknüpfen, das über das klassische Bereitstellungsmodell bereitgestellt wurde. Sie können nicht zwei virtuelle Netzwerke, die mit dem klassischen Bereitstellungsmodell erstellt wurden, mittels Peering miteinander verknüpfen. Wenn Sie mit den Azure-Bereitstellungsmodellen nicht vertraut sind, lesen Sie den Artikel [Grundlegendes zu Azure-Bereitstellungsmodellen](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Sie können ein [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V) verwenden, um zwei virtuelle Netzwerke zu verbinden, die mit dem klassischen Bereitstellungsmodell erstellt wurden.
- Wenn zwei virtuelle Netzwerke, die mit dem Resource Manager erstellt wurden, mittels Peering miteinander verknüpft werden, muss für jedes virtuelle Netzwerk im Peering ein Peering konfiguriert werden. Für den Peeringstatus wird einer der folgenden Typen angezeigt: 
    - *Initiiert:* Wenn Sie das Peering vom ersten virtuellen Netzwerk mit dem zweiten virtuellen Netzwerk erstellen, lautet der Peeringstatus *Initiiert*. 
    - *Verbunden:* Wenn Sie das Peering vom zweiten virtuellen Netzwerk mit dem ersten virtuellen Netzwerk erstellen, lautet der Peeringstatus *Verbunden*. Wenn Sie sich den Peeringstatus für das erste virtuelle Netzwerk ansehen, erkennen Sie, dass der Status von *Initiiert* zu *Verbunden* gewechselt ist. Das Peering wurde erst erfolgreich erstellt, wenn der Peeringstatus für beide Peerings virtueller Netzwerk *Verbunden* lautet.
- Wenn ein über Resource Manager erstelltes virtuelles Netzwerk mittels Peering mit einem über das klassische Bereitstellungsmodell erstellten virtuellen Netzwerk verbunden wird, konfigurieren Sie nur ein Peering für das virtuelle Netzwerk, das über Resource Manager bereitgestellt wurde. Es ist jedoch nicht möglich, ein Peering für ein virtuelles Netzwerk (klassisch) oder zwischen zwei virtuellen Netzwerken zu konfigurieren, die über das klassische Bereitstellungsmodell bereitgestellt wurden. Wenn Sie das virtuelle Netzwerk (Resource Manager) mittels Peering mit dem virtuellen Netzwerk (klassisch) verbinden, lautet der Peeringstatus *Wird aktualisiert* und wechselt anschließend gleich zu *Verbunden*.
- Ein Peering wird zwischen zwei virtuellen Netzwerken eingerichtet. Peerings sind nicht transitiv. Wenn Sie Peerings zwischen Folgendem erstellen:
    - VirtualNetwork1 & VirtualNetwork2
    - VirtualNetwork2 & VirtualNetwork3

  Es erfolgt kein Peering zwischen VirtualNetwork1 und VirtualNetwork3 über VirtualNetwork2. Wenn Sie ein Peering zwischen virtuellen Netzwerken für VirtualNetwork1 und VirtualNetwork3 erstellen möchten, müssen Sie ein Peering zwischen VirtualNetwork1 und VirtualNetwork3 erstellen.
- Über die standardmäßige Azure-Namensauflösung können keine Namen in virtuellen Netzwerken aufgelöst werden, die mittels Peering verbunden sind. Um Namen in anderen virtuellen Netzwerken aufzulösen, müssen Sie [Azure DNS für private Domänen](../dns/private-dns-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) oder einen benutzerdefinierten DNS-Server verwenden. Weitere Informationen zum Einrichten Ihres DNS-Servers finden Sie unter [Namensauflösung mithilfe eines eigenen DNS-Servers](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server).
- Ressourcen in per Peering verbundenen virtuellen Netzwerken in derselben Region können mit derselben Bandbreite und Latenz miteinander kommunizieren, als ob sie sich im selben virtuellen Netzwerk befinden würden. Für die Größe jedes virtuellen Computers liegt jedoch eine eigene maximale Netzwerkbandbreite. Weitere Informationen zur Bandbreitenobergrenze für verschiedene VM-Größen finden Sie in den Artikeln zu den Größen virtueller Computer unter [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) oder [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Die Abonnements, die den beiden zum Peering vorgesehenen virtuellen Netzwerken zugewiesen sind, müssen demselben Azure Active Directory-Mandanten zugeordnet sein. Wenn Sie noch keinen AD-Mandanten besitzen, [erstellen Sie schnell einen](../active-directory/develop/active-directory-howto-tenant.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-new-azure-ad-tenant). Sie können ein [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V) verwenden, um zwei virtuelle Netzwerke in unterschiedlichen Abonnements zu verknüpfen, die verschiedenen Active Directory-Mandanten zugewiesen sind.
- Ein virtuelles Netzwerk kann mittels Peering mit einem anderen virtuellen Netzwerk verbunden werden; dies gilt auch für ein anderes virtuelles Netzwerk mit einem virtuellen Gateway für virtuelle Azure-Netzwerke. Wenn die virtuellen Netzwerke sowohl mittels Peering als auch mithilfe eines Gateways verbunden sind, wird der Datenverkehr zwischen den virtuellen Netzwerken über die Peeringkonfiguration statt über das Gateway geleitet.
- Für ein- und ausgehenden Datenverkehr, der VNET-Peering verwendet, fällt eine Gebühr an. Weitere Informationen hierzu finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/virtual-network).
* <a name="cross-region"></a>Sie können virtuelle Netzwerke in derselben Region oder in verschiedenen Regionen per Peering verknüpfen. Die folgenden Einschränkungen gelten nicht, wenn sich beide virtuelle Netzwerke in der *gleichen* Region befinden, sondern nur, wenn sie global per Peering verknüpft werden: 
    - Die virtuellen Netzwerke können nur in den folgenden Regionen vorhanden sein: USA, Westen-Mitte (Wyoming), USA, Westen 2 (Washington), USA, Mitte (Iowa), USA, Osten 2 (Virginia), Kanada, Mitte (Toronto), Kanada, Osten (Quebec City), Asien-Pazifik, Südosten (Singapur) Korea, Süden (Buscan), Indien, Süden (Chennai), Indien, Mitte (Pune), Indien, Westen (Mumbai), Vereinigtes Königreich, Süden (London), Vereinigtes Königreich, Westen (Cardiff), Europa, Westen (Niederlande)
    - Ressourcen in einem virtuellen Netzwerk können nicht mit der IP-Adresse eines internen Azure-Lastenausgleichs in einem per Peering verbundenen virtuellen Netzwerk kommunizieren. Der Lastenausgleich und die Ressourcen, die mit diesem kommunizieren, müssen sich im selben virtuellen Netzwerk befinden.
    - Sie können keine Remotegateways verwenden oder einen Gatewaytransit zulassen. Dies ist nur möglich, wenn sich beide virtuelle Netzwerke im Peering in derselben Region befinden. 

## <a name="permissions"></a>Berechtigungen

Die Konten, mit denen Sie das Peering in virtuellen Netzwerken erstellt haben, müssen die erforderliche Rolle oder die erforderlichen Berechtigungen haben. Wenn Sie beispielsweise ein Peering zwischen den virtuellen Netzwerken *myVnetA* und *myVnetB* erstellen, muss Ihrem Konto mindestens die folgende Rolle oder die folgenden Berechtigungen für jedes virtuelle Netzwerk zugewiesen sein:
    
|Virtuelles Netzwerk|Bereitstellungsmodell|Rolle|Berechtigungen|
|---|---|---|---|
|myVnetA|Ressourcen-Manager|[Mitwirkender von virtuellem Netzwerk](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write|
| |Klassisch|[Mitwirkender von klassischem Netzwerk](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|N/V|
|myVnetB|Ressourcen-Manager|[Mitwirkender von virtuellem Netzwerk](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/peer|
||Klassisch|[Mitwirkender von klassischem Netzwerk](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|Microsoft.ClassicNetwork/virtualNetworks/peer|

Erfahren Sie mehr über [integrierte Rollen](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) und das Zuweisen bestimmter Berechtigungen zu [benutzerdefinierten Rollen](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (nur für Resource Manager).

## <a name="next-steps"></a>Nächste Schritte

* Ein Peering für virtuelle Netzwerke wird zwischen virtuellen Netzwerken erstellt, die mit dem gleichen oder unterschiedlichen Bereitstellungsmodellen unter demselben oder unterschiedlichen Abonnements erstellt wurden. Arbeiten Sie ein Tutorial für eines der folgenden Szenarien durch:

    |Azure-Bereitstellungsmodell             | Abonnement  |
    |---------                          |---------|
    |Beide mit Resource Manager              |[Gleich](tutorial-connect-virtual-networks-portal.md)|
    |                                   |[Unterschiedlich](create-peering-different-subscriptions.md)|
    |Einmal Resource Manager, einmal klassisch  |[Gleich](create-peering-different-deployment-models.md)|
    |                                   |[Unterschiedlich](create-peering-different-deployment-models-subscriptions.md)|

* Informieren Sie sich über das Erstellen einer [Hub-Spoke-Netzwerktopologie](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual network-peering).
