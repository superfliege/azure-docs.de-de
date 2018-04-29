---
title: Hinzufügen, Ändern und Löschen von Subnetzen virtueller Azure-Netzwerke | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Subnetze virtueller Netzwerke in Azure hinzufügen, ändern oder löschen.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/09/2018
ms.author: jdial
ms.openlocfilehash: 00b0635d894ff8a41c810345cfa94363029e78e8
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2018
---
# <a name="add-change-or-delete-a-virtual-network-subnet"></a>Hinzufügen, Ändern oder Löschen von Subnetzen virtueller Netzwerke

Erfahren Sie, wie Sie Subnetze virtueller Netzwerke hinzufügen, ändern oder löschen. Wenn Sie mit virtuellen Netzwerken nicht vertraut sind, sollten Sie vor dem Hinzufügen, Ändern oder Löschen eines Subnetzes die Artikel [Übersicht über Azure Virtual Network](virtual-networks-overview.md) und [Erstellen, Ändern oder Löschen von virtuellen Netzwerken](manage-virtual-network.md) lesen. Alle in einem virtuellen Netzwerk bereitgestellten Azure-Ressourcen werden in einem Subnetz innerhalb eines virtuellen Netzwerks bereitgestellt.
 
## <a name="before-you-begin"></a>Voraussetzungen

Führen Sie zuerst die folgenden Aufgaben aus, ehe Sie die Schritte in den Abschnitten dieses Artikels durchführen:

