---
title: 'Einschränken des Netzwerkzugriffs auf PaaS-Ressourcen: Azure PowerShell | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie den Netzwerkzugriff auf Azure-Ressourcen wie Azure Storage und die Azure SQL-Datenbank mit virtuellen Netzwerkdienstendpunkten mithilfe von PowerShell einschränken können.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: ''
ms.topic: ''
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/14/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: 7e402af74babda2ce32d4a1597c61d71aba89b9e
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="restrict-network-access-to-paas-resources-with-virtual-network-service-endpoints-using-powershell"></a>Einschränken des Netzwerkzugriffs auf PaaS-Ressourcen mit virtuellen Netzwerkdienstendpunkten mithilfe von PowerShell

Virtuelle Netzwerkdienstendpunkte ermöglichen es Ihnen, den Netzwerkzugriff auf einige Azure-Dienstressourcen auf ein Subnetz eines virtuellen Netzwerks einzuschränken. Sie können auch den Internetzugriff auf die Ressourcen entfernen. Dienstendpunkte ermöglichen eine direkte Verbindung zwischen Ihrem virtuellen Netzwerk und unterstützten Azure-Diensten, sodass Sie mithilfe des privaten Adressraums Ihres virtuellen Netzwerks auf die Azure-Dienste zugreifen können. Datenverkehr, der über Dienstendpunkte für Azure-Ressourcen bestimmt ist, verbleibt immer im Microsoft Azure-Backbonenetzwerk. In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Erstellen eines virtuellen Netzwerks mit einem Subnetz
> * Hinzufügen eines Subnetzes und Aktivieren eines Dienstendpunkts
> * Erstellen einer Azure-Ressource und Zulassen des Netzwerkzugriffs darauf ausschließlich aus einem Subnetz
> * Bereitstellen eines virtuellen Computers für jedes Subnetz
> * Bestätigen des Zugriffs auf eine Ressource aus einem Subnetz
> * Bestätigen, dass der Zugriff auf eine Ressource aus einem Subnetz und dem Internet verweigert wird

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Wenn Sie PowerShell lokal installieren und verwenden möchten, müssen Sie für diesen Artikel mindestens Version 5.4.1 des Azure PowerShell-Moduls verwenden. Führen Sie ` Get-Module -ListAvailable AzureRM` aus, um die installierte Version zu ermitteln. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-azurerm-ps) Informationen dazu. Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Login-AzureRmAccount` ausführen, um eine Verbindung mit Azure herzustellen.

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

Vor der Erstellung eines virtuellen Netzwerks müssen Sie eine Ressourcengruppe für das virtuelle Netzwerk und alle anderen in diesem Artikel erstellten Ressourcen erstellen. Erstellen Sie mit [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) eine Ressourcengruppe. Im folgenden Beispiel wird die Ressourcengruppe *myResourceGroup* erstellt: 

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

Erstellen Sie mit [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) ein virtuelles Netzwerk. Im folgenden Beispiel wird ein virtuelles Netzwerk namens *myVirtualNetwork* mit dem Adresspräfix *10.0.0.0/16* erstellt.

```azurepowershell-interactive
$virtualNetwork = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

Erstellen Sie mit [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) eine Subnetzkonfiguration. Im folgenden Beispiel wird eine Subnetzkonfiguration für das Subnetz *Public* erstellt:

```azurepowershell-interactive
$subnetConfigPublic = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Public `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork
```

Erstellen Sie das Subnetz im virtuellen Netzwerk, indem Sie die Subnetzkonfiguration mit [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork) in das virtuelle Netzwerk schreiben:

```azurepowershell-interactive
$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="enable-a-service-endpoint"></a>Aktivieren eines Dienstendpunkts 

Sie können Dienstendpunkte nur für Dienste aktivieren, die Dienstendpunkte unterstützen. Dienstendpunktfähige Dienste, die an einem Azure-Standort verfügbar sind, können Sie mit [Get-AzureRmVirtualNetworkAvailableEndpointService](/powershell/module/azurerm.network/get-azurermvirtualnetworkavailableendpointservice) anzeigen. Im folgenden Beispiel wird eine Liste der dienstendpunktfähigen Dienste zurückgegeben, die in der Region *eastus* verfügbar sind. Die Liste der zurückgegebenen Dienste wird im Lauf der Zeit länger, da immer mehr Azure-Dienste für Dienstendpunkte aktiviert werden.

```azurepowershell-interactive
Get-AzureRmVirtualNetworkAvailableEndpointService -Location eastus | Select Name
``` 

