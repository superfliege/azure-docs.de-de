---
title: "Virtuelle Azure-Computer: Hinzufügen oder Entfernen von Netzwerkschnittstellenkarten | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Netzwerkschnittstellen zu virtuellen Computern hinzufügen oder aus diesen entfernen können."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/15/2017
ms.author: jdial
ms.openlocfilehash: abe6abb942d206330e809f3aef388b846d7d7c7f
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/03/2018
---
# <a name="add-network-interfaces-to-or-remove-network-interfaces-from-virtual-machines"></a>Hinzufügen von Netzwerkschnittstellen zu virtuellen Computern oder Entfernen von Netzwerkschnittstellen aus diesen

Erfahren Sie, wie eine vorhandene Netzwerkschnittstellenkarte hinzugefügt wird, wenn Sie einen virtuellen Computer erstellen, oder wie Netzwerkschnittstellen einem virtuellen Computer mit dem Zustand „Beendet (Zuordnung aufgehoben)“ hinzugefügt oder von diesem entfernt werden. Eine Netzwerkschnittstelle ermöglicht einem virtuellen Azure-Computer (VM) die Kommunikation mit dem Internet, Azure und lokalen Ressourcen. Ein virtueller Computer kann eine oder mehrere Netzwerkschnittstellen haben. 

Weitere Informationen zum Hinzufügen, Ändern oder Entfernen von IP-Adressen für eine Netzwerkschnittstelle finden Sie im Artikel [Hinzufügen, Ändern oder Entfernen von IP-Adressen für Azure-Netzwerkschnittstellen](virtual-network-network-interface-addresses.md). Informationen zum Erstellen, Ändern oder Löschen von Netzwerkschnittstellen finden Sie im Artikel [Erstellen, Ändern oder Löschen von Netzwerkschnittstellen](virtual-network-network-interface.md).

## <a name="before"></a>Voraussetzungen

Führen Sie zuerst die folgenden Aufgaben aus, ehe Sie die Schritte in den Abschnitten dieses Artikels durchführen:

- Melden Sie sich mit einem Azure-Konto beim [Azure-Portal](https://portal.azure.com), der Azure-Befehlszeilenschnittstelle (CLI) oder bei Azure PowerShell an. Falls Sie noch nicht über ein Azure-Konto verfügen, können Sie sich für ein [kostenloses Testkonto](https://azure.microsoft.com/free) registrieren.
- Wenn Sie PowerShell-Befehle verwenden, um Aufgaben in diesem Artikel durchzuführen, [installieren und konfigurieren Sie Azure PowerShell](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Stellen Sie sicher, dass Sie die neueste Version der Azure PowerShell-Cmdlets installiert haben. Hilfe und Beispiele für PowerShell-Befehle erhalten Sie durch Eingabe von `get-help <command> -full`. Anstatt Azure PowerShell zu installieren, können Sie auch Azure Cloud Shell verwenden. Azure Cloud Shell ist eine kostenlose PowerShell-Variante, die Sie direkt im Azure-Portal ausführen können. Azure PowerShell ist dabei vorinstalliert und für die Verwendung mit Ihrem Konto konfiguriert. Um Cloud Shell zu verwenden, klicken Sie auf die Cloud Shell-Schaltfläche **>_** im oberen Bereich des [Portals](https://portal.azure.com), und wählen Sie in der linken oberen Ecke „PowerShell“ aus, wenn das Shell-Fenster angezeigt wird.
- Wenn Sie Befehle der Azure-Befehlszeilenschnittstelle (CLI) verwenden, um Aufgaben in diesem Artikel durchzuführen, [installieren und konfigurieren Sie Azure CLI](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Stellen Sie sicher, dass Sie die neueste Version der Azure CLI installiert haben. Hilfe zu den Befehlen der Befehlszeilenschnittstelle erhalten Sie durch Eingabe von `az <command> --help`. Anstatt die CLI und ihre Voraussetzungen zu installieren, können Sie die Azure Cloud Shell verwenden. Azure Cloud Shell ist eine kostenlose Bash-Shell, die Sie direkt im Azure-Portal ausführen können. Die Azure CLI ist vorinstalliert und für die Verwendung mit Ihrem Konto konfiguriert. Um Cloud Shell verwenden möchten, klicken Sie auf die Cloud Shell-Schaltfläche **>_** im oberen Bereich des [Portals](https://portal.azure.com), und wählen Sie in der linken oberen Ecke „Bash“ aus, wenn das Shell-Fenster angezeigt wird.

## <a name="vm-create"></a>Hinzufügen vorhandener Netzwerkschnittstellen zu einer neuen VM

Bei der Erstellung eines virtuellen Computers im Portal wird eine Netzwerkschnittstelle mit Standardeinstellungen generiert, die für Sie zu dem virtuellen Computer hinzugefügt wird. Über das Azure-Portal ist es nicht möglich, vorhandene Netzwerkschnittstellen an einen neuen virtuellen Computer anzufügen oder einen virtuellen Computer mit mehreren Netzwerkschnittstellen zu erstellen. Verwenden Sie hierfür die CLI oder PowerShell. Vor dem Erstellen eines virtuellen Computers mit einer vorhandenen Netzwerkschnittstelle mithilfe von PowerShell oder der Befehlszeilenschnittstelle sollten Sie sich mit den [Einschränkungen](#constraints) vertraut machen. Wenn Sie einen virtuellen Computer mit mehreren Netzwerkschnittstellen erstellen, müssen Sie auch das Betriebssystem für die ordnungsgemäße Verwendung der Schnittstellen nach der Erstellung des virtuellen Computers konfigurieren. Einzelheiten finden Sie in den Hinweisen zur Konfiguration von [Linux](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) oder [Windows](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) für mehrere Netzwerkschnittstellen.

**Befehle:** Erstellen Sie vor dem Erstellen des virtuellen Computers eine Netzwerkschnittstelle mithilfe der Schritte unter [Erstellen einer Netzwerkschnittstelle](virtual-network-network-interface.md#create-a-network-interface).

|Tool|Get-Help|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az vm create](/cli/azure/vm?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="vm-add-nic"></a>Hinzufügen einer Netzwerkschnittstelle zu einem vorhandenen virtuellen Computer

1. Melden Sie sich beim Azure-Portal an.
2. Suchen Sie im Suchfeld im oberen Bereich des Portals nach dem Namen des virtuellen Computers, dem Sie die Netzwerkschnittstelle hinzufügen möchten, oder suchen Sie durch Klicken auf **Alle Dienste** und dann **Virtuelle Computer** nach dem virtuellen Computer. Wenn Sie den virtuellen Computer gefunden haben, klicken Sie darauf. Die VM, der Sie eine Netzwerkschnittstelle hinzufügen möchten, muss die gewünschte Anzahl von Netzwerkschnittstellen unterstützen. Weitere Informationen zur unterstützten Anzahl an Netzwerkschnittstellen der einzelnen VM-Größen finden Sie im entsprechenden Artikel für [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) oder [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  
3. Klicken Sie unter **EINSTELLUNGEN** auf **Übersicht**. Klicken Sie auf **Beenden**, und warten Sie, bis der **Status** des virtuellen Computers in *Beendet (Zuordnung aufgehoben)* geändert wird. 
4. Klicken Sie unter **EINSTELLUNGEN** auf **Netzwerk**.
5. Klicken Sie auf **Netzwerkschnittstelle anfügen**. Klicken Sie in der Liste der vorhandenen Netzwerkschnittstellen, die derzeit an keinen anderen virtuellen Computer angefügt sind, auf die Netzwerkschnittstelle, die Sie anfügen möchten. Auf der ausgewählten Netzwerkschnittstelle darf kein beschleunigter Netzwerkbetrieb aktiviert sein, es darf keine IPv6-Adresse zugewiesen sein, und die Schnittstelle muss sich im selben virtuellen Netzwerk befinden in dem sich die derzeit an den virtuellen Computer angefügte Netzwerkschnittstelle befindet. Wenn keine Netzwerkschnittstelle vorhanden ist, müssen Sie zuerst eine erstellen. Klicken Sie zum Erstellen einer Netzwerkschnittstelle auf **Netzwerkschnittstelle erstellen**. Weitere Informationen zum Erstellen einer Netzwerkschnittstelle finden Sie unter [Erstellen einer Netzwerkschnittstelle](virtual-network-network-interface.md#create-a-network-interface). Weitere Informationen zu zusätzlichen Einschränkungen beim Hinzufügen von Netzwerkschnittstellen zu virtuellen Computern finden Sie unter [Einschränkungen](#constraints).
6. Klicken Sie auf **OK**.
7. Klicken Sie unter **EINSTELLUNGEN** auf **Übersicht**. Klicken Sie auf **Start**, um den virtuellen Computer zu starten.
8. Konfigurieren Sie das VM-Betriebssystem für die ordnungsgemäße Verwendung mehrerer Netzwerkschnittstellen. Einzelheiten finden Sie in den Hinweisen zur Konfiguration von [Linux](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) oder [Windows](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) für mehrere Netzwerkschnittstellen.

|Tool|Get-Help|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az vm nic add](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#add) (Verweis) or [detaillierte Schritte](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-a-nic-to-a-vm)|
|PowerShell|[Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) (Verweis) oder [detaillierte Schritte](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-a-nic-to-an-existing-vm)|

## <a name="vm-view-nic"></a>Anzeigen der Netzwerkschnittstellen für einen virtuellen Computer

Sie können die derzeit zu einem virtuellen Computer hinzugefügten Netzwerkschnittstellen anzeigen, um mehr über die Konfiguration der einzelnen Netzwerkschnittstellen und die diesen zugewiesenen IP-Adressen zu erfahren. 

1. Melden Sie sich mit einem Konto, dem für Ihr Abonnement die Rolle „Besitzer“, „Mitwirkender“ oder „Netzwerkmitwirkender“ zugewiesen ist, beim [Azure-Portal](https://portal.azure.com) an. Weitere Informationen zum Zuweisen von Rollen zu Konten finden Sie unter [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Geben Sie im oberen Bereich des Azure-Portals im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *Virtuelle Computer* ein. Wenn **Virtuelle Computer** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
3. Klicken Sie auf den Namen des virtuellen Computers, dessen Netzwerkschnittstellen Sie anzeigen möchten.
4. Klicken Sie im Abschnitt **EINSTELLUNGEN** für den ausgewählten virtuellen Computer auf **Netzwerk**. Weitere Informationen zu Netzwerkschnittstellen-Einstellungen finden Sie im Artikel [Verwalten von Netzwerkschnittstellen](virtual-network-network-interface.md). Weitere Informationen zum Hinzufügen, Ändern und Entfernen von IP-Adressen, die einer Netzwerkschnittstelle zugewiesen sind, finden Sie im Artikel [Manage IP addresses (Verwalten von IP-Adressen)](virtual-network-network-interface-addresses.md).

**Befehle**

|Tool|Get-Help|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az vm show](/cli/azure/vm?toc=%2fazure%2fvirtual-network%2ftoc.json#show)|
|PowerShell|[Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="vm-remove-nic"></a> Entfernen einer Netzwerkschnittstelle von einem virtuellen Computer

1. Melden Sie sich beim Azure-Portal an.
2. Suchen Sie im Suchfeld im oberen Bereich des Portals nach dem Namen des virtuellen Computers, von dem Sie die Netzwerkschnittstelle entfernen (trennen) möchten, oder suchen Sie durch Klicken auf **Alle Dienste** und dann **Virtuelle Computer** nach dem virtuellen Computer. Wenn Sie den virtuellen Computer gefunden haben, klicken Sie darauf.
3. Klicken Sie unter **EINSTELLUNGEN** auf **Übersicht**. Klicken Sie auf **Beenden**, und warten Sie, bis der **Status** des virtuellen Computers in *Beendet (Zuordnung aufgehoben)* geändert wird. 
4. Klicken Sie unter **EINSTELLUNGEN** auf **Netzwerk**.
5. Klicken Sie auf **Netzwerkschnittstelle trennen**. Klicken Sie in der Liste der Netzwerkschnittstellen, die derzeit an den virtuellen Computer angefügt sind, auf die Netzwerkschnittstelle, die Sie trennen möchten. Wenn nur eine Netzwerkschnittstelle aufgeführt ist, können Sie diese nicht trennen, da an einen virtuellen Computer immer mindestens eine Netzwerkschnittstelle angefügt sein muss.
6. Klicken Sie auf **OK**.

**Befehle**

|Tool|Get-Help|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az vm nic remove](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#remove) (Verweis) or [detaillierte Schritte](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#remove-a-nic-from-a-vm)|
|PowerShell|[Remove-AzureRMVMNetworkInterface](/powershell/module/azurerm.compute/remove-azurermvmnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) (Verweis) oder [detaillierte Schritte](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#remove-a-nic-from-an-existing-vm)|

## <a name="next-steps"></a>Nächste Schritte
In den folgenden Artikeln erfahren Sie, wie Sie virtuelle Computer mit mehreren Netzwerkschnittstellen oder IP-Adressen erstellen:

**Befehle**

|Aufgabe|Tool|
|---|---|
|Erstellen eines virtuellen Computers mit mehreren Netzwerkschnittstellenkarten (NICs)|[CLI](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|Erstellen eines virtuellen Computers mit einer NIC und mehreren IPv4-Adressen|[CLI](virtual-network-multiple-ip-addresses-cli.md), [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|
|Erstellen eines virtuellen Computers mit einer NIC und einer IPv6-Adresse (mit Azure Load Balancer)|[CLI](../load-balancer/load-balancer-ipv6-internet-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [Azure Resource Manager-Vorlage](../load-balancer/load-balancer-ipv6-internet-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="constraints"></a>Einschränkungen

- An einen virtuellen Computer muss mindestens eine Netzwerkschnittstelle angefügt sein.
- Es können an einen virtuellen Computer nur so viele Netzwerkschnittstellen angefügt sein, wie die VM-Größe unterstützt. Weitere Informationen zur unterstützten Anzahl von Netzwerkschnittstellen der einzelnen VM-Größen finden Sie unter den VM-Größen für [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) und [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Alle Größen unterstützen mindestens zwei Netzwerkschnittstellen.
- Die Netzwerkschnittstellen, die Sie zu einem virtuellen Computer hinzufügen, können derzeit zu keinem anderen virtuellen Computer hinzugefügt werden. Weitere Informationen zum Erstellen von Netzwerkschnittstellen finden Sie unter [Erstellen einer Netzwerkschnittstelle](virtual-network-network-interface.md#create-a-network-interface).
- In der Vergangenheit konnten Netzwerkschnittstellen nur zu virtuellen Computern hinzugefügt werden, die mehrere Netzwerkschnittstellen unterstützten und mit mindestens zwei Netzwerkschnittstellen erstellt wurden. Es konnte keine Netzwerkschnittstelle zu einem virtuellen Computer hinzugefügt werden, der mit einer Netzwerkschnittstelle erstellt wurde, auch wenn die Größe des virtuellen Computers mehrere Netzwerkschnittstellen unterstützte. Im Gegenzug konnten Sie nur Netzwerkschnittstellen von einem virtuellen Computer mit mindestens drei Netzwerkschnittstellen entfernen, da virtuelle Computer, die mit zwei Netzwerkschnittstellen erstellt wurden, immer mindestens zwei Netzwerkschnittstellen haben mussten. Keine dieser Einschränkungen trifft noch zu. Sie können nun einen virtuellen Computer mit einer beliebigen Anzahl von Netzwerkschnittstellen (bis zu der von der VM-Größe unterstützten Anzahl) erstellen.
- Standardmäßig wird die erste an einen virtuellen Computer angefügte Netzwerkschnittstelle als *primäre* Netzwerkschnittstelle definiert. Alle anderen Netzwerkschnittstellen auf dem virtuellen Computer sind *sekundäre* Netzwerkschnittstellen.
- Sie können festlegen, an welche Netzwerkschnittstelle ausgehender Datenverkehr gesendet werden soll. Standardmäßig wird sämtlicher ausgehender Datenverkehr des virtuellen Computers jedoch über die IP-Adresse gesendet, die der primären IP-Konfiguration der primären Netzwerkschnittstelle zugewiesen ist.
- In der Vergangenheit mussten alle virtuellen Computer in der gleichen Verfügbarkeitsgruppe über eine einzelne Netzwerkschnittstelle (oder über mehrere Netzwerkschnittstellen) verfügen. In einer Verfügbarkeitsgruppe können nun VMs mit bis zu der von der VM-Größe unterstützten Anzahl an Netzwerkschnittstellen vertreten sein. Sie können eine VM erst einer Verfügbarkeitsgruppe hinzufügen, nachdem sie erstellt wurde. Weitere Informationen zu Verfügbarkeitsgruppen finden Sie im Artikel [Verwalten der Verfügbarkeit von VMs in Azure](../virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy).
- Netzwerkschnittstellen in derselben VM können zwar mit unterschiedlichen Subnetzen eines VNet verbunden sein, die Netzwerkschnittstellen müssen jedoch alle mit dem gleichen VNet verbunden sein.
- Sie können eine beliebige IP-Adresse für eine beliebige IP-Konfiguration einer beliebigen primären oder sekundären Netzwerkschnittstelle einem Back-End-Pool von Azure Load Balancer hinzufügen. Bisher konnte nur die primäre IP-Adresse für die primäre Netzwerkschnittstelle einem Back-End-Pool hinzugefügt werden. Weitere Informationen zu IP-Adressen und -Konfigurationen finden Sie im Artikel [Hinzufügen, Ändern oder Entfernen von IP-Adressen](virtual-network-network-interface-addresses.md).
- Beim Löschen eines virtuellen Computers werden seine Netzwerkschnittstellen nicht gelöscht. Wenn ein virtueller Computer gelöscht wird, werden die Netzwerkschnittstellen von dem virtuellen Computer getrennt. Sie können die Netzwerkschnittstellen an andere virtuelle Computer anfügen oder löschen.
- Wenn einer Netzwerkschnittstelle private IPv6-Adressen zugewiesen sind, müssen Sie sie an einen virtuellen Computer anfügen, wenn Sie diesen erstellen. Sie können einem virtuellen Computer keine Netzwerkschnittstelle mit einer zugewiesenen IPv6-Adresse mehr hinzufügen, nachdem dieser erstellt wurde. Wenn Sie eine Netzwerkschnittstelle mit einer zugewiesenen privaten IPv6-Adresse hinzufügen, wenn Sie einen virtuellen Computer erstellen, können Sie dem virtuellen Computer nur diese Netzwerkschnittstelle hinzufügen, ungeachtet davon, wie viele Netzwerkschnittstellen die VM-Größe unterstützt. Weitere Informationen zum Zuweisen von IP-Adressen an Netzwerkschnittstellen finden Sie unter [IP-Adressen für Netzwerkschnittstellen](virtual-network-network-interface-addresses.md).
- Ähnlich wie bei IPv6 können Sie eine Netzwerkschnittstelle, auf der beschleunigter Netzwerkbetrieb aktiviert ist, nicht an einen virtuellen Computer anfügen, nachdem dieser erstellt wurde. Um beschleunigten Netzwerkbetrieb nutzen zu können, müssen Sie außerdem auch Schritte innerhalb des VM-Betriebssystems ausführen. Weitere Informationen zum beschleunigten Netzwerkbetrieb und weiteren Einschränkungen bei dessen Verwendung finden Sie in Abschnitten zum beschleunigten Netzwerkbetrieb für virtuelle [Windows](create-vm-accelerated-networking-powershell.md)- oder [Linux](create-vm-accelerated-networking-cli.md)-Computer.