- Falls Sie noch nicht über ein Azure-Konto verfügen, können Sie sich für ein [kostenloses Testkonto](https://azure.microsoft.com/free) registrieren.
- Öffnen Sie bei Verwendung des Portals https://portal.azure.com, und melden Sie sich mit Ihrem Azure-Konto an.
- Wenn Sie PowerShell-Befehle zum Durchführen von Aufgaben in diesem Artikel verwenden, führen Sie die Befehle entweder in [Azure Cloud Shell](https://shell.azure.com/powershell) oder durch Ausführen von PowerShell auf Ihrem Computer aus. Azure Cloud Shell ist eine kostenlose interaktive Shell, mit der Sie die Schritte in diesem Artikel ausführen können. Sie verfügt über allgemeine vorinstallierte Tools und ist für die Verwendung mit Ihrem Konto konfiguriert. Für dieses Tutorial ist das Azure PowerShell-Modul Version 5.2.0 oder höher erforderlich. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die installierte Version zu ermitteln. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen.
- Wenn Sie Befehle der Azure-Befehlszeilenschnittstelle (CLI) zum Durchführen von Aufgaben in diesem Artikel verwenden, führen Sie die Befehle entweder in [Azure Cloud Shell](https://shell.azure.com/bash) oder durch Ausführen der CLI auf Ihrem Computer aus. Für dieses Tutorial ist mindestens Version 2.0.26 der Azure CLI erforderlich. Führen Sie `az --version` aus, um die installierte Version zu ermitteln. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0](/cli/azure/install-azure-cli) Informationen dazu. Wenn Sie die Azure CLI lokal ausführen, müssen Sie auch `az login` ausführen, um eine Verbindung mit Azure herzustellen.

## <a name="add-a-subnet"></a>Hinzufügen eines Subnetzes

1. Geben Sie oben im Portal *Virtuelle Netzwerke* in das Suchfeld ein. Wenn **Virtuelle Netzwerke** in den Suchergebnissen angezeigt wird, können Sie den Begriff auswählen.
2. Wählen Sie in der Liste der virtuellen Netzwerke das virtuelle Netzwerk aus, dem Sie ein Subnetz hinzufügen möchten.
3. Wählen Sie unter **EINSTELLUNGEN** die Option **Subnetze** aus.
4. Wählen Sie **+ Subnetz** aus.
5. Geben Sie Werte für die folgenden Parameter ein:
    - **Name:** Der Name muss innerhalb des virtuellen Netzwerks eindeutig sein.
    - **Adressbereich:** Der Bereich muss innerhalb des Adressraums für das virtuelle Netzwerk eindeutig sein. Der Bereich darf keine Überschneidungen mit anderen Subnetzadressbereichen innerhalb des virtuellen Netzwerks aufweisen. Der Adressraum muss mithilfe der CIDR-Notation (Classless Inter-Domain Routing, klassenloses domänenübergreifendes Routing) angegeben werden. In einem virtuellen Netzwerk mit dem Adressraum 10.0.0.0/16 können Sie beispielsweise für ein Subnetz den Adressraum 10.0.0.0/24 definieren. Der kleinste Bereich, den Sie angeben können, ist /29, der acht IP-Adressen für das Subnetz bereitstellt. Azure reserviert die erste und letzte Adresse in jedem Subnetz aus Gründen der Protokollkonformität. Drei weitere Adressen sind für die Nutzung durch Azure-Dienste reserviert. Demzufolge führt das Einrichten eines Subnetzes mit dem Adressbereich /29 zu drei nutzbaren IP-Adressen im Subnetz. Wenn Sie ein virtuelles Netzwerk mit einem VPN-Gateway verbinden möchten, müssen Sie ein Gatewaysubnetz erstellen. Weitere Informationen finden Sie unter [Informationen zu VPN Gateway-Einstellungen](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub) im Abschnitt „Gatewaysubnetz“. Unter bestimmten Bedingungen können Sie den Adressbereich ändern, nachdem das Subnetz hinzugefügt wurde. Informationen zum Ändern des Adressbereichs eines Subnetzes finden Sie im Abschnitt [Ändern der Subnetzeinstellungen](#change-subnet-settings).
    - **Netzwerksicherheitsgruppe:** Sie können einem Subnetz keine oder eine vorhandene Netzwerksicherheitsgruppe zuordnen, um ein- und ausgehenden Netzwerkdatenverkehr für das Subnetz zu filtern. Die Netzwerksicherheitsgruppe muss demselben Abonnement und Standort wie das virtuelle Netzwerk angehören. Erfahren Sie mehr über [Netzwerksicherheitsgruppen](security-overview.md) und das [Erstellen einer Netzwerksicherheitsgruppe](tutorial-filter-network-traffic.md).
    - **Routingtabelle:** Sie können einem Subnetz keine oder eine vorhandene Routingtabelle zuordnen, um die Weiterleitung von Netzwerkdatenverkehr an andere Netzwerke zu steuern. Die Routingtabelle muss demselben Abonnement und Standort wie das virtuelle Netzwerk angehören. Erfahren Sie mehr über [Azure-Routing](virtual-networks-udr-overview.md) und das [Erstellen einer Routingtabelle](tutorial-create-route-table-portal.md).
    - **Dienstendpunkte:** Für ein Subnetz können keine oder mehrere Dienstendpunkte aktiviert sein. Zum Aktivieren eines Dienstendpunkts für einen Dienst wählen Sie Dienste, für die Sie Dienstendpunkte aktivieren möchten, in der Liste **Dienste** aus. Zum Entfernen eines Dienstendpunkts heben Sie die Auswahl des Diensts auf, für den der Dienstendpunkt entfernt werden soll. Weitere Informationen zu Dienstendpunkten finden Sie unter der Übersicht über [Dienstendpunkte im virtuellen Netzwerk](virtual-network-service-endpoints-overview.md). Nach dem Aktivieren eines Dienstendpunkts für einen Dienst müssen Sie auch Netzwerkzugriff für das Subnetz für eine mit dem Dienst erstellte Ressource aktivieren. Beispiel: Wenn Sie den Dienstendpunkt für *Microsoft.Storage* aktivieren, müssen Sie auch Netzwerkzugriff auf alle Azure Storage-Konten aktivieren, auf die Netzwerkzugriff gewährt werden soll. Ausführliche Informationen zum Aktivieren des Netzwerkzugriffs auf Subnetze, für die ein Dienstendpunkt aktiviert ist, finden Sie in der Dokumentation zum jeweiligen Dienst, für den Sie den Dienstendpunkt aktiviert haben.
6. Um das Subnetz dem ausgewählten virtuellen Netzwerk hinzuzufügen, wählen Sie **OK**.

**Befehle**

- Azure CLI: [az network vnet subnet create](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create)
- PowerShell: [Add-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig)

## <a name="change-subnet-settings"></a>Ändern von Subnetzeinstellungen

1. Geben Sie oben im Portal *Virtuelle Netzwerke* in das Suchfeld ein. Wenn **Virtuelle Netzwerke** in den Suchergebnissen angezeigt wird, können Sie den Begriff auswählen.
2. Wählen Sie in der Liste der virtuellen Netzwerke das virtuelle Netzwerk mit dem Subnetz aus, für das Sie Einstellungen ändern möchten.
3. Wählen Sie unter **EINSTELLUNGEN** die Option **Subnetze** aus.
4. Wählen Sie in der Liste der Subnetze das Subnetz aus, für das Sie Einstellungen ändern möchten. Sie können die folgenden Einstellungen ändern:

    - **Adressbereich**: Wenn innerhalb des Subnetzes keine Ressourcen bereitgestellt sind, können Sie den Adressbereich ändern. Wenn Ressourcen im Subnetz vorhanden sind, müssen Sie die Ressourcen zuerst entweder in ein anderes Subnetz verschieben oder aus dem Subnetz löschen. Die Schritte zum Verschieben oder Löschen einer Ressource variieren abhängig von der Ressource. Informationen zum Verschieben oder Löschen von Ressourcen in Subnetzen finden Sie in der Dokumentation zum jeweiligen Ressourcentyp, den Sie verschieben oder löschen möchten. Die Einschränkungen für den **Adressbereich** finden Sie in Schritt 5 von [Hinzufügen eines Subnetzes](#add-a-subnet).
    - **Benutzer:** Sie können den Zugriff auf das Subnetz über integrierte Rollen oder eigene benutzerdefinierte Rollen steuern. Weitere Informationen zum Zuweisen von Rollen und Benutzern für den Zugriff auf das Subnetz finden Sie unter [Verwenden von Rollenzuweisungen zum Verwalten des Zugriffs auf Ihre Azure-Ressourcen](../role-based-access-control/role-assignments-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-access).
    - Weitere Informationen zum Ändern von **Netzwerksicherheitsgruppen**, **Routingtabellen**, **Benutzern** und **Dienstendpunkten** finden Sie in Schritt 5 von [Hinzufügen eines Subnetzes](#add-a-subnet).
5. Wählen Sie **Speichern**aus.

**Befehle**

- Azure CLI: [az network vnet subnet update](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-update)
- PowerShell: [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig)

## <a name="delete-a-subnet"></a>Löschen eines Subnetzes

Sie können ein Subnetz erst löschen, wenn es keine Ressourcen mehr enthält. Falls das Subnetz noch Ressourcen enthält, müssen Sie diese Ressourcen zunächst löschen, bevor das Subnetz gelöscht werden kann. Die Schritte zum Löschen einer Ressource variieren abhängig von der Ressource. Informationen zum Löschen von Ressourcen in Subnetzen finden Sie in der Dokumentation zum jeweiligen Ressourcentyp, den Sie löschen möchten.

1. Geben Sie oben im Portal *Virtuelle Netzwerke* in das Suchfeld ein. Wenn **Virtuelle Netzwerke** in den Suchergebnissen angezeigt wird, können Sie den Begriff auswählen.
2. Wählen Sie in der Liste der virtuellen Netzwerke das virtuelle Netzwerk mit dem Subnetz aus, das Sie löschen möchten.
3. Wählen Sie unter **EINSTELLUNGEN** die Option **Subnetze** aus.
4. Wählen Sie in der Liste der Subnetze auf der rechten Seite **...** für das Subnetz, das Sie löschen möchten.
5. Wählen Sie **Löschen** und dann **Ja**.

**Befehle**

- Azure CLI: [az network vnet delete](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-delete)
- PowerShell: [Remove-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/remove-azurermvirtualnetworksubnetconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)

## <a name="permissions"></a>Berechtigungen

Zum Durchführen von Aufgaben für Subnetze muss Ihr Konto der Rolle [Netzwerkmitwirkender](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) oder einer [benutzerdefinierten](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) Rolle zugewiesen sein, die über die entsprechenden Berechtigungen in der folgenden Tabelle verfügt:

|Vorgang                                                                |   Vorgangsname                               |
|-----------------------------------------------------------------------  |   -------------------------------------------  |
|Microsoft.Network/virtualNetworks/subnets/read                           |   Subnetz des virtuellen Netzwerks abrufen                   |
|Microsoft.Network/virtualNetworks/subnets/write                          |   Virtual Network-Subnetz erstellen oder aktualisieren      |
|Microsoft.Network/virtualNetworks/subnets/delete                         |   Subnetz des virtuellen Netzwerks löschen                |
|Microsoft.Network/virtualNetworks/subnets/join/action                    |   Virtuellem Netzwerk beitreten                         |
|Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action  |   Dienst mit einem Subnetz verknüpfen                     |
|Microsoft.Network/virtualNetworks/subnets/virtualMachines/read           |   Virtuelle Computer für ein Virtual Network-Subnetz abrufen  |