Erstellen Sie ein weiteres Subnetz im virtuellen Netzwerk. In diesem Beispiel wird das Subnetz *Private* mit einem Dienstendpunkt für *Microsoft.Storage* erstellt: 

```azurepowershell-interactive
$subnetConfigPrivate = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Private `
  -AddressPrefix 10.0.1.0/24 `
  -VirtualNetwork $virtualNetwork `
  -ServiceEndpoint Microsoft.Storage

$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="restrict-network-access-to-and-from-a-subnet"></a>Einschränken des Netzwerkzugriffs auf das Subnetz und aus diesem

Erstellen Sie mit [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) Netzwerksicherheitsgruppen-Sicherheitsregeln. Die folgende Regel erlaubt den ausgehenden Zugriff auf die öffentlichen IP-Adressen, die dem Azure Storage-Dienst zugewiesen sind: 

```azurepowershell-interactive
$rule1 = New-AzureRmNetworkSecurityRuleConfig `
  -Name Allow-Storage-All `
  -Access Allow `
  -DestinationAddressPrefix Storage `
  -DestinationPortRange * `
  -Direction Outbound `
  -Priority 100 `
  -Protocol * `
  -SourceAddressPrefix VirtualNetwork `
  -SourcePortRange *
```

Die folgende Regel verweigert den Zugriff auf alle öffentlichen IP-Adressen. Die vorherige Regel überschreibt diese Regel. Dies ist auf die höhere Priorität zurückzuführen, die den Zugriff auf die öffentlichen IP-Adressen von Azure Storage zulässt.

```azurepowershell-interactive
$rule2 = New-AzureRmNetworkSecurityRuleConfig `
  -Name Deny-internet-All `
  -Access Deny `
  -DestinationAddressPrefix Internet `
  -DestinationPortRange * `
  -Direction Outbound `
  -Priority 110 `
  -Protocol * `
  -SourceAddressPrefix VirtualNetwork `
  -SourcePortRange *
```

Die folgende Regel erlaubt eingehenden RDP-Datenverkehr (Remote Desktop Protocol) in das Subnetz von überall aus. Remotedesktopverbindungen sind im Subnetz zulässig, sodass Sie den Netzwerkzugriff auf eine Ressource in einem späteren Schritt bestätigen können.

```azurepowershell-interactive
$rule3 = New-AzureRmNetworkSecurityRuleConfig `
  -Name Allow-RDP-All `
  -Access Allow `
  -DestinationAddressPrefix VirtualNetwork `
  -DestinationPortRange 3389 `
  -Direction Inbound `
  -Priority 120 `
  -Protocol * `
  -SourceAddressPrefix * `
  -SourcePortRange *
```

Erstellen Sie eine Netzwerksicherheitsgruppe mit [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup). Im folgenden Beispiel wird eine Netzwerksicherheitsgruppe namens *myNsgPrivate* erstellt.

```azurepowershell-interactive
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myNsgPrivate `
  -SecurityRules $rule1,$rule2,$rule3
```

Ordnen Sie die Netzwerksicherheitsgruppe mit [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) dem Subnetz *Private* hinzu, und schreiben Sie dann die Subnetzkonfiguration in das virtuelle Netzwerk. Im folgenden Beispiel wird die Netzwerksicherheitsgruppe *myNsgPrivate* dem Subnetz *Private* hinzugefügt:

```azurepowershell-interactive
Set-AzureRmVirtualNetworkSubnetConfig `
  -VirtualNetwork $VirtualNetwork `
  -Name Private `
  -AddressPrefix 10.0.1.0/24 `
  -ServiceEndpoint Microsoft.Storage `
  -NetworkSecurityGroup $nsg

$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="restrict-network-access-to-a-resource"></a>Einschränken des Netzwerkzugriffs auf eine Ressource

Die Schritte, die erforderlich sind, um den Netzwerkzugriff auf Ressourcen einzuschränken, die durch Azure-Dienste erstellt und für Dienstendpunkte aktiviert wurden, sind je nach Dienst unterschiedlich. Informationen zu den Schritten für einzelne Dienste finden Sie in der Dokumentation des jeweiligen Diensts. Der Rest dieses Artikels enthält als Beispiel Schritte zum Einschränken des Netzwerkzugriffs für ein Azure Storage-Konto.

### <a name="create-a-storage-account"></a>Speicherkonto erstellen

Erstellen Sie mit [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) ein Azure Storage-Konto. Ersetzen Sie `<replace-with-your-unique-storage-account-name>` durch einen Namen, der an allen Azure-Standorten eindeutig, zwischen 3 und 24 Zeichen lang ist und nur aus Ziffern und Kleinbuchstaben besteht.

```azurepowershell-interactive
$storageAcctName = '<replace-with-your-unique-storage-account-name>'

New-AzureRmStorageAccount `
  -Location EastUS `
  -Name $storageAcctName `
  -ResourceGroupName myResourceGroup `
  -SkuName Standard_LRS `
  -Kind StorageV2
```

Rufen Sie nach dem Erstellen des Speicherkontos mit [Get-AzureRmStorageAccountKey](/powershell/module/azurerm.storage/get-azurermstorageaccountkey) den Schlüssel für das Speicherkonto in eine Variable ab:

```azurepowershell-interactive
$storageAcctKey = (Get-AzureRmStorageAccountKey `
  -ResourceGroupName myResourceGroup `
  -AccountName $storageAcctName).Value[0]
```

Mit dem Schlüssel wird in einem späteren Schritt eine Dateifreigabe erstellt. Geben Sie `$storageAcctKey` ein, und notieren Sie den Wert. Diesen müssen Sie in einem späteren Schritt auch manuell eingeben, wenn Sie die Dateifreigabe einem Laufwerk in einem virtuellen Computer zuordnen.

### <a name="create-a-file-share-in-the-storage-account"></a>Erstellen einer Dateifreigabe im Speicherkonto

Erstellen Sie mit [New-AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext) einen Kontext für das Speicherkonto und den Speicherschlüssel. Der Kontext kapselt den Speicherkontonamen und den Kontoschlüssel:

```azurepowershell-interactive
$storageContext = New-AzureStorageContext $storageAcctName $storageAcctKey
```

Erstellen Sie mit [New-AzureStorageShare](/powershell/module/azure.storage/new-azurestorageshare) eine Dateifreigabe:

$share = New-AzureStorageShare my-file-share -Context $storageContext

### <a name="deny-all-network-access-to-a-storage-account"></a>Verweigern des gesamten Netzwerkzugriffs auf ein Speicherkonto

Standardmäßig akzeptieren Speicherkonten Netzwerkverbindungen von Clients in allen Netzwerken. Um den Zugriff auf ausgewählte Netzwerke einzuschränken, ändern Sie die Standardaktion mit [Update-AzureRmStorageAccountNetworkRuleSet](/powershell/module/azurerm.storage/update-azurermstorageaccountnetworkruleset) in *Deny* (Verweigern). Nachdem der Netzwerkzugriff verweigert wurde, kann auf das Speicherkonto aus keinem Netzwerk mehr zugegriffen werden.

```azurepowershell-interactive
Update-AzureRmStorageAccountNetworkRuleSet  `
  -ResourceGroupName "myresourcegroup" `
  -Name $storageAcctName `
  -DefaultAction Deny
```

### <a name="enable-network-access-from-a-subnet"></a>Aktivieren des Netzwerkzugriffs aus einem Subnetz

Rufen Sie mit [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork) das erstellte virtuelle Netzwerk und dann mit [Get-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig) das Subnetzobjekt „Private“ in eine Variable ab:

```azurepowershell-interactive
$privateSubnet = Get-AzureRmVirtualNetwork `
  -ResourceGroupName "myResourceGroup" `
  -Name "myVirtualNetwork" `
  | Get-AzureRmVirtualNetworkSubnetConfig `
  -Name "Private"
```

Erlauben Sie den Netzwerkzugriff auf das Speicherkonto aus dem Subnetz *Private* mit [Add-AzureRmStorageAccountNetworkRule](/powershell/module/azurerm.network/add-azurermnetworksecurityruleconfig).

```azurepowershell-interactive
Add-AzureRmStorageAccountNetworkRule `
  -ResourceGroupName "myresourcegroup" `
  -Name $storageAcctName `
  -VirtualNetworkResourceId $privateSubnet.Id
```

## <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

Zum Testen des Netzwerkzugriffs auf ein Speicherkonto stellen Sie einen virtuellen Computer für jedes Subnetz bereit.

### <a name="create-the-first-virtual-machine"></a>Erstellen des ersten virtuellen Computers

Erstellen Sie mit *New-AzureRmVM* einen virtuellen Computer im Subnetz [Öffentlich](/powershell/module/azurerm.compute/new-azurermvm). Wenn Sie den folgenden Befehl ausführen, werden Sie aufgefordert, Anmeldeinformationen einzugeben. Die eingegebenen Werte werden als Benutzername und Kennwort für den virtuellen Computer konfiguriert. Mit der Option `-AsJob` wird der virtuelle Computer im Hintergrund erstellt, sodass Sie mit dem nächsten Schritt fortfahren können.

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Location "East US" `
    -VirtualNetworkName "myVirtualNetwork" `
    -SubnetName "Public" `
    -Name "myVmPublic" `
    -AsJob
```

Die zurückgegebene Ausgabe ähnelt der folgenden Beispielausgabe:

```powershell
Id     Name            PSJobTypeName   State         HasMoreData     Location             Command                  
--     ----            -------------   -----         -----------     --------             -------                  
1      Long Running... AzureLongRun... Running       True            localhost            New-AzureRmVM     
```

### <a name="create-the-second-virtual-machine"></a>Erstellen des zweiten virtuellen Computers

Erstellen Sie einen virtuellen Computer im Subnetz *Private*:

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Location "East US" `
    -VirtualNetworkName "myVirtualNetwork" `
    -SubnetName "Private" `
    -Name "myVmPrivate"
```

Das Erstellen des virtuellen Computers für Azure dauert einige Minuten. Fahren Sie erst mit dem nächsten Schritt fort, nachdem der virtuelle Computer in Azure erstellt und eine Ausgabe in PowerShell zurückgegeben wurde. 

## <a name="confirm-access-to-storage-account"></a>Bestätigen des Zugriffs auf das Speicherkonto

Geben Sie mit [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) die öffentliche IP-Adresse eines virtuellen Computers zurück. Das folgende Beispiel gibt die öffentliche IP-Adresse der *myVmPrivate*-VM zurück:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmPrivate `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Ersetzen Sie im folgenden Befehl `<publicIpAddress>` durch die vom vorherigen Befehl zurückgegebene öffentliche IP-Adresse, und geben Sie dann den folgenden Befehl ein: 

```powershell
mstsc /v:<publicIpAddress>
```

Eine RDP-Datei (Remotedesktopprotokoll) wird erstellt und auf Ihren Computer heruntergeladen. Öffnen Sie die heruntergeladene RDP-Datei. Wenn Sie dazu aufgefordert werden, wählen Sie **Verbinden** aus. Geben Sie den Benutzernamen und das Kennwort ein, die Sie beim Erstellen des virtuellen Computers festgelegt haben. Unter Umständen müssen Sie auf **Weitere Optionen** und anschließend auf **Anderes Konto verwenden** klicken, um die Anmeldeinformationen anzugeben, die Sie beim Erstellen des virtuellen Computers eingegeben haben. Klicken Sie auf **OK**. Während des Anmeldevorgangs wird unter Umständen eine Zertifikatwarnung angezeigt. Wenn eine Warnung angezeigt wird, klicken Sie auf **Ja** bzw. **Weiter**, um mit dem Herstellen der Verbindung fortzufahren.

Ordnen Sie auf dem virtuellen Computer *myVmPrivate* mithilfe von PowerShell die Azure-Dateifreigabe dem Laufwerk Z zu. Ersetzen Sie vor dem Ausführen der nachfolgenden Befehle die Werte `<storage-account-key>` und `<storage-account-name>` durch Werte, die Sie unter [Erstellen eines Speicherkontos](#create-a-storage-account) angegeben oder abgerufen haben.

```powershell
$acctKey = ConvertTo-SecureString -String "<storage-account-key>" -AsPlainText -Force
$credential = New-Object System.Management.Automation.PSCredential -ArgumentList "Azure\<storage-account-name>", $acctKey
New-PSDrive -Name Z -PSProvider FileSystem -Root "\\<storage-account-name>.file.core.windows.net\my-file-share" -Credential $credential
```
PowerShell gibt eine Ausgabe ähnlich der folgenden Beispielausgabe zurück:

```powershell
Name           Used (GB)     Free (GB) Provider      Root
----           ---------     --------- --------      ----
Z                                      FileSystem    \\vnt.file.core.windows.net\my-f...
```

Die Azure-Dateifreigabe wurde dem Laufwerk Z erfolgreich zugeordnet.

Bestätigen Sie, dass der virtuelle Computer über keine ausgehende Verbindung mit einer beliebigen anderen öffentlichen IP-Adresse verfügt:

```powershell
ping bing.com
```

Sie erhalten keine Antworten, da die dem Subnetz *Private* zugeordnete Netzwerksicherheitsgruppe keinen ausgehenden Zugriff auf andere öffentliche IP-Adressen als auf die dem Azure Storage-Dienst zugewiesenen Adressen erlaubt.

Schließen Sie die Remotedesktopsitzung für den virtuellen Computer *myVmPrivate*.

## <a name="confirm-access-is-denied-to-storage-account"></a>Bestätigen, dass der Zugriff auf das Speicherkonto verweigert wird

Rufen Sie die öffentliche IP-Adresse des virtuellen Computers *myVmPublic* ab:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmPublic `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Ersetzen Sie im folgenden Befehl `<publicIpAddress>` durch die vom vorherigen Befehl zurückgegebene öffentliche IP-Adresse, und geben Sie dann den folgenden Befehl ein: 

```powershell
mstsc /v:<publicIpAddress>
```

Versuchen Sie, auf dem virtuellen Computer *myVmPublic* die Azure-Dateifreigabe dem Laufwerk Z zuzuordnen. Ersetzen Sie vor dem Ausführen der nachfolgenden Befehle die Werte `<storage-account-key>` und `<storage-account-name>` durch Werte, die Sie unter [Erstellen eines Speicherkontos](#create-a-storage-account) angegeben oder abgerufen haben.

```powershell
$acctKey = ConvertTo-SecureString -String "<storage-account-key>" -AsPlainText -Force
$credential = New-Object System.Management.Automation.PSCredential -ArgumentList "Azure\<storage-account-name>", $acctKey
New-PSDrive -Name Z -PSProvider FileSystem -Root "\\<storage-account-name>.file.core.windows.net\my-file-share" -Credential $credential
```

Der Zugriff auf die Freigabe wird verweigert, und die Fehlermeldung `New-PSDrive : Access is denied` wird angezeigt. Der Zugriff wird verweigert, da der virtuelle Computer *myVmPublic* im Subnetz *Public* bereitgestellt wird. Für das Subnetz *Public* ist kein Dienstendpunkt für Azure Storage aktiviert, und das Speicherkonto erlaubt nur den Netzwerkzugriff aus dem Subnetz *Private*, nicht aus dem Subnetz *Public*.

Schließen Sie die Remotedesktopsitzung für den virtuellen Computer *myVmPublic*.

Versuchen Sie von Ihrem Computer aus, die Dateifreigaben im Speicherkonto mit dem folgenden Befehl anzuzeigen:

```powershell-interactive
Get-AzureStorageFile `
  -ShareName my-file-share `
  -Context $storageContext
```

Der Zugriff wird verweigert, und die Fehlermeldung *Get-AzureStorageFile: The remote server returned an error: (403) Forbidden. HTTP Status Code: 403 - HTTP Error Message: This request is not authorized to perform this operation* (Get-AzureStorageFile: Der Remoteserver hat einen Fehler zurückgegeben: (403) Unzulässig. HTTP-Statuscode: 403 - HTTP-Fehlermeldung: Diese Anforderung ist nicht berechtigt, diesen Vorgang auszuführen) wird ausgegeben, da sich Ihr Computer nicht im Subnetz *Private* des virtuellen Netzwerks *MyVirtualNetwork* befindet.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Ressourcengruppe und alle darin enthaltenen Ressourcen nicht mehr benötigen, können Sie sie mit dem Befehl [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) entfernen:

```azurepowershell-interactive 
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie einen Dienstendpunkt für ein Subnetz eines virtuellen Netzwerks aktiviert. Sie haben erfahren, dass Dienstendpunkte für Ressourcen aktiviert werden können, die mit mehreren Azure-Diensten bereitgestellt werden. Sie haben ein Azure Storage-Konto erstellt und den Netzwerkzugriff auf das Speicherkonto ausschließlich auf Ressourcen im Subnetz eines virtuellen Netzwerks eingeschränkt. Bevor Sie Dienstendpunkte in virtuellen Produktionsnetzwerken erstellen, sollten Sie sich unbedingt gründlich mit [ Dienstendpunkten](virtual-network-service-endpoints-overview.md) vertraut machen.

Wenn Sie mehrere virtuelle Netzwerke in Ihrem Konto verwenden, können Sie zwei virtuelle Netzwerke miteinander verbinden, damit die Ressourcen in jedem virtuellen Netzwerk miteinander kommunizieren können. Im nächsten Tutorial erfahren Sie, wie virtuelle Netzwerke verbunden werden.

> [!div class="nextstepaction"]
> [Herstellen von Verbindungen zwischen virtuellen Netzwerken](./tutorial-connect-virtual-networks-powershell.md)
